# day11
## Summary
反射机制
## 反射是什么
> Go 语言提供了一种机制在运行时更新变量和检查它们的值、调用它们的方法，但是在编译时并不知道这些变量的具体类型，这称为反射机制。

## 从interface值到反射对象
```
package main

import (
    "fmt"
    "reflect"
)

func main() {
    var x float64 = 3.4
    fmt.Println("type:", reflect.TypeOf(x))
}
/*
type: float64
*/
```
```
var x float64 = 3.4
fmt.Println("value:", reflect.ValueOf(x).String())
/*
value: <float64 Value>
*/
```
```
var x float64 = 3.4
v := reflect.ValueOf(x)
fmt.Println("type:", v.Type())
fmt.Println("kind is float64:", v.Kind() == reflect.Float64)
fmt.Println("value:", v.Float())
/*
type: float64
kind is float64: true
value: 3.4
*/
```
```
var x uint8 = 'x'
v := reflect.ValueOf(x)
fmt.Println("type:", v.Type())                            // uint8.
fmt.Println("kind is uint8: ", v.Kind() == reflect.Uint8) // true.
x = uint8(v.Uint())      
/*
type MyInt int
var x MyInt = 7
v := reflect.ValueOf(x)
*/
```
## 从反射对象到接口值
```
// Interface returns v's value as an interface{}.
func (v Value) Interface() interface{}
```
```
y := v.Interface().(float64) // y will have type float64.
fmt.Println(y) //
fmt.Println(v.Interface())
fmt.Printf("value is %7.1e\n", v.Interface()) //3.4e+00
```
## To modify a reflection object, the value must be settable.
```
var x float64 = 3.4
v := reflect.ValueOf(x)
v.SetFloat(7.1) // Error: will panic.
```

## 通过反射修改内容
```
var f float64 = 3.41
fmt.Println(f)
p := reflect.ValueOf(&f)
v := p.Elem()
v.SetFloat(6.18)
fmt.Println(f)
```
## 通过反射调用方法
```
package main

import (
        "fmt"
        "reflect"
)

func hello() {
  fmt.Println("Hello world!")
}

func main() {
  hl := hello
  fv := reflect.ValueOf(hl)
  fv.Call(nil)
}
```