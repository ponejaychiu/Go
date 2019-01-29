[TOC]

# 六、闭包：

## （一）、概念：

英文：closure，在实现深约束时，需要创建一个能显式表示引用环境的东西，并将它与相关的子程序捆绑在一起，这样捆绑起来的整体被称为闭包；即函数+引用环境=闭包。闭包是一个函数值，它来自函数体外部的变量引用。

1、闭包只是形式上像函数，但实际上不是函数；

函数是一些可执行的代码，这些代码在函数被定义后就确定了，不会在执行时发生变化，所以一个函数只有一个实例；

闭包在运行时可以有多个实例，不同的引用环境和相同的函数组合可以产生不同的实例。

2、闭包在某些编程语言中被称为lambda表达式；

3、函数本身不存储任何信息，只有与引用环境结合后形成的闭包才具有“记忆性”，函数是编译器静态的概念，而闭包是编译器运行时动态的概念；

4、对象是附有行为的数据，而闭包是附有数据的行为；

5、闭包函数的返回值一定是一个函数类型；

## （二）、闭包的价值：

1、加强模块化：

​        有益于模块化编程，以简单的方式开发较小的模块，从而提高开发速度和程序的可复用性；和其他没有闭包的程序相比，可以将模块划分的更小；

​        比如需要计算一个数组中所有元素的和，这只需要遍历数组，把遍历到的元素加起来即可；如果又需要计算数组中所有元素的乘积呢？打印所有的元素呢？解决这些问题就需要遍历数组，在没有闭包的程序中需要一遍又一遍的写循环语句，而支持闭包的程序中则不需要；这种处理方法有点儿像回调函数，不过要比回调函数写法更简单，功能更强大。

2、抽象：

闭包是数据和行为的组合，这使得闭包具有较好的抽象能力；

3、简化代码；

## （三）、编程语言需要哪些特性来支持闭包呢？

- 函数是一阶值（first-class value，一等公民），即函数可以作为另一个函数的返回值或参数，还可以作为一个变量的值；
- 函数可以嵌套定义，即一个函数内部定义另一个函数；
- 允许定义匿名函数；
- 可以获取引用环境，并把引用环境和函数代码组成一个可调用的实体；

## （四）、闭包实例：

```go
func myAdder() func(int) int {
	sum := 0
	return func(i int) int {
		fmt.Printf("加i前sum=%d\t", sum)
		sum += i
		fmt.Printf("加i后sum=%d\t", sum)
		return sum
	}
}

func demoFunc1() {
	pos, neg := myAdder(), myAdder()
	for i := 0; i < 5; i++ {
		fmt.Printf("i=%d\t", i)
		fmt.Printf("pos=%d,neg=%d\n", pos(i), neg(-i*2))
	}
}
func main() {
	demoFunc1()
}
结果：
i=0	加i前sum=0	加i后sum=0	加i前sum=0	加i后sum=0	pos=0,neg=0
i=1	加i前sum=0	加i后sum=1	加i前sum=0	加i后sum=-2	pos=1,neg=-2
i=2	加i前sum=1	加i后sum=3	加i前sum=-2	加i后sum=-6	pos=3,neg=-6
i=3	加i前sum=3	加i后sum=6	加i前sum=-6	加i后sum=-12	pos=6,neg=-12
i=4	加i前sum=6	加i后sum=10	加i前sum=-12	加i后sum=-20	pos=10,neg=-20

解释：以pos(i)为例，每次计算完之后，sum常量保存了计算的结果：
函数   ---加i前sum值  ---i值  ---return后（加i后）的sum值
pos(0) --- 0            0           0
pos(1) --- 0            1           1
pos(2) --- 1            2           3
pos(3) --- 3            3           6
pos(4) --- 6            4           10
```

使用和不使用闭包的区别：

```go
func main() {
	// 循环调用add函数
	fmt.Println("不使用闭包：")
	for i := 0; i < 5; i++ {
		fmt.Printf("i=%d\t", i)
		fmt.Println(add(i))
	}
	// 调用adder函数
	result := adder()
	fmt.Println("使用闭包：")
	//fmt.Printf("变量result的类型为：%T\n", result)
	for i := 0; i < 5; i++ {
		fmt.Printf("i=%d\t", i)
		fmt.Println(result(i))
	}
}

// 不使用闭包的时候，实现不了计数的函数
func add(i int) int {
	sum := 0
	fmt.Printf("加i前sum=%d\t", sum)
	sum += i
	fmt.Printf("加i后sum=%d\t", sum)
	return sum
}

// 使用闭包的时候，实现计数的函数
func adder() func(int) int {
	sum := 0
	return func(i int) int {
		fmt.Printf("加i前sum=%d\t", sum)
		sum += i
		fmt.Printf("加i后sum=%d\t", sum)
		return sum
	}
}
结果：
不使用闭包：
i=0	加i前sum=0	加i后sum=0	0
i=1	加i前sum=0	加i后sum=1	1
i=2	加i前sum=0	加i后sum=2	2
i=3	加i前sum=0	加i后sum=3	3
i=4	加i前sum=0	加i后sum=4	4
使用闭包：
i=0	加i前sum=0	加i后sum=0	0
i=1	加i前sum=0	加i后sum=1	1
i=2	加i前sum=1	加i后sum=3	3
i=3	加i前sum=3	加i后sum=6	6
i=4	加i前sum=6	加i后sum=10	10
```

闭包计数器功能：

```go
func main() {
	res := counter()
	fmt.Printf("res()的类型：%T\n", res())
	fmt.Printf("res的类型：%T\n", res)
	fmt.Println("res():", res())
	fmt.Println("res():", res())
	fmt.Println("res():", res())

	res2 := counter()
	fmt.Println("res2:", res2)
	fmt.Println("res2():", res2())
	fmt.Println("res2():", res2())
	fmt.Println("res2():", res2())

}

// 闭包函数，实现计数器功能
func counter() func() int {
	i := 0
	res := func() int {
		i++
		return i
	}
	fmt.Println("counter内部:", res)
	return res
}
结果：
counter内部: 0x1093870
res()的类型：int
res的类型：func() int
res(): 2
res(): 3
res(): 4
counter内部: 0x1093870
res2: 0x1093870
res2(): 1
res2(): 2
res2(): 3
```

匿名闭包函数写法：

```go
func main() {
	// 定义匿名闭包函数
	res := func() (func() int) {
		i := 10
		return func() int {
			i++
			return i
		}
	}()
	fmt.Println(res())
}

func main() {
	// 定义匿名闭包函数
	res := func() (func() int) {
		i := 10
		return func() int {
			i++
			return i
		}
	}
	fmt.Println(res())
}
结果：
11
0x10914e0
总结：匿名函数的调用需要在定义变量之后加上一组括号；
```

# 七、函数可变参数：

## （一）、概念：

如果一个函数的参数列表，类型一致，但个数不一致，可以使用函数的可变参数；

## （二）、语法格式：

```go
func 函数名(参数名...类型) (返回值列表){
    //函数体
}
```

- 该语法格式定义了一个接受任何数量、任何类型参数的函数，这里的语法是三个点**…**，在一个变量后面加上三个点，表示从该处开始接受不定（可变）参数；
- 当要传递若个值到不定参数函数中的时候，可以手动写每个参数，也可以将一个slice传递给该函数，通过**…**可以将slice中的参数对应的传递给函数。

## （三）、函数可变参数实例：

```go
// 可变参数，一个返回值
func main() {
	// 函数可变参数可以传0-n个参数
	fmt.Println(addSum(1, 2, 3, 4, 5, 6, 7))
	fmt.Println(addSum())
	// 传slice切片
	arr := []int{4, 6, 9, 23, 45, 7, 89}
	fmt.Println(addSum(arr...))
}

// 累加求和，参数不定，0-n个都可
func addSum(nums ...int) (sum int) {
	fmt.Printf("nums的类型：%T\n", nums)
	for _, value := range nums {
		sum += value
	}
	return
}
结果：
nums的类型：[]int
28
nums的类型：[]int
0
nums的类型：[]int
183

// 可变参数，多个返回值
func main() {
	// 第一种方式传入
	total, avg, count := getScores(89, 78, 69, 99)
	fmt.Printf("学生一共%d门成绩，总成绩：%.2f，平均分：%.2f\n", count, total, avg)
	// 第二种切片方式传入
	arr := []float64{90, 80.5, 60, 98.5, 78}
	total, avg, count = getScores(arr...)
	fmt.Printf("学生一共%d门成绩，总成绩：%.2f，平均分：%.2f\n", count, total, avg)
}

// 计算考试成绩总分和平均分
func getScores(scores ...float64) (total, avg float64, count int) {
	for _, value := range scores {
		total += value
		count++
	}
	avg = total / float64(count)
	return
}
结果：
学生一共4门成绩，总成绩：335.00，平均分：83.75
学生一共5门成绩，总成绩：407.00，平均分：81.40
```

## （四）、注意点：

- 一个函数最多只能出现一个可变参数；
- 参数列表中如果有其他类型参数，则这个可变参数要写在其他所有参数的最后；

```go
func main() {
	// 第一种方式传入
	total, avg, count, name := getScores("小明", 89, 78, 69, 99)
	fmt.Printf("学生%s一共%d门成绩，总成绩：%.2f，平均分：%.2f\n", name, count, total, avg)
	// 第二种切片方式传入
	arr := []float64{90, 80.5, 60, 98.5, 78}
	total, avg, count, name = getScores("小红", arr...)
	fmt.Printf("学生%s一共%d门成绩，总成绩：%.2f，平均分：%.2f\n", name, count, total, avg)
}

// 计算考试成绩总分和平均分
func getScores(user string, scores ...float64) (total, avg float64, count int, name string) {
	for _, value := range scores {
		total += value
		count++
	}
	name = user
	avg = total / float64(count)
	return
}
结果：
学生小明一共4门成绩，总成绩：335.00，平均分：83.75
学生小红一共5门成绩，总成绩：407.00，平均分：81.40
```

# 八、递归函数：

## （一）、概念：

在函数内部，可以调用其他函数，如果一个函数在内部调用函数自身，则称之为递归函数，满足条件：

- 在第一次调用自己时，必须是（在某种意义上）更接近于结果；
- 必须要有一个终止处理或计算的准则；

## （二）、递归函数案例：

```go
func main() {
	fmt.Println(getMultiple(5))
	fmt.Println(factorial(5))
}

// 用递归实现阶乘
func factorial(num int) int {
	if num == 0 {
		return 1
	}
	return num * factorial(num-1)
}

// 用循环实现阶乘
func getMultiple(num int) (result int) {
	result = 1
	for i := 1; i <= num; i++ {
		result *= i
	}
	return
}
结果：
120
120
```

## （三）、注意事项：

1、递归的计算过程（上面的例子中）：

```go
===>factorial(5)
===>5*factorial(4)
===>5*(4*factorial(3))
===>5*(4*(3*factorial(2)))
===>5*(4*(3*(2*factorial(1))))
===>5*(4*(3*(2*1)))
===>5*(4*(3*2))
===>5*(4*6)
===>5*24
===>120
```

2、优缺点：

- 定义简单、逻辑清晰。理论上所有递归函数都可以用循环的方式实现，但循环的逻辑不如递归清晰；
- 使用递归函数要注意防止栈溢出，递归用的次数过多（过深的调用）时会导致栈溢出；
- 函数调用是通过栈（stack）这种数据结构实现的，每当进入一个函数调用，栈就会加一层栈帧，每当函数返回，栈就会减一层栈帧。由于栈的大小不是无限的，所以，递归调用的次数过多，会导致栈溢出。
- 每个栈帧对应着一个未运行完的函数。栈帧中保存了该函数的返回地址和局部变量。

# 八、指针：

## （一）、概念：

1、指针是存储另一个变量的内存地址的变量；

变量是一种使用方便的占位符，变量都指向计算机的内存地址；

一个指针变量可以指向任何一个值的内存地址；

如：变量b的值为123，存储在内存地址：0xc00004a080；指针变量a持有b的地址，则a被认为指向b；b=123，a=0xc00004a080

2、获取指针变量的地址：

Go语言的取地址符&，一个变量前使用&，会返回该变量的内存地址；

```go
func main() {
	a := 123
	fmt.Printf("%p\n", &a)
	fmt.Printf("%x\n", &a)
	fmt.Printf("%X\n", &a)
}
结果：
0xc00004a080
c00004a080
C00004A080
```
3、Go语言指针的特点：

- 指针不能运算（不同意C语言）；
- 如果对指针进行运算会报错：Invalid operation: &a++ (non-numeric type *int)

## （二）、指针的使用：

1、声明指针：

*T是指针变量的类型，它指向T类型的值；

格式：var 指针变量名 *指针类型（即数据类型）

- *号用于指定变量是一个指针；
- var ip *int  //指向整型的指针；
- var fp *float64 //指向浮点型的指针；

2、使用指针（使用流程）：

- 先定义指针变量；
- 为指针变量赋值；
- 访问指针变量中指向地址的值；
- 获取指针变量的值：在指针类型的变量前加上*号（前缀）来获取指针所指向的内容；
- 获取一个指针意味着访问指针指向的变量的值，语法是：*a

3、实例：

```go
// 普通数据类型
func main() {
	// 实际变量
	a := 120
	// 声明指针变量
	var ip *int
	// 给指针变量ip赋值，将变量a的地址赋值给ip
	ip = &a
	// 打印a的类型和值
	fmt.Printf("a的类型是%T,值是%v\n", a, a)
	// 打印&a的类型和值
	fmt.Printf("&a的类型是%T,值是%v\n", &a, &a)
	// 打印*&a的类型和值
	fmt.Printf("*&a的类型是%T,值是%v\n", *&a, *&a)
	// 打印ip的类型和值
	fmt.Printf("ip的类型是%T,值是%v\n", ip, ip)
	// 打印*ip的类型和值
	fmt.Printf("*ip的类型是%T,值是%v\n", *ip, *ip)
	fmt.Println(a, &a, *&a)
	fmt.Println(ip, &ip, *(&ip), *ip, &(*ip))
}
a的类型是int,值是120
&a的类型是*int,值是0xc00004a080
*&a的类型是int,值是120
ip的类型是*int,值是0xc00004a080
*ip的类型是int,值是120
120 0xc00004a080 120
0xc00004a080 0xc000072018 0xc00004a080 120 0xc00004a080

// 符合数据类型
type student struct {
	name    string
	age     int
	married bool
	sex     int8
}

func main() {
	s1 := student{"jay", 18, true, 1}
	s2 := student{"marry", 21, false, 0}

	var a *student = &s1
	//var b *student = &s2
	b := &s2

	fmt.Printf("s1的类型是%T，值是%v\n", s1, s1)
	fmt.Printf("s2的类型是%T，值是%v\n", s2, s2)
	fmt.Println("-------------")
	fmt.Printf("a的类型是%T，值是%v\n", a, a)
	fmt.Printf("b的类型是%T，值是%v\n", b, b)
	fmt.Println("-------------")
	fmt.Printf("*a的类型是%T，值是%v\n", *a, *a)
	fmt.Printf("*b的类型是%T，值是%v\n", *b, *b)
	fmt.Println("-------------")
	fmt.Println(s1.name, s2.name)
	fmt.Println(a.name, b.name)
	fmt.Println(&a, &a.name, &a.age, &a.married, &a.sex)
}

结果：
s1的类型是main.student，值是{jay 18 true 1}
s2的类型是main.student，值是{marry 21 false 0}
-------------
a的类型是*main.student，值是&{jay 18 true 1}
b的类型是*main.student，值是&{marry 21 false 0}
-------------
*a的类型是main.student，值是{jay 18 true 1}
*b的类型是main.student，值是{marry 21 false 0}
-------------
jay marry
jay marry
0xc000072018 0xc000044400 0xc000044410 0xc000044418 0xc000044419

总结：
&取出来的是一个变量的内存地址，*取出来的是一个变量地址对应的值；
```

## （三）、空指针：

1、概念：

- 当一个指针被定义后没有分配到任何变量时，它的值为nil；
- nil指针也称为空指针；
- nil在概念上和其他语言的null、None、NULL一样，都指代0或者空值；
- 一个指针变量通常缩写为ptr；

2、空指针判断：

```go
if(ptr != nil) // ptr不是空指针
if(ptr == nil) // ptr是空指针
```

3、实例：

```
func main() {
	var ptr *int
	fmt.Printf("ptr类型为%T，值为%v\n", ptr, ptr)

	if ptr == nil {
		fmt.Println("当前指针ptr是一个空指针")
	} else {
		fmt.Println("当前指针ptr是一个非空指针")
	}
}
结果：
ptr类型为*int，值为<nil>
当前指针ptr是一个空指针
```

## （四）、操作指针改变变量的数值：

```go
func main() {
	a := 10
	// 定义指针变量的方式
	//b := &a
	var b *int = &a
	fmt.Printf("指针变量b的数据类型为：%T，指针变量b的值为：%v\n", b, b)
	fmt.Println("a的当前地址（即指针变量b的值）为：", b)
	fmt.Println("用*取指针变量b的当前值：", *b)
	// 对指针变量b进行++的操作
	*b++
	fmt.Println("a的值：", a)
}
结果：
指针变量b的数据类型为：*int，指针变量b的值为：0xc00004a080
a的当前地址（即指针变量b的值）为： 0xc00004a080
用*取指针变量b的当前值： 10
a的值： 11
```

## （五）、使用指针作为函数的参数：

```go
// 基本数据类型的指针变量作为函数的参数
func main() {
	// 定义变量a
	a := 10
	fmt.Println("函数调用前a的值：", a)
	// 将a的地址赋值定义给指针变量b
	b := &a
	// 调用包含有指针变量作为参数的change函数
	change(b)
	fmt.Println("函数调用后a的值：", a)
}

func change(num *int) {
	*num = 20
}
结果：
函数调用前a的值： 10
函数调用后a的值： 20

func main() {
	// 定义两个局部变量
	a, b := 100, 200
	// 普通方式
	fmt.Println(swap1(a, b))
	// 指针方式
	swap2(&a, &b)
	fmt.Println(a, b)
}

// 实现两个数据交换，传统写法
func swap1(x, y int) (int, int) {
	return y, x
}

// 定义指针变量作为参数的函数，不需要返回值
func swap2(x, y *int) {
	*x, *y = *y, *x
}
结果：
200 100
200 100

// 切片数据类型的指针变量作为函数的参数（将一个指向切片的指针传递给函数）
func main() {
	// 定义切片
	a := []int{123, 234, 456}
	// 调用函数modify前的a
	fmt.Println(a)
	// 调用函数modify
	modify(&a)
	// 调用函数modify后的a
	fmt.Println(a)
}

// 定义修改切片的函数
func modify(arr *[]int) {
	(*arr)[0] = 250
}
```

虽然将指针传递给一个切片作为函数的参数，可以实现对切片中元素的修改，但这并不是实现这一目标的惯用方法，惯用方法是使用切片。

## （六）、指针数组：

1、指针数组：就是元素为指针变量类型的数组；

- 定义一个指针数组，例如：var ptr [3]*string;
- 有一个元素个数相同的数组，将该数组中每个元素的地址赋值给该指针数组，也就是说该指针数组与某一个数组切片完全对应；
- 可以通过*指针变量获取到该地址对应的数组元素值；

2、实例：

```go
// 指针数组的使用
const count = 3

func main() {
	// 数组的指针
	a := [count]string{"jay", "chiu", "pone"}
	fmt.Printf("%T,%v\n", &a, &a)
	// 定义一个指针数组
	var ptr [count]*string
	fmt.Printf("%T,%v\n", ptr, ptr)
	// 给指针数组赋值
	for i := 0; i < count; i++ {
		// 将数组中每个元素的地址依次赋值指针数组的每个元素
		ptr[i] = &a[i]
	}
	fmt.Printf("%T,%v\n", ptr, ptr)
	fmt.Println(ptr[:])
	// 根据指针数组元素的每个地址来获取该地址所指向的元素的真实数值
	fmt.Println(*ptr[0])
	// 第一种方式遍历ptr指针数组
	for i := 0; i < count; i++ {
		fmt.Println(*ptr[i])
	}
	// 第二种方式（相当于while循环）遍历ptr指针数组
	for _, value := range ptr {
		fmt.Println(*value)
	}
}
结果：
*[3]string,&[jay chiu pone]
[3]*string,[<nil> <nil> <nil>]
[3]*string,[0xc00005e240 0xc00005e250 0xc00005e260]
[0xc00005e240 0xc00005e250 0xc00005e260]
jay
jay
chiu
pone
jay
chiu
pone
```

## （七）、指针的指针：

1、概念：

如果一个指针变量存放的是另一个指针变量的地址，则称这个指针变量为指向指针的指针变量；

当定义一个指向指针的指针变量时，第一个指针存放第二个指针的地址，第二个指针存放变量的地址；

2、格式：

- var ptr **int
- 以上指向指针的指针变量为整型；
- 访问指向指针的指针变量的值时需要使用两个*号；

3、实例：

```go
func main() {
	var a int
	// 定义指针
	var ptr *int
	// 定义指向指针的指针
	var pptr **int
	a = 123
	// 为指针赋值
	ptr = &a
	fmt.Println("第二个指针ptr：", ptr)
	// 为指针的指针赋值
	pptr = &ptr
	fmt.Println("第一个指针pptr：", pptr)
	// 访问变量a的值
	fmt.Printf("变量a=%v\n", a)
	// 访问指针ptr的值
	fmt.Printf("第二个指针变量ptr的地址：%v\n", ptr)
	fmt.Printf("指针变量ptr地址存放的变量值：%v\n", *ptr)
	// 访问指针指向的指针变量pptr的值
	fmt.Printf("第一个指向指针的指针pptr的地址：%v\n", pptr)
	fmt.Printf("第一个指向指针的指针pptr的地址所存放的值是第二个指针ptr的地址：%v\n", *pptr)
	fmt.Printf("第二个指针ptr的地址所存放的是变量的值：%v\n", **pptr)
}
结果：
第二个指针ptr： 0xc00004a080
第一个指针pptr： 0xc000072018
变量a=123
第二个指针变量ptr的地址：0xc00004a080
指针变量ptr地址存放的变量值：123
第一个指向指针的指针pptr的地址：0xc000072018
第一个指向指针的指针pptr的地址所存放的值是第二个指针ptr的地址：0xc00004a080
第二个指针ptr的地址所存放的是变量的值：123
```

# 九、函数参数传递（值传递与引用传递）：

## （一）、概念：

函数如果使用参数，该参数变量称为函数的形参，形参就像定义在函数体内的局部变量；调用函数，可以通过两种方式来传递参数，即值传递和引用传递，或者叫做传值和传引用。

1、值传递（传值）：

- **是指在调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响原内容数据；**
- 默认情况下Go语言使用的是值传递，即在调用过程中不会影响到原内容的数据；
- 每次调用函数，都将实参拷贝一份再传递到函数中，每次拷贝一份，性能是否就下降了呢？其实在Go语言中使用指针和值传递配合就避免了性能降低问题，也就是通过传指针参数来解决实参拷贝问题。

2、引用传递（传引用）：

- **是指在调用函数时将实际参数的地址传递到函数中，这样在函数中如果对参数进行修改，将会影响原内容的数据；**
- **严格来说Go语言中没有引用传递这种形式，只有值传递一种传参方式；**
- Go语言可以借助指针来实现引用传递的效果，函数参数使用指针参数，传参时其实是在拷贝一份指针参数，也就是拷贝了一份指针变量地址；
- 此时仅仅是拷贝一个指针，也就是一个内存地址，这样就不用担心实参拷贝造成的内存浪费、时间开销、性能降低的情况。

3、引用传递的作用：

- 传指针使得多个函数能操作同一个对象；


- 传指针更轻量（8bytes），只需要传内存地址，所以当要传递大的结构体的时候，用指针是一个常用的方法；
- **Go语言中slice、map、chan类型的实现机制都是类似指针，所以可以直接传递，而不必取地址后传递指针；**

4、实例：

```go
// int类型的传值和传引用
func main() {
	num := 10
	fmt.Printf("1、变量num传递前，内存地址为：%p，值为：%v\n", &num, num) // 原内存地址，10
	// 调用传值函数
	changeVal(num)
	fmt.Printf("2、调用changeVal函数后，变量num的内存地址：%p，值为：%v\n", &num, num) // 原内存地址，10
	// 调用传引用（即传指针参数）函数
	changePtr(&num)
	fmt.Printf("3、调用changePtr函数后，变量num的内存地址：%p，值为：%v\n", &num, num) // 原内存地址，值变为200
}

// 传值
func changeVal(num int) {
	fmt.Printf("---changeVal函数内，值参数num的内存地址：%p，值为：%v\n", &num, num) // 新复制的内存地址，10
	num = 100
}

// 传引用（即传指针参数）
func changePtr(num *int) {
	fmt.Printf("---changePtr函数内，指针参数num的内存地址：%p，值为：%v\n", &num, num) // 新复制的内存地址，原内存地址
	*num = 200
}
结果：
1、变量num传递前，内存地址为：0xc00004a080，值为：10
---changeVal函数内，值参数num的内存地址：0xc00004a0a0，值为：10
2、调用changeVal函数后，变量num的内存地址：0xc00004a080，值为：10
---changePtr函数内，指针参数num的内存地址：0xc000072020，值为：0xc00004a080
3、调用changePtr函数后，变量num的内存地址：0xc00004a080，值为：200
```

```go
// string类型的传值和传引用
func main() {
	str := "abcdef"
	fmt.Printf("1、变量str传递前，内存地址为：%p，值为：%v\n", &str, str) // 原内存地址
	// 调用传值函数
	changeStrVal(str)
	fmt.Printf("2、调用changeStrVal函数后，变量str的内存地址：%p，值为：%v\n", &str, str) // 原内存地址
	// 调用传引用（即传指针参数）函数
	changeStrPtr(&str)
	fmt.Printf("3、调用changeStrPtr函数后，变量str的内存地址：%p，值为：%v\n", &str, str) // 原内存地址
}

// 传值
func changeStrVal(str string) {
	fmt.Printf("---changeStrVal函数内，值参数str的内存地址：%p，值为：%v\n", &str, str) // 新复制的内存地址
	str = strings.ToUpper(str)
}

// 传引用（即传指针参数）
func changeStrPtr(str *string) {
	fmt.Printf("---changeStrPtr函数内，指针参数str的内存地址：%p，值为：%v\n", &str, str) // 新复制的内存地址，原内存地址
	*str = strings.ToUpper(*str)
}
结果：
1、变量str传递前，内存地址为：0xc00003e1c0，值为：abcdef
---changeStrVal函数内，值参数str的内存地址：0xc00003e1e0，值为：abcdef
2、调用changeStrVal函数后，变量str的内存地址：0xc00003e1c0，值为：abcdef
---changeStrPtr函数内，指针参数str的内存地址：0xc000072020，值为：0xc00003e1c0
3、调用changeStrPtr函数后，变量str的内存地址：0xc00003e1c0，值为：ABCDEF
```

```go
// bool类型的传值和传引用
func main() {
	str := true
	fmt.Printf("1、变量str传递前，内存地址为：%p，值为：%v\n", &str, str) // 原内存地址
	// 调用传值函数
	changeBoolVal(str)
	fmt.Printf("2、调用changeBoolVal函数后，变量str的内存地址：%p，值为：%v\n", &str, str) // 原内存地址
	// 调用传引用（即传指针参数）函数
	changeBoolPtr(&str)
	fmt.Printf("3、调用changeBoolPtr函数后，变量str的内存地址：%p，值为：%v\n", &str, str) // 原内存地址
}

// 传值
func changeBoolVal(str bool) {
	fmt.Printf("---changeBoolVal函数内，值参数str的内存地址：%p，值为：%v\n", &str, str) // 新复制的内存地址
	str = false
}

// 传引用（即传指针参数）
func changeBoolPtr(str *bool) {
	fmt.Printf("---changeBoolPtr函数内，指针参数str的内存地址：%p，值为：%v\n", &str, str) // 新复制的内存地址，原内存地址
	*str = false
}
结果：
1、变量str传递前，内存地址为：0xc00004a080，值为：true
---changeBoolVal函数内，值参数str的内存地址：0xc00004a08c，值为：true
2、调用changeBoolVal函数后，变量str的内存地址：0xc00004a080，值为：true
---changeBoolPtr函数内，指针参数str的内存地址：0xc000072020，值为：0xc00004a080
3、调用changeBoolPtr函数后，变量str的内存地址：0xc00004a080，值为：false
```

## （二）、引用类型数据类型和非引用类型数据类型的区别：

```go
// 数组array类型的传值和传引用，数组属于非引用类型
func main() {
	arr := [4]int{1, 2, 3, 4}
	fmt.Printf("1、变量arr传递前，内存地址为：%p，值为：%v\n", &arr, arr) // 原内存地址
	// 调用传值函数
	changeArrayVal(arr)
	fmt.Printf("2、调用changeArrayVal函数后，变量arr的内存地址：%p，值为：%v\n", &arr, arr) // 原内存地址
	// 调用传引用（即传指针参数）函数
	changeArrayPtr(&arr)
	fmt.Printf("3、调用changeArrayPtr函数后，变量arr的内存地址：%p，值为：%v\n", &arr, arr) // 原内存地址
}

// 传值
func changeArrayVal(arr [4]int) {
	fmt.Printf("---changeArrayVal函数内，值参数arr的内存地址：%p，值为：%v\n", &arr, arr) // 新复制的内存地址
	arr[1] = 10
}

// 传引用（即传指针参数）
func changeArrayPtr(arr *[4]int) {
	fmt.Printf("---changeArrayPtr函数内，指针参数arr的内存地址：%p，值为：%v\n", &arr, arr) // 新复制的内存地址，变量arr的指针地址
	(*arr)[2] = 20
}
结果：
1、变量arr传递前，内存地址为：0xc0000480c0，值为：[1 2 3 4]
---changeArrayVal函数内，值参数arr的内存地址：0xc000048100，值为：[1 2 3 4]
2、调用changeArrayVal函数后，变量arr的内存地址：0xc0000480c0，值为：[1 2 3 4]
---changeArrayPtr函数内，指针参数arr的内存地址：0xc000072020，值为：&[1 2 3 4]
3、调用changeArrayPtr函数后，变量arr的内存地址：0xc0000480c0，值为：[1 2 20 4]

// 切片slice类型的传值和传引用，切片属于引用类型
func main() {
	arr := []int{1, 2, 3, 4}
	fmt.Printf("1、变量arr传递前，内存地址为：%p，值为：%v\n", &arr, arr) // 原内存地址
	// 调用传值函数
	changeArrayVal(arr)
	fmt.Printf("2、调用changeArrayVal函数后，变量arr的内存地址：%p，值为：%v\n", &arr, arr) // 原内存地址
	// 调用传引用（即传指针参数）函数
	changeArrayPtr(&arr)
	fmt.Printf("3、调用changeArrayPtr函数后，变量arr的内存地址：%p，值为：%v\n", &arr, arr) // 原内存地址
}

// 传值
func changeArrayVal(arr []int) {
	fmt.Printf("---changeArrayVal函数内，值参数arr的内存地址：%p，值为：%v\n", &arr, arr) // 新复制的内存地址
	arr[1] = 10
}

// 传引用（即传指针参数）
func changeArrayPtr(arr *[]int) {
	fmt.Printf("---changeArrayPtr函数内，指针参数arr的内存地址：%p，值为：%v\n", &arr, arr) // 新复制的内存地址，变量arr的指针地址
	(*arr)[2] = 20
}
结果：
1、变量arr传递前，内存地址为：0xc000044400，值为：[1 2 3 4]
---changeArrayVal函数内，值参数arr的内存地址：0xc000044460，值为：[1 2 3 4]
2、调用changeArrayVal函数后，变量arr的内存地址：0xc000044400，值为：[1 10 3 4]
---changeArrayPtr函数内，指针参数arr的内存地址：0xc000072020，值为：&[1 10 3 4]
3、调用changeArrayPtr函数后，变量arr的内存地址：0xc000044400，值为：[1 10 20 4]
```

## （三）、**重要细节总结：**

1、Go语言中所以的传参都是值传递（传值），都是一个副本，一个拷贝；

- 拷贝的内容有时候是**非引用类型**（int、string、bool、数组、struct属于非引用类型），这样在函数中就无法修改原内容数据；
- 有的是**引用类型**（指针、slice、map、chan属于引用类型），这样在函数中就可以修改原内容数据；
- Go语言中数据类型分为普通数据类型和复合数据类型，按照引用标准又分为引用类型和非引用类型；

2、是否可以修改原内容数据，和传值、传引用没有必然的关系。在Go语言中，虽然只有传值，但是我们也可以修改原内容数据，因为参数可以是引用类型。

3、传引用和引用类型是两个概念，Go语言虽然只有传值一种方式，但是可以通过传引用类型变量达到跟传引用一样的效果。

4、实例：

```go
// struct类型属于非引用类型
// 定义一个struct类型
type teacher struct {
	name    string
	age     int
	married bool
	sex     int8
}

func main() {
	arr := teacher{"jay", 29, true, 1}
	fmt.Printf("1、变量arr传递前，内存地址为：%p，值为：%v\n", &arr, arr) // 原内存地址
	// 调用传值函数
	changeStructVal(arr)
	fmt.Printf("2、调用changeStructVal函数后，变量arr的内存地址：%p，值为：%v\n", &arr, arr) // 原内存地址
	// 调用传引用（即传指针参数）函数
	changeStructPtr(&arr)
	fmt.Printf("3、调用changeStructPtr函数后，变量arr的内存地址：%p，值为：%v\n", &arr, arr) // 原内存地址
}

// 传值
func changeStructVal(arr teacher) {
	fmt.Printf("---changeStructVal函数内，值参数arr的内存地址：%p，值为：%v\n", &arr, arr) // 新复制的内存地址
	arr.name = "chiu"
	arr.age = 38
	arr.married = false
}

// 传引用（即传指针参数）
func changeStructPtr(arr *teacher) {
	fmt.Printf("---changeStructPtr函数内，指针参数arr的内存地址：%p，值为：%v\n", &arr, arr) // 新复制的内存地址，变量arr的指针地址
	arr.name = "小红"
	arr.age = 25
	arr.married = false
}
结果：
1、变量arr传递前，内存地址为：0xc000044400，值为：{jay 29 true 1}
---changeStructVal函数内，值参数arr的内存地址：0xc000044460，值为：{jay 29 true 1}
2、调用changeStructVal函数后，变量arr的内存地址：0xc000044400，值为：{jay 29 true 1}
---changeStructPtr函数内，指针参数arr的内存地址：0xc000072020，值为：&{jay 29 true 1}
3、调用changeStructPtr函数后，变量arr的内存地址：0xc000044400，值为：{小红 25 false 1}
```

