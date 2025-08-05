#computer_science #frontend #deploy #guide 

# 📦 Complete Guide to All npm Commands (2025 Edition)

This document provides an exhaustive, categorized explanation of all major `npm` CLI commands, including their descriptions, usage, flags, and best practices — tailored for modern JavaScript/Next.js development.

---

## 🔹 1. Dependency Management

### `npm install` / `npm i`

Install dependencies from `package.json` or specific packages.

```bash
npm install          # Install all listed dependencies
npm install lodash   # Add lodash to dependencies
npm install -g nodemon  # Global install
npm install --save-dev eslint  # Dev dependency
```

**Flags:**

- `--save-dev`: adds to `devDependencies`
    
- `--global` or `-g`: install globally
    

**Best Practice:**

- Use `--save-dev` for tools not required in production.
    
- Avoid global installs unless tools are CLI utilities.
    

---

### `npm uninstall`

Remove installed packages.

```bash
npm uninstall axios
```

---

### `npm update`

Update packages according to semver.

```bash
npm update
```

---

### `npm outdated`

List outdated packages.

```bash
npm outdated
```

---

### `npm ci`

Clean install from `package-lock.json` (for CI/CD).

```bash
npm ci
```

**Tip:** Faster and safer than `npm install` for automated builds.

---

### `npm dedupe`

Reduce duplication in the dependency tree.

```bash
npm dedupe
```

---

### `npm prune`

Remove extraneous packages not in `package.json`.

```bash
npm prune
```

---

## 🔹 2. Scripts and Automation

### `npm run <script>`

Run a script defined in `package.json`.

```bash
npm run dev
npm run build
```

### `npm start`

Shortcut for `npm run start`

```bash
npm start
```

### `npm test`

Shortcut for `npm run test`

### `npm stop`, `npm restart`

Run associated lifecycle scripts.

---

## 🔹 3. Init and Configuration

### `npm init`

Create a `package.json`.

```bash
npm init
npm init -y  # Auto-fill defaults
```

---

### `npm config`

Manage npm configuration settings.

```bash
npm config get registry
npm config set registry https://registry.npmjs.org/
```

---

### `npm pkg`

Directly manipulate `package.json` values.

```bash
npm pkg get name
npm pkg set name="my-app"
```

---

## 🔹 4. Audit and Security

### `npm audit`

Check for vulnerabilities.

```bash
npm audit
```

### `npm audit fix`

Try to automatically fix issues.

```bash
npm audit fix
```

**Tip:** Be cautious with `npm audit fix` — can break if major versions are upgraded automatically.

---

## 🔹 5. Info and Debugging

### `npm list`

Show installed packages.

```bash
npm list
npm list --depth=0  # Only top-level
```

### `npm explain <package>`

Explain why a package is installed.

```bash
npm explain react
```

### `npm doctor`

Diagnose common issues.

```bash
npm doctor
```

---

## 🔹 6. User & Auth

### `npm login`

Authenticate to publish packages.

### `npm logout`

End session.

### `npm whoami`

Display logged-in user.

---

## 🔹 7. Publish & Share

### `npm publish`

Publish a package to the registry.

```bash
npm publish
```

### `npm unpublish`

Remove a published package.

```bash
npm unpublish my-package --force
```

### `npm version`

Bump the version.

```bash
npm version patch
npm version minor
npm version major
```

---

## 🔹 8. Cache Management

### `npm cache verify`

Verify integrity of the cache.

### `npm cache clean --force`

Clear npm cache (be careful).

---

## 🔹 9. Miscellaneous Utilities

### `npm view <pkg>` / `npm info`

View registry info.

```bash
npm view react version
```

### `npm bin`

Show bin folder path.

### `npm prefix`

Show the root project folder.

### `npm fund`

Show funding links for dependencies.

### `npm search <term>`

Search packages (limited via CLI).

---

## 🔹 10. Advanced/Org

### `npm org`, `npm team`, `npm access`, `npm token`, `npm profile`

Manage orgs, teams, permissions, tokens.

---

## ✅ Best Practices for Pro Developers

- Use `npm ci` in CI/CD pipelines for reproducibility.
    
- Lock versions using `package-lock.json`.
    
- Keep dev tools in `devDependencies`.
    
- Run `npm audit` before deployments.
    
- Avoid unnecessary global installs.
    
- Pin dependencies with care — semantic versioning matters.
    

---

## 📚 References

- [npm CLI Docs](https://docs.npmjs.com/cli/v10/commands)
    
- [npm Scripts Docs](https://docs.npmjs.com/misc/scripts)
    

---

Let me know if you'd like a visual cheat sheet, Obsidian version, or PDF export.