---
uid: mvc/overview/performance/bundling-and-minification
title: バンドルと縮小 |Microsoft Docs
author: Rick-Anderson
description: バンドルと縮小は、2 つの手法を要求の読み込み時間を向上させるために ASP.NET 4.5 で使用することができます。 バンドルと縮小 reducin して読み込み時間が向上しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/23/2012
ms.topic: article
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 4f21184f0917cd957e9e1719c63769e1a027961c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384781"
---
<a name="bundling-and-minification"></a>バンドルと縮小
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> バンドルと縮小は、2 つの手法を要求の読み込み時間を向上させるために ASP.NET 4.5 で使用することができます。 バンドルと縮小は、サーバーへの要求の数が減り、(CSS および JavaScript を使用します。) など、要求された資産のサイズを小さくして読み込み時間を向上します。


現在の主要なブラウザーのほとんどの数を制限する[同時接続](http://www.browserscope.org/?category=network)ごとに 6 つの各ホスト名。 6 つの要求が処理中に、ホスト上の資産に対する追加要求はブラウザーによってキューすることを意味します。 次の図では、IE F12 開発者ツール ネットワーク タブは、サンプル アプリケーションのバージョン情報の表示に必要な資産のタイミングを示します。

![B/M](bundling-and-minification/_static/image1.png)

灰色のバーには、6 つの接続の制限を待機しているブラウザーによって要求がキューに置かれた時間が表示されます。 黄色のバーは、最初のバイトへの要求時間、要求を送信し、サーバーから最初の応答を受信にかかった時間は、します。 青色のバーは、サーバーからの応答データを受信するのにかかる時間を表示します。 詳細なタイミング情報を取得する資産をダブルクリックすることができます。 たとえば、次の図は読み込みのタイミングの詳細を示しています。、 */Scripts/MyScripts/JavaScript6.js*ファイル。

![](bundling-and-minification/_static/image2.png)

上記の図は示しています、**開始**イベント、これにより、ブラウザーのため、要求がキューに入れられた時間は、同時接続の数を制限します。 ここでは、要求は別の要求が完了するを待つ 46 のミリ秒のキューに格納されました。

## <a name="bundling"></a>バンドル

バンドルとは、簡単に結合または複数のファイルを 1 つのファイルをバンドルする ASP.NET 4.5 での新機能です。 CSS、JavaScript、およびその他のバンドルを作成することができます。 少ないファイルは、少数の HTTP 要求とするには、最初のページ読み込みのパフォーマンスを改善するを意味します。

次の図は、バージョン情報に表示されるバンドルと縮小が有効になっていると、この時間が、以前は、同じタイミング ビューを示します。

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>縮小

縮小では、さまざまなスクリプトまたは不要な空白とコメントを削除して、1 つの文字を変数名を短縮することなど、css を別のコードの最適化を実行します。 次の JavaScript 関数を検討してください。

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

縮小後、は、関数は、次に縮小されます。

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

コメントと不要な空白文字を削除するだけでなく、次のパラメーターと変数名が名前を変更 (短縮)。

| **翻訳元** | **名前変更** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | i |

## <a name="impact-of-bundling-and-minification"></a>影響のバンドルと縮小

次の表では、すべての資産を個別に一覧表示して、サンプル プログラムのバンドルと縮小 (B/分) を使用してのいくつかの重要な違いを示します。

|  | **B/分を使用します。** | **B/分なし** | **変更** |
| --- | --- | --- | --- |
| **ファイルの要求** | 9 | 34 | 256% |
| **サポート技術情報の送信** | 3.26 | 11.92 | 266% |
| **サポート技術情報の受信** | 388.51 | 530 | 36% |
| **読み込み時間** | 510 MS | 780 MS | 53% |

送信バイト数では、ブラウザーが要求に適用される HTTP ヘッダーを非常に冗長のバンドルとは大幅に短縮必要があります。 受信したバイト数の減少がその大きさではありませんので、サイズの大きなファイル (*Scripts\jquery-ui-1.8.11.min.js*と*Scripts\jquery-1.7.1.min.js*) 縮小は既に。 注: 使用するサンプル プログラムのタイミング、 [Fiddler](http://www.fiddler2.com/fiddler2/)低速ネットワークをシミュレートするためのツール。 (、Fiddler から**ルール**メニューの **パフォーマンス**し**モデム スピードのシミュレート**)。

## <a name="debugging-bundled-and-minified-javascript"></a>バンドルおよび縮小版の JavaScript デバッグ

開発環境で、JavaScript のデバッグが容易になります (場所、 [compilation 要素](https://msdn.microsoft.com/library/s10awwz0.aspx)で、 *Web.config*に設定されているファイル`debug="true"`)、JavaScript ファイルが組み込まれていないため、または縮小します。 JavaScript ファイルのバンドルおよび縮小では、リリース ビルドをデバッグすることもできます。 IE F12 開発者ツールを使用して、次のアプローチを使用して、縮小されたバンドルに含まれる JavaScript 関数をデバッグします。

1. 選択、**スクリプト**タブを選び、**デバッグを開始**ボタンをクリックします。
2. [アセット] ボタンを使用してデバッグする JavaScript 関数を含む、バンドルを選択します。  
    ![](bundling-and-minification/_static/image4.png)
3. 縮小された JavaScript を選択して書式設定、**構成ボタン**![](bundling-and-minification/_static/image5.png)を選択し、**形式 JavaScript**します。
4. **検索スクリプト**t 入力ボックスに、デバッグする関数の名前を選択します。 次の図の**AddAltToImg**で入力した、**検索スクリプト**t 入力ボックス。  
    ![](bundling-and-minification/_static/image6.png)

F12 開発者ツールを使用したデバッグの詳細については、MSDN の記事を参照してください。 [JavaScript エラーのデバッグに F12 開発者ツールを使用して](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx)します。

## <a name="controlling-bundling-and-minification"></a>制御のバンドルと縮小

バンドルし縮小が有効か無効で debug 属性の値を設定して、 [compilation 要素](https://msdn.microsoft.com/library/s10awwz0.aspx)で、 *Web.config*ファイル。 次の XML で`debug`はこれを true に設定するバンドルと縮小が無効になっています。

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

バンドルと縮小を有効にするには設定、`debug`値"false"にします。 オーバーライドすることができます、 *Web.config*による設定、`EnableOptimizations`プロパティを`BundleTable`クラス。 次のコードのバンドルと縮小を有効にしの設定をすべてオーバーライド、 *Web.config*ファイル。

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> しない限り、`EnableOptimizations`は`true`または debug 属性、 [compilation 要素](https://msdn.microsoft.com/library/s10awwz0.aspx)で、 *Web.config*に設定されているファイル`false`ファイルはバンドルや縮小されません。 さらに、.min バージョンのファイルは使用されませんが、完全なデバッグ バージョンが選択されます。 `EnableOptimizations` debug 属性をオーバーライド、 [compilation 要素](https://msdn.microsoft.com/library/s10awwz0.aspx)で、 *Web.config*ファイル


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>バンドルを使用して、ASP.NET Web フォームと Web ページの縮小

- Web ページ、ブログ記事を参照してください。 [Web ページ サイトに Web の最適化を追加する](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx)します。
- Web フォーム、ブログ記事を参照してください。[バンドルを追加すると Web フォームに縮小](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx)します。

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>バンドルを使用してと ASP.NET MVC での縮小

このセクションではプロジェクトのバンドルを確認して縮小、ASP.NET MVC を作成します。 という名前の新しい ASP.NET MVC インターネット プロジェクトを最初に、作成**MvcBM**既定値を変更することがなく。

開く、*アプリ\_Start\BundleConfig.cs*ファイルを調べ、`RegisterBundles`メソッドを作成し、登録して、バンドルを構成するために使用します。 次のコードの一部を示しています、`RegisterBundles`メソッド。

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

上記のコードは、という名前の新しい JavaScript バンドルを作成します *~/bundles/jquery*を含む、すべての適切な (デバッグまたはなく縮小する。 *。vsdoc*) 内のファイル、*スクリプト*ワイルドカード文字列"~/Scripts/jquery-{version} .js"に一致するフォルダー。 ASP.NET MVC 4、デバッグ構成では、ファイル、つまり*jquery 1.7.1.js*バンドルに追加されます。 リリース構成で*jquery 1.7.1.min.js*が追加されます。 バンドルのフレームワークにはなどのいくつかの一般的な規則が次に示します。

- "FileX.min.js"と"FileX.js"が存在する場合は、リリース".min"ファイルを選択します。
- デバッグ用の非".min"バージョンを選択します。
- 無視"-vsdoc"(jquery-1.7.1-vsdoc.js) などの IntelliSense でのみ使用されるファイル。

`{version}` JQuery の適切なバージョンの jQuery バンドルを自動的に作成される前に示したワイルドカードに一致する、*スクリプト*フォルダー。 この例では、ワイルド カードを使用すると次の利点があります。

- NuGet を使用して、バンドルの上記のコードまたはページの表示で jQuery 参照を変更することがなく新しい jQuery バージョンに更新することができます。
- 自動的に選択デバッグ構成の完全なバージョンとリリースの".min"バージョンをビルドします。

## <a name="using-a-cdn"></a>CDN の使用

 次のコードでは、CDN の jQuery バンドルでローカル jQuery バンドルが置き換えられます。

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

上記のコードでは、jQuery を要求された CDN から中に、リリース モードとデバッグ バージョンの jQuery がフェッチされるローカルでデバッグ モードでは。 CDN を使用する場合、CDN 要求が失敗した場合、フォールバック メカニズムが必要です。 次のマークアップは、jQuery は CDN 失敗要求に追加のレイアウト ファイル示しますスクリプトの末尾からフラグメントします。

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>バンドルの作成

[バンドル](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx)クラス`Include`メソッドは各文字列のリソースへの仮想パスが、文字列の配列を受け取ります。 RegisterBundles メソッドから次のコード、*アプリ\_Start\BundleConfig.cs*ファイルは複数のファイルは、バンドルに追加を示しています。

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

[バンドル](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx)クラス`IncludeDirectory`ディレクトリ (および必要に応じてすべてのサブディレクトリ) 内の指定された検索パターンに一致するすべてのファイルを追加するメソッドが提供されます。 [バンドル](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx)クラス`IncludeDirectory`API を次に示します。

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

バンドルは、Render メソッドを使用してビューで参照されている ( `Styles.Render` css と`Scripts.Render`JavaScript 用)。 次のマークアップ、 *views \shared\\_Layout.cshtml*ファイルは ASP.NET のインターネット プロジェクトの既定のビューで CSS および JavaScript のバンドルを参照する方法を示しています。

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Render メソッドには、文字列の配列があるので、コードの 1 つの行で複数のバンドルを追加することができますに注意してください。 一般に、資産を参照するために必要な HTML を作成するレンダリング メソッドを使用するは。 使用することができます、`Url`アセットを参照するために必要なマークアップことがなく、資産への URL を生成するメソッド。 新しい HTML5 を使用すると想定[async](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async)属性。 次のコードは、modernizr を使用して参照する方法を示します、`Url`メソッド。

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>使用して、"\*"ファイルを選択するワイルドカード文字

指定された仮想パス、`Include`メソッドと、検索パターン、`IncludeDirectory`メソッドには、いずれかを指定できる"\*"プレフィックスまたは最後のパス セグメントをサフィックスとしてワイルドカード文字です。 検索文字列が大文字小文字を区別します。 `IncludeDirectory`メソッドには、オプションのサブディレクトリを検索します。

次の JavaScript ファイルでは、プロジェクトを検討してください。

- *Scripts\Common\AddAltToImg.js*
- *Scripts\Common\ToggleDiv.js*
- *Scripts\Common\ToggleImg.js*
- *Scripts\Common\Sub1\ToggleLinks.js*

![dir imag](bundling-and-minification/_static/image7.png)

次の表に示すように、ワイルドカードを使用してバンドルに追加されたファイルを示します。

| **Call** | **追加されたファイルまたは例外が発生しました** |
| --- | --- |
| Include("~/Scripts/Common/\*.js") | *AddAltToImg.js、ToggleDiv.js、ToggleImg.js* |
| Include("~/Scripts/Common/T\*.js") | 無効なパターンの例外。 ワイルドカード文字はプレフィックスまたはサフィックスにのみ使用できます。 |
| Include("~/Scripts/Common/\*og.\*") | 無効なパターンの例外。 1 つだけのワイルドカード文字を許可します。 |
| "Include("~/Scripts/Common/T\*") | *ToggleDiv.js、ToggleImg.js* |
| "Include("~/Scripts/Common/\*") | 無効なパターンの例外。 純粋なワイルドカード セグメントが無効です。 |
| IncludeDirectory("~/Scripts/Common", "T\*") | *ToggleDiv.js、ToggleImg.js* |
| IncludeDirectory("~/Scripts/Common", "T\*",true) | *ToggleDiv.js、ToggleImg.js、ToggleLinks.js* |

一般に優先が明示的に各ファイルをバンドルに追加する理由は次のファイルの読み込みのワイルドカードの上。

- ワイルドカードの既定値で、通常これが望ましくないのはアルファベット順に読み込むことにスクリプトを追加します。 CSS および JavaScript ファイルは頻繁に (アルファベット以外の) 特定の順序で追加する必要があります。 このリスクを軽減するには、カスタムの追加を[IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx)の実装が、各ファイルは、エラーが発生しやすいを明示的に追加します。 変更する必要がありますが、将来のフォルダーに新しい資産を追加するなど、 [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx)実装します。
- 読み込みワイルド カードを使用してディレクトリに追加されるビューの特定のファイルは、そのバンドルを参照しているすべてのビューに含めることができます。 バンドルには、特定のスクリプトの表示を追加する場合、バンドルを参照している他のビューの JavaScript エラーが発生する可能性があります。
- その他のファイルがインポートされる CSS ファイルと、2 回読み込まれて、インポートされたファイル。 たとえば、次のコードでは、ほとんどの 2 回読み込まれる jQuery UI テーマの CSS ファイルで、バンドルを作成します。 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  ワイルド カード セレクター"\*.css"は、フォルダー内の各 CSS ファイルを含む、 *Content\themes\base\jquery.ui.all.css*ファイル。 *Jquery.ui.all.css*ファイルは、他の CSS ファイルをインポートします。

## <a name="bundle-caching"></a>キャッシュのバンドルします。

バンドルは、バンドルを作成するときから 1 年間の有効期限が切れる HTTP ヘッダーを設定します。 IE がバンドルの条件付き要求を行わない Fiddler 示しています、前に表示したページに移動する場合はありません、バンドルの IE から HTTP GET 要求と、サーバーから HTTP 304 返信はありません。 (その結果、各バンドルに対する応答を HTTP 304) F5 キーを持つ各バンドルの条件付き要求を行い、IE を強制することができます。 使用して完全更新を強制する ^ f5 キーを押して (その結果、HTTP 200 の応答を各バンドルします)。

次の図は、 **Caching** Fiddler 応答ウィンドウのタブ。

![fiddler のキャッシュのイメージ](bundling-and-minification/_static/image8.png)

要求   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 バンドルは**AllMyScripts**クエリ文字列のペアを格納および**v = r0sLDicvP58AIXN\_mc3QdyVvVj5euZNzdsa2N1PKvb81**。 クエリ文字列**v**はトークンのキャッシュに使用される一意の識別子の値を持ちます。 ASP.NET アプリケーションが要求は、バンドルが変更されない限り、 **AllMyScripts**このトークンを使用してバンドルします。 バンドル内のファイルが変更された場合、ASP.NET optimization フレームワークはバンドルのブラウザーの要求が、最新のバンドルを取得することを保証しながら、新しいトークンを生成します。

IE9 F12 開発者ツールを実行して、以前に読み込まれたページに移動して場合、条件付きの GET 要求を各バンドル、および HTTP 304 を返すサーバーに対して表示しない IE 正しくされます。 IE9 のブログ エントリで条件付きの要求が行われたかどうかの問題が理由を読み取ることができます[を使用して Cdn と Web サイトのパフォーマンスを向上させるには、期限切れ日時](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)します。

## <a name="less-coffeescript-scss-sass-bundling"></a>LESS、CoffeeScript、SCSS、バンドルを Sass します。

バンドルと縮小のフレームワークなど、中間言語を処理するためのメカニズムを提供する[SCSS](http://sass-lang.com/)、 [Sass](http://sass-lang.com/)、[少ない](http://www.dotlesscss.org/)または[Coffeescript](http://coffeescript.org/)、し、結果として得られるバンドルに縮小などの変換を適用します。 たとえば、追加する[*.less](http://www.dotlesscss.org/) MVC 4 プロジェクト ファイル。

1. 以下の内容を格納するフォルダーを作成します。 次の例では、 *Content\MyLess*フォルダー。
2. 追加、 [*.less](http://www.dotlesscss.org/) NuGet パッケージ**ドット**をプロジェクトにします。  
    ![NuGet ドットなしのインストール](bundling-and-minification/_static/image9.png)
3. 実装するクラスを追加、 [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx)インターフェイス。 *.Less トランス フォームをプロジェクトに次のコードを追加します。

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. 小さいファイルのバンドルを作成、`LessTransform`と[CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx)変換します。 次のコードを追加、`RegisterBundles`メソッドで、*アプリ\_Start\BundleConfig.cs*ファイル。

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. 小さいバンドルを参照するすべてのビューには、次のコードを追加します。

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>バンドルの考慮事項

バンドルを作成するときに実行する適切な規則では、バンドル名のプレフィックスとして「バンドル」が含まれます。 これによりする可能性のある[ルーティング競合](https://forums.asp.net/post/5012037.aspx)します。

バンドル内の 1 つのファイルを更新するとは、バンドルのクエリ文字列パラメーターの新しいトークンが生成され、フル バンドルが次回クライアントが、バンドルを含むページを要求をダウンロードする必要があります。 個別に各資産が含まれる場合、従来のマークアップでは、変更されたファイルのみをダウンロードできるようにします。 頻繁に変更される資産は、バンドルの適切な候補をできないがあります。

バンドルと縮小は、最初のページ要求の読み込み時間を向上させる主にします。 (JavaScript、CSS、およびイメージ) の資産をキャッシュと、web ページが要求されたバンドルと縮小は示しません、パフォーマンスの向上、同じページを要求するときにしたり、同じページのサイトと同じ資産を要求するようにします。 設定しない場合、ヘッダー、資産を正しく有効期限が切れるとバンドルと縮小を使用しないでください、ブラウザーの鮮度ヒューリスティックがマーク資産古い数日後およびブラウザーは各資産の検証要求が必要です。 この場合は、バンドルと縮小は、最初のページ要求後にパフォーマンスが向上を指定します。 詳細については、ブログをご覧ください。[を使用して Cdn と Web サイトのパフォーマンスを向上させるには、期限切れ日時](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)します。

使用して各ホスト名ごとに 6 つの同時接続のブラウザーの制限事項を軽減することができます、 [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)します。 あるため、CDN は別のホスト名、ホスティング サイトよりも、CDN からの資産の要求は、ホスティング環境に 6 つの同時接続数の制限に対してカウントされません。 CDN には、一般的なパッケージのキャッシュとのエッジ キャッシュの利点も提供できます。

バンドルは、必要とするページでパーティション分割する必要があります。 たとえば、既定のインターネット アプリケーションの ASP.NET MVC テンプレート バンドルを作成します jQuery 検証 jQuery は別です。 値を投稿しないで作成された既定のビューが入力があるないため、検証のバンドルが含まれますはありません。

`System.Web.Optimization` System.Web.Optimization.DLL で名前空間を実装します。 Antlr3.Runtime.dll を使用しているかを縮小機能のため、WebGrease ライブラリ (WebGrease.dll) を活用しています。

*Twitter を使用して簡単な投稿を行い、リンクを共有します。自分の Twitter ハンドルが*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>その他のリソース

- ビデオ:[バンドルと最適化](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing)によって[Howard dierking が](https://twitter.com/#!/howard_dierking)
- [Web ページ サイトに Web の最適化を追加する](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx)します。
- [追加のバンドルおよび縮小する Web フォーム](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx)します。
- [バンドルのパフォーマンスに影響し、Web 閲覧に縮小](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx)によって[Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [Cdn を使用して Web サイトのパフォーマンスを向上させるために有効期限が切れると](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)Rick anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [RTT (ラウンドト リップ時間) を最小限に抑える](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>共同作成者

- Hao 力
- [Howard dierking が](https://twitter.com/#!/howard_dierking)
- Diana LaRose
