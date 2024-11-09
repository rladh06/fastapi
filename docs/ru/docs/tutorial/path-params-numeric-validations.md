# Path-параметры и валидация числовых данных

Так же, как с помощью `Query` вы можете добавлять валидацию и метаданные для query-параметров, так и с помощью `Path` вы можете добавлять такую же валидацию и метаданные для path-параметров.

## Импорт Path

Сначала импортируйте `Path` из `fastapi`, а также импортируйте `Annotated`:

//// tab | Python 3.10+

```Python hl_lines="1  3"
{!> ../../docs_src/path_params_numeric_validations/tutorial001_an_py310.py!}
```

////

//// tab | Python 3.9+

```Python hl_lines="1  3"
{!> ../../docs_src/path_params_numeric_validations/tutorial001_an_py39.py!}
```

////

//// tab | Python 3.8+

```Python hl_lines="3-4"
{!> ../../docs_src/path_params_numeric_validations/tutorial001_an.py!}
```

////

//// tab | Python 3.10+ без Annotated

/// tip | Подсказка

Рекомендуется использовать версию с `Annotated` если возможно.

///

```Python hl_lines="1"
{!> ../../docs_src/path_params_numeric_validations/tutorial001_py310.py!}
```

////

//// tab | Python 3.8+ без Annotated

/// tip | Подсказка

Рекомендуется использовать версию с `Annotated` если возможно.

///

```Python hl_lines="3"
{!> ../../docs_src/path_params_numeric_validations/tutorial001.py!}
```

////

/// info | Информация

Поддержка `Annotated` была добавлена в FastAPI начиная с версии 0.95.0 (и с этой версии рекомендуется использовать этот подход).

Если вы используете более старую версию, вы столкнётесь с ошибками при попытке использовать `Annotated`.

Убедитесь, что вы [обновили версию FastAPI](../deployment/versions.md#fastapi_2){.internal-link target=_blank} как минимум до 0.95.1 перед тем, как использовать `Annotated`.

///

## Определите метаданные

Вы можете указать все те же параметры, что и для `Query`.

Например, чтобы указать значение метаданных `title` для path-параметра `item_id`, вы можете написать:

//// tab | Python 3.10+

```Python hl_lines="10"
{!> ../../docs_src/path_params_numeric_validations/tutorial001_an_py310.py!}
```

////

//// tab | Python 3.9+

```Python hl_lines="10"
{!> ../../docs_src/path_params_numeric_validations/tutorial001_an_py39.py!}
```

////

//// tab | Python 3.8+

```Python hl_lines="11"
{!> ../../docs_src/path_params_numeric_validations/tutorial001_an.py!}
```

////

//// tab | Python 3.10+ без Annotated

/// tip | Подсказка

Рекомендуется использовать версию с `Annotated` если возможно.

///

```Python hl_lines="8"
{!> ../../docs_src/path_params_numeric_validations/tutorial001_py310.py!}
```

////

//// tab | Python 3.8+ без Annotated

/// tip | Подсказка

Рекомендуется использовать версию с `Annotated` если возможно.

///

```Python hl_lines="10"
{!> ../../docs_src/path_params_numeric_validations/tutorial001.py!}
```

////

/// note | Примечание

Path-параметр всегда является обязательным, поскольку он составляет часть пути.

Поэтому следует объявить его с помощью `...`, чтобы обозначить, что этот параметр обязательный.

Тем не менее, даже если вы объявите его как `None` или установите для него значение по умолчанию, это ни на что не повлияет и параметр останется обязательным.

///

## Задайте нужный вам порядок параметров

/// tip | Подсказка

Это не имеет большого значения, если вы используете `Annotated`.

///

Допустим, вы хотите объявить query-параметр `q` как обязательный параметр типа `str`.

И если вам больше ничего не нужно указывать для этого параметра, то нет необходимости использовать `Query`.

Но вам по-прежнему нужно использовать `Path` для path-параметра `item_id`. И если по какой-либо причине вы не хотите использовать `Annotated`, то могут возникнуть небольшие сложности.

Если вы поместите параметр со значением по умолчанию перед другим параметром, у которого нет значения по умолчанию, то Python укажет на ошибку.

Но вы можете изменить порядок параметров, чтобы параметр без значения по умолчанию (query-параметр `q`) шёл первым.

Это не имеет значения для **FastAPI**. Он распознает параметры по их названиям, типам и значениям по умолчанию (`Query`, `Path`, и т.д.), ему не важен их порядок.

Поэтому вы можете определить функцию так:

//// tab | Python 3.8 без Annotated

/// tip | Подсказка

Рекомендуется использовать версию с `Annotated` если возможно.

///

```Python hl_lines="7"
{!> ../../docs_src/path_params_numeric_validations/tutorial002.py!}
```

////

Но имейте в виду, что если вы используете `Annotated`, вы не столкнётесь с этой проблемой, так как вы не используете `Query()` или `Path()` в качестве значения по умолчанию для параметра функции.

//// tab | Python 3.9+

```Python hl_lines="10"
{!> ../../docs_src/path_params_numeric_validations/tutorial002_an_py39.py!}
```

////

//// tab | Python 3.8+

```Python hl_lines="9"
{!> ../../docs_src/path_params_numeric_validations/tutorial002_an.py!}
```

////

## Задайте нужный вам порядок параметров, полезные приёмы

/// tip | Подсказка

Это не имеет большого значения, если вы используете `Annotated`.

///

Здесь описан **небольшой приём**, который может оказаться удобным, хотя часто он вам не понадобится.

Если вы хотите:

* объявить query-параметр `q` без `Query` и без значения по умолчанию
* объявить path-параметр `item_id` с помощью `Path`
* указать их в другом порядке
* не использовать `Annotated`

...то вы можете использовать специальную возможность синтаксиса Python.

Передайте `*` в качестве первого параметра функции.

Python не будет ничего делать с `*`, но он будет знать, что все следующие параметры являются именованными аргументами (парами ключ-значение), также известными как <abbr title="From: K-ey W-ord Arg-uments"><code>kwargs</code></abbr>, даже если у них нет значений по умолчанию.

```Python hl_lines="7"
{!../../docs_src/path_params_numeric_validations/tutorial003.py!}
```

### Лучше с `Annotated`

Имейте в виду, что если вы используете `Annotated`, то, поскольку вы не используете значений по умолчанию для параметров функции, то у вас не возникнет подобной проблемы и вам не придётся использовать `*`.

//// tab | Python 3.9+

```Python hl_lines="10"
{!> ../../docs_src/path_params_numeric_validations/tutorial003_an_py39.py!}
```

////

//// tab | Python 3.8+

```Python hl_lines="9"
{!> ../../docs_src/path_params_numeric_validations/tutorial003_an.py!}
```

////

## Валидация числовых данных: больше или равно

С помощью `Query` и `Path` (и других классов, которые мы разберём позже) вы можете добавлять ограничения для числовых данных.

В этом примере при указании `ge=1`, параметр `item_id` должен быть больше или равен `1` ("`g`reater than or `e`qual").

//// tab | Python 3.9+

```Python hl_lines="10"
{!> ../../docs_src/path_params_numeric_validations/tutorial004_an_py39.py!}
```

////

//// tab | Python 3.8+

```Python hl_lines="9"
{!> ../../docs_src/path_params_numeric_validations/tutorial004_an.py!}
```

////

//// tab | Python 3.8+ без Annotated

/// tip | Подсказка

Рекомендуется использовать версию с `Annotated` если возможно.

///

```Python hl_lines="8"
{!> ../../docs_src/path_params_numeric_validations/tutorial004.py!}
```

////

## Валидация числовых данных: больше и меньше или равно

То же самое применимо к:

* `gt`: больше (`g`reater `t`han)
* `le`: меньше или равно (`l`ess than or `e`qual)

//// tab | Python 3.9+

```Python hl_lines="10"
{!> ../../docs_src/path_params_numeric_validations/tutorial005_an_py39.py!}
```

////

//// tab | Python 3.8+

```Python hl_lines="9"
{!> ../../docs_src/path_params_numeric_validations/tutorial005_an.py!}
```

////

//// tab | Python 3.8+ без Annotated

/// tip | Подсказка

Рекомендуется использовать версию с `Annotated` если возможно.

///

```Python hl_lines="9"
{!> ../../docs_src/path_params_numeric_validations/tutorial005.py!}
```

////

## Валидация числовых данных: числа с плавающей точкой, больше и меньше

Валидация также применима к значениям типа `float`.

В этом случае становится важной возможность добавить ограничение <abbr title="greater than"><code>gt</code></abbr>, вместо <abbr title="greater than or equal"><code>ge</code></abbr>, поскольку в таком случае вы можете, например, создать ограничение, чтобы значение было больше `0`, даже если оно меньше `1`.

Таким образом, `0.5` будет корректным значением. А `0.0` или `0` — нет.

То же самое справедливо и для <abbr title="less than"><code>lt</code></abbr>.

//// tab | Python 3.9+

```Python hl_lines="13"
{!> ../../docs_src/path_params_numeric_validations/tutorial006_an_py39.py!}
```

////

//// tab | Python 3.8+

```Python hl_lines="12"
{!> ../../docs_src/path_params_numeric_validations/tutorial006_an.py!}
```

////

//// tab | Python 3.8+ без Annotated

/// tip | Подсказка

Рекомендуется использовать версию с `Annotated` если возможно.

///

```Python hl_lines="11"
{!> ../../docs_src/path_params_numeric_validations/tutorial006.py!}
```

////

## Резюме

С помощью `Query`, `Path` (и других классов, которые мы пока не затронули) вы можете добавлять метаданные и строковую валидацию тем же способом, как и в главе [Query-параметры и валидация строк](query-params-str-validations.md){.internal-link target=_blank}.

А также вы можете добавить валидацию числовых данных:

* `gt`: больше (`g`reater `t`han)
* `ge`: больше или равно (`g`reater than or `e`qual)
* `lt`: меньше (`l`ess `t`han)
* `le`: меньше или равно (`l`ess than or `e`qual)

/// info | Информация

`Query`, `Path` и другие классы, которые мы разберём позже, являются наследниками общего класса `Param`.

Все они используют те же параметры для дополнительной валидации и метаданных, которые вы видели ранее.

///

/// note | Технические детали

`Query`, `Path` и другие "классы", которые вы импортируете из `fastapi`, на самом деле являются функциями, которые при вызове возвращают экземпляры одноимённых классов.

Объект `Query`, который вы импортируете, является функцией. И при вызове она возвращает экземпляр одноимённого класса `Query`.

Использование функций (вместо использования классов напрямую) нужно для того, чтобы ваш редактор не подсвечивал ошибки, связанные с их типами.

Таким образом вы можете использовать привычный вам редактор и инструменты разработки, не добавляя дополнительных конфигураций для игнорирования подобных ошибок.

///