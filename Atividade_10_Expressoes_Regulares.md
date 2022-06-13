## Expressões Regulares

**Expressões Regulares** contituem uma ferramenta indispensável para programadores de todos os níveis. Estão presentes na resolução de diversos problemas envolvendo o reconhecimento de padrões e a extração de informações de diferentes arquivos de textos, desde de dados brutos até documentos. Ainda que a princípio os símbolos e regras associados às expressões regulares possam parecer indecifráveis, teremos a oportunidade de ao longo desta atividade aprender e praticar o uso destes caracteres tão especiais.

> Expressões regulares também são conhecidas pelas abreviações `regex` ou `regexp`.

Um dos aspectos essenciais para entender expressões regulares é que **tudo em um texto é representado por caracteres** e escrevemos padrões de busca justamente para coincidir com sequências específicas de caracteres (**_strings_**). Os padrões de busca podem corresponder a vários símbolos encontrados no teclado, tais como, letras, números, pontuação, símbolos especiais (%$#@!&), e também símbolos de outros sistemas de escrita (não latino).

Ao longo do aprendizado sobre expressões regulares, você perceberá que várias linhas de código podem ser substituídas por uma única linha usando expressões regulares em conjunto com os comandos `sed`, `grep` ou `awk`. As expressões regulares podem ainda ser utilizadas em várias linguagens de programação, sendo que a linguagem `Perl` se destaca pelo suporte e ambrangência de recursos para o reconhecimento de padrões e edição de arquivos.

> Ainda que os conceitos básicos das expressões regulares sejam compartilhados por diversas ferramentas e linguagens de programação, a representação das expressões regulares e a abrangência das funcionalidades pode variar entre elas. Busque consultar as informações específicas para a linguagem ou comando de interesse.

Uma expressão regular é um método formal de especificar um padrão de texto que é comparado com uma cadeia de caracteres da esquerda para a direita. Quando há coincidência entre o padrão de busca e uma sequência de caracteres (strings), podemos dizer que houve um _match_ ou uma correspondência entre a busca e um trecho do texto. As expressões regulares podem variar em complexidade, entretanto, mesmo o conhecimento básico da sintaxe pode auxiliar de maneira significativa a construção de scripts e a generalização de funções.

### Combinações básicas

Uma expressão regular é apenas um padrão de caracteres que usamos para fazer busca em um texto. Por exemplo, a expressão regular `exon` significa: a letra `e`, seguida da letra `x`, seguida da letra `o` e, finalmente, pela letra `n`.


```bash
$ grep exon ~/unix_lesson/reference_data/chr1-hg19_genes.gtf

```

<pre>
chr1    unknown <a href=red><strong>exon</strong></a>    35277   35481   .       -       .       gene_id "FAM138A"; gene_name "FAM138A"; transcript_id "NR_026818"; tss_id "TSS8099";
chr1    unknown <a href=red><strong>exon</strong></a>     35721   36081   .       -       .       gene_id "FAM138F"; gene_name "FAM138F"; transcript_id "NR_026820"; tss_id "TSS8099";
chr1    unknown <a href=red><strong>exon</strong></a>     35721   36081   .       -       .       gene_id "FAM138A"; gene_name "FAM138A"; transcript_id "NR_026818"; tss_id "TSS8099";
chr1    unknown CDS     69091   70005   .       +       0       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown <a href=red><strong>exon</strong></a>     69091   70008   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown start_codon     69091   69093   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown stop_codon      70006   70008   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
</pre>

Cada caractere da expressão regular é comparado a cada caractere da string de entrada, um após o outro. Expressões regulares são normalmente _case-sensitive_ (diferenciam maiúsculas de minúsculas), logo a expressão regular `Exon` não corresponderá à string `exon`.

```bash
$ grep Exon ~/unix_lesson/reference_data/chr1-hg19_genes.gtf

```

<pre>
chr1    unknown exon    35277   35481   .       -       .       gene_id "FAM138A"; gene_name "FAM138A"; transcript_id "NR_026818"; tss_id "TSS8099";
chr1    unknown exon     35721   36081   .       -       .       gene_id "FAM138F"; gene_name "FAM138F"; transcript_id "NR_026820"; tss_id "TSS8099";
chr1    unknown exon     35721   36081   .       -       .       gene_id "FAM138A"; gene_name "FAM138A"; transcript_id "NR_026818"; tss_id "TSS8099";
chr1    unknown CDS     69091   70005   .       +       0       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown exon     69091   70008   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown start_codon     69091   69093   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown stop_codon      70006   70008   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
</pre>

## Metacaracteres

**Metacaracteres** são caracteres com funções especiais, usadados para representar posições e padrões. Em geral, são utilizados para representar conceitas e forma não passíveis de representação com caracteres normais. Para diferenciá-los de caracteres normais, usamos a barra invertida `\` ou escape. Cada metacaractere apresenta uma função específica capaz de representar um padrão de busca textual. Assim como o caracateres podem ser agrupados em um padrão de busca para formar uma string (ou mesmo uma palavra), os metacaracteres também podem ser agrupados com outros caracteres ou metacaracteres para representar padrões mais complexos de busca textual, nessa situação, recebem o nome de **expressão**. O conhecimento da função de cada metacaracter é importante para interpretar corretamente as expresões regulares e utiliz´z-las de forma efetiva.

Existem diferentes classes de metacaracteres e aprenderemos sobre cada uma delas a seguir.

### Metacaracteres Coringas

|     Metacaracter    |       Nome     |                   Coringas                  |
|:-------------------:|:--------------:|:-------------------------------------------:|
|           .         |      Ponto     |     Casa com qualquer caractere             |
|          .*         |     Coringa    |     Casa com qualquer coisa, tudo e nada    |

#### `.` Ponto

O ponto final `.` corresponde a qualquer caractere sozinho. Ele não se iguala à quebra de linha. Por exemplo, a expressão regular `.xon` significa: qualquer caractere, seguido da letra `x`, seguida da letra `o` e, por fim, da letra `n`.

```bash
$ grep .xon ~/unix_lesson/reference_data/chr1-hg19_genes.gtf

```

<pre>
chr1    unknown <a href=red><strong>exon</strong></a>    35277   35481   .       -       .       gene_id "FAM138A"; gene_name "FAM138A"; transcript_id "NR_026818"; tss_id "TSS8099";
chr1    unknown <a href=red><strong>exon</strong></a>     35721   36081   .       -       .       gene_id "FAM138F"; gene_name "FAM138F"; transcript_id "NR_026820"; tss_id "TSS8099";
chr1    unknown <a href=red><strong>exon</strong></a>     35721   36081   .       -       .       gene_id "FAM138A"; gene_name "FAM138A"; transcript_id "NR_026818"; tss_id "TSS8099";
chr1    unknown CDS     69091   70005   .       +       0       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown <a href=red><strong>exon</strong></a>     69091   70008   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown start_codon     69091   69093   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown stop_codon      70006   70008   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
</pre>

#### `.*` Ponto e asterisco

O símbolo `*` pode ser usado junto ao metacaractere `.` para encontrar qualquer string de caracteres `.*`. Quando essa expressão é utilizada em conjunto com outras expressões, significa que aceita quaisquer caracteres aleém do padrão buscado, inclusive a ausência. Por exemplo, `s.*_codon` poderia coincidir tanto com `start_codon` quanto `stop_codon`, além da string `s_codon`, caso ela existisse no arquivo `chr1-hg19_genes.gtf`. É considerado um padrão "tudo" ou "nada".

```bash
$ grep s.*_codon ~/unix_lesson/reference_data/chr1-hg19_genes.gtf

```

<pre>
chr1    unknown exon    35277   35481   .       -       .       gene_id "FAM138A"; gene_name "FAM138A"; transcript_id "NR_026818"; tss_id "TSS8099";
chr1    unknown exon     35721   36081   .       -       .       gene_id "FAM138F"; gene_name "FAM138F"; transcript_id "NR_026820"; tss_id "TSS8099";
chr1    unknown exon     35721   36081   .       -       .       gene_id "FAM138A"; gene_name "FAM138A"; transcript_id "NR_026818"; tss_id "TSS8099";
chr1    unknown CDS     69091   70005   .       +       0       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown exon     69091   70008   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown <a href=red><strong>start_codon</strong></a>     69091   69093   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown <a href=red><strong>stop_codon</strong></a>      70006   70008   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
</pre>

### Metacaracteres de Lista

|      Metacaracter     |         Nome        |                        Posicionamento                       |
|:---------------------:|:-------------------:|:-----------------------------------------------------------:|
|          [abc]        |         Lista       |     Casa com as letras “a”, “b” ou “c”                      |
|          [a-d]        |         Lista       |     Casa com as letras “a”, “b”, “c” ou “d”                 |
|         [^abc]        |     Lista Negada    |     Casa com qualquer caractere, exceto   “a”, “b” e “c”    |
|     (esse\|aquele)    |          Ou         |     Casa as strings   “esse” ou “aquele”                    |


Conjuntos de caracteres também são chamados de listas. Utilizamos colchetes para especificar conjuntos de caracteres. Use um hífen dentro de um conjunto de caracteres para especificar o intervalo de caracteres. A ordem dos caracteres dentro dos colchetes não faz diferença. Por exemplo, a expressão regular `[Ee]xon` significa: um caractere maiúsculo `E` ou minúsculo `e`, seguido das letra `x`, `o` e `n`.

```bash
$ grep [Ee]xon ~/unix_lesson/reference_data/chr1-hg19_genes.gtf | head

```

<pre>
chr1    unknown <a href=red><strong>exon</strong></a>    35277   35481   .       -       .       gene_id "FAM138A"; gene_name "FAM138A"; transcript_id "NR_026818"; tss_id "TSS8099";
chr1    unknown <a href=red><strong>exon</strong></a>     35721   36081   .       -       .       gene_id "FAM138F"; gene_name "FAM138F"; transcript_id "NR_026820"; tss_id "TSS8099";
chr1    unknown <a href=red><strong>exon</strong></a>     35721   36081   .       -       .       gene_id "FAM138A"; gene_name "FAM138A"; transcript_id "NR_026818"; tss_id "TSS8099";
chr1    unknown CDS     69091   70005   .       +       0       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown <a href=red><strong>exon</strong></a>     69091   70008   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown start_codon     69091   69093   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown stop_codon      70006   70008   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
</pre>

No entanto, um ponto final dentro dos colchetes, significa apenas um ponto final. A expressão regular `exon[.]` significa: a string `exon`, seguida pelo caractere de ponto final `.`.

```bash
$ grep exon[.] ~/unix_lesson/reference_data/chr1-hg19_genes.gtf | head

```

<pre>
chr1    unknown exon    35277   35481   .       -       .       gene_id "FAM138A"; gene_name "FAM138A"; transcript_id "NR_026818"; tss_id "TSS8099";
chr1    unknown exon     35721   36081   .       -       .       gene_id "FAM138F"; gene_name "FAM138F"; transcript_id "NR_026820"; tss_id "TSS8099";
chr1    unknown exon     35721   36081   .       -       .       gene_id "FAM138A"; gene_name "FAM138A"; transcript_id "NR_026818"; tss_id "TSS8099";
chr1    unknown CDS     69091   70005   .       +       0       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown exon     69091   70008   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown start_codon     69091   69093   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown stop_codon      70006   70008   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
</pre>

#### Lista Negada

Quando dentro de colchetes, o símbolo do circunflexo representa a negação do conjunto de caracteres. Por exemplo, a expressão regular `[^ACGT]` significa: qualquer caractere com exceção de `A`, `C`, `G` e `T`. 

> A seguir você aprenderá que o circunflexo `^` também pode cumprir a função de `âncora` quando fora dos colchetes. Fique atento para o contexto de uso dos metacaracteres, pois a função destes pode variar dependendo da forma de uso.

> É possível buscar pelo contrário do padrão especificado usando a opção `-v` para `grep` ou então a `!` antes do padrão em `awk`: `!/REGEXP/`. 

```bash
$ grep "^[^aeiou]" /etc/passwd

```

<pre>
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
(...)
</pre>

### Metacaracteres de repetição

Os metacaracteres `+`, `*` ou `?` são utilizados para especificar quantas vezes um sub-padrão pode ocorrer. 

|     Metacaracter    |        Nome      |                    Posicionamento                  |
|:-------------------:|:----------------:|:--------------------------------------------------:|
|         a{2}        |       Chaves     |     Casa com a letra “a” duas vezes.               |
|        a{2,4}       |       Chaves     |     Casa   com a letra “a” duas a quatro vezes.    |
|         a{2,}       |       Chaves     |     Casa   com a letra “a” duas ou mais vezes.     |
|          a?         |      Opcional    |     Casa   com a letra “a” zero ou uma vez.        |
|          a*         |     Asterisco    |     Casa   com a letra “a” zero ou mais vezes.     |
|          a+         |        Mais      |     Casa   com a letra “a” uma ou mais vezes.      |


#### As chaves `{}`

Em expressões regulares, chaves, que também são chamadas de quantificadores, são utilizadas para especificar o número de vezes que o caractere, ou um grupo de caracteres, pode se repetir. Por exemplo, a expressão regular `[0-9]{2,3}` significa: Encontre no mínimo 2 dígitos, mas não mais que 3 (caracteres no intervalo de 0 a 9). 

```bash
$ egrep 'O{1,3}' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

```
<pre>
chr1    unknown CDS     69091   70005   .       +       0       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown exon    69091   70008   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown start_codon     69091   69093   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown stop_codon      70006   70008   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown exon    136698  136756  .       -       .       gene_id "LOC729737"; gene_name "LOC729737"; transcript_id "NR_039983"; tss_id "TSS17868";
chr1    unknown exon    136806  136854  .       -       .       gene_id "LOC729737"; gene_name "LOC729737"; transcript_id "NR_039983"; tss_id "TSS17868";
chr1    unknown exon    136953  139696  .       -       .       gene_id "LOC729737"; gene_name "LOC729737"; transcript_id "NR_039983"; tss_id "TSS17868";
chr1    unknown exon    139790  139847  .       -       .       gene_id "LOC729737"; gene_name "LOC729737"; transcript_id "NR_039983"; tss_id "TSS17868";
chr1    unknown exon    140075  140566  .       -       .       gene_id "LOC729737"; gene_name "LOC729737"; transcript_id "NR_039983"; tss_id "TSS17868";
chr1    unknown exon    323892  324060  .       +       .       gene_id "LOC100132287"; gene_name "LOC100132287"; transcript_id "NR_028322"; tss_id "TSS11868";
</pre>

Nós podemos retirar o segundo número. Por exemplo, a expressão regular `[0-9]{2,}` significa: Encontre 2 ou mais dígitos. Se removermos a vírgula a expressão regular `[0-9]{3}` significa: Encontre exatamente 3 dígitos.

```bash
$ egrep 'O{2,}' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

```

<pre>
chr1    unknown exon    60280533        60280852        .       +       .       gene_id "HOOK1"; gene_name "HOOK1"; p_id "P21131"; transcript_id "NM_015888"; tss_id "TSS7831";
chr1    unknown CDS     60280790        60280852        .       +       0       gene_id "HOOK1"; gene_name "HOOK1"; p_id "P21131"; transcript_id "NM_015888"; tss_id "TSS7831";
chr1    unknown start_codon     60280790        60280792        .       +       .       gene_id "HOOK1"; gene_name "HOOK1"; p_id "P21131"; transcript_id "NM_015888"; tss_id "TSS7831";
chr1    unknown CDS     60287530        60287615        .       +       0       gene_id "HOOK1"; gene_name "HOOK1"; p_id "P21131"; transcript_id "NM_015888"; tss_id "TSS7831";
chr1    unknown exon    60287530        60287615        .       +       .       gene_id "HOOK1"; gene_name "HOOK1"; p_id "P21131"; transcript_id "NM_015888"; tss_id "TSS7831";
chr1    unknown CDS     60294452        60294524        .       +       1       gene_id "HOOK1"; gene_name "HOOK1"; p_id "P21131"; transcript_id "NM_015888"; tss_id "TSS7831";
chr1    unknown exon    60294452        60294524        .       +       .       gene_id "HOOK1"; gene_name "HOOK1"; p_id "P21131"; transcript_id "NM_015888"; tss_id "TSS7831";
chr1    unknown CDS     60297835        60297885        .       +       0       gene_id "HOOK1"; gene_name "HOOK1"; p_id "P21131"; transcript_id "NM_015888"; tss_id "TSS7831";
chr1    unknown exon    60297835        60297885        .       +       .       gene_id "HOOK1"; gene_name "HOOK1"; p_id "P21131"; transcript_id "NM_015888"; tss_id "TSS7831";
chr1    unknown CDS     60299077        60299209        .       +       0       gene_id "HOOK1"; gene_name "HOOK1"; p_id "P21131"; transcript_id "NM_015888"; tss_id "TSS7831";
</pre>

#### O Sinal de Adição `+`

O símbolo `+` corresponde a uma ou mais repetições do caractere anterior. Por exemplo, a expressão regular `e.+n` significa: a letra minúscula `e`, seguida por pelo menos um caractere, seguido do caractere minúsculo `n`.

```bash
$ awk '$10 ~ /OR.+5/' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

```

<pre>
chr1    unknown CDS     69091   70005   .       +       0       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown exon    69091   70008   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown start_codon     69091   69093   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown stop_codon      70006   70008   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown exon    12567300        12567451        .       +       .       gene_id "SNORA59B"; gene_name "SNORA59B"; transcript_id "NR_003022"; tss_id "TSS2762";
chr1    unknown exon    12567300        12567451        .       +       .       gene_id "SNORA59A"; gene_name "SNORA59A"; transcript_id "NR_003025"; tss_id "TSS2762";
chr1    unknown exon    31441010        31441084        .       -       .       gene_id "SNORD85"; gene_name "SNORD85"; transcript_id "NR_003066"; tss_id "TSS1946";
chr1    unknown exon    40033046        40033182        .       -       .       gene_id "SNORA55"; gene_name "SNORA55"; transcript_id "NR_002983"; tss_id "TSS22213";
chr1    unknown exon    45241537        45241610        .       +       .       gene_id "SNORD55"; gene_name "SNORD55"; transcript_id "NR_000015"; tss_id "TSS14959";
chr1    unknown exon    76252757        76252834        .       +       .       gene_id "SNORD45C"; gene_name "SNORD45C"; transcript_id "NR_003042"; tss_id "TSS25302";
</pre>

#### O símbolo de interrogação `?`

O metacaractere `?` faz o caractere anterior ser opcional. Esse símbolo corresponde a zero ou uma ocorrência do caractere anterior. Por exemplo, a expressão regular `[chr]?1` significa: A string `chr` opcional, seguida do caractere `1`.

```bash
$ awk '$1 ~ /[chr]?1/' unix_lesson/genomics_data/Encode-hesc-Nanog.bed | head

```
<pre>
chr11   92283662        92283923
chr16   9077536 9077828
chr15   71496031        71496283
chr12   130255896       130256146
chr14   42805182        42805428
chr1    203488575       203488821
chr11   97727752        97727972
chr11   120516471       120516755
chr16   72756525        72756788
chr14   59235755        59236078
</pre>

```bash
$ sed 's/chr//g' unix_lesson/genomics_data/Encode-hesc-Nanog.bed | awk '$1 ~ /[chr]?1/' | head

```
<pre>
11      92283662        92283923
16      9077536 9077828
15      71496031        71496283
12      130255896       130256146
14      42805182        42805428
1       203488575       203488821
11      97727752        97727972
11      120516471       120516755
16      72756525        72756788
14      59235755        59236078
</pre>


#### O Asterisco `*`

O símbolo `*` corresponde a zero ou mais repetições do padrão antecedente. A expressão regular `a*` significa: zero ou mais repetições do caractere minúsculo precedente `a`. Mas se o asterisco aparecer depois de um conjunto de caracteres, ou classe de caracteres, ele irá procurar as repetições de todo o conjunto. Por exemplo, a expressão regular `[a-z]*` significa: qualquer quantidade de letras minúsculas numa linha.

O símbolo `*` pode ser usado junto do metacaractere `.` para encontrar qualquer string de caracteres `.*`. O símbolo `*` pode ser usado com o caractere de espaço em branco `\s` para encontrar uma string de caracteres em branco. Por exemplo, a expressão `\s*exon\s*` significa: zero ou mais espaços, seguidos dos caracteres minúsculos `e`, `x`, `o` e `n`, seguido de zero ou mais espaços.

```bash
$ egrep 'LOC.*' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

```
<pre>
chr1    unknown exon    136698  136756  .       -       .       gene_id "LOC729737"; gene_name "LOC729737"; transcript_id "NR_039983"; tss_id "TSS17868";
chr1    unknown exon    136806  136854  .       -       .       gene_id "LOC729737"; gene_name "LOC729737"; transcript_id "NR_039983"; tss_id "TSS17868";
chr1    unknown exon    136953  139696  .       -       .       gene_id "LOC729737"; gene_name "LOC729737"; transcript_id "NR_039983"; tss_id "TSS17868";
chr1    unknown exon    139790  139847  .       -       .       gene_id "LOC729737"; gene_name "LOC729737"; transcript_id "NR_039983"; tss_id "TSS17868";
chr1    unknown exon    140075  140566  .       -       .       gene_id "LOC729737"; gene_name "LOC729737"; transcript_id "NR_039983"; tss_id "TSS17868";
chr1    unknown exon    323892  324060  .       +       .       gene_id "LOC100132287"; gene_name "LOC100132287"; transcript_id "NR_028322"; tss_id "TSS11868";
chr1    unknown exon    323892  324060  .       +       .       gene_id "LOC100132062"; gene_name "LOC100132062"; transcript_id "NR_028325"; tss_id "TSS11868";
chr1    unknown exon    323892  324060  .       +       .       gene_id "LOC100133331"; gene_name "LOC100133331"; transcript_id "NR_028327_1"; tss_id "TSS11868";
chr1    unknown exon    324288  324345  .       +       .       gene_id "LOC100132287"; gene_name "LOC100132287"; transcript_id "NR_028322"; tss_id "TSS11868";
chr1    unknown exon    324288  324345  .       +       .       gene_id "LOC100132062"; gene_name "LOC100132062"; transcript_id "NR_028325"; tss_id "TSS11868";
</pre>

**Exercício 1**

Teste os comandos abaixo e compare os resultados. As buscas resultam em resultados idênticos? Explique o padrão observado.

```bash
$ egrep 'OR.*' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

$ egrep 'OR*' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

$ egrep '"OR"*' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

$ awk '/OR*/' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

$ awk '/"OR"*/' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

```

***

### Metacaracteres de posição (Âncoras)

|     Metacaracter    |         Nome       |            Posicionamento           |
|:-------------------:|:------------------:|:-----------------------------------:|
|           ^         |     Circunflexo    |     Representa o início de linha    |
|           $         |        Cifrão      |      Representa o final da linha    |

Os metacaracteres de posição (ou âncoras) nos permitem verificar se o padrão encontra-se no início ou no final da linha ou campo. As âncoras podem ser de dois tipos: O primeiro tipo é o acento circunflexo `^`, que verifica se o padrão encontrado está no início da linha, e o segundo tipo é o cifrão `$`, que verifica se o padrão encontrado é o último naquela linha ou campo.

#### O Acento Circunflexo `^`

O metacaracter `^` é usado para verificar a posição do padrão, neste caso, se ele ocorre no início da linha. Se usarmos a expressão regular `^a` e `a` for o primeiro caractere na linha contendo `abc`, haverá a coincidência com `a`. Mas se aplicarmos a expressão regular `^b` na mesma string, ela não coincidirá. Isso ocorre porque, no segundo exemplo, `b` não é o caractere inicial. Vamos verificar outra expressão regular, `^(C|c)hr` que significa: o caractere maiúsculo `C` ou o caractere minúsculo `C` que encontra-se no início da linha ou campo, seguido pelos caracteres minúsculos `h` e `r`.

Teste e verifique a diferença entre os dois comandos abaixo, no qual em um deles, usamos o `^` antes de `bin`: `^bin`.

```bash
$ grep bin /etc/passwd

```
<pre>
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
</pre>

```bash
$ grep ^bin /etc/passwd

```
<pre>
bin:x:2:2:bin:/bin:/usr/sbin/nologin
</pre>

#### O Cifrão `$`

O símbolo de cifrão `$` é usado para verificar se o padrão encontrado está ao final da linha (última sequência de caracteres). Por exemplo, a expressão regular `bash$` significa: busque por todas as linhas terminadas pela string `bash`.

```bash
$ grep bash$ /etc/passwd

```
<pre>
root:x:0:0:root:/root:/bin/bash
(...)
</pre>


> O uso da expressão `^$` é muito útil para identificar linhas vazias em um arquivo de texto. Veja o exemplo abaixo:

```bash
$ grep -n '^$' unix_lesson/other/chars.txt

```
<pre>
8:
</pre>

### Formas resumidas ou alternativas de se representar classes de caracteres:

|     Classes de Caracteres    |                Definição               |
|:----------------------------:|:--------------------------------------:|
|           [:alnum:]          |         Caracteres alfanuméricos       |
|           [:alpha:]          |          Caracteres alfabéticos        |
|           [:blank:]          |           Espaços e tabulações         |
|           [:cntrl:]          |          Caracteres de controle        |
|           [:digit:]          |           Caracteres numéricos         |
|           [:graph:]          |     Caracteres impressos e visíveis    |
|           [:lower:]          |         Caracteres em minúsculas       |
|           [:print:]          |           Caracteres impressos         |
|           [:punct:]          |         Caracteres de pontuação        |
|           [:space:]          |           Caracteres de espaço         |
|           [:upper:]          |         Caracteres em maiúsculas       |
|           [:xdigit:]         |     Caracteres dígitos hexadecimais    |

As expressões regulares fornecem abreviações para conjuntos de caracteres comumente usados, que oferecem atalhos convenientes para expressões regulares comumente usadas. As abreviações são as seguintes:

|     Operadores    |                    Definição                  |
|:-----------------:|:---------------------------------------------:|
|         \s        |              Caracteres de espaço             |
|         \S        |         Caracteres que não sejam espaço       |
|         \w        |       Caracteres que constituem palavras      |
|         \W        |     Caracteres que não constituem palavras    |
|         \<        |          Coincide no início da palavra        |
|         \>        |          Coincide ao final da palavra         |
|         \y        |            Caracteres em minúsculas           |
|         \B        |              Caracteres impressos             |
|         \d        |              Caracteres numéricos             |
|         \D        |            Caracteres não numéricos           |

Observe que uma mesma classe de caracteres pode ser representada de diferentes maneiras, sendo concisão e legibilidade as principais diferenças entre eles. Entretanto, fique atento, pois nem todos os metacaracteres são aceitos em todos os comandos ou linguagens. Podem haver diferenças entre os padrões (básico e extendido), implementações (por exemplo, GNU grep e BSD Grep) e 


**Exercício 2**

Teste os comandos abaixo e compare os resultados. As buscas resultam em resultados idênticos?

```bash
$ egrep '[[:digit:]]{6,}[[:blank:]]' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

$ egrep '[0-9]{6,}[[:blank:]]' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

$ grep -P '\d{6,}\s' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

```
> A opção `-P` habilita o uso da sintaxe da linguagem Perl para expressões regulares, caso estivesse desabilitada, o comando `grep` não reconheceria o metacaracter `\d`.

***

### Grupos de Caracteres

Grupo de caracteres é um grupo de sub-padrão que é escrito dentro de parênteses `(...)`. Como mostrado anteriormente, se colocaramos um quantificador depois de um caractere, ele irá repetir o caractere anterior. Mas se colocarmos um quantificador depois de um grupo de caracteres, ele irá repetir todo o conjunto. Por exemplo, a expressão regular `(ab)*` corresponde a zero ou mais repetições dos caracteres "ab". Nós também podemos usar o metacaractere de alternância `|` dentro de um grupo de caracteres. Por exemplo, a expressão regular `(t|g|p)_id` significa: caractere minúsculo `t`, `g` ou `p`, seguido dos caracteres `_`, `i` e `d`.

No exemplo abaixo, desejamos obter todos aqueles genes cujos nomes sejam compostos por duas ou mais sequências `{2,}` de uma letra `[A-Z]` e um número `[0-9]`:

```bash
$ egrep '([A-Z][0-9]){2,}' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

```

<pre>
chr1    unknown CDS     69091   70005   .       +       0       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown exon    69091   70008   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown start_codon     69091   69093   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown stop_codon      70006   70008   .       +       .       gene_id "OR4F5"; gene_name "OR4F5"; p_id "P9488"; transcript_id "NM_001005484"; tss_id "TSS13903";
chr1    unknown CDS     367659  368594  .       +       0       gene_id "OR4F16"; gene_name "OR4F16"; p_id "P1573"; transcript_id "NM_001005277"; tss_id "TSS4765";
chr1    unknown CDS     367659  368594  .       +       0       gene_id "OR4F29"; gene_name "OR4F29"; p_id "P1573"; transcript_id "NM_001005221"; tss_id "TSS4765";
chr1    unknown CDS     367659  368594  .       +       0       gene_id "OR4F3"; gene_name "OR4F3"; p_id "P1573"; transcript_id "NM_001005224"; tss_id "TSS4765";
chr1    unknown exon    367659  368597  .       +       .       gene_id "OR4F16"; gene_name "OR4F16"; p_id "P1573"; transcript_id "NM_001005277"; tss_id "TSS4765";
chr1    unknown exon    367659  368597  .       +       .       gene_id "OR4F29"; gene_name "OR4F29"; p_id "P1573"; transcript_id "NM_001005221"; tss_id "TSS4765";
chr1    unknown exon    367659  368597  .       +       .       gene_id "OR4F3"; gene_name "OR4F3"; p_id "P1573"; transcript_id "NM_001005224"; tss_id "TSS4765";
</pre>

E se desejássemos aqueles que, além do padrão, não possuem a letra `O`?

```bash
$ egrep '[^O]([A-Z][0-9]){2,}' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

```
<pre>
chr1    unknown exon    1189292 1190867 .       -       .       gene_id "UBE2J2"; gene_name "UBE2J2"; p_id "P12331"; transcript_id "NM_058167"; tss_id "TSS24202";
chr1    unknown exon    1189292 1190867 .       -       .       gene_id "UBE2J2"; gene_name "UBE2J2"; p_id "P12898"; transcript_id "NM_194315"; tss_id "TSS24202";
chr1    unknown exon    1189292 1190867 .       -       .       gene_id "UBE2J2"; gene_name "UBE2J2"; p_id "P27115"; transcript_id "NM_194458"; tss_id "TSS24202";
chr1    unknown exon    1189292 1190867 .       -       .       gene_id "UBE2J2"; gene_name "UBE2J2"; p_id "P27115"; transcript_id "NM_194457"; tss_id "TSS24202";
chr1    unknown stop_codon      1190583 1190585 .       -       .       gene_id "UBE2J2"; gene_name "UBE2J2"; p_id "P12331"; transcript_id "NM_058167"; tss_id "TSS24202";
chr1    unknown stop_codon      1190583 1190585 .       -       .       gene_id "UBE2J2"; gene_name "UBE2J2"; p_id "P12898"; transcript_id "NM_194315"; tss_id "TSS24202";
chr1    unknown stop_codon      1190583 1190585 .       -       .       gene_id "UBE2J2"; gene_name "UBE2J2"; p_id "P27115"; transcript_id "NM_194458"; tss_id "TSS24202";
chr1    unknown stop_codon      1190583 1190585 .       -       .       gene_id "UBE2J2"; gene_name "UBE2J2"; p_id "P27115"; transcript_id "NM_194457"; tss_id "TSS24202";
chr1    unknown CDS     1190586 1190867 .       -       0       gene_id "UBE2J2"; gene_name "UBE2J2"; p_id "P12331"; transcript_id "NM_058167"; tss_id "TSS24202";
chr1    unknown CDS     1190586 1190867 .       -       0       gene_id "UBE2J2"; gene_name "UBE2J2"; p_id "P12898"; transcript_id "NM_194315"; tss_id "TSS24202";
</pre>


#### Alternância

Em expressões regulares, a barra vertical `|` é usada para definir alternância. Alternância é como uma condição entre múltiplas expressões. Dependendo do contexto, o significado de `|` pode assumir diferentes funções. Quando utilizada entre parênteses `( | )`, o símbolo `|` denota um caracter ou outro, por exemplo, na expressão `(c|f|h)at` há coincidência do padrão com as palavras `cat`, `fat` e `hat`, pois o primeiro caracter pode corresponder às letras `c`, `f` e `h`. Entretanto, se a barra `|` for utilizada fora dos parênteses, ela passa a significar um padrão ou outro, por exemplo, na expressão `gene|transcript` haverá coincidência com as linhas ou campos que possuírem as strings `gene` ou `transcript`. Já a expressão `(G|g)ene|(T|t)ranscript` coincide com as mesmas strings do exemplo anterior, independente destas iniciarem com uma letra maiúscula ou minúscula.


```bash
$ egrep 'TNF|TLR' unix_lesson/reference_data/chr1-hg19_genes.gtf | head

```
<pre>
chr1    unknown exon    1138888 1139340 .       -       .       gene_id "TNFRSF18"; gene_name "TNFRSF18"; p_id "P6423"; transcript_id "NM_148901"; tss_id "TSS27102";
chr1    unknown exon    1138888 1139348 .       -       .       gene_id "TNFRSF18"; gene_name "TNFRSF18"; p_id "P6568"; transcript_id "NM_004195"; tss_id "TSS27102";
chr1    unknown exon    1138888 1139348 .       -       .       gene_id "TNFRSF18"; gene_name "TNFRSF18"; p_id "P24339"; transcript_id "NM_148902"; tss_id "TSS27102";
chr1    unknown stop_codon      1138971 1138973 .       -       .       gene_id "TNFRSF18"; gene_name "TNFRSF18"; p_id "P6423"; transcript_id "NM_148901"; tss_id "TSS27102";
chr1    unknown CDS     1138974 1139340 .       -       1       gene_id "TNFRSF18"; gene_name "TNFRSF18"; p_id "P6423"; transcript_id "NM_148901"; tss_id "TSS27102";
chr1    unknown stop_codon      1139224 1139226 .       -       .       gene_id "TNFRSF18"; gene_name "TNFRSF18"; p_id "P6568"; transcript_id "NM_004195"; tss_id "TSS27102";
chr1    unknown stop_codon      1139224 1139226 .       -       .       gene_id "TNFRSF18"; gene_name "TNFRSF18"; p_id "P24339"; transcript_id "NM_148902"; tss_id "TSS27102";
chr1    unknown CDS     1139227 1139348 .       -       2       gene_id "TNFRSF18"; gene_name "TNFRSF18"; p_id "P6568"; transcript_id "NM_004195"; tss_id "TSS27102";
chr1    unknown CDS     1139227 1139348 .       -       2       gene_id "TNFRSF18"; gene_name "TNFRSF18"; p_id "P24339"; transcript_id "NM_148902"; tss_id "TSS27102";
chr1    unknown CDS     1139414 1139616 .       -       1       gene_id "TNFRSF18"; gene_name "TNFRSF18"; p_id "P6568"; transcript_id "NM_004195"; tss_id "TSS27102";
</pre>

```bash
$ egrep 'TNF|TLR' unix_lesson/reference_data/chr1-hg19_genes.gtf | tail

```
<pre>
chr1    unknown start_codon     173176313       173176315       .       -       .       gene_id "TNFSF4"; gene_name "TNFSF4"; p_id "P15167"; transcript_id "NM_003326"; tss_id "TSS10422";
chr1    unknown exon    223282748       223286377       .       -       .       gene_id "TLR5"; gene_name "TLR5"; p_id "P18500"; transcript_id "NM_003268"; tss_id "TSS5326";
chr1    unknown stop_codon      223283797       223283799       .       -       .       gene_id "TLR5"; gene_name "TLR5"; p_id "P18500"; transcript_id "NM_003268"; tss_id "TSS5326";
chr1    unknown CDS     223283800       223286373       .       -       0       gene_id "TLR5"; gene_name "TLR5"; p_id "P18500"; transcript_id "NM_003268"; tss_id "TSS5326";
chr1    unknown start_codon     223286371       223286373       .       -       .       gene_id "TLR5"; gene_name "TLR5"; p_id "P18500"; transcript_id "NM_003268"; tss_id "TSS5326";
chr1    unknown exon    223305817       223305981       .       -       .       gene_id "TLR5"; gene_name "TLR5"; p_id "P18500"; transcript_id "NM_003268"; tss_id "TSS5326";
chr1    unknown exon    223308024       223308206       .       -       .       gene_id "TLR5"; gene_name "TLR5"; p_id "P18500"; transcript_id "NM_003268"; tss_id "TSS5326";
chr1    unknown exon    223310520       223310605       .       -       .       gene_id "TLR5"; gene_name "TLR5"; p_id "P18500"; transcript_id "NM_003268"; tss_id "TSS5326";
chr1    unknown exon    223314990       223315105       .       -       .       gene_id "TLR5"; gene_name "TLR5"; p_id "P18500"; transcript_id "NM_003268"; tss_id "TSS5326";
chr1    unknown exon    223316538       223316624       .       -       .       gene_id "TLR5"; gene_name "TLR5"; p_id "P18500"; transcript_id "NM_003268"; tss_id "TSS5326";
</pre>


### Grupos de Captura

As expressões regulares também nos permitem extrair informações para processamento posterior. Para realizar tal ação, também utilzamos os parênteses `()` que nos permetirá capturar um subpadrão dentro de um par de parênteses. O uso de grupos de captura nos permitem processar informações que possuem uma estrutura fixa, tais como, números de telefone, endereços de e-mail, CPFs, extensões de arquivos, dentre outros. 

Caso você desejasse capturar apenas o nome base (ou prefixo) de um arquivo, tal ação seria possível através da expressão: `^([:alphanum:]+)\.pdf$`. Neste exemplo, ainda que toda a expressão fosse utilizada para a busca - excluindo os arquivos que não tivessem a extensão `.pdf`, apenas seria retornada a string contida dentro dos parênteses. Esse trecho do padrão retornado é denominado grupo de captura.

É possível capturar mais de um grupo a partir de uma mesma expressão através do uso de grupos de captura aninhados. A sequência dos resultados retornados é definida pela ordem em que os grupos de captura são definidos (do mais exterior para o mais interior). Retomando o exemplo de um nome de arquivo composto, por exemplo, por letras e depois números, seria possível determinar dois grupos de captura: `^([:alpha:]+([:digit:]+))'\.pdf$`, onde o primeiro grupo de captura retornaria o padrão contendo letras e números, exceto a extensão, e o segundo grupo de captura retornaria apenas os números contidos ao final do prefixo do nome do arquivo.

Nos exemplos a seguir, iremos utilizar a linguagem `Perl` para capturar os grupos de caracteres de interesse. As duas linhas de comando retornam o mesmo resultado, o `nome do usuário` e o `id do usuário` no servidor Darwin.

```bash
$ cat /etc/passwd | perl -n -e'/^([a-z0-9]*):x:([0-9]*).*/ && print $1, "-", $2, "\n"'

$ cat /etc/passwd | perl -n -e'/^(\w*):x:(\d*).*/ && print $1, "-", $2, "\n"'
```

> Em ambas expressões, buscamos capturar a primeira cadeia de caracteres alfanuméricos que representa o nome do usuário (username), seguida pelos caracteres `:x:`, para então recuperar a cadeira de caracteres numéricos que correspondem ao identificador do usuário (uid).



**Exercício 3**

A partir do arquivo `~/unix_lesson/reference_data/chr1-hg19_genes.gtf`, construa uma expressão regular para capturar as seguintes informações:

1. Cromossomo (1ª coluna)
2. Elemento gênico (3ª coluna)
3. Posição de início (4ª coluna)
4. Posição final (5ª coluna)
5. Nome do gene (sem aspas e ;) (10ª coluna)

***
