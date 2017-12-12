---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
title: "コンテンツ ページ (VB) から、マスター ページと対話する |Microsoft ドキュメント"
author: rick-anderson
description: "メソッドを呼び出し、コンテンツ ページのコードからのマスター ページなどのプロパティを設定する方法を説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 081fe010-ba0f-4e7d-b4ba-774840b601c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 2f5cb1712922c355c99bde9f8252dc84f1f590ec
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="interacting-with-the-master-page-from-the-content-page-vb"></a>コンテンツ ページ (VB) から、マスター ページと対話します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_VB.pdf)

> メソッドを呼び出し、コンテンツ ページのコードからのマスター ページなどのプロパティを設定する方法を説明します。


## <a name="introduction"></a>はじめに

マスター ページを作成する方法を見てきました過去 5 つのチュートリアルの過程で、コンテンツ領域を定義する、マスター ページ、ASP.NET ページにバインドおよびページ固有のコンテンツを定義します。 訪問者は、特定のコンテンツ ページを要求するときに、コンテンツおよびマスター ページのマークアップが実行時に、統一されたコントロールの階層構造の表示の結果として得られる: fused です。 したがって、すでに方法の 1 つをマスター ページとそのコンテンツ ページの 1 つが対話することができます: マークアップをマスター ページの ContentPlaceHolder のコントロールに transfuse を詳しくコンテンツ ページ。

まだを調べるには、マスター ページとコンテンツ ページの対話方法プログラムでします。 だけでなく、マスター ページの ContentPlaceHolder のコントロールのマークアップを定義するには、コンテンツ ページまたそのマスター ページのパブリック プロパティに値を割り当てるし、そのパブリック メソッドを呼び出します。 同様に、マスター ページは、そのコンテンツ ページと対話可能性があります。 Master およびコンテンツのページ間の相互作用をプログラムでは、宣言型マークアップ間の相互作用ほど一般的ですが、このようなプログラムによる操作が必要な多くのシナリオがあります。

このチュートリアルではを調べる方法、コンテンツ ページは、そのマスター ページと対話できますプログラムで次のチュートリアルで取り上げます方法、マスター ページは、そのコンテンツ ページと対話できます同様にします。

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>コンテンツ ページと、マスター ページ間でのプログラムによる操作の例

ページの特定の地域では、ページの単位で構成する必要があります、ときに、ContentPlaceHolder コントロールを使用します。 しかし、についてのページ大部分が特定の出力が少数のページを出力する必要がある場合は、何を表示するカスタマイズする必要があります。 でしょうか。 その一例で調べることが、 [*複数 contentplaceholders にコンテンツを既定と*](multiple-contentplaceholders-and-default-content-vb.md)チュートリアルは、各ページにログイン インターフェイスを表示します。 ほとんどのページには、ログインのインターフェイスを含める必要があります、これを抑制するか、ページのいくつかのなど: メイン ログイン ページ (`Login.aspx`) です。 アカウントの作成 ページの; と他のページには、認証されたユーザーにアクセスできます。 [*複数 contentplaceholders にし、既定のコンテンツ*](multiple-contentplaceholders-and-default-content-vb.md)チュートリアルでは、マスター ページで ContentPlaceHolder の既定のコンテンツを定義する方法を示しましたし、し、場所のページにこのメソッドをオーバーライドする方法、既定のコンテンツが不要です。

別のオプションでは、パブリック プロパティまたはメソッドをログインのインターフェイスを非表示かどうかを示すマスター ページ内に作成します。 たとえば、マスター ページがという名前のパブリック プロパティを含めることが`ShowLoginUI`を設定する値が使用される、`Visible`マスター ページで、ログイン コントロールのプロパティです。 ログイン ユーザー インターフェイスを抑制する必要がそれらのコンテンツ ページをプログラムで設定し、でした、`ShowLoginUI`プロパティを`False`です。

おそらくコンテンツおよびマスター ページの相互作用の最も一般的な例は、何らかのアクションがコンテンツ ページで経過した後に更新するマスター ページ ニーズを満たすデータが表示されるときに発生します。 最近、5 を表示する GridView を含んだマスター ページは、特定のデータベース テーブルからレコードを追加が考慮され、そのコンテンツ ページの 1 つには、その同じテーブルに新しいレコードを追加するためのインターフェイスが含まれています。

ユーザーは、新しいレコードを追加するページにアクセス、最後に追加された 5 つのマスター ページに表示されるレコード彼女が表示されます。 入力後、新しいレコードの列の値に、ユーザーがフォームを送信します。 マスター ページの GridView がある想定されるので、`EnableViewState`プロパティを True (既定値) に設定するは、そのコンテンツが状態の表示から再度読み込まれ、新しいレコードをデータベースに追加された場合でもその結果、同じ 5 つのレコードが表示されます。 これにより、ユーザーが混乱する可能性があります。

> [!NOTE]
> ポストバックのたびに基になるデータ ソースに再度バインドされ、ように GridView のビュー ステートを無効にする場合でも、まだ表示されませんだけで追加されたレコード データがページのライフ サイクル、datab に新しいレコードを追加する場合よりも前の GridView にバインドされているためase です。


単に追加されたレコードがマスター ページに表示されるように、これを修正する GridView のポストバック データ ソースに再バインドする GridView に指示する必要があります*後*新しいレコードがデータベースに追加されました。 新しいレコード (とそのイベント ハンドラー) が、コンテンツ ページですが更新する必要がある GridView に追加するためのインターフェイスがマスター ページであるために、コンテンツおよびマスター ページ間の相互作用が必要です。

コンテンツ ページで、イベント ハンドラーから、マスター ページの表示を更新すると、コンテンツ、マスター ページの相互作用のための最も一般的なニーズの 1 つはため、このトピックで詳しく見てみましょう。 このチュートリアルのダウンロードには、という名前の Microsoft SQL Server 2005 Express Edition データベースが含まれている`NORTHWIND.MDF`、web サイトの `App_Data`フォルダーです。 Northwind データベースは、製品、従業員、および、Northwind traders 社という架空の企業の売上情報を格納します。

手順 1、5 つの最も最近表示ではについて説明は、マスター ページの GridView で製品を追加します。 手順 2 では、新しい製品の追加のコンテンツ ページを作成します。 手順 3 は、マスター ページで、パブリック プロパティとメソッドを作成する方法を確認し、手順 4 がプログラムによってこれらのプロパティとコンテンツのページからのメソッドとやり取りする方法を示します。

> [!NOTE]
> このチュートリアルは、ASP.NET でデータの操作の詳細を掘り下げてはされません。 データとデータを挿入するコンテンツ ページを表示するマスター ページを設定するための手順には、完了、まだ breezy です。 表示しデータを挿入して、SqlDataSource および GridView コントロールを使用に関する詳細については、このチュートリアルの最後に、これ以上の測定値セクション内のリソースを参照してください。


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>手順 1: はマスター ページで製品を追加する最も最近、5 を表示します。

Site.master マスター ページを開き、ラベルと GridView コントロールを追加、 `leftContent` `<div>`です。 ラベルのチェック ボックスをオフに`Text`プロパティを設定、`EnableViewState`プロパティを`False`、およびその`ID`プロパティを`GridMessage`; GridView の設定`ID`プロパティを`RecentProducts`です。 次に、デザイナー、GridView のスマート タグを展開し、新しいデータ ソースにバインドを選択します。 これは、データ ソース構成ウィザードを起動します。 Northwind データベースに格納されているため、`App_Data`フォルダーは、Microsoft SQL Server データベースを (図 1 を参照してください) を選択すると、SqlDataSource を作成するを選択します。 名前を、SqlDataSource`RecentProductsDataSource`です。


[![GridView を RecentProductsDataSource をという名前の SqlDataSource コントロールにバインドします。](interacting-with-the-master-page-from-the-content-page-vb/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image1.png)

**図 01**: GridView を名前付き SqlDataSource コントロールにバインド`RecentProductsDataSource`([フルサイズのイメージを表示するをクリックして](interacting-with-the-master-page-from-the-content-page-vb/_static/image3.png))


次の手順では、データベースに接続する新機能を指定するよう求められます。 選択、`NORTHWIND.MDF`ドロップダウン リストからファイルをデータベースし、[次へ] をクリックします。 内の接続文字列を格納するウィザードが、これはこのデータベースを使用して最初の時間であるため、`Web.config`です。 名前を使用して、接続文字列を格納することがある`NorthwindConnectionString`です。


[![Northwind データベースへの接続します。](interacting-with-the-master-page-from-the-content-page-vb/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image4.png)

**図 02**: Northwind データベースへの接続 ([フルサイズのイメージを表示するをクリックして](interacting-with-the-master-page-from-the-content-page-vb/_static/image6.png))


データ ソース構成ウィザードでは、データを取得するためのクエリを指定できます 2 つの手段を提供します。

- カスタム SQL ステートメントまたはストアド プロシージャを指定することによって、または
- テーブルまたはビューを選択し、返される列を指定

ために最後に追加されただけ 5 つの製品を返す、カスタムの SQL ステートメントを指定する必要があります。 以下を使用して`SELECT`クエリ。


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample1.sql)]

`TOP 5`キーワードは、クエリから最初の 5 つのレコードだけを返します。 `Products`テーブルの主キー、`ProductID`は、`IDENTITY`列で、テーブルに追加された新しい各製品が前のエントリより大きい値を持つことを保証します。 したがって、並べ替え結果を`ProductID`降順で始まる最近作成した製品を返します。


[![最近追加された 5 つの製品を返す](interacting-with-the-master-page-from-the-content-page-vb/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image7.png)

**図 03**: 5 つ最も最近追加の製品を返す ([フルサイズのイメージを表示するをクリックして](interacting-with-the-master-page-from-the-content-page-vb/_static/image9.png))


ウィザードを完了すると、Visual Studio に表示する GridView の 2 つの BoundFields が生成されます、`ProductName`と`UnitPrice`データベースからのフィールドが返されます。 この時点で、マスター ページの宣言型マークアップでは、次のようなマークアップを含める必要があります。


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample2.aspx)]

マークアップを含むことがわかります: Label Web コントロール (`GridMessage`); GridView `RecentProducts`、2 つの BoundFields; と最後に追加された 5 つの製品を返す SqlDataSource コントロールを使用します。

この GridView の作成と構成、SqlDataSource コントロール、ブラウザーから web サイトを参照してください。 図 4 では、左下隅に最近 5 つの一覧を表示するグリッドに追加された製品が表示されます。


[![GridView には、5 つの最後に追加された製品が表示されます。](interacting-with-the-master-page-from-the-content-page-vb/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image10.png)

**図 04**: 5 つ最も最近追加の製品を表示する GridView ([フルサイズのイメージを表示するをクリックして](interacting-with-the-master-page-from-the-content-page-vb/_static/image12.png))


> [!NOTE]
> 自由 GridView の外観をクリーンアップしてください。 いくつかの提案が表示されている書式設定を含める`UnitPrice`通貨とグリッドの外観を向上させるために背景色とフォントを使用する値。


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>手順 2: 新しい製品を追加するコンテンツ ページの作成

ユーザーが新しい製品を追加できるコンテンツ ページを作成するが、次のタスク、`Products`テーブル。 新しいコンテンツ ページを追加、`Admin`という名前のフォルダー `AddProduct.aspx`、バインドすることを確認して、`Site.master`マスター ページ。 図 5 は、このページは、web サイトに追加した後に、ソリューション エクスプ ローラーを示します。


[![Admin フォルダーに新しい ASP.NET ページを追加します。](interacting-with-the-master-page-from-the-content-page-vb/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image13.png)

**図 05**: に新しい ASP.NET ページの追加、`Admin`フォルダー ([フルサイズのイメージを表示するをクリックして](interacting-with-the-master-page-from-the-content-page-vb/_static/image15.png))


注意してください、 [*マスター ページのタイトル、メタ タグ、およびその他の HTML ヘッダーを指定する*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)という名前のカスタム ベース ページ クラスを作成したチュートリアル`BasePage`が場合、ページのタイトルを生成します。明示的に設定します。 移動して、`AddProduct.aspx`ページの分離コード クラスをから派生して`BasePage`(の代わりにから`System.Web.UI.Page`)。

最後に、更新、`Web.sitemap`このレッスンのエントリを追加するファイル。 下に次のマークアップを追加、`<siteMapNode>`制御 ID の名前付けに関する問題のレッスン。


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample3.xml)]

図 6、これを追加で示すように`<siteMapNode>`レッスン リストに要素が反映されます。

戻る`AddProduct.aspx`です。 コンテンツ コントロールで、 `MainContent` ContentPlaceHolder、DetailsView コントロールを追加し、名前`NewProduct`です。 DetailsView をという名前の新しい SqlDataSource コントロールにバインド`NewProductDataSource`です。 などの手順 1. で SqlDataSource、による Northwind データベースを使用するように、ウィザードを構成してカスタム SQL ステートメントを指定するを選択します。 両方を指定する必要があります、データベースにアイテムを追加する DetailsView を使用するため、`SELECT`ステートメントおよび`INSERT`ステートメントです。 以下を使用して`SELECT`クエリ。


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample4.sql)]

次に、[挿入] タブから、次の追加`INSERT`ステートメント。


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample5.sql)]

ウィザードの完了後 DetailsView のスマート タグに移動し、「を有効にするを挿入する」のチェック ボックスを確認します。 指定された DetailsView は CommandField に追加の`ShowInsertButton`プロパティを True に設定します。 この DetailsView がデータを挿入するためだけに使用される、ために設定 DetailsView の`DefaultMode`プロパティを`Insert`です。

すべてであることには! このページをテストしてみましょう。 参照してください`AddProduct.aspx`ブラウザーを使って (図 6 を参照してください)、名前と価格を入力します。


[![データベースに新しい製品を追加します。](interacting-with-the-master-page-from-the-content-page-vb/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image16.png)

**図 06**: データベースに新しい製品を追加 ([フルサイズのイメージを表示するをクリックして](interacting-with-the-master-page-from-the-content-page-vb/_static/image18.png))


新しい製品の価格と名前を入力すると、[挿入] ボタンをクリックします。 これにより、フォームのポストバックをします。 ポストバック時に、SqlDataSource コントロールの`INSERT`ステートメントを実行する以外の場合は、2 つのパラメーターが DetailsView の 2 つのテキスト ボックス コントロールでのユーザーが入力した値に設定されます。 残念ながら、視覚的なフィードバック挿入が発生したことはありません。 メッセージが表示するには、新しいレコードが追加されていることを確認するがよいでしょう。 私のままに練習として、リーダーの。 また、DetailsView から新しいレコードを追加した後、マスター ページの GridView まだを示しています、同じ 5 つのレコードとして; の前に単に追加されたレコードは含まれません。 以降の手順でこの問題を解決する方法について確認します。

> [!NOTE]
> 何らかの形式の視覚的なフィードバック、挿入が成功したことを追加するだけでなくことをお勧めもインターフェイスを更新 DetailsView の挿入検証を追加します。 現時点では、検証は行われません。 ユーザーに対して無効な値を入力した場合、`UnitPrice`などのフィールド"が多すぎます高価な"システムがその文字列を 10 進数に変換しようとするとポストバック時に、例外がスローされます。 詳細については、挿入のカスタマイズでのインターフェイスを参照してください、 [*データ変更インターフェイスのカスタマイズ*チュートリアル](https://asp.net/learn/data-access/tutorial-20-vb.aspx)から my[データ チュートリアル シリーズの操作](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>手順 3: マスター ページ内のパブリック プロパティとメソッドの作成

手順 1 でという名前のラベルの Web コントロールを追加しました`GridMessage`マスター ページの GridView 上。 このラベルは必要に応じてメッセージを表示するためのものです。 新しいレコードを追加した後など、`Products`テーブル、必要になるようなメッセージを表示する:"*ProductName*がデータベースに追加されました"。 ハード コード マスター ページでは、このラベルのテキストではなく、メッセージ コンテンツ ページでカスタマイズできる必要になります。

Label コントロールがマスター ページ内でプロテクト メンバー変数として実装されているために、コンテンツ ページから直接アクセスできません。 コンテンツ ページから (または、さらに言えば、マスター ページ内の任意の Web コントロールの)、マスター ページ内のラベルを使用するために必要がありますが、Web コントロールを公開または、そのプロパティのいずれかを指定できますプロキシとして機能するマスター ページのパブリック プロパティを作成するには アクセスします。 ラベルの公開するのには、マスター ページの分離コード クラスに次の構文を追加`Text`プロパティ。


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample6.vb)]

新しいレコードを追加するときに、`Products`コンテンツ ページからのテーブル、 `RecentProducts` GridView のマスター ページには、基になるデータ ソースにバインドする必要があります。 GridView の呼び出しを再バインドする、`DataBind`メソッドです。 GridView のマスター ページにアクセスできないため、プログラムでコンテンツのページに、におパブリック メソッドを作成、マスター ページで、呼び出されたときにする必要があります、GridView にデータをバインドします。 マスター ページの分離コード クラスに次のメソッドを追加します。


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample7.vb)]

`GridMessageText`プロパティおよび`RefreshRecentProductsGrid`メソッド インプレース、任意のコンテンツ ページできるプログラムで設定またはの値を読み取る、`GridMessage`ラベルの`Text`プロパティにデータをバインドし直すか、 `RecentProducts` GridView です。 手順 4 は、コンテンツ ページから、マスター ページのパブリック プロパティおよびメソッドにアクセスする方法を説明します。

> [!NOTE]
> 忘れずに、マスター ページのプロパティとメソッドとしてマークを付ける`Public`です。 かどうか、明示的に表しませんこれらのプロパティおよびメソッドとして`Public`、コンテンツ ページからアクセスはできません。


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>手順 4: は、コンテンツ ページから、マスター ページのパブリック メンバーを呼び出す

これらのプロパティとメソッドを呼び出す準備ができましたマスター ページには、必要なパブリック プロパティとメソッドがあります、これで、`AddProduct.aspx`コンテンツ ページ。 具体的には、マスター ページの設定をする必要があります`GridMessageText`プロパティと呼び出しの`RefreshRecentProductsGrid`新しい製品がデータベースに追加された後にします。 すべての ASP.NET Web コントロールのデータでは、イベントを発生させる直前と直後に、簡単にプログラムで操作する前に、またはタスクの後にページの開発者向けのさまざまなタスクを完了します。 たとえば、エンドユーザーには、DetailsView の挿入 ボタンがクリックすると、上のポストバック DetailsView を発生させますその`ItemInserting`イベント挿入のワークフローを開始する前にします。 次に、レコードをデータベースに挿入します。 DetailsView を発生させる次に、その`ItemInserted`イベント。 したがって、新しい製品を追加した後、マスター ページを使用するために detailsview のイベント ハンドラーを作成`ItemInserted`イベント。

ストアには、コンテンツ ページが、マスター ページとやり取りしてプログラムで 2 つの方法があります。

- 使用して、`Page.Master`マスター ページへの参照を厳密に型を返すプロパティまたは
- 使用して、ページのマスター ページの種類またはファイル パスを指定、`@MasterType`ディレクティブ; という名前のページに自動的に厳密に型指定されたプロパティを追加この`Master`です。

両方の方法を調べてみましょう。

### <a name="using-the-loosely-typedpagemasterproperty"></a>厳密に型を使用して`Page.Master`プロパティ

すべての ASP.NET web ページがから派生する必要があります、`Page`クラスにある、`System.Web.UI`名前空間。 `Page`クラスが含まれています、 [ `Master`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.page.master.aspx)マスター ページへの参照を返します。 マスター ページがページにない場合`Master`返します`Nothing`です。

`Master`プロパティ型のオブジェクトを返します[ `MasterPage` ](https://msdn.microsoft.com/en-us/library/system.web.ui.masterpage.aspx) (にも格納されて、`System.Web.UI`名前空間) はすべてのマスター ページから派生する基本型です。 パブリック プロパティを使用またはおキャストする必要があります、web サイトのマスター ページで定義されているメソッドにそのため、`MasterPage`から返されたオブジェクト、`Master`プロパティを適切な型です。 マスター ページファイルという名前を付けてため`Site.master`、分離コード クラスの名前が`Site`です。 したがって、次のコードのキャスト、`Page.Master`プロパティのインスタンスを`Site`クラスです。


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample8.vb)]

キャストした今、弱い型指定`Page.Master`プロパティ、サイトの種類にプロパティとメソッドの特定のサイトを参照できます。 図 7 では、そのパブリック プロパティ`GridMessageText`IntelliSense のドロップダウン リストに表示されます。


[![IntelliSense は、マスター ページのパブリック プロパティとメソッドを示しています。](interacting-with-the-master-page-from-the-content-page-vb/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image19.png)

**図 07**: IntelliSense は、マスター ページのパブリック プロパティとメソッドを示します ([フルサイズのイメージを表示するをクリックして](interacting-with-the-master-page-from-the-content-page-vb/_static/image21.png))


> [!NOTE]
> マスター ページファイルという名前を付けた場合`MasterPage.master`マスター ページの分離コード クラス名は`MasterPage`します。 これにより、あいまいなコードの型からキャストする場合に`System.Web.UI.MasterPage`を`MasterPage`クラスです。 つまり、できるにくい Web サイト プロジェクトのモデルを使用する場合に、キャスト、型を完全に修飾する必要があります。 推奨されること、マスター ページを作成するときに名前を付けるもの以外のことを確認するか、`MasterPage.master`または、さらに、マスター ページへの厳密に型指定された参照を作成します。


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>厳密に型指定された参照を作成する、`@MasterType`ディレクティブ

詳しく調べる場合、ASP.NET ページの分離コード クラスが部分クラスを確認できます (注、`Partial`クラス定義のキーワード) です。 部分クラスでは、c# および Visual Basic 使用 Framework 2.0 で導入され、簡単に言うと、複数のファイルに定義するクラスのメンバーでは、します。 分離コード クラス ファイル - `AddProduct.aspx.vb`、たとえば、開発者は、ページを作成するコードが含まれます。 コードだけでなく、ASP.NET エンジンでは、プロパティを持つ、別のクラス ファイルを自動的に作成しを内のイベント ハンドラーが、宣言型マークアップをページのクラス階層構造に変換します。

ASP.NET ページにアクセスするたびに発生する自動コード生成、比較的一部ではなく興味深く有用な選択肢です。 マスター ページの場合、コンテンツ ページがどのようなマスター ページを使用している ASP.NET エンジン通知が生成されます、厳密に型指定された`Master`支払えばプロパティです。

使用して、 [ `@MasterType`ディレクティブ](https://msdn.microsoft.com/en-us/library/ms228274.aspx)コンテンツ ページのマスター ページの種類の ASP.NET エンジンに通知します。 `@MasterType`ディレクティブは、マスター ページの型名か、そのファイルのパスを受け入れることができます。 指定する、`AddProduct.aspx`ページ使用`Site.master`として、マスター ページの先頭に次のディレクティブを追加`AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample9.aspx)]

このディレクティブは、という名前のプロパティをマスター ページへの厳密に型指定された参照を追加する ASP.NET エンジンを指示`Master`です。 `@MasterType`インプレース ディレクティブ、お呼び出すことができます、`Site.master`マスター ページのパブリック プロパティとメソッド経由で直接、`Master`任意にキャストしないプロパティです。

> [!NOTE]
> 省略した場合、`@MasterType`構文では、ディレクティブ`Page.Master`と`Master`同じものを返す: マスター ページへの弱い型指定のオブジェクト。 含める場合は、`@MasterType`ディレクティブ、`Master`指定のマスター ページへの厳密に型指定された参照を返します。 `Page.Master`、ただし、厳密に型の参照をまだを返します。 理由をより詳しく見るには、ケース方法、および`Master`プロパティが構築されるときに、`@MasterType`ディレクティブが含まれるを参照してください[K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx)のブログ エントリ[ `@MasterType` ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>新しい製品を追加した後、マスター ページを更新

更新する準備ができましたマスター ページのパブリック プロパティとコンテンツ ページからメソッドを呼び出す方法がわかっているので、`AddProduct.aspx`ページに新しい製品を追加した後、マスター ページを更新できるようにします。 手順 4 の先頭の DetailsView コントロールのイベント ハンドラーを作成した`ItemInserting`イベントで、新しい製品がデータベースに追加された後にすぐに実行します。 イベント ハンドラーに次のコードを追加します。


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample10.vb)]

上記のコードは、両方の厳密に型を使用して`Page.Master`プロパティと、厳密に型指定された`Master`プロパティです。 なお、`GridMessageText`プロパティに設定されている"*ProductName*  グリッドに追加された"単に追加された製品の値は、経由でアクセスできる、`e.Values`コレクション以外の場合は、単に追加された、参照できるよう`ProductName`値が経由でアクセスされる`e.Values("ProductName")`です。

図 8 は、`AddProduct.aspx`データベースに新しい製品 - Scott の Soda - 後すぐにページが追加されました。 だけで追加された製品名が、マスター ページのラベルに示されていると、GridView が製品との価格を含めるように更新されたことに注意してください。


[![マスター ページのラベルと GridView は、単に追加された製品を表示します。](interacting-with-the-master-page-from-the-content-page-vb/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image22.png)

**図 08**: Just-Added 製品を表示する、マスター ページのラベルと GridView ([フルサイズのイメージを表示するをクリックして](interacting-with-the-master-page-from-the-content-page-vb/_static/image24.png))


## <a name="summary"></a>概要

理想的には、マスター ページとそのコンテンツ ページ互いから完全に分離し、の相互作用のレベルは必要ありません。 マスター ページとページのコンテンツは、その目的を念頭にデザインする必要があります、そのマスター ページとのコンテンツ ページする必要がありますインターフェイスの一般的なシナリオの数があります。 コンテンツ ページで発生した状況をいくつかの操作に基づくマスター ページの表示の特定の部分の更新を中心に最も一般的な理由の 1 つです。

良いニュースは、コンテンツ ページに、マスター ページとの対話を比較的簡単であります。 コンテンツ ページによって呼び出される必要がある機能をカプセル化するマスター ページで、パブリック プロパティまたはメソッドを作成して開始します。 次に、コンテンツ ページで、アクセス、マスター ページのプロパティとメソッドを介して、弱い型指定`Page.Master`プロパティまたはを使用して、`@MasterType`マスター ページへの厳密に型指定された参照を作成するディレクティブ。

次のチュートリアルでは、マスター ページをプログラムでそのコンテンツ ページの 1 つと対話する方法を確認します。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [アクセスして、ASP.NET でのデータの更新](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET マスター ページ: ヒント、テクニック、およびトラップ](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType`ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [コンテンツおよびマスター ページの間で情報を渡す.](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [ASP.NET のチュートリアルでのデータの使用](../../data-access/index.md)

### <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、作成者複数受け取ります書籍や 4GuysFromRolla.com の創設者を操作した Microsoft Web テクノロジ 1998 年です。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 3.5 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)です。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログでを介して[http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Zack Jones しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](control-id-naming-in-content-pages-vb.md)
[次へ](interacting-with-the-content-page-from-the-master-page-vb.md)
