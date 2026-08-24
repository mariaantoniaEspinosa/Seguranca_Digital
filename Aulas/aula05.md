### Prova dia 14/09 
## Kali Linux: linha de comando 
- **Revisão da última aula**:
- Handshake TCP e estados de porta
- Tipos de scan e Nmap
- Banner grabbing com nc
- NAT e visibilidade externa
- Por que o SYN scan precisa de sudo?
### Aula 05 
# O que é o Kali Linux?
- É uma distribuição Debian voltada especificamente para testes de invasão e auditoria de segurança
- Mantida pela Offensive Security
- Vêm com ferramentas pré-instaladas
- Voltada para o reconhecimento e exploração
    -  organizada por fases do teste de invasão: coleta, varredura, exploração e pós-exploração
- **Estrutura do sistema: FHS - Filesystem Hierarchy Standard**
  - todo Linux organiza seus arquivos a partir de uma raíz única (/)
    - /etc 
    - /home
    - /root
    - /var
    - /urs
    - /opt
- **Navegação Básica:**
  - pwd: mostra qual diretório atual
  - ls: mostrar o conteúdo do diretório
  - la - la: lista tudo, incluindo ocultos
  - cd/caminho: muda para o diratório informado
  - cd .. : sobre um  nível no diretório
  - cd ~ : vai direto para o diretório home do usuário
- **Manipulação de arquivos e diretórios:**
    - mkdir pasta: cria um novo diretório
    - touch arq.txt: cria um arquivo vazio
    - cp origin destino: copia um arquivo ou diretório
    - mv origin destino: move ou renomeia um arquivo
    - rn arquivo: remove um arquivo
    - cat arquivo: exibe todo o conteúdo do arquivo
    - less arquivo: exibe o conteúdo com rolagem
    - nano/ via arquivo: abre os editores de texto no terminal
- **Wildcards e globbing**:
  - *: qualquer sequência de caractere
  - ?: um único caractere
  - [abc]: qualquer caractere dentro do conjunto
  - {a, b}: expansão de chaves (brace expansion)
- **Encontrando arquivos: find e locate**:
  - find/ - name "*.conf: busca por nome a partir da raiza do sistema
  - find . - type f- ntime -7: arquivos modificados nos últimos 7 dias
  - find /pern - 400 2>/dev/null: arquivos com bit suid - relevante para escalonamento de privilégio
  - locate nome: busca rápida usando um índice pré-construído
- **Links simbólicos:**
  - $ ln - s /caminho/original /caminho/atalho
- **Permissões: rwx e notação octal**:
  - -rwxr-xr--
  - onde:
    - -: tipo
    - rwx: dono
    - r-x: grupo
    - r--: outros
- **chmod, chown - por que o Nmpa pede sudo**:
  - chmod: altera as permissões de um arquivo
    - chmod 754 script.sh
    - chmod +x script.sh
  - chwon: altera o dono e/ou grupo de um arquivo
    - chwon usuario:grupo arquivo
- **Usuários grupos e privilégios**:
  - usuário comum -> sudo comando -> root privilégio total
  - usuário comum -> su - -> root privilégio total
      - whoami (ahomi?)
      - id
      - groups
      - sudo adduser aluno
- **Pedindo ajuda ao prório sistema:**
  - nan nmap: abre o manual completo do comando
  - nmap -- help: mostra um resumo rápido das opções de uso
  - apropos porta: busca comandos relacionados a um termo
  - nan -k rede: equivalente a apropos
- **Histórico, atalhos e aliases**
  - history: lista os comandos já executados na sessão
  - Ctrl + R: busca reversa no histórico de comandos
  - Tab: autocompleta comandos, arquivos e caminhos
  - alias nm='nmap -sV -T4': cria um atalho para um comando longo
- **Redirecionamento e pipes**
  - conectar a saída de um comando a um arquivo - ou à entrada de outro comando
  - nmap -sV alvo (gera saída no terminal (stdout)) -> resultado.txt (redireciona para um arquivo) -> cat resultado.txt (lê o conteúdo salvo no arquivo)
    - nmap -sV alvo -> grep open (pipe: passa a saída para outro comando)
- **-oN também é redirecionamento**
  - $ nmap -oN saida.txt alvo
    - nmap -oN saida.txt: formato nativo do nmap
    - nmap alvo > saida.txt: redirecionamento genérico do shell
    - nmap alvo | grep open:  SLIDE
- **Processos**: todo programa em execução é um processo
  - ps: lista os processos em execução na sessão atual
  - ps aux: lista todos os processos do sistema, de todos os usuários
  - top / htop: monitor interativo de processos em tempo real
  - kill PID: encerra um processo pelo seu identificador
  - comando &: executa um comando em segundo plano (background)
- **Lendo arquivos grandes e logs**
  - head -n 20 arquivo: mostra as 20 primeiras linhas do arquivo
  - tail -n 20 arquivo: mostra as 20 últimas linhas do arquivo
  - tail -f /var/log/auth.log: acompanha um log em tempo real, linha a linha
  - wc -l resultado.txt: conta linhas, útil para saber quantas portas o grep encontrou
- **Espaço em disco**
  - df -h: espaço livre e usado por partição, em formato legível
  - du -sh pasta/: tamanho total de uma pasta específica
  - du -sh * | sort -h: ordena o tamanho das pastas do menor para o maior
- **Compactação e arquivamento**
  - SLIDES
- **Gerenciamento de pacotes (apt)**
  - apt update: atualiza a lista de pacotes dispooníveis nos repositórios
  - apt upgrade: atualiza os pacotes já instalados  para a versão mais recente
  - apt install gobuster: instala uma ferramenta específica 
  - apt remove pacote: remove um pacote instalado
- **Comandos de rede básicos**
  - ip a: mostra as interfaces da rede e endereços IP da máquina
  - ss -tuln: lista as portas e conexões abertas
  - ping alvo: testa conecttividade (ICMP)
  - curl /wget URL: SLIDE
  - nc host porta: SLIDE
- **nc é o mesmo banner grabbing da Aula 04**
  - $ nc 192.168.1.20 22
  - SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.4
- **Conectando a hosts remotos: SSH**
  - ssh usuario@host
  - shh -p 222 usuario@host
  - scp arquivo usuario@host:/destino
  - ssh-keygen
- **Variáveis de ambiente e PATH**: $ echo $PATH
  - variavél de ambiente: um valor nomeado disponível para os programas da sessão
  - PATH: lista de diretórios onde o shell procura executáveis 
  - export VAR=valor: define uma variável de ambiente na sessão atual

### ATIVIDADE EM DUPLA 
```                                                                             
┌──(laboratorio㉿LAB24DT16)-[~]
└─$ mkdir recon && cd recon  
                                                                                
┌──(laboratorio㉿LAB24DT16)-[~/recon]
└─$ sudo nmap -sS -T4 -oN resultado.txt scanme.nmap.org
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-08-24 20:50 -03
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.23s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
Not shown: 994 closed tcp ports (reset)
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
2000/tcp  open  cisco-sccp
5060/tcp  open  sip
9929/tcp  open  nping-echo
31337/tcp open  Elite

Nmap done: 1 IP address (1 host up) scanned in 2.02 seconds
                                                                                
┌──(laboratorio㉿LAB24DT16)-[~/recon]
└─$ cat resultado.txt                            
# Nmap 7.94SVN scan initiated Mon Aug 24 20:50:21 2026 as: nmap -sS -T4 -oN resultado.txt scanme.nmap.org
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.23s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
Not shown: 994 closed tcp ports (reset)
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
2000/tcp  open  cisco-sccp
5060/tcp  open  sip
9929/tcp  open  nping-echo
31337/tcp open  Elite

# Nmap done at Mon Aug 24 20:50:23 2026 -- 1 IP address (1 host up) scanned in 2.02 seconds
                                                                                
┌──(laboratorio㉿LAB24DT16)-[~/recon]
└─$ grep open resultado.txt                            
22/tcp    open  ssh
80/tcp    open  http
2000/tcp  open  cisco-sccp
5060/tcp  open  sip
9929/tcp  open  nping-echo
31337/tcp open  Elite
                          
```
