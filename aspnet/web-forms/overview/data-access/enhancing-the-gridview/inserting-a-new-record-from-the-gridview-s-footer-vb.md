---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
title: GridView のフッター (VB) から新しいレコードの挿入 |Microsoft Docs
author: rick-anderson
description: このチュートリアルが含める GridView を拡張する方法では、GridView コントロールには、データの新しいレコードを挿入するための組み込みのサポートされていません、中に、.
ms.author: aspnetcontent
ms.date: 03/06/2007
ms.assetid: 528acc48-f20c-4b4e-aa16-4cc02f068ebb
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: 69aa6659a6c18ed6d16e2916f0f9088ef38a453f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828320"
---
<a name="inserting-a-new-record-from-the-gridviews-footer-vb"></a>GridView のフッター (VB) から新しいレコードを挿入します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_VB.exe)または[PDF のダウンロード](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/datatutorial53vb1.pdf)

> GridView コントロールには、データの新しいレコードを挿入するための組み込みのサポートされていません、このチュートリアルでは、挿入のインターフェイスを含める GridView を拡張する方法が表示されます。


## <a name="introduction"></a>はじめに

説明したように、[挿入の概要、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)チュートリアルでは、GridView、DetailsView、FormView Web の各のコントロールには、組み込みのデータ変更機能が含まれます。 宣言型のデータ ソース コントロールを併用すると、これら 3 つの Web コントロールできます迅速かつ簡単にするように構成データの変更とでは 1 行のコードを記述する必要はありません。 残念ながら、のみ、DetailsView コントロールと FormView コントロールは、組み込みの挿入、編集、および削除機能を提供します。 GridView には、編集および削除のサポートのみ提供しています。 ただし、少しの屈曲グリースはことができますの強化点、GridView を挿入するインターフェイスを含めます。

挿入機能を GridView に追加するで新しいレコードが追加されますを決定する、挿入のインターフェイスを作成して、新しいレコードを挿入するコードを記述を担当います。 挿入のインターフェイスを GridView のフッターに追加することに注目するはこのチュートリアルでは行が (図 1 参照)。 各列のフッター セルには、適切なデータ コレクションのユーザー インターフェイス要素 (s の製品名 ボックスと供給業者、DropDownList) が含まれています。 列も必要があります、追加のボタンで、クリックされたときのポストバックを発生させるしに新しいレコードを挿入、`Products`テーブル フッター行に指定された値を使用します。


[![フッター行が新しい製品を追加するためのインターフェイスを提供します。](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.png)

**図 1**: フッター行は、新製品の追加のインターフェイスを提供します ([フルサイズの画像を表示する をクリックします](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.png))。


## <a name="step-1-displaying-product-information-in-a-gridview"></a>手順 1: GridView の製品情報を表示します。

GridView のフッターに挿入するインターフェイスを作成すると自分たちに関係、前に、データベースでは、製品を一覧表示されたページに GridView を追加する最初のフォーカスを s を使用できます。 開いて開始、`InsertThroughFooter.aspx`ページで、`EnhancedGridView`フォルダーと、デザイナーの GridView s を設定するのには、ツールボックスからドラッグ GridView`ID`プロパティを`Products`します。 次に、GridView s のスマート タグをという名前の新しい ObjectDataSource にバインドを使用して`ProductsDataSource`します。


[![ProductsDataSource という名前の新しい ObjectDataSource を作成します。](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.png)

**図 2**: 名前付き新しい ObjectDataSource 作成`ProductsDataSource`([フルサイズの画像を表示する をクリックします](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.png))。


構成を使用する ObjectDataSource、`ProductsBLL`クラスの`GetProducts()`製品情報を取得します。 このチュートリアルでは、挿入機能を追加するのには厳密に注目できるようにし、編集および削除について心配ありません。 そのため、[挿入] タブで、ドロップダウン リストに設定されていることを確認`AddProduct()`UPDATE および DELETE の各タブで、ドロップダウン リストが (なし) に設定されているとします。


[![マップ ObjectDataSource s insert() AddProduct メソッド](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.png)

**図 3**: マップ、 `AddProduct` ObjectDataSource s メソッド`Insert()`メソッド ([フルサイズの画像を表示する をクリックします](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.png))。


[![(なし) を更新および削除のタブのドロップダウン リストを設定します。](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.png)

**図 4**: (None) に、更新プログラムとタブ ドロップダウン リストの削除を設定 ([フルサイズの画像を表示する をクリックします](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.png))。


ObjectDataSource s のデータ ソース構成ウィザードを完了するは、対応するデータ フィールドの各 GridView にフィールドを Visual Studio が自動的に追加されます。 ここでは、すべての Visual Studio によって追加されたフィールドのままにします。 戻ってが削除され、このチュートリアルの後半では、新しいレコードを追加するときに指定するいくつかのフィールドの値がない必要があります。

データベースに 80 の製品の近くにある、ので、ユーザーが新しいレコードを追加するには、web ページの下部までスクロールする必要があります。 そのため、%s より見やすくて、アクセス可能な挿入インターフェイスにページングを有効にすることができます。 ページングを有効にするには、GridView s のスマート タグからページングを有効にするチェック ボックスをオンだけです。

この時点では、GridView コントロールと ObjectDataSource s の宣言型マークアップは、次のようになります。


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample1.aspx)]


[![製品のすべてのデータ フィールドはページングされた GridView に表示されます。](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.png)

**図 5**: ページングされた GridView で製品のすべてのデータ フィールドが表示されます ([フルサイズの画像を表示する をクリックします](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.png))。


## <a name="step-2-adding-a-footer-row"></a>手順 2: フッター行を追加します。

およびそのヘッダーとデータ行は、GridView には、フッター行が含まれています。 GridView の秒の値に応じて、ヘッダーとフッター行の表示[ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx)と[ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx)プロパティ。 フッター行を表示する設定、`ShowFooter`プロパティを`True`します。 図 6 に示すように、設定、`ShowFooter`プロパティを`True`フッター行をグリッドに追加します。


[![フッター行を表示するには、True に ShowFooter を設定します。](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.png)

**図 6**: 表示する、フッター行設定`ShowFooter`に`True`([フルサイズの画像を表示する をクリックします](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.png))。


フッター行が濃い赤の背景色を持つことに注意してください。 これは DataWebControls テーマを作成し、すべてのページに適用されるために戻り、 [、ObjectDataSource でデータを表示する](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md)チュートリアル。 具体的には、`GridView.skin`ファイルの構成、`FooterStyle`プロパティなどが使用されて、 `FooterStyle` CSS クラス。 `FooterStyle`でクラスが定義されている`Styles.css`次のようにします。


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample2.css)]

> [!NOTE]
> 私たち GridView のフッター行を使用して、前のチュートリアルで説明します。 必要な場合に戻って、 [GridView のフッターに概要情報を表示する](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md)リフレッシャーのチュートリアル。


設定した後、`ShowFooter`プロパティを`True`、時間、ブラウザーで、出力を表示するのにはかかりません。 現在、フッター行は t には、任意のテキストまたは Web コントロールが含まれます。 手順 3. では、GridView の各フィールドのフッターを変更して、適切な挿入インターフェイスが含まれるようにします。


[![空のフッター行は表示上、ページング インターフェイス コントロールです。](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image13.png)

**図 7**:、空のフッター行は表示上、ページング インターフェイス コントロール ([フルサイズの画像を表示する をクリックします](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image14.png))。


## <a name="step-3-customizing-the-footer-row"></a>手順 3: フッター行をカスタマイズします。

戻り、 [GridView コントロールで TemplateFields を使用して](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md)チュートリアル BoundFields または CheckBoxFields) はなく TemplateFields を使用する特定の GridView 列の表示を大幅にカスタマイズする方法を説明しましたで[。データ変更インターフェイスをカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)GridView で編集インターフェイスをカスタマイズ TemplateFields を使用しました。 TemplateField、マークアップ、Web コントロール、およびデータ バインディング構文の組み合わせを定義するテンプレートの数で構成が再現率は、特定の行の種類に使用します。 `ItemTemplate`、たとえば、読み取り専用の行で使用するテンプレートを指定します中に、`EditItemTemplate`テンプレート編集可能な行を定義します。

と共に、`ItemTemplate`と`EditItemTemplate`、TemplateField にも含まれています、`FooterTemplate`フッター行のコンテンツを指定します。 そのためにインターフェイスを挿入する各フィールドのために必要な Web コントロールを付け加えて、`FooterTemplate`します。 開始するには、TemplateFields を GridView のフィールドのすべてを変換します。 これ行う GridView s スマート タグに、左下隅で各フィールドを選択し、このフィールドに変換を TemplateField リンクをクリックすると、列の編集リンクをクリックします。


![各フィールドを TemplateField に変換します。](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.gif)

**図 8**: 各フィールドを TemplateField に変換します。


このフィールドを TemplateField に変換をクリックすると、同等の TemplateField に現在のフィールドの種類がオンにします。 TemplateField に各 BoundField は置き換えなど、`ItemTemplate`対応するデータ フィールドを表示するラベルを格納していると、 `EditItemTemplate`  ボックスに、データ フィールドを表示します。 `ProductName` BoundField が次の TemplateField マークアップに変換されています。


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample3.aspx)]

同様に、 `Discontinued` CheckBoxField に変換された TemplateField 持つ`ItemTemplate`と`EditItemTemplate`CheckBox Web コントロールを含む (で、 `ItemTemplate` s 無効になっているチェック ボックスをオン)。 読み取り専用`ProductID`BoundField が両方のラベル コントロールを TemplateField に変換された、`ItemTemplate`と`EditItemTemplate`します。 フィールドを TemplateField は、既存のフィールドの機能を失うことがなくカスタマイズをより TemplateField に切り替えるに迅速かつ簡単な方法を簡単に言えばは既存の GridView に変換します。

以降、GridView 操作は t サポート編集して自由に削除、`EditItemTemplate`だけ残して、各 TemplateField から、`ItemTemplate`します。 これを行う、次のように GridView s の宣言型マークアップになります。


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample4.aspx)]

GridView の各フィールドを TemplateField に変換すると、これで、適切な挿入インターフェイスには、各フィールドの入力できます`FooterTemplate`します。 一部のフィールドは挿入のインターフェイスを持ちません (`ProductID`、たとえば)。 他のユーザーは新しい製品の情報を収集するために使用する Web コントロールで異なります。

編集インターフェイスを作成するには、GridView s のスマート タグからテンプレートの編集リンクを選択します。 次に、ドロップダウン リストから適切なフィールドの s を選択`FooterTemplate`し、適切なコントロールをツールボックスからデザイナーにドラッグします。


[![各フィールドの後に、適切な挿入のインターフェイスを追加します。](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image15.png)

**図 9**: 各フィールドに適切な挿入のインターフェイスを追加`FooterTemplate`([フルサイズの画像を表示する をクリックします](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image16.png))。


次の箇条書きリストは、追加する挿入のインターフェイスを指定する、GridView フィールドを列挙します。

- `ProductID` None です。
- `ProductName` テキスト ボックスを追加し、設定、`ID`に`NewProductName`します。 RequiredFieldValidator コントロールを追加すると、ユーザーが新しい製品の名前の値を入力することを確認します。
- `SupplierID` None です。
- `CategoryID` None です。
- `QuantityPerUnit` 設定、テキスト ボックスを追加、`ID`に`NewQuantityPerUnit`します。
- `UnitPrice` という名前のテキスト ボックスを追加`NewUnitPrice`入力された値に確実に CompareValidator は通貨の値より大きいまたは 0 に等しいとします。
- `UnitsInStock` テキスト ボックスを使用している`ID`に設定されている`NewUnitsInStock`します。 入力された値が 0 以上の整数値であることを確認する CompareValidator が含まれます。
- `UnitsOnOrder` テキスト ボックスを使用している`ID`に設定されている`NewUnitsOnOrder`します。 入力された値が 0 以上の整数値であることを確認する CompareValidator が含まれます。
- `ReorderLevel` テキスト ボックスを使用している`ID`に設定されている`NewReorderLevel`します。 入力された値が 0 以上の整数値であることを確認する CompareValidator が含まれます。
- `Discontinued` 設定のチェック ボックスを追加、`ID`に`NewDiscontinued`します。
- `CategoryName` DropDownList を追加し、設定、`ID`に`NewCategoryID`します。 という名前の新しい ObjectDataSource にバインド`CategoriesDataSource`を使用するように構成し、`CategoriesBLL`クラスの`GetCategories()`メソッド。 DropDownList s がある`ListItem`s の表示、`CategoryName`データ フィールドを使用して、`CategoryID`データ フィールドの値として。
- `SupplierName` DropDownList を追加し、設定、`ID`に`NewSupplierID`します。 という名前の新しい ObjectDataSource にバインド`SuppliersDataSource`を使用するように構成し、`SuppliersBLL`クラスの`GetSuppliers()`メソッド。 DropDownList s がある`ListItem`s の表示、`CompanyName`データ フィールドを使用して、`SupplierID`データ フィールドの値として。

検証コントロールごとに、クリア、`ForeColor`プロパティように、 `FooterStyle` CSS クラス s 白い前景色の色を赤の既定の代わりに使用されます。 使用しても、`ErrorMessage`の詳細については、プロパティが設定が、`Text`をアスタリスクのプロパティ。 検証コントロールのテキストが原因で、挿入インターフェイスに 2 つの行に折り返すことを防ぐためには、設定、 `FooterStyle` s`Wrap`プロパティを false ごとに、`FooterTemplate`検証コントロールを使用します。 最後に、GridView とセットの下にある ValidationSummary コントロールを追加、`ShowMessageBox`プロパティを`True`とその`ShowSummary`プロパティを`False`します。

新しい製品を追加するときに提供する必要があります、`CategoryID`と`SupplierID`します。 フッターのセルの Dropdownlist でこの情報がキャプチャされた、`CategoryName`と`SupplierName`フィールド。 使用して、これらのフィールドではなく、`CategoryID`と`SupplierID`TemplateFields のためグリッドのデータ行も、ユーザーが可能性がありますより興味の ID 値ではなく、category と supplier の名前を表示します。 `CategoryID`と`SupplierID`値でキャプチャされるようになりましたが、`CategoryName`と`SupplierName`フィールド s の挿入のインターフェイスを削除できる、`CategoryID`と`SupplierID`TemplateFields GridView から。

同様に、`ProductID`新製品を追加するときに使用されませんので、 `ProductID` TemplateField をも削除することができます。 ただし、秒のままにできるように、`ProductID`グリッドでフィールド。 テキスト ボックス、Dropdownlist、チェック ボックス、および挿入インターフェイスを構成する検証コントロール、に加えても必要があります、追加ボタンをクリックすると、データベースに新しい製品を追加するロジックを実行します。 手順 4. で挿入インターフェイスで、[追加] ボタンを含めます、 `ProductID` TemplateField の`FooterTemplate`します。

GridView のさまざまなフィールドの外観を向上させるためにもかまいません。 書式を設定するなど、`UnitPrice`値を通貨、右揃え、 `UnitsInStock`、 `UnitsOnOrder`、および`ReorderLevel`フィールド、および更新プログラム、 `HeaderText` TemplateFields の値。

挿入でインターフェイスの高速移動機能を作成した後、 `FooterTemplate` s、削除、`SupplierID`と`CategoryID`TemplateFields、して書式設定と TemplateFields、GridView s 宣言型のアラインメントをグリッドの外観を向上させるマークアップは、次のようになります。


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample5.aspx)]

ブラウザーで表示、ときに GridView のフッター行が含まれています、完成したインターフェイスの挿入 (図 10 参照)。 この時点では、挿入のインターフェイスは t には、ユーザーが彼女 s 新しい製品のデータを入力し、データベースに新しいレコードを挿入する必要があるを指定するための手段が含まれます。 ここではまた、フッターに入力されたデータで新しいレコードに変換する方法に対処するには、まだ ve、`Products`データベース。 挿入インターフェイスに、[追加] ボタンを追加する方法とでコードを実行する方法について説明します、手順 4. でポストバックの場合、s をクリックします。 手順 5 では、フッターからデータを使用して新しいレコードを挿入する方法を示します。


[![GridView のフッターは、新しいレコードを追加するインターフェイスを提供します](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image17.png)

**図 10**: GridView のフッターは、新しいレコードを追加するインターフェイスを提供します ([フルサイズの画像を表示する をクリックします](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image18.png))。


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>手順 4: 挿入インターフェイスで、[追加] ボタンを含む

インターフェイスの挿入を現在フッター行のユーザーが新しい製品の情報の入力を終了したことを指定するための手段がないため挿入インターフェイスにどこか、[追加] ボタンを含める必要があります。 これは、既存のいずれかに入れられます。 `FooterTemplate` 、または列を追加新しいグリッドにこの目的のためです。 このチュートリアルでは、let s 配置に追加 ボタン、 `ProductID` TemplateField の`FooterTemplate`します。

デザイナーでは、GridView s のスマート タグにテンプレートの編集リンクをクリックし、、`ProductID`フィールドの`FooterTemplate`ドロップダウン リストから。 テンプレートにその ID に設定 ボタンの Web コントロール (または LinkButton または ImageButton、使用する場合) を追加`AddProduct`その`CommandName`挿入 をその`Text`図 11 に示すように追加するプロパティ。


[![ProductID TemplateField s の後に追加 ボタンを配置します。](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image19.png)

**図 11**: で、[追加] ボタンを配置、 `ProductID` TemplateField s `FooterTemplate` ([フルサイズの画像を表示する をクリックします](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image20.png))。


Ve するには、[追加] ボタンが含まれているとは、ブラウザーでページをテストします。 無効なデータを挿入するインターフェイスの追加 ボタンをクリックすると、ポストバックは短いサーキットをメモし、ValidationSummary コントロール (図 12 を参照してください)、無効なデータを示します。 適切なデータ入力を追加 ボタンをクリックするとポストバックが発生します。 レコードがないデータベースに追加、ただし。 実際に挿入を実行するコードを少し記述する必要があります。


[![ボタンの追加 s ポストバックは短いサーキット挿入インターフェイスに無効なデータがある場合](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image21.png)

**図 12**: ボタンの追加 s ポストバック サーキットの短い挿入インターフェイスに無効なデータがある場合は、([フルサイズの画像を表示する をクリックします](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image22.png))。


> [!NOTE]
> 挿入インターフェイスに検証コントロールが、検証グループに割り当てられていません。 挿入のインターフェイスが、ページの検証コントロールの唯一のセットが正しく動作します。 挿入検証コントロールがインターフェイスし、s のボタンを追加する場合、ただし、(グリッドの編集インターフェイスに検証コントロール) など、ページ上の他の検証コントロールがある、`ValidationGroup`プロパティに同じ値を割り当てる必要がありますを soこれらのコントロールを特定の検証グループに関連付けます。 参照してください[詳細に分析する ASP.NET 2.0 の検証コントロール](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)検証グループへの検証コントロールとボタンを 1 ページのパーティション分割の詳細についてはします。


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>手順 5: に新しいレコードの挿入、`Products`テーブル

GridView の組み込みの編集機能を利用する場合、GridView に自動的に更新プログラムを実行するために必要な作業のすべてを処理します。 具体的には、[更新] ボタンがクリックされたときにコピー編集インターフェイスから ObjectDataSource のパラメーターに入力された値`UpdateParameters`コレクションとが開始され、ObjectDataSource s を呼び出すことによって更新をオフ`Update()`メソッド。 ObjectDataSource s を呼び出すコードを実装する必要があります、GridView が挿入するためのような組み込み機能を提供していないため`Insert()`メソッドと ObjectDataSource s インターフェイスの挿入、値コピー`InsertParameters`コレクション.

[追加] ボタンがクリックしてされた後、この挿入のロジックを実行する必要があります。 説明したように、[を追加して GridView にボタンに応答する](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb.md)チュートリアルでは、ボタン、LinkButton、または、GridView で ImageButton がクリックされた GridView s ときにいつでも`RowCommand`ポストバック イベントが発生します。 このイベントが発生するかどうかボタンや LinkButton、ImageButton が明示的に追加など、フッター行の追加 ボタンまたは GridView で自動的に追加されたかどうか (並べ替えを有効にする機能が選択されている場合は、各列の上部にあるなど、またはLinkbutton でページングを有効にするを選択すると、ページング インターフェイス)。

そのためを追加 ボタンをクリックすると、ユーザーに応答する必要があります、GridView のイベント ハンドラーを作成する`RowCommand`イベント。 ときにこのイベントが起動するため*任意*ボタンや LinkButton、ImageButton、GridView では、クリックすると、その s ことだけに進むロジックを挿入する場合に重要な`CommandName`プロパティが、にイベントハンドラーのマップに渡されます`CommandName`追加ボタン (Insert) の値。 さらに、必要がありますものみ進みます検証コントロールが有効なデータを報告する場合。 これに合わせて、イベント ハンドラーを作成、`RowCommand`次のコードでイベント。


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample6.vb)]

> [!NOTE]
> でしょうか、イベント ハンドラーのチェックが許容できない理由、`Page.IsValid`プロパティ。 結局のところ、受注 t、ポストバックでは、無効なデータが挿入インターフェイスに指定されている場合を抑制するか。 そうでは、ユーザーは、JavaScript が無効またはクライアント側検証ロジックを回避する手段である限り正しい。 つまり、1 つ必要がありますに依存しないように厳密にクライアント側の検証サーバー側の有効性チェックは、データを操作する前に常に実行する必要があります。


手順 1. で作成した、 `ProductsDataSource` ObjectDataSource ようにその`Insert()`をメソッドにマップされて、`ProductsBLL`クラスの`AddProduct`メソッド。 新しいレコードを挿入するのには、`Products`テーブル、ObjectDataSource s を単に呼び出すことができます`Insert()`メソッド。


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample7.vb)]

これで、`Insert()`メソッドが呼び出されてに渡されたままの挿入のインターフェイスからパラメーターに値をコピーするは、`ProductsBLL`クラスの`AddProduct`メソッド。 説明したように、 [、イベントに関連付けられている挿入、更新、および削除の確認](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)チュートリアルでは、これは、ObjectDataSource s を通じて実現できます`Inserting`イベント。 `Inserting`イベントからコントロールをプログラムで参照する必要があります、 `Products` GridView のフッター行し、それらの値を割り当てる、`e.InputParameters`コレクション。 ユーザーは、終了などの値を指定しない場合、`ReorderLevel`テキスト ボックスに空のデータベースに挿入された値があることを指定する必要があります`NULL`します。 以降、`AddProducts`メソッドは null 許容のデータベース フィールドに null 許容型を受け取ります null 許容型を使用して、単にその値に設定`Nothing`の場合はユーザー入力を省略するとします。


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample8.vb)]

`Inserting`イベント ハンドラーが完了した、新しいレコードに追加することができます、 `Products` GridView のフッター行を使用してデータベース テーブル。 さあ、いくつかの新しい製品を追加してみてください。

## <a name="enhancing-and-customizing-the-add-operation"></a>拡張およびカスタマイズするには、操作の追加

現時点では、[追加] ボタンをクリックすると、データベース テーブルに新しいレコードを追加は提供されませんあらゆる種類の視覚的なフィードバック、レコードが正常に追加されたこと。 理想的には、Label Web コントロールやクライアント側の警告ボックスは、正常に完了しましたが、挿入をユーザーに通知します。 ままを演習として、リーダーの。

このチュートリアルで使用される GridView では、以下の製品には、任意の並べ替え順序は当てはまりませんもデータを並べ替えるには、エンドユーザーは許可します。 その結果、レコードは、主キー フィールドでは、データベースのように並べ替えられます。 新しい各レコードがあるため、`ProductID`最後の 1 つのグリッドの最後に数行の新しい製品が追加されるたびにより大きい値です。 そのため、新しいレコードを追加した後、GridView の最後のページにユーザーを自動的に送信したい場合があります。 これは、呼び出しの後に次のコード行を追加することで実現できます`ProductsDataSource.Insert()`で、`RowCommand`をユーザーが最後のページ内にデータを GridView にバインドした後は送信する必要があることを示すためにイベント ハンドラー。


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample9.vb)]

`SendUserToLastPage` 値が割り当てられますが最初にページ レベルのブール値変数`False`します。 GridView s`DataBound`イベント ハンドラーを場合`SendUserToLastPage`が false の場合、`PageIndex`最後のページをユーザーに送信するプロパティを更新します。


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample10.vb)]

理由、`PageIndex`プロパティで設定されて、`DataBound`イベント ハンドラー (ではなく、`RowCommand`イベント ハンドラー) がときに、`RowCommand`イベント ハンドラーを起動新しいレコードを追加するには、まだ ve、`Products`データベース テーブル。 そのため、`RowCommand`イベント ハンドラーの最後のページ インデックス (`PageCount - 1`) 最後のページ インデックスを表す*する前に*新製品が追加されました。 追加されている製品のほとんどは、最後のページ インデックスは、同じを新しい製品を追加した後です。 正しくない更新と、新しい最後のページ インデックスに追加された製品が発生する場合は、`PageIndex`で、`RowCommand`そのイベント ハンドラーは新しい最後のページの i ではなくの 2 番目の最後のページ (新しい製品を追加する前に最後のページ インデックス) に移動しますあります。 以降、`DataBound`イベント ハンドラーが、新しい成果物が追加され、データが設定して、グリッドに再バインド後に起動、`PageIndex`プロパティがありますがわかっています re 正しいの最後のページ インデックスを取得します。

最後に、このチュートリアルで使用される GridView が新しい製品を追加するために収集する必要がありますフィールドの数が非常にさまざまです。 この幅のため DetailsView s の縦方向のレイアウトが優先される可能性があります。 GridView の秒数の入力を収集することによって削減できた全体の幅。 おそらくことを収集する必要はありません、 `UnitsOnOrder`、 `UnitsInStock`、および`ReorderLevel`フィールドの新しい製品を追加するときに GridView から場合これらのフィールドを削除できます。

収集されたデータを調整するには、2 つの方法のいずれかを使用できます。

- 引き続き使用する、`AddProduct`の値を必要とするメソッド、 `UnitsOnOrder`、 `UnitsInStock`、および`ReorderLevel`フィールド。 `Inserting`イベント ハンドラーでは、ハード コーディングされた提供、既定の挿入のインターフェイスから削除されたこれらの入力に使用する値。
- 新しいオーバー ロードを作成、`AddProduct`メソッドで、`ProductsBLL`の入力を受け付けないクラス、 `UnitsOnOrder`、 `UnitsInStock`、および`ReorderLevel`フィールド。 次に、ASP.NET ページでは、この新しいオーバー ロードを使用する ObjectDataSource を構成します。

いずれかのオプションはも同様に動作します。 過去のチュートリアルを使用して、後者のオプションの複数のオーバー ロードを作成、`ProductsBLL`クラスの`UpdateProduct`メソッド。

## <a name="summary"></a>まとめ

GridView、DetailsView や FormView、組み込みの挿入機能が不足していますが、少しの努力でフッター行に挿入のインターフェイスを追加できます。 単に GridView のフッター行を表示する次のように設定します。 その`ShowFooter`プロパティを`True`します。 フィールドを TemplateField に変換すると、各フィールドのフッター行の内容をカスタマイズでき、挿入を追加するためのインターフェイス、`FooterTemplate`します。 このチュートリアルで説明したように、`FooterTemplate`ボタン、テキスト ボックス、Dropdownlist、チェック ボックス、Dropdownlist) などのデータ ドリブンの Web コントロールを設定するためのデータ ソース コントロール、および検証コントロールを含めることができます。 ユーザーの入力を収集するためのコントロール、と共に、ボタンの追加、LinkButton や ImageButton が必要です。

[追加] ボタンがクリックされたとき、ObjectDataSource の`Insert()`挿入のワークフローを開始するメソッドが呼び出されます。 ObjectDataSource は、構成されている insert メソッドを呼び出して (、`ProductsBLL`クラスの`AddProduct`メソッドは、このチュートリアルでは)。 ObjectDataSource にインターフェイスを挿入する GridView s から値をコピーする必要があります`InsertParameters`insert メソッドが呼び出される前に収集します。 これは、ObjectDataSource s で挿入インターフェイス Web コントロールをプログラムで参照することで実現できます`Inserting`イベント ハンドラー。

このチュートリアルでは、GridView の外観を強化するための手法で、外観を完了します。 次の一連のチュートリアルは、画像、Pdf、Word 文書などのバイナリ データを操作する方法とデータ Web コントロールを調べます。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、「社長補佐 Leigh でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](adding-a-gridview-column-of-checkboxes-vb.md)
