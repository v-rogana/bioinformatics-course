# Práticas em Git/Github

Os comandos do Git são fundamentais para o controle de versões e colaboração em projetos de desenvolvimento de software. Aqui está um guia básico sobre como usar os principais comandos do Git em um fluxo de trabalho que inclui criação de branches, merge e pull.

### **Workflow Básico**

1. **Realize alterações nos seus arquivos,** adicionando ao ambiente intermediário stagging (`git add`).
2. **Faça commits regularmente**, mantendo um histórico claro e mensagens significativas sobre as alterações.
3. **Mantenha sua branch atualizada** com a principal (**`main`**) fazendo regularmente **`git pull / git push`**.
4. obs: Se voce estiver trabalhando em grupo, são boas práticas, criar uma nova branch como “develop” e fazer as alterações nela → dar `git pull` na branch principal antes do `git merge` → dar um merge entre develop/main → resolver conflitos (se precisar) → enviar para o github com `git push` .

### Crie sua Conta no Github

1. Acesse https://github.com/
2. Crie a conta e verifique com o código no email.
3. Crie um repositório com o nome: `Praticas_Alinhamento_Sequencias_2024` (Tem que ser esse nome!).
4. Descrição:
O objetivo desse repositorio é armazenar os codigos utilizados para detectar sequências de Wolbachia em bibliotecas de pequenos RNAs, desde o download das sequências e alinhamento com Bowtie até a visualização dos dados em um heatmap no R usando o pacote ComplexHeatmap.
5. O repositório deve ser público.
6. Dê check no Add README file.
7. Crie o Repositório :)

[Caso esteja fazendo em casa](https://www.notion.so/Caso-esteja-fazendo-em-casa-17b5e115016d80ae9887c5ee74c27d05?pvs=21)

### Crie um Token de Acesso Pessoal

1. Clique no **ícone do seu perfil** no canto superior direito.
2. Selecione **"Settings"** (Configurações).
3. No menu lateral esquerdo, role para baixo até encontrar **"Developer settings"** (Configurações de desenvolvedor) e clique.
4. Selecione **"Personal access tokens"** (Tokens de acesso pessoal).
5. Clique em **"Tokens (classic)"**
6. Clique em **"Generate new token"** (Gerar novo token).
7. **Nota (Note):** Dê um nome descritivo ao seu token, como "Token para aula".
8. **Scopes (Escopos):** Marque as permissões necessárias. Para operações de push, selecione:
    - **"repo"**: Acesso total a repositórios privados e públicos.
9. Clique em **"Generate token"** (Gerar token).
10. **Importante:** Copie o token gerado e guarde-o em um local seguro. Você não poderá visualizá-lo novamente após sair da página.
11. Usar o token na URL remota

```bash
git remote set-url origin https://seu_token@github.com/usuario/Praticas_Alinhamento_Sequencias_2024
```

# Exercício Avaliativo: Criando seu Repositório da Disciplina no GitHub

## Introdução

Neste exercício, você irá criar um repositório no GitHub para armazenar os códigos e documentos das aulas práticas da disciplina. Utilizaremos como base o **Guia Teórico e Prático de Detecção de Wolbachia em Bibliotecas de Pequenos RNAs**. O objetivo é familiarizá-lo com o uso básico do Git e GitHub, aplicando boas práticas de versionamento e documentação.

## Objetivos

- **Criar um repositório pessoal** no GitHub para a disciplina.
- **Adicionar pelo menos 4 arquivos**:
    - Um **README.md** explicando o projeto e as atividades práticas.
    - Um arquivo com os **códigos de limpeza de dados**.
    - Um arquivo com os **códigos de alinhamento usando o Bowtie**.
    - Um arquivo com os **códigos utilizados no R.** (exemplo: Rmarkdown)
- **Aplicar boas práticas básicas** no uso do Git e GitHub.
- **Opcional:** Adicionar outros códigos, exemplos ou documentos para obter até **10 pontos adicionais**.

## Instruções Passo a Passo

### **Passo 1: Clonar o Repositório Criado para o Seu Computador**

1. No seu repositório recém-criado, copie a URL HTTPS da pagina.
2. Abra o terminal Linux.
3. Navegue até o diretório onde deseja salvar o repositório.
    
    ```bash
    cd /caminho/para/sua/pasta
    ```
    
4. Clone o repositório:

Substitua `seu-usuario` pelo seu nome de usuário no GitHub.
    
    ```bash
    git clone https://github.com/seu-usuario/Praticas_Alinhamento_Sequencias_2024.git
    ```
    

### **Passo 2: Criar o README.md**

1. Navegue até a pasta do repositório clonado:
    
    ```bash
    cd Praticas_Alinhamento_Sequencias_2024 
    ```
    
2. Crie ou abra um arquivo chamado `README.md` usando um editor de texto. (linha de comando `nano README.md`)
3. No `README.md`, inclua:
    - **Título do projeto**: `Detecção de Wolbachia em Bibliotecas de Pequenos RNAs`.
    - **Introdução**: Resuma o objetivo do projeto e a importância da detecção de **Wolbachia**.
    - **Descrição das atividades práticas**: Explique brevemente as etapas que serão realizadas, baseando-se no guia fornecido.
4. Salve o arquivo.

### **Passo 3: Adicionar o Código de Limpeza de Dados**

1. Crie um arquivo chamado `limpeza_dados.sh`.
2. Copie os comandos de limpeza de dados do guia:
    
    ```bash
    # Avaliação de qualidade das leituras com FastQC
    fastqc SRRXXXXXXX.fastq
    
    # Limpeza e corte de sequências de baixa qualidade com Trim Galore
    trim_galore --fastqc -q 25 --trim-n --max_n 0 -j 1 --length 18 --dont_gzip SRRXXXXXXX.fastq
    
    ```
    
3. Adicione comentários explicando cada comando.
4. Lembre-se que usamos outros comandos, como o seqtk, volte nas aulas de limpeza de dados e melhore esse arquivo o quanto desejar :) 
5. Salve o arquivo.

### **Passo 4: Adicionar o Código de Alinhamento com Bowtie**

1. Crie um arquivo chamado `alinhamento_bowtie.sh`.
2. Insira os comandos relacionados ao alinhamento:
    
    ```bash
    # Construção do índice de alinhamento com Bowtie
    bowtie-build reference.fasta reference_index
    
    # Alinhamento das sequências contra a referência de Wolbachia
    bowtie -f -S -a -v 0 -p 3 -t reference_index sample.fasta > sample.sam 2> sample_bowtie.log
    
    ```
    
3. Inclua comentários explicando os parâmetros usados.
4. Melhore o quanto quiser o código do bowtie, utilizando forloops e outras tecnicas. Abuse de ferramentas como chatgpt para te ajudar na tarefa!
5. Salve o arquivo.

### Passo 5: Adicionar os **códigos utilizados no R**

1. Adicione o HTML estático da aula do R que fizemos
    
    [Aula_RandStatistics_Heatmap.html](Aula_RandStatistics_Heatmap.html)
    
2. Crie um arquivo chamado `Aula_RandStatistics_Heatmap.R`.
3. Apartir do html da aula, crie o seu proprio arquivo de código.

### **Passo 6: Fazer o Commit dos Arquivos**

1. Verifique o status dos arquivos:
    
    ```bash
    git status
    ```
    
2. Adicione os arquivos ao staging area:
    
    ```bash
    git add .
    ```
    
3. Faça o commit com uma mensagem descritiva:
    
    ```bash
    git commit -m "Adiciona README e scripts de limpeza, alinhamento e codigos no R"
    ```
    

### **Passo 7: Enviar as Alterações para o GitHub**

1. Envie os commits para o repositório remoto:
**Nota:** Se sua branch principal for `master`, use `git push origin master`.
    
    ```bash
    git push origin main
    ```
    

### **Passo 8: Verificar o Repositório no GitHub**

1. Acesse o repositório no GitHub pelo navegador.
2. Verifique se os arquivos estão presentes e corretamente formatados.

## Boas Práticas de GitHub

- **Mensagens de Commit Claras**: Descreva o que foi feito em cada commit.
- **Organização**: Utilize nomes de arquivos e pastas que sejam descritivos.
- **Comentários no Código**: Facilita a compreensão e manutenção.
- **Uso do .gitignore**: Para ignorar arquivos que não devem ser versionados (exemplo: arquivos temporários, dados brutos grandes).
- **Documentação**: Mantenha o `README.md` atualizado com informações relevantes.

## Pontos Adicionais (Opcional)

Para obter até **10 pontos extras**, você pode:

- **Adicionar outros códigos** utilizados na disciplina.
- **Incluir exemplos de resultados** ou saídas dos scripts.
- **Melhorar os códigos,** transformando em scripts, for loops e códigos automatizados (o céu é o limite).
- **Criar documentação adicional**, detalhando cada etapa do processo.
- **Adicionar gráficos ou figuras** resultantes das análises.

## Entrega

1. **Verifique** se o seu repositório está público e acessível.
2. **Envie o link do seu repositório** para o email [vrogana@gmail.com](mailto:vrogana@gmail.com)
3. **Certifique-se** de que todos os arquivos necessários estão presentes e atualizados.

## Recursos Úteis

- [Documentação do Git](https://git-scm.com/doc)
- [Tutorial de Markdown](https://www.markdownguide.org/basic-syntax/)
- Minha recomendação máxima é usar o chatgpt ou escrever o que voce quiser no Notion e exportar como Markdown ao inves de pdf (DICA DE OURO!!)
- Abuse do Chatgpt

---

**Boa sorte e bom trabalho!**