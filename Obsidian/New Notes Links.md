#obsidian #guide

Is It possible in Obsidian for every time I generate a note inside another note by clicking in a link that I have just wroten to another page that until that moment it didn't existed, for the note that is generated to be forced to appear inside the folder that the note where I wrote the link that generated the new note is all ready in is, instead of out of that folder in the global enviorment of my obsidian vault?

### ChatGPT said:

Thought for 21 seconds

## Summary

Yes—it’s possible to have Obsidian place new notes created via links into the same folder as the note you’re editing. You can achieve this by adjusting Obsidian’s **core setting** for new note location, by using **relative-path link syntax**, or by leveraging **community plugins** that offer more dynamic control. Below, you’ll find step-by-step guidance for each approach, along with the trade-offs and practical tips.

---

## 1. Core Setting: “Same folder as current file”

Obsidian’s built-in behavior for where new notes land is controlled under **Settings → Files & Links → Default location for new notes**. By default this is often set to a global “Inbox” or vault root, but you can switch it:

- Open **Settings** → **Files & Links**.
    
- Under **Default location for new notes**, select **Same folder as the current file**.
    
    This makes any new note—whether created via the “New note” command or by clicking a non-existent `[[link]]`—automatically appear in the folder of the note containing the link [Obsidian ForumObsidian Forum](https://forum.obsidian.md/t/how-can-i-create-a-new-note-in-the-current-folder-with-bracket-linking/72970?utm_source=chatgpt.com).
    

---

## 2. Relative-Path Link Syntax

If you want per-link control without changing the global default, you can explicitly specify a relative path in the link itself:

- Use `./` to denote “this folder.”
- Example: if you’re editing `Projects/ClientA/Overview.md` and you type `[[./Budget]]`, clicking to create “Budget” will force the new file into `Projects/ClientA/Budget.md` [Obsidian Forum](https://forum.obsidian.md/t/absolute-or-relative-path-default-location-for-new-notes-should-be-ignored-when-creating-files-by-following-a-link-the-link-itself-should-be-used/41743?utm_source=chatgpt.com).

This method neatly overrides the global default only for links where you include `./`, giving you granular placement without navigating menus.

---

## 3. Plugin-Based Approaches

For more advanced or automated workflows, consider one of these community plugins:

### 3.1 Note Auto Creator

SimonTC’s “Note Auto Creator” plugin lets you create new notes via a custom trigger (default `@`), with drop-down suggestions that will automatically generate the note in your desired folder and apply a template if you wish [GitHub](https://github.com/SimonTC/obsidian-note-autocreation?utm_source=chatgpt.com).

### 3.2 Create Note in Folder

The “Create Note in Folder” plugin provides a dialog for choosing a folder on-the-fly whenever you create a note. You can bind it to a hotkey and it will place the new note exactly where you pick, including the current folder if you select it [Obsidian Stats](https://www.obsidianstats.com/plugins/create-note-in-folder?utm_source=chatgpt.com).

### 3.3 Obsidian URI Scheme

Advanced users can leverage the **obsidian://new** URI with path parameters to script note creation in any specific folder. For instance:

```
pgsql
CopyEdit
obsidian://new?vault=MyVault&path=Projects/ClientA/NewNote.md

```

This method also supports templating and appending content via URL parameters for automation [Obsidian Forum](https://forum.obsidian.md/t/create-file-inside-a-folder-by-expliciting-in-the-name-folder-file/27753?utm_source=chatgpt.com).

---

## 4. Workarounds & Limitations

- **Right-click + Move**: After creating a note via `[[link]]`, you can right-click it in the file tree and choose **Move file…**. This is built-in but adds extra steps [Obsidian Forum](https://forum.obsidian.md/t/create-a-link-to-a-new-file-in-a-different-folder/15437?utm_source=chatgpt.com)[Obsidian Forum](https://forum.obsidian.md/t/choose-a-folder-whenever-a-new-note-is-created-from-a-link/23177?utm_source=chatgpt.com).
- **Default setting conflict**: If you already rely on a non-“same folder” default (e.g., an “Inbox” for general notes), switching the core setting may disrupt that workflow. In that case, use the **relative-path syntax** or a **plugin**.

---

### TL;DR

1. **Simple**: Set **Default location for new notes** to **Same folder as current file** in Settings [Obsidian Forum](https://forum.obsidian.md/t/how-can-i-create-a-new-note-in-the-current-folder-with-bracket-linking/72970?utm_source=chatgpt.com).
2. **Granular**: Use `[[./Note Title]]` to force creation in the current folder [Obsidian Forum](https://forum.obsidian.md/t/absolute-or-relative-path-default-location-for-new-notes-should-be-ignored-when-creating-files-by-following-a-link-the-link-itself-should-be-used/41743?utm_source=chatgpt.com).
3. **Advanced**: Install a plugin like **Note Auto Creator** or **Create Note in Folder** for on-demand folder selection [GitHub](https://github.com/SimonTC/obsidian-note-autocreation?utm_source=chatgpt.com)[Obsidian Stats](https://www.obsidianstats.com/plugins/create-note-in-folder?utm_source=chatgpt.com).

Sources

[](https://www.google.com/s2/favicons?domain=https://www.obsidianstats.com&sz=32)

[](https://www.google.com/s2/favicons?domain=https://github.com&sz=32)

[](https://www.google.com/s2/favicons?domain=https://forum.obsidian.md&sz=32)