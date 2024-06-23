## Module Bundlers Tutorial: Webpack and Rollup

This tutorial will guide you through bundling a JavaScript library using Webpack and Rollup. We will start from the basics and progress to advanced configurations such as tree-shaking, code splitting, and output optimization.

### Table of Contents

1. Introduction to Module Bundlers
2. Setting Up Webpack
    1. Basic Configuration
    2. Advanced Features
        - Tree-shaking
        - Code Splitting
        - Optimizing Output
3. Setting Up Rollup
    1. Basic Configuration
    2. Advanced Features
        - Tree-shaking
        - Code Splitting
        - Optimizing Output
4. Conclusion

---

## 1. Introduction to Module Bundlers

Module bundlers are tools that take JavaScript files and their dependencies and bundle them into a single file (or a few files) that can be included in a webpage. The main purpose is to reduce the number of HTTP requests and manage dependencies more effectively. Two of the most popular bundlers are Webpack and Rollup.

- **Webpack**: A highly configurable module bundler that supports advanced features like tree-shaking and code splitting.
- **Rollup**: Known for its simplicity and efficiency, Rollup is great for bundling libraries and offers excellent tree-shaking capabilities.

---

## 2. Setting Up Webpack

### 2.1 Basic Configuration

First, let's set up a basic Webpack configuration.

1. **Initialize a new project:**

```sh
mkdir my-library
cd my-library
npm init -y
```

2. **Install Webpack and its CLI:**

```sh
npm install --save-dev webpack webpack-cli
```

3. **Create the project structure:**

```sh
mkdir src dist
touch src/index.js webpack.config.js
```

4. **Add a basic configuration to `webpack.config.js`:**

```js
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
        },
      },
    ],
  },
};
```

5. **Install Babel and its loader:**

```sh
npm install --save-dev babel-loader @babel/core @babel/preset-env
```

6. **Create a Babel configuration file `.babelrc`:**

```json
{
  "presets": ["@babel/preset-env"]
}
```

7. **Add a sample code to `src/index.js`:**

```js
const message = 'Hello, Webpack!';
console.log(message);
```

8. **Build the project:**

```sh
npx webpack
```

### 2.2 Advanced Features

#### Tree-shaking

Tree-shaking is the process of eliminating dead code. Webpack supports tree-shaking when using ES6 modules.

1. **Update the Webpack configuration to enable tree-shaking:**

```js
module.exports = {
  mode: 'production', // Enables tree-shaking
  // ... rest of the config
};
```

2. **Ensure that your code uses ES6 modules:**

```js
// src/utils.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;

// src/index.js
import { add } from './utils';

console.log(add(2, 3));
```

#### Code Splitting

Code splitting allows you to split your code into various bundles which can be loaded on demand.

1. **Update `webpack.config.js` to configure code splitting:**

```js
module.exports = {
  // ... rest of the config
  optimization: {
    splitChunks: {
      chunks: 'all',
    },
  },
};
```

2. **Dynamic import in `src/index.js`:**

```js
// src/index.js
import('./utils').then(({ add }) => {
  console.log(add(2, 3));
});
```

#### Optimizing Output

1. **Minification**: Webpack uses Terser for minification by default in production mode.

2. **Define plugin**: Define global constants to avoid redundant code.

```sh
npm install --save-dev webpack.DefinePlugin
```

```js
const webpack = require('webpack');

module.exports = {
  // ... rest of the config
  plugins: [
    new webpack.DefinePlugin({
      'process.env.NODE_ENV': JSON.stringify('production'),
    }),
  ],
};
```

---

## 3. Setting Up Rollup

### 3.1 Basic Configuration

1. **Initialize a new project:**

```sh
mkdir my-library-rollup
cd my-library-rollup
npm init -y
```

2. **Install Rollup and its CLI:**

```sh
npm install --save-dev rollup
```

3. **Create the project structure:**

```sh
mkdir src dist
touch src/index.js rollup.config.js
```

4. **Add a basic configuration to `rollup.config.js`:**

```js
export default {
  input: 'src/index.js',
  output: {
    file: 'dist/bundle.js',
    format: 'cjs',
  },
};
```

5. **Install Babel and its plugin for Rollup:**

```sh
npm install --save-dev @rollup/plugin-babel @babel/core @babel/preset-env
```

6. **Update `rollup.config.js` to use Babel:**

```js
import babel from '@rollup/plugin-babel';

export default {
  input: 'src/index.js',
  output: {
    file: 'dist/bundle.js',
    format: 'cjs',
  },
  plugins: [
    babel({
      exclude: 'node_modules/**',
      presets: ['@babel/preset-env'],
    }),
  ],
};
```

7. **Add a sample code to `src/index.js`:**

```js
const message = 'Hello, Rollup!';
console.log(message);
```

8. **Build the project:**

```sh
npx rollup -c
```

### 3.2 Advanced Features

#### Tree-shaking

Tree-shaking is inherent in Rollup due to its design around ES6 modules.

1. **Ensure your code uses ES6 modules:**

```js
// src/utils.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;

// src/index.js
import { add } from './utils';

console.log(add(2, 3));
```

#### Code Splitting

Rollup supports code splitting for ES modules.

1. **Update `rollup.config.js` for multiple outputs:**

```js
export default {
  input: ['src/index.js', 'src/anotherModule.js'],
  output: {
    dir: 'dist',
    format: 'esm',
  },
};
```

#### Optimizing Output

1. **Minification**: Use the Terser plugin for minification.

```sh
npm install --save-dev rollup-plugin-terser
```

```js
import { terser } from 'rollup-plugin-terser';

export default {
  input: 'src/index.js',
  output: {
    file: 'dist/bundle.js',
    format: 'cjs',
  },
  plugins: [
    babel({
      exclude: 'node_modules/**',
      presets: ['@babel/preset-env'],
    }),
    terser(),
  ],
};
```

---

## 4. Conclusion

By following this tutorial, you have learned how to set up and configure both Webpack and Rollup for bundling JavaScript libraries. Starting with basic configurations, we progressed to advanced features like tree-shaking, code splitting, and output optimization. Each tool has its strengths, and the choice between them depends on the specific requirements of your project.