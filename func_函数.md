```go

func main() {
  var c Circle
	c.radius = 10
	fmt.Println(c.getArea())
}

type Circle struct {
	radius float64
}

func (c Circle) getArea() float64 {
	return 3.14 * c.radius * c.radius
}

```

----
```
func getSequence() func() int {
   i:=0
   return func() int {
      i+=1
     return i  
   }
}

func main(){
   
   nextNumber := getSequence()  

   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
}
```
