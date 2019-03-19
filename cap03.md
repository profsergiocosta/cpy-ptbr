## 3. Bibliotecas


Tendo discutido as funções internas, agora nos voltamos para discutir questões relacionadas a funções e separar um programa em vários arquivos.


### 3.1. Protótipos de função

Em C, uma função deve ser declarada acima do local onde você a usa. No programa C de [Figura 2], definimos a função `gcd ()` primeiro, depois a função `main ()`. Isto é significativo: Se nós trocamos as funções `gcd ()` e `main ()`, o compilador iria reclamar em `main() `que a função `gcd ()` não foi declarada. Isso porque C assume que um compilador lê um programa de cima para baixo: No momento em que chega ao `main ()`, ele não foi informado sobre uma função `gcd ()`, e por isso ele acredita que a função não exista.

Isso gera um problema, especialmente em programas maiores que abrangem vários arquivos, em que as funções de um arquivo precisarão chamar funções em outro. Para contornar isso, C fornece a noção de um **protótipo de função**, onde escrevemos o cabeçalho da função mas omitimos a definição do corpo.

Por exemplo, digamos que queremos quebrar nosso programa C em dois arquivos: O primeiro arquivo, math.c, conterá a função `gcd ()` e o segundo arquivo, main.c, conterá o `main () `função. O problema com isso é que, na compilação main.c, o compilador não saberá sobre a função `gcd ()` que está tentando chamar.

Uma solução é incluir um protótipo de função em main.c.


```c
int gcd(int a, int b);

int main() {
    printf("GCD: %d\n", gcd(24, 40));
    return 0;
}

```

A linha `int gcd ...` é o protótipo da função. Você pode ver que começa da mesma forma que uma definição de função, mas nós simplesmente colocamos um ponto-e-vírgula onde o corpo da função normalmente seria. Ao fazer isso, estamos declarando que a função será eventualmente definida, mas ainda não a definimos. O compilador aceita isso e obedientemente compila o programa sem reclamações.

### 3.2. Arquivos de cabeçalho (Header files)

Programas maiores que abrangem vários arquivos freqüentemente contêm muitas funções que são usadas muitas vezes em muitos arquivos diferentes. Seria doloroso repetir cada protótipo de função em todos os arquivos que usam a função. Então, criamos um arquivo - chamado de **arquivo de cabeçalho** - que contém cada protótipo escrito apenas uma vez (e possivelmente algumas informações compartilhadas adicionais), e então podemos nos referir a esse arquivo de cabeçalho em cada arquivo de origem que deseja os protótipos. O arquivo de protótipos é chamado de arquivo de cabeçalho, pois contém as “heads” ou "cabeças" de várias funções. Convencionalmente, os arquivos de cabeçalho usam o prefixo `.h`, em vez do prefixo `.c` usado para os arquivos fonte C.


Por exemplo, podemos colocar o protótipo da nossa função `gcd ()` em um arquivo de cabeçalho chamado math.h.

```c
int gcd(int a, int b);
```

Podemos usar um tipo especial de linha começando com `#include` para incorporar este arquivo de cabeçalho no topo do main.c.

```c
#include <stdio.h>
#include "math.h"

int main() {
    printf("GCD: %d\n",
        gcd(24, 40));
    return 0;
} 
```

Este exemplo em particular não é muito convincente, mas imagine uma biblioteca que consiste em dezenas de funções, que são usadas em dezenas de arquivos: De repente, a economia de tempo de ter apenas um único protótipo para cada função em um arquivo de cabeçalho começa a fazer sentido.

A linha `#include` é um exemplo de uma diretiva para o pré-processador do C, através da qual o compilador C envia cada programa antes de compilá-lo. Um programa pode conter comandos (**diretivas**) informando ao pré-processador para manipular o texto do programa que o compilador realmente processa. A diretiva `#include` diz ao pré-processador para substituir a linha`#include` pelo conteúdo do arquivo especificado.

Você notará que colocamos `stdio.h` entre colchetes, enquanto `math.h` está entre aspas duplas. Os colchetes angulares são para arquivos de cabeçalho padrão - arquivos mais ou menos incorporados ao sistema C. As aspas são para arquivos de cabeçalho personalizados que podem ser encontrados no mesmo diretório dos arquivos fonte.


### 3.3. Constantes

Outra diretiva de pré-processador particularmente útil é a diretiva `#define`. Ela diz ao pré-processador para substituir todas as ocorrências futuras de alguma palavra por outra.

```c
#define PI 3.14159
```

Neste fragmento, dissemos ao pré-processador que, para o resto do programa, ele deveria substituir cada ocorrência de "PI" por "3.14159". Suponha que mais tarde no programa tenha a seguinte linha:


```c
printf("area: %f\n", PI * r * r);
```
Vendo isso, o pré-processador iria traduzi-lo no seguinte texto para o compilador C processar:

```c
printf("area: %f\n", 3.14159 * r * r);
```
> vamos testar ? :)

Essa substituição acontece nos bastidores, para que o programador não veja a substituição.

A diretiva `# define` não está restrita a definir constantes como essa, no entanto. Por usar apenas substituição textual, a diretiva pode ser usada (e abusada) de outras maneiras. Por exemplo, pode-se incluir o seguinte.

```c
#define forever while(1)
```
Posteriormente, você poderia usar `forever` como se fosse uma construção de loop, e o pré-processador a substituiria por“ `while(1)` ”.

```c
forever {  
     printf("hello world\n");  
}
```
Os programadores especialistas em C consideram esse estilo muito ruim, já que ele leva rapidamente a programas ilegíveis.

Estas são as noções básicas de escrever programas em C, dando-lhe o suficiente para poder escrever programas razoavelmente úteis. Mas, para ser um programador proficiente em C, você precisaria saber sobre ponteiros - um tópico que adiaremos em outro momento.

