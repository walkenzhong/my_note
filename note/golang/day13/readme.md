# day13
## summary
并发编程  
并发在图中的解释是两队人排队接咖啡，两队切换。  
并行是两个咖啡机，两队人同时接咖啡。
## goroutine
Go 允许使用 go 语句开启一个新的运行期线程， 即 goroutine，以一个不同的、新创建的 goroutine 来执行一个函数。 同一个程序中的所有 goroutine 共享同一个地址空间。
```
package main

import (
        "fmt"
        "time"
)

func say(s string) {
        for i := 0; i < 5; i++ {
                time.Sleep(100 * time.Millisecond)
                fmt.Println(s)
        }
}

func main() {
        go say("world")
        say("hello")
}
/*
world
hello
hello
world
world
hello
hello
world
world
hello
*/
```
### WaitGroup
```
// 这是我们将在每个goroutine中运行的函数。
// 注意，等待组必须通过指针传递给函数。
func worker(id int, wg *sync.WaitGroup) {

	defer wg.Done()

	fmt.Printf("Worker %d starting\n", id)

	time.Sleep(time.Second)
	fmt.Printf("Worker %d done\n", id)
}

func main() {

	var wg sync.WaitGroup

	for i := 1; i <= 5; i++ {
		wg.Add(1)
		go worker(i, &wg)
	}

	wg.Wait()
}
/*
这里首先把wg 计数设置为1， 每个for循环运行完毕都把计数器减一，主函数中使用Wait() 一直阻塞，直到wg为1——也就是所有的5个for循环都运行完毕。
*/
```
### Once
sync.Once可以控制函数只能被调用一次，不能多次重复调用。
```
var doOnce sync.Once

func main() {
	DoSomething()
	DoSomething()
}

func DoSomething() {
	doOnce.Do(func() {
		fmt.Println("Run once - first time, loading...")
	})
	fmt.Println("Run this every time")
}
/*
Run once - first time, loading...
Run this every time
Run this every tim
*/
```
### 互斥锁Mutex
读写不分离
```
// SafeCounter 的并发使用是安全的。
type SafeCounter struct {
	v   map[string]int
	mux sync.Mutex
}

// Inc 增加给定 key 的计数器的值。
func (c *SafeCounter) Inc(key string) {
  c.mux.Lock()
  defer c.mux.Unlock()
	// Lock 之后同一时刻只有一个 goroutine 能访问 c.v
  c.v[key]++
}

// Value 返回给定 key 的计数器的当前值。
func (c *SafeCounter) Value(key string) int {
	c.mux.Lock()
	// Lock 之后同一时刻只有一个 goroutine 能访问 c.v
	defer c.mux.Unlock()
	return c.v[key]
}

func main() {
	c := SafeCounter{v: make(map[string]int)}
	for i := 0; i < 1000; i++ {
		go c.Inc("somekey")
	}

	time.Sleep(time.Second)
	fmt.Println(c.Value("somekey"))
}
```
读写分离
```
// SafeCounter 的并发使用是安全的。
type SafeCounter struct {
	v     map[string]int
	rwmux sync.RWMutex
}

// Inc 增加给定 key 的计数器的值。
func (c *SafeCounter) Inc(key string) {
	// 写操作使用写锁
	c.rwmux.Lock()
	defer c.rwmux.Unlock()
	// Lock 之后同一时刻只有一个 goroutine 能访问 c.v
	c.v[key]++
}

// Value 返回给定 key 的计数器的当前值。
func (c *SafeCounter) Value(key string) int {
  // 读的时候加读锁
	c.rwmux.RLock()
	// Lock 之后同一时刻只有一个 goroutine 能访问 c.v
	defer c.rwmux.RUnlock()
	return c.v[key]
}

func main() {
	c := SafeCounter{v: make(map[string]int)}
	for i := 0; i < 1000; i++ {
		go c.Inc("somekey")
	}

	time.Sleep(time.Second)

	for i := 0; i < 10; i++ {
		fmt.Println(c.Value("somekey"))
	}
}
```
### 条件变量Cond
```
//NewCond
//创建一个Cond的条件变量。

func NewCond(l Locker) *Cond
//Broadcast
//广播通知，调用时可以加锁，也可以不加。

func (c *Cond) Broadcast()
//Signal
//单播通知，只唤醒一个等待c的goroutine。

func (c *Cond) Signal()
//Wait 等待通知, Wait()会自动释放c.L，并挂起调用者的goroutine。之后恢复执行，Wait()会在返回时对c.L加锁。
//除非被Signal或者Broadcast唤醒，否则Wait()不会返回。

func (c *Cond) Wait()
```
### 原子操作
原子操作即是进行过程中不能被中断的操作。针对某个值的原子操作在被进行的过程中，CPU绝不会再去进行其他的针对该值的操作。 为了实现这样的严谨性，原子操作仅会由一个独立的CPU指令代表和完成。
### 临时对象池Pool
```
var pool *sync.Pool

type Foo struct {
	Name string
}

func Init() {
	pool = &sync.Pool{
		New: func() interface{} {
			return new(Foo)
		},
	}
}

func main() {
	fmt.Println("Init p")
	Init()

	p := pool.Get().(*Foo)
	fmt.Println("第一次取：", p)
	p.Name = "bob"
	pool.Put(p)

	fmt.Println("池子有对象了，调用获取", pool.Get().(*Foo))
	fmt.Println("池子空了", pool.Get().(*Foo))
}
```
## channel
通道可用于两个 goroutine 之间通过传递一个指定类型的值来同步运行和通讯。操作符 <- 用于指定通道的方向，发送或接收。如果未指定方向，则为双向通道。
```
package main

import "fmt"

func sum(s []int, c chan int) {
        sum := 0
        for _, v := range s {
                sum += v
        }
        c <- sum // 把 sum 发送到通道 c
}

func main() {
        s := []int{7, 2, 8, -9, 4, 0}

        c := make(chan int)
        go sum(s[:len(s)/2], c)
        go sum(s[len(s)/2:], c)
        x, y := <-c, <-c // 从通道 c 中接收

        fmt.Println(x, y, x+y)
}
//-5 17 12
```
