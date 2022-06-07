
## AWK

A linguagem `AWK` foi implementada por **A**ho, **K**ernighan e **W**einberger em 1977 com o objetivo de ampliar as potencialidades dos comandos `grep` e `sed` e permitir o tratamento de dados numéricos. Inicialmente desenvolvida para a criação de programas curtos, foi expandida nos anos posteriores para acomodar a demanda de programadores. Dessa forma, **AWK** evoluiu para uma linguagem de programação com diversas funcionalidades, tais como, funções definidas pelo usuário, expressões regulares dinãmicas, funções para busca e substituição de padrões, funções nativas para manipulação de dados, dentre outras.

Assim como **SED**, **AWK** assenta-se sobre o paradigma de programação _data driven_, no qual as instruções descrevem tanto o padrão de busca quanto o processamento desejado pelo usuário. Dessa forma, os dados serão lidos linha-a-linha e a correspondência de quais linhas serão processadas se dará pelo número da linha ou pela correspondência de um padrão de busca. Tanto **AWK** quanto **SED** dependem do conhecimento de **expressões regulares** para determinar padrões de busca no texto sobre os quais alguma ação será realizada (iremos abordar as expressões regulares na próxima atividade). Esse padrão de comportamento é conhecido como _**pattern action**_.

Uma das maiores virtudes de `AWK` é permitir ao usuário manipular arquivos de texto através de uma linguagem concisa e relativamente simples. Dessa forma, `AWK` pode ser empregada em uma miríade de tarefas computacionais e de manipulação de dados, em especial, dados tabulares (organizados em colunas). Essa inclusive é uma das vantagens de **AWK** em relação à **SED**, a simplicidade para processar e manipular dados contidos em colunas. Outras vantagens incluem, a disponibilidade de variáveis definidas pelo usuário, presença de estruturas de controle de fluxo (IF, WHILE, FOR, etc.), impressão de dados flexível (determinada pelo usuário), além de funções nativas para manipulação de _strings_ e cálculo numérico.

Através da linguagem **AWK** você será capaz de criar programas muito úteis, usando apenas uma ou duas linhas. Dentre os usos típicos estão o processamento de arquivos de texto, a criação de relatórios ou sumários de dados, realização de operações envolvendo _strings_ e cálculos aritméticos. Obs: Assim como **SED**, é possível escrever longos programas usando **AWK**, entretanto, para tarefas complexas é recomendado desenvolver programas em outras linguagens, tais como, **Perl** e **Python**.

### Sintaxe

**AWK** segue o paradigma _*pattern action*_, logo, qualquer programa escrito usando esta linguagem irá realizar uma busca por uma expressão regular e uma ação. 

```bash
# pattern {action}
$ awk '<REGEXP> { action }'

```
> Toda instrução em **AWK** necessita ter um padrão e uma ação. Quando um padrão ou uma ação não são definidos pelo usuário, **AWK** aplica as regras pré-estabelecidas, por exemplo, na falta de um padrão de busca, a ação é aplicada a todas as linhas e na ausência de uma ação especificada pelo usuário, o programa irá imprimir a linha atual.

Você pode definir as instruções do programa de duas formas: 

1) Através de um script acessório usando a opção `-f`

```bash
$ awk -f script.awk <FILE>

```

2) Ou especificando o padrão de busca e ação diretamente no prompt de comando, neste caso, as instruções devem estar contidas em aspas simples:
 
```bash
$ awk 'pattern {action}' <FILE>

```

> Observe que as instruções para processamento dos dados sempre estão contidas dentro de chaves, esse é o modo pelo qual diferenciamos as instruções de busca das instruções de processamento.

É possível estabelecer vários conjuntos de busca e processamento:

```bash
# Leitura de arquivo de texto
$ awk 'pattern1 {action1} pattern2 {action2}' <FILE>

```

> IMPORTANTE: **AWK** é capaz de ler e processar arquivos de texto, assim como dados provenientes do _STDIN_ e de redirecionamento. Nas três opções, a definição da fonte de dados ocorre fora das aspas simples.

```bash
# Leitura de arquivo de texto
$ awk 'pattern {action}' <FILE>

# Redirecionamento através do pipe ou STDIN
$ <Comando_1> | awk 'pattern {action}' 

```
### Blocos de código de AWK

Um código em **AWK** pode apresentar três partes operacionais:

1. `BEGIN` 

Trecho onde são descritas todas as ações que serão executadas no início do programa, antes da leitura dos dados. É executado apenas uma vez e pode ser utilizado para inicializar variáveis. Esse bloco é definido pela presença da expressão `BEGIN` e é opcional.

```bash
$ awk 'BEGIN {action}' <FILE>

```

2. Corpo (_body_) 
 
Trecho do código onde serão processados os dados provenientes do(s) arquivos(s) de entrada. As condições ou padrões de busca, assim como as ações, serão aplicadas a cada uma das linhas dos dados de entrada. Esse ploco não necessita de expressões especiais para defini-lo.

```bash
$ awk '/REGEXP/ {action}' <FILE>

$ awk 'Condition {action}' <FILE>

```

3. `END`
 
Trecho onde as instruções são executadas após o processamento dos dados, ao final da execução do programa. Esse bloco é definido pela expressão `END` e é opcional.

```bash
$ awk 'END {action}' <FILE>

```
Um programa contendo as três partes é escrito da seguinte maneira:

```bash
$ awk 'BEGIN {action1} <condition> {action2} END {action3}' <FILE>

```

### Campos (ou colunas)

Cada vez que **AWK** processa uma linha de texto, ele decompõe esta em diferentes campos (ou colunas). Por padrão, espaços em branco - espaços comuns ou tabulações - constituem uma separação entre as colunas. Entretanto, se você desejar usar outros tipos de delimitadores, isso é possível através da opção `-F`, por exemplo:

```bash
# Separa as colunas por tabulações (TAB)
$ awk -F"\t" '<condition> {action}' <FILE>

# Separa as colunas por vírgulas
$ awk -F"," '<condition> {action}' <FILE>

# Separa as colunas por dois pontos
$ awk -F":" '<condition> {action}' <FILE>

# Separa as colunas por ponto e vírgula
$ awk -F";" '<condition> {action}' <FILE>
```

> É possível usar diferentes caracteres e strings como delimitadores de camplo, inclusive, expressões regulares.

Você pode acessar os diferentes campos separados por **AWK** usando um `$` seguido pelo número da coluna, por exemplo, $1, $2, $3, … , $N. O valor especial $0 denota a linha inteira, não apenas um de seus campos. (Observe que se você der a ação print sem nenhum argumento, isso também imprimirá a linha inteira.) A variável `NF` indica o número de colunas e pode ser usada em conjunto com `$`, onde `$NF` denota a última coluna.

Suponha que seja do nosso interesse ler o arquivo `chars.txt` e imprimir a segunda e a última coluna. Como faríamos?

```bash
$ awk -F"," '{print $2, $4}' chars.txt

# Outra maneira usando NF
$ awk -F"," '{print $2, $NF}' chars.txt

```

A instrução `print` imprime o valor das variáveis para o `STDOUT` e você pode redirecionar o output para um arquivo através do `>`. Observe que o delimitador no output foi alterado e não mais corresponde à vírgulas, os campos agora são separados pelo separador do campo de saída (_Output Field Separator_) que, por padrão, é um espaço único. Entretanto, você pode definir o delimitador do output usando a variável OFS. Esta variável pode ser definida no bloco `BEGIN` ou após o script, veja abaixo:

```bash
$ awk -F"," 'BEGIN {OFS=";"} {print $2, $4}' chars.txt

# Outra maneira
$ awk -F"," '{print $2, $4}' OFS=";" chars.txt

```

Repare que há uma vírgula entre os argumentos da impressão `print $2, $4`, é isso o que determina os campos de saída, em outras palavras, quais colunas o outupt terá. Se você usar um espaço, e não uma vírgula, não haverá nada impresso entre as duas colunas do output e as strings serão impressas juntas. Além dos campos determinados pelas variáveis `$`, também é possível imprimir variáveis definidas pelo usuário, strings e números. 

```bash

$ awk -F"," ' BEGIN { OFS=";"; print "Char1","Char2"} {print $2, $4}' chars.txt

$ awk -F"," ' BEGIN {OFS="\t"; print "Char1","Char2"} {print $2, $4, "Número de campos=", NF}' chars.txt

$ awk -F"," ' BEGIN {OFS="\t"; V=2; print "Char1","Char2"} {print $2, $4, NF-V}' chars.txt

```

#### Resumo das variáveis associadas a colunas

* **$0** Imprime toda a linha
* **$1, $2, ..., $N** Imprime as colunas selecionadas
* **FS (_Field Separator_)** Indica o delimitador das colunas no arquivo de entrada
* **OFS (_Output Field Separator_)** Indica o delimitador das colunas no arquivo de saída
* **NF** Indica o número de campos presentes na linha atual


### Linhas (ou registros)

Assim como ocorre com os campos, existem variáveis específicas para registrar o número das linhas e é possível alterar a forma como **AWK** reconhece cada registro (ou linha). Por padrão, a quebra de linha é demarcada pelo caracter de escape `\n`, entretanto este delimitador pode ser alterado através da variável de separador de registro `RS` (_Record Separator_). O novo separador de linha pode assumir qualquer valor, inclusive fazer uso de expressões regulares e deve ser determinado no bloco `BEGIN`.

Outras variáveis nativas envolvendo a contagem de registros são:

* **NR** Variável que indica o número da linha sendo processada
* **FNR** Variável que indica o número da linha sendo processada no arquivo atual (para os casos onde houver mais de um arquivo de input)
* **ORS** Variável que determina o tipo de delimitador para cada registro, por padrão assume o valor do caracter de escape `/n`.

Se desejássemos imprimir o número de cada uma das linhas, bastaria incluir a variável `NR`, como no exemplo abaixo:

```bash
$ awk -F"," '{print NR, $2, $4}' chars.txt

```

***

**Exercício 1**

1. Execute os comandos abaixo e descreva os resultados obtidos. Há alguma diferença?

```bash

$ awk '{print}' ~/unix_lesson/other/Mov10_rnaseq_metadata.txt

$ awk '{print $0}' ~/unix_lesson/other/Mov10_rnaseq_metadata.txt

$ awk '{print $1, $2}' ~/unix_lesson/other/Mov10_rnaseq_metadata.txt

```
2. Para o mesmo arquivo dos exemplos acima, imprima o número da linha na primeira coluna e o número de campos na última coluna.

3. Agora, para o mesmo arquivo, defina `;` como separador de campos de saída (**OFS**).

***


## Filtrando dados 

Quando você deseja testar se uma linha será processada por um bloco de ação ou não, você dispõe de várias opções. Por exemplo:

1. Corresponder a uma expressão regular em qualquer lugar da linha:

```bash
$ awk '/TNF/ {print $0}' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

```

2. Corresponder a uma expressão regular em uma coluna específica:

```bash
$ awk '$10 ~ /TLR/ {print $0}' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

```

3. Usar o operador de igualdade `==` para encontrar uma correspondência exata de uma string ou número:

```bash
$ awk '$3 == "start_codon" {print $0}' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

```

4. Usar operadores de comparação, tais como, `<`, `<=`, `>`, `>=` e `!=` para comparar uma string (ordem alfabética) ou número:

```bash
$ awk '$2 > 17111983 {print $0}' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

```

5. Testar o valor de uma varável definida pelo usuário: 

```bash
$ awk 'BEGIN {gene_id="SSU72"} $10 ~ gene_id {print gene_id, $0}' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

```

6. Testar o valor de uma variável interna:

```bash
$ awk 'NR > 5000 {print NR, $0}' unix_lesson/reference_data/chr1-hg19_genes.gtf

```

A execução ou não da ação ocorre de acordo com o resultado do teste. Caso o teste seja positivo (retorne TRUE), a instrução posterior é executada, caso o resultado seja negativo (retorne FALSE), nada acontece. Um ou mais testes podem ser agrupados através de expressões booleanas, tais como, `||` que representa **ou**/**or**, `&&` que significa **e**/**and** e `!` que representa uma negação, **não**/**not**.

Vamos aplicar uma série de testes de comparação em um mesmo programa:

```bash
$ awk 'NR > 5000 && NR < 5050 && ($3 == "start_codon" || $10 != "\"UBE4B\";") {print NR, $0}' unix_lesson/reference_data/chr1-hg19_genes.gtf

```
O programa acima filtra todas as linhas de número maior que 5000 **E** menor que 5050 **E** que o terceiro campo contenha a palavra **start_codon** ou que o décimo campo seja diferente de **"UBE4B";**. Note algumas coisas importantes no comando acima: i) a terceira e a quarta expressão encontram-se entre parênteses - isso significa que elas formam um grupo que será avaliado em conjunto - caso não estivessem entre parênteses, o resultado retornado seria diferente porque a última expressão é um **OU DIFERENTE DE "UBE4B";**, que no caso, poderia corresponder a qualquer gene no arquivo que não  _UBE4B_. Ao usarmos os parênteses, asseguramos que a terceira e quarta expressão só serão avaliadas caso as duas primeiras sejam verdadeiras. A correta utilização de parênteses para separar expressões é importante não só por tornar o conjunto de expressões mais legível, mas também por alterar a forma como os testes são avaliados. ii) note que utilizamos `\` uma barra invertida antes das aspas dupla na string "UBE4B";, esse recurso é necessário para diferenciar o caracter de delimitação de strings usado pelo **AWK** de uma aspas dupla literal.


***

**Exercício 2**

1. Os genes codificantes para receptores olfativos, em geral, são iniciados pelas letras `OR`. Quantos códons de parada (_stop_codons_) para genes de receptores olfativos existem no arquivo `unix_lesson/reference_data/chr1-hg19_genes.gtf`?

2. Quantos exons possui o gene _TLR5_?

***


## Executando ações

**AWK** é uma linguagem de programação completa e dispõe de muitos recursos para a escrita de códigos concisos e muito úteis. Diferentes instruções podem ser definidas em linhas separadas, especialmente quando escrevemos códigos em arquivos, ou então, podem ser separadas através do uso de `;`.

A atribuição de variáveis é feita com o sinal `=`. Ao contrário do `BASH`, você pode ter espaços ao redor do `=`. Um aspecto interessante do **AWK** é que as variáveis não necessitam ser definidas quanto ao tipo, por exemplo, string ou numérico. Isso significa que você não precisa dizer ao **AWK** antecipadamente se uma variável vai conter um número, ou uma string, etc. Tudo é armazenado como uma string, mas quando usado em um contexto numérico, se fizer sentido, a variável será tratada como um número. Assim, você não precisa se preocupar muito com a definição do tipo de variável.

Se um valor ainda não tiver sido atribuído a uma variável, mas for usado em um contexto numérico, o valor será considerado 0; se usado em um contexto de string, seu valor é considerado a string vazia "".

*** Estruturas de controle - FOR

As estruturas de fluxo estão presentes em **AWK** e possuem uma sintaxe bastante similar à da linguagem C. Por exemplo, a estruturação de iteração `FOR` apresenta a seguinte sintaxe:

```bash
 for(initialization; condition; increment/decrement)

```

Por exemplo, vamos representar a iteração de uma seqência de números de 1 a 10, incrementados um-a-um:

```bash
 for(i=1;i<=10;i++)

```

Observe que o ++ significa adicionar um à variável à minha esquerda, no caso, a variável `i`.

Você também pode incrementar ou diminuir em quantidades maiores. O que você acha que isso faria?

```bash
$ awk '{for(i=5; i<=25; i+=5) print i}' 

```

*** Estruturas de controle - IF e ELSE

Abaixo é demonstrado o uso de IF e ELSE e como eles podem ser colocados juntos para fazer uma construção do tipo IF-ELSE. Ao mesmo tempo, será demonstrado como os resultados podem ser redirecionados para arquivos dentro do próprio script. Para tal, é utilizada praticamente a mesma sintaxe que a do Shell. Imagine que queremos imprimir cada um dos tipos de elementos gênicos em um arquivo diferente. Isso poderia ser feito da seguinte maneira:

```bash

awk '{ if($3 == "CDS") {
      print $0 > "CDS.txt"
     } else if($3 == "exon") {
      print $0 > "exon.txt"
     } else if($3 == "start_codon") {
      print $0 > "start_codon.txt"
     } else {
      print $0 > "stop_codon.txt"
     }
    }' unix_lesson/reference_data/chr1-hg19_genes.gtf

```

## Funções

**Operadores aritméticos:**
|      Operador    |            Descrição          |
|:----------------:|:-----------------------------:|
|         +        |             Adição            |
|         -        |            Subtração          |
|         /        |             Divisão           |
|         *        |          Multiplicação        |
|     **   ou ^    |          Exponenciação        |
|         +x       |     Converte para numérico    |
|                  |                               |
|         =        |     Atribuição (variáveis)    |

**Funções matemáticas**
|     Função    |            Descrição           |
|:-------------:|:------------------------------:|
|       log     |               Log              |
|      rand     |     Número aleatório (0, 1)    |
|      sqrt     |          Raiz-quadrada         |
|       int     |             Integer            |


Na tabela abaixo são mostradas algumas das funções nativas de **AWK** para manipulação de strings:

|      Função    |                         Sintaxe                       |                    Descrição                  |
|:--------------:|:-----------------------------------------------------:|:---------------------------------------------:|
|      printf    |            printf(format,   expression1, …)           |          Imprime a string   formatada         |
|       gsub     |         gsub(regexp, replacement [,   target])        |              Substitui substrings             |
|      index     |                     index(in, find)                   |           Retorna a posição do match          |
|      length    |                    length([string])                   |              Número de caracteres             |
|      split     |     split(string, array [, fieldsep [,   seps ] ])    |     Divide a string em   partes, separador    |
|     sprintf    |            sprintf(format,   expression1, …)          |                Formata a string               |
|      substr    |           substr(string,   start [, length ])         |      Divide a string em   partes, posição     |
|     tolower    |                     tolower(string)                   |                Todas minúsculas               |
|     toupper    |                     toupper(string)                   |                Todas maiúsculas               |





