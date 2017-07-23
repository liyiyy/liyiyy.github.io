---
title: 5.3——hexo补充
date: 2017-05-03 20:29:44
tags:
---

1>解决上次遗留下的图片问题,重新创立了上个文件上个后的时间变成了这个文件之后的时间，所以正确的说是下一篇文章，不是懒，是不想改；哈哈哈

1、首先将目录下_config.yml文件中的 post_asset_folder: true  像这样修改为true
2、打开git；执行	npm install https://github.com/CodeFalling/hexo-asset-image --save
3、在使用命令   hexo new "hexo补充"  新建一个md文件，发现会多出个文件夹，可以将需要引入的资源（图片）放在这个文件夹中；
4、引用   ![image](5-3hexo补充/下载.jpg)

2>博客优化

