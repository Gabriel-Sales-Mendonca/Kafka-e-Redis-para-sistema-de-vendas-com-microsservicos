# Kafka e Redis em um Sistema de Vendas com Microsserviços

Utilizando Kafka para Mensageria e Redis para Cache de um sistema de vendas com o objetivo de colocar em prática conhecimento sobre mensageria, banco de dados para Cache e Microsserviços com Java e Spring Boot

O projeto está organizado como um Monorepo, facilitando a navegação e a compreensão da estrutura dos diferentes Microsserviços que o compõem.

## System Design do projeto 💡

<img width="1358" height="781" alt="Captura de tela 2025-08-28 162443" src="https://github.com/user-attachments/assets/43dee16d-97c4-487e-94bc-2ec1fefda2a4" />

## Fluxo de Funcionamento 🚀
O sistema processa pedidos de vendas através de um fluxo assíncrono, garantindo escalabilidade e desacoplamento entre os serviços.

## Fluxo de criação do resumo do pedido:
**1** - API REST recebe uma requisição com o carrinho de compras e o CEP do cliente.

**2** - A API publica uma mensagem no **tópico do Kafka** para iniciar o processamento do frete.

**3** - O Microsserviço de Frete consome a mensagem, calcula o frete (integrando-se a uma API externa de terceiros) e salva o resumo completo do pedido no banco de dados **PostgreSQL**.

## Fluxo de criação do pedido final:
**1** - A API REST recebe uma nova requisição para finalizar o pedido, referenciando o resumo salvo anteriormente.

**2** - A API publica outra mensagem no **tópico do Kafka** para que o pedido seja criado.

**3** - O Microsserviço de Pedido consome a mensagem, aplica as regras de negócio, atualiza o estoque e gera a fatura.

## O Papel do Redis 🧠
O Redis é utilizado como uma camada de cache para otimizar o desempenho das consultas de catálogo de produtos, reduzindo a carga sobre o banco de dados principal e acelerando o tempo de resposta para os usuários.

## Ponto de Vista do Usuário 👨‍💻
**1** - O usuário navega e adiciona produtos ao carrinho de compras.

**2** - Para ver o valor total, o usuário informa o CEP. O sistema calcula o custo do frete e exibe o resumo da compra (preço dos produtos + frete).

**3** - Após revisar o resumo, o usuário finaliza a compra.

**4** - O sistema processa o pedido e o usuário pode visualizá-lo em uma tela de "meus pedidos".
