### time
 자주 사용하는 Time 함수

* https://golang.org/pkg/time

```go
package main

import (
	"fmt"
	"time"
)

func main() {
    t := time.Now().In(time.Local)
    fmt.Printf("t0:%v\n", t)
    t1 := time.Now() // 현재시간 UTC
    fmt.Printf("t1:%v\n", t1)
    t2 := t1.Add(time.Second * 10)
    fmt.Printf("t2:%v\n", t2)
    d1 := t1.Sub(t2) // duration
    fmt.Printf("d1:%v\n", d1)
    
    // 시간 비교
    if t2.After(t1) {
        fmt.Println("t1 < t2")
    }
    if t1.Before(t2) {
        fmt.Println("t1 < t2")
    }
    if t1.Equal(t1) {
        fmt.Println("t1 == t2")
    }
    
    t3 := time.Date(2021, time.February, 1, 7, 30, 10, 1000, time.Local)
    fmt.Printf("t3:%s\n", t3)
    fmt.Printf("t3.Unix():%d\n", t3.Unix())
    fmt.Printf("t3.UnixNano():%d\n", t3.UnixNano())
    
    const layout = "2006/01/02 15:04:05.000000"
    dtStr := t3.Format(layout)
    fmt.Printf("t3.Format():%s\n", dtStr)
    
    t4, _ := time.Parse(layout, dtStr) // UTC
    fmt.Printf("t4(Utc):%s\n", t4)
    
    t5, _ := time.ParseInLocation(layout, dtStr, time.Local) // Local
    fmt.Printf("t5(Local):%s\n", t5)
    
    time.Sleep(time.Second * 10)


}

# Result
t0:2021-02-13 13:54:09.714159 +0900 KST
t1:2021-02-02 23:25:16.088354 +0900 KST m=+0.000094678
t2:2021-02-02 23:25:26.088354 +0900 KST m=+10.000094678
d1:-10000000000
t1 < t2
t1 < t2
t1 == t2
t3:2021-02-01 07:30:10.000001 +0900 KST
t3.Unix():1612132210
t3.UnixNano():1612132210000001000
t3.Format():2021/02/01 07:30:10.000000
t4(Utc):2021-02-01 07:30:10 +0000 UTC
t5(Local):2021-02-01 07:30:10 +0900 KST
```


* time.After(..), time.Tick(..)
```go
package main

import (
	"fmt"
	"time"
)

func main() {
	timeout := time.After(time.Second * 2)
	tick := time.Tick(time.Millisecond * 500)

	for {
		select {
		case <-timeout:
			fmt.Println("timed out!")
			return
		case <-tick:
			fmt.Println("tick!")
		}
	}
}

$ go run main.go
tick!
tick!
tick!
timed out!

```

* now.Unix(), time.Unix(..)

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	now := time.Now()

	// time to unixtime
	ts := now.Unix()
	fmt.Println(ts)

	// unixtime(int64) to time
	t := time.Unix(ts, 0)
	fmt.Printf("time:%v\n", t)
}

$ go run main.go
1613197380
time:2021-02-13 15:23:00 +0900 KST

```
