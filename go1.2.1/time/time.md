## time 包

常量定义(layouts)

  + 下列时间格式用于对时间数据的格式化
  - 在Golang中将2006年一月2日15:04:05作为时间基准点
  - 格式化日期，时间时传入一个格式化字符串即可
  - 下面所列为常用格式，也可以自己定义常量对时间数据进行格式化操作

```
const (
    ANSIC       = "Mon Jan _2 15:04:05 2006"
    UnixDate    = "Mon Jan _2 15:04:05 MST 2006"
    RubyDate    = "Mon Jan 02 15:04:05 -0700 2006"
    RFC822      = "02 Jan 06 15:04 MST"
    RFC822Z     = "02 Jan 06 15:04 -0700" // RFC822 with numeric zone
    RFC850      = "Monday, 02-Jan-06 15:04:05 MST"
    RFC1123     = "Mon, 02 Jan 2006 15:04:05 MST"
    RFC1123Z    = "Mon, 02 Jan 2006 15:04:05 -0700" // RFC1123 with numeric zone
    RFC3339     = "2006-01-02T15:04:05Z07:00"
    RFC3339Nano = "2006-01-02T15:04:05.999999999Z07:00"
    Kitchen     = "3:04PM"
    // Handy time stamps.
    Stamp      = "Jan _2 15:04:05"
    StampMilli = "Jan _2 15:04:05.000"
    StampMicro = "Jan _2 15:04:05.000000"
    StampNano  = "Jan _2 15:04:05.000000000"
)

```

使用固定字符串格式化时间示例

```
package main

import(
  "fmt"
  "time"
)

func main(){

   const TimeFormat="2006-01-02 15:04:05"      // YYYY-MM-DD hh:mm:ss

   fmt.Println(time.Now().Format(TimeFormat))  // 2015-01-10 22:18:38

}

```

类型

```
//以int64为原型， 定义Duration 类型， Duration的实质是int64类型

type Duration int64

//纳秒，微秒，毫秒，秒，分，时转换关系

const (
    Nanosecond  Duration = 1                   //纳秒
    Microsecond          = 1000 * Nanosecond   //微秒
    Millisecond          = 1000 * Microsecond  //毫秒
    Second               = 1000 * Millisecond  //秒
    Minute               = 60 * Second         //分钟
    Hour                 = 60 * Minute         //小时
)


// 可以直接使用这些常量作为基准量，比如传递一个参数为10秒，可以使用 10 * time.Second 表示


type Location struct {

}

// 指代地理时区

var Local *Location = &localLoc
Local 代表本地（操作系统的使用所在地所在）时区

var UTC *Location = &utcLoc
UTC 代表通用协调时间(UTC).


time.UTC   utc时间
time.Local 本地时间


// 月份
type Month int

const (
    January Month = 1 + iota          //一月， iota=0
    February                          //二月， iota=1, 1+1
    March                             //三月， iota=2, 1+2
    April
    May
    June
    July
    August
    September
    October
    November
    December                          //十二月，iota=11, 1+11
)

// 可以直接使用这些常量，比如一月：time.January

_, month, day := time.Now().Date()           //  func (t Time) Date() year,month,day
if month == time.November && day == 10 {
    fmt.Println("Happy Go day!")
}


// 时间解析错误类型
type ParseError struct {
    Layout     string           //layout 可参考最上面的格式化layout参数
    Value      string           //值
    LayoutElem string           //layout元素
    ValueElem  string           //值元素
    Message    string           //错误信息
    //包含过滤或导出域
}


// Ticker
type Ticker struct {
    C <-chan Time    //C属性，chan只读类型，Time类型数据通道
    //包含过滤或导出域
}


//Time
type Time struct {
    //包含过滤或导出域
}


//Timer(定时器)
type Timer struct {
    C <-chan Time  //C属性，chan只读类型，Time类型数据通道
    //包含过滤或导出域
}

// 星期
type Weekday int

const (
    Sunday Weekday = iota    //0
    Monday                   //1
    Tuesday                  //2
    Wednesday
    Thursday
    Friday
    Saturday                 //6
)

//同样可以直接使用time.Sunday或者Weekday类型的星期天，代码为0， 取值范围为（0-6）

```


函数列表：

1  After

| 函数名  |参数 | 返回值  | 功能  |
|---|---|---|---|
|After|Duration 类型|<-chan（只读chan） Time | 等待，并返回等待结束后的时间 |

函数定义

```
func After(d Duration) <-chan Time
```


2  Sleep

| 函数名  |参数 | 返回值  | 功能  |
|---|---|---|---|
|Sleep|Duration 类型|无| 等待，并返回等待结束后的时间 |

函数定义

```
func Sleep(d Duration)
```

示例

```
package main

import(
  "time"
  "fmt"
)

func main(){
  t1 := time.Now().Second();
  fmt.Println(t1)   //获取输出当前时间
  time.Sleep(100 * time.Millisecond)
  t2 := time.Now().Second()
  fmt.Println(t2)
  fmt.Println(t1-t2)
}

```

3  Tick

| 函数名  |参数 | 返回值  | 功能  |
|---|---|---|---|
|Tick|Duration 类型|<-chan（只读chan） Time| 等待，并返回等待结束后的时间 |

函数定义

```
func Tick(d Duration) <-chan Time
```

示例

```
package main

import(
  "time"
  "fmt"
)

func main(){
    c := time.Tick(1 * time.Minute)
    for now := range c {
        fmt.Printf("%v %s\n", now, statusUpdate())
    }
}

```

4  ParseDuration

| 函数名  |参数 | 返回值  | 功能  |
|---|---|---|---|
|ParseDuration|string 类型|Duration , error |将时间字符串解析成Duration类型 |

函数定义

```
func ParseDuration(s string)(Duration, error)
```

函数说明

ParseDuration 用于解析时间字符串。一个时间字符串可能由十进制数值，可选分数和单位后缀构成, 比如“300ms”，“-15h”或“2h45m”等。有效的时间单位是 "ns"（纳秒）, "us"（微秒） (或者 "µs"), "ms"（毫秒）, "s"（秒）, "m"（分）, "h"（时）.


5  Since

| 函数名  |参数 | 返回值  | 功能  |
|---|---|---|---|
|Since|Time|Duration|将时间字符串解析成Duration类型 |

函数定义

```
func Since(t Time) Duration
```

函数说明




6  Hours

| 函数名  |接受者|参数 | 返回值  | 功能  |
|---|---|---|---|---|
|Hours|Duration|空|float64|将Duration类型的时间转换成表示**小时**的浮点数|

函数定义

```
func (d Duration) Hours() float64

```
函数说明
Hours 作为一个浮点数小时返回时间。

7  Minutes

| 函数名  |接受者|参数 | 返回值  | 功能  |
|---|---|---|---|---|
|Minutes|Duration|空|float64|将Duration类型的时间转换成表示**分钟**的浮点数|


函数定义

```
func (d Duration) Minutes() float64

```
函数说明
Minutes 作为一个浮点数分钟返回时间。

8  NanoSeconds

| 函数名  |接受者|参数 | 返回值  | 功能  |
|---|---|---|---|---|
|NanoSeconds|Duration|空|float64|将Duration类型的时间转换成表示**纳秒**的浮点数|

函数定义

```
func (d Duration) NanoSeconds() float64

```
函数说明
NanoSeconds 作为一个浮点数纳秒返回时间。

9  Seconds

| 函数名  | 接受者 |参数 | 返回值  | 功能  |
|---|---|---|---|---|
|Seconds|Duration|空|float64|将Duration类型的时间转换成表示**秒**的浮点数|

函数定义

```
func (d Duration) Seconds() float64

```
函数说明
Seconds 作为一个浮点数秒返回时间。

10  String

| 函数名  | 接受者 | 参数 | 返回值  | 功能  |
|---|---|---|---|---|
|String|Duration|空 | String|将Duration类型的时间转换成字符串|

函数定义

```
func (d Duration) String() string

```

函数说明
String 函数将Duration类型的时间以字符串类型返回，并将时间表示为“72h3m0.5s”格式。前导零单位略（0时0分1秒，则表示为1s）。作为一个特殊的情况下，持续时间少于一秒的格式使用一个较小的单位（毫，微，或纳秒）以确保开头的数字是非零。零时间格式标示为0，无单位。


11  FixedZone

| 函数名  |参数 | 返回值  | 功能  |
|---|---|---|---|
|FixedZone|string, int |*Location|设置时区名,以及与UTC0的时间偏差.返回Location|

函数定义

```
func FixedZone(name string, offset int) *Location

```

函数说明

传入时区名,以及与UTC0的时间偏差. 返回指向Location结构体的指针 *Location


12  LoadLocation

| 函数名  |参数 | 返回值  | 功能  |
|---|---|---|---|
|LoadLocation|string|*Location, error |根据提供的时区名称，返回相对应的Location|

函数定义

```
func LoadLocation(name string) *Location

```

函数说明

传入时区名,返回指向Location结构体的指针 *Location
假如name是""或者"UTC",函数返回UTC, 假如name是"Local", 函数返回Local,否则，这个名字是对应于在IANA时区数据库文件的位置名称，如"America/New_York"。
LoadLocation函数所需要的时区数据库不一定在所有的系统中都存在，特别是非类Unix系统，可以在$GOROOT/lib/time/zoneinfo.zip.找到

13  String

| 函数名  |接受者| 参数 | 返回值  | 功能  |
|---|---|---|---|---|
|String|*Location|无|String|根据提供的时区名称，返回相对应的Location|

函数定义

```
func (l *Location) String() string

```

函数说明

将本地时区信息以字符串形式返回


14  String

| 函数名  | 接受者 | 参数 | 返回值  | 功能  |
|---|---|---|---|---|
|String|Month|无|String|返回月份的英文全称|

函数定义

```
func (m Month) String() string

```

函数说明

返回月份的英文名称，需要中文的可以自己写类似StringZh函数进行映射


15  NewTicker

| 函数名  | 接受者 | 参数 | 返回值  | 功能  |
|---|---|---|---|---|
|NewTicker| 无 | Duration | *Ticker|返回新的Ticker|

函数定义

```
func NewTicker(d Duration) *Ticker

```

函数说明
返回一个包含一个只读chan成员的新的Ticker, 传入的Duration必须大于0，否则Ticker将发生异常错误。


16  Stop

| 函数名  | 接受者 | 参数 | 返回值  | 功能  |
|---|---|---|---|---|
|Stop| *Ticker | 空 | 无 | 停止Ticker的读操作 |

函数定义

```
func (t *Ticker) Stop()

```
Stop 函数将关闭ticker的通道，函数执行后,将不会有消息被发送。但是为了防止已经发生的从Stop函数并不会关闭Ticker内部的channel进行的读操作发生错误，Stop函数并不会关闭Ticker内部的channel。


17  Date

| 函数名  | 接受者 | 参数 | 返回值  | 功能  |
|---|---|---|---|---|
|Date| 无 | int,month,int,*Location | Time | 返回Time对象 |

函数定义

```
func Date(year int, month Month, day, hour, min, sec, nsec int, loc *Location) Time

```

函数说明

Date 函数根据提供的年，月，日，时，分，秒， 时区偏差量， UTC/Local, 返回一个包含可以被导出或者访问属性的结构体Time对象。
返回的时间格式为YYYY-MM-DD hh:mm:ss + nsec nanoseconds, 时区选择（***）， 如果loc是空对象nil，则Date函会报错。

示例

```
package main

import(
  "time"
  "fmt"
)

func main(){

  t := time.Date(2009, time.November, 10, 23, 0, 0, 0, time.UTC)
  
  fmt.Printf("Go launched at %s\n", t.Local())
  
}


程序运行结果:

Go launched at 2009-11-10 15:00:00 -0800 PST

```

18 Now

| 函数名  | 接受者 | 参数 | 返回值  | 功能  |
|---|---|---|---|---|
|Now| 无 | 空 | Time | 获取当前时间，返回Time对象 |

函数定义

```
func Now() Time

```

函数说明

Now 函数返回当前的本地时间

示例

```

package main

import(
  "fmt"
  "time"
)

func main(){

  t:= t.Now()
 
  fmt.Println(t.String())
 
}


// 程序运行结果
2015-01-11 14:57:51.536225133 +0800 CST

```

19 Parse

| 函数名  | 接受者 | 参数 | 返回值  | 功能  |
|---|---|---|---|---|
|Parse| 无 | layout, value | Time, error | 将时间按照指定的字符串形式进行格式化，并返回 |

函数定义

```   
func Parse(layout, value string) (Time, error)

```

函数说明

Parse 函数 解析一个格式化字符串并返回它所代表的时间值。布局定义的格式显示参考时间。

示例

```
package main

import(
  "time"
  "fmt"
)

func main(){
   
   const longForm = "Jan 2, 2006 at 3:04pm (MST)"
   t, _ := time.Parse(longForm, "Feb 3, 2013 at 7:54pm (PST)")
   fmt.Println(t)
   

   const shortForm = "2006-Jan-02"
   t, _ = time.Parse(shortForm, "2013-Feb-03")
   fmt.Println(t)

}

// 程序运行结果
2013-02-03 19:54:00 +0000 PST
2013-02-03 00:00:00 +0000 UTC


```

20 ParseInLocation

| 函数名  | 接受者 | 参数 | 返回值  | 功能  |
|---|---|---|---|---|
|ParseInLocation| 无 | layout, value， loc | Time, error | 将时间按照指定的字符串形式进行格式化，并返回 |

函数定义

```  
func ParseInLocation(layout, value string, loc *Location) (Time, error)

```

函数说明

ParseInLocation 与 Parse 类似，但是两个函数有两个不同点。
  - 1 ：当时区信息缺失的时候 Parse 返回一个 UTC 的 Time, ParseInLocation 则返回一个本地时间 Time ，
  - 2 ：当给定一个时区偏移量或者时区缩写，Parse 将试图解析本地位置， ParseInLocation 使用给定的位置 。


示例

```
package main

import(
  "fmt"
  "time"
)

func main(){

  loc, _ := time.LoadLocation("Europe/Berlin")
    
  const longForm = "Jan 2, 2006 at 3:04pm (MST)"     
  
  t, _ := time.ParseInLocation(longForm, "Jul 9, 2012 at 5:02am (CEST)", loc)
  
  fmt.Println(t)
 
  const shortForm = "2006-Jan-02"                              //只显示年，月，日
  
  t, _ = time.ParseInLocation(shortForm, "2012-Jul-09", loc)
  
  fmt.Println(t)
  
}

// 程序运行结果

2012-07-09 05:02:00 +0200 CEST
2012-07-09 00:00:00 +0200 CEST

```

21 Unix

| 函数名  | 接受者 | 参数 | 返回值  | 功能  |
|---|---|---|---|---|
|Unix| 无 | sec, nsec | Time | 提供 **秒**, **纳秒**时间， 并返回一个Unix格式的时间 Time |

函数定义

```  
func Unix(sec int64, nsec int64) Time

```

函数说明

Unix 函数返回当地时间对应于给定的UNIX时间，自1970年1月1日UTC秒秒和纳秒纳秒。它是有效的通过纳秒范围以外的[ 0，999999999 ]。


22 Add

| 函数名  | 接受者 | 参数 | 返回值  | 功能  |
|---|---|---|---|---|
|Add| Time | Duration | Time | 时间相加，返回新的时间 |

函数定义

```  
func (t Time) Add(d Duration) Time

```

函数说明

Add 返回相加后的 Time。


23 AddDate

| 函数名  | 接受者 | 参数 | 返回值  | 功能  |
|---|---|---|---|---|
|AddDate| Time |years,months,days | Time | 时间相加，返回新的时间 |

函数定义

```  
func (t Time) AddDate(years int, months int, days int) Time

```
 
函数说明

AddDate 函数返回相应的增加 年，月，天的时间， 例如，AddDate（-1，2，3）, 此时的 t 是 2011年1月1日， 则返回 2010年3月4日返回。
