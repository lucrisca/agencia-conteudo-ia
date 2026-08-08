# Ajustes para Hardware Antigo (ex: Core 2 Duo)

Notas específicas para quem vai instalar o Ubuntu Server **nativo** (sem VM) em
notebook antigo com CPU fraca, mesmo com RAM boa (16GB+). Ler junto com
`pacotes-essenciais.md` — a instalação é a mesma, só o uso no dia a dia muda.

## Por que instalar nativo é a escolha certa aqui

CPUs antigas (ex: Core 2 Duo, 2006-2011) geralmente não têm suporte a SLAT
(Second Level Address Translation), o que torna Hyper-V/VM pouco viável ou
degradado. Instalar o Ubuntu Server direto no hardware (sem camada de
virtualização) evita esse problema por completo e aproveita 100% da RAM disponível.

## Onde o gargalo vai aparecer

Com poucos núcleos e baixa performance por núcleo, o gargalo real não é RAM — é
**processamento**, especificamente:
- Renderização de vídeo (FFmpeg)
- Geração de legenda (Whisper)
- Rodar múltiplos containers Docker pesados ao mesmo tempo

## Ajustes recomendados

### 1. Processar um vídeo por vez, não em paralelo
Configurar o n8n para rodar a etapa de renderização com concorrência 1 (fila
sequencial), em vez de tentar processar vários vídeos ao mesmo tempo — evita que
o processador trave tentando dividir recursos entre múltiplas renderizações.

### 2. Reduzir resolução/qualidade de render no FFmpeg
Renderizar em 720p em vez de 1080p/4K enquanto estiver nesse hardware — reduz
bastante o tempo de processamento sem inviabilizar a qualidade para redes sociais
(a maioria consome em resolução menor no feed mobile de qualquer forma).

Exemplo de flag no comando FFmpeg:
```bash
ffmpeg -i input.mp4 -vf "scale=1280:720" -preset ultrafast output.mp4
```
`-preset ultrafast` prioriza velocidade sobre compressão — arquivo final maior,
mas renderiza bem mais rápido em CPU fraca.

### 3. Whisper no modelo mais leve
Usar o modelo `tiny` ou `base` do Whisper em vez de `medium`/`large` — perde um
pouco de precisão na legenda automática, mas roda em frações do tempo:
```bash
whisper audio.mp3 --model tiny --language Portuguese
```

### 4. Limitar containers Docker simultâneos
No `docker-compose.yml`, considerar não rodar Postgres e n8n com uso pesado ao
mesmo tempo que uma renderização está em andamento — se notar lentidão, pausar
o n8n (`docker compose stop n8n`) durante uma renderização grande, e religar depois.

### 5. Monitorar temperatura e throttling
CPUs antigas de notebook sofrem com superaquecimento sob carga sustentada, o que
reduz ainda mais a performance (throttling). Já com `lm-sensors` instalado:
```bash
watch -n 2 sensors
```
Se a temperatura subir muito durante renderização longa, considerar quebrar o
vídeo em partes menores ou melhorar a ventilação física do notebook.

## Expectativa realista

Esse hardware é suficiente para **validar o modelo de negócio e produzir em baixo
volume** (poucos vídeos por semana, como já era o plano da Fase 1). Não é hardware
para produção em escala — se o volume crescer e a fila de renderização começar a
acumular atraso real, esse é o sinal concreto de migrar para o mini PC dedicado ou
VPS já discutidos anteriormente.
