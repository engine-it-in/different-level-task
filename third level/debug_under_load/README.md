[Вернуться к оглавлению](https://github.com/engine-it-in/different-level-task/blob/main/README.md)
***
  * [Оптимизация](#оптимизация)
    * [Оптимизация алгоритмов](#оптимизация-алгоритмов)
    * [Уменьшение сложности запросов к базе данных](#уменьшение-сложности-запросов-к-базе-данных)
    * [Кэширование](#кэширование)
    * [Асинхронное программирование](#асинхронное-программирование)
    * [Параллелизм](#параллелизм)
    * [Предварительная выборка данных (Prefetching)](#предварительная-выборка-данных--prefetching-)
    * [Удаление неиспользуемого кода](#удаление-неиспользуемого-кода)
    * [Уменьшение количества операций внутри циклов](#уменьшение-количества-операций-внутри-циклов)
    * [Рефакторинг и чистый код](#рефакторинг-и-чистый-код)
***

## Оптимизация

### Оптимизация алгоритмов

Выбор и реализация более эффективных алгоритмов и структур данных может значительно улучшить производительность.
Пример:

```
Использование HashMap вместо List для реализации метода isExist(somthing, someWhere).
В случае обхода списка временная сложность возрастает в кол-во элементов в списке, тогда как получение значения из 
HashMap в данном случае не потребует обхода.
```

### Уменьшение сложности запросов к базе данных

Оптимизация запросов SQL, таких как выбор индексов, уменьшение объема возвращаемых данных и минимизация количества
запросов, может улучшить время отклика базы данных.
Пример:

```
Продумать индексы, чтобы выборка происходила быстрее
```

### Кэширование

Хранение часто используемых данных в памяти (кэше) для быстрого доступа, вместо того чтобы каждый раз извлекать их из
медленных источников, таких как базы данных или файловые системы.
Пример:

```
Закэшировать справочник, чтобы не тратить время на http-взаимодействие для получения данных
```

### Асинхронное программирование

Использование асинхронных операций для предотвращения блокировки основного потока выполнения
Пример:

```
Пока основной поток выполнения обрабатывает отдельный массив данных, программа может продолжить выполнение, после чего
обратиться к результатам обработки этого массива
```

### Параллелизм

Разделение задач на параллельно выполняемые части для лучшего использования многоядерных процессоров и уменьшения общего
времени выполнения.
Пример:

```
После того, как мы собрали все необходимые данные, мы можем распараллелить выполнение однотипных операций с каждым 
элементом для сокращения общего времени выполнения
```

### Предварительная выборка данных (Prefetching)

Загрузка данных заранее, основываясь на предположении о том, что они скоро понадобятся, что может уменьшить задержку при
последующих запросах.

```
При загрузке UI-приложения, выбираем диапазон данных, достаточный для того, чтобы начать работу. В дальнейшем эти данные
можно пополнить, но не выбирать все сразу. Например, показать данные за сегодня +-3 дня. А остальные дозагружать,
если это потребуется пользователю
```

### Удаление неиспользуемого кода

Избавление от мертвого кода и неиспользуемых библиотек может уменьшить размер приложения и сократить время его загрузки.

### Уменьшение количества операций внутри циклов

Перемещение инвариантов за пределы циклов и использование эффективных методов итерации.

Пример:

```
Если что-то может быть расчитано единожды и в дальнейшем не будет изменяться - ему не место в теле цикла.
```

### Рефакторинг и чистый код

Переписывание участков кода для улучшения читаемости и поддерживаемости, что может облегчить обнаружение и устранение
проблем с производительностью.
