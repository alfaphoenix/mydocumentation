

## **1. Базовый синтаксис**

#### **Hello World**

```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "Hello, World!" << endl;
    return 0;
}

```

### **Переменные и типы**

```cpp
int a = 5;               // Целое число
double b = 3.14;         // Дробное число
char c = 'A';            // Символ
string s = "Hello";      // Строка
bool flag = true;        // Логическое значение

```

---

## **2. Условные операторы**

```cpp
if (a > 0) {
    cout << "Положительное";
} else if (a < 0) {
    cout << "Отрицательное";
} else {
    cout << "Ноль";
}

// Тернарный оператор
string result = (a > 0) ? "Да" : "Нет";

```

---

## **3. Циклы**

### **`for`**

```cpp
for (int i = 0; i < 5; i++) {
    cout << i << " ";
}
// Вывод: 0 1 2 3 4

```
### **`while`**

```cpp
int i = 0;
while (i < 5) {
    cout << i << " ";
    i++;
}

```

---

## **4. Массивы и векторы**

### **Статический массив**

```c++
int arr[3] = {1, 2, 3};
```

### **Динамический массив (`vector`)**

```c++
#include <vector>
vector<int> vec = {1, 2, 3};
vec.push_back(4); // Добавить элемент

```

---

## **5. Функции**

```c++
int sum(int x, int y) {
    return x + y;
}

int result = sum(2, 3); // result = 5

```

---

## **6. Указатели и ссылки**

```c++
int x = 10;
int* ptr = &x;    // Указатель на x
int& ref = x;     // Ссылка на x

cout << *ptr;     // 10 (разьименование)
cout << ref;      // 10
```
---

## **7. Класс и объект**

```c++
class Person {
public:
    string name;
    void greet() {
        cout << "Привет, " << name << "!";
    }
};

Person p;
p.name = "Абдугафор";
p.greet(); // Вывод: Привет, Абдугафор!
```

---

### **8. Работа с файлами**

```c++
#include <fstream>

ofstream file("test.txt");
file << "Привет, файл!";
file.close();

```

---

### **9. STL (Standard Template Library)**

### **`vector`**

```c++
vector<int> nums = {1, 2, 3};
nums.push_back(4);

```

### **`map` (словарь)**

```c++
map<string, int> ages;
ages["Абдугафор"] = 25;
cout << ages["Абдугафор"]; // 25
```
---

### **10. Обработка ошибок (`try-catch`)**

```c++
try {
    int a = 10 / 0; // Ошибка
} catch (...) {
    cout << "Произошла ошибка!";
}

```

---

### **11. Полезные ссылки**

- [LearnCpp](https://www.learncpp.com/) — учебник для новичков.
- [cppreference](https://en.cppreference.com/) — справочник по C++.
- [Qt Documentation](https://doc.qt.io/) — если понадобится для GUI.

---
