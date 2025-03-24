# Оператор switch-case

```cpp
#include <iostream>
using namespace std;

int main() {
    int choice = 2;

    switch (choice) {
        case 1:
            cout << "Вы выбрали 1" << endl;
            break;
        case 2:
            cout << "Вы выбрали 2" << endl;
            break;
        default:
            cout << "Некорректный выбор" << endl;
    }

    return 0;
}