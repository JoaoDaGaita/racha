# AGENTS.md — Racha

Instruções para agentes de código que trabalham neste repositório.

## O projeto

O Racha é um agente de IA que organiza o pedido de almoço de uma equipe: entende o que cada pessoa quer, controla o teto de gasto individual, monta uma sacola única e calcula quanto cada um paga, com taxa de entrega e cupom rateados.

É um projeto de portfólio, independente e sem vínculo com o iFood. O restaurante (TchanTchio) e o cardápio são fictícios.

**Fonte da verdade do produto:** `docs/prd.md`. Leia antes de qualquer trabalho. Cenários (C1 a C10), regras de negócio (R1 a R8) e métricas (M1 a M6) são referenciados por esses códigos em specs, tarefas, testes e commits.

## Processo: Spec-Driven Development

O trabalho segue esta ordem. Não pule etapas e não escreva código antes de a etapa anterior estar aprovada pelo autor.

| # | Etapa | Saída | Situação |
|---|---|---|---|
| 1 | PRD | `docs/prd.md` | Concluído |
| 2 | Tech Spec | `docs/techspec.md` | A fazer |
| 3 | Tarefas | `docs/tasks/NN-nome.md` | A fazer |
| 4 | Implementação | `src/` | A fazer |
| 5 | Testes e avaliações | `src/**/*.test.ts`, `evals/` | A fazer |
| 6 | Code review | `docs/review.md` | A fazer |

Ao concluir uma etapa, atualize esta tabela e a tabela de status do `README.md`.

### Como conduzir cada etapa

- **Tech Spec e tarefas:** não escreva o documento de uma vez. Faça perguntas ao autor, um ponto por vez, sempre com uma sugestão e a justificativa. Só escreva depois das respostas.
- **Tarefas:** pequenas, uma responsabilidade cada, com critério de aceite, testes esperados e os códigos do PRD que atendem.
- **Implementação:** uma tarefa por vez. Antes de codificar, diga o plano em poucas linhas. Depois, mostre o que mudou e como verificar.
- **Code review:** revise contra a Tech Spec, as regras deste arquivo e os critérios de aceite da tarefa. Registre achados e correções em `docs/review.md`.

## O autor precisa entender tudo

Este repositório será apresentado em entrevistas técnicas. O autor precisa saber explicar cada decisão.

- Explique o porquê das escolhas, não só o quê.
- Quando houver mais de um caminho razoável, apresente as opções e deixe o autor decidir.
- Prefira código simples e legível a soluções engenhosas.
- Não adicione dependências, arquivos ou funcionalidades que não foram pedidos.

## Stack

- TypeScript (modo estrito) e Node.js
- Mastra para o agente, as ferramentas e a memória
- Claude, via API da Anthropic, como modelo de linguagem
- Zod para os contratos de entrada e saída das ferramentas
- Vitest para testes
- Next.js e React para a tela de chat
- Dados em arquivos JSON e Markdown; sem servidor de banco de dados

Mastra e os SDKs de IA mudam rápido. Antes de usar uma API dessas bibliotecas, consulte a documentação atual ou os tipos instalados em `node_modules`. Não escreva de memória.

## Regras de arquitetura

1. **O LLM nunca faz contas.** Teto, rateio, somas e arredondamento são funções determinísticas em código, com testes. O modelo interpreta a mensagem, chama ferramentas e redige a resposta.
2. **Dinheiro em centavos, como inteiros.** Nunca use ponto flutuante para valores monetários. Formate em reais só na apresentação.
3. **A soma do rateio fecha no centavo** (R6). Todo teste de rateio verifica que a soma das partes é igual ao total.
4. **Lógica de negócio separada do agente.** As regras ficam em funções puras, sem dependência do Mastra ou do LLM, para serem testadas isoladamente. As ferramentas do agente são uma camada fina sobre elas.
5. **Respostas sobre políticas vêm dos documentos** (C8, C9). Se a informação não estiver nos documentos, o agente diz que não sabe.
6. **Fora do escopo é fora do escopo.** Consulte a seção 8 do PRD antes de propor qualquer funcionalidade.

## Segurança

- **Nunca** leia, exiba, copie ou faça commit do arquivo `.env`.
- Nunca escreva chaves, tokens ou segredos em código, testes, logs ou documentação.
- Variáveis novas entram em `.env.example`, sem valor.
- Antes de cada commit, confira com `git status` que nenhum segredo está sendo incluído.

## Convenções

**Idioma:** documentação, mensagens de commit e textos para o usuário em português. Identificadores de código (variáveis, funções, tipos, arquivos) em inglês.

**Commits:** Conventional Commits, em português, um assunto por commit.

```
feat: função de rateio com taxa e cupom (R3, R4, R6)
test: casos de arredondamento do rateio
docs: Tech Spec inicial
fix: item compartilhado não contava para o teto (R5)
chore: configuração do Vitest
```

Cite os códigos do PRD ou o número da tarefa quando fizer sentido.

**Git:** não faça `git push` nem reescreva histórico sem o autor pedir. Mostre o que será commitado antes de commitar.

**Testes:** toda regra de negócio tem teste. Os testes ficam ao lado do código (`split.ts`, `split.test.ts`). Rode a suíte antes de dar uma tarefa como concluída.

## Estrutura planejada

Pode ser ajustada na Tech Spec.

```
docs/        prd.md, techspec.md, review.md, tasks/
src/
  domain/    regras de negócio puras (pedido, teto, rateio)
  tools/     ferramentas do agente
  agent/     definição do agente e instruções
  rag/       ingestão e busca nas políticas
  data/      cardápio (JSON) e políticas (Markdown)
evals/       cenários de avaliação e script de execução
app/         tela de chat
```

## Próximo passo

Etapa 2: escrever a Tech Spec em `docs/techspec.md`, conduzida por perguntas ao autor.
