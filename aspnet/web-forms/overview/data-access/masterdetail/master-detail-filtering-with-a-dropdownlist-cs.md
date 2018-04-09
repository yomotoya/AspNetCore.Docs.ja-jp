---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
title: マスター/詳細のフィルター処理 (c#) DropDownList を |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでは、DropDownList コントロールおよび GridView で選択した項目の詳細のマスター レコードを表示する方法を会いしましょう。
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 53e659cc-eefb-40c1-a1dc-559481c99443
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 42a6a76b0b05045bed1ada227b7c32a51600b760
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>マスター/詳細 DropDownList (c#) によるフィルター処理
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_7_CS.exe)または[PDF のダウンロード](master-detail-filtering-with-a-dropdownlist-cs/_static/datatutorial07cs1.pdf)

> このチュートリアルでは、DropDownList コントロールおよび GridView で選択した項目の詳細のマスター レコードを表示する方法を会いしましょう。


## <a name="introduction"></a>はじめに

レポートの一般的な種類は、*マスター/詳細レポート*、一部の「マスター」のレコードのセットを表示することによって、レポートを開始するのです。 ユーザー、ドリルダウンできますマスターのレコードのいずれかにそれによってそのマスター レコードの"の詳細の表示" マスター/詳細レポートはレポートなどの一対多の関係を視覚化する最適な選択肢すべてのカテゴリを示す特定のカテゴリを選択し、関連付けられた製品を表示するユーザーを許可します。 さらに、マスター/詳細レポートは、特に「幅の広い」テーブル (多数の列があるもの) から詳細な情報を表示するのに役立ちます。 たとえば、「マスター」マスター/詳細レポートのレベルは、データベースにだけ製品名と単位の価格、製品を表示する場合がありますおよび特定の製品へのドリル ダウンは、製品に関するその他のフィールドを表示 (カテゴリ、供給業者、単位あたりの数量とで)。

マスター/詳細レポートの実装されている方法はたくさんあります。 これと次の 3 つのチュートリアルでは、マスター/詳細レポートは多岐に紹介します。 このチュートリアルでは思いますのマスター レコードを表示する方法、 [DropDownList コントロール](https://msdn.microsoft.com/library/dtx91y0z.aspx)と GridView で選択した項目の詳細。 具体的には、このチュートリアルのマスター/詳細レポートには、カテゴリおよび製品情報が表示されます。

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>手順 1: DropDownList でカテゴリを表示します。

マスター/詳細レポートは、表示される選択されたリスト項目の製品と、DropDownList のカテゴリを一覧表示が GridView でページ上にします。 Us, 事前最初のタスクは、DropDownList で表示されるカテゴリを次に、です。 開く、 `FilterByDropDownList.aspx`  ページで、`Filtering`フォルダー、DropDownList のページのデザイナーには、ツールボックスからドラッグし、設定、`ID`プロパティを`Categories`です。 次に、DropDownList のスマート タグからデータ ソースの選択 リンクをクリックします。 これにより、データ ソース構成ウィザードが表示されます。


[![DropDownList のデータ ソースを指定します。](master-detail-filtering-with-a-dropdownlist-cs/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image1.png)

**図 1**: DropDownList のデータ ソースを指定 ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-cs/_static/image3.png))


という名前の新しい ObjectDataSource を追加する`CategoriesDataSource`を呼び出す、`CategoriesBLL`クラスの`GetCategories()`メソッドです。


[![CategoriesDataSource をという名前の新しい ObjectDataSource を追加します。](master-detail-filtering-with-a-dropdownlist-cs/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image4.png)

**図 2**: 新しい ObjectDataSource という追加`CategoriesDataSource`([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-cs/_static/image6.png))


[![CategoriesBLL クラスを使用します。](master-detail-filtering-with-a-dropdownlist-cs/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image7.png)

**図 3**: 使用する、`CategoriesBLL`クラス ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-cs/_static/image9.png))


[![ObjectDataSource GetCategories() メソッドを使用して構成します。](master-detail-filtering-with-a-dropdownlist-cs/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image10.png)

**図 4**: 構成を使用する ObjectDataSource、`GetCategories()`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-cs/_static/image12.png))


DropDownList でどのようなデータ ソースのフィールドを表示するかと、これを指定する必要があります ObjectDataSource を構成した後、リスト項目の値として関連付けられている 1 つ必要があります。 `CategoryName`ディスプレイとしてフィールドと`CategoryID`各リスト項目の値として。


[![値として使用 CategoryID と CategoryName フィールドの DropDownList の表示があります。](master-detail-filtering-with-a-dropdownlist-cs/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image13.png)

**図 5**: DropDownList ディスプレイ、`CategoryName`フィールド`CategoryID`値として ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-cs/_static/image15.png))


レコードが挿入されます DropDownList コントロールがあるこの時点で、`Categories`テーブル (約 6 秒で行うすべて)。 図 6 は、ブラウザーで表示したときにこれまで、進行状況を示します。


[![ドロップダウン リストに現在のカテゴリを一覧表示します。](master-detail-filtering-with-a-dropdownlist-cs/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image16.png)

**図 6**: A ドロップダウン リスト、現在のカテゴリ ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-cs/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>手順 2: 製品 GridView の追加

そのマスター/詳細レポートの最後の手順では、選択したカテゴリに関連付けられている製品の一覧です。 これを実現する、GridView、ページを追加し、という名前の新しい ObjectDataSource を作成する`productsDataSource`です。 `productsDataSource`コントロールからそのデータを選り分けて、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッドです。


[![GetProductsByCategoryID(categoryID) 方法を選択します。](master-detail-filtering-with-a-dropdownlist-cs/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image19.png)

**図 7**: 選択、`GetProductsByCategoryID(categoryID)`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-cs/_static/image21.png))


このメソッドを選択すると、ObjectDataSource ウィザードの指示に従って us メソッドの値の*`categoryID`*パラメーター。 選択した値を使用する`categories`DropDownList 項目コントロールを処理するパラメーターのソースを設定する`Categories`です。


[![カテゴリの DropDownList の値に categoryID パラメーターを設定します。](master-detail-filtering-with-a-dropdownlist-cs/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image22.png)

**図 8**: 設定、 *`categoryID`*パラメーターの値を`Categories`DropDownList ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-cs/_static/image24.png))


すぐをブラウザーで作業の進行状況を確認します。 これらの製品が、選択したカテゴリに属している場合、最初のページへのアクセス、(飲み物) が表示されます (図 9) が DropDownList を変更すると、データが更新されません。 これは、ため、更新する GridView のポストバックが発生したときです。 これを行うには、2 つのオプション (うちどちらが必要、コードを記述) があります。

- **設定カテゴリの DropDownList の**[AutoPostBack プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**True に設定します。** (これを行う DropDownList のスマート タグで AutoPostBack を有効にするオプションをチェックしています。)DropDownList の選択されたときに、ポストバックがこれによりトリガー、ユーザーが項目を変更します。 したがって、ユーザーは、次のドロップダウン リストから新しいカテゴリを選択したときにポストバックが発生したり、GridView、新しく選択したカテゴリの製品で更新されます。 (これは、このチュートリアルで使用したアプローチです)。
- **DropDownList の横にあるボタン Web コントロールを追加します。** 設定の`Text`プロパティを更新または同様です。 この方法で、ユーザーは、新しいカテゴリを選択し、ボタンをクリックする必要があります。 ボタンをクリックし、ポストバックを発生させるが、選択したカテゴリの製品を一覧表示する GridView を更新します。

図 9 と 10 は、マスター/詳細レポートの動作を示しています。


[![最初のページへのアクセス、飲み物の製品が表示されます。](master-detail-filtering-with-a-dropdownlist-cs/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image25.png)

**図 9**: 最初のページへのアクセス、飲み物の製品が表示されます ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-cs/_static/image27.png))


[![GridView の更新、ポストバックを発生させる新しい製品 (生成) を自動的に選択します。](master-detail-filtering-with-a-dropdownlist-cs/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image28.png)

**図 10**: GridView の更新、ポストバックを発生させる新しい製品 (生成) を自動的に選択する ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-cs/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>「--カテゴリの選択-」リスト アイテムを追加します。

最初にアクセスすると、 `FilterByDropDownList.aspx` DropDownList の最初のリスト項目 (飲み物) が既定では、飲み物の製品を示す GridView で選択したカテゴリのページです。 代わりに、DropDownList 項目が必要な場合があります、最初のカテゴリの製品の表示を選択するのではなく次のように「- カテゴリの選択-」が表示されます。

次のドロップダウン リストに新しいリスト アイテムを追加するに [プロパティ] ウィンドウに移動し、内の省略記号をクリックして、`Items`プロパティです。 新しい一覧項目を追加、 `Text` 「--カテゴリの選択-」、および`Value``-1`です。


[![追加-カテゴリ--リスト アイテムの選択](master-detail-filtering-with-a-dropdownlist-cs/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image31.png)

**図 11**: 追加-カテゴリ--リスト アイテムの選択 ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-cs/_static/image33.png))


代わりに、次のドロップダウン リストに、次のマークアップを追加することでリスト項目を追加できます。

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample1.aspx)]

さらの DropDownList の制御を設定する必要があります`AppendDataBoundItems`True の場合に、手動で追加したリスト アイテムを上書きします、カテゴリは、ObjectDataSource から DropDownList にバインドするために`AppendDataBoundItems`True はありません。


![AppendDataBoundItems プロパティを True に設定します。](master-detail-filtering-with-a-dropdownlist-cs/_static/image34.png)

**図 12**: 設定、`AppendDataBoundItems`プロパティを True に


これらの変更後に「--カテゴリの選択-」オプションが選択されている最初のページにアクセスしたときと製品は表示されません。


[![最初のページ読み込みの製品が表示されません。](master-detail-filtering-with-a-dropdownlist-cs/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image35.png)

**図 13**: での初期ページ負荷なし製品が表示されます ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-cs/_static/image37.png))


製品が表示されていない場合、"--カテゴリ--"リスト アイテムの選択 が選択されているため理由がその値があるためには`-1`でデータベースに製品がいなくなると、`CategoryID`の`-1`します。 これは、動作する場合はこの時点で完了したら、! ただし、表示する場合は、*すべて*、カテゴリ、"--カテゴリ--"リスト アイテムの選択 を選択すると、戻り値を`ProductsBLL`クラスし、カスタマイズ、`GetProductsByCategoryID(categoryID)`メソッドを呼び出すので、`GetProducts()`メソッド場合渡されたで*`categoryID`*パラメーターが 0 より小さい。

[!code-csharp[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample2.cs)]

ここで使用される手法はすべてのサプライヤーの表示に使用したアプローチに似ていますに戻り、[宣言型のパラメーター](../basic-reporting/declarative-parameters-cs.md)が、この例を使用して、値は、チュートリアル`-1`を示すすべてのレコードをする必要がありますなく取得`null`です。 これは、ため、 *`categoryID`*のパラメーター、`GetProductsByCategoryID(categoryID)`メソッドが必要ですが、渡される整数値として宣言型のパラメーターのチュートリアルでは、文字列の入力パラメーターに渡していた一方です。

図 14 のスクリーン ショットに示します`FilterByDropDownList.aspx`「--カテゴリの選択-」オプションを選択するとします。 ここでは、既定では、すべての製品が表示され、ユーザーが特定のカテゴリを選択して、表示を絞ることができます。


[![一覧で、既定では、すべての製品](master-detail-filtering-with-a-dropdownlist-cs/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image38.png)

**図 14**: 一覧で、既定では、すべての製品 ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-cs/_static/image40.png))


## <a name="summary"></a>まとめ

階層的に関連するデータを表示するには、マスター/詳細レポート、元のユーザーは、階層の最上位からデータを参照するための開始し、詳細にドリル ダウンを使用してデータを表示する多くの場合、役立ちます。 このチュートリアルでは、選択したカテゴリの製品が表示された単純なマスター/詳細レポートを作成して調査します。 これは、カテゴリと、選択したカテゴリに属する製品の GridView の一覧については、DropDownList を使用して行われました。

[次のチュートリアル](master-detail-filtering-with-two-dropdownlists-cs.md)みましょう DropDownList インターフェイスの 1 つのステップさらに、2 つの DropDownLists を使用します。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

> [!div class="step-by-step"]
> [次へ](master-detail-filtering-with-two-dropdownlists-cs.md)
