# day1
## Summary
day1主要是安装及熟悉go语言的基本语法及大体结构
## 环境说明
* windows 10专业版
* go version go1.15.6 windows/amd64
## 安装go
在[studygolang.com](https://studygolang.com/dl)下载后，安装相应的安装说明进行安装
## 运行第一个go程序
hello.go
```
package main

import "fmt"

func main() {
	fmt.Println("Hello, 世界")
}
```
该程序的作用是在terminal打印*Hello, 世界*  
运行如下命令：  
```
go run hello.go
```
结果
```
PS M:\my_note\note\golang\day1> go run .\hello.go
Hello, 世界
```

## package
package main定义了包名。必须在源文件中非注释的第一行指明这个文件属于哪个包。package main表示一个可独立执行的程序，每个 Go 应用程序都包含一个名为 main 的包。
## import
用于导入相应的package，package[参考链接](https://golang.org/pkg/)  
在没网的情况下，可运行*godoc -http=:6060*，然后访问http://localhost:6060，用以获取文档
## func main()
主函数，每个go程序必须有一个主函数
## 注释
/.../ 是注释，在程序执行时将被忽略。//单行注释， /* ... */ 多行注释也叫块注释,不可以嵌套使用，一般用于包的文档描述或注释成块的代码片段。
## I/O
fmt是一个提供系统I/O的package，类比于C语言printf及scanf  
[文档](https://golang.org/pkg/fmt/)

## Reference
1. [Go指南](https://tour.go-zh.org/list)
2. [datawhale-Go初探](https://github.com/datawhalechina/go-talent/blob/master/0.Go%E5%88%9D%E6%8E%A2.md)
3. [Go简易教程](https://learnku.com/docs/the-little-go-book/imports/3302)