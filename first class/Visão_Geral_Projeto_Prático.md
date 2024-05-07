## Detecção e Quantificação de Carga de Wolbachia em Bibliotecas de small RNAs

Em nosso experimento controle, para verificar a funcionalidade do
método, analisamos 18 bibliotecas de RNA pequeno de mosquitos Aedes
aegypti do laboratório A, comparando-as com dados de Ae. aegypti
disponíveis publicamente. Esses dados de controle incluíam dois grupos:
um tratado com tetraciclina para remover a infecção por Wolbachia e o
outro não tratado e infectado pela bactéria.

Indexamos 67 genomas completos de cepas de Wolbachia do Refseq para
alinhamento. O mapa de calor abaixo mostra dois clusters distintos:
grupos infectados e não infectados. Notavelmente, apenas as 9
bibliotecas de mosquitos não tratados exibiram alinhamento significativo
de pequenos RNAs de Wolbachia, específicos para a cepa wAlb usada na
infecção.

    # Data treatment to make the Heatmap
    library(dplyr)
    library(ggplot2)
    library(reshape2)
    library(ComplexHeatmap)
    library(circlize)
    library(viridis)
    library(readxl)

    # Load Data
    counts_av0 <- read.delim("C:\\Users\\victo\\Desktop\\pasta_compartilhada\\reads_perwolb_controle_aedes_a_v0.tab", header = FALSE)
    Aegypti_controle_a_v0 <- read.csv("C:\\Users\\victo\\Desktop\\pasta_compartilhada\\Aegypti_controle_a_v0.csv")
    wolbs_reduced <- read_xlsx("C:\\Users\\victo\\Downloads\\wolbs_reduced_2.xlsx")

    # Data Preparation
    colnames(counts_av0) <- c("V1", "V2", "V3")
    counts_av0 <- left_join(counts_av0, select(Aegypti_controle_a_v0, Library, total_reads_trimmed, tratamento_tetraciclina), by = c("V3" = "Library"))
    counts_av0$rpm <- counts_av0$V1 / counts_av0$total_reads_trimmed * 1e6
    counts_av0$corrected <- sub("NZ_", "", counts_av0$V2)
    counts_av0 <- left_join(counts_av0, select(wolbs_reduced, AccessionVersion,Title_reduced), by = c("corrected"="AccessionVersion"))

    # Prepare data for heatmap
    d_counts_av0 <- counts_av0 %>% dcast(V3 + tratamento_tetraciclina ~ corrected + Title_reduced, value.var = "rpm")
    d_counts_av0[is.na(d_counts_av0)] <- 0
    d_counts_av0_matrix <- as.matrix(d_counts_av0[, c(-1, -2)])
    row.names(d_counts_av0_matrix) <- paste0(d_counts_av0[, 1], "_", d_counts_av0[, 2])

    # Heatmap code
    Heatmap(d_counts_av0_matrix, 
                heatmap_legend_param = list(color_bar = "continuous",
                                            at = c(0, round(max(d_counts_av0_matrix), 1) * 0.25, round(max(d_counts_av0_matrix), 1) * 0.5, round(max(d_counts_av0_matrix), 1) * 0.75, round(max(d_counts_av0_matrix), 1)),
                                            title_gp = gpar(fontsize = 5, fontface = "bold"),
                                            labels_gp = gpar(fontsize = 5)),
                name = "RPKM", 
                row_labels = row.names(d_counts_av0_matrix), 
                row_title = "Libraries", 
                row_title_gp = gpar(fontsize = 12, fontface = "bold"),
                row_dend_side = "left", 
                row_names_side = "left",
                row_names_gp = gpar(fontsize = 7),
                column_labels = colnames(d_counts_av0_matrix),
                column_title = "Viral Contigs",
                column_title_gp = gpar(fontsize = 10, fontface = "bold"),
                column_names_gp = gpar(fontsize = 4.5),
                clustering_distance_rows = "manhattan", # default is euclidean
                clustering_method_rows = "ward.D", # default is complete
                clustering_distance_col = "euclidean", # default is euclidean
                clustering_method_col = "complete", # default is complete,
                col = viridis(100)
    )

![Heatmap Plot Output Final](/images/Heatmap_Controle_Aegypti_av0.png)

## Projeto Prático que Realizaremos na Aula

Durante nossas próximas aulas práticas, vamos explorar a análise de 8
bibliotecas de pequenos RNAs originadas do mesmo estudo. Nosso objetivo
será alinhar essas bibliotecas contra 5 cepas de Wolbachia,
identificadas pelos números de acesso “CP031221.1”, “CP046925.1”,
“CP072672.1”, “CP046921.1” e “AP013028.1”. Esta atividade é projetada
para atingir resultados comparáveis aos de estudos mais amplos, porém
com um custo computacional significativamente menor. Mais do que apenas
executar uma tarefa, o foco da nossa aula será no desenvolvimento de
habilidades cruciais na área de bioinformática.

## Habilidades e softwares que serão trabalhados

Utilizaremos uma variedade de ferramentas e plataformas para garantir
que cada participante ganhe experiência prática e compreensão teórica
dos processos biológicos e computacionais.

Ferramentas e Softwares que Utilizaremos: Conda: Utilizaremos o Conda
para criar ambientes isolados que facilitam o gerenciamento de pacotes e
dependências necessárias para nossos projetos. Algoritmos e Linhas de
Comando: Aprenderemos a executar algoritmos bioinformáticos essenciais
através de linhas de comando, uma habilidade crucial para a análise de
dados genéticos. Scripts e Automatização: Desenvolveremos scripts para
automatizar processos repetitivos, aumentando a eficiência e a
capacidade de replicar análises com precisão. Notion: Utilizaremos o
Notion para organizar tarefas e manter a documentação do curso
atualizada, o que facilita o acompanhamento do progresso e a revisão dos
conteúdos. Git/GitHub: Empregaremos Git e GitHub para colaboração em
projetos. Isso não só facilita o trabalho em equipe como também mantém
um histórico detalhado de todas as alterações e versões anteriores de
nossos projetos. AWS: Introduziremos conceitos básicos de computação na
nuvem usando a AWS, incluindo o uso de servidores, alocação de recursos
computacionais (vCPUs e memória RAM) e opções de armazenamento.
