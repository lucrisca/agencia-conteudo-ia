# Setup no Windows

Adaptação do `README.md` principal para quem vai rodar o servidor local num PC
com Windows, sem trocar de sistema operacional.

## 1. Ativar o WSL2 (Subsystem for Linux)

Abrir o PowerShell **como administrador** e rodar:

```powershell
wsl --install
```

Isso instala o WSL2 com Ubuntu por padrão. Reiniciar o PC quando pedir. Depois de
reiniciar, o Ubuntu abre sozinho e pede para criar um usuário/senha Linux (pode
ser diferente do usuário do Windows).

Se o WSL já estiver instalado mas desatualizado:
```powershell
wsl --update
```

## 2. Instalar o Docker Desktop

Baixar em [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/)
e instalar normalmente (próximo, próximo, concluir).

Depois de instalar, abrir o Docker Desktop e confirmar em
**Settings → Resources → WSL Integration** que a integração com a distro Ubuntu
está **ativada**. Isso é o que permite rodar os comandos Docker de dentro do WSL.

## 3. Abrir o terminal Ubuntu (WSL) para os próximos passos

A partir daqui, todos os comandos do `README.md` principal (Docker, n8n, Cloudflare
Tunnel, FFmpeg, Whisper, Piper) rodam **dentro do terminal Ubuntu do WSL**, não no
PowerShell/CMD do Windows. Para abrir:

- Menu Iniciar → digitar "Ubuntu" → abrir
- Ou, de dentro do PowerShell: `wsl`

## 4. Diferenças práticas em relação ao guia principal

- **Não precisa** rodar `curl -fsSL https://get.docker.com | sh` — o Docker Desktop
  já cuida disso; o WSL só precisa enxergar o Docker via a integração ativada no passo 2
- Os arquivos criados dentro do WSL (ex: `~/agencia-conteudo-ia/`) ficam num sistema
  de arquivos Linux separado do Windows — para acessar pelo Explorer do Windows,
  digitar `\\wsl$\Ubuntu\home\SEU_USUARIO\` na barra de endereço
- Editar arquivos: pode usar o VS Code no Windows normalmente — instalar a extensão
  "WSL" no VS Code e abrir a pasta direto de dentro do terminal Ubuntu com `code .`

## 5. Manter o WSL ligado 24/7

Diferente do Ubuntu Server "puro", o WSL depende do Windows estar ligado. Pontos
de atenção:

- Desativar suspensão automática do Windows (Configurações → Sistema → Energia →
  nunca suspender), não só do WSL
- O WSL2 desliga sozinho se ficar muito tempo sem uso ativo — para manter os
  serviços (n8n, Postgres) sempre no ar, é melhor deixar uma janela do terminal
  Ubuntu aberta, ou configurar o WSL para não hibernar (`wsl.conf`, seção
  `[boot] systemd=true` já ajuda a manter os serviços do Docker mais estáveis)
- Se o PC reiniciar, o Docker Desktop normalmente sobe os containers de novo
  sozinho (graças ao `restart: unless-stopped` do `docker-compose.yml`) — mas
  confirmar isso na primeira vez

## 6. Dali em diante

Seguir o `README.md` principal normalmente a partir do passo 3 (estrutura de
pastas) — tudo roda igual, só a base (WSL2 + Docker Desktop) é diferente do Ubuntu
Server puro.

## Alternativa mais simples (sem WSL)

Se preferir não mexer com WSL, dá para rodar só com o Docker Desktop no modo
Windows containers/Hyper-V direto — mas os comandos Linux (curl, apt install
ffmpeg, etc.) não funcionam nesse modo. Para o setup completo desta agência
(FFmpeg, Whisper, Piper), o WSL2 é o caminho recomendado.
