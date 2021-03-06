# Функция hasOwnProperty

Если вам необходимо проверить, определено ли свойство у _самого объекта_, а **не** в его [цепочке прототипов](prototype.md), вы можете использовать метод `hasOwnProperty`, который все объекты наследуют от `Object.prototype`.

> **Примечание:** Для проверки наличия свойства **недостаточно** проверять, эквивалентно ли оно `undefined`. Свойство может вполне себе существовать, но при этом ему может быть присвоено значение `undefined`.

`hasOwnProperty` — единственная функция в JavaScript, которая позволяет получить свойства объекта **без обращения** к цепочке его прототипов.

```js
// испортим Object.prototype
Object.prototype.bar = 1
var foo = { goo: undefined }

foo.bar // 1
'bar' in foo // true

foo.hasOwnProperty('bar') // false
foo.hasOwnProperty('goo') // true
```

Только используя `hasOwnProperty` можно гарантировать правильный результат при переборе свойств объекта. И **нет** иного способа для определения свойств, которые определены в _самом_ объекте, а не где-то в цепочке его прототипов.

## hasOwnProperty как свойство

JavaScript **не** резервирует свойство с именем `hasOwnProperty`. Так что, если есть потенциальная возможность, что объект может содержать свойство с таким именем, требуется использовать _внешний_ вариант функции `hasOwnProperty` чтобы получить корректные результаты.

```js
var foo = {
  hasOwnProperty: function () {
    return false
  },
  bar: 'Да прилетят драконы',
}

foo.hasOwnProperty('bar') // всегда возвращает false

// Используем метод hasOwnProperty пустого объекта
// и передаём foo в качестве this
;({}.hasOwnProperty.call(foo, 'bar')) // true
```

## Заключение

**Единственным** способом проверить существование свойства у объекта является использование метода `hasOwnProperty`. При этом рекомендуется использовать этот метод в **каждом** [цикле `for in`](forinloop.md) вашего проекта, чтобы избежать возможных ошибок с ошибочным заимствованием свойств из [прототипов](prototype.md) родительских объектов. Также вы можете использовать конструкцию `{}.hasOwnProperty.call(...)` на случай, если кто-то вздумает расширить [прототипы](prototype.md) встроенных объектов.
