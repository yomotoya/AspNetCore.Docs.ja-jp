---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
title: 計算列 (VB) を使用する |Microsoft Docs
author: rick-anderson
description: Microsoft SQL Server を使用すると、その値が式から計算されますが、計算列の定義、データベース テーブルを作成するときに通常、referen.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 5811b8ff-ed56-40fc-9397-6b69ae09a8f6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 5837baccb9e378546d26703c99dcddd144ba80de
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386774"
---
<a name="working-with-computed-columns-vb"></a>計算列 (VB) を使用します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_VB.zip)または[PDF のダウンロード](working-with-computed-columns-vb/_static/datatutorial71vb1.pdf)

> データベース テーブルを作成するときは、Microsoft SQL Server を使用すると、その値は通常、同じデータベース レコードには、その他の値を参照する式から計算されます、計算列を定義できます。 このような値とは、Tableadapter を使用する場合は、特別な考慮事項を必要とすると、データベースは読み取り専用です。 このチュートリアルでは、計算列によってもたらされる課題に対応する方法について説明します。


## <a name="introduction"></a>はじめに

Microsoft SQL Server では、 *[計算列](https://msdn.microsoft.com/library/ms191250.aspx)*、これらは列の値は通常、同じテーブル内の他の列から値を参照する式から計算されます。 たとえば、データ モデルを追跡する時間がという名前のテーブルを必要があります`ServiceLog`を含む列を含む`ServicePerformed`、 `EmployeeID`、 `Rate`、および`Duration`、他のユーザーの間で。 支払額を中にサービスごと項目 (期間を掛けた料金をされている) は、web ページまたはその他のプログラム インターフェイスを介して計算でした、内の列を含める便利な場合があります、`ServiceLog`という名前のテーブル`AmountDue`これを報告します。情報。 この列は、通常の列として作成する可能性がありますが、いつでも更新することは、`Rate`または`Duration`列値を変更します。 優れたアプローチするように設定、`AmountDue`列、式を使用して計算列`Rate * Duration`します。 これにより、SQL Server を自動的に計算、`AmountDue`クエリで参照されていたときに、列の値。

計算列の値は、式によって決まりますがため、このような列は読み取り専用とそのためことはできませんが値に割り当てられたで`INSERT`または`UPDATE`ステートメント。 ただし、計算列が、アドホック SQL ステートメントを使用する TableAdapter のメイン クエリの一部である場合は、これらは自動的に追加で自動生成された`INSERT`と`UPDATE`ステートメント。 Tableadapter ではその結果、`INSERT`と`UPDATE`クエリと`InsertCommand`と`UpdateCommand`計算列への参照を削除するプロパティを更新する必要があります。

使用しての課題の 1 つは、アドホック SQL ステートメントを使用する TableAdapter の列を計算する、tableadapter は、`INSERT`と`UPDATE`クエリには、いつでも、TableAdapter 構成ウィザードの完了が自動的に再生成します。 そのため、手動で計算列がから削除、`INSERT`と`UPDATE`ウィザードが再実行クエリが表示されなくなります。 ストアド プロシージャを使用する Tableadapter のないこの脆弱性の低下が、手順 3 で扱うは独自の癖が。

このチュートリアルでは、計算列を追加は、 `Suppliers` Northwind データベースのテーブルが表示され、このテーブルとその計算列を操作する対応する TableAdapter を作成します。 TableAdapter 構成ウィザードを使用すると、カスタマイズは t が失われるように、アドホック SQL ステートメントの代わりにストアド プロシージャを使用して、TableAdapter に、します。

Let s を始めましょう。

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>手順 1: 計算列を追加する、`Suppliers`テーブル

Northwind データベースには計算列はないので、私たちは自分たちの 1 つを追加する必要があります。 このチュートリアルが s の計算列を追加できるようにするため、`Suppliers`という名前のテーブル`FullContactName`連絡先の名前、タイトル、および各自が勤務する会社を返す、次の形式: `ContactName` (`ContactTitle`、 `CompanyName`)。 これは、計算列は仕入先に関する情報を表示するときに、レポートで使用する場合があります。

開いて開始、`Suppliers`テーブル定義を右クリックして、`Suppliers`サーバー エクスプ ローラーでテーブルし、テーブル定義を開くのコンテキスト メニューから選択します。 これが可能になるかどうか、テーブルとそれらのデータ型などのプロパティの列が表示されます`NULL`s など。 計算列を追加するには、テーブルの定義に列の名前を入力して起動します。 列のプロパティ ウィンドウで計算列の指定 セクション (数式) テキスト ボックスに次に、その式を入力します (図 1 参照)。 計算列の名前を`FullContactName`し、次の式を使用します。


[!code-sql[Main](working-with-computed-columns-vb/samples/sample1.sql)]

Sql 文字列を連結できることに注意してください。 を使用して、`+`演算子。 `CASE`と従来のプログラミング言語での条件付きステートメントに使用できます。 上記の式で、`CASE`としてステートメントを読み取ることができます: 場合`ContactTitle`でない`NULL`出力し、`ContactTitle`それ以外の場合に、コンマで連結した値が何を出力します。 詳細の有用性について、`CASE`ステートメントを参照してください[SQL の電源`CASE`ステートメント](http://www.4guysfromrolla.com/webtech/102704-1.shtml)します。

> [!NOTE]
> 使用する代わりに、`CASE`ステートメントは、ここでは、別の方法として使用できます`ISNULL(ContactTitle, '')`します。 [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) 返します*checkExpression* NULL 以外の場合、それ以外の場合を返します*replacementValue*します。 中にいずれか`ISNULL`または`CASE`作業は、このインスタンスより複雑なシナリオでの柔軟性、`CASE`ステートメントは、によって照合されることはできません`ISNULL`。


この計算列を追加した後、画面が図 1 でスクリーン ショットのようになります。


[![Suppliers テーブルを FullContactName をという名前の計算列を追加します。](working-with-computed-columns-vb/_static/image2.png)](working-with-computed-columns-vb/_static/image1.png)

**図 1**: 計算列という追加`FullContactName`を`Suppliers`テーブル ([フルサイズの画像を表示する をクリックします](working-with-computed-columns-vb/_static/image3.png))。


計算列の名前を付け、その式を入力して後に、変更を保存の表に、ツールバーの [保存] アイコンをクリックして、[ファイル] メニューに移動して保存を選択または Ctrl + S を押す`Suppliers`します。

テーブルを保存だけで追加した列を含む、サーバー エクスプ ローラーを更新する必要があります、`Suppliers`テーブル列のリスト。 (式) のテキスト ボックスに入力された式が不要な空白文字を削除、列名を角かっこで囲まれている同等の式を自動的に調整されてさらに、(`[]`) より明示的に表示するためにかっこが含まれています操作の順序:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample2.sql)]

Microsoft SQL Server の計算列の詳細についてを参照してください、[のテクニカル ドキュメント](https://msdn.microsoft.com/library/ms191250.aspx)します。 チェック アウトも、[方法: 計算列の指定](https://msdn.microsoft.com/library/ms188300.aspx)計算列の作成のステップ バイ ステップ チュートリアルについてはします。

> [!NOTE]
> 計算列は既定では、テーブルに物理的に格納されませんが代わりに、クエリで参照されるたびに再計算されます。 ただし、永続化は、チェック ボックスをオンは、物理的にテーブルに計算列を格納する SQL Server を指示できます。 これにより、計算列の値を使用するクエリのパフォーマンスを向上させることができます、計算列上に作成されるインデックス、`WHERE`句。 参照してください[計算列でインデックスを作成する](https://msdn.microsoft.com/library/ms189292.aspx)詳細についてはします。


## <a name="step-2-viewing-the-computed-column-s-values"></a>手順 2: 計算列の値を表示します。

Let s を表示するには、1 分でデータ アクセス層の作業を始める前に、`FullContactName`値。 サーバー エクスプ ローラーを右クリックし、`Suppliers`テーブル名と、コンテキスト メニューから新しいクエリを選択します。 これは、クエリに含めるには、どのようなテーブルを選択することを要求するクエリ ウィンドウが表示されます。 追加、`Suppliers`テーブルし、[閉じる] をクリックします。 次に、確認、 `CompanyName`、 `ContactName`、 `ContactTitle`、および`FullContactName`Suppliers テーブルから列。 最後に、クエリを実行し、結果を表示するには、ツールバーの赤色の感嘆符アイコンをクリックします。

図 2 は、結果が含まれます`FullContactName`が一覧表示、 `CompanyName`、 `ContactName`、および`ContactTitle`形式を使用して列`ContactName`(`ContactTitle`、 `CompanyName`)。


[![FullContactName 形式 ContactName (部署、CompanyName) を使用します。](working-with-computed-columns-vb/_static/image5.png)](working-with-computed-columns-vb/_static/image4.png)

**図 2**:`FullContactName`形式を使用して`ContactName`(`ContactTitle`、 `CompanyName`) ([フルサイズの画像を表示する をクリックします](working-with-computed-columns-vb/_static/image6.png))。


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>手順 3: 追加、`SuppliersTableAdapter`データ アクセス層に

アプリケーションでは、仕入先情報を処理するためには、まず、DAL で TableAdapter と DataTable を作成する必要があります。 理想的には、これを達成する前のチュートリアルで同じ簡単な手順を使用します。 ただし、計算列の使用には、ディスカッションだけの価値をいくつかのしわが導入されています。

アドホック SQL ステートメントを使用する TableAdapter を使用している場合、TableAdapter 構成ウィザードを使用して TableAdapter のメイン クエリで計算列を単純に含めることができます。 これは、ただし、自動生成`INSERT`と`UPDATE`計算列を含むステートメント。 これらのメソッドのいずれかを実行しようとした場合、`SqlException`メッセージ列*ColumnName*か計算列、または UNION 演算子の結果がスローされますがために変更できません。 中に、`INSERT`と`UPDATE`ステートメントは、tableadapter で手動で調整`InsertCommand`と`UpdateCommand`プロパティ、これらのカスタマイズは失われます TableAdapter 構成ウィザードが再実行されるたびにします。

アドホック SQL ステートメントを使用して Tableadapter の脆弱性、により、計算列を使用する場合、ストアド プロシージャを使用しましたをお勧めします。 既存のストアド プロシージャを使用している場合だけ、TableAdapter でを構成、[型指定されたデータセット s Tableadapter の既存のストアド プロシージャの使用](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)チュートリアル。 ストアド プロシージャは作成、TableAdapter ウィザードを使っている場合が最初に、メイン クエリの計算列を省略する場合に重要です。 場合は、メイン クエリで計算列を含めると、TableAdapter 構成ウィザードが表示されます、完了すると、対応するストアド プロシージャを作成することはできません。 つまり、最初に計算列のないメイン クエリを使用して、TableAdapter を構成し、対応するストアド プロシージャと、tableadapter を手動で更新する必要があります`SelectCommand`計算列を含めるようにします。 このアプローチで使用されるものと似ています、 [TableAdapter を使用する更新](updating-the-tableadapter-to-use-joins-vb.md)`JOIN`*s*チュートリアル。

このチュートリアルでは新しい TableAdapter を追加し、私たちのストアド プロシージャを自動的に作成させることができます。 最初に省略する必要がありますが、その結果、`FullContactName`メイン クエリから列を計算します。

開いて開始、`NorthwindWithSprocs`データセットで、`~/App_Code/DAL`フォルダー。 デザイナーを右クリックし、コンテキスト メニューから新しい TableAdapter を追加することもできます。 これにより、TableAdapter 構成ウィザードが起動します。 データをクエリするデータベースを指定 (`NORTHWNDConnectionString`から`Web.config`) [次へ] をクリックします。 クエリまたは変更するための任意のストアド プロシージャを作成しないしましたので、`Suppliers`テーブルで、新しいストアド プロシージャはオプション ウィザードを作成し、次へ を作成 を選択します。


[![作成する新しいストアド プロシージャ オプションを選択します。](working-with-computed-columns-vb/_static/image8.png)](working-with-computed-columns-vb/_static/image7.png)

**図 3**: を作成する新しいストアド プロシージャ オプションの選択 ([フルサイズの画像を表示する をクリックします](working-with-computed-columns-vb/_static/image9.png))。


後続の手順には、メイン クエリのことが求められます。 返す次のクエリを入力して、 `SupplierID`、 `CompanyName`、 `ContactName`、および`ContactTitle`各仕入先の列。 このクエリは、計算列を意図的省略に注意してください (`FullContactName`); 手順 4. でこの列を含めるように対応するストアド プロシージャが更新されます。


[!code-sql[Main](working-with-computed-columns-vb/samples/sample3.sql)]

ウィザードでは、メインのクエリを入力し、次にクリックするを生成する 4 つのストアド プロシージャの名前を付けることができます。 これらのストアド プロシージャの名前を付けます`Suppliers_Select`、 `Suppliers_Insert`、 `Suppliers_Update`、および`Suppliers_Delete`図 4 に示すようにします。


[![自動生成ストアド プロシージャの名前をカスタマイズします。](working-with-computed-columns-vb/_static/image11.png)](working-with-computed-columns-vb/_static/image10.png)

**図 4**: Auto-Generated ストアド プロシージャの名前をカスタマイズ ([フルサイズの画像を表示する をクリックします](working-with-computed-columns-vb/_static/image12.png))。


ウィザードの次の手順では、メソッド、TableAdapter の名前し、データにアクセスして更新するために使用するパターンを指定できます。 すべての 3 つチェック ボックスをオンにしたままにしますが、名前を変更、`GetData`メソッドを`GetSuppliers`します。 ウィザードを完了するには、[完了] をクリックします。


[![GetSuppliers に GetData メソッドの名前を変更します。](working-with-computed-columns-vb/_static/image14.png)](working-with-computed-columns-vb/_static/image13.png)

**図 5**: 名前の変更、`GetData`メソッドを`GetSuppliers`([フルサイズの画像を表示する をクリックします](working-with-computed-columns-vb/_static/image15.png))。


[完了] をクリックすると、ウィザードは 4 つのストアド プロシージャを作成し、型指定されたデータセットに TableAdapter と対応する DataTable を追加します。

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>手順 4: TableAdapter のメイン クエリで計算列を含む

これで、TableAdapter を更新する必要があり、DataTable の作成に含める手順 3. で、`FullContactName`計算列。 これには、2 つの手順が含まれます。

1. 更新、`Suppliers_Select`ストアド プロシージャから返される、`FullContactName`計算列、および
2. 更新に対応する DataTable`FullContactName`列。

サーバー エクスプ ローラーに移動し、ストアド プロシージャ フォルダーにドリル ダウンを開始します。 開く、`Suppliers_Select`ストアド プロシージャと更新プログラム、`SELECT`クエリに含まれる、`FullContactName`計算列。


[!code-sql[Main](working-with-computed-columns-vb/samples/sample4.sql)]

ツールバーの 保存 アイコンをクリックすると、Ctrl + S を押す、または保存を選択して、ストアド プロシージャに変更を保存`Suppliers_Select`ファイル メニュー オプション。

次に、データセット デザイナーに戻り、右クリックし、 `SuppliersTableAdapter`、コンテキスト メニューから構成を選択します。 なお、`Suppliers_Select`列が含まれています、`FullContactName`データ列がコレクション内の列。


[![DataTable の列を更新するには、TableAdapter の構成ウィザードを実行します。](working-with-computed-columns-vb/_static/image17.png)](working-with-computed-columns-vb/_static/image16.png)

**図 6**: tableadapter の DataTable の列を更新する構成ウィザードを実行 ([フルサイズの画像を表示する をクリックします](working-with-computed-columns-vb/_static/image18.png))。


ウィザードを完了するには、[完了] をクリックします。 これに対応する列を自動的に追加されます、`SuppliersDataTable`します。 TableAdapter ウィザードでは、ことを検出するのに十分なスマート、`FullContactName`列が計算列および読み取り専用です。 その結果、s の列を設定`ReadOnly`プロパティを`true`します。 これを確認するから列を選択、`SuppliersDataTable`し、[プロパティ] ウィンドウに移動します (図 7 を参照してください)。 なお、`FullContactName`列 s`DataType`と`MaxLength`プロパティが適宜設定もします。


[![FullContactName 列が読み取り専用としてマークされています。](working-with-computed-columns-vb/_static/image20.png)](working-with-computed-columns-vb/_static/image19.png)

**図 7**:`FullContactName`列は読み取り専用とマークされている ([フルサイズの画像を表示する をクリックします](working-with-computed-columns-vb/_static/image21.png))。


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>手順 5: 追加、`GetSupplierBySupplierID`TableAdapter にメソッド

このチュートリアルでは、更新可能なグリッドで仕入先を表示する ASP.NET ページを作成します。 過去のチュートリアルを更新したビジネス ロジック層から 1 つのレコードに変更を反映する DAL バックアップとして厳密に型指定された DataTable、そのプロパティを更新し、更新された DataTable を送信 DAL から特定のレコードを取得することによってデータベースです。 最初に追加する - DAL から更新されるレコードの取得 - この最初の手順を実行する、 `GetSupplierBySupplierID(supplierID)` DAL に対するメソッド。

右クリックし、`SuppliersTableAdapter`データセット デザインで、コンテキスト メニューから、クエリの追加オプションを選択します。 手順 3 で行ったように、私たちにとって新しいストアド プロシージャの作成 オプションを選択して、新しいストアド プロシージャを生成するウィザード (参照図 3 のこの手順のスクリーン ショット)。 このメソッドでは、複数の列を持つレコードを返します、ため複数行を返す SELECT である SQL クエリを使用し、[次へ] をクリックすることを示します。


[![選択 オプションを複数行を返す](working-with-computed-columns-vb/_static/image23.png)](working-with-computed-columns-vb/_static/image22.png)

**図 8**: [選択] オプションを複数行を返す ([フルサイズの画像を表示する をクリックします](working-with-computed-columns-vb/_static/image24.png))。


後続の手順では、このメソッドを使用するクエリの場合、us ように求められます。 特定仕入先が、メインのクエリとしては、同じデータ フィールドを返すと、次を入力します。


[!code-sql[Main](working-with-computed-columns-vb/samples/sample5.sql)]

次の画面は、自動生成されるストアド プロシージャの名前にします。 このストアド プロシージャの名前を付けます`Suppliers_SelectBySupplierID`[次へ] をクリックします。


[![ストアド プロシージャ Suppliers_SelectBySupplierID 名](working-with-computed-columns-vb/_static/image26.png)](working-with-computed-columns-vb/_static/image25.png)

**図 9**: ストアド プロシージャの名前を付けます`Suppliers_SelectBySupplierID`([フルサイズの画像を表示する をクリックします](working-with-computed-columns-vb/_static/image27.png))。


最後に、ウィザードの指示、データの場合、us へのアクセス パターンと、TableAdapter を使用するメソッドの名前。 両方のチェック ボックスがオンのままにしますが、名前を変更、`FillBy`と`GetDataBy`メソッド`FillBySupplierID`と`GetSupplierBySupplierID`、それぞれします。


[![名前、TableAdapter メソッド FillBySupplierID と GetSupplierBySupplierID](working-with-computed-columns-vb/_static/image29.png)](working-with-computed-columns-vb/_static/image28.png)

**図 10**: TableAdapter のメソッドの名前を付けます`FillBySupplierID`と`GetSupplierBySupplierID`([フルサイズの画像を表示する をクリックします](working-with-computed-columns-vb/_static/image30.png))。


ウィザードを完了するには、[完了] をクリックします。

## <a name="step-6-creating-the-business-logic-layer"></a>手順 6: ビジネス ロジック層を作成します。

手順 1 で作成した計算列を使用する ASP.NET ページを作成する前にまず BLL に対応するメソッドを追加する必要があります。 ASP.NET ページで、手順 7. で作成しますが、これにより、ユーザーを表示および suppliers を編集します。 そのため、当社 BLL には少なくとも、すべての仕入先と別特定のサプライヤーの更新を取得する方法を提供する必要があります。

という名前の新しいクラス ファイルを作成`SuppliersBLLWithSprocs`で、`~/App_Code/BLL`フォルダーし、次のコードを追加します。


[!code-vb[Main](working-with-computed-columns-vb/samples/sample6.vb)]

などの他の BLL クラス`SuppliersBLLWithSprocs`が、 `Protected` `Adapter`プロパティのインスタンスを返す、`SuppliersTableAdapter`クラスと共に使用する 2 つ`Public`メソッド:`GetSuppliers`と`UpdateSupplier`します。 `GetSuppliers`メソッドの呼び出しを返します、 `SuppliersDataTable` 、対応するによって返される`GetSupplier`データ アクセス層のメソッド。 `UpdateSupplier`メソッド呼び出し DAL 秒に更新されている特定の仕入先に関する情報を取得`GetSupplierBySupplierID(supplierID)`メソッド。 これは、後更新、 `CategoryName`、 `ContactName`、および`ContactTitle`プロパティとデータ アクセス層の s を呼び出すことによって、これらの変更をデータベースにコミット`Update`メソッドで変更された`SuppliersRow`オブジェクト。

> [!NOTE]
> 除く`SupplierID`と`CompanyName`、Suppliers テーブルのすべての列を許可する`NULL`値。 そのため場合、渡されたで`contactName`または`contactTitle`パラメーターは`Nothing`、対応するを設定する必要があります`ContactName`と`ContactTitle`プロパティを`NULL`データベース値を使用して、`SetContactNameNull`と`SetContactTitleNull`メソッドでは、それぞれします。


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>手順 7: プレゼンテーション層から計算列の使用

追加された計算列で、`Suppliers`テーブルと、DAL と BLL 適宜更新されるようで動作する ASP.NET ページを作成する準備ができました、`FullContactName`計算列。 開いて開始、`ComputedColumns.aspx`ページで、`AdvancedDAL`フォルダーとツールボックスからデザイナーにドラッグする GridView。 GridView s 設定`ID`プロパティを`Suppliers`し、スマート タグ、という名前の新しい ObjectDataSource にバインドする`SuppliersDataSource`します。 構成を使用する ObjectDataSource、`SuppliersBLLWithSprocs`クラスを追加しましたは手順 6. でバックアップし、[次へ] をクリックします。


[![SuppliersBLLWithSprocs クラスを使用する ObjectDataSource を構成します。](working-with-computed-columns-vb/_static/image32.png)](working-with-computed-columns-vb/_static/image31.png)

**図 11**: 構成に使用する ObjectDataSource、`SuppliersBLLWithSprocs`クラス ([フルサイズの画像を表示する をクリックします](working-with-computed-columns-vb/_static/image33.png))。


定義されている 2 つのメソッドがある、`SuppliersBLLWithSprocs`クラス:`GetSuppliers`と`UpdateSupplier`します。 いることを確認これら 2 つのメソッドは、SELECT で指定と、それぞれのタブを更新 ObjectDataSource の構成を完了するには、[完了] をクリックします。

データ ソース構成ウィザードの完了したら、Visual Studio は、返されるデータ フィールドの各、BoundField を追加します。 削除、 `SupplierID` BoundField を変更して、`HeaderText`のプロパティ、 `CompanyName`、 `ContactName`、`ContactTitle`と`FullContactName`BoundFields する会社、連絡先の名前、タイトル、および連絡先の氏名、それぞれします。 スマート タグからには、GridView s 組み込み編集機能を有効にする編集を有効にするチェック ボックスをオンします。

BoundFields を GridView に追加するだけでなく、データ ソース ウィザードの完了も Visual Studio によって ObjectDataSource s を設定する`OldValuesParameterFormatString`プロパティを元に\_{0}します。 この設定を元の既定値に戻す{0}します。

GridView と ObjectDataSource にこれらの編集を行った後、宣言型マークアップは次のようになります。


[!code-aspx[Main](working-with-computed-columns-vb/samples/sample7.aspx)]

次に、ブラウザーからこのページを参照してください。 含むグリッドの各仕入先が表示されている図 12 に示すよう、`FullContactName`として書式設定された値が他の 3 つの列を連結したものでは単に列`ContactName`(`ContactTitle`、 `CompanyName`)。


[![各仕入先がグリッドに一覧表示します。](working-with-computed-columns-vb/_static/image35.png)](working-with-computed-columns-vb/_static/image34.png)

**図 12**: 各仕入先のグリッドが記載されています ([フルサイズの画像を表示する をクリックします](working-with-computed-columns-vb/_static/image36.png))。


特定のサプライヤーがポストバックを発生するがその行で表示、編集ボタンをクリックして (図 13 を参照してください) の編集インターフェイスします。 最初の 3 つの列は、既定のインターフェイスの編集でレンダリング - いるテキスト ボックス コントロール`Text`プロパティがデータ フィールドの値に設定します。 `FullContactName`テキストとしてただし、列のままです。 GridView、データ ソース構成ウィザードの完了時に、BoundFields が追加されたときに、 `FullContactName` BoundField s`ReadOnly`プロパティに設定されました`True`ため、対応する`FullContactName`内の列、 `SuppliersDataTable`その`ReadOnly`プロパティに設定`True`します。 手順 4 で説明したように、 `FullContactName` s`ReadOnly`プロパティに設定されました`True`TableAdapter の列が計算列が検出されたためです。


[![FullContactName 列が編集できません。](working-with-computed-columns-vb/_static/image38.png)](working-with-computed-columns-vb/_static/image37.png)

**図 13**:`FullContactName`列が編集できない ([フルサイズの画像を表示する をクリックします](working-with-computed-columns-vb/_static/image39.png))。


1 つまたは複数の編集可能な列の値を更新して更新 をクリックします。 注方法、`FullContactName`秒の値が変更を反映するように自動的に更新します。

> [!NOTE]
> GridView 現在使用して BoundFields 編集可能なフィールドの編集インターフェイス、既定値します。 以降、`CompanyName`フィールドは必須です、それを含む、RequiredFieldValidator TemplateField に変換します。 ままを演習として興味を持った読者にします。 参照してください、[編集および挿入インターフェイスに検証コントロールを追加](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)チュートリアル手順について、BoundField を TemplateField に変換して、検証コントロールを追加します。


## <a name="summary"></a>まとめ

テーブルのスキーマを定義するときに Microsoft SQL Server によって計算列を含めることができます。 これらは列の値は通常、同じレコード内の他の列から値を参照する式から計算されます。 値から計算列は、式に基づく、読み取り専用の値を割り当てることができません、`INSERT`または`UPDATE`ステートメント。 対応するを自動的に生成しようとする TableAdapter のメイン クエリで計算列を使用する場合の課題が生じます`INSERT`、 `UPDATE`、および`DELETE`ステートメント。

このチュートリアルでは、計算列によってもたらされる課題を回避する方法について説明します。 具体的には、使用してストアド プロシージャ、TableAdapter でアドホック SQL ステートメントを使用して Tableadapter で本質的な脆弱性を克服しました。 ストアド プロシージャを新規作成、TableAdapter ウィザードを使用する場合は、最初に自分の存在により、データの変更ストアド プロシージャが生成されているため、計算列を省略するメインのクエリがあることが重要です。 TableAdapter が最初に構成した後、`SelectCommand`ストアド プロシージャは、計算列を含める retooled ことができます。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者では、Hilton Geisenow Teresa Murphy はでした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](adding-additional-datatable-columns-vb.md)
> [次へ](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
