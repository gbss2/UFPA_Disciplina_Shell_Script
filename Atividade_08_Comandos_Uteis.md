
## Comandos úteis

Como vimos nas atividades anteriores, os sistemas GNu?linux possuem vários programas que nos auxiliam na manipulação e edição de arquivos. É importante ter uma visão geral das ferramentas para aproveitar ao máximo os recursos disponíveis. Nesta atividade vamos conhecer (ou revistar) alguns comandos importantes, assim como suas opções mais comuns. 

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

### _*REPLACEMENT*_

O uso mais comum para o `sed` é a busca e substituição de padrões. 
