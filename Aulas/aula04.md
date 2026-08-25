# DA TRADUÇÃO DE ENDEREÇOS À VARREDURA DE PORTAS
-  assim como um endereço privado precisa ser traduzido pelo NAT para sair, um host atrás do NAT 
também precisa de uma regra explícita (port forwarding) para ser alcançado de fora — é exatamente esse limite que molda uma varredura externa.
- **Se um host está atrás do NAT, como um testador de fora descobre o que existe ali?**
  - Hosts atrás do NAT não são endereçáveis diretamente de fora.
## Onde a varredura entra no teste de invasão?
- 1: Reconhecimento -> coleta de informações públicas
- 2: Varredura -> descobrir portas abertas e serviços ativos no alvo
- 3: Enumeração -> extrair detalhes de cada serviço encontrado
- 4: Exploração -> usar as falhas identificadas para obter acesso
## O que é varredura de portas?
- Port scanning é a técnica de enviar pacotes especialmente formados para um conjunto de portas de um host, e interpretar as respostas (ou a ausência delas) para descobrir quais serviços estão escutando ali.
- **Os três estados possíveis de uma porta**
  - Scanner ---> aberta (syn-ack): serviço responde normalmente
  - Scanner ---> fechada (rst): host ativo, porta sem  serviço
  - Scanner ---> filtrada (sem resposta): firewall descarta o pacote
## Tipos de scan TCP: cada tipo interrompe o handshake em um ponto diferente 
- **SYN scan**: envia syn, recebe syn-ack, mas nunca completa com ack -> rápido e o mais usado, exige privilégio administrativo
- **Connect scan**: completa o handshake normalmente, como uma aplicação real -> mais lento e detectável, mas não exige privilégio especial
- **ACK scan**: envia apenas ACK, sem handshake prévio -> não detecta porta aberta, mapeia regras de firewall
- **FINN / NULL / XMAS**: envia flags incomuns fora do contexto -> tenta passar despercebido por firewalls simples 
## Por que a varredura UDP é mais díficil?
- **TCP com handshake**: a ausência de resposta ao SYN geralmente significa porta filtrada, o comportamento é previsível e rápido de interpretar
- **UDP sem handshake**: sem resposta pode significar porta aberta OU filtrada, a única forma de confirmar é enviar um payload específico do protocolo e esperar uma resposta de aplicação, por isso são muito mais lentos
## NAT e varredura, o que muda na prática
- Um scan de fora para dentro (black box) atinge apenas o endereço público do roteador — as portas que aparecem “abertas” são exatamente aquelas com port forwarding ou DMZ configurados. Os demais  hosts internos permanecem completamente invisíveis para essa varredura.
- **De fora (antes do NAT)**: o scan só vê o IP público e as portas explicitamente encaminhadas
- **De dentro (depois de um acesso inicial)**: uma vez com acesso a um host interno, o testador enxerga a rede local como qualquer outro dispositivo dela - sem NAT no caminho.
# Nmap: a ferramenta padrão do mercado
- Network Mapper é a ferramenta de varredura mais usada em pentests profissionais, CTFs e certificações da área.
- **O que ela faz?** Varre portas TCP/UDP, identifica versões de serviço, detecta sistema operacional e roda scripts de enumeração — tudo em uma única ferramenta.
- **Onde ela roda?** Linha de comando em Linux, Windows e MacOS
## Sintaxe 
- sS: half-open, o tipo padrão mais usado
- sT: Connect scan, completa o handshake, não exige privilégio
- sU: Varredura de portas UDP
- sV: detecta a versão do serviço rodando em cada porta aberta
- 0: tenta identificar o sistema operacional do alvo
- p: define quais portas varrer
- A: modo agressivo, combina -sV, -0 e scripts básicos
- T4: define a velocidade do scan
