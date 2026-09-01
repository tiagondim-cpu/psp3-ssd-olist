# PSP3 — Suporte à decisão com dados da Olist

[![Abrir no Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/tiagondim-cpu/psp3-ssd-olist/blob/main/notebooks/analise_atraso_satisfacao_olist.ipynb)

> **Status:** notebook executado do início ao fim em ambiente limpo, com 29 verificações automáticas aprovadas. Todos os números abaixo vêm da execução registrada no `.ipynb`.

## Identificação

| Campo | Informação |
|---|---|
| Estudante | Tiago Andre Gondim |
| Matrícula | 231013476 |
| Disciplina | EPR0074 — Projeto de Sistemas de Produção 3 |
| Turma | T03 — 2026.2 |
| Professor | Andre Luiz Marques Serrano |
| Modalidade | Atividade individual |

## Situação-problema

Uma operação de comércio eletrônico possui capacidade limitada de atendimento e precisa decidir quais pedidos devem receber acompanhamento proativo quando há atraso na entrega.

## Pergunta de decisão

**Pedidos entregues depois da data prometida apresentam maior risco de receber avaliações ruins? Como esse risco varia de acordo com o número de dias de atraso?**

## Objetivo

Quantificar a associação entre atraso de entrega e avaliação negativa e, a partir das evidências, propor uma regra simples e auditável de priorização do atendimento.

## Principais resultados

**Pedidos entregues depois da data prometida têm risco muito maior de receber avaliação ruim: 62,42% contra 9,27% dos entregues no prazo.** A diferença é de 53,2 pontos percentuais (IC 95%: 51,9 a 54,3) e corresponde a um risco relativo de 6,74x (IC 95%: 6,55 a 6,93). O teste unilateral de duas proporções rejeita a hipótese nula com valor-p abaixo de 0,0001.

**O risco cresce com a duração do atraso até um patamar.** Acima de 14 dias não há aumento adicional detectável: as duas faixas superiores são estatisticamente indistinguíveis entre si.

| Faixa de atraso | Pedidos | Avaliação ruim | Risco relativo | Prioridade sugerida |
|---|---:|---:|---:|---|
| No prazo ou antecipado | 89.443 | 9,27% | referência | Acompanhamento normal |
| 1 a 3 dias | 1.852 | 32,13% | 3,47x | Contato humano proativo |
| 4 a 7 dias | 1.748 | 67,68% | 7,30x | Escalonamento e avaliação de compensação |
| 8 a 14 dias | 1.446 | 80,15% | 8,65x | Escalonamento e avaliação de compensação |
| 15 dias ou mais | 1.335 | 78,35% | 8,45x | Escalonamento e avaliação de compensação |

A regra seleciona 6.381 pedidos, 6,7% da amostra, distribuídos em 24 meses: cerca de 77 contatos humanos e 189 escalonamentos por mês.

Base analisada: 95.824 pedidos entregues, de compras realizadas entre setembro de 2016 e agosto de 2018.

**Robustez.** Duas escolhas de tratamento admitiam alternativa razoável e foram quantificadas no notebook. Conservar a avaliação mais antiga em vez da mais recente move o risco relativo de 6,735 para 6,734. Os pedidos entregues sem avaliação são 0,67% do total apto e estão mais atrasados que a média, 23,68% contra 6,66%; ainda assim, nos dois cenários extremos em que todos eles teriam avaliado bem ou todos mal, o risco relativo permanece entre 6,58 e 6,83.

**Limite da evidência.** Os dados são observacionais. Mostram associação forte entre atraso e insatisfação, o que permite selecionar clientes por risco, mas não demonstram que o atraso cause a nota baixa nem que o contato proativo melhore a avaliação. Comprovar o efeito da ação exige teste A/B.

## Fonte dos dados

Foi utilizado o [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce), conjunto público e anonimizado de pedidos realizados em marketplaces brasileiros. A **versão 2** do conjunto é fixada no identificador de download, de modo que qualquer reexecução leia exatamente os mesmos dados.

Para manter o escopo proporcional à atividade, somente dois arquivos são lidos:

- `olist_orders_dataset.csv`;
- `olist_order_reviews_dataset.csv`.

Nenhum dado bruto é versionado neste repositório. O notebook os obtém de forma reproduzível pelo pacote oficial `kagglehub`.

## Método aplicado

1. Auditoria de estrutura, duplicidades, valores ausentes e datas nos dois arquivos.
2. Uma linha por pedido, com todas as exclusões quantificadas em um funil.
3. Dias de atraso calculados por data-calendário, com as datas normalizadas para o início do dia.
4. Avaliação ruim definida como nota 1 ou 2.
5. Deduplicação das avaliações antes da junção, conservando a mais recente por pedido, e junção validada como um-para-um pelo próprio `pandas`.
6. Comparação entre pedidos atrasados e no prazo, com diferença absoluta, risco relativo e intervalos de confiança.
7. Teste unilateral de duas proporções, por escore e pelo teste z agrupado, e teste de associação ordinal entre as faixas.
8. Faixas de atraso e matriz de priorização derivada do risco absoluto observado.
9. Limitações registradas, separando associação de causalidade.

## Entrega

O artefato principal é o notebook:

`notebooks/analise_atraso_satisfacao_olist.ipynb`

Ele combina explicação em português, código Python, resultados executados, quatro gráficos, teste estatístico, matriz de decisão, conclusão e limitações. O texto das conclusões é gerado a partir das variáveis calculadas, e os números do resumo executivo são conferidos automaticamente contra a análise.

## Estrutura do repositório

```text
psp3-ssd-olist/
├── README.md
├── CLAUDE.md
├── PROMPT_CLAUDE_CODE.md
├── requirements.txt
├── .gitignore
└── notebooks/
    ├── README.md
    └── analise_atraso_satisfacao_olist.ipynb
```

## Reprodutibilidade

Verificado na execução registrada no `.ipynb`:

- execução do início ao fim em ambiente limpo, sem erros e sem células sem saída;
- dados obtidos por `kagglehub`, sem dependência de arquivos locais, caminhos absolutos ou Google Drive;
- resultados e os quatro gráficos conservados no arquivo `.ipynb`;
- nenhum token, senha ou credencial exposto ou versionado;
- 29 verificações automáticas aprovadas, incluindo unicidade de `order_id`, cardinalidade da junção, coerência das variáveis construídas e conferência dos números do resumo executivo.

O notebook interrompe a própria execução se qualquer verificação falhar.

