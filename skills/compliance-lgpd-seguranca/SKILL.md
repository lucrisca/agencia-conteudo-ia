---
name: compliance-lgpd-seguranca
description: Papel de Compliance da agência — cobre LGPD (proteção de dados pessoais), segurança da informação e direitos autorais/copyright. Funciona como consultoria cruzada, acionada sempre que outra skill lida com dado pessoal de cliente/audiência, credenciais, ou conteúdo que possa esbarrar em direito autoral de terceiros. Use para "isso tá de acordo com a LGPD", "como proteger os dados do cliente", "posso usar essa imagem/música/citação", "checa direito autoral disso". Escopo cross-canal e cross-skill — não substitui assessoria jurídica real, mas evita os erros mais comuns antes de precisar de um advogado.
---

# Compliance — LGPD, Segurança e Direitos Autorais

## Função na esteira

Não é uma etapa fixa do ciclo de produção — é consultada sob demanda por outras
skills sempre que a situação envolver dado pessoal, segurança de acesso, ou
conteúdo de terceiros. Funciona como o Avaliador de Produtos/Ofertas, mas para
risco legal/regulatório em vez de risco de qualidade de oferta.

## Quando acionar

- `criador-de-sites` coletando dado de cliente (Tipo 3 — serviço) ou de visitante
  (Tipo 2 — landing page com formulário/e-mail)
- `gestor-publicacao` armazenando credenciais/tokens de plataformas de clientes
- `roteirista` ou `editor-video` usando referência de terceiros (música, imagem,
  citação, estilo de marca)
- Qualquer skill perguntando "posso usar isso comercialmente"
- Configuração de novo servidor/infraestrutura (`desenvolvedor-tecnico`)

## Parte 1 — LGPD (Lei Geral de Proteção de Dados)

### O que se aplica à agência

Qualquer coleta de dado pessoal (nome, e-mail, telefone, dado de navegação) — seja
de cliente do serviço de sites, de assinante de newsletter, ou de lead de landing
page — está sujeita à LGPD, independente do tamanho da operação.

### Checklist mínimo

- **Base legal**: identificar por que o dado é coletado (execução de contrato,
  consentimento explícito, legítimo interesse) — não coletar "porque pode ser útil depois"
- **Política de privacidade publicada**: toda landing page ou site que coleta dado
  (mesmo só e-mail) precisa de link visível para política de privacidade e termos
  de uso — padrão que já aparece nos produtos avaliados pelo Avaliador de Ofertas
  (ex: rodapé com "Política de Privacidade" e "Termos de Uso")
- **Retenção mínima necessária**: não guardar dado além do tempo que o propósito
  exige — definir prazo de retenção por tipo de dado, não guardar indefinidamente
- **Direitos do titular**: ter processo (mesmo que manual no início) para atender
  pedidos de acesso, correção ou exclusão de dados
- **Dado de terceiro em automação de atendimento**: se a skill `criador-de-sites`
  implementar atendimento automatizado (WhatsApp/e-mail, ver referência aprendida
  da "Imersão Agênc[IA]"), as mensagens trocadas com o cliente final também são
  dado pessoal — se aplica a mesma lógica de retenção e proteção

### Quando procurar assessoria jurídica real

Esta skill não substitui advogado — sinalizar essa necessidade quando: a operação
passar a atender múltiplos clientes com dado sensível, ou houver qualquer suspeita
de incidente de vazamento de dado.

## Parte 2 — Segurança da Informação

### Já coberto em outros documentos (não duplicar, só referenciar)

- Credenciais em cofre de secrets, nunca hardcoded (`docs/arquitetura.md`)
- 2FA em todas as contas de plataforma (`gestor-publicacao/references/onboarding-contas-persona.md`)
- Backup diário com teste de restauração (`setup-servidor-local/README.md`)

### Adições desta skill

- **Controle de acesso por privilégio mínimo**: cada credencial/token só deve ter
  o escopo mínimo necessário (ex: token do YouTube só com permissão de upload, não
  acesso total à conta Google)
- **Criptografia em trânsito e em repouso**: conexão ao servidor sempre via HTTPS
  (Cloudflare Tunnel já cobre isso); dados sensíveis no banco (ex: dado de cliente
  do serviço de sites) devem considerar criptografia em repouso conforme o volume crescer
- **Plano de resposta a incidente mínimo**: se uma credencial vazar ou um site for
  comprometido — revogar/trocar credencial imediatamente, notificar o cliente
  afetado se for dado de terceiro, documentar o que aconteceu
- **Segurança de plugins/dependências** (relevante para `criador-de-sites` ao usar
  WordPress): manter plugins atualizados, evitar plugins sem manutenção recente —
  vetor de ataque comum em sites WordPress desatualizados

## Parte 3 — Direitos Autorais / Copyright

### Princípios já estabelecidos em outras skills (reforçados aqui)

- Originalidade real no roteiro — síntese em texto próprio, nunca reprodução
  próxima de fonte específica (`roteirista`)
- Filtro ético do Avaliador de Produtos/Ofertas já cobre violação explícita de
  copyright como motivo de rejeição automática

### Adições específicas de direito autoral

- **Conteúdo gerado por IA pode reproduzir material de treinamento**: ferramentas
  de geração de imagem/vídeo/música ocasionalmente reproduzem elementos muito
  próximos do dado de treinamento (estilo, composição, até trechos quase idênticos).
  Fazer checagem pontual antes de publicar conteúdo gerado, especialmente quando o
  prompt pedir "no estilo de [artista/marca específica]"
- **Estilo de marca como referência vs. cópia de IP**: usar o "tom" de uma marca
  grande como inspiração de prompt é diferente de reproduzir elementos protegidos
  (logo, personagem, trilha sonora específica) — já registrado em
  `editor-video/references/tecnicas-video-ia.md`, reforçado aqui como regra geral
  de toda a agência, não só do Editor de Vídeo
- **Citação de dados/pesquisa de terceiros**: ao usar estatística de fonte externa
  (ex: "MIT NANDA 2025", como visto em produto avaliado), citar a fonte e
  paraphrasear a informação — não reproduzir parágrafos inteiros do material original
- **Licença de assets de terceiros**: música, fonte, banco de imagem usados na
  produção devem ter licença comercial válida — verificar antes de usar,
  especialmente em ferramentas gratuitas que podem ter uso restrito a fins
  não-comerciais
- **Reclamação de direito autoral em plataforma (ex: YouTube Content ID)**: se
  ocorrer, não ignorar — avaliar se é procedente antes de contestar, e ajustar o
  processo de geração que originou o problema para não repetir

## Formato do entregável (quando consultada)

```markdown
# CHECAGEM DE COMPLIANCE — [Situação avaliada]

## Área
[LGPD / Segurança / Direitos Autorais]

## Risco identificado
[descrição objetiva]

## Recomendação
[o que fazer/ajustar antes de prosseguir]

## Requer assessoria jurídica real?
[ ] Não — resolvido com o ajuste acima
[ ] Sim — sinalizar ao Diretor de Conteúdo antes de prosseguir
```

## Handoff

Recomendações desta skill alimentam diretamente quem a acionou (`criador-de-sites`,
`gestor-publicacao`, `roteirista`, `editor-video`, `desenvolvedor-tecnico`) — não
tem output próprio no ciclo de produção, só ajusta a execução das outras skills.
