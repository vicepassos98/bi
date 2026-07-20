# Aula - Power BI: Power Query, Relacionamentos e Funções DAX

## Objetivos da aula

Ao final desta aula você será capaz de:

- Conhecer o Power Query
- Alterar informações de um banco de dados
- Revisar a criação de visualizações
- Importar múltiplas tabelas
- Relacionar bancos de dados
- Criar medidas utilizando DAX
- Utilizar as funções `SUM()` e `RELATED()`

---
[📥 Baixar Base de Dados](https://docs.google.com/spreadsheets/d/1ViJXokuil3uFQ83gYC2OKArLT8WPQCir/export?format=xlsx)
---

# Fluxo de Trabalho

## Importando os dados

1. Faça o download da base de dados no Google Drive.
   > **Importante:** confirme em qual pasta o arquivo foi salvo.

2. Abra o **Power BI Desktop**.

3. Crie um **Relatório em Branco**.

4. Clique em **Obter Dados**.

5. Selecione a fonte **Excel**.

6. Localize o arquivo.

7. Selecione **apenas** a tabela **Clientes**.

8. Clique em **Transformar Dados**.

---

# Power Query

Nesta aula utilizaremos o **Power Query**, ferramenta responsável por importar e tratar dados antes que eles sejam carregados para o Power BI.

Com ela é possível:

- Alterar nomes de colunas;
- Alterar tipos de dados;
- Remover colunas;
- Filtrar registros;
- Corrigir inconsistências;
- Preparar os dados para análise.

---

# Exercícios de Visualização

## Exercício 1

### Pergunta

**Qual estado concentra a maior quantidade de clientes?**

**Variável**

- Estado

**Visualização**

Gráfico de Colunas Clusterizado

| Campo | Valor |
|--------|-------|
| Eixo X | Estado |
| Eixo Y | Contagem de Estado |

---

## Exercício 2

### Pergunta

**Qual estado possui a maior renda média?**

**Variáveis**

- Estado
- Renda

**Visualização**

Gráfico de Barras Clusterizado

| Campo | Valor |
|--------|-------|
| Categoria | Estado |
| Valores | Média da Renda |

---

## Exercício 3

### Pergunta

**Quantos clientes são do gênero feminino?**

**Variável**

- Gênero

**Visualização**

Gráfico de Pizza

| Campo | Valor |
|--------|-------|
| Legenda | Gênero |
| Valores | Contagem |

---

## Exercício 4

### Pergunta

**Qual é a renda mediana, máxima e mínima dos clientes?**

**Variável**

- Renda

**Visualização**

Indicador

| Campo | Resumo |
|--------|---------|
| Valor | Mediana |
| Valor mínimo | Mínimo |
| Valor máximo | Máximo |

---

# Relacionando Bancos de Dados

Em projetos reais é muito comum que as informações estejam distribuídas em diferentes tabelas.

Para relacioná-las, é necessário que elas possuam uma coluna em comum.

Essa coluna é chamada de **chave** e permite que o Power BI conecte corretamente as informações.

No banco utilizado nesta aula serão criados dois relacionamentos:

| Tabela | Relacionamento | Chave |
|----------|----------------|-------|
| Vendas | Produtos | `ID_PRODUTO` |
| Vendas | Clientes | `ID_CLIENTE` |

Após criar esses relacionamentos, o Power BI será capaz de cruzar informações entre todas as tabelas.

---

# Introdução ao DAX

Assim como o Excel possui fórmulas, o Power BI utiliza a linguagem **DAX (Data Analysis Expressions)**.

A sintaxe é bastante semelhante às funções do Excel, porém:

- os nomes das funções são em inglês;
- alguns separadores mudam dependendo da configuração regional;
- além de colunas, podemos criar **medidas**.

Nesta aula veremos duas funções importantes.

---

## RELATED()

A função `RELATED()` é semelhante ao **PROCV** (ou **PROCX**).

Ela permite buscar informações de outra tabela relacionada.

Exemplo:

```DAX
Estado Cliente = RELATED(Clientes[Estado])
```

---

## SUM()

A função `SUM()` soma todos os valores de uma coluna.

Exemplo:

```DAX
Faturamento = SUM(Vendas[Faturamento])
```

Essa função normalmente é utilizada para criar **medidas**.

---

## Operações Matemáticas

Também é possível realizar operações matemáticas tradicionais para calcular Medidas ou criar novas colunas.

| Símbolo | Operação |
|---------|--------- |
| **+** | Soma |
| **-** | Subtração |
| ***** | Multiplicação |
| **/** | Divisão |
| **^** | Exponenciação |
---

# Exercícios utilizando relacionamentos

## Exercício 6

### Pergunta

**Qual estado contribuiu mais para o faturamento?**

**Variáveis**

- Estado
- Faturamento

**Visualização**

Gráfico de Rosca

| Campo | Valor |
|--------|-------|
| Legenda | Estado |
| Valores | Soma do Faturamento |

---

## Exercício 7

### Pergunta

**Monte uma matriz demonstrando:**

- Quantidade de unidades vendidas;
- Faturamento;
- Preço.

**Visualização**

Matriz

| Campo | Valor |
|--------|-------|
| Linhas | Produto |
| Valores | Quantidade, Faturamento e Preço |

---

## Exercício 8

### Pergunta

**Qual mês apresentou o maior faturamento?**

**Variáveis**

- Data da Venda
- Faturamento

**Visualização**

Gráfico de Linhas

| Campo | Valor |
|--------|-------|
| Eixo X | Data da Venda (Mês) |
| Eixo Y | Soma do Faturamento |

---

# Resumo

Nesta aula aprendemos a:

- Importar dados utilizando o Power Query;
- Tratar informações antes do carregamento;
- Relacionar diferentes tabelas;
- Compreender o conceito de chave entre bancos de dados;
- Utilizar as funções `RELATED()` e `SUM()`;
- Criar visualizações utilizando dados provenientes de múltiplas tabelas.
