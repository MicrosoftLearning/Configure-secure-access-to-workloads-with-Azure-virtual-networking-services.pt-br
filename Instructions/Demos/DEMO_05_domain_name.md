---
demo:
  title: 'Demonstração: criar e configurar o DNS do Azure'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---
## Demonstração – Criar e configurar o DNS do Azure

Nesta demonstração, exploraremos o DNS do Azure.

[Tutorial: hospedar seu domínio e subdomínio – DNS do Azure](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns)


**Criar uma zona DNS privada**

1. Acessar o portal do Azure.

1. Procure o serviço de  **zonas DNS** .

1. Crie uma **zona DNS** e explique a finalidade da zona. Para um nome, você pode usar contoso.internal.com.

1.  Aguarde a zona DNS ser criada. Talvez seja necessário **Atualizar** a página.

**Adicionar conjunto de registros DNS**


[Tutorial: criar um registro de alias para se referir a um registro de recurso de zona](https://learn.microsoft.com/azure/dns/tutorial-alias-rr)

1. Depois que sua zona for criada, selecione **+Conjunto de registros**.

1. Use a lista suspensa **Tipo** para exibir os diferentes tipos de registros. Revise como os diferentes tipos de registro são usados. Observe como as informações do registro mudam à medida que você seleciona diferentes tipos de registro.

1. Crie um registro **A** como exemplo. 

**Link VNet para registro automático**

1.  Depois que a Zona DNS for implantada, revise a página de visão geral com os alunos.
1.  vincular a zona DNS privada a uma rede virtual criando um link de rede virtual.
1.  No painel esquerdo, selecione Links de rede virtual.
1.  Selecione Adicionar.
1.  Digite myLink como o Nome do link.
1.  Para Rede virtual, selecione myAzureVNet.
1.  Marque a caixa de seleçãoHabilitar o registro automático.
1.  Selecione OK.

>**Observação**: agora, os alunos devem ser capazes de completar LAB_05