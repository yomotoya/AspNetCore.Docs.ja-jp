---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
title: SqlDataSource コントロール (c#) によるデータのクエリ |Microsoft Docs
author: rick-anderson
description: 上記のチュートリアルでは、ObjectDataSource コントロールを使用して、データ アクセス層からプレゼンテーション層を完全に分離します。 この家庭教師としてを開始しています.
ms.author: riande
ms.date: 02/20/2007
ms.assetid: 60512d6a-b572-4b7a-beb3-3e44b4d2020c
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 0d15e09c2b790c4d1e6b278c4ea35bab7f66b861
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828921"
---
<a name="querying-data-with-the-sqldatasource-control-c"></a>SqlDataSource コントロール (c#) によるデータのクエリ
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_CS.exe)または[PDF のダウンロード](querying-data-with-the-sqldatasource-control-cs/_static/datatutorial47cs1.pdf)

> 上記のチュートリアルでは、ObjectDataSource コントロールを使用して、データ アクセス層からプレゼンテーション層を完全に分離します。 SqlDataSource コントロールを使用して、単純なアプリケーションのプレゼンテーションとデータ アクセスの厳密な分離を必要としない方法を学習以降このチュートリアルでは、します。


## <a name="introduction"></a>はじめに

チュートリアルのすべてこれまでに調査したプレゼンテーション、ビジネス ロジック、およびデータ アクセス層から成る階層型アーキテクチャを使用します。 データ アクセス層 (DAL) が最初のチュートリアルで作成された ([データ アクセス層を作成する](../introduction/creating-a-data-access-layer-cs.md)) と、2 番目のビジネス ロジック層 ([ビジネス ロジック層を作成する](../introduction/creating-a-business-logic-layer-cs.md))。 以降では、 [、ObjectDataSource でデータを表示する](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)チュートリアルでは、プレゼンテーション層のアーキテクチャとのインターフェイス宣言によって ASP.NET 2.0 の新しい ObjectDataSource コントロールを使用する方法を説明しました。

データを操作するすべてのチュートリアルのこれまでに、アーキテクチャを使用して、アクセス、挿入、更新、およびアーキテクチャをバイパスして、ASP.NET ページから直接、データベースのデータを削除することがもできます。 これにより、web ページに直接、特定のデータベース クエリとビジネス ロジックを配置します。 十分に大規模または複雑なアプリケーションの場合、設計、実装、および階層型アーキテクチャを使用するが、成功した場合、更新、およびアプリケーションの保守容易性のきわめて重要です。 堅牢なアーキテクチャを開発するには、ただし、必要がありますいない非常にシンプルで 1 回限りのアプリケーションを作成するときにします。

ASP.NET 2.0 には 5 つの組み込みデータ ソース コントロール[SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx)、 [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx)、 [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx)、 [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx)、および[SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx)します。 アクセスして、Microsoft SQL Server、Microsoft Access、Oracle、MySQL、および他のユーザーを含む、リレーショナル データベースから直接データを変更する、SqlDataSource を使用できます。 このチュートリアルと、次の 3 つの場合は、SqlDataSource コントロールを操作すると、挿入、更新、およびデータを削除するクエリを実行する方法とデータベースのデータのフィルター、ほかに SqlDataSource を使用する方法を調査する方法を考察します。


![ASP.NET 2.0 には、5 つの組み込みのデータ ソース コントロールが含まれています。](querying-data-with-the-sqldatasource-control-cs/_static/image1.gif)

**図 1**: ASP.NET 2.0 には、5 つの組み込みのデータ ソース コントロールが含まれています。


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>ObjectDataSource や SqlDataSource を比較します。

概念的には、ObjectDataSource や SqlDataSource の両方のコントロールは、データへのプロキシだけです。 説明したように、 [、ObjectDataSource でデータを表示する](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)チュートリアルでは、ObjectDataSource が挿入、更新、およびデータの削除、データとを選択するために呼び出すメソッドを提供するオブジェクトの種類を示すプロパティ基になるオブジェクトの種類。 データ Web コントロール、GridView、DetailsView、DataList などを ObjectDataSource s を使用して、コントロールにバインドできます、ObjectDataSource プロパティを構成すると、 `Select()`、 `Insert()`、 `Delete()`、および`Update()`メソッド基になるアーキテクチャと対話します。

SqlDataSource は、同じ機能を提供しますが、オブジェクト ライブラリではなく、リレーショナル データベースに作用します。 SqlDataSource によるデータベース接続文字列と、アドホック SQL クエリを指定する必要があります。 またはストアド プロシージャを挿入、更新、削除、およびデータを取得するために実行します。 SqlDataSource s `Select()`、 `Insert()`、 `Update()`、および`Delete()`メソッドを呼び出すと、指定されたデータベースに接続して、適切な SQL クエリを発行します。 次の図は、これらとメソッドは、データベースへの接続、クエリの結果を返すの単調でつらい作業を行います。


![SqlDataSource は、データベースへのプロキシとして機能します。](querying-data-with-the-sqldatasource-control-cs/_static/image2.gif)

**図 2**: SqlDataSource は、データベースへのプロキシとして機能


> [!NOTE]
> このチュートリアルでは、データベースからデータを取得中に注目します。 [挿入、更新、および、SqlDataSource コントロールでデータを削除しても](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)チュートリアルでは、挿入、更新、および削除をサポートするために SqlDataSource を構成する方法を見ていきます。


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>SqlDataSource と AccessDataSource コントロール

ASP.NET 2.0 には、SqlDataSource コントロールだけでなく、AccessDataSource コントロールも含まれます。 これら 2 つの異なるコントロールは、Microsoft SQL Server のみで動作するように設計、SqlDataSource コントロールで Microsoft へのアクセスのみで動作する AccessDataSource コントロールがデザインされたことを ASP.NET 2.0 に新しい多くの開発者があります。 SqlDataSource コントロールが連携する、AccessDataSource は、Microsoft Access の動作に設計されていますが、*任意*リレーショナル データベース .NET 経由でアクセスできます。 これには、OleDb、ODBC に準拠しているなどのデータ ストア、Microsoft SQL Server、Microsoft Access、Oracle、Informix、MySQL、および PostgreSQL、他の多くが含まれます。

AccessDataSource、SqlDataSource コントロールの唯一の違いは、データベース接続情報を指定する方法です。 AccessDataSource コントロールでは、Access データベース ファイルへのファイル パスだけが必要です。 一方で、SqlDataSource には、完全な接続文字列が必要です。

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>手順 1: SqlDataSource Web ページの作成

SqlDataSource コントロールを使用してデータベースのデータを直接操作する方法の調査を始める前に、このチュートリアルと、次の 3 つの必要がありますが、web サイト プロジェクトで ASP.NET ページを作成する最初に少し s を使用できます。 という名前の新しいフォルダーを追加することで開始`SqlDataSource`します。 次に、次の ASP.NET ページを使用する各ページに関連付けることを確認、そのフォルダーに追加、`Site.master`マスター ページ。

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![SqlDataSource に関連するチュートリアルについては、ASP.NET ページに追加します。](querying-data-with-the-sqldatasource-control-cs/_static/image3.gif)

**図 3**: SqlDataSource に関連するチュートリアルについては、ASP.NET ページに追加します。


などの他のフォルダーで`Default.aspx`で、`SqlDataSource`フォルダーは、チュートリアルのセクションで一覧表示します。 いることを思い出してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 そのため、このユーザー コントロールを追加`Default.aspx`をページのデザイン ビューに ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](querying-data-with-the-sqldatasource-control-cs/_static/image5.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image4.gif)

**図 4**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズの画像を表示する をクリックします](querying-data-with-the-sqldatasource-control-cs/_static/image6.gif))。


最後に、これら 4 つのページを追加するエントリとして、`Web.sitemap`ファイル。 具体的には、DataList と Repeater にカスタム ボタンの追加後、次のマークアップを追加`<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample1.sql)]

更新した後`Web.sitemap`、時間、ブラウザーを使ってチュートリアル web サイトを表示するのにはかかりません。 左側のメニューには、編集、挿入、および削除のチュートリアルの項目が含まれています。


![サイト マップ、SqlDataSource チュートリアル用のエントリになりました](querying-data-with-the-sqldatasource-control-cs/_static/image7.gif)

**図 5**: サイト マップ、SqlDataSource チュートリアル用のエントリになりました


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>手順 2: 追加して、SqlDataSource コントロールを構成します。

開いて開始、`Querying.aspx`ページで、`SqlDataSource`フォルダーとデザイン ビューに切り替えます。 SqlDataSource コントロールをドラッグして、ツールボックスからデザイナーとセットにその`ID`に`ProductsDataSource`します。 同様に、ObjectDataSource、SqlDataSource がレンダリングされた出力を生成しないしたがってデザイン サーフェイス上の灰色のボックスとして表示されます。 SqlDataSource で構成するには、SqlDataSource s のスマート タグからのデータ ソースの構成のリンクをクリックします。


![をクリックして、SqlDataSource s のスマート タグからのデータ ソースのリンクを構成します。](querying-data-with-the-sqldatasource-control-cs/_static/image8.gif)

**図 6**: をクリックして、SqlDataSource s のスマート タグからのデータ ソースのリンクを構成します。


SqlDataSource コントロールのデータ ソースの構成ウィザードが表示されます。 ウィザードの手順は、ObjectDataSource コントロール s と異なる場合、取得、挿入、更新、およびデータ ソースを使用してデータを削除する方法の詳細を提供する同じ最終目標は。 SqlDataSource には使用する基になるデータベースを指定して、アドホック SQL ステートメントまたはストアド プロシージャを提供する必要があります。

ウィザードの最初の手順には、データベースのことが求められます。 ドロップダウン リストには、web アプリケーションの s に、データベースが含まれています。`App_Data`フォルダーと、サーバー エクスプ ローラーでデータ接続 ノードに追加されています。 以降既に追加の接続文字列、`NORTHWIND.MDF`データベースに、 `App_Data` s プロジェクトにフォルダー`Web.config`ファイル、ドロップダウン リストには、その接続文字列への参照が含まれています。`NORTHWINDConnectionString`します。 ドロップダウン リストからこの項目を選択し、[次へ] をクリックします。


![ドロップダウン リストから、NORTHWINDConnectionString を選択します。](querying-data-with-the-sqldatasource-control-cs/_static/image9.gif)

**図 7**: 選択、`NORTHWINDConnectionString`ドロップダウン リストから


データベースを選択すると、ウィザードでデータを返すクエリ要求します。 テーブルまたはビューを返すまたはカスタム SQL ステートメントを入力したり、ストアド プロシージャを指定の列を指定することができますか。 カスタム SQL ステートメントまたはストアド プロシージャとテーブルから列を指定の指定では、この選択を切り替えるまたはラジオ ボタンを表示できます。

> [!NOTE]
> この最初の例では、テーブルまたはビューのオプションから列を指定を使用して、s ことができます。 このチュートリアルの後半で、ウィザードに戻り、カスタム SQL ステートメントまたはストアド プロシージャ オプションの指定を確認します。


図 8 はテーブルまたはビューのオプション ボタンからを指定する列を選択すると、構成に Select ステートメントの画面を示します。 ドロップダウン リストには、テーブルとビューを選択したテーブルまたは下のチェック ボックスの一覧に表示されるビューの列で、Northwind データベースのセットが含まれています。 この例では、let s を返す、 `ProductID`、 `ProductName`、および`UnitPrice`列から、`Products`テーブル。 図 8 に示す結果の SQL ステートメントが表示されたら、ウィザードでこれらの選択を行うよう`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`します。


![Products テーブルからデータを返す](querying-data-with-the-sqldatasource-control-cs/_static/image10.gif)

**図 8**: からデータを返す、`Products`テーブル


返す、ウィザードを構成した後、 `ProductID`、 `ProductName`、および`UnitPrice`列から、`Products`テーブルで、[次へ] ボタンをクリックします。 この最後の画面は、前の手順から構成されているクエリの結果を確認する機会を提供します。 構成済みで実行されるクエリのテスト ボタンをクリックして`SELECT`ステートメントと、結果をグリッドに表示します。


![SELECT クエリを確認するテスト クエリ ボタンをクリックします](querying-data-with-the-sqldatasource-control-cs/_static/image11.gif)

**図 9**: レビュー、テスト クエリ ボタンをクリックして`SELECT`クエリ


ウィザードを完了するには、[完了] をクリックします。

ObjectDataSource で SqlDataSource のウィザード単に値を割り当てます s のコントロールのプロパティ、namely と同様に、 [ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx)と[ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx)プロパティ。 ウィザードを完了すると、SqlDataSource コントロールの宣言型マークアップは次のようになります。


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample2.aspx)]

`ConnectionString`プロパティは、データベースに接続する方法の情報を提供します。 このプロパティは、完全なハード コーディングされた接続文字列の値を割り当てることができるまたは内の接続文字列の指すことができます`Web.config`します。 Web.config で接続文字列の値を参照する構文を使用して、`<%$ expressionPrefix:expressionValue %>`します。 通常、*されて*は ConnectionStrings と*expressionValue*で接続文字列の名前を指定します、 `Web.config` [ `<connectionStrings>`セクション](https://msdn.microsoft.com/library/bf7sd233.aspx)します。 ただし、構文を使用できますの参照を`<appSettings>`要素またはリソース ファイルからのコンテンツ。 参照してください[ASP.NET 式の概要](https://msdn.microsoft.com/library/d5bd1tad.aspx)の詳細については、この構文。

`SelectCommand`プロパティは、アドホック SQL ステートメントまたはデータを返すに実行するストアド プロシージャを指定します。

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>手順 3: データ Web コントロールを追加して、SqlDataSource へのバインド

SqlDataSource が構成されていると、データ、GridView や DetailsView などの Web コントロールにバインドできます。 このチュートリアルでは、s を GridView にデータを表示することができます。 ツールボックスからページに GridView をドラッグしにバインド、 `ProductsDataSource` SqlDataSource GridView s のスマート タグのドロップダウン リストから、データ ソースを選択します。


[![GridView を追加し、SqlDataSource コントロールにバインドします。](querying-data-with-the-sqldatasource-control-cs/_static/image13.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image12.gif)

**図 10**: GridView を追加し、SqlDataSource コントロールにバインドする ([フルサイズの画像を表示する をクリックします](querying-data-with-the-sqldatasource-control-cs/_static/image14.gif))。


GridView s のスマート タグの一覧にしているが、SqlDataSource コントロールを選択すると、Visual Studio が自動的に追加 BoundField または CheckBoxField GridView にデータ ソース コントロールによって返される列の各します。 SqlDataSource は 3 つのデータベース列を返すので`ProductID`、 `ProductName`、および`UnitPrice`GridView で 3 つのフィールドがあります。

GridView の 3 つを構成する少し BoundFields します。 変更、`ProductName`フィールド s`HeaderText`プロパティを製品名、および`UnitPrice`価格フィールド s。 書式設定も、`UnitPrice`通貨フィールド。 これらの変更を行った後、GridView s の宣言型マークアップを次のようになります。


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample3.aspx)]

ブラウザーからこのページを参照してください。 図 11 に示すよう、GridView が s の各製品を一覧表示`ProductID`、 `ProductName`、および`UnitPrice`値。


[![GridView は、各製品の ProductID、ProductName、および UnitPrice の値が表示されます。](querying-data-with-the-sqldatasource-control-cs/_static/image16.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image15.gif)

**図 11**:、GridView 表示の各製品 s `ProductID`、 `ProductName`、および`UnitPrice`値 ([フルサイズの画像を表示する をクリックします](querying-data-with-the-sqldatasource-control-cs/_static/image17.gif))。


GridView がそのデータ ソース コントロール s を呼び出すページがアクセスしたときに`Select()`メソッド。 ObjectDataSource コントロールを使用していますが、これと呼ばれます、`ProductsBLL`クラスの`GetProducts()`メソッド。 ただし、SqlDataSource で、`Select()`メソッドは、指定されたデータベースとの問題への接続を確立、 `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`、この例では)。 SqlDataSource では、GridView、列挙、返されるデータベースのレコードを GridView の行を作成する、結果を返します。

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>組み込みのデータ Web コントロールの機能と、SqlDataSource コントロール

一般に、Web コントロールのページング、並べ替え、編集、データに固有の機能データ Web コントロールに固有の削除、挿入、およびなどとは、データ ソース コントロールの使用に依存しません。 つまり、GridView では、ページング、並べ替え、編集、および、ObjectDataSource や SqlDataSource にバインドされているかどうかを削除する、その組み込みを利用できます。 ただし、特定のデータ Web コントロールの機能は使用されているデータ ソース コントロールに、またはデータ ソース コントロールの構成に機密性の高い。

たとえば、[効率的にページングを大規模な量のデータ](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md)方法については、既定では、Web データのページング ロジックを制御世間知らず返しますを説明したチュートリアル*すべて*基礎となるレコードデータ ソースを現在のページ インデックスと 1 ページに表示するレコードの数を指定するレコードの適切なサブセットのみが表示されます。 このモデルは十分に大きな結果セットをページングするときは非常に効率的なありません。 さいわい、ObjectDataSource は、表示するレコードの正確なサブセットのみを返すカスタム ページングをサポートするために構成できます。 ただし、SqlDataSource コントロールには、カスタム ページングを実装するためのプロパティが不足しています。

SqlDataSource でページングと並べ替えのもう 1 つ微妙に発生します。 既定では、SqlDataSource から返されるデータをページまたは GridView で並べ替えができます。 これを示すためには、GridView s のスマート タグでページングを有効にして、並べ替えを有効にするオプションを確認して`Querying.aspx`予想したとおりに動作することを確認します。

並べ替えとページングは SqlDataSource 疎に型指定されたデータセットに、データベースのデータを取得するためには機能します。 データセットのクエリによって返される、重要な側面にページングを実装するレコードの合計数を確認できます。 さらに、データセットの結果は、DataView で並べ替えられます。 これらの機能は、GridView 要求ページまたはデータを並べ替えるときに自動的に SqlDataSource によって使用されます。

変更することで、データセットではなく、DataReader を返す、SqlDataSource を構成することができます、 [ `DataSourceMode`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx)から`DataSet`(既定値) に`DataReader`します。 SqlDataSource s の結果を DataReader が必要とする既存のコードに渡すときの状況で DataReader を使用してを優先可能性があります。 さらに、Datareader がデータセットよりも大幅に単純なオブジェクトであるため、パフォーマンスの向上を提供します。 ただし、この変更を加えた場合は、データ Web コントロールを並べ替えることができますも SqlDataSource、クエリによって返されるレコードの数を確認することはできませんまたは DataReader からのページが返されたデータを並べ替えるための手法を提供します。

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>手順 4: カスタム SQL ステートメントを使用してストアド プロシージャまたは

SqlDataSource コントロールを構成するときに、カスタム SQL ステートメントまたはストアド プロシージャ、または既存のテーブルまたはビューから列としてデータを返すために使用するクエリは 2 つの方法のいずれかで指定できます。 調べる手順 2 での列を選択すると、`Products`テーブル。 カスタム SQL ステートメントの使い方を見て s を使用できます。

別の GridView コントロールを追加、`Querying.aspx`ページし、スマート タグのドロップダウン リストから新しいデータ ソースの作成を選択します。 次に、指定するデータからプルされ、データベースをこの新しい SqlDataSource コントロールを作成します。 コントロールに名前を`ProductsWithCategoryInfoDataSource`します。


![ProductsWithCategoryInfoDataSource という名前の新しい SqlDataSource コントロールを作成します。](querying-data-with-the-sqldatasource-control-cs/_static/image18.gif)

**図 12**: という名前の新しい SqlDataSource コントロールの作成 `ProductsWithCategoryInfoDataSource`


次の画面では、データベースを指定するよう求められます。 図 7 で行ったように選択して、`NORTHWINDConnectionString`ドロップダウン リストから一覧表示し、[次へ] をクリックします。 構成ステートメントの選択 画面で、カスタム SQL ステートメントまたはストアド プロシージャのラジオ ボタンの指定を選択し、次へ をクリックします。 これは、SELECT、UPDATE、INSERT、および DELETE というラベルの付いたタブを提供するカスタム ステートメントを定義またはストアド プロシージャの画面が表示されます。 各タブで、テキスト ボックスに、カスタムの SQL ステートメントを入力またはストアド プロシージャをドロップダウン リストから選択できます。 このチュートリアルでは、カスタム SQL ステートメントを入力することで注目するは次のチュートリアルには、ストアド プロシージャを使用する例が含まれています。


![カスタム SQL ステートメントを入力するか、ストアド プロシージャの選択](querying-data-with-the-sqldatasource-control-cs/_static/image19.gif)

**図 13**: カスタム SQL ステートメントを入力するか、ストアド プロシージャの選択


カスタムの SQL ステートメントでは、テキスト ボックスに手動で入力することができますか、クエリ ビルダー ボタンをクリックしてグラフィカルに作成できます。 クエリ ビルダーまたはテキスト ボックスのいずれかから返される次のクエリを使用、`ProductID`と`ProductName`フィールドから、`Products`テーブルを使用して、 `JOIN` s 製品を取得する`CategoryName`から、`Categories`テーブル。


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample4.sql)]


![視覚的に、クエリを使用して作成、クエリ ビルダー](querying-data-with-the-sqldatasource-control-cs/_static/image20.gif)

**図 14**: できますグラフィカルに作成、クエリ ビルダーを使用してクエリ


クエリを指定するには、クエリのテストの画面に進むには、[次へ] をクリックします。 SqlDataSource ウィザードを完了するには、[完了] をクリックします。

ウィザードを完了すると、GridView が追加表示する 3 つの BoundFields、 `ProductID`、 `ProductName`、および`CategoryName`クエリから返され、結果の次の宣言型マークアップ内の列。


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample5.aspx)]


[![GridView は、各製品の ID、名前、および関連付けられているカテゴリの名前を示しています。](querying-data-with-the-sqldatasource-control-cs/_static/image22.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image21.gif)

**図 15**: The GridView 表示各製品 ID、名、および関連付けられているカテゴリの名前 ([フルサイズの画像を表示する をクリックします](querying-data-with-the-sqldatasource-control-cs/_static/image23.gif))。


## <a name="summary"></a>まとめ

このチュートリアルでは、クエリし、SqlDataSource コントロールを使用してデータを表示する方法を説明しました。 など、ObjectDataSource、SqlDataSource は、宣言型のデータへのアクセス方法を提供する、プロキシとして機能します。 そのプロパティを指定、データベースに接続して、SQL`SELECT`を実行するクエリは、プロパティ ウィンドウで、またはデータ ソースの構成ウィザードを使用して指定することができます。

`SELECT`調べるこのチュートリアルではクエリの例がすべてのレコードを指定されたクエリから返さします。 ただし、SqlDataSource コントロールを含めることができます、`WHERE`パラメーター値を持つがプログラムによって割り当てられている、または指定したソースから自動的に取得されたを含む句。 作成し、次のチュートリアルでパラメーター化クエリを使用する方法について説明します。

満足のプログラミングです。

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [リレーショナル データベースのデータにアクセスします。](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [SqlDataSource コントロールの概要](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [ASP.NET クイック スタート チュートリアル: SqlDataSource コントロール](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Web.config`<connectionStrings>`要素](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [データベース接続文字列の参照](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Susan Connery や「社長補佐 Leigh、David Suru でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [次へ](using-parameterized-queries-with-the-sqldatasource-cs.md)
