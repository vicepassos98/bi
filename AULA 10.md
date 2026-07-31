# AULA 10 Revisão de Power BI — Análise das Olimpíadas

[DOWNLOAD BANCO DE DADOS] (https://drive.google.com/uc?export=download&id=1NWD1ChCgPLbdxsO6dkepv59SJTr80vJ-&utm_source=chatgpt.com)

# 1. Objetivos da aula

Ao final da aula, o aluno deverá ser capaz de:

* Revisar a importação e preparação de dados no Power Query.
* Diferenciar **coluna calculada** de **medida**.
* Criar colunas utilizando fórmulas DAX.
* Criar medidas utilizando funções de agregação.
* Utilizar as funções:

  * `IF`
  * `SUM`
  * `CALCULATE`
  * `LOOKUPVALUE`
* Criar indicadores (KPIs).
* Configurar e estilizar gráficos.
* Utilizar filtros e segmentações.
* Construir um pequeno dashboard sobre as Olimpíadas.

---

# 2. Dataset utilizado

A base possui informações sobre atletas, países, modalidades e medalhas.

## Colunas disponíveis

| Coluna        | Descrição                    |
| ------------- | ---------------------------- |
| `ano`         | Ano da edição dos Jogos      |
| `edicao`      | Edição dos Jogos Olímpicos   |
| `cidade_sede` | Cidade que sediou os Jogos   |
| `pais`        | País relacionado ao registro |
| `delegacao`   | Delegação participante       |
| `equipe`      | Equipe                       |
| `id_atleta`   | Identificador do atleta      |
| `nome_atleta` | Nome do atleta               |
| `sexo`        | Sexo do atleta               |
| `idade`       | Idade do atleta              |
| `altura`      | Altura do atleta             |
| `peso`        | Peso do atleta               |
| `esporte`     | Esporte praticado            |
| `evento`      | Evento/modalidade específica |
| `medalha`     | Medalha conquistada          |

---

# 3. Primeira etapa — Conhecendo os dados

Antes de começar os cálculos, analisar a estrutura da tabela.

### Quais colunas temos na nossa tabela?

1. Quantas linhas existem na tabela?
2. Qual é o período abrangido pelos dados?
3. Quantos atletas existem?
4. Quantos países aparecem?
5. Quantos esportes existem?
6. Quais tipos de medalhas aparecem?
7. Existem valores vazios?
8. Quais colunas são numéricas?
9. Quais colunas são categóricas?

---

# 4. Preparação dos dados

Abrir o **Power Query** e verificar os tipos de dados.

### Sugestão

| Coluna        | Tipo           |
| ------------- | -------------- |
| `ano`         | Número inteiro |
| `edicao`      | Texto          |
| `cidade_sede` | Texto          |
| `pais`        | Texto          |
| `delegacao`   | Texto          |
| `equipe`      | Texto          |
| `id_atleta`   | Número inteiro |
| `nome_atleta` | Texto          |
| `sexo`        | Texto          |
| `idade`       | Número inteiro |
| `altura`      | Número decimal |
| `peso`        | Número decimal |
| `esporte`     | Texto          |
| `evento`      | Texto          |
| `medalha`     | Texto          |

### Verificações

* Valores nulos
* Tipos de dados
* Nomes das colunas
* Valores inconsistentes
* Espaços extras
* Categorias diferentes para a mesma informação

---

# 5. Coluna calculada x Medida

Antes de começar o DAX, revisar a principal diferença.

## Coluna calculada

É calculada **linha por linha**.

Exemplo:

```DAX
Classificação Medalha =
IF(
    ISBLANK('Olimpiadas'[medalha]),
    "Sem medalha",
    "Medalhista"
)
```

Resultado:

| Atleta | Medalha | Classificação |
| ------ | ------- | ------------- |
| João   | Ouro    | Medalhista    |
| Maria  | Prata   | Medalhista    |
| Carlos | —       | Sem medalha   |

---

## Medida

É calculada dinamicamente de acordo com o contexto do relatório.

Exemplo:

```DAX
Total Atletas =
DISTINCTCOUNT('Olimpiadas'[id_atleta])
```

Ao colocar essa medida em um cartão, ela apresenta o total de atletas.

Ao colocar `sexo` em uma tabela junto com a medida, o resultado passa a ser calculado para cada sexo.

---

# 6. Exercício 1 — Criando uma coluna com IF

Criar uma coluna chamada:

**Status Medalha**

Regra:

* Se `medalha` estiver vazia → `"Sem medalha"`
* Caso contrário → `"Medalhista"`

### Fórmula

```DAX
Status Medalha =
IF(
    ISBLANK('Olimpiadas'[medalha]),
    "Sem medalha",
    "Medalhista"
)
```


---

# 7. Exercício 2 — Criando uma coluna numérica

Criar uma coluna chamada:

**Pontos Medalha**

Regra:

| Medalha     | Pontos |
| ----------- | -----: |
| Ouro        |      3 |
| Prata       |      2 |
| Bronze      |      1 |
| Sem medalha |      0 |

Uma possibilidade:

```DAX
Pontos Medalha =
IF(
    'Olimpiadas'[medalha] = "Ouro",
    3,
    IF(
        'Olimpiadas'[medalha] = "Prata",
        2,
        IF(
            'Olimpiadas'[medalha] = "Bronze",
            1,
            0
        )
    )
)
```

### Discussão

Mostrar aos alunos que o `IF` pode ser utilizado de forma aninhada, mas que existem outras funções DAX que podem deixar regras com muitas categorias mais organizadas.

---

# 8. Criando as primeiras medidas

Agora vamos passar das colunas para as medidas.

## Medida 1 — Total de registros

```DAX
Total Registros =
COUNTROWS('Olimpiadas')
```

---

## Medida 2 — Total de atletas

```DAX
Total Atletas =
DISTINCTCOUNT('Olimpiadas'[id_atleta])
```

---

## Medida 3 — Total de medalhas

```DAX
Total Medalhas =
COUNT('Olimpiadas'[medalha])
```

> Atenção: essa medida considera apenas registros preenchidos na coluna `medalha`.

---

# 9. Revisão da função SUM

A função `SUM` soma os valores de uma coluna numérica.

Exemplo:

```DAX
Total Pontos =
SUM('Olimpiadas'[Pontos Medalha])
```

Essa medida soma os pontos atribuídos às medalhas.

---

# 10. Criando medidas específicas para medalhas

Criar:

### Medalhas de Ouro

```DAX
Medalhas Ouro =
CALCULATE(
    COUNTROWS('Olimpiadas'),
    'Olimpiadas'[medalha] = "Ouro"
)
```

### Medalhas de Prata

```DAX
Medalhas Prata =
CALCULATE(
    COUNTROWS('Olimpiadas'),
    'Olimpiadas'[medalha] = "Prata"
)
```

### Medalhas de Bronze

```DAX
Medalhas Bronze =
CALCULATE(
    COUNTROWS('Olimpiadas'),
    'Olimpiadas'[medalha] = "Bronze"
)
```

---

# 11. Entendendo o CALCULATE

A função `CALCULATE` é uma das funções mais importantes do DAX.

Podemos pensar inicialmente da seguinte forma:

> **CALCULATE pega um cálculo e modifica o contexto em que ele será realizado.**

Exemplo:

```DAX
Total Medalhas =
COUNTROWS('Olimpiadas')
```

Agora:

```DAX
Medalhas Ouro =
CALCULATE(
    [Total Medalhas],
    'Olimpiadas'[medalha] = "Ouro"
)
```

O cálculo passa a considerar somente os registros de medalha de ouro.

---

# 12. Exercício — Ranking de medalhas

Criar uma tabela contendo:

* `pais`
* Medalhas Ouro
* Medalhas Prata
* Medalhas Bronze
* Total Medalhas

Exemplo de medida:

```DAX
Total Medalhas =
[Medalhas Ouro] +
[Medalhas Prata] +
[Medalhas Bronze]
```

Ordenar a tabela pelo número total de medalhas.

### Desafio

> Qual país possui mais medalhas?

> Qual país possui mais medalhas de ouro?

> O país com mais medalhas totais também possui mais ouros?

---

# 13. Trabalhando com LOOKUPVALUE

Para revisar `LOOKUPVALUE`, criar uma pequena tabela auxiliar chamada:

**Tabela Medalhas**

| medalha | pontos |
| ------- | -----: |
| Ouro    |      3 |
| Prata   |      2 |
| Bronze  |      1 |

Criar uma coluna calculada na tabela principal:

```DAX
Pontos Medalha =
LOOKUPVALUE(
    'Tabela Medalhas'[pontos],
    'Tabela Medalhas'[medalha],
    'Olimpiadas'[medalha]
)
```

### O que estamos fazendo?

Estamos dizendo ao Power BI:

> Procure na tabela `Tabela Medalhas` o valor correspondente à medalha atual e retorne a quantidade de pontos.

---

# 14. Comparando IF e LOOKUPVALUE

Mostrar aos alunos duas maneiras de resolver o mesmo problema.

### Com IF

```DAX
Pontos Medalha =
IF(
    'Olimpiadas'[medalha] = "Ouro",
    3,
    IF(
        'Olimpiadas'[medalha] = "Prata",
        2,
        IF(
            'Olimpiadas'[medalha] = "Bronze",
            1,
            0
        )
    )
)
```

### Com LOOKUPVALUE

```DAX
Pontos Medalha =
LOOKUPVALUE(
    'Tabela Medalhas'[pontos],
    'Tabela Medalhas'[medalha],
    'Olimpiadas'[medalha]
)
```

### Discussão

Perguntar:

> Qual solução fica mais fácil de manter se amanhã criarmos novas categorias?

---

# 15. Construindo os KPIs

Criar uma área superior no relatório com cartões:

### KPI 1

**Total de atletas**

```DAX
Total Atletas =
DISTINCTCOUNT('Olimpiadas'[id_atleta])
```

### KPI 2

**Total de medalhas**

```DAX
Total Medalhas =
COUNT('Olimpiadas'[medalha])
```

### KPI 3

**Total de países**

```DAX
Total Países =
DISTINCTCOUNT('Olimpiadas'[pais])
```

### KPI 4

**Total de esportes**

```DAX
Total Esportes =
DISTINCTCOUNT('Olimpiadas'[esporte])
```

---

# 16. Construindo os gráficos

Agora começa a parte visual da revisão.

## Gráfico 1 — Medalhas por país

Visual:

**Gráfico de barras**

Eixo:

`pais`

Valores:

`Total Medalhas`

### Configurações

* Ordenar do maior para o menor.
* Ativar rótulos de dados.
* Ajustar título.
* Remover elementos visuais desnecessários.
* Utilizar uma cor principal.
* Ajustar tamanho das fontes.

---

# 17. Gráfico 2 — Evolução das Olimpíadas

Visual:

**Gráfico de linhas**

Eixo:

`ano`

Valores:

`Total Medalhas`

Objetivo:

> Observar como a quantidade de medalhas registradas varia ao longo das edições.

---

# 18. Gráfico 3 — Medalhas por esporte

Visual:

**Gráfico de barras**

Eixo:

`esporte`

Valores:

`Total Medalhas`

Ordenar do maior para o menor.

### Desafio

Mostrar apenas os **10 esportes com maior número de medalhas**.

---

# 19. Gráfico 4 — Distribuição por sexo

Visual:

**Gráfico de rosca**

Legenda:

`sexo`

Valores:

`Total Atletas`

Objetivo:

Visualizar a distribuição dos atletas por sexo.

---

# 20. Gráfico 5 — Altura x Peso

Visual:

**Gráfico de dispersão**

Eixo X:

`altura`

Eixo Y:

`peso`

Legenda:

`sexo`

Objetivo:

Investigar a relação entre altura e peso dos atletas.

### Perguntas

* Existe relação entre altura e peso?
* Existem grupos muito diferentes?
* Existem valores extremos?
* O comportamento parece diferente entre homens e mulheres?

---

# 21. Segmentações de dados

Adicionar filtros para permitir que o usuário explore o dashboard.

### Segmentações sugeridas

* Ano
* País
* Sexo
* Esporte
* Medalha

### Desafio

Selecionar apenas:

> Olimpíadas de determinado período + um país + um esporte.

Observar como todos os gráficos são alterados.

---

# 22. Estilização dos gráficos

Nesta etapa, o objetivo não é apenas fazer o gráfico funcionar.

O aluno deverá revisar:

### Títulos

Evitar:

> Gráfico 1

Preferir:

> Medalhas por país

---

### Cores

Utilizar uma paleta consistente.

Exemplo:

* Ouro → dourado
* Prata → cinza
* Bronze → marrom/bronze
* Demais gráficos → cor principal do dashboard

---

### Rótulos

Ativar quando ajudarem na interpretação.

Evitar excesso de informações.

---

### Fundo

Evitar excesso de elementos.

Preferir:

* fundo limpo;
* cartões bem definidos;
* alinhamento;
* espaçamento consistente.

---

# 23. Desafio final — Dashboard Olímpico

Construir uma página de dashboard contendo:

## Linha 1 — KPIs

* Total de atletas
* Total de medalhas
* Total de países
* Total de esportes

## Linha 2 — Análises

* Medalhas por país
* Evolução das medalhas ao longo dos anos

## Linha 3 — Detalhamento

* Medalhas por esporte
* Distribuição por sexo
* Altura x peso

## Filtros

* Ano
* País
* Sexo
* Esporte
* Medalha

---

# 24. Desafio DAX

Criar uma medida:

**Percentual de medalhas de ouro**

```DAX
% Medalhas Ouro =
DIVIDE(
    [Medalhas Ouro],
    [Total Medalhas]
)
```

Formatar como porcentagem.

---

# 25. Desafio DAX 2 — Pontuação olímpica

Criar uma medida:

```DAX
Pontuação Olímpica =
SUM('Olimpiadas'[Pontos Medalha])
```

Utilizar essa medida em uma tabela com:

* País
* Pontuação Olímpica
* Total Medalhas

Ordenar pela pontuação.

### Pergunta

> O ranking por quantidade de medalhas é igual ao ranking por pontuação?

---

# 26. Desafio final de análise

Cada aluno deverá responder:

1. Qual país possui mais medalhas?
2. Qual país possui mais medalhas de ouro?
3. Qual esporte possui mais medalhas?
4. Como a participação feminina evoluiu ao longo dos anos?
5. Existe relação entre altura e peso?
6. Qual edição possui maior número de medalhas?
7. Qual país possui maior pontuação olímpica?
8. O país com mais medalhas é também o país com maior pontuação?

---

# 27. Checklist da revisão

Ao terminar a aula, o aluno deverá ter revisado:

* [ ] Importação de dados
* [ ] Power Query
* [ ] Tipos de dados
* [ ] Coluna calculada
* [ ] Medida
* [ ] `IF`
* [ ] `SUM`
* [ ] `CALCULATE`
* [ ] `LOOKUPVALUE`
* [ ] `DISTINCTCOUNT`
* [ ] `COUNT`
* [ ] `COUNTROWS`
* [ ] `DIVIDE`
* [ ] KPIs
* [ ] Gráfico de barras
* [ ] Gráfico de linhas
* [ ] Gráfico de rosca
* [ ] Gráfico de dispersão
* [ ] Segmentações
* [ ] Formatação
* [ ] Organização visual
* [ ] Análise dos resultados

---

# 28. Estrutura sugerida da aula

| Etapa     | Atividade                |    Tempo |
| --------- | ------------------------ | -------: |
| 1         | Apresentação do dataset  |   10 min |
| 2         | Revisão do Power Query   |   15 min |
| 3         | Coluna x medida          |   15 min |
| 4         | `IF`                     |   15 min |
| 5         | `SUM` e medidas          |   15 min |
| 6         | `CALCULATE`              |   25 min |
| 7         | `LOOKUPVALUE`            |   20 min |
| 8         | Criação dos KPIs         |   15 min |
| 9         | Construção dos gráficos  |   30 min |
| 10        | Estilização do dashboard |   20 min |
| 11        | Desafio final            |   30 min |
| **Total** | **Revisão completa**     | **3h30** |

---

# 29. Projeto final

## 🏅 Olimpíadas — Dashboard de Desempenho

O aluno deverá entregar um dashboard interativo capaz de responder:

> **Quem participou, quem ganhou e como o desempenho olímpico mudou ao longo do tempo?**

O relatório deverá conter:

* KPIs;
* gráficos;
* filtros;
* medidas DAX;
* pelo menos uma coluna calculada;
* utilização de `IF`;
* utilização de `SUM`;
* utilização de `CALCULATE`;
* utilização de `LOOKUPVALUE`;
* formatação visual;
* conclusões baseadas nos dados.
