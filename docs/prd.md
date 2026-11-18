# PRD — Racha

**Agente de IA para pedidos de comida em grupo**

| | |
|---|---|
| Autor | Joao Vicente Silva |
| Status | Rascunho para revisão |
| Versão | 0.1 |
| Data | 17/11/2026 |

---

## 1. Resumo

O Racha é um agente de IA que organiza o pedido de almoço de uma equipe. O organizador informa, em linguagem natural, o que cada colega quer; o agente encontra os itens no cardápio, controla o teto de gasto por pessoa, monta uma sacola única e calcula quanto cada um paga, com taxa de entrega e cupom rateados.

## 2. Contexto e oportunidade

O iFood já oferece o Ailo, um assistente de IA que monta e conclui pedidos por conversa. Pelo que foi divulgado publicamente até novembro de 2026, o Ailo é centrado em um consumidor por vez, e o recurso de divisão de conta em teste se limita a combinar duas formas de pagamento no mesmo pedido.

Não foi encontrada, em fontes públicas, uma solução que colete o pedido de várias pessoas, aplique um teto individual e divida a conta por consumo. O Racha explora essa lacuna.

> Esta leitura se baseia em notícias e comunicados, não em acesso interno ao produto. O Racha é um projeto de estudo independente, sem vínculo com o iFood.

## 3. Usuário principal

**Organizador do almoço de uma equipe de trabalho.**

- Reúne os pedidos de 5 a 15 colegas.
- Trabalha com um teto por pessoa (vale-refeição ou verba da empresa).
- Repete o processo toda semana.

Os colegas são participantes do pedido, mas na versão 1 não interagem com o agente.

## 4. Problema

Quem organiza o almoço da equipe gasta tempo juntando pedidos espalhados em mensagens, conferindo se cada um respeitou o teto e calculando quanto cada pessoa deve. O processo é manual, sujeito a erro e termina com alguém pagando a mais ou cobrando colegas depois.

**Dores, em ordem de prioridade**

| Prioridade | Dor | Tratamento no Racha |
|---|---|---|
| Principal | **Ratear** a conta com taxa e cupom de forma justa | Cálculo determinístico, exato no centavo |
| Secundária | **Coletar** pedidos soltos, esquecidos ou alterados | Registro por pessoa, resumo com pendentes |
| Secundária | **Controlar o teto** de cada pessoa | Aviso automático com sugestão de alternativa |
| Fora do escopo | **Cobrar** cada colega depois | Não tratado (envolve pagamento real) |

## 5. Objetivos

1. Entregar o valor exato que cada pessoa deve, sem conta manual.
2. Reduzir o esforço do organizador para consolidar o pedido.
3. Garantir que estouros de teto sejam sempre sinalizados.
4. Responder dúvidas sobre políticas do restaurante sem inventar informação.

## 6. Cenários de uso

### Essenciais

| # | Cenário | Exemplo | Comportamento esperado |
|---|---|---|---|
| C1 | Abrir o pedido | "Almoço da equipe no TchanTchio, teto de R$ 40 por pessoa" | Cria o pedido com restaurante e teto |
| C2 | Adicionar item | "Ana quer parmegiana e suco" | Encontra os itens no cardápio e registra para a Ana |
| C3 | Estourar o teto | Item leva a pessoa acima da verba | Avisa, informa o excedente e sugere alternativa |
| C4 | Item inexistente ou ambíguo | "Bruno quer o frango" (há três pratos de frango) | Pergunta qual, em vez de escolher sozinho |
| C5 | Alterar ou remover | "Bruno trocou o executivo pela salada" | Atualiza o pedido do Bruno |
| C6 | Ver o resumo | "Como está o pedido?" | Mostra a sacola consolidada e quem ainda não pediu |
| C7 | Fechar e ratear | "Fecha o pedido" | Mostra o valor de cada um, com taxa e cupom divididos |

### Complementares

| # | Cenário | Exemplo | Comportamento esperado |
|---|---|---|---|
| C8 | Dúvida de política | "Qual o pedido mínimo?" | Responde com base nos documentos do restaurante |
| C9 | Pergunta sem resposta | Pergunta que os documentos não cobrem | Diz que não sabe; não inventa |
| C10 | Item compartilhado | "A porção de fritas é para Ana, Bruno e Carla" | Divide o item igualmente entre os três |

## 7. Regras de negócio

| # | Regra |
|---|---|
| R1 | **Teto: avisar, não bloquear.** O agente alerta e sugere alternativa; o organizador pode liberar. |
| R2 | **O teto considera só os itens da pessoa.** Taxa de entrega e cupom ficam fora dessa conta. |
| R3 | **Taxa de entrega dividida igualmente** entre quem pediu. |
| R4 | **Cupom dividido proporcionalmente ao consumo** de cada pessoa. |
| R5 | **Item compartilhado dividido igualmente** entre as pessoas indicadas; a parte de cada uma conta para o teto dela. |
| R6 | **A soma fecha no centavo.** Centavos que sobram da divisão vão para o organizador. |
| R7 | **Pedido mínimo:** o agente avisa no fechamento se o total não atingiu, mas não impede. |
| R8 | **Pedido fechado não muda.** Para alterar, o organizador precisa reabrir. |

**Princípio:** todo cálculo (teto, rateio, arredondamento) é feito por código determinístico e testado. O modelo de linguagem interpreta e conversa; nunca faz contas.

### Exemplo de rateio

Pedido com três pessoas, taxa de entrega de R$ 9,00 e cupom de R$ 10,00:

| Pessoa | Itens | Taxa (R3) | Cupom (R4) | A pagar |
|---|---|---|---|---|
| Ana | R$ 40,00 | R$ 3,00 | − R$ 4,00 | R$ 39,00 |
| Bruno | R$ 30,00 | R$ 3,00 | − R$ 3,00 | R$ 30,00 |
| Carla | R$ 30,00 | R$ 3,00 | − R$ 3,00 | R$ 30,00 |
| **Total** | **R$ 100,00** | **R$ 9,00** | **− R$ 10,00** | **R$ 99,00** |

## 8. Fora do escopo

1. Pagamento e cobrança.
2. Integração real com o iFood (restaurante e cardápio são fictícios).
3. Login e contas (participantes identificados só pelo nome).
4. Vários restaurantes no mesmo pedido.
5. Acompanhamento da entrega.
6. Voz e WhatsApp (só texto, em tela própria).
7. Cada pessoa com o próprio link de acesso.
8. Histórico e pedidos recorrentes.

## 9. Métricas de sucesso

Como não há usuários reais, as métricas são medidas por avaliações automáticas sobre os cenários da seção 6.

| # | Métrica | Meta |
|---|---|---|
| M1 | Rateio correto (soma das partes igual ao total, no centavo) | 100% |
| M2 | Atribuição correta (item certo para a pessoa certa) | ≥ 90% |
| M3 | Teto sinalizado (todo estouro gera aviso) | 100% |
| M4 | Ambiguidade tratada (pergunta em vez de escolher) | ≥ 90% |
| M5 | Recusa adequada (sem resposta nos documentos → "não sei") | 100% |
| M6 | Eficiência (pedido de 5 pessoas fechado) | ≤ 8 mensagens do organizador |

As metas de 90% são um ponto de partida; o valor real sai da primeira rodada de avaliações e a evolução fica registrada no README.

## 10. Evolução futura

Em ordem de valor:

1. **Pedidos recorrentes:** lembrar o último pedido de cada pessoa e oferecer repetir. É o passo mais valioso para uma equipe que pede toda semana.
2. **Link por participante:** cada colega entra no pedido e fala direto com o agente.
3. **Regras de rateio configuráveis:** o organizador escolhe entre divisão igual e proporcional.
4. **Cobrança:** gerar o valor e a chave de pagamento para cada pessoa.

## 11. Premissas e questões em aberto

**Premissas**

- O restaurante fictício é o "TchanTchio", com cardápio e políticas em arquivos locais.
- Só o organizador conversa com o agente (versão 1).
- O modelo de linguagem é o Claude, via API da Anthropic; o modelo específico é definido na Tech Spec.

**Em aberto**

- Definir o tipo de cozinha e o cardápio do TchanTchio.

## 12. Referências

- [Mobile Time — Ailo conversacional no WhatsApp](https://www.mobiletime.com.br/noticias/16/07/2026/ailo-ifood-whatsapp-flui/)
- [Consumidor Moderno — iFood transforma o WhatsApp em canal de compras com o Ailo](https://consumidormoderno.com.br/ifood-whatsapp-canal-compras-agente-ailo/)
- [Central do Varejo — pagamento agêntico no Ailo](https://centraldovarejo.com.br/ifood-adota-pagamento-agentico-em-assistente-de-ia-no-whatsapp/)
- [Portal Tela — iFood testa dividir pagamento](https://www.portaltela.com/noticias/geral/2026/06/04/ifood-testa-recurso-de-dividir-pagamento-com-outra-pessoa/)
