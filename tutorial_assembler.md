Este é um pequeno tutorial para o [Simple 8-bit Assembler Simulator](https://schweigi.github.io/assembler-simulator/)

Este simulador é uma ferramenta bastante interessante para aqueles que tem a curiosidade de saber como de fato um programa é executado em baixo nível de abstração. Ele simula uma cpu de arquitetura x86 e programável em linguagem assembly (baseado em NASM). Foi escrito em Javascript com Angular e pode ser executado em todos os dispositivos com navegador web.

# Componentes
- CPU de 8-bit;
- Memória RAM (256 _bytes_);
- Display de saídas (24 células);
- Entrada de código.

![simulador](https://user-images.githubusercontent.com/52214785/109286092-992b0a80-7800-11eb-888d-8f2caf3736a5.png)

Os componentes deste simulador são semelhantes aos componentes básicos de um computador preconinado pela arquitetura de _Von Neumman_.

# CPU
A CPU é composta por registradores e _flags_ que constituem seu estado interno de funcionamento. É capaz de executar um conjunto de instruções pré-definidas, mas somente uma por cada ciclo de CPU.

![cpu](https://user-images.githubusercontent.com/52214785/109286364-e6a77780-7800-11eb-8c12-be36d59660fa.png)

### Registradores
Dentre os registradores existem 4 para propósito geral A,B,C e D e dois registradores especiais também chamados de ponteiros porque sempre apontam para um local na memória RAM, são eles: Ponteiro de instruções (IP) e Ponteiro de pilha (SP). A principal função dos registradores é armazenar os valores necessários para a execução de instrução/comando do usuário. Os registradores possuem capacidade de apenas um _byte_ de armazenamento, ou seja, só podem armazenar valores entre `0` a `255`.

O IP sempre aponta para a próxima instrução a ser executada, ou seja, aponta para a localização da instrução na memória. É dessa forma que a CPU sempre sabe qual instrução deve ser executada a cada ciclo. Ele é sempre incrementado de 3 em 3 _bytes_.

O SP sempre aponta paro topo atual de uma estrutura de dados chamada pilha. Esta pilha está localizada no final da memória RAM e pode ser manipulada por instruções específicas de pilha: `PUSH` e `POP` que podem incrementar ou decrementar o ponteiro.

### Flags
_Flags_ ou em português sinalizadores são ferramentas que auxiliam o programador em tomadas de decisões armazenando ou simplesmente indicando o resultado de uma determinada instrução executada. Este simulador posssui 3 _flags_ que são: Z, C e F. Essas _flags_ representam valores booleanos e só podem assumir dois estados `TRUE` ou `FALSE`.

- _Flag_ Zero (Z): Indica quando o resultado de uma instrução for 0.
- _Flag_ Carry (C): Indica quando uma instrução utilizou bit de transporte (vai um).
- _Flag_ Fault (F): Indica quando uma instrução causa um estado de falha na CPU, por exemplo, divisão por 0. Caso esta _flag_ esteja ativa **nenhuma** outra instrução será executada.

Quando a CPU é resetada (precionado o botão _Reset_) os registradores são limpos descartando qualquer valor armazenado arteriormente e as _flags_ retornam para o valor `FALSE`. 

# Memória RAM

A memória é formada por um conjunto de 256 _bytes_ ordenados de cima para baixo e da esquerda para a direita. Cada _byte_ da memória possui um endereço específico e que pode ser acessado aleatoriamente pelo programador. Além disso, seguindo o conceito de **programa armazenado** todo programa necessita estar armazenado na memória para poder ser executado pela CPU. 

Este simulador tem característica de uma máquina do tipo **memória-registrador** pois permite que a CPU realize operações diretamente com a memória RAM sem ser necessário que os dados de origem estejam armazenados em um registrador.

![RAM](https://user-images.githubusercontent.com/52214785/109286589-3128f400-7801-11eb-87e2-08112d87cf41.png)


# Display de saída 

Caso queira exibir informações como se estivesse utilizando um monitor use o _display_ de saídas. Na verdade ele é bastante simples, composto por apenas 24 células ASCII nas quais é possível reproduzir diferentes caracteres como um "Hello World!". Para utilizá-lo basta mover os caracteres que deseja exibir em um local específico da memória, os últimos 24 _bytes_.

![output](https://user-images.githubusercontent.com/52214785/109286903-a3013d80-7801-11eb-87f4-e46e0a0e1de3.png)


# Assembler

O _assembler_ ou montador faz parte de um conjunto de softwares básicos que exige um profundo conhecimento da arquitetura do sistema onde serão executados. Diferente dos compiladores um montador deve realizar traduções com relação biunívoca entre um símbolo e o conjunto de bits correspondente.

Ele é o responsável por analisar cada linha de código, analisar os operandos e gerar as instruções da CPU. Assim que o botão `ASSEMBLE` é acionado o _assembler_ gera um programa executável que pode ser armazenado na memória RAM. 

# Botões de controle

Os principais botões deste simulador são `RUN`/`STOP`, `STEP` e `RESET`. Pressionar o botão `RUN` aciona o montador que gera, carrega as intruções na memória e inicia um cronômetro necessário para controle dos ciclos de CPU. O botão `STOP` para o cronômetro de ciclos de CPU. O botão `STEP` permite a depuração do código executando um ciclo por vez. E finalmente o botão `RESET` redefine todos os valores de memória e estados da CPU para 0 ou `FALSE`.

Durante a execução de um programa o IP é incrementado ciclo a ciclo até encontrar uma instrução de parada (HALT), neste momento a execução do programa e finalizada. 

![botões](https://user-images.githubusercontent.com/52214785/109287669-a5b06280-7802-11eb-8365-9a09d685dd67.png)

# Programação em Assembly
Basicamente existem dois níveis de abstração para programação: linguagens de alto nível representado por C, Java, Python etc..  e linguagens de baixo nível representado por _assembly_. A principal diferença entre esses níveis é que uma linguagem de alto nível não tem relação unívoca com _assembly_, ou seja, seus comandos podem ser traduzidos de diferentes formas dependendo da situação e normalmente a linguagem de alto nível é idependente da máquina para qual o programa será traduzido. 


Para utilizar esse simulador será necessário conhecimento acerca da linguagem de baixo nível de abstração que é a linguagem mais próxima do _Hardware_. Cada CPU possui um conjunto de instruções disponíveis para uso imediato, essas instruções são representadas por mnemônicos e quando traduzidos formam um conjunto de bit intelegível pela CPU.

Este simulador é capaz de executar diversas intruções como soma, subtração, multiplicação entre outras. Cada instrução é identificada por um _opcode_ e acompanhada de no máximo 2 operandos que podem ser um registrador, endereço de memória, ou uma constante. A sintaxe básica de _assembly_ utilizada será escrever cada instrução em uma única linha contendo: O **mnemônico** seguido de seus **operandos** separados por vírgula. A ordem desses operandos importa. O primeiro operando sempre estará associado ao resultado da operação.

### Conjunto de instruções
Arquitetura do conjunto de instruções ou ISA (_Instruction Set Architecture_) é uma abstração que define como é possivel fazer a máquina funcionar. É um conjunto de palavras que compõe o idioma intelegível ao processador e outras partes da organização do computador.
#### MOV
Copia um valor da origem para destino. É a única instrução capaz de modificar diretamente a memória. Seus operandos podem ser registradores, endereços ou constantes.
```
MOV destino, origem
MOV reg, reg
MOV reg, endereço
MOV reg, constante
MOV endereço, reg
MOV endereço, constante
```

##### DB
Define uma variável. Essa variável pode ser um único número, _string_ ou caracter.
```
DB constante
```

#### ADD e SUB
Realiza operações matemáticas de adição e subtração. Adiciona dois valores ou subtrai um valor de outro. Essas operações podem modificar as _flags_ Z e C. Seus operandos podem ser registradores, endereços e constantes.

```
ADD reg, reg
ADD reg, endereço
ADD reg, constante
SUB reg, reg
SUB reg, endereço
SUB reg, constante
```

#### INC e DEC
Incrementa ou decrementa um determinado registrador por 1. Essas instruções podem modificar as _flags_ Z e C.
```
INC reg
DEC reg
```

#### MUL e DIV
Realiza operações matemáticas de multiplicação e divisão somente entre o registrador A e o valor do operando. Essas instruções podem modificar as _flags_ Z e C. Ao utilizar essas intruções lembre-se de primeiro alterar o valor do registrador A.

```
MUL reg
MUL endereço
MUL constante
DIV reg
DIV endereço
DIV constante
```

#### AND, OR, XOR, NOT
Realiza operações lógicas. Podem modificar as _flags_ Z e C. Seus operandos podem ser registradores, endereços e constantes.

```
AND reg, reg
AND reg, endereço
AND reg, constante
OR reg, reg
OR reg, endereço
OR reg, constante
XOR reg, reg
XOR reg, endereço
XOR reg, constante
NOT reg
```

#### SHL e SHR
Realiza operações de deslocamento de bit para esquerda (SHL) ocasinando em uma multiplicação ou direta (SHR) ocasionando em uma divisão. Essas instruções podem alterar as _flags_ Z e C. 

```
SHL reg, reg
SHL reg, endereço
SHL reg, constante
SHR reg, reg
SHR reg, endereço
SHR reg, constante
```

#### CMP
Compara dois valores e caso sejam iguais altera a _flag_ Z para `TRUE`. Essa comparação é bastante simples de ser implementada basta realizar uma subtração de bit caso o resultado seja 0 os valores são iguais. 
```
CMP reg, reg
CMP reg, endereço
CMP reg, constante
```

#### JMP
Realiza um salto incondicional do IP para o endereço especificado . Seu operando deve ser um endereço na memória. Essa instrução é bastante útil para a implementação de estruturas condicionais do tipo _if-then-else_. Este simulador também permite instruções de saltos condicionais.
```
JMP endereço
```

#### CALL
Realiza um salto para uma subrotina (função). Antes de realizar o salto armazena o endereço do IP na pilha para que não seja perdido.
```
CALL endereço
```

#### RET
Sai de uma subrotina e retorna o endereço da próxima instrução armazenado na pilha pela instrução CALL. Para que o endereço seja o mesmo a pilha deve ser balançada. Essa instrução não necessita de nunhum operando. Geralmente essa é a ultima instrução contida em uma subrotina.
```
RET
```
#### PUSH e POP
São instruções que manipulam a pilha. `PUSH` armazena um valor no topo da pilha e decrementa o ponteiro de pilha (SP). Já a instrução `POP` retorna o valor do topo para um registrador especificado incrementando o SP.
```
PUSH reg
PUSH endereço
PUSH constante
POP reg
```

#### HLT
Finaliza todas as operações do processador. Não necessita de nenhum operando.
```
HLT
```

# Exemplos

Traduzindo operações aritméticas simples para o _assembly_ do simulador.
```
A = B + C + D + E
```
![exemplo1](https://user-images.githubusercontent.com/52214785/109292729-97197980-7809-11eb-8890-899961b270d9.png)
```
A = B + C - (D - E)
```
![exemplo2](https://user-images.githubusercontent.com/52214785/109292762-a3053b80-7809-11eb-8163-9526ac175c6f.png)

Implementando estrutura condicional _if-then-else_.
```
if (i == j){
    h = i + j 
}else{
    h = i - j
}
```

![exemplo3](https://user-images.githubusercontent.com/52214785/109292810-b31d1b00-7809-11eb-8bac-9697de08dabd.png)

Implementando a estrutura de repetição `for`.
```
for(i=0; i<10; i++){
}
```

![exemplo4](https://user-images.githubusercontent.com/52214785/109292841-be704680-7809-11eb-8be9-3d5ea470965f.png)

Programa que calcula a sequência _fibonacci_

![exemplo6](https://user-images.githubusercontent.com/52214785/109311457-9e9a4c00-7824-11eb-9cf7-d93e0e27cca2.png)


Programa que identifica se determinado valor é par ou ímpar. Neste exemplo transformei o registrador D em uma _flag_, se seu valor for 1, indica a presença de um número par no resgistrador A, se seu valor for 0, indica a presença de um número ímpar no registrador A.

![exemplo5](https://user-images.githubusercontent.com/52214785/109306967-b66ed180-781e-11eb-8c7a-90abc9475d66.png)


# Referências

[Tutorial 8-bit assembler simulator (em inglês)](https://gist.github.com/MegaLoler/5ffe47668b5271faed0a3626ed5949b1)

[Tutorial simulador com javascript (em inglês)](https://www.mschweighauser.com/make-your-own-assembler-simulator-in-javascript-part1/)

[Conjunto de instruções do simulador (em inglês)](https://schweigi.github.io/assembler-simulator/instruction-set.html)

[Arquitetura de computadores: A visão do software](https://memoria.ifrn.edu.br/bitstream/handle/1044/982/Arquitetura%20de%20Computadores%20-%20Ebook.pdf)







