# NgJestPreset

## 1. Create new Angular App

```sh
ng new ng-app-a --style=css --routing
```

## 2. Install Prettier

```sh
npm install --save-dev --save-exact prettier
```

### 2.1. Edit package.json

```json
"scripts": {
  // ...
  "prettier": "prettier --write \"**/*.{js,json,css,scss,less,md,ts,html,component.html}\"",
  // ...
},
```

### 2.2. Add prettier.config.js

```js
module.exports = {
  printWidth: 80,
  tabWidth: 2,
  useTabs: false,
  semi: true,
  singleQuote: true,
  trailingComma: 'es5',
  bracketSpacing: true,
  jsxBracketSameLine: false,
  arrowParens: 'always',
  rangeStart: 0,
  rangeEnd: Infinity,
  requirePragma: false,
  insertPragma: false,
  proseWrap: 'preserve',
};
```

### 2.3. Add .prettierignore

```
package.json
dist
```

## 3. Install husky & lint-staged

### 3.1. Edit package.json

```json
{
  "devDependencies": {
    // ...
    "husky": "3.0.3",
    // ...
    "lint-staged": "9.2.1"
    // ...
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.{js,json,css,scss,less,md,ts,html,component.html}": [
      "prettier --write",
      "git add"
    ]
  }
}
```

### 3.2. Install

```sh
npm install
```

## 4. Add Launch Chrome configuration

### 4.1 Edit launch.json

```json
"configurations": [
  {
    "type": "chrome",
    "request": "launch",
    "name": "Launch Chrome",
    "url": "http://localhost:4200",
    "webRoot": "${workspaceFolder}",

    // Set port to avoid 'Cannot connect to runtime process, timeout after 10000 ms' error
    "port": 4000
    // "trace": "verbose",
  }
]
```

## 5. Install jest-preset-angular

### 5.1. Remove Karma

```sh
npm remove karma karma-chrome-launcher karma-coverage-istanbul-reporter karma-jasmine karma-jasmine-html-reporter

rm ./karma.conf.js
rm ./src/test.ts
```

### 5.2 Install jest-preset-angular

```sh
npm install --save-dev --save-exact jest@24.8.0 @types/jest@24.0.17 jest-preset-angular@7.1.1
```

### 5.3 Edit tsconfig.spec.json

Remove `"types"` and `test.ts`.

```json
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "outDir": "./out-tsc/spec"
  },
  "files": ["src/polyfills.ts"],
  "include": ["src/**/*.spec.ts", "src/**/*.d.ts"]
}
```

### 5.4 Edit tsconfig.json

Add `"types": ["jest"],`.

```json
{
  "compilerOptions": {
    "baseUrl": "./",
    "outDir": "./dist/out-tsc",
    "sourceMap": true,
    "declaration": false,
    "downlevelIteration": true,
    "experimentalDecorators": true,
    "module": "esnext",
    "moduleResolution": "node",
    "importHelpers": true,
    "target": "es2015",
    "typeRoots": ["node_modules/@types"],
    "types": ["jest"],
    "lib": ["es2018", "dom"]
  }
}
```

### 5.5 Add src/setUpJest.ts

```ts
import 'jest-preset-angular';
// import './jestGlobalMocks'; // browser mocks globally available for every test
```

### 5.6 Edit package.json

```json
{
  "scripts": {
    // ...
    "test": "jest"
    // ...
  }
}
```

### 5.7 Add jest.config.js

```js
module.exports = {
  preset: 'jest-preset-angular',
  setupFilesAfterEnv: ['<rootDir>/src/setupJest.ts'],
  testPathIgnorePatterns: [
    '<rootDir>/node_modules/',
    '<rootDir>/dist/',
    '<rootDir>/src/test.ts',
  ],
  globals: {
    'ts-jest': {
      tsConfig: '<rootDir>/tsconfig.spec.json',
      stringifyContentPathRegex: '\\.html$',
      astTransformers: [
        '<rootDir>/node_modules/jest-preset-angular/InlineHtmlStripStylesTransformer',
      ],
    },
  },
};
```

### 5.8 Run the tests

```sh
npm run test
```

### 5.9 Edit launch.json

```json
({
  "type": "node",
  "request": "launch",
  "name": "Jest All",
  "program": "${workspaceFolder}/node_modules/.bin/jest",
  "args": ["--runInBand"],
  "console": "integratedTerminal",
  "internalConsoleOptions": "neverOpen",
  "disableOptimisticBPs": true,
  "windows": {
    "program": "${workspaceFolder}/node_modules/jest/bin/jest"
  }
},
{
  "type": "node",
  "request": "launch",
  "name": "Jest Current File",
  "program": "${workspaceFolder}/node_modules/.bin/jest",
  "args": ["${fileBasenameNoExtension}", "--config", "jest.config.js"],
  "console": "integratedTerminal",
  "internalConsoleOptions": "neverOpen",
  "disableOptimisticBPs": true,
  "windows": {
    "program": "${workspaceFolder}/node_modules/jest/bin/jest"
  }
})
```

## 6. Configure vscode-jest extension

No changes required.

## References

- https://github.com/prettier/prettier
- https://github.com/thymikee/jest-preset-angular
- https://github.com/typicode/husky
- https://github.com/okonet/lint-staged

## VS Code Extensions

- [Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome)
- [Jest](https://marketplace.visualstudio.com/items?itemName=Orta.vscode-jest)
- [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
