# Умные указатели
Умные указатели автоматически управляют памятью, предотвращая утечки и обеспечивая безопасную работу с динамической памятью.

## std::unique_ptr – эксклюзивное владение
### Основные характеристики:
- Эксклюзивное владение ресурсом
- Нельзя копировать, только перемещать
- Автоматическое освобождение памяти при выходе из области видимости
```cpp
#include <memory>
std::unique_ptr<int> ptr = std::make_unique<int>(10);// Нельзя копировать, только перемещать
```
#### Пример использования
```cpp
#include <iostream>
#include <memory>

int main() {
    // Создание unique_ptr
    std::unique_ptr<int> ptr = std::make_unique<int>(20);
    std::cout << "Value: " << *ptr << std::endl;

    // Перемещение владения
    std::unique_ptr<int> ptr2 = std::move(ptr);
    if (!ptr) {
        std::cout << "Original pointer is now empty" << std::endl;
    }

    std::cout << "Value of new ptr: " << *ptr2 << std::endl;
    return 0;
} 
```
## std::shared_ptr – разделяемое владение (счетчик ссылок)
### Основные характеристики:
- Разделяемое владение ресурсом
- Использует счетчик ссылок
- Автоматическое освобождение при обнулении счетчика
```cpp
std::shared_ptr<int> ptr1 = std::make_shared<int>(20);
auto ptr2 = ptr1; // Счетчик ссылок = 2
```
#### Пример использования
```cpp
#include <iostream>
#include <memory>
using namespace std;

int main() {
    shared_ptr<int> ptr1 = make_shared<int>(20);
    shared_ptr<int> ptr2 = ptr1;
    
    cout << "Value: " << *ptr1 << endl;
    cout << "Count of pointers: " << ptr1.use_count() << endl;  // 2
    
    ptr2.reset();
    cout << "Count after reset: " << ptr1.use_count() << endl;  // 1
    
    return 0;
}
```
## std::weak_ptr – слабая ссылка (без увеличения счетчика)
### Основные характеристики:
- Слабая ссылка без увеличения счетчика
- Не влияет на время жизни объекта
- Позволяет проверять существование объекта
```cpp

std::weak_ptr<int> wptr = ptr1;
if (auto sptr = wptr.lock()) { // Проверка, жив ли объект
    std::cout << *sptr;
}
```
#### Пример использования
```cpp
#include <iostream>
#include <memory>
using namespace std;

int main() {
    shared_ptr<int> ptr1 = make_shared<int>(20);
    weak_ptr<int> ptr2 = ptr1;
    
    if (auto temp = ptr2.lock()) {
        cout << "Value: " << *temp << endl;
    }
    
    ptr1.reset();
    
    if (auto temp = ptr2.lock()) {
        cout << "Value still exists" << endl;
    } else {
        cout << "Value destroyed" << endl;
    }
    
    return 0;
}
```
## Пользовательский AutoPtr
#### Пример реализации простого умного указателя:
```cpp
#include <iostream>
using namespace std;

template<class T>
class AutoPtr {
public:
    explicit AutoPtr(T* ptr = nullptr) : m_ptr(ptr) {}
    ~AutoPtr() { delete m_ptr; }
    
    // Запрет копирования
    AutoPtr(const AutoPtr&) = delete;
    AutoPtr& operator=(const AutoPtr&) = delete;
    
    // Разрешение перемещения
    AutoPtr(AutoPtr&& other) noexcept : m_ptr(other.m_ptr) {
        other.m_ptr = nullptr;
    }
    
    AutoPtr& operator=(AutoPtr&& other) noexcept {
        if (this != &other) {
            delete m_ptr;
            m_ptr = other.m_ptr;
            other.m_ptr = nullptr;
        }
        return *this;
    }
    
    T& operator*() const { return *m_ptr; }
    T* operator->() const { return m_ptr; }
    
private:
    T* m_ptr;
};

class Item {
public:
    Item() { cout << "ITEM CREATED\n"; }
    ~Item() { cout << "ITEM DELETED\n"; }
    void sayHi() { cout << "HELLO\n"; }
};

int main() {
    AutoPtr<Item> ptr(new Item);
    ptr->sayHi();
    return 0;
}
```
#### Примеры исп. умных указателей
- Пример 1
```cpp
#include <iostream>
using namespace std;

template<class T>
class AutoPtr {
public:
    explicit AutoPtr(T* ptr = nullptr) : m_ptr(ptr) {}
    ~AutoPtr() { delete m_ptr; }
    
    T& operator*() const { return *m_ptr; }  // Исправлено: правильный оператор
    T* operator->() const { return m_ptr; }
    
private:
    T* m_ptr;
};

class Item {
public:
    Item() { cout << "ITEM CREATED\n"; }
    ~Item() { cout << "ITEM DELETED\n"; }
    void sayHi() { cout << "HELLO\n"; }
};

int main() {
    AutoPtr<Item> ptr(new Item);
    ptr->sayHi();
    return 0;
}
```
- Пример 2
```cpp
#include <iostream>
#include <memory>
using namespace std;

int main() {
    shared_ptr<int> ptr1 = make_shared<int>(20);
    shared_ptr<int> ptr2 = ptr1;
    
    cout << "Value: " << *ptr1 << endl;
    cout << "Count of pointers: " << ptr1.use_count() << endl;
    
    ptr2.reset();
    cout << "Count after reset: " << ptr1.use_count() << endl;
    
    return 0;
}
```
- Пример 3
```cpp
#include <iostream>
#include <memory>
using namespace std;

int main() {
    shared_ptr<int> ptr1 = make_shared<int>(20);
    weak_ptr<int> ptr2 = ptr1;
    
    if (auto temp = ptr2.lock()) {
        cout << "Value: " << *temp << endl;
    } else {
        cout << "Value destroyed\n";
    }
    
    ptr1.reset();
    
    if (auto temp = ptr2.lock()) {
        cout << "Value: " << *temp << endl;
    } else {
        cout << "Value destroyed\n";  // Ожидаемый вывод
    }
    
    return 0;
}
```
- Пример 4
```cpp
#include <iostream>
#include <memory>

int main() {
    std::unique_ptr<int> ptr = std::make_unique<int>(20);
    std::cout << "Value: \t" << *ptr << std::endl;

    std::unique_ptr<int> ptr2 = std::move(ptr);
    if (!ptr) {
        std::cout << "Original pointer is now empty\n";
    }

    std::cout << "Value of new ptr: \t" << *ptr2 << std::endl;
    return 0;
}
```
### **Рекомендации по использованию**
- Предпочитайте `make_unique/make_shared`:
```cpp
auto ptr = std::make_unique<int>(10);
auto ptr = std::make_shared<int>(10);
```
- Используйте `unique_ptr` по умолчанию, если не нужен `shared_ptr`
- Для кэширования и наблюдателей используйте `weak_ptr`


### **Избегайте:**

- Создания умных указателей из сырых указателей
- Циклических ссылок между shared_ptr
- Для массивов используйте специализированные версии:
```cpp
std::unique_ptr<int[]> arr(new int[10]);
```

|Сравнение умных указателей                                           |
|----------------------|----------------|--------------|--------------|
|Характеристика        |	unique_ptr	|shared_ptr	   |weak_ptr      |
|Владение              |	Единоличное |	Разделяемое|	Нет       |
|Копирование           |	Нет         |	Да         |    Да        |
|Перемещение           |	Да          |	Да         |	Да        |
|Счетчик ссылок        |	Нет         |	Да         |	Да        |
|Проверка на валидность|	Нет         |	Нет        |	Да (lock) |
|Производительность    |	Высокая     |	Ниже       |	Средняя   |
