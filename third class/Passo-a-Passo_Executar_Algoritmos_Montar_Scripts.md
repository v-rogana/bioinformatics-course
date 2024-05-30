# Guia Passo-a-Passo para Executar Algoritmos e Montar Scripts

Este guia foca no uso do Bowtie para alinhar sequências de DNA a uma referência, usando as ferramentas e técnicas apresentadas. A atividade será dividida em três partes principais: construção do índice Bowtie, alinhamento das sequências e manipulação dos resultados com Samtools.

### **Visão Geral das Atividades**

1. **Crie um ambiente Conda:** Configuração inicial para executar todas as ferramentas necessárias para o alinhamento com Bowtie e manipulação de dados com Samtools.
2. **Construção do Índice Bowtie**: Criação de índices a partir de um arquivo FASTA de referência.
3. **Alinhamento com Bowtie**: Utilização do Bowtie para alinhar sequências de bibliotecas FASTA à referência.
4. **Manipulação e Limpeza com Samtools**: Processamento dos arquivos SAM para extração de informações na analise posterior.
5. **Automatização das tarefas 3 e 4 :** Vamos aprender a montar scripts em bash automatizando a execução das duas ultimas tarefas.
6. **Montando Script de Contagem de Alinhamento:** Criação de um script for-loop para contagem de reads alinhados na nossa referência.

### **1. Crie um ambiente Conda**

- Siga os passos de criação do ambiente

```bash
# Crie o ambiente
conda create -n my_second_env python=3.8
# Ative o ambiente
conda activate my_second_env
# Instale os pacotes úteis para o seu fluxo de trabalho
conda install -c bioconda bowtie
```

### **2. Construção do Índice Bowtie**

Antes de alinhar, é necessário construir um índice Bowtie da sequência de referência. Isso facilita o alinhamento rápido das sequências de leitura.

- **Construa o índice**:
    - Use o comando **`bowtie-build` .**
    - Remova o arquivo fasta para reduzir espaço (apenas os fastas relativos ao indice).

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

Faça um script que extrai as referências usando **`awk`**, conta e ordena as ocorrências usando **`sort`** e **`uniq -c`**. Em seguida, para cada linha de saída, ele extrai o nome da biblioteca, formata a saída e a escreve diretamente no arquivo de saída definido.

- Use o chatgpt para te ajudar a realizar a tarefa **(opcional mas recomendado)**
- Use o `nano` para criar um arquivo chamado `process_sam_and_create_counts_table.sh`
- A primeira linha do arquivo deve ser `#!/bin/bash`
- Use o comando **`awk '$1 !~ /^@/ {print $3}'` (opcional)**
    - **`awk`**: Uma ferramenta de manipulação de texto que permite analisar e transformar textos. Neste caso, é usada para processar linhas em um arquivo SAM.
    - **`$1 !~ /^@/`**: Este trecho filtra as linhas que, na primeira coluna, começar com o símbolo "@", que são linhas de cabeçalho no formato SAM.
    - **`{print $3}`**: Se a linha não começar com "@", esta ação é executada, que imprime o conteúdo da terceira coluna, que normalmente contém o identificador da sequência de referência para onde a leitura foi alinhada.
    - **`| uniq -c`**: Conta o número de entradas únicas.
- O output final deve ser um arquivo tabular com 3 colunas `count\ref\library`
    - count é o número de contagens de reads alinhados na referencia.
    - ref é a referencia usada de wolbachia.
    - library é de onde veio a contagem em relação as bibliotecas de pequenos RNAs.

**Dica: Antes de trabalhar com um arquivo (no caso o .sam), dedique um momento para entender seu conteúdo e estrutura. Isso facilitará a manipulação e a análise dos dados de forma mais eficiente e segura.**

### **6. Automatização das tarefas 3/4/5**

Faça um for-loop que realiza as duas ultimas tarefas de forma automatizada, para isso, crie um script que execute o `bowtie` com as flags usadas na atividade 3 e por fim, filtra o output da ferramenta com `samtools view`, como foi feito na atividade 4.

- Use o chatgpt para te ajudar a realizar a tarefa **(opcional mas recomendado)**
- Use o `nano` para criar um arquivo chamado `process_alignment_onlymapped.sh`
- A primeira linha do arquivo deve ser `#!/bin/bash`
- use os comandos:  **(opcional)**
    - `lib=$(basename "$i" .fasta)` :  extrair o nome base de um arquivo, removendo a extensão especificada (neste caso, **`.fasta`**), e atribuir esse nome base a uma variável chamada **`lib`.**
    - `bowtie + flags + fasta > ${lib}.sam`
    - `samtools view + flags + ${lib}.sam > ${lib}_onlymapped.sam`

Desafio: transforme a tarefa 5 em um for loop (melhorar essa parte pra dar dicas)