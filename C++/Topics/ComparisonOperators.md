# Операторы сравнения
== — равно.

!= — не равно.

> — больше.

< — меньше.

>= — больше или равно.

<= — меньше или равно.

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10, b = 20;
    cout << (a == b) << endl; // 0 (false)
    cout << (a != b) << endl; // 1 (true)
    cout << (a > b) << endl;  // 0 (false)
    cout << (a < b) << endl;  // 1 (true)
    return 0;
}