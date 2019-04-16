\#безопасность \#виды_атак

//  более общий вид, это скорее должен быть список ресурсов

# Виды атаки, фронтенд

## Виды атак

### Confused deputy problem.

Общий вид атак семейста Confused deputy problem характерен для веба.

Случается, когда программа имеет разрешение на опред действие, но применяет это разрешение для действий, которые пермишном не предусмотрены.

Фундаментальная причина - отсутствие прямой связи между тем, что делает приложение и почему оно имеет разрешение на это.


### XSS - cross site scripting

**Способы:** events attributes, src with javascript pseudo protocol, script tag inejcting into forms

**Виды:** reflected, stored, DOM based

### CSRF - cross site forgery 

**Межсайтовая подделка запроса**

**Способы:** использование особенностей http протокола для соврешения действий на приложениях и сайтах, в который на данный момент пользователь авторизован. пользователь не подезревает, что от его имени может быть совершенно действие.

Соц инжинерия, xss, прослушаивание нешифрованного трафика.

## Набор мер по веб безопасности

https://m.habr.com/ru/post/445932/

- Проверять надежность стороннего кода и источников данных
- Актуальная версия фреймворка
- Санитайзер для html на стороне клиента
- Валидация данных на стороне бекенда

Ресурсы
https://en.wikipedia.org/wiki/Confused_deputy_problem
https://www.owasp.org/index.php/Cross-site_Scripting_(XSS)
https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)

Краткие интсрукции по предотвращению: https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets
For fun: https://xss-game.appspot.com/