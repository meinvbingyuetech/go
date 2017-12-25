```go
/* 声明变量，默认 map 是 nil */
var map_variable map[key_data_type]value_data_type

/* 使用 make 函数 */
map_variable := make(map[key_data_type]value_data_type)
```

----
```go
// 定义
var m map[string]int
m = make(map[string]int)

// 赋值
m["a"] = 1
m["b"] = 2
m["c"] = 3

// 单个访问
fmt.Println(m["a"])

// 循环访问
for key := range m {
  fmt.Printf("key : %s, value : %d", key, m[key])
  fmt.Println()
}

// 判断元素是否存在
value, ok := m["d"]
if ok {
  fmt.Println(value)
} else {
  fmt.Println("no this key")
}

// 删除元素
delete(m, "c")

fmt.Println(len(m))
```
