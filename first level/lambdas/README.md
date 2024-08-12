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

### Consumer

Consumer<T> accept, где:

- \<T> – тип аргумента. Mетод accept - void:
- forEach();

### Supplier

Supplier<> ,где:

- \<> – Нет типа. Используется для получения возможного
  отдаленного результата;
- orElseThrow();

### Predicate

Predicate<T> test(T t), где:

- \<T> – тип аргумента. Результат - boolean:
- Работа над самим переданным аргументом;
- `.filter()`;
- Можно делать chain через .and(). Так 
получаем структурированный и понятный код;

### Function

Function<? super T, ? extends R>, где:
- \<T> – входной тип
- \<R> – результат
- Используется в `.map()`;

### Comparator

Comparator<T> int compare(T o1, T o2), где:

- o1 – первый элемент для сравнения,
- o2 – второй элемент для сравнения,
- Служит для реализации логики сравнения 
2х объектов и используется в stream api для 
`.sorted()` - операции;


