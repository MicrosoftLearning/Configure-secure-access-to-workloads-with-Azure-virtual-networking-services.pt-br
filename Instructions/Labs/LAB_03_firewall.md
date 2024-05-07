---
lab:
  title: Exercício – Proteger o aplicativo Web contra tráfego mal-intencionado e bloquear o acesso não autorizado
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Laboratório: proteger o aplicativo Web contra tráfego malicioso e bloquear o acesso não autorizado

## Cenário

Sua organização deseja proteger o aplicativo Web contra tráfego mal-intencionado e bloquear o acesso não autorizado.

Além do NSG e do ASG, um firewall pode ser configurado para adicionar uma camada extra de segurança ao aplicativo Web. Um firewall protege o aplicativo Web contra tráfego mal-intencionado e bloqueia o acesso não autorizado usando as políticas configuradas.

A Política de Firewall do Azure é um recurso de nível superior que contém configurações de segurança e operacionais para o Firewall do Azure. Ela permite que você defina uma hierarquia de regra e imponha a conformidade. Nesta tarefa, você configura regras de aplicativo e regras de rede para o firewall usando a Política de Firewall. É possível usar a Política de Firewall do Azure para gerenciar conjuntos de regras que o Firewall do Azure usa para filtrar o tráfego.

### Diagrama de arquitetura

![Diagrama que mostra uma rede virtual com um firewall e uma tabela de rotas.](../Media/task-3.png)

### Tarefas de habilidades

- Criar um Firewall do Azure.
- Criar e configurar uma política de firewall
- Adicionar uma coleção de regras de aplicativo.
- Criar uma coleção de regras de rede.
  
## Instruções para o exercício

### Criar uma sub-rede do Firewall do Azure em nossa rede virtual existente

1. Na caixa de pesquisa na parte superior do portal, insira **Redes virtuais**. Selecione **Redes virtuais** nos resultados da pesquisa.

1. Selecione **app-vnet**.

1. Selecione **sub-redes**.

1. Selecione **+ Sub-rede**.

1. Insira as informações a seguir e selecione **Salvar**.

    | Propriedade      | Valor                   |
    | :------------ | :---------------------- |
    | Nome          | **AzureFirewallSubnet** |
    | Intervalo de endereços | **10.1.63.0/24**        |

    > **Observação**: deixe todas as outras configurações como padrão.

### Criar um Firewall do Azure

1. Na caixa de pesquisa na parte superior do portal, insira **Firewall**. Selecione **Firewall** nos resultados da pesquisa.

1. Selecione **+ Criar**.

1. Crie um firewall usando os valores na tabela a seguir. Para qualquer propriedade que não seja especificada, use o valor padrão.
    >**Observação**: o Firewall do Azure pode levar alguns minutos para ser implantado.

    | Propriedade                 | Valor                                             |
    | :----------------------- | :------------------------------------------------ |
    | Resource group           | **RG1**                                           |
    | Nome                     | **app-vnet-firewall**                             |
    | SKU do Firewall             | **Standard**                                      |
    | Gerenciamento do firewall      | **Usar uma política de firewall para gerenciar este firewall** |
    | Política de firewall          | selecione **Adicionar nova**                                |
    | Nome de política              | **fw-policy**                                     |
    | Região                   | **Leste dos EUA**                                       |
    | Camada da política              | **Standard**                                      |
    | Escolher uma rede virtual | **Usar existente**                                  |
    | Rede virtual          | **app-vnet** (RG1)                                |
    | Endereço IP público        | Adicionar nova: **fwpip**                                |

    [Saiba mais sobre como criar um firewall](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal).

1. Selecione **Examinar + Criar** e, em seguida, selecione **Criar**.

### Atualizar a Política de Firewall

1. Na caixa de pesquisa na parte superior do portal, insira **Política de Firewall**. Selecione **Políticas de Firewall** nos resultados da pesquisa.

1. Selecione **fw-policy**.

1. Selecione **Regras de aplicativo**.

1. Selecione **"+ Coleção de regras de aplicativo"**.

1. Use os valores na seguinte tabela: Para qualquer propriedade que não seja especificada, use o valor padrão.

    | Propriedade               | Valor                                     |
    | :--------------------- | :---------------------------------------- |
    | Nome                   | **app-vnet-fw-rule-collection**           |
    | Tipo de coleção de regras   | **Aplicativo**                           |
    | Prioridade               | **200**                                   |
    | Ação da coleção de regras | **Permitir**                                 |
    | Grupo de coleções de regras  | **DefaultApplicationRuleCollectionGroup** |

    1. Em **regras**, use os valores na tabela a seguir

        | Propriedade         | Valor                                  |
        | :--------------- | :------------------------------------- |
        | Nome             | **AllowAzurePipelines**                |
        | Tipo de origem      | **Endereço IP**                         |
        | Origem           | **10.1.0.0/23**                        |
        | Protocolo         | **https**                              |
        | Tipo de destino | FQDN                                   |
        | Destino      | **dev.azure.com, azure.microsoft.com** |

        e selecione **Adicionar**

> **Observação**: a regra **AllowAzurePipelines** permite que o aplicativo Web acesse o Azure Pipelines . A regra permite que o aplicativo Web acesse o serviço Azure DevOps e o site do Azure.

1. Crie uma **coleção de regras de rede** que contenha uma regra de endereço IP usando os valores na tabela a seguir. Para qualquer propriedade que não seja especificada, use o valor padrão.

1. Selecione **Regras de rede**.

1. Selecione **"+ Coleção de regras de rede"**.

1. Use os valores na seguinte tabela: Para qualquer propriedade que não seja especificada, use o valor padrão.

    | Propriedade               | Valor                                 |
    | :--------------------- | :------------------------------------ |
    | Nome                   | **app-vnet-fw-nrc-dns**               |
    | Tipo de coleção de regras   | **Rede**                           |
    | Prioridade               | **200**                               |
    | Ação da coleção de regras | **Permitir**                             |
    | Grupo de coleções de regras  | **DefaultNetworkRuleCollectionGroup** |

    1. Em **regras**, use os valores na tabela a seguir

        | Propriedade              | Valor                |
        | :-------------------- | :------------------- |
        | Regra                  | **AllowDns**         |
        | Origem                | **10.1.0.0/23**      |
        | Protocolo              | **UDP**              |
        | Portas de destino     | **53**               |
        | Endereços de destino | **1.1.1.1, 1.0.0.1** |

        e selecione **Adicionar**.

    Saiba mais sobre [como criar uma regra de aplicativo](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal#configure-an-application-rule) e [como criar uma regra de rede](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal#configure-a-network-rule).

1. Para verificar se o estado de provisionamento do Firewall do Azure e da Política de Firewall mostra **Bem-sucedido**.

1.Na caixa de pesquisa na parte superior do portal, insira **Firewall**. Selecione **Firewall** nos resultados da pesquisa.

1. Selecione **app-vnet-firewall**.

1– Validar se o **Estado de provisionamento** é **Bem-sucedido**.

1– Na caixa de pesquisa na parte superior do portal, insira **Políticas de firewall**. Selecione **Políticas de firewall** nos resultados da pesquisa

1. Selecione **fw-policy**.

1– Validar se o **Estado de provisionamento** é **Bem-sucedido**.
