---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
title: データ アクセス層の接続とコマンド レベルの設定 (c#) の構成 |Microsoft Docs
author: rick-anderson
description: 型指定されたデータセット内で Tableadapter に自動的に自動的にデータベースへの接続のコマンドを発行および結果を含む DataTable を作成しています.
ms.author: riande
ms.date: 08/03/2007
ms.assetid: cd330dd9-6254-4305-9351-dd727384c83b
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
msc.type: authoredcontent
ms.openlocfilehash: 142c8e93422ac03d2f2205b6635f88b982b4c9e2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837405"
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-c"></a>データ アクセス層の接続とコマンド レベルの設定 (c#) を構成します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_CS.zip)または[PDF のダウンロード](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/datatutorial72cs1.pdf)

> 型指定されたデータセット内で Tableadapter に自動的に自動的にデータベースへの接続のコマンドを発行および結果を含む DataTable を作成します。 機会があるが、自分たちには、これらの詳細と、このチュートリアルで処理する場合、TableAdapter でデータベース接続とコマンド レベルの設定にアクセスする方法を学習します。


## <a name="introduction"></a>はじめに

チュートリアル シリーズ全体を使用して型指定されたデータセットのデータ アクセス層とビジネス オブジェクトの階層型アーキテクチャの実装します。 説明したように、[最初のチュートリアル](../introduction/creating-a-data-access-layer-cs.md)、型指定されたデータセット s Datatable は、Tableadapter が取得および基になるデータを変更するデータベースと通信するラッパーとして機能はデータのリポジトリとして機能します。 Tableadapter では、データベースの使用に伴う複雑さをカプセル化し、データベースへの接続、コマンドを発行または DataTable に結果を設定するコードを記述することから私たちを保存します。

ただし、TableAdapter の細部まで詳しく burrow し、ADO.NET オブジェクトを直接操作するコードを記述する場合があります。 [トランザクション内のデータベース変更のラッピング](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md)チュートリアルでは、開始、コミット、および ADO.NET トランザクションのロールバックの TableAdapter にメソッドを追加しましたなど。 これらのメソッドは、内部で使用される手動で作成された`SqlTransaction`、tableadapter に割り当てられたオブジェクト`SqlCommand`オブジェクト。

このチュートリアルでは、TableAdapter でデータベース接続とコマンド レベルの設定にアクセスする方法を説明します。 具体的には、機能には追加、`ProductsTableAdapter`できますが、基になる接続文字列へのアクセスし、コマンドのタイムアウト設定します。

## <a name="working-with-data-using-adonet"></a>ADO.NET を使用してデータの使用

Microsoft .NET Framework には、具体的には、データを操作するよう設計されたクラスの多くが含まれています。 これらのクラスは、見つかった、 [ `System.Data`名前空間](https://msdn.microsoft.com/library/system.data.aspx)と呼ばれますが、 *ADO.NET*クラス。 特定に関連付けられている一部の ADO.NET の傘下クラス*データ プロバイダー*します。 データ プロバイダーについては、ADO.NET のクラスと、基になるデータ ストア間でフローを許可する通信チャネルとして考えることができます。 OleDb、ODBC、および特定のデータベース システム用に特別に設計されたプロバイダーなどの汎用化されたプロバイダーがあります。 たとえば、OleDb プロバイダーを使用して Microsoft SQL Server データベースに接続することはできますが、SqlClient プロバイダーははるかに効率的に設計されていますが、具体的には SQL Server 用に最適化されました。

データにプログラムでアクセスすると、次のパターンは、通常使用されます。

- データベースへの接続を確立します。
- コマンドを発行します。
- `SELECT`クエリ、結果のレコードを処理します。

これらの各手順を実行するための別の ADO.NET クラスがあります。 たとえば、SqlClient プロバイダーを使用してデータベースに接続するを使用して、 [ `SqlConnection`クラス](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx)します。 問題を`INSERT`、 `UPDATE`、 `DELETE`、または`SELECT`コマンドを使用して、データベース、 [ `SqlCommand`クラス](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx)します。

を除き、[トランザクション内のデータベース変更のラッピング](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md)チュートリアルではなかった Tableadapter の自動生成されたコードに必要な機能が含まれていますので、自分たち低レベルの ADO.NET コードを作成するにはデータベースへの接続、コマンドを発行、データを取得および Datatable にデータを設定します。 ただし、これらの低レベルの設定をカスタマイズする必要がありますともあります。 次のいくつかの手順では、Tableadapter によって内部的に使用する ADO.NET オブジェクトを利用する方法を説明します。

## <a name="step-1-examining-with-the-connection-property"></a>手順 1: は接続プロパティとチェック

各 TableAdapter クラスには、`Connection`データベース接続情報を指定するプロパティ。 このプロパティのデータ型と`ConnectionString`値は、TableAdapter 構成ウィザードで選択したオプションによって決まります。 最初に型指定されたデータセットに TableAdapter を追加するときこのウィザード求められたとき、データベースのことを思い出してください。 ソース (図 1 参照)。 この最初の手順でドロップダウン リストには、サーバー エクスプ ローラーのデータ接続で他のすべてのデータベースと同様に、構成ファイルで指定されたデータベースにそれらが含まれています。 ドロップダウン リストで使用するデータベースが存在しない場合は、新しい接続 ボタンをクリックし、必要な接続情報を提供する新しいデータベース接続を指定できます。


[![TableAdapter 構成ウィザードの最初のステップ](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image1.png)

**図 1**: TableAdapter 構成ウィザードの最初の手順を ([フルサイズの画像を表示する をクリックします](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image3.png))。


Let s は、TableAdapter のコードを検査する少し`Connection`プロパティ。 説明したとおり、[データ アクセス層を作成する](../introduction/creating-a-data-access-layer-cs.md)チュートリアルでは、クラス ビュー ウィンドウに、適切なクラスの詳細を表示、メンバー名をダブルクリックし、TableAdapter の自動生成されたコードを表示できます。

表示 メニューに移動し、クラス ビューを選択して (または Ctrl + Shift + C を入力して)、クラス ビュー ウィンドウに移動します。 クラス ビュー ウィンドウの上部からにドリルダウンします`NorthwindTableAdapters`名前空間を選択し、`ProductsTableAdapter`クラス。 これが表示されます、`ProductsTableAdapter`のメンバー下には、図 2 に示すように、クラス ビューの半分です。 ダブルクリックして、`Connection`プロパティをそのコードを参照してください。


![クラス ビューの自動生成されたコードを表示する接続プロパティをダブルクリックします。](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image4.png)

**図 2**: クラス ビューの自動生成されたコードを表示する接続プロパティをダブルクリックします。


TableAdapter の`Connection`プロパティおよびその他のコードの接続に関連する次のとおりです。


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample1.cs)]

TableAdapter クラスをインスタンスするときに、メンバー変数`_connection`と等しい`null`します。 ときに、`Connection`かどうかをまずチェック プロパティにアクセスした、`_connection`メンバー変数がインスタンス化されています。 そうでない場合、`InitConnection`メソッドが呼び出される、がインスタンス化`_connection`設定とその`ConnectionString`TableAdapter 構成ウィザードの最初の手順から指定された接続文字列の値にプロパティ。

`Connection`にプロパティを割り当てることも、`SqlConnection`オブジェクト。 新しい関連付けますそう`SqlConnection`、tableadapter の各オブジェクト`SqlCommand`オブジェクト。

## <a name="step-2-exposing-connection-level-settings"></a>手順 2: 接続レベルの設定を公開します。

接続情報は、TableAdapter 内でカプセル化されたままし、アプリケーションのアーキテクチャの他のレイヤーにアクセスできないする必要があります。 ただし、TableAdapter の接続レベルの情報をアクセスまたはクエリ、ユーザー、または ASP.NET ページをカスタマイズする必要がある場合、シナリオする可能性があります。

S を拡張できるように、`ProductsTableAdapter`で、`Northwind`データセットに含める、`ConnectionString`プロパティの読み取りまたは TableAdapter で使用される接続文字列を変更するビジネス ロジック層で使用できます。

> [!NOTE]
> A*接続文字列*は、データベース、認証の資格情報、およびその他のデータベースに関連する設定の場所を使用するプロバイダーなどのデータベース接続情報を指定する文字列です。 さまざまなデータ ストアとプロバイダーで使用される接続文字列のパターンの一覧は、次を参照してください。 [ConnectionStrings.com](http://www.connectionstrings.com/)します。


説明したように、[データ アクセス層を作成する](../introduction/creating-a-data-access-layer-cs.md)チュートリアルでは、型指定されたデータセットの自動生成されたクラスは、部分クラスを使用して拡張できます。 まず、という名前のプロジェクトに新しいサブフォルダーを作成`ConnectionAndCommandSettings`下にある、`~/App_Code/DAL`フォルダー。


![ConnectionAndCommandSettings という名前のサブフォルダーを追加します。](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image5.png)

**図 3**: という名前のサブフォルダーの追加 `ConnectionAndCommandSettings`


という名前の新しいクラス ファイルを追加`ProductsTableAdapter.ConnectionAndCommandSettings.cs`し、次のコードを入力します。


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample2.cs)]

この部分クラスを追加、`public`という名前のプロパティ`ConnectionString`を`ProductsTableAdapter`の読み取りまたは tableadapter を基になる接続の接続文字列を更新するには、どのレイヤーを使用するクラス。

この部分クラスを使用して作成された (および保存) を開き、`ProductsBLL`クラス。 既存のメソッドのいずれかに移動し、入力`Adapter`IntelliSense を表示する期間のキーを押すとします。 表示には、新しい`ConnectionString`するまたはプログラムで読み取ることができます BLL からこの値を調整することを意味します。 IntelliSense で使用できるプロパティです。

## <a name="exposing-the-entire-connection-object"></a>全体の接続オブジェクトを公開します。

この部分クラスが基になる接続オブジェクトの 1 つのプロパティを公開します:`ConnectionString`します。 全体の接続オブジェクトを TableAdapter の境界を超えて使用できるようにする場合は、または変更、`Connection`プロパティ %s の保護レベル。 手順 1. で調べる自動生成されたコードを示した、tableadapter`Connection`プロパティがマーク`internal`、それのみにアクセスできることによって、同じアセンブリ内のクラスを意味します。 これは、ただし、tableadapter を使用して`ConnectionModifier`プロパティ。

開く、`Northwind`データセットをクリックして、`ProductsTableAdatper`デザイナーで、[プロパティ] ウィンドウに移動します。 表示されます、`ConnectionModifier`が既定値に設定`Assembly`します。 させる、`Connection`プロパティの変更、型指定されたデータセットのアセンブリの外部で使用できる、`ConnectionModifier`プロパティを`Public`します。


[![ConnectionModifier プロパティを使用して接続プロパティのアクセシビリティ レベルを構成することができます。](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image6.png)

**図 4**:`Connection`を使用してプロパティのアクセシビリティ レベルを構成できます %s、`ConnectionModifier`プロパティ ([フルサイズの画像を表示する をクリックします](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image8.png))。


データセットを保存してからに戻ります、`ProductsBLL`クラス。 前に、既存のメソッドのいずれかに移動し、入力として`Adapter`IntelliSense を表示する期間のキーを押すとします。 一覧に含める必要があります、`Connection`をできるようになりましたプログラムで読み取りか BLL から任意の接続レベルの設定を割り当てるを意味するプロパティ。

## <a name="step-3-examining-the-command-related-properties"></a>手順 3: は、コマンドに関連するプロパティを調べること。

TableAdapter は、既定では自動生成するメイン クエリ`INSERT`、 `UPDATE`、および`DELETE`ステートメント。 メイン クエリ s `INSERT`、 `UPDATE`、および`DELETE`ステートメントは、TableAdapter のコードを使用して、ADO.NET データ アダプター オブジェクトとして実装されます、`Adapter`プロパティ。 使用するような`Connection`プロパティ、`Adapter`プロパティのデータ型が使用するデータ プロバイダーによって決定されます。 これらのチュートリアルは、SqlClient プロバイダーを使用しているため、`Adapter`プロパティの型は[ `SqlDataAdapter`](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx)します。

TableAdapter s`Adapter`プロパティの型の 3 つのプロパティが`SqlCommand`を使用している問題、 `INSERT`、`UPDATE`と`DELETE`ステートメント。

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

A`SqlCommand`オブジェクトは、データベースに特定のクエリを送信およびなどのプロパティを持つ: [ `CommandText` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx)、アドホック SQL ステートメントまたはストアド プロシージャを実行するには; を含むと[ `Parameters`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx)のコレクションである`SqlParameter`オブジェクト。 説明したように、[データ アクセス層を作成する](../introduction/creating-a-data-access-layer-cs.md)チュートリアルでは、これらコマンドのプロパティ ウィンドウからオブジェクトをカスタマイズできます。

TableAdapter のメインのクエリだけでなくさまざまなメソッドを含めることができますが、呼び出されると、データベースに指定されたコマンドをディスパッチします。 メイン クエリのコマンド オブジェクトとその他のすべてのメソッドのコマンド オブジェクトが、tableadapter に格納されている`CommandCollection`プロパティ。

Let s は、によって生成されたコードを確認する少し、`ProductsTableAdapter`で、`Northwind`これら 2 つのプロパティとそのメンバー変数のサポートとヘルパー メソッドのデータセット。


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample3.cs)]

コードを`Adapter`と`CommandCollection`プロパティが正確に模倣する、`Connection`プロパティ。 プロパティで使用されるオブジェクトを保持するメンバー変数があります。 プロパティ`get`アクセサーは、対応するメンバー変数が調べることから始めます`null`します。 そうである場合は、メンバー変数のインスタンスを作成し、コア コマンド関連のプロパティを割り当てます初期化メソッドが呼び出されます。

## <a name="step-4-exposing-command-level-settings"></a>手順 4: コマンド レベルの設定を公開します。

理想的には、コマンド レベルの情報は、データ アクセス層内でカプセル化されたのままにする必要があります。 この情報をアーキテクチャの他のレイヤーで必要に応じて、ただし、それを通じて公開できます部分クラスと同じように接続レベルの設定で。

TableAdapter は、1 つのみがあるため`Connection`プロパティ、接続レベルの設定を公開するためのコードはとても簡単です。 TableAdapter では複数のコマンド オブジェクトのために、コマンド レベルの設定を変更するときに、モ ノはもう少し複雑な`InsertCommand`、 `UpdateCommand`、および`DeleteCommand`、コマンド オブジェクトの変数の数と共に、 `CommandCollection`プロパティ。 コマンド レベルの設定を更新する場合、これらの設定は、すべてのコマンド オブジェクトに反映される必要があります。

たとえばを実行する特別の長い時間がかかった TableAdapter で特定のクエリがあったことを想像してください。 TableAdapter を使用して、これらのクエリのいずれかを実行する、する可能性が、コマンド オブジェクトを増やす[`CommandTimeout`プロパティ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx)します。 このプロパティは、コマンドの実行を待機する秒数を指定し、既定値は 30。

許可する、 `CommandTimeout` 、BLL によって調整されるプロパティは、次のコードを追加`public`メソッドを`ProductsDataTable`手順 2. で作成された部分クラス ファイルを使用して (`ProductsTableAdapter.ConnectionAndCommandSettings.cs`)。


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample4.cs)]

その TableAdapter インスタンスによってすべてのコマンドの問題のコマンド タイムアウトを設定するには、BLL またはプレゼンテーション層からこのメソッドを呼び出す可能性があります。

> [!NOTE]
> `Adapter`と`CommandCollection`プロパティがマーク`private`TableAdapter 内のコードからのみアクセスを意味します。 異なり、`Connection`プロパティ、これらのアクセス修飾子は構成できません。 したがって、アーキテクチャの他のレイヤーをコマンド レベルのプロパティを公開する必要がある場合は、部分クラスのアプローチを提供する前に説明したを使用する必要があります、`public`メソッドまたはプロパティに対して読み取りまたは書き込みを`private`コマンド オブジェクト。


## <a name="summary"></a>まとめ

データ アクセスの詳細と複雑さをカプセル化する型指定されたデータセット内で Tableadapter が機能します。 Tableadapter を使用して、私たちはありません、データベースへの接続、コマンドを発行または DataTable に結果を読み込むのための ADO.NET コードを記述を気にします。 すべて自動的に処理を。

ただし、ADO.NET の低レベル仕様の接続文字列または接続またはコマンド タイムアウトの既定値の変更などをカスタマイズする必要がある場合もあります。 TableAdapter が自動生成`Connection`、 `Adapter`、および`CommandCollection`プロパティがこれらのいずれかが`internal`または`private`、既定では。 含める部分クラスを使用して、TableAdapter を拡張することによって、この内部情報を公開できる`public`メソッドまたはプロパティ。 Tableadapter ではまた、`Connection`プロパティ アクセス修飾子は、tableadapter を構成できます`ConnectionModifier`プロパティ。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Burnadette Leigh、S ren Jacob Lauritsen、Teresa Murphy、および Hilton Geisenow でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](working-with-computed-columns-cs.md)
> [次へ](protecting-connection-strings-and-other-configuration-information-cs.md)
