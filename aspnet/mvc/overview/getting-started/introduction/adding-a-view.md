---
title: MVC アプリケーションにビューの追加
author: Rick-Anderson
description: MVC アプリケーションにビューの追加
ms.author: riande
manager: wpickett
ms.date: 09/1721/2017
ms.topic: article
ms.technology: dotnet-mvc
ms.prod: .net-framework
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: 21db97e635b5db580df31f46ca7f8b60a80d6f94
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "30873431"
---
<a name="adding-a-view"></a>ビューの追加
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

このセクションでしようとしている変更、`HelloWorldController`クリーンにするテンプレート ファイルは、クライアントに HTML 応答を生成するプロセスをカプセル化するビューを使用するクラス。 

使用してビュー テンプレート ファイルを作成、 [Razor ビュー エンジン](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md)です。 Razor ベースのビューのテンプレートが、 *.cshtml*ファイル拡張子、および HTML を使用して C# の場合の出力を作成する簡潔な方法を提供します。 Razor 文字とビュー テンプレートの作成時に必要なキーの数を最小化され、ワークフローをコーディングする高速で、流体できます。

現在、`Index` メソッドは、コントローラー クラスでハード コーディングされるメッセージを含む文字列を返します。 変更、`Index`を返すメソッドを`View`オブジェクトを次のコードに示すようにします。

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

`Index`上記のメソッドでは、ビュー テンプレートを使用して、ブラウザーに HTML 応答を生成します。 コント ローラーのメソッド (とも呼ばれる[アクション メソッド](http://rachelappel.com/asp.net-mvc-actionresults-explained))、ように、`Index`一般に、上記のメソッドが返す、 [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (から派生したクラスまたは[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)) などの文字列のないプリミティブ型。

右クリックして、 *Views\HelloWorld*フォルダーをクリック**追加**をクリックし、 **MVC 5 ビュー ページ (Razor) のレイアウト**です。
  
![](adding-a-view/_static/image1.png)   
  
**項目の名前を指定** ダイアログ ボックスに、入力*インデックス*、順にクリック**OK**です。  
  
![](adding-a-view/_static/image2.png)  
  
**レイアウト ページの選択**ダイアログ ボックスで、既定値をそのまま使用 **\_Layout.cshtml**  をクリック**OK**です。  
  
![](adding-a-view/_static/image3.png)  
  
上記のダイアログ ボックスで、 *\shared*左側のウィンドウでフォルダーを選択します。 別のフォルダーで、カスタム レイアウト ファイルをした場合を選択する可能性があります。 レイアウト ファイルは、チュートリアルの後半で取り上げます

*MvcMovie\Views\HelloWorld\Index.cshtml*ファイルを作成します。

![](adding-a-view/_static/image4.png)

次の強調表示されているマークアップを追加します。

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

右クリックして、 *Index.cshtml*ファイルおよび選択した**ブラウザーで表示**です。

![PI](adding-a-view/_static/image5.png)

右クリック、 *Index.cshtml*ファイルおよび選択した**Page Inspector で表示します。** 参照してください、 [Page Inspector チュートリアル](../../views/using-page-inspector-in-aspnet-mvc.md)詳細についてはします。

また、アプリケーションを実行しを参照、`HelloWorld`コント ローラー (`http://localhost:xxxx/HelloWorld`)。 `Index`コント ローラーでメソッドが多くの作業を実行していない以外の場合は、ステートメントを単純に実行された`return View()`、指定されている、メソッドが、ブラウザーへの応答を表示するためにテンプレート ファイルの表示を使用する必要があります。 ASP.NET MVC の既定値を使用してに明示的に使用するビューのテンプレート ファイルの名前を指定しませんでしたので、 *Index.cshtml*でファイルの表示、 *\Views\HelloWorld*フォルダーです。 次の図は、文字列を示しています。 &quot;、ビュー テンプレートから Hello!&quot;ビューで、ハードコーディングします。

![](adding-a-view/_static/image6.png)

なかなか良いを検索します。 ただし、ブラウザーのタイトル バーが表示されていることを確認&quot;インデックス - マイ ASP.NET アプリケーション"とページの上部の大規模なリンクは、"Application name"を表示 によってどのように、ブラウザー ウィンドウを行うと、表示する右上の 3 つのバーをクリックする必要がありますに、**ホーム**、**に関する**、**連絡先**、 **登録**と**ログイン**リンクします。

## <a name="changing-views-and-layout-pages"></a>ビューとレイアウト ページを変更します。

最初に、変更する、&quot;アプリケーション名&quot;ページの上部にリンクします。 テキストがすべてのページに共通します。 アプリケーション内の各ページに表示される場合でも、プロジェクト内の 1 か所で実際に実装されます。 移動して、 */ビュー/共有*フォルダー**ソリューション エクスプ ローラー**を開くと、  *\_Layout.cshtml*ファイル。 このファイルが呼び出され、*レイアウト ページ*他のすべてのページを使用する共有フォルダー内にあるとします。

![_LayoutCshtml](adding-a-view/_static/image7.png)

レイアウト テンプレートを使用すると、1 か所で、コンテナーの HTML レイアウトは、サイトを指定し、サイト内の複数のページにわたって適用できます。 `@RenderBody()` という行を見つけます。 `RenderBody` は、作成したビュー固有のページがすべて表示されるプレースホルダーで、レイアウト ページに&quot;ラップ&quot;されます。 たとえば、選択した場合、**に関する**リンク、 *Views\Home\About.cshtml*内部ビューが表示される、`RenderBody`メソッドです。

タイトル要素の内容を変更します。 変更、 [ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx)からレイアウト テンプレートに&quot;アプリケーション名&quot;に&quot;MVC ムービー&quot;とコント ローラーから`Home`に`Movies`です。 完全なレイアウト ファイルは、次に示します。

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

アプリケーションとに注意してください、今すぐ実行&quot;MVC ムービー&quot;です。 をクリックして、**に関する**リンク、およびするは、そのページに表示する方法を参照してください。 &quot;MVC ムービー&quot;、すぎます。 レイアウト テンプレートに 1 回、変更を行うことができましたが、サイトのすべてのページで、新しいタイトルを反映します.

![](adding-a-view/_static/image8.png)

作成したときに、 *Views\HelloWorld\Index.cshtml*ファイルでは、次のコードが含まれています。

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

上記の Razor コードは、レイアウト ページを明示的に設定です。 確認、*ビュー\\は _viewstart.vbhtml*ファイル、正確な同じ Razor マークアップが含まれています。 *[ビュー\\は _viewstart.vbhtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* ファイルは、すべてのビューを使用する一般的なレイアウトを定義、ためアウトまたは削除からそのコードをコメント、 *Views\HelloWorld\Index.cshtml*ファイル。

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

`Layout` プロパティを使用すれば、別のレイアウト ビューを設定することができます。また、`null` に設定して、レイアウト ファイルが使用されないようにすることができます。

ここで、インデックス ビューのタイトルを変更してみましょう。

開いている*MvcMovie\Views\HelloWorld\Index.cshtml*です。 2 つの場所を変更する場合がある: 最初に、テキスト表示される、ブラウザーのタイトルにし、セカンダリのヘッダー (、`<h2>`要素)。 これを少し変えれば、コードのどの部分でアプリのどの部分が変更されるかを確認することができます。

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

HTML 表示するタイトルをセットの上に、コードを示すために、`Title`のプロパティ、`ViewBag`オブジェクト (である、 *Index.cshtml*テンプレートの表示)。 注意してレイアウト テンプレート ( *\shared\\_Layout.cshtml* ) の値を使用して、`<title>`要素の一部として、`<head>`以前に変更した HTML の「します。

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

これを使用して`ViewBag`アプローチでは、簡単に渡せる他のパラメーター、ビュー テンプレートと、レイアウト ファイルの間です。

アプリケーションを実行します。 ブラウザーのタイトル、プライマリ見出し、およびセカンダリ見出しが変更されていることに注意してください  (ブラウザーに変更内容が表示されない場合は、キャッシュされたコンテンツを表示している可能性があります。 ブラウザーで Ctrl + F5 キーを押して、サーバーからの応答が強制的に読み込まれるようにしてください)。ブラウザーのタイトルが作成、`ViewBag.Title`設定、 *Index.cshtml*テンプレートおよびその他の表示&quot;-ムービー アプリ&quot;レイアウト ファイルに追加します。

また方法でコンテンツ、 *Index.cshtml*ビュー テンプレートのトピックとマージされました、  *\_Layout.cshtml*ビュー テンプレートおよび 1 つの HTML 応答をブラウザーに送信されました。 レイアウト テンプレートを使用すれば、アプリケーションのすべてのページに適用される変更をとても簡単に行うことができます。

![](adding-a-view/_static/image9.png)

ほんの少しの&quot;データ&quot;(ここでは、 &quot;、ビュー テンプレートからこんにちは!&quot;メッセージ) は、ハードコードです。 MVC アプリケーションには、 &quot;V&quot; (ビュー) を取得したら、 &quot;C&quot; (コント ローラー) が&quot;M&quot;まだ (モデル)。 間もなく、データベースを作成し、モデル データを取得する方法を説明します。

## <a name="passing-data-from-the-controller-to-the-view"></a>コントローラーからビューへのデータの受け渡し

データベースに移動し、モデルについて説明する前に、しましょう。 まずコント ローラーからビューに情報を渡す処理です。 受信 URL 要求に応答には、コント ローラー クラスが呼び出されます。 コント ローラー クラスでは、受信ブラウザーを処理するコードは、要求をデータベースからデータを取得し、最終的には、ブラウザーに返送する応答の種類を決定を記述します。 テンプレートの表示は、生成し、ブラウザーに HTML 応答の書式を設定するコント ローラーから使用できます。

コント ローラーは、ブラウザーへの応答をレンダリングするビュー テンプレートの順に必要なすべてのデータまたはオブジェクトを提供します。 ベスト プラクティス:**ビュー テンプレートのビジネス ロジックを実行またはデータベースと直接やり取りする必要がありますしない**です。 代わりに、ビュー テンプレートは、コント ローラーに設定されているデータのみを操作する必要があります。 これを維持する&quot;関心の分離&quot;完全テスト可能でありより優れたコードに保ちます。

現時点では、`Welcome`でアクション メソッド、`HelloWorldController`クラスは、`name`と`numTimes`パラメーターと、ブラウザーに直接値を出力します。 この応答を文字列としてのレンダリング コント ローラーがあるのではなくビュー テンプレートを代わりに使用するコント ローラーに変更してみましょう。 このビュー テンプレートでは動的応答が生成されます。これは、応答を生成するために、コントローラーからビューに適量のデータを渡す必要があることを意味します。 コント ローラーにテンプレートの表示に必要な動的なデータ (パラメーター) を置くことによってこれを行う、`ViewBag`ビュー テンプレートにアクセスできるオブジェクト。

戻り、 *HelloWorldController.cs*ファイルし、変更、`Welcome`を追加するメソッド、`Message`と`NumTimes`値を`ViewBag`オブジェクト。 `ViewBag` 動的なオブジェクトは、つまりに自由を配置する;`ViewBag`オブジェクトを持たないプロパティに定義されたものと、その内部に配置するまでです。 [ASP.NET MVC モデル バインド システム](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)名前付きパラメーターが自動的にマップ (`name`と`numTimes`)、アドレス バーに、メソッドのパラメーターのクエリ文字列から。 完全な *HelloWorldController.cs* ファイルは次のようになります。

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

これで、`ViewBag`オブジェクトには、自動的にビューに渡されるデータが含まれています。 次に、開始ビュー テンプレートする必要があります。 **ビルド**メニューの **ソリューションのビルド**(または Ctrl + Shift + B)、プロジェクトをコンパイルするかどうかを確認します。 右クリックして、 *Views\HelloWorld*フォルダーをクリック**追加**をクリックし、 **MVC 5 ビュー ページ (Razor) のレイアウト**です。
  
![](adding-a-view/_static/image10.png)   
  
**項目の名前を指定** ダイアログ ボックスに、入力*へようこそ*、順にクリック**OK**です。   
  
**レイアウト ページの選択**ダイアログ ボックスで、既定値をそのまま使用 **\_Layout.cshtml**  をクリック**OK**です。  
  
![](adding-a-view/_static/image11.png)   

*MvcMovie\Views\HelloWorld\Welcome.cshtml*ファイルを作成します。

内のマークアップを置き換える、 *Welcome.cshtml*ファイル。 表示されているループを作成します&quot;こんにちは&quot;として何度でも、ユーザーの質問にする必要があります。 完全な*Welcome.cshtml*ファイルを次に示します。

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

アプリケーションを実行し、次の URL を参照します。

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

データが URL から取得され、コント ローラーを使用して、渡されるようになりました、[モデル バインダー](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)です。 コント ローラーへのデータをパッケージ化、`ViewBag`オブジェクトをビューにオブジェクトを渡します。 ビューは、データを HTML として、ユーザーにし、表示します。

![](adding-a-view/_static/image12.png)

上記のサンプルで使用して、`ViewBag`コント ローラーからビューにデータを渡すオブジェクト。 チュートリアルの後半で、ビュー モデルを使用して、コントローラーからビューにデータを渡します。 モデルの表示方法でデータを渡すには、ビュー バッグ アプローチ経由で一般にはるかに優先です。 ブログ記事を参照してください[動的 V 厳密に型指定されたビュー](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx)詳細についてはします。 

種類もの&quot;M&quot;モデルがデータベースの種類ではありません。 学習したことを確認し、ムービーのデータベースを作成してみましょう。

> [!div class="step-by-step"]
> [前へ](adding-a-controller.md)
> [次へ](adding-a-model.md)
