
В Go **параллелизм** и **синхронизация** — это ключевые концепции, которые делают язык особенно мощным для написания высокопроизводительных и конкурентных программ. Вот основные идеи и инструменты, которые используются для работы с параллелизмом и синхронизацией:

---

## 1. **Горутины (Goroutines)**

Горутины — это лёгкие потоки выполнения, которые управляются средой выполнения Go. Они позволяют запускать функции параллельно.

**Пример:**

```GO
package main

import (
	"fmt"
	"time"
)

func say(s string) {
	for i := 0; i < 3; i++ {
		time.Sleep(100 * time.Millisecond)
		fmt.Println(s)
	}
}

func main() {
	// Запуск горутины
	go say("world")
	// Основная горутина
	say("hello")
}

```

- `go say("world")` запускает функцию `say` в новой горутине.
- Горутины выполняются независимо друг от друга.

---

## 2. **Каналы (Channels)**

Каналы — это механизм для обмена данными между горутинами и их синхронизации. Каналы обеспечивают безопасную передачу данных между горутинами.

**Пример:**
```go
package main

import "fmt"

func sum(s []int, c chan int) {
	sum := 0
	for _, v := range s {
		sum += v
	}
	c <- sum // отправка суммы в канал
}

func main() {
	s := []int{7, 2, 8, -9, 4, 0}
	c := make(chan int)
	go sum(s[:len(s)/2], c)
	go sum(s[len(s)/2:], c)
	x, y := <-c, <-c // получение данных из канала
	fmt.Println(x, y, x+y)
}

```

- `make(chan int)` создаёт канал для передачи целых чисел.
- `<-c` — операция чтения из канала (блокирует выполнение, пока данные не будут доступны).

---

## 3. **Синхронизация с помощью каналов**

Каналы можно использовать для синхронизации выполнения горутин. Например, чтобы дождаться завершения горутины:
```go
done := make(chan bool)
go func() {
    // какая-то работа
    done <- true // сигнал о завершении
}()
<-done // ожидание сигнала

```

---

## 4. **Select**

Конструкция `select` позволяет работать с несколькими каналами одновременно. Она блокирует выполнение, пока хотя бы один из каналов не будет готов к операции.

**Пример:**
```go
select {
case msg1 := <-ch1:
    fmt.Println("Received", msg1)
case msg2 := <-ch2:
    fmt.Println("Received", msg2)
case ch3 <- 3:
    fmt.Println("Sent 3")
default:
    fmt.Println("No communication")
}

```

---

## 5. **Мьютексы (Mutex)**

Для защиты общих ресурсов от одновременного доступа используются мьютексы из пакета `sync`.

**Пример:**

```go
package main

import (
	"fmt"
	"sync"
)

type SafeCounter struct {
	mu sync.Mutex
	v  map[string]int
}

func (c *SafeCounter) Inc(key string) {
	c.mu.Lock()
	c.v[key]++
	c.mu.Unlock()
}

func (c *SafeCounter) Value(key string) int {
	c.mu.Lock()
	defer c.mu.Unlock()
	return c.v[key]
}

func main() {
	c := SafeCounter{v: make(map[string]int)}
	for i := 0; i < 1000; i++ {
		go c.Inc("somekey")
	}
	fmt.Println(c.Value("somekey"))
}
```

- `sync.Mutex` блокирует доступ к ресурсу для других горутин, пока текущая не завершит работу.

---

## 6. **WaitGroup**

`sync.WaitGroup` используется для ожидания завершения группы горутин.

**Пример:**

```go
package main

import (
	"fmt"
	"sync"
)

func worker(id int, wg *sync.WaitGroup) {
	defer wg.Done()
	fmt.Printf("Worker %d starting\n", id)
	// какая-то работа
	fmt.Printf("Worker %d done\n", id)
}

func main() {
	var wg sync.WaitGroup
	for i := 1; i <= 5; i++ {
		wg.Add(1)
		go worker(i, &wg)
	}
	wg.Wait() // ожидание завершения всех горутин
}

```

- `wg.Add(1)` увеличивает счётчик горутин.
- `wg.Done()` уменьшает счётчик.
- `wg.Wait()` блокирует выполнение, пока счётчик не станет равным нулю.

---

## 7. **Паттерны конкурентности**

- **Worker Pool**: несколько горутин обрабатывают задачи из общего канала.
- **Fan-out/Fan-in**: распределение задач между горутинами и сбор результатов.
- **Pipeline**: цепочка горутин, где выход одного этапа — вход для другого.

---

## 8. **Контекст (Context)**

Пакет `context` позволяет передавать сигналы отмены и тайм-ауты между горутинами.

**Пример:**

```go
package main

import (
	"context"
	"fmt"
	"time"
)

func operation(ctx context.Context) {
	for {
		select {
		case <-ctx.Done():
			fmt.Println("Operation cancelled")
			return
		default:
			// работа
			time.Sleep(100 * time.Millisecond)
		}
	}
}

func main() {
	ctx, cancel := context.WithTimeout(context.Background(), 50*time.Millisecond)
	defer cancel()
	go operation(ctx)
	time.Sleep(100 * time.Millisecond)
}

```

- `context.WithTimeout` создаёт контекст с тайм-аутом.
- `ctx.Done()` возвращает канал, который закрывается при отмене контекста.

---

### Когда использовать?

- **Горутины** — для параллельного выполнения задач.
- **Каналы** — для обмена данными и синхронизации.
- **Мьютексы** — для защиты общих ресурсов.
- **WaitGroup** — для ожидания завершения группы горутин.
- **Контекст** — для управления временем жизни горутин и передачи сигналов отмены.

[[Пример параллелизма go]]
