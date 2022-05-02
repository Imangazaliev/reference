# Итераторы и перебираемые объекты

_Перебираемые_ (или _итерируемые_) объекты – это концепция, которая позволяет использовать любой объект в цикле `for..of`.

Объект является итератором, если он умеет обращаться к элементам коллекции по одному за раз, при этом отслеживая своё текущее положение внутри этой последовательности.

В JavaScript итератор - это объект, который предоставляет метод next(), возвращающий следующий элемент последовательности. Этот метод возвращает объект с двумя свойствами: `done` и `value`.

* `done` - флаг, указывающий, что текущий элемент является последним
* `value` - значение текущей итерации. Значение последнего э

```js
function makeIterator(array){
    var nextIndex = 0;

    return {
       next: function(){
           return nextIndex < array.length ?
               {value: array[nextIndex++], done: false} :
               {done: true};
       }
    }
}
```

Можно сделать объект итерируемым, присвоив `Symbol.iterator` функцию, которая возвращает итератор:

```js
let range = {
  from: 1,
  to: 5
}

// 1. вызов for..of сначала вызывает эту функцию
range[Symbol.iterator] = () => {
    // ...она возвращает объект итератора:
    // 2. Далее, for..of работает только с этим итератором, запрашивая у него новые значения
    return {
        current: this.from,
		    last: this.to,
		
        // 3. next() вызывается на каждой итерации цикла for..of
        next() {
            // 4. он должен вернуть значение в виде объекта {done:.., value :...}
            if (this.current <= this.last) {
                return { done: false, value: this.current++ };
            } else {
                return { done: true };
            }
        }
    }
}
```

## Итерируемые объекты и псевдомассивы

Есть два официальных термина, которые очень похожи, но в то же время сильно различаются. Поэтому убедитесь, что вы как следует поняли их, чтобы избежать путаницы.

* _Итерируемые объекты_ – это объекты, которые реализуют метод `Symbol.iterator`, как было описано выше.
* _Псевдомассивы_ – это объекты, у которых есть индексы и свойство `length`, то есть, они выглядят как массивы.

## Итого

* Технически итерируемые объекты должны иметь метод `Symbol.iterator`.
  * Результат вызова `obj[Symbol.iterator]` называется _итератором_. Он управляет процессом итерации.
  * Итератор должен иметь метод `next()`, который возвращает объект `{done: Boolean, value: any}`, где `done:true` сигнализирует об окончании процесса итерации, в противном случае `value` – следующее значение.
* Метод `Symbol.iterator` автоматически вызывается циклом `for..of`, но можно вызвать его и напрямую.
* Встроенные итерируемые объекты, такие как строки или массивы, также реализуют метод `Symbol.iterator`.
* Строковый итератор знает про суррогатные пары.