---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
title: マスター/詳細の DropDownList (c#) でフィルター処理 |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、DropDownList コントロールと GridView で選択した項目の詳細のマスター レコードを表示する方法がわかります。
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 53e659cc-eefb-40c1-a1dc-559481c99443
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
msc.type: authoredcontent
ms.openlocfilehash: a2d7a27a8bf9da365e4f48d7ca2d9d902ec4a5ba
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835233"
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>マスター/詳細の DropDownList (c#) でフィルター処理
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_7_CS.exe)または[PDF のダウンロード](master-detail-filtering-with-a-dropdownlist-cs/_static/datatutorial07cs1.pdf)

> このチュートリアルでは、DropDownList コントロールと GridView で選択した項目の詳細のマスター レコードを表示する方法がわかります。


## <a name="introduction"></a>はじめに

レポートの一般的な種類は、*マスター/詳細レポート*、「マスター」のレコードのいくつかのセットを表示することによって、レポートを開始するのです。 ユーザーしにドリルダウンできますいずれかのマスター レコード、それによってそのマスター レコードの"の詳細を表示します" マスター/詳細レポートなどの一対多のリレーションシップを視覚化するための最適な選択肢はすべてのカテゴリを示すと、特定のカテゴリを選択して、関連付けられている製品を表示するユーザーを報告します。 また、マスター/詳細レポートは、(多数の列があるもの)、特に「ワイド」テーブルから詳細な情報を表示する場合に便利です。 たとえば、マスター/詳細レポートの「マスター」のレベルは、データベースにだけ、製品名と単位、製品の価格を表示する場合があり、その他の製品のフィールドの表示は、特定の製品のドリル ダウン (カテゴリ、供給業者、単位あたりの数量となどの)。

マスター/詳細レポートの実装されている多くの方法はあります。 これと次の 3 つのチュートリアルでは、マスター/詳細レポートのさまざまなことについて説明します。 このチュートリアルでのマスター レコードを表示する方法を見ていきます、 [DropDownList コントロール](https://msdn.microsoft.com/library/dtx91y0z.aspx)と GridView で選択した項目の詳細。 具体的には、このチュートリアルのマスター/詳細レポートには、カテゴリおよび製品の情報が表示されます。

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>手順 1: DropDownList でカテゴリを表示します。

マスター/詳細レポートは、選択されたリスト項目の製品が表示されると、DropDownList でカテゴリを一覧の後半で、GridView でページ。 前に、最初のタスクは、DropDownList に表示されるカテゴリにし、です。 開く、`FilterByDropDownList.aspx`ページで、`Filtering`フォルダーは、ページのデザイナーには、ツールボックスから DropDownList にドラッグし、設定、`ID`プロパティを`Categories`します。 次に、DropDownList のスマート タグからのデータ ソースの選択のリンクをクリックします。 これにより、データ ソース構成ウィザードが表示されます。


[![DropDownList のデータ ソースを指定します。](master-detail-filtering-with-a-dropdownlist-cs/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image1.png)

**図 1**: DropDownList のデータ ソースを指定 ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-cs/_static/image3.png))。


という名前の新しい ObjectDataSource を追加することも`CategoriesDataSource`を呼び出す、`CategoriesBLL`クラスの`GetCategories()`メソッド。


[![CategoriesDataSource という名前の新しい ObjectDataSource を追加します。](master-detail-filtering-with-a-dropdownlist-cs/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image4.png)

**図 2**: 新しい ObjectDataSource という追加`CategoriesDataSource`([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-cs/_static/image6.png))。


[![CategoriesBLL クラスを使用します。](master-detail-filtering-with-a-dropdownlist-cs/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image7.png)

**図 3**: 使用する、`CategoriesBLL`クラス ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-cs/_static/image9.png))。


[![ObjectDataSource GetCategories() メソッドを使用して構成します。](master-detail-filtering-with-a-dropdownlist-cs/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image10.png)

**図 4**: 構成に使用する ObjectDataSource、`GetCategories()`メソッド ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-cs/_static/image12.png))。


DropDownList にどのようなデータ ソースのフィールドを表示するかを指定する必要があります ObjectDataSource を構成した後は、リスト項目の値として関連付けられている 1 つがあります。 `CategoryName`フィールドとして表示し、`CategoryID`各リスト項目の値として。


[![値として使用 CategoryID と CategoryName フィールド DropDownList 表示があります。](master-detail-filtering-with-a-dropdownlist-cs/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image13.png)

**図 5**: DropDownList の表示、`CategoryName`フィールド`CategoryID`値として ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-cs/_static/image15.png))。


この時点でのレコードが設定された DropDownList コントロールがある、`Categories`テーブル (すべて約 6 秒間で行われます)。 図 6 は、ブラウザーで表示したときにこれまで、進行状況を示します。


[![ドロップダウンには、現在のカテゴリが表示されます。](master-detail-filtering-with-a-dropdownlist-cs/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image16.png)

**図 6**: A ドロップダウン リスト、現在のカテゴリ ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-cs/_static/image18.png))。


## <a name="step-2-adding-the-products-gridview"></a>手順 2: 製品の GridView を追加します。

マスター/詳細レポートの最後のステップでは、選択したカテゴリに関連付けられている製品を一覧表示します。 これを実現するには、ページに GridView を追加という名前の新しい ObjectDataSource を作成して`productsDataSource`します。 `productsDataSource`コントロールからそのデータの数を減らします、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッド。


[![GetProductsByCategoryID(categoryID) メソッドを選択します。](master-detail-filtering-with-a-dropdownlist-cs/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image19.png)

**図 7**: 選択、`GetProductsByCategoryID(categoryID)`メソッド ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-cs/_static/image21.png))。


メソッドの値のこと、ObjectDataSource ウィザードでこのメソッドを選択すると、入力*`categoryID`* パラメーター。 選択した値を使用する`categories`DropDownList 項目コントロールを処理するパラメーターのソースを設定する`Categories`します。


[![CategoryID パラメーター カテゴリの DropDownList の値に設定されます。](master-detail-filtering-with-a-dropdownlist-cs/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image22.png)

**図 8**: 設定、 *`categoryID`* パラメーターの値を`Categories`DropDownList ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-cs/_static/image24.png))。


ブラウザーで進行状況を確認する時間がかかります。 これらの製品が選択したカテゴリに属している場合、最初のページにアクセスして、(飲み物) が表示されます (図 9) が、データが更新されない、DropDownList を変更します。 これは、ため、更新する GridView ポストバックが発生したときです。 これを行うには、2 つのオプション (うちどちらが必要、コードを記述) があります。

- **設定カテゴリ DropDownList の**[AutoPostBack プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**を True にします。** (これを行う DropDownList のスマート タグで AutoPostBack を有効にするオプションをオンにしています。)DropDownList の選択されるたびに、ポストバックがこれによりトリガー、ユーザーが項目を変更します。 そのため、次のドロップダウン リストから新しいカテゴリを選択すると、ポストバックが発生したりして GridView が新しく選択したカテゴリの製品で更新されます。 (これは、このチュートリアルで使用したアプローチです)。
- **DropDownList の横にあるボタンの Web コントロールを追加します。** 設定の`Text`などプロパティを更新します。 この方法により、ユーザーは、新しいカテゴリを選択し、ボタンをクリックする必要があります。 ボタンをクリックして、ポストバックを発生させるし、選択したカテゴリの製品を一覧表示する GridView を更新します。

図 9 と 10 は、マスター/詳細レポートの動作を示しています。


[![まず、ページにアクセスして、飲み物の製品が表示されます。](master-detail-filtering-with-a-dropdownlist-cs/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image25.png)

**図 9**: 最初のページにアクセスして、飲み物の製品が表示されます ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-cs/_static/image27.png))。


[![新しい製品 (生成) を選択すると、自動的にポストバックが発生する、GridView を更新しています](master-detail-filtering-with-a-dropdownlist-cs/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image28.png)

**図 10**: GridView を更新、ポストバックを発生させる (生成) の新しい製品を選択すると、自動的に ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-cs/_static/image30.png))。


## <a name="adding-a----choose-a-category----list-item"></a>「- カテゴリを選択--」リスト アイテムを追加します。

最初にアクセスすると、 `FilterByDropDownList.aspx` DropDownList の最初のリスト項目 (飲み物) は、gridview、飲料を示す、既定で選択したカテゴリのページします。 代わりに、DropDownList 項目が次に必要な場合があります、最初のカテゴリの製品の表示を選択するのではなく、「- カテゴリを選択--」ようなものは述べています。

DropDownList に新しいリスト アイテムを追加する [プロパティ] ウィンドウに移動し、省略記号をクリックして、`Items`プロパティ。 新しいリスト アイテムを追加、 `Text` 「--カテゴリを選択--」および`Value``-1`します。


[![追加-カテゴリ - リスト項目の選択](master-detail-filtering-with-a-dropdownlist-cs/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image31.png)

**図 11**: 追加---カテゴリを選択するには、リスト項目 ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-cs/_static/image33.png))。


または、DropDownList に次のマークアップを追加して、リスト項目を追加できます。

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample1.aspx)]

さらに、DropDownList コントロールを設定する必要があります`AppendDataBoundItems`ObjectDataSource からカテゴリが、DropDownList にバインドされている場合場合、手動で追加したリスト項目が上書きされますので true `AppendDataBoundItems` True はありません。


![AppendDataBoundItems プロパティを True に設定します。](master-detail-filtering-with-a-dropdownlist-cs/_static/image34.png)

**図 12**: 設定、`AppendDataBoundItems`プロパティを True に


これらの変更後「--カテゴリを選択--」オプションが選択されている最初のページにアクセスすると、製品は表示されません。


[![初期ページの読み込み時の製品は表示されません。](master-detail-filtering-with-a-dropdownlist-cs/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image35.png)

**図 13**: で、初期ページ読み込みいいえ製品が表示されます ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-cs/_static/image37.png))。


製品が表示されないときに「- カテゴリを選択--」リスト アイテムが選択されているため、理由は、値は`-1`でデータベースに製品がないと、`CategoryID`の`-1`します。 これは、この時点で完了し、目的の動作です。 場合、 ただし、表示する場合*すべて*のカテゴリの「- カテゴリを選択--」リスト アイテムを選択するに戻り、`ProductsBLL`クラスし、カスタマイズ、`GetProductsByCategoryID(categoryID)`メソッドを呼び出すため、`GetProducts()`メソッド場合渡されたで*`categoryID`* パラメーターが 0 未満。

[!code-csharp[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample2.cs)]

ここで使用される手法はすべての仕入先の表示に使用したアプローチに似ていますに戻り、[宣言パラメーター](../basic-reporting/declarative-parameters-cs.md)チュートリアル, が、この例の値を使用して`-1`をすべてのレコードがあることを示します対照的に取得`null`します。 これは、ため、 *`categoryID`* のパラメーター、`GetProductsByCategoryID(categoryID)`メソッドには、渡される整数値としては、宣言型のパラメーターのチュートリアルでは、文字列入力パラメーターに渡していた。

図 14 のスクリーン ショットを示しています。 `FilterByDropDownList.aspx` "--カテゴリを選択--"オプションを選択するとします。 ここでは、既定では、すべての製品が表示され、ユーザーは、特定のカテゴリを選択して、表示を絞り込むことができます。


[![一覧で、既定ではすべての製品](master-detail-filtering-with-a-dropdownlist-cs/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image38.png)

**図 14**: 一覧で、既定ではすべての製品 ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-cs/_static/image40.png))。


## <a name="summary"></a>まとめ

階層的に関連するデータを表示するには、マスター/詳細レポートから、ユーザーは、階層の最上位からデータを参照するために開始し、詳細にドリル ダウンを使用してデータを表示する多くの場合、役立ちます。 このチュートリアルでは、選択したカテゴリの製品を示す単純なマスター/詳細レポートの作成について確認しました。 これは、カテゴリと、選択したカテゴリに属する製品の GridView の一覧については、DropDownList を使用して行われました。

[次のチュートリアル](master-detail-filtering-with-two-dropdownlists-cs.md)みましょう DropDownList インターフェイスの 1 つ手順さらに、2 つの Dropdownlist を使用します。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

> [!div class="step-by-step"]
> [次へ](master-detail-filtering-with-two-dropdownlists-cs.md)
