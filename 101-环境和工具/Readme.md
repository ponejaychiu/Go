Goland官方地址：https://www.jetbrains.com/

Golang下载：https://pan.baidu.com/s/1EMk32mv35z0fGEur_oJpoQ

Golang官方地址：https://golang.org/dl/

# Golang环境（macOS）及常用命令：

## 环境目录：

GOROOT：go的安装目录——默认位置；

GOPATH：go的工作目录——可自定义；

GOBIN：go的可执行文件目录——自定义目录的bin目录；

PATH：将go可执行文件加入PATH中，使GO命令与我们编写的GO应用可以全局调用；

## 常用命令：

go env：查看go的环境变量设置；

go run：运行当前的.go文件;

go build：加上可编译的go源文件可以得到一个可执行文件，-o可指定其他名字;

eg：go build -o myfirstgo hello.go

go get = git clone + go install 从指定源上面下载或者更新指定的代码和依赖，并对他们进行编译和安装

go install：把编译源代码之后的可执行文件安装到指定$GOBIN目录，即可在系统任意位置执行程序

注意：执行go install时的可执行文件的名称要与项目名称一致

* go install(在src/DIR下)编译出的可执行文件以其所在目录名(DIR)命名
* go install将可执行文件安装到与src同级别的bin目录下，bin目录由go install自动创建
* go install将可执行文件依赖的各种package编译后，放在与src同级别的pkg目录下
  

# Goland快捷键：

## Windows版：

### 文件相关快捷键：


CTRL+E，打开最近浏览过的文件。

CTRL+SHIFT+E，打开最近更改的文件。  

CTRL+N，可以快速打开struct结构体。  

CTRL+SHIFT+N，可以快速打开文件。

### 代码格式化：


CTRL+ALT+T，可以把代码包在一个块内，例如if{…}else{…}。

CTRL+ALT+L，格式化代码。

CTRL+空格，代码提示。

CTRL+/，单行注释。

CTRL+SHIFT+/，进行多行注释。

CTRL+B，快速打开光标处的结构体或方法（跳转到定义处）。

CTRL+“+/-”，可以将当前方法进行展开或折叠。 

### 查找和定位：


CTRL+R，替换文本。

CTRL+F，查找文本。  

CTRL+SHIFT+F，进行全局查找。

CTRL+G，快速定位到某行。

### 代码编辑：


ALT+Q，可以看到当前方法的声明。

CTRL+Backspace，按单词进行删除。

SHIFT+ENTER，可以向下插入新行，即使光标在当前行的中间。

CTRL+X，删除当前光标所在行。

CTRL+D，复制当前光标所在行。

ALT+SHIFT+UP/DOWN，可以将光标所在行的代码上下移动。

CTRL+SHIFT+U，可以将选中内容进行大小写转化。

## macOS版：

### 文件相关快捷键：

向上或向下移动当前行  ⇧⌘↑ ⇧⌘↓
复制并粘贴当前选中的语句  ⌘D
删除当前行  ⌘⌫
行注释  ⌘/
块注释  ⌥⌘/
在当前打开的文件中寻找  ⌘F
在当前文件中查找替换  ⌘R
被选中的单词下一次出现的位置  ⌘G
被选中的单词上一次出现的位置  ⇧⌘G
在打开的标签之间导航（即打开的源码文件之间切换） ⇧⌘] ⇧⌘[
前后导航（即前一个动作后一个动作之间切换） ⌘[ ⌘]
展开或收起代码块 ⌘+ ⌘-
操作一个或多个被选中的代码（组合键按相同代码选中几个） ⌃G

### 代码补全：

查看方法或函数建议的参数（光标首先选中函数名或者光标至于函数后的括号中） ⌘P
代码格式化 ⌥+⌘+L（option+command+L）

### 导航：

查看最近访问的文件 ⌘E
导航至项目中已存在的类型 ⌘O
导航至项目中已存在的文件 ⇧⌘O
导航至类型声明或者方法实现 ⌘B
查找任意文件 Double Shift
弹出字面量定义 ⌥Space
代码检查并提供快速修复 ⌥⏎