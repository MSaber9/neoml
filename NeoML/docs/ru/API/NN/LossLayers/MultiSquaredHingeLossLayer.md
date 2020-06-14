# Класс CMultiSquaredHingeLossLayer

<!-- TOC -->

- [Класс CMultiSquaredHingeLossLayer](#класс-cmultisquaredhingelosslayer)
    - [Настройки](#настройки)
        - [Вес слоя](#вес-слоя)
        - [Урезание градиентов](#урезание-градиентов)
    - [Обучаемые параметры](#обучаемые-параметры)
    - [Входы](#входы)
    - [Выходы](#выходы)
        - [Значение функции потерь](#значение-функции-потерь)

<!-- /TOC -->

Класс реализует слой, вычисляющий модифицированную функцию потерь `SquaredHinge` для многоклассовой классификации.

Формула:

```c++
loss = -4 * (x_right - x_max_wrong)                при x_right - x_max_wrong < -1
loss = sqr(max(0, 1 - (x_right - x_max_wrong)))    при x_right - x_max_wrong >= -1
```

где:

- `x_right` - ответ сети, относящийся к классу, которому принадлежит объект;
- `x_max_wrong` - наибольший из ответов сети, относящихся к остальным классам.

## Настройки

### Вес слоя

```c++
void SetLossWeight( float lossWeight );
```

Установка коэффициента, на который будут домножаться градиенты этой функции потерь при обучении. По умолчанию `1`. Полезен при использовании нескольких функций потерь в одной сети.

### Урезание градиентов

```c++
void SetMaxGradientValue( float maxValue );
```

Установка максимального значения элемента в градиенте. Все значения градиента, по модулю превосходящие `GetMaxGradientValue()`, будут приведены к значениям, равным по модулю `GetMaxGradientValue()`.

## Обучаемые параметры

Слой не имеет обучаемых параметров.

## Входы

Слой имеет 2 или 3 входа:

1. Ответы сети, на которых необходимо вычислить функцию потерь.
2. Метки классов в одном из двух видов:
    * либо в виде блоба с данными типа `float`, у которого `BatchLength`, `BatchWidth` и `ListSize` равны аналогичным у первого входа, а `GetObjectSize()` равен числу классов. Каждый объект в блобе содержит эталонное распределение вероятности принадлежности объекта классам.
    * либо в виде блоба с данными типа `int`, у которого `BatchLength`, `BatchWidth` и `ListSize` равны аналогичным у первого входа, а остальные размерности равны `1`. Каждый объект в блобе содержит номер класса, к которому принадлежит этот объект.
3. *[Опционально]* Веса объектов. Блоб этого входа должен иметь те же `BatchLength`, `BatchWidth` и `ListSize`, что и у первых входов. Остальные размеры блоба должны быть равны `1`.

## Выходы

Слой не имеет выходов.

### Значение функции потерь

```c++
float GetLastLoss() const;
```

Получение значения функции потерь на последнем запуске сети.