# Указатели в C++
## 1. Основы указателей
### 1.1. Объявление и инициализация
```cpp

int a = 10;
int* ptr = &a;  // ptr хранит адрес переменной a
```
### 1.2. Операторы работы с указателями
```cpp

#include <iostream>
using namespace std;

int main() {
    int value = 42;
    
    // Оператор взятия адреса (&)
    cout << "Адрес value: " << &value << endl;
    
    // Оператор разыменования (*)
    int* ptr = &value;
    cout << "Значение через указатель: " << *ptr << endl;  // 42
    
    // Изменение значения через указатель
    *ptr = 100;
    cout << "Новое значение value: " << value << endl;  // 100
    
    return 0;
}
```
## 2. Указатели и классы
### 2.1. Доступ к членам класса через указатель
```cpp

#include <iostream>

class MyClass {
public:
    int a;
    int b;
    void print() {
        cout << a << "\t" << b << endl;
    }
};

int main() {
    MyClass obj = {12, 3};
    MyClass* ptr = &obj;
    
    // Два способа доступа к членам:
    (*ptr).print();  // 12    3
    ptr->print();    // 12    3 (более предпочтительный способ)
    
    return 0;
}
```
## 3. Указатели и массивы
### 3.1. Арифметика указателей
```cpp

#include <iostream>
using namespace std;

int main() {
    int arr[] = {10, 20, 30, 40, 50};
    int* ptr = arr;  // Эквивалентно &arr[0]
    
    cout << *ptr << endl;    // 10
    cout << *(ptr + 2) << endl;  // 30 (ptr[2])
    
    // Итерация по массиву
    for (int i = 0; i < 5; i++) {
        cout << *(ptr + i) << " ";  // 10 20 30 40 50
    }
    
    return 0;
}
```
## 4. Указатели в функциях
### 4.1. Передача параметров по указателю
```cpp

#include <iostream>
using namespace std;

void increment(int* x) {
    (*x)++;  // Изменяем значение по указателю
}

int main() {
    int num = 5;
    increment(&num);
    cout << num;  // 6
    return 0;
}
```
### 4.2. Возврат указателя из функции
```cpp

int* createArray(int size) {
    int* arr = new int[size];
    return arr;
}

// Не забывайте освобождать память!
```
## Рекомендации по безопасности
- Всегда инициализируйте указатели
```cpp

int* ptr = nullptr;  // Хорошо
int* bad_ptr;        // Плохо (может указывать куда угодно)
```
- Проверяйте указатели перед использованием
```cpp

if (ptr != nullptr) {
    *ptr = 10;
}
```
- Предпочитайте умные указатели (unique_ptr, shared_ptr)
- Для массивов используйте std::vector или std::array

- [Умные указатели](./SmartPointers.md)