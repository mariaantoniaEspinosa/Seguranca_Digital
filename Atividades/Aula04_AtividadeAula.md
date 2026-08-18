<img width="993" height="478" alt="image" src="https://github.com/user-attachments/assets/43b98de6-6cf9-4ef9-9177-a48299582d1b" />


```
  ┌──(laboratorio㉿LAB24DT16)-[~]
  └─$ nmap -sV -T4 scanme.nmap.org
  Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-08-17 20:39 -03
  Nmap scan report for scanme.nmap.org (45.33.32.156)
  Host is up (0.33s latency).
  Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
  Not shown: 994 closed tcp ports (conn-refused)
  PORT      STATE SERVICE    VERSION
  22/tcp    open  ssh        OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
  80/tcp    open  http       Apache httpd 2.4.7 ((Ubuntu))
  2000/tcp  open  tcpwrapped
  5060/tcp  open  tcpwrapped
  9929/tcp  open  nping-echo Nping echo
  31337/tcp open  tcpwrapped
  Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
  
  Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
  Nmap done: 1 IP address (1 host up) scanned in 31.64 seconds

```
### Glossário
- Há 6 portas TCP abertas, o Nmap tentou identificar o serviço/versão em cada uma.
  
- **22/TCP**
  - Porta padrão do SSH
  - Acesso remoto ao sistema Linux por terminal
  - serviço está efetivamente aceitando conexões na porta 22
  
- **80/tcp - HTTP**
  - Porta tradicionalmente usada pelo HTTP
  - Onde fica um servidor web
  - Provavelmente existe um site/servidor HTTP respondendo a essa máquina
 
- **2000/tcp - tcpwrapped**
  - O nmap não conseguiu identificar qual serviço está por trás da porta
  - é uma indicação do comportamento observado durante a detecção
  - "A porta aceitou a conexão, mas o serviço fechou/limitou a conexão antes que o Nmap conseguisse identificá-lo."
 
- **5060/tcp - tcpwrapped**
  - A situação é semelhante à porta 2000.
  - A 5060 é conhecida principalmente por ser utilizada pelo SIP (Session Initiation Protocol), muito associado a telefonia VoIP.

- **9929/tcp - Nping Echo**
  - A porta 9929 está sendo utilizada pelo Nping Echo, um serviço relacionado ao Nping, ferramenta que faz parte do conjunto Nmap.
  - O Nping pode funcionar com um modo de "echo", permitindo que pacotes enviados ao serviço sejam refletidos/respondidos para fins de teste e diagnóstico de rede.
 
- **31337/tcp - tcpwrapped**
  - Nmap encontrou uma porta TCP que aparenta estar aberta, mas não conseguiu identificar o serviço.
  - associada a vários softwares e ferramentas diferentes, então o número da porta, sozinho, não permite dizer qual programa está rodando.
  - TCP/31337 está aberta, mas a detecção de serviço retornou tcpwrapped

- **994 closed tcp ports**
  - significa que o Nmap verificou 1.000 portas TCP e encontrou: 6 portas abertas e 994 portas fechadas
  - open: existe alguém atendendo naquela porta.
  - closed: você bateu, mas não há nenhum serviço atendendo ali.
  - conn-refused: "A conexão foi recusada."
 

  
<img width="993" height="478" alt="image" src="https://github.com/user-attachments/assets/324f8a19-03d5-4e7e-bd2d-7047f912feef" />

### Respostas dos cenários
A. Porta 3306 (MySQL) exposta é quase sempre um risco — bancos de dados não deveriam estar
acessíveis diretamente da internet.

B. "Filtered" significa que existe um firewall no caminho — "closed" significa que o host respondeu,
mas não há serviço ali.

C. Só a porta 443 exposta atrás de NAT sugere port forwarding único — os demais hosts da rede
provavelmente seguem inacessíveis de fora.





