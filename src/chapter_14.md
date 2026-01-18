# Chapter 13

## STL libraries

### In C++ (Only) :


#### std::optional

 std::optional<T> answers: “Do I have  zero or one T”

```
#include <iostream>
#include <memory>
#include <optional>

struct Node {
    int data{};
    float data2{};
};

class Stack {
    std::unique_ptr<Node> top;

public:
    Stack() : top(std::make_unique<Node>(Node{0, 0.0f})) {}
    Stack(int x) : top(std::make_unique<Node>(Node{x, 0.0f})) {}
    Stack(float y) : top(std::make_unique<Node>(Node{0, y})) {}

    Stack(const Stack&) = delete;
    Stack& operator=(const Stack&) = delete;
    Stack(Stack&&) noexcept = default;
    Stack& operator=(Stack&&) noexcept = default;

    void print_top() const {
        if (!top) {
            std::cout << "(empty stack)\n";
            return;
        }
        std::cout << top->data << '\n';
        std::cout << top->data2 << '\n';
    }

    // ValueOrNone: may or may not return a Node
    std::optional<Node> pop_value() {
        if (!top) {
            return std::nullopt;
        }

        Node value = *top; // copy out
        top.reset();       // stack becomes empty
        return value;
    }
};

int main() {
    Stack s1;
    Stack s2(1);

    s1 = std::move(s2); // s2 is now empty

    auto result = s2.pop_value();
    if (!result) {
        std::cout << "pop failed: stack empty\n";
    } else {
        std::cout << "popped:\n";
        std::cout << result->data << "\n";
        std::cout << result->data2 << "\n";
    }
}



```

#### std::expected

* std::expected<T, E> answers: “Did it succeed with T or E (error)?”

```
#include <expected>   // C++23
#include <iostream>
#include <memory>

struct Node {
    int data{};
    float data2{};
};

enum class StackError {
    Empty
};

class Stack {
    std::unique_ptr<Node> top;

public:
    Stack() : top(std::make_unique<Node>(Node{0, 0.0f})) {}
    Stack(int x) : top(std::make_unique<Node>(Node{x, 0.0f})) {}
    Stack(float y) : top(std::make_unique<Node>(Node{0, y})) {}

    Stack(const Stack&) = delete;
    Stack& operator=(const Stack&) = delete;
    Stack(Stack&&) noexcept = default;
    Stack& operator=(Stack&&) noexcept = default;

    void print_top() const {
        if (!top) {
            std::cout << "(empty)\n";
            return;
        }
        std::cout << top->data << "\n" << top->data2 << "\n";
    }

    // ValueOrError: either returns Node (success) OR StackError (failure)
    std::expected<Node, StackError> pop_value() {
        if (!top) return std::unexpected(StackError::Empty);
        Node v = *top;          // copy out
        top.reset();            // now empty (for demo; a real stack would move to next)
        return v;
    }
};

int main() {
    Stack s2(1);
    Stack s1;

    s1 = std::move(s2); // s2 becomes empty (moved-from)

    // This is safe and explicit:
    auto r = s2.pop_value();
    if (!r.has_value()) {
        std::cout << "pop failed: empty\n";
    } else {
        std::cout << "popped: " << r->data << ", " << r->data2 << "\n";
    }
}


```
#### std::variant

*   std::variant<A,B,C> answers: “exactly one of A, B, or C”
*   fancy version of a tagged union


```

#include <iostream>
#include <memory>
#include <variant>

struct Node {
    std::variant<int, float> value;
};

class Stack {
    std::unique_ptr<Node> top;

public:
    Stack() = default;

    Stack(int x)
        : top(std::make_unique<Node>(Node{ x })) {}

    Stack(float y)
        : top(std::make_unique<Node>(Node{ y })) {}

    Stack(const Stack&) = delete;
    Stack& operator=(const Stack&) = delete;

    Stack(Stack&&) noexcept = default;
    Stack& operator=(Stack&&) noexcept = default;

    void print_top() const {
        if (!top) {
            std::cout << "(empty)\n";
            return;
        }

        std::visit([](const auto& v) {
            std::cout << v << "\n";
        }, top->value);
    }
};

int main() {
    Stack s1(10);
    Stack s2(3.14f);

    s1.print_top(); // 10
    s2.print_top(); // 3.14

    s1 = std::move(s2);
    s1.print_top(); // 3.14
}


```