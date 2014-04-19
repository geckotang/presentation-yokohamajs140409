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
```

[demo](http://jsfiddle.net/xS4P5/)

------

## それを

------

## こう

```javascript
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
```

[demo](http://jsfiddle.net/xS4P5/1/)

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

```javascript
$.getJSON("json/data.json", function(data){
	var html = [];
	// 取得した内容からHTMLを作成
	$.each(data.diaries, function(diary){
		html.push('<dl>');
		html.push('<dt>'+ diary.date +'</dt>');
		html.push('<dd>'+ diary.text +'</dd>');
		html.push('</dl>');
	});
	// 整形したページに挿入する
	$("#result").append(html.join(''));
	// イベントをつける
	$('dt').click(function(){
		if ($(this).next('dd').is(':visible')) {
			$(this).next('dd').hide();
		} else {
			$(this).next('dd').show();
		}
	});
});
```

```javascript
{
	"diaries": [
		{
			"date": "2014/01/01",
			"text": "The quick brown fox jumps over the lazy dog."
		},
		{
			"date": "2014/01/01",
			"text": "The quick brown fox jumps over the lazy dog."
		}
	]
}
```


------

## それを

------

## こう分けて

```javascript
// 配列からHTMLを作成する
function createDiary (diaries) {
	var html = [];
	$.each(diaries, function(diary){
		html.push('<dl>');
		html.push('<dt>'+ diary.date +'</dt>');
		html.push('<dd>'+ diary.text +'</dd>');
		html.push('</dl>');
	});
	return html.join('');
};
```

```javascript
// イベントをつける
function eventify (selector) {
	$(selector).click(function(){
		var $dd = $(this).next('dd');
		toggle($dd);
	});
};
```

```javascript
// 要素の表示を切り替える
function toggle ($dd) {
	if ($dd.is(':visible')) {
		$dd.hide();
	} else {
		$dd.show();
	}
};
```

------

## こう

```javascript
// もろもろ実行する
$.getJSON("json/data.json", function(data){
	// 配列からHTMLを作成する
	var html = createDiary(data.diaries);
	// 整形したページに挿入する
	$("#result").append(html);
	// イベントをつける
	eventify("#result dt");
});
```

------

## 分けて書くと何がいいか

- 問題が起きた時の発見が早い
- 仕様の変更がしやすい
 - やっぱdlじゃなくて〜
 - クリックしたらdtにclass付けてー
- 機能毎にJSファイルを分けることもできる

<br>

もっと良くする方法は、  
他にもあるけどまずは(ry

---

# ネストを減らす

------

## こんなJS書いてませんか

```html
<button class="changeBox1">100x100の明るい背景に変更</button>
<button class="changeBox2">200x200の暗い背景に変更</button>
<div class="box"></div>
```

```javascript
$(function(){
	$('.changeBox1').click(function(){
		changeBox('light', 100, 100);
	});
	$('.changeBox2').click(function(){
		changeBox('dark', 200, 200);
	});
	function changeBox(bgtype, width, height) {
		if (bgtype === 'light') {
			// 明るかったら
			$('.box').css({
				'width': width + 'px',
				'height': height + 'px',
				'color': '#fff',
				'background-color': 'tomato'
			});
		} else {
			// 暗かったら
			$('.box').css({
				'width': width + 'px',
				'height': height + 'px',
				'color': '#eee'
				'background-color': '#000'
			});
		}
	};
});
```

------

## それを

------

## こうしたり

```javascript
$(function(){
	/* 省略 */
	function changeBox(bgtype, width, height) {
		var setting = {};
		if (bgtype === 'light') {
			setting = {
				'width': width + 'px',
				'height': height + 'px',
				'color': '#fff',
				'background-color': 'tomato'
			};
		} else {
			setting = {
				'width': width + 'px',
				'height': height + 'px',
				'color': '#eee'
				'background-color': '#000'
			};
		}
		$('.box').css(setting);
	});
});
```

------

## こうする

```javascript
$(function(){
	/* 省略 */
	function changeBox(bgtype, width, height) {
		// 明るいかくらいかを判別しておいて
		var isLighten = (bgtype === 'lighten')? true : false ;
		// 先にスタイルを用意して
		var setting = {
			'width': width + 'px',
			'height': height + 'px',
			'color': isLighten ? '#fff' : '#eee',
			'background-color': isLighten ? 'tomato' : '#000'
		};
		// スタイルを当てる
		$('.box').css(setting);
	});
});
```

<br>

もっと良くする方法はあるけど、とり(ry

------

## なぜネストを減らすか

- 横に長いと、流れを追うのが大変
- 処理が追いやすくなる

---

## まとめ

- 登場頻度が多いものは、変数にしておくと、あとで幸せに。
- まとめてやらないで、必要な機能毎に分けて考える。
- ネストが深くなるときは、立ち止まってコードを見直す
- コードは見やすいほうがよい

---

# 宣伝

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
