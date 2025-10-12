# 🐹 Go Cheatsheet — Синтаксис и конструкции

## 📦 Основы

package main

import "fmt"

func main() {
    fmt.Println("Hello, Go!")
}

- package main — объявляет главный пакет (точка входа)
- import — подключает пакеты
- func main() — точка входа программы

------------------------------------------------------------

## 🧩 Переменные и константы

var x int = 10
y := 20          // короткое объявление
const Pi = 3.14

- var — объявление переменной
- := — короткая форма (только внутри функций)
- const — константа (нельзя изменять)

------------------------------------------------------------

## 🔢 Типы данных

int, float64, string, bool

Примеры:
x := 42
pi := 3.14
s := "go"
ok := true

------------------------------------------------------------

## 🔄 Условия

if x > 0 {
    fmt.Println("Positive")
} else if x < 0 {
    fmt.Println("Negative")
} else {
    fmt.Println("Zero")
}

------------------------------------------------------------

## 🔁 Циклы

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

------------------------------------------------------------

## 🔀 Switch

day := "Mon"

switch day {
case "Mon":
    fmt.Println("Start of week")
case "Fri":
    fmt.Println("End of week")
default:
    fmt.Println("Midweek")
}

------------------------------------------------------------

## 🧮 Массивы и срезы

var arr [3]int = [3]int{1, 2, 3}
slice := []int{1, 2, 3}
slice = append(slice, 4)
fmt.Println(slice[1:3]) // от 1 до 2 включительно

append() — добавляет элементы
len(slice) — длина
cap(slice) — ёмкость

------------------------------------------------------------

## 🗺️ Map (словари)

m := map[string]int{"a": 1, "b": 2}
m["c"] = 3
delete(m, "b")

for key, value := range m {
    fmt.Println(key, value)
}

------------------------------------------------------------

## 🧰 Функции

func add(a int, b int) int {
    return a + b
}

res := add(2, 3)

Множественное возвращение:

func swap(x, y string) (string, string) {
    return y, x
}

a, b := swap("hi", "go")

------------------------------------------------------------

## 📍 Указатели

x := 10
p := &x        // указатель на x
fmt.Println(*p) // разыменование

------------------------------------------------------------

## 🧱 Структуры

type Person struct {
    Name string
    Age  int
}

p1 := Person{"Tom", 30}
fmt.Println(p1.Name)

------------------------------------------------------------

## 🧩 Методы

type Person struct {
    Name string
}

func (p Person) Greet() {
    fmt.Println("Hi,", p.Name)
}

p := Person{"Tom"}
p.Greet()

------------------------------------------------------------

## 🧠 Интерфейсы

type Speaker interface {
    Speak()
}

type Dog struct{}

func (d Dog) Speak() {
    fmt.Println("Woof!")
}

var s Speaker = Dog{}
s.Speak()

------------------------------------------------------------

## ⚙️ Goroutines и каналы (основы)

go fmt.Println("Hello from goroutine")

ch := make(chan int)

go func() {
    ch <- 42
}()

val := <-ch
fmt.Println(val)

go — запускает горутину (новый поток)
chan — канал для передачи данных между потоками

------------------------------------------------------------

## ⚠️ Обработка ошибок

result, err := someFunc()
if err != nil {
    fmt.Println("Error:", err)
}

------------------------------------------------------------

## 🧩 Defer / Panic / Recover

defer fmt.Println("Это выполнится в конце")

func demo() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Перехвачена паника:", r)
        }
    }()
    panic("Ошибка!")
}

------------------------------------------------------------

## 🧾 Пример минимальной программы

package main

import "fmt"

func add(a, b int) int {
    return a + b
}

func main() {
    sum := add(5, 7)
    fmt.Println("Сумма =", sum)
}
