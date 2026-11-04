# Hackingk0nf1g1 — RedPentest

App Android (Kotlin, sem dependências pesadas) com ferramentas de auditoria de rede,
feito para rodar no **RedMagic** (ou qualquer Android 8+). Não precisa de root.
Visual neon (cada categoria tem sua cor: ciano, magenta, verde, âmbar, violeta).

> ⚠️ Use apenas em redes e sistemas que você possui ou tem autorização por escrito
> para testar. Acesso não autorizado é crime (Lei 12.737/2012). Ferramentas de scan
> ativo (Nmap, Dir fuzz, Shodan, Tecnologias com teste de WAF) fazem requisições reais
> ao alvo — confirme o escopo autorizado antes de rodar.

## Ferramentas

### Recon
| Aba | O que faz |
|---|---|
| **EASM** | Superfície de ataque externa de um domínio: subdomínios (7 fontes passivas), DNS, subdomain takeover, IP/ASN, portas de risco expostas, serviços web, TLS, arquivos sensíveis (.git, .env, actuator, phpinfo...), SPF/DMARC/CAA/DNSSEC |
| **Harvester** | OSINT estilo theHarvester: subdomínios (crt.sh, HackerTarget, Anubis, AlienVault OTX, urlscan.io, RapidDNS, Wayback), e-mails do site, IPs/provedores, ambientes de homologação expostos |
| **Whois** | Registro de domínio/IP via RDAP: registrador, datas, status, nameservers, bloco de rede |
| **Shodan** | Consulta o Shodan InternetDB (grátis, sem tocar no alvo): portas, tecnologias, CVEs já indexados |
| **CVE** | Busca no NVD por produto/versão ou CVE-ID, com flag CISA KEV (exploração ativa) e score EPSS |
| **Subdomínios** | Descoberta ativa + passiva: 7 fontes passivas + wordlist, resolução DNS e detecção de subdomain takeover (CNAME pendente) |
| DNS | A, AAAA, MX, NS, TXT, CNAME, SOA, CAA via DNS-over-HTTPS; reverso de IP |

### Scan
| Aba | O que faz |
|---|---|
| **Nmap** | Scanner nativo estilo Nmap: descoberta, portas TCP (-p) e UDP (-sU), versão de serviço (-sV), SO por TTL (-O), scripts (-sC: ftp-anon, redis-info, ssl-cert, http-title/methods...), alerta de versões vulneráveis e consulta automática de CVEs no NVD por serviço detectado |
| Hosts na LAN | Descobre dispositivos ativos na rede (sondagem TCP/RST + echo) com hostname |
| Port scan | TCP connect scan (top, all, faixas, listas) com captura de banner |
| **UPnP/SSDP** | Descobre dispositivos UPnP na LAN (roteadores, TVs, câmeras, IoT) com fabricante/modelo |

### Rede
| Aba | O que faz |
|---|---|
| Minha rede | Interface, IP local, gateway, DNS, sub-rede e IP público |
| **Sistema** | whoami/uname: usuário Linux do app, modelo, versão Android/kernel, interfaces de rede, indícios de root |
| Ping | Ping do sistema |
| **Traceroute** | Mapeia os saltos até o alvo via ping com TTL crescente |
| **Wi-Fi** | Redes próximas: canal, sinal, criptografia; alerta rede aberta, WEP, WPA1, WPS |

### Web
| Aba | O que faz |
|---|---|
| **Web Pentest** | Rastreia o site e testa XSS refletido, SQLi, open redirect, CRLF/header injection, CORS mal configurado, Host header injection, segredos vazados em HTML/JS, CSRF ausente e GraphQL introspection. Só GET e payloads inofensivos — não envia formulário nem tenta login |
| **Repeater** | Monta uma requisição HTTP à mão (método, URL, cabeçalhos, corpo) e mostra a resposta crua — edite e reenvie, como o Repeater do Burp/ZAP |
| **Intruder** | Fuzzer de parâmetro estilo Burp Intruder (modo Sniper): marque o ponto com `§FUZZ§`, escolha um conjunto de payloads (SQLi/XSS/LFI/command injection/números/customizado) e compare status, tamanho e tempo de cada resposta |
| HTTP/TLS | Cabeçalhos de segurança, cookies, vazamento de versão, certificado TLS |
| **TLS scan** | Protocolos suportados (SSLv3–TLS1.3), cifras fracas, detalhes do certificado |
| **Tecnologias** | Fingerprint de CMS/framework/JS (com versões vulneráveis), CDN e WAF |
| **Dir fuzz** | Varredura de caminhos/arquivos sensíveis (admin, .git, .env, backups, actuator...) com wordlist embutida |

### Análise
| Aba | O que faz |
|---|---|
| Encoder/Hash | Base64, URL, hex, MD5/SHA-1/SHA-256/SHA-512 (offline) |
| **JWT** | Decodifica e audita tokens JWT: alg=none, expiração, claims sensíveis |

## Relatórios

Toda ferramenta executada fica registrada na sessão, e os achados recebem uma severidade
(Crítica, Alta, Média, Baixa ou Info), com evidência e recomendação.

O botão **👁 Relatório** abre o relatório HTML **direto na tela do app** (sumário executivo,
tabela de achados, detalhamento, metodologia e anexo com as saídas) — não precisa baixar nada
para ver. De dentro dele, o botão **Salvar** grava em `Download/RedPentest/` nos formatos:

- **HTML** (o mesmo que você acabou de ver)
- **Markdown**: para colar em wiki, Jira ou Confluence
- **CSV**: para planilha, SIEM ou abertura de tickets

Depois é só compartilhar por e-mail, Drive, Teams etc. **Zerar** limpa tudo para um novo trabalho.

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
