# Aula - Power BI: Power Query, IF e Colunas Calculadas

## Objetivos da aula

Ao final desta aula você será capaz de:

- Importar múltiplas tabelas para o Power BI;
- Ajustar tipos de dados no Power Query;
- Criar colunas calculadas utilizando DAX;
- Utilizar a função `IF()`;
- Utilizar a função `RELATED()`;
- Calcular faturamento e lucro;
- Criar visualizações para responder perguntas de negócio.

---

# Base de Dados

📥 [**Download da base de dados**](https://docs.google.com/spreadsheets/d/1AOWsEqLRNwyLM7VYisOXmkBnp3p_eImW/export?format=xlsx)
---

# Fluxo de Trabalho

1. Faça o download da base de dados.
   > **Importante:** confirme em qual pasta o arquivo foi salvo.

2. Abra o **Power BI Desktop**.

3. Crie um **Relatório em Branco**.

4. Clique em **Obter Dados**.

5. Escolha a fonte **Excel**.

6. Localize o arquivo.

7. Selecione **todas as planilhas**.

8. Clique em **Transformar Dados**.

---

# Tratando os Dados no Power Query

Antes de carregar os dados, ajuste os tipos das colunas.

| Tipo de informação | Tipo de dado |
|--------------------|--------------|
| Datas | Data |
| IDs | Texto |
| Valores / Preços | Número Decimal Fixo |
| Quantidades | Número Inteiro |

Após realizar as alterações, clique em **Fechar e Aplicar**.

---

# Operadores Lógicos

Durante esta aula utilizaremos operadores lógicos para realizar testes.

| Operador | Significado |
|-----------|-------------|
| `=` | Igual |
| `>` | Maior que |
| `<` | Menor que |
| `>=` | Maior ou igual |
| `<=` | Menor ou igual |

---

# Função IF()

A função `IF()` é utilizada para realizar testes lógicos.

Ela corresponde às funções **SE** e **SES** do Excel.

## Sintaxe

```DAX
IF(Teste Lógico, Valor se Verdadeiro, Valor se Falso)
```

Se a condição for verdadeira, o primeiro valor será retornado.

Caso contrário, será retornado o segundo valor.

---

## Exemplo

Vamos separar os clientes em duas faixas etárias.

- Jovens → idade até 30 anos.
- Adultos → idade acima de 30 anos.

```DAX
Faixa Etária =
IF(
    Clientes[Idade] <= 30,
    "Jovem",
    "Adulto"
)
```

---

# Relacionando as Tabelas

Antes de utilizar a função `RELATED()`, é necessário criar os relacionamentos entre as tabelas.

## Relacionamentos

| Tabela | Relacionamento | Chave |
|----------|----------------|-------|
| Vendas | Estoque | `ID_Produto` |
| Vendas | Clientes | `ID_Cliente` |

---

# Função RELATED()

A função `RELATED()` permite buscar informações de outra tabela relacionada.

### Valor de Venda

```DAX
Valor Venda =
RELATED(Estoque[Preço de Venda])
```

### Custo da Venda

```DAX
Custo da Venda =
RELATED(Estoque[Preço de Compra])
```

---

# Calculando o Lucro

Após criar as duas colunas anteriores, calcule o lucro de cada venda.

```DAX
Lucro =
Vendas[Valor Venda] -
Vendas[Custo da Venda]
```

---

# Desafio

Na tabela **Clientes**, crie sozinho uma nova coluna chamada **Faixa de Renda**.

Critérios:

- **Alta** → renda superior a R$ 9.850.
- **Baixa** → renda igual ou inferior a R$ 9.850.

---

# Exercícios

Utilizando gráficos e visualizações do Power BI, responda às perguntas abaixo.

> Sempre indique quais campos foram utilizados em **Categoria** e **Valores**.

---

## Exercício 1


**Qual mês apresentou o maior lucro? E qual apresentou o menor?**

**Sugestão de visualização**

Gráfico de Linhas

---

## Exercício 2

**Quais clientes geraram mais lucro para a empresa?**

- Clientes de renda **Alta**
- Clientes de renda **Baixa**

**Sugestão de visualização**

Gráfico de Colunas

---

## Exercício 3

**Qual foi o faturamento total do período?**

**Sugestão de visualização**

Cartão (Card)

---

## Exercício 4

**Quais foram os três produtos com maior quantidade de unidades vendidas?**

**Sugestão de visualização**

Gráfico de Barras

---

## Exercício 5

**Qual marca de tênis gerou o maior lucro?**

**Sugestão de visualização**

Gráfico de Rosca ou Barras

---

## Exercício 6

**Qual mês apresentou o pior faturamento?**

**Análise**

Após identificar o resultado, responda:

> O faturamento tende a aumentar, diminuir ou permanecer estável ao longo do tempo?

**Sugestão de visualização**

Gráfico de Linhas

---

## Exercício 7

**Qual faixa etária comprou a maior quantidade de unidades de tênis?**

**Sugestão de visualização**

Gráfico de Colunas

---

# Resumo

Nesta aula aprendemos a:

- Ajustar tipos de dados no Power Query;
- Utilizar operadores lógicos;
- Criar colunas calculadas com `IF()`;
- Buscar informações entre tabelas utilizando `RELATED()`;
- Calcular faturamento, custo e lucro;
- Utilizar visualizações para responder perguntas de negócio.
