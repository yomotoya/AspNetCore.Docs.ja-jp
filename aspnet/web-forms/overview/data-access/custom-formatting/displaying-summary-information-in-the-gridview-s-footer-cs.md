---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
title: "GridView のフッター (c#) で概要情報の表示 |Microsoft ドキュメント"
author: rick-anderson
description: "概要情報は多くの場合、概要の行でのレポートの下部に表示されます。 GridView コントロールは、pr にセルを持つことのフッター行を含めることができます."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d50edc31-9286-4c6a-8635-be09e72752a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: 0d3df976181a4641dbfffe77875989c77ece059d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="displaying-summary-information-in-the-gridviews-footer-c"></a>GridView のフッター (c#) で概要情報を表示します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_15_CS.exe)または[PDF のダウンロード](displaying-summary-information-in-the-gridview-s-footer-cs/_static/datatutorial15cs1.pdf)

> 概要情報は多くの場合、概要の行でのレポートの下部に表示されます。 GridView コントロールがセルに、集計データを挿入おできますプログラムでフッター行を含めることができます。 このチュートリアルでは、このフッター行に集計データを表示する方法を会いしましょう。


## <a name="introduction"></a>はじめに

順序、および発注レベルには、各製品の価格、在庫、ユニット数を表示、だけでなく、ユーザーも平均価格、在庫の合計数などの集計情報に興味に可能性があります。 このような概要情報は多くの場合、概要の行でのレポートの下部に表示されます。 GridView コントロールがセルに、集計データを挿入おできますプログラムでフッター行を含めることができます。

このタスクは、3 つの課題にことを示します。

1. そのフッター行を表示する GridView の構成
2. 概要データを決定します。どのように計算手順を実行お平均価格または商品の在庫数の合計ですか。
3. フッター行の該当するセルに集計データを挿入します。

このチュートリアルではこれらの課題を解決する方法が表示されます。 具体的には、GridView に表示される、選択したカテゴリの製品とドロップダウン リスト内のカテゴリを一覧するページを作成します。 GridView は、在庫と注文でそのカテゴリの製品の平均価格と単位の合計数を表示するフッター行が含まれます。


[![GridView のフッター行に概要情報が表示されます。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image1.png)

**図 1**: GridView のフッター行に概要情報が表示されます ([フルサイズのイメージを表示するをクリックして](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image3.png))


このチュートリアルでは、製品のマスター/詳細インターフェイスには、そのカテゴリが、以前に説明する概念に基づいて構築[マスター/詳細のフィルター処理で、DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)チュートリアルです。 されていない、以前のチュートリアルで作業した場合、を次に続行する前に実行してください。

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>手順 1: 製品 GridView とカテゴリの DropDownList の追加

GridView のフッターに概要情報を追加するには、自分自身をに関する、前に構築してみましょう最初だけで、マスター/詳細レポート。 この最初の手順を完了していると概要データを含める方法に紹介します。

開いて開始、 `SummaryDataInFooter.aspx`  ページで、`CustomFormatting`フォルダーです。 DropDownList コントロールを追加し、設定、`ID`に`Categories`です。 次の DropDownList のスマート タグから データ ソースのリンクをクリックし、という名前の新しい ObjectDataSource を追加することを選択`CategoriesDataSource`を呼び出す、`CategoriesBLL`クラスの`GetCategories()`メソッドです。


[![CategoriesDataSource をという名前の新しい ObjectDataSource を追加します。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image4.png)

**図 2**: 新しい ObjectDataSource という追加`CategoriesDataSource`([フルサイズのイメージを表示するをクリックして](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image6.png))


[![CategoriesBLL クラスの GetCategories() メソッドを呼び出す ObjectDataSource があります。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image7.png)

**図 3**: ObjectDataSource 呼び出しがある、`CategoriesBLL`クラスの`GetCategories()`メソッド ([フルサイズのイメージを表示するをクリックして](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image9.png))


DropDownList のデータ ソースの構成が元となるいただくためにどのようなデータ フィールドの値を指定してウィザードを表示するかと DropDownList のの値に対応する1つ、ObjectDataSourceを構成した後、ウィザードを返します`ListItem` s。 `CategoryName`フィールドを表示し、使用、`CategoryID`値として。


[![テキストと、リスト項目の値として、区分名 と [categoryid] フィールドをそれぞれ使用します。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image10.png)

**図 4**: を使用して、`CategoryName`と`CategoryID`フィールドに対する、`Text`と`Value`の`ListItem`s、それぞれ ([フルサイズのイメージを表示するをクリックして](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image12.png))


DropDownList がこの時点である (`Categories`) システムで、カテゴリを一覧表示します。 これを選択したカテゴリに属しているこれらの製品を一覧表示する GridView を追加する必要があります。 行う前、ただし、にすぐ DropDownList のスマート タグで AutoPostBack を有効にするチェック ボックスをオンにします。 説明したように、*マスター/詳細のフィルター処理で、DropDownList* DropDownList を設定して、チュートリアル`AutoPostBack`プロパティを`true`ページが通知されます戻る DropDownList 値が変更されるたびにします。 これにより、更新する GridView の新しく選択したカテゴリの製品が表示されます。 場合、`AutoPostBack`プロパティに設定されている`false`(既定)、ポストバックを発生しませんカテゴリを変更して表示されている製品が更新されないためです。


[![チェック ボックスを有効にする AutoPostBack DropDownList のスマート タグ](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image13.png)

**図 5**: チェック ボックスを有効にする AutoPostBack DropDownList のスマート タグ ([フルサイズのイメージを表示するをクリックして](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image15.png))


GridView コントロールをページに、選択したカテゴリの製品を表示するために追加します。 GridView の設定`ID`に`ProductsInCategory`という名前の新しい ObjectDataSource にバインド`ProductsInCategoryDataSource`です。


[![ProductsInCategoryDataSource をという名前の新しい ObjectDataSource を追加します。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image16.png)

**図 6**: 新しい ObjectDataSource という追加`ProductsInCategoryDataSource`([フルサイズのイメージを表示するをクリックして](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image18.png))


起動されるように、ObjectDataSource を構成、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`メソッドです。


[![GetProductsByCategoryID(categoryID) メソッドを呼び出す ObjectDataSource があります。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image19.png)

**図 7**: ObjectDataSource 呼び出しがある、`GetProductsByCategoryID(categoryID)`メソッド ([フルサイズのイメージを表示するをクリックして](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image21.png))


以降、`GetProductsByCategoryID(categoryID)`メソッドは、入力パラメーターで使用、ウィザードの最後の手順では、パラメーター値のソースを指定できます。 選択したカテゴリからこれらの製品を表示するためにからプルされたパラメーターを指定しなければ、 `Categories` DropDownList です。


[![選択したカテゴリの DropDownList から categoryID パラメーターの値を取得します。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image22.png)

**図 8**: Get、  *`categoryID`* カテゴリの選択した DropDownList からパラメーター値 ([フルサイズのイメージを表示するをクリックして](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image24.png))


ウィザードの完了後、GridView は、製品のプロパティの各 BoundField があります。 これら BoundFields だけがクリーンアップみましょう、 `ProductName`、 `UnitPrice`、 `UnitsInStock`、および`UnitsOnOrder`BoundFields が表示されます。 自由に残りの BoundFields に、フィールド レベルの設定を追加する (書式設定など、`UnitPrice`通貨として)。 これらの変更を加えたら、GridView の宣言型マークアップを次のようになります。


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample1.aspx)]

この時点で、選択したカテゴリに属している製品の注文の名前、単価、在庫、および単位を示す完全に機能しているマスター/詳細レポートがあります。


[![選択したカテゴリの DropDownList から categoryID パラメーターの値を取得します。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image25.png)

**図 9**: Get、  *`categoryID`* カテゴリの選択した DropDownList からパラメーター値 ([フルサイズのイメージを表示するをクリックして](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image27.png))


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>手順 2: GridView でフッターを表示します。

GridView コントロールは、ヘッダーとフッター行を表示できます。 値に応じてこれらの行が表示されます、`ShowHeader`と`ShowFooter`プロパティにそれぞれ、`ShowHeader`を既定とします`true`と`ShowFooter`に`false`です。 GridView にフッターを単に含める次のように設定します。 その`ShowFooter`プロパティを`true`です。


[![GridView の ShowFooter プロパティを true に設定します。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image28.png)

**図 10**: 設定 GridView の`ShowFooter`プロパティを`true`([フルサイズのイメージを表示するをクリックして](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image30.png))


フッター行が GridView; で定義されているフィールドごとのセルを持つただし、これらのセルは、既定では空です。 ブラウザーで作業の進行状況を表示するのにはしばらくを実行します。 `ShowFooter`プロパティに設定されています`true`GridView に空のフッター行が含まれています。


[![GridView Now には、フッター行が含まれています。](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image31.png)

**図 11**: GridView にフッター行が含まれています ([フルサイズのイメージを表示するをクリックして](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image33.png))


図 11 にフッター行しない目立つように、白色の背景があるとします。 作成してみましょう、 `FooterStyle` CSS クラスの`Styles.css`を濃い赤の背景を指定し、構成、`GridView.skin`のスキン ファイル、 `DataWebControls` GridView にこの CSS クラスを代入するテーマ`FooterStyle`の`CssClass`プロパティです。 スキンとテーマを復習する必要がある場合に戻って、 [、ObjectDataSource でデータを表示する](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)チュートリアルです。

次の CSS クラスを追加することによって開始`Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample2.css)]

`FooterStyle` CSS クラスのスタイルと似ています、`HeaderStyle`クラスを`HeaderStyle`の背景色が濃いほど注意し、そのテキストが太字のフォントで表示されます。 さらに、フッターにテキストが右揃えのヘッダーのテキストが中央に対しです。

次に、この CSS クラスをすべて GridView のフッターに関連付けるを開き、`GridView.skin`ファイルで、`DataWebControls`テーマとセット、`FooterStyle`の`CssClass`プロパティです。 この追加した後、ファイルのマークアップをようになります。


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample3.aspx)]

画面は、下の例、この変更はフッターよりはっきり目立ちます。


[![GridView のフッター行が赤の背景色を持つようになりました](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image34.png)

**図 12**: GridView のフッター行が赤の背景色を持つようになりました ([フルサイズのイメージを表示するをクリックして](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image36.png))


## <a name="step-3-computing-the-summary-data"></a>手順 3: 概要データを計算します。

GridView のフッターを表示、私たちが直面する次の課題は、集計データを計算する方法です。 これにはこの情報を集計を計算する 2 つの方法があります。

1. SQL クエリを使用には、特定のカテゴリの集計データを計算するデータベースに追加のクエリを発行した可能性があります。 SQL にはと共に集計関数の数が含まれています、`GROUP BY`句をデータの集計するデータを指定します。 次の SQL クエリは元に戻す必要な情報。  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample4.sql)]

    もちろんから直接には、このクエリを発行する必要はありません、 `SummaryDataInFooter.aspx`  ページで、内のメソッドを作成することでわけではなく、`ProductsTableAdapter`と`ProductsBLL`です。
2. 説明したようには、GridView に追加されているが、この情報をコンピューティング[カスタム書式指定ベース時にデータ](custom-formatting-based-upon-data-cs.md)チュートリアルでは、GridView の`RowDataBound`イベント ハンドラーが GridView の後に追加される行ごとに 1 回発生そのされましたデータ バインドします。 このイベントのイベント ハンドラーを作成することでは、実行中を保つことができますし値の合計を集計します。 最後の後は、データ行が、合計と平均の計算に必要な情報がある GridView にバインドされました。

通常使用すると 2 番目の方法、トリップ データベースとデータ アクセス層とビジネス ロジック層、要約機能を実装するために必要な作業に保存されますが、いずれかの方法が十分に機能します。 このチュートリアルではみましょう 2 番目のオプションを使用して追跡集計途中経過を使用して、`RowDataBound`イベント ハンドラー。

作成、 `RowDataBound` GridView をデザイナーでを選択し、[プロパティ] ウィンドウから表示される稲妻アイコンをクリックするをダブルクリックするとして GridView のイベント ハンドラー、`RowDataBound`イベント。 これという名前の新しいイベント ハンドラーが作成されます`ProductsInCategory_RowDataBound`で、`SummaryDataInFooter.aspx`ページの分離コード クラスです。


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample5.cs)]

実行中の合計を維持するためには、イベント ハンドラーのスコープの外部変数を定義する必要があります。 次の 4 つのページ レベル変数を作成します。

- `_totalUnitPrice`、型の`decimal`
- `_totalNonNullUnitPriceCount`、型の`int`
- `_totalUnitsInStock`、型の`int`
- `_totalUnitsOnOrder`、型の`int`

次に、各データ行の中に、これら 3 つの変数をインクリメントするコードを記述、`RowDataBound`イベント ハンドラー。


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample6.cs)]

`RowDataBound` DataRow で扱うことを確認してイベント ハンドラーを起動します。 確立されている、`Northwind.ProductsRow`インスタンスだけにバインドされていた、`GridViewRow`オブジェクトに`e.Row`変数に格納されて`product`です。 次に、実行中の合計変数は、現在の製品の対応する値だけインクリメントされます (データベースが含まれていると仮定すると`NULL`値)。 共に、実行するを追跡`UnitPrice`合計と非数`NULL``UnitPrice`平均価格はこれら 2 つの数値の商であるためにを記録します。

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>手順 4: フッター内のデータの概要の表示

合計、集計データでは、最後の手順は、GridView のフッター行に表示するには。 このタスクも実行できますを通じてプログラムによって、`RowDataBound`イベント ハンドラー。 注意してください、`RowDataBound`に対してイベント ハンドラーが発生*すべて*フッター行を含む GridView にバインドされている行。 したがって、フッター行の次のコードを使用してデータを表示するイベント ハンドラーを強化することができます。


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample7.cs)]

フッター行が追加されるので、GridView にすべてのデータ行を追加した後、ある時点で準備ができました集計途中経過の計算が完了しましたが、フッターに集計データを表示することを確信できます。 最後の手順では、次が、フッターのセルにこれらの値を設定します。

特定のページ フッターのセルにテキストを表示する`e.Row.Cells[index].Text = value`、ここで、 `Cells` 0 から始まるインデックスを作成します。 次のコードでは、(製品の数で割った値の合計金額) の平均価格を計算し、在庫と GridView の適切なフッターのセル内の順序の単位で単位の合計数と共に表示します。


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample8.cs)]

図 13 は、このコードを追加した後にレポートを示します。 注方法、`ToString("c")`平均価格概要情報が、通貨のように書式設定します。


[![GridView のフッター行が赤の背景色を持つようになりました](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image37.png)

**図 13**: GridView のフッター行が赤の背景色を持つようになりました ([フルサイズのイメージを表示するをクリックして](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image39.png))


## <a name="summary"></a>概要

概要データを表示することは一般的なレポート要件および GridView コントロール簡単のフッター行にこのような情報を含めます。 フッター行を表示するときに、GridView の`ShowFooter`プロパティに設定されている`true`設定からプログラムでそのセルにテキストを持つことができます、`RowDataBound`イベント ハンドラー。 集計データを計算するか実行できますまたは ASP.NET ページの分離コード クラスの概要データをプログラムで計算するコードを使用して、データベースのクエリを再実行しています。

このチュートリアルでは、GridView、DetailsView、フォーム ビュー コントロールでのカスタム書式弊社の調査で終了します。 [次へ]、チュートリアルの挿入、更新、およびこれらの同じコントロールを使用してデータを削除すると、探索が開始されます。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

>[!div class="step-by-step"]
[前へ](using-the-formview-s-templates-cs.md)
[次へ](custom-formatting-based-upon-data-vb.md)
