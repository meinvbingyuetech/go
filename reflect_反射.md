## 注意，结构体的字段和方法必须是公开的，即首写字母大写，才能反射

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {

	var humen Humen
	humen.Name = "jason"
	humen.Age = 30

	t := reflect.TypeOf(humen)
	//t := reflect.TypeOf(&humen)  // 如果这样传递，则会报类型不正确

	// 判断类型
	if k := t.Kind(); k != reflect.Struct {
		fmt.Println("类型不正确")
	}

	v := reflect.ValueOf(humen)

	for i := 0; i < t.NumField(); i++ {
		field := t.Field(i)
		field_name := field.Name
		field_type := field.Type
		field_value := v.Field(i).Interface()

		fmt.Printf("%6v:%v = %v\n", field_name, field_type, field_value)
	}

	for i := 0; i < t.NumMethod(); i++ {
		method_name := t.Method(i).Name
		method_type := t.Method(i).Type
		fmt.Printf("%6s: %v\n", method_name, method_type)
	}
}

type Humen struct {
	Name string
	Age int
}


func (h Humen) SayHi (str string) string {
	return fmt.Sprint("hello "+ str)
}

```

```
  Name:string = jason
   Age:int = 30
 SayHi: func(main.Humen, string) string
```
----

## 利用反射修改值
```
package main

import (
	"fmt"
	"reflect"
)

func main() {

	var humen Humen
	humen.Name = "jason"
	humen.Age = 30

	v := reflect.ValueOf(&humen)

	// 初始值
	fmt.Println(v)

	if v.Kind() == reflect.Ptr && !v.Elem().CanSet() {
		fmt.Println("不允许设置值")
		return
	} else {
		v = v.Elem() // 重新赋值
	}

	f := v.FieldByName("Name")
	if !f.IsValid() {
		fmt.Println("字段不存在")
		return
	}

	if f.Kind() == reflect.String {
		f.SetString("meinvbingyue")
	}

	// 修改过后的值
	fmt.Println(v)
}

type Humen struct {
	Name string
	Age int
}

```
