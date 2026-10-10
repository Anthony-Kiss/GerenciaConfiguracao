# Atividade 1 - Documentação e Configuração: Acesso Remoto (SSH) e Firewall (UFW)

Disciplina: Gerência e Configuração de Serviços para Internet (GCSI).
Aluno: Anthony Ryan Benigno Kiss.
Ambiente: Docker com imagem ubuntu 26.04 LTS.

Passo 1. Criar e Iniciar o Container Servidor (ubuntu2604_no4. o nome e optativo)
Criei e iniciei o container em modo interativo com a flag --privileged e a porta 2222 mapeada:

comando usado:
docker run --name ubuntu2604_no4 --privileged -it -p 2222:22 ubuntu:26.04 bash

Passo 2. Atualizar o Sistema e Instalar Pacotes
Atualizei os repositórios e instalei o OpenSSH Server, o UFW, o Sudo e o Nano.
configuração do fuso horário:
-Modo Interativo (Escolha manual): Durante a instalação, selecionei a região America e a localidade Recife.

comando usado:
  apt update && apt upgrade -y
  apt install -y openssh-server ufw sudo nano

-Modo Não Interativo (Automatizado): Caso opte por evitar a tela interativa, utilizei:

Vale lembra utilizando essa não vai aparecer o fuso horário.

comando usado:
  DEBIAN_FRONTEND=noninteractive apt update && apt upgrade -y
  DEBIAN_FRONTEND=noninteractive apt install -y openssh-server ufw sudo nano

Passo 3. Configurar e Iniciar o Servidor SSH
1.Defini a senha do root:
comando usado:
   passwd

2.Criei o usuário comum para acesso:
comando usado:

   useradd -m -s /bin/bash aluno
   passwd aluno

3.Iniciei o serviço SSH:
comando usado:
   service ssh start

4.Verifiquei o status:
comando usado:
   service ssh status
5.

Passo 4. Configurar e Ativar o Firewall (UFW)
1.Configurei as políticas padrão:
comando usado:
   ufw default deny incoming
   ufw default allow outgoing

2.Liberei a porta do SSH:
comando usado:
   ufw allow 22/tcp

3.Ativei o UFW:
comando usado:
   ufw enable

4.Verifiquei o status:
comando usado:
   ufw status verbose

Passo 5. Teste de Conexão com o Container Cliente (ubuntu2604_no5)

Nessa parte criei outro container para fazer o teste.
docker run --name ubuntu2604_no5 -it ubuntu:26.04 bash

1.Descobri o IP interno do container servidor no host:
comando usado:
   docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' ubuntu2604_no4

2.Criei e iniciei o container cliente em outro terminal:
comando usado:
   docker run --name ubuntu2604_no5 -it ubuntu:26.04 bash

3.Instalei o cliente SSH no container de teste:
comando usado:
   apt update && apt install -y openssh-client

4.Realizei a conexão SSH do cliente para o servidor:
comando usado:
   ssh aluno@172.17.0.2
   (Confirmei a chave digitando yes e inseri a senha do usuário aluno para estabelecer a conexão com sucesso).


Vale lembrar: para realizar o teste de conexão utilizando o root, é preciso permitir o root login lá no nano. Para isso, acessei o arquivo de configuração do SSH no servidor:
comando:
   nano /etc/ssh/sshd_config.

vcs vão procura porPermitRootLogin
e vão altera para PermitRootLogin yes

depois vão salvar 
com o comando Ctrl + O e vai dar enter

em seguida vão usar o ctrl + x

depois disso só testar denovo!!!!