# Testing

Тестирование - способ обнаружния ошибок(дефектов) в ПО.

- цель тестирование - обнаружение ошибок
- тестирование указывает на качество, но не влияет на него. Отсутсвие ошибок может указывать на кач-во, так и неполноту или неэффективность тестов
- тестирование трубует, чтобы вы ожидали ошибки

## Виды тестирования:

По уровням:

- unit - тестирование отдельного компонента, класса, метода. Другие модули мокаем. Позволяют более точно определить в какой части кода произошла ошибка
    Требования: быстрота (никакой асинхронщины), легковестность, изолированность
- integration - тестирование компонента во взаимодействии с другими. Делам на тот случай, если в отдельности компоненты работают, но вместе могут встерчаться противоречия в их работае. Тут уже асинк штуки тестить можно.
- regression - для обнаружения дефектов уже работающего функционала
- e2e, ui, system

По знанию системы: методом "белого" и "черного" ящика.

## Подходы.

###  Test Driven Design

Test -> Red -> Green -> Refactor until Green

Этап Test - есть по сути планирование на основе требований

### Приемы тестирования

Важно:
- пишите грязные тесты
- используйте разные приемы

Главное: пишите тесты, так чтобы вероятность нахождения ошибок была близка к максимальной

### Подход на основе структуры кода(или потока выполнения):

Покрываем тестами каждый оператор. Мин кол-во тестов - 1 один тест на каждый if, for, while и тп. Напр, для if по тесту на каждый вариант значения условия, а для for условия проверки, условия выхода, границы цикла и тп.

### Подход на основе потока данных

Состяние данных:
- init
- usage
- del

По действию над данными:
- enter - поток управления входит в метод и сразу делает что-то с переменной
- exit - поток упр-я покидает метод сразу после действия

Нормальное цикл жизни переменной: init -> usage -> delete

Подозрительные варианты:

- init-init - двойное определение
- init-exit
- init-del
- enter-del
- enter-usage
- del-del
- del-usage
- usage-init

Проверяем и исправляем подобное еще до начала тестирования. Затем тестируем все init-usage комбинации. 

Еще тесты данных
- **Граничные условия** - по тесту на border - 1, border + 1 и out of range
- **Негативные данные**
- **Избыточные данные**
 