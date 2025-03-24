# Шаблоны классов

Шаблоны классов позволяют создавать универсальные классы.

```cpp
#include <iostream>
using namespace std;

template <typename T>
class Box {
    T value;
public:
    Box(T v) { value = v; }
    void show() { cout << "Value: " << value << endl; }
};

int main() {
    Box<int> intBox(10);    // за счёт шаблона можно подставить разные типы данных
    Box<double> doubleBox(3.14);    // на таком и реализованны вектора и тд.

    intBox.show();
    doubleBox.show();
    return 0;
}