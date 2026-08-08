---
name: criador-de-sites
description: Papel de Criador de Sites da agência — produz três tipos de site com propósitos diferentes: (1) blog/portal de conteúdo com artigos curados por humano, como canal novo em formato texto; (2) landing pages de venda para produtos próprios e canais existentes (funil, link agregador, página de oferta); (3) sites como serviço vendável para clientes externos (a agência cobra para criar o site de outra pessoa/negócio). Use para "cria um site", "monta uma landing page", "quero um blog pro canal X", "vamos vender site pra cliente". Escopo cross-canal, com configuração própria por tipo de uso.
---

# Criador de Sites

## Função na esteira

Diferente das outras skills (que produzem vídeo), esta produz **sites** — mas com
três propósitos distintos que exigem processos diferentes. Sempre identificar qual
dos três antes de começar.

## Quando acionar

- Pedido de criar/estruturar um blog ou portal de conteúdo em texto
- Pedido de landing page para vender um produto próprio, divulgar um canal, ou
  centralizar links (ver skill `gestor-publicacao`, que já cobre a necessidade de
  link agregador)
- Pedido de oferecer criação de site como serviço pago para terceiros

## Tipo 1 — Blog/portal de conteúdo (canal novo em texto)

**Regra inegociável, aprendida com um produto rejeitado (ver
`avaliador-produtos-ofertas/references/checklist-sinais-qualidade.md`)**: nunca usar
geração de artigos em massa como motor do blog. Isso aciona o mesmo risco de
"conteúdo inautêntico"/"scaled content abuse" que já vimos para vídeo, mas na versão
do Google Search — sites que publicam artigos em massa e baixa originalidade correm
risco real de penalização/desindexação, mesmo que cada artigo pareça "de qualidade"
individualmente.

Processo:
1. Definir nicho/cluster de temas (reaproveitar o mesmo processo da skill `estrategista-nicho`)
2. Volume baixo e sustentável — poucos artigos por semana, cada um com curadoria
   humana real antes de publicar, não fila automática sem revisão
3. Pesquisa de palavra-chave (reaproveitar lógica de SEO já usada na skill
   `designer-thumbnail-seo` para vídeo)
4. Cada artigo passa por checkpoint humano antes de publicar — mesma lógica de
   "camada de curadoria visível" que protege os canais de vídeo

## Tipo 2 — Landing page (produto próprio ou canal)

Página simples, objetivo único (vender 1 produto ou centralizar links de 1 canal).

Estrutura recorrente observada nos produtos avaliados pelo Avaliador de
Produtos/Ofertas (útil como referência de estrutura, não copiar texto de terceiros):
headline → lista de benefícios concretos → prova social → oferta/preço → garantia →
FAQ → CTA final.

Ferramentas: Systeme.io (já aprovado como afiliado em `ia-pequenos-negocios`),
Hotmart Pages, ou WordPress + page builder para mais controle.

## Tipo 3 — Site como serviço (venda para clientes externos)

Aqui a agência cobra para criar o site de outra pessoa/negócio — vira uma linha de
receita própria, não um canal de conteúdo.

O que costuma estar incluso num pacote desse tipo (referência: material de
divulgação da "Imersão Agênc[IA]", ver `clients/ia-pequenos-negocios/ofertas.md`):
- Landing page configurada
- Tracking (pixel, analytics)
- Configuração de domínio e subdomínio
- Opcional: automação de atendimento (WhatsApp) integrada

Definir ticket e escopo antes de vender — evitar prometer "site completo com
automação total" por preço de landing page simples.

### Atendimento automatizado ao cliente (aprendido de produto avaliado)

Material da "Imersão Agênc[IA]" descreve agentes que respondem e-mail/WhatsApp do
cliente como parte do pacote de uma agência operada por IA. Vale oferecer como
adicional no Tipo 3 (serviço para clientes): um agente que responde dúvidas
frequentes e envia atualização de status do projeto automaticamente, reduzindo o
tempo humano gasto em comunicação repetitiva — sem substituir o contato humano em
decisões importantes (mesmo princípio de "fallback humano obrigatório" do
`docs/arquitetura.md`).

## Formato do entregável

```markdown
# SITE — [Tipo: Blog / Landing Page / Serviço para Cliente] — [Nome do projeto]

## Objetivo
[o que esse site precisa alcançar]

## Estrutura
[seções da página, ou estrutura de categorias do blog]

## Ferramenta escolhida
[Systeme.io / Hotmart Pages / WordPress / outra]

## Conteúdo necessário
[textos, imagens, ou pauta editorial se for blog]

## Checkpoint de qualidade
- [ ] Revisão humana antes de publicar (obrigatório para blog)
- [ ] Sem geração de conteúdo em massa sem curadoria
```

## Regras importantes

- Nunca adotar ferramenta ou serviço cujo modelo central seja gerar conteúdo em
  massa sem curadoria — mesmo que a comissão/economia de tempo pareça atrativa
- Para landing pages e sites de cliente, verificar domínio/hospedagem antes de
  prometer prazo de entrega
- Reaproveitar o link agregador e a estrutura de link já definidos na skill
  `gestor-publicacao` em vez de recriar do zero quando o pedido for só "central de links"

## Handoff

Blog alimenta a skill `analista-midias` com dados próprios (tráfego, palavra-chave
rankeada). Landing pages de produto conectam com o `gestor-publicacao` (link nas
publicações). Serviço para cliente é acompanhado pelo `gestor-financeiro` como nova
fonte de receita (ticket de serviço, não afiliado/produto digital).
