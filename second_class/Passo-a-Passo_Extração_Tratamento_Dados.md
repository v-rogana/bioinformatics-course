# Guia Passo-a-Passo para Extração e Tratamento de Dados

### Atividade prática aula 2

Este guia fornece instruções detalhadas sobre como configurar um ambiente Conda adequado para bioinformática, executar scripts Python para buscar sequências de DNA a partir de números de acesso, e processar dados de sequenciamento de RNA, a partir do banco de dados do SRA. O foco é aprender a usar as ferramentas necessárias para executar, desde a configuração inicial do ambiente até as etapas finais de extração e tratamento dos dados de sequência.

### **Visão Geral das Atividades**

1. **Configuração do Ambiente**: Instalação do Miniconda e configuração de um ambiente Conda para gerenciar as dependências necessárias para as ferramentas de bioinformática.
2. **Busca de Sequências**: Utilização de um script Python para extrair sequências de DNA a partir da base de dados do NCBI, usando a API do Entrez.
3. **Preparação de Dados de Sequenciamento**: Download e limpeza de dados do Sequence Read Archive (SRA), seguido de processos para melhorar a qualidade e usabilidade dos dados para análises futuras.

### **1. Instalação do Miniconda**

O Miniconda é uma versão mais enxuta do Anaconda que inclui o gerenciador de pacotes Conda e Python, facilitando a instalação rápida e exigindo menos recursos do sistema.

- Crie um diretório para instalar o Miniconda:

```bash
mkdir -p ~/miniconda3
```

- Baixe o instalador do Miniconda para Linux:

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
```

- Execute o instalador:

```bash
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
```

- Remova o instalador para limpar:

```bash
rm ~/miniconda3/miniconda.sh
```

- Inicialize o Conda para os shells bash:

```bash
~/miniconda3/bin/conda init bash
# Depois use o comando
source ~/.bashrc
```

### 2. Configuração do Ambiente Conda

Agora que o Miniconda está instalado, configure um ambiente específico para suas análises.

- Crie um novo ambiente Conda chamado `my_env` com Python 3.8:

```bash
conda create -n my_env python=3.8
```

- Ative o ambiente:

```bash
conda activate my_env
```

- Adicione os canais necessários para bioinformática:

```bash
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict
```

Instale os pacotes necessários:

```bash
# install trim-galore
conda install trim-galore
# install biopython
conda install -c conda-forge biopython
# install sratoolkit
conda install -c bioconda sra-tools
# install setqk
conda install -c bioconda seqtk
# install bowtie 
conda install -c bioconda bowtie
```

### 3. Execução do Script Python para Busca de Sequências

Após a configuração do ambiente, um script Python será utilizado para buscar sequências de DNA específicas. Este script vai interagir com bancos de dados públicos (NCBI) para obter dados de sequencia de nucleotídeos das cepas de Wolbachia que vamos alinhar nas bibliotecas do SRA.

- Crie e edite o script com `nano fetch_sequences.py`:
    - Use o chatgp para te ajudar a montar o código.
    - accession_numbers = ["CP031221.1", "CP046925.1", "CP072672.1", "CP046921.1", "AP013028.1"]
    - Pacotes python from `Bio import Entrez / from Bio import SeqIO / import sys`
    - Use o comando `Entrez.efetch` na base de dados `nucleotide`
    - Pegar sequencias no formato FASTA
    - Salvar os dados em um arquivo chamado `referencia_wolbachia.fasta`
- Preparar, ativar e executar o script:

```bash
# Comando para escrever o script
nano fetch_sequences.py
# Comando para ativar o script
chmod +x fetch_sequences.py
# Comando para executar o script
python fetch_sequences.py
```

- Verifique se as sequências foram salvas corretamente (sanity check):

```bash
grep “>” sequences.fasta
```

### 4. Análise Adicional e Limpeza

Esta seção aborda o download e a preparação inicial dos dados de sequenciamento, essencial para garantir a qualidade e a precisão das análises subsequentes.

- Baixe e processe os dados de sequenciamento com `fasterq-dump` e `trim-galore`:
    - Use o comando `fasterq-dump` na biblioteca para extrair o dado do SRA
    - Use o `trim_galore` para limpar a biblioteca, usando flags para:
        - O arquivo de entrada para o processo de trimming é o `*.fastq`
        - Definir o comprimento mínimo de sequência aceitável.
        - Remova as sequencias com bases 'N'. 'N' indica uma base não determinada.
        - Defina o numero de threads da CPU a serem usadas da servidora (use htop para saber quantas você tem).
        - Rodar o FastQC nos arquivos de saída após o corte das sequências.
        - Evitar arquivos de saída sejam comprimidos em formato gzip, ou descompacte eles.
- Converta o arquivo FASTQ cortado para FASTA:
    - Use o comando `seqtk seq`
    - Remova o arquivo *_trimmed.fq