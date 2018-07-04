---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: Edit メソッドと編集ビューの確認 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 563da09adac93a0f6db4637a5884763ffb36e4fd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396748"
---
<a name="examining-the-edit-methods-and-edit-view"></a>Edit メソッドと編集ビューの確認
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

このセクションで、生成された調べます`Edit`アクション メソッドとムービー コント ローラーのビュー。 最初の見栄えをよくリリース日を短い楽しまかかります。 開く、 *Models\Movie.cs*ファイルし、次の強調表示された行を追加します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

行うことができますも日付カルチャ次のように特定します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

[DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) については、次のチュートリアルで説明します。 [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) 属性は、フィールドの名前として表示する内容 (ここでは、"ReleaseDate" ではなく、"Release Date") を指定します。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性データの種類を指定する、この例では、日付フィールドに格納される時刻情報が表示されないようにします。 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)日付形式が正しくレンダリングされる Chrome ブラウザーでのバグの属性が必要です。

アプリケーションを実行しを参照、`Movies`コント ローラー。 マウス ポインターを置く、**編集**にリンクする URL を表示するリンク。

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**編集**によって生成されたリンク、`Html.ActionLink`メソッドで、 *Views\Movies\Index.cshtml*ビュー。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html`オブジェクトは、プロパティを使用して公開されるヘルパー、 [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx)基本クラス。 `ActionLink`ヘルパー メソッドでは、簡単に動的にコント ローラーのアクション メソッドにリンクする HTML ハイパーリンクを生成できます。 最初の引数、`ActionLink`メソッドは、表示するために、リンク テキスト (たとえば、 `<a>Edit Me</a>`)。 2 番目の引数は、呼び出すアクション メソッドの名前 (ここで、`Edit`アクション)。 最後の引数、[匿名オブジェクト](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx)(この例では、4 の ID) では、ルート データを生成します。

前の図に示すように生成されたリンクは`http://localhost:1234/Movies/Edit/4`します。 既定のルート (で確立された*アプリ\_\routeconfig.cs*) URL パターンは、`{controller}/{action}/{id}`します。 そのため、ASP.NET に変換`http://localhost:1234/Movies/Edit/4`への要求に、`Edit`のアクション メソッド、`Movies`コント ローラー、パラメーターを持つ`ID`4 に等しい。 次のコードを調べて、*アプリ\_\routeconfig.cs*ファイル。 [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md)適切なコント ローラーとアクション メソッドに HTTP 要求をルーティングし、省略可能な ID パラメーターを指定するメソッドを使用します。 [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md)でメソッドを使用しても、 [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx)など`ActionLink`Url が指定されたコント ローラー、アクション メソッド、およびルート データを生成します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

クエリ文字列を使用してアクション メソッドのパラメーターを渡すこともできます。 URL ではたとえば、`http://localhost:1234/Movies/Edit?ID=3`もパラメーターを渡す`ID`3 の場合は、`Edit`のアクション メソッド、`Movies`コント ローラー。

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

開く、`Movies`コント ローラー。 2 つ`Edit`アクション メソッドを以下に示します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

2 番目の `Edit` アクション メソッドの前に `HttpPost` 属性が付いていることに注意してください。 この属性を指定するのオーバー ロード、`Edit`メソッドは、POST 要求に対してのみ呼び出すことができます。 適用することが、`HttpGet`最初の属性の編集メソッドが、既定値である必要はありません。 (暗黙的に割り当てられているアクション メソッドに言及します、`HttpGet`属性として`HttpGet`メソッドです)。[バインド](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx)属性は、モデルにデータがオーバーポスティング攻撃からハッカーを維持するもう 1 つの重要なセキュリティ メカニズム。 プロパティは、bind 属性は変更するにのみ含める必要があります。 過剰ポスティングおよびバインド属性について説明することができます、[セキュリティに関する注意を過剰ポスティング](https://go.microsoft.com/fwlink/?LinkId=317598)します。 このチュートリアルで使用される単純なモデルでは、バインド、モデル内のすべてのデータ。 [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx)属性を使用して要求の偽造を防止とペアにされます`@Html.AntiForgeryToken()`を編集ビュー ファイル (*Views\Movies\Edit.cshtml*)、一部を次に示します。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` 一致する必要があるフォームが非表示のフォージェリ対策トークンを生成、`Edit`のメソッド、`Movies`コント ローラー。 詳細については、サイト間で要求拙著のチュートリアルでフォージェリ (XSRF または CSRF) [MVC での XSRF/CSRF 防止](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)。

`HttpGet` `Edit`メソッドは映画 ID パラメーターを受け取り、Entity Framework を使用してムービーを検索`Find`メソッド、し、編集ビューを選択したムービーを返します。 ムービーが見つからない場合[HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx)が返されます。 スキャフォールディング システムが編集ビューを作成したときは、そのシステムが `Movie` クラスを調べて、クラスの各プロパティの `<label>` および `<input>` 要素をレンダリングするコードを作成しました。 次の例では、visual studio のスキャフォールディング システムによって生成された編集ビューを示します。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

ビュー テンプレートにはどのように、`@model MvcMovie.Models.Movie`ファイルの上部にあるステートメント-これは、ビューがビュー テンプレートの型のモデルが予測しているを指定します。 `Movie`。

スキャフォールディングされたコードを使用して*ヘルパー メソッド*を HTML マークアップを効率化します。 [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx)ヘルパーには、フィールドの名前が表示されます (&quot;タイトル&quot;、 &quot;ReleaseDate&quot;、&quot;ジャンル&quot;、または&quot;価格&quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx)ヘルパーは HTML をレンダリングします`<input>`要素。 [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx)ヘルパーには、そのプロパティに関連付けられた検証メッセージが表示されます。

アプリケーションを実行しに移動し、 */Movies* URL。 **[編集]** リンクをクリックします。 ブラウザーで、ページのソースを表示します。 フォーム要素の HTML は、以下に示します。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

`<input>`要素は、HTML、`<form>`要素が`action`に投稿する属性を設定、 */ムービー/編集*URL。 フォーム データはサーバーにポストされるときに、**保存**ボタンがクリックされました。 2 番目の行を示しています、非表示[XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)によって生成されたトークン、`@Html.AntiForgeryToken()`呼び出します。

## <a name="processing-the-post-request"></a>POST 要求の処理

次のリストでは、`Edit` アクション メソッドの `HttpPost` バージョンを示します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

[ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx)属性を検証、 [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md)によって生成されたトークン、`@Html.AntiForgeryToken()`ビューで呼び出します。

[ASP.NET MVC モデル バインダー](https://msdn.microsoft.com/library/dd410405.aspx)ポストされたフォーム値を受け取り、作成、`Movie`オブジェクトとして渡される、`movie`パラメーター。 `ModelState.IsValid` メソッドは、フォームで送信されたデータを使って `Movie` オブジェクトを変更 (編集または更新) できることを検証します。 ムービー データに保存して、データが有効な場合、`Movies`のコレクション、`db(MovieDBContext`インスタンス)。 新しいムービー データが呼び出すことによって、データベースに保存、`SaveChanges`メソッドの`MovieDBContext`します。 データを保存した後、コードはユーザーを `MoviesController` クラスの `Index` アクション メソッドにリダイレクトします。そこでは、行われたばかりの変更を含むムービー コレクションが表示されます。

クライアント側の検証は、フィールドの値が無効かを判断しますとすぐに、エラー メッセージが表示されます。 JavaScript を無効にした場合は、クライアント側の検証する必要はありませんが、サーバーを検出し、ポストされた値が有効でないフォームの値は、エラー メッセージと共に再表示されます。 チュートリアルの後半では、さらに詳しく検証を調べます。

`Html.ValidationMessageFor`のヘルパー、 *Edit.cshtml*ビュー テンプレートを適切なエラー メッセージを表示する処理します。

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

すべての`HttpGet`メソッドと同様のパターンに従います。 ムービー オブジェクトを取得する (またはの場合、オブジェクトの一覧`Index`)、ビューにモデルを渡すとします。 `Create`メソッドは、作成ビューに空のムービー オブジェクトを渡します。 データの作成、編集、削除、またはそれ以外の変更を行うすべてのメソッドは、メソッドの `HttpPost` のオーバーロードでそれを行います。 ブログの投稿」の説明に従って、セキュリティ リスクには、HTTP GET メソッドでデータを変更する[ASP.NET MVC ヒントと 46 – セキュリティ ホールを作成するため、削除のリンクを使用しない](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)します。 HTTP のベスト プラクティスと、アーキテクチャに違反 GET メソッドのデータの変更も[REST](http://en.wikipedia.org/wiki/Representational_State_Transfer)パターン, を指定する GET 要求には、アプリケーションの状態は変更しないでください。 つまり、GET 操作の実行は、副作用がなく、永続化されたデータを変更しない、安全な操作である必要があります。

英語 (米国) のコンピューターを使用している場合は、このセクションをスキップし、次のチュートリアルに移動します。 このチュートリアルの Globalize バージョンをダウンロードする[ここ](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475)します。 国際化に関する優れた 2 部構成チュートリアルを参照してください[Nadeem の ASP.NET MVC 5 の国際化](http://afana.me/post/aspnet-mvc-internationalization.aspx)します。


> [!NOTE]
> コンマを使用するロケールを英語以外の jQuery の検証をサポートするために (&quot;、&quot;) する必要があります、小数点と英語 (米国) 以外の日付形式は、 *globalize.js*と特定*cultures/globalize.cultures.js*ファイル (から[ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) および使用する JavaScript`Globalize.parseFloat`します。 JQuery の英語以外の検証は、NuGet から取得できます。 (インストールしないでください Globalize 英語ロケールを使用している場合。)


1. **ツール**ボタンをクリックし**NuGetLibrary パッケージ マネージャー**、 をクリックし、**ソリューションの NuGet パッケージの管理**します。  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. 左側のウィンドウで次のように選択します<strong>参照 *。</strong> 。*(下図を参照してください)。
3. 入力ボックスに、次のように入力します。 * Globalize * *。  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) 選択`jQuery.Validation.Globalize`、選択`MvcMovie`クリック**インストール**。 *Scripts\jquery.globalize\globalize.js*ファイルがプロジェクトに追加されます。 *Scripts\jquery.globalize\cultures\*フォルダーが多数のカルチャの JavaScript ファイルに格納されます。 このパッケージをインストールするために 5 分かかる可能性がありますに注意してください。

   次のコードは、Views\Movies\Edit.cshtml ファイルへの変更を示しています。 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

すべての編集ビューでは、このコードを繰り返しを避けるため、レイアウト ファイルを移動できます。 スクリプトのダウンロードを最適化するには、拙著のチュートリアルを参照してください。[バンドルと縮小](../../performance/bundling-and-minification.md)します。

詳細については、次を参照してください。 [ASP.NET MVC 3 Internationalization](http://afana.me/post/aspnet-mvc-internationalization.aspx)と[ASP.NET MVC 3 国際化 - パート 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx)します。

一時的な対策として英語 (米国) を使用するコンピューターを強制することができます、ロケールでの作業を検証できない場合、またはお使いのブラウザーで JavaScript を無効にすることができます。 英語 (米国) を使用するコンピューターを強制的には、プロジェクトのルートにグローバリゼーションの要素を追加できます*web.config*ファイル。 次のコードでは、United States English に設定する、カルチャにグローバリゼーションの要素を示しています。

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a> 次のチュートリアルでは、検索機能を実装します。

> [!div class="step-by-step"]
> [前へ](accessing-your-models-data-from-a-controller.md)
> [次へ](adding-search.md)
