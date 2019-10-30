# SDN: Software Defined Networking

O objetivo da SDN é estabelecer interfaces abertas que possam controlar o fluxo de
pacotes, além de modificar e inspecionar o tráfego, de um conjunto de recursos de
rede a fim de estabelecer conectividade entre diferentes entidades na rede.

O plano de dados é composto por elementos de rede (roteadores, comutadores etc.)
que anunciam os seus recursos de encaminhamento ou processamento de tráfego ao
controlador SDN através da D-CPI (Data-Controller Plane Interface), ou interface
Southbridge. As aplicações residem no plano de aplicações e comunicam os seus
requerimentos através da A-CPI (Application-Controller Plane Interface), também
conhecida como interface Northbridge. O controlador se encontra no meio no plano de
controle, e tem como função traduzir os pedidos recebidos das aplicações em controle
de baixo nível sobre um conjunto exclusivo de recursos dos elementos de rede, além
de enviar estatísticas e informações relevantes sobre a rede às aplicações.

A **Figura 1** mostra um mapa lógico da arquitetura e introduz o plano de gerenciamento,
cuja função principal é alocar recursos de um plano inferior para uma entidade no
plano superior, além de instalar políticas corporativas e de segurança através do
coordenador nas instâncias de cada plano. A arquitetura foi desenvolvida em um
esquema hierárquico, onde a comunicação entre elementos de níveis diferentes, por
exemplo entre planos distintos, ocorre de acordo com o modelo cliente-servidor.

<img class='center' src='assets/images/Mapa SDN.jpg' alt='Mapa SDN'>
<figcaption style='text-align:center;'><b>Figura 1</b>: Mapa da arquitetura SDN. Os diferentes planos se comunicam exclusivamente
através da A-CPI e da D-CPI. Os gerentes são responsáveis pela inicialização dos elementos
em cada um dos três planos e pela instalação de políticas corporativas e de segurança.</figcaption>

A fim de permitir que aplicações de corporações distintas tenham acesso direto ao
controlador SDN, a arquitetura incorpora o conceito de domínios de confiança através
dos agentes, o elemento funcional que define os recursos e o escopo do cliente no
servidor. Por exemplo, na figura acima o agente vermelho no controlador representa
todos os recursos de rede (propriamente virtualizados abstraídos) disponíveis à
aplicação vermelha no plano de aplicação segundo as políticas de segurança definida
pelo gerente vermelho no plano de gerenciamento. Por outro lado, o agente em um
elemento de rede no pano de dados representa o conjunto de recursos de
encaminhamento e processamento disponíveis àquele controlador.
A lógica de controle da SDN é o elemento que, de fato, executa a função principal do
controlador, arbitrar entre os pedidos das diversas aplicações e exercer controle sobre
os elementos de rede.
A abstração dos recursos de rede em conjunto com o anúncio de estatística e estado
dos elementos permitem às aplicações controlar o tráfego e processamento de forma
programática, centralizada e padronizada. Além disso, a hierarquização da rede e a
incorporação de domínios de segurança possibilita que diversas aplicações de
diferentes entidades trabalhem em conjunto para fornecer um serviço mais
abrangente.