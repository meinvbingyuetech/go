```go
	r := Res{
		Name:"jason",
		Age:22,
	}

	c.JSON(
		200,
		struct {
			responseFormat
			Data Res `json:"data"`
		}{
			responseFormat : responseFormat{
				Msg : "成功",
				Code: 0,
			},
			Data : r,
		},
	)
```


```
package main

import (
    "fmt"
)

type User struct {
    Id   int
    Name string
    Age  int
}

type Manger struct {
    User
    title string
}

func main() {
    m1 := Manger{User:User{1, "ok", 12}, title:"123"}  //可以
    m2 := Manger{User{1, "ok", 12}, "123"}  //可以
    m3 := Manger{User:User{Id:1, Name:"ok", Age:12}, title:"123"}  //可以
    
    fmt.Println(m1)
    fmt.Println(m2)
    fmt.Println(m3)
    
}
```

```
type User struct {
  Id        int       `json:"id"`
  Name      string    `json:"name"`
  Bio       string    `json:"about,omitempty"`
  Active    bool      `json:"active"`
  Admin     bool      `json:"-"`
  CreatedAt time.Time `json:"created_at"`
}

{
  "id": 1,
  "name": "John Doe",
  "about": "Some Text",
  "active": true,
  "created_at": "2016-07-16T15:32:17.957714799Z"
}

**************************************
package main
 
import (
  "fmt"
  "reflect"
  "encoding/json"
)
 
// Name of the struct tag used in examples
const tagName = "validate"
 
type User struct {
  Id    int    `validate:"-" json:"id" bson:"_id"`
  Name  string `validate:"presence,min=2,max=32" json:"name" bson:"name"`
  Email string `validate:"email,required" json:"email" bson:"email"`
}
 
func main() {
  user := User{
    Id:    1,
    Name:  "John Doe",
    Email: "john@example",
  }

  // 输出json格式
  u := &User{Id: 1, Name: "tony"}
  j, _ := json.Marshal(u)
  fmt.Println(string(j))
 
  // TypeOf returns the reflection Type that represents the dynamic type of variable.
  // If variable is a nil interface value, TypeOf returns nil.
  t := reflect.TypeOf(user)
 
  // Get the type and kind of our user variable
  fmt.Println("Type:", t.Name())
  fmt.Println("Kind:", t.Kind())
 
  // Iterate over all available fields and read the tag value
  for i := 0; i < t.NumField(); i++ {
    // Get the field, returns https://golang.org/pkg/reflect/#StructField
    field := t.Field(i)
 
    // Get the field tag value
    tag := field.Tag.Get(tagName)
 
    fmt.Printf("%d. %v (%v), tag: '%v'\n", i+1, field.Name, field.Type.Name(), tag)
  }
}


Type: User
Kind: struct
1. Id (int), tag: '-'
2. Name (string), tag: 'presence,min=2,max=32'
3. Email (string), tag: 'email,required'


******************
type S struct {
   F string `species:"gopher" color:"blue"`
}

s := S{}
st := reflect.TypeOf(s)
field := st.Field(0)
fmt.Println(field.Tag.Get("color"), field.Tag.Get("species"))



*******************

package main
 
import (
    "encoding/json"
    "fmt"
    "reflect"
)
 
func main() {
    type User struct {
        UserId   int    `json:"user_id" bson:"user_id"`
        UserName string `json:"user_name" bson:"user_name"`
    }
    // 输出json格式
    u := &User{UserId: 1, UserName: "tony"}
    j, _ := json.Marshal(u)
    fmt.Println(string(j))
    // 输出内容：{"user_id":1,"user_name":"tony"}
 
    // 获取tag中的内容
    t := reflect.TypeOf(u)
    field := t.Elem().Field(0)
    fmt.Println(field.Tag.Get("json"))
    // 输出：user_id
    fmt.Println(field.Tag.Get("bson"))
    // 输出：user_id
}
```
