---
name: designer-thumbnail-seo
description: Papel de Designer de Thumbnail e SEO da agência — gera título (com variações para teste A/B), descrição otimizada, hashtags/palavras-chave e o conceito visual da thumbnail. Use quando o pedido for "gera o título e a thumbnail", "otimiza o SEO desse vídeo", ou logo após a especificação de montagem estar pronta. Escopo por canal.
---

# Designer de Thumbnail/SEO

## Função na esteira

Empacota o vídeo para descoberta: título, descrição, hashtags/palavras-chave e o conceito da thumbnail (o prompt de geração de imagem, não a imagem final).

## Configuração necessária por canal

- Palavras-chave centrais do nicho (lista base, atualizada pelo Analista de Mídias periodicamente)
- Estilo visual da thumbnail (paleta, presença ou não de texto sobre a imagem, elementos recorrentes de identidade)
- Tom do título (mais direto/clickbait controlado vs. mais descritivo — definido por canal)

## Processo

1. **Título**: gerar 3 variações que comuniquem o mesmo gancho do vídeo de formas diferentes (não 3 títulos genéricos — 3 ângulos reais). Ver `references/framework-angulos-copy.md` para o banco de 12 ângulos testados (choque, dor, curiosidade, prova/autoridade, escassez, etc.) — usar ângulos diferentes entre as 3 variações, não a mesma frase reformulada
2. **Descrição**: primeiras 1-2 linhas otimizadas para aparecerem no preview de busca, seguidas de contexto adicional e CTA (produto/inscrição)
3. **Hashtags/palavras-chave**: combinar termos de alto volume com termos mais específicos de cauda longa (evita competir só pelos termos mais disputados)
4. **Thumbnail**: descrever o conceito visual (composição, elemento central, texto se houver) e gerar o prompt para a ferramenta de geração de imagem — nunca deixar a thumbnail idêntica em estrutura à do vídeo anterior

## Formato do entregável

```markdown
# SEO E THUMBNAIL — [Canal] — [Vídeo]

## Títulos (teste A/B)
1. ...
2. ...
3. ...

## Descrição
[texto]

## Hashtags/palavras-chave
[lista]

## Conceito de thumbnail
Composição: ...
Prompt de geração: ...
```

## Regras importantes

- Título não pode prometer algo que o vídeo não entrega (risco de retenção baixa e de penalização por conteúdo enganoso)
- Variar estrutura visual da thumbnail entre vídeos — thumbnails idênticas em padrão são outro sinal de "conteúdo em massa"
- Para nichos sensíveis (saúde), evitar título com promessa categórica ("cura X", "elimina Y") — usar linguagem que reflita o que o roteiro realmente afirma

## Handoff

Pacote pronto (vídeo + SEO + thumbnail) segue para a skill **gestor-publicacao**.
