---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
title: トランザクション (VB) 内のデータベース変更のラッピング |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、更新、削除、およびデータのバッチを挿入するのには 4 つの 1 つ目です。 このチュートリアルではは、データベースのトランザクションを使用する方法について説明します.
ms.author: aspnetcontent
ms.date: 06/26/2007
ms.assetid: 7d821db5-6cbb-4b38-af14-198f9155fc82
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 877174bad08970eed0cab52d0f1d8a521f7d2cc0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840746"
---
<a name="wrapping-database-modifications-within-a-transaction-vb"></a>トランザクション (VB) 内で変更をラップするデータベース
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_VB.zip)または[PDF のダウンロード](wrapping-database-modifications-within-a-transaction-vb/_static/datatutorial63vb1.pdf)

> このチュートリアルでは、更新、削除、およびデータのバッチを挿入するのには 4 つの 1 つ目です。 このチュートリアルではは、データベースのトランザクションが確実にすべての手順が成功したか、またはすべての手順が失敗する、アトミック操作として実行するバッチ変更を許可する方法について説明します。


## <a name="introduction"></a>はじめに

以降で説明したように、[挿入の概要、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)チュートリアルでは、GridView で、行レベルの編集および削除する組み込みサポートが提供します。 マウスの数回のクリックは、コンテンツを編集して、行単位で削除する限り、行のコードを記述することがなく、豊富なデータ変更インターフェイスを作成できます。 ただし、特定のシナリオでこれが不十分で、編集またはレコードのバッチを削除する機能をユーザーに提供する必要があります。

たとえば、web ベースの電子メール クライアントの最もグリッドを使用する各メッセージを一覧表示する行ごとに電子メールの情報 (件名、送信者など) と共にチェック ボックスが含まれています。 このインターフェイスは、削除するには、複数のメッセージをチェックして、選択したメッセージの削除ボタンをクリックするユーザーを許可します。 インターフェイスの編集のバッチとは、ユーザーは、多くの異なるレコードよくを編集する場合に最適です。 ユーザーがクリックするのではなく、編集、変更を変更する必要がある各レコードの更新 をクリックして、インターフェイスの編集のバッチ編集インターフェイスで各の行を表示します。 ユーザーは、簡単に変更する必要がある行のセットを変更し、更新プログラムのすべてのボタンをクリックしてこれらの変更を保存できます。 この一連のチュートリアルでの挿入、編集、およびデータのバッチを削除するためのインターフェイスを作成する方法を説明します。

バッチ操作を実行するときに、失敗するかどうか必要があることの一部の操作が成功するバッチの中に他のユーザーを決定することが重要です。 インターフェイスの場合、選択した最初のレコードが正常に削除されたが、2 つ目が失敗したと答えると、外部キー制約違反のための動作を削除するバッチを検討してください。 最初のレコードの削除をロールバックしまたは最初のレコードが削除されたままにしますか。

バッチ操作として処理する場合、[アトミック操作](http://en.wikipedia.org/wiki/Atomic_operation)、いずれかの場所か、すべての手順が成功またはすべての手順が失敗すると、し、データ アクセス層がサポートするように拡張する必要があります[データベーストランザクション](http://en.wikipedia.org/wiki/Database_transaction)です。 データベース トランザクションは、一連の原子性を保証`INSERT`、 `UPDATE`、および`DELETE`ステートメントはトランザクションの包括的で実行され、ほとんどすべての最新のデータベース システムでサポートされている機能です。

このチュートリアルでは、データベースのトランザクションを使用する DAL を拡張する方法を紹介します。 以降のチュートリアルでは、挿入、更新、およびインターフェイスを削除するバッチの実装の web ページを確認します。 Let s を始めましょう。

> [!NOTE]
> バッチのトランザクション内のデータを変更するときに常に原子性は必要ありません。 一部のシナリオで成功、一部のデータ変更が許容される場合があり、ときなど、他のユーザーが同じバッチ内で失敗電子メールのセットの web ベースの電子メール クライアントから削除します。 データベース エラーの途中を削除での処理が s 場合、s がエラーなしで処理されるこれらのレコードが削除されたまま、許容される可能性があります。 このような場合は、DAL はデータベース トランザクションをサポートするように変更するのには必要ありません。 その他のバッチ操作シナリオは、ただし、原子性が重要です。 顧客は、1 つの銀行口座から自分の資金を移動すると、は、2 つの操作を実行する必要があります。 資金の最初のアカウントから差し引かれます、2 番目に追加する必要があります。 銀行可能性があります、成功の最初の手順が発生しても問題ないが、2 番目の手順が失敗する、中に、お客様は不安を感じていることは明確になります。 ぜひこのチュートリアルを使用してバッチ挿入でそれらを使用して、更新、および次の 3 つのチュートリアルでビルドするインターフェイスを削除する予定がない場合でも、データベース トランザクションをサポートするために DAL の機能強化を実装するため。


## <a name="an-overview-of-transactions"></a>トランザクションの概要

ほとんどのデータベース サポートは含まれません*トランザクション*作業の 1 つの論理単位にグループ化する複数のデータベース コマンドを有効にします。 トランザクションに含まれるデータベース コマンド アトミック、つまり、すべてのコマンドは失敗、またはすべてが成功であることが保証されます。

一般に、次のパターンを使用して SQL ステートメントのトランザクションが実装されます。

1. トランザクションの開始を示します。
2. トランザクションを構成する SQL ステートメントを実行します。
3. 手順 2 では、トランザクションをロールバック ステートメントのいずれかでエラーがある場合
4. エラーなしですべての手順 2 からステートメント完了、トランザクションをコミットします。

を作成するために使用する SQL ステートメントのコミット、およびストアド プロシージャの SQL スクリプトを記述または作成するときに、トランザクションを手動で入力することができますをロールバックまたはプログラムで ADO.NET またはクラスのいずれかを使用することを意味します、 [ `System.Transactions`名前空間](https://msdn.microsoft.com/library/system.transactions.aspx)します。 このチュートリアルを究明するだけで ADO.NET を使用してトランザクションを管理します。 今後のチュートリアルでは、どの時点で、SQL ステートメントの作成、ロールバック、およびトランザクションのコミットについて説明します、データ アクセス層でストアド プロシージャを使用する方法に注目します。 それまでを参照してください[SQL Server ストアド プロシージャでのトランザクションの管理](http://www.4guysfromrolla.com/webtech/080305-1.shtml)詳細についてはします。

> [!NOTE]
> [ `TransactionScope`クラス](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx)で、`System.Transactions`名前空間により、開発者は、トランザクションのスコープ内で一連のステートメントをプログラムでラップして複数の関連する複雑なトランザクションのサポートが含まれています2 つの異なるデータベースまたはでも異種の種類の Microsoft SQL Server データベース、Oracle データベース、Web サービスなどのデータ ストアなどのソース。 このチュートリアルではなく ADO.NET トランザクションの使用を決定したら、`TransactionScope`クラスは ADO.NET がデータベース トランザクションと多くの場合より固有であるために、はるかに少ないリソースを消費します。 さらに、一部のシナリオで、`TransactionScope`クラスは、Microsoft 分散トランザクション コーディネーター (MSDTC) を使用します。 構成、実装、およびパフォーマンスの問題周囲の MSDTC によりではなく特殊および高度なトピックと、これらのチュートリアルの範囲を超えています。


呼び出すことによって、トランザクションが開始された ado.net SqlClient プロバイダーを使用する場合、 [ `SqlConnection`クラス](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx)s [ `BeginTransaction`メソッド](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx)、返された、 [ `SqlTransaction`オブジェクト](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx)します。 構成のトランザクションが内に配置するデータ変更ステートメントを`try...catch`ブロックします。 ステートメントでエラーが発生した場合、`try`への転送の実行をブロック、 `catch` 、トランザクションをロールバックを使用して、ブロック、`SqlTransaction`オブジェクト[`Rollback`メソッド](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx)します。 すべてのステートメントへの呼び出しで完全に成功する場合、`SqlTransaction`オブジェクト[`Commit`メソッド](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx)の最後に、`try`ブロックがトランザクションをコミットします。 次のコード スニペットは、このパターンを示しています。 参照してください[トランザクションでデータベースの整合性を維持](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)構文と ADO.NET を使用したトランザクションを使用しての例を示します。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample1.vb)]

既定では、型指定されたデータセットに Tableadapter は、トランザクションを使用しないでください。 トランザクションのサポートを提供するには、上記のパターンを使用して、一連のトランザクションのスコープ内でデータ変更ステートメントを実行する追加のメソッド、TableAdapter クラスを拡張する必要があります。 手順 2 では、部分クラスを使用して、これらのメソッドを追加する方法がわかります。

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>手順 1: データのバッチ化された Web ページで、作業を作成します。

データベース トランザクションをサポートするために DAL を補強する方法の調査を始める前に、このチュートリアルに必要な ASP.NET web ページと続く 3 つ作成する最初に少し s を使用できます。 という名前の新しいフォルダーを追加することで開始`BatchData`し、次の ASP.NET ページは、使用する各ページの関連付けを追加、`Site.master`マスター ページ。

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![SqlDataSource に関連するチュートリアルについては、ASP.NET ページに追加します。](wrapping-database-modifications-within-a-transaction-vb/_static/image1.gif)

**図 1**: SqlDataSource に関連するチュートリアルについては、ASP.NET ページに追加します。


他のフォルダーと同様`Default.aspx`を使用して、`SectionLevelTutorialListing.ascx`セクション内でチュートリアルを一覧表示するユーザー コントロール。 そのため、このユーザー コントロールを追加`Default.aspx`をページのデザイン ビューに ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](wrapping-database-modifications-within-a-transaction-vb/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image1.png)

**図 2**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズの画像を表示する をクリックします](wrapping-database-modifications-within-a-transaction-vb/_static/image2.png))。


最後に、これら 4 つのページを追加するエントリとして、`Web.sitemap`ファイル。 具体的には、次のマークアップを追加、カスタマイズした後、サイト マップ`<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample2.xml)]

更新した後`Web.sitemap`、時間、ブラウザーを使ってチュートリアル web サイトを表示するのにはかかりません。 左側のメニューには、バッチ化されたデータのチュートリアルを使用した作業項目が含まれています。


![サイト マップ データのバッチ処理のチュートリアルを使用した作業のエントリになりました](wrapping-database-modifications-within-a-transaction-vb/_static/image3.gif)

**図 3**: サイト マップ データのバッチ処理のチュートリアルを使用した作業のエントリになりました


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>手順 2: データベース トランザクションをサポートするために、データ アクセス層を更新しています

最初のチュートリアルで説明したよう[データ アクセス層を作成する](../introduction/creating-a-data-access-layer-vb.md)、DAL に型指定されたデータセットはデータ テーブルと Tableadapter ので構成されます。 データ テーブルは、Tableadapter は、Datatable、Datatable、およびなどに加えられた変更とデータベースを更新するにはデータベースからデータを読み取る機能を提供中にデータを保持します。 Tableadapter が、バッチ更新とダイレクト DB とは呼ばれるデータの更新の 2 つのパターンを指定することを思い出してください。 バッチ更新パターンでは、TableAdapter はデータセット、データ テーブル、または DataRows のコレクションに渡されます。 このデータが列挙し、各挿入、変更、または行を削除、 `InsertCommand`、 `UpdateCommand`、または`DeleteCommand`を実行します。 DB ダイレクトのパターンを持つ、TableAdapter は、挿入、更新、または 1 つのレコードを削除するために必要な列の値を代わりに渡されます。 DB 直接パターン メソッドは、適切な実行を渡された値を使用するし`InsertCommand`、 `UpdateCommand`、または`DeleteCommand`ステートメント。

使用した更新パターンに関係なく、Tableadapter の自動生成されたメソッドは、トランザクションを使用しません。 既定で各挿入、更新、または TableAdapter によって実行された削除は、1 つの独立した操作として扱われます。 たとえば、BLL の一部のコードで、データベースに 10 個のレコードを挿入する DB ダイレクト パターンが使用されることを想像してください。 このコードは、tableadapter を呼び出す`Insert`メソッドでその 10 倍です。 最初の 5 つの挿入が成功、6 番目の 1 つは、例外が発生した場合は、最初の 5 つの挿入されたレコードをデータベースに残します。 同様に、バッチ更新パターンを使用して、挿入操作を実行する場合更新、および削除を挿入、変更、および以降には、以前、これらの変更、エラーが発生しました最初のいくつかの変更が成功した場合に、DataTable の行を削除します。完了した、データベース内になります。

特定のシナリオで、一連の変更で原子性を確保します。 実行する新しいメソッドを追加して、TableAdapter を手動で拡張する必要がありますこれを実現する、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`傘下のトランザクションの秒。 [データ アクセス層を作成する](../introduction/creating-a-data-access-layer-vb.md)を使用しました[部分クラス](http://en.wikipedia.org/wiki/Partial_type)型指定されたデータセット内の Datatable の機能を拡張します。 この手法は、Tableadapter にも使用できます。

型指定されたデータセット`Northwind.xsd`にある、`App_Code`フォルダーの`DAL`サブフォルダーです。 内のサブフォルダーを作成、`DAL`という名前のフォルダー`TransactionSupport`という名前の新しいクラス ファイルを追加および`ProductsTableAdapter.TransactionSupport.vb`(図 4 参照)。 このファイルは、の実装の一部を保持する、`ProductsTableAdapter`トランザクションを使用してデータの変更を実行するためのメソッドが含まれます。


![TransactionSupport をという名前のフォルダーと ProductsTableAdapter.TransactionSupport.vb をという名前のクラス ファイルを追加します。](wrapping-database-modifications-within-a-transaction-vb/_static/image4.gif)

**図 4**: という名前のフォルダーを追加`TransactionSupport`とという名前のクラス ファイル `ProductsTableAdapter.TransactionSupport.vb`


次のコードを入力してください、`ProductsTableAdapter.TransactionSupport.vb`ファイル。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample3.vb)]

`Partial`をコンパイラに追加する内で追加されたメンバーであることを示すクラスの宣言でキーワード、`ProductsTableAdapter`クラス、`NorthwindTableAdapters`名前空間。 注、`Imports System.Data.SqlClient`ファイルの上部にあるステートメント。 TableAdapter は、SqlClient プロバイダーを使用する構成された、以降内部的には、 [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx)オブジェクトをデータベースにそのコマンドを発行します。 そのため、使用する必要があります、`SqlTransaction`クラスは、トランザクションを開始し、コミットまたはロールバックします。 Microsoft SQL Server 以外のデータ ストアを使用している場合は、適切なプロバイダーを使用する必要があります。

これらのメソッドは、ロールバックを開始するために必要な構成要素を提供し、トランザクションをコミットします。 マークされている`Public`、内から使用することを有効にすると、 `ProductsTableAdapter`DAL では、別のクラスまたは BLL など、アーキテクチャの別のレイヤーから。 `BeginTransaction` 内部の tableadapter を開きます`SqlConnection`(必要な) 場合、トランザクションを開始およびに割り当てます、`Transaction`プロパティ、内部トランザクションをアタッチします`SqlDataAdapter`s`SqlCommand`オブジェクト。 `CommitTransaction` `RollbackTransaction`呼び出し、`Transaction`オブジェクト`Commit`と`Rollback`メソッド内部で終了する前に、それぞれ、`Connection`オブジェクト。

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>手順 3: トランザクションの傘下にデータを更新および削除メソッドの追加

これらのメソッドは完全なメソッドを追加 re 準備ができて`ProductsDataTable`または BLL 一連のトランザクションの傘下にコマンドを実行します。 次のメソッドは、バッチ更新パターンを使用して、更新、`ProductsDataTable`インスタンスのトランザクションを使用します。 トランザクションを開始するときに呼び出すことによって、`BeginTransaction`メソッドおよび、使用、`Try...Catch`データ変更ステートメントを発行するブロック。 場合に呼び出し、`Adapter`オブジェクト`Update`メソッドの結果、例外、実行は、転送、`catch`ブロックで、トランザクションはロールバックおよび再スローされる例外。 いることを思い出してください、`Update`メソッドの指定された行を列挙することによってバッチ更新パターンを実装する`ProductsDataTable`と実行のために必要な`InsertCommand`、 `UpdateCommand`、および`DeleteCommand`s。 いずれかのエラーでこれらのコマンドの結果で場合、トランザクションがロールバック、トランザクションの秒の有効期間中に加えられた以前の変更を元に戻します。 必要があります、`Update`ステートメントがエラーなく完了、トランザクションが完全にコミットします。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample4.vb)]

追加、`UpdateWithTransaction`メソッドを`ProductsTableAdapter`部分クラスに関連するクラス`ProductsTableAdapter.TransactionSupport.vb`します。 または、このメソッドは、ビジネス ロジック層 s に追加できませんでした`ProductsBLL`いくつかの軽微な構文の変更をクラス。 キーワードでは具体的には、`Me`で`Me.BeginTransaction()`、 `Me.CommitTransaction()`、および`Me.RollbackTransaction()`に置き換える必要があります`Adapter`(することを思い出してください`Adapter`内のプロパティの名前を指定します`ProductsBLL`型の`ProductsTableAdapter`)。

`UpdateWithTransaction`メソッドが、バッチ更新パターンを使用しますが、一連の DB 直接呼び出しは、次のメソッドのように、トランザクションのスコープ内でも使用できます。 `DeleteProductsWithTransaction`メソッドは入力として受け取ります、`List(Of T)`型の`Integer`、これは、`ProductID`削除します。 メソッドの呼び出しを使用してトランザクションを開始する`BeginTransaction`し、、`Try`ブロック、DB ダイレクト パターンを呼び出して、指定されたリストを反復処理`Delete`メソッドごとに`ProductID`値。 呼び出しのいずれか`Delete`が失敗に制御が移ります、`Catch`トランザクションのロールバックをブロックし、再スローされる例外。 呼び出す場合`Delete`トランザクションがコミットし、成功します。 このメソッドを追加、`ProductsBLL`クラス。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample5.vb)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>複数の Tableadapter にまたがるトランザクションを適用します。

に対する複数のステートメントにより、このチュートリアルで調査トランザクションに関連するコード、`ProductsTableAdapter`アトミック操作として扱われます。 しかし、アトミックに実行する別のデータベース テーブルに複数の変更が必要な場合でしょうか。 たとえば、カテゴリを削除するときにする可能性が最初に、現在の製品が他のカテゴリに再割り当ています。 分割不可能な操作として製品を再割り当てして、カテゴリを削除しています。 これら 2 つの手順を実行する必要があります。 `ProductsTableAdapter`を変更するためのメソッドのみが含まれています、`Products`テーブルおよび`CategoriesTableAdapter`を変更するためのメソッドのみが含まれています、`Categories`テーブル。 トランザクションはどのように両方の Tableadapter を取り込むことができますか。

メソッドを追加する方法が、`CategoriesTableAdapter`という名前の`DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)`製品を再割り当てし、ストアド プロシージャ内で定義されたトランザクションのスコープ内でカテゴリを削除するストアド プロシージャを呼び出してそのメソッドがあるとします。 今後のチュートリアルでは begin、commit、する方法とストアド プロシージャでのトランザクションをロールバックに注目します。

含む DAL でヘルパー クラスを作成することも、`DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)`メソッド。 このメソッドのインスタンスを作成、`CategoriesTableAdapter`と`ProductsTableAdapter`し、設定でこれら 2 つの Tableadapter`Connection`プロパティを同じ`SqlConnection`インスタンス。 At その時点では、2 つの Tableadapter のいずれかの開始、トランザクションへの呼び出しは`BeginTransaction`します。 製品を再割り当てすると、カテゴリを削除する Tableadapter のメソッドが呼び出される、`Try...Catch`コミットまたはロールバックのために必要なため、トランザクションをブロックします。

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>手順 4: 追加、`UpdateWithTransaction`メソッドをビジネス ロジック層

手順 3 で追加されました、`UpdateWithTransaction`メソッドを`ProductsTableAdapter`DAL でします。 BLL に対応するメソッドを追加する必要があります。 プレゼンテーション層は、DAL を呼び出すまで直接呼び出すことが中に、`UpdateWithTransaction`メソッドでは、これらのチュートリアルは、プレゼンテーション層から DAL の影響を受けないようにする、レイヤー アーキテクチャを定義する strived があります。 そのため、この方法を続行することをプレイヤーします。

開く、`ProductsBLL`クラス ファイルをという名前のメソッドを追加`UpdateWithTransaction`その単に呼び出し、対応する DAL メソッドまでです。 2 つの新しいメソッドはず`ProductsBLL`: `UpdateWithTransaction`、追加して`DeleteProductsWithTransaction`、手順 3. で追加されました。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample6.vb)]

> [!NOTE]
> これらのメソッドは含めないでください、`DataObjectMethodAttribute`の他のほとんどのメソッドに割り当てられている属性、`ProductsBLL`クラスのため、ASP.NET ページの分離コード クラスから直接これらのメソッドを呼び出すことができます。 いることを思い出してください`DataObjectMethodAttribute`どのような方法はウィザードと (SELECT、UPDATE、INSERT、または DELETE) は、どのようなタブの データ ソースの構成 ObjectDataSource 秒で表示される必要がありますにフラグを設定するために使用します。 GridView では、バッチを編集または削除の組み込みサポートがありません、ため、コーディングは宣言型のアプローチを使用するのではなく、プログラムでこれらのメソッドを呼び出すがあります。


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>手順 5: は、プレゼンテーション層からデータベースのデータをアトミックに更新

Let s が GridView のすべての製品一覧が表示され、ボタン Web が含まれますユーザー インターフェイスを作成するトランザクションがレコードのバッチを更新するときに及ぼす影響を示すためには、コントロールをクリックすると、製品を再割り当て`CategoryID`値。 そのため、最初のいくつかの製品は、有効な割り当てにカテゴリの再割り当てが具体的には、進行状況`CategoryID`意図的には他のユーザー値には、存在しないが割り当てられている`CategoryID`値。 製品と、データベースの更新を試みると持つ`CategoryID`s の既存のカテゴリと一致しません`CategoryID`、外部キー制約違反が発生し、例外が発生します。 この例では表示されるときに、前トランザクションの外部キー制約違反から発生する例外を使用すると、有効な`CategoryID`変更をロールバックします。 トランザクションを使用していないときにただし、最初のカテゴリへの変更は残ります。

開いて開始、`Transactions.aspx`ページで、`BatchData`フォルダーとツールボックスからデザイナーにドラッグする GridView。 設定の`ID`に`Products`し、スマート タグ、という名前の新しい ObjectDataSource にバインドする`ProductsDataSource`します。 構成からそのデータをプルする ObjectDataSource、`ProductsBLL`クラスの`GetProducts`メソッド。 これが読み取り専用の GridView、ので設定ドロップダウン リストでは、UPDATE、INSERT、および (None) にタブを削除され、[完了] をクリックします。


[![ObjectDataSource ProductsBLL クラスの GetProducts メソッドを使用して構成します。](wrapping-database-modifications-within-a-transaction-vb/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image3.png)

**図 5**: 構成に使用する ObjectDataSource、`ProductsBLL`クラス s`GetProducts`メソッド ([フルサイズの画像を表示する をクリックします](wrapping-database-modifications-within-a-transaction-vb/_static/image4.png))。


[![UPDATE、INSERT でドロップダウン リストを設定し、(なし) タブを削除します。](wrapping-database-modifications-within-a-transaction-vb/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image5.png)

**図 6**: (None) に、UPDATE、INSERT、および削除のタブで、ドロップダウン リストを設定 ([フルサイズの画像を表示する をクリックします](wrapping-database-modifications-within-a-transaction-vb/_static/image6.png))。


データ ソース構成ウィザードを完了すると、Visual Studio は BoundFields と製品のデータ フィールドの CheckBoxField 作成されます。 これらのフィールドを除くのすべてを削除`ProductID`、 `ProductName`、`CategoryID`と`CategoryName`の名前を変更し、`ProductName`と`CategoryName`BoundFields`HeaderText`製品およびカテゴリで、プロパティをそれぞれします。 スマート タグからページングを有効にするオプションをオンにします。 これらの変更を加えたら、GridView コントロールと ObjectDataSource s の宣言型マークアップは、次のようになります。


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample7.aspx)]

次に、GridView、上記の 3 つのボタンの Web コントロールを追加します。 グリッドの更新、(とトランザクションの変更のカテゴリに 2 つ目の s および変更のカテゴリ (トランザクションなし) に 3 つ目の 1 つの s を最初のボタンのテキスト プロパティを設定します。


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample8.aspx)]

この時点で Visual Studio でデザイン ビューのスクリーン ショット、図 7 に示すようなはずです。


[![ページには、GridView と 3 つのボタンの Web コントロールが含まれます。](wrapping-database-modifications-within-a-transaction-vb/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image7.png)

**図 7**: ページには、GridView と 3 つのボタンの Web コントロールが含まれています ([フルサイズの画像を表示する をクリックします](wrapping-database-modifications-within-a-transaction-vb/_static/image8.png))。


秒の 3 つのボタンの各イベント ハンドラーを作成`Click`イベントと、次のコードを使用します。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample9.vb)]

更新ボタン s`Click`イベント ハンドラーは、呼び出すことによって GridView にデータをバインドするだけです、 `Products` GridView の`DataBind`メソッド。

2 つ目のイベント ハンドラーは、製品を再割り当て`CategoryID`秒とは、データベースを実行する BLL から新しいトランザクション メソッドがトランザクションの傘下に更新します。 なお s の各製品`CategoryID`と同じ値に設定されている任意の`ProductID`します。 これはうまく機能、最初のいくつかの製品では、これらの製品があるため`ProductID`に有効なマップに値`CategoryID`秒。 1 回、`ProductID`肥大まず、この偶然の重複の`ProductID`s と`CategoryID`s が適用されなくなった。

3 番目`Click`イベント ハンドラーは、製品を更新`CategoryID`、同じ方法では、データベースを使用して、、更新を送信するが、 `ProductsTableAdapter` s 既定`Update`メソッド。 これは、`Update`メソッドは、一連のトランザクション内でのコマンドをラップしません、それらの変更が、最初の発生した外部キー制約違反エラーの前に行われるようにが保持されます。

この動作を示すためには、ブラウザーからこのページを参照してください。 最初に図 8 に示すようにデータの最初のページを参照する必要があります。 次に、変更のカテゴリ (とトランザクション) をクリックします。 ポストバックを発生させるし、すべての製品を更新しようとしています。 これは`CategoryID`、値が、外部キー制約違反になります (図 9 参照)。


[![ページングの GridView に、製品が表示されます。](wrapping-database-modifications-within-a-transaction-vb/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image9.png)

**図 8**: ページング可能な GridView の製品が表示されます ([フルサイズの画像を表示する をクリックします](wrapping-database-modifications-within-a-transaction-vb/_static/image10.png))。


[![外部キー制約に違反のカテゴリの結果を再割り当てします。](wrapping-database-modifications-within-a-transaction-vb/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image11.png)

**図 9**: 外部キー制約違反のカテゴリの結果を再割り当て ([フルサイズの画像を表示する をクリックします](wrapping-database-modifications-within-a-transaction-vb/_static/image12.png))。


ブラウザーの戻るボタンをクリックして、グリッドの更新 ボタンをクリックします。 データの更新時に図 8 に示すように、まったく同じ出力が表示されます。 でも、製品の一部が`CategoryID`s が法的に変更された値と、データベースの更新がロールバックされた外部キー制約違反が発生しました。

変更のカテゴリ (トランザクションなし) ボタンをクリックしてみましょう。 これは、同じ外部キー制約違反エラーになります (図 9 参照)、これらの製品が持つ`CategoryID`に有効な値が変更された値はロールバックされません。 ブラウザーの戻るボタンをクリックし、グリッドの更新 ボタンをクリックします。 図 10 に示すよう、`CategoryID`の最初の 8 つの製品を再割り当てされています。 たとえば、図 8 に変更が、 `CategoryID` 1、2 を図 10 it s での再割り当てが。


[![製品区分のいくつかの値が更新中に他のユーザーがいません](wrapping-database-modifications-within-a-transaction-vb/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image13.png)

**図 10**: 一部の製品`CategoryID`インポートされなかった値を更新中に他のユーザーが ([フルサイズの画像を表示する をクリックします](wrapping-database-modifications-within-a-transaction-vb/_static/image14.png))。


## <a name="summary"></a>まとめ

既定では、TableAdapter のメソッドでは、トランザクションのスコープ内で実行されるデータベース ステートメントをラップしないでくださいが、簡単な作業でメソッドを作成する、commit、およびトランザクションをロールバックするを追加できます。 このチュートリアルでのような 3 つのメソッドを作成しました、`ProductsTableAdapter`クラス: `BeginTransaction`、 `CommitTransaction`、および`RollbackTransaction`します。 これらのメソッドと共に使用する方法を説明しました、`Try...Catch`一連のデータ変更ステートメントをアトミックにブロックします。 具体的には、作成した、`UpdateWithTransaction`メソッドで、 `ProductsTableAdapter`、バッチ更新パターンを使用して、指定の行に必要な変更を実行するが`ProductsDataTable`します。 追加しました、`DeleteProductsWithTransaction`メソッドを`ProductsBLL`を受け取ると、BLL 内のクラス、`List`の`ProductID`DB ダイレクト パターン メソッドを呼び出して、入力として値を`Delete`各`ProductID`します。 どちらの方法は、トランザクションを作成し、内のデータ変更ステートメントを実行開始、`Try...Catch`ブロックします。 例外が発生する場合、トランザクションはロールバック、それ以外の場合にコミットされます。

手順 5 では、トランザクションを使用していなかったバッチ更新とトランザクション バッチの更新プログラムの効果を示します。 次の 3 つのチュートリアルはこのチュートリアルではレイアウトの基盤に構築し、バッチ更新、削除、および挿入を実行するためのユーザー インターフェイスを作成します。

満足のプログラミングです。

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [トランザクションでデータベースの整合性を維持します。](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [ストアド プロシージャの SQL Server のトランザクションを管理します。](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [トランザクションが簡単に作成できます。 `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope オブジェクトと Dataadapter](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [.NET での Oracle データベースのトランザクションの使用](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Dave Gardner、Hilton Giesenow が、および Teresa Murphy でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](batch-inserting-cs.md)
> [次へ](batch-updating-vb.md)
