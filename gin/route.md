```go
package main

import "github.com/gin-gonic/gin"

func main() {
  // Disable Console Color
// gin.DisableConsoleColor()

// Creates a gin router with default middleware:
// logger and recovery (crash-free) middleware
router := gin.Default()
  
	// 基本用法
	router.GET("/ping", func(c *gin.Context) {

		c.JSON(200, gin.H{
			"message": "pong",
		})

		c.String(http.StatusOK, "Hello %s", name)

	})

	// 所有请求
	router.GET("/someGet", getting)
	router.POST("/somePost", posting)
	router.PUT("/somePut", putting)
	router.DELETE("/someDelete", deleting)
	router.PATCH("/somePatch", patching)
	router.HEAD("/someHead", head)
	router.OPTIONS("/someOptions", options)

	// 获取路径
	router.GET("/user/:name/*action", func(c *gin.Context) {
		name := c.Param("name")
		action := c.Param("action")

		message := name + " is " + action
	})

	router.Run() // listen and serve on 0.0.0.0:8080
	// router.Run(":3000")
}

```
