[![Build Status](https://travis-ci.org/wan-zhen/vue-test.svg?branch=master)](https://travis-ci.org/wan-zhen/vue-test) [![Dependency Status](https://david-dm.org/wan-zhen/vue-test.svg)](https://david-dm.org/wan-zhen/vue-test) [![devDependency Status](https://david-dm.org/wan-zhen/vue-test/dev-status.svg)](https://david-dm.org/wan-zhen/vue-test?type=dev)

# vue-normal

## Project setup

```
npm install
```

### Compiles and hot-reloads for development

```
npm run serve
```

### Compiles and minifies for production

```
npm run build
```

### Run your tests

```
npm run test
```

### Lints and fixes files

```
npm run lint
```

### HeroKu

```
npm install --save express serve-static connect-history-api-fallback
```

### TravisCI VS HeroKu => CICD

TravisCI

1. 根目錄放 .travis.yml 並在裡面寫腳本，寫 CI 要跑的指令
2. 到 travis CI 該 repository Settings，可加 lable 到 Markdown

Heroku

1. Create New App
2. 到 Deploy 的 tab 連結 GitHub
3. Auto 選項裡把 要不要 CI 過了再 Deploy 的選項打勾
4. 在專案裡 install

```
npm install --save express serve-static connect-history-api-fallback
```

5. package.json Script 加兩行指令

```
    "postinstall": "node pack.js",
    "start": "node server.js",
```

6. 根目錄放 service.js & pack.js，代碼如 code
7. 把以上改的都推到 gitHub
8. 下次再 push 就會自動 CICD 囉
