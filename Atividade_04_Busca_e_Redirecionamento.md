## Buscas utilizando o comando `grep`

Aprendemos como pesquisar por **_strings_** dentro de um arquivo usando `less`. Também podemos pesquisar dentro de arquivos sem sequer abri-los, usando `grep`. O comando `grep` é um utilitário do Shell para pesquisar texto simples (**_strings_**) e retornar as linhas que correspondam a um padrão ou expressão regular (**_regex_**).

> Por que a palavra "grep"? É uma forma abreviada de **g**lobally search for a **r**egular **e**xpression and **p**rint matching lines (g/re/p), ou seja, realizar uma busca global por expressões regulares e imprimir as linhas correspondentes.

A sintaxe para `grep` é a seguinte: `grep termo_de_busca nome_do_arquivo`. O padrão que queremos pesquisar é especificado no slot `termo_de_busca`, e o arquivo que queremos pesquisar é especificado no slot `nome_do_arquivo`. Vamos testar o grep realizando buscas nos arquivos FASTQ no diretório `raw_fastq`.

Os arquivos FASTQ contêm as leituras de sequenciamento (sequências de nucleotídeos) de um sequenciador. Cada **_read_** em um arquivo FASTQ está associada a quatro linhas, sendo a primeira linha (linha de cabeçalho) inciada com um símbolo `@`. Os dados correspondentes à uma _read_ no formato FASTQ tem a seguinte estrutura:

	@HWI-ST330:304:H045HADXX:1:1101:1111:61397
	CACTTGTAAGGGCAGGCCCCCTTCACCCTCCCGCTCCTGGGGGANNNNNNNNNNANNNCGAGGCCCTGGGGTAGAGGGNNNNNNNNNNNNNNGATCTTGG
	+
	B?@DDDDDDHHH?GH:?FCBGGB@C?DBEGIIIIAEF;FCGGI#########################################################
  
> **Informações sobre o formato de arquivo FASTQ**
>
> |Linha|Descrição|
> |---- |-----------|
> |1    |Identificador da _read_ precedido por '@'|
> |2    |A sequência de nucleotídeos presente na _read_|
> |3    |Apenas um sinal '+' ou o identificador da _read_ precedido por um '+'|
> |4    |Conjunto de caracteres que representam o escore de qualidade de cada nucleotídeo na linha 2; deve ter o mesmo número de caracteres que a linha 2|

Suponha que queremos ver a quantidade de _reads_ em nosso arquivo `Mov10_oe_1.subset.fq` contendo dados "ruins", por exemplo, _reads_ com 10 Ns consecutivos (`NNNNNNNNNN`).

```bash
$ cd ~/unix_lesson/raw_fastq

$ grep NNNNNNNNNN Mov10_oe_1.subset.fq
```

Haverá uma grande quantidade de _reads_ ou linhas de texto.

E se quiséssemos ver todas as informações (no formato FASTQ) para cada uma dessas _reads_? Precisaríamos modificar o comportamento padrão do `grep` e especificar alguns argumentos/opções. Para procurar todas as opções disponíveis para o comando `grep`, podemos digitar `grep --help` (ou `man grep`).

Os argumentos `-B` e `-A` do `grep` serão úteis para retornar a linha correspondente ao padrão `NNNNNNNNNN` mais uma linha anterior (`-B 1`) e duas linhas posteriores (`-A 2`). Como cada registro para uma _read_ tem quatro linhas, usar esses argumentos retornará a informação completa. Dentro do registro, a segunda linha será a sequência real que possui o padrão que procuramos.

```bash
$ grep -B 1 -A 2 NNNNNNNNNN Mov10_oe_1.subset.fq
```

```
@HWI-ST330:304:H045HADXX:1:1101:1111:61397
CACTTGTAAGGGCAGGCCCCCTTCACCCTCCCGCTCCTGGGGGANNNNNNNNNNANNNCGAGGCCCTGGGGTAGAGGGNNNNNNNNNNNNNNGATCTTGG
+
@?@DDDDDDHHH?GH:?FCBGGB@C?DBEGIIIIAEF;FCGGI#########################################################
--
@HWI-ST330:304:H045HADXX:1:1101:1106:89824
CACAAATCGGCTCAGGAGGCTTGTAGAAAAGCTCAGCTTGACANNNNNNNNNNNNNNNNNGNGNACGAAACNNNNGNNNNNNNNNNNNNNNNNNNGTTGG
+
?@@DDDDDB1@?:E?;3A:1?9?E9?<?DGCDGBBDBF@;8DF#########################################################
```

***

**Exercício 1**

1. Procure pela sequência CTCAATGAGCCA em `Mov10_oe_1.subset.fq`. Quantas sequências foram encontradas?

2. Além de encontrar a sequência, como você pode modificar o comando `grep` para que a pesquisa também retorne o nome da sequência?

3. Se você quiser pesquisar essa sequência em **todos** os arquivos iniciados por `Mov10`, qual comando usaria?

***

### Separadores de grupo (`--`) e como removê-los

Observe que quando usamos os argumentos `-B` e/ou `-A` com o comando `grep`, a saída tem algumas linhas adicionais com traços (`--`), esses traços funcionam para separar os grupos de linha retornados e são conhecidos como "separadores de grupo". Isso pode ser problemático se você quiser manter o formato (por ex., manter a especificação do FASTQ) ou se simplesmente não os quiser em sua saída. A opção `--no-group-separator` permite desabilitar este comportamento:

```bash
$ grep -B 1 -A 2 --no-group-separator NNNNNNNNNN Mov10_oe_1.subset.fq
```

Agora o seu _output_ será assim:

```
@HWI-ST330:304:H045HADXX:1:1101:1111:61397
CACTTGTAAGGGCAGGCCCCCTTCACCCTCCCGCTCCTGGGGGANNNNNNNNNNANNNCGAGGCCCTGGGGTAGAGGGNNNNNNNNNNNNNNGATCTTGG
+
@?@DDDDDDHHH?GH:?FCBGGB@C?DBEGIIIIAEF;FCGGI#########################################################
@HWI-ST330:304:H045HADXX:1:1101:1106:89824
CACAAATCGGCTCAGGAGGCTTGTAGAAAAGCTCAGCTTGACANNNNNNNNNNNNNNNNNGNGNACGAAACNNNNGNNNNNNNNNNNNNNNNNNNGTTGG
+
?@@DDDDDB1@?:E?;3A:1?9?E9?<?DGCDGBBDBF@;8DF#########################################################
```

### Como saber o número das linhas coincidentes com o padrão?

Outra opção útil para usar com o `grep` é a opção `-n`, que imprimirá o número das linhas onde o padrão foi encontrado. Podemos testá-la com o comando anterior:

```bash
$ grep -B 1 -A 2 --no-group-separator -n NNNNNNNNNN Mov10_oe_1.subset.fq
```

E o resultado será:

```
861213-@HWI-ST330:304:H045HADXX:1:1101:1111:61397
861214:CACTTGTAAGGGCAGGCCCCCTTCACCCTCCCGCTCCTGGGGGANNNNNNNNNNANNNCGAGGCCCTGGGGTAGAGGGNNNNNNNNNNNNNNGATCTTGG
861215-+
861216-@?@DDDDDDHHH?GH:?FCBGGB@C?DBEGIIIIAEF;FCGGI#########################################################
861953-@HWI-ST330:304:H045HADXX:1:1101:1106:89824
861954:CACAAATCGGCTCAGGAGGCTTGTAGAAAAGCTCAGCTTGACANNNNNNNNNNNNNNNNNGNGNACGAAACNNNNGNNNNNNNNNNNNNNNNNNNGTTGG
861955-+
861956-?@@DDDDDB1@?:E?;3A:1?9?E9?<?DGCDGBBDBF@;8DF#########################################################
```

Um pequeno detalhe que você deve observar é que, ao usar a opção `-n`, as linhas com correspondência apresentam um `:` após o número da linha (por exemplo, `861214:CACTTGTAAGGGCAGGCCCCCTTCACCCTCCCGCTCCTGGGGGANNNNNNNNNANNN...`), enquanto as linhas com um `-` após o número da linha são as linhas circundantes recuperadas ao usar as opções `-A` e/ou `-B` (por exemplo, `861213-@HWI-ST330:304:H045HADXX:1:1101:1111:61397` ).

### Retornando linhas que **NÃO** correspondem ao padrão procurado

Uma última opção do comando `grep` que você pode achar bastante útil é o argumento `-v`, que faz uma correspondência invertida. Isso retornará tudo o que ***não*** corresponder ao padrão. Para demonstrar isso vamos testar em um arquivo menor.

```bash
$ cat ~/unix_lesson/other/Mov10_rnaseq_metadata.txt 
```

Esse é um arquivo de metadados para os dados FASTQ e o resultado de `cat` será:

```
sample	celltype
OE.1	Mov10_oe
OE.2	Mov10_oe
OE.3	Mov10_oe
IR.1	normal
IR.2	normal
IR.3	normal
```

Agora considere que não desejamos imprimir as linhas com o tipo celular (_celltype_) "normal". Para tal, podemos usar a opção `-v` desta forma:

```bash
$ grep -v normal ~/unix_lesson/other/Mov10_rnaseq_metadata.txt 
```

E essa instrução retornará o seguinte resultado:

```
sample	celltype
OE.1	Mov10_oe
OE.2	Mov10_oe
OE.3	Mov10_oe
```

***

## Redirecionamento

Quando usamos `grep`, as linhas correspondentes são impressas no terminal. Se o resultado da pesquisa `grep` for pequeno, podemos visualizá-lo facilmente, mas se a saída for muito longa, as linhas continuarão sendo impressas e não conseguiremos ver muita coisa, exceto as últimas linhas. Tal situação ocorreu quando pesquisamos o padrão `NNNNNNNNNN`. Mas, como podemos salvar os resultados das buscas?

Podemos fazer isso com algo chamado "redirecionamento". A ideia é que estamos redirecionando a saída dos programas para outro local que não o terminal. Vamos começar salvando o resultado em um arquivo, para que possamos analisá-lo mais tarde.

### Redirecionamento com ">"

**O comando de redirecionamento para salvar algo em um arquivo é `>`.**

Vamos testar e escrever todas as seqüências que contêm 'NNNNNNNNNN' do arquivo `Mov10_oe_1.subset.fq` em um novo chamado `bad_reads.txt`.

```bash
$ grep -B 1 -A 2 NNNNNNNNNN Mov10_oe_1.subset.fq > bad_reads.txt
```

O prompt desaparecerá por algum tempo e depois retornará, mas nada será impresso na tela. Mas, você terá um novo arquivo chamado `bad_reads.txt`!

```bash
$ ls -l
```

Examine o arquivo e veja se ele contém o resultado esperado.

> NOTA: Se já tivéssemos um arquivo chamado `bad_reads.txt` em nosso diretório, ele seria substituído sem nenhum aviso!

### Redirecionado e anexando (_appending_) com ">>"

**O comando de redirecionamento para anexar algo a um arquivo existente é `>>`.**

Se usarmos `>>`, o texto será anexado ao conteúdo existente em um arquivo, em vez de sobrescrevê-lo. Isso pode ser útil para salvar mais de uma pesquisa. Por exemplo, o comando a seguir anexará as _reads_ ruins de Mov10_oe_2 ao arquivo bad_reads.txt que acabamos de gerar.

```bash
$ grep -B 1 -A 2 NNNNNNNNNN Mov10_oe_2.subset.fq >> bad_reads.txt

$ ls -l
```

O tamanho do arquivo `bad_reads.txt` mudou?

Como nosso arquivo `bad_reads.txt` não é um arquivo raw_fastq, devemos movê-lo para um local diferente em nosso diretório. Vamos movê-lo para a pasta `other` usando o comando `mv`.

```bash
$ mv bad_reads.txt ../other/
```

### Redirecionando com _pipes_ "|" (_piping_)

**O comando de redirecionamento para usar a saída de um comando como entrada para um comando diferente é `|`.**

**A tecla pipe** (<kbd>|</kbd>) provavelmente não é algo que você usa com muita frequência (está na mesma tecla que a barra invertida (<kbd>\\</kbd>), direita acima da tecla <kbd>Enter/Return</kbd>).

O que `|` faz é pegar a saída de um comando, por exemplo a saída do `grep` e a direcionar para o comando especificado depois dele. Talvez pudéssemos usar `less` para evitar a impressão rápida - várias linhas por segundo - da saída do comando `grep`. Bem, nós podemos! Podemos redirecionar (**pipe**) a saída do comando `grep` para o programa `less` e navegar lentamente, ou usar `head` para ver apenas as primeiras linhas.

```bash
$ grep -B 1 -A 2 NNNNNNNNNN Mov10_oe_1.subset.fq | less
```

Agora podemos usar as setas para nos movermos pelo arquivo e usar a tecla `q` para sair.

Ou podemos apenas examinar uma pequena parte do resultado:

```bash
$ grep -B 1 -A 2 NNNNNNNNNN Mov10_oe_1.subset.fq | head -n 5
```

Outra coisa que podemos fazer é contar o número de linhas geradas pelo `grep`.

O comando `wc` significa ***w**ord **c**ount*. Este comando conta o número de linhas, palavras e caracteres na entrada de texto fornecida a ele. O argumento `-l` contará apenas o número de linhas em vez de contar tudo.

```bash
$ grep NNNNNNNNNN Mov10_oe_1.subset.fq | wc -l
```

*Experimente sem a opção `-l` para ver a saída completa.*

> **Dica** - Semelhante ao `grep`, você pode digitar `wc --help` ou `man wc` para ver todas as opções.

**Sobre _pipes_:**
* O **_pipe_** é um conceito muito importante/poderoso no Shell
* Você pode encadear quantos comandos desejar

A filosofia por trás dos três operadores de redirecionamento (`>`, `>>`, `|`) que você aprendeu até agora é que nenhum deles por si só faz muito. MAS quando você começa a encadeá-los, você pode fazer algumas coisas realmente poderosas com muita eficiência.

**Para poder usar o shell de forma eficaz, é essencial tornar-se proficiente no uso do pipe e dos operadores de redirecionamento.**

## Praticando busca, _piping_ e redirecionamento

Vamos usar os novos comandos em nosso kit de ferramentas (e alguns novos) para examinar o arquivo de "anotação gênica" **chr1-hg19_genes.gtf**. Vamos usar este arquivo para encontrar as coordenadas genômicas de todos os éxons do cromossomo 1.

```bash
$ cd ~/unix_lesson/reference_data/
```

### Introdução ao formato GTF

Vamos explorar um pouco nosso arquivo `chr1-hg19_genes.gtf`. Que informação contém?

```bash
$ less chr1-hg19_genes.gtf
```

> **O arquivo GTF (Gene Transfer Format)** é um arquivo delimitado por tabulações com informações organizadas de maneira muito específica, geralmente para análises NGS. Essa informação é especificamente sobre elementos funcionais encontados em um genoma; neste caso, no cromossomo 1.
>
> Para mais informações sobre este formato de arquivo, confira o [site do Ensembl](http://useast.ensembl.org/info/website/upload/gff.html).

As colunas no arquivo GTF contêm as coordenadas genômicas (localização) dos elementos de um gene (exon, start_codon, stop_codon, CDS) juntamente com os identificadores de genes, transcritos e proteínas associados.

Um éxon pode fazer parte de dois ou mais transcritos diferentes gerados a partir do mesmo gene (os diferentes transcritos de um gene são conhecidos como isoformas). Dessa forma, em um arquivo GTF, um éxon pode ser representados várias vezes, uma para cada transcrito ou isoforma (derivadas do evento de _splicing_ alternativo).

```bash
$ grep PLEKHN1 chr1-hg19_genes.gtf | head -n 5
```

Essa pesquisa retorna dois transcritos diferentes - NM_001160184 e NM_032129 - do mesmo gene e que contêm o mesmo éxon.

### Comandos `cut` e `sort`

Vamos aprender dois novos comandos para a nossa prática.

* **`cut` é um comando muito útil para extrair colunas de arquivos.**

Usaremos `cut` com a opção `-f` para especificar quais colunas específicas do arquivo queremos extrair. Digamos que desejamos extrair a 1ª coluna (número do cromossomo) e a 4ª coluna (coordenada de início) do arquivo `chr1-hg19_genes.gtf`, para tal podemos usar a seguinte instrução:

```bash
$ cut -f 1,4 chr1-hg19_genes.gtf  | head
```

> O comando `cut` assume que as colunas são separadas por tabulações `TAB` (ou seja, delimitadas por tabulações **TSV**). O `chr1-hg19_genes.gtf` é um arquivo delimitado por tabulações, então o comando padrão `cut` funcionria. No entanto, os dados podem ser separados por outros tipos de delimitadores como "," ou ";". Se os dados não estiverem delimitados por tabulação, você pode adicionar a opção `-d` para especificar o delimitador (por exemplo, `-d ","` para um arquivo .csv).

* ** O comando `sort` é usado para ordenar o conteúdo de um arquivo.** Ele possui argumentos que permitem escolher por qual coluna classificar (`-k`), que tipo de classificação você deseja (por exemplo, numérico `-n`) e também se o resultado da ordenação deve retornar apenas valores únicos (`-u`). Esses são apenas algunns dos muitos recursos do comando `sort`.

Vamos fazer um teste rápido de como o argumento `-u` retorna apenas linhas únicas (e remove duplicatas).

```bash
$ cut -f 1,4 chr1-hg19_genes.gtf | wc -l
```

*Quantas linhas foram retornadas?*

<details>
	<summary><b><i>Clique aqui para conferir a resposta</i></b></summary>
	<p>Seu comando deve ter retornado 76.767 linhas.</p>
</details>

Agora utilize o comando `sort -u` antes de contar as linhas.

```bash
$ cut -f 1,4 chr1-hg19_genes.gtf | sort -u | wc -l
```
*Quantas linhas você vê agora?*

<details>
	<summary><b><i>Clique aqui para conferir a resposta</i></b></summary>
	<p>Seu comando deve ter retornado 27.852 linhas.</p>
</details>

***

**Exercício 2**

Agora que sabemos qual tipo de informação há dentro do arquivo GTF, vamos usar os comandos que aprendemos até agora para responder a uma pergunta simples sobre nossos dados: **A partir do arquivo `chr1-hg19_genes.gtf `, quantos éxons únicos estão presentes no cromossomo 1?**

Para calcular o número de exons únicos no cromossomo 1, vamos realizar uma série de etapas, conforme mostrado abaixo. Neste exercício, precisamos entender qual comando usar para cada etapa.

1. Extrair apenas as linhas contendo éxons
2. Extrair as coordenadas genômicas 
3. Remover os éxons duplicados
4. Contar o número total de éxons

O objetivo final é ter uma única linha de código, com vários comandos encadeados usando o operador pipe. Mas, recomendamos que, em um primeiro momento, você faça isso de maneira gradual, conforme detalhado abaixo.

#### 1. Extrair as coordenadas genômicas das linhas contendo éxons

Nós queremos apenas os exons (não os elementos CDS ou start_codon), então vamos usar `grep` para procurar a palavra "exon". É importante verificar as primeiras linhas  da saída do `grep` direcionando o resultado para o comando `head`. ***Relate o comando usado nesta fase.***

#### 2. Extrair as coordenadas genômicas

Vamos definir a singularidade de um éxon através das coordenadas genômicas, tanto inicial quanto final. Portanto, a partir do resultado da etapa 1, precisamos manter 4 colunas (**chr**, **start**, **stop** e **strand**) para encontrar o número total de éxons únicos. Os números de coluna que você deseja são 1, 4, 5 e 7.

Você pode usar `cut` para extrair essas colunas da saída do passo 1. ***Relate o comando usado nesta etapa.***

Neste ponto, as primeiras linhas devem ficar assim:


```bash

	chr1	14362	14829	-
	chr1	14970	15038	-
	chr1	15796	15947	-
	chr1	16607	16765	-
	chr1	16858	17055	-
	
```

#### 3. Remover os éxons duplicados

Agora, precisamos remover os exons repetidos devido às isoformas. Podemos usar o comando `sort` com a opção `-u`. ***Relate o comando usado nesta fase.***

Você vê uma mudança em como a ordem mudou? Por padrão, o comando `sort` irá ordenar e o que você não pode ver aqui é que ele removeu as duplicatas. Usaremos a etapa 4 para verificar se esta etapa funcionou.

#### 4. Contar o número total de éxons

Primeiro, verifique quantas linhas teríamos sem usar o comando `sort -u`, para tal utilize o comando `wc -l`.

Agora, para contar quantos éxons únicos estão no cromossomo 1, usaremos o `sort -u` e redirecionaremos a saída para `wc -l`. Você observa uma diferença no número de linhas?

***Informe o comando usado ao final e o número de linhas que você vê com e sem o `sort -u`.***

***

### Resumo!

Ao invés de solucionar o problema em apenas uma linha de código no final, poderíamos ter feito em várias etapas salvando a saída de cada comando em um novo arquivo. Entretanto, não seria tão eficiente quanto usar o **_pipe_**! Tudo o que precisávamos era o número de éxons únicos, e as saídas intermediárias não eram úteis. Evitar o armazenamento de dados desnecessários de cada etapa intermediária evita a desordem e nos ajuda a evitar o desperdício de espaço de armazenamento!


---
**Bibliografia / Fontes**

BASH programming: https://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html 

BASH for Genomics: https://angus.readthedocs.io/en/2016/GenomicsShell.html 

GNU/Linux tutorials: https://www.debian.org/doc/manuals/debian-reference/ch01.en.html 

Jargas, Aurelio Marinho. Shell Script Profissional. São Paulo : Novatec Editora, 2008.

*The materials used in this lesson were derived from work of members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *The materials used in this lesson were derived from work that is Copyright © Data Carpentry (http://datacarpentry.org/). 
All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*
* *Adapted from the lesson by Tracy Teal. Original contributors: Paul Wilson, Milad Fatenejad, Sasha Wood and Radhika Khetani for Software Carpentry (http://software-carpentry.org/)*
