## 1. Expressões e comandos

Agora que vimos como construir um programa completo, vamos aprender o universo do que podemos fazer dentro de uma função C, executando através de diferentes tipos de comandos.

### 2.1. Operadores

Um ** operador ** é algo que podemos usar em expressões aritméticas, como um sinal de mais '+' ou um teste de igualdade '\ =='. Os operadores da linguagem C parecerão familiares, pois o designer da Python, Guido van Rossum, utilizou como referência esses operadores; mas existem algumas diferenças significativas.

 
> **C operator precedence**
> 
> `++ --` (postfix)  
> `+ - !` (unário)  
> `* / %`  
> `+ -` (binário)  
> `< > <= >=`  
> `== !=`  
> `&&`  
> `||`  
> `= += -= *= /= %=`

> **Python operator precedence**
> `**`  
> `+ -` (unary)  
> `* / % //`  
> `+ -` (binary)  
> `< > <= >= == !=`  
> `**not**`  
> `**and**`  
> `**or**`


Algumas distinções importantes: 

* C não possui um operador de exponenciação como o operador `**` do Python. Para exponenciação em C, você deverá usar uma função de biblioteca denominada `pow ()`. Por exemplo, `pow (1.1, 2.0)` calcula 1.1². 

* C usa símbolos em vez de palavras para as operações booleanas AND (`&&`), OR (`||`) e NOT (`!`). 
  
* O nível de precedência de NOT (o operador `!`) É muito alto em C. Isso quase nunca é desejado, então você acaba precisando de parênteses na maioria das vezes quer for usar o operador `!`. 
  
* C define a atribuição como um operador, enquanto o Python define a atribuição como um comando. O valor do operador de atribuição é o valor atribuído. Uma consequência do design de C é que uma atribuição pode legalmente ser parte de outra declaração.

```c
while ((a = getchar()) != EOF)
```    
    
Aqui, nós atribuímos o valor retornado por `getchar ()` à variável `a`, e então testamos se o valor atribuído a` a` corresponde à constante `EOF`, que é usada para decidir se deve repetir o loop novamente . Muitas pessoas afirmam que esse estilo de programação é extraordinariamente ruim; outros acham muito conveniente evitar. O Python, é claro, foi projetado de modo que uma atribuição deva ocorrer como sua própria instrução separada, portanto, as atribuições dentro de uma condição de um comando `while` é ilegal no Python.

    
*   C's operators `++` and `--` are for incrementing and decrementing a variable. Thus, the statement “`i++`” is a shorter form of the statement “`i = i + 1`” (or `i += 1`”).
    
*   C's division operator `/` does integer division if both sides of the operator have an `**int**` type; that is, any remainder is ignored with such a division. Thus, in C the expression “`13 / 5`” evaluates to 2, while `13 / 5.0`” is 2.6: The first has integer values on each side, while the second has a floating-point number on the right.
    
    By contrast, with newer versions of Python (3.0 and later), the single-slash operator always does floating-point division. With older Python versions, the single-slash operator worked as with C, but this would often lead to bugs — in part because the type associated with a variable is not fixed as it is in a C program.
    

### 2.2. Basic types

C's list of types is quite constrained.

`**int**`

for an integer

`**char**`

for a single character

`**float**`

for a single-precision floating-point number

`**double**`

for a double-precision floating-point number

The `**int**` type is straightforward. You can also create other types of integers, using the type names `**long**` and `**short**`. A `**long**` reserves at least as many bits as an `**int**`, while a `**short**` reserves fewer bits than an `**int**` (or the same number). The language does not guarantee the number of bits for each, but most current compilers use 32 bits for an `**int**`, which allows numbers up to 2.15×109. This is sufficient for most purposes, and many compilers also use 32 bits for a `**long**` anyway, so people typically use `**int**` in their programs.

The `**char**` type is also straightforward: It represents a single character, like a letter or punctuation symbol. You can represent an individual character in a program by enclosing the character in single quotation marks: “`digit0 = '0';`” would place the zero digit character into the `**char**` variable `digit0`.

Of the two floating-point types, `**float**` and `**double**`, most programmers today stick almost exclusively to `**double**`. These types are for numbers that could have a decimal point in them, like 2.5, or for numbers larger than an `**int**` can hold, like 6.02×1023. The two types differ in that a `**float**` typically takes only 32 bits of storage while a `**double**` typically takes 64 bits. The 32-bit storage technique allows a narrower range of numbers (−3.4×1038 to 3.4×1038) and — more problematic — about 7 significant digits. A `**float**` could not store a number like 281,421,906 (the U.S.'s population in 2000, according to the census), because it requires nine significant digits; it would have to store an approximation instead, like 281,421,920. By contrast, the 64-bit storage technique allows a wider range of numbers (−1.7×10308 to 1.7×10308) and roughly 15 significant digits. This is more adequate for general purposes, and the extra 32 bits of storage is rarely worth saving, so `**double**` is almost always preferred.

C does _not_ have a Boolean type for representing true/false values. This has major implications for a statement like `**if**`, where you need a test to determine whether to execute the body. C's approach is to treat the integer 0 as _false_ and all other integer values as _true_. The following would be a legal C program.

```c
int main() {
    int i = 5;
    if (i) {
        printf("in if\n");
    } else {
        printf("in else\n");
    }
    return 0;
} 
```

This program would compile, and it would print “in if” when executed, since the value of the `**if**` expression (`i`) turns out to be 5, which isn't 0 and thus the `**if**` condition succeeds.

C's operators that look like they should compute Boolean values (like `==`, `&&`, and `||`) actually compute `**int**` values instead. In particular, they compute 1 to represent _true_ and 0 to represent _false_. This means that you could legitimately type the following to count how many of `a`, `b`, and `c` are positive.

```c
pos = (a > 0) + (b > 0) + (c > 0);
```
This quirk — that C regards all non-zero integers as true — is generally regarded as a mistake. C introduced it machine languages rarely have direct support for Boolean values, but typically machine languages expect you to accomplish such tests by comparing to zero. But compilers have improved beyond the point they were when C was invented, and they can now easily translate Boolean comparisons to efficient machine code. What's more, this design of C leads to confusing programs, so most expert C programmers eschew using the shortcut, preferring instead to explicitly compare to zero as a matter of good programming style. But such avoidance doesn't fix the fact that this language quirk often leads to program errors. Most newer languages choose to have a special type associated with Boolean values. (Python has its own Boolean type, but it also treats 0 as false for `**if**` statements.)

### 2.3. Braces

Several statements, like the `**if**` statement, include a body that can hold multiple statements. Typically the body is surrounded by braces ('{' and '}') to indicate its extent. But when the body holds only a single statement, the braces are optional. Thus, we could find the maximum of two numbers with no braces, since the body of both the `**if**` and the `**else**` contain only a single statement.

```c
if (first > second)
    max = first;
else
    max = second; 
```

(We could also include braces on just one of the two bodies, as long as that body contains just one statement.)

C programmers use this quite often when they want one of several `**if**` tests to be executed. An example of this is with the quadratic formula code above. We could compute the number of solutions as follows:

```c
disc = b * b - 4 * a * c;
if (disc < 0) {
    num_sol = 0;
} else {
    if (disc == 0) {
        num_sol = 1;
    } else {
        num_sol = 2;
    }
} 
```

Notice that the `**else**` clause here holds just one statement (an `**if**`…`**else**` statement), so we can omit the braces around it. We might then write it thus:

```c
disc = b * b - 4 * a * c;
if (disc < 0) {
    num_sol = 0;
} else
    if (disc == 0) {
        num_sol = 1;
    } else {
        num_sol = 2;
    } 
```

But this situation arises often enough that C programmers follow a special rule for indenting in this case — a rule that allows all cases to be written at the same level of indentation.

```c
disc = b * b - 4 * a * c;
if (disc < 0) {
    num_sol = 0;
} else if (disc == 0) {
    num_sol = 1;
} else {
    num_sol = 2;
} 
```
Because this is feasible using C's bracing rules, C does not include the concept of an `elif` clause that you find in Python. You can just string together as many “`**else if**`” combinations as you want.

Other than this particular situation, I recommend that you include the braces anyway. As you continue working on a program, you often find that you want to add additional statements into the body of an `**if**`, and having the braces there already saves you the bother of adding them later on. And it makes it easier to keep track of braces, since each indentation level requires a closing right brace.

### 2.4. Statements

We've seen four types of statements, three of which correlate closely with Python.

1\. `**int** x;`

We already discussed variable declarations in [Section 1.2](#s1.2). They have no parallel in Python.

2\. `x = y + z;` or `printf("%d", x);`

You can have an expression as a statement. Technically, the expression could be “`x + 3;`”, but such a statement has no point: We ask the computer to add `x` and 3, but we don't ask anything to happen to it. Almost always, the expressions have one of two forms: One form is an operator that changes a variable's value, like the assignment operator (“`x = 3;`”), the addition assignment operator `+=`, or the the increment operator `++`. The other form of expression that you see as a statement is a function call, like a statement that simply calls the `printf()` function.

3\. `**if** (x < 0) { printf("negative"); }`

You can have an `**if**` statement, which works very similarly to Python's `**if**` statement. The only major difference is the syntax: In C, an `**if**` statement's condition must be enclosed in parentheses, there is no colon following the condition, and the body has a set of braces enclosing it.

As we've already seen, C does not have an `elif` clause as in Python; instead, C programmers use the optional-brace rule and write “`**else if**`”.

4\. `**return** 0;`

You can have a `**return**` statement to exit a function with a given return value. Or for a function with no return value (and a `**void**` return type), you would write simply “`**return**;`”.

There are three more statement types that correlate closely to equivalents from Python.

5\. `**while** (i >= 0) { i--; }`

The `**while**` statement works identically to Python's, although the syntax is different in the same way that the `**if**` syntax is different.

```c
while (i >= 0) {
    printf("%d\n", i);
    i--;
} 
```

Again, the test expression requires a set of parentheses around it, there is no colon, and we use braces to surround the loop's body.

6\. `**break**;`

As in Python, the `**break**` statement immediately exits the innermost loop in which it is found. Of course, the statement has a semicolon following it.

1. `**continue**;`

Also as in Python, the `**continue**` statement skips to the bottom of the innermost loop in which it is found and tests whether to repeat the loop again. It has a semicolon following it, too.

And there are two types of statements that have no good parallel in Python.

8. `for (i = 0; i < 10; i++) {…`

While Python also has a `**for**` statement, its purpose and its syntax bear scant similarity to C's `**for**` statement. In C, the `**for**` keyword is followed by a set of parentheses containing three parts separated by semicolons.

```c
for (init; test; update)
```

The intent of C's `**for**` loop is to enable stepping a variable through a series of numbers, like counting from 0 to 9. The part before the first semicolon (_init_) is performed as soon as the `**for**` statement is reached; it is for initializing the variable that will count. The part between the two semicolons (_test_) is evaluated before each iteration to determine whether the iteration should be repeated. And the part following the final semicolon (_update_) is evaluated at the end of each iteration to update the counting variable for the following iteration.

In practice, `**for**` loops are used most often for counting out n iterations. The standard idiom for this is the following.

```c
for (i = 0; i < n; i++) {
    comandos
}
```
Here we have a counter variable `i` whose value starts at 0. With each iteration, we test whether `i` has reached n or not; and if it hasn't, then we execute the `**for**` statement's body and then perform the `i++` update so that `i` goes to the following integer. The result is that the body is executed for each value of `i` from 0 up to n − 1.

But you can use a `**for**` loop for other purposes, too. In the following example, we display the powers of 2 up to 512. Notice how the update portion of the `**for**` statement has changed to “`p *= 2`”.

```c
for (p = 1; p <= 512; p *= 2) {
    printf("%d\n", p);
} 
```

9\. `**switch** (grade) { **case** 'A':`…

The `**switch**` statement has no equivalent in Python, but it is essentially equivalent to a particular form of an `**if**`…`**elif**`…`**elif**`…`**else**` statement where each of the tests are for different values of the same variable.

A `**switch**` statement is useful when you have several possible blocks of code, one of which should be executed based on the value of a particular expression. Here is a classic instance of the `**switch**` statement:

```c
switch (letter_grade) {
case 'A':
    gpa += 4;
    credits += 1;
    break;
case 'B':
    gpa += 3;
    credits += 1;
    break;
case 'C':
    gpa += 2;
    credits += 1;
    break;
case 'D':
    gpa += 1;
    credits += 1;
    break;
case 'W':
    break;
default:
    credits += 1;
} 
```

Inside the parentheses following the `**switch**` keyword, we have an expression, whose value must be a character or integer. The computer evaluates this expression and goes down to one of the `**case**` keywords based on its value. If the value is the character _A_, then the first block is executed (`gpa += 4; credits += 1;`); if it is _B_, then the second block is executed; if it is none of the characters (like an _F_), the block following the `**default**` keyword is executed.

The `**break**` statement at the end of each block is a crucial detail: If the `**break**` statement is omitted, then the computer continues into the following block. In our above example, if we omitted all `**break**` statements, then a grade of _A_ would lead the computer to execute not only the _A_ case but also the _B_, _C_, _D_, _W_, and `**default**` cases. The result would be that `gpa` would increase by 4 + 3 + 2 + 1 = 10, while `credits` would increase by 5. Occasionally you actually want the computer to continue to the next case (called “fall-through”), and so you omit a `**break**` statement; but in practice you almost always want `**break**` statement at the end of each case.

There is one important exception where fall-through is quite common: Sometimes you want the same code to apply to two different values. For instance, if we wanted the nothing to happen whether the grade is _P_ or _W_, then we could include “`**case** 'P':`” just before “`**case** 'W'`”, with no intervening code.

### 2.5. Arrays

Python supports many types that combine the basic atomic types into a group: tuples, lists, strings, dictionaries, sets.

C's support is much more rudimentary: The _only_ composite type is the **array**, which is similar to Python's list except that an array in C cannot grow or shrink — its size is fixed at the time of creation. You can declare and access an array as follows.

```c

```

In this example, we create an array containing 50 slots for `**double**` values. The slots are indexed 0 through 49.

C does not have an support for accessing the length of an array once it is created; that is, there is nothing analogous to Python's `len(pops)` or Java's `pops.length`.

An important point with respect to arrays: What happens if you access an array index outside the array, like accessing `pops[50]` or `pops[-100]`? With Python or Java, this will terminate the program with a friendly message pointing to the line at fault and saying that the program went beyond the array bounds. C is not nearly so friendly. When you access beyond an array bounds, it blindly does it.

This can lead to peculiar behavior. For example, consider the following program.

```c

```

Some systems (including a Linux installation I've encountered) would place `i` in memory just after the `vals` array; thus, when `i` reaches 5 and the computer executes “`vals[i] = 0`”, it in fact resets the memory corresponding to `i` to 0. As a result, the `**for**` loop has reset, and the program goes through the loop again, and again, repeatedly. The program never reaches the `printf` function call, and the program never terminates.

In more complicated programs, the lack of array-bounds checking can lead to very difficult bugs, where a variable's value changes mysteriously somewhere within hundreds of functions, and you as the programmer must determine where an array index was accessed out of bounds. This is the type of bug that takes a lot of time to uncover and repair.

That's why you should consider the error messages provided by Python (or Java) as extraordinarily friendly: Not only does it tell you the cause of a problem, it even tells you exactly which line of the program was at fault. This saves a lot of debugging time.

Every once in a while, you'll see a C program crash, with a message like “segmentation fault” or bus error.” It won't helpfully include any indication of what part of the program is at fault: all you get is those those two words. Such errors usually mean that the program attempted to access an invalid memory location. This may indicate an attempt to access an invalid array index, but typically the index needs to be pretty far out of bounds for this to occur. (It often instead indicates an attempt to reference an uninitialized pointer or a NULL pointer, which we'll discuss later.)

### 2.6. Comments

In C's original design, all comments begin with a slash followed by an asterisk (“/\*”) and end with an asterisk followed by a slash (“\*/”). The comment can span multiple lines.

```c

```

(The asterisk on the second line is ignored by the compiler. Most programmers would include it, though, both because it looks prettier and also because it indicates to a human reader that the comment is being continued from the previous line.)

Though this multi-line comment was the only comment originally included with C, C++ introduced a single-line comment that has proven so handy that most of today's C compilers also support it. It starts with two slash characters (“//”) and goes to the end of the line.

```c

```
