# LOGÍSTICA EXPRESS

## Trabalho Final de Power BI

**Professor:** Victor Passos
**Projeto:** Análise de Entregas, Clientes, Veículos e Metas por Região

---

# 1. Visão Geral do Projeto

Neste trabalho final, vamos construir um **Dashboard de Logística** utilizando o Power BI.

A empresa fictícia **Logística Express** deseja analisar:

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
BD_LogisticaExpress.xlsx
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

---

# 11. Coluna 1 — Custo_KM_Veiculo

### Objetivo

Precisamos trazer para a tabela `Entregas` o custo por quilômetro do veículo utilizado em cada entrega.

Para isso, utilizaremos a função:

```DAX
RELATED
```

Crie uma nova coluna na tabela `Entregas`:

```DAX
Custo_KM_Veiculo =
RELATED(Veiculos[Custo_km])
```

### Como funciona?

A função `RELATED` utiliza o relacionamento entre as tabelas.

O Power BI identifica o veículo da entrega e busca o seu respectivo custo por quilômetro.

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

O cálculo considera:

* Custo por quilômetro;
* Distância percorrida;
* Peso da carga.

Crie a coluna:

```DAX
Valor_Frete =
Entregas[Custo_KM_Veiculo] * Entregas[Distancia_km]
    + Entregas[Peso_kg] * 0.25
```

### Entendendo a fórmula

O valor do frete é composto por duas partes:

```text
Custo do percurso
+
Custo pelo peso
```

Ou seja:

```text
Custo_KM_Veiculo × Distancia_km
```

mais:

```text
Peso_kg × 0,25
```

O resultado será o:

```text
Valor_Frete
```

---

# 13. Coluna 3 — Premium_Cliente

Agora precisamos identificar se o cliente possui contrato Premium.

Crie a coluna:

```DAX
Premium_Cliente =
RELATED(Clientes[Contrato_Premium])
```

Essa será uma **coluna auxiliar**.

Ela permitirá identificar facilmente quais entregas pertencem a clientes Premium.

---

# 14. Coluna 4 — Preco_Final

Clientes Premium recebem:

```text
5% de desconto
```

Crie a coluna:

```DAX
Preco_Final =
IF(
    Entregas[Premium_Cliente] = "Sim",
    Entregas[Valor_Frete] * 0.95,
    Entregas[Valor_Frete]
)
```

---

## Entendendo o IF

A lógica é:

```text
Cliente é Premium?
       │
   ┌───┴───┐
  SIM     NÃO
   │        │
5% desc.  preço cheio
```

Se o cliente for Premium:

```text
Valor_Frete × 0,95
```

Caso contrário:

```text
Valor_Frete
```

Também seria possível fazer o cálculo diretamente utilizando `RELATED`:

```DAX
Preco_Final =
IF(
    RELATED(Clientes[Contrato_Premium]) = "Sim",
    Entregas[Valor_Frete] * 0.95,
    Entregas[Valor_Frete]
)
```

---

# 15. Etapa 3 — Criando a Tabela de Medidas

Agora vamos criar as principais **medidas DAX** do projeto.

Uma boa prática é centralizar as medidas em uma tabela específica.

Acesse:

**Modelagem → Nova Tabela**

Digite:

```DAX
Medidas = ROW("x", 0)
```

Pressione **Enter**.

Agora teremos uma tabela chamada:

```text
Medidas
```

Para criar uma medida:

**Botão direito na tabela Medidas → Nova Medida**

> **Importante:** estamos criando uma **Medida**, e não uma Coluna Calculada.

---

# 16. Medida 1 — Faturamento_Total

O faturamento total será a soma dos preços finais de todas as entregas.

```DAX
Faturamento_Total =
SUM(Entregas[Preco_Final])
```

Depois formate a medida como:

```text
Moeda (R$)
```

com:

```text
2 casas decimais
```

Essa será uma das principais medidas do Dashboard.

---

# 17. Medida 2 — Ticket_Medio_Entrega

O ticket médio representa o valor médio de cada entrega.

```DAX
Ticket_Medio_Entrega =
AVERAGE(Entregas[Preco_Final])
```

Formate como:

```text
Moeda (R$)
```

---

# 18. Medida 3 — Atrasos

Primeiro vamos criar uma medida auxiliar para contar as entregas atrasadas.

```DAX
Atrasos =
CALCULATE(
    COUNTROWS(Entregas),
    Entregas[Status_Entrega] = "Entregue com Atraso"
)
```

Essa medida contará apenas as entregas com status:

```text
Entregue com Atraso
```

---

# 19. Medida 4 — % de Entregas com Atraso

Agora vamos descobrir qual porcentagem das entregas apresentou atraso.

```DAX
%_Entregas_com_Atraso =
[Atrasos] / COUNTROWS(Entregas)
```

Formate como:

```text
Percentual
```

com:

```text
1 casa decimal
```

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

```DAX
Quilometros_Rodados =
SUM(Entregas[Distancia_km])
```

Formate como:

```text
Número inteiro
```

---

# 21. Medida 6 — Valor_KM_Medio

Agora vamos calcular o custo médio por quilômetro das entregas.

```DAX
Valor_KM_Medio =
AVERAGE(Entregas[Custo_KM_Veiculo])
```

Formate como:

```text
Moeda (R$)
```

com duas casas decimais.

---

# 22. Medidas de Meta

Agora vamos trabalhar com as metas definidas por região.

## Meta de Faturamento

```DAX
Meta_Faturamento =
SUM(Metas_Regiao[Meta_Faturamento])
```

---

## Meta de Pontualidade

```DAX
Meta_Pontualidade =
AVERAGE(Metas_Regiao[Meta_%_Pontualidade])
```

Formate como:

```text
Percentual
```

---

## Percentual de Entregas no Prazo

Como já temos o percentual de atrasos, podemos calcular o percentual no prazo:

```DAX
%_Entregas_no_Prazo =
1 - [%_Entregas_com_Atraso]
```

Formate como:

```text
Percentual
```

---

# 23. Medida — Veiculos_em_Manutencao

Na tabela `Veiculos`, precisamos descobrir quantos veículos estão em manutenção.

Crie a medida:

```DAX
Veiculos_em_Manutencao =
COUNTROWS(
    FILTER(
        Veiculos,
        Veiculos[Em_Manutencao] = "Sim"
    )
)
```

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
LOGÍSTICA EXPRESS

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

No indicador de atrasos, utilize **formatação condicional**.

Sugestão:

```text
Acima de 40% → vermelho
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

Crie um gráfico de colunas.

### Eixo X

```text
Data_Envio
```

Agrupe por:

```text
Mês
```

### Valores

```text
Contagem de entregas
```

Também utilize:

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

Na coluna:

```text
Preco_Final
```

adicione:

**Barras de dados**

Isso permitirá identificar visualmente quais entregas possuem maior valor.

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
* [ ] Formatação condicional
* [ ] Segmentações de dados
* [ ] Botões de navegação

---

# 38. Desafio Final

Depois de concluir o Dashboard, responda às seguintes perguntas utilizando os próprios dados do Power BI:

### 1. Qual região apresenta o maior faturamento?

```text
Resposta:
```

### 2. Qual região está mais distante de sua meta de faturamento?

```text
Resposta:
```

### 3. Qual é o percentual total de entregas atrasadas?

```text
Resposta:
```

### 4. Qual tipo de veículo gera maior faturamento?

```text
Resposta:
```

### 5. Quantos veículos estão atualmente em manutenção?

```text
Resposta:
```

### 6. Clientes Premium representam uma parcela significativa do faturamento?

```text
Resposta:
```

### 7. Existe relação entre distância percorrida e valor do frete?

```text
Resposta:
```

### 8. A empresa está atingindo sua meta de pontualidade?

```text
Resposta:
```

---

# 39. Entrega do Projeto

O projeto final deverá apresentar:

> **Um Dashboard interativo capaz de transformar os dados da Logística Express em informações úteis para tomada de decisão.**

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

# Fim do Projeto

## LOGÍSTICA EXPRESS

**Power BI — Trabalho Final**

**Professor: Victor Passos**
