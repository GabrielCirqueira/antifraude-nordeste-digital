# Antifraude Nordeste Digital — protótipo mobile

Protótipo clicável do aplicativo interno de análise e controle de pedidos suspeitos da **Nordeste Digital**, desenvolvido para o PBL do curso de Análise e Desenvolvimento de Sistemas (4º período, 2026/2) do Centro Universitário Vale do Cricaré (UNIVC).

**Caso:** "Três marketplaces, uma API instável e o pedido fraudulento que passou".
**Grupo:** ADS4techgroup.

## Como executar

Não precisa instalar nada. Abra o arquivo `index.html` em qualquer navegador moderno (a internet só é usada para carregar as fontes do Google Fonts).

No computador, o app aparece dentro de uma moldura de celular, com um guia de telas à esquerda. No celular, ocupa a tela inteira.

## O que o protótipo mostra

Telas do Guia do Aplicativo:

1. **Painel inicial:** fila de pedidos que precisam de análise, com número, valor, canal, nível de risco, situação e prazo. Tem busca, filtros e ordenação.
2. **Detalhes do pedido:** medidor de risco de 0 a 100, sinais que geraram o alerta (com pesos), dados do pedido, comparação de endereços e linha do tempo.
3. **Decisão:** liberar, manter retido ou rejeitar, com justificativa obrigatória, motivo para rejeição e confirmação antes de salvar.
4. **Histórico:** decisões já registradas (quem, quando, decisão e justificativa), com registro de auditoria em JSON.

Telas complementares ligadas ao relatório:

- **Integração:** parceiros A, B e C com os parâmetros de resiliência (timeout, retentativa com jitter, circuit breaker, bulkhead e rate limit). O botão "Simular falha" mostra o circuito abrindo só no parceiro afetado, o enfileiramento de pedidos e o reprocessamento com a mesma chave de idempotência.
- **Regras:** simulador de pontuação do motor antifraude (sinais, pesos e faixas de decisão), ajuste do peso de cada sinal pela empresa (com botão para restaurar os pesos do relatório), custo assimétrico do erro e resumo de conformidade com a LGPD.

## Regras do motor antifraude (do relatório)

| Sinal | Peso |
|---|---|
| Primeira compra com valor acima de R$ 800 | +40 |
| Endereço de entrega diferente do cadastro | +25 |
| Muitas compras em curto período | +20 |
| Cartão diferente dos usados anteriormente | +15 |
| Valor acima de R$ 800 | +10 |
| Horário fora do padrão histórico do cliente | +5 |

Fonte: Quadro 3 do relatório. A pontuação é a soma dos pesos, limitada a 100.

| Pontuação | Faixa |
|---|---|
| 0 a 29 | Aprovação automática |
| 30 a 59 | Revisão humana |
| 60 ou mais | Bloqueio com revisão humana obrigatória |

Não há bloqueio definitivo automático: a decisão final é sempre humana (LGPD, art. 20).

## Limitações

- É um protótipo: não há back-end, API ou banco de dados reais. O motor antifraude, a fila e a integração com os parceiros são simulados no navegador.
- As decisões registradas ficam salvas apenas no navegador de quem usa (`localStorage`).
- Todos os pedidos, nomes, valores e endereços são fictícios.
- Os prazos de decisão recomeçam a cada vez que a página é aberta.

## Tecnologias

HTML, CSS e JavaScript puros, em um único arquivo (`index.html`).
