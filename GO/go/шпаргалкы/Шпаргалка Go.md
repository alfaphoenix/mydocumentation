
# 🧠 Go (Golang) — шпаргалка

> Краткая, практичная шпаргалка по языку Go для ежедневной работы и обучения. Формат подходит для **Obsidian**.

---

## 📦 Структура программы

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go!")
}
```

- `package main` — точка входа
    
- `func main()` — старт программы

---

## 🔤 Переменные

```go
var a int = 10
var b = 20
c := 30 // короткая форма (только внутри функций)
```

### Группировка

```go
var (
    x int
    y string
)
```

---

## 🔢 Базовые типы

- `int`, `int64`, `uint`
    
- `float32`, `float64`
    
- `string`
    
- `bool`
    

```go
var ok bool = true
```

---

## 📐 Константы

```go
const Pi = 3.14
```

```go
const (
    A = 1
    B = 2
)
```

---

## 🔀 Условия

```go
if x > 10 {
    fmt.Println("big")
} else {
    fmt.Println("small")
}
```

```go
if n := 10; n > 5 {
    fmt.Println(n)
}
```

---

## 🔁 Циклы

### for — единственный цикл в Go

```go
for i := 0; i < 5; i++ {
    fmt.Println(i)
}
```

```go
for x < 10 {
    x++
}
```

```go
for {
    break // бесконечный цикл
}
```

---

## 🔧 Функции

```go
func add(a int, b int) int {
    return a + b
}
```

### Несколько возвращаемых значений

```go
func div(a, b int) (int, int) {
    return a / b, a % b
}
```

---

## ⚠️ Ошибки

```go
func read() (string, error) {
    if fail {
        return "", errors.New("fail")
    }
    return "ok", nil
}
```

```go
if err != nil {
    log.Fatal(err)
}
```

---

## 🧱 Структуры

```go
type User struct {
    Name string
    Age  int
}
```

```go
u := User{Name: "Alfa", Age: 25}
```

---

## 🧩 Методы

```go
func (u User) Greet() string {
    return "Hi, " + u.Name
}
```

```go
func (u *User) Birthday() {
    u.Age++
}
```

---

## 🔌 Интерфейсы

```go
type Speaker interface {
    Speak() string
}
```

```go
func Say(s Speaker) {
    fmt.Println(s.Speak())
}
```

> В Go **не нужно явно реализовывать интерфейс** — достаточно методов.

---

## 📚 Массивы и срезы

### Массив

```go
var a [3]int
```

### Срез

```go
s := []int{1, 2, 3}
s = append(s, 4)
```

```go
sub := s[1:3]
```

---

## 🗺️ Map

```go
m := map[string]int{
    "one": 1,
}
```

```go
v, ok := m["one"]
```

```go
delete(m, "one")
```

---

## 📦 Пакеты и экспорт

- **Публичное** — имя с заглавной буквы
    
- **Приватное** — с маленькой
    

```go
func PublicFunc() {}
func privateFunc() {}
```

---

## 📌 Указатели

```go
x := 10
p := &x
*p = 20
```

---

## 🔄 defer

```go
defer fmt.Println("end")
```

> Выполняется **перед выходом из функции**

---

## 🚀 Go routines

```go
go doWork()
```

---

## 📡 Каналы

```go
ch := make(chan int)
```

```go
go func() {
    ch <- 10
}()

v := <-ch
```

---

## 🧰 select

```go
select {
case v := <-ch:
    fmt.Println(v)
default:
    fmt.Println("no data")
}
```

---

## 🧪 Тесты

```go
func TestAdd(t *testing.T) {
    if add(2, 2) != 4 {
        t.Fail()
    }
}
```

---

## 🛠️ Полезные команды

```bash
go run main.go
go build
go test ./...
go mod init example
go mod tidy
```

---

## 📎 Полезные заметки

- Нет классов ❌ → есть **структуры + методы**
    
- Нет исключений ❌ → есть `error`
    
- Интерфейсы — основа архитектуры
    
- Простота важнее магии ✨