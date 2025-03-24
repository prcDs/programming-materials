# Генерация случайных чисел
## Устаревший способ
```cpp
#include <cstdlib>
#include <ctime>

int main() {
    srand(time(0));
    std::cout << rand() % 100; // Не рекомендуется для серьезных проектов
}
```
## Современный способ (C++11+)
```cpp
#include <random>
#include <iostream>

int main() {
    std::random_device rd;
    std::mt19937 gen(rd());
    std::uniform_int_distribution<> dist(1, 100);
    std::cout << dist(gen); // Случайное число от 1 до 100
}