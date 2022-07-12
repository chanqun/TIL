REACT배워서 HUMAN되기 - leo

JS, CSS, react typescript

### 1. SETTING

nextJS -> react framework
이전에 기본적 세팅방법

webPack

npm을 쓸 수 있고
node package manager

yarn cache 많이 씀

```
npm init > enter 연타 > yarn init > enter 연타

"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
},
  
yarn add react react-dom

yarn add -D typescript @types/react @types/react-dom

```

typescript 는 dev 에서만 사용함 -D
자동 완성
@types/react @types/react-dom

react는 깔림

react는 컴포넌트를 재사용하기 위해서 사용함

```
  "compilerOptions": {
    "sourceMap": true,
    "target": "es5",
    "lib": ["dom", "ES2015", "ES2016", "ES2017", "ES2018", "ES2019", "ES2020"],
    "allowJs": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "esModuleInterop": true,
    "module": "commonjs",
    "isolatedModules": true,
    "jsx": "preserve",
    "allowSyntheticDefaultImports": true,
    "baseUrl": "./",
    "outDir": "./dist",
    "moduleResolution": "node"
  },
  "exclude": ["node_modules"],
  "include": ["**/*.ts", "**/*.tsx"]
}
```

3. Babel
   @babel es6 지원 안 하는 곳은 알아서 처리해줌

```
yarn add -D babel-loader @babel/core @babel/preset-env
```

4. Webpack - hot reloading 자동적용
   여러 프로젝트를 하나로 붕쳐주는 것

```
yarn add -D webpack webpack-cli webpack-dev-server
yarn add -D html-webpack-plugin ts-loader
```

entry index.tsx index.html

5. Eslint 구문 에러

```EXTENSIONS: MARKETPLACE 에서 아래 플러그인 설치
ESLint
Prettier - Code formatter
> yarn add -D eslint
> yarn eslint --init
.eslintrc.json 아래 내용으로 수정
{
    "env": {
        "browser": true,
        "es2021": true
    },
    "extends": [
        "eslint:recommended",
        "plugin:react/recommended",
        "plugin:@typescript-eslint/recommended",
        "plugin:@typescript-eslint/recommended-requiring-type-checking"
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "project": "./tsconfig.json",
        "ecmaFeatures": {
            "jsx": true
        },
        "ecmaVersion": "latest",
        "sourceType": "module"
    },
    "ignorePatterns": ["dist/", "node_modules/"],
    "plugins": [
        "react",
        "@typescript-eslint"
    ],
    "rules": {
    }
}
```

7. Prettier

```
yarn add -D prettier eslint-config-prettier eslint-plugin-prettier
eslint-config-prettier: Prettier와 충돌되는 ESLint 규칙들을 무시
eslint-plugin-prettier: Prettier를 사용해 포맷팅을 하도록 ESLint 규칙을 추가하는 플러그인
.eslintrc.json의 extends배열에 아래 내용 추가
"plugin:prettier/recommended",
/* eslint-plugin-prettier + eslint-config-prettier 동시 적용 */
"prettier/@typescript-eslint"
/* prettier 규칙과 충돌하는 @typescript-eslint/eslint-plugin 규칙 비활성화 */
.prettierrc.json 생성 후 아래 내용 기입
{
    "printWidth": 120,
    "tabWidth": 2,
    "trailingComma": "all",
    "singleQuote": true,
    "bracketSpacing": true,
    "semi": true,
    "useTabs": true,
    "arrowParens": "avoid",
    "endOfLine": "lf"
}
```