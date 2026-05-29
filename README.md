# Pipeline de Limpeza de Dados - Olist

## Sobre o projeto

Este projeto foi desenvolvido no Mini-Projeto Avaliativo do Módulo 1, Semana 07, do curso de Machine Learning e Visão Computacional.

A proposta do projeto é simular uma situação real de trabalho na área de dados. A equipe da Olist possui arquivos CSV com informações de produtos e pedidos, mas esses dados apresentam inconsistências, como campos vazios, textos fora de padrão, dimensões físicas incompletas e datas que precisam ser analisadas.

Meu objetivo foi construir um pipeline de sanitização usando Python nativo, sem utilizar Pandas. A ideia principal foi praticar lógica de programação, leitura de arquivos CSV, tratamento de dados ausentes, padronização de strings, uso de Regex, validação de regras de negócio e formatação de datas.

## Base de dados

A base utilizada é composta por arquivos CSV da Olist.

No desenvolvimento do projeto, trabalhei com dois arquivos principais:

- `data information product.csv`: arquivo com informações dos produtos;
- `data request product.csv`: arquivo com informações dos pedidos.

Esses arquivos correspondem aos dados de produtos e pedidos usados no desafio da Olist.

## Tecnologias e bibliotecas utilizadas

O projeto foi desenvolvido em Python, utilizando apenas bibliotecas nativas:

- `csv`: para leitura dos arquivos CSV com `DictReader`;
- `re`: para limpeza e padronização de textos usando Expressões Regulares;
- `datetime`: para conversão e formatação das datas de aprovação dos pedidos.

A biblioteca Pandas não foi utilizada, pois a proposta do projeto era trabalhar com lógica estruturada em Python puro.

## O que o script faz

O script realiza as seguintes etapas:

1. Lê os arquivos CSV de produtos e pedidos.
2. Armazena os dados em listas de dicionários.
3. Trata valores vazios na coluna `product_category_name`.
4. Padroniza os nomes das categorias com `.strip()`, `.lower()` e Regex.
5. Remove caracteres especiais das categorias dos produtos.
6. Trata valores vazios nas dimensões físicas dos produtos.
7. Substitui dimensões vazias pela média da própria coluna.
8. Analisa pedidos com data de entrega vazia.
9. Verifica se pedidos sem data de entrega possuem status `canceled`.
10. Formata a coluna `order_approved_at` para o padrão brasileiro.
11. Exibe um relatório final no terminal com os principais resultados do processamento.

## Tratamento da coluna de categorias

A coluna `product_category_name` foi tratada para evitar categorias vazias ou fora de padrão.

Quando a categoria estava vazia, o valor foi substituído por `Sem_categoria`. Depois, os textos foram padronizados para ficarem em letras minúsculas, sem espaços extras e sem caracteres especiais.

Essa etapa é importante porque categorias escritas de formas diferentes podem prejudicar relatórios, filtros e futuras análises.

## Tratamento das dimensões físicas

As colunas de dimensões físicas analisadas foram:

- `product_weight_g`;
- `product_length_cm`;
- `product_height_cm`;
- `product_width_cm`.

Para tratar valores vazios nessas colunas, escolhi substituir os campos ausentes pela média da própria coluna.

Essa escolha evita descartar produtos do dataset e também impede o uso de valores irreais, como zero. A média representa um valor aproximado do padrão dos produtos cadastrados. Em um contexto logístico, como cálculo de frete, armazenamento ou escolha do veículo de transporte, conhecer as dimensões médias ajuda a estimar se os produtos tendem a ser pequenos, médios ou grandes.

Por exemplo, uma loja de joias normalmente trabalha com produtos pequenos, enquanto outros segmentos podem trabalhar com itens maiores. Por isso, a média pode ajudar a manter uma estimativa coerente quando algum dado dimensional está faltando.

## Análise dos pedidos

Na parte de pedidos, o script analisa principalmente as colunas:

- `order_status`;
- `order_delivered_customer_date`;
- `order_approved_at`.

A regra de negócio analisada foi a seguinte:

> Quando a data de entrega ao cliente está vazia, o pedido está obrigatoriamente com status `canceled`?

Para responder isso, o código percorre os pedidos, conta os pedidos cancelados, identifica os pedidos sem data de entrega e verifica quantos desses realmente possuem status `canceled`.

Também são guardados alguns exemplos de pedidos que possuem data de entrega vazia, mas status diferente de `canceled`, caso existam.

## Formatação de datas

A coluna `order_approved_at` originalmente vem em um formato parecido com:

```text
2017-05-16 15:05:35
```

O script converte essa data para o formato brasileiro simplificado:

```text
16/05/2017
```

Essa conversão foi feita usando a biblioteca nativa `datetime`.

## Relatório final

Ao final da execução, o script exibe no terminal um relatório final com informações como:

- total de produtos processados;
- quantidade de produtos sem categoria corrigidos;
- colunas dimensionais tratadas;
- total de pedidos processados;
- total de pedidos cancelados;
- total de pedidos sem data de entrega;
- total de pedidos sem data e com status `canceled`;
- total de pedidos sem data e com status diferente de `canceled`;
- total de datas de aprovação vazias;
- total de datas de aprovação formatadas.

Esse relatório serve para validar se o pipeline executou as principais etapas de limpeza e análise dos dados.

## Como executar o projeto

Para executar o projeto, siga os passos abaixo:

1. Clone ou baixe este repositório.
2. Coloque os arquivos CSV na mesma pasta do código principal.
3. Verifique se os nomes dos arquivos estão iguais aos usados no código:
   - `data information product.csv`
   - `data request product.csv`
4. Abra o projeto em um editor, como VS Code ou Jupyter Notebook.
5. Execute o arquivo principal do projeto.
6. Confira o relatório final exibido no terminal.

## Estrutura do projeto

```text
projeto-olist/
│
├── main.py
├── README.md
├── data information product.csv
└── data request product.csv
```

## Reflexão sobre Machine Learning e qualidade dos dados

A limpeza de dados é uma etapa fundamental antes da criação de modelos de Machine Learning. Quando os dados estão incompletos, mal formatados ou inconsistentes, o modelo pode aprender padrões errados e gerar resultados ruins. Isso se conecta diretamente com a ideia de `Garbage In, Garbage Out`: se os dados de entrada forem ruins, a saída do sistema também será ruim.

No meu projeto, tratar categorias vazias, padronizar textos, corrigir dimensões físicas e validar datas ajuda a deixar a base mais confiável. Esse tipo de cuidado reduz o risco de viés, overfitting e underfitting em modelos futuros, porque o algoritmo passa a trabalhar com dados mais organizados e próximos da realidade.

## Autor

Desenvolvido por Miguel.

Projeto desenvolvido como parte do estudo de Machine Learning e Visão Computacional.
