# 🦀 Rust Cheatsheet — Основы языка

------------------------------------------------------------
## 🏁 Привет, Rust!

```rust
fn main() {
    println!("Hello, Rust!");
}
```

------------------------------------------------------------
## 📦 Компиляция и запуск

```sh
# Создание проекта
cargo new hello_rust
cd hello_rust

# Сборка
cargo build

# Запуск
cargo run

# Тесты
cargo test
```

------------------------------------------------------------
## 🔢 Переменные и типы

```rust
fn main() {
    let x = 5;              // неизменяемая переменная
    let mut y = 10;         // изменяемая переменная
    y = y + 1;

    const PI: f64 = 3.1415; // константа (только верхний регистр)
    println!("x = {}, y = {}, π = {}", x, y, PI);
}
```

------------------------------------------------------------
## 🧮 Основные типы данных

```rust
// Числа
let a: i32 = 10;
let b: u64 = 20;
let c: f32 = 3.14;

// Булевы значения
let t: bool = true;

// Символы и строки
let ch: char = 'R';
let s: &str = "Hello";      // строковый срез
let mut string = String::from("Rust");
string.push_str(" Lang!");
```

------------------------------------------------------------
## 🔁 Управляющие конструкции

```rust
let n = 3;

if n > 0 {
    println!("Положительное");
} else if n < 0 {
    println!("Отрицательное");
} else {
    println!("Ноль");
}

let res = if n % 2 == 0 { "чётное" } else { "нечётное" };
```

------------------------------------------------------------
## 🔄 Циклы

```rust
// loop — бесконечный цикл
let mut count = 0;
loop {
    if count == 3 { break; }
    println!("count = {}", count);
    count += 1;
}

// while
let mut i = 0;
while i < 3 {
    println!("i = {}", i);
    i += 1;
}

// for
for n in 0..5 {
    println!("{}", n);
}
```

------------------------------------------------------------
## 📦 Коллекции

```rust
// Векторы (динамические массивы)
let mut nums = vec![1, 2, 3];
nums.push(4);
println!("{:?}", nums);

// Кортежи
let person = ("Tom", 25);
println!("Имя: {}, Возраст: {}", person.0, person.1);

// Массивы фиксированной длины
let arr = [10, 20, 30];
println!("{}", arr[1]);
```

------------------------------------------------------------
## 🧱 Функции

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b  // без `;` — возвращаемое значение
}

fn main() {
    println!("{}", add(2, 3));
}
```

------------------------------------------------------------
## 🔄 Управление владением (Ownership)

```rust
fn main() {
    let s1 = String::from("Hello");
    let s2 = s1; // s1 перемещён, недействителен

    // println!("{}", s1); // Ошибка!
    println!("{}", s2);
}
```

------------------------------------------------------------
## 📋 Ссылки и заимствование (Borrowing)

```rust
fn main() {
    let s = String::from("Hello");
    let len = length(&s); // передаём ссылку
    println!("'{}' имеет длину {}", s, len);
}

fn length(s: &String) -> usize {
    s.len()
}
```

------------------------------------------------------------
## 🔐 Изменяемые ссылки

```rust
fn main() {
    let mut s = String::from("Hi");
    change(&mut s);
    println!("{}", s);
}

fn change(s: &mut String) {
    s.push_str(", Rust!");
}
```

------------------------------------------------------------
## 🧩 Срезы (Slices)

```rust
let s = String::from("hello world");
let hello = &s[0..5];
let world = &s[6..];
println!("{} {}", hello, world);
```

------------------------------------------------------------
## 🧮 Enum и match

```rust
enum Direction {
    Up,
    Down,
    Left,
    Right,
}

fn move_to(dir: Direction) {
    match dir {
        Direction::Up => println!("Вверх"),
        Direction::Down => println!("Вниз"),
        Direction::Left => println!("Влево"),
        Direction::Right => println!("Вправо"),
    }
}

fn main() {
    move_to(Direction::Left);
}
```

------------------------------------------------------------
## 🧠 Option и Result

```rust
// Option — значение может быть или нет
fn divide(a: f64, b: f64) -> Option<f64> {
    if b == 0.0 { None } else { Some(a / b) }
}

fn main() {
    match divide(4.0, 2.0) {
        Some(v) => println!("Результат: {}", v),
        None => println!("Ошибка деления на ноль"),
    }
}

// Result — успех или ошибка
use std::fs::File;

fn main() {
    let f = File::open("test.txt");
    match f {
        Ok(file) => println!("Файл открыт: {:?}", file),
        Err(e) => println!("Ошибка: {}", e),
    }
}
```

------------------------------------------------------------
## 🧱 Структуры (Structs)

```rust
struct Person {
    name: String,
    age: u8,
}

fn main() {
    let user = Person {
        name: String::from("Tom"),
        age: 25,
    };

    println!("{} — {} лет", user.name, user.age);
}
```

------------------------------------------------------------
## 🧬 Методы и impl

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect = Rectangle { width: 10, height: 5 };
    println!("Площадь: {}", rect.area());
}
```

------------------------------------------------------------
## 🧱 Модули (mod)

```rust
mod math {
    pub fn add(a: i32, b: i32) -> i32 {
        a + b
    }
}

fn main() {
    println!("{}", math::add(2, 3));
}
```

------------------------------------------------------------
## 🧪 Тесты

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_add() {
        assert_eq!(2 + 2, 4);
    }
}
```
