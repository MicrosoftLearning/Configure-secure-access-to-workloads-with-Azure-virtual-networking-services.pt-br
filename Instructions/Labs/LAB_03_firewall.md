---
lab:
  title: 'Exercício 03: criar e configurar o Firewall do Azure'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Exercício 03: criar e configurar o Firewall do Azure

## Cenário

Sua organização requer segurança de rede centralizada para a rede virtual do aplicativo. À medida que o uso do aplicativo aumenta, será necessária uma filtragem mais granular no nível do aplicativo e proteção contra ameaças avançada. Além disso, espera-se que o aplicativo precise de atualizações contínuas de pipelines do Azure DevOps. Você identifica esses requisitos.
+ O Firewall do Azure é necessário para segurança adicional no app-vnet. 
+ Uma **política de firewall** deve ser configurada para ajudar a gerenciar o acesso ao aplicativo. 
+ É necessária uma **regra do aplicativo** da política de firewall. Essa regra permitirá que o aplicativo acesse o Azure DevOps para que o código do aplicativo possa ser atualizado. 
+ É necessária uma **regra de rede** da política de firewall. Essa regra permitirá a resolução de DNS. 

### Tarefas de habilidades

+ Criar um Firewall do Azure.
+ Criar e configurar uma política de firewall
+ Adicionar uma coleção de regras de aplicativo.
+ Criar uma coleção de regras de rede.

## Diagrama de arquitetura

![Diagrama que mostra uma rede virtual com um firewall e uma tabela de rotas.](../Media/task-3.png)


  
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

**Observação**: deixe todas as outras configurações como padrão.

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

1. No portal, pesquise e selecione `Firewall Policies`. 

1. Selecione **fw-policy**.

### Adicionar uma regra de aplicativo

1. Na folha **Configurações**, selecione **Regras de aplicativo** e, em seguida, **Adicionar uma coleção de regras**.

1. Selecione a coleção de regras de aplicativo e, em seguida, **Adicionar**. 

    | Propriedade               | Valor                                     |
    | :--------------------- | :---------------------------------------- |
    | Nome                   | `app-vnet-fw-rule-collection`         |
    | Tipo de coleção de regras   | **Aplicativo**                           |
    | Prioridade               | `200`                                   |
    | Ação da coleção de regras | **Permitir**                                 |
    | Grupo de coleções de regras  | **DefaultApplicationRuleCollectionGroup** |
    | Nome             | `AllowAzurePipelines`                |
    | Tipo de origem      | **Endereço IP**                         |
    | Origem           | `10.1.0.0/23`                       |
    | Protocolo         | `https`                             |
    | Tipo de destino | **FQDN**                                  |
    | Destino      | `dev.azure.com, azure.microsoft.com` |

**Observação**: a regra **AllowAzurePipelines** permite que o aplicativo Web acesse o Azure Pipelines . A regra permite que o aplicativo Web acesse o serviço Azure DevOps e o site do Azure.

### Adicionar uma regra de rede

1. Na folha **Configurações**, selecione **Regras de rede** e, em seguida, **Adicionar uma coleção de rede**.

1. Configure a regra de rede e selecione **Adicionar**.  

    | Propriedade               | Valor                                 |
    | :--------------------- | :------------------------------------ |
    | Nome                   | `app-vnet-fw-nrc-dns`               |
    | Tipo de coleção de regras   | **Rede**                           |
    | Prioridade               | `200`                        |
    | Ação da coleção de regras | **Permitir**                             |
    | Grupo de coleções de regras  | **DefaultNetworkRuleCollectionGroup** |
    | Regra                  | **AllowDns**         |
    | Origem                | `10.1.0.0/23`      |
    | Protocolo              | **UDP**              |
    | Portas de destino     | `53`               |
    | Endereços de destino | **1.1.1.1, 1.0.0.1** |

### Verificar o firewall e o status da política de firewall

1. No portal, pesquise e selecione **Firewall**. 

1. Exiba o **app-vnet-firewall** e verifique se o **Estado de provisionamento** é **Bem-sucedido**. Isso pode levar alguns minutos. 

1. No portal, pesquise e selecione **Políticas de Firewall**.

1. Visualize a **fw-policy** e verifique se o **Estado de provisionamento** é **Bem-sucedido**. Isso pode levar alguns minutos.

### Saiba mais com o treinamento online

+ [Introdução ao Firewall do Azure](https://learn.microsoft.com/training/modules/introduction-azure-firewall/). Neste módulo, você aprenderá sobre os recursos, as regras, as opções de implantação e a administração do Firewall do Azure.
+ [Introdução ao Gerenciador de Firewall do Azure](https://learn.microsoft.com/training/modules/intro-to-azure-firewall-manager/). Neste módulo, você aprenderá como o Gerenciador de Firewall do Azure fornece política de segurança central e gerenciamento de rotas para parâmetros de segurança baseados em nuvem.

### Principais aspectos a serem lembrados

Parabéns por concluir o exercício. Estas foram as principais conclusões:

+ O Firewall do Azure é um serviço de segurança baseado em nuvem que protege seus recursos da rede virtual do Azure contra ameaças de entrada e de saída.
+ Uma política de firewall do Azure é um recurso que contém uma ou mais coleções de regras de NAT, rede e aplicativo.
+ As regras de rede permitem ou negam tráfego com base em endereços IP, portas e protocolos.
+ As regras de aplicativo permitem ou negam tráfego com base em FQDNs (nomes de domínio totalmente qualificados), URLs e protocolos HTTP/HTTPS.
