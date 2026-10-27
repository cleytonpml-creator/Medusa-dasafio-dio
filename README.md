# Relatório de Projeto: Auditoria de Autenticação e Mecanismos de Defesa

Este repositório documenta a atividade prática e os conceitos abordados no módulo de simulação de autenticação e segurança ofensiva da Digital Innovation One (DIO).

---

## 📋 Resumo do Projeto
O objetivo deste trabalho é analisar o comportamento de serviços de rede frente a tentativas automatizadas de autenticação (como força bruta e *password spraying*), bem como estruturar e documentar as medidas defensivas necessárias para mitigar essas ameaças em ambientes corporativos.

---

## 🏗️ Arquitetura do Laboratório
* **Ambiente Atacante / Auditor:** Kali Linux
* **Ambiente Alvo / Simulado:** Metasploitable 2 (DVWA e serviços padrão)
* **Topologia de Rede:** Rede interna isolada (*Host-Only*) para garantir que nenhum tráfego alcance redes externas.

---

## 🔍 Conceitos Avaliados

### 1. Ataque de Força Bruta (*Brute Force*)
* **Definição:** Teste exaustivo de combinações de utilizadores e palavras-passe contra um serviço (ex.: FTP ou formulários web).
* **Impacto:** Esgotamento de recursos do servidor, registo massivo em logs e potencial comprometimento de contas com credenciais fracas ou padrão.

### 2. *Password Spraying*
* **Definição:** Técnica em que uma palavra-passe comum é testada contra uma vasta lista de utilizadores distintos.
* **Vantagem defensiva a observar:** Evita o bloqueio imediato de contas em sistemas que monitorizam apenas tentativas repetidas num único utilizador.

---

## 🛡️ Medidas de Defesa e Mitigação Recomendadas

1. **Políticas de Bloqueio e Atraso (Account Lockout & Tarpitting):**
   * Configurar bloqueio temporário após 3 a 5 falhas consecutivas.
   * Inserir atrasos progressivos (*delays*) entre tentativas de autenticação para inviabilizar ferramentas automatizadas.

2. **Autenticação Multi-Fator (MFA / 2FA):**
   * Implementação obrigatória em acessos administrativos e serviços externos. Mesmo que a palavra-passe seja descoberta, o acesso permanece protegido.

3. **Monitorização Ativa e Prevenção de Intrusões (Fail2ban / IPS):**
   * Analisar os ficheiros de log de autenticação (ex.: `/var/log/auth.log`, logs de servidores web e FTP).
   * Bloquear automaticamente no firewall (via `iptables`/`nftables`) os endereços de origem que apresentem comportamento suspeito.

4. **Auditoria de Credenciais e Políticas de Complexidade:**
   * Desativar contas padrão e credenciais fracas de fábrica.
   * Exigir palavras-passe robustas com critérios mínimos de entropia.
