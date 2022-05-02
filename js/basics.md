# Основы

## Типы

8 типов:

- string
- number
- BigInt
- boolean
- Symbol
- object
- null
- undefined

[Типы данных JavaScript и структуры данных (MDN)](https://developer.mozilla.org/ru/docs/Web/JavaScript/Data_structures)

## var/const/let

Отличия:

- Область видимости:
    - `var` доступен во всем контексте. До объявление его значение равно либо `window`, либо `undefined` (в строгом режиме).
    - `let` и `const` доступны только в пределах блока: можно внутри условия, цикла или просто фигурный скобок определить переменную и она будет доступна только в ней. Обращение вне блока к переменным, объявленным с их помощью, приведет к ошибке.
- Повторное объявление переменных с существующим идентификатором:
    - `var` не выдаст ошибку, создав переменную с указанным значением (если она тоже была объявлена с помощью `var`, иначе выдаст ошибку)
    - `let` и `const` выдадут ошибку
    
    ```js
    var age = 1
    var age = 2
    // Uncaught SyntaxError: Identifier 'age' has already been declared
    const age = 3
    ```
    

## Сравнение

[Операторы сравнения (MDN)](https://developer.mozilla.org/ru/docs/Web/JavaScript/Equality_comparisons_and_sameness)

### Оператор `==`

- Перед сравнением оператор равенства приводит обе величины к общему типу.
- Приведение типов не происходит для `null` и `undefined`

### Оператор `===`

`TBD`

### shallow equality и deep equality

`TBD`

## NaN

**NaN** - специальное значение в JavaScript представляющее “не число” (”Not-a-Number”).

Тип `NaN`, как ни странно, `number`.

Проверять значение на равенство `NaN` нужно не с помощью операторов равенства, а с помощью специальной функции `isNaN` или метода `Number.isNaN`.
