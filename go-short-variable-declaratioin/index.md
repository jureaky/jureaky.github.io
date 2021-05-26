# [Go] Short Variable Declaration


## 1. What is Short Variable Declaration?

In most programming language, variable is initialized using assignment operator `=`.

Go is not immune from this typical convention. It, however, supports a short-hand version of declaring and initializing variables at the same time.

With the help of what is called "Short Variable Declaration Operator(`:=`)", the code looks much nicer, but this operator adds up complexity which leads us to make some mistakes.

Let's get into more details of short variable declaration operator(`:=`).

## 2. Basic Syntax

- regular variable declaration syntax using `var` keyword

```
var <variable_name> <variable_type>

// Or you can initialize at the same time
var <variable_name> <variable_type> = <value>

```

- variable declaration using Short variable declaration operator

```
<variable_name> := <initial_value>
```

You can declare a variable or many without specifying its type. 

## 3. Rules to Follow

### 1) Only used for Local Variables

You cannot use short variable declaration to declare global variable, which means you can only use it inside a function.

Global variables at package level can only be declared using `var` keyword.

```go
package main

globalVar := 25 // Wrong!! syntax error: non-declaration statement outside function body

func main() {
    localVar := 3 // OK
}
```

### 2) Can be used for multiple variable declarations

Go supports multiple variable declarations in one line, which also works for short variable declaration.

```go
package main

func main() {
    var1, var2 := 5, "hello"
}
```

### 3) Cannot use twice for the same variable

Using `:=` operator means declaring a new variable like using `var` keyword.

Like you cannot use `var` to the variable you declared before, `:=` cannot be used twice. 

If you want to assign a new value to it, just use `=`.

```go
package main

func main() {
    var1 := 5

    var1 = 7 // OK
    var1 := 10 // Wrong!! no new variables on left side of :=
}
```

{{< admonition warning "Warnings" >}}
**1. Using short variable declaration operator without type doesn't mean that the variable is type-free.**

Go compiler infers the variable's type when initializing. Therefore, if you want to assign a new value which has a different type to the variable, you'll encounter an error.

```go
package main

func main() {
    var1 := true
    var1 = "hello" // Wrong!! cannot use "String" (type untyped string) as type bool in assignment
}
```

**2. You can use it twice in multiple variable declarations if you introduce new variables.**

The error message above written in the inline comment(_no new variables on left side of :=_) makes us raise a question. What if there are new variables on the left?

Well, you can re-declare the variable with new variables in multi-variable assignment. This property is used frequently when dealing with errors.

```go
func exampleFunc() {
    var1, err := someFunc1()

    var2, err := someFunc2() // You don't need to write as err1, err2, ...
}
```

But still, you cannot re-declare a variable with different type.
{{< /admonition >}}

### 4) Can be used in statements like if, for ...

You can use short variable declaration to use a certain variable only in the scope of if statements for example.

```go
if i := 10; i > 0 {
    fmt.Println("i is greater than 0")
}
```


---

:point_right: **Thank you for reading my post !** :pray:

:point_right: Please leave a comment if you have any ideas to share ! :notes:

