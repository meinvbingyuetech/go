````go
先定义一个中间件函数：

// 该函数很简单，只会给c上下文添加一个属性，并赋值。后面的路由处理器，可以根据被中间件装饰后提取其值
func MiddleWare() gin.HandlerFunc {
    return func(c *gin.Context) {
        c.Set("request", "test_request")
        c.Next()
    }
}

router.Use(MiddleWare())
{
	router.GET("/middleware", func(c *gin.Context) {
	    request := c.MustGet("request").(string)  // 如果没有注册就使用MustGet方法读取c的值将会抛错，可以使用Get方法取而代之。
	    req, _ := c.Get("request")
	    c.JSON(http.StatusOK, gin.H{
		"middile_request": request,
		"request": req,
	    })
	})
}

````

````
单个路由中间件

router.GET("/before", MiddleWare(), func(c *gin.Context) {
	request := c.MustGet("request").(string)
	c.JSON(http.StatusOK, gin.H{
	    "middile_request": request,
	})
})
````
----

```go
func main() {
	// Creates a router without any middleware by default
	r := gin.New()

	// Global middleware
	// Logger middleware will write the logs to gin.DefaultWriter even you set with GIN_MODE=release.
	// By default gin.DefaultWriter = os.Stdout
	r.Use(gin.Logger())

	// Recovery middleware recovers from any panics and writes a 500 if there was one.
	r.Use(gin.Recovery())

	// Per route middleware, you can add as many as you desire.
	r.GET("/benchmark", MyBenchLogger(), benchEndpoint)

	// Authorization group
	// authorized := r.Group("/", AuthRequired())
	// exactly the same as:
	authorized := r.Group("/")
	// per group middleware! in this case we use the custom created
	// AuthRequired() middleware just in the "authorized" group.
	authorized.Use(AuthRequired())
	{
		authorized.POST("/login", loginEndpoint)
		authorized.POST("/submit", submitEndpoint)
		authorized.POST("/read", readEndpoint)

		// nested group
		testing := authorized.Group("testing")
		testing.GET("/analytics", analyticsEndpoint)
	}

	// Listen and serve on 0.0.0.0:8080
	r.Run(":8080")
}
```
----

- 自定义中间件
```go
func Logger() gin.HandlerFunc {
	return func(c *gin.Context) {
		t := time.Now()

		// Set example variable
		c.Set("example", "12345")

		// before request

		c.Next()

		// after request
		latency := time.Since(t)
		log.Print(latency)

		// access the status we are sending
		status := c.Writer.Status()
		log.Println(status)
	}
}

func main() {
	r := gin.New()
	r.Use(Logger())

	r.GET("/test", func(c *gin.Context) {
		example := c.MustGet("example").(string)

		// it would print: "12345"
		log.Println(example)
	})

	// Listen and serve on 0.0.0.0:8080
	r.Run(":8080")
}
```

----
- 鉴权中间件
```go
router.GET("/auth/signin", func(c *gin.Context) {
	cookie := &http.Cookie{
	    Name:     "session_id",
	    Value:    "123",
	    Path:     "/",
	    HttpOnly: true,
	}
	http.SetCookie(c.Writer, cookie)
	c.String(http.StatusOK, "Login successful")
})

router.GET("/home", AuthMiddleWare(), func(c *gin.Context) {
	c.JSON(http.StatusOK, gin.H{"data": "home"})
})

```

```
func AuthMiddleWare() gin.HandlerFunc {
    return func(c *gin.Context) {
        if cookie, err := c.Request.Cookie("session_id"); err == nil {
            value := cookie.Value
            fmt.Println(value)
            if value == "123" {
                c.Next()
                return
            }
        }
        c.JSON(http.StatusUnauthorized, gin.H{
            "error": "Unauthorized",
        })
        c.Abort()
        return
    }
}

```
