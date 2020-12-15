# day2
## Summary
主要是go的基本数据类型及其派生类型
## 基本数据类型
### 布尔型
该类型只能是常量true或者是false
```
var b bool = true
```
### 数字类型
整数或浮点类型
### 字符及字符串
#### 字符串比较
```
a := "gopher"
b := "hello world"
fmt.Println(strings.Compare(a, b)) //-1
fmt.Println(strings.Compare(a, a)) //0
fmt.Println(strings.Compare(b, a)) //1

fmt.Println(strings.EqualFold("GO", "go")) //true
fmt.Println(strings.EqualFold("壹", "一")) //false
```
#### 是否存在某个字符或子串
```
fmt.Println(strings.ContainsAny("team", "i")) //false
fmt.Println(strings.ContainsAny("failure", "u & i")) //true
fmt.Println(strings.ContainsAny("in failure", "s g")) //true
fmt.Println(strings.ContainsAny("foo", "")) //false
fmt.Println(strings.ContainsAny("", "")) //false
```
#### 字符串长度
```
fmt.Println(len("谷歌中国")) //12
```
#### 子串出现次数
```
fmt.Println(strings.Count("cheese", "e")) //3
```
#### 字符串切割
```
fmt.Printf("Fields are: %q", strings.Fields("  foo bar  baz   ")) //Fields are: ["foo" "bar" "baz"]
fmt.Printf("%q\n", strings.Split("foo,bar,baz", ",")) //["foo" "bar" "baz"]
fmt.Printf("%q\n", strings.SplitAfter("foo,bar,baz", ",")) //["foo," "bar," "baz"]
```

### 复数

## 派生类型
### 指针类型（Pointer）
```
package main

import "fmt"

func main() {
   var a int= 20   /* 声明实际变量 */
   var ip *int        /* 声明指针变量 */

   ip = &a  /* 指针变量的存储地址 */

   fmt.Printf("a 变量的地址是: %x\n", &a  )

   /* 指针变量的存储地址 */
   fmt.Printf("ip 变量储存的指针地址: %x\n", ip )

   /* 使用指针访问值 */
   fmt.Printf("*ip 变量的值: %d\n", *ip )
}
```
### 数组类型
```
package main

import "fmt"

func main() {
   var n [10]int /* n 是一个长度为 10 的数组 */
   var i,j int

   /* 为数组 n 初始化元素 */        
   for i = 0; i < 10; i++ {
      n[i] = i + 100 /* 设置元素为 i + 100 */
   }

   /* 输出每个数组元素的值 */
   for j = 0; j < 10; j++ {
      fmt.Printf("Element[%d] = %d\n", j, n[j] )
   }
}
```
### 结构化类型(struct)
```
package main

import "fmt"

type Books struct {
   title string
   author string
   subject string
   book_id int
}


func main() {

    // 创建一个新的结构体
    fmt.Println(Books{"Go 语言", "www.runoob.com", "Go 语言教程", 6495407})

    // 也可以使用 key => value 格式
    fmt.Println(Books{title: "Go 语言", author: "www.runoob.com", subject: "Go 语言教程", book_id: 6495407})

    // 忽略的字段为 0 或 空
   fmt.Println(Books{title: "Go 语言", author: "www.runoob.com"})
}
```
### Channel 类型
```
package main

import "fmt"

func sum(s []int, c chan int) {
	sum := 0
	for _, v := range s {
		sum += v
	}
	c <- sum // send sum to c
}

func main() {
	s := []int{7, 2, 8, -9, 4, 0}

	c := make(chan int)
	go sum(s[:len(s)/2], c)
	go sum(s[len(s)/2:], c)
	x, y := <-c, <-c // receive from c

	fmt.Println(x, y, x+y)
}

```
### 函数类型
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
### 切片类型
```
package main

import "fmt"

func main() {
   var numbers []int
   printSlice(numbers)

   /* 允许追加空切片 */
   numbers = append(numbers, 0)
   printSlice(numbers)

   /* 向切片添加一个元素 */
   numbers = append(numbers, 1)
   printSlice(numbers)

   /* 同时添加多个元素 */
   numbers = append(numbers, 2,3,4)
   printSlice(numbers)

   /* 创建切片 numbers1 是之前切片的两倍容量*/
   numbers1 := make([]int, len(numbers), (cap(numbers))*2)

   /* 拷贝 numbers 的内容到 numbers1 */
   copy(numbers1,numbers)
   printSlice(numbers1)  
}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```
### 接口类型（interface）
```
package main

import (
    "fmt"
)

type Phone interface {
    call()
}

type NokiaPhone struct {
}

func (nokiaPhone NokiaPhone) call() {
    fmt.Println("I am Nokia, I can call you!")
}

type IPhone struct {
}

func (iPhone IPhone) call() {
    fmt.Println("I am iPhone, I can call you!")
}

func main() {
    var phone Phone

    phone = new(NokiaPhone)
    phone.call()

    phone = new(IPhone)
    phone.call()

}
```
### Map 类型
```
package main

import "fmt"

func main() {
    var countryCapitalMap map[string]string /*创建集合 */
    countryCapitalMap = make(map[string]string)

    /* map插入key - value对,各个国家对应的首都 */
    countryCapitalMap [ "France" ] = "巴黎"
    countryCapitalMap [ "Italy" ] = "罗马"
    countryCapitalMap [ "Japan" ] = "东京"
    countryCapitalMap [ "India " ] = "新德里"

    /*使用键输出地图值 */
    for country := range countryCapitalMap {
        fmt.Println(country, "首都是", countryCapitalMap [country])
    }

    /*查看元素在集合中是否存在 */
    capital, ok := countryCapitalMap [ "American" ] /*如果确定是真实的,则存在,否则不存在 */
    /*fmt.Println(capital) */
    /*fmt.Println(ok) */
    if (ok) {
        fmt.Println("American 的首都是", capital)
    } else {
        fmt.Println("American 的首都不存在")
    }
}
```
## 关键字
### 25个关键字或保留字
break default func interface select case defer go map struct chan else goto package switch const fallthrough if range type continue for import return var
### 36 个预定义标识符
append bool byte cap close complex complex64 complex128 uint16 copy false float32 float64 imag int int8 int16 uint32 int32 int64 iota len make new nil panic uint64 print println real recover string true uint uint8 uintptr

## 标识符
标识符用来命名变量、类型等程序实体。一个标识符实际上就是一个或是多个字母(A~ Z和a~ z)数字(0~9)、下划线“_”组成的序列，但是第一个字符必须是字母或下划线而不能是数字。

## reference
1. https://github.com/datawhalechina/go-talent/blob/master/1.%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E3%80%81%E5%85%B3%E9%94%AE%E5%AD%97%E3%80%81%E6%A0%87%E8%AF%86%E7%AC%A6.md
2. https://www.runoob.com/go/go-functions.html
3. https://learnku.com/docs/the-little-go-book/decl_init/3305
4. https://juejin.cn/post/6844903636313571341