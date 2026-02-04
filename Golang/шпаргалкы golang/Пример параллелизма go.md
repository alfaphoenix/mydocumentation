
```go
package main

import (
	"fmt"  // пакет используется для проверки выполнения условия задачи, не удаляйте его
	"time" // пакет используется для проверки выполнения условия задачи, не удаляйте его
    "sync"
)
func merge2Channels(fn func(int) int, in1 <-chan int, in2 <-chan int, out chan<- int, n int) {
    go func() {
        vals := make([]int, n)
        var wg sync.WaitGroup
        wg.Add(2 * n)
        for i := 0; i < n; i++ {
            x1 := <-in1
            x2 := <-in2
            i := i
            go func() {
                vals[i] += fn(x1)
                wg.Done()
            }()
            go func() {
                vals[i] += fn(x2)
                wg.Done()
            }()
        }
        wg.Wait()
        for _, v := range vals {
            out <- v
        }
    }()
}
```