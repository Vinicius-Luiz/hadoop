# Curso de Hadoop

*https://www.youtube.com/playlist?list=PLeFetwYAi-F_l-NP-TUE2MqKeu_haMP79* 

## 01 - Baixando e configurando a máquina Cloudera

**Download Cloudera-VM**<br>

https://downloads.cloudera.com/demo_vm/virtualbox/cloudera-quickstart-vm-5.13.0-0-virtualbox.zip

**Download Putty**<br>

https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

**Download WinSCP**<br>

https://winscp.net/download/WinSCP-6.1.2-Setup.exe

**Autenticação**<br>

- login: *root* ou *cloudera*
- senha: *cloudera*

**Configurações de rede**

<img src="_images/101.png" width=50%> </img>

<img src="_images/102.png" width=50%> </img>

**Na VM-Cloudera digitar `ifconfig` no CLI para obter os IPS**

<img src="_images/103.png" width=50%> </img>

**Utilizar ip `eth1` ou `eth0` no Putty e WinSCP**

<img src="_images/104.png" width=40%> </img>

## 2 - Componentes principais Hadoop

**Há dois tipos de nós básicos**

- **NÓS MESTRES (MASTERS)** coordena os nós trabalhadores, geralmente são os pontos de entrada para o acesso do usuário ao cluster;
- **NÓS TRABALHADORES** aceitam as tarefas designadas pelos nós mestres para armazenar dados, ler dados ou executar uma aplicação em particular.

Tanto o HDFS como o YARN têm vários serviços mestres responsáveis pela coordenação dos serviços trabalhadores que executam em cada nó

**Serviços do HDFS**

- **NAMENOME (MESTRE)** armazena a árvore de diretórios do sistema de arquivos, metadados de arquivos e as localizações de cada arquivo nc cluster. ele não armazena dados e nem passa dados do datanode ao cliente, o que ele faz é apontar os datanodes corretos aos clientes
- **NAMENOME SECUNDÁRIO (MESTRE)** executam tarefas de manutenção (housekeeping) e de pontos de verificação (checkpointing) em nome do namenode (ele não é um namenode de backup)
- **DATANODE (TRABALHADOR)** armazena e administra blocos HDFS no disco local e informa a saúde e o status de repositórios individuais de dados ao NAMENOME.

**Serviços do YARN**

- **ResourceManager (MESTRE)** aloca e monitora recursos disponíveis no cluster (memória e processadores) para as aplicações e trata do escalonamento dos jobs no cluster
- **ApplicationMaster (MESTRE)** coordena uma aplicação em particular executada no cluster de acordo com o escalonamento feito pelo ResourceManager
- **NodeManager (TRABALHADOR)** executa e administra tarefas de processamento em um nó individual e informa sobre a saúde e o status das tarefas à medida que elas executam.

## 3 - Comandos básicos Hadoop

`hadoop` O comando principal do Hadoop.

`fs` Indica que a operação deve ser realizada no sistema de arquivos distribuído do Hadoop.

**Listar o conteúdo do diretório raiz no sistema de arquivos distribuído do Hadoop**

`hadoop fs -ls /` 

- `-ls`: A opção que significa "listar". Isso solicita ao Hadoop que liste o conteúdo do diretório especificado.
- `/`: O caminho do diretório no sistema de arquivos distribuído do Hadoop. Neste caso, é o diretório raiz, indicado pelo caractere "/".

**Criar a estrutura de diretórios necessária para armazenar dados no caminho especificado no sistema de arquivos distribuído do Hadoop**

`hadoop fs -mkdir -p ~user/vlsf2/txts`

- `-p`: Essa opção permite criar diretórios pais conforme necessário. Se o diretório pai não existir, ele será criado junto com o diretório especificado.
- `~user/vlsf2/txts`: O caminho do diretório que você deseja criar.

**Copiar o arquivo local `shakespeare.txt` para o diretório `/user/vlsf2/txts` no sistema de arquivos distribuído do Hadoop**

`hadoop fs -put shakespeare.txt /user/vlsf2/txts`

- `-put`: Indica que você deseja copiar um arquivo do sistema de arquivos local para o HDFS.
- `shakespeare.txt`: O nome do arquivo local que você deseja copiar para o HDFS.
- `/user/vlsf2/txts`: O caminho no HDFS para onde o arquivo será copiado. Neste caso, o caminho é `/user/vlsf2/txts`.

### Acessando via página web

No seu navegador, acessar através o mesmo IP que você se conecta com o Putty ou WinSCP utilizando a porta *8888* http://192.168.1.86:8888/<br>
O HUE, que significa "Hadoop User Experience", é uma interface web de código aberto que facilita a interação com o ecossistema Hadoop. Ele fornece uma interface gráfica para facilitar o gerenciamento, monitoramento e utilização de várias ferramentas e serviços no ambiente Hadoop.

Algumas das funcionalidades do HUE incluem:

1. **Exploração de Dados:** Permite explorar e visualizar dados armazenados no Hadoop HDFS.
2. **Querying e Processamento de Dados:** Oferece suporte a consultas interativas no Hive (um sistema de data warehousing construído sobre o Hadoop) e ao editor de scripts Pig (uma linguagem de alto nível para processamento de dados no Hadoop).
3. **Trabalhos MapReduce:** Facilita o gerenciamento e monitoramento de trabalhos MapReduce.
4. **Agendamento de Trabalhos:** Permite agendar trabalhos no Oozie, um serviço de orquestração de fluxo de trabalho para o Hadoop.
5. **Gerenciamento de Metadados:** Oferece uma interface para gerenciar metadados no Hadoop, como tabelas Hive e fluxos Oozie.
6. **Gerenciamento de Segurança:** Permite a administração de permissões e políticas de segurança para os dados e serviços Hadoop.
7. **Integração com outros Serviços:** Integra-se com vários serviços Hadoop, tornando mais fácil para os usuários interagirem com esses serviços por meio de uma interface gráfica.

<center>
  <p>Localizando os arquivos armazenados no HDFS através do HUE</p>
</center>

<img src="_images/301.png" width=15%> </img>

<center>
  <p>Exemplo do arquivo shakespeare.txt no HDFS</p>
</center>

<img src="_images/302.png" width=70%> </img>
