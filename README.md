# Sistema ETL para Gestão de Dados de uma Farmácia

> Projeto de integração, transformação e carregamento de dados (ETL) desenvolvido em Pentaho Data Integration, com foco na gestão de uma farmácia.

---

## Autor

**João Ricardo Costa Pereira - Aluno nº31505**

---

## Descrição do Projeto

Este projeto foi desenvolvido utilizando o Pentaho com o objetivo de integrar, transformar, analisar e carregar dados provenientes de diferentes fontes de informação relacionadas com a gestão de uma farmácia.

Foram utilizadas fontes de dados nos formatos JSON, CSV, XML, bem como uma API REST externa para obtenção de dados meteorológicos aplicando operações ETL (*Extract, Transform and Load*), validação de dados, integração de informação, geração de relatórios, carregamento para SQL Server e automatização através de um Job.

---

## Estrutura do Projeto

```text
TP01-31505
│
├── README.md
│
├── doc
│   ├── Sistema ETL - Integração e Análise de Dados de uma Farmácia.docx
│   ├── Sistema ETL - Integração e Análise de Dados de uma Farmácia.pdf
│   └── QR_Video.png
│
├── dataintegration
│   ├── JOB_01_Execucao_Completa.kjb
│   ├── TRF_01_Join_Medicamentos_Vendas.ktr
│   ├── TRF_02_Calculo_Totais.ktr
│   ├── TRF_03_Validacao_Emails.ktr
│   ├── TRF_04_Stock_Baixo.ktr
│   ├── TRF_05_Top_Medicamentos_Vendidos.ktr
│   ├── TRF_06_Join_Fornecedores_Medicamentos.ktr
│   ├── TRF_07_Relatorio_Final_Vendas.ktr
│   ├── TRF_08_Qualidade_Dados.ktr
│   ├── TRF_09_Analise_Faturacao.ktr
│   ├── TRF_10_Analise_Clientes.ktr
│   ├── TRF_11_Analise_Stock.ktr
│   ├── TRF_12_Indicadores_Gerais.ktr
│   ├── TRF_13_Conversao_Formatos.ktr
│   ├── TRF_14_API_Meteorologia.ktr
│   └── TRF_15_Log_Execucao.ktr
│
└── data
    ├── input
    │   ├── clientes.json
    │   ├── vendas.json
    │   ├── medicamentos.csv
    │   └── fornecedores.xml
    │
    └── output
        ├── relatorio_vendas.csv
        ├── clientes_validos.csv
        ├── clientes_invalidos.csv
        ├── faturacao_por_categoria.csv
        ├── top_clientes.csv
        ├── analise_stock.csv
        ├── indicadores_gerais.csv
        ├── clientes_convertidos.xml
        ├── meteorologia.csv
        └── etl_log.csv
```

---

## Fontes de Dados Utilizadas

| Formato | Ficheiros |
|---------|-----------|
| JSON | `clientes.json`, `vendas.json` |
| CSV | `medicamentos.csv` |
| XML | `fornecedores.xml` |
| API REST (JSON) | `Open-Meteo API` |

---

## Transformações Desenvolvidas

| Transformação | Descrição |
|---------------|-----------|
| `TRF_01_Join_Medicamentos_Vendas` | Integra os dados das vendas com a informação dos medicamentos através do componente Stream Lookup. |
| `TRF_02_Calculo_Totais` | Calcula o valor total das vendas com base no preço do medicamento e quantidade vendida. |
| `TRF_03_Validacao_Emails` | Valida os emails dos clientes utilizando expressões regulares (Regex). |
| `TRF_04_Stock_Baixo` | Identifica medicamentos com stock abaixo do limite definido. |
| `TRF_05_Top_Medicamentos_Vendidos` | Apresenta os medicamentos mais vendidos através de operações de agregação e ordenação. |
| `TRF_06_Join_Fornecedores_Medicamentos` | Integra os dados dos fornecedores com os medicamentos utilizando ficheiros XML e CSV. |
| `TRF_07_Relatorio_Final_Vendas` | Transformação principal responsável pela integração dos dados, geração do relatório final, exportação para CSV e carregamento para SQL Server. |
| `TRF_08_Qualidade_Dados` | Normaliza e valida emails dos clientes, separando registos válidos e inválidos. |
| `TRF_09_Analise_Faturacao` | Calcula a faturação total por categoria de medicamento. |
| `TRF_10_Analise_Clientes` | Identifica os clientes com maior volume de compras. |
| `TRF_11_Analise_Stock` | Classifica os níveis de stock dos medicamentos em categorias de risco. |
| `TRF_12_Indicadores_Gerais` | Gera indicadores globais de negócio, incluindo faturação total, quantidade vendida e número de vendas. |
| `TRF_13_Conversao_Formatos` | Converte os dados dos clientes do formato JSON para XML. |
| `TRF_14_API_Meteorologia` | Consome uma API REST externa (Open-Meteo), obtendo dados meteorológicos em tempo real e exportando os resultados para CSV. |
| `TRF_15_Log_Execucao` | Gera um ficheiro de log contendo a data e hora da execução do processo, a identificação do Job executado e o respetivo estado. |

---

## JOB_01_Execucao_Completa

O Job automatiza a execução das principais transformações do projeto, garantindo a produção dos relatórios finais, o carregamento dos dados para SQL Server, a geração do ficheiro de log e o envio automático dos resultados por email.

### Fluxo de Execução

```text
TRF_08_Qualidade_Dados
        ↓
TRF_09_Analise_Faturacao
        ↓
TRF_10_Analise_Clientes
        ↓
TRF_11_Analise_Stock
        ↓
TRF_12_Indicadores_Gerais
        ↓
TRF_13_Conversao_Formatos
        ↓
TRF_14_API_Meteorologia
        ↓
TRF_07_Relatorio_Final_Vendas
        ↓
TRF_15_Log_Execucao
        ↓
Mail


Etapas do Processo:
1. Qualidade dos Dados: valida e separa os emails dos clientes.
2. Análise de Faturação: calcula a faturação por categoria de medicamento.
3. Análise de Clientes: identifica os clientes com maior volume de compras.
4. Análise de Stock: classifica os níveis de stock dos medicamentos.
5. Indicadores Gerais: calcula indicadores globais do negócio.
6. Conversão de Formatos: converte os dados dos clientes de JSON para XML.
7. API de Meteorologia: obtém dados meteorológicos através da API Open-Meteo.
8. Relatório Final de Vendas: gera o relatório CSV e carrega os dados para SQL Server.
9. Registo da Execução: gera o ficheiro de log do processo.
10. Envio de Email: envia automaticamente os resultados produzidos.
```

---

## Resultados Produzidos

O processo ETL gera os seguintes ficheiros na pasta `data/output`:

- `relatorio_vendas.csv`
- `clientes_validos.csv`
- `clientes_invalidos.csv`
- `faturacao_por_categoria.csv`
- `top_clientes.csv`
- `analise_stock.csv`
- `indicadores_gerais.csv`
- `clientes_convertidos.xml`
- `meteorologia.csv`
- `etl_log.csv`


---

## Base de Dados

Os dados do relatório final de vendas são carregados para SQL Server na tabela:

`vendas_processadas`


---
 
## Base de Dados
 
Os dados do relatório final de vendas são carregados para SQL Server na tabela:
 
`vendas_processadas`
 
Para executar o projeto, é necessário criar previamente a tabela na base de dados SQL Server, executando o seguinte script:
 
```sql
CREATE TABLE vendas_processadas
(
    id_venda INT,
    cliente_id INT,
    medicamento_id INT,
    quantidade INT,
    nome_cliente NVARCHAR(100),
    email_cliente NVARCHAR(150),
    nome_medicamento NVARCHAR(100),
    categoria NVARCHAR(100),
    preco DECIMAL(10,2),
    fornecedor_id INT,
    nome_fornecedor NVARCHAR(100),
    telefone_fornecedor NVARCHAR(20),
    total DECIMAL(10,2)
);
```
 

---

## Componentes Pentaho

`JSON Input`, `Text File Input`, `Get Data From XML`, `REST Client`, `Stream Lookup`, `Regex Evaluation`, `String Operations`, `Filter Rows`, `Calculator`, `Group By`, `Sort Rows`, `Modified JavaScript Value`, `Generate Rows`, `Get System Info`, `Add Constants`, `Text File Output`, `XML Output`, `Table Output`, `Job` e `Mail`.


---

## Vídeo Demonstrativo

O acesso ao vídeo demonstrativo encontra-se disponível através do QR Code presente em:

`doc/QR_Video.png`

Ou através do link:

`https://www.youtube.com/watch?v=AwGGbFY69Fc`


---

## Observações

- Todos os dados de entrada encontram-se em: `data/input`
- Todos os resultados produzidos pelo processo ETL encontram-se em: `data/output`
- Repositório Git: `https://github.com/ricardopereira05/TP01-31505-ETL-Farmacia`

---


Projeto desenvolvido no âmbito da unidade curricular de Integração de Sistemas de Informação
