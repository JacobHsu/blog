# blog

https://hexo.io/zh-tw/

```s
npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo server
```

```s
yarn server  
yarn build  
yarn deploy  
```

## theme

`npm i hexo-theme-next`  
blog\blog\node_modules\hexo-theme-next

_config.yml
```s
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next #landscape
```

## deploy

https://jacobhsu.github.io

_config.yml
```s
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repository: https://github.com/JacobHsu/jacobhsu.github.io # https://github.com/JacobHsu/hexo
  branch: main #gh-pages
```

> ERROR Deployer not found: git
`npm install hexo-deployer-git --save`  

## theme-next


blog\node_modules\hexo-theme-next\_config.yml

```js
# Local Search
# Dependencies: https://github.com/next-theme/hexo-generator-searchdb
local_search:
  enable: true #false
```