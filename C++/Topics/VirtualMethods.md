# Виртуальные методы в C++

## Определение
Виртуальные методы позволяют реализовать **полиморфизм**. Они объявляются с ключевым словом `virtual` и могут быть переопределены в дочерних классах.

## Пример
```cpp
class Base {
public:
    virtual void show() { // Виртуальный метод
        cout << "Base class\n";
    }
};

class Derived : public Base {
public:
    void show() override { // Переопределение
        cout << "Derived class\n";
    }
};

int main() {
    Base* obj = new Derived();
    obj->show(); // Выведет "Derived class"
    delete obj;
}
```
### Когда использовать?
1.Когда нужно переопределить метод в дочернем классе.
2.Для реализации полиморфизма.



# Чистые виртуальные методы

## Определение
Чистый виртуальный метод не имеет реализации в базовом классе и делает класс **абстрактным**. Объявляется с `= 0`.

## Пример
```cpp
class AbstractClass {
public:
    virtual void pureVirtual() = 0; // Чистый виртуальный метод
};

class Concrete : public AbstractClass {
public:
    void pureVirtual() override {
        cout << "Implemented in derived class\n";
    }
};
```
### Особенности
1.Нельзя создать объект абстрактного класса.
2.Дочерние классы обязаны реализовать все чистые виртуальные методы.


# Абстракция в C++

## Суть
Абстракция позволяет скрыть детали реализации, показывая только необходимый функционал.

## Пример
```cpp
class Animal {
public:
    virtual void makeSound() = 0; // Абстрактный метод
};

class Dog : public Animal {
public:
    void makeSound() override {
        cout << "Woof!\n";
    }
};
```
### Применение
1.Для создания интерфейсов.
2.Чтобы уменьшить сложность кода.