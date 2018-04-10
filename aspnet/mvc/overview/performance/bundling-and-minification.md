---
uid: mvc/overview/performance/bundling-and-minification
title: バンドルと縮小 |Microsoft ドキュメント
author: Rick-Anderson
description: 2 つの手法のバンドルと縮小要求読み込み時間を向上させるために ASP.NET 4.5 で使用することができます。 バンドルと縮小 reducin して読み込み時間が向上しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/23/2012
ms.topic: article
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 001ebf89cda66a50cddcd7e4944f27b9396d4450
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
---
<a name="bundling-and-minification"></a>バンドルと縮小
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> 2 つの手法のバンドルと縮小要求読み込み時間を向上させるために ASP.NET 4.5 で使用することができます。 バンドルと縮小サーバーへの要求の数が減り、要求されたアセット (CSS および JavaScript を使用します。) などのサイズを小さくして読み込み時間を向上させます。


現在の主なブラウザーのほとんどの数を制限する[同時接続](http://www.browserscope.org/?category=network)ごとに 6 個の各ホスト名です。 つまり、6 つの要求が処理中に、ホスト上のアセットの追加の要求はブラウザーによってキューすることです。 次の図では、IE F12 開発者ツール ネットワーク タブは、サンプル アプリケーションのバージョン情報の表示に必要な資産のタイミングを示します。

![B/M](bundling-and-minification/_static/image1.png)

グレーのバーは、6 つの接続の制限を待機しているブラウザーによって、要求がキューに置かれた時刻を表示します。 黄色のバーは、最初のバイトまで要求時間、要求を送信し、サーバーからの最初の応答の受信にかかった時間は、します。 青いバーは、サーバーからの応答データを受信するのにかかる時間を表示します。 詳細なタイミング情報を取得する資産をダブルクリックすることができます。 たとえば、次の図は読み込みのタイミングの詳細を示しています。、 */Scripts/MyScripts/JavaScript6.js*ファイル。

![](bundling-and-minification/_static/image2.png)

上記の図に示す、**開始**イベント、どのにより、ブラウザーのため、要求がキューに時間が同時接続の数を制限します。 ここでは、要求はキューに置か 46 (ミリ秒) の別の要求が完了するを待っています。

## <a name="bundling"></a>バンドル化

バンドル化とは、簡単に結合または 1 つのファイルを複数のファイルをバンドルする ASP.NET 4.5 での新機能です。 CSS、JavaScript、およびその他のバンドルを作成することができます。 ファイル数を減らしてでは、少数の HTTP 要求をするには、最初のページ読み込みのパフォーマンスを向上できますを意味します。

次の図は、この時点でバンドルと縮小が有効になっているが、以前は、表示されているバージョン情報ビューの同じタイミング ビューを示します。

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>縮小

縮小では、さまざまなスクリプトまたは不要な空白文字とコメントを削除して、1 文字に変数名を短くなど、css を別のコードの最適化を実行します。 次の JavaScript 関数を検討してください。

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

縮小、後に、関数は、次に縮小されます。

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

コメントと不要な空白文字を削除するだけでなく、次のパラメーターと変数の名前が変更された (切り捨て) には、次のように。

| **翻訳元** | **名前変更** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | i |

## <a name="impact-of-bundling-and-minification"></a>バンドルの影響と縮小

次の表は、すべてのアセットを個別に一覧表示して、サンプル プログラムのバンドルと縮小 (B/M) を使用してのいくつかの重要な違いを示します。

|  | **B/M を使用します。** | **B/M なし** | **変更** |
| --- | --- | --- | --- |
| **ファイルの要求** | 9 | 34 | 256% |
| **サポート技術情報の送信** | 3.26 | 11.92 | 266% |
| **KB 受信** | 388.51 | 530 | 36% |
| **読み込み時間** | 510 MS | 780 MS | 53% |

送信されたバイト数には、ブラウザーは要求に適用される HTTP ヘッダーを非常に冗長のバンドルを大幅に軽減が必要があります。 受信したバイト数の削減がためサイズの大きなファイル (*Scripts\jquery-ui-1.8.11.min.js*と*Scripts\jquery-1.7.1.min.js*) 縮小は既にです。 注: 使用するサンプル プログラムでタイミング、 [Fiddler](http://www.fiddler2.com/fiddler2/)低速なネットワークをシミュレートするツールです。 (、Fiddler から**ルール**メニューの [**パフォーマンス**し**モデム スピードのシミュレート**)。

## <a name="debugging-bundled-and-minified-javascript"></a>デバッグとバンドルおよび JavaScript の縮小

簡単に開発環境で、JavaScript のデバッグ (ここで、 [compilation 要素](https://msdn.microsoft.com/library/s10awwz0.aspx)で、 *Web.config*に設定されているファイル`debug="true"`)、JavaScript ファイルが組み込まれていないため、または縮小します。 JavaScript ファイルのバンドルし、縮小の場所は、リリース ビルドをデバッグすることもできます。 IE F12 開発者ツールを使用して、次のアプローチを使用して縮小されたバンドルに含まれる JavaScript 関数をデバッグします。

1. 選択、**スクリプト**タブをクリックし、**デバッグを開始**ボタンをクリックします。
2. [アセット] ボタンを使用してデバッグする JavaScript 関数を含むバンドルを選択します。  
    ![](bundling-and-minification/_static/image4.png)
3. JavaScript の縮小を書式設定を選択して、**構成ボタン**![](bundling-and-minification/_static/image5.png)を選択し**形式 JavaScript**です。
4. **検索スクリプト**t 入力ボックス、デバッグする関数の名前を選択します。 次の図の**AddAltToImg**で入力した、**検索スクリプト**t 入力ボックス。  
    ![](bundling-and-minification/_static/image6.png)

F12 開発者ツールでのデバッグの詳細については、MSDN の記事を参照してください。 [JavaScript エラーのデバッグに F12 開発者ツールを使用して](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx)です。

## <a name="controlling-bundling-and-minification"></a>制御バンドルと縮小

バンドルし縮小が有効か無効でデバッグ属性の値を設定して、 [compilation 要素](https://msdn.microsoft.com/library/s10awwz0.aspx)で、 *Web.config*ファイル。 次の XML で`debug`はこれを true に設定するバンドルと縮小が無効になっています。

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

バンドルと縮小を有効にするには設定、`debug`値を"false"です。 オーバーライドすることができます、 *Web.config*による設定、`EnableOptimizations`プロパティを`BundleTable`クラスです。 次のコードは、バンドルと縮小を有効にしのすべての設定を上書き、 *Web.config*ファイル。

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> しない限り、`EnableOptimizations`は`true`またはデバッグ属性、 [compilation 要素](https://msdn.microsoft.com/library/s10awwz0.aspx)で、 *Web.config*に設定されているファイル`false`ファイルはバンドルまたは縮小されません。 さらに、.min バージョンのファイルは使用されませんが、完全なデバッグ バージョンが選択されます。 `EnableOptimizations` [デバッグの属性をオーバーライドします、 [compilation 要素](https://msdn.microsoft.com/library/s10awwz0.aspx)で、 *Web.config*ファイル


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>バンドルを使用して ASP.NET Web フォームと Web ページの縮小

- Web ページは、ブログ エントリを参照してください。 [Web 最適化 Web Pages サイトを追加する](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx)です。
- Web フォーム、ブログ記事を参照してください。[バンドルを追加すると Web フォームに縮小](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx)です。

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>バンドルを使用して、ASP.NET MVC での縮小

このセクションの内容をバンドルを確認するプロジェクトと縮小 ASP.NET MVC を作成します。 という名前の新しい ASP.NET MVC インターネット プロジェクトを最初に、作成**MvcBM**の既定値のいずれかを変更することがなくです。

開く、*アプリ\_Start\BundleConfig.cs*ファイルを確認、`RegisterBundles`メソッドを作成し、登録し、バンドルを構成するために使用します。 次のコードの一部を示しています、`RegisterBundles`メソッドです。

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

上記のコードでは、という名前の新しい JavaScript バンドル*~/bundles/jquery*適切なすべてが含まれている (デバッグまたはが縮小する*。vsdoc*) 内のファイル、*スクリプト*ワイルドカード文字列"~/Scripts/jquery-{バージョン} .js"に一致するフォルダーです。 ASP.NET MVC 4 では、つまり、デバッグ構成では、ファイル*jquery 1.7.1.js*バンドルに追加されます。 構成では、リリース、 *jquery 1.7.1.min.js*追加されます。 バンドルのフレームワークにはなどのいくつかの一般的な規則が次に示します。

- "FileX.min.js"と"FileX.js"が存在する場合は、".min"リリースのファイルを選択します。
- デバッグ用の非".min"バージョンを選択します。
- 無視しています"-vsdoc"IntelliSense でのみ使用されるファイル (jquery-1.7.1-vsdoc.js) などです。

`{version}` JQuery の適切なバージョン jQuery バンドルを自動的に作成に使用前に示したワイルドカードに一致する、*スクリプト*フォルダーです。 この例ではワイルドカードを使用すると、次の利点があります。

- NuGet を使用してバンドルの前のコードまたはページの表示での jQuery 参照を変更することがなく jQuery の新しいバージョンに更新できます。
- 自動的に選択、フル バージョンのデバッグ構成とリリース".min"バージョンを作成します。

## <a name="using-a-cdn"></a>CDN を使用します。

 次のコードは、ローカルの jQuery バンドルを CDN jQuery バンドルに置き換えます。

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

上記のコードで jQuery からの要求が CDN のリリースでモードと jQuery のデバッグ バージョンはフェッチされてからローカルでデバッグ モードで中にします。 CDN を使用する場合、CDN 要求が失敗した場合にフォールバック メカニズムが必要です。 次のマークアップは、jQuery は CDN 失敗要求に追加レイアウト ファイルはスクリプトの末尾からフラグメントします。

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>バンドルを作成します。

[バンドル](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx)クラス`Include`メソッドは各文字列がリソースへの仮想パスをここでは、文字列の配列を受け取ります。 次のコードで RegisterBundles メソッドから、*アプリ\_Start\BundleConfig.cs*ファイルは、複数のファイルは、バンドルに追加を示しています。

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

[バンドル](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx)クラス`IncludeDirectory`ディレクトリ (および必要に応じてすべてのサブディレクトリ) 内の指定された検索パターンに一致するすべてのファイルを追加するメソッドを提供します。 [バンドル](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx)クラス`IncludeDirectory`API を次に示します。

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Render メソッドを使用して、ビューで参照されるバンドル ( `Styles.Render` CSS のおよび`Scripts.Render`JavaScript 用)。 次のマークアップ、 *\shared\\_Layout.cshtml*ファイルは、既定の ASP.NET インターネット プロジェクトのビューで CSS および JavaScript のバンドルを参照する方法を示しています。

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Render メソッドは、コードの 1 つの行で複数のバンドルを追加するために、文字列の配列を受け取ることがわかります。 アセットを参照するために必要な HTML を作成する Render メソッドを使用する場合は一般にします。 使用することができます、`Url`マークアップのアセットを参照するために必要な資産への URL を生成する方法です。 新しい HTML5 を使用すると想定[async](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async)属性。 次のコードは、modernizr を使用して参照する方法を示します、`Url`メソッドです。

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>使用して、"\*"ファイルを選択するワイルドカード文字

指定された仮想パス、`Include`のメソッドと、検索パターン、`IncludeDirectory`メソッドには、いずれかを指定できる"\*"プレフィックスまたは最後のパス セグメント内にサフィックスとしてワイルドカード文字です。 検索文字列では大文字小文字を区別します。 `IncludeDirectory`メソッドには、オプションのサブディレクトリを検索します。

次の JavaScript ファイルでは、プロジェクトを考慮してください。

- *Scripts\Common\AddAltToImg.js*
- *Scripts\Common\ToggleDiv.js*
- *Scripts\Common\ToggleImg.js*
- *Scripts\Common\Sub1\ToggleLinks.js*

![dir imag](bundling-and-minification/_static/image7.png)

次の表のように、ワイルドカードを使用してバンドルに追加されたファイルを示しています。

| **Call** | **ファイルを追加するか、例外が発生しました** |
| --- | --- |
| Include("~/Scripts/Common/\*.js") | *AddAltToImg.js、ToggleDiv.js、ToggleImg.js* |
| Include("~/Scripts/Common/T\*.js") | 無効なパターンの例外。 ワイルドカード文字はプレフィックスまたはサフィックスにのみ使用できます。 |
| Include("~/Scripts/Common/\*og.\*") | 無効なパターンの例外。 1 つだけのワイルドカード文字を許可します。 |
| "Include("~/Scripts/Common/T\*") | *ToggleDiv.js、ToggleImg.js* |
| "Include("~/Scripts/Common/\*") | 無効なパターンの例外。 純粋なワイルドカード セグメントが正しくありません。 |
| IncludeDirectory("~/Scripts/Common", "T\*") | *ToggleDiv.js、ToggleImg.js* |
| IncludeDirectory("~/Scripts/Common", "T\*",true) | *ToggleDiv.js、ToggleImg.js、ToggleLinks.js* |

バンドルに各ファイルを明示的に追加することをお勧め一般に、次のようなファイルの読み込みのワイルドカード経由で。

- 既定ではワイルドカード スクリプトを追加して、これは通常たくない、アルファベット順に読み込みます。 CSS および JavaScript ファイルは頻繁に (アルファベット以外の) 特定の順序で追加する必要があります。 このリスクを軽減するには、カスタムの追加を[IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx)の実装が各ファイルは、以下のエラーが発生しやすいを明示的に追加します。 たとえば、変更する必要がありますが、将来のフォルダーに新しいアセットを追加する可能性があります、 [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx)実装します。
- 読み込みワイルド カードを使用してディレクトリに追加されるビューの特定のファイルは、そのバンドルを参照しているすべてのビューに含めることができます。 バンドルには、ビューの特定のスクリプトを追加する場合、バンドルを参照している他のビューに JavaScript エラーが発生した可能性があります。
- その他のファイルがインポートされる CSS ファイルは、2 回読み込まれているインポートされたファイルで発生します。 たとえば、次のコードは、ほとんどの 2 回読み込んで jQuery UI のテーマの CSS ファイルをバンドルを作成します。 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  ワイルド カード セレクター"\*.css"では、フォルダー内の各 CSS ファイル内を含め、 *Content\themes\base\jquery.ui.all.css*ファイル。 *Jquery.ui.all.css*ファイルは他の CSS ファイルをインポートします。

## <a name="bundle-caching"></a>バンドルのキャッシュ

バンドルは、バンドルが作成された時点から 1 年間の有効期限が切れる HTTP ヘッダーを設定します。 前に表示したページ、IE がバンドルの条件付き要求を行わない Fiddler 表示に移動する場合はありません、バンドルの IE から HTTP GET 要求およびサーバーからの HTTP 304 応答はありません。 (各バンドルの HTTP 304 応答に結果として得られる) F5 キーを持つ各バンドルの条件付き要求を行い、IE を強制することができます。 使用して完全更新を強制できます ^ f5 キーを押して (各バンドルの HTTP 200 の応答に結果として得られる)。

次の図は、**キャッシュ**Fiddler 応答] ウィンドウのタブ。

![fiddler キャッシュ イメージ](bundling-and-minification/_static/image8.png)

要求   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 バンドルは**AllMyScripts**クエリ文字列のペアを格納および**v = r0sLDicvP58AIXN\_mc3QdyVvVj5euZNzdsa2N1PKvb81**です。 クエリ文字列**v**トークンの一意の識別子のキャッシュに使用されている値を持ちます。 ASP.NET アプリケーションが要求は、バンドルに変化がない限り、 **AllMyScripts**このトークンを使用してバンドルします。 バンドル内のファイルが変更された場合、ASP.NET optimization フレームワークにはバンドルのブラウザーの要求が、最新のバンドルを取得するを確保し、新しいトークンが生成されます。

IE9 F12 開発者ツールを実行して以前に読み込まれたページに移動すると、IE 誤って表示各バンドルおよび HTTP 304 を返すサーバーに対する条件付きの GET 要求。 IE9 のブログ エントリで条件付き要求が行われたかどうかの問題が理由を読み取ることができる[Cdn を使用して、Web サイト パフォーマンスを向上させる Expires](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)です。

## <a name="less-coffeescript-scss-sass-bundling"></a>小さい、CoffeeScript、SCSS、バンドルを Sass です。

バンドルと縮小フレームワークなど、中間言語を処理するメカニズムを提供する[SCSS](http://sass-lang.com/)、 [Sass](http://sass-lang.com/)、[小さい](http://www.dotlesscss.org/)または[Coffeescript](http://coffeescript.org/)、結果として得られるバンドルへの縮小などの変換を適用するとします。 たとえば、追加する[*.less](http://www.dotlesscss.org/) MVC 4 プロジェクトにファイルを。

1. 小さいコンテンツ用のフォルダーを作成します。 次の例では、 *Content\MyLess*フォルダーです。
2. 追加、 [*.less](http://www.dotlesscss.org/) NuGet パッケージ**ドットなし**をプロジェクトにします。  
    ![NuGet ドットなしのインストール](bundling-and-minification/_static/image9.png)
3. 実装するクラスを追加、 [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx)インターフェイスです。 *.Less トランス フォームをプロジェクトに次のコードを追加します。

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. 小さいファイルのバンドルを作成、`LessTransform`と[CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx)を変換します。 次のコードを追加、`RegisterBundles`メソッドで、*アプリ\_Start\BundleConfig.cs*ファイル。

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. 小さいバンドルを参照している任意のビューには、次のコードを追加します。

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>バンドルの考慮事項

バンドルを作成するときに実行する適切な規則は、バンドル名のプレフィックスとして「バンドル」を含めるにです。 こうと、可能な限り[ルーティングの競合](https://forums.asp.net/post/5012037.aspx)です。

バンドル内の 1 つのファイルを更新すると後、は、バンドルのクエリ文字列パラメーターの新しいトークンが生成され、完全バンドルが、次回クライアントが、バンドルを含むページを要求をダウンロードする必要があります。 従来のマークアップが個別に各アセットが含まれる場合、変更されたファイルのみをダウンロードするとします。 頻繁に変更される資産のバンドルのよい候補になりましていない可能性があります。

バンドルと縮小、主に、最初のページ要求の読み込み時間を向上します。 ブラウザー キャッシュ資産 (JavaScript、CSS、および画像) で web ページが要求されると後、同じページを要求するときに、バンドルと縮小すると、すべてのパフォーマンスが向上が提供するされませんやページは、同じサイトの同じアセットを要求するようにします。 設定しない場合、ヘッダー、資産を正しく有効期限が切れるとバンドルと縮小を使用しない場合、ブラウザーの鮮度ヒューリスティックとしてマークされます資産古い数日後とブラウザー資産ごとの検証要求が必要です。 この例では、バンドルと縮小は、最初のページ要求の後に、パフォーマンスが向上を提供します。 詳細については、ブログを参照してください。 [Cdn を使用して、Web サイト パフォーマンスを向上させる Expires](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)です。

使用してすべてのホスト名ごとに 6 つの同時接続のブラウザーの制限事項を軽減することができます、 [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)です。 CDN が別のホスト名、ホスティング サイトよりであるために、CDN からの資産の要求は、ホスティング環境に 6 つの同時接続数の制限としてはカウントされません。 CDN には、一般的なパッケージのキャッシュおよびエッジ キャッシュの利点も提供できます。

バンドルを必要とするページでパーティション分割する必要があります。 たとえば、既定のインターネット アプリケーションの ASP.NET MVC テンプレートを作成 jQuery 検証バンドル jQuery と区別します。 値を投稿しないで、作成される既定のビューが入力があるないため、検証のバンドルは記述していません。

`System.Web.Optimization` System.Web.Optimization.DLL で名前空間の実装です。 活用し WebGrease ライブラリ (WebGrease.dll) のサイズ縮小機能 Antlr3.Runtime.dll を使用します。

*クイック投稿を行うし、リンクを共有する Twitter を使用します。マイ Twitter のハンドルが*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>その他のリソース

- ビデオ:[バンドル化と最適化](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing)によって[Howard Dierking](https://twitter.com/#!/howard_dierking)
- [最適化の Web サイトに追加する、Web ページ](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx)です。
- [追加するバンドルと縮小 Web フォームを](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx)です。
- [バンドルのパフォーマンスに及ぼす影響と Web サイトの参照に縮小](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx)によって[佐藤さん F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [Cdn を使用して Web サイトのパフォーマンスを向上させるために有効期限が切れると](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx)Rick anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [RTT (ラウンドト リップ時間) を最小限に抑える](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>共同作成者

- 最後にハオ トリム
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
