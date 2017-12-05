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
