# 🐍 Python Core & Pro Tips Cheat Sheet

I have learning Python just 1.5 month, but I was understand that lose more time and with could learning onle this:

## Основы и полезные фишки

```
# Распаковка (Unpacking)
a, b, *rest = [1, 2, 3, 4, 5]  # a=1, b=2, rest=[3, 4, 5]

# F-строки (Python 3.6+)
name = "Gemini"
print(f"Hello, {name.upper():>10}") # Выравнивание и методы внутри
num = 123.456
print(f"{num:.2f}")                # Округление до 2 знаков: 123.46

# Быстрый обмен переменными
a, b = b, a

# Оператор морж
if (n := len(items)) > 10:
    print(f"Too many items: {n}")
```

## Коллекции и One-liners (В одну строку)

```
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

```
import json
```

```
import json

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

```
from dataclasses import dataclass, field
from typing import Optional
```

```
@dataclass
class User:
    username: str
    user_id: int
    email: Optional[str] = None  # Значение по умолчанию

@dataclass
class Team:
    name: str
    # default_factory: используется для изменяемых типов (list, dict, set)
    members: list[str] = field(default_factory=list)
    
    # repr=False: поле не будет отображаться при print(obj)
    # compare=False: поле не будет учитываться при сравнении объектов (==)
    internal_id: int = field(default=0, repr=False, compare=False)
    
    # init=False: поле не передается в конструктор __init__
    score: int = field(init=False, default=100)

# Пример
t = Team(name="Developers", members=["Alice", "Bob"], internal_id=999)
print(t)  # Team(name='Developers', members=['Alice', 'Bob']) 
          # (internal_id скрыт благодаря repr=False)

# Обычный класс с "магией" и свойствами
class BankAccount:
    def __init__(self, owner: str, balance: float):
        self.owner = owner
        self._balance = balance  # Инкапсуляция (protected)

    # Вычисляемое свойство (getter)
    @property
    def balance(self):
        return f"${self._balance:.2f}"

    # Сеттер с логикой
    @balance.setter
    def balance(self, value):
        if value < 0: raise ValueError("Нельзя в минус!")
        self._balance = value

    # Магические методы
    def __str__(self): # Для print()
        return f"Account of {self.owner}"

    def __repr__(self): # Для отладки
        return f"BankAccount(owner='{self.owner}', balance={self._balance})"

    def __len__(self): # Для len(obj)
        return int(self._balance)
  
```

# Главные встроенные функции (Вспомнить всё)

- ```map(func, iterable)``` - применить функцию к каждому элементу
- ```filter(func, itertabgle``` - остав
- ```filter(func, iterable)``` - оставить только те, где True
- ```any([True, False])``` - True, если хотя бы один True
- ```all([True,True])``` - True, если все True
- ```sorted(iterable, key=lambda x: x[1])``` - сортировка по ключу
