# PSP3 — Suporte à decisão com dados da Olist

[![Abrir no Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/tiagondim-cpu/psp3-ssd-olist/blob/main/notebooks/analise_atraso_satisfacao_olist.ipynb)

> **Status:** estrutura metodológica criada; análise ainda não executada. Nenhum resultado numérico será apresentado antes da execução e validação integral do notebook.

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

## Fonte dos dados

Será utilizado o [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce), conjunto público e anonimizado de pedidos realizados em marketplaces brasileiros.

Para manter o escopo proporcional à atividade, serão utilizados somente:

- `olist_orders_dataset.csv`;
- `olist_order_reviews_dataset.csv`.

Os dados brutos não serão versionados neste repositório. O notebook deverá obtê-los de forma reproduzível por meio do pacote oficial `kagglehub`.

## Método planejado

1. Auditar estrutura, duplicidades, valores ausentes e datas.
2. Definir uma linha por pedido e documentar todas as exclusões.
3. Calcular dias de atraso usando datas-calendário.
4. Definir avaliação ruim como nota 1 ou 2.
5. Comparar pedidos atrasados e pedidos no prazo.
6. Estimar diferença de risco, risco relativo e intervalos de confiança.
7. Testar a diferença entre as proporções de avaliações ruins.
8. Construir faixas de atraso e uma matriz de priorização gerencial.
9. Registrar limitações e separar associação de causalidade.

## Entrega esperada

O principal artefato será o notebook:

`notebooks/analise_atraso_satisfacao_olist.ipynb`

Ele deverá combinar explicações em português, código Python, resultados executados, gráficos legíveis, teste estatístico, conclusão e recomendação gerencial.

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

O trabalho só será considerado concluído quando o notebook:

- executar do início ao fim em uma sessão limpa do Google Colab;
- baixar os dados sem depender de arquivos locais;
- conservar os resultados e gráficos no arquivo `.ipynb`;
- não expor tokens, senhas ou credenciais;
- produzir conclusões consistentes com os valores calculados.

## Limite de interpretação

Dados observacionais podem indicar associação entre atraso e insatisfação, mas não demonstram que uma ação de atendimento causará melhora nas avaliações. Uma futura política de contato deverá ser validada por experimento controlado.

