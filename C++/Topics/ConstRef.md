# Передача по константной ссылке в C++
## Основные принципы
Передача параметров по константной ссылке (const&) сочетает в себе:
- Эффективность передачи по ссылке (избегает копирования)
- Безопасность передачи по значению (защита от изменений)

### Базовый пример
```cpp

#include <iostream>
#include <string>

void printString(const std::string& str) {
    std::cout << str << std::endl;
    // str[0] = 'A';  // Ошибка компиляции - объект константный
}

int main() {
    std::string text = "Hello, World!";
    printString(text);  // Эффективная передача без копирования
    return 0;
}
```
#### Когда использовать
- **Для больших объектов (чтобы избежать копирования):**
```cpp
void processVector(const std::vector<int>& data);
```
- **Когда нужно гарантировать неизменяемость:**
```cpp
double calculateLength(const Point& p1, const Point& p2);
```
- **Для встроенных типов, когда нужно только чтение:**
```cpp
void printValue(const int& value);
```
## **Сравнение способов передачи параметров**
|Способ                 |	Копирование|	Можно изменить|	Эффективность|
|-----------------------|--------------|------------------|--------------|
|По значению            |	         Да|	    Да (копию)|	       Низкая|
|По ссылке (&)          |	        Нет|	            Да|	      Высокая|
|По константной ссылке  |	        Нет|	           Нет|	      Высокая|

### Пример с пользовательским типом
```cpp

#include <iostream>

struct BigData {
    int values[1000];
    // ... другие большие данные
};

void processData(const BigData& data) {
    // Можем читать данные, но не изменять
    std::cout << "First value: " << data.values[0] << std::endl;
}

int main() {
    BigData myData{};
    myData.values[0] = 42;
    
    processData(myData);  // Передача без копирования массива
    return 0;
}
```
## Особенности
- Можно передавать временные объекты:
```cpp
printString("Temporary string");
```
- Неявное преобразование типов:
```cpp

void printInt(const int& value);
printInt(3.14);  // Будет преобразовано в int
```
- Использование в методах класса:
```cpp
class MyClass {
public:
    void doSomething(const std::string& param) const;
};
`
