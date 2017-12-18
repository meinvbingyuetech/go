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
