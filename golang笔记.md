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

`Fprint`系列函数会将内容输出到一个`io.Writer`接口类型的变量`w`中，我们通常用这个函数往文件中写入内容。

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

