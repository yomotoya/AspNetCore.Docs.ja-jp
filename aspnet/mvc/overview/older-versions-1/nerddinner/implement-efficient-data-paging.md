---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: 効率的なデータ ページングを実装する |Microsoft Docs
author: microsoft
description: 手順 8 では、dinners 一度に数千ものを表示するには、代わりで今後 10 の dinners を表示しますがのみように、/Dinners URL にページング サポートを追加する方法を示します.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: bcd7fdf59fac8328752aa2ebab61c1d50a8b6b0d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842144"
---
<a name="implement-efficient-data-paging"></a>効率的なデータ ページングを実装します。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF のダウンロード](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 8 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。
> 
> 手順 8 では、dinners 一度に数千ものを表示するには、代わりには、のみ - 一度に 10 個の今後の dinners を表示し、戻るページし、SEO フレンドリな方法で、一覧全体を転送するエンドユーザーを許可するありますようにする、/Dinners URL にページング サポートを追加する方法を示します。
> 
> 次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner 手順 8: ページングのサポート

私たちのサイトが成功した場合は、何千もの今後の dinners があります。 UI がすべてこれらの dinners の処理を拡張し、それらを参照することができます、かどうかを確認する必要があります。 これを有効にするのには、ページング サポートを追加します、 */Dinners*のみが数千の dinners を表示するのにはの代わりに URL を一度に - 10 今後 dinners を表示し、戻るページおよびでは、すべてのリストを転送するエンドユーザーを許可します。SEO のわかりやすい方法です。

### <a name="index-action-method-recap"></a>Index() アクション メソッドの要約

Index() アクション メソッド、DinnersController クラス内で現在は次のような。

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

要求はに対して行われた場合、 */Dinners* URL、今後のすべて dinners の一覧を取得し、それらのすべての一覧を表示します。

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>Understanding IQuerable&lt;T&gt;

*IQueryable&lt;T&gt;* は .NET 3.5 の一部として、LINQ で導入されたインターフェイスです。 活用ページング サポートを実装するために実行できる強力な「遅延実行」のシナリオを可能になります。

IQueryable を返すことのときは必ず DinnerRepository で&lt;Dinner&gt; FindUpcomingDinners() メソッドからシーケンス。

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

対象の IQueryable&lt;Dinner&gt; FindUpcomingDinners() メソッドによって返されるオブジェクトは、LINQ to SQL を使用して、データベースから Dinner オブジェクトを取得するクエリをカプセル化します。 重要なは、か ToList() メソッドを呼び出すことで、クエリ内のデータをアクセス/反復処理するまでのデータベースに対してクエリを実行しません。 この FindUpcomingDinners() メソッドを呼び出すコードは、IQueryable に追加の「チェーン」操作/フィルターを追加する必要に応じて選択できます&lt;Dinner&gt;オブジェクト クエリを実行する前にします。 LINQ to SQL は、データが要求されたときに、データベースに対して結合のクエリを実行するのに十分なスマートでは、します。

返される対象の IQueryable を「スキップ」と「実行」の追加の演算子が適用されるページング ロジックを実装するために、DinnersController の Index() アクション メソッドを更新しましたできます&lt;Dinner&gt;に ToList() を呼び出す前にシーケンス。

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

上記のコードでは、データベース内の最初の 10 の今後 dinners をスキップし、戻る 20 dinners を返します。 LINQ to SQL では、– SQL データベースと web サーバーではなくロジックをスキップしています。 これを実行する最適化された SQL クエリを構築できるほどスマートです。 これは、データベースに何百万もの今後の Dinners がある場合でも、必要な 10 のみは (ので効率的でスケーラブルな) この要求の一部として取得することを意味します。

### <a name="adding-a-page-value-to-the-url"></a>URL に「ページ」値を追加します。

特定のページ範囲をハードコーディングするのではなく、Url にユーザーを要求しているどの Dinner 範囲を示す「ページ」パラメーターを含めてがいいでしょう。

#### <a name="using-a-querystring-value"></a>クエリ文字列値を使用します。

次のコードでは、クエリ文字列パラメーターをサポートし、Url などを有効にする、Index() アクション メソッドの更新を示します */Dinners でしょうかページ 2 =*:。

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

上記の Index() アクション メソッドには、「ページ」という名前のパラメーターがあります。 パラメーターが null 許容の整数として宣言されている (どのような int ですか? を示します)。 つまり、 */Dinners でしょうか。 ページ 2 =* URL パラメーター値として渡されるには、"2"の値になります。 */Dinners* (クエリ文字列値) のない URL が渡される null 値になります。

ページ サイズ (この場合は 10 行) をページの値を乗算するをスキップする数の dinners を判断することは。 使用している、 [c# null「結合」演算子 (?)](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) null 許容型を扱う場合に便利です。 上記のコードは、ページのパラメーターが null の場合に、ページの 0 の値を割り当てます。

#### <a name="using-embedded-url-values"></a>埋め込まれた URL 値を使用してください。

クエリ文字列値を使用する代わりには、埋め込む自体は実際の URL 内にあるページ パラメーターになります。 例: */Dinners/Page/2*または */Dinners/2*します。 ASP.NET MVC には、このようなシナリオをサポートしやすい強力な URL ルーティング エンジンが含まれています。

受信 URL または URL の形式、コント ローラー クラスまたはアクション メソッドをマップするカスタムのルーティング規則を登録することができます。 To do 必要がありますは、このプロジェクト内で、Global.asax ファイルを開くことだけです。

![](implement-efficient-data-paging/_static/image2.png)

ルートの最初の呼び出しのような MapRoute() ヘルパー メソッドを使用して、新しいマッピング ルールを登録します。MapRoute() 下:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

上記の"UpcomingDinners"をという名前の新しいルーティング規則が登録されます。 URL の形式であることを示すことは"Dinners/ページ/{ページ}": {ページ} は、URL に埋め込まれたパラメーター値。 MapRoute() メソッドの 3 番目のパラメーターは、DinnersController クラス Index() アクション メソッドには、この形式に一致する Url をマップする必要があることを示します。

点を除いて、URL とクエリ文字列ではないから、「ページ」パラメーターになるようになりました、Querystring シナリオ – 以前まったく同じ Index() コードを使用できます。

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

ここでときに、アプリケーションを実行し、入力と */Dinners*最初の 10 個の今後の dinners を見てみましょう。

![](implement-efficient-data-paging/_static/image3.png)

入力 */Dinners/Page/1* dinners の次のページを見てみましょう。

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>ページ ナビゲーション UI を追加します。

Dinner データを簡単にスキップできるように、ビュー テンプレート内の"next"および「前」のナビゲーション UI を実装するために、ページングのシナリオを完了する最後の手順になります。

これを正しく実装するに必要になります Dinners の総数をデータベースにわかっているにも方法は複数のデータ ページと解釈されます。 現在要求されている「ページ」値が先頭または、データの末尾かどうかを計算し、表示を切り替える適宜「前」と"次へ の UI を非表示にし、必要になります。 このロジックを実装、Index() アクション メソッド内で可能性があります。 また、プロジェクトを再利用可能な複数の方法でこのロジックをカプセル化するヘルパー クラスを追加できます。

一覧から派生した単純な"PaginatedList"ヘルパー クラスを次に示します&lt;T&gt;コレクション クラスに組み込まれて、.NET Framework です。 IQueryable データの任意のシーケンスを改ページを使用できる再利用可能なコレクション クラスを実装しています。 アプリケーションで NerdDinner した IQueryable 経由で動作&lt;Dinner&gt;結果がされる可能性がありますには簡単に IQueryable に対して&lt;製品&gt;または IQueryable&lt;顧客&gt;他のアプリケーション シナリオになります。

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

上の例が計算され、"PageIndex"、"PageSize"、"TotalCount"および"TotalPages"などの公開のプロパティの します。 ヘルパーの 2 つのプロパティ"HasPreviousPage"と"HasNextPage"コレクション内のデータのページが先頭または元のシーケンスの末尾にあるかどうかを示すも公開します。 上記のコードを実行する - Dinner オブジェクトの合計数の数を取得する最初の 2 つの SQL クエリとなります (これには、オブジェクトを返さない – ではなく整数を返す"SELECT COUNT"ステートメントを実行します)、2 番目の行だけを取得するにはデータは、データベースのデータの現在のページから必要があります。

今後、PaginatedList を作成する、DinnersController.Index() ヘルパー メソッドを更新できるし&lt;Dinner&gt; DinnerRepository.FindUpcomingDinners() から、発生して、ビュー テンプレートに渡すこと。

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

ViewPage から継承する \Views\Dinners\Index.aspx ビュー テンプレートを更新することができますし&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt; &gt; ViewPage ではなく&lt;IEnumerable&lt;Dinner&gt;&gt;、または次または前のナビゲーション UI を非表示に、ビュー テンプレートの下部に次のコードを追加します。

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

このハイパーリンクを生成する Html.RouteLink() のヘルパー メソッドを使って方法に注意してください。 このメソッドは、以前に使用した Html.ActionLink() ヘルパー メソッドに似ています。 違いは、"UpcomingDinners"ルーティング規則、Global.asax ファイル内でセットアップを使用して URL を生成することです。 これにより、Url、形式を持つ、Index() アクション メソッドを生成します: */Dinners/ページ/{ページ}* -{ページ} の値が提供するという上記現在 PageIndex に基づいて変数は、場所。

これで、アプリケーションを実行するともう一度おいでください、一度に 10 個の dinners、ブラウザーで。

![](implement-efficient-data-paging/_static/image5.png)

またある&lt; &lt; &lt;と&gt; &gt; &gt;転送をスキップし、旧バージョンとを使用して、データをエンジンにアクセスできる Url を検索できるようにするページの下部にある UI のナビゲーション。

![](implement-efficient-data-paging/_static/image6.png)

| **側のトピック: IQueryable の影響を理解する&lt;T&gt;** |
| --- |
| IQueryable&lt;T&gt;により、遅延実行の興味深いシナリオのさまざまな強力な機能は、(ページングおよびコンポジションのように基づくクエリ)。 として、強力な機能はすべて、使用する方法を慎重にしてはいないに悪用されることを確認します。 IQueryable を返すことを認識することが重要&lt;T&gt;リポジトリからの結果により、呼び出し元のコードは、連結演算子のメソッドに追加し、そのため、最終的なクエリの実行に参加します。 この機能は、呼び出し元のコードを提供したくないかどうかは、返す必要があります IList をバックアップ&lt;T&gt;または IEnumerable&lt;T&gt;が既に実行しているクエリの結果を含んだ結果。 改ページ調整シナリオをリポジトリ メソッドが呼び出される実際のデータの改ページ調整ロジックをプッシュする必要がこれは。 このシナリオでは、返される、PaginatedList がいずれかの署名、FindUpcomingDinners() finder メソッドを更新可能性があります: PaginatedList&lt; Dinner&gt; FindUpcomingDinners (pageIndex の int、int pageSize) {} 返された場合は、IList をバックアップまたは&lt;Dinner&gt;、Dinners の合計数を返す"totalCount"out パラメーターを使用して: IList&lt;Dinner&gt; FindUpcomingDinners (pageIndex の int、int totalCount アウトの int pageSize) {} |

### <a name="next-step"></a>次の手順

これで、アプリケーションへの認証と承認のサポートを追加する方法を見てみましょう。

> [!div class="step-by-step"]
> [前へ](re-use-ui-using-master-pages-and-partials.md)
> [次へ](secure-applications-using-authentication-and-authorization.md)
