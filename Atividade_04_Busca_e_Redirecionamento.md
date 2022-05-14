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

