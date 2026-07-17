# Aula - Power BI: Importação de Dados e Primeiras Visualizações

## Objetivos da aula

Nesta aula você aprenderá a:

- Importar dados para o Power BI
- Criar gráficos e visualizações
- Editar visualizações
- Aplicar filtros
- Compreender formas de resumir dados
- Entender as medidas de tendência central (média e mediana)

[Para baixar a planilha clique aqui](https://docs.google.com/spreadsheets/d/12KZ2YzJKPyHNQGTZIFBWUicfTwVtZF5d/export?format=xlsx)
---

# Fluxo de Trabalho para Carregar os Dados

1. Faça o download da planilha no Google Drive.
   > **Importante:** confirme em qual pasta o arquivo foi salvo.

2. Abra a planilha no **Excel**.

3. Insira as novas informações.

4. Abra o **Power BI Desktop**.

5. Crie um **Relatório em Branco**.

6. Clique em **Obter Dados**.

7. Escolha a fonte de dados desejada.

   Exemplos:

   - Excel
   - CSV
   - SQL Server
   - Azure
   - Entre outras

8. Localize o arquivo.

9. Selecione as planilhas que deseja importar.

10. Escolha uma das opções:

- **Carregar Dados**
  - Utilize quando os dados estiverem organizados corretamente.

- **Transformar Dados**
  - Utilize quando for necessário limpar ou tratar os dados antes da importação.

---

# Criando Visualizações

Antes de construir qualquer gráfico, faça uma pergunta.

> **Todo gráfico responde a uma pergunta.**

---

## Exercício 1

### Pergunta

**Qual é o meio de transporte mais utilizado para vir ao curso?**

**Variável**

- Meios de Transporte

**Visualização**

Gráfico de Colunas Clusterizado

| Campo | Valor |
|--------|-------|
| Eixo X | Meios de Transporte |
| Eixo Y | Contagem de Meios de Transporte |

---

## Exercício 2

### Pergunta

**Qual é o nível de Excel mais comum na turma?**

**Variável**

- Nível de Excel

**Visualização**

Gráfico de Pizza

| Campo | Valor |
|--------|-------|
| Legenda | Nível de Excel |
| Valores | Contagem do Nível de Excel |

---

## Exercício 3

### Pergunta

**Quantas xícaras de café os alunos tomam por dia (no total)?**

**Variável**

- Cafés por Dia

**Visualização**

Indicador (Card)

| Campo | Valor |
|--------|-------|
| Valores | Soma de Cafés por Dia |

---

## Exercício 4

### Pergunta

**Em qual bairro há mais alunos?**

**Variável**

- Bairro de Residência

**Visualização**

Gráfico de Colunas Clusterizado

| Campo | Valor |
|--------|-------|
| Eixo X | Bairro |
| Eixo Y | Contagem |

---

# Média

A **média** é uma medida de tendência central.

Ela procura representar um conjunto de dados através de um único valor.

## Como calcular

1. Some todos os valores.
2. Divida pela quantidade de elementos.

### Exemplo

Valores:

```
2, 3, 8, 6, 7
```

Soma:

```
26
```

Quantidade:

```
5
```

Média:

```
26 ÷ 5 = 5,2
```

---

## Exercício 5

### Pergunta

**Em média, quantas xícaras de café os alunos tomam por dia?**

**Visualização**

Cartão

| Campo | Valor |
|--------|-------|
| Valores | Média de Cafés por Dia |

---

# Mediana

A **mediana** também é uma medida de tendência central.

Ela representa o valor que fica exatamente no meio dos dados quando eles são colocados em ordem.

## Exemplo 1

```
2, 3, 6, 7, 8
```

Mediana:

```
6
```

---

## Exemplo 2

```
2, 3, 4, 8, 9, 10
```

Existem dois valores centrais:

```
4 e 8
```

Logo:

```
(4 + 8) ÷ 2 = 6
```

---

## Média x Mediana

A média pode ser bastante afetada por valores extremamente altos ou baixos.

### Exemplo

Imagine um grupo com:

- 1 pessoa ganha R$ 10.000.000
- 10 pessoas ganham R$ 1.500

A média ficará extremamente alta, apesar de quase ninguém receber esse valor.

Nesses casos, a **mediana representa melhor a realidade**.

---

## Exercício 6

### Pergunta

**Qual é a mediana do consumo de café dos alunos?**

**Visualização**

Indicador

| Campo | Resumo |
|--------|---------|
| Valor | Mediana |
| Valor mínimo | Mínimo |
| Valor máximo | Máximo |

> Utilize sempre a variável **Cafés por Dia**, alterando apenas o tipo de agregação.

---

## Exercício 7

### Pergunta

**Os moradores de quais bairros dormem mais, em média?**

**Variáveis**

- Bairro de Residência
- Horas de Sono

**Visualização**

Gráfico de Barras

| Campo | Valor |
|--------|-------|
| Categoria | Bairro |
| Valores | Média de Horas de Sono |

---

## Exercício 8

### Pergunta

**Qual é a idade média dos estudantes e dos empregados?**

**Variáveis**

- Idade
- Situação Profissional

**Visualização**

Gráfico de Funil

| Campo | Valor |
|--------|-------|
| Categoria | Situação Profissional |
| Valores | Média da Idade |

---

## Exercício 9

### Pergunta

**Qual nível de Excel consome mais café, em média?**

**Variáveis**

- Cafés por Dia
- Nível de Excel

**Visualização**

Gráfico de Colunas

| Campo | Valor |
|--------|-------|
| Categoria | Nível de Excel |
| Valores | Média de Cafés por Dia |

---

## Exercício 10

### Pergunta

**Qual situação profissional possui maior tempo médio de tela?**

**Variáveis**

- Tempo de Tela
- Situação Profissional

**Visualização**

Gráfico de Área Empilhada

| Campo | Valor |
|--------|-------|
| Categoria | Situação Profissional |
| Valores | Média de Horas de Tela |

---

# Resumo

Nesta aula aprendemos:

- Como importar dados para o Power BI
- Como escolher uma visualização adequada
- Como responder perguntas utilizando gráficos
- Diferença entre média e mediana
- Como alterar o método de resumo de uma variável
