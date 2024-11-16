---
lab:
  title: 'Exercício 02: criar e configurar grupos de segurança de rede'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Exercício 02: criar e configurar grupos de segurança de rede

## Cenário

Sua organização requer o controle do tráfego de rede no the app-vnet. Você identifica esses requisitos.
+ A sub-rede front-end tem servidores Web que podem ser acessados pela Internet. Um ASG (**grupo de segurança do aplicativo**) é necessário para esses servidores. O ASG deve ser associado a qualquer interface de máquina virtual que faça parte do grupo. Isso permitirá que os servidores da web sejam facilmente gerenciados. 
+ Uma **regra do NSG** é necessária para permitir o tráfego HTTPS de entrada para o ASG. Essa regra usa o protocolo TCP na porta 443. 
+ A sub-rede de back-end tem servidores de banco de dados usados pelos servidores Web de front-end. Um NSG (**grupo de segurança de rede**) é necessário para controlar esse tráfego. O NSG deve ser associado a qualquer interface de máquina virtual que será acessada pelos servidores Web. 
+ Uma **regra do NSG** é necessária para permitir o tráfego de rede de entrada do ASG para os servidores de back-end.  Essa regra usa o serviço MS SQL e a porta 1443. 
+ Para teste, uma máquina virtual deve ser instalada na sub-rede de front-end (VM1) e na sub-rede de back-end (VM2).  O grupo de TI forneceu um modelo do Azure Resource Manager para implantar esses **servidores Ubuntu**. 

## Tarefas de habilidades

+ Crie um grupo de segurança de rede.
+ Criar regras do grupo de segurança de rede.
+ Associar um grupo de segurança de rede a uma sub-rede.
+ Criar e usar grupos de segurança de aplicativo em regras de grupos segurança de rede.

## Diagrama de arquitetura

![Diagrama que mostra um ASG e um NSG associados a uma rede virtual.](../Media/task-2.png)




## Instruções para o exercício

### Criar a infraestrutura de rede para o exercício

**Observação:** este exercício requer que as redes virtuais e sub-redes do Laboratório 01 sejam instaladas. Um [modelo](https://github.com/MicrosoftLearning/Configure-secure-access-to-workloads-with-Azure-virtual-networking-services/blob/main/Allfiles/Labs/All-Labs/create-vnet-subnets-template.json) será fornecido se você precisar implantar esses recursos.

1. Use o ícone (canto superior direito) para iniciar uma sessão do **Cloud Shell**. Como alternativa, navegue diretamente para `https://shell.azure.com`.

1. Se for solicitado que você selecione **Bash** ou **PowerShell**, selecione **PowerShell**.

1. O armazenamento não é necessário para esta tarefa Selecionar assinatura. 

1. Implante as máquinas virtuais necessárias para este exercício. 

   ```powershell
   $RGName = "RG1"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateUri https://raw.githubusercontent.com/MicrosoftLearning/Configure-secure-access-to-workloads-with-Azure-virtual-networking-services/main/Instructions/Labs/azuredeploy.json
   ```
  
1. No portal, pesquise e selecione `virtual machines`. Verifique se vm1 e vm2 estão **em execução**.

### Criar grupo de segurança do aplicativo

Os [ASGs (grupos de segurança do aplicativo)](https://learn.microsoft.com/azure/virtual-network/application-security-groups) permitem agrupar servidores com funções semelhantes. Por exemplo, todos os servidores da Web que hospedam seu aplicativo. 

1. No portal, pesquise e selecione `Application security groups`.
   
1. Clique em **+Criar** e configure o grupo de segurança do aplicativo. 

    | Propriedade       | Valor                        |
    | :------------- | :--------------------------- |
    | Subscription   | **Selecione sua assinatura** |
    | Grupo de recursos | **RG1**                      |
    | Nome           | `app-backend-asg`          |
    | Region         | **Leste dos EUA**                  |

1. Selecione **Examinar + Criar** e, em seguida, selecione **Criar**.

**Observação**: você está criando o grupo de segurança do aplicativo na mesma região que a rede virtual existente.

**Associar o grupo de segurança do aplicativo à interface de rede da VM**

1. No portal do Azure, pesquise e selecione `VM2`.

1. Clique na folha **Rede**, selecione **Grupos de segurança do aplicativo** e clique em **Adicionar grupos de segurança do aplicativo**.

1. Selecione o **app-backend-asg** e, em seguida, **Adicionar**.
   
### Criar e associar o grupo de segurança de rede

[NSGs (grupos de segurança de rede)](https://learn.microsoft.com/azure/virtual-network/network-security-groups-overview) protegem o tráfego de rede em uma rede virtual. 

1. No portal, pesquise e selecione `Network security group`.

1. Clique em **+ Criar** e configure o grupo de segurança de rede. 

    | Propriedade       | Valor                        |
    | :------------- | :--------------------------- |
    | Subscription   | **Selecione sua assinatura** |
    | Grupo de recursos | **RG1**                      |
    | Nome           | `app-vnet-nsg`            |
    | Region         | **Leste dos EUA**                  |

1. Selecione **Examinar + Criar** e, em seguida, selecione **Criar**.

**Associe o NSG à sub-rede de back-end app-vnet.**

Os NSGs podem ser associados a sub-redes e/ou interfaces de rede individuais conectadas às máquinas virtuais do Azure. 

1. Clique em **Ir para o recurso** ou navegue até o recurso **app-vnet-nsg**.

1. Na folha **Configurações**, selecione **Sub-redes**.

1. Selecione **+ Associar**

1. Selecione **app-vnet (RG1)** e, em seguida, a sub-rede **Back-end**. Selecione **OK**.

### Criar regras do grupo de segurança de rede

Um NSG usa [regras de segurança](https://learn.microsoft.com/azure/virtual-network/network-security-group-how-it-works) para filtrar o tráfego de rede de entrada e saída. 

1. Na caixa de pesquisa na parte superior do portal, digite **grupos de segurança de rede**. Selecione Grupos de segurança de rede nos resultados da pesquisa.

1. Na lista de grupos de segurança de rede, selecione **app-vnet-nsg**.

1. Na folha **Configurações**, selecione **Regras de segurança de entrada**.

1. Selecione **+ Adicionar** e configure uma regra de segurança de entrada. 

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


### Saiba mais com o treinamento online

+ [Filtrar o tráfego de rede com um grupo de segurança de rede usando o portal do Azure](https://learn.microsoft.com/training/modules/filter-network-traffic-network-security-group-using-azure-portal/). Neste módulo, você focará em filtrar o tráfego de rede usando NSGs (Grupos de Segurança de Rede) no portal do Azure. Saiba como criar, configurar e aplicar NSGs para melhorar a segurança de rede.
+ [Proteger e isolar o acesso aos recursos do Azure usando grupos de segurança de rede e pontos de extremidade de serviço](https://learn.microsoft.com/training/modules/secure-and-isolate-with-nsg-and-service-endpoints/). Neste módulo, você aprenderá sobre grupos de segurança de rede e como restringir a conectividade de rede. 

### Principais aspectos a serem lembrados

Parabéns por concluir o exercício. Estas foram as principais conclusões:

+ Os grupos de segurança do aplicativo permitem organizar máquinas virtuais e definir políticas de segurança de rede com base nos aplicativos da sua organização.
+ Use um grupo de segurança de rede do Azure para filtrar o tráfego de rede entre os recursos do Azure em uma rede virtual do Azure.
+ Você pode associar um, ou nenhum, grupo de segurança de rede a cada sub-rede e adaptador de rede de uma rede virtual em uma máquina virtual. 
+ Um grupo de segurança de rede contém regras de segurança que permitem ou negam o tráfego de rede de entrada ou de saída em relação de recursos do Azure.
+ Você une as máquinas virtuais a um grupo de segurança de aplicativo. Depois, usa o grupo de segurança de aplicativo como uma origem ou um destino nas regras do grupo de segurança de rede.



