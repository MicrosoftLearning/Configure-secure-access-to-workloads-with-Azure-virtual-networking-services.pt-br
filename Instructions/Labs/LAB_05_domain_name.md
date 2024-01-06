---
lab:
  title: 'Exercício: registrar e resolver nomes de domínio internamente'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---

# Laboratório: registrar e resolver nomes de domínio internamente

## Cenário

Sua organização requer cargas de trabalho para registrar e resolver nomes de domínio internamente em redes virtuais. Máquinas virtuais em redes virtuais podem usar o nome de domínio em vez de IPs para comunicação interna. Nesse caso, os nomes de domínio serão resolvidos com uma zona DNS privada por meio de um link de rede virtual. 



### Diagrama de arquitetura

![Diagrama do DNS do Azure vinculado a uma rede virtual.](../Media/task-5.png)

### Tarefas de habilidades
- Criar e configurar uma zona DNS privado. 
- Criar e configurar registros DNS.
- Definir as configurações de DNS em uma rede virtual.

## Instruções para o exercício

### Criar uma zona DNS privada

O DNS privado do Azure fornece um serviço DNS confiável e seguro para gerenciar e resolver nomes de domínio em uma rede virtual sem a necessidade de adicionar uma solução DNS personalizada. Ao usar zonas DNS privadas você poderá usar seus próprios nomes de domínio personalizados, em vez dos nomes fornecidos pelo Azure atualmente disponíveis.

1. Na barra de pesquisa do portal, digite **Zonas de DNS privada** na caixa de texto de pesquisa e selecione Zonas de DNS privada nos resultados.

1. Selecione **+ Criar**.

1. Na guia **Noções básicas** em Criar zona de DNS privada, insira as informações conforme listadas na tabela abaixo:

    | Propriedade | Valor    |
    |:---------|:---------|
    |Subscription|**Selecione sua assinatura**|
    |Resource group|**RG1**|
    |Nome|**contoso.com**|
    |Região|**Leste dos EUA**|

1. Selecione **Examinar + criar** e **Criar**.

### Cria uma conexão de rede virtual para uma zona DNS privada

1. Na barra de pesquisa do portal, digite **Zonas de DNS privada** na caixa de texto de pesquisa e selecione Zonas de DNS privada nos resultados.

1. Selecione **contoso.com**.

1. Selecione **+ Link de rede virtual**.

1. Selecione **+ Adicionar"**

1. Na aba **Noções Básicas** de Criar um link de rede virtual, digite as informações conforme listadas na tabela abaixo:

    | Propriedade | Valor    |
    |:---------|:---------|
    |Nome do link|**app-vnet-link**|
    |Rede virtual|**app-vnet**|
    |Habilitar o registro automático|**Enabled**|

1. Selecione **OK**

### Criar um conjunto de registros DNS

1. Na barra de pesquisa do portal, digite **Zonas de DNS privada** na caixa de texto de pesquisa e selecione Zonas de DNS privada nos resultados.

1. Selecione **contoso.com**.

1. Selecione **+ Conjunto de registros**.

1. Na guia **Noções básicas** de Criar conjunto de registros, insira as informações conforme listadas na tabela abaixo:

    | Propriedade | Valor    |
    |:---------|:---------|
    |Nome|**back-end**|
    |Tipo|**A**|
    |TTL|**1**|
    |Endereço IP|**10.1.1.4**|


1. Selecione **OK**

1. Verifique se **contoso.com** tem um conjunto de registros chamado **back-end**
