
## AWK

A linguagem `AWK` foi implementada por **A**ho, **K**ernighan e **W**einberger em 1977 com o objetivo de ampliar as potencialidades dos comandos `grep` e `sed` e permitir o tratamento de dados numéricos. Inicialmente desenvolvida para a criação de programas curtos, foi expandida nos anos posteriores para acomodar a demanda de programadores. Dessa forma, **AWK** evoluiu para uma linguagem de programação com diversas funcionalidades, tais como, funções definidas pelo usuário, expressões regulares dinãmicas, funções para busca e substituição de padrões, funções nativas para manipulação de dados, dentre outras.

Assim como **SED**, **AWK** assenta-se sobre o paradigma de programação _data driven_, no qual as instruções descrevem tanto o padrão de busca quanto o processamento desejado pelo usuário. Dessa forma, os dados serão lidos linha-a-linha e a correspondência de quais linhas serão processadas se dará pelo número da linha ou pela correspondência de um padrão de busca. Tanto **AWK** quanto **SED** dependem do conhecimento de **expressões regulares** para determinar padrões de busca no texto sobre os quais alguma ação será realizada (iremos abordar as expressões regulares na próxima atividade). Esse padrão de comportamento é conhecido como _**pattern action**_.

Uma das maiores virtudes de `AWK` é permitir ao usuário manipular arquivos de texto através de uma linguagem concisa e relativamente simples. Dessa forma, `AWK` pode ser empregada em uma miríade de tarefas computacionais e de manipulação de dados, em especial, dados tabulares (organizados em colunas). Essa inclusive é uma das vantagens de **AWK** em relação à **SED**, a simplicidade para processar e manipular dados contidos em colunas. Outras vantagens incluem, a disponibilidade de variáveis definidas pelo usuário, presença de estruturas de controle de fluxo (IF, WHILE, FOR, etc.), impressão de dados flexível (determinada pelo usuário), além de funções nativas para manipulação de _strings_ e cálculo numérico.

Através da linguagem **AWK** você será capaz de criar programas muito úteis, usando apenas uma ou duas linhas. Dentre os usos típicos estão o processamento de arquivos de texto, a criação de relatórios ou sumários de dados, realização de operações envolvendo _strings_ e cálculos aritméticos. Obs: Assim como **SED**, é possível escrever longos programas usando **AWK**, entretanto, para tarefas complexas é recomendado desenvolver programas em outras linguagens, tais como, **Perl** e **Python**.

### SINTAXE

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

Suponha que seja do nosso interesse ler o arquivo `chars.txt` e imprimir a segunda e a última coluna. 


