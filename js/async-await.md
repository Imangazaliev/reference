# async/await

`async/await` - это синтаксический сахар для промисов, позволяющий писать их в привычном синхронном стиле.

Ключевое слово `async` позволяет сделать из обычной функции асинхронную.

Такая функция делает две вещи:

* Оборачивает возвращаемое значение в Promise
* Позволяет использовать ключевое слово `await`

По сути она делает следующее:

```jsx
// обычная функция
function hello() {
    return 'Hello'
}

// асинхронная функция
async function hello() {
    return 'Hello'
}

// эквивалентно
function hello() {
    return new Promise((resolve, reject) => {
        try {
            // здесь может быть более сложный код, который может вернуть значение
            // или выбросить ошибку
            resolve('Hello')
        } catch (error) {
            reject(error)
        }
    })
}
```

Асинхронные функции становятся мощным инструментом при использовании ключевого слова `await`. `await` **работает только в асинхронных функциях.**

Мы можем использовать await перед promise-based функцией, чтобы остановить поток выполнения и дождаться результата её выполнения (результат Promise).

`await` можно использовать с любыми функциями, которые возвращают промис.

Пример:

```jsx
async function hello() {
  return greeting = await Promise.resolve('Hello')
}
```

Для обработки ошибок в `async/await` используется обычный `try/catch`.

## Подводные камни

Если написать несколько `async/await` подряд в случак, когда они независимы, то один промис будет дожидаться выполнения другого. Пример:

```jsx
await queryClient.prefetchQuery(QUERY_KEYS.BANNERS(restaurant?.id), () => getBanners(restaurant?.id))
await queryClient.prefetchQuery(QUERY_KEYS.CATEGORIES(restaurant?.id), () => getCategories(restaurant?.id))
await queryClient.prefetchQuery(QUERY_KEYS.POPULAR_PRODUCTS(restaurant?.id), () => getPopularProducts(restaurant?.id))
await queryClient.prefetchQuery(QUERY_KEYS.PRODUCTS(restaurant?.id), () => getProducts(restaurant?.id))
```

Можно вынести их в переменные или использовать `Promise.all`:

```jsx
const banners = queryClient.prefetchQuery(QUERY_KEYS.BANNERS(restaurant?.id), () => getBanners(restaurant?.id))
const categories queryClient.prefetchQuery(QUERY_KEYS.CATEGORIES(restaurant?.id), () => getCategories(restaurant?.id))
const popularProducts = await queryClient.prefetchQuery(QUERY_KEYS.POPULAR_PRODUCTS(restaurant?.id), () => getPopularProducts(restaurant?.id))
const products = await queryClient.prefetchQuery(QUERY_KEYS.PRODUCTS(restaurant?.id), () => getProducts(restaurant?.id))

await banners
await categories
await popularProducts
await products

// можно и так:

Promise.all([banners, categories, popularProducts, products])
```

Т. к. промисы в таком случае создаются одновременно, а значит и запускается асинхронный код, то один запрос не будет блокировать другой.

Подробнее: [Недостатки async/await (MDN)](https://developer.mozilla.org/ru/docs/Learn/JavaScript/Asynchronous/Promises#%D0%BD%D0%B5%D0%B4%D0%BE%D1%81%D1%82%D0%B0%D1%82%D0%BA%D0%B8\_asyncawait).
