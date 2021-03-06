---
title: "Quando o negacionismo de governantes matam as pessoas"
author: 'Jonas Campos'
abstract: 'Trabalho da disciplina Repositórios Digitais e Ciência de Dados em Saúde  - Módulo I  - Semana 01 - UNIFESP'
output:
  html_notebook: default
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## Introdução

Se um chimpanzé respondesse um questionário sobre questões sócio-econômicas (o que inclui questões sobre saúde), este chimpanzé se sairia MELHOR do que a maioria das pessoas (Rosling ,Rosling e Rosling Rönnlund, 2019).  Os autores propõem  uma mentalidade que leva em consideração somente os fatos (Factfulness), mentalidade esta que será utilizada neste trabalho.

!['Sugriva in Myrtle Beach Safari, South Carolina, US. Image credit by: https://www.admiravelcurioso.com.br/estaria-este-chimpanze-utilizando-o-instagram'](analytics/chimpanze-1.png)

**Negacionismo que contagia**

Um presidente que contraria a ciência, dizendo que um vírus não causa grandes males, e para os acometidos pela doença, recomendava o uso de uma substância comprovadamente ineficaz para que o doente se cure. Uma estimativa é que por influência direta do presidente, 300.000 pessoas perderam a vida. Tribunais internacionais analisam pedidos para julgar o presidente por crimes contra a humanidade (Krippahl, 2019).
```{r}

```

Figura louvada pelo engajamento de inserir o seu país no grupo de potências econômicas, negou compromissos junto à democracia e direitos humanos (DOPCKE, 2002).

Embora inequívoca semelhança, esta análise não é sobre a figura do presidente da república Jair Messias Bolsonaro (SEM PARTIDO) e sua atuação enquanto chefe de Estado diante do enfrentamento da pandemia de Covid-19.

### Preparando o ambiente para análises e visão geral dos dados

```{r carregando ou instalando pacotes}
#instalando, caso não tenha os pacotes
if (!('rvest' %in% installed.packages())) {
  install.packages('rvest')
}

if (!('reshape2' %in% installed.packages())) {
  install.packages('reshape2')
}

if (!('dplyr' %in% installed.packages())) {
  install.packages('dplyr')
}

if (!('readr' %in% installed.packages())) {
  install.packages('readr')
}

# Carregando pacotes utilizados
library(dplyr) 
library(reshape2) 
library(tidyr)
library(readr)

```

**Carregando conjunto de dados**


```{r carregando dados}
df = read_csv('datasets/adults_with_hiv_percent_age_15_49.csv')
head(df)
```
Como vemos, o conjunto de dados apresenta porcentagem da população adulta infectada por HIV ao longo dos anos, por países. Além de valores não disponíveis (NA), a forma como os dados estão dispostos não estão apropriados para nossa análise.

**Transpondo dados e Removendo valores indisponíveis (NA)**
```{r}
df1 = melt(df, id = c('country'))
head(df1)
```
**Removendo valores NA e renomeando colunas

```{r removendo valores e renomeando colunas NA}
df1a = na.omit(df1)
head(df1a)
#
colnames(df1a)
names(df1a)[1:3] = c('País', 'Ano', 'Incidência')
head(df1a)
```

Ufa! Agora nossos dados estão quase prontos para as análises.
Vamos salvar o que fizemos até aqui. Isso ajuda organizar os arquivos, além de fazer um bakup do resultado de tudo que fizemos até aqui.

```{r salvando dataset limpo e transposto}
write.csv(df1a, file='datasets/HIV_adultos_reshape.csv')
```

**Filtrando dados**
Nossos dados contém um intervalo de tempo bem extenso. Vou considerar somente os fatos registrados entre 2000-2010

```{r}
#  2000-2010
df = read.csv('datasets/HIV_adultos_reshape.csv')
head(df)
df1 = filter(df, Ano > 1999, Ano < 2011)
head(df1)
```
O R gerou uma coluna de índice, que vou remover.
```{r}
df1$X = NULL
head(df1)
```
Agora vamos salvar nosso arquivo para começar nossas análises
```{r}
write.csv(df1, file = 'datasets/HIV_adultos_2000-2010.csv', fileEncoding = 'UTF-8')
```

#### Visualizando dados no DataStudio

Fiz o upload do nosso dataset tratado e filtrado para o google studio. Fiz uma cópia da coluna Data, para que fosse reconhecida como formato de Ano. Para a cópia usei a fórmula:

```
PARSE_DATE("%Y%m", Cópia de Ano)
```
Removi a métrica e depois a coluna de Ano Original. Filtrei os dados por quantidade de infecatados, e filtrei para os 50 primeiros registros (~que não entendi porque apareceram os 66 primeiros...~). Nada que prejudique nossa análise, mas se alguém souber me explicar isso seria muito grato.
Fiz o cruzamento das métricas e gerei o mapa com a tabela das principais ocorrências, no qual disponibilizo na sessão Análises logo a seguir.
Fiz o download do arquivo tratado, usado para a visualização do mapa, para que possa sumarizar e trazer mais algumas informações dentro do RStudio.

## Negacionismo que adoece e mata

### Análises

É espantoso que em 10 anos, a maior população adulta infectada com HIV concentra-se em um único continente. Para acessar o relatório dinâmico, **[clique aqui](https://datastudio.google.com/reporting/912b738a-b4aa-4622-8e36-d68b67ccdd34)**.

![**50 países com maior quantidade de Adultos infectados entre 2000-2010 no mundo.** *Mapa produzido pelo autor*](analytics/Top-50-Adults_HIV-in_World.png)

#### Voltando ao RStudio

Vamos carregar nosso conjunto de dados usado no Data Studio, e espiar com a função ```glimpse()```.

```{r carregando e sumarizando os dados usados na visualização do google studio}

df = read.csv('datasets/Top-50-Adults_HIV-in_World.csv')
glimpse(df)

```

```{r}
#df_summary = count(df, Pais, order())
df1 = df

df1 %>%
  group_by(Pais) %>%
  summarise(Incidencia = mean(Incidencia),
            total = n())

```

Considerando os 50 primeiros resultados, vemos que todos os países são do continente africano, e que ao longo de 11 anos (2000-2010) alguns aparecem 11 vezes na listagem.

## Referências

DOPCKE, Wolfgang. Há salvação para a África? Thabo Mbeki e seu New Partnership for African development. **Rev. bras. polít. int.**,  Brasília ,  v. 45, n. 1, p. 146-155,  June  2002 .   Disponível em <http://dx.doi.org/10.1590/S0034-73292002000100006>, acessado em 12/02/2021.

GAPMINDER. Data. Gapminder, 2019. Disponivel em <https://www.gapminder.org/data/>, acessado em 10/02/2021.
Hans Rosling, Ola Rosling, Anna Rosling Rönnlund. Factfulness:  o hábito libertador de só ter opiniões baseadas em fatos.

Tradução Vitor Paolozzi. – 1. ed. – Rio de Janeiro: Record, 2019. 

IME-USP. Ciência de dados em R. Disponível em <https://livro.curso-r.com/index.html>, acessado em 12/02/2021

Krippahl, Cristina. Perigos da ignorância: Os presidentes africanos e a negação da SIDA. Deutsche Welle, 01/03/2019. Disponível em <https://www.dw.com/pt-002/perigos-da-ignorância-os-presidentes-africanos-e-a-negação-da-sida/g-46511688>, acessado em 12/02/2021.

R Foundadion. The R Project for Statistical Computing. Disponível em <https://www.r-project.org>, acessado em 13/01/2021.
