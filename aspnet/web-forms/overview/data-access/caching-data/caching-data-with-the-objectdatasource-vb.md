---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
title: "ObjectDataSource (VB) を使用してデータをキャッシュ |Microsoft ドキュメント"
author: rick-anderson
description: "キャッシュすると、低速と高速な Web アプリケーションの違いを意味します。 このチュートリアルでは、ASP.NET でのキャッシュを詳細に見ている 4 つの最初の."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 2e56a733-5512-48a6-9276-70a65bbe4d5d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: ce0daabf8d68614c530115cc37b4f088f75dba4d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="caching-data-with-the-objectdatasource-vb"></a>ObjectDataSource (VB) を使用してデータをキャッシュ
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_VB.exe)または[PDF のダウンロード](caching-data-with-the-objectdatasource-vb/_static/datatutorial58vb1.pdf)

> キャッシュすると、低速と高速な Web アプリケーションの違いを意味します。 このチュートリアルは、ASP.NET でのキャッシュを詳細に見ている 4 つの最初。 キャッシュの主な概念と、ObjectDataSource コントロールを介してプレゼンテーション層にキャッシュを適用する方法について説明します。


## <a name="introduction"></a>はじめに

コンピューター サイエンスの世界で*キャッシュ*プロセス データまたは取得するとコストが情報を取得し、アクセスする方が手軽な場所に、そのコピーを格納します。 データ ドリブンのアプリケーションでは、大規模で複雑なクエリは一般的なアプリケーションの実行時間の大部分を消費します。 このようなアプリケーション s のパフォーマンスを次は、アプリケーション メモリに負荷の高いデータベース クエリの結果を格納することにより多くの場合、向上できます。

ASP.NET 2.0 には、さまざまなキャッシュ オプションが用意されています。 Web ページ全体またはユーザー コントロールのレンダリング s マークアップをキャッシュできる*出力キャッシュ*です。 ObjectDataSource および SqlDataSource コントロールは、コントロール レベルのキャッシュに保存する同様、これにより、データを許可するキャッシュの機能を提供します。 ASP.NET s*データ キャッシュ*をプログラムでオブジェクトのキャッシュにページの開発者を有効にする機能豊富なキャッシュ API を提供します。 このチュートリアルでについて見ていきましょう ObjectDataSource s を使用して、[次へ] の 3 つの機能だけでなく、データ キャッシュをキャッシュします。 またの起動時にアプリケーション全体のデータをキャッシュする方法とキャッシュされたデータを SQL キャッシュ依存関係を使用して最新の状態に保つ方法を調べておみます。 これらのチュートリアルでは、出力キャッシュは表示されません。 出力キャッシュの詳細について、次を参照してください。 [ASP.NET 2.0 で出力キャッシュ](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)です。

キャッシュで適用できますアーキテクチャでは、任意の場所でデータ アクセス層からのプレゼンテーション層です。 このチュートリアルでは、ObjectDataSource コントロールを介してプレゼンテーション層にキャッシュを適用することについて見ていきます。 次のチュートリアルについて見ていきましょうでビジネス ロジック層でデータをキャッシュします。

## <a name="key-caching-concepts"></a>キーのキャッシュの概念

アプリケーション s を大幅に改善できるキャッシュ全体のパフォーマンスとスケーラビリティを生成するとコストがデータを取得しより効率的にアクセスできる場所に、そのコピーを格納しています。 キャッシュは、実際、基になるデータのコピーだけを保持しているため、期限切れなるまたは*古い*、基になるデータが変更された場合。 この対処するため、ページの開発者がキャッシュ項目がある、条件を示すことができます*削除*キャッシュからのいずれかの使用します。

- **時間を基準**絶対またはスライド期間のキャッシュにアイテムを追加することがあります。 たとえば、ページの開発者は、たとえば、60 秒間の期間を可能性があります。 絶対期間キャッシュされたアイテムは 60 秒にアクセスする頻度に関係なく、キャッシュに追加された後に削除されます。 スライド式の継続時間、キャッシュされた項目は最終アクセス後に 60 秒間に削除されます。
- **依存関係に基づく条件**依存関係は、キャッシュに追加されたときに項目を関連付けることができます。 項目 %s の依存関係が変更されたときに、キャッシュから削除されます。 依存関係には、ファイル、別のキャッシュ項目、または 2 つの組み合わせがあります。 ASP.NET 2.0 では、SQL キャッシュ依存関係、開発者は、キャッシュに項目を追加して、それを基になるデータベースのデータが変更されたときに削除を有効にすることもできます。 今後の SQL キャッシュ依存関係を見ていきます[を使用して SQL キャッシュ依存関係](using-sql-cache-dependencies-vb.md)チュートリアルです。

指定された削除条件に関係なく、キャッシュ内の項目があります*清掃*時間ベースまたは依存関係に基づく条件が満たされている前にします。 キャッシュは、その容量に達すると、新しいものを追加する前に、既存の項目を削除してください。 そのため、操作するときにプログラムでキャッシュされたデータが s を常に想定する、重要なキャッシュされたデータいない可能性があります。 チュートリアルでは、[次へ]、プログラムでキャッシュからデータにアクセスするときに使用するパターンに紹介*アーキテクチャでは、データをキャッシュ*です。

キャッシュ アプリケーションからより高いパフォーマンスをつかんでの経済的な手段を提供します。 として[Steven Smith](http://aspadvice.com/blogs/ssmith/)の記事を明確に示し[ASP.NET キャッシュ: 技術とベスト プラクティス](https://msdn.microsoft.com/library/aa478965.aspx):

キャッシュとできます上達する十分なパフォーマンス、多くの時間と分析が必要ないことをお勧めします。 メモリが低い、支出、1 日または週に、コードまたはデータベースの最適化を試みるのではなく 30 秒間に出力をキャッシュして、必要なパフォーマンスを得ることができる場合のキャッシュ ソリューションを操作するため (30 秒間のデータと仮定した場合に問題はありません) 移動します。 最終的には、品質が低いデザインはおそらく遅延を解消するには、コースのアプリケーションを正しく設計を行う必要がありますのでです。 適切な十分なパフォーマンスを向上を取得する場合は、キャッシュできる、優れた [方法] で、後でそのためには時間がある場合、アプリケーションをリファクターする時間を購入します。

キャッシュと、ほどのパフォーマンスの強化が提供することができます、にない場合は、該当するなど、頻繁に更新のリアルタイムのデータを使用した、またはもすぐに及ぶ古いデータが許容可能なアプリケーションとします。 ほとんどのアプリケーションは、キャッシュに使用してください。 ASP.NET 2.0 でのキャッシュの詳細についてを参照してください、[パフォーマンスに対するキャッシュ](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx)のセクションで、 [ASP.NET 2.0 のクイック スタート チュートリアル](https://quickstarts.asp.net/QuickStartv20/aspnet/)です。

## <a name="step-1-creating-the-caching-web-pages"></a>手順 1: Web ページのキャッシュを作成します。

ObjectDataSource s キャッシュ機能を探索を始める前に、まず、このチュートリアルと、次の 3 つの必要がありますを web サイト プロジェクトに、ASP.NET ページを作成するのにはしばらく s を使用できます。 という名前の新しいフォルダーを追加することによって開始`Caching`です。 次に、各ページに関連付けるように、そのフォルダーに次の ASP.NET ページを追加、`Site.master`マスター ページ。

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![キャッシュに関連するチュートリアルについては、ASP.NET ページを追加します。](caching-data-with-the-objectdatasource-vb/_static/image1.png)

**図 1**: キャッシュに関連するチュートリアルについては、ASP.NET ページを追加します。


などの他のフォルダーで`Default.aspx`で、`Caching`フォルダーは、チュートリアルのセクションに一覧表示します。 注意してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 そのため、このユーザー コントロールを追加`Default.aspx`をページのデザイン ビューに、ソリューション エクスプ ローラーからドラッグしています。


[![図 2: Default.aspx に SectionLevelTutorialListing.ascx ユーザー コントロールを追加します。](caching-data-with-the-objectdatasource-vb/_static/image3.png)](caching-data-with-the-objectdatasource-vb/_static/image2.png)

**図 2**: 図 2: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズのイメージを表示するをクリックして](caching-data-with-the-objectdatasource-vb/_static/image4.png))


最後に、これらのページを追加するエントリとして、`Web.sitemap`ファイル。 具体的には、バイナリ データを操作した後、次のマークアップを追加`<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-vb/samples/sample1.xml)]

更新した後に`Web.sitemap`ブラウザーを使って、チュートリアルの web サイトを表示します。 左側のメニューには、キャッシュのチュートリアルについては、項目が含まれています。


![サイト マップではチュートリアルについては、キャッシュ エントリが含まれるようになりました](caching-data-with-the-objectdatasource-vb/_static/image5.png)

**図 3**: サイト マップではチュートリアルについては、キャッシュ エントリが含まれるようになりました


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>手順 2: Web ページで製品の一覧を表示します。

このチュートリアルでは、ObjectDataSource コントロール s 組み込みキャッシュ機能を使用する方法について説明します。 これらの機能を見ることができますが、お客様最初必要ページから作業です。 Let s は、製品情報を一覧から、ObjectDataSource によって取得された GridView を使用する web ページを作成、`ProductsBLL`クラスです。

開いて開始、 `ObjectDataSource.aspx`  ページで、`Caching`フォルダーです。 GridView をツールボックスからデザイナーにドラッグして、設定、`ID`プロパティを`Products`、し、そのスマート タグからという名前の新しい ObjectDataSource コントロールにバインドする選択`ProductsDataSource`です。 構成を使用する ObjectDataSource、`ProductsBLL`クラスです。


[![構成、ObjectDataSource ProductsBLL クラスを使用するには](caching-data-with-the-objectdatasource-vb/_static/image7.png)](caching-data-with-the-objectdatasource-vb/_static/image6.png)

**図 4**: 構成を使用する ObjectDataSource、`ProductsBLL`クラス ([フルサイズのイメージを表示するをクリックして](caching-data-with-the-objectdatasource-vb/_static/image8.png))


このページで、s、ObjectDataSource でキャッシュされたデータが GridView の s インターフェイスから変更されたときの動作を検証できるように編集可能な GridView の作成を使用できます。 既定の設定に、選択 タブで、ドロップダウン リストのままに`GetProducts()`が、更新プログラム タブを選択した項目を変更、`UpdateProduct`を受け入れるオーバー ロード`productName`、 `unitPrice`、および`productID`入力パラメーターとして。


[![適切な UpdateProduct オーバー ロードに更新 タブのドロップダウン一覧を設定します。](caching-data-with-the-objectdatasource-vb/_static/image10.png)](caching-data-with-the-objectdatasource-vb/_static/image9.png)

**図 5**: 更新 タブのドロップダウン リストを適切に設定`UpdateProduct`オーバー ロード ([フルサイズのイメージを表示するをクリックして](caching-data-with-the-objectdatasource-vb/_static/image11.png))


最後に、(なし) を挿入および削除のタブで、ドロップダウン リストを設定し、[完了] をクリックします。 データ ソース構成ウィザードを完了すると、Visual Studio 設定 ObjectDataSource s`OldValuesParameterFormatString`プロパティを`original_{0}`です。 説明したように、 [、概要の挿入、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)チュートリアルでは、このプロパティは宣言構文から削除されたり、セットの既定値に戻す必要があります`{0}`、更新したワークフローの順序でエラーなしで続行します。

さらに、ウィザードの完了時に Visual Studio にフィールドを追加、GridView の各製品のデータ フィールドのです。 削除が、 `ProductName`、 `CategoryName`、および`UnitPrice`BoundFields です。 次に、更新、`HeaderText`各製品カテゴリ、および価格、これらの BoundFields のプロパティにそれぞれします。 以降、`ProductName`フィールドは必須、BoundField を TemplateField に変換し、RequiredFieldValidator を追加、`EditItemTemplate`です。 同様に、変換、`UnitPrice`を TemplateField BoundField し、ユーザーが入力した値が有効な通貨の値より大きいまたは 0 に等しい s であることを確認する CompareValidator を追加します。 これらの変更だけでなく自由に右揃えなど、任意の見た目変更を実行、`UnitPrice`値、または書式設定を指定する、`UnitPrice`の読み取り専用と編集インターフェイス内のテキスト。

GridView は、GridView s のスマート タグの編集を有効にするチェック ボックスをオンして編集可能です。 またのページングを有効にして、並べ替えを有効にするチェック ボックスを確認します。

> [!NOTE]
> GridView s 編集インターフェイスをカスタマイズする方法の確認が必要ですか。 場合を参照、[データ変更インターフェイスのカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)チュートリアルです。


[![編集、並べ替え、およびページングの GridView のサポートを有効にします。](caching-data-with-the-objectdatasource-vb/_static/image13.png)](caching-data-with-the-objectdatasource-vb/_static/image12.png)

**図 6**: 編集、並べ替え、およびページングの GridView のサポートを有効にする ([フルサイズのイメージを表示するをクリックして](caching-data-with-the-objectdatasource-vb/_static/image14.png))


GridView のこれらの変更を加えたら、GridView と ObjectDataSource s の宣言型マークアップを次のようになります。


[!code-aspx[Main](caching-data-with-the-objectdatasource-vb/samples/sample2.aspx)]

図 7 に示す、編集可能な GridView は、名前、カテゴリ、および各データベースでは、製品の価格の一覧を示します。 ページの機能並べ替えをテスト結果をそれらをページにはしばらく時間かかるし、レコードを編集します。


[![各製品名、カテゴリ、および Price として記載されて Sortable、Pageable、編集可能な GridView](caching-data-with-the-objectdatasource-vb/_static/image16.png)](caching-data-with-the-objectdatasource-vb/_static/image15.png)

**図 7**: 各製品名、カテゴリ、および Price として記載されて Sortable、Pageable、編集可能な GridView ([フルサイズのイメージを表示するをクリックして](caching-data-with-the-objectdatasource-vb/_static/image17.png))


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>データを要求するときに、ObjectDataSource を調べることは、手順 3 です。

`Products` GridView を呼び出すことによって表示するデータの取得、`Select`のメソッド、 `ProductsDataSource` ObjectDataSource です。 この ObjectDataSource ビジネス ロジック層 s のインスタンスを作成する`ProductsBLL`クラスと呼び出しの`GetProducts()`メソッドで、さらに、データ アクセス層 s `ProductsTableAdapter` s`GetProducts()`メソッドです。 DAL メソッドは、Northwind データベースに接続し、構成された発行`SELECT`クエリ。 このデータは、DAL でのパッケージに返され、`NorthwindDataTable`です。 DataTable オブジェクトには、これを GridView に返すと、ObjectDataSource に戻る BLL が返されます。 GridView は、作成、`GridViewRow`ごとにオブジェクト`DataRow`DataTable、および各`GridViewRow`がクライアントに返され、s の訪問者のブラウザーで表示される HTML に表示されるかは最終的にします。

このイベントのシーケンスは、GridView は、その基になるデータにバインドする必要があるたびに発生します。 ページが最初にアクセスすると、GridView の並べ替えの場合、または編集、またはインターフェイスを削除すると、その組み込みを GridView のデータを変更したとき、別のデータの 1 つのページから移動するときに発生します。 GridView のビュー ステートが無効になっている場合、GridView がも、すべてのポストバック時に再バインドされます。 GridView も明示的に再バインドできるをそのデータを呼び出してその`DataBind()`メソッドです。

データベースからデータを取得する頻度を完全に理解するには、秒を示すデータが再取得されているときにメッセージを表示することができます。 という名前の GridView 上のラベルの Web コントロールを追加`ODSEvents`です。 クリア、`Text`プロパティと設定、`EnableViewState`プロパティを`False`です。 ラベルの下にあるボタン Web コントロールを追加し、設定、`Text`プロパティをポストバックします。


[![GridView 上のページにラベルとボタンを追加します。](caching-data-with-the-objectdatasource-vb/_static/image19.png)](caching-data-with-the-objectdatasource-vb/_static/image18.png)

**図 8**: ページ上の GridView にラベルとボタンを追加 ([フルサイズのイメージを表示するをクリックして](caching-data-with-the-objectdatasource-vb/_static/image20.png))


データ アクセスのワークフロー、ObjectDataSource 秒の間に`Selecting`基になるオブジェクトが作成される前に、イベントの起動とその構成されているメソッドが呼び出されます。 このイベントのイベント ハンドラーを作成し、次のコードを追加します。


[!code-vb[Main](caching-data-with-the-objectdatasource-vb/samples/sample3.vb)]

たびに、ObjectDataSource が要求を送信、アーキテクチャ、データのラベルに発生したテキストを選択するとイベントが表示されます。

ブラウザーでこのページを参照してください。 ページが初めてアクセスしたときに発生したテキストを選択するとイベントが表示されます。 ポストバックのボタンをクリックし、テキストが表示されないことに注意してください (想定される GridView s`EnableViewState`プロパティに設定されている`True`、既定値)。 これは、ビューステートからポストバックで GridView を再構築されない電源に対するデータの ObjectDataSource をそのため、です。 並べ替え、ページング、またはデータの編集は、ただし、により、データ ソースの再バインドする GridView と、そのためを選択するとイベントのテキストが再表示されます。


[![GridView がそのデータ ソースを再バインドされるたびに発生するイベントが表示されます。](caching-data-with-the-objectdatasource-vb/_static/image22.png)](caching-data-with-the-objectdatasource-vb/_static/image21.png)

**図 9**: されるたびに GridView がそのデータ ソースにバインドし直すと、発生するイベントが表示されます ([フルサイズのイメージを表示するをクリックして](caching-data-with-the-objectdatasource-vb/_static/image23.png))


[![クリックするとポストバック ボタンで、ビュー状態から再構築する GridView](caching-data-with-the-objectdatasource-vb/_static/image25.png)](caching-data-with-the-objectdatasource-vb/_static/image24.png)

**図 10**: からそのビュー ステートを再構築する GridView ポストバックのボタンをクリックすると、([フルサイズのイメージを表示するをクリックして](caching-data-with-the-objectdatasource-vb/_static/image26.png))


データがを介してページングや並べ替えするたびに、データベースのデータを取得する無駄なないように見える場合があります。 結局のところ、既定のページングを使用していること、以降、ObjectDataSource が取得、すべてのレコード最初のページを表示するときにします。 GridView で並べ替えとページングのサポートが提供されない場合でも、データから取得する必要データベースごとに、ページが最初にアクセスされる任意のユーザー (とポストバックごと、ビュー ステートが無効になっている場合)。 これらのデータベースを追加要求は余分な GridView には、すべてのユーザーに、同じデータが表示されている場合。 返される結果をキャッシュしない理由、`GetProducts()`キャッシュされた結果をメソッドおよび GridView にバインドしますか?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>手順 4: ObjectDataSource を使用してデータをキャッシュします。

いくつかのプロパティを設定するだけで自動的に ASP.NET データ キャッシュで取得したデータをキャッシュする、ObjectDataSource を構成できます。 次の一覧には、ObjectDataSource のキャッシュに関連するプロパティをまとめたものです。

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx)に設定する必要があります`True`キャッシュを有効にします。 既定値は、`False` です。
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx)時間 (秒)、データがキャッシュされている量。 既定値は 0 です。 場合、ObjectDataSource からデータをキャッシュのみ`EnableCaching`は`True`と`CacheDuration`0 より大きい値に設定されます。
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx)に設定することができます`Absolute`または`Sliding`です。 場合`Absolute`、その取得のデータをキャッシュして、ObjectDataSource`CacheDuration`場合; 秒間`Sliding`、データでは、それがアクセスされていないの後にのみ有効期限が切れる`CacheDuration`(秒)。 既定値は、`Absolute` です。
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx)このプロパティを使用して既存のキャッシュの依存関係に ObjectDataSource のキャッシュ エントリを関連付けます。 ObjectDataSource のデータ エントリ処理の途中で削除できるキャッシュから期限切れにして、関連する`CacheKeyDependency`です。 このプロパティは、SQL キャッシュ依存関係を関連付ける ObjectDataSource のキャッシュに最もよく使用、トピックについては、将来[を使用して SQL キャッシュ依存関係](using-sql-cache-dependencies-vb.md)チュートリアルです。

S を構成できるように、 `ProductsDataSource` ObjectDataSource を絶対スケールで 30 秒間のデータをキャッシュします。 集合 ObjectDataSource s`EnableCaching`プロパティを`True`とその`CacheDuration`30 に設定するプロパティです。 ままにして、`CacheExpirationPolicy`プロパティの既定の設定に`Absolute`です。


[![30 秒間のデータをキャッシュする ObjectDataSource を構成します。](caching-data-with-the-objectdatasource-vb/_static/image28.png)](caching-data-with-the-objectdatasource-vb/_static/image27.png)

**図 11**: 構成を 30 秒間のデータをキャッシュする ObjectDataSource ([フルサイズのイメージを表示するをクリックして](caching-data-with-the-objectdatasource-vb/_static/image29.png))


変更を保存し、ブラウザーのこのページを再表示します。 選択すると発生するイベントのテキストは最初にデータがキャッシュにページを開いたときにも表示されます。 以降のポストバックがトリガーをクリックして、ポストバック、並べ替え、ページング、または編集 または キャンセル ボタンをクリックするが、*いない*発生したテキストを再表示するイベントです。 これは、ため、`Selecting`イベントは、ObjectDataSource; 基になるオブジェクトからのデータを取得するときにのみ発生、`Selecting`イベントでは、データがデータ キャッシュから取得した場合は発生しません。

30 秒後、データをキャッシュから削除されます。 場合、データをキャッシュから削除も、ObjectDataSource s `Insert`、 `Update`、または`Delete`メソッドが呼び出されます。 その結果、または後に 30 秒が経過したか、並べ替え、ページングは、更新 ボタンがクリックしてされた編集 または キャンセル ボタンをクリックすると、その基になるオブジェクトからのデータを取得する ObjectDataSource、テキストに発生するイベントを表示するときに、`Selecting`イベントが発生します。 返される結果は、データ キャッシュに配置されます。

> [!NOTE]
> 選択すると発生するイベントのテキストを頻繁に表示する場合は、キャッシュされたデータで作業を ObjectDataSource が予想される場合もありますメモリ制約によって。 十分な空きメモリ容量がない場合、ObjectDataSource によってキャッシュに追加されたデータを清掃が可能性があります。 ObjectDataSource されないがのみキャッシュまたはデータのキャッシュで正しく表示されない場合はデータ、断続的アプリケーション メモリの空き容量をいくつか閉じてもう一度やり直してください。


図 12 は、ワークフローのキャッシュ、ObjectDataSource s を示しています。 選択するとイベントが発生したときにテキストが表示されたら、データ キャッシュではなかったにあり、基になるオブジェクトから取得するためです。 このテキストが存在しない場合、ただし、その s データがキャッシュから使用可能なためです。 データがキャッシュから返される場合があります s 基になるオブジェクトへの呼び出しと、そのため、ありませんデータベース クエリは実行されません。


![ObjectDataSource ストアとデータ キャッシュからそのデータを取得します](caching-data-with-the-objectdatasource-vb/_static/image30.png)

**図 12**: ObjectDataSource を格納し、データ キャッシュからのデータの取得


各 ASP.NET アプリケーションでは、インスタンスのすべてのページと訪問者の間で共有 s 独自のデータ キャッシュがあります。 つまりは、ObjectDataSource によって、データ キャッシュに格納されたデータ同様に、ページにアクセスしているすべてのユーザー間で共有されます。 これを確認するには、開く、`ObjectDataSource.aspx`ブラウザーのページです。 最初にアクセスすると、ページを選択すると発生するイベントのテキストが表示されます (以前のテストによって、キャッシュに追加されたデータが、ここでは、によって削除された) する場合。 2 番目のブラウザー インスタンスおよびコピーを開き、最初のブラウザー インスタンスから 2 番目の URL を貼り付けます。 2 番目のブラウザー インスタンスを選択すると発生するイベントのテキストが表示されていないためを使用して、同じキャッシュ データを 1 つ目として。

ObjectDataSource を含むキャッシュ キー値を使用して挿入するとき、取得したデータをキャッシュに、:`CacheDuration`と`CacheExpirationPolicy`プロパティの値です指定されていると、ObjectDataSource によって使用されている、基になるビジネス オブジェクトの型。使用して、 [ `TypeName`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx)(`ProductsBLL`、この例では); の値、`SelectMethod`プロパティ名とパラメーターの値、`SelectParameters`コレクションとその値`StartRowIndex`と`MaximumRows`を実装する場合に使用されるプロパティ[カスタム ページング](../paging-and-sorting/paging-and-sorting-report-data-vb.md)です。

これらの値を変更すると、一意のキャッシュ エントリのことを確認、これらのプロパティの組み合わせとして、キャッシュ キーの値を作成します。 たとえば、過去のチュートリアルでは ve を使用して調べる、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`、指定したカテゴリのすべての製品が返されます。 1 人のユーザーがページおよび表示の飲み物をれることもありますが、`CategoryID`は 1 です。 その結果に関係なくが、ObjectDataSource にキャッシュされている場合、`SelectParameters`値は、別のユーザーがページに付属していたときにキャッシュ内にあった飲み物の製品の中に調味料を表示する、調味料ではなく、キャッシュされた飲料、d を参照してください。 値を含むこれらのプロパティで、キャッシュ キーを変更することで、 `SelectParameters`、ObjectDataSource が beverages と調味料の個別のキャッシュ エントリを保持します。

## <a name="stale-data-concerns"></a>古いデータに関する注意事項

ObjectDataSource ときにいずれかのキャッシュから項目を自動的に削除しますその`Insert`、 `Update`、または`Delete`メソッドが呼び出されます。 これにより、ページで、データが変更されたときに、キャッシュ エントリをクリアする古いデータを保護できます。 ただし、可能であれば、ObjectDataSource キャッシュを使用した古いデータが表示されたままにします。 最も簡単な場合は、データがデータベース内で直接変更するためがあります。 おそらく、データベース管理者は、データベース内のレコードの一部を変更するスクリプトを実行するだけです。

このシナリオは些細な方法で展開も可能性があります。 ObjectDataSource は、データ変更メソッドのいずれかが呼び出されたときに、キャッシュからその項目を削除、キャッシュされた項目が削除されたが、ObjectDataSource s 特定の組み合わせのプロパティの値 (`CacheDuration`、 `TypeName`、 `SelectMethod`、同様に続きます)。 さまざまなを使用する 2 つの ObjectDataSources があれば`SelectMethods`または`SelectParameters`、まだことができます、同じデータを更新し、1 つ ObjectDataSource 可能性があります行を更新する 2 番目の ObjectDataSource の独自のキャッシュ エントリが、対応する行を無効には提供されますが、キャッシュされたからです。 ぜひ、この機能が発生するページを作成します。 キャッシュを使用してデータを取得するように構成する ObjectDataSource からそのデータをプルする編集可能な GridView を表示するページを作成、`ProductsBLL`クラスの`GetProducts()`メソッドです。 もう 1 つ追加編集可能な GridView とこのページ (または、もう 1 つ) がこの 2 つ目の ObjectDataSource に対して ObjectDataSource を使用して、ある、`GetProductsByCategoryID(categoryID)`メソッドです。 2 つの ObjectDataSources 以降`SelectMethod`プロパティが異なる場合、それら各 ll、それぞれのキャッシュされた値があります。 次回 (ページング、並べ替え、およびなど) によって、その他のグリッドにデータをバインドする 1 つのグリッド内のある製品を編集する場合、キャッシュされた古いデータに引き続きサービスを提供され、他のグリッドから行われた変更は反映されません。

つまり、のみを使用して時間ベース切れを古いデータを可能性があるし、データの新しさが重要となるシナリオの短い切れを使用してもよい場合。 古いデータを許容しない場合は、キャッシュを断念または SQL キャッシュ依存関係を使用 (データベースのデータであると仮定すると再キャッシュする)。 SQL キャッシュ依存関係は、今後のチュートリアルで検討しましょう。

## <a name="summary"></a>まとめ

このチュートリアルでは、ObjectDataSource s 組み込みキャッシュ機能を確認します。 いくつかのプロパティを設定するだけでは、指定された対象から返される結果をキャッシュする ObjectDataSource を指示できます`SelectMethod`ASP.NET のデータ キャッシュにします。 `CacheDuration`と`CacheExpirationPolicy`項目がキャッシュされる期間とは、絶対またはスライド式有効期限を指定するプロパティです。 `CacheKeyDependency`プロパティがすべて ObjectDataSource のキャッシュ エントリの既存のキャッシュの依存関係に関連付けます。 時間ベースの有効期限に達すると、および SQL キャッシュ依存関係は通常使用する前に、ObjectDataSource のキャッシュからエントリを削除するために使用できます。

データ キャッシュには、その値は、単に、ObjectDataSource にキャッシュから ObjectDataSource の組み込み機能をプログラムで複製します。 これを行う、プレゼンテーション層で、ObjectDataSource が既定では、この機能を提供していますが、アーキテクチャの別のレイヤーでキャッシュ機能を実装しましたので合理的なされません。 これを行うを ObjectDataSource によって使用される同じロジックを繰り返す必要になります。 [次へ]、チュートリアルでは、アーキテクチャ内からのデータ キャッシュをプログラムで使用する方法が見ていきましょう。

満足プログラミング!

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET キャッシュ: 技術とベスト プラクティス](https://msdn.microsoft.com/library/aa478965.aspx)
- [.NET Framework アプリケーション用のキャッシュのアーキテクチャ ガイド](https://msdn.microsoft.com/library/ee817645.aspx)
- [ASP.NET 2.0 で出力キャッシュ](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Teresa マーフィーしました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](using-sql-cache-dependencies-cs.md)
[次へ](caching-data-in-the-architecture-vb.md)
