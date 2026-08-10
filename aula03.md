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

## Rastreando uma tradução NAT, passo a passo
1. Pacote sai do host
2. Roteador aplica NAT
3. Resposta chega ao roteador
4. Roteador desfaz o NAT

## NAT de saída X NAT de entrada
- O NAT resolve muito bem quem inicia a conversa de dentro para fora: o caminho inverso exige uma regra explícita
- Saída (outbound): automático e transparente
  - Quando um host interno inicia a conexão, o roteador cria a tradução automaticamente e sabe para onde devolver a resposta. É o caso de qualquer navegação normal.
- Entrada (inbound): não existe por padrão
  - Se alguém de fora tentar iniciar uma conexão com um host interno, o roteador não sabe para qual máquina privada encaminhar o pacote — e descarta a tentativa.

## Port Forwarding e DMZ
- como expor um serviço interno de propósito, de forma controlada, ou não
- Port Fowarding: regra explícita, porta por porta
  - O administrador cria uma regra dizendo: “tudo que chegar na porta pública X deve ser encaminhado para o host interno Y, na porta Z”. É o caminho correto para expor um serviço específico, como um servidor de jogos ou uma câmera.
- DMZ (Host Exposto): todo tráfego, sem filtro
  - Encaminha para um único host interno todo o tráfego não tratado por outra regra — na prática, remove a proteção do NAT para essa máquina. Deve ser evitado fora de laboratório ou de um cenário muito bem controlado.

## Nem tudo funciona bem atrás do NAT
- P2P (torrent, blockchain)
- VoIP, videochamadas
- Jogos online

## NAT Transversal: controlando o problema ( cai em prova )
- Técnicas usadas por aplicações modernas para funcionar mesmo atrás de um ou mais NATs
- STUN: descobre o endereço público
- TURN: retransmite quando não dá
- ICE: escolhe a melhor rota

<img width="988" height="660" alt="image" src="https://github.com/user-attachments/assets/69d8345f-e73b-4907-82d3-2e40b6a97f16" />

## O que o NAT resolve e o que não resolve
- Um efeito colateral útil não é a mesma coisa que um controle de segurança desenhado para isso.
- O que ele resolve? oculta a topologia interna
  - Um invasor externo não consegue endereçar diretamente um host atrás do NAT — ele só enxerga o endereço público do roteador. Isso dificulta o reconhecimento direto de hosts internos.
- O que ele não resolve? Não filtra, não inspeciona, não autentica
  - NAT não decide o que é tráfego malicioso, não aplica regras de acesso e não substitui um firewall com inspeção de pacotes. Ele sempre deve ser complementado por controles explícitos.

## Carrier-Grade NAT (CGNAT)
- Com o esgotamento dos endereços IPv4, muitos provedores de internet não têm mais um endereço público disponível para cada cliente. A solução foi aplicar mais uma camada de NAT: o cliente recebe um IP privado do próprio provedor, que é traduzido — junto com o de centenas de outros clientes — para um único IP público na saída da operadora.
  - Impactos para o usuário: Serviços que exigem uma porta de entrada exclusiva (jogos, câmeras, servidores) ficam praticamente impossíveis de configurar sem cooperação do provedor.
  - Impacto para investigações: Vários clientes diferentes compartilham o mesmo IP público visível externamente — identificar qual usuário fez uma conexão exige o log de portas do provedor.
- O desafio da rastreabilidade: investigações digitais
- O cenário: Um mesmo endereço IP público, visto em um log de acesso, pode corresponder a centenas de clientes diferentes de um provedor que usa CGNAT — todos compartilhando aquele endereço ao mesmo tempo.
- O que fica de lição: Para identificar o responsável por uma conexão específica, é necessário cruzar o IP público, a porta de origem e o horário exato com os registros de tradução (logs de NAT) mantidos pelo provedor — sem isso, a rastreabilidade se perde

## NAT e o reconhecimento em testes de invasão
- Por que um testador raramente enxerga a topologia interna de uma rede-alvo logo de cara
- Do lado de fora: Um teste black box normalmente só enxerga o(s) endereço(s) público(s) da organização — os hosts internos ficam ocultos atrás do NAT.
- Serviços expostos de propósito: O que é alcançável de fora costuma ser justamente o que tem port forwarding ou DMZ configurado — geralmente o alvo inicial mais relevante
- Movimento lateral, não mais NAT: Uma vez com acesso a um host interno, o testador já está “atrás” do NAT e passa a enxergar a rede local como qualquer outro dispositivo dela.


# Ferramentas de reconhecimento 

## traceroute/tracert
- Releva o caminho (os saltos) que um pacote percorre até o destino; útil para identificar os pontos de NAT na rota
- O que faz? envia pacotes com TTL crescente e registra qual roteador responde em cada salto, reconstruindo o caminho até o destino
- Ligação com o NAT: Saltos que aparecem com endereços privados revelam a existência de NAT em algum ponto da rota
- Limitação: firewalls podem bloquear ICMP e mascarar saltos intermediários; a rota real pode não aparecer completa

## nslookup/dig
- Consultas diretas ao serviço de DNS; o primeiro passo para descobrir a infraestrutura pública de um alvo
- O que fazem? Consultam servidores DNS para resolver nomes em endereços IP, e vice-versa, além de revelar registros como MX (email) e NS (servidores de nome)
- Reconhecimento: Subdomínios expostos frequentemente revelam quais serviços a organização mantém publicamente acessíveis

## whois
- Consulta pública de registros de domínio e de blocos de endereços IP
- O que releva? quem registrou um domínio, datas de criação e expiração, servidores de nome, e o dono de um bloco de endereços IP públicos
- Cuidado com a privacidade: muitos registros hoje usam proteção de privacidade que oculta os dados pessoais do titular do domínio.

# Vulnerabilidade
## Falhas comuns em protocolos 
- Toda vulnerabilidade de rede é, no fundo, um desvio do que o protocolo previa - a base para os próximos testes de invasão
- Confiança implícita: ARP e DCHP assumem que qualquer resposta na rede local é legítima
- Ausência de criptografia: FTP, HTTP trafegam dados e credenciais em texto claro, vulneráveis a sniffing
- Configuração exposta: RDP, SSH expostos diretamente à internet sem NAT nem firewall, ampliam a superfície de ataque.
