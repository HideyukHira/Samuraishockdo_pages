---
title: "PuPHPeteerでアラート・ダイアログ処理 PHPでPuppeteerのpage.on()の書き方"
---
# PuPHPeteerでアラート・ダイアログ処理 PHPでPuppeteerのpage.on()の書き方
## [rialto-php/puphpeteer](https://github.com/rialto-php/puphpeteer) についての記事です
PuPHPeteerとは、PHPからPuppeteerを操作できるパッケージなんです。    
これを使って、PHPからPuppeteerを操作してスクレイピング、データの保存が出来たりします。  
JSで生成されているページはPuppeteerでないとスクレイピング出来ないので、PHPのフレームワーク内でJSコンテンツのスクレイピングを行うときにとても助かっています。
    
## 問題点 アラート ダイアログページに出会うことで$page->gotoが帰ってこない。
問題が起きました、そして解決しました。  
問題とは、「アラート・ダイアログが表示され、OKを押したあとページ遷移する」というURLに出くわすケースです。  
PuPHPeteerでは$page->gotoのところで止まってしまい、帰ってきません。  
おそらく全体のタイムアウトまで待ち続けて終了です。  
いろいろ回避策を試しました、例えば、$page-click() や、 $page->tryCatch->goto も効きませんでした、どうやら下記の page.on() 以外では無理のようです。
## page.on() でダイアログを処理する仕組み  Puppeteer本家
こちらは本家の [class: Dialog](https://github.com/puppeteer/puppeteer/blob/main/docs/api.md#class-dialog) です。
ダイアログの処理についてはこうなっています。  
```javascript
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  page.on('dialog', async (dialog) => {
    console.log(dialog.message());
    await dialog.dismiss();
    await browser.close();
  });
  page.evaluate(() => alert('1'));
})();
```
要は  
```javascript
  page.on('dialog', async (dialog) => {
    console.log(dialog.message());
    await dialog.dismiss();
    await browser.close();
  });
```
で、あらかじめダイアログが出現した場合の対応を前もって定義しておくってことなんですね、よく考えりゃ.onってそういうことだよね！って思いました。  
でも、これは本家のnode.jsのほうの書き方、求めているのはPuPHPeteerの方なんじゃい！
## PHPコードでの page.on処理 PuPHPeteer
```
$page = $browser->newPage();

//ダイアログが出てきたら許可する処理
$page->on('dialog', JsFunction::createWithParameters(['dialog'])
->body('dialog.accept();'));

$page->goto($url);
```
動きました。
[Evaluated functions must be created with JsFunction](https://github.com/rialto-php/puphpeteer#evaluated-functions-must-be-created-with-jsfunction)
にもあるように、
- ページ内で評価する関数はJsFunctionクラスで書くこと
- bodyの中の関数はjsで書くこと（PHPではない）
ってあります。
```
//ダイアログが出てきたら許可する処理
$page->on('dialog', JsFunction::createWithParameters(['dialog'])
->body('dialog.accept();'));
```
の
```
->body('dialog.accept();'));
```
のところで、'dialog'が出てきたときに、JSの関数'dialog.accept();'など、文字列で渡してページ側で実行できるのかな？と思いました。
## 感想
ずいぶん検索したけど、この問題のズバリの回答のコードってありませんでした。  
ぼつぼつと英語圏サイトで質問と回答を見つけ、コードを拾ってきたのですが「動かない」ので驚きました、バージョンが違うのでしょうかね。  
日本語圏には全然情報なかったので、記事を作りました。  
間違いがございましたら、ご指摘ください。
## このページのコードを動作確認した環境・バージョン
- debian10 buster
- nginx 1.14.2
- node V16.10.0
- "nesk/puphpeteer": "^2.0"

[HOME](index.md)