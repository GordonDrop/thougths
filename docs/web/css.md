# Css notes

## selector matching to DOM elements - travesing up by reading selector from right to left

## Специфичность

Способ орпедления какое правило применять. 

Матрица a b c d, где:

a. `1` если стили определены как инлайн, иначе `0`
b. кол-во id селекторво
c. кол-во селекторов классов, аттрибутов и псевдо-классов
d. кол-во селекторов тегов и псевдо элементов

Матрицы разных правил сравниваютс слева направо, значение в левой колонке перевисывает значение в правой. Пр: 0, 1, 0, 0 больше 0, 0, 10, 10.

Случаях равенства - применяется последнее правило в коде.

!important круче всех.

## page parsing, CSSOM, paint, reflow? compositing

## block formatting context

Механизм отображения страницы в css. регион, в котором размещаются блоки в привычном порядке, также в нем взаимодействуют с другими эл-ми.

Охватывает все содержимые элемента, его создающего.

Новый можно создать с помощью:
- float
- overflow
- position absolut
- display inline-block
- flex, grid
- и т.д.

## поток

**Нормальный поток** - это как браузер располагает эл-ты по умолчанию, когда вы не делаете ничего для контроля лейаутата.

!note: Block Direction - направление, в к. располагаются эл-ты. Зависит от языка.

В bfc боксы лежат один за другим вертикально, сверху вниз. Верт дистанция определяется margin. Верт маргины колапсятся.

В ifc боксы лежат горизонтально сверху вниз. Гор падинги и маргины учитываются. Можно вертикально выравнивать боксы.

Блоки занимают все место в инлайн наравлении. 

margin collapse - нижний и верхний маргины смежных блоков коллапсятся - учитывается только больший.

Правило display позволяет менять поведение боксов, которые лежат внутри элемента. Например, display: flex - контейнер ведет себя как обычный блок, а эл-ты внтури эл-ты иметю flex formatting  context.

## Меняем поток

- display
- float
- position
- table
- root(html)
- multi-column

Эл-ты вне потока создают новый bfc.

- float - эл-т сдвигает максимально в направлении флота.  Эл-ты в контексте стекаются к элементу макс близко.

- position absolute - эл-т поднимается над другим. Не влияет на их позиции. Внутри свой bfc по правилам норм потока.

```
.wrapper::after {
  content: "";
  clear: both;
  display: block;
}
```
 
## flexes

**Сложности использования float и position:**

- верт выравнивание и центрирование внутри родителя
- создание колонок или рядов, которые занимают все свободные высоту или ширину
- колонки одинаковой высоты

**Оси:**
- main axis - верт. Main end и main start
- cross axis - перпендикулярна main axis

Flex container - эл-т c display: flex. Flex items inside

**flex-direction** - container. указывает направление главной оси. Как результат направление row или column.

**flex-wrap** -  container. указывает как располалагать контент, когда контент переполняет контейнер.

**flex-flow shorthand**  - Пример flex-flow: row wrap;

**flex: flex-grow flex-shrink flex-basis**:
- flex-grow - число - пропорция
- flex-basis - мин ширина

Выравнивание.

**align-items** set on child. Default *stretch*. Значение *center* выраввнивает отност оси cross.

**justify-content** - определяет, где контент располагается на главной оси

**order** - Число со знаком. эл-ты с большим весом появяться последними.

[Flex cheatsheet](http://yoksel.github.io/flex-cheatsheet)

## Grids

Грид - коллекция гор и верт линий, создаюших паттерн отностильно которго можно располагать эл-ты. У гридов есть колонки, ряды и интервал(промежуток между соседними гридами).

Ex:

```
.container {
    display: grid;
    grid-template-columns: 200px 200px 200px;
    grid-gap: 20px;
}
```

Fractional unit(fr) - единица гибкости. Авто расчет нужной ширины занимаемой одним гридом.
`grid-template-columns: 1fr 1fr 1fr;`

`grid-template-columns: repeat(3, 1fr);` - повторяем три колонки шириной 1fr

**Ширина и высота колонок** 

`grid-template-columns or grid-template-rows`

grid-auto-rows: 100px; - высота рядов
grid-auto-rows: minmax(100px, auto); - мин высота 100xp, макс auto


**Линии**

Занимаем [start, end)
```
grid-column-start
grid-column-end

grid-column: start/end; shorthand

grid-row-start
grid-row-end

grid-row: start/end; 
```

**Поизиционирование**

```
.container {
  ...
  grid-template-areas: 
      "header header"
      "sidebar content"
      "footer footer";
  ...
}

header {
  grid-area: header;
}

article {
  grid-area: content;
}

aside {
  grid-area: sidebar;
}

footer {
  grid-area: footer;
}
```
