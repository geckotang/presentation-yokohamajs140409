帰ってきたYokohama.js (#yjs20140419)

### JavaScript力をほんの少し上げる3個のTips

---

## はじめに

![](https://pbs.twimg.com/profile_images/378800000483687034/574e2fe951f282f256dcf78a8c27f048.jpeg)

- [@GeckoTang](http://twitter.com/GeckoTang)
- フロントエンド・エンジニア
- PixelGrid.inc (10px目)
- CSSが好き
- z-indexむずかしい

---

## 話す内容

<br>

- 1.変数にする
- 2.分けて考える
- 3.ネストを減らす

<br>

すごいむつかしいはなしはしません。

---

# 変数にする

------

## こんなJSを書いてませんか

```javascript
$(function(){

	$('.showItem1').click(function(){
		$('.box .item1').show();
		$('.box .item2').hide();
		$(this).addClass('is-active');
		$('.showItem2').removeClass('is-active');
	});

	$('.showItem2').click(function(){
		$('.box .item1').hide();
		$('.box .item2').show();
		$(this).addClass('is-active');
		$('.showItem1').removeClass('is-active');
	});

});
```

------

## それを

------

## こう

```javascript
$(function(){
	var $box = $('.box');
	var $box__item1 = $box.find('.item1');
	var $box__item2 = $box.find('.item2');
	var $showItem1 = $('.showItem1');
	var $showItem2 = $('.showItem2');
	var cls_is_active = 'is-active';

	$showItem1.click(function(){
		var $that = $(this);
		$box__item1.show();
		$box__item2.hide();
		$that.addClass(cls_is_active);
		$showItem2.removeClass(cls_is_active);
	});
	$showItem2.click(function(){
		var $that = $(this);
		$box__item1.hide();
		$box__item2.show();
		$that.addClass(cls_is_active);
		$showItem1.removeClass(cls_is_active);
	});
});
```
------

## なぜ変数化したか

<br>

- 要素名などが変わった時の変更が楽  
<small>置換でもいいけど、誤置換など痛い目をみる</small>
- 同じ要素(.item1や.item2か)を何回も取得していたので、  
先頭で取得しておいて、変数いれておくほうが高速に

<br>

もっと良くする方法は、  
他にもあるけどまずはこういうとこから。

---

# 分けて考える

------

## こんなJS書いてませんか

------

## それを

------

## こう

---

# ネストを減らす

------

## こんなJS書いてませんか

------

## それを

------

## こう

---

## CodeGrid

「CodeGrid」は、ピクセルグリッドの技術情報配信サービス  
経験豊かなエンジニアと専属編集者の手で、  
確かな情報を定期的にお届けします


- 月額800円（税抜）30日間無料
- 一部の記事は登録なしの無料で読むことができます。  
私の記事：[ここまでできる！HTML＋CSS タイピングゲームを作る](https://app.codegrid.net/entry/derive-html-css-1)

---

## ご清聴ありがとうございました。
