# Ciência e Visualização de Dados (Prática)

Baixe os arquivos xlsx na pasta da quarta aula que vão servir de metadados para fazer sentido do seu dataset na hora de plotar no R.
### **1. Carregamento dos Dados**

- Carregue a tabela de contagens, as informações da biblioteca e os títulos das cepas:
    - `counts`: Carregue os dados que contêm as leituras por *Wolbachia* para cada biblioteca e cepa.
    - `Library_metadados`: Carregue os dados que incluem as leituras trimadas, o tipo de tratamento da biblioteca e o título associado a cada cepa de *Wolbachia*.
    - `Wolbs_title`: Carregue os dados que incluem os nomes das cepas associados ao código de identificação.

```bash
# Carregando os dados
library(readxl)
counts = read.csv("caminho_no_seu_computador/readcounts.csv", header=FALSE) # (lib/strain/count)
metadados_wolb = read_xlsx("caminho_no_seu_computador/metadados_wolb.xlsx") # informações da biblioteca (reads trimadas [essa info esta no log #reads processed] + tipo de tratamento)
wolbs_title = read_xlsx("caminho_no_seu_computador/wolbs_title.xlsx")# titulo associado a cepa [essa info esta no fasta de sequencia]
```

### **2. Preparação dos Dados**

- Carregue o pacote `dplyr` .
- Adicionar metadados sobre Livraria na tabela com **`left_join`** para combinar `counts` com `Library_metadados`
    - Use a coluna da Livraria para concatenar as duas, entenda a estrutura do **`left_join`**
- Adicionar os nomes das cepas com o **`left_join`** para combinar `counts` com `wolbs_title`
    - Use a coluna de identificação da cepa/accession_version (ex: CP031221.1)
- **Cálculo de RPM**:
    - Calcule o RPM dividindo a contagem de alinhamento pelo total de leituras trimadas e multiplique por 1e6 a quantidade de leituras trimadas. Adicione esta nova coluna `rpm` ao dataframe `counts`.

### **3. Transformação dos Dados para Matriz**:

- Utilize a função `dcast` para converter o dataframe `counts` em uma matriz. As linhas devem representar combinações únicas de biblioteca e tratamento, enquanto as colunas devem representar as diferentes cepas de *Wolbachia*.
    - Carregue o pacote `reshape2` no R.
    - Especifique `Library` e `library_treatment` como os identificadores das linhas (`Library + library_treatment`) e `Title_reduced + accession version` como as colunas.
    - Atribua `rpm` como a variável de valor usando `value.var="rpm"`. Isso determinará os valores dentro de cada célula da matriz, correspondendo às leituras por milhão para cada cepa nas respectivas combinações de biblioteca e tratamento.
- Resultado final:

| Library | tratamento_tetraciclina | strain1 | strain2 |
| --- | --- | --- | --- |
| Lib1 | Trat1 | rpm | rpm |
| Lib2 | Trat2 | rpm | rpm |
- Substitua todos os valores NA por 0 no dataframe transformado. Isso é crucial para evitar problemas durante análises estatísticas ou operações matemáticas que não lidam bem com NA, usando o comando `is.na`.

**Finalização da Matriz**:

- Transforme o dataframe `counts` em uma matriz `counts_matrix`, com o comando `as.matrix` removendo as colunas que inicialmente identificavam as bibliotecas e tratamentos (colunas 1 e 2).
- Atribua nomes às linhas usando uma combinação dos valores de biblioteca e tratamento para criar identificadores únicos. Isso ajuda na identificação clara das linhas durante análises posteriores.
    - `row.names(counts_matrix)=paste0(counts[, 1], "_", counts[, 2])`.
- resultado final:

| row names | strain1 | strain2 |
| --- | --- | --- |
| Lib1_Trat1 | rpm | rpm |
| Lib2_Trat2 | rpm | rpm |

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
- **Personalização de Cores**:
    - Utilize a paleta de cores `viridis` para o mapeamento dos valores. Explique como o número de cores (ex.: 100) pode afetar a granularidade e interpretação dos dados no heatmap.

Use o exemplo como base, e aprenda o basico da estrutura do [complexheatmap](https://jokergoo.github.io/ComplexHeatmap-reference/book/introduction.html).

```bash
Heatmap(counts_matrix,
        name = "RPM",
)
```

### Melhore a visualização :)