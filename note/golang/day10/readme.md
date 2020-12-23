# day10
## Summary
异常处理
## 简单异常处理
```
package main

import (
	"fmt"
)

// 定义一个 DivideError 结构
type DivideError struct {
	dividee int
	divider int
}

// 实现 	`error` 接口
func (de *DivideError) Error() string {
	strFormat := `
	Cannot proceed, the divider is zero.
	dividee: %d
	divider: 0`
	return fmt.Sprintf(strFormat, de.dividee)
}

// 定义 `int` 类型除法运算的函数
func Divide(varDividee int, varDivider int) (result int, errorMsg string) {
	if varDivider == 0 {
		dData := DivideError{
			dividee: varDividee,
			divider: varDivider,
		}
		errorMsg = dData.Error()
		return
	} else {
		return varDividee / varDivider, ""
	}

}

func main() {

	// 正常情况
	if result, errorMsg := Divide(100, 10); errorMsg == "" {
		fmt.Println("100/10 = ", result)
	}
	// 当被除数为零的时候会返回错误信息
	if _, errorMsg := Divide(100, 0); errorMsg != "" {
		fmt.Println("errorMsg is: ", errorMsg)
	}

}
/*
100/10 =  10
errorMsg is:  
	Cannot proceed, the divider is zero.
	dividee: 100
	divider: 0
*/
```

## 使用panic和recover的异常处理
```
package main

import (
	"fmt"
)

// 最简单的例子
func SimplePanicRecover() {
	defer func() {
		if err := recover(); err != nil {
			fmt.Println("Panic info is: ", err)
		}
	}()
	panic("SimplePanicRecover function panic-ed!")
}

// 当 defer 中也调用了 panic 函数时，最后被调用的 panic 函数的参数会被后面的 recover 函数获取到
// 一个函数中可以定义多个 defer 函数，按照 FILO 的规则执行
func MultiPanicRecover() {
	defer func() {
		if err := recover(); err != nil {
			fmt.Println("Panic info is: ", err)
		}
	}()
	defer func() {
		panic("MultiPanicRecover defer inner panic")
	}()
	defer func() {
		if err := recover(); err != nil {
			fmt.Println("Panic info is: ", err)
		}
	}()
	panic("MultiPanicRecover function panic-ed!")
}

// recover 函数只有在 defer 函数中被直接调用的时候才可以获取 panic 的参数
func RecoverPlaceTest() {
	// 下面一行代码中 recover 函数会返回 nil，但也不影响程序运行
	defer recover()
	// recover 函数返回 nil
	defer fmt.Println("recover() is: ", recover())
	defer func() {
		func() {
			// 由于不是在 defer 调用函数中直接调用 recover 函数，recover 函数会返回 nil
			if err := recover(); err != nil {
				fmt.Println("Panic info is: ", err)
			}
		}()

	}()
	defer func() {
		if err := recover(); err != nil {
			fmt.Println("Panic info is: ", err)
		}
	}()
	panic("RecoverPlaceTest function panic-ed!")
}

// 如果函数没有 panic，调用 recover 函数不会获取到任何信息，也不会影响当前进程。
func NoPanicButHasRecover() {
	if err := recover(); err != nil {
		fmt.Println("NoPanicButHasRecover Panic info is: ", err)
	} else {
		fmt.Println("NoPanicButHasRecover Panic info is: ", err)
	}
}

// 定义一个调用 recover 函数的函数
func CallRecover() {
	if err := recover(); err != nil {
		fmt.Println("Panic info is: ", err)
	}
}

// 定义个函数，在其中 defer 另一个调用了 recover 函数的函数
func RecoverInOutterFunc() {
	defer CallRecover()
	panic("RecoverInOutterFunc function panic-ed!")
}

func main() {
	SimplePanicRecover()
	MultiPanicRecover()
	RecoverPlaceTest()
	NoPanicButHasRecover()
	RecoverInOutterFunc()
}
/*
Panic info is:  SimplePanicRecover function panic-ed!
Panic info is:  MultiPanicRecover function panic-ed!
Panic info is:  MultiPanicRecover defer inner panic
Panic info is:  RecoverPlaceTest function panic-ed!
recover() is:  <nil>
NoPanicButHasRecover Panic info is:  <nil>
Panic info is:  RecoverInOutterFunc function panic-ed!
*/
```