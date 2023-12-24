# urso de Hadoop

*https://www.youtube.com/playlist?list=PLeFetwYAi-F_l-NP-TUE2MqKeu_haMP79* 

*https://github.com/toticavalcanti/Curso_Hadoop*

*VM possui pyspark 1.6.0*

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
<img src="_images/302.png" width=50%> </img>

<img src="_images/303.png" width=100%> </img>

## 4 - Contagem de palavras usando PySpark

Entre no shell do Spark digitando `pyspark`

```python
# -*- coding: utf-8 -*-
from pyspark import SparkContext, SparkConf

#Cria a app com o nome WordCount
conf = SparkConf().setAppName("WordCount")

#Instacia o SparkContext -- Não é obrigatório porque o Spark já cria um SparkContext
sc = SparkContext.getOrCreate()

#Cria o RDD com o conte�do do shakespeare.txt
contentRDD = sc.textFile("/user/vlsf2/txts/shakespeare.txt")

#Elimina as linha em branco
filter_empty_lines = contentRDD.filter(lambda x: len(x) > 0)

#Splita as palavras pelo espa�o em branco entre elas
words = filter_empty_lines.flatMap(lambda x: x.split(' '))

#Map-Reduce da contagem das palavras
wordcount = words.map(lambda x:(x,1)) \
.reduceByKey(lambda x, y: x + y) \
.map(lambda x: (x[1], x[0])).sortByKey(False)

#Imprime o resultado
for word in wordcount.collect():
	print(word)

#Salva o resultado no HDFS dentro da pasta /user/vlsf2/txts
wordcount.saveAsTextFile("/user/vlsf2/txts")
```

## 5 - Ingestão de dados com o Flume

- **Twitter**: fonte de dados (streaming) 
- **Flume**: coletor dos dados
- **Hive**: realizar consulta nos dados
- **Spark**: script de análise

<img src="_images/501.png"> </img>

### Flume: Arquitetura

O **Flume** é um serviço de ingestão de dados para coletar, agregar e transportar grandes quantidades de fluxo de dados (streaming), como por exemplo: arquivos de log, eventos, dados de redes sociais, sensores, etc. de várias fontes para um armazenamento de dados centralizado (hbase, hdfs...)

**Outras soluções**

- **Facebook's Scribe** é uma ferramenta imensamente popular que é usada para agregar e transmitir (streaming) dados de log. ele é projetado para dimensionar um número muito grande de nós e ser robusto em relação a falhas de nós e de rede
- **Aрасhе Kafka** foi desenvolvido pela apache software foundation. é um agente de mensagens de código aberto. usando kafka, podemos lidar com feeds com alta taxa de transferência (high throughput) e baixa latência.

O **Evento** consiste em duas partes principais: o **cabeçalho (Header)** e a **carga útil (Payload)**:

1. **Evento (Event):**
   - O evento é a unidade básica de dados no Apache Flume.
   - Ele representa a informação que está sendo transferida de uma **origem (source)** para um **destino (sink)**.
   - Cada evento contém um cabeçalho **(Header)** e uma carga útil **(Payload)**.
2. **Cabeçalho (Event Header):**
   - O cabeçalho é uma parte do evento que contém metadados ou informações sobre o próprio evento.
   - Esses metadados incluem detalhes como timestamps, identificadores únicos, e informações adicionais que descrevem o contexto do evento.
   - O cabeçalho fornece informações importantes para o processamento e roteamento do evento através do pipeline do Flume.
3. **Carga Útil (Payload):**
   - A carga útil é a parte principal do evento que contém os dados reais que estão sendo transferidos.
   - Pode ser qualquer tipo de informação, como logs, mensagens, ou qualquer outra forma de dados que você esteja movendo com o Apache Flume.
   - A carga útil é a parte do evento que é realmente consumida e processada pelo Flume para ser movida através do sistema.

​	<img src="_images/503.png" width=50%> </img>

1. **Source:**
   - A Source é a origem dos dados no Apache Flume.
   - Ela é responsável por coletar ou receber dados a partir de uma fonte externa, como logs, eventos de servidor, ou fluxos de dados.
   - A Source é o ponto de entrada no pipeline do Flume, e sua função principal é gerar eventos que serão processados e movidos através do sistema.
2. **Channel:**
   - O Channel é um componente de armazenamento temporário no Apache Flume.
   - Ele atua como um buffer entre a Source e a Sink, permitindo o armazenamento temporário dos eventos enquanto aguardam processamento adicional.
   - Existem diferentes tipos de canais no Flume, como canais de memória, canais de arquivo e canais JDBC, que oferecem diferentes formas de armazenamento temporário.
3. **Sink:**
   - A Sink é o destino final dos dados no Apache Flume.
   - Ela é responsável por consumir os eventos provenientes do Channel e realizar a ação final, que pode ser o armazenamento em um banco de dados, a escrita em arquivos, ou a transferência para outro sistema.
   - A Sink representa o ponto de saída do pipeline do Flume.

**Relacionamento entre Source, Channel e Sink:**

- A Source gera eventos que são colocados no Channel.
- O Channel armazena temporariamente os eventos antes que eles sejam consumidos pela Sink.
- A Sink consome os eventos do Channel e executa a ação final, movendo os dados para o destino desejado.

​    <img src="_images/502.png" width=50%> </img>

1. **Interceptors:**
   - Os Interceptors são módulos opcionais que podem ser configurados em uma Source ou Sink no Apache Flume.
   - Sua função é manipular ou enriquecer eventos à medida que passam pelo pipeline.
   - Os Interceptors podem ser usados para adicionar metadados, modificar o conteúdo do evento ou realizar outras transformações antes que o evento alcance o próximo estágio do pipeline.
2. **Channel Selectors:**
   - Os Channel Selectors são responsáveis por direcionar eventos para canais específicos dentro do Apache Flume.
   - Quando há mais de um canal disponível (por exemplo, canais de memória e canais de arquivo), o Channel Selector decide para qual canal um evento específico deve ser encaminhado.
   - Existem diferentes tipos de Channel Selectors, como replicação simples, seleção aleatória e lógica personalizada, permitindo flexibilidade na distribuição de eventos entre os canais.
3. **Collectors:**
   - Collectors são responsáveis por coletar eventos de múltiplos canais e entregá-los a uma Sink no Apache Flume.
   - Quando há mais de um canal disponível e uma Sink única, o Collector é usado para reunir eventos de diferentes canais e enviá-los à Sink para o processamento final.
   - O uso de Collectors é especialmente útil em cenários em que os eventos podem ser distribuídos entre diferentes canais antes de serem consolidados e enviados para a Sink.

**Relacionamento entre Interceptors, Channel Selectors e Collectors:**

- Interceptors podem ser configurados nas Sources ou Sinks para manipular eventos durante a coleta ou entrega.
- Channel Selectors são utilizados para decidir para qual canal um evento deve ser enviado, permitindo uma distribuição eficiente de eventos entre diferentes armazenamentos temporários.
- Collectors são usados para reunir eventos de diferentes canais e entregá-los à Sink, consolidando os dados antes do processamento final.

### Flume: Tipos de fluxos

No Apache Flume, os conceitos de Multi-hop flow, Fan-out flow e Fan-in flow referem-se a diferentes padrões de fluxo de eventos no sistema:

1. **Multi-hop Flow:**

   - Em um Multi-hop Flow, os eventos fluem através de vários nós do Apache Flume, ou seja, eles percorrem vários agentes do Flume antes de atingirem seu destino final.
   - Cada agente do Flume age como um intermediário que encaminha os eventos para o próximo agente no caminho até o destino final (Sink).
   - Esse tipo de fluxo é útil quando você tem uma arquitetura distribuída e os eventos precisam passar por vários estágios de processamento antes de alcançar o destino final.

   <img src="_images/504.jpg" width=75%> </img>

2. **Fan-out Flow:**

   - Em um Fan-out Flow, os eventos são replicados para múltiplos destinos simultaneamente.
   - A Source gera um evento, e este é replicado por um Channel Selector para ser enviado para diferentes canais ou Sinks.
   - Cada Sink ou canal recebe uma cópia do evento, permitindo que diferentes processamentos ou armazenamentos sejam realizados independentemente.
   - Esse padrão é útil quando você precisa distribuir os mesmos eventos para diferentes destinos para diferentes finalidades.

   <img src="_images/506.jpg" width=50%> </img>

3. **Fan-in Flow:**

   - Em um Fan-in Flow, os eventos de várias fontes (Sources) são consolidados em um único canal antes de serem enviados para um destino comum (Sink).
   - Cada Source gera eventos que são encaminhados para um canal compartilhado.
   - O canal acumula os eventos de diferentes fontes antes de entregá-los à Sink.
   - Esse padrão é útil quando você precisa consolidar dados de várias fontes antes de realizar uma ação final, como armazenamento em um banco de dados centralizado.

   <img src="_images/505.jpg" width=50%> </img>

**Relacionamento entre Multi-hop Flow, Fan-out Flow e Fan-in Flow:**

- Um Multi-hop Flow pode incluir padrões Fan-out e Fan-in em diferentes estágios do percurso dos eventos.
- Fan-out e Fan-in Flows são comumente usados em conjunto para distribuir eventos para diferentes processamentos e consolidá-los posteriormente.
- A escolha entre esses padrões depende dos requisitos específicos de arquitetura e processamento de dados.

### Flume: configuração do Agente

A configuração do agente é feita por meio de arquivos de configuração. Esses arquivos contêm propriedades que definem como o agente se comportará, quais fontes (sources) serão utilizadas, como os eventos serão processados e para onde serão enviados:

```
AGENT_NAME.SOURCES.SOURCE_NAME.TYPE = VALUE
```

Aqui está uma explicação genérica desses elementos e um exemplo hipotético:

- **AGENT_NAME:** O nome atribuído ao agente. Este nome é usado como prefixo para todas as propriedades relacionadas a esse agente específico.
- **SOURCES:** Indica a seção de configuração onde as fontes são definidas. As fontes são as origens de dados que alimentam o pipeline do Flume.
- **SOURCE_NAME:** O nome atribuído à fonte específica dentro do agente. Cada fonte tem um nome exclusivo dentro de um agente.
- **TYPE:** Indica o tipo de fonte que está sendo configurada. O valor desta propriedade especifica qual implementação de fonte o Flume deve usar.
- **VALUE:** Representa o valor associado à propriedade TYPE. Este valor pode variar dependendo do tipo de fonte.

Exemplo hipotético de configuração de uma fonte de log (assumindo que exista uma implementação chamada `avroSource`):

```
MyAgent.SOURCES.LogSource.TYPE = avroSource
```

Neste exemplo:

- **MyAgent:** O nome do agente é "MyAgent".
- **SOURCES:** Estamos definindo configurações relacionadas às fontes do agente.
- **LogSource:** O nome da fonte é "LogSource".
- **TYPE:** Indica que estamos configurando uma fonte do tipo "avroSourc

### Configuração APP Twitter

Instalar o telnet no Máquina Cloudera `yum -y install telnet`

<p style="color: #1A5276"><b>Terminal 1</b></p>

`flume-ng agent -n a1 -c conf -f conf_vlsf2.conf`

<p style="color: #1A5276"><b>Terminal 2</b></p>

`telnet localhost 44444` 
