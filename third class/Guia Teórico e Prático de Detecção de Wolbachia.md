# Guia Teórico e Prático de Detecção de Wolbachia em Bibliotecas de Pequenos RNAs

## Objetivo

Orientá-los nos passos práticos para detectar sequências de **Wolbachia** em bibliotecas de pequenos RNAs, desde o download das sequências até a visualização dos dados em um heatmap no **R** usando o pacote **ComplexHeatmap**.

## Introdução

Mosquitos são vetores de diversas doenças que afetam milhões de pessoas ao redor do mundo. Compreender os fatores que influenciam a transmissão de arbovírus é essencial para o desenvolvimento de estratégias de controle. Um desses fatores é a presença da bactéria **Wolbachia pipientis**, um endossimbionte que pode inibir significativamente a infecção e transmissão de vírus pelo *Aedes aegypti*.

Nesta aula, exploraremos como detectar e quantificar **Wolbachia** em bibliotecas de pequenos RNAs (*small RNA-seq*), aplicando técnicas de bioinformática para analisar dados reais. Vocês realizarão passos semelhantes aos de uma pesquisa que detectou sequências de **Wolbachia** em bibliotecas de pequenos RNAs, utilizando mosquitos infectados e não infectados.

## Fundamentação Teórica

### Bibliotecas de Pequenos RNAs

Bibliotecas de pequenos RNAs são coleções de sequências de RNA não codificante, geralmente de nucleotídeos de comprimento até 50. Esses pequenos RNAs desempenham papéis cruciais na regulação gênica, defesa antiviral e silenciamento de transposons. Exemplos incluem microRNAs (miRNAs), small interfering RNAs (siRNAs) e piwi-interacting RNAs (piRNAs).

### Wolbachia e Pequenos RNAs

**Wolbachia pipientis** é uma bactéria endossimbionte que infecta uma ampla gama de artrópodes. No contexto dos mosquitos, a infecção por **Wolbachia** pode interferir na replicação de vírus, tornando-a uma ferramenta potencial para o controle de doenças.

No entanto, ao contrário de vírus, não há descrição na literatura de uma via específica de pequenos RNAs derivada de **Wolbachia**. Portanto, a hipótese é que os pequenos RNAs correspondentes a **Wolbachia** detectados nas bibliotecas são provenientes de **degradação enzimática aleatória** do material genético bacteriano, e não de um processo biológico regulado.

## Fluxo de Trabalho

1. **Download das Sequências do SRA**
2. **Limpeza e Qualidade das Sequências**
3. **Alinhamento com a Referência de Wolbachia**
4. **Análise e Visualização dos Dados no R**

Neste exercício, vamos percorrer o processo de detecção de **Wolbachia** em bibliotecas de pequenos RNAs, usando sequências específicas de referência e ferramentas bioinformáticas. Vocês seguirão o fluxo completo para download, limpeza, alinhamento e análise, entendendo cada etapa e os códigos usados para executá-las.

### Passo 1: Download de Dados do SRA

Primeiramente, baixamos as bibliotecas de pequenos RNAs do **SRA** (Sequence Read Archive), um repositório público de dados de sequenciamento. Para isso, usamos o comando `fasterq-dump`, que gera arquivos `.fastq`, formatados para conter as sequências de RNA e informações sobre a qualidade da leitura.

```bash
fasterq-dump SRRXXXXXXX
```

### Passo 2: Avaliação de Qualidade das Leituras (Score Phred)

Os **scores Phred** representam a qualidade de cada base sequenciada, indicando a probabilidade de erro. Antes de prosseguir, rodamos o **FastQC** para avaliar essa qualidade. O **Trim Galore** será usado em seguida para limpar as sequências com baixa qualidade.

* Um Phred score de 30 significa uma acurácia de 99,9% (1 erro em 1.000 bases).

```bash
fastqc SRRXXXXXXX.fastq
```

### Passo 3: Limpeza e Corte de Sequências de Baixa Qualidade

Para garantir a qualidade dos dados, usamos **Trim Galore** com um corte mínimo de qualidade (Phred 25). Isso remove sequências de baixa confiança e garante que só passem adiante as que atendem ao padrão de qualidade estabelecido.

```bash
trim_galore --fastqc -q 25 --trim-n --max_n 0 -j 1 --length 18 --dont_gzip SRRXXXXXXX.fastq
```

### Passo 4: Preparação das Referências de Wolbachia

Baixamos as sequências de referência das cepas de **Wolbachia** do banco de dados NCBI. Essas referências serão usadas no alinhamento para identificar a presença de Wolbachia nas amostras.

Cepas utilizadas:

- **wAlbB** (CP031221.1)
- **wMel** (CP046925.1, CP072672.1)
- **wMelPop** (CP046921.1)
- **wCle** (AP013028.1)

Exemplo de comando para download de uma sequência de referência:

```bash
efetch -db nucleotide -id CP031221.1 -format fasta > wAlbB_CP031221.1.fasta
```

### Passo 5: Construção do Índice de Alinhamento com Bowtie

Utilizamos **Bowtie** para construir um índice a partir das referências de **Wolbachia**. Esse índice é essencial para o alinhamento, pois permite um processo mais rápido e eficiente.

```bash
bowtie-build reference.fasta reference_index
```

### Passo 6: Alinhamento das Sequências

Com as bibliotecas preparadas e o índice criado, realizamos o alinhamento dos dados limpos (agora em formato `.fasta`) contra a referência de **Wolbachia**. Neste caso, usamos parâmetros que garantem alta precisão no alinhamento (parâmetros `-f -S -a -v 0`).

```bash
bowtie -f -S -a -v 0 -p 3 -t reference_index infected_SRR12893566.fasta > infected_SRR12893566.sam 2> infected_SRR12893566_bowtie.log
```

### Passo 7: Filtragem de Alinhamentos

Após o alinhamento, usamos **samtools** para filtrar apenas as leituras que se alinharam com sucesso. Isso reduz o arquivo `.sam` para conter apenas as sequências mapeadas.

```bash
samtools view -S -h -F 4 infected_SRR12893566.sam > infected_SRR12893566_onlymapped.sam
```

### Passo 8: Contagem das Leituras Alinhadas

Para entender quais cepas foram detectadas, usamos **awk** para contar as sequências mapeadas e identificar as referências correspondentes. Esse passo é útil para quantificar a presença de **Wolbachia** nas amostras.

```bash
awk '$1 !~ /^@/ {print $3}' infected_SRR12893566_onlymapped.sam | sort | uniq -c
```

### Passo 9: Visualização em Heatmap

Com os dados processados, importaremos os resultados para o **R** e usaremos a biblioteca **ComplexHeatmap** para criar um heatmap que mostrará a presença e quantidade de **Wolbachia** nas amostras analisadas. Esta visualização permitirá uma análise comparativa entre amostras infectadas e não infectadas. (proximas aulas)