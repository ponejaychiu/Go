[TOC]

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

## 2、Go语言提供了以下几种循环语句：

- for循环：重复执行代码块
- 循环嵌套：在for循环中嵌套一个或多个for循环；

## 3、Go语言提供了以下几种循环控制语句：

- break语句：用于中断当前for循环或跳出switch语句
- continue语句：跳出当前循环的剩余语句，然后执行下轮循环；
- goto语句：将控制转移到被标记的语句；

