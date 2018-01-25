---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-vb
title: "データ アクセス層 (VB) の作成 |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルでは、最初から開始をデータ アクセス レイヤー (DAL)、型指定されたデータセットを使用して、データベース内の情報にアクセスを作成します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/05/2010
ms.topic: article
ms.assetid: 6227233a-6254-4b6b-9a89-947efef22330
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: ad578d5d5fb1ef0ac63d3cbde3f307535ea3d98c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-data-access-layer-vb"></a>データ アクセス層 (VB) の作成
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_1_VB.exe)または[PDF のダウンロード](creating-a-data-access-layer-vb/_static/datatutorial01vb1.pdf)

> このチュートリアルでは、最初から開始をデータ アクセス レイヤー (DAL)、型指定されたデータセットを使用して、データベース内の情報にアクセスを作成します。


## <a name="introduction"></a>はじめに

Web 開発者は、生活中心となるデータを操作します。 データの取得し、し、web ページを収集して集計を変更するためのコードを格納するデータベースを作成します。 これは、時間のかかるを ASP.NET 2.0 ではこれらの一般的なパターンを実装するテクニックを紹介する一連の最初のチュートリアルです。 まず始めに作成する、[ソフトウェア アーキテクチャ](http://en.wikipedia.org/wiki/Software_architecture)のデータ アクセス層 (DAL) に型指定されたデータセット、ビジネス ロジック層 (BLL) を使用してを構成するカスタム ビジネス ルール実行し、プレゼンテーション層が ASP.NET の構成ページ共通のページ レイアウトを共有します。 一度このバックエンド築く構築、レポートに移動しますを表示する方法を示す集計、収集、および web アプリケーションからデータを検証します。 これらのチュートリアルは、簡潔なは、スクリーン ショットが多いプロセスを視覚的に順番の詳細な手順についての提供に特化しています。 各チュートリアルは、c# および Visual Basic バージョンで利用可能な使用される完全なコードのダウンロードが含まれています。 (最初のチュートリアルは非常に長くなるですが、残りの部分がはるかにという事実チャンク単位で表示されます)。

Microsoft SQL Server 2005 Express Edition バージョンに Northwind データベースのこれらのチュートリアルを使用する、`App_Data`ディレクトリ。 データベース ファイルに加え、`App_Data`別のデータベース バージョンを使用する場合にフォルダーが、データベースを作成するための SQL スクリプトにも含まれます。 これらのスクリプトですることもできます[マイクロソフトから直接ダウンロード](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en)、希望する場合。 Northwind データベースの別の SQL Server バージョンを使用する場合は、更新する必要があります、`NORTHWNDConnectionString`アプリケーションの設定`Web.config`ファイル。 Web アプリケーションが Visual Studio 2005 Professional Edition を使用して、ファイル システムに基づいた Web サイト プロジェクトとしてビルドします。 ただし、チュートリアルのすべての作業は、Visual Studio 2005 の無料版と同様[Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/)です。

このチュートリアルではおを最初から開始し、データ アクセス レイヤー (DAL)、作成後に作成、[ビジネス ロジック層 (BLL)](creating-a-business-logic-layer-vb.md)は 2 番目のチュートリアルでは、取り組んで[ページ レイアウトとナビゲーション](master-pages-and-site-navigation-vb.md) 、3 番目にします。 チュートリアルでは、3 番目の 1 つが基盤に構築した後は、最初の 3 つに配置されます。 この最初のチュートリアルで説明、そのため、Visual Studio を起動および始めましょう多くしました!

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>手順 1: Web プロジェクトを作成して、データベースに接続します。

データ アクセス層 (DAL) を作成するには、web サイトを作成し、データベースのセットアップには必要があります。 まず、新しいファイル システムに基づいた ASP.NET web サイトを作成します。 これを実現するには、ファイル メニューに移動し、新しい Web サイトを新しい Web サイト ダイアログ ボックスの表示を選択します。 ASP.NET Web サイト テンプレートを選択して、場所ドロップダウン リストをファイル システムに設定、web サイトを配置するフォルダーを選択および Visual Basic 言語に設定します。


[![新しいファイル システムに基づいた Web サイトを作成します。](creating-a-data-access-layer-vb/_static/image2.png)](creating-a-data-access-layer-vb/_static/image1.png)

**図 1**: New File System-Based Web サイトの作成 ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image3.png))


これで新しい web サイトが作成されます、 `Default.aspx` ASP.NET ページ、`App_Data`フォルダー、および`Web.config`ファイル。

Web サイトを作成では、次の手順は、Visual Studio のサーバー エクスプ ローラーで、データベースへの参照を追加するは。 サーバー エクスプ ローラーにデータベースを追加するには、テーブル、ストアド プロシージャ、ビュー、および Visual Studio 内からのすべてを追加できます。 テーブル データを表示したり、クエリ ビルダーを使用して手動でまたは視覚的に独自のクエリを作成できます。 さらに、DAL の型指定されたデータセットを構築するときに型指定されたデータセットを構築するためのデータベースへのポイント Visual Studio には必要があります。 その時点では、この接続情報を提供することができます、Visual Studio でサーバー エクスプ ローラーで既に登録されているデータベースのボックスの一覧が自動的に設定されます。

サーバー エクスプ ローラーに、Northwind データベースを追加するための手順で SQL Server 2005 Express Edition のデータベースを使用するかどうかに依存、`App_Data`フォルダーまたは Microsoft SQL Server 2000 または 2005年データベース サーバーのセットアップを使用することがあるかどうかその代わりに。

## <a name="using-a-database-in-theappdatafolder"></a>データベースを使用して、`App_Data`フォルダー

ダウンロードした websit に配置されている Northwind データベースの SQL Server 2005 Express Edition のバージョンを使用することができる場合に接続するには、SQL Server 2000 または 2005年データベース サーバーがないか、データベース サーバーにデータベースを追加する必要があるようにするだけで、杯`App_Data`フォルダー (`NORTHWND.MDF`)。

データベースの配置で、`App_Data`フォルダーは、サーバー エクスプ ローラーに自動的に追加します。 SQL Server 2005 Express Edition がコンピューターにインストールがあると仮定して NORTHWND をという名前のノードが表示されます。サーバー エクスプ ローラーで MDF これを拡張し、そのテーブル、ビュー、ストアド プロシージャ、およびよびな (図 2 を参照してください) を調査することができます。

`App_Data`フォルダーは、Microsoft Access にも格納できる`.mdb`SQL Server の対応するように自動的に追加サーバー エクスプ ローラーになるファイル。 常にすることが SQL Server のオプションのいずれかを使用しない場合は、 [Microsoft Access バージョンの Northwind データベース ファイルをダウンロード](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN)にドロップし、`App_Data`ディレクトリ。 注意して、ただし、Access データベースがないと機能が豊富で SQL Server と web サイトのシナリオで処理するために設計されていないとします。 さらに、いくつかの 35 + チュートリアルでは、Access によってサポートされていない特定のデータベース レベルの機能が使用されます。

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Microsoft SQL Server 2000 または 2005 のデータベース サーバーでデータベースに接続します。

また、データベース サーバーにインストールされている Northwind データベースに接続することがあります。 データベース サーバーにまだインストールされている Northwind データベースがない場合最初にする必要がありますに追加するデータベース サーバーに含まれているこのチュートリアルのダウンロード、インストール スクリプトを実行して[Northwind の SQL Server 2000 バージョンのダウンロードインストール スクリプト](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en)Microsoft の web サイトから直接です。

インストールされているデータベースを作成したら、Visual Studio の [サーバー エクスプ ローラーには、データ接続] ノードを右クリックし、接続の追加を移動します。 表示されないかどうかは、ビューに移動して、サーバー エクスプ ローラー/サーバー エクスプ ローラー、またはヒット Ctrl + Alt + S。 これには、認証情報とデータベース名への接続にサーバーを指定できる接続の追加 ダイアログ ボックスが表示されます。 データベースの接続情報を構成し、ok ボタンをクリックしてしました、一度データベースが、データ接続 ノードの下にノードとして追加されます。 そのテーブル、ビュー、ストアド プロシージャ、およびなどを調べるデータベース ノードを展開することができます。


![データベース サーバーの Northwind データベースへの接続を追加します。](creating-a-data-access-layer-vb/_static/image4.png)

**図 2**: に、データベース サーバーの Northwind データベースへの接続の追加


## <a name="step-2-creating-the-data-access-layer"></a>手順 2: データ アクセス層の作成

使用するときにデータの 1 つのオプション データに固有のロジックに直接埋め込むプレゼンテーション層で web アプリケーションでは、プレゼンテーション層を ASP.NET ページの構成) を開始します。 ASP.NET ページのコード部分で ADO.NET コードを記述またはマークアップの部分から SqlDataSource コントロールを使用する形式の時間がかかります。 どちらの場合、このアプローチと密データ アクセス ロジック プレゼンテーション層です。 プレゼンテーション層から、データ アクセス ロジックを分離するただしは、方法をお勧めします。 この別のレイヤーし、呼ばれ、データ アクセス層、略して、DAL は通常、別のクラス ライブラリ プロジェクトとして実装します。 このレイヤーのアーキテクチャの利点はも記載されています (これらの利点については、このチュートリアルの最後に、「これ以上の測定値」セクションを参照してください) がこのシリーズの取得方法とします。

データベースへの接続の作成など、基になるデータ ソースに固有のすべてのコードを発行する`SELECT`、 `INSERT`、 `UPDATE`、および`DELETE`コマンド、およびなどを DAL に配置する必要があります。 プレゼンテーション層は、このようなデータ アクセス コードへの参照を含めることはできませんが、すべてのデータ要求、DAL に呼び出しを行う代わりにする必要があります。 通常、データ アクセス レイヤーには、基になるデータベースのデータにアクセスするためのメソッドが含まれます。 たとえば、Northwind データベースには`Products`と`Categories`販売およびが所属するカテゴリの製品を記録するテーブル。 DAL などのメソッドをが。

- `GetCategories(),`これには、すべてのカテゴリに関する情報を返します
- `GetProducts()`、これには、すべての製品に関する情報を返します
- `GetProductsByCategoryID(categoryID)`、するには、指定したカテゴリに属しているすべての製品を返します
- `GetProductByProductID(productID)`、これは、特定の製品に関する情報を返します

呼び出されると、これらのメソッドはデータベースへの接続、適切なクエリを発行し、結果を返します。 これらの結果を返すどのようにすることが重要です。 データセットまたは DataReader データベース クエリによって設定されますが、これらのメソッドに返されるだけでしたが、理想的にはこれらの結果を返すを使用して*オブジェクトの厳密に型指定された*です。 厳密に型指定されたオブジェクトは、コンパイル時にスキーマを持つが定義されている厳密反対に、厳密に型のオブジェクトは 1 つのスキーマが実行時まで不明です。

たとえば、DataReader し (既定) のデータセットは弱い型指定オブジェクトによってそれらの設定に使用されるデータベース クエリによって返される列のスキーマが定義されているためです。 ような構文を使用する必要があります、弱い型指定の DataTable から特定の列にアクセスする:`DataTable.Rows(index)("columnName")`です。 DataTable のこの例で柔軟な型指定は文字列または序数インデックスを使用して、列名にアクセスする必要がありますという事実によって行われます。 厳密に型指定された DataTable では、その一方が、プロパティとして実装されたその列のそれぞれ次のようなコードで結果として得られる:`DataTable.Rows(index).columnName`です。

厳密に型指定されたオブジェクトを返すには、開発者は、独自のカスタム ビジネス オブジェクトを作成するか、型指定されたデータセットを使用することができます。 ビジネス オブジェクトは、プロパティを持つは通常、基になるデータベース テーブル、ビジネス オブジェクトの列を反映してクラスを表します。 開発者によって実装されます。 型指定されたデータセットは、データベース スキーマとそのメンバーが厳密に型指定されたこのスキーマによるに基づいて Visual Studio によって生成したクラスです。 型指定されたデータセット自体は、ADO.NET DataSet、DataTable、および DataRow クラスを拡張するクラスで構成されます。 データ テーブルの厳密に型指定されただけでなく型指定されたデータセット今すぐ含めることも、Tableadapter はデータセットのデータ テーブルを設定し、データベースにデータ テーブル内の変更を反映するためのメソッドを持つクラス。

> [!NOTE]
> 長所と短所を型指定されたデータセットを使用するカスタム ビジネス オブジェクトとの比較の詳細についてを参照してください[データ層のコンポーネントを設計し、層間のデータを渡す](https://msdn.microsoft.com/library/ms978496.aspx)です。


これらのチュートリアルのアーキテクチャ用に厳密に型指定されたデータセットを使用します。 図 3 は、型指定されたデータセットを使用するアプリケーションのさまざまな層間のワークフローを示しています。


[![すべてのデータ アクセス コードは、DAL に移行します。](creating-a-data-access-layer-vb/_static/image6.png)](creating-a-data-access-layer-vb/_static/image5.png)

**図 3**: すべてのデータ アクセス コードは、DAL に移行 ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>型指定されたデータセットとテーブル アダプターを作成します。

DAL を作成するには、型指定されたデータセットをプロジェクトに追加することで開始します。 これを実現するには、ソリューション エクスプ ローラーでプロジェクト ノードを右クリックし、新しい項目の追加を選択します。 テンプレートの一覧からデータセット オプションを選択し、名前を付けます`Northwind.xsd`です。


[![プロジェクトに新しいデータセットを追加します。](creating-a-data-access-layer-vb/_static/image9.png)](creating-a-data-access-layer-vb/_static/image8.png)

**図 4**: プロジェクトに新しいデータセットを追加する ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image10.png))


データセットを追加するように求められたら、追加のクリックして後、`App_Code`フォルダーで、はい を選択します。 型指定されたデータセットをデザイナーが表示されます、および、TableAdapter 構成ウィザードが開始、型指定されたデータセットに、最初の TableAdapter を追加することができます。

データの厳密に型指定されたコレクションとして型指定されたデータセットの機能します。厳密に型指定された DataTable インスタンス、それぞれがさらにから成る厳密に型指定された DataRow インスタンスで構成されます。 このチュートリアルの一連の操作に必要な基になるデータベース テーブルの列ごと厳密に型指定された DataTable を作成します。 データ テーブルの作成を始めましょう、`Products`テーブル。

厳密に型指定された Datatable には、その基になるデータベース テーブルからデータにアクセスする方法の情報が含まれていないことに注意してください。 データ ソースの列にデータを取得するためには、データ アクセス層として機能する TableAdapter クラスを使用します。 `Products` DataTable、TableAdapter にはメソッドが含まれて、 `GetProducts()`、`GetProductByCategoryID(categoryID)`プレゼンテーション層からおを起動します。 DataTable の役割では、データ、レイヤー間で受け渡すに使用される厳密に型指定されたオブジェクトとして機能です。

TableAdapter 構成ウィザードを使用するデータベースを選択するように求められますが開始されます。 ドロップダウン リストでは、サーバー エクスプ ローラーでそれらのデータベースを示します。 サーバー エクスプ ローラーに、Northwind データベースを追加しなかった場合は、これを行うには、この時点で新しい接続ボタンをクリックすることができます。


[![ドロップダウン リストから、Northwind データベースを選択します。](creating-a-data-access-layer-vb/_static/image12.png)](creating-a-data-access-layer-vb/_static/image11.png)

**図 5**: ドロップダウン リストから、Northwind データベースを選択 ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image13.png))


データベースを選択し、[次へ] をクリックして、後にするよう求められます内の接続文字列を保存するかどうか、`Web.config`ファイル。 接続文字列を保存することによってされます必要があるがハード コード化された TableAdapter のクラスが接続文字列が変更された場合、将来の処理を簡素化されます。 配置されている構成ファイルに接続文字列を保存することを選択する場合、`<connectionStrings>`可能性があるセクション[オプションで暗号化された](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)セキュリティの向上や、新しい ASP.NET 2.0 プロパティ ページ内で後で変更IIS GUI 管理ツール、管理者にとってより適切なであります。


[![Web.config への接続文字列を保存します。](creating-a-data-access-layer-vb/_static/image15.png)](creating-a-data-access-layer-vb/_static/image14.png)

**図 6**: への接続文字列を保存`Web.config`([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image16.png))


次に、最初の厳密に型指定された DataTable のスキーマを定義し、厳密に型指定されたデータセットのデータ設定時に使用する、TableAdapter の最初のメソッドを提供する必要があります。 これら 2 つの手順は、データ テーブルに反映すること、テーブルから列を返すクエリを作成することで同時に実行されます。 ウィザードの最後にこのクエリに対するメソッドの名前を付けます。 化は、後であれば、プレゼンテーション層からこのメソッドを呼び出すことができます。 メソッドは、定義済みのクエリを実行し、厳密に型指定された DataTable を設定します。

作業を開始する、TableAdapter にクエリを発行する方法を示す必要があります最初、SQL クエリを定義します。 おアドホック SQL ステートメントを使用して、ストアド プロシージャの新規作成したり既存のストアド プロシージャを使用できます。 これらのチュートリアルは、アドホック SQL ステートメントを使用します。 参照してください[Brian 認識](http://briannoyes.net/)記事 's [Visual Studio 2005 のデータセット デザイナーでのデータ アクセス層のビルド](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)ストアド プロシージャを使用する例についてはします。


[![アドホック SQL ステートメントを使用してデータをクエリします。](creating-a-data-access-layer-vb/_static/image18.png)](creating-a-data-access-layer-vb/_static/image17.png)

**図 7**: アドホック SQL ステートメントを使用してデータのクエリ ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image19.png))


この時点でを手動で SQL クエリで入力できます。 TableAdapter の最初のメソッドを作成するときに通常する対応する DataTable で表現する必要があるこれらの列を返すクエリです。 これを行うすべての列とのすべての行を返すクエリを作成して、`Products`テーブル。


[![テキスト ボックスに、SQL クエリを入力してください。](creating-a-data-access-layer-vb/_static/image21.png)](creating-a-data-access-layer-vb/_static/image20.png)

**図 8**: SQL クエリに、テキスト ボックスの入力 ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image22.png))


または、クエリ ビルダーを使用し、図 9 に示すように、クエリを視覚的に構築します。


[![クエリをグラフィカルに作成、クエリ エディターを使用](creating-a-data-access-layer-vb/_static/image24.png)](creating-a-data-access-layer-vb/_static/image23.png)

**図 9**: クエリをグラフィカルに作成、クエリ エディターを使用 ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image25.png))


クエリを作成した後は、次の画面上に移動する前に、詳細オプション ボタンをクリックします。 Web サイト プロジェクトに「を生成する挿入、更新、および Delete ステートメント」はのみ、既定で選択したオプションの詳細クラス ライブラリまたは Windows プロジェクトからこのウィザードを実行する場合は、"オプティミスティック同時実行制御を使う オプションも選択されます。 ここでは"オプティミスティック同時実行制御を使う オプションをオンにせずには。 今後のチュートリアルでは、オプティミスティック同時実行制御を説明します。


[![生成する Insert、Update、および Delete ステートメントのみオプションを選択します。](creating-a-data-access-layer-vb/_static/image27.png)](creating-a-data-access-layer-vb/_static/image26.png)

**図 10**: のみ生成する Insert、Update、および Delete ステートメント オプションを選択 ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image28.png))


高度なオプションを確認した後は、最終画面に進むには [次へ] をクリックします。 ここで、TableAdapter に追加するメソッドを選択するように求められます。 データを設定するための 2 つのパターンがあります。

- **Datatable**クエリの結果に基づいて、メソッドが作成を受け取り、パラメーターとして DataTable を設定するこの方法でします。 ADO.NET データ アダプター クラスは、たとえば、このパターンを実装、`Fill()`メソッドです。
- **DataTable を返す**この方法でメソッドを作成する DataTable を入力およびメソッドの戻り値として返します。

これらのパターンの一方または両方を実装して、TableAdapter を持つことができます。 ここで提供されるメソッドの名前を変更することもできます。 みましょうのままにして両方のチェック ボックスがオンになっている場合でも、後者のパターンをこれらのチュートリアル全体で使用するだけです。 また、名前ではなくジェネリック`GetData`メソッドを`GetProducts`です。

オンになっている場合、最終的なチェック ボックスを"GenerateDBDirectMethods、"が作成されます`Insert()`、 `Update()`、および`Delete()`TableAdapter のメソッドです。 このオプションをオフのままにすると、すべての更新する必要があります操作には、TableAdapter の唯一`Update()`メソッド、型指定された DataSet、DataTable、単一の DataRow または Datarow の配列を取得します。 (した場合、このチェック ボックスのオプション図 9 で高度なプロパティをオンになっていない「を生成する挿入、更新、および Delete ステートメント」設定には影響はありません)。このチェック ボックスをオンのままにしてみましょう。


[![GetData から GetProducts にメソッド名を変更します。](creating-a-data-access-layer-vb/_static/image30.png)](creating-a-data-access-layer-vb/_static/image29.png)

**図 11**: からメソッド名を変更`GetData`に`GetProducts`([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image31.png))


[完了] をクリックして、ウィザードを完了します。 ウィザードを閉じた後、データ テーブルを示し、先ほど作成したデータセット デザイナーに戻ります。 内の列の一覧を表示することができます、 `Products` DataTable (`ProductID`、`ProductName`など) のメソッドだけでなく、 `ProductsTableAdapter` (`Fill()`と`GetProducts()`)。


[![製品の DataTable と ProductsTableAdapter が型指定されたデータセットに追加されました](creating-a-data-access-layer-vb/_static/image33.png)](creating-a-data-access-layer-vb/_static/image32.png)

**図 12**: `Products` DataTable と`ProductsTableAdapter`型指定されたデータセットに追加されている ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image34.png))


この時点で 1 つの DataTable に型指定されたデータセットがある (`Northwind.Products`) と厳密に型指定されたデータ アダプター クラス (`NorthwindTableAdapters.ProductsTableAdapter`) で、`GetProducts()`メソッドです。 これらのオブジェクトは、リストにアクセスする、すべての製品のようなコードから使用できます。

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample1.vb)]

このコードでは、データ アクセスに固有のコードの 1 つのビットを記述する必要ありませんでした。 すべての ADO.NET クラスのインスタンスを作成する必要がない、任意の SQL クエリでは、接続文字列を参照する必要がありませんでしたまたはストアド プロシージャおです。 代わりに、TableAdapter は、ご利用の米国の下位レベルのデータ アクセス コードを提供します。

この例で使用される各オブジェクトは、厳密に型指定された、Visual Studio IntelliSense およびコンパイル時の型チェックを提供できるようにもします。 TableAdapter によって返されるすべてのデータ テーブルの最適な ASP.NET データなど、GridView、DetailsView、DropDownList、CheckBoxList、およびその他のいくつかの Web コントロールにバインドできます。 によって返される DataTable をバインドする例を紹介します`GetProducts()`だけの十分な 3 行のコード内での GridView にメソッド、`Page_Load`イベント ハンドラー。

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample2.aspx)]

AllProducts.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample3.vb)]


[![GridView には、製品の一覧を表示してください。](creating-a-data-access-layer-vb/_static/image36.png)](creating-a-data-access-layer-vb/_static/image35.png)

**図 13**: GridView に、製品の一覧が表示されます ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image37.png))


この例は、ASP.NET ページの次の 3 つの行のコードに記述したことを必須`Page_Load`イベント ハンドラー、将来のチュートリアル、ObjectDataSource を使用して宣言によって、DAL からデータを取得する方法について確認します。 ObjectDataSource おコードを記述する必要がありますいないとページングや並べ替えのサポートもが表示されます。

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>手順 3:、メソッド、データ アクセス レイヤーを追加するパラメーター化されました。

この時点で、`ProductsTableAdapter`クラスには 1 つのメソッドが`GetProducts()`データベース内のすべての製品が返されます。 すべての製品を使用することは明らかに便利ですが、特定の製品、または特定のカテゴリに属しているすべての製品に関する情報を取得する場合はありますがあります。 データ アクセス層にこのような機能を追加するには、メソッドのパラメーター化された TableAdapter に追加できます。

追加してみましょう。、`GetProductsByCategoryID(categoryID)`メソッドです。 データセット デザイナーに戻り、DAL に新しいメソッドを追加するのには右クリック、`ProductsTableAdapter`セクション、およびクエリの追加を選択します。


![TableAdapter を右クリックして、クエリを追加](creating-a-data-access-layer-vb/_static/image38.png)

**図 14**: TableAdapter を右クリックして、クエリを追加


最初に、アドホック SQL ステートメントまたは新規または既存のストアド プロシージャを使用してデータベースにアクセスするかどうかについてお求められます。 もう一度、アドホック SQL ステートメントを使用するを選択します。 次に、SQL クエリの種類ようを使用することが求められます。 記述する、指定したカテゴリに属しているすべての製品を取得するため、`SELECT`ステートメントの行が返されます。


[![行を返す SELECT ステートメントを作成します。](creating-a-data-access-layer-vb/_static/image40.png)](creating-a-data-access-layer-vb/_static/image39.png)

**図 15**: 作成 を選択して、`SELECT`ステートメントが行を返します ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image41.png))


次の手順では、データにアクセスするために使用する SQL クエリを定義します。 同じ使用を特定のカテゴリに属する製品だけを返すので、`SELECT`ステートメントから`GetProducts()`、次の追加が`WHERE`句:`WHERE CategoryID = @CategoryID`です。 `@CategoryID`メソッドを作成していますが、対応する型 (つまり、null 許容の整数) の入力パラメーターを必要とする、TableAdapter ウィザードにパラメーターを示します。


[![指定されたカテゴリの製品を返すだけにクエリを入力します](creating-a-data-access-layer-vb/_static/image43.png)](creating-a-data-access-layer-vb/_static/image42.png)

**図 16**: に指定されたカテゴリの製品をのみを返すクエリを入力します ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image44.png))


最後の手順で選択できますデータ アクセス パターンを使用して、できるだけでなく、生成されたメソッドの名前をカスタマイズします。 塗りつぶしのパターンの名前を変更してみましょう`FillByCategoryID`および戻り値の DataTable 戻りパターン (、`GetX`メソッド) を使ってみましょう`GetProductsByCategoryID`です。


[![TableAdapter のメソッドの名前を選択します。](creating-a-data-access-layer-vb/_static/image46.png)](creating-a-data-access-layer-vb/_static/image45.png)

**図 17**: TableAdapter のメソッドの名前を選択 ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image47.png))


ウィザードを完了すると、データセット デザイナーには、新しい TableAdapter のメソッドが含まれています。


![カテゴリは、照会する、製品できるようになりました](creating-a-data-access-layer-vb/_static/image48.png)

**図 18**:、製品ことによってクエリ カテゴリ


追加する、`GetProductByProductID(productID)`メソッドと同じ手法を使用します。

データセット デザイナーから直接、これらのパラメーター化クエリをテストできます。 TableAdapter のメソッドを右クリックし、データのプレビューを選択します。 次に、パラメーターを使用し、[プレビュー] をクリックする値を入力します。


[![これらの製品属する飲み物のカテゴリが表示されます。](creating-a-data-access-layer-vb/_static/image50.png)](creating-a-data-access-layer-vb/_static/image49.png)

**図 19**: これらの製品に属する飲み物のカテゴリが表示されます ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image51.png))


`GetProductsByCategoryID(categoryID)` DAL のメソッドは、指定されたカテゴリの製品のみを表示するための ASP.NET ページ今すぐ作成できます。 次の例では、すべての製品、飲料カテゴリ内にあるである必要が、`CategoryID`は 1 です。

Beverages.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample4.aspx)]

Beverages.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample5.vb)]


[![飲料カテゴリの製品が表示されます。](creating-a-data-access-layer-vb/_static/image53.png)](creating-a-data-access-layer-vb/_static/image52.png)

**図 20**: もの、飲料カテゴリの製品が表示されます ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>手順 4: 挿入、更新、およびデータを削除します。

一般的な用途の挿入、更新、およびデータを削除する 2 つのパターンがあります。 最初のパターンは、データベースの直接パターンと呼んでするには、メソッドを作成する、呼び出されたときに、問題、 `INSERT`、 `UPDATE`、または`DELETE`コマンドを 1 つのデータベース レコードの適用対象となるデータベースにします。 このようなメソッドが通常に渡されます、一連のスカラー値 (整数、文字列、ブール値、DateTimes、およびなど) 対応する値を挿入、更新、または削除します。 たとえばのこのパターンで、`Products`整数パラメーターでは delete メソッドを要しますテーブルを示す、`ProductID`の文字列に挿入メソッドがかかるときは、削除するレコードの`ProductName`、の10進数`UnitPrice`、整数、`UnitsOnStock`のようにします。


[![各挿入、更新、および削除要求がすぐにデータベースに送信されます。](creating-a-data-access-layer-vb/_static/image56.png)](creating-a-data-access-layer-vb/_static/image55.png)

**図 21**: 各挿入、更新、および削除要求がすぐにデータベースに送信される ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image57.png))


バッチとしてパターンを更新する呼ぶことに、その他のパターンでは、全体の DataSet、DataTable、または 1 つのメソッドの呼び出しで Datarow のコレクションを更新します。 このパターンを持つ開発者削除挿入、および DataTable に Datarow を変更し、update メソッドにそれらの Datarow または DataTable を渡します。 このメソッドに渡された Datarow の列挙、決定かどうかがいる変更、追加、または削除されました (DataRow のを介して[RowState プロパティ](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx)値)、し、各レコードの適切なデータベースの要求を発行します。


[![更新メソッドが呼び出されたときに、すべての変更がデータベースと同期されます。](creating-a-data-access-layer-vb/_static/image59.png)](creating-a-data-access-layer-vb/_static/image58.png)

**図 22**: 更新メソッドが呼び出されたときに、すべての変更がデータベースと同期されます ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image60.png))


TableAdapter では、既定では、バッチ更新パターンを使用しますも DB 直接パターンをサポートします。 TableAdapter を作成するときに、詳細プロパティから「を生成する挿入、更新、および Delete ステートメント」オプションを選択おため、`ProductsTableAdapter`が含まれています、`Update()`メソッドで、バッチ更新パターンを実装します。 具体的には、TableAdapter が含まれています、`Update()`型指定されたデータセットを厳密に型指定された DataTable、または 1 つまたは複数の Datarow に渡すことができるメソッドです。 中断した場合、"GenerateDBDirectMethods"チェック ボックスと DB 直接パターン、TableAdapter を作成してもによって実装されて`Insert()`、 `Update()`、および`Delete()`メソッドです。

両方のデータ変更のパターンを使用、TableAdapter の`InsertCommand`、 `UpdateCommand`、および`DeleteCommand`を発行するプロパティ、 `INSERT`、 `UPDATE`、および`DELETE`コマンドをデータベースにします。 検査および変更できる、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`TableAdapter はデータセット デザイナーでをクリックし、[プロパティ] ウィンドウでプロパティをします。 (、TableAdapter とを選択するかどうかを確認、`ProductsTableAdapter`オブジェクトが 1 つのプロパティ ウィンドウで、ドロップダウン リストで選択します)。


[![TableAdapter が InsertCommand、UpdateCommand、および DeleteCommand プロパティ](creating-a-data-access-layer-vb/_static/image62.png)](creating-a-data-access-layer-vb/_static/image61.png)

**図 23**:、TableAdapter が`InsertCommand`、 `UpdateCommand`、および`DeleteCommand`プロパティ ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image63.png))


これらのデータベース コマンド プロパティを変更または確認して、をクリックして、`CommandText`サブプロパティで、クエリ ビルダーが表示されます。


[![クエリ ビルダーでの挿入、更新、および DELETE ステートメントを構成します。](creating-a-data-access-layer-vb/_static/image65.png)](creating-a-data-access-layer-vb/_static/image64.png)

**図 24**: 構成、 `INSERT`、 `UPDATE`、および`DELETE`クエリ ビルダー内のステートメント ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image66.png))


次のコード例では、パターンを使用して、バッチ更新は廃止されましたいないおよび在庫以下 25 ユニットがあるすべての製品の価格を倍にする方法を示します。

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample6.vb)]

次のコードでは、プログラムによって特定の製品を削除してから、1 つを更新する DB 直接パターンを使用し、新しいを追加する方法を示します。

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample7.vb)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>作成するカスタム Insert、Update、および Delete メソッド

`Insert()`、 `Update()`、および`Delete()`DB 直接メソッドによって作成されたメソッドを扱い、特に多くの列を含むテーブルのことができます。 IntelliSense のヘルプが明確ではない特に何せず、上記のコード例を見て`Products`テーブル列に各入力パラメーターに対応する、`Update()`と`Insert()`メソッドです。 ときにのみが必要する 1 つの列または 2、更新、カスタマイズされた時間がある可能性があります`Insert()`は、おそらく、メソッドは、新しく挿入されるレコードの値を返す`IDENTITY`(自動インクリメント) フィールドです。

このようなカスタム メソッドを作成するには、データセット デザイナーに戻ります。 TableAdapter を右クリックし、TableAdapter ウィザードに戻って、追加のクエリを選択します。 2 番目の画面には、作成するクエリの種類おを示します。 新しい製品を追加し、後の新しく追加されたレコードの値を返すメソッドを作成しましょう`ProductID`です。 そのため、作成すること、`INSERT`クエリ。


[![Products テーブルに新しい行を追加するメソッドを作成します。](creating-a-data-access-layer-vb/_static/image68.png)](creating-a-data-access-layer-vb/_static/image67.png)

**図 25**: 新規行を追加するメソッドを作成、`Products`テーブル ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image69.png))


次の画面で、`InsertCommand`の`CommandText`が表示されます。 このクエリを追加することによって拡張`SELECT SCOPE_IDENTITY()`に挿入された最後の id 値を返しますクエリの最後に、`IDENTITY`同じスコープ内の列です。 (を参照してください、[のテクニカル ドキュメント](https://msdn.microsoft.com/library/ms190315.aspx)の詳細については`SCOPE_IDENTITY()`と多くの場合にその理由[スコープを使用して\_の代わりに @ IDENTITY()@IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx))。終了するかどうかを確認、`INSERT`ステートメントをセミコロンを追加する前に、`SELECT`ステートメントです。


[![SCOPE_IDENTITY() 値を返すクエリを拡張します。](creating-a-data-access-layer-vb/_static/image71.png)](creating-a-data-access-layer-vb/_static/image70.png)

**図 26**: 戻り値にクエリを拡張したり、`SCOPE_IDENTITY()`値 ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image72.png))


新しいメソッドの名前を最後に、`InsertProduct`です。


[![InsertProduct に新しいメソッドの名前を設定します。](creating-a-data-access-layer-vb/_static/image74.png)](creating-a-data-access-layer-vb/_static/image73.png)

**図 27**: 新しいメソッド名を設定します`InsertProduct`([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image75.png))


戻ったら、データセット デザイナーに表示される、`ProductsTableAdapter`新しいメソッドが含まれています`InsertProduct`です。 この新しいメソッドがそれぞれの列にパラメーターがないかどうか、`Products`テーブル、という事態が終了を忘れた場合、`INSERT`をセミコロンでステートメント。 構成、`InsertProduct`メソッド、セミコロン区切りがあることを確認し、`INSERT`と`SELECT`ステートメントです。

既定では、メソッドの問題クエリ以外メソッドであり、影響を受けた行の数を返すことを挿入します。 ただし、場合、`InsertProduct`影響を受ける行の数ではなく、クエリによって返される値を返すメソッド。 これを実現する、調整、`InsertProduct`メソッドの`ExecuteMode`プロパティを`Scalar`です。


[![スカラーの ExecuteMode プロパティを変更します。](creating-a-data-access-layer-vb/_static/image77.png)](creating-a-data-access-layer-vb/_static/image76.png)

**図 28**: 変更、`ExecuteMode`プロパティを`Scalar`([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image78.png))


次のコードはこの新しい`InsertProduct`メソッドの動作。

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample8.vb)]

## <a name="step-5-completing-the-data-access-layer"></a>手順 5: データ アクセス層の完了

注意してください、`ProductsTableAdapters`クラスを返します、`CategoryID`と`SupplierID`値から、`Products`テーブルしますが、含まれていない、`CategoryName`列から、`Categories`テーブルまたは`CompanyName`列から、 `Suppliers`テーブル、製品情報を表示するときに表示したいが、これらの列では可能性があります。 TableAdapter の最初のメソッドを補強できます。 お`GetProducts()`、両方を含める、`CategoryName`と`CompanyName`列の値は、これらの新しい列を含めるに厳密に型指定された DataTable が更新されます。

これで問題が発生、ただしを挿入するため、TableAdapter のメソッドとして、更新され、データの削除は、この最初のメソッドから基づいています。 幸いにも、自動生成されたメソッドの挿入、更新、および削除されないために影響を受けた内のサブクエリ、`SELECT`句。 によって、クエリに追加するよう注意して`Categories`と`Suppliers`サブクエリとしてではなく`JOIN`s おをせずに済むようデータを変更するため、これらのメソッドを再作成します。 右クリックし、`GetProducts()`メソッドで、`ProductsTableAdapter`構成を選択します。 次に、調整、`SELECT`次のような句。

[!code-sql[Main](creating-a-data-access-layer-vb/samples/sample9.sql)]


[![GetProducts() メソッド用の更新プログラム、SELECT ステートメント](creating-a-data-access-layer-vb/_static/image80.png)](creating-a-data-access-layer-vb/_static/image79.png)

**図 29**: 更新プログラム、`SELECT`のステートメント、`GetProducts()`メソッド ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image81.png))


更新した後、 `GetProducts()` DataTable には、次の 2 つの新しい列が含まれます新しいクエリを使用する方法:`CategoryName`と`SupplierName`です。


![製品 DataTable が 2 つの新しい列](creating-a-data-access-layer-vb/_static/image82.png)

**図 30**: `Products` DataTable が 2 つの新しい列


更新する、`SELECT`句、`GetProductsByCategoryID(categoryID)`メソッドもします。

更新する場合、 `GetProducts()` `SELECT`を使用して`JOIN`構文データセット デザイナーことはできませんを挿入する方法を自動生成を更新して、DB を使用してデータベース データの削除がパターンを転送します。 代わりに、手動で作成するかなりと同じようにする必要があります、`InsertProduct`このチュートリアルで先ほどメソッドです。 さらに、手動でする必要がありますを指定、 `InsertCommand`、 `UpdateCommand`、および`DeleteCommand`パターンの更新バッチを使用するプロパティの値します。

## <a name="adding-the-remaining-tableadapters"></a>残りの Tableadapter を追加します。

ここでは、までのみを説明しました、単一データベース テーブルの 1 つの TableAdapter を使用します。 ただし、Northwind データベースには、web アプリケーションで使用する必要がありますをいくつかの関連テーブルが含まれています。 データ テーブルに関連する型指定されたデータセットを複数含めることができます。 そのため、当社 DAL を完了するこれらのチュートリアルで使用する他のテーブルのデータ テーブルを追加する必要があります。 型指定されたデータセットに新しい TableAdapter を追加するにはデータセット デザイナーを開く、デザイナーを右クリックし、追加 を選択/TableAdapter。 新しい DataTable と TableAdapter が作成され、このチュートリアルで前に検査して、ウィザードの手順がこれです。

次の Tableadapter と次のクエリを使用してメソッドを作成するのには数分を取得します。 なお内のクエリ、`ProductsTableAdapter`各製品のカテゴリおよび仕入先名を取得するサブクエリを含めます。 さらに、に従ってしたら、追加している場合既に、`ProductsTableAdapter`クラスの`GetProducts()`と`GetProductsByCategoryID(categoryID)`メソッドです。

- **ProductsTableAdapter**

    - **GetProducts**: 

        [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample10.sql)]
    - **GetProductsByCategoryID**: 

        [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample11.sql)]
    - **GetProductsBySupplierID**: 

        [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample12.sql)]
    - **GetProductByProductID**: 

        [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample13.sql)]
- **CategoriesTableAdapter**

    - **GetCategories**: 

        [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample14.sql)]
    - **GetCategoryByCategoryID**: 

        [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample15.sql)]
- **SuppliersTableAdapter**

    - **GetSuppliers**: 

        [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample16.sql)]
    - **GetSuppliersByCountry**: 

        [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample17.sql)]
    - **GetSupplierBySupplierID**: 

        [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample18.sql)]
- **EmployeesTableAdapter**

    - **GetEmployees**: 

        [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample19.sql)]
    - **GetEmployeesByManager**: 

        [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample20.sql)]
    - **GetEmployeeByEmployeeID**: 

        [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample21.sql)]


[![次の 4 つの Tableadapter を追加した後、データセット デザイナー](creating-a-data-access-layer-vb/_static/image84.png)](creating-a-data-access-layer-vb/_static/image83.png)

**図 31**:、データセット デザイナーの後に、次の 4 つ Tableadapter が追加されました ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>DAL にカスタム コードを追加します。

Tableadapter と型指定されたデータセットに追加された Datatable は XML スキーマ定義ファイルとして表されます (`Northwind.xsd`)。 右クリックしてこのスキーマ情報を表示することができます、`Northwind.xsd`ソリューション エクスプ ローラーでファイルし、コードの表示を選択します。


[![型指定されたデータセットを Northwinds の XML スキーマ定義 (XSD) ファイル](creating-a-data-access-layer-vb/_static/image87.png)](creating-a-data-access-layer-vb/_static/image86.png)

**図 32**: Northwinds 型指定されたデータセットの XML スキーマ定義 (XSD) ファイル ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image88.png))


このスキーマ情報はデザイン時に c# または Visual Basic のコードに変換のコンパイル時または実行時に (必要な場合)、デバッガーでは、この時点でしてステップことができます。 表示するには、この自動生成されたコードに移動クラス ビューおよびドリル、TableAdapter または型指定されたデータセット クラスにします。 場合は、画面に、クラス ビューが表示されない、表示 メニューに移動し、そこから、選択または Ctrl + Shift + C をヒットします。 クラス ビューから、プロパティ、メソッド、および型指定されたデータセットと TableAdapter のクラスのイベントを表示できます。 特定のメソッドのコードを表示するには、クラス ビューでメソッド名をダブルクリックまたは上を右クリックし、定義へ移動します。


![クラス ビューからの定義を選択すると、Go によって自動生成されたコードを調べる](creating-a-data-access-layer-vb/_static/image89.png)

**図 33**: クラス ビューからの定義を選択すると、Go によって自動生成されたコードを調べる


自動生成されたコードは、最適な時間の節約が、コードは、多くの場合、非常に一般的なは、アプリケーションのニーズに合わせてカスタマイズする必要があります。 リスクを自動生成されたコードでは、拡張は、「再生成」と、カスタマイズの内容を上書きするときにコードを生成したツールが決定可能性があります。 .NET 2.0 の新しい部分クラスの概念が簡単に複数のファイル、クラスを分割します。 これにより、自動生成されたクラスに、カスタマイズ設定を上書きしてクトリについて心配することがなく、独自のメソッド、プロパティ、およびイベントを追加することができます。

DAL をカスタマイズする方法を示すためには、追加、`GetProducts()`メソッドを`SuppliersRow`クラスです。 `SuppliersRow`クラスを表します。 1 つのレコード、`Suppliers`テーブル; 各仕入先のことができますプロバイダー 0、多くの製品にように`GetProducts()`は指定された業者の製品を返します。 これを実現するクラスの新しいファイルが作成、`App_Code`という名前のフォルダー`SuppliersRow.vb`し、次のコードを追加します。

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample22.vb)]

この部分クラスがコンパイラに指示する場合を構築、`Northwind.SuppliersRow`クラスを含める、`GetProducts()`定義したメソッド。 プロジェクトをビルドして、クラス ビューに戻るかどうかわかります`GetProducts()`のメソッドとして表示されるようになりました`Northwind.SuppliersRow`です。


![GetProducts() メソッドは、Northwind.SuppliersRow クラスの一部ようになりました](creating-a-data-access-layer-vb/_static/image90.png)

**図 34**:`GetProducts()`メソッドの一部ではこれで、`Northwind.SuppliersRow`クラス


`GetProducts()`として、次のコードに示す特定の業者の製品のセットを列挙するメソッドを使用するようになりました。

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample23.vb)]

このデータは、ASP のいずれかにも表示されることができます。NET のデータの Web コントロールです。 次のページは、2 つのフィールドの GridView コントロールを使用します。

- 各仕入先の名前を表示する BoundField と
- によって返される結果にバインドされている BulletedList コントロールを含む TemplateField、`GetProducts()`各仕入先のメソッドです。

今後のチュートリアルでこのようなマスター/詳細レポートを表示する方法について確認します。 追加するカスタム メソッドを使用して説明するためにここでは、この例の目的は、`Northwind.SuppliersRow`クラスです。

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample24.aspx)]

SuppliersAndProducts.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample25.vb)]


[![左の列、右側にその製品に、業者の会社の名前が表示されません。](creating-a-data-access-layer-vb/_static/image92.png)](creating-a-data-access-layer-vb/_static/image91.png)

**図 35**: の左側の列で、右側にその製品の仕入先の会社名が表示されている ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-vb/_static/image93.png))


## <a name="summary"></a>まとめ

ときに、DAL を作成する web アプリケーションを構築する必要があります、プレゼンテーション層の作成を開始する前に発生している、最初の手順のいずれか。 Visual studio とは、行のコードを記述することがなく、10 ~ 15 分単位で実行できるタスクには型指定されたデータセットに基づく DAL の作成です。 進むチュートリアルは、この DAL に基づいて構築します。 [次のチュートリアル](creating-a-business-logic-layer-vb.md)うまくビジネス ルールの数を定義し、別のビジネス ロジック層でそれらを実装する方法について説明します。

満足プログラミング!

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [厳密に型指定された Tableadapter と VS 2005 と ASP.NET 2.0 でのデータ テーブルを使用して、DAL の構築](https://weblogs.asp.net/scottgu/435498)
- [データ層のコンポーネントを設計し、層間のデータを渡す](https://msdn.microsoft.com/library/ms978496.aspx)
- [Visual Studio 2005 のデータセット デザイナーでのデータ アクセス層をビルドします。](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [ASP.NET 2.0 の構成情報の暗号化アプリケーション](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [TableAdapter の概要](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [指定されたデータセットの操作](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Visual Studio 2005 と ASP.NET 2.0 での厳密に型指定されたデータ アクセスを使用します。](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [TableAdapter のメソッドを拡張する方法](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [ストアド プロシージャからのスカラー データの取得](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>このチュートリアルに含まれるトピックに関するビデオ トレーニング

- [ASP.NET アプリケーションのデータ アクセス層](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [データ グリッドに、データセットを手動でバインドする方法](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [ASP アプリケーションからのデータセットとフィルターを操作する方法](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Ron 緑、Hilton Giesenow、Dennis Patterson、Liz Shulok、Abel Gomez、および Carlos Santos でした。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](master-pages-and-site-navigation-cs.md)
[次へ](creating-a-business-logic-layer-vb.md)
