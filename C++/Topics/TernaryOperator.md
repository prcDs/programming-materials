# Условный тернарный оператор

Тернарный оператор (`? :`) позволяет сократить запись условного выражения.

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10, b = 20;
    int max = (a > b) ? a : b;
    cout << "Максимум: " << max << endl;
    return 0;
}