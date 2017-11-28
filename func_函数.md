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
```go
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
 
// 输出 1 2
```

----

```go
func main() {
   /* 定义局部变量 */
   var a int = 100
   var b int = 200
   var ret int

   /* 调用函数并返回最大值 */
   ret = max(a, b)

   fmt.Printf( "最大值是 : %d\n", ret )
}

/* 函数返回两个数的最大值 */
func max(num1, num2 int) int {
   /* 定义局部变量 */
   var result int

   if (num1 > num2) {
      result = num1
   } else {
      result = num2
   }
   return result 
}
```
----
```go
func main(){
   /* 声明函数变量 */
   getSquareRoot := func(x float64) float64 {
      return math.Sqrt(x)
   }

   /* 使用函数 */
   fmt.Println(getSquareRoot(9))

}
```

----

```go
func swap(x, y string) (string, string) {
   return y, x
}

func main() {
   a, b := swap("Mahesh", "Kumar")
   fmt.Println(a, b)
}
```
