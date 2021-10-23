---
title: Windows 環境變量設置
date: 2021-10-24 22:22:13
tags: Dev
categories: Windows 
---

  ```sh

  # 查看当前所有可用的环境变量
  set
  # 查看某个环境变量：查看path变量的值
  set path
  # 修改环境变量（注意：这里是覆盖）
  set 变量名=变量内容
  # 设置为空
  set 变量名=
  # 给变量追加内容（%变量名%;代表以前的值）
  set 变量名=%变量名%;变量内容
  # 将C:\Go\bin\添加到path中
  set path=%path%;C:\Go\bin\

  ```

  [Windows 使用 cmd 命令行查看、修改、删除与添加环境变量](https://www.cnblogs.com/springsnow/p/12610417.html)