---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
title: "データ アクセス層の接続とコマンド レベルの設定 (c#) の構成 |Microsoft ドキュメント"
author: rick-anderson
description: "型指定されたデータセット内で Tableadapter に自動的に対処するデータベースへの接続、コマンドを発行して、結果を含む DataTable を設定する."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: cd330dd9-6254-4305-9351-dd727384c83b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
msc.type: authoredcontent
ms.openlocfilehash: 5675c1c2a1c8987412ae79707e4c20e29e0e0df6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-c"></a>データ アクセス層の接続とコマンド レベルの設定 (c#) を構成します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_CS.zip)または[PDF のダウンロード](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/datatutorial72cs1.pdf)

> 型指定されたデータセット内で Tableadapter 自動的に対処データベースへの接続、コマンドを発行して、結果を含む DataTable を設定します。 場合は TableAdapter のデータベースの接続とコマンド レベル設定にアクセスする方法を学習おとこのチュートリアルでは、これらの詳細のように注意する場合があります。


## <a name="introduction"></a>はじめに

一連のチュートリアル全体では、データ アクセス レイヤーと、レイヤー アーキテクチャのビジネス オブジェクトを実装するのに型指定されたデータセットを使用しています。 説明したように、[の最初のチュートリアル](../introduction/creating-a-data-access-layer-cs.md)、型指定されたデータセット s データ テーブルは、Tableadapter が取得および基になるデータを変更するデータベースと通信するラッパーとして機能がデータのリポジトリとして機能します。 Tableadapter では、データベースを操作に関係する複雑さをカプセル化し、データベースへの接続、コマンドを実行またはデータ テーブルに結果を設定するコードを記述することから us を保存します。

ただし、TableAdapter の深さに burrow し、は、ADO.NET オブジェクトを直接操作するコードを記述する必要がお場合があります。 [トランザクション内でデータベースの変更をラッピング](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md)チュートリアルでは、開始、コミット、および ADO.NET のトランザクションをロールバックの TableAdapter にメソッドを追加おなどです。 これらのメソッドは、内部で使用される手動で作成された`SqlTransaction`、tableadapter に割り当てられたオブジェクト`SqlCommand`オブジェクト。

このチュートリアルでは、TableAdapter でデータベース接続とコマンド レベルの設定にアクセスする方法を検討します。 具体的には、機能を追加、`ProductsTableAdapter`有効が基になる接続文字列にアクセスし、コマンドのタイムアウト設定します。

## <a name="working-with-data-using-adonet"></a>ADO.NET を使用してデータの操作

Microsoft .NET Framework には、多種多様な具体的にはデータを処理するよう設計されたクラスが含まれています。 内で見つかった、これらのクラス、 [ `System.Data`名前空間](https://msdn.microsoft.com/en-us/library/system.data.aspx)、呼びます、 *ADO.NET*クラスです。 ADO.NET でのクラスの一部は、特定に結び付けられます*データ プロバイダー*です。 ADO.NET のクラスと、基になるデータ ストア間を流れる情報を許可する通信チャネルとして、データ プロバイダーの考えることができます。 Ole Db と ODBC では、だけでなく、特定のデータベース システム向けに設計された特殊なプロバイダーと同様に、一般的なプロバイダーがあります。 たとえば、ole Db プロバイダーを使用して Microsoft SQL Server データベースに接続することはできますが、SqlClient プロバイダーははるかに効率的に設計されていますが、具体的には SQL Server 用に最適化されたです。

データをプログラムでアクセスするには、次のパターンがよく使用されます。

- データベースへの接続を確立します。
- コマンドを発行します。
- `SELECT`クエリ、結果のレコードを処理します。

これらの各手順を実行するための別の ADO.NET クラスがあります。 たとえば、SqlClient プロバイダーを使用してデータベースに接続する場合を使用して、 [ `SqlConnection`クラス](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlconnection(VS.80).aspx)です。 発行する、 `INSERT`、 `UPDATE`、 `DELETE`、または`SELECT`コマンドを使用して、データベースを[`SqlCommand`クラス](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlcommand.aspx)です。

を除き、[トランザクション内でデータベースの変更をラッピング](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md)チュートリアルではなかった、Tableadapter の自動生成されたコードには、必要な機能が含まれているため自分低レベルの ADO.NET コードを作成するにはデータベースへの接続、コマンドを発行、データを取得およびデータ テーブルにそのデータを設定します。 ただし、これらの低レベルの設定をカスタマイズして必要がある場合もあります。 次に示す手順で、Tableadapter によって内部的に使用する ADO.NET オブジェクトを活用する方法について調べます。

## <a name="step-1-examining-with-the-connection-property"></a>手順 1: は接続プロパティを使用して確認します。

各 TableAdapter クラスには、`Connection`データベース接続情報を指定するプロパティです。 このプロパティのデータ型と`ConnectionString`値は、TableAdapter 構成ウィザードで行った選択によって決まります。 最初に型指定されたデータセットに TableAdapter を追加する際このウィザードを us データベースに注意してください (図 1 を参照してください) のソースします。 この最初の手順でドロップダウン リストには、サーバー エクスプ ローラーのデータ接続でその他のデータベースと同様に、構成ファイルで指定されたこれらのデータベースが含まれています。 ドロップダウン リストで使用するデータベースが存在しない場合は、新しい接続 をクリックし、必要な接続情報を提供する新しいデータベース接続を指定できます。


[![TableAdapter 構成ウィザードの最初の手順](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image1.png)

**図 1**: TableAdapter 構成ウィザードの最初のステップを ([フルサイズのイメージを表示するをクリックして](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image3.png))


Let 秒かかる、tableadapter のコードを調べるにはしばらく`Connection`プロパティです。 説明したとおり、[データ アクセス レイヤーを作成する](../introduction/creating-a-data-access-layer-cs.md)チュートリアルでは、クラス ビュー] ウィンドウに、適切なクラスへのドリルダウンし、[メンバーの名前をダブルクリックして、TableAdapter の自動生成されたコードを表示できます。

表示 メニューに、クラス ビュー を選択して (または Ctrl + Shift + C を入力して) は、クラス ビュー ウィンドウに移動します。 クラス ビュー ウィンドウの上部から下にドリル ダウン、`NorthwindTableAdapters`名前空間を選択し、`ProductsTableAdapter`クラスです。 これが表示されます、`ProductsTableAdapter`下にある s メンバー図 2 に示すように、クラス ビューの半分です。 ダブルクリックして、`Connection`プロパティをそのコードを参照してください。


![[クラス ビューの自動生成されたコードを表示する接続プロパティ] をダブルクリックします。](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image4.png)

**図 2**: クラス ビューの自動生成されたコードを表示する で、接続プロパティ をダブルクリック


TableAdapter の`Connection`プロパティおよびその他のコードの接続に関連する次のようにします。


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample1.cs)]

TableAdapter クラスがインスタンス化されるときは、メンバー変数`_connection`と等しい`null`です。 ときに、`Connection`かどうかをまず確認プロパティにアクセスした、`_connection`メンバー変数がインスタンス化されました。 ない場合、`InitConnection`メソッドが呼び出され、これをインスタンス化`_connection`設定とその`ConnectionString`TableAdapter 構成ウィザードの最初の手順から指定した接続文字列の値をプロパティです。

`Connection`にプロパティを割り当てることも、`SqlConnection`オブジェクト。 これにより、新しいを関連付けます`SqlConnection`、tableadapter の各オブジェクト`SqlCommand`オブジェクト。

## <a name="step-2-exposing-connection-level-settings"></a>手順 2: 接続レベルの設定を公開します。

接続情報は、TableAdapter にカプセル化されたままし、アプリケーションのアーキテクチャの他のレイヤーにアクセスできないする必要があります。 ただし、TableAdapter の接続レベルの情報は、アクセスまたはクエリ、ユーザー、または ASP.NET ページ用にカスタマイズする必要がある場合、シナリオする可能性があります。

S を拡張できるように、`ProductsTableAdapter`で、`Northwind`データセットに含める、`ConnectionString`プロパティの読み取りまたは TableAdapter で使用される接続文字列を変更するビジネス ロジック層で使用できます。

> [!NOTE]
> A*接続文字列*データベース、認証資格情報、およびその他のデータベースに関連する設定の場所を使用するプロバイダーなど、データベース接続情報を指定する文字列です。 各種のデータ ストアおよびプロバイダーで使用される接続文字列のパターンの一覧は、次を参照してください。 [ConnectionStrings.com](http://www.connectionstrings.com/)です。


説明したように、[データ アクセス レイヤーを作成する](../introduction/creating-a-data-access-layer-cs.md)チュートリアルでは、型指定されたデータセットの自動生成されたクラスは、部分クラスを使用して拡張できます。 まず、という名前のプロジェクトに新しいサブフォルダーを作成`ConnectionAndCommandSettings`下にある、`~/App_Code/DAL`フォルダーです。


![ConnectionAndCommandSettings をという名前のサブフォルダーを追加します。](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image5.png)

**図 3**: という名前のサブフォルダーの追加`ConnectionAndCommandSettings`


という新しいクラス ファイルを追加`ProductsTableAdapter.ConnectionAndCommandSettings.cs`し、次のコードを入力します。


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample2.cs)]

この部分クラスを追加、`public`という名前のプロパティ`ConnectionString`を`ProductsTableAdapter`の読み取りまたは tableadapter を基になる接続の接続文字列を更新するには、どのレイヤーを使用するクラス。

この部分クラスを作成しました (および保存) を開く、`ProductsBLL`クラスです。 既存のメソッドのいずれかに移動し、入力`Adapter`IntelliSense を表示する期間のキーを押すとします。 新しい表示`ConnectionString`プロパティをプログラムからの読み取りまたはできる BLL からこの値を調整つまり IntelliSense で使用します。

## <a name="exposing-the-entire-connection-object"></a>全体の接続オブジェクトを公開します。

この部分クラスは、基になる接続オブジェクトの 1 つのプロパティを公開:`ConnectionString`です。 全体の接続オブジェクトを TableAdapter の範囲を超えて使用できるようにする場合は、または変更、`Connection`プロパティの保護レベル。 手順 1. で確認して自動生成されたコードを示した、tableadapter`Connection`プロパティとしてマーク`internal`、ことのみアクセスできる、同じアセンブリ内のクラスを意味します。 これは、ただし、tableadapter を使用して`ConnectionModifier`プロパティです。

開いている、`Northwind`データセットをクリックして、`ProductsTableAdatper`デザイナーで、[プロパティ] ウィンドウに移動します。 表示されます、`ConnectionModifier`その既定値に設定`Assembly`です。 させる、`Connection`プロパティの変更、型指定されたデータセットのアセンブリの外部で使用可能な`ConnectionModifier`プロパティを`Public`です。


[![ConnectionModifier プロパティ経由で接続プロパティのアクセシビリティ レベルを構成することができます。](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image6.png)

**図 4**:`Connection`プロパティ アクセシビリティ レベルを構成できます %s を使用して、`ConnectionModifier`プロパティ ([フルサイズのイメージを表示するをクリックして](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image8.png))


データセットを保存しからに戻り、`ProductsBLL`クラスです。 既存のメソッドのいずれかに移動する前とに入力`Adapter`IntelliSense を表示する期間のキーを押すとします。 一覧に含める必要があります、`Connection`を今すぐプログラムでの読み取りまたはできる BLL から、接続レベルの設定を割り当てるを意味するプロパティです。

## <a name="step-3-examining-the-command-related-properties"></a>手順 3: コマンドに関連するプロパティを確認します。

TableAdapter は、既定が自動生成されるメイン クエリ`INSERT`、 `UPDATE`、および`DELETE`ステートメントです。 メイン クエリ s `INSERT`、 `UPDATE`、および`DELETE`ステートメントはコードで実装、TableAdapter s 経由で ADO.NET データ アダプター オブジェクトとして、`Adapter`プロパティです。 使用するような`Connection`、プロパティ、`Adapter`プロパティのデータ型が使用するデータ プロバイダーによって決定されます。 これらのチュートリアルは、SqlClient プロバイダーを使用するので、`Adapter`プロパティの型は[ `SqlDataAdapter`](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqldataadapter(VS.80).aspx)です。

TableAdapter s`Adapter`プロパティ型の 3 つのプロパティは、`SqlCommand`を使用している問題、 `INSERT`、 `UPDATE`、および`DELETE`ステートメント。

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

A`SqlCommand`オブジェクトは、データベースを特定のクエリを送信しなどのプロパティを持っている: [ `CommandText` ](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlcommand.commandtext.aspx)、アドホック SQL ステートメントまたは実行するストアド プロシージャが含まれていますと[ `Parameters`](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlcommand.parameters.aspx)の集合である`SqlParameter`オブジェクト。 バックアップで示したように、[データ アクセス レイヤーを作成する](../introduction/creating-a-data-access-layer-cs.md)チュートリアルでは、このコマンドのプロパティ ウィンドウからオブジェクトをカスタマイズすることができます。

TableAdapter のメインのクエリだけでなく、可変個のメソッドを含めることができます、呼び出されると、データベースへ指定したコマンドをディスパッチします。 メイン クエリのコマンド オブジェクトとすべての追加方法のコマンド オブジェクトが、tableadapter に格納されている`CommandCollection`プロパティです。

Let s をとってによって生成されたコードを見て、`ProductsTableAdapter`で、`Northwind`これら 2 つのプロパティとそのサポートのメンバー変数とヘルパー メソッドのデータセット。


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample3.cs)]

コードを`Adapter`と`CommandCollection`プロパティが正確に模倣の`Connection`プロパティです。 プロパティで使用されるオブジェクトを保持するメンバー変数があります。 プロパティ`get`アクセサーがかどうかを対応するメンバー変数の確認から開始`null`です。 場合は、メンバー変数のインスタンスを作成し、コア コマンドに関連するプロパティが割り当てられます初期化メソッドが呼び出されます。

## <a name="step-4-exposing-command-level-settings"></a>手順 4: コマンド レベルの設定を公開します。

理想的には、コマンド レベルの情報は、データ アクセス層のカプセル化されたのままにする必要があります。 この情報をアーキテクチャの他のレイヤーで必要なただし、それを公開できます、部分クラスを使用すると同じように接続レベルの設定をします。

TableAdapter は、1 つだけがあるため`Connection`プロパティは、接続レベルの設定を公開するためのコードはきわめて簡単です。 TableAdapter は、複数のコマンド オブジェクトを持つことができますので、コマンド レベルの設定を変更するときに次の点がもう少し複雑なして、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`、内のコマンド オブジェクトの変数の数と共に、 `CommandCollection`プロパティ。 コマンド レベルの設定を更新するには、これらの設定をすべてのコマンド オブジェクトに反映する必要があります。

たとえば、時間がかかり、異例な長い実行は TableAdapter の特定のクエリがあったことを想像してください。 TableAdapter を使用して、それらのクエリのいずれかを実行する、必要になるコマンド オブジェクトを向上させる[`CommandTimeout`プロパティ](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx)です。 このプロパティは、コマンドの実行を待機する秒数を指定、既定値は 30 です。

使用できるように、 `CommandTimeout` 、BLL によって調整されるようにプロパティが次のコードを追加`public`メソッドを`ProductsDataTable`手順 2. で作成される部分クラス ファイルを使用して (`ProductsTableAdapter.ConnectionAndCommandSettings.cs`)。


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample4.cs)]

このメソッドは、その TableAdapter インスタンスによってコマンドのすべての問題のコマンド タイムアウトを設定するには、BLL またはプレゼンテーション層から呼び出すことでした。

> [!NOTE]
> `Adapter`と`CommandCollection`としてマークされているプロパティ`private`TableAdapter 内のコードからのみアクセスを意味します。 異なり、`Connection`プロパティ、これらのアクセス修飾子は構成できません。 そのため、アーキテクチャの他のレイヤーをコマンド レベルのプロパティを公開する必要がある場合必要がありますアプローチを使用する、部分クラスを提供する上で説明した、`public`メソッドまたはプロパティに対して読み取りまたは書き込みを`private`コマンド オブジェクトです。


## <a name="summary"></a>概要

型指定されたデータセット内で Tableadapter の提供をデータ アクセスの詳細と複雑さをカプセル化します。 Tableadapter を使用すると、私たちはありません、データベースへの接続、コマンドを実行またはデータ テーブルに結果を設定する ADO.NET コードの記述について心配します。 これはすべて自動的に処理ご利用の米国。

ただし、接続文字列または接続またはコマンド タイムアウトの既定値の変更などの ADO.NET 固有の低レベルのカスタマイズは必要がある場合もあります。 TableAdapter が自動生成`Connection`、 `Adapter`、および`CommandCollection`プロパティも、これらのいずれかが`internal`または`private`既定でします。 含める部分クラスを使用して、TableAdapter を拡張することによって、この内部情報を公開できる`public`メソッドまたはプロパティ。 Tableadapter ではまた、`Connection`プロパティ アクセス修飾子は、tableadapter を構成できます`ConnectionModifier`プロパティです。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者が Burnadette Leigh、S ren 一 Lauritsen Teresa マーフィーと Hilton Geisenow 発生しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](working-with-computed-columns-cs.md)
[次へ](protecting-connection-strings-and-other-configuration-information-cs.md)
