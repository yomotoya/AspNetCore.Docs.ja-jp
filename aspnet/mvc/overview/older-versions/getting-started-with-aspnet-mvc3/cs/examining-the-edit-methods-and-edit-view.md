---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
title: Edit メソッドと Edit ビュー (c#) |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 1d266bf0-a61e-423b-a3d2-13773d7dafe2
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: ea4f940af7c3b598daad66542c6d07003cfe75b6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375805"
---
<a name="examining-the-edit-methods-and-edit-view-c"></a>Edit メソッドと Edit ビュー (c#)
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

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


このセクションでは、生成されたアクション メソッドとムービー コント ローラーのビューをについて説明します。 カスタム検索ページを追加します。

アプリケーションを実行しを参照、`Movies`コント ローラーを追加して */Movies*お使いのブラウザーのアドレス バーで URL にします。 マウス ポインターを置く、**編集**にリンクする URL を表示するリンク。

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image2.png)](examining-the-edit-methods-and-edit-view/_static/image1.png)

**編集**によって生成されたリンク、`Html.ActionLink`メソッドで、 *Views\Movies\Index.cshtml*ビュー。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image4.png)](examining-the-edit-methods-and-edit-view/_static/image3.png)

`Html`オブジェクトは、プロパティを使用して公開されるヘルパー、`WebViewPage`基本クラス。 `ActionLink`ヘルパー メソッドでは、簡単に動的にコント ローラーのアクション メソッドにリンクする HTML ハイパーリンクを生成できます。 最初の引数、`ActionLink`メソッドは、表示するために、リンク テキスト (たとえば、 `<a>Edit Me</a>`)。 2 番目の引数は、呼び出すアクション メソッドの名前です。 最後の引数、[匿名オブジェクト](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx)(この例では、4 の ID) では、ルート データを生成します。

前の図に示すように生成されたリンクは`http://localhost:xxxxx/Movies/Edit/4`します。 既定のルートは URL パターン`{controller}/{action}/{id}`します。 そのため、ASP.NET に変換`http://localhost:xxxxx/Movies/Edit/4`への要求に、`Edit`のアクション メソッド、`Movies`コント ローラー、パラメーターを持つ`ID`4 に等しい。

クエリ文字列を使用してアクション メソッドのパラメーターを渡すこともできます。 URL ではたとえば、`http://localhost:xxxxx/Movies/Edit?ID=4`もパラメーターを渡す`ID`4、`Edit`のアクション メソッド、`Movies`コント ローラー。

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image6.png)](examining-the-edit-methods-and-edit-view/_static/image5.png)

開く、`Movies`コント ローラー。 2 つ`Edit`アクション メソッドを以下に示します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

2 番目の `Edit` アクション メソッドの前に `HttpPost` 属性が付いていることに注意してください。 この属性は指定のオーバー ロード、`Edit`メソッドは、POST 要求に対してのみ呼び出すことができます。 適用することが、`HttpGet`最初の属性の編集メソッドが、既定値である必要はありません。 (暗黙的に割り当てられているアクション メソッドに言及します、`HttpGet`属性として`HttpGet`メソッドです)。

`HttpGet` `Edit`メソッドは映画 ID パラメーターを受け取り、Entity Framework を使用してムービーを検索`Find`メソッド、し、編集ビューを選択したムービーを返します。 スキャフォールディング システムが編集ビューを作成したときは、そのシステムが `Movie` クラスを調べて、クラスの各プロパティの `<label>` および `<input>` 要素をレンダリングするコードを作成しました。 次の例では、生成された編集ビューを示します。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

ビュー テンプレートにはどのように、`@model MvcMovie.Models.Movie`ファイルの上部にあるステートメント-これは、ビューがビュー テンプレートの型のモデルが予測しているを指定します。 `Movie`。

スキャフォールディングされたコードを使用して*ヘルパー メソッド*を HTML マークアップを効率化します。 [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx)ヘルパー ("Title"、"ReleaseDate"、"Genre"または"Price") のフィールドの名前を表示します。 [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx)ヘルパーは HTML を表示します。`<input>`要素。 [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx)ヘルパーには、そのプロパティに関連付けられた検証メッセージが表示されます。

アプリケーションを実行しに移動し、 */Movies* URL。 **[編集]** リンクをクリックします。 ブラウザーで、ページのソースを表示します。 ページの HTML は、次の例のようになります。 (メニュー マークアップは、わかりやすくするため除外されました)。

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample4.html)]

`<input>`要素は、HTML、`<form>`要素が`action`に投稿する属性を設定、 */ムービー/編集*URL。 フォーム データはサーバーにポストされるときに、**編集**ボタンがクリックされました。

## <a name="processing-the-post-request"></a>POST 要求の処理

次のリストでは、`Edit` アクション メソッドの `HttpPost` バージョンを示します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs)]

ASP.NET framework のモデル バインダーはポストされたフォーム値を取得および作成を`Movie`オブジェクトとして渡される、`movie`パラメーター。 `ModelState.IsValid`コードのチェックでは、変更する形式で送信されたデータを使用できることを検証、`Movie`オブジェクト。 コードがムービー データを保存して、データが有効な場合は、`Movies`のコレクション、`MovieDBContext`インスタンス。 コードし、新しいムービー データがデータベースに保存を呼び出して、`SaveChanges`メソッドの`MovieDBContext`データベースへの変更を永続化します。 データを保存した後、コードがユーザーをリダイレクト、`Index`のアクション メソッド、`MoviesController`クラスは、ムービーの一覧に表示される更新されたムービーをによりします。

ポストされた値が無効になると、それらが形式で再表示されます。 `Html.ValidationMessageFor`のヘルパー、 *Edit.cshtml*ビュー テンプレートを適切なエラー メッセージを表示する処理します。

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image8.png)](examining-the-edit-methods-and-edit-view/_static/image7.png)

> **ロケールに関する注意事項**通常の英語以外のロケールを使用する場合は、次を参照してください。[英語以外のロケールを使用した ASP.NET MVC 3 検証をサポートします。](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>Edit メソッドをより堅牢な作成

`HttpGet` `Edit`に渡される ID が有効であるスキャフォールディング システムによって生成されたメソッドをチェックしません。 ユーザーが URL から ID のセグメントを削除するかどうか (`http://localhost:xxxxx/Movies/Edit`)、次のエラーが表示されます。

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image10.png)](examining-the-edit-methods-and-edit-view/_static/image9.png)

ユーザーなど、データベースに存在しない ID を渡すこともできます`http://localhost:xxxxx/Movies/Edit/1234`します。 2 つの変更を行うことができます、 `HttpGet` `Edit`アクション メソッドは、この制限に対処します。 最初に、変更、`ID`パラメーター ID が渡されない場合は明示的にときに既定値は 0 です。 チェックすることも、`Find`メソッドがビュー テンプレートにムービー オブジェクトを返す前に、ムービー実際に見つかりませんでした。 更新された`Edit`メソッドを次に示します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

ムービーが見つからない場合、`HttpNotFound`メソッドが呼び出されます。

すべての`HttpGet`メソッドと同様のパターンに従います。 ムービー オブジェクトを取得する (またはの場合、オブジェクトの一覧`Index`)、ビューにモデルを渡すとします。 `Create`メソッドは、作成ビューに空のムービー オブジェクトを渡します。 データの作成、編集、削除、またはそれ以外の変更を行うすべてのメソッドは、メソッドの `HttpPost` のオーバーロードでそれを行います。 ブログの投稿」の説明に従って、セキュリティ リスクには、HTTP GET メソッドでデータを変更する[ASP.NET MVC ヒントと 46 – セキュリティ ホールを作成するため、削除のリンクを使用しない](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)します。 GET メソッドのデータの変更もに違反している HTTP のベスト プラクティスとアーキテクチャの REST のパターンは、GET 要求がアプリケーションの状態を変更する必要がありますを指定します。 つまり、GET 操作を実行する副作用がないセーフ操作である必要があります。

## <a name="adding-a-search-method-and-search-view"></a>Search メソッドとの検索ビューを追加します。

このセクションで追加します、`SearchIndex`できるようにするアクション メソッドがムービーをジャンルで名前を検索します。 使用して可用性になります、 */ムービー/SearchIndex* URL。 要求は、ムービーを検索するためにユーザーを入力する入力の要素を含む HTML フォームに表示されます。 ユーザーがフォームを送信するときにアクション メソッドは、ユーザーが投稿した検索の値を取得し、データベースを検索する値を使用します。

## <a name="displaying-the-searchindex-form"></a>SearchIndex フォームを表示します。

追加することで開始、`SearchIndex`を既存のアクション メソッド`MoviesController`クラス。 メソッドは HTML フォームを含むビューを返します。 次のコードに示します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cs)]

最初の行、`SearchIndex`メソッドは、次を作成します。 [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) 、ムービーを選択するクエリ。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cs)]

クエリでは、この時点では、定義されているが、データ ストアに対してまだ実行されていません。

場合、`searchString`パラメーターには文字列が含まれています、ムービー クエリが、次のコードを使用して、検索文字列の値にフィルターを変更します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

定義されているとき、またはなどのメソッドを呼び出すことによって変更されるときに LINQ クエリは実行されません`Where`または`OrderBy`します。 代わりに、クエリの実行は延期されます、つまり、その具体値が実際に反復されるまで、式の評価が遅れること、または[ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx)メソッドが呼び出されます。 `SearchIndex`サンプルを SearchIndex ビューでクエリを実行します。 クエリの遅延実行の詳細については、「[クエリの実行](https://msdn.microsoft.com/library/bb738633.aspx)」を参照してください。

実装することができますので、`SearchIndex`ユーザーに、フォームを表示するビュー。 内側を右クリックし、`SearchIndex`メソッドとクリック**ビューの追加**。 **ビューの追加** ダイアログ ボックスで、渡すしようとしていることを指定、`Movie`モデル クラスとしてテンプレートを表示するオブジェクト。 **スキャフォールディング テンプレート**一覧で、選択**一覧**、 をクリックし、**追加**します。

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

クリックすると、**追加** ボタン、 *Views\Movies\SearchIndex.cshtml*ビュー テンプレートを作成します。 選択したので、**一覧**で、**スキャフォールディング テンプレート**一覧で、Visual Web Developer が自動的に生成されます (スキャフォールディングされた) ビューで既定のコンテンツ。 スキャフォールディングは、HTML フォームを作成します。 検証が、`Movie`クラスおよび表示するために作成したコード`<label>`クラスの各プロパティの要素。 下のリストは、生成されたビューを作成するを示しています。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

アプリケーションを実行しに移動します */ムービー/SearchIndex*します。 `?searchString=ghost` などのクエリ文字列を URL に追加します。 フィルターされたムービーが表示されます。

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

シグネチャを変更する場合、`SearchIndex`メソッドという名前のパラメーターに`id`、`id`パラメーターが一致、`{id}`既定値のプレース ホルダーで一連のルーティング、 *Global.asax*ファイル。

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

変更された`SearchIndex`メソッドは次のようになります。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cs)]

これで、クエリ文字列の値ではなく、ルート データ (URL セグメント) として検索タイトルを渡すことができます。

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

ただし、ユーザーがムービーを検索するたびに URL の変更を求めることはできません。 これを追加しますに役立つ UI でムービーをフィルターしています。 シグネチャを変更した場合、`SearchIndex`ルート バインド ID パラメーターを渡す方法をテストする方法を変更することができるように、`SearchIndex`という名前の文字列パラメーターを受け取ります`searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample13.cs)]

開く、 *Views\Movies\SearchIndex.cshtml*ファイル、および後だけ`@Html.ActionLink("Create New", "Create")`以下を追加します。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cshtml)]

次の例の一部を示しています、 *Views\Movies\SearchIndex.cshtml*ファイルを追加したフィルターのマークアップ。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cshtml)]

`Html.BeginForm`ヘルパーの作成開始`<form>`タグ。 `Html.BeginForm`ヘルパーがユーザーをクリックして、フォームを送信すると、それ自体に投稿するためのフォーム、**フィルター**ボタンをクリックします。

アプリケーションを実行し、ムービーを検索してみてください。

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

ない`HttpPost`のオーバー ロード、`SearchIndex`メソッド。 不要になった、メソッドは、アプリケーションの状態を変更されていないため、データをフィルターするだけです。

以下の `HttpPost SearchIndex` メソッドを追加できます。 その場合は、アクション呼び出し元が一致、`HttpPost SearchIndex`メソッド、および`HttpPost SearchIndex`メソッドは、次の図に示すように実行が。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

ただし、この `HttpPost` バージョンの `SearchIndex` メソッドを追加しても、実装方法は制限されます。 たとえば、特定の検索をブックマークするか、友だちにリンクを送信し、友だちがそれをクリックしてムービーのフィルターされた同じリストを表示できるようにするとします。 HTTP POST 要求の URL は、GET 要求 (localhost:xxxxx/ムービー/SearchIndex) の URL と同じ--、URL 自体で検索情報がないことに注意してください。 右ここで、検索文字列の情報がサーバーに送信、フォーム フィールドの値として。 つまり、ブックマークまたは URL で友人に送信するには、その検索情報をキャプチャすることはできません。

ソリューションは、のオーバー ロードを使用する`BeginForm`URL に POST 要求が検索情報を追加する必要がありますをいる必要がありますを指定の HttpGet バージョンにルーティング、`SearchIndex`メソッド。 既存のパラメーターなし`BeginForm`を次のメソッド。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml)]

[![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image22.png)](examining-the-edit-methods-and-edit-view/_static/image21.png)

今すぐ検索を送信するときに、URL には検索のクエリ文字列が含まれます。 `HttpPost SearchIndex` メソッドがある場合でも、検索時には `HttpGet SearchIndex` アクション メソッドにも移動します。

[![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image24.png)](examining-the-edit-methods-and-edit-view/_static/image23.png)

## <a name="adding-search-by-genre"></a>ジャンルによる検索の追加

追加した場合、`HttpPost`のバージョン、`SearchIndex`メソッド、今すぐ削除しています。

次に、ユーザーがムービー ジャンルによる検索できるようにするための機能を追加します。 `SearchIndex` メソッドを次のコードで置き換えます。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cs)]

このバージョンの`SearchIndex`メソッドは、追加のパラメーターが namely`movieGenre`します。 最初の数行のコードを作成、`List`データベースからムービー ジャンルを保持するオブジェクト。

次のコードは、データベースからすべてのジャンルを取得する LINQ クエリです。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

コードを使用して、`AddRange`メソッドがジェネリックの`List`個々 のすべてのジャンルを一覧に追加するコレクション。 (なし、`Distinct`修飾子は、重複するジャンルが追加されます: たとえば、コメディ サンプルでは 2 回追加されます)。 コードでジャンルのリストを格納し、`ViewBag`オブジェクト。

次のコードを確認する方法を示しています、`movieGenre`パラメーター。 空でない場合、コードをさらには、指定のジャンルを選択したムービーを制限するムービーのクエリを制約します。

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>ジャンルによる検索をサポートするために SearchIndex ビューにマークアップを追加します。

追加、`Html.DropDownList`ヘルパーが、 *Views\Movies\SearchIndex.cshtml*直前に、ファイル、`TextBox`ヘルパー。 完成したマークアップは、以下に示します。

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cshtml)]

アプリケーションを実行しを参照する */ムービー/SearchIndex*します。 ジャンル、映画の名称、および両方の条件は、検索を実行してください。

このセクションでは、CRUD アクション メソッドと、フレームワークによって生成されるビューを調査します。 検索アクション メソッドとユーザーがムービーのタイトルとジャンルで検索できるようにするビューを作成したとします。 次のセクションでは、プロパティを追加する方法について説明します、`Movie`モデルおよびテスト データベースが自動的に作成するには初期化子を追加する方法。

> [!div class="step-by-step"]
> [前へ](accessing-your-models-data-from-a-controller.md)
> [次へ](adding-a-new-field.md)
