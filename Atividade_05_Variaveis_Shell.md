
## Scripts em Shell

Até esse momento, você foi apresentado a vários comandos para explorar seus dados. Para demonstrar a função de cada comando, nós os executamos um de cada vez no prompt de comando. O prompt de comando é útil para testar comandos e também executar tarefas simples, como explorar e organizar arquivos (e diretórios). Quando estamos executando análises que exigem a execução de uma série de tarefas, existe uma maneira mais eficiente: utilizar `shell scripts`.

`Shell scripts` são **arquivos de texto que contêm os comandos que queremos executar**. Nesta atividade, apresentaremos um exemplo simples do uso de um **_shell script_**. Também serão apresentadas as **variáveis bash** e como utilizá-las em um exemplo de uso um pouco mais complexo.

### Um script simples

Finalmente vamos entender o que torna o **shell** um ambiente de programação tão poderoso. Para criar nosso primeiro script, vamos usar alguns dos comandos executados anteriormente e salvá-los em um arquivo para que possamos **reexecutar todas essas operações** quantas vezes desejarmos, digitando **um único comando**. Por razões históricas, uma lista de comandos salvos em um arquivo é chamado de `shell script`, mas nesse momento faremos algo mais modesto!

Ao trabalhar com o Shell ou na linha de comando, os arquivos podem ter qualquer (ou nenhuma) extensão (.txt, .tsv, .csv, etc.). Da mesma forma, para um **_shell script_**, você não precisa de uma extensão específica. No entanto, é uma boa prática dar aos scripts em shell a extensão `.sh`. Isso é útil para que qualquer um (inclusive você depois de algum tempo) consiga identificar facilmente que um determinado arquivo é um **_shell script_**.

Vá para o diretório `other` e crie um novo arquivo usando `vim`. Chamaremos nosso script de `listing.sh`:

```bash
$ cd ~/unix_lesson/other
$ vim list.sh
```

Este script de shell fará duas coisas:

1. Informar o diretório de trabalho atual
2. Listar o conteúdo do diretório

Já utilizamos comandos para realizar essas duas ações, então vamos alternar para o modo de edição no vim e adicionar os 2 comandos abaixo ao nosso script:

```bash
pwd
ls -l
```

Nesse ponto, já poderíamos salvar e sair, e o script funcionaria adequadamente. Mas, adicionaremos um pouco de "verbosidade" ao nosso script usando o comando `echo`. O comando `echo` é usado para imprimir uma _string_ que é passada como argumento. Este é um programa do bash que é usado principalmente para imprimir informações no prompt de comando ou em um arquivo.

> _Verbose Mode_ é uma opção que acompanha vários programas e disponibiliza informações adicionais detalhadas sobre a execução (por exemplo, quais arquivos estão sendo lidos ou escritos, quais recursos estão sendo usados, se há rotinas com falhas, dentre outras). É um modo muito útil para diagnóstico do funcionamento de um programa e disponibiliza informações úteis para o processo de _debugging_.

Insira a instrução `echo` antes de cada comando:

```bash
echo "Your current working directory is:"
pwd

echo "These are the contents of this directory:"
ls -l 
```

Nosso script está pronto! Agora você pode alternar para o _command mode_ no vim para salvar as modificações e sair do `vim`. Isso deve levá-lo de volta ao prompt de comando. Verifique e veja se o novo arquivo de script que você criou está no diretório:

```bash
$ ls -l
```

Para executar o `shell script` você pode usar o comando `bash` ou `sh`, seguido pelo nome do seu script:

```bash
$ sh listing.sh
```
> **O script funcionou como você esperava?**
>
> **Os comandos `echo` foram úteis?**

> É ainda possível executar Shell Scripts usando o atalho `./`. Para entender a fundo o funcionamento, precisaríamos de alguns conceitos sobre variáveis de ambiente, que serão abordados na atividade 7. Mas por agora, podemos dizer que esse atalho é uma forma de dizer ao sistema operacional para executar um aplicativo que está naquele diretório.

Este é um script muito simples, apenas para apresentá-lo ao conceito. Antes vermos mais scripts mais poderosos, vamos abordar alguns conceitos-chave para ajudá-lo a chegar lá.

**Exercício 1**

1. Abra o script `listing.sh` usando o vim. Adicione o comando que permite imprimir na tela o conteúdo do arquivo `Mov10_rnaseq_metadata.txt`.
2. Adicione uma instrução echo antes do comando acima, e esta deve informar ao usuário "This is information about the files in our dataset:"
3. Execute o script com as alterações. Descreva o código do script e a saída obtida após executá-lo.

## Variáveis do Bash

*Variável* é um conceito comum compartilhado por muitas linguagens de programação. **As variáveis são essencialmente um nome simbólico ou uma referência a algumas informações**. Variáveis são análogas a "recipientes", onde **as informações podem ser armazenadas, mantidas e modificadas** sem muita dificuldade. As variáveis devem ter um nome associado a elas e ao nos referirmos às informações contidas em uma variável, usamos o nome dela, e não aos dados armazenados nela.

Para criar uma variável no bash, você digita o nome da variável, seguido pelo sinal de igual `=` e acrescenta o valor que quer atribuir à variável. Lembre-se que o nome de uma variável não pode conter espaços, e não pode haver espaços em ambos os lados do sinal de igual.

Vamos começar criando uma variável chamada `num` e armazenar o número 25 dentro dela:

```bash
$ num=25
```

Depois de pressionar <kbd>Enter</kbd>, o prompt de comando retornorá para você. **Mas como saber se realmente criamos a variável bash?**

Uma maneira de examinar uma variável é através do comando `echo`. Como aprendemos anteriormente, este comando usa o argumento fornecido e o imprime no terminal. Se fornecermos `num` como argumento ao `echo`, ele será interpretado como uma _string_ "num". Para que `echo` exiba o conteúdo da variável e não o nome, precisamos **explicitamente usar um `$` na frente do nome da variável**:

```bash
$ echo $num
```

O número 25 deve ter sido impresso na tela. Você notou que quando criamos a variável, não havia necessidade de um `$`? Esta é a notação shell padrão (sintaxe) para definir e usar variáveis. Ao definir a variável (ou seja, definir o valor), você pode simplesmente digitá-la (<nome>=<valor>), mas para **recuperar o valor de uma variável, é obrigatório o uso do `$`!**

> **OBSERVAÇÃO:** variáveis não são entidades físicas como arquivos. Quando você cria arquivos, você pode usar `ls` para listar o conteúdo e ver se o arquivo existe. Ao criar variáveis, para listar todas as variáveis em seu ambiente você pode usar o comando `declare` com a opção `-p`. Você notará que, embora tenha criado apenas uma variável até agora, a saída de `declare -p` será mais do que apenas uma variável. Essas outras variáveis são chamadas de variáveis de ambiente e também serão discutidas na atividade 07.
>
> Se você usar `declare -p`, redirecione a saída para o comando `grep` seguido pelo nome da variável. Dessa forma, você listará apenas a variável de interesse.
>
> ```bash
> `declare -p | grep num`
> ```
>
 
### Usando variáveis como entrada para comandos

Até agora, pode não ser clara a utilidade de uma variável e por que precisamos dela. Um aspecto importante de uma variável é que o valor armazenado dentro dela pode ser usado como entrada para comandos. Para demonstrar isso vamos criar uma variável chamada `file`. E iremos adiconar o nome de um dos arquivos no diretório `raw_fastq` como o valor dessa variável.

```bash
$ file=Mov10_oe_1.subset.fq
```

Após pressionar <kbd>Enter</kbd>, você deve voltar ao prompt de comando. Vamos verificar o que está armazenado dentro de `file` usando o comando `echo`:

```bash
$ eco $file
```

Agora vamos usar a variável `file` como _input_ (entrada) para o comando `wc`:

```bash
$ wc -l $file
```

**O que você vê no terminal? O que você esperava que `wc -l` retornasse?**

O comando `wc -l` é usado para retornar o número de linhas em um arquivo. Aqui, fornecemos um arquivo, mas não obtivemos um número como resposta. Em vez disso, ocorreu um erro `wc: Mov10_oe_1.subset.fq: No such file or directory`. Isso aconteceu porque o arquivo não está presente no diretório corrente (atual). Para evitar esse tipo de situção, podemos fazer duas coisas:

* Fornecer o caminho (path) para o arquivo:

```bash
$ wc -l ~/unix_lesson/raw_fastq/$file
```
Ou

* Acessar o diretório onde o arquivo está localizado: 

```bash
$ cd ~/unix_lesson/raw_fastq
$ wc -l $file
```
  
Utilizando qualquer uma dessas opções, você deverá ter o resultado almejado.
  
> **OBSERVAÇÃO:** as variáveis criadas abrangem todo o sistema de um usuário e independem do local (diretório) onde o usuário se encontra. É por isso que podemos referenciá-las a partir de qualquer diretório. No entanto, elas estarão disponíveis apenas para a sua sessão atual. Se você sair da sessão e efetuar login novamente mais tarde, as variáveis que você criou não existirão mais.

 **Exercício 3**

1. Use a variável `$file` como entrada para os comandos `head` e `tail` e use a opção para exibir apenas quatro linhas. Indique os comandos utilizados e compare as linhas de cabeçalho (`@HWI`) obtidas por cada um dos comandos. 
2. Crie uma nova variável chamada `meta` e atribua a ela o valor `Mov10_rnaseq_metadata.txt`. Para responder as perguntas abaixo, use a variável `$meta` e mantenha-se no diretório atual. Indique os comandos que você utilizaria para:
	1. Exibir o conteúdo do arquivo usando `cat`.
	2. Recuperar apenas as linhas que contêm amostras normais. (*Dica: use o `grep`*).  

***

### Atribuindo a saída de um comando a uma variável

Ao criar _shell scripts_, as variáveis desempenham a função de armazenar informações que podem ser usadas posteriormente no script (uma ou mais vezes). O valor pode ser atribuído diretamente, como fizemos acima, designando à variável um valor numérico ou de caractere. Ou então, o valor assinalado pode ser a saída de um programa. Vamos demonstrar isso usando o comando `basename`.

O comando **`basename`** é usado para extrair o nome do arquivo sem o caminho (_path_). Para testarmos, retorne ao seu diretório base (_home_):

```bash
$ cd
```
  
E então utilize o comando `basename` em um dos arquivos FASTQ. Lembre-se de incluir o caminho:

```bash
$ basename ~/unix_lesson/raw_fastq/Mov10_oe_1.subset.fq
```

**O que foi retornado?**

O trecho `~/unix_lesson/raw_fastq/` é descartado/apagado, e é retornada toda a string após a última barra `/`, que representa o nome do arquivo `Mov10_oe_1.subset.fq`. Dessa forma, o comando `basename` é muito útil para **retornar o nome de arquivos**.

Agora, suponha que queremos **apagar a extensão do arquivo** (ou seja, remover `.fq` deixando apenas o nome do arquivo). Podemos fazer isso passando um argumento ao comando para especificar qual string de caracteres queremos apagar.

```bash
$ basename ~/unix_lesson/raw_fastq/Mov10_oe_1.subset.fq .fq
```

Perceba que agora, apenas a string `Mov10_oe_1.subset` é retornada. 

**Exercício 4**

1. Como você usaria o comando `basename` para retornar apenas `Mov10_oe_1`?
2. Use `basename` e o arquivo `Irrel_kd_1.subset.fq` como entrada. Como você obteria a string `Irrel_kd_1`?
  
***
  
  
  
  
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
