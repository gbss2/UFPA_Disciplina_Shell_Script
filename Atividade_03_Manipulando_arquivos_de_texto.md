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

Uma GUI é uma interface que possui botões e menus nos quais você pode clicar para executar comandos e mover-se pela interface apenas apontando e clicando. Você pode estar familiarizado com editores de texto GUI, como [Notepad++ ](http://notepad-plus-plus.org/) e [TextPad](https://www.textpad.com/home), que permite escrever e editar documentos de texto simples. Esses editores geralmente têm recursos para pesquisar texto com facilidade, extrair texto e destacar a sintaxe de várias linguagens de programação. São ótimas ferramentas, mas como são 'apontar e clicar', não podemos usá-las com eficiência a partir da linha de comando.




