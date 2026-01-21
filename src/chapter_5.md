# Chapter 5

## Printing strings

### In C :

--- 

#### Pointer 

```
#include <stdio.h>

int main(void)
{
        char* p = "Hello World!";
        printf("%s", p);
        printf("%d", sizeof(p)); // size of pointer
}
```

```
#include <stdio.h>

int main(void)
{
        char* p = "Hello World!";
        printf("%c", *p);
        printf("%d", sizeof(p)); // size of pointer
}

```

- "Hello World!" is stored in read-only static memory

- p is just a pointer to that memory

- Since the string literal is allocated in read-only memory, it is non-modifiable1. Any attempt to modify it will lead to
undefined behaviour.

p[0] = 'h';   // Undefined behavior 

-  To avoid this, add const to get a compile-time error. 

const char *p = "Hello,, world!";

---

#### Array

This is the only way to create a modifiable string.

```
#include <stdio.h>

int main(void)
{
        char p[] = "Hello World!";
        printf("%c", *p);
        p[0] = 'h';   // OK
        printf("%c", *p);

        printf("%s", p);
        printf("%d", sizeof(p)); // size of array
}

```

- Creates a string literal "Hello World!"

- Copies its characters into an array

- p is a real array, not a pointer

- The array typically lives on the stack (if local)
---


#### Copying Strings

##### Common Errors
```
#include <stdio.h>

int main(void) 
{
        char a[] = "abc";
        char b[8];
        b = a; /* compile error */
        printf("%s", b);
        return 0;
}

```
```
#include <stdio.h>
#include <string.h>

int main(void) {
    char a[] = "abc";
    char b[2];

    strcpy(b, a);   /* buffer overflow */
    printf("%s\n", b);

    return 0;
}

```
##### Solution

```
#include <stdio.h>
#include <string.h>

int main(void) {
    char a[] = "abc";
    char b[2];

    snprintf(b, sizeof b, "%s", a); /* truncates to fit */
    printf("%s\n", b);

    return 0;
}

```
---

#### Iterating over Strings


```
#include <stdio.h>

int main(void)
{
    char *string = "hello world";
    for (size_t i = 0; i < strlen(string); i++) {  // string[i] != '\0' works
        printf("%c\n", string[i]);
    }
    return 0;
}

```

---

### In C++ :
---

```
#include <iostream>
#include <string>

int main()
{
        std::string s = "Hello World."; // never call new or delete yourself.
        std::cout << s;
}

```
---
#### Copying, Iterating over Strings

```
#include <iostream>
#include <string>

int main()
{
        std::string a = "hello";
        std::string b = a;      // copy
        a[0] = 'H';           // OK
        a += " world";        // automatic reallocation

        for (std::size_t i = 0; i < a.size(); ++i) {
                std::cout << a[i] << '\n';
        }
}


```
---

