# 数据类型转换的格式：

1、T（表达式）：

- 采用数据类型前置加括号的方式进行类型转换，T表示要转换的类型；表达式包括变量、数值、函数返回值等
- 类型转换时要考虑两种类型之间的关系和范围，否则会发生数值截断
- 布尔类型无法与其他类型转换

2、Float与int类型之间的转换：

float转int时会有精度的缺失，例子如下：

```go
func main(){
	chinese := 90
	english := 80.5
	avg := (float64(chinese) + english) / 2
	fmt.Println(avg)
}
结果：85.25
func main(){
	chinese := 90
	english := 80.5
	avg := (chinese + int(english) / 2
	fmt.Println(avg)
}
结果：85
```

3、int转string：

- 相当于byte或rune转string
- 该int数值为ascii码的编号或Unicode字符集的编号，转成string就是将根据字符集把对应编号的汉字字符查找出来
- 当该数值超出Unicode编码范围时，则会输出乱码
- 例如19968转string，就是汉字"一"

例子如下：

```go
func main(){
	a := int('一')
	b := string(19988)
	fmt.Println(a,b)
}
结果：19968 且
```

备注：

- ascii字符集中数字的十进制范围是[30-39]
- ascii字符集中大写字母的十进制范围是[65-90]
- ascii字符集中小写字母的十进制范围是[91-122]
- Unicode字符集中汉字的十六进制范围是[4e00-9fa5]，十进制范围是[19968-40869]

