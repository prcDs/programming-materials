# Управление динамической памятью в C++
## 1. Работа с динамической памятью
### 1.1. Базовые операции
```cpp

// Выделение памяти для одного элемента
int* num = new int(42);  // Инициализация значением 42
delete num;              // Освобождение памяти

// Выделение памяти для массива
int* arr = new int[10];  // Массив из 10 элементов
delete[] arr;            // Освобождение массива
```
## 1.2. Проблемы ручного управления
- Утечки памяти (если забыть delete)
- Двойное освобождение (если вызвать delete дважды)
- Висячие указатели (использование после освобождения)

## 2. Динамические массивы
### 2.1. Классический подход (не рекомендуется)
```cpp
#include <iostream>

int main() {
    size_t size;
    std::cout << "Введите размер массива: ";
    std::cin >> size;
    
    // Выделение памяти
    int* dynamicArray = new int[size];
    
    // Заполнение массива
    for (size_t i = 0; i < size; ++i) {
        dynamicArray[i] = i * 2;
    }
    
    // Использование массива
    for (size_t i = 0; i < size; ++i) {
        std::cout << dynamicArray[i] << " ";
    }
    
    // Освобождение памяти
    delete[] dynamicArray;
    return 0;
}
```
### 2.2. Современный подход с std::vector (рекомендуется)
```cpp

#include <iostream>
#include <vector>

int main() {
    size_t size;
    std::cout << "Введите размер массива: ";
    std::cin >> size;
    
    // Создание вектора
    std::vector<int> vec(size);
    
    // Заполнение вектора
    for (size_t i = 0; i < size; ++i) {
        vec[i] = i * 2;
    }
    
    // Использование вектора
    for (auto num : vec) {
        std::cout << num << " ";
    }
    
    // Память освобождается автоматически
    return 0;
}
```
## 3. Сравнение подходов
|Характеристика         |	new/delete          |	std::vector             |
|-----------------------|-----------------------|---------------------------|
|Управление памятью     |	Вручную             |	Автоматическое          |
|Безопасность           |	Низкая              |	Высокая                 |
|Изменение размера      |	Сложно              |	resize(), push_back()   |
|Передача в функции     |	Указатель + размер  |   Сам знает свой размер   |
|Исключения             |	Опасны              |	Безопасны               |
|Производительность     |	Чуть выше           |	Оптимизирован           |
## 4. Рекомендации
- Всегда предпочитайте std::vector raw-указателям с new/delete
- Для одиночных объектов используйте умные указатели:
```cpp

#include <memory>
std::unique_ptr<int> ptr = std::make_unique<int>(42);
```
- Если нужно изменить размер массива:
```cpp
vec.resize(new_size);  // Для вектора
```
**vs**
```cpp

// Для raw-указателя:
int* new_arr = new int[new_size];
std::copy(old_arr, old_arr + std::min(old_size, new_size), new_arr);
delete[] old_arr;
```
## 5. Пример с двумерным массивом
### Плохой вариант:
```cpp

int** matrix = new int*[rows];
for (int i = 0; i < rows; ++i) {
    matrix[i] = new int[cols];
}
// ... использование ...
for (int i = 0; i < rows; ++i) {
    delete[] matrix[i];
}
delete[] matrix;
```
### Хороший вариант:
```cpp

#include <vector>
std::vector<std::vector<int>> matrix(rows, std::vector<int>(cols));
// Память освобождается автоматически