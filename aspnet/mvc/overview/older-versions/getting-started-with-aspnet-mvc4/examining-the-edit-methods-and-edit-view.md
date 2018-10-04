---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
title: Edit メソッドと編集ビューの確認 |Microsoft Docs
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンは ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全なはるかに簡単に従い、デモをお勧めしています.'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 41eb99ca-e88f-4720-ae6d-49a958da8116
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 1223e110ce98c5b511312de42bc2992045a4a40b
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577991"
---
<a name="examining-the-edit-methods-and-edit-view"></a>Edit メソッドと編集ビューの確認
====================
によって[Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全ではるかに簡単に従うしより多くの機能を示します。


このセクションでは、生成されたアクション メソッドとムービー コント ローラーのビューをについて説明します。 カスタム検索ページを追加します。

アプリケーションを実行しを参照、`Movies`コント ローラーを追加して */Movies*お使いのブラウザーのアドレス バーで URL にします。 マウス ポインターを置く、**編集**にリンクする URL を表示するリンク。

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**編集**によって生成されたリンク、`Html.ActionLink`メソッドで、 *Views\Movies\Index.cshtml*ビュー。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html`オブジェクトは、プロパティを使用して公開されるヘルパー、 [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx)基本クラス。 `ActionLink`ヘルパー メソッドでは、簡単に動的にコント ローラーのアクション メソッドにリンクする HTML ハイパーリンクを生成できます。 最初の引数、`ActionLink`メソッドは、表示するために、リンク テキスト (たとえば、 `<a>Edit Me</a>`)。 2 番目の引数は、呼び出すアクション メソッドの名前です。 最後の引数、[匿名オブジェクト](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx)(この例では、4 の ID) では、ルート データを生成します。

前の図に示すように生成されたリンクは`http://localhost:xxxxx/Movies/Edit/4`します。 既定のルート (で確立された*アプリ\_\routeconfig.cs*) URL パターンは、`{controller}/{action}/{id}`します。 そのため、ASP.NET に変換`http://localhost:xxxxx/Movies/Edit/4`への要求に、`Edit`のアクション メソッド、`Movies`コント ローラー、パラメーターを持つ`ID`4 に等しい。 次のコードを調べて、*アプリ\_\routeconfig.cs*ファイル。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

クエリ文字列を使用してアクション メソッドのパラメーターを渡すこともできます。 URL ではたとえば、`http://localhost:xxxxx/Movies/Edit?ID=4`もパラメーターを渡す`ID`4、`Edit`のアクション メソッド、`Movies`コント ローラー。

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

開く、`Movies`コント ローラー。 2 つ`Edit`アクション メソッドを以下に示します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cs)]

2 番目の `Edit` アクション メソッドの前に `HttpPost` 属性が付いていることに注意してください。 この属性は指定のオーバー ロード、`Edit`メソッドは、POST 要求に対してのみ呼び出すことができます。 適用することが、`HttpGet`最初の属性の編集メソッドが、既定値である必要はありません。 (暗黙的に割り当てられているアクション メソッドに言及します、`HttpGet`属性として`HttpGet`メソッドです)。

`HttpGet` `Edit`メソッドは映画 ID パラメーターを受け取り、Entity Framework を使用してムービーを検索`Find`メソッド、し、編集ビューを選択したムービーを返します。 ID パラメーターを指定します、[既定値](https://msdn.microsoft.com/library/dd264739.aspx)の場合は 0、`Edit`パラメーターを指定せずにメソッドが呼び出されます。 ムービーが見つからない場合[HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx)が返されます。 スキャフォールディング システムが編集ビューを作成したときは、そのシステムが `Movie` クラスを調べて、クラスの各プロパティの `<label>` および `<input>` 要素をレンダリングするコードを作成しました。 次の例では、生成された編集ビューを示します。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cshtml)]

ビュー テンプレートにはどのように、`@model MvcMovie.Models.Movie`ファイルの上部にあるステートメント-これは、ビューがビュー テンプレートの型のモデルが予測しているを指定します。 `Movie`。

スキャフォールディングされたコードを使用して*ヘルパー メソッド*を HTML マークアップを効率化します。 [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx)ヘルパーには、フィールドの名前が表示されます (&quot;タイトル&quot;、 &quot;ReleaseDate&quot;、&quot;ジャンル&quot;、または&quot;価格&quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx)ヘルパーは HTML をレンダリングします`<input>`要素。 [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx)ヘルパーには、そのプロパティに関連付けられた検証メッセージが表示されます。

アプリケーションを実行しに移動し、 */Movies* URL。 **[編集]** リンクをクリックします。 ブラウザーで、ページのソースを表示します。 フォーム要素の HTML は、以下に示します。

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html?highlight=7,10-11)]

`<input>`要素は、HTML、`<form>`要素が`action`に投稿する属性を設定、 */ムービー/編集*URL。 フォーム データはサーバーにポストされるときに、**編集**ボタンがクリックされました。

## <a name="processing-the-post-request"></a>POST 要求の処理

次のリストでは、`Edit` アクション メソッドの `HttpPost` バージョンを示します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

[ASP.NET MVC モデル バインダー](https://msdn.microsoft.com/magazine/hh781022.aspx)ポストされたフォーム値を受け取り、作成、`Movie`オブジェクトとして渡される、`movie`パラメーター。 `ModelState.IsValid` メソッドは、フォームで送信されたデータを使って `Movie` オブジェクトを変更 (編集または更新) できることを検証します。 ムービー データに保存して、データが有効な場合、`Movies`のコレクション、`db(MovieDBContext`インスタンス)。 新しいムービー データが呼び出すことによって、データベースに保存、`SaveChanges`メソッドの`MovieDBContext`します。 データを保存した後、コードがユーザーをリダイレクト、`Index`のアクション メソッド、`MoviesController`クラスが表示される、行った変更を含め、ムービー コレクションの。

ポストされた値が無効になると、それらが形式で再表示されます。 `Html.ValidationMessageFor`のヘルパー、 *Edit.cshtml*ビュー テンプレートを適切なエラー メッセージを表示する処理します。

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

> [!NOTE]
> コンマを使用するロケールを英語以外の jQuery の検証をサポートするために (&quot;、&quot;) と小数点を含める必要があります*globalize.js*と特定*cultures/globalize.cultures.js*ファイル (から[ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) および使用する JavaScript`Globalize.parseFloat`します。 次のコードは操作する Views\Movies\Edit.cshtml ファイルへの変更、 &quot;FR-FR&quot;カルチャ。


[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

10 進数のフィールドには、コンマ、小数点ではありませんが必要です。 一時的な対策として、プロジェクトのルート web.config ファイルにグローバリゼーションの要素を追加できます。 次のコードでは、United States English に設定する、カルチャにグローバリゼーションの要素を示しています。

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.xml)]

すべての`HttpGet`メソッドと同様のパターンに従います。 ムービー オブジェクトを取得する (またはの場合、オブジェクトの一覧`Index`)、ビューにモデルを渡すとします。 `Create`メソッドは、作成ビューに空のムービー オブジェクトを渡します。 データの作成、編集、削除、またはそれ以外の変更を行うすべてのメソッドは、メソッドの `HttpPost` のオーバーロードでそれを行います。 ブログの投稿」の説明に従って、セキュリティ リスクには、HTTP GET メソッドでデータを変更する[ASP.NET MVC ヒントと 46 – セキュリティ ホールを作成するため、削除のリンクを使用しない](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)します。 HTTP のベスト プラクティスと、アーキテクチャに違反 GET メソッドのデータの変更も[REST](http://en.wikipedia.org/wiki/Representational_State_Transfer)パターン, を指定する GET 要求には、アプリケーションの状態は変更しないでください。 つまり、GET 操作の実行は、副作用がなく、永続化されたデータを変更しない、安全な操作である必要があります。

## <a name="adding-a-search-method-and-search-view"></a>Search メソッドとの検索ビューを追加します。

このセクションで追加します、`SearchIndex`できるようにするアクション メソッドがムービーをジャンルで名前を検索します。 使用して可用性になります、 */ムービー/SearchIndex* URL。 要求は、ユーザーがムービーを検索するために入力できる入力の要素を含む HTML フォームに表示されます。 ユーザーがフォームを送信するときにアクション メソッドは、ユーザーが投稿した検索の値を取得し、データベースを検索する値を使用します。

## <a name="displaying-the-searchindex-form"></a>SearchIndex フォームを表示します。

追加することで開始、`SearchIndex`を既存のアクション メソッド`MoviesController`クラス。 メソッドは HTML フォームを含むビューを返します。 次のコードに示します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

最初の行、`SearchIndex`メソッドは、次を作成します。 [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) 、ムービーを選択するクエリ。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cs)]

クエリでは、この時点では、定義されているが、データ ストアに対してまだ実行されていません。

場合、`searchString`パラメーターには文字列が含まれています、ムービー クエリが、次のコードを使用して、検索文字列の値にフィルターを変更します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample11.cs)]

上の `s => s.Title` コードは[ラムダ式](https://msdn.microsoft.com/library/bb397687.aspx)です。 ラムダは、メソッド ベースで使用[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)などの標準クエリ演算子メソッドの引数としてクエリ、[場所](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx)上記のコードで使用されるメソッド。 定義されているとき、またはなどのメソッドを呼び出すことによって変更されるときに LINQ クエリは実行されません`Where`または`OrderBy`します。 代わりに、クエリの実行は延期されます、つまり、その具体値が実際に反復されるまで、式の評価が遅れること、または[ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx)メソッドが呼び出されます。 `SearchIndex`サンプルを SearchIndex ビューでクエリを実行します。 クエリの遅延実行の詳細については、「[クエリの実行](https://msdn.microsoft.com/library/bb738633.aspx)」を参照してください。

実装することができますので、`SearchIndex`ユーザーに、フォームを表示するビュー。 内側を右クリックし、`SearchIndex`メソッドとクリック**ビューの追加**。 **ビューの追加** ダイアログ ボックスで、渡すしようとしていることを指定、`Movie`モデル クラスとしてテンプレートを表示するオブジェクト。 **スキャフォールディング テンプレート**一覧で、選択**一覧**、 をクリックし、**追加**します。

![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image5.png)

クリックすると、**追加** ボタン、 *Views\Movies\SearchIndex.cshtml*ビュー テンプレートを作成します。 選択したので、**一覧**で、**スキャフォールディング テンプレート**一覧で、Visual Studio で自動的に生成された (スキャフォールディングされた) ビューでの既定のマークアップ。 スキャフォールディングは、HTML フォームを作成します。 検証が、`Movie`クラスおよび表示するために作成したコード`<label>`クラスの各プロパティの要素。 下のリストは、生成されたビューを作成するを示しています。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cshtml)]

アプリケーションを実行しに移動します */ムービー/SearchIndex*します。 `?searchString=ghost` などのクエリ文字列を URL に追加します。 フィルターされたムービーが表示されます。

![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image6.png)

シグネチャを変更する場合、`SearchIndex`メソッドという名前のパラメーターに`id`、`id`パラメーターが一致、`{id}`既定値のプレース ホルダーで一連のルーティング、 *Global.asax*ファイル。

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample13.json)]

元の`SearchIndex`次のようなメソッド。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cs)]

変更された`SearchIndex`メソッドは次のようになります。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cs?highlight=1,3)]

これで、クエリ文字列の値ではなく、ルート データ (URL セグメント) として検索タイトルを渡すことができます。

![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image7.png)

ただし、ユーザーがムービーを検索するたびに URL の変更を求めることはできません。 これを追加しますに役立つ UI でムービーをフィルターしています。 シグネチャを変更した場合、`SearchIndex`ルート バインド ID パラメーターを渡す方法をテストする方法を変更することができるように、`SearchIndex`という名前の文字列パラメーターを受け取ります`searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

開く、 *Views\Movies\SearchIndex.cshtml*ファイル、および後だけ`@Html.ActionLink("Create New", "Create")`以下を追加します。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml?highlight=2)]

次の例の一部を示しています、 *Views\Movies\SearchIndex.cshtml*ファイルを追加したフィルターのマークアップ。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cshtml?highlight=12-15)]

`Html.BeginForm`ヘルパーの作成開始`<form>`タグ。 `Html.BeginForm`ヘルパーがユーザーをクリックして、フォームを送信すると、それ自体に投稿するためのフォーム、**フィルター**ボタンをクリックします。

アプリケーションを実行し、ムービーを検索してみてください。

![](examining-the-edit-methods-and-edit-view/_static/image8.png)

ない`HttpPost`のオーバー ロード、`SearchIndex`メソッド。 不要になった、メソッドは、アプリケーションの状態を変更されていないため、データをフィルターするだけです。

以下の `HttpPost SearchIndex` メソッドを追加できます。 その場合は、アクション呼び出し元が一致、`HttpPost SearchIndex`メソッド、および`HttpPost SearchIndex`メソッドは、次の図に示すように実行が。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image9.png)

ただし、この `HttpPost` バージョンの `SearchIndex` メソッドを追加しても、実装方法は制限されます。 たとえば、特定の検索をブックマークするか、友だちにリンクを送信し、友だちがそれをクリックしてムービーのフィルターされた同じリストを表示できるようにするとします。 HTTP POST 要求の URL は、GET 要求 (localhost:xxxxx/ムービー/SearchIndex) の URL と同じ--、URL 自体で検索情報がないことに注意してください。 右ここで、検索文字列の情報がサーバーに送信、フォーム フィールドの値として。 つまり、ブックマークまたは URL で友人に送信するには、その検索情報をキャプチャすることはできません。

ソリューションは、のオーバー ロードを使用する`BeginForm`POST 要求が URL に検索情報を追加する必要があり、HttpGet のバージョンにルーティングする必要がありますを指定する、`SearchIndex`メソッド。 既存のパラメーターなし`BeginForm`を次のメソッド。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cshtml)]

![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

今すぐ検索を送信するときに、URL には検索のクエリ文字列が含まれます。 `HttpPost SearchIndex` メソッドがある場合でも、検索時には `HttpGet SearchIndex` アクション メソッドにも移動します。

![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image11.png)

## <a name="adding-search-by-genre"></a>ジャンルによる検索の追加

追加した場合、`HttpPost`のバージョン、`SearchIndex`メソッド、今すぐ削除しています。

次に、ユーザーがムービー ジャンルによる検索できるようにするための機能を追加します。 `SearchIndex` メソッドを次のコードで置き換えます。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cs)]

このバージョンの`SearchIndex`メソッドは、追加のパラメーターが namely`movieGenre`します。 最初の数行のコードを作成、`List`データベースからムービー ジャンルを保持するオブジェクト。

次のコードは、データベースからすべてのジャンルを取得する LINQ クエリです。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample22.cs)]

コードを使用して、`AddRange`メソッドがジェネリックの`List`個々 のすべてのジャンルを一覧に追加するコレクション。 (なし、`Distinct`修飾子は、重複するジャンルが追加されます: たとえば、コメディ サンプルでは 2 回追加されます)。 コードでジャンルのリストを格納し、`ViewBag`オブジェクト。

次のコードを確認する方法を示しています、`movieGenre`パラメーター。 空ではない、コードをさらには、指定されたジャンルを選択したムービーを制限するムービーのクエリを制約します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample23.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>ジャンルによる検索をサポートするために SearchIndex ビューにマークアップを追加します。

追加、`Html.DropDownList`ヘルパーが、 *Views\Movies\SearchIndex.cshtml*直前に、ファイル、`TextBox`ヘルパー。 完成したマークアップは、以下に示します。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample24.cshtml?highlight=4)]

アプリケーションを実行しを参照する */ムービー/SearchIndex*します。 ジャンル、映画の名称、および両方の条件は、検索を実行してください。

![](examining-the-edit-methods-and-edit-view/_static/image12.png)

このセクションでは、CRUD アクション メソッドと、フレームワークによって生成されるビューを調査します。 検索アクション メソッドとユーザーがムービーのタイトルとジャンルで検索できるようにするビューを作成したとします。 次のセクションでは、プロパティを追加する方法について説明します、`Movie`モデルおよびテスト データベースが自動的に作成するには初期化子を追加する方法。

> [!div class="step-by-step"]
> [前へ](accessing-your-models-data-from-a-controller.md)
> [次へ](adding-a-new-field-to-the-movie-model-and-table.md)
