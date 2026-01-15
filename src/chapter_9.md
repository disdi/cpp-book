# Chapter 9

## Templates (Only in C++)

### Function Templates :

```
#include <iostream>

template <class T>
T largest(T n1, T n2) {
    return (n1 > n2) ? n1 : n2;
}

int main() {
    int x = 2;
    int y = 3;
    std::cout << largest(x, y) << std::endl;
    return 0;
}
```

### Class Templates :

```
template <typename T>
class Stack {
    struct Node {
        T data;
    };
};

int main() {
    Stack<std::string> a;
 }
 ```