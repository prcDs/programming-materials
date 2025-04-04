# Разница между struct и class в C++

- **`struct:` По умолчанию все члены имеют public-доступ**
```cpp
#include <iostream>
struct A {
    int b; // public по умолчанию
};
```

- **`class:` По умолчанию все члены имеют private-доступ**
```cpp
class C {
    int c; // private по умолчанию
public:
    int d; // Доступ открыт явно
};
```
### Пример вызова обьектов класса и структуры:
```cpp
int main() {
    A objA;
    objA.b = 10;  // Корректно, так как b имеет public-доступ
    
    C objC;
    // objC.c = 10;  // Ошибка, так как c имеет private-доступ
    objC.d = 10;    // Корректно, так как d объявлен public
    
    std::cout << "A.b = " << objA.b << "\n";
    std::cout << "C.d = " << objC.d << "\n";
    
    return 0;
}
```