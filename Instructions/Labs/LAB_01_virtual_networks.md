---
lab:
  title: 'Exercício 01: criar e configurar redes virtuais'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Exercício 01: criar e configurar redes virtuais

## Cenário

Sua organização está migrando um aplicativo baseado na Web para o Azure. Sua primeira tarefa é implementar as redes virtuais e sub-redes. Você também precisa emparelhar as redes virtuais com segurança. Você identifica esses requisitos. 
+ Duas redes virtuais são necessárias, **app-vnet** e **hub-vnet**. Isso simula uma arquitetura de rede hub and spoke. 
+ O app-vnet hospedará o aplicativo. Essa rede virtual requer duas sub-redes. A **sub-rede de front-end** hospedará os servidores Web. A **sub-rede de back-end ** hospedará os servidores de banco de dados.
+ O hub-vnet requer apenas uma sub-rede para o firewall. 
+ As duas redes virtuais devem ser capazes de se comunicar entre si de forma segura e privada por meio do **emparelhamento de rede virtual**. 
+ As duas redes virtuais emparelhadas devem estar na mesma região. 

## Tarefas de habilidades

+ Crie uma rede virtual.
+ Crie uma sub-rede.
+ Configurar o emparelhamento de VNets.

## Diagrama de arquitetura

![Diagrama que mostra duas redes virtuais emparelhadas.](../Media/task-1.png)

## Instruções para o exercício

**Observação**: para concluir este laboratório, você precisará de uma [Assinatura do Azure](https://azure.microsoft.com/free/) com a função RBAC do **Colaborador** atribuída. Neste laboratório, quando for solicitado que você crie um recurso, para qualquer propriedade que não for especificada, use o valor padrão.

### Criar redes e sub-redes virtuais hub and spoke

Uma [rede virtual do Azure](https://learn.microsoft.com/azure/virtual-network/virtual-networks-overview) permite que muitos tipos de recursos do Azure se comuniquem com segurança entre si, com a Internet e com redes locais. Todos os recursos do Azure em uma rede virtual são implantados em [sub-redes](https://learn.microsoft.com/azure/virtual-network/virtual-network-manage-subnet?tabs=azure-portal) dentro da rede virtual. 

1. Entre no **portal do Azure** - `https://portal.azure.com`.
   
1. Pesquise e selecione `Virtual Networks`.
   
1. Clique em **+ Criar** e conclua a configuração do **app-vnet**. Essa rede virtual requer duas sub-redes, **front-end** e **back-end**. 

    | Propriedade             | Valor           |
    | :------------------- | :-------------- |
    | Resource group       | **RG1**         |
    | Nome da rede virtual | `app-vnet`    |
    | Region               | **Leste dos EUA**     |
    | Espaço de endereço IPv4   | **10.1.0.0/16** |
    | Nome da sub-rede          | `frontend`    |
    | Intervalo de endereços da sub-rede | **10.1.0.0/24** |
    | Nome da sub-rede          | `backend`     |
    | Intervalo de endereços da sub-rede | **10.1.1.0/24** |

    **Observação**: deixe todas as outras configurações como padrão. Quando terminar, clique em **Revisar+ Criar** e depois em **Criar**.
   
1. Crie a configuração de rede virtual **Hub-vnet**. Esta rede virtual tem a sub-rede do firewall. 

    | Propriedade             | Valor                    |
    | :------------------- | :----------------------- |
    | Resource group       | **RG1**                  |
    | Nome                 | `hub-vnet` |
    | Region               | **Leste dos EUA**              |
    | Espaço de endereço IPv4   | **10.0.0.0/16**          |
    | Nome da sub-rede          | **AzureFirewallSubnet**  |
    | Intervalo de endereços da sub-rede | **10.0.0.0/24**          |

1. Depois que as implantações forem concluídas, pesquise e selecione as suas "redes virtuais".

1. Verifique se suas redes virtuais e sub-redes foram implantadas. 

### Configurar uma relação de par entre as redes virtuais

O [emparelhamento de rede virtual](https://learn.microsoft.com/azure/virtual-network/virtual-network-peering-overview) permite que você conecte duas ou mais redes virtuais no Azure sem interrupção. 

1. Pesquise e selecione a rede virtual `app-vnet`.
   
1. Na folha **Configurações**, selecione **Emparelhamentos**.
   
1. Clique em **+ Adicionar** umemparelhamento entre as duas redes virtuais. 

    | Propriedade                                 | Valor                          |
    | :--------------------------------------- | :----------------------------- |
    | Nome do link de emparelhamento remoto              | `app-vnet-to-hub` |
    | Rede virtual    | `hub-vnet` |
    | Nome do link do emparelhamento de rede virtual | `hub-to-app-vnet` |

    **Observação**: deixe todas as outras configurações como padrão. Selecione **"Adicionar"** para criar o emparelhamento de rede virtual.

1. Depois que a implantação for concluída, verifique se o **Status do emparelhamento** é **Conectado**.

## Saiba mais com o treinamento online

+ [Introdução às Redes Virtuais do Azure](https://learn.microsoft.com/training/modules/introduction-to-azure-virtual-networks/). Neste módulo, você aprenderá a projetar e implementar os serviços de rede do Azure. Você aprenderá sobre redes virtuais, IPs públicos e privados, DNS, emparelhamento de rede virtual, roteamento e NAT Virtual do Azure.

## Principais aspectos a serem lembrados

Parabéns por concluir o exercício. Estas foram as principais conclusões:

+ As VNets (redes virtuais) do Azure fornecem um ambiente de rede seguro e isolado para os seus recursos de nuvem. É possível criar várias redes virtuais por região por assinatura.
+ Ao projetar redes virtuais, certifique-se de que o espaço de endereço da VNet (bloco CIDR) não se sobreponha aos outros intervalos de rede da sua organização.
+ Uma sub-rede é um intervalo de endereços IP na rede virtual. Você pode segmentar VNets em sub-redes de tamanhos diferentes, criando quantas sub-redes você precisar para organização e segurança dentro do limite de assinatura. Cada sub-rede deve ter um intervalo de endereços exclusivo.
+ Determinados serviços do Azure, como o Firewall do Azure, requerem uma sub-rede própria.
+ O emparelhamento de rede virtual permite que você conecte duas redes virtuais do Azure sem interrupção. As redes virtuais aparecerão como uma só para fins de conectividade.
