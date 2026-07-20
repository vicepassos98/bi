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
| + | Soma |
| - | Subtração |
| * | Multiplicação |
| / | Divisão |
| ^ | Exponenciação |

---

# Criando Colunas

Vamos procurar pelo "Modo de tabela", presente na lateral esquerda do power BI. 
Esse modo permite visualizar e editar Os dados dos bancos de dados carregados.
Depois, na lateral direita podemos navegar através dos diversos bancos de dados carregados nesse modelo semântico.
Acesse o banco de dados "VENDAS", nele iremos criar colunas que vão nos auxiliar na análise das informações

## Criando a coluna "Preço Produto"

Para Criar a nova coluna precisamos procurar pela opção "Nova Coluna"
O preço dos produtos já está presente na tabela "PRODUTOS", o que vamos fazer aqui é relacionar as tabelas.
De acordo com o "ID_PRODUTO" de cada linha o PBI irá buscar aquele mesmo código na tabela "PRODUTOS", e retornar o respectivo preço.
Para resolver esse problema iremos usar a função RELATED, relacionando nossa nova coluna com a coluna "Valor Unitário Unitário"

```DAX
Preço Produto = RELATED(PRODUTOS[Valor Unitário Unitário])
```

## Criando a coluna "Valor da Venda"

Para calcular o valor da venda vamos simplesmente multiplicar as colunas "Preço Produto" que acabamos de criar com a coluna "Quantidade".
Dessa forma saberemos qual foi o valor total de cada uma das vendas.
Assim como na coluna anterior, o PBI irá multiplicar os valores linha por linha

```DAX
Valor de Venda = Preço Produto*Quantidade
```

---

# Criando Medidas

Medidas são uma forma de calcular valores presentes nos bancos de dados e tranformá-los em índices. Vamos criar alguns deles facilitar nossa análise

## Faturamento
É o valor recebido pela empresa durante o período. Portanto será a soma de toda coluna "Valor de venda"

```DAX
Faturamento = SUM(VENDAS[Valor de Venda])
```

## Unidades Vendidas
É qauntidade de itenss vendidos pela empresa durante o período. Portanto será a soma de toda coluna "Quantidade"

```DAX
Unidades Vendidas = SUM(VENDAS[Unidades Vendidas])
```






# Exercícios utilizando relacionamentos

## Exercício 6

### Pergunta

**Qual estado contribuiu mais para o faturamento?**

**Visualização**

Gráfico de Rosca

---

## Exercício 7

### Pergunta

**Qual Produto teve a maior quantidade Vendida?**

- Unidades vendidas;
- Porduto;

**Visualização**

Gráfico de Barras

---

## Exercício 8

**Qual mês apresentou o maior faturamento?**

**Variáveis**

- Data da Venda (MÊS)
- Faturamento

**Visualização**

Gráfico de Linhas

---
# Resumo

Nesta aula aprendemos a:

- Importar dados utilizando o Power Query;
- Tratar informações antes do carregamento;
- Relacionar diferentes tabelas;
- Compreender o conceito de chave entre bancos de dados;
- Utilizar as funções `RELATED()` e `SUM()`;
- Criar visualizações utilizando dados provenientes de múltiplas tabelas.
