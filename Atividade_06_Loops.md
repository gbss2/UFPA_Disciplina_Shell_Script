
## Loops

Usualmente, ao desenvolver um _pipeline_ de análises bioinformáticas, você irá executar vários comandos em sequência correspondendo a cada etapa do fluxo de análises. Nas atividades anteriores, aprendemos como criar um script que nos permite tornar este processo mais eficiente. Nesta atividade iremos aprender como aumentar ainda mais a eficiência - e a generalização - de nossos scripts de forma que todo o _pipeline_ possa ser executado para cada amostra de nosso conjunto de dados,

Loops (estruturas de repetição/iteração) é um conceito compartilhado por várias linguagens de programação, e sua implementação no bash é muito semelhante a outras linguagens.

A estrutura ou a sintaxe dos loops (*for*) no bash é a seguinte:

```bash
for (variable_name) in (list)
	do
	(command1 $variable_name)
	(command2 $variable_name)
	...
	....
	done
```

As palavras em **vermelho, são partes da estrutura do loop que permanecem constantes**. Ou seja, cada loop deverá ter as palavras: `for`, `in`, `do` e `done`. *Esta sintaxe/estrutura é praticamente imútavel.* Já o texto que entre essas palavras mudará dependendo das condições e ações desejadas.

### Como funcionam os loops?

Para entender como um loop funciona, vamos analisar passo-a-passo o exemplo a seguir:

```bash
$ cd ~/unix_lesson/raw_fastq/

$ for x in Mov10_oe_1.subset.fq Mov10_oe_2.subset.fq Mov10_oe_3.subset.fq
 do
   echo $x
   wc -l $x
 done
```

|   Componente do Loop                   |         Valor          |
| ---------------------------------------| ---------------------- |
| ***variable_name*** (nome da variável) |  `x`                   |
| **list** (lista de elementos)          | `Mov10_oe` FASTQ files |
|  Corpo - comandos a serem executados   | `echo` and `wc -l`     |


1. Quando iniciamos o loop, uma variável temporária é inicializada recebendo o valor do primeiro item da lista.

> **Isso não é visto explicitamente, mas a variável `x` recebeu o valor de `Mov10_oe_1.subset.fq`.**

2. A seguir, todos os comandos no corpo do loop (entre o `do` e o `done`) são executados. Normalmente, os comandos colocados aqui usarão a variável temporária como entrada. **Lembre-se, se você estiver usando o valor armazenado na variável, você precisa usar $ para referenciá-lo!** No exemplo, estamos executando dois comandos:

* `echo $x`: imprime o valor armazenado em `x`
* `wc -l $x`: retorna o número de linhas em `x`

3. Uma vez que esses dois comandos estejam completos, a variável temporária recebe um novo valor. Agora ele recebe o valor do segundo item da lista.

> **A variável `x` recebe um novo valor `Mov10_oe_2.subset.fq`.**

4. Mais uma vez, todos os comandos entre o `do` e o `done` são executados. Desta vez, usando o novo valor armazenado em `x`.

5. Por fim, a variável temporária assume o valor do terceiro item da lista.

> **A variável `x` recebe o valor de `Mov10_oe_3.subset.fq`.**

6. Novamente, todos os comandos entre `do` e `done` são executados usando o novo valor armazenado em `x`.

7. Assim que iteramos passando por todos os itens da lista, o loop é 'concluído'.

Essencialmente, **o número de itens na lista == número de vezes que o código será repetido**. Então, no nosso caso, tínhamos três arquivos listados e a série de comandos no corpo do loop foi repetida três vezes. Se tivéssemos fornecido todos os seis arquivos, a série de comandos seria repetida seis vezes.

> #### Executando loops no prompt de comando
> Usualmente, um loop **_for_** é escrito ao longo de várias linhas, e para executar essa estrutura de repetição no prompt de comando, comece digitando a instrução `for` e pressione a tecla Enter. Note que o prompt de comando não retornará e, em vez de um `$`, você verá um `>`. Isso indica que o shell reconheceu a inicialização de um loop **_for_** e está aguardando as demais instruções. Você pode continuar a digitar o código linha por linha. Ao digitar `done` e pressionar <kbd>Enter</kbd>, o shell entenderá que as instruções terminaram e executará o loop.

> > **Observação:** Use a seta <kbd> ↑ </kbd> no prompt de comando para acessar o histórico e ver o loop for que você acabou de executar. Embora tenha sido digitado em várias linhas, ele aparece em uma única linha. Essa é uma outra sintaxe para escrever loops, mas não é recomendada, por ser mais difícil de ler e interpretar (especialmente nos códigos com várias instruções).

### Boas práticas na criação de Loops

#### Nomes de variáveis inteligíveis
O nome da variável que usamos não interfere no funciomanento do **_Loop_**, mas é aconselhável usar nomes intuitivo. A longo prazo, é melhor usar um nome que ajude a apontar a funcionalidade de uma variável, para que a interpretação do código se torne mais simples para você e demais usuários.

#### Recorrer ao _wildcard_ `*` para definir uma lista
No exemplo acima, digitamos cada item da lista separado por um espaço. Isso geralmente funciona para poucos itens, mas listas maiores podem tornar esse processo tedioso e propenso a erros. Se a lista sobre a qual você está iterando compartilhar semelhanças na nomenclatura, recomenda-se usar o caracter-coringa `*` para especificar a lista. Por exemplo, em vez de digitar cada nome de arquivo Mov10, é possível listá-los usando `Mov*.fq`.

Vamos reescrever o loop **for** usando um nome de variável mais claro e usando o _wildcard_:

```bash
$ for file in Mov10*.fq
 do
   echo $file
   wc -l $file
 done
```
> **Não se esqueça de alterar o nome da variável que está sendo referenciada dentro do loop!**

***

**Exercício 1**

1. Altere o loop acima para que ele seja executado em todos os seis arquivos FASTQ.
2. Altere o loop para que ele imprima a primeira linha de cada um dos seis arquivos.

***

## Scripts e automação de análises

Um dos principais benefícios de aprender a usar estruturas como os Loops é poder incorporá-los aos scripts para minimizar o esforço em aplicar uma série de análises à diferentes conjuntos de dados. Por exemplo, imagine que possamos escrever um script capaz de realizar cada uma das etapas a seguir para vários arquivos:


- Usar o loop for para iterar sobre cada arquivo FASTQ
- Gerar um prefixo para nomear os arquivos de saída
- Descartar _reads_ ruins em um outro arquivo
- Calcular o número de _reads_ de baixa qualidade e reportar essas informações em um arquivo de log

Ainda que pareça complicado, todas as etapas necessárias para criar esse script foram aprendidas ao longo das atividades.

Vamos iniciar criando um diretório para armazenar as _reads_ de baixa qualidade:


```bash
$ mkdir ~/unix_lesson/badreads
```

Agora vamos acesso o diretório `badreads` e utilizar o `vim` para criar um novo script:

```bash
$ cd ~/unix_lesson/badreads

$ vim generate_bad_reads_summary.sh
```

Na primeira linha do script, vamos adicionar o que é chamado de **_shebang_**. Esta linha é o caminho absoluto para o interpretador Bash. Esta linha **_shebang_** garante que o BASH seja o interpretador do script mesmo se ele for executado a partir de uma outra versão de shell.

> #### Por que eu preciso de uma linha **_shebang_**? Meus scripts rodavam perfeitamente bem antes sem ele.
> Adicionar uma linha _shebang_ é uma boa prática. Embora os scripts possam funcionar bem sem ela em ambientes em que o BASH é o shell padrão, eles podem não rodar adequadamente se forem executados em um shell diferente. Para evitar problemas, declaramos explicitamente que esse script precisa ser executado usando o shell BASH.

```bash
#!/bin/bash
```

Após a linha _shebang_, vamos inserir os comandos que queremos executar. O primeiro será para acessar o diretório `raw_fastq`.

> *Observe o uso de comentários nas linhas iniciadas pelo símbolo `#`. Sempre use comentários ao criar seus scripts!*

```bash
# enter directory with raw FASTQs
cd ~/unix_lesson/raw_fastq
```

E agora vamos iterar por todos os arquivos FASTQ:

```bash
# loop over each FASTQ file
for filename in *.fq
```

Após a iteração (loop for), em uma nova linha, adicionamos a instrução `do` e, nas próximas linhas, declaramos os comandos que desejamos executar.

Para cada arquivo a ser processado, podemos usar o comando `basename` para criar um prefixo do nome do arquivo original. Esse prefixo será armzenado em uma variável chamada `samplename` e está será usada para nomear os arquivos de saída.

> *Observe o uso de acentos graves para atribuir a saída do comando `basename` como valor para a variável! Se você não conseguir encontrar a tecla de acento grave no teclado, basta copiar e colar da atividade.*
> Uma outra opção é usar a expressão `$(command)`, em algumas situações, essa sintaxe é preferível ao uso de acentos graves.

```bash
do
  # create a prefix for all output files
  samplename=`basename $filename .subset.fq`
```

Na próxima etapa, vamos adicionar o comando necessário para descartar as _reads_ ruins em um _output_, mas antes usaremos uma instrução `echo` para manter o usuário informado sobre o funcionamento do script. O comando `grep` será responsável por encontrar as _reads_ de baixa qualidade (neste exemplo, _reads_ ruins serão definidas como aquelas contendo 10 N's consecutivos), extrair as quatro linhas associadas a cada _read_ (formato FASTQ) e escrevê-las em um _output_. O arquivo de saída será nomeado usando a variável `samplename` que criamos anteriormente no loop. Também será adicionado um caminho para redirecionar a saída para o diretório `badreads`.

```bash
  # tell us what file we're working on
  echo $filename
  
  # grab all the bad read records into new file
  grep -B1 -A2 NNNNNNNNNN $filename > ~/unix_lesson/badreads/${samplename}_badreads.fq
``` 

> #### Por que estamos usando o nome da variável entre chaves?
> Quando juntamos o nome de uma variável a alguma outra _string_, precisamos que o shell saiba onde o nome da variável termina. Ao encapsular o nome da variável entre chaves, informamos ao shell que o texto contido entre chaves corresponde ao nome da variável. Desta forma, quando a referenciamos, o shell recupera o valor da variável `$samplename` e anexa a string `_badreads.fq`, ao invés de buscar por uma variável chamada `$samplename-badreads.fq`.

A quantidade de _reads_ de baixa qualidade será obtida usando o comando `grep` e o argumento `-c`, que retorna o número de correspondências ao invés das linhas correspondentes. Também será utilizado o argumento `-H`; este responsável por retornar o nome do arquivo junto ao valor da contagem. Essa etapa é útil porque gravaremos as informações em um arquivo de log, e o nome do arquivo nos permitirá saber a qual arquivo a contagem se refere.

```bash
  # grab the number of bad reads and write it to a summary file
  grep -cH NNNNNNNNNN $filename >> ~/unix_lesson/badreads/badreads.count.summary
done
```

O loop será finalizado usando a expressão `done`. Salve o arquivo e saia do `vim`. Agora você dispõe de um script para analisar a qualidade de todos os arquivos FASTQC desta atividade. Ao finalizar o script, ele deverá ter um conteúdo similar ao abaixo:

```bash
#!/bin/bash 

# enter directory with raw FASTQs
cd ~/unix_lesson/raw_fastq

# loop over each FASTQ file
for filename in *.fq 
do 

  # create a prefix for all output files
  samplename=`basename $filename .subset.fq`

  # tell us what file we're working on	
  echo $filename

  # grab all the bad read records
  grep -B1 -A2 NNNNNNNNNN $filename > ~/unix_lesson/badreads/${samplename}_badreads.fq

  # grab the number of bad reads and write it to a summary file
  grep -cH NNNNNNNNNN $filename >> ~/unix_lesson/badreads/badreads.count.summary
done

```

Para executar esse script, você precisa apenas digitar o seguinte comando:

```bash
$ sh generate_bad_reads_summary.sh
```

Como sabemos se o script funcionou? Verifique o diretório `badreads`, para cada um dos arquivos FASTQ originais devemos ter um arquivo de _reads_ de baixa qualidade. Também deve haver um arquivo de log documentando o número total de _reads_ ruins para cada arquivo.

```bash
$ ls -l ~/unix_lesson/badreads
```



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
