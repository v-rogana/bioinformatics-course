# Algoritmos em Bioinformática (Alinhamento de Nucleotideos)

### Conectando ao Servidor

Vamos começar conectando ao servidor onde trabalharemos com os dados. Use o seguinte comando de SSH para entrar no servidor:

```bash
ssh topicos@bioinfo.icb.ufmg.br
# senha: carrarataxis
```

### Configuração do Ambiente (Não Faça essa Parte Apenas Acompanhe a Tela)

Vamos criar um ambiente micromamba com todos os pacotes necessários, incluindo `blast` para alinhamento e `emboss` para usar o algoritmo needleman-wunch.

**Criar o ambiente com os pacotes necessários**:

```bash
micromamba create -n algoritmos_de_alinhamento -c conda-forge blast biopython
micromamba activate algoritmos_de_alinhamento
micromamba install -c bioconda emboss
micromamba install -c conda-forge libiconv
```

### Passo a Passo da Aula Prática

Visualize o diretório com o path com os dados pra aual e veja as sequencias que você usara nessa primeira parte, copie eles para o seu diretório de trabalho

```bash
# Visualizar para diretorio com os dados que serão usados
ls /home/topicos/parte1_aula_alinhamento_algoritmos
# Copiar os arquivos para a minha
cp /home/topicos/parte1_aula_alinhamento_algoritmos/* .
# Visualizar arquivos que começam com seq
more seq*
```

### Parte 1: Alinhamento Global com Algoritmo Needleman-Wunch

O **algoritmo Needleman-Wunsch** é um método de alinhamento global, que busca alinhar duas sequências em toda a sua extensão. Ele usa uma abordagem de **programação dinâmica**, onde uma matriz é preenchida com pontuações para cada possível par de posições das duas sequências.

### Funcionamento do Algoritmo

1. **Construção da Matriz**: Cada célula na matriz representa um par de posições entre as duas sequências que estamos alinhando. A pontuação em cada célula reflete a "melhor pontuação possível" para alinhar as subsequências até aquele ponto.
2. **Cálculo de Pontuações**:
    - **Match** (quando os nucleotídeos/aminoácidos são iguais) recebe uma pontuação positiva.
    - **Mismatch** (diferença entre nucleotídeos) recebe uma pontuação que depende da **matriz de substituição**.
    - **Gap** recebe uma penalidade, que varia conforme os parâmetros `gapopen` e `gapextend`.
3. **Backtracking**: Após preencher a matriz, o algoritmo traça o caminho com a maior pontuação desde a célula final até o início, identificando a melhor forma de alinhar as sequências de ponta a ponta.

```bash
needle -asequence seq1.fasta -bsequence seq2.fasta -gapopen 10 -gapextend 0.5 -outfile global_alignment_example.txt
```

### Conceitos Importantes

### Penalidades de Gaps: `gapopen` e `gapextend`

- **`gapopen`**: É a penalidade aplicada ao abrir um gap (espaço onde um nucleotídeo está ausente em uma das sequências). Por exemplo, um `gapopen` de 10 indica que abrir um novo gap reduz a pontuação do alinhamento em 10.
- **`gapextend`**: Penalidade aplicada a cada nucleotídeo adicional dentro de um gap já existente. Uma penalidade menor para extensão (por exemplo, `gapextend` de 0.5) incentiva gaps contínuos em vez de múltiplos gaps pequenos.

Essas penalidades ajudam o algoritmo a "decidir" se é melhor abrir gaps maiores em uma posição ou vários gaps pequenos em locais diferentes, afetando o alinhamento final.

### Matriz de Substituição

A **matriz de substituição** é uma tabela de pontuação que define quão similar cada par de nucleotídeos (ou aminoácidos) é. No caso de sequências de DNA, a matriz **EDNAFULL** especifica pontuações para combinações entre os quatro nucleotídeos (A, T, C, G).

- **Match**: Valores altos para nucleotídeos idênticos.
- **Mismatch**: Penalidades ou pontuações baixas para combinações de nucleotídeos diferentes.
- **Identity**: Percentual de identidade entre as sequências, com 35 nucleotídeos idênticos de um total de 52 (67.3%).
- **Similarity**: Percentual de similaridade, considerando nucleotídeos idênticos e similares (no caso de proteínas, é útil para aminoácidos com propriedades semelhantes).

```bash
Sequence1          1 ATGCGTACGTAGCTAG-----CTAGCTAGCTAGCTAGCTGACTGACTGAC     45
                     ||||||||||||||||     ||..||..||..||||||||||..|||
Sequence2          1 ATGCGTACGTAGCTAGTACGACTGACTGACTGACTAGCTGACTAGCTG--     48
```

- **Alinhamento**:
    - **`|`** indica nucleotídeos idênticos.
    - **`.`** indica nucleotídeos diferentes (mismatches).
    - Gaps estão representados por ``.

### Alinhamento Local (chegando em um exemplo mais próximo da nossa aula prática)

1. **Obter as sequências das duas cepas de Wolbachia**: Você pode baixar essas sequências do NCBI, salvando-as como arquivos `.fasta`.
    
    Exemplo de comando para baixar:
    
    ```bash
    wget "https://www.ncbi.nlm.nih.gov/sviewer/viewer.fcgi?id=NZ_CP072672.1&db=nuccore&report=fasta&retmode=text" -O NZ_CP072672.1.fasta
    ```
    

### Parte 2: Preparação da Base de Dados para Alinhamento Local com BLAST

1. **Obter uma sequência de referência de Wolbachia** (por exemplo, `wolbachia_reference.fasta`).
2. **Criar uma base de dados indexada com BLAST**:
    

    
    - **dbtype nucl** indica que a base de dados é composta de sequências de nucleotídeos.
3. **Preparar um arquivo multifasta** com sequências pequenas que deseja alinhar localmente. Esse arquivo será o `queries.fasta`. (explicar como eu preparei)
4. **Executar o BLAST para alinhamento local** entre o arquivo `queries.fasta` e a base de dados `wolbachia_db`.
    
    ```bash
    blastn -query queries.fasta -db wolbachia_db -out local_alignment_results.txt -outfmt 6
    ```
    
    - **outfmt 6** produz uma saída tabular com os resultados.
    
    ### Análise das Linhas de Alinhamento
    
    | Coluna | Descrição |
    | --- | --- |
    | **1. Query ID** | Identificador da sequência de consulta (query) no arquivo `queries.fasta`. Esse campo ajuda a identificar qual sequência foi alinhada contra a base de dados. |
    | **2. Subject ID** | Identificador da sequência da base de dados (subject) com a qual a query foi alinhada. No nosso caso, esse identificador refere-se a uma sequência em `wolbachia_db`. |
    | **3. % Identity** | Percentual de identidade entre a query e o subject ao longo do alinhamento. Esse valor indica a similaridade e ajuda a avaliar a proximidade entre as sequências. |
    | **4. Alignment Length** | Comprimento total do alinhamento (em pares de bases), considerando as posições em que a query e o subject estão alinhados. Um alinhamento mais longo pode indicar uma correspondência mais abrangente entre as sequências. |
    | **5. Mismatches** | Número de nucleotídeos que não correspondem entre a query e o subject ao longo do alinhamento. Esse valor é útil para entender onde ocorrem diferenças específicas entre as duas sequências. |
    | **6. Gap Openings** | Número de vezes que gaps foram introduzidos no alinhamento. Um alto número de gaps pode sugerir inserções ou deleções significativas entre a query e o subject, indicando diferenças evolutivas ou estruturais. |
    
    | Coluna | Descrição |
    | --- | --- |
    | **7. Query Start** | Posição inicial do alinhamento na sequência de consulta. Esse valor mostra onde o alinhamento começa na sequência query, ajudando a localizar a região específica de similaridade. |
    | **8. Query End** | Posição final do alinhamento na sequência de consulta. Juntamente com `Query Start`, esse campo define a extensão do alinhamento na query. |
    | **9. Subject Start** | Posição inicial do alinhamento na sequência da base de dados. Esse valor indica onde o alinhamento começa na sequência subject, permitindo identificar a região de similaridade na sequência de referência. |
    | **10. Subject End** | Posição final do alinhamento na sequência da base de dados. Juntamente com `Subject Start`, este campo define a extensão do alinhamento na sequência da base de dados. |
    | **11. E-value** | Valor expectado (E-value) do alinhamento, que representa a probabilidade de um alinhamento similar ocorrer ao acaso. Valores menores indicam alinhamentos mais significativos. |
    | **12. Bit Score** | Pontuação em bits que reflete a qualidade do alinhamento. Alinhamentos com pontuações mais altas são considerados mais confiáveis e biologicamente significativos. |
    
    ### 1. Primeiro Alinhamento
    
    ```
    gbkey=CDS;gene=rpsG;go_component=small  NZ_CP072672.1   100.000 480     0       0       1       480     13530   14009   0.0887
    
    ```
    
    - **Query ID**: `gbkey=CDS;gene=rpsG;go_component=small` — a sequência de consulta representa o gene **rpsG** que está associado à subunidade ribossômica pequena (small ribosomal subunit).
    - **Subject ID**: `NZ_CP072672.1` — sequência alvo, identificando uma região específica no genoma.
    - **% Identity**: 100.0% — indica que todas as posições do alinhamento são idênticas entre a query e o subject, sugerindo uma correspondência perfeita.
    - **Alignment Length**: 480 nucleotídeos — um alinhamento relativamente longo, indicando uma alta similaridade na estrutura do gene.
    - **Mismatches e Gaps**: 0 — não há divergências ou gaps, o que reforça a correspondência perfeita.
    - **Query Start/End e Subject Start/End**: O alinhamento cobre toda a extensão do gene `rpsG` na sequência query.
    - **E-value**: 0.0887 — embora próximo de zero, um valor de E ligeiramente acima de zero indica que o alinhamento pode ocorrer ao acaso em uma frequência muito baixa. No entanto, como está próximo de zero, é biologicamente relevante.
    
    ### Interpretação:
    
    Esse alinhamento sugere que o gene `rpsG` está presente de forma idêntica na sequência `NZ_CP072672.1`, sem qualquer mutação ou variação, indicando uma possível conservação do gene no genoma.
    
    ---
    
    ### 2. Segundo Alinhamento
    
    ```
    NZ_CP072672.1:698661-699857     NZ_CP072672.1   100.000 1197    0       0       1       1197    698661  699857  0.0     2211
    
    ```
    
    - **Query ID**: `NZ_CP072672.1:698661-699857` — uma região específica na sequência de consulta (FtsZ).
    - **% Identity**: 100.0% — também uma correspondência perfeita.
    - **Alignment Length**: 1197 nucleotídeos — um alinhamento muito longo, sugerindo uma grande região de similaridade.
    - **Mismatches e Gaps**: 0 — a ausência de diferenças indica uma correspondência exata.
    - **E-value**: 0.0 — o valor zero indica que é altamente improvável que o alinhamento seja devido ao acaso, o que reforça a relevância biológica desse alinhamento.
    - **Bit Score**: 2211 — uma pontuação em bits extremamente alta, que reforça a importância e a confiabilidade desse alinhamento.
    
    ---
    
    ### 3. Terceiro Alinhamento
    
    ```
    Sequencia       NZ_CP072672.1   88.000  75      0       4       1       73      1       68      4.00e-17        80.5
    
    ```
    
    - **Query ID**: `Sequencia` — representa uma sequência query genérica.
    - **% Identity**: 88.0% — um alinhamento com uma similaridade de 88%, indicando algumas diferenças entre a query e o subject.
    - **Alignment Length**: 75 nucleotídeos — um alinhamento mais curto em comparação com os anteriores.
    - **Mismatches e Gaps**: 0 mismatches e 4 gaps — o alinhamento inclui 4 gaps, sugerindo algumas inserções ou deleções.
    - **E-value**: 4.00e-17 — um valor extremamente baixo, o que indica uma alta significância, mesmo com uma identidade menor que 100%.
    - **Bit Score**: 80.5 — uma pontuação em bits considerável, embora menor do que as outras linhas, devido ao comprimento mais curto e menor identidade.
    
    ### Interpretação:
    
    Esse alinhamento sugere uma correspondência significativa entre a query e o subject, apesar da menor identidade e da presença de gaps. A presença de gaps indica algumas mutações estruturais (inserções ou deleções), o que sugere variações genéticas que podem estar associadas a adaptações ou diferenças evolutivas.
    
    ### Conclusão Geral
    
    - As duas primeiras linhas mostram correspondências perfeitas, indicando regiões altamente conservadas e possivelmente essenciais para a função da bactéria **Wolbachia**.
    - A terceira linha revela uma correspondência com mutações, indicando uma possível variação genética que pode estar associada a adaptações específicas ou diferenças entre subpopulações de **Wolbachia**.

### Faça Você Mesmo (Exercício)

Voce pode usar o comando `blastn -help` para olhar a documentação ou usar uma simplificação que estou enviando a baixo:

1. **`query`**: Especifica o arquivo contendo as sequências que queremos alinhar contra a base de dados. É a entrada principal do BLAST e define a sequência "query" para o alinhamento.
2. **`db`**: Define a base de dados de referência para o alinhamento. Esse arquivo contém as sequências que o BLAST irá comparar com a query.
3. **`evalue`**: Define o valor esperado máximo (E-value) para reportar resultados. O E-value mede a probabilidade de um alinhamento ocorrer ao acaso; um E-value menor (por exemplo, 0.001) indica um alinhamento mais significativo.
4. **`outfmt`**: Especifica o formato da saída. O formato 6 gera uma tabela de fácil leitura, ideal para processamento automatizado e análises. Outros formatos incluem o 0 (formato padrão) e o 5 (formato XML).
5. **`perc_identity`**: Define o percentual mínimo de identidade para um alinhamento ser reportado. Com isso, é possível filtrar alinhamentos que não atingem um nível mínimo de similaridade.
6. **`qcov_hsp_perc`**: Percentual mínimo de cobertura da query em um único alinhamento. Esse parâmetro ajuda a remover alinhamentos que cobrem apenas uma pequena porção da query, forçando a obtenção de alinhamentos mais completos.
7. **`max_target_seqs`**: Define o número máximo de resultados que o BLAST deve reportar por sequência de query. Isso é útil para limitar a saída e focar nos resultados mais relevantes.
8. **`gapopen` e `gapextend`**: Configuram as penalidades para abertura e extensão de gaps, respectivamente. Alterar esses valores influencia o número de gaps no alinhamento e o resultado final, especialmente em alinhamentos onde há inserções ou deleções.

**Objetivo**: Filtrar os resultados do BLAST para identificar apenas as sequências com alta identidade e alta cobertura, removendo as que possuem menor correspondência (como a sequência com identidade de 88% no exemplo anterior).

### **Atividade de Filtro com E-value ou Percentual de Identidade**

- **Tarefa**: Refaça o alinhamento aplicando um cutoff no E-value e no percentual de identidade para excluir alinhamentos menos significativos.

**Filtro com Cobertura da Query**

- **Tarefa**: Refaça o comando anterior, mas agora adicionando o parâmetro `qcov_hsp_perc` para manter apenas os alinhamentos que cobrem uma porcentagem da query, mantendo apenas os 2 alinhamentos com maior correspondência.

**Explorando a Limitação de Resultados com `-max_target_seqs`**

- **Tarefa**: Execute o BLAST com um limite de resultados, por exemplo, mantendo apenas os dois melhores alinhamentos por sequência query.

Boa Tarefa!