# 一、数据类型：

## 布尔型：

布尔型的值只可以是常量true 或者 false。

一个简单的例子：varb bool = true。

## 字符串类型：

字符串就是一串固定长度的字符连接起来的字符序列。Go 的字符串是由单个字节连接起来的。Go 语言的字符串的字节使用 UTF-8 编码标识 Unicode 文本。

- 字符串中可以使用转移符：

  \r回车符return，返回行首；

  \n换行符new line，直接跳到下一行的同列位置；

  \t制表符tab；

  \\‘单引号；

  \\"双引号；

  \\\反斜杠；

- 定义多行字符串：

  双引号书写的字符串被称为字符串字面量，不能换行；

  使用`反引号，多用于嵌套源码或数据；

  反引号中的所有代码都不会被编译器识别，只是作为字符串一部分；

## 数字类型：

整型 int 和浮点型 float32、float64，Go 语言支持整型和浮点型数字，并且支持复数，其中位的运算采用补码。

### 一般数字类型：

整型分为两大类：

按长度分（例如：0到127）：int8 int16 int32(rune) int64 int 

无符号整型（例如：-128到127）：uint8(byte) uint16 uint32 uint64 uint

浮点型：float32 float64

复数型：complex64 complex128

字符型：rune（类似int32——2的32次方）

### 特殊数字类型：

byte，uint8 别名，用于表示二进制数据的bytes，代表了ascii码的一个字符；

rune，int32 别名， 用于表示一个符号，代表一个utf-8字符，当需要处理中文等unicode字符集时需要用到rune；

例如：

```go
func main(){
	a := 100
	var b byte = 200
	var c rune = 300
	var e byte = 'z'
	var f rune = '嘿'
	fmt.Printf("%T %v\n",a,a)
	fmt.Printf("%T %v\n",b,b)
	fmt.Printf("%T %v\n",c,c)
	fmt.Printf("%T %v\n",e,e)
	fmt.Printf("%T %v\n",f,f)
}

int 100
uint8 200
int32 300
uint8 122
int32 22079
```

int，与uint一样，32位或64位，根据计算机系统自己判断；

uintptr：无符号整型，用于存放一个指针；

## 其他类型：

错误型：error

## 高级类型：

### 指针类型（pointer & *）：

指针pointer：

一个指针变量指向了一个值的内存地址，类似于变量和常量，在使用指针前你需要声明指针，由*来指定变量作为一个指针；

由&来取地址，放到一个变量前使用就会返回相应变量的内存地址。

&：返回变量存储地址，&a将给出变量的实际地址；

*：返回指针变量。*a是一个指针变量；

格式：

var var_name *var-type

释义：

1、var：指针类型由var开始声明；

2、var_name：为指针变量名， 

3、var-type：为指针类型，*号用于指定变量是作为一个指针。

例子1、：

var ip *int        /* 指向整型*/

var fp *float32    /* 指向浮点型 */

例子2、：

```go
func main() {
   var a int = 10
   fmt.Printf("变量的地址: %x\n", &a)
}
```

### 数组类型（array&[]）：

数组：

数组是具有相同唯一类型的一组已编号且长度固定的数据项序列，这种类型可以是任意的原始类型例如整形、字符串或者自定义类型。

格式：

var variable_name [SIZE] variable_type

释义：

注意：数组声明需要指定元素类型及元素个数。

1、var：数组类型由var开始声明；

2、variable_name：定义数组名称； 

3、[SIZE]：定义数组长度；

4、variable_type：定义数组类型；

例子：

var balance [10] float32

定义了数组 balance长度为 10 类型为 float32：

### 切片类型（slice&make）：

切片：

Go 语言切片是对数组的抽象，数组的长度不可改变，在特定场景中这样的集合就不太适用，Go中提供了一种灵活，功能强悍的内置类型切片("动态数组"),与数组相比切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大；

切片不需要说明长度；

一个切片在未初始化之前默认为 nil，长度为0；

切片追加内容：

append()：append(numbers, 0)

切片复制：

copy()：copy(numbers1,numbers2)

格式：

```go
var identifier []type
var slice1 []type = make([]type, len)
slice1 := make([]type, len)
slice1 := make([]type, len, cap)
```

释义：

1、var：切片由var来开始声明；

2、identifier：切片名称；

3、[]type：切片数据类型；

4、len：是数组的长度并且也是切片的初始长度；

5、cap：可以测量切片最长可以达到多少;

例子：

```go
var s []int {1,2,3}
```

或简写

```go
s := []int {1,2,3 } 
```

直接初始化切片，[]int表示是切片类型，{1,2,3}初始化值依次是1,2,3.其cap=len=3

### 集合类型（map）：

集合：

Map 是一种无序的键值对的集合。Map 最重要的一点是通过 key 来快速检索数据，key 类似于索引，指向数据的值。

Map 是一种集合，所以我们可以像迭代数组和切片那样迭代它。不过，Map 是无序的，我们无法决定它的返回顺序，这是因为 Map 是使用 hash 表来实现的。

格式：

可以使用内建函数make 也可以使用 map 关键字来定义 Map：

/* 声明变量，默认 map 是nil */

var map_variablemap[key_data_type]value_data_type

 

/* 使用 make 函数 */

map_variable :=make(map[key_data_type]value_data_type)



如果不初始化map，那么就会创建一个 nil map，nil map不能用来存放键值对；

delete() 函数用于删除集合的元素, 参数为map和其对应的 key；

delete(key, "values")

释义：

1、var：集合由var来开始声明；

2、map_variable map：集合名称；

3、map：关键字map；

4、[key_data_type]：key值的数据类型；

5、value_data_type：value值的数据类型；

例子：

创建变量集合：

```go
countryCapitalMap := make(map[string]string)
```

赋值给变量集合：

```go
countryCapitalMap[string] = string
```

实例：

```go
func main() {
	makeMap()
}

func makeMap() {
	countryMap := make(map[int]string)

	countryMap[1] = "北京"
	countryMap[2] = "上海"
	countryMap[3] = "广州"
	countryMap[4] = "深圳"

	fmt.Println(countryMap)
}
结果：
map[1:北京 2:上海 3:广州 4:深圳]
```

### 结构化类型（type…struct）：

自定义类型type：

type NAME TYPE

例子：type City string

City就是一个新的数据类型；

结构体struct：

结构体

1、是由一系列具有相同类型或不同类型的数据构成的数据集合，一旦定义了结构体类型，它就能用于变量的声明。

2、访问结构体成员，需要使用点号 . 操作符，格式为：

结构体.成员名

3、可以像其他数据类型一样将结构体类型作为参数传递给函数;

格式：

```go
typestruct_variable_type struct {
   member definition;
   member definition;
   ...
   member definition;
}
```

用于变量的声明：

```go
variable_name := structure_variable_type {value1, value2...valuen}
```

释义：

1、type：结构化类型由type开始定义；

2、struct_variable_type：定义结构体类型的名称；

3、struct：定义一个新的数据类型，结构体有中有一个或多个成员。

4、member definition：定义成员类型，有一个或多个；

5、variable_name：变量名称；

6、:=：简短声明模式；

例子：

```go
typeBooks struct {
   title string
   author string
   subject string
   book_id int
}
```

### 函数类型（func）：

函数：

函数是基本的代码块，用于执行一个任务；

Go 语言最少有个 main() 函数；

你可以通过函数来划分不同功能，逻辑上每个函数执行的是指定的任务；

函数声明告诉了编译器函数的名称，返回类型，和参数；

Go 语言标准库提供了多种可动用的内置的函数；

例如，len() 函数可以接受不同类型参数并返回该类型的长度；

如果我们传入的是字符串则返回字符串的长度，如果传入的是数组，则返回数组中包含的元素个数。

格式：

```go
funcfunction_name ( [parameterlist] ) [return_types] {
   函数体
}
```

释义：

1、func：函数类型由 func 开始声明；

2、function_name：函数名称，函数名和参数列表一起构成了函数签名；

3、parameter list：参数列表，参数就像一个占位符，当函数被调用时，你可以将值传递给参数，这个值被称为实际参数。参数列表指定的是参数类型、顺序、及参数个数。参数是可选的，也就是说函数也可以不包含参数；

4、return_types：返回类型，函数返回一列值。return_types 是该列值的数据类型。有些功能不需要返回值，这种情况下return_types 不是必须的。

5、函数体：函数定义的代码集合。

### 接口类型（interface）：

方法method：

方法（method）的声明和函数很相似,只不过它必须指定接收者：

```go
func (t T) F() {}
```

注意：

1、接收者的类型只能为用关键字type定义的类型，例如自定义类型，结构体。

2、同一个接收者的方法名不能重复(没有重载)，如果是结构体，方法名还不能和字段名重复。

3、值作为接收者无法修改其值，如果有更改需求，需要使用指针类型。

接口interface：

Go 语言提供了另外一种数据类型即接口，它把所有的具有共性的方法定义在一起，任何其他类型只要实现了这些方法就是实现了这个接口。

结构：

``` go
/* 定义接口 */
type interface_name interface {
  method_name1 [return_type]
  method_name2 [return_type]
  method_name3 [return_type]
   ...
  method_namen [return_type]
}

/* 定义结构体 */
type struct_name struct {
   /* variables */
}

/* 实现接口方法 */
func (struct_name_variable struct_name) method_name1() [return_type] {
   /* 方法实现 */
}
...
func (struct_name_variable struct_name) method_namen() [return_type] {
   /* 方法实现*/
}
```

释义：

一、/* 定义接口 */interface：

1、typ：开始定义接口；

2、interface_name：接口名称；

3、interface：关键字interface；

4、method_name1[return_type]：实现接口的方法，

二、/* 定义结构体 */struct：

详见：[结构化类型（type…struct）：](#_结构化类型（type…struct）：)

三、/* 实现接口方法 */method：

1、func：开始定义实现接口的方法函数；

2、(struct_name_variablestruct_name)：变量名及变量结构体类型的名称；

3、method_name1()：接口名，即上面定义的接口名称interface_name；

4、{/* 方法实现*/}：具体实现方法的代码块；

### 通道类型（channel）：

通道channel：

goroutine 是 Go 中实现并发的重要机制，channel 是 goroutine 之间进行通信的重要桥梁。

使用内建函数 make 可以创建 channel：

例子：

ch:= make(chanint) // 注意： channel 必须定义其传递的数据类型

可以用 var 声明 channel, 如下：

varch chan int

# 二、变量和常量：

## 常量const：

常量是一个简单值的标识符，在程序运行时，不会被修改的量；数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型。

格式：

const identifier [type] = value

释义：

1、const：关键字

2、identifier：常量名

3、[type]：常量数据类型

4、Value：常量值

显式类型定义：const b string = "abc"

隐式类型定义：const b = "abc"

多个常量定义：const c_name1, c_name2 = value1,value2

## 特殊常量iota：

iota，特殊常量，可以认为是一个可以被编译器修改的常量。

iota 在 const关键字出现时将被重置为 0(const 内部的第一行之前)，const中每新增一行常量声明将使 iota 计数一次(iota 可理解为 const 语句块中的行索引)。

例子：

``` go
funcmain() {
    const (
            a =iota   //0
            b          //1
            c          //2
            d ="ha"   //独立值，iota += 1
            e          //"ha"   iota += 1
            f =100   //iota +=1
            g          //100  iota +=1
            h =iota   //7,恢复计数
            i          //8
    )
    fmt.Println(a,b,c,d,e,f,g,h,i)
}
```

结果：0 12 ha ha 100100 7 8

## 变量var：

一般声明：

格式：

var v_name v_type

v_name= value

例子：

```go
var s string
s = “hello world”
```

自行判断声明：

格式：

var v_name =value

例子：

```go
var a = “hello world”
```

简短声明：

格式：v_name := value

例子：a := 50 或 b :=false

a 和 b 的类型（int 和 bool）将由编译器自动推断

这是使用变量的首选形式，但是它只能被用在函数体内，而不可以用于全局变量的声明与赋值。

使用操作符 :=可以高效地创建一个新的变量，称之为初始化声明。

## 匿名变量：

Go语言的函数返回多个值，在不需要使用所有返回值时，可以使用匿名变量“_"下划线代替，匿名变量不占用命名空间，不会分配内存。

## 变量多重赋值：

可以实现变量交换，多个变量同时赋值，减少了代码量。

例子：

```go
x := 10
y := 20
fmt.Println(x,y)
x,y = y,x
fmt,Println(x,y)
```

结果：10 20,20 10