# A Arquitetura SDN em Detalhe

Como apresentado anteriormente, o novo paradigma SDN adota um modelo
hierárquico do tipo cliente-servidor que define a forma como as aplicações,
controladores e elementos de rede trocam informação sobre a rede. Como visto na
Figura 2, todas as instâncias dos elementos da arquitetura possuem os seguintes
blocos funcionais em comum:

+ **Agente**: Funciona como o servidor na instância de nível N.
+ **Lógica de Controle (exceto os ERs)**: Age como o cliente na instância de nível
N+1.

Apesar da estrutura hierárquica, todas as interfaces são bidirecionais. O motivo pela
adoção do modelo cliente-servidor é a crescente abstração da topologia, recursos e
informação sobre a rede com o nível hierárquico; por exemplo, o plano de controle tem
uma visão virtualizada e abstraída dos recursos anunciados e alocados pelo plano de
dados. A documentação fornecida pela ONS (Open Networking Foundation) nomeia as
interfaces de servidor de A-CPI e as de cliente de D-CPI.

<img class='center' src='assets/images/Hierarquia.jpg' alt='Hierarquia SDN'>
<figcaption style='text-align:center;'><b>Figura 2</b>: Estrutura hierárquica da arquitetura SDN. Os blocos cinza representam instâncias de
aplicações, controladores e elementos de rede.</figcaption>

## 3.1 O Plano de Dados
A **Figura 3** expande o conceito de elemento de rede mostrando alguns blocos a mais.
O plano de dados incorpora os recursos físicos que lidam com o tráfego na rede
através do bloco funcional de Recursos do ER (Elemento de Rede), que contém uma
fonte e um destino de dados, ferramentas de encaminhamento e processamento de
pacotes (define-se como recursos de rede) e um virtualizador, cuja função é fornecer
uma versão abstrata de tais recursos ao controlador SDN e implementar políticas
corporativas e de segurança. O bloco RDB (Resource Data Base) Mestre, o Banco de
Dados de Recursos Mestre, age como um repositório global para as ferramentas do
elemento de rede.

<img class='center' src='assets/images/Detalhe do Plano de Dados.jpg' alt='Detalhe do plano de dados SDN'>
<figcaption style='text-align:center;'><b>Figura 3</b>: Estrutura funcional detalhada do plano de dados. Apesar de não mostrado na
imagem, cada ER pode estar subordinado a vários controladores, cada um com o seu agente.
O virtualizador tem como função abstrair os recursos físicos do ER e apresenta-los aos
diferentes agentes que possam residir no mesmo.</figcaption> 

O elemento funcional que implementa as decisões de encaminhamento e
processamento de pacotes definidas pelo controlador SDN no plano de dados é o
agente, unicamente associado a um controlador. O Coordenador é a entidade que
aloca recursos de diferentes elementos de rede a cada agente e implementa as
políticas definidas pelo plano de gerenciamento.

A D-CPI é a única interface pela qual os planos de dados e de controle se comunicam
e tem como objetivo implementar certas funções como:
+ Controle programático de todos os recursos expostos pelo RDB Mestre.
+ Anúncio de competências.
+ Notificação de eventos.
  
## 3.2 O Plano de Controle

A arquitetura SDN não especifica o desenho interno do controlador, ao invés disso ela
define as interfaces de comunicação e o modelo comportamental do elemento,
efetivamente criando uma “caixa preta”. O propósito dos elementos funcionais
apresentados a seguir é exclusivamente de clarificar certos aspectos do
funcionamento do controlador.

O controle na rede SDN é logicamente centralizado e não há disputa por recursos, ou
seja, cada controlador tem autoridade exclusiva sobre um conjunto de recursos de
rede alocados a ele pelo gerente. O cálculo de rota e o conhecimento da topologia de
rede são algumas das funções implementadas pelo controlador.

<img class='center' src='assets/images/Detalhe do Plano de Controle.jpg' alt='Detalhe do plano de controle SDN'>
<figcaption style='text-align:center;'><b>Figura 4</b>: Mapa detalhado dos blocos funcionais 
do controlador SDN. Note que o plano de controle pode hospedar vários controladores, 
cada um com o seu DCPF que se comunica com uma série de agentes em ERs diferentes. 
O virtualizador é responsável por abstrair tais detalhes para os agentes de instâncias
de nível mais alto, fornecendo a eles uma visão abstrata da rede e dos recursos subordinados ao controlador.</figcaption> 


A **Figura 4** introduz alguns dos elementos funcionais do controlador SDN, como o RDB
Mestre, que aqui tem a mesma função que no elemento de rede, porém lida com uma
versão virtualizada dos recursos físicos presentes no plano inferior (segundo o
virtualizador visto na seção anterior) e alocados ao controlador em questão. Os
agentes têm o mesmo comportamento em todos os planos da arquitetura e
representam os recursos e requerimentos das aplicações dentro do controlador.

O bloco funcional de lógica de controle SDN merece especial atenção pois é o
responsável pela tradução dos pedidos feitos pelas aplicações em controle de baixo
nível sobre os recursos subordinados, processo executado em duas etapas:

1. O virtualizador recebe os pedidos vindos da A-CPI, os valida segundo as políticas
corporativas e de segurança, os traduz em termos dos recursos do controlador e
então os passa ao DCPF (Data Plane Control Function).
2. O DCPF emite comandos para os recursos subordinados que serão executados no
plano de dados pelos agentes relevantes associados ao controlador SDN.

### 3.2.1 Alguns Controladores Comerciais
O mercado possui várias soluções para controladores SDN, tanto comerciais quanto
de código aberto. Algumas delas são:
+ NOX/POX: O primeiro controlador SDN de código aberto, foi entregue à
comunidade em 2009 que subsequentemente serviu como base para diversos
outros projetos como o POX, que adiciona suporte ao Python.
+ OpenDaylight: Outro controlador de código aberto que roda exclusivamente
dentro de uma máquina virtual Java (JVM) e possui suporte ao OpenFlow, bem
como protocolos Southbridge de diversos fabricantes como a Cisco.
+ ONOS: Um controlador desenvolvido para redes de grande porte, como
aquelas geridas por provedores de backbone. Altamente escalável e com
diversificado suporte a políticas corporativas, o ONOS está sob constante
desenvolvimento pela comunidade da ONF.

## 3.3 O Plano de Aplicações
A arquitetura SDN permite que as aplicações requeiram recursos e comportamentos
da rede, tarefa realizada através da A-CPI. Segundo a Figura 5, cada aplicação
também suporta uma própria A-CPI, o que permite que programas sejam agregados
de forma hierárquica para a realização de uma tarefa mais abrangente. Cada
aplicação pode estar associada a um número de controladores para realizar as suas
tarefas, segundo as funções de gerenciamento; esta associação está fora do controle
da aplicação e é feita automaticamente pelo plano de gerenciamento segundo os seus
requerimentos de recursos e comportamentos.

<img class='center' src='assets/images/Detalhe do Plano de Aplicação.jpg' alt='Detalhe do plano de aplicações SDN'>
<figcaption style='text-align:center;'><b>Figura 5</b>: Detalhe do plano de aplicação. 
A natureza hierárquica das instâncias na arquitetura SDN permite que várias aplicações 
se comuniquem através de agentes A-CPI de forma natural. Cada aplicação pode, por sua vez,
enviar os seus pedidos à diversos controladores,segundo a alocação feita pelo coordenador.</figcaption> 

A estrutura hierárquica recursiva suportada pela arquitetura faz com que as aplicações
possam se comportar de diferentes maneiras, segundo o esquema cliente-servidor
adotado:
+ Uma aplicação pode se comportar como uma instância de servidor, fornecendo
informações a outras aplicações.
+ Uma aplicação pode se comportar com um cliente, operando sobre informação
exposta por uma aplicação instanciada como servidor.
+ Uma aplicação pode se comportar como ambos, recebendo e fornecendo
informações. Por exemplo, uma aplicação de cálculo de rota recebe uma visão
virtualizada e completa da rede e fornece o cálculo de rota ao controlador SDN.