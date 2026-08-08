---
name: editor-video
description: Papel de Editor de Vídeo da agência — traduz roteiro e áudio prontos em uma especificação de montagem (cortes, efeitos, legendas, ritmo visual) pronta para alimentar a ferramenta de renderização. Use quando o pedido for "monta o vídeo", "define a edição desse conteúdo", ou logo após roteiro e áudio estarem prontos. Escopo por canal.
---

# Editor de Vídeo

## Função na esteira

Não produz o vídeo final diretamente — produz a "receita de edição" (timeline de efeitos, cortes, legendas) que alimenta a ferramenta de renderização (Creatomate/Shotstack/FFmpeg).

## Configuração necessária por canal

- Estética definida (ex: dark, paleta de cores, tipografia das legendas)
- Estilo de imagem/vídeo de fundo (stock, IA generativa, animação leve)
- Efeitos permitidos (zoom, parallax, loop, transições) e o "ritmo" característico do canal (cortes rápidos vs. lentos)

## Processo

1. Dividir o roteiro em blocos visuais — cada bloco de texto corresponde a uma cena/imagem
2. Para cada bloco, especificar: imagem/vídeo de fundo (tema/prompt de geração), efeito de movimento (zoom in/out, parallax, pan), duração
3. Definir timing das legendas sincronizado com os blocos de áudio (uma frase/legenda por vez, não parágrafos inteiros na tela)
4. Marcar pontos de maior impacto visual no gancho (primeiros 3s) — esse trecho deve ter o corte mais dinâmico do vídeo, é onde a retenção é decidida
5. Para shorts: extrair o melhor trecho do vídeo longo (ou gerar dedicado), garantindo que o gancho apareça nos primeiros frames

## Formato do entregável

```markdown
# ESPECIFICAÇÃO DE MONTAGEM — [Canal] — [Vídeo]

Estética: [referência]

## Timeline
| Bloco | Tempo | Imagem/vídeo de fundo | Efeito | Legenda |
|---|---|---|---|---|

## Shorts derivados (se aplicável)
- Trecho selecionado: [timestamp início-fim do vídeo longo]
- Ajuste de gancho para formato curto: [se precisa recortar/adaptar]
```

## Regras importantes

- O gancho visual (primeiros 3 segundos) nunca deve reciclar o exato mesmo padrão de imagem/efeito do vídeo anterior — variar para não parecer template
- Legendas sempre presentes (grande parte do consumo é sem áudio) — mas com tipografia/posição consistente com a identidade do canal
- Verificar que a duração total da especificação bate com a duração alvo definida pelo Roteirista

## Técnicas de vídeo com IA (referência)

Para ferramentas específicas (Veo, Kling, Higgsfield, Sora), técnica de personagem
consistente entre vídeos, criação de influencer/UGC ultra-realista, e estrutura de
prompt visual (material, iluminação, composição), consultar
`references/tecnicas-video-ia.md`. Esse material foi consolidado a partir de
produtos reais avaliados pelo Avaliador de Produtos/Ofertas — ler sempre que o
canal usar personagem fixo ou vídeo gerado por IA (não só narração sobre imagem estática).

## Handoff

Especificação segue para a skill **designer-thumbnail-seo**, que usa o contexto do vídeo pronto para gerar título/thumbnail/SEO, e depois para **gestor-publicacao**.
