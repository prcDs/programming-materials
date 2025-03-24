# Константы

Константы объявляются с помощью ключевого слова `const`.

```cpp
#include <iostream>
using namespace std;

int main() {
    const double PI = 3.14159;
    // PI = 3.14; // Ошибка: нельзя изменить значение константы
    cout << "PI = " << PI << endl;
    return 0;
}