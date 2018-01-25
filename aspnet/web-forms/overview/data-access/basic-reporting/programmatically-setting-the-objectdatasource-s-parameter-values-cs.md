---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
title: "ObjectDataSource のパラメーターの値 (c#) をプログラムによって設定 |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルでは、DAL および BLL を単一の入力パラメーターを受け入れてデータを返すメソッドを追加するのに紹介します。 例は、このパラメーターを設定しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 1c4588bb-255d-4088-b319-5208da756f4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
msc.type: authoredcontent
ms.openlocfilehash: 7694c56fa5c50ff75db931e88c2334f560631d74
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-c"></a>プログラムによって設定 ObjectDataSource のパラメーターの値 (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_6_CS.exe)または[PDF のダウンロード](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/datatutorial06cs1.pdf)

> このチュートリアルでは、DAL および BLL を単一の入力パラメーターを受け入れてデータを返すメソッドを追加するのに紹介します。 例では、このパラメーターをプログラムで設定されます。


## <a name="introduction"></a>はじめに

説明したとおり、[前のチュートリアル](declarative-parameters-cs.md)、宣言によって ObjectDataSource のメソッドにパラメーター値を渡すためのオプションが多数利用できます。 ページで、Web コントロールに由来または読み取り可能なデータ ソースによっては、その他のソースでは、パラメーターの値がハード コーディングされた場合は、`Parameter`オブジェクト、たとえば、コードの行を記述することがなく値が入力パラメーターにバインドできます。

ただし、組み込みのデータ ソースのいずれかによってについて考慮されていないソースからパラメーター値を取得する場合がある可能性があります`Parameter`オブジェクト。 サイトには、ユーザー アカウントがサポートされている場合に、現在ログインしている訪問者のユーザー ID に基づくパラメーターを設定することがあります。 または、に沿って ObjectDataSource の基になるオブジェクトのメソッドに送信する前に、パラメーターの値をカスタマイズする必要があります。

たびに ObjectDataSource の`Select`メソッドが呼び出される、ObjectDataSource を最初に発生させるその[を選択するとイベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx)です。 ObjectDataSource の基になるオブジェクトのメソッドは、呼び出されます。 完了し、ObjectDataSource[選択したイベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx)(図 1 は、このイベントのシーケンスを示しています) に発生します。 ObjectDataSource の基になるオブジェクトのメソッドに渡されたパラメーター値を設定またはのイベント ハンドラーでカスタマイズできる、`Selecting`イベント。


[![ObjectDataSource の選択とを選択するとイベント起動する前に、後の基になるオブジェクトのメソッドが呼び出されます](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image1.png)

**図 1**:、ObjectDataSource`Selected`と`Selecting`イベントの発生前に、と後の基になるオブジェクトのメソッドが呼び出されます ([フルサイズのイメージを表示するをクリックして](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image3.png))


このチュートリアルで見ていきます、DAL と 1 つの入力パラメーターを受け入れる BLL にメソッドを追加する`Month`、型の`int`を返します、`EmployeesDataTable`オブジェクトを指定したその雇用日を持つそれらの従業員が設定されます`Month`. この例ではプログラムによって「従業員の記念日今月」の一覧を示す現在の月に基づくこのパラメーターを設定します。

開始しましょう!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>手順 1: 追加する方法`EmployeesTableAdapter`

それらの従業員を取得するための手段を追加する必要があります最初の例の持つ`HireDate`で指定した月が発生しました。 最初のメソッドを作成する必要があります、アーキテクチャに従ってこの機能を提供する`EmployeesTableAdapter`適切な SQL ステートメントに対応します。 これを実現するには、Northwind の型指定されたデータセットを開くことによって開始します。 右クリックし、`EmployeesTableAdapter`にラベルを付けるし、クエリの追加を選択します。


[![新しいクエリ、EmployeesTableAdapter を追加します。](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image4.png)

**図 2**: 新しいクエリを追加、 `EmployeesTableAdapter` ([フルサイズのイメージを表示するをクリックして](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image6.png))


選択すると、行を返す SQL ステートメントを追加します。 指定に達すると、`SELECT`ステートメントは、既定値を画面`SELECT`のステートメント、`EmployeesTableAdapter`は既に読み込まれます。 単に追加、`WHERE`句:`WHERE DATEPART(m, HireDate) = @Month`です。 [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) T-SQL 関数の特定の日付部分を返しますです、`datetime`入力です。 ここではを使用して`DATEPART`の月を返す、`HireDate`列です。


[![戻り値のみもの行が、HireDate 列が以下に、@HiredBeforeDateパラメーター](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image7.png)

**図 3**: 返すのみもの行が、`HireDate`列は、以下に、`@HiredBeforeDate`パラメーター ([フルサイズのイメージを表示するをクリックして](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image9.png))


最後に、変更、`FillBy`と`GetDataBy`メソッドの名前を`FillByHiredDateMonth`と`GetEmployeesByHiredDateMonth`、それぞれします。


[![FillBy と GetDataBy より適切なメソッド名を選択します。](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image10.png)

**図 4**: 選択より適切なメソッド名よりも`FillBy`と`GetDataBy`([フルサイズのイメージを表示するをクリックして](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image12.png))


ウィザードを完了し、データセットのデザイン画面に戻るには、[完了] をクリックします。 `EmployeesTableAdapter`指定された月で採用された従業員にアクセスするためのメソッドの新しいセットを含める必要がありますようになりました。


[![メソッドを新しいデータセットのデザイン画面に表示します。](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image13.png)

**図 5**: 新しいメソッドに表示データセットのデザイン サーフェイス ([フルサイズのイメージを表示するをクリックして](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>手順 2: 追加、`GetEmployeesByHiredDateMonth(month)`ビジネス ロジック層メソッド

ロジックをアプリケーション アーキテクチャで使用する別のレイヤーのビジネス ロジックとデータにアクセスするために指定した日付より前に採用された従業員を取得するまで、DAL の呼び出しを BLL メソッドを追加する必要があります。 開く、`EmployeesBLL.cs`ファイルし、次のメソッドを追加します。


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample1.cs)]

このクラスで、他のメソッドと同様`GetEmployeesByHiredDateMonth(month)`だけで、DAL 呼び出し、結果を返します。

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>手順 3: 社員が雇用記念日はこの月を表示します。

この例の最後の手順では、それらの従業員が雇用記念日はこの月を表示します。 GridView を追加して、開始、 `ProgrammaticParams.aspx`  ページで、`BasicReporting`フォルダーし、そのデータ ソースとして新しい ObjectDataSource を追加します。 構成を使用する ObjectDataSource、`EmployeesBLL`クラス、 `SelectMethod` 'éý'`GetEmployeesByHiredDateMonth(month)`です。


[![EmployeesBLL クラスを使用します。](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image16.png)

**図 6**: を使用して、`EmployeesBLL`クラス ([フルサイズのイメージを表示するをクリックして](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image18.png))


[![GetEmployeesByHiredDateMonth(month) から選択メソッド](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image19.png)

**図 7**: Select From、`GetEmployeesByHiredDateMonth(month)`メソッド ([フルサイズのイメージを表示するをクリックして](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image21.png))


最終画面には、提供するよう求められます、`month`パラメーター値のソース。 この値プログラムで設定します、パラメーターのソースが なし、既定値に設定のままにしてオプションし、ため完了 をクリックできます。


[![[なし] にパラメーターのソースの設定のままにしてください。](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image22.png)

**図 8**: なし にパラメーターのソースの設定のままにして ([フルサイズのイメージを表示するをクリックして](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image24.png))


これが作成されます、 `Parameter` objectdatasource のオブジェクト`SelectParameters`値が指定されていないコレクションです。


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample2.aspx)]

この値をプログラムで設定する必要があります、ObjectDataSource のイベント ハンドラーを作成する`Selecting`イベント。 これを実現するには、デザイン ビューに移動し、ObjectDataSource をダブルクリックします。 または、ObjectDataSource を選択、[プロパティ] ウィンドウに移動し、稲妻のアイコンをクリックします。 次に、いずれかをダブルクリックしてテキスト ボックスの横に、`Selecting`イベントまたは使用する場合、イベント ハンドラーの名前を入力します。


![Web コントロールのイベントを一覧表示する [プロパティ] ウィンドウで表示される稲妻アイコンをクリックします。](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image25.png)

**図 9**: Web コントロールのイベントを一覧表示する [プロパティ] ウィンドウで表示される稲妻アイコンをクリックして


両方の方法では、新しいイベント ハンドラーを追加、ObjectDataSource の`Selecting`ページの分離コード クラスをイベント。 このイベント ハンドラー内には、読み取りし、書き込みを使用してパラメーターの値おできます`e.InputParameters[parameterName]`ここで、  *`parameterName`* の値は、`Name`属性、`<asp:Parameter>`タグ (、`InputParameters`コレクションすることもできます序数に基づく、としてのインデックス付き`e.InputParameters[index]`)。 設定する、`month`現在の月にパラメーターを追加するには、次の`Selecting`イベントのハンドラー。


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample3.cs)]

ブラウザーからこのページにアクセスしたときがわかりますその 1 つだけの従業員には、今月 (年 3 月) が採用された山久吉田一佳、1994 年より会社とされました。


[![これらの従業員の記念日今月が表示されます。](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image26.png)

**図 10**: これらの従業員を記念日この月が表示されます ([フルサイズのイメージを表示するをクリックして](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image28.png))


## <a name="summary"></a>まとめ

ObjectDataSource のパラメーターの通常設定できる値は、宣言によって、コードの行を必要とせずは、簡単にプログラムでパラメーターの値を設定します。 ObjectDataSource のためのイベント ハンドラーの作成が必要なことを行うには`Selecting`基になるオブジェクトのメソッドが呼び出され、経由で 1 つまたは複数のパラメーターの値を手動で設定する前に発生するイベント、`InputParameters`コレクション。

このチュートリアルでは、基本レポートのセクションで終了します。 [次のチュートリアル](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)が開始されると、フィルタ リングとマスター/詳細シナリオ セクションをうまくビジター データをフィルター処理を許可するための手法を見てし、マスター レポートから詳細レポートにドリル ダウンします。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Hilton Giesenow しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](declarative-parameters-cs.md)
[次へ](displaying-data-with-the-objectdatasource-vb.md)
