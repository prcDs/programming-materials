# Заголовочные (.h) и исходные (.cpp) файлы

## Пример структуры
- `myclass.h`:
```cpp
#pragma once
class MyClass {
public:
    void method();
};
```
-`myclass.cpp`:
```cpp
#include "myclass.h"
void MyClass::method() { /* реализация */ }
```
### Правила
Объявления — в .h.

Определения — в .cpp.

Используйте #pragma once для защиты от двойного включения.