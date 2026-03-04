---
tags:
  - python
---
### Введение

Colorama - библиотека с помощью которой можно оформлять вывод текста в терминале, работает одинаково на всех ОС

### Как работает

- Установка: `pip install colorama`

С помощью констант `Fore`, `Back`, `Style` становится возможно управлять оформлением без ANSI кодов

Пример:

```python
from colorama import init, Back, Fore, Style

init() # инициализация Colorama

s = "abc"
s = Back.WHITE + Style.BRIGHT + Fore.RED + s + Style.RESET_ALL

print(s)
# Проверяю что 
print("Hello")

#3
def info(lol):
    print(Fore.CYAN + "[cab] " + Style.RESET_ALL + lol)  

info("123321")


```