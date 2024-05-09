# Guia Passo-a-Passo para Executar Algoritmos e Montar Scripts

### **Atividade prática aula 3**

Este guia foca no uso do Bowtie para alinhar sequências de DNA a uma referência, usando as ferramentas e técnicas apresentadas. A atividade será dividida em três partes principais: construção do índice Bowtie, alinhamento das sequências e manipulação dos resultados com Samtools.

### **Visão Geral das Atividades**

1. **Crie um ambiente Conda:** Configuração inicial para executar todas as ferramentas necessárias para o alinhamento com Bowtie e manipulação de dados com Samtools.
2. **Construção do Índice Bowtie**: Criação de índices a partir de um arquivo FASTA de referência.
3. **Alinhamento com Bowtie**: Utilização do Bowtie para alinhar sequências de bibliotecas FASTA à referência.
4. **Manipulação e Limpeza com Samtools**: Processamento dos arquivos SAM para extração de informações na analise posterior.
5. **Montando Script de Contagem de Alinhamento:** Vamos aprender a montar scripts em bash com um exercicio de for-loop para contagem de reads alinhados na nossa referência.

### **1. Crie um ambiente Conda**

- Siga os passos de criação do ambiente

```bash
# Crie o ambiente
conda create -n aula3_env python=3.8
# Ative o ambiente
conda activate aula3_env
# Ative os canais necessarios
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict
# Instale os pacotes úteis para o seu fluxo de trabalho
conda install bowtie samtools
```

### **2. Construção do Índice Bowtie**

Antes de alinhar, é necessário construir um índice Bowtie da sequência de referência. Isso facilita o alinhamento rápido das sequências de leitura.

- **Construa o índice**:
    - Use o comando **`bowtie-build` .**
    - Remova o arquivo fasta para reduzir espaço.

### **3. Alinhamento com Bowtie**

Após a construção do índice, você procederá ao alinhamento das bibliotecas de sequências.

- **Realize o alinhamento**:
    - Use o chatgpt e a documentação do [bowtie](https://bowtie-bio.sourceforge.net/manual.shtml) para te ajudar.
    - Que parâmetros do Bowtie você precisa ajustar para garantir um alinhamento sensível e específico?
    - Use a flag que aceita formato de entrada como fasta.
    - Determine o formato de saida como SAM.
    - Use a flag `-a` que reportará todos os alinhamentos possíveis para uma leitura que satisfaçam as condições de alinhamento especificadas (por exemplo, número de mismatches permitidos).
    - Verifique o significado de **`v`** e escolha o número de mismatches.
    - Use 4 threads (confira com `htop` quantas threads estão disponíveis para você).

### **4. Manipulação e Limpeza com Samtools**

Finalmente, use Samtools para manipular os arquivos de alinhamento e preparar os dados para análises posteriores.

- Filtre com `samtools view` apenas os dados alinhados:
    - Use a flag que indica que a entrada é em formato SAM.
    - Use a flag `-h` para incluir o cabeçalho no output de saida.
    - Use a flag é usada para filtrar reads que foram alinhadas.
    - Considere remover os arquivos SAM se houver falta de espaço.

### 5. **Montando Script de Contagem de Alinhamento**

Faça um for-loop que itera sobre todos os arquivos que terminam com **`onlymapped.sam`**, extrai as referências usando **`awk`**, conta e ordena as ocorrências usando **`sort`** e **`uniq -c`**. Em seguida, para cada linha de saída, ele extrai o nome da biblioteca, formata a saída e a escreve diretamente no arquivo de saída definido.

- Use o chatgpt para te ajudar a realizar a tarefa **(opcional mas recomendado)**
- Use o `nano` para criar um arquivo chamado `process_sam_and_create_counts_table.sh`
- A primeira linha do arquivo deve ser `#!/bin/bash`
- Use o comando **`awk '$1 !~ /^@/ {print $3}'` (opcional)**
    - **`awk`**: Uma ferramenta de manipulação de texto que permite analisar e transformar textos. Neste caso, é usada para processar linhas em um arquivo SAM.
    - **`$1 !~ /^@/`**: Este trecho filtra as linhas. O **`$1`** refere-se à primeira coluna de cada linha (no contexto de arquivos SAM, a primeira coluna é o nome da sequência/leitura). A expressão **`!~ /^@/`** indica que a linha deve ser ignorada se a primeira coluna começar com o símbolo "@", que são linhas de cabeçalho no formato SAM.
    - **`{print $3}`**: Se a linha não começar com "@", esta ação é executada, que imprime o conteúdo da terceira coluna, que normalmente contém o identificador da sequência de referência para onde a leitura foi alinhada.
    - **`| sort`**: Este é um comando que recebe a saída do comando anterior (**`awk`**) e ordena as linhas. Isso é necessário para o próximo passo, que conta ocorrências únicas.
    - **`| uniq -c`**: Filtra ou agrega linhas consecutivas idênticas de entrada ordenada, prefixando cada linha com o número de ocorrências. **`c`** adiciona a contagem de quantas vezes cada linha foi encontrada, que são essencialmente as contagens de quantas vezes cada referência apareceu nas leituras alinhadas.
- O output final deve ser um arquivo tabular com 3 colunas `count\ref\library`
    - count é o número de contagens de reads alinhados na referencia.
    - ref é a referencia usada de wolbachia.
    - library é de onde veio a contagem em relação as bibliotecas de pequenos RNAs.

[!IMPORTANT]
**Antes de trabalhar com um arquivo (no caso o .sam), dedique um momento para entender seu conteúdo e estrutura. Isso facilitará a manipulação e a análise dos dados de forma mais eficiente e segura.**