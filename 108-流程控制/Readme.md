# 概述：

## 1、Go语言提供了以下几种条件判断、分支语句：

### if：

由一个布尔表达式后紧跟一个或多个语句组成；

语法格式：

```go
if 布尔表达式 {
    /*布尔表达式为true时执行*/
}
```

如果为false则不执行；

例子：

```go 
func main(){
    num := 10
    if num%2==0{
    	fmt.Println("偶数")
	}
}
```

### if...else：

if语句后可以使用可选的else句，else语句中的表达式在布尔表达式为false时执行；

语法格式：

```go
if 布尔表达式 {
    /*布尔表达式为true时执行*/
} else {
    /*布尔表达式为false时执行*/
}
```

例子：

```go
func main() {
	num := 11
	if num%2 == 0 {
		fmt.Println("偶数")
	} else {
		fmt.Println("奇数")
	}
}
```

### if嵌套：

在if或else if语句中嵌套一个或多个if或else if语句；

语法格式：

```go
if 布尔表达式 {
    /*布尔表达式为true时执行*/
} else if 布尔表达式 {
    /*布尔表达式为true时执行*/
} else {
    /*布尔表达式为false时执行*/
}
```

例子1：

```go
func main() {
	score := 78
	if score >= 90 {
		fmt.Println("优秀")
	} else if score >= 80 {
		fmt.Println("优良")
	} else if score >= 70 {
		fmt.Println("中等")
	} else {
		fmt.Println("一般")
	}
}
```

例子2：

```go
func main() {
	score := 58
	if score >= 60 {
		if score >= 70 {
			if score >= 80 {
				if score >= 90 {
					fmt.Println("优秀")
				} else {
					fmt.Println("优良")
				}
			} else {
				fmt.Println("中等")
			}
		} else {
			fmt.Println("一般")
		}
	} else {
		fmt.Println("不及格")
	}
}
```

### switch：

用于执行基于不同条件执行不同动作，每一个case分支都是唯一的，从上至下逐一测试直到匹配为止，匹配项后面也不需要再加break；

可以同时测试多个符合条件的值，使用逗号分隔他们，例如：case var1，var2，var3；

switch后面的表达式可以省略，省略则默认为true；

语法格式：

```go
switch var1{
    case value1:
    case value2:
    default:
}
```

select语句：类似switch语句，但是select会随机执行一个可运行的case，如果没有将会阻塞，直到有可运行的case；

例子1：

```go
func main() {
	score("A")
}

func score(a string) {
	grade := a
	switch grade {
	case "A":
		fmt.Println("优秀")
	case "B":
		fmt.Println("良好")
	case "C":
		fmt.Println("中等")
	case "D":
		fmt.Println("及格")
	default:
		fmt.Println("差")
	}
}
```

例子2:

```go
func main() {
	scores()
}

func scores() {
	score := 88.5
	grade := ""
	switch {
	case score >= 90:
		grade = "A"
	case score >= 80:
		grade = "B"
	case score >= 70:
		grade = "C"
	case score >= 60:
		grade = "D"
	default:
		grade = "E"
	}

	switch grade {
	case "A":
		fmt.Println("优秀")
	case "B":
		fmt.Println("良好")
	case "C":
		fmt.Println("中等")
	case "D":
		fmt.Println("及格")
	default:
		fmt.Println("差")
	}
}
```

例子3:

```go
func main() {
	getDaysbyMonth()
}

func getDaysbyMonth() {
	// 定义局部变量：年、月、日
	year := 2019
	month := 12
	days := 0

	switch month {
	case 1, 3, 5, 7, 8, 10, 12:
		days = 31
	case 4, 6, 9, 11:
		days = 30
	case 2:
		// 判断闰年的2月，每一百年的2月和每四百年的2月
		if (year%4 == 0 && year%100 != 0) || year%400 == 0 {
			days = 29
		} else {
			days = 28
		}
	default:
		days = -1
	}
	fmt.Printf("%d年%d月的天数为：%d天", year, month, days)
}
```

例子4——fallthrough：

由于switch case自带break，所以如果需要执行后面的case，则需要在case分支的最后一行加上fallthrough（中文意思贯穿），强制执行后面的case；

```go
func main() {
	getMath()
}

func getMath() {
	num1, num2, result := 12, 5, 0
	operation := "+"

	switch operation {
	case "+":
		result = num1 + num2
		fallthrough
	case "-":
		result = num1 - num2
	case "*":
		result = num1 * num2
	case "/":
		result = num1 / num2
	case "%":
		result = num1 % num2
	default:
		result = -1
	}
	fmt.Println(result)
}
加上fallthrough后结果为7，不是17。
```

## 2、Go语言提供了以下几种循环处理语句：

### for:

循环语句表示条件满足，重复执行代码块；

for是Go语法中唯一的循环语句，Go中没有while、do...while循环；

for关键字后面不能加小括号；

Go语法中for循环有4种，只有**语法形式一**中使用分号；

#### 语法形式一：

```go
for 初始化语句init;条件表达式condition;结束语句post {
    //循环体代码
}
```

初始化语句init;条件表达式condition;结束语句这三个部分都是可选的，因此可以演化出4种略微不同的写法；

第一种写法：

```go
func main() {
	for i := 0; i < 10; i++ {
		fmt.Printf("%d ", i)
	}
}
结果：0 1 2 3 4 5 6 7 8 9
```

第二种写法：

1、初始化语句init：

初始语句是在第一次循环前执行的语句，一般为赋值表达式，给**控制变量**赋初始值；

如果**控制变量**在此处被声明，其作用域将被局限在这个for循环内，变量仅在循环内可用；

初始语句可以省略不写，但是分号必须写，如下；

```go
func main() {
	i := 0
	for ; i < 10; i++ {
		fmt.Printf("%d ", i)
	}
}
结果：0 1 2 3 4 5 6 7 8 9 
```

第三种写法：

2、条件表达式condition：

条件表达式是控制循环与否的开关；

如果表达式为true，则循环继续，否则结束循环；

条件表达式可以省略不写，但是分号必须写，如下；

省略条件表达式默认形成无线循环，即死循环；

```go 
func main() {
	i := 0
	for ; ; i++ {
		if (i > 10) {
			break
		}
		fmt.Printf("%d ", i)
	}
}
结果：0 1 2 3 4 5 6 7 8 9 
```

第四种写法：

3、结束语句post：

一般为赋值表达式，给**控制变量**递增或者递减；

post语句将在循环的每次成功迭代之后执行；

```go
func main() {
	i := 0
	for ; ; {
		if (i > 10) {
			break
		}
		i++
		fmt.Printf("%d ", i)
	}
}
结果：1 2 3 4 5 6 7 8 9 10 11
```

for循环语句执行过程：

1. 先执行初始化语句，对控制变量进行赋值，初始化语句只执行一次；
2. 其次根据控制变量判断条件表达式的返回值，若返回值为true，满足循环条件，则执行循环体内语句，之后执行post语句，开始下一次循环；
3. 执行post语句之后，将重新计算条件表达式的返回值，若返回值为true，循环将继续执行，否则循环终止；然后执行循环体外的语句；

#### 语法形式二：

for关键字后只有一个表达式；

语法结构：

```go
for 循环条件condition{}
```

类似其他语言中的while循环

例子：

```go
func main() {
	i := 0
	for i < 10 {
		fmt.Printf("%d ",i)
		i++
	}
}
```

#### 语法形式三：

for关键字后无表达式；

语法结构：

```go
for {}
```

效果与其他语言的for (;;) {}一致，此时for执行无限循环；

例子：

```go
func main() {
   i := 0
   for {
      if (i > 10) {
         break
      }
      fmt.Printf("%d ", i)
      i++
   }
}
```

#### 语法形式四：

for循环的range格式，对字符串、数组、slice、map等进行迭代循环

语法结构：

```go
for key,value := range oldMap{
    newMap[key] = values
}
```

例子1：

```go
func main() {
	str := "123abc一丁"
	for i, value := range str {
		i++
		fmt.Printf("第%d个的ascii码为：%d，字符为:%c\n", i, value, value)
	}
}
结果：
第1个的ascii码为：49，字符为:1
第2个的ascii码为：50，字符为:2
第3个的ascii码为：51，字符为:3
第4个的ascii码为：97，字符为:a
第5个的ascii码为：98，字符为:b
第6个的ascii码为：99，字符为:c
第7个的ascii码为：19968，字符为:一
第10个的ascii码为：19969，字符为:丁
```

#### for循环案例：

```go
例子1：
//求1-100的数字之和
func main() {
    summation()
}

func summation() {
	sum := 0
	for i := 1; i <= 100; i++ {
		sum += i
	}
	fmt.Printf("1-100的和为:%d", sum)
}
结果：1-100的和为:5050

例子2：
//求1-100之间3的倍数之和
func main() {
	summation()
}

func summation() {
	sum := 0
	i := 1
	for i <= 100 {
		if i%3 == 0 {
			sum += i
			fmt.Print(i)
			if i < 99 {
				fmt.Print("+")
			} else {
				fmt.Printf("=%d", sum)
			}
		}
		i++
	}
}
结果：3+6+9+12+15+18+21+24+27+30+33+36+39+42+45+48+51+54+57+60+63+66+69+72+75+78+81+84+87+90+93+96+99=1683

例子3：
//32米竹竿，每次截1.5米，最快截几次之后能小于4米？
func main() {
	count()
}

func count() {
	sum := 0
	for i := 32.0; i >= 4; i -= 1.5 {
		sum++
	}
	fmt.Printf("最快截%d次竹竿小于4米", sum)
}
结果：最快截19次竹竿小于4米

例子4：
//遍历切片slice
func main() {
	slice()
}

func slice() {
	arry := []int{100, 200, 300}
	for i, value := range arry {
		i++
		fmt.Printf("切片第%d个值为%v\n", i, value)
	}
}
结果：
切片第1个值为100
切片第2个值为200
切片第3个值为300

例子5：
//遍历集合map
func main() {
	makeMap()
}

func makeMap() {
	countryMap := make(map[int]string)

	countryMap[1] = "北京"
	countryMap[2] = "上海"
	countryMap[3] = "广州"
	countryMap[4] = "深圳"

	for i := range countryMap {
		fmt.Printf("%v的编号是%d\n", countryMap[i], i)
	}
}
结果：
北京的编号是1
上海的编号是2
广州的编号是3
深圳的编号是4
```

### 循环嵌套：

在for循环中嵌套一个或多个for循环；

```go
var lines = 9
func main() {
	printRectangle()
	printRightTriangleLB()
	printRightTriangleLT()
	printRightTriangleRB()
	printRightTriangleRT()
	printTriangle()
	mutilple99()
}

// 1、打印矩形
func printRectangle() {
	fmt.Println("矩形：")
	for i := 1; i <= lines; i++ {
		for j := 1; j <= lines; j++ {
			fmt.Print("❤ ")
		}
		fmt.Println()
	}
}

// 2、打印左下角直角三角形
func printRightTriangleLB() {
	fmt.Println("左下角直角三角形：")
	for i := 1; i <= lines; i++ {
		for j := 1; j <= i; j++ {
			fmt.Print("❤ ")
		}
		fmt.Println()
	}
}

// 3、打印左上角直角三角形
func printRightTriangleLT() {
	fmt.Println("左上角直角三角形：")
	for i := 1; i <= lines; i++ {
		for j := lines; j >= i; j-- {
			fmt.Print("❤ ")
		}
		fmt.Println()
	}
}

// 4、打印右下角直角三角形
func printRightTriangleRB() {
	fmt.Println("右下角直角三角形：")
	for i := 1; i <= lines; i++ {
		// 打印空格
		for m := lines; m > i; m-- {
			fmt.Print("  ")
		}
		// 打印三角形
		for j := 1; j <= i; j++ {
			fmt.Print("❤ ")
		}
		fmt.Println()
	}
}

// 5、打印右上角直角三角形
func printRightTriangleRT() {
	fmt.Println("右上角直角三角形：")
	for i := 1; i <= lines; i++ {
		// 打印空格
		for m := 1; m < i; m++ {
			fmt.Print("  ")
		}
		// 打印三角形
		for j := lines; j >= i; j-- {
			fmt.Print("❤ ")
		}
		fmt.Println()
	}
}

// 6、打印等腰三角形
func printTriangle() {
	fmt.Println("等腰三角形：")
	for i := 1; i <= lines; i++ {
		// 打印空格
		for m := lines; m > i; m-- {
			fmt.Print("  ")
		}
		// 打印三角形
		for j := 1; j <= 2*i-1; j++ {
			fmt.Print("❤ ")
		}
		fmt.Println()
	}
}

// 9、打印99乘法表
func mutilple99() {
	fmt.Println("99乘法表:")
	// 控制99乘法表的第二个数字
	for i := 1; i <= lines; i++ {
		// 控制99乘法表的第一个数字
		for j := 1; j <= i; j++ {
			fmt.Printf("%d*%d=%d ", j, i, i*j)
		}
		fmt.Println()
	}
}
```

## 3、Go语言提供了以下几种循环控制语句：

### break：

用于中断当前for循环或跳出switch语句

### continue：

跳出当前循环的剩余语句，然后执行下轮循环；

区别如下：

```go
func main() {
	breakContinue()
}

// break与continue的区别
func breakContinue() {
	fmt.Println("一、break:")
	for i := 0; i <= 10; i++ {
		if i == 5 {
			break
		}
		fmt.Println(i)
	}

	fmt.Println("二、continue:")
	for i := 0; i <= 10; i++ {
		if i == 5 {
			fmt.Println("没有5了")
			continue
		}
		fmt.Println(i)
	}
}
结果：
一、break:
0
1
2
3
4
二、continue:
0
1
2
3
4
没有5了
6
7
8
9
10
```

continue实例：

```go
//输出1-50之间所有不包含4的数字（个位和十位）-continue实现
func eludefour() {
	fmt.Println("输出1-50之间所有不包含4的数字:")
	num := 0
	for num < 50 {
		num++
		if num%10 == 4 || num/10 == 4 {
			continue
		}
		fmt.Printf("%d\t", num)
	}
}
结果：1	2	3	5	6	7	8	9	10	11	12	13	15	16	17	18	19	20	21	22	23	25	26	27	28	29	30	31	32	33	35	36	37	38	39	50	
```

### goto：

将控制转移到被标记的语句，goto语句可以无条件的转移到程序指定的行；

通常与条件语句配合使用，实现条件转移，构成循环，跳出循环等功能，但结构化程序设计中一般不主张使用goto，以免造成流程的混乱，使理解和调试程序产生困难；

语法格式：

LABEL：statement

goto LABEL

goto实例：

```go
例子一：
//输出1-50之间所有不包含4的数字（个位和十位）-goto实现
func gotofour() {
	fmt.Println("输出1-50之间所有不包含4的数字:")
	num := 0
LOOP:
	for num < 50 {
		num++
		if num%10 == 4 || num/10 == 4 {
			goto LOOP
		}
		fmt.Printf("%d\t", num)
	}
}
结果：1	2	3	5	6	7	8	9	10	11	12	13	15	16	17	18	19	20	21	22	23	25	26	27	28	29	30	31	32	33	35	36	37	38	39	50	

例子二：
//求1-100之间的所有素数-goto实现
func gotoss() {
	fmt.Println("输出1-100之间所有的素数:")
	num := 0
LOOP:
	for num < 100 {
		num++
		for i := 2; i < num; i++ {
			if num%i == 0 {
				goto LOOP
			}
		}
		fmt.Printf("%d\t", num)
	}
}
结果：1	2	3	5	7	11	13	17	19	23	29	31	37	41	43	47	53	59	61	67	71	73	79	83	89	97
```

