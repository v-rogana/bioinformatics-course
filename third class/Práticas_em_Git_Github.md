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
3. Crie um repositório com o nome: `Praticas_Alinhamento_Sequencia` (nome que quiser).
4. Descrição:
Resposta das Atividades Práticas em Linhas de Comando. 
5. O repositório deve ser público.
6. Dê check no Add README file.
7. Crie o Repositório :)

### Config inicial

```bash
# Configs globais
git config --global user.name "{seu_usuario}"
git config --global user.email "seu_email@gmail.com"
# Chamar a Branch principal de "main"
git config --global init.default branch main
```

### **Iniciando um Repositório**

- Para clonar um repositório existente:

```bash
git clone https://github.com/{seu_usuario}/Praticas_Alinhamento_Sequencia
```

### Crie um Token de Acesso Pessoal

1. Clique no **ícone do seu perfil** no canto superior direito.
2. Selecione **"Settings"** (Configurações).
3. No menu lateral esquerdo, role para baixo até encontrar **"Developer settings"** (Configurações de desenvolvedor) e clique.
4. Selecione **"Personal access tokens"** (Tokens de acesso pessoal).
5. Clique em **"Tokens (classic)"**
6. Clique em **"Generate new token"** (Gerar novo token).
7. **Nota (Note):** Dê um nome descritivo ao seu token, como "Token para push".
8. **Expiração (Expiration):** Defina a validade do token conforme sua necessidade.
9. **Scopes (Escopos):** Marque as permissões necessárias. Para operações de push, selecione:
    - **"repo"**: Acesso total a repositórios privados e públicos.
10. Clique em **"Generate token"** (Gerar token).
11. **Importante:** Copie o token gerado e guarde-o em um local seguro. Você não poderá visualizá-lo novamente após sair da página.

### Usar o token na URL remota

```bash
git remote set-url origin https://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@github.com/usuario/Praticas_Alinhamento_Sequencia
```

### Realizando alterações no Git

- Crie/adicione um script (como no exemplo abaixo).
- Use o `git status` para ver se essa alteração foi comitada.

```bash
#!/bin/bash
for i in *.fasta
do 
bowtie -f -S -v 0 -a -p 20 -t ../index_cdhitest_control_viral_contigs $i > ${i}_a_v0.sam 2> ${i}_a_v0_bowtie.log
samtools view -S -h -F 4 ${i}_a_v0.sam > ${i}_a_v0_onlymapped.sam
samtools sort -O SAM -o ${i}_a_v0_onlymapped_sorted.sam ${i}_a_v0_onlymapped.sam
samtools view -Sb ${i}_a_v0.sam > ${i}_a_v0.bam
done
```

### **Fazendo Commits**

Commits são como salvar pontos no seu projeto e são a chave para manter um histórico de mudanças. Pense como um snapshot da sua versão atual que pode ser usada para voltar nessa versão posteriormente.

- Adicionar mudanças para staging (preparação para commit):

```bash
git add .
# ou use
git add caminho/para/script
```

- Use o git status para confirmar se a mudança foi realizada.
- Commitar as mudanças:

```bash
# informação importante para depois achar a versão que voce queria
git commit -m "Adicionei script de automatizacao do bowtie" 
```

### **Sincronizando Mudanças**

Sincronize suas mudanças com um repositório remoto (como o GitHub).

- **Enviar suas mudanças para o repositório remoto**:

```bash
git push origin main
```

### Exercício 1 - Realizando uma nova mudança

- Faça uma alteração no script anterior (exemplo: removendo os outputs indesejáveis).

```bash
rm ${i}_a_v0.sam
rm ${i}_a_v0_onlymapped.sam
```

- Adicione essa mudança para o staging.
- Realize o commit com a flag `-m`.
- Sincronize a sua branch main, com o seu github.

### Exercício 2 - Voltar na versão anterior a mudança

Retornar a uma versão anterior do seu projeto pode ser necessário por vários motivos, como desfazer alterações que introduziram bugs ou restaurar um estado estável do código.

O `git revert` é a maneira segura de desfazer mudanças, pois cria um novo commit que inverte as alterações introduzidas por um commit anterior, sem modificar o histórico existente

- **Obtenha o hash do commit desejado:**
    
    ```bash
    git log
    ```
    
- **Faça o res:**
    - Para manter as alterações no diretório de trabalho:
        
        ```bash
        git reset --soft <hash_do_commi
        ```
        
    - Para descartar as alterações no diretório de trabalho:
        
        ```bash
        git reset --hard <hash_do_commit
        ```
        
- **Force o push para o repositório remoto (com cautela):**
    
    ```bash
    git push origin <nome_da_branch> --force
    ```
    
    **Atenção:** O uso de `--force` pode sobrescrever alterações no repositório remoto e afetar outros colaboradores.
    

---

# Exercício Avaliativo: Criando seu Repositório da Disciplina no GitHub

## Introdução

Neste exercício, você irá criar um repositório no GitHub para armazenar os códigos e documentos das aulas práticas da disciplina. Utilizaremos como base o **Guia Teórico e Prático de Detecção de Wolbachia em Bibliotecas de Pequenos RNAs**. O objetivo é familiarizá-lo com o uso básico do Git e GitHub, aplicando boas práticas de versionamento e documentação.

## Objetivos

- **Criar um repositório pessoal** no GitHub para a disciplina.
- **Adicionar pelo menos 3 arquivos**:
    - Um **README.md** explicando o projeto e as atividades práticas.
    - Um arquivo com os **códigos de limpeza de dados**.
    - Um arquivo com os **códigos de alinhamento usando o Bowtie**.
- **Aplicar boas práticas básicas** no uso do Git e GitHub.
- **Opcional:** Adicionar outros códigos, exemplos ou documentos para obter até **10 pontos adicionais**.

## Instruções Passo a Passo

### **Passo 1: Criar uma Conta no GitHub (se ainda não tiver)**

1. Acesse [github.com](https://github.com/) e clique em **Sign up**.
2. Siga as instruções para criar uma conta gratuita.

### **Passo 2: Instalar e Configurar o Git**

1. **Instalar o Git**:
    - Baixe em [git-scm.com/downloads](https://git-scm.com/downloads) e instale conforme seu sistema operacional.
2. **Configurar seu nome de usuário e e-mail**:
    
    ```bash
    git config --global user.name "Seu Nome"
    git config --global user.email "seuemail@example.com"
    ```
    

### **Passo 3: Criar um Novo Repositório no GitHub**

1. Faça login na sua conta do GitHub.
2. No canto superior direito, clique no ícone **+** e selecione **New repository**.
3. Preencha:
    - **Repository name**: Escolha um nome, por exemplo, `wolbachia-detection`.
    - **Description**: Uma breve descrição do projeto.
    - Marque a opção **Public**.
    - **Não** selecione a opção para criar um README automaticamente.
4. Clique em **Create repository**.

### **Passo 4: Clonar o Repositório para o Seu Computador**

1. No seu repositório recém-criado, copie a URL HTTPS (botão verde **Code**).
2. Abra o terminal (Prompt de Comando, PowerShell ou Terminal no Mac/Linux).
3. Navegue até o diretório onde deseja salvar o repositório.
    
    ```bash
    cd /caminho/para/sua/pasta
    ```
    
4. Clone o repositório:

Substitua `seu-usuario` pelo seu nome de usuário no GitHub.
    
    ```bash
    git clone https://github.com/seu-usuario/wolbachia-detection.git
    ```
    

### **Passo 5: Criar o README.md**

1. Navegue até a pasta do repositório clonado:
    
    ```bash
    cd wolbachia-detection
    ```
    
2. Crie um arquivo chamado `README.md` usando um editor de texto.
3. No `README.md`, inclua:
    - **Título do projeto**: `Detecção de Wolbachia em Bibliotecas de Pequenos RNAs`.
    - **Introdução**: Resuma o objetivo do projeto e a importância da detecção de **Wolbachia**.
    - **Descrição das atividades práticas**: Explique brevemente as etapas que serão realizadas, baseando-se no guia fornecido.
4. Salve o arquivo.

### **Passo 6: Adicionar o Código de Limpeza de Dados**

1. Crie um arquivo chamado `limpeza_dados.sh`.
2. Copie os comandos de limpeza de dados do guia:
    
    ```bash
    # Avaliação de qualidade das leituras com FastQC
    fastqc SRRXXXXXXX.fastq
    
    # Limpeza e corte de sequências de baixa qualidade com Trim Galore
    trim_galore --fastqc -q 25 --trim-n --max_n 0 -j 1 --length 18 --dont_gzip SRRXXXXXXX.fastq
    
    ```
    
3. Adicione comentários explicando cada comando.
4. Salve o arquivo.

### **Passo 7: Adicionar o Código de Alinhamento com Bowtie**

1. Crie um arquivo chamado `alinhamento_bowtie.sh`.
2. Insira os comandos relacionados ao alinhamento:
    
    ```bash
    # Construção do índice de alinhamento com Bowtie
    bowtie-build reference.fasta reference_index
    
    # Alinhamento das sequências contra a referência de Wolbachia
    bowtie -f -S -a -v 0 -p 3 -t reference_index sample.fasta > sample.sam 2> sample_bowtie.log
    
    ```
    
3. Inclua comentários explicando os parâmetros usados.
4. Salve o arquivo.

### **Passo 8: Fazer o Commit dos Arquivos**

1. Verifique o status dos arquivos:
    
    ```bash
    git status
    ```
    
2. Adicione os arquivos ao staging area:
    
    ```bash
    git add README.md limpeza_dados.sh alinhamento_bowtie.sh
    ```
    
3. Faça o commit com uma mensagem descritiva:
    
    ```bash
    git commit -m "Adiciona README e scripts de limpeza e alinhamento"
    ```
    

### **Passo 9: Enviar as Alterações para o GitHub**

1. Envie os commits para o repositório remoto:
**Nota:** Se sua branch principal for `master`, use `git push origin master`.
    
    ```bash
    git push origin main
    ```
    

### **Passo 10: Verificar o Repositório no GitHub**

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
- **Adicionar gráficos ou figuras** resultantes das análises (ainda vamos chegar na aula de R para isso).

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