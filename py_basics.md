# 🐍 Python Cheatsheet — Синтаксис и конструкции

------------------------------------------------------------
## 🏁 Основы

```python
print("Hello, Python!")

# Комментарии
# Однострочные комментарии начинаются с #

"""
Многострочный
комментарий
"""
```

------------------------------------------------------------
## 🔢 Переменные и типы

```python
x = 10              # int
pi = 3.14           # float
name = "Python"     # str
is_active = True    # bool
items = [1, 2, 3]   # list
data = (1, 2, 3)    # tuple
person = {"name": "Tom", "age": 25}  # dict
unique = {1, 2, 3}  # set
none_value = None   # NoneType

print(type(x))      # <class 'int'>
```

------------------------------------------------------------
## 📏 Приведение типов

```python
int("5")        # 5
float("3.14")   # 3.14
str(42)         # "42"
bool(0)         # False
list("abc")     # ['a', 'b', 'c']
```

------------------------------------------------------------
## 🔄 Условия
```python
x = 5
if x > 0:
    print("Положительное")
elif x == 0:
    print("Ноль")
else:
    print("Отрицательное")

# Тернарный оператор
result = "even" if x % 2 == 0 else "odd"
```

------------------------------------------------------------
## 🔁 Циклы

```python
# for
for i in range(5):
    print(i)

# while
count = 0
while count < 3:
    print(count)
    count += 1

# for с break / continue
for n in range(5):
    if n == 2:
        continue
    if n == 4:
        break
    print(n)
```

------------------------------------------------------------
## 📚 Коллекции

```python
# Списки

nums = [1, 2, 3]
nums.append(4)
nums.remove(2)
print(nums[0:2])  # срез

# Кортежи

t = (1, 2, 3)
print(t[1])

# Множества

s = {1, 2, 2, 3}
print(s)  # {1, 2, 3}

# Словари

person = {"name": "Tom", "age": 25}
print(person["name"])
person["city"] = "NY"

for key, value in person.items():
    print(key, value)
```

------------------------------------------------------------
## 🧮 Функции
```python
def add(a, b):
    return a + b

print(add(2, 3))

# Аргументы по умолчанию
def greet(name="Гость"):
    print("Привет,", name)

greet()
greet("Том")

# Произвольные аргументы
def total(*args):
    return sum(args)

print(total(1, 2, 3))

# Ключевые аргументы
def show_info(**kwargs):
    print(kwargs)

show_info(name="Tom", age=25)
```

------------------------------------------------------------
## 🧱 Классы и объекты

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def greet(self):
        print(f"Привет, меня зовут {self.name}")

p = Person("Tom", 25)
p.greet()
```

------------------------------------------------------------
## 🧬 Наследование

```python
class Student(Person):
    def __init__(self, name, age, grade):
        super().__init__(name, age)
        self.grade = grade

s = Student("Alice", 20, "A")
print(s.name, s.grade)
```

------------------------------------------------------------
## ⚙️ Исключения

```python
try:
    x = 1 / 0
except ZeroDivisionError:
    print("Деление на ноль!")
finally:
    print("Блок finally выполнен")
```

------------------------------------------------------------
## 🔁 Списковые выражения (List Comprehensions)

```python
nums = [1, 2, 3, 4]
squares = [x**2 for x in nums]
print(squares)

even = [x for x in nums if x % 2 == 0]
print(even)
```

------------------------------------------------------------
## 🧩 Lambda, map, filter

```python
add = lambda a, b: a + b
print(add(2, 3))

nums = [1, 2, 3, 4]
doubled = list(map(lambda x: x * 2, nums))
filtered = list(filter(lambda x: x > 2, nums))
print(doubled, filtered)
```

------------------------------------------------------------
## 📦 Модули и импорты

```python
# Импорт всего модуля
import math
print(math.sqrt(16))

# Импорт конкретной функции
from math import sqrt
print(sqrt(9))

# Импорт с псевдонимом
import datetime as dt
print(dt.datetime.now())
```

------------------------------------------------------------
## 🧵 Работа с файлами

```python
with open("data.txt", "w", encoding="utf-8") as f:
    f.write("Привет, файл!")

with open("data.txt", "r", encoding="utf-8") as f:
    content = f.read()
    print(content)
```

------------------------------------------------------------
## 🧠 Итераторы и генераторы

```python
def countdown(n):
    while n > 0:
        yield n
        n -= 1

for i in countdown(3):
    print(i)
```

------------------------------------------------------------
## 🕒 Работа со временем

```python
import time, datetime

print(time.time())  # секунды с 1970
print(datetime.datetime.now())
```

------------------------------------------------------------
## 🧩 Работа с JSON

```python
import json

data = {"name": "Tom", "age": 25}

json_str = json.dumps(data)         # в строку JSON
obj = json.loads(json_str)          # обратно в dict

with open("data.json", "w") as f:
    json.dump(data, f)
```

------------------------------------------------------------
## 🧾 Пример минимального приложения

```python
def greet(name):
    return f"Привет, {name}!"

if __name__ == "__main__":
    print(greet("Python"))
```
