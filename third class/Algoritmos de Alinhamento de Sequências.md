# Algoritmos de Alinhamento de Sequências

### **Algoritmo Needleman-Wunch (Alinhamento Global de Sequências)**

- Input → Algoritmo → Output
- Algoritmos Exatos X Algoritmos Heurísticos.
- Desenvolvido por Saul B. Needleman e Christian D. Wunsch em 1970, é um método de programação dinâmica usado para alinhamento global de sequências.
- Inicializa uma matriz de pontuação com penalidades para aberturas e extensões de gaps.
- Preenche a matriz com base na pontuação de correspondência ou desajuste entre os caracteres das sequências e nas penalidades de gap.
- Traça o caminho de maior pontuação de volta à origem da matriz para construir o alinhamento ótimo.

![Matriz Needleman-Wunch](/images/Needleman-Wunsch_pairwise_sequence_alignment.png)

### **Matriz de Pontuação para Nucleotídeos**

No DNA, as bases são pareadas por pontes de hidrogênio em arranjos específicos:

- **Adenina (A)** se liga a **Timina (T)** com 2 pontes de hidrogênio.
- **Citosina (C)** se liga a **Guanina (G)** com 3 pontes de hidrogênio.

Substituições que mantêm o mesmo número de pontes de hidrogênio são chamadas de transições (ex: A↔G ou C↔T), enquanto substituições que alteram o número de pontes são chamadas de transversões (ex: A↔C, A↔T, C↔G, G↔T).

### **Matriz de Pontuação (Penalidade)**

|  | A | T | C | G |
| --- | --- | --- | --- | --- |
| A | 2 | -1 | -2 | 0 |
| T | -1 | 2 | 0 | -2 |
| C | -2 | 0 | 2 | -1 |
| G | 0 | -2 | -1 | 2 |
- **+2** é a pontuação para uma correspondência exata.
- **0** é a penalidade reduzida para transições (A↔G, C↔T), refletindo a mudança menos drástica em termos de estrutura de pontes de hidrogênio.
- **1** é a penalidade para transversões que envolvem mudança de uma base de dupla ponte de hidrogênio para uma base de tripla ponte (T↔G, A↔C).
- **2** é a penalidade mais severa para transversões que representam uma mudança mais drástica na configuração das pontes de hidrogênio (A↔T, C↔G).
- O fluxo informacional biológico é capturado em modelos computacionais/matemáticos para fornecer um modelo probabilístico da realidade (never take a model to serious - Sapolsky).

### **Aplicando o Princípio da Navalha de Occam em Algoritmos**

- **Definição:** Princípio da parcimônia ou simplicidade.
- **Hipótese 1:** Um homem com traje vermelho e barba branca, conhecido como Papai Noel, decola do Polo Norte em um trenó voador puxado por renas. Ele voa globalmente, entra na sua casa pela chaminé (que sua casa não tem), e deixa presentes apenas por você ser uma criança bem-comportada, tudo em uma única noite.
- **Hipótese 2:** Seu pai, acorda cedo e compra uma pista de Hot Wheels na loja, embrulha-a em papel colorido e a coloca sob a falso pinheiro de plástico antes de você acordar.
- **Aplicação da Navalha de Occam:** A hipótese 2 é preferida por ser a mais simples.

### **Aplicação em Algoritmos - O Caso do Algoritmo de Needleman-Wunsch**

- **Hipótese 1:** Remoção completa e posterior inserção de todos os nucleotídeos.
- **Hipótese 2:** Caminho de menor custo na matriz, minimizando alterações.
- **Aplicação da Navalha de Occam:** A hipótese 2 é escolhida por ser mais eficiente e parcimoniosa.

### **Algoritmo Smith–Waterman (Alinhamento Local de Sequências)**

Começar do zero permite que o algoritmo "ignore" partes das sequências que não contribuem para um alinhamento significativo. Isso significa que o algoritmo não é penalizado por regiões de baixa similaridade ao início ou ao final das sequências, focando-se apenas nas partes que realmente mostram correspondências significativas. Dessa forma, cada parte da matriz de pontuação pode desenvolver um alinhamento local independentemente, sem ser influenciado por pontuações negativas ou baixas de outras regiões.

### **Passos Algoritmo Smith–Waterman**  
![Passo 1](/images/Smith-Waterman-Algorithm-Example-Step1.png)
![Passo 2](/images/Smith-Waterman-Algorithm-Example-Step2.png)
![Passo 3](/images/Smith-Waterman-Algorithm-Example-Step3.png)