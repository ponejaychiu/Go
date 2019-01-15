# 一、代码示例：

```go
package main
import "fmt"
func main() {
    /* 这是一个简单的Hello World程序 */
    fmt.Println("Hello World!!!")
}
```

# 二、程序解释：

1、package main：

定义包名，必须在源文件中非注释的第一行指明这个文件属于哪个包，如题；package main表示一个可独立执行的程序，每一个go应用程序都包含一个名为main的包。

2、import "fmt":

告诉编译器此程序需要使用fmt包，fmt是实现了格式化I/O（输入/输出）的函数。

3、func main ():

程序入口，main函数是每一个可执行程序所必须包含的，一般来说都是程序启动后第一个执行的函数，如果有init()函数则会先执行init()函数。

4、`/*...*/`:

注释，程序执行时忽略；//表示单行注释，`/*...*/`表示多行注释，不可以嵌套使用；

5、fmt.Println(...):

将字符串输出到控制台，并自动换行。使用`fmt.Print("Hello World!!!\n")`可以得到相同的结果。

# 三、Go语言编码规范：

1、标识符：

- 第一个字符必须是下划线或字母而不能是数字，不能使用特殊符号；

- Go区分大小写，因此func和Func是两个不同的标识符；

2、可见性规则：

Go语言中，使用大小写来决定标识符（变量、常量、类型、接口、结构或函数）是否可以被外部包所调用，大写开头的可以被外部包调用，小写开头的则只能在包内调用。

# 四、Go程序结构组成：

//当前程序的包名

```go
package main
```

//导入其他的包

```go
import "fmt"
```

//常量定义

```go
const PI = 3.14
```

//全局变量的声明和赋值

```go
var name = "golang"
```

//一般类型声明

```go
type newType int
```

//结构的声明

```go
type golang struct{}
```

//接口的声明

```go
type golang interface{}
```

//由main函数作为程序入口点启动

```go
func main() {
    /* 这是一个简单的Hello World程序 */
    fmt.Println("Hello World!!!")
}
```


