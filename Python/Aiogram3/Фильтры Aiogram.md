

Фильтры можно записать таким образом:
```python
@dp.message(CommandStart())

@dp.message(F.text.lower().in_(['да', 'давай', 'сыграем', 'игра', 'играть', 'хочу играть']))

@dp.message(F.text.lower().in_(['нет', 'не', 'не хочу', 'не буду']))

@dp.message(lambda x: x.text and x.text.isdigit() and 1 <= int(x.text) <= 100)
```

## Магические фильтры

Довольно удобной фишкой `aiogram3` считаются, так называемые, [магические фильтры](https://docs.aiogram.dev/en/dev-3.x/dispatcher/filters/magic_filters.html#magic-filters). Это способ записывать фильтры через `F` - экземпляр класса `MagicFilter`. Через `F` можно удобно составлять фильтры, обращаясь к атрибутам объектов. Мы ими уже пользовались, когда писали первых ботов. Сейчас еще раз приведу парочку примеров, а дальше расскажу как это работает. Импорт довольно тривиальный:

```python
from aiogram import F
```

**Примеры фильтров через F:**

```python
F.photo                                    # Фильтр для фото
F.voice                                    # Фильтр для голосовых сообщений
F.content_type.in_({ContentType.PHOTO,
                    ContentType.VOICE,
                    ContentType.VIDEO})    # Фильтр на несколько типов контента
F.text == 'привет'                         # Фильтр на полное совпадение текста
F.text.startswith('привет')                # Фильтр на то, что текст сообщения начинается с 'привет'
~F.text.endswith('bot')                    # Инвертирование результата фильтра
```

Мы уже записывали с вами фильтры через анонимную функцию `lambda`, подразумевая, что на вход ей подается апдейт, а на выходе `True/False`. Вот так, например, можно записывать фильтры с помощью анонимной функции.

**Примеры фильтров через lambda:**

```python
lambda message: message.photo                        # Фильтр для фото
lambda message: message.voice                        # Фильтр для голосовых сообщений
lambda message: message.content_type in {ContentType.PHOTO,
                                         ContentType.VOICE,
                                         ContentType.VIDEO}   # Фильтр на несколько типов контента
lambda message: message.text == 'привет'             # Фильтр на полное совпадение текста
lambda message: message.text.startswith('привет')    # Фильтр на то, что текст сообщения начинается с 'привет'
lambda message: not message.text.endswith('bot')   # Инвертирование результата фильтра
```

Если внимательно посмотреть на эти две группы примеров - не очень сложно догадаться как работает `F`. Ведь по сути, это просто упрощенная запись для анонимных функций. Фильтры становятся лаконичнее и получают некоторый дополнительный функционал, по сравнению с `lambda`.

Вместо `lambda <параметр>: <параметр>` пишется просто `F`. Дальше через точку может идти обращение к последовательности атрибутов, переданного в `F` аргумента, и какие-то действия с этой последовательностью.

Еще немного практических примеров, чтобы лучше понять как работают магические фильтры.

**Пример 1**. Фильтр, который будет пропускать только апдейты от пользователя с ID = 173901673.

Через `lambda`:

```python
lambda message: message.from_user.id == 173901673
```

Через `F`:

```python
F.from_user.id == 173901673
```

**Пример 2.** Фильтр, который будет пропускать только апдейты от админов из списка 193905674, 173901673, 144941561.

Через `lambda`:

```python
lambda message: message.from_user.id in {193905674, 173901673, 144941561}
```

Через `F`:

```python
F.from_user.id.in_({193905674, 173901673, 144941561})
```

**Пример 3.** Фильтр, который будет пропускать апдейты текстового типа, кроме тех, которые начинаются со слова "Привет".

Через `lambda`:

```python
lambda message: not message.text.startswith('Привет')
```

Через `F`:

```python
~F.text.startswith('Привет')
```

**Пример 4.** Фильтр, который будет пропускать апдейты любого типа, кроме фото, видео, аудио и документов.

Через `lambda`:

```python
lambda message: not message.content_type in {ContentType.PHOTO,
                                             ContentType.VIDEO,
                                             ContentType.AUDIO,
                                             ContentType.DOCUMENT}
```

Через `F`:

```python
~F.content_type.in_({ContentType.PHOTO,
                     ContentType.VIDEO,
                     ContentType.AUDIO,
                     ContentType.DOCUMENT})
