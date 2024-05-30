# Aula 4 - Ciencia de dados

A estatística é a ciência que se ocupa da coleta, análise, interpretação e apresentação de dados.

### **Conceito de Probabilidade na Filosofia Jaina (433 - 357 a.C.)**

- Na filosofia Indiana, especificamente nos escritos de Bhadrabahu, já há uma compreensão inicial da probabilidade, discutindo a incerteza e as diferentes possibilidades em eventos.
- Introdução ao conceito de "syadvada" ou 'a asserção de possibilidades' (syat = 'pode ser', vada = 'afirmação/asserção').
- Discussão de exemplos como o lançamento de uma moeda (analogias iniciais ao conceito de probabilidade) e o estudo da fisiologia dos órgãos dos sentidos.

### [**Lady Tasting Tea](https://web.ma.utexas.edu/users/mcudina/m358k-lady-tasting-tea-original.pdf) - Fischer**

1. **Introdução ao Experimento**
    - A senhora afirma distinguir se o leite ou o chá foi colocado primeiro durante o preparo.
    - Como testar a alegação de maneira estatisticamente rigorosa.
2. **Configuração do Experimento**
    - Preparação: 8 xícaras de chá, 4 com leite adicionado primeiro e 4 com chá adicionado primeiro, distribuído em ordem aleatoria.
    - Tarefa: A senhora deve identificar corretamente a ordem de adição em todas as xícaras.
3. **Cálculo das Frequências de Sucesso**
    - 0 Sucessos:
        - Apenas 1 maneira: escolher todas as 4 xícaras incorretas.
    - 1 Sucesso e 3 Falhas:
        - Combinação para escolher 1 xícara correta:  binomial{4}{1} = 4
        - Combinação para escolher 3 xícaras incorretas: binomial{4}{3} = 4
        - Total de maneiras: 4 × 4 = 16

```latex
# Regra Geral Binomial
\binom{n}{k} = \frac{n!}{k!(n-k)!}
# Lady Testing Tea
\binom{8}{4} = \frac{8!}{4!(8-4)!} = 70
# Combinacao de 1 acerto 3 erros
\binom{4}{1} = \frac{4!}{1!(4-1)!} = 4x3x2x1/3x2x1 = 4
\binom{4}{3} = \frac{4!}{3!(4-3)!} = 4x3x2x1/3x2x1 = 4
**4x4=16**
```

| Success count | Combinations of selection | Number of Combinations |
| --- | --- | --- |
| 0 | oooo | 1 × 1 = 1 |
| 1 | ooox, ooxo, oxoo, xooo | 4 × 4 = 16 |
| 2 | ooxx, oxox, oxxo, xoxo, xxoo, xoox | 6 × 6 = 36 |
| 3 | oxxx, xoxx, xxox, xxxo | 4 × 4 = 16 |
| 4 | xxxx | 1 × 1 = 1 |
1. **Região Crítica para Rejeição da Hipótese Nula**
    - A senhora tem habilidade para distinguir a ordem, uma vez que acertou todas.
    - Critério de Rejeição:
        - Região Crítica: Caso único de 4 sucessos em 4 tentativas.
        - Justificativa: Sob a hipótese nula, 4 sucessos têm 1 chance em 70 (\(\approx 1.4\% < 5\%\)) de ocorrer.
        - Probabilidade de Pelo Menos 3 Sucessos: (16+1)/70 (approx 24.3% > 5%).

### Galton Board ([conceitos basicos de estátistica](https://medium.com/codex/20-statistical-concepts-every-data-scientist-analyst-should-know-2d28a06a5483))

1. **População vs. Amostra (realidade x modelo)**:
    - **População**: Conjunto completo de todos os elementos de interesse.
    - **Amostra**: Subconjunto selecionado da população para análise.
2. **Distribuição Normal**:
    - Uma das distribuições mais comuns que descreve como os dados estão distribuídos de forma simétrica ao redor da média, formando uma curva de sino.
3. **Estatística Descritiva**:
    - Envolve resumir dados usando:
        - **Medidas de Tendência Central**: Média, mediana e modo.
        - **Medidas de Dispersão**: Variância, desvio padrão, amplitude.
4. **P Valor e Falso Positivo**:
    - **P Valor**: Indica a probabilidade de obter um resultado tão extremo quanto o observado, sob a hipótese nula.
    - **Falso Positivo (Erro Tipo I)**: Ocorre quando a hipótese nula é falsamente rejeitada.
5. **Lei dos Grandes Números**:
    - Afirma que conforme o tamanho da amostra aumenta, a média das amostras se aproxima da média da população.
6. **Probabilidade**:
    - Mede a frequência com que se espera que um evento ocorra, dado um número infinito de repetições da experiência.

![galton_board](/images/Galton_board.png))

colocar o Rshiny do galton board aqui

### Apresentacao de dados

complex heatmap / Rshiny