---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: 型指定された DataSet の TableAdapters (c#) のストアド プロシージャを既存の使用 |Microsoft Docs
author: rick-anderson
description: 前のチュートリアルでは、TableAdapter ウィザードを使用して、新しいストアド プロシージャを生成する方法について説明しました。 このチュートリアルで説明する方法と同じ TableAdapter.
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 440bef2a-1641-4238-99e3-8e2d44e7d94c
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: df095a7eeac0910078cfa206ed1ba7be9a1334d2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834762"
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>型指定された DataSet の TableAdapters (c#) のストアド プロシージャを既存の使用
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_CS.zip)または[PDF のダウンロード](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial68cs1.pdf)

> 前のチュートリアルでは、TableAdapter ウィザードを使用して、新しいストアド プロシージャを生成する方法について説明しました。 このチュートリアルではは、既存のストアド プロシージャと同じ TableAdapter ウィザードを機能させる方法について説明します。 データベースに新しいストアド プロシージャを手動で追加する方法も説明します。


## <a name="introduction"></a>はじめに

[前のチュートリアル](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)でした、型指定されたデータセットの Tableadapter をアクセス、アドホックではなく、データの SQL ステートメントをストアド プロシージャを使用して構成する方法を説明しました。 具体的には、TableAdapter ウィザードで自動的にこれらのストアド プロシージャを作成する方法を確認します。 ASP.NET 2.0 にレガシ アプリケーションを移植するとき、または既存のデータ モデルを ASP.NET 2.0 web サイトを構築するときに、データベースに既に必要があるストアド プロシージャが含まれる可能性があります。 または、手動でまたは何らかのツール、ストアド プロシージャの自動生成する TableAdapter ウィザード以外、ストアド プロシージャを作成することもできます。

このチュートリアルでは、既存のストアド プロシージャを使用して TableAdapter を構成する方法になります。 のみ、Northwind データベースには、少数の組み込みストアド プロシージャがあるので、Visual Studio 環境を使用して、データベースに新しいストアド プロシージャを手動で追加に必要なステップに注目するはもします。 Let s を始めましょう。

> [!NOTE]
> [トランザクション内のデータベース変更のラッピング](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md)チュートリアルがトランザクションをサポートする TableAdapter にメソッドを追加しました (`BeginTransaction`、`CommitTransaction`など)。 または、トランザクションは、データ アクセス層のコードに変更を必要としないストアド プロシージャ内に完全に管理できます。 このチュートリアルでは、トランザクションのスコープ内でストアド プロシージャのステートメントを実行するために使用する T-SQL コマンドについて説明します。


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>手順 1: Northwind データベースにストアド プロシージャを追加します。

Visual Studio では、簡単に新しいストアド プロシージャをデータベースに追加できます。 Let s のすべての列を返す、Northwind データベースに新しいストアド プロシージャを追加する、`Products`を持つ特定のテーブル`CategoryID`値。 サーバー エクスプ ローラー ウィンドウからのデータベース ダイアグラム、テーブル、ビュー、およびなど、そのフォルダーが表示されるように、Northwind データベースを展開します。 前のチュートリアルで説明したように、ストアド プロシージャ フォルダーには、データベースの既存のストアド プロシージャが含まれています。 新しいストアド プロシージャを追加するには、Stored Procedures フォルダーを右クリックし、コンテキスト メニューから新しいストアド プロシージャの追加オプションを選択します。


[![ストアド プロシージャのフォルダーを右クリックし、新しいストアド プロシージャの追加](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**図 1**: ストアド プロシージャ フォルダーを右クリックし、新しいストアド プロシージャを追加 ([フルサイズの画像を表示する をクリックします](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png))。


図 1 に示すようは、ストアド プロシージャを作成するために必要な SQL スクリプトの輪郭をスクリプト ウィンドウを Visual Studio に開きます新しいストアド プロシージャの追加オプションを選択します。 私たちの仕事をしながらこのスクリプトを実行するか、この時点で、ストアド プロシージャをデータベースに追加されます。

次のスクリプトを入力します。


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

実行すると、このスクリプトはという Northwind データベースに新しいストアド プロシージャを追加`Products_SelectByCategoryID`します。 このストアド プロシージャは、1 つの入力パラメーターを受け取り (`@CategoryID`、型の`int`) であり、すべてのフィールドを対応する製品について返します`CategoryID`値。

これを実行する`CREATE PROCEDURE`スクリプトしストアド プロシージャをデータベースに追加、ツールバーの [保存] アイコンをクリックします。 または Ctrl + S をヒットします。 これを行った後、ストアド プロシージャのフォルダーの更新、新しく作成されたを示すと、プロシージャが格納されます。 ウィンドウでスクリプトがから注意を変更することも、`CREATE PROCEDURE dbo.Products_SelectProductByCategoryID`に`ALTER PROCEDURE``dbo.Products_SelectProductByCategoryID`します。 `CREATE PROCEDURE` 新しいストアド プロシージャをデータベースに追加中に`ALTER PROCEDURE`既存のものを更新します。 スクリプトの先頭が変更されたため`ALTER PROCEDURE`パラメーターまたは SQL ステートメントを入力するストアド プロシージャを変更する、および、これらの変更をストアド プロシージャを更新、保存アイコンをクリックします。

図 2 は後の Visual Studio、`Products_SelectByCategoryID`ストアド プロシージャが保存されました。


[![ストアド プロシージャ Products_SelectByCategoryID がデータベースに追加されました](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png)

**図 2**: ストアド プロシージャの`Products_SelectByCategoryID`がデータベースに追加されました ([フルサイズの画像を表示する をクリックします](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png))。


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>手順 2: 既存のストアド プロシージャを使用する TableAdapter の構成

これで、 `Products_SelectByCategoryID` concigure にそのメソッドのいずれかが呼び出されるときに、このストアド プロシージャを使用するデータ アクセス層をできるはデータベースにストアド プロシージャに追加されました。 具体的には、追加、`GetProducstByCategoryID(categoryID)`メソッドを`ProductsTableAdapter`で、`NorthwindWithSprocs`型指定されたデータセットを呼び出す、`Products_SelectByCategoryID`ストアド プロシージャを作成しました。

開いて開始、`NorthwindWithSprocs`データセット。 右クリックし、 `ProductsTableAdapter` TableAdapter クエリ構成ウィザードを起動するには、クエリの追加を選択します。 [前のチュートリアル](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)ある TableAdapter の私たちにとって、新しいストアド プロシージャを作成することにしました。 このチュートリアルでは、ただし、する新しい TableAdapter メソッドを既存ワイヤ`Products_SelectByCategoryID`ストアド プロシージャ。 そのため、ウィザードの最初の手順から既存のストアド プロシージャを使用する オプションを選択し、し、次へ をクリックします。


[![既存のストアド プロシージャ オプションの使用を選択します。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)

**図 3**: ストアド プロシージャ オプションを使用して既存の選択 ([フルサイズの画像を表示する をクリックします](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png))。


次の画面では、ドロップダウン リストでは、ストアド プロシージャをデータベース %s でに設定を提供します。 ストアド プロシージャを選択するには、左側と右側 (あれば) が返されるデータ フィールドに入力パラメーターが一覧表示します。 選択、`Products_SelectByCategoryID`ストアド プロシージャの一覧から、[次へ] をクリックします。


[![選択、Products_SelectByCategoryID ストアド プロシージャ](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)

**図 4**: 選択、`Products_SelectByCategoryID`ストアド プロシージャ ([フルサイズの画像を表示する をクリックします](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png))。


次の画面で、求められたときデータの種類は、ストアド プロシージャによって返され、回答は、TableAdapter のメソッドによって返される型を決定します。 たとえば、表形式のデータが返されることを示すことがある場合、メソッドは、`ProductsDataTable`インスタンス ストアド プロシージャによって返されるレコードを格納します。 これに対し、このストアド プロシージャが 1 つの値を返すことを指定する場合、TableAdapter を返します、`object`ストアド プロシージャによって返される最初のレコードの最初の列の値を割り当てることができます。

以降、`Products_SelectByCategoryID`ストアド プロシージャは、特定のカテゴリに属している、- 表形式のデータの最初の回答を選択し、[次へ] をクリックします。 すべての製品を返します。


[![ストアド プロシージャが表形式のデータを返すことを示します](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)

**図 5**: ストアド プロシージャが表形式のデータを返すことを示します ([フルサイズの画像を表示する をクリックします](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png))。


残っているはこれらのメソッド名を使用して後に使用するメソッドのパターンを示すです。 DataTable オプションがオンになっているが、メソッドの名前を変更 DataTable と戻り値のままに、両方の塗りつぶし`FillByCategoryID`と`GetProductsByCategoryID`します。 ウィザードが実行するタスクの概要を確認するのには、[次へ] をクリックします。 すべてが正しい場合、[完了] をクリックします。


[![名前のメソッド FillByCategoryID と GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**図 6**: メソッドの名前を付けます`FillByCategoryID`と`GetProductsByCategoryID`([フルサイズの画像を表示する をクリックします](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))。


> [!NOTE]
> TableAdapter のメソッドを先ほど作成した`FillByCategoryID`と`GetProductsByCategoryID`、型の入力パラメーターを想定`int`します。 この入力パラメーターの値が使用してストアド プロシージャに渡されるその`@CategoryID`パラメーター。 変更する場合、`Products_SelectByCategory`ストアド プロシージャのパラメーター、またこれら TableAdapter のメソッドのパラメーターを更新する必要があります。 説明したように、[前のチュートリアル](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)、2 つの方法のいずれかにできます: 手動で追加または削除によってパラメーターまたはパラメーターのコレクションから、TableAdapter ウィザードを再実行しています。


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>手順 3: 追加、`GetProductsByCategoryID(categoryID)`BLL にメソッド

`GetProductsByCategoryID` DAL メソッドの完全なビジネス ロジック層では、このメソッドへのアクセスを提供すること、次の手順です。 開く、`ProductsBLLWithSprocs`クラス ファイルと、次のメソッドを追加します。


[!code-csharp[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.cs)]

この BLL メソッドだけを返します、`ProductsDataTable`から返される、 `ProductsTableAdapter` s`GetProductsByCategoryID`メソッド。 `DataObjectMethodAttribute`属性は、ObjectDataSource のデータ ソースの構成ウィザードによって使用されるメタデータを提供します。 具体的には、このメソッドは、タブ s のドロップダウン リストに表示されます。

## <a name="step-4-displaying-products-by-category"></a>手順 4: カテゴリによって製品を表示します。

新しく追加されたテストに`Products_SelectByCategoryID`ストアド プロシージャと対応する DAL、BLL メソッド、s、DropDownList と GridView を含む ASP.NET ページを作成できるようにします。 GridView は選択したカテゴリに属する製品を表示中に、DropDownList はすべてのデータベース内のカテゴリで表示されます。

> [!NOTE]
> マスター/詳細インターフェイスの作成 Dropdownlist を前のチュートリアルを使用します。 このようなマスター/詳細レポートの実装に関する詳細についてを参照してください、[マスター/詳細のフィルター処理で、DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)チュートリアル。


開く、`ExistingSprocs.aspx`ページで、`AdvancedDAL`フォルダーと、ツールボックスからデザイナーにドラッグ DropDownList します。 DropDownList s 設定`ID`プロパティを`Categories`とその`AutoPostBack`プロパティを`true`します。 次に、スマート タグ、という名前の新しい ObjectDataSource に DropDownList をバインド`CategoriesDataSource`します。 データを取得するために、ObjectDataSource を構成、`CategoriesBLL`クラスの`GetCategories`メソッド。 UPDATE、INSERT でドロップダウン リストを設定し、(なし) タブを削除します。


[![CategoriesBLL クラスのメソッドからデータを取得します。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**図 7**: からのデータの取得、`CategoriesBLL`クラス s`GetCategories`メソッド ([フルサイズの画像を表示する をクリックします](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png))。


[![UPDATE、INSERT でドロップダウン リストを設定し、(なし) タブを削除します。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png)

**図 8**: (None) に、UPDATE、INSERT、および削除のタブで、ドロップダウン リストを設定 ([フルサイズの画像を表示する をクリックします](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png))。


ObjectDataSource ウィザードを完了すると、構成を表示する DropDownList、`CategoryName`データ フィールドを使用して、`CategoryID`フィールドとして、`Value`各`ListItem`します。

この時点では、DropDownList、ObjectDataSource s の宣言型マークアップは、次のようなする必要があります。


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.aspx)]

次に、GridView を DropDownList の下に配置することをデザイナーにドラッグします。 GridView s 設定`ID`に`ProductsByCategory`し、スマート タグ、という名前の新しい ObjectDataSource にバインドする`ProductsByCategoryDataSource`します。 構成、 `ProductsByCategoryDataSource` ObjectDataSource を使用する、`ProductsBLLWithSprocs`してクラスを使用してそのデータを取得する、`GetProductsByCategoryID(categoryID)`メソッド。 データを表示するこの GridView のみ使用するため、UPDATE、INSERT でのドロップダウン リストの設定し (None) にタブを削除して [次へ] をクリックします。


[![ProductsBLLWithSprocs クラスを使用する ObjectDataSource を構成します。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png)

**図 9**: 構成に使用する ObjectDataSource、`ProductsBLLWithSprocs`クラス ([フルサイズの画像を表示する をクリックします](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png))。


[![GetProductsByCategoryID(categoryID) メソッドからのデータを取得します。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)

**図 10**: からのデータの取得、`GetProductsByCategoryID(categoryID)`メソッド ([フルサイズの画像を表示する をクリックします](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png))。


タブで選択した方法には、ウィザードの最後の手順では、私たちを求めるパラメーター s のソースのため、パラメーターが期待しています。 パラメーターのソースのドロップダウン リストをコントロールに設定し、選択、 `Categories` ControlID のドロップダウン リストからのコントロール。 ウィザードを完了するには、[完了] をクリックします。


[![カテゴリの DropDownList の categoryID パラメーターのソースとして使用します。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**図 11**: 使用して、 `Categories` DropDownList のソースとして、`categoryID`パラメーター ([フルサイズの画像を表示する をクリックします](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))。


ObjectDataSource ウィザードを完了するとは、Visual Studio は各製品のデータ フィールドのも、BoundFields と、CheckBoxField に追加されます。 自由に、必要に応じて、これらのフィールドをカスタマイズできます。

ブラウザーを使用してページを参照してください。 ときに、飲料カテゴリが選択されているページと対応する製品にアクセスでは、グリッドに表示されます。 図 12 として、別のカテゴリをドロップダウン リストの変更とを示しています、ポストバックが発生し、新しく選択したカテゴリの製品とグリッドを再度読み込みます。


[![生成カテゴリの製品が表示されます。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**図 12**: 生成カテゴリ、製品が表示されます ([フルサイズの画像を表示する をクリックします](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png))。


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>手順 5: トランザクションのスコープ内で、ストアド プロシージャのステートメントをラップします。

[トランザクション内のデータベース変更のラッピング](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md)チュートリアルの一連のトランザクションのスコープ内でのデータベース変更ステートメントを実行するための手法について説明しました。 すべて成功するか、トランザクションの傘下に実行される変更のことを思い出してくださいまたはすべての失敗、原子性を保証します。 トランザクションを使用する方法は次のとおりです。

- クラスを使用して、`System.Transactions`名前空間
- などの ADO.NET クラスを使用して、データ アクセス層を持つ`SqlTransaction`と
- ストアド プロシージャ内で直接 T-SQL トランザクション コマンドを追加します。

*トランザクション内のデータベース変更のラッピング*チュートリアルは、DAL で ADO.NET クラスを使用します。 このチュートリアルの残りの部分では、ストアド プロシージャ内からの T-SQL コマンドを使用してトランザクションを管理する方法について説明します。

手動で開始、コミット、およびトランザクションのロールバックの 3 つのキー SQL コマンドは`BEGIN TRANSACTION`、 `COMMIT TRANSACTION`、および`ROLLBACK TRANSACTION`、それぞれします。 などの ADO.NET アプローチでは、次のパターンを適用する必要があります。 ストアド プロシージャ内からトランザクションを使用する場合。

1. トランザクションの開始を示します。
2. トランザクションを構成する SQL ステートメントを実行します。
3. 手順 2 では、トランザクションをロールバック ステートメントのいずれかでエラーがある場合
4. エラーなしですべての手順 2 からステートメント完了、トランザクションをコミットします。

次のテンプレートを使用して T-SQL 構文では、このパターンを実装できます。


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]

テンプレートを定義することで開始、`TRY...CATCH`コンス トラクターを初めて使用する SQL Server 2005 をブロックします。 使用するような`try...catch`c# で SQL ブロック`TRY...CATCH`ブロック内のステートメントの実行、`TRY`ブロックします。 制御が移りますすぐに任意のステートメントでは、エラーが発生した場合、`CATCH`ブロックします。

その構成は、トランザクションでは、SQL ステートメントを実行するエラーがない場合、`COMMIT TRANSACTION`ステートメントが、変更をコミットし、トランザクションを完了します。 、エラーが発生、ステートメントのいずれかの場合は、、`ROLLBACK TRANSACTION`で、`CATCH`ブロックがトランザクションの開始前の状態にデータベースを返します。 ストアド プロシージャもを使用して、エラーを発生させる、 [RAISERROR コマンド](https://msdn.microsoft.com/library/ms178592.aspx)、原因となる、`SqlException`が、アプリケーションで発生します。

> [!NOTE]
> 以降、`TRY...CATCH`ブロックは新しい SQL Server 2005 には、Microsoft SQL Server の以前のバージョンを使用している場合、上記のテンプレートは機能しません。 SQL Server 2005 を使用していない場合を参照してください[SQL Server ストアド プロシージャでのトランザクションの管理](http://www.4guysfromrolla.com/webtech/080305-1.shtml)の他のバージョンの SQL Server で動作するテンプレート。


具体的な例を見て s を使用できます。 間に外部キー制約が存在する、`Categories`と`Products`テーブル、つまり各`CategoryID`フィールドに、`Products`をテーブルにマップする必要があります、`CategoryID`値、`Categories`テーブル。 外部キー制約に違反など、製品に関連付けられたカテゴリを削除しようとしています。 この制約に違反するすべての操作の結果します。 これを確認するには、バイナリ データのセクションを使用した作業の例では更新および削除する既存のバイナリ データを再検討 (`~/BinaryData/UpdatingAndDeleting.aspx`)。 このページは、編集、削除ボタン (図 13 を参照してください)、と共にシステムに各カテゴリを一覧表示されますが、外部キー制約違反のため、削除が失敗した飲み物のなどの製品が関連付けられているカテゴリを削除しようとした場合 (図 14 を参照してください)。


[![各カテゴリが編集と削除ボタンの GridView に表示されます。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png)

**図 13**: 編集と削除ボタンの GridView に各カテゴリが表示されます ([フルサイズの画像を表示する をクリックします](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png))。


[![持つ既存の製品カテゴリを削除することはできません。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png)

**図 14**: を持つ既存の製品カテゴリを削除することはできません ([フルサイズの画像を表示する をクリックします](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png))。


ただし、製品に関連付けられているかどうかに関係なく削除するカテゴリをできるようにすることに想像してください。 製品のカテゴリを削除するか、また、既存の製品を削除することを想像してください (別のオプションは単にその製品を設定することが`CategoryID`値を`NULL`)。 この機能は、外部キー制約の連鎖ルールによって実装できます。 受け取るストアド プロシージャを作成または、`@CategoryID`入力パラメーターと、呼び出されると、削除に明示的にすべての関連する製品とし、指定したカテゴリ。

このようなストアド プロシージャで最初の試みは、次のようになります。


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

これは、関連付けられている製品と分類に間違いなく削除されます、ことはないため、トランザクションの傘下にします。 その他の外部キー制約があることを想像してみてください`Categories`、特定の削除を禁止`@CategoryID`値。 問題は、このような場合にすべての製品が削除されること、カテゴリを削除する前に。 最終的な結果は、このようなカテゴリには、このストアド プロシージャは削除することの製品すべてさせながらも、カテゴリ別のテーブル内のレコードは関連がそれ以降のままです。

ストアド プロシージャがラップされて場合、トランザクションのスコープ内で、削除を`Products`テーブルはロールバックされます失敗した削除が発生した場合`Categories`します。 次のストアド プロシージャのスクリプトは、2 つの間の原子性を確保するためにトランザクションを使用して`DELETE`ステートメント。


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.sql)]

追加する少し、`Categories_Delete`ストアド プロシージャを Northwind データベース。 ストアド プロシージャをデータベースに追加する手順について、手順 1 を戻す参照します。

## <a name="step-6-updating-thecategoriestableadapter"></a>手順 6: 更新する、`CategoriesTableAdapter`

追加中に、 `Categories_Delete` DAL、データベースにストアド プロシージャは、アドホック SQL ステートメントを使用して、削除を実行する現在構成されています。 更新する必要があります、`CategoriesTableAdapter`を使用するよう指示して、`Categories_Delete`ストアド プロシージャの代わりにします。

> [!NOTE]
> このチュートリアルで先ほど作業した、`NorthwindWithSprocs`データセット。 そのデータセットは 1 つのエンティティのみがある`ProductsDataTable`カテゴリを使用する必要があります。 そのため、データ アクセス レイヤー I m を参照するについて言及している場合は、このチュートリアルの残りの部分の`Northwind`データセットで最初に作成した 1 つ、[データ アクセス層を作成する](../introduction/creating-a-data-access-layer-cs.md)チュートリアル。


開いている Northwind データセットの選択、 `CategoriesTableAdapter`、し、[プロパティ] ウィンドウに移動します。 プロパティ ウィンドウが表示されます、 `InsertCommand`、 `UpdateCommand`、 `DeleteCommand`、および`SelectCommand`TableAdapter と、その名前と接続情報を使用します。 展開、`DeleteCommand`プロパティをその詳細を参照してください。 図 15 に示すよう、 `DeleteCommand` s`ComamndType`プロパティのテキストとして送信するように指示しますのテキストに設定されて、`CommandText`アドホック SQL クエリとしてプロパティ。


![[プロパティ] ウィンドウでプロパティを表示するデザイナーで、CategoriesTableAdapter を選択します](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png)

**図 15**: 選択、`CategoriesTableAdapter`プロパティ ウィンドウでプロパティを表示するデザイナー


これらの設定を変更するには、(DeleteCommand) テキスト プロパティ ウィンドウを選択し、ドロップダウン リストから (新規) を選択します。 設定をクリアします、 `CommandText`、 `CommandType`、および`Parameters`プロパティ。 次に、設定、`CommandType`プロパティを`StoredProcedure`、ストアド プロシージャの名前を入力し、 `CommandText` (`dbo.Categories_Delete`)。 必ずこの順序で最初のプロパティを入力する場合、`CommandType`をクリックし、 `CommandText` -Visual Studio がパラメーター コレクションを自動的に設定します。 この順序でこれらのプロパティを入力しない場合はパラメーター コレクション エディターを使ってパラメーターを手動で追加する必要があります。 どちらの場合、s (図 16 を参照してください)、適切なパラメーターの設定の変更が加えられましたことを確認するパラメーター コレクション エディターをするためのパラメーター プロパティで省略記号をクリックすることをお勧めします。 すべてのパラメーター ダイアログ ボックスが表示されない場合は、追加、`@CategoryID`パラメーター手動で (を追加する必要はありません、`@RETURN_VALUE`パラメーター)。


![パラメーターの設定が正しいことを確認します。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**図 16**: パラメーターの設定が正しいことを確認します。


DAL が更新されたら、カテゴリを削除は自動的に関連付けられた製品をすべて削除し、トランザクションの傘下に行います。 これを確認するには、更新および削除する既存のバイナリ データのページに戻り、カテゴリの 1 つの Delete ボタンをクリックします。 マウスの 1 つシングル クリックで、カテゴリとの関連付けられている製品すべては削除されます。

> [!NOTE]
> テストする前に、`Categories_Delete`ストアド プロシージャには、選択したカテゴリと製品数を削除することが考えられます、データベースのバックアップ コピーを作成することをお勧めします。 使用する場合、`NORTHWND.MDF`データベース`App_Data`、単に Visual Studio を終了し、MDF および LDF ファイルをコピー`App_Data`他のフォルダーにします。 機能をテストした後は、Visual Studio を閉じることで、データベースを戻すことができ、置き換える現在 MDF および LDF ファイル`App_Data`バックアップ コピーを使用します。


## <a name="summary"></a>まとめ

TableAdapter のウィザードは、私たちのストアド プロシージャを自動的に生成は、中にはが既にこのようなストアド プロシージャを作成したりに手動でまたはその他のツールを代わりに作成します。 このようなシナリオに合わせてに、既存のストアド プロシージャを指す、TableAdapter を構成することもできます。 このチュートリアルでは、Visual Studio 環境を使用してデータベースにストアド プロシージャを手動で追加する方法とこれらのストアド プロシージャを TableAdapter のメソッドを接続する方法について説明しました。 T-SQL コマンドとスクリプトのパターンの開始、コミット、およびストアド プロシージャ内からトランザクションをロールバックするためも調べる。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Hilton Geisenow、S ren Jacob Lauritsen、および Teresa Murphy でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [次へ](updating-the-tableadapter-to-use-joins-cs.md)
