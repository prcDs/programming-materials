# Оператор `sizeof`

Оператор `sizeof` возвращает размер типа или переменной в байтах.

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "Размер int: " << sizeof(int) << " байт" << endl;
    cout << "Размер double: " << sizeof(double) << " байт" << endl;
    cout << "Размер char: " << sizeof(char) << " байт" << endl;
    return 0;
}