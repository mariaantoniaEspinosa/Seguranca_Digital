# O que vimos na aula 1?
- O que é segurança digital? Proteger informações, contas e sistemas para que ninguém não autorizado acesse, altere ou derrube o que importa
- A tríade CIA
  - Confidencialidade
  - Integridade
  - Disponibilidade
- Ativos, ameaças, vulnerabilidades, riscos e controles
- Defesa em profundidade e Zero Trust: nenhum controle é infalível sozinho; cada acesso deve ser verificado, sempre.
- Casos reais.
# Segurança de redes locais e de longa distância
- Reconhecimento: O invasor mapeia a rede, hosts ativos, serviços expostos e versões de software
- Enumeração: Detalha o que foi encontrado, portas abertas, protocolos, usuários e permissões
- Exploração: Usa uma vulnerabilidade conhecida (ou não) para obter acesso não autorizado
### Por que uma senha é tão delicada para a segurança?
- O que realmente protege uma senha?
  - comprimento > complexidade
  - Nunca reaproveitar
  - Autenticação multifator (MFA)
  - Gerenciador de senhas
- Como uma senha é comprometida?
  - Vazamento de dados
  - Reaproveitamento
  - Phising (digitar a senha em um site falso)
  - Brute force (tentativas automatizadas e massivas)
# Introdução a testes de invasão
## Da rede local à internet 
- rede Local (LAN): Poucos hosts, mesmo domínio físico ou lógico — uma sala, um prédio, um campus. Comunicação direta, geralmente via switch.
- rede de longa distância (WAN): Interconecta redes locais distantes entre si — a própria internet é a maior WAN que existe. Depende de roteamento entre redes 
- Internetworing: O conjunto de protocolos (a suíte TCP/IP) que permite que redes diferentes, de fabricantes e tecnologias diferentes, conversem entre si.
### As quatro camadas do TCP/IP
- **Aplicação (4)**: HTTP, DNS, SSH, FTP — o que o usuário e os programas efetivamente usam.
  - é a camada mais exposta a ataques
  - define como aplicações trocam dados
  - HTTP/HTTPS: navegação web - o S significa que o tráfego é criptografado
  - DNS: traduz nomes em endereços IP
  - SSH: acesso remoto administrativo criptografado
  - FTP: transferência de arquivos - sem criptografia
- **Transporte (3)**: TCP e UDP — organiza a entrega de dados entre processos.
  - organiza a entrega de dados entre processos
  - TCP: confiável e orientado a conexão
  - UDP: rápido e sem conexão, envia os dados sem garantir entrega e ordem.
- **Internet (2)**: IP — endereça e encaminha pacotes entre redes diferentes.
  - responsável por endereçar hosts e encaminhar pacotes entre redes diferentes
  - IP: endereça cada host de forma única e leva o pacote de origem até o destino, rede após rede
  - Roteamento: Cada roteador decide, com base na tabela de rotas, para qual próximo salto encaminhar o pacote.
  - ICMP: Protocolo de controle e diagnóstico, usado por ferramentas como ping e traceroute, e também por técnicas de reconhecimento de rede.
    - **ipv4 x ipv6**
      - ipv4: 32 bits · cerca de 4,3 bilhões de endereços possíveis · notação decimal (192.168.1.10) · praticamente esgotado desde 2011, sustentado hoje por NAT.
      - ipv6: 128 bits · quantidade de endereços praticamente inesgotável · notação hexadecimal (2001:db8::1) · não depende de NAT para funcionar.
- **Acesso à rede (1)**: Ethernet, Wi-Fi, MAC — coloca os bits fisicamente no meio.
  - coloca os dados fisicamente no meio de transmissão (cabo, fibra ou rádio)
  - Ethernet: padrão para redes cabeadas locais, organiza os dados em frames endereçados por MAC
  - Endereço MAC: identificador físico, único por fabricante, gravado na placa de rede, usado apenas dentro do mesmo segmento local.
  - Wi-FI: versão sem fio do mesmo princípio, com desafios extras de segurança pela natureza aberta do meio de transmissão
  - ARP: traduz endereço IP em endereço MAC dentro da rede local, alvo clássico de ataque de intercepção (ARP spoofing)





Glossário:
chave pública versus chave privada
