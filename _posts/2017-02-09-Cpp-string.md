---
layout: post
title: "Cpp string, char [], char *, and null character"
description: ""
categories: 
tags: []
---


The length of a C string is determined by the terminating __null-character__: A C string is as long as the number of characters between the beginning of the string and the terminating null character (without including the terminating null character itself).

`sizeof()` returns size in bytes of the object representation of type.

`strlen()` returns the length of the given byte string, that is, the number of characters in a character array whose first element is pointed to by str up to and not including the first __null character__. _The behavior is undefined if there is no null character in the character array pointed to by str_.

`length()` and `size()` returns the number of `char` elements in `string`.


```cpp
// Example program 1
#include <iostream>
#include <string>
#include <string.h>
using namespace std;

int main() {
  char x[3] = {'a', 'b', 'c'};
  cout << x << endl;
  return 0;
}
```

If we don't declare more variables after `char x[3]` on the stack, we will visit beyond memory boundary when we call `cout`. This illegal memory access leads to output like this:

```shell
abc��
```

We have another example.

```cpp
// Example program 2
#include <iostream>
#include <string>
#include <string.h>
using namespace std;

int main() {
  char x[3] = {'a', 'b', 'c'};
  char new_x[] = {'a', 'b', 'c', '\0'};
  string y = (string) x;
  const char* z = y.c_str();

  cout << "x, new_x, y, z: " << x << " "
       << new_x << " " << y << " " << z << endl;
  cout << "strlen(x): " << strlen(x) << endl;
  cout << "strlen(new_x): " << strlen(new_x) << endl;
  cout << "sizeof(new_x): " << sizeof(new_x) << endl;
  cout << "y.size() y.length(): " << y.size() << " " << y.length() << endl;
  cout << "sizeof(y): " << sizeof(y) << endl;
  cout << "sizeof(z): " << sizeof(z) << endl;
  cout << "sizeof(x[0]): " << sizeof(x[0]) << endl
  cout << "strlen(z): " << strlen(z) << endl;
  
  cout << "addr of char [] " << &x << endl;
  cout << "addr of x[0] " << (void*) &x[0] << endl;
  cout << "addr of x[0] " << (char*) &x[0] << endl;

  return 0;
}
```

Output:

```shell
x, new_x, y, z: abc abc abc abc
strlen(x): 3
strlen(new_x): 3
sizeof(new_x): 4
y.size() y.length(): 3 3
sizeof(y): 8 		// the size of pointer in byte
sizeof(z): 8 		// the size of pointer in byte
sizeof(x[0]): 1 	// the size of char in byte
strlen(z): 3
addr of char [] 0x7fffbf9db420
addr of x[0] 0x7fffbf9db420
addr of x[0] abc
```