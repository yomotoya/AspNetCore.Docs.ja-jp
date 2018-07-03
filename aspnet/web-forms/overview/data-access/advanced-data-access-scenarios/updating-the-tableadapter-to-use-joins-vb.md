---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
title: 使用して TableAdapter を更新する (VB) を結合 |Microsoft Docs
author: rick-anderson
description: データベースを使用する場合は、複数のテーブルに分散される要求のデータに共通します。 2 つの異なるテーブルからデータを取得するには、いずれかを使用しましたできます.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: e624a3e0-061b-4efc-8b0e-5877f9ff6714
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
msc.type: authoredcontent
ms.openlocfilehash: 9987d4dab7de4fc19d36625fcebc9d63e21acbe8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377173"
---
<a name="updating-the-tableadapter-to-use-joins-vb"></a>(VB) を Join を使用して TableAdapter を更新
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_VB.zip)または[PDF のダウンロード](updating-the-tableadapter-to-use-joins-vb/_static/datatutorial69vb1.pdf)

> データベースを使用する場合は、複数のテーブルに分散される要求のデータに共通します。 2 つの異なるテーブルからデータを取得するには、相関サブクエリや結合操作のいずれかを使用しましたできます。 このチュートリアルでは相関サブクエリと比較結合構文をメイン クエリで結合を含む TableAdapter を作成する方法を見る前にします。


## <a name="introduction"></a>はじめに

リレーショナル データベースで、多くの場合、複数のテーブルにデータを操作するために注目していますが展開されます。 たとえば、製品情報を表示するときに可能性がありますする各製品の対応するカテゴリと仕入先の名を一覧表示します。 `Products`テーブルに`CategoryID`と`SupplierID`値が実際の category と supplier の名前は、`Categories`と`Suppliers`テーブルに、それぞれします。

使用するか、別の関連テーブルから情報を取得すること*相関サブクエリ*または`JOIN` *s*します。 相関サブクエリは、入れ子になった`SELECT`外側のクエリで列を参照するクエリ。 など、[データ アクセス層を作成する](../introduction/creating-a-data-access-layer-vb.md)チュートリアルの 2 つの相関サブクエリを使用して、 `ProductsTableAdapter` s のメイン クエリに各製品カテゴリと仕入先名が返されます。 A`JOIN`は 2 つの異なるテーブルから関連する行をマージする SQL コンストラクトです。 使用して、`JOIN`で、 [SqlDataSource コントロールでデータのクエリ](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb.md)と共に各製品カテゴリの情報を表示するチュートリアル。

使用してから abstained が理由`JOIN`を自動生成の対応する TableAdapter のウィザードでの制限により、Tableadapter とは、 `INSERT`、 `UPDATE`、および`DELETE`ステートメント。 具体的には、TableAdapter のメイン クエリでは、いずれかが含まれる場合`JOIN`s、TableAdapter できません自動作成、アドホック SQL ステートメントまたはストアド プロシージャをその`InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティ。

このチュートリアルを簡単に比較とコントラストの相関サブクエリと`JOIN`s を含む TableAdapter を作成する方法を調べる前に`JOIN`メイン クエリで s。

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>比較および対照相関サブクエリと`JOIN`s

いることを思い出してください、`ProductsTableAdapter`最初のチュートリアルで作成した、`Northwind`データセットでは、相関サブクエリを使用して、各製品の対応するカテゴリと仕入先名を元に戻します。 `ProductsTableAdapter` S メイン クエリを次に示します。


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample1.sql)]

2 つの相関サブクエリ -`(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)`と`(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)`-は`SELECT`を外部列を追加すると製品ごとに 1 つの値を返すクエリ`SELECT`ステートメントの列のリスト。

または、`JOIN`各製品の仕入先とカテゴリ名を取得するために使用できます。 次のクエリは、上記と同じ出力を返しますが、使用`JOIN`サブクエリの代わりに s:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample2.sql)]

A`JOIN`いくつかの条件に基づいて別のテーブルからレコードを 1 つのテーブルからレコードをマージします。 たとえば、上記のクエリで、 `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` SQL サーバーごとにマージするように指示、カテゴリの製品レコードをレコードを`CategoryID`値に一致する製品の`CategoryID`値。 マージされた結果では、製品ごとに対応するカテゴリ フィールドを操作できます (など`CategoryName`)。

> [!NOTE]
> `JOIN` リレーショナル データベースからデータを照会するときに s がよく使用されます。 初めて使用する場合、`JOIN`構文または使用法に関するを少し復習する必要がある、d をお勧めします、 [SQL Join チュートリアル](http://www.w3schools.com/sql/sql_join.asp)で[W3 学校](http://www.w3schools.com/)します。 またに読む価値は、 [ `JOIN`基礎](https://msdn.microsoft.com/library/ms191517.aspx)と[サブクエリの基礎](https://msdn.microsoft.com/library/ms189575.aspx)のセクションでは、[オンライン ブックの「](https://msdn.microsoft.com/library/ms130214.aspx)。


`JOIN` S と相関サブクエリ両方を使用できるその他のテーブルから関連データを取得する、多くの開発者の頭をかきむしりおよび使用する方法をご参考までにままになります。 すべて、SQL の第一人者であるはほぼ同じことを言ったに ve その it されない問題では効果は絶大ように SQL Server のほぼ同一の実行プランが生成されます。 そのアドバイスは、次は自分とチームが最も慣れている手法を使用します。 これを効率的にこと後、このアドバイスを作られています。 これらのエキスパートすぐに express の基本設定を注意してください`JOIN`相関サブクエリ経由で s。

型指定されたデータセットを使用して、データ アクセス層を構築するときに、ツールにも適してサブクエリを使用する場合。 具体的には、TableAdapter の s ウィザードがない自動生成対応`INSERT`、 `UPDATE`、および`DELETE`メイン クエリでは、いずれかが含まれる場合、ステートメント`JOIN`s が自動生成相関している場合、これらのステートメントが、サブクエリ使用されます。

この欠点を参照するで一時的な型指定されたデータセットを作成、`~/App_Code/DAL`フォルダー。 TableAdapter 構成ウィザードで、アドホック SQL ステートメントを使用して、次を入力します。 選択`SELECT`クエリ (図 1 参照)。


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample3.sql)]


[![結合を含むメイン クエリを入力します。](updating-the-tableadapter-to-use-joins-vb/_static/image2.png)](updating-the-tableadapter-to-use-joins-vb/_static/image1.png)

**図 1**: を含むメイン クエリを入力して`JOIN`s ([フルサイズの画像を表示する をクリックします](updating-the-tableadapter-to-use-joins-vb/_static/image3.png))。


既定では、TableAdapter の作成は自動的に`INSERT`、 `UPDATE`、および`DELETE`ステートメントは、メインのクエリに基づいています。 [詳細設定] をクリックした場合、この機能が有効になっていることが確認できます。 この設定に関係なく、TableAdapter はできませんを作成する、 `INSERT`、 `UPDATE`、および`DELETE`ステートメント、メイン クエリに含まれているため、`JOIN`します。


![結合を含むメイン クエリを入力します。](updating-the-tableadapter-to-use-joins-vb/_static/image4.png)

**図 2**: を含むメイン クエリを入力して`JOIN`s


ウィザードを完了するには、[完了] をクリックします。 この時点で、データセットにはデザイナーにが含まれます列を含む DataTable を 1 つの TableAdapter にはで返されるフィールドの各、`SELECT`クエリの列のリスト。 これが含まれています、`CategoryName`と`SupplierName`図 3 に示すように、します。


![DataTable には列の一覧で返される各フィールドの列が含まれています](updating-the-tableadapter-to-use-joins-vb/_static/image5.png)

**図 3**: DataTable に列の一覧に返される各フィールドの列が含まれます


TableAdapter の値が不足しています、DataTable には、適切な列が、その`InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティ。 これには、デザイナーで TableAdapter クリックし、[プロパティ] ウィンドウに移動します。 表示されますが、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティ (なし) に設定されます。


[![InsertCommand、UpdateCommand、および DeleteCommand プロパティは、(なし) に設定されます。](updating-the-tableadapter-to-use-joins-vb/_static/image7.png)](updating-the-tableadapter-to-use-joins-vb/_static/image6.png)

**図 4**: `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティ (なし) に設定されます ([フルサイズの画像を表示する をクリックします](updating-the-tableadapter-to-use-joins-vb/_static/image8.png))。


この欠点を回避することが手動で、SQL ステートメントと提供のパラメーター、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティ ウィンドウを使用してプロパティ。 または、する TableAdapter のメイン クエリを構成することによって開始でした*いない*を含める`JOIN`s。 これにより、 `INSERT`、 `UPDATE`、および`DELETE`ステートメントを私たちの自動生成されます。 ウィザードを完了すると、私たちでしたを手動で更新、tableadapter`SelectCommand`プロパティ ウィンドウでその it が含まれていますので、`JOIN`構文。

このアプローチは、中には非常に不安定がの場合、TableAdapter のメイン クエリにいつであるため、アドホック SQL クエリを使用して、ウィザードは、自動生成された構成し直す`INSERT`、 `UPDATE`、および`DELETE`ステートメントが再作成します。 つまり、すべてのカスタマイズを行った後で失われる TableAdapter を右クリックして、コンテキスト メニューから構成を選択するともう一度ウィザードを完了します。

自動生成された tableadapter の脆弱性`INSERT`、 `UPDATE`、および`DELETE`ステートメントは幸運にも、アドホック SQL ステートメントに制限されます。 カスタマイズすることができます、TableAdapter は、ストアド プロシージャを使用している場合、 `SelectCommand`、 `InsertCommand`、 `UpdateCommand`、または`DeleteCommand`ストアド プロシージャおよびストアド プロシージャになることを心配しなくても、TableAdapter 構成ウィザードを再実行変更します。

作成、TableAdapter を最初に、いくつかの手順を今後には、いずれかの指定を省略するメインのクエリを使用して`JOIN`s を対応する挿入、更新、および削除のストアド プロシージャは、自動生成されます。 マイクロソフトは、更新、`SelectCommand`使用するため、`JOIN`関連テーブルから追加の列を返します。 最後に、対応するビジネス ロジック層のクラスを作成し、ASP.NET web ページで、TableAdapter を使用する方法を示します。

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>手順 1: 簡略化されたメイン クエリを使用して、TableAdapter を作成します。

このチュートリアルでは TableAdapter と厳密に型指定された DataTable を追加しましたが、`Employees`テーブルに、`NorthwindWithSprocs`データセット。 `Employees`テーブルが含まれています、`ReportsTo`指定されているフィールド、`EmployeeID`の従業員のマネージャー。 Anne 複数のグラフ エリアが従業員など、 `ReportTo` 5 の値、 `EmployeeID` Steven Buchanan の。 その結果、Anne を Steven、上司に報告します。 各従業員の s をレポートと共に`ReportsTo`値、する可能性がもそれぞれのマネージャーの名前を取得します。 これを使用して、`JOIN`します。 使用していますが、`JOIN`ときに、ウィザードを自動的に対応する挿入が生成されないため最初に TableAdapter を作成、更新、および機能を削除します。 そのため、メインのクエリは、TableAdapter の作成から始めます`JOIN`秒。 次で手順 2 では更新を使用してマネージャーの名前を取得するメインのクエリがストアド プロシージャ、`JOIN`します。

開いて開始、`NorthwindWithSprocs`データセットで、`~/App_Code/DAL`フォルダー。 デザイナーを右クリックし、コンテキスト メニューから追加のオプションを選択および TableAdapter のメニュー項目を選択します。 これにより、TableAdapter 構成ウィザードが起動します。 図 5 を示していますとしてのウィザードで新しいストアド プロシージャを作成し、[次へ] をクリックします。 新規作成は、ストアド プロシージャを TableAdapter の s ウィザードからを参照してください、[型指定されたデータセット s Tableadapter の新しいのストアド プロシージャの作成](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)チュートリアル。


[![作成する新しいストアド プロシージャ オプションを選択します。](updating-the-tableadapter-to-use-joins-vb/_static/image10.png)](updating-the-tableadapter-to-use-joins-vb/_static/image9.png)

**図 5**: 新しいストアド プロシージャ オプションの選択の作成 ([フルサイズの画像を表示する をクリックします](updating-the-tableadapter-to-use-joins-vb/_static/image11.png))。


次を使用して、 `SELECT` TableAdapter のメイン クエリのステートメント。


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample4.sql)]

このクエリは含まれませんので`JOIN`s、TableAdapter ウィザードは自動的にストアド プロシージャの作成と、対応する`INSERT`、 `UPDATE`、および`DELETE`メインの実行にストアド プロシージャと同様に、ステートメントクエリ。

次の手順により、TableAdapter の格納されているプロシージャの名前を付けます。 名を使用して`Employees_Select`、 `Employees_Insert`、 `Employees_Update`、および`Employees_Delete`図 6 に示すようにします。


[![TableAdapter の格納されているプロシージャの名前](updating-the-tableadapter-to-use-joins-vb/_static/image13.png)](updating-the-tableadapter-to-use-joins-vb/_static/image12.png)

**図 6**: TableAdapter s ストアド プロシージャの名前 ([フルサイズの画像を表示する をクリックします](updating-the-tableadapter-to-use-joins-vb/_static/image14.png))。


最後の手順では、メソッド、TableAdapter の名前を求められます。 使用`Fill`と`GetEmployees`メソッド名として。 必ず、データベース (GenerateDBDirectMethods) チェック ボックスはオンに直接更新を送信するためのメソッドを作成するのままにしてください。


[![TableAdapter のメソッドの塗りつぶしの名前と GetEmployees](updating-the-tableadapter-to-use-joins-vb/_static/image16.png)](updating-the-tableadapter-to-use-joins-vb/_static/image15.png)

**図 7**: tableadapter のメソッドの名前を付けます`Fill`と`GetEmployees`([フルサイズの画像を表示する をクリックします](updating-the-tableadapter-to-use-joins-vb/_static/image17.png))。


ウィザードを完了すると、データベース内のストアド プロシージャを調べるには少しを実行します。 次の 4 つの新しい値が表示されます: `Employees_Select`、 `Employees_Insert`、 `Employees_Update`、および`Employees_Delete`します。 次に、検査、`EmployeesDataTable`と`EmployeesTableAdapter`だけを作成します。 DataTable には、メインのクエリによって返される各フィールドの列が含まれています。 TableAdapter でクリックし、[プロパティ] ウィンドウに移動します。 表示されますが、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティが対応するストアド プロシージャの呼び出しを正しく構成されています。


[![TableAdapter の Insert、Update が含まれています、機能の削除](updating-the-tableadapter-to-use-joins-vb/_static/image19.png)](updating-the-tableadapter-to-use-joins-vb/_static/image18.png)

**図 8**:、TableAdapter を含む Insert、Update、および機能の削除 ([フルサイズの画像を表示する をクリックします](updating-the-tableadapter-to-use-joins-vb/_static/image20.png))。


挿入、更新、および delete ストアド プロシージャが自動的に作成し、 `InsertCommand`、`UpdateCommand`と`DeleteCommand`をカスタマイズする準備が整いましたプロパティが正しく構成されている、 `SelectCommand` s は、プロシージャを返す追加各従業員のマネージャーについて説明します。 具体的には、更新する必要があります、`Employees_Select`ストアド プロシージャを使用して、 `JOIN` manager s を返すと`FirstName`と`LastName`値。 ストアド プロシージャが更新された後はこれらの列を含むように、DataTable を更新する必要があります。 これら 2 つのタスクの手順 2 および 3 に取り組むします。

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>手順 2: に含めるストアド プロシージャをカスタマイズします。`JOIN`

サーバー エクスプ ローラーに、Northwind データベースのストアド プロシージャのフォルダーにドリル ダウンして開くことにより、`Employees_Select`ストアド プロシージャ。 このストアド プロシージャが表示されない場合は、ストアド プロシージャのフォルダーを右クリックし、更新を選択します。 使用するように、ストアド プロシージャを更新、 `LEFT JOIN` manager s を最初に返されますし、姓、名します。


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample5.sql)]

更新した後、`SELECT`ステートメントでは、[ファイル] メニューに移動し、保存を選択して変更を保存、`Employees_Select`します。 または、ツールバーの [保存] アイコンをクリックします。 または Ctrl + S をヒットできます。 右クリックし、変更を保存した後、`Employees_Select`サーバー エクスプ ローラーでのストアド プロシージャを実行 を選択します。 これは、ストアド プロシージャを実行され、出力ウィンドウにその結果を表示する (図 9 参照)。


[![ストアド プロシージャの結果が出力ウィンドウに表示されます。](updating-the-tableadapter-to-use-joins-vb/_static/image22.png)](updating-the-tableadapter-to-use-joins-vb/_static/image21.png)

**図 9**: 出力ウィンドウに、ストアド プロシージャの結果が表示されます ([フルサイズの画像を表示する をクリックします](updating-the-tableadapter-to-use-joins-vb/_static/image23.png))。


## <a name="step-3-updating-the-datatable-s-columns"></a>手順 3: DataTable の列の更新

この時点で、 `Employees_Select` 、ストアド プロシージャ`ManagerFirstName`と`ManagerLastName`、値が、`EmployeesDataTable`これらの列がありません。 これらの不足している列は、2 つの方法で DataTable に追加できます。

- **手動で**データセット デザイナーでデータ テーブルを右クリックし、[追加] メニューから列を選択して - です。 列の名前を付け、それに応じてそのプロパティを設定します。
- **自動的に**-TableAdapter 構成ウィザードがによって返されるフィールドを反映するために DataTable の列を更新、`SelectCommand`ストアド プロシージャ。 ウィザードは削除も、アドホック SQL ステートメントを使用する場合、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティ、`SelectCommand`が含まれています、`JOIN`します。 ただし、ストアド プロシージャを使用して、これらのコマンド プロパティがそのまま残ります。

DataTable 列を手動で追加するなど、前のチュートリアルについて解説しました[マスター/マスター レコードの箇条書きリストを使用すると詳細 DataList について詳しく説明](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)と[のファイルのアップロード](../working-with-binary-files/uploading-files-vb.md)、ここで、このプロセスの詳細をもう一度、次のチュートリアルで確認します。 このチュートリアルでは、TableAdapter 構成ウィザードを使用して、自動のアプローチを使用して、使用できます。

右クリックして開始、`EmployeesTableAdapter`し、コンテキスト メニューから構成を選択します。 選択、挿入、更新、および削除、その戻り値とパラメーター (ある場合) と共に使用するストアド プロシージャを一覧表示すると、TableAdapter 構成ウィザードが表示されます。 図 10 では、このウィザードを示します。 ここで確認できます、`Employees_Select`ストアド プロシージャ、`ManagerFirstName`と`ManagerLastName`フィールド。


[![ウィザード、Employees_Select の更新された列の一覧に示すストアド プロシージャ](updating-the-tableadapter-to-use-joins-vb/_static/image25.png)](updating-the-tableadapter-to-use-joins-vb/_static/image24.png)

**図 10**: 更新の列の一覧が表示されます、`Employees_Select`ストアド プロシージャ ([フルサイズの画像を表示する をクリックします](updating-the-tableadapter-to-use-joins-vb/_static/image26.png))。


[完了] をクリックしてウィザードを完了します。 データセット デザイナーに戻ると、 `EmployeesDataTable` 2 つの列が含まれています:`ManagerFirstName`と`ManagerLastName`します。


[![EmployeesDataTable には、2 つの新しい列が含まれています。](updating-the-tableadapter-to-use-joins-vb/_static/image28.png)](updating-the-tableadapter-to-use-joins-vb/_static/image27.png)

**図 11**: `EmployeesDataTable` 2 つの新しい列が含まれています ([フルサイズの画像を表示する をクリックします](updating-the-tableadapter-to-use-joins-vb/_static/image29.png))。


説明するため、更新された`Employees_Select`ユーザーを表示し、従業員を削除できる web ページを作成して、ストアド プロシージャが有効では、挿入、更新、および、TableAdapter の機能を削除することは引き続き機能します。 このようなページを作成する前に、必要があります最初から従業員を操作するためのビジネス ロジック層で新しいクラスを作成する、`NorthwindWithSprocs`データセット。 手順 4 で作成しますが、`EmployeesBLLWithSprocs`クラス。 手順 5 では、ASP.NET ページからこのクラスを使用します。

## <a name="step-4-implementing-the-business-logic-layer"></a>手順 4: ビジネス ロジック層を実装します。

新しいクラス ファイルを作成、`~/App_Code/BLL`という名前のフォルダー`EmployeesBLLWithSprocs.vb`します。 このクラスは、既存のセマンティクスを模倣`EmployeesBLL`クラス、のみ、この新しい少ないメソッドを提供しを使用して 1 つ、`NorthwindWithSprocs`データセット (の代わりに、`Northwind`データセット)。 `EmployeesBLLWithSprocs` クラスに次のコードを追加します。


[!code-vb[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample6.vb)]

`EmployeesBLLWithSprocs`クラス s`Adapter`プロパティのインスタンスを返します、`NorthwindWithSprocs`データセットの`EmployeesTableAdapter`します。 これは、クラス s によって使用`GetEmployees`と`DeleteEmployee`メソッド。 `GetEmployees`メソッドの呼び出し、`EmployeesTableAdapter`対応する s`GetEmployees`によって実行されるメソッド、`Employees_Select`ストアド プロシージャとでは、その結果を設定します、`EmployeeDataTable`します。 `DeleteEmployee`メソッドを呼び出す同様に、 `EmployeesTableAdapter` s`Delete`によって実行されるメソッド、`Employees_Delete`ストアド プロシージャ。

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>手順 5: プレゼンテーション層でデータの使用

`EmployeesBLLWithSprocs`クラス完了すると、私たちは ASP.NET ページ経由の従業員データを処理する準備が整いました。 開く、`JOINs.aspx`ページで、`AdvancedDAL`フォルダーと、デザイナーの設定には、ツールボックスからドラッグ、GridView、`ID`プロパティを`Employees`します。 次に、GridView s のスマート タグからという名前の新しい ObjectDataSource コントロールをグリッドにバインド`EmployeesDataSource`します。

構成を使用する ObjectDataSource、`EmployeesBLLWithSprocs`クラスし、SELECT および DELETE のタブからいることを確認、`GetEmployees`と`DeleteEmployee`メソッドは、ドロップダウン リストから選択します。 ObjectDataSource の構成を完了するには、[完了] をクリックします。


[![EmployeesBLLWithSprocs クラスを使用する ObjectDataSource を構成します。](updating-the-tableadapter-to-use-joins-vb/_static/image31.png)](updating-the-tableadapter-to-use-joins-vb/_static/image30.png)

**図 12**: 構成に使用する ObjectDataSource、`EmployeesBLLWithSprocs`クラス ([フルサイズの画像を表示する をクリックします](updating-the-tableadapter-to-use-joins-vb/_static/image32.png))。


[![ObjectDataSource 使用 GetEmployees とするメソッドがあります。](updating-the-tableadapter-to-use-joins-vb/_static/image34.png)](updating-the-tableadapter-to-use-joins-vb/_static/image33.png)

**図 13**: ObjectDataSource 使用している、`GetEmployees`と`DeleteEmployee`メソッド ([フルサイズの画像を表示する をクリックします](updating-the-tableadapter-to-use-joins-vb/_static/image35.png))。


Visual Studio は各の GridView に、BoundField を追加するが、`EmployeesDataTable`の列。 これらの BoundFields 以外をすべて削除`Title`、 `LastName`、 `FirstName`、`ManagerFirstName`と`ManagerLastName`の名前を変更し、`HeaderText`姓、名、マネージャーの名、最後の 4 つ BoundFields のプロパティとマネージャーの姓、名、それぞれします。

このページから従業員を削除するユーザーを許可するのには、2 つの作業を行う必要があります。 最初に、スマート タグの削除を有効にするオプションをオンにして削除機能を提供する GridView に指示します。 ObjectDataSource s を次に、変更`OldValuesParameterFormatString`値からのプロパティは、ObjectDataSource ウィザードによって設定 (`original_{0}`) をその既定値 (`{0}`)。 これらの変更を加えたら、GridView コントロールと ObjectDataSource s 宣言型マークアップを次のようになります。


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample7.aspx)]

ブラウザーを使用して、ページをテストします。 図 14 に示すよう、各従業員と (あると仮定)、ユーザーのマネージャーの名前に、ページが表示されます。


[![Employees_Select 内の結合、ストアド プロシージャ、マネージャー名](updating-the-tableadapter-to-use-joins-vb/_static/image37.png)](updating-the-tableadapter-to-use-joins-vb/_static/image36.png)

**図 14**:`JOIN`で、`Employees_Select`ストアド プロシージャは、マネージャー名を返します ([フルサイズの画像を表示する をクリックします](updating-the-tableadapter-to-use-joins-vb/_static/image38.png))。


実行が完了する、削除ワークフローの開始、削除ボタンをクリックすると、`Employees_Delete`ストアド プロシージャ。 ただし、試行`DELETE`外部キー制約違反のため、ストアド プロシージャ内のステートメントは失敗し (図 15 を参照してください)。 具体的には、各従業員が、1 つまたは複数のレコード、`Orders`テーブル、削除が失敗します。


[![外部キー制約の違反に対応する注文の結果を持つ従業員を削除します。](updating-the-tableadapter-to-use-joins-vb/_static/image40.png)](updating-the-tableadapter-to-use-joins-vb/_static/image39.png)

**図 15**: を外部キー制約の違反に対応する注文の結果を持つ従業員を削除しています ([フルサイズの画像を表示する をクリックします](updating-the-tableadapter-to-use-joins-vb/_static/image41.png))。


削除する従業員を許可する可能性があります。

- 更新プログラムを削除、カスケード外部キー制約
- レコードを手動で削除、`Orders`を削除するには、名のテーブルまたは
- 更新プログラム、`Employees_Delete`最初から、関連するレコードを削除するのには、プロシージャ、`Orders`テーブルを削除する前に、`Employees`レコード。 この手法で説明した、[型指定されたデータセット s Tableadapter の既存のストアド プロシージャの使用](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)チュートリアル。

ままを演習として、リーダーの。

## <a name="summary"></a>まとめ

リレーショナル データベースを使用する場合は、関連テーブルにクエリ複数からのデータをプルするが一般的です。 相関サブクエリと`JOIN`s は、クエリ内の関連テーブルからデータにアクセスするため 2 つの異なる手法を提供します。 TableAdapter は自動生成のために、前のチュートリアルで最もよく行いましたが相関サブクエリの使用`INSERT`、 `UPDATE`、および`DELETE`ステートメントが含まれたクエリの`JOIN`秒。 アドホック SQL ステートメントを使用する場合に、これらの値を手動で指定することができます、TableAdapter 構成ウィザードが完了したときに、カスタマイズが上書きされます。

さいわい、ストアド プロシージャを使用して作成された Tableadapter では、アドホック SQL ステートメントを使用して作成されたものと同じの脆弱性からことはありません。 そのため、メインのクエリを使用して TableAdapter を作成することは、`JOIN`ストアド プロシージャを使用する場合。 このチュートリアルでは、このような TableAdapter を作成する方法を説明しました。 使用して開始した、 `JOIN`-小さい`SELECT`TableAdapter のメイン クエリのクエリを実行できるように、対応する insert、update、および削除のストアド プロシージャは自動的に作成します。 TableAdapter s 初期構成の完了を補強、`SelectCommand`ストアド プロシージャを使用して、`JOIN`を更新する TableAdapter 構成ウィザードを再実行し、`EmployeesDataTable`の列。

TableAdapter 構成ウィザードが自動的に更新を再実行、`EmployeesDataTable`列によって返されるデータ フィールドを反映するように、`Employees_Select`ストアド プロシージャ。 代わりに、私たちでしたがこれらの列に手動で追加、DataTable。 列を手動で追加する、DataTable に、次のチュートリアルで説明されます。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Hilton Geisenow、David Suru、および Teresa Murphy でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [次へ](adding-additional-datatable-columns-vb.md)
