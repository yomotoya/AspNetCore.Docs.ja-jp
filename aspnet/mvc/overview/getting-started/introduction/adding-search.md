---
uid: mvc/overview/getting-started/introduction/adding-search
title: "検索 |Microsoft ドキュメント"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 116f681e14af0a09a4eb1502ef9f057c5db2f97d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="search"></a>検索
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a>Search メソッドとの検索ビューの追加

このセクションの内容に検索機能を追加、`Index`できるようにするアクション メソッドは、ジャンルまたは名前で映画を検索します。

## <a name="updating-the-index-form"></a>インデックスのフォームの更新

更新することで開始、`Index`を既存のアクション メソッド`MoviesController`クラスです。 コードを次に示します。

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

最初の行、`Index`メソッドは、次を作成[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)映画を選択するクエリ。

[!code-csharp[Main](adding-search/samples/sample2.cs)]

クエリがこの時点では、定義されているが、まだデータベースに対して実行されていません。

場合、`searchString`パラメーター文字列に含まれる、映画クエリは、次のコードを使用して、検索文字列の値にフィルターを変更します。

[!code-csharp[Main](adding-search/samples/sample3.cs)]

上の `s => s.Title` コードは[ラムダ式](https://msdn.microsoft.com/library/bb397687.aspx)です。 ラムダは、メソッド ベースで使用[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)など、標準クエリ演算子メソッドを引数としてクエリを実行する、[場所](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx)上記のコードで使用されるメソッド。 定義されている場合、またはなどのメソッドを呼び出すことで変更されるときに、LINQ クエリは実行されません`Where`または`OrderBy`です。 代わりに、クエリの実行が遅延、つまり、その実際の値が実際に反復処理されるまで、式の評価が遅延されるか、 [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx)メソッドが呼び出されます。 `Search`サンプルについては、クエリで、 *Index.cshtml*ビュー。 クエリの遅延実行の詳細については、「[クエリの実行](https://msdn.microsoft.com/library/bb738633.aspx)」を参照してください。

> [!NOTE]
> [Contains](https://msdn.microsoft.com/library/bb155125.aspx)メソッドが、データベースでは、c# コードではなく上で実行します。 データベースで[Contains](https://msdn.microsoft.com/library/bb155125.aspx)にマップ[SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx)は大文字小文字を区別します。

これで、更新することができます、`Index`をユーザーに、フォームを表示するビュー。

アプリケーションを実行しに移動*/ビデオ/インデックス*です。 `?searchString=ghost` などのクエリ文字列を URL に追加します。 フィルターされたムービーが表示されます。

![SearchQryStr](adding-search/_static/image1.png)

署名を変更する場合、`Index`メソッドという名前のパラメーターに`id`、`id`パラメーターが一致、`{id}`既定値のプレース ホルダーのセットのルーティング、*アプリ\_スタートアップRouteConfig.cs*ファイル。

[!code-json[Main](adding-search/samples/sample4.json)]

元の`Index`メソッドは、次のようになります。

[!code-csharp[Main](adding-search/samples/sample5.cs)]

変更された`Index`メソッドは次のようになります。

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

これで、クエリ文字列の値ではなく、ルート データ (URL セグメント) として検索タイトルを渡すことができます。

![](adding-search/_static/image2.png)

ただし、ユーザーがムービーを検索するたびに URL の変更を求めることはできません。 する追加のヘルプを UI にはムービーにフィルターを適用します。 署名を変更した場合、`Index`ルート バインド ID パラメーターを渡す方法をテストする方法を変更することができるように、`Index`メソッドは、という名前の文字列パラメーターを受け取る`searchString`:

[!code-csharp[Main](adding-search/samples/sample7.cs)]

開く、 *Views\Movies\Index.cshtml*ファイル、および後だけ`@Html.ActionLink("Create New", "Create")`、下強調表示されている形式のマークアップを追加します。

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

`Html.BeginForm`ヘルパーの作成開始`<form>`タグ。 `Html.BeginForm`ヘルパーがユーザーをクリックしてフォームを送信するときにそれ自体に投稿する形式、**フィルター**ボタンをクリックします。

Visual Studio 2013 では、表示およびファイルの表示を編集するときに、便利な向上があります。 アプリケーションを実行する、ファイルの表示を開くときに、Visual Studio 2013 は、ビューを表示する正しいコント ローラー アクション メソッドを呼び出します。

![](adding-search/_static/image3.png)

ビューがインデックス (図のように、上記) を Visual Studio で開く、範囲 f5 キーまたは f5 キーを押してアプリケーションを実行してから、ムービーの検索 をタップします。

![](adding-search/_static/image4.png)

ない`HttpPost`のオーバー ロード、`Index`メソッドです。 必要はありません、メソッドは、アプリケーションの状態を変更されていないためだけのデータをフィルター処理します。

以下の `HttpPost Index` メソッドを追加できます。 アクション呼び出し元が一致する場合は、`HttpPost Index`メソッド、および`HttpPost Index`の次の図に示すようにメソッドが実行されます。

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

ただし、この `HttpPost` バージョンの `Index` メソッドを追加しても、実装方法は制限されます。 たとえば、特定の検索をブックマークするか、友だちにリンクを送信し、友だちがそれをクリックしてムービーのフィルターされた同じリストを表示できるようにするとします。 HTTP POST 要求の URL は、GET 要求 (localhost:xxxxx/ビデオ/インデックス) の URL と同じです--URL 自体に検索情報がないことに注意してください。 右ここで、検索文字列の情報がサーバーに送信、フォーム フィールドの値として。 つまり、ブックマークまたは URL で友人に送信するには、その検索情報をキャプチャすることはできません。

解決のオーバー ロードを使用するには`BeginForm`POST 要求が URL に検索情報を追加する必要がありますおよびルーティングする必要がありますを指定する、`HttpGet`のバージョン、`Index`メソッドです。 既存のパラメーターを置き換える`BeginForm`次のマークアップ メソッド。

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

今すぐ検索を送信するときに、URL には検索クエリ文字列が含まれます。 `HttpPost Index` メソッドがある場合でも、検索時には `HttpGet Index` アクション メソッドにも移動します。

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>ジャンルで検索を追加します。

追加した場合、`HttpPost`のバージョン、`Index`メソッドを削除します。

次に、ジャンルで映画を検索できるようにするための機能を追加します。 `Index` メソッドを次のコードで置き換えます。

[!code-csharp[Main](adding-search/samples/sample11.cs)]

このバージョンの`Index`メソッドには、追加のパラメーター、つまり`movieGenre`です。 最初の数行のコードを作成、`List`データベースからムービー ジャンルを保持するオブジェクト。

次のコードは、データベースからすべてのジャンルを取得する LINQ クエリです。

[!code-csharp[Main](adding-search/samples/sample12.cs)]

コードを使用して、`AddRange`ジェネリックのメソッド`List`一覧にすべての個別ジャンルを追加するコレクション。 (なし、`Distinct`修飾子は、重複するジャンルを追加するには — たとえば、コメディ サンプルに 2 回追加されます)。 コードでジャンルの一覧を格納し、`ViewBag.MovieGenre`オブジェクト。 カテゴリのデータ (このようなムービー ジャンルの) として格納する、 [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx)内のオブジェクト、 `ViewBag`、MVC アプリケーションの一般的なアプローチは、ドロップダウン リスト ボックスでカテゴリ データにアクセスし、します。

次のコードを確認する方法を示しています、`movieGenre`パラメーター。 空ではない、コードをさらには指定されたジャンルを選択したムービーを制限するムービーのクエリを制約します。

[!code-csharp[Main](adding-search/samples/sample13.cs)]

既に説明したように、クエリで実行していないデータ ベース ムービーの一覧を反復処理されるまで (動作は、ビューでは、後に、`Index`アクション メソッドを返します)。

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>マークアップをジャンルで検索をサポートするために、インデックス ビューに追加します。

追加、`Html.DropDownList`ヘルパーが、 *Views\Movies\Index.cshtml*ファイル、直前に、`TextBox`ヘルパー。 完全なマークアップを次に示します。

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

次のコード。

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

パラメーター"MovieGenre"のキーを提供する、`DropDownList`を検索するためのヘルパーを`IEnumerable<SelectListItem>`で、`ViewBag`です。 `ViewBag`がアクション メソッドで設定します。

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

オプション ラベルは、"All"パラメーター。 お使いのブラウザーでその選択を検査する場合、「値」属性が空であるが表示されます。 のみをフィルター処理、コント ローラーから`if`文字列ではありません`null`または空で、空の値を送信する`movieGenre`すべてジャンルを示しています。

既定で選択するオプションを設定することもできます。 コント ローラーでコードを変更すると、既定のオプションとして「コメディ」場合は、次のようにします。

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

アプリケーションを実行しを参照*/ビデオ/インデックス*です。 ジャンル、ムービーの名前、および両方の条件は、検索を再試行してください。

![](adding-search/_static/image8.png)

ここでは、アクション メソッドの検索とムービーのタイトル、ジャンルで検索できるようにするビューを作成します。 次のセクションでプロパティを追加する方法について見ていきます、`Movie`モデルおよびテスト用データベースが自動的に作成するには初期化子を追加する方法です。

>[!div class="step-by-step"]
[前へ](examining-the-edit-methods-and-edit-view.md)
[次へ](adding-a-new-field.md)
