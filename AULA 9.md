# Trabalhando com KPIs no Power BI

## Bloco 1 — Preparação da Base de Dados

### 1.1 Obter os dados

[Clique aqui para pegar um vírus letal (ou baixar o banco de dados 🎲🎲)](https://docs.google.com/spreadsheets/d/1v7LDyP8Pr0idP8O3yCFM_XtOquIaq2aX/export?format=xlsx)


### Importar o arquivo para o Power BI

No Power BI:

1. Clique em **Obter Dados**.
2. Selecione **Excel**.
3. Localize o arquivo `BD_KPI.xlsx`.
4. Selecione as quatro abas:
   - Funcionários
   - Estoque
   - Vendas
   - Financeiro
5. Clique em **Transformar Dados**.

### Verificar os tipos de dados no Power Query

Antes de carregar os dados, verifique se cada coluna possui o tipo de dado correto.

| Tipo de informação | Tipo de coluna |
|---|---|
| Datas | Data |
| Valores monetários | Número Decimal Fixo (Moeda) |
| IDs | Texto |

> **Dica:** definir corretamente os tipos de dados evita erros durante a criação de relacionamentos, medidas DAX e visualizações.

---

## 1.2 Criar as relações no modelo

Após carregar os dados, acesse:

**Exibição de Modelo**

Crie as seguintes relações:

| Tabela | Campo | Tabela relacionada | Campo |
|---|---|---|---|
| Vendas | `ID_Funcionario` | Funcionarios | `ID_Funcionario` |
| Vendas | `ID_Produto` | Estoque | `ID_Produto` |
| Vendas | `ID_Financeiro` | Financeiro | `ID_Financeiro` |

Essas relações permitem que informações de diferentes tabelas sejam utilizadas conjuntamente nos gráficos, cartões, matrizes e medidas.

---

# Bloco 2 — KPI: Compreendendo o conceito

## O que é KPI?

KPI é a sigla para:

> **Key Performance Indicator**

Em português:

> **Indicador-Chave de Desempenho**

Um KPI é uma ferramenta de gestão utilizada para acompanhar o desempenho de objetivos, projetos, processos e ações.

Por meio dos KPIs, gestores conseguem verificar se a empresa está caminhando na direção dos seus objetivos estratégicos.

Os indicadores podem ser apresentados em diferentes unidades de medida, como:

- Quantidade
- Percentual
- Valor monetário
- Taxa
- Coeficiente
- Média

Não existe um único KPI ideal para todas as empresas.

Cada organização deve definir seus indicadores de acordo com:

- Seu setor
- Seus objetivos
- Suas estratégias
- Suas metas
- Suas necessidades

---

# KPI x Métrica

KPI e métrica não são exatamente a mesma coisa.

### Métrica

Uma **métrica** é um dado que pode ser medido.

Exemplos:

- Número de vendas
- Faturamento
- Número de funcionários
- Quantidade de clientes
- Número de cancelamentos

A métrica apresenta um resultado, mas não necessariamente está relacionada a um objetivo estratégico.

### KPI

Um **KPI** é uma métrica escolhida por sua importância estratégica para acompanhar determinado objetivo.

Por exemplo:

> Uma empresa possui a meta de aumentar as vendas em 20%.

Nesse caso:

**Métrica:** faturamento mensal.

**KPI:** crescimento percentual do faturamento em relação ao período anterior.

### Resumindo

> **Todo KPI é uma métrica, mas nem toda métrica é um KPI.**

O KPI depende de:

**Empresa → Setor → Objetivo → Meta → Indicador**

---

# Exemplos de KPIs

## 1. Financeiro — Customer Lifetime Value (CLV)

O **Customer Lifetime Value (CLV)** busca estimar quanto um cliente gasta com a empresa durante todo o período de relacionamento.

Esse indicador pode ajudar a empresa a entender se o retorno obtido com um cliente compensa os investimentos realizados para conquistá-lo e mantê-lo.

O CLV não deve ser analisado isoladamente.

É importante compará-lo com outros indicadores, como o **CAC (Custo de Aquisição de Clientes)**.

---

## 2. Recursos Humanos — Taxa de Turnover

A **Taxa de Turnover** mede a movimentação de funcionários que entram e saem da empresa.

Esse indicador pode ajudar o setor de Recursos Humanos a avaliar:

- Recrutamento
- Seleção
- Integração
- Onboarding
- Retenção de funcionários

Por exemplo:

Se muitos funcionários recém-contratados deixam a empresa rapidamente, pode ser necessário investigar problemas no processo de contratação ou integração.

---

## 3. Comercial — Win Rate

O **Win Rate**, também chamado de **Taxa de Conversão**, é utilizado para avaliar a capacidade do setor comercial de transformar oportunidades em vendas.

Por exemplo:

Uma equipe recebeu 100 oportunidades e conseguiu fechar 25 vendas.

```text
Win Rate = 25 / 100
Win Rate = 25%
```

Nesse caso, a taxa de conversão foi de **25%**.

Esse indicador pode ajudar os gestores a avaliar a eficiência das estratégias comerciais.

---

## 4. Clientes — CAC

O **CAC (Custo de Aquisição de Clientes)** representa quanto a empresa gasta, em média, para conquistar um novo cliente.

A fórmula básica é:

```text
CAC = Gastos com Marketing e Vendas / Número de Clientes Conquistados
```

### Exemplo

Uma empresa gastou:

```text
R$ 10.000
```

em marketing e vendas e conquistou:

```text
200 clientes
```

Então:

```text
CAC = 10.000 / 200
CAC = R$ 50,00
```

Cada novo cliente custou, em média, **R$ 50,00** para a empresa.

---

# Bloco 3 — Medidas DAX para KPIs

## 3.1 Criar uma tabela de medidas

Para organizar as medidas DAX, vamos criar uma tabela específica.

No Power BI:

**Modelagem → Nova Tabela**

Digite:

```DAX
Medidas = ROW("x", BLANK())
```

Depois:

1. Localize a tabela `Medidas`.
2. Delete a coluna automática gerada.
3. Crie todas as medidas dentro da tabela `Medidas`.

> **Objetivo:** manter as medidas organizadas em uma única tabela.

---

# 3.2 Medidas — Vendas

## Receita Total

Calcula a receita considerando somente as vendas concluídas.

```DAX
Receita Total =
CALCULATE(
    SUM(Vendas[Valor_Total]),
    Vendas[Status_Venda] = "Concluída"
)
```

---

## Ticket Médio

O ticket médio representa o valor médio das vendas concluídas.

```DAX
Ticket Médio =
DIVIDE(
    [Receita Total],
    CALCULATE(
        COUNTROWS(Vendas),
        Vendas[Status_Venda] = "Concluída"
    )
)
```

### Fórmula conceitual

```text
Ticket Médio = Receita Total / Quantidade de Vendas
```

---

## Taxa de Cancelamento

Indica a proporção de vendas canceladas em relação ao total de vendas.

```DAX
Taxa Cancelamento =
DIVIDE(
    CALCULATE(
        COUNTROWS(Vendas),
        Vendas[Status_Venda] = "Cancelada"
    ),
    COUNTROWS(Vendas)
)
```

### Fórmula conceitual

```text
Taxa de Cancelamento =
Quantidade de Vendas Canceladas / Quantidade Total de Vendas
```

> Formate a medida como **Porcentagem (%)**.

---

## Margem Bruta

A margem bruta representa a diferença entre o preço de venda e o custo do produto em relação ao preço de venda.

```DAX
Margem Bruta =
DIVIDE(
    SUM(Estoque[Preco_Venda]) - SUM(Estoque[Custo_Unitario]),
    SUM(Estoque[Preco_Venda])
)
```

### Fórmula conceitual

```text
Margem Bruta =
(Preço de Venda - Custo) / Preço de Venda
```

> Formate a medida como **Porcentagem (%)**.

---

# 3.3 Medidas — Financeiro

## Receita Líquida

Calcula a receita líquida considerando somente os pagamentos recebidos.

```DAX
Receita Liquida =
CALCULATE(
    SUM(Financeiro[Valor_Liquido]),
    Financeiro[Status_Pgto] = "Recebido"
)
```

---

## Taxa de Inadimplência

Calcula a proporção do valor bruto que está pendente.

```DAX
Taxa de Inadimplência =
DIVIDE(
    CALCULATE(
        SUM(Financeiro[Valor_Bruto]),
        Financeiro[Status_Pgto] = "Pendente"
    ),
    SUM(Financeiro[Valor_Bruto])
)
```

### Fórmula conceitual

```text
Taxa de Inadimplência =
Valor Pendente / Valor Bruto Total
```

> Formate a medida como **Porcentagem (%)**.

---

# Bloco 4 — Visualizações Básicas

## 4.1 Visuais a construir

| Visual | Eixo / Linhas / Legenda | Valores | Tabela(s) |
|---|---|---|---|
| Gráfico de Barras | Nome (`Funcionarios`) | `SUM(Valor_Total)` | Vendas + Funcionarios |
| Gráfico de Linha | Data_Venda (mês) | `SUM(Valor_Total)` | Vendas |
| Gráfico de Pizza | Canal_Venda | `SUM(Valor_Total)` | Vendas |
| Matriz | Nome_Produto | `Qtd_Atual`, `Preco_Venda` | Estoque |
| Matriz | Nome nas linhas × Mês nas colunas | `SUM(Valor_Total)` | Vendas + Funcionarios |
| Cartão (Card) | — | `SUM(Valor_Total)` | Vendas |

---

# 4.2 Segmentação de Dados

Adicione uma **Segmentação de Dados (Slicer)** para:

### Status da Venda

Campo:

```text
Status_Venda
```

Tipo:

```text
Lista
```

Também adicione uma segmentação para:

### Data da Venda

Configure a segmentação para permitir a análise das vendas por mês.

---

# 4.3 Perguntas de análise

Utilize o dashboard criado para responder às perguntas abaixo.

### 1. Vendedores

**Qual vendedor atingiu mais de 80% da meta no 1º semestre?**

Analise o desempenho individual dos vendedores.

---

### 2. Cancelamentos

**Em qual mês ocorreu o maior número de cancelamentos?**

Utilize o gráfico de linha ou uma matriz para comparar os meses.

---

### 3. Estoque

**Quais produtos estão abaixo do estoque mínimo?**

Analise a quantidade atual de cada produto e compare com o estoque mínimo.

---

### 4. Canal de venda

**Qual canal de venda gera maior ticket médio?**

Compare o ticket médio entre os diferentes canais de venda.

---

### 5. Receita

**Qual a diferença entre a receita bruta e a receita líquida acumulada no ano?**

Compare:

```text
Receita Bruta
      ↓
Impostos / Descontos / Deduções
      ↓
Receita Líquida
```

Analise a diferença entre os dois valores e identifique o impacto das deduções sobre o resultado financeiro.

---

# Desafio Final — Dashboard de KPIs

Ao final da aula, o dashboard deverá permitir uma análise rápida dos principais indicadores da empresa.

## Indicadores sugeridos

- Receita Total
- Receita Líquida
- Ticket Médio
- Taxa de Cancelamento
- Margem Bruta
- Taxa de Inadimplência

## O dashboard deve responder:

1. Quanto a empresa vendeu?
2. Qual é o ticket médio?
3. Qual é a taxa de cancelamento?
4. Qual é a margem dos produtos?
5. Quanto a empresa recebeu efetivamente?
6. Qual é a taxa de inadimplência?
7. Qual vendedor apresenta o melhor desempenho?
8. Qual canal de venda apresenta melhor resultado?
9. Quais produtos precisam de reposição?
10. Como as vendas evoluíram ao longo do tempo?

