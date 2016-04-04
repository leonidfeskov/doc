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



