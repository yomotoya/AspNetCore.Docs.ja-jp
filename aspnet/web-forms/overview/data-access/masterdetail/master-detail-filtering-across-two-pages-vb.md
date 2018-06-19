---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
title: マスター/詳細 (VB) の 2 つのページにわたってフィルター |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでをデータベースの仕入先の一覧を表示する GridView を使用して、このパターンを実装します。 GridView のサプライヤーの各行には、Vie が含まれます.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 361d6a44-3f1f-4daf-85df-d4c2b8bf065d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: b8250c0f8d1befacf66d6be517514aad8bc31b09
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887006"
---
<a name="masterdetail-filtering-across-two-pages-vb"></a>マスター/詳細の 2 つのページ (VB) の間でフィルター処理
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_9_VB.exe)または[PDF のダウンロード](master-detail-filtering-across-two-pages-vb/_static/datatutorial09vb1.pdf)

> このチュートリアルでをデータベースの仕入先の一覧を表示する GridView を使用して、このパターンを実装します。 GridView のサプライヤーの各行は、クリックされると、別のページにユーザーを受け取るときに一覧表示されるこれらの製品の仕入先を選択した製品の表示のリンクが格納されます。


## <a name="introduction"></a>はじめに

前の 2 つのチュートリアルで DropDownLists を使用して、「マスター」のレコードを表示する単一の web ページにマスター/詳細レポートを表示する方法を説明しました、 [GridView](master-detail-filtering-with-a-dropdownlist-vb.md)または[DetailsView](master-detail-filtering-with-two-dropdownlists-vb.md)表示するコントロールを"詳細情報。" マスター/詳細レポートに使用されるもう 1 つの一般的なパターンでは、1 つの web ページと他の場所で表示される詳細のマスター レコードです。 フォーラムの web サイトと同様、 [ASP.NET フォーラム](https://forums.asp.net/)実際には、このパターンの優れた例を示します。 ASP.NET フォーラムでは、さまざまなフォーラム作業の開始、Web フォーム、データのプレゼンテーション コントロールで構成されておよびなどです。 多くのスレッドの各フォーラムがで構成され、各スレッドは、投稿の数で構成されます。 ASP.NET のフォーラムのホーム ページで、フォーラムの一覧が表示されます。 Whisks をフォーラムをクリックすると、`ShowForum.aspx`ページで、そのフォーラムのスレッドが一覧表示します。 同様に、スレッドをクリックするにはする`ShowPost.aspx`、クリックされたスレッドの投稿を表示します。

このチュートリアルでをデータベースの仕入先の一覧を表示する GridView を使用して、このパターンを実装します。 GridView のサプライヤーの各行は、クリックされると、別のページにユーザーを受け取るときに一覧表示されるこれらの製品の仕入先を選択した製品の表示のリンクが格納されます。

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>手順 1: 追加`SupplierListMaster.aspx`と`ProductsForSupplierDetails.aspx`ページを`Filtering`フォルダー

"Starter"のページの数を追加しました 3 番目のチュートリアルでは、ページ レイアウトを定義するときに、 `BasicReporting`、 `Filtering`、および`CustomFormatting`フォルダーです。 ただし、その時点で、このチュートリアルのスターター ページを追加おしなかったできません、のですぐをする 2 つの新しいページを追加、`Filtering`フォルダー:`SupplierListMaster.aspx`と`ProductsForSupplierDetails.aspx`です。 `SupplierListMaster.aspx` 中に「マスター」のレコード (仕入先) ボックスの一覧は`ProductsForSupplierDetails.aspx`選択した供給業者の製品が表示されます。

ときにこれら 2 つの新しいページを作成する必要がでそれらを関連付けて、`Site.master`マスター ページ。


![フィルターのフォルダーに SupplierListMaster.aspx と ProductsForSupplierDetails.aspx ページを追加します。](master-detail-filtering-across-two-pages-vb/_static/image1.png)

**図 1**: 追加、`SupplierListMaster.aspx`と`ProductsForSupplierDetails.aspx`ページを`Filtering`フォルダー


また、新しいページをプロジェクトに追加すると、ときに必ずサイト マップ ファイルを更新する`Web.sitemap`、それに従っています。 このチュートリアルでは単に追加、 `SupplierListMaster.aspx` 、フィルター処理のレポートの子として、次の XML コンテンツを使用してサイト マップにページ`<siteMapNode>`要素。


[!code-xml[Main](master-detail-filtering-across-two-pages-vb/samples/sample1.xml)]

> [!NOTE]
> 使用してページの新しい ASP.NET を追加するときに、サイト マップ ファイルの更新のプロセスを自動化できます[K. Scott Allen](http://odetocode.com/Blogs/scott/)Visual Studio を空き 's[サイト マップ マクロ](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx)です。


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>手順 2: の仕入先の一覧を表示します。`SupplierListMaster.aspx`

`SupplierListMaster.aspx`と`ProductsForSupplierDetails.aspx`の仕入先の GridView を作成するページが作成された、次の手順は`SupplierListMaster.aspx`します。 GridView をページに追加し、新しい ObjectDataSource にバインドします。 この ObjectDataSource を使用する必要があります、`SuppliersBLL`クラスの`GetSuppliers()`all を返します。


[![SuppliersBLL クラスを選択します。](master-detail-filtering-across-two-pages-vb/_static/image3.png)](master-detail-filtering-across-two-pages-vb/_static/image2.png)

**図 2**: 選択、`SuppliersBLL`クラス ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-across-two-pages-vb/_static/image4.png))


[![ObjectDataSource GetSuppliers() メソッドを使用して構成します。](master-detail-filtering-across-two-pages-vb/_static/image6.png)](master-detail-filtering-across-two-pages-vb/_static/image5.png)

**図 3**: 構成を使用する ObjectDataSource、`GetSuppliers()`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-across-two-pages-vb/_static/image7.png))


リンクを含める必要がありますという製品の表示 GridView 行ごとをクリックすると、ユーザーには、 `ProductsForSupplierDetails.aspx` 、選択した行を渡して`SupplierID`クエリ文字列を使用して値。 たとえば、ユーザーが東京 Traders 業者の製品の表示のリンクをクリックする (を持つ、 `SupplierID` 4 の値) を送信する`ProductsForSupplierDetails.aspx?SupplierID=4`です。

これを実現するには追加、[内](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx)GridView に GridView 行ごとにハイパーリンクを追加します。 GridView のスマート タグの表示から列の編集リンクをクリックして開始します。 次に、左上にある一覧から、内を選択し、内の一覧に含める GridView のフィールドの追加 をクリックします。


[![GridView に内を追加します。](master-detail-filtering-across-two-pages-vb/_static/image9.png)](master-detail-filtering-across-two-pages-vb/_static/image8.png)

**図 4**: GridView に内に追加 ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-across-two-pages-vb/_static/image10.png))


内は、同じテキストを使用するように構成できます。 または URL が GridView の行ごとにあるリンクを値または特定の行ごとにバインドされているデータ値に対するこれらの値を基本ことができます。 すべての行に値を静的なを指定するには、内を使用して`Text`または`NavigateUrl`プロパティです。 すべての行について同じにするには、このリンク テキストので、セット内の`Text`製品を表示するプロパティです。


[![製品の表示を内のテキストのプロパティを設定します。](master-detail-filtering-across-two-pages-vb/_static/image12.png)](master-detail-filtering-across-two-pages-vb/_static/image11.png)

**図 5**: 設定内の`Text`製品を表示するプロパティ ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-across-two-pages-vb/_static/image13.png))


テキストまたは GridView の行にバインドされている基になるデータに基づく URL 値を設定するデータ フィールド、テキストまたはで URL 値をプルする必要がありますを指定して、`DataTextField`または`DataNavigateUrlFields`プロパティです。 `DataTextField` 1 つのデータ フィールドにのみ設定できます。`DataNavigateUrlFields`、ただし、データ フィールドのコンマ区切りの一覧を設定することができます。 多くの場合、テキストまたは現在の行のデータ フィールドの値といくつかの static のマークアップの組み合わせで URL を基準とする必要があります。 このチュートリアルでは、たとえば、必要である内のリンクの URL`ProductsForSupplierDetails.aspx?SupplierID=supplierID`ここで、 *`supplierID`* 各 GridView の行は、`SupplierID`値。 静的必要があります、データ ドリブン値をここに注意してください:`ProductsForSupplierDetails.aspx?SupplierID=`リンクの URL の部分は静的では、 *`supplierID`* 部分は、データ ドリブンの値が行ごとの独自`SupplierID`値。

静的およびデータ ドリブンの値の組み合わせを示すために使用して、`DataTextFormatString`と`DataNavigateUrlFormatString`プロパティです。 これらのプロパティには static のマークアップを必要に応じて入力し、マーカーを使用して`{0}`で指定されたフィールドの値となる、`DataTextField`または`DataNavigateUrlFields`プロパティを表示します。 場合、`DataNavigateUrlFields`プロパティが複数のフィールドの指定した使用`{0}`挿入すると、最初のフィールド値を表示する場所`{1}`2 番目のフィールドの値を示す場合。

設定する必要があります、チュートリアルには、これを適用すること、`DataNavigateUrlFields`プロパティを`SupplierID`データ フィールドの行ごとにカスタマイズする必要があります値を持つので、および`DataNavigateUrlFormatString`プロパティを`ProductsForSupplierDetails.aspx?SupplierID={0}`です。


[![SupplierID に基づいて適切なリンクの url 内を構成します。](master-detail-filtering-across-two-pages-vb/_static/image15.png)](master-detail-filtering-across-two-pages-vb/_static/image14.png)

**図 6**: に、適切なリンク URL ベース時に含める内の構成、 `SupplierID` ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-across-two-pages-vb/_static/image16.png))


内を追加すると、自由にカスタマイズし、GridView のフィールドの順序を変更できます。 次のマークアップは、マイナー フィールド レベルのカスタマイズの一部を作成した後、GridView を示しています。


[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample2.aspx)]

表示するのにはしばらく時間かかる、`SupplierListMaster.aspx`ページがブラウザーを使用します。 図 7 に示すページ現在すべての製品の表示のリンクを含む仕入先を示します。 製品の表示をクリックするとリンクをクリックすると、`ProductsForSupplierDetails.aspx`業者に沿って渡す`SupplierID`クエリ文字列にします。


[![各仕入先の行には、ビューの製品リンクが含まれています。](master-detail-filtering-across-two-pages-vb/_static/image18.png)](master-detail-filtering-across-two-pages-vb/_static/image17.png)

**図 7**: 仕入先の行ごとにビューの製品のリンクが含まれています ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-across-two-pages-vb/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>手順 3: 供給業者の製品を一覧表示します。`ProductsForSupplierDetails.aspx`

この時点で、`SupplierListMaster.aspx`ページがユーザーに送信して`ProductsForSupplierDetails.aspx`、選択した業者を渡す`SupplierID`クエリ文字列の。 チュートリアルの最後での GridView に製品を表示する`ProductsForSupplierDetails.aspx`が`SupplierID`equals、`SupplierID`クエリ文字列が渡されます。 GridView を追加することによってこの開始を実行する、`ProductsForSupplierDetails.aspx`という名前の新しい ObjectDataSource コントロールを使ってページ`ProductsBySupplierDataSource`を呼び出す、`GetProductsBySupplierID(supplierID)`メソッドから、`ProductsBLL`クラスです。


[![ProductsBySupplierDataSource をという名前の新しい ObjectDataSource を追加します。](master-detail-filtering-across-two-pages-vb/_static/image21.png)](master-detail-filtering-across-two-pages-vb/_static/image20.png)

**図 8**: 新しい ObjectDataSource という追加`ProductsBySupplierDataSource`([フルサイズのイメージを表示するをクリックして](master-detail-filtering-across-two-pages-vb/_static/image22.png))


[![ProductsBLL クラスを選択します。](master-detail-filtering-across-two-pages-vb/_static/image24.png)](master-detail-filtering-across-two-pages-vb/_static/image23.png)

**図 9**: 選択、`ProductsBLL`クラス ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-across-two-pages-vb/_static/image25.png))


[![GetProductsBySupplierID(supplierID) メソッドを呼び出す ObjectDataSource があります。](master-detail-filtering-across-two-pages-vb/_static/image27.png)](master-detail-filtering-across-two-pages-vb/_static/image26.png)

**図 10**: ObjectDataSource 呼び出しがある、`GetProductsBySupplierID(supplierID)`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-across-two-pages-vb/_static/image28.png))


データ ソース構成ウィザードの最後の手順では、ソースを提供するよう求められます、`GetProductsBySupplierID(supplierID)`メソッドの*`supplierID`* パラメーター。 クエリ文字列値を使用するクエリ文字列にパラメーターのソースを設定し、QueryStringField ボックスで使用するクエリ文字列値の名前を入力 (`SupplierID`)。


[![SupplierID SupplierID Querystring 値からパラメーター値の設定します。](master-detail-filtering-across-two-pages-vb/_static/image30.png)](master-detail-filtering-across-two-pages-vb/_static/image29.png)

**図 11**: への追加、 *`supplierID`* からパラメーター値、 `SupplierID` Querystring 値 ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-across-two-pages-vb/_static/image31.png))


すべてであることには! 図 12 を示しています、`ProductsForSupplierDetails.aspx`から東京 Traders リンクをクリックしてアクセスしたときにページ`SupplierListMaster.aspx`です。


[![東京 Traders によって提供される製品が表示されます。](master-detail-filtering-across-two-pages-vb/_static/image33.png)](master-detail-filtering-across-two-pages-vb/_static/image32.png)

**図 12**: 東京 Traders によって提供される、製品が表示されます ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-across-two-pages-vb/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>仕入先情報を表示します。`ProductsForSupplierDetails.aspx`

図 12 に示す、 `ProductsForSupplierDetails.aspx`  ページで提供されている製品を単純に一覧表示、`SupplierID`クエリ文字列で指定します。 このページに直接送信ただしはわからない図 12 に東京 Traders 製品が表示されていること。 この問題を解決するもこのページで仕入先の情報を表示おできます。

製品 GridView 上のフォーム ビューを追加することで開始します。 という新しい ObjectDataSource コントロールを作成`SuppliersDataSource`を呼び出す、`SuppliersBLL`クラスの`GetSupplierBySupplierID(supplierID)`メソッドです。


[![SuppliersBLL クラスを選択します。](master-detail-filtering-across-two-pages-vb/_static/image36.png)](master-detail-filtering-across-two-pages-vb/_static/image35.png)

**図 13**: 選択、`SuppliersBLL`クラス ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-across-two-pages-vb/_static/image37.png))


[![GetSupplierBySupplierID(supplierID) メソッドを呼び出す ObjectDataSource があります。](master-detail-filtering-across-two-pages-vb/_static/image39.png)](master-detail-filtering-across-two-pages-vb/_static/image38.png)

**図 14**: ObjectDataSource 呼び出しがある、`GetSupplierBySupplierID(supplierID)`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-across-two-pages-vb/_static/image40.png))


同様、`ProductsBySupplierDataSource`が、 *`supplierID`* パラメーターの値が割り当て、 `SupplierID` querystring 値。


[![SupplierID SupplierID Querystring 値からパラメーター値の設定します。](master-detail-filtering-across-two-pages-vb/_static/image42.png)](master-detail-filtering-across-two-pages-vb/_static/image41.png)

**図 15**: への追加、 *`supplierID`* からパラメーター値、 `SupplierID` Querystring 値 ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-across-two-pages-vb/_static/image43.png))


FormView のデザイン ビューで ObjectDataSource を FormView をバインドするときに、自動的に Visual Studio を作成`ItemTemplate`、 `InsertItemTemplate`、および`EditItemTemplate`の各によって返されるデータ フィールドのラベルと TextBox Web コントロールで、ObjectDataSource です。 仕入先情報お気軽に削除を表示したいだけのため、`InsertItemTemplate`と`EditItemTemplate`です。 業者の会社名を表示するように、ItemTemplate を次に、編集、`<h3>`要素と、アドレス、city、country、および会社名の下に電話番号。 FormView を手動で設定する代わりに、`DataSourceID`を作成し、 `ItemTemplate` 、マークアップで行ったように、"[、ObjectDataSource でデータを表示する](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)"チュートリアルです。

これらの編集後フォーム ビューの宣言型マークアップを次のようになります。


[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample3.aspx)]

図 16 のスクリーン ショットを示しています、`ProductsForSupplierDetails.aspx`ページ後上述仕入先の情報が含まれています。


[![製品の一覧には、供給業者に関する概要が含まれています。](master-detail-filtering-across-two-pages-vb/_static/image45.png)](master-detail-filtering-across-two-pages-vb/_static/image44.png)

**図 16**: 製品の一覧には、「業者に関する概要が含まれています ([フルサイズ イメージを表示するに、をクリックして](master-detail-filtering-across-two-pages-vb/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>触れた最後の適用、`ProductsForSupplierDetails.aspx`UI

このレポートがありますのエクスペリエンスが、いくつかの追加をするべきは、ユーザーを向上させるために、`ProductsForSupplierDetails.aspx`ページ。 現在、唯一の方法から移行できる、ユーザー、`ProductsForSupplierDetails.aspx`ページ仕入先の一覧には、ブラウザーの戻るボタンをクリックしてします。 ハイパーリンク コントロールを追加してみましょう。、`ProductsForSupplierDetails.aspx`にリンク バック ページ`SupplierListMaster.aspx`、マスター リストを取得するユーザーの別の方法を提供します。


[![ユーザーを SupplierListMaster.aspx を取り戻すにハイパーリンク コントロールを追加します。](master-detail-filtering-across-two-pages-vb/_static/image48.png)](master-detail-filtering-across-two-pages-vb/_static/image47.png)

**図 17**: を取り戻すに、ユーザーへのハイパーリンク コントロールを追加`SupplierListMaster.aspx`([フルサイズのイメージを表示するをクリックして](master-detail-filtering-across-two-pages-vb/_static/image49.png))


すべての製品がない業者の製品の表示のリンクをクリックすると、ユーザー、`ProductsBySupplierDataSource`で ObjectDataSource`ProductsForSupplierDetails.aspx`結果が返されません。 ObjectDataSource にバインドされた GridView は、ユーザーのブラウザーでページ上の空白領域の結果として得られる任意マークアップを表示しません。 GridView のさらに明確に選択した供給業者に関連付けられている製品がないことをユーザーに通信するために設定できる`EmptyDataText`プロパティをこのような状況が発生したときに表示されているメッセージにします。 このプロパティを"There are no この業者によって提供される製品"に設定しました

既定では、Northwinds データベース内のすべてのサプライヤーには、少なくとも 1 つの製品を提供します。 ただし、このチュートリアルでは手動で変更しました、`Products`テーブルされるよう、供給業者 Escargots Nouveaux 不要になったすべての製品に関連付けられています。 図 18 では、この変更が行われた後に、Escargots Nouveaux 詳細ページを利用します。


[![供給業者がすべての製品を提供しないユーザーに通知されます。](master-detail-filtering-across-two-pages-vb/_static/image51.png)](master-detail-filtering-across-two-pages-vb/_static/image50.png)

**図 18**: 供給業者がすべての製品を提供しないユーザーに通知されます ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-across-two-pages-vb/_static/image52.png))


## <a name="summary"></a>まとめ

マスター/詳細レポートを 1 ページにマスター/詳細の両方のレコードが表示されることができます、多くの web サイトで区切られます複数の 2 つの web ページ。 このチュートリアルでは、「マスター」の web ページの GridView に一覧表示 [仕入先] と [詳細] ページに示される関連付けられている製品で、このようなマスター/詳細レポートを実装する方法について説明しました。 マスターの web ページのサプライヤーの各行に渡されます行の詳細ページへのリンクが含まれている`SupplierID`値。 このような特定の行へのリンクは、GridView の内を使用して簡単に追加できます。

[詳細] ページで、指定された業者のこれらの製品を取得する呼び出すことによって実現されました、`ProductsBLL`クラスの`GetProductsBySupplierID(supplierID)`メソッドです。 *`supplierID`* パラメーターのソースとして、クエリ文字列を使用して宣言型パラメーターの値が指定されました。 詳細 ページを使用して、FormView で仕入先の詳細を表示する方法についても説明しました。

当社[次のチュートリアル](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md)マスター/詳細レポートの最終的なであります。 行のそれぞれが [選択] ボタンが GridView で製品の一覧を表示する方法に紹介します。 [選択] ボタンをクリックすると、同じページ上の DetailsView コントロールで製品の詳細が表示されます。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Hilton Giesenow しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](master-detail-filtering-with-two-dropdownlists-vb.md)
> [次へ](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md)
