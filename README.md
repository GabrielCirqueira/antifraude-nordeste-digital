# Antifraude Nordeste Digital — protótipo mobile

Protótipo clicável do aplicativo interno de análise e controle de pedidos suspeitos da **Nordeste Digital**, desenvolvido para o PBL do curso de Análise e Desenvolvimento de Sistemas (4º período, 2026/2) do Centro Universitário Vale do Cricaré (UNIVC).

**Caso:** "Três marketplaces, uma API instável e o pedido fraudulento que passou".
**Grupo:** ADS4techgroup.

## Como executar

Não precisa instalar nada. Abra o arquivo `index.html` em qualquer navegador moderno (a internet só é usada para carregar as fontes do Google Fonts).

No computador, o app aparece dentro de uma moldura de celular, com um guia de telas à esquerda. No celular, ocupa a tela inteira.

## O que o protótipo mostra

O visual é um flat suave (fundo branco, cards com sombra leve, ícones gráficos e animações). Os dados são todos fictícios e gerados na hora.

Telas do Guia do Aplicativo:

1. **Painel inicial:** fila de revisão **ao vivo**. Novos pedidos chegam sozinhos, em intervalos variados, com nomes, produtos, endereços e cartões aleatórios. O motor calcula a pontuação a partir dos dados do pedido. Pedidos de baixo risco são aprovados automaticamente e os demais entram na fila com prazo de análise.
2. **Detalhes do pedido:** medidor de risco de 0 a 100, sinais ativados (com pesos), dados do pedido, comparação de endereços, comportamento do cliente e linha do tempo.
3. **Decisão:** liberar, manter retido ou rejeitar, com justificativa obrigatória, motivo para rejeição e confirmação antes de salvar. Gera protocolo de auditoria.
4. **Histórico:** decisões registradas (quem, quando, decisão e justificativa), com registro de auditoria em JSON.

Telas complementares ligadas ao relatório:

- **Parceiros:** os três marketplaces com timeout, tentativas, circuit breaker e bulkhead. Quando o circuito de um parceiro abre, os pedidos dele esperam na fila de reprocessamento e entram depois, sem duplicar.
- **Regras:** simulador de pontuação, peso de cada sinal, faixas de decisão e como cada sinal é detectado (valor considerado alto, histórico mínimo, janela de compras em sequência). Tudo editável.
- **Ajustes:** liga e desliga o fluxo de pedidos, define o intervalo entre pedidos e a chance de pedido suspeito, os prazos de análise, a instabilidade aleatória dos parceiros e restaura os dados.

Tudo que é configurado fica salvo no navegador (`localStorage`) e muda o resultado das análises na hora.

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
- Os pedidos retidos na fila de um parceiro com o circuito aberto ficam só na memória e se perdem ao recarregar a página.

## Tecnologias

HTML, CSS e JavaScript puros, em um único arquivo (`index.html`).
