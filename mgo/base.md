```
获取

go get gopkg.in/mgo.v2

连接

session, err := mgo.Dial(url)

切换数据库

db := session.DB("test")

切换集合

通过Database.C()方法切换集合（Collection）。

func (db Database) C(name string) Collection

插入

func (c *Collection) Insert(docs ...interface{}) error
1
c := session.DB("store").C("books")  
err = c.Insert(book)  

查询

func (c Collection) Find(query interface{}) Query

更新

c := session.DB("store").C("books")  
err = c.Update(bson.M{"isbn": isbn}, &book)

查询所有

c := session.DB("store").C("books")

var books []Book
err := c.Find(bson.M{}).All(&books)

删除

c := session.DB("store").C("books")
err := c.Remove(bson.M{"isbn": isbn})

应用

package main

import (
    "fmt"
    "log"

    "gopkg.in/mgo.v2"
    "gopkg.in/mgo.v2/bson"
)

type Person struct {
    Name  string
    Phone string
}

func main() {
    session, err := mgo.Dial("localhost:27017")
    if err != nil {
        panic(err)
    }
    defer session.Close()

    // Optional. Switch the session to a monotonic behavior.
    session.SetMode(mgo.Monotonic, true)

    c := session.DB("test").C("people")
    err = c.Insert(&Person{"superWang", "13478808311"},
        &Person{"David", "15040268074"})
    if err != nil {
        log.Fatal(err)
    }

    result := Person{}
    err = c.Find(bson.M{"name": "superWang"}).One(&result)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Println("Name:", result.Name)
    fmt.Println("Phone:", result.Phone)
}
```
