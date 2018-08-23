---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: マスター/詳細の 2 つのページ (c#) 間のフィルター処理 |Microsoft Docs
author: rick-anderson
description: このチュートリアルではこのパターンを仕入先データベースを一覧表示する GridView を使用して実装します。 GridView のサプライヤーの各行には、Vie が含まれます.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 69e5f010507784229360f71cf6f570b342f5ff46
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835135"
---
<a name="masterdetail-filtering-across-two-pages-c"></a>マスター/詳細の 2 つのページ (c#) 間のフィルター処理
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe)または[PDF のダウンロード](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> このチュートリアルではこのパターンを仕入先データベースを一覧表示する GridView を使用して実装します。 GridView のサプライヤーの各行は、クリックされると、別のページにユーザーがかかります。 場合が一覧表示されるこれらの製品の仕入先を選択した製品の表示のリンクが格納されます。


## <a name="introduction"></a>はじめに

前の 2 つのチュートリアルで見た方法[Dropdownlist を使用して単一の web ページでマスター/詳細レポートを表示](master-detail-filtering-with-a-dropdownlist-cs.md)に[「マスター」のレコードと GridView、DetailsView コントロールを表示](master-detail-filtering-with-two-dropdownlists-cs.md)を表示する、"詳細情報。" マスター/詳細レポートに使用されるもう 1 つの一般的なパターンでは、1 つの web ページと別に表示される詳細にマスター レコードがあります。 フォーラムの web サイトと同様に、 [ASP.NET フォーラム](https://forums.asp.net/)、実際には、このパターンの優れた例です。 ASP.NET フォーラムでは、さまざまなフォーラムをはじめ、Web フォーム、データ プレゼンテーション コントロールで構成されてし、具合です。 各フォーラムがの多数のスレッドで構成され、各スレッドは、投稿の数で構成されます。 ASP.NET フォーラムのホーム ページにフォーラムの一覧が表示されます。 移動するフォーラムをクリックすると、`ShowForum.aspx`ページで、そのフォーラムのスレッドが一覧表示します。 同様に、クリックすると、スレッドの移動を`ShowPost.aspx`、クリックされたスレッドの投稿が表示されます。

このチュートリアルではこのパターンを仕入先データベースを一覧表示する GridView を使用して実装します。 GridView のサプライヤーの各行は、クリックされると、別のページにユーザーがかかります。 場合が一覧表示されるこれらの製品の仕入先を選択した製品の表示のリンクが格納されます。

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>手順 1: 追加`SupplierListMaster.aspx`と`ProductsForSupplierDetails.aspx`ページを`Filtering`フォルダー

3 番目のチュートリアルでは、ページ レイアウトを定義するときに"starter"ページ数を追加しました、 `BasicReporting`、 `Filtering`、および`CustomFormatting`フォルダー。 ただし、その時点でこのチュートリアルでは、スタート ページを追加していませんに 2 つの新しいページを追加するため少し、`Filtering`フォルダー:`SupplierListMaster.aspx`と`ProductsForSupplierDetails.aspx`します。 `SupplierListMaster.aspx` 中に「マスター」のレコード (仕入先) ボックスの一覧は`ProductsForSupplierDetails.aspx`選択されている業者の製品が表示されます。

ときにこれら 2 つの新しいページを作成することでそれらを関連付ける、`Site.master`マスター ページ。


![SupplierListMaster.aspx と ProductsForSupplierDetails.aspx ページをフィルター処理のフォルダーに追加します。](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**図 1**: 追加、`SupplierListMaster.aspx`と`ProductsForSupplierDetails.aspx`ページを`Filtering`フォルダー


をプロジェクトに新しいページを追加する場合の、サイト マップ ファイルを更新することを確認しても、する`Web.sitemap`、それに応じて。 このチュートリアルでは単に追加、`SupplierListMaster.aspx`ページをフィルター処理のレポートの子として次の XML コンテンツを使用して、サイト マップ`<siteMapNode>`要素。


[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> 使用してページで新しい ASP.NET を追加するときに、サイト マップ ファイルの更新プロセスの自動化に役立つできます[K. Scott Allen](http://odetocode.com/Blogs/scott/)Visual Studio を無料 's[サイト マップ マクロ](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx)します。


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>手順 2: で仕入先の一覧を表示します。`SupplierListMaster.aspx`

`SupplierListMaster.aspx`と`ProductsForSupplierDetails.aspx`仕入先の GridView を作成するページが作成された、次の手順は`SupplierListMaster.aspx`します。 GridView をページに追加し、新しい ObjectDataSource にバインドします。 この ObjectDataSource を使用する必要があります、`SuppliersBLL`クラスの`GetSuppliers()`all を返します。


[![SuppliersBLL クラスを選択します。](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**図 2**: 選択、`SuppliersBLL`クラス ([フルサイズの画像を表示する をクリックします](master-detail-filtering-across-two-pages-cs/_static/image4.png))。


[![ObjectDataSource GetSuppliers() メソッドを使用して構成します。](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**図 3**: 構成に使用する ObjectDataSource、`GetSuppliers()`メソッド ([フルサイズの画像を表示する をクリックします](master-detail-filtering-across-two-pages-cs/_static/image7.png))。


リンクを含める必要があります「View Products GridView 行ごとにクリックするユーザーには、`ProductsForSupplierDetails.aspx`選択した行を渡して`SupplierID`、クエリ文字列を使用して値。 たとえば、ユーザーが東京 Traders 業者の製品の表示リンクをクリックする (を持つ、 `SupplierID` 4 の値) に送信される`ProductsForSupplierDetails.aspx?SupplierID=4`。

これを行うには、追加、[内](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx)GridView を GridView の行ごとにハイパーリンクを追加します。 GridView のスマート タグからの列の編集リンクをクリックして開始します。 次に、左上の一覧から、内を選択し、追加、内を GridView のフィールドの一覧に含める をクリックします。


[![GridView に内を追加します。](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**図 4**: GridView を内の追加 ([フルサイズの画像を表示する をクリックします](master-detail-filtering-across-two-pages-cs/_static/image10.png))。


内は、同じテキストを使用するように構成できますか URL で GridView 行ごとに、リンクの値または特定の行ごとにバインドされているデータ値にこれらの値を基本ことができます。 すべての行の値を静的なを指定するには、内を使用して`Text`または`NavigateUrl`プロパティ。 すべての行に対して同一にするリンク テキスト、たいので設定内の`Text`製品を表示するプロパティ。


[![製品の表示内の Text プロパティを設定します。](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**図 5**: 設定内の`Text`製品を表示するプロパティ ([フルサイズの画像を表示する をクリックします](master-detail-filtering-across-two-pages-cs/_static/image13.png))。


テキストまたは GridView 行にバインドされている基になるデータに基づく URL 値を設定するデータ フィールド、テキストまたはから URL 値を取得する必要がありますを指定、`DataTextField`または`DataNavigateUrlFields`プロパティ。 `DataTextField` 1 つのデータ フィールドにのみ設定できます。`DataNavigateUrlFields`、ただし、データ フィールドのコンマ区切りのリストを設定することができます。 頻繁に、テキストや、現在の行のデータ フィールドの値といくつかの静的マークアップの組み合わせに URL のベースにする必要があります。 このチュートリアルでは、たとえば、します内のリンクの URL を`ProductsForSupplierDetails.aspx?SupplierID=supplierID`ここで、 *`supplierID`* は各 GridView の行の`SupplierID`値。 静的にする必要がありますをデータに基づく値をここに注意してください:`ProductsForSupplierDetails.aspx?SupplierID=`リンクの URL の部分は静的では、 *`supplierID`* 部分がデータに基づくようにその値が行ごとの独自`SupplierID`値。

静的およびデータ ドリブンの値の組み合わせを示すために使用して、`DataTextFormatString`と`DataNavigateUrlFormatString`プロパティ。 これらのプロパティで必要に応じて静的マークアップを入力し、マーカーを使用して`{0}`で指定されたフィールドの値を取得する、`DataTextField`または`DataNavigateUrlFields`プロパティを表示します。 場合、`DataNavigateUrlFields`プロパティが複数のフィールドの指定した使用`{0}`挿入されると、最初のフィールド値を表示する場所`{1}`2 番目のフィールドの値を示す場合。

これをチュートリアルを適用して、私たちは、設定する必要があります、`DataNavigateUrlFields`プロパティを`SupplierID`データ フィールドの値の行ごとにカスタマイズする必要がありますので、および`DataNavigateUrlFormatString`プロパティを`ProductsForSupplierDetails.aspx?SupplierID={0}`します。


[![SupplierID に基づいて適切なリンクの url 内を構成します。](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**図 6**: に、適切なリンク URL ベースの時に含める内の構成、 `SupplierID` ([フルサイズの画像を表示する をクリックします](master-detail-filtering-across-two-pages-cs/_static/image16.png))。


内を追加すると、自由にカスタマイズおよび GridView のフィールドの順序を変更できます。 次のマークアップは、いくつかの小規模なフィールド レベルのカスタマイズを行った後に、GridView を示します。


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

表示する少し、`SupplierListMaster.aspx`ページがブラウザーを使用します。 図 7 に示すよう、ページ現在すべての製品の表示のリンクを含む仕入先を紹介します。 製品の表示をクリックするとリンクをクリックすると、`ProductsForSupplierDetails.aspx`業者に沿ったを渡して、`SupplierID`クエリ文字列。


[![各仕入先の行には、ビューの製品リンクが含まれています。](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**図 7**: 供給業者の各行には、ビューの製品リンクが含まれています ([フルサイズの画像を表示する をクリックします](master-detail-filtering-across-two-pages-cs/_static/image19.png))。


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>手順 3: 供給業者の製品を一覧表示します。`ProductsForSupplierDetails.aspx`

この時点で、`SupplierListMaster.aspx`ページがユーザーに送信して`ProductsForSupplierDetails.aspx`、選択した業者を渡す`SupplierID`クエリ文字列。 チュートリアルの最後の手順での GridView に製品を表示する`ProductsForSupplierDetails.aspx`が`SupplierID`equals、`SupplierID`クエリ文字列が渡されます。 GridView を追加することで、このスタートを実行する、`ProductsForSupplierDetails.aspx`という名前の新しい ObjectDataSource コントロールを使用して、ページ`ProductsBySupplierDataSource`を呼び出す、`GetProductsBySupplierID(supplierID)`からメソッド、`ProductsBLL`クラス。


[![ProductsBySupplierDataSource という名前の新しい ObjectDataSource を追加します。](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**図 8**: 新しい ObjectDataSource という追加`ProductsBySupplierDataSource`([フルサイズの画像を表示する をクリックします](master-detail-filtering-across-two-pages-cs/_static/image22.png))。


[![ProductsBLL クラスを選択します。](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**図 9**: 選択、`ProductsBLL`クラス ([フルサイズの画像を表示する をクリックします](master-detail-filtering-across-two-pages-cs/_static/image25.png))。


[![ObjectDataSource GetProductsBySupplierID(supplierID) メソッドの呼び出しがあります。](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**図 10**: ObjectDataSource 呼び出しがある、`GetProductsBySupplierID(supplierID)`メソッド ([フルサイズの画像を表示する をクリックします](master-detail-filtering-across-two-pages-cs/_static/image28.png))。


データ ソース構成ウィザードの最後の手順では、ソースを提供するよう求められます、`GetProductsBySupplierID(supplierID)`メソッドの*`supplierID`* パラメーター。 クエリ文字列値を使用するパラメーターのソースをクエリ文字列に設定し、QueryStringField ボックスに、使用するクエリ文字列値の名前を入力します (`SupplierID`)。


[![SupplierID SupplierID クエリ文字列値からパラメーター値の設定します。](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**図 11**: 設定、 *`supplierID`* からパラメーター値、`SupplierID`クエリ文字列値 ([フルサイズの画像を表示する をクリックします](master-detail-filtering-across-two-pages-cs/_static/image31.png))。


すべてです。 図 12 は、`ProductsForSupplierDetails.aspx`から東京 Traders リンクをクリックしてアクセスしたときにページ`SupplierListMaster.aspx`します。


[![東京 Traders によって提供される製品が表示されます。](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**図 12**: 東京 Traders によって提供される、製品が表示されます ([フルサイズの画像を表示する をクリックします](master-detail-filtering-across-two-pages-cs/_static/image34.png))。


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>仕入先情報を表示します。`ProductsForSupplierDetails.aspx`

図 12 に示すよう、`ProductsForSupplierDetails.aspx`ページによって提供される製品を単純に一覧表示、`SupplierID`クエリ文字列で指定します。 このページに直接送信ただしはわからない図 12 に東京のトレーダーの製品が表示されています。 これを修正するもこのページで仕入先情報を表示することができます。

まず上記製品 GridView、FormView を追加します。 という名前の新しい ObjectDataSource コントロールを作成`SuppliersDataSource`を呼び出す、`SuppliersBLL`クラスの`GetSupplierBySupplierID(supplierID)`メソッド。


[![SuppliersBLL クラスを選択します。](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**図 13**: 選択、`SuppliersBLL`クラス ([フルサイズの画像を表示する をクリックします](master-detail-filtering-across-two-pages-cs/_static/image37.png))。


[![ObjectDataSource GetSupplierBySupplierID(supplierID) メソッドの呼び出しがあります。](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**図 14**: ObjectDataSource 呼び出しがある、`GetSupplierBySupplierID(supplierID)`メソッド ([フルサイズの画像を表示する をクリックします](master-detail-filtering-across-two-pages-cs/_static/image40.png))。


同様、`ProductsBySupplierDataSource`が、 *`supplierID`* パラメーターの値が割り当て、`SupplierID`クエリ文字列値。


[![SupplierID SupplierID クエリ文字列値からパラメーター値の設定します。](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**図 15**: 設定、 *`supplierID`* からパラメーター値、`SupplierID`クエリ文字列値 ([フルサイズの画像を表示する をクリックします](master-detail-filtering-across-two-pages-cs/_static/image43.png))。


FormView のデザイン ビューで ObjectDataSource をフォーム ビューをバインドするときに、自動的に Visual Studio を作成`ItemTemplate`、 `InsertItemTemplate`、および`EditItemTemplate`の各によって返されるデータ フィールドのラベルとテキスト ボックスに Web コントロールと、ObjectDataSource します。 仕入先情報を自由に削除を表示するだけですので、`InsertItemTemplate`と`EditItemTemplate`します。 次に、仕入先の会社名を表示するよう、ItemTemplate の編集、`<h3>`要素と、アドレス、市区町村、国、および会社名の下に電話番号。 FormView を手動で設定する代わりに、`DataSourceID`を作成し、 `ItemTemplate` 、マークアップで行ったように、"[、ObjectDataSource でデータを表示する](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)"チュートリアル。

これらの編集後 FormView の宣言型マークアップは次のようになります。


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

図 16 のスクリーン ショットを示しています、`ProductsForSupplierDetails.aspx`ページ上で詳述仕入先の情報が含まれています。


[![製品の一覧には、供給業者に関する概要が含まれています。](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**図 16**: 製品の一覧には、サプライヤーに関する概要が含まれています ([フルサイズの画像を表示する をクリックします](master-detail-filtering-across-two-pages-cs/_static/image46.png))。


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>接触、最後の適用、`ProductsForSupplierDetails.aspx`UI

ユーザーを向上させるためにこのレポートに存在するためのエクスペリエンスのいくつか追加することですね、`ProductsForSupplierDetails.aspx`ページ。 現在、唯一の方法から参照できます、`ProductsForSupplierDetails.aspx`仕入先の一覧に戻るページは、ブラウザーの戻るボタンをクリックします。 ハイパーリンク コントロールを追加してみましょう、`ProductsForSupplierDetails.aspx`にリンク バック ページ`SupplierListMaster.aspx`、マスター リストに戻るユーザーの別の方法を提供します。


[![SupplierListMaster.aspx のために、ユーザーがバックアップにハイパーリンクを追加します。](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**図 17**: にバックアップする、ユーザーへのハイパーリンク コントロールを追加`SupplierListMaster.aspx`([フルサイズの画像を表示する をクリックします](master-detail-filtering-across-two-pages-cs/_static/image49.png))。


ユーザーが任意の製品がない業者の製品の表示リンクをクリックした場合、`ProductsBySupplierDataSource`で ObjectDataSource`ProductsForSupplierDetails.aspx`結果が返されません。 GridView を ObjectDataSource にバインドされているユーザーのブラウザーでページ上の空白の領域でどのマークアップもレンダリングしません。 GridView のより明確に選択した供給業者に関連付けられている製品がないことをユーザーに通信するために設定できる`EmptyDataText`プロパティを私たちは、このような状況が発生した場合に表示メッセージ。 このプロパティはありません。"この業者によって提供される製品"を設定しました

既定では、Northwinds データベース内のすべてのサプライヤーは、少なくとも 1 つの製品を提供します。 ただし、このチュートリアルでは手動で変更しました、`Products`テーブル Escargots Nouveaux 仕入先は任意の製品と関連付けられなくようにします。 図 18 では、この変更が行われた後、Escargots Nouveaux 詳細ページを利用します。


[![供給業者がすべての製品を提供しないユーザーに通知されます。](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**図 18**: 供給業者がすべての製品を提供しないユーザーに通知されます ([フルサイズの画像を表示する をクリックします](master-detail-filtering-across-two-pages-cs/_static/image52.png))。


## <a name="summary"></a>まとめ

マスター/詳細レポートは、1 ページにマスター/詳細の両方のレコードを表示することができます、多くの web サイトで区切られます 2 つの web ページ。 このチュートリアルでは、「マスター」の web ページで、GridView の仕入先と「詳細」ページに示される関連付けられている製品でこのようなマスター/詳細レポートを実装する方法を説明しました。 マスター web ページのサプライヤーの各行に渡されます行の詳細ページへのリンクが含まれている`SupplierID`値。 このような行に固有のリンクは、GridView の内を使用して簡単に追加できます。

詳細ページで、指定された業者の製品を取得する呼び出すことによって実現されました、`ProductsBLL`クラスの`GetProductsBySupplierID(supplierID)`メソッド。 *`supplierID`* パラメーターのソースとして、クエリ文字列を使用して宣言パラメーターの値が指定されました。 FormView を使用して、詳細ページで、仕入先の詳細を表示する方法についても説明しました。

この[次のチュートリアル](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)マスター/詳細レポートの最終的なであります。 各行が Select ボタンの GridView に製品の一覧を表示する方法について説明します。 [選択] ボタンをクリックすると、同じページに DetailsView コントロールで製品の詳細が表示されます。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者が、Hilton Giesenow です。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](master-detail-filtering-with-two-dropdownlists-cs.md)
> [次へ](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
