---
lab:
  title: 'Exercício 04: configurar o roteamento de rede'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Exercício 04: configurar o roteamento de rede

## Cenário

Para garantir que as políticas de firewall sejam aplicadas, o tráfego de aplicativos de saída deve ser roteado pelo firewall. Você identifica esses requisitos. 
+ Uma tabela de rotas é necessária. Essa tabela de rotas será associada às sub-redes de front-end e back-end.  
+ Uma rota é necessária para filtrar todo o tráfego IP de saída das sub-redes para o firewall. O endereço IP privado do firewall será usado. 

## Tarefas de habilidades

+ Criar e configurar uma tabela de rotas.
+ Vincular uma tabela de rotas a uma sub-rede.
  
## Diagrama de arquitetura

![Diagrama que mostra uma rede virtual com um firewall e uma tabela de rotas.](../Media/task-3.png)


## Instruções para o exercício

### Criar uma tabela de rotas

O Azure cria automaticamente uma [tabela de rotas](https://learn.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) para cada sub-rede dentro de uma rede virtual do Azure. A tabela de rotas padrão inclui as [rotas padrão do sistema](https://learn.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#system-routes): Você pode criar tabelas de rotas e rotas para substituir as rotas padrão do sistema do Azure.

**Registrar o endereço IP privado de app-vnet-firewall.**

1. Na caixa de pesquisa na parte superior do portal, insira **Firewall**. Selecione **Firewall** nos resultados da pesquisa.

1. Selecione **app-vnet-firewall**.

1. Selecione **Visão geral** e registre o **endereço IP privado**.

**Adicione a tabela de rotas**

1. Na caixa de pesquisa, insira **Tabela de rotas**. Quando a Tabela de rotas é exibida nos resultados da pesquisa, selecione-a.

1. Na página Tabela de rotas, clique em **+ Criar** e crie a tabela de rotas. 

    | Propriedade       | Valor                        |
    | :------------- | :--------------------------- |
    | Subscription   | **Selecione sua assinatura** |
    | Grupo de recursos | **RG1**                      |
    | Region         | **Leste dos EUA**                  |
    | Nome           | `app-vnet-firewall-rt`     |

1. Selecione **Examinar + criar** e **Criar**.

1. Aguarde a conclusão da implantação da tabela de rotas e selecione **Ir para o recurso**.  

### Associar a tabela de rotas à sub-rede

1. No portal, continue trabalhando com a tabela de rotas, selecione **app-vnet-firewall-rt**.

1. Na folha **Configurações**, escolha **Sub-redes** e, em seguida, **+ Associar**.

1. Configure uma associação à sub-rede de front-end e clique em **OK.**  

    | Propriedade        | Valor              |
    | :-------------- | :----------------- |
    | Rede virtual | **app-vnet** (RG1) |
    | Sub-rede          | **frontend**       |

1. Configure uma associação à sub-rede de back-end e selecione **OK.**  

    | Propriedade        | Valor              |
    | :-------------- | :----------------- |
    | Rede virtual | **app-vnet** (RG1) |
    | Sub-rede          | **frontend**       |

### Crie uma rota na tabela de rotas

1. No portal, continue trabalhando com a tabela de rotas, selecione **app-vnet-firewall-rt**.

1. Na folha **Configurações**, selecione **Rotas** e, em seguida, **+ Adicionar**.

1. Configure a rota e selecione **Adicionar**. 

    | Propriedade                            | Valor                                                   |
    | :---------------------------------- | :------------------------------------------------------ |
    | Nome da rota                          | **outbound-firewall**                                   |
    | Tipo de destino                    | **Endereços IP**                                        |
    | Intervalo de CIDR /endereço IP de destino | **0.0.0.0/0**                                           |
    | Tipo do próximo salto                       | **Solução de virtualização**                                   |
    | Endereço do próximo salto                    | **endereço IP privado do firewall** |


### Saiba mais com o treinamento online

+ [Gerenciar e controlar o fluxo de tráfego em sua implantação do Azure com rotas](https://learn.microsoft.com/training/modules/control-network-traffic-flow-with-routes/). Neste módulo, você aprenderá a controlar o tráfego de rede virtual do Azure implementando rotas personalizadas. Este módulo tem duas áreas restritas. 

### Principais aspectos a serem lembrados

Parabéns por concluir o exercício. Estas foram as principais conclusões:

+ O tráfego de rede no Azure é roteado automaticamente entre sub-redes do Azure, redes virtuais e redes locais. As rotas do sistema controlam esse roteamento.
+ Rotas definidas pelo usuário substituem as rotas padrão do sistema para que o tráfego possa ser roteado por meio de NVAs (soluções de virtualização de rede). 
+ As NVAs (soluções de virtualização de rede) controlam o fluxo do tráfego de rede. Exemplos de NVAs são firewalls, balanceadores de carga e roteadores.
+ As tabelas de rotas contêm informações de roteamento e estão associadas a uma sub-rede. 
