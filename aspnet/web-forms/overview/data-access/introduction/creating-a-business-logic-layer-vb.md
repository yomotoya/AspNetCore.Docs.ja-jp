---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
title: ビジネス ロジック層 (VB) を作成する |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは t 間のデータ交換の仲介役として機能するビジネス ロジック層 (BLL) に、ビジネス ルールを集中管理する方法を見ていきます。
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 142e5181-29ce-4bb9-907b-2a0becf7928b
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 345f4981ebdd5384068bd42bce0581f94866ad1d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832373"
---
<a name="creating-a-business-logic-layer-vb"></a>ビジネス ロジック層 (VB) を作成します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_2_VB.exe)または[PDF のダウンロード](creating-a-business-logic-layer-vb/_static/datatutorial02vb1.pdf)

> このチュートリアルでは、プレゼンテーション層と DAL 間のデータ交換の仲介役として機能するビジネス ロジック層 (BLL) に、ビジネス ルールを集中管理する方法がわかります。


## <a name="introduction"></a>はじめに

作成したデータ アクセス層 (DAL)、[最初のチュートリアル](creating-a-data-access-layer-vb.md)明確に分離、データ アクセス ロジックをプレゼンテーション ロジックから。 ただし、DAL は、プレゼンテーション層から、データ アクセスの詳細を明確に区分、中に適用される任意のビジネス ルールは実行されません。 たとえば、アプリケーションのすることも許可しないように、`CategoryID`または`SupplierID`のフィールド、`Products`ときに変更するテーブル、`Discontinued`フィールドが 1 に設定または状況は禁止勤続の規則を適用する可能性があります、従業員が入社後にユーザーによって管理されます。 もう 1 つの一般的なシナリオは、特定のロール内のユーザーの承認などのみが製品を削除できますまたはを変更することができます、`UnitPrice`値。

このチュートリアルでは、プレゼンテーション層と DAL 間のデータ交換の仲介役として機能するビジネス ロジック層 (BLL) にこれらのビジネス ルールを集中管理する方法がわかります。 実際のアプリケーションで、別のクラス ライブラリ プロジェクトとして、BLL を実装する必要があります。ただし、これらのチュートリアルを実装します BLL を一連のクラスとして、`App_Code`プロジェクト構造を簡略化するためのフォルダー。 図 1 は、プレゼンテーション層、BLL は、DAL 間のアーキテクチャの関係を示しています。


![BLL はデータ アクセス層とプレゼンテーション層を分離し、ビジネス ルールを課す](creating-a-business-logic-layer-vb/_static/image1.png)

**図 1**: BLL はデータ アクセス層とプレゼンテーション層を分離し、ビジネス ルールを課す


実装する別のクラスを作成するのではなく、[ビジネス ロジック](http://en.wikipedia.org/wiki/Business_logic)、部分クラスで型指定されたデータセットに直接またはこのロジックを配置できます。 作成と型指定されたデータセットを拡張の例は、最初のチュートリアルに戻る参照してください。

## <a name="step-1-creating-the-bll-classes"></a>手順 1: BLL クラスを作成します。

BLL は DAL; の各 TableAdapter に 1 つ、4 つのクラスから構成されます。各 BLL クラスを取得、挿入、更新、および適切なビジネス ルールの適用、DAL でそれぞれ TableAdapter から削除するためのメソッドとなります。

明確にするには、DAL に BLL 関連クラスを区切りますで 2 つのサブフォルダーを作成してみましょう、`App_Code`フォルダー、`DAL`と`BLL`します。 右クリック、`App_Code`ソリューション エクスプ ローラーでフォルダーの新しいフォルダーを選択します。 これら 2 つのフォルダーを作成した後に最初のチュートリアルで作成された型指定されたデータセットを移動、`DAL`サブフォルダーです。

内の 4 つの BLL クラス ファイルを次に、作成、`BLL`サブフォルダーです。 これを行うを右クリックし、`BLL`サブフォルダーを追加、新しいアイテムの選択し、クラス テンプレートを選択します。 4 つのクラスの名前を付けます`ProductsBLL`、 `CategoriesBLL`、 `SuppliersBLL`、および`EmployeesBLL`します。


![4 つの新しいクラスを App_Code フォルダーに追加します。](creating-a-business-logic-layer-vb/_static/image2.png)

**図 2**: 4 つの新しいクラスを追加、`App_Code`フォルダー


次に、単に最初のチュートリアルから Tableadapter に定義されたメソッドをラップするクラスの各メソッドを追加しましょう。 ここでは、これらのメソッドはだけ; DAL を直接呼び出しますすべての必要なビジネス ロジックを追加する後で取り上げます。

> [!NOTE]
> Visual Studio Standard Edition を使用している場合、またはの上 (なら、*いない*Visual Web Developer を使用して)、視覚的に使用して、クラスを設計することができます必要に応じて、[クラス デザイナー](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp)します。 参照してください、[クラス デザイナー ブログ](https://blogs.msdn.com/classdesigner/default.aspx)Visual Studio のこの新機能の詳細についてはします。


`ProductsBLL`クラスの 7 つのメソッドの合計を追加する必要があります。

- `GetProducts()` すべての製品を返します
- `GetProductByProductID(productID)` 指定された製品 ID の積を返します
- `GetProductsByCategoryID(categoryID)` 指定したカテゴリからすべての製品を返します
- `GetProductsBySupplier(supplierID)` 指定された業者からのすべての製品を返します
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` 値を使用してデータベースに新しい製品を挿入します。 渡されたのです。返します、`ProductID`新しく挿入されたレコードの値
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` 渡された値では; を使用して、データベース内の既存の製品を更新します。返します`True`正確に 1 つの行が更新された場合`False`それ以外の場合
- `DeleteProduct(productID)` データベースから、指定された製品を削除します。

ProductsBLL.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample1.vb)]

単にデータを返すメソッド`GetProducts`、 `GetProductByProductID`、 `GetProductsByCategoryID`、および`GetProductBySuppliersID`に DAL 呼び出すだけとは、きわめて単純です。 一部のシナリオでもありますがビジネス ルールを実装する必要があります (承認規則は、現在ログオンしているユーザーまたはユーザーが所属するロールに基づく) などのこのレベルで単のままにしますとしてこれらのメソッドには。 これらのメソッドを次に、BLL だけでプロキシとして機能、プレゼンテーション層、データ アクセス層から基になるデータにアクセスします。

`AddProduct`と`UpdateProduct`メソッド両方パラメーターとして製品のさまざまなフィールドの値取得し新しい商品を追加または既存のものをそれぞれ更新します。 多く、`Product`テーブルの列を受け入れることができます`NULL`値 (`CategoryID`、`SupplierID`と`UnitPrice`、いくつかの名前を付ける)、入力パラメーターを`AddProduct`と`UpdateProduct`そのような列の使用にマップします。[null 許容型](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx)します。 Null 許容型は .NET 2.0 に新しいと方法を指定するかを示すかどうか、値型、代わりに、`Nothing`します。 参照してください、 [Paul Vick](http://www.panopticoncentral.net/)のブログ エントリ[null 許容型について、真実と VB](http://www.panopticoncentral.net/archive/2004/06/04/1180.aspx)と技術文書には、 [Nullable](https://msdn.microsoft.com/library/b3h38hb0%28VS.80%29.aspx)詳細については、構造体。

3 つのメソッドは、行の電源が挿入されていること、更新、または削除操作が影響を受ける行にならない可能性がありますのでかどうかを示すブール値を返します。 ページの開発者が呼び出す場合など`DeleteProduct`を渡して、 `ProductID` 、存在しない製品、 `DELETE` 、データベースへの発行ステートメントには影響はありませんし、そのため、`DeleteProduct`メソッドは`False`。

新しい製品を追加するときに注意して、既存のものを更新しましたのまたは新しいまたは変更された製品のフィールドの値を受け入れるのではなくスカラーのリストとして、`ProductsRow`インスタンス。 に、このアプローチが選択されました、`ProductsRow`クラスは、ADO.NET から派生`DataRow`クラスで、既定のパラメーターなしのコンス トラクターはありません。 新規作成するために`ProductsRow`インスタンス、する必要があります最初に作成、`ProductsDataTable`インスタンス化しを呼び出してその`NewProductRow()`メソッド (で行っている`AddProduct`)。 この欠点は、挿入と ObjectDataSource を使用して、製品の更新を有効にするとき、頭で木の実をこの。 簡単に言えば、ObjectDataSource は、入力パラメーターのインスタンスを作成してください。 BLL メソッドが必要な場合、`ProductsRow`インスタンス、ObjectDataSource が作成するか、既定のパラメーターなしのコンス トラクターがないのため失敗する可能性が再試行してください。 この問題の詳細については、次の 2 つの ASP.NET フォーラム投稿を参照してください: [Strongly-Typed データセット更新 ObjectDataSources](https://forums.asp.net/1098630/ShowPost.aspx)、および[ObjectDataSource で問題と Strongly-Typed データセット](https://forums.asp.net/1048212/ShowPost.aspx).

次に、両方で`AddProduct`と`UpdateProduct`、コードを作成、`ProductsRow`インスタンスし、単に渡された値を設定します。 DataRow の DataColumns に値を割り当てるときにさまざまなフィールド レベルの検証チェックが発生することができます。 そのため、手動で渡された値に戻す DataRow は、BLL メソッドに渡されるデータの妥当性を確認します。 残念ながら Visual Studio によって生成される厳密に型指定された DataRow クラスには、null 許容型は使用しません。 代わりに、DataRow で特定の DataColumn に対応することを示す、`NULL`データベースの値を使用しなければならない、`SetColumnNameNull()`メソッド。

`UpdateProduct`を使用して更新する製品で最初に読み込む`GetProductByProductID(productID)`します。 思われるかもしれませんが、データベースへの不要なアクセス、この追加のトリップをオプティミスティック同時実行制御を紹介するチュートリアルが将来に価値のある証明されます。 オプティミスティック同時実行制御は、同じデータを同時に作業している 2 つのユーザーが 1 つに相互の変更を誤って上書きされないようにする手法です。 レコード全体の取得も簡単で DataRow の列のサブセットにのみ変更 BLL 更新メソッドを作成します。 紹介すると、`SuppliersBLL`クラスのような例を見てみましょう。

最後に、注意、`ProductsBLL`クラスには、 [DataObject 属性](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx)が適用されて (、`[System.ComponentModel.DataObject]`ファイルの先頭付近にあるクラス ステートメントの直前の構文)、メソッドが[DataObjectMethodAttribute 属性](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx)します。 `DataObject`属性でマークへのバインドに適したオブジェクトとして、クラス、 [ObjectDataSource コントロール](https://msdn.microsoft.com/library/9a4kyhcx.aspx)であるのに対し、`DataObjectMethodAttribute`メソッドの目的を示します。 ご覧のとおりでは、今後のチュートリアル、ASP.NET 2.0 の ObjectDataSource 簡単に宣言によって、クラスのデータにアクセスします。 ObjectDataSource のウィザード内でバインド可能なクラスの一覧をフィルター処理するには、既定では、としてマークされているクラスだけ`DataObjects`ウィザードのドロップダウン リストに表示されます。 `ProductsBLL`これらの属性のないクラスはでも動作しますが、それらを追加すると、ObjectDataSource のウィザードで操作しやすくします。

## <a name="adding-the-other-classes"></a>その他のクラスを追加します。

`ProductsBLL`クラスの完全なあとカテゴリ、サプライヤー、および従業員を操作するためのクラスを追加します。 次のクラスと上記の例からの概念を使用してメソッドを作成する時間がかかります。

- **CategoriesBLL.cs**

    - `GetCategories()`
    - `GetCategoryByCategoryID(categoryID)`
- **SuppliersBLL.cs**

    - `GetSuppliers()`
    - `GetSupplierBySupplierID(supplierID)`
    - `GetSuppliersByCountry(country)`
    - `UpdateSupplierAddress(supplierID, address, city, country)`
- **EmployeesBLL.cs**

    - `GetEmployees()`
    - `GetEmployeeByEmployeeID(employeeID)`
    - `GetEmployeesByManager(managerID)`

注目すべき 1 つのメソッドは、`SuppliersBLL`クラスの`UpdateSupplierAddress`メソッド。 このメソッドは、業者のアドレス情報だけを更新するためのインターフェイスを提供します。 内部的には、このメソッドの読み込み、`SupplierDataRow`指定したオブジェクト`supplierID`(を使用して`GetSupplierBySupplierID`)、そのアドレスに関連するプロパティを設定し呼び出して、`SupplierDataTable`の`Update`メソッド。 `UpdateSupplierAddress`メソッド次のとおりです。


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample2.vb)]

この記事のダウンロード、BLL クラスの完全な実装を参照してください。

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>手順 2: BLL クラスを使用して型指定されたデータセットにアクセスします。

最初のチュートリアルではプログラムでは、型指定されたデータセットを直接操作する例を説明しましたが、BLL クラスの追加により、プレゼンテーション層は動作 BLL に対して代わりにします。 `AllProducts.aspx`から最初のチュートリアルでは、例、`ProductsTableAdapter`に次のコードに示すように、GridView に製品の一覧をバインドに使用されました。


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample3.vb)]

コードの最初の行を置き換えるだけですが変更が必要されるクラスの新しい BLL を使用するが、`ProductsTableAdapter`オブジェクトを`ProductBLL`オブジェクト。


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample4.vb)]

BLL クラスは、ObjectDataSource を使用して (型指定されたデータセットのことができます)、宣言によってもアクセスできます。 ここで説明するさらに詳しく ObjectDataSource で、次のチュートリアル。


[![GridView に製品の一覧が表示されます。](creating-a-business-logic-layer-vb/_static/image4.png)](creating-a-business-logic-layer-vb/_static/image3.png)

**図 3**: GridView に、製品一覧が表示されます ([フルサイズの画像を表示する をクリックします](creating-a-business-logic-layer-vb/_static/image5.png))。


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>手順 3: DataRow クラスにフィールド レベルの検証を追加します。

フィールド レベルの検証は、挿入または更新するときに、ビジネス オブジェクトのプロパティの値に関連するチェックです。 製品の一部のフィールド レベルの検証規則は次のとおりです。

- `ProductName`フィールドは 40 文字、または以下でなければなりません
- `QuantityPerUnit`フィールドが 20 文字、または以下でなければなりません
- `ProductID`、 `ProductName`、および`Discontinued`フィールドが必要ですが、その他のすべてのフィールドは省略可能です
- `UnitPrice`、 `UnitsInStock`、 `UnitsOnOrder`、および`ReorderLevel`フィールドが 0 以上にする必要があります

これらの規則ができ、データベース レベルで表す必要があります。 文字の制限、`ProductName`と`QuantityPerUnit`フィールドは、これらの列のデータ型によってキャプチャされた、`Products`テーブル (`nvarchar(40)`と`nvarchar(20)`、それぞれ)。 フィールドは必須および省略可能なのかどうか、データベース テーブルの列を許可する場合にで表現されます`NULL`秒。 次の 4 つ[check 制約](https://msdn.microsoft.com/library/ms188258.aspx)存在ゼロ以上の値のみがそれを実行できることを確認する、 `UnitPrice`、 `UnitsInStock`、 `UnitsOnOrder`、または`ReorderLevel`列。

データベースでのこれらの規則を適用するだけでなくもを適用するデータセットのレベル。 実際には、フィールド長、値が必須または省略可能なのかどうか既に DataColumns のそれぞれの DataTable のセットをキャプチャします。 自動的に提供される既存のフィールド レベルの検証を表示するには、データセット デザイナーに移動し、Datatable のいずれかからフィールドを選択して、[プロパティ] ウィンドウに移動し。 図 4 に示すよう、`QuantityPerUnit`で DataColumn、`ProductsDataTable`が 20 文字の最大長を許可して`NULL`値。 設定する場合、`ProductsDataRow`の`QuantityPerUnit`プロパティを 20 文字より長い文字列値に、`ArgumentException`がスローされます。


[![DataColumn は、フィールド レベルの基本的な検証を提供します。](creating-a-business-logic-layer-vb/_static/image7.png)](creating-a-business-logic-layer-vb/_static/image6.png)

**図 4**: The DataColumn は、基本的なフィールド レベルの検証 ([フルサイズの画像を表示する をクリックします](creating-a-business-logic-layer-vb/_static/image8.png))。


残念ながら、指定できない、境界のチェックなど、`UnitPrice`プロパティ ウィンドウで、0 以上の値がある必要があります。 この種類のフィールド レベルの検証を提供するために、DataTable のイベント ハンドラーを作成する必要[ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx)イベント。 説明したように、[前のチュートリアル](creating-a-data-access-layer-vb.md)、部分クラスを使用して型指定されたデータセットによって作成された DataSet、Datatable、および DataRow オブジェクトを拡張することができます。 作成するこの手法を使用して、`ColumnChanging`のイベント ハンドラー、`ProductsDataTable`クラス。 クラスを作成して開始、`App_Code`という名前のフォルダー`ProductsDataTable.ColumnChanging.vb`します。


[![App_Code フォルダーに新しいクラスを追加します。](creating-a-business-logic-layer-vb/_static/image10.png)](creating-a-business-logic-layer-vb/_static/image9.png)

**図 5**: 新しいクラスを追加、`App_Code`フォルダー ([フルサイズの画像を表示する をクリックします](creating-a-business-logic-layer-vb/_static/image11.png))。


イベント ハンドラーを次に、作成、`ColumnChanging`によりイベント、 `UnitPrice`、 `UnitsInStock`、 `UnitsOnOrder`、および`ReorderLevel`列の値 (ない場合`NULL`) より大きいかゼロに等しい。 このような任意の列が範囲外にある場合は、スロー、`ArgumentException`します。

ProductsDataTable.ColumnChanging.vb


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample5.vb)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>手順 4: BLL のクラスにカスタムのビジネス ルールを追加します。

フィールド レベルの検証に加えてなどさまざまなエンティティまたは 1 つの列レベルでは表現されません概念に関連する高度なカスタムのビジネス ルールがあります。

- 製品が提供が中止された場合、`UnitPrice`を更新できません
- 従業員のお住まいの国には、それぞれのマネージャーのお住まいの国と同じである必要があります。
- 供給業者によって提供される唯一の製品である場合、製品を廃止ことはできません。

BLL クラスには、アプリケーションのビジネス ルールに準拠していることを確認するためのチェックを含める必要があります。 これらのチェックを適用するメソッドに直接追加できます。

当社のビジネス ルールは、ある製品マークできませんでした提供が中止された場合は、指定された業者からの唯一の製品がディクテーションを想像してください。 つまり場合、製品*X*が唯一の製品を供給業者から購入します*Y*、私たちに設定できませんでした*X*廃止; 場合、ただし、業者*Y*3 つの製品に付属している米国*A*、 *B*、および*C*、任意に設定でしたおよびとしてこれらすべての提供が中止されました。 奇数のビジネス ルールがビジネス ルールと一般的な意味が合って常に!

場合は、このビジネス ルールを適用する、`UpdateProducts`メソッドをチェックすることで始めたい`Discontinued`に設定された`True`を呼び出すと、場合`GetProductsBySupplierID`製品の数を判断するこの製品の仕入先から購入します。 1 つの製品のこの業者から購入だけの場合にスローします、`ApplicationException`します。


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample6.vb)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>プレゼンテーション層の検証エラーへの応答

プレゼンテーション層から、BLL を呼び出すときに例外が発生したまたは ASP.NET バブリングそれらを処理しようとするかどうかを判断できます (これが発生、`HttpApplication`の`Error`イベント)。 BLL をプログラムで使用する場合は、例外を処理するには、使用、[お試しください.キャッチ](https://msdn.microsoft.com/library/fk6t46tz%28VS.80%29.aspx)ブロックは、次の例を示します。


[!code-vb[Main](creating-a-business-logic-layer-vb/samples/sample7.vb)]

後でチュートリアルを見て、データを使用する場合、BLL からバブルアップ例外の処理 Web コントロールの挿入、更新、またはでコードをラップすることではなく、イベント ハンドラーで直接処理するデータを削除することができます、`Try...Catch`ブロックします。

## <a name="summary"></a>まとめ

うまく設計されたアプリケーションの構造は、異なるレイヤーにそれぞれが特定のロールをカプセル化します。 この記事シリーズの最初のチュートリアルで型指定されたデータセット; を使用して、データ アクセス層を作成しましたこのチュートリアルではビジネス ロジック層としてビルド一連のクラスをアプリケーションでの`App_Code`DAL 呼び出せるフォルダー。 BLL は、フィールド レベルとビジネス レベル、アプリケーション ロジックを実装します。 個別 BLL を作成するだけでなく、このチュートリアルで行ったように別のオプションを部分クラスを使用して、Tableadapter のメソッドを拡張するがします。 ただし、この手法を使用しないことができない既存のメソッドをオーバーライドするとを分離、DAL、BLL 今回略述アプローチとして明確にします。

DAL BLL 完了と、プレゼンテーション層で開始する準備ができました。 [次のチュートリアル](master-pages-and-site-navigation-vb.md)トピックへのアクセスのデータから、少し迂回をチュートリアル全体で使用するための一貫したページ レイアウトを定義します。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者はされた Liz Shulok、Dennis Patterson、Carlos の Santos、Hilton Giesenow です。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](creating-a-data-access-layer-vb.md)
> [次へ](master-pages-and-site-navigation-vb.md)
