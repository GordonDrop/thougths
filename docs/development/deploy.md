# Deploy

## Intro

Деплой - **(1)предсказуемая (2)prod-ready (3)воспроизводимое** размещение проекта на том или ином устр-ве, предназначеннго общего доступа.

1. Предсказуемость - один и тот же результат деплоя для каждой версии преокта. Lock файлы.

2. Prod-ready - мин необходимое кол-во пакетов для работы проекта. Из соображение безопасности и воспроизводимости условий.

3. Воспроизводимость

## Docker

Помогает обеспечить:
- prod-ready - изолированность окружения для минимизирования зав-тей 
- воспроизводимость

12 factor app https://12factor.net/ru/

## Идеальный вариант деплоя

kbs - kubernates кластер

Схема
3 kbs: dev, stage, "смотрит в мир" prod
Один Container registry - CR
CI-kbs kbs кластер - для запуска всяких CI штук, типа контейнеров с браузерами и тп

Деплой:
1. Коммит в репозиторий тригерит старт
2. CI с помощью lerna понимает какие пакеты были изменены. lerna собирает докер файл и отправляет его в CR по имени app-package-'git describe'
3. Перед сборкой образов для того докер файла происходит прогон unit и интеграц тестов
4.Образы собираются и отправляются в CR
5. CI система дергает kubctl и говорит CI-kbs обновить пакеты, к были собраны до нужной версии.
6. CI kbs передает их в CR
7. CI система запускает e2e тесты, которые крятятся  
5. Публикация контейнеров в CR и обновление пакетов, затем прогон e2e тестов внутри CI-kbs. Для перфоманса может быть несколь CI кластеров
6. После успеха - dev kbs дергает свеиже версии пакетов из CR 

Доп для релиза можно пушит все далее в kbs stage, и prod

TODO: CI tools, docker/kb, lerna, e2e в кластерах