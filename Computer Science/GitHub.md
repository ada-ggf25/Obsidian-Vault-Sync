
#computer_science #github #git 

[[GitHub Repo Rules]]
[[GitHub Commits]]
[[GitHub Branchs]]
[[GitHub Achievements]]
[[GitHub Gist]]
[[GitHub Packages]]
[[Deleting GitHub Brunchs]]
[[Galaxy Brain - GitHub Achievement]]
[[Pair Extraordinaire - GitHub Achievement]]
[[GitHub Pull Requests]]
[[Github - Brunch Commit Backup]]
### 🔧 1. Criar um novo **repositório** no GitHub:

**Pelo site do GitHub:**

1. Vá para [](https://github.com/)[https://github.com](https://github.com).
2. Clique no botão verde “**New repository**” (ou “+” no canto superior direito > “New repository”).
3. Dê um nome ao repositório, adicione uma descrição se quiser.
4. Escolha se será público ou privado.
5. (Opcional) Marque a opção para inicializar com um `README.md`.
6. Clique em “Create repository”.

**Depois, no seu terminal, clone e gerencie localmente:**

```bash
git clone <https://github.com/seu-usuario/nome-do-repositorio.git>
cd nome-do-repositorio

```

---

### 📁 2. Criar um **diretório (pasta)** dentro de um repositório existente:

Git não armazena diretórios vazios, então você precisa colocar um arquivo dentro.

**No seu terminal:**

```bash

mkdir minha-pasta
touch minha-pasta/README.md

```

Depois, adicione, commite e envie para o GitHub:

```bash

git add minha-pasta
git commit -m "Adiciona nova pasta com README"
git push

```

Agora a pasta aparecerá no repositório remoto.

---

### 📦 Gerenciar o diretório (pasta)

Uma vez que o diretório existe, você pode:

- Adicionar arquivos nele: `touch minha-pasta/arquivo.py`
- Editar arquivos: `nano`, `vim`, VSCode, etc.
- Mover arquivos: `mv`
- Apagar arquivos ou pastas: `rm arquivo.py` ou `rm -r minha-pasta`
- Renomear: `mv minha-pasta novo-nome`

Depois de qualquer alteração, não esquece o ciclo:

```bash

git add .
git commit -m "Descreve o que foi mudado"
git push

```

### 📌 O que significa _clonar_ um repositório?

**Clonar** (`git clone`) é o processo de **copiar um repositório remoto (do GitHub, por exemplo) para a tua máquina local**, incluindo **todo o histórico de commits, branches, e arquivos**.

---

### 🧠 Por que clonar um repositório?

Aqui vão os principais motivos:

### 1. **Para trabalhar localmente**

Você precisa do código no seu computador pra poder editar, testar, compilar, etc. Clonar traz tudo pra tua máquina.

### 2. **Para colaborar num projeto**

Se alguém cria um repositório no GitHub e você quer contribuir, você clona o repo, faz mudanças localmente, e depois envia de volta com `git push`.

### 3. **Para ter controle de versões local**

Com o repo clonado, você pode usar todos os comandos do Git: `git commit`, `git branch`, `git merge`, etc. Você tem a história do projeto completa.

---

### 📂 Diferença entre clonar e baixar ZIP

- **Baixar como ZIP**: você só pega os arquivos atuais — sem histórico, sem branches, sem controle de versão.
- **Clonar com `git clone`**: você traz todo o repositório com o histórico e o controle total.

---

### 💻 Exemplo prático

Suponha que você criou um repositório no GitHub chamado `simulacao-fisica`.

Para começar a trabalhar nele localmente:

```bash

git clone <https://github.com/seu-usuario/simulacao-fisica.git>
cd simulacao-fisica

```

Agora você está dentro do repositório local. Pode criar arquivos, fazer commits, etc., e depois enviar pro GitHub com:

```bash

git add .
git commit -m "Adiciona simulação inicial"
git push

```

## 🚀 Comandos Git mais usados

### 🛠️ Inicialização e clonagem

|Comando|O que faz|
|---|---|
|`git init`|Inicializa um novo repositório Git na pasta atual|
|`git clone <url>`|Clona um repositório remoto para tua máquina|

---

### 📦 Trabalhar com arquivos

|Comando|O que faz|
|---|---|
|`git add <arquivo>`|Adiciona arquivos à "staging area" (prontos pra commit)|
|`git add .`|Adiciona todos os arquivos modificados|
|`git status`|Mostra o estado atual dos arquivos (modificados, staged, etc)|
|`git rm <arquivo>`|Remove um arquivo do Git|
|`git mv <velho> <novo>`|Renomeia/move um arquivo|

---

### 💾 Commit e histórico

|Comando|O que faz|
|---|---|
|`git commit -m "mensagem"`|Cria um commit com uma mensagem descritiva|
|`git log`|Mostra o histórico de commits|
|`git show <commit>`|Mostra detalhes de um commit específico|

---

### 🔄 Branches e merge

|Comando|O que faz|
|---|---|
|`git branch`|Lista as branches locais|
|`git branch <nome>`|Cria uma nova branch|
|`git checkout <nome>`|Troca para outra branch|
|`git switch <nome>`|Alternativa moderna ao checkout|
|`git merge <branch>`|Junta a branch indicada com a atual|
|`git rebase <branch>`|Reescreve o histórico para aplicar commits "por cima"|

---

### 🌐 Repositório remoto

|Comando|O que faz|
|---|---|
|`git remote -v`|Lista os repositórios remotos configurados|
|`git push`|Envia commits para o repositório remoto|
|`git pull`|Baixa atualizações do repositório remoto e junta com seu código|
|`git fetch`|Baixa as atualizações sem fazer merge automático|

---

### 🧹 Manutenção e correções

|Comando|O que faz|
|---|---|
|`git diff`|Mostra as diferenças entre arquivos modificados|
|`git stash`|Salva alterações temporariamente (sem fazer commit)|
|`git reset`|Desfaz mudanças em commits ou staging|
|`git revert <commit>`|Cria um novo commit que desfaz um commit anterior|

---

## ⚡ Dica extra: Ajuda interna

Se quiser ajuda sobre qualquer comando, só digita:

```bash

git help <comando>
# ou
git <comando> --help

```

## ✅ **Commit**

### 👉 O que é:

Um **commit** é um _"salvamento"_ de um estado do seu projeto.

### 🧠 Imagina assim:

É como tirar um _snapshot_ do projeto — você diz “a partir daqui, esse é um ponto importante”.

### 📌 Exemplo:

```bash

git add arquivo.txt
git commit -m "Adiciona o arquivo com funções principais"

```

> O commit só afeta seu repositório local (ainda não vai pro GitHub).

---

## 🚀 **Push**

### 👉 O que é:

Um **push** envia seus commits locais para o **repositório remoto**, geralmente no GitHub.

### 📌 Exemplo:

```bash

git push

```

> Isso faz com que suas alterações apareçam no GitHub (ou outro repositório remoto).

---

## ⬇️ **Pull**

### 👉 O que é:

Um **pull** baixa as alterações do repositório remoto (ex: GitHub) e tenta aplicá-las no seu repositório local.

É basicamente um:

```bash

git fetch && git merge

```

### 📌 Exemplo:

```bash

git pull

```

> Isso garante que você está trabalhando com a versão mais atualizada do projeto.

---

## 🔀 **Merge**

### 👉 O que é:

**Merge** é o processo de combinar duas **branches** (ou duas versões diferentes do projeto).

Você geralmente faz um merge quando:

- Criou uma nova **branch** pra testar algo
- Terminou essa funcionalidade
- E agora quer **juntar de volta** na branch principal (geralmente `main` ou `master`)

### 📌 Exemplo:

```bash

git checkout main
git merge minha-feature

```

> Isso aplica as mudanças da branch minha-feature dentro da main.

---

## 🧠 Resumo visual:

```
perl
CopyEdit
[Seu código local] ── git commit ──> [Histórico local]
       │
       └─ git push ──> [GitHub / remoto]
              ↑
        git pull <───
              ↑
       Outra pessoa fez push

```

## 🖥️ **Ctrl+S = salvar no editor**

Quando fazes **Ctrl+S**, estás só a **gravar o conteúdo do ficheiro no disco do teu computador**. Nenhum histórico é guardado — estás basicamente a dizer: "agora este ficheiro tem este conteúdo".

- ✔️ Salva as alterações do ficheiro atual.
- ❌ Não guarda _como estava antes_.
- ❌ Não cria histórico.
- ❌ Não acompanha _por que_ foi feita a mudança.

---

## 🧠 **git commit = salvar com história e contexto**

Quando fazes um `git commit`, estás a dizer:

> "Grava estas mudanças como uma versão importante do projeto, com uma mensagem que explica o que foi feito, e marca este ponto no tempo."

Ou seja:

- ✔️ Guarda o estado atual do projeto.
- ✔️ Cria um ponto no histórico que podes voltar atrás.
- ✔️ Permite ver o que mudou, quando e por quê.
- ✔️ Pode ser partilhado com outras pessoas (via `push`).

---

### 📸 Uma analogia simples:

|Ação|Analogia|
|---|---|
|`Ctrl+S`|Gravar por cima de uma foto no Photoshop|
|`git commit`|Salvar uma nova versão da foto no histórico, com nota: “melhorei brilho”|

---

### 👀 Exemplo prático

Digamos que estás a trabalhar num ficheiro chamado `simulacao.py`:

1. Escreves uma função nova.
    
2. Fazes **Ctrl+S** → o ficheiro está gravado no teu disco.
    
3. Mas o Git ainda nem sabe que alteraste nada.
    
4. Agora fazes:
    
    ```bash
    
    git add simulacao.py
    git commit -m "Adiciona função para simular colisões"
    
    ```
    
    Agora sim, o Git guardou essa versão com esse contexto.
    

---

### 💥 Conclusão rápida

- **Ctrl+S** → salva conteúdo do ficheiro.
- **git commit** → salva um _estado do projeto_ com contexto e histórico.

## ✅ 1. **Instalar a GitHub CLI (`gh`)**

Se ainda não tens instalada, aqui está como fazê-lo:

### macOS (Homebrew)

```bash

brew install gh

```

### Ubuntu/Debian

```bash

sudo apt install gh

```

---

## ✅ 2. **Autenticar com o GitHub**

Depois de instalar, corre:

```bash

gh auth login

```

Vai-te perguntar:

- Se queres [GitHub.com](http://GitHub.com) ou Enterprise → escolhe [GitHub.com](http://GitHub.com).
- Se queres HTTPS ou SSH → escolhe o que preferires (HTTPS é mais simples).
- Se queres usar o browser para login → responde sim.

---

## ✅ 3. **Criar um novo repositório GitHub via terminal**

Agora, dentro da pasta do teu projeto local, faz:

```bash

gh repo create nome-do-repositorio

```

Ele vai perguntar:

- Nome do repositório → podes aceitar o nome atual da pasta.
- Descrição → opcional.
- Visibilidade (public/private).
- Se queres adicionar README, .gitignore, etc.

Depois disso, ele **cria o repositório remoto no GitHub** e (opcionalmente) já liga com o repositório local!

---

## ✅ 4. **Subir o código local (caso ainda não tenhas feito)**

```bash

git add .
git commit -m "primeiro commit"
git push -u origin main

```

> O -u serve para ligar a tua branch main local à main do GitHub.

---

## 🎁 Exemplo completo do zero:

```bash

mkdir meu-projeto
cd meu-projeto
git init
echo "# Meu Projeto" > README.md
git add .
git commit -m "Commit inicial"
gh repo create meu-projeto --public --source=. --remote=origin --push

```

Pronto! Isso:

- Inicializa o Git.
- Cria o repositório remoto no GitHub.
- Liga tudo e faz o `push` inicial.

## 🔹 `git commit`

|Flag|O que faz|
|---|---|
|`-m "mensagem"`|Define a mensagem do commit diretamente na linha de comando|
|`--amend`|Edita o último commit (útil para corrigir mensagens ou adicionar ficheiros esquecidos)|

### 📌 Exemplo:

```bash

git commit -m "Corrige bug na simulação"
git commit --amend -m "Mensagem corrigida"

```

---

## 🔹 `git push`

|Flag|O que faz|
|---|---|
|`-u` ou `--set-upstream`|Liga a tua branch local à branch remota (só é necessário na 1ª vez)|
|`--force`|Força o envio, sobrescrevendo histórico remoto (⚠️ perigoso!)|
|`--tags`|Envia também as _tags_ criadas localmente|

### 📌 Exemplo:

```bash

git push -u origin main
git push --force

```

---

## 🔹 `git log`

|Flag|O que faz|
|---|---|
|`--oneline`|Mostra o histórico em uma linha por commit|
|`--graph`|Mostra um gráfico visual das branches e merges|
|`--decorate`|Mostra labels como HEAD, branches e tags|
|`-p`|Mostra as diferenças (diffs) de cada commit|

### 📌 Exemplo:

```bash

git log --oneline --graph --decorate

```

---

## 🔹 `git diff`

|Flag|O que faz|
|---|---|
|`--staged`|Mostra diferenças entre o _staging_ e o último commit|
|`HEAD`|Compara a versão atual com o último commit|

---

## 🔹 `git status`

|Flag|O que faz|
|---|---|
|`-s` ou `--short`|Mostra um resumo curto|
|`-b`|Mostra em que branch estás|

---

## 🔹 `git checkout` (ou `git switch`)

|Flag|O que faz|
|---|---|
|`-b <branch>`|Cria e troca para uma nova branch|
|`--`|Separa nomes de ficheiros de nomes de branches|

### 📌 Exemplo:

```bash

git checkout -b nova-feature
```

---

## 🔹 `gh repo create` (GitHub CLI)

|Flag|O que faz|
|---|---|
|`--public` / `--private`|Define visibilidade do repositório|
|`--source=.`|Usa a pasta atual como código-fonte|
|`--remote=origin`|Define o nome do remote (geralmente `origin`)|
|`--push`|Já faz push do código local após criar|

---

## 🧠 Dica geral

- = flag curta (ex: `m`)
- `-` = flag longa (ex: `-help`)

Podes sempre usar:

```bash

git <comando> --help

```

Exemplo:

```bash

git push --help

```

## ✅ **1. Commit**

### 📌 O que faz:

- Cria um **commit local**, com a mensagem que escreveste.
- **Não envia** nada para o GitHub (ou outro repositório remoto).

### 🧠 Usa quando:

- Estás a fazer várias alterações e queres gravar um ponto de progresso.
- Não queres fazer push ainda, só guardar localmente.

---

## ♻️ **2. Commit (Amend)**

### 📌 O que faz:

- Em vez de criar um novo commit, **substitui o último** commit com as novas mudanças.
- Útil para **corrigir ou completar** o commit anterior.

### 🧠 Usa quando:

- Esqueceste de incluir um ficheiro ou queres mudar a mensagem do commit anterior.
- Ainda **não fizeste push** do commit anterior (⚠️ cuidado se já fizeste!).

---

## 🚀 **3. Commit & Push**

### 📌 O que faz:

- Cria um commit local **e envia imediatamente** para o repositório remoto (GitHub).
    
- É o mesmo que:
    
    ```bash
    bash
    CopyEdit
    git commit -m "mensagem"
    git push
    
    ```
    

### 🧠 Usa quando:

- Queres guardar e **compartilhar imediatamente** as alterações com outras pessoas.
- Estás a trabalhar sozinho e queres manter o repositório remoto sempre atualizado.

---

## 🔁 **4. Commit & Sync**

### 📌 O que faz:

- Cria o commit local.
- Depois:
    - **puxa (`pull`) alterações remotas**.
    - E **faz push das tuas alterações**.

### 🧠 Usa quando:

- Estás a trabalhar em equipe e queres garantir que tens a versão mais recente antes de subir a tua.
- Evita conflitos ao sincronizar com o que outras pessoas já fizeram.

---

## 🧠 Resumo rápido:

|Opção|Cria commit local|Faz push|Faz pull antes do push|
|---|---|---|---|
|**Commit**|✅|❌|❌|
|**Commit (Amend)**|✅ (substitui)|❌|❌|
|**Commit & Push**|✅|✅|❌|
|**Commit & Sync**|✅|✅|✅|

## 🧠 O que é `git fetch`?

`git fetch` é o comando que **vai buscar as mudanças mais recentes do repositório remoto (como o GitHub), mas sem aplicá-las no teu código local**.

### 👉 Em outras palavras:

> Ele atualiza a tua cópia local da estrutura do repositório remoto — branches, commits, etc. — mas não mistura nada com o teu trabalho local.

---

## 🧭 Comparação com `git pull`

|Comando|Faz o quê?|
|---|---|
|`git fetch`|**Baixa** as novidades do remoto, mas não aplica|
|`git pull`|Faz `fetch` + **merge** (ou `rebase`) no que estás a trabalhar|

---

### 🎯 Exemplo prático

Imagina que alguém fez `push` de novos commits na branch `main`.

Se tu correres:

```bash
bash
CopyEdit
git fetch origin

```

O Git vai:

- Contactar o GitHub.
- Ver o que mudou.
- Baixar os novos commits **mas não aplicar** esses commits na tua branch local ainda.

Agora o Git sabe que o `origin/main` (a versão no GitHub) está à frente da tua `main`, mas **tu ainda não tens essas mudanças no teu código local**.

---

### 💡 Como ver o que mudou após um `fetch`

Depois de fazer `git fetch`, podes ver o que mudou com:

```bash
bash
CopyEdit
git log HEAD..origin/main

```

Ou, para ver um diff direto:

```bash
bash
CopyEdit
git diff origin/main

```

---

### 🔧 Quando usar `git fetch`?

- ⚠️ Quando **queres ver se há mudanças no repositório remoto antes de mexer no teu código**.
- 🧪 Quando estás a preparar merges ou rebase, mas ainda não queres aplicar.
- 🔍 Quando só queres **verificar o que mudou** antes de te comprometer a juntar.

---

## ✅ Resumo visual

```bash
bash
CopyEdit
# Antes
[local/main] — A — B
[origin/main] — A — B — C — D

# Após git fetch
[local/main] — A — B
[origin/main] — A — B — C — D

# Após git pull
[local/main] — A — B — C — D (agora igual ao origin/main)

```

## 🔀 **git merge**

### 📌 O que faz:

Combina duas branches **criando um novo commit de junção (merge commit)**. O histórico das duas branches fica **preservado como ele é**.

### 🧠 Analogia:

Imagina que duas pessoas escreveram capítulos de um livro em paralelo. O `merge` junta os dois capítulos, **mas deixa claro onde cada um começou e terminou**.

### ✅ Características:

- Preserva a história **exatamente como aconteceu**.
- Mais transparente para equipes.
- Pode criar históricos mais “bagunçados”.

### 📊 Exemplo visual:

```bash
bash
CopyEdit
# Branch main
A---B---C

# Branch feature
     \\
      D---E

# git merge feature
A---B---C---F
             \\
              D---E

```

Resultado final:

```bash
bash
CopyEdit
A---B---C-------M
         \\     /
          D---E

```

(M = commit de merge)

---

## 🧱 **git rebase**

### 📌 O que faz:

Move a branch atual e **"recoloca" os commits dela em cima de outra branch**, como se tivesse sido criada mais tarde. **Reescreve o histórico.**

### 🧠 Analogia:

Como se tivesses feito a tua tarefa depois da outra pessoa terminar, mesmo que na verdade vocês fizeram ao mesmo tempo.

### ✅ Características:

- Cria um histórico **linear** e limpo.
- Mais elegante para históricos públicos.
- ⚠️ Reescreve a história — **cuidado se já fizeste push!**

### 📊 Exemplo visual:

```bash
bash
CopyEdit
# Antes
A---B---C        (main)
     \\
      D---E      (feature)

# Depois de git rebase main (na branch feature)
A---B---C---D'---E'

```

> Os commits D e E são regravados como D' e E'.

---

## 🤔 Quando usar cada um?

|Situação|Usa `merge`|Usa `rebase`|
|---|---|---|
|Estás a trabalhar em equipe|✅ Mais seguro|⚠️ Reescreve história|
|Estás a limpar teu histórico local|❌|✅ Deixa linear|
|Já fizeste `push` para o remoto|✅ Não tem problema|⚠️ Pode causar conflitos para outros|
|Queres ver a sequência real dos commits|✅ Transparente|❌ Finge que aconteceu em outra ordem|

---

## 🧠 Dica prática

Se fores o único a trabalhar na branch, usa `rebase` para manter tudo limpo.

Se estiveres a colaborar com outras pessoas, `merge` é mais seguro e transparente.

## 🌿 O que é uma **branch** no Git?

### 🧠 Definição simples:

Uma **branch (ramo)** é uma **linha de desenvolvimento independente** dentro do teu projeto.

> É como criar uma cópia temporária do teu projeto para experimentares, sem afetar a versão principal.

---

### 📚 Analogia:

Imagina que estás a escrever um livro.

- A `main` é a versão oficial.
- Crias uma branch chamada `novo-final` para testar um final alternativo.
- Se gostares, juntas (faz `merge`) com a principal.
- Se não, deixas de lado sem afetar o original.

---

## 🔀 Por que usar branches?

Porque te permitem:

✅ Trabalhar em **novas funcionalidades** sem mexer no código estável

✅ Testar ideias, correções, experiências

✅ Colaborar com outros sem pisar no trabalho de ninguém

✅ Organizar o histórico do projeto de forma limpa

---

## 📌 Exemplo prático

```bash
bash
CopyEdit
# Estás na branch principal
git checkout main

# Criar e mudar para nova branch
git checkout -b nova-feature

```

Agora estás numa nova linha de desenvolvimento chamada `nova-feature`. Tudo o que fizeres ali **não vai afetar a main até que faças um merge.**

---

## 📊 Visual do processo

```
mathematica
CopyEdit
main:     A---B---C
                   \\
nova-feature:       D---E---F

```

Depois de `merge`:

```
mathematica
CopyEdit
main:     A---B---C-------M
                   \\     /
                    D---E---F

```

---

## ⚙️ Comandos úteis com branches

|Comando|O que faz|
|---|---|
|`git branch`|Lista as branches locais|
|`git branch nome`|Cria uma nova branch|
|`git checkout nome`|Muda para a branch `nome`|
|`git checkout -b nome`|Cria e já muda para a nova branch|
|`git merge nome`|Junta a branch `nome` com a atual|
|`git branch -d nome`|Apaga uma branch (se já foi mergeada)|

---

## 🧠 Quando usar?

|Situação|Criar branch?|
|---|---|
|Nova funcionalidade|✅ Sim|
|Corrigir um bug|✅ Sim|
|Testar uma ideia|✅ Sim|
|Refatorar código|✅ Sim|
|Traduzir projeto, mudar estrutura...|✅ Sim|

## 🏷️ O que são **tags** no Git?

### 📌 Definição simples:

Uma **tag** é uma **etiqueta (ou marcador)** que aponta para um commit específico e lhe dá um **nome significativo**, normalmente para marcar **versões importantes** do projeto.

---

### 🎯 Para que servem?

- Marcar **releases** (ex: `v1.0.0`, `v2.1.3`)
- Indicar um ponto **estável ou significativo** no histórico
- Facilitar voltar a versões anteriores
- Ajudar ferramentas de CI/CD (como GitHub Actions, GitLab CI, etc.)

---

### 📚 Analogia:

Imagina que tens uma linha do tempo com várias alterações (commits), e queres marcar pontos importantes com uma bandeirinha, tipo:

```
mathematica
CopyEdit
A---B---C---D---E---F
             ↑
           v1.0.0

```

A tag `v1.0.0` aponta para o commit D, por exemplo.

---

## 🧰 Tipos de Tags

### 1. **Lightweight tag**

- Um marcador simples, só um nome ligado ao commit.
- Mais leve, menos metadados.

```bash
bash
CopyEdit
git tag v1.0

```

### 2. **Annotated tag**

- Guarda também: autor, data, mensagem, e pode ser assinado com GPG.
- Ideal para versões públicas.

```bash
bash
CopyEdit
git tag -a v1.0 -m "Primeira versão estável"

```

---

## 🚀 Enviar tags para o GitHub

Depois de criares uma tag localmente, tens de fazer `push` dela para o repositório remoto:

```bash
bash
CopyEdit
git push origin v1.0

```

Ou para enviar todas de uma vez:

```bash
bash
CopyEdit
git push --tags

```

---

## 📜 Ver tags existentes

```bash
bash
CopyEdit
git tag

```

Ou:

```bash
bash
CopyEdit
git show v1.0

```

para ver a que commit a tag aponta e o conteúdo.

---

## 🔁 Apagar tags

### Local:

```bash
bash
CopyEdit
git tag -d v1.0

```

### Remoto:

```bash
bash
CopyEdit
git push origin --delete tag v1.0

```

---

## 🧠 Quando usar tags?

|Situação|Usar tag?|
|---|---|
|Preparar uma versão de release (ex: v1.2.0)|✅ Sim|
|Marcar um ponto estável|✅ Sim|
|Registrar um milestone|✅ Sim|
|Marcar commits experimentais|❌ Melhor não|