- 基本用法
```go
package main

import "github.com/gin-gonic/gin"

func main() {
	// Disable Console Color
	// gin.DisableConsoleColor()

	// logger and recovery (crash-free) middleware
	router := gin.Default()
  
	router.GET("/ping", func(c *gin.Context) {

		c.JSON(200, gin.H{
			"message": "pong",
			"status":  "geted",
		})

		c.String(http.StatusOK, "Hello %s", name)

	})
 
	router.Run() // listen and serve on 0.0.0.0:8080
	// router.Run(":3000")
}
```
----

- 所有请求
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
- 获取路径
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
