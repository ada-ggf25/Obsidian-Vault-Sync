#computer_science #os
### 📁 **Navegação e gerenciamento de arquivos**

- `pwd` – Mostra o diretório atual.
- `ls` – Lista arquivos no diretório (use `ls -la` para ver arquivos ocultos e permissões).
- `cd <diretório>` – Muda de diretório.
- `cp <origem> <destino>` – Copia arquivos ou diretórios (use `r` para diretórios).
- `mv <origem> <destino>` – Move ou renomeia arquivos/diretórios.
- `rm <arquivo>` – Remove arquivos (use `r` para diretórios, com cuidado!).
- `mkdir <nome>` – Cria um diretório.
- `touch <arquivo>` – Cria um arquivo vazio.
- `find . -name "<padrão>"` – Procura arquivos recursivamente.

---

### 🛠️ **Manipulação de texto e código**

- `cat <arquivo>` – Exibe o conteúdo do arquivo.
- `less <arquivo>` – Visualiza o conteúdo de forma paginada.
- `head -n 10 <arquivo>` – Mostra as primeiras 10 linhas.
- `tail -n 10 <arquivo>` – Mostra as últimas 10 linhas.
- `grep "<texto>" <arquivo>` – Procura texto dentro de arquivos.
- `sed 's/antigo/novo/g' <arquivo>` – Substitui texto.
- `awk '{print $1}' <arquivo>` – Manipula colunas em arquivos de texto.

---

### 🧠 **Gerenciamento de processos**

- `ps aux` – Mostra todos os processos em execução.
- `top` ou `htop` – Monitor de processos em tempo real.
- `kill <PID>` – Encerra um processo.
- `jobs` – Lista processos em segundo plano no shell atual.
- `fg %<n>` – Traz processo em segundo plano de volta para o primeiro plano.
- `bg %<n>` – Coloca um processo em segundo plano.

---

### 🗃️ **Controle de versão (Git)**

- `git clone <url>` – Clona um repositório.
- `git status` – Mostra o estado atual do repositório.
- `git add <arquivo>` – Adiciona mudanças ao stage.
- `git commit -m "mensagem"` – Salva um snapshot das mudanças.
- `git push` – Envia mudanças para o repositório remoto.
- `git pull` – Puxa mudanças do repositório remoto.

---

### 🌐 **Rede e internet**

- `ping <host>` – Verifica conectividade.
- `curl <url>` – Faz requisições HTTP simples.
- `wget <url>` – Baixa arquivos.
- `ssh user@host` – Conecta a outro servidor via SSH.

---

### ⚙️ **Ambientes e compilação**

- `make` – Executa Makefiles.
- `gcc <arquivo.c> -o executável` – Compila código C.
- `g++ <arquivo.cpp> -o executável` – Compila código C++.
- `python <arquivo.py>` – Executa scripts Python.
- `node <arquivo.js>` – Executa scripts Node.js.
- `virtualenv venv` / `source venv/bin/activate` – Ambientes Python.

---

### 🧰 **Outros úteis**

- `man <comando>` – Mostra o manual do comando.
- `which <comando>` – Mostra o caminho do executável.
- `alias ll='ls -lah'` – Cria atalhos de comandos.
- `chmod +x <arquivo>` – Torna um script executável.
- `history` – Mostra histórico de comandos.
- `clear` ou `Ctrl+L` – Limpa o terminal.