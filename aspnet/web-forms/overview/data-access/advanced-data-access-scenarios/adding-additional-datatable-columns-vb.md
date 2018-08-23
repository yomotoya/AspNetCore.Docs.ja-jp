---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
title: その他の DataTable 列 (VB) の追加 |Microsoft Docs
author: rick-anderson
description: TableAdapter ウィザードを使用して、型指定されたデータセットを作成する、対応するデータ テーブルには、主なデータベース クエリによって返される列が含まれています。 しかし、.
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 1e8e65f9-fe3e-4250-810b-c90227786bed
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 75b5a1e1d6beb00079d754601860d0c25bc8a23e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837165"
---
<a name="adding-additional-datatable-columns-vb"></a>その他の DataTable 列を追加する (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_VB.zip)または[PDF のダウンロード](adding-additional-datatable-columns-vb/_static/datatutorial70vb1.pdf)

> TableAdapter ウィザードを使用して、型指定されたデータセットを作成する、対応するデータ テーブルには、主なデータベース クエリによって返される列が含まれています。 機会、DataTable の追加列を含める必要がある場合があります。 このチュートリアルではは、追加の DataTable 列必要なときにストアド プロシージャが推奨される理由について説明します。


## <a name="introduction"></a>はじめに

型指定されたデータセットに TableAdapter を追加するときに、対応するデータ テーブルのスキーマは、TableAdapter のメイン クエリによって決まります。 たとえば、メインのクエリは、データ フィールドを返します*A*、 *B*、および*C*、DataTable がという名前の 3 つの対応する列が*A*、*B*、および*C*します。TableAdapter は、メインのクエリだけでなく、おそらく、いくつかのパラメーターに基づくデータのサブセットを返す追加のクエリを含めることができます。 加え、たとえば、 `ProductsTableAdapter` s メイン クエリのすべての製品に関する情報を返すメソッドも格納など`GetProductsByCategoryID(categoryID)`と`GetProductByProductID(productID)`、指定されたパラメーターに基づく特定の製品情報を返すをします。

DataTable のスキーマを TableAdapter のメイン クエリを反映することのモデルは、同じか、またはメイン クエリで指定されているよりも少ないデータ フィールドを返す TableAdapter のメソッドのすべての場合も動作します。 TableAdapter メソッドは、追加のデータ フィールドを返す必要がある場合、する必要がありますスキーマを拡張 DataTable s それに応じて。 [マスター/詳細のマスター レコードの箇条書きリストを使用すると詳細 DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)チュートリアルへのメソッドを追加しました、`CategoriesTableAdapter`返される、 `CategoryID`、`CategoryName`と`Description`で定義されているデータ フィールドプラスのメイン クエリ`NumberOfProducts`、各カテゴリに関連付けられている製品の数を報告する追加のデータ フィールド。 新しい列を手動で追加しました、`CategoriesDataTable`をキャプチャするために、`NumberOfProducts`データ フィールドの値この新しいメソッドから。

説明したように、[のファイルのアップロード](../working-with-binary-files/uploading-files-vb.md)ケアのチュートリアルはすばらしいことですが、アドホック SQL ステートメントを使用し、データ フィールドは、メイン クエリを正確に一致しないメソッドを持つ Tableadapter を使用して実行する必要があります。 データ フィールドの一覧がメインのクエリと一致するようにすべての TableAdapter のメソッド、TableAdapter 構成ウィザードを再実行とサーバー オブジェクトが更新されます。 そのため、カスタマイズされた列のリストを持つすべてのメソッドは、メイン クエリの列リストに戻すし、予想されるデータは返されません。 ストアド プロシージャを使用する場合、この問題は発生しません。

このチュートリアルでは、追加の列を含める DataTable のスキーマを拡張する方法に注目します。 このチュートリアルでは、アドホック SQL ステートメントを使用する場合、TableAdapter の脆弱性、によりストアド プロシージャを使用します。 参照してください、[型指定されたデータセット s Tableadapter の新しいのストアド プロシージャの作成](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)と[型指定されたデータセット s Tableadapter の既存のストアド プロシージャの使用](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)の詳細についてはチュートリアルストアド プロシージャを使用して TableAdapter を構成します。

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>手順 1: 追加、`PriceQuartile`列を`ProductsDataTable`

*型指定されたデータセット s Tableadapter の新しいのストアド プロシージャの作成*という名前の型指定されたデータセットを作成したチュートリアル`NorthwindWithSprocs`します。 このデータセットには現在 2 つの Datatable が含まれています:`ProductsDataTable`と`EmployeesDataTable`します。 `ProductsTableAdapter`次の 3 つの方法があります。

- `GetProducts` -メインのクエリは、すべてのレコードを返します、`Products`テーブル
- `GetProductsByCategoryID(categoryID)` -指定したすべての製品を返します*categoryID*します。
- `GetProductByProductID(productID)` -指定した特定の製品を返します*productID*します。

メインのクエリと、2 つの追加のメソッドは、つまりすべての列のデータ フィールドの同じセットを返す、`Products`テーブル。 相関サブクエリがないまたは`JOIN`関連データを抽出して、`Categories`または`Suppliers`テーブル。 そのため、`ProductsDataTable`内の各フィールドの対応する列を持つ、`Products`テーブル。

このチュートリアルでは、let s を追加するメソッドを`ProductsTableAdapter`という`GetProductsWithPriceQuartile`すべての製品を返します。 標準の製品のデータ フィールドに加え`GetProductsWithPriceQuartile`も含まれる、`PriceQuartile`どの四分位数で製品の価格があることを示します。 データ フィールド。 たとえば、価格が最も高コストの 25% ではこれらの製品が必要があります、`PriceQuartile`値 1、4 の値がありますが、下部の 25% で価格が含まれています。 この情報を返すストアド プロシージャの作成について気にし、前に、まず必要がありますを更新する、`ProductsDataTable`を保持する列を含めるように、`PriceQuartile`結果と、`GetProductsWithPriceQuartile`メソッドを使用します。

開く、`NorthwindWithSprocs`データセットを右クリックし、`ProductsDataTable`します。 コンテキスト メニューから追加を選択し、列を選択してください。


[![ProductsDataTable に新しい列を追加します。](adding-additional-datatable-columns-vb/_static/image2.png)](adding-additional-datatable-columns-vb/_static/image1.png)

**図 1**: 新しい列を追加、 `ProductsDataTable` ([フルサイズの画像を表示する をクリックします](adding-additional-datatable-columns-vb/_static/image3.png))。


これは、新しい列がという名前の型の列 1 の DataTable に追加されます`System.String`します。 この列の名前に更新 PriceQuartile とその型にする必要があります`System.Int32`される 1 ~ 4 の数値を保持するために使用します。 新しく追加された列の選択、`ProductsDataTable`し、[プロパティ] ウィンドウから次のように設定します。、 `Name` PriceQuartile にプロパティと`DataType`プロパティを`System.Int32`します。


[![新しい列の名前とデータ型のプロパティを設定します。](adding-additional-datatable-columns-vb/_static/image5.png)](adding-additional-datatable-columns-vb/_static/image4.png)

**図 2**: 設定の新しい列 s`Name`と`DataType`プロパティ ([フルサイズの画像を表示する をクリックします](adding-additional-datatable-columns-vb/_static/image6.png))。


図 2 は、列の値を列が自動インクリメント列の場合は一意である必要があるかどうかなど、設定できる追加のプロパティは、データベースのかどうか`NULL`値を許可、および具合です。 これらの値が既定値に設定のままにします。

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>手順 2: 作成、`GetProductsWithPriceQuartile`メソッド

これで、`ProductsDataTable`を含めるが更新されました、`PriceQuartile`を作成する準備ができました 列、`GetProductsWithPriceQuartile`メソッド。 TableAdapter を右クリックし、コンテキスト メニューから追加のクエリを選択して開始します。 まず、アドホック SQL ステートメントまたは新規または既存のストアド プロシージャを使用するかどうかについて私たちを求められます TableAdapter クエリ構成ウィザードが表示されます。 後で私たちはないまだ価格の四分位数のデータを返すストアド プロシージャがある、s をこのストアド プロシージャを作成する TableAdapter を許可することができます。 新しいストアド プロシージャの作成 オプションを選択し、次へ をクリックします。


[![私たちにとって、ストアド プロシージャを作成する TableAdapter ウィザードの指示します。](adding-additional-datatable-columns-vb/_static/image8.png)](adding-additional-datatable-columns-vb/_static/image7.png)

**図 3**: TableAdapter ウィザードで、ストアド プロシージャの私たちを作成するように指示 ([フルサイズの画像を表示する をクリックします](adding-additional-datatable-columns-vb/_static/image9.png))。


図 4 に示すように、後続の画面で、ウィザード求められたときを追加するクエリの種類。 以降、`GetProductsWithPriceQuartile`メソッドは、すべての列とレコードを返します、`Products`テーブル、行のオプションと [次へ] を選択します。


[![クエリが SELECT ステートメントを返します。 その複数行になります](adding-additional-datatable-columns-vb/_static/image11.png)](adding-additional-datatable-columns-vb/_static/image10.png)

**図 4**: このクエリが行われる、`SELECT`ステートメントを複数行を返します ([フルサイズの画像を表示する をクリックします](adding-additional-datatable-columns-vb/_static/image12.png))。


次のよう求められます、`SELECT`クエリ。 ウィザードには、次のクエリを入力します。


[!code-sql[Main](adding-additional-datatable-columns-vb/samples/sample1.sql)]

上記のクエリを新しい SQL Server 2005 s 使用[`NTILE`関数](https://msdn.microsoft.com/library/ms175126.aspx)によって、グループを決定する、4 つのグループに、結果を分割する、`UnitPrice`値の降順に並べ替えられます。

残念ながら、クエリ ビルダーに解析する方法を把握されません、`OVER`キーワードと、上記のクエリの解析中にエラーが表示されます。 そのため、クエリ ビルダーを使用せず、ウィザードで、テキスト ボックス内で直接、上記のクエリを入力します。

> [!NOTE]
> NTILE と SQL Server 2005 に関する詳細については、他の順位付け関数を参照してください[Microsoft SQL Server 2005 でランク付けされた結果を返す](http://www.4guysfromrolla.com/webtech/010406-1.shtml)と[順位付け関数セクション](https://msdn.microsoft.com/library/ms189798.aspx)から、 [SQLServer 2005 Books Online](https://msdn.microsoft.com/library/ms189798.aspx)します。


入力した後、`SELECT`クエリ、[次へ] をクリックして、ウィザード求められたときに、作成、ストアド プロシージャの名前を指定します。 新しいストアド プロシージャの名前を付けます`Products_SelectWithPriceQuartile`[次へ] をクリックします。


[![ストアド プロシージャ Products_SelectWithPriceQuartile 名](adding-additional-datatable-columns-vb/_static/image14.png)](adding-additional-datatable-columns-vb/_static/image13.png)

**図 5**: ストアド プロシージャの名前を付けます`Products_SelectWithPriceQuartile`([フルサイズの画像を表示する をクリックします](adding-additional-datatable-columns-vb/_static/image15.png))。


最後に、TableAdapter のメソッドの名前を付けるように求められます。 両方の塗りつぶしの DataTable のままにし、DataTable のチェック ボックスがオンと名、メソッドが返されます`FillWithPriceQuartile`と`GetProductsWithPriceQuartile`します。


[![名前、tableadapter のメソッドとクリック完了します。](adding-additional-datatable-columns-vb/_static/image17.png)](adding-additional-datatable-columns-vb/_static/image16.png)

**図 6**: TableAdapter のメソッドとは [完了] の名前 ([フルサイズの画像を表示する をクリックします](adding-additional-datatable-columns-vb/_static/image18.png))。


`SELECT`クエリを指定し、ストアド プロシージャとという名前の TableAdapter メソッドは、ウィザードを完了するには、[完了] をクリックします。 この時点で警告または 2 つのことを示すウィザードから取得可能性があります、 `OVER` SQL コンストラクトまたはステートメントはサポートされていません。 これらの警告を無視できます。

ウィザードを完了すると、TableAdapter を含める必要があります、`FillWithPriceQuartile`と`GetProductsWithPriceQuartile`メソッドと、データベースは、という名前のストアド プロシージャを含める必要があります`Products_SelectWithPriceQuartile`します。 TableAdapter がこの新しいメソッドを含む実際とストアド プロシージャがデータベースに正しく追加されたことを確認する時間がかかります。 ときに、ストアド プロシージャ フォルダーを右クリックしてお試しくださいをストアド プロシージャと更新 を選択が表示されない場合は、データベースを確認しています。


![新しいメソッドが TableAdapter に追加されたことを確認します。](adding-additional-datatable-columns-vb/_static/image19.png)

**図 7**: 新しいメソッドが TableAdapter に追加されたことを確認します。


[![データベースには、Products_SelectWithPriceQuartile ストアド プロシージャ](adding-additional-datatable-columns-vb/_static/image21.png)](adding-additional-datatable-columns-vb/_static/image20.png)

**図 8**: データベースの含まれていることを確認、`Products_SelectWithPriceQuartile`ストアド プロシージャ ([フルサイズの画像を表示する をクリックします](adding-additional-datatable-columns-vb/_static/image22.png))。


> [!NOTE]
> アドホック SQL ステートメントの代わりにストアド プロシージャを使用する利点の 1 つは、TableAdapter 構成ウィザードを再実行が変更しないこと、ストアド プロシージャの列リストです。 TableAdapter を右クリックして、ウィザードを開始するコンテキスト メニューから構成オプションを選択および完了するには、[完了] をクリックし、これを確認します。 次に、データベースとビューに移動、`Products_SelectWithPriceQuartile`ストアド プロシージャ。 その列リストが変更されていないことに注意してください。 ここで使用されていた、アドホック SQL ステートメント、TableAdapter 構成ウィザードを再実行が元に戻す NTILE ステートメントで使用されるクエリから削除、メイン クエリ列リストに一致するようにこのクエリの列のリスト、`GetProductsWithPriceQuartile`メソッド。


ときにデータ アクセス層 s`GetProductsWithPriceQuartile`メソッドが呼び出される、TableAdapter の実行、`Products_SelectWithPriceQuartile`ストアド プロシージャとに行を追加、`ProductsDataTable`返されたレコードの。 ストアド プロシージャによって返されるデータ フィールドのマップ、`ProductsDataTable`の列。 あるため、 `PriceQuartile` 、ストアド プロシージャから返されるデータ フィールドの値に割り当てられている、 `ProductsDataTable` s`PriceQuartile`列。

クエリでそのを返さない TableAdapter メソッド、`PriceQuartile`データ フィールドに、 `PriceQuartile` s の列の値によって指定された値は、その`DefaultValue`プロパティ。 この値に設定して、図 2 に示す`DBNull`、既定値。 別の既定値は場合は、設定、`DefaultValue`プロパティに応じて。 大丈夫、`DefaultValue`値が有効な列 %s を指定して`DataType`(つまり、`System.Int32`の`PriceQuartile`列)。

この時点での列を DataTable に追加のために必要な手順を実行しました。 この列の追加が正常に動作することを確認するには、s の各製品の名前、価格、および価格の四分位数を表示する ASP.NET ページを作成することができます。 その前に、まず必要がありますをに、DAL s が呼び出すメソッドを含めるビジネス ロジック層を更新する`GetProductsWithPriceQuartile`メソッド。 手順 3 で BLL を次に、更新、手順 4. で、ASP.NET ページを作成おされます。

## <a name="step-3-augmenting-the-business-logic-layer"></a>手順 3: ビジネス ロジック層の拡張

新しい使用前に`GetProductsWithPriceQuartile`メソッド、プレゼンテーション層から必要がありますまず対応するメソッドに追加、BLL します。 開く、`ProductsBLLWithSprocs`クラス ファイルと、次のコードを追加します。


[!code-vb[Main](adding-additional-datatable-columns-vb/samples/sample2.vb)]

その他のデータ取得メソッドと同様`ProductsBLLWithSprocs`、`GetProductsWithPriceQuartile`メソッドは、DAL を呼び出すだけです s が対応する`GetProductsWithPriceQuartile`メソッドの結果を返します。

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>手順 4: ASP.NET Web ページで価格の四分位数情報を表示します。

BLL の追加には、各製品の価格の四分位数を示す ASP.NET ページを作成する準備ができたらを完了します。 開く、`AddingColumns.aspx`ページで、`AdvancedDAL`フォルダーと、デザイナーの設定には、ツールボックスからドラッグ、GridView、`ID`プロパティを`Products`します。 GridView のスマート タグからという名前の新しい ObjectDataSource にバインド`ProductsDataSource`します。 構成を使用する ObjectDataSource、`ProductsBLLWithSprocs`クラスの`GetProductsWithPriceQuartile`メソッド。 これは読み取り専用グリッドになる、ため、UPDATE、INSERT でドロップダウン リストを設定し、(None) にタブを削除します。


[![ProductsBLLWithSprocs クラスを使用する ObjectDataSource を構成します。](adding-additional-datatable-columns-vb/_static/image24.png)](adding-additional-datatable-columns-vb/_static/image23.png)

**図 9**: 構成に使用する ObjectDataSource、`ProductsBLLWithSprocs`クラス ([フルサイズの画像を表示する をクリックします](adding-additional-datatable-columns-vb/_static/image25.png))。


[![GetProductsWithPriceQuartile メソッドから製品情報を取得します。](adding-additional-datatable-columns-vb/_static/image27.png)](adding-additional-datatable-columns-vb/_static/image26.png)

**図 10**: から製品情報の取得、`GetProductsWithPriceQuartile`メソッド ([フルサイズの画像を表示する をクリックします](adding-additional-datatable-columns-vb/_static/image28.png))。


データ ソース構成ウィザードを完了すると、Visual Studio が自動的に追加 BoundField または CheckBoxField GridView の各メソッドによって返されるデータ フィールドの。 これらのデータ フィールドの 1 つは`PriceQuartile`、これは、列を追加しました、`ProductsDataTable`手順 1. でします。

GridView のフィールドを削除する編集はすべて、 `ProductName`、`UnitPrice`と`PriceQuartile`BoundFields します。 構成、`UnitPrice`その値を通貨として書式設定し、BoundField、`UnitPrice`と`PriceQuartile`BoundFields 右揃え、中央揃え、それぞれします。 最後に、残りの BoundFields を更新`HeaderText`プロパティ製品、価格、および価格の四分位数をそれぞれします。 また、GridView s のスマート タグから並べ替えを有効にするチェック ボックスを確認します。

これらの変更後に GridView コントロールと ObjectDataSource s 宣言型マークアップは、次のようになります。


[!code-aspx[Main](adding-additional-datatable-columns-vb/samples/sample3.aspx)]

図 11 では、ブラウザーからアクセスしたときに、このページを示します。 最初に、製品順に適切な割り当てられている各製品での降順での価格に注意してください。`PriceQuartile`値。 まだ価格に関して、製品の順位付けを反映した価格の四分位数の列値が他の条件でコースのこのデータの並べ替えは (図 12 を参照してください)。


[![製品は、価格順に並べ替えられます。](adding-additional-datatable-columns-vb/_static/image30.png)](adding-additional-datatable-columns-vb/_static/image29.png)

**図 11**: The 製品は、価格で並べ替えられます ([フルサイズの画像を表示する をクリックします](adding-additional-datatable-columns-vb/_static/image31.png))。


[![製品は、名前順に並べ替えられます。](adding-additional-datatable-columns-vb/_static/image33.png)](adding-additional-datatable-columns-vb/_static/image32.png)

**図 12**: The 製品は、名前で並べ替えられます ([フルサイズの画像を表示する をクリックします](adding-additional-datatable-columns-vb/_static/image34.png))。


> [!NOTE]
> 数行のコードはでしたの強化点、GridView に基づく製品の行を表示できるように、`PriceQuartile`値。 最初の四分位数の 2 番目の四分位数淡い黄色の場合は、明るい緑には、これらの製品を色し、など可能性があります。 この機能を追加する少しことをお勧めします。 GridView の書式設定の更新機能が必要な場合を参照してください、[カスタム書式設定時にデータ](../custom-formatting/custom-formatting-based-upon-data-vb.md)チュートリアル。


## <a name="an-alternative-approach---creating-another-tableadapter"></a>別のアプローチのもう 1 つの TableAdapter を作成します。

このチュートリアルで説明したメインのクエリを簡潔にメソッド以外のデータ フィールドを返す TableAdapter を追加するときに、ように対応する列を DataTable に追加できます。 このアプローチ、ただし、少数のさまざまなデータ フィールドを返すことは TableAdapter のメソッドがある場合にのみ、およびこれらの代替データ フィールドは、メイン クエリからあまり変化しない場合に効果的に機能します。

列を DataTable に追加するのではなくをさまざまなデータ フィールドを返す最初の TableAdapter のメソッドを含むデータセットをもう 1 つの TableAdapter を代わりに追加できます。 このチュートリアルでは、追加ではなく、`PriceQuartile`列を`ProductsDataTable`(によって使用されてのみ、`GetProductsWithPriceQuartile`メソッド)、という名前のデータセットに追加の TableAdapter を追加しましたでした`ProductsWithPriceQuartileTableAdapter`使用、`Products_SelectWithPriceQuartile`格納されています。そのメイン クエリとしてプロシージャです。 価格の四分位数で製品情報を取得するために必要な ASP.NET ページを使用、`ProductsWithPriceQuartileTableAdapter`を引き続き使用するものではありませんでしたが、`ProductsTableAdapter`します。

新しい TableAdapter を追加すると、データ テーブルのまま untarnished とその列は、TableAdapter のメソッドによって返されるデータ フィールドを正確にミラー化します。 ただし、追加の Tableadapter には、反復的なタスクと機能を導入できます。 例では、その場合の ASP.NET ページを表示、`PriceQuartile`列もために必要なため、挿入、更新、および削除のサポート、`ProductsWithPriceQuartileTableAdapter`必要とするその`InsertCommand`、`UpdateCommand`と`DeleteCommand`プロパティ適切構成されています。 これらのプロパティが反映中に、`ProductsTableAdapter`秒で、この構成は、追加の手順を紹介します。 さらが 2 つの方法で更新、削除、または経由のデータベースに製品を追加、`ProductsTableAdapter`と`ProductsWithPriceQuartileTableAdapter`クラス。

このチュートリアルでは、ダウンロードが含まれています、`ProductsWithPriceQuartileTableAdapter`クラス、`NorthwindWithSprocs`データセットこの代替アプローチを示します。

## <a name="summary"></a>まとめ

ほとんどのシナリオでデータ フィールドの同じセットを返すすべての TableAdapter で方法がありますが、特定のメソッドまたは 2 つの追加のフィールドを返す必要があります。 など、[マスター/詳細と詳細 DataList マスター レコードの箇条書きリストを使用して](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)チュートリアルへのメソッドを追加しました、`CategoriesTableAdapter`だけでなく、返されるメイン クエリのデータ フィールドを`NumberOfProducts`フィールド各カテゴリに関連付けられている製品の数を報告します。 このチュートリアルでは、内のメソッドを追加するところ、`ProductsTableAdapter`返される、`PriceQuartile`フィールドだけでなく、メイン クエリのデータ フィールド。 追加のデータをキャプチャするには、は、フィールドは DataTable に対応する列を追加する必要があります TableAdapter のメソッドによって返されます。

データ テーブルに列を手動で追加する場合は、TableAdapter がストアド プロシージャを使用することをお勧めします。 TableAdapter は、アドホック SQL ステートメントを使用する場合、いつ、TableAdapter 構成ウィザードを実行のすべてのデータ フィールドの一覧がメインのクエリによって返されるデータ フィールドに元に戻すメソッド。 この問題には拡張されませんストアド プロシージャは、推奨される、このチュートリアルで使用されていたためにです。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者はされた Randy Schmidt、Jacky Goor、「社長補佐 Leigh、および Hilton Giesenow です。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](updating-the-tableadapter-to-use-joins-vb.md)
> [次へ](working-with-computed-columns-vb.md)
