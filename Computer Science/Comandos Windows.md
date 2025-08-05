
#computer_science #os 

### 📁 **Navegação e gerenciamento de arquivos**

|Comando|Descrição|
|---|---|
|`cd [pasta]`|Muda de diretório|
|`cd ..`|Sobe um nível|
|`dir`|Lista os arquivos e pastas no diretório atual|
|`mkdir [pasta]`|Cria uma nova pasta|
|`rmdir [pasta]` / `rm -r [pasta]`|Remove uma pasta|
|`del [arquivo]`|Deleta um arquivo|
|`move [origem] [destino]`|Move arquivos ou pastas|
|`copy [origem] [destino]`|Copia arquivos|

---

### ⚙️ **Gerenciamento de sistema e rede**

|Comando|Descrição|
|---|---|
|`tasklist`|Mostra os processos em execução|
|`taskkill /PID [id] /F`|Encerra um processo|
|`ipconfig`|Exibe informações de rede (IP, gateway, etc)|
|`ping [endereço]`|Testa conexão com um servidor|
|`netstat -an`|Mostra portas e conexões de rede|
|`systeminfo`|Informações detalhadas sobre o sistema|

---

### 🧰 **Ferramentas de desenvolvimento**

|Comando|Descrição|
|---|---|
|`code .`|Abre o VS Code no diretório atual (se instalado)|
|`git init` / `git clone`|Inicializa ou clona um repositório Git|
|`git status` / `git commit` / `git push`|Fluxo básico de versionamento|
|`npm init` / `npm install`|Inicializa projeto Node.js e instala dependências|
|`python script.py`|Roda um script Python|
|`javac HelloWorld.java && java HelloWorld`|Compila e executa código Java|
|`dotnet new console`|Cria um novo projeto .NET console (C#)|
|`docker run` / `docker build`|Comandos Docker básicos (se instalado)|

---

### 🧪 **Utilidades e outros comandos úteis**

|Comando|Descrição|
|---|---|
|`cls`|Limpa a tela do terminal|
|`echo [texto]`|Imprime texto no terminal|
|`type [arquivo]`|Mostra o conteúdo de um arquivo|
|`where [programa]`|Mostra o caminho de um executável|
|`start [arquivo ou URL]`|Abre um arquivo ou site no app padrão|
|`set` / `setx`|Cria/define variáveis de ambiente|

### 📂 `dir` (Windows)

- **Usado no CMD (Command Prompt)** e também funciona no **PowerShell**.
- Lista os arquivos e pastas do diretório atual no **formato do Windows**.
- Exibe colunas como: data de modificação, tamanho, nome do arquivo.

Exemplo de uso:

```bash
C:\\Users\\Dev> dir

```

---

### 📂 `ls` (Linux/macOS/WSL/PowerShell)

- **Nativo no Linux e macOS**. Também funciona no **PowerShell** (porque ele é compatível com muitos comandos Unix).
- Tem **mais opções e personalizações**, como `ls -la` para mostrar arquivos ocultos e permissões.
- Apresenta a saída no estilo Unix: permissões, proprietário, grupo, tamanho, data, nome.

Exemplo de uso:

```bash
$ ls -la

```

---

### ✅ Comparação rápida

|Característica|`dir`|`ls`|
|---|---|---|
|Sistema principal|Windows|Unix-like (Linux/macOS/WSL)|
|Funciona no PowerShell?|Sim|Sim|
|Funciona no CMD?|Sim|Não nativamente|
|Suporte a parâmetros avançados|Limitado|Muito flexível|
|Aparência da saída|Estilo Windows|Estilo Unix|

### 📁 **Como "subir" um nível**

Como você já sabe:

```bash
cd ..

```

Isso te leva **um diretório acima** do atual.

---

### 📁 **Como "descer" um nível**

Você "desce" um nível indo **para dentro de uma pasta**:

```bash
cd nome-da-pasta

```

Por exemplo, se você está em `C:\\Users\\João` e quer entrar na pasta `Projetos`, basta fazer:

```bash
cd Projetos

```

Agora você estará em `C:\\Users\\João\\Projetos`.

---

### 🔗 **Dicas úteis sobre caminhos**

- Se quiser navegar direto para um caminho completo, use:

```bash
cd C:\\Users\\João\\Projetos\\MeuApp

```

- Para ver onde você está:

```bash
cd

```

(ou `pwd` no PowerShell ou no WSL)

- E se o nome da pasta tiver espaço, use aspas:

```bash
cd "Meus Documentos"

```