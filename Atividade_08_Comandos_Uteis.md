
## Comandos úteis

Como vimos nas atividades anteriores, os sistemas GNU/Linux possuem vários programas que nos auxiliam na manipulação e edição de arquivos. É importante ter uma visão geral das ferramentas para aproveitar ao máximo os recursos disponíveis. Nesta atividade vamos conhecer (ou revistar) alguns comandos importantes, assim como suas opções mais comuns. 

## SED (_Stream EDitor_)

O comando `sed` pode realizar várias funções para editar um ou mais arquivos de texto, dentre elas, busca, substituição, inserção e deleção. Ao contrário dos editores de texto citados anteriormente como o `vim` e o `Nano`, o programa sed não é interativo, ou seja, não é possível abrir e interagir com o arquivo de texto. Ao invés disso, o usuário irá repassar as instruções desejadas ao programa que procederá com as modificações. O comando `sed` lê as informações do `STDIN` (_standard input_), processa cada linha de texto, uma-a-uma, e então direciona o texto com as alterações para o `STDOUT`. Em outras palavras, o comando `sed` é capaz de ler informações de arquivos de texto ou redirecionadas a partir do _pipe_ `|` e o texto modificado é impresso no prompt de comando ou redirecionado à um arquivo de saída através do `>`.

> O comando `sed` não edita o arquivo de entrada, mas esse pode ser sobrescrito através da opção `-i`. Entretanto, tenha bastante cuidado ao utilizar a opção `-i`, pois uma vez sobrescrito, o arquivo original não poderá ser restaurado.

### Comportamento do `SED`

![image](https://user-images.githubusercontent.com/17560094/170845608-4ac150b5-5aa7-44c9-960b-8b251d53084a.png)

A ferramenta `sed` lê e processa, individualmente, cada linha do _input_, caso a linha corresponda ao padrão indicada, a instrução de edição é aplicada à linha e as modificações direcionados ao _standard output_. Se não houver correspondência de padrão, a instrução é ignorada. Uma ou mais intruções podem ser aplicadas a cada linha, uma por vez, sequencialmente.

Sintaxe do comando `sed`:

```bash
$ sed [opções] [endereços] [comando] [arquivo]

```

* **Endereços** são parâmetros que determinam em quais linhas os **comandos** serão aplicados. Os endereços podem ser numéricos (número(s) da(s) linha(s)), conter textos ou expressão regulares ou especificar intervalos (numéricos ou de coincidência de padrão).
* **Comandos** correspondem às ações possíveis do programa, são determinadas por uma letra (por exemplo, `d` para deleção) e cada ação possui uma sintaxe específica.

A seguir vamos aprender algumas operações básicas usando o comando `sed`.

### **Substituição**

O uso mais comum para o `sed` é a busca e substituição de padrões, na qual buscamos por um determinado padrão na linha e este é então subsituído por um texto indicado pelo usuário.

A sintaxe de substituição é:

```bash
$ sed 's/REGEXP/REPLACEMENT/flag'

```
O argumento `s` representa o comando para realizar substituições no texo. Em geral, o comando `s` será seguido por três barras `/`, responsáveis por separar o padrão a ser buscado `REGEXP` e o texto de substituição `REPLACEMENT`. `REGEXP` pode ser um texto comum ou apresentar expressões regulares para generalizar os padrões de correspondência.

> As barras `/` podem ser substituídas por outros caracteres, tais como, dois pontos `:`, pipe `|` e sublinhado `_`. A menos que a troca por estes caracteres torne mais simples a visualização da expressão, prefira o uso convencional com barras `/`.

As **_flags_** permitem controlar o processo de substituição, se nenhuma for indicada, apenas a primeira ocorrência do padrão `REGEXP` é substituída. Dentre as opções de **_flags_** mais comuns, temos:

* `g` - global, substitui todas as ocorrências de `REGEXP`
* `n` - número, indica a posição da ocorrência a ser substituída, por exemplo, se o número `2` for usado, apenas a segunda ocorrência de `REGEXP` será substituída.

Vamos testar alguns tipos de substituições para que você possa se habituar ao uso do comando `sed`. Em primeiro lugar, verifique o conteúdo do arquivo `~/unix_lesson/genomics_data/Encode-hesc-Nanog.bed`.

```bash
$ cat ~/unix_lesson/genomics_data/Encode-hesc-Nanog.bed | head

```

> Os arquivos BED (_Browser Extensible Data_) são arquivos de texto delimitados por tabulações (**TSV**) que permitem a representação de anotações e informações genômicas. É constituído por três colunas mandatórias: 1a) Cromossomo; 2a) Posição Inicial; 3a) Posição Final. A demais colunas são opcionais e podem conter informações sobre elementos genômicos, por exemplo, poderíamos representar a posição de um gene desta forma: `chr11 5225464 5227071  HBB`.

Agora vamos trocar a expressão `chr` por `chromossome`:

```bash
$ sed 's/chr/chromosome/g' ~/unix_lesson/genomics_data/Encode-hesc-Nanog.bed

```

Observe que em todas as linhas o padrão "chr" foi substituído por "chromomose". Verifique o arquivo original novamente. As alterações foram salvas no arquivo?

As alterações de `sed` ocorrem apenas no que é chamado de `pattern space`, logo, o arquivo original não é alterado. Para salvar as alterações devemos redirecioná-las para um novo arquivo.

```bash
$ sed 's/chr/chromosome/g' ~/unix_lesson/genomics_data/Encode-hesc-Nanog.bed > chromosome_Encode.bed

```
Ao examinar o novo arquivo `chromosome_Encode.bed`, salvo no seu siretório atual, note que as alterações desta vez foram salvas. Outra opção para gravar as modificações é utilizar a opção `-i`:

```bash
$ sed -i 's/chromosome//g' chromosome_Encode.bed

```

No exemplo acima, realizamos a troca da expressão `chromossome` por uma string vazia, ou seja, deletamos o prefixo `chromosome` e mantemos apenas o valor numérico. Caso desejássemos retornar com o prefixo `chr`, bastaria usar o seguinte comando:

```bash
$ sed -i 's/^/chr/g' chromosome_Encode.bed

```

> O caracter `^` indica o início de uma string ou de uma linha em um arquivo com múltiplas linhas. No exemplo acima, o `sed` encontra a correspondência de cada início de linha e substitui pelo texto `chr`.

Em um dos exemplos anteriorers, utilizamos o comando *substitute* `s` para remover um trecho de uma string, mas e se desejássemos remover linhas inteiras?

Uma opção para deletar linhas em branco, é usar o comando abaixo, onde o caracter `$` é um marcador de fim de linha, logo, o padrão `^$` reconhece todas as linhas nas quais não há nenhum caracter entre o marcador de início e de fim de linha.

```bash
$ sed -i 's/^$//g' chromosome_Encode.bed

```

### **Deleção**

Através do comando `d` é possível deletar linhas específicas a partir do `sed`. A sintaxe para deleção é um pouco diferente daquela utilizada para substituir. Confira abaixo a sintaxe usual para deleção de linhas:

```bash
$ sed [Endereço] ou [Padrão] d [Arquivo]

```

Vamos usar alguns exemplos para tornar mais claro o uso do comando `d`. Primeiro, vamos reexaminar o arquivo `~/unix_lesson/other/Mov10_rnaseq_metadata.txt`

```bash
$ cat ~/unix_lesson/other/Mov10_rnaseq_metadata.txt

```
Agora vamos testar algumas instruções para deletar linhas deste arquivo. **ATENÇÃO** **NÃO utilize a opção `-i` para não sobrescrever o arquivo original!**

Para deletar o cabeçalho do arquivo `Mov10_rnaseq_metadata.txt` podemos usar a expressão:

```bash

$ sed '1d' ~/unix_lesson/other/Mov10_rnaseq_metadata.txt

```

Caso desejássemos remover as linhas 5 a 7, teríamos de indicar a linha de início e fim para deleção separadas por uma `,`, como no exemplo abaixo:

```bash

$ sed '5,7d' ~/unix_lesson/other/Mov10_rnaseq_metadata.txt

```

E as linhas 1 e 5 a 7? Como faríamos?

```bash

$ sed '1d;5,7d' ~/unix_lesson/other/Mov10_rnaseq_metadata.txt

```

Podemos separar múltiplos intervalos de linha usando o caracter ponto e vírgula `;`. Nesse caso, cada instrução de para deleção é separada por `;` como no exemplo acima. É possível usar `;` para aplicar múltiplos comandos, não apenas o `d` de deleção. A opção `-e` é outra forma de passar várias instruções em um mesmo comando `sed`, veja o mesmo exemplo, mas agora usando a opção `-e`:

```bash

$ sed -e 5,7d -e 1d ~/unix_lesson/other/Mov10_rnaseq_metadata.txt

```

### **Impressão**

Para imprimir uma linha específica, é possível usar os endereços, entranto, por padrão `sed` imprime todas as linhas, por isso precisamos desativar essa função usando a opção `-n`. Ao utilizar a opção `-n`, nenhum conteúdo será impresso. Ao utilizar a opção `-n` em conjunto com o comando `p`, podemos imprimir apenas as linhas que desejamos, por exemplo, para imprimir apenas as linhas 2 a 4:


```bash

$ sed -n '2,4p' ~/unix_lesson/other/Mov10_rnaseq_metadata.txt

```

Note que é possível atingir o mesmo objetivo de diferentes maneiras, um método ainda não utilizado foi o uso de padrões de strings no endereço. Como conseguiríamos realizar a mesma ação usando um padrão de texto?

```bash

$ sed -n '/Mov10_oe/p' ~/unix_lesson/other/Mov10_rnaseq_metadata.txt

```

> Sempre que quiser realizar alterações em linhas específicas usando um padrão de texto, utilize `/REGEXP/` antes do comando desejado. 
> Para os comandos de substituição e de transliteração, o uso de `REGEXP` no endereço faz com que o `sed` realize duas buscas: a primeira para encontrar as linhas correspondentes ao endereço e a segunda busca para encontrar o padrão a ser substituído.

### Transliteração

O comando de transliteração `y` permite a substituição de caracteres. Quando utilizado, cada caracter na linha que apresente correspondência será substituído por aquele indicado na instrução. A sintaxe para transliteração é semelhante à de substituição, sendo o comando `y` seguido por dois campos separados por barras `/`: `y/CARACTER_BUSCA/CARACTER_SUBSTITUIÇÃO/`. Para alterar todas ocorrências de `e` por `u`, podemos utilizar:

```bash

$ sed 'y/e/u/' ~/unix_lesson/other/Mov10_rnaseq_metadata.txt

```

> Observe que para o comando `y` não é necessário utilizar a **_flag_** `g` após a terceira barra.

É possível alterar mais de um caracter em um mesmo comando, por exemplo, para alterar `e` por `u` e `a` por `A` podemos:

```bash

$ sed 'y/ea/uA/' ~/unix_lesson/other/Mov10_rnaseq_metadata.txt

```

> **IMPORTANTE:** Se mais de um caracter for utilizado no comando de transliteração, cada um deles será avaliado e modificado individualmente, ao contrário do que ocorre com o comando de substituição que usa toda a string para realizar a busca.

***

**Exercício 1**

Que tal aplicarmos nossos conhecimentos em `sed` para converter um arquivo de _reads_ no formato FASTQ em um arquivo FASTA contendo a sequência complementar das reads? Parece complicado? Não se assuste, vamos dividir o problema para facilitar a resolução e apresentar dois novos recursos do `sed`.

1) O primeiro recurso é o uso de padrões numéricos no endereço. É possível indicar ao `sed` certos padrões como, aplicar as alterações de 10 em 10 linhas, dentre outras possibilidades. Para imprimir todas os cabeçalhos de um arquivo FASTQ, podemos usar o comando a seguir. 

```bash

$ sed -n '1~4p' ~/unix_lesson/raw_fastq/Mov10_oe_1.subset.fq | head

```

E para imprimir as sequências, podemos usar:

```bash

$ sed -n '2~4p' ~/unix_lesson/raw_fastq/Mov10_oe_1.subset.fq | head

```

A instrução `1~4p` indica para o `sed` imprimir a primeira linha do arquivo e todas aquelas a cada 4 linhas (1, 5, 9, 13, ..., n+4). O padrão `2~4p` é iniciado na segunda linha e imprime esta e todas aquelas na posição +4 posteriores (2, 6, 10, 14, ..., n+4).

Os dois comandos para impressão podem ser combinados utilizando o `;`.

Uma parte do problema foi solucionada, pois agora sabemos como imprimir apenas as linhas de interesse do arquivo FASTQ.

2) As sequências em arquivos FASTQ são iniciadas por um `@` enquanto as sequências em formato FASTA são iniciadas por `>`. Logo, para realizar a conversão de modo adequado, devemos **substituir** todas as ocorrências de `@` no início da linha por `>`. Para este passo, lembre-se de usar o caracter que indica o início de linha, por exemplo, `^@`.

Outra etapa da solução do problema foi esclarecida.

3) Agora precisamos substituir as bases nucleotídicas por suas bases complementares. Para tal, podemos usar o comando de **transliteração** apenas nas linhas contendo  sequências (segunda linha). Para aplicar as modificações de troca de caracteres apenas nas linhas desejadas existem duas opções:

a) Indicar o padrão numérico como apresentado na primeira etapa, por exemplo, `2~4`.

b) Usar uma expressão regular para buscar quaisquer linhas que contenham caracteres além de **A, C, G, T e N**. Como ainda não aprendemos expressões regulares, esta não é a maneira ideal de solucionar o problema, mas é uma oportunidade de aprendermos como funciona a instrução `!` (ou negação).

```bash

$  sed -e '/[^ACGTN]/! y/ACGT/tgca/' ~/unix_lesson/raw_fastq/Mov10_oe_1.subset.fq | head

```

> A expressão regular `[^ACGTN]` indica que buscamos por quaisquer strings (ou linhas, neste caso) que contenham caracteres além dos representeados entre colchetes.

Note que usamos `!` após a expressão regular `[^ACGTN]`, o uso da exclamação indica que desejamos o contrário do que está indicado no padrão. Por exemplo, o comando abaixo imprime apenas as linhas que não são iniciadas por `@`:

```bash

$  sed -n '/^@/!p' ~/unix_lesson/raw_fastq/Mov10_oe_1.subset.fq | head

```

Voilá, dispomos de todas as ferramentas necessárias para criar um script capaz de converter um arquivo FASTQ em FASTA e imprimir a sequência complementar. Tente agora unir todas estas etapas em uma única linha de comando para realizar a conversão!

**IMPORTANTE** A ordem dos comandos importa, lembre-se que cada instrução é executada sequencialmente e uma dica é deixar a impressão (etapa 1) para o final.

***

## **Sort**

O comando `sort` possibilita ordenar - alfabeticamente ou numericamente - as linhas de um ou mais arquivos. Por padrão, todo o input é avaliado para a ordenação e espaços em branco são considerados separadores de campos. Algumas opções estão disponíveis para customizar o funcionamento deste comando, como veremos a seguir.

A sintaxe de `sort` é:


```bash

$  sort [OPTION]... [FILE]

```

Vamos iniciar ordenando o arquivo `~/unix_lesson/genomics_data/Encode-hesc-Nanog.bed`.

```bash

$  sort ~/unix_lesson/genomics_data/Encode-hesc-Nanog.bed |  head

```

Note que o cromossomo nas primeiras posições é o `chr10`. Mesmo que usássemos a opção `-n` para ordenar numericamente, o resultado seria o mesmo (você pode tentar se quiser). Entretanto, versões mais atuais do `sort` dispõe da opção `-V` que permite ordenar alfanumericamente. Vamos ordenar usando esta opção:

```bash

$  sort -k1 -V  ~/unix_lesson/genomics_data/Encode-hesc-Nanog.bed | head

```
Agora o ordenamento das três colunas funcionou como o desejado! A opção `-k 1` indica ao `sort` qual coluna ele deve usar como referência para realizar a ordenação, no exemplo acima, instruímos o programa a ordernar a partir da primeira coluna. Abaixo veja uma tabela com as principais opções do comando `sort`.

**Principais opções para o programa SORT**
|     Opção    |                          Descrição                        |
|:------------:|:---------------------------------------------------------:|
|       -n     |     Ordena numericamente                                  |
|       -r     |     Ordem reversa (Z a A; 9   a 0)                        |
|       -k     |     Ordena pela coluna indicada em k                      |
|       -t     |     Separador de campo/coluna (padrão é   tabulação)      |
|       -u     |     Exclui duplicatas                                     |
|       -f     |     Ignora a diferença entre maiúsculas e   minúsculas    |
|      -V      | Ordena alfanumericamente                                  |
|      -h      | Ordena valores human-readable (5K, 1G)                    |
|      -M      | Ordena por meses                                          |


## **Cut**

O comando extrai campos ou trechos de um arquivo. É possível realizar a extração do conteúdo indicando as colunas de interesse, e caso as colunas não estejam separadas por tabulações, é necessário indicar a espécie de delimitador, por exemplo, `,`, `:`, `-`, dentre outros. A opção para selecionar o campo/coluna é `-f`(_field_) e o delimitador é indicado através da opção `-d`.

Para aprender o funcionamento do comando `cut`, precisamos copiar um novo arquivo para o nosso diretório `$HOME` ou `~`.


```bash

$  cp /data/2022_shell_script/unix_lesson/other/chars.txt ~ 

```

Examine o arquivo para identificar o padrão de separação dos campos e o número de colunas. Você perceberá que os campos são separados por `,` (vírgulas) e há 4 campos no arquivo. Como faríamos para selecionar as colunas dois e três do arquivo `chars.txt`? Bem, podemos usar as opções `-d` para delimitador e `-f` para campos:

```bash

$  cut -d, -f2,3 chars.txt

```

A ferramenta `cut` é muito poderosa para trechos de arquivos, nesta atividade, nos limitaremos a aprender sobre a extração de colunas, mas saiba que existem outras formas de proceder, usando caracteres ou bytes. Nas tabelas abaixo, são listadas as opções mais comuns para `cut`.

**Principais opções para o comando cut**
|     Opção    |                       Descrição                      |
|:------------:|:----------------------------------------------------:|
|       -d     |     Delimitador de campo (tabulação é o   padrão)    |
|       -f     |     Extrai os seguintes campos ou colunas            |
|       -c     |     Extrai os seguintes caracteres                   |

**Modos de representar os campos ou caracteres para extração**
|     -f / -c    |        Abrange       |                      Descrição                     |
|:--------------:|:--------------------:|:--------------------------------------------------:|
|       2,5      |        2 e   5       |     Segundo e quinto                               |
|       2-5      |        2 a   5       |     Do segundo ao quinto                           |
|        2-      |     2 3   4 5 ...    |     Do segundo em frente                           |
|        -5      |      1 2   3 4 5     |     Até o quinto                                   |
|       2,5-     |     2 5   6 7 ...    |     O segundo e do quinto em diante                |
|     2,3,5-8    |     2 3   5 6 7 8    |     O segundo, terceiro e do quinto ao   oitavo    |


## **Uniq**

O comando `uniq` é capaz de identificar e excluir duplicatas em linhas consecutivas. Devido à necessidade de que as linhas repretidas sejam adjacentes, em geral, é utilizado após o comando `sort`. Algumas versões de `sort` possuem a opção `-u` que também permite a exclusão de linhas repetidas, desta forma, uma das principais vantagens do comando `uniq` é identificar e imprimir replicatas usando a opção `-d` ou então contar o numéro de duplicatas existente no arquivo. 

Como exemplo, podemos contar o número de replicatas para cada tipo celular no arquivo `~/unix_lesson/raw_fastq/Mov10_oe_1.subset.fq`

```bash

$  cut -f2 unix_lesson/other/Mov10_rnaseq_metadata.txt | uniq -c

```

Quantas replicatas temos para cada tipo celular? São 3 replicatas. Note que primeiro foi necessaário extrair a coluna de interesse usando `cut`, para então aplicar o comando `uniq`. Neste caso, não foi necessário usar o comando `sort` pois as linhas as duplicatas da segunda coluna eram adjacentes.

**Opções mais usuais para o comando uniq**
|     Opção    |                        Descrição                       |
|:------------:|:------------------------------------------------------:|
|       -i     |     Ignora diferença entre maiúscula e   minúsculas    |
|       -d     |     Mostra apenas as linhas duplicadas                 |
|       -u     |     Mostras apenas as que são únicas                   |
|       -c     |     Conta o número de repetições                       |


***

**Exercício 2**

No exemplo anterior, devido ao tamanho do arquivo, não era difícil calcular o número de elementos repetidos visualmente, mas para arquivos um pouco maiores, essa tarefa se torna cada vez mais demorada e propensa a erros. Por isso, o ideal é utilizar a "caixa de ferramentas" que o GNU/Linux nos proporciona. 

Neste exercício, sua tarefa é contar o número de elementos gênicos presentes em cada cromossomo no arquivo `~/unix_lesson/genomics_data/Encode-hesc-Nanog.bed`. 

Quantos elementos genômicos temos nos cromossomos X e Y?

***

## Xargs (eXtended ARGumentS)

O comando `xargs` é extremamente útil para executar programas a partir de uma lista de arquivos. Ele funciona como um gerenciador dos argumentos que serão repassados à um programa para execução. 

A sintaxe básica de `xargs` é:

```bash

$  xargs [opções] [comando [argumentos iniciais]]

```

Vamos começar por um exemplo bem simples, contar o número de linhas em cada arquivo contido na pasta `~/unix_lesson/raw_fastq`.


```bash
$  cd ~/unix_lesson/raw_fastq

$  ls  | xargs wc -l
```

> Note que foi necessário mover-se até a pasta para realizar a contagem. Isso ocorre porque o comando `xargs` busca os arquivos no caminho especificado (relativo ou absoluto) e como o comando `ls` retorna apenas o nome do arquivo e não o caminho, `xargs` irá repassar apenas o nome do arquivo ao comando `wc -l`. Caso o diretório atual não seja o mesmo da localização dos arquivos, o comando `wc -l` não encontrará os arquivos listados. Existem algumas opções para evitar esse tipo de situação, por exemplo, utilizar o caminho absoluto ou relativo dos diretórios contendo os arquivos.

Uma situação de uso comum para `xargs` é quando desejamos criar um _back-up_ (ou cópias) de vários arquivos contidos em uma determinada pasta, ou listados em um arquivo de texto. 

Verifique o conteúdo da pasta `/data/2022_shell_script/unix_lesson/other/random_files`, há 8 arquivos sem qualquer padrão comum para utilizarmos caracteres-coringa (_wildcards_). Mas podemos copiar cada um desses arquivos usando apenas uma linha de comando, como a seguir:

```bash

$  find /data/2022_shell_script/unix_lesson/other/random_files | xargs -i cp {} .

```

Traduzindo o comando acima, o comando `find` irá gerar uma lista de todo o conteúdo da pasta `random_files`, então esses arquivos serão  repassados ao `xargs` um-a-um, o argumento `-i` indica ao `xargs` para trocar a string `{}` pelo argumento da vez e, por fim, o `.` indica o local cópia - o diretório atual do usuário.

Outra vantagem de `xargs` é a possibilidade de executar comandos em paralelo através da opção `-P` seguida do número de execuções em paralelo desejada.
