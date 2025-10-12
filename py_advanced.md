# 🐍 Python Advanced Cheatsheet — Продвинутые возможности

------------------------------------------------------------
## ⚙️ Декораторы (Decorators)

```python
# Декораторы — это функции, которые модифицируют поведение других функций.

def logger(func):
    def wrapper(*args, **kwargs):
        print(f"Вызов функции {func.__name__} с аргументами {args} {kwargs}")
        result = func(*args, **kwargs)
        print(f"Результат: {result}")
        return result
    return wrapper

@logger
def add(a, b):
    return a + b

add(2, 3)
```

------------------------------------------------------------
## ⏩ Генераторы (Generators)

```python
# Генераторы создают значения по одному и "запоминают" состояние между вызовами.

def countdown(n):
    while n > 0:
        yield n
        n -= 1

for num in countdown(3):
    print(num)

# Использование next()
gen = countdown(2)
print(next(gen))  # 2
print(next(gen))  # 1
```

------------------------------------------------------------
## 🔄 Итераторы (Iterators)

```python
class Counter:
    def __init__(self, start, end):
        self.current = start
        self.end = end

    def __iter__(self):
        return self

    def __next__(self):
        if self.current <= self.end:
            x = self.current
            self.current += 1
            return x
        raise StopIteration

for num in Counter(1, 3):
    print(num)
```
------------------------------------------------------------
## ⚡ Асинхронность (async / await)

```python
import asyncio

async def greet(name):
    await asyncio.sleep(1)
    print(f"Привет, {name}")

async def main():
    await asyncio.gather(
        greet("Tom"),
        greet("Alice"),
    )

asyncio.run(main())
```

------------------------------------------------------------
## 🧠 Контекстные менеджеры (with)

```python
# Управление ресурсами (файлы, соединения) через протокол __enter__ / __exit__.

class ManagedFile:
    def __init__(self, name):
        self.name = name

    def __enter__(self):
        self.file = open(self.name, "w")
        return self.file

    def __exit__(self, exc_type, exc_val, exc_tb):
        self.file.close()

with ManagedFile("hello.txt") as f:
    f.write("Привет!")
```

------------------------------------------------------------
## 🧩 Typing — аннотации типов

```python
from typing import List, Dict, Tuple, Optional, Union

def add(a: int, b: int) -> int:
    return a + b

def process(data: List[int]) -> Dict[str, int]:
    return {"sum": sum(data)}

value: Optional[str] = None
num: Union[int, float] = 3.14
```

------------------------------------------------------------
## 🧬 Dataclass

```python
# Упрощённое создание классов с автоматическим __init__, __repr__ и т.д.

from dataclasses import dataclass

@dataclass
class Person:
    name: str
    age: int
    city: str = "Москва"

p = Person("Том", 25)
print(p)
```

------------------------------------------------------------
## 🧰 NamedTuple и Enum

```python
from collections import namedtuple
from enum import Enum

Point = namedtuple("Point", ["x", "y"])
p = Point(10, 20)
print(p.x, p.y)

class Status(Enum):
    OK = 1
    ERROR = 2

print(Status.OK)
```

------------------------------------------------------------
## 🧪 Тестирование (unittest и pytest)

```python
# unittest

import unittest

def multiply(a, b):
    return a * b

class TestMath(unittest.TestCase):
    def test_multiply(self):
        self.assertEqual(multiply(2, 3), 6)

if __name__ == "__main__":
    unittest.main()

# pytest (просто функции)

def test_add():
    assert 2 + 2 == 4
```

Запуск:
```sh
pytest test_file.py -v
```

------------------------------------------------------------
## 💾 Работа с файлами и путями (pathlib)

```python
from pathlib import Path

p = Path("example.txt")
p.write_text("Hello world")
print(p.read_text())
print(p.exists())
print(p.resolve())
```

------------------------------------------------------------
## 🕸️ HTTP-запросы (requests)

```python
import requests

res = requests.get("https://api.github.com")
print(res.status_code)
print(res.json())
```

------------------------------------------------------------
## 🧵 Многопоточность и многопроцессность

```python
import threading, time

def worker(name):
    print(f"{name} начинает")
    time.sleep(1)
    print(f"{name} завершил")

t1 = threading.Thread(target=worker, args=("A",))
t2 = threading.Thread(target=worker, args=("B",))
t1.start(); t2.start()
t1.join(); t2.join()
```

------------------------------------------------------------
```python
from multiprocessing import Process

def task():
    print("Процесс запущен")

if __name__ == "__main__":
    p = Process(target=task)
    p.start()
    p.join()
```

------------------------------------------------------------
## 📦 Итерабельные утилиты (itertools, functools)

```python
import itertools, functools, operator

# itertools
nums = [1, 2, 3]
print(list(itertools.permutations(nums)))
print(list(itertools.combinations(nums, 2)))

# functools
@functools.lru_cache
def fib(n):
    if n < 2:
        return n
    return fib(n-1) + fib(n-2)

print(fib(10))
```

------------------------------------------------------------
## 🧩 Замыкания (Closures)

```python
def make_multiplier(factor):
    def multiply(x):
        return x * factor
    return multiply

double = make_multiplier(2)
print(double(5))  # 10
```

------------------------------------------------------------
## 🧭 Профилирование

```python
import cProfile

def work():
    sum(range(1000000))

cProfile.run("work()")
```

------------------------------------------------------------
## 🧾 Пример: Мини веб-сервис на FastAPI

```python
# pip install fastapi uvicorn

from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Привет, FastAPI!"}

# Запуск: uvicorn app:app --reload
```
