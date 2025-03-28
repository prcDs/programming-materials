# Шаблоны функций в C++

Шаблоны функций позволяют создавать универсальные функции для работы с разными типами данных.

## 1. Базовый пример
```cpp
#include <iostream>
using namespace std;

template <typename T>
T add(T a, T b) {
    return a + b;
}

int main() {
    cout << add(5, 10) << endl;      // Работает с int
    cout << add(3.5, 2.5) << endl;   // Работает с double
    return 0;
}
```
### Вывод:
```
15
6.0
```
## 2. Функция для нахождения максимума
```cpp
template <typename T>
const T& max(const T& a, const T& b) {
    return (a > b) ? a : b;
}

int main() {
    cout << max(12, 2) << "\n";       // int
    cout << max(12.32, 2.32) << "\n"; // double
    cout << max('b', 'a') << "\n";    // char
    return 0;
}
```
## 3. Вычисление среднего значения массива
```cpp
template <class T>
T average(T* array, int length) {
    T sum = 0;
    for (int i = 0; i < length; ++i)
        sum += array[i];
    return sum / length;
}

int main() {
    int arr1[] = {1, 2, 3, 4, 5};
    double arr2[] = {2.2, 4.1, 5.7, 3.2};
    cout << average(arr1, 5) << "\n";  // 3
    cout << average(arr2, 4) << "\n";  // 3.8
    return 0;
}
```
## 4. Вариативные шаблоны (C++11)
```cpp
template<typename T>
T min(T a, T b) {
    return (a < b) ? a : b;
}

template<typename T, typename... Args>
T min(T a, Args... args) {
    return min(a, min(args...));
}

int main() {
    cout << min(5, 2, 8, 1) << "\n";  // Находит минимум: 1
    return 0;
}
```
Особенности
- Компилятор генерирует конкретные версии функций при их использовании
- Поддерживается перегрузка шаблонных функций
- Можно указывать параметры шаблона вручную: `add<double>(5, 2.5)`