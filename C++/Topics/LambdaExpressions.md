# Лямбда-выражения

Лямбда-выражения позволяют создавать анонимные функции прямо в коде.

```cpp
#include <iostream>
using namespace std;

int main() {
    auto greet = []() {
        cout << "Привет, мир!" << endl;
    };

    greet();
    return 0;
}
```

## Лямбда с параметрами
```cpp
#include <iostream>
using namespace std;

int main() {
    auto add = [](int a, int b) {
        return a + b;
    };

    cout << "Сумма: " << add(5, 10) << endl;
    return 0;
}

