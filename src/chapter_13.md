# Chapter 13

## Smart Pointers

### In C++ (Only) :

//Without Rule of Five

```
#include <iostream>
#include <memory>

struct Node {
    int data;
    float data2;
};


class Stack {
    std::unique_ptr<Node> top;
public:
    Stack() : top(std::make_unique<Node>(Node{0, 0.0f})) {}
    Stack(int x) : top(std::make_unique<Node>(Node{x, 0.0f})) {}
    Stack(float y) : top(std::make_unique<Node>(Node{0, y})) {}

    // copying now needs to be defined explicitly (or deleted)

    int print_top() {
        return top->data;
    }
};

int main() {
    Stack s1;
    Stack s2(1);
    Stack s3(1.0f);
 
    s2.print_top();
    s3.print_top();
   
    return 0;
}

```

//With Rule of Five

```

#include <iostream>
#include <memory>

struct Node {
    int data;
    float data2;
};


class Stack {
    std::unique_ptr<Node> top;
public:
    Stack() : top(std::make_unique<Node>(Node{0, 0.0f})) {}
    Stack(int x) : top(std::make_unique<Node>(Node{x, 0.0f})) {}
    Stack(float y) : top(std::make_unique<Node>(Node{0, y})) {}

    // Stacks shouldn’t be copyable → explicitly delete copying
    Stack(const Stack&) = delete;
    Stack& operator=(const Stack&) = delete;

    // but moving is fine:
    Stack(Stack&&) noexcept = default;
    Stack& operator=(Stack&&) noexcept = default;

    void print_top() {
        std::cout << top->data << std::endl;
        std::cout << top->data2 << std::endl;
    }

    // No Rule of Three needed; the destructor is automatic and safe.
};

int main() {
    Stack s1;
    Stack s2(1);
    Stack s3(1.0f);
 
    s2.print_top();
    s3.print_top();
   
    s1 = std::move(s2); 
    s1.print_top();

    return 0;
}

```