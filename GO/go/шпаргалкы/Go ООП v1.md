Отлично, Alfa Phoenix! Давай рассмотрим, как можно реализовать основные принципы ООП (инкапсуляция, наследование, полиморфизм) в Go на примере управления станками — это как раз актуально для твоего проекта по созданию самодельного парка станков.

---

## 1. **Инкапсуляция**

**Суть:** Сокрытие внутреннего состояния объекта и предоставление доступа к нему только через публичные методы. Это защищает данные от неправильного использования и позволяет контролировать доступ.

В Go инкапсуляция достигается за счёт использования **неэкспортируемых полей и методов** (начинающихся с маленькой буквы). Экспортируемые (с большой буквы) доступны из других пакетов.

**Пример: Станок с скрытыми деталями реализации**

```go
package main 
import "fmt" // Machine — интерфейс для всех станков 
type Machine interface {     
	Start() error    
	Stop() error    
	GetStatus() string 
} // drillPress — структура для сверлильного станка (инкапсуляция) 
type drillPress struct {     
	isRunning bool    
	rpm       int 
} 
func (d *drillPress) Start() error {     
	d.isRunning = true    
	d.rpm = 1500    
	return nil 
} 
func (d *drillPress) Stop() error {     
	d.isRunning = false    
	d.rpm = 0    
	return nil 
} 
func (d *drillPress) GetStatus() string {     
	return fmt.Sprintf("Сверлильный станок: работает=%t, RPM=%d", d.isRunning, d.rpm) 
} 
func main() {     
	var m Machine = &drillPress{}    
	m.Start()    
	fmt.Println(m.GetStatus())    
	m.Stop() 
}
```

**Что здесь:**

- Поля `isRunning` и `rpm` — неэкспортируемые (инкапсулированы).
- Методы `Start`, `Stop`, `GetStatus` — экспортируемые (доступны через интерфейс).

---

## 2. **"Наследование" через композицию и встраивание**

В Go нет классического наследования, но есть **композиция** и **встраивание** (embedding). Это позволяет "расширять" поведение структур.

**Пример: Фрезерный станок на базе общего станка**

```go
// baseMachine — базовая структура для всех станков
type baseMachine struct {
    powerSupply bool
}

func (b *baseMachine) TurnOn() {
    b.powerSupply = true
}

func (b *baseMachine) TurnOff() {
    b.powerSupply = false
}

// millingMachine — фрезерный станок (расширяет baseMachine)
type millingMachine struct {
    baseMachine // Встраивание
    spindleSpeed int
}

func (m *millingMachine) Start() error {
    m.TurnOn() // Вызов метода базовой структуры
    m.spindleSpeed = 2000
    return nil
}

func (m *millingMachine) GetStatus() string {
    return fmt.Sprintf("Фрезерный станок: питание=%t, RPM=%d", m.powerSupply, m.spindleSpeed)
}

func main() {
    mm := millingMachine{}
    mm.Start()
    fmt.Println(mm.GetStatus())
}
```

**Что здесь:**

- `millingMachine` встраивает `baseMachine` и наследует его методы (`TurnOn`, `TurnOff`).
- Можно добавлять новые методы или переопределять поведение.

---

## 3. **Полиморфизм через интерфейсы**

Полиморфизм в Go реализуется через **интерфейсы**. Один и тот же интерфейс может быть реализован разными структурами, и их методы будут вызываться динамически.

**Пример: Управление разными станками через один интерфейс**

```go
type Machine interface {
    Start() error
    GetStatus() string
}

type lathe struct {
    isRunning bool
}

func (l *lathe) Start() error {
    l.isRunning = true
    return nil
}

func (l *lathe) GetStatus() string {
    return fmt.Sprintf("Токарный станок: работает=%t", l.isRunning)
}

func operateMachine(m Machine) {
    m.Start()
    fmt.Println(m.GetStatus())
}

func main() {
    machines := []Machine{&drillPress{}, &millingMachine{}, &lathe{}}
    for _, m := range machines {
        operateMachine(m) // Вызовет нужный метод в зависимости от типа
    }
}
```

**Что здесь:**

- Функция `operateMachine` работает с любым типом, реализующим интерфейс `Machine`.
- В runtime вызывается правильная реализация метода (`GetStatus`).

---

## 4. **Практическое применение для твоего проекта**

- **Инкапсуляция**: Скрывай детали реализации станков (например, параметры двигателей, датчиков).
- **Композиция**: Создавай базовые структуры для общих функций (например, управление питанием, аварийная остановка).
- **Интерфейсы**: Используй для управления разными станками через единый API (например, для ЧПУ, автоматизации).

---

## 5. **Пример полной реализации**

```go
package main

import "fmt"

type Machine interface {
    Start() error
    Stop() error
    GetStatus() string
}

type baseMachine struct {
    powerSupply bool
}

func (b *baseMachine) TurnOn() {
    b.powerSupply = true
}

func (b *baseMachine) TurnOff() {
    b.powerSupply = false
}

type DrillPress struct {
    baseMachine
    rpm int
}

func (d *DrillPress) Start() error {
    d.TurnOn()
    d.rpm = 1500
    return nil
}

func (d *DrillPress) Stop() error {
    d.TurnOff()
    d.rpm = 0
    return nil
}

func (d *DrillPress) GetStatus() string {
    return fmt.Sprintf("Сверлильный станок: питание=%t, RPM=%d", d.powerSupply, d.rpm)
}

type MillingMachine struct {
    baseMachine
    spindleSpeed int
}

func (m *MillingMachine) Start() error {
    m.TurnOn()
    m.spindleSpeed = 2000
    return nil
}

func (m *MillingMachine) Stop() error {
    m.TurnOff()
    m.spindleSpeed = 0
    return nil
}

func (m *MillingMachine) GetStatus() string {
    return fmt.Sprintf("Фрезерный станок: питание=%t, RPM=%d", m.powerSupply, m.spindleSpeed)
}

func main() {
    machines := []Machine{&DrillPress{}, &MillingMachine{}}
    for _, m := range machines {
        m.Start()
        fmt.Println(m.GetStatus())
        m.Stop()
    }
}

```

---
