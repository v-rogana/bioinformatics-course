# Plotando Dados no R com ComplexHeatmap

## **1. Carregamento dos Dados**

- Carregue a tabela de contagens, as informações da biblioteca e os títulos das cepas:
    - `counts`: Carregue os dados que contêm as leituras por *Wolbachia* para cada biblioteca e cepa.
    - `Library_metadados`: Carregue os dados que incluem as leituras trimadas, o tipo de tratamento da biblioteca e o título associado a cada cepa de *Wolbachia*.
    - `Wolbs_title`: Carregue os dados que incluem os nomes das cepas associados ao código de identificação.

```bash
# Carregando os dados
counts = read.csv("caminho_no_seu_computador/readcounts.csv", header=FALSE) # (lib/strain/count)
metadados_wolb = read_xlsx("caminho_no_seu_computador/metadados_wolb.xlsx") # informações da biblioteca (reads trimadas [essa info esta no log #reads processed] + tipo de tratamento)
wolbs_title = read_xlsx("caminho_no_seu_computador/wolbs_title.xlsx")# titulo associado a cepa [essa info esta no fasta de sequencia]
```

### **2. Preparação dos Dados**

- Carregue o pacote `dplyr` .
- Adicionar metadados sobre biblioteca na tabela com **`inner_join`** para combinar `counts` com `Library_metadados`
    - Use a coluna da biblioteca para concatenar as duas, entenda a estrutura do `inner**_join`**

Fazer um join ou um merge nada mais é do que juntar dois conjuntos de dados por meio de um ou mais campos em comum (chaves/key), vantagens:
● Sempre preserva a ordem das linhas originais
● Sintaxe mais intuitiva

### **Inner Join**

inner_join(x, y): Retorna todas as linhas de x onde existem valores correspondentes em y e todas as colunas de x e y. Se houver múltiplas combinações entre x e y, todas as combinações das correspondências são retornadas.

![Untitled](/images/inner_join.png)

Resumindo, o inner join retorna todos os campos de ambos os data.frames, mas somente as linhas em que as chaves são iguais.

- Adicionar os nomes das cepas com o `inner**_join`** para combinar `counts` com `wolbs_title`
    - Use a coluna de identificação da cepa/accession_version (ex: CP031221.1)

### **Cálculo de RPM**:

O **RPM (Reads Per Million)** é uma métrica usada em bioinformática para normalizar os contagens de leituras em bibliotecas de tamanhos diferentes. A ideia é ajustar os valores para que as contagens de diferentes bibliotecas possam ser comparadas de maneira proporcional.

### Fórmula do RPM

A fórmula para calcular o RPM é:

$$
RPM=read counts / library size * 10^6
$$

- **read counts**: O número de leituras mapeadas para um elemento específico (por exemplo, um gene ou sequência).
- **library size**: O número total de leituras sequenciadas na biblioteca (coluna `lib_size`).
- 10^6: Um fator de escala para normalizar o valor e expressar em "reads por milhão".

**Adicionar uma coluna RPM ao dataset**:
Vamos criar uma nova coluna no dataset para armazenar o valor do RPM.

```r
# Calcular RPM para cada linha
counts_av0 <- counts_av0 %>%
  mutate(RPM = (read_counts / lib_size) * 1e6)
```

**Explicação do código**:

- `mutate()`: Adiciona ou modifica colunas em um dataset.
- `read_counts / lib_size`: Divide o número de leituras pelo tamanho da biblioteca.
- `1e6`: Multiplica o resultado por  para normalizar em "reads por milhão".

### Demonstração Didática

Imagine que temos um exemplo específico da tabela:

| read_counts | lib_size | RPM |
| --- | --- | --- |
| 120 | 13,201,822 | 9.09 |

Para o cálculo:

1. Dividimos `120` por `13,201,822`= 0.00000909
2. Multiplicamos o resultado por 10^6= `9.09`
3. Isso permite comparar dados entre bibliotecas de tamanhos diferentes, eliminando o impacto do número total de leituras.

### Adicionar Nome da Cepa

No dataset `counts_av0`, a coluna `cepa` contém identificadores que começam com o prefixo `"NZ_"`. No entanto, o dataset `wolbs` usa o mesmo identificador sem esse prefixo, na coluna `AccessionVersion`. Para unir os dois datasets com um **inner join**, precisamos primeiro remover o prefixo `"NZ_"` de `cepa`.

1. **Remoção do Prefixo `"NZ_"`**:
    - Usamos a função `sub()` para substituir `"NZ_"` por uma string vazia `""`.
    - Isso cria uma nova coluna `corrected` em `counts_av0` com os identificadores corrigidos.
    
    ```r
    counts_av0$corrected <- sub("NZ_", "", counts_av0$cepa)
    ```
    
    **Exemplo**:
    
    - Antes: `"NZ_CP010981.1"`
    - Depois: `"CP010981.1"`
2. **Realizar o Inner Join**:
    - O parâmetro `by = c("corrected"="AccessionVersion")` indica que a junção será feita entre a coluna `corrected` de `counts_av0` e `AccessionVersion` de `wolbs`.
    
    ```r
    counts_av0 <- inner_join(counts_av0, wolbs, by = c("corrected"="AccessionVersion"))
    ```
    

## **3. Exercício : Transformação dos Dados para Matriz**

Nesta etapa, você irá reorganizar os dados em um formato de matriz, no qual as linhas representam combinações únicas de biblioteca e tratamento, enquanto as colunas representam diferentes cepas de *Wolbachia*. Essa transformação é fundamental para gerar visualizações como heatmaps.

1. **Carregue o Pacote Necessário**
    - Utilize a função `library()` para carregar o pacote **`reshape2`**, que contém a função `dcast`.
2. **Estrutura da Função `dcast`**
    
    A função `dcast` reestrutura os dados de um formato longo para um formato largo. Para este exercício, você deverá configurar:
    
    - **Fórmula**: Identifique quais colunas serão usadas para formar as linhas e quais formarão as colunas.
    Exemplo: `linha1 + linha2 ~ coluna1 + coluna2`
    - **`value.var`**: Nome da coluna que conterá os valores a serem preenchidos nas células. Neste caso, os valores de RPM.
    - **Dados**: O dataframe de entrada, que já deve conter as colunas necessárias (biblioteca, tratamento, cepa, e valores de RPM).
    
    ```r
    d_counts_av0 <- dcast(dados, formula, value.var = "XXX")
    ```
    
3. **Configuração da Matriz**
Após usar `dcast`, transforme o resultado em uma matriz com os seguintes passos:
    - Converta o dataframe em matriz com `as.matrix()`, excluindo as colunas descritivas que não fazem parte da matriz numérica.
    
    ```r
    d_counts_av0_matrix <- as.matrix(d_counts_av0[, c(-1, -2)])
    ```
    
    - Atribua nomes às linhas da matriz usando `paste0()`, que concatena os valores das colunas `library` e `tratamento_tetraciclina` para criar identificadores únicos.
    
    ```r
    row.names(d_counts_av0_matrix) <- paste0(d_counts_av0[, 1], "_", d_counts_av0[, 2])
    ```
    

### **Exemplo Estruturado de Resultado**

Suponha que os dados originais tenham as seguintes colunas:

- `library`: Nome da biblioteca.
- `tratamento_tetraciclina`: Indica se a biblioteca foi tratada ou não.
- `Title_reduced`: Nome reduzido da cepa de *Wolbachia*.
- `corrected`: Versão corrigida do identificador da cepa.
- `RPM`: Valores de leitura normalizados por milhão.

Ao final, o formato esperado será:

### **Dataframe Largo Após `dcast`**

| Library | tratamento_tetraciclina | strain1 | strain2 | strain3 |
| --- | --- | --- | --- | --- |
| Lib1 | Tratado | 10.5 | 20.3 | 15.7 |
| Lib2 | Não tratado | 12.7 | 18.9 | 14.2 |

### **Matriz Transformada**

| row.names | strain1 | strain2 | strain3 |
| --- | --- | --- | --- |
| Lib1_Tratado | 10.5 | 20.3 | 15.7 |
| Lib2_Não tratado | 12.7 | 18.9 | 14.2 |

### Reflita sobre os seguintes pontos:

- Por que é importante que os nomes das linhas sejam únicos?
- Como os valores na matriz (RPM) podem ser usados em análises subsequentes, como a criação de heatmaps?

## 4. Exercicio: Visualização de Dados (Pacote complexheatmap)

- Carregue os pacotes necessários:
    - `ComplexHeatmap`: Pacote principal para criação de heatmaps.
    - `viridis`: Para uma paleta de cores que é visualmente acessível e esteticamente agradável.

### **Preparação dos Dados**

- Certifique-se de que a matriz `d_counts_av0_matrix` está corretamente formatada e pronta para ser usada no plot de heatmap. Esta matriz deve ter as bibliotecas como linhas e contagem de alinhamento para cada cepa nas colunas.

### **Plotagem do Heatmap**

- **Configuração Básica**:
    - Defina um nome para a legenda "RPM", .
    - Especifique os títulos para as linhas e para o plot, respectivamente "Bibliotecas de Pequenos RNAs" e "Cepas de Wolbachia”.
        - exemplo: `row_title = "Bibliotecas de Pequenos RNAs", # Titulo Rows`
- **Personalização de Cores**:
    - Utilize a paleta de cores `viridis` para o mapeamento dos valores. Explique como o número de cores (ex.: 100) pode afetar a granularidade e interpretação dos dados no heatmap.

Use o exemplo como base, e aprenda o basico da estrutura do [complexheatmap](https://jokergoo.github.io/ComplexHeatmap-reference/book/introduction.html).

```bash
Heatmap(counts_matrix,
        name = "RPM", # Titulo Legenda
        col = viridis(100) # Paleta de cores
)
```

### Melhore a visualização :)

[https://jokergoo.github.io/ComplexHeatmap-reference/book/a-single-heatmap.html](https://jokergoo.github.io/ComplexHeatmap-reference/book/a-single-heatmap.html)

## 5. Análise Estatística: Teste ANOVA

```r
# Converter a variável para fator, se ainda não for
counts_av0$tratamento_tetraciclina <- as.factor(counts_av0$tratamento_tetraciclina)

# Realizar o teste ANOVA
anova_result <- aov(RPM ~ tratamento_tetraciclina, data = counts_av0)

# Resumo do resultado ANOVA
anova_summary <- summary(anova_result)
anova_summary
```

---

### Interpretação dos Resultados

### Hipóteses

- **H_0 (Hipótese Nula)**: Não há diferença significativa nos valores médios de RPM entre os grupos (tratados e não tratados com tetraciclina).
- **H_1 (Hipótese Alternativa)**: Existe uma diferença significativa nos valores médios de RPM entre os grupos.

### Resultados Obtidos

- **Graus de liberdade (`Df`)**:
    - `Dftratamento`:  (número de grupos - 1). [2-1 = 1]
    - `Dfresiduo`:  (total de observações - número de grupos). [1206 - 2 =1204]
- **Soma dos Quadrados (`Sum Sq`)**:
    - `1.033×10^11`: Variabilidade explicada pelo tratamento.
    - `1.356×10^12`: Variabilidade residual (não explicada).
- **Estatística F**:
    - `F = 91.68`: Relação entre a variabilidade explicada pelos grupos e a variabilidade residual.
- **pvalue**:
    - `p<2×10^−16`: Extremamente pequeno, indicando uma diferença altamente significativa entre os grupos.

### Conclusão

- Como , rejeitamos  e concluímos que o tratamento com tetraciclina influencia significativamente os valores médios de RPM.
- Essa diferença observada entre os grupos tratados e não tratados não pode ser atribuída ao acaso.