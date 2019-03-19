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

Vimos quatro tipos de instruções, três das quais se correlacionam de perto com o Python.

1. `int x;`

Nós já discutimos declarações de variáveis em [Seção 1.2]. Eles não têm paralelo no Python.

2. `x = y + z;` ou `printf("%d", x);`

Você pode ter uma expressão como um comando. Tecnicamente, a expressão poderia ser `x + 3`; mas tal afirmação não tem sentido: pedimos ao computador para adicionar x e 3, mas não pedimos que nada aconteça a ele. Quase sempre, as expressões têm uma de duas formas: Uma forma é um operador que altera o valor de uma variável, como o operador de atribuição (`x = 3;`), o operador de atribuição de adição `+ =` ou o operador de incremento ` ++ `. A outra forma de expressão que você vê como uma instrução é uma chamada de função, como uma instrução que simplesmente chama a função `printf ()`.

3. `if (x < 0) { printf("negative"); }`

Você pode ter um comando  `if`, que funciona de forma muito semelhante ao `if` do Python. A única grande diferença é a sintaxe: Em C, uma condição da declaração if deve ser colocada entre parênteses, não há dois pontos após a condição, e o corpo tem um conjunto de chaves que a encerram.

Como já vimos, C não possui uma cláusula `elif` como no Python; em vez disso, os programadores C usam a regra de chave opcional e escrevem ` else if` .

4. `return 0;`

Você pode ter um comando `return` para sair de uma função com um determinado valor de retorno. Ou para uma função sem valor de retorno (ou seja o tipo de retorno 'void'), você escreveria simplesmente `return;`.

Existem mais três tipos de instruções que se correlacionam de perto com os equivalentes do Python.

5. `while (i >= 0) { i--; }`

O comando `while` funciona de forma idêntico à do Python, embora a sintaxe seja diferente da mesma maneira que a sintaxe `if`.

```c
while (i >= 0) {
    printf("%d\n", i);
    i--;
} 
```

Novamente, a expressão de teste requer um conjunto de parênteses em torno dela, não há cólon e usamos chaves para cercar o corpo do loop.

6. `break;`

Como no Python, o comando `break` sai imediatamente do loop mais interno em que ela é encontrada. Naturalmente, a declaração tem um ponto e vírgula a seguir.

7. `continue;`

Também como em Python, o comando `continue` pula para a parte inferior do loop mais interno no qual ele é encontrado e testa se deve repetir o loop novamente. Tem um ponto e vírgula também.

E existem dois tipos de comandos que não possuem um bom paralelo no Python.

8. `for (i = 0; i < 10; i++) {…`

Embora o Python também tenha um comando `for`, seu propósito e sua sintaxe possuem pouca similaridade com a declaração` for` de C. Em C, a palavra-chave `for` é seguida por um conjunto de parênteses contendo três partes separadas por ponto e vírgula.

```c
for (init; test; update)
```

A intenção do loop `for` de C é permitir passar uma variável através de uma série de números, como a contagem de 0 a 9. A parte antes do primeiro ponto e vírgula (_init_) é executada assim que a instrução` for` é alcançada; é para inicializar a variável que contará. A parte entre os dois pontos e vírgulas (_test_) é avaliada antes de cada iteração para determinar se a iteração deve ser repetida. E a parte que segue o ponto-e-vírgula final (_update_) é avaliada no final de cada iteração para atualizar a variável de contagem para a iteração a seguir.

Na prática, loops 'for' são usados com mais freqüência para contar n iterações. O idioma padrão para isso é o seguinte.

```c
for (i = 0; i < n; i++) {
    comandos
}
```
Aqui temos uma variável de contador `i` cujo valor começa em 0. Com cada iteração, testamos se ` i` atingiu `n` ou não; e se não tiver, então nós executamos o corpo da declaração `for` e então executamos a atualização` i++ `para que `i` vá para o inteiro seguinte. O resultado é que o corpo é executado para cada valor de `i` de 0 até n - 1.

Mas você também pode usar um laço `for` para outros propósitos. No exemplo a seguir, exibimos as potências de 2 até 512. Observe como a parte de atualização da declaração `for` mudou para“ `p *= 2`”.

```c
for (p = 1; p <= 512; p *= 2) {
    printf("%d\n", p);
} 
```

9. `switch (conceito) { case 'A':`…

O comando `switch` não tem equivalente em Python, mas é essencialmente equivalente a uma forma particular de uma declaração` if`… `elif`…` elif`… `else` onde cada um dos testes é para valores diferentes da mesma variável.

Um comando `switch` é útil quando você tem vários blocos de código possíveis, um dos quais deve ser executado com base no valor de uma expressão específica. Aqui está uma instância clássica da declaração `switch`:

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

Dentro dos parênteses após a palavra chave `switch`, temos uma expressão cujo valor deve ser um caractere ou inteiro. O computador avalia essa expressão e desce para uma das palavras-chave `case` com base em seu valor. Se o valor é o caractere _A_, então o primeiro bloco é executado (`gpa + = 4; créditos += 1;`); se for _B_, então o segundo bloco é executado; se não for nenhum dos caracteres (como um _F_), o bloco que segue a palavra-chave `default` é executado.

O comando `break` no final de cada bloco é um detalhe crucial: Se o comando `break` for omitida, o computador continua no bloco seguinte. Em nosso exemplo acima, se omitimos todas as declarações `break`, então uma nota de _A_ levaria o computador a executar não apenas o caso _A_, mas também os casos _B_, _C_, _D_, _W_ e `default`. O resultado seria que o `gpa` aumentaria em 4 + 3 + 2 + 1 = 10, enquanto que `credits` aumentaria em 5. Ocasionalmente você realmente deseja que o computador continue no próximo caso (chamado de "fall-through") , e assim você omite uma declaração `break`; mas na prática você quase sempre quer a declaração `break` no final de cada caso.

Há uma exceção importante em que fall-through é bastante comum: às vezes, você deseja que o mesmo código seja aplicado a dois valores diferentes. Por exemplo, se quiséssemos que nada acontecesse se o grau fosse _P_ ou _W_, então poderíamos incluir ` case 'P': ` logo antes de `case 'W'`, sem nenhum código intermediário.


### 2.5. Arrays

O Python suporta muitos tipos que combinam os tipos atômicos básicos em um grupo:tuples, lists, strings, dictionaries, sets. 

O suporte de C é muito mais rudimentar: o tipo composto _só_ é o array, que é semelhante à lista do Python, exceto que um array em C não pode crescer ou encolher - seu tamanho é fixo no momento da criação. Você pode declarar e acessar uma matriz da seguinte maneira.


```c
double pops[50];
pops[0] = 897934;
pops[1] = pops[0] + 11804445; 
```

Neste exemplo, criamos um array contendo 50 slots para valores double. Os slots são indexados de 0 a 49.

C não tem suporte para acessar o tamanho de um array depois de criado; isto é, não há nada análogo ao `len (pops) do Python ou` pops.length` do Java.

Um ponto importante no que diz respeito aos arrays: O que acontece se você acessar um índice de array fora do array, como acessar `pops [50]` ou `pops [-100]`? Com Python ou Java, isso terminará o programa com uma mensagem amigável apontando para a linha com falha e dizendo que o programa foi além dos limites da matriz. C não é tão amigável. Quando você acessa além dos limites de um array, ele o faz cegamente.

Isso pode levar a um comportamento peculiar. Por exemplo, considere o programa a seguir.

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


Alguns sistemas (incluindo uma distribuição do Linux que eu uso) colocariam `i` na memória logo após a matriz` vals`; assim, quando `i` atinge 5 e o computador executa" `vals [i] = 0`", ele de fato redefine a memória correspondente a `i` para 0. Como resultado, o loop` for` foi zerado, e o programa passa pelo loop novamente e repetidamente. O programa nunca alcança a chamada de função `printf` e o programa nunca termina.

> Vamos testar ? :)

Em programas mais complicados, a falta de checagem de limites de array pode levar a bugs muito difíceis, onde o valor de uma variável muda misteriosamente em centenas de funções, e você como programador deve determinar onde um índice de array foi acessado fora dos limites. Este é o tipo de bug que leva muito tempo para descobrir e reparar.

É por isso que você deve considerar as mensagens de erro fornecidas pelo Python (ou Java) como extraordinariamente amigáveis: não apenas informa a causa de um problema, mas também informa exatamente qual linha do programa estava com defeito. Isso economiza muito tempo de depuração.

De vez em quando, você verá uma falha no programa em C, com uma mensagem como "falha de segmentação" ou erro de barramento. "Não será útil incluir qualquer indicação de qual parte do programa é a culpa: tudo que você consegue é aquelas essas duas palavras. Esses erros geralmente significam que o programa tentou acessar um local de memória inválido. Isso pode indicar uma tentativa de acessar um índice de matriz inválido, mas normalmente o índice precisa estar bem fora dos limites para que isso ocorra. (Em geral, isso indica uma tentativa de fazer referência a um ponteiro não inicializado ou a um ponteiro NULL, que discutiremos mais adiante.)

### 2.6. Comments

No design original de C, todos os comentários começam com uma barra seguida por um asterisco (“\*”) e terminam com um asterisco seguido por uma barra (“*/”). O comentário pode abranger várias linhas.

```c
/* gcd - returns the greatest common
 * divisor of its two parameters */
int gcd(int a, int b) { 
```

(O asterisco na segunda linha é ignorado pelo compilador. A maioria dos programadores o incluiria, porém, porque parece mais bonito e também porque indica a um leitor humano que o comentário está sendo continuado a partir da linha anterior.)

Embora este comentário em várias linhas tenha sido o único comentário originalmente incluído no C, o C ++ introduziu um comentário de linha única que provou ser tão útil que a maioria dos compiladores C atuais também o suportam. Começa com dois caracteres de barra (“//”) e vai para o final da linha.

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
