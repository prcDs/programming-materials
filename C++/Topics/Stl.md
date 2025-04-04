# Библиотека STL

## Основные компоненты
- **Контейнеры**
    STL предоставляет множество контейнеров для хранения данных. Они делятся на 3 категории:

    - **Последовательные (линейные)**
        Хранят элементы в линейном порядке.
    - **Ассоциативные (упорядоченные/неупорядоченные)**
        Хранят элементы в отсортированном/хешированном виде.
    - **Адаптеры (надстройки над другими контейнерами)**
        Надстройки над другими контейнерами.
- **Алгоритмы**
    STL предоставляет широкий набор методов для работы со своими контейнерами
- **Итераторы**
    Итераторы – это объекты, которые позволяют перебирать элементы контейнеров.

    Типы итераторов:
        Input / Output – только чтение/запись.

        Forward – однонаправленный (std::forward_list).

        Bidirectional – двунаправленный (std::list, std::set).

        Random Access – произвольный доступ (std::vector, std::deque).
    ```cpp
    std::vector<int> vec = {1, 2, 3};
    for (auto it = vec.begin(); it != vec.end(); ++it) {
    std::cout << *it << " "; // 1 2 3
    }

    #include <iostream>
    #include <set>

    int main() {
    std::set<int> mySet = {1, 2, 3, 4, 5};  // Упрощённая инициализация
    for (auto it = mySet.cbegin(); it != mySet.cend(); ++it) {
        std::cout << *it << std::endl;  // Исправлено: убрана лишняя кавычка
    }
    return 0;
    }

    #include <iostream>
    #include <map>
    #include <string>

    int main() {
    std::map<int, std::string> myMap = {
        {1, "one"}, {2, "two"}, {3, "three"}  // Исправлено: добавлены значения
    };
    for (auto it = myMap.cbegin(); it != myMap.cend(); ++it) {
        std::cout << it->first << ": " << it->second << std::endl;
    }
    return 0;
    }
    ```

## Пример
### std::vector – динамический массив
```cpp
#include <vector>
std::vector<int> vec = {1, 2, 3};
vec.push_back(4); // Добавление в конец
vec.pop_back();   // Удаление с конца
```
Плюсы:
    Быстрый доступ по индексу (O(1)).
    Динамически расширяется.
Минусы:
    Вставка/удаление в середину – O(n).

### std::list – двусвязный список
```cpp
#include <list>
std::list<int> lst = {1, 2, 3};
lst.push_front(0); // Вставка в начало
lst.pop_back();    // Удаление с конца
```
Плюсы:
    Быстрая вставка/удаление (O(1)).
Минусы:
    Нет доступа по индексу (O(n)).

### std::deque – двусторонняя очередь
```cpp
#include <deque>
std::deque<int> dq = {1, 2, 3};
dq.push_front(0); // В начало
dq.push_back(4);  // В конец
```
Плюсы:
    Быстрая вставка/удаление с обоих концов (O(1)).
Минусы:
    Доступ по индексу медленнее, чем у vector.
### std::forward_list — односвязный список
```cpp
#include <forward_list>
#include <iostream>

int main() {
    std::forward_list<int> flist = {1, 2, 3};

    flist.push_front(0);  // 0 -> 1 -> 2 -> 3
    flist.pop_front();    // 1 -> 2 -> 3
    flist.insert_after(flist.begin(), 99);  // 1 -> 99 -> 2 -> 3

    for (int x : flist) {
        std::cout << x << " ";  // Вывод: 1 99 2 3
    }
}
```
Когда использовать?
    Когда нужны частые вставки/удаления в середине.
    Если не требуется обратный проход или доступ по индексу.

### std::array — статический массив
Аналог обычного массива int[N], но с методами STL.
```cpp
#include <array>
#include <iostream>

int main() {
    std::array<int, 3> arr = {1, 2, 3};

    arr.fill(5);  // 5 5 5
    std::cout << arr.at(1);  // 5 (с проверкой границ)
    std::cout << arr.size();  // 3
}
```
Сравнение с обычным массивом:
```cpp
int c_array[3] = {1, 2, 3};
std::array<int, 3> stl_array = {1, 2, 3};
```
Плюсы std::array:
    Есть методы (size(), fill()).
    Передается в функции по значению (как обычный тип).
    Совместим с STL-алгоритмами (std::sort).
Минусы:
    Размер должен быть известен на этапе компиляции.
    
### std::set – уникальные элементы (упорядоченные)
```cpp
#include <set>
std::set<int> s = {3, 1, 2};
s.insert(4); // Вставка (автоматическая сортировка)
```
Плюсы:
    Уникальность элементов.
    Поиск за O(log n).
Минусы:
    Нет доступа по индексу.

### std::map – ключ-значение (упорядоченные)
```cpp
#include <map>
std::map<std::string, int> m = {{"Alice", 25}, {"Bob", 30}};
m["Charlie"] = 35; // Вставка
```
Плюсы:
    Быстрый поиск по ключу (O(log n)).
Минусы:
    Занимает больше памяти, чем unordered_map.
### std::unordered_set / std::unordered_map (хеш-таблицы)
```cpp
#include <unordered_map>
std::unordered_map<std::string, int> um = {{"Alice", 25}};
um["Bob"] = 30; // Вставка (O(1) в среднем)
```
Плюсы:
    Быстрый поиск (O(1) в среднем).
Минусы:
    Элементы не упорядочены.
### std::stack – LIFO (последний вошел – первый вышел)
```cpp
#include <stack>
std::stack<int> st;
st.push(1); // Добавить
st.pop();   // Удалить
```
### std::queue – FIFO (первый вошел – первый вышел)
```cpp
#include <queue>
std::queue<int> q;
q.push(1); // В конец
q.pop();   // Удалить первый
```
### std::priority_queue – очередь с приоритетом
```cpp
#include <queue>
std::priority_queue<int> pq;
pq.push(3); // Автоматическая сортировка
pq.pop();   // Удаление наибольшего
```

## Пример
```cpp
#include <array>
#include <vector>
#include <set>
#include <string>
#include <deque>
#include <list>
#include <map>

// Статический массив
std::array<int, 5> arr = {1, 2, 3, 4, 5};  // Исправлено: убраны кавычки у размера
arr.at(1);   // Доступ с проверкой границ
arr.size();  // Размер

// Динамический массив
std::vector<int> vec;
vec.push_back(10);

// Уникальные отсортированные элементы
std::set<int> unique_set = {3, 1, 2};  // Станет {1, 2, 3}
std::multiset<int> multi_set;  // С дубликатами

// Строки
std::string text = "Hello";

// Двусторонняя очередь
std::deque<int> dq = {1, 2, 3};
dq.push_front(0);

// Связный список
std::list<int> lst;

// Словари
std::map<std::string, int> my_map = {{"a", 1}};
std::multimap<std::string, int> multi_map;
```
#include <iostream>
#include <vector>
#include <map>
using namespace std;

int main() {
    // Vector example
    vector<int> arr;
    for (int i = 0; i < 7; i++) {
        arr.push_back(i + 1);
    }

    // Итерация с const_iterator
    vector<int>::const_iterator it = arr.cbegin();
    while (it != arr.cend()) {
        cout << *it << '\t';  // Исправлено: вывод значения
        it++;
    }
    cout << endl;

    // Map example
    map<int, string> mm;
    mm[12] = "que";
    mm.insert(make_pair(3, "qrff"));

    // Итерация по map
    map<int, string>::iterator it2 = mm.begin();
    while (it2 != mm.end()) {
        cout << it2->first << ": " << it2->second << '\t';  // Исправлено форматирование
        ++it2;  // Исправлено: инкремент правильного итератора
    }

    return 0;
}