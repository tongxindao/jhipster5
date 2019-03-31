# 第二部分 JHipster的UI组件

现代Web应用程序包含大量UI组件。它可能有某种模型-视图-控制器（MVC）框架以及CSS框架，以及简化这些框架使用的工具。使用Web应用程序，您可以在全球范围内拥有用户，因此将应用程序翻译成其他语言可能非常重要。如果你正在开发大量的CSS，你可能会使用像CSS或Sass这样的CSS预处理器。然后，您将需要一个构建工具来刷新浏览器，运行预处理器，运行测试，缩小Web资源，并准备应用于生产环境。

本节介绍JHipster如何为您提供所有这些UI组件，并使您产生愉快的开发体验。

## Angular

JHipster支持两个UI框架：Angular和React。在本书的前两个版本中，我向您展示了如何使用AngularJS。在最近两个版本（4.0和4.5）中，我向您展示了如何使用Angular。由于这是一本迷你书，我将坚持只展示Angular。从下图中可以看出，Angular和React是JavaScript框架中最受欢迎的。

![图19.2018年9月 Indeed上职位招聘数](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPusDoGr3Hsj-z4E%2F19.jpg?alt=media&token=88b25b7f-9c61-46af-8401-b1dc391e4692)

图19.2018年9月 Indeed上职位招聘数

![图20.2018年9月 Stack Overflow 标签数](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPuvo55U5jmTBqDG%2F20.jpg?alt=media&token=d8889505-e623-479b-8e19-f9c953068408)

图20.2018年9月 Stack Overflow 标签数

Angular是JHipster使用的默认UI框架。它是用TypeScript编写的，编译成JavaScript，使用它将让你成为开发潮客！就像21世纪初的Struts和2000年代中期的Rails一样，Angular和其他JavaScript框架改变了开发人员编写Web应用程序的方式。 今天，数据通过REST API对外暴露，UI用JavaScript（或TypeScript）编写。 作为一名Java开发人员，当我看到它关注的分离时，我立即被Angular所吸引。它建议将应用程序组织成几个不同的组件：

* 组件：从服务检索数据并将其公开给模板的类。
* 服务：对JSON API进行HTTP调用的类。
* 模板：显示数据的HTML页面。使用Angular指令迭代集合并显示/隐藏元素。
* 管道：可以转换数据的数据操作工具（例如大写，小写，排序和搜索）。
* 指令：允许编写组件的HTML处理器。与JSP标签类似。

### 历史

AngularJS由Miško Hevery在2009年创立。当时他正在开发一个使用GWT的项目。三个开发人员已经开发了六个月的产品，Miško在三周内重写了AngularJS中的所有内容。那时，AngularJS是他创建的一个副项目。它不需要你在JavaScript中编写太多，因为你可以用HTML编写大部分逻辑。该产品的GWT版本包含17,000行代码。 AngularJS版本只有1000行代码！

2014年10月，AngularJS团队宣布他们正在构建Angular 2.0。该公告引发了Angular开发者社区的一些动荡。用于编写Angular应用程序的API将发生变化，它将基于一种新语言AtScript。没有迁移路径，用户必须继续使用1.x或重写其应用程序2.x。

引入的新语法，它将数据绑定到元素属性（element properties），而不是属性（attributes）。这种语法的优点是它允许您在Angular应用程序中使用任何Web组件，而不仅仅是那些用于Angular的改进。

```
<input type="text" [value]="firstName">
<button (click)="addPerson()">Add</button>
<input type="checkbox" [checked]="someProperty">
```

2015年3月，Angular团队解决了社区问题，宣布他们将使用TypeScript而不是AtScript，并且他们将为Angular 1.x用户提供迁移路径。他们还采用了语义版本控制，并建议人们将其称为“Angular”而不是 Angular 2.0。

Angular 2.0于2016年9月发布。Angular 4.0于2017年3月发布。JHipster 4.6.0于2017年7月6日发布，是第一个包含生产就绪Angular支持的版本。 JHipster 5在本书的制作过程中使用了Angular 6. 同一时期Angular发布其第7版。

你可在https://angular.io中找到Angular项目。

### 基本语法

使用Angular创建一个说“Hello World”的组件非常简单。

```
import {
    Component
}
from '@angular/core';

@Component({
    selector: 'my-app',
    template: ` < h1 > Hello {{name}} < /h1>`
})

export class AppComponent {
    name = 'World';
}
```

在上述示例中，组件中的```name```变量映射到```{{name}}```中并显示变量的值。要在页面上渲染此组件，您需要更多文件：模块定义，引导类和HTML文件。基本模块定义包含组件声明（declarations），导入（imports），供应商（providers）和要引导的类（bootstrap）。

```
import {
    BrowserModule
}
from '@angular/platform-browser';

import {
    NgModule
}
from '@angular/core';

import {
    AppComponent
}
from './app.component';

@NgModule({
    declarations: [
        AppComponent
    ],
    imports: [
        BrowserModule
    ],
    providers: [],
    bootstrap: [AppComponent]
})

export class AppModule {}
```

然后你需要一个通常命名为```main.ts```的文件来引导模块。

```
import {
    enableProdMode
}
from '@angular/core';

import {
    platformBrowserDynamic
}
from '@angular/platform-browser-dynamic';

import {
    AppModule
}
from './app/app.module';

import {
    environment
}
from './environments/environment';

if (environment.production) {
    enableProdMode();
}

platformBrowserDynamic().bootstrapModule(AppModule)
    .catch(err = >console.error(err));
```

最后，您需要一个基本的HTML文件渲染组件。

```
<html>
    <head>
        <title>Howdy</title>
    </head>
    <body>
        <my-app></my-app>
        <script src="path/to/compiled/javascript.js"></script>
    </body>
</html>
```

MVC模式是Web框架实现的常见模式。 在Angular中，模型（model）由您从服务创建或取回的对象表示。视图（view）是HTML模板，组件是一个类，用于设置模板要读取的变量。

![图21.Angular MVC模型](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPuyRYOGeXa4HL4O%2F21.jpg?alt=media&token=a294c2f1-6bae-42c3-ab5c-81c5f8834060)

图21.Angular MVC模型

以下代码是用于获取搜索结果的```SearchService```。预计在同一服务器上的```/api/search```中存在JSON端点（endpoint）。

*SearchService*

```
import {
    Injectable
}
from '@angular/core';

import {
    HttpClient
}
from '@angular/common/http';

import {
    Observable
}
from 'rxjs';

@Injectable({
    providedIn: 'root'
})

export class SearchService {
    constructor(private http: HttpClient) {}
    search(term): Observable <any> {
        return this.http.get(`/api/search/${term}`);
    }
}
```

关联的```SearchComponent```可用于显示此服务的结果。请注意如何使用构造函数注入来获取对服务的引用。

*SearchComponent*

```
import {
    Component
}
from '@angular/core';

import {
    SearchService
}
from '../search.service';

@Component({
    selector: 'app-search',
    templateUrl: './search.component.html',
    styleUrls: ['./search.component.css']
})
export class SearchComponent {
    query: string;
    searchResults: Array < any > ;
    constructor(private searchService: SearchService) {
        console.log('In Search Component...');
    }

    search(): void {
        this.searchService.search(this.query).subscribe(
        data => {
            this.searchResults = data;
        },
        error => console.log(error)
        );
    }
}
```

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)要在Chrome中查看JavaScript控制台，请在Mac OS X/macOS中使用```Command+Option+J```。Windows或Linux中使用```Control+Shift+J```。

要使此组件在URL上可用，您可以使用Angular的路由（```Router```）并在模块中指定包含该组件的路径。

*AppModule*

```
import {
    BrowserModule
}
from '@angular/platform-browser';

import {
    NgModule
}
from '@angular/core';

import {
    FormsModule
}
from '@angular/forms';

import {
    HttpModule
}
from '@angular/http';

import {
    AppComponent
}
from './app.component';

import {
    SearchComponent
}
from './search/search.component';

import {
    Routes,
    RouterModule
}
from '@angular/router';

const appRoutes: Routes = [
    { path: 'search', component: SearchComponent },
    { path: '', redirectTo: '/search', pathMatch: 'full'}
];

@NgModule({
    declarations: [
        AppComponent,
        SearchComponent
    ],
    imports: [
        BrowserModule,
        FormsModule,
        HttpModule,
        RouterModule.forRoot(appRoutes)
    ],
    providers: [],
    bootstrap: [AppComponent]
})
export class AppModule {}
```

在应用程序的主入口点，您需要在模板中指定```AppComponent```的路由出口。

*app.component.html*

```
<router-outlet></router-outlet>
```

```SearchComponent```的模板可以像带有按钮的表单一样简单。

```
<h2>Search</h2>
<form>
    <input type="search" name="query" [(ngModel)]="query" (keyup.enter)="search()">
    <button type="button" (click)="search()">Search</button>
</form>
```

现在您已经看过代码了，让我们看一下```SearchComponent```中的一切是如何工作的。

```
@Component({
    selector: 'app-search',
    templateUrl: './search.component.html',
    styleUrls: ['./search.component.css']
})
export class SearchComponent {
    query: string;
    searchResults: Array <any> ;

    constructor(private searchService: SearchService) {①
        console.log('In Search Component...');
    }

    search() : void {②
        this.searchService.search(this.query).subscribe(③
        data => {
            this.searchResults = data;
        },
        error => console.log(error)
        );
    }
}
```

①要将```SearchService```注入到```SearchComponent```，只需将其作为参数添加到组件的构造函数中。TypeScript自动使构造函数依赖项可用作类变量。
②```search()```是从HTML的```<input>```和```<button>```调用的方法，使用```(keyup.enter)```和```(click)```事件处理程序将其连接起来。
③```this.query```是一个使用```[(ngModel)]```指令连接到```<input>```的变量。 这种语法提供了双向绑定，因此如果您更改组件中的值，它会在渲染中的HTML中发生改变。您可以这样想：```[] ⇒ 组件到模板```和```() ⇒ 模板到组件```。

要使上述代码有效，您可以使用Angular CLI生成新的Angular项目。 要安装Angular CLI，您可以使用npm。

```
npm i -g @angular/cli
```

然后使用```ng new```生成一个新的应用程序。当提示安装Angular路由时，键入“Y”。对于样式表格式，请选择“CSS”（默认值）。

```
ng new ng-demo
```

这将创建基本应用程序所需的所有文件，安装依赖项，并设置构建系统以将TypeScript代码编译为JavaScript。

您可以使用```ng generate service search```（或```ng g s search```）生成```SearchService```。

```
$ ng g s search
CREATE src/app/search.service.spec.ts (333 bytes)
CREATE src/app/search.service.ts (135 bytes)
```

您可以使用```ng generate component search```（或```ng g c search```）生成```SearchComponent```。

```
$ ng g c search
CREATE src/app/search/search.component.css (0 bytes)
CREATE src/app/search/search.component.html (25 bytes)
CREATE src/app/search/search.component.spec.ts (628 bytes)
CREATE src/app/search/search.component.ts (269 bytes)
UPDATE src/app/app.module.ts (396 bytes)
```

您的API是否返回如下数据？

```
[
        {
                "id": 1, 
                "name": "Peyton Manning", 
                "phone": "(303) 567-8910", 
                "address": {
                        "street": "1234 Main Street", 
                        "city": "Greenwood Village", 
                        "state": "CO", 
                        "zip": "80111"
                }
        }, 
        {
                "id": 2, 
                "name": "Demaryius Thomas", 
                "phone": "(720) 213-9876", 
                "address": {
                        "street": "5555 Marion Street", 
                        "city": "Denver", 
                        "state": "CO", 
                        "zip": "80202"
                }
        }, 
        {
                "id": 3, 
                "name": "Von Miller", 
                "phone": "(917) 323-2333", 
                "address": {
                        "street": "14 Mountain Way", 
                        "city": "Vail", 
                        "state": "CO", 
                        "zip": "81657"
                }
        }
]
```

如果是这样，您可以使用Angular的```*ngFor```指令在```search.component.html```模板中显示它。

```
<table *ngIf="searchResults">
    <thead>
    <tr>
        <th>Name</th>
        <th>Phone</th>
        <th>Address</th>
    </tr>
    </thead>
    <tbody>
    <tr *ngFor="let person of searchResults; let i=index">
        <td><a [routerLink]="['/edit', person.id]">{{person.name}}</a></td>
        <td>{{person.phone}}</td>
        <td>{{person.address.street}}<br/>
            {{person.address.city}}, {{person.address.state}} {{person.address.zip}}
        </td>
    </tr>
    </tbody>
</table>
```

想阅读使用Angular构建搜索/编辑应用程序的更深入的示例（包括源代码和测试），请参阅我的Angular和Angular CLI教程。

既然您已经了解了地球上最热门的Web框架之一，那么让我们再来看看最流行的CSS框架：Bootstrap。

## Bootstrap

Bootstrap是一个简化Web应用程序开发的CSS框架。它提供了许多CSS类和HTML结构，允许您开发默认情况下看起来很好的HTML用户界面。不仅如此，它默认是响应式的，这意味着它在移动设备上运行得很好（甚至更好）。

### Bootstrap的网格系统

大多数CSS框架都提供了一个网格系统，允许您以可敬的方式定位列和单元格。Bootstrap强大的网格使用容器，行和列构建。它基于CSS3 flexible box或flexbox。Flexbox是一种布局模式，旨在适应不同的屏幕尺寸和不同的显示设备。它比使用块更容易进行布局，因为它不使用浮动，灵活容器的边距也不会因其内容的边距而崩溃。CSS-Tricks有一个Flexbox完整指南，可以很好地解释它的概念。

> flex布局背后的主要思想是让容器能够改变其项目的宽度/高度（和顺序）以最好地填充可用空间，主要是为了适应各种显示设备和屏幕尺寸。Flex容器扩展项目以填充可用空间或缩小它们以防止溢出。

Bootstrap的网格宽度根据视口（viewport）宽度而变化。下表显示了网格系统的各个方面如何在不同设备上工作。

|                  | **超小**    | **小**         | **中**         | **大**        | **超大**      |
| ---------------- | ----------- | -------------- | -------------- | ------------- | ------------- |
| **最大容器宽度** | None(auto)  | 540px          | 720px          | 960px         | 1140px        |
| **类前缀**       | ```.col-``` | ```.col-sm-``` | ```.col-md-``` | ```.col-lg``` | ```.col-xl``` |
| **列数据**   | 12               |
| **间距宽度** | 30px（每侧15px） |
| **嵌套**     | 是               |
| **列排序**   | 是               |

网格的基本示例如下所示。

```
<div class="row">
    <div class="col-md-3">.col-md-3 <!-- 3 列在左 --></div>
    <div class="col-md-9">.col-md-9 <!-- 9 列在右 --></div>
</div>
```

使用Bootstrap的CSS渲染时，上面的HTML在桌面设备上看起来如下。桌面设备上容器元素的最小宽度设置为1200像素。

![图22.在桌面设备上基本的栅格布局](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPv0hJc231FtwX2n%2F22.jpg?alt=media&token=af623fd2-9103-4d79-9800-2f6fc6ee3aec)

图22.在桌面设备上基本的栅格布局

如果您将浏览器压缩到宽度小于1200像素或在较小的屏幕上渲染相同的文档，则列将被堆叠。

![图23.在移动设备上基本栅格布局](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPv3ohhtY5NlvQId%2F23.jpg?alt=media&token=6df15e50-41fe-4e22-bcc2-a3f0c3e16327)

图23.在移动设备上基本栅格布局

Bootstrap的网格可用于对齐和定位应用程序的元素，小部件和特性（features）。如果想要有效地使用它须理解以下一些基础知识。

* 它基于12列。
* 只需使用“md”类并根据需要进行修复（fix）。
* 它可用于调整输入字段的大小。

Bootstrap的网格系统有五层：xs（肖像电话），sm（风景电话），md（平板电脑），lg（桌面）和xl（大型桌面）。 您几乎可以使用这些类的任意组合来创建更加动态和灵活的布局。 下面是一个更高级的网格示例。

每层类都会向上扩展，这意味着如果您计划为xs和sm设置相同的宽度，则只需指定xs。

```
<div class="row">
    <div class="col-xs-12 col-md-8">.col-xs-12 .col-md-8</div>
    <div class="col-xs-6 col-md-4">.col-xs-6 .col-md-4</div>
</div>
<div class="row">
    <div class="col-xs-6 col-md-4">.col-xs-6 .col-md-4</div>
    <div class="col-xs-6 col-md-4">.col-xs-6 .col-md-4</div>
    <div class="col-xs-6 col-md-4">.col-xs-6 .col-md-4</div>
</div>
<div class="row">
    <div class="col-xs-6">.col-xs-6</div>
    <div class="col-xs-6">.col-xs-6</div>
</div>
```

![图24.高级栅格](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPv7ho0TleeZU3py%2F24.jpg?alt=media&token=562c8955-83e7-4d63-8313-30e4c84bde07)

图24.高级栅格

您可以使用大小指示符指定列中的断点。断点表示列包装到下一行的位置。在上面的HTML中，“xs”和“md”是大小指示符（当然，“sm”，“lg”和“xl”是其他选项）。 Bootstrap使用以下media查询范围。

```
// 超小型设备（人像手机，小于576像素）
// 没有media查询，因为这是Bootstrap中的默认值

// 小型设备（风景电话，576像素及以上）
@media（min-width：576px）{...}

// 中型设备（平板电脑，768像素及以上）
@media（min-width：768px）{...}

// 大型设备（台式机，992像素及以上）
@media（min-width：992px）{...}

// 超大型设备（大型桌面，1200像素及以上）
@media (min-width: 1200px) { ... }
```

如果你正在使用Sass，所有Bootstrap的media查询都可以通过Sass mixins获得：

```
// 没有xs断点所需的media查询，因为它实际上是`@media（min-width：0）{...}`
@include media - breakpoint - up（sm） {...}
@include media - breakpoint - up（md） {...}
@include media - breakpoint - up（lg） {...}
@include media - breakpoint - up（xl） {...}

// 示例：从`min-width：0`开始隐藏，然后在`sm`断点处显示
.custom - class {
    display：none;
}
@include media - breakpoint - up（sm） {
    .custom - class {
        Display: block;
    }
}
```

### 响应式实用程序类

Bootstrap还包括许多实用程序类，可用于根据浏览器大小显示和隐藏元素，如```.d-[xs|sm|md|lg]-block```和```.d-[xs|sm|md|lg]-none```。没有明确的“show”响应实用程序类；只要不让其隐藏在该断点尺寸即可使元素可见。

Bootstrap用于设置显示的类是使用以下格式的名称：

* ```.d-{value}```表示```xs```
* ```.d-{breakpoint}-{value}```表示```sm```，```md```，```lg```，和```xl```

media查询使用给定断点或更大的效果影响屏幕宽度。 例如，```.d-lg-none```在```lg```和```xl```屏幕上都设置```display: none```。

以下来自21-Points Health的示例显示了如何在移动设备上显示较短的标题，在较大的屏幕上显示较大的标题。

```
<div class="col-6 text-nowrap">
  <h4 class="mt-1 d-none d-md-inline"
      jhiTranslate="home.bloodPressure.title"> Blood Pressure:</h4>
  <h4 class="mt-1 d-sm-none"
      jhiTranslate="home.bloodPressure.titleMobile">BP:</h4>
</div>
```

### 表单

当您将Bootstrap的CSS添加到Web应用程序时，外观看起来会变得更好。默认情况下，排版，边距和填充看起来会更好。 但是，您的表单可能看起来很有趣，因为Bootstrap需要在表单元素上使用几个类才能使它们看起来很好。下面是表单元素的示例。

```
<div class="form-group">
    <label for="description">Description</label>
    <textarea class="form-control" rows="4" name="description" id="description"></textarea>
</div>
```

![图25.基本的表单元素](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPvAJvXT6PRQuLIi%2F25.jpg?alt=media&token=2323815a-c14f-4324-bc27-261f4cb7c920)

图25.基本的表单元素

如果您想表明此表单元素无效，则需要修改上述HTML以显示验证警告。

```
<div class="form-group">
    <label for="description" class="control-label">Description</label>
    <textarea class="form-control is-invalid" rows="4" id="description" required></textarea>
    <span class="invalid-feedback">Description is a required field.</span>
</div>
```

![图26.带验证的表单元素](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPvDXb2lMLUDHV1_%2F26.jpg?alt=media&token=9cb9fa22-51b9-46fa-8051-1faa9eaeaa1d)

图26.带验证的表单元素

### CSS（层叠样式表）

将Bootstrap的CSS添加到HTML页面时，默认设置会立即改进排版。 您的<h1>和<h2>标题会变为半粗体，并相应地调整大小。 您的段落边距，正文和块引号看起来会更好。 如果要对齐页面中的文本，```text-[left|center|right]```是有用的类。 对于表，默认情况下，```table```类使它们具有更好的外观。

为了让你的按钮看起来更好，Bootstrap提供了```btn```和许多```btn-*```类。

```
<button type="button" class="btn btn-primary">Primary</button>
<button type="button" class="btn btn-secondary">Secondary</button>
<button type="button" class="btn btn-success">Success</button>
<button type="button" class="btn btn-danger">Danger</button>
<button type="button" class="btn btn-warning">Warning</button>
<button type="button" class="btn btn-info">Info</button>
<button type="button" class="btn btn-light">Light</button>
<button type="button" class="btn btn-dark">Dark</button>
<button type="button" class="btn btn-link">Link</button>
```

![图27.按钮](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPvGPUzoWvFYnw3a%2F27.jpg?alt=media&token=92795c69-f069-437d-becd-069c7643b8f6)

图27.按钮

### 组件

Bootstrap附带了许多组件。 有些需要JavaScript; 有些只需要HTML5标记和CSS类来工作。 其丰富的组件使其成为GitHub上最受欢迎的项目之一。 Web开发人员一直喜欢他们框架中的组件。提供易于使用的组件的框架通常允许开发人员编写更少的代码。 编写更少的代码意味着维护的代码更少！

流行的Bootstrap组件包括：下拉列表，按钮组，按钮下拉列表，导航栏，面包屑，分页，警报，进度条和面板。 以下是带下拉列表的导航栏示例。

![图28.带下拉列表的导航栏](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPvKL6EcAehfR7yW%2F28.jpg?alt=media&token=53929795-7074-4b9f-874f-914ffbf099de)

图28.带下拉列表的导航栏

当在移动设备上呈现时，所有内容都会折叠成可向下扩展的汉堡菜单。

![图29.移动设备下的导航栏](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPvMtc6eB__XylLC%2F29.jpg?alt=media&token=b88ad57b-0e94-45e8-a7ec-6c48f020ae82)

图29.移动设备下的导航栏

此导航栏需要相当多的HTML标记，为简洁起见，此处未显示。您可以在Bootstrap的文档中在线查看此源代码。下面的一个更简单的例子显示了它的基本结构。

```
<nav class="navbar navbar-expand-lg navbar-light bg-light">
    <a class="navbar-brand" href="#">Navbar</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent">
        <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav mr-auto">
            <li class="nav-item active">
                <a class="nav-link" href="#">Home</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="#">Link</a>
            </li>
            <li class="nav-item dropdown">
                <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown">
                    Dropdown
                </a>
                <div class="dropdown-menu" aria-labelledby="navbarDropdown">
                    <a class="dropdown-item" href="#">Action</a>
                    <a class="dropdown-item" href="#">Another action</a>
                    <div class="dropdown-divider"></div>
                    <a class="dropdown-item" href="#">Something else here</a>
                </div>
            </li>
            <li class="nav-item">
                <a class="nav-link disabled" href="#">Disabled</a>
            </li>
        </ul>
        <form class="form-inline">
            <input class="form-control" type="search" placeholder="Search">
            <button class="btn btn-outline-success" type="submit">Search</button>
        </form>
    </div>
</nav>
```

警报（alert）对于向用户显示反馈非常有用。 您可以使用不同的类调用不同颜色的警报。 你需要添加一个```alert```类，加上```alert-[success|info|warning|danger]```来表示颜色。

```
<div class="alert alert-primary" role="alert">
    This is a primary alert&mdash;check it out!
</div>
<div class="alert alert-secondary" role="alert">
    This is a secondary alert&mdash;check it out!
</div>
<div class="alert alert-success" role="alert">
    This is a success alert&mdash;check it out!
</div>
<div class="alert alert-danger" role="alert">
    This is a danger alert&mdash;check it out!
</div>
<div class="alert alert-warning" role="alert">
    This is a warning alert&mdash;check it out!
</div>
<div class="alert alert-info" role="alert">
    This is a info alert&mdash;check it out!
</div>
<div class="alert alert-light" role="alert">
    This is a light alert&mdash;check it out!
</div>
<div class="alert alert-dark" role="alert">
    This is a dark alert&mdash;check it out!
</div>
```

这会呈现如下告警。

![图30.警报](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPvPgyngCTHub-nk%2F30.jpg?alt=media&token=1f27b5e8-6b5b-4ef1-be46-2b763a3f3d75)

图30.警报

要使警报可关闭，您需要添加```.alert-dismissible```类和关闭按钮。

![图31.可关闭的警报](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPvSYw2fc8Zs3XvY%2F31.jpg?alt=media&token=a5035101-9736-4ea8-9694-666ab4f3e014)

图31.可关闭的警报

![标注](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx6ZThl3Rt16hD5%2Ftips.png?alt=media&token=fb300145-5e11-4393-9844-0f19274a52af)要使警报中的链接与警报的颜色匹配，请使用```.alert-link```。

### 图标（Icons）

图标一直是Web应用程序的重要组成部分。 向用户显示小图像通常比纯文本更性感和更时髦。 人类是视觉生物，图标是增添趣味的好方法。 在过去几年中，字体图标已经成为Web开发的流行。 字体图标只是字体，但它们包含符号和字形而不仅文本。 由于尺寸小，您可以快速设置，缩放和加载它们。

Bootstrap 3.x包括Glyphicons Halflings字体图标集。 它通常不是免费的，但字体创建者为Bootstrap用户免费提供。 Bootstrap 4的一个变化是它删除了Glyphicons图标字体。 您可以使用Font Awesome和GitHub Octicons作为Glyphicons的免费替代品。

Font Awesome有1,341个图标，默认包含在JHipster中，带有angular-fontawesome。 它通常显示带图标的按钮。

```
<button class="btn btn-info"><fa-icon [icon]="'plus'"></fa-icon> Add</button>
<button class="btn btn-danger"><fa-icon [icon]="'times'"></fa-icon> Delete</button>
<button class="btn btn-success"><fa-icon [icon]="'pencil-alt'"></fa-icon> Edit</button>
```

您可以根据为包含它们的元素定义的字体颜色来查看图标如何更改颜色。

![图32.带图标的按钮](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPvbIJ_Rf_9XJK0g%2F32.jpg?alt=media&token=fb39b071-5885-4b06-8c80-8fdaa79cfa19)

图32.带图标的按钮

### 自定义CSS

如果您想在项目中覆盖Bootstrap类，最简单的方法是将覆盖规则放在Bootstrap的CSS之后的CSS文件中。或者您可以直接修改```src/main/webapp/content/css/global.css```。如果您使用Sass编译```*.scss```文件，则可以修改```src/main/webapp/content/scss/global.scss```。使用Sass可以创建更简洁的创作环境。以下是JHipster生成的默认```vendor.scss```文件。你可以看到它导入了Bootstrap，我为AngularCalendar添加了一个导入。在```src/main/webapp/content/scss/global.scss```中重写了默认的Bootstrap规则。

*src/main/webapp/scss/vendor.scss*

```
/* 更改此文件后运行 'yarn run webpack：build' */

/***************************
将Sass变量放在这里：
例如$ input-color：red;
***************************/

// 日历样式
@import '~angular-calendar/scss/angular-calendar';
// 覆盖Boostrap变量
@import 'bootstrap-variables';
// 从node_modules导入Bootstrap源文件
@import '~bootstrap/scss/bootstrap';
/* jhipster-needle-scss-add-vendor JHipster将添加新的css样式 */
```

还有一个```src/main/webapp/content/scss/_bootstrap-variable.scss```文件。您可以修改此文件以更改默认的Bootstrap设置，如颜色，边框半径等。

### Angular和Bootstrap

JHipster默认包含ng-bootstrap。 该库提供了由Angular而不是jQuery驱动的Bootstrap 4组件。

Toastr是一个受jQuery支持的流行的非阻塞通知库。JHipster有一个```alert.service.ts```，允许您使用Toastr-like的警报。

要在项目中使用类似Toastr的警报，请将```shared-libs.module.ts```中的```alertAsToast```修改为```true```。

*src/main/webapp/app/shared/shared-libs.module.ts*

```
@NgModule({
    imports: [
        NgbModule.forRoot(),
        NgJhipsterModule.forRoot({
            // 将下面设置为true以使警报看起来像toast
            alertAsToast: true,
            i18nEnabled: true,
            defaultI18nLang: 'en'
        }),
        InfiniteScrollModule,
        CookieModule.forRoot(),
        FontAwesomeModule
    ],
    exports: [...]
})
```

Bootstrap的热门替代品包括Angular Material和Ionic Framework。目前不支持这些框架。要集成它们，需要重写所有模板以包含它们的类而不是Bootstrap。虽然可能，但创建和维护需要做很多工作。

## 国际化（i18n）

国际化（也称为i18n，因为单词在“i”和“n”之间有18个字母）是JHipster中的第一类公民。 如果您在项目开始时将i18n系统放置到位，则将应用程序翻译成另一种语言是最简单的。 ngx-translate提供的指令可以轻松地将您的应用程序翻译成多种语言。 你不会在你的JHipster项目的```package.json```中看到这种依赖，因为它包含在ng-jhipster项目中并包含在```jhiTranslate```指示中。

要在JHipster项目中使用i18n，只需的代码段中添加“jhiTranslate”属性即可。

```
<label for="username" jhiTranslate="global.form.username">Login</label>
```

该键引用了一个JSON文档，该文档将返回已翻译的字符串。然后Angular将用翻译版本替换“FirstName”字符串。

JHipster允许您在首次创建项目时选择默认语言和翻译。它在```src/main/webapp/i18n```中存储这些语言的JSON文档。您可以使用```yo jhipster: languages```安装其他语言。截至2018年10月，JHipster支持42种语言。您还可以添加了一种新语言。要设置默认语言，请修改```src/main/webapp/app/shared/sharedlibs.module.ts```及其```defaultI18nLang```设置即可。

*src/main/webapp/app/shared/shared-libs.module.ts*

```
imports: [
    ...
    NgJhipsterModule.forRoot({
        // 将下面设置为true以使警报看起来像toast
        alertAsToast: false,
        i18nEnabled: true,
        defaultI18nLang: 'en'
    }),
    ...
],
```

## Sass（语法上令人敬畏的样式表）

Sass代表“语法上令人敬畏（很棒）的样式表”。它是一种用CSS编写的语言，是使用现代编程语言中的好东西，例如变量，嵌套，mixins和继承。Sass使用```$```符号表示一个变量，然后可以在文档中稍后引用该变量。

```
$font-stack: Helvetica, sans-serif
$primary-color: #333

body
    font: 100% $font-stack
    color: $primary-color
```

Sass 3引入了一种称为SCSS的新语法，它与CSS3的语法完全兼容，同时仍然支持Sass的全部功能。它看起来更像CSS。

```
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
    font: 100% $font-stack;
    color: $primary-color;
}
```

上面的代码将呈现为以下CSS。

```
body {
    font: 100% Helvetica, sans-serif;
    color: #333;
}
```

Sass的另一个强大功能是能够编写嵌套的CSS选择器。 编写HTML时，您通常可以看到元素的层次结构。 Sass允许您将该层次结构引入CSS。

```
nav {
    ul {
        margin: 0;
        padding: 0;
        list - style: none;
    }

    li {
        display: inline - block;
    }

    a {
        display: block;
        padding: 6px 12px;
        text - decoration: none;
    }
}
```

![警告](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LauRN4EpK_MjXhOnPl7%2F-LauZgtqHq7W3A5MysML%2F-LauaPx8wFVf0uxw998r%2Fwarning.png?alt=media&token=5e68c3d7-37db-4a98-822f-de2e3f6534ad)过度的嵌套导致过度限定下的CSS难以维护。

如前所述，Sass还支持partials，import，mixins和inheritance。Mixins对于处理供应商前缀特别有用。

```
@mixin border-radius($radius) { ①
    -webkit-border-radius: $radius;
        -moz-border-radius: $radius;
            -ms-border-radius: $radius;
                    border-radius: $radius;
}
.box { @include border-radius(10px); } ②
```

①使用```@mixin```创建一个mixin并为其命名。使用```$radius```作为变量来设置半径值。
②使用```@include```后跟mixin的名称。

从上面的代码生成的CSS如下所示。

```
.box {
    -webkit-border-radius: 10px;
    -moz-border-radius: 10px;
    -ms-border-radius: 10px;
    border-radius: 10px;
}
```

Bootstrap 3.x是用Less编写的，这是一个与Sass具有类似功能的CSS预处理器。 它使用Sass 4.0+版本。

JHipster允许您默认使用Sass。 要了解有关构建CSS和命名类的更多信息，请阅读Scalable and Modular Architecture for CSS。

## Webpack（模块打包器）

JHipster 4+使用webpack构建客户端。 JHipster 3.x使用了Gulp。 Gulp允许您执行缩小，连接，编译等任务（例如从TypeScript / CoffeeScript到JavaScript），单元测试等等。 webpack是一种更现代的解决方案，在Angular项目中非常流行，并且包含在Angular CLI下。

webpack是一个模块捆绑器，它以应用程序所需的每个模块递归地构建依赖关系图。 它将所有这些模块打包成少量的捆绑包，由浏览器加载。 它的代码分割功能可以将大型JavaScript应用程序分解成可以按需加载的小块。


它有四个核心概念：

* Entry：这告诉webpack从何处开始并遵循依赖关系图以了解要捆绑的内容。
* Output：将所有资产捆绑在一起后，您需要告诉webpack将它们放在何处。
* Loaders：webpack将每个文件（.css，.scss，.ts，.png，.html等）视为一个模块，但只能理解JavaScript。 加载器在将文件添加到依赖关系图时将文件转换为模块。
* Plugins：对加载器的每个文件执行转换。插件在捆绑模块的块上执行操作和自定义。

下面是一个基本的```webpack.config.js```，它显示了所有正在使用的四个概念。

*webpack.config.js*

```
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过npm安装
const webpack = require('webpack'); // 访问内置插件
const path = require('path');

const config = {
    entry: './path/to/my/entry/file.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'my-first-webpack.bundle.js'
    },
    module: {
        rules: [
            { test: /\.txt$/,  use: 'raw-loader' }
        ]
    },
    plugins: [
        new webpack.optimize.UglifyJsPlugin(),
        new HtmlWebpackPlugin({template: './src/index.html'})
    ]
};

module.exports = config;
```

在Angular部分，我提到了Angular CLI和我的ng-demo项目，它展示了如何使用它.Angular CLI在内部使用webpack，但是你从未看到它，因为它包含了ng命令中的所有内容。 要查看其webpack配置，或根据需要调整它，可以通过运行```ng eject```将其从项目中弹出。

我尝试运行它，发现它为项目的```package.json```添加了20个依赖项，并生成了一个超过450行代码的```webpack.config.js```文件！这大致相当于JHipster项目的webpack配置中的代码行。JHipster生成一个```webpack```目录，其中包含不同场景的文件：具有热重新加载，测试和准备生产的开发模式。

* ```logo-jhipster.png```是用于构建通知的桌面警报中显示的徽标。
* ```utils.js```包含用于查找项目版本和确定外部的实用程序功能库。
* ```webpack.common.js```是由以下每个文件扩展的公共父配置。
* ```webpack.dev.js```是运行```yarn start```时使用的配置。 它支持使用Browsersync和对桌面通知进行热重新加载。
* ```webpack.prod.js```是用于生产的配置。 使用Angular的AOT编译，将HTML模板转换为JavaScript，并创建源映射。

要了解有关webpack的更多信息，我建议您访问https://webpack.academy，这是一个专门用于教授webpack的网站。 它由Webpack核心团队成员，Angular团队成员和Angular CLI核心团队的成员Sean Larkin创建。 他是webpack得以成功的一个重要原因，因为他不断推广它，提供学习材料，并帮助开源项目采用它。

## WebSockets（一种通过TCP工作的有状态通信协议）

WebSockets是一种先进的技术，可以在用户的浏览器和服务器之间打开交互式通信通道。 使用此API，您可以将消息发送到服务器并接收事件驱动的响应，而无需轮询服务器以进行回复。 WebSockets被称为“TCP for the Web”。

如果在创建JHipster项目时选择“使用Spring Websocket的WebSockets”作为“其他技术”选项的一部分，两个JavaScript库将被添加到您的项目中：

* webstomp-client：STOMP代表“简单的面向文本的消息传递协议”。
* SockJS：SockJS提供类似WebSocket的对象。 如果本机WebSocket不可用，它将回退到其他浏览器技术。

要查看WebSockets如何工作，请查看启用WebSockets的项目中的```JhiTrackerComponent```。这将显示已发布到```/websocket/tracker```端点的实时活动信息。

*src/main/webapp/app/admin/tracker/tracker.component.ts*

```
import {
    Component,
    OnInit,
    OnDestroy
}
from '@angular/core';

import {
    JhiTrackerService
}
from 'app/core';

@Component({
    selector: 'jhi-tracker',
    templateUrl: './tracker.component.html'
})

export class JhiTrackerComponent implements OnInit, OnDestroy {
    activities: any[] = [];

    constructor(private trackerService: JhiTrackerService) {}

    showActivity(activity: any) {
        let existingActivity = false;
        for (let index = 0; index < this.activities.length; index++) {
            if (this.activities[index].sessionId === activity.sessionId) {
                existingActivity = true;
                if (activity.page === 'logout') {
                    this.activities.splice(index, 1);
                } else {
                    this.activities[index] = activity;
                }
            }
        }
        if (!existingActivity && activity.page !== 'logout') {
            this.activities.push(activity);
        }
    }

    ngOnInit() {
        this.trackerService.subscribe();
        this.trackerService.receive().subscribe(activity = >{
            this.showActivity(activity);
        });
    }

    ngOnDestroy() {
        this.trackerService.unsubscribe();
    }
}
```

```Tracker```服务允许您发送跟踪信息。例如，跟踪某人是否已通过身份验证。

*src/main/webapp/app/core/tracker/tracker.service.ts*

```
import {
    Injectable
}
from '@angular/core';

import {
    Router,
    NavigationEnd
}
from '@angular/router';

import {
    Observable,
    Observer,
    Subscription
}
from 'rxjs';

import {
    CSRFService
}
from '../auth/csrf.service';

import {
    WindowRef
}
from './window.service';

import {
    AuthServerProvider
}
from '../auth/auth-jwt.service';

import * as SockJS from 'sockjs-client';
import * as Stomp from 'webstomp-client';

@Injectable({
    providedIn: 'root'
}) export class JhiTrackerService {
    stompClient = null;
    subscriber = null;
    connection: Promise <any> ;
    connectedPromise: any;
    listener: Observable <any> ;
    listenerObserver: Observer <any> ;
    alreadyConnectedOnce = false;
    private subscription: Subscription;

    constructor(
        private router: Router, 
        private authServerProvider: AuthServerProvider, 
        private $window: WindowRef,
        // tslint:disable-next-line: no-unused-variable
        private csrfService: CSRFService
    ) {
        this.connection = this.createConnection();
        this.listener = this.createListener();
    }

    connect() {
        if (this.connectedPromise === null) {
            this.connection = this.createConnection();
        }
        // 构建绝对路径，以便在使用上下文路径部署时websocket不会失败
        const loc = this.$window.nativeWindow.location;
        let url;
        url = '//' + loc.host + loc.pathname + 'websocket/tracker';
        const authToken = this.authServerProvider.getToken();
        if (authToken) {
            url += '?access_token=' + authToken;
        }
        const socket = new SockJS(url);
        this.stompClient = Stomp.over(socket);
        const headers = {};
        this.stompClient.connect(
            headers, 
            () => {
                this.connectedPromise('success');
                this.connectedPromise = null;
                this.sendActivity();
                if (!this.alreadyConnectedOnce) {
                    this.subscription = this.router.events.subscribe(event => {
                        if (event instanceof NavigationEnd) {
                            this.sendActivity();
                        }
                    });
                    this.alreadyConnectedOnce = true;
                }
        });
    }

    disconnect() {
        if (this.stompClient !== null) {
            this.stompClient.disconnect();
            this.stompClient = null;
        }
        if (this.subscription) {
            this.subscription.unsubscribe();
            this.subscription = null;
        }
        this.alreadyConnectedOnce = false;
    }

    receive() {
        return this.listener;
    }

    sendActivity() {
        if (this.stompClient !== null && this.stompClient.connected) {
            this.stompClient.send('/topic/activity', // 目标
                JSON.stringify({
                    page: this.router.routerState.snapshot.url
                }), // 信息体
                {} // 头信息
            );
        }
    }

    subscribe() {
        this.connection.then(() => {
            this.subscriber = this.stompClient.subscribe('/topic/tracker', data = >{
                this.listenerObserver.next(JSON.parse(data.body));
            });
        });
    }

    unsubscribe() {
        if (this.subscriber !== null) {
            this.subscriber.unsubscribe();
        }
        this.listener = this.createListener();
    }

    private createListener() : Observable <any> {
        return new Observable(observer => {
            this.listenerObserver = observer;
        });
    }

    private createConnection() : Promise <any> {
        return new Promise((resolve, reject) = >(this.connectedPromise = resolve));
    }
}
```

JHipster项目服务器端的WebSockets是用Spring的WebSocket支持实现的。要了解有关Spring的WebSockets的更多信息，请参阅Baeldung的Intro to WebSockets with Spring。下一节将介绍使用WebSockets的开发人员生产力工具如何实现非常酷的功能。

## Browsersync

Browsersync是那类让你想知道没有它你是如何生活的工具之一。它可以使您的资源（assets）与浏览器保持同步。它还能够同步浏览器，因此您可以在Safari中滚动并观看同步窗口在Chrome和在iOS模拟器中运行的Safari中滚动。保存文件时，它会更新浏览器窗口，为您节省大量时间。正如其网站上所说，“这是快速而且完全免费的。”

Browsersync可以自由运行和重用，这是由其开源Apache 2.0许可证所保证的。它包含许多带来丝滑体验的功能：

* 交互同步：浏览时，浏览器镜像滚动，单击，刷新和表单操作。
* 文件同步：当您更改HTML，CSS，图像和其他项目文件时，浏览器会自动更新。
* URL历史记录：Browsersync会记录您的测试URL，以便您只需单击一下即可将其推送回所有设备。
* 远程检查器：您可以远程调整和调试在连接的设备上运行的网页。

要在项目中集成Browsersync，需要```package.json```和```gulpfile.js```。你的```package.json```文件只需要包含一些东西，体量仅13行JSON。

```
{
        "name": "jhipster-book", 
        "version": "5.0.1", 
        "description": "The JHipster Mini-Book", 
        "repository": {
                "type": "git", 
                "url": "git@bitbucket.org:mraible/jhipster-book.git"
        }, 
        "devDependencies": {
                "gulp": "3.9.1", 
                "browser-sync": "2.24.7"
        }
}
```

```gulpfile.js```利用```package.json```中指定的工具来启用Browsersync并创建神奇的Web开发体验。

```
var gulp = require('gulp'),
    browserSync = require('browser-sync').create();

gulp.task('serve', function() {

    browserSync.init({
        server: './src/main/webapp'
    });

    gulp.watch(['src/main/webapp/*.html', 'src/main/webapp/css/*.css']).on('change', browserSync.reload);
});

gulp.task('default', ['serve']);
```

创建这些文件后，您需要安装Node.js及其软件包管理器，npm。运行以下命令来安装Browsersync和Gulp。只有在```package.json```中依赖项发生更改时，才需要运行此命令。

```
	npm install
```

然后运行以下命令以创建一个令人开心的开发环境，在该环境中，当硬盘驱动器上的文件发生更改时，浏览器会自动刷新。
```
gulp
```
JHipster使用webpack而不是Gulp为您集成Browsersync。 我在这里展示了一个Gulp示例，因为它非常简单。我强烈推荐Browsersync用于您的项目。它对于确定您的Web应用程序是否可以处理页面重新加载而不会丢失当前用户的状态非常有用。

## 总结

本节描述典型JHipster项目中的UI组件。 它教你如何使用非常流行的UI框架Angular。 它向您展示了如何创建HTML页面并使用Bootstrap使界面看起来更美观。 构建工具对于构建现代Web应用程序至关重要，它向您展示了如何使用webpack。 最后，它向您展示了WebSockets如何工作并描述了Browsersync的美妙之处。

既然您已经了解了JHipster项目中的许多UI组件，那么让我们了解API方面的内容。

