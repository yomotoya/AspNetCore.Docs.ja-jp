---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: ビュー (c#) を追加する |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 50ce4a2024ffd9e2bbb5526717052d486689ff38
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-view-c"></a>ビュー (c#) を追加します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全な非常に簡単に従うしより多くの機能を示します。
> 
> 
> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 開始する前に、以下に示す前提条件がインストールされていることを確認してください。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。 また、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。
> 
> C# ソース コードの Visual Web Developer プロジェクトは、このトピックを使用できます。 [C# バージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。 Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/intro-to-aspnet-mvc-3.md)このチュートリアルのです。


このセクションでしようとしている変更、`HelloWorldController`クリーンにするテンプレート ファイルは、クライアントに HTML 応答を生成するプロセスをカプセル化するビューを使用するクラス。

使用して、新しいビュー テンプレート ファイルを作成する[Razor ビュー エンジン](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)ASP.NET MVC 3 で導入されました。 Razor ベースのビューのテンプレートが、 *.cshtml*ファイル拡張子、および HTML を使用して C# の場合の出力を作成する簡潔な方法を提供します。 Razor 文字とビュー テンプレートの作成時に必要なキーの数を最小化され、ワークフローをコーディングする高速で、流体できます。

ビューのテンプレートを使用して、開始、`Index`メソッドで、`HelloWorldController`クラスです。 現在、`Index` メソッドは、コントローラー クラスでハード コーディングされるメッセージを含む文字列を返します。 変更、`Index`を返すメソッドを`View`オブジェクトを次に示すようにします。

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

このコードでは、ビュー テンプレートを使用して、ブラウザーに HTML 応答を生成します。 プロジェクトで、とともに使用できるビュー テンプレートの追加、`Index`メソッドです。 これを行うには、内部を右クリックして、`Index`メソッドとクリック**ビューの追加**です。

![](adding-a-view/_static/image1.png)

**ビューの追加** ダイアログ ボックスが表示されます。 既定値は、をクリックする方法のままにして、**追加**ボタン。

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld*フォルダーおよび*MvcMovie\Views\HelloWorld\Index.cshtml*ファイルが作成されます。 ご覧**ソリューション エクスプ ローラー**:

![](adding-a-view/_static/image3.png)

次に示す、 *Index.cshtml*作成されたファイル。

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

HTML を追加、`<h2>`タグ。 変更された*MvcMovie\Views\HelloWorld\Index.cshtml*ファイルを次に示します。

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

アプリケーションを実行しを参照、`HelloWorld`コント ローラー (`http://localhost:xxxx/HelloWorld`)。 `Index`コント ローラーでメソッドが多くの作業を実行していない以外の場合は、ステートメントを単純に実行された`return View()`、指定されている、メソッドが、ブラウザーへの応答を表示するためにテンプレート ファイルの表示を使用する必要があります。 ASP.NET MVC の既定値を使用してに明示的に使用するビューのテンプレート ファイルの名前を指定しませんでしたので、 *Index.cshtml*でファイルの表示、 *\Views\HelloWorld*フォルダーです。 次の図は、ビューで、ハードコーディング文字列を示しています。

![](adding-a-view/_static/image6.png)

なかなか良いを検索します。 ただし、"Index"ブラウザーのタイトル バーに表示し、ページで、大きなタイトルは「My MVC アプリケーションです」ことに注意してください。 これらの名前を変更してみましょう。

## <a name="changing-views-and-layout-pages"></a>ビューとレイアウト ページを変更します。

最初に、ページの上部にある「マイ MVC アプリケーション」タイトルを変更します。 テキストがすべてのページに共通します。 実際に実装するもの、プロジェクト内の 1 か所で、アプリケーション内の各ページに表示される場合でも。 移動して、 */ビュー/共有*フォルダー**ソリューション エクスプ ローラー**を開くと、  *\_Layout.cshtml*ファイル。 このファイルが呼び出され、*レイアウト ページ*は共有「シェル」その他のすべてのページを使用するとします。

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

レイアウト テンプレートを使用すると、1 か所で、コンテナーの HTML レイアウトは、サイトを指定し、サイト内の複数のページにわたって適用できます。 注、`@RenderBody()`ファイルの下部にある行。 `RenderBody` 場所を作成するすべてのビューに固有のページを表示する、レイアウト ページで「ラップされた」プレース ホルダー。 「MVC ムービー アプリ」に「マイ MVC アプリケーション」からレイアウト テンプレートのタイトルの見出しを変更します。

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

アプリケーションを実行し、今すぐ"MVC ムービー App"をということに注意してください。 クリックして、**に関する**リンク、および、そのページがすぎます"MVC ムービー App"を表示を参照してください。 レイアウト テンプレートに 1 回、変更を行うことができましたが、サイトのすべてのページで、新しいタイトルを反映します.

![](adding-a-view/_static/image9.png)

完全な *\_Layout.cshtml*ファイルを次に示します。

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

ここで、インデックス ページ (ビュー) のタイトルを変更してみましょう。

Open *MvcMovie\Views\HelloWorld\Index.cshtml*. 2 つの場所を変更する場合がある: 最初に、テキスト表示される、ブラウザーのタイトルにし、セカンダリのヘッダー (、`<h2>`要素)。 これを少し変えれば、コードのどの部分でアプリのどの部分が変更されるかを確認することができます。

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

HTML 表示するタイトルをセットの上に、コードを示すために、`Title`のプロパティ、`ViewBag`オブジェクト (である、 *Index.cshtml*テンプレートの表示)。 戻るレイアウト テンプレートのソース コードを見ることがわかりますテンプレートがこの値を使用する、`<title>`要素の一部として、 `<head>` HTML の「します。 このアプローチを使用して、ビューのテンプレートと、レイアウト ファイルの間での他のパラメーターを簡単に渡すことができます。

アプリケーションを実行しを参照`http://localhost:xx/HelloWorld`です。 ブラウザーのタイトル、プライマリ見出し、およびセカンダリ見出しが変更されていることに注意してください  (ブラウザーに変更内容が表示されない場合は、キャッシュされたコンテンツを表示している可能性があります。 ブラウザーで Ctrl + F5 キーを押して、サーバーからの応答が強制的に読み込まれるようにしてください)。

また方法でコンテンツ、 *Index.cshtml*ビュー テンプレートのトピックとマージされました、  *\_Layout.cshtml*ビュー テンプレートおよび 1 つの HTML 応答をブラウザーに送信されました。 レイアウト テンプレートを使用すれば、アプリケーションのすべてのページに適用される変更をとても簡単に行うことができます。

![](adding-a-view/_static/image10.png)

ここでは、"データ" のごく一部 (この場合は "Hello from our View Template!" というメッセージ) を ハード コーディングしました。 MVC アプリケーションには "V" (ビュー) があり、"C" (コントローラー) もありますが、"M" (モデル) はまだありません。 間もなく、方法を説明するデータベースを作成し、モデル データを取得します。

## <a name="passing-data-from-the-controller-to-the-view"></a>コントローラーからビューへのデータの受け渡し

データベースに移動し、モデルについて説明する前に、しましょう。 まずコント ローラーからビューに情報を渡す処理です。 受信 URL 要求に応答には、コント ローラー クラスが呼び出されます。 コント ローラー クラスでは、受信パラメーターを処理、データがデータベースから取得、および最終的には、ブラウザーに返送する応答の種類を決定するコードを記述します。 テンプレートの表示は、生成し、ブラウザーに HTML 応答の書式を設定するコント ローラーから使用できます。

コント ローラーは、ブラウザーへの応答をレンダリングするビュー テンプレートの順に必要なすべてのデータまたはオブジェクトを提供します。 ビュー テンプレートは、ビジネス ロジックを実行したり、データベースと直接やり取りする必要がありますしないでください。 代わりに、コント ローラーに設定されているデータでのみ処理が必要です。 この「関心の分離」を維持することは、クリーンと保守の容易なコードを維持に役立ちます。

現時点では、`Welcome`でアクション メソッド、`HelloWorldController`クラスは、`name`と`numTimes`パラメーターと、ブラウザーに直接値を出力します。 この応答を文字列としてのレンダリング コント ローラーがあるのではなくビュー テンプレートを代わりに使用するコント ローラーに変更してみましょう。 このビュー テンプレートでは動的応答が生成されます。これは、応答を生成するために、コントローラーからビューに適量のデータを渡す必要があることを意味します。 コント ローラーにテンプレートの表示に必要な動的なデータをまとめることによってこれを行う、`ViewBag`ビュー テンプレートにアクセスできるオブジェクト。

戻り、 *HelloWorldController.cs*ファイルし、変更、`Welcome`を追加するメソッド、`Message`と`NumTimes`値を`ViewBag`オブジェクト。 `ViewBag` 動的なオブジェクトは、つまりに自由を配置する;`ViewBag`オブジェクトを持たないプロパティに定義されたものと、その内部に配置するまでです。 完全な *HelloWorldController.cs* ファイルは次のようになります。

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

これで、`ViewBag`オブジェクトには、自動的にビューに渡されるデータが含まれています。

次に、開始ビュー テンプレートする必要があります。 **デバッグ**メニューの **ビルド MvcMovie**プロジェクトをコンパイルするかどうかを確認します。

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

内で右クリックし、`Welcome`メソッドとクリック**ビューの追加**です。 ここでは、どのような**ビューの追加** ダイアログ ボックスはようになります。

![](adding-a-view/_static/image13.png)

をクリックして**追加**、下にある次のコードを追加し、 `<h2>` 、新しい要素*Welcome.cshtml*ファイル。 数は、ユーザーの質問と同じ回数「こんにちは」と表示されているループを作成します。 完全な*Welcome.cshtml*ファイルを次に示します。

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

アプリケーションを実行し、次の URL を参照します。

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

今すぐデータが URL から取得され、コント ローラーに自動的に渡されます。 コント ローラーへのデータをパッケージ化、`ViewBag`オブジェクトをビューにオブジェクトを渡します。 ビューは、データを HTML として、ユーザーにし、表示します。

![](adding-a-view/_static/image14.png)

"M" (モデル) については学習しましたが、データベースについてはまだです。 学習したことを確認し、ムービーのデータベースを作成してみましょう。

> [!div class="step-by-step"]
> [前へ](adding-a-controller.md)
> [次へ](adding-a-model.md)
