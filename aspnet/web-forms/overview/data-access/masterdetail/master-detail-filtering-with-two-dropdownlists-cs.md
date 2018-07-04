---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
title: マスター/詳細のフィルタ リングを 2 つの Dropdownlist (c#) |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、目的の親と祖父母 recor を選択する 2 つの DropDownList コントロールを使用して、3 番目のレイヤーを追加するマスター/詳細リレーションシップを展開しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: ac4b0d77-4816-4ded-afd0-88dab667aedd
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
msc.type: authoredcontent
ms.openlocfilehash: b4d7c6661bf95550ae91ae34f4bd4272c18e24d3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368286"
---
<a name="masterdetail-filtering-with-two-dropdownlists-c"></a>マスター/詳細を 2 つの Dropdownlist (c#) でフィルター処理
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_8_CS.exe)または[PDF のダウンロード](master-detail-filtering-with-two-dropdownlists-cs/_static/datatutorial08cs1.pdf)

> このチュートリアルでは、目的の親、親の親レコードを選択する 2 つの DropDownList コントロールを使用して、3 番目のレイヤーを追加するマスター/詳細リレーションシップを展開します。


## <a name="introduction"></a>はじめに

[前のチュートリアル](master-detail-filtering-with-a-dropdownlist-cs.md)カテゴリと、選択したカテゴリに属しているこれらの製品を示す GridView を含む 1 つの DropDownList を使用して単純なマスター/詳細レポートを表示する方法について確認しました。 このレポートのパターンは、一対多のリレーションシップがあり、複数の一対多リレーションシップを含むシナリオの作業に簡単に拡張できるレコードを表示する場合に機能します。 たとえば、受注システムでは、顧客、注文、および注文品目に対応するテーブルがあります。 特定の顧客は、複数の項目で構成される各注文に複数の注文があります。 このようなデータは、2 つの Dropdownlist と GridView をユーザーに表示することができます。 最初の DropDownList では、データベース内に 2 つ目では、各顧客のリスト項目が、選択した顧客の注文をされているいずれかの内容します。 GridView には、選択された注文から品目が一覧表示します。

Northwind データベースの標準的な顧客/注文/注文詳細情報を含めるときにその`Customers`、`Orders`と`Order Details`アーキテクチャでこれらのテーブルのテーブルがキャプチャされません。 それでも、依存する 2 つの Dropdownlist を使用してについては説明できますも。 最初の DropDownList は、選択したカテゴリに属する製品、カテゴリと、2 つ目が表示されます。 DetailsView を選択した製品の詳細、一覧表示されます。

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>手順 1: を作成して、カテゴリの DropDownList の作成

最初の目標では、カテゴリの一覧を次のドロップダウン リストを追加します。 この手順では、前のチュートリアルで詳しく解説したが、完全を期すのためここでまとめます。

オープン、`MasterDetailsDetails.aspx`ページで、`Filtering`フォルダー、DropDownList に ページで、追加設定その`ID`プロパティを`Categories`のスマート タグのデータ ソースの構成のリンクをクリックします。 データ ソース構成ウィザードからは、新しいデータ ソースを追加を選択します。


[![DropDownList に新しいデータ ソースを追加します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image1.png)

**図 1**: DropDownList に新しいデータ ソースの追加 ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image3.png))。


新しいデータ ソース、当然ながら、べき ObjectDataSource。 名前をこの新しい ObjectDataSource`CategoriesDataSource`してそれを呼び出す、`CategoriesBLL`オブジェクトの`GetCategories()`メソッド。


[![CategoriesBLL クラスを使用します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image4.png)

**図 2**: 使用する、`CategoriesBLL`クラス ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image6.png))。


[![ObjectDataSource GetCategories() メソッドを使用して構成します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image7.png)

**図 3**: 構成に使用する ObjectDataSource、`GetCategories()`メソッド ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image9.png))。


ObjectDataSource を構成すた後必要がありますにどのデータ ソースのフィールドを表示するかを指定する、 `Categories` DropDownList、どれをリスト項目の値として構成する必要があります。 設定、`CategoryName`フィールドとして表示し、`CategoryID`各リスト項目の値として。


[![値として使用 CategoryID と CategoryName フィールド DropDownList 表示があります。](master-detail-filtering-with-two-dropdownlists-cs/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image10.png)

**図 4**: DropDownList の表示、`CategoryName`フィールド`CategoryID`値として ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image12.png))。


この時点で DropDownList コントロールがある (`Categories`) からのレコードに設定されます、`Categories`テーブル。 ユーザーが次のドロップダウン リストから新しいカテゴリを選択すると、DropDownList、手順 2. で作成することが私たちの製品を更新するために発生するへのポストバックがいいでしょう。 そのため、AutoPostBack を有効にするオプションを確認してください、 `categories` DropDownList のスマート タグ。


[![カテゴリの DropDownList の AutoPostBack を有効にします。](master-detail-filtering-with-two-dropdownlists-cs/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image13.png)

**図 5**: の AutoPostBack を有効にする、 `Categories` DropDownList ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image15.png))。


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>手順 2: 2 つ目の DropDownList で選択したカテゴリの製品を表示します。

`Categories` DropDownList が完了したら、次の手順は、選択したカテゴリに属する製品の DropDownList を表示します。 これを実現するもう 1 つの DropDownList をという名前のページに追加`ProductsByCategory`します。 同様、 `Categories` DropDownList の新しい ObjectDataSource を作成、`ProductsByCategory`という名前の DropDownList`ProductsByCategoryDataSource`します。


[![ProductsByCategory DropDownList に新しいデータ ソースを追加します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image16.png)

**図 6**: 新しいデータ ソースを追加、 `ProductsByCategory` DropDownList ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image18.png))。


[![ProductsByCategoryDataSource という名前の新しい ObjectDataSource を作成します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image19.png)

**図 7**: 名前付き新しい ObjectDataSource 作成`ProductsByCategoryDataSource`([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image21.png))。


以降、`ProductsByCategory`選択したカテゴリに属する製品だけを表示する DropDownList ニーズがある呼び出す ObjectDataSource、`GetProductsByCategoryID(categoryID)`からメソッド、`ProductsBLL`オブジェクト。


[![ProductsBLL クラスを使用します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image22.png)

**図 8**: 使用する、`ProductsBLL`クラス ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image24.png))。


[![ObjectDataSource GetProductsByCategoryID(categoryID) メソッドを使用して構成します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image25.png)

**図 9**: 構成に使用する ObjectDataSource、`GetProductsByCategoryID(categoryID)`メソッド ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image27.png))。


値を指定しなければ、ウィザードの最後の手順で、 *`categoryID`* パラメーター。 選択されたアイテムにこのパラメーターを割り当てる、 `Categories` DropDownList します。


[![カテゴリ DropDownList から categoryID パラメーターの値を取得します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image28.png)

**図 10**: プル、 *`categoryID`* からパラメーター値、 `Categories` DropDownList ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image30.png))。


ObjectDataSource が構成されているとは次のドロップダウン リストの項目の値や表示の対象に使用されるデータ ソース フィールドを指定します。 表示、`ProductName`フィールドし、を使用して、`ProductID`値としてフィールド。


[![DropDownList のリスト項目のテキストと値のプロパティに使用されるデータ ソースのフィールドを指定します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image31.png)

**図 11**: DropDownList に使用されるデータ ソースのフィールドを指定`ListItem`s'`Text`と`Value`プロパティ ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image33.png))。


ObjectDataSource でと`ProductsByCategory`DropDownList には、ページが構成されている 2 つの Dropdownlist に表示されます。 最初はすべて一覧表示、カテゴリの 2 つ目は、選択したカテゴリに属しているこれらの製品を一覧表示中にします。 ユーザーは、最初の DropDownList から新しいカテゴリを選択してポストバックが発生したりする 2 つ目の DropDownList を再バインドは、新しく選択したカテゴリに属しているこれらの製品を表示します。 図 12、13 show`MasterDetailsDetails.aspx`ブラウザーで表示したときにアクションにします。


[![飲み物のカテゴリが選択されている場合、最初のページにアクセスして、](master-detail-filtering-with-two-dropdownlists-cs/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image34.png)

**図 12**: 飲み物のカテゴリが選択されている場合、最初のページにアクセスして、([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image36.png))。


[![別のカテゴリを選択すると、新しいカテゴリの製品が表示されます。](master-detail-filtering-with-two-dropdownlists-cs/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image37.png)

**図 13**: 別のカテゴリが表示されます、新しいカテゴリの製品を選択する ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image39.png))。


現在、 `productsByCategory` DropDownList を変更されたときに*いない*ポストバックが発生します。 ただし、選択した製品の詳細 (手順 3) を表示する、DetailsView を追加すると発生するへのポストバックします。 そのためから AutoPostBack を有効にするチェック ボックスをオン、 `productsByCategory` DropDownList のスマート タグ。


[![ProductsByCategory DropDownList の AutoPostBack 機能を有効にします。](master-detail-filtering-with-two-dropdownlists-cs/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image40.png)

**図 14**: AutoPostBack 機能を有効にする、 `productsByCategory` DropDownList ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image42.png))。


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>手順 3: DetailsView を使用して、選択した製品の詳細を表示するには

最後の手順では、DetailsView で選択した製品の詳細を表示します。 ページに、DetailsView を追加するこれを実現するに次のように設定します。 その`ID`プロパティを`ProductDetails`、し、その新しい ObjectDataSource を作成します。 構成からそのデータをプルするには、この ObjectDataSource、`ProductsBLL`クラスの`GetProductByProductID(productID)`メソッドの選択した値を使用して、`ProductsByCategory`の DropDownList の値の*`productID`* パラメーター。


[![ProductsBLL クラスを使用します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image43.png)

**図 15**: 使用する、`ProductsBLL`クラス ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image45.png))。


[![ObjectDataSource GetProductByProductID(productID) メソッドを使用して構成します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image46.png)

**図 16**: 構成に使用する ObjectDataSource、`GetProductByProductID(productID)`メソッド ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image48.png))。


[![ProductsByCategory DropDownList から productID パラメーターの値を取得します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image49.png)

**図 17**: プル、 *`productID`* からパラメーター値、 `ProductsByCategory` DropDownList ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image51.png))。


使用可能なフィールドのいずれかを DetailsView で表示することができます。 削除することを選択する、 `ProductID`、 `SupplierID`、および`CategoryID`フィールドし順序を変更し、残りのフィールドを書式設定します。 DetailsView は取り除かさらに、`Height`と`Width`プロパティ、指定されたサイズに制約があるのではなく、そのデータに最適な表示に必要な幅を拡張する DetailsView を許可します。 完全なマークアップは、以下が表示されます。


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample1.aspx)]

少し試して、`MasterDetailsDetails.aspx`ブラウザーでページ。 一見、必要に応じて動作しますが、微妙な問題があるように思われる場合します。 新しいカテゴリを選択すると、 `ProductsByCategory` DropDownList が更新され、選択したカテゴリの製品が、 `ProductDetails` DetailsView は引き続き以前の製品情報を表示します。 選択したカテゴリ別の製品を選択するときに、DetailsView が更新されます。 さらに、十分な徹底的にテストすると、見つかりますを継続的に新しいカテゴリを選択した場合 (から飲み物を選択するなど、 `Categories` DropDownList、調味料、し、ある) その他のすべてのカテゴリ選択により、 `ProductDetails`DetailsView を更新します。

この問題を具体化するためは、具体的な例を見てみましょう。 飲み物のカテゴリが選択されているし、関連製品に読み込まれるページに初めてアクセスするときに、 `ProductsByCategory` DropDownList します。 Chai は、選択した製品とその詳細が表示されます、 `ProductDetails` DetailsView、図 18 に示すようにします。


[![選択されている製品の詳細は、DetailsView で表示されます。](master-detail-filtering-with-two-dropdownlists-cs/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image52.png)

**図 18**: DetailsView、選択した製品の詳細が表示されます ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image54.png))。


ポストバックが発生した飲み物から調味料にカテゴリ選択を変更した場合、 `ProductsByCategory` DropDownList は、それに応じて更新されますが、DetailsView はまだ Chai の詳細を表示します。


[![以前選択されている製品の詳細については引き続き表示されます。](master-detail-filtering-with-two-dropdownlists-cs/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image55.png)

**図 19**: The 以前選択されている製品の詳細については引き続き表示されます ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image57.png))。


想定どおり、DetailsView を更新リストから新しい製品を選択します。 製品を変更した後、新しいカテゴリを選択する場合、DetailsView もう一度更新しません。 ただし、新しい製品を選択するのではなく、新しいカテゴリを選択した場合、DetailsView が更新されます。 世界で起こっているここか。

問題は、ページのライフ サイクルのタイミングの問題です。 たびに、そのレンダリングといくつかの手順を進め、ページが要求されます。 ObjectDataSource のいずれかを確認の制御手順のいずれかでその`SelectParameters`値が変更されました。 そのため、データ Web コントロールがバインドされている場合、ObjectDataSource では、その表示を更新する必要があることがわかっています。 たとえば、新しいカテゴリを選択すると、 `ProductsByCategoryDataSource` ObjectDataSource は、パラメーター値が変更されたことを検出、 `ProductsByCategory` DropDownList を再バインド自体には、選択したカテゴリの製品を取得します。

このような状況で発生した問題は、ObjectDataSources パラメーターの変更をチェックするページのライフ サイクル内のポイントが発生する*する前に*関連付けられているデータ Web コントロールの再バインドします。 そのため、新しいカテゴリを選択するときに、 `ProductsByCategoryDataSource` ObjectDataSource では、そのパラメーターの値の変更を検出します。 ObjectDataSource で使用される、 `ProductDetails` DetailsView、ただしは書き留めておきます。 このような変更、 `ProductsByCategory` DropDownList はまだ再バインドできます。 ライフ サイクルの後で、 `ProductsByCategory` DropDownList を新しく選択したカテゴリの製品を取得して、その ObjectDataSource に再バインドします。 中に、 `ProductsByCategory` DropDownList の値が変更されて、 `ProductDetails` DetailsView の ObjectDataSource が既にそのパラメーター値のチェックを実行。 そのため、DetailsView がその前の結果が表示されます。 この操作は、図 20 で表したものです。


[![ProductsByCategory DropDownList の値が変更された ProductDetails DetailsView の ObjectDataSource の変更を確認した後](master-detail-filtering-with-two-dropdownlists-cs/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image58.png)

**図 20**: `ProductsByCategory` DropDownList の値の変更後、 `ProductDetails` DetailsView の変更の ObjectDataSource 確認 ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image60.png))。


明示的に再バインドする必要があります。 これを解決するには、`ProductDetails`後 DetailsView、 `ProductsByCategory` DropDownList にバインドします。 これを呼び出すことによって実現しましたできます、 `ProductDetails` DetailsView の`DataBind()`メソッドと、 `ProductsByCategory` DropDownList の`DataBound`イベントが発生します。 次のイベント ハンドラー コードを追加、`MasterDetailsDetails.aspx`ページの分離コード クラス (を参照してください、"[ObjectDataSource のパラメーター値をプログラムによって設定](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)"イベント ハンドラーを追加する方法の詳細については)。


[!code-csharp[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample2.cs)]

この明示的な呼び出しの後、 `ProductDetails` DetailsView の`DataBind()`メソッドが追加されていますが、チュートリアルが期待どおりに動作します。 図 21 の強調表示がこれを変更する方法は、以前の問題を修正できます。


[![ProductDetails DetailsView は明示的に更新されるときに、ProductsByCategory DropDownList のデータ バインド イベントが発生します](master-detail-filtering-with-two-dropdownlists-cs/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image61.png)

**図 21**: `ProductDetails` DetailsView は明示的に更新されるときに、 `ProductsByCategory` DropDownList の`DataBound`イベントが発生します ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-two-dropdownlists-cs/_static/image63.png))。


## <a name="summary"></a>まとめ

マスター/詳細レポートの最適なユーザー インターフェイス要素として、DropDownList の機能が、マスター/詳細レコードの間に一対多のリレーションシップがある場合。 前のチュートリアルでは、1 つの DropDownList を使用して、選択したカテゴリによって表示される製品をフィルター処理する方法を説明しました。 このチュートリアルでは製品の GridView に置き換え、DropDownList を DetailsView、選択した製品の詳細を表示するために使用します。 このチュートリアルで説明する概念は、顧客、注文、注文項目など、複数の一対多リレーションシップに関連するデータ モデルに簡単に拡張できます。 一般に、一対多リレーションシップの「一」のエンティティの各 DropDownList をいつでも追加できます。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者が、Hilton Giesenow です。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](master-detail-filtering-with-a-dropdownlist-cs.md)
> [次へ](master-detail-filtering-across-two-pages-cs.md)
