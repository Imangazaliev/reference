# Function Declaration и Function Expression

* _Function Declaration -_ функция, объявленная в основном потоке кода.
* _Function Expression -_ объявление функции в контексте какого-либо выражения, например присваивания.

Несмотря на немного разный вид, по сути две эти записи делают одно и то же:

```jsx
// Function Declaration
function sum(a, b) {
  return a + b;
}

// Function Expression
var sum = function(a, b) {
  return a + b;
}
```

Оба этих объявления говорят интерпретатору: "объяви переменную`sum`, создай функцию с указанными параметрами и кодом и сохрани её в`sum`".

**Основное отличие между ними: функции, объявленные как Function Declaration, создаются интерпретатором до выполнения кода.**

Поэтому их можно вызвать **до** объявления, например:

```js
sayHi('Вася'); // Привет, Вася

function sayHi(name) {
  console.log('Привет, ' + name );
}
```

А если бы это было объявление **Function Expression, то такой вызов бы не сработал**:

```js
sayHi('Вася'); // ошибка!

var sayHi = function(name) {
  console.log('Привет, ' + name);
}
```

Это из-за того, что JavaScript перед запуском кода ищет в нём **Function Declaration** (их легко найти: они не являются частью выражений и начинаются со слова`function`) и обрабатывает их. А **Function Expression** создаются в процессе выполнения выражения, в котором созданы, в данном случае – функция будет создана при операции присваивания `sayHi = function...`.

Как правило, возможность Function Declaration вызвать функцию до объявления – это удобно, так как даёт больше свободы в том, как организовать свой код. Можно расположить функции внизу, а их вызов – сверху или наоборот.

В отличие от объявлений **Function Declaration**, которые создаются заранее, до выполнения кода, объявления **Function Expression** создают функцию, когда до них доходит выполнение. **Благодаря этому свойству Function Expression можно (и даже нужно) использовать для условного объявления функции.**

## Источники

* [Сравнение Function Decaration и Function Expression](https://behemothoz.gitbooks.io/js-learn/content/chapter1/raznitsa-mezhdu-function-expression-i-function-declaration.html)
