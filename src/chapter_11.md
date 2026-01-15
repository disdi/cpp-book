# Chapter 11

## Struct

### In C :

```
#include <stdio.h>

struct Node {
    int data;
};

int main() {
    struct Node object1;
    object1.x = 1;
    printf("%d\n", object1.x);
    return 0;
}

```

### In C++ :

```
#include <iostream>

struct Node {
    int data;
};

int main() {
    struct Node object1;
    object1.x = 1;
    std::cout << object1.x << std::endl;
    return 0;
}

```

## Classes (In C++ Only) :


```

class Stack {
    public:    
        int data;
};


int main() {
    Stack n;
    n.data = 1;
    std::cout << n.data << std::endl;
    return 0;
}


```
