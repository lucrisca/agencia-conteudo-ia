# Pacotes Essenciais — Instalação Consolidada

Todos os pacotes necessários, de todos os guias (`README.md`, `windows.md`,
`hyperv.md`), reunidos aqui para rodar de uma vez só, na ordem certa. Rodar dentro
do terminal Ubuntu (nativo, WSL2, ou VM Hyper-V — é o mesmo comando nos três casos).

## 1. Atualizar o sistema primeiro

```bash
sudo apt update && sudo apt upgrade -y
```

## 2. Utilitários básicos de sistema

```bash
sudo apt install -y \
  net-tools \
  curl \
  wget \
  git \
  unzip \
  nano \
  htop \
  tmux \
  lm-sensors
```

## 3. Segurança básica

```bash
sudo apt install -y openssh-server ufw fail2ban
sudo ufw allow OpenSSH
sudo ufw enable
```

**Nota**: `openssh-server` precisa ser instalado **antes** de `ufw allow OpenSSH` —
é ele quem registra o perfil "OpenSSH" no ufw. Se rodar fora de ordem, aparece o
erro `could not find a profile named 'OpenSSH'`. Se isso acontecer, resolver com:
```bash
sudo apt install -y openssh-server
sudo ufw allow OpenSSH
```
Ou, alternativa sem depender do nome do perfil:
```bash
sudo ufw allow 22/tcp
```

## 4. Docker (base de tudo)

```bash
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker $USER
```
Depois deste comando, sair e entrar de novo na sessão (ou `newgrp docker`) para o
grupo fazer efeito sem precisar reiniciar.

## 5. Linguagens/runtime

```bash
sudo apt install -y python3 python3-pip
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
```

## 6. Ferramentas de IA locais

```bash
sudo apt install -y ffmpeg
```

**Whisper (legendas)** — instalar via pip (mais simples e confiável):
```bash
pip install -U openai-whisper
```
Uso: `whisper audio.mp3 --model base --language Portuguese`

Alternativa em Docker, se preferir integrar via API HTTP com o n8n (imagem real
da comunidade, mantida — a referência anterior a `ghcr.io/openai/whisper` estava
incorreta, essa imagem pública não existe):
```bash
docker pull onerahmet/openai-whisper-asr-webservice:latest
```

Piper TTS:
```bash
mkdir -p ~/piper && cd ~/piper
curl -LO https://github.com/rhasspy/piper/releases/latest/download/piper_linux_x86_64.tar.gz
tar -xvzf piper_linux_x86_64.tar.gz
cd ~
```

## 7. Cloudflare Tunnel

```bash
curl -L --output cloudflared.deb https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared.deb
```

## 8. Subir os serviços principais (n8n + Postgres)

```bash
mkdir -p ~/agencia-conteudo-ia/{n8n_data,postgres_data,media}
cd ~/agencia-conteudo-ia
# copiar o docker-compose.yml e o .env.example (de setup-servidor-local/) para AQUI
cp setup-servidor-local/.env.example .env
nano .env   # preencher POSTGRES_PASSWORD e N8N_HOST com valores reais
docker compose up -d
```

## 9. Editor de código (VS Code)

**Se a VM/servidor tiver interface gráfica** (Ubuntu Desktop):
```bash
sudo apt update
sudo apt install -y wget gpg
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list
sudo apt update
sudo apt install -y code
```

**Se for Ubuntu Server sem interface gráfica**: não instalar ali — usar o VS Code
do Windows com a extensão **Remote - SSH** (`Ctrl+Shift+P` → "Remote-SSH: Connect
to Host" → `usuario@IP_DA_VM`), ou instalar `code-server` para acessar pelo
navegador (`curl -fsSL https://code-server.dev/install.sh | sh`).

## 10. Integração com GitHub

**Configurar identidade do Git:**
```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu-email@exemplo.com"
```

**Gerar chave SSH:**
```bash
ssh-keygen -t ed25519 -C "seu-email@exemplo.com"
```
- Na pergunta do arquivo (`Enter file in which to save the key...`): apertar
  **Enter** para aceitar o caminho padrão
- Na pergunta de senha (`Enter passphrase...`): apertar **Enter** duas vezes
  para não usar senha (mais simples para servidor pessoal), ou digitar uma senha
  se quiser proteção extra

**Copiar a chave pública e cadastrar no GitHub:**
```bash
cat ~/.ssh/id_ed25519.pub
```
Copiar a saída inteira e colar em **GitHub → foto de perfil → Settings → SSH
and GPG keys → New SSH key**.

**Testar conexão:**
```bash
ssh -T git@github.com
```
Deve responder "Hi usuario! You've successfully authenticated".

**Subir o repositório da agência pela primeira vez:**
```bash
cd ~/agencia-conteudo-ia
git init
git add .
git commit -m "Estrutura inicial da agência"
git branch -M main
git remote add origin git@github.com:SEU_USUARIO/agencia-conteudo-ia.git
git push -u origin main
```

**No VS Code**: abrir a pasta (`code ~/agencia-conteudo-ia`), ir na aba **Source
Control** (ícone de ramificação) e clicar em "Sign in to GitHub" quando aparecer.
Extensão recomendada: **GitHub Pull Requests and Issues** (Microsoft).

⚠️ **Segurança**: nunca commitar o arquivo `.env` com valores reais (senha,
domínio) — o `.gitignore` da raiz do repositório já cobre isso, versionando só
o `.env.example` com placeholder. Se algo sensível já foi commitado, trocar a
credencial imediatamente (ver `skills/compliance-lgpd-seguranca/`).

## Checklist de verificação final

```bash
docker --version
docker compose version
node --version
python3 --version
ffmpeg -version
cloudflared --version
```

Se todos retornarem versão sem erro, a base está pronta. Próximo passo: seguir
`README.md` a partir da configuração do Cloudflare Tunnel (domínio + túnel) e
depois `manual-uso.md` para operar o ciclo de produção.
