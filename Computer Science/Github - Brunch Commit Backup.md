#computer_science #github #git 

## Resumo

Para restaurar o seu branch ao estado de um commit anterior usando a interface do GitHub e do VS Code, as abordagens são diferentes em cada ferramenta. No **GitHub Web UI**, não há um comando “reset” direto; em vez disso, pode usar a funcionalidade de **Revert** de commits ou criar um pull request para apontar o branch para um commit específico ([GitHub](https://github.com/orgs/community/discussions/143794?utm_source=chatgpt.com "Setting git branch to point to the tip using GitHub ui #143794")). Já no **VS Code**, pode desfazer o último commit com o comando **Git: Undo Last Commit** no Command Palette ou, usando extensões como o **GitLens**, clicar com o botão direito num commit e selecionar **Revert Commit** ([Visual Studio Code](https://code.visualstudio.com/docs/sourcecontrol/overview?utm_source=chatgpt.com "Using Git source control in VS Code"), [GitKraken](https://www.gitkraken.com/learn/git/problems/revert-git-commit?utm_source=chatgpt.com "Git Revert Commit | Solutions to Git Problems - GitKraken")). Caso precise de uma redefinição mais profunda (equivalente a `git reset --hard`), não há suporte gráfico nativo, mas pode recorrer ao terminal integrado ou usar o **GitHub Desktop** para uma UI que implemente esse recurso ([Stack Overflow](https://stackoverflow.com/questions/74144236/is-there-any-way-to-git-reset-hard-in-the-vscode-app?utm_source=chatgpt.com "Is there any way to \"git reset --hard\" in the VSCode app?"), [GitHub](https://github.com/orgs/community/discussions/60023?utm_source=chatgpt.com "Revert commit on Website · community · Discussion #60023 - GitHub")).

---

## 1. GitHub Web UI

### 1.1 Reverter um Commit

Para criar um novo commit que anule as alterações de um commit anterior:

1. Navegue até o **repositório** no GitHub e clique em **Commits**.
    
2. Encontre o commit que deseja reverter e clique no botão **Revert** (ícone de seta curva).
    
3. Confirme a criação do novo pull request que será aberto automaticamente para aplicar esse “revert” ao branch-alvo. ([GitHub](https://github.com/orgs/community/discussions/143794?utm_source=chatgpt.com "Setting git branch to point to the tip using GitHub ui #143794")).
    

Este método preserva o histórico e é recomendado em branches partilhados.

### 1.2 Mover o Branch para um Commit Específico

Como o GitHub Web UI não oferece um “reset” direto de branch, pode:

- **Criar um pull request** onde o **base branch** é o commit desejado e, ao mesclar, o branch-alvo será atualizado para esse commit. ([GitHub](https://github.com/orgs/community/discussions/143794?utm_source=chatgpt.com "Setting git branch to point to the tip using GitHub ui #143794")).
    
- **Usar o GitHub Desktop**, que expõe um botão “Revert changes in commit” ao clicar com o botão direito num commit no histórico ([GitHub](https://github.com/orgs/community/discussions/60023?utm_source=chatgpt.com "Revert commit on Website · community · Discussion #60023 - GitHub")).
    

---

## 2. VS Code Interface

### 2.1 Desfazer o Último Commit

1. Abra o **Command Palette** (`Ctrl+Shift+P` ou `⇧⌘P`).
    
2. Digite e selecione **Git: Undo Last Commit**.
    
3. Isto desfaz o commit mais recente, mantendo as alterações no diretório de trabalho para que possa ajustá-las e commitar de novo ([Visual Studio Code](https://code.visualstudio.com/docs/sourcecontrol/overview?utm_source=chatgpt.com "Using Git source control in VS Code")).
    

### 2.2 Reverter Commit com GitLens

Se usar a extensão **GitLens**:

1. Na aba **Source Control**, expanda **COMMITS**.
    
2. Clique com o botão direito no commit que deseja reverter.
    
3. Selecione **Revert Commit**.
    
4. Empurre a nova alteração para o remoto. ([GitKraken](https://www.gitkraken.com/learn/git/problems/revert-git-commit?utm_source=chatgpt.com "Git Revert Commit | Solutions to Git Problems - GitKraken")).
    

### 2.3 Reset “Hard” via Terminal Integrado

O VS Code não tem, por padrão, uma opção gráfica para `git reset --hard` ([Stack Overflow](https://stackoverflow.com/questions/74144236/is-there-any-way-to-git-reset-hard-in-the-vscode-app?utm_source=chatgpt.com "Is there any way to \"git reset --hard\" in the VSCode app?")). Para um reset completo:

1. Abra o **Terminal Integrado** (`Ctrl+``).
    
2. Execute:
    
    ```bash
    git fetch origin
    git reset --hard origin/SEU_BRANCH
    ```
    
    Isto alinha o seu branch local ao remoto, descartando todas as alterações não confirmadas ([Stack Overflow](https://stackoverflow.com/questions/1628088/reset-local-repository-branch-to-be-just-like-remote-repository-head?utm_source=chatgpt.com "Reset local repository branch to be just like remote repository HEAD")).
    

### 2.4 Recuperar Commits via Reflog

Se precisar de voltar a um commit antigo perdido:

1. No terminal integrado, execute:
    
    ```bash
    git reflog
    ```
    
2. Identifique o ponto desejado (ex.: `HEAD@{3}`).
    
3. Redefina o branch:
    
    ```bash
    git reset --hard HEAD@{3}
    ```
    
    ou crie um novo branch a partir daí:
    
    ````bash
    git branch nome-recuperado HEAD@{3}
    git checkout nome-recuperado
    ``` :contentReference[oaicite:10]{index=10}.
    ````
    

---

## 3. Dicas Finais

- **Proteja-se** criando antes um branch de backup:
    
    ````bash
    git branch backup-antes-reset
    ``` :contentReference[oaicite:11]{index=11}.
    ````
    
- Em branches partilhados, prefira **revert** a **reset**, para não reescrever histórico.
    
- Se precisar de UI para **reset** hard e não quiser usar o terminal, considere o **GitHub Desktop** ([GitHub](https://github.com/orgs/community/discussions/60023?utm_source=chatgpt.com "Revert commit on Website · community · Discussion #60023 - GitHub")).
    
- Para explorações temporárias, use `git checkout <hash>` em modo **detached HEAD**, criando depois um branch se necessário ([Reddit](https://www.reddit.com/r/git/comments/1cx2tjl/in_github_synced_my_fork_with_upstream_using_the/?utm_source=chatgpt.com "In gitHub Synced my fork with upstream using the UI, how to undo ...")).
    

Com estas abordagens, você consegue controlar o estado dos seus branches tanto no GitHub quanto no VS Code, escolhendo entre desfazer commits, reverter ou resetar de forma segura.