# Генераторы

Обычные функции возвращают только одно-единственное значение (или ничего).

Генераторы - это специальный тип функции, который работает как фабрика итераторов.

Для объявления генератора используется новая синтаксическая конструкция: `function*`  (ключевое слово `function` со звёздочкой).

```js
function* gen() {
    yield 1;
    yield 2;
    yield 3;
}

const iterator = gen()
```

Вызов генератора создает объект класса `Generator`, который является итерируемым (iterable), т. е. его можно перебирать в цикле `for..of` или просто вызывая метод `next()`, при этом никакой код внутри генератора пока не выполняется.

Каждый вызов метода `next()` у созданного итератора запустить выполнение кода до первого `yield`, после чего будет возвращено значение, указанное в нем.

```js
// 1
console.log(iterator.next())
// [2, 3]
console.log(...iterator)
```

## Передача параметров в метод next

В метод `next` можно передать значение, которое будет возвращено `yield`:

```js
function* gen() {
    let i = 0

    while (true) {
        const param = yield i        

        console.log(param)

        if (param) {
            break
        }

        i++
    }
}

const iterator = gen()

iterator.next()     // { value: 0, done: false }
iterator.next(true) // { value: 1, done: false }
iterator.next()     // { value: undefined, done: true }
```

## Метод throw

У генераторов есть метод `throw`, который позволяет бросить исключение внутри генератора.

```js
function* gen() {
    try {
        // в этой строке возникнет ошибка
        let result = yield "Сколько будет 2 + 2?"; // (**)

        alert("выше будет исключение ^^^");
    } catch(e) {
        alert(e); // выведет ошибку
    }
}

let generator = gen();

let question = generator.next().value;

generator.throw(new Error("ответ не найден в моей базе данных")); // (*)
```

«Вброшенная» в строке `(*)` ошибка возникает в строке с `yield` `(**)`. Далее она обрабатывается как обычно. В примере выше она перехватывается `try..catch` и выводится.

Подробнее:

* [https://learn.javascript.ru/generator#generator-throw](https://learn.javascript.ru/generator#generator-throw)

## Метод return

У генераторов есть метод `return(value)`, которое останавливает работу генератора.

```js
function* gen() {
    yield 1;
    yield 2;
    yield 3;
}

var g = gen();

g.next();        // { value: 1, done: false }
g.return('foo'); // { value: "foo", done: true }
g.next();        // { value: undefined, done: true }
```

Если `return(value)` вызывает генератор, который находится в уже "завершённом" состоянии, генератор останется в "завершённом" состоянии. Если аргумент не был передан, свойство `value` вернёт тот же объект, что и `.next()`. Если аргумент был передан, он будет установлен как значение свойства `value` возвращаемого объекта.

Подробнее: [MDN](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global\_Objects/Generator/return).

## Композиция генераторов

Для генераторов есть особый синтаксис `yield*`, который позволяет «вкладывать» генераторы один в другой (осуществлять их композицию). Пример:

```js
function* generateSequence(start, end) {
    for (let i = start; i <= end; i++) yield i
}

function* generatePasswordCodes() {
    // 0..9
    yield* generateSequence(48, 57)
    // A..Z
    yield* generateSequence(65, 90)
    // a..z
    yield* generateSequence(97, 122)
}

let str = ''

for(let code of generatePasswordCodes()) {
  str += String.fromCharCode(code)
}

alert(str)
```

## Примечания

* Нельзя определить стрелочную функцию генератор, стандартом определен только синтаксис `function*`
