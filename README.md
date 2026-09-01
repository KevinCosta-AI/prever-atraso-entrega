# Dá pra saber que vai atrasar antes de sair?

Modelo que prevê atraso de entrega no e-commerce brasileiro usando apenas o que se sabe **no momento da compra**. 96.470 pedidos, dados públicos da Olist (2016–2018).

## A pergunta

No [projeto anterior](https://github.com/KevinCosta-AI/nordeste-espera-mais) descobri que a entrega para o Nordeste não é só mais lenta: é imprevisível. Se é imprevisível, dá pra prever mesmo assim?

## O que fiz

Treinei dois modelos para responder, no instante da compra, se aquele pedido vai chegar depois da data prometida. Usei só informação disponível naquele momento: estado do cliente e do vendedor, preço, frete, peso, categoria, prazo prometido e o congestionamento dos 7 dias anteriores.

**Separei treino e teste por tempo, não por sorteio:** treino no passado, teste no futuro. Sortear teria deixado o modelo ver o futuro.

E revendo o notebook depois de pronto, achei um vazamento que tinha passado: uma variável contava *"quantos pedidos nesta semana"* — mas quem compra na quarta não sabe quantos pedidos virão no fim de semana. Troquei pelos **7 dias anteriores**. Custou 0,004 de AUC. Conto no fim.

## O que encontrei

**Acurácia não serve aqui.** Só 6,8% dos pedidos atrasam, então um modelo que diz "nunca atrasa" acerta 93,2% e nunca avisa ninguém.

**A regressão logística ganhou do gradient boosting** — 0,736 contra 0,636 de AUC. Não era o esperado. A taxa de atraso caiu pela metade entre o período de treino e o de teste, e o modelo mais simples resistiu melhor à mudança.

**O modelo separou o Brasil por região sozinho.** Os 9 estados do Nordeste e 4 dos 7 do Norte puxam o risco pra cima; São Paulo, Minas, Paraná e o Distrito Federal puxam pra baixo. Ele nunca recebeu mapa, distância nem a palavra "região" — só siglas de duas letras.

**Entre o décimo mais seguro e o mais arriscado, a taxa real de atraso vai de 0,2% a 7,6%.** Quase quarenta vezes maior. Avisando os 30% mais arriscados, pegaria 64% de todos os atrasos.

## Limitação

Com AUC 0,736 e precisão de 6,3%, a maioria dos avisos seria alarme falso. Serve para priorizar operação, não para prometer prazo diferente ao cliente.

## O vazamento que eu mesmo achei

A variável de congestionamento contava os pedidos da **semana inteira** do pedido. Parece inofensiva — não é uma data proibida, é uma contagem —, e por isso passou pela minha própria conferência coluna a coluna. Mas na hora da compra a semana ainda não acabou: metade daqueles pedidos ainda não existia.

Troquei por "pedidos nos **7 dias anteriores**" e rodei tudo de novo:

| | AUC | décimo mais arriscado | avisando 30% |
|---|---|---|---|
| com vazamento | 0,740 | 8,0% | pega 64% |
| **corrigido** | **0,736** | **7,6%** | **pega 64%** |

Quatro milésimos. O sinal era real, só estava sendo medido de um jeito impossível — o peso da variável nem se moveu (+0,48 nas duas versões).

## Como rodar

Abra o notebook no Google Colab e execute as células em ordem. Os dados são baixados pelo próprio notebook.

**Ferramentas:** Python · pandas · scikit-learn · matplotlib · Google Colab

---

## A trilogia

Três projetos sobre a mesma base, feitos em sequência. **Os três estão publicados.**

| Projeto | Pergunta | Resposta |
|---|---|---|
| [1 — O Nordeste espera mais?](https://github.com/KevinCosta-AI/nordeste-espera-mais) | quem espera mais? | o Nordeste — e não é distância, é imprevisibilidade |
| **2 — este** | dá pra prever? | dá, e o modelo redesenhou o mapa do Brasil sozinho |
| [3 — O que o cliente reclama?](https://github.com/KevinCosta-AI/o-que-o-cliente-reclama) | o que o cliente sente? | não sente atraso: sente que o pedido sumiu |

---

**Dados:** [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) · 2016–2018 · uso público

Kevin Moreira da Costa · [LinkedIn](https://www.linkedin.com/in/kevincosta-ai) · kevincosta-ai@outlook.com
