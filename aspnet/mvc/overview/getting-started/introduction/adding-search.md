---
uid: mvc/overview/getting-started/introduction/adding-search
title: 検索 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/22/2015
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 31fd35ac63f3eb31d824e1710833ad83a0852ac9
ms.sourcegitcommit: a91e8dd2f4b788114c8bc834507277f4b5e8d6c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2019
ms.locfileid: "55712264"
---
<a name="search"></a>検索
====================
によって[Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a>Search メソッドとの検索ビューを追加します。

このセクションでは、検索機能を追加します、`Index`できるようにするアクション メソッドがムービーをジャンルで名前を検索します。

## <a name="updating-the-index-form"></a>インデックスのフォームを更新しています

更新することで開始、`Index`を既存のアクション メソッド`MoviesController`クラス。 次のコードに示します。

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

最初の行、`Index`メソッドは、次を作成します。 [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) 、ムービーを選択するクエリ。

[!code-csharp[Main](adding-search/samples/sample2.cs)]

クエリでは、この時点では、定義されているが、まだデータベースに対して実行されていません。

場合、`searchString`パラメーターには文字列が含まれています、ムービー クエリが、次のコードを使用して、検索文字列の値にフィルターを変更します。

[!code-csharp[Main](adding-search/samples/sample3.cs)]

上の `s => s.Title` コードは[ラムダ式](https://msdn.microsoft.com/library/bb397687.aspx)です。 ラムダは、メソッド ベースで使用[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)などの標準クエリ演算子メソッドの引数としてクエリ、[場所](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx)上記のコードで使用されるメソッド。 定義されているとき、またはなどのメソッドを呼び出すことによって変更されるときに LINQ クエリは実行されません`Where`または`OrderBy`します。 代わりに、クエリの実行は延期されます、つまり、その具体値が実際に反復されるまで、式の評価が遅れること、または[ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx)メソッドが呼び出されます。 `Search`サンプルでは、クエリがで実行、 *Index.cshtml*ビュー。 クエリの遅延実行の詳細については、「[クエリの実行](https://msdn.microsoft.com/library/bb738633.aspx)」を参照してください。

> [!NOTE]
> [Contains](https://msdn.microsoft.com/library/bb155125.aspx)メソッドが、データベースでは、c# コードではなく上で実行します。 データベースに対して、 [Contains](https://msdn.microsoft.com/library/bb155125.aspx)にマップ[SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx)、これは大文字と小文字を区別しません。

更新することができますので、`Index`ユーザーに、フォームを表示するビュー。

アプリケーションを実行しに移動します */ムービー/インデックス*します。 `?searchString=ghost` などのクエリ文字列を URL に追加します。 フィルターされたムービーが表示されます。

![SearchQryStr](adding-search/_static/image1.png)

シグネチャを変更する場合、`Index`メソッドという名前のパラメーターに`id`、`id`パラメーターが一致、`{id}`既定値のプレース ホルダーで一連のルーティング、*アプリ\_作成できます。RouteConfig.cs*ファイル。

[!code-json[Main](adding-search/samples/sample4.json)]

元の`Index`次のようなメソッド。

[!code-csharp[Main](adding-search/samples/sample5.cs)]

変更された`Index`メソッドは次のようになります。

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

これで、クエリ文字列の値ではなく、ルート データ (URL セグメント) として検索タイトルを渡すことができます。

![](adding-search/_static/image2.png)

ただし、ユーザーがムービーを検索するたびに URL の変更を求めることはできません。 これを追加しますに役立つ UI でムービーをフィルターしています。 シグネチャを変更した場合、`Index`ルート バインド ID パラメーターを渡す方法をテストする方法を変更することができるように、`Index`という名前の文字列パラメーターを受け取ります`searchString`:

[!code-csharp[Main](adding-search/samples/sample7.cs)]

開く、 *Views\Movies\Index.cshtml*ファイル、および後だけ`@Html.ActionLink("Create New", "Create")`、以下に示す形式のマークアップを追加します。

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

`Html.BeginForm`ヘルパーの作成開始`<form>`タグ。 `Html.BeginForm`ヘルパーがユーザーをクリックして、フォームを送信すると、それ自体に投稿するためのフォーム、**フィルター**ボタンをクリックします。

Visual Studio 2013 では、表示およびファイルの表示を編集するときに便利な機能強化があります。 アプリケーションを実行するビュー ファイルを開き、Visual Studio 2013 は、ビューを表示する適切なコント ローラー アクション メソッドを呼び出します。

![](adding-search/_static/image3.png)

インデックス ビュー (上記の図で示す) のように、Visual Studio で開く、Ctr f5 キーまたは f5 キーを押してアプリケーションを実行してから、ムービーを検索 をタップします。

![](adding-search/_static/image4.png)

ない`HttpPost`のオーバー ロード、`Index`メソッド。 不要になった、メソッドは、アプリケーションの状態を変更されていないため、データをフィルターするだけです。

以下の `HttpPost Index` メソッドを追加できます。 その場合は、アクション呼び出し元が一致、`HttpPost Index`メソッド、および`HttpPost Index`メソッドは、次の図に示すように実行が。

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

ただし、この `HttpPost` バージョンの `Index` メソッドを追加しても、実装方法は制限されます。 たとえば、特定の検索をブックマークするか、友だちにリンクを送信し、友だちがそれをクリックしてムービーのフィルターされた同じリストを表示できるようにするとします。 HTTP POST 要求の URL は、GET 要求 (localhost:xxxxx/ムービー/インデックス) の URL と同じ--、URL 自体で検索情報がないことに注意してください。 右ここで、検索文字列の情報がサーバーに送信、フォーム フィールドの値として。 つまり、ブックマークまたは URL で友人に送信するには、その検索情報をキャプチャすることはできません。

ソリューションは、のオーバー ロードを使用する`BeginForm`POST 要求が URL に検索情報を追加する必要がありにルーティングする必要がありますを指定する、`HttpGet`のバージョン、`Index`メソッド。 既存のパラメーターのない`BeginForm`メソッドを次のマークアップ。

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

今すぐ検索を送信するときに、URL には検索のクエリ文字列が含まれます。 `HttpPost Index` メソッドがある場合でも、検索時には `HttpGet Index` アクション メソッドにも移動します。

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>ジャンルによる検索の追加

追加した場合、`HttpPost`のバージョン、`Index`メソッド、今すぐ削除しています。

次に、ユーザーがムービー ジャンルによる検索できるようにするための機能を追加します。 `Index` メソッドを次のコードで置き換えます。

[!code-csharp[Main](adding-search/samples/sample11.cs)]

このバージョンの`Index`メソッドは、追加のパラメーターが namely`movieGenre`します。 最初の数行のコードを作成、`List`データベースからムービー ジャンルを保持するオブジェクト。

次のコードは、データベースからすべてのジャンルを取得する LINQ クエリです。

[!code-csharp[Main](adding-search/samples/sample12.cs)]

コードを使用して、`AddRange`メソッドがジェネリックの`List`個々 のすべてのジャンルを一覧に追加するコレクション。 (なし、`Distinct`修飾子は、重複するジャンルが追加されます: たとえば、コメディ サンプルでは 2 回追加されます)。 コードでジャンルのリストを格納し、`ViewBag.MovieGenre`オブジェクト。 カテゴリ データ (このようなムービー ジャンルの) として格納する、 [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx)オブジェクト、 `ViewBag`、MVC アプリケーションの一般的なアプローチには、ドロップダウン ボックスでカテゴリ データにアクセスします。

次のコードを確認する方法を示しています、`movieGenre`パラメーター。 空ではない、コードをさらには、指定されたジャンルを選択したムービーを制限するムービーのクエリを制約します。

[!code-csharp[Main](adding-search/samples/sample13.cs)]

既に説明したように、クエリで実行していないデータベース ムービー リストが反復処理されるまで (後のビューで動作する、`Index`アクション メソッドを返します)。

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>ジャンルによる検索をサポートするために、インデックス ビューにマークアップを追加します。

追加、`Html.DropDownList`ヘルパーが、 *Views\Movies\Index.cshtml*直前に、ファイル、`TextBox`ヘルパー。 完成したマークアップは、以下に示します。

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

次のコード。

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

パラメーター"MovieGenre"キーを提供する、`DropDownList`を検索するためのヘルパーを`IEnumerable<SelectListItem>`で、`ViewBag`します。 `ViewBag`がアクション メソッドで設定されます。

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

オプション ラベルは、"All"パラメーター。 お使いのブラウザーでその選択を検査する場合、"value"属性が空であることがわかります。 コント ローラーのみがフィルターから`if`文字列ではありません`null`または空の値を送信する、空`movieGenre`すべてのジャンルを示しています。

既定で選択するオプションを設定することもできます。 コント ローラーのコードを変更すると、既定のオプションとして「コメディ」場合は、次のようにします。

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

アプリケーションを実行しを参照する */ムービー/インデックス*します。 ジャンル、映画の名称、および両方の条件は、検索を実行してください。

![](adding-search/_static/image8.png)

このセクションでは、検索アクション メソッドとユーザーがムービーのタイトルとジャンルで検索できるようにするビューを作成します。 次のセクションでは、プロパティを追加する方法について説明します、`Movie`モデルおよびテスト データベースが自動的に作成するには初期化子を追加する方法。

> [!div class="step-by-step"]
> [前へ](examining-the-edit-methods-and-edit-view.md)
> [次へ](adding-a-new-field.md)
