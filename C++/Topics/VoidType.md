# Тип данных `void`

Тип `void` используется для указания того, что функция не возвращает значение.

```cpp
#include <iostream>
using namespace std;

void printMessage() {
    cout << "Это сообщение выведено из функции с типом void." << endl;
}

int main() {
    printMessage();
    return 0;
}