---
demo:
  title: 'Demonstração: criar e configurar o roteamento de rede'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---
## Demonstração – Criar e configurar o roteamento de rede

Nesta demonstração, aprenderemos como criar uma tabela de rotas, definir uma rota personalizada e associar a rota a uma sub-rede. 


**Observação:** esta demonstração requer uma rede virtual com pelo menos uma sub-rede.

[Rotear o tráfego de rede – tutorial – portal do Azure](https://learn.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal#create-a-route-table)


### Criar uma tabela de rotas 

1. À medida que tiver tempo, reveja o diagrama do tutorial. Explique por que você precisa criar uma rota definida pelo usuário. 

1. Acesse o portal do Azure.

1. Pesquise por **Tabela de rotas** e selecione essa opção. Discuta quando **as rotas de gateway de propagação** devem ser usadas. 

1. Crie uma tabela de roteamento e explique as configurações incomuns. 

1. Aguarde até que a nova tabela de roteamento seja implantada.

**Adicionar uma rota**

1.  Selecione sua nova tabela de roteamento e, em seguida, selecione **Rotas**.

1.  Crie uma nova **rota**. Discuta os diferentes **tipos de saltos** disponíveis. 

1.  Crie a nova rota e aguarde a implantação do recurso.
 
### Associar uma tabela de rotas a uma sub-rede
Uma tabela de rotas pode ser associada a zero ou mais sub-redes. As tabelas de rotas não estão associadas às redes virtuais. Você deve associar a tabela de rotas para cada sub-rede à qual deseja associar a tabela de rotas.


1.  Navegue até a sub-rede que você deseja associar à tabela de roteamento.

1.  Selecione  **Tabela de rotas** e escolha sua nova tabela de roteamento. 

1.  **Salve** suas alterações.

 
>**Observação**: você só pode associar uma tabela de rotas a sub-redes em redes virtuais que existam no mesmo local e assinatura do Azure que a tabela de rotas.

>**Observação**: os alunos devem agora ser capazes de completar LAB_04
