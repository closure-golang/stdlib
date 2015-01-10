## time 包

常亮

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

函数：


func After(d Duration) <-chan Time

函数名： After
参数 ：  Duration 类型               // type Duration int64
返回值： <-chan（只读chan）Time
功能：等待，并返回等待结束后的时间



func Sleep(d Duration)

函数名： Sleep
参数 ：  Duration 类型                // type Duration int64
返回值： 无
功能： 等待d时长，d之后继续执行代码
示例：
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


func Tick(d Duration) <-chan Time

函数名： Tick
参数 ：  Duration 类型                // type Duration int64
返回值： <-chan（只读chan）Time
功能：等待，并返回等待结束后的时间
示例：
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

结构体：
```
type Duration int64

const (
    Nanosecond  Duration = 1                   //纳秒
    Microsecond          = 1000 * Nanosecond   //微秒
    Millisecond          = 1000 * Microsecond  //毫秒
    Second               = 1000 * Millisecond  //秒
    Minute               = 60 * Second         //分钟
    Hour                 = 60 * Minute         //小时
)
```