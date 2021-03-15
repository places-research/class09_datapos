Hotel bookings - data wrangling
================
Mine Çetinkaya-Rundel - Renata Oliveira (tradução)

``` r
library(tidyverse)
library(skimr)
```

``` r
# From TidyTuesday: https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-02-11/readme.md
hotels <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-02-11/hotels.csv")
```

## Exercícios

### Exercício 1.

Aquecimento! Dê uma olhada geral nos dados com a função `skim()`.

**Nota:** Eu já dei a resposta a este exercício. Você só precisa fazer o
`knit` do documento e visualizar a saída. Uma definição de todas as
variáveis é dada na seção [Dicionário de dados](#dicionário-de-dados) no
final, embora você não precise se familiarizar com todas as variáveis
para poder trabalhar com estes exercícios.

``` r
skim(hotels)
```

|                                                  |        |
|:-------------------------------------------------|:-------|
| Name                                             | hotels |
| Number of rows                                   | 119390 |
| Number of columns                                | 32     |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |        |
| Column type frequency:                           |        |
| character                                        | 13     |
| Date                                             | 1      |
| numeric                                          | 18     |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |        |
| Group variables                                  | None   |

Data summary

**Variable type: character**

| skim\_variable        | n\_missing | complete\_rate | min | max | empty | n\_unique | whitespace |
|:----------------------|-----------:|---------------:|----:|----:|------:|----------:|-----------:|
| hotel                 |          0 |              1 |  10 |  12 |     0 |         2 |          0 |
| arrival\_date\_month  |          0 |              1 |   3 |   9 |     0 |        12 |          0 |
| meal                  |          0 |              1 |   2 |   9 |     0 |         5 |          0 |
| country               |          0 |              1 |   2 |   4 |     0 |       178 |          0 |
| market\_segment       |          0 |              1 |   6 |  13 |     0 |         8 |          0 |
| distribution\_channel |          0 |              1 |   3 |   9 |     0 |         5 |          0 |
| reserved\_room\_type  |          0 |              1 |   1 |   1 |     0 |        10 |          0 |
| assigned\_room\_type  |          0 |              1 |   1 |   1 |     0 |        12 |          0 |
| deposit\_type         |          0 |              1 |  10 |  10 |     0 |         3 |          0 |
| agent                 |          0 |              1 |   1 |   4 |     0 |       334 |          0 |
| company               |          0 |              1 |   1 |   4 |     0 |       353 |          0 |
| customer\_type        |          0 |              1 |   5 |  15 |     0 |         4 |          0 |
| reservation\_status   |          0 |              1 |   7 |   9 |     0 |         3 |          0 |

**Variable type: Date**

| skim\_variable            | n\_missing | complete\_rate | min        | max        | median     | n\_unique |
|:--------------------------|-----------:|---------------:|:-----------|:-----------|:-----------|----------:|
| reservation\_status\_date |          0 |              1 | 2014-10-17 | 2017-09-14 | 2016-08-07 |       926 |

**Variable type: numeric**

| skim\_variable                    | n\_missing | complete\_rate |    mean |     sd |      p0 |     p25 |     p50 |  p75 | p100 | hist  |
|:----------------------------------|-----------:|---------------:|--------:|-------:|--------:|--------:|--------:|-----:|-----:|:------|
| is\_canceled                      |          0 |              1 |    0.37 |   0.48 |    0.00 |    0.00 |    0.00 |    1 |    1 | ▇▁▁▁▅ |
| lead\_time                        |          0 |              1 |  104.01 | 106.86 |    0.00 |   18.00 |   69.00 |  160 |  737 | ▇▂▁▁▁ |
| arrival\_date\_year               |          0 |              1 | 2016.16 |   0.71 | 2015.00 | 2016.00 | 2016.00 | 2017 | 2017 | ▃▁▇▁▆ |
| arrival\_date\_week\_number       |          0 |              1 |   27.17 |  13.61 |    1.00 |   16.00 |   28.00 |   38 |   53 | ▅▇▇▇▅ |
| arrival\_date\_day\_of\_month     |          0 |              1 |   15.80 |   8.78 |    1.00 |    8.00 |   16.00 |   23 |   31 | ▇▇▇▇▆ |
| stays\_in\_weekend\_nights        |          0 |              1 |    0.93 |   1.00 |    0.00 |    0.00 |    1.00 |    2 |   19 | ▇▁▁▁▁ |
| stays\_in\_week\_nights           |          0 |              1 |    2.50 |   1.91 |    0.00 |    1.00 |    2.00 |    3 |   50 | ▇▁▁▁▁ |
| adults                            |          0 |              1 |    1.86 |   0.58 |    0.00 |    2.00 |    2.00 |    2 |   55 | ▇▁▁▁▁ |
| children                          |          4 |              1 |    0.10 |   0.40 |    0.00 |    0.00 |    0.00 |    0 |   10 | ▇▁▁▁▁ |
| babies                            |          0 |              1 |    0.01 |   0.10 |    0.00 |    0.00 |    0.00 |    0 |   10 | ▇▁▁▁▁ |
| is\_repeated\_guest               |          0 |              1 |    0.03 |   0.18 |    0.00 |    0.00 |    0.00 |    0 |    1 | ▇▁▁▁▁ |
| previous\_cancellations           |          0 |              1 |    0.09 |   0.84 |    0.00 |    0.00 |    0.00 |    0 |   26 | ▇▁▁▁▁ |
| previous\_bookings\_not\_canceled |          0 |              1 |    0.14 |   1.50 |    0.00 |    0.00 |    0.00 |    0 |   72 | ▇▁▁▁▁ |
| booking\_changes                  |          0 |              1 |    0.22 |   0.65 |    0.00 |    0.00 |    0.00 |    0 |   21 | ▇▁▁▁▁ |
| days\_in\_waiting\_list           |          0 |              1 |    2.32 |  17.59 |    0.00 |    0.00 |    0.00 |    0 |  391 | ▇▁▁▁▁ |
| adr                               |          0 |              1 |  101.83 |  50.54 |   -6.38 |   69.29 |   94.58 |  126 | 5400 | ▇▁▁▁▁ |
| required\_car\_parking\_spaces    |          0 |              1 |    0.06 |   0.25 |    0.00 |    0.00 |    0.00 |    0 |    8 | ▇▁▁▁▁ |
| total\_of\_special\_requests      |          0 |              1 |    0.57 |   0.79 |    0.00 |    0.00 |    0.00 |    1 |    5 | ▇▁▁▁▁ |

### Exercício 2.

As pessoas estão viajando por um capricho? Vamos ver…

Preencha os espaços em branco para filtragem de reservas de hotel onde o
hóspede é **não** dos EUA (código do país `EUA`) e o `lead_time` é menos
de 1 dia.

**Nota:** Você precisará definir `eval=TRUE` quando tiver uma resposta
que queira experimentar.

``` r
hotels %>%
  filter(
    country ____ "USA", 
    lead_time ____ ____
    )
```

### Exercício 3.

Quantas marcações envolvem pelo menos 1 criança **ou** bebê?

No seguinte `chunk`, substitua

-   `[AT LEAST]` com o operador lógico para “pelo menos” (em dois
    lugares)
-   `[OR]` com o operador lógico para “ou”

**Nota:** Você precisará definir `eval=TRUE` quando tiver uma resposta
que queira experimentar.

``` r
hotels %>%
  filter(
    children [AT LEAST] 1 [OR] babies [AT LEAST] 1
    )
```

### Exercício 4.

Você acha que é mais provável encontrar reservas com crianças ou bebês
em hotéis urbanos ou resorts hoteleiros? Teste sua intuição.

Usando `filter()` determine o número de reservas em hotéis resort que
têm mais de 1 criança **ou** bebê no quarto? Então, faça o mesmo para
hotéis urbanos, e compare o número de linhas no dataframe filtrado
resultantes.

``` r
# add code here
# pay attention to correctness and code style
```

``` r
# add code here
# pay attention to correctness and code style
```

### Exercício 5.

Criar uma tabela de freqüência do número de `adults` em uma reserva.

Mostre os resultados em ordem decrescente para que a observação mais
comum esteja no topo.

Qual é o número mais comum de adultos em reservas neste conjunto de
dados?

Há algum resultado surpreendente?

**Nota:** Não esqueça de rotular também seu `chunk` R (onde diz
`lable-me-1`). Seu rótulo deve ser curto, informativo, e não deve
incluir espaços. Também não deve repetir uma etiqueta anterior, caso
contrário o R Markdown lhe dará um erro sobre a repetição de etiquetas R
em pedaços.

``` r
# add code here
# pay attention to correctness and code style
```

### Exercício 6.

Repita o exercício 5, uma vez para reservas canceladas (`is_canceled`
codificado como 1) e uma vez para reservas não canceladas (`is_canceled`
codificado como 0).

O que isto revela sobre os resultados surpreendentes que você observou
no exercício anterior?

**Note:** Não se esqueça de rotular também seu `chunk` de R (onde diz
`label-me-2`).

``` r
# add code here
# pay attention to correctness and code style
```

### Exercício 7.

Calcular a tarifa mínima, média, mediana e máxima média diária (`adr`)
agrupados por tipo de `hotel` para que você possa obter estas
estatísticas separadamente para hotéis de resorts e cidades.

Que tipo de hotel é mais caro, em média?

``` r
# add code here
# pay attention to correctness and code style
```

### Exercício 8.

Observamos dois valores incomuns nas estatísticas resumidas acima – um
mínimo negativo, e um máximo muito alto). Que tipos de hotéis são estes?

Localize estas observações no conjunto de dados e descubra a data de
chegada (ano e mês), assim como quantas pessoas (adultos, crianças e
bebês) permaneceram no quarto.

Você pode investigar os dados no espectador para localizar estes
valores, mas de preferência você deve identificá-los de forma
reprodutível com algum código.

**Dica:** Por exemplo, você pode `filter` para o dado quantidade `adr` e
`select` as colunas relevantes.

``` r
# add code here
# pay attention to correctness and code style
```

## Dicionário de dados

Abaixo está o dicionário de dados completo. Note que é longo (há muitas
variáveis nos dados), mas utilizamos um conjunto limitado de variáveis
para nossa análise.

| variable                          | class     | description                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------|:----------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| hotel                             | character | Hotel (H1 = Resort Hotel or H2 = City Hotel)                                                                                                                                                                                                                                                                                                                                                                                        |
| is\_canceled                      | double    | Value indicating if the booking was canceled (1) or not (0)                                                                                                                                                                                                                                                                                                                                                                         |
| lead\_time                        | double    | Number of days that elapsed between the entering date of the booking into the PMS and the arrival date                                                                                                                                                                                                                                                                                                                              |
| arrival\_date\_year               | double    | Year of arrival date                                                                                                                                                                                                                                                                                                                                                                                                                |
| arrival\_date\_month              | character | Month of arrival date                                                                                                                                                                                                                                                                                                                                                                                                               |
| arrival\_date\_week\_number       | double    | Week number of year for arrival date                                                                                                                                                                                                                                                                                                                                                                                                |
| arrival\_date\_day\_of\_month     | double    | Day of arrival date                                                                                                                                                                                                                                                                                                                                                                                                                 |
| stays\_in\_weekend\_nights        | double    | Number of weekend nights (Saturday or Sunday) the guest stayed or booked to stay at the hotel                                                                                                                                                                                                                                                                                                                                       |
| stays\_in\_week\_nights           | double    | Number of week nights (Monday to Friday) the guest stayed or booked to stay at the hotel                                                                                                                                                                                                                                                                                                                                            |
| adults                            | double    | Number of adults                                                                                                                                                                                                                                                                                                                                                                                                                    |
| children                          | double    | Number of children                                                                                                                                                                                                                                                                                                                                                                                                                  |
| babies                            | double    | Number of babies                                                                                                                                                                                                                                                                                                                                                                                                                    |
| meal                              | character | Type of meal booked. Categories are presented in standard hospitality meal packages: <br> Undefined/SC – no meal package;<br>BB – Bed & Breakfast; <br> HB – Half board (breakfast and one other meal – usually dinner); <br> FB – Full board (breakfast, lunch and dinner)                                                                                                                                                         |
| country                           | character | Country of origin. Categories are represented in the ISO 3155–3:2013 format                                                                                                                                                                                                                                                                                                                                                         |
| market\_segment                   | character | Market segment designation. In categories, the term “TA” means “Travel Agents” and “TO” means “Tour Operators”                                                                                                                                                                                                                                                                                                                      |
| distribution\_channel             | character | Booking distribution channel. The term “TA” means “Travel Agents” and “TO” means “Tour Operators”                                                                                                                                                                                                                                                                                                                                   |
| is\_repeated\_guest               | double    | Value indicating if the booking name was from a repeated guest (1) or not (0)                                                                                                                                                                                                                                                                                                                                                       |
| previous\_cancellations           | double    | Number of previous bookings that were cancelled by the customer prior to the current booking                                                                                                                                                                                                                                                                                                                                        |
| previous\_bookings\_not\_canceled | double    | Number of previous bookings not cancelled by the customer prior to the current booking                                                                                                                                                                                                                                                                                                                                              |
| reserved\_room\_type              | character | Code of room type reserved. Code is presented instead of designation for anonymity reasons                                                                                                                                                                                                                                                                                                                                          |
| assigned\_room\_type              | character | Code for the type of room assigned to the booking. Sometimes the assigned room type differs from the reserved room type due to hotel operation reasons (e.g. overbooking) or by customer request. Code is presented instead of designation for anonymity reasons                                                                                                                                                                    |
| booking\_changes                  | double    | Number of changes/amendments made to the booking from the moment the booking was entered on the PMS until the moment of check-in or cancellation                                                                                                                                                                                                                                                                                    |
| deposit\_type                     | character | Indication on if the customer made a deposit to guarantee the booking. This variable can assume three categories:<br>No Deposit – no deposit was made;<br>Non Refund – a deposit was made in the value of the total stay cost;<br>Refundable – a deposit was made with a value under the total cost of stay.                                                                                                                        |
| agent                             | character | ID of the travel agency that made the booking                                                                                                                                                                                                                                                                                                                                                                                       |
| company                           | character | ID of the company/entity that made the booking or responsible for paying the booking. ID is presented instead of designation for anonymity reasons                                                                                                                                                                                                                                                                                  |
| days\_in\_waiting\_list           | double    | Number of days the booking was in the waiting list before it was confirmed to the customer                                                                                                                                                                                                                                                                                                                                          |
| customer\_type                    | character | Type of booking, assuming one of four categories:<br>Contract - when the booking has an allotment or other type of contract associated to it;<br>Group – when the booking is associated to a group;<br>Transient – when the booking is not part of a group or contract, and is not associated to other transient booking;<br>Transient-party – when the booking is transient, but is associated to at least other transient booking |
| adr                               | double    | Average Daily Rate as defined by dividing the sum of all lodging transactions by the total number of staying nights                                                                                                                                                                                                                                                                                                                 |
| required\_car\_parking\_spaces    | double    | Number of car parking spaces required by the customer                                                                                                                                                                                                                                                                                                                                                                               |
| total\_of\_special\_requests      | double    | Number of special requests made by the customer (e.g. twin bed or high floor)                                                                                                                                                                                                                                                                                                                                                       |
| reservation\_status               | character | Reservation last status, assuming one of three categories:<br>Canceled – booking was canceled by the customer;<br>Check-Out – customer has checked in but already departed;<br>No-Show – customer did not check-in and did inform the hotel of the reason why                                                                                                                                                                       |
| reservation\_status\_date         | double    | Date at which the last status was set. This variable can be used in conjunction with the ReservationStatus to understand when was the booking canceled or when did the customer checked-out of the hotel                                                                                                                                                                                                                            |
