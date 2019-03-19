## 1. Expressões e comandos

Agora que vimos como construir um programa completo, vamos aprender o universo do que podemos fazer dentro de uma função C, executando através de diferentes tipos de comandos.

### 2.1. Operadores

Um  operador  é algo que podemos usar em expressões aritméticas, como um sinal de mais '+' ou um teste de igualdade '\ =='. Os operadores da linguagem C parecerão familiares, pois o designer da Python, Guido van Rossum, utilizou como referência esses operadores; mas existem algumas diferenças significativas.

 
> C operator precedence
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

> Python operator precedence
> ``  
> `+ -` (unary)  
> `* / % //`  
> `+ -` (binary)  
> `< > <= >= == !=`  
> `not`  
> `and`  
> `or`


Algumas distinções importantes: 

* C não possui um operador de exponenciação como o operador `` do Python. Para exponenciação em C, você deverá usar uma função de biblioteca denominada `pow ()`. Por exemplo, `pow (1.1, 2.0)` calcula 1.1². 

* C usa símbolos em vez de palavras para as operações booleanas AND (`&&`), OR (`||`) e NOT (`!`). 
  
* O nível de precedência de NOT (o operador `!`) É muito alto em C. Isso quase nunca é desejado, então você acaba precisando de parênteses na maioria das vezes quer for usar o operador `!`. 
  
* C define a atribuição como um operador, enquanto o Python define a atribuição como um comando. O valor do operador de atribuição é o valor atribuído. Uma consequência do design de C é que uma atribuição pode legalmente ser parte de outra declaração.

```c
while ((a = getchar()) != EOF)
```    
    
Aqui, nós atribuímos o valor retornado por `getchar ()` à variável `a`, e então testamos se o valor atribuído a` a` corresponde à constante `EOF`, que é usada para decidir se deve repetir o loop novamente . Muitas pessoas afirmam que esse estilo de programação é extraordinariamente ruim; outros acham muito conveniente evitar. O Python, é claro, foi projetado de modo que uma atribuição deva ocorrer como sua própria instrução separada, portanto, as atribuições dentro de uma condição de um comando `while` é ilegal no Python.

    
*   Os operadores `++` e `--` são para incrementar e decrementar uma variável. Assim, a declaração ` i ++ ` é uma forma mais curta da declaração ` i = i + 1` (ou` i + = 1` ).
    
*  O operador de divisão `/` realiza uma divisão inteira se ambos operandos forem do tipo `int`; isto é, qualquer resto é ignorado com tal divisão. Assim, em C, a expressão ` 13 / 5` é avaliada como 2, enquanto `13 / 5.0` é 2.6: A primeira tem valores inteiros como operandos de ambos lados, enquanto a segunda tem um número de ponto flutuante à direita.
    
    > Por outro lado, com versões mais recentes do Python (3.0 e posterior), o operador de barra única sempre faz a divisão de ponto flutuante. Com versões mais antigas do Python, o operador single-slash funcionava como o C, mas isso muitas vezes levava a bugs - em parte porque o tipo associado a uma variável não é fixo como está em um programa em C.
    

### 2.2. Tipos básicos

A lista de tipos de C é bastante restrita.

* `int` para um inteiro
* `char` para um único caractere
* `float` para um número de ponto flutuante de precisão simples
* `double` para um número de ponto flutuante de precisão dupla

O tipo `int` é o mais simples. Você também pode criar outros tipos inteiros, usando os nomes de tipo `long` e `short`. Um `long` reserva pelo menos tantos bits quanto um `int`, enquanto um `short` reserva menos bits que um `int` (ou o mesmo número ). A linguagem não garante o número de bits para cada um, mas a maioria dos compiladores atuais usa 32 bits para um `int`, o que permite números de até 2.15 × 10<sup>9</sup>. Isso é suficiente para a maioria dos propósitos, e muitos compiladores também usam 32 bits para um `long`, então as pessoas normalmente usam o `int` em seus programas.

O tipo ` char` também é simples: representa um único caracter, como uma letra ou pontuação. Você pode representar um caracter individual em um programa colocando o caractere entre aspas simples: `digit0 = '0'; ` atribuiria o caracter zero em uma variável `char denominada` `digit0`.

Dos dois tipos de ponto flutuante, `float` e `double`, a maioria dos programadores hoje utilizam quase exclusivamente o tipo `double`. Esses tipos são para números que podem ter um ponto decimal neles, como 2.5, ou para números maiores que um `int` podendo conter valores como 6.02 × 10<sup>23</sup>. Os dois tipos diferem em que um `float` normalmente utiliza apenas 32 bits de armazenamento, enquanto um `double` normalmente utiliza 64 bits. A técnica de armazenamento de 32 bits permite um intervalo de números mais estreito (−3,4 × 10<sup>38</sup> a 3,4 × 10<sup>38</sup>) e - mais problemático - cerca de 7 dígitos significativos. Um `float` não pôde armazenar um número como 281.421.906 (a população dos EUA em 2000, de acordo com o censo), porque requer nove dígitos significativos; teria que armazenar uma aproximação, como 281,421,920. Por outro lado, a técnica de armazenamento de 64 bits permite um intervalo mais amplo de números (-1,7 × 10<sup>308</sup> a 1,7 × 10<sup>308</sup>) e aproximadamente 15 dígitos significativos. Isso é mais adequado para propósitos gerais, e os 32 bits extras de armazenamento raramente valem a pena, então `double` é quase sempre preferível.


C não possui um tipo booleano para representar valores true / false. Isto tem implicações importantes para uma declaração como `if `, onde você precisa de um teste para determinar se deve executar o corpo. A abordagem de C é tratar o inteiro 0 como _false_ e todos os outros valores inteiros como _true_. O seguinte seria um programa C legal.

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

Este programa compilaria, e imprimiria “in if” quando executado, desde que o valor da expressão `if( i)` seja 5, o que não é 0 e, portanto a condição seria verdadeira.

Os operadores de C que parecem operar sobre valores booleanos (como `==`, `&&`, e `||`), na verdade, operam sobre valores inteiros. Em particular, eles computam 1 para representar _true_ e 0 para representar _false_. Isso significa que você pode legitimamente digitar o seguinte para contar quantos `a`,` b` e `c` são positivos.

```c
pos = (a > 0) + (b > 0) + (c > 0);
```
Essa peculiaridade - que C considera verdadeiros os inteiros diferentes de zero - é geralmente considerado um erro. Quando a linguagem foi desenvolvido as linguagens de máquina raramente tinham suporte direto para valores booleanos, normalmente esperava que você realize esses testes comparando-os com zero. Mas os compiladores melhoraram além do ponto em que estavam quando o C foi inventado, e agora podem facilmente traduzir comparações booleanas com código de máquina eficiente. Além do mais, esse design de C leva a programas confusos, portanto, a maioria dos programadores especialistas em C evitam usar o atalho, preferindo compará-lo explicitamente a zero como uma questão de bom estilo de programação. Mas tal prática não conserta o fato de que essa peculiaridade da linguagem geralmente leva a erros de programação. A maioria das linguagens mais novas optam por um tipo especial associado a valores booleanos. (O Python possui seu próprio tipo booleano, mas também trata 0 como falso para as instruções `if`).

### 2.3.Chaves 


Várias declarações, como a instrução `if `, incluem um corpo que pode conter várias instruções. Normalmente, o corpo é cercado por chaves ('{' e '}') para indicar sua extensão. Mas quando o corpo possui apenas uma declaração, as chaves são opcionais. Assim, podemos encontrar o máximo de dois números sem chaves, já que o corpo de ambos, ` if ` e `else`, contém apenas uma única instrução.

```c
if (first > second)
    max = first;
else
    max = second; 
```

(Também poderíamos incluir chaves em apenas um dos dois corpos, desde que esse corpo contenha apenas uma declaração.)

Os programadores C usam isso com bastante frequência quando querem que um dos vários testes ` if ` sejam executados. Um exemplo disso é com o código da fórmula quadrática abaixo. Podemos calcular o número de soluções da seguinte forma:

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

Note que a cláusula `else ` aqui contém apenas uma instrução (uma declaração `if … else`), então podemos omitir as chaves ao redor dela. Podemos então escrever da seguinte maneira:

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

Mas essa situação surge com freqüência suficiente para que os programadores C sigam uma regra especial de indentanção nesse caso - uma regra que permite que todos os casos sejam escritos no mesmo nível de recuo.
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
Como isso é viável usando as regras de chaves de C, C não inclui o conceito de uma cláusula `elif` que você encontra no Python. Você pode apenas agrupar quantas combinações `else if ` que você quiser. 

Além desta situação particular, eu recomendo que você inclua as chaves de qualquer maneira. À medida que você continua trabalhando em um programa, você freqüentemente acha que deseja adicionar instruções adicionais no corpo de um `if `, e ter as chaves lá já lhe poupa o incômodo de adicioná-las mais tarde. E torna mais fácil manter o controle das chaves, já que cada nível de indentação requer uma chave direita de fechamento.

### 2.4. Comandos

We've seen four types of statements, three of which correlate closely with Python.

1. `int x;`

We already discussed variable declarations in [Section 1.2](#s1.2). They have no parallel in Python.

2. `x = y + z;` ou `printf("%d", x);`

You can have an expression as a statement. Technically, the expression could be “`x + 3;`”, but such a statement has no point: We ask the computer to add `x` and 3, but we don't ask anything to happen to it. Almost always, the expressions have one of two forms: One form is an operator that changes a variable's value, like the assignment operator (“`x = 3;`”), the addition assignment operator `+=`, or the the increment operator `++`. The other form of expression that you see as a statement is a function call, like a statement that simply calls the `printf()` function.

3\. `if (x < 0) { printf("negative"); }`

You can have an `if` statement, which works very similarly to Python's `if` statement. The only major difference is the syntax: In C, an `if` statement's condition must be enclosed in parentheses, there is no colon following the condition, and the body has a set of braces enclosing it.

As we've already seen, C does not have an `elif` clause as in Python; instead, C programmers use the optional-brace rule and write “`else if`”.

4\. `return 0;`

You can have a `return` statement to exit a function with a given return value. Or for a function with no return value (and a `void` return type), you would write simply “`return;`”.

There are three more statement types that correlate closely to equivalents from Python.

5\. `while (i >= 0) { i--; }`

The `while` statement works identically to Python's, although the syntax is different in the same way that the `if` syntax is different.

```c
while (i >= 0) {
    printf("%d\n", i);
    i--;
} 
```

Again, the test expression requires a set of parentheses around it, there is no colon, and we use braces to surround the loop's body.

6\. `break;`

As in Python, the `break` statement immediately exits the innermost loop in which it is found. Of course, the statement has a semicolon following it.

1. `continue;`

Also as in Python, the `continue` statement skips to the bottom of the innermost loop in which it is found and tests whether to repeat the loop again. It has a semicolon following it, too.

And there are two types of statements that have no good parallel in Python.

8. `for (i = 0; i < 10; i++) {…`

While Python also has a `for` statement, its purpose and its syntax bear scant similarity to C's `for` statement. In C, the `for` keyword is followed by a set of parentheses containing three parts separated by semicolons.

```c
for (init; test; update)
```

The intent of C's `for` loop is to enable stepping a variable through a series of numbers, like counting from 0 to 9. The part before the first semicolon (_init_) is performed as soon as the `for` statement is reached; it is for initializing the variable that will count. The part between the two semicolons (_test_) is evaluated before each iteration to determine whether the iteration should be repeated. And the part following the final semicolon (_update_) is evaluated at the end of each iteration to update the counting variable for the following iteration.

In practice, `for` loops are used most often for counting out n iterations. The standard idiom for this is the following.

```c
for (i = 0; i < n; i++) {
    comandos
}
```
Here we have a counter variable `i` whose value starts at 0. With each iteration, we test whether `i` has reached n or not; and if it hasn't, then we execute the `for` statement's body and then perform the `i++` update so that `i` goes to the following integer. The result is that the body is executed for each value of `i` from 0 up to n − 1.

But you can use a `for` loop for other purposes, too. In the following example, we display the powers of 2 up to 512. Notice how the update portion of the `for` statement has changed to “`p *= 2`”.

```c
for (p = 1; p <= 512; p *= 2) {
    printf("%d\n", p);
} 
```

9\. `switch (grade) { case 'A':`…

The `switch` statement has no equivalent in Python, but it is essentially equivalent to a particular form of an `if`…`elif`…`elif`…`else` statement where each of the tests are for different values of the same variable.

A `switch` statement is useful when you have several possible blocks of code, one of which should be executed based on the value of a particular expression. Here is a classic instance of the `switch` statement:

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

Inside the parentheses following the `switch` keyword, we have an expression, whose value must be a character or integer. The computer evaluates this expression and goes down to one of the `case` keywords based on its value. If the value is the character _A_, then the first block is executed (`gpa += 4; credits += 1;`); if it is _B_, then the second block is executed; if it is none of the characters (like an _F_), the block following the `default` keyword is executed.

The `break` statement at the end of each block is a crucial detail: If the `break` statement is omitted, then the computer continues into the following block. In our above example, if we omitted all `break` statements, then a grade of _A_ would lead the computer to execute not only the _A_ case but also the _B_, _C_, _D_, _W_, and `default` cases. The result would be that `gpa` would increase by 4 + 3 + 2 + 1 = 10, while `credits` would increase by 5. Occasionally you actually want the computer to continue to the next case (called “fall-through”), and so you omit a `break` statement; but in practice you almost always want `break` statement at the end of each case.

There is one important exception where fall-through is quite common: Sometimes you want the same code to apply to two different values. For instance, if we wanted the nothing to happen whether the grade is _P_ or _W_, then we could include “`case 'P':`” just before “`case 'W'`”, with no intervening code.

### 2.5. Arrays

Python supports many types that combine the basic atomic types into a group: tuples, lists, strings, dictionaries, sets.

C's support is much more rudimentary: The _only_ composite type is the array, which is similar to Python's list except that an array in C cannot grow or shrink — its size is fixed at the time of creation. You can declare and access an array as follows.

```c
double pops[50];
pops[0] = 897934;
pops[1] = pops[0] + 11804445; 
```

In this example, we create an array containing 50 slots for `double` values. The slots are indexed 0 through 49.

C does not have an support for accessing the length of an array once it is created; that is, there is nothing analogous to Python's `len(pops)` or Java's `pops.length`.

An important point with respect to arrays: What happens if you access an array index outside the array, like accessing `pops[50]` or `pops[-100]`? With Python or Java, this will terminate the program with a friendly message pointing to the line at fault and saying that the program went beyond the array bounds. C is not nearly so friendly. When you access beyond an array bounds, it blindly does it.

This can lead to peculiar behavior. For example, consider the following program.

```c
int main() {
    int i;
    int vals[5];

    for (i = 0; i <= 5; i++) {
        vals[i] = 0;
    }
    printf("%d\n", i);
    return 0;
} 
```

Some systems (including a Linux installation I've encountered) would place `i` in memory just after the `vals` array; thus, when `i` reaches 5 and the computer executes “`vals[i] = 0`”, it in fact resets the memory corresponding to `i` to 0. As a result, the `for` loop has reset, and the program goes through the loop again, and again, repeatedly. The program never reaches the `printf` function call, and the program never terminates.

In more complicated programs, the lack of array-bounds checking can lead to very difficult bugs, where a variable's value changes mysteriously somewhere within hundreds of functions, and you as the programmer must determine where an array index was accessed out of bounds. This is the type of bug that takes a lot of time to uncover and repair.

That's why you should consider the error messages provided by Python (or Java) as extraordinarily friendly: Not only does it tell you the cause of a problem, it even tells you exactly which line of the program was at fault. This saves a lot of debugging time.

Every once in a while, you'll see a C program crash, with a message like “segmentation fault” or bus error.” It won't helpfully include any indication of what part of the program is at fault: all you get is those those two words. Such errors usually mean that the program attempted to access an invalid memory location. This may indicate an attempt to access an invalid array index, but typically the index needs to be pretty far out of bounds for this to occur. (It often instead indicates an attempt to reference an uninitialized pointer or a NULL pointer, which we'll discuss later.)

### 2.6. Comments

In C's original design, all comments begin with a slash followed by an asterisk (“/\*”) and end with an asterisk followed by a slash (“\*/”). The comment can span multiple lines.

```c
/* gcd - returns the greatest common
 * divisor of its two parameters */
int gcd(int a, int b) { 
```

(The asterisk on the second line is ignored by the compiler. Most programmers would include it, though, both because it looks prettier and also because it indicates to a human reader that the comment is being continued from the previous line.)

Though this multi-line comment was the only comment originally included with C, C++ introduced a single-line comment that has proven so handy that most of today's C compilers also support it. It starts with two slash characters (“//”) and goes to the end of the line.

```c
int gcd(int a, int b) {
  if (b == 0) {
    return a;
  } else {
    // recurse if b != 0
    return gcd(b, a % b);
  }
} 
```
