---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
title: "データベースの変更 (VB) のトランザクション内での折り返し |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルでは、更新、削除、およびデータのバッチの挿入を参照する 4 つのうちの最初です。 このチュートリアルでは、データベース トランザクションを使用する方法を学習しました."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 7d821db5-6cbb-4b38-af14-198f9155fc82
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
msc.type: authoredcontent
ms.openlocfilehash: f054445091edbc27263127fb3b7b851776ec617f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="wrapping-database-modifications-within-a-transaction-vb"></a>トランザクション (VB) 内での折り返しデータベースの変更
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_VB.zip)または[PDF のダウンロード](wrapping-database-modifications-within-a-transaction-vb/_static/datatutorial63vb1.pdf)

> このチュートリアルでは、更新、削除、およびデータのバッチの挿入を参照する 4 つのうちの最初です。 このチュートリアルでは、データベース トランザクションが確実にすべての手順が成功したか、またはすべてのステップが失敗する、アトミックな操作として実行するバッチの変更を許可する方法お学習します。


## <a name="introduction"></a>はじめに

以降で説明したように、 [、概要の挿入、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)チュートリアルでは、GridView 組み込みのサポートを提供行レベルの編集および削除します。 マウスの数回のクリックでコンテンツを編集して、行ごとに削除する限り、行のコードを記述することがなく、豊富なデータ変更のインターフェイスを作成することができます。 ただし、特定のシナリオでこれでは不十分と編集、またはレコードのバッチを削除する機能をユーザーに提供する必要があります。

たとえば、web ベースの電子メール クライアントの最もグリッドを使用して、各メッセージを一覧表示する行ごとに電子メールの情報 (件名、送信者など) と共に チェック ボックスが含まれています。 このインターフェイスは、それらのチェックと、選択したメッセージの削除ボタンをクリックし、複数のメッセージを削除するユーザーを許可します。 編集インターフェイス バッチとは、ユーザーは、多くの異なるレコードよくを編集する場合に最適です。 ユーザーがクリックするのではなく編集、その変更を加えを変更する必要がある各レコードの更新 をクリックして、編集インターフェイス バッチが編集インターフェイスで各の行を表示します。 ユーザーでは、簡単に変更する必要がある行のセットを変更でき、更新プログラムのすべてのボタンをクリックしてこれらの変更を保存することができます。 このチュートリアルのセットに挿入する、編集、およびデータのバッチを削除するためのインターフェイスを作成する方法について確認します。

バッチ操作を実行するときにそれを決定するかどうかが考えられるいくつかの操作が成功するバッチの中に他の重要な失敗します。 インターフェイスの場合は最初、選択したレコードは正常に削除されますが、もう 1 つ失敗すると、たとえば、外部キー制約違反のための動作を削除するバッチを考慮しますか。 最初のレコードの削除ロールバックするかまたは最初のレコードが削除されたままで使用可能ですか。

バッチ操作として扱われる場合、[アトミック操作](http://en.wikipedia.org/wiki/Atomic_operation)、いずれかの場所か、すべてのステップが成功またはのすべてのステップが失敗すると、し、データ アクセス層をサポートするように拡張する必要があります[データベーストランザクション](http://en.wikipedia.org/wiki/Database_transaction)です。 データベースのトランザクションで、一連の原子性は保証`INSERT`、 `UPDATE`、および`DELETE`ステートメントがトランザクションの包括的な下で実行し、ほとんどすべての最新のデータベース システムでサポートされている機能であり、します。

このチュートリアルでは、データベース トランザクションを使用する、DAL を拡張する方法を紹介します。 以降のチュートリアルでは、挿入、更新、およびインターフェイスを削除するバッチの実装の web ページを確認します。 開始 s を let!

> [!NOTE]
> バッチのトランザクション内のデータを変更するときに常に原子性は必要ありません。 一部のシナリオでいくつかのデータ変更が正常に許容される場合があり、ときなど、失敗、同じバッチ内の他、web ベースの電子メール クライアントからの電子メールの設定を削除します。 S を削除してデータベース エラー中間にある、処理する場合、s がエラーなしで処理されるこれらのレコードが削除されたまま、許容できる可能性があります。 このような場合、DAL はデータベース トランザクションをサポートするように変更するのには必要ありません。 その他のバッチ操作シナリオ、ただし、原子性が不可欠があります。 顧客では、1 つの銀行口座間彼女資金を移動すると、は、2 つの操作を実行する必要があります。 資金を最初のアカウントから差し引かれます、、2 番目に追加する必要があります。 銀行可能性がありますいない気にすることは成功の最初の手順が、2 番目のステップが失敗する、ときに、お客様は当然不安を感じているになります。 お勧めこのチュートリアルを使用してバッチの挿入でそれらを使用して、更新、および次の 3 つのチュートリアルで構築するインターフェイスを削除する予定がない場合でも、データベース トランザクションをサポートするために DAL の機能強化を実装するためです。


## <a name="an-overview-of-transactions"></a>トランザクションの概要

ほとんどのデータベースがサポートされるよう*トランザクション*作業の 1 つの論理単位にグループ化する複数のデータベース コマンドを有効にします。 トランザクションを構成するデータベースのコマンドはアトミック、いずれかのすべてのコマンドが失敗したり、すべてが成功することは保証します。

一般に、トランザクションは、次のパターンを使用して SQL ステートメントを通じて実装されます。

1. トランザクションの開始を示します。
2. トランザクションを構成する SQL ステートメントを実行します。
3. 手順 2、ロールバック、トランザクションのステートメントのいずれかでエラーがある場合
4. エラーなしですべての手順 2 からステートメント完了、トランザクションをコミットします。

を作成するための SQL ステートメントのコミット、およびストアド プロシージャの SQL スクリプトを記述または作成するときに、トランザクションを手動で入力することができますをロールバック、またはプログラムで ADO.NET またはクラスでは、のいずれかを使用することを意味します、 [ `System.Transactions`名前空間](https://msdn.microsoft.com/library/system.transactions.aspx)です。 見ていきますのみ、このチュートリアルでは、ADO.NET を使用してトランザクションを管理します。 今後のチュートリアルでは、時に作成、ロールバック、およびトランザクションをコミットするための SQL ステートメントについて見ていきます、データ アクセス レイヤーでストアド プロシージャを使用する方法について取り上げます。 その間を参照してください[SQL Server ストアド プロシージャでのトランザクションの管理](http://www.4guysfromrolla.com/webtech/080305-1.shtml)詳細についてはします。

> [!NOTE]
> [ `TransactionScope`クラス](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx)で、`System.Transactions`名前空間により、開発者は、トランザクションのスコープ内で、一連のステートメントをプログラムでラップして、複数の関連する複雑なトランザクションのサポートが含まれています2 つの異なるデータベースまたはでも異種の種類の Microsoft SQL Server データベース、Oracle データベース、および Web サービスなどのデータ ストアなどのソース。 トランザクションを使用する ADO.NET の代わりに、このチュートリアルを決めたら、`TransactionScope`クラス ADO.NET をデータベース トランザクションとは、多くの場合、絞り込んだがはるかに少ないリソースを消費します。 さらに、特定のシナリオで、`TransactionScope`クラスは、Microsoft 分散トランザクション コーディネーター (MSDTC) を使用します。 構成、実装、およびパフォーマンスの問題周囲の MSDTC ではなく特殊および高度なトピックと、これらのチュートリアルの対象外です。


呼び出すことによってトランザクションが開始された ADO.NET での SqlClient プロバイダーを使用する場合、 [ `SqlConnection`クラス](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx)s [ `BeginTransaction`メソッド](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx)、返された、 [ `SqlTransaction`オブジェクト](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx)です。 構成のトランザクションが内に配置するデータ変更ステートメント、`try...catch`ブロックします。 内のステートメントでエラーが発生した場合、`try`への転送の実行をブロック、 `catch` 、トランザクションをロールバックを使用して、ブロック、`SqlTransaction`オブジェクト s [ `Rollback`メソッド](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx)です。 すべてのステートメントへの呼び出しで完全に成功する場合、`SqlTransaction`オブジェクト s [ `Commit`メソッド](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx)の最後に、`try`ブロックがトランザクションをコミットします。 次のコード スニペットは、このパターンを示しています。 参照してください[トランザクションでデータベースの整合性を維持する](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)構文と ADO.NET を使用してトランザクションを使用しての例を示します。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample1.vb)]

既定では、型指定されたデータセットに Tableadapter は、トランザクションを使用しません。 トランザクションのサポートを提供するには、一連のトランザクションのスコープ内でデータ変更ステートメントを実行する上記のパターンを使用する追加のメソッドを含む TableAdapter のクラスを拡張するために必要があります。 手順 2 は、部分クラスを使用して、これらのメソッドを追加する方法が表示されます。

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>手順 1: データのバッチ化された Web ページで、作業を作成します。

データベース トランザクションをサポートするために DAL を強化する方法の探索を開始して、前にまず、このチュートリアルが必要な ASP.NET web ページ、次の 3 つを作成するのにはしばらく s を使用できます。 という名前の新しいフォルダーを追加することによって開始`BatchData`し、次の ASP.NET ページで各ページの関連付けを追加、`Site.master`マスター ページ。

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![SqlDataSource に関連するチュートリアルについては、ASP.NET ページを追加します。](wrapping-database-modifications-within-a-transaction-vb/_static/image1.gif)

**図 1**: SqlDataSource に関連するチュートリアルについては、ASP.NET ページを追加します。


同様に、他のフォルダー`Default.aspx`を使用して、`SectionLevelTutorialListing.ascx`ユーザー コントロールをそのセクション内でチュートリアルを一覧表示します。 そのため、このユーザー コントロールを追加`Default.aspx`をページのデザイン ビューに、ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](wrapping-database-modifications-within-a-transaction-vb/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image1.png)

**図 2**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズのイメージを表示するをクリックして](wrapping-database-modifications-within-a-transaction-vb/_static/image2.png))


最後に、これら 4 つのページを追加するエントリとして、`Web.sitemap`ファイル。 具体的には、次のマークアップを追加、カスタマイズした後、サイト マップ`<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample2.xml)]

更新した後に`Web.sitemap`ブラウザーを使って、チュートリアルの web サイトを表示します。 左側のメニューには、バッチ化されたデータのチュートリアルを使用した作業項目が含まれています。


![サイト マップではバッチ化されたデータのチュートリアルを使用した作業のエントリが含まれるようになりました](wrapping-database-modifications-within-a-transaction-vb/_static/image3.gif)

**図 3**: サイト マップではバッチ化されたデータのチュートリアルを使用した作業のエントリが含まれるようになりました


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>手順 2: データベース トランザクションをサポートするために、データ アクセス層の更新

最初のチュートリアルで説明したよう[データ アクセス レイヤーを作成する](../introduction/creating-a-data-access-layer-vb.md)、当社の DAL で型指定されたデータセットはデータ テーブルと Tableadapter ので構成されます。 データ テーブルは、Tableadapter は、データをテーブルにするには、Datatable、およびなどに加えられた変更と、データベースを更新するにはデータベースからデータを読み取るための機能を提供中にデータを保持します。 Tableadapter がすればと呼ばれるバッチ更新と DB ダイレクト、データの更新の 2 つのパターンを提供することに注意してください。 バッチ更新パターンでは、TableAdapter はデータセット、データ テーブル、または Datarow のコレクションに渡されます。 このデータが列挙し、各挿入、変更、または行を削除、 `InsertCommand`、 `UpdateCommand`、または`DeleteCommand`を実行します。 DB ダイレクトのパターンを持つ、TableAdapter は、挿入、更新、または 1 つのレコードを削除するために必要な列の値を代わりに渡されます。 DB 直接的なパターン メソッドは、適切な実行をそれらの渡された値を使用して`InsertCommand`、 `UpdateCommand`、または`DeleteCommand`ステートメントです。

使用した更新パターンに関係なく、Tableadapter の自動生成されたメソッドは、トランザクションを使用しません。 既定では、各 insert、update、または、TableAdapter によって実行される delete が 1 つの独立した操作として扱われます。 たとえば、DB ダイレクト パターンを使用する、BLL の一部のコードで 10 個のレコードをデータベースに挿入想像してください。 このコードは、tableadapter を呼び出します`Insert`メソッドでその 10 倍です。 最初の 5 つの挿入が成功すると、6 番目の 1 つ例外が発生した場合は、データベース内の最初の 5 つ挿入したレコードが残っていません。 同様に、バッチ更新パターンを介して挿入操作を実行する場合に、更新、および削除、挿入を変更し、削除、DataTable の行最初のいくつかの変更が成功しましたが、後で、1 には、以前、これらの変更、エラーが発生しましたを。完了しましたが、データベースに残っていません。

特定のシナリオで一連の変更で原子性を確保することができます。 これを実行する新しいメソッドを追加することにより、TableAdapter を手動で拡張する必要がありますを実現する、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`トランザクションの傘下に s。 [データ アクセス レイヤーを作成する](../introduction/creating-a-data-access-layer-vb.md)を使用してきました[部分クラス](http://en.wikipedia.org/wiki/Partial_type)型指定されたデータセット内のデータ テーブルの機能を拡張します。 この方法は、Tableadapter にも使用できます。

型指定されたデータセット`Northwind.xsd`内にある、`App_Code`フォルダーの`DAL`サブフォルダーです。 内のサブフォルダーを作成、`DAL`という名前のフォルダー`TransactionSupport`という新しいクラス ファイルを追加および`ProductsTableAdapter.TransactionSupport.vb`(図 4 を参照してください)。 このファイルは、の実装の一部を保持する、`ProductsTableAdapter`にトランザクションを使用してデータの変更を実行するためのメソッドが含まれています。


![TransactionSupport をという名前のフォルダーと ProductsTableAdapter.TransactionSupport.vb をという名前のクラス ファイルを追加します。](wrapping-database-modifications-within-a-transaction-vb/_static/image4.gif)

**図 4**: という名前のフォルダーを追加`TransactionSupport`とという名前のクラス ファイル`ProductsTableAdapter.TransactionSupport.vb`


次のコードを入力してください、`ProductsTableAdapter.TransactionSupport.vb`ファイル。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample3.vb)]

`Partial`をコンパイラに追加する内で追加されたメンバーであることを示すクラスの宣言でキーワード、`ProductsTableAdapter`クラス内で、`NorthwindTableAdapters`名前空間。 注、`Imports System.Data.SqlClient`ファイルの上部にあるステートメントです。 TableAdapter は、SqlClient プロバイダーを使用する構成された、以降内部的に使用して、 [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx)データベースにそのコマンドを実行するオブジェクト。 そのため、使用する必要があります、`SqlTransaction`トランザクションを開始するクラスとそのコミットまたはロールバックするためにします。 Microsoft SQL Server 以外のデータ ストアを使用している場合は、適切なプロバイダーを使用する必要があります。

これらのメソッドは、ロールバックを開始するために必要な構成要素を提供し、トランザクションをコミットします。 マークされている`Public`、内から使用することを有効にすると、 `ProductsTableAdapter`DAL に別のクラスまたは BLL など、アーキテクチャの別のレイヤーからです。 `BeginTransaction`内部の tableadapter を開きます`SqlConnection`(必要な場合)、トランザクションを開始し、それを`Transaction`プロパティ、トランザクションを内部に添付`SqlDataAdapter`s`SqlCommand`オブジェクト。 `CommitTransaction`および`RollbackTransaction`を呼び出す、`Transaction`オブジェクト s`Commit`と`Rollback`メソッド、それぞれ、閉じてから、内部`Connection`オブジェクト。

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>手順 3: トランザクションの傘下にデータを更新および削除メソッドの追加

これらのメソッドは完全なメソッドを追加お re 準備ができて`ProductsDataTable`または BLL 一連のトランザクションの傘下にコマンドを実行します。 次のメソッドは、バッチ更新パターンを使用して、更新、`ProductsDataTable`インスタンスのトランザクションを使用します。 呼び出してトランザクションを開始、`BeginTransaction`メソッドおよび、使用、`Try...Catch`データ変更ステートメントを発行するブロック。 場合への呼び出し、`Adapter`オブジェクト s`Update`例外でメソッドの結果、実行に転送して、`catch`ここで、トランザクションはロールバックされますブロックと再スローされる例外。 注意してください、`Update`メソッドは、指定された行を列挙することで、バッチ更新パターンを実装`ProductsDataTable`、必要なを実行して`InsertCommand`、 `UpdateCommand`、および`DeleteCommand`s。 いずれかのエラーこれらのコマンドの結果で場合、トランザクションがロールバック、トランザクションの有効期間中に行われた以前の変更を元に戻します。 必要があります、`Update`エラーなしステートメントが完了すると、その完全な形でトランザクションをコミットします。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample4.vb)]

追加、`UpdateWithTransaction`メソッドを`ProductsTableAdapter`で部分クラスを通じてクラス`ProductsTableAdapter.TransactionSupport.vb`です。 また、このメソッドは、ビジネス ロジック層 s に追加できませんでした`ProductsBLL`多少構文の変更を持つクラス。 つまり、キーワード`Me`で`Me.BeginTransaction()`、`Me.CommitTransaction()`と`Me.RollbackTransaction()`で置き換える必要があります`Adapter`(ことに注意してください`Adapter`内のプロパティの名前を指定します`ProductsBLL`型の`ProductsTableAdapter`)。

`UpdateWithTransaction`メソッドは、バッチ更新パターンを使用しますが、一連の DB 直接呼び出しは、メソッドを次に示すように、トランザクションのスコープ内でも使用できます。 `DeleteProductsWithTransaction`メソッドが入力として受け取ります、`List(Of T)`型の`Integer`、これは、`ProductID`削除します。 メソッドへの呼び出しを使用してトランザクションを開始する`BeginTransaction`し、、`Try`ブロックは、DB ダイレクト パターンを呼び出して、指定されたリストを反復処理`Delete`ごとメソッド`ProductID`値。 呼び出しのいずれかの`Delete`失敗した場合に制御が移ります、`Catch`トランザクションのロールバックをブロックし、再スローされる例外。 すべての呼び出し場合`Delete`トランザクションがコミットが成功します。 このメソッドを追加、`ProductsBLL`クラスです。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample5.vb)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>複数の Tableadapter の間でトランザクションを適用します。

に対して複数のステートメントでは、このチュートリアルで確認トランザクションに関連するコード、`ProductsTableAdapter`分割不可能な操作として扱われます。 原子的に実行する別のデータベース テーブルに複数の変更が必要な場合ですか。 インスタンスのカテゴリを削除するときに可能性があります最初するその他のいくつかのカテゴリに現在の製品を再割り当ています。 分割不可能な操作として製品を再割り当てして、カテゴリを削除する 2 つの手順を実行する必要があります。 `ProductsTableAdapter`を変更する方法のみが含まれています、`Products`テーブルおよび`CategoriesTableAdapter`を変更する方法のみが含まれています、`Categories`テーブル。 トランザクションはどのように両方の Tableadapter を取り込むことができますか。

メソッドを追加するのには、1 つのオプション、`CategoriesTableAdapter`という`DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)`製品を再割り当てして、ストアド プロシージャ内で定義されたトランザクションのスコープ内のカテゴリを削除するストアド プロシージャを呼び出してそのメソッドがあるとします。 今後のチュートリアルでは、開始、コミットする方法、およびストアド プロシージャでのトランザクションをロールバックお見ていきます。

含む DAL でヘルパー クラスを作成することもできます、`DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)`メソッドです。 このメソッドのインスタンスを作成、`CategoriesTableAdapter`と`ProductsTableAdapter`2 つの Tableadapter に設定し、`Connection`プロパティを同じ`SqlConnection`インスタンス。 At その時点では、次の 2 つの Tableadapter のいずれかを呼び出してトランザクションを開始する`BeginTransaction`です。 製品を再割り当てすると、カテゴリを削除する Tableadapter のメソッドで呼び出される、`Try...Catch`コミットまたはロールバックに必要なトランザクションをブロックします。

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>手順 4: 追加、`UpdateWithTransaction`ビジネス ロジック層メソッド

手順 3 では追加、`UpdateWithTransaction`メソッドを`ProductsTableAdapter`DAL にします。 対応するメソッド、BLL に追加する必要があります。 プレゼンテーション層が、DAL を呼び出すまで直接呼び出し中に、`UpdateWithTransaction`メソッド、これらのチュートリアルは、プレゼンテーション層の DAL の影響を受けないようにする、レイヤー アーキテクチャを定義する strived がします。 そのため、この方法を引き続き behooves します。

開く、`ProductsBLL`クラス ファイルをという名前のメソッドを追加`UpdateWithTransaction`その単に呼び出し、対応する DAL メソッドまでです。 2 つの新しいメソッドはず`ProductsBLL`: `UpdateWithTransaction`、追加して`DeleteProductsWithTransaction`、手順 3. で追加されました。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample6.vb)]

> [!NOTE]
> これらのメソッドを含めないでください、`DataObjectMethodAttribute`の他のほとんどのメソッドに割り当てられている属性、`ProductsBLL`クラスため ASP.NET ページの分離コード クラスから直接、これらのメソッドが起動します。 注意してください`DataObjectMethodAttribute`どのような方法が表示されます、ObjectDataSource s で構成するデータ ソース ウィザード (SELECT、UPDATE、INSERT、または DELETE) は、どのようなタブの下にフラグを設定するために使用します。 GridView に編集または削除するバッチの組み込みサポートがないため、宣言型コードを必要としないアプローチを使用するのではなく、プログラムによってこれらのメソッドを呼び出すお必要があります。


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>手順 5: は、プレゼンテーション層からデータベースのデータをアトミックに更新

Let s を GridView のすべての製品を一覧表示し、ボタン Web を含むユーザー インターフェイスを作成するトランザクションがレコードのバッチを更新するときに及ぼす影響を示すためをクリックすると、制御、製品を再割り当て`CategoryID`値。 具体的には、カテゴリの再割り当てが進行状況の最初のいくつかの製品は有効な割り当てられるように`CategoryID`意図的にも値が存在しない`CategoryID`値。 製品で、データベースを更新しようかどうか持つ`CategoryID`s の既存のカテゴリと一致しません`CategoryID`、外部キー制約違反が発生し、例外が発生します。 この例を見てみましょうとなるのトランザクションの外部キー制約違反から発生する例外を使用すると、前の有効な`CategoryID`変更をロールバックできます。 トランザクションを使用していない場合はただし、最初のカテゴリへの変更は残ります。

開いて開始、 `Transactions.aspx`  ページで、`BatchData`フォルダーと、ツールボックスからデザイナーにドラッグする GridView。 設定の`ID`に`Products`と、という名前の新しい ObjectDataSource へのバインドのスマート タグから`ProductsDataSource`です。 構成からそのデータをプルする ObjectDataSource、`ProductsBLL`クラスの`GetProducts`メソッドです。 これは、読み取り専用の GridView、ので、UPDATE、INSERT でのドロップダウン リストの設定し (なし) にタブを削除してする完了 をクリックします。


[![ObjectDataSource ProductsBLL クラスの GetProducts メソッドを使用して構成します。](wrapping-database-modifications-within-a-transaction-vb/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image3.png)

**図 5**: 構成を使用する ObjectDataSource、`ProductsBLL`クラス s`GetProducts`メソッド ([フルサイズのイメージを表示するをクリックして](wrapping-database-modifications-within-a-transaction-vb/_static/image4.png))


[![UPDATE、INSERT のドロップダウン リストを設定し、(なし) タブを削除します。](wrapping-database-modifications-within-a-transaction-vb/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image5.png)

**図 6**: (なし) に、UPDATE、INSERT、および削除のタブで、ドロップダウン リストを設定 ([フルサイズのイメージを表示するをクリックして](wrapping-database-modifications-within-a-transaction-vb/_static/image6.png))


データ ソース構成ウィザードを完了すると、Visual Studio は、BoundFields と製品データ フィールドの CheckBoxField に作成されます。 これらのフィールドを除くすべて削除`ProductID`、 `ProductName`、`CategoryID`と`CategoryName`の名前を変更し、`ProductName`と`CategoryName`BoundFields`HeaderText`製品および分類プロパティをそれぞれします。 スマート タグからページングを有効にするオプションをオンにします。 このような変更を加えたら、GridView と ObjectDataSource s の宣言型マークアップは、次のようになります。


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample7.aspx)]

次に、GridView 上の 3 つのボタンの Web コントロールを追加します。 グリッドの更新、変更のカテゴリ (で TRANSACTION)、2 番目の s および変更のカテゴリ (トランザクションなし) に 3 つ目の 1 つの s を最初のボタンのテキスト プロパティを設定します。


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample8.aspx)]

この時点で Visual Studio でデザイン ビューは、スクリーン ショット図 7 に示すようになります。


[![ページには、GridView と、次の 3 つのボタン Web コントロールが含まれています。](wrapping-database-modifications-within-a-transaction-vb/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image7.png)

**図 7**: ページには、GridView と、次の 3 つのボタン Web コントロールが含まれています ([フルサイズのイメージを表示するをクリックして](wrapping-database-modifications-within-a-transaction-vb/_static/image8.png))


次の 3 つのボタン s の各イベント ハンドラーを作成`Click`イベントと、次のコードを使用します。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample9.vb)]

更新ボタン s`Click`イベント ハンドラーは、呼び出すことによって GridView にデータをバインドするだけで、 `Products` GridView の`DataBind`メソッドです。

2 つ目のイベント ハンドラーは、製品を再割り当て`CategoryID`s と、新しいトランザクション メソッド データベースを実行する BLL から更新トランザクションの傘下に使用します。 なお s の各製品`CategoryID`と同じ値に設定されている任意の`ProductID`します。 これはうまく機能、最初のいくつかの製品では、これらの製品があるため`ProductID`有効なにマップに値`CategoryID`s。 1 回、`ProductID`肥大 s スタートのこの偶然の一致の重なり`ProductID`s および`CategoryID`s が適用されなくなった。

3 番目`Click`イベント ハンドラーは、製品、更新`CategoryID`、同じ方法を使用して、データベースに、更新を送信するが、 `ProductsTableAdapter` s 既定`Update`メソッドです。 これは、`Update`メソッドは、一連のトランザクション内でのコマンドをラップしません、それらの変更が最初の発生の外部キー制約違反エラーの前に行われたようにが保持されます。

この動作を示すためには、ブラウザーからこのページを参照してください。 最初に図 8 に示すようにデータの最初のページが表示されます。 次に、変更のカテゴリ (でトランザクション) をクリックします。 ポストバックを発生させるし、すべての製品を更新しようとしています。 これは`CategoryID`、値が、外部キー制約違反になります (図 9 を参照してください)。


[![ページング可能な GridView に製品が表示されます。](wrapping-database-modifications-within-a-transaction-vb/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image9.png)

**図 8**: ページング可能な GridView に、製品が表示されます ([フルサイズのイメージを表示するをクリックして](wrapping-database-modifications-within-a-transaction-vb/_static/image10.png))


[![外部キー制約に違反カテゴリ結果を再割り当てします。](wrapping-database-modifications-within-a-transaction-vb/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image11.png)

**図 9**: 外部キー制約の違反でカテゴリの結果を再割り当て ([フルサイズのイメージを表示するをクリックして](wrapping-database-modifications-within-a-transaction-vb/_static/image12.png))


ブラウザーの 戻る ボタンをクリックして、グリッドの更新 ボタンをクリックします。 データの更新時に図 8 に示すように、まったく同じ出力が表示されます。 でもは、製品の一部が`CategoryID`s が法的に変更された値と、データベースの更新、それらがロールバックされた外部キー制約違反が発生しました。

今すぐ変更のカテゴリ (トランザクションなし) ボタンをクリックしてください。 これは、同じ外部キー制約違反エラーになります (図 9 を参照)、これらの製品が持つ`CategoryID`に有効な値が変更された値はロールバックされません。 ブラウザーの戻るボタンをクリックし、グリッドの更新ボタンに達してください。 図 10 に示す、`CategoryID`最初の 8 つの製品の s を再割り当てされています。 たとえば、図 8 に変更が、 `CategoryID` 1 が 2 に、図 10 it s されて再割り当ています。


[![一部の製品 CategoryID インポートされなかった値を更新中に他のユーザー](wrapping-database-modifications-within-a-transaction-vb/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image13.png)

**図 10**: 一部の製品`CategoryID`インポートされなかった値を更新中に他のユーザーが ([フルサイズのイメージを表示するをクリックして](wrapping-database-modifications-within-a-transaction-vb/_static/image14.png))


## <a name="summary"></a>まとめ

既定では、TableAdapter のメソッドでは、トランザクションのスコープ内で実行されるデータベース ステートメントをラップしないでくださいが簡単な作業で追加できるメソッドを作成する、commit、およびトランザクションをロールバックします。 このチュートリアルでのような 3 つのメソッドを作成しました、`ProductsTableAdapter`クラス: `BeginTransaction`、 `CommitTransaction`、および`RollbackTransaction`です。 これらのメソッドと共に使用する方法を説明しました、`Try...Catch`一連のデータ変更ステートメントをアトミックにブロックします。 具体的には、作成した、`UpdateWithTransaction`メソッドで、 `ProductsTableAdapter`、バッチ更新パターンを使用して、指定された行数に必要な変更を実行する`ProductsDataTable`です。 追加されています、`DeleteProductsWithTransaction`メソッドを`ProductsBLL`を受け入れると、BLL 内のクラス、`List`の`ProductID`DB ダイレクト パターン メソッドを呼び出して、入力として値を`Delete`各`ProductID`です。 両方のメソッドの開始、トランザクションを作成し、内のデータ変更ステートメントを実行、`Try...Catch`ブロックします。 例外が発生する場合、トランザクションがロールバック、それ以外の場合とコミットされます。

手順 5 では、トランザクションを使用していなかったバッチ更新とトランザクションのバッチ更新の影響を示します。 次の 3 つのチュートリアルでは、このチュートリアルに配置されている基盤に構築し、バッチ更新、削除、および挿入を実行するためのユーザー インターフェイスを作成おします。

満足プログラミング!

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [トランザクションでデータベースの整合性を維持します。](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [ストアド プロシージャを SQL Server のトランザクションを管理します。](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [トランザクションが簡単に作成します。`System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope と Dataadapter](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [.NET での Oracle データベースのトランザクションの使用](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者には、Dave ガードナー、Hilton Giesenow Teresa マーフィーがされていました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](batch-inserting-cs.md)
[次へ](batch-updating-vb.md)
