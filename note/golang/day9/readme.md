# day9
## Summary
包管理
## 环境变量
```
$ go env
GO111MODULE="auto"
GOPROXY="https://proxy.golang.org,direct"
GONOPROXY=""
GOSUMDB="sum.golang.org"
GONOSUMDB=""
GOPRIVATE=""
```
**GOPROXY**:此环境变量主要用于设计Go Module的代理  
**GOSUMDB**:此环境变量用于在拉取模块的时候保证模块版本数据的一致性。
## 初始化模块
Go Modules的使用方法比较灵活，在目录下包含go.mod文件即可
```
go mod init [module name] //运行后生产go.mod
```
```
//go.mod的内容，以添加gorose为例子
module ModuleName
 
require (
	github.com/gohouse/gorose v1.0.5
)
```
## 拉去新的依赖
```
go get
/*
-d 只下载不安装
-f 只有在你包含了 -u 参数的时候才有效，不让 -u 去验证 import 中的每一个都已经获取了，这对于本地 fork 的包特别有用
-fix 在获取源码之后先运行 fix，然后再去做其他的事情
-t 同时也下载需要为运行测试所需要的包
-u 强制使用网络去更新包和它的依赖包
-v 显示执行的命令
*/
```

## 其他常用命令
```
go mod init  // 初始化go.mod
go mod tidy  // 更新依赖文件
go mod download  // 下载依赖文件
go mod vendor  // 将依赖转移至本地的vendor文件
go mod edit  // 手动修改依赖文件
go mod graph  // 查看现有的依赖结构
go mod verify  // 校验依赖
```