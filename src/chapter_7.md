# Chapter 7

## Array

### In C :

```
#include <stdio.h>

int main() {
    int my_array[5];

    size_t len = sizeof(my_array) / sizeof(my_array[0]);
    for(size_t i = 0; i < len; i++) {
        my_array[i] = i;
    }

    printf("%d\n", my_array[2]);
    return 0;
}

```

### In C++ :

```
#include <iostream>
#include <array>

int main() {
    std::array<int, 5> my_array;

    for(size_t i = 0; i < my_array.size(); i++) {
        my_array[i] = i;
    }

    std::cout << my_array[2] << std::endl;
    return 0;
}

```

#### Range-based for loop

```
#include <iostream>
#include <array>

int main() {
    std::array<int, 5> my_array;

    size_t i = 0;
    for (auto& value : my_array) {
        value = i++;
    }

    std::cout << my_array[2] << std::endl;
    return 0;
}

```

#### Bounds-checked access

```
#include <iostream>
#include <array>

int main() {
    std::array<int, 5> my_array;

    size_t i = 0;
    for (auto& value : my_array) {
        value = i++;
    }

    std::cout << my_array.at(2) << std::endl;
    return 0;
}

```

### Vector

```
#include <iostream>
#include <vector>

int main() {
    std::vector<int> my_vector(5);  // vector with 5 elements

    for (size_t i = 0; i < my_vector.size(); i++) {
        my_vector[i] = i;
    }

    std::cout << my_vector[2] << std::endl;
    return 0;
}

```

#### Range-based for loop

```
#include <iostream>
#include <vector>

int main() {
    std::vector<int> my_vector(5);  // vector with 5 elements

    size_t i = 0;
    for (auto& value : my_vector) {
        value = i++;
    }

    std::cout << my_vector[2] << std::endl;
    return 0;
}

```

#### Bounds-checked access

```
#include <iostream>
#include <vector>

int main() {
    std::vector<int> my_vector(5);  // vector with 5 elements

    size_t i = 0;
    for (auto& value : my_vector) {
        value = i++;
    }

    std::cout << my_vector.at(2) << std::endl;
    return 0;
}

```

#### Push_back

```

#include <iostream>
#include <vector>

int main() {
std::vector<int> my_vector;

    for (int i = 0; i < 5; i++) {
        my_vector.push_back(i);
    }
    
    std::cout << my_vector.at(2) << std::endl;
    return 0;
}

```

## Span

```

#include <iostream>
#include <vector>
#include <span>

int main() {
    std::vector<int> data(5);

    std::span<int> my_span(data);  // view over the vector

    for (size_t i = 0; i < my_span.size(); i++) {
        my_span[i] = i;
    }

    std::cout << my_span[2] << std::endl;
    return 0;
}

```