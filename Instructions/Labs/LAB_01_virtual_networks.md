---
lab:
  title: 'Exercício: fornecer isolamento e segmentação de rede para o aplicativo Web'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Laboratório: Forneça uma rede virtual de hub de serviços compartilhados com isolamento e segmentação

## Cenário

Você foi encarregado de aplicar os [princípios Confiança Zero](https://learn.microsoft.com/security/zero-trust/azure-infrastructure-networking) a uma rede virtual de hub no Azure. O departamento de TI precisa de isolamento e segmentação de rede para a aplicação web em uma rede falada. Para fornecer isolamento e segmentação de rede para a aplicação web, é necessário criar uma rede virtual Azure com sub-redes com espaço de endereço fornecido pela equipa de TI. Após a rede virtual ser criada, a próxima etapa é configurar o emparelhamento de rede virtual. Isso permite que as redes virtuais se comuniquem entre si de modo seguro e privado.

### Diagrama de arquitetura

![Diagrama que mostra duas redes virtuais emparelhadas.](../Media/task-1.png)

### Tarefas de habilidades

- Criar uma rede virtual
- Criar uma sub-rede
- Configurar o emparelhamento VNet

## Instruções para o exercício

>**Observação**: para concluir este laboratório, você precisará de uma [Assinatura do Azure](https://azure.microsoft.com/free/) com a função RBAC do **Colaborador** atribuída.

> Neste laboratório, quando for solicitado que você crie um recurso, para qualquer propriedade que não for especificada, use o valor padrão.

### Criar redes e sub-redes virtuais hub and spoke

Comece criando as redes virtuais mostradas no diagrama acima.

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

1. Depois que as implantações forem concluídas, pesquise e selecione seu **grupo de recursos**. Confirme se suas novas redes virtuais fazem parte do grupo de recursos. 

### Configurar uma relação de par entre as redes virtuais

1. Pesquise e selecione a rede virtual `app-vnet`.
   
1. Na folha **Configurações**, selecione **Emparelhamentos**.
   
1. Clique em **+ Adicionar** umemparelhamento entre as duas redes virtuais. 

    | Propriedade                                 | Valor                          |
    | :--------------------------------------- | :----------------------------- |
    | Nome do link de emparelhamento              | `app-vnet-to-hub` |
    | Rede virtual    | `hub-vnet` |
    | Nome do link do emparelhamento de rede virtual | `hub-to-app-vnet` |

    **Observação**: deixe todas as outras configurações como padrão. Selecione **"Adicionar"** para criar o emparelhamento de rede virtual.

    [Saiba mais sobre o Emparelhamento de Rede Virtual](https://learn.microsoft.com/azure/virtual-network/virtual-network-manage-peering?tabs=peering-portal)

1. Depois que a implantação for concluída, verifique se o **Status do emparelhamento** é **Conectado**. 
