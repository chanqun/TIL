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
@babel  es6 지원 안 하는 곳은 알아서 처리해줌
 
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
