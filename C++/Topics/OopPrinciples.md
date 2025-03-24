
# 4 принципа ООП

1. **Инкапсуляция**  
   Сокрытие данных (используйте `private`/`protected`).

```cpp
class BankAccount {
private:
    double balance; // Скрыто
public:
    void deposit(double amount) { // Контролируемый доступ
        if (amount > 0) balance += amount;
    }
};
```
2. **Наследование**  
   Создание иерархии классов.
```cpp
class Animal {
public:
    virtual void speak() { std::cout << "Some sound\n"; }
};
class Dog : public Animal {
public:
    void speak() override { std::cout << "Woof!\n"; }
};
```
3. **Полиморфизм**  
   Возможность объектов вести себя по-разному (`virtual`).
```cpp
Animal* a = new Dog();
a->speak(); // Woof! (вызов переопределенного метода)
```
4. **Абстракция**  
   Упрощение сложных систем.
```cpp
class AbstractShape {
public:
    virtual double area() = 0; // Чисто виртуальный метод
};
class Circle : public AbstractShape {
    double radius;
public:
    double area() override { return 3.14 * radius * radius; }
};
```