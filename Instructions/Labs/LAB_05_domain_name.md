---
lab:
  title: 'Exercício 05: criar zonas DNS e definir configurações de DNS'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Exercício 05: criar zonas DNS e definir configurações de DNS

## Cenário

Sua organização requer cargas de trabalho para usar nomes de domínio em vez de endereços IP para comunicações internas.  A organização não quer adicionar uma solução DNS personalizada. Você identifica esses requisitos.
+ Uma **zona DNS privada** é necessária para contoso.com.
+ O DNS usará um **link de rede virtual** para app-vnet. 
+ Um novo **registro DNS** é necessário para a sub-rede de back-end. 

## Tarefas de habilidades

+ Criar e configurar uma zona DNS privado.
+ Criar e configurar registros DNS.
+ Definir as configurações de DNS em uma rede virtual.
  
## Diagrama de arquitetura

![Diagrama do DNS do Azure vinculado a uma rede virtual.](../Media/task-5.png)



## Instruções para o exercício

**Observação:** este exercício requer que as redes virtuais e sub-redes do Laboratório 01 sejam instaladas. Um [modelo](https://github.com/MicrosoftLearning/Configure-secure-access-to-workloads-with-Azure-virtual-networking-services/blob/main/Allfiles/Labs/All-Labs/create-vnet-subnets-template.json) será fornecido se você precisar implantar esses recursos.

### Criar uma zona DNS privada

O [DNS privado do Azure](https://learn.microsoft.com/azure/dns/private-dns-overview) fornece um serviço DNS confiável e seguro para gerenciar e resolver nomes de domínio em uma rede virtual sem a necessidade de adicionar uma solução DNS personalizada. Ao usar as zonas DNS privadas, você pode usar seus nomes de domínio personalizados em vez dos nomes fornecidos pelo Azure.

1. No portal do Azure, pesquise e selecione `Private dns zones`.

1. Selecione **+ Criar** e configure a zona DNS. 

    | Propriedade       | Valor                        |
    | :------------- | :--------------------------- |
    | Subscription   | **Selecione sua assinatura** |
    | Grupo de recursos | **RG1**                      |
    | Nome           | `private.contoso.com`              |
    | Region         | **Leste dos EUA**                  |

1. Selecione **Examinar + Criar** e, em seguida, selecione **Criar**.

1. Aguarde a implantação da zona DNS e selecione **Ir para o recurso**. 

### Cria uma conexão de rede virtual para uma zona DNS privada

Para resolver registros DNS em uma zona DNS privada, os recursos precisam estar vinculados à zona privada. Um [link de rede virtual](https://learn.microsoft.com/azure/dns/private-dns-virtual-network-links) associa a rede virtual à zona privada.

1. No portal, continue trabalhando na zona DNS **private.contoso.com**. 

1. Na folha **Gerenciamento de DNS**, escolha **+ Links de rede virtual**.

1. Selecione **+ Adicionar** e configure o link da rede virtual. 

    | Propriedade                 | Valor             |
    | :----------------------- | :---------------- |
    | Nome do link                | `app-vnet-link` |
    | Rede virtual          | **app-vnet**      |
    | Habilitar o registro automático | **Enabled**       |

1. Selecione **Criar** e aguarde até que a implantação seja concluída. Se necessário, **atualize** a página. 

### Criar um conjunto de registros DNS

[Registros de DNS](https://learn.microsoft.com/en-us/azure/dns/dns-zones-records#dns-records) fornecem informações sobre a zona DNS. 

1. No portal, continue trabalhando na zona DNS **private.contoso.com**. 

1. Na folha **Gerenciamento de DNS**, selecione **+ Conjuntos de registros**.

1. Observe que dois registros A foram criados automaticamente para cada uma das máquinas virtuais. 

1. Selecione **+ Adicionar** e configure um conjunto de registros. Quando terminar, selecione **Adicionar**. 
   
    | Propriedade   | Valor        |
    | :--------- | :----------- |
    | Nome       | `backend`    |
    | Tipo       | **A**        |
    | TTL        | **1**        |
    | Endereço IP | **10.1.1.5** |

**Observação:** esse conjunto de registros implica que há uma máquina virtual em app-vnet com um endereço IP privado de 10.1.1.5.

### Saiba mais com o treinamento online

+ [Introdução ao DNS do Azure](https://learn.microsoft.com/training/modules/intro-to-azure-dns/). Este módulo explica o que o DNS do Azure faz, como ele funciona e quando você deve optar por usar o DNS do Azure como uma solução para atender às necessidades da sua organização.
+ [Hospede seu domínio no DNS do Azure](https://learn.microsoft.com/training/modules/host-domain-azure-dns/). Neste módulo, você aprenderá a criar uma zona DNS e registros DNS.

### Principais aspectos a serem lembrados

Parabéns por concluir o exercício. Estas foram as principais conclusões:

+ O DNS do Azure é um serviço de nuvem que permite hospedar e gerenciar domínios DNS (sistema de nomes de domínio), também conhecidos como zonas DNS. 
+ As zonas públicas do DNS do Azure hospedam dados de zona de nome de domínio para registros que você pretende que sejam resolvidos por qualquer host na Internet.
+ As zonas DNS privadas do Azure permitem que você configure um namespace de zona DNS privada para recursos privados do Azure.
+ Uma zona DNS é uma coleção de registros DNS. Registros DNS fornecem informações sobre o domínio.
