## 下载地址

`https://studygolang.com/dl`



## idea go plugin

桌面版 `https://plugins.jetbrains.com/plugin/9568-go/versions`



## 变量的定义

- 变量名在前，类型在后；

- 有初始值，不像c不确定，java为null int 0，string 空串

- go定义了变量后必须使用，否则报错

- go可以推断出变量的类型，赋予默认值时可以省略类型不写

- := 定义并且赋值，是一种简化的写法，这种写法在函数外不支持使用

- 函数外定义的变量不是全局变量，是包内变量

- go可以用括号省略频繁的 var  如：
  

  ```go
  var (
     h = 1
     k = "s"
  )
  ```

  

- 函数内定义的变量只能在函数内使用





## 内建变量类型

- bool、string
- 整数类型 前有u代表无符号整数 还可以规定长度，没有long类型(u)int  (u)int8   (u)int16   (u)int32   (u)int64   uintptr（指针）
- byte rune（字节类型，不是char一样的1字节，1字节的坑非常多）
- float32 float64 complex64 complex128（数学中的复数）



### 复数回顾

- 规定 了 i = √(-*1*)
- 复数：3+4i  3是实部 4i是虚部 
- |3+4i| = √3² + 4² = 5
- i^2 = -1,i^3=-1,i^4=1，。。。。



### 强制类型转换

- 类型转换是强制的
- var a,b int = 3,4
- var c int = math.Sqrt(a×a+b×b)  ××
- var c int = int(math.Sqrt(float64(a×a+b×b)))  



## 常量的定义

- go语言定义常量不做大写

- const 数值可作为各种类型使用，可以不做强转

  ```go
  func consts() {
  	const fileName string = "abc.txt"
  	const a, b = 3, 4
  	var c int
  	c = int(math.Sqrt(a*a + b*b))
  	fmt.Println(fileName,c)
  }
  ```

- 枚举通常也通过常量方式

  ```go
  func enums() {
  	const (
  		java   = 1
  		php    = 2
  		c      = 3
  		golang = 4
  	)
  }
  
  简化写法,横线可以跳过
  func enums2() {
  	const (
  		java = iota
          _
  		php
  		c
  		golang
  	)
  	fmt.Println(java, php, c, golang)
  }
  
  iota还可以使用表达式
  func enums3() {
  	const (
  		java = 1 << (10 * iota)
  		_
  		php
  		c
  		golang
  	)
  	fmt.Println(java, php, c, golang)
  }
  ```

  
