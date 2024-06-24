[Вернуться к оглавлению](https://github.com/engine-it-in/different-level-task/blob/main/README.md)
***
* [Multithreading](#multithreading)
  * [Thread](#thread)
  * [Runnable](#runnable)
  * [ExecutorService](#executorservice)
  * [Synchronized](#synchronized)
  * [Volatile](#volatile)
***

# Multithreading

В Java существует несколько подходов к работе с многопоточностью.

## Thread

Создать новый экземпляр класса Thread и переопределить метод run() для определения задачи, которую поток будет
выполнять. Затем запустить поток с помощью метода start().

```java
class MyThread extends Thread {
    @Override
    public void run() {
        // Ваш код для выполнения задачи
    }
}

    // Создание и запуск потока
    MyThread thread = new MyThread();
thread.start();

```

## Runnable

Реализовать интерфейс Runnable и реализовать метод run() для определения задачи потока. Затем создать экземпляр класса
Thread, передавая экземпляр Runnable в конструктор, и запустить поток с помощью метода start().

```java
class MyRunnable implements Runnable {
    @Override
    public void run() {
        // Ваш код для выполнения задачи
    }
}

    // Создание и запуск потока
    Thread thread = new Thread(new MyRunnable());
thread.start();
```

## ExecutorService

Создать пул потоков с помощью метода `Executors.newFixedThreadPool()`, получить экземпляр ExecutorService и отправить
задачу (`Runnable`) на выполнение с помощью метода execute().

```java
ExecutorService executorService=Executors.newFixedThreadPool(5);
        executorService.execute(new Runnable(){
@Override
public void run(){
        // Ваш код для выполнения задачи
        }
        });
// Завершение работы пула потоков после выполнения всех задач
        executorService.shutdown(); 
```

## Synchronized

Есть общий ресурс, доступ к которому могут получать несколько потоков одновременно. Следует синхронизировать доступ к
этому ресурсу с помощью ключевого слова `synchronized` или использовать классы с механизмами блокировки, такие
как `Lock` и `ReentrantLock`, чтобы избежать проблем с одновременным доступом к ресурсу.

Ключевое слово `synchronized` позволяет синхронизировать доступ к блоку кода или методу. Это гарантирует, что только
один поток может выполнять синхронизированный участок кода одновременно.

## Volatile

Ключевое слово volatile позволяет обеспечить видимость изменений переменных между потоками. Оно гарантирует, что
операции чтения и записи значения переменной будут происходить непосредственно в памяти, а не в локальном кэше потока.
