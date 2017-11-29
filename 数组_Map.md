```go
/* 声明变量，默认 map 是 nil */
var map_variable map[key_data_type]value_data_type

/* 使用 make 函数 */
map_variable := make(map[key_data_type]value_data_type)
```

----

```go
var variable_name [SIZE] variable_type

// 方式1
var arr [3] int
arr[0] = 0
arr[1] = 1
arr[2] = 2

// 方式2
var arr = [5]int {1,2,3,4,5}

// 方式3
var arr = [...]int {1,2,3,4,5}

//--------------------------------------
// 多维 3行4列
arr := [3][4]int {
		{1,2,3,4},
		{5,6,7,8},
		{9,9,9,9},
	}

// 访问多维数组
var a = arr[0][1]
fmt.Println(a)

// 传递到函数
arr := []int {1,2,3,4,5}
fmt.Println(arrLen(arr))
  
func arrLen(arr []int) int {
	return len(arr)
}
```
