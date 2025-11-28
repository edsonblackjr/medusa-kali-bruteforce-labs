# Laboratório de Ataques de Força Bruta com Kali Linux, Medusa e Hydra

## 1. Introdução
Este projeto tem como objetivo demonstrar, documentar e analisar ataques de força bruta em um ambiente controlado utilizando Kali Linux, Metasploitable 2, DVWA (Damn Vulnerable Web Application), Medusa e Hydra. Toda a infraestrutura foi construída para fins acadêmicos na pós-graduação em Cybersegurança (DIO + Santander Tech+), permitindo compreender metodologias ofensivas e, principalmente, propor medidas de mitigação.

---

## 2. Arquitetura do Ambiente
O laboratório consiste em três elementos principais:

- **Host (Windows 10 + Termius):** 192.168.88.1  
- **Kali Linux (Atacante):** 192.168.88.128  
  - Interface 1: Host‑Only (rede do laboratório)  
  - Interface 2: NAT (permitindo acesso ao GitHub)  
- **Metasploitable 2 (Alvo):** 192.168.88.129  
  - Serviços vulneráveis intencionalmente

---

## 3. Preparação do GitHub para documentação

### 3.1 Configurando o GitHub Desktop
O repositório “medusa-kali-bruteforce-labs” foi criado para armazenar:
- Evidências
- Wordlists
- Scripts
- Logs de ataque
- README documentado

Evidência: `01-git-config.png`

### 3.2 Estrutura de pastas inicial
```
/evidences
/wordlists
README.md
```
Evidência: `02-folder-structure.png`

### 3.3 Git detecta alterações
Evidência: `03-git-detected-changes.png`

### 3.4 Commit inicial e push
Evidência: `04-git-commit-push.png`

### 3.5 Visualização do repositório no GitHub
Evidência: `05-github-images.png`

---

## 4. Clone do repositório no Kali
Para facilitar o envio das evidências diretamente do ambiente atacante:

```
git clone https://github.com/edsonblackjr/medusa-kali-bruteforce-labs.git
```

Evidência: `06-git-clone-kali.png`

Posteriormente, o status do repositório foi verificado:
Evidência: `07-git-status-kali.png`

---

## 5. Medida de Segurança: Alteração da Senha no Kali
Como boa prática antes da execução de ferramentas ofensivas, a senha padrão do Kali foi alterada usando:

```
passwd
```

Evidência: `08-kali-password-change.png`

---

## 6. Reconhecimento — Varredura Nmap Completa
O escaneamento inicial mapeou serviços vulneráveis na Metasploitable 2:

Comando:
```
nmap -sV -p- 192.168.88.129 -oN nmap_full.txt
```

Principais serviços identificados:
- FTP (vsftpd, ProFTPD)
- SSH (OpenSSH antigo)
- Telnet
- HTTP (Apache)
- Samba
- MySQL
- VNC
- IRC
- NFS
- Backdoors intencionais

Evidência: `09-nmap-fullscan.png`  
Arquivo salvo: `/evidences/nmap/nmap_full.txt`

---

## 7. Ataque de Força Bruta — FTP com Medusa

### 7.1 Teste com usuário específico
Comando:
```
medusa -h 192.168.88.129 -u msfadmin -P wordlists/pass.txt -M ftp
```

Evidência: `10-medusa-ftp-fixeduser.png`

### 7.2 Teste completo com lista de usuários
```
medusa -h 192.168.88.129 -U wordlists/users.txt -P wordlists/pass.txt -M ftp
```

Evidência: `11-medusa-ftp-userlist.png`

### 7.3 Sucesso da invasão
A ferramenta identificou:

**Usuário:** msfadmin  
**Senha:** msfadmin  

Evidência: `12-medusa-ftp-success.png`

### 7.4 Login validado
Evidência: `13-ftp-login-success.png`

---

## 8. Tentativa de Ataque SSH com Hydra (documentação técnica)

O Metasploitable utiliza **protocolos SSH antigos** (ssh-rsa, ssh-dss), enquanto versões modernas do OpenSSH (como a do Kali) não os aceitam por padrão.

Resultado:  
- Ataque não pôde ser executado
- A incompatibilidade foi registrada como aprendizado importante

Evidência: *Nenhuma credencial obtida*

---

## 9. Ataque FTP com Hydra (comparativo entre ferramentas)

Comando:
```
hydra -L wordlists/users.txt -P wordlists/pass.txt ftp://192.168.88.129
```

Evidência: `14-hydra-ftp-bruteforce.png`

A Hydra validou o comportamento encontrado com Medusa.

---

## 10. Ataque a Formulário Web — DVWA (Hydra HTTP POST)

Este foi um dos ataques mais ricos em aprendizado.

Comando utilizado:

```
hydra 192.168.88.129 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed" -L wordlists/users.txt -P wordlists/pass.txt -t 6
```

### Resultado:
Hydra encontrou a credencial válida:

- **Usuário:** admin  
- **Senha:** password  

Evidência: `15-hydra-dvwa-form-bruteforce.png`

### Login validado:
Evidência: `16-dvwa-login-success.png`

---

## 11. Recomendações de Mitigação

### 11.1 FTP
- Desabilitar FTP e substituir por SFTP
- Implementar fail2ban
- Bloquear portas no firewall
- Exigir autenticação forte

### 11.2 SSH
- Habilitar somente chaves modernas (ed25519)
- Desabilitar SSH1, ssh-rsa, ssh-dss
- Usar portas alternativas
- Limitar IPs permitidos

### 11.3 Formulários Web
- Implementar Captcha
- Limite de tentativas
- MFA
- Hash forte (bcrypt, scrypt ou Argon2)
- Tokens antifraude (CSRF, JWT)
- WAF + IDS

---

## 12. Conclusão Técnica
Este laboratório mostrou, na prática, como ataques de força bruta podem ser realizados de várias formas:

- Serviços expostos (FTP, SSH)  
- Formulários Web vulneráveis  
- Senhas fracas  
- Protocolos antigos  

Foram exploradas vulnerabilidades reais em ambiente controlado, utilizando ferramentas amplamente usadas no mercado ofensivo e defensivo.

Com isso, fica evidente a importância de:
- Políticas de senha  
- Hardening dos serviços  
- Monitoramento ativo  
- Resposta rápida a incidentes  

Este material serve como portfólio profissional e pode ser apresentado tanto em ambientes acadêmicos quanto corporativos.

---

## 13. Estrutura Final do Repositório

```
/evidences
    01-git-config.png
    02-folder-structure.png
    03-git-detected-changes.png
    04-git-commit-push.png
    05-github-images.png
    06-git-clone-kali.png
    07-git-status-kali.png
    08-kali-password-change.png
    09-nmap-fullscan.png
    10-medusa-ftp-fixeduser.png
    11-medusa-ftp-userlist.png
    12-medusa-ftp-success.png
    13-ftp-login-success.png
    14-hydra-ftp-bruteforce.png
    15-hydra-dvwa-form-bruteforce.png
    16-dvwa-login-success.png

/wordlists
    users.txt
    pass.txt

README.md
```

---

## 14. Assinatura
Projeto desenvolvido por:

Projeto desenvolvido por **Edson Caetano Jr**., como parte da pós-graduação em Cybersegurança — DIO + Santander Tech+.  

