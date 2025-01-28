
## Примеры API

- [GitHub-репозиторий](https://github.com/public-apis/public-apis) с большим количеством публичных API
- [Статья на Хабре](https://habr.com/ru/company/macloud/blog/562700/) со списком "интересных (и забавных)" API

## Фильтры Aiogram

Фильтры можно записать таким образом:
```python
@dp.message(CommandStart())

@dp.message(F.text.lower().in_(['да', 'давай', 'сыграем', 'игра', 'играть', 'хочу играть']))

@dp.message(F.text.lower().in_(['нет', 'не', 'не хочу', 'не буду']))

@dp.message(lambda x: x.text and x.text.isdigit() and 1 <= int(x.text) <= 100)
```

