---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: "編集方法と編集ビューを調べて |Microsoft ドキュメント"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: d7e1ba503b8aa815cebf431d2f5ffc9436b3575b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="examining-the-edit-methods-and-edit-view"></a>編集方法と編集ビューの確認
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

このセクションでを確認する、生成された`Edit`アクション メソッドと、ムービーのコント ローラーのビューです。 外観をよくリリース日に短い迂回が最初になります。 開く、 *Models\Movie.cs*ファイルし、次の強調表示された行を追加します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

作成することも、日付カルチャ次のような特定。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

[DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) については、次のチュートリアルで説明します。 [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) 属性は、フィールドの名前として表示する内容 (ここでは、"ReleaseDate" ではなく、"Release Date") を指定します。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性データの種類を指定する、ここでは、日付フィールドに格納されている時刻情報が表示されないようにします。 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)日付の形式が正しくレンダリングされない Chrome ブラウザーでのバグの属性が必要です。

アプリケーションを実行しを参照、`Movies`コント ローラー。 上にマウス ポインターを置く、**編集**にリンクする URL を表示するリンクです。

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**編集**によって生成されたリンク、`Html.ActionLink`メソッドで、 *Views\Movies\Index.cshtml*ビュー。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html`オブジェクトのプロパティを使用して公開されているヘルパーは、 [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx)基本クラスです。 `ActionLink`ヘルパーのメソッドでは、簡単にコント ローラー アクション メソッドにリンクする HTML ハイパーリンクを動的に生成します。 1 番目の引数、`ActionLink`メソッドは、表示するには、このリンク テキスト (たとえば、 `<a>Edit Me</a>`)。 2 番目の引数は、呼び出すアクション メソッドの名前 (この場合、`Edit`アクション) です。 最後の引数、[匿名オブジェクト](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx)(ここでは、4 の ID) のルート データを生成します。

前の図に示すように生成されたリンクは`http://localhost:1234/Movies/Edit/4`します。 既定のルート (で確立された*アプリ\_Start\RouteConfig.cs*) URL パターンを受け取る`{controller}/{action}/{id}`です。 そのため、ASP.NET 変換`http://localhost:1234/Movies/Edit/4`への要求に、`Edit`のアクション メソッド、`Movies`パラメーターを使用してコント ローラー `ID` 4 に等しい。 次のコードを調べて、*アプリ\_Start\RouteConfig.cs*ファイル。 [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md)メソッドを使用して正しいコント ローラーとアクション メソッドへの HTTP 要求をルーティングし、省略可能な ID パラメーターを指定します。 [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md)でメソッドを使用しても、 [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx)など`ActionLink`コント ローラー、アクション メソッド、およびルート データを指定する Url を生成します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

クエリ文字列を使用してアクション メソッドのパラメーターを渡すこともできます。 たとえば、URL`http://localhost:1234/Movies/Edit?ID=3`もパラメーターを渡します`ID`3 の場合は、`Edit`のアクション メソッド、`Movies`コント ローラー。

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

開く、`Movies`コント ローラー。 2 つ`Edit`アクション メソッドを以下に示します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

2 番目の `Edit` アクション メソッドの前に `HttpPost` 属性が付いていることに注意してください。 この属性を指定するオーバー ロードの`Edit`メソッドは、POST 要求に対してのみ呼び出すことができます。 適用する可能性があります、`HttpGet`最初の属性がメソッドを編集がされていないに必要な既定値になっているためです。 (暗黙的に割り当てられているアクション メソッドに参照お、`HttpGet`属性に`HttpGet`メソッドです)。[バインド](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx)属性が過剰には、モデル データの送信からハッカーを保持する別の重要なセキュリティ メカニズム。 バインド属性を変更するには、プロパティのみを含める必要があります。 Overposting およびでバインド属性の読み取ることができます、[セキュリティに関する注意を過剰ポスティング](https://go.microsoft.com/fwlink/?LinkId=317598)です。 このチュートリアルで使用される単純なモデルでバインド、モデル内のすべてのデータ。 [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx)属性が要求の偽造を防ぐために使用されと対になっている`@Html.AntiForgeryToken()`を編集ビュー ファイル (*Views\Movies\Edit.cshtml*)、一部を次に示します。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()`一致する必要があります隠しフォームした偽造防止トークンを生成、`Edit`のメソッド、`Movies`コント ローラー。 詳細を読み取ることができますクロス サイトに関する要求の偽造防止 (XSRF または CSRF とも呼ばれます)、チュートリアルでは[mvc XSRF/CSRF 防止](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)です。

`HttpGet` `Edit`メソッド ムービー ID パラメーターを受け取り、Entity Framework を使用してムービーを検索`Find`メソッド、し、編集ビューを選択したムービーを返します。 ムービーが見つからない場合[HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx)が返されます。 スキャフォールディング システムが編集ビューを作成したときは、そのシステムが `Movie` クラスを調べて、クラスの各プロパティの `<label>` および `<input>` 要素をレンダリングするコードを作成しました。 次の例では、visual studio のスキャフォールディング システムによって生成された編集ビューを示します。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

ビュー テンプレートには、方法に注意してください、`@model MvcMovie.Models.Movie`ファイルの上部にあるステートメント — ビュー ビュー テンプレート型にするためのモデルが必要ですが、この指定`Movie`です。

スキャフォールディングのコードを使用して*ヘルパー メソッド*を HTML マークアップを効率化します。 [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx)ヘルパーには、フィールドの名前が表示されます (&quot;タイトル&quot;、 &quot;ReleaseDate&quot;、&quot;ジャンル&quot;、または&quot;価格&quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx)ヘルパーは、HTML をレンダリング`<input>`要素。 [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx)ヘルパーには、そのプロパティに関連付けられている検証メッセージが表示されます。

アプリケーションを実行しに移動し、 */Movies* URL。 **[編集]** リンクをクリックします。 ブラウザーで、ページのソースを表示します。 HTML フォーム要素を次に示します。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

`<input>`要素が HTML に`<form>`要素が`action`に投稿に属性が設定されている、 */ビデオ/編集*URL。 フォームのデータは、サーバーに書き込まれますときに、**保存**ボタンをクリックします。 2 番目の行が非表示を示しています[XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)によって生成されたトークン、`@Html.AntiForgeryToken()`呼び出します。

## <a name="processing-the-post-request"></a>POST 要求の処理

次のリストでは、`Edit` アクション メソッドの `HttpPost` バージョンを示します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

[ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx)属性を検証、 [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)によって生成されたトークン、`@Html.AntiForgeryToken()`ビューで呼び出します。

[ASP.NET MVC モデル バインダー](https://msdn.microsoft.com/library/dd410405.aspx)ポストされたフォーム値を取得し、作成、`Movie`オブジェクトとして渡される、`movie`パラメーター。 `ModelState.IsValid` メソッドは、フォームで送信されたデータを使って `Movie` オブジェクトを変更 (編集または更新) できることを検証します。 ムービー データに保存されているデータが有効である場合、`Movies`のコレクション、`db(MovieDBContext`インスタンス)。 新しいムービー データが呼び出すことによって、データベースに保存、`SaveChanges`メソッドの`MovieDBContext`します。 データを保存した後、コードはユーザーを `MoviesController` クラスの `Index` アクション メソッドにリダイレクトします。そこでは、行われたばかりの変更を含むムービー コレクションが表示されます。

クライアント側の検証で、フィールドの値が無効と認識するとすぐに、エラー メッセージが表示されます。 JavaScript を無効にする場合は、クライアント側の検証する必要はありませんが、ポストされた値が有効でないと、エラー メッセージにフォームの値が表示される、サーバーが検出されます。 このチュートリアルで後ほど詳しく検証を考察します。

`Html.ValidationMessageFor`のヘルパー、 *Edit.cshtml*ビュー テンプレートを該当するエラー メッセージを表示するように注意します。

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

すべての`HttpGet`メソッドと同様のパターンに従います。 ムービーのオブジェクトを取得した (またはの場合、オブジェクトの一覧`Index`)、ビューにモデルを渡すとします。 `Create`メソッドは、ビューを作成するに空のムービー オブジェクトを渡します。 データの作成、編集、削除、またはそれ以外の変更を行うすべてのメソッドは、メソッドの `HttpPost` のオーバーロードでそれを行います。 ブログの投稿「」の説明に従って、セキュリティ上のリスクは、HTTP GET メソッドでデータを変更する[ASP.NET MVC ヒント #46 – セキュリティ ホールを作成するため削除リンクを使用しない](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)です。 GET メソッドでデータを変更する、HTTP のベスト プラクティスと、アーキテクチャに違反するも[REST](http://en.wikipedia.org/wiki/Representational_State_Transfer)パターン、GET 要求では、アプリケーションの状態は変更する必要がありますを指定します。 つまり、GET 操作の実行は、副作用がなく、永続化されたデータを変更しない、安全な操作である必要があります。

英語 (米国) コンピューターを使用している場合は、このセクションをスキップし、次のチュートリアルを参照してください。 このチュートリアルの Globalize バージョンをダウンロードする[ここ](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475)です。 国際化に関する素晴らしい 2 部構成チュートリアルを参照してください。 [Nadeem の ASP.NET MVC 5 の国際化](http://afana.me/post/aspnet-mvc-internationalization.aspx)です。


> [!NOTE]
> コンマを使用するロケールを英語以外の jQuery 検証をサポートするために (&quot;、&quot;) する必要があります、小数点と日付の形式を英語 (米国) 以外の場合は、 *globalize.js*と特定の*cultures/globalize.cultures.js*ファイル (から[https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) および使用する JavaScript`Globalize.parseFloat`です。 NuGet からは、jQuery、英語以外の検証を取得できます。 (しないインストール Globalize 英語ロケールを使用している場合)。


1. **ツール**ボタンをクリックし**NuGetLibrary Package Manager**、クリックして**Manage NuGet Packages for Solution**です。  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. 左側のウィンドウで次のように選択します。 **参照*。 * * * (次の図を参照してください)。
3. 入力のボックスでは、次のように入力します。 * Globalize * *。  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image6.png)選択`jQuery.Validation.Globalize`、選択`MvcMovie` をクリック**インストール**です。 *Scripts\jquery.globalize\globalize.js*ファイルは、プロジェクトに追加されます。 *Scripts\jquery.globalize\cultures\*多くのカルチャの JavaScript ファイルが格納されます。 このパッケージのインストールに 5 分かかる場合がありますに注意してください。

 次のコードは、Views\Movies\Edit.cshtml ファイルへの変更を示しています。 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

すべての編集ビューでこのコードの繰り返しを避けるためには、レイアウト ファイルを移動できます。 スクリプトのダウンロードを最適化するには、自分のチュートリアルを参照してください。 [Bundling and Minification](../../performance/bundling-and-minification.md)です。

詳細については、次を参照してください。 [ASP.NET MVC 3 の国際化](http://afana.me/post/aspnet-mvc-internationalization.aspx)と[ASP.NET MVC 3 国際化 - パート 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx)です。

一時的な修正として英語 (米国) を使用するコンピューターを強制することができます、ロケールで作業して検証できない場合、またはお使いのブラウザーで JavaScript を無効にすることができます。 英語 (米国) を使用するコンピューターには、プロジェクトのルートにグローバリゼーションの要素を追加することができます*web.config*ファイル。 次のコードでは、United States English に設定する、カルチャにグローバリゼーションの要素を示しています。

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a>チュートリアルでは、[次へ] を検索機能を実装します。

>[!div class="step-by-step"]
[前へ](accessing-your-models-data-from-a-controller.md)
[次へ](adding-search.md)
