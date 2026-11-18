# Compara??o das perdas do Ultraball

Tr?s execu??es independentes no Colab (T4), com reconstru??o L1, BCE ou SSIM. Todas mant?m m?scara alfa, Sobel com peso 0,1, KL normalizada por 32, beta final 1, warm-up de 40 ?pocas, semente 42 e limite de 150 ?pocas. A arquitetura permanece a mesma. O notebook `pokeBWTPST_ultraball.ipynb` permite selecionar `original_bordas`, `bce_bordas` ou `ssim_bordas` e treinar do zero.

## Resultados

| Configura??o | Melhor ?poca | L1 no Pok?mon | L1 no fundo | Erro Sobel | KL normalizada |
|---|---:|---:|---:|---:|---:|
| L1 + Sobel | 112 | 130,39 | 13,51 | 73,23 | 19,56 |
| BCE + Sobel | 82 | 134,75 | 15,56 | 74,14 | 18,07 |
| SSIM + Sobel | 59 | 129,20 | 6,95 | 64,52 | 30,98 |

As m?tricas foram recalculadas nas mesmas 60 imagens de valida??o, com reconstru??o determin?stica pelo mu e infer?ncia em CPU. Scheduler, parada antecipada e checkpoint usam o crit?rio comum L1 mascarada + KL com beta final fixo. N?o comparar diretamente totais das diferentes perdas. SSIM usa o erro (1-SSIM)/2, janela gaussiana 7?7, sigma 1,5 e padding refletido; seus par?metros est?o nos metadados.

Visualmente, SSIM preservou mais detalhes nas reconstru??es selecionadas. O usu?rio preferiu BCE a L1 na compara??o anterior. A gera??o pelo prior continua fragmentada e pouco sens?vel aos tipos nas amostras observadas. A galeria usa os mesmos quatro latentes por condi??o, semente 2026 e atributos medianos do treino. Nenhuma configura??o resolveu a gera??o de sprites in?ditos coerentes.

## Canais latentes

KL somada nas posi??es de cada canal e promediada nas 60 imagens de valida??o. Canais numerados de 1 a 16; ?ndices Python de 0 a 15.

- L1: canais 11 (65,2%) e 4 (34,8%).
- BCE: canais 1 (65,9%) e 10 (34,1%).
- SSIM: canais 5 (40,1%), 11 (30,0%) e 14 (29,8%).

A varia??o de mu entre imagens e o teste de zerar um canal por vez complementam a KL. O canal 5 teve o maior efeito na reconstru??o do SSIM. KL n?o mede diretamente porcentagem de caracter?sticas armazenadas. A numera??o n?o representa a mesma caracter?stica entre modelos. Zerar um canal pode produzir valores fora da distribui??o posterior; o teste ? diagn?stico, n?o import?ncia causal absoluta.

## Arquivos e limites

Inclui hist?ricos completos, configura??es, m?tricas e gr?ficos. Checkpoints e ZIPs permanecem localmente na pasta `resultados/comparacao_l1_bce_ssim_sobel_2026-11-18`, ignorada pelo Git. Os hashes em `configuracao.json` identificam os arquivos originais exportados do Colab; os checkpoints n?o est?o nesta publica??o.

Uma execu??o por configura??o, sem repeti??o com outras sementes. A escala das perdas muda o peso efetivo relativo da KL e do Sobel. Foram analisadas imagens de valida??o; esta compara??o n?o constitui avalia??o final em teste nem an?lise de memoriza??o. As figuras foram geradas localmente a partir dos checkpoints treinados no Colab.
