---
layout: default
title: xsl:decimal-format
nav_order: 10
parent: XSLT
---

<!-- prettier-ignore-start -->
# xsl:decimal-format
{: .no_toc }
<!-- prettier-ignore-end -->

XSLT позволяет изменять специальные символы, влияющие на форматирование строки. Именованный набор таких символов и некоторых других указаний называется десятичным форматом и определяется элементом **`xsl:decimal-format`**.

От атрибутов этого элемента зависит, как будут обрабатываться символы образца форматирования и как число будет отображаться на выходе.

<!-- prettier-ignore -->
1. TOC
{:toc}

## Синтаксис

### XSLT 1.0 и 2.0

```xml
<xsl:decimal-format
    name = "строка"
    decimal-separator = "символ"
    grouping-separator = "символ"
    infinity = "строка"
    minus-sign = "символ"
    NaN = "строка"
    percent = "символ"
    per-mille = "символ"
    zero-digit = "символ"
    digit = "символ"
    pattern-separator = "символ" />
```

Атрибуты:

- `name` — _необязательный_ атрибут, задает расширенное имя десятичного формата. Если имя не указано, это означает, что элемент `xsl:decimal-format` определяет десятичный формат по умолчанию.
- `decimal-separator` — _необязательный_ атрибут, задает символ, разделяющий целую и дробную части числа. Значением этого атрибута по умолчанию является символ ".", с Unicode-кодом `#x2e`. Атрибут `decimal-separator` рассматривается как специальный символ образца форматирования. Кроме того, он будет использован как разделяющий символ при выводе;
- `grouping-separator` — _необязательный_ атрибут, задает символ, группирующий цифры в целой части записи числа. Такие символы используются, например, для группировки тысяч ("1,234,567.89"). Значением по умолчанию является символ ",", код `#x2c`. `grouping-separator` рассматривается как специальный символ образца форматирования. Помимо этого, он будет использован как разделяющий символ групп цифр при выводе числа;
- `percent` — _необязательный_ атрибут, задает символ процента. Значением по умолчанию является символ "%", код `#x25`. Этот символ будет распознаваться в образце форматирования и использоваться при выводе;
- `per-mille` — _необязательный_ атрибут, задает символ промилле. Значением по умолчанию является символ "‰", код `#x2030`. Символ промилле распознается в образце форматирования и используется в строковом представлении числа;
- `zero-digit` — _необязательный_ атрибут, задает символ нуля. Значением по умолчанию является символ "0", код `#x30`. В качестве цифр при отображении числа будут использоваться символ нуля и 9 символов, следующих за ним. Символ нуля распознается в образце форматирования и используется при выводе строкового представления числа;
- `digit` — _необязательный_ атрибут, определяет символ, который используется в образце форматирования для определения позиции необязательного символа. Значением по умолчанию является символ "#". Этот символ распознается как форматирующий символ необязательной цифры. Он не включается в строковое представление числа;
- `pattern-separator` — _необязательный_ атрибут, определяет символ, который используется в образце форматирования для разделения положительного и отрицательного форматов числа. Он не включается в строковое представление числа. Значением этого атрибута по умолчанию является символ ";";
- `infinity` — _необязательный_ атрибут, задает строку, которая будет представлять бесконечность. Значением по умолчанию является строка "Infinity";
- `NaN` — _необязательный_ атрибут, задает строку, которая будет представлять не-числа. Значением по умолчанию является строка "NaN";
- `minus-sign` — _необязательный_ атрибут, задает символ, который будет использоваться для обозначения отрицательных чисел. Значением по умолчанию является символ "-", код `#x2D`.

### XSLT 3.0

```xml
<xsl:decimal-format
    name? = eqname
    decimal-separator? = char
    grouping-separator? = char
    infinity? = string
    minus-sign? = char
    exponent-separator? = char
    NaN? = string
    percent? = char
    per-mille? = char
    zero-digit? = char
    digit? = char
    pattern-separator? = char />
```

## Описание и примеры

Элемент `xsl:decimal-format` не имеет смысла без функции [`format-number`](/xpath/format-number/). Все, на что влияют его атрибуты — это формат, который будет использоваться при преобразовании чисел в строку функцией `format-number`.

### Примеры

Определение десятичного формата:

```xml
<xsl:decimal-format
    name="format1"
    decimal-separator=","
    minus-sign="N"
    grouping-separator=":"
    infinity="?"
    NaN="not-a-number"
    percent="%"
    digit="$"
    pattern-separator="|"/>
```

Примеры функций format-number:

```
format-number(123456.78, '$,0000', 'format1) ? '123456,7800'
format-number(-123456.78, '$,00$$', 'format1') ? 'N123456,78'
format-number(123456.78, '$,0000|$,0000-', 'format1') ? '123456,7800'
format-number(-123456.78, '$,00001$,0000-', 'format1') ? '123456,7800-'
format-number(-123456.78, '000:000:000,00$$', 'format1') ? 'N000:123:456,78'
format-number('zero', '000:000:000,00$$', 'format1') -> 'not-a-number'
format-number(1 div 0, '$,$', 'format1') ? '?'
format-number(-1 div 0, '$,$', 'format1') ? 'N?'
```

Определение десятичного формата:

```xml
<xsl:decimal-format name="format2" zero-digit="/"/>
```

Примеры функций format-number:

```
format-number(123456789, '#', 'format2') ? '012345678'
format-number(123456789, '#') ? '123456780'
```

Определение десятичного формата:

```xml
<xsl:decimal-format name="format3" zero-digit="1"/>
```

Примеры функций format-number:

```xml
format-number(123456789, '#', 'format3') ? '23456789:'
format-number(12345.06789, '#.#####', 'format3') ? '23456.1789:'
```

Десятичный формат, определяемый элементом `xsl:decimal-format`, в отличие от многих других элементов не может переопределяться в преобразованиях со старшим порядком импорта. Элементы `xsl:decimal-format` должны определять десятичные форматы с различными именами (за исключением тех случаев, когда значения их атрибутов полностью совпадают).

В XSLT 2.0 устанавливаются более жесткие ограничения синтаксиса таблиц стилей. Например, таблица стилей с командой `1 div 0` работать не будет. Помните, что в любой элемент XSLT 2.0 можно включить атрибут `version`, если требуется использовать поведение XSLT 1.0 в таблице стилей XSLT 2.0:

```xml
<?xml version="1.0" encoding="ISO-8859-1" ?>
<!-- decimal-format2.xsl -->
<xsl:stylesheet version="2.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    ...
    <xsl:text>
    format-number(1 div 0, '###,###.00', 'f2')=</xsl:text>
    <xsl:value-of version="1.0" select="format-number(1 div 0, '###,###.00', 'f2')"/>
    ...
</xsl:stylesheet>
```

Элементу [`<xsl:stylesheet>`](/xslt/xsl-stylesheet/) задается атрибут `version` со значением `2.0`, однако позднее в элемент [`<xsl:value-of>`](/xslt/xsl-value-of/) включается атрибут `version="1.0"`. В результате поведение XSLT 1.0 активизируется только для одного элемента, а это означает, что при делении на нуль будет получено значение `Really, really big`. При использовании стандартного поведения XSLT 2.0 таблица стилей выдает исключение и осмысленный вывод не генерируется.

В XSLT 2.0 деление `xs:double` на нуль возвращает строку `Infinity`, а деление `xs:integer` или `xs:decimal` на нуль приводит к ошибке времени выполнения. Если привести выражение в таблице стилей к виду `xs:double(1) div xs:double(0)`, таблица стилей отработает без ошибок и выдаст тот же результат (`Infinity`), что и таблица стилей XSLT 1.0 с выражением `1 div 0`.

## См. также

- [`format-number`](/xpath/format-number/) -- функция для форматирования чисел.

## Ссылки

- [`xsl:decimal-format`](https://developer.mozilla.org/en/XSLT/decimal-format) на MDN
- [`xsl:decimal-format`](https://msdn.microsoft.com/en-us/library/ms256092.aspx) на MSDN