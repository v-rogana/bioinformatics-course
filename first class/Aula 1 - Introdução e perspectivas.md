# Aula 1 - Introdução e perspectivas

### **Boas-vindas e Perspectiva sobre Bioinformática**

**Introdução à Bioinformática:**

- Área em rápida expansão na biologia, com alta demanda por profissionais qualificados.
- Adaptação às novas tecnologias é essencial, mesmo para biólogos que não focam em algoritmos e códigos complexos.
- Uso inevitável de ferramentas de bioinformática como extração de bancos de dados e ciência de dados para visualização de dados.
- Minha historia na area de bioinformática e como a area te da controle sobre seu proprio destino (tudo esta entre voce e o teclado).

**Campo de Atuação Amplo:**

- **Metagenômica na Medicina de Precisão**: Identificação de patógenos para tratamentos na medicina de precisão, com algoritmos como kraken + redução de falso positivos (cut offs) + visualização dos dados.
- **Análise de Proteínas por Docking Molecular**: Identificação de ligantes efetivos para fármacos.
- **Vigilância Genômica e Filogenômica**: Identificação de cepas e rastreamento de doenças.
[Nextstrain / ncov / open / global / 6m](https://nextstrain.org/ncov/open/global/6m)
- **Modelagem Ecológica de Nichos e Serviços Ecológicos**: Estudo de ecossistemas e sua dinâmica.

**Perspectiva do Curso - Enfoque em "Universais" da Bioinformática:**

- O curso não se concentrará em algoritmos específicos ou em como extrair datasets de bases de dados.
- Em vez disso, o foco será em ensinar conceitos fundamentais e habilidades versáteis que são aplicáveis em diversas situações na bioinformática.

**Quatro Passos Fundamentais:**

- Entendimento abstrato de que a maioria das tarefas em bioinformática pode ser resumida em quatro etapas principais:
    1. **Extração e Tratamento de Dados**: Coleta e preparação de dados para análise.
    2. **Algoritmos em Linha de Comando**: Uso de ferramentas de linha de comando para aplicar algoritmos de bioinformática.
    3. **Comandos e Scripts no Linux**: Manipulação de dados e automação de tarefas usando o sistema operacional Linux.
    4. **Ciência de Dados em R/Python**: Análise e visualização de dados biológicos usando as linguagens de programação R e Python.

**Organização e Armazenamento de Informações:**

- Aprendizado sobre como armazenar informações e se organizar usando ferramentas como Notion e Google Drive, evitando problemas comuns de desorganização e perdas de dados (exemplo do caso de Carlos).

**Uso do ChatGPT em Tarefas Bioinformáticas:**

- Incorporação do ChatGPT em diversas atividades, incluindo:
    - Realização de verificações de sanidade (sanity checks) com comandos no Linux.
    - Desenvolvimento de scripts e códigos.
    - Plotagem de dados e melhoria da visualização de dados.
    - Aprimoramento da escrita científica.

**Introdução à Computação em Nuvem:**

- Introdução básica à computação em nuvem, uma área ainda pouco explorada em muitos laboratórios de bioinformática.
- Oportunidade para aprender conceitos computacionais importantes, como memória RAM, armazenamento em HD/SSD e servidores (exemplificado pelo caso de André, que utilizou esses conhecimentos para transição para a área privada).

**Oportunidades e Competitividade:**

- Exemplo do André: Uso de computação em nuvem AWS para transição para a área privada.
- Competição com profissionais com no máximo 15 anos de experiência em bioinformática.
- Vantagem competitiva devido ao rápido avanço das ferramentas e tecnologias, que podem deixar profissionais mais experientes defasados. (temos que tornar os idosos defasados)

### **O que é Bioinformática**

**Definição e Origem do Termo:**

- A palavra "informática" vem do francês "informacion" + "automatique", que significa automação da informação. *pegar com alan texto deleuse/gatarriu e video
- A bioinformática é a arte de modelar fluxos informacionais biológicos de forma computacional.

**Modelagem de Fluxos Informacionais Biológicos:**

- Na biologia, quase tudo pode ser visto como um fluxo informacional. Por exemplo:
    - **Sequências de DNA**: Podem ser representadas e analisadas como cadeias de caracteres em um computador.
    - **Redes de Interação Proteica**: Podem ser modeladas como grafos, onde os nós representam proteínas e as arestas representam interações entre elas.
    - **Vias Metabólicas**: Podem ser entendidas como algoritmos que transformam substratos em produtos.

**Exemplo Didático - Fluxos informacionais na biologia:**

### **Introdução ao Curso e ao Tema de Pesquisa**

- **Contexto Global**:
    - Wolbachia pipientis é investigada como agente de controle biológico na inibição de arbovírus.
    - Vamos alinhar genomas de diferentes cepas de wolbachia em bibliotecas de RNA infectadas e não infectadas pelo endossimbionte, buscando detectar e quantificar a bacteria nessas bibliotecas de pequenos RNAs.
    - conceitos a serem explicados: Metagenomica, alinhamento de sequencias e porque bibliotecas de small RNAs (coisa do meu laboratorio).
- **Objetivo**:
    - De modo geral, isso eh uma tarefa muito especifica e pouco pratica, apesar disso, ela eh simples e permite conhecer todos os passos básicos que envolvem a bioinfo, portanto o foco nem eh exatamente na tarefa, mas nos conceitos que serao aplicados e ensinados.

### **Aula 1: Extração de Dados e Bancos de Dados Públicos**

- Como extrair datasets de bancos de dados publicos
- O que são os dados que vamos trabalhar e como eles serao abordados.
- Como usar o Notion para anotar e organizar tudo que sera feito nas outras tarefas
- Chatgpt para ajudar a extrair dados

### **Aula 2: Alinhamento de Sequências e Uso de Algoritmos**

- Como ler uma documentacao, parametrizacao e linhas de comando
- Como usar o chatgpt + enviroment conda para realizar seus projetos
- Como conectar numa servidora e como movimentar entre pastas
- Entender o fluxo input → algoritmo
- Sanity checks

### **Aula 3: Processamento e Tratamento de Dados em linux**

- Entender o que significa os outputs
- Tratar os dados e gerar tabelas para enviar para o R
- Usar o chat gpt para ajudar a montar scripts e codigos

### **Módulo 4: Data analysis and visualization**

- Plotar no R/python usando chatgpt pra facilitar.
- Abstrair o que voce pretende apresentar e melhores formas de apresentar o grafico

**Desmistificando a Bioinformática:**

- Muitos bioinformatas inserem dados de entrada (input) em uma linha de comando, recebem resultados (output) e consideram isso como algo mágico. No entanto, é crucial entender o processo por trás disso:
    - **Input**: Contém todos os tipos de informações, como sequências de DNA, dados de expressão gênica, estruturas de proteínas, etc.
    - **Algoritmo**: Processa essas informações seguindo regras específicas, realizando cálculos e análises.
    - **Output**: Resultado gerado pelo algoritmo, que pode ser uma sequência alinhada, uma árvore filogenética, um modelo de proteína, etc.
- Entender esse processo é especialmente importante para a resolução de problemas (troubleshooting), pois muitos erros ocorrem devido à má compreensão dos dados de entrada ou do funcionamento dos algoritmos.

**Recapitulação**

- O grupo de estudos tem como objetivo incorporar softwares e tecnologias para o mundo da bioinformática, visando reduzir os “gaps” na formação do bioinformata que geralmente ocorre pelo fato de a ciência ser feita nas coxas e o ensino ser transmitido de forma vertical (pos-graduação ensinando graduandos no ambiente laboratorial).
- Forcar em “Universais” dentro da bioinfo, ensinando como usar ferramentas essenciais para a bioinformatica e para a informatica, dando estrutura e conhecimento para entrar em qualquer laboratorio (skillset amplo que demonstre conhecimento global da bioinfo) e ao mesmo tempo permitindo a vasão para outras áreas da informatica (como computação em núvem)
- Entender os fluxos informacionais num nível biológico e entender como isso eh matematicamente/computacionalmente modelado nos datasets e como eles sao tratados pelos algoritmos, compreendendo a area em um nivel abstrato e aprofundado.