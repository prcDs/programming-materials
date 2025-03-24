# Умные указатели
Позволяют автоматически управлять памятью.

## std::unique_ptr – эксклюзивное владение
```cpp

#include <memory>
std::unique_ptr<int> ptr = std::make_unique<int>(10);// Нельзя копировать, только перемещать
```
## std::shared_ptr – разделяемое владение (счетчик ссылок)
```cpp

std::shared_ptr<int> ptr1 = std::make_shared<int>(20);
auto ptr2 = ptr1; // Счетчик ссылок = 2
```
## std::weak_ptr – слабая ссылка (без увеличения счетчика)
```cpp

std::weak_ptr<int> wptr = ptr1;
if (auto sptr = wptr.lock()) { // Проверка, жив ли объект
    std::cout << *sptr;
}