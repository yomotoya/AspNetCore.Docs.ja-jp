---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
title: その他のデータ テーブルの列 (c#) を追加する |Microsoft ドキュメント
author: rick-anderson
description: TableAdapter ウィザードを使用して、型指定されたデータセットを作成する、対応するデータ テーブルには、主なデータベース クエリによって返される列が含まれています。 しかし、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 615f3361-f21f-4338-8bc1-fce8ae071de9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 9c2232c9867fc605a5b5d3973d4dbe31895841ca
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877503"
---
<a name="adding-additional-datatable-columns-c"></a>その他の DataTable の列を追加する (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_CS.zip)または[PDF のダウンロード](adding-additional-datatable-columns-cs/_static/datatutorial70cs1.pdf)

> TableAdapter ウィザードを使用して、型指定されたデータセットを作成する、対応するデータ テーブルには、主なデータベース クエリによって返される列が含まれています。 DataTable の追加列を含める必要がある場合に必要です。 このチュートリアルでは、追加のデータ テーブルの列が必要なときにストアド プロシージャを推奨する理由は学習します。


## <a name="introduction"></a>はじめに

に型指定されたデータセットを TableAdapter を追加するときに、対応する DataTable のスキーマは、TableAdapter のメイン クエリによって決まります。 たとえば、メインのクエリがデータ フィールドを返します*A*、 *B*、および*C*、DataTable はという 3 つの対応する列が*A*、*B*、および*C*です。メインのクエリだけでなく、TableAdapter は、おそらく、いくつかのパラメーターに基づくデータのサブセットを返す他のクエリを含めることができます。 インスタンスに加え、`ProductsTableAdapter`すべての製品に関する情報を返します、s メイン クエリも含まれますなどのメソッド`GetProductsByCategoryID(categoryID)`と`GetProductByProductID(productID)`、指定されたパラメーターに基づいて特定の製品情報を返します。

TableAdapter のメインのクエリを反映 DataTable のスキーマを持つのモデルは、同じか、またはメイン クエリで指定されているよりも少ないデータ フィールドを返す TableAdapter のメソッドの場合はうまく機能します。 TableAdapter メソッドは、追加のデータ フィールドを返す必要があるを場合しおを展開する DataTable のスキーマに応じて。 [マスター/詳細レコードを使用して、箇条書きリストのマスター詳細 DataList で](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)チュートリアルにメソッドを追加しました、`CategoriesTableAdapter`返される、 `CategoryID`、 `CategoryName`、および`Description`で定義されているデータ フィールドプラスのメイン クエリ`NumberOfProducts`、各カテゴリに関連付けられている製品の数を報告する追加のデータ フィールドです。 新しい列を手動で追加して、`CategoriesDataTable`をキャプチャするために、`NumberOfProducts`データ フィールドの値この新しいメソッドからです。

説明したように、[ファイルのアップロード](../working-with-binary-files/uploading-files-cs.md)チュートリアル、優れたは注意が必要、アドホック SQL ステートメントを使用する、その結果、データ フィールドは、メインのクエリを正確に一致しないメソッドが Tableadapter にします。 場合は、TableAdapter 構成ウィザードを再実行、更新されます TableAdapter のメソッドのすべてのデータ フィールドの一覧が、メインのクエリと一致するようにします。 その結果、カスタマイズされた列リストを持つ任意のメソッドは、メイン クエリの列リストに戻すし、予期されたデータは返されません。 ストアド プロシージャを使用する場合、この問題は発生しません。

このチュートリアルでは、追加の列を含める DataTable のスキーマを拡張する方法について取り上げます。 このチュートリアルでは、アドホック SQL ステートメントを使用する場合、TableAdapter の脆弱性、によりストアド プロシージャを使用します。 参照してください、[を作成する新しいストアド プロシージャを型指定されたデータセットの Tableadapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)と[を使用して既存のストアド プロシージャを型指定されたデータセットの Tableadapter](https://data-access/tutorials/using-existing-stored-procedures-for-the-typed-dataset-amp-rsquo-s-tableadapters-cs)についてのチュートリアルストアド プロシージャを使用して TableAdapter を構成します。

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>手順 1: 追加、`PriceQuartile`列を`ProductsDataTable`

*を作成する新しいストアド プロシージャを型指定されたデータセットの Tableadapter*という名前の型指定されたデータセットを作成したチュートリアル`NorthwindWithSprocs`です。 このデータセットには現在 2 つのデータ テーブルが含まれています:`ProductsDataTable`と`EmployeesDataTable`です。 `ProductsTableAdapter`次の 3 つの方法があります。

- `GetProducts` メインのクエリは、すべてのレコードを返します、`Products`テーブル
- `GetProductsByCategoryID(categoryID)` -指定したすべての製品を返します*categoryID*です。
- `GetProductByProductID(productID)` -指定した特定の製品を返します*productID*です。

メインのクエリと 2 つの追加のメソッドは、つまりすべての列のデータ フィールドの同じセットを返す、`Products`テーブル。 相関サブクエリがないか`JOIN`s 関連データを抽出、`Categories`または`Suppliers`テーブル。 したがって、`ProductsDataTable`の各フィールドの対応する列を持つ、`Products`テーブル。

このチュートリアルでは、let s メソッドを追加する、`ProductsTableAdapter`という`GetProductsWithPriceQuartile`を返すすべての製品です。 フィールドだけでなく、標準の製品データ、`GetProductsWithPriceQuartile`も含まれる、`PriceQuartile`どの四分位数の下の製品の価格があることを示します。 データ フィールドです。 たとえば、価格が最も高コストの 25% ではこれらの製品がある、`PriceQuartile`値 1 を下の 25% で価格が含まれている 4 の値が使用されます。 前に、この情報を返すストアド プロシージャを作成する心配し、ただし、まず必要がありますを更新する、`ProductsDataTable`を保持する列を含めるように、`PriceQuartile`結果ときに、`GetProductsWithPriceQuartile`メソッドを使用します。

開く、`NorthwindWithSprocs`データセットを右クリックし、`ProductsDataTable`です。 コンテキスト メニューから追加] をクリックし、[列を選択します。


[![ProductsDataTable に新しい列を追加します。](adding-additional-datatable-columns-cs/_static/image2.png)](adding-additional-datatable-columns-cs/_static/image1.png)

**図 1**: 新しい列を追加、 `ProductsDataTable` ([フルサイズのイメージを表示するをクリックして](adding-additional-datatable-columns-cs/_static/image3.png))


これには、型の Column1 のという DataTable に新しい列が追加`System.String`です。 PriceQuartile とその型にするこの列の名前を更新する必要があります`System.Int32`ので、これは 1 ~ 4 の数値を保持するために使用されます。 新しく追加された列を選択、`ProductsDataTable`し、[プロパティ] ウィンドウから次のように設定します。、 `Name` PriceQuartile プロパティおよび`DataType`プロパティを`System.Int32`です。


[![新しい列の名前とデータ型のプロパティを設定します。](adding-additional-datatable-columns-cs/_static/image5.png)](adding-additional-datatable-columns-cs/_static/image4.png)

**図 2**: 新しい列 s を設定`Name`と`DataType`プロパティ ([フルサイズのイメージを表示するをクリックして](adding-additional-datatable-columns-cs/_static/image6.png))


図 2 に示すかどうか、列の値が、列が自動増分列の場合は、一意にする必要がありますなど、設定できる追加のプロパティがある、かどうか、データベース`NULL`値が許可されなどです。 既定値にこれらの値のままにします。

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>手順 2: 作成、`GetProductsWithPriceQuartile`メソッド

これで、`ProductsDataTable`に含める更新されました、`PriceQuartile`を作成する準備ができました 列、`GetProductsWithPriceQuartile`メソッドです。 TableAdapter を右クリックして、コンテキスト メニューから [クエリの追加] を選択して開始します。 まず us、アドホック SQL ステートメントまたは新規または既存のストアド プロシージャを使用するかどうかに関する入力を求められる TableAdapter クエリの構成ウィザードが表示されます。 ないお価格の四分位数のデータを返すストアド プロシージャがあるため、ご利用の米国このストアド プロシージャを作成する TableAdapter を許可する秒を使用できます。 新しいストアド プロシージャの作成 オプションを選択し、次へ をクリックします。


[![TableAdapter ウィザードでご利用の米国ストアド プロシージャを作成するように指示します。](adding-additional-datatable-columns-cs/_static/image8.png)](adding-additional-datatable-columns-cs/_static/image7.png)

**図 3**: TableAdapter ウィザードで、ストアド プロシージャの意見を作成するように指示 ([フルサイズのイメージを表示するをクリックして](adding-additional-datatable-columns-cs/_static/image9.png))


後続の画面では、図 4 に示すように、ウィザード要求することを追加するクエリの種類。 以降、`GetProductsWithPriceQuartile`メソッドは、すべての列とからレコードを返します、`Products`テーブル、行のオプションと 次へ をクリックする を選択します。


[![このクエリは SELECT ステートメントを複数行を返しますになります](adding-additional-datatable-columns-cs/_static/image11.png)](adding-additional-datatable-columns-cs/_static/image10.png)

**図 4**: Our クエリになります、`SELECT`ステートメントを複数行を返します ([フルサイズのイメージを表示するをクリックして](adding-additional-datatable-columns-cs/_static/image12.png))


次のよう求められます、`SELECT`クエリ。 ウィザードには、次のクエリを入力します。


[!code-sql[Main](adding-additional-datatable-columns-cs/samples/sample1.sql)]

上記のクエリでは SQL Server 2005 s の新しい[`NTILE`関数](https://msdn.microsoft.com/library/ms175126.aspx)によって、グループを決定する、4 つのグループに結果を分割する、`UnitPrice`値の降順で並べ替えられます。

残念ながら、クエリ ビルダーに解析する方法を識別できない、`OVER`キーワードと、上記のクエリの解析中にエラーが表示されます。 そのため、クエリ ビルダーを使用せず、ウィザードで、テキスト ボックス内で直接上記のクエリを入力します。

> [!NOTE]
> その他の順位付け関数を参照してください NTILE の SQL Server 2005 の詳細については[Microsoft SQL Server 2005 で順位付けされた結果を返す](http://www.4guysfromrolla.com/webtech/010406-1.shtml)と[」の「順位付け関数](https://msdn.microsoft.com/library/ms189798.aspx)から、 [SQLServer 2005 Books Online](https://msdn.microsoft.com/library/ms189798.aspx)です。


入力した後に、`SELECT`クエリ、[次へ] をクリックして、ウィザードが自動的に作成するストアド プロシージャの名前を指定することです。 新しいストアド プロシージャの名前を付けます`Products_SelectWithPriceQuartile`[次へ] をクリックします。


[![名前のストアド プロシージャ Products_SelectWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image14.png)](adding-additional-datatable-columns-cs/_static/image13.png)

**図 5**: ストアド プロシージャの名前を付けます`Products_SelectWithPriceQuartile`([フルサイズのイメージを表示するをクリックして](adding-additional-datatable-columns-cs/_static/image15.png))


最後に、TableAdapter のメソッドの名前を付けるように求められます。 DataTable、両方の塗りつぶしのままにし、データ テーブルのチェック ボックスがオンになって、名メソッド`FillWithPriceQuartile`と`GetProductsWithPriceQuartile`です。


[![名前、tableadapter のメソッドとをクリックして終了します。](adding-additional-datatable-columns-cs/_static/image17.png)](adding-additional-datatable-columns-cs/_static/image16.png)

**図 6**: TableAdapter のメソッドとは [完了] の名前を付けます ([フルサイズのイメージを表示するをクリックして](adding-additional-datatable-columns-cs/_static/image18.png))


`SELECT`クエリを指定し、ストアド プロシージャとという名前の TableAdapter メソッドは、ウィザードを完了するの完了 をクリックします。 この時点で受け取ることがあります、警告またはことを示すウィザードから 2 つ、 `OVER` SQL コンストラクトまたはステートメントはサポートされていません。 これらの警告を無視できます。

ウィザードを完了すると、TableAdapter を含める必要があります、`FillWithPriceQuartile`と`GetProductsWithPriceQuartile`メソッドと、データベースは、という名前のストアド プロシージャを含める必要があります`Products_SelectWithPriceQuartile`です。 すぐに TableAdapter がこの新しいメソッドを含む実際に、ストアド プロシージャがデータベースに正しく追加されていることを確認します。 Stored Procedures フォルダーを右クリックしてお試しをストアド プロシージャおよび更新 をクリックして表示されない場合は、データベースを確認して ときに。


![新しいメソッドが TableAdapter に追加されたことを確認してください。](adding-additional-datatable-columns-cs/_static/image19.png)

**図 7**: 新しいメソッドが TableAdapter に追加されたことを確認してください。


[![データベースが、Products_SelectWithPriceQuartile が含まれていることを確認ストアド プロシージャ](adding-additional-datatable-columns-cs/_static/image21.png)](adding-additional-datatable-columns-cs/_static/image20.png)

**図 8**: データベース含まれていることを確認、`Products_SelectWithPriceQuartile`ストアド プロシージャ ([フルサイズのイメージを表示するをクリックして](adding-additional-datatable-columns-cs/_static/image22.png))


> [!NOTE]
> アドホック SQL ステートメントの代わりにストアド プロシージャを使用する利点の 1 つは、TableAdapter 構成ウィザードを再実行は変更されませんストアド プロシージャの列リストです。 TableAdapter を右クリックして、ウィザードを起動するコンテキスト メニューから構成オプションを選択し、完了するには、[完了] をクリックし、これを確認します。 次に、データベース、ビューにアクセスして、`Products_SelectWithPriceQuartile`ストアド プロシージャです。 その列リストが変更されていないことに注意してください。 ここで使用されていた、アドホック SQL ステートメント、TableAdapter 構成ウィザードを再実行は元に戻さ NTILE ステートメントで使用されるクエリから削除、メインのクエリ列リストに一致するこのクエリの列リスト、`GetProductsWithPriceQuartile`メソッドです。


ときに、データ アクセス層 s`GetProductsWithPriceQuartile`メソッドが呼び出され、TableAdapter の実行、`Products_SelectWithPriceQuartile`ストアド プロシージャと、行を追加、`ProductsDataTable`返されたレコードのです。 ストアド プロシージャによって返されるデータ フィールドのマップを`ProductsDataTable`の列です。 存在しないため、`PriceQuartile`その値は、ストアド プロシージャから返されたデータ フィールドに割り当てられて、 `ProductsDataTable` s`PriceQuartile`列です。

クエリでそのを返さない TableAdapter メソッド、`PriceQuartile`データ フィールド、 `PriceQuartile` s の列の値によって指定された値は、その`DefaultValue`プロパティです。 図 2 では、この設定は`DBNull`、既定値です。 別の既定値を使用する場合場合、設定するだけ、`DefaultValue`プロパティに応じて。 同じことを確認、`DefaultValue`値が有効では、[s] 列を指定`DataType`(つまり、`System.Int32`の`PriceQuartile`列)。

この時点で、必要な手順を実行おの列を DataTable に追加します。 この追加の列が期待どおりに動作することを確認するには、秒の各製品の名前、価格、および価格の四分位数を表示するための ASP.NET ページを作成することができます。 その前に、まず必要がありますをに、DAL s が呼び出すメソッドを含めるビジネス ロジック層を更新する`GetProductsWithPriceQuartile`メソッドです。 おによりが手順 3 で BLL を次に、更新され、手順 4. で、ASP.NET ページを作成します。

## <a name="step-3-augmenting-the-business-logic-layer"></a>手順 3: ビジネス ロジック層の拡張

新しいを使用する前に`GetProductsWithPriceQuartile`メソッド プレゼンテーション層から必要がありますまず対応するメソッドに追加 BLL です。 開く、`ProductsBLLWithSprocs`クラス ファイルと、次のコードを追加します。


[!code-csharp[Main](adding-additional-datatable-columns-cs/samples/sample2.cs)]

その他のデータの取得のメソッドと同様`ProductsBLLWithSprocs`、`GetProductsWithPriceQuartile`メソッドは、DAL を呼び出すだけ s が対応する`GetProductsWithPriceQuartile`メソッドし、その結果を返します。

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>手順 4: ASP.NET Web ページの価格の四分位数情報を表示します。

追加すると、BLL には、各製品の価格の四分位数を表示する ASP.NET ページを作成する準備ができたらおを完了します。 開く、 `AddingColumns.aspx`  ページで、`AdvancedDAL`フォルダーと、デザイナーの設定には、ツールボックスからドラッグ、GridView、`ID`プロパティを`Products`です。 GridView s のスマート タグからにバインドして、という名前の新しい ObjectDataSource`ProductsDataSource`です。 構成を使用する ObjectDataSource、`ProductsBLLWithSprocs`クラスの`GetProductsWithPriceQuartile`メソッドです。 これは読み取り専用グリッドになる、ため、更新、挿入でドロップダウン リストを設定し、(なし) にタブを削除します。


[![構成、ObjectDataSource ProductsBLLWithSprocs クラスを使用するには](adding-additional-datatable-columns-cs/_static/image24.png)](adding-additional-datatable-columns-cs/_static/image23.png)

**図 9**: 構成を使用する ObjectDataSource、`ProductsBLLWithSprocs`クラス ([フルサイズのイメージを表示するをクリックして](adding-additional-datatable-columns-cs/_static/image25.png))


[![GetProductsWithPriceQuartile メソッドから製品情報を取得します。](adding-additional-datatable-columns-cs/_static/image27.png)](adding-additional-datatable-columns-cs/_static/image26.png)

**図 10**: 製品情報を取得、`GetProductsWithPriceQuartile`メソッド ([フルサイズのイメージを表示するをクリックして](adding-additional-datatable-columns-cs/_static/image28.png))


データ ソース構成ウィザードを完了すると、Visual Studio が自動的に追加 BoundField または CheckBoxField GridView の各メソッドによって返されるデータ フィールドのです。 これらのデータ フィールドの 1 つは`PriceQuartile`に追加した列は、`ProductsDataTable`手順 1. でします。

削除する GridView s のフィールドを編集以外のすべての`ProductName`、 `UnitPrice`、および`PriceQuartile`BoundFields です。 構成、 `UnitPrice` BoundField を通貨としてその値の書式を設定し、`UnitPrice`と`PriceQuartile`BoundFields 右揃え、中央揃え、それぞれします。 最後に、残りの BoundFields を更新`HeaderText`プロパティを製品、価格、およびコストの 25%、それぞれします。 また、GridView s のスマート タグからの並べ替えを有効にするチェック ボックスを確認します。

これらの変更後は、GridView と ObjectDataSource s の宣言型マークアップを次のようになります。


[!code-aspx[Main](adding-additional-datatable-columns-cs/samples/sample3.aspx)]

図 11 は、ブラウザーからアクセスすると、このページを示します。 最初に、製品順に適切な割り当てられている各 product に降順での価格に注意してください`PriceQuartile`値。 もちろんこのデータを並べ替えるその他の条件によって価格に関して、製品の順位付けをまだ反映価格の四分位数の列値を持つ (図 12 を参照してください)。


[![製品はその価格順に並べ替えられます。](adding-additional-datatable-columns-cs/_static/image30.png)](adding-additional-datatable-columns-cs/_static/image29.png)

**図 11**: この製品はその価格順に並べ替えられます ([フルサイズのイメージを表示するには、をクリックして](adding-additional-datatable-columns-cs/_static/image31.png))。


[![製品は、名前順に並べ替えられます。](adding-additional-datatable-columns-cs/_static/image33.png)](adding-additional-datatable-columns-cs/_static/image32.png)

**図 12**: この製品は、名前順に並べ替えられます ([フルサイズのイメージを表示するには、をクリックして](adding-additional-datatable-columns-cs/_static/image34.png))。


> [!NOTE]
> 基にして製品の行を色分けするよう、いくつかの行のコードで GridView を補強おでした、`PriceQuartile`値。 最初の四分位数、2 番目の四分位数、明るい黄色で明るい緑でこれらの製品を色やなどお可能性があります。 ぜひ、この機能を追加します。 GridView の書式設定の更新を必要がある場合を参照してください、[カスタム書式指定ベース時にデータ](../custom-formatting/custom-formatting-based-upon-data-cs.md)チュートリアルです。


## <a name="an-alternative-approach---creating-another-tableadapter"></a>その他の方法のもう 1 つの TableAdapter を作成します。

見たようこのチュートリアルでは、メインのクエリによって略さず以外のデータ フィールドを返す TableAdapter にメソッドを追加するときに、対応する列には、DataTable に追加できます。 このようなアプローチにより、ただしを別のデータ フィールドを返す、TableAdapter のメソッドの数が少ないがある場合にのみ、およびそれらの代替データ フィールドが、メイン クエリから大きく変化しない場合に効果的に機能します。

列を DataTable に追加するではなく別のデータ フィールドを取得する最初の TableAdapter のメソッドを含むデータセットを代わりに別の TableAdapter を追加できます。 このチュートリアルでは、追加するのではなく、`PriceQuartile`列を`ProductsDataTable`(使用されている場所だけで、`GetProductsWithPriceQuartile`メソッド)、という名前のデータセットに追加の TableAdapter を追加おでした`ProductsWithPriceQuartileTableAdapter`ために使用される、`Products_SelectWithPriceQuartile`格納されています。そのメイン クエリとしてプロシージャです。 価格の四分位数で製品情報を取得するために必要な ASP.NET ページを使用、`ProductsWithPriceQuartileTableAdapter`を使用し続けることがしなかったものになる一方で、`ProductsTableAdapter`です。

新しい TableAdapter を追加すると、データ テーブルが untarnished ままになり、列は、TableAdapter のメソッドによって返されるデータ フィールドを正確にミラー化します。 ただし、追加の Tableadapter には、反復的なタスクと機能を導入できます。 たとえば、これら ASP.NET ページを表示、`PriceQuartile`列もために必要なため、挿入、更新、および削除のサポート、`ProductsWithPriceQuartileTableAdapter`必要とするその`InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティ正しく構成されています。 これらのプロパティはミラー化中に、 `ProductsTableAdapter` s、この構成、余分なステップが導入されています。 さらはこれで、更新するには 2 つの方法は、削除、または経由のデータベースに製品を追加、`ProductsTableAdapter`と`ProductsWithPriceQuartileTableAdapter`クラスです。

このチュートリアルでのダウンロードに含まれています、`ProductsWithPriceQuartileTableAdapter`クラス内で、`NorthwindWithSprocs`データセットをこの方法を示しています。

## <a name="summary"></a>まとめ

ほとんどのシナリオでデータ フィールドの同じセットを返すすべての TableAdapter のメソッドが特定のメソッドまたは 2 つの追加フィールドを返す必要があります。 たとえば、[マスター/詳細 DataList でマスター レコードの箇条書きリストの使用について詳しく説明](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)チュートリアルにメソッドを追加しました、 `CategoriesTableAdapter` s のメイン クエリのデータ フィールドが返されるだけでなくを`NumberOfProducts`フィールド各カテゴリに関連付けられている製品の数を報告します。 このチュートリアルでは見てでメソッドを追加する、`ProductsTableAdapter`返される、`PriceQuartile`メイン クエリのデータ フィールドだけでなくフィールドです。 追加のデータをキャプチャするには、は、フィールドは DataTable に対応する列を追加する必要があります TableAdapter のメソッドによって返されます。

DataTable に列を手動で追加する場合は、TableAdapter がストアド プロシージャを使用することをお勧めします。 TableAdapter は、アドホック SQL ステートメントを使用して、いつでも、TableAdapter 構成ウィザード実行のメインのクエリによって返されるデータ フィールドにデータ フィールドのリストを元に戻す方法はすべてされます。 この問題には拡張されませんストアド プロシージャは、これが推奨される、このチュートリアルで使用されていた理由。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、ものです。 Schmidt、武島 Goor、「社長補佐 Leigh および Hilton Giesenow でした。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](updating-the-tableadapter-to-use-joins-cs.md)
> [次へ](working-with-computed-columns-cs.md)
