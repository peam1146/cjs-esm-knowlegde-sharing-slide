---
# You can also start simply with 'default'
theme: dracula
highlighter: shiki
---

# CommonJS, ES Modules, and Bundlers

Introduction to CommonJS, ES Modules, and Bundlers

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
hideToc: true
---

# Table of Contents

<Toc/>

---
---


# Why module systems matter in JavaScript:

> From ChatGPT

- Organize and structure code effectively
- Manage dependencies between different parts of an application
- Enhance code reusability and maintainability
- Improve collaboration among developers
- Control variable scope and prevent naming conflicts

> From Peam

- JavaScript is bullshit!!!

# Overview of today's topics:

1. ESModules: The modern JavaScript module system
2. CommonJS: The traditional Node.js module system
3. Bundlers: Tools that package our code for deployment

---
---

# History and Background of CommonJS

• Aimed to standardize JavaScript modules for server-side development
• Became the default module system for Node.js
• Widely adopted in the npm ecosystem

Syntax: require() and module.exports

```js
// Importing modules:
const module = require('./module-name');

// Exporting modules:
module.exports = { /* exported content */ };

// or
exports.functionName = function() { /* ... */ };
```

---
---

Advantages:

- Simple and intuitive syntax
- Synchronous loading, suitable for server-side
- Dynamic loading of modules at runtime
- Established ecosystem with extensive npm support

Limitations:

- **Not natively supported in browsers**
- **Lack of static analysis for better tree-shaking**
- Synchronous nature can lead to performance issues in certain scenarios
- No built-in support for cyclic dependencies

---
---

# Introduction to ECMAScript Modules

- Introduced in ECMAScript 2015 (ES6)
- Designed to be the standard module system for JavaScript
- Supports both browser and server-side environments
- Provides a more powerful and flexible syntax

```js
// Syntax: import and export

// Importing modules:
import { function, variable } from './module-name.js';
import defaultExport from './module-name.js';

// Exporting modules:
export const variable = value;
export function functionName() { /* ... */ }
export default expression;
```

---
---

Browser Support:

- Modern browsers support ES Modules natively
- Older browsers require transpilation or use of bundlers
- Use type="module" in script tags:

```html
<script type="module" src="main.js"></script>
```

> [Browser compatibility](https://www.w3schools.com/js/js_versions.asp)

---
---

# Dynamic Imports: Flexible Module Loading

- Basic syntax:

```js
import(moduleSpecifier).then(module => {
  // Use the module
}).catch(error => {
  // Handle errors
});

// Using async/await:
async function loadModule() {
  try {
    const module = await import(moduleSpecifier);
    // Use the module
  } catch (error) {
    // Handle errors
  }
}

// Example:
button.addEventListener('click', async () => {
  const { default: myFunction } = await import('./myModule.js');
  myFunction();
});
```

---
---

# Benefits of Dynamic Importing

- Code Splitting:
  - Load modules on-demand, reducing initial bundle size
  - Improve application startup time
- Conditional Loading:
  - Import modules based on runtime conditions
  - Useful for feature flags or A/B testing
- Performance Optimization:
  - Load non-critical modules after initial page load
  - Enhance perceived performance for users
- Reduced Memory Usage:
  - Load modules only when needed, saving memory

---
---

# Benefits of Dynamic Importing

- Lazy Loading:
  - Defer loading of heavy modules until required
  - Useful for large libraries or infrequently used features
- Better Resource Management:
  - Control when network requests for modules are made
  - Optimize bandwidth usage in low-connectivity scenarios
- Enhanced User Experience:
  - Load features progressively as users interact with the app
  - Implement smoother transitions between app states

---
---

# What are bundlers and why use them?

Bundlers are tools that:

- Combine multiple JavaScript files into a single file
- Process and optimize code for production
- Handle dependencies and module systems
- Transform and transpile modern JavaScript for broader browser support
- Manage assets like CSS, images, and fonts

Why use bundlers:

- Improved performance: Reduce HTTP requests
- Code optimization: Minification and tree-shaking
- Dependency management: Resolve and include required modules
- Environment consistency: Ensure code works across different environments
- Development workflow: Enable use of modern tools and practices

---
layout: image
image: https://miro.medium.com/v2/resize:fit:1400/1*-JfKHumt7O_RsDg601EDIg.png
backgroundSize: contain
---

---
---

# Popular Bundler Options

- Webpack
- Rollup
- Parcel
- Snowpack
- **Esbuild**
- **Vite**
  - Rollup + Webpack
  - **Rolldown**
- **RsPack**
- **Turbopack**

> [Build Tools Comparison](https://2023.stateofjs.com/en-US/libraries/build_tools/)

---
---

# Basic Bundling Process

- Entry Point Identification:
  - Bundler starts with one or more entry point files
- Dependency Resolution:
  - Analyzes import/require statements
  - Builds a dependency graph of all modules
- Module Loading:
  - Reads the content of each module
  - Applies necessary transformations (e.g., transpilation)

---
---

# Basic Bundling Process

- Asset Processing:
  - Handles non-JavaScript assets (CSS, images, etc.)
  - Applies loaders or plugins for processing
- Code Generation:
  - Combines all modules into one or more bundle files
  - Generates a runtime to manage module execution
- Output:
  - Writes the bundled files to the specified output directory
  Handling Different Module Formats:

---
---

# Handling Different Module Formats:

- ES Modules (import/export):
  - Natively understood by modern bundlers
  - Preferred for tree shaking and static analysis
- CommonJS (require/module.exports):
  - Supported through module resolution algorithms
  - May require additional plugins or configuration

---
---

# Tree Shaking:

Tree Shaking:

- Eliminates dead code (unused exports)
- Works best with ES Modules

Process:

1. Mark exported code as used or unused
2. Remove unused code during minification

> [Webpack Tree Shaking](https://webpack.js.org/guides/tree-shaking/)

---
---

# Code Splitting:

- Divides code into smaller chunks
- Enables lazy loading of modules
- Implemented through:
  - Dynamic imports
  - Multiple entry points
  - Bundler-specific optimizations

> [Webpack Code Splitting](https://webpack.js.org/guides/code-splitting/)

---
---

# Node.js Module Resolution Algorithm

- Core Modules:
  - Built-in modules (e.g., 'fs', 'http') are checked first
- File Modules:
  - If path begins with '/' (absolute) or './' or '../' (relative):
    - Try exact filename
    - Try filename.js
    - Try filename.json
    - Try filename.node

---
---

# Node.js Module Resolution Algorithm

- Directory as Module:
  - If directory, look for package.json and use "main" field
  - If no package.json or no "main", look for index.js
- node_modules Resolution:
  - Look in ./node_modules
  - If not found, move to parent directory and repeat
  - Continue up to filesystem root

---
---

# Role of package.json:

- Defines package metadata and dependencies
- Key fields for module resolution:
  - "main": Entry point for CommonJS
  - "module": Entry point for ES Modules
  - "exports": Fine-grained control over exposed modules
  - "type": Sets default module system (CommonJS or ES Modules)

---
---

Example package.json:

```json
{
  "name": "my-package",
  "version": "1.0.0",
  "main": "./dist/index.js",
  "module": "./dist/index.mjs",
  "exports": {
    ".": {
      "import": "./dist/index.mjs",
      "require": "./dist/index.js"
    }
  },
  "type": "module"
}
```

---
---

Dual Package Hazard:

- Same package used as both CommonJS and ES Module
- Potential problems:
  - Different instances of the same module
  - Inconsistent state between instances
  - Breaking of singleton patterns

---
---

# Module in TypeScript

```json
{
  "compilerOptions": {
    /* Language and Environment */
    "target": "es2016" /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */,
    /* Modules */
    "module": "Node16" /* Specify what module code is generated. */,
    "moduleResolution": "Node16" /* Specify how TypeScript looks up a file from a given module specifier. */
  }
}
```
