---
title: My New Post
date: 2021-03-29 23:22:13
tags: hexo
---

增加文章分類 (Categories)  
`$ hexo new page categories`

接著修改剛剛產生的 source/categories/inex.md 成下面的樣子並儲存。
```js

title: categories
date: 2021-03-29 23:24:52
type: "categories"

```

讓文章增加分類，只要在 categories 後面加入類別就可以了，記得要空格！

以上修改完後，記得還要去主題配置的 menu 中把 categories 跟 tags 打開，這樣才能在側欄中看到喔！

blog\node_modules\hexo-theme-next\_config.yml
```js
menu:
  #home: / || fa fa-home
  #about: /about/ || fa fa-user
  tags: /tags/ || fa fa-tags
  categories: /categories/ || fa fa-th
  #archives: /archives/ || fa fa-archive
```


增加標籤 (tags)  
`$ hexo new page tags`

接著修改剛剛產生的 source/tags/inex.md 成下面的樣子並儲存。

```js

title: tags
date: 2021-03-29 23:24:52
type: "tags"

```

讓文章增加標籤，只要在 tags  後面加入類別就可以了，記得要空格！

```js
title: 新的文章
date: 2020-04-02 12:12:57
categories: 文章類別
tags: 標籤_1 標籤_2
```
