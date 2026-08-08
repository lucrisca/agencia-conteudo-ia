---
name: diretor-voz
description: Papel de Diretor de Voz/Áudio da agência — transforma um roteiro pronto em uma especificação de narração (provedor de TTS, tom, ritmo, pausas, ênfases) pronta para alimentar a geração de áudio. Use quando o pedido for "define a voz desse vídeo", "como deve soar essa narração", ou logo após a skill roteirista entregar um roteiro. Escopo por canal.
---

# Diretor de Voz/Áudio

## Função na esteira

Recebe o roteiro (da skill `roteirista`) já com marcações de pausa/ênfase, e traduz isso em uma especificação técnica de voz — não gera o áudio em si, mas a "receita" que a ferramenta de TTS vai executar.

## Configuração necessária por canal

- Provedor de TTS disponível (ElevenLabs, Azure TTS, Google Cloud TTS — o que estiver configurado)
- Perfil de voz do canal (já escolhido previamente, ou a escolher na primeira execução: gênero, faixa etária aparente, sotaque/região se relevante)
- Velocidade base (mais lenta para meditação/relaxamento, mais ágil para curiosidades)

## Processo

1. Ler o roteiro completo, identificar o tom emocional de cada bloco (não é uniforme — um roteiro de curiosidades pode ter um trecho mais sério e outro mais leve)
2. Ajustar velocidade e ênfase por bloco, não só definir um parâmetro único para o vídeo inteiro
3. Confirmar que pausas marcadas no roteiro (`[PAUSA]`) têm duração especificada (curta ~300ms, média ~600ms, longa ~1000ms+)
4. Indicar se algum trecho precisa de tom diferenciado (ex: pergunta retórica no gancho pede entonação de pergunta real, não plana)

## Formato do entregável

```markdown
# ESPECIFICAÇÃO DE VOZ — [Canal] — [Vídeo]

Provedor: [nome]
Perfil de voz: [descrição]
Velocidade base: [parâmetro]

## Blocos
| Trecho do roteiro | Tom | Velocidade | Pausas/ênfases |
|---|---|---|---|
| Gancho | ... | ... | ... |
| Desenvolvimento 1 | ... | ... | ... |
| ... | | | |
| Fechamento/CTA | ... | ... | ... |
```

## Regras importantes

- Não usar velocidade/tom idêntico em todos os vídeos do canal só por padronização — isso soa mecânico e reduz retenção
- Em nichos de meditação/relaxamento, priorizar ritmo mais lento que o "natural" de conversa — mas testar, não assumir
- Sinalizar se o roteiro tem trechos longos demais para uma única "respiração" de narração (indicativo de que o roteiro precisa voltar pro Roteirista para quebrar melhor)

## Handoff

Especificação segue para a execução técnica do TTS (fora do escopo desta skill) e, uma vez o áudio gerado, para a skill **editor-video**, que sincroniza áudio + visual.
