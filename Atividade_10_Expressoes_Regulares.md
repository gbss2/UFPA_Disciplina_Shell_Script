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








### Metacaracteres de posição (Âncoras)

|     Metacaracter    |         Nome       |            Posicionamento           |
|:-------------------:|:------------------:|:-----------------------------------:|
|           ^         |     Circunflexo    |     Representa o início de linha    |
|           $         |        Cifrão      |      Representa o final da linha    |


