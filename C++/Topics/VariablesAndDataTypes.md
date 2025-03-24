
# Переменные и типы данных

Переменные используются для хранения данных в программе. Каждая переменная имеет тип, который определяет, какие данные она может хранить.

## Основные типы данных

- `int` — целые числа.
- `float` — числа с плавающей точкой.
- `double` — числа с двойной точностью.
- `char` — символы.
- `string` - текст.
- `bool` — логические значения (true/false).

```cpp
#include <iostream>
using namespace std;

int main() {
    int age = 25;
    float weight = 70.5;
    double height = 175.5;
    char gender = 'M';
    string name = "BOB";
    bool isStudent = true;

    cout << "Возраст: " << age << endl;
    cout << "Вес: " << weight << endl;
    cout << "Рост: " << height << endl;
    cout << "Пол: " << gender << endl;
    cout << "Имя:" << name << endl;
    cout << "Студент: " << isStudent << endl;

    return 0;
}