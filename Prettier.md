# Prettier Coding Best Practices

Welcome to the Prettier Coding Best Practices guide! This document provides essential guidelines for configuring and using Prettier to maintain a consistent code style in your projects.

## Table of Contents

- [General Guidelines](#general-guidelines)
- [Configuration](#configuration)
- [Integration](#integration)
- [Code Style Preferences](#code-style-preferences)
- [Common Issues](#common-issues)
- [Advanced Usage](#advanced-usage)
- [Resources](#resources)

## General Guidelines

1. **Automate Code Formatting**: Use Prettier to automate code formatting and ensure a consistent code style across your project.

2. **Include Prettier in Your Workflow**: Integrate Prettier into your development workflow to automatically format code on save and before commits.

3. **Use Prettier Consistently**: Apply Prettier consistently across your entire codebase to avoid discrepancies in code style.

## Configuration

1. **Create a Configuration File**: Use a `.prettierrc` file to define your Prettier configuration. This file should be placed in the root directory of your project.

   ```json
   // .prettierrc
   {
     "semi": false,
     "singleQuote": true,
     "trailingComma": "es5",
     "tabWidth": 2,
     "printWidth": 80
   }
   ```

2. **Add Configuration for Specific Languages**: If necessary, create language-specific configuration files like `.prettierignore` for files or directories to be ignored.

   ```plaintext
   # .prettierignore
   node_modules
   build
   dist
   ```

3. **Use Prettier's Default Settings**: Consider using Prettier’s default settings if they align with your project's style guide. This minimizes configuration and reduces the risk of conflicts.

## Integration

1. **Integrate with Editors**: Install Prettier extensions for your code editor (e.g., VSCode, Sublime Text) to automatically format code on save.

    - **VSCode**: Install the Prettier extension and enable format on save in settings.
      ```json
      // settings.json
      {
        "editor.formatOnSave": true,
        "[javascript]": {
          "editor.formatOnSave": true
        }
      }
      ```

2. **Configure Pre-commit Hooks**: Use tools like Husky and lint-staged to run Prettier as part of your pre-commit hook.

   ```bash
   # Install dependencies
   npm install husky lint-staged --save-dev

   # Add to package.json
   {
     "husky": {
       "hooks": {
         "pre-commit": "lint-staged"
       }
     },
     "lint-staged": {
       "*.{js,jsx,ts,tsx}": [
         "prettier --write",
         "git add"
       ]
     }
   }
   ```

3. **Integrate with CI/CD**: Ensure Prettier runs as part of your CI/CD pipeline to catch formatting issues before merging code.

   ```yaml
   # .github/workflows/ci.yml
   jobs:
     format:
       runs-on: ubuntu-latest
       steps:
         - name: Checkout code
           uses: actions/checkout@v2
         - name: Install dependencies
           run: npm install
         - name: Run Prettier
           run: npx prettier --check .
   ```

## Code Style Preferences

1. **Consistent Use of Semicolons**: Decide whether to use semicolons consistently and configure Prettier accordingly.

   ```json
   // .prettierrc
   {
     "semi": true
   }
   ```

2. **Choose Between Single and Double Quotes**: Decide on single or double quotes for strings and configure Prettier to enforce this choice.

   ```json
   // .prettierrc
   {
     "singleQuote": true
   }
   ```

3. **Trailing Commas**: Configure Prettier to use trailing commas where applicable (e.g., in arrays and objects).

   ```json
   // .prettierrc
   {
     "trailingComma": "es5"
   }
   ```

4. **Line Length**: Set a maximum line length to improve readability, usually 80 or 100 characters.

   ```json
   // .prettierrc
   {
     "printWidth": 80
   }
   ```

5. **Indentation**: Configure the number of spaces per indentation level, typically 2 or 4 spaces.

   ```json
   // .prettierrc
   {
     "tabWidth": 2
   }
   ```

## Common Issues

1. **Conflicts with ESLint**: Prettier and ESLint can sometimes conflict. Use `eslint-config-prettier` and `eslint-plugin-prettier` to integrate them smoothly.

   ```bash
   # Install dependencies
   npm install eslint-config-prettier eslint-plugin-prettier --save-dev

   # Add to .eslintrc
   {
     "extends": ["prettier"],
     "plugins": ["prettier"],
     "rules": {
       "prettier/prettier": "error"
     }
   }
   ```

2. **Unformatted Code in Existing Files**: If you encounter unformatted code in existing files, run Prettier manually to format the entire codebase.

   ```bash
   npx prettier --write .
   ```

3. **Prettier Not Running**: Ensure Prettier is correctly configured in your editor and verify that any pre-commit hooks or CI/CD integrations are properly set up.

## Advanced Usage

1. **Custom Prettier Plugins**: Use custom Prettier plugins for additional formatting rules or language support.

   ```bash
   npm install --save-dev @prettier/plugin-xml
   ```

   ```json
   // .prettierrc
   {
     "plugins": ["@prettier/plugin-xml"]
   }
   ```

2. **Ignore Specific Files or Directories**: Use `.prettierignore` to specify files or directories to be excluded from formatting.

   ```plaintext
   # .prettierignore
   build/
   dist/
   *.min.js
   ```

3. **Format Only Specific Files**: Use Prettier from the command line to format only specific files or directories.

   ```bash
   npx prettier --write src/
   ```

## Resources

- [Prettier Documentation](https://prettier.io/docs/en/)
- [Prettier GitHub Repository](https://github.com/prettier/prettier)
- [Prettier with ESLint](https://prettier.io/docs/en/integrating-with-linters.html)
- [Husky](https://typicode.github.io/husky/)
- [lint-staged](https://github.com/okonet/lint-staged)

Feel free to contribute to this guide or suggest improvements. Happy coding!
```

This `README.md` file provides a thorough overview of best practices for configuring and using Prettier, ensuring consistent and efficient code formatting across your projects. It includes sections on configuration, integration with various tools, and handling common issues.