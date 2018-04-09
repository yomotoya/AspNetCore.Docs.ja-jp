---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
title: (VB) を結合に使用する TableAdapter の更新 |Microsoft ドキュメント
author: rick-anderson
description: データベースを使用する場合は、複数のテーブルに分散される要求のデータに共通します。 2 つの異なるテーブルからデータを取得するにはいずれかを使用しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: e624a3e0-061b-4efc-8b0e-5877f9ff6714
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
msc.type: authoredcontent
ms.openlocfilehash: 91d700f3de02dc78692e933644e221e2ac8175a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="updating-the-tableadapter-to-use-joins-vb"></a>(VB) を結合に使用する TableAdapter の更新
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_VB.zip)または[PDF のダウンロード](updating-the-tableadapter-to-use-joins-vb/_static/datatutorial69vb1.pdf)

> データベースを使用する場合は、複数のテーブルに分散される要求のデータに共通します。 2 つの異なるテーブルからデータを取得するには、ことができます、相関サブクエリや結合操作のいずれかを使用します。 このチュートリアルでは相関サブクエリと比較 JOIN 構文メイン クエリで結合が含まれる、TableAdapter を作成する方法を探す前にします。


## <a name="introduction"></a>はじめに

リレーショナル データベースでの操作では、データは複数のテーブルに分散多くの場合。 たとえば、製品情報を表示するときにおする可能性が高く各製品 s の対応するカテゴリと仕入先 %s の名前を一覧表示します。 `Products`テーブルに`CategoryID`と`SupplierID`値が実際のカテゴリと供給業者の名前では、`Categories`と`Suppliers`テーブルをそれぞれします。

使用するか、別の関連テーブルから情報を取得すること*相関サブクエリ*または`JOIN` *s*です。 相関サブクエリは、入れ子になった`SELECT`外側のクエリで列を参照するクエリ。 たとえば、[データ アクセス レイヤーを作成する](../introduction/creating-a-data-access-layer-vb.md)で 2 つの相関サブクエリを使用したチュートリアル、`ProductsTableAdapter`メイン クエリの各製品カテゴリと供給業者の名前を取得します。 A `JOIN` 2 つの異なるテーブルから関連する行をマージする SQL 構造体です。 使用して、`JOIN`で、 [SqlDataSource コントロールでのデータのクエリを実行する](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb.md)と共に各製品カテゴリ情報を表示するチュートリアルです。

使用してから abstained が理由`JOIN`対応の自動生成する TableAdapter のウィザードでの制限により、Tableadapter とは、 `INSERT`、 `UPDATE`、および`DELETE`ステートメントです。 具体的には、TableAdapter のメイン クエリでは、いずれかが含まれる場合`JOIN`s、TableAdapter できない自動的に作成、アドホック SQL ステートメントまたはストアド プロシージャをその`InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティです。

このチュートリアルを簡単に比較とコントラストの相関サブクエリと`JOIN`を含む TableAdapter を作成する方法を調べる前に s`JOIN`メイン クエリで s。

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>比較対照相関サブクエリと`JOIN`s

注意してください、`ProductsTableAdapter`最初のチュートリアルで作成した、`Northwind`データセットでは、相関サブクエリを使用して、各製品の対応するカテゴリおよび仕入先名を元に戻します。 `ProductsTableAdapter` S メイン クエリを次に示します。


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample1.sql)]

2 つの相関サブクエリ -`(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)`と`(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)`-は`SELECT`、外側の追加の列と製品ごとに 1 つの値を返すクエリを`SELECT`ステートメントの列の一覧です。

または、`JOIN`各製品の業者とカテゴリ名を返すために使用できます。 次のクエリは上記の 1 つと同じ出力が返されますが、使用`JOIN`サブクエリの代わりに s:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample2.sql)]

A`JOIN`いくつかの条件に基づく別のテーブルからレコードを含む 1 つのテーブルからレコードをマージします。 たとえば、上記のクエリで、`LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID`をマージする各 SQL Server に指示カテゴリの製品レコードがレコードを`CategoryID`値に一致する製品の`CategoryID`値。 マージされた結果には、各製品カテゴリの対応するフィールドを使用することができます (など`CategoryName`)。

> [!NOTE]
> `JOIN` s はリレーショナル データベースからデータを照会する際によく使用されます。 初めて使用する場合、`JOIN`構文や使用法に関するもう少しを復習する必要があります、d をお勧め、 [SQL の Join チュートリアル](http://www.w3schools.com/sql/sql_join.asp)で[W3 学校](http://www.w3schools.com/)です。 また読み取り価値は、 [ `JOIN`基礎](https://msdn.microsoft.com/library/ms191517.aspx)と[サブクエリの基礎](https://msdn.microsoft.com/library/ms189575.aspx)のセクションでは、 [SQL オンライン ブック](https://msdn.microsoft.com/library/ms130214.aspx)です。


`JOIN` S と相関サブクエリを両方使用できるその他のテーブルから関連するデータを取得する、そのヘッドをスクラッチおよび使用する方法を知り、多くの開発者は残されます。 すべて SQL エキスパートの I はほぼ同じことにしたことされない問題では性能とほぼ同じ実行プランを SQL Server が生成されます。 アドバイスは、次はおよびチームが最も慣れている手法を使用します。 このアドバイスを深めます後にこれらのエキスパートすぐに express の設定に注意してください。 その最もメリットのある`JOIN`相関サブクエリ経由で s。

型指定されたデータセットを使用してデータ アクセス レイヤーを作成するときに、ツール優れてサブクエリを使用しています。 具体的には、TableAdapter のウィザードがない自動生成対応`INSERT`、 `UPDATE`、および`DELETE`ステートメント、メインのクエリでは、いずれかが含まれる場合`JOIN`s が自動生成相関ときに、これらのステートメントが、サブクエリ使用されます。

この欠点を調査するに一時的な型指定されたデータセットを作成、`~/App_Code/DAL`フォルダーです。 TableAdapter 構成ウィザードの中に、アドホック SQL ステートメントを使用して、次を入力を選択`SELECT`クエリ (図 1 を参照してください)。


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample3.sql)]


[![結合を含むメイン クエリを入力してください。](updating-the-tableadapter-to-use-joins-vb/_static/image2.png)](updating-the-tableadapter-to-use-joins-vb/_static/image1.png)

**図 1**: を含むメイン クエリを入力して`JOIN`s ([フルサイズのイメージを表示するをクリックして](updating-the-tableadapter-to-use-joins-vb/_static/image3.png))


既定では、TableAdapter が自動的に作成`INSERT`、 `UPDATE`、および`DELETE`ステートメントは、メインのクエリに基づいています。 [詳細設定] をクリックした場合、この機能が有効になっていることが確認できます。 この設定に関係なく、TableAdapter ことはできませんを作成する、 `INSERT`、 `UPDATE`、および`DELETE`ステートメント、メインのクエリが含まれているため、`JOIN`です。


![結合を含むメイン クエリを入力してください。](updating-the-tableadapter-to-use-joins-vb/_static/image4.png)

**図 2**: を含むメイン クエリを入力して`JOIN`s


ウィザードを完了するには、[完了] をクリックします。 この時点で、データセットの Designer は、列を含む DataTable を 1 つの TableAdapter で返されるフィールドの各、`SELECT`クエリの列リストです。 これが含まれています、`CategoryName`と`SupplierName`図 3 に示すようにします。


![DataTable には列の一覧で返される各フィールドの列が含まれています](updating-the-tableadapter-to-use-joins-vb/_static/image5.png)

**図 3**: DataTable に列の一覧で返される各フィールドの列が含まれます


TableAdapter がの値がない DataTable には、適切な列が、その`InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティです。 これには、デザイナーの TableAdapter にクリックし、[プロパティ] ウィンドウに移動します。 ある \outputfromtestproviderdebugmode.txt、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティ (なし) に設定されます。


[![InsertCommand、UpdateCommand、および DeleteCommand プロパティを (なし) に設定されます。](updating-the-tableadapter-to-use-joins-vb/_static/image7.png)](updating-the-tableadapter-to-use-joins-vb/_static/image6.png)

**図 4**: `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティ (なし) に設定されます ([フルサイズのイメージを表示するをクリックして](updating-the-tableadapter-to-use-joins-vb/_static/image8.png))


この欠点を回避するには、手動で提供できます SQL ステートメントとパラメーターを`InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティ ウィンドウを使用してプロパティです。 またはに TableAdapter のメイン クエリを構成することによって開始でした*いない*を含める`JOIN`s。 これにより、 `INSERT`、 `UPDATE`、および`DELETE`うえで自動的に生成されるステートメントです。 ウィザードの完了後に、手動で更新すること、tableadapter`SelectCommand`そのが組み込まれているため、プロパティ ウィンドウから、`JOIN`構文です。

このアプローチは、中には不安定に非常がの場合、TableAdapter のメイン クエリをいつために、アドホック SQL クエリを使用して、自動生成される、ウィザードを使用して構成し直す`INSERT`、 `UPDATE`、および`DELETE`ステートメントが再作成します。 つまり、すべての後で行ったカスタマイズが失われる TableAdapter を右クリックして、コンテキスト メニューから構成を選択すると、ウィザードをもう一度完了します。

自動生成された tableadapter の脆弱性`INSERT`、 `UPDATE`、および`DELETE`ステートメントは幸いにも、アドホック SQL ステートメントに制限されます。 カスタマイズすることができます、TableAdapter は、ストアド プロシージャを使用している場合、 `SelectCommand`、 `InsertCommand`、 `UpdateCommand`、または`DeleteCommand`ストアド プロシージャおよびストアド プロシージャになることを心配しなくても、TableAdapter 構成ウィザードを再実行変更します。

作成、TableAdapter を最初に、いくつかの手順を今後には、いずれかの指定を省略するメインのクエリを使用して`JOIN`s できるように、対応する挿入、更新、および削除のストアド プロシージャは、自動生成されます。 マイクロソフトは、更新、`SelectCommand`使用するため、`JOIN`関連テーブルから追加の列を返します。 最後に、対応するビジネス ロジック層クラスを作成し、ASP.NET web ページで、TableAdapter を使用する方法を示しますおします。

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>手順 1: 簡素化されたメイン クエリを使用して、TableAdapter を作成します。

このチュートリアルでは、追加、TableAdapter の DataTable の厳密に型指定された、`Employees`テーブルに、`NorthwindWithSprocs`データセット。 `Employees`テーブルが含まれています、`ReportsTo`指定されているフィールド、`EmployeeID`従業員のマネージャーのです。 Anne 複数のグラフ エリアが従業員など、`ReportTo`は 5 の値、 `EmployeeID` Steven Buchanan のです。 その結果、Anne は Steven、上司に報告します。 各従業員の s をレポートと一緒に`ReportsTo`値、おすることも、マネージャーの名前を取得します。 これを使用して、`JOIN`です。 使用して、`JOIN`ときから対応する挿入を自動的に生成するウィザードが除外最初に、TableAdapter を作成、更新、および機能を削除します。 したがっては、まずそのメインのクエリは、TableAdapter を作成することで`JOIN`s。 次に、手順 2. では更新からマネージャーの名前を取得するメインのクエリがストアド プロシージャ、`JOIN`です。

開いて開始、`NorthwindWithSprocs`内のデータセット、`~/App_Code/DAL`フォルダーです。 デザイナーを右クリックし、コンテキスト メニューから、[追加] オプションを選択および TableAdapter のメニュー項目を選択します。 これにより、TableAdapter 構成ウィザードが起動します。 図 5 では、ある新しいストアド プロシージャを作成し、[次へ] をクリックしてウィザードになります。 リフレッシャーで、新規に作成するストアド プロシージャ、TableAdapter のウィザードを参照してください、[を作成する新しいストアド プロシージャを型指定されたデータセットの Tableadapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)チュートリアルです。


[![新しいストアド プロシージャの作成オプションを選択します。](updating-the-tableadapter-to-use-joins-vb/_static/image10.png)](updating-the-tableadapter-to-use-joins-vb/_static/image9.png)

**図 5**: 新しいストアド プロシージャ オプションの選択、作成 ([フルサイズのイメージを表示するをクリックして](updating-the-tableadapter-to-use-joins-vb/_static/image11.png))


以下を使用して`SELECT`TableAdapter のメイン クエリのステートメント。


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample4.sql)]

このクエリは含まれませんので`JOIN`s、TableAdapter ウィザードが自動的に作成ストアド プロシージャに対応する`INSERT`、 `UPDATE`、および`DELETE`メインを実行するためのストアド プロシージャと同様に、ステートメントクエリ。

次の手順では、TableAdapter s ストアド プロシージャの名前を付けることができます。 名前を使用して`Employees_Select`、 `Employees_Insert`、 `Employees_Update`、および`Employees_Delete`図 6 に示すように、します。


[![TableAdapter s ストアド プロシージャの名前](updating-the-tableadapter-to-use-joins-vb/_static/image13.png)](updating-the-tableadapter-to-use-joins-vb/_static/image12.png)

**図 6**: TableAdapter s ストアド プロシージャの名前を付けます ([フルサイズのイメージを表示するをクリックして](updating-the-tableadapter-to-use-joins-vb/_static/image14.png))


最後の手順には、TableAdapter のメソッドの名前を付けることが求められます。 使用して`Fill`と`GetEmployees`メソッド名とします。 直接、データベース (GenerateDBDirectMethods) チェック ボックスをオンになって更新を送信するメソッドを作成するままにしておくことを確認できます。


[![TableAdapter のメソッドの塗りつぶしの名前とため](updating-the-tableadapter-to-use-joins-vb/_static/image16.png)](updating-the-tableadapter-to-use-joins-vb/_static/image15.png)

**図 7**: tableadapter のメソッドの名前を付けます`Fill`と`GetEmployees`([フルサイズのイメージを表示するをクリックして](updating-the-tableadapter-to-use-joins-vb/_static/image17.png))


ウィザードを完了すると、すぐをデータベース内のストアド プロシージャを確認します。 次の 4 つの新しいファイルが表示されます: `Employees_Select`、 `Employees_Insert`、 `Employees_Update`、および`Employees_Delete`です。 次に、検査、`EmployeesDataTable`と`EmployeesTableAdapter`だけを作成します。 DataTable には、メインのクエリによって返される各フィールドの列が含まれています。 TableAdapter でクリックし、[プロパティ] ウィンドウに移動します。 ある \outputfromtestproviderdebugmode.txt、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`対応するストアド プロシージャを呼び出すのプロパティが正しく構成されています。


[![TableAdapter の Insert、Update が含まれています、機能の削除](updating-the-tableadapter-to-use-joins-vb/_static/image19.png)](updating-the-tableadapter-to-use-joins-vb/_static/image18.png)

**図 8**: TableAdapter を含む Insert、Update、および機能の削除 ([フルサイズのイメージを表示するをクリックして](updating-the-tableadapter-to-use-joins-vb/_static/image20.png))


挿入、更新、および delete ストアド プロシージャが自動的に作成され、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`をカスタマイズする準備が整いましたプロパティが正しく構成されている、 `SelectCommand` s ストアド プロシージャから返される追加各従業員のマネージャーについて説明します。 具体的には、更新する必要があります、`Employees_Select`ストアド プロシージャを使用する、 `JOIN` manager s を返すと`FirstName`と`LastName`値。 ストアド プロシージャが更新された後はこれら追加の列が含まれるように、データ テーブルを更新する必要があります。 これら 2 つのタスクの手順 2 および 3 取り上げるです。

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>手順 2: のカスタマイズ、ストアド プロシージャを含めるには`JOIN`

開始しようとして、サーバー エクスプ ローラー、Northwind データベース %s の Stored Procedures フォルダーへのドリル ダウンを開くと、`Employees_Select`ストアド プロシージャです。 このストアド プロシージャが表示されない場合は、ストアド プロシージャ フォルダーを右クリックし、更新 を選択します。 使用するように、ストアド プロシージャを更新、 `LEFT JOIN` manager s を最初に返されますの姓と名にします。


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample5.sql)]

更新した後、`SELECT`ステートメントでは、ファイル メニューの 保存 を選択して変更を保存`Employees_Select`です。 または、ツールバーの [保存] アイコンをクリックしてしたり、Ctrl キーを押しながら S キーを押してできます。 右クリックし、変更内容を保存した後、`Employees_Select`サーバー エクスプ ローラーでストアド プロシージャと実行 を選択します。 これは、ストアド プロシージャを実行され、出力ウィンドウにその結果を表示 (図 9 を参照してください)。


[![ストアド プロシージャの結果が、出力ウィンドウに表示されます。](updating-the-tableadapter-to-use-joins-vb/_static/image22.png)](updating-the-tableadapter-to-use-joins-vb/_static/image21.png)

**図 9**: は、出力ウィンドウに、ストアド プロシージャの結果が表示されます ([フルサイズのイメージを表示するをクリックして](updating-the-tableadapter-to-use-joins-vb/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>手順 3: データ テーブルの列を更新します。

この時点で、 `Employees_Select` 、ストアド プロシージャ`ManagerFirstName`と`ManagerLastName`値、ですが、`EmployeesDataTable`これらの列がありません。 これらの不足している列は、2 つの方法のいずれかでデータ テーブルに追加できます。

- **手動で**- データセット デザイナーでデータ テーブルを右クリックし、[追加] メニューから列を選択します。 列の名前し、それに応じてそのプロパティを設定できます。
- **自動的に**-TableAdapter 構成ウィザードは、DataTable s の列を更新によって返されるフィールドを反映するように、`SelectCommand`ストアド プロシージャです。 ウィザードが削除されますも、アドホック SQL ステートメントを使用する場合、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティ、`SelectCommand`が含まれています、`JOIN`です。 これらのコマンド プロパティはそのまま保持ストアド プロシージャを使用する場合。

など、前のチュートリアルで手動で追加のデータ テーブルの列について説明してきました[マスター/詳細レコードを使用して、箇条書きリストのマスター詳細 DataList で](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)と[ファイルのアップロード](../working-with-binary-files/uploading-files-vb.md)では、[次へ]、チュートリアルでは、もう一度の詳細については、このプロセスを見てください。 このチュートリアルでは、ただし、TableAdapter 構成ウィザードを使用して自動アプローチを使用して s を使用できます。

右クリックして開始、`EmployeesTableAdapter`し、コンテキスト メニューから構成を選択します。 選択すると、挿入、更新、および削除、およびその戻り値とパラメーター (ある場合) を使用するストアド プロシージャを一覧表示すると、TableAdapter 構成ウィザードが表示されます。 図 10 では、このウィザードを示します。 ここでことが分かります、 `Employees_Select` 、ストアド プロシージャ now、`ManagerFirstName`と`ManagerLastName`フィールドです。


[![ストアド プロシージャをウィザードに示す、Employees_Select の更新された列の一覧](updating-the-tableadapter-to-use-joins-vb/_static/image25.png)](updating-the-tableadapter-to-use-joins-vb/_static/image24.png)

**図 10**: ウィザードの更新された列の一覧を示しています、`Employees_Select`ストアド プロシージャ ([フルサイズのイメージを表示するをクリックして](updating-the-tableadapter-to-use-joins-vb/_static/image26.png))


[完了] をクリックして、ウィザードを完了します。 データセット デザイナーに戻ったときに、 `EmployeesDataTable` 2 つの列が含まれています:`ManagerFirstName`と`ManagerLastName`です。


[![EmployeesDataTable には、2 つの新しい列が含まれています。](updating-the-tableadapter-to-use-joins-vb/_static/image28.png)](updating-the-tableadapter-to-use-joins-vb/_static/image27.png)

**図 11**: `EmployeesDataTable` 2 つの新しい列が含まれています ([フルサイズのイメージを表示するをクリックして](updating-the-tableadapter-to-use-joins-vb/_static/image29.png))


説明するために、更新された`Employees_Select`ストアド プロシージャが有効になり、挿入が更新、および、TableAdapter の機能の削除が機能して、web ページを表示および従業員を削除することができますを作成しています。 このようなページを作成する前に、必要があります最初の従業員を操作するためのビジネス ロジック層で、新しいクラスを作成する、`NorthwindWithSprocs`データセット。 手順 4 では作成、`EmployeesBLLWithSprocs`クラスです。 手順 5 では、ASP.NET ページからこのクラスを使用します。

## <a name="step-4-implementing-the-business-logic-layer"></a>手順 4: 層のビジネス ロジックを実装します。

クラスの新しいファイルを作成、`~/App_Code/BLL`という名前のフォルダー`EmployeesBLLWithSprocs.vb`です。 このクラスは、既存のセマンティクスを模倣`EmployeesBLL`のみ、この新しい 1 つ少ないメソッドを提供され、クラスを使用して、`NorthwindWithSprocs`データセット (の代わりに、`Northwind`データセット)。 `EmployeesBLLWithSprocs` クラスに次のコードを追加します。


[!code-vb[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample6.vb)]

`EmployeesBLLWithSprocs`クラス s`Adapter`プロパティがのインスタンスを返します、`NorthwindWithSprocs`データセットの`EmployeesTableAdapter`です。 これは、クラス s によって使用`GetEmployees`と`DeleteEmployee`メソッドです。 `GetEmployees`メソッドの呼び出し、 `EmployeesTableAdapter` s 対応`GetEmployees`によって実行されるメソッド、`Employees_Select`ストアド プロシージャでは、その結果を取り込んで、`EmployeeDataTable`です。 `DeleteEmployee`メソッドを呼び出す同様に、 `EmployeesTableAdapter` s`Delete`によって実行されるメソッド、`Employees_Delete`ストアド プロシージャです。

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>手順 5: プレゼンテーション層で、データの使用

`EmployeesBLLWithSprocs`お ASP.NET ページから従業員データを処理する準備が完了すると、クラスです。 開く、 `JOINs.aspx`  ページで、`AdvancedDAL`フォルダーと、デザイナーの設定には、ツールボックスからドラッグ、GridView、`ID`プロパティを`Employees`です。 次に、GridView s のスマート タグからグリッドにバインドという名前の新しい ObjectDataSource コントロール`EmployeesDataSource`です。

構成を使用する ObjectDataSource、`EmployeesBLLWithSprocs`クラス、タブの中を選択し、削除、ことを確認して、`GetEmployees`と`DeleteEmployee`メソッドは、ドロップダウン リストから選択します。 ObjectDataSource の構成を完了するには、[完了] をクリックします。


[![構成、ObjectDataSource EmployeesBLLWithSprocs クラスを使用するには](updating-the-tableadapter-to-use-joins-vb/_static/image31.png)](updating-the-tableadapter-to-use-joins-vb/_static/image30.png)

**図 12**: 構成を使用する ObjectDataSource、`EmployeesBLLWithSprocs`クラス ([フルサイズのイメージを表示するをクリックして](updating-the-tableadapter-to-use-joins-vb/_static/image32.png))


[![ObjectDataSource を使用するためとするメソッドがあります。](updating-the-tableadapter-to-use-joins-vb/_static/image34.png)](updating-the-tableadapter-to-use-joins-vb/_static/image33.png)

**図 13**: ObjectDataSource に使用されている、`GetEmployees`と`DeleteEmployee`メソッド ([フルサイズのイメージを表示するをクリックして](updating-the-tableadapter-to-use-joins-vb/_static/image35.png))


Visual Studio は、それぞれに対して GridView に BoundField を追加するが、`EmployeesDataTable`の列です。 これら BoundFields 以外をすべて削除`Title`、 `LastName`、 `FirstName`、 `ManagerFirstName`、および`ManagerLastName`の名前を変更し、`HeaderText`姓、名、マネージャーの名、最後の 4 つ BoundFields のプロパティとマネージャーの姓、名、それぞれします。

従業員をこのページから削除するユーザーを許可するのには、2 つの作業を行う必要があります。 最初に、そのスマート タグからの削除を有効にするオプションをオンにして削除機能を提供する GridView に指示します。 次に、ObjectDataSource s を変更`OldValuesParameterFormatString`プロパティ値から、ObjectDataSource ウィザードによって設定 (`original_{0}`) をその既定値 (`{0}`)。 これらの変更を加えたら、GridView と ObjectDataSource s 宣言型マークアップを次のようになります。


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample7.aspx)]

ブラウザーからアクセスして、ページをテストします。 図 14 に示すように、各従業員と (1 つがあると仮定) 自分のマネージャーの名前に、ページが表示されます。


[![Employees_Select 内の結合、ストアド プロシージャ、マネージャー名](updating-the-tableadapter-to-use-joins-vb/_static/image37.png)](updating-the-tableadapter-to-use-joins-vb/_static/image36.png)

**図 14**:`JOIN`で、`Employees_Select`ストアド プロシージャは、マネージャー名を返します ([フルサイズのイメージを表示するをクリックして](updating-the-tableadapter-to-use-joins-vb/_static/image38.png))


実行が完了、削除のワークフローの開始 [削除] ボタンをクリックすると、`Employees_Delete`ストアド プロシージャです。 ただし、試み`DELETE`は外部キー制約違反のため、ストアド プロシージャ内のステートメントは失敗し (図 15 を参照してください)。 具体的には、各従業員が、1 つまたは複数のレコード、`Orders`テーブル、原因で、削除に失敗します。


[![外部キー制約の違反に対応する結果の順序を持つ従業員を削除します。](updating-the-tableadapter-to-use-joins-vb/_static/image40.png)](updating-the-tableadapter-to-use-joins-vb/_static/image39.png)

**図 15**: を外部キー制約の違反に対応する結果の順序を持つ従業員を削除する ([フルサイズのイメージを表示するをクリックして](updating-the-tableadapter-to-use-joins-vb/_static/image41.png))


削除する従業員を許可する可能性があります。

- 更新プログラムを削除、カスケード外部キー制約
- レコードを手動で削除、 `Orders` 、削除する名のテーブルまたは
- 更新プログラム、`Employees_Delete`ストアド プロシージャを最初から関連するレコードを削除する、`Orders`テーブルを削除する前に、`Employees`レコード。 この手法を説明した、[を使用して既存のストアド プロシージャを型指定されたデータセットの Tableadapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)チュートリアルです。

私のままに練習として、リーダーの。

## <a name="summary"></a>まとめ

リレーショナル データベースを使用する場合は、関連テーブルにクエリから複数のデータをプルするが一般的です。 相関サブクエリと`JOIN`s がクエリ内の関連テーブルからデータにアクセスするために 2 つの異なる方法を提供します。 TableAdapter は自動生成するため、相関サブクエリの最も一般的に行われた前のチュートリアルを使用して`INSERT`、 `UPDATE`、および`DELETE`関連するクエリに対するステートメント`JOIN`s。 アドホック SQL ステートメントを使用する場合に、これらの値を手動で指定することができます、TableAdapter 構成ウィザードが完了したときに、カスタマイズは上書きされます。

幸いにも、ストアド プロシージャを使用して作成された Tableadapter では、アドホック SQL ステートメントを使用して作成されたものと同じ脆弱性からことはありません。 したがってがメイン クエリを使用して TableAdapter を作成する実行可能では、`JOIN`ストアド プロシージャを使用する場合。 このチュートリアルでは、このような TableAdapter を作成する方法を説明しました。 使用して起動しました、 `JOIN`-小さい`SELECT`対応する挿入、更新、および削除のストアド プロシージャの自動作成ができるように、TableAdapter のメイン クエリでクエリを実行します。 補強、TableAdapter の初期構成が完了した、`SelectCommand`ストアド プロシージャを使用する、`JOIN`を更新する TableAdapter 構成ウィザードを再実行し、`EmployeesDataTable`の列です。

TableAdapter 構成ウィザードが自動的に更新を再実行、`EmployeesDataTable`列によって返されるデータ フィールドを反映するように、`Employees_Select`ストアド プロシージャです。 代わりに、おでしたがこれらの列に手動で追加、DataTable です。 について学びます列を手動で追加するには、DataTable に次のチュートリアルでします。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者には、Hilton Geisenow、David Suru Teresa マーフィーがされていました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [次へ](adding-additional-datatable-columns-vb.md)
