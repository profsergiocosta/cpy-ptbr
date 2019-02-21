## 1. Construindo um simples programa


Começaremos com vários princípios gerais, usnando para isso programa C completo, mas limitado, até o final da Seção 1.

### 1.1. Compiladores versus interpretadores

Uma grande diferença entre C e Python é simplesmente como você executa programas escritos nas duas linguagens. Com programas em C, você geralmente usa um _compilador_ antes de executar. Por outro lado, com o Python, você normalmente usa um _interpretador_. Um ** compilador ** gera um arquivo contendo a tradução do programa no código nativo da máquina. O compilador na verdade não executa o programa; em vez disso, você primeiro executa o compilador para criar um executável nativo e, em seguida, executa o executável gerado. Assim, depois de criar um programa em C, executá-lo é um processo de duas etapas.

```
sh:~$ gcc meuprograma.c
sh:~$ ./a.out
GCD: 8
```

No primeiro comando ("gcc meuprograma.c"), invocamos o compilador, chamado _gcc_. O compilador lê o arquivo meuprograma.c, no qual salvamos nosso código C, e ele gera um novo arquivo chamado a.out contendo uma tradução deste código no código binário usado pela máquina. No segundo comando ("./a.out"), dizemos ao computador para executar este código binário. Como está executando o programa, o computador não tem idéia de que a.out acabou de ser criado a partir de algum programa em C: ele simplesmente executa cegamente o código encontrado no arquivo a.out, da mesma forma que executa cegamente o código encontrado no gcc arquivo em resposta ao primeiro comando.

Por outro lado, um ** interpretador** lê o programa escrito pelo usuário e o executa diretamente. Isso remove uma etapa do processo de execução, mas um compilador tem a vantagem de gerar um executável que é executado da mesma forma que a maioria dos outros aplicativos da máquina, e usualmente gerando códigos mais rápidos. 

Ser compilado tem algumas implicações radicais no design da linguagem. C é projetada para que o compilador possa dizer tudo o que precisa saber para traduzir o programa C sem realmente executar o programa.

> Observação 1: esse texto tem como objetivo ser mais simples. Por exemplo, o processo de execução em C é feito em diversas outras etapas, que inclui a **pré-compilaação** e a **linkedição**.

> Observação 2: alguns interpretadores atuais podem usar um processo de compilação para um código intermediário. Então, o código executado não é exatamente o código escrito pelo programador.

### 1.2. Declarações Variáveis

Entre as informações que compilador da linguagem C requrer, um dos exemplos mais notáveis é a **declarações de variáveis**, informando ao compilador sobre a variável antes que a variável seja realmente usada. Isso é típico de muitas linguagens de programação proeminentes, particularmente entre aquelas que devem ser compiladas antes de serem executadas.

> Observação: essas linguagens que requerem a declaração de variáveis, associando cada variável a um tipo, são conhecidas como estaticamente tipadas.

Em C, a declaração da variável define o tipo **da variável**, especificando se é um inteiro (`int`), um número de ponto flutuante (`double`), caracter (`char`), ou algum outro tipo que estudaremos depois. Uma vez que você declara que uma variável é de um tipo particular, você não pode mudar seu tipo: Se a variável `x` é declarada do tipo`double`, e você atribui `x = 3;` , então ` x` irá, na verdade, manter o valor de ponto flutuante 3.0 em vez do inteiro 3. Você pode, se desejar, imaginar que `x` é uma caixa que é capaz apenas de manter valores `double `; se você tentar colocar alguma outra coisa dentro dele (como o `int` valor 3), o compilador o converterá em um `double` para que ele caiba. 

Declarar uma variável é bastante simples: você insere o tipo da variável, alguns espaços em branco, o nome da variável e um ponto e vírgula:

```c
`double x;`
```

Em C, as declarações de variáveis pertencem ao topo da função em que são usadas.

Se você esquecer de declarar uma variável, o compilador se recusará a compilar um programa no qual uma variável é usada, mas não é declarada. A mensagem de erro indicará a linha dentro do programa, o nome da variável e uma mensagem como "símbolo não declarado" (symbol undeclared). Por exemplo, dado o seguinte programa:

```c
int main () {
    a = 10;
    b = 20;
    c = a + b;
}
```

Uma tentativa de compilação, levaria aos seguintes erros:

```
$ gcc programaerrovariavelnaodeclarada.c
programaerrovariavelnaodeclarada.c:5:5: error: ‘a’ undeclared (first use in this function)
     a = 10;
     ^
programaerrovariavelnaodeclarada.c:5:5: note: each undeclared identifier is reported only once for each function it appears in
programaerrovariavelnaodeclarada.c:6:5: error: ‘b’ undeclared (first use in this function)
     b = 20;
     ^
programaerrovariavelnaodeclarada.c:7:5: error: ‘c’ undeclared (first use in this function)
     c = a + b;
     ^
```

Para um programador Python, parece difícil incluir essas declarações de variáveis em um programa, embora isso seja mais fácil com mais prática. Os programadores de C tendem a sentir que as declarações variáveis valem a menor dor. A maior vantagem é que o compilador irá identificar automaticamente sempre que um nome de variável for digitado incorretamente e apontar diretamente para a linha onde ele está escrito incorretamente. Isso é muito mais conveniente do que executar um programa e descobrir que ele está errado em algum lugar devido ao nome da variável com erros ortográficos.

### 1.3. Espaço em branco

Em Python, os espaços em branco, tabs e quebra de linhas são importantes: você separa suas instruções colocando-as em linhas separadas e indica a extensão de um bloco (como o corpo de uma instrução `while` ou `if`) usando recuo. Esses usos do espaço em branco são idiossincrasias do Python. (Reconhecidamente, FORTRAN e BASIC também usam quebras de linha para instruções separadas, mas nenhuma outra linguagem entre as principais depende de espaços em branco para indicar blocos.) 

Como a maioria das linguagens de programação, C não usa espaço em branco, exceto para separar palavras. A maioria das declarações é terminada com um ponto e vírgula `;`, e blocos de instruções são indicados usando um conjunto de chaves, `{` e `}`. Aqui está um fragmento de exemplo de um programa em C, com seu equivalente em Python.


```C
disc = b * b - 4 * a * c;
if (disc < 0) {
    num_sol = 0;
} else {
    t0 = -b / a;
    if (disc == 0) {
        num_sol = 1;
        sol0 = t0 / 2;
    } else {
        num_sol = 2;
        t1 = sqrt(disc) / a;
        sol0 = (t0 + t1) / 2;
        sol1 = (t0 - t1) / 2;
    }
} 
```
Equivalente em Python:

```python
disc = b * b - 4 * a * c
if disc < 0:
    num_sol = 0
else:
    t0 = -b / a
    if disc == 0:
        num_sol = 1
        sol0 = t0 / 2
    else:
        num_sol = 2
        t1 = disc ** 0.5 / a
        sol0 = (t0 + t1) / 2
        sol1 = (t0 - t1) / 2 
```

O programa C à esquerda é como eu escreveria. No entanto, o espaço em branco é insignificante, então o computador ficaria tão feliz se eu tivesse escrito o seguinte.

```C
disc=b*b-4*a*c;if(disc<0){
num_sol=0;}else{t0=-b/a;if(
disc==0){num_sol=1;sol0=t0/2
;}else{num_sol=2;t1=sqrt(disc/a;
sol0=(t0+t1)/2;sol1=(t0-t1)/2;}} 
```

Enquanto o computador pode estar tão feliz com isso, nenhum humano sensato preferiria. Assim, qualquer programador competente tende a ser muito cuidadoso com os espaços em branco para indicar a estrutura do programa.

(Existem algumas exceções à regra de ignorar o espaço em branco: É ocasionalmente significativo separar palavras e símbolos. O fragmento `intmain` é diferente do fragmento ` int main` ; da mesma forma, o fragmento `a++ + 1` é diferente do fragmento `a+ + +1`.)

### 1.4. The `printf()` function

As we work toward writing useful C programs, one important ingredient is displaying results for the user to see, which you would accomplish using `print` in Python. In C, you use `printf()` instead. This is actually a function, one of the most useful among C's library of language-defined functions.

The way the parameters to `printf()` work is a bit complicated but also quite convenient: The first parameter is a string specifying the format of what to print, and the following parameters indicate the values to print. The easiest way to understand this is to look at an example.

```c
`printf("# solns: %d\n", num_sol);`
```

This line says to print using “# solns: %d\n” as the format string. The `printf()` function goes through this format string, printing the characters “# solns: ” before getting to the percent character '%'. The percent character is special to `printf()`: It says to print a value specified in a subsequent parameter. In this case, a lower-case _d_ follows the percent character, indicating to display the parameter as an `int` in decimal form. (The _d_ stands for decimal.) So when `printf()` reaches “%d”, it looks at the value of the following parameter (let's imagine `num_sol` is 2 in this example) and displays that value instead. It then continues through the format string, in this case displaying a newline character. Overall, then, the user sees the following line of output:

> \# solns: 2

Like Python, C allows you to include escape characters in a string using a backslash. The “\n” sequence represents the newline character — that is, the character that represents a line break. Similarly, “\t” represents the tab character, “\"” represents the double-quote character, and “\\” represents the backslash character. These escape characters are part of C syntax, not part of the `printf()` function. (That is, the string the `printf()` function receives actually contains a newline, not a backslash followed by an _n_. Thus, the nature of the backslash is fundamentally different from the percent character, which `printf()` would see and interpret at run-time.)

Let's look at another example.

```c
printf("# solns: %d", num_sol);  
printf("solns: %f, %f", sol0, sol1);`
```

Let's assume `num_sol` holds 2, `sol0` holds 4, and `sol1` holds 1. When the computer reaches these two `printf()` function calls, it first executes the first line, which displays “\# solns: 2”, and then the second, which displays “solns: 4.0, 1.0”, as illustrated below.

> \# solns: 2solns: 4.0, 1.0

Note that `printf()` displays only what it is told, without adding any extra spaces or newlines; if we want a newline to be inserted between the two pieces of output, we would need to include “\n” at the end of the first format string.

The second call to `printf()` in this example illustrates how the function can print multiple parameter values. In fact, there's really no reason we couldn't have combined the two calls to `printf()` into one in this case.

```c
`printf("# solns: %dsolns: %f, %f",  
     num_sol, sol0, sol1);`
```

By the way, the `printf()` function displays “4.0” rather than simply “4” because the format string uses “%f”, which says to interpret the parameters as floating-point numbers. If you want it to display just “4”, you might be tempted to use “%d” instead. But that wouldn't work, because `printf()` would interpret the binary representation used for a floating-point number as a binary representation for an integer, and these types are stored in completely different ways. On my computer, replacing each “%f” with “%d” leads to the following output:

> `#** solns: 2solns: 0, 1074790400`

There's a variety of characters that can follow the percent character in the formatting string.

*   %d, as we've already seen, says to print an `int` value in decimal form.
*   %x says to print an `int` value in hexadecimal form.
*   %f says to print a `double` value in decimal-point form.
*   %e says to print a `double` value in scientific notation (for example, 3.000000e8).
*   %c says to print a `char` value.
*   %s says to print a string. There's no variable type for representing a string, but C does support some string facilities using arrays of characters. We'll defer discussion of these facilities to later, after we discuss pointers, as strings involve some more complex concepts that we haven't seen yet.

You can also include a number between the percent character and the format descriptor as in “%10d”, which tells `printf()` to right-justify a decimal integer over ten columns.

### 1.5. Functions

Unlike Python, all C code must be nested within functions, and functions cannot be nested within each other. Thus, a C program's overall structure is typically very straightforward: It is a list of function definitions, one after another, each containing a list of statements to be executed when the function is called.

Here's a simple example of a function definition:

```c
double expon(double b, int e) {
    if (e == 0) {
        return 1.0;
    } else {
        return b * expon(b, e - 1);
    }
}
```

A C function is defined by naming the return type (`double` here, since the function produces a floating-point result), followed by the function name (`expon`), followed by a set of parentheses listing the parameters. Each parameter is described by including the type of the parameter and the parameter name. Following the parameter list in parentheses is a set of braces, in which you nest the body of the function.

If you have a function that does not have any useful return value, then you'd use `void` as the return type.

Programs have one special function named `main`, whose return type is an integer. This function is the “starting point” for the program: The computer essentially calls the program's `main` function when it wants to execute the program. The integer return value is largely meaningless; we'll always return 0 rather than worrying about how the return value might be used.

We are now in a position to present a complete C program, along with its Python equivalent.

```c
int gcd(int a, int b) {
  if (b == 0) {
    return a;
  } else {
    return gcd(b, a % b);
  }
}

int main() {
  printf("GCD: %d\n",
    gcd(24, 40));
  return 0;
} 
```

```python
def gcd(a, b):
  if b == 0:
    return a
  else:
    return gcd(b, a % b)

print("GCD: " + str(gcd(24, 40)))
```

As you can see, the C program consists of two function definitions. In contrast to the Python program, where the `print` line exists outside any function definitions, the C program requires `printf()` to be in the program's `main` function, since this is where we put all top-level code that is to complete when the program is executed.)