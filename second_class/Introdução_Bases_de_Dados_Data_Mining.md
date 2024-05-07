# Introdução a Bases de Dados e Data Mining

### **Analogia com o MasterChef na Bioinformática**

- **Seleção de Ingredientes**: Assim como escolher os melhores ingredientes na cozinha, selecionamos dados relevantes do banco de dados NCBI usando scripts e ferramentas como o Entrez e o SRAtoolkit.
- **Limpeza dos Ingredientes**: Equivalente a limpar e preparar alimentos, utilizamos algoritmos como Trim Galore e FASTQC para “purificar” e verificar a qualidade das sequências de DNA.
- **Cozinhar o Prato**: Com os dados prontos, aplicamos algoritmos de bioinformática, nesse caso o Bowtie, para alinhamento de sequências, transformando os dados brutos em informação útil.
- **Apresentação do Prato**: Na etapa final, utilizamos ferramentas de ciência de dados e software como R e Python para criar visualizações e relatórios, tornando os resultados não apenas úteis, mas também visualmente atraentes.

### **Seleção de Dados**

- **Escolha de ingredientes (Dados)**: Identificar e extrair as sequências de DNA e RNA necessárias de bancos de dados públicos, como o NCBI.
- **Ferramentas e métodos**: Uso de scripts Python e a API do Entrez para automatizar a coleta de dados de sequencia de nucleotídeos e o .

### **Limpeza de Dados**

- **Preparação dos ingredientes (Dados)**: Limpeza e filtragem inicial para garantir a qualidade dos dados.
- **Ferramentas envolvidas**: Utilização de Trim Galore e FASTQC para cortar sequências de baixa qualidade e verificar a saúde dos dados.

### **Implementação Prática**

- **Configuração do ambiente**: Estabelecimento de um ambiente de trabalho usando Conda, que suporta as ferramentas necessárias.
- **Execução de scripts**: Aplicação prática dos conceitos teóricos através da execução de scripts para extração e limpeza de dados.

### **Resultados Esperados**

- **Dados prontos para análise**: Ao final da aula, os alunos terão um conjunto de dados limpo e de alta qualidade, pronto para ser 'cozinhado' em análises de bioinformática mais complexas nas próximas aulas.

### Links externos

- [https://youtu.be/XiWcXUS15fI?si=qcCNStmSlIsthRHo](https://youtu.be/XiWcXUS15fI?si=qcCNStmSlIsthRHo) (Como rodar Entrez no python)
- [https://youtu.be/goXvL9fW-1Q?si=DsGzg_2VdxYWYEiv](https://youtu.be/goXvL9fW-1Q?si=DsGzg_2VdxYWYEiv) / [https://youtu.be/Poh1gzcR2tQ?si=_HiY0CEUeY8hqOmy](https://youtu.be/Poh1gzcR2tQ?si=_HiY0CEUeY8hqOmy) (SRAtoolkit)
- [https://youtu.be/Lvi48eEQdHE?si=xewLIdjesZCfIJNK](https://youtu.be/Lvi48eEQdHE?si=xewLIdjesZCfIJNK) (NCBI browser)
- [A Search Example in Five Steps - Finding Sequence Using NCBI Nucleotide - Welch Medical Library Guides at Johns Hopkins University-Welch Medical Library (jhmi.edu)](https://browse.welch.jhmi.edu/finding-sequence-using-nucleotide/search-example) (como operar buscas no nucleotide NCBI) - Importante

### Tabela de bibliotecas que serão extraídas do SRA e accession das cepas de wolbachia que serão alinhadas contra as bibliotecas

| https://www.ncbi.nlm.nih.gov/Traces/study/?acc=SRP288520&o=acc_s%3Aa# | https://www.ncbi.nlm.nih.gov/Traces/study/?acc=SRP288520&o=acc_s%3Aa# | tissue |
| --- | --- | --- |
| https://trace.ncbi.nlm.nih.gov/Traces/sra?run=SRR12893565 | wAlbB_infected-12days-R3 | Wolbachia infected |
| https://trace.ncbi.nlm.nih.gov/Traces/sra?run=SRR12893566 | wAlbB_infected-12days-R2 | Wolbachia infected |
| https://trace.ncbi.nlm.nih.gov/Traces/sra?run=SRR12893568 | wAlbB_infected-6days-R3 | Wolbachia infected |
| https://trace.ncbi.nlm.nih.gov/Traces/sra?run=SRR12893569 | wAlbB_infected-6days-R2 | Wolbachia infected |
| https://trace.ncbi.nlm.nih.gov/Traces/sra?run=SRR12893572 | uninfected-12days-R3 | uninfected |
| https://trace.ncbi.nlm.nih.gov/Traces/sra?run=SRR12893573 | uninfected-12days-R2 | uninfected |
| https://trace.ncbi.nlm.nih.gov/Traces/sra?run=SRR12893575 | uninfected-6days-R3 | uninfected |
| https://trace.ncbi.nlm.nih.gov/Traces/sra?run=SRR12893576 | uninfected-6days-R2 | uninfected |

accession_numbers = ["CP031221.1", "CP046925.1", "CP072672.1", "CP046921.1", "AP013028.1"]