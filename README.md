# insales-theme
Рекомендации по структуре проекта темы на платформе InSales

- [Особенности шаблонов на InSales](#Особенности-шаблонов-на-insales)
- [Подключение стилей и скриптов](#Подключение-стилей-и-скриптов)
- [Как пользоваться сниппетами](https://github.com/VladimirIvanin/insales-theme/blob/master/README.md#Как-пользоваться-сниппетами)

## Особенности шаблонов на InSales

Темы InSales состоят из liquid-файлов, каждый из которых служит для определенных задач. Например, collection.liquid используется для отображения товаров внутри категории, а product.liquid - для отображения информации о конкретном товаре.

Структура папок:
```
root/
    |-- config/
    |-- media/
    |-- snippets/
    |-- templates/
```
### media
Эта директория содержит ассеты, используемые в теме, в том числе изображения, стили, скрипты, svg-файлы (css, js и svg-файлы из директории media можно править с помощью редактора шаблонов в бек-офисе магазина). Для обращения к файлам из этой директории из шаблонов используйте фильтр asset_url (важно! css и картинки лежат в одном месте потому в таких случаях как background-image: url('./images/main.jpg') ссылка на файл указывается от корневой директории `background-image: url('main.jpg')`, папки на платформе создавать нельзя)

### snippets
Директория содержит Liquid-снипеты с кусочками кода, используемого в разных шаблонах /  в разных частях сайта. Используйте include для включения снипета в шаблон. 

### templates
Эта директория содержит файлы Liquid-шаблонов для основных типов страниц сайта, а также шаблоны лэйаутов, определяющий общую часть оформления и вёрстки страниц. По умолчанию в теме присутствует 2 лэйаута - layouts.layout.liquid (для всех страниц сайта, кроме личного кабинета и шаго оформления заказа) и layouts.checkout.liquid  (для страниц оформления заказа).

### config
тут файлы конфигурации темы

Нейминг шаблонов:

- Главный лэйаут - layouts.layout.liquid (Все остальные шаблоны не являющиеся лэйаутами наследуют содержимое данного файла. Контент шаблонов будет вставлен на место тега `{{ content_for_layout }}`)
- Главная - index.liquid
- Категория - collection.liquid
- Товар - product.liquid
- Корзина - cart.iquid
- Страница  page.liquid
- Поиск search.liquid
- Блог  blog.liquid
- Статья  article.liquid  

Кастомные шаблоны для любой страницы можно сделать, со следующим неймингом:

Сначала идет имя лэйаута, потом кастомное имя.

- `product.hits.liquid`
- `product.new.liquid`
- `blog.news.liquid`
- `page.contacts.liquid `

## Подключение стилей и скриптов

Для подключения стоит использовать внутренний функционал платформы для склеивания файлов.

Для стилей нужно пользоваться директивой препроцессора sass — `@import` или внутренней директивой `#= require`

Пример plugins.css:
```
#= require jquery.min
#= require magiczoomplus.min
#= require normalize.min
#= require swiper.min
#= require alertify.min
#= require magnific-popup.min
```

Пример main.scss:
```
@import 'header';
@import 'footer';
@import 'slider';
```
> Важно! Файлы header, footer, slider могут быть как `scss` так и `css`.

> Используя `@import` и `#= require` можно игнорировать расширение.

> Для удобности в файлы относящиеся к main.scss именуют с префиксом `_`, при импорте префикс игнорируется.

> main.scss компилируется в main.css, в тему подключается скомпилированный или склеенный main.scss.

Подключение js происходит аналогично, только используется директива `#= require`.

> Js файлы так же разделяются на два файла `plugins` и `main`.

Пример main.js:

```
#= require cart
#= require product
#= require collection
```
Подключение скриптов и стилей разделяется сниппетами, сниппет для стилей `styles.liquid` для скриптов `scripts.liquid`.

Сниппеты подключаются в главный лэйаут `layouts.layout.liquid`.

Пример `layouts.layout.liquid`:
```html
<!DOCTYPE html>
<html>

<head>

  {% include 'head' %}

  {% include 'styles' %}

</head>
<body class="adaptive">

<div class="page-wrapper">

  <div class="page-inner container">

    {% include 'header' %}

    {{ content_for_layout }}

  </div>

  {% include 'footer' %}

</div>

  {% include 'modals' %}

  {% include 'scripts' %}

</body>
</html>
```

Содержимое styles.liquid:
```
<link rel="stylesheet" href="{{ 'plugins.css' | asset_url }}">
<link rel="stylesheet" href="{{ 'main.css' | asset_url }}">
```

Содержимое scripts.liquid:
```
<script type="text/javascript" src="{{ 'plugins.js' | asset_url }}"></script>
<script type="text/javascript" src="{{ 'main.js' | asset_url }}"></script>

```

## Как пользоваться сниппетами

Сниппеты в программировании — это небольшие фрагменты кода которые обычно повторно используются в коде программы ([Статья на википедии](https://ru.wikipedia.org/wiki/%D0%A1%D0%BD%D0%B8%D0%BF%D0%BF%D0%B5%D1%82)).

Сниппеты на платформе InSales хранят в себе html код разметки и код написанный на шаблонизаторе `liquid`.

Сниппеты включаются в шаблоны через директиву `include`.

Пример:
```liquid
Пример включения сниппета без передачи параметров.
{% include 'header' %}
```

```liquid
Пример включения сниппета с передачей строки в виде параметра
{% include 'header' with "index" %}
При таком включении внутри сниппета header, параметр будет доступен в одноименной переменной {{ header }}

Пример кода сниппета header
{% if header == 'index' %}
# код для включения с параметром - index
{% else %}
# код обычного включения
{% endif %}
```

```html
Пример включения сниппета с передачей нескольких параметров:
{% include 'logo', use_image: false, logo_text: 'Моя компания' %}
При таком виде включения, все передаваемые переменные стоит обнулять в конце кода сниппета

Пример кода сниппета logo
{% if use_image %}
  <img src="{{ 'logo.svg' | asset_url }}" />
{% else %}
  {{ logo_text }}
{% endif %}
{% assign use_image = null %}
{% assign logo_text = null %}
```
Сниппеты могут содержать в себе включение других сниппетов.

Пример сниппета `header`:

```liquid
<div class="container">
  <div class="row">
    <div class="cell-2">
      {% include "logo", modificator: 'in-header', use_image: true %}
    </div>
    <div class="cell-6">
      {% include "menu", modificator: 'in-header', menu_class: 'main-menu', source_type: 'collection', source_handle: 'all', show_icon: true %}
    </div>
    <div class="cell-2">
      {% include "search", modificator: 'in-header' %}
    </div>
    <div class="cell-2">
      {% include "cart_widget", modificator: 'in-header' %}
    </div>
  </div>
</div>
```
