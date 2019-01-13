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

## 1、package main：

定义包名，必须在原文件中非注释的第一行指明这个文件属于哪个包，如题；package main表示一个可独立执行的程序，每一个go应用程序都包含一个名为main的包。

## 2、import "fmt":

告诉编译器此程序需要使用fmt包，fmt是实现了格式化I/O（输入/输出）的函数。

## 3、func main ():

程序入口，main函数是每一个可执行程序所必须包含的，一般来说都是程序启动后第一个执行的函数，如果有init()函数则会先执行init()函数。

## 4、`/*...*/`:

注释，程序执行时忽略；//表示单行注释，`/*...*/`表示多行注释，不可以嵌套使用；

## 5、fmt.Println(...):

将字符串输出到控制台，并自动换行。使用`fmt.Print("Hello World!!!\n")`可以得到相同的结果。

# 三、Go语言编码规范：






