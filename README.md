# go
 
- 抛出错误
```go
errors.New("set to zero")
```

- 一些坑
```go
num, exists := nums[1]
if !exists {
  num = 2
}
```

----

- 类型断言

如果断言失败一般会导致panic的发生。所以为了防止panic的发生，我们需要在断言前进行一定的判断

```go
value, ok := a.(string)
if !ok {
    fmt.Println("It's not ok for type string")
    return
}
fmt.Println("The value is ", value)
```
----
 
输出字符串

```go
panic(fmt.Sprintf("Unknown gin mode %s", gin.Mode()))
```
