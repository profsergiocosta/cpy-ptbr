3\. Libraries
-------------

Having discussed the internals to functions, we now turn to discussing issues surrounding functions and separating a program into various files.



### 3.1. Function prototypes

In C, a function must be declared above the location where you use it. In the C program of [Figure 2](#fig2), we defined the `gcd()` function first, then the `main()` function. This is significant: If we swapped the `gcd()` and `main()` functions, the compiler would complain in `main()` that the `gcd()` function is undeclared. The is because C assumes that a compiler reads a program from the top down: By the time it gets to `main()`, it hasn't been told about a `gcd()` function, and so it believes that no such function exists.

This raises a problem, especially in larger programs that span several files, where functions in one file will need to call functions in another. To get around this, C provides the notion of a **function prototype**, where we write the function header but omit the body definition.

As an example, say we want to break our C program into two files: The first file, math.c, will contain the `gcd()` function, and the second file, main.c, will contain the `main()` function. The problem with this is that, in compiling main.c, the compiler won't know about the `gcd()` function that it is attempting to call.

A solution is to include a function prototype in main.c.

> `**int** gcd(**int** a, **int** b);  
>   
> **int** main() {  
>     printf("GCD: %d\n",  
>         gcd(24, 40));  
>     **return** 0;  
> }`

The “`**int** gcd`…” line is the function prototype. You can see that it begins the same as a function definition begins, but we simply put a semicolon where the body of the function would normally be. By doing this, we are declaring that the function will eventually be defined, but we are not defining it yet. The compiler accepts this and obediently compiles the program with no complaints.

### 3.2. Header files

Larger programs spanning several files frequently contain many functions that are used many times in many different files. It would be painful to repeat every function prototype in every file that happens to use the function. So we instead create a file — called a **header file** — that contains each prototype written just once (and possibly some additional shared information), and then we can refer to this header file in each source file that wants the prototypes. The file of prototypes is called a header file, since it contains the “heads” of several functions. Conventionally, header files use the .h prefix, rather than the .c prefix used for C source files.

For example, we might put the prototype for our `gcd()` function into a header file called math.h.

> `**int** gcd(**int** a, **int** b);`

We can use a special type of line starting with `**#**include` to incorporate this header file at the top of main.c.

> `**#**include <stdio.h>  
> **#**include "math.h"  
>   
> **int** main() {  
>     printf("GCD: %d\n",  
>         gcd(24, 40));  
>     **return** 0;  
> }`

This particular example isn't very convincing, but imagine a library consisting of dozens of functions, which is used in dozens of files: Suddenly the time savings of having just a single prototype for each function in a header file begins making sense.

The `**#**include` line is an example of a directive for C's **preprocessor**, through which the C compiler sends each program before actually compiling it. A program can contain commands (**directives**) telling the preprocessor to manipulate the program text that the compiler actually processes. The `**#**include` directive tells the preprocessor to replace the `**#**include` line with the contents of the file specified.

You'll notice that we've placed stdio.h in angle brackets, while math.h is in double quotation marks. The angle brackets are for standard header files — files more or less built into the C system. The quotation marks are for custom-written header files that can be found in the same directory as the source files.

### 3.3. Constants

Another particularly useful preprocessor directive is the `**#**define` directive. It tells the preprocessor to substitute all future occurrences of some word with something else.

> `**#**define PI 3.14159`

In this fragment, we've told the preprocessor that, for the rest of the program, it should replace every occurrence of “`PI`” with “`3.14159`” instead. Suppose that later in the program is the following line:

> `printf("area: %f\n", PI * r * r);`

Seeing this, the preprocessor would translate it into the following text for the C compiler to process:

> `printf("area: %f\n", 3.14159 * r * r);`

This replacement happens behind the scenes, so that the programmer won't see the replacement.

The `**#**define` directive is not restricted to defining constants like this, though. Because it uses textual replacement only, the directive can be used (and abused) in other ways. For example, one might include the following.

> `**#**define forever **while**(1)`

Subsequently, you could use `forever` as if it were a loop construct, and the preprocessor would replace it with “`**while**(1)`.”

> `forever {  
>     printf("hello world\n");  
> }`

Expert C programmers consider this very poor style, since it quickly leads to unreadable programs.

These are the basics of writing C programs, giving you enough to be able to write reasonably useful programs. But to be a proficient programmer in C, you'd need to know about pointers — a topic that we'll defer to another time.
