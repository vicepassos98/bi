# Roteiro de aula: Power BI com dados do IBGE, banco online, Python e regressão

[arquivo parcial](https://drive.google.com/uc?export=download&id=1lOqxEJvBuxGym5jgTSWSXdco2mnLCboC&utm_source=chatgpt.com)



**Caso:** Lei de Okun no Brasil. Qual a relação entre o crescimento do PIB e a variação da taxa de desemprego?

---

## 1. Objetivos de aprendizagem

Ao final da aula, o aluno será capaz de:

- Buscar dados públicos em uma API (SIDRA/IBGE) e tratá-los no Power Query.
- Conectar o Power BI a um banco de dados online e entender o fluxo (servidor, credencial, Import vs DirectQuery).
- Usar um visual de Python para gerar um gráfico estatístico.
- Interpretar uma regressão linear simples (inclinação, R², p-valor, resíduos, outliers).
- Construir cenários "e se" com parâmetro What-if e reconhecer os limites de uma previsão.

---

## 2. Fontes de dados

| Dado | Tabela SIDRA | Observação |
|---|---|---|
| PIB (Contas Nacionais Trimestrais) | **5932** | Taxas de variação. Escolher PIB a preços de mercado e a variação em relação ao **mesmo trimestre do ano anterior**. |
| Taxa de desocupação (PNAD Contínua) | **4095** | Brasil, trimestral. Série a partir do 1º trimestre de 2012. |

**Por que usar variação interanual nas duas séries?**
Regredir *níveis* de séries que crescem ao longo do tempo gera correlação espúria. Comparar variações resolve isso, e comparar com o mesmo trimestre do ano anterior elimina o efeito sazonal sem precisar de ajuste.

- Variável X: crescimento do PIB (% em relação ao mesmo trimestre do ano anterior).
- Variável Y: variação da taxa de desocupação em pontos percentuais (taxa do trimestre menos a taxa do mesmo trimestre do ano anterior).
- Sinal esperado da inclinação: **negativo** (mais crescimento, menos desemprego).

---


---

## 5. Bloco 0 · Conceitos 

Sem tocar no Power BI. Exemplo guia: "se a economia cresce 2% a mais, quantos pontos o desemprego cai?"

- **Regressão linear simples:** uma reta que resume a relação entre duas variáveis.
- **Inclinação:** quanto Y muda para cada unidade a mais de X. Aqui: pontos percentuais de desemprego por 1 p.p. de crescimento do PIB.
- **R²:** quanto da variação de Y o modelo explica (0 a 100%). Sozinho, não mede "qualidade".
- **p-valor:** a relação observada pode ser só acaso? Valores baixos (ex.: < 0,05) indicam que provavelmente não.
- **Resíduos:** o que a reta não explica. Se tiverem padrão, o modelo está perdendo algo.
- **Outliers e pontos influentes:** o que fazer com 2020 (pandemia)?
- **Extrapolação:** prever fora da faixa dos dados observados é arriscado.
- **Previsão × cenário:** previsão estima o que deve acontecer; cenário responde "e se eu mudar X?".
- **Correlação não é causalidade.**
- **Séries temporais:** por que comparar variações e não níveis.

---

## Bloco 1 · API do IBGE no Power BI

* a API do SIDRA é pública, sem chave e sem login, e responde em JSON.

**Passo a passo**

1. Na página da tabela no SIDRA, escolher variável, território (Brasil) e período, e copiar a URL da aba **"Links de API"**.
2. No Power BI: Obter dados > **Web** > colar a URL > autenticação **Anônima**.
3. No Power Query: Transformar > **Usar a primeira linha como cabeçalho** (a primeira linha do JSON traz os rótulos das colunas).
4. Remover as colunas desnecessárias (códigos, unidade, nível territorial), mantendo período e valor.
5. Converter a coluna de valor para número decimal. Se vier com vírgula, usar "Alterar tipo > Usando localidade > Inglês (Estados Unidos)".
6. Tratar valores ausentes (`-` ou `...`): substituir por nulo ou remover erros.
7. Criar a coluna de data a partir do código do trimestre (ex.: `201201` = 1º trimestre de 2012). Coluna personalizada, ajustando o nome da coluna de código:

```
#date(
    Number.FromText( Text.Start([Trimestre (Código)], 4) ),
    ( Number.FromText( Text.End([Trimestre (Código)], 2) ) - 1 ) * 3 + 1,
    1
)
```

8. Repetir para a segunda tabela e unir as duas com **Mesclar consultas** pela coluna de data.


**Checkpoint:** tabela com `data`, `taxa_desocupacao` e `pib_var_interanual` e um gráfico de linhas de cada série.

---



## 8. Bloco 3 · Visual de Python

**Limites**

- O resultado é uma imagem estática (sem interação direta no gráfico).
- Limite de aproximadamente 150 mil linhas e 5 minutos de execução.
- Linhas duplicadas são removidas dos dados do visual. Inclua um campo único (a `data`) entre os campos arrastados.
- No Service só funcionam as bibliotecas suportadas. Conferir a lista oficial antes de publicar.

**Prática**

0. Baixar as bibliotecas no python através do CMD:

    a. Win+R para abrir o executar e depois digite cmd
    
    b. Vamos Precisar baixar 4 bibliotecas para fazer esses visuais
    
        i. Pandas
        
        ii. Numpy
        
        iii. matplotlib
        
        iv. Statsmodels
        
    c. Vamos utilizar o pip para baixar as bibliotecas:
```
    pip install pandas
    pip install numpy
    pip install matplotlib
    pip install statsmodels
```
1. Inserir o visual de Python e habilitar scripts.
2. Arrastar `data`, `pib_var_interanual` e `desocupacao_var_pp` para Valores.
3. Colar o script: dispersão com reta, banda de confiança, equação, R², p-valor e gráfico de resíduos.
```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import statsmodels.api as sm

# ---- AJUSTES PERMITIDOS AOS ALUNOS ----
TITULO = "Crescimento do PIB x variação do desemprego"
ROTULO_X = "Crescimento do PIB (% vs mesmo trimestre do ano anterior)"
ROTULO_Y = "Variação da taxa de desocupação (p.p.)"
COR_PONTOS = "#2E75B6"
COR_RETA = "#C00000"
COR_2020 = "#ED7D31"
# ---------------------------------------

df = dataset[["data", "pib_var_interanual", "desocupacao_var_pp"]].dropna().copy()
df["data"] = pd.to_datetime(df["data"])
x = df["pib_var_interanual"]
y = df["desocupacao_var_pp"]

# Regressão linear simples (mínimos quadrados)
modelo = sm.OLS(y, sm.add_constant(x)).fit()
intercepto, inclinacao = modelo.params
r2 = modelo.rsquared
p_valor = modelo.pvalues.iloc[1]
ic = modelo.conf_int().iloc[1]

# Reta e banda de confiança (apenas dentro da faixa observada)
xs = np.linspace(x.min(), x.max(), 100)
pred = modelo.get_prediction(sm.add_constant(xs)).summary_frame(alpha=0.05)

fig, ax1 = plt.subplots(figsize=(8, 5))

pandemia = df["data"].dt.year == 2020
ax1.scatter(x[~pandemia], y[~pandemia], color=COR_PONTOS, alpha=0.8, label="Trimestres")
ax1.scatter(x[pandemia], y[pandemia], color=COR_2020, alpha=0.9, label="2020")
ax1.plot(xs, pred["mean"], color=COR_RETA, linewidth=2, label="Reta de regressão")
ax1.fill_between(xs, pred["mean_ci_lower"], pred["mean_ci_upper"],
                 color=COR_RETA, alpha=0.15, label="IC 95% da reta")
ax1.axhline(0, color="gray", linewidth=0.8)
ax1.axvline(0, color="gray", linewidth=0.8)
ax1.set_title(TITULO)
ax1.set_xlabel(ROTULO_X)
ax1.set_ylabel(ROTULO_Y)

texto = (f"y = {intercepto:.2f} + ({inclinacao:.2f})·x\n"
         f"R² = {r2:.2f}\n"
         f"p-valor = {p_valor:.4f}\n"
         f"IC 95% inclinação: [{ic[0]:.2f}; {ic[1]:.2f}]\n"
         f"n = {len(df)} trimestres")
ax1.text(0.03, 0.03, texto, transform=ax1.transAxes, va="bottom",
         bbox=dict(boxstyle="round", facecolor="white", alpha=0.9))
ax1.legend(loc="upper right")

plt.tight_layout()
plt.show()
```

5. Alterem apenas título, cores e rótulos dos eixos.
6. Filtrar por período com um segmentador e ver o gráfico mudar.



**Checkpoint:** dispersão PIB × variação do desemprego com reta, R² e equação.

---

## 9. Bloco 4 · Regressão

**Passo 1 · Linha de tendência nativa

Gráfico de dispersão > painel Análise > Linha de tendência. Mostrar o que ela entrega (a reta) e o que não entrega (p-valor, resíduos, previsão controlável).

**Passo 2 · Coeficientes no DAX com LINESTX 

Mostrar pronto, sem derivar a fórmula (ajuste os nomes da tabela e das colunas):

```
Coeficientes =
LINESTX(
    okun_trimestral,
    okun_trimestral[desocupacao_var_pp],
    okun_trimestral[pib_var_interanual]
)
```

```
Inclinacao = MAXX( [Coeficientes], [Slope1] )
Intercepto = MAXX( [Coeficientes], [Intercept] )
```

Verifique os nomes das colunas retornadas na versão do Power BI da turma (normalmente `Slope1` e `Intercept`). Se a versão não tiver `LINESTX`, use os valores do script Python como parâmetros fixos.

Passo 3 · Interpretação com Python

Ler o resumo do `statsmodels` na tela:

- Inclinação em linguagem de negócio (ex.: "cada 1 p.p. a mais de crescimento do PIB está associado a X p.p. a menos de desocupação").
- R² e o que ele diz (e não diz).
- p-valor e intervalo de confiança.
- Resíduos: parecem aleatórios ou há padrão?

Passo 4 · O caso 2020

- Identificar os trimestres da pandemia no gráfico. São outliers? Pontos influentes?
- Refazer a regressão **com e sem** 2020 (segmentador de período) e comparar inclinação e R².
- Discutir: remover o ponto é legítimo? Quando e com que justificativa?

Para discutir

- O desemprego costuma reagir ao PIB com defasagem. Testar o PIB de um trimestre antes (extensão opcional).
- Com cerca de 50 observações e dados em série temporal, os resíduos podem ter dependência entre trimestres (ponto avançado, só mencionar).

---

##Bloco 5 · Cenários com What-if (30 min)

1. Modelagem > Novo parâmetro > Numérico. Nome: `Crescimento do PIB (% a/a)`, de -4 a +6, passo 0,5, valor padrão 2. O intervalo deve ficar próximo da faixa histórica observada.
2. Criar as medidas:

```
Variacao prevista desocupacao (p.p.) =
    [Intercepto] + [Inclinacao] * [Valor Crescimento do PIB (% a/a)]

Taxa atual =
    CALCULATE(
        MAX( okun_trimestral[taxa_desocupacao] ),
        LASTDATE( okun_trimestral[data] )
    )

Taxa projetada em 12 meses =
    [Taxa atual] + [Variacao prevista desocupacao (p.p.)]
```

3. Colocar o slider, um cartão com a taxa atual e outro com a taxa projetada, e um indicador da diferença.
4. Discutir: "e se o PIB crescer 2%?", depois "e se crescer 10%?" (extrapolação).

**Pontos de discussão**

- O cenário assume que a relação histórica continua valendo.
- Fora da faixa observada, o modelo perde confiabilidade. O slider deveria estar limitado a ela.
- Em vez de um número único, apresentar uma faixa (intervalo de confiança).
- Cenário não é previsão: é uma simulação condicionada ao valor escolhido.
- Outros fatores (política monetária, informalidade, demografia) não entram no modelo.

---

## 11. Fechamento e desafio final (10 min)

Recapitular o fio condutor: **API pública → banco online → análise estatística → cenário**.

**Mini-desafio (para casa ou próxima aula)**

Escolher outro par de variáveis do SIDRA (ex.: inflação × taxa de juros, rendimento médio × desocupação, PIB × consumo das famílias):

1. Buscar os dados pela API e tratá-los no Power Query.
2. Gerar o visual de Python com regressão.
3. Interpretar R² e p-valor em duas frases.
4. Montar um cenário What-if e apontar um limite do modelo.

**Critérios de avaliação sugeridos:** dados buscados e tratados corretamente, gráfico correto, interpretação em linguagem de negócio, reconhecimento de pelo menos uma limitação (outliers, extrapolação, causalidade).

---

## 12. Checklist para o dia

- [ ] Códigos das tabelas 5932 e 4095 e variáveis conferidos no SIDRA
- [ ] URLs da API testadas e distribuídas
- [ ] Banco no ar, usuário somente leitura e firewall liberado
- [ ] Python testado em máquina de aluno
- [ ] CSV de `okun_trimestral` como plano B
- [ ] `.pbix` base e script enviados
- [ ] Rede da sala testada (API do IBGE e porta do banco)
