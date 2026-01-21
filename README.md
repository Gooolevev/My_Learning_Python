# 🐍 Python Core & Pro Tips Cheat Sheet

I have learning Python just 1.5 month, but I was understand that lose more time and with could learning onle this:

## Основы и полезные фишки

# Главные встроенные функции (Вспомнить всё)

- ```map(func, iterable)``` - применить функцию к каждому элементу
- ```filter(func, itertabgle``` - остав
- ```filter(func, iterable)``` - оставить только те, где True
- ```any([True, False])``` - True, если хотя бы один True
- ```all([True,True])``` - True, если все True
- ```sorted(iterable, key=lambda x: x[1])``` - сортировка по ключу

```python


# Распаковка (Unpacking)
"""тут суть в том, что просто дополнительно присвоить третьей переменной оставшиеся значения, типо * - значит, собери все оставшиеся в этот список"""
a, b, *rest = [1, 2, 3, 4, 5]  # a=1, b=2, rest=[3, 4, 5]

# F-строки (Python 3.6+)
"""просто красивенько"""
name = "Gemini"
print(f"Hello, {name.upper():>10}") # Выравнивание и методы внутри
num = 123.456
print(f"{num:.2f}")                # Округление до 2 знаков: 123.46

# Быстрый обмен переменными
a, b = b, a

# Оператор морж
"""крутая штука, которое может помочь перестать дублировать переменные"""
while (line := input("Enter text: ")) != "stop":
    print(f"You wrote: {line  }")

line = input("Enter text: ")
while line != "stop":
    print(f"You wrote: {line}")
    line = input("Enter text: ")
```

## Коллекции и One-liners (В одну строку)

```python
# List/Dict/Set Comprehensions
nums = [res for i in range(10) if (res := i) % 2 == 0]       # Список четных
unique_chars = {char for char in "apple"}          # Множество {'a', 'p', 'l', 'e'}
square_dict = {x: x**2 for x in range(5)}         # {0: 0, 1: 1, 2: 4...}

# Полезные функции
names = ["Alice", "Bob"]
scores = [85, 92]

list(zip(names, scores))       # [('Alice', 85), ('Bob', 92)]
for i, v in enumerate(names):  # Индекс и значение одновременно
    print(i, v)

# Условие в одну строку (Ternary)
status = "Adult" if age >= 18 else "Child"
```

## Работа с файлами и JSON

```python
import json
```

```python
data = {"id": 1, "name": "Admin", "skills": ["Python", "SQL"]}

# Запись в JSON
with open("config.json", "w", encoding="utf-8") as f:
    json.dump(data, f, indent=4)

# Чтение из JSON
with open("config.json", "r") as f:
    loaded_data = json.load(f)

# Чтение текстового файла построчно
with open("notes.txt") as f:
    lines = [line.strip() for line in f]
```

## ООП: Классы, @property и @dataclass
#### Важно
```python
# ✅ Правильно
@dataclass
class Person:
    name: str          # ← без default
    age: int = 0       # ← с default

# ❌ Ошибка: "non-default argument follows default"
@dataclass
class BadPerson:
    name: str = "Anonymous"
    age: int   # ← нельзя!
```
```python
@dataclass(frozen=True) # не изменяемые значения в классе
```
```python
@dataclass(slots=True) # крутая темка для ускорения памяти (работать только с большими данными)
```
```python
@dataclass(order=True) # Автоматические <, >, <=, >= (по полям, где compare=True)
```

```python
from dataclasses import dataclass, field
```
#### Пример
```python
@dataclass
class User:
    username: str
    user_id: int
    email: str = None
```
#### Валидация через __post_init__
```python
@dataclass
class Product:
    name: str
    price: float
    in_stock: int

    def __post_init__(self):
        if self.price <= 0:
            raise ValueError("Price must be > 0")
        if self.in_stock < 0:
            raise ValueError("in_stock can't be negative")
```

#### Пример с field
```python
@dataclass(order=True)
class Employee:
    name: str
    age: int
    job: str
    password: str = field(default="12345", repr=False)
    user_id: int = field(default=0, compare=False)
    skills: list = field(default_factory=list)

emp = Employee("Ivan", 25, "Dev", password="12345", user_id=1, skills=["Good man", "important"])
print(emp)
```
#### Пример с field + json
```python
from dataclasses import dataclass, field, asdict
import json
@dataclass(order=True)
class Employee:
    name: str
    age: int
    job: str
    password: str = field(default="12345", repr=False)
    user_id: int = field(default=0, compare=False)
    skills: list = field(default_factory=list)

emp = Employee("Ivan", 25, "Dev", password="12345", user_id=1, skills=["Good man", "important"])
print(emp)

with open("config.json", "w",) as f:
    json.dump(asdict(emp), f, indent=4)

with open("config.json", "r") as f:
    loaded_data = json.load(f)

new_emp = Employee(**loaded_data)
print(new_emp)
```
#### База
```python
class Main:
    def __init__(self, name: str, age: int, job: str):
        self.name = name
        self.age = age
        self.job = job

    @property
    def age(self):
        return self._age
    
    @age.setter
    def age(self, value):
        if value < 0: raise ValueError("Не может быть у человека отрицательный возраст!!!")
        self._age = value


    def __repr__(self):
        return f"Main (name = '{self.name}', age= '{self.age}', job = '{self.job}')"

    def __eq__(self, other):
        if not isinstance(other, Main):
            return NotImplemented
        return self.age == other.age
    
    def __lt__(self, other):
        if not isinstance(other, Main):
            return NotImplemented
        return self.age < other.age
    
    def __str__(self):
        return f"Сотрудник {self.name}, возраст: {self.age}"

    
user1 = Main("Ivan", 25, "Dev")
user2 = Main("Alex", 30, "CEO")

print(user1 < user2) # True
print(user1 == user2) # False
print(user1 > user2) # False
print(user1.__repr__)
  
```

## Pygame 

```python
import pygame

pygame.init()
screen = pygame.display.set_mode((800, 600))
clock = pygame.time.Clock()

running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running == False
    
    screen.fill((255,255,255))
    pygame.draw.rect(screen,(0,255,0), (50,50,300,500))
    pygame.display.flip()
    clock.tick(60)

pygame.quit()
```
---

> P.S. Будет все обновляться. я это просто делаю что бы самому весь этот бред запомнить
