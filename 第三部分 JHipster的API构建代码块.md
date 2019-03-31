# 第三部分 JHipster的API构建代码块

JHipster由两个主要组件组成：现代UI框架和API。 API是现代数据检索机制。创建优秀的用户界面就是让人们感到愉悦的方式。

今天的许多API都是RESTful API。事实上，表述性状态转移（REST）是万维网的软件架构风格。 RESTful系统通常使用动词（GET，POST，PUT，DELETE等）通过HTTP（超文本传输协议）进行通信。这与浏览器检索网页并将数据发送到远程服务器的方式相同。 REST最初由Roy Fielding在2000年获得博士学位时发表的论文-建筑风格和基于网络的软件体系结构设计。

JHipster利用Spring MVC及其```@RestController```注释创建REST API。它的端点向客户端发布JSON并使用JSON。通过将业务逻辑和数据持久性与客户端分离，您可以向许多不同的客户端提供数据（HTML5，iOS，Android，电视，手表，物联网设备等）。它还允许第三方和合作伙伴将其功能集成到您的应用中.Spring Boot通过简化微服务从而允许您创建独立的JAR（Java Archive）文件，并进一步补充了Spring MVC。

## Spring Boot

2013年8月，Pivotal的工程师Phil Webb和Dave Syer宣布推出Spring Boot的第一个里程碑版本。 Spring Boot可以轻松创建Spring应用程序。 它需要一个Spring视图，并为您自动配置依赖项。 这允许您编写更少的代码但仍然利用Spring的强大功能。 下图（来自https://spring.io）显示了Spring Boot如何成为更大的Spring生态系统的网关。

![图33.Spring Boot 2.0](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPveMGcr_DVP3jsn%2F33.jpg?alt=media&token=3792fa1b-cb57-4389-9ee7-beb1d2037332)

图33.Spring Boot 2.0

Spring Boot的主要目标是：

* 为所有Spring开发提供从根本上更快、更广泛访问的“入门”体验;
* 开箱即用，但随着要求开始偏离默认值，请尽快摆脱困境; 和
* 提供大类项目常见的一系列非功能性特性（例如，嵌入式服务器，安全性，指标，运行状况检查，外部化配置）。

想要在JHipster应用程序之外使用Spring Boot的人可以使用Spring Initializr来实现，Spring Initializr是一个用于生成Spring项目的可配置服务。 您可以在浏览器中访问https://start.spring.io，也可以通过```curl```调用它。

![图34.在浏览器上配置Spring Initializr](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPvhmI24I61zZZWS%2F34.jpg?alt=media&token=5b24b567-3e60-4653-adac-cc2c7076e07a)

图34.在浏览器上配置Spring Initializr

![图35.通过curl配置Spring Initializr](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPvlaBVGuoXQzZAx%2F35.jpg?alt=media&token=e690d1cb-ba8f-47c5-8c03-3d7b4c5de967)

图35.通过```curl```配置Spring Initializr

Spring Initializr是一个Apache 2.0许可的开源项目，您可以安装和自定义该项目，以便为您的公司或团队生成Spring项目。 您可以在GitHub上找到它，网址为https://github.com/spring-io/initializr。

Spring Initializr也可以在基于Eclipse的Spring Tool Suite（STS）和IntelliJ IDEA中使用。

> <center>Spring CLI</center>
> 你也可以下载和安装Spring Boot CLI。最简单的方法是通过SDKMAN安装它。
> ```
> curl -s “https://get.sdkman.io” | bash
> sdk install springboot
> ```
> Spring CLI最适合用于快速原型设计：当您想要向某人展示如何快速地执行某些操作时，您可能会在完成后丢弃代码。例如，如果要在Groovy中创建“Hello World”Web应用程序，则可以使用七行代码执行此操作。
>
> *hello.groovy*
>
> ```
> @RestController
> class WebApplication {
>     @RequestMapping("/")
>     String home() {
>         "Hello World!"
>     }
> }
> ```
>
> 要编译和运行此应用程序，只需键入：
> ```
> spring run hello.groovy
> ```
> 运行此命令后，您可以在http://localhost:8080上看到该应用程序。更多有关Spring Boot CLI的信息，请参阅其文档。

为了向您展示如何使用Spring Boot创建一个简单的应用程序，请转到https://start.spring.io并选择```Web```，```JPA```，```H2```和```Actuator```作为项目依赖项。单击“生成项目”以下载项目的.zip文件。 将其解压缩到硬盘上并将其导入您喜欢的IDE中。

这个项目只有几个文件，你可以通过运行```tree```命令（在* nix[类UNIX]上）看到。

```
.
├── mvnw
├── mvnw.cmd
├── pom.xml
└── src
    ├── main
    │   ├── java
    │   │   └── com
    │   │       └── example
    │   │           └── demo
    │   │               └── DemoApplication.java
    │   └── resources
    │       ├── application.properties
    │       ├── static
    │       └── templates
    └── test
        └── java
            └── com
                └── example
                    └── demo
                        └── DemoApplicationTests.java

14 directories, 6 files
```

```DemoApplication.java```是这个应用程序的核心; 文件和类名不相关。相关的是```@SpringBootApplication```注释和类的```public static void main```方法。

*src/main/java/com/example/demo/DemoApplication.java*

```
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

对于此应用程序，您将创建一个实体，一个JPA存储库和一个REST端点，以在浏览器中显示数据。 要创建实体，请将以下代码添加到```DemoApplication```类之外的```DemoApplication.java```文件中。

*src/main/java/demo/com/example/demo/DemoApplication.java*

```
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
...

@Entity
class Blog {

    @Id
    @GeneratedValue
    private Long id;
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Blog{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}
```

在同一文件中，添加扩展```JpaRepository```的```BlogRepository```接口。 Spring Data JPA使得为实体创建CRUD存储库变得非常容易。 它会自动为您创建与底层数据存储区通信的实现。

*src/main/java/com/example/demo/DemoApplication.java*

```
import org.springframework.data.jpa.repository.JpaRepository;

....
interface BlogRepository extends JpaRepository<Blog, Long> {}
```

定义一个```CommandLineRunner```，它注入此存储库并打印出通过调用其```findAll()```方法找到的所有数据。```CommandLineRunner```是一个接口，用于指示bean在```SpringApplication```中被包含时应如何运行。

*src/main/java/com/example/demo/DemoApplication.java*

```
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

...

@Component
class BlogCommandLineRunner implements CommandLineRunner {

    private BlogRepository repository;

    public BlogCommandLineRunner(BlogRepository repository) {
        this.repository = repository;
    }

    @Override
    public void run(String... strings) throws Exception {
        System.out.println(repository.findAll());
    }
}
```

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)Spring 4.3添加了隐式构造函数注入，无需```@Autowired```注解。

要提供默认数据，请创建```src/main/resources/data.sql```并添加SQL语句以插入数据。

*src/main/resources/data.sql*

```
insert into blog (name) values ('First');
insert into blog (name) values ('Second');
```

使用```mvn spring-boot:run```（或右键单击 →“在IDE中运行”）启动应用程序，您应该会在日志中看到此默认数据。

```
2017-08-31 23:09:27.436 INFO 67327 --- [main] s.b.c.e.t.TomcatEmbeddedServletContainer :
 Tomcat started on port(s): 8080 (http)
2017-08-31 23:09:27.470 INFO 67327 --- [main] o.h.h.i.QueryTranslatorFactoryInitiator :
 HHH000397: Using ASTQueryTranslatorFactory
[Blog{id=1, name='First'}, Blog{id=2, name='Second'}]
2017-08-31 23:09:27.549 INFO 67327 --- [main] com.example.demo.DemoApplication :
 Started DemoApplication in 3.924 seconds (JVM running for 4.492)
```

要将此数据发布为REST API，请创建一个```BlogController```类并添加一个返回博客列表的```/blogs```端点。

*src/main/java/demo/com/example/demo/DemoApplication.java*

```
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import java.util.Collection;

...

@RestController
class BlogController {
    private final BlogRepository repository;

    public BlogController(BlogRepository repository) {
        this.repository = repository;
    }

    @RequestMapping("/blogs")
    Collection<Blog> list() {
        return repository.findAll();
    }
}
```

添加此代码并重新启动应用程序后，您可以使用curl访问端点或在您喜欢的浏览器中打开它。

```
$ curl localhost:8080/blogs
[{"id":1,"name":"First"},{"id":2,"name":"Second"}]
```

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)[HTTPie](<https://httpie.org/>)是cURL的替代品，可以使许多事情变得更容易。

Spring拥有javaland最好的内行（hipness）追踪记录。 它是API是否可靠的重要基石，因它使JHipster令人敬畏。 Spring Boot允许您创建直接嵌入Tomcat，Jetty或Undertow的独立Spring应用程序。 无论您使用的是Maven还是Gradle，它都提供了自发的启动依赖关系，简化了您的构建配置。

### 外部配置

您可以在外部配置Spring Boot，这样您就可以在不同的环境中使用相同的应用程序代码。您可以使用属性文件，YAML文件，环境变量和命令行参数来外部化配置。

Spring Boot为```PropertySource```运行这个特定的序列，以确保它明智地覆盖值：

1. 在您的用户主目录上进行开发工具（Devtools）的全局属性设置（当devtools处于活动状态时，```~/.spring-boot-devtools.properties```）。
2. 在你的测试中的```@TestPropertySource```注释。
3. 在你的测试中的```@SpringBootTest#properties```注释属性。
4. 命令行参数，
5. 来自```SPRING_APPLICATION_JSON```的属性（嵌入在环境变量中的内联JSON或系统属性）。
6. ```ServletConfig init```参数。
7. ```ServletContext init```参数。
8. 来自```java:comp/env```的JNDI属性。
9. Java系统属性（```System.getProperties()```）。
10. OS（操作系统）环境变量。
11. 一个只有```random.*```属性的```RandomValuePropertySource```。
12. 打包JAR之外的特定于配置文件的应用程序属性（```application-{profile}.properties```和YAML变体）。
13. 在JAR（```application-{profile}.properties```和YAML变体）中打包的特定于配置文件的应用程序属性。
14. 打包JAR之外的应用程序属性（```application.properties```和YAML变体）。
15. 打包在JAR（```application.properties```和YAML变体）中的应用程序属性。
16. ```@Configuration```类上的```@PropertySource```注释。
17. 默认属性（使用```SpringApplication.setDefaultProperties```指定）。

### 应用属性文件

默认情况下，```SpringApplication```将从以下位置的```application.properties```文件加载属性，并将它们添加到Spring ```Environment```中：

1. 当前目录的```/config```子目录，
2. 当前目录，
3. 类路径下```/config```包，和
4. root类路径。

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)您还可以使用YAML（```.yml```）文件替代属性文件。 JHipster使用YAML文件进行配置。

有关Spring Boot外部配置功能的更多信息，请参阅Spring Boot的[“Externalized Configuration” reference documentation](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html)。

![警告](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx8wFVf0uxw998r%2Fwarning.png?alt=media&token=5e68c3d7-37db-4a98-822f-de2e3f6534ad)如果您使用的是需要外部配置文件的第三方库，则可能在加载它们时遇到问题。这些文件可能加载：```XXX.class.getResource().toURI().getPath()```使用Spring Boot的可执行JAR时，此代码不起作用，因为类路径是相对于JAR本身而不是文件系统。 一种解决方法是在servlet容器中将应用程序作为WAR运行。 您也可以尝试联系第三方库的维护者以找到解决方案。

### 自动配置

Spring Boot的独特之处在于它尽可能自动配置Spring。 它通过窥视JAR文件来查看它们是否是时髦(hip)的。如果是，则它们包含在```META-INF/spring.factories```，用于定义```EnableAutoConfiguration```键下的配置类。 例如，下面是```spring-boot-actuator-autoconfigure```中包含的内容。

*spring-boot-actuator-autoconfigure-2.0.5.RELEASE.jar!/META-INF/spring.factories*

```
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.actuate.autoconfigure.amqp.RabbitHealthIndicatorAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.audit.AuditAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.audit.AuditEventsEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.beans.BeansEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.cassandra.CassandraHealthIndicatorAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.cloudfoundry.servlet.CloudFoundryActuatorAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.cloudfoundry.reactive.ReactiveCloudFoundryActuatorAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.condition.ConditionsReportEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.context.properties.ConfigurationPropertiesReportEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.context.ShutdownEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.couchbase.CouchbaseHealthIndicatorAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.elasticsearch.ElasticsearchHealthIndicatorAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.endpoint.EndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.endpoint.jmx.JmxEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.endpoint.web.WebEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.env.EnvironmentEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.flyway.FlywayEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.health.HealthEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.health.HealthIndicatorAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.influx.InfluxDbHealthIndicatorAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.info.InfoContributorAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.info.InfoEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.jdbc.DataSourceHealthIndicatorAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.jms.JmsHealthIndicatorAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.jolokia.JolokiaEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.ldap.LdapHealthIndicatorAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.liquibase.LiquibaseEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.logging.LogFileWebEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.logging.LoggersEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.mail.MailHealthIndicatorAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.management.HeapDumpWebEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.management.ThreadDumpEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.CompositeMeterRegistryAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.MetricsAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.MetricsEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.amqp.RabbitMetricsAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.cache.CacheMetricsAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.export.atlas.AtlasMetricsExportAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.export.datadog.DatadogMetricsExportAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.export.ganglia.GangliaMetricsExportAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.export.graphite.GraphiteMetricsExportAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.export.influx.InfluxMetricsExportAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.export.jmx.JmxMetricsExportAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.export.newrelic.NewRelicMetricsExportAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.export.prometheus.PrometheusMetricsExportAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.export.simple.SimpleMetricsExportAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.export.signalfx.SignalFxMetricsExportAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.export.statsd.StatsdMetricsExportAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.export.wavefront.WavefrontMetricsExportAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.jdbc.DataSourcePoolMetricsAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.web.client.RestTemplateMetricsAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.web.reactive.WebFluxMetricsAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.web.servlet.WebMvcMetricsAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.metrics.web.tomcat.TomcatMetricsAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.mongo.MongoHealthIndicatorAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.neo4j.Neo4jHealthIndicatorAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.redis.RedisHealthIndicatorAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.scheduling.ScheduledTasksEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.session.SessionsEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.solr.SolrHealthIndicatorAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.system.DiskSpaceHealthIndicatorAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.trace.http.HttpTraceAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.trace.http.HttpTraceEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.web.mappings.MappingsEndpointAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.web.reactive.ReactiveManagementContextAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.web.server.ManagementContextAutoConfiguration,\
org.springframework.boot.actuate.autoconfigure.web.servlet.ServletManagementContextAutoConfiguration
org.springframework.boot.actuate.autoconfigure.web.ManagementContextConfiguration=\
org.springframework.boot.actuate.autoconfigure.endpoint.web.ServletEndpointManagementContextConfiguration,\
org.springframework.boot.actuate.autoconfigure.endpoint.web.reactive.WebFluxEndpointManagementContextConfiguration,\
org.springframework.boot.actuate.autoconfigure.endpoint.web.servlet.WebMvcEndpointManagementContextConfiguration,\
org.springframework.boot.actuate.autoconfigure.endpoint.web.jersey.JerseyWebEndpointManagementContextConfiguration,\
org.springframework.boot.actuate.autoconfigure.web.jersey.JerseyManagementChildContextConfiguration,\
org.springframework.boot.actuate.autoconfigure.web.reactive.ReactiveManagementChildContextConfiguration,\
org.springframework.boot.actuate.autoconfigure.web.servlet.ServletManagementChildContextConfiguration,\
org.springframework.boot.actuate.autoconfigure.web.servlet.WebMvcEndpointChildContextConfiguration

org.springframework.boot.diagnostics.FailureAnalyzer=\
org.springframework.boot.actuate.autoconfigure.metrics.MissingRequiredConfigurationFailureAnalyzer
```

这些配置类通常包含```@Conditional```注释以帮助自行配置。开发人员可以使用```@ConditionalOnMissingBean```来覆盖自动配置的默认值。 在开发Spring Boot插件时，可以使用几个与条件相关的注释：

* @ConditionalOnClass 和 @ConditionalOnMissingClass
* @ConditionalOnMissingClass 和 @ConditionalOnMissingBean
* @ConditionalOnProperty
* @ConditionalOnResource
* @ConditionalOnWebApplication 和 @ConditionalOnNotWebApplication
* @ConditionalOnExpression

这些注释使Spring Boot具有巨大的功能，并使其易于使用，配置和覆盖。

### 执行器

Spring Boot的Actuator子项目可以轻松地为您的应用程序添加几个生产级服务。 您可以通过添加```spring-boot-starter-actuator```依赖项将执行器添加到基于Maven的项目中。

```
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
</dependencies>
```

如果你使用Gradle，你须保存如下几行代码：

```
dependencies {
    compile("org.springframework.boot:spring-boot-starter-actuator")
}
```

执行器的主要功能是端点，指标，审计和过程监控。 执行器自动创建许多REST端点。 默认情况下，Spring Boot还会将管理端点公开为```org.springframework.boot```域下的JMX MBean。 执行器REST端点包括：

* ```/auditevents``` - 暴露当前应用程序的审计事件信息。
* ```/beans``` - 返回应用程序中所有Spring bean的完整列表。
* ```/conditions``` - 显示在配置和自动配置上评估的条件类。
* ```/configprops``` - 返回所有```@ConfigurationProperties```的列表。
* ```/env``` - 从Spring的```ConfigurableEnvironment```返回属性。
* ```/flyway``` - 显示已应用的任何Flyway数据库迁移。
* ```/health``` - 返回有关应用程序运行状况的信息。
* ```/httptrace``` - 返回跟踪信息（默认情况下，最后100个HTTP请求）。
* ```/info``` - 返回基本的应用程序信息。
* ```/loggers``` - 显示和修改应用程序中记录器的配置。
* ```/liquibase``` - 显示已应用的任何Liquibase数据库迁移。
* ```/metrics``` - 返回当前应用程序的性能信息。
* ```/mappings``` - 返回所有```@RequestMapping```路径的列表。
* ```/scheduledtasks``` - 显示应用程序中的计划任务。
* ```/sessions``` - 允许从Spring Session支持的会话中检索和删除用户会话存储。
* ```/shutdown``` - 正常关闭应用程序（默认情况下不启用）。
* ```/threaddump``` - 执行线程转储。

JHipster默认包含大量的Spring Boot入门依赖项。 这允许开发人员编写更少的代码，而不必担心依赖关系和配置。 21-Points Health应用程序中的启动器(boot-starter)依赖关系如下：

```
spring-boot-starter-cache
spring-boot-starter-mail
spring-boot-starter-logging
spring-boot-starter-actuator
spring-boot-starter-aop
spring-boot-starter-data-jpa
spring-boot-starter-data-elasticsearch
spring-boot-starter-data-jest
spring-boot-starter-security
spring-boot-starter-web
spring-boot-starter-undertow
spring-boot-starter-thymeleaf
spring-boot-starter-test
```

Spring Boot在自动配置库和简化Spring方面做得很好。 JHipster通过将Spring Boot的精彩世界与现代UI和开发人员体验相结合来补充这一点。

## Spring WebFlux

Spring Boot 2.0还支持通过Spring WebFlux构建具有反应堆栈的应用程序。当使用WebFlux（而不是Web）时，您的应用程序将基于Reactive Streams API并在非阻塞服务器（如Netty，Undertow和Servlet 3.1+容器）上运行。

在撰写本文时，JHipster已经为使用WebFlux生成微服务应用程序提供了实验性支持。 有关更多信息，请参阅pull request＃7983。

展示Spring WebFlux如何工作超出了这本迷你书的范围。 如果您想了解更多信息，我建议您阅读Josh Long和我的Build Reactive APIs with Spring WebFlux博客文章。

## Maven 对 Gradle

Maven和Gradle是当今Java项目中使用的两个主要构建工具。 JHipster允许您使用其中任何一个。使用Maven，您有一个1090行XML的```pom.xml```文件。使用Gradle，您最终得到了几个```*.gradle```文件。在21-Points项目中，Groovy代码总共只有496行。

Apache将Apache Maven称为“软件项目管理和理解工具”。基于项目对象模型（POM）的概念，Maven可以从中心信息管理项目中构建，报告和文档。 Maven的大多数功能都来自插件。 Maven插件用于构建，测试，源代码控制管理，运行Web服务器，生成IDE项目文件等等.

Gradle是一个通用的构建工具。它可以构建你想要实现的任何东西构建脚本。但是，除非您在构建脚本中添加代码以提出要求，否则它不会构建任何内容。 Gradle具有基于Groovy的特定于域的语言（DSL），而不是更传统的XML形式来声明项目配置。与Maven一样，Gradle具有允许您为项目配置任务的插件。大多数插件都添加了一些预配置的任务，这些任务一起做了一些有用的事情。例如，Gradle的Java插件向您的项目添加任务，这些任务将编译和单元测试您的Java源代码，并将其捆绑到JAR文件中。

2014年1月，ZeroTurnaround的RebelLabs发布了一份名为Java Build Tools的报告 - 第2部分：决策者对Maven，Gradle和Ant + Ivy的比较，它提供了从1977年到2013年构建工具的时间表。

![图36. 构建工具的演变 1977-2013](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPvoLkX88VPcxO1a%2F36.jpg?alt=media&token=c58921f1-4d82-4097-8e74-d3ee4ffe6854)

图36. 构建工具的演变 1977-2013

那时候，RebelLabs建议你在下一个项目中试验Gradle。

> 引用：如果我们被迫以任何一般性建议结束，那么如果您要开始一个新项目，那将是Gradle。
>
> —— RebelLabs，Java构建工具 - 第2部分：决策者对Maven，Gradle和Ant + Ivy的比较

我已经使用这两种工具来构建项目，并且它们都运行良好。 Maven为我工作，但我已经使用它超过10年，并认识到我的历史和经验可能会导致我对它有偏见。 如果你更喜欢Gradle只是因为你试图避免使用XML，那么Polyglot for Maven可能会改变你的观点。 它支持Atom，Groovy，Clojure，Ruby，Scala和YAML语言。 具有讽刺意味的是，您需要包含一个XML文件才能使用它。 要添加对非XML语言的支持，须创建一个```${project}/.mvn/extensions.xml```文件并向其中添加以下XML。

```
<?xml version="1.0" encoding="UTF-8"?>
<extensions>
    <extension>
        <groupId>io.takari.polyglot</groupId>
        <artifactId>${artifactId}</artifactId>
        <version>0.3.0</version>
    </extension>
</extensions>
```

在此示例中，```${artifactId}```应为```polyglot-language```，其中```language```是上述语言之一。

要将现有```pom.xml```文件转换为其他格式，可以使用以下命令。

```
mvn io.takari.polyglot:polyglot-translate-plugin:translate \
  -Dinput=pom.xml -Doutput=pom.{format}
```

支持的格式是```rb```，```groovy```，```scala```，```yaml```，```atom```，当然还有```xml```。 您甚至可以转换回XML或在所有支持的格式之间进行交叉转换。 要了解有关Maven的替代语言的更多信息，请参阅GitHub上的Polyglot for Maven。

许多Internet资源都支持使用Gradle。 有Gradle自己的Gradle vs Maven功能比较。 Gradle的首席工程师Benjamin Muschko写了一篇Dobb博士的文章，题为“为什么用Gradle而不是Ant或Maven构建你的Java项目？”他也是Gradle in Action的作者。

Gradle是Android开发的默认构建工具。 Android Studio使用Gradle包装器完全集成Gradle的Android插件。

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)Maven和Gradle都提供了包装器，允许您将构建工具嵌入到项目和源代码控制系统中。 这允许开发人员在仅安装Java之后构建或运行项目。由于嵌入了构建工具，因此可以键入```gradlew```或```mvnw```以使用嵌入式构建工具。

无论您喜欢哪种，Spring Boot都支持Maven和Gradle。 您可以访问各自的文档页面了解更多信息：

* Spring Boot Maven插件
* Spring Boot Gradle插件

我建议您从最熟悉的工具开始。 如果你是第一次使用JHipster，你将需要限制你必须处理的新技术的数量。 您可以随时为下一个应用程序添加一些。 JHipster是一个很好的学习工具，您还可以使用不同的构建工具生成项目，以查看它。

## IDE支持：运行，调试，和剖析

IDE代表“集成开发环境”。键盘快捷键和快速键入是程序员的生命线。好的IDE具有代码自动补齐功能，允许您键入一些关键代码字符，按Tab键，并为您自动补齐剩余代码。此外，它们还提供快速格式化，轻松访问文档和代码调试。您可以使用IDE在静态类型语言（如Java）中生成大量代码，例如POJO上的getter和setter以及接口和类中的方法。您还可以轻松找到方法的参考文档。

JHipster文档包括配置Eclipse，IntelliJ IDEA，Visual Studio Code和NetBeans的指南。不仅如此，Spring Boot还有一个devtools插件，默认情况下在生成的JHipster应用程序中配置。此插件允许在重新编译类时热重载应用程序。

IntelliJ IDEA为Java开发带来了这些相同的功能，是一个真正令人惊叹的IDE。如果您只编写JavaScript，他们的WebStorm IDE很可能成为您最好的朋友。两种IntelliJ产品都具有出色的CSS支持，并且可以接受许多Web语言/框架的插件。要想在保存时进行IDEA自动编译，请执行以下步骤：

* 导航到文件>设置>构建，执行，部署 > 编译器：启用自动生成项目（```Make project automatically```）
* 打开注册表（Mac:```Cmd+Shift+A```，Linux:```Ctrl+Shift+A```，选择注册表...）并启用```compiler.automake.allow.when.app.running```

Eclipse是IntelliJ IDEA的免费替代品。它的错误高亮提示（通过自动编译），代码辅助和重构支持非常出色。当我在2002年开始使用它时，它已经击败其竞争对手。它是第一个使用起来给人以快速高效之感的Java IDE。遗憾的是，它在JavaScript MVC时代落后，缺乏对JavaScript或CSS的良好支持。

NetBeans有一个Spring Boot插件。 NetBeans团队在Web工具支持方面做了大量工作;他们拥有良好的JavaScript / AngularJS支持，还有适用于Chrome的NetBeans Connector插件，允许在NetBeans和Chrome中进行双向编辑。

Visual Studio Code是Microsoft制作的开源文本编辑器。它已成为TypeScript的流行编辑器，并具有用于Java开发的插件。

Spring Boot的优点是您可以将其作为一个简单的Java进程运行。这意味着您可以右键单击```*Application.java```类并从IDE运行（或调试它）。调试时，您将能够在Java类中设置断点，并查看在进程执行之前要设置的变量。

要了解有关分析Java应用程序的信息，我建议您观看Nitsan Wakart的““Java Profiling from the Ground Up!”要了解有关内存和JavaScript应用程序的更多信息，我推荐Addy Osmani的“JavaScript Memory Management Masterclass”。

## 安全

Spring Boot与Spring Security集成，具有出色的安全功能。 当您使用```spring-boot-starter-security```依赖关系创建Spring Boot应用程序时，您将获得开箱即用的HTTP Basic身份验证。 默认情况下，使用用户名```user```创建用户，并在应用程序启动时在日志中打印密码。 要覆盖生成的密码，可以定义```spring.security.user.password```。可以在Spring Boot的安全性指南中找到Spring Boot的其他安全功能。

最基本的Spring Security Java配置创建了一个servlet ```Filter```，它负责所有安全性（保护URL，验证凭据，重定向到登录等）。 这涉及几行代码，但其中一半是类导入。

```
import org.springframework.context.annotation.Bean;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;

import static org.springframework.security.core.userdetails.User.UserBuilder;

@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Bean
    public UserDetailsService userDetailsService() {
        // 确保密码编码正确
        UserBuilder users = User.withDefaultPasswordEncoder();
        InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager();
        manager.createUser(users.username("user").password("password").roles("USER").build());
        return manager;
    }
}
```

代码不多，但它提供了许多功能：

* 它需要对应用程序中的每个URL进行身份验证。
* 它为您生成登录表单。
* 它允许用户：密码通过基于表单的身份验证进行身份验证。
* 它允许用户注销。
* 它可以防止CSRF攻击。
* 它可以防止会话固定。
* 它包括与以下内容的安全标头集成：
  * 安全请求的HTTP严格传输安全性，
  * X-Content-Type-Options集成，
  * 缓存控制，
  * X-XSS-Protection集成，以及
  * X-Frame-Options集成有助于防止点击劫持
* 它与HttpServletRequest API方法集成：```getRemoteUser()```, ```getUserPrinciple()```, ```isUserInRole(role)```, ```login(username, password)```, 和```logout()```。

JHipster采用了Spring Security的卓越性，并使用它来提供应用程序所需的真实身份验证机制。当您创建新的JHipster项目时，它为您提供了三个身份验证选项：

* JWT身份验证 - 无状态安全机制。 JSON Web令牌（JWT）是IETF提出的标准，它使用紧凑的，URL安全的方式来表示在双方之间转移的声明。 JHipster的实现使用Java JWT项目。
* HTTP会话身份验证 - 使用HTTP会话，因此它是一种有状态机制。推荐用于小型应用程序。
* OAuth 2.0 / OIDC身份验证 - 一种有状态的安全机制，如HTTP Session。 如果您想在多个应用程序之间共享用户，则可能更喜欢它。

> <center>OAuth 2.0</center>
>
> OAuth 2.0是OAuth框架的当前版本（最初创建于2006年）。 OAuth 2.0主要用于简化客户端开发，同时支持Web应用程序和桌面应用程序，移动电话和起居室设备。 如果您想了解OAuth的工作原理，请参阅What the Heck is OAuth?

除了身份验证选择之外，JHipster还提供了安全性改进：改进了“记住我”（存储在数据库中的独特令牌），cookie-theft protection和CSRF保护。

默认情况下，JHipster有四个不同的用户：

* system - 当事情自动完成时由审计日志使用。
* anonymousUser - 匿名用户执行操作时。
* user - 具有“ROLE_USER”授权的普通用户; 默认密码为“user”。
* admin - 具有“ROLE_USER”和“ROLE_ADMIN”授权的管理员用户; 默认密码为“admin”。

出于安全原因，您应该到```src/main/resources/config/liquibase/users.csv```更改默认密码或部署时通过用户管理功能修改它。

## JPA对MongoDB对Cassandra

传统的关系数据库管理系统（RDBMS）提供了许多属性，可以保证其事务的可靠处理：ACID，具有原子性，一致性，隔离性和持久性。像MySQL和PostgreSQL这样的数据库提供了RDBMS支持并且通过不懈努力以减少数据库的成本。 JHipster还支持Oracle和Microsoft等供应商。 如果您想使用传统数据库，请在创建JHipster项目时选择SQL。

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)JHipster的使用Oracle指南解释了如何使用Oracle帐户下载其专有的JDBC驱动程序。

NoSQL数据库帮助许多互联网公司通过最终的一致性实现了高可扩展性：因为NoSQL数据库通常分布在多台机器上，并且有一些延迟，它只保证所有实例最终都是一致的。 与传统的ACID属性相比，最终一致的服务通常称为BASE（基本可用，软状态，最终一致性）服务。

创建新的JHipster项目时，系统会提示您以下内容。

```
? Which *type* of database would you like to use? (Use arrow keys)
  SQL (H2, MySQL, MariaDB, PostgreSQL, Oracle, MSSQL)
 MongoDB
 Couchbase
 Cassandr
```

如果您熟悉RDBMS数据库，我建议您使用PostgreSQL或MySQL进行开发和生产。 PostgreSQL对Heroku有很大的支持，MySQL在AWS上有很好的支持。 JHipster的AWS子生成器仅限于使用MySQL。

如果你的想法是下一个Facebook，你可能想要考虑一个比第三范式更关注性能的NoSQL数据库。

> NoSQL包含各种不同的数据库技术，这些技术是为了响应有关用户的存储，对象和产品的数据量的增加，访问此数据的频率以及性能和处理需求而开发的。 另一方面，关系数据库并非旨在应对面向现代应用程序的规模和敏捷性挑战，也不是为了利用当今可用的廉价存储和处理能力而构建的。
>
> —— MongoDB，NOSQL数据库解释

MongoDB由成立于2007年的DoubleClick，ShopWiki和Gilt Groupe开发。 它在GitHub上使用Apache和GNU-APGL许可证。 它的许多大客户包括Adobe，eBay和eHarmony。

Cassandra是“用于管理结构化数据的分布式存储系统，旨在跨越许多商用服务器扩展到非常大的尺寸，没有单点故障”（来自Facebook上的“Cassandra – A structured storage system on a P2P Network”） 工程博客）。 最初是在Facebook开发，以支持其收件箱搜索功能。 其创建者Avinash Lakshman（亚马逊DynamoDB的创建者之一）和Prashant Malik于2008年7月将其作为一个开源项目发布。2009年3月，它成为了一个Apache孵化器项目，并2010年2月成为其顶级项目之一。

除了Facebook之外，Cassandra还帮助其他许多公司扩展其网络规模。 它在主页上有关于可扩展性的一些数字令人印象深刻。

> 最大的生产部署之一是Apple，其中超过75,000个节点存储超过10 PB的数据。 其他大型Cassandra安装包括Netflix（2,500个节点，420 TB，每天超过1万亿个请求），中文搜索引擎宜搜（270个节点，300 TB，每天超过8亿个请求）和eBay（超过100个节点，250 TB）。
>
> —— Cassandra，[项目主页](http://cassandra.apache.org/)

JHipster的数据支持让您梦想成真！

> <center>使用JHipster的NoSQL</center>
>
> 选择MongoDB时：
>
> * JHipster将使用Spring Data MongoDB，类似于Spring Data JPA。
> * JHipster将使用Mongobee而不是Liquibase来管理数据库迁移。
> * 实体子生成器不会询问您关系。 您不能与NoSQL数据库建立关系（relationships）。
> * ```de.flapdoodle.embed.mongo```用于运行内存版本的数据库以运行单元测试。

## Liquibase

Liquibase是一款“基于源代码控制的数据库”。 它是一个基于（Apache 2.0）开源项目，它允许您在构建或运行时进程中操作数据库。 它允许您根据数据库表区分实体并创建迁移脚本。 它甚至允许您提供以逗号分隔的默认数据！ 例如，默认用户从```src/main/resources/config/liquibase/users.csv```加载。

Liquibase在创建数据库模式时会加载此文件。

*src/main/resources/config/liquibase/changelog/00000000000000_initial_schema.xml*

```
<loadData encoding="UTF-8"
        file="config/liquibase/users.csv"
        separator=";"
        tableName="jhi_user">
    <column name="activated" type="boolean"/>
    <column name="created_date" type="timestamp"/>
</loadData>
<dropDefaultValue tableName="jhi_user" columnName="created_date" columnDataType="datetime"/>
```

Liquibase支持大多数主要数据库。 如果使用MySQL或PostgreSQL，则可以使用```mvn liquibase:diff```（或```./gradlew generateChangeLog```）自动生成更改日志。

JHipster的开发指南推荐以下工作流程：

1. 修改JPA实体（添加字段，关系等）。
2. 运行```mvn compile liquibase:diff```。
3. 在```src/main/resources/config/liquibase/changelog```目录中创建一个新的更改日志。
4. 查看此更改日志并将其添加到```src/main/resources/config/liquibase/master.xml```文件中，以便下次运行应用程序时应用它。

如果使用Gradle，则可以通过运行```./gradlew generateChangeLog```来使用相同的工作流程。

## Elasticsearch

Elasticsearch为您的实体添加了可搜索性。JHipster的Elasticsearch支持需要使用SQL的数据库。Spring Boot使用和配置Spring Data Elasticsearch。使用JHipster的实体子生成器时，它会自动为实体编制索引并创建端点以支持搜索其属性。搜索超级用户也会添加到Angular UI中，因此您可以在实体的列表屏幕中进行搜索。

使用（默认）“dev”配置文件时，内存中的Elasticsearch实例将文件存储在```build/elasticsearch```文件夹中。您可以通过修改```applicationdev.yml```中的以下设置来更改此设置。

*src/main/resources/config/application-dev.yml*

```
data:
    elasticsearch:
        properties:
            path:
                home: build/elasticsearch
```

当使用“prod”配置文件时，JHipster将使用Spring Data Jest在端口```9200```上与Elasticsearch的REST API进行通信。此设置在```application-prod.yml```中配置。

*src/main/resources/config/application-prod.yml*

```
data:
    jest:
        uri: http://localhost:9200
```

如果要在本地运行“prod”配置文件，则需要首先启动Elasticsearch Docker映像。

```
docker-compose -f src/main/docker/elasticsearch.yml up -d
```

Elasticsearch被许多知名公司使用：Facebook，GitHub和优步等。 该项目得到了Elastic的支持，它提供了围绕Elasticsearch的项目生态系统。一些例子是：

* Elasticsearch即服务 - “托管和管理Elasticsearch”。
* Logstash - “处理来自任何来源的任何数据”。
* Kibana - “探索和可视化您的数据”。

ELK（Elasticsearch，Logstash和Kibana）堆栈全是由Elastic赞助的开源项目。它是监控应用程序和查看应用程序使用方式的强大解决方案。

## 部署

JHipster应用程序可以运行在任何能运行Java程序的地方部署。 Spring Boot使用```public static void main```作为您启动嵌入式Web服务器的入口。 Spring Boot应用程序嵌入在“fat JAR”中，其中包括所有必需的依赖项，例如Web服务器及其启动/停止脚本。 你可以给任何人这个```.jar```，他们可以轻松运行你的应用程序：不需要构建工具，无需设置、Web服务器配置等。仅须运行```java -jar killerapp.jar```。

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)Josh Long的“Deploying Spring Boot Applications”是学习如何自定义你的应用程序存档的绝佳资源。 它显示了如何将应用程序更改为传统WAR：扩展```SpringBootServletInitializer```，将其打包更改为```war```，并将```spring-boot-starter-tomcat```设置为提供的依赖项。

要使用生产配置文件构建JHipster应用程序，请使用预配置的“prod”Maven配置文件。

```
mvn -Pprod package
```

如果是Gradle，则使用：

```
gradlew -Pprod bootWar
```

“prod”配置文件将触发```webpack:prod```，它可以优化您的静态资源。 它将结合您的JavaScript和CSS文件，缩小它们，并让它们准备好生产环境。 它还会更新您的HTML（在您的```(build|target)```/www```目录中）以引用您的经过版本化，组合和缩小后的文件。

JHipster应用程序可以部署到您自己的JVM，Cloud Foundry，Heroku，Kubernetes和AWS。

我已经使用Kubernetes将JHipster应用程序部署到Heroku，Cloud Foundry和Google Cloud。

## 总结

Spring Framework是Javaland中最好的前沿记录之一。 它在许多版本之间保持向后兼容，并且作为一个开源项目已经存在超过14年。 Spring Boot为使用Spring的人员带来了一片新鲜空气，包括入门依赖项，自动配置和监控工具。 这使得在JVM上构建微服务并将它们部署到云中变得很容易。

您已经看到了Spring Boot的一些很酷的功能以及可用于打包和运行JHipster应用程序的构建工具。我已经描述了Spring Security的强大功能并向您展示了它的许多功能，只需几行代码即可实现。 JHipster支持关系数据库和NoSQL数据库，它允许您选择存储数据的方式。您可以在创建新应用程序时选择JPA，MongoDB或Cassandra。

Liquibase将为您创建数据库模式，并在需要时帮助您更新数据库。它提供了一个易于使用的工作流程，可以使用其diff功能向JHipster生成的实体添加新属性。

您可以使用Elasticsearch为您的JHipster应用添加丰富的搜索功能。这是GitHub上最受欢迎的Java项目之一，这是有原因的：它运行得非常好。

JHipster应用程序是Spring Boot应用程序，因此您可以在可以运行Java的任何地方部署它们。您可以将它们部署在传统的Java EE（或servlet）容器中，也可以将它们部署在云中。天空无限宽广（The sky’s the limit）！