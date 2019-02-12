[TOC]

# 一、数组的用法：

## （一）、概念：

1. 数组是具有相同类型的一组长度固定的数据序列，这种类型可以是任意的基本数据类型或符合数据类型及自定义数据类型；
2. 数组元素可以根据索引下标（位置）来读取或修改元素数据，索引从0开始，数组下标的取值范围是从0开始，到长度减1；
3. 数组一旦定义长度后，大小不能更改；

## （二）、语法：

1、声明数组：

需要指定**元素类型**和**元素个数**，语法格式如下：

- var 变量名 [数组长度]数据类型
- 数组长度必须是整型且大于0
- 未初始化的数组不是nil，也就是说没有空数组（与切片slice不同），而是一堆0；

2、初始化数组：

- var nums = [5]int{1,2,3,4,5}，其中{}中的元素个数不能大于[]中的数字；
- 如果忽略[]中的数字，不设置数组的大小，Go语言会根据元素的个数来设置数组的大小，可以忽略数组的长度并替换为...，例如：var nums = [...]int{1,2,3,4,5}，效果与上面一致。
- 通过将数组作为参数传递给len()函数，可以获取数组的长度；

## （三）、遍历数组：

1、普通数组：

```go
// 方式1
var arr1 [3]int
var arr2 = [4]int{1, 2, 3, 4}

func main() {
	// 方式2
	arr3 := [5]float64{1, 2, 3, 4, 5.5}
	// 方式3
	arr4 := [...]int{12, 3, 43, 54}
	fmt.Println(arr1)
	fmt.Println(arr2)
	fmt.Println(arr3)
	fmt.Println(arr4)
	// 遍历数组方式1
	for i := 0; i < len(arr4); i++ {
		fmt.Print(arr4[i], "\t")
	}
	fmt.Println()
	// 遍历数组方式2
	for _, value := range arr4 {
		fmt.Print(value, "\t")
	}
	//if arr2 == nil {
	//	fmt.Println("当前的arr是：", nil)
	//}
}
结果：
[0 0 0]
[1 2 3 4]
[1 2 3 4 5.5]
[12 3 43 54]
12	3	43	54	
12	3	43	54
如果判断数组为nil，编译器会报错：cannot convert nil to type [4]int；
```

2、多维数组：

语法格式：

var 变量名 [数组长度1]...[数组长度n] 数据类型，例如：

```go
var arr [10][5][4]int
```

实例：

```go
// 创建多维数组
a = [3][4]int{
    {0,1,2,3}   /*第一行索引为0*/
    {4,5,6,7}   /*第一行索引为1*/
    {8,9,10,11} /*第一行索引为2*/
}
// 访问多维数组
func main() {
	a := [3][4]int{
		{1, 2, 3, 4},
		{5, 6, 7, 8},
		{9, 10, 11, 12},
	}
	fmt.Println(a[2][3])
}
结果：12
a[2][3]访问的是a数组中的第三行的第四列的元素；

// 用for循环的方式访问多维数组
func main() {
	a := [3][4]int{
		{1, 2, 3, 4},
		{5, 6, 7, 8},
		{9, 10, 11, 12},
	}
	for i := 0; i < len(a); i++ {
		for j := 0; j < len(a[0]); j++ {
			fmt.Printf("a[%d][%d]=%d\n", i, j, a[i][j])
		}
	}
}
结果：
a[0][0]=1
a[0][1]=2
a[0][2]=3
a[0][3]=4
a[1][0]=5
a[1][1]=6
a[1][2]=7
a[1][3]=8
a[2][0]=9
a[2][1]=10
a[2][2]=11
a[2][3]=12
```

## （四）、数组是值类型：

1、数组是非引用（值类型）类型，这意味着当他们被分配一个新变量时，将把原始数组的副本分配给新变量，如果对新变量修改，则不会在原始数组中反映；

2、当数组传递给函数作为参数时，它们将通过值传递，而原始数组将保持不变；

3、实例：

```go
func main() {
	a := [...]string{"a", "b", "c", "d"}
	b := a
	b[0] = "x"
	fmt.Println("a:", a)
	fmt.Println("b:", b)
}
结果：
a: [a b c d]
b: [x b c d]
```

# 二、切片的用法：

## （一）、概念：

- 切片是对数组的抽象，由于数组的长度不可改变，在特定场景中这样的集合就不太适用，所以Go中提供了内置类型切片（动态数组）；
- 与数组相比切片的长度是不固定的，可以追加元素，追加时可能是切片容量变大；
- 切片本身没有任何数据，只是对现有数组的引用；
- 切片不需要设定长度，[]不用设置，相对自由；
- 概念上来说slice切片更像一个结构体，这个结构体包含三个元素：1、指针，指向数组中slice指定的开始位置，2、长度，即slice的长度，3、最大长度，即slice开始位置到数组最后位置的长度；

## （二）、切片的语法：

1、声明切片：

声明一个没有指定长度的数组来定义切片

- var 变量名 []数据类型
- 切片不需要指定长度
- 该声明方式，且未初始化的切片为空切片，默认为nil，长度为0；

2、使用make()函数来创建切片：

```go
//标准格式：
var slice []type = make([]type, len)
//短变量格式：
slice := make([]type, len)
//指定容量，其中capacity是可选参数：
slice := make([]type, len, cap)
//例子：
func main() {
	//var num = make([]int, 5)
    num := make([]int, 5)
	fmt.Printf("%T\n", num)
	fmt.Printf("len=%d,cap=%d,slice=%v\n", len(num), cap(num), num)
}
结果：
[]int
len=5,cap=5,slice=[0 0 0 0 0]
```

3、初始化切片：

（1）、直接初始化切片：

slice := []int{1,2,3}

（2）、通过数组截取来初始化切片：

数组：arr := [5]int{1,2,3,4,5}

- slice := arr[:]——切片中包含所有数组元素
- slice := arr[startindex:endindex]——将arr从下标startindex到endindex-1的元素创建为一个新的切片（前闭后开），长度为：endindex-startindex；
- slice := arr[startindex:]——缺省endindex时将表示一直到arr元素的最后一个
- slice := arr[:endindex]——缺省startindex时将表示从arr元素的第一个开始

（3）、通过切片截取来初始化切片：

可以通过设置上限和下限来设置截取切片[lower-bound:upper-bound]

实例：

```go
func main() {
	// 创建切片
	nums := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
	printSlice(nums)

	nums1 := nums[:]
	printSlice(nums1)

	nums2 := nums[1:4]
	printSlice(nums2)

	nums3 := nums[4:]
	printSlice(nums3)

	nums4 := nums[:4]
	printSlice(nums4)
}

func printSlice(s []int) {
	fmt.Printf("len=%d,cap=%d,slice=%v\n", len(s), cap(s), s)
}
结果：
len=10,cap=10,slice=[0 1 2 3 4 5 6 7 8 9]
len=10,cap=10,slice=[0 1 2 3 4 5 6 7 8 9]
len=3,cap=9,slice=[1 2 3]
len=6,cap=6,slice=[4 5 6 7 8 9]
len=4,cap=10,slice=[0 1 2 3]
```

## （三）、len()和cap()函数：

1、切片的长度是切片元素中的数量；

2、切片的容量是从创建切片的索引开始的底层数组中元素的数量；

3、切片是可以索引的，并且可以由len()方法获取长度；切片提供了计算容量的方法cap()，可以测量切片最长可以达到多少，但数组计算len()和cap()的结果是一致的；

4、切片实际上是获取的数组的某一部分，len切片<=cap切片<=len数组；

5、cap()的结果决定了切片截取的注意细节；

实例：

```go
func main() {
	sliceCap()
}

func sliceCap() {
	arr := [...]string{"a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k"}
	fmt.Println("cap(arr)=", cap(arr), arr)
	// 截取数组，生成切片
	s1 := arr[1:5]
	fmt.Printf("%T\n", s1)
	fmt.Println("cap(s1)=", cap(s1), s1)
	s2 := arr[3:9]
	fmt.Println("cap(s2)=", cap(s2), s2)

	// 截取切片，生成切片
	s3 := s1[3:6]
	fmt.Println("截取s1[3:6]后形成s3切片，cap(s3)=", cap(s3), s3)
	s4 := s2[5:8]
	fmt.Println("截取s2[5:8]后形成s4切片，cap(s4)=", cap(s4), s4)
}
结果：
cap(arr)= 11 [a b c d e f g h i j k]
[]string
cap(s1)= 10 [b c d e]
cap(s2)= 8 [d e f g h i]
截取s1[3:6]后形成s3切片，cap(s3)= 7 [e f g]
截取s2[5:8]后形成s4切片，cap(s4)= 3 [i j k]
```

## （四）、切片是引用类型：

1、slice切片没有自己的任何数据，它只是底层数组的一个引用，对slice切片所做的任何修改都将反映在底层数组中；

2、数组是值类型，切片是引用类型；

3、两者区别的示例：

```go
// 上面的例子中增加以下内容：
// 切片是引用类型
s4[0] = "x"
fmt.Println(arr, s1, s2, s3, s4)
结果：[a b c d e f g h x j k] [b c d e] [d e f g h x] [e f g] [x j k]
```

由此可得，所以来源于同一个数组的切片，只要对其中一个切片元素进行修改，就会对底层数组的元素进行修改，那所以相关联的切片元素的值也都会修改，说明切片就是引用类型。

```go
// 数组（值类型）和切片（引用类型）的区别
func main() {
	a := [4]int{1, 2, 3, 4}
	b := []int{100, 200, 300, 400}

	c := a
	d := b

	c[1] = 200
	d[0] = 1

	fmt.Println("a=", a, "c=", c)
	fmt.Println("b=", b, "d=", d)
}
结果：
a= [1 2 3 4] c= [1 200 3 4]
b= [1 200 300 400] d= [1 200 300 400]
```

## （五）、append()和copy()函数：

1、append()函数：

- 往切片中追加新元素；
- 可以向一个slice切片中追加一个或多个元素，也可以追加一个slice切片；
- append函数会改变slice所引用的数组的内容，从而影响到引用了这个数组的其他slice切片；
- 当使用append追加元素到切片时，如果容量不够（也就是（cap-len）==0），Go就会创建一个新的内存地址来存储新元素；

2、copy()函数：

- 复制切片元素；
- 将源切片中的元素复制到目标切片中，返回值是所复制元素的个数；
- copy方法不会建立源切片与目标切片的联系，一个修改不影响另一个；

以上两个方法只适用于slice切片，不能用于数组。

3、append和copy函数实例：

```go
func main() {
	fmt.Println("1、追加元素---------------")
	var numbers []int
    // numbers := make([]int, 0, 10)
	printSlice("numbers:", numbers)
	//append一个元素
	numbers = append(numbers, 0)
	printSlice("numbers:", numbers)
	//append多个元素
	numbers = append(numbers, 1, 2, 3, 4, 5)
	printSlice("numbers:", numbers)
	//append追加元素
	s1 := []int{100, 200, 300, 400, 500}
	numbers = append(numbers, s1...)
	printSlice("numbers:", numbers)
	fmt.Println("2、删除元素---------------")
	// 删除第一个元素
	numbers = numbers[1:]
	printSlice("numbers:", numbers)
	// 删除最后一个元素
	numbers = numbers[:len(numbers)-1]
	printSlice("numbers:", numbers)
	// 删除中间元素
	x := int(len(numbers) / 2)
	numbers = append(numbers[:x], numbers[x+1:]...)
	printSlice("numbers:", numbers)
	fmt.Println("3、拷贝元素---------------")
	// 创建目标切片
	numbers1 := make([]int, len(numbers), cap(numbers)*2)
	// 拷贝元素
	count := copy(numbers1, numbers)
	fmt.Println("拷贝的个数：", count)
	printSlice("numbers1:", numbers1)
	// 验证拷贝的两个切片是否有关系
	numbers[0] = 90
	numbers1[len(numbers1)-1] = 100
	printSlice("numbers:", numbers)
	printSlice("numbers1:", numbers1)
}

func printSlice(name string, x []int) {
	fmt.Print(name, "\t")
	fmt.Printf("地址：%p \t len=%d \t cap=%d \t value=%v \n", x, len(x), cap(x), x)
}
结果：
1、追加元素---------------
numbers:	地址：0x0 	 len=0 	 cap=0 	 value=[] 
numbers:	地址：0xc00004a090 	 len=1 	 cap=1 	 value=[0] 
numbers:	地址：0xc000070030 	 len=6 	 cap=6 	 value=[0 1 2 3 4 5] 
numbers:	地址：0xc00003c060 	 len=11 	 cap=12 	 value=[0 1 2 3 4 5 100 200 300 400 500] 
2、删除元素---------------
numbers:	地址：0xc00003c068 	 len=10 	 cap=11 	 value=[1 2 3 4 5 100 200 300 400 500] 
numbers:	地址：0xc00003c068 	 len=9 	 cap=11 	 value=[1 2 3 4 5 100 200 300 400] 
numbers:	地址：0xc00003c068 	 len=8 	 cap=11 	 value=[1 2 3 4 100 200 300 400] 
3、拷贝元素---------------
拷贝的个数： 8
numbers1:	地址：0xc000088000 	 len=8 	 cap=22 	 value=[1 2 3 4 100 200 300 400] 
numbers:	地址：0xc00003c068 	 len=8 	 cap=11 	 value=[90 2 3 4 100 200 300 400] 
numbers1:	地址：0xc000088000 	 len=8 	 cap=22 	 value=[1 2 3 4 100 200 300 100] 
```

4、append函数的优化：

```go
func main() {
	var sa []string
	//sa := make([]string, 0, 5)
	if sa == nil {
		fmt.Println("sa==nil", len(sa))
	}
	fmt.Println(len(sa))
	for i := 0; i < 5; i++ {
		sa = append(sa, strconv.Itoa(i))
		printSliceData(sa)
	}
}

func printSliceData(sa []string) {
	fmt.Printf("内存地址：%p \t len:%d \t cap:%d \t values:%v \n", sa, len(sa), cap(sa), sa)
}
结果：
sa==nil 0
0
内存地址：0xc00003e1c0 	 len:1 	 cap:1 	 values:[0] 
内存地址：0xc000044460 	 len:2 	 cap:2 	 values:[0 1] 
内存地址：0xc000046100 	 len:3 	 cap:4 	 values:[0 1 2] 
内存地址：0xc000046100 	 len:4 	 cap:4 	 values:[0 1 2 3] 
内存地址：0xc000088000 	 len:5 	 cap:8 	 values:[0 1 2 3 4] 

func main() {
	//var sa []string
	sa := make([]string, 0, 5)
	if sa == nil {
		fmt.Println("sa==nil", len(sa))
	}
	fmt.Println(len(sa))
	for i := 0; i < 5; i++ {
		sa = append(sa, strconv.Itoa(i))
		printSliceData(sa)
	}
}

func printSliceData(sa []string) {
	fmt.Printf("内存地址：%p \t len:%d \t cap:%d \t values:%v \n", sa, len(sa), cap(sa), sa)
}
结果：
0
内存地址：0xc000084000 	 len:1 	 cap:5 	 values:[0] 
内存地址：0xc000084000 	 len:2 	 cap:5 	 values:[0 1] 
内存地址：0xc000084000 	 len:3 	 cap:5 	 values:[0 1 2] 
内存地址：0xc000084000 	 len:4 	 cap:5 	 values:[0 1 2 3] 
内存地址：0xc000084000 	 len:5 	 cap:5 	 values:[0 1 2 3 4] 
```

结论：

创建切片时，推荐使用make的方式：

```go
sa := make([]string, 0, 5)
```

这样可以节省内存地址的重新申请和空间，以提升程序性能。

## （六）、切片冒泡排序及优化：

代码示例：

```go
func main() {
	arr := []int{1, 2, 3, 4, 5}
	//arr := []int{5, 4, 3, 2, 1}
	bubbleSort(arr)
	fmt.Println(arr)
}

func bubbleSort(arr []int) {
	// 外部循环次数
	iCount := 0
	// 内部循环次数
	jCount := 0
	// 双层for循环
	for i := 0; i < len(arr)-1; i++ {
		for j := 0; j < len(arr)-1-i; j++ {
			// 判断两个元素的大小
			if arr[j] > arr[j+1] {
				// 如果成立，则两个元素换位置
				arr[j], arr[j+1] = arr[j+1], arr[j]
			}
			jCount++
		}
		iCount++
	}
	fmt.Println("i循环的次数：", iCount)
	fmt.Println("j循环的次数：", jCount)
}
结果：
i循环的次数： 4
j循环的次数： 10
[1 2 3 4 5]
```

优化代码后：

```go
func main() {
	arr := []int{1, 2, 3, 4, 5}
	//arr := []int{5, 4, 3, 2, 1}
	bubbleSort(arr)
	fmt.Println(arr)
}

func bubbleSort(arr []int) {
	// 外部循环次数
	iCount := 0
	// 内部循环次数
	jCount := 0
	// 双层for循环
	for i := 0; i < len(arr)-1; i++ {
		// 定义一个标记判断本轮是否有两两换位，如果没有则利用break退出循环
		flag := true
		for j := 0; j < len(arr)-1-i; j++ {
			// 判断两个元素的大小
			if arr[j] > arr[j+1] {
				// 如果成立，则两个元素换位置
				arr[j], arr[j+1] = arr[j+1], arr[j]
				// 如果出现两两换位，说明排序还未结束，需要继续执行
				flag = false
			}
			jCount++
		}
		iCount++
		if flag == true {
			break
		}
	}
	fmt.Println("i循环的次数：", iCount)
	fmt.Println("j循环的次数：", jCount)
}
结果：
i循环的次数： 1
j循环的次数： 4
[1 2 3 4 5]
```