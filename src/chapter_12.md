# Chapter 12

## Constructor & Destructor

### In C++ :

#### Access control by Encapsulation

```
#include <iostream>

struct Node {
    int data;
};

class Stack {
private:
    Node* top;

public:
    Stack() {
        top = new Node{0};
    }
    Stack(int x) {
        top = new Node{x};
    }
    ~Stack() {
        delete top;
    }

    int print_top() {
        return top->data;
    }
};

int main() {
    Stack s1;
    Stack s2(1);

    std::cout << s2.print_top() << std::endl;

    return 0;
}

```

#### Polymorphism

```
#include <iostream>

struct Node {
    int data;
    float data2;
};

class Stack {
private:
    Node* top;

public:
    Stack() {
        top = new Node{ 0, 0.0f };
    }

    Stack(int x) {
        top = new Node{ x, 0.0f };
    }

    Stack(float y) {
        top = new Node{ 0, y };
    }

#if 1    // Rule of Three

    Stack(const Stack& other) {
        top = new Node(*other.top);
    }

    Stack& operator=(const Stack& other) {
        if (this != &other) {
            delete top;
            top = new Node(*other.top);
        }
        return *this;
    }

    ~Stack() {
        delete top;
    }
#endif
    void print_top() {
        std::cout << top->data << std::endl;
        std::cout << top->data2 << std::endl;
    }
};

int main() {
    Stack s1;
    Stack s2(1);
    Stack s3(1.0f);
 
    s2.print_top();
    s3.print_top();
   
    s1 = s2;
    s1.print_top();

    s1 = s3;
    s1.print_top();

    return 0;
}

```


#### Inheritance


```


#include <iostream>

class human {
public:
    int height;
    int weight;
public: 
    human(int h, int w) {
        height = h;
        weight = w;
    }
    int get_height() const {
        return height;
    }
    int get_weight() const {
        return weight;
    }
};

class adult : public human { // inherit from human class
public:
    std::string occupation;
public:
    adult(int h, int w) : human(h, w) {
       occupation = "doctor"; 
    } // call the base class constructor from this constructor

    std::string get_occupation() const {
        return occupation;
    }   
};

int main() {
    adult obj(180, 220);  // object of derived class
    std::cout << obj.get_occupation() << std::endl;

    obj.occupation = "lawyer";
    
    std::cout << obj.get_height() << std::endl;
    std::cout << obj.get_weight() << std::endl;
    std::cout << obj.get_occupation() << std::endl;
    
    return 0;
}


```

##### Virtual Functions


```

#include <iostream>

class human {
public:
    int height;
    int weight;
public: 
    human(int h, int w) {
        height = h;
        weight = w;
    }
    int get_height() const {
        return height;
    }
    int get_weight() const {
        return weight;
    }
    virtual void print_all() = 0; // ()=0 creates a _pure_ virtual function that must be overridden.
    //Alternatively you can just write a default function as a virtual function with no requirement to be overwritten.
};

class adult : public human { // inherit from human class
public:
    std::string occupation;
public:
    adult(int h, int w) : human(h, w) {
       occupation = "doctor"; 
    } // call the base class constructor from this constructor

    std::string get_occupation() const {
        return occupation;
    }   

    void print_all() { // virtual function overridden in derived class
        std::cout << height << std::endl;
        std::cout << weight << std::endl;
        std::cout << occupation << std::endl;
    }
};

int main() {
    adult obj(180, 220); // object of derived class
    std::cout << obj.get_occupation() << std::endl;
    
    obj.occupation = "lawyer";
    
    obj.print_all();
    return 0;
}

```

