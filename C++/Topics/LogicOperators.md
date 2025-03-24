# Логические операторы
&& — логическое И.

|| — логическое ИЛИ.

! — логическое НЕ.

```cpp
#include <iostream>
using namespace std;

int main() {
    bool a = true, b = false;
    cout << (a && b) << endl; // 0 (false)
    cout << (a || b) << endl; // 1 (true)
    cout << (!a) << endl;     // 0 (false)
    return 0;
}