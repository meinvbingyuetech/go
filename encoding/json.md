# Unmarshal 解码
```go
package main
 
import (
    "encoding/json"
    "fmt"
)
 
func main() {
    // json格式字符串
    var jsonUsers = []byte(`[
        {"id": "1", "name": "Anny"},
        {"id": "2", "name": "Tom"}
    ]`)
 
    // 用结构体User进行接收
    // Id属性后的``内的内容表示转化json数据时的一个设定
    // 例如：`json:"id,string"`，表示对应的json格式字符串键值为id，类型为字符串
    // `json:"name"`，表示对应的json格式字符串键值为name
    type User struct {
        Id   int    `json:"id,string"`
        Name string `json:"name"`
    }
 
    // 因为json格式字符串是个数组格式，这里也要定义数组
    var users []User
    err := json.Unmarshal(jsonUsers, &users)
    if err != nil {
        panic(err)
    }
 
    fmt.Printf("%+v\n", users)
    // output: [{Id:1 Name:Anny} {Id:2 Name:Tom}]
}
```

```go
package main
 
import (
    "encoding/json"
    "fmt"
)
 
func main() {
    // json格式字符串
    var jsonUsers = []byte(`[
        {"id": "1", "name": "Anny"},
        {"id": "2", "name": "Tom"}
    ]`)
 
    // 尝试用字典进行接收
    // 因为json格式的键通常为字符串类型
    // 值有可能为各种类型，如果整形、浮点、数组、对象等
    // 所以map存储的值定义为interface{}类型
    var users []map[string]interface{}
    err := json.Unmarshal(jsonUsers, &users)
    if err != nil {
        panic(err)
    }
 
    fmt.Printf("%+v\n", users)
    // output: [map[id:1 name:Anny] map[id:2 name:Tom]]
}
```

----

# Marshal 编码
```go
package main
 
import (
    "encoding/json"
    "fmt"
)
 
func main() {
    // 声明一个结构体
    type ColorGroup struct {
        ID     int      `json:"id,string"`
        Name   string   `json:"name,omitempty"`
        Colors []string `json:"colors"`
    }
 
    // 对ColorGroup类型的变量进行编码
    group := ColorGroup{
        ID:     1,
        Name:   "Reds",
        Colors: []string{"Crimson", "Red", "Ruby", "Maroon"},
    }
    b, err := json.Marshal(group)
    if err != nil {
        panic(err)
    }
    fmt.Println(string(b))
    // output: {"id":"1","name":"Reds","colors":["Crimson","Red","Ruby","Maroon"]}
 
    // 如果没有设置Name属性值，因为标记为了omitempty属性，则在编码成json的时候会忽略Name属性
    group = ColorGroup{
        ID:     1,
        Colors: []string{"Crimson", "Red", "Ruby", "Maroon"},
    }
    b, err = json.Marshal(group)
    if err != nil {
        panic(err)
    }
    fmt.Println(string(b))
    // output: {"id":"1","colors":["Crimson","Red","Ruby","Maroon"]}
 
    // 如果没有设置Colors值，因为没有omitempty属性，会输出nil
    group = ColorGroup{
        ID:   1,
        Name: "Reds",
    }
    b, err = json.Marshal(group)
    if err != nil {
        panic(err)
    }
    fmt.Println(string(b))
    // output: {"id":"1","name":"Reds","colors":null}
 
}
```

```go
package main
 
import (
    "encoding/json"
    "fmt"
)
 
func main() {
    m := map[string]interface{}{
        "id":      1,
        "name":    "Socrates",
        "friends": []string{"Plato", "Aristotle"},
    }
    b, err := json.Marshal(m)
    if err != nil {
        panic(err)
    }
    fmt.Println(string(b))
}
```
