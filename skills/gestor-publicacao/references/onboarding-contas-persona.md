# Contas, Persona e Regras das Plataformas

## Parte 1 — Criação de contas por plataforma

### YouTube

1. Criar conta Google dedicada ao negócio (não misturar com conta pessoal)
2. Criar o canal via [youtube.com/create_channel](https://youtube.com) — escolher nome, handle (@) e identidade visual (foto, banner)
3. Configurar **YouTube Studio** → Personalização → adicionar descrição do canal, links, palavras-chave
4. Ativar verificação em duas etapas (2FA) — requisito para YPP mais adiante
5. Vincular ao AdSense (só quando for aplicar para monetização — não precisa fazer isso no dia 1)
6. Configurar categoria do canal e classificação "feito para crianças" corretamente já na criação (mesmo que não seja o canal infantil, toda conta precisa declarar isso por vídeo)
7. Gerar credenciais de API (Google Cloud Console → criar projeto → ativar YouTube Data API v3 → OAuth Client ID) para automação

### TikTok

1. Criar conta no app ou via [tiktok.com](https://www.tiktok.com)
2. Converter para **conta Business/Creator** (Configurações → Gerenciar conta → Mudar para conta business) — necessário para acessar métricas completas e Content Posting API
3. Preencher bio, foto de perfil, link (quando disponível)
4. Solicitar acesso à **TikTok Content Posting API** via [developers.tiktok.com](https://developers.tiktok.com) — cadastro de app, descrição de caso de uso (agência gerenciando conteúdo em nome de contas próprias/clientes)
5. Elegibilidade para **Creator Rewards Program** só vem depois: 10.000 seguidores + 100.000 views nos últimos 30 dias + conta em bom histórico — não é algo pra configurar no início, é uma meta a alcançar

### Instagram

1. Criar conta (pode ser direto como Business, sem precisar ser pessoal antes)
2. Converter para **conta Profissional → Business** (não Creator, se a ideia é gerenciar como empresa/agência)
3. Vincular a uma **Página do Facebook** (obrigatório para automação via API)
4. Preencher categoria do perfil, bio, link
5. Acesso via **Meta Graph API** — mesmo processo de app do Facebook abaixo, a permissão de Instagram vem junto

### Pinterest

1. Criar conta em [pinterest.com](https://www.pinterest.com) e converter para **conta Business** (Configurações → Converter conta)
2. Reivindicar o site/domínio da agência ou do produto (Configurações → Contas reivindicadas) — habilita analytics completo e atribuição de tráfego
3. Criar estrutura de **boards** por tema (Pinterest é organizado em boards, não em feed cronológico — cada pin precisa pertencer a um) — ex: um board por sub-tema do nicho (afirmações, meditação guiada, curiosidades de ciência)
4. Solicitar acesso à **Pinterest API v5** via [developers.pinterest.com](https://developers.pinterest.com) — cadastro de app, descrição do caso de uso
5. Não há programa de monetização por ads/RPM direto ao criador — a receita vem de tráfego (afiliados, produto próprio, Programa de Vendedor Verificado para quem vende produto físico/digital)

### Facebook

1. Criar uma **Página** (não perfil pessoal) para o canal/marca — [facebook.com/pages/create](https://www.facebook.com/pages/create)
2. Acessar o [Meta Business Suite](https://business.facebook.com) e criar uma conta comercial (Business Manager)
3. Vincular a Página + a conta do Instagram dentro do mesmo Business Manager
4. Criar um **App** no [developers.facebook.com](https://developers.facebook.com) → solicitar produtos "Pages API" e "Instagram Graph API"
5. Passar pelo **App Review da Meta** — processo de aprovação que pede descrição do caso de uso, vídeo demonstrativo de como o app usa a permissão (planeje isso com antecedência, pode levar semanas)

### Checklist de segurança (todas as plataformas)

- [ ] E-mail dedicado só para as contas da agência (não pessoal)
- [ ] 2FA ativado em todas
- [ ] Senhas armazenadas em gerenciador de senha, não em texto puro
- [ ] Tokens de API salvos em cofre de secrets (nunca no código ou no Git)

---

## Parte 2 — Framework de definição de persona (por canal)

Cada canal precisa de uma persona definida antes do primeiro vídeo — ela orienta roteiro, voz, thumbnail e tom de resposta a comentários.

```markdown
# PERSONA — [Nome do canal]

## Identidade
- Nome do canal:
- Nicho/sub-nicho:
- Proposta de valor em 1 frase (o que o espectador ganha ao assistir):

## Público-alvo
- Faixa etária:
- Interesses relacionados:
- Nível de familiaridade com o tema (iniciante/avançado):

## Voz e tom
- 3 adjetivos que descrevem o tom (ex: calmo, direto, acolhedor):
- O que NUNCA dizer/fazer (ex: promessas categóricas de saúde):
- Referências de canais com tom parecido (para calibrar, não copiar):

## Identidade visual
- Paleta de cores:
- Estilo de thumbnail (composição, presença de texto):
- Elemento visual recorrente (ex: ícone/mascote/moldura fixa):

## Estrutura de conteúdo
- Duração padrão (vídeo longo / short):
- Estrutura de gancho característica:
- CTA padrão (produto, inscrição, outro canal):
```

### Exemplo aplicado (Curiosidades)

```markdown
Nome: [Ex: Fatos Ocultos]
Proposta: "Fatos reais que parecem ficção, contados em menos de 2 minutos"
Tom: direto, curioso, um pouco provocador
Nunca: afirmar como fato algo não verificável, usar clickbait enganoso
Paleta: dark, com destaque em uma cor de acento (ex: âmbar)
Gancho: pergunta intrigante ou afirmação contraintuitiva nos 3s iniciais
CTA: "segue pra não perder o próximo" + link do produto na bio
```

Repetir esse template para cada canal (Saúde/Bem-estar, Meditação, Infantil) antes de gerar o primeiro roteiro — isso vira a configuração base que alimenta as skills `roteirista`, `diretor-voz` e `designer-thumbnail-seo`.

---

## Parte 3 — Regras das plataformas (consolidado)

### Regra transversal mais importante em 2026: rotulagem de conteúdo IA

| Plataforma | Quando precisa rotular | Como |
|---|---|---|
| YouTube | Conteúdo com pessoas, lugares ou eventos realistas gerados/alterados por IA | Toggle "conteúdo alterado ou sintético" no upload |
| TikTok | Conteúdo sintético realista (pessoas, cenas) — detecção automática via credenciais C2PA, mas o creator também pode/deve marcar manualmente | Toggle de divulgação de IA (AIGC) no fluxo de postagem |
| Meta (Instagram/Facebook) | Mídia sintética fotorealista, principalmente em anúncios — detecção automática crescente via C2PA | Label "AI info" aplicado automaticamente quando detectado |
| Pinterest | Rolando filtro/label automático para pins criados ou retocados com IA generativa (rollout em andamento em 2026) | Detecção automática; acompanhar a política, ainda em expansão |

**Ponto de alívio pro seu caso**: conteúdo com **roteiro escrito por IA mas narrado/montado com curadoria humana, sem gerar pessoas/rostos realistas falsos**, geralmente **não exige rotulagem obrigatória** nessas plataformas — a exigência é mais forte para deepfakes e pessoas sintéticas realistas, não para roteiro+voz+imagens ilustrativas. Ainda assim, sempre usar o toggle disponível quando a ferramenta permitir — reduz risco de penalização por "não disclosure".

### Regras específicas por plataforma

**YouTube**
- Conteúdo repetitivo/em massa não é elegível para monetização (política de "conteúdo inautêntico")
- Conteúdo claramente infantil deve ser marcado "Made for Kids" — desativa ads personalizados, comentários, telas finais

**TikTok**
- Creator Rewards exige vídeos de 60s+ — shorts abaixo disso não geram essa receita (mas ainda contam para alcance/crescimento)
- Conteúdo deve ser original — reaproveitamento de conteúdo de terceiros sem transformação significativa é penalizado

**Instagram/Facebook (Meta)**
- Conta precisa estar vinculada a Business Manager para automação
- Reels de baixa qualidade/repetitivos têm RPM/alcance reduzido — mesma lógica do YouTube, ainda que menos formalizada
- Aprovação de app (App Review) é obrigatória para qualquer automação via API — planejar prazo

**Pinterest**
- Não confundir com rede social de engajamento imediato — é motor de busca visual, otimizar título/descrição do pin com palavras-chave, não só estética
- Conteúdo tem vida útil muito mais longa — vale manter um board por sub-tema e nutrir com pins novos regularmente, não é "publica e esquece" como um Reel
- Reivindicar o domínio/site vinculado é o que habilita métricas de tráfego reais — não pular essa etapa

**Geral (todas)**
- Nunca reutilizar roteiro/áudio/vídeo de forma idêntica entre canais da agência — cada canal precisa de variação real, não só troca de marca d'água
- Manter camada de curadoria humana visível no processo (revisão de roteiro, thumbnails) — é o que diferencia "produção assistida por IA" de "conteúdo em massa" aos olhos de todas as plataformas
