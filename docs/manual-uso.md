# Manual de Uso — Agência de Conteúdo IA

## Como rodar um ciclo completo (manual, via chat com IA)

1. Cole os dados de desempenho do período (views, retenção, CTR, watch time) e peça o relatório do **Analista de Mídias**
2. Com o relatório em mãos, peça ao **Estrategista de Nicho** o calendário do próximo ciclo
3. Revise e aprove o calendário (papel de **Diretor de Conteúdo** — você, nesta fase)
4. Para cada tema aprovado, peça ao **Roteirista** o roteiro do canal correspondente
5. Peça ao **Diretor de Voz** a especificação de narração a partir do roteiro
6. Peça ao **Editor de Vídeo** a especificação de montagem
7. Peça ao **Designer de Thumbnail/SEO** o pacote de título/descrição/thumbnail
8. Peça ao **Gestor de Publicação** o calendário de publicação e o status das conexões
9. No fechamento do mês, peça ao **Gestor Financeiro** o relatório de custo x receita
10. Volte ao **Diretor de Conteúdo** para consolidar tudo e decidir o próximo ciclo

## Nível de autonomia

Por padrão, todos os papéis operam em **modo recomendação**: produzem o entregável e aguardam validação antes de considerar aplicado. Conforme a operação de um canal amadurece e as recomendações do Estrategista se mostram consistentemente boas, pode-se mudar esse canal para **modo autônomo** — registrar essa decisão em `clients/<cliente>/config.yaml`.

## Adicionando um novo canal

1. Definir configuração do canal: nicho, tom de voz, estética, plataformas ativas
2. Criar `clients/<cliente>/config.yaml` com esses dados
3. Rodar o primeiro ciclo manualmente (sem histórico do Analista de Mídias, o Estrategista parte só da definição inicial de nicho)
4. A partir do segundo ciclo, o loop completo (Analista → Estrategista → produção → publicação) já opera com dado real

## Adicionando um novo cliente (agência como produto)

1. Onboarding: coletar nicho, plataformas desejadas, tom de marca, produto/oferta do cliente (se houver)
2. Gestor de Publicação verifica quais plataformas já têm conexão aprovada vs. pendente — comunicar isso claramente ao cliente antes de prometer automação total
3. Primeiro ciclo roda em modo recomendação, sempre — nunca modo autônomo com cliente novo
4. Relatórios (Analista, Financeiro, Diretor de Conteúdo) devem sair em formato apresentável ao cliente, sem jargão interno

## Alertas permanentes (todos os ciclos)

- Verificar sinais de conteúdo repetitivo/template-driven (risco de classificação como "conteúdo inautêntico")
- Checar afirmações de saúde/finanças antes de publicar (nichos sensíveis)
- Nunca classificar conteúdo infantil incorretamente quanto a "Made for Kids"
- Verificar tokens/permissões de API antes de cada leva de publicações
