# Структуры в C++
## Базовое использование структур
Структуры (struct) в C++ позволяют объединять данные разных типов под одним именем.

### Пример структуры Person:
```cpp

#include <iostream>
#include <string>
using namespace std;

struct Person {
    int age;
    double weight;
    double height;
    string name;
};

struct Point {
    double x = 0.0;  // Значения по умолчанию
    double y = 0.0;
};

int main() {
    // Инициализация структур
    Person p1 = {25, 70.5, 180.0, "Alice"};  // Стандартный синтаксис
    Person p2 {30, 80.2, 175.0, "Bob"};      // Uniform initialization (C++11)
    
    Point pt;
    pt.x = 10.5;  // Доступ к полям через точку
    
    cout << p1.name << " is " << p1.age << " years old\n";
    return 0;
}
```
## Инициализация структур
В C++ доступно несколько способов инициализации структур:

- **Агрегатная инициализация:**
```cpp

Person p = {25, 70.5, 180.0, "Alice"};
Uniform initialization (C++11):
```

```
Person p {30, 80.2, 175.0, "Bob"};
```

- **Значения по умолчанию:**
```cpp

struct Point {
    double x = 0.0;
    double y = 0.0;
};
```
- **Постепенное заполнение:**
```cpp
Person p;
p.age = 25;
p.name = "Alice";
```
- **Пример с дробями**
Работа со структурами на примере дробей:
```cpp

#include <iostream>
using namespace std;

struct Fraction {
    int numerator;
    int denominator;
};

// Функция для умножения дробей
double multiply(Fraction a, Fraction b) {
    return (a.numerator * b.numerator) / 
           static_cast<double>(a.denominator * b.denominator);
}

int main() {
    Fraction f1, f2;
    
    // Ввод данных
    cout << "Enter first fraction (num denom): ";
    cin >> f1.numerator >> f1.denominator;
    
    cout << "Enter second fraction (num denom): ";
    cin >> f2.numerator >> f2.denominator;
    
    // Вычисление и вывод результата
    cout << "Result: " << multiply(f1, f2) << endl;
    
    return 0;
}
```

#### Рекомендации по использованию
- Используйте struct для простых типов данных, которые не требуют инкапсуляции
- Для сложных объектов предпочитайте class
- Добавляйте методы в структуры, если они непосредственно работают с данными:
```cpp

struct Fraction {
    int numerator;
    int denominator;
    
    double toDouble() const {
        return static_cast<double>(numerator) / denominator;
    }
};
```
- Используйте современные способы инициализации (C++11 и новее)
- Для больших структур передавайте по ссылке:
```cpp

void printPerson(const Person& p) {
    cout << p.name << ", " << p.age << " years";
}
```
- Рассмотрите возможность использования конструкторов:
```cpp

struct Point {
    double x, y;
    Point(double x, double y) : x(x), y(y) {}
};
```

