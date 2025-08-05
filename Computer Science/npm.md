#computer_science #frontend #deploy #guide 

[[npm Commands]]

Absolutely! Here's a **comprehensive guide to `npm`** — what it is, how it works, and a detailed list of commands (with usage examples). This will be especially useful for developing and managing projects like your **Next.js web app**.

---

## 🧠 What is `npm`?

**`npm` stands for Node Package Manager**. It is the default package manager for the **Node.js runtime environment**. It helps developers:

- Install **packages (libraries, modules, tools)**.
    
- Manage **dependencies** for a JavaScript project.
    
- Run **scripts** defined in `package.json`.
    
- Share and publish **open-source packages**.
    

`npm` comes **bundled with Node.js**. To check your installed version:

```bash
node -v
npm -v
```

---

## 📦 What is a Package?

A **package** is any folder containing a `package.json` file. This file defines:

- The **metadata** of the project (name, version, description).
    
- The **dependencies** (other npm packages your project uses).
    
- The **scripts** (like `npm run dev` for starting your app).
    
- The **entry point** (e.g. `index.js`).
    

Example `package.json`:

```json
{
  "name": "my-nextjs-app",
  "version": "1.0.0",
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "next": "13.4.0",
    "react": "18.2.0",
    "react-dom": "18.2.0"
  }
}
```

---

## 📥 Installing and Managing Packages

### Installing a Package

```bash
npm install <package-name>
```

Example:

```bash
npm install axios
```

By default, it adds the package to `dependencies`.

### Installing a Package as a Dev Dependency

```bash
npm install <package-name> --save-dev
```

Example:

```bash
npm install eslint --save-dev
```

---

## 📂 Dependency Types

|Type|Location in `package.json`|Use case|
|---|---|---|
|`dependencies`|`"dependencies"`|Needed to run the app|
|`devDependencies`|`"devDependencies"`|Needed only for development|
|`peerDependencies`|`"peerDependencies"`|Required by library users|

---

## ⚙️ Common `npm` Files

- `package.json`: Project configuration file
    
- `package-lock.json`: Exact version tree of dependencies
    
- `node_modules/`: All installed packages
    

---

## 🔧 Core `npm` Commands

### 🔹 `npm init`

Initialises a new project and creates a `package.json`.

```bash
npm init
# or quick mode:
npm init -y
```

---

### 🔹 `npm install` (or `npm i`)

- Installs all dependencies listed in `package.json`
    
- Installs a specific package
    

```bash
npm install
npm install react
```

---

### 🔹 `npm uninstall`

Removes a package.

```bash
npm uninstall lodash
```

---

### 🔹 `npm update`

Updates packages to the latest version that matches the semver in `package.json`.

```bash
npm update
```

---

### 🔹 `npm outdated`

Lists packages that are outdated.

---

### 🔹 `npm run`

Runs a script defined in `package.json`.

```bash
npm run dev
npm run build
npm run start
```

---

### 🔹 `npm list`

Shows the dependency tree.

```bash
npm list
npm list --depth=0  # Only top-level
```

---

### 🔹 `npm audit`

Scans for vulnerabilities in dependencies.

```bash
npm audit
npm audit fix
```

---

### 🔹 `npm cache`

Manages the npm cache.

```bash
npm cache verify
npm cache clean --force
```

---

### 🔹 `npm config`

Get and set npm configuration.

```bash
npm config get registry
npm config set registry https://registry.npmjs.org/
```

---

### 🔹 `npm whoami`

Shows your logged-in npm user (if publishing packages).

---

### 🔹 `npm login / logout`

For authentication when publishing packages.

---

### 🔹 `npm publish`

Publishes a package to the npm registry.

---

### 🔹 `npm version`

Bumps version in `package.json`.

```bash
npm version patch
npm version minor
npm version major
```

---

### 🔹 `npm ci`

Clean install from `package-lock.json` (faster and reproducible). Great for CI/CD.

```bash
npm ci
```

---

## 📃 Complete List of All `npm` Commands (v10+)

|Command|Description|
|---|---|
|`npm access`|Set access level on published packages|
|`npm adduser`|Create or log in to an npm user account|
|`npm audit`|Run security audit|
|`npm bin`|Show npm binaries path|
|`npm bugs`|Show bug tracker for a package|
|`npm cache`|Manage npm cache|
|`npm ci`|Clean install from lock file|
|`npm completion`|Enable shell autocompletion|
|`npm config`|Manage npm config|
|`npm dedupe`|Reduce duplication in `node_modules/`|
|`npm deprecate`|Deprecate a published version|
|`npm dist-tag`|Manage distribution tags|
|`npm docs`|Open package documentation|
|`npm doctor`|Check your environment for problems|
|`npm edit`|Edit a package locally|
|`npm exec`|Run commands in the context of local packages|
|`npm explain`|Explain dependency tree|
|`npm explore`|Open a shell in a package directory|
|`npm fund`|Show funding information|
|`npm help`|Show help|
|`npm hook`|Manage webhooks|
|`npm init`|Initialise a project|
|`npm install`|Install packages|
|`npm link`|Symlink a package|
|`npm login`|Log in to npm|
|`npm logout`|Log out from npm|
|`npm ls`|List installed packages|
|`npm org`|Manage organisations|
|`npm outdated`|Show outdated packages|
|`npm owner`|Manage package owners|
|`npm pack`|Create a tarball of your package|
|`npm ping`|Ping the registry|
|`npm pkg`|Manipulate `package.json` directly|
|`npm prefix`|Show the root directory of current project|
|`npm profile`|Manage your profile|
|`npm prune`|Remove extraneous packages|
|`npm publish`|Publish your package|
|`npm rebuild`|Rebuild native addons|
|`npm repo`|Open package repo|
|`npm restart`|Run `restart` script|
|`npm root`|Show `node_modules/` root|
|`npm run`|Run scripts from `package.json`|
|`npm search`|Search for packages|
|`npm set`|Set npm config values|
|`npm shrinkwrap`|Lock down dependency versions|
|`npm star`|Star a package|
|`npm stars`|View starred packages|
|`npm start`|Run `start` script|
|`npm stop`|Run `stop` script|
|`npm test`|Run `test` script|
|`npm token`|Manage access tokens|
|`npm uninstall`|Uninstall a package|
|`npm unpublish`|Remove a package from the registry|
|`npm update`|Update installed packages|
|`npm version`|Bump version and create git tag|
|`npm view`|View registry info|
|`npm whoami`|Show current user|

---

## 🧪 Common `npm` Scripts in Next.js

|Script|Purpose|
|---|---|
|`npm run dev`|Launches development server|
|`npm run build`|Builds production bundle|
|`npm run start`|Starts the production server|
|`npm run lint`|Runs ESLint if configured|
|`npm run test`|Runs unit/integration tests|

---

## 📚 Learning More

- 📘 [npm Docs](https://docs.npmjs.com/)
    
- 🛠️ [Node.js API](https://nodejs.org/api/)
    
- 🔧 Explore your `node_modules/` folder — it helps understanding dependency trees
    

---

Would you like me to create a printable PDF version of this guide or generate a cheatsheet tailored for your current Next.js project?