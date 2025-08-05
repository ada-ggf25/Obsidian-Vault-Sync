#computer_science #github #git #guide 

When I do a commit in my separate brunch the commit only appears on my green wall of commits history in my github profile when that commit goes to the "main" brunch?

### ChatGPT said:

Thought for 10 seconds

## Summary

GitHub’s contributions graph (the “green squares” on your profile) only counts commits that meet specific criteria. In particular, commits must be made on the repository’s default branch (usually `main` or `master`) or its `gh‑pages` branch, use an email address associated with your GitHub account, and be in a standalone (non‑fork) repository [GitHub Docs](https://docs.github.com/articles/why-are-my-contributions-not-showing-up-on-my-profile?utm_source=chatgpt.com). Therefore, if you commit to a separate feature branch, those commits won’t show up on your contributions graph until they are merged into the default branch (or you change the repository’s default branch to your feature branch) [Stack Overflow](https://stackoverflow.com/questions/49586318/github-why-when-i-push-to-specific-branch-does-not-recorded-in-my-contribution?utm_source=chatgpt.com).

---

## How GitHub Counts Contributions

### 1. Branch Scope

- **Default branch**: Only commits on the repository’s default branch (commonly named `main` or `master`) are counted.
- **gh‑pages branch**: Commits to the `gh‑pages` branch also count, but only for repositories serving a GitHub Pages site [GitHub Docs](https://docs.github.com/articles/why-are-my-contributions-not-showing-up-on-my-profile?utm_source=chatgpt.com).

### 2. Email Association

- The commit author’s email must be associated with your GitHub account (check with `git config user.email`) [GitHub](https://github.com/orgs/community/discussions/155455?utm_source=chatgpt.com).

### 3. Repository Type

- The repository must be standalone (not a fork). Commits to forks only count if you open a pull request and have it merged into the parent repository [GitHub Docs](https://docs.github.com/articles/why-are-my-contributions-not-showing-up-on-my-profile?utm_source=chatgpt.com).

---

## Why Your Feature‑Branch Commits Don’t Appear

When you commit on a non‑default branch:

1. **Not on default**: GitHub ignores those commits for the contributions graph until they’re merged into the default branch.
2. **No automatic back‑port**: Even if the branch exists in your own repo, GitHub doesn’t count it unless you merge or change settings.

Multiple community threads confirm this behavior as long‑standing (10+ years) and by design [GitHub](https://github.com/orgs/community/discussions/65920?utm_source=chatgpt.com).

---

## Making Your Commits Count

To have your feature‑branch work show up on your profile:

1. **Open a Pull Request**
    
    Merge your feature branch into `main` (or your repo’s default branch). Once merged, each commit in that branch will appear on your contributions graph with its original commit date [Reddit](https://www.reddit.com/r/github/comments/x5q26l/github_not_showing_commits_to_non_main_branch/?utm_source=chatgpt.com).
    
2. **Change the Default Branch**
    
    In the repository’s **Settings → Branches**, set your feature branch as the default. After doing so, commits to that branch are treated as if they were to `main`.
    
    > ⚠️ This affects all collaborators and pull‑request bases for the repo GitHub Docs.
    
3. **Wait for Propagation**
    
    GitHub may take up to 24 hours to refresh your contributions graph after commits meet all criteria [Reddit](https://www.reddit.com/r/github/comments/x5q26l/github_not_showing_commits_to_non_main_branch/?utm_source=chatgpt.com).
    

---

## Additional Tips

- **Private Repositories**: To show private‑repo contributions, enable “Show private contributions” in your profile settings. Details remain anonymized visually.
- **Time Zones**: Contributions are timestamped in UTC; ensure your commit times align if you’re near midnight UTC.
- **SSO and Organization Repos**: If you belong to an organization requiring SAML SSO, ensure your session is active or contributions may not register [GitHub](https://github.com/orgs/community/discussions/155455?utm_source=chatgpt.com).

---

By following these guidelines, you’ll ensure that all your hard work on feature branches is reflected on your GitHub profile once it’s merged or appropriately configured.