---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: マスター/詳細のフィルター処理 (c#) DropDownList を |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでは、DropDownLists を使用して、'master' のレコードと、DataList displ に表示する単一の web ページにマスター/詳細レポートを表示する方法を確認しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: c84902ccf028c976246380abfaebb6a76c573603
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880675"
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>マスター/詳細 DropDownList (c#) によるフィルター処理
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe)または[PDF のダウンロード](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> このチュートリアルでは、「マスター」のレコードと「詳細」を表示する DataList 表示 DropDownLists を使用して単一の web ページにマスター/詳細レポートを表示する方法を確認します。


## <a name="introduction"></a>はじめに

GridView を使用する前に示したを最初に作成したマスター/詳細レポート[マスター/詳細のフィルター処理で、DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)チュートリアルでは、一部の「マスター」のレコードのセットを表示することによって開始します。 ユーザー、ドリルダウンできますマスターのレコードのいずれかにそれによってそのマスター レコードの"の詳細の表示" マスター/詳細レポートは、一対多の関係を視覚化し、特に「幅」のテーブル (多数の列があるもの) の詳細な情報を表示するための最適な選択肢です。 前のチュートリアルで、GridView と DetailsView コントロールを使用してマスター/詳細レポートを実装する方法おについて説明してきました。 このチュートリアルを次の 2 つの場合は、DataList でフォーカスが、これらの概念を再確認しますおし Repeater を代わりに制御します。

このチュートリアルでは、DataList に表示される [詳細] のレコードを含む、「マスター」のレコードを格納して DropDownList を使用してに紹介します。

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>手順 1: マスター/詳細のチュートリアル Web ページの追加

このチュートリアルを始める前にまずみましょうフォルダーおよびしなければならない、このチュートリアルを DataList およびリピータ コントロールを使用してマスター/詳細レポートを処理する次の 2 つの ASP.NET ページを追加します。 という名前のプロジェクトに新しいフォルダーを作成して開始`DataListRepeaterFiltering`です。 次に、すべてのマスター ページを使用するように構成して、このフォルダーに次の 5 つの ASP.NET ページを追加`Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![DataListRepeaterFiltering フォルダーを作成し、チュートリアルの ASP.NET ページを追加](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**図 1**: 作成、`DataListRepeaterFiltering`フォルダー チュートリアルの ASP.NET ページを追加


次に、開く、`Default.aspx`ページし、ドラッグ、`SectionLevelTutorialListing.ascx`からユーザー コントロール、`UserControls`デザイン画面上のフォルダーです。 作成した、このユーザー コントロール、[マスター ページとサイト ナビゲーション](../introduction/master-pages-and-site-navigation-cs.md)チュートリアルでは、サイト マップを列挙し、箇条書きリストの現在のセクションのチュートリアルが表示されます。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**図 2**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))


箇条書き一覧表示するために作成する、マスター/詳細チュートリアルでは、必要がありますサイト マップに追加します。 開く、`Web.sitemap`ファイルし、マークアップの後に「を表示するデータを DataList とリピータ」サイト マップ ノード、次のマークアップを追加します。

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]


![マップを更新するサイトに新しい ASP.NET ページを含める](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**図 3**: 新規の ASP.NET ページを含むサイト マップを更新


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>手順 2: DropDownList でカテゴリを表示します。

マスター/詳細レポートは、選択されたリスト項目の製品が表示されると、DropDownList のカテゴリを一覧、DataList 内のページ上にします。 Us, 事前最初のタスクは、DropDownList で表示されるカテゴリを次に、です。 開いて開始、 `FilterByDropDownList.aspx`  ページで、`DataListRepeaterFiltering`フォルダーと、ページのデザイナーには、ツールボックスからドラッグ DropDownList です。 次に、設定の DropDownList の`ID`プロパティを`Categories`です。 DropDownList のスマート タグから データ ソースのリンクをクリックし、作成という名前の新しい ObjectDataSource`CategoriesDataSource`です。


[![CategoriesDataSource をという名前の新しい ObjectDataSource を追加します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**図 4**: 新しい ObjectDataSource という追加`CategoriesDataSource`([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))


起動するように、新しい ObjectDataSource を構成、`CategoriesBLL`クラスの`GetCategories()`メソッドです。 DropDownList でどのようなデータ ソースのフィールドを表示するかと、これを指定する必要があります ObjectDataSource を構成した後は、各リスト項目の値として関連付けられている 1 つがあります。 `CategoryName`ディスプレイとしてフィールドと`CategoryID`各リスト項目の値として。


[![値として使用 CategoryID と CategoryName フィールドの DropDownList の表示があります。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**図 5**: DropDownList ディスプレイ、`CategoryName`フィールド`CategoryID`値として ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))


レコードが挿入されます DropDownList コントロールがあるこの時点で、`Categories`テーブル (約 6 秒で行うすべて)。 図 6 は、ブラウザーで表示したときにこれまで、進行状況を示します。


[![ドロップダウン リストに現在のカテゴリを一覧表示します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**図 6**: A ドロップダウン リスト、現在のカテゴリ ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>手順 2: 製品 DataList の追加

マスター/詳細レポートの最後の手順では、選択したカテゴリに関連付けられている製品の一覧です。 これを実現する、DataList をページに追加し、という名前の新しい ObjectDataSource を作成する`ProductsByCategoryDataSource`です。 `ProductsByCategoryDataSource`コントロールからそのデータの取得、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッドです。 このマスター/詳細レポートが読み取り専用であるために、INSERT、UPDATE、および DELETE のタブのオプション (なし) を選択します。


[![GetProductsByCategoryID(categoryID) 方法を選択します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**図 7**: 選択、`GetProductsByCategoryID(categoryID)`メソッド ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))


[次へ] をクリックすると、ObjectDataSource ウィザードの指示に従って us ソースの値の`GetProductsByCategoryID(categoryID)`メソッドの*`categoryID`* パラメーター。 選択した値を使用する`categories`DropDownList 項目コントロールを処理するパラメーターのソースを設定する`Categories`です。


[![カテゴリの DropDownList の値に categoryID パラメーターを設定します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**図 8**: 設定、 *`categoryID`* パラメーターの値を`Categories`DropDownList ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))


データ ソース構成ウィザードを完了すると、Visual Studio が自動的に生成、 `ItemTemplate` DataList の各データ フィールドの値と名前を表示するためです。 代わりに使用する DataList を強化してみましょう、`ItemTemplate`製品の名前、カテゴリ、供給業者、単位、およびと共に価格ごとの数だけを表示する、`SeparatorTemplate`挿入される、`<hr>`各項目の間の要素。 使用して、`ItemTemplate`の例から、 [DataList とリピータ コントロールのデータの表示](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md)が、チュートリアルでは、お気軽にどのようなテンプレートのマークアップ最も視覚に訴える検索を使用します。

DataList とその ObjectDataSource のマークアップがこれらの変更を行った後は、次のようにする必要がありますなります。

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

すぐをブラウザーで作業の進行状況を確認します。 最初のページへのアクセス、ときに、選択したカテゴリ (飲み物) に属しているこれらの製品が表示されます (図 9) が DropDownList を変更すると、データが更新されません。 これは、ため、更新する DataList ポストバックが発生したときです。 か、これを実現する設定の DropDownList の`AutoPostBack`プロパティを`true`またはボタンの Web コントロールをページに追加します。 このチュートリアルでは、選択したの DropDownList を設定する`AutoPostBack`プロパティを`true`です。

図 9 と 10 は、マスター/詳細レポートの動作を示しています。


[![最初のページへのアクセス、飲み物の製品が表示されます。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**図 9**: 最初のページへのアクセス、飲み物の製品が表示されます ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))


[![DataList の更新、ポストバックを発生させる新しい製品 (生成) を自動的に選択します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**図 10**: DataList の更新、ポストバックを発生させる新しい製品 (生成) を自動的に選択する ([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>「--カテゴリの選択-」リスト アイテムを追加します。

最初にアクセスすると、 `FilterByDropDownList.aspx` DropDownList の最初のリスト項目 (飲み物) が既定では、DataList で飲み物の製品が表示された選択したカテゴリのページです。 *マスター/詳細のフィルター処理で、DropDownList* DropDownList が既定でオンにし、選択すると表示を「--カテゴリの選択-」オプションを追加しましたチュートリアル*すべて*のデータベース内の製品です。 このようなアプローチでした管理しやすい、GridView では、製品を一覧表示するときに各製品の行が画面領域の量が少ないをかかりました。 DataList でただし、各製品の情報を消費画面の容量より大きなチャンクです。 まだみましょう「--カテゴリの選択-」オプションを追加して既定では、選択されていることがあるがすべての製品を表示することはなく選択すると、みましょう構成製品が表示されないように。

次のドロップダウン リストに新しいリスト アイテムを追加するに [プロパティ] ウィンドウに移動し、内の省略記号をクリックして、`Items`プロパティです。 新しい一覧項目を追加、 `Text` 「--カテゴリの選択-」、および`Value``0`です。


![追加します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**図 11**:「--カテゴリの選択-」リスト アイテムを追加


代わりに、次のドロップダウン リストに、次のマークアップを追加することでリスト項目を追加できます。

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

さらの DropDownList の制御を設定する必要があります`AppendDataBoundItems`に`true`ために設定されている場合`false`(既定)、任意の手動で追加したリストを上書きしますカテゴリは、ObjectDataSource から DropDownList にバインドします。項目。


![AppendDataBoundItems プロパティを True に設定します。](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**図 12**: 設定、`AppendDataBoundItems`プロパティを True に


値を選択した理由`0`「--カテゴリの選択-」リスト アイテムは、値は、システム内のカテゴリが存在しないため`0`、したがって製品レコードは返されません、"--カテゴリ--"リスト アイテムの選択 を選択するとします。 これには、すぐをブラウザーでページを参照してください。 図 13 に示す最初に、"--カテゴリ--"リスト アイテムの選択 が選択されているページを表示すると、製品は表示されません。


[![ときに、](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**図 13**: いいえ製品が表示される、"--カテゴリ--"リスト アイテムの選択 を選択すると、([フルサイズのイメージを表示するをクリックして](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))


表示ではなく場合*すべて*の製品の「--カテゴリの選択-」オプションを選択すると、値を使用して、`-1`代わりにします。 鋭いリーダーがそのチェックインを思い出してください、*マスター/詳細のフィルター処理で、DropDownList*に更新されたチュートリアル、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッドように場合、 *`categoryID`* 値`-1`が渡され、すべての製品レコードが返されました。

## <a name="summary"></a>まとめ

階層的に関連するデータを表示するには、マスター/詳細レポート、元のユーザーは、階層の最上位からデータを参照するための開始し、詳細にドリル ダウンを使用してデータを表示する多くの場合、役立ちます。 このチュートリアルでは、選択したカテゴリの製品が表示された単純なマスター/詳細レポートを作成して調査します。 これは、カテゴリと、選択したカテゴリに属する製品用 DataList の一覧については、DropDownList を使用して行われました。

次のチュートリアルでは、次の 2 つのページにわたってマスターと詳細レコードを分離することで紹介します。 最初のページでは、「マスター」のレコードの一覧が表示されます、詳細を表示するリンクを使用します。 リンクをクリックすると、2 番目のページは、選択したマスター レコードの詳細を表示するユーザーは whisk します。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>特別に感謝しています.

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客がものです。 Schmidt しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [次へ](master-detail-filtering-acess-two-pages-datalist-cs.md)
