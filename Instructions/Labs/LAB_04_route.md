---
lab:
  title: 'Exercício: rotear o tráfego para o firewall'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Laboratório: rotear o tráfego para o firewall


## Cenário

Agora que há um firewall implantado, com políticas que impõem requisitos de segurança às suas organizações, você precisa rotear o tráfego de rede para a sub-rede do firewall a fim de permitir que ele filtre e inspecione o tráfego. As tabelas de rotas fornecem controle sobre o roteamento do tráfego de rede de/para o aplicativo Web. O tráfego de rede está sujeito às regras do firewall quando você direciona o tráfego de rede ao firewall como o gateway padrão da sub-rede. 

### Diagrama de arquitetura


![Diagrama que mostra uma rede virtual com um firewall e uma tabela de rotas.](../Media/task-3.png)

### Tarefas de habilidades

- Criar e configurar uma tabela de rotas.
- Vincular uma tabela de rotas a uma sub-rede.
  

## Instruções para o exercício

### Criar uma tabela de rotas

1. Registre o endereço IP público e privado de **app-vnet-firewall**.

    1. Na caixa de pesquisa na parte superior do portal, insira **Firewall**. Selecione **Firewall** nos resultados da pesquisa.

    1. Selecione **app-vnet-firewall**.

    1. Selecione **Visão geral**.

        1. Registre o **endereço IP privado**.

    1. No painel Visão Geral selecione **fwpip**

    1. Registre o **endereço IP público**. 


1. Na caixa de pesquisa, insira **Tabela de rotas**. Quando a Tabela de rotas é exibida nos resultados da pesquisa, selecione-a.

1. Na página Tabela de rotas, selecione **+ Criar**.

1. Na guia **Noções básicas**, crie uma nova tabela de rotas usando as informações na tabela a seguir:

    | Propriedade | Valor    |
    |:---------|:---------|
    |Subscription|**Selecione sua assinatura**|
    |Resource group|**RG1**|
    |Região|**Leste dos EUA**|
    |Nome|**app-vnet-firewall-rt**|

    

1. Selecione **Examinar + criar** e **Criar**.

    [Saiba mais sobre como criar tabelas de rotas](https://docs.microsoft.com/azure/virtual-network/manage-route-table) e [como associar uma tabela de rotas a uma sub-rede](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal#associate-a-route-table-to-a-subnet).

### Associar a tabela de rotas à sub-rede

1. Na caixa de pesquisa, insira **Tabela de rotas**. e selecione Tabelas de rotas nos resultados da pesquisa.

1. Selecione **app-vnet-firewall-rt**.

1. Selecione **sub-redes**.

1. Selecione **+ Associar**.

1. Na página **Associar sub-rede**, insira as informações listadas na tabela abaixo:

    | Propriedade | Valor    |
    |:---------|:---------|
    |Rede virtual|**app-vnet** (RG1)|
    |Sub-rede|**frontend**|

1. Selecione **OK**.

1. Repita as etapas acima para associar a tabela de rotas **app-vnet-firewall-rt** à sub-rede de **back-end** no **app-vnet**.

### Crie uma rota na tabela de rotas

1. Na caixa de pesquisa, insira **Tabela de rotas**. e selecione Tabelas de rotas nos resultados da pesquisa.

1. Selecione **app-vnet-firewall-rt**.

1. Selecione **Rotas**.

1. Selecione **+ Adicionar**.

1. Na página **Adicionar rota**, insira as informações listadas na tabela a seguir.

    | Propriedade | Valor    |
    |:---------|:---------|
    |Nome da rota|**outbound-firewall**|
    |Tipo de destino|**Endereços IP**|
    |Intervalo de CIDR /endereço IP de destino|**0.0.0.0/0**|
    |Tipo do próximo salto|**Solução de virtualização**|
    |Endereço do próximo salto|**endereço IP privado do firewall registrado anteriormente**|


1. Selecione **Adicionar**.

[Saiba mais sobre como criar rotas](https://docs.microsoft.com/azure/virtual-network/manage-route-table#add-a-route).

Agora, o tráfego de saída da sub-rede de front-end e back-end será roteado para o firewall. 


