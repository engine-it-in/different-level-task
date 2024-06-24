[Вернуться к оглавлению](https://github.com/engine-it-in/different-level-task/blob/main/README.md)
***

* [Lambdas](#lambdas)
    * [Основные](#основные)
        * [Predicate](#predicate)
        * [Collector](#collector)
        * [Function](#function)
        * [Comparator](#comparator)

***

# Lambdas

* Простые функциональные интерфейсы, которые используются самостоятельно, в [Stream](../streams/README.md);
* Помогают лаконично обрабатывать данные по заданным критериям;

## Основные

### Predicate

Predicate<T> test(T t), где:

- \<T> – тип аргумента. Результат - boolean:
    - Это позволяет использовать его в методе `.filter()` stream api;
    - Можно делать chain через .and() и иметь структурированный и понятный код;

### Collector

Collector<? super T, A, R> , где:

- \<T> – the type of input elements to the reduction operation;
- \<A> – the mutable accumulation type of the reduction operation (often hidden as an implementation detail);
- \<R> – the result type of the reduction operation; 
- Используется в `.collect()` - операции. Помогает собирать stream в Collection в Stream API.

### Function

Function<? super T, ? extends R>, где:
- \<T> – the type of the input to the function
- \<R> – the type of the result of the function
- Простая функция. Используется в `.map()` - операции.

### Comparator

Comparator<T> int compare(T o1, T o2), где:

- o1 – the first object to be compared,
- o2 – the second object to be compared
- Служит для реализации логики сравнения 2х объектов и используется в stream api для `.sorted()` - операции.


