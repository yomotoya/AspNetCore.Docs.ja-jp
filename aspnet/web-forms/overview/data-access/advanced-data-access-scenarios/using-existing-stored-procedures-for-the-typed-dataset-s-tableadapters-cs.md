---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: 型指定されたデータセットの Tableadapter (c#) のストアド プロシージャを既存の使用 |Microsoft ドキュメント
author: rick-anderson
description: 前のチュートリアルでは、TableAdapter ウィザードを使用して、新しいストアド プロシージャを生成する方法を学習します。 このチュートリアルで学習はどのように同じ TableAdapter しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 440bef2a-1641-4238-99e3-8e2d44e7d94c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: df8a714325ce99db615eddc3d457da5c926919ba
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>型指定されたデータセットの Tableadapter (c#) のストアド プロシージャを既存の使用
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_CS.zip)または[PDF のダウンロード](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial68cs1.pdf)

> 前のチュートリアルでは、TableAdapter ウィザードを使用して、新しいストアド プロシージャを生成する方法を学習します。 このチュートリアルでは、同じ TableAdapter ウィザードが既存のストアド プロシージャを機能させる方法おについて説明します。 私たちは、データベースに新しいストアド プロシージャを手動で追加する方法も学習します。


## <a name="introduction"></a>はじめに

[前のチュートリアル](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)アクセス データではなく、アドホック SQL ステートメントをストアド プロシージャを使用して型指定されたデータセットの Tableadapter の構成方法を説明しました。 具体的には、TableAdapter ウィザードで自動的にこれらのストアド プロシージャを作成する方法を確認します。 ASP.NET 2.0 にレガシ アプリケーションを移植するとき、または既存のデータ モデルの周囲で ASP.NET 2.0 web サイトを構築するときに、データベースに既に必要なストアド プロシージャが含まれる可能性があります。 また、手動で、または、ストアド プロシージャを自動生成する TableAdapter ウィザード以外のいくつかのツールをストアド プロシージャを作成することができます。

このチュートリアルでは、既存のストアド プロシージャを使用して、TableAdapter を構成する方法について取り上げます。 Northwind データベースでは、組み込みのストアド プロシージャの小さなセットのみがある後も見ていきます Visual Studio 環境によってデータベースに新しいストアド プロシージャを手動で追加するために必要な手順を実行します。 開始 s を let!

> [!NOTE]
> [トランザクション内でデータベースの変更をラッピング](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md)トランザクションをサポートするために、TableAdapter にメソッドを追加したチュートリアル (`BeginTransaction`、`CommitTransaction`など)。 代わりに、トランザクションは、データ アクセス層のコードに変更が必要としない、ストアド プロシージャ内に完全に管理できます。 このチュートリアルではトランザクションのスコープ内でストアド プロシージャのステートメントを実行するために使用する T-SQL コマンドをについて説明します。


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>手順 1: Northwind データベースへのストアド プロシージャの追加

Visual Studio では簡単に新しいストアド プロシージャをデータベースに追加します。 Let s は、Northwind データベースからすべての列を表すオブジェクトを新しいストアド プロシージャを追加、`Products`を持つ特定のテーブル`CategoryID`値。 サーバー エクスプ ローラー ウィンドウからのデータベース ダイアグラム、テーブル、ビュー、およびなど、そのフォルダーが表示されるように、Northwind データベースを展開します。 前のチュートリアルで説明したとおり、Stored Procedures フォルダーには、データベースの既存のストアド プロシージャが含まれています。 新しいストアド プロシージャを追加するには、ストアド プロシージャ フォルダーを右クリックし、コンテキスト メニューから新しいストアド プロシージャの追加オプションを選択します。


[![ストアド プロシージャのフォルダーを右クリックし、新しいストアド プロシージャの追加](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**図 1**: ストアド プロシージャ フォルダーを右クリックし、新しいストアド プロシージャの追加 ([フルサイズのイメージを表示するをクリックして](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png))


図 1 に示す新しいストアド プロシージャの追加オプションを選択するウィンドウを開き、スクリプト Visual Studio でストアド プロシージャを作成するために必要な SQL スクリプトのアウトラインします。 このジョブをこのスクリプトを具体化し、実行するか、この時点でストアド プロシージャをデータベースに追加 ことをお勧めします。

次のスクリプトを入力します。


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

実行すると、このスクリプトはという Northwind データベースに新しいストアド プロシージャを追加`Products_SelectByCategoryID`です。 このストアド プロシージャは 1 つの入力パラメーターを受け取ります (`@CategoryID`、型の`int`) を返しますのすべての一致する製品のフィールドと`CategoryID`値。

これを実行する`CREATE PROCEDURE`スクリプトおよびストアド プロシージャをデータベースに追加、ツールバーの [保存] アイコンをクリックしてまたは Ctrl キーを押しながら S キーをヒットします。 これを行った後、ストアド プロシージャのフォルダーの更新、新しく作成されたを示すと、プロシージャが格納されます。 また、ウィンドウ内のスクリプトがから注意を変更`CREATE PROCEDURE dbo.Products_SelectProductByCategoryID`に`ALTER PROCEDURE``dbo.Products_SelectProductByCategoryID`です。 `CREATE PROCEDURE` 新しいストアド プロシージャをデータベースに追加中に`ALTER PROCEDURE`既存のものを更新します。 スクリプトの先頭に変更されたため`ALTER PROCEDURE`ストアド プロシージャを変更する入力パラメーターまたは SQL ステートメント、および保存 アイコンをクリックするとすると、ストアド プロシージャがこれらの変更が更新されます。

図 2 は、Visual Studio の後に、`Products_SelectByCategoryID`ストアド プロシージャが保存されました。


[![ストアド プロシージャ Products_SelectByCategoryID がデータベースに追加されました](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png)

**図 2**: ストアド プロシージャの`Products_SelectByCategoryID`がデータベースに追加されました ([フルサイズのイメージを表示するをクリックして](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>手順 2: 既存のストアド プロシージャを使用する TableAdapter を構成します。

これで、 `Products_SelectByCategoryID` concigure に独自のメソッドのいずれかが呼び出されるときに、このストアド プロシージャを使用するデータ アクセス層をできるには、データベースのストアド プロシージャに追加されました。 具体的には、追加、`GetProducstByCategoryID(categoryID)`メソッドを`ProductsTableAdapter`で、`NorthwindWithSprocs`型指定されたデータセットを呼び出す、`Products_SelectByCategoryID`ストアド プロシージャを作成したばかりです。

開いて開始、`NorthwindWithSprocs`データセット。 右クリックし、 `ProductsTableAdapter` TableAdapter クエリの構成ウィザードを起動するには、クエリの追加を選択します。 [前のチュートリアル](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)するうえで、新しいストアド プロシージャを作成する TableAdapter があることにしました。 このチュートリアルでは、ただし、したいネットワーク上で、既存の新しい TableAdapter メソッド`Products_SelectByCategoryID`ストアド プロシージャです。 そのため、ウィザードの最初の手順から既存のストアド プロシージャを使用する オプションを選択し、次へ をクリックします。


[![既存のストアド プロシージャ オプションの使用を選択します。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)

**図 3**: を使用して既存のストアド プロシージャ オプションを選択 ([フルサイズのイメージを表示するをクリックして](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png))


次の画面は、ドロップダウン リストに表示されます、データベースのストアド プロシージャを提供します。 ストアド プロシージャを選択するには、左側と右側 (存在する場合) は返されたデータ フィールドに入力パラメーターが一覧表示します。 選択、`Products_SelectByCategoryID`ストアド プロシージャの一覧から、[次へ] をクリックします。


[![選択、Products_SelectByCategoryID ストアド プロシージャ](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)

**図 4**: Pick、`Products_SelectByCategoryID`ストアド プロシージャ ([フルサイズのイメージを表示するをクリックして](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png))


次の画面をかけるとデータの種類は、ストアド プロシージャによって返されるし、答えが TableAdapter のメソッドによって返される型を判別します。 たとえば、表形式のデータが返されることを示すは場合、メソッドは、`ProductsDataTable`インスタンスが、ストアド プロシージャによって返されるレコードを格納します。 これに対し、このストアド プロシージャが単一の値を返すことを示している場合、TableAdapter が返されます、`object`ストアド プロシージャによって返される最初のレコードの最初の列の値を割り当てることができます。

以降、`Products_SelectByCategoryID`ストアド プロシージャが返すすべての製品を特定のカテゴリに属するを表形式データの最初の応答を選択して [次へ] をクリックします。


[![ストアド プロシージャが表形式のデータを返すことを示します](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)

**図 5**: ストアド プロシージャが表形式のデータを返すことを示します ([フルサイズのイメージを表示するをクリックして](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png))


これらのメソッド名を使用して後に使用するメソッドのパターンを示すためにします。 両方の塗りつぶし DataTable オプションがオンになっているが、メソッドの名前を変更 DataTable と戻り値のままにして`FillByCategoryID`と`GetProductsByCategoryID`です。 ウィザードが実行するタスクの概要を確認するには、次へ をクリックします。 すべてが正しい場合、[完了] をクリックします。


[![名前のメソッド FillByCategoryID と GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**図 6**: メソッドの名前を付けます`FillByCategoryID`と`GetProductsByCategoryID`([フルサイズのイメージを表示するをクリックして](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


> [!NOTE]
> TableAdapter のメソッドを先ほど作成した`FillByCategoryID`と`GetProductsByCategoryID`、型の入力パラメーターを想定して`int`です。 この入力パラメーターの値が使用してストアド プロシージャに渡されたその`@CategoryID`パラメーター。 変更する場合、`Products_SelectByCategory`ストアド プロシージャのパラメーター、またこれら TableAdapter のメソッドのパラメーターを更新する必要があります。 説明したように、[前のチュートリアル](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)、2 つの方法のいずれかでこれ行う: 手動で追加または削除してパラメーターをパラメーター コレクションから、または、TableAdapter ウィザードを再実行しています。


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>手順 3: 追加、`GetProductsByCategoryID(categoryID)`BLL メソッド

`GetProductsByCategoryID` DAL メソッドが完了したら、次に、ビジネス ロジック層でこのメソッドにアクセスできるようにします。 開く、`ProductsBLLWithSprocs`クラス ファイルと、次のメソッドを追加します。


[!code-csharp[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.cs)]

この BLL メソッドだけを返します、`ProductsDataTable`から返される、 `ProductsTableAdapter` s`GetProductsByCategoryID`メソッドです。 `DataObjectMethodAttribute`属性は、ObjectDataSource のデータ ソース構成ウィザードで使用されるメタデータを提供します。 具体的には、このメソッドは、タブの s ドロップダウン リストに表示されます。

## <a name="step-4-displaying-products-by-category"></a>手順 4: カテゴリによって製品を表示します。

新しく追加されたテスト`Products_SelectByCategoryID`ストアド プロシージャと、対応する DAL と BLL メソッド、s、DropDownList と GridView を含む ASP.NET ページを作成できるようにします。 DropDownList はすべて一覧表示、データベース内のカテゴリの中に、GridView、選択したカテゴリに属する製品が表示されます。

> [!NOTE]
> 作成したマスター/詳細インターフェイスお DropDownLists を前のチュートリアルで使用します。 このようなマスター/詳細レポートの実装をより詳しくを参照してください、[マスター/詳細のフィルター処理で、DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)チュートリアルです。


開く、 `ExistingSprocs.aspx`  ページで、`AdvancedDAL`フォルダーと、ツールボックスからデザイナーにドラッグ DropDownList です。 集合の DropDownList s`ID`プロパティを`Categories`とその`AutoPostBack`プロパティを`true`です。 次に、そのスマート タグからバインド DropDownList という名前の新しい ObjectDataSource`CategoriesDataSource`です。 データを取得するための構成、ObjectDataSource、`CategoriesBLL`クラスの`GetCategories`メソッドです。 UPDATE、INSERT でドロップダウン リストを設定し、(なし) タブを削除します。


[![CategoriesBLL クラスのメソッドからデータを取得します。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**図 7**: からのデータの取得、`CategoriesBLL`クラス s`GetCategories`メソッド ([フルサイズのイメージを表示するをクリックして](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png))


[![UPDATE、INSERT のドロップダウン リストを設定し、(なし) タブを削除します。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png)

**図 8**: (なし) に、UPDATE、INSERT、および削除のタブで、ドロップダウン リストを設定 ([フルサイズのイメージを表示するをクリックして](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png))


ObjectDataSource ウィザードを完了すると、構成を表示する DropDownList、`CategoryName`データ フィールドを使用して、`CategoryID`フィールド、`Value`各`ListItem`です。

この時点での DropDownList と ObjectDataSource s の宣言型マークアップは、次のようにする必要があります。


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.aspx)]

次に、デザイナーにドラッグ、GridView、DropDownList の下に配置します。 集合 GridView s`ID`に`ProductsByCategory`と、という名前の新しい ObjectDataSource へのバインドのスマート タグから`ProductsByCategoryDataSource`です。 構成、 `ProductsByCategoryDataSource` ObjectDataSource を使用する、`ProductsBLLWithSprocs`してクラスを使用してそのデータを取得する、`GetProductsByCategoryID(categoryID)`メソッドです。 この GridView のみが使用されますデータを表示するため、UPDATE、INSERT でのドロップダウン リストの設定 (なし) にタブを削除して 次へ をクリックします。


[![構成、ObjectDataSource ProductsBLLWithSprocs クラスを使用するには](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png)

**図 9**: 構成を使用する ObjectDataSource、`ProductsBLLWithSprocs`クラス ([フルサイズのイメージを表示するをクリックして](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png))


[![GetProductsByCategoryID(categoryID) メソッドからデータを取得します。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)

**図 10**: からのデータの取得、`GetProductsByCategoryID(categoryID)`メソッド ([フルサイズのイメージを表示するをクリックして](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png))


選択 タブで選択された方法ではため、ウィザードの最後の手順はごパラメーター s のソースの入力が求め、パラメーターが必要です。 パラメーターのソースのドロップダウン リストをコントロールに設定し、選択、 `Categories` ControlID ドロップダウン リストから管理します。 ウィザードを完了するには、[完了] をクリックします。


[![カテゴリの DropDownList の categoryID パラメーターのソースとして使用します。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**図 11**: を使用して、 `Categories` DropDownList のソースとして、`categoryID`パラメーター ([フルサイズのイメージを表示するをクリックして](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


ObjectDataSource ウィザードを完了すると、Visual Studio は追加 BoundFields、CheckBoxField の各製品のデータ フィールドのします。 自由に応じて表示と、これらのフィールドをカスタマイズしてください。

ブラウザーでページを参照してください。 飲み物のカテゴリが選択されているページと対応する製品へのアクセスは、グリッドに表示されます。 図 12 として、代替のカテゴリにドロップダウン リストを変更するとを示しています、ポストバックを発生させるし、新しく選択したカテゴリの製品を持つグリッドが再度読み込まれます。


[![生成カテゴリの製品が表示されます。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**図 12**: 生成カテゴリ、製品が表示されます ([フルサイズのイメージを表示するをクリックして](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>手順 5: トランザクションのスコープ内のストアド プロシージャのステートメントのラッピング

[トランザクション内でデータベースの変更をラッピング](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md)一連のトランザクションのスコープ内でのデータベース変更ステートメントを実行するための手法を説明したチュートリアルです。 すべて成功するか、トランザクションの包括的な実行の変更を取り消します。 または原子性を確保し、すべて失敗します。 トランザクションを使用するための手法は次のとおりです。

- クラスを使用して、`System.Transactions`名前空間
- 同様に、ADO.NET クラスを使用して、データ アクセス レイヤーを持つ`SqlTransaction`、および
- ストアド プロシージャ内で直接 T-SQL トランザクション コマンドを追加します。

*トランザクション内でデータベースの変更をラッピング*チュートリアルは、DAL で ADO.NET クラスを使用します。 このチュートリアルの残りの部分は、ストアド プロシージャ内から T-SQL コマンドを使用してトランザクションを管理する方法を説明します。

手動で開始、コミット、およびトランザクションのロールバックの 3 つのキー SQL コマンドは、 `BEGIN TRANSACTION`、 `COMMIT TRANSACTION`、および`ROLLBACK TRANSACTION`、それぞれします。 などの方法では、ADO.NET、次のパターンを適用する必要があります、ストアド プロシージャ内からトランザクションを使用する場合。

1. トランザクションの開始を示します。
2. トランザクションを構成する SQL ステートメントを実行します。
3. 手順 2、ロールバック、トランザクションのステートメントのいずれかでエラーがある場合
4. エラーなしですべての手順 2 からステートメント完了、トランザクションをコミットします。

次のテンプレートを使用する T-SQL 構文では、このパターンを実装できます。


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]

テンプレートを定義することで開始、`TRY...CATCH`を初めて使用する SQL Server 2005 のコンストラクトをブロックします。 使用するような`try...catch`ブロックを C# の場合、SQL`TRY...CATCH`ブロック内のステートメントを実行する、`TRY`ブロックします。 コントロールに転送してすぐに任意のステートメントでは、エラーが発生した場合、`CATCH`ブロックします。

その構成トランザクション、SQL ステートメントの実行エラーがない場合、`COMMIT TRANSACTION`ステートメントが、変更をコミットし、トランザクションを完了します。 、ただし、ステートメントのいずれかの結果になった場合、エラー、`ROLLBACK TRANSACTION`で、`CATCH`ブロックがトランザクションの開始前の状態にデータベースを返します。 ストアド プロシージャでを使用して、エラーを発生させます、 [RAISERROR コマンド](https://msdn.microsoft.com/library/ms178592.aspx)、これにより、`SqlException`がアプリケーションで発生します。

> [!NOTE]
> 以降、`TRY...CATCH`ブロックは初めて使用する SQL Server 2005、Microsoft SQL Server の以前のバージョンを使用している場合、このテンプレートは機能しません。 SQL Server 2005 を使用していない場合を参照してください[SQL Server ストアド プロシージャでのトランザクションの管理](http://www.4guysfromrolla.com/webtech/080305-1.shtml)for SQL Server の他のバージョンで動作するテンプレートです。


具体的な例を見て s を使用できます。 間で外部キー制約が存在する、`Categories`と`Products`テーブル、つまり各`CategoryID`フィールドで、`Products`をテーブルにマップする必要があります、`CategoryID`値で、`Categories`テーブル。 外部キー制約に違反など、製品に関連付けられているカテゴリを削除しようとしています。 この制約に違反するすべての操作の結果します。 これを確認するには、更新および既存のバイナリ データを削除する例では、「バイナリ データ操作を再表示 (`~/BinaryData/UpdatingAndDeleting.aspx`). このページは、編集および削除のボタンが (図 13 を参照してください)、と共にシステムに各カテゴリを一覧表示されますが、外部キー制約違反のため、削除に失敗 - 飲み物のなどの製品に関連付けられているカテゴリを削除しようとすると (図 14 を参照してください)。


[![編集と削除ボタンを含む GridView に各カテゴリが表示されます。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png)

**図 13**: 編集と削除ボタンを含む GridView に各カテゴリが表示されます ([フルサイズのイメージを表示するをクリックして](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png))


[![持つ既存の製品カテゴリを削除することはできません。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png)

**図 14**: を持つ既存の製品カテゴリを削除することはできません ([フルサイズのイメージを表示するをクリックして](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png))


ただし、製品に関連付けられているかどうかに関係なく削除するカテゴリを許可するかを想像してください。 製品のカテゴリを削除する必要も、既存の製品を削除することを想像してください (単にその製品を設定する別のオプションが`CategoryID`値を`NULL`)。 この機能は、外部キー制約の連鎖規則によって実装できます。 受け取るストアド プロシージャを作成代わりに、`@CategoryID`入力パラメーターと、呼び出されると、明示的にすべての関連する製品と削除し、指定されたカテゴリ。

このようなストアド プロシージャでの最初の計画は、次のようになります。


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

関連する製品と分類を削除確実中に、行われないトランザクションの傘下にします。 その他の外部キー制約があることを想像してください`Categories`、特定の削除を禁止`@CategoryID`値。 この問題は、このようなケースですべての製品が削除されること、カテゴリを削除する前にです。 最終的な結果は、このようなカテゴリには、このストアド プロシージャは削除することの製品すべてさせながらも、カテゴリ別のテーブル内のレコードは関連がそれ以降のままです。

場合は、ストアド プロシージャがラップされた、トランザクションのスコープ内でただし、削除を`Products`テーブルはロールバックされます失敗した削除が発生した場合`Categories`です。 次のストアド プロシージャのスクリプトを 2 つの間の原子性を保証するにはトランザクションを使用して`DELETE`ステートメント。


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.sql)]

追加する、`Categories_Delete`ストアド プロシージャを Northwind データベース。 参照手順 1. のストアド プロシージャをデータベースに追加する手順についてはします。

## <a name="step-6-updating-thecategoriestableadapter"></a>手順 6: 更新する、`CategoriesTableAdapter`

ここに追加しました、`Categories_Delete`をアドホック SQL ステートメントを使用して、削除を実行するには、DAL、データベースのストアド プロシージャが現在構成されています。 更新する必要があります、`CategoriesTableAdapter`を使用するように指定し、`Categories_Delete`ストアド プロシージャの代わりにします。

> [!NOTE]
> このチュートリアルで前に作業していた、`NorthwindWithSprocs`データセット。 データセットは 1 つのエンティティのみがある、その`ProductsDataTable`カテゴリを使用する必要があります。 そのため、データ アクセス レイヤー I m を参照する方法については、このチュートリアルの残りの部分、`Northwind`データセットを最初に作成した、[データ アクセス レイヤーを作成する](../introduction/creating-a-data-access-layer-cs.md)チュートリアルです。


開いている Northwind データセットの選択、 `CategoriesTableAdapter`、[プロパティ] ウィンドウに移動します。 プロパティ ウィンドウの一覧、 `InsertCommand`、 `UpdateCommand`、 `DeleteCommand`、および`SelectCommand`TableAdapter と、その名前と接続情報を使用します。 展開して、`DeleteCommand`プロパティをその詳細を参照してください。 図 15 に示す、 `DeleteCommand` s`ComamndType`プロパティのテキストとして送信するよう指示のテキストに設定されて、`CommandText`アドホック SQL クエリとしてプロパティです。


![[プロパティ] ウィンドウでプロパティを表示するデザイナーで、CategoriesTableAdapter を選択します。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png)

**図 15**: 選択、`CategoriesTableAdapter`デザイナーで、[プロパティ] ウィンドウでそのプロパティを表示


これらの設定を変更するには、プロパティ ウィンドウで (DeleteCommand) テキストを選択し、(新規) をドロップダウン リストから選択します。 設定をクリアします、 `CommandText`、 `CommandType`、および`Parameters`プロパティです。 次に、設定、`CommandType`プロパティを`StoredProcedure`、ストアド プロシージャの名前を入力し、 `CommandText` (`dbo.Categories_Delete`)。 プロパティを入力します - この順序で最初に確認する場合、`CommandType`し、 `CommandText` -Visual Studio はパラメーター コレクションを自動的に設定します。 この順序でこれらのプロパティを入力しないと場合、は、パラメーター コレクション エディターを使ってパラメーターを手動で追加する必要があります。 どちらの場合に、パラメーター コレクション エディター を表示 (図 16 を参照してください) 適切なパラメーター設定の変更が行われたことを確認するには、パラメーター プロパティで省略記号をクリックするが賢明です。 任意のパラメーター ダイアログ ボックスが表示されない場合は、追加、`@CategoryID`パラメーター手動で (を追加する必要はありません、`@RETURN_VALUE`パラメーター)。


![パラメーターの設定が適切であることを確認してください。](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**図 16**: パラメーター設定が正しいことを確認


DAL が更新されると、カテゴリを削除するは自動的に関連付けられた製品をすべて削除し、包括的なトランザクションの下にあるようにします。 これを確認するには、更新と既存のバイナリ データを削除するページに戻るし、カテゴリの 1 つの削除 をクリックします。 マウスの 1 つ 1 回のクリックで、カテゴリとの関連付けられている製品すべて削除されます。

> [!NOTE]
> テストする前に、`Categories_Delete`ストアド プロシージャは、選択したカテゴリと製品の数が削除されますがあります、データベースのバックアップ コピーを作成します。 使用している場合、`NORTHWND.MDF`データベース`App_Data`、単に Visual Studio を終了およびに MDF と LDF ファイルをコピー`App_Data`の他のフォルダーにします。 機能をテストした後は、Visual Studio を終了して、データベースを戻すことができ、置き換える現在 MDF と LDF ファイル`App_Data`バックアップのコピーにします。


## <a name="summary"></a>まとめ

TableAdapter の s ウィザードでは、ご利用の米国のストアド プロシージャを自動的に生成されますもおが既にこのようなストアド プロシージャを作成したりに手動でまたはその他のツールを使用して代わりに作成します。 このようなシナリオに合わせてに、既存のストアド プロシージャを指す、TableAdapter を構成することもできます。 このチュートリアルでは、これらのストアド プロシージャを TableAdapter のメソッドを接続するための方法と、Visual Studio 環境を使用してデータベースにストアド プロシージャを手動で追加する方法を説明しました。 おは、T-SQL コマンドおよびスクリプトのパターンの開始、コミット、およびストアド プロシージャ内からトランザクションをロールバックするためにも検討します。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者には、Hilton Geisenow、S ren 一 Lauritsen Teresa マーフィーがされていました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [次へ](updating-the-tableadapter-to-use-joins-cs.md)
