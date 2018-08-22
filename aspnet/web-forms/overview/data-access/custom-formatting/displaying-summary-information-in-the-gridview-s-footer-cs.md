---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
title: 概要情報を表示する GridView のフッター (c#) |Microsoft Docs
author: rick-anderson
description: 概要情報は多くの場合、概要の行で、レポートの下部に表示されます。 GridView コントロールは、pr にセルを持つことのフッター行を含めることができます.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d50edc31-9286-4c6a-8635-be09e72752a4
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: 76df8ea925f4485b52090723b2f0a37b25f7e684
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826985"
---
<a name="displaying-summary-information-in-the-gridviews-footer-c"></a>GridView のフッター (c#) で概要情報を表示します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_15_CS.exe)または[PDF のダウンロード](displaying-summary-information-in-the-gridview-s-footer-cs/_static/datatutorial15cs1.pdf)

> 概要情報は多くの場合、概要の行で、レポートの下部に表示されます。 GridView コントロールをセルに、集計データを挿入しますできるプログラムでフッター行を含めることができます。 このチュートリアルでは、このフッター行に集計データを表示する方法がわかります。


## <a name="introduction"></a>はじめに

順序、および並べ替えのレベルで各製品の価格、在庫数、ユニットを表示すること、に加えて、ユーザーも、在庫数の合計数の平均価格などの集計情報を参考にして、その可能性があります。 このような概要情報は多くの場合、概要の行で、レポートの下部に表示されます。 GridView コントロールをセルに、集計データを挿入しますできるプログラムでフッター行を含めることができます。

このタスクで 3 つの課題を表示します。

1. GridView のフッター行を表示するを構成します。
2. 概要データを決定します。方法は計算の平均価格や在庫数の合計ですか。
3. フッター行の適切なセルに集計データを挿入します。

このチュートリアルでは、これらの課題を克服する方法がわかります。 具体的には、GridView に表示される、選択したカテゴリの製品をドロップダウン リスト内のカテゴリを一覧表示されたページを作成します。 GridView では、在庫と注文でそのカテゴリの製品の平均価格と単位の合計数が表示されるフッター行が含まれます。


[![GridView のフッター行に概要情報が表示されます。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image1.png)

**図 1**: GridView のフッター行のサマリー情報が表示されます ([フルサイズの画像を表示する をクリックします](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image3.png))。


前述の対象とした概念に基づいて、このチュートリアルでは、製品マスター/詳細のインターフェイスには、そのカテゴリに[マスター/詳細のフィルター処理で、DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)チュートリアル。 以前のチュートリアルに従って作業したされていない、する場合は、この 1 つを続行する前におようにしてください。

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>手順 1: DropDownList のカテゴリと製品の GridView の追加

GridView のフッターに概要情報を追加することに留意する前に、みましょうまずマスター/詳細レポートを単純に作成します。 この最初の手順が完了しましたとは、概要データを含める方法を紹介します。

開いて開始、`SummaryDataInFooter.aspx`ページで、`CustomFormatting`フォルダー。 DropDownList コントロールを追加し、設定、`ID`に`Categories`します。 次に、DropDownList のスマート タグから データ ソースのリンクをクリックし、という名前の新しい ObjectDataSource を追加することを選択`CategoriesDataSource`を呼び出す、`CategoriesBLL`クラスの`GetCategories()`メソッド。


[![CategoriesDataSource という名前の新しい ObjectDataSource を追加します。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image4.png)

**図 2**: 新しい ObjectDataSource という追加`CategoriesDataSource`([フルサイズの画像を表示する をクリックします](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image6.png))。


[![ObjectDataSource CategoriesBLL クラスの GetCategories() メソッドの呼び出しがあります。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image7.png)

**図 3**: ObjectDataSource 呼び出しがある、`CategoriesBLL`クラスの`GetCategories()`メソッド ([フルサイズの画像を表示する をクリックします](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image9.png))。


DropDownList のデータ ソースの構成ウィザードはどのようなデータ フィールドの値を指定しなければ表示するかと DropDownList のの値に対応する1つ、ObjectDataSourceを構成した後、ウィザードを返します`ListItem`秒。 `CategoryName`フィールドを表示し、使用、`CategoryID`値として。


[![テキストと、リスト項目の値として、区分名と [categoryid] フィールドをそれぞれ使用します](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image10.png)

**図 4**: 使用して、`CategoryName`と`CategoryID`フィールドに対する、`Text`と`Value`の`ListItem`s、それぞれ ([フルサイズの画像を表示する をクリックします](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image12.png))。


DropDownList がこの時点である (`Categories`) システムで、カテゴリを一覧表示します。 これで、選択したカテゴリに属しているこれらの製品を一覧表示する GridView を追加する必要があります。 ただし実行前に少し DropDownList のスマート タグで AutoPostBack を有効にするチェック ボックスをオンにします。 説明したように、*マスター/詳細のフィルター処理で、DropDownList* DropDownList を設定して、チュートリアル`AutoPostBack`プロパティを`true`ページ投稿されます戻る DropDownList の値が変更されるたびにします。 これによりに更新されますが、GridView 新しく選択したカテゴリの製品を表示します。 場合、`AutoPostBack`プロパティに設定されて`false`(既定)、ポストバックは発生しませんし、以下の製品が更新されないため、カテゴリを変更します。


[![DropDownList のスマート タグで有効にする AutoPostBack のチェック ボックスをオンします。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image13.png)

**図 5**: チェック ボックスを有効にする AutoPostBack DropDownList のスマート タグ ([フルサイズの画像を表示する をクリックします](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image15.png))。


選択したカテゴリの製品を表示するために、ページに GridView コントロールを追加します。 GridView の設定`ID`に`ProductsInCategory`という名前の新しい ObjectDataSource にバインド`ProductsInCategoryDataSource`します。


[![ProductsInCategoryDataSource という名前の新しい ObjectDataSource を追加します。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image16.png)

**図 6**: 新しい ObjectDataSource という追加`ProductsInCategoryDataSource`([フルサイズの画像を表示する をクリックします](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image18.png))。


呼び出すように ObjectDataSource を構成、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッド。


[![ObjectDataSource GetProductsByCategoryID(categoryID) メソッドの呼び出しがあります。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image19.png)

**図 7**: ObjectDataSource 呼び出しがある、`GetProductsByCategoryID(categoryID)`メソッド ([フルサイズの画像を表示する をクリックします](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image21.png))。


以降、`GetProductsByCategoryID(categoryID)`メソッドは、入力パラメーターでは、ウィザードの最後の手順では、パラメーター値のソースを指定できます。 取得したパラメーターを指定すると、選択したカテゴリからこれらの製品を表示するために、 `Categories` DropDownList します。


[![選択したカテゴリ DropDownList から categoryID パラメーターの値を取得します。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image22.png)

**図 8**: 取得、 *`categoryID`* カテゴリの選択した DropDownList からパラメーター値 ([フルサイズの画像を表示する をクリックします](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image24.png))。


ウィザードの完了後に、GridView では、BoundField を各製品のプロパティのがあります。 これらの BoundFields だけがクリーンアップみましょう、 `ProductName`、 `UnitPrice`、`UnitsInStock`と`UnitsOnOrder`BoundFields が表示されます。 残りの BoundFields に、フィールド レベルの設定を追加してみてください (書式設定など、`UnitPrice`通貨として)。 これらの変更を加えたら、GridView の宣言型マークアップを次のようになります。


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample1.aspx)]

この時点で、選択したカテゴリに属する製品の注文の名前、単価、在庫、および単位を示す完全に機能しているマスター/詳細レポートがあります。


[![選択したカテゴリ DropDownList から categoryID パラメーターの値を取得します。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image25.png)

**図 9**: 取得、 *`categoryID`* カテゴリの選択した DropDownList からパラメーター値 ([フルサイズの画像を表示する をクリックします](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image27.png))。


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>手順 2: GridView のフッターを表示します。

GridView コントロールは、ヘッダーとフッターの両方の行を表示できます。 値に応じてこれらの行が表示されます、`ShowHeader`と`ShowFooter`プロパティをそれぞれ`ShowHeader`既定では`true`と`ShowFooter`に`false`します。 GridView にフッターを単に含める次のように設定します。 その`ShowFooter`プロパティを`true`します。


[![GridView の ShowFooter プロパティを true に設定します。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image28.png)

**図 10**: 設定、GridView の`ShowFooter`プロパティを`true`([フルサイズの画像を表示する をクリックします](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image30.png))。


フッター行は、GridView; で定義されているフィールドの各セルを持つただし、これらのセルが既定では空です。 ブラウザーで、進行状況を表示する時間がかかります。 `ShowFooter`プロパティ設定するようになりました`true`GridView には、空のフッター行が含まれています。


[![GridView ここには、フッター行が含まれています。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image31.png)

**図 11**: GridView がフッター行が含まれています ([フルサイズの画像を表示する をクリックします](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image33.png))。


図 11 にフッター行意味はありません、白の背景があるためです。 作成しましょう、 `FooterStyle` CSS クラスの`Styles.css`を濃い赤の背景を指定し、構成、`GridView.skin`スキン ファイルで、 `DataWebControls` GridView にこの CSS クラスを割り当てるテーマ`FooterStyle`の`CssClass`プロパティ。 スキンとテーマを復習する場合を参照、 [、ObjectDataSource でデータを表示する](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)チュートリアル。

次の CSS クラスを追加することによって開始`Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample2.css)]

`FooterStyle` CSS クラスのスタイルに似ていますが、`HeaderStyle`クラスを`HeaderStyle`の背景色が濃いはらみ、そのテキストが太字のフォントで表示されます。 さらに、フッター テキストは右揃えはヘッダーのテキストが中央に配置します。

次に、この CSS クラスをすべて GridView のフッターに関連付けるを開き、`GridView.skin`ファイル、`DataWebControls`テーマとセット、`FooterStyle`の`CssClass`プロパティ。 この追加した後、ファイルのマークアップをようになります。


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample3.aspx)]

画面ショット下の例では、この変更はフッターよりはっきり目立ちます。


[![GridView のフッター行が赤の背景色になりました](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image34.png)

**図 12**: GridView のフッター行が赤の背景色になりました ([フルサイズの画像を表示する をクリックします](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image36.png))。


## <a name="step-3-computing-the-summary-data"></a>手順 3: コンピューティングの概要データ

GridView のフッターを表示、私たちが直面している次の課題は、集計データを計算する方法です。 この情報を集計を計算する 2 つの方法はあります。

1. SQL クエリを使用には、特定のカテゴリの概要データを計算するデータベースに追加のクエリを発行し可能性があります。 SQL には集計関数と共に数が含まれています、`GROUP BY`データの集計するデータを指定する句。 次の SQL クエリは、必要な情報を戻しています。  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample4.sql)]

    もちろん、このクエリから直接を発行する必要はありません、 `SummaryDataInFooter.aspx`  ページで、メソッドを作成してものではなく、 `ProductsTableAdapter` 、`ProductsBLL`します。
2. 説明したようには、GridView に追加されているが、この情報をコンピューティング[カスタム書式設定時にデータ](custom-formatting-based-upon-data-cs.md)チュートリアルでは、GridView の`RowDataBound`イベント ハンドラーは、GridView の後に追加される行ごとに 1 回発生そのされましたデータ バインドします。 このイベントのイベント ハンドラーを作成して、実行中保持できるし値の合計を集計します。 最後の後は、データ行が、合計と平均の計算に必要な情報がある GridView にバインドされました。

データベースとデータ アクセス層とビジネス ロジック層、概要の機能を実装するために必要な作業へのトリップを削減できますが、どちらの方法があれば十分とは通常、2 番目のアプローチを使用します。 このチュートリアルでは 2 番目のオプションを使用して集計途中経過の使用の追跡、`RowDataBound`イベント ハンドラー。

作成、`RowDataBound`デザイナーで、GridView を選択し、[プロパティ] ウィンドウから稲妻のアイコンをクリックとダブルクリック GridView のイベント ハンドラー、`RowDataBound`イベント。 これはという名前の新しいイベント ハンドラーを作成`ProductsInCategory_RowDataBound`で、`SummaryDataInFooter.aspx`ページの分離コード クラス。


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample5.cs)]

実行中の合計を維持するためには、イベント ハンドラーのスコープ外の変数を定義する必要があります。 次のようなページ レベルの 4 つの変数を作成します。

- `_totalUnitPrice`、型の `decimal`
- `_totalNonNullUnitPriceCount`、型の `int`
- `_totalUnitsInStock`、型の `int`
- `_totalUnitsOnOrder`、型の `int`

次に、各データ行の中のこれら 3 つの変数をインクリメントするコードを記述、`RowDataBound`イベント ハンドラー。


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample6.cs)]

`RowDataBound` DataRow を扱っていることを確認してイベント ハンドラーを起動します。 確立されると、`Northwind.ProductsRow`インスタンスにバインドされていただけ、`GridViewRow`オブジェクト`e.Row`変数に格納されている場合は、`product`します。 次に、実行中の合計変数が、現在の製品の対応する値がインクリメントされます (データベースが含まれていると仮定すると`NULL`値)。 両方の実行中の`UnitPrice`合計と非数`NULL``UnitPrice`平均価格はこれら 2 つの数値の商を記録します。

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>手順 4: フッターに概要データを表示します。

合計、集計データでは、最後の手順は、GridView のフッター行に表示するには。 このタスクでも実現できますでプログラムによって、`RowDataBound`イベント ハンドラー。 いることを思い出してください、`RowDataBound`を発生させるイベント ハンドラー*すべて*フッター行を含む、GridView にバインドされている行。 そのため、次のコードを使用してフッター行にデータを表示するイベント ハンドラーを補強することができます。


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample7.cs)]

すべてのデータ行を追加した後に、GridView のフッター行を追加するために私たちに集計途中経過の計算が完了したフッターに概要データを表示する準備ができていることを確信できます。 最後の手順では、次に、フッターのセルでこれらの値を設定するは。

特定のフッター セルでテキストを表示する`e.Row.Cells[index].Text = value`、場所、 `Cells` 0 から始まるインデックスを作成します。 次のコードでは、平均価格 (製品の数で割った値の合計価格) を計算し、在庫数および GridView のフッターの適切なセルの順序でユニット単位の合計数と共に表示します。


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample8.cs)]

図 13 は、このコードを追加した後にレポートを示します。 注方法、`ToString("c")`通貨のような形式に平均価格の概要情報します。


[![GridView のフッター行が赤の背景色になりました](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image37.png)

**図 13**: GridView のフッター行が赤の背景色になりました ([フルサイズの画像を表示する をクリックします](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image39.png))。


## <a name="summary"></a>まとめ

概要データを表示するは、一般的なレポート要件と GridView コントロールでは、簡単のフッター行にこのような情報を追加できます。 フッター行を表示するときに GridView の`ShowFooter`プロパティに設定されて`true`でプログラムによって設定セルのテキストを持つことができ、`RowDataBound`イベント ハンドラー。 概要データを計算できますもデータベースのクエリを再実行して ASP.NET ページの分離コード クラスの概要データをプログラムで計算するコードを使用しています。

このチュートリアルでは、GridView、DetailsView、FormView コントロールのカスタムの書式設定。 当社の調査で終了します。 挿入、更新、およびこれらの同じコントロールを使用してデータの削除のところ、次のチュートリアルが開始されます。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

> [!div class="step-by-step"]
> [前へ](using-the-formview-s-templates-cs.md)
> [次へ](custom-formatting-based-upon-data-vb.md)
