---
demo:
  title: 'Demonstração: criar e configurar o Firewall do Azure'
  module: Guided Project - Configure secure access to workloads with Azure virtual networking services
---
## Demonstração – Criar e configurar o Firewall do Azure

**Observação:** o Firewall do Azure pode levar alguns minutos para ser implantado.

Nesta demonstração, explore o Firewall do Azure.
Revise e crie uma política de firewall e Firewall do Azure.
1.  [Slide de apoio] Antes de começar a demonstração, vamos revisar o que é o Firewall do Azure.
2.  Acesse o portal do Azure.
3.  Criar um Firewall do Azure.
4.  ⓘ na guia Noções básicas, explique as opções de configuração disponíveis à medida que você as preenche. 
5.  Aceite os outros valores padrão e selecione Examinar + criar.
6.  Após a conclusão da implantação, vá para o recurso de firewall e revise a página de visão geral. 


### Configurar uma regra de aplicativo 

1. [Slide de apoio] Regras da política do Firewall do Azure

Essa é a regra de aplicativo que permite o acesso de saída para www.google.com.
1.  Navegue até a política de firewall que você criou.
2.  Selecione Regras de aplicativo.
3.  Selecione Adicionar uma coleção de regras.
4.  Em Nome, insira App-Coll01.
5.  Em Prioridade, insira 200.
6.  Em Ação de coleção de regras, selecione Permitir.
7.  Em Regras, para Nome, insira Allow-Google.
8.  Em Tipo de origem, selecione Endereço IP.
9.  Em Origem, insira 10.0.2.0/24.
10. Em Protocol:port, insirahttp, https.
11. Em Tipo de Destino, selecione FQDN.
12. Em Destino, insira www.google.com
13. Selecione Adicionar.

O Firewall do Azure inclui uma coleção de regras internas para FQDNs de infraestrutura que têm permissão por padrão. Esses FQDNs são específicos da plataforma e não podem ser usados para outras finalidades. Para saber mais, veja FQDNs de infraestrutura.

### Configurar uma regra de rede
Essa é a regra de rede que permite o acesso de saída para dois endereços IP na porta 53 (DNS).
1.  Escolha Regras de rede.
2.  Selecione Adicionar uma coleção de regras.
3.  Em Nome, insira Net-Coll01.
4.  Em Prioridade, insira 200.
5.  Em Ação de coleção de regras, selecione Permitir.
6.  Em Grupo de coleta de regra, selecione DefaultNetworkRuleCollectionGroup.
7.  Em Regras, para Nome, insira Allow-DNS.
8.  Em Tipo de origem, selecione Endereço IP.
9.  Em Origem, insira 10.0.2.0/24.
10. Em Protocolo, selecione UDP.
11. Em Portas de Destino, insira 53.
12. Em Tipo de destino, selecione Endereço IP.
13. Em Destino, insira 209.244.0.3,209.244.0.4.
Esses são servidores DNS públicos operados pelo CenturyLink.
14. Selecione Adicionar.

