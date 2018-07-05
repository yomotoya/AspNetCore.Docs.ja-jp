---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: マスター/詳細の DropDownList (c#) でフィルター処理 |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、Dropdownlist を使用して 'master' のレコードと、DataList displ に表示する 1 つの web ページでマスター/詳細レポートを表示する方法を見る.
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: ed2631e49786c81075099cca6941d98ba3b67e37
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818253"
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>マスター/詳細の DropDownList (c#) でフィルター処理
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe)または[PDF のダウンロード](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> このチュートリアルでは「マスター」のレコードと「詳細」を表示する DataList 表示 Dropdownlist を使用して単一の web ページでマスター/詳細レポートを表示する方法がわかります。


## <a name="introduction"></a>はじめに

GridView を使用する前に示したを最初に作成したマスター/詳細レポート[マスター/詳細のフィルター処理で、DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) 「マスター」のレコードのいくつかのセットを表示することによってチュートリアルを開始します。 ユーザーしにドリルダウンできますいずれかのマスター レコード、それによってそのマスター レコードの"の詳細を表示します" マスター/詳細レポートは、一対多の関係を視覚化するため、特に「ワイド」のテーブル (多数の列があるもの) からの詳細情報を表示するための最適な選択肢です。 前のチュートリアルで GridView および DetailsView コントロールを使用してマスター/詳細レポートを実装する方法について紹介しました。 このチュートリアルと次の 2 つの場合では、DataList を使用してフォーカスですが、これらの概念を再確認しますし、Repeater を代わりに制御します。

このチュートリアルでは、DataList で表示される"details"レコードで、「マスター」のレコードを格納して DropDownList を使用して紹介します。

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>手順 1: マスター/詳細チュートリアル Web ページの追加

このチュートリアルを始める前にまず少し探ってみましょうフォルダーとこのチュートリアルの次の 2 つ DataList と Repeater コントロールを使用してマスター/詳細レポートを処理する必要があります、ASP.NET ページに追加します。 という名前のプロジェクトで新しいフォルダーを作成して開始`DataListRepeaterFiltering`します。 次に、次の 5 つの ASP.NET ページをこのフォルダーは、すべてのマスター ページを使用するように構成する追加`Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![DataListRepeaterFiltering フォルダーを作成し、チュートリアルの ASP.NET ページを追加します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**図 1**: 作成、`DataListRepeaterFiltering`フォルダー チュートリアル ASP.NET ページを追加


次に、開く、`Default.aspx`ページし、ドラッグ、`SectionLevelTutorialListing.ascx`からユーザー コントロール、`UserControls`デザイン サーフェイスにフォルダー。 作成した、このユーザー コントロール、[マスター ページとサイト ナビゲーション](../introduction/master-pages-and-site-navigation-cs.md)チュートリアルでは、サイト マップの列挙し、箇条書きリストに現在のセクションから、チュートリアルを表示します。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**図 2**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))。


箇条書きリストに表示するために作成します、マスター/詳細チュートリアルでは、必要がありますサイト マップに追加します。 開く、`Web.sitemap`ファイルを開き、"を表示するデータを DataList と Repeater"サイト マップ ノードのマークアップの後に次のマークアップを追加します。

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]


![新しい ASP.NET ページは、サイト マップを更新します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**図 3**: 新しい ASP.NET ページは、サイト マップを更新


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>手順 2: DropDownList でカテゴリを表示します。

マスター/詳細レポートは、DropDownList、内のカテゴリを一覧表示、選択されたリスト項目の製品が表示されますが、DataList でページ上にします。 前に、最初のタスクは、DropDownList に表示されるカテゴリにし、です。 開いて開始、`FilterByDropDownList.aspx`ページで、`DataListRepeaterFiltering`フォルダーと、ページのデザイナーには、ツールボックスから、DropDownList をドラッグします。 DropDownList を次に、設定`ID`プロパティを`Categories`します。 DropDownList のスマート タグから データ ソースのリンクをクリックし、作成という名前の新しい ObjectDataSource`CategoriesDataSource`します。


[![CategoriesDataSource という名前の新しい ObjectDataSource を追加します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**図 4**: 新しい ObjectDataSource という追加`CategoriesDataSource`([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))。


新しい ObjectDataSource を呼び出すように構成、`CategoriesBLL`クラスの`GetCategories()`メソッド。 DropDownList にどのようなデータ ソースのフィールドを表示するかを指定する必要があります ObjectDataSource を構成した後は、各リスト項目の値として関連付けられている 1 つ必要があります。 `CategoryName`フィールドとして表示し、`CategoryID`各リスト項目の値として。


[![値として使用 CategoryID と CategoryName フィールド DropDownList 表示があります。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**図 5**: DropDownList の表示、`CategoryName`フィールド`CategoryID`値として ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))。


この時点でのレコードが設定された DropDownList コントロールがある、`Categories`テーブル (すべて約 6 秒間で行われます)。 図 6 は、ブラウザーで表示したときにこれまで、進行状況を示します。


[![ドロップダウンには、現在のカテゴリが表示されます。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**図 6**: A ドロップダウン リスト、現在のカテゴリ ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))。


## <a name="step-2-adding-the-products-datalist"></a>手順 2: 製品 DataList を追加します。

マスター/詳細レポートの最後の手順では、選択したカテゴリに関連付けられている製品を一覧表示します。 これを実現するには、ページに、DataList を追加という名前の新しい ObjectDataSource を作成して`ProductsByCategoryDataSource`します。 `ProductsByCategoryDataSource`コントロールからそのデータの取得、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッド。 このマスター/詳細レポートが読み取り専用であるために、INSERT、UPDATE、および DELETE の各タブのオプション (なし) を選択します。


[![GetProductsByCategoryID(categoryID) メソッドを選択します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**図 7**: 選択、`GetProductsByCategoryID(categoryID)`メソッド ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))。


値のソースの私たち ObjectDataSource ウィザードで [次へ] をクリックすると、入力、`GetProductsByCategoryID(categoryID)`メソッドの*`categoryID`* パラメーター。 選択した値を使用する`categories`DropDownList 項目コントロールを処理するパラメーターのソースを設定する`Categories`します。


[![CategoryID パラメーター カテゴリの DropDownList の値に設定されます。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**図 8**: 設定、 *`categoryID`* パラメーターの値を`Categories`DropDownList ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))。


データ ソース構成ウィザードを完了すると、Visual Studio が自動的に生成、 `ItemTemplate` DataList 名前と各データ フィールドの値を表示するのです。 代わりに使用する DataList を強化しましょう、`ItemTemplate`製品の名前、カテゴリ、供給業者、単位、およびと共に価格ごとの数だけを表示する、`SeparatorTemplate`挿入される、`<hr>`各項目の間の要素。 使用して、`ItemTemplate`で例から、 [DataList と Repeater コントロールでデータを表示する](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md)チュートリアルが自由にどのようなテンプレート マークアップ最も視覚に訴える検索を使用します。

これらの変更を行った後、DataList、およびその ObjectDataSource のマークアップに次のようななる必要があります。

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

ブラウザーで進行状況を確認する時間がかかります。 最初のページにアクセスして、ときに (飲み物) 選択したカテゴリに属しているこれらの製品が表示されます (図 9) が、DropDownList を変更するデータは更新されません。 これは、ため、更新する DataList ポストバックが発生したときです。 DropDownList を設定できるか、これを実現する`AutoPostBack`プロパティを`true`またはボタンの Web コントロールをページに追加します。 このチュートリアルでは、DropDownList の設定を選択した`AutoPostBack`プロパティを`true`します。

図 9 と 10 は、マスター/詳細レポートの動作を示しています。


[![まず、ページにアクセスして、飲み物の製品が表示されます。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**図 9**: 最初のページにアクセスして、飲み物の製品が表示されます ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))。


[![DataList の更新、ポストバックを発生させる新しい製品 (生成) を自動的に選択します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**図 10**: 新しい製品 (生成) を選択すると、自動的にポストバックが発生する、DataList を更新しています ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))。


## <a name="adding-a----choose-a-category----list-item"></a>「- カテゴリを選択--」リスト アイテムを追加します。

最初にアクセスすると、 `FilterByDropDownList.aspx` DropDownList の最初のリスト項目 (飲み物) が既定では、DataList で飲み物の製品を表示して選択したカテゴリのページします。 *マスター/詳細のフィルター処理で、DropDownList*チュートリアルが既定で選択されていると、選択すると表示し、DropDownList に「- カテゴリを選択--」オプションを追加しました*すべて*のデータベース内の製品です。 このアプローチ管理しやすいときに、GridView では、製品の一覧を表示するように各製品の行に少量の実際の画面。 DataList、ただし、各製品の情報を使用、画面のはるかに大きいチャンクします。 まだみましょう「--カテゴリを選択--」オプションを追加して、既定で選択されていることがあるがすべての製品を表示することではなく選択すると、みましょう構成製品が表示されないように。

DropDownList に新しいリスト アイテムを追加する [プロパティ] ウィンドウに移動し、省略記号をクリックして、`Items`プロパティ。 新しいリスト アイテムを追加、 `Text` 「--カテゴリを選択--」および`Value``0`します。


![追加します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**図 11**:「--カテゴリを選択--」リスト アイテムを追加


または、DropDownList に次のマークアップを追加して、リスト項目を追加できます。

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

さらに、DropDownList コントロールを設定する必要があります`AppendDataBoundItems`に`true`ために設定されている場合`false`(既定)、ObjectDataSource からカテゴリが、DropDownList にバインドされている場合、手動で追加したリストが上書きされます項目。


![AppendDataBoundItems プロパティを True に設定します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**図 12**: 設定、`AppendDataBoundItems`プロパティを True に


値を選択した理由`0`「--カテゴリを選択--」リストの項目は、値は、システム内のカテゴリが存在しないため`0`、したがって製品レコードは返されません「--カテゴリを選択--」リスト アイテムを選択します。 これを確認する少しブラウザーを使用してページを参照してください。 図 13 に示す最初に「- カテゴリを選択--」リスト アイテムが選択されているページを表示して、製品は表示されません。


[![ときに、](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**図 13**:「--カテゴリを選択--」リスト アイテムを選択すると、いいえ製品が表示されます ([フルサイズの画像を表示する をクリックします](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))。


表示ではなく場合*すべて*製品「- カテゴリを選択--」オプションを選択するでの値が使用`-1`代わりにします。 鋭い読者ならがそのチェックインを思い出してください、*マスター/詳細のフィルター処理で、DropDownList*更新されたチュートリアル、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッドように場合、 *`categoryID`* 値`-1`レコードが返されたすべての製品に渡されました。

## <a name="summary"></a>まとめ

階層的に関連するデータを表示するには、マスター/詳細レポートから、ユーザーは、階層の最上位からデータを参照するために開始し、詳細にドリル ダウンを使用してデータを表示する多くの場合、役立ちます。 このチュートリアルでは、選択したカテゴリの製品を示す単純なマスター/詳細レポートの作成について確認しました。 これは、カテゴリと、DataList、選択したカテゴリに属する製品の一覧については、DropDownList を使用して行われました。

次のチュートリアルでは、2 つのページ間でマスターと詳細レコードを分離することで紹介します。 最初のページでは、「マスター」のレコードの一覧が表示されます、詳細を表示するリンクを含む。 リンクをクリックすると、ユーザーに 2 番目のページでは、選択したマスター レコードの詳細が表示されますが whisk は。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別に感謝しています.

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Randy Schmidt でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [次へ](master-detail-filtering-acess-two-pages-datalist-cs.md)
