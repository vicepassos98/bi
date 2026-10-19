# AULA 6 — Funções DAX, Filtros e Interatividade

## [DOWNLOAD DO BANCO DE DADOS 🎲🎲🎲](https://docs.google.com/spreadsheets/d/1B_Rq0H7M52ncIJbcHQC4ZxrObzpGM-En/export?format=xlsx&utm_source=chatgpt.com)

## Objetivos da aula

Nesta aula, vamos aprender a:

- Utilizar a função `LOOKUPVALUE` para buscar informações em outras tabelas;
- Utilizar a função `DATEDIFF` para calcular diferenças entre datas;
- Utilizar a função `IF` de forma alinhada para criar múltiplas categorias;
- Trabalhar com filtros no Power BI;
- Alterar e configurar visuais;
- Criar botões interativos para facilitar a navegação no relatório.

---

# 1. Carregando os bancos de dados

Para começar, vamos carregar no Power BI os três bancos de dados disponíveis no arquivo.

Nesta aula, trabalharemos principalmente com as tabelas:

- `Vendas`
- `Produtos`
- `Clientes`

Antes de começar a criar nossas análises, é importante verificar como as tabelas estão estruturadas e quais informações estão disponíveis em cada uma delas.

---

# 2. Função LOOKUPVALUE

## O que é?

A função `LOOKUPVALUE` é utilizada para buscar um determinado valor em outra tabela com base em uma informação correspondente.

Ela é especialmente útil quando não conseguimos utilizar diretamente a função `RELATED` ou quando não existe um relacionamento adequado entre as tabelas.

Podemos pensar na `LOOKUPVALUE` como uma função semelhante ao `PROCX` do Excel. Em alguns casos, seu funcionamento também pode lembrar a combinação entre `ÍNDICE` e `CORRESP`.

A principal diferença é que, enquanto a função `RELATED` depende de um relacionamento entre as tabelas, a `LOOKUPVALUE` consegue realizar a busca utilizando uma coluna de referência.

## Sintaxe

```DAX
LOOKUPVALUE(
    Coluna_Resultado,
    Coluna_Busca,
    Valor_Procurado
)
```

### Argumentos

| Argumento | Descrição |
|---|---|
| `Coluna_Resultado` | Coluna que contém o valor que queremos retornar |
| `Coluna_Busca` | Coluna utilizada para realizar a busca |
| `Valor_Procurado` | Valor que será utilizado como referência para encontrar o resultado |

> **Observação:** Para utilizar a função `LOOKUPVALUE`, não é obrigatório existir um relacionamento entre as tabelas.

---

## Exemplo

Imagine que temos a tabela `Vendas` com o código de um produto:

```text
CD_PRODUTO_1
```

E temos a tabela `Produtos`, contendo:

```text
CD_PRODUTO
Preço Venda
```

Podemos utilizar o código do produto na tabela `Vendas` para buscar seu preço na tabela `Produtos`.

A fórmula seria:

```DAX
Preço_1 =
LOOKUPVALUE(
    PRODUTOS[Preço Venda],
    PRODUTOS[CD_PRODUTO],
    VENDAS[CD_PRODUTO_1]
)
```

A lógica é:

> "Procure na tabela `PRODUTOS` o código que corresponde ao código do produto vendido e retorne o seu preço de venda."

---

# Exercício 1 — Buscando os preços dos produtos

Na tabela `Vendas`, crie quatro novas colunas:

- `Preço_1`
- `Preço_2`
- `Preço_3`
- `Preço_4`

Cada coluna deverá buscar o preço de venda correspondente ao produto utilizando a função `LOOKUPVALUE`.

Por exemplo:

```DAX
Preço_1 =
LOOKUPVALUE(
    PRODUTOS[Preço Venda],
    PRODUTOS[CD_PRODUTO],
    VENDAS[CD_PRODUTO_1]
)
```

Repita o processo para os demais produtos.

Depois, repita o procedimento para buscar o custo de cada produto.

Crie as colunas:

- `Custo_1`
- `Custo_2`
- `Custo_3`
- `Custo_4`

---

# Exercício 2 — Calculando o preço total da compra

Depois de encontrar o preço de cada produto, crie uma nova coluna chamada:

```text
Preço da Compra
```

Essa coluna deverá representar a soma dos preços dos quatro produtos.

A lógica será:

```DAX
Preço da Compra =
[Preço_1] +
[Preço_2] +
[Preço_3] +
[Preço_4]
```

Agora faça o mesmo para os custos:

```text
Custo da Compra
```

Somando:

```DAX
Custo da Compra =
[Custo_1] +
[Custo_2] +
[Custo_3] +
[Custo_4]
```

Por fim, crie a coluna:

```text
Margem da Compra
```

A margem será calculada pela diferença entre o preço da compra e o custo da compra:

```DAX
Margem da Compra =
[Preço da Compra] -
[Custo da Compra]
```

---

# 3. Função DATEDIFF

## O que é?

A função `DATEDIFF` é utilizada para calcular a diferença entre duas datas.

Ela pode ser utilizada para descobrir, por exemplo:

- Quantos anos uma pessoa tem;
- Quantos meses se passaram entre duas datas;
- Quantos dias existem entre dois eventos;
- Quanto tempo um cliente está cadastrado;
- Quanto tempo levou um processo.

A função também existe no Excel, embora seu uso seja menos comum.

## Sintaxe

```DAX
DATEDIFF(
    Data_Inicial,
    Data_Final,
    Unidade_de_Tempo
)
```

### Argumentos

| Argumento | Descrição |
|---|---|
| `Data_Inicial` | Data utilizada como ponto de partida |
| `Data_Final` | Data utilizada como ponto final |
| `Unidade_de_Tempo` | Unidade utilizada para calcular a diferença |

Algumas unidades possíveis são:

```text
YEAR
MONTH
DAY
```

---

## Exemplo

Na tabela `Clientes`, podemos calcular a idade de cada cliente comparando sua data de nascimento com a data atual.

A fórmula será:

```DAX
Idade =
DATEDIFF(
    CLIENTES[Data_Nascimento],
    TODAY(),
    YEAR
)
```

A função `TODAY()` retorna a data atual.

Assim, a fórmula calcula quantos anos completos existem entre a data de nascimento e o dia atual.

---

# Exercício 3 — Calculando a idade dos clientes

Na tabela `Clientes`, crie uma nova coluna chamada:

```text
Idade
```

Utilize a função `DATEDIFF` para calcular a idade de cada cliente.

Utilize:

- A data de nascimento como data inicial;
- A data atual como data final;
- `YEAR` como unidade de tempo.

---

# 4. Função IF alinhada

## O que é?

A função `IF` é utilizada para criar condições.

Ela verifica se uma determinada condição é verdadeira ou falsa e retorna um resultado diferente para cada situação.

Sua estrutura básica é:

```DAX
IF(
    Condição,
    Resultado_se_verdadeiro,
    Resultado_se_falso
)
```

Porém, podemos colocar uma nova função `IF` dentro do resultado falso de outra função `IF`.

Essa técnica é chamada de **IF alinhado** ou **IF aninhado**.

Ela permite criar mais de duas categorias.

---

## Exemplo

Vamos classificar nossos clientes em três grupos:

- Jovem Adulto: abaixo de 35 anos;
- Meia Idade: de 35 até 55 anos;
- Idoso: acima de 55 anos.

Podemos utilizar:

```DAX
Faixa Etária =
IF(
    [Idade] < 35,
    "Jovem Adulto",
    IF(
        [Idade] < 55,
        "Meia Idade",
        "Idoso"
    )
)
```

A lógica funciona da seguinte maneira:

1. Primeiro, verificamos se a idade é menor que 35.
2. Se for, classificamos como `Jovem Adulto`.
3. Caso contrário, verificamos se a idade é menor que 55.
4. Se for, classificamos como `Meia Idade`.
5. Caso contrário, classificamos como `Idoso`.

Dessa forma, conseguimos criar três categorias utilizando duas funções `IF`.

---

# Exercício 4 — Criando faixas etárias

Na tabela `Clientes`, crie uma nova coluna chamada:

```text
Faixa Etária
```

Classifique os clientes de acordo com sua idade:

| Idade | Faixa Etária |
|---|---|
| Abaixo de 30 anos | Jovem  |
| 30 até 50 anos | Adulto |
| Acima de 50 anos | Idoso |

Utilize uma estrutura de `IF` alinhado para realizar a classificação.

---

# 5. Criando visualizações

Agora que temos as informações sobre a idade e a faixa etária dos clientes, podemos criar visualizações para entender melhor a origem dos nossos clientes.

Vamos criar um **gráfico de colunas empilhadas**.

Configure o visual da seguinte maneira:

| Campo | Configuração |
|---|---|
| Eixo X | Estado de Residência |
| Eixo Y | Contagem de Estado de Residência |
| Legenda | Faixa Etária |

O resultado permitirá visualizar a distribuição dos clientes por estado e, ao mesmo tempo, observar a composição das diferentes faixas etárias.

---

# 6. Filtros

Os filtros permitem limitar os dados apresentados em um visual ou relatório.

Por exemplo, podemos utilizar a coluna `Faixa Etária` como filtro e selecionar apenas os clientes classificados como:

```text
Idoso
```

Nesse caso, o gráfico será atualizado e exibirá somente os clientes pertencentes a essa categoria.

---

## Filtros x Segmentação de Dados

Os filtros e as segmentações de dados possuem funções semelhantes, mas apresentam diferenças na forma como o usuário interage com eles.

A **segmentação de dados (Slicer)** aparece diretamente no relatório como um elemento visual. O usuário pode clicar nas opções disponíveis para alterar os dados apresentados.

Já os **filtros** podem ser configurados no painel de filtros do Power BI. Nesse caso, não é necessário criar um elemento visual específico para que o filtro seja aplicado.

Por exemplo, podemos configurar um filtro para mostrar somente clientes da faixa etária:

```text
Idoso
```

Assim, os visuais afetados pelo filtro passarão a exibir somente os clientes dessa categoria.

---

# Exercício 5 — Aplicando filtros

Utilize o campo:

```text
Faixa Etária
```

como filtro do relatório ou do visual.

Configure o filtro para exibir apenas:

```text
Idoso
```

Observe como o gráfico é atualizado.

Depois, altere o filtro para:

```text
Jovem Adulto
```

e, posteriormente:

```text
Meia Idade
```

Compare os resultados e observe como a distribuição dos clientes se modifica.

---

# 7. Alterando visuais

O Power BI permite alterar diversos elementos dos visuais depois que eles são criados.

Podemos modificar, por exemplo:

- Tipo de gráfico;
- Título;
- Eixos;
- Legenda;
- Rótulos de dados;
- Formatação;
- Cores;
- Tamanho;
- Posição;
- Campos utilizados no visual.

Uma mesma informação pode ser apresentada de diferentes maneiras.

Por exemplo, podemos transformar um gráfico de colunas empilhadas em:

- Gráfico de barras;
- Gráfico de colunas agrupadas;
- Gráfico de pizza;
- Gráfico de rosca.

A escolha do visual deve considerar qual formato facilita melhor a compreensão dos dados.

---

# Exercício 6 — Modificando um visual

Selecione o gráfico criado anteriormente.

Experimente alterar:

1. O tipo de gráfico;
2. A posição da legenda;
3. O título;
4. A exibição dos rótulos de dados;
5. Os campos utilizados no eixo;
6. A formatação geral do visual.

Depois, escolha o formato que você considera mais adequado para representar a distribuição dos clientes por faixa etária e estado.

---

# 8. Botões interativos

O Power BI permite utilizar botões para criar relatórios mais interativos.

Os botões podem ser utilizados para:

- Navegar entre páginas;
- Voltar para uma página anterior;
- Abrir links;
- Controlar elementos do relatório;
- Criar menus de navegação.

Uma das opções disponíveis é o **Navegador de Páginas**.

Ele permite criar automaticamente um menu com botões correspondentes às páginas do relatório.

Assim, o usuário pode navegar pelo dashboard clicando nos botões.

---

## Criando um menu de navegação

Para criar um menu principal para o dashboard:

1. Acesse a guia **Inserir**;
2. Selecione **Botões**;
3. Escolha a opção de **Navegador de Páginas**;
4. Posicione o navegador no relatório;
5. Ajuste sua aparência e formatação.

O Power BI criará botões correspondentes às páginas existentes no relatório.

Ao clicar em um botão, o usuário será direcionado automaticamente para a página correspondente.

---

# Exercício 7 — Criando um menu interativo

Crie um menu de navegação para o seu dashboard utilizando o **Navegador de Páginas**.

O menu deverá permitir que o usuário navegue entre as diferentes páginas do relatório.

Depois:

1. Altere a posição do menu;
2. Modifique sua aparência;
3. Ajuste o tamanho dos botões;
4. Personalize os textos;
5. Teste a navegação entre as páginas.

---

# Desafio Final

Agora vamos combinar os conhecimentos aprendidos nesta aula.

Crie uma página de dashboard que apresente informações sobre os clientes e suas compras.

O dashboard deverá conter:

- Um gráfico com a distribuição dos clientes por estado;
- Uma visualização por faixa etária;
- Um filtro ou segmentação de dados por faixa etária;
- Informações sobre preço e custo das compras;
- A margem das compras;
- Um menu de navegação entre as páginas.

Utilize os recursos aprendidos nesta aula para tornar o relatório interativo e facilitar a exploração dos dados.

## Recursos utilizados

Nesta aula, utilizamos:

- `LOOKUPVALUE`
- `DATEDIFF`
- `TODAY`
- `IF`
- IF alinhado
- Filtros
- Segmentação de dados
- Gráficos de colunas empilhadas
- Formatação de visuais
- Botões
- Navegador de páginas
