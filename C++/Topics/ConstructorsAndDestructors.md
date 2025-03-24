# Конструкторы и деструкторы

Конструкторы и деструкторы используются для инициализации и очистки объектов.

## Конструктор

```cpp
#include <iostream>
using namespace std;

class Example {
    int a;
public:
    Example(int value) { a = value; }
    void show() { cout << "a = " << a << endl; }
};

int main() {
    Example obj(10);
    obj.show();
    return 0;
}
```
## Деструктор

```cpp
#include <iostream>
using namespace std;

class Example {
public:
    Example() { cout << "Конструктор" << endl; }
    ~Example() { cout << "Деструктор" << endl; }
};

int main() {
    Example obj;
    return 0;
}