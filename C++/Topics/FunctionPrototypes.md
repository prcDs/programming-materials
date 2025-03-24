# Прототип функции и предварительное объявление

Прототип функции позволяет объявить функцию до её определения.

```cpp
#include <iostream>
using namespace std;

// Прототип функции
void greet();

int main() {
    greet();
    return 0;
}

// Определение функции
void greet() {
    cout << "Привет, мир!" << endl;
}