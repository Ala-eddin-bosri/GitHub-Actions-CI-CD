# GitHub Actions CI/CD with Git Hooks



## Setup Instructions
### 1. Clone the Repository
```sh
git clone https://github.com/your-username/GitHub-Actions-CI-CD.git
cd GitHub-Actions-CI-CD
```
This command clones the repository and navigates into the project directory.

---

### 2. Initialize a Node.js Project
```sh
npm init -y
```
This initializes a new Node.js project with a `package.json` file.

---

### 3. Install Dependencies
```sh
npm install express
npm install --save-dev jest supertest
```
- `express` - A minimal Node.js web framework.
- `jest` - A JavaScript testing framework.
- `supertest` - A library for HTTP assertions for testing APIs.

---

### 4. Run the Application
```sh
node server.js
```
Starts the Node.js server locally.

---

### 5. Run Tests
```sh
npm test
```
Executes all test cases written with Jest.

---

### 6. Set Up Git Hooks (Pre-commit Hook for Tests)
#### Install Husky & Lint-Staged
```sh
npm install --save-dev husky lint-staged eslint
```
- `husky` - A tool for managing Git hooks.
- `lint-staged` - Runs linters on staged files before committing.
- `eslint` - A tool to check JavaScript code for errors.

#### Initialize Husky
```sh
npx husky install
npm set-script prepare "husky install"
```
This sets up Husky for managing Git hooks.

#### Add a Pre-Commit Hook
```sh
npx husky add .husky/pre-commit "npm test"
```
This creates a pre-commit hook that **runs tests before allowing a commit**.

#### Add Linting to Git Hooks
```json
"lint-staged": {
    "*.js": "eslint --fix"
}
```
This ensures that staged JavaScript files are automatically fixed before committing.

---

### 7. Set Up GitHub Actions Workflow
#### Create a Workflows Directory
```sh
mkdir -p .github/workflows
```
This command creates the directory where GitHub Actions workflow files will be stored.

#### Create the Workflow File
Create a file `.github/workflows/ci-cd.yml` and add the following content:
```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Set Up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: "18"

      - name: Install Dependencies
        run: npm install

      - name: Run Tests
        run: npm test
```
This workflow:
- Runs whenever code is pushed to `main`.
- Checks out the repository.
- Sets up Node.js.
- Installs dependencies.
- Runs tests to verify code integrity.

---



