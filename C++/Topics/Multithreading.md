# Многопоточность в C++

## Что такое многопоточность?

Многопоточность позволяет выполнять несколько задач параллельно, используя потоки (**threads**). Это даёт следующие преимущества:
- **Ускорение выполнения** – возможность загружать несколько ядер процессора.
- **Реактивность программы** – пользовательские интерфейсы остаются отзывчивыми во время тяжёлых вычислений.
- **Эффективность** – разделение задач между потоками позволяет лучше использовать ресурсы.

В C++ многопоточные программы реализуются с помощью библиотеки `<thread>`.
---

## Создание потоков

Запустить поток можно, передав в него функцию:
```cpp
#include <iostream>
#include <thread>

using namespace std;

void task() {
    cout << "Выполняется поток\n";
}

int main() {
    thread t(task); // Запускаем поток
    t.join(); // Ждём завершения потока
    cout << "Основной поток завершён\n";
    return 0;
}
```
Разбор кода:
- `thread t(task);` – создаём поток, который выполняет task().

- `t.join();` – основной поток ждёт завершения t, иначе программа может завершиться раньше.

## Ожидание завершения потоков
Есть два способа управлять потоками:

1. `join()` – основной поток ждёт завершения потока.

2. `detach()` – поток выполняется самостоятельно, без ожидания.

### Пример `join()`:

```cpp


#include <iostream>
#include <thread>

using namespace std;

void task() {
    cout << "Запущен поток\n";
}

int main() {
    thread t(task);
    t.join(); // Ждём завершения потока
    cout << "Основной поток завершён\n";
    return 0;
}
```
### Пример `detach()`:

```cpp


#include <iostream>
#include <thread>

using namespace std;

void task() {
    cout << "Отделённый поток выполняется\n";
}

int main() {
    thread t(task);
    t.detach(); // Поток продолжает выполнение независимо
    cout << "Основной поток завершён\n";
    return 0;
}
```
Опасность `detach()`: если `main()` завершится раньше, отделённый поток может работать с несуществующими объектами, что приведёт к ошибкам.

## Передача аргументов в потоки
Можно передавать аргументы в функцию потока:

```cpp


#include <iostream>
#include <thread>

using namespace std;

void task(int id) {
    cout << "Поток " << id << " выполняется\n";
}

int main() {
    thread t1(task, 1);
    thread t2(task, 2);

    t1.join();
    t2.join();

    return 0;
}
```
Аргументы передаются в `thread`, после имени функции.

### Работа с лямбда-функциями в потоках
Вместо обычной функции можно использовать лямбда-выражение:

```cpp


#include <iostream>
#include <thread>

using namespace std;

int main() {
    thread t([]() {
        cout << "Лямбда-функция в потоке\n";
    });

    t.join();
    return 0;
}
```
Лямбды удобны для небольших функций, которые не требуют отдельного объявления.

|Различие между join и detach                       |
|---------------------------------------------------|
|Метод   |	Описание                                |
|join()  |	Основной поток ждёт завершения потока.  |
|detach()|	Поток выполняется независимо.           |

### Пример, показывающий разницу:

```cpp


#include <iostream>
#include <thread>

using namespace std;

void task() {
    cout << "Поток выполняется\n";
}

int main() {
    thread t1(task);
    thread t2(task);

    t1.join();  // Ждём завершения первого потока
    t2.detach(); // Отделяем второй поток

    cout << "Основной поток завершён\n";
    return 0;
}
```
## Проблемы многопоточности
При использовании потоков могут возникать ошибки:
- Гонки данных (Race Condition) – два потока обращаются к одной переменной одновременно.
- Дедлоки (Deadlocks) – поток блокируется, ожидая ресурс, который уже занят другим потоком.
- Голодание (Starvation) – один поток получает ресурсы, а другой — нет.

### Рассмотрим проблему гонки данных:

```cpp


#include <iostream>
#include <thread>

using namespace std;

int counter = 0;

void increment() {
    for (int i = 0; i < 100000; i++) {
        counter++; // Несколько потоков изменяют одну переменную
    }
}

int main() {
    thread t1(increment);
    thread t2(increment);

    t1.join();
    t2.join();

    cout << "Counter = " << counter << endl;
    return 0;
}
```
Возможная ошибка: значение counter будет разным при каждом запуске из-за одновременного доступа к переменной из нескольких потоков.

## Синхронизация потоков (мьютексы)
Чтобы избежать ошибок, используются мьютексы `(std::mutex)`.

### Пример с мьютексом:

```cpp

#include <iostream>
#include <thread>
#include <mutex>

using namespace std;

mutex mtx;
int counter = 0;

void increment() {
    for (int i = 0; i < 100000; i++) {
        lock_guard<mutex> lock(mtx);
        counter++;
    }
}

int main() {
    thread t1(increment);
    thread t2(increment);

    t1.join();
    t2.join();

    cout << "Counter = " << counter << endl;
    return 0;
}
```
`lock_guard<mutex>` автоматически блокирует мьютекс, защищая переменную counter.

### Пример кода без мьютекса и с 
```cpp


#include <iostream>
#include <thread>
#include <chrono>

using namespace std;

/**
 * Функция, имитирующая задержку (сон потока)
 * @param duration Время задержки в секундах
 */
void sleep(int duration) {
    this_thread::sleep_for(chrono::seconds(duration));
}

int main() {
    cout << "First thread started\n";
    thread th1(sleep, 10); // Первый поток засыпает на 10 секунд

    cout << "Second thread started\n";
    thread th2(sleep, 5);  // Второй поток засыпает на 5 секунд

    cout << "Waiting for threads to finish...\n";

    th1.join(); // Ожидаем завершения первого потока
    cout << "First thread finished\n";

    th2.join(); // Ожидаем завершения второго потока
    cout << "Second thread finished\n";

    return 0;
}
```
#### Разбор кода
- `std::thread th1(sleep, 10);` – создаём первый поток, который "засыпает" на 10 секунд.

- `std::thread th2(sleep, 5);` – создаём второй поток, который "засыпает" на 5 секунд.

- `th1.join();` – основной поток ожидает завершения первого потока.

- `th2.join();` – основной поток ожидает завершения второго потока.

Вывод программы:
```
First thread started
Second thread started
Waiting for threads to finish...
Second thread finished
First thread finished
(Второй поток завершается раньше, потому что его задержка короче.)
```

### С использованием мьютекса (`std::mutex`) для синхронизации
```cpp


#include <iostream>
#include <thread>
#include <chrono>
#include <mutex>

using namespace std;

mutex mtx; // Глобальный мьютекс для защиты консоли

/**
 * Функция, выводящая сообщение в потокобезопасном режиме
 * @param msg Текст сообщения
 * @param count Количество повторений
 */
void print_msg(const string& msg, int count) {
    for (int i = 0; i < count; i++) {
        {
            lock_guard<mutex> lock(mtx); // Блокировка мьютекса в этом блоке
            cout << msg << " (" << i + 1 << ")" << endl;
        }
        this_thread::sleep_for(chrono::seconds(1)); // Задержка в 1 секунду
    }
}

int main() {
    thread t1(print_msg, "Thread 1", 5);
    thread t2(print_msg, "Thread 2", 5);

    t1.join();
    t2.join();

    cout << "--- Done ---" << endl;
    return 0;
}
```
#### Разбор кода
- Мьютекс `std::mutex mtx;` – создаётся глобальный объект для защиты разделяемых ресурсов.

- `lock_guard<mutex> lock(mtx);` – автоматически блокирует и разблокирует мьютекс.

- Вывод сообщений синхронизирован, так как только один поток может писать в cout в момент времени.

- Потоки работают параллельно, но не мешают друг другу при выводе в консоль.

Вывод программы (пример):

```
Thread 1 (1)
Thread 2 (1)
Thread 1 (2)
Thread 2 (2)
Thread 1 (3)
Thread 2 (3)
Thread 1 (4)
Thread 2 (4)
Thread 1 (5)
Thread 2 (5)
--- Done ---
(Вывод может чередоваться, но не будет накладываться друг на друга.)
```

### **Итог**
- `std::thread` позволяет работать с потоками.

- `join()` обязателен для ожидания завершения потока.

- `std::mutex` предотвращает гонки данных, когда потоки работают с общими ресурсами.

- `lock_guard<mutex>` автоматически блокирует и разблокирует мьютекс, защищая код от deadlock'ов.

#### Читать также
- [Мьютексы](./Mutex.md)