---
tags:
  - todo_study
---
### Теория

Класс - это шаблон (описание типа объекта), в котором задаются данные (атрибуты) и поведение (методы)

Объект (экземпляр) - конкретный "представитель" класса с уже заполненными значениями атрибутов

В Python почти все является объектом, а классы объявляются через ключевое слово `class`

Экземпляр создается вызовом класса как функции: `obj = MyClass(..)`

Пример:

```python
class Student:
	def __init__(self, name, marks):
		self.name = name # атрибут экземпляра
		self.marks = marks
		
	def info(self):
		return f"{self.name}: {self.marks}"
		
s1 = Student{"Harry", 85}
s2 = Student{"Hermione", 100}

print(s1.info()) # Harry: 85
print(s2.info()) # Harmione: 100
```


### Атрибуты класса и экземпляры


Атрибуты экземпляра хранят состояние конкретного объекта и обычно задаются в `__init__` через `self.имя`
Атрибуты класса общие для всех экземпляров и объявляются прямо в теле класса

При обращении `obj.attr` Python сначала ищет атрибут в объекте, потом в классе

Пример:

```python
class Dog:
	# атрибут класса - общий для всех собак
	species = "Canis familiaris"
	
	def __init__(self, name, age):
		# атрибуты экземпляра - у каждого свои
		self.name = name
		slf.age = age
		
dog1 = Dog("Miles", 4)

print(dog1.species, dog1.name, dog1.age)
# Canis familiaris Miles 4

# можно обратиться к атрибуту класса через сам класс
print(Dog.species) # Canis familiaris
```


### Методы и `self`

Метод - функция, объявленная внутри класса, которая описывает поведение объектов этого класса
Первый параметр обычно метода по соглашению называется `self` и ссылается на конкретный экземпляр
Когда мы пишем `jbj.method()`, Python неявно подставляет `obj` в `self`

Пример:

```python
class Person:
	def __init__(self, name):
		self.name = name
		
	def greet(self): # метод экземпляра
		return f"Hello, my name is {self.name}"
		
p = Person("Emma")
print(p.greet)  # Hello, my name is Emma
```


### Конструктор `__init__` и жизненный цикл

При создании объекта `MyClass(..)` Python сначала создает "пустой" объект, затем вызывает метод `__init__(self, ..`

`__init__` не возвращает объект, а инициализирует уже созданный: записывает начальные значения атрибутов

Таким образом, конструктор в Python -