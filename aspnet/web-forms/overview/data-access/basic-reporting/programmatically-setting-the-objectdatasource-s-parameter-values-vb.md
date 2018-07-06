---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: ObjectDataSource のパラメーター値 (VB) をプログラムによって設定 |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、DAL および BLL を 1 つの入力パラメーターを受け取り、データを返すメソッドを追加で紹介します。 例は、このパラメーターを設定しています.
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: d779de4f5bd0d03f413237689e5a64330fcb491d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825783"
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>ObjectDataSource のパラメーター値 (VB) をプログラムで設定します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe)または[PDF のダウンロード](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> このチュートリアルでは、DAL および BLL を 1 つの入力パラメーターを受け取り、データを返すメソッドを追加で紹介します。 例は、このパラメーターをプログラムによって設定されます。


## <a name="introduction"></a>はじめに

説明したように、[前のチュートリアル](declarative-parameters-vb.md)さまざまなオプションは宣言によって ObjectDataSource のメソッドにパラメーター値を渡すために使用できます。 ページで、Web コントロールから取得ハード コーディングされたパラメーター値の場合、またはデータ ソースによって読み取り可能なその他のソースでは、`Parameter`オブジェクトなど、コードを記述しなくても値が入力パラメーターにバインドできます。

ただし、組み込みのデータ ソースのいずれかで考慮されていないソースからパラメーター値を取得する場合がある可能性があります`Parameter`オブジェクト。 私たちのサイトには、ユーザー アカウントがサポートされている場合、現在ログインしている訪問者のユーザー ID に基づくパラメーターを設定する必要があります。 または、に沿って ObjectDataSource の基になるオブジェクトのメソッドに送信する前に、パラメーターの値をカスタマイズする必要があります。

たびに ObjectDataSource の`Select`メソッドが呼び出される、ObjectDataSource がその[するイベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx)します。 ObjectDataSource の基になるオブジェクトのメソッドが呼び出されます。 ObjectDataSource の完了後[選択したイベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx)(図 1 は、このイベントのシーケンスを示しています) が起動します。 ObjectDataSource の基になるオブジェクトのメソッドに渡されたパラメーター値を設定またはのイベント ハンドラーでカスタマイズできる、`Selecting`イベント。


[![ObjectDataSource の選択と選択イベント起動する前に、後の基になるオブジェクトのメソッドが呼び出されます](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**図 1**:、ObjectDataSource の`Selected`と`Selecting`イベントの発生前に、と後の基になるオブジェクトのメソッドが呼び出されます ([フルサイズの画像を表示する をクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))。


このチュートリアルでは、DAL と 1 つの入力パラメーターを受け取る BLL にメソッドを追加することに注目します`Month`、型の`Integer`を返します、`EmployeesDataTable`雇用、記念日を指定したがある従業員を使用して作成されたオブジェクト`Month`. この例ではプログラムで「従業員の記念日今月」の一覧を表示、現在の月に基づくこのパラメーターを設定します。

それでは、始めましょう!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>手順 1: 追加する方法`EmployeesTableAdapter`

これらの従業員を取得するための手段を追加する必要があります最初の例の持つ`HireDate`で指定した月が発生しました。 最初のメソッドを作成する必要があります、アーキテクチャに従ってこの機能を提供する`EmployeesTableAdapter`適切な SQL ステートメントに対応します。 これを実現するには、Northwind の型指定されたデータセットを開くことで開始します。 右クリックし、`EmployeesTableAdapter`ラベルし、クエリの追加 を選択します。


[![新しいクエリ、EmployeesTableAdapter を追加します。](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**図 2**: 新しいクエリを追加、 `EmployeesTableAdapter` ([フルサイズの画像を表示する をクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))。


行を返す SQL ステートメントを追加することもできます。 指定に達すると、`SELECT`ステートメントは、既定値を画面`SELECT`のステートメント、`EmployeesTableAdapter`は既に読み込まれます。 追加するだけです、`WHERE`句:`WHERE DATEPART(m, HireDate) = @Month`します。 [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) T-SQL 関数の特定の日付部分を返しますです、`datetime`型。 この場合、使用している`DATEPART`の月を返す、`HireDate`列。


[![戻り値、行が、HireDate 列のみが未満かと等しい、@HiredBeforeDateパラメーター](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**図 3**: 返すのみこれらの行が、`HireDate`列は、以下に、`@HiredBeforeDate`パラメーター ([フルサイズの画像を表示する をクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))。


最後に、変更、`FillBy`と`GetDataBy`メソッド名をする`FillByHiredDateMonth`と`GetEmployeesByHiredDateMonth`、それぞれします。


[![FillBy と GetDataBy よりもより適切なメソッド名を選択します。](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**図 4**: 選択より適切なメソッド名よりも`FillBy`と`GetDataBy`([フルサイズの画像を表示する をクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))。


ウィザードを完了し、データセットのデザイン画面に戻るには、[完了] をクリックします。 `EmployeesTableAdapter`従業員を雇用すると、指定した 1 か月にアクセスするためのメソッドの新しいセットを含める必要がありますようになりました。


[![新しいメソッド、データセットのデザイン画面に表示します。](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**図 5**: [新しいメソッドが表示されます、データセットのデザイン画面で ([フルサイズの画像を表示する] をクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))。


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>手順 2: 追加、`GetEmployeesByHiredDateMonth(month)`メソッドをビジネス ロジック層

ロジックをアプリケーション アーキテクチャは、別のレイヤーのビジネス ロジックとデータにアクセスするために指定した日付より前に採用された従業員を取得するまで、DAL の呼び出しを BLL メソッドを追加する必要があります。 開く、`EmployeesBLL.vb`ファイルを開き、次のメソッドを追加します。


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

このクラスで、その他のメソッドと同様`GetEmployeesByHiredDateMonth(month)`DAL 呼び出すだけですし、結果を返します。

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>手順 3: 従業員の雇用の記念日はこの月を表示します。

この例の最後の手順では、従業員の雇用の記念日はこの月を表示します。 GridView を追加することで開始、`ProgrammaticParams.aspx`ページで、`BasicReporting`フォルダーとそのデータ ソースとして新しい ObjectDataSource を追加します。 構成を使用する ObjectDataSource、`EmployeesBLL`クラス、`SelectMethod`に設定`GetEmployeesByHiredDateMonth(month)`します。


[![EmployeesBLL クラスを使用して、](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**図 6**: 使用して、`EmployeesBLL`クラス ([フルサイズの画像を表示する をクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))。


[![The GetEmployeesByHiredDateMonth(month) から選択メソッド](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**図 7**: Select From、`GetEmployeesByHiredDateMonth(month)`メソッド ([フルサイズの画像を表示する をクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))。


最後の画面には、提供するよう求められます、`month`パラメーターの値のソース。 この値をプログラムで設定しますがため、パラメーター ソースが [なし]、既定値に設定したままにはオプションし、[完了] をクリックします。


[![[なし] に、パラメーター ソースの設定をそのまま使用します。](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**図 8**: [なし] にパラメーターのソースの設定のままに ([フルサイズの画像を表示する をクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))。


これが作成されます、`Parameter`オブジェクトで ObjectDataSource の`SelectParameters`値が指定されていないコレクション。


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

この値をプログラムで設定する必要がありますの ObjectDataSource のイベント ハンドラーを作成する`Selecting`イベント。 これを実現するには、デザイン ビューに移動し、ObjectDataSource をダブルクリックします。 または、ObjectDataSource を選択、[プロパティ] ウィンドウに移動し、稲妻のアイコンをクリックします。 次をダブルクリックするか、テキスト ボックスの横に、`Selecting`イベントまたはイベント ハンドラーを使用する名前を入力します。 ObjectDataSource を選択して、イベント ハンドラーを作成するには 3 つ目のオプションとして、その`Selecting`ページの分離コード クラスの上部にある 2 つのドロップダウン リストからイベント。


![Web コントロールのイベントを一覧表示する [プロパティ] ウィンドウで稲妻のアイコンをクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**図 9**: Web コントロールのイベントを一覧表示する [プロパティ] ウィンドウで稲妻のアイコンをクリックします


すべての 3 つのアプローチでは、新しいイベント ハンドラーを追加の ObjectDataSource の`Selecting`ページの分離コード クラスにイベント。 このイベント ハンドラーには、読み取りし、書き込みを使用してパラメーター値できます`e.InputParameters(parameterName)`ここで、 *`parameterName`* の値である、`Name`属性、`<asp:Parameter>`タグ (、`InputParameters`コレクションこともできます序数、としてのインデックス付き`e.InputParameters(index)`)。 設定する、`month`パラメーター、現在の月を追加するには、次の`Selecting`イベント ハンドラー。


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

ブラウザーからこのページにアクセスすると、1 つだけの従業員が採用された今月 (3 月) 確認できます Laura Callahan、1994 年以来勤務しています。


[![これらの従業員を記念日今月が表示されます。](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**図 10**: これらの従業員を記念日この 1 か月が表示されます ([フルサイズの画像を表示する をクリックします](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))。


## <a name="summary"></a>まとめ

ObjectDataSource のパラメーターの設定できる値は通常ここでは、行のコードを必要とせずに、パラメーター値をプログラムで設定する簡単です。 ObjectDataSource のイベント ハンドラーを作成を行う必要がありますすべてが`Selecting`、基になるオブジェクトのメソッドが呼び出され、使用して、1 つまたは複数のパラメーターの値を手動で設定する前に発生するイベント、`InputParameters`コレクション。

このチュートリアルでは、基本レポートのセクションで終了します。 [次のチュートリアル](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md)マスター詳細シナリオのフィルター処理を私たちがデータをフィルター処理の訪問者を許可するための手法を見てとセクション マスター レポートから詳細レポートにドリル ダウンを開始します。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者が、Hilton Giesenow です。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](declarative-parameters-vb.md)
