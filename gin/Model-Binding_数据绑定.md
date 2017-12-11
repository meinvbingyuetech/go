```go
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
	"github.com/gin-gonic/gin/binding"
)

// Binding from JSON
type Login struct {
	User     	string 		`form:"user" json:"user" binding:"required"`
	Password 	string 		`form:"password" json:"password" binding:"required"`
	Age      	int    		`form:"age" json:"age"`
	CreatedAt   	time.Time 	`form:"-" json:"created_at" db:"created_at"`
	UpdatedAt   	time.Time 	`form:"-" json:"updated_at" db:"updated_at"`
}

type form struct {
    Colors []string `form:"colors[]"`
}

func formHandler(c *gin.Context) {
    var myform form
    c.ShouldBind(&myform)
    c.JSON(200, gin.H{"color": myform.Colors})
}

func main() {
	router := gin.Default()

	// Example for binding JSON {"user": "jason", "password": "123"}
	router.POST("/loginAuto", func(c *gin.Context) {
		var data Login
		if err := c.ShouldBindWith(&data, binding.Form); err == nil {
			if data.User == "jason" && data.Password == "123" {
				c.JSON(http.StatusOK, gin.H{"status": "you are logged in by auto"})
			} else {
				c.JSON(http.StatusUnauthorized, gin.H{"status": "unauthorized"})
			}
		} else {
			c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
		}
	})

	// Example for binding JSON {"user": "manu", "password": "123"}
	router.POST("/loginJSON", func(c *gin.Context) {
		var json Login
		if err := c.ShouldBindJSON(&json); err == nil {
			if json.User == "manu" && json.Password == "123" {
				c.JSON(http.StatusOK, gin.H{"status": "you are logged in by json"})
			} else {
				c.JSON(http.StatusUnauthorized, gin.H{"status": "unauthorized"})
			}
		} else {
			c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
		}
	})

	// Example for binding QUERY ?user=jason&password=123
	router.GET("/loginQuery", func(c *gin.Context) {
		var query Login
		if err := c.ShouldBindQuery(&query); err == nil {
			if query.User == "jason" && query.Password == "123" {
				c.JSON(http.StatusOK, gin.H{"status": "you are logged in by query"})
			} else {
				c.JSON(http.StatusUnauthorized, gin.H{"status": "unauthorized"})
			}
		} else {
			c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
		}
	})

	// Example for binding a HTML form (user=manu&password=123)
	// If `GET`, only `Form` binding engine (`query`) used.
	// If `POST`, first checks the `content-type` for `JSON` or `XML`, then uses `Form` (`form-data`).
	// See more at https://github.com/gin-gonic/gin/blob/master/binding/binding.go#L48
	router.POST("/loginForm", func(c *gin.Context) {
		var form Login
		// This will infer what binder to use depending on the content-type header.
		if err := c.ShouldBind(&form); err == nil {
			if form.User == "manu" && form.Password == "123" {
				c.JSON(http.StatusOK, gin.H{"status": "you are logged in by form"})
			} else {
				c.JSON(http.StatusUnauthorized, gin.H{"status": "unauthorized"})
			}
		} else {
			c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
		}
	})

	// Listen and serve on 0.0.0.0:8080
	router.Run(":8080")
}
```

----
- 示例
```go
type User struct {
    Username string `form:"username" json:"username" binding:"required"`
    Passwd   string `form:"passwd" json:"passwd" bdinding:"required"`
    Age      int    `form:"age" json:"age"`
}

func main(){
    router := gin.Default()
    
    router.POST("/login", func(c *gin.Context) {
        var user User
        var err error
        contentType := c.Request.Header.Get("Content-Type")

        switch contentType {
        case "application/json":
            err = c.BindJSON(&user)
        case "application/x-www-form-urlencoded":
            err = c.BindWith(&user, binding.Form)
        }

        if err != nil {
            fmt.Println(err)
            log.Fatal(err)
        }

        c.JSON(http.StatusOK, gin.H{
            "user":   user.Username,
            "passwd": user.Passwd,
            "age":    user.Age,
        })

    })

}
```
