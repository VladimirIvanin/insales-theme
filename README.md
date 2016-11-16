# insales-theme
Рекомендации по структуре проекта темы на платформе InSales

## Особенности шаблонов на InSales

Структура папок:
```
root/
    |-- config/
    |-- media/
    |-- snippets/
    |-- templates/
```

- `media` - тут всё что не liquid. (важно css и картинки лежат в одном месте потому в таких случаях как background-image: url('./images/main.jpg') ссылка на файл указывается от корневой директории `background-image: url('main.jpg')`, папки на платформе создавать нельзя)
- `snippets` - тут сниппеты (Они вставляются в темплейты, могут принимать параметры)
- `templates` - тут шаблоны, имена шаблонов стандартизированы иначе не будет работать.
- `config` - тут файлы конфигурации темы

Нейминг шаблонов:

- Главный лэйаут - layouts.layout.liquid
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

Для стилей нужно пользоваться директивой препроцессора sass — `@import` и внутренней директивой `#= require`

Пример plugin.css:
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

