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

<img width="993" height="478" alt="image" src="https://github.com/user-attachments/assets/324f8a19-03d5-4e7e-bd2d-7047f912feef" />

### Respostas dos cenários
A. Porta 3306 (MySQL) exposta é quase sempre um risco — bancos de dados não deveriam estar
acessíveis diretamente da internet.

B. "Filtered" significa que existe um firewall no caminho — "closed" significa que o host respondeu,
mas não há serviço ali.

C. Só a porta 443 exposta atrás de NAT sugere port forwarding único — os demais hosts da rede
provavelmente seguem inacessíveis de fora.





