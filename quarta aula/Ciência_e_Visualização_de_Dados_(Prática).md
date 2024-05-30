# Ciência e Visualização de Dados (Prática)

Complete a tabela de metadados que sera utilizada para fazer sentido dos nossos dados

| library | total_reads | library_treatment | collection_date |
| --- | --- | --- | --- |
| SRR12893565 | 12698221 |  |  |
| SRR12893566 | 15539404 |  |  |
| SRR12893567 | 11859587 |  |  |
| SRR12893571 | 14962000 |  |  |
| SRR12893572 | 14454900 |  |  |
| SRR12893573 | 10499400 |  |  |
| SRR12893575 | 16493955 |  |  |

### **1. Carregamento dos Dados**

- Carregue a tabela de contagens, as informações da biblioteca e os títulos das cepas:
    - `counts`: Carregue os dados que contêm as leituras por *Wolbachia* para cada biblioteca e cepa.
    - `Aegypti_controle_metadados`: Carregue os dados que incluem as leituras trimadas, o tipo de tratamento da biblioteca e o título associado a cada cepa de *Wolbachia*.

### **2. Preparação dos Dados**

- **Adicionar informações sobre biblioteca na tabela com `left_join`**
- Use `left_join` para combinar `counts` com `Aegypti_controle_metadados`
- **Cálculo de RPM**:
    - Calcule o RPM dividindo a contagem de alinhamento pelo total de leituras trimadas e multiplique por 1.000.000. Adicione esta nova coluna `rpm` ao dataframe `counts`.

### **3. Preparação de Dados para Heatmap**

- **Transformação dos Dados para Matriz**:
    - Use `dcast` para transformar `counts` em uma matriz onde as linhas são combinações de biblioteca e tratamento e as colunas são as cepas de *Wolbachia*. Use `rpm` como a variável de valor.
    - Substitua valores NA por 0 para evitar problemas com a matriz.
- **Finalização da Matriz**:
    - Converta `d_counts` para uma matriz (`as.matrix`), removendo as colunas de biblioteca e tratamento, pois essas serão usadas como nomes de linhas.
    - Defina os nomes das linhas combinando os valores de biblioteca e tratamento para formar identificadores únicos para cada linha.

## Visualização de Dados (Pacote complexheatmap)

- Carregue os pacotes necessários:
    - `ComplexHeatmap`: Pacote principal para criação de heatmaps.
    - `circlize`: Fornece funcionalidades adicionais de coloração, útil em conjunto com `ComplexHeatmap`.
    - `viridis`: Para uma paleta de cores que é visualmente acessível e esteticamente agradável.

### **1. Preparação dos Dados**

- Certifique-se de que a matriz `d_counts_av0_matrix` está corretamente formatada e pronta para ser usada no plot de heatmap. Esta matriz deve ter as bibliotecas como linhas e os contigs virais como colunas.

### **2. Plotagem do Heatmap**

- **Configuração Básica**:
    - Defina um nome para a legenda "RPM", .
    - Especifique os títulos para as linhas e para o plot, respectivamente "Libraries" e "Wolb Alignment”.
- **Distâncias e Métodos de Agrupamento**:
    - Escolha a distância de agrupamento para as linhas e colunas. Por exemplo, use "manhattan" para as linhas e "euclidean" para as colunas.  (escolha de métrica de distância)
    - Escolha o método de agrupamento para as linhas e colunas. Por exemplo, "ward.D" para as linhas e "complete" para as colunas. (métodos de agrupamento)
- **Personalização de Cores**:
    - Utilize a paleta de cores `viridis` para o mapeamento dos valores. Explique como o número de cores (ex.: 100) pode afetar a granularidade e interpretação dos dados no heatmap.

### Melhore a visualização :)

obs: da pra fazer e ensinar testes estatisticos fazendo analise dos dias ou do tipo de biblioteca, alem de comentar sobre shiny e sobre plotly