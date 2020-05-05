# References in C++


## 1 Concept of a Reference in C++

A **Reference** is a new name(alias) for another existing object or variable. And, as the name suggests, it is a _pointer_ internally. 

Reference is declared as:

`<type>& <name> = <referenced_variable>`

Like pointer symbol(`*`), reference symbol(`&`) can be placed anywhere between `<type>` and `<name>`.

{{< admonition type=note title="Note" >}}
1. Note that references **must be initialized** when they are created. References without objects to reference do not make sense.

2. **NULL reference** does not exist.

3. Cannot reference to constants. Like pointers cannot.(We need `const` keyword for this, sometimes called const references)

```cpp
int a = 10;
int& ref1 = a; // O

int& ref2 = NULL // X

const int b = 5;
int& ref3 = b; // X
int& ref4 = 3; // X
```
{{< /admonition >}}

---

## 2 Uses of References

### Simple Alias

References are pointers(of course they don't look like pointers) referring to the exact same object initialized with. References act like the referenced variables but only with different names.

```cpp
#include <iostream>

int main()
{
    int data = 10;
    int& rData = data;
    
    rData = 20; // same as data = 20;
    
    std::cout << "data: " << data << std::endl;
    std::cout << "rData: " << rData << std::endl;
    return 0;
}
```

The result is:

{{< admonition quote "Execution Result" >}}
data: 20

rData: 20
{{< /admonition >}}

### Function Parameters

It is very common that references are used as function parameters. 

When an argument is not passed as a reference, the copy of the argument is made, and the function deals with the copied value.

However, in this case, passing argument as a reference makes us modify the original value directly.

Also, if the size of the argument is big(like an array), it is efficient to use a reference as no copy of the big data happens.

```cpp
#include <iostream>

void increase(int& value)
{
    value++;
}

int main()
{
    int data = 10;
    increase(data);
    // increase(&data) Wrong!
    
    std::cout << "data: " << data << std::endl;
    return 0;
}
```

The result is:

{{< admonition quote "Execution Result" >}}
data: 11
{{< /admonition >}}


{{< admonition warning "Warning" >}}
Passing constant argument to `increase()` function like `increase(10)` will generate a compile error.

The reason is that reference can't refer to contant value.

To avoid this, we use const reference, which will be described below of this article.
{{< /admonition >}}

---

## 3 References vs Pointers

References can be thought of as pointers with more restrictive power. For example, the first example can be substitued using pointer like:

```cpp
#inlcude <iostream>

int main()
{
    int data = 10;
    int* pData = &data;
    *pData = 20;
}
```

Then, why do we need references? 

Actually, references are recommended in most cases because those restrictions prevent people from certain dangers of using pointers.

<div class="tenor-gif-embed" data-postid="4050585" data-share-method="host" data-width="100%" data-aspect-ratio="2.4057971014492754"><a href="https://tenor.com/view/spiderman-uncle-ben-power-responsiblity-movie-quotes-gif-4050585">With Great Power Comes Great Responsibility GIF</a> from <a href="https://tenor.com/search/spiderman-gifs">Spiderman GIFs</a></div><script type="text/javascript" async src="https://tenor.com/embed.js"></script>

Below are some of significant differences between References and Pointers.

### Allowing NULL values

Like said above, a reference can't have NULL value.

### Memory allocation

Pointer itself is allocated a memory address, and stores the memory address of the variable it points.

However, reference shares the same memory address with the original variable(Of course it should also have memory address in low level).

```cpp
#include <iostream>

int main()
{
    int data = 10;

    int* pData = &data;
    int& rData = data;

    std::cout << "addr of data: " << &data << std::endl;
    std::cout << "addr of pData: " << &pData << std::endl;
    std::cout << "addr of rData: " << &rData << std::endl;
    
    return 0;
}
```

The result is:

{{< admonition quote "Execution Result" >}}
addr of data: 010FFE80

addr of pData: 010FFE74

addr of rData: 010FFE80
{{< /admonition >}}

### Reassigning reference

Once a reference is initialized, this reference can't change.

```cpp
#include <iostream>

int main()
{
    int num1 = 10;
    int num2 = 20;
    int& rNum1 = num1;
    rNum1 = num2
}
```

This code changes the value of num1 from 10 to the value of num2(20). Once `rNum1` variable refers to `num1`, it doesn't change.

However, for the case of pointers, it is possible to change where it points.

```cpp
#include <iostream>

int main()
{
    int num1 = 10;
    int num2 = 20;
    
    int* pNum1 = &num1;
    pNum1 = &num2;
}
```

Pointer variable `pNum1` now points `num2`. Now we can see something is wrong. In some cases this can be critical. To prevent this, use reference or `const` keyword. After all, `int& rNum1` can be regarded as `int* const pNum1`.

---

## 4 Const References

It seems like reference covers all the pointer's problem, but reference can also bring some problems. 

One big problem is that **When a function's parameter is reference and we are trying to use that function, we can't be sure whether it is reference or not.**

```cpp
int data = 10;
someFunc(data);
```

In C language, we can be sure that the value of `data` is still 10 after `someFunc(data)` has been executed.

In C++, however, if the function is declared like `void someFunc(int&)`, there exists a possibility of modifying the value of `data`.

This is where const reference should come in. we can prevent modifying the value of original argument by declaring the function as `void someFunc(const int&)`.

Const reference also makes referencing to constant values possible.

```cpp
const int data = 10;
const int& rData = data; // O
const int& rNum = 20;    // O
```

This resolves another problem stated above. I mentioned `increase(10)` is not allowed in the function parameter example.

If the function parameter is reference, argument should be made as a non-constant variable. This is very annoying.

However, if we use const reference, we are then be able to pass contant argument to the function. Therefore, `increase(10)`is possible (Actually this function modifies the value of the argument, so it is not the right case to use it).

---

## 5 R-value References (C++11)

R-value reference is introduced in C++11. Reference we discussed so far is actually a L-value reference.

R-value reference uses two `&` symbols:

`<type>&& <name> = <r-value>`


### L-value and R-value

Simply we distinguish l-values and r-values by figuring out whether the value can be put left-side or right-side of assignment operator(`=`). However, I need slightly more concrete criteria. 

**L-value** is an object whose memory location is identifiable. Most variables are l-values.

**R-value** is any object that is not l-value, which means we cannot identify its memory location. Literals and temporary values obtained from arithmetic operations or returned by function calls are usually r-values.

{{< admonition warning "Misconceptions" >}}
1. Functions or operators always yields R-values

```cpp
int a = 5 + 3; // (5 + 3) is r-value

int b;
int& test() { return b; }
test() = 1; // return value of test() is l-value

int arr[10];
arr[2] = 100; // [] operator yields l-value
```

2. R-values are not modifiable

```cpp
TestClass().testFunc(); // TestClass() returns r-value, but testFunc() may modify TestClass object
```

{{< /admonition >}}

### Referencing temporary values

A result of arithmetic operations is temporary value. its scope is the line the operation was performed. After the execution of that line, we lose the temporary value.

R-value reference makes us keep track of the temporary value.

```cpp
#include <iostream>

int twice(int input)
{
    return input * 2;
}

int main()
{
    int&& num1 = 3 + 7;
    int&& num2 = twice(10);
}
```

This looks a bit wierd because we can just use simple `int num1 = 3 + 7` instead of that ugly `&&`.

I know this is not the real usefulness of r-value reference. 

I need to study further to figure out that. To be continued...

---

:point_right: **Thank you for reading my post !** :pray:

:point_right: Please leave a comment if you have any ideas to share ! :notes:

