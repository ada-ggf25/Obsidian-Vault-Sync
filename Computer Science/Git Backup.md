#computer_science #github #git 

## Resumo

Para restaurar o estado do branch em que está a trabalhar usando o seu último commit como “backup”, pode usar principalmente o comando `git reset` para descartar alterações locais e, caso precise de recuperar commits antigos, o `git reflog`. Em poucos passos, volta ao ponto exato do último commit confirmado, seja para desfazer alterações no diretório de trabalho ou mesmo para reavivar um branch que tenha sido movido ou apagado inadvertidamente.

---

## 1. Descartar alterações locais e voltar ao último commit

Se o objetivo é simplesmente que o seu branch volte exatamente ao estado do último commit (descartando todas as alterações não commitadas), utilize:

```bash
git reset --hard HEAD
```

- `--hard` faz com que o **HEAD**, o índice (staging area) e o diretório de trabalho fiquem iguais ao estado do commit apontado (neste caso, `HEAD`, ou seja, o último commit) ([Atlassian](https://www.atlassian.com/git/tutorials/undoing-changes/git-reset?utm_source=chatgpt.com "Git Reset | Atlassian Git Tutorial"), [Git](https://git-scm.com/book/ms/v2/Git-Tools-Reset-Demystified?utm_source=chatgpt.com "Reset Demystified - Git")).
    
- **Atenção**: todas as modificações não commitadas serão perdidas.
    

---

## 2. Recuperar commits ou branches “perdidos” com o reflog

Caso tenha feito reset, checkout ou até mesmo apagado um branch e queira encontrar commits antigos, o **reflog** guarda um histórico de onde o `HEAD` andou:

1. Liste o histórico de referências:
    
    ```bash
    git reflog
    ```
    
2. Identifique o commit desejado (p. ex., `HEAD@{3}` ou um hash específico) ([Git](https://git-scm.com/docs/git-reflog?utm_source=chatgpt.com "git-reflog Documentation - Git")).
    
3. Para restaurar o branch a esse ponto:
    
    ```bash
    git reset --hard HEAD@{3}
    ```
    
    ou, caso o branch tenha sido apagado:
    
    ```bash
    git branch nome-do-branch <hash_do_commit>
    git checkout nome-do-branch
    ```
    

Isto permite “voltar no tempo” e continuar a trabalhar normalmente a partir de um estado anterior.

---

## Exemplo de fluxo completo

```bash
# 1. Certifique-se de estar no branch correto
git checkout meu-branch

# 2. Descartar tudo até ao último commit
git reset --hard HEAD

# 3. (Opcional) Se tiver apagado algo ou feito um reset errado, recupere pelo reflog:
git reflog
# supondo que o estado desejado esteja em HEAD@{2}
git reset --hard HEAD@{2}

# 4. (Opcional) Se o branch foi removido:
git branch meu-branch-recuperado HEAD@{2}
git checkout meu-branch-recuperado
```

---

## Dicas adicionais

- **Verificar antes de resetar**: use `git status` e `git diff` para confirmar o que será perdido.
    
- **Branch temporário de segurança**: se tem receio de perder algo, crie antes um branch de backup:
    
    ```bash
    git branch backup-antes-reset
    ```
    
- **Evite em branches partilhados**: `git reset --hard` reescreve histórico e deve ser evitado em branches já partilhados com outros colaboradores.
    
- **Revert para commits “publicados”**: se o commit já foi pushado a um repositório remoto e partilhado, prefira `git revert` para criar um novo commit de “undo” em vez de reescrever histórico ([Atlassian](https://www.atlassian.com/git/tutorials/undoing-changes/git-revert?utm_source=chatgpt.com "Git Revert | Atlassian Git Tutorial")).
    

Com estes passos, consegue restaurar de forma segura o seu branch ao estado do último commit.