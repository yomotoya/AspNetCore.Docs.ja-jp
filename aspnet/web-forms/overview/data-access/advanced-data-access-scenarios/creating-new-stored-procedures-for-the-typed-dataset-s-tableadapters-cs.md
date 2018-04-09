---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: 型指定されたデータセットの Tableadapter (c#) のストアド プロシージャを新規作成 |Microsoft ドキュメント
author: rick-anderson
description: 前のチュートリアルでは、コードで SQL ステートメントを作成し、ステートメントを実行するデータベースに渡されるおがします。 別の方法が s を使用しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 751282ca-5870-4d66-84e4-6cefae23eb4a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: 3baff73d63cbc96a1a9f2222d077035cdd9a68f7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>型指定されたデータセットの Tableadapter (c#) のストアド プロシージャを新規に作成します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_CS.zip)または[PDF のダウンロード](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial67cs1.pdf)

> 前のチュートリアルでは、コードで SQL ステートメントを作成し、ステートメントを実行するデータベースに渡されるおがします。 その他の方法では、ストアド プロシージャを使用して、SQL ステートメントがデータベースに事前に定義された場所です。 このチュートリアルでは、TableAdapter ウィザードでの新しいストアド プロシージャを生成する方法について説明します。


## <a name="introduction"></a>はじめに

これらのチュートリアルのデータ アクセス層 (DAL) は、型指定されたデータセットを使用します。 説明したように、[データ アクセス レイヤーを作成する](../introduction/creating-a-data-access-layer-cs.md)チュートリアルでは、厳密に型指定された Datatable と Tableadapter の型指定されたデータセットで構成されます。 データ テーブルは、基になるデータベース、データにアクセスする作業を実行すると、Tableadapter インターフェイスの中に、システム内の論理エンティティを表します。 これには、データをデータ テーブルを設定する、スカラーのデータを返すクエリを実行し、挿入、更新、およびデータベースからレコードを削除するが含まれます。

Tableadapter によって実行される SQL コマンドでは、いずれか、アドホック SQL ステートメントをなどで`SELECT columnList FROM TableName`、またはストアド プロシージャです。 アーキテクチャでは、Tableadapter は、アドホック SQL ステートメントを使用します。 多くの開発者やデータベース管理者、ただし、セキュリティ、保守容易性、および updateability 上の理由から、アドホック SQL ステートメントをストアド プロシージャを優先します。 他のユーザー ardently、柔軟性、アドホック SQL ステートメントを使用します。 自分の作業でしましたが、アドホック SQL ステートメントをストアド プロシージャを最優先が、前のチュートリアルを簡単に、アドホック SQL ステートメントを使用することを選択します。

ときに TableAdapter を定義するか、新しいメソッドの追加、TableAdapter のウィザードと同様に簡単に新しいストアド プロシージャを作成したり、アドホック SQL ステートメントを使用すると、既存のストアド プロシージャを使用します。 このチュートリアルでは、TableAdapter s ウィザードが、ストアド プロシージャの自動生成する方法について確認します。 次のチュートリアルでは、または手動で作成された既存のストアド プロシージャを使用して TableAdapter のメソッドを構成する方法について取り上げます。

> [!NOTE]
> Rob Howard のブログ記事を参照してください[しないを使用してストアド プロシージャまだ?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/)と[Frans Bouma](https://weblogs.asp.net/fbouma/) s ブログ エントリ[ストアド プロシージャは、不良、M Kay?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx)の長所と短所をより効果的な議論のストアド プロシージャおよびアドホック SQL です。


## <a name="stored-procedure-basics"></a>ストアド プロシージャの基本

関数は、すべてのプログラミング言語に共通のコンス トラクターです。 関数は、関数が呼び出されたときに実行されるステートメントのコレクションです。 関数は、入力パラメーターを受け取ることができ、値を返すことが必要に応じて可能性があります。 *[ストアド プロシージャ](http://en.wikipedia.org/wiki/Stored_procedure)*データベース構造をプログラミング言語の関数で多くの類似点を共有します。 ストアド プロシージャは、ストアド プロシージャが呼び出されたときに実行される T-SQL ステートメントのセットで構成されます。 ストアド プロシージャは、多くの入力パラメーターに 0 を受け入れることがあり、出力パラメーター、スカラー値を返すことができます、ほとんどの場合、結果からの設定も`SELECT`クエリ。

> [!NOTE]
> 多くの場合にストアド プロシージャをストアド プロシージャまたは Sp と呼びます。


使用してストアド プロシージャが作成された、 [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) T-SQL ステートメントです。 たとえば、次の T-SQL スクリプトがという名前のストアド プロシージャを作成`GetProductsByCategoryID`という名前の単一パラメーターを受け取る`@CategoryID`を返します、 `ProductID`、 `ProductName`、 `UnitPrice`、および`Discontinued`のこれらの列のフィールド、`Products`には、対応するテーブル`CategoryID`値。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

このストアド プロシージャが作成されたら、呼び出すことができます、次の構文を使用します。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.sql)]

> [!NOTE]
> 次のチュートリアルでは、Visual Studio IDE を介してストアド プロシージャの作成を見ていきます。 このチュートリアルでは、ただし、しようとして、TableAdapter ウィザードで自動的にご利用の米国のストアド プロシージャを生成します。


に加えて、データを単に返すストアド プロシージャは、単一のトランザクションのスコープ内の複数のデータベース コマンドを実行するよく使用されます。 という名前のストアド プロシージャ`DeleteCategory`などかかる場合があります、`@CategoryID`パラメーター 2 を実行および`DELETE`ステートメント: 関連する製品と指定されたカテゴリを削除するもう 1 つを削除する 1 つは、1 つです。 ストアド プロシージャ内で複数のステートメントが*いない*トランザクション内で自動的にラップします。 追加の T-SQL コマンドを発行すると、ストアド プロシージャの複数のコマンドはアトミックな操作として扱われますことを確認する必要があります。 以降のチュートリアルでは、トランザクションのスコープ内でストアド プロシージャのコマンドをラップする方法を会いしましょう。

をアーキテクチャ内でストアド プロシージャを使用する場合、データ アクセス層のメソッドは、アドホック SQL ステートメントを発行するのではなく、特定のストアド プロシージャを呼び出します。 これには、アプリケーションのアーキテクチャ内で定義されていることはなく (on database) 実行される SQL ステートメントの場所が集中化します。 この集中管理はほぼ間違いないやすく検索、分析、および、クエリをチューニングし、データベースが使用されている場所と方法について程度わかりやすく図を提供します。

ストアド プロシージャの基礎の詳細については、このチュートリアルの最後に、関連項目セクション内のリソースを参照してください。

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>手順 1: 高度なデータ アクセス層のシナリオの Web ページの作成

ストアド プロシージャを使用して、DAL を作成する方法、ディスカッションを始める前に、まず、次のいくつかのチュートリアルとこれには必要な web サイト プロジェクトに、ASP.NET ページを作成するのにはしばらく s を使用できます。 という名前の新しいフォルダーを追加することによって開始`AdvancedDAL`です。 次に、各ページに関連付けるように、そのフォルダーに次の ASP.NET ページを追加、`Site.master`マスター ページ。

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![高度なデータ アクセス層のシナリオのチュートリアルについては、ASP.NET ページを追加します。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**図 1**: 高度なデータ アクセス層のシナリオのチュートリアルについては、ASP.NET ページを追加します。


などの他のフォルダーで`Default.aspx`で、`AdvancedDAL`フォルダーは、チュートリアルのセクションに一覧表示します。 注意してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 そのため、このユーザー コントロールを追加`Default.aspx`をページのデザイン ビューに、ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)

**図 2**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズのイメージを表示するをクリックして](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png))


最後に、これらのページを追加するエントリとして、`Web.sitemap`ファイル。 具体的には、次のマークアップを追加、データの操作をバッチ処理の後に`<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.xml)]

更新した後に`Web.sitemap`ブラウザーを使って、チュートリアルの web サイトを表示します。 左側のメニューには、高度な DAL シナリオのチュートリアルの項目が含まれています。


![サイト マップには、DAL の高度なシナリオのチュートリアルのエントリが含まれますようになりました](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)

**図 3**: サイト マップには、DAL の高度なシナリオのチュートリアルのエントリが含まれますようになりました


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>手順 2: ストアド プロシージャを新規作成する TableAdapter を構成します。

Let s をデモンストレーションするため、アドホック SQL ステートメントではなくストアド プロシージャを使用してデータ アクセス レイヤーを作成するには、新しい型指定されたデータセットを作成する、`~/App_Code/DAL`という名前のフォルダー`NorthwindWithSprocs.xsd`です。 このプロセスの詳細を前のチュートリアルでおに沿って説明しましたが、ためここに示す手順をすばやくお続行されます。 スタックを作成して、型指定されたデータセットを構成するのにはさらに詳細な手順を必要するかを参照、[データ アクセス レイヤーを作成する](../introduction/creating-a-data-access-layer-cs.md)チュートリアルです。

右クリックして、プロジェクトに新しいデータセットを追加、`DAL`フォルダー、新しい項目の追加 を選択して、図 4 に示すように、データセットのテンプレートを選択します。


[![NorthwindWithSprocs.xsd をという名前のプロジェクトに新しい型指定されたデータセットを追加します。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png)

**図 4**: プロジェクトの名前に新しい型指定されたデータセットを追加`NorthwindWithSprocs.xsd`([フルサイズのイメージを表示するをクリックして](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png))


これは、新しい型指定されたデータセットを作成、そのデザイナーを開き、新しい TableAdapter を作成および、TableAdapter 構成ウィザードを起動します。 TableAdapter 構成ウィザードの最初の手順を使用するデータベースを選択するよう求められます。 Northwind データベースへの接続文字列は、ドロップダウン リストに表示する必要があります。 このオプションを選択し、[次へ] をクリックします。

この次の画面から TableAdapter がデータベースにアクセスする方法を選択できます。 前のチュートリアルは、最初のオプションを使用する SQL ステートメントを選択します。 このチュートリアルでは、2 番目のオプションを選択して、新しいストアド プロシージャの作成および [次へ] をクリックします。


[![新しいストアド プロシージャを作成する TableAdpater を指示します。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png)

**図 5**: TableAdpater 新しいストアド プロシージャを作成するように指示 ([フルサイズのイメージを表示するをクリックして](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png))


同様に、アドホック SQL ステートメントを使用すると、次の手順では入力を求められる、 `SELECT` TableAdapter のメイン クエリのステートメント。 使用する代わりに、`SELECT`ここに入力したアドホック クエリを直接実行するステートメントでは、TableAdapter のウィザードには、これを含むストアド プロシージャが作成されます`SELECT`クエリ。

以下を使用して`SELECT`この TableAdapter のクエリ。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]


[![SELECT クエリを入力してください。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png)

**図 6**: 入力、`SELECT`クエリ ([フルサイズのイメージを表示するをクリックして](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png))


> [!NOTE]
> 上記のクエリとは若干異なりますのメインのクエリ、`ProductsTableAdapter`で、`Northwind`型指定されたデータセット。 注意してください、`ProductsTableAdapter`で、`Northwind`型指定されたデータセットには、カテゴリ名と各製品のカテゴリと仕入先の会社名に戻すに 2 つの相関サブクエリが含まれています。 次回に[TableAdapter に更新を使用して結合](updating-the-tableadapter-to-use-joins-cs.md)この TableAdapter に関連するデータのチュートリアルはこれを追加することを見ていきましょう。


すぐを高度なオプション をクリックします。 ここからかどうか、ウィザード必要がありますも挿入、更新、および delete ステートメントを生成、TableAdapter をオプティミスティック同時実行を使用するか、挿入と更新後、データ テーブルを更新するかどうかを指定できます。 生成する Insert、Update および Delete ステートメントのオプションは既定でオンにします。 オンのままにします。 このチュートリアルでは、オプティミスティック同時実行制御オプションを使用してをオフのままにします。

TableAdapter ウィザードで自動的に作成したストアド プロシージャを持つ、更新、データ テーブルのオプションが無視されることが表示されます。 このチェック ボックスがオンになっているかどうか、結果の挿入と更新に関係なくは、ストアド プロシージャは、手順 3. で紹介するようにだけ挿入または単に更新されるレコードを取得します。


![生成 Insert、Update および Delete ステートメントのオプションをオンのままにしてください。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png)

**図 7**: 生成 Insert、Update および Delete のステートメントのオプションをオンのままにして


> [!NOTE]
> ウィザードで追加条件が追加されますを使用するオプティミスティック同時実行制御オプションをオンにした場合、`WHERE`データが他のフィールドの変更があった場合に更新されないようにする句。 戻って、[オプティミスティック同時実行制御で実装する](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md)TableAdapter s 組み込みオプティミスティック同時実行制御機能を使用する方法についてのチュートリアルです。


入力した後に、`SELECT`クエリを実行し、[次へ] を生成する Insert、Update および Delete ステートメントのオプションがオンになっていることを確認するには、します。 図 8 に示すようにこの次の画面を選択すると、挿入、更新、およびデータを削除するを作成するストアド プロシージャの名前のメッセージが表示されます。 これらのストアド プロシージャ名を変更`Products_Select`、 `Products_Insert`、 `Products_Update`、および`Products_Delete`です。


[![ストアド プロシージャの名前を変更します。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**図 8**: ストアド プロシージャの名前を変更 ([フルサイズのイメージを表示するをクリックして](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


T-SQL、4 つのストアド プロシージャの作成に使用、TableAdapter ウィザードを表示するには、SQL スクリプトのプレビュー ボタンをクリックします。 SQL スクリプトのプレビュー ダイアログ ボックスからスクリプトをファイルに保存またはをクリップボードにコピーする可能性があります。


![ストアド プロシージャの生成に使用する SQL スクリプトをプレビューします。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**図 9**: ストアド プロシージャの生成に使用される SQL スクリプトのプレビュー


ストアド プロシージャの名前を付けた後に、対応するメソッドの名前、tableadapter の横にあるをクリックします。 場合、アドホック SQL ステートメントを使用すると同じように既存の DataTable にデータを新しいものを返さないメソッドを作成できます。 TableAdapter が挿入、更新、およびレコードの削除 DB ダイレクト パターンを含めるかどうかも指定できます。 すべての 3 つチェック ボックスをオンのままにして、DataTable メソッドに戻り値の名前を変更`GetProducts`に示すように図 10)。


[![メソッドの名前を付けます塗りつぶしと GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)

**図 10**: メソッドの名前を付けます`Fill`と`GetProducts`([フルサイズのイメージを表示するをクリックして](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png))


ウィザードの手順の概要を表示するには、次へ をクリックします。 [完了] ボタンをクリックして、ウィザードを完了します。 ウィザードが完了すると、戻りますデータセット デザイナーで、含める必要があります今すぐに、`ProductsDataTable`です。


[![DataSet の s デザイナーは、新しく追加された ProductsDataTable を示しています。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)

**図 11**: 新しく追加された DataSet の s デザイナーを示しています`ProductsDataTable`([フルサイズのイメージを表示するをクリックして](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>手順 3: 新しく作成されたストアド プロシージャを確認します。

手順 2 で自動的に使用される、TableAdapter ウィザードを選択すると、挿入、更新、およびデータを削除するためのストアド プロシージャが作成されます。 これらのストアド プロシージャは、Visual Studio でサーバー エクスプ ローラーに移動し、データベースの s Stored Procedures フォルダーへのドリル ダウンの変更または表示できます。 Northwind データベースには図 12 に示す 4 つの新しいストアド プロシージャが含まれています: `Products_Delete`、 `Products_Insert`、 `Products_Select`、および`Products_Update`です。


![データベースのストアド プロシージャのフォルダーに手順 2. で作成された 4 つのストアド プロシージャが見つかりません](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)

**図 12**: データベースのストアド プロシージャのフォルダーに手順 2. で作成された 4 つのストアド プロシージャが見つかりません


> [!NOTE]
> サーバー エクスプ ローラーが表示されない場合は、[表示] メニューに移動し、サーバー エクスプ ローラーのオプションを選択します。 手順 2 から追加された製品に関連するストアド プロシージャが表示されない場合は try Stored Procedures フォルダーを右クリックして選択を更新します。


表示またはストアド プロシージャの変更、サーバー エクスプ ローラーでその名前をダブルクリックして、代わりに、ストアド プロシージャを右クリックし、開く を選択 図 13 を示しています、`Products_Delete`開かれたときに、ストアド プロシージャです。


[![ストアド プロシージャを開くし、Visual Studio 内からに変更されました](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png)

**図 13**: ストアド プロシージャが開くことができ、変更から内で Visual Studio ([フルサイズのイメージを表示するをクリックして](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png))


両方のコンテンツ、`Products_Delete`と`Products_Select`ストアド プロシージャは、かなり簡単です。 `Products_Insert`と`Products_Update`ストアド プロシージャは、その一方で、どちらも実行するために、よく検査を立証、`SELECT`後のステートメント、`INSERT`と`UPDATE`ステートメントです。 たとえば、次の SQL は、構成、`Products_Insert`ストアド プロシージャ。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

入力パラメーターとしてストアド プロシージャでは、`Products`列によって返された、 `SELECT` TableAdapter のウィザードとこれらの値で指定されたクエリで使用される、`INSERT`ステートメントです。 次の`INSERT`ステートメントでは、`SELECT`を返すクエリが使用される、`Products`列の値 (など、 `ProductID`)、新しく追加されたレコードのです。 この更新機能は、新しく追加された更新を使用して、バッチ更新パターン、自動的に新しいレコードを追加する際に役立ちます`ProductRow`インスタンス`ProductID`データベースによって割り当てられた自動インクリメント値を持つプロパティです。

次のコードは、この機能を示しています。 含まれている、`ProductsTableAdapter`と`ProductsDataTable`用に作成された、`NorthwindWithSprocs`型指定されたデータセット。 新しい製品が作成することで、データベースに追加、`ProductsRow`インスタンス、その値を指定して、tableadapter を呼び出して`Update`に渡して、メソッド、`ProductsDataTable`です。 Tableadapter では内部的には、`Update`メソッドは、列挙、`ProductsRow`渡されたデータ テーブル内のインスタンス (この例では 1 つだけに追加したばかりのいずれか) を実行し、適切な挿入、更新、または delete コマンド。 ここで、`Products_Insert`ストアド プロシージャを実行すると、新しいレコードを追加、`Products`テーブルが表示され、新しく追加されたレコードの詳細を返します。 `ProductsRow`インスタンスの`ProductID`値が更新されます。 後に、`Update`メソッドが完了したら、新しく追加されたレコード s アクセスできる`ProductID`を通じて値、 `ProductsRow` s`ProductID`プロパティです。


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.cs)]

`Products_Update`ストアド プロシージャが同様に含まれています、`SELECT`ステートメントの後にその`UPDATE`ステートメントです。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample7.sql)]

注このストアド プロシージャには、2 つの入力パラメーターが含まれています。 `ProductID`:`@Original_ProductID`と`@ProductID`です。 この機能により、シナリオの主キーを変更する可能性があります。 たとえば、従業員データベースの各従業員レコード可能性があります従業員の社会保障番号として使用主キー。 既存の従業員の社会保障番号を変更するために新しい社会保障番号と元の両方を指定する必要があります。 `Products`ために、テーブル、そのような機能は必要ありません、`ProductID`列は、`IDENTITY`列変更不可能とします。 実際には、`UPDATE`内のステートメント、`Products_Update`ストアド プロシージャにはではありませんが含まれて、`ProductID`その列リスト内の列です。 ときに、`@Original_ProductID`で使用される、`UPDATE`ステートメント`WHERE`句はの余分な`Products`テーブルし、置き換えられる可能性があります、`@ProductID`パラメーター。 ストアド プロシージャのパラメーターを変更するときは、そのストアド プロシージャを使用して TableAdapter メソッドも更新されていることが重要です。

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>手順 4: ストアド プロシージャのパラメーターを変更して、TableAdapter の更新

以降、`@Original_ProductID`パラメーターが不要な let s を削除してから、`Products_Update`ストアド プロシージャは完全です。 開いている、`Products_Update`ストアド プロシージャを削除、`@Original_ProductID`パラメーター、および、`WHERE`の句、`UPDATE`ステートメントから使用されるパラメーターの名前を変更`@Original_ProductID`に`@ProductID`です。 これらの変更を加えたら、ストアド プロシージャ内で T-SQL は、次のようになります。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample8.sql)]

これらの変更をデータベースに保存、ツールバーの [保存] アイコンをクリックするか Ctrl キーを押しながら S キーをヒットします。 この時点で、`Products_Update`ストアド プロシージャは想定していない、`@Original_ProductID`入力パラメーターは、TableAdapter は、このようなパラメーターを渡すように構成します。 TableAdapter を送信するパラメーターを表示できます、`Products_Update`ストアド プロシージャを TableAdapter をデータセット デザイナーでを選択し、[プロパティ] ウィンドウで省略記号をクリックして、 `UpdateCommand` s`Parameters`コレクション。 図 14 に示すようにパラメーター コレクション エディター ダイアログ ボックスが表示されます。


![パラメーター コレクション エディターのリストに使用されるパラメーターは、Products_Update に渡されるストアド プロシージャ](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png)

**図 14**: にパラメーター コレクション エディターのリストに使用されるパラメーターが渡される、`Products_Update`ストアド プロシージャ


ここから選択するだけで、このパラメーターを削除することができます、`@Original_ProductID`パラメーターから一連のメンバーと削除 をクリックします。

または、デザイナーの TableAdapter を右クリックし、構成を選択するすべてのメソッドに使用されるパラメーターを更新することができます。 選択すると、挿入、更新、使用するストアド プロシージャを一覧表示する、TableAdapter 構成ウィザードが表示されます、パラメータと共に削除するストアド プロシージャを受信することです。 更新プログラムのドロップダウン リストをクリックすることがわかります、`Products_Update`ストアド プロシージャが必要、入力パラメーターを今すぐが含まれなくなりました`@Original_ProductID`(図 15 を参照してください)。 TableAdapter で使用されるパラメーターのコレクションを自動的に更新するには、[完了] をクリックします。


[![そのメソッドのパラメーターのコレクションを更新するのに TableAdapter の構成ウィザードを使用することができますまたは](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**図 15**: または tableadapter のメソッドのパラメーター コレクションの更新の構成ウィザードを使用することができます ([フルサイズのイメージを表示するをクリックして](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>手順 5: 追加の TableAdapter のメソッドを追加します。

手順 2 を示す、として新しい TableAdapter を作成するときに簡単に対応するストアド プロシージャを自動的に生成します。 TableAdapter に追加のメソッドを追加するときにも同様です。 これを示すためには、秒を追加できるように、`GetProductByProductID(productID)`メソッドを`ProductsTableAdapter`手順 2. で作成します。 このメソッドが受け取る入力として、`ProductID`値し、指定された製品に関する詳細を確認します。

TableAdapter を右クリックして、コンテキスト メニューの [クエリの追加] を選択して開始します。


![TableAdapter に新しいクエリを追加します。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**図 16**: TableAdapter に新しいクエリを追加


これにより、TableAdapter がデータベースにアクセスする方法を最初に要求すると、TableAdapter クエリの構成ウィザードが開始されます。 作成した新しいストアド プロシージャには、作成にストアド プロシージャの新しいオプションを選択し、[次へ] をクリックします。


[![作成、新しいストアド プロシージャ オプションの選択します。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)

**図 17**: 作成、新しいストアド プロシージャ オプションの選択 ([フルサイズのイメージを表示するをクリックして](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png))


次の画面には、クエリを実行するには、一連の行または 1 つのスカラー値を返すを実行するかどうかの種類を識別するよう求められます、 `UPDATE`、 `INSERT`、または`DELETE`ステートメントです。 以降、`GetProductByProductID(productID)`メソッドは、行を返すを行のオプションが選択されており、次へのヒットを返す SELECT のままにします。


[![選択 オプションの行が返されます](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)

**図 18**: 選択オプションの行が返されます ([フルサイズのイメージを表示するをクリックして](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png))


次の画面には、TableAdapter の s メイン クエリには、ストアド プロシージャの名前を一覧表示だけが表示されます (`dbo.Products_Select`)。 ストアド プロシージャの名前を次に置き換えます`SELECT`ステートメントでは、すべての指定した製品の製品フィールドが返されます。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample9.sql)]


[![SELECT クエリでストアド プロシージャの名前を置き換えます](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)

**図 19**: でストアド プロシージャ名を置き換える、`SELECT`クエリ ([フルサイズのイメージを表示するをクリックして](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png))


後続の画面では、作成されるストアド プロシージャの名前が求められます。 名前を入力します`Products_SelectByProductID`[次へ] をクリックします。


[![新しいストアド プロシージャ Products_SelectByProductID の名前](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**図 20**: 新しいストアド プロシージャの名前を付けます`Products_SelectByProductID`([フルサイズのイメージを表示するをクリックして](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image46.png))


ウィザードの最後の手順により、メソッド名が生成だけでなく、塗りつぶしを使用するかどうかを示すために DataTable パターンを変更するには、DataTable パターンでは、またはその両方を返します。 このメソッドの両方のオプションがオンのままには、メソッドの名前を変更`FillByProductID`と`GetProductByProductID`です。 ウィザードを実行し、ウィザードを完了するには、[完了] をクリックし、手順の概要を表示するには、[次へ] をクリックします。


[![FillByProductID および GetProductByProductID TableAdapter のメソッドの名前を変更します。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image47.png)

**図 21**: する TableAdapter の s メソッドの名前を変更`FillByProductID`と`GetProductByProductID`([フルサイズのイメージを表示するをクリックして](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image49.png))


TableAdapter ウィザードを完了すると、使用可能な新しいメソッドがあります`GetProductByProductID(productID)`、呼び出されると、実行、`Products_SelectByProductID`ストアド プロシージャで作成されました。 Stored Procedures フォルダーへのドリル インを開くと、サーバー エクスプ ローラーからこの新しいストアド プロシージャを表示するのにはしばらく時間かかる`Products_SelectByProductID`(は表示されない場合に、ストアド プロシージャ フォルダーを右クリックしておよび更新 を選択) します。

注意してください、`SelectByProductID`ストアド プロシージャは`@ProductID`入力パラメーターとして実行し、`SELECT`ウィザードで入力してステートメントです。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>手順 6: ビジネス ロジック層クラスを作成します。

一連のチュートリアル全体では、プレゼンテーション層がビジネス ロジック層 (BLL) への呼び出しのすべてに加え、レイヤー アーキテクチャを維持するために strived おがします。 この設計の決定に準拠するために最初にプレゼンテーション層から製品データにアクセスして前に、新しい型指定されたデータセットの BLL クラスを作成する必要があります。

という新しいクラス ファイルを作成する`ProductsBLLWithSprocs.cs`で、`~/App_Code/BLL`フォルダーし、次のコードを追加します。


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample11.cs)]

このクラスを模倣、`ProductsBLL`クラス使用する点が、前のチュートリアルのセマンティクスとは、`ProductsTableAdapter`と`ProductsDataTable`オブジェクトから、`NorthwindWithSprocs`データセット。 例ではなく、`using NorthwindTableAdapters`としてクラス ファイルの先頭にステートメント`ProductsBLL`、`ProductsBLLWithSprocs`クラスの使用`using NorthwindWithSprocsTableAdapters`です。 同様に、`ProductsDataTable`と`ProductsRow`このクラスで使用されるオブジェクトが付いた、`NorthwindWithSprocs`名前空間。 `ProductsBLLWithSprocs`クラスは、2 つのデータ アクセス方法、`GetProducts`と`GetProductByProductID`、メソッドとメソッドを追加、更新、および 1 つの製品のインスタンスを削除します。

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>手順 7: 操作、`NorthwindWithSprocs`プレゼンテーション層からのデータセット

この時点でストアド プロシージャを使用してアクセスし、基になるデータベースのデータを変更する DAL を作成しました。 すべての製品またはメソッドを追加、更新、および特定の製品と製品の削除を取得するメソッドと基本的な BLL もビルドしました。 このチュートリアルの丸め、let s 作成 BLL s を使用する ASP.NET ページ`ProductsBLLWithSprocs`表示、更新、およびレコードを削除するためのクラスです。

開く、 `NewSprocs.aspx`  ページで、`AdvancedDAL`フォルダーとその名前を付け、デザイナーには、ツールボックスからドラッグ GridView`Products`です。 GridView から s スマート タグをという名前の新しい ObjectDataSource にバインドする選択`ProductsDataSource`です。 構成を使用する ObjectDataSource、`ProductsBLLWithSprocs`クラス、22 の図に示すようにします。


[![構成、ObjectDataSource ProductsBLLWithSprocs クラスを使用するには](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image50.png)

**図 22**: 構成を使用する ObjectDataSource、`ProductsBLLWithSprocs`クラス ([フルサイズのイメージを表示するをクリックして](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image52.png))


タブで、ドロップダウン リストが 2 つのオプション、`GetProducts`と`GetProductByProductID`です。 GridView にすべての製品を表示するので、選択、`GetProducts`メソッドです。 UPDATE、INSERT、および DELETE の各のタブで、ドロップダウン リストでは、1 つのメソッドがあるだけです。 これらの各ドロップダウン リストが選択されている、適切なメソッドであることを確認し、[完了] をクリックします。

ObjectDataSource ウィザードが完了した後、Visual Studio は、製品データ フィールドの GridView に BoundFields と、CheckBoxField を追加します。 編集を有効にして削除を有効にするオプションをスマート タグの存在をチェックして GridView s の組み込みの編集と削除機能に有効にします。


[![ページには、編集および削除のサポートが有効な GridView が含まれます。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image53.png)

**図 23**: ページに含まれる GridView に編集および削除のサポートを有効に ([フルサイズのイメージを表示するをクリックして](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image55.png))


おとして説明した、ObjectDataSource のウィザードの完了時の前のチュートリアルでは、Visual Studio の設定、`OldValuesParameterFormatString`プロパティを元に\_{0}。 これは、必要があります、データ変更の機能の順序で {0} の既定値に戻ります正しく、BLL のメソッドで想定されているパラメーターを指定します。 そのため、設定することを確認する、 `OldValuesParameterFormatString` {0} プロパティまたはプロパティを宣言構文から完全に削除します。

編集および GridView でサポートを削除して、ObjectDataSource s を返す有効にする、データ ソースの構成ウィザードの完了後`OldValuesParameterFormatString`プロパティを既定値は、ページ s 宣言型マークアップは、次のようになります。


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample12.aspx)]

この時点では、検証を追加、編集インターフェイスをカスタマイズすることにより、GridView を整理おでしたが、`CategoryID`と`SupplierID`列 DropDownLists、として表示したりできます。 クライアント側の確認 [削除] にも追加できるし、すると、これらの拡張機能の実装に時間がかかることをお勧めします。 これらのトピックは、前のチュートリアルで説明しています、のでただし、いないにもう一度ここで説明します。

GridView を強化するはかどうかには、かに関係なく、ブラウザーでページ コア機能をテストします。 図 24 では、ページには、行ごとの編集と削除機能を提供する GridView の製品が一覧表示します。


[![製品を表示、編集、および GridView から削除できます。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image56.png)

**図 24**:、製品を表示できます、編集、および GridView から削除 ([フルサイズのイメージを表示するをクリックして](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image58.png))


## <a name="summary"></a>まとめ

型指定されたデータセットに Tableadapter やストアド プロシージャ、アドホック SQL ステートメントを使用して、データベースからデータにアクセスできます。 ストアド プロシージャに基づくのときにストアド プロシージャの使用、か既存のストアド プロシージャを使用したり、TableAdapter ウィザードは新規作成するように指定することができます、`SELECT`クエリ。 このチュートリアルでは、ストアド プロシージャをご利用の米国が自動的に作成する方法について説明します。

ストアド プロシージャによりの自動生成された時間を節約するとき、ことがウィザードでは t によって作成されるストアド プロシージャが、独自のおの作成と連携します。 1 つの例は、`Products_Update`ストアド プロシージャで、両方が必要です`@Original_ProductID`と`@ProductID`にもかかわらずの入力パラメーター、`@Original_ProductID`パラメーターが不要です。

多くのシナリオでストアド プロシージャが既に作成されている、またはある細かくストアド プロシージャのコマンドが細かく制御するために手動でそれらを構築したい場合があります。 どちらの場合は、TableAdapter のメソッドに既存のストアド プロシージャを使用するように指示をします。 次のチュートリアルでこれを実現する方法がわかりますいてはいけない。

満足プログラミング!

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ストアド プロシージャを作成および保守します。](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [ストアド プロシージャからのスカラー データの取得](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [SQL Server のストアド プロシージャの基本](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [ストアド プロシージャ: 概要](http://www.sqlteam.com/item.asp?ItemID=563)
- [ストアド プロシージャを作成](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Hilton Geisenow しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [次へ](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
