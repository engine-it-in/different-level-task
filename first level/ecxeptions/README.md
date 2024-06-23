[Вернуться к оглавлению](https://github.com/engine-it-in/different-level-task/blob/main/README.md)
***
# Java Exceptions
- [Java Exceptions](#java-exceptions)
    * [Classification](#classification)
    * [Hierarchy](#hierarchy)
***

Исключения - это классы, необходимость которых заключается в том, чтобы сообщить об исключительной ситуации (нередко
ошибке) в процессе исполнения программы.

## Classification

Подразделяются на `checked` и `unchecked`.

**Checked** обязательны для декларирования в сигнатуре метода. Код, вызывающий метод, в сигнатуре которого
есть `checked`
исключение, обязан:

- либо прокинуть его выше,
- либо обработать.

**Unchecked** не декларируются в сигнатуре, но также может быть обработан.

## Hierarchy

![Изображение с иерархией](https://avatars.dzeninfra.ru/get-zen_doc/271828/pub_656ee47a6e45c80d07cc418d_656ee73a649b955cd301713c/scale_1200)
Корневое исключение - `Throwable` (является checked-исключением).
От него наследуется `Error` (unchecked) и `Exception` (checked).

**Error** - ошибки возникающие при работе java (исключения, связанные с памятью, JVM и т.п.).
**Exception** - исключения в их широком понимании. Подразделяются на RuntimeException (unchecked) и другие (checked).