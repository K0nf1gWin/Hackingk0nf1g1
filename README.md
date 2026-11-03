# Hackingk0nf1g1 — RedPentest

App Android (Kotlin, sem dependências pesadas) com ferramentas de auditoria de rede,
feito para rodar no **RedMagic** (ou qualquer Android 8+). Não precisa de root.

> ⚠️ Use apenas em redes e sistemas que você possui ou tem autorização por escrito
> para testar. Acesso não autorizado é crime (Lei 12.737/2012).

## Ferramentas

| Aba | O que faz |
|---|---|
| **EASM** | Superfície de ataque externa de um domínio: subdomínios (CT + wordlist), DNS, subdomain takeover, IP/ASN, portas de risco expostas, serviços web, TLS, arquivos sensíveis (.git, .env, actuator, phpinfo...), SPF/DMARC/CAA/DNSSEC |
| Minha rede | Interface, IP local, gateway, DNS, sub-rede e IP público |
| Hosts na LAN | Descobre dispositivos ativos na rede (sondagem TCP/RST + echo) com hostname |
| Port scan | TCP connect scan (top, all, faixas, listas) com captura de banner |
| Ping | Ping do sistema |
| DNS | A, AAAA, MX, NS, TXT, CNAME, SOA, CAA via DNS-over-HTTPS; reverso de IP |
| Subdomínios | Recon passivo via Certificate Transparency (crt.sh) |
| HTTP/TLS | Cabeçalhos de segurança, cookies, vazamento de versão, certificado TLS |
| Encoder/Hash | Base64, URL, hex, MD5/SHA-1/SHA-256/SHA-512 (offline) |

## Relatórios

Toda ferramenta executada fica registrada na sessão, e os achados recebem uma severidade
(Crítica, Alta, Média, Baixa ou Info), com evidência e recomendação.
O botão **📄 Relatório** gera e salva em `Download/RedPentest/`:

- **HTML**: sumário executivo, tabela de achados, detalhamento, metodologia e anexo com as saídas
- **Markdown**: para colar em wiki, Jira ou Confluence
- **CSV**: para planilha, SIEM ou abertura de tickets

Depois é só compartilhar por e-mail, Drive, Teams etc. **Zerar sessão** limpa tudo para um novo trabalho.

## Como instalar no RedMagic

1. Abra a aba **Releases** do repositório no celular e baixe o `RedPentest.apk` mais recente
   (cada push gera um build automático pelo GitHub Actions).
2. Abra o arquivo e permita "instalar apps de fontes desconhecidas" para o navegador/arquivos.
3. Abra o **RedPentest** e aceite o termo de uso autorizado.

## Compilar localmente

```bash
./gradlew assembleRelease
# APK em app/build/outputs/apk/release/app-release.apk
```

Requer JDK 17 e Android SDK (compileSdk 35).
