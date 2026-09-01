# Dá pra saber que vai atrasar antes de sair?

Modelo que prevê atraso de entrega no e-commerce brasileiro usando apenas o que se sabe no momento da compra. 96.470 pedidos, dados públicos da Olist (2016-2018).

## A pergunta

No [projeto anterior](https://github.com/KevinCosta-AI/nordeste-espera-mais) descobri que a entrega para o Nordeste não é só mais lenta: é imprevisível. Se é imprevisível, dá pra prever mesmo assim?

## O que fiz

Treinei dois modelos para responder, no instante da compra, se aquele pedido vai chegar depois da data prometida. Usei só informação disponível naquele momento: estado do cliente e do vendedor, preço, frete, peso, categoria, prazo prometido e congestionamento da semana.

Separei treino e teste **por tempo**, não por sorteio: treino no passado, teste no futuro. Sortear teria deixado o modelo ver o futuro.

## O que encontrei

**Acurácia não serve aqui.** Só 6,8% dos pedidos atrasam, então um modelo que diz "nunca atrasa" acerta 93,2% e nunca avisa ninguém.

**A regressão logística ganhou do gradient boosting** - 0,740 contra 0,646 de AUC. Não era o esperado. A taxa de atraso caiu pela metade entre o período de treino e o de teste, e o modelo mais simples resistiu melhor à mudança.

**O modelo separou o Brasil por região sozinho.** Os 9 estados do Nordeste e 4 dos 7 do Norte puxam o risco pra cima; São Paulo, Minas, Paraná e o Distrito Federal puxam pra baixo. Ele nunca recebeu mapa, distância nem a palavra "região" - só siglas de duas letras.

**Entre o décimo mais seguro e o mais arriscado, a taxa real de atraso vai de 0,2% a 8,0%.** Quarenta vezes maior. Avisando os 30% mais arriscados, pegaria 64% de todos os atrasos.

## Limitação

Com AUC 0,740 e precisão perto de 8%, a maioria dos avisos seria alarme falso. Serve para priorizar operação, não para prometer prazo diferente ao cliente.

## Como rodar

Abra o notebook no Google Colab e execute as células em ordem. Os dados são baixados pelo próprio notebook.

## Ferramentas

Python · pandas · scikit-learn · matplotlib · Google Colab

---

Kevin Moreira da Costa · [LinkedIn](https://www.linkedin.com/in/kevincosta-ai) · kevincosta-ai@outlook.com
