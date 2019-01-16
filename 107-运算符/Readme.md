# 1、算术运算符：

+加、-减、*乘、/除、%求余、++加加、--减减

```go
package main

import "fmt"

var a = 21.0
var b = 5.0
var c float64

func main() {
	math()
}

func math() {
	c = a + b
	fmt.Printf("第一行的值为%.2f\n", c)
	c = a - b
	fmt.Printf("第二行的值为%.2f\n", c)
	c = a * b
	fmt.Printf("第三行的值为%.2f\n", c)
	c = a / b
	fmt.Printf("第四行的值为%.2f\n", c)
	//c=a%b
	fmt.Printf("第五行的值为%d\n", int(a)%int(b))
	a++
	fmt.Printf("第六行的值为%.2f\n", a)
	a = 21
	a--
	fmt.Printf("第七行的值为%.2f\n", a)
}
结果：
第一行的值为26.00
第二行的值为16.00
第三行的值为105.00
第四行的值为4.20
第五行的值为1
第六行的值为22.00
第七行的值为20.00
```

# 2、关系运算符：

==，!=，'>'，'<'，'>='，'<='

# 3、逻辑运算符：

&&：逻辑and运算符；

||：逻辑or运算符；

!：逻辑not运算符；



# 4、位运算符

# 5、赋值运算符

# 6、其他运算符

# 7、运算符优先级