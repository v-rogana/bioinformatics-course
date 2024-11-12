# Alinhamento - Parte 2

### Guia Passo a Passo para Alinhamento de Sequências pequenas com Bowtie e Manipulação com Samtools

Este guia foca no uso do Bowtie para alinhar sequências (bibliotéca) a uma referência, utilizando ferramentas e técnicas específicas. A atividade está dividida em cinco partes principais:

1. [**Configuração do Ambiente**](#1-configuracao-do-ambiente-com-micromamba)
2. [**Construção do Índice Bowtie**](#2-construcao-do-indice-bowtie)
3. [**Alinhamento com Bowtie**](#3-alinhamento-com-bowtie)
4. [**Manipulação e Limpeza com Samtools**](#4-manipulacao-e-limpeza-com-samtools)
5. [**Montagem da Tabela de Alinhamento**](#5-montagem-da-tabela-de-alinhamento)

A principal diferença entre o Bowtie (aula de hoje) e o BLAST (aula passada) reside na otimização para tipos específicos de tarefas e no formato de saída gerado. O Bowtie oferece vantagens significativas ao trabalhar com bibliotecas de pequenas sequências, tanto em termos de velocidade quanto de compatibilidade com ferramentas e formatos padrão na bioinformática, como o formato SAM. Já o BLAST é mais adequado para análises de similaridade gerais e não é otimizado para o processamento em massa de leituras curtas, sendo mais apropriado para analises comparativas do que quantitativas.

---

### **1. Configuração do Ambiente com Micromamba**

Vamos começar configurando um ambiente isolado para garantir que todas as dependências necessárias estejam instaladas e não conflitem com outras instalações no sistema.

**Passos:**

1. Conectando ao Servidor

```bash
ssh topicos@bioinfo.icb.ufmg.br
```

`# senha: carrarataxis`

1. **Ativar um Ambiente com Micromamba**

```bash
# Ative o ambiente
micromamba activate alinhamento_parte2
```

1. **Instalar Pacotes Necessários (*NÃO FAÇAM ISSO EM AULA!!!!*)**

Isso serve apenas para caso vcs um dia queiram fazer em casa :) voce usaria o comando `micromamba install bowtie samtools` ou então pediria pro chatgpt como instalar isso no seu gerenciador de pacotes. Eu vou parar de colocar esse código para as aulas práticas, porque se voces ficarem instalando pacote num enviroment compartilhado, vai dar pau (igual já deu em outras aulas).

---

### **2. Construção do Índice Bowtie**

Antes de alinhar as sequências, precisamos construir um índice da sequência de referência. Isso permite que o Bowtie faça o alinhamento de forma eficiente.

**Passos:**

1. **Obter as Sequências de Referência e as Bibliotecas de leituras curtas (query)**
Faça isso dentro do seu diretorio! Você estará copiando os arquivos que serão usados na aula prática dessa pasta (/home/topicos/aula_alinhamento_parte2).
    
    ```bash
    cp /home/topicos/aula_alinhamento_parte2/* .
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
- `-f`: Indica que o arquivo de entrada está no formato FASTA.
- `-q`: Indica que o arquivo de entrada está no formato FASTQ.
- `-S`: Especifica que a saída será no formato SAM.
- `-a`: Reporta todos os alinhamentos que satisfazem os critérios (mais computacionalmente pesado que o -k).
- **`-k <int>`**: Limita o número de alinhamentos reportados por leitura (por exemplo, `k 1` reporta apenas o melhor).
- **`-v <int>`**: Permite até um certo número de mismatches (diferenças) no alinhamento.
- **`-p <int>`**: Define o número de threads para processamento paralelo.
- **`-y`**: Desativa o comportamento padrão de evitar regiões repetitivas na referência.
- `reference_index`: Prefixo dos arquivos de índice criados anteriormente.
- `library1.fasta`: Arquivo FASTA com as sequências de leitura.
- `> library1.sam`: Redireciona a saída para um arquivo SAM.
- `2> bowtie_infected_SRR12893566.log`: Redireciona uma segunda saída com um relatório (.log) com as informações importantes para analisar o arquivo.
    
    Certifique-se de que suas sequências de leitura estão em arquivos FASTA. Por exemplo, `library1.fasta`, `library2.fasta`, etc.
    
1. **Executar o Alinhamento**
    
    Utilizaremos o Bowtie com parâmetros específicos para otimizar o alinhamento. 
    O -p deve ser 3!
    
    ```bash
    bowtie -p 3 [seus parâmetros] reference_index infected_SRR12893566.fasta > infected_SRR12893566.sam 2> bowtiereport_SRR12893566.log
    ```
    
    **Determinar a Parametrização Ideal**
    
    **Pergunta:** Qual combinação de parâmetros você considera ideal para identificar as cepas presentes nas suas amostras? Para esse problema, pense nos seguintes pontos:
    
    - Sensibilidade vs. especificidade.
    - Presença de múltiplas cepas na referência.
    - Tamanho das sequências de leitura.
    
    *Não tenha medo de testar e errar!
    
2. **Comparar os logs entre as bibliotecas que foram alinhadas com bowtie
-** Analise o *.log das bibliotecas infectadas e não infectadas com wolbachia, o que voce consegue concluir em relação a quantidade diferencial de alinhamento? 
- Lembre que estamos analisando bibliotecas de pequenos RNAs, que são majoritariamente compostas por degradação enzimática. Apartir desses dados, formule uma hipotese para que tipo de biblioteca possui a maior quantidade de fragmentos de degradação enzimatica da bactéria wolbachia?

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

### Desafio (Quem apresentar uma resposta para o desafio e até o final da disciplina e postar no github, vai ganhar 10 pontos extra)

Olá, pessoal!

A proposta deste desafio é proporcionar uma atividade que estimule aqueles que já têm familiaridade com bioinformática, permitindo que vocês se aprofundem em técnicas de automação e processamento de dados, enquanto dedico tempo para auxiliar os colegas que estão iniciando na área.

O objetivo é que vocês desenvolvam um script de automação que realize tarefas semelhantes ao script fornecido. Esse script automatiza o processamento de vários arquivos FASTA, alinhando sequências com uma referência, filtrando leituras alinhadas e gerando uma tabela de contagens.

**Resumo das etapas realizadas pelo script:**

1. **Definição do índice de referência:** Especifica o índice que será usado pelo Bowtie para o alinhamento.
2. **Preparação do ambiente:**
    - Remove o arquivo de tabela de contagens anterior, se existir, para evitar a mistura de dados.
3. **Processamento em lote dos arquivos FASTA:**
    - Percorre todos os arquivos com extensão `.fasta` no diretório.
    - Para cada arquivo:
        - Extrai o nome base para identificar a biblioteca.
        - **Alinhamento com Bowtie:**
            - Alinha as sequências do arquivo FASTA com o índice de referência, gerando um arquivo SAM.
        - **Filtragem com Samtools:**
            - Filtra apenas as leituras que foram alinhadas (mapeadas), criando um novo arquivo SAM com essas leituras.
        - **Processamento dos dados:**
            - Utiliza comandos como `awk`, `sort` e `uniq` para contar a frequência de cada sequência mapeada.
            - Atualiza a tabela de contagens com os resultados obtidos.
        - **Limpeza de arquivos intermediários:**
            - Remove os arquivos SAM intermediários para economizar espaço em disco.

**Dicas para desenvolver um script semelhante:**

- **Escolha da linguagem de script:**
    - Embora o script fornecido esteja em Bash, vocês podem optar por outras linguagens como Python ou Perl (se você for um lunático), que oferecem bibliotecas para manipulação de arquivos e integração com ferramentas externas.
- **Automatização do fluxo de trabalho:**
    - Implementem um loop que percorra todos os arquivos de entrada (neste caso, arquivos `.fasta`).
    - Certifiquem-se de extrair corretamente os nomes dos arquivos para uso posterior.
- **Integração com ferramentas de bioinformática:**
    - Garantam que os caminhos para essas ferramentas estejam corretos e que elas estejam instaladas no ambiente onde o script será executado.
- **Processamento e análise dos resultados:**
    - Contem as ocorrências de cada sequência ou alinhamento conforme necessário.
    - Armazenem os resultados em um formato organizado, como uma tabela delimitada por tabulações (.csv/.tab/…).
- **Melhorias e otimizações:**
    - Implementem verificações de erro para garantir que cada etapa seja concluída com sucesso.
    - Considerem adicionar opções de linha de comando para tornar o script mais flexível.
    - Documentem o código para facilitar a compreensão e manutenção futura.

Este desafio é uma ótima oportunidade para praticar habilidades de programação e entender melhor como automatizar processos comuns em bioinformática. Incentivo vocês a explorarem diferentes abordagens e compartilharem insights entre si e tirar duvidas comigo.

Boa sorte e divirtam-se.

---

## **Explicação do Formato SAM**

Vamos analisar o arquivo SAM fornecido, linha por linha, para entender sua estrutura e o significado de cada campo. Isso ajudará a compreender como as informações de alinhamento são representadas nesse formato.

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
        - Se for o valor `4` significa que a leitura não foi alinhada (unmapped).
        - Lembre disso para o codigo operado com o samtools onlymapped - se voce remover as linhas com valor `4` o que acontece?
3. **RNAME (Reference Name):** `NZ_CP031221.1`
    - **Descrição:** Nome da sequência de referência à qual a leitura está alinhada.
    - **Interpretação:** A leitura está alinhada ao contig ou cromossomo identificado como `NZ_CP031221.1`.
    - Lembre dessa coluna para o código de contagem na tabela - se você contar o número de linhas com alinhamento em um nome único (sequencia especifica da referencia), você tem uma tabela de contagens.
    
    {Apartir daqui, essas colunas não serão usadas para aula, mas vale a explicação, quando vc se tornar bioinformata, pode usar todos esses dados para responder e plotar coisas muito interessantes dependendo do seu problema. Portanto ENTENDA os formatos de output!}
    
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

Boa Atividade :)

[resposta](https://www.notion.so/resposta-1355e115016d806c889bd8b26a1a7fdb?pvs=21)