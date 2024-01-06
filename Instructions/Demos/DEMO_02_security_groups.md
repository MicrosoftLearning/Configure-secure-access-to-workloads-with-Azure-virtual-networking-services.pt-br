---
demo:
  title: 'Demonstração: criar e configurar grupos de segurança de rede'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---
## Demonstração – Criar e configurar grupos de segurança de rede


Nesta demonstração, exploraremos os grupos de segurança. 

**Observação:** está disponível uma **[simulação de laboratório interativo para redes virtuais](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2013?azure-portal=true)** que permite que você clique em um laboratório semelhante se não puder fazer uma demonstração ao vivo. Você pode encontrar pequenas diferenças entre a simulação interativa e a demonstração sugerida, mas os principais conceitos e ideias demonstrados são os mesmos. 

[Restringir o acesso de recursos de PaaS – tutorial – portal do Azure](https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources)

### Criar um grupo de segurança de rede

1. Acessar o portal do Azure.

1. Procure e selecione os  **Grupos de segurança de rede**.

1. [Slide de apoio] Crie um NSG explicando as configurações à medida que avança. 
 
1. Aguarde a implantação do novo NSG.

**Explore as regras de entrada e saída**

1. Selecione seu novo NSG.

1. [Slide de apoio] Discuta como o NSG pode ser associado com sub-redes ou interfaces de rede.

1. Discuta a finalidade das regras de entrada e saída.  

1. Revise as regras padrão de entrada e saída. 

1. Crie uma nova regra, explicando as configurações à medida que você avança. Discuta especificamente a seleção de serviços (como HTTPS) e as configurações de prioridade. 
 

### Criar o ASG
 
1. [Slide de apoio] Pesquise e selecione os  **Grupos de segurança do aplicativo**.

1. Crie um ASG explicando as configurações à medida que avança. 
 
1. Aguarde a implantação do novo ASG.

1. Discuta como o ASG pode ser associado às regras do NSG.


### Associar os NSGs 
1.  Navegue até o NSG que você criou
1.  Selecione Sub-redes na seção Configurações.
1.  Na página Sub-redes, selecione +Associar
1.  Em Associar sub-rede, selecione sua rede virtual.


>**Observação**: os alunos devem agora ser capazes de completar LAB_02

