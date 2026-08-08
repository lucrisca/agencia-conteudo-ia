# Arquitetura Agentic/Workflow

Este documento detalha como o ciclo de produção da agência (as 11 skills) pode ser
implementado tecnicamente como um **grafo de agentes com estado**, não apenas como
um pipeline linear. Complementa `arquitetura.md` — aqui o foco é a lógica interna
de orquestração, não a infraestrutura de hospedagem.

## Por que grafo, não pipeline linear

O n8n resolve bem fluxos lineares (A → B → C → D). Mas o ciclo real da agência tem
pontos de decisão, loops e interrupções que um pipeline linear simples não modela
com naturalidade:

- O Avaliador de Produtos/Ofertas pode **rejeitar** uma oferta e mandar o
  Estrategista escolher outro tema — não é uma linha reta
- O Diretor de Conteúdo em modo recomendação precisa **pausar** o fluxo até
  aprovação humana — um ponto de interrupção, não uma etapa que sempre executa
- O ciclo Analista de Mídias → Estrategista se repete a cada período — é um
  **loop**, não uma sequência que termina

LangGraph (ou equivalente) modela isso nativamente como nós e arestas condicionais,
com estado compartilhado entre eles. n8n continua sendo a camada de execução
prática (chamar APIs, mover arquivos, publicar) — os dois trabalham juntos: n8n
executa as ações de cada nó, LangGraph decide a sequência e as condições.

## Estado compartilhado (o que "flui" entre os agentes)

```
EstadoCiclo {
  canal_id
  ciclo_periodo
  relatorio_analista        # output do Analista de Mídias
  calendario_editorial       # output do Estrategista
  status_aprovacao           # pendente / aprovado / rejeitado
  oferta_avaliada            # output do Avaliador de Produtos/Ofertas (se aplicável)
  roteiro
  especificacao_voz
  especificacao_video
  pacote_seo
  status_publicacao
  autonomy_level             # recomendacao | autonomo (por canal, ver arquitetura.md)
}
```

Cada agente/skill lê o que precisa desse estado e escreve sua parte antes de
passar adiante — mesmo princípio dos "Handoffs" já descritos em cada SKILL.md,
só que agora como estrutura de dados explícita, não só combinação verbal.

## Mapa do grafo (nós e arestas condicionais)

```
[analista-midias] 
      ↓
[estrategista-nicho]
      ↓
   ┌──[envolve afiliado/produto?]──sim──→ [avaliador-produtos-ofertas]
   │                                              │
   │                                    ┌─rejeitado┴─aprovado
   │                                    ↓            ↓
   │                          [estrategista-nicho]   │
   │                          (escolhe outro tema)    │
   não                                                │
   ↓                                                  ↓
[diretor-conteudo] ←── ponto de interrupção (human-in-the-loop) ──┘
      │
   autonomy_level == recomendacao? ──sim──→ PAUSA até aprovação humana
      │
      não/aprovado
      ↓
[roteirista] → [diretor-voz] → [editor-video] → [designer-thumbnail-seo]
      ↓
[gestor-publicacao]
      ↓
   (fecha o ciclo, dado alimenta o próximo relatório do analista-midias)
```

## Padrões de implementação por tipo de nó

- **Nós lineares** (Roteirista → Diretor de Voz → Editor de Vídeo →
  Designer de Thumbnail/SEO): podem continuar como workflow simples no n8n,
  chamado por um único nó do grafo — não precisa de complexidade extra aqui
- **Nós condicionais** (Avaliador de Produtos/Ofertas, verificação de
  autonomy_level): implementados como *conditional edges* — a saída de um nó
  decide qual nó roda em seguida
- **Ponto de interrupção humana** (Diretor de Conteúdo em modo recomendação):
  implementado como *interrupt* — o grafo pausa e persiste o estado até receber
  aprovação externa (ex: resposta do usuário no chat, ou aprovação num painel)
- **Loop** (Analista de Mídias ↔ Estrategista a cada ciclo): o próprio grafo
  reinicia a partir do nó `analista-midias` ao fim de cada ciclo, alimentado pelos
  dados de publicação do ciclo anterior

## Agentes que precisam de acesso a ferramentas externas (tool use)

Nem toda skill precisa de "tool calling" — várias são só transformação de texto
(ex: Roteirista, Diretor de Voz). As que precisam de ferramentas externas:

| Skill | Ferramenta externa necessária |
|---|---|
| Avaliador de Produtos/Ofertas | Busca web (validar produto/produtor), leitura de página de vendas |
| Analista de Mídias | API de analytics da plataforma (YouTube Analytics, etc.), busca web para tendências |
| Gestor de Publicação | APIs das plataformas de publicação (YouTube Data API, Meta Graph API, etc.) |
| Gestor Financeiro | Possível integração com Hotmart/Kiwify/Stripe para puxar receita automaticamente |

## Quando vale a complexidade de um grafo de verdade (vs. manter simples)

Não vale migrar pra LangGraph antes de precisar — aplicar o mesmo princípio de
"regra fixa antes de agente" já descrito em `arquitetura.md`. Sinais de que vale a
migração:
- O número de decisões condicionais no ciclo cresceu (hoje são só 2-3: aprovação
  do Diretor, rejeição do Avaliador — ainda gerenciável no n8n com nós de decisão)
- Múltiplos canais rodando em paralelo com autonomy_level diferentes, exigindo
  orquestração mais sofisticada entre eles
- Necessidade de um agente "revisar sua própria decisão" em loop antes de seguir
  (ex: Estrategista testando várias hipóteses de calendário antes de escolher)

Até esses sinais aparecerem, o n8n com nós de decisão simples é suficiente e mais
barato de manter.
