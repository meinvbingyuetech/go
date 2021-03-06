- 方法
```go

func main() {
   var c Circle
   c.radius = 10
   fmt.Println(c.getArea())
}

type Circle struct {
   radius float64  //Circle 类型对象的属性
}

//该 method 属于 Circle 类型对象中的方法
func (c Circle) getArea() float64 {
   return 3.14 * c.radius * c.radius
}

```

----
- 闭包

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

```go
func main() {
	a := []int{1, 2, 3}
	for _, i := range a {
		defer func() {
			fmt.Println(i)
		}()
	}
}
输出 3 3 3

闭包里的非传递参数外部变量值是传引用的，在闭包函数里那个 i 就是外部非闭包函数自己的参数，所以是相当于引用了外部的变量， i 的值执行到第三次是3 ，闭包是地址引用所以打印了3次i地址指向的值，所以是3，3，3
```

```go
func main() {
	a := []int{1, 2, 3}
	for _, i := range a {
		func() {
			fmt.Println(i)
		}()
	}
}
输出 1 2 3
```

----

- 函数作为值
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
