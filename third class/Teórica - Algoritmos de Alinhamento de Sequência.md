# Teórica - Algoritmos de Alinhamento de Sequências

Alinhamento Global

![image.png](image.png)

- Alinha todos os nucleotídeos da sequencia query/target.
- Apropriado para sequencias geneticamente similares/relacionadas.

Exemplos:

- Analise evolutiva de uma sequencia de nucleotídeo
- Analise de genomas de seres da mesma espécie para identificar genes/regiões de interesse
- Identificação de variantes de sequência em genes ortólogos

Alinhamento Local

![image.png](image%201.png)

- Alinha alguns nucleotídeos da sequencia query/target
- Alinha regiões de maior similaridade.
- Mais apropriado para sequencias divergentes ou alinhamento de pedaços menores (query).

Exemplos:

- Caracterização de microbioma (16S contra dados de referencia).
- O nosso projeto de wolbachia usando pequenos RNAs.
- Identificação de fragmentos de DNA em amostras ambientais

### Algoritmo Needleman-Wunch (Alinhamento Global de Sequências)

- Input → Algoritmo → Output
- Algoritmos Exatos X Algoritmos Heurísticos.
- Desenvolvido por Saul B. Needleman e Christian D. Wunsch em 1970, é um método de programação dinâmica usado para alinhamento global de sequências.
- Quebra o problema de alinhamento em subproblemas menores, construindo uma matriz onde cada célula representa um par de nucleotídeos (ou aminoácidos) a ser alinhado (programação dinâmica).
- Passos: Inicialização → Preenchimento da Matriz → Backtracking
1. **Construção da Matriz**: Cada célula na matriz representa um par de posições entre as duas sequências que estamos alinhando. A pontuação em cada célula reflete a "melhor pontuação possível" para alinhar as subsequências até aquele ponto.
2. **Cálculo de Pontuações**:
    - **Match** (quando os nucleotídeos/aminoácidos são iguais) recebe uma pontuação positiva.
    - **Mismatch** (diferença entre nucleotídeos) recebe uma pontuação que depende da **matriz de substituição**.
    - **Gap** recebe uma penalidade, que varia conforme os parâmetros `gapopen` e `gapextend`.
3. **Backtracking**: Após preencher a matriz, o algoritmo traça o caminho com a maior pontuação desde a célula final até o início, identificando a melhor forma de alinhar as sequências de ponta a ponta.

### Montando a Matriz de Fato

Para preencher a matriz, escolha a maior pontuação entre:

- Valor da célula a Esquerda (todo movimento pra direita [-2] gap)
- Valor da célula a Cima (todo movimento para baixo [-2] gap)
- Valor da célula Diagonal (movimento diagonal depende do mismatch[-1]/match[+1])

match = +1 / mismatch = -1 / gap = -2

[Resposta](https://www.notion.so/Resposta-12f5e115016d80dca482ca97f68f3f33?pvs=21)

|  |  | A | T | G | C | T |
| --- | --- | --- | --- | --- | --- | --- |
|  | 0 | -2 | -4 | -6 | -8 | -10 |
| A | -2 | -4/-4/+1 | -1/-6/-3 |  |  |  |
| G | -4 | -6/-1/-3 | -3/-3/0 |  |  |  |
| C | -6 |  |  |  |  |  |
| T | -8 |  |  |  |  |  |

![image.png](image%202.png)

### **Matriz de Pontuação para Nucleotídeos**

No DNA, as bases são pareadas por pontes de hidrogênio em arranjos específicos:

- **Adenina (A)** se liga a **Timina (T)** com 2 pontes de hidrogênio.
- **Citosina (C)** se liga a **Guanina (G)** com 3 pontes de hidrogênio.

Substituições que mantêm o mesmo número de pontes de hidrogênio são chamadas de transições (ex: A↔G ou C↔T), enquanto substituições que alteram o número de pontes são chamadas de transversões (ex: A↔C, A↔T, C↔G, G↔T).

### **Matriz de Pontuação (Penalidade) - Sem Transição**

|  | A | G | C | T |
| --- | --- | --- | --- | --- |
| A | 1 | −1 | −1 | −1 |
| G | −1 | 1 | −1 | −1 |
| C | −1 | −1 | 1 | −1 |
| T | −1 | −1 | −1 | 1 |

|  | A | T | C | G |
| --- | --- | --- | --- | --- |
| A | 10 | -1 | -3 | -4 |
| T | -1 | 7 | -5 | -3 |
| C | -3 | -5 | 9 | 0 |
| G | -4 | -3 | 0 | 8 |
- **+2** é a pontuação para uma correspondência exata.
- **0** é a penalidade reduzida para transições (A↔G, C↔T), refletindo a mudança menos drástica em termos de estrutura de pontes de hidrogênio.
- **1** é a penalidade para transversões que envolvem mudança de uma base de dupla ponte de hidrogênio para uma base de tripla ponte (T↔G, A↔C).
- **2** é a penalidade mais severa para transversões que representam uma mudança mais drástica na configuração das pontes de hidrogênio (A↔T, C↔G).
- O fluxo informacional biológico é capturado em modelos computacionais/matemáticos para fornecer um modelo probabilístico da realidade (never take a model to serious - Sapolsky).

### Aplicando o Princípio da Navalha de Occam em Algoritmos

- **Definição:** Princípio da parcimônia ou simplicidade.
- **Hipótese 1:** Um homem com traje vermelho e barba branca, conhecido como Papai Noel, decola do Polo Norte em um trenó voador puxado por renas. Ele voa globalmente, entra na sua casa pela chaminé (que sua casa não tem), e deixa presentes apenas por você ser uma criança bem-comportada, tudo em uma única noite.
- **Hipótese 2:** Seu pai, acorda cedo e compra uma pista de Hot Wheels na loja, embrulha-a em papel colorido e a coloca sob a falso pinheiro de plástico antes de você acordar.
- **Aplicação da Navalha de Occam:** A hipótese 2 é preferida por ser a mais simples.

**Aplicação em Algoritmos - O Caso do Algoritmo de Needleman-Wunsch**

- **Hipótese 1:** Remoção completa e posterior inserção de todos os nucleotídeos.
- **Hipótese 2:** Caminho de menor custo na matriz, minimizando alterações.
    - **Aplicação da Navalha de Occam:** A hipótese 2 é escolhida por ser mais eficiente e parcimoniosa.
- Após a matriz ser preenchida, o algoritmo realiza o **backtracing** (ou caminho de volta) para encontrar a trajetória com a maior pontuação total. Esse caminho representa a **sequência de mutações mais simples e direta** que explica a relação entre as duas sequências.
- Na prática, o backtracing envolve **escolher o caminho que maximiza similaridades e minimiza gaps**, refletindo as mutações mais prováveis e com menos passos desnecessários.

### **Algoritmo Smith–Waterman (Alinhamento Local de Sequências)**

Começar do zero permite que o algoritmo "ignore" partes das sequências que não contribuem para um alinhamento significativo. Isso significa que o algoritmo não é penalizado por regiões de baixa similaridade ao início ou ao final das sequências, focando-se apenas nas partes que realmente mostram correspondências significativas. Dessa forma, cada parte da matriz de pontuação pode desenvolver um alinhamento local independentemente, sem ser influenciado por pontuações negativas ou baixas de outras regiões.

![image.png](image%203.png)

![image.png](image%204.png)

![image.png](image%205.png)