# Configuración ESLint, Prettier y Husky

Este es un repositorio base usando la versión 16 de Angular, en caso de existir un cambio en las librerias utilizadas se debe de adaptar el proyecto de acuerdo a esos cambios.

## Extensiones VSCode y Tips

Agregaremos las extensiones de ESLint y Prettier, luego iran a la configuración del vscode y pegaran lo siguiente:

```json
"editor.formatOnSave": true,
  "prettier.requireConfig": true,
  "typescript.preferences.importModuleSpecifier": "relative",
  "editor.codeActionsOnSave": {
    "source.organizeImports": true
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[jsonc]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[scss]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "prettier.printWidth": 80
```

## Migracion TSLint a ESLint Angular < v12

Recuerda que estos pasos solo aplica a las versiones que estan antes de la versión 12, ya que esas versiones usaban TSLint y generaban un archivo llamado **_tslint.json_**.

Ejecuta el siguiente comando en tu consola

```
ng add @angular-eslint/schematics
ng g @angular-eslint/schematics:convert-tslint-to-eslint --remove-tslint-if-no-more-tslint-targets --ignore-existing-tslint-config
```

Cuando termine de ejecutar esos comandos ya puedes eliminar el archivo **_tslint.json_**.

## Instalación EsLint Angular >= v12

Esta configuración solo aplica si usas angular versión 12 o superior.
AL momento de realizar este video la versión de esta librería para angular 14 es 14.0.2 pero si ustedes llegaran a presentar problemas al ejecutar el comando **ng lint**, usen la versión 14.0.0, ya que hay reportes de que a algunos les da error en la version 14.0.2.

```
ng add @angular-eslint/schematics
```

más información en: https://github.com/angular-eslint/angular-eslint

## Code Quality

1. Prettier
2. eslint-config-prettier
3. eslint-plugin-prettier

Ejecuta el siguiente comando en tu consola

```
npm install -D prettier eslint-config-prettier eslint-plugin-prettier
```

Luego dentro de tu archivo .eslinttrc.json agrega lo siguiente en la sección **extends** de las configuraciones para los archivos **.ts**
Configuración para los archivos TS:

```json
    		"extends": [
			  "eslint:recommended",
        "plugin:@typescript-eslint/recommended",
        "plugin:@angular-eslint/recommended",
        "plugin:@angular-eslint/template/process-inline-templates",
        "plugin:prettier/recommended"
			]
```

Ahora agregaremos unas reglas en la sección de **rules**:

```json
		 "rules": {
        "@angular-eslint/directive-selector": [
          "error",
          {
            "type": "attribute",
            "prefix": "app",
            "style": "camelCase"
          }
        ],
        "@angular-eslint/component-selector": [
          "error",
          {
            "type": "element",
            "prefix": "app",
            "style": "kebab-case"
          }
        ],
        "@angular-eslint/relative-url-prefix": ["error"],
        "@angular-eslint/sort-ngmodule-metadata-arrays": ["error"],
        "@angular-eslint/use-component-selector": ["error"],
        "@angular-eslint/use-pipe-transform-interface": ["error"],
        "@typescript-eslint/no-explicit-any": [
          "error",
          { "fixToUnknown": true }
        ],
        "@typescript-eslint/explicit-member-accessibility": [
          "off",
          {
            "accessibility": "explicit"
          }
        ],
        "@typescript-eslint/lines-between-class-members": [
          "error",
          "always",
          { "exceptAfterSingleLine": true }
        ],
        "@typescript-eslint/no-non-null-assertion": "off",
        "@typescript-eslint/explicit-function-return-type": "error",
        "prettier/prettier": [
          "error",
          {
            "endOfLine": "auto"
          }
        ],
        "no-restricted-syntax": [
          "error",
          {
            "selector": "Decorator[expression.callee.name=Injectable] Property[key.name=providedIn][value.value=root]",
            "message": "Do not use `providedIn: root` in Injectables"
          }
        ],
        "import/prefer-default-export": "off",
        "no-use-before-define": "off",
        "prefer-const": "off",
        "no-console": "warn"
      }
```

Puedes encontrar todas las reglas para TypeScript en este link: https://github.com/typescript-eslint/typescript-eslint/tree/main/packages/eslint-plugin/docs/rules

Configuración para los archivos HTML:

```json
{
  "files": ["*.html"],
  "extends": ["plugin:@angular-eslint/template/recommended", "prettier"],
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": [
      "error",
      {
        "endOfLine": "auto"
      }
    ]
  }
}
```

**LISTADO DE REGLAS PARA TYPESCRIPT EN: https://github.com/typescript-eslint/typescript-eslint**

## Configurar archivos Prettier

Debes de crear los siguientes archivos en la raíz de tu proyecto:

1. .prettierrc
2. .prettierignore

dentro del archivo **.prettierrc** coloca lo siguiente:

```json
{
  "singleQuote": true,
  "arrow-body-style": "off",
  "prefer-arrow-callback": "off",
  "printWidth": 80,
  "arrowParens": "always",
  "overrides": [
    {
      "files": "*.html",
      "options": {
        "parser": "html"
      }
    },
    {
      "files": "*.component.html",
      "options": {
        "parser": "angular"
      }
    }
  ]
}
```

Para evitar formatear algunos archivos podemos hacer uso del archivo **.prettierignore**, agrega lo siguiente:

```console
node_modules/*
package-lock.json
yarn.lock
src/*.ts
src/index.html
```

Recuerda que puedes configurar los atributos a tu gusto, más información en https://prettier.io/playground/

Dentro tu archivo **package.json** existe una sección llamada **scripts**, dentro agregaras lo siguiente:

```json
"lint": "ng lint --fix --max-warnings 10",
"prettier": "pretty-quick --pattern \"src/**/*.{ts,js,scss,html}\" --staged",
```

## Configurar Husky

Husky nos ayudara a verificar nuestros cambios de git, más información en:
https://typicode.github.io/husky/#/

Ejecuta el siguiente comando:

```console
npx husky-init
```

Esto creara una carpeta llamada **".husky"**, dentro de esta carpeta existe un archivo llamado **pre-commit**, abajo se modifica su contenido.

## Formatear solo los archivos modificados

Para poder formater unicamente los archivos modificados usaremos la librería **pretty-quick**

```console
npm i -D pretty-quick
```

Ahora debemos de modificar nuestro archivo **pre-commit** con lo siguiente:

```console
npm run prettier && npm run lint
```

OPCIONAL: aregar un archivo **pre-push** para ejecutar los test, dentro agrega lo siguiente:

```console
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npm run test
```
