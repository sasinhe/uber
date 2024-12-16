# Introdução ao Problema

O objetivo deste algoritmo é gerar um conjunto de pares de endereços na cidade de São Paulo para o treinamento de um modelo de precificação de corridas de Uber.

O algoritmo terá natureza estocástica, dando preferência para endereços de chegada em distritos com maior densidade populacional e renda média por habitante (fatores relevantes para o uso do aplicativo Uber).

Para os endereços de saída, o algoritmo usará uma função de decaimento exponencial $P(d)$ (descrita abaixo) para gerar uma distância máxima para o endereço do destino, onde escolherá o endereço de destino uniformemente dentro desse intervalo.

Usaremos o conjunto de dados geográficos (disponível como um arquivo `.shape` com um grafo representando as ruas de São Paulo) distribuído pela plataforma [GeoSampa](https://geosampa.prefeitura.sp.gov.br) da Prefeitura de São Paulo para acessar os endereços disponíveis dentro da distância $d$ da origem.

---

# Escolha do Endereço de Chegada

## Cálculo Das Probabilidades de Escolha de Cada Distrito

Queremos selecionar os distritos oficiais de São Paulo e calcular a probabilidade de escolha de cada um deles. Utilizamos a base de dados do IBGE, que contém a população e a renda média de cada distrito. O cálculo da probabilidade de escolha segue a fórmula:

$$
\text{Probabilidade(distrito)} = \frac{\text{População(distrito)} \cdot \text{RendaMédia(distrito)}}{\sum_{distritos} (\text{População} \cdot \text{RendaMédia})}
$$

Essa probabilidade será utilizada para realizar a amostragem ponderada entre os distritos.

---

## Escolha do Endereço de Origem dentro do Distrito

Escolheremos o endereço de origem de forma uniforme. Utilizamos a base de dados do OpenStreetMap, que contém as ruas de São Paulo e a quantidade de números em cada uma. O processo é:

1. Selecionar uma rua aleatória dentro do distrito.
2. Escolher um número dentro do intervalo de números válidos para a rua.

---

# Escolha do Endereço de Destino

Após selecionar o endereço de origem, escolhemos um endereço de destino. Neste caso, fatores como população e renda não são tão relevantes. O que importa é a distância entre os endereços e a "popularidade" de alguns bairros, que será incorporada em versões futuras. Por ora, consideramos apenas a distância.

## Distância Máxima para o Endereço de Destino

A função de decaimento exponencial define a probabilidade de escolha de uma distância máxima $d$:

$$
P(d) = \frac{1}{\sqrt{2\pi}\sigma} e^{-\frac{d^2}{2\sigma^2}}
$$

Onde:

- $ d $: distância máxima entre origem e destino.
- $ \sigma $: parâmetro que controla a dispersão da distribuição.

Dentro do raio $ d $, o endereço de destino será selecionado de forma uniforme.

---

# Implementação em Código Python

A implementação segue as etapas principais descritas abaixo:

1. __Carregamento e Preparação dos Dados:__

   - Utilize bibliotecas como `geopandas` para manipular arquivos `.shapefile`.
   - Construa o grafo das ruas de São Paulo com `networkx`.
2. __Cálculo das Probabilidades de Escolha:__

   - Importe os dados do IBGE utilizando `pandas`.
   - Calcule e normalize as probabilidades com base na população e renda dos distritos.
3. __Seleção do Endereço de Origem:__

   - Escolha aleatória de uma rua com `numpy.random.choice`.
   - Geração de um número dentro do intervalo válido.
4. __Seleção do Endereço de Destino:__

   - Use $P(d)$ para calcular o raio máximo:

     $$
     P(d) = \frac{1}{\sqrt{2\pi}\sigma} e^{-\frac{d^2}{2\sigma^2}}
     $$

   - Escolha uniformemente dentro do raio usando coordenadas do grafo.

5. **Visualização dos Resultados:**
   - Use `matplotlib` ou `folium` para criar mapas interativos.

---

# Visualização e Validação

Após a implementação, as corridas simuladas podem ser visualizadas em mapas interativos:

- **Mapas com `folium`:** Representam pares de origem e destino.
- **Gráficos:** Analisam estatisticamente a distribuição das distâncias e destinos.

Além disso, as corridas geradas podem ser exportadas para arquivos `.csv` para uso em modelos de treinamento.

---