## eslint-ts-aribnb-config-issue-example

Problem: config order matters. If `airbnb` config is placed after [`plugin:@typescript-eslint/recommended`](https://github.com/typescript-eslint/typescript-eslint/blob/61c60dc047da680b8cc74943c52c33562942c95a/packages/eslint-plugin/src/configs/recommended.json), ESLint reports some errors, disabled by `plugin:@typescript-eslint/recommended`, twice, one as base ESLint error and the other from the `@typescript-eslint` counterpart.

Solution: place `plugin:@typescript-eslint/recommended` after `airbnb`:

```patch
@@ -1,5 +1,5 @@
 {
-    "extends": ["plugin:@typescript-eslint/recommended", "airbnb"],
+    "extends": ["airbnb", "plugin:@typescript-eslint/recommended"],
     "parser": "@typescript-eslint/parser",
     "parserOptions": {
         "ecmaFeatures": {
```

#### How to run:

1. Install dependencies: `yarn install` (or `npm install`)
2. Run linter:
    - `yarn lint-airbnb-first` (or `npm run lint-airbnb-first`)
    - `yarn lint-ts-first` (or `npm run lint-ts-first`)

Output:

`yarn lint-airbnb-first`

```
> \$ eslint --config .eslintrc-airbnb-first.json ./src --ext '.js,.ts'

.../eslint-ts-aribnb-example/src/camel-case.js
2:7 error Identifier 'camel_case' is not in camel case @typescript-eslint/camelcase

.../eslint-ts-aribnb-example/src/camel-case.ts
2:7 error Identifier 'camel_case' is not in camel case @typescript-eslint/camelcase

.../eslint-ts-aribnb-example/src/indent.js
1:1 error Expected indentation of 0 spaces but found 1 @typescript-eslint/indent
1:8 warning 'x' is assigned a value but never used @typescript-eslint/no-unused-vars

.../eslint-ts-aribnb-example/src/indent.ts
1:1 error Expected indentation of 0 spaces but found 1 @typescript-eslint/indent
1:8 warning 'x' is assigned a value but never used @typescript-eslint/no-unused-vars

.../eslint-ts-aribnb-example/src/no-array-constructor.ts
2:15 error The array literal notation [] is preferrable @typescript-eslint/no-array-constructor

.../eslint-ts-aribnb-example/src/no-unused-vars.js
1:7 warning 'x' is assigned a value but never used @typescript-eslint/no-unused-vars

.../eslint-ts-aribnb-example/src/no-unused-vars.ts
1:7 warning 'x' is assigned a value but never used @typescript-eslint/no-unused-vars

✖ 9 problems (5 errors, 4 warnings)
3 errors and 0 warnings potentially fixable with the `--fix` option.

error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```

`yarn lint-ts-first`

```
\$ eslint --config .eslintrc-ts-first.json ./src --ext '.js,.ts'

.../eslint-ts-aribnb-example/src/camel-case.js
2:7 error Identifier 'camel_case' is not in camel case camelcase
2:7 error Identifier 'camel_case' is not in camel case @typescript-eslint/camelcase
2:7 error 'camel_case' is assigned a value but never used no-unused-vars

.../eslint-ts-aribnb-example/src/camel-case.ts
2:7 error Identifier 'camel_case' is not in camel case camelcase
2:7 error Identifier 'camel_case' is not in camel case @typescript-eslint/camelcase
2:7 error 'camel_case' is assigned a value but never used no-unused-vars

.../eslint-ts-aribnb-example/src/indent.js
1:1 error Expected indentation of 0 spaces but found 1 indent
1:1 error Expected indentation of 0 spaces but found 1 @typescript-eslint/indent
1:8 error 'x' is assigned a value but never used no-unused-vars
1:8 warning 'x' is assigned a value but never used @typescript-eslint/no-unused-vars

.../eslint-ts-aribnb-example/src/indent.ts
1:1 error Expected indentation of 0 spaces but found 1 indent
1:1 error Expected indentation of 0 spaces but found 1 @typescript-eslint/indent
1:8 error 'x' is assigned a value but never used no-unused-vars
1:8 warning 'x' is assigned a value but never used @typescript-eslint/no-unused-vars

.../eslint-ts-aribnb-example/src/no-array-constructor.js
2:7 error 'array' is assigned a value but never used no-unused-vars

.../eslint-ts-aribnb-example/src/no-array-constructor.ts
2:7 error 'array' is assigned a value but never used no-unused-vars
2:15 error The array literal notation [] is preferable no-array-constructor
2:15 error The array literal notation [] is preferrable @typescript-eslint/no-array-constructor

.../eslint-ts-aribnb-example/src/no-unused-vars.js
1:7 error 'x' is assigned a value but never used no-unused-vars
1:7 warning 'x' is assigned a value but never used @typescript-eslint/no-unused-vars

.../eslint-ts-aribnb-example/src/no-unused-vars.ts
1:7 error 'x' is assigned a value but never used no-unused-vars
1:7 warning 'x' is assigned a value but never used @typescript-eslint/no-unused-vars

✖ 22 problems (18 errors, 4 warnings)
5 errors and 0 warnings potentially fixable with the `--fix` option.

error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```
