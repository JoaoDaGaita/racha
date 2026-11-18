# Racha

Agente de IA que organiza o pedido de almoço de uma equipe: entende o que cada pessoa quer, controla o teto de gasto individual, monta uma sacola única e calcula quanto cada um paga, com taxa de entrega e cupom rateados.

> Projeto de estudo independente, sem vínculo com o iFood. O restaurante e o cardápio são fictícios.

## Status

Em planejamento. O projeto segue Spec-Driven Development: cada etapa gera um documento antes do código.

| Etapa | Documento | Situação |
|---|---|---|
| Requisitos de produto | [docs/prd.md](docs/prd.md) | Concluído |
| Especificação técnica | `docs/techspec.md` | A fazer |
| Tarefas | `docs/tasks/` | A fazer |
| Implementação | `src/` | A fazer |
| Testes e avaliações | `evals/` | A fazer |
| Code review | `docs/review.md` | A fazer |

## O problema

Quem organiza o almoço da equipe junta pedidos espalhados em mensagens, confere se cada um respeitou a verba e calcula à mão quanto cada pessoa deve. O Racha automatiza a coleta, o controle do teto e, principalmente, o rateio.

Os detalhes estão no [PRD](docs/prd.md): usuário, cenários de uso, regras de negócio, métricas e o que fica fora do escopo.

## Princípio de arquitetura

O modelo de linguagem interpreta e conversa. Todo cálculo (teto, rateio, arredondamento) é feito por código determinístico e testado.

## Stack planejada

TypeScript, Node.js, Mastra, Claude (API da Anthropic), Zod, Vitest e Next.js.

## Autor

Joao Vicente Silva
