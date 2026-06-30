---
tags:
  - databases
  - netology
---
### Object-Relation Mapping
ORM (объектно-реляционное отображение) - это дополнительный способ взаимодействия с БД из кода, который работает с таблицами и запросами к БД как с классами, объектами и методами в ООП

Мотивация использовать ORM:
- Необязательно знать SQL и специальные функции СУБД
- Возможность десериализации (распаковки) результата из БД в удобном формате для API или обработки
- Не нужно придумывать сложные парсеры массивов, работаем с готовыми структурами данных
- Один и тот же код может работать в разных БД, если на это рассчитана ORM
- Разработчиками ORM продуманы примитивные вопросы безопасности, экранирования и оптимизации запросов

Недостатки ORM:
- Изначально не очевидны итоговые запросы
- Ограничения и читаемость при построении сложных, многоуровневых запросов

### ORM в C++
В отличие от других языков программирования, В С++ концепция ORM не настолько популярная
Две самых популярных ORM в C++:
- QxOrm - библиотека для ORM в составе фреймворка Qt
- wtdbo - библиотека для ORM, часть набора библиотек Wt

### Установка wtdbo
Установить Wt можно двумя способами:
- Собрать из исходного кода Wt
- Установить уже скомпилированные библиотеки Wt для своей операционной системы
Поскольку для сборки Wt требуется много зависимостей, включая Boost, то для урока установим уже скомпилированные библиотеки для Windows

После установки можно использовать wtdbo в своем проекте VS
Для того, чтобы использовать wtdbo вместе с PostgreSQL в своем проекте, необходимо настроить проект, а именно:
- Указать путь к заголовочным файлам Wt
- Указать путь к статическим библиотекам Wt
- Указать библиотеки, с которыми будет линковаться проект

### Заголовочные файлы Wt
```
#include <Wt/Dbo/Dbo.h>
#include <Wt/Dbo/backend/Postgre.h>

int main()
{
	return 0;
}
```

### Объявление ORM-класса
```
class User {
public:
	std::string name = "";
	std::string phone = "";
	int karma = 0;
		
	template<class Action>
	void persist(Action& a)
	{
		Wt::Dbo::field(a, name, "name");
		Wt::Dbo::field(a, phone, "phone");
		Wt::Dbo::field(a, karma, "karma");
	}
};
```

### Подключение к PostgreSQL
Для работы ORM, необходимо подключиться к базе данных PostgreSQL
```
int main()
{
	std::string connectionString =
		"host=localhost"
		" port=5432"
		" dbname=lesson03"
		" user=lesson03user"
		" password=lesson03user";
		
	auto postgres = std::make_unique<Wt::Dbo::backend::Postgres>(connectionString);
	
	Wt::Dbo::Session session;
	session.setConnection(std::move(postgres));
	session.mapClass<User>("user");
	
	// ...
}
```

Совет: для упрощения обработки ошибок, оборачивать вызовы к Wt::Dbo в блок try, а в блоке catch ловить исключение Wt::Dbo::Exception. Пример:
```
try
{
	std::string connectionString =
	"host=localhost"
	" port=5432"
	" dbname=lesson03"
	" user=lesson03user"
	" password=lesson03user";

	// ...
} catch (const Wt::Dbo::Exception& e)
{
	std::cout << e.what() << std::endl;
}
```

### Создание таблиц
При работе ORM не нужно заранее создавать таблицы с необходимой структурой - ORM позволяет сделать это автоматически:
```
session.createTables();
```

### Добавление записей
Пример создания транзакции для добавления новой записи в таблицу:
```
Wt::Dbo::Transaction transaction{ session };

std::unique_ptr<User> user{ new User() };
user->name = "Joe";
user->phone = "1234567890";
user->karma - 13;

Wt::Dbo::ptr<User> userPtr = session.add(std::move(user));

transaction.commit();
```

### Получение записей
Пример кода для получения всех записей:
```
typedef Wt::Dbo::collection<Wt::Dbo::ptr<User>> Users;
Wt::Dbo::Transaction transaction{ session };
Users users = session.find<User>();
std::cout << "We have " << users.size() << " users:" << std::endl;
for(const Wt::Dbo::ptr<User>& user : users)
{
	std::cout << " user " << user->name 
		<< " with karma of " << user->karma << std::endl;
}

transaction.commit();
```

Если нужно получить только одну запись:
```
Wt::Dbo::ptr<User> joe = session.find<User>().where("name = ?").bind("Joe");

std::cout << "Joe has karma " << joe->karma << std::endl;
```

### Редактирование записей
В отличие от других ORM, по умолчанию все записи Read-Only. Если нужно редактировать запись, то нужно вызвать метод modify():
```
Wt::Dbo::ptr<User> joe = session.find<User>().where("name = ?").bind("Joe");

std::cout << "Joe has karma: " << joe->karma << std::endl;

joe.modify()->name = "John";
joe.modify()->karma = 100;
```

### Отношение Many-To-One
Предположим, что у нас есть отношения между таблицами user и post. Один пользователь может написать много постов. Для этого определим класс Post:
```
class User;

class Post {
public:
	std::string title = "";
	std::string text = "";
	dbo::ptr<User> user;
	
	template<class Action>
	void persist(Action& a)
	{
		Wt::Dbo::field(a,title, "title");
		Wt::Dbo::field(a, text, "text");
		dbo::belongsTo(a, user, "user");
	}
};
```

Класс User тоже необходимо дополнить:
```
class User {
public:
	std::string name = "";
	std::string phone = "";
	int karma = 0;
	Wt::Dbo::collection< dbo::ptr<Post> >posts;
	
	template<class Action>
	void persist(Action& a)
	{
		Wt::Dbo::field(a, name, "name");
		Wt::Dbo::field(a, phone, "phone");
		Wt::Dbo::field(a, karma, "karma");
		Wt::Dbo::hasMany(a, posts, dbo::ManyToOne, "user");
	}
};
```

Теперь можно легко добавлять пользователям посты, или наоборот - назначать посту пользователя. Система ORM автоматически создаст внешние ключи, и будет сама управлять связями между сущности:
```
Wt::Dbo::ptr<Post> post = session.add(std::unique_ptr<Post>{new Post()});
post.modify()->user = joe; // or joe.modify()->posts.inser(post);

// will print 'Joe has 1 post(s).'
std::cout << "Joe has " << joe->posts.size() << " post(s)." << std::endl;
```

