# Introdução ao R

# **Aula: Introdução ao R**

---

## **1. Apresentação da Interface do RStudio**

O RStudio é um ambiente de desenvolvimento integrado (IDE) que torna o uso do R mais intuitivo e organizado. Ele é dividido em quatro painéis principais:

- **Console:** Onde os comandos são executados imediatamente. Pense nele como uma calculadora interativa.
- **Editor de Código:** Espaço para escrever scripts completos, permitindo salvar e reutilizar seu código.
- **Environment/Variáveis:** Mostra todas as variáveis e objetos criados na sessão.
    - **Conceito de Variáveis:** Imagine variáveis como "gavetas" que armazenam valores ou dados.
        - **Exemplo:** `x <- 5` cria uma gaveta chamada `x` que guarda o número 5.

---

## **2. Usando o R como Calculadora**

O R pode realizar desde operações simples até cálculos complexos.

- **Operações Básicas:**
    
    ```r
    5 + 7  # Retorna 12
    ```
    
- **Criando Variáveis e Vetores:**
    
    ```r
    x <- -12  # Atribui -12 à variável x
    y <- c(-12, 6, 0, -1)  # Cria um vetor y com quatro elementos
    ```
    
- **Operações com Variáveis:**
    
    ```r
    2 * y      # Multiplica cada elemento de y por 2
    abs(y)     # Retorna o valor absoluto de cada elemento de y
    ```
    

---

## **3. Trabalhando com Pacotes**

Pacotes expandem as funcionalidades do R, adicionando novas funções e ferramentas.

- **Instalação e Carregamento de Pacotes:**
    
    ```r
    install.packages("nome_do_pacote")  # Instala o pacote
    library(nome_do_pacote)             # Carrega o pacote na sessão atual
    ```
    

Importando conjunto de dados

```r
scooby <- read_csv("Scooby-Doo Completed.csv")  # Importa o dataset
```

---

## **4. Explorando o Dataset Scooby**

Vamos analisar informações básicas sobre o dataset.

- Use ?mean para ler uma documentação sobre o comando.
- **Calculando a Média da Duração dos Episódios:**
    
    ```r
    mean(scooby$run.time)  # Calcula a média da coluna 'runtime'
    ```
    
- **Lidando com Valores Ausentes (NA):**
    
    ```r
    mean(scooby$imdb)          # Pode retornar NA se houver valores ausentes
    str(scooby$imdb)           # Fala o tipo de coluna (character)
    scooby$imdb <- as.numeric(scooby$imdb) # Converta para numerico
    mean(scooby$imdb, na.rm = TRUE)  # Remove NAs antes de calcular a média
    ```
    
    - O argumento `na.rm = TRUE` ignora os valores ausentes durante o cálculo.
    - A função `str()` exibe a estrutura interna de um objeto em R, incluindo o tipo de cada coluna. Se `scooby$imdb` for do tipo `character` ou `factor`, precisamos convertê-la.
    - **`as.numeric()`**: Converte os valores para o tipo numérico, **`scooby$imdb`**: Especifica a coluna que queremos converter e **Atribuição (`<-`)**: Reatribui os valores convertidos à mesma coluna no dataset `scooby`.

---

## **5. Introdução ao Tidyverse**

O `tidyverse` é um conjunto de pacotes para ciência de dados que compartilham uma filosofia e estruturas de dados comuns.

- **Instalação e Carregamento:**
    
    ```r
    install.packages("tidyverse")
    library(tidyverse)
    ```
    
- **Explorando Dados com `glimpse`:**
    
    ```r
    glimpse(scooby)  # Visualiza a estrutura e os tipos de dados do dataset
    ```
    

---

## **6. Filtrando Dados**

Filtrar permite extrair subconjuntos de dados com base em condições.

- **Filtrar Episódios com Duração Menor que 19 Minutos:**
    
    ```r
    filter(scooby, run.time < 19)
    ```
    
- **Salvar o Filtro em um Novo Dataset:**
    
    ```r
    scooby_short <- filter(scooby, run_time < 19)
    ```
    
- **Filtrar por Título Específico:**
    
    ```r
    scooby_title <- filter(scooby, series.name == "Scooby Doo, Where Are You!")
    ```
    

---

## **7. Criando Novas Colunas com `mutate`**

`mutate` adiciona novas colunas ou modifica existentes.

- **Exemplo: Convertendo Duração em Minutos para Horas:**
    
    ```r
    scooby <- mutate(scooby, runtime_hours = run.time / 60)
    ```
    

---

## **8. Usando Pipes (`%>%`) para Combinar Comandos**

O operador pipe `%>%` permite encadear funções de forma legível.

- **Exemplo de Encadeamento:**
    
    ```r
    scooby_short <- scooby %>%
      filter(run.time < 19) %>%
      mutate(runtime_hours = run.time / 60)
    ```
    
    - **Explicação:** Filtramos os episódios com menos de 19 minutos e criamos uma nova coluna com a duração em horas.

---

## **9. Agrupando e Sumarizando Dados**

Agrupar dados é útil para calcular estatísticas por grupos.

- **Exemplo: Estatísticas por Temporada:**
    
    ```r
    scooby %>%
      group_by(season) %>%
      summarize(
        mean_runtime = mean(runtime, na.rm = TRUE),
        median_runtime = median(runtime, na.rm = TRUE)
      )
    ```
    

---

## **10. Introdução à Visualização de Dados com `ggplot2`**

O `ggplot2` é uma ferramenta poderosa para criar gráficos personalizados.

- **Usando Comentários:**
    
    ```r
    # Criando um histograma da duração dos episódios
    ```
    
- **Gráfico Simples:**
    
    ```r
    ggplot(scooby, aes(x = run.time)) +
      geom_histogram(binwidth = 1, fill = "blue", color = "black")
    ```
    
- **Comparando com `geom_freqpoly()`:**
    
    ```r
    ggplot(scooby, aes(x = run.time)) +
      geom_freqpoly(binwidth = 1, color = "red")
    ```
    
    - **Explicação:** O histograma mostra a distribuição em barras, enquanto o `freqpoly` usa linhas.
- Unificado

```bash
ggplot(scooby, aes(x = run.time)) + 
geom_freqpoly(binwidth = 1, color = "red") + 
geom_histogram(binwidth = 1, fill = "blue", color = "black", alpha=0.6)

```

---

## **11. Trabalhando com Variáveis no Gráfico**

Adicionar mais dimensões enriquece a análise visual.

- **Gráfico de Dispersão com Linha de Tendência:**
    
    ```r
    ggplot(scooby, aes(x = run.time, y = imdb)) +
      geom_point(alpha = 0.6) +
      geom_smooth(method = "lm", se = FALSE, color = "blue")
    ```
    
    - **Explicação:** `geom_point()` plota os pontos; `geom_smooth()` adiciona uma linha de regressão linear.
- **Colorindo por Temporada e Usando Paletas:**
    
    ```r
    ggplot(scooby, aes(x = run.time, y = imdb, color = season)) +
      geom_point() +
      geom_smooth(method = "lm", se = FALSE) +
      scale_color_brewer(palette = "Dark2")
    ```
    

---

## **12. Introdução ao RMarkdown**

O RMarkdown permite combinar texto, código e resultados em um único documento dinâmico.

- **Por que Usar RMarkdown:**
    - Ideal para relatórios reproduzíveis.
    - Facilita o compartilhamento de análises com código e resultados integrados.
- **Criando um Arquivo `.Rmd`:**
    - No RStudio, vá em **File > New File > R Markdown...**.
    - Use blocos de código R delimitados por três crases e `{r}`:
        
        ```markdown
        ```{r}
        # Seu código aqui
        
        ggplot(scooby, aes(x = run.time)) + 
        geom_freqpoly(binwidth = 1, color = "red") + 
        geom_histogram(binwidth = 1, fill = "blue", color = "black", alpha=0.6)
        ```
        
- **Configurando a Exibição do Código:**
    - Controle a exibição com opções como `echo = FALSE` para ocultar o código:
        
        ```markdown
        ```{r echo=FALSE}
        # Este código não será exibido no documento final
        ```
        

---

**Parabéns!** Você explorou os conceitos fundamentais do R, caso queira aprofundar eu aconselho tentar analisar esse dataset, entender ele e tentar criar seus proprios plots (use o chatgpt a vontade) e crie um documento em Rmarkdown para simular uma apresentação de dados mais realista, sinta-se a vontade para adicionar isso posteriormente no seu proprio github.

Exemplos (resposta no final da pagina)

1. **Analisar os Episódios Mais Bem Avaliados**
    - Usaremos os dados de IMDb para encontrar os episódios favoritos dos fãs.
2. **Descobrir a Frequência de Personagens Extras**
    - Vamos analisar com que frequência personagens como Batman ou Hex Girls aparecem nos episódios.
3. **Ver Temporadas mais Icônicas**
    - Identificar qual temporada teve maior engajamento médio.

## **Introdução à Visualização de Dados com ggplot2**

![image.png](/images/escolha_plot.png)

---

## **1. Fundamentos do ggplot2**

O **ggplot2** é um pacote baseado na **Gramática dos Gráficos**, permitindo criar visualizações complexas de forma sistemática.

- **Estrutura Básica de um Gráfico com ggplot2:**
    
    ```r
    ggplot(data = <DATASET>, aes(x = <X-VARIÁVEL>, y = <Y-VARIÁVEL>)) +
      <GEOM_FUNÇÃO>()
    
    ```
    
    - **`data`**: O conjunto de dados que será utilizado.
    - **`aes()`**: A estética do gráfico, onde mapeamos as variáveis aos atributos visuais (eixos, cores, formas).
    - **`geom_*()`**: A camada geométrica que define o tipo de gráfico (pontos, linhas, barras).
- **Exemplo Prático:**
    
    Vamos utilizar o dataset `crickets` para explorar a relação entre a temperatura e a frequência do piar dos grilos.
    
    ```r
    # Carregando os pacotes necessários
    library(tidyverse)
    install.packages("modeldata")
    library(modeldata)
    
    # Visualizando a documentação do dataset
    ?crickets
    
    # Visualizando o dataset
    View(crickets)
    
    # Criando um gráfico básico
    ggplot(crickets, aes(x = temp, y = rate)) +
      geom_point()
    ```
    
    - **Explicação:**
        - **`crickets`**: Dataset contendo informações sobre grilos.
        - **`aes(x = temp, y = rate)`**: Mapeia a temperatura (`temp`) no eixo X e a taxa de piados (`rate`) no eixo Y.
        - **`geom_point()`**: Adiciona pontos ao gráfico, criando um gráfico de dispersão.

---

## **2. Aprimorando o Gráfico Básico**

Vamos tornar nosso gráfico mais informativo e esteticamente agradável.

- **Adicionando Títulos e Rótulos:**
    
    ```r
    ggplot(crickets, aes(x = temp, y = rate)) +
      geom_point() +
      labs(
        x = "Temperature",
        y = "Frequencia de Piar",
        title = "Piar de Grilos",
        caption = "Fonte: MacDonald (2009)"
      )
    ```
    
    - **`labs()`**: Permite adicionar títulos e rótulos aos eixos e ao gráfico.
        - **`x`**: Rótulo do eixo X.
        - **`y`**: Rótulo do eixo Y.
        - **`title`**: Título principal do gráfico.
        - **`caption`**: Legenda ou fonte dos dados.
- **Diferenciando Espécies por Cor:**
    
    ```r
    ggplot(crickets, aes(x = temp, y = rate, color = species)) +
      geom_point() +
      labs(
        x = "Temperature",
        y = "Frequencia de Piar",
        color = "Especies:",
        title = "Piar de Grilos",
        caption = "Fonte: MacDonald (2009)"
      )
    ```
    
    - **`color = species`**: Mapeia a variável `species` à cor dos pontos, diferenciando as espécies.
    - **Alterando a Legenda da Cor**:
        - Dentro de `labs()`, usamos **`color = "Espécies:"`** para alterar o título da legenda.
- **Utilizando uma Paleta de Cores Acessível:**
    
    ```r
    ggplot(crickets, aes(x = temp, y = rate, color = species)) +
      geom_point() +
      labs(
        x = "Temperature",
        y = "Frequencia de Piar",
        color = "Especies:",
        title = "Piar de Grilos",
        caption = "Fonte: MacDonald (2009)"
      ) +
      scale_color_brewer(palette = "Dark2")
    ```
    
    - **`scale_color_brewer(palette = "Dark2")`**: Aplica uma paleta de cores que é amigável para pessoas com daltonismo.

---

## **3. Personalizando Propriedades do Gráfico**

Vamos explorar como modificar aspectos visuais dos nossos gráficos.

- **Alterando a Aparência dos Pontos:**
    
    ```r
    ggplot(crickets, aes(x = temp, y = rate)) +
      geom_point(color = "red", size = 2, alpha = 0.3, shape = "square") +
      labs(
        x = "Temperature",
        y = "Frequencia de Piar",
        title = "Piar de Grilos",
        caption = "Fonte: MacDonald (2009)"
      )
    ```
    
    - **Parâmetros em `geom_point()`:**
        - **`color = "red"`**: Define a cor dos pontos como vermelho.
        - **`size = 2`**: Aumenta o tamanho dos pontos.
        - **`alpha = 0.3`**: Define a transparência dos pontos, útil para evitar sobreposição excessiva.
        - **`shape = "square"`**: Altera o formato dos pontos para quadrados.
- **Adicionando uma Linha de Regressão Linear:**
    
    ```r
    ggplot(crickets, aes(x = temp, y = rate)) +
      geom_point() +
      geom_smooth(method = "lm", se = FALSE) +
      labs(
        x = "Temperature",
        y = "Frequencia de Piar",
        title = "Piar de Grilos",
        caption = "Fonte: MacDonald (2009)"
      )
    ```
    
    - **`geom_smooth(method = "lm", se = FALSE)`**:
        - **`method = "lm"`**: Aplica um modelo linear aos dados.
        - **`se = FALSE`**: Remove a faixa de erro padrão da visualização.
- **Separando Regressões por Espécie:**
    
    ```r
    ggplot(crickets, aes(x = temp, y = rate, color = species)) +
      geom_point() +
      geom_smooth(method = "lm", se = FALSE) +
      labs(
        x = "Temperature",
        y = "Frequencia de Piar",
        color = "Especies:",
        title = "Piar de Grilos",
        caption = "Fonte: MacDonald (2009)"
      ) +
      scale_color_brewer(palette = "Dark2")
    ```
    
    - Agora, tanto os pontos quanto as linhas de regressão são diferenciados por espécie, permitindo uma análise comparativa.

---

## **4. Explorando Outros Tipos de Gráficos**

Dependendo do tipo de dados, diferentes gráficos podem ser mais apropriados.

- **Histogramas (Variável Quantitativa):**
    
    ```r
    ggplot(crickets, aes(x = rate)) +
      geom_histogram(bins = 15, fill = "lightblue", color = "black")
    ```
    
    - **`bins = 15`**: Define o número de intervalos no histograma.
- **Gráfico de Frequência Poligonal:**
    
    ```r
    ggplot(crickets, aes(x = rate)) +
      geom_freqpoly(bins = 15, color = "darkgreen")
    ```
    
    - Uma alternativa ao histograma, representando a distribuição com linhas.
- **Gráfico de Barras (Variável Categórica):**
    
    ```r
    ggplot(crickets, aes(x = species)) +
      geom_bar(color = "black", fill = "lightblue")
    ```
    
    - Útil para visualizar a contagem de observações em cada categoria.
- **Boxplot (Comparação entre Grupos):**
    
    ```r
    ggplot(crickets, aes(x = species, y = rate, color = species)) +
      geom_boxplot(show.legend = FALSE) +
      scale_color_brewer(palette = "Dark2") +
      theme_minimal()
    ```
    
    - **`geom_boxplot()`**: Exibe a distribuição dos dados por grupo.
    - **`theme_minimal()`**: Aplica um tema clean ao gráfico.

---

## **5. Utilizando Facet_wrap para Comparação**

As facet_wrap permitem criar múltiplos gráficos baseados em subgrupos dos dados.

- **Problema sem Facetas:**
    
    ```r
    ggplot(crickets, aes(x = rate, fill = species)) +
      geom_histogram(bins = 15) +
      scale_fill_brewer(palette = "Dark2")
    ```
    
    - A sobreposição das distribuições pode dificultar a interpretação.
- **Aplicando Facetas:**
    
    ```r
    ggplot(crickets, aes(x = rate, fill = species)) +
      geom_histogram(bins = 15, show.legend = FALSE) +
      facet_wrap(~species) +
      scale_fill_brewer(palette = "Dark2") +
      theme_minimal()
    ```
    
    - **`facet_wrap(~species)`**: Cria um gráfico separado para cada espécie.
    - **`ncol = 1`** (opcional): Alinha os gráficos em uma única coluna.
- **Exemplo com Facetas em Coluna Única:**
    
    ```r
    ggplot(crickets, aes(x = rate, fill = species)) +
      geom_histogram(bins = 15, show.legend = FALSE) +
      facet_wrap(~species, ncol = 1) +
      scale_fill_brewer(palette = "Dark2") +
      theme_minimal()
    ```
    

---

## **6. Temas e Estilos**

Personalize o estilo geral do seu gráfico com temas predefinidos ou crie o seu próprio.

- **Aplicando um Tema Minimalista:**
    
    ```r
    ggplot(crickets, aes(x = temp, y = rate, color = species)) +
      geom_point() +
      geom_smooth(method = "lm", se = FALSE) +
      labs(
        x = "Temperature",
        y = "Frequencia de Piar",
        color = "Especies:",
        title = "Piar de Grilos",
        caption = "Fonte: MacDonald (2009)"
      ) +
      scale_color_brewer(palette = "Dark2") +
      theme_minimal()
    ```
    
- **Explorando Outros Temas:**
    
    ```r
    ?theme_minimal  # Para ver detalhes sobre o tema minimalista
    ```
    
    - Experimente outros temas como `theme_bw()`, `theme_classic()`, `theme_void()` para ver diferentes estilos.

---

## **7. Dicas e Boas Práticas**

- **Consulte a Documentação:**
    - Use `?ggplot2`, `?geom_point`, `?facet_wrap` para acessar a ajuda e descobrir novas funcionalidades.
- **Personalização Adicional:**
    - Explore parâmetros adicionais em geoms, scales e themes para aprimorar seus gráficos.
- **Acessibilidade:**
    - Sempre considere paletas de cores acessíveis para tornar seus gráficos inclusivos.

## Exercicios Extras: Dataset Scooby Doo

### Exemplos de Inferências em R:

1. **Analisar os Episódios Mais Bem Avaliados**
    - Usaremos os dados de IMDb para encontrar os episódios favoritos dos fãs.
2. **Descobrir a Frequência de Personagens Extras**
    - Vamos analisar com que frequência personagens como Batman ou Hex Girls aparecem nos episódios.
3. **Ver Temporadas mais Icônicas**
    - Identificar qual temporada teve maior engajamento médio.

1. 

```r
# Encontrar os episódios mais bem avaliados no IMDb
top_episodes <- scooby[order(-scooby_data$imdb), ][1:5, c("title", "imdb")]
print("Episódios mais bem avaliados:")
print(top_episodes)
```

*Explicação:* Aqui, ordenamos os episódios pelo IMDb e exibimos os 5 mais bem avaliados.

2. 

```r
# Contar quantas vezes cada personagem aparece
special_chars <- colSums(scooby[, c("batman", "scooby.dum", "scrappy.doo", "hex.girls", "blue.falcon")])
print(special_chars)
```

3. 

```r
# Calcular o engajamento médio por temporada
scooby$engagement <- as.numeric(as.character(scooby$engagement))
season_engagement <- aggregate(engagement ~ season, data = scooby, mean)
top_season <- season_engagement[which.max(season_engagement$engagement), ]
print(top_season)

```