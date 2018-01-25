---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
title: "ビジネス ロジック層 (c#) を作成 |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルでは、t 間のデータ交換の仲介役として機能するビジネス ロジック層 (BLL) に、ビジネス ルールを集中管理する方法を思いますしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 85554606-47cb-4e4f-9848-eed9da579056
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 7518ddd11a05a9e3d5df85e3cf6ceffa09a25060
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-business-logic-layer-c"></a>ビジネス ロジック層 (c#) を作成します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_2_CS.exe)または[PDF のダウンロード](creating-a-business-logic-layer-cs/_static/datatutorial02cs1.pdf)

> このチュートリアルではプレゼンテーション層と DAL 間のデータ交換の仲介役として機能するビジネス ロジック層 (BLL) に、ビジネス ルールを集中管理する方法が表示されます。


## <a name="introduction"></a>はじめに

データ アクセス レイヤー (DAL) が作成された、[の最初のチュートリアル](creating-a-data-access-layer-cs.md)プレゼンテーション ロジックからクリーンに分離、データ アクセス ロジック。 ただし、DAL は、プレゼンテーション層から、データ アクセスの詳細を明確に区分、中に適用される任意のビジネス ルールは実行されません。 たとえば、アプリケーションのすることも許可しないように、`CategoryID`または`SupplierID`のフィールド、`Products`ときに変更するテーブル、`Discontinued`フィールドが 1 に設定されているか、必要になる勤続ルールを適用する場合に、禁止すること、従業員は、それらの後が採用されたすべてのユーザーによって管理されます。 もう 1 つの一般的なシナリオは、特定のロールのユーザーが承認おそらくだけが製品を削除できますか、変更できます、`UnitPrice`値。

このチュートリアルではプレゼンテーション層と DAL 間のデータ交換の仲介役として機能するビジネス ロジック層 (BLL) にこれらのビジネス ルールを集中管理する方法が表示されます。 実際のアプリケーションでは、別のクラス ライブラリ プロジェクト; として BLL を実装します。ただし、これらのチュートリアルについてを実装します BLL 一連のクラスとして、`App_Code`プロジェクトの構造を簡略化するためのフォルダーです。 図 1 は、プレゼンテーション層、BLL、DAL のアーキテクチャの関係を示します。


![BLL がデータ アクセス層とプレゼンテーション層を分離し、ビジネス ルールでは、](creating-a-business-logic-layer-cs/_static/image1.png)

**図 1**: BLL がデータ アクセス層とプレゼンテーション層を分離し、ビジネス ルールでは、


## <a name="step-1-creating-the-bll-classes"></a>手順 1: BLL クラスの作成

BLL がで構成されている各 TableAdapter DAL; 内の 1 つ、4 つのクラスこれらの各 BLL クラスにより、取得、挿入、更新、および適切なビジネス ルールの適用、DAL のそれぞれの TableAdapter から削除するためのメソッドがあります。

明確に分離 DAL および BLL に関連するクラス、内の 2 つのサブフォルダーを作成しましょう、`App_Code`フォルダー、`DAL`と`BLL`です。 右クリック、`App_Code`ソリューション エクスプ ローラーでフォルダーの新しいフォルダーを選択します。 これら 2 つのフォルダーを作成した後に最初のチュートリアルで作成された入力データセットに移動、`DAL`サブフォルダーです。

次の 4 つの BLL クラス ファイルを作成、`BLL`サブフォルダーです。 これを行うを右クリックし、`BLL`サブフォルダー、追加、新しい項目をクラス テンプレートを選択します。 4 つのクラスの名前を付けます`ProductsBLL`、 `CategoriesBLL`、 `SuppliersBLL`、および`EmployeesBLL`です。


![4 つの新しいクラスを App_Code フォルダーに追加します。](creating-a-business-logic-layer-cs/_static/image2.png)

**図 2**: に 4 つの新しいクラスを追加、`App_Code`フォルダー


次に、単に最初のチュートリアルから Tableadapter に定義されたメソッドをラップするクラスの各メソッドを追加してみましょう。 ここでは、これらのメソッドがだけ DAL; に直接呼び出す必要なビジネス ロジックを追加する以降を返します。

> [!NOTE]
> Visual Studio Standard Edition を使用しているか (できたら、*いない*Visual Web Developer を使用して)、視覚的に使用するクラスをデザインすることができます必要に応じて、[クラス デザイナー](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp)です。 参照してください、[クラス デザイナー ブログ](https://blogs.msdn.com/classdesigner/default.aspx)Visual Studio のこの新機能についての詳細。


`ProductsBLL`クラス 7 つのメソッドの合計を追加する必要があります。

- `GetProducts()`すべての製品を返します
- `GetProductByProductID(productID)`指定された製品 ID の積を返します
- `GetProductsByCategoryID(categoryID)`指定されたカテゴリからすべての製品を返します
- `GetProductsBySupplier(supplierID)`指定された業者からのすべての製品を返します
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)`値を使用してデータベースに新しい製品を挿入します。 渡されたのです。返します、`ProductID`新しく挿入したレコードの値
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)`渡されたの値を使用して、データベースの既存の製品を更新します。返します`true`正確に 1 つの行が更新された場合`false`それ以外の場合
- `DeleteProduct(productID)`指定した製品をデータベースから削除します。

ProductsBLL.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample1.cs)]

単にデータを返すメソッド`GetProducts`、 `GetProductByProductID`、 `GetProductsByCategoryID`、および`GetProductBySuppliersID`だけを呼び出すことがダウンして、DAL に、非常に簡単です。 一部のシナリオでもありますが実装する必要があるビジネス規則レベルでは、現在ログオンしているユーザーまたはユーザーが属しているロールに基づいて承認規則) など、単にままにこれらのメソッドには。 これらのメソッドでは、次に、BLL 単プロキシとして機能プレゼンテーション層、データ アクセス層から基になるデータにアクセスします。

`AddProduct`と`UpdateProduct`メソッド両方パラメーターとして製品のさまざまなフィールドの値取得し、新しい製品を追加または、既存のものをそれぞれ更新します。 以降の多く、`Product`テーブルの列を受け入れることができます`NULL`値 (`CategoryID`、 `SupplierID`、および`UnitPrice`、いくつかの例)、入力パラメーターを`AddProduct`と`UpdateProduct`このような列の使用にマップします。[null 許容型](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx)です。 Null 許容型は .NET 2.0 に追加された新しいおよび技法を提供するかどうか、値型は、代わりを示す`null`です。 C# でフラグを設定できます値型で null 許容型として追加することによって`?`、型の後に (など`int? x;`)。 参照してください、 [null 許容型](https://msdn.microsoft.com/library/1t3y8s4s.aspx)」の「、 [c# プログラミング ガイド](https://msdn.microsoft.com/library/67ef8sbd%28VS.80%29.aspx)詳細についてはします。

3 つのメソッドは、行の電源が挿入されていること、更新、または操作が影響を受ける行のされないことがあるために、削除かどうかを示すブール値を返します。 ページの開発者が呼び出す場合など、`DeleteProduct`で渡すこと、`ProductID`存在しない製品では、 `DELETE` 、データベースへの発行ステートメントには効果がありませんおよび、`DeleteProduct`メソッドが返す`false`です。

新しい製品を追加するときに注意してくださいか、既存の更新おのまたは新しいまたは変更された製品のフィールドの値を受け入れるではなくスカラーのリストとして、`ProductsRow`インスタンス。 この方法が選択された、 `ProductsRow` ADO.NET クラス`DataRow`クラスで、既定のパラメーターなしのコンス トラクターがありません。 新規作成するために`ProductsRow`インスタンスである必要があります最初に作成、`ProductsDataTable`インスタンスを起動し、その`NewProductRow()`メソッド (で行っている`AddProduct`)。 挿入して、ObjectDataSource を使用して製品の更新に移るときに、この欠点は、ヘッドをこのです。 つまり、ObjectDataSource しようとした入力パラメーターのインスタンスを作成します。 BLL メソッドが受け取る場合、`ProductsRow`インスタンス、ObjectDataSource は、1 つを作成し、既定のパラメーターなしのコンス トラクターがないのため失敗する可能性が試行されます。 この問題の詳細については、次の 2 つの ASP.NET フォーラム投稿を参照してください: [Strongly-Typed データセットで更新 ObjectDataSources](https://forums.asp.net/1098630/ShowPost.aspx)、および[ObjectDataSource で問題と Strongly-Typed データセット](https://forums.asp.net/1048212/ShowPost.aspx).

次に、両方で`AddProduct`と`UpdateProduct`、コードを作成、`ProductsRow`インスタンスし、単に渡された値を設定します。 DataRow の DataColumns に値を割り当てるときにさまざまなフィールド レベルの検証チェックが発生することができます。 そのため、手動で渡された値に戻す DataRow により、BLL メソッドに渡されるデータの妥当性。 残念ながら Visual Studio によって生成される厳密に型指定された DataRow クラスには、null 許容型は使用しません。 代わりに、DataRow で特定の DataColumn に対応することを示すために、`NULL`データベースの値を使用する必要があります、`SetColumnNameNull()`メソッドです。

`UpdateProduct`を使用して更新するには、この製品に最初に読み込むこと`GetProductByProductID(productID)`です。 このかもしれませんが、データベースへの不要なアクセスと同様に、この追加のトリップが証明されてオプティミスティック同時実行制御を探索するチュートリアルが将来の価値のあるいます。 オプティミスティック同時実行制御は、同じデータに対して同時に作業している 2 人のユーザーが 1 つに相互の変更を誤って上書きしないようにする手法です。 レコード全体をグラブもやすく DataRow の列のサブセットにのみ変更 BLL で update メソッドを作成します。 ナビゲートすると、`SuppliersBLL`クラスのような例を見てみましょう。

最後に、なお、`ProductsBLL`クラスには、 [DataObject 属性](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx)、適用される (、`[System.ComponentModel.DataObject]`右ステートメントの前に、クラス ファイルの先頭付近に構文) あり、メソッドを持つ[DataObjectMethodAttribute 属性](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx)です。 `DataObject`属性でマークされているバインディングに適したオブジェクトとクラス、 [ObjectDataSource コントロール](https://msdn.microsoft.com/library/9a4kyhcx.aspx)であるのに対し、`DataObjectMethodAttribute`メソッドの目的を示します。 思います将来のチュートリアル、ASP.NET 2.0 の ObjectDataSource 簡単に宣言してクラスのデータにアクセスできます。 ObjectDataSource のウィザードにバインド可能なクラスのリストをフィルター処理するには、既定では、としてマークされているクラスだけ`DataObjects`ウィザードのドロップダウン リストに表示されます。 `ProductsBLL`クラスなしでは機能と同様、これらの属性が、それらを追加すると、ObjectDataSource のウィザードで作業しやすくします。

## <a name="adding-the-other-classes"></a>その他のクラスを追加します。

`ProductsBLL`クラスが完了したら、必要がありますカテゴリ、供給業者、および従業員を操作するためのクラスを追加します。 次のクラスと上記の例からの概念を使用してメソッドを作成するのにはしばらくを実行します。

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

注目すべき 1 つのメソッドは、`SuppliersBLL`クラスの`UpdateSupplierAddress`メソッドです。 このメソッドは、仕入先のアドレスの情報だけを更新するためのインターフェイスを提供します。 このメソッドが内部的には、読み取り、 `SupplierDataRow` 、指定されたオブジェクト`supplierID`(を使用して`GetSupplierBySupplierID`)、そのアドレスに関連するプロパティを設定してからに、`SupplierDataTable`の`Update`メソッドです。 `UpdateSupplierAddress`メソッドに従います。


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample2.cs)]

この記事のダウンロード、BLL クラスの完全な実装を参照してください。

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>手順 2: BLL クラスを介して型指定されたデータセットにアクセスします。

プログラムでは、型指定されたデータセットを直接操作の例の最初のチュートリアルで見た BLL クラスの追加により、プレゼンテーション層使用する必要が BLL に対して代わりにします。 `AllProducts.aspx`から最初のチュートリアルでは、例を`ProductsTableAdapter`次のコードに示すように、製品の一覧を GridView にバインドする使用されました。


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample3.cs)]

使用して新しい BLL するものをすべて変更する必要があるクラスは単純に置き換えます。 最初のコード行、`ProductsTableAdapter`オブジェクトを、`ProductBLL`オブジェクト。


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample4.cs)]

BLL クラスは、ObjectDataSource を使用して (型指定されたデータセットをできます)、宣言によってアクセスすることもできます。 ここで説明するさらに詳しく ObjectDataSource 次のチュートリアルです。


[![GridView には、製品の一覧を表示してください。](creating-a-business-logic-layer-cs/_static/image4.png)](creating-a-business-logic-layer-cs/_static/image3.png)

**図 3**: GridView に、製品の一覧が表示されます ([フルサイズのイメージを表示するをクリックして](creating-a-business-logic-layer-cs/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>手順 3: DataRow クラスにフィールド レベルの検証を追加します。

フィールド レベルの検証には、挿入または更新すると、ビジネス オブジェクトのプロパティの値に関連する確認です。 製品の一部のフィールド レベルの検証ルールは、次のとおりです。

- `ProductName`フィールドは 40 文字の長さ
- `QuantityPerUnit`フィールドが 20 文字またはの長さにする必要があります
- `ProductID`、 `ProductName`、および`Discontinued`フィールドが必要ですが、その他のすべてのフィールドは省略可能
- `UnitPrice`、 `UnitsInStock`、 `UnitsOnOrder`、および`ReorderLevel`フィールドが 0 以上にする必要があります

これらの規則は、ことができ、データベース レベルで表す必要があります。 文字数の制限、`ProductName`と`QuantityPerUnit`フィールドがそれらの列のデータ型によってキャプチャされた、`Products`テーブル (`nvarchar(40)`と`nvarchar(20)`、それぞれ)。 場合により、データベース テーブルの列によって表されますフィールドは必須および省略可能なのかどうか`NULL`s。 次の 4 つ[check 制約](https://msdn.microsoft.com/library/ms188258.aspx)が存在するは、0 以上の値のみを実行できることを確認してください。、 `UnitPrice`、 `UnitsInStock`、 `UnitsOnOrder`、または`ReorderLevel`列です。

データベースでこれらの規則を適用するだけでなくする必要がありますも適用できます、データセット レベル。 実際には、フィールド長、および値が必須またはオプション既に DataColumns の各データ テーブルのセットをキャプチャします。 自動的に提供される既存のフィールド レベルの検証を表示するには、データセット デザイナーに切り替え、データ テーブルのいずれかからフィールドを選択し、[プロパティ] ウィンドウに移動します。 図 4 に示す、`QuantityPerUnit`に DataColumn、 `ProductsDataTable` 20 文字の最大長であるし、では、`NULL`値。 設定する場合、`ProductsDataRow`の`QuantityPerUnit`プロパティを文字列値が 20 文字以内に、`ArgumentException`がスローされます。


[![DataColumn は、フィールド レベルの基本的な検証を提供します。](creating-a-business-logic-layer-cs/_static/image7.png)](creating-a-business-logic-layer-cs/_static/image6.png)

**図 4**: の DataColumn は、基本的なフィールド レベルの検証 ([フルサイズのイメージを表示するをクリックして](creating-a-business-logic-layer-cs/_static/image8.png))


残念ながら、ことを指定できません範囲チェックなど、`UnitPrice`以上のプロパティ ウィンドウからの 0 に等しいを値として使用することがあります。 この種類のフィールド レベルの検証を提供するために必要がありますの DataTable のイベント ハンドラーを作成する[ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx)イベント。 説明したように、[前のチュートリアル](creating-a-data-access-layer-cs.md)、部分クラスを使用して型指定されたデータセットによって作成された DataSet、Datatable、および DataRow オブジェクトを拡張することができます。 作成できるよう、この手法を使用して、`ColumnChanging`のイベント ハンドラー、`ProductsDataTable`クラスです。 内のクラスを作成することで開始、`App_Code`という名前のフォルダー`ProductsDataTable.ColumnChanging.cs`です。


[![新しいクラスを App_Code フォルダーに追加します。](creating-a-business-logic-layer-cs/_static/image10.png)](creating-a-business-logic-layer-cs/_static/image9.png)

**図 5**: 新しいクラスを追加、`App_Code`フォルダー ([フルサイズのイメージを表示するをクリックして](creating-a-business-logic-layer-cs/_static/image11.png))


イベント ハンドラーを次に、作成、`ColumnChanging`確実にするイベント、 `UnitPrice`、 `UnitsInStock`、 `UnitsOnOrder`、および`ReorderLevel`列の値 (ない場合`NULL`) より大きいか 0 に等しい。 このような任意の列が範囲外にある場合は、スロー、`ArgumentException`です。

ProductsDataTable.ColumnChanging.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample5.cs)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>手順 4: BLL のクラスへのカスタム ビジネス ルールの追加

フィールド レベルの検証だけでなくなどさまざまなエンティティまたは 1 つの列レベルでない表現の概念を含むレベルのカスタム ビジネス ルールがあります。

- 場合は、製品が廃止されましたが、その`UnitPrice`を更新できません
- お住まいの国の従業員がお住まいの国の上司のと同じにする必要があります。
- 供給業者によって提供される唯一の製品である場合、製品を取り消すことはできません。

BLL クラスには、アプリケーションのビジネス ルールに準拠していることを確認するためのチェックを含める必要があります。 これらのチェックは、適用するメソッドに直接追加することができます。

当社のビジネス ルールは、ある製品マークできませんでした提供が中止された場合は、指定された業者からの唯一の製品がディクテーション想像してください。 つまり場合、製品*X*が唯一の製品を供給業者から購入したお*Y*、おに設定できませんでした*X*ように提供が中止された; 場合、ただし、業者*Y*3 つの製品に付属しているご*A*、 *B*、および*C*、任意に設定でしたおよびとしてこれらすべての提供が中止されました。 奇数のビジネス ルールが、ビジネス ルールと一般的な意味が合っていない常に!

このビジネス ルールを適用する、`UpdateProducts`メソッドを確認する場合は、まず`Discontinued`に設定された`true`と、ため、コールはお`GetProductsBySupplierID`この製品の供給業者から購入おを製品の数を決定します。 スローこの業者から 1 つの製品を購入すると、専用の場合、`ApplicationException`です。


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample6.cs)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>プレゼンテーション層の検証エラーへの応答

プレゼンテーション層から、BLL を呼び出すときに発生する可能性がありますまたは ASP.NET バブリングを知らせる例外を処理しようとするかどうかを判断できます (これが発生、`HttpApplication`の`Error`イベント)。 BLL をプログラムで使用する場合は、例外を処理するには、使用、 [try… catch](https://msdn.microsoft.com/library/0yd65esw.aspx)ブロックは、次の例を示します。


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample7.cs)]

後でチュートリアルをお見せデータを使用する場合、BLL からバブルアップ例外の処理 Web コントロールの挿入、更新、または内のコードをラップするのではなく、イベント ハンドラー内で直接処理するデータを削除することができます、`try...catch`ブロックします。

## <a name="summary"></a>まとめ

うまく設計されたアプリケーションを作成して、特定のロールをカプセル化の種類のレイヤーにします。 この記事シリーズの最初のチュートリアルで型指定されたデータセット; を使用してデータ アクセス レイヤーを作成しましたこのチュートリアルおが組み込まれているビジネス ロジック層、一連のクラスとして、アプリケーションの`App_Code`DAL 呼び出せるフォルダーです。 BLL では、アプリケーションのフィールド レベルおよびビジネス レベルのロジックを実装します。 加え個別 BLL を作成するには、このチュートリアルで行ったように別のオプションは部分クラスを使用して、Tableadapter のメソッドを拡張します。 ただし、この手法を使用することはできませんを既存のメソッドをオーバーライドするもはこれとを分離、DAL、BLL この記事の内容を使用しましたアプローチと明確にします。

DAL BLL 完了と、プレゼンテーション層で開始する準備ができました。 [次のチュートリアル](master-pages-and-site-navigation-cs.md)データ アクセスに関するトピックから簡単な迂回を取得し、チュートリアルでは、全体で使用する一貫性のあるページ レイアウトを定義します。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Liz Shulok、Dennis Patterson、Carlos Santos および Hilton Giesenow でした。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](creating-a-data-access-layer-cs.md)
[次へ](master-pages-and-site-navigation-cs.md)
