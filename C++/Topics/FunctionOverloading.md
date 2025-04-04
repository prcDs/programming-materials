# Перегрузка функций в C++

## Определение
Перегрузка функций - это возможность определить несколько функций с одинаковым именем, но разными параметрами.

## Ключевые особенности
1. Функции должны отличаться типом или количеством параметров
2. Возвращаемое значение не учитывается при перегрузке
3. Компилятор выбирает подходящую функцию на основе аргументов при вызове

## Пример кода
```cpp
#include <iostream>
#include <string>
using namespace std;

// Перегрузка функции print
void print(int a) {
    cout << "Целое число: " << a << endl;
}

void print(double a) {
    cout << "Дробное число: " << a << endl;
}

void print(const string& s) {
    cout << "Строка: " << s << endl;
}

// Перегрузка функции get
int get(int a) { return a; }
double get(double a) { return a; }
const string& get(const string& a) { return a; }

int main() {
    // Использование перегруженной функции print
    print(10);
    print(3.14);
    print("Hello, world!");

    // Использование перегруженной функции get
    cout << get(8) << "\n";
    cout << get(12.2) << "\n";
    cout << get("hello") << "\n";
    return 0;
}
```
## Преимущества перегрузки функций
1. Улучшает читаемость кода
2. Позволяет использовать интуитивно понятные имена для схожих операций
3. Обеспечивает типобезопасность

## Правила разрешения перегрузки
1. Точное соответствие
2. Продвижение (promotion) типов
3. Стандартное преобразование типов
4. Пользовательское преобразование типов

### Ограничения
- Нельзя перегружать функции, отличающиеся только возвращаемым типом
- Осторожно с неявными преобразованиями типов, которые могут привести к неоднозначности

# Перегрузка операторов
Перегрузка функций также применима к операторам:
```cpp
class Complex {
public:
    Complex(double r = 0, double i = 0) : real(r), imag(i) {}

    Complex operator+(const Complex& other) const {
        return Complex(real + other.real, imag + other.imag);
    }

private:
    double real, imag;
};
