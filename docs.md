
# Introdução ao problema


# Escolha do endereço de chegada

##  Cálculo das probabilidades de escolha de cada distrito
Nesse primeiro momento, queremos selecionar os distritos oficiais de São Paulo e calcular a probabilidade de escolha de cada um deles. Para isso, vamos utilizar a base de dados do IBGE, que contém a população de cada distrito da cidade.
O cálculo será feito usando a população total e renda média de cada distrito da seguinte forma:

$$PopulacaoNormalizada(distrito) = \frac{Populacao(distrito)}{\sum_{distritos} Populacao(distrito)}$$
$$PopulacaoNormalizada(distrito) = \frac{Populacao(distrito)}{\sum_{distritos} Populacao(distrito)}$$

## Escolha do endereço de origem dentro do distrito
Escolheremos o endereço de origem dentro de um distrito por escolher uniformemente uma rua dentro das ruas do distrito e um número dentro do intervalo de números da rua. Para isso, vamos utilizar a base de dados do OpenStreetMap, que contém as ruas de São Paulo e a quantidade de números em cada uma delas.

# Escolha do endereço de destino

Já escolhido o endereço de origem da corrida, queremos escolher um endereço de destino para nosso passageiro. Neste caso, fatores como população do distrito do destino e a renda média dos moradores não são tão relevantes, ninguém pede Uber para ir a um distrito porque ele é populoso ou porque a renda média dos moradores é alta. O que é relevante é a distância entre o distrito de origem e o distrito de destino, e a quantidade de corridas que são feitas entre esses dois distritos.
Seria interessante integrar ao nosso modelo um fator de "popularidade" a cada distrito, dado que alguns bairros são mais visitados (bairros onde as pessoas costumam trabalhar ou a passeio), mas neste primeiro momento vamos considerar apenas a distância entre os endereços.

## Distância máxima para o endereço de chegada

O cálculo usará uma função de decaimento exponencial para selecionar um raio de endereços possíveis ao redor do endereço de origem, e então escolher um endereço de destino uniformemente dentro desse raio.

$$
P(d) = \frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{d^2}{2\sigma^2}}
$$

onde $d$ é uma variável aleatória escolhida através da probabilidade P(d), que a distância máxima escolhida para o endereço de destino, e $\sigma$ é um parâmetro que controla a dispersão da distribuição.

