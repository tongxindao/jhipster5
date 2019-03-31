# 第一部分 用JHipster构建应用

当我开始编写此书时，我对示例应用程序有一些不同的想法。我的第一个想法是创建一个照片库来展示我自2006年以来一直在开的1966年大众巴士。项目最近完成了，同时我想要一个网站来展示它的进展情况。

我还想过创建一个博客应用程序。作为我首次关于JHipster的演讲的一部分(在Denver Java用户组），我创建了一个我在观众面前实时编码的博客应用程序。在那之后，我花了数小时来完善应用程序并启动了[The JHipster Mini-Book站点](http://www.jhipster-book.com/#!/)。

在考虑了大众汽车图库和博客应用程序开发过程后，我心想，它时髦吗？是否配得上JHipster官网首页上“时髦”一词并展示如何构建这样一款应用程序？

我写信给JHipster的创始人Julien Dubois和本书的技术编辑Dennis Sharpe询问他们的想法。我们在一些想法上来回碰撞：一个Gitter克隆，一个JHipster编码器工作板，购物车应用程序。然后它打了我：一个我一直想开发的项目的想法。

它基本上是一个可用于监控您的健康状况的应用程序。2014年9月下旬至10月中旬，我做了糖排毒，并在此期间停止吃糖，同时开始定期锻炼，停止喝酒。十多年来我一直患有高血压，并按时用药。在排毒的第一周，我用完了血压药。由于新的处方需要医生过问，我决定等到排毒后才能得到它。后三个星期，我不仅减掉了15磅，而且我的血压处于正常水平！

在我开始排毒之前，我想出了一个21分系统，看看我每周健康状况。它的规则很简单：由于以下原因，您每天最多可以获得3分：

1. 如果您饮食健康，将得到一分。否则分数为零。

2. 如果您锻炼，将得到一分。

3. 如果您不饮酒，将得到一分。

我惊讶地发现在使用这个系统的第一周我得到了8分。在排毒期间，第一周我得到了16分，第二周得20分，第三周得21分。在排毒之前，我认为饮食健康意味着，吃除快餐以外的任何东西。排毒后，我意识到健康饮食意味着不吃糖。我也是精酿啤酒的狂热爱好者，所以我修改了酒精规则以允许两种更健康的酒精饮料（如灰狗或红酒）。

我的目标是每周得15分。我发现，如果我得到更多，我的血压与体重将得到控制。如果我所获少于15分，我就有可能生病。从那以后我一直在跟踪我的健康状况。2014年9月，我体重减轻30磅（约27.2斤），血压恢复正常水平。自20岁起我的血压值就不太正常，所以这对我来说是改变生活的方式。

我认为编写“21分健康”应用程序会起作用，因为它始终跟踪您的健康状况。可追踪您健康状况的可穿戴设备可使用我创建的API或挂钩记录一天的积分。想象一下连接到dailymile（我跟踪我的锻炼）或Untappd（其中我有时会列出我的啤酒量）。或甚至展示一天的其他活动（例如展示您在iOS Health上保留的血压评分）。

从监控的角度来看，我认为我的想法很适合JHipster和Spring Boot。Spring Boot有许多适用于应用程序的运行状况监视器，现在您可以使用此JHipster应用程序来监视您的健康状况！

## 创建应用

我开始使用Installing
JHipster说明文档。我是Java开发者，所以我已完成Java 8，以及Maven和Git安装。我使用Nodejs.org和Yarn的说明文档安装Node.js，然后运行以下命令来安装JHipster。

```npm install -g generator-jhipster```

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)如果您需要安装Java，Maven，或Git，请看JHipster的本地安装文档。

然后我继续构建我的应用程序。与Javaland中的许多应用程序生成器不同，Yeoman希望您进入要创建项目的目录，而不是为您创建目录。所以我创建了21-points目录，并在其中输入以下命令来调用JHipster

```jhipster```

运行此命令后，程序会询问您一些问题以确定如何生成应用。您可以在以下屏幕截图中看到我的选择。

![图1.生成应用程序](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPtgk4AKbCjTKYWG%2F1.jpg?alt=media&token=10b97ace-c100-4375-9ca2-456db2ee8533)
图1.生成应用程序

![备注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)我尝试使用“21-poypeScript类名称出现问题。使用数字启动类名是非法的，就像在Java中一样。

此过程会生成一个```.yo-rc.json```文件，该文件会捕获您所做的所有选择。您可以使用此文件在空目录中创建具有相同设置的项目。

*yo-rc.json*

```
{
    "generator-jhipster":{
        "promptValues":{
            "packageName":"org.jhipster.health",
            "nativeLanguage":"en"
        },
        "jhipsterVersion":"5.2.1",
        "applicationType":"monolith",
        "baseName":"TwentyOnePoints",
        "packageName":"org.jhipster.health",
        "packageFolder":"org/jhipster/health",
        "serverPort":"8080",
        "authenticationType":"jwt",
        "cacheProvider":"ehcache",
        "enableHibernateCache":true,
        "websocket":false,
        "databaseType":"sql",
        "devDatabaseType":"h2Disk",
        "prodDatabaseType":"postgresql",
        "searchEngine":"elasticsearch",
        "messageBroker":false,
        "serviceDiscoveryType":false,
        "buildTool":"gradle",
        "enableSwaggerCodegen":false,
        "jwtSecretKey":"43d99bb3ca6ffaa96b33b632e4a82fb45b672d80",
        "clientFramework":"angularX",
        "useSass":true,
        "clientPackageManager":"yarn",
        "testFrameworks":[
            "gatling",
            "protractor"
        ],
        "jhiPrefix":"jhi",
        "enableTranslation":true,
        "nativeLanguage":"en",
        "languages":[
            "en",
            "fr"
        ]
    }
}
```

您可以看到我选择了基于磁盘的持久性H2用于开发，同时由PostgreSQL用于我的生产数据库。我这样做是因为它为使用非嵌入式数据库提供了一些重要的功能：

* 重新启动应用程序时会保留您的数据。

* 您的应用程序启动速度更快。

* 您可以使用Liquibase生成数据库更改日志

[Liquibase](http://www.liquibase.org/)官网主页将其描述为通过源代码控制您的数据库。当您将它们添加到您的实体时它将有助于创造新的字段。它还将重构您的数据库，例如创建表和删除列。它还可以使用自定义SQL自动或撤消对数据库的更改。

在回答完所有问题后，JHipster会创建一大堆文件，然后运行```yarn install```。为了证明一切都很能正常运行，我使用```./gradlew test```运行Java单元测试。

```
BUILD SUCCESSFUL in 1m 7s
10 actionable tasks: 9 executed, 1 up-to-date
```

JHipster v5只能与外部Elasticsearch实例一起使用。在以前的版本中，您可以使用嵌入式Elasticsearch实例，但Elasticsearch在最近的版本中删除了此功能。运行本地Elasticsearch实例的最简单方法是使用Docker Compose。我运行以下命令将Elasticsearch作为守护进程启动。如果您不希望它作为守护程序运行，请删除```-d```选项。

```
docker-compose -f src/main/docker/elasticsearch.yml up -d
```

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)在JHipster 5.3.0+中再次将Elasticsearch作为嵌入式实例运行感谢est。

接下来，我使用```./gradlew```启动应用程序，然后使用```yarn e2e```运行UI集成测试。所有测试都出色的通过了。

```
$ yarn e2e
yarn run v1.9.4
$ protractor src/test/javascript/protractor.conf.js
(node:30302) [DEP0022] DeprecationWarning: os.tmpDir() is deprecated. Use os.tmpdir()
instead.
[14:45:36] W/configParser - pattern ./e2e/entities/**/*.spec.ts did not match any files. 
[14:45:36] I/launcher - Running 1 instances of WebDriver
[14:45:36] I/direct - Using ChromeDriver directly...
Started
...........

11 specs, 0 failures
Finished in 21.309 seconds
[14:45:59] I/launcher - 0 instance(s) of WebDriver still running
[14:45:59] I/launcher - chrome #01 passed
 Done in 23.25s
```

为验证```prod```（生产环境）配置文件有效，应用须与PostgreSQL通信，运行Docker Compose启动PostgreSQL。

```docker-compose -f src/main/docker/postgresql.yml up -d```

然后我使用prod配置文件重新启动应用程序。


``` $ ./gradlew -Pprod ```

```
\----------------------------------------------------------  

 Application 'TwentyOnePoints' is running! Access URLs:  

 Local: [http://localhost:8080](http://localhost:8080/)  

 External: [http://192.168.105.207:8080](http://192.168.105.207:8080/)  

 Profile(s): [prod]  

\---------------------------------------------------------

```

呕吼 — 好使！

> <center>使用本地PostgreSQL数据库</center>
>
> 您还可以使用本地PostgreSQL数据库。要在Mac上执行此操作，我通过```src/main/resources/config/application-prod.yml```安装了Postgres.app并尝试使用以下设置创建本地PostgreSQL数据库。
>
> ```
> psql (9.6.10)
> Type "help" for help.
> 
> template1=# create user TwentyOnePoints with password '21points';
> CREATE ROLE
> template1=# create database TwentyOnePoints;
> CREATE DATABASE
> template1=# grant all privileges on database TwentyOnePoints to TwentyOnePoints;
> GRANT
> template1=#
> ```
>
> 我更新了```application-prod.yml```，使用```21points```作为数据源密码。 我确认在使用```prod```配置文件运行时我可以与PostgreSQL数据库通信。我被遇到未正确设置错误。
>
> ```
> $ ./gradlew -Pprod
> ...
> 2018-08-27 09:59:32.094 ERROR 5180 --- [ main] com.zaxxer.hikari.pool.HikariPool :
> HikariPool-1 - Exception during pool initialization.
> 
> org.postgresql.util.PSQLException: FATAL: role "TwentyOnePoints" does not exist
>     at org.postgresql.core.v3.QueryExecutorImpl.receiveErrorResponse(QueryExecutorImpl.java:2440)
>     at org.postgresql.core.v3.QueryExecutorImpl.readStartupMessages(QueryExecutorImpl.java:2559)
>     at org.postgresql.core.v3.QueryExecutorImpl.(QueryExecutorImpl.java:133)
> ```
>
>   我很快意识到PostgreSQL不区分大小写，所以即使我输入“TwentyOnePoints”，它也会将数据库名称和用户名配置为“twentyonepoints”。我用正确的配置更新了```applicationprod.yml```并再次尝试。这次它奏效了！

### 添加源码控制

在创建新项目时，我首先要做的事情之一是将其添加到版本控制系统（VCS）。 在这个特殊情况下，我选择了Git和Bitbucket。

如果您安装了Git，JHipster会自动为您的项目初始化Git。以下命令显示我如何添加对远程Bitbucket存储库的引用，然后推送所有内容。我在执行这些命令之前在Bitbucket上创建了存储库。

```
$ git remote add origin git@bitbucket.org:mraible/21-points.git
$ git push origin master
Delta compression using up to 8 threads.
Compressing objects: 100% (495/495), done.
Writing objects: 100% (523/523), 616.70 KiB | 8.94 MiB/s, done.
Total 523 (delta 61), reused 0 (delta 0)
remote: Resolving deltas: 100% (61/61), done.
To bitbucket.org:mraible/21-points.git
* [new branch] master -> master
```

这就是我用JHipster创建一个新应用程序并将其检入源代码控制的方法。如果您按照类似的步骤创建应用程序，我相信有两种常用的方法可以继续。第一个是开发应用程序，然后进行测试和部署。第二种选择是建立持续集成，部署，然后开始开发和测试。在团队开发环境中，我建议使用第二个选项。但是，由于您可能会将其作为个人项目使用而阅读，因此我将遵循第一种方法并正确编码。如果您有兴趣与Jenkins建立持续集成，请参阅Setting
up Continuous Integration on Jenkins 2。

## 构建UI和业务逻辑

我希望21-Points Health比股票JHipster应用程序更加时髦。几年前Bootstrap风靡一时，但现在Google的Material Design越来越受欢迎。我在JHipster市场搜索了“material”，找到了Bootstrap Material Design模块。 不幸的是，我很快发现它不支持JHipster 4+。

在本书的第4版（和21-Points Health）中，我选择使用Bootstrap及其默认主题；改变一些变量使它看起来像Angular Material。 因为我已经习惯了，所以我决定为这个版本保留相同的设置。要使默认的Bootstrap主题看起来像Material Design，请修改```_bootstrapvariables.scss```并将其替换为下面的内容。

*src/main/webapp/content/scss/_bootstrap-variables.scss*

```
/*
* Bootstrap覆盖https://getbootstrap.com/docs/4.0/getting-started/theming/
* bootstrap源中定义的所有值
* https://github.com/twbs/bootstrap/blob/v4-dev/scss/_variables.scss可以被覆盖到这里
* 请确保不要在此处添加！默认值
*/
// 颜色：
// 在Bootstrap中使用的灰度和品牌颜色。
// 自定义颜色以匹配Bootstrap Material Theme
// https://github.com/FezVrasta/bootstrap-materialdesign/blob/master/sass/_variables.scss
$primary: #009688;
$success: #4caf50;
$info: #03a9f4;
$warning: #ff5722;
$danger: #f44336;
$blue: #0275d8;

// 可选项：
// 通过启用或禁用可选功能快速修改全局样式
$enable-rounded: true;
$enable-shadows: false;
$enable-gradients: false;
$enable-transitions: true;
$enable-hover-media-query: false;
$enable-grid-classes: true;
$enable-print-styles: true;

// 组件：
// 定义常见的填充和边框半径大小等。
$border-radius: 0.15rem;
$border-radius-lg: 0.125rem;
$border-radius-sm: 0.1rem;

// Body元素:
// 设置 `<body>` 元素.
$body-bg: #fff;

//排版
// body元素文本的字体，行高，颜色，headings及更多
$font-size-base: 0.9rem;
$border-radius: 2px;
$border-radius-sm: 1px;
$font-family-sans-serif: 'Roboto', 'Helvetica', 'Arial', sans-serif;
$headings-font-weight: 300;
$link-color: $primary;
$input-focus-border-color: lighten($blue, 25%);
$input-focus-box-shadow: none;
```

然后将以下Sass添加到```global.scss```的底部。

```
/* ==========================================================================
21-Points Health 自定义样式
==========================================================================*/
.jh-card {
    border: none !important;
}

.jh-navbar {
    background-color: #009688 !important;
}

.blockquote {
    padding: 0.5rem 1rem;
    margin-bottom: 1rem;
    font-size: 1rem !important;
    font-weight: 100;
    border-left: 0.25rem solid #eceeef;
}

a {
    font-weight: normal !important;
}

.truncate {
    width: 180px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    cursor: pointer;
    &.cal-day-notes {
        width: 150px;
    }
}

.footer {
    bottom: 0;
    left: 0;
    color: #666;
    background: #eee;
    border-top: 1px solid silver;
    position: fixed;
    width: 100%;
    padding: 10px;
    padding-bottom: 0;
    text-align: center;
    z-index: 2;
    font-size: 0.9em;
    p {
	margin-bottom: 7px;
    }
}

.thread-dump-modal-lock {
    max-width: 450px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}

/* 覆盖Bootstrap样式 vertical-align: top */
.table {
    th,
        td {
	    vertical-align: middle !important;
        }
}
```

> <center>如何在JHipster5中使用Bootstrap Material Design</center>
>
> 如果您想将Bootstrap Material Design与JHipster 5一起使用，那也是可能的。
>
> 以下是使用Bootstrap Material Design和Sass所需的步骤：
>
> 1.安装bootstrap-material-design：
>
> ```npm install bootstrap-material-design@4.1.1```
>
> 2.从```src/main/webapp/content/scss/_bootstrap-variables.scss```中删除所有变量。
>
> 3.在```src/main/webapp/content/scss/vendor.scss```中注释掉Bootstrap的导入：
>
> ```
> // 从node_modules导入Bootstrap源文件
> // @import '~bootstrap/scss/bootstrap';
> ```
>
> 4.在```src/main/webapp/content/scss/vendor.scss```中添加以下内容以导入bootstrap-material-design：
>
> ```
> // 导入 Bootstrap Material Design
> @import url('https://fonts.googleapis.com/css?family=Roboto:300,400,500,700|Material+Icons');
> @import '~bootstrap-material-design/scss/bootstrap-material-design';
> ```
>
> 5.从```global.scss```中删除以下样式：
>
> ```
> /* 输入字段上的错误高亮显示 */
> .ng-valid[required],
> .ng-valid.required {
>     border-left: 5px solid green;
> }
> 
> .ng-invalid:not(form) {
>     border-left: 5px solid red;
> }
> ```
>
> 6.在下拉菜单中添加以下覆盖，让它们变得好看起来。
>
> ```
> .dropdown-menu .dropdown-item.active, .dropdown-menu .dropdown-item:active {
>     color: #fff !important;
> }
>
> .dropdown-menu .dropdown-item {
>     display: inline-block !important;
>     padding: 0.5rem 1.5rem !important;
>     min-height: 2rem !important;
> }
> ```
>
> 以下是这些更改后的截图。
> ![图2.带有Bootstrap Angular Material的JHipster](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPtuBtueUq0cVVSy%2F2.jpg?alt=media&token=b7fd79be-db20-4e5d-819d-a4e0563837dc)
> 图2. 带有Bootstrap Angular Material的JHipster

此时，我第一次部署到Heroku。本章的部署到Heroku部分对此进行了介绍。

### 生成实体（entities）

* 数据库表;
* Liquibase变更集;
* JPA实体类;
* Spring Data ```JpaRepository```接口;
* Spring MVC ```RestController```类;
* Angular路由器，控制器和服务; 和
* HTML页面。

此外，您应该进行集成测试以验证一切正常和通过性能测试以验证它是否能够快速运行。在理想的状态下，您还可以为Angular代码进行单元测试和集成测试。

好消息是JHipster可以为您生成所有这些代码，包括集成测试和性能测试。此外，如果您有具有关联的实体，它将生成支持它们的必要模式（使用外键），以及用于管理它们的TypeScript和HTML代码。您还可以设置验证以要求某些字段以及控制它们的长度。

JHipster支持几种代码生成方法。第一个使用其实体子生成器。实体子生成器是一个命令行工具，可以提示您回答问题。 JDL-Studio是一个基于浏览器的工具，使用JHipster域语言（JDL）定义域模型。最后，JHipster-UML是那些喜欢UML的人的选择。支持的UML编辑器包括Modelio，UML Designer，GenMyModel和Visual Paradigm。因为实体子生成器是最简单的使用之一，所以我为此项目选择了它。

![提示](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)如果您想了解使用JDL-Studio是多么容易，请参阅我的Get Started with JHipster 5 Screencast。

此时，我使用数据模型进行了一些试错设计。我使用JHipster生成实体，尝试了应用程序，并改为以UI优先方法开始。作为一个用户，我希望能够轻松添加关于我是否锻炼，膳食健康或饮酒的日常条目。当我测量它时，我也想记录我的体重和血压指标。当我开始使用我刚刚创建的UI时，似乎它能够实现这些目标，但它似乎也有点麻烦。那时我决定使用主屏幕及其辅助屏幕创建一个UI模型进行数据输入。我使用OmniGraffle和Bootstrap模板来创建以下UI模型。

![图3.UI图样](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPu-vd_FuPwasPTm%2F3.jpg?alt=media&token=02075714-c6a8-4e53-bbc8-a2cc29ac3818)

图3.UI图样

在弄清楚我想要UI的样子之后，我开始考虑数据模型。我很快就决定不需要追踪高水平的目标（例如在2018年第三季度减掉十磅）。我更关注跟踪每周目标，而21-Points Health是关于您在一周内得到多少积分的项目。我创建了下图作为我的数据模型。

![图4.21-Points Health实体图](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPu2irnGH_fEYGQM%2F4.jpg?alt=media&token=ac90ece4-a345-4d4c-ad93-7d51eb720e57)

图4.21-Points Health实体图

我运行```jhipster entity points```。 我添加了相应的字段及其验证规则，并指定了与用户的多对一关系。以下是我的答案的最终结果。

```
================= Points =================
Fields
date (LocalDate) required
exercise (Integer)
meals (Integer)
alcohol (Integer)
notes (String) maxlength='140'

Relationships
user (User) many-to-one

? Do you want to use separate service class for your business logic? No, the REST controller should use the repository directly
? Do you want pagination on your entity? Yes, with pagination links

Everything is configured, generating the entity...

create .jhipster/Points.json
create src/main/resources/config/liquibase/changelog/20180828004742_added_entity_Points.xml
create src/main/resources/config/liquibase/changelog/20180828004742_added_entity_constraints_Points.xml
create src/main/java/org/jhipster/health/domain/Points.java
create src/main/java/org/jhipster/health/repository/PointsRepository.java
create src/main/java/org/jhipster/health/web/rest/PointsResource.java
create src/main/java/org/jhipster/health/repository/search/PointsSearchRepository.java
create src/test/java/org/jhipster/health/web/rest/PointsResourceIntTest.java
create src/test/java/org/jhipster/health/repository/search/PointsSearchRepositoryMockConfiguration.java
create src/test/gatling/user-files/simulations/PointsGatlingTest.scala
conflict src/main/resources/config/liquibase/master.xml
? Overwrite src/main/resources/config/liquibase/master.xml? overwrite this and all others
force src/main/resources/config/liquibase/master.xml
force src/main/java/org/jhipster/health/config/CacheConfiguration.java
create src/main/webapp/app/entities/points/points.component.html
create src/main/webapp/app/entities/points/points-detail.component.html
create src/main/webapp/app/entities/points/points-update.component.html
create src/main/webapp/app/entities/points/points-delete-dialog.component.html
force src/main/webapp/app/layouts/navbar/navbar.component.html
create src/main/webapp/i18n/en/points.json
force src/main/webapp/i18n/en/global.json
create src/main/webapp/i18n/fr/points.json
force src/main/webapp/i18n/fr/global.json
create src/main/webapp/app/entities/points/index.ts
create src/main/webapp/app/entities/points/points.module.ts
create src/main/webapp/app/entities/points/points.route.ts
create src/main/webapp/app/shared/model/points.model.ts
create src/main/webapp/app/entities/points/points.component.ts
create src/main/webapp/app/entities/points/points-update.component.ts
create src/main/webapp/app/entities/points/points-delete-dialog.component.ts
create src/main/webapp/app/entities/points/points-detail.component.ts
create src/main/webapp/app/entities/points/points.service.ts
create src/test/javascript/spec/app/entities/points/points-detail.component.spec.ts
create src/test/javascript/spec/app/entities/points/points-update.component.spec.ts
create src/test/javascript/spec/app/entities/points/points-delete-dialog.component.spec.ts
create src/test/javascript/spec/app/entities/points/points.component.spec.ts
create src/test/javascript/spec/app/entities/points/points.service.spec.ts
create src/test/javascript/e2e/entities/points/points.page-object.ts
create src/test/javascript/e2e/entities/points/points.spec.ts
force src/main/webapp/app/entities/entity.module.ts

Running `webpack:build` to update client app
```

您可以看到上面的日期和注释的验证规则，但是您没有看到我如何创建与用户的关系。以下是该部分的问题和答案。

```
? Do you want to add a relationship to another entity? Yes
? What is the name of the other entity? user
? What is the name of the relationship? user
? What is the type of the relationship? many-to-one
? When you display this relationship on client-side, which field from 'user' do you want
 to use? This field will
be displayed as a String, so it cannot be a Blob login
? Do you want to add any validation rules to this relationship? No
```

我对```Weight```和```BloodPressure```实体有类似的答案。 请参阅实体图，了解每个实体中的字段名称。 对于```Preferences```，我与```User```建立了一对一的关系。

为了确保人们有效地使用21-Points Health，我将每周目标设置为最少10分，最多21分。我还将```weightUnits```属性设置为枚举。

```
================= Preferences =================
Fields
weeklyGoal (Integer) required min='10' max='21'

Generating field #2

? Do you want to add a field to your entity? Yes
? What is the name of your field? weightUnits
? What is the type of your field? Enumeration (Java enum type)
? What is the class name of your enumeration? Units
? What are the values of your enumeration (separated by comma, no spaces)? kg,lb
? Do you want to add validation rules to your field? Yes
? Which validation rules do you want to add? Required

================= Preferences =================
Fields
weeklyGoal (Integer) required min='10' max='21'
weightUnits (Units) required
```

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)在生成具有日期/时间字段的```date```属性的```Weight```和```BloodPressure```实体之后，我确定```timestamp```是更好的属性名称。为了解决这个问题，我修改了```.jhipster```目录中的相应JSON文件，并再次为每个实体运行```jhipster entity```。 这似乎比使用IntelliJ重构更容易，并希望它能捕获所有名称实例。

当我运行```./gradlew```测试时，我很高兴看到所有测试都通过了。

```
BUILD SUCCESSFUL in 36s
```

在继续实现我的UI模型之前，我检查了六个更改的文件和由JHipster生成的130个新文件。

## 应用改进

为了使我的新JHipster应用程序成为我可以引以为傲的作品，我做了一些改进，如下所述。

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)此时，我使用Jenkins建立了对该项目的持续测试。本章的持续集成和部署部分对此进行了介绍。

### 改进HTML布局和I18N消息

在我编写的所有代码中，UI代码（HTML，JavaScript和CSS）是我的最爱。我喜欢它可以立即看到变化并快速取得进展 - 特别是当您使用带有[Browsersync](https://www.browsersync.io/)的双显示器时。下面是我对HTML所做的更改的综合列表，以使事情看起来更好：

* 改进了表格和按钮的布局，
* 通过编辑```src/main/webapp/i18n/en```中生成的JSON文件改进了标题和按钮标签，
* 使用Angular的DatePipe格式化本地时区的日期（例如：```{{bloodPressure.timestamp | date：'short'}}```），
* 默认为新条目的当前日期，
* 用列表/详细信息屏幕上的图标替换得分指标，和
* 在更新屏幕上使用复选框替换了点指标。

最大的视觉改进在列表屏幕上。我将按钮设置得更小，将按钮文本转换为工具提示，并将添加/搜索按钮移到右上角。对于得分列表屏幕，我将1和0度量标准值转换为图标。在得分列表的屏幕截图之前和之后说明了改进的紧凑布局。

![图5.默认每日得分列表](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPu5pTy4Z4BSMvlf%2F5.jpg?alt=media&token=a9e726af-44fa-4bd0-b6ac-6b4d49224edb)

图5.默认每日得分列表

![图6.UI改进后的默认每日点列表](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPu75lzgvABL5FPE%2F6.jpg?alt=media&token=73a02a9f-bca3-4c98-937a-d3670e204893)

图6.UI改进后的默认每日点列表

我在```points.component.html```顶部重构了HTML，以便在同一行上放置标题，搜索和添加按钮。我还删除了按钮文本，转而使用ng-bootstrap的tooltip指令。您在按钮工具提示中看到的```jhiTranslate```指令由JHipster的Angular库提供。

*src/main/webapp/app/entities/points/points.component.html*

```
<div class="row">
    <div class="col-sm-8">
        <h2 id="page-heading" jhitranslate="twentyOnePointsApp.points.home.title">Points</h2>
    </div>
    <div class="col-sm-4 text-right">
        <button id="jh-create-entity" class="btn btn-primary float-right jh-create-entity create-points" [routerlink]="['/points/new']" [ngbtooltip]="addTooltip" placement="bottom">
            <fa-icon [icon]="'plus'"></fa-icon>
            <ng-template #addtooltip="">
                <span jhitranslate="twentyOnePointsApp.points.home.createLabel">Add Points</span>
            </ng-template>
        </button>
        <form name="searchForm" class="form-inline">
            <div class="input-group w-100 mr-1">
                <input type="text" class="form-control" [(ngmodel)]="currentSearch" id="currentSearch" name="currentSearch" placeholder="{{ 'twentyOnePointsApp.points.home.search' | translate }}" />
                <button class="input-group-append btn btn-info" (click)="search(currentSearch)">
                    <fa-icon [icon]="'search'"></fa-icon>
                </button>
                <button class="input-group-append btn btn-danger" (click)="clear()" *ngif="currentSearch">
                    <fa-icon [icon]="'trash-alt'"></fa-icon>
                </button>
            </div>
        </form>
    </div>
</div>
```

感谢Angular的表达式语言，可以非常容易的将数字转换为图标。

*src/main/webapp/app/entities/points/points.component.html*

```
<td class="text-center">
    <fa-icon [icon]="points.exercise ? 'check' : 'times'" aria-hidden="true" class="{{points.exercise ? 'text-success' : 'text-danger'}}"></fa-icon>
</td>
<td class="text-center">
    <fa-icon [icon]="points.meals ? 'check' : 'times'" aria-hidden="true" class="{{points.meals ? 'text-success' : 'text-danger'}}"></fa-icon>
</td>
<td class="text-center">
    <fa-icon [icon]="points.alcohol ? 'check' : 'times'" aria-hidden="true" class="{{points.alcohol ? 'text-success' : 'text-danger'}}"></fa-icon>
</td>
```

添加此HTML后，我在浏览器的开发者控制台中看到了一个错误。

```
FontAwesome: Could not find icon with iconName=check and prefix=fas
```

为了解决这个问题，我修改了vendor.ts并将faCheck添加为导入图标。

*src/main/webapp/app/vendor.ts*

```
library.add(faTimes);
library.add(faCheck); // 添加这一行
```

接下来，我在```points-update.component.html```中将输入字段更改为复选框。

*src/main/webapp/app/entities/points/points-update.component.html*

```
<div class="form-check">
    <label class="form-check-label" for="field_exercise">
        <input type="checkbox" class="form-check-input" name="exercise" id="field_exercise" [(ngModel)]="points.exercise" />
        <span jhiTranslate="twentyOnePointsApp.points.exercise" for="field_exercise">Exercise</span>
    </label>
</div>
<div class="form-check">
    <label class="form-check-label" for="field_meals">
        <input type="checkbox" class="form-check-input" name="meals" id="field_meals" [(ngModel)]="points.meals" />
        <span jhiTranslate="twentyOnePointsApp.points.meals">Meals</span>
    </label>
</div>
<div class="form-check">
    <label class="form-check-label" for="field_alcohol">
        <input type="checkbox" class="form-check-input" name="alcohol" id="field_alcohol" [(ngModel)]="points.alcohol" />
        <span jhiTranslate="twentyOnePointsApp.points.alcohol" for="field_alcohol">Alcohol</span>
    </label>
</div>
```

在```points-update.component.ts```中，我不得不修改save()方法，将每个复选框的布尔值转换为整型值。

*src/main/webapp/app/entities/points/points-update.component.ts*

```
save() {
    this.isSaving = true;

    // 将布尔类型转换成整型
    this.points.exercise = this.points.exercise ? 1 : 0;
    this.points.meals = this.points.meals ? 1 : 0;
    this.points.alcohol = this.points.alcohol ? 1 : 0;

    if (this.points.id !== undefined) {
        this.subscribeToSaveResponse(this.pointsService.update(this.points));
    } else {
        this.subscribeToSaveResponse(this.pointsService.create(this.points));
    }
}
```

在进行这些更改，修改一些HTML并调整一些i18n消息之后，“添加分数”屏幕开始看起来像我创建的UI模型。

![图7.添加分数页面](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPuA8MCudf_065Gx%2F7.jpg?alt=media&token=11eeb48c-8c78-4858-b14e-88506597ee1a)

图7.添加分数页面

改进UI是最有趣的，但也是最耗时的，因为它涉及对多个屏幕的大量微调。下一个任务更直接：实现业务逻辑。

### 添加了逻辑，因此非管理员用户只能看到自己的数据

我希望根据用户的角色对用户可以看到的内容进行一些改进。用户应该能够查看和修改他们的数据，但没有其他人可以。我还想确保管理员可以查看和修改每个人的数据。

#### 隐藏非管理员用户的用户选择

多对一关系的默认更新组件允许您在添加/编辑记录时选择用户。为了使管理员具备这种能力，我修改了更新模板并使用了```*jhiHasAnyAuthority```指令。 该指令包含在JHipster中，位于```src/main/webapp/app/shared/auth/has-any-authority.directive.ts```中。 它允许您传入单个角色或角色列表。

*src/main/webapp/app/entities/points/points-update.component.html*

```
<div class="form-group" *jhiHasAnyAuthority="'ROLE_ADMIN'">
    <label class="form-control-label" jhiTranslate="twentyOnePointsApp.points.user" for="field_user">User</label>
    <select class="form-control" id="field_user" name="user" [(ngModel)]="points.user">
        <option [ngValue]="null"></option>
        <option [ngValue]="userOption.id === points.user?.id ? points.user : userOption" *ngFor="let userOption of users; trackBy: trackUserById">{{userOption.login}}</option>
    </select>
</div>
```

由于下拉列表对非管理员隐藏，因此我必须在创建新记录时将每个```Resource```类修改为默认为当前用户。下面是一个差异，显示了我需要对```PointsResource.java```进行的更改。

*src/main/java/org/jhipster/health/web/rest/PointsResource.java*

```
+import org.jhipster.health.repository.UserRepository;
+import org.jhipster.health.security.AuthoritiesConstants;
+import org.jhipster.health.security.SecurityUtils;

    private final PointsSearchRepository pointsSearchRepository;

-   public PointsResource(PointsRepository pointsRepository, PointsSearchRepository pointsSearchRepository) {
+  private final UserRepository userRepository;
+
+  public PointsResource(PointsRepository pointsRepository, PointsSearchRepository pointsSearchRepository,
+      UserRepository userRepository) {
             this.pointsRepository = pointsRepository;
             this.pointsSearchRepository = pointsSearchRepository;
+           this.userRepository = userRepository;
    }

    @PostMapping("/points")
    @Timed
    public ResponseEntity<Points> createPoints(@Valid @RequestBody Points points) throws URISyntaxException {
        log.debug("REST request to save Points : {}", points);
        if (points.getId() != null) {
            return ResponseEntity.badRequest().headers(
                HeaderUtil.createFailureAlert(ENTITY_NAME, "idexists",
                    "A new points cannot already have an ID")).body(null);
        }
+     if (!SecurityUtils.isCurrentUserInRole(AuthoritiesConstants.ADMIN)) {
+         log.debug("No user passed in, using current user: {}", SecurityUtils.getCurrentUserLogin());
+         points.setUser(userRepository.findOneByLogin(SecurityUtils.getCurrentUserLogin()).get());
+     }
        Points result = pointsRepository.save(points);
        pointsSearchRepository.save(result);
        return ResponseEntity.created(new URI("/api/points/" + result.getId()))
            .headers(HeaderUtil.createEntityCreationAlert(ENTITY_NAME, result.getId().toString()))
            .body(result);
```

```SecurityUtils```是JHipster在创建项目时提供的类。 在进行此更改后，我必须修改```PointsResourceIntTest.java```以使其具有安全性。

Spring MVC Test提供了一个名为```RequestPostProcessor```的便捷接口，您可以使用它来修改请求。 Spring Security提供了许多简化测试的```RequestPostProcessor```实现。 为了使用Spring Security的```RequestPostProcessor```实现，您可以使用以下静态导入包含它们。

```
import static org.springframework.security.test.web.servlet.request.SecurityMockMvcRequestPostProcessors.*;
```

然后，我修改了```PointsResourceIntTest.java```，创建了一个新的```MockMvc```实例，该实例具有安全性，并使用```with(user("user"))```指定，以使用经过身份验证的用户填充Spring Security的```SecurityContext```。

*src/test/java/org/jhipster/health/web/rest/PointsResourceIntTest.java*

```
+ import org.jhipster.health.domain.User;
+ import org.springframework.web.context.WebApplicationContext;
+ import java.time.DayOfWeek;
+ import java.time.format.DateTimeFormatter;
+ import java.time.temporal.ChronoField;
+ import static org.springframework.security.test.web.servlet.request.SecurityMockMvcRequestPostProcessors.user;
+ import static org.springframework.security.test.web.servlet.setup.SecurityMockMvcConfigurers.springSecurity;

public class PointsResourceIntTest {
    ...
    @Autowired
    private PointsSearchRepository pointsSearchRepository;

    + @Autowired
    + private UserRepository userRepository;

    ...

    + @Autowired
    + private WebApplicationContext context;
    +
    private MockMvc restPointsMockMvc;

    private Points points;

    @Before
    public void setup() {
        MockitoAnnotations.initMocks(this);
        - PointsResource pointsResource = new PointsResource(pointsRepository, pointsSearchRepository);
        + PointsResource pointsResource = new PointsResource(pointsRepository, pointsSearchRepository, userRepository);
        this.restPointsMockMvc = MockMvcBuilders.standaloneSetup(pointsResource)
            .setCustomArgumentResolvers(pageableArgumentResolver)
            .setControllerAdvice(exceptionTranslator)
            .setMessageConverters(jacksonMessageConverter).build();
    }

    ...

    public void createPoints() throws Exception {
        int databaseSizeBeforeCreate = pointsRepository.findAll().size();
        + // 创建安全感知mockMvc
        + restPointsMockMvc = MockMvcBuilders
        + .webAppContextSetup(context)
        + .apply(springSecurity())
        + .build();

        +

        // 创建Points
        restPointsMockMvc.perform(post("/api/points")
        + .with(user("user"))
            .contentType(TestUtil.APPLICATION_JSON_UTF8)
            .content(TestUtil.convertObjectToJsonBytes(points)))
            .andExpect(status().isCreated());
        ....
    }

}
```

#### 列表屏幕应仅显示用户的数据

我想要的下一个业务逻辑改进是修改列表屏幕，以便它们只显示当前用户的记录。管理员用户应该看到所有用户的数据。为了方便这个功能，我修改了```PointsResource#getAll```使其根据用户的角色进行切换。而不是向您展示方法的差异，这是整个要做事情。

*src/main/java/org/jhipster/health/web/rest/PointsResource.java*

```
public ResponseEntity < List < Points >> getAllPoints(@ApiParam Pageable pageable) {
    log.debug("REST request to get a page of Points");
    Page < Points > page;
    if (SecurityUtils.isCurrentUserInRole(AuthoritiesConstants.ADMIN)) {
        page = pointsRepository.findAllByOrderByDateDesc(pageable);
    } else {
        page = pointsRepository.findByUserIsCurrentUser(pageable);
    }
    HttpHeaders headers = PaginationUtil.generatePaginationHttpHeaders(page, "/api/points");
    return new ResponseEntity < >(page.getContent(), headers, HttpStatus.OK);
}
```

JHipster生成的```PointsRepository#findByUserIsCurrentUser()```方法包含一个自定义查询，该查询使用Spring Expression Language从Spring Security获取用户的信息。 我将它从返回```List<Points>```更改为返回```Page<Points>```。

*src/main/java/org/jhipster/health/repository/PointsRepository.java*

```
@Query("select points from Points points where points.user.login = ?#{principal.username}")
Page findByUserIsCurrentUser(Pageable pageable);
```

> <center>按日期排序</center>
>
> 后来，我将上面的查询更改为按日期排序，因此列表中的第一个记录将是最新的。
>
> *src/main/java/org/jhipster/health/repository/PointsRepository.java*
>
> @Query("select points from Points points where points.user.login = ?#{principal.username} order by points.date desc")
>
> 另外，我将对```pointsRepository.findAll```的调用更改为```pointsRepository.findAllByOrderByDateDesc```，因此admin用户的查询将按日期排序。对此的查询是由Spring Data动态生成的，只需将方法添加到存储库即可。
>
> ```
> Page findAllByOrderByDateDesc(Pageable pageable);
> ```

为了使测试通过，我不得不更新```PointsResourceIntTest#getAllPoints```以使用Spring Security Test的```user```后期(post)处理器。

*src/test/java/org/jhipster/health/web/rest/PointsResourceIntTest.java*

```
@Test
@Transactional
public void getAllPoints() throws Exception {
    // 初始化数据库
    pointsRepository.saveAndFlush(points);

    + // 创建安全感知的mockMvc
    + restPointsMockMvc = MockMvcBuilders
    + .webAppContextSetup(context)
    + .apply(springSecurity())
    + .build();
    +
    // 获取全部points
    - restPointsMockMvc.perform(get("/api/points?sort=id,desc"))
    + restPointsMockMvc.perform(get("/api/points?sort=id,desc")
    + .with(user("admin").roles("ADMIN")))
    .andExpect(status().isOk())
```

#### 实现UI模型

使主页变成类似于我的UI模型(mockup)的东西需要几个步骤：

1.添加按钮以方便从主页添加新数据。
2.添加API以获取本周获得的积分。
3.添加API以获取过去30天的血压读数。
4.添加API以获取过去30天的体重。
5.在每周显示得分和最近30天的血压/体重并添加到图表。

我开始重用更新组件来输入JHipster为我创建的数据。 我使用Angular的```routerLink```语法导航到组件，从每个实体的主列表页面中复制。 例如，下面是“增加分数”（Add Points）按钮的代码。

```
<a [routerLink]="['/points/new']" class="btn btn-primary m-0 mb-1 text-white">Add Points</a>
```

然后我不得不修改```home.component.ts```来监听这些组件在修改实体时触发的事件。

*src/main/webapp/app/home/home.component.ts*

```
import {
    JhiEventManager
}
from 'ng-jhipster';

import {
    Component,
    OnDestroy,
    OnInit
}
from '@angular/core';

import {
    Subscription
}
from 'rxjs';

...
export class HomeComponent implements OnInit, OnDestroy {
    ...
    eventSubscriber: Subscription;
    constructor(..., private eventManager: EventManager) {}

    ngOnDestroy() {
        this.eventManager.destroy(this.eventSubscriber);
    }

    registerAuthenticationSuccess() {
        this.eventManager.subscribe('authenticationSuccess', () = >{
            this.principal.identity().then((account) = >{
                this.account = account;
                this.getUserData();
            });
        });
        this.eventSubscriber = this.eventManager
            .subscribe('pointsListModification', () = >this.getUserData());
        this.eventSubscriber = this.eventManager
            .subscribe('bloodPressureListModification', () = >this.getUserData());
        this.eventSubscriber = this.eventManager
            .subscribe('weightListModification', () = >this.getUserData());
    }
    ...
}
```

#### 本周得分

为了得到本周获得的积分，我首先在```PointsResourceIntTest.java```中添加了一个单元测试，以便我证明我的API正在运行。

*src/test/java/org/jhipster/health/web/rest/PointsResourceIntTest.java*

```
private void createPointsByWeek(LocalDate thisMonday, LocalDate lastMonday) {
    User user = userRepository.findOneByLogin("user").get();
    // 在两个星期内创建积分
    points = new Points(thisMonday.plusDays(2), 1, 1, 1, user);①
    pointsRepository.saveAndFlush(points);

    points = new Points(thisMonday.plusDays(3), 1, 1, 0, user);
    pointsRepository.saveAndFlush(points);

    points = new Points(lastMonday.plusDays(3), 0, 0, 1, user);
    pointsRepository.saveAndFlush(points);

    points = new Points(lastMonday.plusDays(4), 1, 1, 0, user);
    pointsRepository.saveAndFlush(points);
}

@Test
@Transactional
public void getPointsThisWeek() throws Exception {
    LocalDate today = LocalDate.now();
    LocalDate thisMonday = today.with(DayOfWeek.MONDAY);
    LocalDate lastMonday = thisMonday.minusWeeks(1);
    createPointsByWeek(thisMonday, lastMonday);

    // 创建安全感知的mockMvc
    restPointsMockMvc = MockMvcBuilders
        .webAppContextSetup(context)
        .apply(springSecurity())
        .build();

    // 获取全部积分
    restPointsMockMvc.perform(get("/api/points")
        .with(user("user").roles("USER")))
        .andExpect(status().isOk())
        .andExpect(content().contentTypeCompatibleWith(MediaType.APPLICATION_JSON))
        .andExpect(jsonPath("$", hasSize(4)));

    // 仅获取本周积分
    restPointsMockMvc.perform(get("/api/points-this-week")
        .with(user("user").roles("USER")))
        .andExpect(status().isOk())
        .andExpect(content().contentTypeCompatibleWith(MediaType.APPLICATION_JSON))
        .andExpect(jsonPath("$.week").value(thisMonday.toString()))
        .andExpect(jsonPath("$.points").value(5));
}
```

```①```为了简化测试，我在```Points.java```中添加了一个新的构造函数，其中包含我想要设置的参数。 我为我创建的大多数测试继续这种模式。

当然，这个测试在我第一次运行时失败，因为```/api/points-this-week```在```PointsResource.java```中不存在。 您可能会注意到points-this-week API需要两个返回值：```week```字段中的日期和```points```字段中的点数。 我在项目的```rest.vm```包中创建了```PointsPerWeek.java```来保存这些信息。

*src/main/java/org/jhipster/health/web/rest/vm/PointsPerWeek.java*

```
package org.jhipster.health.web.rest.vm;

import java.time.LocalDate;

public class PointsPerWeek {
    private LocalDate week;
    private Integer points;

    public PointsPerWeek(LocalDate week, Integer points) {
        this.week = week;
        this.points = points;
    }

    public Integer getPoints() {
        return points;
    }

    public void setPoints(Integer points) {
        this.points = points;
    }

    public LocalDate getWeek() {
        return week;
    }

    public void setWeek(LocalDate week) {
        this.week = week;
    }

    @Override
    public String toString() {
        return "PointsThisWeek{" + "points=" + points + ", week=" + week + '}';
    }
}
```

Spring Data JPA可以轻松查找特定周内的所有得分条目。我在```PointsRepository.java```中添加了一个新方法，允许我在两个日期之间进行查询。

*src/main/java/org/jhipster/health/repository/PointsRepository.java*

```
List findAllByDateBetweenAndUserLogin(LocalDate firstDate, LocalDate secondDate, String login);
```

从这里，只需计算当前周的开始和结束并处理```PointsResource.java```中的数据即可。

*src/main/java/org/jhipster/health/web/rest/PointsResource.java*

```
/**
* GET /points : 获取本周的全部得分
*/
@GetMapping("/points-this-week")
@Timed
public ResponseEntity < PointsPerWeek > getPointsThisWeek(
    @RequestParam(value = "tz", required = false) String timezone) {

    // 获取当前日期 (如果传入，则带有时区)
    LocalDate now = LocalDate.now();
    if (timezone != null) {
        now = LocalDate.now(ZoneId.of(timezone));
    }

    // 得到一周内的第一天
    LocalDate startOfWeek = now.with(DayOfWeek.MONDAY);
    // 得到一周内的最后一天
    LocalDate endOfWeek = now.with(DayOfWeek.SUNDAY);
    log.debug("Looking for points between: {} and {}", startOfWeek, endOfWeek);

    List < Points > points =
        pointsRepository.findAllByDateBetweenAndUserLogin(
            startOfWeek, endOfWeek, SecurityUtils.getCurrentUserLogin());
    return calculatePoints(startOfWeek, points);
}

private ResponseEntity < PointsPerWeek > calculatePoints(LocalDate startOfWeek,
List < Points > points) {
    Integer numPoints = points.stream()
        .mapToInt(p - >p.getExercise() + p.getMeals() + p.getAlcohol())
        .sum();

    PointsPerWeek count = new PointsPerWeek(startOfWeek, numPoints);
    return new ResponseEntity < >(count, HttpStatus.OK);
}
```

为了在客户端上支持这种新方法，我在位于```src/main/webapp/app/entities/points/points.service.ts```文件中的```PointsService```内添加了一个新方法。

*src/main/webapp/app/entities/points/points.service.ts*

```
thisWeek() : Observable < EntityResponseType > {
    const tz = Intl.DateTimeFormat().resolvedOptions().timeZone;
    return this.http
        .get(`api / points - this - week ? tz = $ {tz}`, {observe: 'response'})
        .pipe(map((res: EntityResponseType) = >this.convertDateFromServer(res)));
}
```

然后我将服务作为依赖项添加到```home.component.ts```并计算我想要显示的数据。

*src/main/webapp/app/home/home.component.ts*

```
import {
    Account,
    LoginModalService,
    Principal
}
from 'app/core';

import {
    PreferencesService
}
from 'app/entities/preferences';

import {
    BloodPressureService
}
from 'app/entities/blood-pressure';

import {
    WeightService
}
from 'app/entities/weight';

import {
    D3ChartService
}
from './d3-chart.service';

import {
    Preferences
}
from 'app/shared/model/preferences.model';

...

export class HomeComponent implements OnInit, OnDestroy {
    account: Account;
    modalRef: NgbModalRef;
    pointsThisWeek: any = {};
    pointsPercentage: number;

    constructor(private principal: Principal,
                     private loginModalService: LoginModalService,
                     private eventManager: EventManager,
                     private pointsService: PointsService) {
    }

    getUserData() {
        // 获取本周的全部得分
        this.pointsService.thisWeek().subscribe((points: any) = >{
            points = points.body;
            this.pointsThisWeek = points;
            this.pointsPercentage = (points.points / 21) * 100;
        });
    }
    ...
}
```

我在```home.component.html```中添加了一个进度条，以显示本周的进度。

*src/main/webapp/app/home/home.component.html*

```
<div class="row">
    <div class="col-md-11">
        <ngb-progressbar max="21" [value]="pointsThisWeek.points"
            [hidden]="!pointsThisWeek.points" [striped]="true">
                <span *ngIf="pointsThisWeek.points">
                    {{pointsThisWeek.points}} / Goal: 10
                </span>
        </ngb-progressbar>
        <ngb-alert [dismissible]="false" [hidden]="pointsThisWeek.points">
            <span jhiTranslate="home.points.getMoving">
                No points yet this week, better get moving!</span>
        </ngb-alert>
    </div>
</div>
```

下面是重新启动服务器并为当前用户输入一些数据后，此进度条的外观截图。

![图8.本周积分进度条](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPuD_gck_qN2I2iz%2F8.jpg?alt=media&token=3760a8f5-ae4d-44ce-9cdc-1bc647081865)

图8.本周积分进度条

您可能会注意到目标在进度条的HTML中被硬编码为10。要解决这个问题，我需要添加获取用户首选项的功能。为了更容易访问用户的首选项，我修改了```PreferencesRepository.java```并添加了一个方法来检索用户的首选项。

*src/main/java/org/jhipster/health/repository/PreferencesRepository.java*

```
public interface PreferencesRepository extends JpaRepository {
    Optional findOneByUserLogin(String login);
}
```

我在```PreferencesResource.java```中创建了一个新方法来返回用户的首选项（如果没有定义首选项，则默认每周目标为10分）。

*src/main/java/org/jhipster/health/web/rest/PreferencesResource.java*

```
/**
* GET /my-preferences ->获取当前用户的首选项.
*/
@GetMapping("/my-preferences")
@Timed
public ResponseEntity < Preferences > getUserPreferences() {
    String username = SecurityUtils.getCurrentUserLogin().get();
    log.debug("REST request to get Preferences : {}", username);
    Optional < Preferences > preferences =
        preferencesRepository.findOneByUserLogin(username);

    if (preferences.isPresent()) {
        return new ResponseEntity < >(preferences.get(), HttpStatus.OK);
    } else {
        Preferences defaultPreferences = new Preferences();
        defaultPreferences.setWeeklyGoal(10); // default
        return new ResponseEntity < >(defaultPreferences, HttpStatus.OK);
    }
}
```

为了便于调用此端点，我在客户端的```PreferencesService```中添加了一个新的用户方法。

*src/main/webapp/app/entities/preferences/preferences.service.ts*

```
user() : Observable {
    return this.http.get('api/my-preferences', {
        observe: 'response'
    });
}
```

在```home.component.ts```中，我将```PreferencesService```添加为依赖项，并在本地```preferences```变量中设置首选项，以便HTML模板可以读取它。我还添加了一个用于```preferences```更新和逻辑的监听器来计算进度条的背景颜色。

*src/main/webapp/app/home/home.component.ts*

```
export class HomeComponent implements OnInit, OnDestory {
    ...
    preferences: Preferences;

    constructor(...
        private preferencesService: PreferencesService,
        private pointsService: PointsService) {
    }

    registerAuthenticationSuccess() {
        ...
        this.eventSubscriber = this.eventManager.subscribe(
            'preferencesListModification', () = >this.getUserData());
    }

    getUserData() {
        // 获取首选项
        this.preferencesService.user().subscribe((preferences: any) = >{
            this.preferences = preferences.body;

            // 获取本周积分
            this.pointsService.thisWeek().subscribe((points: any) = >{
                points = points.body;
                this.pointsThisWeek = points;
                this.pointsPercentage =
                (points.points / this.preferences.weeklyGoal) * 100;

                // 计算成功，警告或危险
                if (points.points >= preferences.weeklyGoal) {
                    this.pointsThisWeek.progress = 'success';
                } else if (points.points < 10) {
                    this.pointsThisWeek.progress = 'danger';
                } else if (points.points > 10 &&
                points.points < this.preferences.weeklyGoal) {
                    this.pointsThisWeek.progress = 'warning';
                }
            });
            ...
        });
    }
    ...
}
```

现在用户的首选项可用，我修改了```home.component.html```以显示用户的每周目标，并使用```[type]```属性适当地为进度条着色。

*src/main/webapp/app/home/home.component.html*

```
<ngb-progressbar max="21" [value]="pointsThisWeek.points"
            [type]="pointsThisWeek.progress" [striped]="true"
            [hidden]="!pointsThisWeek.points">
    <span *ngIf="pointsThisWeek.points">
        {{pointsThisWeek.points}} / Goal: {{preferences.weeklyGoal}}
    </span>a
</ngb-progressbar>
<ngb-alert [dismissible]="false" [hidden]="pointsThisWeek.points">
    <span jhiTranslate="home.points.getMoving">
        No points yet this week, better get moving!
    </span>
</ngb-alert>
```

为了完成这项工作，我添加了一个指向用户可以编辑其首选项的组件的链接。

*src/main/webapp/app/home/home.component.html*

```
<a [routerLink]="['/preferences', preferences.id, 'edit']"
    class="float-right" jhiTranslate="home.link.preferences">Edit Preferences</a>
```

#### 最近30天的血压和体重

要填充主页上的其余两个图表，我需要获取用户的血压读数和过去30天的权重。我在```BloodPressureResourceIntTest.java```中添加了一个方法来设置我的期望。

*src/test/java/org/jhipster/health/web/rest/BloodPressureResourceIntTest.java*

```
private void createBloodPressureByMonth(ZonedDateTime firstDate, ZonedDateTime firstDayOfLastMonth) {
    User user = userRepository.findOneByLogin("user").get();
    bloodPressure = new BloodPressure(firstDate, 120, 80, user);
    bloodPressureRepository.saveAndFlush(bloodPressure);
    bloodPressure = new BloodPressure(firstDate.plusDays(10), 125, 75, user);
    bloodPressureRepository.saveAndFlush(bloodPressure);
    bloodPressure = new BloodPressure(firstDate.plusDays(20), 100, 69, user);
    bloodPressureRepository.saveAndFlush(bloodPressure);

    // 最后一个月
    bloodPressure = new BloodPressure(firstDayOfLastMonth, 130, 90, user);
    bloodPressureRepository.saveAndFlush(bloodPressure);
    bloodPressure = new BloodPressure(firstDayOfLastMonth.plusDays(11), 135, 85, user);
    bloodPressureRepository.saveAndFlush(bloodPressure);
    bloodPressure = new BloodPressure(firstDayOfLastMonth.plusDays(23), 130, 75, user);
    bloodPressureRepository.saveAndFlush(bloodPressure);
}

@Test
@Transactional
public void getBloodPressureForLast30Days() throws Exception {
    ZonedDateTime now = ZonedDateTime.now();
    ZonedDateTime twentyNineDaysAgo = now.minusDays(29);
    ZonedDateTime firstDayOfLastMonth = now.withDayOfMonth(1).minusMonths(1);
    createBloodPressureByMonth(twentyNineDaysAgo, firstDayOfLastMonth);

    // 创建安全感知的mockMvc
    restBloodPressureMockMvc = MockMvcBuilders
        .webAppContextSetup(context)
        .apply(springSecurity())
        .build();

    // 获取所有的血压读数
    restBloodPressureMockMvc.perform(get("/api/blood-pressures")
        .with(user("user").roles("USER")))
        .andExpect(status().isOk())
        .andExpect(content().contentTypeCompatibleWith(MediaType.APPLICATION_JSON))
        .andExpect(jsonPath("$", hasSize(6)));

    // 获取过去30天的血压读数
    restBloodPressureMockMvc.perform(get("/api/bp-by-days/{days}", 30)
        .with(user("user").roles("USER")))
        .andDo(print())
        .andExpect(status().isOk())
        .andExpect(content().contentTypeCompatibleWith(MediaType.APPLICATION_JSON))
        .andExpect(jsonPath("$.period").value("Last 30 Days"))
        .andExpect(jsonPath("$.readings.[*].systolic").value(hasItem(120)))
        .andExpect(jsonPath("$.readings.[*].diastolic").value(hasItem(69)));
}
```

我创建了一个```BloodPressureByPeriod.java```类来返回API的结果。

*src/main/java/org/jhipster/health/web/rest/vm/BloodPressureByPeriod.java*

```
public class BloodPressureByPeriod {
    private String period;
    private List < BloodPressure > readings;

    public BloodPressureByPeriod(String period, List < BloodPressure > readings) {
        this.period = period;
        this.readings = readings;
    }
    ...
}
```

使用points-this-week的类似逻辑，我在```BloodPressureRepository.java```中创建了一个新方法，允许我在两个不同的日期之间进行查询。我还添加了“OrderBy”逻辑，因此记录将按输入的日期排序。

*src/main/java/org/jhipster/health/repository/BloodPressureRepository.java*

```
List findAllByTimestampBetweenOrderByTimestampDesc(
    ZonedDateTime firstDate, ZonedDateTime secondDate);
```

接下来，我在```BloodPressureResource.java```中创建了一个新方法，该方法计算当前月份的第一天和最后一天，执行当前用户的查询，并构造返回的数据。

*src/main/java/org/jhipster/health/web/rest/BloodPressureResource.java*

```
/**
* GET /bp-by-days : 获得最近x天的所有血压读数。
*/
@RequestMapping(value = "/bp-by-days/{days}")
@Timed
public ResponseEntity < BloodPressureByPeriod > getByDays(@PathVariable int days) {
    ZonedDateTime rightNow = ZonedDateTime.now(ZoneOffset.UTC);
    ZonedDateTime daysAgo = rightNow.minusDays(days);

    List < BloodPressure > readings =
        bloodPressureRepository.findAllByTimestampBetweenOrderByTimestampDesc(daysAgo, rightNow);
    BloodPressureByPeriod response =
        new BloodPressureByPeriod("Last " + days + " Days", filterByUser(readings));
    return new ResponseEntity < >(response, HttpStatus.OK);
}

private List < BloodPressure > filterByUser(List < BloodPressure > readings) {
    Stream < BloodPressure > userReadings = readings.stream()
        .filter(bp - >bp.getUser().getLogin().equals(SecurityUtils.getCurrentLogin().get()));
    return userReadings.collect(Collectors.toList());
}
```

> <center>按方法过滤</center>
> 
> 我后来通过向```BloodPressureRepository.java```添加以下方法，学习了如何在数据库中进行过滤：
> 
> *src/main/java/org/jhipster/health/repository/BloodPressureRepository.java*
> 
> ```
> List findAllByTimestampBetweenAndUserLoginOrderByTimestampDesc(
>     ZonedDateTime firstDate, ZonedDateTime secondDate, String login);
> ```
>
> 然后我能够删除```filterByUser```方法并将```BloodPressureResource#getByDays```更改为：
>
> *src/main/java/org/jhipster/health/web/rest/BloodPressureResource.java*
>
> ```
> public ResponseEntity < BloodPressureByPeriod > getByDays(@PathVariable int days) {
>     ZonedDateTime rightNow = ZonedDateTime.now();
>     ZonedDateTime daysAgo = rightNow.minusDays(days);
> 
>     List < BloodPressure > readings =
>         bloodPressureRepository.findAllByTimestampBetweenAndUserLoginOrderByTimestampDesc(
>             daysAgo, rightNow, SecurityUtils.getCurrentUserLogin().get());
>     BloodPressureByPeriod response =
>         new BloodPressureByPeriod("Last " + days + " Days", readings);
>     return new ResponseEntity < >(response, HttpStatus.OK);
> }
> ```

我在```blood-pressure.service.ts```中添加了一种新方法来支持这个API。

*src/main/webapp/app/entities/blood-pressure/blood-pressure.service.ts*

```
last30Days() : Observable < EntityResponseType > {
    return this.http
        .get('api/bp-by-days/30', {observe: 'response'})
        .pipe(map((res: EntityResponseType) = >this.convertDateFromServer(res)));
}
```

虽然收集这些数据似乎很容易，但困难的部分是弄清楚用什么图表库来显示它。

#### 最近30天的图表

根据我编写本书前两个版本的经验，我找到了一个与D3.js集成的Angular库，并找到了ng2-nvd3。要安装ng2-nvd3，我使用了Yarn的add命令。

```
Yarn add ng2-nvd3
```

然后我更新了```home.module.ts```以导入```NvD3Module```，以及我发现必要的其他导入。

*src/main/webapp/app/home/home.module.ts*

```
import {
    NvD3Module
}
from 'ng2-nvd3';

import 'd3';
import 'nvd3';

@NgModule({
    imports: [
        TwentyOnePointsSharedModule,
        NvD3Module,
        ...
    ],
    ...
})
export class TwentyOnePointsHomeModule {}
```

我修改了```home.component.ts```以将```BloodPressureService```作为依赖项并开始构建数据，以便用D3可以渲染它。我发现图表需要一些JSON来配置它们，所以我创建了一个服务来包含这个配置。

*src/main/webapp/app/home/d3-chart.service.ts*

```
declare const d3, nv: any;

/**
* ChartService定义D3的图表配置
*/
export class D3ChartService {

    static getChartConfig() {
        const today = new Date();
        const priorDate = new Date().setDate(today.getDate() - 30);
        return {
            chart: {
                type: 'lineChart',
                height: 200,
                margin: {
                    top: 20,
                    right: 20,
                    bottom: 40,
                    left: 55
                },
                x(d) {
                    return d.x;
                },
                y(d) {
                    return d.y;
                },
                useInteractiveGuideline: true,
                dispatch: {},
                xAxis: {
                    axisLabel: 'Dates',
                    showMaxMin: false,
                    tickFormat(d) {
                        return d3.time.format('%b %d')(new Date(d));
                    }
                },
                xDomain: [priorDate, today],
                yAxis: {
                    axisLabel: '',
                    axisLabelDistance: 30
                },
                transitionDuration: 250
            },
            title: {
                enable: true
            }
        };
    }
}
```

在```home.component.ts```中，我捕获到API的血压读数，并将它们变成D3可以理解的数据。

*src/main/webapp/app/home/home.component.ts*

```
// 获取过去30天的血压读数
this.bloodPressureService.last30Days().subscribe((bpReadings: any) = >{
    bpReadings = bpReadings.body;
    this.bpReadings = bpReadings;
    this.bpOptions = {...D3ChartService.getChartConfig()
    };

    if (bpReadings.readings.length) {
        this.bpOptions.title.text = bpReadings.period;
        this.bpOptions.chart.yAxis.axisLabel = 'Blood Pressure';
        let systolics, diastolics, upperValues, lowerValues;
        systolics = [];
        diastolics = [];
        upperValues = [];
        lowerValues = [];
        bpReadings.readings.forEach((item) = >{
            systolics.push({
                x: new Date(item.timestamp),
                y: item.systolic
            });
            diastolics.push({
                x: new Date(item.timestamp),
                y: item.diastolic
            });
            upperValues.push(item.systolic);
            lowerValues.push(item.diastolic);
        });

        this.bpData = [{
            values: systolics,
            key: 'Systolic',
            color: '#673ab7'
        }, {
            values: diastolics,
            key: 'Diastolic',
            color: '#03a9f4'
        }];
        // 将y比例（scale）设置为比最大值和最小值多10
        this.bpOptions.chart.yDomain =
            [Math.min.apply(Math, lowerValues) - 10,
                Math.max.apply(Math, upperValues) + 10];
    } else {
        this.bpReadings.readings = [];
    }
});
```

最后，我使用```home.component.html```中的“nvd3”指令来读取```bpOptions```和```bpData```，然后显示一个图表。

*src/main/webapp/app/home/home.component.html*

```
<div class="row mt-1">
    <div class="col-md-11 col-xs-12">
        <span *ngIf="bpReadings.readings && bpReadings.readings.length">
            <nvd3 [options]="bpOptions" [data]="bpData"
                class="with-3d-shadow with-transitions"></nvd3>
        </span>
        <ngb-alert [dismissible]="false"
            [hidden]="bpReadings.readings && bpReadings.readings.length">
            <span jhiTranslate="home.bloodPressure.noReadings">
                No blood pressure readings found.
            </span>
        </ngb-alert>
    </div>
</div>
```

输入一些测试数据后，我对结果非常满意。

![图9.过去30天内的血压值图表](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPuGsD2clKiMdcl4%2F9.jpg?alt=media&token=623cc9b4-b371-41ff-bf80-7eb6b41a8fcc)

图9.过去30天内的血压值图表

我对过去30天的显示权重图表进行了类似的更改。

#### 代码行

在完成21-Points Health的MVP（最小可运行产品）之后，我做了一些快速计算，看看JHipster产生了多少行代码。您可以从下图中看到，我只需要编写2,080行代码。JHipster为我做了剩下的工作，在我的项目中产生了91.2％的代码！

![图10.项目代码行数](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPuQTIhzOOuDn5uf%2F10.jpg?alt=media&token=300ac7e0-33bb-48b4-9c1f-448a4484a5a1)

图10.项目代码行数

为了进一步深入研究，我制作了项目中前三种语言的图表：Java，TypeScript和HTML。

![图11.按语言划分代码行](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPuUnvH761-85kKq%2F11.jpg?alt=media&token=0967379b-bb54-49b2-a109-d91b593dd5a4)

图11.按语言划分代码行

我必须用每种语言编写的代码量是561行TypeScript，687行Java和351行HTML。 其他481行是JSON（135），XML（187），Sass（80），YAML（54），CSS（12），Markdown（6），Groovy（4）和Bourne Shell（3）。

嗬！ 谢谢，JHipster！

> <center>测试</center>
> 您可能已经注意到我编写的许多Java代码都是针对测试的。 我觉得这些测试对于证明我实施的业务逻辑是正确的至关重要。 使用日期并不容易，但Java 8的Date-Time API大大简化了它，Spring Data JPA可以轻松编写“日期之间”查询。
> 我相信TDD（测试驱动开发）是编写代码的好方法。 但是，在开发UI时，我倾向于在编写测试之前使它们工作。 它通常是一种非常直观的活动，在Browsersync的帮助下，在您看到您的变化之前很少有延迟。 我喜欢使用Jasmine为我的Angular组件和指令编写单元测试，我喜欢用Protractor编写集成测试。
> 我没有在本节中展示任何UI测试，但JHipster为我生成了它们。 运行```yarn test --coverage```显示UI中包含78.98％的线！

## 部署到Heroku

JHipster支持部署到Cloud Foundry，Heroku，Kubernetes，Microsoft Azure，OpenShift，Rancher，AWS和Boxfuse。我使用Heroku将我的应用程序部署到云端，因为我之前使用过它。当您为生产环境准备JHipster应用程序时，建议使用预先配置的“prod”配置文件。使用Gradle，您可以通过在构建时指定此配置文件来打包应用程序。

```
./gradlew -Pprod bootWar
```

使用Maven时，该命令看起来类似。

```
./mvnw -Pprod package
```

生产配置文件用于构建优化的JavaScript客户端。您可以通过运行```yarn webpack：prod```来使用webpack调用它。生产配置文件还使用servlet过滤器，缓存标头和通过Metrics标准进行监视来配置gzip压缩。如果您在```application-prod.yml```文件中配置了Graphite服务器，您的应用程序将自动向其发送指标(metrics)数据。

要部署21-Points Health，我登录了我的Heroku帐户。我已经安装了Heroku CLI。

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)我在创建应用程序后首先部署到Heroku，这意味着我有一个没有实体的默认JHipster应用程序。

```
$ heroku login
Enter your Heroku credentials.
Email: matt@raibledesigns.com
Password (typing will be hidden):
Authentication successful.
```

我按照Heroku文档中的建议运行z```jhipster heroku```。在提示时使用 “21points”作为我的应用程序。

```
$ jhipster heroku
Heroku configuration is starting
? Name to deploy as: 21points
? On which region do you want to deploy ? us
? Which type of deployment do you want ? Git (compile on Heroku)

Using existing Git repository

Heroku CLI deployment plugin already installed

Creating Heroku application and setting up node environment
    { Error: Command failed: heroku create 21-points
Creating 21-points... !
    Name must start with a letter and can only contain lowercase letters,
    numbers, and dashes.
```

您可以看到我的第一次尝试失败的原因与创建初始JHipster应用程序失败的原因相同：它不喜欢应用程序名称以数字开头。我再次尝试“health”，但也失败了，因为已经存在具有此名称的Heroku应用程序。最后，我选择了“health-by-points”作为应用程序名称。

```
$ jhipster heroku
Using JHipster version installed locally in current project's node_modules
Executing jhipster:heroku
Options:
Heroku configuration is starting
? Name to deploy as: health-by-points
? On which region do you want to deploy ? us

Using existing Git repository

Heroku CLI deployment plugin already installed

Creating Heroku application and setting up node environm
https://health-by-points.herokuapp.com/ | https://git.heroku.com/health-by-points.git

Provisioning addons
Created Elasticsearch addon
Created Database addon

Creating Heroku deployment files
    create src/main/resources/config/bootstrap-heroku.yml
    create src/main/resources/config/application-heroku.yml
    create Procfile
    create gradle/heroku.gradle
conflict build.gradle
? Overwrite build.gradle? overwrite this and all others
    force build.gradle

Skipping build

Updating Git repository
git add .
git commit -m "Deploy to Heroku" --allow-empty

Configuring Heroku

Deploying application

remote: Compressing source files... done.
remote: Building source:
...

remote: BUILD SUCCESSFUL in 4m 30s
remote: 8 actionable tasks: 7 executed, 1 up-to-date
remote: -----> Discovering process types
remote: Procfile declares types -> web
remote:
remote: -----> Compressing...
remote: Done: 141.3M
remote: -----> Launching...
remote: Released v6
remote: https://health-by-points.herokuapp.com/ deployed to Heroku
remote:
remote: Verifying deploy... done.
To https://git.heroku.com/health-by-points.git
* [new branch] HEAD -> master
```

我很高兴看到这个过程有效，我的应用程序可以在[http://health-by-points.herokuapp.com](https://health-by-points.herokuapp.com/#/)上找到。我迅速更改了admin和user的默认密码，以使程序更安全。

![图12.首次部署到Heroku](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPuX-T0Bc6pjRxTs%2F12.jpg?alt=media&token=a06d82c3-c483-4bdc-b7da-085fe946fe38)

图12.首次部署到Heroku

接下来，我从Google Domains购买了21-points.com域名。要为Heroku配置此域名，须运行```heroku domain:add```。

```
$ heroku domains:add www.21-points.com
Adding www.21-points.com to health-by-points... done
! Configure your app's DNS provider to point to the DNS Target www.21-points.com
! For help, see https://devcenter.heroku.com/articles/custom-domains
```

我阅读了文档，然后开始在Google Domains上配置DNS设置。我配置了一个子域名：

```
21-points.com → http://www.21-points.com
```

我还使用CNAME配置了一个自定义资源记录，指向```health-by-points.herokuapp.com```。

表1.Google Domains上的自定义资源记录

| **名称** | **类型** | **TTL** | **数据**                       |
| -------- | -------- | ------- | ------------------------------ |
| *        | CNAME    | 1h      | health-by-points.herokuapp.com |

这就是让我的JHipster应用程序在Heroku上运行所需的全部内容。对于后续部署，我再次运行```jhipster heroku```，或者使用```git push heroku master```。

> <center>JAR部署到Heroku</center>
> 如果您使用Heroku进行JAR部署，除了使用```jhipster heroku```之外，您还可以使用heroku-cli-deploy重新部署应用程序。 使用以下命令安装此插件。
> ```
> heroku plugins:install heroku-cli-deploy
> ```
> 之后，您可以打包JHipster项目进行生产环境部署。 使用Gradle，它看起来像这样。
> ```
> ./gradlew -Pprod bootWar
> heroku war:deploy build/libs/*war --app health-by-points
> ```
> 使用Maven，命令看起来略有不同：
> ```
> ./mvnw package -Pprod
> heroku war:deploy target/*war --app health-by-points
> ```

### Heroku上的Elasticsearch

为了证明一切都在Heroku上工作，我尝试注册一个新用户。我收到了一个似乎来自Elasticsearch的错误。

```
2018-08-27T21:15:27.588565+00:00 app[web.1]: 2018-08-27 21:15:27.587 ERROR 4 --- [ XNIO-2 task-15]
    o.z.p.spring.web.advice.AdviceTrait : Internal Server Error
2018-08-27T21:15:27.588578+00:00 app[web.1]:
2018-08-27T21:15:27.588581+00:00 app[web.1]: org.elasticsearch.client.transport.NoNodeAvailableException:
    None of the configured nodes are available: [{#transport#-1}{7Du0TB4WQ_6crjWFO1iJVQ}{localhost}{127.0.0.1:9300}]
2018-08-27T21:15:27.588583+00:00 app[web.1]: at org.elasticsearch.client.transport.TransportClientNodesService
    .ensureNodesAreAvailable(TransportClientNodesService.java:347)
2018-08-27T21:15:27.588585+00:00 app[web.1]: at org.elasticsearch.client.transport.TransportClientNodesService.execute
    (TransportClientNodesService.java:245)
```

我在JHipster项目中创建了一个问题，说Elasticsearch不能与Heroku一起开箱即用。 我与Heroku的Joe Kutner合作提出了一个解决方案并解决了问题。

Heroku上的Elasticsearch修复程序在JHipster 5.3.0中发布，因此我使用升级子生成器进行了升级。

命令完成后，我将更改为```yarn.lock```，运行```git push```，并使用```jhipster heroku```再次部署到Heroku。

### Heroku上的邮件

进行此更改后，我重新打包并重新部署。这次，当我尝试注册时，当我的```MailService```尝试向我发送激活电子邮件时收到错误。

```
2018-08-27T21:26:12.193734+00:00 app[web.1]: 2017-08-14 21:26:12.193 WARN 4 --- [ints-Executor-2]
    org.jhipster.health.service.MailService : Email could not be sent to user 'mraible@gmail.com':
    Mail server connection failed; nested exception is com.sun.mail.util.MailConnectException:
    Couldn't connect to host, port: localhost, 25; timeout -1;
2018-08-27T21:26:12.193748+00:00 app[web.1]: nested exception is:
2018-08-27T21:26:12.193751+00:00 app[web.1]: java.net.ConnectException: Connection refused
    (Connection refused). Failed messages: com.sun.mail.util.MailConnectException: Couldn't connect
    to host, port: localhost, 25; timeout -1;
```

我过去曾使用Heroku的SendGrid收发电子邮件，因此我将其添加到了我的项目中。

```
$ heroku addons:create sendgrid
Creating giving-softly-5465... done, (free)
Adding giving-softly-5465 to health-by-points... done
Setting SENDGRID_PASSWORD, SENDGRID_USERNAME and restarting health-by-points... done, v17
Use `heroku addons:docs sendgrid` to view documentation.
```

然后我更新了```application-prod.yml```，以便为邮件使用已配置的```SENDGRID_PASSWORD```和```SENDGRID_USERNAME```环境变量，以及启用身份验证。

*src/main/resources/config/application-prod.yml*

```
mail:
    host: smtp.sendgrid.net
    port: 587
    username: ${SENDGRID_USERNAME}
    password: ${SENDGRID_PASSWORD}
    protocol: smtp
    properties:
        tls: false
        auth: true
```

我还在此文件中进一步更改了```jhipster.mail.*```属性。

```
mail:
    from: app@21-points.com
    base-url: http://www.21-points.com
```

重新打包和重新部署后，我使用应用程序的内置运行状况检查功能来验证是否正确配置了所有内容。

## 监控和分析

JHipster在每个应用程序的```src/main/webapp/index.html```文件中生成Google
Analytics所需的代码。 我选择不启用它，但我希望最终能使用它。我已经拥有Google Analytics帐户，因此只需为www.21-points.com创建新帐户，复制帐号以及修改```index.html```的以下部分：

*src/main/webapp/index.html*

```
<!-- Google Analytics: uncomment and change UA-XXXXX-X to be your site's ID.
<script>
    (function(b,o,i,l,e,r){b.GoogleAnalyticsObject=l;b[l]||(b[l]=
    function(){(b[l].q=b[l].q||[]).push(arguments)});b[l].l=+new Date;
    e=o.createElement(i);r=o.getElementsByTagName(i)[0];
    e.src='//www.google-analytics.com/analytics.js';
    r.parentNode.insertBefore(e,r)}(window,document,'script','ga'));
    ga('create','UA-XXXXX-X');ga('send','pageview');
</script>-->
```

我过去曾使用New Relic来监控我的生产应用程序。 Heroku有一个免费的New Relic插件。 Heroku的New Relic APM描述了如果让Heroku为您构建（如果您使用```git push heroku master```部署），如何设置。 但是，如果您使用的是heroku deploy插件，则会有所不同。

为此，您首先需要手动下载New Relic代理以及newrelic.yml许可文件，并将它们放在项目的根目录中。然后您可以运行如下命令：

```
heroku war:deploy build/libs/*war --includes newrelic.jar:newrelic.yml
```

这将把JAR文件包括在其中。然后您需要修改procfile以包含javaagent参数：

```
web: java -javaagent:newrelic.jar $JAVA_OPTS -Xmx256m -jar build/libs/*.war ...
```

## 保护用户数据

在Heroku上运行5.0版本的21-Points Health项目几周之后，有人在GitHub上报告了项目的安全性问题。他们指出，通过搜索可以看到另一个用户的数据。我还发现根据URL可以编辑数据。

为了解决这个数据泄漏问题，我增强了Java代码，因此只允许拥有实体的用户编辑它。这里有一些sudo代码来显示逻辑：

```
Optional < Points > points = pointsRepository.findById(id);
if (points.isPresent() && <user doesn 't match current user>) {
    return new ResponseEntity<>("error.http.403", HttpStatus.FORBIDDEN);
}
return ResponseUtil.wrapOrNotFound(points);'
```

请参阅pull request#52，了解我需要在资源类及其测试中进行的所有更改。

## 持续集成和部署

在为这个项目生成实体之后，每当我检查Git的更改时，我都想配置一个持续集成（CI）服务器来构建/测试/部署。我为我的CI服务器选择了Jenkins并使用了最简单的配置：我在我的MacBook Pro上将```jenkins.war```下载到```/opt/tools/jenkins```。 我用以下命令启动它。

```
java -jar jenkins.war --httpPort=9000
```

JHipster有关于在Jenkins 2上设置CI并部署到Heroku的良好文档。它还有一个便捷的子生成器，用于生成Jenkins所需的配置文件。运行```jhipster ci-cd```然后，看！魔法出现了。

```
$ jhipster ci-cd
Using JHipster version installed locally in current project's node_modules
Executing jhipster:ci-cd
Options:
" Welcome to the JHipster CI/CD Sub-Generator "
? What CI/CD pipeline do you want to generate? Jenkins pipeline
? Would you like to perform the build in a Docker container ? No
? Would you like to send build status to GitLab ? No
? What tasks/integrations do you want to include ? Deploy to *Heroku* (requires HEROKU_API_KEY set on CI service)
? *Heroku*: name of your Heroku Application ? health-by-points
    create Jenkinsfile
    create src/main/docker/jenkins.yml
    create src/main/resources/idea.gdsl
```

生成这些文件后，我检查了它们并推送到Bitbucket。

> <center>Jenkins选项</center>
> 选择Jenkins时，您还可以为任务/集成选择以下选项：
> * 将工件（artifact）部署到Artifactory。
> * 使用Sonar分析代码。
> * 构建和发布Docker镜像。

导航到http://localhost:9000登录Jenkins。从启动日志文件中复制并粘贴密码以解锁Jenkins页面。

![图13.解锁Jenkins](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPu_LdWaLfIlbV2j%2F13.jpg?alt=media&token=5fd9fe65-e3e1-4584-bf89-947d6ac6680b)

图13.解锁Jenkins

接下来，我选择安装已选插件并等待其安装完成。

![图14.自定义Jenkins](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPubzC6wdW_Wuio2%2F14.jpg?alt=media&token=51efcc40-990b-495b-8d46-b951a7a79c7e)

图14.自定义Jenkins

我用SCM的Pipeline脚本创建了一个名为“21-points”的新作业（job）。我配置了一个“Poll SCM”构建触发器，其时间表为```H/5 * * * *```。 保存作业后，我确认它成功运行。

![图15.构建Jenkins #1](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPue31tdQajlt7B3%2F15.jpg?alt=media&token=49df5eac-b31e-47f3-8497-6ba8dab24c68)

图15.构建Jenkins #1

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)第一次```部署```可能会失败。如果发生这种情况，请停止Jenkins，运行```heroku login```，然后重新启动Jenkins。

我修改了这个文件，添加了```protractor test```阶段来运行所有的Protractor测试。我检查了我的更改以触发另一个构建。

*Jenkinsfile*

```
# ! /usr/bin / env groovy

node {
    stage('checkout') {
        checkout scm
    }

    stage('check java') {
        sh "java -version"
    }

    stage('clean') {
        sh "chmod +x gradlew"
        sh "./gradlew clean --no-daemon"
    }

    stage('install tools') {
        sh "./gradlew yarn_install -PnodeInstall --no-daemon"
    }

    stage('backend tests') {
        try {
            sh "./gradlew test -PnodeInstall --no-daemon"
        } catch(err) {
            throw err
        } finally {
            junit '**/build/**/TEST-*.xml'
        }
    }

    stage('frontend tests') {
        try {
            sh "./gradlew yarn_test -PnodeInstall --no-daemon"
        } catch(err) {
            throw err
        } finally {
            junit '**/build/test-results/jest/TESTS-*.xml'
        }
    }

    stage('protractor tests') {
        sh '''./gradlew &
        bootPid=$!
        sleep 60s
        yarn e2e
        kill $bootPid
        '''
    }

    stage('packaging') {
        sh "./gradlew bootWar -x test -Pprod -PnodeInstall --no-daemon"
        archiveArtifacts artifacts: '**/build/libs/*.war', fingerprint: true
    }

    stage('deployment') {
        sh "./gradlew deployHeroku --no-daemon"
    }
}
```

观察管道中所有阶段的状态。

![图16.Jenkins部署成功！](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPuhJF0qS58h9vLL%2F16.jpg?alt=media&token=68d617a7-67d2-4730-b1d8-06a7075e34c2)

图16.Jenkins部署成功！

在处理这个项目时，我会启动Jenkins并在我检查（checked）代码时运行它。 我没有将它安装在服务器上并让它持续运行。 我的理由很简单：我只是在击发（bursts）编码，不需要浪费运算周期或支付云实例来运行它。

## 代码质量

当我完成应用程序的开发时，我想确保我有良好的代码质量，并且测试得很好。JHipster默认生成具有高代码质量的应用程序。使用Sonar分析代码质量，Sonar由JHipster自动配置。“代码质量”度量标准由测试所涵盖的代码百分比确定。为了查看我完成的应用程序的代码质量，我启动了prod配置文件所需的所有Docker容器以及Sonar。

```
docker-compose -f src/main/docker/elasticsearch.yml up -d
docker-compose -f src/main/docker/postgresql.yml up -d
docker-compose -f src/main/docker/sonar.yml up -d
```

然后我运行了所有测试和sonarqube任务。

```
/gradlew -Pprod clean test sonarqube
```

完成此过程后，可在Sonar仪表板上获取该项目的分析，网址为http://127.0.0.1:9001。21-Points Heath是一款三A级应用程序！不错，嗯？

![图17.Sonar分析结果](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPum5eq-kLiFR-XC%2F17.jpg?alt=media&token=373b75a0-d6cd-4d87-adbf-366c341b49af)

图17.Sonar分析结果

## 渐进式网络应用程序

渐进式网络应用程序（又称PWA）是开发人员使其Web应用程序加载速度更快，性能更佳的最佳方式。 简而言之，PWA是使用最新Web标准的网站，允许在用户的计算机或设备上安装，并为这些用户提供类似应用程序的体验。

成为PWA需要三个功能：

1. 该应用必须通过HTTPS提供。
2. 应用程序必须注册服务工作者，以便它可以缓存请求并脱机工作。
3. 应用程序必须包含带有安装信息和图标的Web应用程序清单。

对于HTTPS，您可以将JHipster的“tls”配置文件用于localhost，或者（甚至更好）将其部署到生产环境中！

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)要在带有Gradle的localhost上使用HTTPS，请为后端运行```./gradlew-Ptls```，为前端运行```yarn start-tls```。

像Heroku和Cloud Foundry这样的云服务提供商将为您提供开箱即用的HTTPS，但他们不会强迫它。为了强制HTTPS，我在```src/main/java/org/jhipster/health/config/SecurityConfiguration.java```进行了修改并添加了一条规则，以便在发送```X-Forwarded-Proto```标头时强制建立安全通道。

```
@Override
public void configure(HttpSecurity http) throws Exception {
    http
        ...
    .and()
        .apply(securityConfigurerAdapter())
    .and()
        .requiresChannel()
        .requestMatchers(r -> r.getHeader("X-Forwarded-Proto") != null)
        .requiresSecure();
}
```

workbox-webpack-plugin已经配置用于生成服务工作者，但它仅在使用生产配置文件运行应用程序时才有效。 这很好，因为这意味着您在开发时不会在浏览器中缓存数据。

为了注册服务工作者，我修改了```src/main/webapp/index.html```并取消注释了以下代码块。

```
<script>
if ('serviceWorker' in navigator) {
    window.addEventListener('load', function() {
        navigator.serviceWorker.register('/service-worker.js')
            .then(function() {
                console.log('Service Worker Registered');
            });
    });
}
</script>
```

最终功能 -- 网络应用程序清单 - 已包含在```src/main/webapp/manifest.webapp```中。 它定义应用程序名称，颜色和图标。

在进行这些更改后，我将21-Points Health重新部署到生产环境中并使用Lighthouse进行分析。 您可以在下面的屏幕截图中看到结果。

![图18.Lighthouse分析](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPup0yY7h-qbtanP%2F18.jpg?alt=media&token=cc036846-957f-4bf3-abe6-790fe50e4408)

图18.Lighthouse分析

您可以看到仍然需要进行一些性能改进。我没有在这里解决这些问题，而是创建了一个可以订阅的问题。

## 源码

在将此应用程序置于足够好的状态后，我将其从Bitbucket移至GitHub并将其作为开源项目提供。您可以在[https://github.com/mraible/21-points](https://github.com/mraible/21-points)找到21-Points Health的源代码。

## 升级21-Points Health

在用JHipster 5.3.0完成了21-Points Health的MVP后，我想将它升级到最新版本。我使用了upgrade sub-generator，并将其升级到5.3.4，然后再升级到5.4.2。

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)如果在升级JHipster时升级子生成器不适合您，则可以手动升级。您需要先确保将项目检入源代码管理。 然后，创建一个新分支并在项目中运行```rm -rf *```，然后运行```jhipster --with-entities```。 完成此过程后，使用您喜欢的工具（例如，IntelliJ IDEA）来比较更改的文件并恢复您的自定义代码。

如果您正在阅读此内容并注意到21-Points Health使用的是比5.4.2更新的版本，则可能是因为我再次升级。

## 总结

本节向您展示了我如何使用JHipster创建健康跟踪Web应用程序。 它引导您完成升级到最新版本的JHipster以及如何使用```jhipster entity```生成代码。 您在编写新API时学习了如何进行测试优先开发，以及Spring Data JPA如何轻松添加自定义查询。 您还了解了如何在不同页面上重用现有组件，如何向客户端服务添加方法，以及如何操作数据以显示漂亮的图表。

在将应用程序修改为我的UI模型之后，我向您展示了如何部署到Heroku以及我遇到的一些常见问题。 最后，您学习了如何使用Jenkins构建，测试和部署基于Gradle的JHipster项目。 我强烈建议您在创建项目后立即执行类似操作并验证它是否通过了所有测试。

在下一章中，我将更详细地解释JHipster的UI组件。 Angular，Bootstrap，webpack，Sass，WebSockets和Browsersync都包含在JHipster应用程序中，因此深入了解这些技术并了解更多信息非常有用。

