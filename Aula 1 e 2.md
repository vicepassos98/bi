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

1. Faça o download da planilha
   > **Importante:** confirme em qual pasta o arquivo foi salvo.

3. Abra o **Power BI Desktop**.

4. Crie um **Relatório em Branco**.

5. Clique em **Obter Dados**.

6. Escolha a fonte de dados desejada.

   Exemplos:

   - Excel
   - CSV
   - SQL Server
   - Azure
   - Entre outras

7. Localize o arquivo.

8. Selecione as planilhas que deseja importar.

9. Escolha uma das opções:

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

Gráfico de Pizza

| Campo | Valor |
|--------|-------|
| Legenda | Trasnporte |
| Valores | Contagem de Transporte|

---

## Exercício 2

### Pergunta

**Em qual bairro há mais alunos?**

**Variável**

- Bairro

**Visualização**

Gráfico de Colunas Clusterizado

| Campo | Valor |
|--------|-------|
| Eixo X | Bairro |
| Eixo Y | Contagem |

---

## Exercício 3

### Pergunta

**Qual é o nível de Excel mais comum na turma?**

**Variável**

- Nível de Excel

**Visualização**

Gráfico de Barras Clusterizado

| Campo | Valor |
|--------|-------|
| Eixo X | Contagem Nível Excel |
| Eixo Y | Nível Excel |

---

## Exercício 4

### Pergunta

**Quantas horas de sono os alunos dormem (no total)?**

**Variável**

- Horas de Sono

**Visualização**

Indicador (Card)

| Campo | Valor |
|--------|-------|
| Valores | Soma de Horas de Sono |

---


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

**Em média, quantas horas de sono os alunos tem por dia?**

**Visualização**

Cartão

| Campo | Valor |
|--------|-------|
| Valores | Média de Hr de Sono |

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

**Qual é a mediana do horas de sono dos alunos?**

**Visualização**

Indicador

| Campo | Resumo |
|--------|---------|
| Valor | Mediana |
| Valor mínimo | Mínimo |
| Valor máximo | Máximo |

> Utilize sempre a variável **Horas de Sono**, alterando apenas o tipo de agregação.

---

## Exercício 7

### Pergunta

**Os moradores de quais bairros tem mais horas de tela, em média?**

**Variáveis**

- Bairro de Residência
- Horas de Tela

**Visualização**

Gráfico de Barras

| Campo | Valor |
|--------|-------|
| Categoria | Bairro |
| Valores | Média de Horas de Tela|

---

## Exercício 8

### Pergunta

**Qual é a idade média dos estudantes de cada bairro?**

**Variáveis**

- Idade
- Bairro

**Visualização**

Gráfico de Funil

| Campo | Valor |
|--------|-------|
| Categoria | Bairro |
| Valores | Média da Idade |

---

## Exercício 9

### Pergunta

**Qual nível de Excel tem mais horas de tela, em média?**

**Variáveis**

- Horas de Tela
- Nível de Excel

**Visualização**

Gráfico de Colunas

| Campo | Valor |
|--------|-------|
| Categoria | Nível de Excel |
| Valores | Média de Horas de Tela|

---

# Resumo

Nesta aula aprendemos:

- Como importar dados para o Power BI
- Como escolher uma visualização adequada
- Como responder perguntas utilizando gráficos
- Diferença entre média e mediana
- Como alterar o método de resumo de uma variável
