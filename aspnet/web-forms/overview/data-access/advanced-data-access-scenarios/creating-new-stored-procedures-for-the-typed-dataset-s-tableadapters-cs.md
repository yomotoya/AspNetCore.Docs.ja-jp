---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: ストアド プロシージャ、型指定されたデータセットの Tableadapter (c#) を新規作成 |Microsoft Docs
author: rick-anderson
description: 前のチュートリアル、コードで SQL ステートメントを作成し、ステートメントを実行するデータベースに渡されるしました。 その他の方法では、秒を使用する.
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 751282ca-5870-4d66-84e4-6cefae23eb4a
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: 0932749d6cf1665eedd5f452ab5dd63ed8678962
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833972"
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>ストアド プロシージャ、型指定されたデータセットの Tableadapter (c#) を新規作成
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_CS.zip)または[PDF のダウンロード](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial67cs1.pdf)

> 前のチュートリアル、コードで SQL ステートメントを作成し、ステートメントを実行するデータベースに渡されるしました。 その他の方法では、SQL ステートメントがデータベースで定義済みストアド プロシージャを使用します。 このチュートリアルでは、TableAdapter ウィザードでの新しいストアド プロシージャを生成する方法について説明します。


## <a name="introduction"></a>はじめに

これらのチュートリアルのデータ アクセス層 (DAL) は、型指定されたデータセットを使用します。 説明したように、[データ アクセス層を作成する](../introduction/creating-a-data-access-layer-cs.md)チュートリアルでは、厳密に型指定された Datatable および Tableadapter の型指定されたデータセットで構成されます。 データ テーブルは、基になるデータベース、データにアクセスする作業を実行すると、Tableadapter インターフェイスの中に、システム内の論理エンティティを表します。 これには、データと Datatable の作成、スカラーのデータを返すクエリを実行し、挿入、更新、およびデータベースからレコードを削除するが含まれます。

Tableadapter によって実行される SQL コマンドでは、アドホック SQL ステートメントのいずれかをなどある`SELECT columnList FROM TableName`、またはストアド プロシージャ。 アーキテクチャで Tableadapter では、アドホック SQL ステートメントを使用します。 多くの開発者やデータベース管理者、ただし、セキュリティ、保守容易性、および updateability 上の理由から、アドホック SQL ステートメントをストアド プロシージャを使用します。 Ardently の柔軟性を高めるため、アドホック SQL ステートメントを好む開発者もいます。 自分の作業では、ストアド プロシージャの優先順位、アドホック SQL ステートメントが、アドホック SQL ステートメントを使用して、前のチュートリアルを簡略化することを選択します。

ときに TableAdapter を定義するか、新しいメソッドを追加、TableAdapter のウィザードでは、簡単に新しいストアド プロシージャを作成したり、アドホック SQL ステートメントを使用すると、既存のストアド プロシージャを使用すると同様です。 このチュートリアルでは TableAdapter s ウィザードでストアド プロシージャの自動生成する方法を説明します。 次のチュートリアルでは、既存または手動で作成されたストアド プロシージャを使用して、TableAdapter メソッドを構成する方法になります。

> [!NOTE]
> Rob Howard のブログ記事を参照してください[しない使用してストアド プロシージャまだ?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/)と[Frans Bouma](https://weblogs.asp.net/fbouma/) s ブログ エントリ[ストアド プロシージャは残念ですが、M Kay でしょうか](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx)長所と短所の活発な議論の。ストアド プロシージャおよびアドホック SQL です。


## <a name="stored-procedure-basics"></a>ストアド プロシージャの基本

関数は、コンス トラクターをすべてのプログラミング言語に共通です。 関数は、関数が呼び出されたときに実行されるステートメントのコレクションです。 関数は、入力パラメーターを受け入れることができ、値を返してもかまいません。 *[ストアド プロシージャ](http://en.wikipedia.org/wiki/Stored_procedure)* はプログラミング言語の関数と多くの類似点を共有するデータベースの構成要素。 ストアド プロシージャは、ストアド プロシージャが呼び出されたときに実行される T-SQL ステートメントのセットで構成されます。 結果を設定するほとんどの場合、多くの入力パラメーターに 0 を受け入れることがあり、出力パラメーター、スカラー値を返すことができます、ストアド プロシージャまたは`SELECT`クエリ。

> [!NOTE]
> 多くの場合にストアド プロシージャをストアド プロシージャまたは Sp と呼びます。


使用してストアド プロシージャが作成された、 [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) T-SQL ステートメント。 たとえば、次の T-SQL スクリプトはという名前のストアド プロシージャを作成します`GetProductsByCategoryID`という名前の単一パラメーターを受け取る`@CategoryID`を返します、 `ProductID`、 `ProductName`、 `UnitPrice`、および`Discontinued`のこれらの列のフィールド、。`Products`に対応するがテーブル`CategoryID`値。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

このストアド プロシージャが作成されたら、呼び出すことができる次の構文を使用します。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.sql)]

> [!NOTE]
> 次のチュートリアルでは、Visual Studio IDE でのストアド プロシージャの作成を見ていきます。 このチュートリアルでは、ただし、ここのストアド プロシージャを自動的に生成する TableAdapter ウィザードを使用します。


単に、データを返すだけでなく、多くの場合、1 つのトランザクションのスコープ内の複数のデータベース コマンドを実行するにストアド プロシージャが使用されます。 という名前のストアド プロシージャ`DeleteCategory`などかかる場合があります、`@CategoryID`パラメーター 2 つの実行と`DELETE`ステートメント: 関連する製品と指定したカテゴリを削除するもう 1 つを削除する最初に、1 つ。 ストアド プロシージャ内で複数のステートメントが*いない*トランザクション内で自動的にラップします。 その他の T-SQL コマンドでは、ストアド プロシージャの複数のコマンドはアトミック操作として扱われます s を確実に発行される必要があります。 後続のチュートリアルでは、トランザクションのスコープ内でストアド プロシージャの s コマンドをラップする方法を見ていきます。

を、アーキテクチャ内でストアド プロシージャを使用する場合、データ アクセス層の s メソッドは、アドホック SQL ステートメントを発行するのではなく、特定のストアド プロシージャを呼び出します。 これは、アプリケーション、アーキテクチャ内で定義することではなく (データベース) で実行される SQL ステートメントの場所を一元化します。 この集中管理はほぼ間違いなく簡単に検索、分析、および、クエリをチューニングし、データベースが使用されている場所と方法についてわかりやすく程度画像を提供します。

ストアド プロシージャの基礎に関する詳細については、このチュートリアルの最後に、関連項目」セクション内のリソースを参照してください。

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>手順 1: 高度なデータ アクセス層のシナリオの Web ページの作成

ストアド プロシージャを使用して DAL を作成する方法について説明を始める前に、これを次のいくつかのチュートリアルに必要な web サイト プロジェクトで ASP.NET ページを作成する最初に少し s を使用できます。 という名前の新しいフォルダーを追加することで開始`AdvancedDAL`します。 次に、次の ASP.NET ページを使用する各ページに関連付けることを確認、そのフォルダーに追加、`Site.master`マスター ページ。

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![高度なデータ アクセス層のシナリオのチュートリアルについては、ASP.NET ページを追加します。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**図 1**: 高度なデータ アクセス層のシナリオのチュートリアルについては、ASP.NET ページを追加


などの他のフォルダーで`Default.aspx`で、`AdvancedDAL`フォルダーは、チュートリアルのセクションで一覧表示します。 いることを思い出してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 そのため、このユーザー コントロールを追加`Default.aspx`をページのデザイン ビューに ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)

**図 2**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズの画像を表示する をクリックします](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png))。


最後に、これらのページを追加するエントリとして、`Web.sitemap`ファイル。 具体的には、データのバッチ処理を使用した作業の後に、次のマークアップを追加`<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.xml)]

更新した後`Web.sitemap`、時間、ブラウザーを使ってチュートリアル web サイトを表示するのにはかかりません。 左側のメニューで、高度な DAL シナリオのチュートリアルの項目できるようになりました。


![サイト マップの DAL の高度なシナリオのチュートリアルのエントリになりました](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)

**図 3**: サイト マップの DAL の高度なシナリオのチュートリアルのエントリになりました


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>手順 2: 新しいを作成する TableAdapter の構成ストアド プロシージャ

Let s は、アドホック SQL ステートメントの代わりにストアド プロシージャを使用してデータ アクセス層の作成を示すためで新しい型指定されたデータセットを作成、`~/App_Code/DAL`という名前のフォルダー`NorthwindWithSprocs.xsd`します。 このプロセスの詳細は、前のチュートリアルでスルーいますがため、すぐにこちらの手順を続行します。 停止するか、またはさらに手順を作成すると、型指定されたデータセットを構成する必要がある場合に戻って、[データ アクセス層を作成する](../introduction/creating-a-data-access-layer-cs.md)チュートリアル。

右クリックし、プロジェクトに新しいデータセットを追加、`DAL`フォルダー、新しい項目の追加 を選択して、図 4 に示すように、データセットのテンプレートを選択します。


[![新しい型指定されたデータセットを NorthwindWithSprocs.xsd という名前のプロジェクトに追加します。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png)

**図 4**: プロジェクトの名前に新しい型指定されたデータセットを追加`NorthwindWithSprocs.xsd`([フルサイズの画像を表示する をクリックします](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png))。


これは新しい型指定されたデータセットを作成、そのデザイナーを開き、新しい TableAdapter を作成および、TableAdapter 構成ウィザードを起動します。 TableAdapter 構成ウィザードの最初の手順では、使用するデータベースを選択するよう求められます。 Northwind データベースへの接続文字列は、ドロップダウン リストに表示する必要があります。 これを選択し、[次へ] をクリックします。

この次の画面から TableAdapter がデータベースにアクセスする方法を選択できます。 前のチュートリアルでは、最初のオプションを使用して SQL ステートメントを選択します。 このチュートリアルでは、2 つ目のオプションを選択、新しいストアド プロシージャを作成および [次へ] をクリックします。


[![新しいストアド プロシージャを作成する TableAdpater を指示します。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png)

**図 5**: 新しいストアド プロシージャを作成する TableAdpater を指示する ([フルサイズの画像を表示する をクリックします](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png))。


したとおり、アドホック SQL ステートメントを使用すると、次の手順では提供する、 `SELECT` TableAdapter のメイン クエリのステートメント。 使用する代わりに、`SELECT`ここに入力したアドホック クエリを直接実行するステートメントでは、TableAdapter の s ウィザードには、これを含むストアド プロシージャが作成されます`SELECT`クエリ。

次を使用して、`SELECT`この TableAdapter のクエリ。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]


[![SELECT クエリを入力します。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png)

**図 6**: 入力、`SELECT`クエリ ([フルサイズの画像を表示する をクリックします](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png))。


> [!NOTE]
> 上記のクエリは、のメインのクエリとは若干異なって、`ProductsTableAdapter`で、`Northwind`型指定されたデータセット。 いることを思い出してください、`ProductsTableAdapter`で、`Northwind`型指定されたデータセットには、各製品のカテゴリと仕入先の会社名とカテゴリの名前を元に戻しますに 2 つの相関サブクエリが含まれています。 近日出版予定の[結合の使用に TableAdapter を更新する](updating-the-tableadapter-to-use-joins-cs.md)この TableAdapter に関連するデータのチュートリアルはこれを追加することを紹介します。


少し高度なオプション ボタンをクリックするには ここからかどうか、ウィザードする必要がありますも insert、update、および delete ステートメントを生成、TableAdapter、オプティミスティック同時実行制御を使用するかどうか、挿入と更新後、データ テーブルを更新するかどうかを指定できます。 生成 Insert、Update および Delete ステートメントのオプションは既定でオンにします。 オンのままにします。 このチュートリアルでは、オプティミスティック同時実行制御オプションを使用してをオフのままにします。

TableAdapter ウィザードによって自動的に作成するストアド プロシージャがある場合、更新、データ テーブル オプションは無視されることが表示されます。 このチェック ボックスがオンかどうか、結果の挿入と更新に関係なくストアド プロシージャは、手順 3. でわかるとおり、単に挿入されたまたは単に更新されるレコードを取得します。


![生成 Insert、Update および Delete のステートメントのオプションをオンにままにします](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png)

**図 7**: 生成 Insert、Update および Delete のステートメントのオプションをオンにままにします


> [!NOTE]
> 追加の条件を追加、オプションを使用してオプティミスティック同時実行制御をオンにした場合、`WHERE`データが他のフィールドに変更があった場合に更新されないようにする句。 参照、[オプティミスティック同時実行を実装する](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md)の詳細については、TableAdapter の組み込みのオプティミスティック同時実行制御機能の使用に関するチュートリアル。


入力した後、`SELECT`クエリを実行し、生成 Insert、Update および Delete のステートメントのオプションをオンになっていることを確認するには、[次へ] をクリックします。 図 8 では、この次の画面では、作成するストアド プロシージャの名前を選択、挿入、更新、およびデータの削除のメッセージが表示されます。 これらのストアド プロシージャ名を変更`Products_Select`、 `Products_Insert`、 `Products_Update`、および`Products_Delete`します。


[![ストアド プロシージャの名前を変更します。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**図 8**: ストアド プロシージャの名前を変更 ([フルサイズの画像を表示する をクリックします](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))。


TableAdapter ウィザードが次の 4 つのストアド プロシージャの作成に使用する T-SQL を表示するには、SQL スクリプトのプレビュー ボタンをクリックします。 SQL スクリプトのプレビュー ダイアログ ボックスからスクリプトをファイルに保存します。 または、クリップボードにコピーできます。


![ストアド プロシージャの生成に使用する SQL スクリプトをプレビューします。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**図 9**: ストアド プロシージャを生成するために使用して SQL スクリプトのプレビュー


ストアド プロシージャの名前を付け、対応するメソッドの名前、tableadapter の横にあるをクリックします。 ときに、アドホック SQL ステートメントを使用すると同じようにすると、既存のデータ テーブルを埋めるか、新しいものを返すメソッドを作成できます。 TableAdapter が挿入、更新、およびレコードの削除の DB ダイレクト パターンを含めるかどうかも指定できます。 すべての 3 つチェック ボックスをオンにしたままにしますが、戻り値に DataTable メソッドの名前を変更`GetProducts`で示した図 10)。


[![メソッドの名前を付けます塗りつぶしと GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)

**図 10**: メソッドの名前を付けます`Fill`と`GetProducts`([フルサイズの画像を表示する をクリックします](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png))。


ウィザードの手順の概要を表示するには、次へ をクリックします。 [完了] ボタンをクリックしてウィザードを完了します。 データセットのデザイナーで、今すぐに含める必要がありますに返されるウィザードの完了後、`ProductsDataTable`します。


[![DataSet の s デザイナーには、新しく追加された ProductsDataTable が表示されます。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)

**図 11**: DataSet の s Designer は、新しく追加されたを示しています`ProductsDataTable`([フルサイズの画像を表示する をクリックします。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png))。


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>手順 3: 新しく作成されたストアド プロシージャのチェック

手順 2 で自動的に使用される、TableAdapter ウィザードには、選択、挿入、更新、およびデータを削除するためのストアド プロシージャが作成されます。 これらのストアド プロシージャは、表示したり、サーバー エクスプ ローラーに移動し、データベースのストアド プロシージャ フォルダーにドリルダウンして、Visual Studio で変更することです。 Northwind データベースには、図 12 に示す 4 つの新しいストアド プロシージャが含まれています: `Products_Delete`、 `Products_Insert`、 `Products_Select`、および`Products_Update`します。


![手順 2 で作成した 4 つのストアド プロシージャはデータベースのストアド プロシージャのフォルダーにあります。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)

**図 12**: 手順 2 で作成した 4 つのストアド プロシージャはデータベースのストアド プロシージャのフォルダーにあります


> [!NOTE]
> サーバー エクスプ ローラーが表示されない場合は、表示 メニューに移動し、サーバー エクスプ ローラーのオプションを選択します。 手順 2 から追加された製品に関連するストアド プロシージャが表示されない場合は、Stored Procedures フォルダーを右クリックしてから、選択を更新します。


ストアド プロシージャの変更を表示またはサーバー エクスプ ローラーでその名前をダブルクリックしますまたは、代わりに、ストアド プロシージャを右クリックしして、開く を選択します。 図 13 では、`Products_Delete`開かれたときに、ストアド プロシージャをします。


[![ストアド プロシージャを開くし、Visual Studio 内からに変更されました](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png)

**図 13**: ストアド プロシージャが開くことができ、変更から内で Visual Studio ([フルサイズの画像を表示する をクリックします](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png))。


両方の内容、`Products_Delete`と`Products_Select`ストアド プロシージャは、とても簡単です。 `Products_Insert`と`Products_Update`ストアド プロシージャ、その一方で、近くの検査、両方の実行を保証する`SELECT`後のステートメント、`INSERT`と`UPDATE`ステートメント。 たとえば、次の SQL は、構成、`Products_Insert`ストアド プロシージャ。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

入力パラメーターとしてストアド プロシージャでは、`Products`列によって返された、 `SELECT` TableAdapter の作成ウィザードおよびこれらの値で指定されたクエリで使用、`INSERT`ステートメント。 次の`INSERT`ステートメント、`SELECT`を返すクエリを使用して、`Products`列の値 (など、 `ProductID`)、新しく追加されたレコードの。 新しく追加された更新ほどバッチ更新パターンを使用すると、自動的に新しいレコードを追加するときに、この更新機能は便利な`ProductRow`インスタンス`ProductID`データベースによって割り当てられた自動インクリメント値を持つプロパティです。

次のコードでは、この機能を説明します。 含まれている、`ProductsTableAdapter`と`ProductsDataTable`用に作成された、`NorthwindWithSprocs`型指定されたデータセット。 新製品は、作成して、データベースに追加されます、`ProductsRow`インスタンス、その値を指定して、tableadapter を呼び出す`Update`に渡して、メソッド、`ProductsDataTable`します。 Tableadapter では内部的には、`Update`メソッドは、列挙、`ProductsRow`渡されたデータ テーブル内のインスタンス (この例では 1 つしかない - 追加したばかりのいずれか) を実行し、適切な挿入、更新、または delete コマンド。 ここで、`Products_Insert`ストアド プロシージャを実行すると、新しいレコードを追加、`Products`テーブルが表示され、新しく追加されたレコードの詳細を返します。 `ProductsRow`インスタンス`ProductID`値が更新されます。 後に、`Update`メソッドが完了したら、新しく追加されたレコード %s にアクセスできます`ProductID`値を通じて、 `ProductsRow` s`ProductID`プロパティ。


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.cs)]

`Products_Update`ストアド プロシージャが同様に含まれています、`SELECT`後のステートメントの`UPDATE`ステートメント。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample7.sql)]

このストアド プロシージャであることの 2 つの入力パラメーターが含まれています`ProductID`:`@Original_ProductID`と`@ProductID`します。 この機能により、シナリオの主キーを変更する可能性があります。 など、従業員データベースに各従業員レコード可能性がありますを使用して、従業員の社会保障番号の主キーとして。 既存の従業員の社会保障番号を変更するには、新しい社会保障番号と元の両方を指定する必要があります。 `Products`ため、テーブル、そのような機能が必要ありません、`ProductID`列は、`IDENTITY`列変更する必要がないとします。 実際には、`UPDATE`内のステートメント、`Products_Update`ストアド プロシージャは t が含まれて、`ProductID`列リスト内の列。 そのため、while`@Original_ProductID`で使用されて、`UPDATE`ステートメント`WHERE`の過剰な句では、`Products`テーブルし、置き換えられる可能性があります、`@ProductID`パラメーター。 ストアド プロシージャの s パラメーターを変更する場合は、そのストアド プロシージャを使用する TableAdapter のメソッドも更新されますが重要です。

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>手順 4: ストアド プロシージャの s パラメーターを変更して、TableAdapter を更新します。

以降、`@Original_ProductID`パラメーターは不要、let s を削除してから、`Products_Update`ストアド プロシージャを完全です。 開く、`Products_Update`ストアド プロシージャを削除、`@Original_ProductID`パラメーター、し、`WHERE`の句、`UPDATE`ステートメントから使用されるパラメーターの名前を変更`@Original_ProductID`に`@ProductID`します。 これらの変更を行った後、次のよう T-SQL ストアド プロシージャ内になります。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample8.sql)]

これらの変更をデータベースに保存、ツールバーの [保存] アイコンをクリックします。 または Ctrl + S をヒットします。 この時点で、`Products_Update`ストアド プロシージャは想定していない、`@Original_ProductID`の入力パラメーターですが、TableAdapter はこのようなパラメーターを渡すように構成します。 TableAdapter に送信パラメーターを確認できます、`Products_Update`ストアド プロシージャをデータセット デザイナーで TableAdapter を選択し、[プロパティ] ウィンドウと内の省略記号をクリックすると、 `UpdateCommand` s`Parameters`コレクション。 図 14 に示すようにパラメーター コレクション エディター ダイアログ ボックスが表示されます。


![パラメーター コレクション エディターのリストに使用するパラメーターが、Products_Update に渡されるストアド プロシージャ](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png)

**図 14**: に渡されるパラメーター コレクション エディターのリストに使用するパラメーター、`Products_Update`ストアド プロシージャ


ここから選択するだけで、このパラメーターを削除することができます、 `@Original_ProductID` [削除] ボタンをクリックしてメンバーの一覧からパラメーター。

または、デザイナーで TableAdapter を右クリックして、構成を選択してすべてのメソッドで使用されるパラメーターを更新することができます。 選択、挿入、更新、使用するストアド プロシージャを一覧表示、TableAdapter 構成ウィザードが表示され、パラメーターと共に、削除するストアド プロシージャが表示されます。 更新プログラムのドロップダウン リストをクリックする場合は、表示、`Products_Update`ストアド プロシージャは、ここで廃止されていますが、入力パラメーターが必要です`@Original_ProductID`(図 15 を参照してください)。 TableAdapter で使用されるパラメーターのコレクションを自動的に更新するには、[完了] をクリックします。


[![TableAdapter の構成ウィザードを使用してそのメソッドのパラメーターのコレクションを更新することもできます。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**図 15**また、tableadapter のメソッドのパラメーター コレクションの更新の構成ウィザードを使用することができます ([フルサイズの画像を表示する をクリックします。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))。


## <a name="step-5-adding-additional-tableadapter-methods"></a>手順 5: 追加の TableAdapter のメソッドを追加します。

手順 2 が示すよう、として新しい TableAdapter を作成するときに自動的に生成された対応するストアド プロシージャを簡単にします。 TableAdapter に追加のメソッドを追加するときにも同様です。 これを示すためには、s を追加できるように、`GetProductByProductID(productID)`メソッドを`ProductsTableAdapter`手順 2. で作成します。 このメソッドは実行の入力として、`ProductID`値し、指定された製品に関する詳細を確認します。

TableAdapter を右クリックし、コンテキスト メニューから追加のクエリを選択して開始します。


![TableAdapter に新しいクエリを追加します。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**図 16**: TableAdapter に新しいクエリを追加


これにより、最初に TableAdapter がデータベースにアクセスする方法を要求すると、TableAdapter クエリ構成ウィザードが開始されます。 作成された新しいストアド プロシージャは、作成ストアド プロシージャの新しいオプションを選択し、[次へ] をクリックします。


[![作成、新しいストアド プロシージャ オプションの選択します。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)

**図 17**: 作成、新しいストアド プロシージャ オプションの選択 ([フルサイズの画像を表示する をクリックします](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png))。


次の画面には、行または単一のスカラー値のセットを返す、またはを実行するかどうかを実行するクエリの種類を識別するよう求められます、 `UPDATE`、 `INSERT`、または`DELETE`ステートメント。 以降、`GetProductByProductID(productID)`メソッドは行を返す、返す行のオプションが選択されており、[次へ] の選択のままにします。


[![選択 オプションの行を返す](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)

**図 18**: 返す行オプションを選択 ([フルサイズの画像を表示する をクリックします](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png))。


[次へ] の画面には、TableAdapter の s メイン クエリには、ストアド プロシージャの名前を一覧表示のみが表示されます (`dbo.Products_Select`)。 次のストアド プロシージャの名前に置き換えます`SELECT`ステートメントで、すべての指定製品の製品のフィールドを返します。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample9.sql)]


[![SELECT クエリでストアド プロシージャ名を置き換えます](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)

**図 19**: ストアド プロシージャの名前を`SELECT`クエリ ([フルサイズの画像を表示する をクリックします](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png))。


後続の画面では、作成されるストアド プロシージャの名前を付けます。 名前を入力します`Products_SelectByProductID`[次へ] をクリックします。


[![新しいストアド プロシージャ Products_SelectByProductID 名](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**図 20**: 新しいストアド プロシージャの名前を付けます`Products_SelectByProductID`([フルサイズの画像を表示する をクリックします](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image46.png))。


ウィザードの最後の手順では、メソッド名が生成だけでなく、塗りつぶしを使用するかどうかを示す、DataTable のパターンを変更するには、DataTable パターンでは、またはその両方を返すことができます。 このメソッドは、両方のオプションがオンのままには、メソッドの名前を変更`FillByProductID`と`GetProductByProductID`します。 実行し、ウィザードを完了するには、[完了] をクリックし、ウィザードの手順の概要を表示するには、[次へ] をクリックします。


[![FillByProductID を GetProductByProductID メソッド、TableAdapter の名前を変更します。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image47.png)

**図 21**: する TableAdapter のメソッドの名前を変更`FillByProductID`と`GetProductByProductID`([フルサイズの画像を表示する をクリックします](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image49.png))。


TableAdapter が使用できる、新しいメソッドをウィザードを完了すると、 `GetProductByProductID(productID)` 、呼び出されるときに実行されます、`Products_SelectByProductID`ストアド プロシージャが作成されたばかりです。 サーバー エクスプ ローラーからこの新しいストアド プロシージャを表示するには、ストアド プロシージャ フォルダーへのドリル ダウンし、開く少し`Products_SelectByProductID`(しない場合、ストアド プロシージャのフォルダーを右クリックしておよび更新 を選択)。

なお、`SelectByProductID`プロシージャが格納されている`@ProductID`入力パラメーターとしてし、実行、`SELECT`ウィザードで入力ステートメント。


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>手順 6: ビジネス ロジック層のクラスを作成します。

チュートリアル シリーズでは、プレゼンテーション層がビジネス ロジック層 (BLL) への呼び出しのすべてに加え、階層型アーキテクチャを維持するために strived いますが。 この設計の決定に従うするにはまず製品データ、プレゼンテーション層からアクセスできる前に、新しい型指定されたデータセットの BLL クラスを作成する必要があります。

という名前の新しいクラス ファイルを作成`ProductsBLLWithSprocs.cs`で、`~/App_Code/BLL`フォルダーし、次のコードを追加します。


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample11.cs)]

このクラスを模倣、`ProductsBLL`セマンティクスを使用して、前のチュートリアルからのクラス、`ProductsTableAdapter`と`ProductsDataTable`オブジェクトから、`NorthwindWithSprocs`データセット。 必要はなくなど、`using NorthwindTableAdapters`としてクラス ファイルの先頭にステートメント`ProductsBLL`、`ProductsBLLWithSprocs`クラスで使用`using NorthwindWithSprocsTableAdapters`。 同様に、`ProductsDataTable`と`ProductsRow`このクラスで使用されるオブジェクトが付いて、`NorthwindWithSprocs`名前空間。 `ProductsBLLWithSprocs`クラスは、2 つのデータ アクセス方法、`GetProducts`と`GetProductByProductID`メソッドを追加するには、更新、および 1 つの製品のインスタンスを削除するとします。

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>手順 7: 操作、`NorthwindWithSprocs`プレゼンテーション層からの DataSet

この時点で、ストアド プロシージャを使用してアクセスして、基になるデータベースのデータを変更する DAL を作成しました。 すべての製品またはメソッドの追加、更新、と共に特定の製品および削除の製品を取得する方法と基本的な BLL を構築しましたがも。 Let s BLL s を使用する ASP.NET ページの作成にこのチュートリアルを丸み`ProductsBLLWithSprocs`表示、更新、およびレコードを削除するためのクラス。

開く、`NewSprocs.aspx`ページで、`AdvancedDAL`フォルダーとその名前を付け、デザイナーには、ツールボックスからドラッグ GridView`Products`します。 GridView から s のスマート タグの選択という名前の新しい ObjectDataSource にバインドする`ProductsDataSource`します。 構成を使用する ObjectDataSource、`ProductsBLLWithSprocs`クラス、図 22 に示すようにします。


[![ProductsBLLWithSprocs クラスを使用する ObjectDataSource を構成します。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image50.png)

**図 22**: 構成に使用する ObjectDataSource、`ProductsBLLWithSprocs`クラス ([フルサイズの画像を表示する をクリックします](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image52.png))。


タブで、ドロップダウン リストが 2 つのオプション、`GetProducts`と`GetProductByProductID`します。 GridView のすべての製品を表示するため、選択、`GetProducts`メソッド。 UPDATE、INSERT、および DELETE の各のタブで、ドロップダウン リストでは、1 つのメソッドがあるだけです。 選択した適切なメソッドは、これらの各ドロップダウン リストにことを確認し、[完了] をクリックします。

ObjectDataSource ウィザードが完了すると、Visual Studio は製品データ フィールドで、GridView に BoundFields と、CheckBoxField を追加します。 編集の有効化と削除を有効にするオプションをスマート タグの存在をチェックして、GridView s の組み込みの編集と削除機能に有効にします。


[![ページには編集および削除のサポートが有効で、GridView が含まれています](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image53.png)

**図 23**: ページに含まれる GridView 編集と削除のサポートを有効に ([フルサイズの画像を表示する をクリックします](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image55.png))。


私たちとして説明した、ObjectDataSource のウィザードの完了時に、前のチュートリアルで Visual Studio の設定、`OldValuesParameterFormatString`プロパティを元に\_{0}します。 既定値に元に戻す必要があります{0}データ変更機能が正しく動作するために、BLL のメソッドで必要となるパラメーターを指定します。 そのため、必ず設定、`OldValuesParameterFormatString`プロパティを{0}または宣言型構文からプロパティを完全に削除します。

編集して、GridView でのサポートを削除し、ObjectDataSource s を返すことの有効化、データ ソースの構成ウィザードの完了後`OldValuesParameterFormatString`プロパティを既定値は、ページ、宣言型マークアップは、次のようになります。


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample12.aspx)]

この時点では、編集、検証をインクルードするインターフェイスをカスタマイズすることで、GridView を整理でしたが、`CategoryID`と`SupplierID`列 Dropdownlist、として表示したりします。 クライアント側の確認 [削除] にも追加できるし、すると、これらの拡張機能を実装するために時間がかかることをお勧めします。 これらのトピックは、前のチュートリアルで説明していますため、ただし、いないにもう一度ここで説明します。

かどうかどうかは、GridView を拡張に関係なく、ブラウザーでページのコア機能をテストします。 図 24 に示すように 1 行当たりの編集と削除機能を提供する GridView では、製品がページに一覧表示します。


[![製品を表示、編集、および GridView から削除できます。](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image56.png)

**図 24**:、編集、および GridView から削除された、製品を表示できます ([フルサイズの画像を表示する をクリックします](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image58.png))。


## <a name="summary"></a>まとめ

型指定されたデータセットに Tableadapter やストアド プロシージャ、アドホック SQL ステートメントを使用して、データベースからデータにアクセスできます。 ストアド プロシージャに基づくのストアド プロシージャの使用、いずれかの既存のストアド プロシージャが使用できますまたは TableAdapter ウィザードは、新しいを作成するように指示することと、`SELECT`クエリ。 このチュートリアルでは、ストアド プロシージャを自動的に作成する方法について説明します。

ストアド プロシージャによりの自動生成された時間を節約することができますが、場合によっては、ウィザードは t によって作成されたストアド プロシージャが、独自の私たちの作成と連携があります。 1 つの例は、`Products_Update`ストアド プロシージャで、両方が必要です`@Original_ProductID`と`@ProductID`入力パラメーターの場合でも、`@Original_ProductID`パラメーターが不要。

多くのシナリオでストアド プロシージャが作成されている、またはストアド プロシージャのコマンドは、制御をより細かくがあるため、手動でそれらをビルドしたい場合があります。 どちらの場合、TableAdapter のメソッドに既存のストアド プロシージャを使用するよう指示することがあります。 次のチュートリアルでこれを実現する方法表示されます。

満足のプログラミングです。

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ストアド プロシージャの作成と保守](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [ストアド プロシージャからのスカラー データの取得](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [SQL Server ストアド プロシージャの基本](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [ストアド プロシージャ: 概要](http://www.sqlteam.com/item.asp?ItemID=563)
- [ストアド プロシージャの作成](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Hilton Geisenow でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [次へ](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
