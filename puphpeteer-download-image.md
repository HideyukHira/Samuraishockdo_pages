---
title: "PuPHPeteerで画像をダウンロード"
---
# PuPHPeteerで画像をダウンロードする書き方
## [rialto-php/puphpeteer](https://github.com/rialto-php/puphpeteer) についての記事です
PuPHPeteerとは、PHPからPuppeteerを操作できるパッケージなんです。    
これを使って、PHPからPuppeteerを操作してスクレイピング、データの保存が出来たりします。  
JSで生成されているページはPuppeteerでないとスクレイピング出来ないので、PHPのフレームワーク内でJSコンテンツのスクレイピングを行うときにとても助かっています。
    
## ズバリこれがダウンロードで出来るコードだ！
というかまんま抜粋！
[How to download image? ](https://github.com/rialto-php/puphpeteer/issues/91)
```
use Nesk\Puphpeteer\Puppeteer;

$puppeteer = new Puppeteer;
$browser = $puppeteer->launch();

$page = $browser->newPage();
$s = $page->goto('https://pestphp.com/assets/img/webp/hero.webp');

$base64 = $s->buffer()->toString('base64');
file_put_contents('./example.webp', base64_decode($base64));

$browser->close();
```
## 要は
```
$s = $page->goto('https://pestphp.com/assets/img/webp/hero.webp');
$base64 = $s->buffer()->toString('base64');
```
の所で、画像にgotoして、base64してるところがアイデアですね。
## 感想
検索してみたら、Puppeteer自体で画像ダウンロードについての決定的な方法ってないみたいですね。  
みなさんが、いろんな方法で試してる。  
この方法だと、素直にfile_put_contents()で保存できるので便利です。  
base64 ってこういう風にも使いどころがあるんだ！って勉強になりました。  
ちなみに自分の最初のアイデアは、キャプチャ機能でやる方法だった。
これだと、画像の解像度によっては画像の周りに黒フチがつくんですよね・・。
このページで助かりました。
## このページのコードを動作確認した環境・バージョン
- debian10 buster
- nginx 1.14.2
- node V16.10.0
- "nesk/puphpeteer": "^2.0"

[HOME](index.md)