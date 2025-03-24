# Шаблоны функций

Шаблоны функций позволяют создавать универсальные функции для разных типов данных.

```cpp
#include <iostream>
using namespace std;

template <typename T>
T add(T a, T b) {
    return a + b;
}

int main() {
    cout << add(5, 10) << endl;      // Целые числа
    cout << add(3.5, 2.5) << endl;  // Дробные числа
    return 0;
}