---
layout: post
comments: true
title:  "Kindle Anki template example"
categories: Study
tags:  Kindle Anki template
date: 2017/07/25 10:50:31
---

* content
{:toc}

Kindle + Anki -> English


###

下面代码这里是在下面这篇文章中看到的一个模板的代码：

[Anki + Kindle Mate + Kindle：打造记单词利器](http://kmate.me/2017/02/21/anki-kindlemate-kindle-cn/)

（此外还可以参考[KindleMate+Anki之小白记单词v2 (Anki发音升级版)](http://kmate.me/2016/10/10/kindle-mate-anki-card-for-reciter-cn/#1KindleMateV20))

**Front Template**

```
<!-- 页眉区块  -->
<div class="bar head">牌组名称 : {{Deck}}</div>


<!-- 正面区块 -->
<div class="section">
<div id="front" class="items">{{单词}}{{发音}}  <span id="yx">-> </span></div>
<div id="front-extra1" class="items" ><div class="im">{{用法}}</div></div>

<div id="front-extra1" class="items" ><div class="im">{{字源}}</div></div>

</div>
```

**Back Template**
```


<!-- 背面区块 -->

{{FrontSide}}
<div class="section">
<div id="paraphrase" class="items2">{{释义}}</div>
</div>

<script type="text/javascript">
var colorMap = {
    'n.':'#86c440',
    'a.':'#f8b002',
    'adj.':'#727272',
    'ad.':'#684b9d',
    'adv.':'#684b9d',
    'v.':'#479cdf',
    'vi.':'#3e480d',
    'vt.':'#3e480d',
    'prep.':'#04B7C9',
    'conj.':'#04B7C9',
    'pron.':'#04B7C9',
    'art.':'#04B7C9',
    'num.':'#04B7C9',
    'int.':'#04B7C9',
    'interj.':'#04B7C9',
    'modal.':'#04B7C9',
    'aux.':'#04B7C9',
    'pl.':'#D111D3',
    'abbr.':'#D111D3',
  };
  [].forEach.call(document.querySelectorAll('#paraphrase'), function(div) {
  div.innerHTML = div.innerHTML
  .replace(/\b[a-z]+\./g, function(symbol) {
    if(colorMap[symbol]) {
      return '<a style="background-color:' + colorMap[symbol] + ';border-radius: 4px; color:white; padding:0 3px;margin-right:5px">'+
      symbol + '</a>';
    }else{
      return symbol;
    }
  });
 });

</script>
```


**Styling**

```css
</style>

<!--
Anki 基本模板 Facebook样式

 -->

<style>

/*卡片全局样式*/
.card {
  font-family      : helvetica, arial, sans-serif; /*字体名称*/
  font-size        : 20px;    /*字体大小-14px*/
  text-align       : left;    /*对齐方式-左对齐*/
  color            : #1d2129; /*字体颜色-#1d2129*/
  background-color : #e9ebee; /*背景颜色-#e9ebee*/
}

/*页眉页脚全局样式*/
.bar{
  border-radius   : 3px;               /*圆角幅度-3px*/
  border-bottom   : 1px solid #29487d; /*底边线颜色-#29487d*/
  color           : #fff;              /*字体颜色-白色*/
  padding         : 5px;               /*四周留白-5px*/
  text-decoration : none;              /*文本修饰-无*/
  font-size       : 12px;              /*字体大小-12px*/
  font-weight     : bold;              /*字体磅数-加粗*/
}

.bar #url a{
  text-decoration : none;
  font-size       : 12px;
  color           : #fff;
  font-weight     : bold;
}

/*页眉样式*/
.head{
  padding-left : 25px;  /*左侧留白-25px 为图标预留空间*/
  background   : #365899 url(_clipboard.png) no-repeat /*记事本图标*/
}

/*页脚样式*/
.foot{
  padding-right : 25px;  /*右侧留白-25px 为图标预留空间*/
  text-align    : right; /*文本右对齐*/
  background    : #365899 url(_cloud.png) no-repeat right /*云端图标*/
}

/*区块全局样式*/
.section {
  border           : 1px solid;               /*边缘样式-实线1px*/
  border-color     : #e5e6e9 #dfe0e4 #d0d1d5; /*边缘颜色*/
  border-radius    : 13px;                     /*圆角幅度-3px*/
  background-color : #fff;                    /*背景颜色-白色*/
  position         : relative;                /*定位-相对定位*/
  margin           : 5px 0;                   /*间隔-上下5px，左右0px*/
}
.section {
padding:1px;margin           : 10px -10px 10px -10px
}
/*区块项全局样式*/
.items{
  font-size  : 14px;               /*字体大小-12px*/
  border-top : 1.5px solid #e5e5e5;  /*边缘样式-实线1px*/
  margin     : 0 14px;             /*区块间隔*/
  padding    : 10px 0 8px 0;       /*区块留白*/
  line-height: 19px;			 /*行高*/

}
.items2{
  font-size  : 14px;               /*字体大小-12px*/
  border-top : 0px solid #e5e5e5;  /*边缘样式-实线1px*/
  margin     : 0 14px;             /*区块间隔*/
  padding    : 10px 0 8px 0;       /*区块留白*/
  line-height: 19px;			 /*行高*/

}

/*正面背面全局样式*/
#front,
#back{
  border-top  : 0px;        /*顶部边线-0px，防止和定期区块边线重叠*/
  line-height : 1.1em;      /*段落行高-1.2em*/
}
.im{line-height : 1.4em;}
/*正面字段样式*/
#front{
  font-family:helvetica;
  font-size  : 24px;       /*字体大小-24px*/
  font-weight: bold;
  font-family: courier;
  color      : #000;       /*字体颜色-黑色*/
  text-align : center;       /*文本对齐-居左*/

}

.book{
  font-size:12px;       /*字体大小-12px*/  
  text-align:right;		 /*右对齐*/  
  line-height: 22px;			 /*行高*/
  padding-top:9px;
}
.book:before{content:"——";}
 /*书籍字段样式控制*/  

/*u {
#	color: #fff;
#	font-weight: bold;
	text-decoration:none;
	border-bottom: 2px solid #080808;
	padding: 2px 2px 2px 2px;
	display:inline;
#	background-color: #365899;

}
可以在此处设置下划线的样式*/

/*背面字段样式*/
#back{
  font-size  : 16px;       /*字体大小-16px*/
  color      : #0000ff;    /*字体颜色-蓝色*/
  text-align : left;       /*文本对齐-居左*/
}

/*额外字段预留样式*/
#front-extra1{line-height : 1.8em;font-family: sans-serif;
}
#yx{font-size  : 16px;       /*字体大小-16px*/
#  color      : #0000ff;    /*字体颜色-蓝色*/
  text-align : left;       /*文本对齐-居左*/
}
#back-extra1{
}
#back-extra1{
}

</style>

<!--
JS帮助函数
效果：折叠展开效果。点击特定的区块/图片后，使指定的某区块折叠或展开
用法：将函数toggle赋予区块或者图片的onclick事件，并将e命名为要折叠的区块ID
举例：点击页眉，将正面区块折叠，可以按如下方式操作
在页眉的区块中加onclick=toggle(e），其中将e命名为要折叠的区块ID'front'
修改后的页眉区块如下
<div class="bar head" onclick="toggle('front')">牌组名称 : {{Deck}}</div>
-->
<script>
function toggle(e){
	var box=document.getElementById(e);
	if(box.style.display=='none'){
		box.style.display='block';
	}
	else{
		box.style.display='none';
	}
}
</script>

<style>
```



## Links

* [anki-connect](https://github.com/FooSoft/anki-connect#mac-os-x)
* [我不是针对谁，我是说在座各位...](https://ninja33.github.io/)
* [Chrome插件《Anki 划词制卡助手》使用说明(含视频教程)](https://ninja33.github.io/20160817/anki-dict-helper-chrome-extension/)
* []()
* []()
