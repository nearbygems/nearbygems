# 🦀 Rust Advanced Cheatsheet — Продвинутые возможности

------------------------------------------------------------
## 🧩 Traits (интерфейсы)

```rust
trait Greet {
    fn greet(&self);
}

struct Person {
    name: String,
}

impl Greet for Person {
    fn greet(&self) {
        println!("Привет, {}!", self.name);
    }
}

fn main() {
    let p = Person { name: String::from("Tom") };
    p.greet();
}
```

------------------------------------------------------------
## 🔧 Дженерики (Generics)

```rust
fn largest<T: PartialOrd + Copy>(list: &[T]) -> T {
    let mut max = list[0];
    for &item in list.iter() {
        if item > max {
            max = item;
        }
    }
    max
}

fn main() {
    let nums = vec![10, 20, 5];
    println!("Максимум: {}", largest(&nums));
}
```

------------------------------------------------------------
## ⚙️ Trait Bounds и where

```rust
fn print_sum<T>(a: T, b: T)
where
    T: std::ops::Add<Output = T> + std::fmt::Display,
{
    println!("{} + {} = {}", a, b, a + b);
}

fn main() {
    print_sum(2, 3);
}
```

------------------------------------------------------------
## 🧬 Lifetimes (времена жизни ссылок)

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}

fn main() {
    let s1 = String::from("Rust");
    let s2 = String::from("Language");
    let res = longest(&s1, &s2);
    println!("Самая длинная: {}", res);
}
```

------------------------------------------------------------
## 🧱 Associated Functions и Static Methods

```rust
struct Circle {
    radius: f64,
}

impl Circle {
    fn new(r: f64) -> Self {
        Self { radius: r }
    }

    fn area(&self) -> f64 {
        3.14 * self.radius * self.radius
    }
}

fn main() {
    let c = Circle::new(3.0);
    println!("Площадь круга: {}", c.area());
}
```

------------------------------------------------------------
## 🔄 Pattern Matching (шаблонное сопоставление)

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
}

fn process(msg: Message) {
    match msg {
        Message::Quit => println!("Выход"),
        Message::Move { x, y } => println!("Перемещение: {}, {}", x, y),
        Message::Write(text) => println!("Текст: {}", text),
    }
}

fn main() {
    process(Message::Write(String::from("Привет")));
}
```

------------------------------------------------------------
## 🧱 Option и Result (продвинуто)

```rust
fn divide(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 {
        Err(String::from("Деление на ноль"))
    } else {
        Ok(a / b)
    }
}

fn main() {
    match divide(10.0, 0.0) {
        Ok(v) => println!("Результат: {}", v),
        Err(e) => println!("Ошибка: {}", e),
    }
}
```

------------------------------------------------------------
## 🧠 Smart Pointers (Box, Rc, Arc)

```rust
use std::rc::Rc;
use std::sync::{Arc, Mutex};
use std::thread;

// Box — хранит данные в куче
let b = Box::new(5);
println!("Box: {}", b);

// Rc — подсчёт ссылок (для однопоточной среды)
let a = Rc::new(String::from("data"));
let b = Rc::clone(&a);
println!("Количество ссылок: {}", Rc::strong_count(&a));

// Arc + Mutex — потокобезопасное разделение данных
let counter = Arc::new(Mutex::new(0));
let mut handles = vec![];

for _ in 0..5 {
    let c = Arc::clone(&counter);
    let h = thread::spawn(move || {
        let mut num = c.lock().unwrap();
        *num += 1;
    });
    handles.push(h);
}

for h in handles {
    h.join().unwrap();
}
println!("Счётчик: {}", *counter.lock().unwrap());
```

------------------------------------------------------------
## ⚡ async / await (асинхронность)

```rust
use std::time::Duration;
use tokio::time::sleep;

async fn say_hello() {
    sleep(Duration::from_secs(1)).await;
    println!("Привет из async!");
}

#[tokio::main]
async fn main() {
    let f1 = say_hello();
    let f2 = say_hello();
    futures::join!(f1, f2);
}
```

------------------------------------------------------------
## 🧵 Threads (потоки)

```rust
use std::thread;
use std::time::Duration;

fn main() {
    let handle = thread::spawn(|| {
        for i in 1..=3 {
            println!("Поток: {}", i);
            thread::sleep(Duration::from_millis(500));
        }
    });

    handle.join().unwrap();
}
```

------------------------------------------------------------
## 🧩 Макросы

```rust
macro_rules! add {
    ($a:expr, $b:expr) => {
        println!("{} + {} = {}", $a, $b, $a + $b);
    };
}

fn main() {
    add!(2, 3);
}
```

------------------------------------------------------------
## ⚙️ Итераторы и замыкания (Closures)

```rust
let nums = vec![1, 2, 3];
let squares: Vec<_> = nums.iter().map(|x| x * x).collect();
println!("{:?}", squares);

let sum = |a, b| a + b;
println!("{}", sum(2, 5));
```

------------------------------------------------------------
## 🧰 Traits и Default / Debug / Clone

```rust
#[derive(Debug, Clone, Default)]
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let p1 = Point { x: 5, y: 10 };
    let p2 = p1.clone();
    println!("{:?}", p2);
}
```

------------------------------------------------------------
## 🔐 Unsafe (небезопасный код)

```rust
unsafe fn dangerous() {
    println!("Небезопасная функция!");
}

fn main() {
    unsafe {
        dangerous();
    }
}
```

------------------------------------------------------------
## 🧪 Модули и организация кода

```rust
// src/main.rs
mod utils;

fn main() {
    utils::hello();
}

// src/utils.rs
pub fn hello() {
    println!("Из модуля!");
}
```

------------------------------------------------------------
## 🧱 Тестирование

```rust
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() {
        assert_eq!(2 + 2, 4);
    }

    #[test]
    #[should_panic]
    fn fails() {
        panic!("Ошибка!");
    }
}
```

------------------------------------------------------------
## 🧮 Error Handling: ? оператор

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_file() -> io::Result<String> {
    let mut file = File::open("data.txt")?;
    let mut content = String::new();
    file.read_to_string(&mut content)?;
    Ok(content)
}

fn main() {
    match read_file() {
        Ok(c) => println!("Файл: {}", c),
        Err(e) => println!("Ошибка: {}", e),
    }
}
```

------------------------------------------------------------
## 🧭 Профилирование и логирование

```rust
// Cargo.toml
// [dependencies]
// log = "0.4"
// env_logger = "0.10"

use log::{info, warn};

fn main() {
    env_logger::init();
    info!("Программа запущена");
    warn!("Это предупреждение");
}
```
