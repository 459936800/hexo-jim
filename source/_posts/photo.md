---
title: Hexo插入图片
date: 2020-11-11 13:34:45
categories:
- web前端
tags:
- Hexo
- 插件
- 教程
---

写篇blog记录一下。

## 第一步：安装插件，在hexo根目录打开Git Bash,执行
``` bash
npm install hexo-asset-image --save
```
## 第二步：打开hexo的配置文件_config.yml

找到 post_asset_folder，把这个选项从false改成true

<!-- more -->

## 第三步：修改一下代码

打开/node_modules/hexo-asset-image/index.js
将内容更换为下面的代码

``` JavaScript
'use strict';
var cheerio = require('cheerio');

// http://stackoverflow.com/questions/14480345/how-to-get-the-nth-occurrence-in-a-string
function getPosition(str, m, i) {
  return str.split(m, i).join(m).length;
}

var version = String(hexo.version).split('.');
hexo.extend.filter.register('after_post_render', function(data){
  var config = hexo.config;
  if(config.post_asset_folder){
        var link = data.permalink;
    if(version.length > 0 && Number(version[0]) == 3)
       var beginPos = getPosition(link, '/', 1) + 1;
    else
       var beginPos = getPosition(link, '/', 3) + 1;
    // In hexo 3.1.1, the permalink of "about" page is like ".../about/index.html".
    var endPos = link.lastIndexOf('/') + 1;
    link = link.substring(beginPos, endPos);

    var toprocess = ['excerpt', 'more', 'content'];
    for(var i = 0; i < toprocess.length; i++){
      var key = toprocess[i];
 
      var $ = cheerio.load(data[key], {
        ignoreWhitespace: false,
        xmlMode: false,
        lowerCaseTags: false,
        decodeEntities: false
      });

      $('img').each(function(){
        if ($(this).attr('src')){
            // For windows style path, we replace '\' to '/'.
            var src = $(this).attr('src').replace('\\', '/');
            if(!/http[s]*.*|\/\/.*/.test(src) &&
               !/^\s*\//.test(src)) {
              // For "about" page, the first part of "src" can't be removed.
              // In addition, to support multi-level local directory.
              var linkArray = link.split('/').filter(function(elem){
                return elem != '';
              });
              var srcArray = src.split('/').filter(function(elem){
                return elem != '' && elem != '.';
              });
              if(srcArray.length > 1)
                srcArray.shift();
              src = srcArray.join('/');
              $(this).attr('src', config.root + link + src);
              console.info&&console.info("update link as:-->"+config.root + link + src);
            }
        }else{
            console.info&&console.info("no src attr, skipped...");
            console.info&&console.info($(this));
        }
      });
      data[key] = $.html();
    }
  }
});
```
## 第四步：现在就可以插入图片了
比如hexo new post photo之后
就在source/_posts生成photo.md文件和photo文件夹，我们把要插入的图片复制到photo文件夹内，
在photo.md文件里面按markdown的标准写,（我的文件名是head.jpeg）比如

``` bash
![这是代替图片的文字，随便写](head.jpeg)
```

![这是代替图片的文字，随便写](avatar.gif)