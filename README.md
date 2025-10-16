# Projeto ETL - Importação e Extração de Dados Fictícios

Este projeto demonstra um pipeline de **ETL (Extract, Transform, Load)** simples utilizando a biblioteca `pandas` em Python. O objetivo é simular a extração de dados de múltiplas fontes (vendas, produtos e clientes), aplicar transformações e regras de qualidade (limpeza e padronização) e, finalmente, integrar e carregar os dados em um Data Mart consolidado para análise.

## 🚀 Tecnologias Utilizadas

* **Python**
* **Pandas** (Manipulação e Transformação de Dados)
* **NumPy** (Uso para valores nulos, `np.nan`)

## 📋 Estrutura do Projeto

O código executa as seguintes etapas:

### 1. Extração (E)
Simula a importação de dados de três fontes diferentes, cada uma representada por um `DataFrame` do `pandas` com dados fictícios:

* **`df_vendas`**: Detalhes das transações (ID da transação, SKU, Cliente, Data, Quantidade, Receita).
* **`df_produtos`**: Informações sobre os produtos (SKU, Nome, Categoria, Custo).
* **`df_clientes`**: Dados dos clientes (ID, Nome, Cidade).

### 2. Transformação (T)
Aplica diversas regras de negócio e qualidade nos dados para prepará-los para a integração:

* **Padronização de Chaves:** A coluna `product_sku` em `df_vendas` é padronizada para maiúsculas (`.str.upper()`) e as chaves de ligação (`SKU` em `df_produtos` e `ID` em `df_clientes`) são renomeadas para facilitar o `merge`.
* **Tratamento de Nulos:** Valores nulos (`NaN`) na coluna `category` de produtos são preenchidos com 'Desconhecida'.
* **Padronização de Dados:** A coluna `city` (cidade) em `df_clientes` é padronizada para o formato Título (`.str.title()`).
* **Integração (`JOIN`):** Os DataFrames são unidos usando `pd.merge` (Vendas + Produtos, e o resultado + Clientes) para criar o `df_datamart` unificado.
* **Modelagem/Feature Engineering:** Criação de colunas de métricas de negócio essenciais:
    * `total_cost`: Custo total da transação (`cost * quantity`).
    * `profit`: Lucro da transação (`revenue - total_cost`).

### 3. Carga (L)
O Data Mart final (filtrado e ordenado) é persistido em disco em um formato otimizado para sistemas de Big Data e análise.

* **Formato de Saída:** O `DataFrame` final é salvo como **`ecommerce_data_mart.parquet`**.

## 📊 Data Mart Final

O Data Mart unificado é composto pelas seguintes colunas, prontas para consumo:

| Coluna | Tipo | Origem | Finalidade |
| :--- | :--- | :--- | :--- |
| `date` | `object` | Vendas | Chave Temporal para Análise de Séries |
| `transaction_id` | `int64` | Vendas | Identificador Único da Transação |
| `customer_id` | `object` | Vendas/Clientes | Chave de Ligação |
| `product_sku` | `object` | Vendas/Produtos | Chave de Ligação |
| `name_x` | `object` | Produtos | Nome do Produto (após merge) |
| `category` | `object` | Produtos | Variável de Agrupamento Principal |
| `quantity` | `int64` | Vendas | Quantidade Vendida |
| `revenue` | `float64` | Vendas | Variável de Negócio (Receita) |
| `cost` | `float64` | Produtos | Custo Unitário do Produto |
| `total_cost` | `float64` | Transformação | Custo Total da Transação |
| **`profit`** | **`float64`** | **Transformação** | **KPI Principal (Lucro da Transação)** |
| `city` | `object` | Clientes | Variável Geográfica |

## 🛠️ Como Executar

1.  Certifique-se de ter o Python e as bibliotecas necessárias instaladas:
    ```bash
    pip install pandas numpy pyarrow
    ```
2.  Execute o script Python que contém o pipeline ETL.

Após a execução, o arquivo `ecommerce_data_mart.parquet` será gerado no diretório raiz do projeto.

## 👨‍💻 Criado por

**Paloma Cordeiro**

* **LinkedIn:** [\[LinkedIn\]](br.linkedin.com/in/paloma-cordeiro-119750b6)
* **GitHub:** [\[GitHub\]](https://github.com/palomacdev)

**Repositório:** `Projeto_ETL_Ecommerce_Data_Engineer` 

