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

1. Abra um navegador, navegue até o <a href="https://portal.azure.com/#home">portal do Azure</a> e faça logon.
1. Para criar uma Rede Virtual, na barra de pesquisa da parte superior do portal, insira **"Redes virtuais"** e escolha **"Redes virtuais"** nos resultados.
1. No painel do portal **"Redes Virtuais"**, selecione ""**+ Criar**".
1. Preencha todas as guias do processo de criação usando os valores na tabela a seguir:

    | Propriedade             | Valor           |
    | :------------------- | :-------------- |
    | Resource group       | **RG1**         |
    | Nome                 | **app-vnet**    |
    | Region               | **Leste dos EUA**     |
    | Espaço de endereço IPv4   | **10.1.0.0/16** |
    | Nome da sub-rede          | **frontend**    |
    | Intervalo de endereços da sub-rede | **10.1.0.0/24** |
    | Nome da sub-rede          | **back-end**     |
    | Intervalo de endereços da sub-rede | **10.1.1.0/24** |

    **Observação**: deixe todas as outras configurações como padrão. Selecione **"Avançar"** para avançar para a próxima guia e **Criar** para criar a rede virtual.
1. Seguindo os mesmos passos acima, crie a rede virtual Azure **Hub-vnet** utilizando os valores da tabela seguinte:

    | Propriedade             | Valor                    |
    | :------------------- | :----------------------- |
    | Resource group       | **RG1**                  |
    | Nome                 | **Hub-vnet** |
    | Region               | **Leste dos EUA**              |
    | Espaço de endereço IPv4   | **10.0.0.0/16**          |
    | Nome da sub-rede          | **AzureFirewallSubnet**  |
    | Intervalo de endereços da sub-rede | **10.0.0.0/24**          |

1. Quando a implantação estiver concluída. Navegue de volta para o portal, na barra de pesquisa digite **"grupos de recursos"** e selecione **"Grupos de Recursos"** nos resultados. Selecione **"RG1"** no painel principal e confirme que ambas as redes virtuais foram implantadas.

### Configurar uma relação de par entre as redes virtuais

1. A configuração de uma relação de pares entre as duas redes virtuais permitirá que o tráfego flua em ambas as direções entre as redes virtuais **app-vnet** e **hub-vnet**.
1. No Portal, na exibição do grupo de recursos RG1. Selecione a rede virtual **"app-vnet"**.
1. No menu de contexto **app-vnet** no lado esquerdo do portal, role para baixo e selecione **emparelhamento **
1. No painel de emparelhamentos **app-vnet**, selecione **+ Adicionar**.
1. Preencha o formulário usando os valores da tabela a seguir:

    | Propriedade                                 | Valor                          |
    | :--------------------------------------- | :----------------------------- |
    | O nome do link de emparelhamento desta rede virtual   | **app-vnet-to-hub** |
    | Nome do link de emparelhamento da rede virtual remota | **hub-to-app-vnet** |
    | Rede virtual                          | **hub-vnet**       |

    **Observação**: deixe todas as outras configurações como padrão. Selecione **"Adicionar"** para criar o emparelhamento de rede virtual.

    [Saiba mais sobre o Emparelhamento de Rede Virtual](https://learn.microsoft.com/azure/virtual-network/virtual-network-manage-peering?tabs=peering-portal)

1. Quando o processo for concluído e após as atualizações de configuração. Valide se o ** Status de emparelhamento ** está definido como **Conectado**. (talvez seja necessário atualizar a página para ver o status atualizado)
