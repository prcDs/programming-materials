# Абстрактные классы в C++

## Определение
Абстрактный класс - это класс, который содержит хотя бы одну **чистую виртуальную функцию** (объявленную с `= 0`). Такие классы:
- Не могут быть инстанциированы напрямую
- Служат базовыми классами для наследования
- Определяют интерфейс для производных классов

## Пример кода
```cpp
#include <iostream>
using namespace std;

/**
 * Абстрактный класс Animal
 * Содержит чистую виртуальную функцию makeSound()
 */
class Animal {
public:
    // Чисто виртуальная функция
    virtual void makeSound() = 0;

    // Виртуальный деструктор
    virtual ~Animal() {
        cout << "Animal destructor\n";
    }

    // Обычный метод (не виртуальный)
    void breathe() {
        cout << "Breathing...\n";
    }
};

/**
 * Производный класс Dog
 * Должен реализовать makeSound()
 */
class Dog : public Animal {
public:
    void makeSound() override {
        cout << "Woof! Woof!\n";
    }

    ~Dog() override {
        cout << "Dog destructor\n";
    }
};

/**
 * Производный класс Cat
 * Должен реализовать makeSound()
 */
class Cat : public Animal {
public:
    void makeSound() override {
        cout << "Meow! Meow!\n";
    }

    ~Cat() override {
        cout << "Cat destructor\n";
    }
};

int main() {
    // Animal a; // Ошибка! Нельзя создать экземпляр абстрактного класса

    Animal* animals[] = {new Dog(), new Cat()};

    for (Animal* animal : animals) {
        animal->breathe();   // Вызов обычного метода
        animal->makeSound(); // Полиморфный вызов
        delete animal;       // Корректное удаление через виртуальный деструктор
    }

    return 0;
}
```

## Ключевые особенности
1. **Чистые виртуальные функции**:
   ```cpp
   virtual void makeSound() = 0;
   ```
   - Делают класс абстрактным
   - Должны быть реализованы в производных классах

2. **Виртуальный деструктор**:
   ```cpp
   virtual ~Animal()
   ```
   - Гарантирует правильный порядок вызова деструкторов
   - Обязателен, если есть виртуальные функции

3. **Обычные методы**:
   ```cpp
   void breathe()
   ```
   - Наследуются "как есть"
   - Могут быть переопределены (но не обязательно)

## Результат выполнения
```
Breathing...
Woof! Woof!
Dog destructor
Animal destructor
Breathing...
Meow! Meow!
Cat destructor
Animal destructor
```

## Когда использовать абстрактные классы?
1. Когда нужно задать общий интерфейс для группы классов
2. Для реализации полиморфизма
3. Когда базовый класс не имеет смысла без конкретной реализации

## Отличие от интерфейсов
В C++ нет отдельного понятия "интерфейс", но принято считать:
- **Абстрактный класс** может содержать:
  - Чистые виртуальные функции
  - Виртуальные функции с реализацией
  - Обычные методы
  - Поля данных
- **Интерфейс** (в стиле C++) - это абстрактный класс, содержащий только чистые виртуальные функции


## Пример множественного наследования с абстрактными классами

```cpp
#include <iostream>
using namespace std;

// Абстрактный класс Flyer
class Flyer {
public:
    virtual void fly() = 0;
    virtual ~Flyer() { cout << "Flyer destructor\n"; }
};

// Абстрактный класс Swimmer
class Swimmer {
public:
    virtual void swim() = 0;
    virtual ~Swimmer() { cout << "Swimmer destructor\n"; }
};

// Конкретный класс, наследующий от двух абстрактных классов
class Duck : public Animal, public Flyer, public Swimmer {
public:
    void makeSound() override {
        cout << "Quack! Quack!\n";
    }

    void fly() override {
        cout << "Duck is flying\n";
    }

    void swim() override {
        cout << "Duck is swimming\n";
    }

    ~Duck() override {
        cout << "Duck destructor\n";
    }
};

int main() {
    Duck* duck = new Duck();

    Animal* animal = duck;
    Flyer* flyer = duck;
    Swimmer* swimmer = duck;

    animal->makeSound();
    flyer->fly();
    swimmer->swim();

    delete duck;

    return 0;
}
## Практическое задание
class Base {
public:
    virtual ~Base() { cout << "Base destructor\n"; }
};

class Derived : public Base {
public:
    ~Derived() override { cout << "Derived destructor\n"; }
};

int main() {
    Base* ptr = new Derived();
    delete ptr;  // Вызовет оба деструктора: Derived и Base
    return 0;
}


