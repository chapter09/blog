---
layout: post
title: "Cpp string, char [], char *, and null character"
description: ""
categories: 
tags: []
---


The length of a C string is determined by the terminating null-character: A C string is as long as the number of characters between the beginning of the string and the terminating null character (without including the terminating null character itself).

`sizeof()`


`strlen()`


`length()` and `size()` 


```cpp
 // Example program 1
 #include <iostream>
 #include <string>
 #include <string.h>
 using namespace std;

 int main()
 {
   char x[3] = {'a', 'b', 'c'};
   cout << x << endl;
   return 0;
 }
```

If we don't clare 

Output:

```shell
abc��
```

If I 

```cpp
// Example program 2
#include <iostream>
#include <string>
#include <string.h>
using namespace std;

int main()
{
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
  cout << "strlen(z): " << strlen(z) << endl;

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
sizeof(y): 8
sizeof(z): 8
strlen(z): 3
```