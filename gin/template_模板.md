```go

func main() {
	router := gin.Default()
  
	router.LoadHTMLGlob("templates/*")
	//router.LoadHTMLFiles("templates/template1.html", "templates/template2.html")
  
	router.GET("/index", func(c *gin.Context) {
		c.HTML(http.StatusOK, "index.tmpl", gin.H{
			"title": "Main website",
		})
	})
  
	router.Run(":8080")
}

```

```html
<html>
	<h1>
		{{ .title }}
	</h1>
</html>
```
