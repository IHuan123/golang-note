[参照文档地址](http://www.topgoer.com/)     [参照视频](https://www.bilibili.com/video/BV16E411H7og)

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

## 基本数据类型

![WeChate81cefee74e3eb1a433eb742d8216109](./imgs/WeChate81cefee74e3eb1a433eb742d8216109.png)

## 整型

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

## 浮点型

Go语言支持两种浮点型：`float32`和`float64`。这两种浮点型数据格式遵循IEEE754 标准：float32的浮点数的最大范围约为3.4e38，可以使用常量定义：math.MaxFloat32。float64的浮点数最大约为1.8e308，可以使用常量定义：math.MaxFloat64

![WeChatae6bf6b5b1aecab0e5da8c2b91ef39d6](./imgs/WeChatae6bf6b5b1aecab0e5da8c2b91ef39d6.png)

打印浮点数时，可以使用fmt包含动词%f，代码如下

```go
fmt.Printf("%f\n",math.Pi)
fmt.Printf("%.2f\n",math.Pi)
```

## 布尔值

Go语言中以`bool`类型进行声明布尔型数据，布尔型数据只有true（真）和false（假）两个值。

> 注意
>
> 布尔类型变量的默认值为false
>
> Go语言中不允许将整型强制转换为布尔型。
>
> 布尔型无法参加数值运算，也无法与其他类型进行转换。

## 其他数字类型

![WeChat58fcb102e506e011201deaf4fa41d29b](./imgs/WeChat58fcb102e506e011201deaf4fa41d29b.png)

## 字符串

Go语言中字符串是双引号包裹的！！！

Go语言中单引号包裹的是字符！！！

```go
// 字符串
s := "hello 沙河"
// 单独的字母、汉子、符号表示一个字符
c1 := 'h'
c2 := '1'
```

### 字节编码

+ 字符串的字节默认使用UTF-8编码。
+ 支持Unicode字符

### 字符串定义

* 双引号（""）：双引号中的转义字符会被替换。
* 反引号（``）：反引号中原生字符串中的转义字符（如\n）会被原样输出。
* 单引号（''）：单引号中的单个字符，需要格式化输出，%c，否则输出字符码数。

### 字符串长度

+ `len()`：获取的是每个字符的UTF-8编码的***长度和***，而不是直接的字符串数量。如`len("hello,世界")`的结果不是8而是12.

+ `utf8.RuneCountInString()`：获取字符串真正的长度，上面的`utf8.RuneCountInString("hello,世界")`的结果为8.

  (注意)：需要import "unicode/utf8"

### 字符串拼接

##### 运算符+：

```go
str := "hello, " +
"world"
```

+ 字符串之间用加号”+“拼接。如何换行，+要放在当前行的末尾，不能放在一行的开头。
+ 使用这种拼接方式，里面的字符串是不可变的，每次运算多会产生一个新的、临时的字符串，个GC带来额外的负担，所以性能比较差。

#### fmt.Sprintf()

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

## 数组

数组是同一种数据类型元素的集合。在Go语言中，数组从声明时就确认，使用时可以修改数组成员，但是数组大小不可变化，基本语法：

```go
//定义一个长度为3元素类型为int的数组a.
var a [3]int
```

### 声明数组(数组定义)

```go
var 数组变量名 [元素数量]T
```

比如：`var a [5]int`，数组的长度必须时变量，并且长度是数组的一部分。一旦定义，长度不能变。`[5]int`和`[10]int`是不同的类型。

```go
var a [3]int
var b [4]int
a = b //不可以这样做，因为此时a和b是不同的类型
```

数组之间可以通过下标从0开始，最后一个元素下标：`len-1`，访问越界（下标在合法范围之外），则触发访问越界，会panic。

+ 数组元素必须类型唯一，要么都是字符串，要么都是数字类型。如果想让数组元素类型为任意类型，可以是用空接口interface{}作为类型，当使用值时必须先做一个类型判断。

+ 声明时需要确定长度，如何采用不定长度的声明方式声明，在初始化时会自动确认好数组的长度。

+ | ① var arr [2]int //var 数组名称 [数组长度]数组元素类型       |
  | ------------------------------------------------------------ |
  | ② var arr = [2]int{1,2} //var 数组名称 = [数组长度]数组元素类型{元素1,元素2,...} |
  | ③ var arr = [5]string{3: "ab", 4: "cd"} //指定索引位置       |
  | ④ var arr = [...]int{1,2} //var 数组名称 = [...]数组元素类型{元素1,元素2,...}，不定长数组声明方式 |
  | ⑤ new()：创建的是**数组引针**,eg. var arr1 = new([5]int)     |

### 数组的初始化

```go
	//数组初始化
	//如果不初始化：默认元素都为零值（布尔值：false，整型和浮点型都是0，字符串：""）
	fmt.Println(a1, a2)
	//1、初始化方式1
	a1 = [3]bool{true, true, false}
	fmt.Println(a1)
	//2.初始化2
	//根据初始值自动推断数组长度
	a10 := [...]int{1, 2, 3, 4, 5}
	fmt.Println(len(a10))
	//3.初始化3
	//根据索引来初始化（剩余的用0来补充）
	a3 := [5]int{0: 1, 4: 2}
	fmt.Println(a3)
```

### 值传递和引用传递

```go
var arr1 = new([5]int)
arr := arr1
arr1[2] = 100
fmt.Println(arr1[2],arr[2])

var arr2 [5]int
newarr := arr2
arr2[2] = 100
fmt.Println(arr2[2],newarr[2])

// 程序输出
// 100 100
// 100 0
```

+ new([5]int)创建的是数组指针，arr和arr1指向同一地址，故而修改arr1时arr同样也生效；

+ 而newarr是由arr2值传递（拷贝），故而修改任何一个都不会改变另一个的值。

  

### 数组的遍历

```go
	//数组的遍历
	citys := [...]string{"北京", "上海", "成都"}
	//1.根据索引遍历
	for i := 0; i < len(citys); i++ {
		fmt.Println(citys[i])
	}
	//2.for range遍历
	for i, v := range citys {
		fmt.Println(i, v)
	}
```

### 多维数组

```go
//多维数组
	// [[x,x],[x,x],[x,x]]
	var a11 [3][2]int
	a11 = [3][2]int{
		[2]int{1, 2},
		[2]int{3, 4},
		[2]int{5, 6},
	}
	fmt.Println(a11)
	//多维数组的遍历
	for _, v := range a11 {
		for _, v2 := range v {
			fmt.Println(v2)
		}
	}
```

### 数组是值类型

数组是值类型，赋值和传参会复制整个数组。因此改变副文本的值，不会改变本身的值。

### 注意

1.数组支持"=="、"!="操作符，因为内存总是被初始化过的。

1.`[n]*T`表示指针数组，`*[n]T`表示数组指针。

## 切片（slice）

切片(slice)是 Golang 中一种比较特殊的数据结构，这种数据结构更便于使用和管理数据集合。切片是围绕动态数组的概念构建的，可以按需自动增长和缩小。切片的动态增长是通过内置函数 append() 来实现的，这个函数可以快速且高效地增长切片，也可以通过对切片再次切割，缩小一个切片的大小。因为切片的底层也是在连续的内存块中分配的，所以切片还能获得索引、迭代以及为垃圾回收优化的好处。

### 定义

+ 切片是引用（对底层数组一个连续片段的引用），不需要占用额外的内存，比数组效率高。
+ 一个切片为初始化前默认为null，长度为0.
+ 切片的长度可变，可以把切片看出一个长度可变的数组。

### 声明

```go
① 切分数组

var arr = [5]int{1,2,3,4,5} //数组

var s []type = arr[start:end:max] //切片（包含数组start到end-1之间的元素）,end-start表示长度，max-start表示容量

② 初始化

var s = []int{2, 3, 5, 7, 11} //创建了一个长度为 5 的数组并且创建了一个相关切片。

③ 切片重组

var s []type = make([]type, len, cap) //len 是数组的长度也是切片的初始长度,cap是容量，可选参数

简写为slice := make([]type, len)
```

### 切片的长度和容量

切片拥有自己的长度和容器，我们可以通过使用内置的`len()`函数求长度，使用内置的`cap()`函数求切片的容量。

切片是一个很小的对象，它对底层的数组(内部是通过数组保存数据的)进行了抽象，并提供相关的操作方法。切片是一个有三个字段的数据结构，这些数据结构包含 Golang 需要操作底层数组的元数据：

![切片的长度和容量](./imgs/slice.png)

这 3 个字段分别是指向底层数组的指针、切片访问的元素的个数(即长度)和切片允许增长到的元素个数(即容量)。

**特点**

* 切片指向一个底层的数组。
* 切片的长度就是它元素的个数。
* 切片的容器是底层数组从切片的第一个元素到最后一个元素的数量。
* 

### 基于数组定义切片

由于切片的底层就是一个数组，所有我梦可以基于数组定义切片。

```go
func main(){
	// 基于数组定义切片
  a := [5]int{1,2,3,4,5}
  b := a[1:4]    //基于数组a创建切片，包含元素a[1],a[2],a[3]
  fmt.Println(b) //[2 3 4]
  fmt.Printf("type of b:%T",b) //type of b:[]int
}
```

还支持如下方式：

```go
c := a[1:] //从a[1]到末尾
c := a[:3] //从开始到a[3]
e := a[:] //从开始到末尾
```

总结：如何在剪切数组是，开始（或者结束）位置没有的话，默认为开头（或者末尾）位置进行剪切。

### 使用make( )函数创建切片

我们上面是基于数组来创建切片，如何需要动态的创建一个切片，我们就需要使用内置的`make()`函数，格式如下：

```go
var s []type = make([]type, len, cap) //len 是数组的长度也是切片的初始长度,cap是容量，可选参数,如何省略cap参数，cap就默认为len

```

### 切片的本质

切片就是一个框，框住了一块连续的内存。属于引用类型，真正的数据都保存在底层数组中。

### 切片不能直接比较

切片之间是不能直接比较的，我们不能使用`==`操作符来判断两个切片是否含有全部相等元素。切片唯一合法的比较操作是和`nil`比较。一个`nil`值的切片并没有底层数组，一个`nil`值的切片的长度和容器都是0。但是我们不能说一个长度和容器都是0的切片一定是`nil`,例如下面示例：

```go
var s1 []int				// len(s1)=0;cap(s1)=0;s1==nil
s2 := []int{}				// len(s2)=0;cap(s2)=0;s2!=nil
s3 := make([]int,0) // len(s3)=0;cap(s3)=0;s3!=nil
```

所以要判断一个切片是否是空，要使用`len(s) == 0`来判断，不应该使用`s == nil`来判断。

### 切片的赋值拷贝

下面的代码中演示了拷贝前后两个变量共享底层数组，对一个切片的修改会影响另一个切片的内容，这点需要特别注意。

```go
func main() {
	s1 := make([]int, 3)
	s2 := s1   			//将s1直接赋值给s2，s1和s2共用一个底层数组
	s2[0] = 100
	fmt.Println(s1) //[100 0 0]
	fmt.Println(s2) //[100 0 0]
}
```

### 切片的遍历

```go
func main() {
	//切片的遍历
	s3 := make([]int, 5, 10)
	for i := 0; i < len(s3); i++ {
		fmt.Println(i)
	}
	for i, v := range s3 {
		fmt.Println(i, v)
	}
}
```

### append()方法为切片添加元素

Go语言的内建函数`append`可以为切片动态添加元素。每个切片会指向一个底层数组，这个数字能容纳一定数量的元素。当底层数组不能容纳新增的元素时，切片就会自动按照一定的策略进行扩容，此时该切片指向的底层数组就会更换。“扩容”操作往往发生在`append()`函数。举个例子：

```go
func main{
	var numSlice []int
  for i:=0;i<10;i++{
    numSlice = append(numSlice,i)
    fmt.Printf("%v len:%d cap:%d ptr:%p\n",numSlice,len(numSlice),cap(numSlice),numSlice)
  }
}
```

### 切片的扩容规则

+ 首先判断，如何新申请容量（cap）大于2倍就容量（old.cap），最终容量（newcap）就是新申请的容量（cap）。
+ 否则判断，如何旧切片的长度小于1024，则最终容量（newcap）就是旧容量（old.cap）的两倍，即（newcap=doublecap）。
+ 否则判断，如何旧切片长度大于等于1024，则最终容量（newcap）从旧容量（old.cap）开始循环增加原来的1/4，即（newcap=old.cap，for{newcap += new cap/4}）直到最终容量（newcap）大于等于新申请的容量（cap），即（newcap=>cap）
+ 如果最终容量（cap）计算值溢出，则最终容量（cap）就是新申请容量（cap）。

![cap](/Volumes/D/Golang笔记/Golang-/imgs/cap.png)

需要注意的事，切片扩容还会根据切片中元素的类型不同而做不同的处理，比如`int`和`strint`类型处理方式就不一样。

### 使用copy()函数复制切片

由于切片类型是引用类型，所以a和b其实都指向了同一块内存地址。修改b的同时a的值也会发生变化。

Go语言内建的`copy()`函数可以迅速的将一个切片的数据复制到另一个切片空间，`copy()`函数的使用格式如下：

```go
copy(destSlice,srcSlice,[]T) //srcSlice:数据来源切片；destSlice：目标切片

//实例
	a1 := []int{1, 3, 5}
	a2 := a1 //赋值
	var a3 = make([]int, 10)
	copy(a3, a1) //copy
	a1[0] = 200
	fmt.Println(a1, a2, a3)
```

### 从切片中删除元素

Go语言中并没有删除切片的专用方法，我们可以使用切片本身的特性删除元素。代码如下：

```go
func main(){
  //从切片中删除元素
  a := []int{1,2,3,4,5,6,7,8,9}
  a = append(a[:1],a[3:]...)
  fmt.println(a) //[1 4 5 6 7 8 9]
}
```

## 指针

Go语言中不存在指针操作，只需要记住两个符号：

​	1.`&`：取地址

​	2.`*` :根据地址取值

### 指针取值

在对普通变量使用&操作符取地址后会获得这个变量的指针，然后可以对指针使用*操作，也就是指针取值，代码如下。

```go
// pointer
func main() {
	// 1.&:取地址
	n := 18
	fmt.Println(&n)
	p := &n
	fmt.Println(p)
	fmt.Printf("%T\n", p)
	//*：根据地址取值
	m := *p
	fmt.Println(m)
}
```

**总结**：取地址操作符`&`和取值符``*`是对互补操作符，`&`取出地址，`*`根据地址取出地址指向的值。

变量、指针地址、指针变量、取地址、取值的相互关系和特性如下：

+ 对变量进行取地址（&）操作，可以获得这个变量的指针变量。
+ 指针变量的值是指向地址。
+ 对指针变量进行取值（*）操作，可以获得指针变量的原变量的值。

### new和make

```go
func main(){
	var a *int
  *a = 100
  fmt.Println(*a)
  var b map[string]int
  b["沙河娜扎"]=100
  fmt.Println(b)
}
```

执行上面的代码会引发panic，为什么呢？在go语言中对于引用类型的变量，我们在使用的时候不仅要声明它，还要为他=分配内存空间，否则我们的值就没办法存储。而对于值的类型不需要分配内存空间，是因为他们在声明的时候已经默认分配好了内存空间。要分配内存，就引出来今天的new和make。Go语言中new和make是内建的两个函数，主要用来分配内存。

#### new

new是一个内置函数，它的函数签名如下：

```go
fun new(Type) *Type
```

其中：

+ Type表示类型，new函数只接受一个参数，这个参数是一个类型
+ *Type表示类型指针，new函数返回一个指向该类型内存地址的指针。

new函数不太常用，使用new函数得到的是一个类型的指针，并且该指针对应的值为该类型的零值。举个例子：

```go
func main(){
	a:=new(int)
	b:=new(bool)
	fmt.Printf("%T\n",a) // *int
	fmt.Printf("%T\n",b) // *bool
	fmt.Println(*a)			 // 0	
	fmt.Println(*b)			 // false
}
```

#### make

make也是用于内存分配的，区别于new，它只用于slice，map以及chan的内存创建，而且它返回的类型就是这三个本身，而不是它们的指针类型，因为这三种类型就是引用类型，所以就没有必要返回他们的指针了。make函数的函数签名如下：

```go
func make(t Type,size ...InterType) Type
```

make函数是无可替代的，我们在使用slice、map以及channel的时候，都需要使用make进行初始化，然后才可以对它们进行操作。

本节开始的示例中`var b map[string]int`只是声明变量b是一个map类型的变量，需要像下面的示例代码一样使用make函数进行初始化操作之后，才能对其进行键值对赋值：

```go
func main(){
	var b map[string]int
	b = make(map[string]int,10)
	b["沙河娜扎"] = 100
	fmt.Println(b)
}
```

make和new的区别

1.make和new都是用来申请内存的。

2.new很少用，一般用来给基本的数据类型申请内存，string\int，返回的是对应类型的指针（*string、*int）。

3.make是用来给`slice`、`map`、`chan`申请内存的，make函数返回的是对应的这三个类型本身。

## Map(集合)

Map是一种无序的键值对的集合。Map最重要的一点是通过key来快速检索数据，key类似于数据，key类似于索引，指向数据的值。

Map是一种集合，所以我们可以像迭代数组和切片那样迭代它。不过，Map是无序的，我们无法决定它的返回顺序，这是因为Map是使用hash表累实现。

### Map定义

Go语言中`map`的定义语法如下啊：

```go
map[keyType]ValueType
```

其中：

+ KeyType：表示键的类型。
+ ValueType：表示键对应的类型。

map类型的变量默认初始值为nil，需要使用make()函数来分配内存。语法为：

```go
make(map[keyType]ValueType,[cap])
```





其中cap表示map的容量，该参数虽然不是必须的，但是我们应该在初始化map的时候就为其指定一个合适的容量。

### 按照指定顺序遍历map

```go
func main(){
  rand.Seed(time.Now().UnixNano)//初始化随机数种子
  var scoreMap = make(map[string]int,200)
  for i := 0; i < 100; i++{
    key := fmt.Sprintf("stu%02d", i) //生成stu开头的字符串
    value := rand.Intn(100)
    scoreMap[key] = value
  }
  //取出map中的所有key存入切片keys
  var keys = make([]string,0,20)
  for key := range scoreMap {
    keys = append(keys,key)
  }
  //对切片进行排序
  sort.Strings(keys)
  //按照排序后的key遍历map
  for _, key := range keys{
    fmt.Println(key,scoreMap[key])
  }
}
```

### 元素类型为map的切片

```go
func main() {
	// 元素类型为map的切片
	var s1 = make([]map[int]string, 10)
	s1[0] = make(map[int]string, 1)
	s1[0][1] = "A"
	fmt.Println(s1) //[map[1:A] map[] map[] map[] map[] map[] map[] map[] map[] map[]]
}
```

### 元素为切片类型的map

```go
func main() {
	// 元素为切片类型的map
	var m1 = make(map[string][]int, 10)
	m1["s1"] = []int{1, 2, 3, 4, 5}
	fmt.Println(m1) //map[s1:[1 2 3 4 5]]
}
```



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
		case 2,4,6,8,10:
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

### Type Switch

switch 语句还可以被用于 type-switch 来判断某个 interface 变量中实际存储的变量类型。

Type Switch 语法格式如下：

```go
switch x.(type){
    case type:
       statement(s)      
    case type:
       statement(s)
    /* 你可以定义任意个数的case */
    default: /* 可选 */
       statement(s)
}
```

#### 实例：

```go
package main

import "fmt"

func main() {
    var x interface{}
    //写法一：
    switch i := x.(type) { // 带初始化语句
    case nil:
        fmt.Printf(" x 的类型 :%T\r\n", i)
    case int:
        fmt.Printf("x 是 int 型")
    case float64:
        fmt.Printf("x 是 float64 型")
    case func(int) float64:
        fmt.Printf("x 是 func(int) 型")
    case bool, string:
        fmt.Printf("x 是 bool 或 string 型")
    default:
        fmt.Printf("未知型")
    }
    //写法二
    var j = 0
    switch j {
    case 0:
    case 1:
        fmt.Println("1")
    case 2:
        fmt.Println("2")
    default:
        fmt.Println("def")
    }
    //写法三
    var k = 0
    switch k {
    case 0:
        println("fallthrough")
        fallthrough
        /*
            Go的switch非常灵活，表达式不必是常量或整数，执行的过程从上至下，直到找到匹配项；
            而如果switch没有表达式，它会匹配true。
            Go里面switch默认相当于每个case最后带有break，
            匹配成功后不会自动向下执行其他case，而是跳出整个switch,
            但是可以使用fallthrough强制执行后面的case代码。
        */
    case 1:
        fmt.Println("1")
    case 2:
        fmt.Println("2")
    default:
        fmt.Println("def")
    }
    //写法三
    var m = 0
    switch m {
    case 0, 1:
        fmt.Println("1")
    case 2:
        fmt.Println("2")
    default:
        fmt.Println("def")
    }
    //写法四
    var n = 0
    switch { //省略条件表达式，可当 if...else if...else
    case n > 0 && n < 10:
        fmt.Println("i > 0 and i < 10")
    case n > 10 && n < 20:
        fmt.Println("i > 10 and i < 20")
    default:
        fmt.Println("def")
    }
}
```

以上代码执行结果为：

```
    x 的类型 :<nil>
    fallthrough
    1
    1
    def
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

注意：`++`（自增）和`--`（自减）在Go语言中是单独的语句，并不是运算符，不能放在等号的右边赋值。

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

# 函数

Go语言中支持函数、匿名函数和闭包，并且函数在Go语言中属于“一等公民”。

## 函数的定义

Go语言中定义函数使用`func`关键字，具体如下：

```go
func 函数名(参数)(返回值){
	函数体
}
```

其中：

* 函数名：由字母、数字、下划线组成。但函数名的第一个字母不能是数字。在同一个包内，函数名称也不能重名。
* 参数：参数由参数变量和参数变量的类型组成，多个参数之间使用“，”分割。
* 返回值：返回值由返回值和其变量类型组成，也可以只写返回值的类型，多个返回值必须用“（）”包裹，并用“，”分割。
* 函数体：实现指定功能的代码块。

```go
package main

import "fmt"

//函数

//函数存在的意义？
//函数是一段代码的封装
//把一段逻辑抽象出来封装到一个函数中，给它起个名字，每次用到它的时候直接调用
//使用函数能够让代码更清晰、更简洁。

//函数的定义
func sum(x int, y int) (ret int) {
	return x + y
}

//没有返回值的函数
func f1(x int, y int) {
	fmt.Println(x + y)
}

//没有参数但由返回值的函数
func f3() int {
	return 100
}

//参数可以命名也可以不命名
//命名的返回值就相当于函数中声明一个变量
func f4(x int, y int) (ret int) {
	ret = x + y
	fmt.Println(ret)
	return
}

//多个返回值
func f5() (int, string) {
	return 1, "沙河"
}

//参数的类型简写
//(当参数中连续两个参数的类型一致时，我们可以将非最后一个参数的类型省略)
func f6(x, y int, m, n string, i, j bool) int {
	return x + y
}

//可边长参数
//可边长参数必须放在参数最后
func f7(x string, y ...int) {
	fmt.Println(x) //下雨了
	fmt.Println(y) //[30 1 12 4 4 1 14 1] y的类型是切片 []int
}

//Go语言中函数没有默认参数这个概念
func main() {
	r := sum(10, 20)
	fmt.Println(r)
	f1(1, 2)
	fmt.Println(f3())
	fmt.Println(f4(11, 22))
	fmt.Println(f5())
	num := f6(6, 5, "", "", true, false)
	fmt.Println(num)
	f7("下雨了", 30, 1, 12, 4, 4, 1, 14, 1)
}

```

注意：在一个命名函数中不能在声明命名函数。

## defer语句

Go语言中`defer`语句会将其后面跟随的语句进行延迟出来。在`defer`归属的函数即将返回时，将延迟出来的语句按`defer`定义的逆序进行执行，也就是说，先被`defer`语句最后被执行，最后`defer`的语句，最先被执行。

```go
func main(){
	fmt.Println("start")
	defer fmt.Println(1)
	defer fmt.Println(2)
	defer fmt.Println(3)
	fmt.Println("end")
}
```

输出结构：

```go
start
3
2
1
end
```

由于`defer`语句延迟调用的特性，所以`defer`语句非常方便的处理资源释放问题。比如：资源清理、文件关闭、解锁及记录时间。

## defer执行时机

在Go语言的函数中`return `语句在底层并不是原子操作，它分为给返回值赋值和RET指令两步。而`defer`语句执行的时机就在返回值赋值操作后，RET指令执行前。具体如下图：

![defer](./imgs/defer.png 'defer')

示例：

```go

//Go语言中函数的return不是原子操作，在底层是分为两步来执行
//第一步：返回值赋值
//第二步：真正的return返回
//函数中如果存在defer，那么defer执行的时机是在第一步和第二步之间
func f1() int {  
	x := 5
	defer func() {
		x++
	}()
	return x  // 5
}
func f2() (x int) {
	defer func() {
		x++
	}()
	return 5 // 6
}
func f3() (y int) {
	x := 5
	defer func() {
		x++
	}()
	return x // 5
}
func f4() (x int) {
	defer func(x int) {
		x++
	}(x)
	return x // 0
}
func f5()(x int){
  defer func(x int) int {
    x++
    return x
  }(x)
  return 5 //5
}
```



# 函数的进阶

## 变量作用域

### 全局变量

全局变量是定义在函数外部的变量，它在程序整个运行周期内都有效。在函数中可以访问到全局变量。

```go
package main

import "fmt"
//定义全局变量
var num int64 = 10
func testGlobalVar(){
	fmt.Printf("num:%d\n",num)//函数中可以访问全局变量num
}
func main(){
	testGlobalVar() //num:10
}
```

### 局部变量

局部变量又分为两种：函数内定义的变量无法在该函数外使用，例如下面代码main函数中无法使用testLocalVar函数中定义的变量x:

```go
func testLocalVar(){
	//定义一个函数局部变量x，仅在该函数内生效
	var x int64 = 100
	fmt.Printf("x=%d\n",x)
}
func main(){
	testLocalVar()
	fmt.Println(x) //此时无法使用变量x
}
```

如何局部变量和全局变量重名，优先访问局部变量。

在`if`或者`for`语句块中的变量不能在语句外部访问。

## 函数类型和变量

### 定义函数类型

我们可以使用`type`关键字来定义一个函数类型，具体格式如下：

```go
type calclation func(int, int) int
```

上面语句定义了一个`calculation`类型，它是一种函数类型，这种函数接受两个int类型的参数并且返回一个int类型的返回值。

简单来说，凡是满足这个条件的函数都是calculation类型的函数，例如下面的add和sub是calculation类型。

```go
func add(x, y int) int {
	return x + y
}
func sub(x, y int) int {
	reutrn x - y
}
```

## 匿名函数

没有名字的函数，一般在函数内部使用。

```go
func main() {
	//匿名函数
	f := func() {
		fmt.Println("匿名函数")
	}
	f()
	//立即执行函数
	func() {
		fmt.Println("立即执行函数")
	}()
}
```

## 闭包

闭包指的是一个函数和与其相关的引用环境组合而成的实体。简单来说，`闭包=函数+引用环境`。

下面例子：

```go
func adder() func(int) int{
  var x int
  return func(y int) int {
    x += y
    return x
  }
}
func main(){
  var f = adder()
  fmt.Println(f(10))
  fmt.Println(f(20))
  fmt.Println(f(30))
  
  f1 := addre()
  fmt.Println(f(40))
  fmt.Println(f(50))
}
```

变量`f`是一个函数并且它应用了其外部作用域中的`x`变量，此时`f`就是一个闭包。在`f`的生命周期内，变量`x`也一直有效。

进阶示例：

```go
package main

import (
	"fmt"
	"strings"
)

//闭包
//示例1
func makeSuffixFunc(suffix string) func(string) string {
	return func(name string) string {
		if !strings.HasSuffix(name, suffix) {
			return name + suffix
		}
		return name
	}
}

//示例2
func calc(base int) (func(int) int, func(int) int) {
	add := func(i int) int {
		base += i
		return base
	}
	sub := func(i int) int {
		base -= i
		return base
	}
	return add, sub
}
func main() {
	jpgFunc := makeSuffixFunc(".jpg")
	txtFunc := makeSuffixFunc(".text")
	fmt.Println(jpgFunc("test")) //test.jpg
	fmt.Println(txtFunc("test")) //test.text

	f1, f2 := calc(10)
	fmt.Println(f1(1)) //11
	fmt.Println(f2(2)) //9
}
```

## 内置函数

| 内置函数       | 介绍                                                         |
| -------------- | ------------------------------------------------------------ |
| close          | 主要用来关闭channel                                          |
| len            | 用来求长度，比如string、array、slice、map、channel           |
| new            | 用来分配内存，主要用来分配值类型，比如int、struct。返回的是指针 |
| make           | 用来分配内存，主要用来分配引用类型，比如chan、map、slice     |
| append         | 用来追加元素到数组、slice中                                  |
| panic和recover | 用来做错误处理                                               |

### panic/recover

Go语言中目前是没有异常机制，但是使用`panic/recover`模式来处理错误。`panic`可以在任何地方引发，但`recover`只有在`defer`调佣的函数中有效。示例：

```go
func funcA(){
  fmt.Println("func A")
}
func funcB(){
  panic("panic in B")
}
func funcC(){
  fmt.Println("func C")
}
func main(){
  funcA()
  funcB()
  funcC()
}
```

程序运行期间`funcB`中引发了`panic`导致程序崩溃，异常退出了。这个时候我们就可以通过`recover`将程序恢复回来，继续执行。

```go
//panic 和 recover
func funcA() {
	fmt.Println("A")
}
func funcB() {
	defer func() {
		err := recover()
    //如果程序出现了panic错误，可以通过recover恢复过来
    if err!=nil{
      fmt.Println(err)
    }
		fmt.Println("模拟数据库连接...")
	}()
	panic("出现了严重的错误！！！") //程序崩溃退出
	fmt.Println("B")
}
func funcC() {
	fmt.Println("C")
}
func main() {
	funcA()
	funcB()
}
```

注意：

​	1.recover()必须搭配defer使用。

​	2.defer一定要在可能引发panic的语句之前定义。

### fmt

标准库`fmt`提供了一下几种输出相关函数。

#### Print

`Print`系列函数会将内容输出到系统的标准输出，区别在于`Print`函数直接输出内容，`Printf`函数支持格式化输出字符串，`Println`函数会在输出内容的结尾添加一行换行符。

```go
func Print(a ...interface{}) (n int,err error)
func Printf(format string,a ...interface{}) (n int,err error)
// Pirntf("格式化字符串",值)
// %T：查看类型  %d：十进制   %b：二进制   %o：八进制   %x：十六进制   %c：字符   %s：字符串
// %p：指针   %v：值   %f：浮点数   %t：布尔值
func Println(a ...interface{}) (n int,err error)
```

#### Fprint

Fprint系列函数会将内容输出到一个io.Writer接口类型的变量w中，我们通常用这个函数往文件中写入内容。

```go
func Fprint(w io.Writer, a ...interface{}) (n int, err error)
func Fprintf(w io.Writer, format string, a ...interface{}) (n int, err error)
func Fprintln(w io.Writer, a ...interface{}) (n int, err error)
```

举个例子：

```go
// 向标准输出写入内容
fmt.Fprintln(os.Stdout, "向标准输出写入内容")
fileObj, err := os.OpenFile("./xx.txt", os.O_CREATE|os.O_WRONLY|os.O_APPEND, 0644)
if err != nil {
    fmt.Println("打开文件出错，err:", err)
    return
}
name := "枯藤"
// 向打开的文件句柄中写入内容
fmt.Fprintf(fileObj, "往文件中写如信息：%s", name)
```

注意，只要满足io.Writer接口的类型都支持写入。

#### Sprint

Sprint系列函数会把传入的数据生成并返回一个字符串。

```go
func Sprint(a ...interface{}) string
func Sprintf(format string, a ...interface{}) string
func Sprintln(a ...interface{}) string
```

简单的示例代码如下：

```go
s1 := fmt.Sprint("枯藤")
name := "枯藤"
age := 18
s2 := fmt.Sprintf("name:%s,age:%d", name, age)
s3 := fmt.Sprintln("枯藤")
fmt.Println(s1, s2, s3)
```

#### Errorf

Errorf函数根据format参数生成格式化字符串并返回一个包含该字符串的错误。

```go
func Errorf(format string, a ...interface{}) error
```

通常使用这种方式来自定义错误类型，例如：

```go
err := fmt.Errorf("这是一个错误")
```

#### 通用站位符

| 站位符 | 说明                               |
| ------ | ---------------------------------- |
| %v     | 值的默认格式表示                   |
| %+v    | 类似%v，但输出结构体时会添加字段名 |
| %#v    | 值的Go语法表示                     |
| %T     | 打印值的类型                       |
| %%     | 百分号                             |

#### 布尔型

| 占位符 | 说明        |
| ------ | ----------- |
| %t     | true或false |

#### 整型

| 占位符 | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| %b     | 二进制                                                       |
| %c     | 对应的uincode                                                |
| %d     | 十进制                                                       |
| %o     | 八进制                                                       |
| %x     | 十六进制，使用a-f                                            |
| %X     | 十六进制，使用A-F                                            |
| %U     | 表示Unicode格式：U+1234,等价于“U+%o4X”                       |
| %q     | 该值对应的单引号括起来的go语法字符字面值，必要时会采用安全的转义表示 |

#### 浮点数与复数

| 占位符 | 说明                                                     |
| ------ | -------------------------------------------------------- |
| %d     | 无小数部分，二进制指数的科学计数法，如-123456p-78        |
| %e     | 科学计数法，如-1234.456e+78                              |
| %E     | 科学计数法，如-1234.456eE+78                             |
| %f     | 有小数部分但无指数部分，如123.456                        |
| %F     | 等价于%f                                                 |
| %g     | 根据实际情况采用%e或者%f格式（以获得更简洁、准确的输出） |
| %G     | 根据实际情况采用%E或者%F格式（以获取更简洁、准确的输出） |

#### 字符串和[]byte

| 占位符 | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| %s     | 直接输出字符串或者[]type                                     |
| %q     | 该值对应的双引号括起来的go语法字符串字面值，必要时会采用安全的转义表示 |
| %x     | 每个字节用两字符十六进制数表示（使用a-f                      |
| %X     | 每个字节用两字符十六进制数表示（使用A-F                      |

#### 宽度标识符

宽度通过一个紧跟在百分号后面的十进制数指定，如果未指定宽度，则表示值时除必需之外不做填充。精度通过（可选的）宽度后跟点号后跟点十进制指定。如何未指定精度，会使用默认精度；如果点号后没有跟数字，表示精度为o。

| 占位符 | 说明               |
| ------ | ------------------ |
| %f     | 默认宽度，默认精度 |
| %9f    | 宽度9，默认精度    |
| %.2f   | 默认宽度，精度2    |
| %9.f   | 宽度9，精度2       |
| %9.f   | 宽度9，精度0       |

### 其他flag

| 占位符 | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| "+"    | 总是输出数值的正负号；对%q（%+q）会生成全部是ASCII字符的输出（通过转义）； |
| ""     | 对数值，正数前加空格而负数前加负号；对字符串采用%x或%X时（%x或%X）会给各打印的字节之间加空格。 |
| "_"    | 在输出右边填充空白而不是默认的左边（即从默认的右对齐切换为左对齐）； |
| "#"    | 八进制数前加o（%#o），十六进制数前加ox（%#x）或者oX（%#X），指针去掉前的ox（%#p），对%q（%#q），对%U（%#U）会输出空格和单引号括起来的go字面值。 |
| "o"    | 使用o而不是空格填充，对于数值类型会把填充的o放在正负号后面； |

## 递归函数

递归，就是在运行的过程中调用自己。

语法格式如下：

```go
func recursion() {
   recursion() /* 函数调用自身 */
}

func main() {
   recursion()
}
```

Go 语言支持递归。但我们在使用递归时，开发者需要设置退出条件，否则递归将陷入无限循环中。

递归函数对于解决数学上的问题是非常有用的，就像计算阶乘，生成斐波那契数列等。

# Go语言基础结构体

Go语言中没有“类”的概念，也不支持“类”的继承等面向对象的概念。Go语言中通过结构体的内嵌再配合接口比面向对象具有更高的扩展和灵活性。

# 类型别名和自定义类型

## 自定义类型

在Go语言中有一些基本的数据，如`string`、`整型`、`浮点型`、`布尔`等数据类型，Go语言中可以使用`type`关键字来定义自定义类型。

自定义类型是定义一个全新的类型。我们呢过可以基于内置的基本类型定义，也可以通过struct定义。例如：

```gp
//将MyInt定义为int类型
type MyInt int
```

通过`Type`关键字的定义，`MyInt`就是一种新的类型，它具有`int`的特性。

## 类型别名

类型别名规定：TypeAlias只是Type的别名，本质上TypeAlias与Type是同一个类型。就像一个孩子小时候有小名、乳名，上学后用学名，英语老师又会给它起英文名，但这些名字都指的是他本人。

```
type TypeAlias = Type
```

我们之前见的`rune`和`byte`就是类型别名，它们定义如下：

```go
type byte = uint8
type rune = int32
```

## 类型定义和类型别名的区别

类型别名与类型定义表面上只有一个等号的差异，我们通过下面的这段代码来理解它们之间的区别。

```go
package main

import "fmt"

//自定义类型和别名

//type后面跟的是类型
type myInt int     //自定义类型
type yourInt = int //类型别名

func main() {
	var n myInt
	n = 100
	fmt.Println(n)        //100
	fmt.Printf("%T\n", n) //main.myInt

	var m yourInt
	m = 100
	fmt.Println(m)        //100
	fmt.Printf("%T\n", m) //int

	var c rune
	c = '中'
	fmt.Println(c)        //20013
	fmt.Printf("%T\n", c) //int32
}
```

结果显示n的类型是`main.myInt`，表示main包下面定义的类型。m的类型是`int`。myInt类型只会在代码中存在，编译完成是并不会有myInt类型。

# 结构体

Go语言中基本数据类型可以表示一下事物的基本属性，但是当我们想表达一个事物的全部或者部分属性时，这时候再用单一的基本数据类型明显就无法满足需求了，Go语言提供了一种自定义数据类型，可以封装多个基本数据类型，这种数据类型叫结构体，英文名叫`struct`。也就是我们可以通过`struct`来定义自己的类型了。

结构体是值类型，赋值的时候是拷贝。

## 结构体的定义

使用`type`和`struct`关键字来定义结构体，具体代码格式如下：

```go
type 类型名 struct {
	字段名 字段类型
	字段名 字段类型
	...
}
```

其中：

+ 类型名：标识自定义结构体的名称，在同一个包内不能重复。
+ 字段名：标识结构体字段名。结构体中的字段名必须唯一。
+ 字段类型：标识结构体字段的具体类型。

```go
package main
import "fmt"
//结构体
type person struct {
	name   string
	age    int
	gender string
	hobby  []string
}
func main() {
	//声明一个person类型的变量zhangsan
	var zhangsan person
	//通过字段赋值
	zhangsan.name = "zhangsan"
	zhangsan.age = 20
	zhangsan.gender = "男"
	zhangsan.hobby = []string{"篮球", "足球"}
	//访问变量zhangsan的字段
	fmt.Printf("%T\n", zhangsan) //main.person
	fmt.Println(zhangsan)        //{zhangsan 20 男 [篮球 足球]}
	fmt.Println(zhangsan.name)   //zhangsan
}
```

## 匿名结构体

```go
//匿名结构体
	var s struct { //使用var声明变量作为结构体名
		x string
		y int
	}
	s.x = "hehehe"
	s.y = 100
	fmt.Printf("type:%T value:%v \n", s, s) //type:struct { x string; y int } value:{hehehe 100}

```

## 创建指针类型的结构体

我们还可以通过`new`关键字对结构体进行实例化，得到的是结构体的地址。格式如下：

```go
var p2 = new(person)
fmt.Printf("%T\n",p2)  //*main.person
fmt.Printf("p2=%#v\n",p2) //p2=&main.person{name:"",city:"",age:0}
```

从打印的结果中我们可以看出`p2`值一个结构体指针。

需要注意的是在Go语言中指出对结构体指针直接使用`.`来访问结构体成员。

```go
var p2 = new(person)
p2.name="小王子"
p2.age=18
p2.city="上海"
fmt.Printf("p2=%3v\n",p2) //p2=&main.person{name:"小王子",city:"上海",age:18}
```

## 取结构体的地址实例化

使用`&`对结构体进行取地址操作相当于对该结构体类型进行了一次`new`实例化操作。

```go
p3:=&person{}
fmt.Printf("%T\n",p3) //*main.person
fmt.Printf("p3=%#v\n",p3) //p3=&main.person{name:"",city:"",age:0}
p3.name = "七米"
p3.age = 30
p3.city="成都"
fmt.Printf("p3=%#v\n",p3)//p3=&main.person{name:"七米",city:"成都",age:30}
```

`p3.name="七米"`其实在底层是`(*p3).name="七米"`,这是Go语言帮我们实现的语法糖。

## 结构体初始化

没有初始化的结构体，其成员变量都是对应类型的零值。

```go
//结构体是值类型
type person struct {
	name, sex string
	age       int
}
func main(){
  //结构体指针1
	var p2 = new(person)
	fmt.Printf("%#v %p\n", p2) //&main.person{name:"", sex:"女", age:0}
}

```

### 使用键值对初始化

使用键值对对结构体进行初始化，键对应结构体的字段，值对应字段的初始值。

```go
	var p3 = person{
		name: "展博",
		age:  20,
		sex:  "男",
	}
	fmt.Printf("%#v %p\n", p3, &p3) //main.person{name:"展博", sex:"男", age:20} 0xc00006e2d0
```

也可以对结构体指针进行键值对初始化，例如 ：

```go
	//key-value初始化,使用的值列表的形式初始化，值的顺序要和结构体定义时字段的顺序一致
	p3 := &person{
		name: "展博",
		age:  20,
		sex:  "男",
	}
	fmt.Printf("%#v %p\n", p3, &p3) //main.person{name:"展博", sex:"男", age:20} 0xc00006e2d0
```

当某些字段没有初始值时，该字段可以不写。此时，没有指定初始值的字段的值就是该字段类型的零值。

```go
p7 :=&person{
	city:"北京"
}
fmt.Printf("p7=%#v\n",p7) // p7=&main.person{name:"",city:"北京",age:0}
```

### 使用值的列表初始化

初始化结构体的时候可以简写，也就是简写时可以不用写键，直接写值：

```go
	//key-value初始化,使用的值列表的形式初始化，值的顺序要和结构体定义时字段的顺序一致
	p4 := &person{
		"lilei",
		"男",
		20,
	}
	fmt.Printf("%#v %p\n", p4, &p4) //main.person{name:"lilei", sex:"男", age:20} 0xc00006e330
```

使用这种格式初始化时，需要注意：

+ 必须初始化结构体所有字段。
+ 初始值的填充顺序必须与字段就在结构体中的声明顺序一致。
+ 该方式不能和键值初始化方式混用。

## 结构体内存分布

结构体占用一块连续的内存。

```go
//结构体占用一块连续的内存空间
type x struct {
	a int8 // 8bit->1byte
	b int8
	c int8
}

func main() {
	m := x{
		int8(10),
		int8(20),
		int8(30),
	}
	fmt.Printf("%p\n", &m.a) //0xc000124002
	fmt.Printf("%p\n", &m.b) //0xc000124003
	fmt.Printf("%p\n", &m.c) //0xc000124004
}
```

【进阶知识】关于Go语言中的内存对齐推荐阅读：[在Go中恰到好处的内存对齐](https://segmentfault.com/a/1190000017527311?utm_campaign=studygolang.com&utm_medium=studygolang.com&utm_source=studygolang.com)

## 构造函数

返回一个结构体变量的函数。

```go

//构造函数
type person struct {
	name string
	age  int
}
//构造函数：约定成俗以new开头
//返回的是结构体还是结构体指针
//当结构体比较大的时候尽量使用结构体指针，减少程序的内存开销
func newPerson(name string, age int) *person {
	return &person{
		name,
		age,
	}
}

type dog struct {
	name string
}

func newDog(name string) dog {
	return dog{
		name,
	}
}
func main() {
	var p = newPerson("lilei", 30) //main.person{name:"lilei", age:30}

	p2 := newPerson("周林", 90) //main.person{name:"周林", age:90}
	fmt.Printf("%#v %#v\n", p, p2)
	d1 := newDog("林狗")
	fmt.Printf("%#v\n", d1) //main.dog{name:"林狗"}
}

```

## 方法和接受者

Go语言中的`方法（Method）`是一种作用于特定类型变量的函数。这种特定类型变量叫做`接收者（Receiver）`。接受者的概念就类似于其他语言中的`this`或者`self`。

方法的定义格式如下：

```
func (接受者变量 接受者类型) 方法名(参数列表)(返回参数){
	函数体
}
```

其中：

+ 接收者变量：接收者中的参数变量名在命名时，官方建议使用接收者类型名的第一个小写字母，而不是`self`、`this`之类的命名。例如，`Person`类型的接收者变量应该命名为`p`，Connector类型的接收者变量应该命名为c等。
+ 接收者类型：接收者类型参数类型，可以是指针和非指针类型。
+ 方法名、参数列表、返回参数：具体格式与函数定义相同。

```go
//方法和接收者
type dog struct {
	name string
	age  int
}

//构造函数
func newDog(name string, age int) dog {
	return dog{
		name,
		age,
	}
}

//方法是作用于特定类型的函数
//d:形参 dog:类型
//接收者表示的是调用该方法的具体类型变量，多用类型名首字母小写表示
func (d dog) wang() {
	fmt.Println("汪汪汪", d.name)
}
func main() {
	d1 := newDog("zhoulin", 20)
	d1.wang() //汪汪汪 zhoulin
}
```

### 什么时候应该使用指针类型接收者

+ 需要修改接收者中的值。
+ 接收者是拷贝代价比较到的大对象。
+ 保证一致性，如果有某个方法使用了指针接收者，那么其他的方法也应该使用指针接收者。

### 任意类型添加方法

在Go语言中，接收者的类型可以是任何累，不仅仅是结构体，任何类型都可以拥有方法。举个例子，我们基于内置的`int`类型使用type关键字可以定义新的自定义类型，然后为我们的自定义类型添加方法。

```go
//MyInt 将int定义为MyInt类型
type MyInt int
//SayHello 为MyInt添加一个SayHello的方法
func (m MyInt) SayHello(){
	fmt.Println("Hello,我是一个int。")
}
func mian(){
	var m1 MyInt
	m1.SayHello() //Hello，我是一个int。
	m1 = 100
	fmt.Printf("%#v %T\n",m1,m1) //100 main.MyInt
}
```

***注意事项***：非本地类型不能定义方法，也就是说我们不能给别的包的类型定义方法。

## 函数版学生系统

```go
package main

import (
	"fmt"
	"os"
)

//学习管理系统函数版
/*
	需求
	查看、新增、删除 学生

*/
var (
	allStuden map[int64]*studen
)

type studen struct {
	id   int64
	name string
}

func newStudent(id int64, name string) *studen {
	return &studen{id, name}
}
func showAllStuden() {
	if len(allStuden) == 0 {
		fmt.Println("没有学生")
	}
	//遍历所有的学生
	for k, v := range allStuden {
		fmt.Printf("学号：%d 姓名：%s\n", k, v.name)
	}
}
func addStuden() {
	//向allStuden中添加一个新的学生
	//创建一个新学生
	var (
		id   int64
		name string
	)
	fmt.Println("请输入学号：")
	fmt.Scanln(&id)
	fmt.Println("请输入姓名")
	fmt.Scanln(&name)
	var stu = newStudent(id, name)
	//追加到allStuden中
	allStuden[id] = stu

}
func deleteStuden() {
	var (
		id int64
	)
	fmt.Println("请输入要删除学号：")
	fmt.Scanln(&id)
	for k := range allStuden {
		fmt.Println(k)
		if k == id {
			delete(allStuden, id)
			fmt.Printf("学号为%d的学生已删除\n", id)
		} else {
			fmt.Println("没有找到该学生")
		}
	}
}
func main() {
	allStuden = make(map[int64]*studen, 48)
	for {
		//1.打印菜单
		fmt.Println("欢迎光临学生管理系统！！！")
		fmt.Println(`
			1、查看所有学生
			2、新增学生
			3、删除学生
			4、退出
		`)
		//2.等待用户选择
		fmt.Println("请输入你要干啥：")
		var choice int
		fmt.Scanln(&choice)
		fmt.Printf("你选择了%d这个选项！\n", choice)

		//3.执行对应的函数
		switch choice {
		case 1:
			showAllStuden()
		case 2:
			addStuden()
		case 3:
			deleteStuden()
		case 4:
			os.Exit(1) //退出
		default:
			fmt.Println("滚！")
		}
	}

}

```

## 结构体的匿名字段

<font color=red>不常用</font>

结构体允许其成员字段在生命时没有字段名而只有类型，这种没有名字的字段称为匿名字段。

```go
//Person结构体Person类型
type Person struct{
	string
	int
}
func main(){
	p1:=Person{
		"小王子",
		18,
	}
  fmt.Printf("%#v\n",p1)  //main.Person{string:"小王子",int:18}
  fmt.Println(p1.string,p1.int) //小王子 18
}
```

匿名字段默认采用类型名作为字段名，结构体要求字段名称必须唯一，因此一个结构体中同种类型的匿名字段只鞥有一个。

## 嵌套结构体

一个结构体中还可以嵌套其他的结构体或者指针。

```go

//嵌套结构体
type address struct {
	province string
	city     string
}
type workPlace struct {
	province string
	city     string
}
type person struct {
	name    string
	age     int
	address //匿名嵌套结构体
	// workPlace
	//address address
}
type company struct {
	name string
	addr address
}

func main() {
	p1 := person{
		name: "zhouling",
		age:  90,
		address: address{
			province: "山东",
			city:     "威海",
		},
	}
	fmt.Println(p1)
	fmt.Println(p1.name, p1.address.city)
	fmt.Println(p1.city) //先在自己的结构体中查找这个字段，如果不存在就去匿名嵌套结构体中查找字段,当有两个或多个匿名嵌套结构体存在相同字段时，就不能使用简写
}

```

### 结构体的”继承“

Go语言中使用结构体也可以实现其他编程语言中的面向对象的继承。

```go
//结构体默认实现其他语言中的"继承"
type animal struct {
	name string
}

//给amimal实现一个移动的方法
func (a animal) move() {
	fmt.Printf("%s会动！\n", a.name)
}

//狗类
type dog struct {
	feet   uint8
	animal //animal拥有的方法，dog也有了
}

//给狗实现一个汪汪汪的方法
func (d dog) wang() {
	fmt.Printf("%s在叫:汪汪汪~\n", d.name)
}
func main() {
	d1 := dog{
		feet: 4,
		animal: animal{
			name: "zhoulin",
		},
	}
	fmt.Println(d1)
	d1.wang()
	d1.move()
}
```

## 结构体与JSON

```go
package main

import (
	"encoding/json"
	"fmt"
)

//结构体与JSON
//1.序列化：把Go语言中的结构体变量--》json格式的字符串
//2.反序列化：json格式的字符串--》Go语言中能识别的结构体变量
type person struct {
	Name string `json:"name" db:"name" ini:"name"` //设置在不同数据类型下显示的别名
	Age  int    `json:"age"`
}

func main() {
	p1 := person{
		Name: "周林",
		Age:  9000,
	}
	//序列化
	b, err := json.Marshal(p1)
	if err != nil {
		fmt.Printf("marshal failed,err:%v\n", err)
		return
	}
	fmt.Printf("%#v\n", string(b))
	//反序列化
	str := `{"name":"理想","age":18}`
	var p2 person
	json.Unmarshal([]byte(str), &p2) //传指针是为了能在json.Unmarshal中修改这个值
	fmt.Printf("%#v\n", p2)
}
```

# 接口（interface）

在Go语言中接口（interface）是一种类型，一种抽象的类型。

interface是一组method的集合，是duck-type programming的一种体现。接口做的事情就像是定义一个协议（规则），只要一台机器有洗衣服和甩干的功能，我就称它为洗衣机。不关心属性（数据），只关心行为（方法）。

为了保护你的Go语言职业生涯，请牢记接口（interface）是一种类型。

## 接口的定义

Go语言提倡面向接口编程。

```
   接口是一个或多个方法签名的集合。
    任何类型的方法集中只要拥有该接口'对应的全部方法'签名。
    就表示它 "实现" 了该接口，无须在该类型上显式声明实现了哪个接口。
    这称为Structural Typing。
    所谓对应方法，是指有相同名称、参数列表 (不包括参数名) 以及返回值。
    当然，该类型还可以有其他方法。

    接口只有方法声明，没有实现，没有数据字段。
    接口可以匿名嵌入其他接口，或嵌入到结构中。
    对象赋值给接口时，会发生拷贝，而接口内部存储的是指向这个复制品的指针，既无法修改复制品的状态，也无法获取指针。
    只有当接口存储的类型和对象都为nil时，接口才等于nil。
    接口调用不会做receiver的自动转换。
    接口同样支持匿名字段方法。
    接口也可实现类似OOP中的多态。
    空接口可以作为任何类型数据的容器。
    一个类型可实现多个接口。
    接口命名习惯以 er 结尾。
```

每个接口由数个方法组成，接口的定义如下：

```go
type 接口类型名 interface{
	方法名1（参数列表1）返回值列表1
	方法名2（参数列表2）返回值2
	...
}
```

其中：

+ 1.接口名：使用type将接口定义为自定义的类型名。Go语言的接口在命名时，一般会在单词后面添加er，如有写操作的接口叫Writer，有字符串功能的接口叫Stringer等。接口名最好要能突出该接口的类型含义。
+ 2.方法名：当方法名首字母是大写且这个接口类型名首字母也是大写时，这个方法可以被接口所在的包（package）之外的代码访问。
+ 3.参数列表、返回值列表：参数列表和返回值列表中的参数变量名可以省略。

举个例子：

```go
type writer interface{
	Write([]byte) error
}
```

## 实现接口的条件

一个对象只要全部实现了接口中的方法，那么就实现了这个接口。换句话说，接口就是一个需要实现的方法列表。

定义一个Sayer接口：

```go
//Sayer接口
type Sayer interface {
	say()
}
```

定义dog和cat两个结构体：

```go
type dog struct{}
type cat struct{}
```

因为Sayer接口里只有一个say方法，所以我们只需要给dog和cat分别实现say方法就可以实现Sayer接口了。

```go
//dog实现Sayer接口
func (d dog) say(){
	fmt.Println("汪汪汪")
}
//cat实现了Sayer接口
func (c cat) say(){
	fmt.Println("喵喵喵")
}
```

## 接口类型变量

接口类型变量能够存储所有实现了该接口的实例。 例如上面的示例中，Sayer类型的变量能够存储dog和cat类型的变量。

```go
func main() {
    var x Sayer // 声明一个Sayer类型的变量x
    a := cat{}  // 实例化一个cat
    b := dog{}  // 实例化一个dog
    x = a       // 可以把cat实例直接赋值给x
    x.say()     // 喵喵喵
    x = b       // 可以把dog实例直接赋值给x
    x.say()     // 汪汪汪
}
```

## 值接收者和指针接收者实现接口的区别

使用值接收者实现接口和使用指针接收者的区别。

是哦那个值接收者实现实现接口，结构体类型和结构体指针类型的变量都能存。

指针接收者实现接口只能存结构体指针。

实例：

我们有一个Mover接口和一个dog结构体。

```go
type Mover interface {
    move()
}

type dog struct {}
```

## 值接收者实现接口

```go
func (d dog) move() {
    fmt.Println("狗会动")
}
```

此时实现接口的是dog类型：

```go
func main() {
    var x Mover
    var wangcai = dog{} // 旺财是dog类型
    x = wangcai         // x可以接收dog类型
    var fugui = &dog{}  // 富贵是*dog类型
    x = fugui           // x可以接收*dog类型
    x.move()
}
```

从上面的代码中我们可以发现，使用值接收者实现接口之后，不管是dog结构体还是结构体指针`*dog`类型的变量都可以赋值给该接口变量。因为Go语言中有对指针类型变量求值的语法糖，dog指针fugui内部会自动求值`*fugui`。

## 类型与接口的关系

###  一个类型实现多个接口

一个类型可以同时实现多个接口，而接口间彼此独立，不知道对方的实现。 例如，狗可以叫，也可以动。我们就分别定义Sayer接口和Mover接口，如下： Mover接口。

```go
// Sayer 接口
type Sayer interface {
    say()
}

// Mover 接口
type Mover interface {
    move()
}
```

dog既可以实现Sayer接口，也可以实现Mover接口。

```go
type dog struct {
    name string
}

// 实现Sayer接口
func (d dog) say() {
    fmt.Printf("%s会叫汪汪汪\n", d.name)
}

// 实现Mover接口
func (d dog) move() {
    fmt.Printf("%s会动\n", d.name)
}

func main() {
    var x Sayer
    var y Mover

    var a = dog{name: "旺财"}
    x = a
    y = a
    x.say()
    y.move()
}
```

### 多个类型实现同一接口

Go语言中不同的类型还可以实现同一接口 首先我们定义一个Mover接口，它要求必须由一个move方法。

```go
// Mover 接口
type Mover interface {
    move()
}
```

例如狗可以动，汽车也可以动，可以使用如下代码实现这个关系：

```go
type dog struct {
    name string
}

type car struct {
    brand string
}

// dog类型实现Mover接口
func (d dog) move() {
    fmt.Printf("%s会跑\n", d.name)
}

// car类型实现Mover接口
func (c car) move() {
    fmt.Printf("%s速度70迈\n", c.brand)
}
```

这个时候我们在代码中就可以把狗和汽车当成一个会动的物体来处理了，不再需要关注它们具体是什么，只需要调用它们的move方法就可以了。

```go
func main() {
    var x Mover
    var a = dog{name: "旺财"}
    var b = car{brand: "保时捷"}
    x = a
    x.move()
    x = b
    x.move()
}
```

上面的代码执行结果如下：

```
 旺财会跑
 保时捷速度70迈
```

并且一个接口的方法，不一定需要由一个类型完全实现，接口的方法可以通过在类型中嵌入其他类型或者结构体来实现。

```go
// WashingMachine 洗衣机
type WashingMachine interface {
    wash()
    dry()
}

// 甩干器
type dryer struct{}

// 实现WashingMachine接口的dry()方法
func (d dryer) dry() {
    fmt.Println("甩一甩")
}

// 海尔洗衣机
type haier struct {
    dryer //嵌入甩干器
}

// 实现WashingMachine接口的wash()方法
func (h haier) wash() {
    fmt.Println("洗刷刷")
}
```

## 接口嵌套

接口与接口间可以通过嵌套创造出新的接口。

```go
// Sayer 接口
type Sayer interface {
    say()
}

// Mover 接口
type Mover interface {
    move()
}
// 接口嵌套
//type animal interface {
//    say()
//    move()
//}
type animal interface {
    Sayer
    Mover
}
```

嵌套得到的接口的使用与普通接口一样，这里我们让cat实现animal接口：

```go
type cat struct {
    name string
}

func (c cat) say() {
    fmt.Println("喵喵喵")
}

func (c cat) move() {
    fmt.Println("猫会动")
}

func main() {
    var x animal
    x = cat{name: "花花"}
    x.move()
    x.say()
}
```



## 空接口

### 空接口定义

空接口是指没有定义任何方法的接口。因此任何类型都实现了空接口。

空接口类型的变量可以存储任意类型的变量。

```go
func main() {
    // 定义一个空接口x
    var x interface{}
    s := "pprof.cn"
    x = s
    fmt.Printf("type:%T value:%v\n", x, x)
    i := 100
    x = i
    fmt.Printf("type:%T value:%v\n", x, x)
    b := true
    x = b
    fmt.Printf("type:%T value:%v\n", x, x)
}
```

### 空接口的应用

#### 空接口作为函数的参数

使用空接口实现可以接收任意类型的函数参数。

```go
// 空接口作为函数参数
func show(a interface{}) {
    fmt.Printf("type:%T value:%v\n", a, a)
}
```

####  空接口作为map的值

使用空接口实现可以保存任意值的字典。

```go
// 空接口作为map值
    var studentInfo = make(map[string]interface{})
    studentInfo["name"] = "李白"
    studentInfo["age"] = 18
    studentInfo["married"] = false
    fmt.Println(studentInfo)
```

## 类型断言

空接口可以存储任意类型的值，那我们如何获取其存储的具体数据呢？

###  接口值

一个接口的值（简称接口值）是由一个具体类型和具体类型的值两部分组成的。这两部分分别称为接口的动态类型和动态值。

我们来看一个具体的例子：

```go
var w io.Writer
w = os.Stdout
w = new(bytes.Buffer)
w = nil
```

请看下图分解：

![图片]( ./imgs/interface.png )

想要判断空接口中的值这个时候就可以使用类型断言，其语法格式：

```go
    x.(T)
```

其中：

```
    x：表示类型为interface{}的变量
    T：表示断言x可能是的类型。
```

该语法返回两个参数，第一个参数是x转化为T类型后的变量，第二个值是一个布尔值，若为true则表示断言成功，为false则表示断言失败。

举个例子：

```go
func main() {
    var x interface{}
    x = "pprof.cn"
    v, ok := x.(string)
    if ok {
        fmt.Println(v)
    } else {
        fmt.Println("类型断言失败")
    }
}
```

上面的示例中如果要断言多次就需要写多个if判断，这个时候我们可以使用switch语句来实现：

```go
func justifyType(x interface{}) {
    switch v := x.(type) {
    case string:
        fmt.Printf("x is a string，value is %v\n", v)
    case int:
        fmt.Printf("x is a int is %v\n", v)
    case bool:
        fmt.Printf("x is a bool is %v\n", v)
    default:
        fmt.Println("unsupport type！")
    }
}
```

因为空接口可以存储任意类型值的特点，所以空接口在Go语言中的使用十分广泛。

关于接口需要注意的是，只有当有两个或两个以上的具体类型必须以相同的方式进行处理时才需要定义接口。不要为了接口而写接口，那样只会增加不必要的抽象，导致不必要的运行时损耗。





# Go语言基础之包

2017年6月24日

 

| [Golang](https://www.liwenzhou.com/categories/Golang)

 

本文总阅读量次



在工程化的Go语言开发项目中，Go语言的源码复用是建立在包（package）基础之上的。本文介绍了Go语言中如何定义包、如何导出包的内容及如何导入其他包。

# Go语言的包（package）

## 包介绍

`包（package）`是多个Go源码的集合，是一种高级的代码复用方案，Go语言为我们提供了很多内置包，如`fmt`、`os`、`io`等。

## 定义包

我们还可以根据自己的需要创建自己的包。一个包可以简单理解为一个存放`.go`文件的文件夹。 该文件夹下面的所有go文件都要在代码的第一行添加如下代码，声明该文件归属的包。

```go
package 包名
```

注意事项：

- 一个文件夹下面直接包含的文件只能归属一个`package`，同样一个`package`的文件不能在多个文件夹下。
- 包名可以不和文件夹的名字一样，包名不能包含 `-` 符号。
- 包名为`main`的包为应用程序的入口包，这种包编译后会得到一个可执行文件，而编译不包含`main`包的源代码则不会得到可执行文件。

## 可见性

如果想在一个包中引用另外一个包里的标识符（如变量、常量、类型、函数等）时，该标识符必须是对外可见的（public）。在Go语言中只需要将标识符的首字母大写就可以让标识符对外可见了。

举个例子， 我们定义一个包名为`pkg2`的包，代码如下：

```go
package pkg2

import "fmt"

// 包变量可见性

var a = 100 // 首字母小写，外部包不可见，只能在当前包内使用

// 首字母大写外部包可见，可在其他包中使用
const Mode = 1

type person struct { // 首字母小写，外部包不可见，只能在当前包内使用
	name string
}

// 首字母大写，外部包可见，可在其他包中使用
func Add(x, y int) int {
	return x + y
}

func age() { // 首字母小写，外部包不可见，只能在当前包内使用
	var Age = 18 // 函数局部变量，外部包不可见，只能在当前函数内使用
	fmt.Println(Age)
}
```

结构体中的字段名和接口中的方法名如果首字母都是大写，外部包可以访问这些字段和方法。例如：

```go
type Student struct {
	Name  string //可在包外访问的方法
	class string //仅限包内访问的字段
}

type Payer interface {
	init() //仅限包内访问的方法
	Pay()  //可在包外访问的方法
}
```

## 包的导入

要在代码中引用其他包的内容，需要使用`import`关键字导入使用的包。具体语法如下:

```go
import "包的路径"
```

注意事项：

- import导入语句通常放在文件开头包声明语句的下面。
- 导入的包名需要使用双引号包裹起来。
- 包名是从`$GOPATH/src/`后开始计算的，使用`/`进行路径分隔。
- Go语言中禁止循环导入包。

### 单行导入

单行导入的格式如下：

```go
import "包1"
import "包2"
```

### 多行导入

多行导入的格式如下：

```go
import (
    "包1"
    "包2"
)
```

## 自定义包名

在导入包名的时候，我们还可以为导入的包设置别名。通常用于导入的包名太长或者导入的包名冲突的情况。具体语法格式如下：

```go
import 别名 "包的路径"
```

单行导入方式定义别名：

```go
import "fmt"
import m "github.com/Q1mi/studygo/pkg_test"

func main() {
	fmt.Println(m.Add(100, 200))
	fmt.Println(m.Mode)
}
```

多行导入方式定义别名：

```go
import (
    "fmt"
    m "github.com/Q1mi/studygo/pkg_test"
 )

func main() {
	fmt.Println(m.Add(100, 200))
	fmt.Println(m.Mode)
}
```

## 匿名导入包

如果只希望导入包，而不使用包内部的数据时，可以使用匿名导入包。具体的格式如下：

```go
import _ "包的路径"
```

匿名导入的包与其他方式导入的包一样都会被编译到可执行文件中。

## init()初始化函数

### init()函数介绍

在Go语言程序执行时导入包语句会自动触发包内部`init()`函数的调用。需要注意的是： `init()`函数没有参数也没有返回值。 `init()`函数在程序运行时自动被调用执行，不能在代码中主动调用它。

包初始化执行的顺序如下图所示：![包中的init()执行时机](./imgs/init01.png)

### init()函数执行顺序

Go语言包会从`main`包开始检查其导入的所有包，每个包中又可能导入了其他的包。Go编译器由此构建出一个树状的包引用关系，再根据引用顺序决定编译顺序，依次编译这些包的代码。

在运行时，被最后导入的包会最先初始化并调用其`init()`函数， 如下图示：![包之间的init()执行顺序](./imgs/init02.png)

# 文件操作

计算机中的文件储存在外部介质（磁盘）上的数据集合，文件分为文本文件和二进制文件。

## 打开和关闭文件

`os.Open()`函数能够打开一个文件，返回一个`*File`和一个`err`。对得到的文件安实例调用`close()`方法能够关闭文件。

```go
import (
	"fmt"
	"os"
)

//文件操作
func main() {
	//打开文件
	file, err := os.Open("../test/main.go")
	if err != nil {
		fmt.Println("open file failed,err:", err)
	}
	//记得关闭文件
	defer file.Close()

}

```

为了防止文件忘记关闭，我们通常使用defer注册文件关闭语句。

## 读取文件

### File.Read()

基本使用

Read方法定义如下：

```go
func (f *File) Read(b []type)(n int,err error)
```

它接收一个字节切片，返回读取的字节数和可能的具体错误，读到文件末尾时会返回`0`和`io.EOF`。实例：

```go
package main

import (
	"fmt"
	"os"
)

//文件操作
func main() {
	//打开文件
	file, err := os.Open("/Volumes/D/Golang笔记/Golang-/golang笔记.md")
	if err != nil {
		fmt.Printf("open file failed,err:%v\n", err)
	}
	//记得关闭文件
	defer file.Close()
	//读取文件
	var tmp = make([]byte, 128) //指定读的长度
	// var tmp = [128]byte
	for {
		n, err := file.Read(tmp)
		if err != nil {
			fmt.Printf("read from failed,err:%v\n", err)
		}
		fmt.Printf("读了%d字节：\n", n)
		fmt.Println(string(tmp[:n]))
		if n == 0 {
			return
		}
	}

}
```

### bufio读取文件

bufio是在file的基础上封装了一层API，支持更多的功能。

````go
package main

import (
	"bufio"
	"fmt"
	"io"
	"os"
)

func main() {
	readFromFilebyBufio()
}

//利用bufio这个包读取文件
func readFromFilebyBufio() {
	fileObj, err := os.Open("/Users/superhuan/Desktop/函数.md")
	if err != nil {
		fmt.Printf("open file failed,error %v\n", err)
	}
	//关闭文件
	defer fileObj.Close()
	//创建一个用来从文件中读的对象
	reader := bufio.NewReader(fileObj)
	for {
		line, err := reader.ReadString('\n') //注意这里是字节
		if err == io.EOF {
      fmt.Println("文件读完了")
			break
		}
		if err != nil {
			fmt.Printf("read line failed,err :%v\n", err)
			return
		}
		fmt.Println(line)
	}
}
````

### ioutil读取文件

`io/ioutil`包的`ReadFile`方法能够读取完整的文件，只需将文件名作为参数传入。

```go
func readFromByIoutil() {
	file, err := ioutil.ReadFile("/Users/superhuan/Desktop/函数.md")
	if err != nil {
		fmt.Printf("read file failed,error:%v\n", err)
	}
	fmt.Println(string(file))
}
```

## 文件写入

`os.OpenFile()`函数能够以指定模式打开文件，从而实现文件写入相关功能。

```go
func os.OpenFile(name string, flag int, perm fs.FileMode) (*os.File, error){
	...
}
```

其中：

`name`：要打开的文件路径。

`flag`：打开文件的模式。

模式有以下几种：

| 模式         | 含义     |
| ------------ | -------- |
| os.O_WRTONLY | 只写     |
| os.O_CREATE  | 创建文件 |
| os.O_RDONLY  | 只读     |
| os.O_RDWR    | 读写     |
| os.O_TRUNC   | 清空     |
| os.O_APPEND  | 追加     |

`perm`:文件权限，一个八进制数。r（读）04，w（写）02，x（执行）01.

### Write和WriteString

```go

//打开文件写入内容
var path = "/Users/superhuan/Desktop/GoProject/src/superWangHuan.com/wanghaun"
func main() {
	fileObj, err := os.OpenFile(path+"/static/test.text", os.O_WRONLY|os.O_CREATE|os.O_APPEND, 0644)
	if err != nil {
		fmt.Printf("open file failed, error: %v", err)
		return
	}
	defer fileObj.Close()
	str := "你好， 北京! \n wecome to beijin !!!"
	//write
	// fileObj.Write([]byte(str)) //写入字节切片数据
	//writeString
	fileObj.WriteString(str) //直接写入字节串数据
}

```

### Bufio.NewWriter

- bufio包实现了带缓冲区的读写，是对文件读写的封装
- bufio缓冲写数据

```go
func selfNewWriter(filePath string) {
	str := "你好， 北京! \n wecome to beijin !!!"
	fileObj, err := os.OpenFile(filePath, os.O_WRONLY|os.O_APPEND|os.O_CREATE, 0644)
	if err != nil {
		fmt.Printf("open file failed, error: %v", err)
		return
	}
	defer fileObj.Close()
	//创建一个写的对象

	wr := bufio.NewWriter(fileObj)
	wr.WriteString(str) //写入缓存中
	wr.Flush()          //将缓存中的内容写入文件
}
```

### Ioutil.WriteFile

```go
func main(){
	str:="hello 世界！！！"
	err:=ioutil.WriteFile("./xx.txt",[]byte(str),0666)
	if err!=nil{
		fmt.Printf("write file failed, error: %v\n",err)
		return 
	}
}
```

# time包

time包提供了时间的显示和测量用的函数。日历的计算采用的是公历。

## 时间类型

`time.Time`类型表示时间。我们可以通过`time.Now()`函数获取当前的时间对象，然后获取时间对象的年月日时分秒等信息。示例：

```go
func timeDemo(){
	now := time.Now() //获取当前时间
	fmt.Printf("current time:%v\n",now)
	
	year := now.Year() //年
	month := now.Month() //月
	daty := now.Day()	//日
	hour := now.Hour() //小时
	minute := now.Minute() //分钟
	second := now.Second() //秒
	fmt.Print("%d-%02d-%02d %02d:%02d:%02d\n",year,month,day,hour,minute,second)
}
```

## 时间戳

时间戳是自1970年1月1日（08：00：00 GMT）至当前时间的总毫秒数。它也被称为Unix时间戳。

基于时间对象获取时间错的示例代码如下：

```go
func timestampDemo(){
	now := time.Now() //获取当前时间
	timestamp1 := now.Unix() //时间戳
	timestamp2 := now.UnixNano() //纳秒时间戳
	fmt.Printf("current timestamp1:%v\n",timestamp1)
	fmt.Printf("current timestamp2:%v\n",timestamp2)
}
```

使用`time.Unix()`函数可以将时间戳转为时间格式。

```go
func temstampDemo2(timestamp int64){
  timeObj := time.Unix(timestamp,0) //将时间戳转为时间格式
  fmt.Println(timeObj)
  year := timeOBj.Year()
  month := timeObj,Month()
  day := timeObj.Day()
  hour := timeObj.Hour()
  minute := timeObj.Minute()
  second := timeObj.Second()
}
```

## 时间间隔

time.Duration是time包定义的一个类型，它代表两个时间点之间经过的时间，以纳秒为单位。time.Duration表示一段时间间隔，可表示的最长时间段大约290年。

time包中定义的时间间隔类型的常量如下：

```go
const (
    Nanosecond  Duration = 1
    Microsecond          = 1000 * Nanosecond
    Millisecond          = 1000 * Microsecond
    Second               = 1000 * Millisecond
    Minute               = 60 * Second
    Hour                 = 60 * Minute
)
```

例如：time.Duration表示1纳秒，time.Second表示1秒。

## 时间操作

#### Add

我们在日常的编码过程中可能会遇到要求时间+时间间隔的需求，Go语言的时间对象有提供Add方法如下：

```
    func (t Time) Add(d Duration) Time
```

举个例子，求一个小时之后的时间：

```go
func main() {
    now := time.Now()
    later := now.Add(time.Hour) // 当前时间加1小时后的时间
    fmt.Println(later)
}
```

#### Sub

求两个时间之间的差值：

```
    func (t Time) Sub(u Time) Duration
```

返回一个时间段t-u。如果结果超出了Duration可以表示的最大值/最小值，将返回最大值/最小值。要获取时间点t-d（d为Duration），可以使用t.Add(-d)。

#### Equal

```
    func (t Time) Equal(u Time) bool
```

判断两个时间是否相同，会考虑时区的影响，因此不同时区标准的时间也可以正确比较。本方法和用t==u不同，这种方法还会比较地点和时区信息。

#### Before

```
    func (t Time) Before(u Time) bool
```

如果t代表的时间点在u之前，返回真；否则返回假。

#### After

```
    func (t Time) After(u Time) bool
```

如果t代表的时间点在u之后，返回真；否则返回假。

## 定时器

使用time.Tick(时间间隔)来设置定时器，定时器的本质上是一个通道（channel）。

```go
func tickDemo() {
    ticker := time.Tick(time.Second) //定义一个1秒间隔的定时器
    for i := range ticker {
        fmt.Println(i)//每秒都会执行的任务
    }
}
```

## 时间格式化

时间类型有一个自带的方法Format进行格式化，需要注意的是Go语言中格式化时间模板不是常见的Y-m-d H:M:S而是使用Go的诞生时间2006年1月2号15点04分（记忆口诀为2006 1 2 3 4）。也许这就是技术人员的浪漫吧。

补充：如果想格式化为12小时方式，需指定PM。

```go
func formatDemo() {
    now := time.Now()
    // 格式化的模板为Go的出生时间2006年1月2号15点04分 Mon Jan
    // 24小时制
    fmt.Println(now.Format("2006-01-02 15:04:05.000 Mon Jan"))
    // 12小时制
    fmt.Println(now.Format("2006-01-02 03:04:05.000 PM Mon Jan"))
    fmt.Println(now.Format("2006/01/02 15:04"))
    fmt.Println(now.Format("15:04 2006/01/02"))
    fmt.Println(now.Format("2006/01/02"))
}
```

#### 解析字符串格式的时间

```go
now := time.Now()
fmt.Println(now)
// 加载时区
loc, err := time.LoadLocation("Asia/Shanghai")
if err != nil {
    fmt.Println(err)
    return
}
// 按照指定时区和指定格式解析字符串时间
timeObj, err := time.ParseInLocation("2006/01/02 15:04:05", "2019/08/04 14:15:20", loc)
if err != nil {
    fmt.Println(err)
    return
}
fmt.Println(timeObj)
fmt.Println(timeObj.Sub(now))
```

# Log

需求分析

1.支持往不同的地方输出日志。

2.日志分级别：

+ Debug
+ Trace
+ Info
+ Warning
+ Error
+ Fatal

3.日志要支持开关控制。

4.日志要有时间、行号、文件名、日志级别、日志信息

5.日志文件要分割。

## 使用Logger

log包定义了Logger类型，该类型提供了一些格式化输出的方法。本包也提供了一个预定义的“标准”logger，可以通过调用函数Print系列(Print|Printf|Println）、Fatal系列（Fatal|Fatalf|Fatalln）、和Panic系列（Panic|Panicf|Panicln）来使用，比自行创建一个logger对象更容易使用。

例如，我们可以像下面的代码一样直接通过log包来调用上面提到的方法，默认它们会将日志信息打印到终端界面：

```go
package main

import (
    "log"
)

func main() {
    log.Println("这是一条很普通的日志。")
    v := "很普通的"
    log.Printf("这是一条%s日志。\n", v)
    log.Fatalln("这是一条会触发fatal的日志。")
    log.Panicln("这是一条会触发panic的日志。")
}
```

编译并执行上面的代码会得到如下输出：

```
    2019/10/11 14:04:17 这是一条很普通的日志。
    2019/10/11 14:04:17 这是一条很普通的日志。
    2019/10/11 14:04:17 这是一条会触发fatal的日志。
```

logger会打印每条日志信息的日期、时间，默认输出到系统的标准错误。Fatal系列函数会在写入日志信息后调用os.Exit(1)。Panic系列函数会在写入日志信息后panic。

#  1. 反射

反射是指在程序运行期对程序本身进行访问和修改的能力

### 1.1.1. 变量的内在机制

- 变量包含类型信息和值信息 var arr [10]int arr[0] = 10
- 类型信息：是静态的元信息，是预先定义好的
- 值信息：是程序运行过程中动态改变的

### 1.1.2. 反射的使用

- reflect包封装了反射相关的方法
- 获取类型信息：reflect.TypeOf，是静态的
- 获取值信息：reflect.ValueOf，是动态的

### 1.1.3. 空接口与反射

- 反射可以在运行时动态获取程序的各种详细信息
- 反射获取interface类型信息

```go
package main

import (
   "fmt"
   "reflect"
)

//反射获取interface类型信息

func reflect_type(a interface{}) {
   t := reflect.TypeOf(a)
   fmt.Println("类型是：", t)
   // kind()可以获取具体类型
   k := t.Kind()
   fmt.Println(k)
   switch k {
   case reflect.Float64:
      fmt.Printf("a is float64\n")
   case reflect.String:
      fmt.Println("string")
   }
}

func main() {
   var x float64 = 3.4
   reflect_type(x)
}
```

- 反射获取interface值信息

```go
package main

import (
    "fmt"
    "reflect"
)

//反射获取interface值信息

func reflect_value(a interface{}) {
    v := reflect.ValueOf(a)
    fmt.Println(v)
    k := v.Kind()
    fmt.Println(k)
    switch k {
    case reflect.Float64:
        fmt.Println("a是：", v.Float())
    }
}

func main() {
    var x float64 = 3.4
    reflect_value(x)
}
```

- 反射修改值信息

```go
package main

import (
    "fmt"
    "reflect"
)

//反射修改值
func reflect_set_value(a interface{}) {
    v := reflect.ValueOf(a)
    k := v.Kind()
    switch k {
    case reflect.Float64:
        // 反射修改值
        v.SetFloat(6.9)
        fmt.Println("a is ", v.Float())
    case reflect.Ptr:
        // Elem()获取地址指向的值
        v.Elem().SetFloat(7.9)
        fmt.Println("case:", v.Elem().Float())
        // 地址
        fmt.Println(v.Pointer())
    }
}

func main() {
    var x float64 = 3.4
    // 反射认为下面是指针类型，不是float类型
    reflect_set_value(&x)
    fmt.Println("main:", x)
}
```

### 1.1.4. 结构体与反射

查看类型、字段和方法

```go
package main

import (
    "fmt"
    "reflect"
)

// 定义结构体
type User struct {
    Id   int
    Name string
    Age  int
}

// 绑方法
func (u User) Hello() {
    fmt.Println("Hello")
}

// 传入interface{}
func Poni(o interface{}) {
    t := reflect.TypeOf(o)
    fmt.Println("类型：", t)
    fmt.Println("字符串类型：", t.Name())
    // 获取值
    v := reflect.ValueOf(o)
    fmt.Println(v)
    // 可以获取所有属性
    // 获取结构体字段个数：t.NumField()
    for i := 0; i < t.NumField(); i++ {
        // 取每个字段
        f := t.Field(i)
        fmt.Printf("%s : %v", f.Name, f.Type)
        // 获取字段的值信息
        // Interface()：获取字段对应的值
        val := v.Field(i).Interface()
        fmt.Println("val :", val)
    }
    fmt.Println("=================方法====================")
    for i := 0; i < t.NumMethod(); i++ {
        m := t.Method(i)
        fmt.Println(m.Name)
        fmt.Println(m.Type)
    }

}

func main() {
    u := User{1, "zs", 20}
    Poni(u)
}
```

查看匿名字段

```go
package main

import (
    "fmt"
    "reflect"
)

// 定义结构体
type User struct {
    Id   int
    Name string
    Age  int
}

// 匿名字段
type Boy struct {
    User
    Addr string
}

func main() {
    m := Boy{User{1, "zs", 20}, "bj"}
    t := reflect.TypeOf(m)
    fmt.Println(t)
    // Anonymous：匿名
    fmt.Printf("%#v\n", t.Field(0))
    // 值信息
    fmt.Printf("%#v\n", reflect.ValueOf(m).Field(0))
}
```

修改结构体的值

```go
package main

import (
    "fmt"
    "reflect"
)

// 定义结构体
type User struct {
    Id   int
    Name string
    Age  int
}

// 修改结构体值
func SetValue(o interface{}) {
    v := reflect.ValueOf(o)
    // 获取指针指向的元素
    v = v.Elem()
    // 取字段
    f := v.FieldByName("Name")
    if f.Kind() == reflect.String {
        f.SetString("kuteng")
    }
}

func main() {
    u := User{1, "5lmh.com", 20}
    SetValue(&u)
    fmt.Println(u)
}
```

调用方法

```go
package main

import (
    "fmt"
    "reflect"
)

// 定义结构体
type User struct {
    Id   int
    Name string
    Age  int
}

func (u User) Hello(name string) {
    fmt.Println("Hello：", name)
}

func main() {
    u := User{1, "5lmh.com", 20}
    v := reflect.ValueOf(u)
    // 获取方法
    m := v.MethodByName("Hello")
    // 构建一些参数
    args := []reflect.Value{reflect.ValueOf("6666")}
    // 没参数的情况下：var args2 []reflect.Value
    // 调用方法，需要传入方法的参数
    m.Call(args)
}
```

获取字段的tag

```go
package main

import (
    "fmt"
    "reflect"
)

type Student struct {
    Name string `json:"name1" db:"name2"`
}

func main() {
    var s Student
    v := reflect.ValueOf(&s)
    // 类型
    t := v.Type()
    // 获取字段
    f := t.Elem().Field(0)
    fmt.Println(f.Tag.Get("json"))
    fmt.Println(f.Tag.Get("db"))
}
```

### 1.1.5. 反射练习

- 任务：解析如下配置文件
  - 序列化：将结构体序列化为配置文件数据并保存到硬盘
  - 反序列化：将配置文件内容反序列化到程序的结构体
- 配置文件有server和mysql相关配置

```
#this is comment
;this a comment
;[]表示一个section
[server]
ip = 10.238.2.2
port = 8080

[mysql]
username = root
passwd = admin
database = test
host = 192.168.10.10
port = 8000
timeout = 1.2
```

# Strconv

strconv包实现了基本数据类型与其字符串表示的转换，主要有以下常用函数： Atoi()、Itia()、parse系列、format系列、append系列。

更多函数请查看[官方文档](https://golang.org/pkg/strconv/)。

### 1.1.1. string与int类型转换

这一组函数是我们平时编程中用的最多的。

### 1.1.2. Atoi()

Atoi()函数用于将字符串类型的整数转换为int类型，函数签名如下。

```go
func Atoi(s string) (i int, err error)
```

如果传入的字符串参数无法转换为int类型，就会返回错误。

```go
s1 := "100"
i1, err := strconv.Atoi(s1)
if err != nil {
    fmt.Println("can't convert to int")
} else {
    fmt.Printf("type:%T value:%#v\n", i1, i1) //type:int value:100
}
```

### 1.1.3. Itoa()

Itoa()函数用于将int类型数据转换为对应的字符串表示，具体的函数签名如下。

```go
func Itoa(i int) string
```

示例代码如下：

```go
i2 := 200
s2 := strconv.Itoa(i2)
fmt.Printf("type:%T value:%#v\n", s2, s2) //type:string value:"200"
```

### 1.1.4. a的典故

【扩展阅读】这是C语言遗留下的典故。C语言中没有string类型而是用字符数组(array)表示字符串，所以Itoa对很多C系的程序员很好理解。

### 1.1.5. Parse系列函数

Parse类函数用于转换字符串为给定类型的值：ParseBool()、ParseFloat()、ParseInt()、ParseUint()。

### 1.1.6. ParseBool()

```go
func ParseBool(str string) (value bool, err error)
```

返回字符串表示的bool值。它接受1、0、t、f、T、F、true、false、True、False、TRUE、FALSE；否则返回错误。

### 1.1.7. ParseInt()

```go
func ParseInt(s string, base int, bitSize int) (i int64, err error)
```

返回字符串表示的整数值，接受正负号。

base指定进制（2到36），如果base为0，则会从字符串前置判断，”0x”是16进制，”0”是8进制，否则是10进制；

bitSize指定结果必须能无溢出赋值的整数类型，0、8、16、32、64 分别代表 int、int8、int16、int32、int64；

返回的err是`*NumErr`类型的，如果语法有误，err.Error = ErrSyntax；如果结果超出类型范围err.Error = ErrRange。

### 1.1.8. ParseUnit()

```go
func ParseUint(s string, base int, bitSize int) (n uint64, err error)
```

ParseUint类似ParseInt但不接受正负号，用于无符号整型。

### 1.1.9. ParseFloat()

```go
func ParseFloat(s string, bitSize int) (f float64, err error)
```

解析一个表示浮点数的字符串并返回其值。

如果s合乎语法规则，函数会返回最为接近s表示值的一个浮点数（使用IEEE754规范舍入）。

bitSize指定了期望的接收类型，32是float32（返回值可以不改变精确值的赋值给float32），64是float64；

返回值err是`*NumErr`类型的，语法有误的，err.Error=ErrSyntax；结果超出表示范围的，返回值f为±Inf，err.Error= ErrRange。

### 1.1.10. 代码示例

```go
b, err := strconv.ParseBool("true")
f, err := strconv.ParseFloat("3.1415", 64)
i, err := strconv.ParseInt("-2", 10, 64)
u, err := strconv.ParseUint("2", 10, 64)
```

这些函数都有两个返回值，第一个返回值是转换后的值，第二个返回值为转化失败的错误信息。

### 1.1.11. Format系列函数

Format系列函数实现了将给定类型数据格式化为string类型数据的功能。

### 1.1.12. FormatBool()

```go
func FormatBool(b bool) string
```

根据b的值返回”true”或”false”。

### 1.1.13. FormatInt()

```go
func FormatInt(i int64, base int) string
```

返回i的base进制的字符串表示。base 必须在2到36之间，结果中会使用小写字母’a’到’z’表示大于10的数字。

### 1.1.14. FormatUint()

```go
func FormatUint(i uint64, base int) string
```

是FormatInt的无符号整数版本。

### 1.1.15. FormatFloat()

```go
func FormatFloat(f float64, fmt byte, prec, bitSize int) string
```

函数将浮点数表示为字符串并返回。

bitSize表示f的来源类型（32：float32、64：float64），会据此进行舍入。

fmt表示格式：’f’（-ddd.dddd）、’b’（-ddddp±ddd，指数为二进制）、’e’（-d.dddde±dd，十进制指数）、’E’（-d.ddddE±dd，十进制指数）、’g’（指数很大时用’e’格式，否则’f’格式）、’G’（指数很大时用’E’格式，否则’f’格式）。

prec控制精度（排除指数部分）：对’f’、’e’、’E’，它表示小数点后的数字个数；对’g’、’G’，它控制总的数字个数。如果prec 为-1，则代表使用最少数量的、但又必需的数字来表示f。

### 1.1.16. 代码示例

```go
s1 := strconv.FormatBool(true)
s2 := strconv.FormatFloat(3.1415, 'E', -1, 64)
s3 := strconv.FormatInt(-2, 16)
s4 := strconv.FormatUint(2, 16)
```

### 1.1.17. 其他

### 1.1.18. isPrint()

```go
func IsPrint(r rune) bool
```

返回一个字符是否是可打印的，和unicode.IsPrint一样，r必须是：字母（广义）、数字、标点、符号、ASCII空格。

### 1.1.19. CanBackquote()

```go
func CanBackquote(s string) bool
```

返回字符串s是否可以不被修改的表示为一个单行的、没有空格和tab之外控制字符的反引号字符串。

### 1.1.20. 其他

除上文列出的函数外，strconv包中还有Append系列、Quote系列等函数。具体用法可查看[官方文档](https://golang.org/pkg/strconv/)。

# Go语言并发

## 并发与并行

并发：同一时间段内执行多个任务

并行：同一时刻执行多个任务

Go语言的并发通过`goroutine`实现。`goroutine`类似于线程，属于用户态的线程，我们可以根据需要创建成千上万个`goroutine`并发工作。`goroutine`是由Go语言的运行时(runtime)调度完成，而线程是由操作系统调度完成。

Go语言还提供`channel`在多个`goroutine`见进行通信。`goroutine`和`channel`是Go语言秉承的GSP（Communicating Sequential Process）并发模式的重要实现基础。

## goroutine

在java/c++中我们要实现并发编程的时候，我们通常需要自己维护一个线程池，并且需要自己去包装一个又一个的任务，同时需要自己去调度线程执行任务并维护上下文切换，这一切通常会耗费程序员大量的心智。那么能不能有一种机制，程序员只需要定义很多个任务，让系统去帮助我们把这些任务分配到CPU上实现并发执行呢？

Go语言中的`goroutine`就是这样一种机制，`goroutine`的概念类似于线程，但 `goroutine`是由Go的运行时（runtime）调度和管理的。Go程序会智能地将 goroutine 中的任务合理地分配给每个CPU。Go语言之所以被称为现代化的编程语言，就是因为它在语言层面已经内置了调度和上下文切换的机制。

在Go语言编程中你不需要去自己写进程、线程、协程，你的技能包里只有一个技能–`goroutine`，当你需要让某个任务并发执行的时候，你只需要把这个任务包装成一个函数，开启一个`goroutine`去执行这个函数就可以了，就是这么简单粗暴。

## 使用goroutine

Go语言中使用`goroutine`非常简单，只需要在调用函数的时候在前面加上`go`关键字，就可以为一个函数创建一个`goroutine`。

一个`goroutine`必定对应一个函数，可以创建多个`goroutine`去执行相同函数。

### 启动单个goroutine

启动goroutine的方式非常简单，只需要在调用的函数（普通函数和匿名函数）前面加上一个`go`关键字。

举个例子如下：

```go
func hello() {
	fmt.Println("Hello Goroutine!")
}
func main() {
	hello()
	fmt.Println("main goroutine done!")
}
```

这个示例中hello函数和下面的语句是串行的，执行的结果是打印完`Hello Goroutine!`后打印`main goroutine done!`。

接下来我们在调用hello函数前面加上关键字`go`，也就是启动一个goroutine去执行hello这个函数。

```go
func main() {
	go hello() // 启动另外一个goroutine去执行hello函数
	fmt.Println("main goroutine done!")
}
```

这一次的执行结果只打印了`main goroutine done!`，并没有打印`Hello Goroutine!`。为什么呢？

在程序启动时，Go程序就会为`main()`函数创建一个默认的`goroutine`。

当main()函数返回的时候该`goroutine`就结束了，所有在`main()`函数中启动的`goroutine`会一同结束，`main`函数所在的`goroutine`就像是权利的游戏中的夜王，其他的`goroutine`都是异鬼，夜王一死它转化的那些异鬼也就全部GG了。

所以我们要想办法让main函数等一等hello函数，最简单粗暴的方式就是`time.Sleep`了。

```go
func main() {
	go hello() // 启动另外一个goroutine去执行hello函数
	fmt.Println("main goroutine done!")
	time.Sleep(time.Second)
}
```

执行上面的代码你会发现，这一次先打印`main goroutine done!`，然后紧接着打印`Hello Goroutine!`。

首先为什么会先打印`main goroutine done!`是因为我们在创建新的goroutine的时候需要花费一些时间，而此时main函数所在的`goroutine`是继续执行的。

## math/rand

```go
func f() {
	rand.Seed(time.Now().UnixNano()) //保证每次执行的时间都不一样
	for i := 0; i < 5; i++ {
		n1 := rand.Int()
		n2 := rand.Intn(10) // 0 <= x <10
		fmt.Println(n1, n2)
	}

}
```



## 启动多个goroutine

在Go语言中实现并发就是这样简单，我们还可以启动多个`goroutine`。（sync.Waiting来实现goroutine的同步）

```go
func f1(i int) {
	defer wg.Done()
	time.Sleep(time.Second * time.Duration(rand.Intn(3)))
	fmt.Println(i)
}

var wg sync.WaitGroup

func main() {
	for i := 0; i < 10; i++ {
		wg.Add(1)
		go f1(i)
	}
	//如何知道这10个goroutine都结束了
	wg.Wait() //等待wg的计数器减为0
}

```

多次执行上面的代码，会发现每次打印的数字的顺序都不一致。这是因为10个`goroutine`是并发执行的，而`goroutine`的调度是随机的。

## goroutine与线程

### 可增长的栈

OS线程（操作系统线程）一般都有固定的栈内存（通常为2MB）,一个`goroutine`的栈在其生命周期开始时只有很小的栈（典型情况下2KB），`goroutine`的栈不是固定的，他可以按需增大和缩小，`goroutine`的栈大小限制可以达到1GB，虽然极少会用到这么大。所以在Go语言中一次创建十万左右的`goroutine`也是可以的。

### goroutine调度

`GPM`是Go语言运行时（runtime）层面的实现，是go语言自己实现的一套调度系统。区别于操作系统调度OS线程。

- `G`很好理解，就是个goroutine的，里面除了存放本goroutine信息外 还有与所在P的绑定等信息。
- `P`管理着一组goroutine队列，P里面会存储当前goroutine运行的上下文环境（函数指针，堆栈地址及地址边界），P会对自己管理的goroutine队列做一些调度（比如把占用CPU时间较长的goroutine暂停、运行后续的goroutine等等）当自己的队列消费完了就去全局队列里取，如果全局队列里也消费完了会去其他P的队列里抢任务。
- `M（machine）`是Go运行时（runtime）对操作系统内核线程的虚拟， M与内核线程一般是一一映射的关系， 一个groutine最终是要放到M上执行的；

P与M一般也是一一对应的。他们关系是： P管理着一组G挂载在M上运行。当一个G长久阻塞在一个M上时，runtime会新建一个M，阻塞G所在的P会把其他的G 挂载在新建的M上。当旧的G阻塞完成或者认为其已经死掉时 回收旧的M。

P的个数是通过`runtime.GOMAXPROCS`设定（最大256），Go1.5版本之后默认为物理线程数。 在并发量大的时候会增加一些P和M，但不会太多，切换太频繁的话得不偿失。

单从线程调度讲，Go语言相比起其他语言的优势在于OS线程是由OS内核来调度的，`goroutine`则是由Go运行时（runtime）自己的调度器调度的，这个调度器使用一个称为m:n调度的技术（复用/调度m个goroutine到n个OS线程）。 其一大特点是goroutine的调度是在用户态下完成的， 不涉及内核态与用户态之间的频繁切换，包括内存的分配与释放，都是在用户态维护着一块大的内存池， 不直接调用系统的malloc函数（除非内存池需要改变），成本比调度OS线程低很多。 另一方面充分利用了多核的硬件资源，近似的把若干goroutine均分在物理线程上， 再加上本身goroutine的超轻量，以上种种保证了go调度方面的性能。

### GOMAXPROCS

Go运行时的调度器使用`GOMAXPROCS`参数来确定需要使用多少个OS线程来同时执行Go代码。默认值是机器上的CPU核心数。例如在一个8核心的机器上，调度器会把Go代码同时调度到8个OS线程上（GOMAXPROCS是m:n调度中的n）。

Go语言中可以通过`runtime.GOMAXPROCS()`函数设置当前程序并发时占用的CPU逻辑核心数。

Go1.5版本之前，默认使用的是单核心执行。Go1.5版本之后，默认使用全部的CPU逻辑核心数。

我们可以通过将任务分配到不同的CPU逻辑核心上实现并行的效果，这里举个例子：

```go
func a() {
	for i := 1; i < 10; i++ {
		fmt.Println("A:", i)
	}
}

func b() {
	for i := 1; i < 10; i++ {
		fmt.Println("B:", i)
	}
}

func main() {
	runtime.GOMAXPROCS(1)
	go a()
	go b()
	time.Sleep(time.Second)
}
```

两个任务只有一个逻辑核心，此时是做完一个任务再做另一个任务。 将逻辑核心数设为2，此时两个任务并行执行，代码如下。

```go
func a() {
	for i := 1; i < 10; i++ {
		fmt.Println("A:", i)
	}
}

func b() {
	for i := 1; i < 10; i++ {
		fmt.Println("B:", i)
	}
}

func main() {
	runtime.GOMAXPROCS(2)
	go a()
	go b()
	time.Sleep(time.Second)
}
```

Go语言中的操作系统线程和goroutine的关系：

1. 一个操作系统线程对应用户态多个goroutine。
2. go程序可以同时使用多个操作系统线程。
3. goroutine和OS线程是多对多的关系，即m:n。
