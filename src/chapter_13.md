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
#include <utility>

struct Node {
  int data;
  float data2;
  std::unique_ptr<Node> next;
};

class Stack {
  std::unique_ptr<Node> top;

public:
  Stack() = default;

  Stack(const Stack&) = delete;
  Stack& operator=(const Stack&) = delete;

  Stack(Stack&&) noexcept = default;
  Stack& operator=(Stack&&) noexcept = default;

  void push(int x, float y = 0.0f) {
    auto n = std::make_unique<Node>(Node{x, y, std::move(top)});
    top = std::move(n);
  }

  void pop() {
    if (top) top = std::move(top->next);
  }

  void print_top() const {
    if (!top) { std::cout << "(empty)\n"; return; }
    std::cout << top->data << "\n" << top->data2 << "\n";
  }
};

int main() {
  Stack s;
  s.push(1);
  s.push(2, 3.14f);
  s.print_top();
  s.pop();
  s.print_top();
}

```