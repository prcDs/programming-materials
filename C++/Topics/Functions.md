# Функции в C++

Функции позволяют организовать код в логические блоки, которые можно вызывать многократно.

## Простая функция

```cpp
#include <iostream>
using namespace std;

void greet() {
    cout << "Привет, мир!" << endl;
}

int main() {
    greet();
    return 0;
}
```

## Функция с параметрами и возвращаемым значением

```cpp
#include <iostream>
using namespace std;

int add(int a, int b) {
    return a + b;
}

int main() {
    int result = add(5, 10);
    cout << "Сумма: " << result << endl;
    return 0;
}