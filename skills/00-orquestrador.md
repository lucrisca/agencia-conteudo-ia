---
name: 00-orquestrador
description: Ponto de entrada da Agência de Conteúdo IA — decide qual dos 13 papéis (Diretor de Conteúdo, Estrategista de Nicho, Roteirista, Diretor de Voz, Editor de Vídeo, Designer de Thumbnail/SEO, Gestor de Publicação, Analista de Mídias, Gestor Financeiro, Avaliador de Produtos/Ofertas, Criador de Sites, Desenvolvedor Técnico, Compliance/LGPD/Segurança) se aplica a um pedido. Use como referência sempre que o pedido for sobre a operação da agência e não estiver claro qual papel específico deve responder.
---

# Orquestrador da Agência de Conteúdo IA

## Como decidir o papel

| Se o pedido for sobre... | Acionar |
|---|---|
| Visão geral, aprovar prioridades entre canais/clientes, decisão final | `diretor-conteudo` |
| Definir pauta/calendário editorial do ciclo | `estrategista-nicho` |
| Escrever roteiro de um vídeo específico | `roteirista` |
| Definir como a narração deve soar | `diretor-voz` |
| Definir cortes, efeitos, montagem do vídeo | `editor-video` |
| Título, thumbnail, descrição, SEO | `designer-thumbnail-seo` |
| Quando/onde publicar, status de conexões de plataforma | `gestor-publicacao` |
| Desempenho, tendências, o que está funcionando | `analista-midias` |
| Custo, receita, ROI por canal | `gestor-financeiro` |
| Vale a pena promover esse afiliado/produto, vale lançar produto próprio | `avaliador-produtos-ofertas` |
| Criar site/blog/landing page, vender site pra cliente | `criador-de-sites` |
| Montar workflow no n8n, integrar API, implementar skill de verdade | `desenvolvedor-tecnico` |
| LGPD, segurança de dados, direito autoral/copyright | `compliance-lgpd-seguranca` |

## Fluxo padrão de um ciclo completo

```
analista-midias (relatório do ciclo anterior)
  → estrategista-nicho (novo calendário)
    → diretor-conteudo (aprova, se em modo recomendação)
      → avaliador-produtos-ofertas (se o tema envolve promover afiliado/produto)
        → roteirista (por canal, por tema aprovado)
          → diretor-voz (por canal)
            → editor-video (por canal)
              → designer-thumbnail-seo (por canal)
                → gestor-publicacao (agenda e publica)
                  ↺ ciclo seguinte alimenta analista-midias de novo

gestor-financeiro roda em paralelo, cross-canal, alimentando diretor-conteudo
```

## Regra geral

Se o pedido cobrir múltiplos papéis de uma vez (ex: "produz o vídeo completo sobre X"), percorrer a esteira na ordem acima, indicando claramente em qual etapa a resposta está e o que falta para as próximas.

Se não estiver claro em qual etapa do ciclo a agência está para aquele cliente/canal, perguntar antes de assumir.
