---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
title: GridView のフッター (VB) から新しいレコードの挿入 |Microsoft ドキュメント
author: rick-anderson
description: GridView コントロールでは、データの新しいレコードを挿入するための組み込みのサポートは提供しません、中にこのチュートリアルでは、GridView に含めるを拡張するためには.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 528acc48-f20c-4b4e-aa16-4cc02f068ebb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: 32f3cb23805813135bf463720e7479f5f819deb7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="inserting-a-new-record-from-the-gridviews-footer-vb"></a>GridView のフッター (VB) から新しいレコードを挿入します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_VB.exe)または[PDF のダウンロード](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/datatutorial53vb1.pdf)

> GridView コントロールでは、データの新しいレコードを挿入するための組み込みのサポートは提供しません、中に、このチュートリアルを挿入するインターフェイスを含める GridView を強化する方法を示します。


## <a name="introduction"></a>はじめに

説明したように、 [、概要の挿入、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)チュートリアル、GridView、DetailsView、および FormView Web のそれぞれのコントロールには、組み込みのデータ変更機能が含まれます。 宣言型のデータ ソース コントロールを併用すると、これら 3 つの Web コントロールできます迅速かつ簡単にするように構成データの変更とでは、1 つの行のコードを記述する必要はありません。 残念ながら、DetailsView およびフォームのコントロールのみでは、組み込みの挿入、編集、および削除機能を提供します。 GridView には、編集および削除のサポートのみ提供しています。 ただし、少しのひじグリースでお補強できます。 GridView に挿入するインターフェイスを含めます。

挿入する機能を GridView に追加するは新しいレコードに追加されますを決定する、挿入のインターフェイスを作成して、新しいレコードを挿入するコードの記述を担当します。 GridView のフッターに挿入するインターフェイスを追加することでこのチュートリアルでは行が (図 1 を参照してください)。 各列のフッターのセルには、適切なデータ コレクションのユーザー インターフェイス要素 (製品の名前をテキスト ボックス、供給業者、およびなどの DropDownList) が含まれています。 列も必要があります、追加のボタンで、クリックされたときがポストバックを発生させるし、に新しいレコードを挿入、`Products`テーブル フッター行に指定された値を使用します。


[![フッター行が新しい製品を追加するためのインターフェイスを提供します。](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.png)

**図 1**: フッター行が新しい製品を追加するためのインターフェイスを提供 ([フルサイズのイメージを表示するをクリックして](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>手順 1: GridView で製品情報を表示します。

社内でをおに関係 GridView のフッターに挿入するインターフェイスを作成すると、前に、GridView をデータベースでは、製品を一覧するページに追加する最初のフォーカスを s を使用できます。 開いて開始、 `InsertThroughFooter.aspx`  ページで、`EnhancedGridView`フォルダーと、デザイナーの GridView s を設定するのには、ツールボックスからドラッグ GridView`ID`プロパティを`Products`です。 次に、という名前の新しい ObjectDataSource にバインドする GridView s のスマート タグを使って`ProductsDataSource`です。


[![ProductsDataSource をという名前の新しい ObjectDataSource を作成します。](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.png)

**図 2**: 名前付き新しい ObjectDataSource 作成`ProductsDataSource`([フルサイズのイメージを表示するをクリックして](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.png))


構成を使用する ObjectDataSource、`ProductsBLL`クラスの`GetProducts()`製品情報を取得します。 このチュートリアルでは、挿入する機能を追加するのには厳密に注目、編集および削除について心配はありません。 したがって、[挿入] タブで、ドロップダウン リストに設定されていることを確認`AddProduct()`UPDATE および DELETE の各タブで、ドロップダウン リストを (なし) に設定されているとします。


[![マップ ObjectDataSource s insert() AddProduct メソッド](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.png)

**図 3**: マップ、`AddProduct`メソッドに、ObjectDataSource s`Insert()`メソッド ([フルサイズのイメージを表示するをクリックして](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.png))


[![(なし) を更新および削除のタブのドロップダウン リストを設定します。](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.png)

**図 4**: 更新プログラムやタブ ドロップダウン リストの削除 (なし) に設定 ([フルサイズのイメージを表示するをクリックして](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.png))


ObjectDataSource のデータ ソース構成ウィザードを完了するは、対応するデータ フィールドの各 GridView にフィールドを Visual Studio が自動的に追加されます。 ここでは、すべての Visual Studio によって追加されたフィールドのままにします。 戻ってがうまくを削除し、このチュートリアルの後半では、新しいレコードを追加するときに指定するいくつかのフィールドの値がない必要があります。

あるため 80 の製品の近くに、データベースで、ユーザーが新しいレコードを追加するために、web ページの下部までスクロールする必要があります。 そのためより可視でアクセス可能な挿入のインターフェイスにページングを有効にする秒を使用できます。 ページングを有効にするには、GridView s のスマート タグからのページングを有効にするチェック ボックスを簡単にチェックします。

この時点では、GridView と ObjectDataSource s の宣言型マークアップは、次のようになります。


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample1.aspx)]


[![ページングされた GridView に表示されるすべての製品データ フィールド](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.png)

**図 5**: ページングされた GridView に表示されるすべての製品データ フィールド ([フルサイズのイメージを表示するをクリックして](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.png))


## <a name="step-2-adding-a-footer-row"></a>手順 2: フッター行を追加します。

およびそのヘッダーとデータ行は、GridView には、フッター行が含まれています。 ヘッダーとフッター行が表示される GridView 秒の値に応じて[ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx)と[ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx)プロパティです。 フッター行を表示するには、単に設定、`ShowFooter`プロパティを`True`です。 図 6 に示すように設定、`ShowFooter`プロパティを`True`フッター行をグリッドに追加します。


[![フッター行を表示するには、True に ShowFooter を設定します。](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.png)

**図 6**: を表示、フッター行設定`ShowFooter`に`True`([フルサイズのイメージを表示するをクリックして](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.png))


フッター行が濃い赤の背景色を持つことに注意してください。 これは DataWebControls テーマ作成し、すべてのページに適用されるために戻り、 [、ObjectDataSource でデータを表示する](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md)チュートリアルです。 具体的には、`GridView.skin`ファイルの構成、`FooterStyle`このようなであるプロパティは、 `FooterStyle` CSS クラス。 `FooterStyle`でクラスが定義されている`Styles.css`次のようにします。


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample2.css)]

> [!NOTE]
> 前のチュートリアルでの GridView のフッター行の使用を検討しました。 必要な場合に戻って、 [GridView のフッターに概要情報を表示する](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md)リフレッシャーのためのチュートリアルです。


設定した後、`ShowFooter`プロパティを`True`、すぐをブラウザーで、出力を表示します。 現在、フッター行ではありませんが含まれていないには、テキストや Web コントロールです。 手順 3 では、GridView の各フィールドのフッターを変更して、適切な挿入インターフェイスが含まれるようにしてみます。


[![空のページ フッターは、行の表示上、ページング インターフェイス コントロールです。](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image13.png)

**図 7**:「空のフッター行は表示上、ページング インターフェイス コントロール ([フルサイズ イメージを表示するに、をクリックして](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>手順 3: フッター行をカスタマイズします。

戻り、 [GridView コントロールを使用して TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) TemplateFields (ではなく BoundFields または CheckBoxFields) を使用して特定の GridView 列の表示を大幅にカスタマイズする方法を説明しましたチュートリアル  [データ変更インターフェイスのカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)TemplateFields を使用した GridView で編集インターフェイスをカスタマイズについて説明しました。 TemplateField、マークアップ、Web コントロール、およびデータ バインド構文の両方を定義するテンプレートの数で構成されます再呼び出しは、特定の行の型に使用されます。 `ItemTemplate`、たとえば、読み取り専用の行で使用するテンプレートを指定します中に、`EditItemTemplate`編集可能な行のテンプレートを定義します。

と共に、`ItemTemplate`と`EditItemTemplate`、TemplateField にも含まれています、`FooterTemplate`フッター行のコンテンツを指定します。 インターフェイスを挿入する各フィールドに必要な Web コントロールを追加してそのため、`FooterTemplate`です。 開始するには、GridView のフィールドのすべてを TemplateFields に変換します。 これを GridView s スマート タグに、左下隅で各フィールドを選択しを TemplateField リンクにこのフィールドに変換をクリックすると、列の編集リンクをクリックします。


![各フィールドを TemplateField に変換します。](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.gif)

**図 8**: 各フィールドを TemplateField に変換


このフィールドを TemplateField に変換をクリックすると同等の TemplateField に現在のフィールドの種類がオンにします。 TemplateField に各 BoundField は置き換えなど、 `ItemTemplate` 、対応するデータ フィールドを表示するラベルを格納していると、`EditItemTemplate`テキスト ボックスに、データ フィールドを表示します。 `ProductName` BoundField は TemplateField マークアップを次に変換されました。


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample3.aspx)]

同様に、 `Discontinued` CheckBoxField に変換された TemplateField が`ItemTemplate`と`EditItemTemplate`CheckBox Web コントロールを含む (で、 `ItemTemplate` s チェック ボックスが無効になっています)。 読み取り専用`ProductID`両方でのラベル コントロールを備えた TemplateField に変換された BoundField、`ItemTemplate`と`EditItemTemplate`です。 要するに、既存の GridView に変換するフィールドを TemplateField は、迅速かつ簡単な方法は既存のフィールドの機能のいずれかを失うことがなく複数カスタマイズ可能な TemplateField に切り替えるにです。

GridView 以降するでは t 編集をサポート、お気軽に削除、`EditItemTemplate`だけ各 TemplateField からまま、`ItemTemplate`です。 その後で、GridView s の宣言型マークアップは、次のようになります。


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample4.aspx)]

各フィールド s に、適切な挿入インターフェイスを入力 GridView の各フィールドを TemplateField に変換すると、これで`FooterTemplate`です。 一部のフィールドに挿入するインターフェイスはありません (`ProductID`のインスタンス) です。 新しい製品の情報を収集するために使用する Web コントロールで他のユーザーは異なります。

編集のインターフェイスを作成するには、GridView s のスマート タグからテンプレートの編集リンクを選択します。 次に、ドロップダウン リストからフィールドを選択、適切な s`FooterTemplate`し、適切なコントロールをツールボックスからデザイナーにドラッグします。


[![各フィールドの後に、適切な挿入インターフェイスを追加します。](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image15.png)

**図 9**: 各フィールドに、適切な挿入インターフェイスを追加`FooterTemplate`([フルサイズのイメージを表示するをクリックして](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image16.png))


次の箇条書きリストは、追加を挿入するインターフェイスを指定する、GridView フィールドを列挙します。

- `ProductID` None です。
- `ProductName` テキスト ボックスを追加し、設定、`ID`に`NewProductName`です。 RequiredFieldValidator コントロールを追加すると、ユーザーが新しい製品の名の値を入力することを確認します。
- `SupplierID` None です。
- `CategoryID` None です。
- `QuantityPerUnit` 設定、テキスト ボックスを追加、`ID`に`NewQuantityPerUnit`です。
- `UnitPrice` という名前のテキスト ボックスを追加`NewUnitPrice`や入力した値に確実に CompareValidator 通貨の値は 0 以上。
- `UnitsInStock` テキスト ボックスを使用している`ID`に設定されている`NewUnitsInStock`です。 入力した値がより大きいまたは 0 に等しい整数値であることを確認する CompareValidator が含まれます。
- `UnitsOnOrder` テキスト ボックスを使用している`ID`に設定されている`NewUnitsOnOrder`です。 入力した値がより大きいまたは 0 に等しい整数値であることを確認する CompareValidator が含まれます。
- `ReorderLevel` テキスト ボックスを使用している`ID`に設定されている`NewReorderLevel`です。 入力した値がより大きいまたは 0 に等しい整数値であることを確認する CompareValidator が含まれます。
- `Discontinued` 設定のチェック ボックスを追加、`ID`に`NewDiscontinued`です。
- `CategoryName` DropDownList を追加し、設定、`ID`に`NewCategoryID`です。 という名前の新しい ObjectDataSource にもバインド`CategoriesDataSource`を使用するように構成し、`CategoriesBLL`クラスの`GetCategories()`メソッドです。 DropDownList s がある`ListItem`s の表示、`CategoryName`データ フィールドを使用して、`CategoryID`データ フィールドの値として。
- `SupplierName` DropDownList を追加し、設定、`ID`に`NewSupplierID`です。 という名前の新しい ObjectDataSource にもバインド`SuppliersDataSource`を使用するように構成し、`SuppliersBLL`クラスの`GetSuppliers()`メソッドです。 DropDownList s がある`ListItem`s の表示、`CompanyName`データ フィールドを使用して、`SupplierID`データ フィールドの値として。

検証コントロールのそれぞれについて、クリア、`ForeColor`プロパティできるように、 `FooterStyle` CSS クラス s 白のフォア グラウンド色が赤の既定の代わりに使用されます。 使用しても、`ErrorMessage`プロパティの詳細についてが設定されて、`Text`プロパティには、アスタリスクをします。 設定を検証コントロールのテキストが原因で、挿入のインターフェイスを 2 つの行に折り返すことを防ぐために、 `FooterStyle` s`Wrap`プロパティを false の列ごとに、`FooterTemplate`検証コントロールを使用します。 最後に、GridView とセットの下に ValidationSummary コントロールを追加、`ShowMessageBox`プロパティを`True`とその`ShowSummary`プロパティを`False`です。

新しい製品を追加するときに提供する必要があります、`CategoryID`と`SupplierID`です。 フッターのセルに DropDownLists を介してこの情報をキャプチャしたら、`CategoryName`と`SupplierName`フィールドです。 使用して、これらのフィールドはなく、`CategoryID`と`SupplierID`TemplateFields グリッド s、ユーザーのデータ行もためにが可能性がありますより興味の ID 値ではなく、カテゴリと業者の名前を表示します。 以降、`CategoryID`と`SupplierID`値でキャプチャされるようになりました、`CategoryName`と`SupplierName`フィールド s の挿入のインターフェイスは削除できます、`CategoryID`と`SupplierID`GridView から TemplateFields です。

同様に、`ProductID`に新しい製品を追加するときに使用されませんので、 `ProductID` TemplateField を同様に削除することができます。 ただし、s のままにできるように、`ProductID`グリッドでフィールドです。 だけでなく、テキスト ボックス、DropDownLists、チェック ボックス、および挿入のインターフェイスを構成するための検証コントロールも必要な追加ボタンをクリックすると、データベースに、新しい製品を追加するロジックを実行します。 手順 4 では、挿入インターフェイスで、[追加] ボタン、 `ProductID` TemplateField の`FooterTemplate`です。

GridView のさまざまなフィールドの外観を向上させるために自由にできます。 書式を設定するなど、`UnitPrice`を通貨値を右揃え、 `UnitsInStock`、 `UnitsOnOrder`、および`ReorderLevel`フィールド、および更新プログラム、 `HeaderText` TemplateFields の値。

含まれるインターフェイスを挿入する高速移動機能を作成した後、 `FooterTemplate` s、削除、 `SupplierID`、および`CategoryID`TemplateFields、および書式設定および TemplateFields、宣言、GridView s の整列をグリッドの外観の向上マークアップは、次のようになります。


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample5.aspx)]

表示すると、ブラウザーを使って GridView のフッター行が含まれています、完成した (図 10 参照) を挿入するインターフェイスです。 この時点では、挿入のインターフェイスでは t には、ユーザーが新しい製品のデータを入力し、データベースに新しいレコードを挿入しようと彼女 s を指定するための手段が含まれます。 ここではまた、フッターに入力されたデータで新しいレコードに変換する方法に対処するには、まだ ve、`Products`データベース。 挿入するインターフェイスに、[追加] ボタンを含める方法およびでコードを実行する方法に紹介する手順 4. でポストバック時に、秒をクリックします。 手順 5 では、フッターからデータを使用して新しいレコードを挿入する方法を示します。


[![GridView フッターは、新しいレコードを追加するためのインターフェイスを提供します。](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image17.png)

**図 10**: GridView フッターは、新しいレコードを追加するためのインターフェイスを提供 ([フルサイズのイメージを表示するをクリックして](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>手順 4: 挿入インターフェイスで、[追加] ボタンを含む

現在インターフェイスを挿入するフッター行のユーザーが新しい製品の情報の入力を終了したことを指定するための手段がないため、挿入するインターフェイスにどこかに、[追加] ボタンを含める必要があります。 これは、既存のいずれかに入れられます。 `FooterTemplate` s またはおでした新しい列を追加、グリッドをこの目的のためです。 このチュートリアルでは、let s 配置に追加 ボタン、 `ProductID` TemplateField の`FooterTemplate`です。

デザイナーから GridView s のスマート タグにテンプレートの編集リンクをクリックしを選択し、`ProductID`フィールドの`FooterTemplate`ドロップダウン リストからです。 Button Web コントロール (または LinkButton または ImageButton、希望する場合)、テンプレート、その ID に設定を追加`AddProduct`、その`CommandName`挿入 をその`Text`図 11 に示すように追加するプロパティです。


[![ProductID TemplateField の後に、[追加] ボタンを配置します。](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image19.png)

**図 11**: で、[追加] ボタンを配置、 `ProductID` TemplateField s `FooterTemplate` ([フルサイズのイメージを表示するをクリックして](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image20.png))


Ve するには、[追加] ボタンが含まれている、ブラウザーでページをテストします。 無効なデータ挿入のインターフェイスで [追加] ボタンをクリックすると、ポストバックが短いサーキットに注意してください、ValidationSummary コントロールが (図 12 を参照してください)、無効なデータを示します。 適切なデータが入力した、[追加] ボタンをクリックするとポストバックが発生します。 レコードがないデータベースに追加、ただし。 実際に挿入を実行するコードを記述する必要があります。


[![ボタンの追加のポストバックが短いサーキットを挿入するインターフェイスに無効なデータがある場合](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image21.png)

**図 12**: ボタンの追加のポストバックが短いサーキットを挿入するインターフェイスに無効なデータがある場合 ([フルサイズのイメージを表示するをクリックして](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image22.png))


> [!NOTE]
> 挿入のインターフェイスでの検証コントロールはされた検証グループに割り当てられていません。 挿入のインターフェイスは、ページ上の検証コントロールののみセット限り、正しく動作します。 挿入検証コントロールがインターフェイスし、ボタンの s を追加、ただし、ある場合 (検証コントロールなど、グリッド s 編集インターフェイスで)、ページ上の他の検証コントロール、`ValidationGroup`プロパティに同じ値を割り当てる必要がありますとして soこれらのコントロールを特定の検証グループに関連付けます。 参照してください[ASP.NET 2.0 の検証コントロールを分解](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)検証グループへの検証コントロール、ページ上のボタンのパーティション分割の詳細についてです。


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>手順 5: に新しいレコードを挿入する、`Products`テーブル

GridView の組み込みの編集機能を利用しているとき、GridView に自動的に更新プログラムを実行するために必要な作業のすべてを処理します。 具体的には、[更新] ボタンがクリックされたときにその値をコピーします ObjectDataSource s のパラメーターに、編集用のインターフェイスから入力`UpdateParameters`コレクションと、ObjectDataSource s を呼び出すことによって更新をオフは`Update()`メソッドです。 GridView に挿入するためこのような組み込みの機能が用意されていないため、ObjectDataSource s を呼び出すコードを実装する必要があります`Insert()`メソッドと、挿入値インターフェイス ObjectDataSource s をコピー`InsertParameters`コレクション.

この insert ロジックは、[追加] ボタンがクリックしてされた後に実行される必要があります。 説明したように、[を追加して、GridView のボタンに応答して](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb.md)チュートリアルでは、ボタン、LinkButton、または、GridView の ImageButton がクリックされた GridView s いつでも`RowCommand`ポストバック イベントが発生します。 このイベントが発生するかどうか、ボタン、LinkButton、または ImageButton が追加された明示的にフッター行の追加 ボタンなどまたは GridView によって自動的に追加されたかどうか (並べ替えを有効にする機能が選択されている場合は、各列の上部にあるなど、またはあるページングを有効にするには、選択したときのページング インターフェイスで)。

したがってを追加 ボタンをクリックすると、ユーザーに応答する必要があります GridView s のイベント ハンドラーを作成する`RowCommand`イベント。 このイベントが発生するたびに*任意*ボタン、LinkButton、または GridView の ImageButton がクリックされた、s の重要なことのみを続けていくロジックを挿入する場合、 `CommandName` にイベントハンドラーのマップに渡されるプロパティ`CommandName` (Insert) の [追加] ボタンの値。 さらに、検証コントロールが有効なデータを報告している場合を続けていく必要がありますものみです。 これに合わせて、イベント ハンドラーを作成、`RowCommand`次のコードでイベント。


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample6.vb)]

> [!NOTE]
> でしょうか、イベント ハンドラーが確認を悩ませる理由、`Page.IsValid`プロパティです。 結局のところ、受注 t ポストバックのでは、無効なデータが挿入のインターフェイスで提供される場合を抑制するか。 ユーザーが JavaScript が無効かがクライアント側の検証ロジックを回避するための手順を踏まない限り、この想定は正しく表示になります。 つまり、1 つ必要がありますに依存しないように厳密にクライアント側の検証サーバー サイドのチェックの有効性は、データを操作する前に常に実行する必要があります。


手順 1. で作成した、 `ProductsDataSource` ObjectDataSource ようにその`Insert()`メソッドのマップ先、`ProductsBLL`クラスの`AddProduct`メソッドです。 新しいレコードを挿入する、`Products`テーブル、ObjectDataSource s を単に呼び出すことができますお`Insert()`メソッド。


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample7.vb)]

これで、`Insert()`メソッドが呼び出されてに渡されたままの挿入のインターフェイスからパラメーターに値をコピーするは、`ProductsBLL`クラスの`AddProduct`メソッドです。 バックアップで示したように、 [、イベントに関連付けられている挿入、更新、および削除を確認する](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)チュートリアルでは、これには、ObjectDataSource s を通じて`Inserting`イベント。 `Inserting`イベントからコントロールをプログラムで参照する必要があります、 `Products` GridView のフッター行し、それらの値を割り当てる、`e.InputParameters`コレクション。 ユーザーは、終了などの値を省略した場合、 `ReorderLevel`  ボックスに空白の値をデータベースに挿入する必要がありますを指定する必要があります`NULL`です。 以降、`AddProducts`メソッドが null 許容のデータベース フィールドの null 許容型を受け入れる、単に null 許容型を使用し、その値に設定`Nothing`ユーザー入力が省略されている場合。


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample8.vb)]

`Inserting`イベント ハンドラーが完了しました、新しいレコードを追加するのには`Products`GridView のフッター行を使用してデータベース テーブルです。 いくつかの新しい製品の追加を再試行してください。

## <a name="enhancing-and-customizing-the-add-operation"></a>拡張とカスタマイズの操作を追加

現時点では、[追加] ボタンをクリックすると、データベース テーブルに新しいレコードを追加は実現されませんあらゆる種類の視覚的なフィードバック レコードが正常に追加されたことです。 理想的には、ラベル Web コントロールまたはクライアント側の警告ボックスはユーザーに通知する、insert が正常に完了しました。 私のままに練習として、リーダーの。

このチュートリアルで使用される GridView に一覧の製品では、任意の並べ替え順序は適用されません。 また、では、データの並べ替えにエンド ユーザー。 その結果、レコードは、その主キー フィールドで、データベースのように並べ替えられています。 新しい各レコードであるため、`ProductID`最後の 1 つのグリッドの最後に数行の新しい製品が追加されるたびにより大きい値です。 したがって、GridView の最後のページに新しいレコードを追加した後、ユーザーを自動的に送信することがあります。 これには、呼び出しの後に次のコード行を追加することによって`ProductsDataSource.Insert()`で、`RowCommand`を送信する最後のページ GridView にデータをバインドした後、ユーザーが必要なことを示すイベントのハンドラー。


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample9.vb)]

`SendUserToLastPage` 初期値をページ レベルのブール値変数に値が代入の`False`します。 GridView s `DataBound` 、イベント ハンドラー場合`SendUserToLastPage`が false の場合、`PageIndex`最後のページをユーザーに送信するプロパティを更新します。


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample10.vb)]

理由、`PageIndex`にプロパティを設定、`DataBound`イベント ハンドラー (はなく、`RowCommand`イベント ハンドラー) ではときに、`RowCommand`イベント ハンドラーは発生に新しいレコードを追加するには、まだ ve、`Products`データベース テーブルです。 そのためで、`RowCommand`イベント ハンドラーの最後のページ インデックス (`PageCount - 1`) 最後のページ インデックスを表す*する前に*新しい製品が追加されました。 追加される製品のほとんどは、最後のページ インデックスは、同じを新しい製品を追加した後です。 正しく更新されず、新しい最後のページ インデックスで追加された製品が発生する場合は、`PageIndex`で、`RowCommand`イベント ハンドラーが表示されます (新しい製品を追加する前に最後のページ インデックス) の最後のページから 2 番目の新しい i 最後のページではなくあります。 以降、`DataBound`イベント ハンドラーが、新しい製品が追加されており、データがグリッドに設定して再バインド後に起動、`PageIndex`プロパティがあることが分かってお re 正しいの最後のページ インデックスを取得します。

最後に、このチュートリアルで使用される GridView は非常に幅の広い新しい製品を追加するために収集する必要がありますのフィールドの数が原因です。 この幅により DetailsView s の垂直レイアウトが優先される可能性があります。 GridView s 少ない入力を収集して全体の幅が低下する可能性があります。 収集する必要ありませんおそらく、 `UnitsOnOrder`、 `UnitsInStock`、および`ReorderLevel`フィールドに新しい製品を追加するときに GridView からこれらのフィールドを削除できなかった場合。

収集されたデータを調整するには、2 つの方法のいずれかを使用できます。

- 引き続き使用する、`AddProduct`の値を必要とするメソッド、 `UnitsOnOrder`、 `UnitsInStock`、および`ReorderLevel`フィールドです。 `Inserting`ハード コーディングされたを指定して、イベント ハンドラーは、挿入のインターフェイスから削除されたこれらの入力に使用する値を既定値です。
- 新しいオーバー ロードを作成、`AddProduct`メソッドで、`ProductsBLL`の入力を受け付けないクラス、 `UnitsOnOrder`、 `UnitsInStock`、および`ReorderLevel`フィールドです。 次に、ASP.NET ページでは、この新しいオーバー ロードを使用する ObjectDataSource を構成します。

いずれかのオプションはも同様に動作します。 過去のチュートリアルを使用して、後者のオプションの複数のオーバー ロードを作成する、`ProductsBLL`クラスの`UpdateProduct`メソッドです。

## <a name="summary"></a>まとめ

GridView DetailsView や FormView、組み込みの挿入機能がないが、若干の手間でフッター行に挿入するインターフェイスを追加できます。 GridView に単にフッター行を表示する次のように設定します。 その`ShowFooter`プロパティを`True`です。 フィールドを TemplateField に変換すると、各フィールドのフッター行の内容をカスタマイズでき、挿入、追加するインターフェイスを`FooterTemplate`です。 このチュートリアルで示したように、`FooterTemplate`ボタン、テキスト ボックス、DropDownLists、チェック ボックス、(DropDownLists) などのデータ ドリブン Web コントロールを設定するためのデータ ソース コントロールと検証コントロールを含めることができます。 ユーザーの入力を収集するため、コントロールと共にボタンの追加、LinkButton、または ImageButton が必要です。

ときに、[追加] ボタンをクリックして ObjectDataSource の`Insert()`挿入のワークフローを開始するメソッドが呼び出されます。 構成されている挿入メソッドを呼び出すが、ObjectDataSource (、`ProductsBLL`クラスの`AddProduct`このチュートリアルでは、メソッド)。 お ObjectDataSource s インターフェイスを挿入する GridView s から値をコピーする必要があります`InsertParameters`挿入メソッドが呼び出される前に収集します。 これには、プログラムによって、ObjectDataSource s 挿入インターフェイス Web コントロールを参照して`Inserting`イベント ハンドラー。

このチュートリアルでは、GridView s の外観を強化するための手法で、外観を完了します。 次のチュートリアルのセットは、Web コントロールのデータとイメージ、Pdf、Word 文書などのバイナリ データを操作する方法を調べます。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客は、「社長補佐 Leigh をでした。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](adding-a-gridview-column-of-checkboxes-vb.md)
