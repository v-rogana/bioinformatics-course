# Alinhamento - Parte 2

### Guia Passo a Passo para Alinhamento de Sequências de DNA com Bowtie e Manipulação com Samtools

Este guia foca no uso do Bowtie para alinhar sequências de DNA a uma referência, utilizando ferramentas e técnicas específicas. A atividade está dividida em seis partes principais:

1. **Configuração do Ambiente com Micromamba**
2. **Construção do Índice Bowtie (indexar database igual no blast)**
3. **Alinhamento com Bowtie (alinhamento local)**
4. **Manipulação e Limpeza com Samtools**
5. **Comando para Montar uma Tabela de Contagem de Alinhamento**

---

### **1. Configuração do Ambiente com Micromamba**

Vamos começar configurando um ambiente isolado para garantir que todas as dependências necessárias estejam instaladas e não conflitem com outras instalações no sistema.

**Passos:**

1. **Ativar um Ambiente com Micromamba**
    
    ```bash
    # Ative o ambiente
    micromamba activate alinhamento_parte2
    ```
    
2. **Instalar Pacotes Necessários (não façam isso em aula**

Isso serve apenas para caso vcs um dia queiram fazer em casa :)

```bash
# Instale o Bowtie e o Samtools
micromamba install bowtie samtools
```

---

### **2. Construção do Índice Bowtie**

Antes de alinhar as sequências, precisamos construir um índice da sequência de referência. Isso permite que o Bowtie faça o alinhamento de forma eficiente.

**Passos:**

1. **Obter as Sequências de Referência e as Bibliotecas de pequenos RNAs (query)
* Faça isso dentro do seu diretorio!**
    
    ```bash
    cp /home/topicos/aula_alinhamento_parte2 .
    ```
    
2. **Construir o Índice**
    
    ```bash
    bowtie-build reference.fasta reference_index
    ```
    
    - `reference.fasta` é o arquivo de sequência de referência composto pela nossa cepa de wolbachia (especificamente a usada para soltura de mosquitos infectados aqui do lado da UFMG).
    - `reference_index` é o prefixo para os arquivos de índice que serão gerados.

---

### **3. Alinhamento com Bowtie**

Você tem um arquivo de sequências de leitura curtas chamado `(un)infected_SRR*.fasta`. A sequência de referência é um arquivo multifasta chamado `reference_multi.fasta`, que contém sequências de diferentes cepas de um organismo. Seu objetivo é alinhar as leituras às sequências de referência para identificar quais cepas estão presentes nas suas amostras.

Agora, vamos alinhar as sequências das bibliotecas FASTA à sequência de referência usando o Bowtie.

**Passos:**

1. Familiarize-se com os seguintes parâmetros:
- **`f`**: Indica que o arquivo de entrada está no formato FASTA.
- **`S`**: Especifica que a saída será no formato SAM.
- **`a`**: Reporta todos os alinhamentos que satisfazem os critérios.
- **`k <int>`**: Limita o número de alinhamentos reportados por leitura (por exemplo, `k 1` reporta apenas o melhor).
- **`v <int>`**: Permite até um certo número de mismatches (diferenças) no alinhamento.
- **`p <int>`**: Define o número de threads para processamento paralelo.
- **`y`**: Desativa o comportamento padrão de evitar regiões repetitivas na referência.
- `reference_index`: Prefixo dos arquivos de índice criados anteriormente.
- `library1.fasta`: Arquivo FASTA com as sequências de leitura.
- `> library1.sam`: Redireciona a saída para um arquivo SAM.
    
    Certifique-se de que suas sequências de leitura estão em arquivos FASTA. Por exemplo, `library1.fasta`, `library2.fasta`, etc.
    
1. **Executar o Alinhamento**
    
    Utilizaremos o Bowtie com parâmetros específicos para otimizar o alinhamento. 
    O -p deve ser 3!
    
    ```bash
    bowtie -p 3 [seus parâmetros] reference_index infected_SRR12893566.fasta > infected_SRR12893566.sam
    ```
    
    **Determinar a Parametrização Ideal**
    
    **Pergunta:** Qual combinação de parâmetros você considera ideal para identificar as cepas presentes nas suas amostras? Para esse problema, pense nos seguintes pontos:
    
    - Sensibilidade vs. especificidade.
    - Presença de múltiplas cepas na referência.
    - Tamanho das sequências de leitura.
    
    *Não tenha medo de testar e errar!
    

---

### **4. Manipulação e Limpeza com Samtools**

Após o alinhamento, usaremos o Samtools para filtrar e preparar os dados para análises posteriores.

**Filtrar Somente as Leituras Alinhadas**

```bash
samtools view -S -h -F 4 infected_SRR12893566.sam > infected_SRR12893566_onlymapped.sam
```

**Explicação dos Parâmetros:**

- `S`: Indica que a entrada está no formato SAM.
- `h`: Inclui o cabeçalho na saída.
- `F 4`: Exclui as leituras não alinhadas (flag 4 indica leituras não mapeadas; `F` exclui essas leituras). [Para entender isso, analise o arquivo SAM. Essa informação esta no final do guia]
- `infected_SRR12893566.sam`: Arquivo de entrada com todas as leituras.
- `> infected_SRR12893566_onlymapped.sam`: Redireciona a saída filtrada para um novo arquivo SAM.

---

### **5. Montagem da Tabela de Alinhamento**

```bash
awk '$1 !~ /^@/ {print $3}' infected_SRR12893566_onlymapped.sam | sort | uniq -c
```

**Explicação do Script:**

- `awk '$1 !~ /^@/ {print $3}' "$sam_file"`: Extrai a referência (terceira coluna) das linhas de alinhamento.
- `sort | uniq -c`: Conta as ocorrências de cada referência.

---

### Desafio (não faremos em aula mas sinta-se a vontade para realizar)

Agora, vamos criar um script que automatiza as etapas de alinhamento, filtragem e contagem para todas as bibliotecas.

**Passos:**

1. **Criar o Arquivo de Script**
    
    ```bash
    vim process_alignment_onlymapped.sh
    ```
    
2. **Escrever o Script**
    
    Insira o seguinte conteúdo:
    

---

### **Explicação do Formato SAM com Base no Exemplo Fornecido**

Vamos analisar o arquivo SAM fornecido, linha por linha, para entender sua estrutura e o significado de cada campo. Isso ajudará a compreender como as informações de alinhamento são representadas nesse formato.

---

### **Cabeçalho (@)**

As linhas que começam com `@` compõem o cabeçalho do arquivo SAM. Elas fornecem metadados sobre o alinhamento e a sequência de referência.

**Linha 1:**

```
@HD     VN:1.0  SO:unsorted

```

- **@HD**: Header (cabeçalho) do arquivo SAM.

**Linhas 2 a 5:**

```
@SQ     SN:NZ_CP031221.1        LN:1484007
@SQ     SN:NZ_CP046921.1        LN:1330657
@SQ     SN:NZ_CP069053.1        LN:1394392
@SQ     SN:NZ_CP072672.1        LN:1267801

```

Essas linhas fornecem informações sobre as sequências de referência usadas no alinhamento.

- **@SQ**: Sequence Header (cabeçalho de sequência).
- **SN:** Sequence Name (nome da sequência). Exemplo: `NZ_CP031221.1`.
- **LN:** Length (comprimento) da sequência em pares de bases (bp).

Essas referências correspondem a diferentes cepas ou contigs do organismo em estudo.

**Linha 6:**

```bash
@PG     ID:Bowtie       VN:1.3.1        CL:"/home/topicos/micromamba/envs/alinhamento_parte2/bin/bowtie-align-s --wrapper basic-0 -f -S -a -v 0 -p 20 -t reference_index ../infected_SRR12893566.fasta"
```

- **CL:** Command Line (linha de comando) utilizada para executar o alinhamento.

---

### **Linhas de Alinhamento**

As linhas que não começam com `@` representam as leituras e seus alinhamentos. Vamos analisar cada campo da linha de exemplo fornecida.

**Exemplo de Linha de Alinhamento:**

```bash
SRR12893566.215	16	NZ_CP031221.1	1173515	255	29M	*	0	0	ATCGAATTAAACCACATGCTCCACCGCTT	IIIIIIIIIIIIIIIIIIIIIIIIIII	XA:i:0	MD:Z:29	NM:i:0	XM:i:4
```

**Campo por Campo:**

1. **QNAME (Query Name):** `SRR12893566.215`
    - **Descrição:** Nome ou identificador da leitura. Neste caso, é a leitura número 215 do arquivo `SRR12893566`.
2. **FLAG:** `16`
    - **Descrição:** Valor numérico que indica o status da leitura.
    - **Interpretação:**
        - O valor `16` significa que a leitura está alinhada à **fita reversa** da referência.
        - O valor `4` significa que a leitura não foi alinhada (unmapped).
3. **RNAME (Reference Name):** `NZ_CP031221.1`
    - **Descrição:** Nome da sequência de referência à qual a leitura está alinhada.
    - **Interpretação:** A leitura está alinhada ao contig ou cromossomo identificado como `NZ_CP031221.1`.
4. **POS (Position):** `1173515`
    - **Descrição:** Posição inicial (1-baseada) onde o alinhamento começa na referência.
    - **Interpretação:** O alinhamento inicia na posição 1.173.515 da sequência de referência.
5. **MAPQ (Mapping Quality):** `255`
    - **Descrição:** Qualidade do alinhamento, variando geralmente entre 0 e 60.
    - **Interpretação:** O valor `255` é especial e indica que a qualidade do mapeamento não está disponível ou não foi calculada (usado pelo Bowtie quando `v` é utilizado).
6. **CIGAR:** `29M`
    - **Descrição:** Descreve as operações de alinhamento entre a leitura e a referência.
    - **Interpretação:** `29M` indica que todas as 29 bases da leitura são **matches** (alinhamento direto) com a referência, sem inserções ou deleções.
7. **RNEXT (Reference Name of the Mate/Next Read):** ``
    - **Descrição:** Nome da referência da leitura pareada (para dados pareados).
    - **Interpretação:** `` indica que não há informação sobre uma leitura pareada ou não se aplica.
8. **PNEXT (Position of the Mate/Next Read):** `0`
    - **Descrição:** Posição da leitura pareada.
    - **Interpretação:** `0` porque não há leitura pareada ou a informação não está disponível.
9. **TLEN (Template Length):** `0`
    - **Descrição:** Comprimento total do fragmento (template) incluindo ambas as leituras pareadas.
    - **Interpretação:** `0` indica que não se aplica ou não foi calculado.
10. **SEQ (Sequence):** `ATCGAATTAAACCACATGCTCCACCGCTT`
    - **Descrição:** Sequência de nucleotídeos da leitura.
    - **Interpretação:** A sequência da leitura, que deve ser complementada e revertida para corresponder à fita reversa da referência.
11. **QUAL (Quality):** `IIIIIIIIIIIIIIIIIIIIIIIIIII`
    - **Descrição:** Qualidades de cada base na leitura, no formato ASCII (Phred score).
    - **Interpretação:** Cada `I` corresponde a uma alta qualidade (Phred score de 40), indicando que a leitura é confiável.

---

Boa Atividade :)

[resposta](https://www.notion.so/resposta-1355e115016d806c889bd8b26a1a7fdb?pvs=21)