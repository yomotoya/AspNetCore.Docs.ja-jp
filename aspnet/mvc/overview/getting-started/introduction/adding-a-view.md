---
title: MVC アプリへのビューの追加
author: Rick-Anderson
description: MVC アプリへのビューの追加
ms.author: riande
ms.date: 09/1721/2017
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: 8b9ef79d630623019b22414ef730edffa5a83a09
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824578"
---
<a name="adding-a-view"></a>ビューの追加
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

このセクションではしようとしている変更、`HelloWorldController`ビュー テンプレート ファイルを明確には、クライアントへの HTML 応答を生成するプロセスをカプセル化を使用するクラス。 

使用してビュー テンプレート ファイルを作成します、 [Razor ビュー エンジン](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md)します。 Razor ベースのビュー テンプレートが、 *.cshtml*ファイル拡張子、および HTML 出力を c# を使用して作成する洗練された方法を提供します。 Razor の文字と、ビュー テンプレートの作成時に必要なキーストローク数を最小化し、ワークフローのコーディングの高速で、流体できます。

現在、`Index` メソッドは、コントローラー クラスでハード コーディングされるメッセージを含む文字列を返します。 変更、`Index`を返すメソッドを`View`オブジェクト、次のコードに示すようにします。

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

`Index`上記のメソッドでは、ビュー テンプレートを使用して、ブラウザーへの HTML 応答を生成します。 コント ローラー メソッド (とも呼ばれます[アクション メソッド](http://rachelappel.com/asp.net-mvc-actionresults-explained)) など、`Index`一般に、上記のメソッドが返す、 [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (から派生したクラスまたは[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx))、string と同様にないプリミティブ型。

右クリックして、 *Views\HelloWorld*フォルダーをクリックします**追加**、 をクリックし、**レイアウト (Razor) を使用する MVC 5 ビュー ページ**します。
  
![](adding-a-view/_static/image1.png)   
  
**項目の名前を指定** ダイアログ ボックスに、入力*インデックス*、順にクリックします**OK**します。  
  
![](adding-a-view/_static/image2.png)  
  
**レイアウト ページの選択**ダイアログ ボックスで、既定値をそのまま使用 **\_Layout.cshtml**  をクリック**OK**。  
  
![](adding-a-view/_static/image3.png)  
  
上記のダイアログ ボックスで、 *views \shared*左側のウィンドウでフォルダーを選択します。 カスタム レイアウト ファイルを別のフォルダーである場合は、選択する可能性があります。 チュートリアルの後半で、レイアウト ファイルについて説明します

*MvcMovie\Views\HelloWorld\Index.cshtml*ファイルが作成されます。

![](adding-a-view/_static/image4.png)

次の強調表示されたマークアップを追加します。

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

右クリックして、 *Index.cshtml*ファイルおよび選択**ブラウザーで表示**します。

![PI](adding-a-view/_static/image5.png)

右クリック、 *Index.cshtml*ファイルおよび選択**Page Inspector で表示します。** 参照してください、 [Page Inspector チュートリアル](../../views/using-page-inspector-in-aspnet-mvc.md)詳細についてはします。

また、アプリケーションを実行しを参照、`HelloWorld`コント ローラー (`http://localhost:xxxx/HelloWorld`)。 `Index`コント ローラーのメソッドは多くの作業を行わない; に単に、ステートメントを実行した`return View()`、指定されている、メソッドが応答をブラウザーにレンダリングするビュー テンプレート ファイルを使用する必要があります。 ASP.NET MVC の既定値を使用してに明示的に使用するビュー テンプレート ファイルの名前を指定していないので、 *Index.cshtml*でファイルの表示、 *\Views\HelloWorld*フォルダー。 次の図は、文字列&quot;こんにちは from our View Template!&quot;ビューにハードコーディングします。

![](adding-a-view/_static/image6.png)

ように見えますね。 ただし、ブラウザーのタイトル バーが表示されることを確認&quot;インデックス - マイ ASP.NET アプリケーション"、ページ上部にあるビッグ リンクという"Application name"です。 によってどのようにブラウザー ウィンドウを作成、表示する右上にある 3 本の棒をクリックする必要がありますに、**ホーム**、**について**、**連絡先**、 **登録**と**ログイン**リンク。

## <a name="changing-views-and-layout-pages"></a>ビューとレイアウト ページを変更します。

最初に、変更したい、&quot;アプリケーション名&quot;ページの上部にあるリンクです。 このテキストはすべてのページに共通です。 アプリケーション内の各ページに表示される場合でも、プロジェクト内の 1 か所で実際に実装されます。 移動して、 */ビュー/共有*フォルダー**ソリューション エクスプ ローラー**を開くと、  *\_Layout.cshtml*ファイル。 このファイルと呼ばれます、*レイアウト ページ*他のすべてのページを使用する共有フォルダー内にあるとします。

![_LayoutCshtml](adding-a-view/_static/image7.png)

レイアウト テンプレートを使用すると、1 つの場所で、サイトの HTML コンテナー レイアウトを指定し、そのサイト内の複数のページに適用できます。 `@RenderBody()` という行を見つけます。 `RenderBody` は、作成したビュー固有のページがすべて表示されるプレースホルダーで、レイアウト ページに&quot;ラップ&quot;されます。 たとえば、選択した場合、**について**リンクを*Views\Home\About.cshtml*内でビューが表示される、`RenderBody`メソッド。

タイトル要素の内容を変更します。 変更、 [ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx)からレイアウト テンプレート&quot;アプリケーション名&quot;に&quot;MVC ムービー&quot;とコント ローラーから`Home`に`Movies`します。 完全なレイアウト ファイルは、以下に示します。

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

アプリケーションと、次のようになりました注意実行&quot;MVC ムービー&quot;します。 をクリックして、**について**リンク、およびするは、そのページの表示方法を参照してください。 &quot;MVC ムービー&quot;もします。 レイアウト テンプレートで 1 回、変更を行うことができましたが、サイトのすべてのページで、新しいタイトルを反映します.

![](adding-a-view/_static/image8.png)

作成したときに、 *Views\HelloWorld\Index.cshtml*ファイルでは、次のコードが含まれています。

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

上記の Razor コードでは、レイアウト ページを明示的に設定です。 確認、*ビュー\\_ViewStart.cshtml*ファイル、正確な同じ Razor マークアップが含まれています。 *[ビュー\\_ViewStart.cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* ファイルは、すべてのビューを使用する一般的なレイアウトを定義、アウトまたは削除からそのコードをコメントするため、 *Views\HelloWorld\Index.cshtml*ファイル。

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

`Layout` プロパティを使用すれば、別のレイアウト ビューを設定することができます。また、`null` に設定して、レイアウト ファイルが使用されないようにすることができます。

ここで、インデックス ビューのタイトルを変更してみましょう。

開いている*MvcMovie\Views\HelloWorld\Index.cshtml*します。 2 か所変更する場合: 最初に、テキスト表示される、ブラウザーのタイトルにおよび、セカンダリ ヘッダー (、`<h2>`要素)。 これを少し変えれば、コードのどの部分でアプリのどの部分が変更されるかを確認することができます。

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

セット上のコードを表示する HTML のタイトルを示すために、`Title`のプロパティ、`ViewBag`オブジェクト (これは、 *Index.cshtml*テンプレートの表示)。 注意してレイアウト テンプレート ( *views \shared\\_Layout.cshtml* ) では、この値を使用して、`<title>`要素の一部として、`<head>`セクション html で以前に変更します。

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

これを使用して`ViewBag`アプローチでは、渡すことができます簡単に他のパラメーター、テンプレートの表示と、レイアウト ファイルの間。

アプリケーションを実行します。 ブラウザーのタイトル、プライマリ見出し、およびセカンダリ見出しが変更されていることに注意してください  (ブラウザーに変更内容が表示されない場合は、キャッシュされたコンテンツを表示している可能性があります。 ブラウザーで Ctrl + F5 キーを押して、サーバーからの応答が強制的に読み込まれるようにしてください)。ブラウザーのタイトルが作成された、`ViewBag.Title`設定します、 *Index.cshtml*テンプレートと追加表示&quot;-Movie App&quot;レイアウト ファイルに追加します。

また方法でコンテンツ、 *Index.cshtml*とテンプレートの表示がマージされた、  *\_Layout.cshtml*テンプレートの表示と 1 つの HTML 応答がブラウザーに送信されました。 レイアウト テンプレートを使用すれば、アプリケーションのすべてのページに適用される変更をとても簡単に行うことができます。

![](adding-a-view/_static/image9.png)

ごく一部&quot;データ&quot;(この場合、&quot;こんにちは from our View Template!&quot;メッセージ) はハード コーディング、ただしします。 MVC アプリケーションには、 &quot;V&quot; (ビュー) があると、 &quot;C&quot; (コント ローラー) が&quot;M&quot;まだ (モデル)。 まもなく、データベースを作成し、モデル データを取得する方法について説明します。

## <a name="passing-data-from-the-controller-to-the-view"></a>コントローラーからビューへのデータの受け渡し

データベースに移動してモデルについて説明する前に、まずについて説明しましょうコント ローラーからビューに情報を渡します。 受信 URL 要求に応答には、コント ローラー クラスが呼び出されます。 コント ローラー クラスは、受信ブラウザーを処理するコードは、要求、データベースからデータを取得し、最終的には、ブラウザーに送信する応答の種類を決定を記述します。 テンプレートの表示は、生成し、ブラウザーへの HTML 応答を書式設定するコント ローラーから使用できます。

コント ローラーはどのようなデータまたはオブジェクトが応答をブラウザーにレンダリングするテンプレートの表示のために必要なを提供する責任を負います。 ベスト プラクティス:**ビュー テンプレートのビジネス ロジックを実行またはデータベースと直接やり取りする必要がありますしない**します。 代わりに、ビュー テンプレートは、コント ローラーによって提供されるデータのみを操作する必要があります。 この保守&quot;懸念事項の分離&quot;も利用できるコード クリーン、テストが容易な保守しやすくなります。

現時点では、`Welcome`内のアクション メソッド、`HelloWorldController`クラスは、`name`と`numTimes`パラメーターとし、ブラウザーに直接値を出力します。 この応答を文字列としてレンダリングするコント ローラーがあるのではなく、代わりにビュー テンプレートを使用するコント ローラーを変更してみましょう。 このビュー テンプレートでは動的応答が生成されます。これは、応答を生成するために、コントローラーからビューに適量のデータを渡す必要があることを意味します。 コント ローラーにビュー テンプレートに必要な動的データ (パラメーター) を配置することでこれを行う、`ViewBag`ビュー テンプレートでアクセスできるオブジェクト。

戻り、 *HelloWorldController.cs*ファイルし、変更、`Welcome`を追加するメソッド、`Message`と`NumTimes`値を`ViewBag`オブジェクト。 `ViewBag` 動的オブジェクト、つまりに自由を配置します。`ViewBag`オブジェクトには定義されているプロパティがない内部に、何かを設定するまでです。 [ASP.NET MVC モデル バインド システム](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)名前付きパラメーターを自動的にマップされます (`name`と`numTimes`)、メソッドのパラメーターのアドレス バーで、クエリ文字列から。 完全な *HelloWorldController.cs* ファイルは次のようになります。

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

これで、`ViewBag`オブジェクトに自動的にビューに渡されるデータが含まれています。 次に、[ようこそ] ビュー テンプレートをする必要があります。 **ビルド**メニューの **ソリューションのビルド**(または Ctrl + Shift + B)、プロジェクトがコンパイルされるかどうかを確認します。 右クリックして、 *Views\HelloWorld*フォルダーをクリックします**追加**、 をクリックし、**レイアウト (Razor) を使用する MVC 5 ビュー ページ**します。
  
![](adding-a-view/_static/image10.png)   
  
**項目の名前を指定** ダイアログ ボックスに、入力*へようこそ*、順にクリックします**OK**します。   
  
**レイアウト ページの選択**ダイアログ ボックスで、既定値をそのまま使用 **\_Layout.cshtml**  をクリック**OK**。  
  
![](adding-a-view/_static/image11.png)   

*MvcMovie\Views\HelloWorld\Welcome.cshtml*ファイルが作成されます。

内のマークアップを置き換える、 *Welcome.cshtml*ファイル。 というループを作成します&quot;こんにちは&quot;回数だけ、ユーザーの質問にする必要があります。 完全な*Welcome.cshtml*ファイルを次に示します。

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

アプリケーションを実行して、次の URL を参照してください。

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

データは URL から取得され、コント ローラーを使用して、渡されるようになりました、[モデル バインダー](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)します。 コント ローラーにデータをパッケージ化、`ViewBag`オブジェクトと、ビューにオブジェクトを渡します。 ビューはし、ユーザーに、HTML としてデータを表示します。

![](adding-a-view/_static/image12.png)

上記のサンプルで使用して、`ViewBag`コント ローラーからビューにデータを渡すオブジェクト。 チュートリアルの後半で、ビュー モデルを使用して、コントローラーからビューにデータを渡します。 ビュー モデル アプローチは、データを渡すことは、ビュー バッグのアプローチで一般的に推奨です。 ブログ記事を参照してください。[動的 V 厳密に型指定されたビュー](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx)詳細についてはします。 

一種の&quot;M&quot;モデルがデータベースの種類ではありません。 学習したことを確認し、ムービーのデータベースを作成してみましょう。

> [!div class="step-by-step"]
> [前へ](adding-a-controller.md)
> [次へ](adding-a-model.md)
