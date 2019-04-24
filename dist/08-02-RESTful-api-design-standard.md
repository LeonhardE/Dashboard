## RESTful API 设计规范

REST API 是一系列个体可描述 (individually-addressable) 的资源（API的名词）的模型。
资源可以通过他们的资源名称来提及，并可以通过一个小集合内的方法（即API的动词）来操作。

**建议按照下列步骤来设计面向资源的API**

1.确定API提供的资源类型

2.查明不同资源间的关系

3.根据资源的类型和关系，决定资源名称的规范

4.决定资源的范式 (schema)

5.为资源加上方法的最小集合

### 1. 协议

API与用户的通信协议，总是使用HTTPs协议。

### 2. 域名

应该尽量将API部署在专用域名之下。

https://api.example.com

如果确定API很简单，不会有进一步扩展，可以考虑放在主域名下。

https://example.org/api/

### 3. 版本

应该将API的版本号放入URL。

https://api.example.com/v1/

另一种做法是，将版本号放在HTTP头信息中，但不如放入URL方便和直观。Github采用这种做法。

### 4. 路径

路径又称"终点"（endpoint），表示API的具体网址。

在RESTful架构中，每个网址代表一种资源（resource），**所以网址中不能有动词，只能有名词**，而且所用的名词往往与数据库的表格名对应。

一般来说，数据库中的表都是同种记录的"集合"（collection），**所以API中的名词也应该使用复数。**

举例来说，有一个API提供动物园（zoo）的信息，还包括各种动物和雇员的信息，则它的路径应该设计成下面这样。

https://api.example.com/v1/zoos

https://api.example.com/v1/animals

https://api.example.com/v1/employees

不要使用：

/getZoos
/createNewAnimals
/deleteAllEmployees

### 5. HTTP动词

对于资源的具体操作类型，由HTTP动词表示。

常用的HTTP动词有下面五个（括号里是对应的SQL命令）。

GET（SELECT）：从服务器取出资源（一项或多项）。

POST（CREATE）：在服务器新建一个资源。

PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。

PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。

DELETE（DELETE）：从服务器删除资源。

还有两个不常用的HTTP动词。

HEAD：获取资源的元数据。

OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的。

下面是一些例子。

GET /zoos：列出所有动物园

POST /zoos：新建一个动物园

PUT /zoos/ID：更新某个指定动物园的信息（提供该动物园的全部信息）

PATCH /zoos/ID：更新某个指定动物园的信息（提供该动物园的部分信息）

DELETE /zoos/ID：删除某个动物园

**使用PUT, POST 和DELETE 方法 而不是 GET 方法来改变状态，不要使用GET 进行状态改变.**

GET /users/711?activate 

GET /users/711/activate

### 6.过滤、排序、选择、分页

API应该提供参数，过滤返回结果。

如：

http://api.example.com/zoos/ID/animals?limit=10: 指定返回的动物园的动物记录的数量

参数的设计允许存在冗余，即允许API路径和URL参数偶尔有重复。

比如，GET /zoo/ID/animals 与 GET /animals?zoo_id=ID 的含义是相同的。

API允许针对多个字段排序

GET /cars?sort=-manufactorer,+model

这是返回根据生产者降序和模型升序排列的car集合

GET /tickets?sort=-priority,created_at

复杂的排序规则应该通过组合实现，排序规则有多个rule以逗号间隔组合而成。

选择：

有时候API使用者不需要所有的结果，在进行横向限制的时候（例如值返回API结果的前十项）还应该可以进行纵向限制。

并且这个功能能有效的提高网络带宽使用率和速度。

可以使用fields查询参数来限制返回的域例如：

GET /cars?fields=manufacturer,model,id,color

分页：

将分页信息放到link header里面：http://tools.ietf.org/html/rfc5988#page-6。

来自github的文档：

Link: <https://api.github.com/user/repos?page=3&per_page=100>; rel="next", 

<https://api.github.com/user/repos?page=50&per_page=100>; rel="last"

### 7. 状态码

服务器向用户返回的状态码和提示信息，常见的有以下一些（方括号中是该状态码对应的HTTP动词）。

200 – OK – 一切正常

201 – OK – 新的资源已经成功创建

204 – OK – 资源已经成功擅长

304 – Not Modified – 客户端使用缓存数据

400 – Bad Request – 请求无效，需要附加细节解释如 "JSON无效"

401 – Unauthorized – 请求需要用户验证

403 – Forbidden – 服务器已经理解了请求，但是拒绝服务或这种请求的访问是不允许的。

404 – Not found – 没有发现该资源

422 – Unprocessable Entity – 只有服务器不能处理实体时使用，比如图像不能被格式化，或者重要字段丢失。

500 – Internal Server Error – API开发者应该避免这种错误。

**更新和创建操作应该返回对应的状态码，防止用户多次的API调用。**

### 8. 错误处理

如果状态码是4xx，就应该向用户返回出错信息。一般来说，返回的信息中将error作为键名，出错信息作为键值即可。
使用详细的错误包装错误：

```json
{
  "errors": [
   {
    "userMessage": "Sorry, the requested resource does not exist",
    "internalMessage": "No car found in the database",
    "code": 34,
    "more info": "http://dev.mwaysolutions.com/blog/api/v1/errors/12345"
   }
  ]
}
```

### 9. 返回结果

针对不同操作，服务器向用户返回的结果应该符合以下规范。

GET /collection：返回资源对象的列表（数组）

GET /collection/resource：返回单个资源对象

POST /collection：返回新生成的资源对象

PUT /collection/resource：返回完整的资源对象

PATCH /collection/resource：返回完整的资源对象

DELETE /collection/resource：返回一个空文档

### 10. Hypermedia API

RESTful API最好做到Hypermedia，即返回结果中提供链接，连向其他API方法，使得用户不查文档，也知道下一步应该做什么。

比如，当用户向api.example.com的根目录发出请求，会得到这样一个文档。

```json
{"link":{
  "rel":  "collection https://www.example.com/zoos",
  "href": "https://api.example.com/zoos",
  "title":"List of zoos",
  "type": "application/vnd.yourformat+json"
}}
```

上面代码表示，文档中有一个link属性，用户读取这个属性就知道下一步该调用什么API了。

rel表示这个API与当前网址的关系（collection关系，并给出该collection的网址），href表示API的路径，title表示API的标题，type表示返回类型。


### 11. 如果一个资源与另外一个资源有关系，使用子资源

GET /cars/711/drivers/ 返回 car 711的所有司机

GET /cars/711/drivers/4 返回 car 711的4号司机

### 12. 使用Http头声明序列化格式

在客户端和服务端，双方都要知道通讯的格式，格式在HTTP-Header中指定

Content-Type 定义请求格式

Accept 定义系列可接受的响应格式

### 13. 基于验证的缓存

**不要在服务端存储应用状态**

RESTful HTTP的交互必须是无状态的，这表明每一次请求要包含处理该请求所需的一切信息，每一次请求都应该包含鉴权证明。

客户端负责维护应用状态，RESTful服务端不需要在请求间保留应用状态，服务端负责维护资源状态而不是应用状态。

通过使用ssl我们可以不用每次都提供用户名和密码：我们可以给用户返回一个随机产生的token。

这样可以极大的方便使用浏览器访问API的用户。

这种方法适用于用户可以首先通过一次用户名-密码的验证并得到token，并且可以拷贝返回的token到以后的请求中。

如果不方便，可以使用OAuth 2来进行token的安全传输。

### 14.其他

（1）**应该尽量使用JSON**，避免使用XML。

（2）使用蛇形命令（下划线和小写）。


