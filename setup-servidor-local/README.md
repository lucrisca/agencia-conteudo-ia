# Setup do Servidor Local — Agência de Conteúdo IA

Passo a passo para transformar um PC em servidor doméstico rodando a infraestrutura da agência com custo mínimo.

## 1. Sistema operacional

- Recomendado: **Ubuntu Server** (ou Debian), sem interface gráfica, dedicado a essa função
- Alternativa se o PC também é usado no dia a dia: **WSL2** no Windows

## 2. Instalar Docker

```bash
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker $USER
```

Reinicie a sessão após esse comando.

## 3. Estrutura de pastas

```bash
mkdir -p ~/agencia-conteudo-ia/{n8n_data,postgres_data,media,docker}
cd ~/agencia-conteudo-ia
```

## 4. Subir os serviços principais

Usar o `docker-compose.yml` e o `.env.example` (neste mesmo diretório):

```bash
cp .env.example .env
nano .env   # preencher POSTGRES_PASSWORD e N8N_HOST com valores reais
docker compose up -d
```

O `.env` com valores reais **nunca** é commitado no Git (já coberto pelo
`.gitignore` da raiz do repositório) — só o `.env.example` (com placeholder) fica versionado.

**Nota sobre RAG/busca de referências**: se as referências das skills (checklist,
técnicas de vídeo, ângulos de copy) crescerem muito, considerar trocar a imagem
`postgres:16` por `pgvector/pgvector:pg16` no `docker-compose.yml` — mesma
interface, mas já vem com a extensão `pgvector` pronta para indexar e buscar
esses documentos por similaridade em vez de carregar o arquivo inteiro a cada consulta.

## 5. Expor com Cloudflare Tunnel

Evita abrir portas no roteador e resolve o problema de IP dinâmico.

1. Criar conta gratuita na Cloudflare e apontar um domínio (~R$40/ano)
2. Instalar o `cloudflared`:
```bash
curl -L --output cloudflared.deb https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared.deb
```
3. Autenticar e criar o túnel:
```bash
cloudflared tunnel login
cloudflared tunnel create agencia
cloudflared tunnel route dns agencia seudominio.com
```
4. Configurar `~/.cloudflared/config.yml`:
```yaml
tunnel: agencia
credentials-file: /root/.cloudflared/<TUNNEL_ID>.json

ingress:
  - hostname: seudominio.com
    service: http://localhost:5678
  - service: http_status:404
```
5. Rodar como serviço:
```bash
sudo cloudflared service install
```

## 6. Ferramentas de IA locais

```bash
# FFmpeg (renderização de vídeo)
sudo apt install ffmpeg

# Whisper (legendas, via Docker)
docker pull ghcr.io/openai/whisper

# Piper TTS (voz)
mkdir ~/piper && cd ~/piper
curl -LO https://github.com/rhasspy/piper/releases/latest/download/piper_linux_x86_64.tar.gz
tar -xvzf piper_linux_x86_64.tar.gz
```

## 7. Garantir disponibilidade 24/7

```bash
# Desativar suspensão automática
sudo systemctl mask sleep.target suspend.target
```

- Docker já reinicia os serviços automaticamente (`restart: unless-stopped` no compose)
- Se possível, usar nobreak — queda de energia durante uma publicação agendada é a falha mais comum

## 8. Backup (não pular)

```bash
# Rodar diariamente via cron
docker exec postgres pg_dump -U agencia agencia_db > backup_$(date +%F).sql
```

Copiar esse backup para um local fora do PC (Google Drive, HD externo, outro serviço de nuvem). Sem isso, uma falha de disco apaga todo o histórico.

Exemplo de linha de cron (editar com `crontab -e`):
```
0 3 * * * docker exec postgres pg_dump -U agencia agencia_db > ~/agencia-conteudo-ia/backups/backup_$(date +\%F).sql
```

## 9. Ordem de teste recomendada

1. Acessar `https://seudominio.com` e confirmar que o n8n abre
2. Criar um workflow de teste simples no n8n ("hello world")
3. Testar a conexão do n8n com o Postgres
4. Testar geração de um áudio curto com Piper TTS
5. Testar um render simples com FFmpeg
6. Só então montar o pipeline completo (roteiro → voz → vídeo → publicação)

## Checklist rápido

- [ ] Ubuntu Server ou WSL2 instalado
- [ ] Docker instalado e usuário no grupo docker
- [ ] docker-compose.yml configurado (senha e domínio trocados)
- [ ] Serviços no ar (`docker compose ps`)
- [ ] Domínio + Cloudflare Tunnel funcionando
- [ ] FFmpeg, Whisper, Piper instalados
- [ ] Suspensão automática desativada
- [ ] Backup diário configurado e testado (restaurar 1x pra confirmar que funciona)
- [ ] Nobreak conectado (se disponível)
