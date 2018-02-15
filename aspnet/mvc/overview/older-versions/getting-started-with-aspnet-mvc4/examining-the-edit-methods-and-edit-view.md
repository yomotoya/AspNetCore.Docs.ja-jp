---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
title: "編集方法と編集ビューを調べて |Microsoft ドキュメント"
author: Rick-Anderson
description: "注: このチュートリアルの最新バージョンはここで ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全な非常に簡単に従い、デモをお勧めしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 41eb99ca-e88f-4720-ae6d-49a958da8116
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 315914056c0a666fdf23cf82a314a999e03114b6
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2018
---
<a name="examining-the-edit-methods-and-edit-view"></a>編集方法と編集ビューの確認
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全な非常に簡単に従うしより多くの機能を示します。


このセクションで、生成されたアクション メソッドと、ムービーのコント ローラーのビューを確認します。 カスタムの検索ページを追加します。

アプリケーションを実行しを参照、`Movies`コント ローラーを追加して*/Movies*お使いのブラウザーのアドレス バーの URL にします。 上にマウス ポインターを置く、**編集**にリンクする URL を表示するリンクです。

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**編集**によって生成されたリンク、`Html.ActionLink`メソッドで、 *Views\Movies\Index.cshtml*ビュー。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html`オブジェクトのプロパティを使用して公開されているヘルパーは、 [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx)基本クラスです。 `ActionLink`ヘルパーのメソッドでは、簡単にコント ローラー アクション メソッドにリンクする HTML ハイパーリンクを動的に生成します。 1 番目の引数、`ActionLink`メソッドは、表示するには、このリンク テキスト (たとえば、 `<a>Edit Me</a>`)。 2 番目の引数は、呼び出すアクション メソッドの名前です。 最後の引数、[匿名オブジェクト](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx)(ここでは、4 の ID) のルート データを生成します。

前の図に示すように生成されたリンクは`http://localhost:xxxxx/Movies/Edit/4`します。 既定のルート (で確立された*アプリ\_Start\RouteConfig.cs*) URL パターンを受け取る`{controller}/{action}/{id}`です。 そのため、ASP.NET 変換`http://localhost:xxxxx/Movies/Edit/4`への要求に、`Edit`のアクション メソッド、`Movies`パラメーターを使用してコント ローラー `ID` 4 に等しい。 次のコードを調べて、*アプリ\_Start\RouteConfig.cs*ファイル。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

クエリ文字列を使用してアクション メソッドのパラメーターを渡すこともできます。 URL など、`http://localhost:xxxxx/Movies/Edit?ID=4`もパラメーターを渡す`ID`4、`Edit`のアクション メソッド、`Movies`コント ローラー。

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

開く、`Movies`コント ローラー。 2 つ`Edit`アクション メソッドを以下に示します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cs)]

2 番目の `Edit` アクション メソッドの前に `HttpPost` 属性が付いていることに注意してください。 この属性は指定のオーバー ロードする、`Edit`メソッドは、POST 要求に対してのみ呼び出すことができます。 適用する可能性があります、`HttpGet`最初の属性がメソッドを編集がされていないに必要な既定値になっているためです。 (暗黙的に割り当てられているアクション メソッドに参照お、`HttpGet`属性に`HttpGet`メソッドです)。

`HttpGet` `Edit`メソッド ムービー ID パラメーターを受け取り、Entity Framework を使用してムービーを検索`Find`メソッド、し、編集ビューを選択したムービーを返します。 ID パラメーターを指定します、[既定値](https://msdn.microsoft.com/library/dd264739.aspx)の場合は 0、`Edit`パラメーターを指定せずにメソッドが呼び出されます。 ムービーが見つからない場合[HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx)が返されます。 スキャフォールディング システムが編集ビューを作成したときは、そのシステムが `Movie` クラスを調べて、クラスの各プロパティの `<label>` および `<input>` 要素をレンダリングするコードを作成しました。 次の例では、生成された編集ビューを示します。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cshtml)]

ビュー テンプレートには、方法に注意してください、`@model MvcMovie.Models.Movie`ファイルの上部にあるステートメント — ビュー ビュー テンプレート型にするためのモデルが必要ですが、この指定`Movie`です。

スキャフォールディングのコードを使用して*ヘルパー メソッド*を HTML マークアップを効率化します。 [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx)ヘルパーには、フィールドの名前が表示されます (&quot;タイトル&quot;、 &quot;ReleaseDate&quot;、&quot;ジャンル&quot;、または&quot;価格&quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx)ヘルパーは、HTML をレンダリング`<input>`要素。 [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx)ヘルパーには、そのプロパティに関連付けられている検証メッセージが表示されます。

アプリケーションを実行しに移動し、 */Movies* URL。 **[編集]** リンクをクリックします。 ブラウザーで、ページのソースを表示します。 HTML フォーム要素を次に示します。

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html?highlight=7,10-11)]

`<input>`要素が HTML に`<form>`要素が`action`に投稿に属性が設定されている、 */ビデオ/編集*URL。 フォームのデータは、サーバーに書き込まれますときに、**編集**ボタンをクリックします。

## <a name="processing-the-post-request"></a>POST 要求の処理

次のリストでは、`Edit` アクション メソッドの `HttpPost` バージョンを示します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

[ASP.NET MVC モデル バインダー](https://msdn.microsoft.com/magazine/hh781022.aspx)ポストされたフォーム値を取得し、作成、`Movie`オブジェクトとして渡される、`movie`パラメーター。 `ModelState.IsValid` メソッドは、フォームで送信されたデータを使って `Movie` オブジェクトを変更 (編集または更新) できることを検証します。 ムービー データに保存されているデータが有効である場合、`Movies`のコレクション、`db(MovieDBContext`インスタンス)。 新しいムービー データが呼び出すことによって、データベースに保存、`SaveChanges`メソッドの`MovieDBContext`します。 データを保存した後、コードがユーザーをリダイレクト、`Index`のアクション メソッド、`MoviesController`クラスが表示される、先ほど行った変更も含め、ムービーのコレクション。

ポストされた値が有効でない場合がフォームで、再表示されます。 `Html.ValidationMessageFor`のヘルパー、 *Edit.cshtml*ビュー テンプレートを該当するエラー メッセージを表示するように注意します。

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

> [!NOTE]
> コンマを使用するロケールを英語以外の jQuery 検証をサポートするために (&quot;、&quot;) する必要があります、小数点の*globalize.js*と特定の*cultures/globalize.cultures.js*ファイル (から[https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) および使用する JavaScript`Globalize.parseFloat`です。 次のコードを操作する Views\Movies\Edit.cshtml ファイルへの変更を示しています、 &quot;FR-FR&quot;カルチャ。


[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

10 進数のフィールドには、コンマ、小数点ではない可能性がありますが必要です。 一時的な修正としては、プロジェクトのルートの web.config ファイルにグローバリゼーションの要素を追加できます。 次のコードでは、United States English に設定する、カルチャにグローバリゼーションの要素を示しています。

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.xml)]

すべての`HttpGet`メソッドと同様のパターンに従います。 ムービーのオブジェクトを取得した (またはの場合、オブジェクトの一覧`Index`)、ビューにモデルを渡すとします。 `Create`メソッドは、ビューを作成するに空のムービー オブジェクトを渡します。 データの作成、編集、削除、またはそれ以外の変更を行うすべてのメソッドは、メソッドの `HttpPost` のオーバーロードでそれを行います。 ブログの投稿「」の説明に従って、セキュリティ上のリスクは、HTTP GET メソッドでデータを変更する[ASP.NET MVC ヒント #46 – セキュリティ ホールを作成するため削除リンクを使用しない](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)です。 GET メソッドでデータを変更する、HTTP のベスト プラクティスと、アーキテクチャに違反するも[REST](http://en.wikipedia.org/wiki/Representational_State_Transfer)パターン、GET 要求では、アプリケーションの状態は変更する必要がありますを指定します。 つまり、GET 操作の実行は、副作用がなく、永続化されたデータを変更しない、安全な操作である必要があります。

## <a name="adding-a-search-method-and-search-view"></a>Search メソッドとの検索ビューの追加

このセクションでは追加、`SearchIndex`できるようにするアクション メソッドは、ジャンルまたは名前で映画を検索します。 使用可能なを使用してこれになります、 */ビデオ/SearchIndex* URL。 要求は、映画を検索するために、ユーザーが入力する入力要素を含む、HTML フォームに表示されます。 ユーザーがフォームを送信したときに、アクション メソッドは、ユーザーによって送信された検索値を取得し、データベースを検索する値を使用します。

## <a name="displaying-the-searchindex-form"></a>SearchIndex フォームを表示します。

追加することで開始、`SearchIndex`を既存のアクション メソッド`MoviesController`クラスです。 メソッドは HTML フォームを含むビューに戻ります。 コードを次に示します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

最初の行、`SearchIndex`メソッドは、次を作成[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)映画を選択するクエリ。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cs)]

クエリがこの時点では、定義されているが、まだデータ ストアに対して実行されていません。

場合、`searchString`パラメーター文字列に含まれる、映画クエリは、次のコードを使用して、検索文字列の値にフィルターを変更します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample11.cs)]

上の `s => s.Title` コードは[ラムダ式](https://msdn.microsoft.com/library/bb397687.aspx)です。 ラムダは、メソッド ベースで使用[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)など、標準クエリ演算子メソッドを引数としてクエリを実行する、[場所](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx)上記のコードで使用されるメソッド。 定義されている場合、またはなどのメソッドを呼び出すことで変更されるときに、LINQ クエリは実行されません`Where`または`OrderBy`です。 代わりに、クエリの実行が遅延、つまり、その実際の値が実際に反復処理されるまで、式の評価が遅延されるか、 [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx)メソッドが呼び出されます。 `SearchIndex`サンプル、SearchIndex ビューで、クエリを実行します。 クエリの遅延実行の詳細については、「[クエリの実行](https://msdn.microsoft.com/library/bb738633.aspx)」を参照してください。

これで、実装することができます、`SearchIndex`をユーザーに、フォームを表示するビュー。 内部を右クリックし、`SearchIndex`メソッドをクリックして**ビューの追加**です。 **ビューの追加** ダイアログ ボックスで、渡すしようとしていることを指定、`Movie`モデル クラスとしてテンプレートを表示するオブジェクト。 **Scaffold テンプレート**一覧で、選択**リスト**をクリックし、**追加**です。

![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image5.png)

クリックすると、**追加** ボタン、 *Views\Movies\SearchIndex.cshtml*ビュー テンプレートを作成します。 選択したので、**リスト**で、 **Scaffold テンプレート**一覧で、自動的に生成された Visual Studio (スキャフォールディングされた) ビューでの既定のいくつかのマークアップ。 スキャフォールディングでは、HTML フォームを作成します。 調べること、`Movie`クラスおよび表示するために作成したコード`<label>`クラスのプロパティごとの要素。 以下のリストは、生成されたビューを作成するを示しています。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cshtml)]

アプリケーションを実行しに移動*/ビデオ/SearchIndex*です。 `?searchString=ghost` などのクエリ文字列を URL に追加します。 フィルターされたムービーが表示されます。

![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image6.png)

署名を変更する場合、`SearchIndex`メソッドという名前のパラメーターに`id`、`id`パラメーターが一致、`{id}`既定値のプレース ホルダーのセットをルーティングする、 *Global.asax*ファイル。

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample13.json)]

元の`SearchIndex`メソッドは、次のようになります。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cs)]

変更された`SearchIndex`メソッドは次のようになります。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cs?highlight=1,3)]

これで、クエリ文字列の値ではなく、ルート データ (URL セグメント) として検索タイトルを渡すことができます。

![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image7.png)

ただし、ユーザーがムービーを検索するたびに URL の変更を求めることはできません。 する追加のヘルプを UI にはムービーにフィルターを適用します。 署名を変更した場合、`SearchIndex`ルート バインド ID パラメーターを渡す方法をテストする方法を変更することができるように、`SearchIndex`メソッドは、という名前の文字列パラメーターを受け取る`searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

開く、 *Views\Movies\SearchIndex.cshtml*ファイル、および直後`@Html.ActionLink("Create New", "Create")`次の追加。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml?highlight=2)]

次の例の一部を示しています、 *Views\Movies\SearchIndex.cshtml*マークアップを追加したフィルターを持つファイルです。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cshtml?highlight=12-15)]

`Html.BeginForm`ヘルパーの作成開始`<form>`タグ。 `Html.BeginForm`ヘルパーがユーザーをクリックしてフォームを送信するときにそれ自体に投稿する形式、**フィルター**ボタンをクリックします。

アプリケーションを実行し、映画を検索してください。

![](examining-the-edit-methods-and-edit-view/_static/image8.png)

ない`HttpPost`のオーバー ロード、`SearchIndex`メソッドです。 必要はありません、メソッドは、アプリケーションの状態を変更されていないためだけのデータをフィルター処理します。

以下の `HttpPost SearchIndex` メソッドを追加できます。 アクション呼び出し元が一致する場合は、`HttpPost SearchIndex`メソッド、および`HttpPost SearchIndex`の次の図に示すようにメソッドが実行されます。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image9.png)

ただし、この `HttpPost` バージョンの `SearchIndex` メソッドを追加しても、実装方法は制限されます。 たとえば、特定の検索をブックマークするか、友だちにリンクを送信し、友だちがそれをクリックしてムービーのフィルターされた同じリストを表示できるようにするとします。 HTTP POST 要求の URL は、要求の URL、GET (localhost:xxxxx/ビデオ/SearchIndex) と同じ--URL 自体に検索情報がないことに注意してください。 右ここで、検索文字列の情報がサーバーに送信、フォーム フィールドの値として。 つまり、ブックマークまたは URL で友人に送信するには、その検索情報をキャプチャすることはできません。

解決のオーバー ロードを使用するには`BeginForm`POST 要求が URL に検索情報を追加しての HttpGet バージョンにルーティングする必要がありますを指定する、`SearchIndex`メソッドです。 既存のパラメーターを置き換える`BeginForm`を次のメソッド。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cshtml)]

![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

今すぐ検索を送信するときに、URL には検索クエリ文字列が含まれます。 `HttpPost SearchIndex` メソッドがある場合でも、検索時には `HttpGet SearchIndex` アクション メソッドにも移動します。

![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image11.png)

## <a name="adding-search-by-genre"></a>ジャンルで検索を追加します。

追加した場合、`HttpPost`のバージョン、`SearchIndex`メソッドを削除します。

次に、ジャンルで映画を検索できるようにするための機能を追加します。 `SearchIndex` メソッドを次のコードで置き換えます。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cs)]

このバージョンの`SearchIndex`メソッドには、追加のパラメーター、つまり`movieGenre`です。 最初の数行のコードを作成、`List`データベースからムービー ジャンルを保持するオブジェクト。

次のコードは、データベースからすべてのジャンルを取得する LINQ クエリです。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample22.cs)]

コードを使用して、`AddRange`ジェネリックのメソッド`List`一覧にすべての個別ジャンルを追加するコレクション。 (なし、`Distinct`修飾子は、重複するジャンルを追加するには — たとえば、コメディ サンプルに 2 回追加されます)。 コードでジャンルの一覧を格納し、`ViewBag`オブジェクト。

次のコードを確認する方法を示しています、`movieGenre`パラメーター。 空ではない、コードをさらには指定されたジャンルを選択したムービーを制限するムービーのクエリを制約します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample23.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>ジャンルで検索をサポートするために SearchIndex ビューにマークアップを追加します。

追加、`Html.DropDownList`ヘルパーが、 *Views\Movies\SearchIndex.cshtml*ファイル、直前に、`TextBox`ヘルパー。 完全なマークアップを次に示します。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample24.cshtml?highlight=4)]

アプリケーションを実行しを参照*/ビデオ/SearchIndex*です。 ジャンル、ムービーの名前、および両方の条件は、検索を再試行してください。

![](examining-the-edit-methods-and-edit-view/_static/image12.png)

このセクションでは、CRUD アクション メソッドと、フレームワークによって生成されたビューを調査します。 アクション メソッドの検索とムービーのタイトル、ジャンルで検索できるようにするビューを作成したとします。 次のセクションでプロパティを追加する方法について見ていきます、`Movie`モデルおよびテスト用データベースが自動的に作成するには初期化子を追加する方法です。

>[!div class="step-by-step"]
[前へ](accessing-your-models-data-from-a-controller.md)
[次へ](adding-a-new-field-to-the-movie-model-and-table.md)
