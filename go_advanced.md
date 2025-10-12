# 🚀 Go Advanced Cheatsheet — Продвинутые возможности

------------------------------------------------------------
## 🌐 Веб-сервер (net/http)

Go имеет встроенный HTTP-сервер, без сторонних библиотек.

```go
package main

import (
    "fmt"
    "net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Привет, %s!", r.URL.Path[1:])
}

func main() {
    http.HandleFunc("/", handler)
    fmt.Println("Сервер запущен на :8080")
    http.ListenAndServe(":8080", nil)
}
```

------------------------------------------------------------
Работа с маршрутизацией:

```go
func hello(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(w, "Hello!")
}

func main() {
    mux := http.NewServeMux()
    mux.HandleFunc("/hello", hello)
    http.ListenAndServe(":8080", mux)
}
```

------------------------------------------------------------
JSON API:

```go
import (
    "encoding/json"
)

type User struct {
    Name string `json:"name"`
    Age  int    `json:"age"`
}

func userHandler(w http.ResponseWriter, r *http.Request) {
    u := User{Name: "Tom", Age: 25}
    json.NewEncoder(w).Encode(u)
}
```

------------------------------------------------------------
## 🧪 Тестирование (testing)

Go имеет встроенную систему тестирования.

Файл должен называться имя_test.go

```go
package main

import "testing"

func Add(a, b int) int {
    return a + b
}

func TestAdd(t *testing.T) {
    got := Add(2, 3)
    want := 5

    if got != want {
        t.Errorf("Ожидалось %d, получено %d", want, got)
    }
}
```

Запуск тестов:

```sh
go test ./...
```

------------------------------------------------------------
Бенчмарки:

```go
func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Add(1, 2)
    }
}
```

------------------------------------------------------------
## 🔄 Дженерики (Generics)

Go с версии 1.18 поддерживает обобщённые типы.

```go
func Max[T comparable](a, b T) T {
    if a > b {
        return a
    }
    return b
}

fmt.Println(Max(10, 5))
fmt.Println(Max("go", "lang"))
```
Обобщённые структуры:

```go
type Pair[T any] struct {
    First  T
    Second T
}

p := Pair[int]{First: 1, Second: 2}
```

------------------------------------------------------------
## ⚙️ Каналы и select

```go
ch1 := make(chan string)
ch2 := make(chan string)

go func() {
    ch1 <- "Сообщение из канала 1"
}()
go func() {
    ch2 <- "Сообщение из канала 2"
}()

select {
case msg1 := <-ch1:
    fmt.Println(msg1)
case msg2 := <-ch2:
    fmt.Println(msg2)
default:
    fmt.Println("Нет данных")
}
```

------------------------------------------------------------
## 🧵 Sync: WaitGroup и Mutex

WaitGroup — ожидание горутин:

```go
import "sync"

var wg sync.WaitGroup

func worker(id int) {
    defer wg.Done()
    fmt.Println("Работает горутина", id)
}

func main() {
    for i := 1; i <= 3; i++ {
        wg.Add(1)
        go worker(i)
    }
    wg.Wait()
}

```
Mutex — защита общих данных:

```go
var mu sync.Mutex
count := 0

func increment() {
    mu.Lock()
    count++
    mu.Unlock()
}
```

------------------------------------------------------------
## ⏱️ Context — управление временем и отменой

```go
import (
    "context"
    "time"
)

func main() {
    ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
    defer cancel()

    go func() {
        <-ctx.Done()
        fmt.Println("Контекст отменён:", ctx.Err())
    }()

    time.Sleep(3 * time.Second)
}
```

------------------------------------------------------------
## 🪞 Reflection (reflect)

Позволяет исследовать и изменять типы во время выполнения.

```go
import (
    "fmt"
    "reflect"
)

type Person struct {
    Name string
}

func main() {
    p := Person{"Tom"}
    t := reflect.TypeOf(p)
    v := reflect.ValueOf(p)

    fmt.Println("Тип:", t.Name())
    fmt.Println("Полей:", t.NumField())
    fmt.Println("Значение Name:", v.Field(0))
}
```

------------------------------------------------------------
## 📦 Встраивание ресурсов (embed)

Go позволяет встраивать файлы прямо в бинарник.

```go
import _ "embed"

//go:embed hello.txt
var message string

func main() {
    fmt.Println(message)
}
```

Файл hello.txt будет встроен в исполняемый файл.

------------------------------------------------------------
## 🧭 Профилирование и отладка

Профилирование CPU и памяти:

```go
import (
    "os"
    "runtime/pprof"
)

func main() {
    f, _ := os.Create("cpu.prof")
    pprof.StartCPUProfile(f)
    defer pprof.StopCPUProfile()
}
```

Встроенный pprof-сервер:

```go
import _ "net/http/pprof"
import "net/http"

func main() {
    go func() {
        http.ListenAndServe(":6060", nil)
    }()
    select {}
}
```

http://localhost:6060/debug/pprof/

------------------------------------------------------------
## 🧰 Полезные встроенные пакеты

fmt — вывод \
strings — работа со строками \
strconv — конвертация типов \
math — математика \
time — работа со временем \
os / io — файлы и потоки \
encoding/json — JSON \
sync / context — конкурентность \
testing — тестирование \
net/http — веб-сервер \
runtime / pprof — системная информация и профилирование

------------------------------------------------------------
## 🧾 Пример: Мини веб-сервис с тестом и горутинами

```go
package main

import (
    "encoding/json"
    "fmt"
    "net/http"
    "sync"
)

type Message struct {
    Text string `json:"text"`
}

var (
    msgs []Message
    mu   sync.Mutex
)

func handler(w http.ResponseWriter, r *http.Request) {
    if r.Method == "POST" {
        var m Message
        json.NewDecoder(r.Body).Decode(&m)
        mu.Lock()
        msgs = append(msgs, m)
        mu.Unlock()
        w.WriteHeader(http.StatusCreated)
        return
    }

    mu.Lock()
    json.NewEncoder(w).Encode(msgs)
    mu.Unlock()
}

func main() {
    http.HandleFunc("/messages", handler)
    fmt.Println("Server running on :8080")
    http.ListenAndServe(":8080", nil)
}
```
