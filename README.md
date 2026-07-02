# ☕ Café Sales Pipeline — Databricks + Unity Catalog

Pipeline de dados end-to-end usando arquitetura medalhão (Bronze → Silver → Gold) 
para processar e analisar dados de vendas de uma cafeteria fictícia.

## 🎯 Objetivo

Projeto de estudo/portfólio demonstrando um pipeline completo de engenharia de dados:
ingestão, tratamento, agregação e visualização, usando Databricks e Unity Catalog.

## 🏗️ Arquitetura
📁 Volume (CSV bruto)
↓
🥉 bronze.dirty_cafe_sales      → dados brutos, como vieram da fonte
↓
🥈 silver.clean_cafe_sales      → dados tratados, 100% das linhas preservadas
↓
🥇 gold.sales_by_item            → agregação por item
🥇 gold.sales_by_location        → agregação por localização
🥇 gold.sales_by_payment_method  → agregação por método de pagamento
🥇 gold.sales_by_date            → série temporal de receita

## 📊 Estrutura do Catalog

| Camada | Schema | Objetos |
|---|---|---|
| Raw | `bronze` | Volume (arquivo CSV) + Tabela `dirty_cafe_sales` |
| Tratado | `silver` | Tabela `clean_cafe_sales` |
| Agregado | `gold` | 4 tabelas de métricas de negócio |

## 🧹 Estratégia de tratamento de dados

O dataset bruto continha inconsistências como valores `UNKNOWN`, `ERROR` e nulos.
A decisão de design foi **preservar 100% das linhas originais**, aplicando:

- **Recálculo matemático**: `Quantity`, `Price_Per_Unit` e `Total_Spent` têm relação 
  entre si (`Total = Quantity × Price`), então valores ausentes foram recalculados 
  a partir das outras colunas quando possível, usando `try_cast`.
- **Marcação como "Não Informado"**: campos sem forma de recuperação matemática 
  (`Item`, `Payment_Method`, `Location`, `Transaction_Date`) foram marcados como 
  `"Não Informado"` ao invés de descartar a linha inteira — evitando perda de até 
  ~40% dos dados que ocorreria com uma abordagem de remoção direta.

## 📈 Resultados (Dashboard)

![Dashboard](caminho/para/print-dashboard.png)

Principais insights:
- **Salad** foi o item com maior receita total, apesar de não ter o maior volume de vendas
- **Coffee** teve o maior volume de vendas, mas ticket médio mais baixo
- Distribuição de receita relativamente equilibrada entre os métodos de pagamento
- Receita mensal estável ao longo do período analisado

## 🛠️ Tecnologias

- Databricks (Serverless SQL Warehouse)
- Unity Catalog
- PySpark
- Databricks Dashboards (Lakeview)
- Git / GitHub (versionamento)

## 📁 Estrutura do repositório
databricks-cafe-sales-pipeline/
├── README.md
├── .gitignore
└── notebooks/
├── 02_ingest_bronze.py
├── 03_transform_silver.py
└── 04_aggregate_gold.py

## 👤 Autor

Marcos Willian

