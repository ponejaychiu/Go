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

用于执行基于不同条件执行不同动作；

语法格式：

```go
switch var1{
    case value1:
    case value2:
    default:
}
```

select语句：类似switch语句，但是select会随机执行一个可运行的case，如果没有将会阻塞，直到有可运行的case；

## 2、Go语言提供了以下几种循环语句：

- for循环：重复执行代码块
- 循环嵌套：在for循环中嵌套一个或多个for循环；

## 3、Go语言提供了以下几种循环控制语句：

- break语句：用于中断当前for循环或跳出switch语句
- continue语句：跳出当前循环的剩余语句，然后执行下轮循环；
- goto语句：将控制转移到被标记的语句；

