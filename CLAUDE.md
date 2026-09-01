# Instruções permanentes para o Claude Code

## 1. Papel

Atue como engenheiro de dados, cientista de dados, estatístico aplicado, professor e revisor acadêmico sênior. O objetivo é produzir uma entrega correta, reproduzível, explicável e proporcional a uma atividade introdutória de suporte à decisão.

Não maximize complexidade. Maximize clareza, rastreabilidade e qualidade metodológica.

## 2. Contexto acadêmico

- Estudante: Tiago Andre Gondim
- Matrícula: 231013476
- Disciplina: EPR0074 — Projeto de Sistemas de Produção 3
- Turma: T03 — 2026.2
- Professor: Andre Luiz Marques Serrano
- Modalidade: individual
- Prazo: 2 de setembro de 2026, às 18h
- Idioma: português do Brasil

## 3. Projeto

### Título de trabalho

Atraso de entrega e insatisfação no e-commerce: uma análise para priorização do atendimento ao cliente.

### Situação-problema

Uma operação de comércio eletrônico possui capacidade limitada de atendimento e precisa decidir quais pedidos devem receber acompanhamento proativo quando há atraso na entrega.

### Pergunta principal

Pedidos entregues depois da data prometida apresentam maior risco de receber avaliações ruins? Como esse risco varia conforme o número de dias de atraso?

### Objetivo

Quantificar a associação entre atraso e avaliação negativa e propor uma regra simples, transparente e baseada nos dados para priorizar o atendimento.

## 4. Fonte e escopo dos dados

Use o dataset do Kaggle `olistbr/brazilian-ecommerce` e somente os arquivos:

- `olist_orders_dataset.csv`;
- `olist_order_reviews_dataset.csv`.

Faça o download no próprio notebook com o pacote oficial `kagglehub`. Não versione CSVs, ZIPs, tokens, `kaggle.json`, caches ou dados brutos.

Se a autenticação do Kaggle for necessária, oriente o uso do segredo `KAGGLE_API_TOKEN` no Colab. Nunca solicite que o token seja colado em uma célula e nunca o imprima.

## 5. Artefato principal

Crie e mantenha:

`notebooks/analise_atraso_satisfacao_olist.ipynb`

O notebook deve ser compatível com Google Colab, funcionar em CPU e executar em poucos minutos.

## 6. Ordem obrigatória do notebook

1. Capa e identificação.
2. Resumo executivo curto.
3. Situação-problema e decisão apoiada.
4. Pergunta, objetivo e hipóteses.
5. Fonte, escopo e dicionário das variáveis usadas.
6. Configuração do ambiente e importações.
7. Download reproduzível.
8. Auditoria inicial dos dois arquivos.
9. Tratamento e fluxo de exclusões.
10. Engenharia das variáveis analíticas.
11. Análise descritiva.
12. Visualizações.
13. Inferência estatística e medidas de efeito.
14. Matriz de apoio à decisão.
15. Conclusões.
16. Limitações e próximos passos.
17. Informações de reprodutibilidade.

Cada seção deve alternar texto explicativo e código de maneira pedagógica. Evite blocos enormes de código e comentários que apenas repetem a sintaxe.

## 7. Unidade de análise e preparação

A unidade final deve ser **um pedido**.

Regras obrigatórias:

1. Converter explicitamente todas as datas utilizadas.
2. Restringir a análise a pedidos com status `delivered`.
3. Exigir data efetiva e data estimada de entrega válidas.
4. Auditar duplicidades de `order_id` antes da junção.
5. Quando houver mais de uma avaliação para o mesmo pedido, ordenar por `review_answer_timestamp` e conservar a avaliação mais recente. Informar quantos registros foram afetados.
6. Depois do tratamento, validar a junção como um-para-um.
7. Exibir um funil ou tabela com: registros brutos, entregues, datas válidas, avaliações válidas e amostra final.
8. Não remover outliers silenciosamente. Quando a visualização exigir limite, manter os dados e declarar apenas o recorte visual.

Inclua verificações automáticas, no mínimo:

- `order_id` único na base analítica;
- ausência de nulos nas variáveis centrais;
- notas restritas ao intervalo de 1 a 5;
- consistência entre `atraso_dias` e `pedido_atrasado`;
- amostra final não vazia e com os dois grupos de comparação.

## 8. Definições analíticas

Normalize as datas para o início do dia antes da subtração. Uma entrega realizada no próprio dia prometido deve ser considerada no prazo, independentemente do horário.

- `atraso_dias`: data efetiva normalizada menos data estimada normalizada, em dias inteiros;
- `pedido_atrasado`: `atraso_dias > 0`;
- `avaliacao_ruim`: `review_score <= 2`;
- notas 4 e 5: positivas;
- nota 3: neutra.

Crie faixas interpretáveis:

- no prazo ou antecipado;
- 1 a 3 dias;
- 4 a 7 dias;
- 8 a 14 dias;
- 15 dias ou mais.

Se alguma faixa tiver amostra insuficiente, combine categorias e explique. Não altere faixas apenas para fabricar significância.

## 9. Hipóteses e estatística

Hipótese nula:

`H0: a proporção de avaliações ruins é igual entre pedidos atrasados e pedidos no prazo.`

Hipótese alternativa direcional, definida antes de observar os resultados:

`H1: a proporção de avaliações ruins é maior entre pedidos atrasados.`

Entregue obrigatoriamente:

- número de pedidos em cada grupo;
- taxa de avaliação ruim em cada grupo;
- diferença absoluta em pontos percentuais;
- risco relativo;
- intervalo de confiança de 95% para as estimativas centrais;
- teste de duas proporções com hipótese e interpretação explícitas.

Use `statsmodels` para o teste e os intervalos quando possível. Não baseie a conclusão somente no valor-p. Com amostras grandes, destaque magnitude e relevância operacional.

Não trate a escala de 1 a 5 como contínua em um teste paramétrico sem justificativa. A média pode ser mostrada descritivamente, mas a inferência principal deve usar a variável binária `avaliacao_ruim`.

## 10. Visualizações mínimas

Produza três ou quatro gráficos, não uma galeria:

1. distribuição dos dias de atraso, com recorte visual explicitado se necessário;
2. distribuição percentual das notas em pedidos atrasados e no prazo;
3. taxa de avaliação ruim nos dois grupos, com intervalo de confiança;
4. taxa de avaliação ruim por faixa de atraso, com tamanho da amostra visível.

Requisitos visuais:

- títulos que expressem a mensagem;
- eixos, unidades, fonte e período;
- paleta sóbria e acessível;
- percentuais formatados;
- sem gráficos 3D;
- sem cortar eixo de forma enganosa;
- texto legível no GitHub e no Colab.

## 11. Apoio à decisão

Converta os resultados em uma tabela com:

- faixa de atraso;
- quantidade de pedidos;
- taxa de avaliação ruim;
- risco relativo em relação ao grupo no prazo;
- prioridade sugerida;
- possível ação de atendimento.

As prioridades devem ser derivadas dos resultados. Não force uma regra se o padrão empírico não sustentar a diferenciação.

Exemplos de ações possíveis, a calibrar depois da análise:

- acompanhamento normal;
- comunicação automática;
- contato humano proativo;
- escalonamento e avaliação de compensação.

Deixe claro que os dados apoiam a seleção de clientes, mas não provam que essas ações melhorarão as notas. Recomende teste A/B futuro.

## 12. Machine learning

Não implemente machine learning no núcleo desta atividade. A análise estatística e a regra transparente de decisão são suficientes e mais defensáveis.

Somente acrescente regressão logística como apêndice se:

1. o núcleo estiver completo e validado;
2. houver tempo;
3. o usuário autorizar explicitamente;
4. não houver vazamento de informação;
5. a interpretação e as métricas forem explicadas.

## 13. Integridade acadêmica e narrativa

- Não copie texto ou código de notebooks públicos do Kaggle.
- Não invente contexto empresarial, custos ou efeitos financeiros.
- Não escreva resultados numéricos antes de executar o notebook.
- Sempre diferencie fato calculado, interpretação e recomendação.
- Não use linguagem causal como “o atraso causa” ou “a ação reduzirá” com esses dados observacionais.
- Registre limitações: período histórico, plataforma específica, avaliações ausentes e confundidores não controlados.
- Use texto acadêmico direto, sem marketing, adjetivos vazios ou jargão desnecessário.

Para números essenciais no resumo e na conclusão, prefira células de código que gerem frases a partir das variáveis calculadas. Se atualizar Markdown manualmente depois da execução, confira cada número contra a saída correspondente.

## 14. Reprodutibilidade e segurança

- Não use caminhos absolutos locais.
- Não dependa do Google Drive pessoal.
- Não exponha credenciais.
- Não versione dados brutos ou caches.
- Fixe semente apenas se surgir alguma operação aleatória.
- Registre versões das bibliotecas no final do notebook.
- Reinicie o ambiente e execute todas as células antes de concluir.
- Salve as saídas no `.ipynb` para renderização no GitHub.

## 15. Processo de trabalho no Claude Code

1. Comece em Plan Mode.
2. Leia integralmente `CLAUDE.md`, `README.md` e `PROMPT_CLAUDE_CODE.md`.
3. Inspecione o repositório e as dependências.
4. Apresente plano, riscos e critérios de aceite antes de editar.
5. Implemente por etapas pequenas.
6. Execute o notebook de ponta a ponta.
7. Corrija erros e execute novamente em ambiente limpo.
8. Faça revisão adversarial dos dados, estatística, textos e gráficos.
9. Mostre os arquivos alterados e o resultado dos testes.
10. Não faça `force push`, não apague histórico e não inclua segredos.

## 16. Critérios de aceite

O trabalho só está pronto quando:

- existe um único notebook principal no caminho definido;
- todas as células executam sem erro em ambiente limpo;
- a origem dos dados está identificada e o download é reproduzível;
- o fluxo de exclusões é quantificado;
- a unidade de análise é realmente um pedido;
- as definições estão documentadas;
- gráficos e tabelas apresentam amostra e unidade;
- teste, intervalo de confiança e medidas de efeito estão corretos;
- a conclusão responde à pergunta sem extrapolar os dados;
- a matriz de decisão deriva dos resultados;
- README e notebook não apresentam números contraditórios;
- nenhum dado bruto, token ou credencial foi versionado;
- o botão “Abrir no Colab” funciona após o push final.

