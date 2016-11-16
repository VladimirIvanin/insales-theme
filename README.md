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

- `media` - тут всё что не liquid. (важно css и картинки лежат в одном месте потому в таких случаях как background-image: url('main.jpg') ссылка на файл будет указна от корня, папки на платформе создавать нельзя)
- `snippets` - тут сниппеты (Они вставляются в темплейты, могут принимать параметры)
- `templates` - тут шаблоны, имена шаблонов стандартизированы иначе не будет работать. Далее опишу как их именовать.
- `config` - тут конфиги платформы

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
