---
demo:
  title: 'Demonstração: criar e configurar Redes virtuais e emparelhamento'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---
## Demonstração – Criar e configurar Redes virtuais e emparelhamento


Nesta demonstração, você criará redes virtuais.

**Observação:** você pode usar os valores sugeridos para as configurações ou seus próprios valores personalizados, se preferir.

**Observação:** uma **[simulação interativa de laboratório para redes virtuais](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%204?azure-portal=true)** e **[emparelhamento de rede virtual](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%209?azure-portal=true)** está disponível para você clicar em um laboratório semelhante, caso não consiga fazer uma demonstração ao vivo. Você pode encontrar pequenas diferenças entre a simulação interativa e a demonstração sugerida, mas os principais conceitos e ideias demonstrados são os mesmos. 


[Guia de início rápido: criar uma rede virtual – portal do Azure](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

### Criar uma rede virtual no portal


   
1.  [Slide de apoio] Antes de começar a demonstração, vamos revisar o que são redes virtuais e conceitos fundamentais das Redes Virtuais do Azure. Use este slide para destacar os recursos das Redes Virtuais do Azure. Bem como conceitos e práticas recomendadas da Rede Virtual do Azure. Ao demonstrar a criação de uma rede virtual, você pode explicar os conceitos básicos de espaço de endereço, sub-redes, regiões e assinaturas. Você também pode discutir esses slides no final e ir direto para a demonstração.
   
2.  Entre no portal do Azure e pesquise por **Redes Virtuais**.
   
3.  Crie uma rede virtual explicando as configurações básicas à medida que avança. Verifique se pelo menos uma sub-rede foi criada. 
   
4.  Explique que o portal do Azure fornece uma interface fácil de usar. Os itens marcados com um asterisco vermelho são obrigatórios.
   
5.  [Slide de apoio] Selecione a guia Segurança. Use este slide para destacar os serviços de segurança brevemente, esses tópicos serão abordados com mais detalhes mais adiante no curso. Saiba mais, Serviços que podem ser implantados em uma rede virtual. 
   
6.  [Slide de apoio] Selecione a guia Endereços IP. Use este slide para revisar: planejamento de redes virtuais e sub-redes. Adicione ou modifique uma sub-rede para demonstrar aos alunos como configurar sub-redes. 
7.  Clique em Revisar e verifique se não há erros de validação.
8.  Clique em Criar e aguarde a implantação da rede virtual. Destaque as mensagens de notificação. 
9.  Mostrar como ir para o recurso.
10. Repita o processo de criação de outra rede virtual para que você possa demonstrar o Emparelhamento VNet.

## Configurar o emparelhamento de VNet

**Observação:**  para esta demonstração você precisará de duas redes virtuais.

[Conectar redes virtuais com o Emparelhamento VNet – tutorial](https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal)

**Configurar o emparelhamento VNet na primeira rede virtual**

1. No **portal do Azure**, selecione a primeira rede virtual. Analise o valor do emparelhamento. 

1. Em  **Configurações**, selecione **Emparelhamentos** e **+ Adicionar** um novo emparelhamento.

1. Configure o emparelhamento da segunda rede virtual. Use os ícones de informação para revisar as diferentes configurações. 

1. Quando o emparelhamento estiver concluído, revise o **Status de emparelhamento**. 

**Confirmar o emparelhamento VNet na segunda rede virtual**

1. No  **portal do Azure**, selecione a segunda rede virtual

1. Em  **Configurações**, selecione  **Emparelhamentos**.

1. Observe que um emparelhamento foi criado automaticamente. Observe que o  **Status de emparelhamento**  está como  **Conectado**.


>**Observação**: os alunos devem agora ser capazes de concluir o LAB_01

