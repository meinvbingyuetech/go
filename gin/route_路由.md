> *  <a href="#基本用法">导航</a>
> *  <a href="#所有请求">所有请求</a>
> *  <a href="#路由组">路由组</a>

- 基本用法<a name="基本用法"></a>
```go
package main

import "github.com/gin-gonic/gin"

func main() {
	// Disable Console Color
	// gin.DisableConsoleColor()

	// logger and recovery (crash-free) middleware
	router := gin.Default()
  
	router.GET("/ping", func(c *gin.Context) {

		// 输出json
		c.JSON(200, gin.H{
			"message": "pong",
			"status":  "geted",
		})
		
		var msg struct {
			Name    string `json:"user"`
			Message string
			Number  int
		}
		msg.Name = "Lena"
		msg.Message = "hey"
		msg.Number = 123
		c.JSON(http.StatusOK, msg)
		
		// 输出xml
		c.XML(http.StatusOK, gin.H{
			"message": "hey", 
			"status": http.StatusOK
		})
		
		// 输出yaml
		c.YAML(http.StatusOK, gin.H{
			"message": "hey", 
			"status": http.StatusOK
		})

		// 输出text
		c.String(http.StatusOK, "Hello %s", name)

	})
	
	// 输出安全json
	route.SecureJsonPrefix(")]}',\n")
	route.GET("/secureJSON", func(c *gin.Context) {
		names := []string{"lena", "austin", "foo"}
		c.SecureJSON(http.StatusOK, names)
	})
	/* output:
	)]}',
	["lena","austin","foo"]
	*/
	
 	route.Any("/testing", testing)
	
	router.Run() // listen and serve on 0.0.0.0:8080
	// router.Run(":3000")
}

func testing(c *gin.Context) {
	c.String(200, "Success")
}
```

----
- 所有请求<a name="所有请求"></a>
```go
router.GET("/someGet", getting)
router.POST("/somePost", posting)
router.PUT("/somePut", putting)
router.DELETE("/someDelete", deleting)
router.PATCH("/somePatch", patching)
router.HEAD("/someHead", head)
router.OPTIONS("/someOptions", options)
```


----
<a name="路由组"></a>
- 路由组
```go
// Simple group: v1
v1 := router.Group("/v1")
{
	v1.POST("/login", loginEndpoint)
	v1.POST("/submit", submitEndpoint)
	v1.POST("/read", readEndpoint)
}

// Simple group: v2
v2 := router.Group("/v2")
{
	v2.POST("/login", loginEndpoint)
	v2.POST("/submit", submitEndpoint)
	v2.POST("/read", readEndpoint)
}
```

----
- 获取路径<a name="获取路径"></a>
```go
router.GET("/user/:name/*action", func(c *gin.Context) {
	name := c.Param("name")
	action := c.Param("action")

	message := name + " is " + action
})
	
```
----
- 获取参数
```go
// /welcome?firstname=Jane&lastname=Doe
router.GET("/welcome", func(c *gin.Context) {
	firstname := c.DefaultQuery("firstname", "Guest")
	lastname := c.Query("lastname") // shortcut for c.Request.URL.Query().Get("lastname")

	c.String(http.StatusOK, "Hello %s %s", firstname, lastname)
})
```

----
- 表单提交
```go
router.POST("/form_post", func(c *gin.Context) {
	message := c.PostForm("message")
	nick := c.DefaultPostForm("nick", "anonymous")
})
```

---- 
- 单个文件上传
```go
// router.MaxMultipartMemory = 8 << 20  // 8 MiB
router.POST("/upload", func(c *gin.Context) {
	file, _ := c.FormFile("file")
	log.Println(file.Filename)

	// Upload the file to specific dst.
	// c.SaveUploadedFile(file, dst)

})

```
```
curl -X POST http://localhost:8080/upload \
  -F "file=@/Users/appleboy/test.zip" \
  -H "Content-Type: multipart/form-data"
```

---- 
- 多个文件上传
```go
// router.MaxMultipartMemory = 8 << 20  // 8 MiB
router.POST("/upload", func(c *gin.Context) {
	// Multipart form
	form, _ := c.MultipartForm()
	files := form.File["upload[]"]

	for _, file := range files {
		log.Println(file.Filename)

		// Upload the file to specific dst.
		// c.SaveUploadedFile(file, dst)
	}
	c.String(http.StatusOK, fmt.Sprintf("%d files uploaded!", len(files)))
})

```
```
curl -X POST http://localhost:8080/upload \
  -F "upload[]=@/Users/appleboy/test1.zip" \
  -F "upload[]=@/Users/appleboy/test2.zip" \
  -H "Content-Type: multipart/form-data"
```

----
- 静态文件
```go
router.Static("/assets", "./assets")
router.StaticFS("/more_static", http.Dir("my_file_system"))
router.StaticFile("/favicon.ico", "./resources/favicon.ico")
```

----
- 跳转
```go
router.GET("/test", func(c *gin.Context) {
	c.Redirect(http.StatusMovedPermanently, "http://www.google.com/")
})
```
