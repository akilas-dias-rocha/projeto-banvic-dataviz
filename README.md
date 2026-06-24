# 🏦 Banco Vitória - Análise de Dados

[![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-2.3.3-150458?style=flat&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-3.8.0-11557C?style=flat&logo=python&logoColor=white)](https://matplotlib.org/)
[![Seaborn](https://img.shields.io/badge/Seaborn-0.12.2-4C72B0?style=flat&logo=python&logoColor=white)](https://seaborn.pydata.org/)
[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)

Análise exploratória de dados (EDA) e desenvolvimento de dashboard para visualização de insights do **Banco Vitória S.A. (Banvic)** — banco fictício utilizado como estudo de caso.

---

## Descrição

Este projeto consiste em duas etapas principais:

1. **Análise Exploratória de Dados (EDA)** — Notebook em Python que realiza limpeza, tratamento e visualização dos dados brutos do banco, exportando tabelas processadas para uso no dashboard.

2. **Dashboard Power BI** — Relatório interativo com modelo semântico (star schema) que permite explorar métricas de agências, clientes, contas, transações e propostas de crédito do Banvic.

---

## Estrutura do Projeto

```
projeto-banvic-dataviz/
├── banvic_dados/               # Dados brutos (CSV)
│   ├── agencias.csv
│   ├── clientes.csv
│   ├── colaborador_agencia.csv
│   ├── colaboradores.csv
│   ├── contas.csv
│   ├── propostas_credito.csv
│   └── transacoes.csv
├── dashboard/                  # Power BI Desktop (formato .pbip)
│   ├── Dashboard_banvic.pbip
│   ├── Dashboard_banvic.Report/
│   └── Dashboard_banvic.SemanticModel/
├── EDA_Banvic.ipynb            # Notebook de análise exploratória
├── requirements.txt            # Dependências Python
├── .gitignore
└── README.md
```

---

## Dados

O dataset é composto por 7 tabelas com dados do banco fictício Banvic:

| Tabela | Colunas Principais | Descrição |
|--------|-------------------|-----------|
| **agencias** | cod_agencia, nome, cidade, uf, tipo_agencia, data_abertura | 10 agências (Física/Digital) em São Paulo |
| **clientes** | cod_cliente, primeiro_nome, ultimo_nome, email, tipo_cliente, cpfcnpj, data_nascimento | ~998 clientes (Pessoa Física) |
| **colaborador_agencia** | cod_colaborador, cod_agencia | Tabela de junção colaborador ↔ agência |
| **colaboradores** | cod_colaborador, primeiro_nome, ultimo_nome, email, cpf, data_nascimento | Dados dos funcionários do banco |
| **contas** | num_conta, cod_cliente, tipo_conta, saldo_total, saldo_disponivel, data_ultimo_lancamento | ~999 contas com saldos e datas |
| **propostas_credito** | cod_proposta, cod_cliente, valor_proposta, valor_financiamento, status_proposta, taxa_juros_mensal | Propostas de financiamento com status (Aprovada/Análise/Validação/Reprovada) |
| **transacoes** | cod_transacao, num_conta, nome_transacao, valor_transacao, data_transacao | Transações (Crédito/Débito/Transferência/Depósito) |

---

## 🔍 Análise Exploratória (EDA)

O notebook `EDA_Banvic.ipynb` realiza as seguintes etapas para cada tabela:

### Tratamento dos Dados
- Conversão de colunas de data para `datetime`
- Verificação de duplicatas
- Verificação de tipos e estrutura (`info()`, `describe()`)

### Visualizações Geradas
| Tabela | Visualizações |
|--------|--------------|
| agencias | Distribuição por tipo (Física/Digital) e por cidade |
| clientes | Distribuição por tipo de cliente (PF/PJ) |
| contas | Histograma e boxplot de saldo total |
| propostas_credito | Boxplots de valores financiados e propostos |
| transacoes | Contagem de transações por tipo |

### Exportação
Ao final, o notebook exporta todas as tabelas tratadas para a pasta `banvic_limpo/` em formato CSV, que são utilizadas como fonte de dados do dashboard Power BI.

---

## Dashboard Power BI

O dashboard foi desenvolvido no **Power BI Desktop** utilizando o formato de projeto `.pbip` (Power BI Project), com modelo semântico versionado separadamente.

### Modelo Semântico (Star Schema)

**Tabelas de Dimensão:**
| Tabela | Descrição |
|--------|-----------|
| dim_agencias | Dados das agências (código, nome, endereço, cidade, UF, tipo, data de abertura) |
| dim_clientes | Dados dos clientes (código, nome, email, tipo, CPF/CNPJ, data de nascimento/inclusão) |
| dim_contas | Dados das contas (número, código do cliente/agência/colaborador, tipo, saldos, datas) |
| dim_colaboradores | Dados dos colaboradores (código, nome, email, CPF, data de nascimento) |

**Tabelas de Fato:**
| Tabela | Descrição |
|--------|-----------|
| fat_transacoes | Transações bancárias (código, conta, data, tipo, valor) |
| fat_propostas_credito | Propostas de financiamento (código, cliente, colaborador, valores, parcelas, carência, status) |

**Tabelas Calculadas:**
| Tabela | Descrição |
|--------|-----------|
| dCalendario | Calendário para análise temporal (Ano, Mês, Dia, Nome_mês, Nome_dia) |
| Medidas | Medidas DAX do relatório |

### Medidas DAX Principais

| Medida | Descrição |
|--------|-----------|
| Qnt. transações | Contagem distinta de transações |
| Valor Total Movimentado | Soma dos valores transacionados |
| Valor Médio Movimentado | Média dos valores transacionados |
| Total Financiamento Aprovado | Soma de financiamentos com status "Aprovada" |
| Qnt. Financeamento Aprovado | Contagem de propostas aprovadas |
| Taxa de Aprovação | Proporção de propostas aprovadas |
| Total Financiamento Proposto | Soma dos valores propostos |
| Valor Médio Proposto | Média dos valores propostos |
| Carência Média | Média de meses de carência |
| MoM Qnt. Transações | Variação mês a mês de transações |
| % Participação em Financiamentos Aprovados | Participação de cada agência no total |
| Média Transações | Média mensal de transações |

### Páginas do Relatório
O dashboard possui **2 páginas** com visuais incluindo:
- Gráficos de barras e colunas
- Gráficos de pizza
- Cartões (KPIs)
- Gráficos de linhas (tendências temporais)
- Filtros interativos

---

## Como Rodar

### Pré-requisitos
- Python 3.11+
- [Power BI Desktop](https://powerbi.microsoft.com/) (para visualizar o dashboard)

### Configuração do Ambiente Python

```bash
# Clonar o repositório
git clone <url-do-repositorio>
cd projeto-banvic-dataviz

# Criar ambiente virtual
python -m venv venv

# Ativar o ambiente virtual
# Linux/Mac:
source venv/bin/activate
# Windows:
venv\Scripts\activate

# Instalar dependências
pip install -r requirements.txt
```

### Executar a Análise Exploratória

```bash
# Abrir o Jupyter Notebook ou VS Code com extensão de notebooks
jupyter notebook EDA_Banvic.ipynb
# ou
code EDA_Banvic.ipynb
```

### Abrir o Dashboard

Abra o arquivo `dashboard/Dashboard_banvic.pbip` no **Power BI Desktop**.

> **Nota:** O dashboard carrega dados da pasta `banvic_limpo/`. Execute primeiro o notebook EDA para gerar essa pasta com os dados tratados.

---

## Dependências

| Biblioteca | Versão | Uso |
|-----------|--------|-----|
| pandas | 2.3.3 | Manipulação e análise de dados |
| matplotlib | 3.8.0 | Criação de gráficos estáticos |
| seaborn | 0.12.2 | Visualização estatística |

---

## Tecnologias

- **Python** — Linguagem principal para análise de dados
- **Pandas** — Manipulação e transformação de DataFrames
- **Matplotlib** / **Seaborn** — Geração de gráficos e visualizações
- **Power BI Desktop** — Desenvolvimento do dashboard interativo
- **Power BI Semantic Model** — Modelo de dados com schema estrela e medidas DAX


