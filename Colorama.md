---
tags:
  - python
---
### Введение

Colorama - библиотека с помощью которой можно оформлять вывод текста в терминале, работает одинаково на всех ОС

### Как работает

- Установка: `pip install colorama`

С помощью констант `Fore`, `Back`, `Style` становится возможно управлять оформлением без ANSI кодов


### Синтаксис

- F

Пример:

```python
from colorama import init, Back, Fore, Style

init() # инициализация Colorama

#1
s = "abc"
s = Back.WHITE + Style.BRIGHT + Fore.RED + s + Style.RESET_ALL

print(s)
# Проверяю что RESET_ALL сработало правильно
print("Hello")

#2
def info(lol):
    print(Fore.CYAN + "[cab] " + Style.RESET_ALL + lol)  

info("123321")


```