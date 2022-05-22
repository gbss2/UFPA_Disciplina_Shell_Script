
## Permissões

O GNU/Linux controla quem pode ler, altetar e executar arquivos conferindo *permissões* a arquivos e diretórios. Devido ao caráter multiusuário dos sistemas operacionais GNU/Linux, a proteção dos dados de cada usuário é vital. Isso é alcançado por meio de **permissões**.

Nesta atividade, aprenderemos sobre como essas permissões são definidas e como elas podem ser modificadas.

Para começar, cada arquivo e diretório em um sistema operacional GNU/Linux pertence a um proprietário (referido como `usuário`) e a um `grupo`. Além do conteúdo de cada arquivo, o sistema operacional armazena as informações sobre o usuário e o grupo que o possui, ou seja, os **metadados** de um determinado arquivo.

**Usuários de um sistema GNU/Linux multiusuário podem pertencer a vários grupos.**

Vamos ver a que grupos pertencemos. Digite `groups` no prompt de comando.

```bash
$ groups
```

Todo usuário pertence ao menos a um grupo que recebe o mesmo nome do `user`. Além deste grupo, você também participa do grupo `2022shell`, este grupo criado para os alunos da disciplina de Shell Script permitiu a todos copiar os arquivos localizados no diretório compartilhado `/data/2022_shell_script/unix_lesson/`.

O modelo usuário e grupo significa que para cada arquivo e diretório **todo usuário naquele sistema se enquadra em uma das 3 categorias:**

* `user` ou `u`: **o proprietário**
* `group` ou `g`: **membro do grupo ao qual o arquivo/diretório pertence**
* `others` ou `o`: **todos os outros**

Para cada um desses três níveis de permissão, o sistema controla se as pessoas naquela categoria podem ler o arquivo (`r`), gravar alterações no arquivo (`w`) ou executar o arquivo (ou seja, executar o código escrito nele ) (`x`). Mais informações sobre esse aspecto das permissões serão apresentadas posteriormente nesta atividade.

Como visto anteriormente, o comando `ls` acompanhado pelo argumento `-l` retorna a lista dos arquivos em diretório no formato "longo", ou seja, com informações mais detalhadas. A partir da listagem dos arquivos contidos no diretório `unix_lesson`:

```bash
ls -l ~/unix_lesson/

drwxr-xr-x 2 giordano giordano 4096 May 10 15:34 exercise1
drwxr-xr-x 2 giordano giordano 4096 May  6 20:40 genomics_data
drwxr-xr-x 2 giordano giordano 4096 May 15 21:30 other
drwxr-xr-x 2 giordano giordano 4096 May 11 12:21 raw_fastq
-rw-r--r-- 1 giordano giordano  377 May  6 20:40 README.txt
drwxr-xr-x 2 giordano giordano 4096 May  6 20:40 reference_data
```

Vamos analisar as colunas desse _output_, da direita para a esquerda: 

1. Nomes dos arquivos/diretórios
2. Horário e data da última modificação. Os sistemas de backup e outras ferramentas usam essas informações de várias maneiras, mas você pode usá-las para saber quando você (ou qualquer outra pessoa com permissão) alterou um arquivo pela última vez.
3. Tamanho do arquivo em bytes.
4. Nome do grupo que possui o arquivo.
5. Nome de usuário do proprietário do arquivo.
6. Número de _hard links_ do arquivo (não é importante neste momento).
7. Permissões para o arquivo

Quem é o proprietário dos arquivos neste diretório? A qual grupo os arquivos pertencem?

Basicamente, será você (o ID da sua conta) listado como proprietário e grupo, e isso geralmente é a atribuição dos arquivos e pastas em seu diretório pessoal. Essencialmente, quando um novo usuário é criado em um sistema Unix, um grupo com o mesmo nome é criado e os arquivos pessoais desse usuário também são "propriedade" do grupo desse usuário.

### Interpretando os símbolos de permissões

Vamos analisar os símbolos de permissão na primeira coluna para o arquivo `README.txt`:

```bash
-rw-rw-r--
```

* O primeiro caractere indica o tipo de arquivo. Entre os diferentes tipos, um traço inicial (`-`) significa um arquivo regular, enquanto um `d` indica um diretório.

> No exemplo acima, `-` indica que o README.txt é um arquivo normal.

* Os próximos 9 caracteres são geralmente uma combinação de `r`, `w` e `x`, onde:

<kbd>r</kbd> = permissão de leitura

<kbd>w</kbd> = permissão de gravação/edição
 
<kbd>x</kbd> = permissão de execução (executar um script/programa ou percorrer um diretório).

O **primeiro trio** são as permissões para o **usuário proprietário (`u`)** do arquivo. Aqui, o proprietário pode ler e editar o arquivo: `rw-`. Quando uma permissão está desativada, vemos um traço, então `rw-` significa "ler e escrever, mas não executar".

```bash
rw-
```

O **segundo trio** mostra as **permissões do grupo (`g`)**. Aqui, o grupo pode ler e editar o arquivo. (Neste caso, o grupo e o proprietário são os mesmos, então faz sentido que seja o mesmo para ambos.)

```bash
rw-
```

O **último trio** nos mostra o que **todo mundo (`o`)** pode fazer. Neste caso, é `r--`, então **todos os outros** no sistema podem apenas ler o conteúdo do arquivo.

> "Todo mundo" se refere a outros usuários no sistema que não são os proprietários do arquivo ou estão no grupo ao qual o arquivo pertence.
 
```bash
r--
```

> #### Permissões de execução
> Não vemos a permissão de execução definida aqui, pois não estamos trabalhando com arquivos executáveis. Para ver um exemplo de um arquivo que é realmente executável, tente `ls -l /bin/ls`.
>
> Às vezes o `x` é substituído por outro caractere, mas está além do escopo do curso. Você pode [obter mais informações aqui](https://en.wikipedia.org/wiki/File_system_permissions#Notation_of_tradicional_Unix_permissions), se estiver interessado.
>

#### As permissões são interpretadas da mesma maneira para diretórios?

Se analisarmos as permissões dos diretórios (por exemplo, `drwxrwsr-x`), podemos notar que o `x` está presente. O que isso significa, dado que um diretório não é um programa ou um arquivo executável?

Neste caso, o `x` concede o direito de *percorrer* o diretório, mas não de examinar (ou listar) seu conteúdo. Isso está além do curso, mas observe que você pode dar a alguém acesso a um arquivo que está dentro de um diretório *sem permitir que ele veja quais outros arquivos existem nos subdiretórios que fazem parte do caminho*.

### Alterando as permissões

Para alterar as permissões, usamos o comando `chmod` ("_change mode_") e os seguintes argumentos:

* Qual nível de permissão será alterado? ("usuário/_user_" <kbd>u</kbd>, "grupo/_group_" <kbd>g</kbd> ou "outro/_other_" <kbd>o</kbd>)
* Estamos adicionando (<kbd>+</kbd>) ou removendo permissões (<kbd>-</kbd>)?
* Quais permissões (ou combinação delas) gostaríamos de adicionar/remover? ("ler/_read_" <kbd>r</kbd>, "editar/_write_" <kbd>w</kbd> e "executar/_execute_" <kbd>x</kbd>)

Vamos testar uma alteração tornando o arquivo README.txt **inacessível** para todos os usuários além de você e do grupo ao qual o arquivo pertence. Atualmente, todos os outros podem ler o arquivo.

```bash
$ ls -l ~/unix_lesson/README.txt

-rw-r--r-- 1 giordano giordano 377 May  6 20:40 /home/giordano/unix_lesson/README.txt
```

```bash
$ chmod o-r ~/unix_lesson/README.txt

$ ls -l ~/unix_lesson/README.txt

-rw-r----- 1 giordano giordano 377 May  6 20:40 /home/giordano/unix_lesson/README.txt
```
O `-` (hífen ou símbolo de menos) indica que estamos removendo uma permissão.

O caractere `o` representa os demais usários ("_others_").

Vamos desfazer a alteração para torná-lo legível para os demais usuários:

```bash
$ chmod o+r ~/unix_lesson/README.txt

$ ls -l ~/unix_lesson/README.txt

-rw-r--r-- 1 giordano giordano 377 May  6 20:40 /home/giordano/unix_lesson/README.txt
```
O `+` (símbolo de mais) indica que estamos adicionando uma permissão.

Para tornar esse arquivo executável, poderíamos usar o comando `chmod u+x`, onde o `u` sinaliza que estamos alterando a permissão para o proprietário do arquivo. Para alterar as permissões do "grupo", usaríamos a letra `g`. Para remover as permissões de edição para o grupo, o comando seria `chmod g-w`.

> O fato de um arquivo ter a permissão de execução não significa que ele contém ou é um programa. Podemos dar a permissão de execução ao arquivo `~/unix_lesson/raw_fastq/Irrel_kd_1.subset.fq` usando o comando `chmod`. Entretanto, dependendo do sistema operacional, tentar "executar" esse arquivo incorrerá em um erro, pois este não contém instruções ou comandos que o computador é capaz de interpretar.

****

**Exercício 1**

Se `ls -l myfile.php` retornar os seguintes detalhes:

```bash
-rwxr-xr-- 1 caro zoo 2312 2014-10-25 18:30 myfile.php
```
 
Qual das seguintes afirmações é verdadeira?
 
1. membros de caro (um grupo) podem ler, escrever e executar myfile.php
2. membros do zoo (um grupo) não podem executar myfile.php
3. caro (o proprietário) pode ler, escrever e executar myfile.php

****

## Variáveis de ambiente

As variáveis de ambiente são, resumidamente, variáveis que descrevem o ambiente no qual os programas são executados, os valores para estas variáveis são predefinidas de acordo com o computador ou servidor em que você está. Você pode redefini-las para personalizar a sessão.

Para ver a lista completa de variáveis de ambiente do seu sistema (ou do servidor ao qual estiver conectado), digite:

```bash
$ env
```

Esta pode ser uma longa lista! **No contexto do shell, as variáveis de ambiente geralmente estão todas em maiúsculas.**

Nesta atividade, vamos nos concentrar em duas variáveis de ambiente: `$HOME` e `$PATH`.

* `$HOME` define o caminho absoluto para o diretório inicial de um determinado usuário.
* `$PATH` define uma lista de diretórios para pesquisar ao procurar um comando/programa para executar.

As variáveis de ambiente, na maioria dos sistemas, são chamadas ou indicadas com um `$` antes do nome da variável, assim como uma variável regular. Vamos usar o comando `echo` para ver o que está armazenado em `$HOME`:

```bash
$ echo $ HOME
```

O caminho para o seu diretório inicial deve ser impresso. `$HOME` pode ser usado no lugar do `~`.

O conteúdo de `$HOME` é bem simples, que tal examinarmos o que está armazenado na variável `$PATH`:

```bash
$ echo $PATH

/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
```

O conteúdo agora é mais complexo! Vamos decompô-lo. A saída de `echo $PATH` é composta por uma lista de caminhos absolutos separados uns dos outros por um `:`.

Essa seria a lista em um formato mais legível:
* `/usr/local/bin`
* `/usr/bin`
* `/bin`
* `/usr/local/games`
* `/usr/games`

Cada caminho refere-se a um diretório, e alguns deles apresentam a pasta `bin`.

### O que são esses caminhos? E o que eles representam?

Esses são os diretórios nos quais o shell procurará (na mesma ordem em que estão listados) qualquer programa ou arquivo executável que você digitar no prompt de comando.

Por exemplo, quando digitamos `ls` no prompt de comando, o shell procura em cada um dos diretórios listados em `$PATH` por um arquivo executável chamado `ls`. Caso não encontrasse, você receberia a messagem de erro `command not found`. 

E como podemos saber em qual diretório se encontra o arquivo executável do programa que queremos executar?

Para qualquer comando executado no prompt de comando, você pode descobrir onde o arquivo executável está localizado usando o comando `which`.

```bash
$ which ls
```

Em qual diretório se encontra o arquivo executável do comando `ls`? Ele é um dos caminhos armazenados em `$PATH`?

Agora tente com alguns dos comandos de sua escolha:

```bash
$ which <command>
$ which <command>
```

Examine o diretório `/usr/bin/` e veja quais outros arquivos executáveis você reconhece. (Note que os arquivos executáveis serão listados em verde e terão o `*` após o nome).

```bash
$ ls -lF /usr/bin/
```

O diretório `/usr/bin` geralmente é onde os executáveis para os comandos mais comuns compartilhados pelos usuários são armazenados.

> Como apontado anteriormente, muitas das pastas listadas na variável `$PATH` são chamadas de `bin`. Isso se deve a uma convenção no GNU/Linux para chamar diretórios que contenham comandos (no formato ***binário***) de **`bin`**.

***

**Exercício 2**

Os diretórios listados pelas execuções do comando `which <programa de sua preferência>` também estavam presentes na variável `$PATH`?

***

#### Modificando variáveis de ambiente

Você pode criar uma variável de ambiente ou modificar o conteúdo de uma existente usando o comando `export`.

Por exemplo, se quiséssemos alterar o conteúdo de `$PATH` usando o comando `export`:
* A instrução seria `export PATH=$PATH:~/opt/bin` (**não execute isso**)
* Você sempre deve incluir a variável `$PATH` dentre os argumentos, pois:
  * Isso especifica que você deseja manter o conteúdo existente em `$PATH`.
  * Se você apagar todos os diretórios `bin`, nenhum de seus comandos funcionará mais!
* Use o ":" para separar cada um dos caminhos adicionados, **sem espaços**
* O novo caminho adicionado não deve terminar em `/`. Mesmo que `ls ~/opt/bin` e `ls ~/opt/bin/` forneçam os mesmos resultados, a variável `$PATH` não pode ter o `/` final.
* **A ordem importa!**
  * Se você executar `export PATH=$PATH:~/opt/bin`, o shell adicionará o diretório `~/opt/bin` ao **final da lista pré-existente** dentro da variável de ambiente `$PATH` .
  * Mas se você usar `export PATH=~/opt/bin:$PATH`, o mesmo diretório será adicionado ao início da lista. A ordem determina quais diretórios o shell irá buscar primeiro para encontrar o executável de um programa.
  
O comando `export $PATH` geralmente é usado para adicionar diretórios com programas que você deseja usar (por exemplo, aqueles programas instalados localmente no diretório pessoal).

Digamos que você costuma usar o comando `bowtie2` para alinhamento e ele existe em `/home/user/tools/dna/bowtie/`.

Se você deseja executar esta ferramenta, você terá que digitar:

```bash
$ /home/user/tools/dna/bowtie/bowtie2 <inputfile>
```

No entanto, se `/home/user/tools/dna/bowtie` for parte da variável `$PATH`, você pode simplesmente digitar:

```bash
$ bowtie2 <inputfile>
```

#### Entenda melhor o funcionamento interno do shell, no contexto de $PATH
 
Cada vez que você efetua login em um servidor ou inicia uma nova sessão, 2 **scripts especiais** são executados automaticamente em segundo plano. Você pode modificar esses scripts, portanto, se desejar personalizar seu ambiente (shell), poderá adicionar instruções personalizadas a esses scripts. Um uso comum é acrescentar um comando `export` que adiciona caminhos (_paths_) ao conteúdo pré-existente da variável de ambiente `$PATH`.

E quais são esses scripts especiais e onde eles estão localizados? Eles são chamados `.bashrc` e `.bash_profile`, e estão localizados em seu diretório pessoal. Você pode criá-los se eles não existirem, e o shell fará uso dos mesmos!

Verifique quais arquivos ocultos existem no diretório inicial usando o comando `ls` com o argumento `-a` e :

```bash
$ ls -al ~/
```

> Você pode usar o `vim` ou `nano` para modificar esses arquivos e/ou criá-los.

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
