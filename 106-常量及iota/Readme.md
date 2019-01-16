# 常量：

1、常量的数据类型只能是布尔型、数字型和字符串；

2、格式：const 常量名 [类型] = 值

3、常量组格式：const(k=v)，常量组中如果不定义类型和初始值，则与上一行非空常量的值相同；

```go
const (
    a = 10
    b
    c
)
打印a、b、c，输出结果：10 10 10
```

# 特殊常量iota：

1、iota是一个系统定义的可以被编译器修改的常量值，iota只能用在常量赋值中

；

2、可以理解成常量组中常量的计数器，只要有一个常量，iota就会加1；

```go
const (
    a = iota
    b = iota
    c = iota
)
fmt.Println(a,b,c)
结果：0，1，2
const (
    a = "jay"
    b = iota
    c
)
fmt.Println(a,b,c)
结果：jay，1，2
const (
    d = 1<<iota //1*2^iota,iota=0，所以1*2^0=1
    e = 3<<iota //3*2^iota,iota=1，所以3*2^1=6
    f //iota=2，所以3*2^2=12
    g //iota=3，所以3*2^3=24
)
fmt.Println(d,e,f,g)
结果：1，6，12，24
const (
    l = '一'
    m
    n = iota
    o
)
fmt.Println(l,m,n,o)
结果：19968，19968，2，3
```

