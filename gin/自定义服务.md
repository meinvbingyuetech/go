````go
func main() {
    router := gin.Default()
    http.ListenAndServe(":8080", router)
}
````

````go
func main() {
    router := gin.Default()

    s := &http.Server{
        Addr:           ":8000",
        Handler:        router,
        ReadTimeout:    10 * time.Second,
        WriteTimeout:   10 * time.Second,
        MaxHeaderBytes: 1 << 20,
    }
    s.ListenAndServe()
}

````
