---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
title: SqlDataSource コントロール (VB) を使用してデータを照会 |Microsoft ドキュメント
author: rick-anderson
description: 前のチュートリアルでは、ObjectDataSource コントロールを使用して、データ アクセス層からプレゼンテーション層を完全に分離します。 この家庭教師としてを開始しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: b12f752d-3502-40a4-b695-fc7b7d08cfd3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 6f886ca85a2a4dea5daeff109370bedc1a3f7265
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876567"
---
<a name="querying-data-with-the-sqldatasource-control-vb"></a>SqlDataSource コントロール (VB) を使用してデータのクエリを実行します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_VB.exe)または[PDF のダウンロード](querying-data-with-the-sqldatasource-control-vb/_static/datatutorial47vb1.pdf)

> 前のチュートリアルでは、ObjectDataSource コントロールを使用して、データ アクセス層からプレゼンテーション層を完全に分離します。 SqlDataSource コントロールを使用して、単純なアプリケーションのプレゼンテーションとデータ アクセスの厳密な分離を必要としない方法を学習お以降このチュートリアルでは、します。


## <a name="introduction"></a>はじめに

すべてのチュートリアルでは、お解析してきた ve プレゼンテーション、ビジネス ロジックとデータ アクセス レイヤーで構成される階層化されたアーキテクチャを使用します。 データ アクセス層 (DAL) を最初のチュートリアルで作成された ([データ アクセス レイヤーを作成する](../introduction/creating-a-data-access-layer-vb.md)) と、ビジネス ロジック層、2 つ目の ([ビジネス ロジック層を作成する](../introduction/creating-a-business-logic-layer-vb.md))。 以降で、 [、ObjectDataSource でデータを表示する](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md)チュートリアルでは、ASP.NET 2.0 s の新しい ObjectDataSource コントロールを使用して、プレゼンテーション層からのアーキテクチャを持つインターフェイスの宣言する方法を説明しました。

データを操作するすべてのチュートリアルはこれまでのアーキテクチャを使用して、アクセス、挿入、更新、およびアーキテクチャをバイパスして、ASP.NET ページから直接データベースのデータを削除することはもです。 これにより、web ページに直接特定のデータベースのクエリやビジネス ロジックを配置します。 十分に大規模または複雑なアプリケーションの場合、設計、実装、および階層化されたアーキテクチャを使用するが、成功した場合、更新、およびアプリケーションの保守容易性のきわめて重要です。 堅牢なアーキテクチャの開発、ただし、必要がありますいない非常に単純な 1 回限りのアプリケーションを作成するときにします。

ASP.NET 2.0 には 5 つの組み込みデータ ソース コントロール[SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx)、 [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx)、 [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx)、 [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx)、および[SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx)です。 アクセスして、Microsoft SQL Server、Microsoft Access、Oracle、MySQL、および他のユーザーを含む、リレーショナル データベースから直接データを変更するは、SqlDataSource を使用できます。 このチュートリアルと、次の 3 つの場合は、SqlDataSource コントロールの使用、挿入、更新、およびデータの削除を照会する方法とフィルター データだけでなくを SqlDataSource を使用する方法を調査する方法について確認します。


![ASP.NET 2.0 には、5 つの組み込みのデータ ソース コントロールが含まれています。](querying-data-with-the-sqldatasource-control-vb/_static/image1.gif)

**図 1**: ASP.NET 2.0 には、5 つの組み込みのデータ ソース コントロールが含まれています。


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>ObjectDataSource と SqlDataSource の比較

概念的には、ObjectDataSource と SqlDataSource コントロールは、データをプロキシするだけです。 説明したように、 [、ObjectDataSource でデータを表示する](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md)チュートリアルでは、ObjectDataSource が挿入、更新、およびデータを削除するデータとを選択するために呼び出すメソッドを提供するオブジェクトの種類を示すプロパティオブジェクトの基になる型。 ObjectDataSource s を使用して、コントロールにデータ GridView、DetailsView、DataList などの Web コントロールをバインドできます、ObjectDataSource プロパティが構成されると、 `Select()`、 `Insert()`、 `Delete()`、および`Update()`メソッド基になるアーキテクチャと対話します。

SqlDataSource は同様の機能を提供しますが、オブジェクト ライブラリではなく、リレーショナル データベースに作用します。 SqlDataSource おは、データベース接続文字列とアドホック SQL クエリを指定する必要があります。 またはストアド プロシージャを挿入、更新、削除、およびデータを取得するために実行できます。 SqlDataSource s `Select()`、 `Insert()`、 `Update()`、および`Delete()`呼び出されると、指定したデータベースへの接続し、適切な SQL クエリを発行します。 次の図に示すように、これらのメソッドは、データベースへの接続、クエリを発行して、結果を返すの grunt 作業を行います。


![SqlDataSource は、データベースへのプロキシとして機能します。](querying-data-with-the-sqldatasource-control-vb/_static/image2.gif)

**図 2**: SqlDataSource は、データベースへのプロキシとして機能


> [!NOTE]
> このチュートリアルでは、データベースからデータを取得中に注目します。 [挿入、更新、および SqlDataSource コントロールにデータを削除しても](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)チュートリアルでは、挿入、更新、および削除をサポートするために SqlDataSource を構成する方法を見てみましょう。


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>SqlDataSource と AccessDataSource コントロール

ASP.NET 2.0 には、SqlDataSource コントロールだけでなく、AccessDataSource コントロールも含まれています。 これら 2 つのコントロールが生じる AccessDataSource コントロールがのみ Microsoft SQL Server と連携するように設計 SqlDataSource コントロールと Microsoft Access で排他的に作られていることが問題ありに ASP.NET 2.0 に新しい多くの開発者。 SqlDataSource コントロールが連動する、AccessDataSource が Microsoft Access を具体的に使用するように設計中に*任意*.NET 経由でアクセスできるリレーショナル データベースです。 これには、ole Db または ODBC の準拠データ ストア、Microsoft SQL Server、Microsoft Access、Oracle、Informix、MySQL、PostgreSQL などその他の多くが含まれます。

唯一の違い、AccessDataSource および SqlDataSource コントロールは、データベースの接続情報を指定する方法です。 AccessDataSource コントロールには、Access データベース ファイルへのファイル パスだけが必要があります。 SqlDataSource には、その一方で、完全な接続文字列が必要です。

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>手順 1: SqlDataSource Web ページの作成

SqlDataSource コントロールを使用してデータベースのデータを直接操作する方法の探索を開始して、前に、まず、このチュートリアルと、次の 3 つの必要がありますを web サイト プロジェクトに、ASP.NET ページを作成するのにはしばらく s を使用できます。 という名前の新しいフォルダーを追加することによって開始`SqlDataSource`です。 次に、各ページに関連付けるように、そのフォルダーに次の ASP.NET ページを追加、`Site.master`マスター ページ。

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![SqlDataSource に関連するチュートリアルについては、ASP.NET ページを追加します。](querying-data-with-the-sqldatasource-control-vb/_static/image3.gif)

**図 3**: SqlDataSource に関連するチュートリアルについては、ASP.NET ページを追加します。


などの他のフォルダーで`Default.aspx`で、`SqlDataSource`フォルダーは、チュートリアルのセクションに一覧表示します。 注意してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 そのため、このユーザー コントロールを追加`Default.aspx`をページのデザイン ビューに、ソリューション エクスプ ローラーからドラッグしています。


[![Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](querying-data-with-the-sqldatasource-control-vb/_static/image5.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image4.gif)

**図 4**: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズのイメージを表示するをクリックして](querying-data-with-the-sqldatasource-control-vb/_static/image6.gif))


最後に、これら 4 つのページを追加するエントリとして、`Web.sitemap`ファイル。 具体的には、DataList とリピータにカスタム ボタンの追加後に、次のマークアップを追加`<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample1.sql)]

更新した後に`Web.sitemap`ブラウザーを使って、チュートリアルの web サイトを表示します。 左側のメニューには、編集、挿入、およびチュートリアルを削除する項目が含まれています。


![サイト マップでは SqlDataSource チュートリアルについては、エントリが含まれるようになりました](querying-data-with-the-sqldatasource-control-vb/_static/image7.gif)

**図 5**: サイト マップでは SqlDataSource チュートリアルについては、エントリが含まれるようになりました


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>手順 2: 追加して、SqlDataSource コントロールを構成します。

開いて開始、 `Querying.aspx`  ページで、`SqlDataSource`フォルダーとデザイン ビューに切り替えます。 SqlDataSource コントロールをドラッグして、ツールボックスからデザイナーとセットにその`ID`に`ProductsDataSource`です。 同様に、ObjectDataSource SqlDataSource レンダリングされた出力を生成しないしたがってデザイン サーフェイス上の灰色のボックスとして表示されます。 SqlDataSource を構成するのには、SqlDataSource s のスマート タグからのデータ ソースの構成のリンクをクリックします。


![をクリックして、SqlDataSource s のスマート タグからのデータ ソースのリンクを構成します。](querying-data-with-the-sqldatasource-control-vb/_static/image8.gif)

**図 6**: をクリックして、SqlDataSource s のスマート タグからのデータ ソースのリンクを構成します。


SqlDataSource コントロールのデータ ソース構成ウィザードが表示されます。 ウィザードの手順は、ObjectDataSource コントロール s と異なる場合、終了な目標を取得、挿入、更新、およびデータ ソースを使用してデータを削除する方法の詳細を提供する、同じです。 SqlDataSource 用には、基になるデータベースを使用するを指定して、アドホック SQL ステートメントまたはストアド プロシージャを提供する必要があります。

ウィザードの最初の手順では、データベースに、私たちを求めます。 ドロップダウン リストには、これらのデータベースについては、web アプリケーションの s が含まれています。`App_Data`フォルダーおよびサーバー エクスプ ローラーでデータ接続 ノードに追加されたものです。 以降お ve は既に追加の接続文字列、`NORTHWIND.MDF`データベースに格納されて、 `App_Data` s プロジェクトにフォルダー`Web.config`ファイル、ドロップダウン リストには、その接続文字列への参照が含まれています。`NORTHWINDConnectionString`です。 ドロップダウン リストからこの項目を選択し、[次へ] をクリックします。


![ドロップダウン リストから、NORTHWINDConnectionString を選択します。](querying-data-with-the-sqldatasource-control-vb/_static/image9.gif)

**図 7**: 選択、`NORTHWINDConnectionString`ドロップダウン リストから


データベースを選択すると、ウィザードはデータを返すクエリを要求します。 列のテーブルまたはビューを返すまたはカスタム SQL ステートメントを入力したり、ストアド プロシージャを指定するかを指定できます。 この選択の指定を行う間を切り替える、カスタム SQL ステートメントまたはストアド プロシージャとテーブルから列を指定したり、オプション ボタンを表示できます。

> [!NOTE]
> この最初の例テーブルまたはビューのオプションからを指定する列を使用して s を使用できます。 このチュートリアルの後半で、ウィザードに戻り、カスタム SQL ステートメントまたはストアド プロシージャ オプションの指定を調査します。


図 8 画面を示しています、構成の Select ステートメントを選択するテーブルまたはビューのオプション ボタンから列を指定します。 ドロップダウン リストには、選択したテーブルまたはビューの列の下のチェック ボックスの一覧に表示されると、Northwind データベースのテーブルおよびビューのセットが含まれています。 この例では、let s を返す、 `ProductID`、 `ProductName`、および`UnitPrice`から列、`Products`テーブル。 図 8 に示す結果の SQL ステートメントが表示されたら、ウィザードでこれらの選択を行う`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`です。


![Products テーブルからデータを返す](querying-data-with-the-sqldatasource-control-vb/_static/image10.gif)

**図 8**: からデータを返す、`Products`テーブル


返す、ウィザードを構成した後、 `ProductID`、 `ProductName`、および`UnitPrice`からの列、`Products`テーブルで、[次へ] ボタンをクリックします。 この最終画面では、前の手順から構成されているクエリの結果を確認する機会を提供します。 構成されたクエリのテスト ボタンをクリックすると実行`SELECT`ステートメントと、結果をグリッドに表示されます。


![SELECT クエリを確認するテスト クエリ ボタンをクリックします。](querying-data-with-the-sqldatasource-control-vb/_static/image11.gif)

**図 9**: レビュー テスト クエリ ボタンをクリックして`SELECT`クエリ


ウィザードを完了するには、[完了] をクリックします。

ObjectDataSource を SqlDataSource のウィザードだけを実行値プロパティに割り当てられる、コントロール s、つまりと同じように、 [ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx)と[ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx)プロパティです。 ウィザードを完了すると、SqlDataSource コントロール s 宣言型マークアップを次のようになります。


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample2.aspx)]

`ConnectionString`プロパティは、データベースに接続する方法に関する情報を提供します。 このプロパティは、完全なハード コーディングされた接続文字列の値を割り当てることができるか、内の接続文字列を指すことができます`Web.config`です。 Web.config で接続文字列の値を参照する構文を使用`<%$ expressionPrefix:expressionValue %>`です。 通常、*されて*は ConnectionStrings および*expressionValue* 、接続文字列での名前を指定、 `Web.config` [ `<connectionStrings>`セクション](https://msdn.microsoft.com/library/bf7sd233.aspx)です。 ただし、構文を使用することができますを参照する`<appSettings>`要素またはリソース ファイルからのコンテンツ。 参照してください[ASP.NET 式の概要](https://msdn.microsoft.com/library/d5bd1tad.aspx)詳細構文を使用します。

`SelectCommand`プロパティは、アドホック SQL ステートメントまたはデータを返すに実行するストアド プロシージャを指定します。

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>手順 3: データ Web コントロールの追加と SqlDataSource へのバインド

SqlDataSource が構成されるは、データ、GridView または DetailsView などの Web コントロールにバインドできます。 このチュートリアルでは、s、GridView にデータを表示を使用できます。 ツールボックスから、ページにドラッグ、GridView とにバインド、 `ProductsDataSource` SqlDataSource GridView s のスマート タグのドロップ ダウン リストから、データ ソースを選択しています。


[![GridView を追加し、SqlDataSource コントロールにバインド](querying-data-with-the-sqldatasource-control-vb/_static/image13.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image12.gif)

**図 10**: GridView を追加し、SqlDataSource コントロールにバインド ([フルサイズのイメージを表示するをクリックして](querying-data-with-the-sqldatasource-control-vb/_static/image14.gif))


GridView s のスマート タグのドロップダウン リストにしているが SqlDataSource コントロールを選択すると、Visual Studio が自動的に追加 BoundField または CheckBoxField GridView にデータ ソース コントロールによって返される列の各します。 SqlDataSource が 3 つのデータベースの列を返すので`ProductID`、 `ProductName`、および`UnitPrice`GridView に 3 つのフィールドがあります。

GridView の 3 を構成するのにはしばらく時間かかる BoundFields です。 変更、`ProductName`フィールド s`HeaderText`プロパティを製品名、`UnitPrice`フィールドの価格にします。 書式設定も、`UnitPrice`通貨としてフィールドです。 このような変更を加えたら、GridView s の宣言型マークアップを次のようになります。


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample3.aspx)]

ブラウザーからこのページを参照してください。 GridView に秒の各製品が一覧表示図 11 に示す`ProductID`、 `ProductName`、および`UnitPrice`値。


[![GridView は、各製品の ProductID、ProductName、および UnitPrice の値が表示されます。](querying-data-with-the-sqldatasource-control-vb/_static/image16.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image15.gif)

**図 11**:「GridView 表示各製品の s `ProductID`、 `ProductName`、および`UnitPrice`値 ([フルサイズ イメージを表示するに、をクリックして](querying-data-with-the-sqldatasource-control-vb/_static/image17.gif))


GridView がそのデータ ソース コントロール s を呼び出すページにアクセスするときに`Select()`メソッドです。 ObjectDataSource コントロールを使用していた私たちは、ときにこれを呼び出す、`ProductsBLL`クラスの`GetProducts()`メソッドです。 ただし、SqlDataSource による、`Select()`メソッドが、指定されたデータベースと問題への接続を確立、 `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`、この例では)。 SqlDataSource では、GridView しを列挙する、返されるデータベースのレコードを GridView の行を作成する結果を返します。

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>組み込みのデータの Web コントロールの機能と、SqlDataSource コントロール

一般に、Web コントロールのページング、並べ替え、編集、データに固有の機能データ Web コントロールに固有の削除、挿入、およびなどとは、データ ソース コントロールの使用に依存しません。 つまり、GridView は、ページング、並べ替え、編集、および、ObjectDataSource または、SqlDataSource にバインドされているかどうかを削除すると、その組み込みを利用できます。 ただし、Web コントロールの機能は、特定のデータは、データ ソース コントロールの構成または使用されているデータ ソース コントロールに機密性の高いです。

たとえば、[効率的にから大規模な量のデータのページング](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md)、既定では、Web データのページングのロジックを制御する方法単純返しますに説明したチュートリアル*すべて*から基になるレコードデータ ソースし、データの現在のページ インデックスと 1 ページに表示するレコードの数を指定されたレコードの適切なサブセットのみが表示されます。 十分に大きな結果セットをページングするとき、このモデルは非常に効率的ではありません。 さいわい、表示するレコードの正確なサブセットのみを返す、カスタム ページングをサポートするために、ObjectDataSource を構成できます。 ただし、SqlDataSource コントロールには、カスタム ページングを実装するためのプロパティが不足しています。

ページングや並べ替えを他の注意は、SqlDataSource で発生します。 既定では、SqlDataSource から返されたデータ ページしたり GridView で並べ替えられます。 これを示すためには、GridView s スマート タグでページングを有効にして、並べ替えを有効にするオプションを確認して`Querying.aspx`これは予想どおりことを確認してください。

並べ替えとページングは、SqlDataSource 疎に型指定されたデータセットに、データベースのデータを取得するためには機能します。 クエリによって返される、重要な側面にページングを実装するレコードの合計数は、データセットから確認できます。 さらに、データセットの結果は、DataView を並べ替えることができます。 これらの機能は、GridView 要求ページまたはデータを並べ替えるときに自動的に SqlDataSource によって使用されます。

変更することによって、データセットではなく DataReader を返す、SqlDataSource を構成することができます、 [ `DataSourceMode`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx)から`DataSet`(既定) に`DataReader`です。 SqlDataSource の結果を DataReader を期待している既存のコードに渡すときの状況で DataReader を使用してを優先可能性があります。 さらに、Datareader がデータセットよりもはるかに単純なオブジェクトであるため、優れたパフォーマンスを提供します。 この変更を行うと、ただし、データ Web コントロールを並べ替えることができますもでない場合は、SqlDataSource、クエリによって返されるレコードの数を確認することはできませんまたは DataReader からのページが返されるデータを並べ替えるためのすべての手法を提供します。

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>手順 4: カスタム SQL ステートメントを使用してストアド プロシージャまたは

SqlDataSource コントロールを構成するときに、カスタム SQL ステートメントまたはストアド プロシージャ、または既存のテーブルまたはビューから列としてデータを返すために使用するクエリは 2 つの方法のいずれかで指定できます。 手順 2. を調べることでから列を選択、`Products`テーブル。 カスタム SQL ステートメントを使用して見て s を使用できます。

別の GridView コントロールの追加、`Querying.aspx`ページし、スマート タグのドロップダウン リストから新しいデータ ソースを作成します。 次に、指定するデータ プルされ、データベースからこの新しい SqlDataSource コントロールを作成します。 コントロールの名前を付けます`ProductsWithCategoryInfoDataSource`です。


![ProductsWithCategoryInfoDataSource をという名前の新しい SqlDataSource コントロールを作成します。](querying-data-with-the-sqldatasource-control-vb/_static/image18.gif)

**図 12**: という名前の新しい SqlDataSource コントロールを作成します。 `ProductsWithCategoryInfoDataSource`


次の画面を使用して、データベースを指定するよう求められます。 図 7 に戻ると、選択、`NORTHWINDConnectionString`ドロップダウン リストからを一覧表示し、[次へ] をクリックします。 ステートメントの選択 画面の構成 では、カスタム SQL ステートメントまたはストアド プロシージャのオプション ボタンを指定してくださいを選択し、次へ をクリックします。 SELECT、UPDATE、INSERT、および DELETE というラベルの付いたタブを提供する、定義のカスタム ステートメントまたはストアド プロシージャ画面が表示されます。 各タブで、テキスト ボックスに、カスタムの SQL ステートメントを入力したり、ストアド プロシージャをドロップダウン リストから選択できます。 このチュートリアルで見ていきます。 カスタム SQL ステートメントを入力します。次のチュートリアルには、ストアド プロシージャを使用する例が含まれています。


![カスタム SQL ステートメントを入力するか、ストアド プロシージャを選択](querying-data-with-the-sqldatasource-control-vb/_static/image19.gif)

**図 13**: カスタム SQL ステートメントを入力するか、ストアド プロシージャを選択


カスタムの SQL ステートメントは、textbox に手動で入力できますか、クエリ ビルダー ボタンをクリックしてグラフィカルに作成できます。 クエリ ビルダーは、またはテキスト ボックスで、次のクエリを使用して返す、`ProductID`と`ProductName`からのフィールド、`Products`テーブルを使用して、 `JOIN` s 製品を取得する`CategoryName`から、`Categories`テーブル。


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample4.sql)]


![視覚的に、クエリを使用して作成、クエリ ビルダー](querying-data-with-the-sqldatasource-control-vb/_static/image20.gif)

**図 14**: できます視覚的に構築、クエリ ビルダーを使用して、クエリ


クエリを指定すると、クエリのテストの画面に進むには、[次へ] をクリックします。 SqlDataSource ウィザードを完了するには、[完了] をクリックします。

ウィザードを完了すると、GridView が追加表示する次の 3 つの BoundFields、 `ProductID`、 `ProductName`、および`CategoryName`クエリから返され、結果の次の宣言型マークアップ内の列。


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample5.aspx)]


[![GridView は、各製品の ID、名前と、関連付けられているカテゴリの名前を示しています。](querying-data-with-the-sqldatasource-control-vb/_static/image22.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image21.gif)

**図 15**:「GridView 示します各製品の s ID、名、および関連付けられているカテゴリの名前 ([フルサイズ イメージを表示するに、をクリックして](querying-data-with-the-sqldatasource-control-vb/_static/image23.gif))


## <a name="summary"></a>まとめ

このチュートリアルでは、クエリおよび SqlDataSource コントロールを使用してデータを表示する方法を説明しました。 ObjectDataSource と同様に、SqlDataSource は宣言型のデータへのアクセス方法を提供する、プロキシとして機能します。 そのプロパティを指定データベースに接続して、SQL`SELECT`を実行するクエリには、[プロパティ] ウィンドウやデータ ソースの構成ウィザードを使用して、指定することができます。

`SELECT`クエリ例のこのチュートリアルで調べることが、指定されたクエリから返さのすべてのレコードです。 ただし、SqlDataSource コントロールを含めることができます、`WHERE`パラメーターの値を持つプログラムによって割り当てられたまたは自動的に指定されたソースからプルされなくを含む句。 作成して、次のチュートリアルではパラメーター化クエリを使用する方法について確認します。

満足プログラミング!

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [リレーショナル データベースのデータにアクセスします。](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [SqlDataSource コントロールの概要](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [ASP.NET のクイック スタート チュートリアル: SqlDataSource コントロール](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Web.config`<connectionStrings>`要素](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [データベース接続文字列の参照](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Susan Connery、「社長補佐 Leigh および David Suru でした。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
> [次へ](using-parameterized-queries-with-the-sqldatasource-vb.md)
