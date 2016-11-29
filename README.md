PF2 — веб-фреймворк для Parser 3
================================

Новая версия [библиотеки PF](https://bitbucket.org/ovolchkov/parser3-pf).

Релиз в ветке master. Разработка в бранче develop и ветках feature/name.

Авторы:
* Олег Волчков ([oleg@volchkov.net](mailto:oleg@volchkov.net), [unhandled-exception.ru](http://unhandled-exception.ru))
* Алексей Марьин ([groundzero.ru](http://groundzero.ru))

## Установка

`git clone https://github.com/unhandled-exception/pf2.git`

## Что меняем при переходе с PF1 на PF2

Делаем фреймворк вместо библиотеки. Стараемся сохранить слабую связь классов, чтобы их можно было использовать не загружая в память программы всю библиотеку.

[Концепция и стили кодирования](docs/concepts.md)

[Список классов в модулях](classes.md)

### Базовые принципы и идеи
* Минимизируем число файловых операций. Объединяем классы в модули. Модуль — файл с несколькими классами, которые реализуют какой-то законченный функционал. В отдельных файлах оставляем API-классы и классы, которые используются редко.
* Вводим миксины. Как замена множественному наследованию.
* @preRequest[aRequest] или описать как работать с @processRequest[...]
* Сделать хороший response-класс
* Редиректы в добавок к роутам
* Авторедирект с /name -> /name/
* Глобальный ingoreCase(false) для роутов
* Генератор кода: generate.p ...
* Переписать sql-классы, добавит подднржку диалектов sql, для orm’а. Вместо sql-классов — коннекторы.
* Делаем класс для форматирования дат и времени. dtf.p нам не нужен.

### Новый pfAuth 2
* Переводим на работу с  request/response в вебфреймворке.
* БД-стораджи через pfSQLTable. Сторадж — модель с допинтерфейсом для проверки паролей.
* Единый авторизационный токен. Зашифрованная key-value строка с подписью sha512+hmac. Токен можно передавать через http-заголовок, куку или form.
* pfAnitiFlood переделываем в «csrf protection». Посмотреть какие еще варианты защиты можно добавить: проверка реферера, других заголовков и пр. https://docs.djangoproject.com/en/1.8/topics/security/, http://laravel.com/docs/5.1/routing#csrf-protection
* ^can[…]{if true}{else} - метод с блоком, чтобы обойтись без ифа. (`@can[permission;true_code;false_code]`) [from the Laravel]

### Удаляем из библиотеки старые и неподдерживаемые классы
* Юнит-тесты. Нужен полноценный фреймворк.
* Wiki. Переносим класс pfURLTranslit в моудуль web.
* Коллекции: хеш умеет все, что нужно.
* Management. Пишем новый консольный фреймворк (generator.p и т.п.)
* Highlighters.
* images. Надо новый api-класс над IM сделать.
* another.
* deprecated. pfScroller делаем полноценным классом вместо обвязки над scroll.p.
* io/pfCurlFile.p [Возможно, что его и стоит оставить как пару к pfCFile.]
* pf/sql/generics. pfSQLSettings переводим на ORM.
* Пишем документацию. Markdown. :)

### Новый способ проброса экшна в контролеры

Настройка mod_rewrite:

RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^ _ind.html [L]

