---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
title: マスター/詳細フィルターは、次の 2 つの DropDownLists (c#) と共に |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでは、DropDownList の 2 つのコントロールを使用して、必要なは、親と祖父母 recor を選択する 3 番目の層を追加するマスター/詳細関係を展開しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: ac4b0d77-4816-4ded-afd0-88dab667aedd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
msc.type: authoredcontent
ms.openlocfilehash: d971dcb3814dc088202c3a3e4addb03375049ca0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887253"
---
<a name="masterdetail-filtering-with-two-dropdownlists-c"></a>マスター/詳細のフィルター処理を次の 2 つの DropDownLists (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_8_CS.exe)または[PDF のダウンロード](master-detail-filtering-with-two-dropdownlists-cs/_static/datatutorial08cs1.pdf)

> このチュートリアルでは、2 つの DropDownList コントロールを使用して、目的の親またはその上の先祖のレコードを選択する 3 番目の層を追加するマスター/詳細関係を展開します。


## <a name="introduction"></a>はじめに

[前のチュートリアル](master-detail-filtering-with-a-dropdownlist-cs.md)カテゴリおよび選択したカテゴリに属しているこれらの製品を示す GridView が設定されて 1 つの DropDownList を使用して単純なマスター/詳細レポートを表示する方法について説明しました。 このレポートのパターンは、レコードを一対多のリレーションシップを持ち、複数の一対多リレーションシップを含むシナリオに有効に簡単に拡張することができますを表示するときにも動作します。 たとえば、注文入力システムでは、顧客、注文、および注文品目に対応するテーブルがあります。 特定の顧客は、複数の項目で構成される各注文に複数の注文があります。 このようなデータは、次の 2 つの DropDownLists や GridView でユーザーに提示できます。 最初の DropDownList では、データベースと 2 番目の顧客ごとにリスト項目が、選択した顧客の注文をされているいずれかの内容します。 GridView は、選択した注文の品目を一覧表示します。

Northwind データベースを含めるの標準的な顧客/注文/注文の詳細情報は、その`Customers`、`Orders`と`Order Details`アーキテクチャでは、これらのテーブルのテーブルがキャプチャされません。 ただし、2 つの依存 DropDownLists を使用してについては説明ことができますも。 最初の DropDownList は製品の一覧が、カテゴリと、2 つ目、選択したカテゴリに属しています。 DetailsView が一覧表示、選択した製品の詳細。

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>手順 1: を作成して、カテゴリの DropDownList の作成

最初の目標は、カテゴリの一覧の DropDownList を追加します。 この手順では、前のチュートリアルで詳しく解説したが、完全を期すのためここでまとめます。

開いている、 `MasterDetailsDetails.aspx`  ページで、`Filtering`フォルダー、DropDownList ページを追加、設定、`ID`プロパティを`Categories`、し、リンクをクリックし、データ ソースの構成のスマート タグでします。 データ ソース構成ウィザードからは、新しいデータ ソースを追加するを選択します。


[![DropDownList の新しいデータ ソースを追加します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image1.png)

**図 1**: DropDownList の新しいデータ ソースの追加 ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image3.png))


新しいデータ ソース、当然があります、ObjectDataSource。 名前をこの新しい ObjectDataSource`CategoriesDataSource`してそれを呼び出し、`CategoriesBLL`オブジェクトの`GetCategories()`メソッドです。


[![CategoriesBLL クラスを使用します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image4.png)

**図 2**: 使用する、`CategoriesBLL`クラス ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image6.png))


[![ObjectDataSource GetCategories() メソッドを使用して構成します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image7.png)

**図 3**: 構成を使用する ObjectDataSource、`GetCategories()`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image9.png))


ObjectDataSource を構成すた後必要がありますでデータ ソース フィールドを表示するかを指定する、 `Categories` DropDownList どれがリスト項目の値として構成する必要があります。 設定、`CategoryName`ディスプレイとしてフィールドと`CategoryID`各リスト項目の値として。


[![値として使用 CategoryID と CategoryName フィールドの DropDownList の表示があります。](master-detail-filtering-with-two-dropdownlists-cs/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image10.png)

**図 4**: DropDownList ディスプレイ、`CategoryName`フィールド`CategoryID`値として ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image12.png))


この時点での DropDownList のコントロールがある (`Categories`) からのレコードに設定されます、`Categories`テーブル。 次のドロップダウン リストから、新しいカテゴリを選択するは手順 2. で作成することの DropDownList の製品を更新するために発生する可能性へのポストバックがいいでしょう。 したがってから AutoPostBack を有効にするオプションを確認、 `categories` DropDownList のスマート タグです。


[![カテゴリの DropDownList の AutoPostBack を有効にします。](master-detail-filtering-with-two-dropdownlists-cs/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image13.png)

**図 5**: を有効にする AutoPostBack、 `Categories` DropDownList ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>手順 2: 2 つ目の DropDownList で選択したカテゴリの製品を表示します。

`Categories` DropDownList が完了すると、次に、選択したカテゴリに属している製品の DropDownList を表示します。 これを実現するには、という名前のページに別の DropDownList を追加`ProductsByCategory`です。 同様、 `Categories` DropDownList の新しい ObjectDataSource を作成、`ProductsByCategory`という名前の DropDownList`ProductsByCategoryDataSource`です。


[![ProductsByCategory DropDownList の新しいデータ ソースを追加します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image16.png)

**図 6**: 新しいデータ ソースを追加、 `ProductsByCategory` DropDownList ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image18.png))


[![ProductsByCategoryDataSource をという名前の新しい ObjectDataSource を作成します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image19.png)

**図 7**: 名前付き新しい ObjectDataSource 作成`ProductsByCategoryDataSource`([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image21.png))


以降、`ProductsByCategory`を選択したカテゴリに属する製品だけを表示する DropDownList ニーズがある呼び出し ObjectDataSource、`GetProductsByCategoryID(categoryID)`メソッドから、`ProductsBLL`オブジェクト。


[![ProductsBLL クラスを使用します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image22.png)

**図 8**: 使用する、`ProductsBLL`クラス ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image24.png))


[![ObjectDataSource GetProductsByCategoryID(categoryID) メソッドを使用して構成します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image25.png)

**図 9**: 構成を使用する ObjectDataSource、`GetProductsByCategoryID(categoryID)`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image27.png))


値を指定しなければ、ウィザードの最後の手順で、 *`categoryID`* パラメーター。 このパラメーターをから選択した項目に割り当てる、 `Categories` DropDownList です。


[![カテゴリの DropDownList から categoryID パラメーターの値をプルします。](master-detail-filtering-with-two-dropdownlists-cs/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image28.png)

**図 10**: プル、 *`categoryID`* からパラメーター値、 `Categories` DropDownList ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image30.png))


構成 ObjectDataSource には、ディスプレイと DropDownList の項目の値の使用がどのようなデータ ソースのフィールドを指定します。 表示、`ProductName`フィールドに使用して、`ProductID`値としてフィールドです。


[![DropDownList のリスト項目のテキストと値のプロパティを使用するデータ ソース フィールドを指定します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image31.png)

**図 11**: DropDownList を使用するデータ ソースのフィールドを指定`ListItem`s'`Text`と`Value`プロパティ ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image33.png))


ObjectDataSource と`ProductsByCategory`DropDownList には、ページが構成されている 2 つの DropDownLists が表示されます: 最初はすべて一覧表示、カテゴリの 2 つ目は、選択したカテゴリに属する製品を一覧表示中にします。 ユーザーは、最初の DropDownList から新しいカテゴリを選択してポストバックが発生したりする 2 番目の DropDownList を再バインドは、新しく選択したカテゴリに属しているこれらの製品が表示されます。 図 12、13 ショー`MasterDetailsDetails.aspx`ブラウザーで表示したときにアクションにします。


[![飲み物のカテゴリが選択されている場合、最初のページへのアクセス、](master-detail-filtering-with-two-dropdownlists-cs/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image34.png)

**図 12**: 飲み物のカテゴリが選択されている最初のページへのアクセス、ときに ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image36.png))


[![別のカテゴリを選択すると、新しいカテゴリの製品が表示されます。](master-detail-filtering-with-two-dropdownlists-cs/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image37.png)

**図 13**: さまざまなカテゴリが表示されます、新しいカテゴリの製品を選択する ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image39.png))


現在、 `productsByCategory` DropDownList を変更した場合は*いない*ポストバックが発生します。 ただし、選択した製品の詳細 (手順 3) を表示する DetailsView を追加したが発生する可能性へのポストバックします。 したがって、チェック ボックスを有効にする AutoPostBack から、 `productsByCategory` DropDownList のスマート タグです。


[![ProductsByCategory DropDownList の AutoPostBack 機能を有効にします。](master-detail-filtering-with-two-dropdownlists-cs/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image40.png)

**図 14**: AutoPostBack 機能を有効にする、 `productsByCategory` DropDownList ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>手順 3: を DetailsView を使用して、選択した製品の詳細を表示するには

最後の手順では、DetailsView で選択した製品の詳細を表示します。 これを追加する、DetailsView のページに次のように設定します。 その`ID`プロパティを`ProductDetails`、し、その新しい ObjectDataSource を作成します。 構成からそのデータを取得するには、この ObjectDataSource、`ProductsBLL`クラスの`GetProductByProductID(productID)`メソッドの選択した値を使用して、`ProductsByCategory`の DropDownList の値の*`productID`* パラメーター。


[![ProductsBLL クラスを使用します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image43.png)

**図 15**: 使用する、`ProductsBLL`クラス ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image45.png))


[![ObjectDataSource GetProductByProductID(productID) メソッドを使用して構成します。](master-detail-filtering-with-two-dropdownlists-cs/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image46.png)

**図 16**: 構成を使用する ObjectDataSource、`GetProductByProductID(productID)`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image48.png))


[![ProductsByCategory DropDownList から productID パラメーターの値をプルします。](master-detail-filtering-with-two-dropdownlists-cs/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image49.png)

**図 17**: プル、 *`productID`* からパラメーター値、 `ProductsByCategory` DropDownList ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image51.png))


DetailsView で表示可能なフィールドのいずれかを選択できます。 削除する選択した、 `ProductID`、 `SupplierID`、および`CategoryID`フィールドし順序を変更およびその他のフィールドを書式設定します。 さらを消去した DetailsView のアウト`Height`と`Width`プロパティ、指定されたサイズに制限があるのではなく、そのデータの表示を最適に必要な幅に拡大する DetailsView を許可します。 マークアップを完全には、以下が表示されます。


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample1.aspx)]

お試しに少し時間を取って、`MasterDetailsDetails.aspx`ブラウザーのページです。 一見すべてが、必要に応じて動作しますが、微妙な問題があることがあることがあります。 新しいカテゴリを選択すると、 `ProductsByCategory` DropDownList が更新され、選択したカテゴリにこれらの製品ですが、 `ProductDetails` DetailsView は引き続き以前の製品情報を表示します。 DetailsView は、選択したカテゴリに別の製品を選択するときに更新されます。 さらに場合ほど十分にテストすることがわかりますを継続的に新しいカテゴリを選択した場合 (から飲み物を選択するなど、 `Categories` DropDownList、し、調味料、し、ある) その他のすべてのカテゴリ選択により、 `ProductDetails`DetailsView を更新します。

この問題を具体化するには、具体的な例を見てみましょう。 飲み物のカテゴリが選択されているしに関連する製品が読み込まれる最初のページにアクセスすると、 `ProductsByCategory` DropDownList です。 Chai は選択した製品とその詳細に表示されます、 `ProductDetails` DetailsView、図 18 に示すようにします。


[![DetailsView で選択されている製品の詳細が表示されます。](master-detail-filtering-with-two-dropdownlists-cs/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image52.png)

**図 18**: DetailsView で、選択した製品の詳細が表示されます ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image54.png))


ポストバックが発生した調味料に飲み物のカテゴリの選択を変更した場合、 `ProductsByCategory` DropDownList が同様に、更新されるものの、DetailsView はまだ Chai の詳細を表示します。


[![以前選択されている製品の詳細については引き続き表示されます。](master-detail-filtering-with-two-dropdownlists-cs/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image55.png)

**図 19**: の以前選択されている製品の詳細については引き続き表示されます ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image57.png))


期待どおりに、DetailsView を更新リストから新しい製品を選択します。 製品を変更した後、新しいカテゴリを選択した場合、DetailsView もう一度更新しません。 新しい製品を選択するのではなく、新しいカテゴリを選択した場合に、DetailsView が更新されます。 世界中で起こっているここですか。

問題は、ページのライフ サイクルのタイミングの問題です。 ときに、レンダリングとしていくつかの手順を進め、ページが要求されます。 次の手順のいずれかで、ObjectDataSource がのいずれかを確認を制御、`SelectParameters`値が変更されました。 そのため、データの Web コントロールがバインドされている場合、ObjectDataSource は、その表示を更新する必要があることを認識します。 たとえば、新しいカテゴリを選択すると、 `ProductsByCategoryDataSource` ObjectDataSource がそのパラメーター値が変更されたことを検出し、 `ProductsByCategory` DropDownList の再バインド数自体には、選択したカテゴリの製品を取得します。

このような状況で発生する問題は、ObjectDataSources パラメーターの変更を確認するページ ライフ サイクルの段階が発生する*する前に*Web コントロール、関連するデータの再バインドします。 したがって、新しいカテゴリを選択するときに、 `ProductsByCategoryDataSource` ObjectDataSource では、そのパラメーターの値の変更を検出します。 によって使用される ObjectDataSource、 `ProductDetails` DetailsView、いませんただし、このような変更を加えたのため、 `ProductsByCategory` DropDownList はまだ再バインドにします。 ライフ サイクルの後で、 `ProductsByCategory` DropDownList の新しく選択したカテゴリの製品を取得、その ObjectDataSource を再バインド数します。 中に、 `ProductsByCategory` DropDownList の値が変更されて、`ProductDetails`以外のため、DetailsView には、その前の結果が表示されます。 DetailsView の ObjectDataSource が既にそのパラメーター値のチェックを実行します。 この連携では、図 20 で表したものです。


[![ProductsByCategory DropDownList 値が変更された ProductDetails DetailsView の ObjectDataSource によって変更を確認した後](master-detail-filtering-with-two-dropdownlists-cs/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image58.png)

**図 20**: `ProductsByCategory` DropDownList 値の変更後、 `ProductDetails` DetailsView の ObjectDataSource によって変更を確認 ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image60.png))


明示的に再バインドする必要があります。 この問題を解決する、`ProductDetails`後 DetailsView、 `ProductsByCategory` DropDownList はバインドされています。 これを行うを呼び出して、 `ProductDetails` DetailsView の`DataBind()`メソッドと、 `ProductsByCategory` DropDownList の`DataBound`イベントが発生します。 次のイベント ハンドラー コードを追加、`MasterDetailsDetails.aspx`ページの分離コード クラス (を参照してください、"[ObjectDataSource のパラメーターの値をプログラムによって設定](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)"イベント ハンドラーを追加する方法の詳細については)。


[!code-csharp[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample2.cs)]

この明示的な呼び出しの後、 `ProductDetails` DetailsView の`DataBind()`メソッドが追加されて、このチュートリアルが期待どおりに動作します。 図 21 ハイライトこれの変更は、以前の問題を修正できます。


[![ProductDetails DetailsView は明示的に更新されるときに、ProductsByCategory DropDownList のデータ バインドされたイベントの起動](master-detail-filtering-with-two-dropdownlists-cs/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image61.png)

**図 21**: `ProductDetails` DetailsView は明示的に更新されるときに、 `ProductsByCategory` DropDownList の`DataBound`イベントの起動 ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-two-dropdownlists-cs/_static/image63.png))


## <a name="summary"></a>まとめ

マスター/詳細レポートの理想的なユーザー インターフェイス要素として、DropDownList の機能が、マスター/詳細レコードの間に一対多のリレーションシップがある場合。 前のチュートリアルでは、1 つの DropDownList を使用して、選択したカテゴリによって表示される製品をフィルター処理する方法を説明しました。 このチュートリアルでは製品の GridView に置き換え、DropDownList を DetailsView、選択した製品の詳細を表示するために使用します。 このチュートリアルで説明する概念は、顧客、注文、発注品目などの複数の一対多リレーションシップに関連するデータ モデルに簡単に拡張できます。 一般に、一対多のリレーションシップの「一」のエンティティの各 DropDownList をいつでも追加できます。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Hilton Giesenow しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](master-detail-filtering-with-a-dropdownlist-cs.md)
> [次へ](master-detail-filtering-across-two-pages-cs.md)
