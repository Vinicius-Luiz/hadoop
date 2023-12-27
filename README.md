# Construindo Big Data com Cluster de Hadoop e Ecossistema

<p style="color: red"><b>Seção 1-5 foi extraído do curso no youtube. O curso está depreciado devido a desatualização do ambiente</b></p>

***curso:** https://www.udemy.com/course/construindo-big-data-com-cluster-de-hadoop-e-ecossistema/*

***outros:***

*https://www.youtube.com/playlist?list=PLeFetwYAi-F_l-NP-TUE2MqKeu_haMP79* 

*https://github.com/toticavalcanti/Curso_Hadoop*

<h2 style="color: red">1 - Baixando e configurando a máquina Cloudera </h2>

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

<h2 style="color: red">2 - Componentes principais </h2>

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

<h2 style="color: red">3 - Comandos básicos </h2>

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

<h2 style="color: red">4 - Contagem de palavras usando PySpark  </h2>

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

<h2 style="color: red">5 - Ingestão de dados com Flume  </h2>

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

## 6 - Características iniciais de um ambiente distribuído

1. **Client:**
   - Entidade que faz solicitações ou envia comandos a um sistema ou serviço distribuído.
2. **Management Node:**
   - Nó responsável por controlar e gerenciar outros nós no sistema distribuído, coordenando tarefas e alocando recursos.
3. **Worker Node:**
   - Nó que executa tarefas específicas ou processa dados conforme instruído, sendo coordenado e gerenciado pelos nós de gerenciamento.

<img src="_images/601.gif" width="50%"></img>

**Aplicação em Sistemas Distribuídos de Processamento de Dados:**

- Em sistemas como Apache Hadoop ou Apache Spark, o "Client" pode ser a aplicação ou usuário que envia trabalhos para processamento distribuído.
- O "Management Node" pode ser representado por componentes como ResourceManager no Hadoop ou o Master Node no Spark, que gerenciam a execução e coordenação dos trabalhos.
- Os "Worker Nodes" seriam os nós que realizam efetivamente o processamento de dados, como os DataNodes no Hadoop ou os Worker Nodes no Spark.

<img src="_images/602.png" width="70%"></img>

| **Módulo** | **Função**                                                   | **Descrição**                                                |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| ZooKeeper  | Serviço de Coordenação Distribuída                           | Fornece serviços para coordenação e gerenciamento de configuração em sistemas distribuídos. Amplamente usado para sincronização, eleição de líder e gerenciamento de configurações. |
| Oozie      | Orquestração de Fluxo de Trabalho                            | Sistema de orquestração que permite a criação, agendamento e coordenação de fluxos de trabalho complexos no ambiente Hadoop. Permite a execução sequencial ou condicional de tarefas, facilitando o gerenciamento de processos de dados em grande escala. |
| Spark      | Motor de Processamento de Dados em Memória                   | Framework de processamento de dados distribuído e em memória. Suporta análise de dados em larga escala, processamento de batch e stream, e aprendizado de máquina. |
| Kafka      | Sistema de Mensagens Distribuídas                            | Plataforma de streaming distribuída para ingestão, armazenamento e processamento em tempo real de fluxos de dados. |
| Hive       | Armazenamento e Consulta de Dados                            | Camada de armazenamento e consulta de dados que utiliza a linguagem de consulta HiveQL para interagir com dados armazenados no HDFS. |
| HBase      | Banco de Dados NoSQL Distribuído                             | Banco de dados NoSQL distribuído e orientado a colunas que fornece acesso rápido a grandes volumes de dados. |
| Solr       | Plataforma de Pesquisa de Texto Completa                     | Plataforma de pesquisa de texto completa construída sobre o Apache Lucene. Fornece recursos avançados de pesquisa, indexação e recuperação de dados. |
| Flume      | Coleta, Agregação e Movimentação de Dados                    | Ferramenta para coleta, agregação e movimentação de grandes volumes de dados de logs de diferentes fontes para sistemas de armazenamento centralizados. |
| Sqoop      | Transferência de Dados entre Hadoop e Bancos de Dados Relacionais | Ferramenta para transferência eficiente de dados entre bancos de dados relacionais e o HDFS. |
| Mahout     | Biblioteca de Aprendizado de Máquina                         | Biblioteca de aprendizado de máquina para Hadoop, projetada para implementar algoritmos escaláveis e distribuídos de aprendizado de máquina. |
| Pig        | Linguagem e Plataforma para Análise de Dados                 | Permite a expressão de transformações de dados em uma linguagem chamada Pig Latin, facilitando a escrita de programas para processamento de dados em ambientes Hadoop. |

<div style="float: left">
    <img src="_images/604.png" width="100px" alt="Imagem">
</div>





1. **Hive Clients:**
   - Aplicativos ou ferramentas que interagem com o Apache Hive para executar consultas ou gerenciar metadados.
2. **Hive Services:**
   - Componentes principais que incluem o Hive Driver, Hive Server e Hive CLI, fornecendo funcionalidades essenciais do Apache Hive.
3. **Hive Metastore:**
   - Armazena metadados relacionados a dados no Hadoop, como informações sobre tabelas, partições e esquemas.
4. **File Storage (Armazenamento de Arquivos):**
   - Refere-se à maneira como os dados são armazenados no Hadoop Distributed File System (HDFS), geralmente em formatos otimizados para consultas como texto, Avro, Parquet ou ORC.

<img src="_images/603.png" width="50%"></img>

<div style="float: left">
    <img src="_images/605.webp" width="100px" alt="Imagem">
</div>





O Apache Pig é uma plataforma para análise e processamento de dados no ecossistema Hadoop. Ele fornece uma linguagem chamada Pig Latin, que é uma linguagem de script de alto nível projetada para facilitar a escrita de programas para processamento de dados em larga escala. Aqui estão alguns pontos-chave sobre o Apache Pig:

1. **Linguagem Pig Latin:**
   - Pig Latin é a linguagem de script utilizada no Apache Pig. Ela é projetada para ser simples e expressiva, permitindo que os usuários descrevam operações de transformação de dados em um estilo declarativo.
2. **Modelo de Programação de Fluxo de Dados:**
   - O Apache Pig segue um modelo de programação de fluxo de dados, onde as operações de transformação de dados são expressas como um fluxo de dados dirigido (DAG). Isso facilita a representação de operações complexas em termos de fluxo de dados.
3. **Otimização Automática:**
   - O Pig otimiza automaticamente as operações do usuário sempre que possível, permitindo que os usuários se concentrem na lógica da aplicação em vez de otimizações de execução.
4. **Extensibilidade:**
   - O Pig é extensível, permitindo que os usuários definam suas próprias funções em Java, Python, ou outras linguagens, para realizar operações personalizadas durante o processamento.
5. **Compatibilidade com o Hadoop:**
   - O Pig é executado sobre o Hadoop e aproveita as capacidades do Hadoop Distributed File System (HDFS) para armazenamento distribuído e processamento paralelo.
6. **Facilidade de Aprendizado:**
   - Pig é frequentemente considerado mais acessível para usuários que estão familiarizados com linguagens de script, como Python. Isso facilita a transição de usuários que não têm experiência em programação Java, que é comumente usada em muitas outras tecnologias Hadoop.

```pig
-- Carrega os dados do arquivo CSV
data = LOAD 'usuarios.csv' USING PigStorage(',') AS (nome:chararray, idade:int, cidade:chararray);

-- Filtra usuários com idade válida (idade maior que 0)
usuarios_validos = FILTER data BY idade > 0;

-- Agrupa os usuários por cidade
usuarios_por_cidade = GROUP usuarios_validos BY cidade;

-- Calcula a média de idade para cada cidade
media_idade_por_cidade = FOREACH usuarios_por_cidade GENERATE group AS cidade, AVG(usuarios_validos.idade) AS media_idade;

-- Armazena os resultados
STORE media_idade_por_cidade INTO 'saida/';

-- Exibe os resultados
DUMP media_idade_por_cidade;
```

<img src="_images/605.png"></img>

O **HCatalog** fornece um serviço de metadados que permite a criação, gerenciamento e compartilhamento de metadados sobre dados armazenados no Hadoop Distributed File System (HDFS).

- **Integração com Hive e Pig:** O HCatalog é integrado com o Apache Hive e o Apache Pig, permitindo que essas ferramentas acessem e compartilhem as informações do esquema (metadados) sem a necessidade de redefinir ou duplicar essas informações.

- **APIs para Linguagens de Programação:** O HCatalog fornece APIs para várias linguagens de programação, incluindo Java e Python, permitindo que os desenvolvedores acessem e interajam com os metadados do Hadoop.
- **Facilita a Compartilhamento de Dados:** Ao centralizar os metadados, o HCatalog facilita o compartilhamento de dados entre diferentes aplicativos e projetos dentro do ecossistema Hadoop.

<div style="float: left">
    <img src="_images/606.png" width="100px" alt="Imagem">
</div>



O Apache Spark é um framework de processamento de dados em cluster que se integra ao ecossistema Hadoop. Destacam-se:

- **Processamento em Memória:**
  - Armazenamento de dados em memória para acesso rápido.
- **APIs Amigáveis:**
  - APIs em Scala, Java, Python e SQL para desenvolvimento acessível.
- **Modelo de Programação Avançado:**
  - Suporta operações avançadas para pipelines eficientes.
- **Diversos Workloads:**
  - Além de batch, oferece suporte a stream processing, machine learning e processamento de grafos.
- **Spark Core e Módulos Adicionais:**
  - Núcleo do Spark com módulos como Spark SQL, Spark Streaming, MLlib e GraphX.
- **Integração com Hadoop:**
  - Acessa dados do HDFS, pode ser executado em clusters Hadoop existentes.
- **Tempo de Resposta Interativo:**
  - Processamento em memória e DAG contribuem para tempos de resposta rápidos.
- **Estruturas de Dados Abstratas:**
  - Introduz RDDs e DataFrames para simplificar o processamento distribuído.
- **Ecossistema Hadoop Unificado:**
  - Integra-se facilmente com ferramentas Hadoop como Hive, HBase e Flume.

<div style="float: left">
    <img src="_images/608.png" width="100px" alt="Imagem">
</div>





O Apache Storm é um sistema de processamento de dados em tempo real, projetado para lidar com fluxos contínuos de dados e realizar processamento em tempo real em um ambiente distribuído.

- **Topologias de Fluxo de Dados:** Define fluxos de dados por meio de topologias com spouts (fontes) e bolts (processadores).

- **Escalabilidade e Tolerância a Falhas:** Altamente escalável e tolerante a falhas, permitindo processamento contínuo mesmo com falhas de componentes.

- **Modelo de Programação Simples:** Desenvolvimento simples com spouts que emitem dados e bolts que os processam.

- **Integração com Diversas Fontes e Destinos:** Pode se integrar a várias fontes e destinos, como bancos de dados, sistemas de mensagens e APIs externas.

- **Garantia de Processamento:** Oferece garantias de processamento, como "exatamente uma vez" ou "pelo menos uma vez".

- **Ecossistema:** Ecossistema inclui Trident (para operações de estado) e integração com ferramentas Hadoop como HBase e Kafka.

- **Utilizado em Diversos Casos de Uso:** Aplicações em análise de sentimentos, monitoramento em tempo real, detecção de fraudes e IoT.

## 7 - Projetando um ambiente de supercomputação com Hadoop

  <img src="_images/701.png" width="50%"></img>

1. **NameNode:** Gerencia informações sobre onde os dados estão no Hadoop Distributed File System (HDFS). Armazena metadados e é crucial para o HDFS.
2. **DataNode:** Armazena efetivamente os dados no HDFS. Múltiplos DataNodes distribuem e mantêm os blocos de dados no cluster.
3. **Resource Manager:** Gerencia os recursos do cluster. Aloca recursos para diferentes aplicações, garantindo uso eficiente do cluster.
4. **Node Manager:** Roda em cada nó do cluster, monitorando recursos locais como CPU e memória. Responsável por executar e monitorar tarefas (containers) atribuídas pelo Resource Manager.

<div style="display: flex; width: 100%;">
    <img src="_images/702.png" alt="Imagem 1" style="width: 50%; height: auto; display: block;">
    <img src="_images/703.png" alt="Imagem 2" style="width: 50%; height: auto; display: block;">
</div>
## 8 - Entendendo o Sistema de Arquivos Hadoop Distributed Filesystem - HDFS

### NFS (Network File System) 

O NFS (Network File System) é um protocolo que permite que sistemas operacionais compartilhem arquivos e diretórios em uma rede. O NFS permite que computadores em uma rede acessem remotamente os arquivos e diretórios como se estivessem localmente armazenados em seus próprios sistemas.

  <img src="_images/801.png" width="50%"></img>

1. **Servidor NFS:** O servidor NFS é o sistema que possui os arquivos e diretórios que serão compartilhados. Ele disponibiliza esses recursos para outros sistemas na rede.
2. **Cliente NFS:** O cliente NFS é o sistema que acessa os arquivos compartilhados. Ele monta os diretórios remotos do servidor NFS como se fossem parte de seu próprio sistema de arquivos.

### GFS (Google File System)

O GFS (Google File System) é um sistema de arquivos distribuído desenvolvido pelo Google para gerenciar grandes volumes de dados em seus centros de dados. Ele foi projetado para fornecer um armazenamento confiável e de alto desempenho, otimizado para a leitura e gravação eficientes de grandes arquivos, como aqueles usados em aplicações de pesquisa na web e processamento de dados em larga escala.

1. **Distribuição de Dados:** O GFS divide grandes arquivos em blocos fixos (geralmente de 64 ou 128 megabytes) e distribui esses blocos entre vários servidores para permitir o paralelismo e a recuperação de falhas.
2. **Replicação:** Cada bloco de dados é replicado em vários servidores para garantir a durabilidade e a disponibilidade dos dados, mesmo em caso de falha de hardware ou interrupção de serviços.
3. **Mestre (Master) e Trabalhadores (Workers):** O GFS possui um servidor mestre que gerencia os metadados e coordena as operações no sistema. Os servidores trabalhadores armazenam os dados e respondem às solicitações de leitura e gravação dos clientes.

<img src="_images/802.png" width="50%"></img>

### Hadoop Distributed File System (HDFS)

O HDFS, ou Hadoop Distributed File System, é um sistema de arquivos distribuído desenvolvido para lidar com o armazenamento e processamento eficientes de grandes conjuntos de dados em ambientes de computação distribuída. Projetado como parte integrante do ecossistema Hadoop, o HDFS divide arquivos em blocos de tamanho fixo, distribuindo-os em diversos nós de um cluster. Essa abordagem facilita a leitura e gravação paralelas, possibilitando o processamento eficiente de dados em larga escala. Com mecanismos de replicação para tolerância a falhas, balanceamento dinâmico de carga e integração com ferramentas Hadoop, o HDFS é essencial para operações de big data, suportando aplicações como análise de dados, processamento de logs e outras tarefas intensivas em armazenamento e processamento.

1. **Blocos e Distribuição de Dados:** O HDFS divide grandes arquivos em blocos, geralmente de tamanho fixo (por exemplo, 128 MB ou 256 MB).
2. **Servidores Namenode e Datanode:**
   - O Namenode mantém os metadados, como informações sobre a localização dos blocos e a estrutura do arquivo.
   - Os Datanodes armazenam os blocos de dados e respondem às solicitações de leitura e gravação.
3. **Leitura e Gravação em Paralelo:**
   - O HDFS permite a leitura e gravação eficientes de grandes conjuntos de dados em paralelo.
   - Múltiplos nós podem acessar e processar diferentes partes do arquivo simultaneamente.

<img src="_images/803.png" width="50%"></img>

A arquitetura de Rack no HDFS (Hadoop Distributed File System) refere-se à organização física dos nós de dados em racks (estruturas de armazenamento) em um data center. Essa organização é fundamental para otimizar o desempenho e garantir a tolerância a falhas. 


A arquitetura de Rack no HDFS (Hadoop Distributed File System) refere-se à organização física dos nós de dados em racks (estruturas de armazenamento) em um data center. Essa organização é fundamental para otimizar o desempenho e garantir a tolerância a falhas. A arquitetura de Rack no HDFS é projetada com base nos seguintes conceitos:

1. **Nós e Racks:**
   - Um cluster HDFS consiste em vários nós, e esses nós são agrupados em racks.
   - Cada rack contém vários nós de dados.
2. **Localidade de Dados:**
   - O princípio chave é maximizar a localidade de dados, significando que o processamento de dados deve ocorrer o mais próximo possível dos dados armazenados.
   - Isso reduz a latência de acesso aos dados, pois evita transferências desnecessárias pela rede.
3. **Princípio de Colocação de Réplicas:**
   - O HDFS segue o princípio de colocar a primeira réplica localmente no mesmo nó, a segunda em um rack diferente, e a terceira em outro rack distante.
   - Isso garante que haja redundância e recuperação de falhas enquanto mantém a eficiência de localidade de dados.

<img src="_images/804.svg" width="50%"></img>

O HDFS fornece uma interface de linha de comando (CLI) que permite interagir com o sistema de arquivos distribuído. Aqui estão alguns comandos fundamentais do HDFS no CLI:

1. **`hadoop fs -ls <caminho>`:**
   - Lista o conteúdo de um diretório no HDFS.
   - Exemplo: `hadoop fs -ls /user/nome_usuario`.
2. **`hadoop fs -mkdir <caminho>`:**
   - Cria um diretório no HDFS.
   - Exemplo: `hadoop fs -mkdir /user/nome_usuario/diretorio_novo`.
3. **`hadoop fs -put <origem> <destino>`:**
   - Copia arquivos ou diretórios do sistema de arquivos local para o HDFS.
   - Exemplo: `hadoop fs -put arquivo_local.txt /user/nome_usuario/diretorio_hdfs/`.
4. **`hadoop fs -get <origem> <destino>`:**
   - Copia arquivos ou diretórios do HDFS para o sistema de arquivos local.
   - Exemplo: `hadoop fs -get /user/nome_usuario/diretorio_hdfs/arquivo_hdfs.txt .` (o ponto final representa o diretório atual).
5. **`hadoop fs -cat <caminho>`:**
   - Exibe o conteúdo de um arquivo no HDFS.
   - Exemplo: `hadoop fs -cat /user/nome_usuario/arquivo_hdfs.txt`.
6. **`hadoop fs -rm <caminho>`:**
   - Remove um arquivo ou diretório do HDFS.
   - Exemplo: `hadoop fs -rm /user/nome_usuario/arquivo_hdfs.txt`.
7. **`hadoop fs -cp <origem> <destino>`:**
   - Copia arquivos ou diretórios dentro do HDFS.
   - Exemplo: `hadoop fs -cp /user/nome_usuario/arquivo_hdfs.txt /user/nome_usuario/diretorio_destino/`.
8. **`hadoop fs -mv <origem> <destino>`:**
   - Move arquivos ou diretórios dentro do HDFS.
   - Exemplo: `hadoop fs -mv /user/nome_usuario/arquivo_hdfs.txt /user/nome_usuario/diretorio_destino/`.
9. **`hadoop fs -chmod <permissões> <caminho>`:**
   - Modifica as permissões de um arquivo ou diretório no HDFS.
   - Exemplo: `hadoop fs -chmod 755 /user/nome_usuario/arquivo_hdfs.txt`.
10. **`hadoop fs -chown <proprietário:grupo> <caminho>`:**
    - Modifica o proprietário e o grupo de um arquivo ou diretório no HDFS.
    - Exemplo: `hadoop fs -chown nome_novo:grupo_novo /user/nome_usuario/arquivo_hdfs.txt`.

<img src="_images/803.png" width="50%"></img>

1. **NameNode (Mestre):**
   - O NameNode é o mestre do sistema de arquivos HDFS.
   - Armazena metadados, como a estrutura do sistema de arquivos, informações sobre os blocos de dados e os locais dos DataNodes.
   - Mantém um registro de todos os arquivos, diretórios e suas respectivas estruturas hierárquicas.
   - Gerencia os namespaces e controla as operações de leitura e gravação, coordenando a comunicação com os DataNodes.
2. **DataNodes (Nós de Dados):**
   - Os DataNodes são responsáveis pelo armazenamento efetivo dos dados.
   - Armazenam os blocos de dados e replicam esses blocos conforme instruído pelo NameNode.
   - Periodicamente, enviam relatórios de status para o NameNode para informar sobre a saúde e disponibilidade.
   - Executam operações de leitura e gravação conforme instruído pelo cliente ou pelo NameNode.
3. **Secondary NameNode:**
   - Apesar do nome, o Secondary NameNode não é um substituto para o NameNode principal.
   - Ele realiza operações de backup regulares dos metadados do NameNode para evitar perda de dados em caso de falha do NameNode.
   - Não assume automaticamente as funções do NameNode em caso de falha; sua principal responsabilidade é manutenção e backup.

**Fluxo de Operação:**

- Quando um cliente deseja ler ou gravar dados, ele se comunica com o NameNode para obter informações sobre a localização dos blocos de dados.
- O NameNode responde com os locais dos blocos de dados nos DataNodes.
- O cliente então interage diretamente com os DataNodes para acessar ou modificar os dados.
