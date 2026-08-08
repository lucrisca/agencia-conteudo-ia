# Clientes

Configuração por cliente/canal (nicho, plataformas ativas, tom de voz, produto do funil, nível de autonomia).
Cada cliente = uma pasta com config.yaml.

Exemplo de estrutura de config.yaml:

nome: Cliente A
canais:
  - nome: curiosidades
    nicho: curiosidades e fatos
    tom: energico e direto
    plataformas: [youtube, tiktok, instagram]
    modo_estrategista: recomendacao  # ou autonomo
