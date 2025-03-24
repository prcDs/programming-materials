# Перегрузка функций

Перегрузка функций позволяет создавать несколько функций с одинаковым именем, но разными параметрами.

```cpp
#include <iostream>
using namespace std;

void print(int a) {
    cout << "Целое число: " << a << endl;
}

void print(double a) {
    cout << "Дробное число: " << a << endl;
}

int main() {
    print(10);
    print(3.14);
    return 0;
}