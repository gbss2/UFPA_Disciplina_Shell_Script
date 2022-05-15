## Examinando arquivos

Agora que já aprendemos como nos movimentar pela estrutura de arquivos do GNU/Linux e a investigar o conteúdo das pastas, precisamos aprender como examinar o conteúdo dos arquivos. Em nossos computadores pessoais, isso pode ser tão simples quanto clicar duas vezes sobre o ícone de um arquivo. Entretanto, quando estamos trabalhando no prompt de comando, o recurso de apontar e clicar do mouse não está disponível. Desta forma, para ver o conteúdo de arquivos através da interface de linha de comando, necessitamos aprender alguns comandos novos.


### O comando `cat`

O comando `cat` é um programa que lê arquivos sequencialemente, concatenando o conteúdo destes e imprimindo o resultado na tela (**_standard output_** ou **_stdout_**).

> O `stdout` é um dos três recursos utilizados para gerenciar a entrada e saída de dados de programas no **_shell_**. O `stdout` permite a impressão dos resultados de um programa diretamente no prompt de comando. O `stderr` ou **_standard_error_** é utilizado para a impressão de mensagens de erro ou diagnóstico do programa. E, por fim, o `stdin` ou **_standard input_** permite a leitura de dados de entrada (**_input_**) e o redirecionamento destes para o programa.

Ainda que a função primária do comando `cat` seja concatenar arquivos, a habilidade deste programa em imprimir todas as linhas de um arquivo na tela é bastante utilizada para investigar o conteúdo de arquivos. E esse recurso pode ser utilizado para um único arquivo! Podemos testar o funcionamento deste programa imprimindo o conteúdo do arquivo `~/unix_lesson/other/sequences.fa`. Para tal, digite o comando `cat` seguido pelo nome do arquivo (se necessário, inclua o caminho até o arquivo:

```bash
$ cat ~/unix_lesson/other/sequences.fa
```
O comando `cat` irá imprimir todo o conteúdo do arquivo `sequences.fa` na tela. 

**Qual o conteúdo desse arquivo?**

```bash

>SRR014849.1 EIXKN4201CFU84 length=93 
GGGGGGGGGGGGGGGGCTTTTTTTGTTTGGAACCGAAAGGGTTTTGAATTTCAAACCCTTTTCGGTTTCCAACCTTCCAAAGCAATGCCAATA

>gi|340780744|ref|NC_015850.1| Acidithiobacillus caldus SM-1 chromosome, complete genome
ATGAGTAGTCATTCAGCGCCGACAGCGTTGCAAGATGGAGCCGCGCTGTGGTCCGCCCTATGCGTCCAACTGGAGCTCGTCACGAG
TCCGCAGCAGTTCAATACCTGGCTGCGGCCCCTGCGTGGCGAATTGCAGGGTCATGAGCTGCGCCTGCTCGCCCCCAATCCCTTCG
TCCGCGACTGGGTGCGTGAACGCATGGCCGAACTCGTCAAGGAACAGCTGCAGCGGATCGCTCCGGGTTTTGAGCTGGTCTTCGCT
CTGGACGAAGAGGCAGCAGCGGCGACATCGGCACCGACCGCGAGCATTGCGCCCGAGCGCAGCAGCGCACCCGGTGGTCACCGCCT
CAACCCAGCCTTCAACTTCCAGTCCTACGTCGAAGGGAAGTCCAATCAGCTCGCCCTGGCGGCAGCCCGCCAGGTTGCCCAGCATC
CAGGCAAATCCTACAACCCACTGTACATTTATGGTGGTGTGGGCCTCGGCAAGACGCACCTCATGCAGGCCGTGGGCAACGATATC
CTGCAGCGGCAACCCGAGGCCAAGGTGCTCTATATCAGCTCCGAAGGCTTCATCATGGATATGGTGCGCTCGCTGCAACACAATAC
CATCAACGACTTCAAACAGCGTTATCGCAAGCTGGACGCCCTGCTCATCGACGACATCCAGTTCTTTGCGGGCAAGGACCGCACCC

>gi|129295|sp|P01013|OVAX_CHICK GENE X PROTEIN (OVALBUMIN-RELATED)
QIKDLLVSSSTDLDTTLVLVNAIYFKGMWKTAFNAEDTREMPFHVTKQESKPVQMMCMNNSFNVATLPAE

```

> Esse arquivo contém sequências no formato [**FASTA**](https://en.wikipedia.org/wiki/FASTA_format).

### O comando `less`

O comando `cat` é muito eficiente para examinar arquivos pequenos, entretanto, a leitura de arquivos maiores pode se tornar problemática. E, no contexto da bioinformática, a situação mais comum será a manipulação de arquivos grandes. Como exemplo, podemos verificar o tamanho dos arquivos **FASTQ** presentes na pasta `raw_fastq`. Para tal, vamos utilizar o comando `ls` em conjunto com as opções `-lh`.

> A opção `-l` imprime a lista de arquivos no formato longo (com informações mais detalhadas) e a opção `-h` traduz essas informações num formato mais conveniente para a leitura humana (por ex. o tamanho dos arquivos é expresso usando prefixos métricos - 1K 234M 2G). 

```bash
$ ls -lh ~/unix_lesson/raw_fastq
```

Na quarta coluna você verá o tamanho de cada arquivo e irá notar que eles são grandes (por volta de algumas dezenas de Mb). Dessa forma, examinar o conteúdo desses arquivos com o comando `cat` não é adequado e, para isso, recorreremos ao comando `less`.

Acesse a pasta `raw_fastq` e corra o seguinte comando: 

```bash
$ less Mov10_oe_1.subset.fq
```
Ao invés de imprimir o conteúdo do arquivo, o comando `less` abre um **buffer** que permite ao usuário navegar pelo conteúdo do arquivo. Esse tipo de interface é semelhante àquela vista ao utilizar o comando `man`. Isso ocorre pois o comando `man` utiliza o comando `less` para abrir a documentação dos programas. Desta maneira, as teclas utilizadas para se movimentar pelo arquivo são as mesmas que utilizamos durante o uso do comando `man`. A seguir, são apresentados alguns atalhos adicionais para navegar por um arquivo usando `less`.

<span class="caption">Atalhos para o uso do `less`</span>

| Atalho           | Ação                   |
| ---------------- | ---------------------- |
| <kbd>SPACE</kbd> | Avançar pelo arquivo   |
| <kbd>b</kbd>     | Retroceder             |
| <kbd>g</kbd>     | Move para o início     |
| <kbd>G</kbd>     | Move para o fim        |
| <kbd>q</kbd>     | Sair do aplicativo     |

> O programa `less` é uma versão melhorada do `more`, outro programa usual na visualização de arquivos.

Teste o uso dos atalhos para se mover ao longo do arquivo **FASTQ**.

#### Busca de arquivos usando o `less`

O comando `less` também proporciona maneiras de realizar buscas em arquivos.

Digite a tecla <kbd>/</kbd> para iniciar a busca, você verá que uma `/` irá aparecer ao final do buffer. Agora, você pode digitar a sequência de caracteres (**_string_**) que deseja buscar no arquivo e depois apertar <kbd>ENTER</kbd>. A interface então irá se mover para mostrar a localização da _string_ e esta será realçada. Se você novamente digitar <kbd>/</kbd> seguida por <kbd>ENTER</kbd>, `less` irá repetir a busca anterior.

> **_string_** é um tipo de dado usado na programação para representar textos. É composta por uma cadeia de caracteres que também podem conter números e espaços.

O comando `less` pesquisa a partir do local atual e segue em frente. Por exemplo, vamos procurar a sequência `GAGACCC` em nosso arquivo. Note que vamos direto para essa sequência.

Se você iniciar uma pesquisa quando estiver no final do arquivo, `less` não encontrará o padrão desejado. Você precisa ir para o início do arquivo e pesquisar.

Para sair do buffer, pressione <kbd>q</kbd>. Existem outros comandos mais sofisticados para pesquisar em seu arquivo (e abordaremos isso mais tarde), mas essa pesquisa é útil para uma verificação rápida. Você pode pensar nisso como sendo análogo a usar a tecla <kbd>Ctrl-F</kbd> ao pesquisar em seu computador.

### Os comandos `head` e `tail`

Há outra maneira de examinarmos os arquivos, mas apenas parte deles. Em particular, se queremos apenas ver o início ou o fim do arquivo para ver como ele está formatado.

Os comandos são `head` e `tail` e eles permitem que você apenas veja o início e o fim de um arquivo, respectivamente.

```bash
$ head Mov10_oe_1.subset.fq
```

```bash
$ tail Mov10_oe_1.subset.fq
```

Por padrão, as 10 primeiras ou últimas linhas serão impressas na tela. A opção `-n` pode ser usada com qualquer um desses comandos para especificar o número `n` linhas a ser exibido. Por exemplo, vamos imprimir a primeira e a última linha do arquivo `Mov10_oe_1.subset.fq`:


```bash
$ head -n 1 Mov10_oe_1.subset.fq

$ tail -n 1 Mov10_oe_1.subset.fq
```

*** 

**Exercício 1**

1. Acesse o diretório `genomics_data`. Você pode fazer isso usando um caminho completo ou relativo.
2. Use o comando `less` para abrir o arquivo `Encode-hesc-Nanog.bed`.
3. Procure a string `chr11`; você verá todas as instâncias no arquivo destacadas.
4. Permanecendo no buffer do `less`, use o atalho para chegar ao final do arquivo. Quais são as três linhas realçadas no final do arquivo onde você vê `chr11` realçado.
5. Saia do buffer `less` e volte ao prompt de comando.
6. Imprima na tela as últimas 5 linhas do arquivo `Encode-hesc-Nanog.bed`. O que você vê como o **_standard_output_** (saída) no prompt de comando?

***

## Criando e escrevendo em arquivos

Aprendemos como interagir com arquivos que já existem, mas e se quisermos escrever e/ou criar nossos próprios arquivos? Obviamente, não é viável digitar os dados para um arquivo FASTA, mas você verá à medida que avançamos que há muitas situações nas quais precisaríamos escrever/criar um arquivo ou editar um arquivo existente.

Para criar ou editar arquivos, precisaremos usar um **editor de texto**. Quando dizemos "editor de texto", realmente queremos dizer "texto": esses editores podem
trabalhar apenas com dados de caracteres simples, e não com tabelas, imagens ou qualquer outra mídia. Os tipos de editores de texto disponíveis geralmente podem ser agrupados em duas categorias: **editores de texto de interface gráfica do usuário (GUI)** e **editores de linha de comando**.

### Editores de texto com interface gráfica (GUI)

Uma GUI é uma interface que possui botões e menus com os quais pode interagir, por exemplo, você pode clicar para executar comandos ou pode mover-se pela interface apenas apontando e clicando. Você pode conhecer editores de texto GUI, como o [Notepad++ ](http://notepad-plus-plus.org/) e o [TextPad](https://www.textpad.com/home), os quais permitem escrever e editar documentos de texto simples. Esses editores geralmente são fáceis e intuitivos para usar, e possuem recursos para pesquisar texto, extrair texto ou mesmo destacar a sintaxe de várias linguagens de programação. São ótimas ferramentas, mas como são baseadas em 'apontar e clicar', não podemos usá-las com eficiência a partir da linha de comando.

#### Editores de linha de comando

Ao trabalhar remotamente, precisamos de um editor de texto que funcione a partir da interface de linha de comando. Com editores de linha de comando você deve navegar usando as teclas de seta e os atalhos, já que você não tem a opção de 'apontar e clicar'. Alguns editores populares incluem [Emacs](http://www.gnu.org/software/emacs/), [Vim](http://www.vim.org/), ou um editor gráfico como [Gedit]( http://projects.gnome.org/gedit/). Esses são editores que geralmente estão disponíveis para uso em clusters de computação de alto desempenho. Há também editores mais simples disponíveis para uso no cluster (por exemplo, [nano](http://www.nano-editor.org/)), mas tendem a ter funcionalidade limitada e nem sempre estão instalados no servidor remoto.

### Introdução ao Vim

Para escrever e editar arquivos, vamos aprender a usar um editor de texto chamado **'Vim'**. O Vim é um editor de texto robusto com amplos recursos para edição de texto. No entanto, neste momento, vamos nos concentrar em **explorar funções mais básicas**.

> #### Como posso me lembrar de todos os atalhos do Vim?
> Para ajudá-lo a se lembrar de alguns dos atalhos de teclado e permitir que você explore funcionalidades adicionais por conta própria, faço uso desta folha de dicas [cheatsheet Vim](https://github.com/hbctraining/In-depth-NGS-Data-Analysis-Course/blob/master/resources/VI_CommandReference.pdf). Faça o download para o seu computador ou imprima uma cópia, é um recurso útil ao interagir com o Vim.

### Interface do programa Vim

É possível criar um documento executando um editor de texto (por exemplo, o `vim`) e atribuir um nome ao documento que deseja criar.

Acesse o diretório `~/unix_lesson/other` e crie um documento chamado `draft.txt` usando o comando `vim`:

```bash
$ cd ~/unix_lesson/other
	
$ vim draft.txt
```

**Observe o `"draft.txt" [New File]` na seção inferior esquerda da tela.** Isso informa que você criou um novo arquivo no vim.

> **ATENÇÃO:** Caso crie um novo arquivo com o comando acima, mas saia sem salvar as alterações, o novo arquivo não será efetivamente criado

### Modos Vim

O Vim dispõe de **_dois modos básicos_** que permitem criar e editar documentos:

- **_command mode (modo padrão):_** recurso que te permitirá inserir os comandos desejados, por exemplo, salvar, sair, buscar, substituir, dentre outros.

- **_insert mode:_** recurso que permitirá que você escreva e edite texto.

- **_visual mode:_** recurso usado para destacar e editar texto em massa (não será abordado nesse curso).

Após a criação de um arquivo, o vim entra automaticamente no **_modo de comando**_. Para alterar para o **_modo de edição_** digite <kbd>i</kbd>. **Observe a mensagem `--INSERT--` no canto inferior esquerdo da tela.** Digite algumas linhas de texto:

Interface do programa Vim ao criar um arquivo (modo de comando):
![Interface Vim ao iniciar - Modo de comando](https://user-images.githubusercontent.com/17560094/168376633-fd93996a-18a9-4623-8959-38a0d30c7d19.png)


Interface do programa Vim no modo de edição (após digitar <kbd>i</kbd> a partir do modo de comando)
![Interface Vim - Modo de edição](https://user-images.githubusercontent.com/17560094/168376783-642d8fa4-a040-4bae-ac81-42b81521f985.png)


Interface do programa Vim no modo de edição (após digitar <kbd>i</kbd> a partir do modo de comando)
![Interface Vim - Texto digitado](https://user-images.githubusercontent.com/17560094/168380565-2af8c5e7-08a7-4a20-8701-5b4c071a97a6.png)


Após terminar de digitar, **pressione <kbd>esc</kbd> para voltar ao modo de comando.**

**Note que `--INSERT--` desapareceu da parte inferior da tela.**

> ### Revisão dos modos de Vim
> 
> | Tecla                 | Ação                   |
> | ----------------------|---------------------------------------------- |
> | <kbd>i</kbd>          | Modo de edição - Escrever e editar textos     |
> | <kbd>esc</kbd>        | Modo de comando - Emitir comandos / atalhos   |
> 

### Salvando as alterações e Saindo do programa Vim

Para **_"write to file"_** ou salvar as modificações feitas no arquivo, **digite <kbd>:w</kbd>** no modo de comando. Os comandos digitados podem ser vistos no canto inferior esquerdo da tela. (Note que o comando é composto por `:` e `w`.

![image](https://user-images.githubusercontent.com/17560094/168382172-c6e741d0-5ff5-4291-a1ad-f23581de226d.png)

Depois de salvar o arquivo, o número total de linhas e caracteres presentes aparacerá na seção inferior esquerda da tela.

![image](https://user-images.githubusercontent.com/17560094/168382494-c61a5dcf-fdec-4a05-9b79-ea5ccf5f6137.png)

De uma maneira mais concisa, podemos **salvar e sair** de uma só vez **digitando <kbd>:wq</kbd>**. Ao fazer isso, você deve ter saído do vim e retornado ao seu prompt de comando.

Para editar o arquivo `draft.txt` recém-criado, você pode abri-lo novamente com o vim: `vim draft.txt`. Primeiro, mude para o **modo de edição** e digite um texto adicional. Caso queira **sair sem salvar**, entre no **modo de comando** pressionando a tecla <kbd>esc</kbd> e depois **digite <kbd>:q!</kbd>**.

![image](https://user-images.githubusercontent.com/17560094/168383468-dc52ac33-b93c-49d1-af19-7d2ee8fb34c1.png)

> ### Revisão das ações de salvar e sair
> 
> | Tecla (no modo de comando) | Ação            |
> | -------------------------- | ----------------|
> | <kbd>:w</kbd>              | Salvar edições  |
> | <kbd>:wq</kbd>             | Salvar e sair   |
> | <kbd>:q!</kbd>             | Sair sem salvar |


### Atalhos no Vim

Embora não possamos apontar e clicar para navegar pelo documento, podemos usar as teclas de seta para se mover. No entanto, navegar com as teclas de seta pode ser muito lento. Com o intuito de otimizar o uso, o Vim disponibiliza diversos atalhos (que são completamente não intuitivos, mas muito úteis à medida que você se acostuma com eles).

Crie um novo arquivo chamado `code_poetry.txt` usando `vim`. Copie o texto abaixo, entre no *modo de inserção* e cole usando o comando `p`:

```perl
BEFOREHAND: close door, each window & exit; wait until time.
    open spellbook, study, read (scan, select, tell us);
write it, print the hex while each watches,
    reverse its length, write again;
    kill spiders, pop them, chop, split, kill them.
        unlink arms, shift, wait & listen (listening, wait),
sort the flock (then, warn the "goats" & kill the "sheep");
    kill them, dump qualms, shift moralities,
    values aside, each one;
        die sheep! die to reverse the system
        you accept (reject, respect);
next step,
    kill the next sacrifice, each sacrifice,
    wait, redo ritual until "all the spirits are pleased";
    do it ("as they say").
do it(*everyone***must***participate***in***forbidden**_*_*_*).
return last victim; package body;
    exit crypt (time, times & "half a time") & close it,
    select (quickly) & warn your next victim;
AFTERWORDS: tell nobody.
    wait, wait until time;
    wait until next year, next decade;
        sleep, sleep, die yourself,
        die at last
```
| ![Black Perl](https://en.wikipedia.org/wiki/Black_Perl) |
|:--:|
| <b>Poema escrito em código Perl (autor desconhecido)</b>|


Após colar o texto, você pode exibir a numeração de cada linha alternando para o modo de comando e digitando o comando `:set number`. Mais tarde, se você optar por removê-los, poderá redefinir esse recurso usando `:set nonumber`. Note que agora é `nonumber`.


| Tecla (modo de comando)         | Ação                         |
| ------------------------------- | ---------------------------- |
| <kbd>:set number</kbd>          | Aciona o número das linhas   |
| <kbd>:set nonumber</kbd>        | Desabilita o número de linhas|

**Salve o documento.** 

Verifique se está **no modo de comando**, vamos aprender algumas instruções sobre como nos mover pelo arquivo `code_poetry.txt`.

**Movendo-se pelo arquivo**

| Tecla (modo de comando) | Ação                              |
| ----------------------- | --------------------------------- |
| <kbd>gg</kbd>           | Mover-se para o início do arquivo |
| <kbd>G</kbd>            | Mover-se para o final do arquivo  |
| <kbd>$</kbd>            | Mover-se para o final da linha    |
| <kbd>0</kbd>            | Mover-se para o início da linha   |
| <kbd>w</kbd>            | Mover-se para a próxima palavra   |
| <kbd>b</kbd>            | Mover-se para a palavra anterior  |

Tente particar alguns desses comandos e depois saia sem salvar as alterações.

**Editando o arquivo**

| Tecla (modo de comando)             | Ação                                                        |
| ----------------------------------- | ----------------------------------------------------------- |
| <kbd>dw</kbd>                       | Deletar palavra                                             |
| <kbd>dd</kbd>                       | Deletar linha                                               |
| <kbd>u</kbd>                        | Desfazer                                                    |
| <kbd>Ctrl + r</kbd>                 | Refazer                                                     |
| <kbd>/*pattern*</kbd>               | Buscar por padrão (*n/N* move para as próximas ocorrências) |
| <kbd>:%s/*search*/*replace*/g</kbd> | Busca por um padrão e substitui todas as ocorrências        |

*** 

**Exercício 2**

Alguns comandos básicos do vim foram mostrados, mas a prática é a chave para se sentir confortável com o programa. Neste sentido, vamos exercitar o que aprendemos tentando resolver um pequeno desafio.

1. Abra o arquivo `code_poetry.txt` e exclua a palavra "spellbook" da linha #2.
2. Saia sem salvar.
3. Abra o `code_poetry.txt` novamente e substitua todas as ocorrências de "wait" por "continue".
4. Exclua: "AFTERWORDS: tell nobody."
5. Salve o arquivo.
6. Desfaça sua exclusão anterior.
7. Refaça sua exclusão anterior.
8. Apague as primeiras e últimas palavras das duas primeiras linhas.
9. Salve o arquivo.
10. Faça uma cópia local (no seu computador) e poste no Classroom.


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
