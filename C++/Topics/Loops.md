# Циклы

Циклы позволяют повторять выполнение блока кода несколько раз.

## Цикл for

```cpp
#include <iostream>
using namespace std;

int main() {
    for (int i = 0; i < 5; i++) {
        cout << "i = " << i << endl;
    }
    return 0;
}
```

## Цикл while

```cpp
#include <iostream>
using namespace std;

int main() {
    int i = 0;
    while (i < 5) {
        cout << "i = " << i << endl;
        i++;
    }
    return 0;
}
```

## Цикл do-while

```cpp
#include <iostream>
using namespace std;

int main() {
    int i = 0;
    do {
        cout << "i = " << i << endl;
        i++;
    } while (i < 5);
    return 0;
}
```
