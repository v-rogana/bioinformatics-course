# Passo a Passo do Projeto Completo

## Objetivo

O objetivo deste guia é integrar a fundamentação teórica com as etapas práticas de bioinformática, para detectar **Wolbachia** em bibliotecas de pequenos RNAs. Desde o download e pré-processamento dos dados, passando pelo alinhamento contra genomas de Wolbachia, até a análise de expressão relativa e visualização dos resultados em **R**.

## Introdução Teórica

**Contexto Biológico:**
Mosquitos Aedes aegypti são vetores de doenças como Dengue, Zika e Chikungunya. A infecção pelo endossimbionte **Wolbachia pipientis** pode interferir na replicação viral dentro desses mosquitos, reduzindo a transmissão de patógenos. Embora não haja evidências de vias reguladas de pequenos RNAs derivadas especificamente de Wolbachia, a presença de fragmentos do genoma bacteriano em bibliotecas de pequenos RNAs pode indicar a degradação enzimática do material genético da bactéria.

**Por que Pequenos RNAs?**
Pequenos RNAs (até 50 nt) têm funções regulatórias e de defesa antiviral. Apesar de Wolbachia não gerar esses RNAs de forma regulada, sequências correspondentes à bactéria podem ser detectadas em bibliotecas de pequenos RNAs, refletindo a interação do endossimbionte com o hospedeiro.

## Fluxo de Trabalho Prático

1. **Preparação do Ambiente Bioinformático**
2. **Download e Pré-processamento das Leituras**
3. **Download das Sequências de Referência de Wolbachia**
4. **Alinhamento contra a Referência de Wolbachia**
5. **Filtragem, Quantificação e Análise Estatística**
6. **Visualização no R (Heatmap)**

A seguir, apresentamos cada etapa com o código correspondente, mantendo-o **exatamente igual** ao fornecido inicialmente e adicionando comentários teóricos e práticos.

---

## Metadados das Bibliotecas

As tabelas a seguir mostram as amostras disponíveis.

**Bibliotecas Infectadas (Wolbachia)**

| SRR accession | Infected Aegypti |
| --- | --- |
| SRR12893565 | wAlbB_infected-12days-R3 |
| SRR12893566 | wAlbB_infected-12days-R2 |
| SRR12893567 | wAlbB_infected-12days-R1 |
| SRR12893568 | wAlbB_infected-6days-R3 |
| SRR12893569 | wAlbB_infected-6days-R2 |
| SRR12893570 | *wAlbB_infected-6days-R1* |
| SRR12893571 | *wAlbB_infected-2days-R3* |
| SRR12893580 | *wAlbB_infected-2days-R2* |
| SRR12893581 | *wAlbB_infected-2days-R1* |

**Bibliotecas Não Infectadas (Controle)**

| SRR accession | Uninfected Aegypti |
| --- | --- |
| SRR12893564 | uninfected-2days-R1 |
| SRR12893572 | uninfected-12days-R3 |
| SRR12893573 | *uninfected-12days-R2* |
| SRR12893574 | *uninfected-12days-R1* |
| SRR12893575 | *uninfected-6days-R3* |
| SRR12893576 | *uninfected-6days-R2* |
| SRR12893577 | *uninfected-6days-R1* |
| SRR12893578 | *uninfected-2days-R3* |
| SRR12893579 | *uninfected-2days-R2* |

## 1. Preparando o Ambiente Conda

Ambientes virtuais garantem a reprodutibilidade da análise, isolando dependências de pacotes. Aqui usamos `micromamba`/`conda` para criar e gerenciar o ambiente.

Instalamos ferramentas essenciais como seqtk, sra-tools, trim-galore, bowtie e samtools para trabalhar com dados de sequenciamento e alinhamento.

---

```bash
### CASO VOCE CRIE UM NOVO ENVIROMENT, FAÇA COM OUTRO NOME############
micromamba create -n xxxxx
micromamba activate bioinfo_env
# Caso voce faça esse passo, crie um enviroment novo com um novo nome
micromamba install -n bioinfo_env -c bioconda seqtk
micromamba install -n bioinfo_env -c bioconda sra-tools
micromamba install -n bioinfo_env -c bioconda trim-galore
micromamba install -n bioinfo_env -c bioconda bowtie
micromamba install -n bioinfo_env -c bioconda samtools
```

---

## 2. Download e Pré-processamento das Leituras

O `fasterq-dump` faz download de dados do SRA. O `Trim Galore` remove adaptadores e bases de baixa qualidade, garantindo leituras mais confiáveis. O `seqtk` converte formatos fastq para fasta.

Abaixo, um exemplo para uma única amostra SRR12893567, onde após o download, realizamos limpeza e conversão.

```bash
fasterq-dump SRR12893567
# Run Trim Galore
trim_galore --fastqc --length 18 --trim-n --max_n 0 -j 7 --dont_gzip SRR12893567.fastq
# Remove the original fastq file to save space
rm -f SRR12893567.fastq
# Convert trimmed fastq to fasta using seqtk
seqtk seq -A SRR12893567_trimmed.fq > SRR12893567_trimmed.fasta
# Optionally, remove the trimmed fastq file to save space
rm -f SRR12893567_trimmed.fq
```

---

## 3. Baixando Sequências de Wolbachia

O genoma de Wolbachia é obtido do NCBI em formato FASTA. Essas sequências serão usadas para criar um índice de referência e, depois, alinhar as leituras das bibliotecas de pequenos RNAs.

Usamos `wget` para baixar as referências em lote. Ao final, concatenamos todas em um único multifasta.

```bash
# Baixar as sequências em formato FASTA
wget -O wAlbB.fasta "<https://www.ncbi.nlm.nih.gov/sviewer/viewer.fcgi?id=CP031221.1&db=nuccore&report=fasta&retmode=text>"
wget -O wMel_CP046925.fasta "<https://www.ncbi.nlm.nih.gov/sviewer/viewer.fcgi?id=CP046925.1&db=nuccore&report=fasta&retmode=text>"
wget -O wMel_CP072672.fasta "<https://www.ncbi.nlm.nih.gov/sviewer/viewer.fcgi?id=CP072672.1&db=nuccore&report=fasta&retmode=text>"
wget -O wMelPop.fasta "<https://www.ncbi.nlm.nih.gov/sviewer/viewer.fcgi?id=CP046921.1&db=nuccore&report=fasta&retmode=text>"
wget -O wCle.fasta "<https://www.ncbi.nlm.nih.gov/sviewer/viewer.fcgi?id=AP013028.1&db=nuccore&report=fasta&retmode=text>"

# Concatenar todas as sequências em um único arquivo multifasta
cat wAlbB.fasta wMel_CP046925.fasta wMel_CP072672.fasta wMelPop.fasta wCle.fasta > wolbachia_multifasta.fasta

```

**Opção Manual:** Você pode baixar manualmente pelo NCBI, enviando os IDs e baixando em formato FASTA.

---

## 4. Alinhamento contra Wolbachia

**Teoria:**

O `bowtie-build` cria um índice para mapeamento rápido. O `bowtie` alinha as leituras contra o genoma de Wolbachia, permitindo identificar quais amostras possuem sequências bacterianas. Parâmetros conservadores (`-v 0`) garantem alinhamentos exatos. Primeiro criamos o índice, depois alinhamos.

```bash
# Criar index
bowtie-build wolbachia_multifasta.fasta wolb_ref
# Alinhamento
bowtie -f -S -a -v 0 -p 20 -t wolb_ref uninfected_SRR12893564.fasta > uninfected_SRR12893564_aligned.sam 2> uninfected_SRR12893564_bowtie.log
# Limpeza onlymapped com Samtools
samtools view -S -h -F 4 uninfected_SRR12893564_aligned.sam > uninfected_SRR12893564_onlymapped.sam
samtools view -Sb uninfected_SRR12893564_aligned.sam > uninfected_SRR12893564.bam
# Tabela de Contagem
echo "Count,Reference,Library" > counts_table.csv
# Loop through all *onlymapped.sam files
for lib in *onlymapped.sam; do
  awk '$1 !~ /^@/ {print $3}' $lib | sort | uniq -c | awk -v library="$lib" '{print $1","$2","library}' >> counts_table.csv
done

```

**Automatizando a Tabela de Contagem para Múltiplas Bibliotecas:**

```bash
# Resolvendo com o for loop
for i in *fasta; do
    lib=$(basename "$i" .fasta)
    bowtie -f -S -a -v 0 -p 20 -t wolb_ref $i > "${lib}_aligned.sam" 2> "${lib}_bowtie.log"
    samtools view -S -h -F 4 "${lib}_aligned.sam" > "${lib}_onlymapped.sam"
    samtools view -Sb "${lib}_aligned.sam" > "${lib}.bam"
    rm "${lib}_aligned.sam"
done

```

---

## 5. Preparando Dados para Análise em R

Após o alinhamento, gera-se uma tabela de contagem da quantidade de leituras por referência de Wolbachia. Esses dados serão utilizados para análises de expressão relativa (RPM) e visualização em heatmaps.

Transferimos os arquivos de contagem para a máquina local (se estiverem em um servidor remoto):

```bash
scp topicos@bioinfo.icb.ufmg.br:/home/topicos/revisao_geral_aula/alinhamento_bowtie/count*

```

---

## 6. Análise de Dados e Visualização no R

No R, integramos as contagens com metadados (como se a amostra é infectada ou não, tempo após a infecção, etc.), calculamos normalização por RPM (reads per million) e produzimos um heatmap. O heatmap é uma representação visual que facilita a comparação entre bibliotecas e cepas de Wolbachia detectadas.

O código a seguir exemplifica o carregamento, processamento e visualização dos dados. Também mostra testes estatísticos (ANOVA) para avaliar diferenças na expressão entre grupos.

```r
# Data treatment to make the Heatmap
# Carregando os dados
library(dplyr)
library(reshape2)
counts_bowtie_av0 <- read.csv("C:\\\\Users\\\\victo\\\\Documents\\\\Aula_Introducao_R_2024\\\\counts_table.csv") # (lib/strain/count)
metadados_wolb = read.csv("C:\\\\Users\\\\victo\\\\Downloads\\\\metadados_wolb.csv") # informações da biblioteca (reads trimadas + tipo de tratamento)
wolbs <- read.csv("C:\\\\Users\\\\victo\\\\Downloads\\\\cepas_de_wolbachia.csv") # info sobre o titulo da cepa

# modificando os nomes das libs
counts_bowtie_av0$Library <- gsub("infected_|uninfected_|_onlymapped\\\\.sam", "", counts_bowtie_av0$Library)

counts_av0 <- inner_join(counts_bowtie_av0, metadados_wolb, by = c("Library"="library"))

# Calcular RPM
counts_av0 <- counts_av0 %>%
  mutate(RPM = (Count / lib_size) * 1e6)

# Adicionando Cepa
counts_av0$corrected <- sub("NZ_", "", counts_av0$Reference)
counts_av0 <- inner_join(counts_av0, wolbs, by = c("corrected"="AccessionVersion"))

# Matriz para Heatmap
d_counts_av0 <- counts_av0 %>% dcast(Library + tratamento_tetraciclina ~ corrected + Title_reduced, value.var = "RPM")
d_counts_av0_matrix <- as.matrix(d_counts_av0[, c(-1, -2)])
row.names(d_counts_av0_matrix) <- paste0(d_counts_av0[, 1], "_", d_counts_av0[, 2])

# Heatmap code
library(ComplexHeatmap)
library(circlize)
library(viridis)

Heatmap(d_counts_av0_matrix,
        name = "RPM",
        row_title = "Bibliotecas de Pequenos RNAs",
        row_title_gp = gpar(fontsize = 14, fontface = "bold"),
        row_dend_side = "left",
        row_names_side = "left",
        row_names_gp = gpar(fontsize = 10),
        column_title = "Cepas de Wolbachia",
        column_title_gp = gpar(fontsize = 10, fontface = "bold"),
        column_names_gp = gpar(fontsize = 6),
        col = viridis(100)
)

# Heatmap com pheatmap
library(pheatmap)

pheatmap(
  d_counts_av0_matrix,
  color = viridis(100),
  main = "Heatmap de RPM",
  fontsize_row = 8,
  fontsize_col = 8,
  fontsize = 10,
  border_color = NA
)

# Teste ANOVA
counts_av0$tratamento_tetraciclina <- as.factor(counts_av0$tratamento_tetraciclina)
anova_result <- aov(RPM ~ tratamento_tetraciclina, data = counts_av0)
summary(anova_result)
```

---

Parabens 🥳

Se você chegou nesse ponto, você conseguiu fazer seu próprio sensor de sequencias de Wolbachia em bibliotecas de pequenos RNAs!!!