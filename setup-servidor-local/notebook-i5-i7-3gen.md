# Ajustes para Notebook i5/i7 3ª Geração (Ivy Bridge + NVIDIA GT 635M)

Perfil de hardware: Core i5-3210M ou i7-3632QM, 8-16GB RAM, GPU dedicada NVIDIA
GeForce GT 635M (2GB). Diferente do `hardware-antigo.md` (Core 2 Duo) — esta
máquina tem SLAT/VT-x e GPU com encoder de vídeo, então aguenta bem mais carga.

## Diferença chave: pode escolher VM ou nativo

Este processador tem suporte a SLAT — Hyper-V/VM funciona normalmente aqui, sem
a restrição técnica do Core 2 Duo. Ainda assim, **nativo continua recomendado**
para uso como servidor dedicado — mais simples, sem overhead de virtualização,
sem trade-off real a favor da VM nesse cenário específico.

## Upgrades físicos que valem a pena (em ordem de impacto)

1. **SSD no lugar do HD original**: maior impacto de performance de todos —
   Postgres e Docker sofrem muito com HD mecânico, independente da CPU
2. **RAM para 16GB**: barato, o notebook aceita — dá folga para rodar
   Docker + Postgres + n8n + processamento simultâneo sem swap
3. GPU já é suficiente como está (2GB de VRAM não precisa de upgrade para este uso)

## Aproveitando a GPU dedicada (NVENC) na renderização

A GT 635M tem encoder de vídeo por hardware (NVENC), que tira boa parte do
trabalho de renderização da CPU:

```bash
# Instalar driver NVIDIA primeiro (necessário para NVENC funcionar)
sudo apt install -y nvidia-driver-535  # ou versão compatível mais recente disponível

# Renderização usando GPU em vez de CPU
ffmpeg -i input.mp4 -c:v h264_nvenc -preset fast -vf "scale=1280:720" output.mp4
```

Comparar o tempo de renderização entre `-preset fast` (GPU/NVENC) e
`-preset ultrafast` (CPU) na prática — em geral NVENC é mais rápido e libera a
CPU para o n8n/Postgres continuarem respondendo bem durante a renderização.

**Se o driver NVIDIA der problema** (comum em notebooks com GPU híbrida
Intel+NVIDIA/Optimus no Linux): pode renderizar via CPU normalmente como
fallback — não é bloqueante, só um ganho de performance a mais.

## Whisper (legendas)

Com 4 núcleos/8 threads, o modelo `base` ou até `small` do Whisper já roda em
tempo razoável — não precisa ficar preso no `tiny` como no hardware mais fraco:
```bash
whisper audio.mp3 --model base --language Portuguese
```

## Processamento em paralelo

Diferente do Core 2 Duo, esta máquina aguenta processar mais de uma etapa ao
mesmo tempo (ex: gerar áudio de um vídeo enquanto renderiza outro) — não precisa
forçar fila estritamente sequencial, mas ainda vale monitorar temperatura sob
carga sustentada (notebook continua sendo notebook):
```bash
sudo apt install -y lm-sensors
watch -n 2 sensors
```

## Expectativa realista de volume

Esta máquina aguenta operação de **múltiplos canais em volume baixo-médio**
(alguns vídeos por dia, não só por semana) sem grande sofrimento — um passo
acima do hardware muito antigo, mas ainda vale monitorar a fila de renderização
conforme os canais forem crescendo. Sinal de migrar para mini PC dedicado/VPS
continua sendo o mesmo: fila de produção acumulando atraso real, não uma métrica
fixa de "X vídeos por dia".
