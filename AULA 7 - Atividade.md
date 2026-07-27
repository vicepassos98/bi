# Atividade Avaliativa 2
## Análise de Desempenho Escolar

---

## Contexto

A **Escola Estadual General Tito** deseja entender melhor o desempenho acadêmico de seus alunos. A coordenação pedagógica suspeita que existe uma relação entre a **frequência dos alunos e suas notas**, ou seja, alunos que faltam mais tendem a ter notas mais baixas.

Você foi contratado como **analista de dados** e receberá três tabelas com informações sobre alunos, notas e turmas.

Sua missão é construir um **relatório no Power BI** que confirme ou refute essa hipótese.

---

# Tabelas Fornecidas

## Tabela: Alunos

| ID_Aluno | Nome | Turma | Ano |
|---|---|---|---|
| 1 | Ana Lima | 1º A | 1 |
| 2 | Bruno Costa | 1º A | 1 |
| 3 | Carla Souza | 1º A | 1 |
| ... | ... | ... | ... |
| 60 | Zacarias Góes | 3º B | 2 |

---

## Tabela: Notas

| ID_Nota | ID_Aluno | Disciplina | Nota | Faltas |
|---|---|---|---|---|
| 1 | 1 | Matemática | 8.5 | 3 |
| 2 | 1 | Português | 7.0 | 5 |
| 3 | 1 | História | 9.0 | 1 |
| ... | ... | ... | ... | ... |
| 300 | 60 | Geografia | 7.5 | 17 |

---

## Tabela: Turmas

| Turma | Professor_Responsavel | Período | Sala |
|---|---|---|---|
| 1º A | Prof. Giovanni Arrighi | Manhã | Sala 101 |
| 1º B | Profa. Marina Manzucato | Tarde | Sala 112 |
| 2º A | Prof. Celso Furtado | Manhã | Sala 102 |
| 2º B | Profa. Lélia Gonzales | Tarde | Sala 108 |
| 3º A | Profa. Carlota Perez | Manhã | Sala 107 |
| 3º B | Prof. Florestan Fernandes | Tarde | Sala 113 |

---

# Preparação de Dados

Antes de iniciar a análise, verifique se existem **erros ou inconsistências** nas tabelas fornecidas.

Em seguida, altere os tipos de dados conforme as orientações abaixo.

## Tabela Alunos

| Coluna | Tipo de Dados |
|---|---|
| ID_Aluno | Texto |
| Nome | Texto |
| Turma | Texto |
| Ano | Número inteiro |

---

## Tabela Notas

| Coluna | Tipo de Dados |
|---|---|
| ID_Nota | Texto |
| ID_Aluno | Texto |
| Disciplina | Texto |
| Nota | Número decimal |
| Faltas | Número inteiro |

---

## Tabela Turmas

Todas as colunas devem ser configuradas como:

- Texto

> **Observação:** Caso necessário, utilize a opção **Promover Primeira Linha como Cabeçalho**.

---

# Relacionamentos no Power BI

Configure os seguintes relacionamentos antes de começar as atividades:

```text
Alunos[ID_Aluno] → Notas[ID_Aluno]

Alunos[Turma] → Turmas[Turma]
```

Os relacionamentos devem permitir que as informações dos alunos e das turmas sejam utilizadas na análise dos dados presentes na tabela **Notas**.

---

# Atividades

## 1. Colunas Calculadas com RELATED

Na tabela `Notas`, crie as seguintes colunas calculadas utilizando a função `RELATED`:

### a) Nome do Aluno

Crie uma coluna que traga o **nome do aluno** a partir da tabela `Alunos`.

### b) Turma do Aluno

Crie uma coluna que traga a **turma do aluno** a partir da tabela `Alunos`.

---

## 2. Coluna Calculada com LOOKUPVALUE

Ainda na tabela `Notas`, crie uma coluna que busque o **Professor Responsável** de cada turma diretamente na tabela `Turmas`, utilizando a função `LOOKUPVALUE`.

Não utilize `RELATED` nesta atividade.

Utilize como referência a seguinte fórmula:

```DAX
Professor =
LOOKUPVALUE(
    Turmas[Professor_Responsavel],
    Turmas[Turma],
    Notas[Turma_Aluno]
)
```

---

## 3. Medidas com SUM, AVERAGE e Calculate

Crie as seguintes medidas na tabela `Notas`.

### a) Total de Faltas

Crie uma medida para calcular a soma total de faltas.


### b) Média de Faltas

Crie uma medida para calcular a soma total de faltas.

```DAX
Média_Faltas = AVERAGE(Notas[Faltas])
```

### c) Soma das Notas

Crie uma medida para calcular a soma de todas as notas.


### d) Média das Notas

Crie uma medida para calcular a média das notas utilizando a função `AVERAGE`.


### e) Total de Alunos Aprovados

Crie uma medida para calcular o total de alunos aprovados, usando a função `CALCUALATE`.

### f) Total de Alunos Reprovados

Crie uma medida para calcular o total de alunos reprovados, usando a função `CALCUALATE`.



---

# 4. Classificação com IF

Utilize a função `IF` para criar classificações relacionadas ao desempenho e à frequência dos alunos.

---

## 4.1 Situação do Aluno

Na tabela `Notas`, crie uma coluna calculada para classificar cada registro como:

- **Aprovado**
- **Reprovado**

Para ser considerado aprovado, o aluno deve possuir uma nota **maior ou igual a 5**.

Utilize a função `IF` para realizar essa classificação.

Exemplo:

```DAX
Situacao =
IF(
    Notas[Nota] >= 5,
    "Aprovado",
    "Reprovado"
)
```

---

## 4.2 Nível de Frequência

Crie uma coluna calculada para classificar o nível de frequência dos alunos.

Utilize as seguintes regras:

| Quantidade de Faltas | Classificação |
|---|---|
| Até 5 faltas | Boa |
| De 6 até 10 faltas | Regular |
| Acima de 10 faltas | Crítica |

Utilize uma estrutura de **IF alinhada** para realizar a classificação.

Exemplo:

```DAX
Nivel_Frequencia =
IF(
    Notas[Faltas] <= 5,
    "Boa",
    IF(
        Notas[Faltas] <= 10,
        "Regular",
        "Crítica"
    )
)
```

---

# 5. Análise de Correlação — Gráfico de Dispersão

Crie um **Gráfico de Dispersão (Scatter Chart)** no Power BI para analisar a possível relação entre a quantidade de faltas e o desempenho acadêmico dos alunos.

Configure o gráfico conforme a tabela abaixo:

| Campo do Gráfico | Medida/Coluna a utilizar |
|---|---|
| Eixo X (Valores) | `Total_Faltas` |
| Eixo Y (Valores) | `Media_Notas` |
| Legenda | `Nome` — tabela `Alunos` |

Analise o gráfico e responda:

> **Existe uma relação entre a quantidade de faltas e a média das notas dos alunos?**

A partir do gráfico, determine se a hipótese apresentada no contexto da atividade pode ser **confirmada ou refutada**.

---

# Dashboard Final Esperado

Organize o relatório final no Power BI utilizando os seguintes elementos visuais:

## Cartões (Cards)

Crie três cartões com os seguintes indicadores:

- **Total de Alunos**
- **Média Geral de Notas**
- **Total de Faltas**

---

## Gráfico de Barras 1

Crie um gráfico apresentando:

- **Média de Notas por Turma**

---

## Gráfico de Barras 2

Crie um gráfico apresentando:

- **Total de Faltas por Disciplina**

---

## Gráfico de Dispersão

Crie um gráfico de dispersão apresentando:

- **Faltas × Média de Notas**

Utilize esse gráfico para investigar a relação entre frequência e desempenho acadêmico.

---

## Matriz

Crie uma matriz contendo as seguintes informações:

- Nome do aluno
- Turma
- Professor
- Média de Notas
- Total de Faltas
- Situação

---

## Segmentações de Dados (Slicers)

Adicione duas segmentações de dados ao relatório:

1. **Turma**
2. **Disciplina**

As segmentações devem permitir que o usuário filtre e explore os dados de forma interativa.

---

# Resultado Esperado

Ao final da atividade, o relatório deverá permitir uma análise completa do desempenho escolar dos alunos, possibilitando:

- Identificar o desempenho médio por turma;
- Analisar a quantidade de faltas por disciplina;
- Identificar alunos aprovados e reprovados;
- Classificar o nível de frequência dos alunos;
- Comparar faltas e desempenho acadêmico;
- Utilizar filtros para explorar diferentes turmas e disciplinas;
- Avaliar se existe uma relação entre **frequência e desempenho escolar**.

> **Desafio final:** Com base nos dados apresentados no relatório e no gráfico de dispersão, responda à pergunta:
>
> **Alunos que faltam mais realmente apresentam notas mais baixas?**
