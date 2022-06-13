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


