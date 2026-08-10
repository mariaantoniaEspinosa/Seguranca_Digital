# Definições básicas sobre protocolos de testes
###  O que estudaremos? 
Conceitos sobre endereços válidos e inválidos, e como podemos nos “camuflar” para navegarmos na
internet sem sermos expostos, protegidos por um tradutor de nomes (NAT).
## Como um endereço "inválido" consegue navegar na internet?
## Protocolos de Rede
<img width="1171" height="392" alt="image" src="https://github.com/user-attachments/assets/d37ceb5b-d8fc-4f43-b631-dffd7c2b5bbf" />

- DHCP: como um host recebe um endereço
  - Antes de traduzir endereços com NAT, cada host da rede local já precisou receber um endereço IP automaticamente
  1. Discover: O host nome envia um broadcast perguntando "há algum servidor DHCP nesta rede?"
  2. Offer: O servidor DCHP responde oferecendo um endereço IP disponível, com máscara, gateway e DNS
  3. Request: O host solicita formalmente o uso do endereço oferecido (pode haver mais de um servidor)
  4. Acknowledge: O servidor confirma a concessão (lease) - o host passa a usar aquele endereço por um período

- Endereços IP válidos e inválidos
  - End. Válido (público) - roteável na internet
    - Único no mundo todo, atribuído por uma entidade regional (como o LACNIC, na América Latina) a provedores e organizações. É o endereço que um servidor precisa ter para ser alcançado por qualquer host da internet.
  - End. Inválido (privado) - não roteável na internet
    - Reservado pela RFC 1918 para uso interno em redes locais. Roteadores da internet descartam pacotes com esses endereços de origem ou destino -  por isso ele precisa ser “traduzido” antes de sair.

- Faixas de endereços reservados
<img width="847" height="243" alt="image" src="https://github.com/user-attachments/assets/8b6d5d38-710e-4ffd-9c4f-8e3137fa0692" />

- Por que a internet ignora um endereço privado? é um comportamento padrão dos roteadores
  - Os roteadores da internet possuem, por convenção (RFC 1918), regras para descartar qualquer pacote cujo endereço de origem ou destino pertença a uma faixa privada.
  - Consequência prática: um host com endereço privado não consegue, sozinho, se comunicar com a internet pública
  - O que resolve isso? Um dispositivo de borda (normalmente o roteador/gateway) precisa reescrever o endereço de origem antes do pacote sair da rede local. Esse dispositivo faz NAT.
 
- Uma rede inteira, um único IP público
  - Contrato do provedor: endereço ipv4 público por contrato residencial
  - Dezenas de dispositivos: cada dispositivo recebe um endereço privado do roteador via DHCP
  - Um só "portão de saída": todos precisam sair para internet usando o mesmo endereço público

## NAT: aprofundando o conceito
-  É a técnica pela qual um dispositivo de rede (tipicamente um roteador) reescreve, em tempo real, os endereços IP — e frequentemente também as portas — dos pacotes que atravessam ele, permitindo que hosts com endereços privados se comuniquem com a internet pública através de um único endereço válido.
-  Onde vive o Nat? na borda da rede - roteadores domésticos, firewalls corporativos e gateways de nuvem
  
### Passo a Passo
<img width="841" height="197" alt="image" src="https://github.com/user-attachments/assets/32aa6cf7-31d9-40f2-801b-1d52faa24848" />

## Static NAT
- Tradução fixa, um-para-um, entre um endereço privado e um endereço público
- Como funciona? cada endereço IP privado é mapeado permanentemente para um endereço IP público específico
- Quando usar? Servidores internos que precisam ser alcançados pelo mesmo endereço externo, como um servidor de e-mail ou site
- Custo? consome um endereço público para cada host mapeado; pouco escalável quando os endereços públicos são escassos.

## Dynamic NAT
- Tradução um-para-um, mas a partir de um pool (conjunto) de endereços públicos disponíveis
- Como funciona? o roteador mantém um conjunto de endereços públicos e atribui um deles, temporariamente, a cada host que precisa sair para internet
- Quando usar? empresas com vários endereços públicos contratados, mas menos do que o total de hosts internos ativos ao mesmo tempo
- Limite? se todos os endereços do pool estiverem em uso, o próximo host que tentar sair para internet fica bloqueado até haver um endereço livre

## PAT / NAT Overload (o mais usado)
- Port Addres Translation - muitos hosts privados compartilhando um único endereço público, diferenciado pela porta
- Como funciona? Todos os hosts saem usando o mesmo endereço IP público; o que os diferencia é a porta de origem, reescrita pelo roteador para cada conexão
- Onde está? é um modo padrão dos roteadores domésticos e da grande maioria das redes corporativas
- Vantagem? Permite dezenas de milhares de hosts internos compartilhando um único endereço ipv4 público



