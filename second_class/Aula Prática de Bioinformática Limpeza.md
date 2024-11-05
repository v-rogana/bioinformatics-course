# Aula Prática de Bioinformática: Limpeza e Preparação de Dados de Sequenciamento

Nesta aula, iremos realizar juntos o pré-processamento de dados de uma biblioteca 16S de DNA, utilizando as ferramentas **FastQC**, **Trim Galore**, e **seqtk**. Vamos seguir um passo a passo detalhado para garantir que as leituras estejam limpas e prontas para análise.

### 1. Conectando ao Servidor

Vamos começar conectando ao servidor onde trabalharemos com os dados. Use o seguinte comando de SSH para entrar no servidor:

```bash
ssh topicos@bioinfo.icb.ufmg.br
# senha: carrarataxis
```

**SSH** é um protocolo que permite acesso seguro a um servidor remoto. Estamos nos conectando ao servidor `bioinfo.icb.ufmg.br` com o usuário `topicos`.

Após conectar, faça um **ls** (listar arquivos e pastas no diretório atual) e um **pwd** para se localizar. Você estará no diretório `/home/topicos`.

- `ls`: Lista os arquivos e pastas no diretório atual.
- `pwd`: Mostra o caminho completo do diretório atual.

```bash
ls
pwd
```

### 2. Entrando no Diretório da Aula

No diretório inicial, há uma pasta chamada com seu nome, ou então crie uma para voce. Vamos entrar nela para iniciar a aula:

```bash
cd minha_pasta
# ou
mkdir minha_pasta
```

### 3. Preparando o Ambiente

Antes de começarmos a trabalhar com os dados, precisamos ativar o ambiente `micromamba` configurado para esta aula, onde já temos as ferramentas instaladas. Use o comando abaixo:

obs: Compare com a instalação local do [trimgalore](https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/)

```bash
micromamba activate aula3_trimmagem
# Já esta instalado, mas apenas por demonstração mostrar como foi instalado o trimgalore
# Usar o chatgpt na frente deles, mostrando como seria feito no mundo real
micromamba install -c bioconda trim-galore
```

**O que é um ambiente virtual?**

- É uma forma de isolar pacotes e dependências específicos para um projeto, evitando conflitos com outras instalações.

Agora estamos prontos para trabalhar!

### 4. Baixando a Biblioteca de Sequenciamento

**Informações da biblioteca:**

- **ID:** SRR1289356
- **Descrição:** Biblioteca metagenômica 16S de microbioma intestinal humano.
- **Tipo de leitura:** Single-end (apenas uma extremidade é sequenciada).

**Baixando a biblioteca:**

```bash
fasterq-dump SRR1289356
```

- **O que este comando faz?**
    - `fasterq-dump` é uma ferramenta que baixa dados do SRA e converte para o formato FASTQ.
    - `SRR1289356` é o identificador único da nossa biblioteca de interesse.

**Por que não usamos `--split-files`?**

- Como esta biblioteca é **single-end**, não há necessidade de separar arquivos de leitura forward e reverse.

### 5. Verificando a Qualidade com FastQC

Após o download, o primeiro passo é verificar a qualidade das nossas leituras. Para isso, usamos a ferramenta **FastQC**, que gera um relatório gráfico sobre a qualidade dos dados de sequenciamento:

```bash
fastqc SRR1289356.fastq
```

*Verifique o relatório que será gerado no diretório atual. Analise as métricas como qualidade de base por posição, conteúdo GC, presença de adaptadores, etc.*

### 6. Limpando os Dados com Trim Galore

Agora, vamos remover sequências de baixa qualidade, adaptadores e bases indefinidas.

```bash
trim_galore --fastqc -q 25 --trim-n --max_n 0 -j 1 --dont_gzip SRR1289356.fastq
```

**Vamos entender cada parte do comando**:

**`--fastqc`**: Gera um relatório do FastQC **antes e depois** do trimming.

**`-q 25`**: Define o **limite de qualidade** para as bases. Bases com qualidade inferior a 25 serão removidas.

**`--trim-n`**: Remove bases 'N' (bases indefinidas) do início e fim das leituras.

**`--max_n 0`**: Remove leituras que contenham ao menos um 'N'.

**`-j 1`**: Usa **1 thread** para o processamento. Para grandes volumes de dados, poderíamos aumentar o número de threads.

**`--dont_gzip`**: Gera o arquivo de saída no formato **não comprimido** (`.fastq`), em vez de `.gz`.

Após o comando, o **Trim Galore** vai gerar um arquivo FASTQ limpo, pronto para as próximas etapas de análise.

### 7. Convertendo o FASTQ para FASTA

Depois de limpar as leituras, podemos convertê-las para o formato **FASTA**, que é frequentemente utilizado em análises subsequentes, como alinhamentos e buscas de similaridade. Para isso, utilizamos a ferramenta **seqtk**:

```bash
seqtk seq -A SRR1289356_trimmed.fq > SRR1289356_trimmed.fasta
```

Agora temos um arquivo **FASTA** com as leituras limpas!

### 8. Comparando a Qualidade Pós-Trimming

Vamos verificar se o processo de trimming melhorou a qualidade das nossas sequências.

**Analisando o novo relatório FastQC:**

- Abra o arquivo `SRR1289356_trimmed_fastqc.html` no seu navegador.
- Compare com o relatório inicial.
- **O que observar:**
    - Melhoria na qualidade das bases nas extremidades.
    - Redução ou eliminação de adaptadores.
    - Diminuição de bases 'N'.

---

### Resumo do que foi feito:

- Conectamos ao servidor e preparamos o ambiente.
- Baixamos uma biblioteca de sequenciamento 16S do SRA.
- Verificamos a qualidade das leituras com FastQC.
- Realizamos a limpeza das leituras utilizando Trim Galore.
- Convertimos as leituras limpas para o formato FASTA.

Este é um fluxo básico de pré-processamento de dados de sequenciamento, mas essencial para garantir que os dados que entrarem em nossas análises estejam em boas condições!

### Agora é a Sua Vez!

Agora que já fizemos a parte prática juntos, é hora de você repetir o processo por conta própria, mas com as bibliotecas do nosso projeto de **detecção de Wolbachia por alinhamento**. Vamos trabalhar com quatro bibliotecas de sequenciamento que já estão disponíveis no servidor. Siga as instruções abaixo para completar a tarefa.

### Passo 1: Listando as Bibliotecas Disponíveis

As bibliotecas já foram baixadas do **SRA** e estão localizadas no seguinte caminho:

```bash
ls /home/topicos/BIBLIOTECAS_SEQUENCIADAS_SRA
```

**O que você deve observar?**

- Existem **4 bibliotecas** no diretório. Duas dessas bibliotecas são de amostras infectadas com **Wolbachia** e as outras duas são controles (não infectadas).
- A ideia é que você processe todas essas bibliotecas, tanto as infectadas quanto as controles, para garantir que as leituras estejam limpas e prontas para análise.

### Passo 2: Objetivo

Nosso objetivo é **limpar as sequências** dessas quatro bibliotecas para, posteriormente, diferenciarmos entre as amostras infectadas e as não infectadas. Isso é fundamental para conferir a validade do nosso projeto de detecção **in silico** da bactéria **Wolbachia**.

### Passo 3: Documentação do Trim Galore

Antes de começar, é **importante** que você leia a documentação oficial do Trim Galore:

- [Trim Galore Documentation](https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/)

### Passo 4: Realizando o Processamento de Trimagem

Agora, você vai repetir o processo de **trimming** (limpeza) que fizemos juntos, mas usando as quatro bibliotecas disponíveis. Siga as etapas abaixo para cada uma delas.

**Dicas Importantes:**

1. **Use o path completo** ao especificar os arquivos de entrada.
2. Utilize o comando `nohup` para rodar o processo em segundo plano, caso o processamento demore.
3. **Remova todas as leituras que contenham N** (representando bases indefinidas).
4. Utilize a flag **`--dont_gzip`** para que os arquivos de saída não sejam comprimidos.
5. Gere automaticamente o relatório **FastQC** para verificar a qualidade pós-trimming.
6. Como estamos trabalhando com uma biblioteca de pequenos RNAs, você pode decidir **remover sequências muito curtas ou muito longas** para garantir que estamos analisando apenas as sequências de interesse.
7. Para otimizar os recursos do servidor, **use apenas um core** (thread).

### Comandos Sugeridos:

Para cada biblioteca, execute um comando como este (lembre-se de ajustar o nome do arquivo para cada uma):

```bash
nohup trim_galore -j 1 --dont_gzip [*] /home/topicos/BIBLIOTECAS_SEQUENCIADAS_SRA/SRR12893567.fastq &
# Na região [*], coloque outras flags que achar válido
```

### Dicas Extras:

- **Remover leituras de pequeno tamanho:** Como estamos lidando com **pequenos RNAs**, você pode utilizar o parâmetro `--length` para remover sequências muito curtas.

Bom trabalho!