# 🐹 Go Cheatsheet — Синтаксис и конструкции

## 📦 Основы

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go!")
}
```

- package main — объявляет главный пакет (точка входа)
- import — подключает пакеты
- func main() — точка входа программы

------------------------------------------------------------

## 🧩 Переменные и константы

```go
var x int = 10
y := 20          // короткое объявление
const Pi = 3.14
```

- var — объявление переменной
- := — короткая форма (только внутри функций)
- const — константа (нельзя изменять)

------------------------------------------------------------

## 🔢 Типы данных

int, float64, string, bool

Примеры:
```go
x := 42
pi := 3.14
s := "go"
ok := true
```

------------------------------------------------------------

## 🔄 Условия

```go
if x > 0 {
    fmt.Println("Positive")
} else if x < 0 {
    fmt.Println("Negative")
} else {
    fmt.Println("Zero")
}
```

------------------------------------------------------------

## 🔁 Циклы

```go
for i := 0; i < 5; i++ {
    fmt.Println(i)
}

sum := 0
for sum < 10 {
    sum++
}

for { // бесконечный цикл
    break
}
```

------------------------------------------------------------

## 🔀 Switch

```go
day := "Mon"

switch day {
case "Mon":
    fmt.Println("Start of week")
case "Fri":
    fmt.Println("End of week")
default:
    fmt.Println("Midweek")
}
```

------------------------------------------------------------

## 🧮 Массивы и срезы

```go
var arr [3]int = [3]int{1, 2, 3}
slice := []int{1, 2, 3}
slice = append(slice, 4)
fmt.Println(slice[1:3]) // от 1 до 2 включительно
```

append() — добавляет элементы \
len(slice) — длина \
cap(slice) — ёмкость

------------------------------------------------------------

## 🗺️ Map (словари)

```go
m := map[string]int{"a": 1, "b": 2}
m["c"] = 3
delete(m, "b")

for key, value := range m {
    fmt.Println(key, value)
}
```

------------------------------------------------------------

## 🧰 Функции

```go
func add(a int, b int) int {
    return a + b
}

res := add(2, 3)
```

Множественное возвращение:

```go
func swap(x, y string) (string, string) {
    return y, x
}

a, b := swap("hi", "go")
```

------------------------------------------------------------

## 📍 Указатели

```go
x := 10
p := &x        // указатель на x
fmt.Println(*p) // разыменование
```

------------------------------------------------------------

## 🧱 Структуры

```go
type Person struct {
    Name string
    Age  int
}

p1 := Person{"Tom", 30}
fmt.Println(p1.Name)
```

------------------------------------------------------------

## 🧩 Методы

```go
type Person struct {
    Name string
}

func (p Person) Greet() {
    fmt.Println("Hi,", p.Name)
}

p := Person{"Tom"}
p.Greet()
```

------------------------------------------------------------

## 🧠 Интерфейсы

```go
type Speaker interface {
    Speak()
}

type Dog struct{}

func (d Dog) Speak() {
    fmt.Println("Woof!")
}

var s Speaker = Dog{}
s.Speak()
```

------------------------------------------------------------

## ⚙️ Goroutines и каналы (основы)

```go
go fmt.Println("Hello from goroutine")

ch := make(chan int)

go func() {
    ch <- 42
}()

val := <-ch
fmt.Println(val)
```

go — запускает горутину (новый поток) \
chan — канал для передачи данных между потоками

------------------------------------------------------------

## ⚠️ Обработка ошибок

```go
result, err := someFunc()
if err != nil {
    fmt.Println("Error:", err)
}
```

------------------------------------------------------------

## 🧩 Defer / Panic / Recover

```go
defer fmt.Println("Это выполнится в конце")

func demo() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Перехвачена паника:", r)
        }
    }()
    panic("Ошибка!")
}
```

------------------------------------------------------------
## 🧩 Перечисления (Enum через iota)

Go не имеет встроенных enum'ов (как в Rust или Java), \
но аналог реализуется через `const` и счётчик `iota`.

```go
type Direction int

const (
    Up Direction = iota
    Down
    Left
    Right
)

func main() {
    fmt.Println(Up, Down, Left, Right) // 0 1 2 3
}
```

------------------------------------------------------------
## 🏷️ Приведение enum к строкам

```go
type Direction int

const (
    Up Direction = iota
    Down
    Left
    Right
)

func (d Direction) String() string {
    return [...]string{"Up", "Down", "Left", "Right"}[d]
}

func main() {
    fmt.Println(Up)   // Up
    fmt.Println(Down) // Down
}
```

------------------------------------------------------------
## 🧰 Enum из строк (удобно для JSON и API)

```go
type Status string

const (
    StatusOK    Status = "ok"
    StatusError Status = "error"
)

func main() {
    fmt.Println(StatusOK)
}
```

------------------------------------------------------------
## ⚙️ Автоматическая генерация String() методом stringer

```sh
go install golang.org/x/tools/cmd/stringer@latest
```

```go
 //go:generate stringer -type=Direction
 type Direction int

 const (
     Up Direction = iota
     Down
     Left
     Right
 )
```

```sh
# Затем:
go generate

# Создаст direction_string.go с реализацией:
# func (Direction) String() string { ... }
```

------------------------------------------------------------

## 🧾 Пример минимальной программы

```go
package main

import "fmt"

func add(a, b int) int {
    return a + b
}

func main() {
    sum := add(5, 7)
    fmt.Println("Сумма =", sum)
}
```
