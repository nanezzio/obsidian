---
tags:
  - python
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

Таким образом, конструктор в Python - это именно инициализатор состояния

Пример:

```python
class Point:
	def __init__(self, x, y):
		# инициализирует состояние объекта
		self.x = x
		self.y = y
		
	def coords(self):
		return (self.x, self.y)
		
p = Point(2, 5)
print(p.coords()) # (2, 5)
```


### Наследование

Наследование позволяет описать новый класс на основе существующего: дочерний класс получает данные и методы родительского

Запись `class Child(Parent):` означает, что `Child` наследует `Parent`

Дочерний класс может добавлять новые методы и переопределять унаследованные

Пример:

```python
class Animal:
	def speak(self)
		return "Some generic sound"
		
class Dog(Animal): # Dog наследует Animal
	def speak(self): # переопределяем метод
		return "Woof!"
		
class Cat(Animal):
	def speak(self):
		return "Meow"
		
a = Animal()
d = Dog()
c = Cat()

print(a.speak()) # Some generic sound
print(d.speak()) # Woof!
print(c.speak()) # Meow
```


### Полиморфизм

Полиморфизм - возможность обращаться к объектам разных классов через общий интерфейс (одинаковые имена методов), не заботясь о конкретном типе

Главное: "если объект умеет этот метод, его можно использовать"

В Python это хорошо ложится на утиный тип: "если ходит как утка и крякает как утка - это утка"

Пример:

```python
class Bird:
	def fly(self):
		return "Burd is flying"
		
class Airplane:
	def fly(self):
		return "Airplane is flying"
		
def make_it_fly(obj):
	# не важно Bird это или Airplane главное что есть метод fly()
	print(obj.fly())
	
make_it_fly(Bird()) # Bird is flying
make_it_fly(Airplane()) # Airplane is flying
```


### Инкапсуляция и "приватные" данные

Инкапсуляция - объединение данных и методов внутри класса и сокрытие деталей реализации за публичным интерфейсом

В Python нет настоящего `private`, поэтому используют соглашения:
 - `_name` - "внутренний" атрибут (не трогай снаружи без необходимости)
 - `__name` - включается "name mangling" (имя переписывается в `_ClassName__name`)