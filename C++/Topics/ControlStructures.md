# Управляющие конструкции

Управляющие конструкции позволяют управлять потоком выполнения программы.

## Условный оператор if-else

```cpp
#include <iostream>
using namespace std;

int main() {
    int age = 18;

    if (age >= 18) {
        cout << "Вы совершеннолетний." << endl;
    } else {
        cout << "Вы несовершеннолетний." << endl;
    }

    return 0;
}