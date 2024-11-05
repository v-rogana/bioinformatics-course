# Introdução ao Linux para Bioinformática

### Introdução básica ao linux (linha de comando)

Disco Rigido (HD/CPU/RAM) → Linux Kernel → Distribuição, por exemplo ubunto, é onde você interage com o linux pela sua maquina 

### Por que Linux?

- É mais rápido e eficiente.
- Possui maiores garantias de segurança.
- É um sistema operacional de código aberto (open source).

### Entendendo o Linux comparado ao Windows:

1. Identificar a localização atual: Use o comando `pwd` para exibir o diretório atual em que você se encontra.
2. Explorar arquivos graficamente: Se for possivel, abra o explorador de arquivos e compare visualmente com o que faremos na linha de comando (`explorer.exe .`)
3. Listar arquivos: Diferente do explorador de arquivos, na linha de comando não aparece nada, para resolver isso, use o comando `ls`, agora você pode ver todos os arquivos e subdiretórios dentro do diretório atual.
4. Criar um novo diretório: Utilize o comando `mkdir <nome_do_diretorio>` para criar um novo diretório. (make directory = mkdir)
5. Navegar entre diretórios: Mova-se para o diretório criado com `cd <nome_do_diretorio>`, e faça o mesmo no explorador de arquivos para visualizar a mudança. (change directory = cd)
6. Verificar a localização novamente: Confirme sua localização com o comando `pwd`.
7. Retornar ao diretório anterior: Use `cd ..` para voltar ao diretório anterior e repita o processo no explorador de arquivos.
8. Navegar até a raiz: Continue usando `cd ..` até chegar ao diretório raiz (`root`).
9. Explorar o diretório raiz: No diretório raiz, execute `ls` para listar diretórios importantes como `boot`, que contém arquivos essenciais para a inicialização do sistema.
10. Explorar binários do sistema: Entre no diretório `/bin` com `cd bin` e localize o arquivo `ls` com o comando `ls | grep ls`.
11. Visualizar conteúdo de um comando: Utilize `cat ls` para ver o código ou o script que executa o comando `ls`.
12. Volte para o diretorio criado `mkdir <nome_do_diretorio>` e crie o arquivo de texto `nano sequencias.fasta` 
13. **Abrir o editor Vim**: No terminal, digite `vim sequencias.fasta` para criar ou editar o arquivo `sequencias.fasta`.
- **Entrar no modo de inserção**: Pressione a tecla `i` para entrar no modo de inserção e começar a editar o texto no Vim.
- **Inserir sequências**: Digite as sequências desejadas no formato FASTA dentro do arquivo.
- **Salvar e sair do Vim**: Pressione `Esc` para sair do modo de inserção, digite `:wq` e pressione `Enter` para salvar as alterações e fechar o editor.
- insira algumas seqs

```bash
>seq1
ATGAACTTAACTAGCCCTAAGGTTTCTACTATGTTTTTTGATCAGATTATTACAGTAACGGATCACAATG
TTACTTGGGAAAAAATCCAAAATTGTCTTTATAATCTTTATGGAGAAGCAACATATAATAGCTGGCTGAG
TTCACTAAAATTTGTCAGTAGCAGAAATGGGGAAGTTCTTTTATCCGTGTCAACAAGGTTTATAAAAGAG
>seq2
ATATGGTGGAGTGGGACTTGGTAAAACACATCTAATGCATGCTATAGCTTGGCACATAGTCAATTCCCC
ATCAGCAAAAAGAAAAGTGGTGTATTTATCAGCAGAGAAATTCATGTACCAATATATTACAGCGCTGCGA
AGCAAAGATATTATGTTATTTAAAGAGCAATTTAGATCAGTAGATGTATTGATGGTAGATGATGTGCAAT
TTATCAGTGGTAAAGATAGTACACAAGAAGAATTTTTTCACACTTTCAATGCATTGATAGACCAAAATAA
ACAATTGGTTATATCAGCTGATAGGTCTCCTAGTGATCTTGATGGAGTGGAAGAAAGAATAAAATCACGA
CTCGGTTGGGGGTTGGTGGCAGATATTAATGAAACAACTT
>seq3
ATGTATTTTGCCAAAAAATTTACGCAAAAAAGCCTACCAGATATTGGG
AGAAACTTTGGTGGCAGAGACCATGCTACTGTTATACATGCAGTAAAACAAGT
```

### Exercicio Explorando o comando `grep` em Bioinformática

- Com o `man grep` ou `grep --help` para ler o manual do comando grep e descubrir como contar o numero de sequencias usando o comando, use ao seu favor o chatbot monitor para te ajudar na tarefa.
- Use o blast online para analisar as sequencias do arquivo em análise.
- Altere algumas letras e alinhe novamente no blast (analisar alterações no alinhamento).
    - Inserção de uma Base de nucleotídeos.
    - Deleção de uma Base de nucleotídeos.
    - Substituição de uma Base de nucleotídeos.
- Diferenciar Alinhamento Global/Local (não entrar em muitos detalhes).

### Pausa para Revisão de conceitos Base de Biologia Molecular

DeoxyriboNucleic Acid (DNA)

- O primeiro D (desoxirribose), é um açúcar monossacarídeo que é a estrutura do DNA (backbone).
- O ácido vem do fosfato, mas na verdade a magia acontece nas bases (Adenina-Timina/Citosina=Guanina).
- Esse pareamento de bases em uma ordem especifica, se torna um código que pode ser replicado com apenas um lado da dupla helice (RNA).
- Isso permite que o DNA mantenha uma sequencia informacional e conformacional, tanto na **Replicação** quanto na **Transcrição.**

Complete o pareamento a seguir:

> ACTGAATC
TGACTTAG
> 

Com esse raciocínio, entendemos a **replicação e transcrição**. Agora, a tradução é um processo mediado por Ribosomos (fig3)

- Entenda a diferença entre os nomes transcrição e tradução, um se refere a mesma gramática (transcrição), o outro se refere a uma transcrição em outra gramática (tradução).
- Agora olhe a figura inicial do blast e entenda os tipos.

![image.png](image.png)

![image.png](image%201.png)

![image.png](image%202.png)

### E a Informática? (o que nos interessa aqui)

Como já foi dito previamente, a computação só entende arquivos binários. Para isso vamos fazer uma brincadeira de modelar computacionalmente a sequencia do exemplo de pareamento que acabamos de fazer ACTGAATC.

No exemplo didático, vamos fazer o seguinte:

- Adenina = 00
- Timina = 01
- Citosina = 10
- Guanina = 11

> ACTGAATC
TGACTTAG
> 

> 00 10 01 11 00 00 01 10
01 11
> 

Agora que modelamos computacionalmente o problema, você consegue entender o que é um pareamento de nucleotídeos para um computador? Pra ser mais exato, ele nem entende binario, ele entende 0 = 0 voltz / 1 = 0.5 voltz mas ai acho que já é abstração de mais :)

### Com esses princípios vamos entender um modelo de RNAseq

Para entender essa ideia vamos manter os exemplos didáticos. Imagine o Genoma como sendo um grande livro de arquitetura, cada capitulo descreve como construir uma parte da casa (gene/DNA). Nesse mundo, um engenheiro (trasncrito/RNA), só consegue ler 1 capítulo e construir uma singular unidade de parte da casa. Depois de um dia, o engenheiro morto desaparece (degradação). 

- Capítulo 1 = Construir uma Janela (ATGTA…)
- Capítulo 2 = Construir uma Porta (TAGGTC…)
- Capítulo 3 = Construir uma Pilastra (GGACT…)

Vamos imaginar que você esta analisando duas obras diferentes, uma do lado da minha casa, no morro das pedras e outra no Alphaville (condições distintas do organismo). Se você contasse o numero de engenheiros mortos em cada obra, perceberia que existe uma diferença estatística entre o número de mortos no morro das pedras (menor) e no Alphaville (maior), além disso emerge um padrão no número de transcritos entre as obras da mesma região.

### Continuando a aprender linux

1. Movendo e Renomeando Arquivos com `mv` 
- Renomeie o arquivo de backup `mv sequencias.fasta sequencias_wolbachia.fasta`
- Faremos isso porque agora sabemos que essas sequencias são de Wolbachia.
1. Crie um arquivo temporario e remova ele
- `touch temporario.txt` → `cp temporario.txt`→ `rm temporario.txt`
1. Exibindo Conteúdo de Arquivos com `cat`: `cat sequencias_wolbachia.fasta` 
2. **Exercício:** 
- Crie um diretório chamado `mkdir lixo`
- Entre nele com `cd`
- Use o `pwd` para se situar/entender no path (caminho) da sua maquina
- Copie o arquivo `sequencias_wolbachia.fasta` do diretório anterior no diretorio atual
    - Diretorio anterior = `..`   /   Diretorio atual = `.`
- Use o `cat` para visualizar o conteúdo copiado em `sequencias_wolbachia.fasta`.
- Remova o arquivo `sequencias_wolbachia.fasta` com `rm`
- Volte para pasta anterio com `cd ..`
- Remova a pasta `lixo` com `rm`
1. Criando um Script Básico em Bash:
- Crie um novo arquivo de script: `nano contar_sequencias.sh` e insira o seguinte codigo
- o $1 indica uma variável que sera enviada por linha de comando no seu código

```bash
#!/bin/bash
grep -c ">" $1
```

1. Saia do nano e torne o script executável:

```bash
chmod +x contar_sequencias.sh
# Execute o script passando o arquivo sequencias.fasta como parâmetro:
./contar_sequencias.sh sequencias_wolbachia.fasta
```