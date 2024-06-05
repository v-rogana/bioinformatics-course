# Tutorial Git/Github

Os comandos do Git são fundamentais para o controle de versões e colaboração em projetos de desenvolvimento de software. Aqui está um guia básico sobre como usar os principais comandos do Git em um fluxo de trabalho que inclui criação de branches, merge e pull.

### **Workflow Básico**

1. **Realize alterações nos seus arquivos,** adicionando ao ambiente intermediário stagging (`git add`).
2. **Faça commits regularmente**, mantendo um histórico claro e mensagens significativas sobre as alterações.
3. **Mantenha sua branch atualizada** com a principal (**`main`**) fazendo regularmente **`git pull / git push`**.
4. obs: Se voce estiver trabalhando em grupo, são boas práticas, criar uma nova branch como “develop” e fazer as alterações nela → dar `git pull` na branch principal antes do `git merge` → dar um merge entre develop/main → resolver conflitos (se precisar) → enviar para o github com `git push` .

### Config inicial

```bash
# Configs globais
git config --global user.name "v-rogana"
git config --global user.email "vrogana@gmail.com"
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

### **Trabalhando com Branches (opcional)**

Branches permitem que você desenvolva recursos, corrija bugs ou faça alterações de forma segura separada da branch principal (geralmente chamada de **`main`**). Se você estiver trabalhando sozinho, não tem porque criar uma branch “develop”, mas vamos criar por motivos de ensino.

- Criar uma nova branch:

```bash
git checkout -b develop
```

- Mudar entre branches:

```bash
git checkout nome-da-branch
```

- Listar todas as branches `git branch`
- Para descobrir qual branch esta e o que esta atualizado use:

```bash
git status
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

### **Merge**

Merging é o processo de unir mudanças de diferentes branches. Para trazer as mudanças de outra branch para a branch atual (por exemplo, merge da **`branch-feature`** para **`main`**):

- **Puxar as últimas atualizações do repositório remoto (caso esteja trabalhando em grupo)**:
    - Tanto a criação de um branch “develop” quanto o merge e o git pull só são uteis em códigos que está sofrendo alterações de múltiplas pessoas ao mesmo tempo, isso porque todos esses passos são formas de evitar conflitos entre as alterações no código original.
    - Digamos que voce esta trabalhando com outra pessoa com um script, e essa outra pessoa fez uma alteração no github, se voce não usar o git pull, vai gerar conflitos pois o codigo que esta lá não é o mesmo que voce alterou.

```bash
git pull origin main
```

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

- **Enviar suas mudanças para o repositório remoto**:

```bash
git push origin main
```

### Realizando uma nova mudança (exercício)

- Volte para o branch de develop.
- Faça uma alteração no script anterior (exemplo: removendo os outputs indesejáveis).

```bash
rm ${i}_a_v0.sam
rm ${i}_a_v0_onlymapped.sam
```

- Adicione essa mudança para o staging.
- Realize o commit com a flag `-m`.
- Dentro da branch main, de um `git pull` e realize o merge com a branch develop.
- Sincronize a sua branch main, com o seu github.