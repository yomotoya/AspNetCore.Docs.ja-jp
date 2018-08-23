---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
title: コンテンツ ページ (c#) からマスター ページと対話する |Microsoft Docs
author: rick-anderson
description: メソッドを呼び出して、コンテンツ ページのコードからのマスター ページなどのプロパティを設定する方法について説明します。
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 32d54638-71b2-491d-81f4-f7417a13a62f
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
msc.type: authoredcontent
ms.openlocfilehash: afcec6cb7a6763d068301c7afdd28ff560a3065b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835859"
---
<a name="interacting-with-the-master-page-from-the-content-page-c"></a>コンテンツ ページ (c#) からマスター ページと対話します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_CS.pdf)

> メソッドを呼び出して、コンテンツ ページのコードからのマスター ページなどのプロパティを設定する方法について説明します。


## <a name="introduction"></a>はじめに

マスター ページを作成する方法を見てきました過去の 5 つのチュートリアルの過程で、コンテンツ領域を定義し、マスター ページ、ASP.NET ページにバインドして、ページ固有のコンテンツを定義します。 訪問者は、特定のコンテンツ ページを要求するときに、コンテンツとマスター ページのマークアップが統一されたコントロールの階層構造の表示になり、実行時に組み合わされします。 そのため、既に 1 つの方法、マスター ページとそのコンテンツ ページの 1 つできますと連動する検出された: コンテンツ ページは、マスター ページの ContentPlaceHolder のコントロールに transfuse マークアップについて詳しく説明します。

まだを調べるにはマスター ページとコンテンツ ページやり取りプログラムを使用します。 マスター ページの ContentPlaceHolder のコントロールのマークアップを定義するだけでなく、コンテンツ ページもそのマスター ページのパブリック プロパティに値を割り当てるし、パブリック メソッドを呼び出すことができます。 同様に、マスター ページは、そのコンテンツ ページと対話可能性があります。 マスターおよびコンテンツのページ間の相互作用をプログラムでは、宣言型マークアップ間の相互作用よりもまれですが、このようなプログラムによる操作が必要な多くのシナリオがあります。

このチュートリアルで調べるコンテンツ ページのプログラムで対話方法、マスター ページ。次のチュートリアルでは、マスター ページの同様に対話方法、コンテンツ ページに注目します。

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>コンテンツ ページとそのマスター ページの間のプログラムによる操作の例

ページの特定のリージョンでは、ページごとに構成する必要がある、ときに、プレース ホルダー コントロールを使用します。 しかし、についてページの大半が特定の出力がページの数が少ないを出力する必要がある場合は、その他を表示するようにカスタマイズする必要があります。 でしょうか。 たとえば、説明されていますが、 [*の複数の ContentPlaceHolders と既定のコンテンツ*](multiple-contentplaceholders-and-default-content-cs.md)チュートリアルでは、各ページにログイン インターフェイスを表示する必要があります。 ほとんどのページには、ログインのインターフェイスを含める必要があります、これを抑制するか、ページのいくつかのなど: メインのログイン ページ (`Login.aspx`); アカウントの作成 ページでは、認証されたユーザーにアクセスできる他のページ。 [*の複数の ContentPlaceHolders と既定のコンテンツ*](multiple-contentplaceholders-and-default-content-cs.md)チュートリアルでは、マスター ページで ContentPlaceHolder の既定のコンテンツを定義する方法を示しましたし、し、場所のページにオーバーライドする方法、既定のコンテンツが必要でないです。

別のオプションでは、パブリック プロパティまたはログインのインターフェイスを非表示かどうかを示すマスター ページ内のメソッドを作成します。 たとえば、マスター ページがという名前のパブリック プロパティを含めることが`ShowLoginUI`設定に使用された値を持つ、`Visible`マスター ページで、ログイン コントロールのプロパティ。 ログインのユーザー インターフェイスを抑制する必要があるこれらのコンテンツ ページをプログラムで設定でしたし、`ShowLoginUI`プロパティを`false`します。

おそらくコンテンツとマスター ページの相互作用の最も一般的な例は、何らかのアクションが [コンテンツ] ページで経過した後に更新するマスター ページのニーズにデータが表示される場合に発生します。 最近、5 つを表示する GridView を含むマスター ページは、特定のデータベース テーブルからレコードを追加を検討してくださいし、そのコンテンツ ページの 1 つには同じテーブルに新しいレコードを追加するためのインターフェイスが含まれています。

ユーザーは、新しいレコードを追加するページにアクセス、最後に追加された 5 つのマスター ページに表示されるレコードが表示されます。 新しいレコードの列の値では、値を入力し、彼女は、フォームを送信します。 マスター ページに GridView があると仮定すると、`EnableViewState`プロパティが true (既定) は、ビュー ステートからそのコンテンツが再度読み込まれ、新しいレコードをデータベースに追加された場合でもその結果、同じ 5 つのレコードが表示されます。 これにより、ユーザーが混乱する可能性があります。

> [!NOTE]
> ポストバックのたびに基になるデータ ソースに、再バインドするために GridView のビュー ステートを無効にした場合でもまだ表示されませんだけで追加したレコードのデータがページのライフ サイクル、データベース オブジェクトに新しいレコードを追加する場合よりも前の GridView にバインドされているためase。


マスター ページで、単に追加されたレコードが表示されるように、これを解決するには、ポストバックにそのデータ ソースを再バインドする GridView を指示する必要があります GridView の*後*新しいレコードがデータベースに追加されています。 新しいレコード (とそのイベント ハンドラー) が [コンテンツ] ページが更新する必要がある GridView に追加するためのインターフェイスがマスター ページのため、コンテンツおよびマスター ページ間の相互作用が必要です。

[コンテンツ] ページで、イベント ハンドラーからマスター ページの表示を更新すると、コンテンツ、マスター ページの相互作用のための最も一般的なニーズの 1 つは、ため、このトピックでさらに詳しく見ていきましょう。 このチュートリアルのダウンロードには、という名前の Microsoft SQL Server 2005 Express Edition データベースが含まれている`NORTHWIND.MDF`で web サイトの`App_Data`フォルダー。 Northwind データベースには、製品、従業員、および Northwind traders 社という架空の会社の売上情報が格納されます。

手順 1、5 つの最も最近表示いきますは、マスター ページで、GridView で製品を追加します。 手順 2 では、新製品を追加するためのコンテンツ ページを作成します。 マスター ページで、パブリック プロパティおよびメソッドを作成する方法を次の手順 3. と手順 4. がプログラムによってこれらのプロパティと、[コンテンツ] ページからのメソッドとやり取りする方法を示しています。

> [!NOTE]
> このチュートリアルは、ASP.NET 内のデータの操作の詳細を掘り下げてはされません。 データとデータを挿入するためのコンテンツ ページを表示するマスター ページを設定するための手順は、完全なまだ手軽なです。 データを挿入すると、SqlDataSource コントロールと GridView コントロールを使用して表示に関する詳細については、このチュートリアルの最後に、それ以上の測定値のセクションでリソースを参照してください。


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>手順 1: 最近、5 つを表示するマスター ページで製品を追加

開く、`Site.master`マスター ページと、ラベルと GridView コントロールを追加、 `leftContent` `<div>`します。 ラベルのクリアします`Text`プロパティを設定、`EnableViewState`プロパティを false に、その`ID`プロパティを`GridMessage`; GridView の設定`ID`プロパティを`RecentProducts`します。 次に、デザイナーには、GridView のスマート タグを展開し、新しいデータ ソースにバインドを選択します。 これにより、データ ソース構成ウィザードが起動します。 Northwind データベースのため、`App_Data`フォルダーは、Microsoft SQL Server データベース (図 1 参照) を選択して、SqlDataSource を作成することも; SqlDataSource を名前`RecentProductsDataSource`します。


[![RecentProductsDataSource SqlDataSource コントロールを GridView にバインドします。](interacting-with-the-master-page-from-the-content-page-cs/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image1.png)

**図 01**: SqlDataSource コントロールの名前、GridView にバインド`RecentProductsDataSource`([フルサイズの画像を表示する をクリックします](interacting-with-the-master-page-from-the-content-page-cs/_static/image3.png))。


次の手順では、どのようなデータベースに接続するかを指定するよう求められます。 選択、`NORTHWIND.MDF`ドロップダウン リストからファイルをデータベースし、[次へ] をクリックします。 接続文字列を保存するがウィザードによって提供これはこのデータベースを使用して初めてであるため、`Web.config`します。 名前を使用して、接続文字列を格納することがある`NorthwindConnectionString`します。


[![Northwind データベースへの接続します。](interacting-with-the-master-page-from-the-content-page-cs/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image4.png)

**図 02**: Northwind データベースへの接続 ([フルサイズの画像を表示する をクリックします](interacting-with-the-master-page-from-the-content-page-cs/_static/image6.png))。


データ ソース構成ウィザードでは、データを取得するためのクエリを指定して、2 つの手段を提供します。

- カスタム SQL ステートメントまたはストアド プロシージャを指定することで、または
- テーブルまたはビューを選択し、返される列を指定

製品を最後に追加した 5 を返すため、カスタム SQL ステートメントを指定する必要があります。 次の SELECT クエリを使用します。


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample1.sql)]

`TOP 5`キーワードは、クエリから最初の 5 つのレコードのみを返します。 `Products`テーブルの主キー、`ProductID`は、`IDENTITY`列は、テーブルに追加された新しい各製品が前のエントリより大きい値にあることを保証します。 そのためで結果を並べ替える`ProductID`降順以降最も最近では作成した製品を返します。


[![最近追加された 5 つの製品を返す](interacting-with-the-master-page-from-the-content-page-cs/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image7.png)

**図 03**: 5 つ最も最近追加の製品を返す ([フルサイズの画像を表示する をクリックします](interacting-with-the-master-page-from-the-content-page-cs/_static/image9.png))。


ウィザードを完了すると、Visual Studio に表示する GridView の 2 つの BoundFields が生成されます、`ProductName`と`UnitPrice`フィールドは、データベースから返されます。 この時点でマスター ページの宣言型マークアップは、次のようなマークアップを含める必要があります。


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample2.aspx)]

ご覧のように、マークアップが含まれています: Label Web コントロール (`GridMessage`); GridView `RecentProducts`、2 つの BoundFields; と、SqlDataSource コントロールを最後に追加された 5 つの製品を返します。

GridView を作成し、SqlDataSource コントロールを構成するには、このブラウザーから web サイトにアクセスします。 図 4 に示すよう、左下隅を最も最近、5 つの一覧を表示するグリッドを追加の製品が表示されます。


[![GridView には、5 つの最も最近追加された製品が表示されます。](interacting-with-the-master-page-from-the-content-page-cs/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image10.png)

**図 04**: GridView には、5 つ最も最近追加された製品が表示されます ([フルサイズの画像を表示する をクリックします](interacting-with-the-master-page-from-the-content-page-cs/_static/image12.png))。


> [!NOTE]
> 自由に、GridView の外観をクリーンアップできます。 いくつかの候補が表示されている書式設定を含める`UnitPrice`通貨と背景色とフォントを使用して、グリッドの外観を改善する値。


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>手順 2: 新しい製品を追加するコンテンツ ページの作成

ユーザーが新しい製品を追加するコンテンツ ページを作成する、次のタスクでは、`Products`テーブル。 新しいコンテンツ ページを追加、`Admin`という名前のフォルダー`AddProduct.aspx`を確実にそれをバインドに、`Site.master`マスター ページ。 図 5 は、このページは、web サイトに追加した後、ソリューション エクスプ ローラーを示します。


[![Admin フォルダーに新しい ASP.NET ページを追加します。](interacting-with-the-master-page-from-the-content-page-cs/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image13.png)

**図 05**: に新しい ASP.NET ページの追加、`Admin`フォルダー ([フルサイズの画像を表示する をクリックします](interacting-with-the-master-page-from-the-content-page-cs/_static/image15.png))。


そのことを思い出してください、 [*マスター ページのタイトル、メタ タグ、およびその他の HTML ヘッダーを指定する*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)という名前のカスタム ベース ページ クラスを作成したチュートリアル`BasePage`いた場合、ページのタイトルを生成します。明示的に設定します。 移動して、`AddProduct.aspx`ページの分離コード クラスし、派生させる`BasePage`(の代わりにから`System.Web.UI.Page`)。

最後に、更新、`Web.sitemap`このレッスンのエントリを追加するファイル。 下に次のマークアップを追加、`<siteMapNode>`コントロール ID の名前付けの問題のレッスン。


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample3.xml)]

図 6、これを追加で示すように`<siteMapNode>`要素は、レッスンの一覧に反映されます。

戻り`AddProduct.aspx`します。 コンテンツ コントロールで、 `MainContent` ContentPlaceHolder、DetailsView コントロールを追加し、名前を付けます`NewProduct`します。 DetailsView をという名前の新しい SqlDataSource コントロールにバインド`NewProductDataSource`します。 手順 1. で SqlDataSource、Northwind データベースを使用するように、ウィザードを構成し、カスタム SQL ステートメントを指定するようにします。 データベースに項目を追加、DetailsView を使用するため、両方とも指定する必要があります、`SELECT`ステートメントおよび`INSERT`ステートメント。 次を使用して、`SELECT`クエリ。


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample4.sql)]

次に、[挿入] タブから次の追加`INSERT`ステートメント。


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample5.sql)]

ウィザードの完了後は、DetailsView のスマート タグに移動し、"挿入を有効にする チェック ボックスをオンします。 DetailsView を [commandfield] に追加の`ShowInsertButton`プロパティを true に設定します。 データを挿入するためにだけこの DetailsView を使用するための設定、DetailsView の`DefaultMode`プロパティを`Insert`します。

すべてです。 このページをテストしてみましょう。 参照してください`AddProduct.aspx`ブラウザーで、(図 6 参照)、名前と価格を入力します。


[![データベースに新しい製品を追加します。](interacting-with-the-master-page-from-the-content-page-cs/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image16.png)

**図 06**: データベースに新しい製品を追加 ([フルサイズの画像を表示する をクリックします](interacting-with-the-master-page-from-the-content-page-cs/_static/image18.png))。


新しい製品の価格と名前で入力すると、[挿入] ボタンをクリックします。 これにより、ポストバックするためのフォームです。 ポストバック時に、SqlDataSource コントロールの`INSERT`ステートメントが実行されます。 その 2 つのパラメーターは、DetailsView の 2 つの TextBox コントロールにユーザーが入力した値が格納されます。 残念ながら、視覚的なフィードバック、挿入が発生したことはありません。 表示するには、新しいレコードが追加されたことを確認するメッセージを用意すると良いでしょう。 ままを演習として、リーダーの。 また、DetailsView から新しいレコードを追加した後のマスター ページに GridView まだを示しています; 前に、と、同じ 5 つのレコードこれは、単に追加されたレコードには含まれません。 後の手順でこの問題を解決する方法を考察します。

> [!NOTE]
> 何らかの形式の視覚的なフィードバック、挿入が成功したことを追加するだけでも検証をインクルードする DetailsView の挿入のインターフェイスを更新することをぜひなります。 現時点では、検証することはありません。 ユーザーの無効な値を入力した場合、`UnitPrice`フィールドに、"すぎます安価ですが、"システムがその文字列を 10 進数に変換しようとするとポストバック時に、例外がスローされます。 挿入する、カスタマイズの詳細については、インターフェイスを参照してください、 [*データ変更インターフェイスをカスタマイズ*チュートリアル](../../data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)からマイ[データのチュートリアルシリーズの操作](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>手順 3: マスター ページでのパブリック プロパティおよびメソッドの作成

手順 1. でという名前のラベルの Web コントロールを追加しました`GridMessage`のマスター ページに GridView 上。 このラベルは、必要に応じてメッセージを表示するものです。 新しいレコードを追加した後など、`Products`テーブル、というメッセージを表示する可能性があります:"*ProductName*がデータベースに追加されました"。 ハードコードされたマスター ページでは、このラベルのテキストではなく、メッセージ コンテンツ ページでカスタマイズできるようにします可能性があります。

ラベル コントロールは、マスター ページ内でプロテクト メンバー変数として実装されるため、コンテンツ ページから直接アクセスできません。 Web コントロールを公開または使用されるプロパティのいずれかを指定できますプロキシとして機能するマスター ページ内のパブリック プロパティの作成に必要なコンテンツ ページから (または、さらに言えば、マスター ページ内の任意の Web コントロール)、マスター ページ内のラベルを使用するには アクセスします。 ラベルを公開する、マスター ページの分離コード クラスに次の構文を追加`Text`プロパティ。


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample6.cs)]

新しいレコードを追加するときに、`Products`コンテンツ ページからテーブル、`RecentProducts`マスター ページに GridView がその基になるデータ ソースを再バインドする必要があります。 GridView の呼び出しを再バインドするその`DataBind`メソッド。 マスター ページに GridView にコンテンツのページにプログラムでアクセスできないためパブリック メソッドを作成、マスター ページで、呼び出されたときにする必要があります、GridView にデータを再バインドします。 マスター ページの分離コード クラスには、次のメソッドを追加します。


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample7.cs)]

`GridMessageText`プロパティと`RefreshRecentProductsGrid`メソッドの場所で、任意のコンテンツ ページ プログラムで設定したりの値を読み取る、`GridMessage`ラベルの`Text`プロパティにデータを再バインドまたは、 `RecentProducts` GridView。 手順 4 では、コンテンツ ページからマスター ページのパブリック プロパティおよびメソッドにアクセスする方法について説明します。

> [!NOTE]
> マスター ページのプロパティとメソッドをマークすることを忘れないでください`public`します。 かどうかを明示的に表していませんこれらのプロパティとメソッドとして`public`、コンテンツ ページからアクセスはできません。


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>手順 4: は、コンテンツ ページからマスター ページのパブリック メンバーを呼び出す

これらのプロパティとメソッドを呼び出す準備ができましたマスター ページは、必要なパブリック プロパティとメソッドを持つ、これで、`AddProduct.aspx`コンテンツ ページです。 具体的には、マスター ページを設定する必要があります`GridMessageText`プロパティと呼び出しの`RefreshRecentProductsGrid`後に、新しい製品がデータベースに追加されました。 すべての ASP.NET データ Web コントロールの直前にし、ページ開発者はプログラムによるアクションの前に、または後のタスクを実行しやすくさまざまなタスクを完了した後、イベントを発生させます。 たとえば、エンドユーザーは、DetailsView の挿入 ボタンをクリックするでポストバック DetailsView 発生させる、`ItemInserting`挿入のワークフローを開始する前にイベント。 次に、レコードをデータベースに挿入します。 次に、DetailsView を発生させますその`ItemInserted`イベント。 そのため、新しい製品を追加した後、マスター ページを使用するには、DetailsView のイベント ハンドラーを作成`ItemInserted`イベント。

コンテンツ ページがそのマスター ページと対話できますプログラムで 2 つの方法はあります。

- 使用して、`Page.Master`マスター ページへの参照を厳密に型を返すプロパティをまたは
- 使用して、ページのマスター ページの種類またはファイル パスを指定する`@MasterType`ディレクティブ; という名前のページに自動的に厳密に型指定されたプロパティを追加この`Master`します。

どちらの方法を見てみましょう。

### <a name="using-the-loosely-typedpagemasterproperty"></a>厳密に型を使用して`Page.Master`プロパティ

すべての ASP.NET web ページがから派生する必要があります、`Page`クラスにある、`System.Web.UI`名前空間。 `Page`クラスが含まれています、 [ `Master`プロパティ](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx)マスター ページへの参照を返します。 ページがマスター ページを持たない場合`Master`返します`null`します。

`Master`プロパティ型のオブジェクトを返します[ `MasterPage` ](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (も内にある、`System.Web.UI`名前空間) から派生するすべてのマスター ページ元となる基本型です。 パブリック プロパティを使用またはキャストする必要があります、web サイトのマスター ページで定義されているメソッドに、そのため、`MasterPage`から返されたオブジェクト、`Master`プロパティを適切な型。 という名前のマスター ページファイルのため`Site.master`、分離コード クラスの名前が`Site`します。 そのため、次のコードのキャスト、`Page.Master`サイト クラスのインスタンスへのプロパティ。


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample8.cs)]

あるキャストしたので、厳密に型`Page.Master`プロパティを`Site`型のプロパティとメソッドの特定のサイトから参照できます。 図 7 に示す、パブリック プロパティ`GridMessageText`IntelliSense ドロップダウン リストに表示されます。


[![IntelliSense は、マスター ページのパブリック プロパティおよびメソッドを示しています。](interacting-with-the-master-page-from-the-content-page-cs/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image19.png)

**図 07**: IntelliSense は、マスター ページのパブリック プロパティとメソッドを示します ([フルサイズの画像を表示する をクリックします](interacting-with-the-master-page-from-the-content-page-cs/_static/image21.png))。


> [!NOTE]
> マスター ページファイルの名前を付けた場合`MasterPage.master`マスター ページの分離コード クラス名は`MasterPage`します。 これにより、あいまいなコード型からキャストする場合に`System.Web.UI.MasterPage`を`MasterPage`クラス。 つまり、Web サイト プロジェクト モデルを使用する場合は少し困難ですができますが、キャスト先と型を完全に修飾する必要があります。 推奨されるか、マスター ページを作成するときに名前を付けることが何か以外であることを確認する`MasterPage.master`または、さらに、マスター ページへの参照を厳密に型指定を作成します。


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>厳密に型指定された参照を作成する、`@MasterType`ディレクティブ

よく見る場合、ASP.NET ページの分離コード クラスが部分クラスであるを参照できます (注、`partial`クラス定義のキーワード)。 部分クラスでは、c# および Visual Basic.net を使用して Framework 2.0 で導入され、簡単に言うと、複数のファイルに定義するクラスのメンバーでは、します。 分離コード クラス ファイル - `AddProduct.aspx.cs`、たとえば、作成して、ページ開発者は、コードが含まれます。 だけでなく、コードでは、ASP.NET エンジンでは、プロパティと、別のクラス ファイルを自動的に作成し、イベント ハンドラーを宣言型マークアップをページのクラス階層に変換します。

ASP.NET ページにアクセスするたびに発生する自動コード生成の一部ではなく興味深く有用な可能性と続きます。 場合は、マスター ページ、ASP.NET エンジンがどのようなマスター ページは、[コンテンツ] ページで使用されている通知が生成されます、厳密に型指定された`Master`のプロパティ。

使用して、 [ `@MasterType`ディレクティブ](https://msdn.microsoft.com/library/ms228274.aspx)コンテンツ ページのマスター ページの型の ASP.NET エンジンに通知します。 `@MasterType`ディレクティブは、いずれかのマスター ページの型名またはそのファイルのパスを受け入れることができます。 指定する、`AddProduct.aspx`ページ使用`Site.master`としてマスター ページでは、次のディレクティブを追加のページのトップへ`AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample9.aspx)]

このディレクティブをという名前のプロパティを使用してマスター ページへの厳密に型指定された参照を追加する、ASP.NET エンジンに指示`Master`します。 `@MasterType`インプレース ディレクティブを呼び出し、`Site.master`マスター ページのパブリック プロパティとメソッド経由で直接、`Master`任意にキャストしないプロパティ。

> [!NOTE]
> 省略した場合、`@MasterType`ディレクティブ構文では、`Page.Master`と`Master`同じを返します: マスター ページを柔軟に型指定されたオブジェクト。 含める場合は、`@MasterType`ディレクティブ、`Master`指定されたマスター ページへの参照を厳密に型指定されたを返します。 `Page.Master`、ただし、まだ厳密に型の参照を返します。 詳しい理由について、これに該当する方法と、`Master`プロパティを構築時に、`@MasterType`ディレクティブが含まれるを参照してください[K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx)のブログ エントリ[ `@MasterType` ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>新しい製品を追加した後、マスター ページを更新しています

更新する準備ができましたマスター ページのパブリック プロパティおよびコンテンツ ページからメソッドを呼び出す方法がわかっているので、`AddProduct.aspx`ページの新しい製品を追加した後、マスター ページが更新されるようにします。 手順 4 の先頭に DetailsView コントロールのイベント ハンドラーを作成した`ItemInserting`イベントで、新しい製品がデータベースに追加された後すぐに実行します。 イベント ハンドラーに次のコードを追加します。


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample10.cs)]

上記のコードは両方、厳密に型を使用して`Page.Master`プロパティと厳密に型指定`Master`プロパティ。 なお、`GridMessageText`プロパティに設定されて"*ProductName*  グリッドに追加された"だけに追加された製品の値はからアクセスできる、`e.Values`コレクションご覧のとおり、単に追加された;`ProductName`値が使用してアクセス`e.Values["ProductName"]`。

図 8 は、`AddProduct.aspx`データベースに新しい製品 - Scott の Soda - 後すぐにページが追加されました。 そのマスター ページのラベルに追加したばかりの製品名が記載されて、製品の価格など、GridView が更新されたことに注意してください。


[![マスター ページのラベルと GridView が単に追加された製品を表示します。](interacting-with-the-master-page-from-the-content-page-cs/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image22.png)

**図 08**:、マスター ページのラベルと GridView Just-Added 製品を表示する ([フルサイズの画像を表示する をクリックします](interacting-with-the-master-page-from-the-content-page-cs/_static/image24.png))。


## <a name="summary"></a>まとめ

理想的には、マスター ページとそのコンテンツ ページは互いから完全に分離し、操作のレベルは必要ありません。 マスター ページとページのコンテンツは、その目的を念頭に設計する必要がありますは、さまざまな、マスター ページとのコンテンツ ページする必要がありますインターフェイスの一般的なシナリオがあります。 最も一般的な理由の 1 つは、何らかのアクションが [コンテンツ] ページで発生した状況に基づいてマスター ページの表示の特定の部分を更新を中心として展開します。

良い知らせは、コンテンツ ページがそのマスター ページとの対話を比較的簡単であります。 コンテンツ ページによって呼び出される必要のある機能をカプセル化するマスター ページで、パブリック プロパティまたはメソッドを作成して開始します。 次に、コンテンツ ページで、アクセス、マスター ページのプロパティとメソッドを介して、弱い型指定`Page.Master`プロパティまたは使用して、`@MasterType`マスター ページへの参照を厳密に型指定を作成するディレクティブ。

次のチュートリアルでは、マスター ページがそのコンテンツ ページのいずれかのプログラムで対話する方法を考察します。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [アクセスして、ASP.NET のデータの更新](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET マスター ページ: ヒント、テクニック、およびトラップ](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [コンテンツとマスター ページの情報を渡す](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [ASP.NET のチュートリアルでのデータの使用](../../data-access/index.md)

### <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)作成者の複数受け取りますブックと 4GuysFromRolla.com の創設者で、携わって Microsoft Web テクノロジ 1998 年からです。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 3.5 in 24 時間*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)します。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Zack Jones でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](control-id-naming-in-content-pages-cs.md)
> [次へ](interacting-with-the-content-page-from-the-master-page-cs.md)
