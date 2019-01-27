[TOC]

# 六、闭包：

（一）、概念：

英文：closure，在实现深约束时，需要创建一个能显式表示引用环境的东西，并将它与相关的子程序捆绑在一起，这样捆绑起来的整体被称为闭包；即函数+引用环境=闭包。闭包是一个函数值，它来自函数体外部的变量引用。

1、闭包只是形式上像函数，但实际上不是函数；

函数是一些可执行的代码，这些代码在函数被定义后就确定了，不会在执行时发生变化，所以一个函数只有一个实例；

闭包在运行时可以有多个实例，不同的引用环境和相同的函数组合可以产生不同的实例。

2、闭包在某些编程语言中被称为lambda表达式；

3、函数本身不存储任何信息，只有与引用环境结合后形成的闭包才具有“记忆性”，函数是编译器静态的概念，而闭包是编译器运行时动态的概念；

4、对象是附有行为的数据，而闭包是附有数据的行为；

5、闭包函数的返回值一定是一个函数类型；

（二）、闭包的价值：

1、加强模块化：

​        有益于模块化编程，以简单的方式开发较小的模块，从而提高开发速度和程序的可复用性；和其他没有闭包的程序相比，可以将模块划分的更小；

​        比如需要计算一个数组中所有元素的和，这只需要遍历数组，把遍历到的元素加起来即可；如果又需要计算数组中所有元素的乘积呢？打印所有的元素呢？解决这些问题就需要遍历数组，在没有闭包的程序中需要一遍又一遍的写循环语句，而支持闭包的程序中则不需要；这种处理方法有点儿像回调函数，不过要比回调函数写法更简单，功能更强大。

2、抽象：

闭包是数据和行为的组合，这使得闭包具有较好的抽象能力；

3、简化代码；

（三）、编程语言需要哪些特性来支持闭包呢？

- 函数是一阶值（first-class value，一等公民），即函数可以作为另一个函数的返回值或参数，还可以作为一个变量的值；
- 函数可以嵌套定义，即一个函数内部定义另一个函数；
- 允许定义匿名函数；
- 可以获取引用环境，并把引用环境和函数代码组成一个可调用的实体；

（四）、闭包实例：

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

（一）、概念：

如果一个函数的参数列表，类型一致，但个数不一致，可以使用函数的可变参数；

（二）、语法格式：

```go
func 函数名(参数名...类型) (返回值列表){
    //函数体
}
```

- 该语法格式定义了一个接受任何数量、任何类型参数的函数，这里的语法是三个点**…**，在一个变量后面加上三个点，表示从该处开始接受不定（可变）参数；
- 当要传递若个值到不定参数函数中的时候，可以手动写每个参数，也可以将一个slice传递给该函数，通过**…**可以将slice中的参数对应的传递给函数。

（三）、实例：

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

（四）、注意点：

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

（一）、概念：

在函数内部，可以调用其他函数，如果一个函数在内部调用函数自身，则称之为递归函数，满足条件：

- 在第一次调用自己时，必须是（在某种意义上）更接近于结果；
- 必须要有一个终止处理或计算的准则；

（二）、案例：

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

（三）、注意事项：

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

# 九、函数参数传递（值传递与引用传递）：