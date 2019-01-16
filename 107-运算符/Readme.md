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

```go
func main(){
	a := 10
	b := 20
	if (a==b){
		fmt.Println("第一行——a等于b")
	}else {
		fmt.Println("第一行——a不等于b")
	}
	if (a>b){
		fmt.Println("第二行——a大于b")
	}else{
		fmt.Println("第二行——a不大于b")
	}
	if (a<b){
		fmt.Println("第三行——a小于b")
	}else{
		fmt.Println("第三行——a不小于b")
	}
	a = 20
	b = 15
	if (b<=a){
		fmt.Println("第四行——b小于等于a")
	}
	if (a>=b){
		fmt.Println("第五行——a大于等于b")
	}
}
结果：
第一行——a不等于b
第二行——a不大于b
第三行——a小于b
第四行——b小于等于a
第五行——a大于等于b
```

# 3、逻辑运算符：

&&：逻辑and运算符，如果两边操作值都为true，则为true，否则为false；

||：逻辑or运算符，两边只要有一个true，则为true，否则为false；

!：逻辑not运算符，如果条件为true，则逻辑not为false，否则为true；

```go
func main() {
	var a = true
	var b = false
	if (a && b) {
		fmt.Println("第一行为true")
	}else {
		fmt.Println("第一行为false")
	}
	if (a || b) {
		fmt.Println("第二行为true")
	}else {
		fmt.Println("第二行为false")
	}
	if !(a && b) {
		fmt.Println("第三行为true")
	}else {
		fmt.Println("第三行为false")
	}
}
结果：
第一行为false
第二行为true
第三行为true
```

# 4、位运算符

# 5、赋值运算符

# 6、其他运算符

# 7、运算符优先级