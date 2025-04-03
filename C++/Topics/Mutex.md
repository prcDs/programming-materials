# Мьютексы в C++


## Что такое мьютекс?

**Мьютекс (`mutex`, "mutual exclusion")** — это механизм синхронизации, который предотвращает одновременный доступ нескольких потоков к одним и тем же данным.

Когда один поток захватывает мьютекс, другие потоки вынуждены **ждать** освобождения ресурса.
--- 

## Зачем нужен мьютекс?

Рассмотрим проблему без мьютекса:
```cpp
#include <iostream>
#include <thread>

using namespace std;

int counter = 0;

void increment() {
    for (int i = 0; i < 100000; i++) {
        counter++; // Несколько потоков изменяют переменную одновременно
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
### Ошибка: Значение `counter` будет разным при каждом запуске, так как оба потока одновременно изменяют переменную.

Используем мьютекс, чтобы исправить это:
```cpp
#include <iostream>
#include <thread>
#include <mutex>

using namespace std;

mutex mtx; // Глобальный мьютекс
int counter = 0;

void increment() {
    for (int i = 0; i < 100000; i++) {
        mtx.lock();   // Блокировка мьютекса
        counter++;    
        mtx.unlock(); // Разблокировка мьютекса
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
Теперь переменная counter изменяется без ошибок, потому что только один поток может выполнять counter++ одновременно.

## Использование std::mutex
Базовые операции с мьютексами:

- `lock()` – захватывает мьютекс (блокирует другие потоки).

- `unlock()` – освобождает мьютекс.

- `try_lock()` – пытается захватить мьютекс (не блокируя поток, если он занят).

### Пример try_lock():
```cpp
#include <iostream>
#include <thread>
#include <mutex>

using namespace std;

mutex mtx;

void task(int id) {
    if (mtx.try_lock()) { // Пытаемся заблокировать мьютекс
        cout << "Поток " << id << " выполняется\n";
        this_thread::sleep_for(chrono::seconds(1));
        mtx.unlock();
    } else {
        cout << "Поток " << id << " не смог получить мьютекс\n";
    }
}

int main() {
    thread t1(task, 1);
    thread t2(task, 2);

    t1.join();
    t2.join();

    return 0;
}
```
`try_lock()` не блокирует поток, если мьютекс занят, что помогает избежать дедлоков.
---
## Автоматическое управление мьютексами (lock_guard)
`std::lock_guard` автоматически захватывает мьютекс в момент создания объекта и освобождает его при выходе из области видимости.
```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <chrono>

using namespace std;

mutex mtx;

void print_numbers(int start) {
    lock_guard<mutex> guard(mtx);  // Захват мьютекса
    for (int i = 0; i < 3; i++) {
        cout << start + i << " ";
        this_thread::sleep_for(chrono::seconds(1));
    }
    cout << endl;
}

int main() {
    thread th1(print_numbers, 1);
    thread th2(print_numbers, 5);
    
    th1.join();
    th2.join();
    
    return 0;
}
```
Здесь не нужно вызывать lock() и unlock() вручную – lock_guard делает это автоматически.

## Альтернативный способ блокировки (unique_lock)
`std::unique_lock` более гибкий, чем `lock_guard`, так как позволяет отложенную блокировку и разблокировку вручную.

### Пример:
```cpp

#include <iostream>
#include <thread>
#include <mutex>

using namespace std;

mutex mtx;

void task() {
    unique_lock<mutex> lock(mtx, defer_lock); // Мьютекс пока не заблокирован
    cout << "Подготовка...\n";
    
    lock.lock(); // Блокируем мьютекс вручную
    cout << "Поток выполняется\n";
    lock.unlock(); // Разблокируем вручную

    cout << "Поток завершён\n";
}

int main() {
    thread t1(task);
    thread t2(task);

    t1.join();
    t2.join();
    
    return 0;
}
```
Здесь defer_lock позволяет отложить блокировку мьютекса до нужного момента.

#### Проблемы мьютексов
Хотя мьютексы решают проблему гонок данных, они могут приводить к:

- Дедлокам – два потока ждут друг друга, блокируя работу программы.

- Голоданию – один поток постоянно захватывает ресурс, не давая другим потокам выполнить задачу.

- Снижению производительности – если потоки часто блокируются, многопоточность теряет смысл.

## Дедлоки и как их избежать
Пример дедлока:
```cpp


#include <iostream>
#include <thread>
#include <mutex>

using namespace std;

mutex mtx1, mtx2;

void task1() {
    lock_guard<mutex> lock1(mtx1);
    this_thread::sleep_for(chrono::milliseconds(100));
    lock_guard<mutex> lock2(mtx2); // Ожидание mtx2
    cout << "Task 1 завершён\n";
}

void task2() {
    lock_guard<mutex> lock2(mtx2);
    this_thread::sleep_for(chrono::milliseconds(100));
    lock_guard<mutex> lock1(mtx1); // Ожидание mtx1
    cout << "Task 2 завершён\n";
}

int main() {
    thread t1(task1);
    thread t2(task2);

    t1.join();
    t2.join();

    return 0;
}
```
Ошибка: Поток t1 держит mtx1 и ждёт mtx2, а t2 держит mtx2 и ждёт mtx1 – программа зависает.
Решение: `std::lock()`
```cpp


#include <iostream>
#include <thread>
#include <mutex>

using namespace std;

mutex mtx1, mtx2;

void task() {
    lock(mtx1, mtx2); // Блокируем оба мьютекса одновременно
    lock_guard<mutex> lock1(mtx1, adopt_lock);
    lock_guard<mutex> lock2(mtx2, adopt_lock);
    cout << "Оба мьютекса захвачены безопасно\n";
}

int main() {
    thread t1(task);
    thread t2(task);

    t1.join();
    t2.join();

    return 0;
}
```
`std::lock(mtx1, mtx2)` блокирует оба мьютекса одновременно, предотвращая дедлок.