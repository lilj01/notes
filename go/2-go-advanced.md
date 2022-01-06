## 1.面向对象  结构体和方法

- go语言仅支持封装，不支持继承和多态
- go语言没有class，只有struct
- 不论地址还是结构本身，一律使用.来访问成员



### 结构的创建

```go
package main

import "fmt"

type treeNode struct {
	value       int
	left, right *treeNode
}

func createTreeNode(value int) *treeNode {
	return &treeNode{value: value}
}

func main() {
	var root treeNode

	root = treeNode{value: 3}
	root.left = &treeNode{}
	root.right = &treeNode{5, nil, nil}
	root.right.left = new(treeNode)
	root.left.right = createTreeNode(2)
	nodes := []treeNode{
		{value: 3},
		{},
		{6, nil, &root},
	}
	fmt.Println(root, nodes)
}

```



```go
func createTreeNode(value int) *treeNode {
	return &treeNode{value: value}
}
```

- 使用自定义工厂函数
- 注意返回了局部变量的地址，go中是可以的



### 结构创建在堆上还是栈上？

不需要知道，垃圾回收



### 怎么给结构定义一个方法

需要指明接收者

```go
func (node treeNode) print() {
	fmt.Println(node.value)
}
```



### 值传递，setXX方法如何解决

```go
×
func (node treeNode) setValue(value int) {
	node.value = value
}

√
func (node *treeNode) setValue(value int) {
	node.value = value
}
```



### 值接受者vs指针接收者

- 要改变内容必须使用指针接收者
- 结构过大也考虑使用指针接收者
- 一致性：（建议，非必须），如有指针接收者，最好全部保持一致
- 值接收者是go语言特有
- 值、指针接收者对调用者没有影响



### 总结

- 显式定义和命名方法接收者
- 只有使用指针才可以改变结构内容
- nil指针也能调用方法





## 2.封装



### 结构和结构方法

- 名字一般使用CamelCase
- 首字母大写：public  针对包
- 首字母小写：private  针对包
- 每个目录一个包，包名和目录名可以不一致
- main包较特殊，包含可执行入口
- 为结构定义的方法必须在同一个包内
- 可以是不同文件



### 扩展已有类型

- 定义别名
- 使用组合
- 使用内嵌 Embedding：需要省下许多代码



## 3.go语言的依赖管理



- 依赖的概念  这里特指引入外围代码库
- 依赖管理的三个阶段 ：GOPATH GOVENDOR,go mod



### GOPATH  选修

- 默认在用户目录/go  Linux unix ~/go  windows: %USERPROFILE%\go
- 所有项目依赖都在GOPATH下，大
- 历史：google将20亿行代码，900晚个文件放在一个repo



### GOVERDOR 选修

- 每个项目都有自己的verdor目录，存放第三方库
- 大量第三方依赖管理工具：glide，dep，go dep，，，，



### go mod

- 由go命令统一的管理，用户不必关心目录结构
- 初始化 go mod init
- 增加依赖：1：go get   2：import 自动修改
- 更新依赖：go get [@v] , go mod tidy去除多余的依赖
- 将项目迁移到go mod: 1:go mod init 2 :go build ./...
- go build ./...不产生编译文件，检查作用 go install ./...产生



## 4.接口

Something that can do



![](https://bclz_xc.gitee.io/lilj_01-static/go/%E5%A4%A7%E9%BB%84%E9%B8%AD.jpg)

### duck typing

大黄鸭是鸭子吗?

- “像鸭子走路，像鸭子叫（长的像鸭子），那么就是鸭子”
- 描述事物的外部行为而非内部结构
- 严格说go属于结构化类型系统，类似duck typing



### 接口变量里面有什么

- 接口变量自带指针
- 接口变量同样采用值传递，几乎不需要使用接口的指针
- 指针接收者实现智能以指针方式使用；值接收者都可以；



### 查看接口变量

- 表示任何类型：interface{}
- Type Assertion 类型断言
- Type Switch



### 常用系统接口

- Stringer  String()  类似 java 中 toString()
- Reader Read(p []bytes)(n int,err error)
- Writer Write(p []bytes)(n int,err error)


