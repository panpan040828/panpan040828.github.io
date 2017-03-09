---
layout: post
title: 用原生js实现由className获取dom节点
date: 2016-08-01
categories: javaScript
tags: [javaScript,DOM]
description: 
---
可以实现传入多个类名获取相应的dom节点

- 由（a）获取 `<p class='a'></p>`
- 由（a）获取 `<p class='a b'></p>`
- 由（a b）获取 `<p class='a b'></p>`
- 由（a b）获取 `<p class='a b c'></p>`

```
var getElementsByClassName = function (searchClass, node,tag) {
  	  //函数后面两个参数可选，设置默认参数
      node = node || document;
      tag = tag || "*";

      //将传入的class按照空格分裂开
      var classes = searchClass.split(" ");
      var elements = node.getElementsByTagName(tag),
      patterns = [],
      current,
      match;

      //将传入的class变成正则表达式
      for(var i = 0; i < classes.length; i++) {
        patterns.push(new RegExp("(^|\\s)" + classes[i] + "(\\s|$)"));
      }

      for(var j = 0; j < elements.length; j++) {
        current = elements[j];
        match = false;
        for(var k = 0; k < patterns.length; k++){

          //Reg.test(string)判断string中是否有匹配的字符串，如果有，返回true
          match = patterns[k].test(current.className);
          if (!match)  {
            break;//跳出最内层循环
          }
        }
        if (match) {
          result.push(current);
        }
      }
        
      
      return result;
    }
  }
```