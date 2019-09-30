O encaminhamento de pacotes tradicional faz uso somente do endereço de IP de destino para decidir a placa de rede para qual enviar o datagrama em questão. O encaminhamento generalizado, por outro lado, inspeciona endereços de origem e de destino de diferentes camadas para tomar decisões variadas, como modificar valores de cabeçalhos, inspecionar o pacote e encaminhá-lo à uma interface específica. O conjunto de endereços utilizados para a identificação dos pacotes define um fluxo, e o conjunto de fluxos e suas respectivas ações define uma tabela de fluxos.
O principal protocolo de comunicação na Southbridge da SDN, o OpenFlow, tem como função a transmissão de tabelas de fluxos entre os dispositivos de rede e o controlador. A Figura 2 mostra uma tabela de fluxos, onde cada linha pode ser programada e redefinida dinamicamente de acordo com as políticas de gerenciamento de rede, dando à SDN grande flexibilidade para lidar com as mudanças na demanda.

<img style="display: block;margin-left: auto;margin-right: auto;" src="assets/Images/openflowtable.png">