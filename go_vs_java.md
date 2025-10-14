# <img src="https://go.dev/blog/go-brand/Go-Logo/PNG/Go-Logo_Blue.png" alt="Go logo" width="40"/> Gotchas & Best Practices
*(для Java-разработчика, переходящего на Go)*

---

## ⚙️ Основные отличия от Java

| 📘 Java | 🐹 Go |
|----------|--------|
| Всё — классы и объекты | Только структуры и функции |
| Есть исключения (try/catch) | Ошибки — обычные значения (`error`) |
| Есть перегрузка методов и операторов | Нет перегрузки, всё должно быть явно |
| Многопоточность через `Thread`, `Executor` | Параллелизм через `goroutine` + `channel` |
| Автоматическое ООП | Композиция вместо наследования |
| Null reference | Zero values (`0`, `""`, `nil`, `false`) |

---

## 🧩 1. Zero Values
Все переменные **всегда имеют значение по умолчанию**:

```go
var i int       // 0
var s string    // ""
var b bool      // false
var m map[string]int // nil
```

⚠️ Если `map` или `slice` равен `nil`, перед добавлением нужно `make()`.

---

## 🔧 2. make() vs new()

| Функция | Что делает | Где использовать |
|----------|-------------|------------------|
| `make()` | создаёт slice, map, channel | для коллекций |
| `new()`  | выделяет память под тип и возвращает указатель | для структур и простых типов |

```go
s := make([]int, 0, 10)
p := new(int)
*p = 42
```

---

## 🧮 3. Срезы (Slices)
```go
s := make([]int, 0, 5)
s = append(s, 1, 2, 3)
```

- `len` — длина, `cap` — ёмкость.  
- `append` возвращает **новый** срез.  
- Никаких `add()` — всё через `append()`.

💡 Лучше `make([]int, 0, N)` вместо `make([]int, N)` если будешь аппендить.

---

## 🚦 4. Управление ошибками

Нет исключений — ошибки это значения.

```go
data, err := os.ReadFile("file.txt")
if err != nil {
    log.Fatal(err)
}
fmt.Println(string(data))
```

- Всегда проверяй `err`.  
- Для сложных случаев можно использовать `errors.Is` / `errors.As`.  

---

## ⚙️ 5. Defer, Panic, Recover

- `defer` выполняется **в обратном порядке (LIFO)** при выходе из функции.
- `panic` = исключение, но не стоит злоупотреблять.
- `recover()` можно вызвать, чтобы поймать `panic`.

```go
defer fmt.Println("End")
defer fmt.Println("Cleanup")
fmt.Println("Start")
// Выведет: Start → Cleanup → End
```

---

## 💬 6. Структуры и методы

```go
type User struct {
  Name string
  Age  int
}

func (u *User) Greet() {
  fmt.Println("Hello,", u.Name)
}
```

- Методы объявляются отдельно, не внутри struct.
- Передача по значению (`u User`) создаёт копию.
- Передача по указателю (`*User`) изменяет оригинал.

---

## ⚖️ 7. Интерфейсы

Интерфейсы реализуются **неявно**.

```go
type Greeter interface {
  Greet()
}

type Person struct{ Name string }

func (p Person) Greet() {
  fmt.Println("Hi,", p.Name)
}

func greetAll(g Greeter) {
  g.Greet()
}
```

---

## 🔄 8. Goroutines и Channels

```go
go func() {
  fmt.Println("async!")
}()

ch := make(chan int)
go func() { ch <- 42 }()
fmt.Println(<-ch)
```

- `go` запускает функцию в отдельной горутине.  
- `chan` — потокобезопасный обмен данными.  
- Не блокируй канал без `go`.  

---

## 🧠 9. Context

Для отмены и таймаутов:

```go
ctx, cancel := context.WithTimeout(context.Background(), time.Second)
defer cancel()

select {
case <-time.After(2 * time.Second):
  fmt.Println("timeout!")
case <-ctx.Done():
  fmt.Println("cancelled:", ctx.Err())
}
```

---

## 🪣 10. Maps

```go
m := make(map[string]int)
m["a"] = 1
val, ok := m["b"] // ok = false если ключа нет
```

- Нет `containsKey` — только проверка `ok`.  
- `nil` map нельзя изменять, только читать.  

---

## 🧰 11. Тесты

```go
func TestAdd(t *testing.T) {
  got := Add(2, 2)
  want := 4
  if got != want {
    t.Errorf("got %v want %v", got, want)
  }
}
```

Запуск:  
```
go test ./...
```

---

## 🚀 12. Мелочи, которые часто путают

- `:=` — короткое объявление **только внутри функций**.  
- `for` — единственный цикл, `while` нет.  
- `range` возвращает индекс и значение.  
- `_` — игнор переменной.  
- Имена с заглавной буквы = экспортируемые.  
- Порядок вызова `defer` — **в обратном порядке**.  
- `init()` вызывается перед `main()`.

---

---

🐹 Go — это про *простоту и контроль*, а не про "удобство любой ценой".  
Если что-то кажется "слишком примитивным" — вероятно, так и задумано 😉
