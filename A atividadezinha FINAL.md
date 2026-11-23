# VICE CITY EXPRESS - Trabalho Final de Power BI

**Projeto:** Análise de Entregas, Clientes, Veículos e Metas por Região

---

# 1. Visão Geral do Projeto

Neste trabalho final, vamos construir um **Dashboard de Logística** utilizando o Power BI.

A empresa fictícia **VICE CITY Express** deseja analisar:

* Entregas realizadas;
* Clientes e contratos;
* Desempenho dos veículos;
* Faturamento;
* Atrasos nas entregas;
* Quilômetros percorridos;
* Metas de faturamento;
* Metas de pontualidade;
* Desempenho por região.

O projeto será dividido em **três grandes etapas**:

1. **Preparação dos dados no Power Query**
2. **Criação de cálculos utilizando DAX**
3. **Construção do Dashboard**

---

# 2. Bases de Dados

[PARA BAIXAR O BANCO DE DADOS CLIQUE AQUI](https://docs.google.com/spreadsheets/d/1amYMWey4tU46yEQwCe4cHTwCvzy3bEle/export?format=xlsx)

O projeto utiliza três tabelas que estão no arquivo Excel e uma quarta tabela que será criada diretamente no Power BI.

| Tabela           | Descrição                           | Principais Colunas                                                               |
| ---------------- | ----------------------------------- | -------------------------------------------------------------------------------- |
| **Entregas**     | Registros de todas as entregas      | ID_Entrega, Datas, ID_Cliente, ID_Veiculo, Peso_kg, Distancia_km, Status_Entrega |
| **Clientes**     | Cadastro dos clientes e contratos   | ID_Cliente, Nome, Segmento, Cidade_Origem, Contrato_Premium                      |
| **Veiculos**     | Informações da frota                | ID_Veiculo, Tipo_Veiculo, Capacidade_kg, Custo_km, Em_Manutencao                 |
| **Metas_Regiao** | Metas de faturamento e pontualidade | Regiao_Destino, Meta_Faturamento, Meta_%_Pontualidade                            |

> **Importante:** a tabela `Metas_Regiao` **não existe no arquivo Excel**. Ela será criada manualmente dentro do Power BI.

---

# 3. Etapa 1 — Importação e Transformação dos Dados

## 3.1 Importando o arquivo Excel

1. Abra o **Power BI Desktop**.
2. Acesse:

**Página Inicial → Obter Dados → Excel**

3. Selecione o arquivo:

```text
BD_VCExpress.xlsx
```

4. Na janela de navegação, selecione:

* Entregas
* Clientes
* Veiculos

5. Clique em:

**Transformar Dados**

Isso abrirá o **Power Query**.

---

# 4. Verificando os Dados no Power Query

Antes de criar os cálculos, precisamos garantir que os dados estão corretamente configurados.

Essa etapa é importante porque **dados com tipos incorretos podem gerar erros nos cálculos e nos gráficos**.

---

## 4.1 Tabela Entregas

A tabela possui:

* 11 colunas
* 100 registros

### Colunas de Data

As seguintes colunas devem estar configuradas como **Data**:

* `Data_Envio`
* `Data_Prevista`
* `Data_Entrega`

Caso uma data apareça como um número, por exemplo:

```text
45350
```

selecione a coluna e altere o tipo para:

**Data**

---

### Status_Entrega

Confira os valores da coluna:

```text
Entregue
Entregue com Atraso
Extraviado
```

### Atenção!

Existem entregas que possuem `Data_Entrega` em branco:

```text
ENT0054
ENT0064
ENT0094
```

Essas entregas representam **entregas extraviadas**.

**Não exclua essas linhas.**

Elas serão importantes para a análise de status das entregas.

---

### Peso e distância

As colunas abaixo devem ser configuradas como:

**Número Decimal**

```text
Peso_kg
Distancia_km
```

---

# 5. Tabela Veiculos

A tabela possui:

* 8 colunas
* 10 registros

Confira:

### Custo_km

Tipo:

```text
Número Decimal
```

Valores utilizados:

```text
0,90
1,20
2,80
3,50
```

### Em_Manutencao

Tipo:

```text
Texto
```

Valores:

```text
Sim
Não
```

Os seguintes veículos estão em manutenção:

```text
VEI007
VEI008
VEI009
VEI010
```

### Km_Rodados_Total

Tipo:

```text
Número Inteiro
```

---

# 6. Tabela Clientes

A tabela possui:

* 5 colunas
* 20 registros

### Contrato_Premium

Tipo:

```text
Texto
```

Valores:

```text
Sim
Não
```

Os clientes Premium são:

| ID_Cliente | Cliente               |
| ---------- | --------------------- |
| CLI001     | Distribuidora Alpha   |
| CLI015     | Omicron Supermercados |
| CLI019     | Tau Ferramentas       |
| CLI020     | Upsilon Serviços      |

Verifique também se todos os clientes de:

```text
CLI001
```

até:

```text
CLI020
```

estão presentes.

---

# 7. Aplicando as Transformações

Depois de conferir todas as tabelas:

**Página Inicial → Fechar e Aplicar**

O Power BI carregará os dados para o modelo.

---

# 8. Criando a Tabela Metas_Regiao

A tabela de metas **não está no Excel**.

Precisamos criá-la manualmente.

Acesse:

**Página Inicial → Inserir Dados**

Crie a tabela abaixo:

| Regiao_Destino | Meta_Faturamento | Meta_%_Pontualidade |
| -------------- | ---------------: | ------------------: |
| Sudeste        |            80000 |                 70% |
| Sul            |            60000 |                 70% |
| Nordeste       |            50000 |                 65% |
| Centro-Oeste   |            55000 |                 65% |
| Norte          |            45000 |                 60% |

Nomeie a tabela como:

```text
Metas_Regiao
```

Depois clique em:

**Carregar**

---

# 9. Criando os Relacionamentos

Agora precisamos informar ao Power BI como as tabelas estão relacionadas.

Acesse a visualização:

**Modelo**

Crie os seguintes relacionamentos:

| Tabela   | Coluna         | Tabela       | Coluna         |
| -------- | -------------- | ------------ | -------------- |
| Entregas | ID_Cliente     | Clientes     | ID_Cliente     |
| Entregas | ID_Veiculo     | Veiculos     | ID_Veiculo     |
| Entregas | Regiao_Destino | Metas_Regiao | Regiao_Destino |

O relacionamento pode ser visualizado como:

```text
Clientes
    │
    │ ID_Cliente
    ▼
Entregas
    │
    │ ID_Veiculo
    ▼
Veiculos

Entregas
    │
    │ Regiao_Destino
    ▼
Metas_Regiao
```

> **Atenção:** confira se os relacionamentos foram criados corretamente antes de continuar.

---


# 10. Etapa 2 — Colunas Calculadas com DAX

Agora começaremos a utilizar **DAX**.

As colunas calculadas são criadas diretamente dentro das tabelas.

Para criar uma coluna:

**Exibição de Dados → selecione a tabela → Nova Coluna**

> **Atenção:** nesta etapa, as fórmulas **não serão fornecidas**. Cada item descreve a lógica que deve ser construída. Escolha as funções DAX adequadas e escreva as fórmulas por conta própria.

---

# 11. Coluna 1 — Custo_KM_Veiculo

### Objetivo

Trazer para a tabela `Entregas` o custo por quilômetro do veículo utilizado em cada entrega.

### Como fazer

Na tabela `Entregas`, crie uma nova coluna calculada com o nome `Custo_KM_Veiculo`. O valor de cada linha deve ser o `Custo_km` do veículo que realizou aquela entrega.

Como o `Custo_km` está em **outra tabela** (`Veiculos`), utilize a função do DAX que busca o valor de uma coluna em uma tabela relacionada, aproveitando o relacionamento criado entre `Entregas` e `Veiculos` pela coluna `ID_Veiculo`.

Ao final, confira se os valores da coluna correspondem aos custos da tabela abaixo.

---

## Custos por tipo de veículo

| Tipo de Veículo      | Veículos                       | Custo por km |
| -------------------- | ------------------------------ | -----------: |
| Utilitário           | VEI002, VEI003, VEI007, VEI008 |      R$ 0,90 |
| Van                  | VEI006, VEI010                 |      R$ 1,20 |
| Caminhão Baú         | VEI009                         |      R$ 2,80 |
| Caminhão Refrigerado | VEI001, VEI004, VEI005         |      R$ 3,50 |

---

# 12. Coluna 2 — Valor_Frete

Agora vamos calcular o valor bruto do frete.

### Como fazer

Na tabela `Entregas`, crie uma coluna calculada chamada `Valor_Frete`. O valor bruto de cada entrega é a **soma de duas partes**:

1. **Custo do percurso:** multiplique o custo por quilômetro do veículo (coluna criada no passo anterior) pela distância percorrida na entrega.
2. **Custo pelo peso:** multiplique o peso da carga, em quilos, pelo valor fixo de **R$ 0,25 por quilo**.

Some as duas partes. O resultado de cada linha é o valor bruto do frete daquela entrega.

---

# 13. Coluna 3 — Premium_Cliente

Agora precisamos identificar se o cliente de cada entrega possui contrato Premium.

### Como fazer

Na tabela `Entregas`, crie uma coluna calculada chamada `Premium_Cliente` que traga, para cada entrega, a informação de `Contrato_Premium` do cliente correspondente (tabela `Clientes`).

Essa será uma **coluna auxiliar**: ela permitirá identificar facilmente quais entregas pertencem a clientes Premium. Assim como na Coluna 1, o dado está em outra tabela, então pense em qual função permite buscá-lo por meio do relacionamento.

---

# 14. Coluna 4 — Preco_Final

Clientes Premium recebem **5% de desconto** sobre o valor do frete.

### Como fazer

Na tabela `Entregas`, crie uma coluna calculada chamada `Preco_Final` com a seguinte regra de decisão, avaliada linha a linha:

* Se o cliente da entrega **for Premium**, o preço final é o valor do frete com o desconto de 5% aplicado (ou seja, 95% do valor do frete);
* Caso contrário, o preço final é o próprio valor do frete, sem desconto.

Utilize a função condicional do DAX e a coluna auxiliar criada no passo anterior para testar se o cliente é Premium.

> **Desafio extra:** é possível chegar ao mesmo resultado sem a coluna auxiliar `Premium_Cliente`. Pense em como fazer o teste buscando o contrato do cliente diretamente dentro da condição.

---

# 15. Etapa 3 — Criando a Tabela de Medidas

Agora vamos criar as principais **medidas DAX** do projeto.

Uma boa prática é centralizar as medidas em uma tabela específica.

### Como fazer

1. Acesse **Modelagem → Nova Tabela**.
2. Crie uma tabela chamada `Medidas` que contenha apenas **uma linha e uma coluna**, com um valor qualquer (por exemplo, zero). Utilize uma função DAX que construa uma tabela de uma única linha.
3. Pressione **Enter** e confirme que a tabela `Medidas` apareceu no painel de dados.

Para criar uma medida dentro dela:

**Botão direito na tabela Medidas → Nova Medida**

> **Importante:** estamos criando uma **Medida**, e não uma Coluna Calculada. Medidas são recalculadas conforme os filtros aplicados no relatório.

---

# 16. Medida 1 — Faturamento_Total

O faturamento total é o **somatório dos preços finais** de todas as entregas.

### Como fazer

Crie a medida `Faturamento_Total` somando a coluna `Preco_Final` da tabela `Entregas`.

Depois formate a medida como **Moeda (R$)**, com **2 casas decimais**.

Essa será uma das principais medidas do Dashboard.

---

# 17. Medida 2 — Ticket_Medio_Entrega

O ticket médio representa o valor médio de cada entrega.

### Como fazer

Crie a medida `Ticket_Medio_Entrega` calculando a **média** da coluna `Preco_Final` da tabela `Entregas`.

Formate como **Moeda (R$)**.

---

# 18. Medida 3 — Atrasos

Primeiro vamos criar uma medida auxiliar para contar as entregas atrasadas.

### Como fazer

Crie a medida `Atrasos` que **conte as linhas da tabela `Entregas`**, considerando **apenas** aquelas cujo status seja `Entregue com Atraso`.

Para isso, será necessário combinar uma função de contagem de linhas com a função que altera o contexto de filtro da medida, aplicando a condição sobre a coluna `Status_Entrega`.

---

# 19. Medida 4 — % de Entregas com Atraso

Agora vamos descobrir qual porcentagem das entregas apresentou atraso.

### Como fazer

Crie a medida `%_Entregas_com_Atraso` dividindo a quantidade de entregas atrasadas (medida `Atrasos`) pelo **total de entregas** (contagem de linhas da tabela `Entregas`).

Formate como **Percentual**, com **1 casa decimal**.

### Exemplo

Se existirem:

```text
100 entregas
```

e:

```text
25 atrasos
```

teremos:

```text
25 / 100 = 25%
```

---

# 20. Medida 5 — Quilometros_Rodados

Vamos calcular a distância total percorrida.

### Como fazer

Crie a medida `Quilometros_Rodados` somando a coluna `Distancia_km` da tabela `Entregas`.

Formate como **Número inteiro**.

---

# 21. Medida 6 — Valor_KM_Medio

Agora vamos calcular o custo médio por quilômetro das entregas.

### Como fazer

Crie a medida `Valor_KM_Medio` calculando a **média** da coluna `Custo_KM_Veiculo` da tabela `Entregas`.

Formate como **Moeda (R$)**, com 2 casas decimais.

---

# 22. Medidas de Meta

Agora vamos trabalhar com as metas definidas por região.

## Meta de Faturamento

Crie a medida `Meta_Faturamento` somando a coluna `Meta_Faturamento` da tabela `Metas_Regiao`.

---

## Meta de Pontualidade

Crie a medida `Meta_Pontualidade` calculando a **média** da coluna `Meta_%_Pontualidade` da tabela `Metas_Regiao`.

Formate como **Percentual**.

> **Pergunta para reflexão:** por que usamos soma na meta de faturamento e média na meta de pontualidade?

---

## Percentual de Entregas no Prazo

Crie a medida `%_Entregas_no_Prazo` a partir da medida `%_Entregas_com_Atraso`. Como os dois percentuais são complementares (juntos representam 100% das entregas), basta descobrir quanto falta para chegar a 100%.

Formate como **Percentual**.

---

# 23. Medida — Veiculos_em_Manutencao

Na tabela `Veiculos`, precisamos descobrir quantos veículos estão em manutenção.

### Como fazer

Crie a medida `Veiculos_em_Manutencao` que **conte as linhas da tabela `Veiculos`** em que a coluna `Em_Manutencao` seja igual a `Sim`.

Existem mais de uma forma de fazer isso em DAX: você pode filtrar a tabela antes de contar as linhas, ou alterar o contexto de filtro da contagem. Escolha a que preferir.

O resultado esperado é:

```text
4 veículos
```

São eles:

```text
VEI007
VEI008
VEI009
VEI010
```

---

# 24. Construção do Dashboard

Agora vamos transformar todos os cálculos em um painel visual.

O Dashboard terá **4 páginas**:

| Página | Nome                | Foco                      |
| ------ | ------------------- | ------------------------- |
| 1      | Capa                | Apresentação e navegação  |
| 2      | Visão Geral         | KPIs, faturamento e metas |
| 3      | Análise de Entregas | Atrasos e desempenho      |
| 4      | Frota e Veículos    | Desempenho da frota       |

---

# 25. Página 1 — Capa

Crie uma página inicial para o projeto.

Inclua:

```text
VICE CITY EXPRESS

Dashboard de Logística

Análise de Entregas, Clientes,
Veículos e Metas por Região
```

Adicione também botões de navegação para:

* Visão Geral
* Análise de Entregas
* Frota e Veículos

A ideia é criar uma apresentação semelhante a um sistema ou aplicativo.

---

# 26. Página 2 — Visão Geral

A segunda página será o principal painel gerencial.

## Cards de KPI

Crie cartões para:

### Faturamento

```text
Faturamento_Total
```

### Ticket médio

```text
Ticket_Medio_Entrega
```

### Quilômetros rodados

```text
Quilometros_Rodados
```

### Entregas com atraso

```text
%_Entregas_com_Atraso
```

---

# 27. Indicadores com Meta

Crie dois visuais do tipo **KPI**.

## KPI 1 — Faturamento

Configure:

```text
Valor:
Faturamento_Total
```

Meta:

```text
Meta_Faturamento
```

---

## KPI 2 — Pontualidade

Configure:

```text
Valor:
%_Entregas_no_Prazo
```

Meta:

```text
Meta_Pontualidade
```

---

# 28. Faturamento x Meta por Região

Crie um:

**Gráfico de Barras Agrupadas**

### Eixo

```text
Regiao_Destino
```

### Valores

```text
Faturamento_Total
Meta_Faturamento
```

O gráfico permitirá comparar:

```text
Quanto faturamos
        X
Quanto deveríamos faturar
```

em cada região.

---

# 29. Página 3 — Análise de Entregas

Nesta página vamos investigar o comportamento das entregas.

---

## Gráfico de Pizza — Status das Entregas

Crie um gráfico de pizza.

### Legenda

```text
Status_Entrega
```

### Valores

```text
Contagem de ID_Entrega
```

O gráfico deverá apresentar:

* Entregue
* Entregue com Atraso
* Extraviado

---

# 30. Entregas por Mês

Crie um gráfico de linhas.

### Eixo X

```text
Data_Envio
```

Agrupe por:

```text
Mês
```

### Valores (Eixo Y)

```text
Contagem de entregas
```

Também utilize:

(Eixo y Secundário)
```text
%_Entregas_com_Atraso
```

para analisar a evolução dos atrasos.

> **Dica:** se o Power BI criar automaticamente uma hierarquia de datas, utilize o nível de mês adequado para a análise.

---

# 31. Tabela Detalhada

Crie uma tabela contendo:

| Campo          |
| -------------- |
| ID_Entrega     |
| Nome_Cliente   |
| Regiao_Destino |
| Tipo_Carga     |
| Preco_Final    |
| Status_Entrega |

---

# 32. Página 4 — Frota e Veículos

Agora vamos analisar o desempenho da frota.

---

# 33. Faturamento por Tipo de Veículo

Crie um:

**Gráfico de Barras**

### Eixo

```text
Tipo_Veiculo
```

### Valores

```text
Faturamento_Total
```

O Power BI utilizará o relacionamento entre:

```text
Entregas
      ↓
Veiculos
```

para identificar o tipo de veículo utilizado em cada entrega.

---

# 34. Veículos em Manutenção

Crie um cartão utilizando:

```DAX
Veiculos_em_Manutencao
```

O resultado esperado é:

```text
4
```

---

# 35. Gráfico de Dispersão — Distância x Frete

Crie um:

**Gráfico de Dispersão**

Configure:

### Eixo X

```text
Distancia_km
```

### Eixo Y

```text
Valor_Frete
```

### Legenda

```text
Tipo_Veiculo
```

Esse gráfico permitirá observar a relação entre:

```text
Distância percorrida
        X
Valor do frete
```

Também será possível identificar diferenças entre os tipos de veículos.

---

# 36. Filtros e Segmentações

Adicione segmentações de dados ao Dashboard.

Utilize:

```text
Regiao_Destino
Status_Entrega
Data_Envio
Contrato_Premium
```

Esses filtros deverão permitir que o usuário explore o Dashboard de forma interativa.

Por exemplo:

### Filtrar apenas a região Sudeste

```text
Regiao_Destino = Sudeste
```

O Dashboard deverá atualizar automaticamente os indicadores e gráficos relacionados.

---

# 37. Checklist Final

Antes de entregar o projeto, confira se todas as etapas foram realizadas.

## Dados

* [ ] Excel importado
* [ ] Entregas conferida
* [ ] Clientes conferida
* [ ] Veiculos conferida
* [ ] Tipos de dados corrigidos
* [ ] Entregas extraviadas mantidas
* [ ] Metas_Regiao criada

## Modelo

* [ ] Relacionamento Entregas → Clientes
* [ ] Relacionamento Entregas → Veiculos
* [ ] Relacionamento Entregas → Metas_Regiao

## DAX

* [ ] Custo_KM_Veiculo
* [ ] Valor_Frete
* [ ] Premium_Cliente
* [ ] Preco_Final
* [ ] Faturamento_Total
* [ ] Ticket_Medio_Entrega
* [ ] Atrasos
* [ ] %_Entregas_com_Atraso
* [ ] Quilometros_Rodados
* [ ] Valor_KM_Medio
* [ ] Meta_Faturamento
* [ ] Meta_Pontualidade
* [ ] %_Entregas_no_Prazo
* [ ] Veiculos_em_Manutencao

## Dashboard

* [ ] Página de Capa
* [ ] Página Visão Geral
* [ ] Página Análise de Entregas
* [ ] Página Frota e Veículos
* [ ] Cards de KPI
* [ ] Indicadores com metas
* [ ] Gráficos
* [ ] Tabela detalhada
* [ ] Segmentações de dados
* [ ] Botões de navegação

---



# 38. Entrega do Projeto

O projeto final deverá apresentar:

> **Um Dashboard interativo capaz de transformar os dados da Vice City Express em informações úteis para tomada de decisão.**

O objetivo não é apenas criar gráficos.

É necessário utilizar o Power BI para responder:

* Quanto a empresa fatura?
* Quais regiões apresentam melhor desempenho?
* A empresa está atingindo suas metas?
* Quantas entregas atrasam?
* Quais veículos geram mais faturamento?
* Quantos veículos estão em manutenção?
* Como os clientes Premium impactam o preço?
* Existe relação entre distância e valor do frete?

## Resultado esperado

Ao final do projeto, o usuário deverá conseguir abrir o Dashboard, aplicar filtros e **explorar os dados para encontrar respostas sem precisar consultar diretamente as tabelas originais**.

---

# AO TERMINAR, REVISE TODO O DASHBOARD. E ENVIE O ARQUIVO COMO ANEXO PARA O E-MAIL: vicepassos98@gmail.com
## No Assunto insira os nomes dos alunos
No corpo do e-mail diga "Power BI é incrível" e depois indique qual é o seu [favorito](https://www.instagram.com/reel/DbHQawzIw1I) (Teste contra IA)

**Power BI — Trabalho Final**

**Professor: Victor Passos, aka VICEpassinhos**
