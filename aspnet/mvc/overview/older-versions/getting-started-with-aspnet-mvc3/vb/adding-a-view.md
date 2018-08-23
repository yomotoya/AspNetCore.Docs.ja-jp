---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: ビュー (VB) の追加 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: riande
ms.date: 01/12/2011
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: cbb1b9572c1b1eb671eb2756b5920dc823963e8c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835271"
---
<a name="adding-a-view-vb"></a>ビュー (VB) を追加します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 始める前に、以下の前提条件がインストールされていることを確認します。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。 または、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。
> 
> VB.NET のソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。 [VB.NET のバージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)します。 C# を使用する場合に切り替えて、 [c# バージョン](../cs/adding-a-view.md)このチュートリアルの。


このセクションでお見せ変更、`HelloWorldController`をクリーンにビュー テンプレート ファイルを使用するクラスがクライアントへの HTML 応答を生成するプロセスをカプセル化します。

ビュー テンプレートを使用して、まず、`Index`メソッドで、`HelloWorldController`クラス。 現在、`Index`メソッドがコント ローラー クラス内でハードコーディングされているメッセージに文字列を返します。 変更、`Index`を返すメソッドを`View`オブジェクトを次に示すようにします。

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

呼び出すことができるプロジェクトにビュー テンプレートを追加してみましょう今すぐ、`Index`メソッド。 これを行うには、内を右クリックして、`Index`メソッドとクリック**ビューの追加**します。

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

**ビューの追加** ダイアログ ボックスが表示されます。 既定のエントリのままにし、をクリックして、**追加**ボタンをクリックします。

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

*MvcMovie\Views\HelloWorld*フォルダーと*MvcMovie\Views\HelloWorld\Index.vbhtml*ファイルが作成されます。 わかります**ソリューション エクスプ ローラー**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

HTML を追加、`<h2>`タグ。 変更された*MvcMovie\Views\HelloWorld\Index.vbhtml*ファイルを次に示します。

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

アプリケーションを実行しを参照、 &quot;hello world&quot;コント ローラー (`http://localhost:xxxx/HelloWorld`)。 `Index`コント ローラーのメソッドは多くの作業を行わない; に単に、ステートメントを実行した`return View()`クライアントに応答をレンダリングするビュー テンプレート ファイルを使用したいことが示されます。 ASP.NET MVC の既定値を使用してに明示的に使用するビュー テンプレート ファイルの名前を指定しましたいない、ため、 *Index.vbhtml*内でファイルの表示、 *\Views\HelloWorld*フォルダー。 次の図では、ビューにハード コードされた文字列を示します。

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

ように見えますね。 ただし、ブラウザーのタイトル バーに表示されることを確認&quot;インデックス&quot;ビッグ、ページのタイトルと&quot;マイ MVC アプリケーションです。&quot;これらの名前を変更してみましょう。

## <a name="changing-views-and-layout-pages"></a>ビューとレイアウト ページの変更

まず、テキストを変更してみましょう&quot;マイ MVC アプリケーションです。&quot;そのテキストは、共有され、すべてのページに表示されます。 アプリケーション内の各ページ上にある場合でも実際に、プロジェクト内の 1 か所で表示します。 移動して、 */ビュー/共有*フォルダー**ソリューション エクスプ ローラー**を開くと、  *\_Layout.vbhtml*ファイル。 このファイルは、レイアウト ページと呼ばれ、共有は&quot;シェル&quot;他のすべてのページを使用します。

注、`@RenderBody()`のファイルの下部にあるコード行。 `RenderBody` プレース ホルダーを作成するすべてのページが表示するには&quot;ラップ&quot;レイアウト ページ。 変更、`<h1>`から見出し**&quot;** My MVC Application&quot;に&quot;MVC ムービー アプリ&quot;します。

[!code-html[Main](adding-a-view/samples/sample3.html)]

アプリケーションを実行し、伝えることに注意してください。 &quot;MVC ムービー アプリ&quot;します。 をクリックして、**について**リンク、およびページ表示&quot;MVC ムービー アプリ&quot;もします。

完全な *\_Layout.vbhtml*ファイルを次に示します。

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

ここで、インデックス ページ (ビュー) のタイトルを変更してみましょう。

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Open *MvcMovie\Views\HelloWorld\Index.vbhtml*. 2 か所変更する場合: 最初に、テキスト表示される、ブラウザーのタイトルにおよび、セカンダリ ヘッダー (、`<h2>`要素)。 しましょうに若干異なるため、どのビット コードの変更、アプリのどの部分を参照してください。

アプリケーションを実行しを参照する`http://localhost:xx/HelloWorld`します。 ブラウザーのタイトル、プライマリ見出し、およびセカンダリ見出しが変更されていることに注意してください  ビューに少し変更を加えて、アプリケーションで大きな変更を加えるには簡単です。 (ブラウザーに変更内容が表示されない場合は、キャッシュされたコンテンツを表示している可能性があります。 ブラウザーで Ctrl + F5 キーを押して、サーバーからの応答が強制的に読み込まれるようにしてください)。

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

ごく一部&quot;データ&quot;(この場合、 &quot;Hello World!&quot;メッセージ) はハード コーディング、ただしします。 MVC アプリケーションは V (ビュー) を備え、C (コント ローラー) がないの M (モデル) まだがあります。 方法を説明します、データベースを作成し、モデル データを取得します。

## <a name="passing-data-from-the-controller-to-the-view"></a>コントローラーからビューへのデータの受け渡し

データベースに移動してモデルについて説明する前に、まずについて説明しましょうコント ローラーからビューに情報を渡します。 クライアントへの HTML 応答を表示するためにビュー テンプレートで必要がありますを渡すたいのです。 これらのオブジェクトが通常作成され、コント ローラー クラスによって、ビュー テンプレートに渡され、テンプレートの表示が必要とするデータのみを含める必要がありますとは。

以前、`HelloWorldController`クラス、`Welcome`アクション メソッドが、`name`と`numTimes`パラメーターと、ブラウザーに、パラメーターの値からの出力。 代わりこの応答を直接表示するために引き続きコント ローラーがある、みましょう代わりを用意データ バッグに、ビューの。 コント ローラーとビューを使用できる、`ViewBag`そのデータを保持するオブジェクト。 ビュー テンプレートに自動的には、渡さし、するデータとして、バッグのコンテンツを使用して HTML 応答をレンダリングするために使用します。 これにより、コント ローラーでは 1 つと別のビュー テンプレート-クリーンに維持するためにできるようになりました&quot;懸念事項の分離&quot;アプリケーション内で。

またはカスタム クラスを定義、当社が単独でそのオブジェクトのインスタンスを作成とデータの挿入し、ビューに渡すことです。 カスタム ビューのモデルのため、ViewModel は、多くの場合、呼び出されます。 少量のデータは、ただし、ViewBag うまく動作します。

戻り、 *HelloWorldController.vb*ファイルの変更、 `Welcome` ViewBag に NumTimes とメッセージを格納するコント ローラーの内部メソッド。 ViewBag、動的オブジェクトです。 つまりに自由に配置することができます。 内部に、何かを設定するまで、ViewBag は定義されているプロパティがありません。

完全な`HelloWorldController.vb`同じファイル内の新しいクラスを使用します。

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

これで、ViewBag には、渡される経由でのビューに自動的にデータが含まれています。 ここでも、またはでしたが渡された次のように、独自のオブジェクト苦する場合。

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

必要があります、`WelcomeView`テンプレート。 新しいコードをコンパイルするためには、アプリケーションを実行します。 ブラウザーを閉じて、内を右クリックし、`Welcome`メソッド、およびクリック**ビューの追加**します。

ここでは、**ビューの追加** ダイアログ ボックスのようになります。

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

次のコードを追加、 `<h2>` 、新しい要素<em>ようこそ</em>。vbhtml ファイルです。 ループを作成し、たとえば&quot;こんにちは&quot;回数だけ、ユーザーがあります。

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

アプリケーションを実行しを参照 `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

今すぐデータでは、URL から取得され、コント ローラーに自動的に渡されます。 コント ローラーがパッケージにデータを`Model`オブジェクトと、ビューにオブジェクトを渡します。 ビューよりも、ユーザーに HTML としてデータが表示されます。

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

一種の&quot;M&quot;モデルがデータベースの種類ではありません。 学習したことを確認し、ムービーのデータベースを作成してみましょう。

> [!div class="step-by-step"]
> [前へ](adding-a-controller.md)
> [次へ](adding-a-model.md)
