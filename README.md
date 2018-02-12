# go

- [配置环境变量](https://github.com/meinvbingyuetech/go/blob/master/install_%E5%AE%89%E8%A3%85.md#配置)

- 命令
```
go env
go version
```

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
