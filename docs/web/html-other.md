## Doctype

Doctype as DOCument type, декларирует DTD документа. Всегда ассоцируетс с DTD. 

DTD определяет как документ может быть структурирован, какие эл-ты какой эл-т может содержать. В hmtl используется, что указать версию html. В зависимости если распознан doctype бразуер включит:

- no-quirks mode, если разпознан - режимы общих стандартов

- quirks mode - в режиме совместимости (quirks mode), разметка эмулирует нестандартное поведение браузеров Navigator 4 и Internet Explorer 5.

### multiple languages

User agent обычно шлет Accept-Language заголовк, и сервер может ответить соотв контентом:

- <html lang="en">...</html>

### designing or developing for multilingual sites

- lang attribute
- Allow user to select language
- Avoid images with text
- Restrictive words/sentence length
-  Colors are perceived differently across languages and cultures
- Formatting dates and currencies
- Language reading direction

### building blocks of HTML5

- Semantics
- Client side storages
- Multimedia
- Device access

### Cookie

- Может выставить сервер/клиент
- Можно выставить срок устаревания
- Отправляются на сервер браузером, из js нельзя узнать домен
- Размер 4kb

** In UTF-8 characters need between 1 and 4 bytes. So, you can store between 4096 and 1024, respectively, UTF-8 characters in 4KB.**

### localStorage, sessionStorage

- Выставляет только клиент
- Доступ только с клиента
- Размер от 5MB(зависит от браузера)
- sessionStorage живет только на время жизни вкладки

### script, async, defer

- script - загружается и блочиться, парситься и сразу испольняется
- async - загружается паралелльно парсингу HTML, и выполняется как только доступен
- defer - загружается паралелльно парсингу HTML, и выполняется после парсинга html

### progressive rendering

Techniques used to improve perceived load time.

Examples: Lazy loading of images, Content loading prioritization, async HTML fragments

### Progressive and optimistic rendering

TODO: