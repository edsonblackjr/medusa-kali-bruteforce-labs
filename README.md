
# Ataques de Força Bruta com Medusa – Laboratório Prático (Kali Linux + Metasploitable 2)

Este projeto implementa um laboratório completo para estudo de ataques de força bruta utilizando **Kali Linux**, **Medusa**, **Metasploitable 2**, **DVWA**, enumeração SMB e técnicas de password spraying.  
Inclui documentação detalhada, prints de cada etapa, mitigações recomendadas, scripts auxiliares e wordlists customizadas.

---

## 1. Arquitetura do Ambiente

Ambiente montado em **VMware Workstation** com rede **Host-Only**.

- **Kali Linux:** 192.168.88.128  
- **Metasploitable 2:** 192.168.88.129  
- **Host (Windows) com Termius:** 192.168.88.1  

Kali recebeu uma **segunda interface (eth1)** configurada em NAT para permitir acesso ao GitHub durante o desenvolvimento.

---

## 2. Estrutura do Repositório

```
medusa-kali-bruteforce-labs/
├── images/
├── scripts/
├── wordlists/
└── README.md
```

---

## 3. Controle de Versão (Git Workflow)

Inclui:

- Configuração e identificação do Git  
- Detecção de mudanças (GitHub Desktop)  
- Commit e push de arquivos e imagens de evidência  
- Versionamento da estrutura do projeto  
- Clone do repositório diretamente no Kali Linux  
- Acompanhamento de alterações usando `git status`  

---

## 4. Wordlists Customizadas

**users.txt** reúne combinações típicas encontradas em serviços vulneráveis.  
**pass.txt** contém senhas fracas comuns e variações reais.

---

## 5. Reconhecimento Inicial (Nmap)

Exemplo utilizado:

```
nmap -sV -p- 192.168.88.129
```

---

## 6. Ataques de Força Bruta

Inclui:

- FTP com Medusa  
- Formulário Web DVWA (modo Low Security)  
- SMB – password spraying e enumeração de usuários  
- Scripts auxiliares para automação  

---

## 7. Mitigações Recomendadas

- Políticas de senha fortes  
- MFA  
- Bloqueio após tentativas inválidas  
- Firewall e permissões por IP  
- Monitoramento contínuo (fail2ban/SIEM)  
- Hardening de serviços  

---

## 8. Conclusão

Laboratório completo, realista e totalmente documentado, servindo como base sólida para estudos em:

- Pentest ofensivo  
- Cibersegurança  
- Hardening  
- Auditoria de autenticação  
- Domínio de Git em ambientes Windows e Linux  

---

Autor: **Edson Caetano Jr.**  
Repositório: https://github.com/edsonblackjr/medusa-kali-bruteforce-labs
