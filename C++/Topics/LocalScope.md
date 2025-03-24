# Локальная область видимости

Переменные, объявленные внутри блока, видны только внутри этого блока.

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10;

    {
        int b = 20;
        cout << "a внутри блока: " << a << endl;
        cout << "b внутри блока: " << b << endl;
    }

    // cout << "b вне блока: " << b << endl; // Ошибка: b не видна
    cout << "a вне блока: " << a << endl;

    return 0;
}