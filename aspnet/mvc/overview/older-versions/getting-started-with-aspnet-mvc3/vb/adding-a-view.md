---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: "ビュー (VB) を追加する |Microsoft ドキュメント"
author: Rick-Anderson
description: "このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 7e8564c743510780b93d56bc1215f4c5b1faeb43
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-view-vb"></a>ビュー (VB) を追加します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 開始する前に、以下に示す前提条件がインストールされていることを確認してください。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。 また、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。
> 
> VB.NET のソース コードと Visual Web Developer プロジェクトは、このトピックを使用できます。 [バージョンをダウンロードして VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。 C# を使用する場合に切り替え、 [c# バージョン](../cs/adding-a-view.md)このチュートリアルのです。


このセクションではことを変更する、`HelloWorldController`クリーンにするビュー テンプレート ファイルを使用するクラスがクライアントに HTML 応答を生成するプロセスをカプセル化します。

ビューのテンプレートを使用して、まず、`Index`メソッドで、`HelloWorldController`クラスです。 現在、`Index`メソッドがコント ローラー クラス内でハードコーディングされているメッセージに文字列を返します。 変更、`Index`を返すメソッドを`View`オブジェクトを次に示すようにします。

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

呼び出すことのできるおプロジェクトにビュー テンプレートを追加してみましょうようになりました、`Index`メソッドです。 これを行うには、内部を右クリックして、`Index`メソッドとクリック**ビューの追加**です。

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

**ビューの追加** ダイアログ ボックスが表示されます。 既定のエントリのままにし、をクリックして、**追加**ボタンをクリックします。

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

*MvcMovie\Views\HelloWorld*フォルダーおよび*MvcMovie\Views\HelloWorld\Index.vbhtml*ファイルが作成されます。 ご覧**ソリューション エクスプ ローラー**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

HTML を追加、`<h2>`タグ。 変更された*MvcMovie\Views\HelloWorld\Index.vbhtml*ファイルを次に示します。

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

アプリケーションを実行しを参照、 &quot;hello world&quot;コント ローラー (`http://localhost:xxxx/HelloWorld`)。 `Index`コント ローラーでメソッドが多くの作業を実行していない以外の場合は、ステートメントを単純に実行された`return View()`クライアントへの応答を表示するためにテンプレート ファイルの表示を使用したいことが示されます。 ASP.NET MVC の既定値を使用してに明示的を使用するビューのテンプレート ファイルの名前を指定していない、ため、 *Index.vbhtml*内でファイルの表示、 *\Views\HelloWorld*フォルダーです。 次の図は、ビューで、ハードコーディング文字列を示しています。

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

なかなか良いを検索します。 ただし、ブラウザーのタイトル バーに表示されることを確認&quot;インデックス&quot; ページで大きなタイトルという&quot;マイ MVC アプリケーション。&quot;これらの名前を変更してみましょう。

## <a name="changing-views-and-layout-pages"></a>ビューとレイアウト ページの変更

最初に、テキストを変更してみましょう&quot;マイ MVC アプリケーション。&quot;そのテキストを使用して、共有は、すべてのページに表示されます。 アプリケーション内の各ページ上にあるいても実際には、プロジェクト内の 1 か所に表示します。 移動して、 */ビュー/共有*フォルダー**ソリューション エクスプ ローラー**を開くと、  *\_Layout.vbhtml*ファイル。 このファイルし、呼ばれ、レイアウト ページ、共有は&quot;シェル&quot;他のすべてのページを使用します。

注、`@RenderBody()`ファイルの下部にあるコードの行。 `RenderBody`場所を作成するすべてのページは表示のプレース ホルダー&quot;ラップ&quot;レイアウト ページでします。 変更、`<h1>`から見出し **&quot;** マイ MVC アプリケーション&quot;に&quot;MVC ムービー アプリ&quot;です。

[!code-html[Main](adding-a-view/samples/sample3.html)]

アプリケーションを実行し、今すぐということに注意してください&quot;MVC ムービー アプリ&quot;です。 をクリックして、**に関する**リンク、およびページ表示&quot;MVC ムービー アプリ&quot;、すぎます。

完全な *\_Layout.vbhtml*ファイルを次に示します。

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

ここで、インデックス ページ (ビュー) のタイトルを変更してみましょう。

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

開いている*MvcMovie\Views\HelloWorld\Index.vbhtml*です。 2 つの場所を変更する場合がある: 最初に、テキスト表示される、ブラウザーのタイトルにし、セカンダリのヘッダー (、`<h2>`要素)。 しましょうに若干異なるため、どのビット コードの変更、アプリのどの部分を参照してください。

アプリケーションを実行しを参照`http://localhost:xx/HelloWorld`です。 ブラウザーのタイトル、プライマリ見出し、およびセカンダリ見出しが変更されていることに注意してください  ビューに小さな変更を使用してアプリケーションに大きな変更を加えるには簡単です。 (ブラウザーに変更内容が表示されない場合は、キャッシュされたコンテンツを表示している可能性があります。 ブラウザーで Ctrl + F5 キーを押して、サーバーからの応答が強制的に読み込まれるようにしてください)。

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

当社ほんの少しの&quot;データ&quot;(ここでは、 &quot;Hello World!&quot;メッセージ) は、ハードコードです。 MVC アプリケーションは V (views) を持ち、C (コント ローラー) がないの M (モデル)。 まだお手伝いします。 間もなく、方法を説明するデータベースを作成し、モデル データを取得します。

## <a name="passing-data-from-the-controller-to-the-view"></a>コントローラーからビューへのデータの受け渡し

データベースに移動し、モデルについて説明する前に、しましょう。 まずコント ローラーからビューに情報を渡す処理です。 クライアントへの HTML 応答を表示するために必要とビューのテンプレートを通過することができます。 これらのオブジェクトが通常作成され、コント ローラー クラスによって、ビュー テンプレートに渡され、テンプレートの表示を必要とするデータのみを含んでいる必要があります: とは。

以前、`HelloWorldController`クラス、`Welcome`アクション メソッドがかかりました、`name`と`numTimes`パラメーターと、パラメーターの値をブラウザーにし、出力します。 はなくこの応答を直接表示するために続行コント ローラーがあるより代わりにおを見ていきましょうデータ バッグに、ビューの。 コント ローラーとビューを使用できる、`ViewBag`をそのデータを保持するオブジェクト。 経由で渡されるビュー テンプレートに自動的に、しするデータとしてバッグの内容を使用して、HTML 応答を表示するために使用します。 このようにして、1 つの点とそのと他のビュー テンプレートに関しては、コント ローラー: クリーンを維持することを有効にする&quot;関心の分離&quot;アプリケーション内で。

代わりに、おでしたカスタム クラスを定義、独自にそのオブジェクトのインスタンスを作成、データを入力し、ビューに渡します。 カスタム ビューのモデルになっているため、ViewModel 多くの場合、呼び出されます。 少量のデータは、ただし、ViewBag うまく動作します。

戻り、 *HelloWorldController.vb*ファイルの変更、 `Welcome` ViewBag をメッセージと NumTimes にコント ローラーの内部メソッドです。 ViewBag は、動的なオブジェクトです。 つまりに自由に配置することができます。 その中のものを配置するまで、ViewBag は定義されているプロパティがありません。

完全な`HelloWorldController.vb`同じファイルで、新しいクラスを使用します。

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

これで、ViewBag には、渡される経由でのビューに自動的にデータが含まれています。 もう一度、またはでしたが渡された次のように、独自のオブジェクトは良かった場合。

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

いく必要があります、`WelcomeView`テンプレートです。 新しいコードがコンパイルされるために、アプリケーションを実行します。 ブラウザーを閉じるには、内部を右クリックして、`Welcome`メソッド、およびクリック**ビューの追加**です。

ここでは、どのような**ビューの追加** ダイアログ ボックスはようになります。

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

次のコードを追加、 `<h2>` 、新しい要素*へようこそ* 。vbhtml ファイルです。 うまくループを作成し、言う&quot;こんにちは&quot;ユーザーの質問がお回数だけです。

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

アプリケーションを実行しを参照`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

今すぐデータが URL から取得され、コント ローラーに自動的に渡されます。 コント ローラーへのデータをパッケージ化、`Model`オブジェクトをビューにオブジェクトを渡します。 ビューよりも、ユーザーに html 形式でデータが表示されます。

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

種類もの&quot;M&quot;モデルがデータベースの種類ではありません。 学習したことを確認し、ムービーのデータベースを作成してみましょう。

>[!div class="step-by-step"]
[前へ](adding-a-controller.md)
[次へ](adding-a-model.md)
