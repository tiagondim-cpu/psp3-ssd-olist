# Prompts de execução no Claude Code

## Como iniciar

Abra o terminal na raiz do repositório e inicie o Claude Code em Plan Mode:

```bash
claude --permission-mode plan
```

Em seguida, envie o prompt da Fase 1.

## Fase 1 — planejamento obrigatório

```text
Leia integralmente CLAUDE.md, README.md, PROMPT_CLAUDE_CODE.md, requirements.txt, .gitignore e o conteúdo atual de notebooks/. Esta é uma atividade individual de PSP3 com prazo em 2 de setembro de 2026 às 18h.

Antes de editar qualquer arquivo:
1. resuma a situação-problema e a decisão que a análise apoiará;
2. confirme a unidade de análise, variáveis, filtros, hipóteses e métodos estatísticos;
3. detalhe como evitará duplicação na junção e resultados inventados;
4. apresente a arquitetura célula a célula do notebook;
5. descreva como fará download, execução integral e validação em ambiente limpo;
6. liste riscos, decisões pendentes e critérios objetivos de aceite;
7. indique qualquer simplificação que aumente robustez sem reduzir a qualidade acadêmica.

Não implemente ainda. Termine pedindo aprovação do plano.
```

## Fase 2 — implementação após aprovação

```text
Plano aprovado. Implemente o núcleo completo conforme CLAUDE.md em notebooks/analise_atraso_satisfacao_olist.ipynb.

Requisitos desta execução:
- use apenas as duas tabelas autorizadas da Olist;
- baixe os dados por kagglehub sem expor credenciais;
- documente o funil de exclusões;
- garanta uma linha por order_id;
- crie métricas, gráficos, teste de proporções, intervalos e medidas de efeito;
- derive a matriz de apoio à decisão dos resultados observados;
- não implemente machine learning;
- não invente números em Markdown;
- execute o notebook inteiro e mantenha as saídas;
- atualize o README apenas com resultados que tenham sido confirmados pela execução.

Ao terminar, apresente: arquivos alterados, comandos executados, verificações aprovadas, resultados principais, limitações e problemas ainda existentes. Não faça push antes da revisão.
```

## Fase 3 — auditoria adversarial

```text
Agora atue como revisor estatístico e professor rigoroso, sem presumir que a implementação está correta.

Audite:
1. duplicidades, cardinalidade da junção e unidade de análise;
2. definição de atraso por data-calendário;
3. coerência do funil de exclusões;
4. valores ausentes e possíveis vieses de seleção;
5. cálculo das proporções, diferença absoluta, risco relativo e intervalos;
6. hipótese, teste utilizado, alternativa e interpretação do valor-p;
7. correspondência entre números, gráficos, resumo e conclusão;
8. linguagem causal indevida;
9. reprodutibilidade no Colab e ausência de caminhos locais ou segredos;
10. clareza suficiente para defesa oral do aluno.

Corrija somente problemas comprovados, execute novamente o notebook do início ao fim e entregue um relatório final de aprovação ou reprovação por critério. Não acrescente escopo decorativo.
```

## Fase 4 — publicação

Somente depois da auditoria aprovada:

```text
Faça a checagem final de publicação. Confirme que o notebook executado e o README estão consistentes, que não há dados brutos ou credenciais no Git e que o link do Colab aponta para o caminho correto. Mostre o diff final e proponha uma mensagem de commit objetiva. Aguarde minha autorização antes de enviar ao GitHub.
```

