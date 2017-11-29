```
定义接口 interface

定义结构体 struct

实现接口方法

----------------------

var出接口

new出结构体

调用接口方法
```

```go

// main函数
func main() {
	var phone Phone
	phone = new(vivo)
	phone.call()
}


type Phone interface {
	call()
}

type vivo struct {

}

func (v vivo) call() {
	fmt.Println("vivo 手机")
}

```
