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
```
## Вывод:
```
Size int: 4 bytes
Size double: 8 bytes
Size char: 1 bytes
```