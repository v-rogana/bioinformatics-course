# Aula Git/Github

Os comandos do Git são fundamentais para o controle de versões e colaboração em projetos de desenvolvimento de software. Aqui está um guia básico sobre como usar os principais comandos do Git em um fluxo de trabalho que inclui criação de branches, merge e pull.

### **Workflow Básico**

1. **Criar uma branch para cada novo recurso ou correção**.
2. **Faça commits regularmente**, mantendo um histórico claro e mensagens significativas.
3. **Mantenha sua branch atualizada** com a principal (**`main`**) fazendo regularmente **`git pull / git push`**.
4. **Faça merge de suas branches no `main`** quando o trabalho estiver concluído e após resolver quaisquer conflitos.

### Config inicial

```bash
# Configs globais
git config --global user.name "Seu Nome"
git config --global user.email "seuemail@example.com"
# Chamar a Branch principal de "main"
git config --global init.default branch main
```

### **Iniciando um Repositório**

- Para começar um novo repositório Git local, use:

```bash
git init
```

- Para clonar um repositório existente:

```bash
git clone https://github.com/v-rogana/bioinformatics-course
```

### **Trabalhando com Branches**

Branches permitem que você desenvolva recursos, corrija bugs ou faça alterações de forma segura separada da branch principal (geralmente chamada de **`main`** ou **`master`**).

- Criar uma nova branch:

```bash
git checkout -b develop
```

- Mudar entre branches:

```bash
git checkout develop
```

- Listar todas as branches:

```bash
git branch
```

### **Fazendo Commits**

Commits são como salvar pontos no seu projeto e são a chave para manter um histórico de mudanças. Pense como um snapshot da sua versão atual que pode ser usada para voltar nessa versão posteriormente.

- Adicionar mudanças para staging (preparação para commit):

```bash
git add .
```

ou

```bash
git add caminho/para/arquivo
```

- Commitar as mudanças:

```bash
git commit -m "Mensagem explicativa sobre o commit" 
# Importante para posteriormente achar e voltar na versão que você queria
```

### **Merge**

Merging é o processo de unir mudanças de diferentes branches. Para trazer as mudanças de outra branch para a branch atual (por exemplo, merge da **`branch-feature`** para **`main`**):

- Mudar para a branch que receberá as mudanças:

```bash
git checkout main
```

- Executar o merge:

```bash
git merge develop
```

**Resolver conflitos** (se houver): Editar os arquivos marcados pelo Git, resolver as diferenças e então fazer o commit das mudanças resolvidas.

### **Sincronizando Mudanças**

Sincronize suas mudanças com um repositório remoto (como o GitHub).

- **Puxar as últimas atualizações do repositório remoto**:

```bash
git pull origin nome-da-branch
```

- **Enviar suas mudanças para o repositório remoto**:

```bash
git push origin nome-da-branch
```