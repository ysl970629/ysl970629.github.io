---
title: hexo-local-image
abbrlink: 56cd5f86
date: 2020-11-29 19:39:34
tags:
categories:
---

原链接：https://www.jianshu.com/p/f72aaad7b852

菜鸟一枚，插入图片的时候走了不少弯路，写篇blog记录一下。

第一步：安装插件，在hexo根目录打开Git Bash,执行

```text
npm install hexo-asset-image --save
```

第二步：打开hexo的配置文件`_config.yml`
找到 `post_asset_folder`，把这个选项从`false`改成`true`
第三步：打开

```text
/node_modules/hexo-asset-image/index.js
```

将内容更换为下面的代码
<!--more-->
（在此感谢Ericam_ 大神：https://blog.csdn.net/xjm850552586）

```js
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

第四步：现在就可以插入图片了，比如hexo new post photo之后
就在source/_posts生成photo.md文件和photo文件夹，我们把要插入的图片复制到photo文件夹内，
在photo.md文件里面按markdown的标准写,（我的文件名是head.jpeg）比如

```text
![这是代替图片的文字，随便写](2020-11-29-19-02-04.png)
```

注意不要有中文，并且如果改了文章文件名，同名的附件文件夹也要改成相同的名字

然后就……完事了。

![picture4](./hexo-local-image/2020-11-29-19-40-25.png)


参考链接：

- https://blog.csdn.net/xjm850552586/article/details/84101345
- https://blog.csdn.net/qq_38148394/article/details/79997971

