# Динамическая память
```cpp
int* arr = new int[10]; // Выделение памяти
delete[] arr;           // Освобождение
```

# Динамические массивы

## Через `new/delete`
```cpp
int* arr = new int[size];
delete[] arr;
```

### Тот же код но через вектор
```cpp
#include <vector>
std::vector<int> vec(size); // Лучше использовать вектор!