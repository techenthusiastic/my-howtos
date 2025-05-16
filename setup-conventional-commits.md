# 🧾 Conventional Commits Workflow with Yarn, Commitizen, Husky, and Commitlint

This setup ensures that every commit is structured, machine-readable, and developer-friendly, especially useful for teams and CI/CD automation.

This guide sets up a developer-friendly Git workflow using [Conventional Commits](https://www.conventionalcommits.org/), powered by:

- [`Commitizen`](https://github.com/commitizen/cz-cli) – interactive commit prompt for writing standardized commits
- [`cz-conventional-changelog`](https://github.com/commitizen/cz-conventional-changelog) – Commitizen adapter for Conventional Commits
- [`Husky`](https://typicode.github.io/husky) – to trigger Git hooks
- [`Commitlint`](https://commitlint.js.org) – to validate commit messages

This ensures every commit is consistent, readable, and compatible with changelog automation and semantic versioning.

---

## ✍️ What Are Conventional Commits?

Conventional Commits use a consistent structure to describe changes:

```text
<type>[optional scope]: <description>

[optional body]

[optional footer]
```

Example:

```bash
feat(login): add remember me checkbox
```

Where:

- type is feat, fix, chore, etc.
- scope is optional (e.g. login)
- description is a short summary of the change

---

### Prerequisites

- Node.js installed
- yarn installed

---

## 🛠️ Step-by-Step Setup

### 1. Initialize your project

```bash
yarn init -y
```

### 2. Install Commitizen and the Adapter

```bash
yarn add -D commitizen cz-conventional-changelog
```

#### I. Configure Commitizen

Add this to your package.json:

```bash
"config": {
  "commitizen": {
    "path": "./node_modules/cz-conventional-changelog"
  }
}
```

This tells Commitizen to use the cz-conventional-changelog adapter — the adapter that enforces the Conventional Commits format.

#### II. Add a commit script to package.json

Add a commit script to invoke commitizen:

```bash
"scripts": {
  "commit": "cz"
}
```

### 2. Install Husky (for Git hooks)

```bash
yarn add -D husky
```

#### I. Setup boilerplate for husky

```bash
npx husky init
```

- automatically sets up scripts for git hooks execution
- automatically adds "[`prepare`](https://docs.npmjs.com/cli/v8/using-npm/scripts#life-cycle-scripts)": "husky" in scripts of package.json

###### Guidelines:

- make no changes in ./husky/\_/\* as this contains initial script for git hook to work
- don't confuse git hooks in ./husky/\_/\* for real hook they are just scripts helping husky to run git hooks
- new git hooks can be added at .husky/{hook_name}; [`reference`](https://typicode.github.io/husky/how-to.html)

### 3. Add Commitlint to Enforce Message Format

If you want to enforce commit message linting, add @commitlint/config-conventional and @commitlint/cli:

```bash
yarn add -D @commitlint/{cli,config-conventional}
```

#### I. Configure commitlint

Add commitlint to your package.json scripts:

```bash
"scripts": {
  "commitlint": "commitlint --edit"
}
```

Create a commitlint.config.js:

```bash
module.exports = { extends: ['@commitlint/config-conventional'] };
```

#### II. Setup commitlint to run via git hooks

Then add the commit-msg hook:

```bash
echo 'yarn commitlint --edit "$1"' > .husky/commit-msg
```

Now, if someone writes a bad commit message (not using conventional format), the commit will be blocked.

### 8. Usage

To commit use

```bash
yarn commit
```

which will trigger the interactive commit prompt.
