---
title: JsBarcode循环生成多个条形码
date: 2022-12-09 11:44:43
author: Evan
categories: 笔记
tags:
---

## JsBarcode.js生成条形码

#### 1.引入jsbarcode

[JsBarcode官方文档](https://lindell.me/JsBarcode/)

```
//npm下载
npm install jsbarcode --save
```

#### 2.html

```html
<canvas id="canvas"></canvas>
<img id="barcodeimg"/>
<svg id="barcodesvg"></svg>
```

#### 3.JS调用JsBarcode方法

```js
JsBarcode("#canvas", "Hi world!");
JsBarcode("#barcodeimg", "what s up!");
JsBarcode("#barcodesvg", "grr bowwww");
```

#### 使用vue遍历生成条形码

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>  
    <script type="text/javascript" src="https://code.jquery.com/jquery-2.1.3.min.js"></script>
	<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.1.8/vue.min.js"></script>
    <script src="/js/JsBarcode.all.min.js"></script>
</head>
  <style>-
    #app{
    display:flex;
    flex-wrap:wrap;
    justify-content:space-between;
    align-items:center;
    }
    img{  
    width: auto;  
    height: auto;  
    max-width: 100%;  
    max-height: 100%;     
	}  
    .imgbox{
    height:80px;
    width:300px;
    }
  </style>
<body>
   <div id="app">
   	<div class="imgbox" v-for='(item,index) in jsBarcodeList ' :key='index' >
      <img :id="'jsbarcodeImg' + index"/>
   	</div>
  </div>
</body>
</html>
<script>

//循环生成条形码
const jsBarcodeList =['abc21312','qwe558','ttt999'];
function generateJSBarcodeImg(){
	jsBarcodeList.forEach((v,index)=>{
		// 根据动态id，动态赋值，动态生成条形码
      JsBarcode('#jsbarcodeImg' + index, v[0], {
        format: 'CODE39',
   		text:v,
        width: 2,
        height: 50,
  		fontSize:30,
		fontOptions: "bold"
      })
	})
}

 var vm = new Vue({
	el:"#app",
	data:{},
	mounted() {
			setTimeout(() => {
		    	generateJSBarcodeImg()
		  	}, 500)
		
	},
	
})

</script>
```

