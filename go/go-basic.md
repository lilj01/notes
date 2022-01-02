## 1.下载地址

`https://studygolang.com/dl`



## 2. 插件  go 

桌面版 `https://plugins.jetbrains.com/plugin/9568-go/versions`



## 3.变量的定义

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





## 4.内建变量类型

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



## 5.常量的定义

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

  

## 6.条件语句



### if

- if的条件里可以赋值
- if的条件里赋值的变量作用域就在这个if语句里

```go
package main

import (
	"fmt"
	"io/ioutil"
)

func main() {

	readFile()
	readFile2()
	
}

func readFile(){
	const filename = "abc.txt"
	contents, err := ioutil.ReadFile(filename)
	if err != nil {
		fmt.Println(err)
	}else {
		fmt.Printf("%s\n",contents)
	}
}

/*if后的变量只在块内有效*/
func readFile2(){
	const filename = "abc.txt"
	if contents, err := ioutil.ReadFile(filename); err == nil {
		fmt.Printf("%s\n",contents)
	}else {
		fmt.Println(err)
	}
}

```





### switch

- switch后可以没有表达式
- switch可以自动break，出发使用fallthrough



```go
func grade(score int) string {
	g := ""
	switch {
	case score < 0 || score > 100:
		panic(fmt.Sprintf("wrong score: %d", score))
	case score < 60:
		g = "F"
	case score < 80:
		g = "C"
	case score < 90:
		g = "B"
	case score <= 100:
		g = "A"
	default:
		panic(fmt.Sprintf("wrong score: %d", score))
	}
	return g
}
```



### for

- for的条件里不需要括号
- for的条件里可以省略初始条件，结束条件，递增表达式
- 结束条件不写，死循环，没有while，可以用这种方法代替死循环



```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

func main() {

	sum := 0
	for i := 1; i <= 100; i++ {
		sum += i
	}
	fmt.Printf("1到100的和为： %d\n", sum)

	fmt.Println(
		convertToBin(0),
		convertToBin(5),
		convertToBin(13))

	printFile("abc.txt")
}

/**转二进制*/
func convertToBin(n int) string {
	if n == 0 {
		return "0"
	}
	if n < 0 {
		panic("error： n can't <0")
	}
	result := ""
	for ; n > 0; n /= 2 {
		lsb := n % 2
		result = strconv.Itoa(lsb) + result
	}
	return result
}

func printFile(filename string) {
	file, err := os.Open(filename)
	if err != nil {
		panic(err)
	}
	scanner := bufio.NewScanner(file)
	for scanner.Scan() {
		fmt.Println(scanner.Text())
	}
}
```



## 7.函数

- 函数可以返回多个值
- 函数返回多个值可以起名字
- 仅用于非常简单的函数
- 对于调用者而言没有区别
- 没有重载，默认参数值可选参数等花哨说法
- 有可变参数列表
- 返回值类型写后面



```go
package main

import (
	"fmt"
	"math"
	"reflect"
	"runtime"
)

func main() {
	/*函数*/
	fmt.Println(eval(3, 4, "+"))
	/*多返回值*/
	i, err := eval2(3, 4, "x")
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Println(i)
	}

	q, r := div(13, 3)
	fmt.Println(q, r)

	/*函数式编程*/
	applyRes := apply(pow, 3, 4)
	fmt.Println(applyRes)

	/*匿名函数   go没有花哨的lambda写法*/
	fmt.Println(apply(func(a, b int) int {
		return int(math.Pow(float64(a), float64(b)))
	}, 3, 4))

	sum := sum(1, 2, 3, 4)
	fmt.Println(sum)
}

func eval(a, b int, op string) int {
	switch op {
	case "+":
		return a + b
	case "-":
		return a - b
	case "*":
		return a * b
	case "/":
		q, _ := div(a, b)
		return q
	default:
		panic("unsupported op: " + op)
	}
}

/*多返回值*/
func eval2(a, b int, op string) (int, error) {
	switch op {
	case "+":
		return a + b, nil
	case "-":
		return a - b, nil
	case "*":
		return a * b, nil
	case "/":
		q, _ := div(a, b)
		return q, nil
	default:
		return 0, fmt.Errorf("unsupported op: %s", op)
	}
}

func pow(a, b int) int {
	return int(math.Pow(float64(a), float64(b)))
}

/*函数式编程*/
func apply(op func(int, int) int, a, b int) int {
	p := reflect.ValueOf(op).Pointer()
	opFuncName := runtime.FuncForPC(p).Name()
	fmt.Printf("calling function %s with args (%d,%d)\n", opFuncName, a, b)
	return op(a, b)
}

//func div(a, b int) (int, int) {
//	return a / b, a % b
//}

//func div(a, b int) (q, r int) {
//	return a / b, a % b
//}

/*不建议这么写，代码多容易混淆*/
func div(a, b int) (q, r int) {
	q = a / b
	r = a % b
	return
}

/*可变参数*/
func sum(numbers ...int) int {
	s := 0
	for i := range numbers {
		s += numbers[i]
	}
	return s
}
```





## 8.指针

- go中，指针不能运算

- go语言只有值传递一种方式，但是可以配合指针

  ```go
  var a int = 2
  var pa *int = &a
  *pa = 3
  fmt.Println(a)
  
  a是3
  ```



### 参数传递

```c++
值传递，将结果拷贝一份到函数中，不影响原本的值
void pass_by_val(int a){
    a++;
}

引用传递，引用同一个变量，会影响原来的值
void pass_by_ref(int& a){
    a++;
}

after pass_by_val：3
after pass_by_val：4
```









## 9.数组



### 数组的定义

```go
var arr1 [5]int
arr2 := [3]int{1, 2, 3}
arr3 := [...]int{4, 5, 6, 7}
var grid [4][5]int

fmt.Println(arr1, arr2, arr3, grid)
```



### 数组的遍历

```go
for i := 0; i < len(arr3); i++ {
	fmt.Println(arr3[i])
}

for i := range arr3 {
	fmt.Println(arr3[i])
}

for i, v := range arr3 {
	fmt.Println(i, v)
}

for _, v := range arr3 {
	fmt.Println(v)
}
```



### 为什么使用range

- 意义明确，美观
- c++，没有类似能力
- java、python：foreach，不能同时获取i,v



### 数组是值类型（拷贝）

传递过程中，不会影响原数值



### 注意点

- [10]int和[20]int是不同类型
- 调用func f(arr [10]int)会拷贝数组，即值传递
- 在go语言中一般不直接使用数组，使用切片





## 10.切片(slice)



### demo

```
arr:=[...]int{0,1,2,3,4,5,6,7}
s:=arr[2:6]

s的值为{2,3,4,5}
```



 

