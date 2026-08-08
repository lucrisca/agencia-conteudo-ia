# Setup em VM no Hyper-V

Caminho alternativo ao WSL2 — roda um Ubuntu Server "de verdade" dentro de uma VM,
isolado do Windows. Requer Windows 10/11 **Pro, Enterprise ou Education** (não
funciona no Windows Home sem contorno).

## Alternativa mais rápida: Criação Rápida (Quick Create)

O Hyper-V tem uma opção de criar VM a partir de uma imagem pronta da galeria da
Microsoft, que já inclui Ubuntu — evita os passos manuais de 2 a 6 abaixo.

No Gerenciador Hyper-V → **Ação → Criação Rápida**:
- Escolher **Ubuntu** na lista de imagens da galeria (baixa automaticamente)
- Clicar em **Criar Máquina Virtual**

**Vantagens**: mais rápido, e a imagem da galeria já vem pré-configurada pra
bootar certo em Geração 2 — geralmente **não precisa desativar Secure Boot**
manualmente como no caminho via ISO.

**Ajustes que ainda vale fazer depois de criada**:
- **Disco**: a imagem da galeria costuma vir com disco pequeno — abrir
  Configurações da VM → Disco Rígido e expandir para 60-128GB (ver requisitos)
- **Memória**: confirmar em Configurações → Memória se está em pelo menos 4GB
  (8GB+ recomendado), ajustar se a Criação Rápida usou um valor menor por padrão
- **Rede**: a Criação Rápida usa o **"Default Switch"** (NAT interno do Windows),
  diferente do Switch Externo do caminho manual. **Isso não é problema para o
  Cloudflare Tunnel** — ele só precisa de conexão de saída para a internet, que o
  Default Switch já fornece. Só importa se você quiser acessar a VM diretamente
  por outro dispositivo da rede local (nesse caso, aí sim vale trocar para um
  Switch Externo depois, em Configurações da VM → Rede)

Depois de criada por esse caminho, seguir a partir do passo 6 (instalar
OpenSSH se ainda não vier, e o resto do guia normalmente).

## Caminho manual (mais controle)

## 1. Ativar o Hyper-V

PowerShell como administrador:
```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```
Reiniciar o PC quando pedir.

Alternativa pela interface: Painel de Controle → Programas → Ativar ou desativar
recursos do Windows → marcar **Hyper-V** → OK → reiniciar.

## 2. Baixar a imagem do Ubuntu Server

Baixar o `.iso` da versão LTS mais recente em
[ubuntu.com/download/server](https://ubuntu.com/download/server).

## 3. Criar o Virtual Switch (rede)

No **Gerenciador Hyper-V** → painel direito → **Gerenciador de Comutador Virtual**:
- Criar um switch do tipo **Externo**, vinculado ao adaptador de rede físico
  (Wi-Fi ou cabo) do notebook
- Isso faz a VM pegar IP direto do seu roteador, como se fosse mais um
  dispositivo na rede — importante pra acessar a VM e pro Cloudflare Tunnel funcionar

## 4. Criar a VM

No Gerenciador Hyper-V → **Ação → Novo → Máquina Virtual**:
- **Geração**: escolher **Geração 2**
- **Memória**: usar Memória Dinâmica, mínimo 4GB (8GB+ se o notebook aguentar, ver
  requisitos já levantados)
- **Rede**: selecionar o Virtual Switch criado no passo 3
- **Disco rígido virtual**: criar novo, 60-128GB (formato VHDX)
- **Opções de instalação**: instalar sistema operacional de um arquivo de imagem
  (.iso) → selecionar o Ubuntu Server baixado

## 5. Ajuste importante antes de ligar (Geração 2)

Antes de iniciar a VM pela primeira vez, abrir **Configurações da VM → Segurança**
e **desmarcar "Inicialização Segura" (Secure Boot)**, ou trocar o template para
"Autoridade de Certificação UEFI da Microsoft" — sem isso o instalador do Ubuntu
pode não bootar em VM Geração 2.

## 6. Instalar o Ubuntu Server

Ligar a VM e seguir o instalador normalmente. Ponto de atenção:
- Na etapa de pacotes adicionais, marcar **OpenSSH Server** — isso permite
  acessar a VM pelo terminal do Windows via SSH depois, sem precisar abrir a
  janela do Hyper-V toda vez
- Anotar o nome de usuário/senha criados

## 7. Depois de instalado

Descobrir o IP da VM (dentro da própria VM):
```bash
ip a
```

Do Windows, acessar via SSH (PowerShell ou Windows Terminal):
```powershell
ssh usuario@IP_DA_VM
```

A partir daqui, seguir o **`README.md` principal a partir do passo 2** (instalar
Docker) — é Ubuntu Server nativo dentro da VM, então os comandos são exatamente
os mesmos do guia original, sem adaptação.

## 8. Rede estável (recomendado)

Como a VM pega IP por DHCP do roteador, o IP pode mudar se o roteador reiniciar.
Duas opções:
- Configurar **reserva de DHCP** no roteador (fixar o IP pra sempre o mesmo,
  baseado no endereço MAC da VM) — mais simples
- Configurar IP estático direto na VM (`/etc/netplan/`) — mais manual

## 9. Manter ligado

Configurar o Hyper-V para iniciar a VM automaticamente com o Windows:
**Configurações da VM → Ação de Inicialização Automática → Iniciar
automaticamente se estava em execução quando o serviço parou**.

Vale o mesmo cuidado do notebook em geral (ver conversa anterior sobre 24/7):
não é obrigatório manter a VM ligada o tempo todo na fase de validação — só
ligar quando for produzir conteúdo, a menos que ative atendimento automatizado
ao cliente.

## Perfil: PC compartilhado (ex: também usado para jogos)

Se o PC for de uso misto — máquina potente que também roda jogos ou outras
tarefas pesadas, não dedicada só à agência — alguns ajustes mudam em relação ao
resto deste guia:

### Alocação de recursos mais generosa

Hardware potente (ex: CPU 8-core, GPU dedicada, 16GB+ RAM) permite alocar mais
para a VM sem comprometer o resto:
- **RAM**: 8-16GB para a VM (usar Memória Dinâmica com esse teto)
- **CPU**: 4 núcleos virtuais
- Isso ainda deixa recursos de sobra para o Windows e outros usos quando a VM
  não estiver rodando tarefa pesada

### Não iniciar automaticamente

Diferente do cenário de servidor dedicado (passo 9 acima), aqui faz mais
sentido a VM **não** subir sozinha com o Windows — em **Configurações da VM →
Ação de Inicialização Automática**, escolher **"Nada"** em vez de "Iniciar
automaticamente". Ligar a VM manualmente só quando for de fato produzir
conteúdo, deixando o PC livre (sem a VM consumindo recursos em segundo plano)
no resto do tempo, inclusive durante jogos.

### Checar espaço em disco antes de criar a VM

Se o disco C: já estiver com uso alto (acima de ~70%), confirmar espaço livre
real antes de alocar o disco virtual da VM (60-128GB) — o disco virtual ocupa
esse espaço no host mesmo que a VM esteja desligada. Considerar criar a VM num
disco/partição secundária se houver uma disponível, para não apertar o disco
principal do sistema.

### Sem necessidade de ajustes de hardware fraco

Como este perfil de máquina é bem mais potente que os cenários de notebook
antigo já cobertos (`hardware-antigo.md`, `notebook-i5-i7-3gen.md`), não é
necessário aplicar os ajustes de renderização reduzida, Whisper `tiny`, ou fila
sequencial — a máquina aguenta os padrões "normais" (1080p, Whisper `base`/`small`,
processamento em paralelo) sem sofrer.
