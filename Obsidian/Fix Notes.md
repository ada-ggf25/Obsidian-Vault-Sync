#obsidian #guide

Below is a summary of the main ways people achieve “pin-and-sort” behaviour in Obsidian’s file explorer, followed by step-by-step instructions for each approach.

## Summary of Options

- **Filename prefix trick** – If you sort by **Name**, giving key notes a special prefix (e.g. `!`, `_`, or `0-`) will make them always sit at the top of the folder. [Sebastian Witowski](https://switowski.com/blog/obsidian/?utm_source=chatgpt.com)
- **Custom Sort plugin** – A community plugin that lets you define arbitrary ordering rules (YAML-based or drag-and-drop) on a per-folder basis. [Obsidian Forum](https://forum.obsidian.md/t/new-plugin-obsidian-custom-sort/42706?utm_source=chatgpt.com)[GitHub](https://github.com/SebastianMC/obsidian-custom-sort?utm_source=chatgpt.com)
- **File Tree Alternative plugin** – Replaces the core file explorer with an Evernote-style tree that supports “pinning” files at the top of any folder. [Reddit](https://www.reddit.com/r/ObsidianMD/comments/oeegh1/pin_text_filesnotes_to_top_above_folders/?utm_source=chatgpt.com)
- **Manual numbering** – Simply prefix filenames with numbers (e.g. `01-Overview.md`, `02-Tasks.md`) so they sort before the rest when using any alphabetical sort. [Reddit](https://www.reddit.com/r/ObsidianMD/comments/1galgkb/anyways_to_have_custom_sort_order_in_file_tree/?utm_source=chatgpt.com)

---

## 1. Filename-Prefix Trick

1. **Rename** each note you want “pinned” so its name begins with a character that sorts _before_ letters (e.g. `!`, `_`, or `0`).
    
2. In the **File Explorer**, open **Sort options** (⋯ menu in the header) and choose **Sort by name**.
    
3. Those prefixed files will now always appear at the very top of the folder list.
    
    - Many users adopt `!` because it’s visually distinctive:
        
        ```

        !Dashboard.md
        !Today’s Tasks.md
        A-Project Notes.md
        B-Other Notes.md
        
        ```
        
    
    [Sebastian Witowski](https://switowski.com/blog/obsidian/?utm_source=chatgpt.com)
    

### Pros & Cons

- **Pros:** No plugins required; works instantly.
- **Cons:** Requires renaming; inflexible if you later want to reorder beyond simple prefix.

---

## 2. Custom Sort Plugin

The **Custom Sort** (aka Custom File Sorter) plugin gives you full control over explorer order via frontmatter or drag-and-drop:

1. **Install** the plugin from **Settings → Community plugins → Browse**, search for **Custom File Sorter** (or **Custom Sort**).
2. **Enable** it and open its settings to choose per-folder rules.
3. **Specify** an ordering either by:
    - **Drag & drop** within the file tree (if you enable the bookmarks integration).
    - **YAML frontmatter**: add a `sortOrder:` key listing note filenames or glob patterns.

```yaml
yaml
CopiarEditar
---
sortOrder:
  - "🏆 Overview.md"
  - "🚧 In-Progress/*.md"
  - "*.md"
---

```

1. Once configured, the plugin will override the default “Sort by” for folders where rules exist. [Obsidian Forum](https://forum.obsidian.md/t/new-plugin-obsidian-custom-sort/42706?utm_source=chatgpt.com)[GitHub](https://github.com/SebastianMC/obsidian-custom-sort?utm_source=chatgpt.com)

### Pros & Cons

- **Pros:** Fully flexible; supports grouping, wildcards, multiple rules.
- **Cons:** Initial setup can be fiddly; frontmatter editing required for complex setups.

---

## 3. File Tree Alternative Plugin

If you prefer an Evernote-style explorer with true “pin” functionality:

1. **Install** **File Tree Alternative** from the Community plugin browser.
2. Open its settings and **enable** the “pin” feature.
3. **Right-click** any file in the new tree view and choose **Pin to top**.
4. Pinned files stay at the top, regardless of which sort mode you’ve selected for the rest. [Reddit](https://www.reddit.com/r/ObsidianMD/comments/oeegh1/pin_text_filesnotes_to_top_above_folders/?utm_source=chatgpt.com)

### Pros & Cons

- **Pros:** True pinning; no filename hacks.
- **Cons:** Replaces core explorer; UI behaves slightly differently.

---

## 4. Manual Numbering

A low-tech but reliable way is to prefix your key notes with numbers:

```
text
CopiarEditar
01-Home.md
02-Index.md
   Archive-A.md
   Archive-B.md

```

- Simply sort by **Name**, and the numbered files will always lead. [Reddit](https://www.reddit.com/r/ObsidianMD/comments/1galgkb/anyways_to_have_custom_sort_order_in_file_tree/?utm_source=chatgpt.com)

### Pros & Cons

- **Pros:** Immediate; familiar pattern for many knowledge-workers.
- **Cons:** Number management overhead; renaming if you insert new notes mid-sequence.

---

## Which Should You Use?

- **Want zero plugins?** → **Prefix** with `!` or numbers, then sort by name.
- **Need full control (and don’t mind YAML)?** → **Custom Sort** plugin.
- **Prefer genuine “pin” UI?** → **File Tree Alternative** plugin.

Each method lets you keep your “overview” or “dashboard” notes always visible at the top, while leaving the rest of the folder to be sorted (by modified date, name, or any other criterion you choose).
