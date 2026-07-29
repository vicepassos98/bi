# Trabalhando com Correlação na Prática: Renda x Setores Econômicos

🎲🎲[Download do Banco de dados](https://docs.google.com/spreadsheets/d/1eX2PEHQeL2aUsokLgvH7RKDdO7RNuJk2tPxm2G64fZ8/export?format=csv&gid=2064696384)

## 1. Crrelação


> **Correlação é uma medida que indica o quanto duas variáveis estão relacionadas entre si.**

Quando analisamos duas variáveis, podemos observar diferentes situações.

### Correlação positiva

Acontece quando, em geral, **o aumento de uma variável está associado ao aumento da outra**.

Por exemplo:

Imagine que estamos analisando uma turma de alunos e comparando:

* Horas de estudo por semana;
* Nota média nas provas.

Se observarmos que, de maneira geral, os alunos que estudam mais apresentam notas maiores, podemos dizer que existe uma **correlação positiva** entre as duas variáveis.

Em um gráfico de dispersão, os pontos tendem a formar uma trajetória crescente:

```text
Nota
  ^
  |                 •
  |             •
  |          •
  |       •
  |   •
  +------------------------> Horas de estudo
```

Nesse caso:

> **Mais horas de estudo → tendência de notas maiores**

---

### Correlação negativa

Acontece quando, em geral, **o aumento de uma variável está associado à diminuição da outra**.

Por exemplo:

Imagine que estamos analisando:

* Número de faltas;
* Nota média dos alunos.

Se os alunos que faltam mais tendem a apresentar notas menores, podemos encontrar uma **correlação negativa** entre as variáveis.

```text
Nota
  ^
  |   •
  |      •
  |         •
  |             •
  |                 •
  +------------------------> Número de faltas
```

Nesse caso:

> **Mais faltas → tendência de notas menores**

---

### Correlação próxima de zero

Também podemos encontrar situações em que não existe uma relação linear clara entre as variáveis.

Por exemplo, imagine que analisamos:

* Número de irmãos;
* Nota média dos alunos.

É possível que alunos com muitos ou poucos irmãos apresentem notas muito variadas, sem que exista uma tendência clara.

Nesse caso, os pontos do gráfico podem aparecer espalhados:

```text
Nota
  ^
  |     •       •
  |          •
  |  •             •
  |       •
  |             •      •
  +------------------------> Número de irmãos
```

Nesse cenário, a correlação será próxima de **zero**, indicando que não encontramos uma relação linear clara entre as duas variáveis.

---

## 2. O coeficiente de correlação

Para medir a intensidade e a direção da relação entre duas variáveis, podemos utilizar o **coeficiente de correlação**.

O coeficiente varia entre:

> **-1 e +1**

Podemos interpretar seus valores de maneira simplificada:

| Coeficiente   | Interpretação                          |
| ------------- | -------------------------------------- |
| Próximo de +1 | Forte correlação positiva              |
| Próximo de 0  | Correlação linear fraca ou inexistente |
| Próximo de -1 | Forte correlação negativa              |

Por exemplo:

* **+0,90** → forte relação positiva;
* **+0,60** → relação positiva moderada;
* **+0,10** → relação positiva muito fraca;
* **0,00** → ausência de relação linear;
* **-0,50** → relação negativa moderada;
* **-0,90** → forte relação negativa.

É importante lembrar que esses valores são uma forma simplificada de interpretação. A classificação de uma correlação como "fraca", "moderada" ou "forte" pode variar dependendo da área de estudo e do contexto da análise.

---

## 3. Um cuidado importante: correlação não é causalidade

Encontrar uma correlação entre duas variáveis **não significa que uma variável necessariamente cause a outra**.

Por exemplo, imagine que uma cidade tenha:

* Maior participação da indústria;
* Maior PIB per capita.

Podemos encontrar uma correlação positiva entre essas duas variáveis.

Isso significa que:

> **Municípios com maior participação industrial tendem a apresentar maior PIB per capita.**

Mas não podemos concluir automaticamente que:

> **A indústria é a causa direta do aumento do PIB per capita.**

Pode haver outros fatores envolvidos, como:

* Escolaridade da população;
* Infraestrutura;
* Localização geográfica;
* Investimentos públicos;
* Presença de universidades;
* Produtividade;
* Tecnologia;
* Qualificação profissional.

Portanto, a correlação nos ajuda a **identificar padrões e relações**, mas precisamos de outras análises para investigar relações de causa e efeito.

---

# 4. Aplicando a correlação na prática

Agora que relembramos o conceito de correlação, vamos aplicar esse conhecimento a uma situação prática de análise de dados.

Nesta aula, vamos nos colocar na posição de um **gestor público** que precisa decidir quais tipos de empresas e atividades econômicas sua administração deveria incentivar a se instalar no município.

O objetivo desse gestor é atrair empresas e atividades que possam contribuir para o aumento da renda dos moradores da cidade.

Pensando de maneira intuitiva sobre a economia de um município, podemos nos perguntar:

> **Quais setores econômicos permitem maiores retornos financeiros para a população?**

Algumas possibilidades poderiam ser:

* Indústria;
* Logística;
* Tecnologia;
* Agropecuária;
* Administração Pública;
* Entre outros.

Agora, pensando nos grandes setores econômicos utilizados na nossa análise, temos:

* **Indústria**;
* **Agropecuária**;
* **Serviços**;
* **Administração Pública**.

A pergunta que queremos responder é:

> **Quais desses setores possuem maior relação com a renda dos habitantes dos municípios brasileiros?**

---

# 5. Analisando os dados

Para verificar se nossa intuição está correta, o gestor organizou uma tabela contendo informações sobre os **municípios brasileiros**.

Entre as informações disponíveis, temos dados relacionados ao:

* PIB dos municípios;
* PIB per capita;
* Participação da Indústria;
* Participação da Agropecuária;
* Participação dos Serviços;
* Participação da Administração Pública.

A partir desses dados, podemos investigar se existe uma relação entre a **estrutura econômica de um município** e sua **renda per capita**.

Por exemplo:

> Se um município possui uma participação elevada da indústria em sua economia, ele também tende a apresentar um PIB per capita mais elevado?

Ou:

> Municípios com maior participação da Administração Pública possuem maior ou menor PIB per capita?

Para responder a essas perguntas, vamos utilizar uma medida estatística estudada na aula anterior:

> **A correlação.**

---

# 6. Correlação entre PIB per capita e setores econômicos

Neste exercício, vamos correlacionar duas variáveis:

1. **PIB per capita**;
2. **Percentual de participação de cada setor na economia do município**.

A ideia é verificar se existe uma relação entre a presença de determinado setor econômico e a renda per capita dos municípios.

É importante lembrar:

> **Correlação não significa causalidade.**

Ou seja, encontrar uma correlação positiva entre duas variáveis não significa necessariamente que uma delas seja responsável por causar a outra.

Por exemplo, se municípios com maior participação industrial apresentam maior PIB per capita, isso indica uma **associação entre as variáveis**, mas não prova, por si só, que a indústria seja a única responsável pelo aumento da renda.

---

# 7. Criando o PIB per capita

O primeiro passo da análise será criar uma coluna para calcular o **PIB per capita** de cada município.

De maneira geral, o cálculo é realizado da seguinte forma:

**PIB per capita = PIB total ÷ População**

No Power BI, podemos criar uma nova coluna calculada utilizando os campos correspondentes ao PIB e à população.

O resultado será uma nova variável que permitirá comparar municípios de diferentes tamanhos.

Isso é importante porque comparar apenas o PIB total poderia gerar uma interpretação equivocada.

Um município muito populoso pode possuir um PIB total elevado simplesmente por possuir uma população muito grande.

O **PIB per capita** permite observar uma medida média de riqueza econômica por habitante.

---

# 8. Calculando a participação dos setores

Em seguida, vamos criar novas colunas que determinarão a participação de cada setor econômico em relação ao PIB total do município.

Teremos, portanto, uma variável para cada setor:

* Participação da Indústria;
* Participação da Agropecuária;
* Participação dos Serviços;
* Participação da Administração Pública.

O cálculo geral será:

**Participação do setor = PIB do setor ÷ PIB total**

Dessa maneira, conseguimos descobrir qual percentual da economia de cada município é representado por cada setor.

Por exemplo:

> Se a indústria representa 30% do PIB de um município, teremos uma participação industrial de 30%.

Essas informações serão utilizadas posteriormente para comparar a estrutura econômica dos municípios com seu PIB per capita.

---

# 9. Criando o gráfico de dispersão

Agora vamos visualizar a relação entre a renda per capita e a participação dos setores econômicos.

Para isso, vamos utilizar um **gráfico de dispersão**.

Nesse gráfico:

* Cada ponto representa um município;
* O eixo X representa o **PIB per capita**;
* O eixo Y representa a **participação do setor no PIB**.

A configuração será:

| Elemento | Campo                 |
| -------- | --------------------- |
| Eixo X   | PIB per capita        |
| Eixo Y   | Participação do setor |
| Legenda  | UF ou Município       |

Podemos utilizar a **UF** como legenda para identificar os municípios por estado.

Também podemos utilizar o **Município** para identificar individualmente cada ponto.

O objetivo do gráfico é observar visualmente se existe algum padrão entre as duas variáveis.

---

# 10. Linha de tendência

Além dos pontos individuais, podemos adicionar uma **linha de tendência** ao gráfico.

A linha de tendência ajuda a visualizar a direção geral da relação entre as variáveis.

De maneira simplificada:

* Linha crescente → tendência de correlação positiva;
* Linha decrescente → tendência de correlação negativa;
* Linha próxima da horizontal → relação fraca ou inexistente.

Essa visualização facilita a interpretação dos dados, mas é importante utilizar também o **coeficiente de correlação** para quantificar a intensidade dessa relação.

---

# 11. Calculando a correlação

Para calcular a correlação entre as variáveis, podemos utilizar a funcionalidade de **medida rápida** do Power BI.

A configuração deverá considerar:

* **Categoria:** Município;
* **Eixo Y:** PIB per capita;
* **Eixo X:** Participação do setor;
* **Resumo:** Média para as duas variáveis.

O procedimento será repetido para cada um dos quatro grandes setores econômicos.

Assim, teremos uma correlação entre:

* PIB per capita × Participação da Indústria;
* PIB per capita × Participação da Agropecuária;
* PIB per capita × Participação dos Serviços;
* PIB per capita × Participação da Administração Pública.

Os coeficientes de correlação podem ser apresentados em **cartões** posicionados sobre ou próximos aos respectivos gráficos de dispersão.

Dessa forma, teremos uma combinação entre:

* **Visualização dos dados**;
* **Linha de tendência**;
* **Coeficiente de correlação**.
