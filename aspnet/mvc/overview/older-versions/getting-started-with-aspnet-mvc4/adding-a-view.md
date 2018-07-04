---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: ビューの追加 |Microsoft Docs
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンは ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全なはるかに簡単に従い、デモをお勧めしています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 0974cd2e06ed86c736214944a29a5a1eab8af50b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391510"
---
<a name="adding-a-view"></a>ビューの追加
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全ではるかに簡単に従うしより多くの機能を示します。


このセクションではしようとしている変更、`HelloWorldController`ビュー テンプレート ファイルを明確には、クライアントへの HTML 応答を生成するプロセスをカプセル化を使用するクラス。

使用してビュー テンプレート ファイルを作成します、 [Razor ビュー エンジン](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)ASP.NET MVC 3 で導入されました。 Razor ベースのビュー テンプレートが、 *.cshtml*ファイル拡張子、および HTML 出力を c# を使用して作成する洗練された方法を提供します。 Razor の文字と、ビュー テンプレートの作成時に必要なキーストローク数を最小化し、ワークフローのコーディングの高速で、流体できます。

現在、`Index` メソッドは、コントローラー クラスでハード コーディングされるメッセージを含む文字列を返します。 変更、`Index`を返すメソッドを`View`オブジェクト、次のコードに示すようにします。

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

`Index`上記のメソッドでは、ビュー テンプレートを使用して、ブラウザーへの HTML 応答を生成します。 コント ローラー メソッド (とも呼ばれます[アクション メソッド](http://rachelappel.com/asp.net-mvc-actionresults-explained)) など、`Index`一般に、上記のメソッドが返す、 [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (から派生したクラスまたは[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx))、string と同様にないプリミティブ型。

プロジェクトで、追加で使用できるテンプレートの表示、`Index`メソッド。 これを行うには、内を右クリックして、`Index`メソッドとクリック**ビューの追加**します。

![](adding-a-view/_static/image1.png)

**ビューの追加** ダイアログ ボックスが表示されます。 既定値は、をクリックする方法のまま、**追加**ボタンをクリックします。

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld*フォルダーと*MvcMovie\Views\HelloWorld\Index.cshtml*ファイルが作成されます。 わかります**ソリューション エクスプ ローラー**:

![](adding-a-view/_static/image3.png)

次に示す、 *Index.cshtml*が作成されたファイル。

![HelloWorldIndex](adding-a-view/_static/image4.png)

次の HTML を追加、`<h2>`タグ。

[!code-html[Main](adding-a-view/samples/sample2.html)]

完全な*MvcMovie\Views\HelloWorld\Index.cshtml*ファイルを次に示します。

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

ソリューション エクスプ ローラーで、Visual Studio 2012 を使用する場合を右クリックして、 *Index.cshtml*ファイルおよび選択**Page Inspector で表示**します。

![PI](adding-a-view/_static/image5.png)

[Page Inspector チュートリアル](../../views/using-page-inspector-in-aspnet-mvc.md)この新しいツールに関する詳細情報します。

また、アプリケーションを実行しを参照、`HelloWorld`コント ローラー (`http://localhost:xxxx/HelloWorld`)。 `Index`コント ローラーのメソッドは多くの作業を行わない; に単に、ステートメントを実行した`return View()`、指定されている、メソッドが応答をブラウザーにレンダリングするビュー テンプレート ファイルを使用する必要があります。 ASP.NET MVC の既定値を使用してに明示的に使用するビュー テンプレート ファイルの名前を指定していないので、 *Index.cshtml*でファイルの表示、 *\Views\HelloWorld*フォルダー。 次の図は、文字列&quot;こんにちは from our View Template!&quot;ビューにハードコーディングします。

![](adding-a-view/_static/image6.png)

ように見えますね。 ただし、ブラウザーのタイトル バーが表示されることを確認&quot;インデックス My ASP.NET A&quot; 、ページ上部にあるビッグ リンクという&quot;貴社のロゴです。&quot;以下、 &quot;、ロゴ。 ここで&quot;登録とログのリンクで、以下の家庭では、そのリンクを約は、Contact ページをリンクします。 これらの一部を変更してみましょう。

## <a name="changing-views-and-layout-pages"></a>ビューとレイアウト ページを変更します。

最初に、変更したい、 &quot;、ロゴ。 ここで&quot;ページの上部でタイトル。 このテキストはすべてのページに共通です。 アプリケーション内の各ページに表示される場合でも、プロジェクト内の 1 か所で実際に実装されます。 移動して、 */ビュー/共有*フォルダー**ソリューション エクスプ ローラー**を開くと、  *\_Layout.cshtml*ファイル。 このファイルと呼ばれます、*レイアウト ページ*、共有あり&quot;シェル&quot;他のすべてのページを使用します。

![_LayoutCshtml](adding-a-view/_static/image7.png)

レイアウト テンプレートを使用すると、1 つの場所で、サイトの HTML コンテナー レイアウトを指定し、そのサイト内の複数のページに適用できます。 `@RenderBody()` という行を見つけます。 `RenderBody` は、作成したビュー固有のページがすべて表示されるプレースホルダーで、レイアウト ページに&quot;ラップ&quot;されます。 バージョン情報のリンクを選択する場合など、 *Views\Home\About.cshtml*内でビューが表示される、`RenderBody`メソッド。

レイアウト テンプレートからのサイトのタイトル見出しの変更&quot;貴社のロゴ&quot;に&quot;MVC ムービー&quot;します。

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Title 要素の内容を次のマークアップに置き換えます。

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

アプリケーションと、次のようになりました注意実行&quot;MVC ムービー&quot;します。 をクリックして、**について**リンク、およびするは、そのページの表示方法を参照してください。 &quot;MVC ムービー&quot;もします。 レイアウト テンプレートで 1 回、変更を行うことができましたが、サイトのすべてのページで、新しいタイトルを反映します.

![](adding-a-view/_static/image8.png)

ここで、インデックス ビューのタイトルを変更してみましょう。

開いている*MvcMovie\Views\HelloWorld\Index.cshtml*します。 2 か所変更する場合: 最初に、テキスト表示される、ブラウザーのタイトルにおよび、セカンダリ ヘッダー (、`<h2>`要素)。 これを少し変えれば、コードのどの部分でアプリのどの部分が変更されるかを確認することができます。

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

セット上のコードを表示する HTML のタイトルを示すために、`Title`のプロパティ、`ViewBag`オブジェクト (これは、 *Index.cshtml*テンプレートの表示)。 レイアウト テンプレートのソース コードを振り返ることがわかりますテンプレートでは、この値が使用されて、`<title>`要素の一部として、`<head>`セクション html で以前に変更します。 これを使用して`ViewBag`アプローチでは、渡すことができます簡単に他のパラメーター、テンプレートの表示と、レイアウト ファイルの間。

アプリケーションを実行しを参照する`http://localhost:xx/HelloWorld`します。 ブラウザーのタイトル、プライマリ見出し、およびセカンダリ見出しが変更されていることに注意してください  (ブラウザーに変更内容が表示されない場合は、キャッシュされたコンテンツを表示している可能性があります。 ブラウザーで Ctrl + F5 キーを押して、サーバーからの応答が強制的に読み込まれるようにしてください)。ブラウザーのタイトルが作成された、`ViewBag.Title`設定します、 *Index.cshtml*テンプレートと追加表示&quot;-Movie App&quot;レイアウト ファイルに追加します。

また方法でコンテンツ、 *Index.cshtml*とテンプレートの表示がマージされた、  *\_Layout.cshtml*テンプレートの表示と 1 つの HTML 応答がブラウザーに送信されました。 レイアウト テンプレートを使用すれば、アプリケーションのすべてのページに適用される変更をとても簡単に行うことができます。

![](adding-a-view/_static/image9.png)

ごく一部&quot;データ&quot;(この場合、&quot;こんにちは from our View Template!&quot;メッセージ) はハード コーディング、ただしします。 MVC アプリケーションには、 &quot;V&quot; (ビュー) があると、 &quot;C&quot; (コント ローラー) が&quot;M&quot;まだ (モデル)。 方法を説明します、データベースを作成し、モデル データを取得します。

## <a name="passing-data-from-the-controller-to-the-view"></a>コントローラーからビューへのデータの受け渡し

データベースに移動してモデルについて説明する前に、まずについて説明しましょうコント ローラーからビューに情報を渡します。 受信 URL 要求に応答には、コント ローラー クラスが呼び出されます。 コント ローラー クラスは、受信ブラウザーを処理するコードは、要求、データベースからデータを取得し、最終的には、ブラウザーに送信する応答の種類を決定を記述します。 テンプレートの表示は、生成し、ブラウザーへの HTML 応答を書式設定するコント ローラーから使用できます。

コント ローラーはどのようなデータまたはオブジェクトが応答をブラウザーにレンダリングするテンプレートの表示のために必要なを提供する責任を負います。 ベスト プラクティス:**ビュー テンプレートのビジネス ロジックを実行またはデータベースと直接やり取りする必要がありますしない**します。 代わりに、ビュー テンプレートは、コント ローラーによって提供されるデータのみを操作する必要があります。 この保守&quot;懸念事項の分離&quot;も利用できるコード クリーン、テストが容易な保守しやすくなります。

現時点では、`Welcome`内のアクション メソッド、`HelloWorldController`クラスは、`name`と`numTimes`パラメーターとし、ブラウザーに直接値を出力します。 この応答を文字列としてレンダリングするコント ローラーがあるのではなく、代わりにビュー テンプレートを使用するコント ローラーを変更してみましょう。 このビュー テンプレートでは動的応答が生成されます。これは、応答を生成するために、コントローラーからビューに適量のデータを渡す必要があることを意味します。 コント ローラーにビュー テンプレートに必要な動的データ (パラメーター) を配置することでこれを行う、`ViewBag`ビュー テンプレートでアクセスできるオブジェクト。

戻り、 *HelloWorldController.cs*ファイルし、変更、`Welcome`を追加するメソッド、`Message`と`NumTimes`値を`ViewBag`オブジェクト。 `ViewBag` 動的オブジェクト、つまりに自由を配置します。`ViewBag`オブジェクトには定義されているプロパティがない内部に、何かを設定するまでです。 [ASP.NET MVC モデル バインド システム](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)名前付きパラメーターを自動的にマップされます (`name`と`numTimes`)、メソッドのパラメーターのアドレス バーで、クエリ文字列から。 完全な *HelloWorldController.cs* ファイルは次のようになります。

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

これで、`ViewBag`オブジェクトに自動的にビューに渡されるデータが含まれています。

次に、[ようこそ] ビュー テンプレートをする必要があります。 **ビルド**メニューの **ビルド MvcMovie**プロジェクトがコンパイルされるかどうかを確認します。

内で右クリックし、`Welcome`メソッドとクリック**ビューの追加**します。

![](adding-a-view/_static/image10.png)

ここでは、**ビューの追加** ダイアログ ボックスのようになります。

![](adding-a-view/_static/image11.png)

クリックして**追加**、し、次のコードを追加、 `<h2>` 、新しい要素*Welcome.cshtml*ファイル。 というループを作成します&quot;こんにちは&quot;回数だけ、ユーザーの質問にする必要があります。 完全な*Welcome.cshtml*ファイルを次に示します。

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

アプリケーションを実行して、次の URL を参照してください。

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

データは URL から取得され、コント ローラーを使用して、渡されるようになりました、[モデル バインダー](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)します。 コント ローラーにデータをパッケージ化、`ViewBag`オブジェクトと、ビューにオブジェクトを渡します。 ビューはし、ユーザーに、HTML としてデータを表示します。

![](adding-a-view/_static/image12.png)

上記のサンプルで使用して、`ViewBag`コント ローラーからビューにデータを渡すオブジェクト。 後者の場合、このチュートリアルでは、ビュー モデルをコント ローラーからビューにデータを渡すに使用します。 ビュー モデル アプローチは、データを渡すことは、ビュー バッグのアプローチで一般的に推奨です。 ブログ記事を参照してください。[動的 V 厳密に型指定されたビュー](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx)詳細についてはします。

一種の&quot;M&quot;モデルがデータベースの種類ではありません。 学習したことを確認し、ムービーのデータベースを作成してみましょう。

> [!div class="step-by-step"]
> [前へ](adding-a-controller.md)
> [次へ](adding-a-model.md)
