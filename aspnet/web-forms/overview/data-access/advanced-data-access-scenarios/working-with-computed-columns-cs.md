---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
title: 計算列 (c#) の使用 |Microsoft ドキュメント
author: rick-anderson
description: Microsoft SQL Server が、式からその値を計算する計算列を定義することができます、データベース テーブルを作成するときに通常、referen しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 57459065-ed7c-4dfe-ac9c-54c093abc261
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 7a67abd2a0c140c0503c07f764549a6d90ef7298
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="working-with-computed-columns-c"></a>計算列 (c#) の使用
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_CS.zip)または[PDF のダウンロード](working-with-computed-columns-cs/_static/datatutorial71cs1.pdf)

> データベース テーブルを作成するときに Microsoft SQL Server では、通常は同じデータベース レコードの他の値を参照する式からその値を計算する計算列を定義することができます。 そのような値は、Tableadapter を使用する場合は、特別な考慮事項を必要とすると、データベースに読み取り専用です。 このチュートリアルでは、計算列によって引き起こされる問題に対応する方法について説明します。


## <a name="introduction"></a>はじめに

Microsoft SQL Server では、 *[計算列](https://msdn.microsoft.com/library/ms191250.aspx)*、これらは列の値は、通常、同じテーブル内の他の列から値を参照する式から計算されます。 例として、追跡データ モデルにテーブルがあるという`ServiceLog`を含む列を含む`ServicePerformed`、 `EmployeeID`、 `Rate`、および`Duration`、他のユーザー間でします。 額を中にサービスごとの項目 (される期間を掛けたレート) でしたが計算されます web ページまたはその他のプログラム インターフェイス経由で列を含めるように便利な場合があります、`ServiceLog`という名前のテーブル`AmountDue`を報告この情報です。 この列は、通常の列として作成できませんでしたが、いつでも更新する必要があります、`Rate`または`Duration`列値を変更します。 させることにより、`AmountDue`列、式を使用して計算列`Rate * Duration`です。 これには SQL Server が自動的に計算が発生、`AmountDue`クエリで参照されていたときに、列の値。

計算列の値は、式によって決まりますがため、このような列読み取り専用あり、したがってできない値に割り当てたでそれら`INSERT`または`UPDATE`ステートメントです。 ただし、計算列が、アドホック SQL ステートメントを使用する TableAdapter のメイン クエリの一部である場合は、それらが自動的に追加で自動生成された`INSERT`と`UPDATE`ステートメントです。 Tableadapter ではその結果、`INSERT`と`UPDATE`クエリと`InsertCommand`と`UpdateCommand`計算列への参照を削除するプロパティを更新する必要があります。

使用する 1 つの課題は、アドホック SQL ステートメントを使用して、TableAdapter を持つ列を計算する、tableadapter は、`INSERT`と`UPDATE`クエリには、いつでも、TableAdapter 構成ウィザードが完了したが自動的に再生成します。 そのため、計算列から手動で削除、`INSERT`と`UPDATE`場合、ウィザードを再実行クエリが再表示します。 ストアド プロシージャを使用する Tableadapter のないこの脆弱性が、手順 3. で対処する独自の quirks です。

このチュートリアルでは、計算列に、追加、 `Suppliers` Northwind データベースのテーブルが表示され、このテーブルとその計算列を操作する対応する TableAdapter を作成します。 アドホック SQL ステートメントではなくストアド プロシージャを使用して、TableAdapter 構成ウィザードを使用すると、カスタマイズは t が失われるように、TableAdapter があります。

開始 s を let!

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>手順 1: する計算列の追加、`Suppliers`テーブル

Northwind データベースには計算列はないのでおは社内で 1 つを追加する必要があります。 このチュートリアルは、計算列を追加 s を使用できます。 の、`Suppliers`という名前のテーブル`FullContactName`次の形式で連絡先の名前、タイトル、および会社の作業を返す: `ContactName` (`ContactTitle`、 `CompanyName`)。 これは、計算列は仕入先に関する情報を表示するときに、レポートで使用する場合があります。

開いて開始、`Suppliers`テーブル定義を右クリックして、`Suppliers`サーバー エクスプ ローラーでテーブルし、テーブル定義を開くのコンテキスト メニューから選択します。 これが可能にするかどうかの列、テーブルと、そのデータ型などのプロパティが表示されます`NULL`s などです。 計算列を追加するには、テーブルの定義に列の名前を入力して起動します。 次に、その式テキスト ボックスに入力 (数式) 列のプロパティ] ウィンドウの [計算列の指定」セクションの下 (図 1 を参照してください)。 計算列の名前を付けます`FullContactName`し、次の式を使用します。


[!code-sql[Main](working-with-computed-columns-cs/samples/sample1.sql)]

SQL の文字列を連結できることに注意してください。 を使用して、`+`演算子。 `CASE`ステートメントは、従来のプログラミング言語で条件付きのように使用することができます。 上記の式で、`CASE`としてステートメントを読み取ることができます: 場合`ContactTitle`は`NULL`し、出力、`ContactTitle`コンマ、それ以外の場合と連結された値を出力何も行われません。 詳細の有用性について、`CASE`ステートメントを参照してください[SQL の電源`CASE`ステートメント](http://www.4guysfromrolla.com/webtech/102704-1.shtml)です。

> [!NOTE]
> 使用する代わりに、`CASE`ステートメントは、ここではまたは使用することも`ISNULL(ContactTitle, '')`します。 [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) 返します*checkExpression* NULL 以外の場合、それ以外の場合を返します*replacementValue*です。 どちらか一方`ISNULL`または`CASE`は機能、このインスタンスより複雑なシナリオは場所の柔軟性、`CASE`でステートメントを照合することはできません`ISNULL`です。


この計算列を追加した後、画面が図 1 でスクリーン ショットのようになります。


[![Suppliers テーブルを FullContactName を名前付き計算列を追加します。](working-with-computed-columns-cs/_static/image2.png)](working-with-computed-columns-cs/_static/image1.png)

**図 1**: 追加、計算列の名前は`FullContactName`を`Suppliers`テーブル ([フルサイズのイメージを表示するをクリックして](working-with-computed-columns-cs/_static/image3.png))


計算列の名前を付け、その式を入力して後の変更を保存の表に、ツールバーの 保存 アイコンをクリックして、Ctrl + S をクリックして、またはファイル メニューの 保存 を選択して`Suppliers`です。

テーブルを保存する必要があります、サーバー エクスプ ローラーを更新で単に追加された列を含む、`Suppliers`テーブル列の一覧です。 (数式) ボックスに入力された式は不要な空白文字を削除、列名を角かっこで囲まれている同等の式を自動的に調整されてさらに、(`[]`) より明示的に表示するかっこが含まれています操作の順序:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample2.sql)]

Microsoft SQL Server の計算列の詳細についてを参照してください、[のテクニカル ドキュメント](https://msdn.microsoft.com/library/ms191250.aspx)です。 チェック アウト、[する方法: 計算列の指定](https://msdn.microsoft.com/library/ms188300.aspx)計算列の作成のステップ バイ ステップ チュートリアルについてはします。

> [!NOTE]
> 既定では、計算列は、テーブルに物理的に格納されていないが、クエリで参照されるたびに再計算は、代わりにします。 ただし、チェック ボックスをオンが永続化されるには、計算列をテーブルに物理的に格納する SQL Server を指示することができます。 これにより、インデックスを作成する、計算列、計算列の値を使用するクエリのパフォーマンスを向上させることができます、`WHERE`句。 参照してください[計算列のインデックスを作成する](https://msdn.microsoft.com/library/ms189292.aspx)詳細についてはします。


## <a name="step-2-viewing-the-computed-column-s-values"></a>手順 2: 計算列の値の表示

データ アクセス層の作業を始める前に let s、時間がかかるを表示する、`FullContactName`値。 サーバー エクスプ ローラーを右クリックし、`Suppliers`テーブル名と、コンテキスト メニューから新しいクエリを選択します。 これは、どのようなクエリに含めるテーブルを選択するように入力が求められるクエリ ウィンドウが表示されます。 追加、`Suppliers`テーブルし、[閉じる] をクリックします。 次に、確認、 `CompanyName`、 `ContactName`、 `ContactTitle`、および`FullContactName`Suppliers テーブルから列です。 最後に、クエリを実行し、結果を表示するには、ツールバーの赤い感嘆符アイコンをクリックします。

図 2 に示す結果に含める`FullContactName`、リスト、 `CompanyName`、 `ContactName`、および`ContactTitle`形式 ldquo; を使用して列`ContactName`(`ContactTitle`, `CompanyName`) .


[![FullContactName 形式 ContactName (部署、CompanyName) を使用します。](working-with-computed-columns-cs/_static/image5.png)](working-with-computed-columns-cs/_static/image4.png)

**図 2**:`FullContactName`形式を使用して`ContactName`(`ContactTitle`、 `CompanyName`) ([フルサイズのイメージを表示するをクリックして](working-with-computed-columns-cs/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>手順 3: 追加、`SuppliersTableAdapter`には、データ アクセス レイヤー

アプリケーションでは、仕入先情報を使用するのには、最初に、DAL に TableAdapter と DataTable を作成する必要があります。 理想的には、これは、行われる前のチュートリアルで確認簡単な手順と同じです。 ただし、計算列の使用には、ディスカッションを示す、いくつかのしわが導入されています。

アドホック SQL ステートメントを使用して、TableAdapter を使用している場合、TableAdapter のメインのクエリに TableAdapter 構成ウィザードを使用して、計算列を単純に含めることができます。 これには、ただし、自動生成`INSERT`と`UPDATE`計算列を含むステートメント。 これらのメソッドのいずれかを実行しようとすると、`SqlException`メッセージ列と共に*ColumnName*は、計算列または UNION 演算子の結果がスローされるため、変更できません。 中に、`INSERT`と`UPDATE`ステートメントは、tableadapter を手動で調整できる`InsertCommand`と`UpdateCommand`プロパティ、こうしたカスタマイズは失われます TableAdapter 構成ウィザードが再実行されるたびにします。

アドホック SQL ステートメントを使用して Tableadapter の脆弱性、原因は、計算列を使用するときにストアド プロシージャを使用をお勧めします。 既存のストアド プロシージャを使用している場合だけ、TableAdapter を構成で説明したように、[を使用して既存のストアド プロシージャを型指定されたデータセットの Tableadapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)チュートリアルです。 ストアド プロシージャは作成、TableAdapter ウィザードがあれば、ただし、これが重要を最初にメインのクエリから計算列を省略する場合です。 メインのクエリで計算列を含める場合、TableAdapter 構成ウィザードは、通知完了すると、対応するストアド プロシージャを作成することはできません。 つまり、最初に計算列を必要としないメイン クエリを使用して、TableAdapter を構成し、対応するストアド プロシージャと、tableadapter を手動で更新する必要があります`SelectCommand`計算列を含めるようにします。 この方法と似ていますで使用される、 [TableAdapter を使用する更新](updating-the-tableadapter-to-use-joins-cs.md)`JOIN`*s*チュートリアルです。

このチュートリアルでは、s 新しい TableAdapter を追加して、それを自動的にご利用の米国ストアド プロシージャの作成を使用できます。 その結果、最初に省略する必要が、`FullContactName`メイン クエリから列を計算します。

開いて開始、`NorthwindWithSprocs`内のデータセット、`~/App_Code/DAL`フォルダーです。 デザイナー内を右クリックし、新しい TableAdapter を追加、コンテキスト メニューから選択します。 これにより、TableAdapter 構成ウィザードが起動します。 データベースからデータをクエリするを指定する (`NORTHWNDConnectionString`から`Web.config`) し、[次へ] をクリックします。 私たちが作成されていないクエリを実行または変更するための任意のストアド プロシージャから、`Suppliers`テーブルで、作成する新しいストアド プロシージャをウィザードが us 用に作成し、[次へ] のオプションを選択します。


[![新しいストアド プロシージャの作成オプションを選択します。](working-with-computed-columns-cs/_static/image8.png)](working-with-computed-columns-cs/_static/image7.png)

**図 3**: 新しいストアド プロシージャの作成オプションを選択 ([フルサイズのイメージを表示するをクリックして](working-with-computed-columns-cs/_static/image9.png))


後続の手順では、メインのクエリに us を求めます。 返す次のクエリを入力して、 `SupplierID`、 `CompanyName`、 `ContactName`、および`ContactTitle`各仕入先の列です。 注このクエリは、計算列を意図的に省略 (`FullContactName`) です。 この列を含める手順 4. で対応するストアド プロシージャを更新します。


[!code-sql[Main](working-with-computed-columns-cs/samples/sample3.sql)]

後、メインのクエリを入力して、次をクリックすると、ウィザードにより、名前を次の 4 つのストアド プロシージャが生成されます。 これらのストアド プロシージャの名前を付けます`Suppliers_Select`、 `Suppliers_Insert`、 `Suppliers_Update`、および`Suppliers_Delete`図 4 に示すようにします。


[![自動生成ストアド プロシージャの名前を変更します。](working-with-computed-columns-cs/_static/image11.png)](working-with-computed-columns-cs/_static/image10.png)

**図 4**: Auto-Generated ストアド プロシージャの名前を変更する ([フルサイズのイメージを表示するをクリックして](working-with-computed-columns-cs/_static/image12.png))


ウィザードの次の手順では、TableAdapter のメソッドの名前し、データにアクセスして更新するために使用するパターンを指定することができます。 すべての 3 つチェック ボックスをオンのままにして、名前を変更、`GetData`メソッドを`GetSuppliers`です。 ウィザードを完了するには、[完了] をクリックします。


[![GetSuppliers に GetData メソッドの名前を変更します。](working-with-computed-columns-cs/_static/image14.png)](working-with-computed-columns-cs/_static/image13.png)

**図 5**: 名前を変更、`GetData`メソッドを`GetSuppliers`([フルサイズのイメージを表示するをクリックして](working-with-computed-columns-cs/_static/image15.png))


[完了] をクリックすると、ウィザードは次の 4 つのストアド プロシージャを作成し、型指定されたデータセットに TableAdapter と対応する DataTable を追加します。

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>手順 4: TableAdapter のメイン クエリで計算列を含む

これで、TableAdapter を更新する必要があり、DataTable の作成に含める手順 3. で、`FullContactName`計算列です。 これには、2 つの手順が含まれます。

1. 更新、`Suppliers_Select`ストアド プロシージャを返す、`FullContactName`計算列、および
2. 更新に含める、対応する DataTable`FullContactName`列です。

サーバー エクスプ ローラーに移動して Stored Procedures フォルダーへのドリル ダウンを開始します。 開く、`Suppliers_Select`ストアド プロシージャと更新プログラム、`SELECT`クエリに含まれる、`FullContactName`計算列。


[!code-sql[Main](working-with-computed-columns-cs/samples/sample4.sql)]

ツールバーの 保存 アイコンをクリックして、Ctrl + S をクリックして、または保存を選択して、ストアド プロシージャに変更を保存`Suppliers_Select`ファイル メニュー オプション。

次に、データセット デザイナーに戻りを右クリックし、 `SuppliersTableAdapter`、コンテキスト メニューから構成を選択します。 なお、`Suppliers_Select`列が含まれています、`FullContactName`そのデータ列のコレクション内の列です。


[![DataTable の列を更新するには、TableAdapter の構成ウィザードを実行します。](working-with-computed-columns-cs/_static/image17.png)](working-with-computed-columns-cs/_static/image16.png)

**図 6**: tableadapter の DataTable の列を更新するための構成ウィザードを実行 ([フルサイズのイメージを表示するをクリックして](working-with-computed-columns-cs/_static/image18.png))


ウィザードを完了するには、[完了] をクリックします。 これが自動的に対応する列を追加、`SuppliersDataTable`です。 TableAdapter ウィザードでは、ことを検出するのに十分なスマート、`FullContactName`列は計算列および読み取り専用です。 その結果、その列に、設定 s`ReadOnly`プロパティを`true`です。 これを確認するから列を選択、`SuppliersDataTable`し、[プロパティ] ウィンドウに移動 (図 7 を参照してください)。 なお、`FullContactName`列 s`DataType`と`MaxLength`プロパティはそれに応じても設定します。


[![FullContactName 列が読み取り専用としてマークされています。](working-with-computed-columns-cs/_static/image20.png)](working-with-computed-columns-cs/_static/image19.png)

**図 7**:`FullContactName`列は読み取り専用とマークされています ([フルサイズのイメージを表示するをクリックして](working-with-computed-columns-cs/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>手順 5: 追加、`GetSupplierBySupplierID`TableAdapter にメソッド

このチュートリアルでは更新可能なグリッドで、仕入先を表示するための ASP.NET ページを作成します。 過去のチュートリアルを更新したビジネス ロジック層から 1 つのレコードを厳密に型指定された DataTable として、そのプロパティを更新し、更新された DataTable を送信 DAL から特定のレコードが、DAL に変更を反映するバックアップを取得することでデータベースです。 -DAL から更新されているレコードを取得して、この最初の手順を実行する必要があります最初に追加、 `GetSupplierBySupplierID(supplierID)` DAL にメソッドです。

右クリックし、`SuppliersTableAdapter`データセット デザインで、コンテキスト メニューから、クエリの追加オプションを選択します。 手順 3. で行ったようにご利用の米国を新しいストアド プロシージャの作成 オプションを選択すると、新しいストアド プロシージャを生成するウィザードを使用できます (戻って図 3 にこの手順のスクリーン ショットの)。 このメソッドは、複数の列を持つレコードを返しますが、以降を行を返す SELECT は、SQL クエリを使用し、[次へ] をクリックすることを示します。


[![選択 オプションの行が返されます](working-with-computed-columns-cs/_static/image23.png)](working-with-computed-columns-cs/_static/image22.png)

**図 8**: 選択オプションの行が返されます ([フルサイズのイメージを表示するをクリックして](working-with-computed-columns-cs/_static/image24.png))


後続の手順では、このメソッドを使用するクエリの場合、us ように求められます。 特定仕入先が、メインのクエリは、同じデータ フィールドを返すと、次を入力します。


[!code-sql[Main](working-with-computed-columns-cs/samples/sample5.sql)]

次の画面では、自動生成されるストアド プロシージャの名前を付けることが求められます。 このストアド プロシージャの名前を付けます`Suppliers_SelectBySupplierID`[次へ] をクリックします。


[![名前のストアド プロシージャ Suppliers_SelectBySupplierID](working-with-computed-columns-cs/_static/image26.png)](working-with-computed-columns-cs/_static/image25.png)

**図 9**: ストアド プロシージャの名前を付けます`Suppliers_SelectBySupplierID`([フルサイズのイメージを表示するをクリックして](working-with-computed-columns-cs/_static/image27.png))


最後に、ウィザードのプロンプト パターンと、TableAdapter を使用するメソッドの名前へのアクセス、データの場合、us です。 両方のチェック ボックスがオンのままにして、名前の変更、`FillBy`と`GetDataBy`メソッド`FillBySupplierID`と`GetSupplierBySupplierID`、それぞれします。


[![名前の TableAdapter メソッド FillBySupplierID と GetSupplierBySupplierID](working-with-computed-columns-cs/_static/image29.png)](working-with-computed-columns-cs/_static/image28.png)

**図 10**: TableAdapter のメソッドの名前を付けます`FillBySupplierID`と`GetSupplierBySupplierID`([フルサイズのイメージを表示するをクリックして](working-with-computed-columns-cs/_static/image30.png))


ウィザードを完了するには、[完了] をクリックします。

## <a name="step-6-creating-the-business-logic-layer"></a>手順 6: ビジネス ロジック層を作成します。

手順 1. で作成された計算列を使用する ASP.NET ページを作成する前に、BLL に対応するメソッドを追加する必要があります。 ASP.NET ページ、手順 7. で作成する、により、ユーザーを表示して、suppliers を編集できます。 そのため、当社 BLL に少なくとも、仕入先との別を特定のサプライヤーの更新すべてを取得する方法を提供する必要があります。

という新しいクラス ファイルを作成する`SuppliersBLLWithSprocs`で、`~/App_Code/BLL`フォルダーし、次のコードを追加します。


[!code-csharp[Main](working-with-computed-columns-cs/samples/sample6.cs)]

などの他の BLL クラス`SuppliersBLLWithSprocs`が、 `protected` `Adapter`プロパティのインスタンスを返す、`SuppliersTableAdapter`と共に 2 つのクラス`public`メソッド:`GetSuppliers`と`UpdateSupplier`です。 `GetSuppliers`メソッドの呼び出しを返します、 `SuppliersDataTable` 、対応するによって返される`GetSupplier`データ アクセス層内のメソッドです。 `UpdateSupplier`メソッドは、DAL s への呼び出しによって更新されている特定の仕入先に関する情報を取得`GetSupplierBySupplierID(supplierID)`メソッドです。 更新、 `CategoryName`、 `ContactName`、および`ContactTitle`プロパティ データ アクセス層 s を呼び出すことによって、これらの変更をデータベースにコミットし、`Update`メソッドで変更された`SuppliersRow`オブジェクト。

> [!NOTE]
> 除く`SupplierID`と`CompanyName`、Suppliers テーブルのすべての列を許可する`NULL`値。 そのため場合は、渡された`contactName`または`contactTitle`パラメーターは`null`、対応するを設定する必要があります`ContactName`と`ContactTitle`プロパティを`NULL`データベース値を使用して、`SetContactNameNull`と`SetContactTitleNull`メソッド、それぞれします。


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>手順 7: プレゼンテーション層から、計算列の使用

追加された計算列で、`Suppliers`テーブル DAL および BLL が適宜更新で動作する ASP.NET ページを作成する準備が整いました、`FullContactName`計算列です。 開いて開始、 `ComputedColumns.aspx`  ページで、`AdvancedDAL`フォルダーと、ツールボックスからデザイナーにドラッグする GridView。 集合 GridView s`ID`プロパティを`Suppliers`と、という名前の新しい ObjectDataSource へのバインドのスマート タグから`SuppliersDataSource`です。 構成を使用する ObjectDataSource、`SuppliersBLLWithSprocs`クラスを追加しましたは手順 6. でバックアップし、[次へ] をクリックします。


[![構成、ObjectDataSource SuppliersBLLWithSprocs クラスを使用するには](working-with-computed-columns-cs/_static/image32.png)](working-with-computed-columns-cs/_static/image31.png)

**図 11**: 構成を使用する ObjectDataSource、`SuppliersBLLWithSprocs`クラス ([フルサイズのイメージを表示するをクリックして](working-with-computed-columns-cs/_static/image33.png))


定義された 2 つのメソッドがある、`SuppliersBLLWithSprocs`クラス:`GetSuppliers`と`UpdateSupplier`です。 ことを確認これら 2 つの方法の選択で指定されたそれぞれのタブを更新、ObjectDataSource の構成を完了するには、[完了] をクリックします。

完了すると、データ ソース構成ウィザードの Visual Studio は、返されるデータ フィールドの各、BoundField を追加します。 削除、 `SupplierID` BoundField を変更して、`HeaderText`のプロパティ、 `CompanyName`、 `ContactName`、 `ContactTitle`、および`FullContactName`BoundFields 会社、連絡先の名前、タイトル、および完全の連絡先名をそれぞれします。 スマート タグから、編集を有効にするチェック ボックスをオン GridView s 組み込みの編集機能を有効にします。

BoundFields GridView に追加するだけでなく、データ ソース ウィザードの完了もが ObjectDataSource s を設定する Visual Studio`OldValuesParameterFormatString`プロパティを元に\_{0}。 この設定を既定値、{0} に戻されます。

ObjectDataSource、GridView にこれらの編集を行った後、宣言型マークアップを次のようになります。


[!code-aspx[Main](working-with-computed-columns-cs/samples/sample7.aspx)]

次に、ブラウザーからこのページを参照してください。 各仕入先を含むグリッドに表示されている図 12 に示す、`FullContactName`として書式設定された列、値があるその他の 3 つの列を連結したものでは単に`ContactName`(`ContactTitle`、 `CompanyName`)。


[![各仕入先がグリッドに一覧表示します。](working-with-computed-columns-cs/_static/image35.png)](working-with-computed-columns-cs/_static/image34.png)

**図 12**: 各仕入先がグリッドに表示されている ([フルサイズのイメージを表示するをクリックして](working-with-computed-columns-cs/_static/image36.png))


特定供給業者がポストバックを発生させるしがその行で表示の [編集] ボタンをクリックすると、編集インターフェイスの (図 13 を参照してください)。 最初の 3 つの列の表示で、既定のインターフェイスを編集 - いるテキスト ボックス コントロール`Text`プロパティがデータ フィールドの値に設定します。 `FullContactName`列、依然、テキストとして。 データ ソース構成ウィザードの完了時に GridView に、BoundFields が追加されたときに、 `FullContactName` BoundField s`ReadOnly`プロパティに設定された`true`ため、対応する`FullContactName`内の列、 `SuppliersDataTable`その`ReadOnly`プロパティに設定`true`です。 手順 4 で説明したとおり、 `FullContactName` s`ReadOnly`プロパティに設定された`true`TableAdapter の列が計算列が検出されたためです。


[![FullContactName 列が編集できません。](working-with-computed-columns-cs/_static/image38.png)](working-with-computed-columns-cs/_static/image37.png)

**図 13**:`FullContactName`列が編集できない ([フルサイズのイメージを表示するをクリックして](working-with-computed-columns-cs/_static/image39.png))


1 つまたは複数の編集可能な列の値を更新してください [更新] をクリックします。 注方法、`FullContactName`の値が変更を反映するように自動的に更新します。

> [!NOTE]
> GridView 現在使用して BoundFields 編集可能なフィールドの結果として既定のインターフェイスを編集します。 以降、`CompanyName`フィールドは必須では、RequiredFieldValidator を含む TemplateField に変換する必要があります。 I のままに練習として、興味のリーダーの。 参照してください、[検証コントロールを編集および挿入するインターフェイスを追加する](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)BoundField を TemplateField に変換し、検証コントロールを追加する手順についてのチュートリアルです。


## <a name="summary"></a>まとめ

テーブルのスキーマを定義するときに Microsoft SQL Server によって計算列を含めることができます。 これらは、値を持つは、通常、同じのレコードの他の列から値を参照する式から計算列です。 以降の値の計算列は、式に基づいて、読み取り専用、内の値を割り当てることができません、`INSERT`または`UPDATE`ステートメントです。 対応するを自動的に生成しようとする TableAdapter のメイン クエリで計算列を使用する場合の課題が生じます`INSERT`、 `UPDATE`、および`DELETE`ステートメントです。

このチュートリアルでは、計算列によって引き起こされる問題を回避する方法について説明します。 具体的には、使用してストアド プロシージャ、TableAdapter のアドホック SQL ステートメントを使用して Tableadapter に特有の脆弱性を解消しました。 ストアド プロシージャを TableAdapter ウィザードを新規作成するときは、最初に生成されないデータの変更ストアド プロシージャが含まれているのため、計算列を省略するメインのクエリがあることが重要です。 TableAdapter が最初に構成された後にその`SelectCommand`ストアド プロシージャは、計算列を含める retooled ことができます。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Hilton Geisenow および Teresa マーフィーがいました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](adding-additional-datatable-columns-cs.md)
> [次へ](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
