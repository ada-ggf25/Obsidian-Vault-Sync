#git #github #computer_science 

On Github if I creat in my branch (not main) and then do PR to the main. Does the label cronology gets updated with the new label I created in my branch?

### ChatGPT said:

Thought for 20 seconds

No – GitHub labels aren’t carried along on a per-branch basis and merging a branch into main does not itself create or modify the repository’s labels. Labels live at the repository level, not in any particular branch, and the “label history” (the events you see in an issue or PR’s timeline) only reflects actual label-add/remove actions performed via the GitHub API or UI.

- Labels are global to the repository: once you create a label (whether via the web UI, the REST/GraphQL API, or a label-sync GitHub Action), it exists everywhere in that repo. You don’t “commit” labels in a branch the way you commit code; you have to explicitly create or update them at the repo level. [GitHub Docs](https://docs.github.com/en/issues/using-labels-and-milestones-to-track-work/managing-labels?utm_source=chatgpt.com)
- The pull request merge only brings code (and any files you’ve changed) into main; it does not invoke GitHub’s label machinery or generate new label events. Only when you add or remove a label on an issue or PR will the timeline (“chronology”) show a label-added or label-removed event. [GitHub Docs](https://docs.github.com/articles/creating-a-pull-request?utm_source=chatgpt.com)

If you want to keep labels in sync across branches or repos via code, you’ll need to use a tool—such as the EndBug/label-sync action or the GitHub CLI—to explicitly create or update labels from a JSON/YAML config as part of your workflow.

Sources

[](https://www.google.com/s2/favicons?domain=https://docs.github.com&sz=32)