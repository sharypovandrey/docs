---
editable: false
---

# REGEXP_MATCH

_Строковые функции_

#### Синтаксис


```
REGEXP_MATCH( string, pattern )
```

#### Описание
Возвращает `TRUE`, если в строке `string` есть подстрока, которая соответствует шаблону регулярного выражения `pattern`.

**Типы аргументов:**
- `string` — `Строка`
- `pattern` — `Строка`


**Возвращаемый тип**: `Логический`

{% note info %}

Информацию о синтаксисе регулярных выражений уточняйте в документации к источникам данных.

{% endnote %}

Для материализованных датасетов шаблоны описываются в синтаксисе [ClickHouse](https://github.com/google/re2/wiki/Syntax).



#### Примеры

```
REGEXP_MATCH("RU 912873","\w\s\d") = TRUE
```


#### Поддержка источников данных

`Материализованный датасет`, `ClickHouse 1.1`, `MySQL 5.6`, `PostgreSQL 9.3`.
