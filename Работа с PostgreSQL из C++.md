---
tags:
  - databases
  - netology
---
`libpq++` - это официальный API для С++ клиента PostgreSQL
Эта библиотека - с открытым исходным кодом под лицензией BSD. Исходный код libpq++ доступен на Github
libpq++ собирается с помощью CMake, следовательно, его можно легко добавить в свой проект

### Подключение libpq++ к CMake-проекту
1. Скачать свежий релиз libpq++ с гитхаба и распаковать его в отдельный каталог
2. Добавить путь к этому каталогу в ваш файл CMakeLists.txt
3. Включить поддержку C++17 в проекте
4. Слинковать проект с libpq++

Пример CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.20)
project(MyProject1)
add_executable(MyProject1 main.cpp)

add_subdirectory(C:/path/to/libpqxx libpqxx-build) # указать путь к libpq++

target_compile_features(MyProject1 PRIVATE cxx_std_17) # Включить C++17

target_link_libraries(MyProject1 pqxx) # Линковка libpq++ к проекту
```

Для работы с libpq++ необходимо:
```
#include <pqxx/pqxx>

int main()
{
	try
	{
		pqxx::connection c(
			"host=localhost "
			"port=5432 "
			"dbname=my_datebase "
			"user=my_datebase_user "
			"password=my_password_123");
		
		// ...
	} catch (pqxx::sql_error e)
	{
		std::cout << e.what() << std::endl;
	}


	return 0;
}
```

Совет: для упрощения обработки ошибок, оборачивать вызовы к libpq++ в блок try, а в блоке catch ловить исключение pqxx::sql_error

### Select-запросы

Допустим есть таблица book, для того чтобы получить данные из этой таблицы, нужно создать объект work и выполнить над ним запрос query():
```
pqxx::work tx{ c };

for (auto [title, author] : tx.query<std::string, std::string>(
	"SELECTR title, author FROM book"))
{
	std::cout 
}

```