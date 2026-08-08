# Agência de Conteúdo IA

Estrutura da agência de produção de conteúdo automatizada com IA — pipeline de roteiro → voz → vídeo → SEO → publicação → análise, organizado como 9 papéis profissionais (skills), com orquestração via n8n e distribuição multi-plataforma.

## Estrutura do repositório

```
/skills                 → os 9 papéis da agência, em Markdown (SKILL.md)
/workflows               → exports JSON dos fluxos do n8n
  /por-canal              → workflows específicos de cada canal
  /compartilhados         → pipeline principal e publicação multi-plataforma
/prompts                 → prompts-base por papel/canal
/clients                 → configuração por cliente (nicho, plataformas, tom)
/docs                     → documentação técnica e operacional
  arquitetura.md          → infraestrutura, ferramentas, estrutura de dados
  manual-uso.md           → como operar a agência no dia a dia
/reports                 → relatórios gerados por ciclo
```

## Os 13 papéis

1. **Diretor de Conteúdo** (`diretor-conteudo`) — decisão final, cross-canal
2. **Estrategista de Nicho** (`estrategista-nicho`) — calendário editorial, cross-canal
3. **Avaliador de Produtos/Ofertas** (`avaliador-produtos-ofertas/SKILL.md`) — valida afiliados (digitais e físicos via marketplace) e produtos próprios antes da produção, cross-canal. Inclui `references/checklist-sinais-qualidade.md`, consolidado a partir de avaliações reais
4. **Roteirista** (`roteirista`) — por canal
5. **Diretor de Voz/Áudio** (`diretor-voz`) — por canal
6. **Editor de Vídeo** (`editor-video/SKILL.md`) — por canal. Inclui `references/tecnicas-video-ia.md` com técnicas de vídeo com IA consolidadas de produtos avaliados (personagem consistente, ferramentas Veo/Kling/Higgsfield/Sora, estrutura de prompt visual)
7. **Designer de Thumbnail/SEO** (`designer-thumbnail-seo/SKILL.md`) — por canal. Inclui `references/framework-angulos-copy.md` com 12 ângulos de copy testados, extraídos de material de divulgação real
8. **Gestor de Publicação** (`gestor-publicacao/SKILL.md`) — cross-canal/cliente. Inclui `references/onboarding-contas-persona.md` com o passo a passo de criação de conta por plataforma, template de persona por canal e regras de rotulagem de IA
9. **Analista de Mídias e Tendências** (`analista-midias`) — cross-canal/cliente
10. **Gestor Financeiro** (`gestor-financeiro`) — cross-canal/cliente
11. **Criador de Sites** (`criador-de-sites`) — blog editorial, landing pages e sites como serviço para clientes, cross-canal
12. **Desenvolvedor Técnico** (`desenvolvedor-tecnico`) — implementa workflows n8n, integrações de API e infraestrutura a partir das especificações já documentadas pelas outras skills; representa o papel que seria terceirizado para uma empresa de tecnologia externa
13. **Compliance — LGPD, Segurança e Direitos Autorais** (`compliance-lgpd-seguranca`) — consultoria cruzada acionada sempre que outra skill lida com dado pessoal, credenciais, ou conteúdo de terceiros; não substitui assessoria jurídica real

Ponto de entrada: `skills/00-orquestrador.md` — decide qual papel aciona para cada tipo de pedido.

## Ciclo de produção (resumo)

```
Analista de Mídias (relatório) → Estrategista (calendário) → Diretor de Conteúdo (aprova)
  → Avaliador de Produtos/Ofertas (se envolve afiliado/produto)
    → Roteirista → Diretor de Voz → Editor de Vídeo → Designer de SEO/Thumbnail
      → Gestor de Publicação → [novo ciclo alimenta o Analista de Mídias]

Gestor Financeiro roda em paralelo, cross-canal, alimentando o Diretor de Conteúdo.
```

## Status do projeto

- [x] Papéis definidos e skills escritas
- [x] Arquitetura de infraestrutura desenhada
- [ ] Workflows n8n implementados
- [ ] Painel/dashboard
- [ ] Primeiro canal em produção
- [ ] Camada multi-tenant (para clientes externos)

Ver `docs/arquitetura.md` para detalhes técnicos e `docs/manual-uso.md` para o guia operacional. Ver `docs/arquitetura-agentica.md` para o mapeamento das skills como grafo de agentes (LangGraph), com estado compartilhado, pontos de interrupção humana e loops.
