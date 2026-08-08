# Arquitetura — Agência de Conteúdo IA

## Visão geral

```
                    [Diretor de Conteúdo]
                            |
                    [Estrategista de Nicho]
                            |
                 [Avaliador de Produtos/Ofertas]
                    (se envolve afiliado/produto)
                            |
      ┌─────────────┬───────┼──────────────┬───────────────┐
[Roteirista]   [Diretor de Voz]  [Editor de Vídeo]   [Designer Thumb/SEO]   ← por canal
      └─────────────┴───────┼──────────────┴───────────────┘
                            |
                 [Gestor de Publicação]         ← cross-canal/cliente
                            |
                 [Analista de Mídias]            ← cross-canal/cliente
                            |
                 [Gestor Financeiro]             ← cross-canal/cliente
                            ↺ realimenta o Diretor de Conteúdo
```

## Infraestrutura por camada

| Camada | Ferramenta | Custo aprox./mês |
|---|---|---|
| Orquestração de fluxos | n8n (self-hosted) | $0-20 |
| Banco de dados | Supabase/Postgres | $0-25 |
| Armazenamento de mídia | Cloudflare R2 / S3 | $5-20 |
| Legendas automáticas | Whisper (open-source) | grátis (custo de processamento) |
| Painel/dashboard | Next.js + Supabase Auth | $0-20 |
| Roteiro/texto | API Claude/GPT | uso variável |
| Voz (TTS) | ElevenLabs / Azure TTS / Google TTS | $5-30 |
| Imagem | Flux/Ideogram via API, ou stock (Pexels/Pixabay) | $0-30 |
| Renderização de vídeo | Creatomate / Shotstack / FFmpeg self-hosted | $0-50 |
| Publicação YouTube | YouTube Data API v3 (oficial) | grátis |
| Publicação Meta (FB/IG) | Meta Graph API (app aprovado) — fase 2 | grátis |
| Publicação TikTok | TikTok Content Posting API — fase 2 | grátis |
| Publicação Pinterest | Pinterest API v5 (oficial) — sem RPM de ads, mas canal de tráfego evergreen de baixo custo | grátis |
| Publicação multi-plataforma (fase inicial) | Blotato / Metricool / Publer | $20-80 |
| Kwai | Manual ou parcial via terceira ferramenta | — |
| Logs/monitoramento | Logtail ou log estruturado no Postgres | $0-15 |
| Busca/recuperação de referências (RAG) | pgvector (extensão do Postgres já em uso) | incluso, sem custo extra |
| Orquestração de agentes com estado complexo | LangGraph (complementar ao n8n, não substituto) | uso variável, geralmente gratuito em escala pequena |

## Camada de produto (multi-cliente)

| Necessidade | Ferramenta |
|---|---|
| Autenticação multi-usuário | Supabase Auth / Clerk |
| Cobrança/assinatura | Stripe (internacional) ou Asaas (Brasil) |
| Isolamento de dados por cliente | Schema separado ou `client_id` em cada tabela — decidir antes de escalar |
| Onboarding automatizado | Formulário → grava config no banco → dispara primeiro ciclo |
| Relatórios brandados | Template com identidade do cliente |

## Estrutura de dados de referência

```
clients (id, nome, plano, ...)
channels (id, client_id, nicho, estetica, plataformas_ativas, autonomy_level)
platform_connections (id, channel_id, plataforma, metodo, status, token_ref)
content_items (id, channel_id, tema, roteiro, status_pipeline)
publications (id, content_item_id, plataforma, data_hora, status)
metrics (id, content_item_id, views, retencao, ctr, coletado_em)
costs (id, channel_id, ferramenta, valor, periodo)
revenue (id, channel_id, fonte, valor, periodo)
agent_decisions (id, channel_id, skill, decisao, justificativa, autonomo, timestamp)
```

`channels.autonomy_level`: campo explícito (`recomendacao` / `autonomo`) por canal,
em vez de tratar isso só como convenção de conversa — permite ao Gestor de
Publicação e ao Estrategista consultar o nível de autonomia programaticamente
antes de agir.

`agent_decisions`: tabela de auditoria — toda decisão relevante tomada em modo
autônomo (não em modo recomendação) fica registrada com justificativa, permitindo
revisar depois se o agente errou e ajustar o threshold de autonomia daquele canal.
Implementa de forma concreta o princípio "fallback humano e governança de dados
obrigatórios" já descrito acima.

## Onde armazenar cada tipo de artefato

| O quê | Onde | Motivo |
|---|---|---|
| Skills (9 papéis) | Git, Markdown | Versionável, portável, independe do desenvolvedor |
| Workflows n8n | Git (export JSON) + n8n em execução | Backup e histórico fora da plataforma |
| Prompts por papel/canal | Git, separados do workflow | Editar prompt não deve exigir mexer no fluxo técnico |
| Configuração por cliente/canal | Banco de dados | Precisa ser consultável em tempo real |
| Relatórios gerados | Banco de dados + export opcional PDF/HTML | Histórico consultável |
| Mídia (vídeo/áudio/imagem) | Object storage (R2/S3) | Não cabe em Git |
| Credenciais/tokens | Cofre de secrets (n8n Credentials, Vault, Doppler) | Nunca versionar em texto puro |

## Princípios de design (aprendidos avaliando produtos do próprio nicho)

Consolidado a partir do currículo do produto "De Gargalos a Agentes" (Scoras
Academy) — princípios de decisão sobre automação que valem para a arquitetura
da própria agência, não só como conteúdo a vender:

1. **Regra fixa antes de agente**: antes de automatizar uma decisão com IA/agente,
   perguntar se uma regra fixa e simples resolveria com o mesmo resultado por menos
   custo. Boa parte do checklist do Avaliador de Produtos/Ofertas já são regras
   fixas (ex: "nota com poucas avaliações = tratar como neutro") — não precisam de
   um agente "pensando" a cada vez, só de consulta à regra documentada.
2. **Custo de manutenção nunca é zero**: todo cálculo de ROI/projeção financeira
   (skill `gestor-financeiro`) deve descontar o custo de manter a automação rodando
   (workflows quebrando, API mudando de preço/comportamento, credencial expirando),
   não só o custo direto de uso das APIs.
3. **Fallback humano e governança de dados são obrigatórios, não opcionais**: todo
   sistema em produção (não só as skills que já têm "modo recomendação") precisa
   de um ponto de intervenção humana antes de decisões de maior impacto, e uma
   política clara de onde/como os dados são armazenados e protegidos (ver
   `setup-servidor-local` para backup e credenciais).
4. **Timing da mudança técnica importa**: trocar ferramenta/processo tecnicamente
   superior no meio de uma campanha ou lançamento de canal pode custar mais do que
   vale — avaliar o momento, não só a qualidade técnica da mudança.

## Princípio de portabilidade

Toda a lógica de negócio (skills, prompts, workflows) fica fora de qualquer plataforma proprietária específica — pode ser lida, editada e executada por qualquer desenvolvedor com acesso ao repositório, sem depender de quem construiu o sistema originalmente.
