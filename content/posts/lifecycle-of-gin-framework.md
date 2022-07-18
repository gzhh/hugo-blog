---
title: "Gin生命周期"
date: 2022-07-05T17:38:50+08:00
draft: false
categories: [Go]
tags: [Gin]
---
<!--more-->
{{< toc >}}

## 一、Gin 简介
> Gin is a HTTP web framework written in Go (Golang). It features a Martini-like API with much better performance -- up to 40 times faster. If you need smashing performance, get yourself some Gin.

Gin 是使用 Golang/Go 实现的 HTTP Web 框架，代码简洁且性能高。截止 1.8.0 版本为止，总体实现代码 18K 行，而去掉测试代码的话则仅为 7K 行。
```
$ find . -name "*.go" | xargs cat | wc -l
17970
$ find . -name "*_test.go" | xargs cat | wc -l
11060
```

### Gin 特性
1. 快速：路由不使用反射，而是基于 Radix 树实现的，内存占用少且速度快。
2. 中间件：一个 HTTP 请求可以很方便的多个中间件处理。
3. 路由分组：可以按不同需要将路由分成多个组，很方便。
4. 异常处理：使用 defer panic 机制可以很方便的处理服务异常，防止服务宕机。

### 目录结构
查看项目结构，其中以 context.go、gin.go、routergroup.go、tree.go 这几个文件最为核心。
其中又以 Context、Engine、RouterGroup、MethodTree 这几个结构体最为核心。
1. Engine：实现了标准库 net/http 包 Handler 接口的 ServeHTTP 方法
2. MethodTree：根据 HTTP 请求方法分别维护路由树
3. RouterGroup：将路由表分组，方便中间件统一处理
4. Context：Gin 的上下文，用于在 handler 之间传递参数

`tree -P "*.go" -I "testdata|*_test.go" .`

```
├── any.go
├── auth.go
├── binding
│   ├── any.go
│   ├── binding.go
│   ├── binding_nomsgpack.go
│   ├── default_validator.go
│   ├── form.go
│   ├── form_mapping.go
│   ├── header.go
│   ├── json.go
│   ├── msgpack.go
│   ├── multipart_form_mapping.go
│   ├── protobuf.go
│   ├── query.go
│   ├── toml.go
│   ├── uri.go
│   ├── xml.go
│   └── yaml.go
├── context.go
├── context_appengine.go
├── debug.go
├── deprecated.go
├── doc.go
├── errors.go
├── examples
├── fs.go
├── gin.go
├── ginS
│   └── gins.go
├── internal
│   ├── bytesconv
│   │   └── bytesconv.go
│   └── json
│       ├── go_json.go
│       ├── json.go
│       └── jsoniter.go
├── logger.go
├── mode.go
├── path.go
├── recovery.go
├── render
│   ├── any.go
│   ├── data.go
│   ├── html.go
│   ├── json.go
│   ├── msgpack.go
│   ├── protobuf.go
│   ├── reader.go
│   ├── redirect.go
│   ├── render.go
│   ├── text.go
│   ├── toml.go
│   ├── xml.go
│   └── yaml.go
├── response_writer.go
├── routergroup.go
├── test_helpers.go
├── tree.go
├── utils.go
└── version.go
```

## 二、实现原理
### 从官网 Demo 开始
``` go
package main

import (
	"github.com/gin-gonic/gin"
)

func main() {
    // 创建 Gin Engine 实例
	r := gin.Default()

    // 注册路由及处理函数
	r.GET("/ping", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"message": "pong",
		})
	})

    // 启动 Web 服务
	r.Run() // listen and serve on 0.0.0.0:8080
}
```

使用 Gin 运行一个 web server，需要分为三个步骤
- 创建 Engine 实例
- 注册路由及处理函数
- 启动 Web 服

### Gin 生命周期
接上一节的简单 web server 我们继续往深处分析一个 HTTP 请求在 Gin 框架里面的生命周期是什么样的。

**1. 首先我们从 gin.Default() 方法开始**
- 创建 gin Engine 对象
- 配置中间件
- 返回 gin Engine 对象
```golang
func Default() *Engine {
	debugPrintWARNINGDefault()
	engine := New()
	engine.Use(Logger(), Recovery())
	return engine
}
```

**2. 接着我们再看 gin.New() 方法的具体实现**
- 初始化 Engine 对象
- 初始化路由组 Engine.RouterGroup
- 初始化 Engine.pool，用来存储 context 上下文对象，优化 HTTP 请求性能
```golang
func New() *Engine {
	debugPrintWARNINGNew()
    // 初始化 gin 最核心的 Engine 对象
	engine := &Engine{
		RouterGroup: RouterGroup{
			Handlers: nil,
			basePath: "/",
			root:     true,
		},
        ...
		trees:                  make(methodTrees, 0, 9),
        ...
	}

    // 关联 Engine 和 RouterGroup 对象
	engine.RouterGroup.engine = engine

    // 初始化 pool
	engine.pool.New = func() any {
		return engine.allocateContext() // 初始化上下文对象
	}
	return engine
}
```

**3. 再来看下 gin.Engine{} 结构体**
> Engine 是 gin 框架的核心实例，所有的组件都是由 Engine 实例驱动的。
```
// Engine is the framework's instance, it contains the muxer, middleware and configuration settings.
// Create an instance of Engine, by using New() or Default()
type Engine struct {
	RouterGroup // 路由组

	RedirectTrailingSlash bool
	RedirectFixedPath bool
	HandleMethodNotAllowed bool
	ForwardedByClientIP bool

    AppEngine bool
	UseRawPath bool

    ...

	delims           render.Delims
	secureJSONPrefix string
	HTMLRender       render.HTMLRender
	FuncMap          template.FuncMap

	allNoRoute       HandlersChain
	allNoMethod      HandlersChain
	noRoute          HandlersChain
	noMethod         HandlersChain

	pool             sync.Pool
	trees            methodTrees

    ...
}
```

**4. gin.Get() 的具体实现**

由 Demo 代码中的 `r.GET("/ping", func(c *gin.Context){})` 实现，找到 gin.GET() 方法的具体实现可进一步分析 HTTP 请求被路由的过程

```go
// GET is a shortcut for router.Handle("GET", path, handle).
func (group *RouterGroup) GET(relativePath string, handlers ...HandlerFunc) IRoutes {
	return group.handle(http.MethodGet, relativePath, handlers)
}
```

接下来我们详细查看 routergroup.go 文件中的几个重要概念：

4.1 gin.RouterGroup{}
```go
// RouterGroup is used internally to configure router, a RouterGroup is associated with
// a prefix and an array of handlers (middleware).
type RouterGroup struct {
	Handlers HandlersChain // 路由处理器数组
	basePath string
	engine   *Engine // 关联 gin.Engine 对象
	root     bool
}
```

4.2 gin.IRouter{}、gin.IRoutes{}
```go
// IRouter defines all router handle interface includes single and group router.
type IRouter interface {
	IRoutes
	Group(string, ...HandlerFunc) *RouterGroup
}

// IRoutes defines all router handle interface.
type IRoutes interface {
	Use(...HandlerFunc) IRoutes

	Handle(string, string, ...HandlerFunc) IRoutes
	Any(string, ...HandlerFunc) IRoutes
	GET(string, ...HandlerFunc) IRoutes
	POST(string, ...HandlerFunc) IRoutes
	DELETE(string, ...HandlerFunc) IRoutes
	PATCH(string, ...HandlerFunc) IRoutes
	PUT(string, ...HandlerFunc) IRoutes
	OPTIONS(string, ...HandlerFunc) IRoutes
	HEAD(string, ...HandlerFunc) IRoutes

	StaticFile(string, string) IRoutes
	StaticFileFS(string, string, http.FileSystem) IRoutes
	Static(string, string) IRoutes
	StaticFS(string, http.FileSystem) IRoutes
}
```

4.3 gin.handle{}
```go
func (group *RouterGroup) handle(httpMethod, relativePath string, handlers HandlersChain) IRoutes {
	absolutePath := group.calculateAbsolutePath(relativePath)
	handlers = group.combineHandlers(handlers)

    // 添加路由，即最终将路由配置添加到 gin.Engine.trees 属性（路由path与handler的绑定暂不深入探究）
	group.engine.addRoute(httpMethod, absolutePath, handlers)
	return group.returnObj()
}
```

**5. gin.Run() 启动服务**

接着我们回到 web server Demo 中的最后一步，gin.Run() 启动 web 服务，查看 gun.Run() 方法的实现

```go
// Run attaches the router to a http.Server and starts listening and serving HTTP requests.
// It is a shortcut for http.ListenAndServe(addr, router)
// Note: this method will block the calling goroutine indefinitely unless an error happens.
func (engine *Engine) Run(addr ...string) (err error) {
	defer func() { debugPrintError(err) }()
    ...
	address := resolveAddress(addr)
	debugPrint("Listening and serving HTTP on %s\n", address)
	err = http.ListenAndServe(address, engine.Handler()) // 调用 go 标准库 net/http 包的 ListenAndServe() 启动 HTTP 服务，处理 HTTP 请求
	return
}
```

5.1 http.ListenAndServe()、http.Handler{}

- 分析 net/http 标准库 http.http.ListenAndServe() 的实现，接受 addr 和 handler 参数，监听地址并调用服务处理 HTTP 请求
- 此处 handler 的参数是 http.Handler{} 接口类型，所以 gin 如果要接入 net/http 标准库的 ListenAndServe() 方法，则必须对传入的 handler 参数实现 http.Handler{} 接口中的 `ServeHTTP(ResponseWriter, *Request)` 方法

```go
type Handler interface {
	ServeHTTP(ResponseWriter, *Request)
}

func ListenAndServe(addr string, handler Handler) error {
	server := &Server{Addr: addr, Handler: handler}
	return server.ListenAndServe()
}
```

5.2 gin.ServeHTTP() 的实现
```
// ServeHTTP conforms to the http.Handler interface.
func (engine *Engine) ServeHTTP(w http.ResponseWriter, req *http.Request) {
	c := engine.pool.Get().(*Context) // 从对象池 pool 获取 context 上下文对象
	c.writermem.reset(w)
	c.Request = req
	c.reset()

	engine.handleHTTPRequest(c) // 处理 HTTP 请求

	engine.pool.Put(c) // 处理完请求将 context 对象归还给对象池 pool
}
```

5.3 gin.handleHTTPRequest()

接下来我们看下 gin 处理 HTTP 请求的具体实现
- 接受上下文对象参数
- 遍历路由树，如果请求上下文中的路径及各个参数在路由树中匹配得到的话，则执行 c.Next() 方法调用具体的 handler 处理器
```go
func (engine *Engine) handleHTTPRequest(c *Context) {
	httpMethod := c.Request.Method
	rPath := c.Request.URL.Path

    ...

	// Find root of the tree for the given HTTP method
	t := engine.trees
	for i, tl := 0, len(t); i < tl; i++ {
        ...

		root := t[i].root
		// Find route in tree
		value := root.getValue(rPath, c.params, c.skippedNodes, unescape)
		if value.params != nil {
			c.Params = *value.params
		}
		if value.handlers != nil {
			c.handlers = value.handlers
			c.fullPath = value.fullPath
			c.Next() // 关键逻辑，调用 handler 处理请求
			c.writermem.WriteHeaderNow()
			return
		}

        ...

		break
	}

    ...

	c.handlers = engine.allNoRoute

    ...
}
```

**5.4 gin.Context{}**

上面我们提到了 gin.Context 对象池 pool 概念，以及使用 Context 对象 c.Next 方法调用具体 handler，接下来我们具体看下 gin 的 Context 对象：

Context 是 gin 最重要的部分，我们可以通过 Context 在中间件之间传递参数、管理请求流、验证 json，以及返回响应 json
```
// Context is the most important part of gin. It allows us to pass variables between middleware,
// manage the flow, validate the JSON of a request and render a JSON response for example.
type Context struct {
	writermem responseWriter

	Request   *http.Request // HTTP 请求
	Writer    ResponseWriter // HTTP 响应

	Params   Params
	handlers HandlersChain // handler 处理数组
	index    int8
	fullPath string

	engine       *Engine // 关联 gin.Engine 对象
	params       *Params
	skippedNodes *[]skippedNode

	// This mutex protects Keys map.
	mu sync.RWMutex

	// Keys is a key/value pair exclusively for the context of each request.
	Keys map[string]any

	// Errors is a list of errors attached to all the handlers/middlewares who used this context.
	Errors errorMsgs

	// Accepted defines a list of manually accepted formats for content negotiation.
	Accepted []string

    ...
}
```

## 三、总结

总结下一个 gin 框架的大致运行流程：

![HTTP request lifecycle in gin](https://uploads.gzhh.tech/2022/07/http-request-lifecycle-in-gin.png)

还有 Gin 中基本最重要的概念就是围绕 Context 来展开的，理解 Context 的实现对用好 Gin 框架可以起到重要的作用。

最后 MethodTree 这部分的逻辑稍微复杂一些，待后续有时间再仔细研究研究。


## Ref
1. [https://pkg.go.dev/net/http](https://pkg.go.dev/net/http)
2. [https://github.com/hhstore/blog/issues/131](https://github.com/hhstore/blog/issues/131)
3. [https://learnku.com/articles/52480](https://learnku.com/articles/52480)
4. [https://geektutu.com/post/quick-go-gin.html](https://geektutu.com/post/quick-go-gin.html)
