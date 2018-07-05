---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
title: データ アクセス層を作成 (c#) |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、最初から開始を作成、データ アクセス層 (DAL)、型指定されたデータセットを使用して、データベース内の情報にアクセスします。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/05/2010
ms.topic: article
ms.assetid: cfe2a6a0-1e56-4dc8-9537-c8ec76ba96a4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 3e124edc137db8444c687092f33db7e03aaeb179
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398321"
---
<a name="creating-a-data-access-layer-c"></a>データ アクセス層 (c#) を作成します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_1_CS.exe)または[PDF のダウンロード](creating-a-data-access-layer-cs/_static/datatutorial01cs1.pdf)

> このチュートリアルでは、最初から開始を作成、データ アクセス層 (DAL)、型指定されたデータセットを使用して、データベース内の情報にアクセスします。


## <a name="introduction"></a>はじめに

Web 開発者は、私たちの生活に焦点を絞ってデータを操作します。 コードを取得し、変更、および web ページを収集して集計データを格納するデータベースを作成します。 これは、時間のかかるを ASP.NET 2.0 ではこれらの一般的なパターンを実装するテクニックを紹介する一連の最初のチュートリアルです。 まず、作成、[ソフトウェア アーキテクチャ](http://en.wikipedia.org/wiki/Software_architecture)でのデータ アクセス層 (DAL) ビジネス ロジック層 (BLL)、型指定されたデータセットを使用して構成されていますが、カスタムのビジネス ルールを強制し、プレゼンテーション層が ASP.NET の構成ページ共通のページ レイアウトを共有します。 レポートに移動します、このバックエンド土台をレイアウトされてが後を表示する方法を示す集計、収集、および web アプリケーションからデータを検証します。 簡潔であり、プロセスを視覚的に順番に多数のスクリーン ショットの手順を説明には、これらのチュートリアルが適しています。 各チュートリアルは、c# および Visual Basic バージョンで使用可能なために使用する完全なコードのダウンロードが含まれています。 (この最初のチュートリアルでは、非常に長い時間がかかるが、残りの部分をより消化しやすいチャンク単位で表示されます)。

配置で Northwind データベースの Microsoft SQL Server 2005 Express Edition バージョンこれらのチュートリアルを使用する、**アプリ\_データ**ディレクトリ。 データベースのファイルに加えて、**アプリ\_データ**フォルダーは、別のデータベース バージョンを使用する場合にも、データベースを作成するための SQL スクリプトを格納します。 これらのスクリプトもあり[マイクロソフトから直接ダウンロード](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en)したい場合は、します。 Northwind データベースの別の SQL Server バージョンを使用する場合は、更新する必要があります。、 **NORTHWNDConnectionString**アプリケーションの設定**Web.config**ファイル。 Web アプリケーションは、ファイル システム ベースの Web サイト プロジェクトとして Visual Studio 2005 Professional Edition を使用して構築されました。 ただし、すべてのチュートリアルでは、動作は、Visual Studio 2005 の無料版と同様[Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/)します。  
  
このチュートリアルでは、最初から開始をデータ アクセス層 (DAL)、続いて 2 番目のチュートリアルでは、ビジネス ロジック層 (BLL) を作成してページ レイアウトと 3 番目のナビゲーションで作業を作成します。 チュートリアルでは、3 つが、基盤の上に構築した後は、最初の 3 つに配置されます。 この最初のチュートリアルで説明、そのため、Visual Studio を起動および作業を開始するのには多くがあります。

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>手順 1: Web プロジェクトを作成して、データベースに接続します。

データ アクセス層 (DAL) を作成する前にまず web サイトを作成し、データベースのセットアップに必要があります。 まず、新しいファイル システムに基づく ASP.NET web サイトを作成します。 これを行うには、ファイル メニューに移動し、新しい Web サイトを新しい Web サイト ダイアログ ボックスを表示するを選択します。 ASP.NET Web サイト テンプレートの選択、場所ドロップダウン リストをファイル システムに設定、web サイトを配置するフォルダーを選択および c# 言語を設定します。


[![新しいファイル システムに基づく Web サイトを作成します。](creating-a-data-access-layer-cs/_static/image2.png)](creating-a-data-access-layer-cs/_static/image1.png)

**図 1**: New File System-Based Web サイトの作成 ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image3.png))。


これで新しい web サイトが作成されます、 **Default.aspx** ASP.NET ページと**アプリ\_データ**フォルダー。

作成された web サイトを次の手順は Visual Studio のサーバー エクスプ ローラーで、データベースへの参照を追加します。 サーバー エクスプ ローラーにデータベースを追加するには、テーブル、ストアド プロシージャ、ビュー、および Visual Studio 内からのすべてを追加できます。 テーブルのデータを表示またはクエリ ビルダーを使用して手動でまたは視覚的に、独自のクエリを作成もできます。 さらに、dal に型指定されたデータセットを構築するときに型指定されたデータセットを構築するためのデータベースへのポイントの Visual Studio に必要があります。 その時点では、この接続情報を提供することができます、Visual Studio でサーバー エクスプ ローラーで既に登録されているデータベースのドロップダウン リストが自動的に設定されます。

Northwind データベースをサーバー エクスプ ローラーに追加する手順は、SQL Server 2005 Express Edition データベースを使用するかどうかに依存、**アプリ\_データ**フォルダーまたは Microsoft SQL Server 2000 または 2005 があるかどうか代わりに使用するデータベース サーバーのセットアップ。

## <a name="using-a-database-in-theappdatafolder"></a>アプリケーションでデータベースを使用して\_DataFolder

ダウンロードした websit に配置されている Northwind データベースの SQL Server 2005 Express Edition のバージョンを使用することができる場合に、接続するには、SQL Server 2000 または 2005年データベース サーバーがないか、データベース サーバーにデータベースを追加しなくてもすむようにしたい、杯**アプリ\_データ**フォルダー (**NORTHWND します。MDF**)。

データベースに配置、**アプリ\_データ**フォルダーがサーバー エクスプ ローラーに自動的に追加します。 SQL Server 2005 Express Edition をコンピューターにインストールがあると仮定すると NORTHWND という名前のノードが表示されます。サーバー エクスプ ローラーで MDF を展開し、そのテーブル、ビュー、ストアド プロシージャ、および具合 (図 2 参照) を調べることができます。

**アプリ\_データ** フォルダーは、Microsoft Access にも格納できる **.mdb** SQL Server の対応するように自動的に追加サーバー エクスプ ローラーになるファイル。 常に、SQL Server オプションのいずれかを使用しない場合は[、Microsoft Access バージョンの Northwind データベース ファイルのダウンロード](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN)にドロップし、**アプリ\_データ**ディレクトリ。 ただし、おいてとして Access データベースではないとして SQL Server では、機能豊富な web サイトのシナリオで使用するものではありません。 さらに、いくつかの 35 + チュートリアルへのアクセスでサポートされていない特定のデータベース レベルの機能が使用されます。

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Microsoft SQL Server 2000 または 2005 のデータベース サーバーでデータベースに接続します。

また、データベース サーバーにインストールされている Northwind データベースに接続することがあります。 データベース サーバーにまだインストールされている Northwind データベースがない場合最初にする必要がありますに追加するデータベース サーバーでこのチュートリアルのダウンロードまたはインストール スクリプトを実行して[Northwind の SQL Server 2000 のバージョンをダウンロードインストール スクリプトと](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en)Microsoft の web サイトから直接します。

インストールされているデータベースを作成したら、Visual Studio の [サーバー エクスプ ローラーには、データ接続] ノードを右クリックし、接続の追加を移動します。 ビューに移動して、サーバー エクスプ ローラーが表示されないかどうかは/サーバー エクスプ ローラーまたはヒット Ctrl + Alt + S。 これには、認証情報、およびデータベース名への接続にサーバーを指定できます、接続の追加 ダイアログ ボックスが表示されます。 データベース接続情報を構成し、ok ボタンをクリックしてしました、一度データベースがデータ接続 ノードの下にノードとして追加されます。 そのテーブル、ビュー、ストアド プロシージャ、および具合を探索するデータベース ノードを展開することができます。


![データベース サーバーの Northwind データベースに接続を追加します。](creating-a-data-access-layer-cs/_static/image4.png)

**図 2**: データベース サーバーの Northwind データベースに接続を追加


## <a name="step-2-creating-the-data-access-layer"></a>手順 2: データ アクセス層の作成

使用する場合、データの 1 つは、(web アプリケーションでは、プレゼンテーション層を ASP.NET ページの構成) で、プレゼンテーション層に直接データに固有のロジックを埋め込むことです。 ASP.NET ページのコード部分で ADO.NET コードを記述またはマークアップの部分から SqlDataSource コントロールを使用する形式の時間がかかります。 いずれの場合も、このアプローチは、データ アクセス ロジックをプレゼンテーション層と密に結合します。 推奨のアプローチは、プレゼンテーション層からデータ アクセス ロジックを分離するただし、です。 この別のレイヤーを使用して、DAL を簡単に言えば、データ アクセス層と呼ばは、通常、別のクラス ライブラリ プロジェクトとして実装します。 この階層型アーキテクチャのメリットはも記載されています (これらの利点については、このチュートリアルの最後に「それ以上の読み取り」セクションを参照してください)、このシリーズでは採用するアプローチします。

など、データベースへの接続を作成する基になるデータ ソースに固有のすべてのコードを発行**選択**、**挿入**、 **UPDATE**、および**削除**コマンドとでは、DAL に存在する必要があります。 プレゼンテーション層は、このようなデータ アクセス コードへの参照を含めることはできませんにすべてのデータを要求の DAL の呼び出しを行う代わりにする必要があります。 通常、データ アクセス レイヤーには、基になるデータベースのデータにアクセスするためのメソッドが含まれます。 たとえば、Northwind データベースには**製品**と**カテゴリ**販売および所属するカテゴリの製品を記録するテーブル。 DAL などのメソッドになります。

- **GetCategories()、** これには、すべてのカテゴリに関する情報を返します
- **GetProducts()**、これには、すべての製品に関する情報を返します
- **GetProductsByCategoryID(*categoryID*)**, which will return all products that belong to a specified category
- **GetProductByProductID (*productID*)**、これは、特定の製品に関する情報を返します

これらのメソッドが呼び出されるはデータベースに接続する、適切なクエリを発行し、結果を返します。 これらの結果を返すどのようにすることが重要です。 単に、これらのメソッドにデータセットまたは DataReader データベース クエリによって設定されますを返すことができますを使用してこれらの結果が返される理想的*厳密に型指定されたオブジェクト*します。 厳密に型指定されたオブジェクトは、反対に、弱い型指定のオブジェクトは 1 つのスキーマが実行時まで不明、コンパイル時にスキーマが定義されている厳格です。

たとえば、DataReader と DataSet (既定) は疎に型指定されたオブジェクトでそれらを設定するために使用するデータベース クエリによって返される列のスキーマが定義されているためです。 ような構文を使用する必要があります、弱い型指定の DataTable から特定の列にアクセスする: <strong><em>DataTable</em>です。行[<em>インデックス</em>]["<em>columnName</em>"]</strong>です。 DataTable のこの例での柔軟な型指定は、文字列または序数インデックスを使用して列名にアクセスする必要があるという事実によって発生します。 厳密に型指定された DataTable では、その一方が、プロパティとして実装されたその列のそれぞれ次のようなコードで結果として得られる: <strong><em>DataTable</em>です。行[<em>インデックス</em>].*columnName</strong>* です。

厳密に型指定されたオブジェクトを返すには、開発者は、独自のカスタム ビジネス オブジェクトを作成するか、型指定されたデータセットを使用できます。 ビジネス オブジェクトは、プロパティを持つ通常ビジネス オブジェクトの基になるデータベース テーブルの列を反映するクラスを表している開発者によって実装されます。 型指定されたデータセットは、データベース スキーマとそのメンバーは、厳密に型指定されたこのスキーマに従ってに基づいて Visual Studio によって生成されたクラスです。 型指定されたデータセット自体は、ADO.NET DataSet、DataTable、および DataRow クラスを拡張するクラスで構成されます。 Datatable の厳密に型指定されただけでなく型指定されたデータセットもが追加されました、Tableadapter はデータセットのデータ テーブルを設定し、元のデータベースにデータ テーブル内の変更を伝達するためのメソッドを持つクラス。

> [!NOTE]
> 長所と短所のカスタム ビジネス オブジェクトと型指定されたデータセットを使用する方法の詳細についてを参照してください[データ層コンポーネントの設計と層間のデータの受け渡し](https://msdn.microsoft.com/library/ms978496.aspx)します。


これらのチュートリアルのアーキテクチャの厳密に型指定されたデータセットを使用します。 図 3 は、型指定されたデータセットを使用するアプリケーションの異なる層間のワークフローを示しています。


[![すべてのデータ アクセス コードは、DAL に追いやら](creating-a-data-access-layer-cs/_static/image6.png)](creating-a-data-access-layer-cs/_static/image5.png)

**図 3**: すべてのデータ アクセス コードは、DAL に追いやら ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image7.png))。


## <a name="creating-a-typed-dataset-and-table-adapter"></a>型指定されたデータセットとテーブル アダプターの作成

DAL を作成するには、型指定されたデータセットをプロジェクトに追加することで開始します。 これを実現するには、ソリューション エクスプ ローラーでプロジェクト ノードを右クリックし、新しい項目の追加を選択します。 テンプレートの一覧からデータセット オプションを選択し、名前を付けます**Northwind.xsd**します。


[![プロジェクトに新しいデータセットを追加します。](creating-a-data-access-layer-cs/_static/image9.png)](creating-a-data-access-layer-cs/_static/image8.png)

**図 4**: 新しいデータセットをプロジェクトに追加することも ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image10.png))。


データセットを追加するように求められたら、[追加] をクリックした後、**アプリ\_コード**フォルダー、[はい] を選択します。 型指定されたデータセットのデザイナーが表示されます、され、型指定されたデータセットを最初に TableAdapter を追加することができます、TableAdapter 構成ウィザードが開始されます。

厳密に型指定されたデータのコレクションをとして型指定されたデータセット厳密に型指定された DataTable インスタンス、厳密に型指定された DataRow インスタンスのそれぞれの順番で構成されていますから構成されます。 このチュートリアル シリーズで使用する必要がある、基になるデータベース テーブルのそれぞれの厳密に型指定された DataTable を作成します。 DataTable の作成から始めましょう、**製品**テーブル。

厳密に型指定された Datatable には、その基になるデータベース テーブルからデータにアクセスする方法に関する情報が含まれていないことに留意してください。 DataTable に表示するデータを取得するためには、データ アクセス層として機能する TableAdapter クラスを使用します。 この**製品**DataTable には、TableAdapter にはメソッドが含まれて、 **GetProducts()**、 **GetProductByCategoryID (*categoryID*)** など、プレゼンテーション層からを起動します。 DataTable の役割では、レイヤー間でデータを渡すために使用、厳密に型指定されたオブジェクトとして機能です。

使用するデータベースを選択するよう求められますが、TableAdapter 構成ウィザードを開始します。 ドロップダウン リストでは、サーバー エクスプ ローラーでそれらのデータベースを示しています。 サーバー エクスプ ローラーに、Northwind データベースを追加しなかった場合は、これを行うには、この時点で、新しい接続ボタンをクリックできます。


[![ドロップダウン リストから、Northwind データベースを選択します。](creating-a-data-access-layer-cs/_static/image12.png)](creating-a-data-access-layer-cs/_static/image11.png)

**図 5**: ドロップダウン リストから、Northwind データベースを選択 ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image13.png))。


データベースを選択し、[次へ] をクリックすると、求められます内の接続文字列を保存するかどうか、 **Web.config**ファイル。 接続文字列を保存することによってします必要があるがハード TableAdapter のクラスにコードが、接続文字列情報が、今後変更された場合は、モ ノを簡略化されます。 配置されている構成ファイルに接続文字列を保存することを選択する場合、 **&lt;connectionStrings&gt;** 可能性があるセクション[オプションで暗号化された](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)改善セキュリティや IIS の GUI 管理ツールでは、管理者にとってより適切な新しい ASP.NET 2.0 プロパティ ページで後で変更されました。


[![接続文字列を Web.config に保存します。](creating-a-data-access-layer-cs/_static/image15.png)](creating-a-data-access-layer-cs/_static/image14.png)

**図 6**: への接続文字列を保存**Web.config** ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image16.png))。


次に、最初の厳密に型指定された DataTable のスキーマを定義し、厳密に型指定されたデータセットを設定するときに使用する、TableAdapter の最初のメソッドを提供する必要があります。 これら 2 つの手順は、DataTable に反映されるように、テーブルから列を返すクエリを作成して同時に実行されます。 ウィザードの最後に紹介メソッドの名前をこのクエリにします。 化は、後であれば、プレゼンテーション層からこのメソッドを呼び出すことができます。 メソッドは、定義済みのクエリを実行し、厳密に型指定された DataTable を設定します。

SQL クエリの定義を開始するには、TableAdapter クエリを発行する方最初指定する必要があります。 アドホック SQL ステートメントを使用して、新しいストアド プロシージャを作成または既存のストアド プロシージャを使用してできます。 これらのチュートリアルについては、アドホック SQL ステートメントを使用します。 参照してください[Brian Noyes](http://briannoyes.net/)の記事[Visual Studio 2005 のデータセット デザイナーでデータ アクセス層の構築](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)ストアド プロシージャを使用した例についてはします。


[![アドホック SQL ステートメントを使用して、データをクエリします。](creating-a-data-access-layer-cs/_static/image18.png)](creating-a-data-access-layer-cs/_static/image17.png)

**図 7**: アドホック SQL ステートメントを使用してデータの照会 ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image19.png))。


この時点で手動で SQL クエリで入力できます。 TableAdapter の最初のメソッドを作成するときに、対応する DataTable で表現する必要があるこれらの列を返すクエリに通常でします。 すべての列とのすべての行を返すクエリを作成するためには、**製品**テーブル。


[![テキスト ボックスに、SQL クエリを入力します。](creating-a-data-access-layer-cs/_static/image21.png)](creating-a-data-access-layer-cs/_static/image20.png)

**図 8**: SQL クエリに、テキスト ボックスに入力します ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image22.png))。


または、クエリ ビルダーを使用し、図 9 に示すように、クエリをグラフィカルに構築します。


[![クエリをグラフィカルに作成、クエリ エディターを使用](creating-a-data-access-layer-cs/_static/image24.png)](creating-a-data-access-layer-cs/_static/image23.png)

**図 9**: クエリをグラフィカルに作成、クエリ エディターを使用 ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image25.png))。


クエリを作成した後は、次の画面上に移動する前に、詳細オプション ボタンをクリックします。 Web サイト プロジェクトに「生成の Insert、Update、および Delete ステートメント」はのみ、既定で選択したオプションの詳細クラス ライブラリまたは Windows プロジェクトからこのウィザードを実行する場合は、「オプティミスティック同時実行制御を使用する」オプションも選択します。 オフのままに「オプティミスティック同時実行制御を使用する」オプション今のところです。 今後のチュートリアルでオプティミスティック同時実行制御をについて説明します。


[![生成を挿入、更新、および Delete ステートメントのみオプションを選択します。](creating-a-data-access-layer-cs/_static/image27.png)](creating-a-data-access-layer-cs/_static/image26.png)

**図 10**: のみ生成を挿入、更新、および Delete ステートメント オプションを選択します ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image28.png))。


高度なオプションを確認した後、最後の画面に進むには、[次へ] をクリックします。 ここで、TableAdapter に追加する方法を選択するように求められます。 データを設定するための 2 つのパターンがあります。

- **Datatable**メソッドが作成をパラメーターとして DataTable の受け取りを設定するこの方法に基づいて、クエリの結果。 ADO.NET データ アダプター クラスは、たとえば、このパターンを実装、 **Fill()** メソッドです。
- **DataTable を返す**このアプローチでメソッドを作成およびする DataTable を入力し、メソッドの戻り値は、それを返します。

TableAdapter のいずれかまたは両方のパターンを実装することができます。 ここで提供されるメソッドの名前を変更することもできます。 後者のパターンをこれらのチュートリアル全体で使用するだけでも、両方のチェック ボックスがオンになっているがみましょうままにします。 また、名前ではなくジェネリック**GetData**メソッドを**GetProducts**します。

選択した場合、最後のチェック ボックス"GenerateDBDirectMethods、"を作成します**Insert()**、 **Update()**、および**Delete()** TableAdapter のメソッド。 このオプションをオフのままにすると、すべての更新する必要があります操作には、TableAdapter の唯一 **Update()** メソッド、型指定された DataSet、DataTable、単一の DataRow または Datarow の配列を取得します。 (した場合は、図 9 での高度なプロパティからこのチェック ボックスのオプションをオフ、"生成の Insert、Update、および Delete ステートメントの設定は効果がありません)。このチェック ボックスをオンのままにしてみましょう。


[![GetData から GetProducts にメソッド名を変更します。](creating-a-data-access-layer-cs/_static/image30.png)](creating-a-data-access-layer-cs/_static/image29.png)

**図 11**: からメソッドの名前を変更**GetData**に**GetProducts** ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image31.png))。


[完了] をクリックしてウィザードを完了します。 ウィザードを閉じた後、先ほど作成した DataTable を示すデータセット デザイナーに戻ります。 内の列の一覧を表示できます、**製品**DataTable (**ProductID**、 **ProductName**など)、メソッドのほか、 **ProductsTableAdapter** (**Fill()** と**GetProducts()**)。


[![製品の DataTable と ProductsTableAdapter が型指定されたデータセットに追加されました](creating-a-data-access-layer-cs/_static/image33.png)](creating-a-data-access-layer-cs/_static/image32.png)

**図 12**:**製品**DataTable と**ProductsTableAdapter**型指定されたデータセットに追加されている ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image34.png))。


この時点で 1 つの DataTable に型指定されたデータセットがある (**Northwind.Products**) と厳密に型指定されたデータ アダプター クラス (**NorthwindTableAdapters.ProductsTableAdapter**) で、 **GetProducts()** メソッドです。 これらのオブジェクトのようなコードからすべての製品の一覧へのアクセスに使用できます。

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample1.html)]

このコードでは、データ アクセスに固有のコードの 1 つのビットを記述する必要ありませんでした。 すべての ADO.NET クラスをインスタンス化する必要はなく、任意の SQL クエリ、接続文字列を参照する必要がなかったか、ストアド プロシージャします。 代わりに、TableAdapter は、私たちにとって低レベルのデータ アクセス コードを提供します。

この例で使用される各オブジェクトは、厳密に型指定、Visual Studio の IntelliSense とコンパイル時の型チェックを提供することができますも。 TableAdapter によって返されるすべてのデータ テーブルの ASP.NET データなど、GridView、DetailsView、DropDownList、CheckBoxList、およびその他のいくつかの Web コントロールにバインドできます。 によって返された DataTable をバインドする次の例を示しています、 **GetProducts()** メソッドをコード内の十分な 3 行だけで、GridView、**ページ\_ロード**イベント ハンドラー。

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample2.aspx)]

AllProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample3.cs)]


[![GridView に製品の一覧が表示されます。](creating-a-data-access-layer-cs/_static/image36.png)](creating-a-data-access-layer-cs/_static/image35.png)

**図 13**: GridView に、製品一覧が表示されます ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image37.png))。


この例は、ASP.NET ページの 3 行のコードを記述したことを必須**ページ\_ロード**イベント ハンドラー、後でチュートリアルは、ObjectDataSource を使用して宣言してからデータを取得する方法について説明します、DAL します。 ObjectDataSource ではコードを記述する必要がありますいないと、ページングと並べ替えのサポートもが得!

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>手順 3:、データ アクセス層にメソッドがパラメーター化されたを追加します。

この時点で、 **ProductsTableAdapter** クラスには 1 つのメソッドが **GetProducts()** データベース内のすべての製品が返されます。 すべての製品を使用することは間違いなく役立ちますが、中には、特定の製品、または特定のカテゴリに属するすべての製品に関する情報を取得する場合します。 このような機能をデータ アクセス層に追加するには、パラメーター化されたメソッド、TableAdapter に追加できます。

追加してみましょう。、 **GetProductsByCategoryID (*categoryID*)** メソッドです。 データセット デザイナーに戻り、DAL に新しいメソッドを追加するのには右クリック、 **ProductsTableAdapter** セクション、およびクエリの追加を選択します。


![TableAdapter を右クリックして、クエリを追加](creating-a-data-access-layer-cs/_static/image38.png)

**図 14**: TableAdapter を右クリックして、クエリを追加


最初に、アドホック SQL ステートメントまたは新規または既存のストアド プロシージャを使用してデータベースにアクセスするかどうかについて求められます。 もう一度、アドホック SQL ステートメントを使用するを選択します。 次に、SQL クエリの種類を使用するなどが求められます。 記述する、指定したカテゴリに属するすべての製品を取得するため、**選択**ステートメントの行を返します。


[![複数行を返す SELECT ステートメントを作成します。](creating-a-data-access-layer-cs/_static/image40.png)](creating-a-data-access-layer-cs/_static/image39.png)

**図 15**: 作成 を選択、**選択**ステートメントが行を返します ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image41.png))。


次の手順では、データにアクセスするために使用する SQL クエリを定義します。 同じ使用すると、特定のカテゴリに属する製品だけを返すので、<strong>選択</strong>ステートメントから<strong>GetProducts()</strong>、以下を追加しますが、<strong>場所</strong>句:<strong>が CategoryID = @CategoryID</strong>します。 <strong>@CategoryID</strong>パラメーター、メソッドを作成していますが、対応する型 (つまり、null 許容の整数) の入力パラメーターを必要とする TableAdapter ウィザードを示します。


[![指定されたカテゴリの製品を返すだけのクエリを入力します](creating-a-data-access-layer-cs/_static/image43.png)](creating-a-data-access-layer-cs/_static/image42.png)

**図 16**: 指定されたカテゴリの製品をのみを返すクエリを入力します ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image44.png))。


最後の手順で選択できますデータ アクセス パターンを使用して、できるだけでなく、生成されるメソッドの名前をカスタマイズします。 塗りつぶしのパターンに名前を変更してみましょう<strong>FillByCategoryID</strong>および戻り値の DataTable 戻りパターン (、<strong>取得*X</strong>* メソッド)、使用してみましょう<strong>GetProductsByCategoryID</strong>します。


[![TableAdapter のメソッドの名前を選択します。](creating-a-data-access-layer-cs/_static/image46.png)](creating-a-data-access-layer-cs/_static/image45.png)

**図 17**: TableAdapter のメソッドの名前を選択 ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image47.png))。


ウィザードを完了すると、データセット デザイナーには、新しい TableAdapter のメソッドが含まれています。


![カテゴリは、照会する、製品できるようになりました](creating-a-data-access-layer-cs/_static/image48.png)

**図 18**: The 製品ことによってクエリのカテゴリ


追加する、 **GetProductByProductID(*productID*)** メソッドと同じ手法を使用します。

これらのパラメーター化されたクエリは、データセット デザイナーから直接テストできます。 TableAdapter のメソッドを右クリックし、データのプレビューを選択します。 次に、パラメーターを使用して、[プレビュー] をクリックして値を入力します。


[![製品に属する飲み物のカテゴリにより、それらが表示されます。](creating-a-data-access-layer-cs/_static/image50.png)](creating-a-data-access-layer-cs/_static/image49.png)

**図 19**: これらの製品に属する飲み物のカテゴリが表示されます ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image51.png))。


**GetProductsByCategoryID (*categoryID*)** メソッド、DAL で、指定されたカテゴリの製品のみを表示する ASP.NET ページ今すぐ作成できます。 次の例では、すべての製品飲み物のカテゴリに含まれるが、 **CategoryID** 1。

Beverages.asp

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample4.aspx)]

Beverages.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample5.cs)]


[![これらの製品、飲料カテゴリが表示されます。](creating-a-data-access-layer-cs/_static/image53.png)](creating-a-data-access-layer-cs/_static/image52.png)

**図 20**: 飲料カテゴリ、製品が表示されます ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image54.png))。


## <a name="step-4-inserting-updating-and-deleting-data"></a>手順 4: 挿入、更新、およびデータを削除します。

一般的に使用を挿入、更新、およびデータを削除する 2 つのパターンがあります。 最初のパターンは、データベースの直接のパターンと呼ぶことに、そのメソッドを作成、呼び出されたときに、問題、**挿入**、 **UPDATE**、または**削除**コマンドを1 つのデータベース レコードを操作するデータベース。 このようなメソッドは、挿入、更新、または削除する値を通常一連の対応スカラーの値 (整数、文字列、ブール値、Datetime、およびなど) で渡されます。 このパターンの使用など、**製品**delete メソッドは、整数パラメーターでかかるテーブルを示す、 **ProductID**でしますが、insert メソッドを削除するレコードの文字列、 **ProductName**の 10 進数、 **UnitPrice**、整数、 **UnitsOnStock**など。


[![各挿入、更新、および削除要求は、すぐにデータベースに送信されます。](creating-a-data-access-layer-cs/_static/image56.png)](creating-a-data-access-layer-cs/_static/image55.png)

**図 21**: 各挿入、更新、および削除要求は、すぐにデータベースに送信されます ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image57.png))。


バッチ更新パターンを呼ぶことに、その他のパターンでは、全体 DataSet、DataTable には、または 1 つのメソッドの呼び出しで DataRows のコレクションを更新します。 このパターンで開発者を削除します挿入、および DataTable の Datarow を変更し、更新メソッドにこれらの Datarow または DataTable を渡します。 このメソッドに渡された Datarow の列挙、かどうかがいる変更、追加、または削除されたを決定します (DataRow のを介して[RowState プロパティ](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx)値)、し、各レコードの適切なデータベースの要求を発行します。


[![すべての変更は、Update メソッドが呼び出されたときに、データベースと同期されます。](creating-a-data-access-layer-cs/_static/image59.png)](creating-a-data-access-layer-cs/_static/image58.png)

**図 22**: すべての変更は、Update メソッドが呼び出されたときに、データベースと同期されます ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image60.png))。


TableAdapter では、既定では、バッチ更新パターンを使用するも DB 直接パターンをサポートします。 TableAdapter を作成するときに、詳細プロパティから「を生成する挿入、更新、および Delete ステートメント」オプションを選択おため、 **ProductsTableAdapter** が含まれています、 **Update()** メソッドこれは、バッチ更新パターンを実装します。 具体的には、TableAdapter が含まれています、 **Update()** 型指定されたデータセットを厳密に型指定された DataTable、または 1 つまたは複数の Datarow に渡すことができるメソッドです。 中断した場合、"GenerateDBDirectMethods"チェック ボックスと DB 直接パターン、TableAdapter を作成してもによって実装されて **Insert()** 、 **Update()** 、および **Delete()** メソッドです。

両方のデータ変更パターンを使用して、TableAdapter の**InsertCommand**、 **UpdateCommand**、および**DeleteCommand**を発行するためのプロパティ、**挿入**、 **UPDATE**、および**削除**コマンドをデータベースにします。 検査および変更できる、 **InsertCommand**、 **UpdateCommand**、および**DeleteCommand**データセット デザイナーで TableAdapter をクリックし、次のプロパティプロパティ ウィンドウ。 (、TableAdapter とを選択するかどうかを確認、 **ProductsTableAdapter**オブジェクトが 1 つのプロパティ ウィンドウで、ドロップダウン リストで選択します)。


[![TableAdapter が InsertCommand、UpdateCommand、および DeleteCommand プロパティ](creating-a-data-access-layer-cs/_static/image62.png)](creating-a-data-access-layer-cs/_static/image61.png)

**図 23**: The TableAdapter が**InsertCommand**、 **UpdateCommand**、および**DeleteCommand**プロパティ ([フルサイズのビューをクリックします。イメージ](creating-a-data-access-layer-cs/_static/image63.png))


確認や、これらのデータベース コマンドのプロパティのいずれかの変更には、をクリックして、 **CommandText**サブプロパティで、クエリ ビルダーが表示されます。


[![クエリ ビルダーでの INSERT、UPDATE、および DELETE ステートメントを構成します。](creating-a-data-access-layer-cs/_static/image65.png)](creating-a-data-access-layer-cs/_static/image64.png)

**図 24**: 構成、 **挿入** 、 **更新** と **削除** クエリ ビルダー内のステートメント ([フルサイズのイメージを表示するをクリックします。](creating-a-data-access-layer-cs/_static/image66.png))


次のコード例では、すべての製品を提供が中止されたしないと 25 のユニットがある在庫以下の価格の 2 倍にバッチ更新パターンを使用する方法を示します。

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample6.cs)]

次のコードでは、プログラムで特定の製品を削除してから、1 つを更新する DB 直接パターンを使用して、新しくを追加する方法を示します。

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample7.cs)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>作成するカスタムの挿入、更新、および Delete メソッド

**Insert()**、 **Update()**、および**Delete()** DB ダイレクト メソッドによって作成されたメソッドを少し面倒で、特に多数の列を持つテーブルのことができます。 IntelliSense のヘルプが特にはっきりしない内容なし、前のコード例を見て**製品**テーブルの列に各入力パラメーターにマップされて、 **Update()** と**Insert()** メソッド。 ときにのみする 1 つの列または 2 つの更新またはカスタマイズされた時間がある可能性があります**Insert()** は、おそらく、メソッドは新しく挿入されたレコードの値を返す**IDENTITY** (自動インクリメント)フィールド。

このようなカスタム メソッドを作成するには、データセット デザイナーに戻ります。 TableAdapter を右クリックし、追加のクエリ、TableAdapter ウィザードに戻る を選択します。 2 番目の画面を作成するクエリの種類を指定できます。 新しい製品を追加して、新しく追加されたレコードの値を返しますメソッドを作成しましょう**ProductID**します。 そのため、作成することを選択、**挿入**クエリ。


[![Products テーブルに新しい行を追加するメソッドを作成します。](creating-a-data-access-layer-cs/_static/image68.png)](creating-a-data-access-layer-cs/_static/image67.png)

**図 25**: 新規行を追加するメソッドを作成、**製品**テーブル ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image69.png))。


次の画面で、 **InsertCommand**の**CommandText**が表示されます。 追加することでこのクエリを拡張する **選択スコープ\_IDENTITY()** に挿入された最後の id 値を返しますクエリの最後に、 **IDENTITY** 同じスコープ内の列です。 (を参照してください、[のテクニカル ドキュメント](https://msdn.microsoft.com/library/ms190315.aspx)の詳細については **スコープ\_IDENTITY()** と多くの場合にその理由[スコープを使用して\_IDENTITY() の代わりに @@IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).)終了するかどうかを確認、**挿入**をセミコロンでステートメントを追加する前に、**選択**ステートメント。


[![Scope_identity() で値を返すクエリを拡張します。](creating-a-data-access-layer-cs/_static/image71.png)](creating-a-data-access-layer-cs/_static/image70.png)

**図 26**: 戻り値にクエリを拡張したり、 **スコープ\_IDENTITY()** 値 ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-cs/_static/image72.png))


新しいメソッドの名前を最後に、 **InsertProduct**します。


[![InsertProduct に新しいメソッドの名前を設定します。](creating-a-data-access-layer-cs/_static/image74.png)](creating-a-data-access-layer-cs/_static/image73.png)

**図 27**: 新しいメソッドの名前を設定**InsertProduct** ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image75.png))。


戻ると、データセット デザイナーに表示されている、 **ProductsTableAdapter**含まれる新しいメソッドは**InsertProduct**します。 この新しいメソッドが、パラメーター内の各列がないかどうか、**製品**テーブル、可能性は終了していません、**挿入**をセミコロンでステートメント。 構成、 **InsertProduct**メソッド セミコロンで区切る必要があることを確認し、**挿入**と**選択**ステートメント。

既定では、影響を受けた行の数を返す、つまり、メソッドを問題非クエリ メソッドを挿入します。 ただし、必要、 **InsertProduct**影響を受ける行の数ではなく、クエリによって返される値を返すメソッド。 これを行うには、調整、 **InsertProduct**メソッドの**ExecuteMode**プロパティを**スカラー**します。


[![スカラーの ExecuteMode プロパティを変更します。](creating-a-data-access-layer-cs/_static/image77.png)](creating-a-data-access-layer-cs/_static/image76.png)

**図 28**: 変更、 **ExecuteMode**プロパティを**スカラー** ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image78.png))。


次のコードはこの新しい**InsertProduct**メソッドの動作。

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample8.cs)]

## <a name="step-5-completing-the-data-access-layer"></a>手順 5: データ アクセス層の完了

なお、 **ProductsTableAdapters**クラスを返します、 **CategoryID**と**SupplierID**値から、**製品**テーブルが含まれていない、 **CategoryName**列から、**カテゴリ**テーブルまたは**CompanyName**列から、 **Suppliers**テーブル、製品情報を表示するときに表示する必要がある列は可能性があります。 TableAdapter の最初のメソッドの強化点は**GetProducts()**、両方を含める、 **CategoryName**と**CompanyName**列の値は、更新は、これらの新しい列を含める厳密に型指定された DataTable です。

これで問題が発生、ただし、挿入すると、TableAdapter のメソッドとして更新、およびデータの削除がこの最初のメソッドに基づいています。 さいわい、自動生成されたメソッドの挿入、更新、および削除は影響を受ける内のサブクエリ、**選択**句。 クエリを追加するよう注意して**カテゴリ**と**Suppliers**サブクエリとしてなく**参加**s、避けデータを変更するため、これらのメソッドを作り直す必要です。 右クリックし、 **GetProducts()** メソッドで、 **ProductsTableAdapter** 構成を選択します。 次に、調整、**選択**次のような句。

[!code-sql[Main](creating-a-data-access-layer-cs/samples/sample9.sql)]


[![GetProducts() メソッドの SELECT ステートメントを更新します。](creating-a-data-access-layer-cs/_static/image80.png)](creating-a-data-access-layer-cs/_static/image79.png)

**図 29**: 更新プログラム、 **選択** のステートメント、 **GetProducts()** メソッド ([フルサイズのイメージを表示するをクリックして](creating-a-data-access-layer-cs/_static/image81.png))


更新した後、 **GetProducts()** メソッドを使用して、この新しいクエリを DataTable には、2 つの新しい列が含まれます: **CategoryName**と**仕入**します。


![製品の DataTable が 2 つの新しい列](creating-a-data-access-layer-cs/_static/image82.png)

**図 30**:**製品**DataTable が 2 つの新しい列


更新する、 **選択** 句、 **GetProductsByCategoryID(*categoryID*)** メソッドもします。

更新する場合、 **GetProducts()** **選択**を使用して**参加**構文データセット デザイナーは、挿入、更新、および削除する方法を自動生成することはできませんDB の直接のパターンを使用してデータベースのデータ。 代わりに、手動で作成してはるかを扱ったようにする必要があります、 **InsertProduct**このチュートリアルで先ほどメソッド。 さらに、手動でする必要があります提供、 **InsertCommand**、 **UpdateCommand**、および**DeleteCommand**プロパティ値が、バッチ更新パターンを使用する場合。

## <a name="adding-the-remaining-tableadapters"></a>残りの Tableadapter を追加します。

これまでは、1 つのデータベース テーブルの 1 つの TableAdapter の操作でのみ確認しました。 ただし、Northwind データベースには、web アプリケーションで使用する必要がありますをいくつかの関連テーブルが含まれています。 データ テーブルに関連する型指定されたデータセットを複数含めることができます。 そのため、完了、DAL にこれらのチュートリアルで使用する他のテーブルのデータ テーブルを追加する必要があります。 に型指定されたデータセットを新しい TableAdapter を追加するデータセット デザイナーを開き、デザイナーで、右クリックして追加 を選択/TableAdapter。 新しい DataTable と TableAdapter の作成を調べるこのチュートリアルで前にウィザードについて説明します。

次の Tableadapter と次のクエリを使用してメソッドを作成に数分かかります。 なお内のクエリ、 **ProductsTableAdapter**各 product の category と supplier の名前を取得するサブクエリが含まれます。 さらに、に従ってしたら、追加している場合既に、 **ProductsTableAdapter** クラスの **GetProducts()** と **GetProductsByCategoryID(*categoryID*)** メソッドです。

- **ProductsTableAdapter**

  - **GetProducts**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample10.sql)]
  - **GetProductsByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample11.sql)]
  - **GetProductsBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample12.sql)]
  - **GetProductByProductID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample13.sql)]
- **CategoriesTableAdapter**

  - **GetCategories**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample14.sql)]
  - **GetCategoryByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample15.sql)]
- **SuppliersTableAdapter**

  - **GetSuppliers**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample16.sql)]
  - **GetSuppliersByCountry**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample17.sql)]
  - **GetSupplierBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample18.sql)]
- **EmployeesTableAdapter**

  - **GetEmployees**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample19.sql)]
  - **GetEmployeesByManager**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample20.sql)]
  - **GetEmployeeByEmployeeID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample21.sql)]


[![次の 4 つの Tableadapter を追加した後、データセット デザイナー](creating-a-data-access-layer-cs/_static/image84.png)](creating-a-data-access-layer-cs/_static/image83.png)

**図 31**:、データセット デザイナーの後、次の 4 つ Tableadapter が追加されています ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image85.png))。


## <a name="adding-custom-code-to-the-dal"></a>DAL にカスタム コードを追加します。

Tableadapter と型指定されたデータセットに追加されたデータ テーブルは、XML スキーマ定義ファイルとして表されます (**Northwind.xsd**)。 右クリックしてこのスキーマ情報を表示することができます、 **Northwind.xsd**ソリューション エクスプ ローラーでファイルし、コードの表示を選択します。


[![型指定されたデータセットを Northwinds の XML スキーマ定義 (XSD) ファイル](creating-a-data-access-layer-cs/_static/image87.png)](creating-a-data-access-layer-cs/_static/image86.png)

**図 32**: Northwinds 型指定されたデータセットの XML スキーマ定義 (XSD) ファイル ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image88.png))。


このスキーマ情報は、コンパイル時または実行時に (必要な) 場合、この時点でをステップ実行できますが、デバッガーでデザイン時に c# または Visual Basic のコードに変換されます。 この自動生成されたコード」をご覧くださいクラス ビューとドリル TableAdapter または型指定されたデータセット クラスにします。 画面上のクラス ビューが表示されない場合は、表示 メニューに移動するかと、そこから選択します Ctrl + Shift + C をヒットします。 クラス ビューから、プロパティ、メソッド、および、型指定されたデータセットおよび TableAdapter クラスのイベントを確認できます。 特定のメソッドのコードを表示するには、クラス ビューでは、メソッド名をダブルクリックまたは右クリックし、定義へ移動 を選択します。


![クラス ビューから定義へ移動を選択して、自動生成されたコードを検査します。](creating-a-data-access-layer-cs/_static/image89.png)

**図 33**: Go を定義するクラス ビューから選択して、自動生成されたコードの検査


自動生成されたコードでは、優れた時間の節約をすることができます、コードは非常に一般的な多くの場合であり、アプリケーションの独自のニーズに合わせてカスタマイズする必要があります。 自動生成されたコードでは、拡張のリスクは「再生」し、カスタマイズした内容を上書きする可能性があります、コードを生成したツールで決定します。 .NET 2.0 の新しい部分クラスの概念では、複数のファイルをクラスに分割する簡単です。 これにより、自動生成されたクラスに、カスタマイズが上書きされる Visual Studio について心配することがなく、独自のメソッド、プロパティ、およびイベントを追加することができます。

DAL をカスタマイズする方法を示すためには、追加、 **GetProducts()** メソッドを**SuppliersRow**クラス。 **SuppliersRow** クラスを表します 1 つのレコード、 **Suppliers** テーブル; 各仕入先のことができますプロバイダー 0、多くの製品にように **GetProducts()** ものを返します。指定された業者の製品です。 これで新しいクラス ファイルが作成を実現する、**アプリ\_コード**という名前のフォルダー **SuppliersRow.cs**し、次のコードを追加します。

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample22.cs)]

この部分クラスがコンパイラに指示する場合の構築、 **Northwind.SuppliersRow**クラスを**GetProducts()** メソッドを定義しました。 プロジェクトを構築し、クラス ビューに戻るかどうかが表示されます**GetProducts()** のメソッドとして一覧表示されます。 **Northwind.SuppliersRow**します。


![GetProducts() メソッドは Northwind.SuppliersRow クラスの一部になりました](creating-a-data-access-layer-cs/_static/image90.png)

**図 34**: **GetProducts()** メソッドの一部ではこれで、 **Northwind.SuppliersRow** クラス


**GetProducts()** として、次のコードに示す特定の業者の製品のセットを列挙するメソッドを使用するようになりました。

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample23.html)]

このデータは、ASP のいずれかでも表示されます。NET のデータ Web コントロール。 次のページでは、2 つのフィールドと GridView コントロールを使用します。

- 各仕入先の名前を表示する BoundField と
- によって返される結果にバインドされている BulletedList コントロールを含む TemplateField、 **GetProducts()** 各仕入先のメソッドです。

今後のチュートリアルでこのようなマスター/詳細レポートを表示する方法を考察します。 説明に追加されたカスタム メソッドを使用するためにここでは、この例の目的は、 **Northwind.SuppliersRow**クラス。

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample24.aspx)]

SuppliersAndProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample25.cs)]


[![左の列を右にあるその製品の仕入先の会社名が一覧表示します。](creating-a-data-access-layer-cs/_static/image92.png)](creating-a-data-access-layer-cs/_static/image91.png)

**図 35**: 左側の列で、右側で、製品、仕入先の会社名が表示されている ([フルサイズの画像を表示する をクリックします](creating-a-data-access-layer-cs/_static/image93.png))。


## <a name="summary"></a>まとめ

ときに DAL を作成する web アプリケーションを構築する必要がある、プレゼンテーション層の作成を開始する前に発生している、最初の手順のいずれか。 Visual Studio を使用して、行のコードを記述することがなく、10 ~ 15 分に実行できるタスクは、型指定されたデータセットに基づいて DAL を作成します。 今後のチュートリアルはこの DAL に基づいて進められます。 [次のチュートリアル](creating-a-business-logic-layer-cs.md)をビジネス ルールの数を定義し、別のビジネス ロジック層で実装する方法を参照してください。

満足のプログラミングです。

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [厳密に型指定された Tableadapter と VS 2005 および ASP.NET 2.0 でのデータ テーブルを使用して DAL の構築](https://weblogs.asp.net/scottgu/435498)
- [データ層のコンポーネントを設計して、層間のデータを渡す](https://msdn.microsoft.com/library/ms978496.aspx)
- [Visual Studio 2005 のデータセット デザイナーでデータ アクセス層を構築します。](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [ASP.NET 2.0 の構成情報の暗号化アプリケーション](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [TableAdapter の概要](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [型指定されたデータセットの操作](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Visual Studio 2005 および ASP.NET 2.0 でアクセスを厳密に型指定されたデータを使用します。](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [TableAdapter のメソッドを拡張する方法](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [ストアド プロシージャからのスカラー データの取得](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>このチュートリアルに含まれるトピックのビデオ トレーニング

- [ASP.NET アプリケーションのデータ アクセス層](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [データ グリッドにデータセットを手動でバインドする方法](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [ASP アプリケーションからのデータセットとフィルターを操作する方法](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Ron 緑、Hilton Giesenow、Dennis Patterson、Liz Shulok、Abel Gomez、および Carlos Santos でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [次へ](creating-a-business-logic-layer-cs.md)
