# 設計典範 programming paradigms
* 函式程式設計 -> 宣告式程式設計 (Declarative programming)
* 物件導向程式設計 -> 指令式程式設計(Imperative programming)
* 語言導向程式設計(Language-Oriented programming)
* 元程式設計 (Metaprogramming)
* 事件驅動程式設計 (Event-driven programming)
* 資料驅動程式設計 (Data-driven programming)

# 物件導向 OOP (Object-oriented programming)
* 四大特性
  * 抽象 Abstraction
  * 封裝 Encapsulation
  * 繼承 Inheritance
  * 多型 Polymorphism

# 物件與類別 Classes and Objects
* 繼承 extends
* implements

# 設計模式 Design Patterns

* Creational patterns
  * Factory Method
  * Abstract Factory
  * Builder
  * Prototype
  * Singleton

* Structural patterns
  * Adapter
  * Bridge
  * Composite
  * Decorator
  * Facade
  * Flyweight
  * Proxy

* Behavioral patterns
  * Chain of Responsibility
  * Command
  * Lterator
  * Mediator
  * Memento
  * Observer
  * State
  * Strategy
  * Template Method
  * Visitor

# Coding Style

# RESTful

```
用URL定位資源
用HTTP動詞（GET,POST,DELETE,DETC）描述操作
資源是REST的核心，重點在如何設計server與client的交互形式
```

* HTTP Methods
  * GET
  * POST
  * PUT
  * DELETE

### HTTP Response Status Code
```
1xx: Informational
2xx: Success
3xx: Redirection
4xx: Client Error
5xx: Server Error
```

### REST Specific HTTP Status Codes
```
200 OK
301 Moved Permanently
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
500 Internal Server Error
```

### Singleton and Collection Resources
資源可以是單例或是集合
例如 customer 或者 customers 
用URI表示/customers
/customers
/customers/{id}

### Collection and Sub-collection Resources
```
子集合
/customers/{customerId}/accounts
子集合資源內的單例資源
/customers/{customerId}/accounts/{accountId}
```

### Use nouns to represent resources
### document

### Do not use trailing forward slash (/) in URIs
```
BAD  http://api/posts/
GOOD http://api/posts
```
### Use forward slash (/) to indicate hierarchical relationships
```
http://api/shapes/polygons/quadrilaterals/squares
```

### Use hyphens (-) to improve the readability of URIs
```
http://api.example.com/blogs/guy-levin/posts/this-is-my-first-post
```

### Do not use underscores ( _ )
### Use lowercase letters in URIs
```
BAD  http://api.example.com/My-Folder/My-doc
GOOD http://api.example.com/my-folder/my-doc
```
### Do not use file extensions
```
BAD  http://api.example.com/students/3248234/courses/2005/fall.json
GOOD http://api.example.com/students/3248234/courses/2005/fall
```

### Never use CRUD function names in URIs
```
HTTP GET http://api.example.com/posts  //Get all devices
HTTP POST http://api.example.com/posts  //Create new Device

HTTP GET http://api.example.com/posts/{id}  //Get device for given Id
HTTP PUT http://api.example.com/posts/{id}  //Update device for given Id
HTTP DELETE http://api.example.com/posts/{id}  //Delete device for given Id
```

### Use query component to filter URI collection
```
http://api.example.com/posts
http://api.example.com/posts?region=USA
http://api.example.com/posts?region=USA&brand=XYZ
http://api.example.com/posts?region=USA&brand=XYZ&sort=installation-date
```

# MVC
- Model 數據模型，業務模型
- View 數據顯示
- Controller 控制器 連接M與V的

# SPA (Single Page Application)
單頁式應用，利用AJAX同步資料
缺點是有較差的SEO

# SSR (Server side render)
由伺服器渲染資料
有利於SEO

# AMP (Accelerated Mobile Pages)
加速行動網頁

# PWA (Progressive Web App)
漸進式網頁應用程式
使用網頁也可以是APP使用
