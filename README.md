# Начало работы

1.  Склонируйте репозиторий
2.  Перейдите в папку с проектом и выполните команду `npm install`
3.  Выполните пкоманду `bower install`
4.  Выполните команду `gulp` для начала работы с проектом или `gulp build` только для сборки проекта.

### Другие команды gulp
* `gulp clean` - очищает папку build  
* `gupl sprites` - перегенерация спрайтов

# Используемые технологии
* Сборщик - [gulp](http://gulpjs.com/)
* Менеджер пакетов - [bower](http://bower.io/)
* Шаблонизатор верстки - [Jade](http://jade-lang.com/).
* CSS-препроцессов - [Stylus](http://stylus-lang.com/).
* JS-фреймворки - [jQuery](http://jquery.com/), [browserify](http://browserify.org/).

# Структура проекта
Структура проекта содержит в себе два основных каталога: source и build.
Разработчик взаимодействует только с каталогом source. Содержимое каталога build генерирует сборщик статики.
### Структура каталога source:
```
source/
    jade/
        _layout.jade
        home.jade
        news.jade
        ...
    blocks/
        foundation/
        base/
        project/
        cosmetic/
    f/
        fonts/
    content/
        i/
```

В каталоге `jade` хранятся jade-шаблоны страниц, собранные из блоков, которые описаны в каталоге blocks.
Блоки в папке blocks лежат структурировано, согласно [методологии разделения стилей по слоям](): foundation, base, project, cosmetic.  
**Структура каталога source/blocks/foundation/:**
```
foundation/
    _mixins.styl
    _variables.styl
    fonts.styl
    normalize.styl
    typography.styl
    main.js
```
Каталоги base и project состоят из отдельных папок, каждая из которых полностью описывает конкретный блок. Название папки конкретного блока совпадает с названием css-класса этого блока.

**Пример одного блока в каталоге source/blocks/base/:**
```
base/
    btn/
        i/
            icon-info.svg
            icon-error.svg
        _btn.jade
        btn.jade
        btn.styl
        btn.js
        
```
`i/` - содержит изображения, относящиеся к блоку.  
`_btn.jade` - миксин блока (имя файла должно начинаться с символа “_”). Данный файл не обязательно должен присутствовать в структуре блока. Он нужен тогда, когда блок может использоваться на странице/ах с разными параметрами;  
`btn.jade` - шаблон блока, используется для подключения блока на страницу с помощью `include`. Если у блока есть миксин `_btn.jade`, то содержимое `btn.jade` - это вызов данного миксина. Иначе - это обычный jade-шаблон.  
`btn.styl` - стили блока;  
`btn.js` - javascript блока;  

Пример использования блоков при верстке страниц (/source/jade/home.jade):
```jade
extends _layout.jade
block content
    +btn('Подключить') // подключение блока с помощью миксина
	include ../blocks/base/btn/btn.jade // подключение блока с помощью include
```

### Структура каталога build:
Каталог build содержит скомпилированные html-страницы и статику.
```
build/
    f/
        css/
            foundation.css
            base.css
            project.css
            cosmetic.css
        fonts/
        js/
            maim.js
        i/
            btn/
                icon-info.svg
                icon-error.svg
            sprite-icons.png
            sprite-icons@2x.png
    html/
        home.html
        news.html
        ...
```

# Соглашение по именованию CSS-классов
Для именования CSS-классов используется БЭМ-методология.

## CSS-селекторы
* Для хранения информации об именах блоков, элементов и модификаторов используются CSS-классы.
* Имена классов записываются с помощью латинских букв и цифр в нижнем регистре.
* Имя блока должно быть осмысленным английским словом, отражающим его сущность. Допускаются сокращения слов (btn, desc и т.п.).
* Не допускается использование цифр, как модификаторов блока, кроме случаев, когда модификатор явно не обозначает порядковый номер. То есть вместо `_icon-1`, `_icon-2` должно быть `_icon-{NAME-ICON1}`, `_icon-{NAME-ICON2}`.
* Имя БЭМ-сущностей может состоять из нескольких слов. Для разделения слов в именах используется дефис (-).

## Правила формирования имен
### Имя блока
Имя блока должно отражать его сущность. Формируется по схеме **block-name** и задает пространство имен для элементов и модификаторов.  
Например: `news`  
### Имя элемента
Имя элемента отделяется от имени блока двумя подчеркиваниями (__). Полное имя элемента создается по схеме **block-name__elem-name**.  
Например: `news__date`.  
*Важно!* Не существует элементов элементов.  
Не правильно: `menu__item__link`  
Правильно: `menu__link` или `menu__item-link`
### Имя модификатора
Модификатор может принадлежать как блоку, так и элементу. Имя модификатора отделяется от имени блока или элемента одним подчеркиванием (\_).  
Модификаторы могут быть *булевыми* или вида «*ключ-значение*».  
Полное имя модификатора создается по схеме:  
* Для булевых модификаторов — **owner-name_mod-name**.
* Для модификаторов вида «ключ-значение» — **owner-name_mod-name_mod-val**.  

##### Модификатор блока
**Булевый модификатор**. Значение такого модификатора не указывается. Полное имя создается по схеме: **block-name_mod-name**.  
Пример: `menu_tablet`  
**Модификатор типа «ключ — значение»**. Значение модификатора отделяется от его названия одним подчеркиванием (\_). Полное имя создается по схеме: **block-name_mod-name_mod-val**.  
Пример: `menu_color_red`  
##### Модификатор элемента
**Булевый модификатор**. Значение такого модификатора не указывается. Полное имя создается по схеме: **block-name__elem-name_mod-name**.  
Пример: `menu__item_active`  
**Модификатор типа «ключ — значение»**. Значение модификатора отделяется от имени одним подчеркиванием (\_). Полное имя создается по схеме: **block-name__elem-name_mod-name_mod-val**.  
Пример: `menu__item_type_radio`

## Пример использования
Реализация блока «Новость» в HTML и CSS:  
**HTML:**  
```html
<div class="news news_narrow">
  <img class="news__image" src="news-image.jpg">
  <div class="news__title">
    <a class="news__title-link" href="">Заголовок новости</a>
  </div>
  <div class="news__desc">Описание новости</div>
</div>
```
**CSS:**
```css
.news {...}
.news_narrow {...}
.news__image {...}
.news__title {...}
.news__title-link {...}
.news__desc {...}
```

# Система организации CSS
Система организации CSS основана на принципах MCSS (многослойный CSS).  
Вёрстка страниц делится на блоки, каждый из которых сам по себе независим и максимально абстрагирован от фиксированных размеров и местоположения. В свою очередь блоки распределяются по слоям, каждый из которых имеет свой свод правил использования и взаимодействия с другими блоками.  
Предполагается разделение всех стилей на 4 слоя + 1 псевдослой:  

1.  Foundation (normalize.css, стили тегов).
2.  Base (стили базовых блоков).
3.  Project (стили остальных блоков).
4.  Cosmetic (косметические стили).
5.  Context (контекст) не выделяется в отдельный слой, может располагаться по всем слоям.

## Слой foundation
Закладывает основу стилей. Данный слой содержит normalize.css и typography.css
В typography.css находятся стили для тегов типографики согласно гайдлайну сайта (`<h1>, …, <h6>, <p>, <ul>, <ol>, <blockquote> `и т.п.). В него же входят стили по-умолчанию для ссылок (`<a>`), начертание и размер шрифта для `<body>`. Typography.css позволит формировать простые статьи на сайте согласно гайдлайну, используя теги без классов.  
В foundation.css нельзя использовать селекторы классов. Foundation.css подключается самым первым на всех страницах.

## Слой base
Базовый слой - это стилевой фреймфорк, который состоит из часто используемых блоков. К таким блокам относятся кнопки, текстовые поля, чекбоксы, радиокнопки, выпадающие списки, формы, всплывающие подсказки, различные виды маркированных/нумерованных списков, .clearfix и другие. К base.css также относится мультиколоночная сетка.  
Основные критерии, по которым можно отнести тот или иной блок к base.css:
* часто используется на разных страницах сайта;
* небольшой;
* не может содержать других блоков внутри себя (не всегда может соблюдаться).

Такие, часто используемые блоки, как шапка, подвал не относятся к слою base.
### Правила базового слоя
Основное правило базового слоя — абстрактность как в названиях, так и в разметке. Названия классов этого слоя не должны выглядеть чужеродно в любом месте на сайте. Блоки должны иметь стиль по умолчанию, но должны оставаться простыми и легко модифицируемыми под задачи различных проектных блоков.  
Стили базового слоя могут быть модифицированы каскадом от других блоков этого же слоя и от блоков проектного слоя.
Например:  
В base.css
```
.form-field .btn { }
```
`.form-field` - контейнер для поля формы (слой base), `.btn` - кнопка (слой base)  

В project.css  
```
.news .btn { }
```
`.news` - блок новости (слой project), `.btn` - кнопка (слой base)  

Модификация проектного слоя от базового запрещена!  
`.form-field .news {}` не правильно!  
В таких случаях необходимо использовать модификатор блока `.news`
```
.news { }
.news_in-form { }
```
Base.css подключается после foundation.css на всех страницах сайта.  

## Слой project
Проектный слой содержит стили отдельных блоков сайта, не вошедших в базовый слой. Примеры блоков проектного слоя: шапка, подвал, блок новости, корзина и др. Каждый блок должен быть максимально изолирован и существовать как отдельная, независимая единица интерфейса.  
В html-коде проектного блока можно использовать блоки базового слоя. При этом в CSS коде можно каскадом модифицировать блок базового слоя.
```html
<div class=”product-card”>
  <button class=”btn”>Купить<button>
</div>
```
```css
.product-card .btn { }
```
Стили проектного слоя могут быть модифицированы каскадом только от других блоков этого слоя.  
*Данный файл стилей может собираться не из всех блоков, а только из тех, которые используется на конкретной странице.*
Project.css подключается после базового слоя.

## Слой cosmetic
В данном слое содержатся вспомогательные, простые, незначительно влияющие на внешний вид стили. В косметическом слое допускается использование !important.  
Например:  
```css
.mt-10 {margin-top: 10px !important;}
.pb-5 {padding-bottom: 10px !important;}
.fs-12 {font-size: 12px;}
.tac {text-align: center;}
.fl {float: left;}
```
Стили таких классов не могут менять свое значение с течением времени. Также их нельзя модифицировать каскадом от других слоёв.  
Стили косметического слоя должны быть построены так, чтобы при их удалении вёрстка не ломалась. Максимум могут теряться какие-то незначительные стили — размер шрифта, отступы и пр.  
Слой cosmetic позволяет решить проблему с дублированием стилей и описывать редкие, непроектные ситуации, не плодя лишние модификаторы блоков. Однако такие стили следует как можно реже использовать при верстке отдельных блоков.  
Cosmetic.css подключается самым последним файлом стилей на всех страницах сайта.

## Слой context
Слой включает в себя стили высшего контекста и @media-правила, которые могут использоваться для изменения стандартных стилей под особенности различного контекста:
* `.ie9` — браузеры
* `.touch`, `.no-boxshadow` — особенности браузера или устройства
* Media queries

Слой контекста является исключением из правил расположения стилей. Стили этого слоя не выделяются в отдельный файл, а располагаются по всем слоям, рядом с селекторами, которые каскадом модифицируются от контекста.
```css
.menu { }
.ie9 .menu { }
@media screen and (max-width: 1000px) {
  .menu { }
}
```
Контекстные стили могут модифицировать каскадом любые стили других слоев, кроме стилей косметического слоя.

### Добавление нового блока в верстку
1.  Определите к какому из слоев (foundation, base, project или cosmetic) относится новый блок. Чаще всего это либо base, либо project
2.  В каталоге `/source/blocks/НАЗВАНИЕ_СЛОЯ/` создайте папку с названием блока. Название блока совпадает с его CSS-классом.
3.  В созданную папке блока добавьте следующие файлы:  
    * `_block.jade` - миксин блока. Данный файл не обязательно должен присутствовать в структуре блока. Он нужен тогда, когда блок может использоваться на странице/ах с разными параметрами;
    * `block.jade` - шаблон блока, используется для подключения блока на страницу с помощью `include`. Если у блока есть миксин `_block.jade`, то содержимое `block.jade` - это вызов данного миксина. Иначе - это обычный jade-шаблон.
    * `block.styl` - стили блока *(не обязательный)*;
    * `block.js` - javascript блока *(не обязательный)*;
    * каталог `i/` - содержит изображения, относящиеся к блоку *(не обязательный)*;
    * каталог `i/sprite/` - содержит изображения, которые попадут в общий **png**-спрайт `build/f/i/sprite-icons.png`;
    * каталог `i/sprite@2x/` - содержит изображения, которые попадут в общий **png**-спрайт `build/f/i/sprite-icons@2x.png`;
    * каталог `i/sprite-svg/` - содержит изображения, которые попадут в общий **svg**-спрайт;  
    , где `block` - имя блока.
4.  Подключите блок в нужном jade-шаблоне с помощью `mixin` или `include`.  
    Например в `/source/home.jade`:
    ```jade
    +news('Это первая новость')
    ```
    или
    ```jade
    include ../blocks/project/news/news.jade
    ```

### Рассмотрим содержимое одного блока на примера блока news
#### `_news.jade`
```jade
mixin news(title, date, desc)
    news
        news__title
            a(href="")= title
        news__date= date
        news__desc!= desc
```
#### `news.jade`
```jade
block news
    +news('Заголовок новости', '10.05.2016', 'Краткое описание новости')
```
#### `news.styl`
```
@import '../../foundation/_import' //подключаем переменные и миксины
.news
    border: 1px solid #eee;
    &__title
        font-size: 20px;
    &__date
        font-size: 13px;
```
#### `news.js`
```javascript
// подключение необходимых модулей
var App = require('./App.js'); 
var bxSlider = require('jquery.bxslider');
// js блока news
console.log('Здесь javascript для блока news')
```
подключите `news.js` в `source/foundation/main.js`
```javascript
require('./mainmenu.js');
```
### Подключение нового js-плагина
...
### Изображения
Предпочтительный формат для иконок - svg.  
Svg-изображение можно использовать либо как фоновое изображение, либо как inline-svg.  
**Пример использования фонового svg в news.styl:**
```
.news__icon-internet
    background-image: url("../i/news/icon-internet.svg#datauri");
```
если указать `#datauri` после расширения изображения, то оно будет перекодировано в base64-формат.

**Пример использования inline-svg в news.jade:**  
```
svg.news__icon-internet
    use(xlink:href="#news-icon-internet")
```
При этом для генерации svg-спрайта в папке `source/blocks/project/news/i` необходимо создать папку `/sprite-svg`. В эту папку складываем все svg-изображения, которые хотим использовать inline-способом. Имя иконки, которая попадет в спрайт, должно начинаться с имени блока плюс “-” (в данном примере это `news-icon-internet`). Сам спрайт должен быть вставлен в html-документ как `<svg>`.

В случаях, когда svg-формат не подходит, возможно использование png, jpg или gif форматов. Небольшие png-картинки следует собирать в спрайт. Для этого в папке блока создаем папки `/i/sprite` и `/i/sprite@2x`. В первую папку кладем обычные иконки, во вторую иконки равно в 2 раза больше обычных. Имя иконки, которая попадет в спрайт, должно начинаться с имени блока плюс “-”. Имена соответствующих иконок в обоих папках совпадают.

**Пример использования png из спрайта с учетом retina:**  
```
.news__icon-internet
	sprite($news-icon-internet);
```
, где `news-icon-internet` - имя png-изображения.

**Пример использования png без спрайта:**  
```
.news__icon-internet
	background: url("../i/news/news-icon-internet.png#datauri") repeat-x 0 0;
```
если указать “#datauri” после расширения изображения, то оно будет перекодировано в base64-формат.

