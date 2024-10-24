# Aula de Sequenciamento e DataBases

### Retomando Aula Passada

- Estrutura do DNA.
- Pareamento de bases de nucleotídeo pode ser representado computacionalmente como uma string apartir de nucleotideos A-T / C=G.
- Devido a complementaridade do pareamento, a informação de uma fita única é suficiente para compor fita dupla (DNA polimerase).
- Conceito de similaridade de sequencia e alinhamento de nucleotídeos por pareamento.

### Objetivos da aula

- Entender o processo de sequenciamento de nucleotídeos.
- Compreender os scores de qualidade, tanto de forma teórica quanto no dataset que vamos trabalhar (arquivo FASTQ).
- Conhecer e aprender como usar bases de dados na bioinformática (Browser e Terminal Linux)
    - Browser do NCBI
    - Eutilities API (linha de comando)
- Instalar e criar enviroments com micromamba para rodar baixar bibliotecas publicas com o SRAtoolkit

### Links externos

- [https://youtu.be/goXvL9fW-1Q?si=DsGzg_2VdxYWYEiv](https://youtu.be/goXvL9fW-1Q?si=DsGzg_2VdxYWYEiv) / [https://youtu.be/Poh1gzcR2tQ?si=_HiY0CEUeY8hqOmy](https://youtu.be/Poh1gzcR2tQ?si=_HiY0CEUeY8hqOmy) (SRAtoolkit)
- [https://youtu.be/Lvi48eEQdHE?si=xewLIdjesZCfIJNK](https://youtu.be/Lvi48eEQdHE?si=xewLIdjesZCfIJNK) (NCBI browser)
- [A Search Example in Five Steps - Finding Sequence Using NCBI Nucleotide - Welch Medical Library Guides at Johns Hopkins University-Welch Medical Library (jhmi.edu)](https://browse.welch.jhmi.edu/finding-sequence-using-nucleotide/search-example) (como operar buscas no NCBI)

### Introdução: Sequenciamento de Nucleotídeos

**Sequenciamento de Sanger**

- **Princípio Básico:**
    - Utiliza a incorporação de nucleotídeos modificados, os **ddNTPs (didesoxinucleotídeos)**, que causam a terminação da síntese de DNA.
- **Etapas:**
    1. **Preparação da Amostra:**
        - DNA de fita simples é utilizado como molde.
    2. **Adição de Reagentes:**
        - DNA polimerase, primer, dNTPs normais e ddNTPs marcados com fluorescência.
    3. **Extensão da Cadeia:**
        - A DNA polimerase adiciona nucleotídeos à cadeia crescente até incorporar um ddNTP.
    4. **Terminação da Síntese:**
        - A incorporação de um **ddNTP** interrompe a extensão da cadeia devido à ausência do grupo 3'-OH.
    5. **Separação por Eletroforese:**
        - Fragmentos resultantes são separados por tamanho em um sequenciador capilar.

![image.png](image.png)

![image.png](image%201.png)

![Screenshot_20241016-063831_YouTube.jpg](Screenshot_20241016-063831_YouTube.jpg)

![image.png](image%202.png)

### Next Generation Sequence (NGS) - Segunda Geração

---

### **Comparação com um Quebra-Cabeça**

- **Quebra-Cabeça com Peças Grandes:**
    - **Facilidade de Montagem:**
        - Menor número de peças para encaixar.
        - Peças maiores têm características únicas mais facilmente identificáveis.
- **Quebra-Cabeça com Peças Pequenas:**
    - **Aumento da Complexidade:**
        - Maior número de peças semelhantes.
        - Dificuldade em distinguir detalhes entre peças.
    - **Tempo e Esforço:**
        - Requer mais tempo para encontrar as correspondências corretas.

### **Montagem de Genomas**

- **Fragmentos Grandes (Leituras Longas):**
    - **Vantagens:**
        - **Menor Número de Fragmentos:**
            - Facilita o alinhamento e a montagem do genoma.
        - **Cobertura de Regiões Repetitivas:**
            - Fragmentos longos podem atravessar sequências repetitivas, reduzindo ambiguidades.
        - **Melhor Contiguidade:**
            - Gera montagens mais contínuas e completas.
    - **Tecnologias Associadas:**
        - **Sequenciamento de Terceira Geração:**
            - Plataformas como PacBio e Oxford Nanopore.
- **Fragmentos Pequenos (Leituras Curtas):**
    - **Desafios:**
        - **Maior Complexidade de Montagem:**
            - Necessidade de alinhar um número muito maior de fragmentos.
            - Dificuldade em resolver regiões repetitivas ou complexas.
        - **Ambiguidade:**
            - Fragmentos curtos podem se encaixar em múltiplas posições.
    - **Tecnologias Associadas:**
        - **Sequenciamento de Segunda Geração:**
            - Plataformas como Illumina.

---

**Importância da Queda de Custos com o Sequenciamento de Segunda Geração**

---

### **Revolução no Acesso ao Sequenciamento**

- **Redução de Custos:**
    - O custo do sequenciamento genômico diminuiu drasticamente.
    - Permitiu que laboratórios menores realizassem projetos genômicos.
- **Aumento da Velocidade:**
    - Sequenciamento mais rápido em comparação com métodos tradicionais.
    - Possibilidade de sequenciar múltiplas amostras simultaneamente.

---

**Importância do Projeto do Genoma Humano e do Projeto Brasileiro da Xylella fastidiosa**

---

### **Projeto do Genoma Humano (HGP)**

- **Marco Histórico:**
    - Primeiro esforço internacional para sequenciar o genoma humano completo.
    - Iniciado em 1990 e concluído em 2003.
- **Contribuições Científicas:**
    - Mapeamento de todos os genes humanos.
    - Base para compreender doenças genéticas e desenvolvimento de tratamentos.
- **Avanços Tecnológicos:**
    - Desenvolvimento de novas técnicas de sequenciamento e bioinformática.
    - Redução significativa dos custos de sequenciamento.

### **Projeto Brasileiro da Xylella fastidiosa**

- Xylella fastidiosa é a bactéria causadora da praga do amarelinho nos laranjais.
- Impacto direto na agricultura brasileira, especialmente na citricultura.
- **Desenvolvimento Científico Nacional:**
    - Capacitação de pesquisadores brasileiros em genômica e bioinformática.
    - Estabelecimento de infraestruturas de pesquisa de ponta no país.
- **Impacto Científico e Tecnológico:**
    - Ambos impulsionaram o desenvolvimento de novas tecnologias e metodologias.
    - Estabeleceram bases para futuros projetos genômicos.

[Exercicio da Aula](https://www.notion.so/Exercicio-da-Aula-1205e115016d808cbcb1e7a9d35aef6c?pvs=21)

### O Que É um Ambiente Virtual?

Um **ambiente virtual** é um espaço isolado no sistema onde você pode instalar pacotes e bibliotecas sem afetar o ambiente global ou outros projetos. Cada ambiente virtual tem seu próprio diretório de instalação para pacotes, permitindo que diferentes projetos usem versões diferentes das mesmas bibliotecas sem conflitos.

### Por Que Usar Ambientes Virtuais?

1. **Isolamento**: Evita conflitos entre projetos que dependem de diferentes versões de pacotes.
2. **Reprodutibilidade**: Facilita a reprodução do ambiente de trabalho em outras máquinas ou por outros pesquisadores.
3. **Organização**: Mantém o ambiente global do sistema limpo e organizado.

### **Exercício** - Criação de ambientes

- **Conteúdo:**
    - Criação de ambientes virtuais para garantir a reproducibilidade (usando chatgpt).
    - Instalação do SRA Toolkit em um enviroment no micromamba.

### Entendendo o Arquivo FASTQ e Scores de Qualidade

FASTQ (SRATOOLKIT)

O formato FASTQ é um dos formatos mais fundamentais e amplamente utilizados na bioinformática para armazenar sequências de nucleotídeos obtidas através de tecnologias de sequenciamento de alto desempenho, como Illumina, Ion Torrent, entre outras. Este formato combina tanto a informação da sequência de DNA ou RNA quanto a qualidade associada a cada base sequenciada, permitindo uma análise detalhada e precisa dos dados genômicos.

## Estrutura do Arquivo FASTQ

Um arquivo FASTQ consiste em múltiplos registros, cada um representando uma sequência individual e suas informações associadas. Cada registro é composto por **quatro linhas**:

1. **Linha 1**: Começa com o caractere `@` e é seguida por um identificador de sequência único, podendo conter descrições adicionais.
2. **Linha 2**: Contém a sequência de nucleotídeos correspondente (A, T, C, G e, ocasionalmente, N para bases desconhecidas).
3. **Linha 3**: Começa com o caractere `+` e pode repetir o identificador da sequência ou ficar em branco.
4. **Linha 4**: Apresenta a sequência de qualidades, codificadas em caracteres ASCII, correspondendo a cada base da sequência na Linha 2.

### Exemplo Prático

Vamos analisar o exemplo fornecido:

```
@SRR12893567.1 1 length=51
GCCCGGCTAGCTCAGTTAGATCGGAAGAGCACACGTCTGAACTCCAGTCAC
+SRR12893567.1 1 length=51
AAFFFJJJJJJFJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJ

```

**Linha 1** (`@SRR12893567.1 1 length=51`):

- **`@`**: Indica o início de um novo registro de sequência.
- **`SRR12893567.1`**: Identificador único da sequência, geralmente fornecido pela plataforma de sequenciamento ou pelo pesquisador.
- **`1 length=51`**: Informações adicionais, indicando que esta é a primeira leitura e que o comprimento da sequência é de 51 nucleotídeos.

**Linha 2** (`GCCCGGCTAGCTCAGTTAGATCGGAAGAGCACACGTCTGAACTCCAGTCAC`):

- Contém a sequência de nucleotídeos.

**Linha 3** (`+SRR12893567.1 1 length=51`):

- **`+`**: Sinaliza o fim da sequência e o início da linha de qualidades.
- O texto após o `+` é opcional e pode repetir o identificador ou ser omitido.

**Linha 4** (`AAFFFJJJJJJFJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJ`):

- Contém os caracteres que representam as qualidades Phred associadas a cada base na Linha 2.

## Qualidade Phred e Codificação ASCII

### O Que São Scores Phred?

Os scores Phred são valores numéricos que representam a qualidade de cada base sequenciada, indicando a probabilidade de um erro na identificação daquela base. Eles são calculados usando a seguinte fórmula:

$$
Q=−10 * log10 (P)
$$

Onde:

- Q é o score Phred.
- P é a probabilidade de erro na chamada da base.

**Exemplo**: Se a probabilidade de erro é de 1 em 1000 (ou 0,1%), o score Phred é:

$$
Q= −10 * log10 (1/1000) = -10 * -3 = 30
$$

### Interpretando os Scores Phred

- **Score Phred Alto**: Indica alta confiança na leitura (baixa probabilidade de erro).
- **Score Phred Baixo**: Indica baixa confiança na leitura (alta probabilidade de erro).

| Qualidade Phred | Probabilidade de Erro | Acurácia |
| --- | --- | --- |
| 10 | 1 em 10 | 90% |
| 20 | 1 em 100 | 99% |
| 30 | 1 em 1.000 | 99,9% |
| 40 | 1 em 10.000 | 99,99% |
| 50 | 1 em 100.000 | 99,999% |
| 60 | 1 em 1.000.000 | 99,9999% |

### Por Que os Scores Phred São Importantes?

- **Qualidade dos Dados**: Permitem avaliar a qualidade geral do sequenciamento.
- **Análises Confiáveis**: Bases com alta qualidade levam a resultados mais confiáveis em análises downstream, como alinhamento e identificação de variantes.
- **Filtragem de Dados**: Bases ou sequências inteiras com scores baixos podem ser filtradas para melhorar a qualidade dos dados.

### Codificação em ASCII

Os scores Phred são codificados em caracteres ASCII para serem armazenados de forma compacta. Cada caractere na Linha 4 corresponde a um score Phred para a base na posição equivalente na Linha 2.

Existem diferentes escalas de codificação, mas a mais comum atualmente é a **Sanger Phred+33**, onde:

- **Caractere ASCII**: Representa um valor numérico baseado na tabela ASCII.
- **Conversão**: Q = ord(c) - 33, onde ord(c) é o valor ASCII do caractere.

### Como Funciona a Codificação?

1. **Offset de Codificação**: O score Phred é ajustado por um valor constante (geralmente 33) para se alinhar com caracteres ASCII imprimíveis.
2. **Conversão para Caractere ASCII**:
    
    $$
    CaractereASCII = Q + \text{Offset}
    $$
    
    Onde:
    
    - `Q` é o score Phred.
    - O `offset` é geralmente 33 (para a codificação Sanger Phred+33).
3. **Exemplo de Conversão:**
    
    Tomemos o caractere `F` na Linha 4.
    
    - Valor ASCII de `F` é 70.
    - Score Phred: 70 - 33 = 37
    
    Isto indica uma alta qualidade para aquela base específica.
    
    | Símbolo | Phred | Probabilidade de Erro |
    | --- | --- | --- |
    | ! | 0 | 1.000 |
    | " | 1 | 0.794 |
    | # | 2 | 0.631 |
    | … | … | … |
    | C | 34 | 0.0004 |
    | D | 35 | 0.0003 |
    | E | 36 | 0.0002 |
    | F | 37 | 0.0002 |
    | G | 38 | 0.0002 |
    | H | 39 | 0.0001 |
    | I | 40 | 0.0001 |

## Variações no Formato FASTQ

É importante notar que existem diferentes versões de codificação de qualidade, como:

- **Sanger Phred+33**: Valores de 0 a 93, usando caracteres ASCII 33 a 126.
- **Illumina 1.5 Phred+64**: Agora obsoleto, usava uma escala diferente.

Para determinar a codificação usada em um arquivo FASTQ, pode-se analisar os valores mínimos e máximos dos caracteres na Linha 4 e compará-los com as tabelas de codificação conhecidas.