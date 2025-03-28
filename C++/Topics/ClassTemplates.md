# Шаблоны классов в C++

Шаблоны классов позволяют создавать универсальные классы для работы с разными типами данных.

## 1. Простой шаблон класса
```cpp
#include <iostream>
using namespace std;

template <typename T>
class Box {
    T value;
public:
    Box(T v) : value(v) {}
    void show() { cout << "Value: " << value << endl; }
};

int main() {
    Box<int> intBox(10);
    Box<double> doubleBox(3.14);
    
    intBox.show();     // Value: 10
    doubleBox.show();  // Value: 3.14
    return 0;
}
```
## 2. Шаблон класса с несколькими параметрами
```cpp
template <typename T, typename U>
class Pair {
    T first;
    U second;
public:
    Pair(T f, U s) : first(f), second(s) {}
    void display() {
        cout << "(" << first << ", " << second << ")\n";
    }
};

int main() {
    Pair<int, double> p1(10, 3.14);
    Pair<string, bool> p2("Open", true);
    
    p1.display();  // (10, 3.14)
    p2.display();  // (Open, 1)
    return 0;
}
```
## 3. Шаблон класса-контейнера
```cpp
template <typename T>
class Stack {
    vector<T> elements;
public:
    void push(T value) {
        elements.push_back(value);
    }
    T pop() {
        T val = elements.back();
        elements.pop_back();
        return val;
    }
};

int main() {
    Stack<int> intStack;
    intStack.push(10);
    intStack.push(20);
    cout << intStack.pop() << endl;  // 20
    return 0;
}
```
### Применение в STL
Шаблоны классов используются в:
- `vector<T>`
- `list<T>`
- `map<K, V>`
- и других контейнерах