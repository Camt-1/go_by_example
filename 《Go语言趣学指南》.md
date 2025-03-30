## 1_GOGOGOGO
### 唯一允许的大括号放置风格
```go
func main() {
	//..
}
```
# 一 命令式编程
## 2_被美化的计算器
### 运算符
\+ \- * / %
### 格式化输出
```go
fmt.Println("hello", 1, "hello1", 2, "hello2")
fmt.Printf("hello1 %v hello2 %v \n", 1, 2) //接受第一个参数是文本,第二个是表达式 fmt.Printf("hello1 %v %v %v \n", 1, "hello2", 2)
//%4v就是向左填充四个空格, %-4v就是向右填充四个空格
fmt.Printf("hello1 $%4v \nhello2 %-4v$ \n", 1, 2)
```
### 常量与变量
```go
const A int = 111
var B float = 22.2
C := 333 //短声明
var D, E, F = 1, 2, 3 //一次声明多个变量
```
## 3_循环和分支
### 真或假
`true` 是唯一真值, `false`是唯一假值
### 比较
比较两个值是得出true和false的另一种方法
```go
var age = 41
var minor = age < 18 //minor为假
```
### 循环
1. 用If实现分支判断
    
2. 用switch实现分支判断
    
    case到了之后只会执行那一个case的语句,需要执行后面的语句需要使用`fallthrough`
    
	即用户需要显示的使`fallthrough`关键字才会引发下降
    
3. 用for循环实现重复执行
    
    可以使用break跳出循环
## 4_变量作用域
Go的作用域通常会随着大括号{ }的出现而开启和结束

声明变量的位置决定了变量所处的作用域

包作用域在声明时不允许使用简短声明

# 二 类型
## 6_实数
### 声明浮点类型变量
`int != 11.11`所有贷小数点的数字在默认情况下都会被设置为float64
如果使用一个整数去初始化变量,只有在显示地指定浮点类型地情况下,才会将其声明为浮点类型变量 在Go中,浮点类型默认为float64,但还提供另一种类型为float32,又称为单精度浮点型
### 打印浮点类型
在使用Print或者Println处理浮点类型地时候,函数默认将打印出尽可能多的小数位数
Printf的格式化变量`%f`可以指定被打印小数的位数,小数点前表示宽度,小数点后表示精度
```go
third := 1.0 / 3
fmt.Print(third) //打印出0.3333333333333333
fmt.Printf("%f\n", third) //打印出0.333333
fmt.Printf("%.3f\n", third) //打印出0.333
fmt.Printf("%4.2f\n", third) //打印出0.33
```
### 浮点精确性
计算机虽然可以精确地表示出1/3,但是使用这个数字和其他数字进行计算的时候却会引发舍入错误
比较浮点数的时候,可以计算出它们之间的差,然后通过判断这个差的绝对值是否足够小来判断是否相等
```go
num := 0.1
num += 0.2
fmt.Println(num == 0.3) //打印出"fales"
fmt.Println(math.Abs(num - 0.3) < 0.0001) //打印出"true"
// num变量的值是0.3000...00000000004而不是0.30
```
## 7_整数
### 声明整数类型变量
| 类型     | 取值范围                                                  | 内存占用情况  |
| :----- | :---------------------------------------------------- | :------ |
| int8   | -128到127                                              | 8位 1字节  |
| uint8  | 0到255                                                 | 8位 1字节  |
| int16  | -32 768到32 767                                        | 16位 2字节 |
| uint16 | 0到65 535                                              | 16位 2字节 |
| int32  | -2 147 483 648到2 147 483 647                          | 32位 4字节 |
| uint32 | 0到4 294 967 295                                       | 32位 4字节 |
| int64  | -9 223 372 036 854 775 808到-9 223 372 036 854 775 807 | 位 8字节   |
| uint64 | 0到18 446 744 073 709 551 615                          | 64位 8字节 |
Go在进行类型判断时,总会选择int作为整数值的类型
int和uint会根据硬件选择最合适的位长,在32位架构上是32位值,在64位架构上是64位值
```
year : = 2000
fmt.Printf("Type %T for %v\n", year, year)
//Printf提供格式化函数 %T 去输出指定变量的类型
``` 
虽然int和uint是最常用地整数类型,但在某些情况下也会用到更长或更短的类型
### 为8位颜色使用uint8类型
`var red, green, blue, uint8 = 0, 141, 213`
### 整数回绕
uint8类型的取值范围为0~255,而针对该类型的增量操作超过255时将回绕至0
除非回绕正是需要的结果,否则就应该谨慎地选择合适的整数类型以避免回绕
## 8_大数
- 当原生类型无法存储非常大的值时,可以使用big包来代替它们
- 无类型常量可以存储非常大的值,并且所有数值型字面量都是无类型常量
- 无类型常量在被用作函数参数的时候,必须之后为由类型变量
### big包
big包提供三种类型:
1. 存储大整数的big.Int,它可以存储超过18艾的数字
2. 存储任意精度浮点数的big.Float
3. 存储诸如1/3的分数的big.Rat
```go
package main
import (
	"fmt"
	"math/big"
)
func main() {
	lightSpeed := big.NewInt(299792)
	swcondsPerDay := big.NweInt(86400)
	distance := new(big.Int)
	distance.SetString("24000000000000000000", 10)
	//打印出"Andromeda Galaxy is 24000000000000000000 km away."
	fmt.Println("Andromeda Galaxy is", distance, "km away.")
	seconds := new(big.Int)
	seconds := Div(diatance, lightSpeed)
	days := new(big.Int)
	days.Div(seconds, secondsPerDay)
	//打印出"That is 926568346 days of travel at light speed."
	fmt.Println("That is", days, "days of travel at light speed.")
}
```
*像big.Int这样的大类型虽然能够精确表示任意大小的数值,但代价是使用起来比起int, float64等原生类型要麻烦,并且运行速度h也会相对较慢*
### 大小非同寻常的常量
常量声明可以跟变量声明一样带有类型,但是常量也无法用uint64类型存储像24艾这样的巨大值:```
```go
const diatance uint64 = 24000000000000000000
//尝试定义一个值为24000000000000000000的常量将导致uint64类型溢出
```
但是如果声明的是一个不带类型的常量时,Go语言不会为常量推断类型,而是直接将其标识为无类型(untyped),例如:
```go
const distance = 24000000000000000000//这样就不会报错
```
除此之外,程序里的每个字面量(lieral value)也都是常量.这意味着那些大小非同寻常的数值可以被直接使用,因为Go的编译器就是用Go语言编写的,并且在底层实现中,无类型的数值常量将由big包提供支持,所以程序能够直接对超过18艾的数值常量执行所有常规运算:
```go
fmt.Println("Andromeda Galaxy is", 24000000000000000000/299792/86400, "light days away.") //打印出"Andromeda Galaxy is 926568346 ight days away."
```
变量也可以使用常量作为值,只要变量的大小能够容纳常量即可.
```go
km := distance //常量24000000000000000000无法存储
days := distance / lightSpeed / sencondsPerDay //926568346能够存储在int变量中
```
虽然Go编译器使用big包处理无类型常量,但是常量与big.Int值时无法互换的
```go
//虽然可以打印出值24艾的big.Int变量,但尝试直接打印24艾的distance常量将引发溢出错误
fmt.Println("Andromeda Galaxy is", distance, "km away.") //常量24000000000000000000将引发int类型溢出
```
## 9_多语言文本
### 声明字符串变量
Go语言会把" "包围的字面值推断为string类型,字符串字面量可以包含转义字符
使用\`\`包围的字符串是原始字符串字面量
### 字符 代码点 符文和字节
Unicode Consortium把名为代码点的一系列数值赋值给了上百万个独一无二的字符

Go语言提供了rune(符文)类型用于表示单个统一代码点,该类型是int32类型的别名
除此之外,Go还提供了uint8类型的别名byte,这种类型既可以表示二进制数据,又可以表示由美国信息交换标准代码(ASCII)定义的英文字符
>- 类型别名
>类型别名就是同一类型的不同名字,所以rune和int32是可以互换的.
>`type byte = uint8``type rune = int32`
>byte和rune跟它们为之创建别名的整数类型具有完全相同的表现

虽然rune类型代表的是一个字符,但它实际存储的依然是数字值.
```go
var grade rune = 65
grade := 'A'
var grade := 'A'
var grade rune = 'A'
fmt.Printf("%v, grade")//打印出"65"
fmt.Printf("%c, grade")//打印出"A"
```
### 拉弦
虽然可以将不同字符串赋值给同一个变量,但是无法对字符串本身进行修改:
```go
peace := "shalom"
peace = "salam"
```
与此同时,虽然可以独立访问字符串中的单个字符,但是不能修改这些字符
```go
message := "shalom"
c := message[5]
fmt.Printf("%C\n", c) //打印出"m"

message[5] = 'd'//无法赋值给message[5]
```
### 使用凯撒加密法处理字符
回转13(rotate13,简称ROT13)时凯撒密码的一个变体,它给字符添加的量是13,并且ROT13的加密和解密可以通过同一个方法实现.
```go
message := "uv vagreangvvbany fcnpr fgngvba"

for i := 0; i < len(message); i++ {
	c := message[i]
	//只解密英文字母,至于空格和表达符号保持不变
	if c > 'a' && c <= 'z' {
		c = c + 13
		if c > 'z' {
			c = c - 26
		}
	}
	fmt.Printf("%c", c) //打印出"hi international space station"
}
```
这段代码的ROT13实现只能处理ASCII字符
### 将字符串解码为符文
Go中使用UTF-8编码为同一码代码点编码,UTF-8是一种高效的可变长度的编码方式,它可以用8个, 16个, 或32个二进制位为单个代码点编码.在可变长度编码的基础上,UTF-8沿用了ASCII字符的编码,从而使得ASCII字符可以直接转换为相应的UTF-8编码字符.
为了能够支持多种语言,程序需要做的就是在处理字符之前先将它们解码为rune类型. utf8包提供了两个函数,其中RuneCountInString函数能够以符文而不是字节为单位返回字符串的长度, 而DecodeRuneInString函数则能够解码字符串的首个字符并返回解码后的符文占用的字节数量.
```go
package main
import {
	"fmt"
	"unicode/utf8"
}
func main() {
	question := "¿Cómo estás?"
	fmt.Println(len(question), "bytes") //打印出"15 bytes"
	fmt.Println(utf8.RuneCountInString(question), "runes") //打印出"12 runes"

	c, size := utf8.DecodeRuneInString(question)
	fmt.Println("First rune: %c %v bytes", c, size) //打印出"First runes: ¿ 2 bytes"

	//在每次迭代中,变量i都会被赋值为字符串的当前索引,而变量c则会被赋值为该索引上的代码点(rune)
	for i, c := range question {
		fmt.Printf("$v %c\n", i, c)	
	}
	//如果不需要在迭代时获取索引,只需使用_来省略即可
	for _, c := range question {
		fmt Printf("%c", c) //打印出"¿Cómoestás?"	
	}
}
```
## 10_类型转换
### 数字类型转换
Go语言不允许混合使用不同类型的变量,但是可以类型转换
```go
age := 41
marsAge := float64(age)

marsDays := 687.0
earthDays := 365.2425
marsAge = marsAge \* earthDays / marsDays
fmt.Println(marsAge)//输出结果为21.79758...
```
除了整数和浮点数之外,有符号数整数和无符号整数,以及各种不同长度的类型之间都需要进行类型转换

### 转换布尔值
Print系列的函数可以将布尔值true和false打印成相应的文本
```go
launch := false

launchText := fmt.Sprintf("%v", launch)
fmt.Println(launchText)//打印出"false"

var yerNo string
if launch {
	yesNo = "yes"
} else {
	yesNo = "no"
}
fmt.Println(yesNo)//打印出"no"
```
# 三 构建块
## 12_函数
- 声明函数需要提供函数名, 形参列表, 和返回值列表
- 名称中首字母大写的函数和类型将被导出并为其他包所用
- 函数声明中的每一个形参和返回值都由名字后跟类型组成,如果多个形参或者返回值具有相同的类型,那么类型只需要给出一次即可.此外,函数声明中的返回值也可以省略名字,而只给出类型
- 除调用同一包中声明的函数之外,调用外部包声明的函数都需要使用相应的包名作为前缀
- 调用函数时需要根据其接受的形参给予相应的实参,至于函数的执行结果则会通过关键字return返回给调用者
## 13_方法
```go
type kelvin float64
var k kelvin = 294.0
var c celius

//kelvinToCelsius函数
func kelvinToCelsius(k kelvin) celsius {
    return celsius(k - 273. 15)
}
//kelvin类型的celsius方法
func (k kelvin) celsius() celsius {
    return celsius(k - 273.15)
}

c = kelvinToCelsius(k) //调用kelvinToCelsius函数
c = k.celsius() //调用celsius方法
```
- 使用自定义类型能够让代码变得更易读且更可靠
- 方法就像是跟特定类型相关联的函数,其中被关联的类型将通过方法名前面的接收者来指定.方法和函数一样,都可以接受多个形参并返回多个值,但是一个方法必须并且只能有一个接收者.在方法体内部接收者的行为就跟其他形参一样
- 调用方法需要用到点记号,排在最前面的是适当类型的变量,之后是一个点号和方法名,最后才是实参
## 14_一等函数
在go语言里面,函数是一等值,它可以用在整数, 字符串, 或其他类型能够应用的所有地方: 可以将函数赋值给变量, 可以将函数传递给函数, 甚至可以编写创建并返回函数的函数
### 将函数赋值给变量
```go
type kelvin float64

func fakeSensor() kelvin {
	return kelvin (rand.Intn(151) + 150)
}
func realSensor() kelvin {
	retutn 0
}

func main() {
	sensor := fakeSensor
	fmt.Println(sensor())
	sensor := realSensor
	fmt.Println(senser())
}
```
### 将函数传递给其他函数
```go
type kelvin float64
//measureTemperature接受另一个函数作为它的第二个形参
func measureTemprature(samples int, sensor func() kelvin) {
	for i := 0; i < samples; i++ {
		k := sensor()
		fmt.Printf("%v, k)
		time.Sleep(time.Second)
	}
}
```
### 声明函数类型
为函数声明新的类型有助于精简和明确调用者的代码.例如,使用kelvin类型而不是底层表示来代表温度单位,同样的方法也可以应用于被传递的函数 :`type sensor func() kelvin`
跟"不接受任何形参并且只返回一个kelvin值的函数"这一模糊的概念相比,现在代码可以通过sensor类型来明确地声明一个传感器函数.通过sensor类型还能够有效地精简代码,使函数声明: 
`func measureTemperature(samples int, a func() kelvin)`
能够改写为
`func measureTemperature(samples int, s sensor)`
### 闭包和匿名函数
匿名函数也就是没有名字的函数,在Go中也被称为函数字面量.跟普通函数不一样的是,因为函数字面量需要保留外部作用域的变量引用,所以函数字面量都是闭包的.
- 将匿名函数赋值给变量
```go
var f = func() {
	fmt.Println("Dress up for the masquerade.")
}
func main() {
	f()
	
	y := func(message string) {
		fmt.Println(message)
	}
	y("Go to the party.")
}
```
- 闭包特性的匿名函数
术语*闭包*是由于匿名函数*封闭并包围*作用域中的变量而得名的
以下匿名函数利用了闭包特性,它引用了calibrate函数用作形参的s变量和offset变量.尽管calibrate函数已经返回了,但是被闭包捕获的变量将继续存在,因此调用sensor仍然能够访问这两个变量.
```go
type kelvin float64
//sensor函数类型
type sensor func() kelvin

func realSensor() kelvin {
	return 0
}
func calibrate(s sensor, offset kelvin) sensor {
	//声明并返回匿名函数
	return func() kelvin {
		return s() + offset
	}
}
func main() {
	sensor := calibrate(realSensor, 5) 
	fmt.Println(sensor()) //打印出5
}
```
闭包保留的是周围变量的**引用**而不是副本值,所以修改被闭包捕获的变量可能会导致匿名函数的结果发生变化
```go
var k kelvin = 294.0
sensor := func() kelvin {
	return k
}
fmt.Println(sensor()) //打印出294
k++
fmt.Println(sensor()) //打印出295
```
# 四 收集器
## 16_劳苦功高的数组
### 声明数组并访问其元素
```go
var planets [8]string

planets[0] = "Mercury"
planets[1] = "Venus"
planets[2] = "Earth"

earth := planets[2]
fmt.Println(earth) //打印出"Earth"
``` 
### 小心越界
Go编译器在检测到对越界元素的访问时会报错:
```go
var planets [8]string
planets[8] = "Pluto"
pluto := planets[8]
```
如果Go编译器在编译时未能发现越界错误,那么程序将在运行时出现惊恐(panic):
```go
var planets [8]string
i := 8
planets[i] = "Pluto"
pluto := planets[i]
```
### 使用复合字面量初始化数组
**复合字面量**是一种使用给定值对任意符合类型实施初始化的紧凑语法.
与先声明一个数组,然后再一个接一个地为它的元素赋值相比,Go语言的复合字面量语法允许我们再单个步骤里面完成声明数组和初始化数组这两项工作.
*为了方便,还可以在函数字面量里面使用省略号...而不是具体的数组作为数组长度*
```go
dwarfs := [5]string{"Ceres", "Pluto", "Haumea", "Makemake", "Eris"}
```
### 迭代数组
```go
dwarfs := [5]string{"Ceres", "Pluto", "Haumea", "Makemake", "Eris"}

for i := 0; i < len(dwarfs); i++ {
	dwarf := dwarfs[i]
	fmt.Prinfln(i, dwarf)
}
for i, dwarf := range dwarfs {
	fmt.Println(i, dwarf)
}
```
使用关键字range可以取得数组中每个元素对应的索引和值
### 数组被复制
无论是将数组赋值给新的变量还是将它传递给函数,都会产生一个完整的数组副本
因为数组也是一种值,而函数通过传递值接受参数,所以数组作为形参的函数将非常低效,在函数内部操作的是数组的副本,所以对数组的修改,不会影响函数外的数组.
*数组的长度也是数组类型的一部分,虽然\[8\]string和\[5\]string都属于字符串收集器,但它们实际上是不同类型*
### 由数组组成的数组
```go
var board [8][8]string

board[0][0] = "r"
board[0][7] = "r"

for column := range board[i] {
	board[1][p] = "P"
}
```
## 17_切片: 指向数组的窗口
### 切分数组
通过切分数组创建切片需要用到的**半开区间**,`planets[0:4]`从索引0的行星开始一直持续到索引4的行星为止(但不包括索引为4的行星本身)
在切分数组创建切片的时候,省略半开区间中的起始索引表示使用数组的起始位置作为索引,而省略半开区间的结束索引则表示使用数组的长度作为索引.
### 切片的复合字面量
如果需要一个跟底层数组具有同样元素的切片,其中一种方法就是声明数组然后使用\[ : \]对其进行切分;还可以选择直接声明切片,声明切片不需要在\[ \]内提供任何值
```go
dwarfArray := [...]string("Ceres", "Pluto", "Haumea", "Makemake", "Eris")
dwarfSlice := dwarfArray[ : ]

dwarfs := []string("Ceres", "Pluto", "Haumea", "Makemake", "Eris")
```
直接声明切片仍然会有相应的底层数组,go首先会在内部生成一个包含5个元素的数组,然后再创建一个能够看到数组所有元素的切片.
### 切片的威力
```go
//这个实参是一个切片而非数组
func hyperspace(worlds []string) {
	for i := range worlds {
		workds[i] = strings.TrimSpace(worlds[i])
	}
}
func main() {
	planets := []string("Venus", "Earth", "Mars")
	hyperspace(planets)
	fmt.Println(strings.Join(planets, "")) //打印出"VenusEarthMars"
}
```
worlds和planets都是切片,并且前者还是后者的副本,但是它们都指向相同的数组
如果hyperspace函数想要修改的是worlds切片的指向,无论是指向开头还是结尾,这些修改都不会对planets切片产生任何影响.但由于hyperspace函数能够访问worlds指向的底层数组并修改其包含的元素,因此这些修改将见诸同一数组的其他切片(视图).
### 带有方法的切片
在Go语言中,声明底层为切片或数组的类型,并为其绑定相应的方法,跟其它语言的类相比,Go语言在类型之上声明方法的能力更通用.
例如,标准库的sort包声明了一种StringSlice类型: `type StringSlice []string`并且该类型还有关联的Sort方法: `type (p StringSlice) Sort()`
```go
func main()  {
	planets := []string{
		"Mercury", "Venus", "Earth", "Mars",
		"Jupiter", "Saturn", "Uranus", "Neptune"
	}
	sort.StringSlince(planets).Sort() //按照字母顺序对行星进行排序
}
```
为了进一步简化上述操作,sort包提供了Strings辅助函数,它会自动执行所需的类型转换并调用
Sort方法: `sort.Strings(planets)`
## 18_更大的切片
### 长度和容量
切片中可见元素的数量决定了切片的长度.如果切片底层的数组比切片大,就说该切片还用容量可供增长
```go
package main
import "fmt"
//dump函数会打印出切片的长度, 容量, 和内容
func dump(label string, slice []string) {
	fmt.Printf("%v: length %v, capacity %v %v\n", label, len(slice), cap(slice), slice)
}

func main() {
	dwarfs := []string{"Ceres", "Pluto", "Haumea", "Makemake", "Eris"}
	dump("dwarfs", dwarfs) //打印出"dwarf:length5,capacity5[CeresPlutoHaumeaMakemakeEris]
	dump("dwarfs[1:2]", dwarfs[1:2])//打印出"dwarfs[1:2]:length1,capacity4[Pluto]"
}
```
### append函数
通过内置的append函数,可以将更多元素添加到dwarfs切片里面
```go
dwarfs := []string{"Ceres", "Pluto", "Haumea", "Makemake", "Eris"}
dwarfs = append(dwarfs, "Orcus")
dwarfs = append(dwarfs, "Salacia", "Quaoar", "Sedna")
fmt.Println(dwarfs) //打印出[CeresPlutoHaumeaMakemakeErisOrcusSalaciaQuaoarSedna]
```
当支撑切片的底层数组没有足够的空间(容量)执行追加操作时,append函数切片包含的元素复制到新分配的数组里面.新数组的容量时原数组的两倍,其中额外分配的容量将为后续可能发生的append操作提供空间
### 三索引切分操作
```go
planets := []string{
	"Mercury", "Venus", "Earth", "Mars",
	"Jupiter", "Saturn", "Uranus", "Neptune",
}
terrestrial := planets[0:4:4] //长度为4,容量为4
worlds := append(terrestrial, "Ceres")
fmt.Println(planets) //打印出"[MercuryVenusEarthMarsJupiterSaturnUranusNeptune]"

//相反,如果在执行切片操作时没有指定第3个索引,那么terrestrial的容量将为8,并且也不会应为追加Ceres而分配新的数组,而是会覆盖原数组中的Jupiter:
terrestrial := planets[0:4]
worlds = appends(terrestrial, "Ceres")
fmt.Println(planets) //打印出"[MercuryVenusEarthMarsCeresSaturnUranusNeptune]"
```
*除非特意想要覆盖底层数组的元素,否则使用三索引切片操作设置将会更安全*
### 使用meke函数对切片实行预分配
通过内置的make函数对切片实行预分配策略,可以尽量避免额外的内存分配和数组复制操作
```go
//make函数分别指定了0和10作为dwarfs切片的长度和容量,从而使得切片可以追加最多10个元素
dwarfs := make([]string, 0, 10)
dwarfs = append(dwarfs, "Ceres", "Pluto", "Haumea", "Makemake", "Eris")
```
### 声明可变参数函数
为了声明像Printf和append这样能够接受可变数量的实参的可变参数函数,需要在该函数的最后一个形参前面加上省略号
```go
func terraform(prefix string, world ...string) []string {
	newWorlds := make([]string, len(worlds)) //创建新的切片而不是直接修改worlds

	for i := range worlds {
		newWorlds[i] = prefix + " " + worlds[i]		
	}
	return newWorlds
}

//worlds形参时一个字符串切片,它包含传递给terraform函数的零个或多个实参
twoWorlds := terraform("New", "Venus", "Mars")
fmt.Println(twoWorlds) //打印出[NewVenusNewMars]

//通过省略号可以展开切片中的多个元素,并将它们用作传递给函数的多个实参
planets := []string{"Venus", "Mars", "Jupiter"}
newPlanets := terrform("New", planets...)
fmt.Println(newPlanets) //打印出[NewVenusNewMarsNewJupiter]
```
如果terrform函数直接修改或改变(mutate) worlds形参中的元素,那么这些修改将见诸planets切片,但是terrafrom函数通过使用newWorlds切片避免了这一点
## 19_无所不能的映射
### 声明映射
与数组和切片只能使用序列整数作为键的做法不一样,映射的键几乎可以是任何类型.在使用Go语言的映射时,必须为映射的键和值指定类型.
```go
//通过复合字面值为映射提供键值对
trmperature := map[string]int {
	"Earth": 15,
	"Mars": -65,
}

temp := temperature["Earth"]
//打印出"On average the Earth is 15 °C."
fmt.Printf("On average the Earth is %v°C.\n", temp)
//修改数据以反应气候变化
temperature["Earth"] = 16
temperature["Venus"] = 464

//打印出"map[Venus:464 Earth: 16 Mars:-65]"
fmt.Println(temperature)
```
如果程序访问的键并不存在,那么Go语言将根据值得类型返回相应的零值作为结果
```go
if moon, ok := temperature["Moon"]; ok {
	fmt.Printf("On average the moon is %v°C.\n", moon)
} else {
	fmt.Println("Where is the moon?")
}
```
额外的ok变量会在键Moon存在时被设为true,并在键Moon不存在时被设置为false
### 映射不会被复制
数组在被赋值给了新变量或者传递至函数或方法的时候都会创建相应的副本,而映射不会,与切片类似,如果将映射传递给函数或者方法那么映射的内容就有可能被修改.

### 使用make函数对映射实行预分配

虽然映射与切片不一样,不会在赋值或者传递至函数的时候被复制;但与切片相似的是,除非使用复合字面量来初始化映射,否则必须使用内置的make函数来为映射分配空间.
make函数在创建映射的时候可以接受一个或两个形参,其中第二个形参用于为指定数量的键预先分配空间,就像分配切片的容量一样.使用make函数创建的新映射的初始长度总为0: `trmperature := make(map[float64]int, 8)`
### 使用映射进行计数
```go
temperatures := []float64 {
	-28.0, 32.0, -31.0, -29.0, -23.0, -28.0, -33.0
}

frequency := make(map[float64]int)
//迭代切片,取出其中的索引和值
for _, t := range temperatures {
	frequency[t]++
}
//迭代映射,取出其中的键和值
for t, num := range  frequency {
	fmt.Printf("%+.2f occurs %d times\n", t, num)
}
```
Go在迭代映射时并不保证键的顺序,因此,同样的映射在进行多次迭代时可能会产生不一样的结果
### 使用映射和切片实现数据分组
```go
temperatures := []float64 {
	-28.0, 32.0, -31.0, -29.0, -23.0, -29.0, -28.0, -33.0
}

group := make(map[float64] []float64)

for _, t := range temperatures {
	g := math.Trunc(t/10) * 10
	groups[g] = append(groups[g], t)
}

for g, temperatures := range group {
	fmt.Printf("%v: %v\n", g, temperatures)
}
```
以上输出结果为:
30: \[32\]
-30: \[-31 -33\]
-20: \[-28 -29 -23 -29 -28\]
### 将映射用作集合
集合这种收集器与数组非常相似,唯一的区别在于,集合保证其中的每个元素只会出现一次.
```go
var temperatures = []float64 {
	-28.0, 32.0, -31.0, -29.0, -23.0, -29.0, -28.0, -33.0
}

//创建一个映射,它的值为布尔类型
set := make(map[float64]bool)
for _, t := range temperatures {
	set[t] = ture
}
//打印出"set member"
if set[-28.0] {
	fmt.Println("set member")
}
//打印出"map[-31:true-29:true-23:true-33:true-28:true32:true]"
fmt.Println(set)
```
对于被用作集合的映射来说,键的值通常并不重要,但是为了便于检查集合成员关系,键的值通常会被设置为true.
```go
//因为映射的键在Go语言中是无序的,所以为了产生有序输出,必须先将键中存储的温度转换成切片,然后再对切片元素进行排序
unique := make([]float64, 0, len(set))
for t := range set {
	unique = append(unique, t)
}
sort.Float64s(unique)
fmt.Println(unique) //打印出"[-33 -31 -29 -28 -23 32]"
```
# 五 状态与行为
## 21_结构
### 声明结构
```go
var curiosity struct {
	lat float64
	long float64
}
//为结构中的字段赋值
curiosity.lat = -4.5895
curiosity.long = 137.4417

//通过复用复用结构
var curiosity1 curiosity
curiosity1.lat = -1.5895
curiosity1.long = 111.4417
var curiosity2 curiosity
curiosity2.lat = -2.5895
curiosity2.long = 222.4417
//通过复合字面量来初始化结构
curiosity3 := curiosity{lat = 3, long = 33}
curiosity4 := curiosity{4, 44}

fmt.printf("%v\n", curiosity) //打印出"{4, 44}"
fmt.printf("%v+\n", curiosity)//打印出"{lat: 4, long: 44}"
```
### 结构被复制
结构赋值将建立相应的副本,两个结构发生的变化不会对对方产生影响
```go
bradbury := location{-4.5895, 137.4417}
curiosity := Bradbury

curiosity.long += 0.0106
fmt.Println(bradbury, curiosity) // 打印出"{-4.5895 137.4417}{-4.5895 137.4523}"
```
### 由结构组成的切片
\[ \]struct用于表示由结构组成的切片, 切片包含的每一个值都是一个结构,而不是float64这样的基本类型
```go
type location struct {
	name string,
	lat float64,
	long float64
}

locations := []locations{
	{name: "Bradbury Landing", lat: -4.5895, long: 137.4417},
	{name: "Columbia Memorial Station", lat: -14.5684, long: 175.472636},
	{name: "Challenger Memorial Station", lat: -1.9426, long: 354.4734},
}
```
### 将结构编码成JSON
JavaScript对象标识法(JavaScript Object Notation, Json)常常被用于Web API(应用程序接口)
```go
package main
import (
	"encoding/json"
	"fmt"
	"os"
)
func main() {
	//字段必须以大写字母开头(需要导出,被外部访问)
	type location struct {
		Lat, Long float64
	}
	curiosity := location{-4.5895, 137.4417}

	bytes, err := json.Marshal(curiosity)
	exitOnError(err)
	//打印出"{"Lat":-4.5895,"Long":137.4417}"
	fmt.Println(string(bytes))
}
//exitOnError打印所有错误和退出信息
func exitOnError(err error) {
	if err != nil {
		fmt.Println(err)
		os.Exit(1)
	}
}
```
来自json包的Marshal函数将把location结构中的数据编码为JSON格式,并以字节形式返回编码后的JSON数据.这些数据既可以通过网络进行传输,也可以转换为字符串以便打印.(Marshal函数在某些情况下还会返回一个错误)
编码得出的JSON数据的键与location结构的字段名是一一对应的.需要注意的是,Marshal函数只会对结构中被导出的字段实施编码.换句话说,如果上例中location结构的Lat字段和Long字段都以小写字母开头,那么编码的结果将会是{ }
### 使用结构标签定制JSON
Go语言的json包要求结构中的字段必须以大写开头,并且包含多个单词的字段名称必须使用驼峰形命名惯例.对结构中的字段打标签(tag),是json包在编码数据的时候能够按照任意修改字段的名称.
```go
type location struct {
	Lat float64 `json: "latitude"`
	Long float64 `json: "longitude"`
}

curiosity := location{-4.5895, 137.4417}

bytes, err := json.Marshal(curiosity)
exitOnError(err)
//打印出"{"latitude":-4.5895,"longitude":137.4417}"
fmt.Println(string(bytes))
```
结构标签实际上就是一段与结构字段相关联的字符串
## 22_Go没有类
### 将方法绑定到结构
通过为coordinate类型绑定decimal方法,将DMS格式的坐标转换为十进制格式.
```go
//使用读/分/秒格式的坐标标识东南西北半球
type coordinate struct {
	d, m, s float64
	h rune
}
//decimal方法会将DMS格式的坐标转换为十进制格式
func (c corrinate) decimal() float64 {
	sign := 1.0
	switch c.h {
		case 'S', 'W', 's', 'w': sign - 1
	}
	return sign * (c.d + c.m/60 + c.s/3600)
}
lat := coordinate{4, 35, 22.2, 'S'}
long := coordinate{137, 26, 30.12, 'E'}
//打印出"4.5859 137.4417"
fmt.Println(lat.decimal(), long.decimal())

//使用以下复合字面量的同时调用decimal方法,基于DMS坐标构建一个十进制位置
type location struct {
	lat, long float64
}
curiosity := location{lat.decimal(), long.decimal()}
//如果想在初始化结构的同时做更多事情,可以编写一个构造函数
//newLocation函数会根据DMS格式的纬度坐标和经度坐标,创建出对应的十进制位置
func newLocation(lat, long coordiante) location {
	return location{lat.decimal(), long.decimal()}
}
```
Go语言没把构造对象的构造器设置成特殊的语言特性,而是选择了名称格式为newType或NewType的函数用于构造指定类型的值,至于函数名首字母的大小写则由函数是否需要导出以供其他包使用来决定.上例中的newLocation函数就是一个遵循上述构造器命名规则惯例的普通函数,可以像使用其他函数一样使用它:
```go
curiosity := newLocation(coordinate{4, 35, 22.3, 'S'}, coordinate{137, 26 30.12, 'E'})
//打印出"{-4.5895 137.4417}"
fmt.Println(curriosity)
```
### 类的替代品
Go语言没有提供类,而是通过结构和方法来满足相同的需求
```go
import "math"
//构建一个全新的world类型,world类型有一个记录行星半径的字段,这个字段用户计算行星中两个位置之间的距离
type world struct {
	radius float64
}
var mars = world{radius: 3389.5}
//rad函数会将角度转换为弧度
func rad(deg float64) float64 {
	return deg * math.Pi / 180
}
func (w world) distance(p1, p2 location) float64 {
	s1, c1 := math.Sincos(rad(p1.lat))
	s2, c2 := matc.Sincos(rad(p2.lat))
	clong := math.Cos(rad(p1.long - p2.long))
	//使用world结构的radius字段
	return w.radius * math.Acos(s1*s2+c1*c2*clong)
}

spirit := location{-14.5684, 175.472636}
oportunity := location{-1.9362, 354.4734}
//使用mars变量的distance方法
dist := mars.distance(spririt, opportunity)
fmt.Printf("%.2f km\n", dist) //打印出"9669,71"
```
为了计算出火星上的两个位置之间的距离,distance方法使用了火星的半径,但计算所用的公式实际上是为地球而写的.通过将distance声明为world类型的方法,可以将该方法用于计算其他世界(如地球)上两个位置之间的距离,而不仅仅是火星.
## 23_组合与转发
在面向对象编程的世界中,对象同样会由更小的对象组合而成,把这种行为叫做对象组合或简称组合,Go通过结构实现组合,并通过名为嵌入的特殊语言特性实现方法转发.
### 合并结构
```go
//未合并的单一结构
type repore struct {
	sol int
	high, low float64
	lat, long float64
}



//位于结构中的结构
type report struct {
	sol int
	temperature temperature //temperature字段的值是一个temperature类型的结构
	location location
}

type temperature struct {
	high, low selsius
}
type location struct {
	lat, long float64
}
type celsius float64

//通过位置和温度数据,构造出以下天气报告
bradbury := location{-4.5895, 137.4417}
t := temperature{high: -1.0, low: -78.0}
report := report{sol:15, temperature:{high: -1.0, low: -78.0}, location:{-4.5895, 137.4417}}
//打印出"{sol: 15, temperature: t, location: bradbury}"
fmt.Printf("%+v\n", report)
//打印出"a balmy -1°C"
fmt.Printf("a balmy %v°C\n", report.temperature.hingh)

//使用average方法来计算平均温度
func (t temperature) average() celsius {
	return (t.high + t.low) / 2
}
```
temperature类型和average方法可以在report结构体之外独立使用:
```go
t := temperature{high: -1.0, low: -78.0}
//打印出"average-39.0°C
fmt.Printf("average %v°C\n", t.average())
```
如果创建了一个report,同样可以通过temperature字段访问average方法:
```go
report := report{sol: 15, temperature: t}
//打印出"average-39.0°C
fmt.Printf("average %v°C\n", report.temperature.average())
```
如果想要通过report类型直接访问平均温度,只需要编写一个转发至实际实现的方法:
```go
func (r report) average() celsius {
	//打印出"average-39.0°C
	return r.temperature.average()
}
```
### 实现自动的转发方法
转发方法能够令方法变得易用.但如果每次转发方法都要像上例那样手动编写方法,那么转发方法将会变得相当不便.Go语言可以通过结构嵌入实现自动的转发方法,只需要在不给定字段名的情况下指定类型即可:
```go
//不给定字段名的情况下指定类型
type report struct {
	sol int
	temperature //将temperature类型嵌入report中
	location
}
//这样一来,temperature类型的所有方法将自动未report类型所用
report := report {
	sol : 15,
	location: location{-4.5895, 137.4417},
	temperature: temperature{high: -1.0, low: -78.0},
}
fmt.Printf("average %°C\n", report.average()) //打印出"average-39.0°C
//将类型嵌入结构需要指定字段名,结构会自动为被嵌入的类型生成同名的自动
fmt.Printf("average %°C\n", report.temperature.average()) //打印出"average-39.0°C

//嵌入不仅会转发方法,还能够让外部结构直接访问内部结构中的字段
fmt.Printf("%v°C\n",report.high) //打印出"-1°C"
report.high = 32
fmt.Printf("%v°C\n", report.temperature.high) //打印出"32°C"

//除了结构,还可以将任意其他类型嵌入结构
type sol int
type report struct {
	sol
	location
	temperature
}
//在此之后,基于sol类型声明的所有方法都能够通过sol字段或者report类型进行访问:
func (s sol) days(s2 sol) int {
	days := int(s2 - s)
	if days < 0 {
		days = -days
	}
	return days
}
func main() {
	report := report{sol: 15}

	//打印出"1431"
	fmt.Println(report.sol.days(1446))
	fmt.Println(report.days(1446))
}
```
### 命名冲突
```go
//report结构嵌入了sol和location两种类型,并且它们都具有名为days的方法
func (l location) days(12 location) int {
	//...
	return 5
}

//如果report类型的days方法被调用了,那么Go编译器将报告一个错误,因为它不知道要调用哪个方法
//Go编译器只会在同名方法被调用的情况下报错
d := report.days(1446) //这是一个有歧义的选择器

//当为report类型实现一个days方法,那么它的优先级就会高于嵌入类型的其他同名方法,可以手动转发新的days方法至指定的嵌入类型,也可以执行一些其他的操作:
func (r report) days(s2 sol) int {
	retrurn r.sol.days(s2)
}
```
## 24_接口
Go标准库提供了写入接口Writer,可以通过这个接口将文本, 图像, 逗号分隔值(CSV)和压缩文档等数据写入屏幕, 磁盘文件会在Web请求的响应当中.
### 接口类型
大多数类型关心的是自己存储的值,但接口关注的是类型可以做什么,而不是存储了什么值.
类型通过方法表达自己的行为,而接口通过列举类型必须满足的一组方法来进行声明.
```go
//任何类型的值,只要它满足了接口的要求,就能够称为变量t的值
var t interface {
	talk() string
}

//满足接口的要求
//虽然martian类型是一个不包含任何字段的结构,而laser类型则是一个整数,但由于它们都提供了满足接口要求talk()方法,因此它们都能够被赋值给t
type martian struct{}
func (m martian) talk() string {
	return "nack nack"
}
type laser int
func (l laser) talk() string {
	return strings.Repeat("pew", int(1))
}
```
具有变形功能的变量t能够采用martian或者laser两种形式,用计算机科学家的话讲就是接口通过多态让变量t具备了"多种形态"
```go
t = martian{}
fmt.Println(t.talk()) //打印出"neckneck"
t = laser(3)
fmt.Println(t.talk()) //打印出"pewpewpew"
```
为了便于复用,通常会把接口声明为类型并为其命名.按照惯例,接口类型的名称会以-er作为后缀
```go
type talker interface {
	talk() string
}
```
接口类型可以用在其他类型能够使用的任何地方
```go
//shout函数接收talker类型的值作为形参
func shout(t talker) {
	louder := strings.ToUpper(t.talk())
	fmt.Println(louder)
}

//传递给shout函数的实参必须满足talker接口
shout(martian{}) //打印出"NECKNECK"
shout(laser(2)) //打印出"PEWPEW"
``` 
将laser嵌入starship使得starship能够转发laser的talk方法,因此当尝试让starship开口说话的时候,laser将为其代劳.与此同时,因为starship满足了talker接口,所以shout函数可以使用starship类型的值作为参数
```go
type starship struct {
	laser
}

s := starship(laser(3))

fmt.Println(s.talk()) //打印出"pewpewpew"
shout(s) ////打印出"PEWPEWPEW"
```
### 探索接口
Go语言允许在实现代码的过程中随时创建新的接口.任何代码都能实现接口,包括那些已经存在的代码
```go
package main

import (
	"fmt"
	"time"
)

//stardate函数为给定日期返回一个虚构的星历
func stardate(t time.Time) float64 {
	doy := dloat64(t.YearDay())
	h := float64(t.Hour()) / 24.0
	return 1000 + doy + h
}

func main()  {
	day := time.Date(2012, 8, 6, 5, 17, 0, 0, time.UTC)
	fmt.Printf("%.1f Curiosity has landed\n", stardate(day)) //打印出"1219.2 Curiosity has landed"
}
```
以上的stardate函数只能使用地球日期作为输入,为了解决这个问题,声明一个接口以供stardate使用.
因为标准库中的time.Time类型满足stardater接口,所以以下新的stardate函数将能够继续处理地球日期.Go语言的接口都是隐式满足的,这一特性在使用其他人编写的代码时特别有用.
```go
type stardater interface {
	YearDay() int
	Hout() int
}

//stardate函数返回虚构的星历
func stardate(t stardater) float64 {
	doy := float64(t.YearDay())
	h := float64(t.Hout()) / 24.0
	return 1000 + doy + h
}
```
在具有stardater接口之后,就可以使用sol类型对星历的实现进行扩展
```go
//sol类型通过实现YearDay方法和Hour方法满足了stardater接口
type sol int

func (s sol) YearDay() int {
	return int(s % 668) //一火星年有668火星日
}
func (s sol) Hour() int {
	return 0 //小时数未知
}

//现在stardate函数将能偶同时处理地球日期和火星日
day := time.Date(2012, 8, 6, 5, 17, 0, 0, time.UTC)
fmt.Printf("%.1f Curiosity has landed\n", stardate(day)) //打印出"1219.2Curiosityhaslanded"
s := sol(1422)
fmt.Printf("%.1f Happy birthday\n", stardate(s)) //打印出"1086.0Happybirthday"
```
*隐式满足接口使得自己声明的接口可以由其他编写的代码来满足*
### 满足接口
Go标准库导出了很多只有单个方法的接口.例如,fmt包就声明了以下所示的Stringer接口:
```go
type Stringer interface {
	
	String() string
}
```
得益于这个接口,一种类型只要提供了String方法,它的值就能够为Println Sprintf等打印函数所用
location类型实现了一个String方法,它可以告诉fmt包该如何打印一个位置
```go
package main

import "fmt"

//location结构使用十进制格式存储纬度和经度
type location struct {
	lat, long float64
}
//String方法会对位置的纬和经度进行格式化
func (l location) String() string {
	return fmt.Sprintf("%v, %v, l.lat, l.long)
}
func main() {
	curiosity := location{-4.5895, 137.4417}
	fmt.Println(curiosity) //打印出"-4.5895,137.4417"
}
```
除了fmt.Stringer,标准库中常用的接口还包括io,Reader, io.Writer, 和json.Marshaler.
# 六 深入Go语言
## 26_关于指针的二三事
指针时指向另一变量的地址的变量. Go语言确实提供了指针,但同时野强调内存安全.
### &和*
变量会将它们的值存储在计算机的随机访问存储器里面,而值的存储位置则是该变量的内存地址.通过使用&表示的地址操作符,可以得到指定变量的内存地址.
*地址操作符无法取得字符串字面量, 数字字面量,和布尔值字面量的地址; 诸如&42 和 &"another level 偶发indirection"的语句都会导致报错*

```go
answer := 42
fmr.Println(&answer) //打印出"0x..."
```
地址操作符(&)提供值的内存地址,而它的反向操作**解引用**则提供内存地址指向的值
*在C语言中的内存地址可以通过诸如address+这样的指针运算进行操作,但Go语言不允许*
```go
//address变量虽然没有直接持有answer变量的值42,但因为它持有answer变量的内存地址,所以知道在哪里能找到这个值
address := &answer
fmt.Println(*address) //打印出42

//address变量实际上就是*int类型的指针
fmt.Printf("address is a %T\n", addrss) //打印出"address is a int"
```
指针类型可以跟其他普通类型一样出现在所有需要用到类型的地方,如函数声明,返回值类型,结构字段类型等.
```go
canada := "Canada"
//声明一个指针类型的变量
var home *string
fmt.Printf("home is a %T\n", home) //打印出"home is a string"

homt = &canada
fmt.Println(*home) //打印出"Canada"
```
### 指针的作用就是指向
