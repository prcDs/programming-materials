# Лямбда-выражения

Лямбда-выражения позволяют создавать анонимные функции прямо в коде. Они особенно полезны для кратких операций, которые не требуют отдельного объявления функции.
## Базовый синтаксис
`[захват](параметры) -> возвращаемый_тип { тело_функции }`
## 1. Простые лямбда-выражения
### 1.1. Лямбда без параметров
```cpp

#include <iostream>
using namespace std;

int main() {
    auto greet = []() {
        cout << "Привет, мир!" << endl;
    };
    
    greet();  // Вызов лямбда-выражения
    return 0;
}
```
### Вывод:

```
Привет, мир!
```
### 1.2. Сравнение с обычной функцией
```cpp

#include <iostream>

void hello1() {
    std::cout << "HELLO";
}

int main() {
    hello1();  // Обычная функция
    auto hello = []() { std::cout << " HELLO from lambda"; };
    hello();   // Лямбда-выражение
    return 0;
}
```

### Вывод:
```
HELLO HELLO from lambda
```

## 2. Лямбды с параметрами и возвращаемым значением
### 2.1. Лямбда с параметрами
```cpp

#include <iostream>
using namespace std;

int main() {
    auto add = [](int a, int b) {
        return a + b;
    };

    cout << "Сумма: " << add(5, 10) << endl;
    return 0;
}
```
### Вывод:
```
Сумма: 15
```

### 2.2. Сравнение с обычной функцией
```cpp

#include <iostream>

int sum1(int a, int b) {
    return a + b;
}

int main() {
    std::cout << sum1(12, 12) << std::endl;  // Обычная функция
    auto sum = [](int a, int b) { return a + b; };
    std::cout << sum(12, 12) << std::endl;   // Лямбда
    return 0;
}
```
### Вывод:
```
24
24
```
## 3. Захват переменных из окружающего контекста
### 3.1. Захват по значению ([=])
```cpp

#include <iostream>
using namespace std;

int main() {
    int x = 10, y = 20;
    
    auto printSum = [=]() {  // Захват x и y по значению
        cout << x + y << endl;
    };
    
    printSum();  // 30
    return 0;
}
```
### 3.2. Захват по ссылке ([&])
```cpp

#include <iostream>
using namespace std;

int main() {
    int counter = 0;
    
    auto increment = [&]() {  // Захват по ссылке
        counter++;
    };
    
    increment();
    cout << counter << endl;  // 1
    return 0;
}
```

### 3.3. Явный захват конкретных переменных
```cpp

#include <iostream>
using namespace std;

int main() {
    int a = 5, b = 10, c = 15;
    
    auto sum = [a, &b](int d) {  // a по значению, b по ссылке
        b = 20;
        return a + b + d;
    };
    
    cout << sum(c) << endl;  // 5 + 20 + 15 = 40
    cout << b << endl;       // 20 (значение изменилось)
    return 0;
}
```
## 4. Возвращаемый тип
### 4.1. Автоматическое определение типа

`auto multiply = [](int a, int b) { return a * b; };`
### 4.2. Явное указание типа
`auto divide = [](double a, double b) -> double { return a / b; };`
## 5. Лямбды в алгоритмах STL
### Пример с `std::sort`
```cpp

#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> numbers = {5, 2, 8, 1, 9};
    
    // Сортировка по убыванию
    std::sort(numbers.begin(), numbers.end(), [](int a, int b) {
        return a > b;
    });
    
    for (int n : numbers) {
        std::cout << n << " ";
    }
    return 0;
}
```
### Вывод:
```
9 8 5 2 1
```
## 6. Рекомендации по использованию
- Используйте лямбды для кратких операций, которые не требуют повторного использования

- Для сложной логики лучше создать именованную функцию

- Будьте осторожны с захватом по ссылке - убедитесь, что захваченные переменные существуют при вызове лямбды

- Используйте явный захват ([var1, &var2]) вместо общего ([=] или [&]) для ясности кода