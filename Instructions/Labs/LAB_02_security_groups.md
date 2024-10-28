---
lab:
  title: Exercício – Controlar o tráfego de rede de e para o aplicativo Web
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Laboratório: controlar o tráfego de rede de e para o aplicativo Web

## Cenário

Sua organização requer o controle do tráfego de rede direcionado e proveniente do aplicativo Web. Para aprimorar ainda mais a segurança do aplicativo Web, os NSGs (grupos de segurança de rede) e os ASGs (grupos de segurança de aplicativos) podem ser configurados. O NSG é uma camada de segurança que filtra o tráfego de rede de e para recursos do Azure, enquanto o ASG permite que o agrupamento de recursos seja gerenciado coletivamente. Esses grupos de segurança fornecem controle refinado sobre o tráfego de rede de e para componentes do aplicativo Web.

### Diagrama de arquitetura

![Diagrama que mostra um ASG e um NSG associados a uma rede virtual.](../Media/task-2.png)

### Tarefas de habilidades

- Criar um NSG.
- Criar regras de NSG.
- Associar um NSG a uma sub-rede.
- Criar e usar grupos de segurança de aplicativo em regras de NSG.

## Instruções para o exercício

### Criar grupo de segurança do aplicativo

Um ASG (grupo de segurança do aplicativo) permite agrupar servidores com funções semelhantes, como servidores Web.

1. No portal, pesquise e selecione `Application security groups`.
   
1. Clique em **+Criar** e configure o grupo de segurança do aplicativo. 

    | Propriedade       | Valor                        |
    | :------------- | :--------------------------- |
    | Subscription   | **Selecione sua assinatura** |
    | Grupo de recursos | **RG1**                      |
    | Nome           | `app-backend-asg`          |
    | Region         | **Leste dos EUA**                  |

1. Selecione **Examinar + Criar** e, em seguida, selecione **Criar**.

[Saiba mais sobre como criar um grupo de segurança do aplicativo](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic#create-application-security-groups).

>**Observação**: você está criando o grupo de segurança do aplicativo na mesma região que a rede virtual existente.

### Criar e associar o grupo de segurança de rede

Um NSG (grupo de segurança de rede) protege o tráfego de rede na sua rede virtual. Os NSGs contêm uma lista de regras de segurança que permitem ou negam o tráfego de rede a recursos conectados às VNets (redes virtuais) do Azure. Os NSGs podem ser associados com sub-redes e/ou interfaces de rede individuais conectadas às VMs (máquinas virtuais) do Azure.

1. No portal, pesquise e selecione `Network security group`.

1. Clique em **+ Criar** e configure o grupo de segurança de rede. 

    | Propriedade       | Valor                        |
    | :------------- | :--------------------------- |
    | Subscription   | **Selecione sua assinatura** |
    | Grupo de recursos | **RG1**                      |
    | Nome           | `app-vnet-nsg`            |
    | Region         | **Leste dos EUA**                  |

    [Saiba mais sobre como criar um grupo de segurança de rede](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic#create-a-network-security-group).

1. Selecione **Examinar + Criar** e, em seguida, selecione **Criar**.

**Associe o NSG ao back-end app-vnet.**

1. Clique em **Ir para o recurso** ou navegue até o recurso **app-vnet-nsg**.

1. Na folha **Configurações**, selecione **Sub-redes**.

1. Selecione **+ Associar**

1. Selecione **app-vnet (RG1)** e, em seguida, a sub-rede **Back-end**. Selecione **OK**.

    [Saiba mais sobre como associar um grupo de segurança de rede a uma sub-rede](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic#associate-a-network-security-group-to-a-subnet).

### Criar regras do grupo de segurança de rede

Um NSG (grupo de segurança de rede) protege o tráfego de rede na sua rede virtual.

1. Na caixa de pesquisa na parte superior do portal, digite **Grupo de segurança de rede**. Selecione Grupos de segurança de rede nos resultados da pesquisa.

1. Na lista de grupos de segurança de rede, selecione **app-vnet-nsg**.

1. Selecione **Regras de segurança de entrada** na seção configurações **app-vnet-nsg**.

1. Selecione **+ Adicionar**.

1. Na página **Adicionar regra de segurança de entrada**, insira as informações conforme listadas na tabela abaixo:

    | Propriedade                               | Valor                          |
    | :------------------------------------- | :----------------------------- |
    | Fonte                                 | **Qualquer**                        |
    | Intervalos de portas de origem                     | **\***                         |
    | Destino                            | **Grupo de segurança do aplicativo** |
    | Grupo de segurança do aplicativo de destino | **app-backend-asg**            |
    | Serviço                                | **SSH**                        |
    | Ação                                 | **Permitir**                      |
    | Prioridade                               | **100**                        |
    | Nome                                   | **AllowSSH**                   |

    [Saiba mais sobre como criar uma regra de grupo de segurança de rede](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic#create-a-network-security-group).

### Implantar um modelo do ARM usando o Cloud Shell para criar as VMs necessárias para este exercício

1. No portal do Azure, abra o **Azure Cloud Shell** clicando no ícone no canto superior direito do portal do Azure.

1. Se for solicitado que você selecione **Bash** ou **PowerShell**, selecione **PowerShell**.

    >**Observação**: se esta é a primeira vez que você inicia o **Cloud Shell** e recebe a mensagem **Você não tem nenhum armazenamento montado**, selecione a assinatura que está usando neste laboratório e selecione **Criar armazenamento**.

1. Implemente o seguinte modelo do ARM usando o Cloud Shell para criar as VMs necessárias para este exercício:

>**Observação**: você pode selecionar o texto na seção abaixo e copiá-lo/colá-lo no Cloud Shell.

   ```powershell
   $RGName = "RG1"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateUri https://raw.githubusercontent.com/MicrosoftLearning/Configure-secure-access-to-workloads-with-Azure-virtual-networking-services/main/Instructions/Labs/azuredeploy.json
   ```
  
1. para verificar se as máquinas virtuais **VM1** e **VM2** estão em execução, navegue até o grupo de recursos **RG1** e selecione **VM1**.

1. Verifique se o status da máquina virtual está **Em execução**.

1. Repita a etapa anterior para **VM2**.

### Como associar o grupo de segurança do aplicativo à interface de rede da VM

Quando você criou as VMs, o Azure criou a interface de rede para cada VM e anexou à VM.

Adicione o grupo de segurança do aplicativo que você criou anteriormente à interface de rede da VM2.

1. No portal do Azure, navegue até o grupo de recursos **RG1** e selecione **VM2**.

1. Navegue até a guia de rede da VM, selecione **+ Adicionar grupos de segurança do aplicativo** na seção **Grupos de segurança do aplicativo**.

1. Selecione **app-backend-asg** na lista de grupos de segurança do aplicativo.

1. Selecione **Adicionar**.

  [Saiba mais sobre como adicionar uma NIC a um grupo de segurança do aplicativo](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-network-interface?tabs=azure-portal#add-or-remove-from-application-security-groups).
