---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: ビューの追加 |Microsoft ドキュメント
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンはここで ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全な非常に簡単に従い、デモをお勧めしています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 61a93c1430e9e39543c69b84901a50ceb710a5ae
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-view"></a>ビューの追加
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全な非常に簡単に従うしより多くの機能を示します。


このセクションでしようとしている変更、`HelloWorldController`クリーンにするテンプレート ファイルは、クライアントに HTML 応答を生成するプロセスをカプセル化するビューを使用するクラス。

テンプレート ファイルを使用してビューを作成、 [Razor ビュー エンジン](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)ASP.NET MVC 3 で導入されました。 Razor ベースのビューのテンプレートが、 *.cshtml*ファイル拡張子、および HTML を使用して C# の場合の出力を作成する簡潔な方法を提供します。 Razor 文字とビュー テンプレートの作成時に必要なキーの数を最小化され、ワークフローをコーディングする高速で、流体できます。

現在、`Index` メソッドは、コントローラー クラスでハード コーディングされるメッセージを含む文字列を返します。 変更、`Index`を返すメソッドを`View`オブジェクトを次のコードに示すようにします。

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

`Index`上記のメソッドでは、ビュー テンプレートを使用して、ブラウザーに HTML 応答を生成します。 コント ローラーのメソッド (とも呼ばれる[アクション メソッド](http://rachelappel.com/asp.net-mvc-actionresults-explained))、ように、`Index`一般に、上記のメソッドが返す、 [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (から派生したクラスまたは[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)) などの文字列のないプリミティブ型。

プロジェクトで、とともに使用できるビュー テンプレートの追加、`Index`メソッドです。 これを行うには、内部を右クリックして、`Index`メソッドとクリック**ビューの追加**です。

![](adding-a-view/_static/image1.png)

**ビューの追加** ダイアログ ボックスが表示されます。 既定値は、をクリックする方法のままにして、**追加**ボタン。

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld*フォルダーおよび*MvcMovie\Views\HelloWorld\Index.cshtml*ファイルが作成されます。 ご覧**ソリューション エクスプ ローラー**:

![](adding-a-view/_static/image3.png)

次に示す、 *Index.cshtml*作成されたファイル。

![HelloWorldIndex](adding-a-view/_static/image4.png)

次の HTML を追加、`<h2>`タグ。

[!code-html[Main](adding-a-view/samples/sample2.html)]

完全な*MvcMovie\Views\HelloWorld\Index.cshtml*ファイルを次に示します。

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

ソリューション エクスプ ローラーで、Visual Studio 2012 を使用している場合を右クリックして、 *Index.cshtml*ファイルおよび選択した**Page Inspector で表示**です。

![PI](adding-a-view/_static/image5.png)

[Page Inspector チュートリアル](../../views/using-page-inspector-in-aspnet-mvc.md)この新しいツールの詳細についてはします。

また、アプリケーションを実行しを参照、`HelloWorld`コント ローラー (`http://localhost:xxxx/HelloWorld`)。 `Index`コント ローラーでメソッドが多くの作業を実行していない以外の場合は、ステートメントを単純に実行された`return View()`、指定されている、メソッドが、ブラウザーへの応答を表示するためにテンプレート ファイルの表示を使用する必要があります。 ASP.NET MVC の既定値を使用してに明示的に使用するビューのテンプレート ファイルの名前を指定しませんでしたので、 *Index.cshtml*でファイルの表示、 *\Views\HelloWorld*フォルダーです。 次の図は、文字列を示しています。 &quot;、ビュー テンプレートから Hello!&quot;ビューで、ハードコーディングします。

![](adding-a-view/_static/image6.png)

なかなか良いを検索します。 ただし、ブラウザーのタイトル バーが表示されていることを確認&quot;My ASP.NET A をインデックス&quot;ページの上部の大規模なリンクと&quot;ここにロゴです。&quot;下、 &quot;、ロゴ。 ここで&quot;リンクの登録とリンクでと、自宅にリンクしている以下のログについては、ページにお問い合わせください。 これらのいくつかの名前を変更してみましょう。

## <a name="changing-views-and-layout-pages"></a>ビューとレイアウト ページを変更します。

最初に、変更する、 &quot;、ロゴ。 ここで&quot;、ページの上部にタイトルです。 テキストがすべてのページに共通します。 アプリケーション内の各ページに表示される場合でも、プロジェクト内の 1 か所で実際に実装されます。 移動して、 */ビュー/共有*フォルダー**ソリューション エクスプ ローラー**を開くと、  *\_Layout.cshtml*ファイル。 このファイルが呼び出され、*レイアウト ページ*であり、共有&quot;シェル&quot;他のすべてのページを使用します。

![_LayoutCshtml](adding-a-view/_static/image7.png)

レイアウト テンプレートを使用すると、1 か所で、コンテナーの HTML レイアウトは、サイトを指定し、サイト内の複数のページにわたって適用できます。 `@RenderBody()` という行を見つけます。 `RenderBody` は、作成したビュー固有のページがすべて表示されるプレースホルダーで、レイアウト ページに&quot;ラップ&quot;されます。 たとえば、バージョン情報のリンクを選択する場合、 *Views\Home\About.cshtml*内部ビューが表示される、`RenderBody`メソッドです。

レイアウト テンプレートからのサイトのタイトルの見出しを変更&quot;ここにロゴ&quot;に&quot;MVC ムービー&quot;です。

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Title 要素の内容を次のマークアップに置き換えます。

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

アプリケーションとに注意してください、今すぐ実行&quot;MVC ムービー&quot;です。 をクリックして、**に関する**リンク、およびするは、そのページに表示する方法を参照してください。 &quot;MVC ムービー&quot;、すぎます。 レイアウト テンプレートに 1 回、変更を行うことができましたが、サイトのすべてのページで、新しいタイトルを反映します.

![](adding-a-view/_static/image8.png)

ここで、インデックス ビューのタイトルを変更してみましょう。

Open *MvcMovie\Views\HelloWorld\Index.cshtml*. 2 つの場所を変更する場合がある: 最初に、テキスト表示される、ブラウザーのタイトルにし、セカンダリのヘッダー (、`<h2>`要素)。 これを少し変えれば、コードのどの部分でアプリのどの部分が変更されるかを確認することができます。

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

HTML 表示するタイトルをセットの上に、コードを示すために、`Title`のプロパティ、`ViewBag`オブジェクト (である、 *Index.cshtml*テンプレートの表示)。 戻るレイアウト テンプレートのソース コードを見ることがわかりますテンプレートがこの値を使用する、`<title>`要素の一部として、`<head>`以前に変更した HTML の「します。 これを使用して`ViewBag`アプローチでは、簡単に渡せる他のパラメーター、ビュー テンプレートと、レイアウト ファイルの間です。

アプリケーションを実行しを参照`http://localhost:xx/HelloWorld`です。 ブラウザーのタイトル、プライマリ見出し、およびセカンダリ見出しが変更されていることに注意してください  (ブラウザーに変更内容が表示されない場合は、キャッシュされたコンテンツを表示している可能性があります。 ブラウザーで Ctrl + F5 キーを押して、サーバーからの応答が強制的に読み込まれるようにしてください)。ブラウザーのタイトルが作成、`ViewBag.Title`設定、 *Index.cshtml*テンプレートおよびその他の表示&quot;-ムービー アプリ&quot;レイアウト ファイルに追加します。

また方法でコンテンツ、 *Index.cshtml*ビュー テンプレートのトピックとマージされました、  *\_Layout.cshtml*ビュー テンプレートおよび 1 つの HTML 応答をブラウザーに送信されました。 レイアウト テンプレートを使用すれば、アプリケーションのすべてのページに適用される変更をとても簡単に行うことができます。

![](adding-a-view/_static/image9.png)

ほんの少しの&quot;データ&quot;(ここでは、 &quot;、ビュー テンプレートからこんにちは!&quot;メッセージ) は、ハードコードです。 MVC アプリケーションには、 &quot;V&quot; (ビュー) を取得したら、 &quot;C&quot; (コント ローラー) が&quot;M&quot;まだ (モデル)。 間もなく、方法を説明するデータベースを作成し、モデル データを取得します。

## <a name="passing-data-from-the-controller-to-the-view"></a>コントローラーからビューへのデータの受け渡し

データベースに移動し、モデルについて説明する前に、しましょう。 まずコント ローラーからビューに情報を渡す処理です。 受信 URL 要求に応答には、コント ローラー クラスが呼び出されます。 コント ローラー クラスでは、受信ブラウザーを処理するコードは、要求をデータベースからデータを取得し、最終的には、ブラウザーに返送する応答の種類を決定を記述します。 テンプレートの表示は、生成し、ブラウザーに HTML 応答の書式を設定するコント ローラーから使用できます。

コント ローラーは、ブラウザーへの応答をレンダリングするビュー テンプレートの順に必要なすべてのデータまたはオブジェクトを提供します。 ベスト プラクティス:**ビュー テンプレートのビジネス ロジックを実行またはデータベースと直接やり取りする必要がありますしない**です。 代わりに、ビュー テンプレートは、コント ローラーに設定されているデータのみを操作する必要があります。 これを維持する&quot;関心の分離&quot;完全テスト可能でありより優れたコードに保ちます。

現時点では、`Welcome`でアクション メソッド、`HelloWorldController`クラスは、`name`と`numTimes`パラメーターと、ブラウザーに直接値を出力します。 この応答を文字列としてのレンダリング コント ローラーがあるのではなくビュー テンプレートを代わりに使用するコント ローラーに変更してみましょう。 このビュー テンプレートでは動的応答が生成されます。これは、応答を生成するために、コントローラーからビューに適量のデータを渡す必要があることを意味します。 コント ローラーにテンプレートの表示に必要な動的なデータ (パラメーター) を置くことによってこれを行う、`ViewBag`ビュー テンプレートにアクセスできるオブジェクト。

戻り、 *HelloWorldController.cs*ファイルし、変更、`Welcome`を追加するメソッド、`Message`と`NumTimes`値を`ViewBag`オブジェクト。 `ViewBag` 動的なオブジェクトは、つまりに自由を配置する;`ViewBag`オブジェクトを持たないプロパティに定義されたものと、その内部に配置するまでです。 [ASP.NET MVC モデル バインド システム](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)名前付きパラメーターが自動的にマップ (`name`と`numTimes`)、アドレス バーに、メソッドのパラメーターのクエリ文字列から。 完全な *HelloWorldController.cs* ファイルは次のようになります。

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

これで、`ViewBag`オブジェクトには、自動的にビューに渡されるデータが含まれています。

次に、開始ビュー テンプレートする必要があります。 **ビルド**メニューの **ビルド MvcMovie**プロジェクトをコンパイルするかどうかを確認します。

内で右クリックし、`Welcome`メソッドとクリック**ビューの追加**です。

![](adding-a-view/_static/image10.png)

ここでは、どのような**ビューの追加** ダイアログ ボックスはようになります。

![](adding-a-view/_static/image11.png)

をクリックして**追加**、下にある次のコードを追加し、 `<h2>` 、新しい要素*Welcome.cshtml*ファイル。 表示されているループを作成します&quot;こんにちは&quot;として何度でも、ユーザーの質問にする必要があります。 完全な*Welcome.cshtml*ファイルを次に示します。

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

アプリケーションを実行し、次の URL を参照します。

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

データが URL から取得され、コント ローラーを使用して、渡されるようになりました、[モデル バインダー](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)です。 コント ローラーへのデータをパッケージ化、`ViewBag`オブジェクトをビューにオブジェクトを渡します。 ビューは、データを HTML として、ユーザーにし、表示します。

![](adding-a-view/_static/image12.png)

上記のサンプルで使用して、`ViewBag`コント ローラーからビューにデータを渡すオブジェクト。 後者のチュートリアルでは、ビュー モデルをデータを渡すコント ローラーからビューを使用します。 モデルの表示方法でデータを渡すには、ビュー バッグ アプローチ経由で一般にはるかに優先です。 ブログ記事を参照してください[動的 V 厳密に型指定されたビュー](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx)詳細についてはします。

種類もの&quot;M&quot;モデルがデータベースの種類ではありません。 学習したことを確認し、ムービーのデータベースを作成してみましょう。

> [!div class="step-by-step"]
> [前へ](adding-a-controller.md)
> [次へ](adding-a-model.md)
