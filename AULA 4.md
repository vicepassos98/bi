# AULA 5 - Tipos de Dados

☢☢🎲🎲[Download do Banco de dados de hoje](https://docs.google.com/spreadsheets/d/1eX2PEHQeL2aUsokLgvH7RKDdO7RNuJk2tPxm2G64fZ8/export?format=csv&gid=2064696384)

---

# Diferença entre arquivos XLSX, CSV e SQL

Os formatos **XLSX**, **CSV** e **SQL** são muito utilizados para armazenar e trabalhar com dados, mas cada um possui uma finalidade diferente.

## XLSX — Planilha do Excel

O **XLSX** é o formato padrão utilizado pelo **Microsoft Excel**.

Ele permite armazenar dados em **planilhas organizadas em linhas e colunas**, além de recursos como:

- Fórmulas e funções;
- Gráficos;
- Tabelas dinâmicas;
- Formatação de células;
- Filtros;
- Formatação condicional;
- Várias abas dentro do mesmo arquivo.

**Exemplo:** uma empresa pode utilizar um arquivo `.xlsx` para controlar vendas, calcular o faturamento e criar gráficos.

**Extensão:** `.xlsx`

---

## CSV — Dados separados por delimitadores

O **CSV** significa **Comma-Separated Values** (Valores Separados por Vírgulas).

É um arquivo de texto simples utilizado para armazenar dados em formato de tabela. As informações são separadas por um delimitador, que pode ser:

- Vírgula `,`
- Ponto e vírgula `;`
- Tabulação

Diferente do XLSX, o CSV **não armazena fórmulas, gráficos ou formatações complexas**.

**Exemplo:**

```text
Nome;Idade;Cidade
João;25;São Paulo
Maria;30;Campinas
Pedro;22;Jundiaí
```

**Extensão:** `.csv`

O CSV é muito utilizado para **trocar dados entre diferentes sistemas**, importar informações para o **Power BI**, bancos de dados e outras ferramentas.

---

## SQL — Linguagem para trabalhar com bancos de dados

O **SQL** (Structured Query Language) é uma **linguagem utilizada para criar, consultar e manipular bancos de dados relacionais**.

Diferentemente do XLSX e do CSV, SQL não é simplesmente um formato de arquivo de planilha. Ele é utilizado para trabalhar com **bancos de dados**, que podem armazenar grandes volumes de informações.

Com SQL, podemos:

- Criar tabelas;
- Inserir dados;
- Alterar informações;
- Excluir registros;
- Fazer consultas;
- Relacionar diferentes tabelas;
- Filtrar e organizar dados.

**Exemplo de consulta SQL:**

```sql
SELECT Nome, Cidade
FROM Clientes
WHERE Cidade = 'São Paulo';
```

Essa consulta busca os **nomes e cidades dos clientes que moram em São Paulo**.

---

## Comparação rápida

| Característica | XLSX | CSV | SQL |
|---|---|---|---|
| Tipo | Planilha | Arquivo de texto | Linguagem de banco de dados |
| Organização | Linhas e colunas | Linhas e colunas | Tabelas e relacionamentos |
| Fórmulas | Sim |  Não |  Não diretamente |
| Gráficos | Sim |  Não |  Não |
| Formatação |  Sim |  Não |  Não |
| Grande volume de dados |  Limitado |  Pode ficar pesado |  Excelente |
| Banco de dados |  Não |  Não |  Sim |
| Uso comum | Análises e planilhas | Troca de dados | Sistemas e bancos de dados |

## Resumindo

> **XLSX** é ideal para trabalhar com **planilhas e análises no Excel**.  
> **CSV** é ideal para **armazenar e transferir dados de forma simples**.  
> **SQL** é utilizado para **gerenciar e consultar dados armazenados em bancos de dados**.

---

# Compreendendo o Banco de dados de hoje
A tabela hoje contempla informações de PIB (Produto Interno Bruto) de todos os municípios Brasileiros.
O PIB é uma forma de contabilizar tudo que é produzido em um espaço geográfico em um período de tempo.
No nosso caso, temos o PIB Anual do ano de 2023
Além do PIB total, temos os valores desagregados de acordo com os setores econômicos:
- Serviços
- Indústria
- Adiminstração Pública
- Impostos
- Agropecuária.

Além disso temos a população de cada um dos municípios e o Estado Pertencente a cada município.

## Calculando o PIB PER CAPITA
Esse índice é a média de produção por pessoa em uma cidade. Quanto maior o índice, maior é a produção de valor por pessoa.
Contudo é bom lembrar que a riqueza não é plenamente distribuida, sendo que alguns acabam recebendo mais que outros.
Para Calcular, basta dividir o PIB Total pela População.





