# day7
## Summary
函数
## 函数定义
```
func functionName([parameter list]) [returnTypes]{
   //body
}
//函数由func关键字进行声明。
//functionName：代表函数名。
//parameter list：代表参数列表，函数的参数是可选的，可以包含参数也可以不包含参数。
//returnTypes：返回值类型，返回值是可选的，可以有返回值，也可以没有返回值。
//body：用于写函数的具体逻辑
```
### 例子
```
package main

import "fmt"

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
## 变长参数
在对函数进行传参时，函数接受的传参根据实际传入长度进行变化
```
func main() {
	slice := []int{7, 9, 3, 5, 1}
	x := min(slice...)
	fmt.Printf("The minimum is: %d", x)
}

func min(s ...int) int {
	if len(s) == 0 {
		return 0
	}
	min := s[0]
	for _, v := range s {
		if v < min {
			min = v
		}
	}
	return min
}
```
## 函数返回多个值
func div(a, b float64) (float64, error) {
	if b == 0 {
		return 0, errors.New("The divisor cannot be zero.")
	}
	return a / b, nil
}

func main() {
	result, err := div(1, 2)
	if err != nil {
		fmt.Printf("error: %v", err)
		return
	}
	fmt.Println("result: ", result)
}
## 命名返回值
```
func div(a, b float64) (result float64, err error) {
	if b == 0 {
		return 0, errors.New("被除数不能等于0")
	}
	result = a / b
	return
}
```

## 匿名函数
```
func main() {
	f := func() string {
		return "hello world"
	}
	fmt.Println(f())
}
```
## 闭包
Go 语言支持匿名函数，可作为闭包。匿名函数是一个"内联"语句或表达式。匿名函数的优越性在于可以直接使用函数内的变量，不必申明。
```
package main

import "fmt"

func getSequence() func() int {
   i:=0
   return func() int {
      i+=1
     return i  
   }
}

func main(){
   /* nextNumber 为一个函数，函数 i 为 0 */
   nextNumber := getSequence()  

   /* 调用 nextNumber 函数，i 变量自增 1 并返回 */
   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
   
   /* 创建新的函数 nextNumber1，并查看结果 */
   nextNumber1 := getSequence()  
   fmt.Println(nextNumber1())
   fmt.Println(nextNumber1())
}
/*
1
2
3
1
2
*/
```
