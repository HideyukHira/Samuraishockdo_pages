---
title: OUTLOOKでテンプレートから自動で日付挿入
---
毎日OUTLOOK2010で日報メールを書いている。  
内容は
```
宛先アドレス ：固定
件名：今日の日付 ＋ 日報
本文： &nbsp;
○○様
今日の日付 の日報です。
以下箇条書き~~
```
ってな感じで、箇条書きの内容を除いて後は毎日同じです。  
宛先を選んだり、日付を毎日書くのがだるいのでマクロにしました。  
マクロはコレ
```
Sub 日報()
    Dim MyItem As Outlook.MailItem

    Set MyItem = Application.CreateItemFromTemplate("C:\Users\username\AppData\Roaming\Microsoft\Templates\日報.oft")
    MyItem.Display
    dtShort = FormatDateTime(Now, vbShortDate)
    MyItem.Subject = Replace(MyItem.Subject, "yyyy/mm/dd", dtShort)
    MyItem.Body = Replace(MyItem.Body, "yyyy/mm/dd", dtShort)
End Sub
```
## 手順 １

新規メールで送りたいメールを作成します。  
のちにマクロで日付と置換させるための文字列、&nbsp;yyyy/mm/dd &nbsp;を件名と、本文に書いておきます。場所は好きなところで構いません。  

それを名前を付けて保存するのですが、この時「OUTLOOKテンプレート形式」を選択して保存します。  
そして、保存先のファイルpathをメモ。  

## 手順２
OUTLOOKのVBAエディターを開き、上記のマクロを書き込みます。
```
Set MyItem = Application.CreateItemFromTemplate("C:\Users\username\AppData\Roaming\Microsoft\Templates\日報.oft")
```
のパスの部分にさっき保存したテンプレートのパスを記入します。
作成はこれで終了です。  

### 【使い方】
開発＞マクロ から呼び出して使います。  
自動で、件名と本文のyyyy/mm/ddが、今日の日付 2015/01/23 などに置換され入力された新規メールがあなたの目の前にあるはずです。  
筆者はリボンのユーザー設定をカスタマイズして、すぐ使えるようリボンに表示させています。これで毎日の無駄な数十秒を節約  
### マクロの解説
このマクロは、yyyy/mm/ddなど置換対象となる文字を、あらかじめOUTLOOKテンプレートファイルに書き込み保存しておいて、そのテンプレートから新規メールを作るときに、対象文字列を現在時刻に置換するというものです。  
コードを見てみましょう。  
筆者はリボンのユーザー設定をカスタマイズして、すぐ使えるよう表示しています。  
オブジェクト型変数の宣言です、MyItem は、新しいメール メッセージを表す MailItem オブジェクトだぞと言ってます。  
```
Dim MyItem As Outlook.MailItem
```
オブジェクト型変数にSetを使って、Outlook テンプレート (.oft) から新しい Outlook アイテムを作成し、そのアイテムを代入すぞと言っています。()の中にはOutlook テンプレートのパスとファイル名を指定しております。
```
Set MyItem = Application.CreateItemFromTemplate("C:\Users\username\AppData\Roaming\Microsoft\Templates\日報.oft")
```
テンプレートから新しくできたオブジェクト（新しくできたメール）を表示します。
```
MyItem.Display
```
日付情報を取得します、、FormatDateTime(Now関数で現在のシステムの日付と時刻取得,vbShortDate「地域のプロパティ」で指定されている短い形式で日付)にしたものを変数dtshortに代入しています。  
dtshortの中身は、2015/01/23などのような形になっています。
```
dtShort = FormatDateTime(Now, vbShortDate)
```
メールの件名&nbsp;MyItem.Subject &nbsp;の "yyyy/mm/dd"を 上記のdtshort に置換・代入します。この部分で、件名に書かれている「yyyy/mm/dd」が、2015/01/23などの文字に入れ替わるという仕組みです。
```
MyItem.Subject = Replace(MyItem.Subject, "yyyy/mm/dd", dtShort)
```
同じやり方で、メール本文MyItem.Bodyを置換・代入します。
```
MyItem.Body = Replace(MyItem.Body, "yyyy/mm/dd", dtShort)
```
というわけです。
要はテンプレートにあらかじめ書いてあった文字を置換しているので、
```
MyItem.Body = Replace(MyItem.Body, "置換対象文字列", "置換文字")
```
をいろいろと付け足せば、本文のカスタマイズできると思います。
ちなみに、OUTLOOK研究所さんの
[https://outlooklab.wordpress.com/2008/04/12/ テンプレートに自動で今日の日付を設定するマクロ](https://outlooklab.wordpress.com/2008/04/12/)
を参考にしています。ほぼパクリじゃねえか？と思われると思われる方もいらっしゃると思いますが、なにとぞ穏便によろしくお願いいたします。  
## 追記：OUTLOOK豆知識
Google　アドワーズのキーワードアドバイスツールに予測で出ていたキーワードをご紹介
## おうtぉおk
見つけた時、笑っちゃいました！
