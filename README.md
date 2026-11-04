# Hackingk0nf1g1 — Blackout Hat

App Android (Kotlin, sem dependências pesadas) com ferramentas de auditoria de rede,
feito para rodar no **RedMagic** (ou qualquer Android 8+). Não precisa de root.
Visual neon com chuva de código vermelha de fundo, ferramentas em cartões clicáveis com efeito
de toque (ripple neon ao tocar), cada categoria com sua cor: OSINT ciano, Black Hat magenta,
Rede verde, Web âmbar, Análise violeta. Tem um campo de **ALVO** global no
topo: defina domínio/IP/URL uma vez e ele preenche o campo correspondente em qualquer ferramenta
que você abrir.

> ⚠️ Use apenas em redes e sistemas que você possui ou tem autorização por escrito
> para testar. Acesso não autorizado é crime (Lei 12.737/2012). Ferramentas de scan
> ativo (Nmap, Dir fuzz, Shodan, Tecnologias com teste de WAF) fazem requisições reais
> ao alvo — confirme o escopo autorizado antes de rodar.

## Ferramentas

### OSINT
| Aba | O que faz |
|---|---|
| **EASM** | Superfície de ataque externa de um domínio: subdomínios (7 fontes passivas), DNS, subdomain takeover, IP/ASN, portas de risco expostas, serviços web, TLS, arquivos sensíveis (.git, .env, actuator, phpinfo...), SPF/DMARC/CAA/DNSSEC |
| **Harvester** | OSINT estilo theHarvester: subdomínios (crt.sh, HackerTarget, Anubis, AlienVault OTX, urlscan.io, RapidDNS, Wayback, com nova tentativa automática em falha passageira), e-mails do site, IPs/provedores, ambientes de homologação expostos, lista final pronta pra copiar |
| **Whois** | Registro de domínio/IP via RDAP: registrador, datas, status, nameservers, bloco de rede |
| **Shodan** | Consulta o Shodan InternetDB (grátis, sem tocar no alvo): portas, tecnologias, CVEs já indexados |
| **CVE** | Busca no NVD por produto/versão ou CVE-ID, com flag CISA KEV (exploração ativa) e score EPSS |
| **Subdomínios** | Descoberta ativa + passiva: 7 fontes passivas + wordlist, resolução DNS e detecção de subdomain takeover (CNAME pendente) |
| DNS | A, AAAA, MX, NS, TXT, CNAME, SOA, CAA via DNS-over-HTTPS; reverso de IP |

### Black Hat (testes ativos/agressivos)
| Aba | O que faz |
|---|---|
| **Nmap** | Scanner nativo estilo Nmap: descoberta, portas TCP (-p) e UDP (-sU), versão de serviço (-sV), SO por TTL (-O), scripts (-sC: ftp-anon, redis-info, ssl-cert, http-title/methods...), alerta de versões vulneráveis e consulta automática de CVEs no NVD por serviço detectado |
| Hosts na LAN | Descobre dispositivos ativos na rede (sondagem TCP/RST + echo) com hostname |
| Port scan | TCP connect scan (top, all, faixas, listas) com captura de banner |
| **UPnP/SSDP** | Descobre dispositivos UPnP na LAN (roteadores, TVs, câmeras, IoT) com fabricante/modelo |
| **Câmeras/CFTV** | Descoberta ONVIF (WS-Discovery) na LAN + varredura de portas de câmera/DVR/NVR (RTSP, Hikvision, Dahua, Xiongmai/XMeye) com fingerprint de fabricante e alerta de falhas conhecidas (backdoor, credencial padrão). Não tenta login nem acessa o vídeo |
| **Web Pentest** | Rastreia o site e testa XSS (3 contextos), SQLi (erro + booleana + time-based), **SSTI**, **LFI/path traversal**, **command injection** (refletido + time-based), **SSRF** (metadado de nuvem/loopback), **IDOR heurístico**, open redirect, CRLF/header injection, CORS mal configurado, Host header injection, segredos e **endpoints de API escondidos em JS**, CSRF ausente e GraphQL introspection. Só GET e payloads inofensivos — não envia formulário nem tenta login |
| **Vuln Scan** | Varredura de assinaturas estilo Nuclei: dezenas de painéis DevOps/monitoramento mal protegidos (Jenkins, H2 Console, Druid, Solr, Tomcat Manager, Elasticsearch, etcd, CouchDB, RabbitMQ, Portainer, Docker Registry...), endpoints de debug de framework (Laravel, Symfony, Django, ASP.NET), backups/dumps esquecidos na raiz e indicadores de **webshell já plantado** (checagem defensiva pós-incidente) |
| **Auto Recon** | Pipeline de um clique estilo bug bounty: subdomínios (passivo) → quais estão vivos → tecnologia → Vuln Scan em cada host vivo → Web Pentest completo no alvo principal. Reaproveita as outras ferramentas do app — tudo cai no mesmo terminal e relatório |
| **Repeater** | Monta uma requisição HTTP à mão (método, URL, cabeçalhos, corpo) e mostra a resposta crua — edite e reenvie, como o Repeater do Burp/ZAP |
| **Intruder** | Fuzzer de parâmetro estilo Burp Intruder (modo Sniper): marque o ponto com `§FUZZ§`, escolha um conjunto de payloads (SQLi/XSS/LFI/command injection/números/customizado) e compare status, tamanho e tempo de cada resposta |
| **Dir fuzz** | Varredura de caminhos/arquivos sensíveis (admin, .git, .env, backups, actuator...) com wordlist embutida |
| **Bypass CDN (origem)** | Procura o servidor de origem real por trás de um CDN/WAF (Cloudflare, Akamai, Sucuri...): cruza subdomínios, SPF/MX e IPs manuais, conecta direto em cada candidato e compara com o site oficial. Se achar, o CDN/WAF pode ser contornado por completo — causa raiz clássica de "tínhamos WAF e mesmo assim fomos invadidos". Só leitura/comparação, não autentica em nada |

### Rede
| Aba | O que faz |
|---|---|
| Minha rede | Interface, IP local, gateway, DNS, sub-rede e IP público |
| **Sistema** | whoami/uname: usuário Linux do app, modelo, versão Android/kernel, interfaces de rede, indícios de root |
| Ping | Ping do sistema |
| **Traceroute** | Mapeia os saltos até o alvo via ping com TTL crescente |
| **Wi-Fi** | Redes próximas: canal, sinal, criptografia; alerta rede aberta, WEP, WPA1, WPS |

### Web (leitura/recon não intrusivo)
| Aba | O que faz |
|---|---|
| HTTP/TLS | Cabeçalhos de segurança, cookies, vazamento de versão, certificado TLS |
| **TLS scan** | Protocolos suportados (SSLv3–TLS1.3), cifras fracas, detalhes do certificado |
| **Tecnologias** | Fingerprint de CMS/framework/JS (com versões vulneráveis), CDN e WAF |

### Análise
| Aba | O que faz |
|---|---|
| Encoder/Hash | Base64, URL, hex, MD5/SHA-1/SHA-256/SHA-512 (offline) |
| **JWT** | Decodifica e audita tokens JWT: alg=none, expiração, claims sensíveis |

### Terminal
| Aba | O que faz |
|---|---|
| **Terminal** | Shell real do Android (`/system/bin/sh`, sem root), desenvolvido direto no app — não embute nem chama o Termux. Fica vivo durante a sessão: `cd` e variáveis de ambiente valem pro próximo comando, como um terminal de verdade. Roda com as mesmas permissões do app e os binários que já vêm no sistema (ls, cat, ps, ip, netstat, ping, grep, find...). Sem gerenciador de pacotes e sem pseudo-terminal completo (comandos interativos como vim/top não funcionam). Digite `exit` pra encerrar o shell atual |

## Quando acha uma falha

Os achados aparecem **direto no terminal, ao vivo**, destacados em vermelho/laranja/amarelo
conforme a severidade — não precisa gerar relatório pra ver (o relatório é só pra guardar/enviar
depois). Subdomínios, hosts, e-mails etc. também aparecem na hora, linha por linha, enquanto a
ferramenta roda.

Ao final de qualquer execução que gerar achados, o app também pergunta o que você quer fazer:
consultar o CVE do item mais grave (abre a aba CVE já preenchida) ou gerar o relatório na hora.

## Proxy (opcional)

Abaixo do campo ALVO tem um indicador "⚙ Proxy" — toque nele pra configurar um proxy HTTP ou
SOCKS5 que **você** escolhe (ex.: um bridge SOCKS5 de VPN, um SOCKS próprio, Burp/ZAP). Fica
salvo entre sessões. SOCKS5 vale pra tudo (chamadas HTTP e os scanners de porta/conexão direta);
proxy HTTP só vale pras chamadas HTTP(S). Ferramentas baseadas em UDP (SSDP/WS-Discovery) e as
que chamam binários do sistema (ping/traceroute) não passam pelo proxy.

Isso é diferente de uma VPN instalada no celular (ProtonVPN e afins): se você já tem uma VPN
conectada no sistema, **todo** o tráfego do aparelho — incluindo o do Blackout Hat — já passa por
ela automaticamente, sem precisar configurar nada aqui. O campo de proxy serve pra quando você
quer apontar só o tráfego das ferramentas pra algo específico (ex.: um SOCKS seu, ou o proxy do
Burp pra inspecionar as requisições). O app nunca escolhe nem troca de proxy/país sozinho — é
sempre o que você configurar, de forma explícita.

## Relatórios

Toda ferramenta executada fica registrada na sessão, e os achados recebem uma severidade
(Crítica, Alta, Média, Baixa ou Info), com evidência e recomendação.

O botão **👁 Relatório** abre o relatório HTML **direto na tela do app** (sumário executivo,
tabela de achados, detalhamento, metodologia e anexo com as saídas) — não precisa baixar nada
para ver. De dentro dele, o botão **Salvar** grava em `Download/BlackoutHat/` nos formatos:

- **HTML** (o mesmo que você acabou de ver)
- **Markdown**: para colar em wiki, Jira ou Confluence
- **CSV**: para planilha, SIEM ou abertura de tickets

De dentro do relatório também tem o botão **📧 EMAIL**, que salva os arquivos e já abre o
app de e-mail do celular com HTML, Markdown e CSV anexados — só escrever o destinatário e
mandar. **Zerar** limpa tudo para um novo trabalho.

O botão **P/ DEVS** copia só o resultado da última ferramenta executada (comando, achados e
saída completa) para a área de transferência — pronto para colar num ticket, e-mail ou
mensagem para a equipe de desenvolvimento, sem precisar gerar o relatório inteiro.

## Como instalar no RedMagic

1. Abra a aba **Releases** do repositório no celular e baixe o `BlackoutHat.apk` mais recente
   (cada push gera um build automático pelo GitHub Actions).
2. Abra o arquivo e permita "instalar apps de fontes desconhecidas" para o navegador/arquivos.
3. Abra o **Blackout Hat** e aceite o termo de uso autorizado.

## Compilar localmente

```bash
./gradlew assembleRelease
# APK em app/build/outputs/apk/release/app-release.apk
```

Requer JDK 17 e Android SDK (compileSdk 35).
