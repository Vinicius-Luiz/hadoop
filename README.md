# Curso de Hadoop

*https://www.youtube.com/playlist?list=PLeFetwYAi-F_l-NP-TUE2MqKeu_haMP79* 

## 01 - Baixando e configurando a máquina Cloudera

**Download Cloudera-VM**<br>

https://downloads.cloudera.com/demo_vm/virtualbox/cloudera-quickstart-vm-5.13.0-0-virtualbox.zip

**Download Putty**<br>

https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

**Download WinSCP**<br>

https://winscp.net/download/WinSCP-6.1.2-Setup.exe

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

**NAMENOME (MESTRE)** armazena a árvore de diretórios do sistema de arquivos, metadados de arquivos e as localizações de cada arquivo nc cluster. ele não armazena dados e nem passa dados do datanode ao cliente, o que ele faz é apontar os datanodes corretos aos clientes
**NAMENOME SECUNDÁRIO (MESTRE)** executam tarefas de manutenção (housekeeping) e de pontos de verificação (checkpointing) em nome do namenode (ele não é um namenode de backup)
**DATANODE (TRABALHADOR)** armazena e administra blocos HDFS no disco local e informa a saúde e o status de repositórios individuais de dados ao NAMENOME.
