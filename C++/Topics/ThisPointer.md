# Скрытый указатель `this`

Указатель `this` используется в методах класса для ссылки на текущий объект.

```cpp
#include <iostream>
using namespace std;

class Example {
    int a;
public:
    Example(int a) { this->a = a; }
    void show() { cout << "a = " << this->a << endl; }
};

int main() {
    Example obj(5);
    obj.show();
    return 0;
}