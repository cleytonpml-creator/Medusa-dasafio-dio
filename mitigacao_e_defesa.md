# Guia Técnico de Mitigação e Configuração Defensiva

## 1. Configuração do Fail2ban (Serviços Linux)
Para proteger serviços expostos (como SSH e FTP) contra ferramentas de força bruta automatizadas:

```ini
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 3600
findtime = 600
```

## 2. Hardening em Serviços Web
* **Mecanismos Anti-Automação:** Implementar CAPTCHA (ex.: reCAPTCHA ou Turnstile) em rotas sensíveis como `/login`.
* **Rate Limiting:** Limitar requisições por IP a nível de Proxy Reverso (Nginx / Cloudflare):
  ```nginx
  limit_req_zone $binary_remote_addr zone=login_limit:10m rate=5r/m;
  
  location /login {
      limit_req zone=login_limit burst=5 nodelay;
      ...
  }
  ```

## 3. Monitorização de SMB e Active Directory
* Habilitar auditoria de eventos de início de sessão (Event ID 4625 no Windows: "An account failed to log on").
* Implementar alertas no SIEM para detetar picos anómalos de Event ID 4625 distribuídos por múltiplas contas de utilizador (*Password Spraying*).
