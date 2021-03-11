

# 基础概念



REST stands for **Re**presentational **S**tate **T**ransfer,它只是一组体系结构标准或准则，构成了您如何在应用程序与世界其他地方或应用程序的不同组件之间通信数据的结构。

RESTful APIs 是基于http来传输信息的，和我们在浏览器上访问网站的工作方式一样。

使用REST API有六项关键的体系结构指南：

##  使用REST Resource和Collection

REST data用Resource表示，Resource是类似名词的对象。Resource是故意将其抽象化，以便可以表示需要与之交互的任何数据。

假设您正在为杂货店构建API，以提供在线送货服务。顾客能够购买产品，雇员可以添加产品以及更新库存。Resource就可能有这些：

- Customer
- Employee
- Cart
- Inventory
- Product

每个Resource都包含有关你要描述的数据的额外信息。

- An employee resource could have:  `first_name`,  `last_name`,  `id`,  `start_date` 
- A customer resource could have:  `first_name`,  `last_name`,  `number_of_visits`,  ` loyalty_customer` ,  `order_history`

一组resource叫一**collection**，**collection**通过resource的复数形式被引用：

| **Resource** | **Collection** |
| ------------ | -------------- |
| employee     | employees      |
| customer     | customers      |

通过**Resource**和**Collection**来存储要在API中使用的数据。

如果**Resource**是存储数据的对象，则访问资源的方式是统一资源标识符（URI）

API通过URI 访问数据

![Endpoint](D:\File\Learning-Resource\images\endpoint.png)

REST API数据有两种可使用的语言：**XML**和**JSON**

# 定义常规请求和响应

##  Request结构

![A Typical Request Structure](D:\File\Learning-Resource\images\requestStruct.png)

请求采取以下形式:

HTTP Verb + URI + HTTP Version + Headers + Optional Message Body

HTTP动词是发出请求时可能采取的操作类型。最常见的有 **GET,** **PUT,** **POST,** 和**DELETE.**

A **URI** or **resource path** 是识别resource的方式。

The request URL is the full endpoint you are using for your request.

The **HTTP version** is just the version of HTTP that is being used. The most common one you'll see is  `HTTP/1.1`.

A **header** enables you to pass along additional information about the message. 比如：你使用的语言是什么，你发送请求的时间，你使用什么软件，你的authentication key是什么。

请求中的消息体是可选项，一般在更新或者创建数据的时候会有数据（以XML或JSON格式）

## Response 结构

![A Response Structure](D:\File\Learning-Resource\images\15606885092306_response.png)

响应采取以下形式:

HTTP Version + HTTP Response Code + Headers + Message Body

下面是Postman中的一个响应实例：

![A Typical Response Message](D:\File\Learning-Resource\images\15620654533504_response-marked.png)

常见的HTTP Response Code 有这些：

- 100+ ➡ Informational
- 200+ ➡ Success
- 300+ ➡ Redirection
- 400+ ➡ Client error
- 500+ ➡ Server error

# CRUD

当使用API时，CRUD是流行的行业首字母缩写. 尽管CRUD并不是真正的技术机制，但是每个CRUD操作都有一个关联的HTTP动词，您可以在使用API时使用它。

| **CRUD Action** | **Associated HTTP Verb** |
| --------------- | ------------------------ |
| Create          | POST                     |
| Read            | GET                      |
| Update          | PUT                      |
| Delete          | DELETE                   |

POST新建数据

PUT或者PATCH更新数据

DELETE删除数据

# 验证API以提高安全性

必须进行身份验证，以确保只有具有适当权限的人员才能访问您的API。

一种常见的身份验证方法是要求开发人员通过API网站注册令牌或密钥，然后在其请求中使用令牌或密钥。

与其他任何事情一样，重要的是要牢记安全性，并确保API来自可靠的来源。高质量的API将具有安全性措施，例如身份验证，授权和加密。

API密钥或令牌通常与请求一起用于验证用户身份。

# 设计 API Endpoints

比如为一个名为InstaPhoto的分享照片的APP设计API。我们想要用户可以上传并分享他们自己的照片，可以评论别人的照片，创建主题，通过地理位置搜索图片等等...

最终的resource可能有这些：

- /photo
- /user
- /location
- /post

同时我们也要考虑更重要的事情，比如：

- Which endpoints will require authorization?
- Which resources will you want to be able to update?
- Will you need to be able to edit posts after they are created?
- Should comments be deletable?
- Do you need all the CRUD operations for each resource? Or just one or two?

设计Endpoints，首先是命名。当前的命名约定仅包括您要更新的资源，而不包括您要完成的动词，因为该动作已经在HTTP动词中。

- `POST /photo`
- `PUT /photo`
- `GET /photo`

用于创建、查看、删除照片的Endpoints可能是：

- `POST /photos`
- `GET /photos/{photoId}`
- `DELETE /photos/{photoId}`

由于您通常要查看或删除特定的照片，因此有必要为后两个端点添加一个ID。

对于用户，能够编辑用户自己的个人信息是有必要的。

与用户相关的Eenpoints可能有：

- `POST /users`
- `GET /users/{userId}`
- `PUT /users/{userId}`
- `DELETE /users/{userId}`

对于创建，编辑，删除用户信息是需要身份验证和指定的ID的。

有关评论，可能会有这些Eenpoints：

| Endpoint                                        | Description                                                  |
| ----------------------------------------------- | ------------------------------------------------------------ |
| `POST /photos/{photoId}/comments`               | Create a new comment for photo with id {photoId}             |
| `GET /photos/{photoId}/comments`                | Get all the comments for a specific photo with id {photoId}  |
| `GET /photos/{photoId}/comments/{commentId}`    | Get the comment with id {commentId} for photo with id {photoId} |
| `DELETE /photos/{photoId}/comments/{commentId}` | Delete the comment with id {commentId} for photo with id {photoId} |

小结：

- When planning, think about what CRUD operations you'll need for which resources.
- Use resource names and n*ot* verbs when naming endpoints.
- Think about what resources you'll need to contain *within* other resources.

# 构建REST API

构建API的原因和场景：

* API是介于数据库和访问用户的，它避免了前端开发人员之间操作数据库。
* 如果你在构建一个应用程序，你希望其他开发工程师能和你的数据交互

文档和正确的错误处理对于其他开发人员了解您的API至关重要。

## 开发前准备

Start by asking yourself about important functionality you need to use with your API:

- What kind of endpoints will you need? 
- What resources do you need to create?
- What resources will you need to perform **CRUD** operations on?
- Do you need all four **CRUD** operations for each resource? Or just one or two?

## API文档

考虑到使用API的开发员，好的文档和错误处理能够很快明白你的API能够干什么。

有很多信息需要解释给API用户，包括：

1. Descriptions of the API resources.
2. The available URIs and HTTP verbs along with what they do.
3. The parameters (if any) that need to be given to the endpoint.
4. A request example. 
5. A typical response for the given request.

## API文档范例

1. 一个很好的示例，显示了HTTP动词，URI，参数以及终端功能的描述。

![The Pinterest API is a good example of a URI, parameters, and the description](https://user.oc-static.com/upload/2019/06/17/15607936998385_Screen%20Shot%202019-06-17%20at%208.47.48%20PM.png)

2. Twitter API Documentation

![Resource Information in Twitter Documentation](https://user.oc-static.com/upload/2019/06/17/15607929776075_Screen%20Shot%202019-06-17%20at%208.36.07%20PM.png)

# Advanced Endpoint Functionality

以InstaPhoto应用为例，通过设计URI的方式,我们可以建立更复杂的URI,比如过滤、搜索、排序等等。

## Filter

```
GET /photos?location={locationId}
```

## Search

```
GET /photos?search=dogs
```

## Sort

```
GET /users/{userId}/followers?sort=lastName&order=asc
```

## Pagination

```
GET /photos?page=23
```

## Versioning

通常有两种方式：

1. 在请求头的version字段加版本号，"accept-version": "1.3"
2. 直接加到URI中

# 选择框架构建API

构建API的过程取决于您使用的编程语言或工具。以下是开发人员使用的一些流行工具和框架：

## Spring (Java)

![Spring](D:\File\Learning-Resource\images\15608640024551_spring.png)

[Spring](https://spring.io/projects/spring-boot)是一个用Jave的web框架。Wix，TicketMaster和BillGuard等网站都使用它

## Django (Python)

![Django](D:\File\Learning-Resource\images\15608642443903_django-logo.png)

[Django](https://www.djangoproject.com/) 使用Python进行Web开发,一些知名的公司（例如Google，YouTube和Instagram）都在使用它.

使用Django构建REST API时，[Django Rest Framework ](https://www.django-rest-framework.org/)很容易使用。对于初学者来说，它的学习曲线更陡峭，但是具有强大的内置功能，例如身份验证和消息传递。

## Flask

![Flask](D:\File\Learning-Resource\images\15608640190374_flask.png)

[Flask](http://flask.pocoo.org/) 使用Python进行Web和REST API开发。这是一个简约的框架，易于学习和使用。Flask内置的功能少于Django，但使开发人员可以在使用的其他工具中拥有更多发言权。

## Express.js

![Express with Node.js](D:\File\Learning-Resource\images\15608640694189_express.jpeg)

[Express](https://expressjs.com/) 使用Node.js和Javascript，是一个最小且快速的框架。它非常灵活，并且支持完整的应用程序以及REST API。最大的缺点是没有明确的处理方式，这对初学者来说可能很难。 

## Ruby on Rails

![Ruby on Rails](D:\File\Learning-Resource\images\15608641046099_ruby.png)

[Ruby on Rails](https://rubyonrails.org/) 内置在Ruby中，并且是许多开发人员中流行的框架。它被认为是“幕后魔术”框架，因为它隐藏了很多复杂性。有许多第三方插件（称为Ruby gems）可供使用，并且Rails开发人员社区非常庞大，有大量的在线教程。

## AWS API Gateway

[AWS API Gateway](https://aws.amazon.com/api-gateway/) 和[AWS Lambda](https://aws.amazon.com/lambda/) 是一种主要使用用户界面（因此代码更少！）来创建和使用REST API的方法。它使您可以轻松地将您的网站与所有AWS服务集成在一起，并可以通过其基础架构轻松扩展和管理更多请求。

