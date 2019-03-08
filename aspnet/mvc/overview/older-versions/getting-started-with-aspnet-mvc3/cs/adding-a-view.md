---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: 追加すると表示されます (C#) |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: riande
ms.date: 01/12/2011
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 9fc8c6cad44016511c462b4206c28ea3449ff97e
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576613"
---
<a name="adding-a-view-c"></a>ビュー (C#) を追加します。
====================
によって[Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全ではるかに簡単に従うしより多くの機能を示します。
> 
> 
> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 始める前に、以下の前提条件がインストールされていることを確認します。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。 または、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。
> 
> C# ソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。 [C# バージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)します。 Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/intro-to-aspnet-mvc-3.md)このチュートリアルの。


このセクションではしようとしている変更、`HelloWorldController`ビュー テンプレート ファイルを明確には、クライアントへの HTML 応答を生成するプロセスをカプセル化を使用するクラス。

新しいビュー テンプレート ファイルを作成します[Razor ビュー エンジン](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)ASP.NET MVC 3 で導入されました。 Razor ベースのビュー テンプレートが、 *.cshtml*ファイル拡張子、および HTML 出力を C# を使用して作成する洗練された方法を提供します。 Razor の文字と、ビュー テンプレートの作成時に必要なキーストローク数を最小化し、ワークフローのコーディングの高速で、流体できます。

ビュー テンプレートを使用して開始、`Index`メソッドで、`HelloWorldController`クラス。 現在、`Index` メソッドは、コントローラー クラスでハード コーディングされるメッセージを含む文字列を返します。 変更、`Index`を返すメソッドを`View`オブジェクトを次に示すようにします。

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

このコードでは、ビュー テンプレートを使用して、ブラウザーへの HTML 応答を生成します。 プロジェクトで、追加で使用できるテンプレートの表示、`Index`メソッド。 これを行うには、内を右クリックして、`Index`メソッドとクリック**ビューの追加**します。

![](adding-a-view/_static/image1.png)

**ビューの追加** ダイアログ ボックスが表示されます。 既定値は、をクリックする方法のまま、**追加**ボタンをクリックします。

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld*フォルダーと*MvcMovie\Views\HelloWorld\Index.cshtml*ファイルが作成されます。 わかります**ソリューション エクスプ ローラー**:

![](adding-a-view/_static/image3.png)

次に示す、 *Index.cshtml*が作成されたファイル。

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

HTML を追加、`<h2>`タグ。 変更された*MvcMovie\Views\HelloWorld\Index.cshtml*ファイルを次に示します。

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

アプリケーションを実行しを参照、`HelloWorld`コント ローラー (`http://localhost:xxxx/HelloWorld`)。 `Index`コント ローラーのメソッドは多くの作業を行わない; に単に、ステートメントを実行した`return View()`、指定されている、メソッドが応答をブラウザーにレンダリングするビュー テンプレート ファイルを使用する必要があります。 ASP.NET MVC の既定値を使用してに明示的に使用するビュー テンプレート ファイルの名前を指定していないので、 *Index.cshtml*でファイルの表示、 *\Views\HelloWorld*フォルダー。 次の図では、ビューにハード コードされた文字列を示します。

![](adding-a-view/_static/image6.png)

ように見えますね。 ただしに、ブラウザーのタイトル バーでは、"Index"が表示されます、そのページで大きなタイトルは"My MVC Application です"注意してください。 これらの名前を変更してみましょう。

## <a name="changing-views-and-layout-pages"></a>ビューとレイアウト ページを変更します。

最初に、ページの上部にある"My MVC Application"タイトルを変更します。 このテキストはすべてのページに共通です。 実際に実装は、プロジェクト内の 1 か所で場合でも、アプリケーションの各ページに表示されます。 移動して、 */ビュー/共有*フォルダー**ソリューション エクスプ ローラー**を開くと、  *\_Layout.cshtml*ファイル。 このファイルと呼ばれる、*レイアウト ページ*共有"シェルが"その他のすべてのページを使用するとします。

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

レイアウト テンプレートを使用すると、1 つの場所で、サイトの HTML コンテナー レイアウトを指定し、そのサイト内の複数のページに適用できます。 注、`@RenderBody()`ファイルの下部にある行。 `RenderBody` 場所を作成するすべてのビューに固有のページを表示、レイアウト ページで「ラップされた」プレース ホルダーです。 "MVC Movie App"を"My MVC Application"からレイアウト テンプレートのタイトル見出しを変更します。

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

アプリケーションを実行し、"MVC Movie App"今すぐ書かれていることに注意してください。 をクリックして、**について**リンク、およびするは、そのページがすぎます"MVC Movie App"を表示を参照してください。 レイアウト テンプレートで 1 回、変更を行うことができましたが、サイトのすべてのページで、新しいタイトルを反映します.

![](adding-a-view/_static/image9.png)

完全な *\_Layout.cshtml*ファイルを次に示します。

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

ここで、インデックス ページ (ビュー) のタイトルを変更してみましょう。

開いている*MvcMovie\Views\HelloWorld\Index.cshtml*します。 2 か所変更する場合: 最初に、テキスト表示される、ブラウザーのタイトルにおよび、セカンダリ ヘッダー (、`<h2>`要素)。 これを少し変えれば、コードのどの部分でアプリのどの部分が変更されるかを確認することができます。

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

セット上のコードを表示する HTML のタイトルを示すために、`Title`のプロパティ、`ViewBag`オブジェクト (これは、 *Index.cshtml*テンプレートの表示)。 レイアウト テンプレートのソース コードを振り返ることがわかりますテンプレートでは、この値が使用されて、`<title>`要素の一部として、 `<head>` HTML のセクション。 このアプローチを使用して、テンプレートの表示と、レイアウト ファイルの間の他のパラメーターを簡単に通過できます。

アプリケーションを実行しを参照する`http://localhost:xx/HelloWorld`します。 ブラウザーのタイトル、プライマリ見出し、およびセカンダリ見出しが変更されていることに注意してください  (ブラウザーに変更内容が表示されない場合は、キャッシュされたコンテンツを表示している可能性があります。 ブラウザーで Ctrl + F5 キーを押して、サーバーからの応答が強制的に読み込まれるようにしてください)。

また方法でコンテンツ、 *Index.cshtml*とテンプレートの表示がマージされた、  *\_Layout.cshtml*テンプレートの表示と 1 つの HTML 応答がブラウザーに送信されました。 レイアウト テンプレートを使用すれば、アプリケーションのすべてのページに適用される変更をとても簡単に行うことができます。

![](adding-a-view/_static/image10.png)

ここでは、"データ" のごく一部 (この場合は "Hello from our View Template!" というメッセージ) を ハード コーディングしました。 MVC アプリケーションには "V" (ビュー) があり、"C" (コントローラー) もありますが、"M" (モデル) はまだありません。 方法を説明します、データベースを作成し、モデル データを取得します。

## <a name="passing-data-from-the-controller-to-the-view"></a>コントローラーからビューへのデータの受け渡し

データベースに移動してモデルについて説明する前に、まずについて説明しましょうコント ローラーからビューに情報を渡します。 受信 URL 要求に応答には、コント ローラー クラスが呼び出されます。 コント ローラー クラスは、受信パラメーターを処理し、データベースからデータを取得、最終的には、ブラウザーに送信する応答の種類を決定するコードを記述します。 テンプレートの表示は、生成し、ブラウザーへの HTML 応答を書式設定するコント ローラーから使用できます。

コント ローラーはどのようなデータまたはオブジェクトが応答をブラウザーにレンダリングするテンプレートの表示のために必要なを提供する責任を負います。 ビュー テンプレートのビジネス ロジックを実行またはデータベースと直接やり取りする必要がありますしないでください。 代わりに、コント ローラーによって提供されるデータでのみ、動作する必要があります。 この「関心の分離」を維持すると、クリーンで保守しやすくなりますにコードを保持できます。

現時点では、`Welcome`内のアクション メソッド、`HelloWorldController`クラスは、`name`と`numTimes`パラメーターとし、ブラウザーに直接値を出力します。 この応答を文字列としてレンダリングするコント ローラーがあるのではなく、代わりにビュー テンプレートを使用するコント ローラーを変更してみましょう。 このビュー テンプレートでは動的応答が生成されます。これは、応答を生成するために、コントローラーからビューに適量のデータを渡す必要があることを意味します。 コント ローラーにビュー テンプレートに必要な動的なデータを格納することでこれを行う、`ViewBag`ビュー テンプレートでアクセスできるオブジェクト。

戻り、 *HelloWorldController.cs*ファイルし、変更、`Welcome`を追加するメソッド、`Message`と`NumTimes`値を`ViewBag`オブジェクト。 `ViewBag` 動的オブジェクト、つまりに自由を配置します。`ViewBag`オブジェクトには定義されているプロパティがない内部に、何かを設定するまでです。 完全な *HelloWorldController.cs* ファイルは次のようになります。

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

これで、`ViewBag`オブジェクトに自動的にビューに渡されるデータが含まれています。

次に、[ようこそ] ビュー テンプレートをする必要があります。 **デバッグ**メニューの **ビルド MvcMovie**プロジェクトがコンパイルされるかどうかを確認します。

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

内で右クリックし、`Welcome`メソッドとクリック**ビューの追加**します。 ここでは、**ビューの追加** ダイアログ ボックスのようになります。

![](adding-a-view/_static/image13.png)

クリックして**追加**、し、次のコードを追加、 `<h2>` 、新しい要素*Welcome.cshtml*ファイル。 「こんにちは」をユーザーが必要な数だけというループを作成します。 完全な*Welcome.cshtml*ファイルを次に示します。

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

アプリケーションを実行して、次の URL を参照してください。

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

今すぐデータでは、URL から取得され、コント ローラーに自動的に渡されます。 コント ローラーにデータをパッケージ化、`ViewBag`オブジェクトと、ビューにオブジェクトを渡します。 ビューはし、ユーザーに、HTML としてデータを表示します。

![](adding-a-view/_static/image14.png)

"M" (モデル) については学習しましたが、データベースについてはまだです。 学習したことを確認し、ムービーのデータベースを作成してみましょう。

> [!div class="step-by-step"]
> [前へ](adding-a-controller.md)
> [次へ](adding-a-model.md)
