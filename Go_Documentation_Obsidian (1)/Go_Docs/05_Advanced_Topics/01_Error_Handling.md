---
tags: [golang, errors, panic, recover]
aliases: [Go Обработка Ошибок, Go Panic Recover]
---

# ⚠️ Обработка Ошибок

## 1. Идиоматическая Обработка Ошибок

В Go ошибки — это значения. Они возвращаются как последнее возвращаемое значение функции.

```go
func Sqrt(x float64) (float64, error) {
	if x < 0 {
		// errors.New создает новую ошибку
		return 0, errors.New("math: square root of negative number")
	}
	return math.Sqrt(x), nil
}

func main() {
	result, err := Sqrt(-1)
	
	// Идиоматическая проверка ошибки
	if err != nil {
		log.Fatal(err) // Выход с ошибкой
	}
	fmt.Println(result)
}
```

## 2. Пользовательские Ошибки

Для создания пользовательских ошибок необходимо реализовать интерфейс `error`.

```go
type MyError struct {
	When time.Time
	What string
}

// Реализация интерфейса error
func (e *MyError) Error() string {
	return fmt.Sprintf("at %v, %s", e.When, e.What)
}

func run() error {
	return &MyError{
		time.Now(),
		"it didn't work",
	}
}
```

## 3. `panic` и `recover`

### `panic`

`panic` вызывает немедленное завершение программы. Обычно используется для невосстановимых ошибок (например, нарушение безопасности, выход за пределы массива).

### `recover`

`recover` используется внутри `defer` для перехвата `panic` и возобновления нормального выполнения.

```go
func mayPanic() {
	panic("a problem")
}

func main() {
	// defer с recover должен быть объявлен до вызова mayPanic
	defer func() {
		if r := recover(); r != nil {
			// r содержит значение, переданное в panic
			fmt.Println("Recovered. Error:", r)
		}
	}()

	mayPanic()
	fmt.Println("After panic (will not be reached)")
}
```

## 4. Обертывание Ошибок (Error Wrapping)

Go 1.13+ представил обертывание ошибок, позволяющее сохранять цепочку ошибок.

*   `fmt.Errorf("context: %w", err)`: Обертывает ошибку.
*   `errors.Is(err, target)`: Проверяет, является ли ошибка `err` или любая ошибка в ее цепочке ошибкой `target`.
*   `errors.As(err, &target)`: Находит первую ошибку в цепочке, которая соответствует типу `target`, и устанавливает `target` на это значение.
