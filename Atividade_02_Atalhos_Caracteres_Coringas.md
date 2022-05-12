## Wildcards (Caracteres-coringa) e Shortcuts (Atalhos) úteis no Shell

### Wildcards

Os **_wildcards_** ou **caracteres-coringa** são símbolos utilizados para representar um ou mais caracteres. Esses símbolos são utilizados para descrever o casamento de padrões, ou seja, verificar a presença de um padrão ou estrutura pré-determinada em um conjunto de dados. 

Uma das aplicações dos **_wildcards_** é na busca por arquivos através de scripts ou do prompt de comando.

Conecte-se ao servidor Darwin - caso necessário, revisite a lição anterior - para realizar as ações descritas abaixo e descobrir aplicações dos **_wildcards_**. 

#### Como usar **_wildcards_**?

**O wildcard: `*` (asterisco)**

Acesse o diretório `~/unix_lesson/raw_fastq`. Esse diretório contem arquivos FASTQ de um conjunto de dados de sequenciamento de nova geração (NGS).

> Arquivos FASTQ são arquivos de texto criados para armazenar sequências biológicas (por exemplo, sequências de DNA ou RNA). Esse formato de arquivo permite a representação dos nucleotídeos e de seus respectivos escores de qualidade. [FASTQ](https://support.illumina.com/bulletins/2016/04/fastq-files-explained.html)

O caracter `*` é um atalho para "qualquer sequência de caracteres". Logo, se você utilizar o comando `ls *`, verá todo o conteúdo de um determinado diretório. Entretanto, o caso de uso mais interessante é quando combinado a determinados padrões. Por exemplo, teste esse comando:

```bash
$ ls *fq
```
A lista retornada irá conter apenas arquivos com nomes terminados em `fq`. Agora teste o seguinte comando:

```bash
$ ls /usr/bin/*.sh
```

A lista apresenta todos os arquivos presentes em `/usr/bin` terminados com os caracteres `.sh`. O caracter-coringa `*` pode ser posicionado em qualquer local do padrão desejado. Por exemplo:

```bash
$ ls Mov10*fq
```
Agora são retornados apenas arquivos iniciados por `Mov10` e terminados por `fq`.

Qual é a lógica por detrás do `*`? O Shell (e alguns outros interpretadores e linguagens de programação) interpreta um `*` como um caracter-coringa que pode coincidir com zero, uma ou mais ocorrências de qualquer caracter. No caso de zero ocorrências, um arquivo nomeado `Mov10fq` seria reconhecido no exemplo acima.

> O caracter `*` é apenas um dos _wildcars_ reconhecidos pelo GNU/Unix, mas um dos mais poderosos e mais utilizados no dia-a-dia. Outros exemplos são apresentados em uma tabela ao final desta seção.

**O wildcard: `?` (interrogação)**

Outro _wildcard_ é o `?`, ele guarda semelhanças com `*`por poder coincidir com qualuqer caracter, mas está restrito a apenas uma posição, ou seja, a coincidência possível será de apenas UM caracter qualquer.

Lembre-se que `*` pode representar qualquer número de coincidências, incluindo nenhuma. Para reforçar essa distinção, vamos a alguns exemplos. Primeiro, corra comando:

```bash
$ ls /bin/d*
```

Isso exibirá todos os arquivos em `/bin/` que começam com "d", independentemente do número de caracteres que compõe os nomes. Entretanto, se você quiser apenas nomes de arquivos em `/bin/` que comecem com "d" e possuem apenas dois caracteres, então você pode usar:

```bash
$ ls /bin/d?
```

Por fim, você pode encadear vários "?" para especificar o comprimento desejado. No exemplo abaixo, você estaria procurando por todos os arquivos em `/bin/` nos quais os nomes começam com um "d" e têm um comprimento de três caracteres:

```bash
$ ls /bin/d??
```

**Tabela 1:** _Wildcards_ (caracteres-coringa) mais comuns no Shell 
|           Comando         |                             Definição                            |
|:-------------------------:|:----------------------------------------------------------------:|
|     *                     |     Coincide com zero ou mais caracteres                         |
|     *.txt                 |     Todos os arquivos terminados em .txt                         |
|     ?                     |     Coincide com apenas um caractere                             |
|     a?.txt                |     Arquivos iniciados com a, + 1 caracter qualquer, + .txt      |
|     [abcde]               |     Coincide com qualquer caracter da   lista                    |
|     sample_R[12].fastq    |     Coincide com sample_R1.fastq e   sample_R2.fastq             |
|     [a-e]                 |     Coincide com qualquer caractere no  intervalo                |
|     [!abcde]              |     Coincide com qualquer caractere  não listado                 |
|     [!a-e]                |     Coincide com qualquer caractere  fora do intervalo           |
|     {sample,amostra}      |     Coincide com uma das palavras dentro   das chaves            |


****

**Exercício 1**

Utilize o comando `ls` para resolver cada uma das questões abaixo (sem sair do diretório atual).

1. Liste todos os arquivos em `/bin` que começam com a letra 'c'

2. Liste todos os arquivos em `/bin` que contenham a letra 'a'

3. Liste todos os arquivos em `/bin` que terminam com a letra 'o'

4. Use o comando `ls` para listar todos os arquivos em `/bin` que contenham 'a' ou 'c'. (Dica: consulte a tabela acima)

****

### Atalhos (Shortcuts)

O Shell também disponibiliza alguns atalhos que podem ser muito úteis para se movimentar pela estutura de pastas/arquivos.

#### Pasta Home ou "~"

Lidar com o diretório inicial é muito usual. No shell, o caractere til, "~", é um atalho para seu diretório pessoal. Para testar esse recurso, vamos primeiro acessar a pasta `raw_fastq` (com o intuito de se habituar ao uso do recurso autocompletar, utilize a tecla `TAB` para concluir o preenchimento do caminho):

```bash
$ cd
$ cd unix_lesson/raw_fastq
```

E então, digite o seguinte comando:

```bash
$ ls ~
```
O conteúdo do seu diretório pessoal será impresso, sem que você precise digitar o todo o caminho. Isso ocorre porque o til `~` equivale a `/home/<username>`.

#### Diretório pai ou ".." (dois pontos)

Outro atalho visto anteriormente é o "..":

```bash
$ ls ..
```

O atalho `..` sempre se refere ao diretório pai do corrente, ou seja, o diretório em que você se encontra atualmente. Neste caso, o comando `ls ..` imprimirá o conteúdo de `unix_lesson` (o diretório pai da pasta `raw_fastq`). Você pode encadear vários `..`, separados por `/`:

```bash
$ ls ../..
```

Tal ação irá listar o conteúdo de `/home/<nome_do_usario>` (seu diretório pessoal), que está dois níveis acima do seu diretório atual.

#### Diretório corrente, atual ou "."

O atalho `.` sempre se refere ao seu diretório atual. Logo, `ls` e `ls .` farão a mesma coisa - eles imprimem o conteúdo do diretório atual. Isso pode parecer um atalho inútil, mas o atalho `.` pode ser muito útil em situações como a cópia de arquivos e diretórios. Inclusive, lembre-se de que o usamos na atividade anterior quando copiamos os dados de uma outra pasta para nosso diretório inicial.

Para resumir, os comandos `ls ~`, `ls ~/.` e `ls /home/<nome_do_usario>` fazem exatamente a mesma coisa. Os atalhos podem ser muito convenientes convenientes quando você navega pelos diretórios!


#### Histórico de comandos `history`

Você pode acessar facilmente os comandos digitados anteriormente pressionando a tecla de seta <kbd>↑</kbd> (para cima) em seu teclado, desta forma você pode retroceder no histórico de comandos. Por outro lado, a tecla de seta <kbd>↓</kbd> avança em relação aos comandos mais recentes no histórico de comandos.

***Enquanto estiver no prompt de comando, pressione a seta <kbd>↑</kbd> algumas vezes e, em seguida, pressione a seta <kbd>↓</kbd> algumas vezes até voltar para onde começou.***

Você também pode revisar seus comandos recentes com o comando `history`. Basta digitar: 

```bash
$ history
```

Esta ação irá retornar uma lista numerada de comandos (do mais antigo para o mais recente), incluindo o comando `history` que você acabou de executar!

> **Curiosidade:** Por padrão, apenas uma quantidade limitada de comandos será armazenada e exibida com o comando `history`, mas você pode aumentar ou reduzir este número. Essa alteração envolve a modificação de variáveis do Shell, sobre as quais estudaremos no futuro.

> **NOTA:** Até o momento, executamos apenas comandos muito curtos que têm poucos ou nenhum argumento. Seria mais rápido reescrever do que verificar o histórico. No entanto, quando você começar a executar análises na linha de comando, verá que os comandos são mais longos e mais complexos, e o comando `history` será muito útil! Entretanto, como boa prática, não dependa do histórico para ter uma memória dos comandos executados!

#### Cancelar um comando

Às vezes, ao digitar um comando, você percebe que não deseja continuar ou executar a linha atual. Em vez de deletar tudo que você digitou (o que pode tomar algum tempo), você pode cancelar rapidamente a linha atual e iniciar um novo prompt com <kbd>Ctrl</kbd> + <kbd>C</kbd>.

Para testar esse recurso, escreva algumas palavras aleatórios no prompt de comando e então pressione `Ctrl` + `C`, oberserve o que ocorre:

```bash
$ digitar palavras aleatorias e depois usar o atalho `Ctrl + C`
```

**Outros atalhos úteis relacionados a comandos**

- <kbd>Ctrl</kbd> + <kbd>A</kbd> o leva ao início do comando que você está escrevendo.
- <kbd>Ctrl</kbd> + <kbd>E</kbd> leva você ao final do comando digitado.

****

**Exercício 2**

1. Ao verificar a saída do comando `history`, quantos comandos você digitou até agora?
2. Use a tecla de seta para cima <kbd>↑</kbd> para verificar o comando que você digitou antes do comando `history`. O que ele significa? Isso faz sentido?
3. Digite vários caracteres aleatórios no prompt de comando. Você pode mover o cursor para o início com <kbd>Ctrl</kbd> + <kbd>A</kbd>? Em seguida, você pode mover o cursor até o final com <kbd>Ctrl</kbd> + <kbd>E</kbd>? O que acontece quando você usa <kbd>Ctrl</kbd> + <kbd>C</kbd>?

****

## Sumário: Comandos, opções e atalhos de teclado

```
~           # home dir (diretório base do usuário)
.           # current dir (diretório corrente ou atual)
..          # parent dir (diretório pai)
*           # wildcard (caractere-coringa)
ctrl + c    # Cancela o comando atual (apaga todo o comando)
ctrl + a    # Move o cursor para o início do comando (linha)
ctrl + e    # Move o cursor para o final do comando (linha)
history     # Imprime os últimos comandos digitados pelo usuário
```

> Imprima e tenha sempre à mão uma folha de dicas (_cheatsheet_) para relembrar ou verificar rapidamente os comandos que deseja executar.

Abaixo são apresentadas duas tabelas com opções e atalhosúteis para serem utilizados em conjunto com os comandos `ls` e `cd`.


**Tabela 2:** Lista de opções para o comando `ls`
|     Comando    |                                  Definição                                |
|:--------------:|:-------------------------------------------------------------------------:|
|     ls -F      |     Lista e indica os tipos de arquivos   (comum, programa, diretório)    |
|     ls -a      |     Lista todos os arquivos e diretórios   (mesmo ocultos)                |
|     ls -l      |     Lista e exibe informações detalhadas   (formato longo)                |
|     ls -h      |     Lista e exibe informações detalhadas e   inteligíveis (human)         |
|     ls -r      |     Lista na ordem reversa                                                |
|     ls -t      |     Lista pelo horário de modificação                                     |
|     ls -S      |     Lista pelo tamanho                                                    |


**Tabela 3:** Exemplos de uso do comando `cd` e `atalhos`
|         Comando       |                                Definição                               |
|:---------------------:|:----------------------------------------------------------------------:|
|     cd <folder>       |     Muda para o diretório indicado                                     |
|     cd ..             |     Muda para o diretório pai (ou superior)                            |
|     cd                |     Retorna ao diretório home                                          |
|     cd -              |     Retorna ao diretório anterior                                      |
|     cd ../<folder>    |     Sobe para o diretório pai e então entra   no diretório indicado    |
|     cd ../..          |     Sobe dois níveis (diretório “pai do   pai”)                        |
|     cd /<folder>      |     Muda para um diretório usando caminho   absoluto                   |
|     cd <folder>/      |     Muda para um diretório usando caminho   relativo                   |

 
  
---
**Bibliografia / Fontes**

BASH programming: https://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html 

BASH for Genomics: https://angus.readthedocs.io/en/2016/GenomicsShell.html 

GNU/Linux tutorials: https://www.debian.org/doc/manuals/debian-reference/ch01.en.html 

Jargas, Aurelio Marinho. Shell Script Profissional. São Paulo : Novatec Editora, 2008.

Markdown Tables Generator: https://www.tablesgenerator.com/markdown_tables#  
  
*The materials used in this lesson were derived from work of members of the teaching team at the [Harvard Chan Bioinformatics Core (HBC)](http://bioinformatics.sph.harvard.edu/). These are open access materials distributed under the terms of the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.*

* *The materials used in this lesson were derived from work that is Copyright © Data Carpentry (http://datacarpentry.org/). 
All Data Carpentry instructional material is made available under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).*
* *Adapted from the lesson by Tracy Teal. Original contributors: Paul Wilson, Milad Fatenejad, Sasha Wood and Radhika Khetani for Software Carpentry (http://software-carpentry.org/)*

