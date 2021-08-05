#  Go语言开发环境搭建

## 安装go开发包

[下载地址](https://golang.org/doc/)



# 编译

### 使用go build编译成二进制文件

1.在项目路径下执行go build,

2.在其他路径上至下go build，需要在后面加上项目路径（src目录下的路径）

3.go build -o xxx （-o:自定义打包文件名称）

# Go语言基本结构

```go
import "fmt"
// 函数外只能防止标识符（变量、常量、函数、类型）的类型
//fmt.Println("Hello") // 非法

//程序的入口函数
func main(){
  fmt.Println("Hello World！")
}
```

# fmt中的三种打印方式

```go
	fmt.Print(isOk)             //终端输出要打印的内容
	fmt.Printf("name:%s", name) //%s:站位符 使用name这个变量去替换占位符
	fmt.Println(age)            //打印完内容会在后面加换位符
```



# 变量和常量

go语言中的变量必须先声明再使用

## 1.变量

1、`var s1 string`:声明一个保存字符串类型数据的s1

2、go语言中变量在非全局中声明了必须使用，不使用无法编译

```go
// 类型推导（根据值判断该变量类型）
var s2 = "20"
//简短变量声明 ,只能在函数里面用
s3 := "哈哈哈" //var s2 = "xxxx"简写
```

```go
//批量声明
var (
	name string // default:""
	age  int    // default:0
	isOk bool		// default:false
)
```

3、匿名变量

 	匿名变量不占用命名空间，不会分配内存，所以匿名变量之间不存在重复声明。

```
func foo() (int, string) {
	return 10, "Q1mi"
}

//匿名变量
x, _ := foo()
_, y := foo()
```

4、多变量声明

````go
//类型相同多个变量, 非全局变量
var vname1, vname2, vname3 type
vname1, vname2, vname3 = v1, v2, v3

var vname1, vname2, vname3 = v1, v2, v3 // 和 python 很像,不需要显示声明类型，自动推断

vname1, vname2, vname3 := v1, v2, v3 // 出现在 := 左侧的变量不应该是已经被声明过的，否则会导致编译错误


// 这种因式分解关键字的写法一般用于声明全局变量
var (
    vname1 v_type1
    vname2 v_type2
)


//多变量可以在同一行进行赋值，如：
var a, b int
var c string
a, b, c = 5, 7, "abc"
````



注意事项：

​		1.函数外的每个语句都必须以关键字开始（var、const、func等）

​		2.:=不能在函数外使用

​		3._多用于占位，表示忽略值

​		4.同一个作用域中不能声明同一个变量名

## 2.常量

### 定义

相对于变量，常量时恒定不变到值，多用于定义程序运行期间不会改变的那些值。常量的声明和变量声明非常类似，只是把`code`换成`const`，常量在定义的时候必须赋值。

```go
const pi = 3.1415926
const e = 2.7182
```

```go
//批量声明常量时，如何某一行声明后没有赋值，默认和上一行一致
const (
	n1 = 100 //100
	n2       //100
	n3       //100
)

```

### iota

`iota`时go语言的常量计数器，只能在常量的表达式中使用。

`iota`在const关键出现时将被重置为0。const中每新增一行常量声明将使`iota	`计数一次（iota可理解为const语句块中的行索引）。使用iota能简化定义，在定义枚举时很有用。

例：

```go
const (
		n1 = iota   // 0
		n2				  // 1
		n3					// 2
		n4					// 3
)
```

```go
package main

import "fmt"

func main() {
    const (
            a = iota   //0
            b          //1
            c          //2
            d = "ha"   //独立值，iota += 1
            e          //"ha"   iota += 1
            f = 100    //iota +=1
            g          //100  iota +=1
            h = iota   //7,恢复计数
            i          //8
    )
    fmt.Println(a,b,c,d,e,f,g,h,i)
}

// 结果0 1 2 ha ha 100 100 7 8
```

iota声明不同行才会 +1，在同一行内不会+1

```go
//多个常量声明在一行
const (
	d1, d2 = iota + 1, iota + 2  // 1 2
	d3, d4 = iota + 1, iota + 2  // 2 3
)
```

定义数量级

```go
//定义数量级
const (
	_  = iota
	KB = 1 << (10 * iota)
	MB = 1 << (10 * iota)
	GB = 1 << (10 * iota)
	TB = 1 << (10 * iota)
	PB = 1 << (10 * iota)
)

```

# Go语言基础之基本数据类型

Go语言中有丰富的数据类型，除了基本的数据类型、浮点型、布尔型、字符串外，还有数组、切片、结构体、函数、map、通道等。Go语言的基本类型和其他语言大同小异。

### 基本数据类型

![WeChate81cefee74e3eb1a433eb742d8216109](./imgs/WeChate81cefee74e3eb1a433eb742d8216109.png)

### 整型

整型分为以下两大类：按长度分为：`int8`、`int16`、`int32`、`int64`，对应的无符号整型：`uint8`、`uint16`、`uint32`、`uint64`

其中，`uint8`就是我们熟知的`byte`型，`int16`对应c语言中的`short`型，`int64`对应c语言中的`long`型。

| 类型 | 描述 |
| :---- | :---- |
|**uint8** | 无符号 8 位整型 (0 到 255)|
|**uint16** | 无符号 16 位整型 (0 到 65535) |
|**uint32** | 无符号 32 位整型 (0 到 4294967295) |
|**uint64** | 无符号 64 位整型 (0 到 18446744073709551615) |
|**int8** | 有符号 8 位整型 (-128 到 127) |
|**int16** | 有符号 16 位整型 (-32768 到 32767) |
|**int32** | 有符号 32 位整型 (-2147483648 到 2147483647) |
|**int64** | 有符号 64 位整型 (-9223372036854775808 到 9223372036854775807) |

### 浮点型

Go语言支持两种浮点型：`float32`和`float64`。这两种浮点型数据格式遵循IEEE754 标准：float32的浮点数的最大范围约为3.4e38，可以使用常量定义：math.MaxFloat32。float64的浮点数最大约为1.8e308，可以使用常量定义：math.MaxFloat64

![WeChatae6bf6b5b1aecab0e5da8c2b91ef39d6](./imgs/WeChatae6bf6b5b1aecab0e5da8c2b91ef39d6.png)

打印浮点数时，可以使用fmt包含动词%f，代码如下

```go
fmt.Printf("%f\n",math.Pi)
fmt.Printf("%.2f\n",math.Pi)
```

### 布尔值

Go语言中以`bool`类型进行声明布尔型数据，布尔型数据只有true（真）和false（假）两个值。

> 注意
>
> 布尔类型变量的默认值为false
>
> Go语言中不允许将整型强制转换为布尔型。
>
> 布尔型无法参加数值运算，也无法与其他类型进行转换。

### 其他数字类型

![WeChat58fcb102e506e011201deaf4fa41d29b](./imgs/WeChat58fcb102e506e011201deaf4fa41d29b.png)

### 字符串

Go语言中字符串是双引号包裹的！！！

Go语言中单引号包裹的是字符！！！

```go
// 字符串
s := "hello 沙河"
// 单独的字母、汉子、符号表示一个字符
c1 := 'h'
c2 := '1'
```

#### 字节编码

+ 字符串的字节默认使用UTF-8编码。
+ 支持Unicode字符

#### 字符串定义

* 双引号（""）：双引号中的转义字符会被替换。
* 反引号（``）：反引号中原生字符串中的转义字符（如\n）会被原样输出。
* 单引号（''）：单引号中的单个字符，需要格式化输出，%c，否则输出字符码数。

#### 字符串长度

+ `len()`：获取的是每个字符的UTF-8编码的***长度和***，而不是直接的字符串数量。如`len("hello,世界")`的结果不是8而是12.

+ `utf8.RuneCountInString()`：获取字符串真正的长度，上面的`utf8.RuneCountInString("hello,世界")`的结果为8.

  (注意)：需要import "unicode/utf8"

#### 字符串拼接

##### 运算符+：

```go
str := "hello, " +
"world"
```

+ 字符串之间用加号”+“拼接。如何换行，+要放在当前行的末尾，不能放在一行的开头。
+ 使用这种拼接方式，里面的字符串是不可变的，每次运算多会产生一个新的、临时的字符串，个GC带来额外的负担，所以性能比较差。

##### fmt.Sprintf()

`fmt.Sprintf("%d:%s",2018,"年")`

+ 内部使用`[]byte`实现，不会产生临时的字符串
+ 内部逻辑复杂，还用到了`interface`，性能一般

##### strings.Join()

```go
strings.Join([]string{"hello","world"},"，")
```

+ Join会先根据**字符串数组**的内容，计算出一个拼接之后的长度，然后申请对应大小的内存，一个一个字符串填入。
+ 已有一个数组的情况下，这种拼接方式的效率很高，但如果没有，去构造这个数据的代价也不小。

##### byte.Buffer——<font color=red>优先推荐</font>

```go
var buffer bytes.Buffer
buffer.WriteString("hello")
buffer.WriteString(", ")
buffer.WriteString("world")
 
fmt.Print(buffer.String())
```

+ 使用这种拼接房方式，可以把字符串当成可变字符使用，对内存的增长也有优化。
+ 如果能预估字符串的长度，还可以用`buffer.Grow()`接口累设置capacity。

##### strings.Builder——<font color=red>推荐</font>

```go 
var b1 strings.Builder
b1.WriteString("ABC")
b1.WriteString("DEF")
 
fmt.Print(b1.String())
```

+ 内部通过slice来保存和管理内容。
+ 提供了Grow()来支持预定义容量，这样可以避免扩容而创建新的slice。
+ 非线程安全，性能上和`bytes.Buffer`相差无几。

### 数组

#### 声明数组

# Go语言流程控制

流程控制是每种编程语言控制逻辑走向和执行次序的重要部分，流程控制可以说是一门语言的”静脉“。

Go语言中最常用的流程控制有`if`和`for`，而`switch`和`goto`主要是为了简化代码、降低重复代码而生的结构，属于扩展类的流程控制。

## <font color=#C76934>if else(分支结构)</font>

<font color=#C76934>if条件判断基本写法</font>

Go语言中`if`条件判断的格式如下：

```go
if 表达式1{
	分支1
}else if 表达式2{
	分支2
}else{
	分支3
}
```

`if`条件判断语句中存在作用域，例如：

```go
if age := 20; age > 18 { //age在if中声明，{}外部无法访问，且乙方流程中的每个{}都有一个作用于（和js不同）
		fmt.Println("成年人")
	} else if age < 18 {
		fmt.Println("未成年")
	} else {
		fmt.Println("刚成年")
	}
	fmt.Println(age) //这里无法访问此时的age
```

## <font color=#C76934>for(循环结构)</font>

Go语言中的所有循环类型均可使用`for`关键字来完成。

for循环的基本格式如下：

```go
for 初始值;条件表达式;结束语句{
	循环体语句
}
```

条件表达式返回`true`是循环体不停地进行循环，直到条件表达式返回`false`时自动退出循环。

```go
func forDemo() {
	for i := 0; i <= 20; i++ {
		fmt.Printf("%d\n", i)
	}
}
```

for循环的初始语句可以省略，可以使用for语句外部的声明变量，但是初始语句后的分号必须要写，例如：

```go
func forDemo(){
	i := 0
	for ; i <= 10; i-- {
		fmt.Println(i)
	}
}
```

for循环的初始语句和结束语句都可以省略,例如：

```go
func forDemo(){
	i := 0
	for i <= 10 {
		fmt.Println(i)
		i ++
	}
}
```

### 无限循环

```go
for {
	循环语句
}
```

for循环可以通过`break`、`goto`、`return`、`panic`语句强制退出循环。

### for range(键值循环)

Go语言中可以使用`for range`遍历数组、切片、字符串、map及通道（channel）。通过`for range`遍历返回值有以下规律：

+ 数组、切片、字符串返回索引和值。
+ map返回键和值。
+ 通道（channel）只返回通道内的值。

```go 
	s := "Hello,沙河"       // 一个字符站1字节，utf8编码的在3字节
	for i, v := range s {  //i为对应的字节位置，v为对应的字节
		fmt.Printf("%d %c\n", i, v)
	}
```

## switch

+ switch 语句用于基于不同条件执行不同动作，每一个 case 分支都是唯一的，从上至下逐一测试，直到匹配为止。
+ switch 语句执行的过程从上至下，直到找到匹配项，匹配项后面也不需要再加 break。
+ switch 默认情况下 case 最后自带 break 语句，匹配成功后就不会执行其他 case，如果我们需要执行后面的 case，可以使用 **fallthrough** 。

```go
switch var1 {
    case val1:
        ...
    case val2:
        ...
    default:
        ...
}
```

+ 变量 var1 可以是任何类型，而 val1 和 val2 则可以是同类型的任意值。类型不被局限于常量或整数，但必须是相同的类型；或者最终结果为相同类型的表达式

![switch](./imgs/switch.png 'switch')

Go语言规定每个`switch`中只能有一个`default`分支。

一个分支可以有多个值，多个case值中间使用英文逗号分隔。

```go
func testSwitch(){
	switch n := 7;n {
		case 1,3,5,7,9:
			fmt.Println("奇数")
		caes 2,4,6,8,10:
			fmt.Println("偶数")
		default:
			fmt.Println(n)
	}
}
```

分支还可以使用表达式，这时候switch语句后面不需要在跟判断变量。例如：

```go
func switchDemo(){
	age := 20
	switch {
		case age <25:
			fmt.Println("好好学习吧")
		case age >= 25 && age < 35:
			fmt.Println("努力工作吧")
		case age >= 35:
			fmt.Println("保养头发吧")
		default:
			fmt.Println("好好活着吧")
	}
}
```

`fallthrough`语法可以执行满足条件的case的下一个case，是为了兼容c语言中的case设计的。

```go
  s := "a"
  switch {
    case s == "a":
    	fmt.Println("a")
    	fallthrough
    case s == "b":
    	fmt.Println("b")
    default:
    	fmt.Println("......")
  }
}
```



输出：

```
a
b
```

`goto`语句通过标签进行代码的无条件跳转。`goto`遇见可以在快速跳出循环、避免重复退出上有一定的帮助。Go语言中使用`goto`语句能简化一些代码的实现过程。例如双层嵌套的for循环要退出时：<font color=red>（尽量少用）</font>

```go
func gotoDemo(){
	var breakFlag bool
	for i:=0;i<10;i++{
		for j:=0;i<10;j++{
			if j == 2 {
				breakFlag = true;
				break
			}
			fmt.Printf("%v-%v\n",i,j)
		}
		//外层for循环判断
		if breakFlag {
			break
		}
	}
}
```

使用`goto`简化代码：<font color=red>（不推荐使用）</font>

```go
func gotoDemo(){
	for i:=0;i<10;i++{
		for j:=0;i<10;j++{
			if j == 2 {
        // 设置退出标签
				goto breakTag
			}
			fmt.Printf("%v-%v\n",i,j)
		}
	}
  return
  //标签
  breakTag:
  	fmt.Println("结束for循环")
}
```

## break跳出循环

`break`语句还可以在语句后面添加标签，表示退出某个标签对应的代码块，标签要求必须定义在对应的`for`、`switch`和`select`的代码块上。例如：

```go
func breakDemo(){
BREAKDEMO:
  for i := 0; i < 10; i++ {
    for j := 0; j < 10; j++{
      if j == 2 {
        	break BREAKDEMO
      }
      fmt.Printf("%v-%v\n",i,j)
    }
  }
  fmt.Println("结束for循环")
}
```

## continue(继续下次循环)

`continue`语句可以在结束当前循环，开始下一次循环迭代过程，仅限在for循环中使用。

在`continue`语句后面添加标签时，表示开始对应的循环。例如

```go
func continueDemo(){
forloop:
  for i := 0; i < 10; i++ {
    for j := 0; j < 10; j++{
      if j == 2 && i == 2 {
        	continue forloop
      }
      fmt.Printf("%v-%v\n",i,j)
    }
  }
}
```

# 运算符

运算符用于在程序运行时执行数学或者逻辑运算。

Go语言内置运算符有：

+ 算术运算符
+ 关系运算符
+ 逻辑运算符
+ 位运算符
+ 赋值运算符
+ 其他运算符

## 算术运算符

下表列出了所有Go语言的算术运算符。假定 A 值为 10，B 值为 20。

| 运算符 | 描述 |
| ------ | ---- |
| +      | 相加 |
| -      | 相减 |
| *      | 相乘 |
| /      | 相除 |
| %      | 求余 |

注意：`++`（自增）和`--`（自减）在Go语言中是单独的语句，并不是运算符。

## 关系运算符

下表列出了所有Go语言的关系运算符。假定 A 值为 10，B 值为 20。

| 运算符 | 描述                                                         | 实例          |
| :----- | :----------------------------------------------------------- | ------------- |
| ==     | 检查两个值是否相等，如果相等返回 True 否则返回 False。       | (A==B)为false |
| !=     | 检查两个值是否不相等，如果不相等返回 True 否则返回 False。   | (A!=B)为true  |
| >      | 检查左边值是否大于右边值，如果是返回 True 否则返回 False。   | (A>B)为false  |
| <      | 检查左边值是否小于右边值，如果是返回 True 否则返回 False。   | (A<B)为true   |
| >=     | 检查左边值是否大于等于右边值，如果是返回 True 否则返回 False。 | (A>=B)为false |
| <=     | 检查左边值是否小于等于右边值，如果是返回 True 否则返回 False。 | (A<=B)为true  |

## 逻辑运算符

下表列出了所有Go语言的逻辑运算符。假定 A 值为 True，B 值为 False。

| 运算符 | 描述                                                         | 实例               |
| ------ | ------------------------------------------------------------ | ------------------ |
| &&     | 逻辑 AND 运算符。 如果两边的操作数都是 True，则条件 True，否则为 False。 | (A && B) 为 False  |
| \|\|   | 逻辑 OR 运算符。 如果两边的操作数有一个 True，则条件 True，否则为 False。 | (A \|\| B) 为 True |
| !      | 逻辑 NOT 运算符。 如果条件为 True，则逻辑 NOT 条件 False，否则为 True。 | !(A && B) 为 True  |

## 位运算符

位运算符对整数在内存中的二进制位进行操作。

下表列出了位运算符 &, |, 和 ^ 的计算：

| p    | q    | p&q  | p\|q | p^q  |
| ---- | ---- | ---- | ---- | ---- |
| 0    | 0    | 0    | 0    | 0    |
| 0    | 1    | 0    | 1    | 1    |
| 1    | 1    | 1    | 1    | 0    |
| 1    | 0    | 0    | 1    | 1    |

Go 语言支持的位运算符如下表所示。假定 A 为60，B 为13：

| **运算符** | **描述**                                                     | **实例**                               |
| ---------- | ------------------------------------------------------------ | -------------------------------------- |
| &          | 按位与运算符"&"是双目运算符。 其功能是参与运算的两数各对应的二进位相与。 | (A & B) 结果为 12, 二进制为 0000 1100  |
| \|         | 按位或运算符"\|"是双目运算符。 其功能是参与运算的两数各对应的二进位相或 | (A\|B) 结果为 61, 二进制为 0011 1101   |
| ^          | 按位异或运算符"^"是双目运算符。 其功能是参与运算的两数各对应的二进位相异或，当两对应的二进位相异时，结果为1。 | (A ^ B) 结果为 49, 二进制为 0011 0001  |
| <<         | 左移运算符"<<"是双目运算符。左移n位就是乘以2的n次方。 其功能把"<<"左边的运算数的各二进位全部左移若干位，由"<<"右边的数指定移动的位数，高位丢弃，低位补0。 | A << 2 结果为 240 ，二进制为 1111 0000 |
| >>         | 右移运算符">>"是双目运算符。右移n位就是除以2的n次方。 其功能是把">>"左边的运算数的各二进位全部右移若干位，">>"右边的数指定移动的位数 | A >> 2 结果为 15 ，二进制为 0000 1111  |

## 赋值运算符

下表列出了所有Go语言的赋值运算符。

| **运算符** | **描述**                                       | **实例**                              |
| ---------- | ---------------------------------------------- | ------------------------------------- |
| =          | 简单的赋值运算符，将一个表达式的值赋给一个左值 | C = A + B 将 A + B 表达式结果赋值给 C |
| +=         | 相加后再赋值                                   | C += A 等于 C = C + A                 |
| -=         | 相减后再赋值                                   | C -= A 等于 C = C - A                 |
| *=         | 相乘后再赋值                                   | C *= A 等于 C = C * A                 |
| /=         | 相除后再赋值                                   | C /= A 等于 C = C / A                 |
| %=         | 求余后再赋值                                   | C %= A 等于 C = C % A                 |
| <<=        | 左移后赋值                                     | C <<= 2 等于 C = C << 2               |
| >>=        | 右移后赋值                                     | C >>= 2 等于 C = C >> 2               |
| &=         | 按位与后赋值                                   | C &= 2 等于 C = C & 2                 |
| ^=         | 按位异或后赋值                                 | C ^= 2 等于 C = C ^ 2                 |
| \|=        | 按位或后赋值                                   | C \|= 2 等于 C = C \| 2               |

## 其他运算符

| **运算符** | **描述**         | **实例**                   |
| ---------- | ---------------- | -------------------------- |
| &          | 返回变量存储地址 | &a; 将给出变量的实际地址。 |
| *          | 指针变量。       | *a; 是一个指针变量         |

## 运算符优先级

| 优先级 | 运算符           |
| ------ | ---------------- |
| 5      | * / % << >> & &^ |
| 4      | + - \| ^         |
| 3      | == != < <= > >=  |
| 2      | &&               |
| 1      | \|\|             |

<font color=green>当然，你可以通过使用括号来临时提升某个表达式的整体运算优先级。</font>

