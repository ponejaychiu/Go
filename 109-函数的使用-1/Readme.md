[TOC]

# 一、函数是什么：

1、函数是组织好的、可重复使用的执行特定任务的代码块，可以提高应用程序的模块性和代码的重复利用率；

2、Go语言支持普通函数、匿名函数和闭包，对函数进行了优化和改进，使用更方便；

3、Go语言的函数属于first-class（一等公民）：

- 函数本身可以作为值进行传递；
- 支持匿名函数和闭包；
- 函数可以满足接口；

# 二、函数声明：

1、语法格式：

```go
func 函数名（参数列表） （返回参数列表）{
    //函数体
}
func funcName(paramentername1 type1,parametername2 type2...) (output1 type1,output2 type2...){
    //逻辑代码
    //返回多个值
    return value1,value2...
}
```

2、函数定义解析：

func：函数关键字；

- 由func开始声明；

funcname：函数名字；

- 由数字、字母和下划线组成，且第一个字母不能为数字，同一个包内，函数名不能重名；

parametername type：参数列表；

- 定义函数时的参数叫形式参数（形参），形参变量时函数的局部变量；
- 当函数被调用的时候，可以将值传给形参，这个值被称为实际参数（实参）；
- 参数列表指定的是参数参数类型、顺序及个数，参数列表是可选的，函数可以不包含参数；
- 参数列表的简写：如果有多个参数变量，则以逗号分隔；如果相邻两个变量类型一致，则可以省略类型；例如：func add(a,b int){};
- 函数还支持可变参数，即函数有着不定数量的参数；例如：func mufunc(arg ...int){}，arg ...int告诉Go这个函数接受不定量的参数，注意这些参数类型都是int，在函数体中变量arg是一个int的slice；

output type：返回值列表；

- Go语言的函数可以返回多个值；
- 返回值列表形式可以是：返回数据的数据类型，或者是：变量名+变量类型的组合；
- 函数声明时有返回值，则必须在函数体中使用return语句提供返回值列表；
- 如果只有一个返回值且不声明返回值变量，那么可以省略包括返回值的括号；

函数体：函数定义的代码集合，能够被重复调用的代码片段。

# 三、变量作用域：

1、作用域是变量、常量、类型、函数的作用范围；

Go语言中变量有三个作用范围：

- 函数内定义的变量叫局部变量
- 函数外定义的变量叫全局变量
- 函数中定义的参数叫形式参数

2、局部变量：在函数体内声明的变量，作用域只在函数体内，参数和返回值变量也是局部变量；

3、全局变量：在函数体外声明的变量，可以在整个包甚至外部包（被导出后）使用；全局变量可以在任何函数中使用，Go语言中全局变量和局部变量名称可以相同，但函数内的局部变量会被优先考虑。

4、形式参数：作为函数的局部变量来使用。

实例：

```go
// 声明全局变量
var a = 7
var b = 9

func main() {
	// 声明局部变量
	a, b, c := 10, 20, 30
	fmt.Printf("main函数中a=%d\n", a)
	fmt.Printf("main函数中b=%d\n", b)
	fmt.Printf("main函数中c=%d\n", c)
	// 匿名变量
	c, _ = sum(a, b)
	fmt.Printf("main函数调用sum函数后c=%d\n", c)
	_, c = sum2(a, b)
	fmt.Printf("main函数调用sum2函数后c=%d\n", c)
}

// 第一种方式
func sum(a, b int) (int, int) {
	// 声明局部变量
	a++
	b += 3
	c := a + b
	d := a * b
	fmt.Printf("sum函数中a=%d\n", a)
	fmt.Printf("sum函数中b=%d\n", b)
	fmt.Printf("sum函数中c=%d\n", c)
	return c, d
}

// 第二种方式
func sum2(a, b int) (c, d int) {
	// 声明局部变量
	a++
	b += 3
	c = a + b
	d = a * b
	fmt.Printf("sum2函数中a=%d\n", a)
	fmt.Printf("sum2函数中b=%d\n", b)
	fmt.Printf("sum2函数中c=%d\n", c)
	return
}
结果：
main函数中a=10
main函数中b=20
main函数中c=30
sum函数中a=11
sum函数中b=23
sum函数中c=34
main函数调用sum函数后c=34
sum2函数中a=11
sum2函数中b=23
sum2函数中c=34
main函数调用sum2函数后c=253
```

# 四、函数变量（函数作为值）：

1、在Go语言中，函数也是一种类型，可以和其他类型一样保存在变量里；也可以通过type定义一个自定义类型，定义的函数参数要与实现的函数参数完全相同（包括：参数类型、个数、顺序），函数返回值的类型也要相同；

实例：

```go
//函数普通使用方法：
func main() {
	str := "BAVSNHbsbajhjdbjavHVAHS"
	result := processStr(str)
	fmt.Println(result)
}

// 处理字符串，实现大小写奇数偶数交替
func processStr(str string) string {
	result := ""
	for i, value := range str {
		if i%2 == 0 {
			result += strings.ToUpper(string(value))
		} else {
			result += strings.ToLower(string(value))
		}
	}
	return result
}

//函数变量（函数作为值）的使用方法：
func main() {
	str := "BAVSNHbsbajhjdbjavHVAHS"
	fmt.Println(StringToCase(str, processStr))
}

// 处理字符串，实现大小写奇数偶数交替
func processStr(str string) string {
	result := ""
	for i, value := range str {
		if i%2 == 0 {
			result += strings.ToUpper(string(value))
		} else {
			result += strings.ToLower(string(value))
		}
	}
	return result
}

func StringToCase(str string, upLow func(string) string) string {
	return upLow(str)
}

//函数变量（函数作为值）type的使用方法：
func main() {
	str := "BAVSNHbsbajhjdbjavHVAHS"
	fmt.Println(StringToCase2(str, processStr))
}

// 处理字符串，实现大小写奇数偶数交替
func processStr(str string) string {
	result := ""
	for i, value := range str {
		if i%2 == 0 {
			result += strings.ToUpper(string(value))
		} else {
			result += strings.ToLower(string(value))
		}
	}
	return result
}

type caseFunc func(string) string

func StringToCase2(str string, uplow caseFunc) string {
	return uplow(str)
}
结果都一样为：
BaVsNhBsBaJhJdBjAvHvAhS
```

2、函数变量的使用步骤：

- 定义一个函数类型
- 实现定义的函数类型
- 作为一个参数调用

意义：函数变量的用法类似接口的用法；在函数当作值和类型的时候在一些通用接口的时候非常有用，上面的例子中caseFunc这个类型是一个函数类型，然后processStr函数的参数和返回值与caseFunc类型是一样的；用户可以实现多种逻辑，这样使程序更加的灵活。

实例：

```go
// 遍历切片的奇偶数
// 1、定义一个函数类型
type sliceFunc func(int) bool

func main() {
	arr := []int{12, 3, 5, 78, 9, 321, 44, 239}
	fmt.Println("原始切片为：", arr)
	// 3、作为一个参数调用
	// 获取切片中的奇数元素
	odd := Filter(arr, isOdd)
	fmt.Println("奇数元素：", odd)
	// 获取切片中的偶数元素
	even := Filter(arr, isEven)
	fmt.Println("偶数元素：", even)
}

// 2、实现定义的函数类型
// 判断元素为偶数
func isEven(num int) bool {
	if num%2 == 0 {
		return true
	}
	return false
}

// 判断元素为奇数
func isOdd(num int) bool {
	if num%2 == 0 {
		return false
	}
	return true
}

// 根据函数来处理切片，实现奇偶数分组并返回新的切片
func Filter(arr []int, s sliceFunc) []int {
	var result []int
	for _, value := range arr {
		if s(value) {
			result = append(result, value)
		}
	}
	return result
}
```

上面的例子中**sliceFunc**

```go
type sliceFunc func(int) bool
```

是一个函数类型的数据类型，然后两个**Filter**函数的

```go
odd := Filter(arr, isOdd)--func isOdd(num int) bool--此时函数当作值（变量）传入Filter
even := Filter(arr, isEven)--func isEven(num int) bool
Filter(arr []int, s sliceFunc) []int
```

参数和返回值与**sliceFunc**类型是一样的。

# 五、匿名函数：

1、概念：

Go语言支持匿名函数，即在需要使用函数时，再定义函数，匿名函数没有函数名，只有函数体，函数可以作为一种类型被赋值给变量，匿名函数一般以变量方式被传递；常用于实现回调函数、闭包等。

2、语法格式：

```go
func (参数列表)(返回参数列表){
    //函数体
}
```

3、在定义时调用匿名函数：

```go
func main() {
    // 1、无返回值的情况
	func(data int) {
		fmt.Println("你的成绩是：", data)
	}(98)
    // 2、有返回值的情况
	result := func(data float64) float64 {
		return math.Sqrt(data)
	}(81)
	fmt.Println("平方根是：", result)
}
结果：
你的成绩是： 98
平方根是： 9
```

4、将匿名函数赋值给变量：

```go
func main() {
	myfunc := func(data string) string {
		fmt.Println(data)
		return data
	}
	myfunc("Hello World!!!")
}
//或者
func main() {
	myfunc := func(data string) string {
		return data
	}
	fmt.Println(myfunc("Hello World!!!"))
}
结果：
Hello World!!!
```

5、匿名函数作为回调函数使用：

```go
// 自定义一个函数数据类型
type mySlice func(float64) string

func main() {
	// 定义一个切片，对其中对数据进行平方根或求平方对计算
	arr := []float64{1, 6, 9, 18, 24, 81}
	// 求平方根
	result := FliterSlice(arr, func(v float64) string {
		v = math.Sqrt(v)
		return fmt.Sprintf("%.f", v)
	})
	fmt.Println(result)
	// 求N次方
	result = FliterSlice(arr, func(v float64) string {
		v = math.Pow(v, 2)
		return fmt.Sprintf("%.f", v)
	})
	fmt.Println(result)
}

// 遍历切片，对其中对元素进行计算
func FliterSlice(arr []float64, slice mySlice) []string {
	var result []string
	for _, value := range arr {
		result = append(result, slice(value))
	}
	return result
}
```