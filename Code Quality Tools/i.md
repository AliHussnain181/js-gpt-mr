## Code Quality Tools Tutorial: ESLint, Prettier, and Husky

This tutorial will guide you through setting up ESLint for code linting and style consistency, configuring Prettier for code formatting, and using Husky for pre-commit hooks and automated checks. We will start from the basics and move towards advanced configurations.

### Table of Contents

1. Introduction to Code Quality Tools
2. Setting Up ESLint
    1. Installing ESLint
    2. Initializing ESLint
    3. Configuring ESLint
3. Setting Up Prettier
    1. Installing Prettier
    2. Configuring Prettier
    3. Integrating Prettier with ESLint
4. Setting Up Husky
    1. Installing Husky
    2. Configuring Pre-commit Hooks
    3. Running Automated Checks
5. Conclusion

---

## 1. Introduction to Code Quality Tools

Code quality tools are essential for maintaining a clean, consistent, and error-free codebase. ESLint helps in identifying and fixing problems in your JavaScript code, Prettier ensures that your code is formatted consistently, and Husky allows you to run automated checks before committing your code.

---

## 2. Setting Up ESLint

### 2.1 Installing ESLint

1. **Initialize a new project (if not already initialized)**:

```sh
mkdir my-project
cd my-project
npm init -y
```

2. **Install ESLint**:

```sh
npm install --save-dev eslint
```

### 2.2 Initializing ESLint

1. **Run the ESLint initialization command**:

```sh
npx eslint --init
```

2. **Follow the prompts to configure ESLint**:
   - Choose "To check syntax, find problems, and enforce code style".
   - Select the JavaScript modules (import/export).
   - Choose your framework (React, Vue.js, None).
   - Use TypeScript if needed.
   - Choose the style guide (Airbnb, Standard, Google) or none.
   - Decide if you want to use a configuration file format (JavaScript, YAML, JSON).
   - Install the necessary dependencies.

### 2.3 Configuring ESLint

1. **Create or update the `.eslintrc.json` file** (if not already created by `eslint --init`):

```json
{
  "env": {
    "browser": true,
    "es2021": true
  },
  "extends": "eslint:recommended",
  "parserOptions": {
    "ecmaVersion": 12,
    "sourceType": "module"
  },
  "rules": {
    "semi": ["error", "always"],
    "quotes": ["error", "double"]
  }
}
```

2. **Run ESLint on your project**:

```sh
npx eslint src/**/*.js
```

---

## 3. Setting Up Prettier

### 3.1 Installing Prettier

1. **Install Prettier**:

```sh
npm install --save-dev prettier
```

2. **Create a Prettier configuration file `.prettierrc`**:

```json
{
  "semi": true,
  "singleQuote": true,
  "printWidth": 80
}
```

### 3.2 Configuring Prettier

1. **Create a `.prettierignore` file to ignore files and directories**:

```
node_modules
dist
```

2. **Format your code with Prettier**:

```sh
npx prettier --write .
```

### 3.3 Integrating Prettier with ESLint

1. **Install the necessary packages**:

```sh
npm install --save-dev eslint-config-prettier eslint-plugin-prettier
```

2. **Update your `.eslintrc.json` to integrate Prettier**:

```json
{
  "env": {
    "browser": true,
    "es2021": true
  },
  "extends": [
    "eslint:recommended",
    "plugin:prettier/recommended"
  ],
  "parserOptions": {
    "ecmaVersion": 12,
    "sourceType": "module"
  },
  "rules": {
    "semi": ["error", "always"],
    "quotes": ["error", "double"],
    "prettier/prettier": ["error"]
  }
}
```

---

## 4. Setting Up Husky

### 4.1 Installing Husky

1. **Install Husky**:

```sh
npm install --save-dev husky
```

2. **Initialize Husky**:

```sh
npx husky install
```

3. **Add the Husky installation script to `package.json`**:

```json
{
  "scripts": {
    "prepare": "husky install"
  }
}
```

4. **Run the prepare script**:

```sh
npm run prepare
```

### 4.2 Configuring Pre-commit Hooks

1. **Add a pre-commit hook**:

```sh
npx husky add .husky/pre-commit "npx lint-staged"
```

2. **Install `lint-staged`**:

```sh
npm install --save-dev lint-staged
```

3. **Add `lint-staged` configuration to `package.json`**:

```json
{
  "lint-staged": {
    "src/**/*.js": [
      "eslint --fix",
      "prettier --write",
      "git add"
    ]
  }
}
```

### 4.3 Running Automated Checks

1. **Create a sample JavaScript file `src/index.js`**:

```js
const greet = (name) => {
  console.log(`Hello, ${name}!`);
};

greet("World");
```

2. **Make a change and commit it**:

```sh
git add .
git commit -m "Initial commit"
```

The pre-commit hook will run ESLint and Prettier before the commit is completed.

---

## 5. Conclusion

By following this tutorial, you have learned how to set up ESLint for code linting and style consistency, configure Prettier for code formatting, and use Husky for pre-commit hooks and automated checks. These tools will help you maintain a high-quality codebase with consistent style and fewer errors.