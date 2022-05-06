




## Primeiros Passos

As atividades práticas serão, preferencialmente, executadas no servidor Darwin hospedado no Laboratório de Genética Humana e Molecular (LGHM/UFPA). Dessa forma, os alunos deverão acessar o servidor para ter acesso aos dados e recursos para execução das atividades.

### Servidor

Um **_servidor_** é um dos componentes de um modelo de computação distribuída denominado **_cliente/servidor_**. Esse modelo baseia-se na distribuição de funções entre dois tipos de processos independentes: o servidor e o cliente, estes podem residir em uma mesma máquina ou em diferentes computadores conectados através de uma rede. O cliente é qualquer processo que requisita um serviço ao servidor, sendo este último o responsável por ofertar e executar o processo. Para tal, uma aplicação ou usuário cliente envia uma mensagem ao servidor através da rede (local ou internet) requisitando ao servidor que execute uma determinada tarefa, os dados são então processados no servidor e após a execução, os resultados são retornados ao cliente. 

<p align="center">
<img src="./IMG/client_server.png" width="500"/>
</p>

As principais vantagens da arquitetura cliente/servidor são:

1) Todos os recursos são centralizados, dessa forma, o servidor é capaz gerenciar os recursos que são comuns a todos os usuários. (Por exemplo, evitando problemas de redundância ou conflito de dados).

2) Maior segurança, 

3) Níveis de acesso administrativo mais altos não são necessários e não estão disponíveis aos clientes.

4) Escalabilidade da rede, uma vez que é possível adicionar ou remover clientes sem afetar a operação da rede ou a necessidade de grandes alterações.

Dentre as desvantagens estão:

1) Maior custo de implementação e manutenção devido ao aumento da complexidade do sistema.

2) No caso de falha do servidor, os serviços estarão indisponíveis a todos os usuários.

3) No caso de falha da rede, os serviços não poderão ser acessados.

4) Se um cliente gera alto tráfego de rede ou consome muitos recursos, os demais clientes podem sofrer com alta latência (atraso na resposta).

### Cluster (HPC)

<p align="center">
<img src="./IMG/hpc.png">
</p>

Para processar dados de larga escala é necessário o uso de computadores ou grupos de computadores mais potentes que os computadores pessoais. O conjunto de servidores ligados através de uma rede rápida e atuando em conjunto é denominado **_cluster_** ou **_HPC_**. Cada computador integrando essa rede recebe o nome de nó (**_node_**). Os nós podem ter diferentes funções, por exemplo, existem os nós de conexão (**_login nodes_**) responsáveis por administrar as conexões e submissões de processo ao cluster, os nós de processamento (**_compute nodes_**) encarregados de processar os dados e os nós de armazenamento (**_storage_**) onde os dados são depositados e conservados. 

### Glossário de termos

#### Código ou programa

Lista de instruções consecutivas executadas em um computador. Código-fonte (_source code_) é o texto que constitui o programa (embora seja um arquivo de texto, a extensão ".txt" será substituída por aquela adequada à linguagem de programação utilizada, por ex., **Shell** = ".sh"; **Perl** = ".pl"; **Python** = ".py"; **R** = ".R").

Para executar um programa, iremos utilizar o **_interpretador_** correspondente à linguagem utilizada. Esse interpretador será responsável por ler e executar a sequência de comandos contidas no arquivo executável - embora um programa possa conter um ou mais arquivos de código associados, apenas um será responsável por iniciar e coordenar a execução das instruções contidas no programa.

#### Algoritmo

A receita dos passos (ou instruções) de um programa é denominada **_algoritmo_**. Em algumas situações, os algoritmos são praticamente indistinguíveis do código do programa, em outras, especialmente em programas complexos, é recomendado escrever um algoritmo antes de desenvolver um programa. Isso porque a linguagem de algoritmos costuma ser mais simples, compacta e próxima da linguagem humana, simplificando o processo de desenvolvimento. Além disso, algoritmos são portáveis, podendo ser implementados em diferentes linguagens de programação.

Um exemplo de algoritmo:

```bash
inicializar as variáveis a e z
Definir o valor das variáveis a e z
Imprimir os valores de a e z na tela
```

A **_implementação_** do algoritmo é o processo de escrever o algoritmo usando uma linguagem de programação e testá-lo. A etapa de testes e verificação dos resultados é essencial para assegurar que as instruções foram definidas corretamente e que os resultados estão corretos. Essa etapa é crítica e não deve ser menosprezada.

> Sempre verifique se os dados de entrada e os de saída correspondem ao esperado e jamais assuma que se o programa foi executado com êxito, os resultados estão corretos.

> No desenvolvimento de programas complexos, dê preferência ao desenvolvimento modular e teste o código após a implementação de cada rotina, isso facilita o processo de implementação e a depuração de erros.

Os erros na implementação do programa são comumente chamados de **_bugs_** e o processo de encontrá-los e corrigi-los é denominado **_debugging_**.

#### GNU/Linux

**_Unix_** é um **sistema operacional** em desenvolvimento desde os anos 1960. É constituído por uma suíte de aplicativos (programas) que permitem o funcionamento do computador. É um sistema estável, multiusuário e multitarefa utilizado em diferentes tipos de computadores, desde computadores pessoais até HPCs e dispositivos eletrônicos.

Atualmente, há uma grande variedade de versões derivadas do sistema Unix, sendo as variedades mais populares: Sun Solaris, MacOS X e GNU/Linux.

Os sistemas operacionais GNU/Linux são formados pelo kernel Linux, por programas acessórios GNU e o **_shell_**. 

O **_Kernel_** é responsável pelo gerenciamento dos processos e realiza a alocação de recursos - tais como memória, processamento e acesso ao sistema de arquivos para a execução das tarefas.

#### Shell

Atua como uma interface entre o usuário e o kernel. A cada conexão de um usuário, o programa de acesso verifica as credenciais de acesso (geralmente, login e senha) e então inicia a interface por linha de comando (CLI). Após iniciado, o Shell é responsável por interpretar os comandos inseridos pelo usuário e providenciar para que sejam executados. Quando um comando é submetido ao shell, este não estará disponível a novos comandos até que a tarefa em execução termine e os resultados sejam apresentados no terminal.

O shell pode ser customizado pelo usuário e existem diferentes versões de shell, sendo uma das mais populares o **_BASH_** (Bourne-Again Shell).

> Um dos recursos disponibilizados pelo Shell é o de autocompletar. Através desse recurso, ao iniciar a digitação do nome de um programa, arquivo ou diretório e digitar a tecla **_TAB_**, o Shell irá completar o nome automaticamente. Nos casos onde houver mais de uma opção disponível para autocompletar, apertar a tecla **TAB** duas vezes imprimir todos os nomes que correspondem à sequência digitada.

> Outro recurso do Shell é o histórico de comandos (**_History_**), através deste recurso o Shell armazena os comandos executados, sendo possível acessá-los novamente através das setas para cima e para baixo, através do comando **_history_** ou consultando o arquivo .bash_history.

> Boa parte dos programas de bioinformática são executados através do Shell ou **_prompt de comando_**.

## Logando no servidor Darwin

Para logar a um servidor (ou cluster) é necessário ter uma conta constituída por um nome de usuário e senha. No contexto desta disciplina, contas foram criadas para cada um dos alunos e os respectivos usuários e senhas de acesso disponibilizados individualmente.

### Ferramentas de acesso ao servidor

#### Usuários do sistema operacional Windows

Os usuários do sistema operacional Windows poderão utilizar diferentes formas de acesso ao servidor, tais como, aplicativo [**Putty**](https://www.putty.org/), [**Git Bash**](https://gitforwindows.org/), [**_Bash for Windows_**](https://livreeaberto.com/instalando-bash-no-windows) e [**_Bitvise SSH Client_**](https://www.bitvise.com/ssh-client-download). Qualquer forma de acesso pode ser utilizada no contexto da disciplina, entretanto, na primeira aula nos restringimos ao acesso através da ferramentas Bitvise SSH.

#### Usuários do sistema operacional Mac OS

Os usuários do sistema operacional Mac OS podem recorrer ao aplicativo **Terminal** para executar as tarefas no servidor remoto. Para acessar o servidor remoto, devem utilizar o comando **_SSH_** (explicado abaixo).

#### Usuários de sistemas operacionais GNU/Linux

O acesso ao servidor pode ser realizado através do comando **_SSH_**. O SSH (_Secure Socket Shell_) é um protocolo que permite ao usuário se conectar de maneira segura a um computador remoto usando uma interface por linha de comando (CLI). Assim que a conexão é aprovada, uma sessão do Shell é iniciada e o usuário está apto a submeter comandos através do cliente.

```bash
username@host:~$ ssh
usage: ssh [-1246AaCfGgKkMNnqsTtVvXxYy] [-b bind_address] [-c cipher_spec]
[-D [bind_address:]port] [-E log_file] [-e escape_char]
[-F configfile] [-I pkcs11] [-i identity_file]
[-J [user@]host[:port]] [-L address] [-l login_name] [-m mac_spec] [-O ctl_cmd] [-o option] [-p port] [-Q query_option] [-R address] [-S ctl_path] [-W host:port] [-w local_tun[:remote_tun]]
[user@]hostname [command]
```

### Acesso ao servidor

> Os passos (1 a 4) são necessários para os usuários de sistemas operacionais Mac OS e GNU/Linux ou usuários do Windows usando os aplicativos **Git Bash** e **Bash for Windows**. Usuários do Windows usando os programas **Putty** e do **Bitvise SSH** serão automaticamente redirecionados para o Shell do servidor remoto após o login pela interface gráfica.

Para conectar ao Darwin, as seguintes etapas devem ser seguidas:

1. Digite o comando `ssh` no prompt de comando seguido por um espaço em branco e então digite o nome de usuário (_username_) seguido por `@200.239.101.201`. Não há espaços entre o nome de usuário e o símbolo de arroba `@`.

```bash
ssh giordano@@200.239.101.201
```
![image](https://user-images.githubusercontent.com/17560094/167214224-d47efa6c-ab54-4188-bbc2-cc20b06a5707.png)

2. Pressione `Enter` ou `Return` e você será requisitado a inserir a sua senha.

![image](https://user-images.githubusercontent.com/17560094/167214093-496d4155-a153-48fc-a3af-f0eafa44bd5b.png)

> Ao digitar a senha, o cursor não irá se mover, tal situação é normal e indica que o computador está recebendo e transmitindo a senha digitada ao computador remoto.

> Caso você queira apagar o conteúdo digitado durante esta etapa, é possível utilizar o atalho `Ctrl` + `U` (não funciona em todos os terminais).

3. Caso esta seja a primeira conexão entre o cliente e o servidor, uma mensagem de aviso aparecerá perguntando se você deseja adicionar 

4. A

![image](https://user-images.githubusercontent.com/17560094/167213536-1e576f9a-4fdf-49ce-b4de-809dee4492ac.png)

![image](https://user-images.githubusercontent.com/17560094/167213629-b6f3bf1e-da6b-407d-9991-aa01748b9162.png)





### Bibliografia / Fontes

Langtangen, H. P. (2016). A Primer on Scientific Programming with Python (5th edition 2016.). Springer Berlin Heidelberg : Imprint: Springer.


