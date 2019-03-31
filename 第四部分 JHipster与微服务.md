# 第四部分 JHipster与微服务

采用微服务架构为您的系统添加故障转移和弹性提供了独特的机会，因此您的组件可以优雅地处理负载峰值和错误。 微服务也使更改变得更便宜。 当你有一个庞大的团队在一个产品上工作时，它们也是一个好主意。 您可以将项目分解为可以彼此独立运行的组件。 一旦组件可以独立运行，就可以独立构建，测试和部署它们。 这为组织及其团队提供了快速开发和部署的灵活性。

在深入研究代码教程之前，我想谈谈微服务，它们的历史，以及为什么你应该（或不应该）考虑下一个项目使用微服务架构。

## 微服务历史

根据维基百科的说法，2011年5月威尼斯附近的软件架构师研讨会上首次使用术语“微服务”作为一种常见的架构风格。2012年5月，同一组决定“微服务”是一个更合适的名称。

当时在Netflix工作的Adrian Cockcroft将这种架构描述为“细粒度的SOA”。 Martin Fowler和James Lewis于2014年3月25日撰写了一篇名为“微服务”的文章。多年后，这仍被认为是微服务的权威文章。

## 组织架构和康威定律

传统上，技术被组织成技术层：UI团队，数据库团队，运营团队。当团队沿着这些线分开时，即使是简单的更改也会导致跨队项目消耗大量的时间和预算。

一个聪明的团队将围绕这一点进行优化，并选择两个恶魔中较小的一个：将逻辑强制转换为他们可以访问的任何应用程序。这是康威定律的一个例子。

![图37.康威定律](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPvr1HYSIE3CkgI9%2F37.jpg?alt=media&token=566e6ec4-4efe-46be-85a1-a6af1b117cae)

图37.康威定律

> 设计系统（广泛定义）的任何组织都将生成一种设计，其结构是组织通信结构的副本。
>
> —— Melvyn Conway，1967年

### 微服务架构哲学

微服务架构的哲学基本上等同于“做一件事，做好它”的Unix哲学。微服务架构的特征如下：

* 通过服务进行组件化，
* 围绕业务能力进行组织，
* 产品不是项目，
* 智能端点和哑管，
* 分散治理，
* 分散的数据管理，
* 基础设施自动化
* 故障设计，和
* 渐近式设计。

## 为何选择微服务？

对于大多数开发人员，开发团队和组织来说，更容易处理小型“做好一件事”的服务。单体程序代表整个应用程序，因此服务可以在不增加成本的情况下更改框架（甚至语言）。只要服务使用与语言无关的协议（HTTP或轻量级消息传递），应用程序就可以在几个不同的平台上编写 - Java，Ruby，Node，Go，.NET等 - 没有问题。

平台即服务（PaaS）提供商和容器使部署微服务变得容易。支持整体所需的所有技术（例如，负载平衡，发现，流程监控）都是由容器外部的PaaS提供的。部署工作接近于零。

### 微服务是未来吗？

架构决策的后果，比如采用微服务，通常只有在你使用它们数年后才会变得明显。微服务在LinkedIn，Twitter，Facebook，Amazon.com和Netflix等公司取得了成功，但这并不意味着它们将对您的组织取得成功。组件边界很难定义。如果您无法干净利落地创建组件，那么您只需将组件内部的复杂性转移到组件之间的连接。此外，还需要考虑团队能力。弱小的团队总是会创建一个弱小的系统。

> 您不应该从微服务架构开始。相反，从整体开始，保持模块化，并在整体成为问题时将其拆分为微服务。
>
> —— 马丁福勒

## 使用JHipster的微服务

在这个例子中，我将向您展示如何使用JHipster构建微服务架构。作为此过程的一部分，您将生成三个应用程序并运行其他几个应用程序。

* 生成网关。
* 生成博客微服务。
* 生成商店微服务。
* 运行JHipster Registry，Keycloak和MongoDB。

您可以在下图中看到这些组件如何配合

![图38.JHipster微服务架构](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPvuzT_P6rQFfYR0%2F38.jpg?alt=media&token=38afe1ee-9df9-4853-a6c6-580f3b256e95)

图38.JHipster微服务架构

要查看应用程序中发生的情况，可以使用JHipster控制台，这是一种基于Elastic Stack的监视工具。我将在Docker Compose部分介绍JHipster Console。

本教程将向您展示如何使用JHipster 5.4.2构建微服务架构。您将生成一个网关（由Netflix Zuul提供支持），一个博客微服务（与PostgreSQL通信），一个商店微服务（使用MongoDB），并使用Docker Compose确保它们都在本地运行。然后把它们全部部署到Heroku。

## 生成API网关和微服务应用程序

JHipster 5.3.x中添加的新功能之一是使用```import-jdl```命令生成完整的微服务堆栈的功能。 打开一个终端窗口，创建一个目录（例如，```jhipster-microservices-example```）并在其中创建一个```apps.jh```文件。 将下面的JDL复制到此文件中。

*Example 1. apps.jh*

```
application { ①
    config {
        baseName gateway,
        packageName com.okta.developer.gateway,
        applicationType gateway,
        authenticationType oauth2, ②
        prodDatabaseType postgresql,
        searchEngine elasticsearch, ③
        serviceDiscoveryType eureka, ④
        testFrameworks [protractor] ⑤
    }
    entities Blog, Post, Tag, Product
}

application {
    config {
        baseName blog,
        packageName com.okta.developer.blog,
        applicationType microservice, ⑥
        authenticationType oauth2, ⑦
        prodDatabaseType postgresql,
        searchEngine elasticsearch,
        serverPort 8081, ⑧
        serviceDiscoveryType eureka
    }
    entities Blog, Post, Tag
}

application {
    config {
        baseName store,
        packageName com.okta.developer.store,
        applicationType microservice,
        authenticationType oauth2,
        databaseType mongodb, ⑨
        devDatabaseType mongodb,
        prodDatabaseType mongodb,
        enableHibernateCache false,
        searchEngine elasticsearch,
        serverPort 8082,
        serviceDiscoveryType eureka
    }
    entities Product
}

⑩
entity Blog {
    name String required minlength(3),
    handle String required minlength(2)
}

entity Post {
    title String required,
    content TextBlob required,
    date Instant required
}

entity Tag {
    name String required minlength(2)
}

entity Product {
    title String required,
    price BigDecimal required min(0),
    image ImageBlob
}

⑪
relationship ManyToOne {
    Blog{user(login)} to User,
    Post{blog(name)} to Blog
}

relationship ManyToMany {
    Post{tag(name)} to Tag{post}
}

⑫
paginate Post, Tag with infinite-scroll
paginate Product with pagination

⑬
microservice Product with store
microservice Blog, Post, Tag with blog
```

①JDL支持v3.0中的完整堆栈定义！
②网关的认证类型为OAuth 2.0。
③为网关指定弹性搜索是JHipster为使用```elasticsearch```的微服务生成正确的客户端代码所必需的。
④必须将```eureka```指定为网关和所有微服务应用程序的服务发现类型。
⑤在网关上包含Protractor，您可以使用```npm run e2e```测试所有内容。
⑥对于```microservice```应用程序，您需要指定微服务的应用程序类型。
⑦微服务应用程序的身份验证类型必须与网关匹配。
⑧默认服务器端口为8080.您必须为每个应用程序指定不同的端口。
⑨商店应用程序使用MongoDB作为其数据库。使用NoSQL选项时，必须为dev和prod使用相同的数据库。
⑩实体定义位于应用程序定义之外。您可以使用JDL-Studio验证JDL。
⑪实体之间的关系可以在JDL中定义！
⑫如果要在列表屏幕上分页，可以使用无限滚动或页面链接。
⑬您需要指定哪些微服务包含哪些实体或API类在您的网关上而不是在微服务中生成。

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)你也可以在GitHub上找到这个文件

运行JHipster的```import-jdl```命令导入此微服务架构定义。

```
jhipster import-jdl apps.jh
```

项目生成过程将需要一两分钟才能运行，具体取决于您的Internet连接速度和硬件。

在您等待的同时，您可以开始使用Okta设置OAuth。

### 什么是OAuth 2.0?

JHipster中的OAuth实现利用Spring Boot及其OAuth 2.0支持（```@EnableOAuthSso```注释）。 OAuth为JHipster应用程序提供单点登录（SSO）。”使用Spring Security OAuth保护微服务”展示了使用OAuth的简单的Spring微服务架构。JHipster使用相同的内部设置。

JHipster默认配置为OAuth配置的Keycloak。这对于本地开发非常有用。但是，如果要将应用程序部署到生产环境，则可能需要使用始终打开的身份提供程序，如Okta。 Okta提供永久免费的帐户，每月免费为您提供1,000个用户。

要配置您的应用程序以使用Okta，您首先需要创建一个免费的开发人员帐户。完成后，您将获得自己的Okta域名，其名称为https://dev-123456.oktapreview.com。

![OpenID](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx09gK90TKdS8vl%2Fopenid.jpg?alt=media&token=07bf55bd-ffed-4556-a1e6-8f2d5e68adba)

### 在Okta上创建一个OpenID 连接应用程序

在Okta中创建一个OpenID Connect（OIDC）应用程序以获取客户端ID和密码。这基本上意味着你用Okta“注册”你的应用程序。登录您的Okta帐户并导航到“应用程序”>“添加应用程序”。单击Web，然后单击下一步。给应用程序起你能记住的名字（例如，```JHipster Microservices```），并指定http://localhost:8080/login作为登录重定向URI。单击“完成”并记下您的客户端ID和客户端密钥值。

为了使来自Okta的角色与JHipster中的默认角色相匹配，您需要创建它们。创建```ROLE_ADMIN```和```ROLE_USER```组（用户>组>添加组）并向其添加用户。您可以使用您注册的帐户，或创建新用户（用户>添加人员）。导航到API>授权服务器，单击“授权服务器”选项卡并编辑默认选项卡。单击声明选项卡和添加声明。将其命名为```roles```，并将其包含在ID令牌中。将值类型设置为```Groups```，并将过滤器设置为```.*```的正则表达式。

修改```gateway/src/main/resources/config/application.yml```以具有以下值：

```
security:
    oauth2:
        client:
            access-token-uri: https://{yourOktaDomain}/oauth2/default/v1/token
            user-authorization-uri: https://{yourOktaDomain}/oauth2/default/v1/authorize
            client-id: {clientId}
            client-secret: {clientSecret}
            scope: openid profile email
        resource:
            user-info-uri: https://{yourOktaDomain}/oauth2/default/v1/userinfo
```

您还可以使用环境变量来覆盖默认值。我建议使用这种技术，因为1）您不需要修改每个微服务应用程序中的值; 2）它可以防止您在源代码存储库中泄露客户机密。 创建```~/.okta.env```并将下面的```export```命令复制到其中。 有了该文件，您可以运行```source ~/.okta.env```来覆盖默认的Spring Security设置。

```
export SECURITY_OAUTH2_CLIENT_ACCESS_TOKEN_URI="https://{yourOktaDomain}/oauth2/default/v1/token"
export SECURITY_OAUTH2_CLIENT_USER_AUTHORIZATION_URI="https://{yourOktaDomain}/oauth2/default/v1/authorize"
export SECURITY_OAUTH2_RESOURCE_USER_INFO_URI="https://{yourOktaDomain}/oauth2/default/v1/userinfo"
export SECURITY_OAUTH2_CLIENT_CLIENT_ID="{clientId}"
export SECURITY_OAUTH2_CLIENT_CLIENT_SECRET="{clientSecret}"
```

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)如果要将Okta设置设为默认值，可以将```source ~/.okta.env```添加到```~/.bashrc```（或```~/.zshrc```）。

如果您在```application.yml```中对Okta设置进行硬编码，请确保在博客中更新设置并存储应用程序。如果您正在使用环境变量，则无需进行任何更改。

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)如果您正在使用Protractor并想要针对Okta运行测试，则需要添加一个用户到Okta上的```ROLE_ADMIN```组并更改凭据以匹配该用户文件位于```src/test/javascript/e2e/account/account.spec.ts```和```src/test/javascript/e2e/admin/administration.spec.ts```。

## 启动JHipster Registry，Keycloak和MongoDB

在启动网关之前，您需要安装服务发现服务器。您还需要Keycloak进行身份验证，使用MongoDB作为商店微服务。博客应用程序依赖于Elasticsearch和PostgreSQL，但仅限于在生产模式下运行时。幸运的是，JHipster创造了Docker为您的应用所依赖的所有服务撰写文件。 您可以从项目的根目录运行以下命令，以启动JHipster Registry，Keycloak和MongoDB的Docker容器。

```
docker-compose -f gateway/src/main/docker/jhipster-registry.yml up -d
docker-compose -f gateway/src/main/docker/keycloak.yml up -d
docker-compose -f store/src/main/docker/mongodb.yml up -d
```

默认情况下，JHipster Registry在使用Docker Compose运行时将使用Keycloak。为了使一切正常工作，您需要在```hosts```文件中为Keycloak添加一个条目。

```
127.0.0.1 keycloak
```

如果要将其更改为使用Okta，则需要修改```gateway/src/main/docker/jhipster-registry.yml```并更改默认的Keycloak设置以使用Okta设置或环境变量（推荐）。

```
- SECURITY_OAUTH2_CLIENT_ACCESS_TOKEN_URI=${SECURITY_OAUTH2_CLIENT_ACCESS_TOKEN_URI}
- SECURITY_OAUTH2_CLIENT_USER_AUTHORIZATION_URI=${SECURITY_OAUTH2_CLIENT_USER_AUTHORIZATION_URI}
- SECURITY_OAUTH2_CLIENT_CLIENT_ID=${SECURITY_OAUTH2_CLIENT_CLIENT_ID}
- SECURITY_OAUTH2_CLIENT_CLIENT_SECRET=${SECURITY_OAUTH2_CLIENT_CLIENT_SECRET}
- SECURITY_OAUTH2_RESOURCE_USER_INFO_URI=${SECURITY_OAUTH2_RESOURCE_USER_INFO_URI}
```

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)您还可以将这些变量放在文件中并指定```env_file```设置。 请参阅Compose中的环境变量以了解更多信息。

然后你需要重新启动你的JHipster registry（如果它已经运行）：

```
docker-compose -f gateway/src/main/docker/jhipster-registry.yml restart
```

须登录，您需要在Okta应用程序中添加http://localhost:8761/login作为登录重定向URI。

## 运行您的微服务架构

打开三个终端窗口并导航到每个应用程序（```gateway```，```blog```和```store```）。 在每个窗口中，运行Maven以启动每个应用程序：

```
./mvnw
```

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)如果已安装Maven，可以仅运行mvn。

打开你的浏览器然后定向至[http://localhost:8761](http://localhost:8761/)。登录，随后你将看到欢迎页并显示网关和两个已注册应用程序。

![图39.JHipster Registry与网关注册](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPvxkoLqQrd4-DSk%2F39.jpg?alt=media&token=ba200a01-c25d-413f-9970-ac83a571fa51)

图39.JHipster Registry与网关注册

一切运行后，打开浏览器，转到http://localhost:8080，然后单击登录。你将被重定向到您的Okta租户登录，然后在您输入有效凭证信息后返回网关证书。

![图40.欢迎，JHipster](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPw496PS1Zv5fNoL%2F40.jpg?alt=media&token=ad7dc3bd-52eb-4780-9592-b627bad43ae1)

图40.欢迎，JHipster

![图41.Okta 登陆页面](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPw7IcR_OSOCjnb8%2F41.jpg?alt=media&token=1c51374a-8673-4024-a9fd-29069c31e236)

图41.Okta 登陆页面

![图42.Okta单点登陆后的JHipster](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPwA-L7ne0l1iAmh%2F42.jpg?alt=media&token=d0fd460d-2750-4d3c-bd2e-657043415437)

图42.Okta单点登陆后的JHipster

您应该能够导航到实体>博客并将新的博客记录添加到您的博客微服务中。

![图43.新建博客](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPwDGBzVyGS2FeGs%2F43.jpg?alt=media&token=c65354e7-011a-491f-a062-7920b1260caa)

图43.新建博客

导航到实体>产品以证明您的产品微服务正在运行。 由于您将图像添加为属性，因此在创建新记录时会提示您上传图像。

![图44.添加商品页](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPwH14RMbLFpIEiR%2F44.jpg?alt=media&token=45f23e69-db22-4cd5-98e6-70461bad053c)

图44.添加商品页

单击“保存”，您将根据生成的ID知道它正确使用MongoDB。

![图45.新建商品](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPwKX6GCJpWIECHM%2F45.jpg?alt=media&token=c228ca62-34c0-4717-a7f3-d0ad1f69b33e)

图45.新建商品

### 使用Docker Compose运行一切

您可以使用Docker Compose一次性启动所有服务，而不是单独启动所有服务。 要了解有关Docker Compose的更多信息，请参阅“A Developer’s Guide to Docker Compose”。

在根目录（```jhipster-microservices-example```）中创建```docker-compose```目录并运行JHipster的Docker Compose子生成器。

```
mkdir docker-compose
cd docker-compose
jhipster docker-compose
```

提示时回答如下：

| **提问**               | **回答**                         |
| ---------------------- | -------------------------------- |
| 应用程序类型？         | ```Microservice application```   |
| 网关类型？             | ```JHipster gateway```           |
| 目录位置？             | ```../```                        |
| 应用包含？             | ```<select all>```               |
| 集群数据库的应用程序？ | ```<blank>```                    |
| 设置监控？             | ```可以，通过JHipster Console``` |
| 其它技术？             | ```Zipkin```                     |
| 超级用户密码           | ```<choose your own>```          |

在```blog```，```gateway```和```store```目录中运行以下命令来生成Docker镜像时您将收到警告。构建Docker镜像前停止所有正在运行的进程。

```
./mvnw package -Pprod jib:dockerBuild
```

在等待构建的过程中，编辑```docker-compose/docker-compose.yml```并将Spring Security设置从硬编码更改为环境变量。对所有应用程序进行更改，并确保添加客户端ID和密码，因为默认情况下不包括这些更改。

```
services:
    blog-app:
        image: blog
        environment:
            - SECURITY_OAUTH2_CLIENT_CLIENT_ID=${SECURITY_OAUTH2_CLIENT_CLIENT_ID}
            - SECURITY_OAUTH2_CLIENT_CLIENT_SECRET=${SECURITY_OAUTH2_CLIENT_CLIENT_SECRET}
            - SECURITY_OAUTH2_RESOURCE_USER_INFO_URI=${SECURITY_OAUTH2_RESOURCE_USER_INFO_URI}
    ...
    gateway-app:
        image: gateway
        environment:
            - SECURITY_OAUTH2_CLIENT_ACCESS_TOKEN_URI=${SECURITY_OAUTH2_CLIENT_ACCESS_TOKEN_URI}
            - SECURITY_OAUTH2_CLIENT_USER_AUTHORIZATION_URI=${SECURITY_OAUTH2_CLIENT_USER_AUTHORIZATION_URI}
            - SECURITY_OAUTH2_CLIENT_CLIENT_ID=${SECURITY_OAUTH2_CLIENT_CLIENT_ID}
            - SECURITY_OAUTH2_CLIENT_CLIENT_SECRET=${SECURITY_OAUTH2_CLIENT_CLIENT_SECRET}
            - SECURITY_OAUTH2_CLIENT_SCOPE=openid profile email
            - SECURITY_OAUTH2_RESOURCE_USER_INFO_URI=${SECURITY_OAUTH2_RESOURCE_USER_INFO_URI}
    ....
    store-app:
        image: store
        environment:
            - SECURITY_OAUTH2_CLIENT_CLIENT_ID=${SECURITY_OAUTH2_CLIENT_CLIENT_ID}
            - SECURITY_OAUTH2_CLIENT_CLIENT_SECRET=${SECURITY_OAUTH2_CLIENT_CLIENT_SECRET}
            - SECURITY_OAUTH2_RESOURCE_USER_INFO_URI=${SECURITY_OAUTH2_RESOURCE_USER_INFO_URI}
```

您可以从```docker-compose/docker-compose.yml```中删除Keycloak，因为它不会与此配置一起使用。

```
keycloak:
    extends:
        file: keycloak.yml
        service: keycloak
```

您还需要编辑```docker-compose/jhipster-registry.yml```。

```
services:
    jhipster-registry:
        ...
        environment:
            - SECURITY_OAUTH2_CLIENT_ACCESS_TOKEN_URI=${SECURITY_OAUTH2_CLIENT_ACCESS_TOKEN_URI}
            - SECURITY_OAUTH2_CLIENT_USER_AUTHORIZATION_URI=${SECURITY_OAUTH2_CLIENT_USER_AUTHORIZATION_URI}
            - SECURITY_OAUTH2_CLIENT_CLIENT_ID=${SECURITY_OAUTH2_CLIENT_CLIENT_ID}
            - SECURITY_OAUTH2_CLIENT_CLIENT_SECRET=${SECURITY_OAUTH2_CLIENT_CLIENT_SECRET}
            - SECURITY_OAUTH2_RESOURCE_USER_INFO_URI=${SECURITY_OAUTH2_RESOURCE_USER_INFO_URI}
```

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)您可以运行```docker-compose config```来验证环境变量是否被正确替换。

当一切都完成构建后，从```docker-compose```目录运行```docker-compose up -d```。 启动所有14个容器可能需要一段时间，所以现在是休息或锻炼身体的好时机。您可以使用Docker的Kitematic在镜像开始时观察它的状态。

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)在开始所有操作之前，请确保为Docker提供了足够的CPU和内存。 它默认为一个CPU和2 GB内存 - 对于14个容器来说还不够！

![图46.新建产品](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPwNWXytqXfDfjSY%2F46.jpg?alt=media&token=457be809-4461-4cb2-b381-28f64ee0b46b)

图46.新建产品

在验证一切正常后，可以使用以下命令停止所有Docker容器：

```
docker stop $(docker ps -a -q)
```

如果您还想删除镜像，可以运行：

```
docker rm $(docker ps -a -q)
```

## 部署到Heroku

JHipster的创始人Julien Dubois在Heroku博客上撰写了一篇博文，名为“Bootstrapping Your Microservices Architecture with JHipster and Spring”。这是将所有应用程序部署到Heroku的一组简略步骤。

### 部署JHipster Registry

Heroku和JHipster为你配置了一个JHipster Registry，所以你只需要点击下面的按钮开始你自己的JHipster注册表：

![Deploy to Heroku](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPwyF7fyvypx3YSI%2Fdeploytoheroku.jpg?alt=media&token=ad504a38-553a-42ef-bd3f-cadaed150b39)

输入应用程序名称（我使用```okta-jhipster-registry```），添加```JHIPSTER_PASSWORD```，然后单击**Deploy app**

### 在Heroku上部署你的网关和应用

在每个项目中，运行```jhipster heroku```并回答如下问题：

| **提问**                | **回答**                                                     |
| ----------------------- | ------------------------------------------------------------ |
| 要部署的名称？          | ```<独特前缀>;-<应用名>```(e.g., okta-gateway,okta-blog, etc.) |
| 哪个地区？              | ```us```                                                     |
| 部署类型？              | ```Git```                                                    |
| 注册应用名？            | ```<独特前缀>-jhipster-registry```                           |
| JHipster Registry用户名 | ```admin```                                                  |
| JHipster Registry密码   | ```<JHIPSTER_PASSWORD from Registry>```                      |

当提示您覆盖文件时，键入```a```。

每个应用程序完成部署后，您将需要运行以下内容，以便他们使用Okta进行身份验证。

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)确保在运行以下命令之前运行```source ~/.okta.env```。 如果您没有预先设置环境变量，您将看到一个错误，```did not find property 'jhipster.security.client-authorization.client-id'```。

```
heroku config:set \
    SECURITY_OAUTH2_CLIENT_ACCESS_TOKEN_URI="$SECURITY_OAUTH2_CLIENT_ACCESS_TOKEN_URI" \
    SECURITY_OAUTH2_CLIENT_USER_AUTHORIZATION_URI="$SECURITY_OAUTH2_CLIENT_USER_AUTHORIZATION_URI" \
    SECURITY_OAUTH2_RESOURCE_USER_INFO_URI="$SECURITY_OAUTH2_RESOURCE_USER_INFO_URI" \
    SECURITY_OAUTH2_CLIENT_CLIENT_ID="$SECURITY_OAUTH2_CLIENT_CLIENT_ID" \
    SECURITY_OAUTH2_CLIENT_CLIENT_SECRET="$SECURITY_OAUTH2_CLIENT_CLIENT_SECRET"
```

更新您的Okta应用程序以获得与您的Heroku应用程序匹配的登录重定向URI（例如，https://oktagateway.herokuapp.com/login）。 要执行此操作，请登录到您的Okta帐户，转到“应用程序”>“JHipster微服务”>“常规”>“编辑”。

要查看您的应用是否已正确启动，您可以在每个应用的目录中运行```heroku logs --tail```。 您可能会看到超时错误，但您的应用应该会在下次尝试时成功启动。

如果它崩溃并且没有启动，请尝试运行```heroku restart```。 如果这不能解决问题，请转到https://help.heroku.com并单击顶部的创建故障单。 单击“运行应用程序”>“Java”，滚动到底部，然后单击“创建票证”。 为主题和说明输入以下内容，选择您的某个应用，然后提交。

```
Subject: JHipster App Startup Timeout

Description: Hello, I have a JHipster (Spring Boot) app that has the following error on startup:

Error R10 (Boot timeout) -> Web process failed to bind to $PORT within 90 seconds of launch

The URL is:

* https://<your-app>-store.herokuapp.com/

Can you please increase the timeout on this app?

Thanks!
```

下面是我部署到Heroku后证明一切正常的截图。

![图47.在Heroku上的JHipster Registry](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPwSorxRKXhvolY0%2F47.jpg?alt=media&token=023e00ee-45ef-4f07-aaa6-daa91a2598ca)

图47.在Heroku上的JHipster Registry

![图48.在Heroku上成功登录](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPwa4ogsUs_St2E-%2F48.jpg?alt=media&token=aee8f971-43fc-477c-b9a3-9bd242410ba6)

图48.在Heroku上成功登录

![图49.Heroku网关路由](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPwd0wjjEO0tXphO%2F49.jpg?alt=media&token=f45a7054-d489-42fc-9a86-0be95ef82577)

图49.Heroku网关路由

![图50.Heroku上的博客应用](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPwgx-Uy4Zw1h3ON%2F50.jpg?alt=media&token=bf8e9dbd-383a-43c3-a583-7cff84e4972a)

图50.Heroku上的博客应用

![图51.Heroku上的商店](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPwmvFANplrB2sdT%2F51.jpg?alt=media&token=1e62771e-02b4-4f54-8da4-151416b32ee5)

图51.Heroku上的商店

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)如果您有兴趣使用Kubernetes部署到Google Cloud，您可能会喜欢我的博客文章和关于如何“Build JHipster Microservices and Deploy to Google Cloud with Kubernetes”的截屏视频。

## 源码

您可以在[https://github.com/oktadeveloper/oktajhipster-microservices-oauth-example](https://github.com/oktadeveloper/oktajhipster-microservices-oauth-example)找到此微服务示例的源代码。

## 总结

我希望你喜欢这个如何用JHipster创建微服务架构的旋风之旅。仅仅因为JHipster使微服务变得简单并不意味着你应该使用它们。 使用微服务架构是扩展开发团队的一种很好的方法，但是如果你没有庞大的团队，那么单体应用（Majestic Monolith）可能会更好。

如果您想了解有关微服务，身份验证和JHipster的更多信息，请参阅以下资源。

* “Build a Microservices Architecture for Microbrews with Spring Boot”
* “Secure a Spring Microservices Architecture with Spring Security and OAuth”
* “Develop and Deploy Microservices with JHipster” (uses JWT for authentication)
* “Use OpenID Connect Support with JHipster”
* “Hello Istio, welcome to JHipster”

