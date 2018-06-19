---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: 効率的なデータのページングを実装して |Microsoft ドキュメント
author: microsoft
description: 手順 8 では、一度にディナーの 1,000 件を表示するには、代わりにのみ表示で 10 今後ディナーように、/Dinners URL にページング サポートを追加する方法を示します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 0188e21438820adf2adbe05b047fdb772540e1a0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873249"
---
<a name="implement-efficient-data-paging"></a>効率的なデータのページングを実装します。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF をダウンロードします。](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 8 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。
> 
> 手順 8 では、ディナー一度にの 1,000 件を表示するには、代わりにうまくのみ一度に - 10 今後ディナーを表示し、へ戻るページし、SEO フレンドリな方法ですべてのリストを転送するエンドユーザーを許可するように、/Dinners URL にページング サポートを追加する方法を示します。
> 
> ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner 手順 8: ページングのサポート

サイトが成功した場合は、今後ディナー数千になります。 UI がすべてこれらディナーの処理を拡張し、により、ユーザーはそれらを参照することを確認する必要があります。 ページング サポートを追加すると、これを有効にするには、当社 */Dinners*のみありますに 1 回、おディナーの 1,000 件を表示する方法の代わりに URL を一度に - 10 今後ディナーを表示し、エンドユーザーがページの背面とのすべてのリストを転送するを許可します。SEO フレンドリな方法です。

### <a name="index-action-method-recap"></a>Index() アクション メソッドの要約

Index() アクション メソッド、DinnersController クラス内で現在よう下。

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

ときに、要求が行われる、 */Dinners* URL、今後すべてディナーの一覧を取得し、それらのすべての一覧を表示。

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>Understanding IQuerable&lt;T&gt;

*IQueryable&lt;T&gt;* は .NET 3.5 の一部として、LINQ で導入されたインターフェイスです。 お利用できるにページング サポートを実装する強力な「遅延実行」のシナリオを可能になります。

IQueryable を返すことは、DinnerRepository で&lt;Dinner&gt; FindUpcomingDinners() メソッドからシーケンス。

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

IQueryable&lt;Dinner&gt; FindUpcomingDinners() メソッドによって返されるオブジェクトは、LINQ to SQL を使用して、データベースから Dinner オブジェクトを取得するクエリをカプセル化します。 重要なは、クエリ内のデータに対するアクセス/反復処理するまで、またはそれに ToList() メソッドを呼び出してデータベースに対してクエリを実行ことはありません。 当社 FindUpcomingDinners() メソッドを呼び出すコードは、必要に応じてフィルターを追加するその他「チェーン」操作/IQueryable に選択できます&lt;Dinner&gt;オブジェクト クエリを実行する前にします。 LINQ to SQL はデータが要求されたときに、結合されたデータベースに対してクエリを実行するのに十分なスマートになります。

ページングのロジックを実装する更新することが、DinnersController の Index() アクション メソッドに返される IQueryable"Skip"と「実行」の追加の演算子が適用されるよう&lt;Dinner&gt;で ToList() を呼び出す前にシーケンス。

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

上記のコードでは、データベース内の最初の 10 個の今後ディナーをスキップし、バックアップ 20 ディナーを返します。 LINQ to SQL は – SQL データベースと web サーバーではなくロジックをスキップしています。 これを実行する最適化された SQL クエリを構築できるほどです。 これは、何百万も今後ディナーのデータベースにある、場合でも、必要な 10 だけは (やすく効率的でスケーラブルな) この要求の一部として取得することを意味します。

### <a name="adding-a-page-value-to-the-url"></a>URL に「ページ」の値を追加します。

ハードコーディングする代わりに、特定のページ範囲、ユーザーが要求しているどの Dinner 範囲を示す「ページ」のパラメーターを含む Url がいいでしょう。

#### <a name="using-a-querystring-value"></a>クエリ文字列値を使用します。

次のコードは、クエリ文字列パラメーターをサポートすると同様に Url を有効にする、Index() アクション メソッドを更新お方法を示します */Dinners しますか? ページ 2 =*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

上記の Index() アクション メソッドには、「ページ」を名前付きパラメーターがあります。 パラメーターが null 許容の整数として宣言されている (はどのような int? を示します)。 つまり、 */Dinners しますか? ページ 2 =* URL パラメーター値として渡されるには、「2」の値になります。 */Dinners* URL (クエリ文字列の値) が渡される null 値になります。

おはサイズを乗算ページの値 ページ (この場合は 10 行) をスキップする数ディナーを決定します。 使用して、 [c# null「合体」演算子 (?)](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx)は null 許容型を処理する場合に便利です。 上記のコードでは、ページのパラメーターが null の場合、ページが 0 の値に割り当てます。

#### <a name="using-embedded-url-values"></a>埋め込まれた URL 値を使用します。

クエリ文字列の値を使用する代わりには、実際の URL 自体内でページのパラメーターを埋め込むになります。 例: */Dinners/Page/2*または*ディナー/2*です。 ASP.NET MVC には、強力な URL ルーティング エンジン容易に次のようなシナリオをサポートするものにはが含まれています。

カスタムのルーティング規則受信 URL または URL を選びましたコント ローラー クラスまたはアクション メソッドをフォーマットするマップ登録できます。 タスクが必要なは、プロジェクト内での Global.asax ファイルを開きます。

![](implement-efficient-data-paging/_static/image2.png)

ルートの最初の呼び出しのような MapRoute() ヘルパー メソッドを使用して、新しいマッピング ルールを登録します。MapRoute() 下:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

上記の"UpcomingDinners"をという名前の新しいルーティング ルールが登録されます。 URL の形式であることを示しているお"ディナー/ページ/{ページ}"– {ページ} は、URL に埋め込まれたパラメーター値。 MapRoute() メソッドを 3 番目のパラメーターは、DinnersController クラス Index() アクション メソッドには、この形式に一致する Url を割り当てる必要があることを示します。

発生しました。 前に – クエリ文字列に紹介したシナリオと、"page"パラメーターは、URL と、クエリ文字列ではないから取得するようになりました点を除いてとまったく同じ Index() コードを使用できます。

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

今すぐおアプリケーションを実行および入力 */Dinners*最初の 10 個の今後のディナー後ほどお見せします。

![](implement-efficient-data-paging/_static/image3.png)

ときに入力することと */Dinners/Page/1*おディナーの次のページが表示されます。

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>ページ ナビゲーション UI を追加します。

ページングに紹介したシナリオを完了する最後の手順は、Dinner データを簡単にスキップするユーザーを有効にするには、当社ビュー テンプレート内で、次へ」や「以前」のナビゲーション UI を実装するされます。

これを正しく実装するには必要がありますディナーの合計数をデータベースにわかっているにも方法と、データの多くのページと解釈されます。 現在の要求の「ページ」の値を先頭または、データの末尾にあるかどうか計算および表示を切り替えるそれに応じて「以前」と [次へ] の UI を非表示にし必要になります。 このロジックを実装すると、当社 Index() アクション メソッド内でお可能性があります。 代わりにヘルパー クラスを再利用可能な複数の方法でこのロジックをカプセル化するプロジェクトに追加できます。

一覧から派生した単純な"PaginatedList"ヘルパー クラスを次に示します&lt;T&gt;に組み込まコレクション クラス、.NET Framework。 IQueryable データの任意のシーケンスを改ページ調整に使用できる再利用可能なコレクション クラスを実装します。 NerdDinner アプリケーションでは必要がある IQueryable 経由で動作する&lt;Dinner&gt;結果が同じくらい簡単にでも使用 IQueryable&lt;製品&gt;または IQueryable&lt;顧客&gt;他のアプリケーション シナリオで発生します。

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

上記を計算する方法および"PageIndex"、"PageSize"、"TotalCount"および"TotalPages"プロパティを公開と同様に、されます。 ヘルパーの 2 つのプロパティ"HasPreviousPage"と"HasNextPage"コレクション内のデータのページが先頭または元のシーケンスの末尾にあるかどうかを示すも公開します。 上記のコードを実行する - Dinner オブジェクトの合計数のカウントを取得する最初の 2 つの SQL クエリとなります (これは、オブジェクトを返さない – 整数を返す「数の選択」ステートメントの実行ではなく)、2 番目の行だけを取得するにはデータのデータの現在のページをデータベースから必要があります。

当社 DinnersController.Index() ヘルパー メソッド、PaginatedList を作成し、更新することが&lt;Dinner&gt; DinnerRepository.FindUpcomingDinners() から発生し、このビュー テンプレートに渡すこと。

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

ViewPage から継承する \Views\Dinners\Index.aspx ビュー テンプレート、更新することが&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt; &gt; ViewPage ではなく&lt;IEnumerable&lt;Dinner&gt;&gt;、前後のナビゲーション UI を非表示のビュー テンプレートの下部に次のコードを追加します。

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

方法を使用して上記 Html.RouteLink() ヘルパー メソッド、ハイパーリンクを生成することを確認します。 このメソッドは、以前に使用した Html.ActionLink() ヘルパー メソッドに似ています。 違いは、ルール、Global.asax ファイル内でセットアップをルーティングする"UpcomingDinners"を使用して、URL を生成することです。 これにより、形式を持つ、Index() アクション メソッドに Url を生成しますお: */Dinners/ページ/{ページ}* {ページ} の値は、変数が提供するという上記現在 PageIndex に基づいて – です。

今すぐアプリケーションを実行したときにもう一度おが表示されます、一度に 10 ディナー ブラウザーに。

![](implement-efficient-data-paging/_static/image5.png)

あるも&lt; &lt; &lt;と&gt; &gt; &gt;ナビゲーションを転送をスキップし、上方向へ、データを使用して経由での検索エンジンのアクセス可能な Url を使用すると、ページの下部にある UI:

![](implement-efficient-data-paging/_static/image6.png)

| **側トピックの内容は、IQueryable の影響を理解する&lt;T&gt;** |
| --- |
| IQueryable&lt;T&gt;非常に強力な機能をさまざまな興味深い遅延実行のシナリオを有効にする (ページングと構成のように基づくクエリ)。 として、強力な機能はすべて、使用する方法を慎重にしてはいないに悪用されることを確認します。 IQueryable を返すことを認識することが重要&lt;T&gt;結果をリポジトリから、連結演算子のメソッドに追加し、そのため、最終的なクエリの実行に参加するための呼び出し元のコードを有効にします。 この機能は、呼び出し元のコードを提供したくないかどうかは、返す必要があります IList をバックアップ&lt;T&gt;または IEnumerable&lt;T&gt;結果 - は既に実行したクエリの結果が含まれています。 改ページ調整シナリオには、リポジトリ メソッドが呼び出されるとに、実際のデータの改ページ調整ロジックをプッシュするこれが必要です。 このシナリオでは、FindUpcomingDinners() finder メソッドに返される、PaginatedList がいずれかの署名がある可能性があります更新: PaginatedList&lt; Dinner&gt; FindUpcomingDinners (int pageIndex、int pageSize) {} の戻り値は、IList を戻す&lt;Dinner&gt;、ディナーの合計数を返す"totalCount"out パラメーターを使用して: IList&lt;Dinner&gt; FindUpcomingDinners (int pageIndex、int totalCount out の int pageSize) {} |

### <a name="next-step"></a>次の手順

これで、アプリケーションへの認証と承認をサポートを追加お方法を見てみましょう。

> [!div class="step-by-step"]
> [前へ](re-use-ui-using-master-pages-and-partials.md)
> [次へ](secure-applications-using-authentication-and-authorization.md)
