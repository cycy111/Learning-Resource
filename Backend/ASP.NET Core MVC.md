# 概念

## .NET Core MVC

**ASP.NET MVC**是一个构建功能强大的web应用程序的框架，这个框架将通用的**model-view-controller**设计模式应该到ASP.NET开发平台。它也是敏捷开发的一种手段，是ASP.NET平台的核心部分，并且可以替代过时且缓慢的Web窗体。借助.NET Core，MVC框架又向前迈进了一步，使它还具有整合RESTful API开发的能力。

**.NET Core**不仅仅是C＃语言。 它是一个完整的软件开发平台，可让开发人员为所有操作系统和所有设备构建项目。您可以构建桌面应用程序，Web应用程序，移动应用程序，甚至嵌入式应用程序，.NET MVC只是该平台的一部分。

# 创建.NET MVC项目

1. 创建 Azure 账号

2. 新建一个观看列表的项目，这是您观看或拥有的电影的简单库存和评级系统。该应用程序将使用关系数据库存储您输入的信息，包括您分配给每部电影的等级（1到5星）。



![The Create a New Account window lets you specify which type of project you want to build. Choose Web from the bottom of the Project Type dropdown.](D:\File\Learning-Resource\images\15653464199575_MVC P1CH3m.png)

3. 选择ASP.NET Core Web Application.

![From the options now listed, select ASP.NET Core Web Application and click Next.](D:\File\Learning-Resource\images\15653465354793_MVC P1CH3n.png)

4. 配置项目名称，路径等

5. 从应用类型中选*Web Application (model-view-controller)* 

   ![The Create a New ASP.NET Core Application window. Options for different types of applications are available.. We’re building a .NET MVC application, so select Web Application (Model-View-Controller) from the list of application types. Then, under Authen](D:\File\Learning-Resource\images\15653482839832_MVC P1CH3p.png)

6. 改变身份验证类型

   ![The Change Authentication window. Options include No Authentication, Individual User Accounts, Work or School Accounts, and Window Authentication. Choose Individual User Accounts and ensure that the dropdown lists Store User Accounts in-App. Click OK.](D:\File\Learning-Resource\images\15653483320112_MVC P1CH3q.png)

   这将在安装了Entity Framework的情况下创建您的项目，并在项目中内置了安全的用户帐户。

​	现在Visual Studio正在生成您的基本应用程序，并带有用于管理用户帐户（包括注册和登录）的安全系统。 这些帐户是使用ASP.NET Core Identity构建和管理的，ASP.NET Core Identity作为Razor类库包含在应用程序中。

# 初始化数据库

当创建项目时，将生成用于构建数据库并与数据库交互的代码，但是数据库本身尚未更新。 为此，您将使用称为代码优先迁移的过程。打开包管理控制台，运行一下代码

```
update-database
```

这将使用创建项目时生成的当前迁移状态来更新数据库。

运行项目，会在浏览器看到如下页面：

![Your Application Home Page. The top navigation bar says Watchlist and provides links for Home, Privacy, Register, and Login. The content says Welcome in large font. Beneath that, Learn about building Web apps with ASP.NET Core.](D:\File\Learning-Resource\images\15653486387618_MVC P1CH3s.png)

# .NET MVC项目解析

创建完的.NET MVC项目结构大概如下:

![The Solution Explorer window in Visual Studio. The main folders are Areas, Controllers, Data, Models, Views.](D:\File\Learning-Resource\images\15653488995024_MVC P1CH3t.png)

这些文件夹代表将用来构建此应用程序的MVC设计模式组件。 Models文件夹包含所有视图模型（C＃类），这些视图模型将用于将数据库中的数据呈现给浏览器。 Views文件夹包含将呈现用模型表示的数据的所有HTML页面。 Controllers文件夹包含用于处理数据，构造模型和调用服务器上视图的控制器类，以响应每个请求返回它们。

每个控制器的名称也是Views文件夹中子文件夹的名称。 每个控制器都有一个对应的视图文件夹。 这就是MVC路由机制知道要渲染哪个视图以及在哪里找到它的方式。URL请求以[域] / [控制器] / [操作] / {id}格式构造。控制器名称是URL之后域名的第一部分。 随后是调用的控制器动作。操作名称也是View文件夹中与控制器匹配的HTML页面的名称。 {id}参数是可选的，并且在从数据库中检索特定项目时提供。

比如如果你访问*https://yourdomain.com/movies/create*，这时您要您的应用程序导航到MoviesController并在其中找到create action方法。 然后，该方法的代码返回使用名为Create.cshtml的视图呈现的动态HTML页面，该视图可在/ Views / Movies文件夹中找到。 这就是.NET MVC中所有路由的工作方式。

注意Views件夹下的另一个子文件夹：*Shared*。 放置在此文件夹中的所有页面都可以从任何控制器调用。 如果控制器无法在其相应的“Views”文件夹中找到请求的页面，则它将在*Shared*文件夹中查找匹配项。

*Shared*文件夹还包含其他两种重要的视图类型：布局视图和局部视图。 布局包含跨多个页面通用的代码，例如菜单导航和其他页眉元素以及页脚。 然后，各个视图页面仅包含特定于那些页面的代码元素。 例如，如果需要对菜单或页脚进行更改，则只需更改一次（在布局页面中）即可影响对该布局使用的所有其他页面的更改。

局部视图通常是小的HTML代码段，可在多个页面（例如联系表单）中重复使用。 部分视图包含表单，并通过简单的代码行呈现到请求的页面中，而不是在多个页面中重复表单代码。

# ORM与数据库上下文

在Data文件夹下，有两个文件。1.*Migrations*文件夹2.一个被称为*ApplicationDbContext*的*.cs文件。Migrations文件夹包含实体框架生成的所有代码，这些代码为您的应用程序构建关系数据库。 该数据库包含启用，创建和管理安全用户登录所需的所有表。

ApplicatonDbContext类是C＃类，它从实体框架中称为IdentityDbContext的类派生其基本功能。该类包含处理用户帐户和数据库的基本功能。ApplicationDbContext类是数据库的代码表示。 您可以使用它通过对象关系映射（ORM）与数据库进行交互。ORM是将数据库实体映射到C＃代码的一种方法，其中一个类可以表示表中的记录，而类属性则表示列。