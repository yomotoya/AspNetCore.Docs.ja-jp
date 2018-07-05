---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
title: ObjectDataSource (VB) でデータをキャッシュ |Microsoft Docs
author: rick-anderson
description: キャッシュすると、低速と高速な Web アプリケーションの違いを意味します。 このチュートリアルでは、ASP.NET でのキャッシュで詳しく説明する 4 つの 1 つ目は.
ms.author: aspnetcontent
ms.date: 05/30/2007
ms.assetid: 2e56a733-5512-48a6-9276-70a65bbe4d5d
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: aa2af934a45ebd7e23d5d2ccf5a80f4949f1ec4f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817539"
---
<a name="caching-data-with-the-objectdatasource-vb"></a>ObjectDataSource (VB) でデータをキャッシュ
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_VB.exe)または[PDF のダウンロード](caching-data-with-the-objectdatasource-vb/_static/datatutorial58vb1.pdf)

> キャッシュすると、低速と高速な Web アプリケーションの違いを意味します。 このチュートリアルでは、ASP.NET でのキャッシュで詳しく説明する 4 つの 1 つ目です。 キャッシュの主な概念と ObjectDataSource コントロールを通じて、プレゼンテーション層にキャッシュを適用する方法について説明します。


## <a name="introduction"></a>はじめに

コンピューター サイエンス、*キャッシュ*データまたは取得のコストが情報を取得し、アクセスする方が手軽な場所にそのコピーを格納するプロセスです。 データ ドリブンのアプリケーションでは、大規模で複雑なクエリはよく、アプリケーションの実行時間の大部分を消費します。 このようなアプリケーションのパフォーマンス、次は、アプリケーション メモリに負荷の高いデータベース クエリの結果を格納することで多くの場合、改善できます。

ASP.NET 2.0 には、さまざまなキャッシュ オプションが用意されています。 キャッシュできる、web ページ全体またはユーザー コントロールにレンダリングされますマークアップ*出力キャッシュ*します。 ObjectDataSource や SqlDataSource コントロールでは、コントロール レベルでキャッシュするためにも、データを許可するキャッシュの機能を提供します。 ASP.NET s*データ キャッシュ*により、ページ開発者は、プログラムでのキャッシュ オブジェクトをリッチなキャッシュ API を提供します。 このチュートリアルと ObjectDataSource s の使用について説明します、次の 3 つのデータ キャッシュだけでなく、機能をキャッシュします。 起動時にアプリケーション全体のデータをキャッシュする方法と新しい SQL キャッシュ依存関係を使用してキャッシュされたデータを保持する方法についても説明します。 これらのチュートリアルでは、出力キャッシュは表示されません。 出力キャッシュの詳細については、次を参照してください。 [ASP.NET 2.0 で出力キャッシュ](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)します。

キャッシュを適用できるアーキテクチャでは、任意の場所でデータ アクセス層からのプレゼンテーション レイヤーを通して行われます。 このチュートリアルでは、ObjectDataSource コントロールを通じて、プレゼンテーション層にキャッシュを適用することに紹介します。 について説明します、次のチュートリアルでは、ビジネス ロジック層でデータをキャッシュします。

## <a name="key-caching-concepts"></a>キーのキャッシュの概念

秒のアプリケーションを大幅に改善できるキャッシュ全体のパフォーマンスとスケーラビリティが生成する負荷の高いデータを取得しより効率的にアクセスできる場所にそのコピーを格納します。 キャッシュは、実際、基になるデータのコピーだけを保持しているため、古いかかるまたは*古い*、基になるデータが変更された場合。 このページの開発者がキャッシュ項目があるされる条件を指定できます*削除*キャッシュからいずれかを使用します。

- **時間ベースの条件**絶対またはスライド時間のキャッシュに項目を追加することがあります。 たとえば、ページの開発者は、60 秒間の期間に可能性があります。 絶対時間の場合は、キャッシュされた項目にアクセスした頻度に関係なく、キャッシュに追加された後、60 秒が削除されます。 スライディング継続時間が、キャッシュされた項目は最終アクセス後に 60 秒間に削除されます。
- **依存関係に基づく条件**依存関係がキャッシュに追加されたときに項目を関連付けることができます。 項目 %s の依存関係が変更されたときに、キャッシュから削除されます。 ファイルを別のキャッシュ項目または 2 つの組み合わせ、依存関係があります。 ASP.NET 2.0 では、開発者は、キャッシュに項目を追加して、それを基になるデータベースのデータが変更されたときの削除を有効にする SQL キャッシュ依存関係もできます。 近日出版予定の SQL キャッシュ依存関係を究明する[を使用して SQL キャッシュ依存関係](using-sql-cache-dependencies-vb.md)チュートリアル。

指定された削除条件に関係なく、キャッシュ内の項目があります*清掃*前に、時間ベースまたは依存関係に基づく条件は満たされました。 キャッシュのキャパシティに達して新しいファイルを追加する前に、既存の項目を削除してください。 その結果、使用する場合プログラムでキャッシュされたデータが s を常に想定することが重要、キャッシュされたデータいない存在できます。 次のチュートリアルでは、プログラムでキャッシュからデータにアクセスするときに使用するパターンについて説明します*アーキテクチャでデータをキャッシュ*します。

キャッシュは、アプリケーションから複数のパフォーマンスをつかんでの経済的な手段を提供します。 として[Steven Smith](http://aspadvice.com/blogs/ssmith/)彼の記事で示される[ASP.NET キャッシュ: テクニックとベスト プラクティス](https://msdn.microsoft.com/library/aa478965.aspx):

キャッシュは、上達するための十分なパフォーマンス、多くの時間と分析を必要とせず、適切な方法です。 メモリが低コスト、30 秒、1 日または週に、コードまたはデータベースの最適化を試みるをかける代わりに、出力をキャッシュすることによって必要なパフォーマンスを得ることができる場合のキャッシュ ソリューションを実行するため ([ok] は秒前-30 のデータと仮定) 移動します。 最終的には、不適切なデザインはおそらく追いつきます、使用するため、コースのアプリケーションを正しく設計するとき必要があります。 良いための十分なパフォーマンスを向上を取得する場合は、キャッシュできる優れた [方法]、そのためには時間がある場合、後で、アプリケーションをリファクターする時間を購入します。

キャッシュと、ほどのパフォーマンスの強化が提供することができます、適用可能でないすべての状況でなど、データをリアルタイムで頻繁に更新を使用するかも間もなく有効期間の古いデータが許容されないアプリケーションとします。 ただし、大半のアプリケーションでは、キャッシュの使用します。 ASP.NET 2.0 でのキャッシュの詳細についてを参照してください、[パフォーマンス キャッシュ](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx)のセクション、 [ASP.NET 2.0 クイック スタート チュートリアル](https://quickstarts.asp.net/QuickStartv20/aspnet/)します。

## <a name="step-1-creating-the-caching-web-pages"></a>手順 1: Web ページのキャッシュを作成します。

ObjectDataSource s のキャッシュ機能の探索を始める前に、このチュートリアルと、次の 3 つの必要がありますが、web サイト プロジェクトで ASP.NET ページを作成する最初に少し s を使用できます。 という名前の新しいフォルダーを追加することで開始`Caching`します。 次に、次の ASP.NET ページを使用する各ページに関連付けることを確認、そのフォルダーに追加、`Site.master`マスター ページ。

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`


![キャッシュに関連するチュートリアルについては、ASP.NET ページに追加します。](caching-data-with-the-objectdatasource-vb/_static/image1.png)

**図 1**: キャッシュに関連するチュートリアルについては、ASP.NET ページに追加します。


などの他のフォルダーで`Default.aspx`で、`Caching`フォルダーは、チュートリアルのセクションで一覧表示します。 いることを思い出してください、`SectionLevelTutorialListing.ascx`ユーザー コントロールは、この機能を提供します。 そのため、このユーザー コントロールを追加`Default.aspx`をページのデザイン ビューに ソリューション エクスプ ローラーからドラッグしています。


[![図 2: SectionLevelTutorialListing.ascx ユーザー コントロールを Default.aspx に追加します。](caching-data-with-the-objectdatasource-vb/_static/image3.png)](caching-data-with-the-objectdatasource-vb/_static/image2.png)

**図 2**図 2: 追加、`SectionLevelTutorialListing.ascx`ユーザー コントロールを`Default.aspx`([フルサイズの画像を表示する をクリックします。](caching-data-with-the-objectdatasource-vb/_static/image4.png))。


最後に、これらのページを追加するエントリとして、`Web.sitemap`ファイル。 具体的には、バイナリ データで作業した後、次のマークアップを追加`<siteMapNode>`:


[!code-xml[Main](caching-data-with-the-objectdatasource-vb/samples/sample1.xml)]

更新した後`Web.sitemap`、時間、ブラウザーを使ってチュートリアル web サイトを表示するのにはかかりません。 左側のメニューで、チュートリアルについては、キャッシュ項目できるようになりました。


![サイト マップのチュートリアルでは、キャッシュ エントリになりました](caching-data-with-the-objectdatasource-vb/_static/image5.png)

**図 3**: サイト マップのチュートリアルでは、キャッシュ エントリになりました


## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>手順 2: Web ページで製品の一覧を表示します。

このチュートリアルでは、ObjectDataSource コントロール s 組み込みキャッシュ機能を使用する方法について説明します。 これらの機能を見て、前に、まず必要がありますのページから作業をします。 Let s は、製品情報を一覧から、ObjectDataSource によって取得された、GridView を使用する web ページを作成、`ProductsBLL`クラス。

開いて開始、`ObjectDataSource.aspx`ページで、`Caching`フォルダー。 ツールボックスからデザイナーに、GridView をドラッグして、設定、`ID`プロパティを`Products`、という名前の新しい ObjectDataSource コントロールにバインドを選択して、スマート タグからとは、`ProductsDataSource`します。 構成を使用する ObjectDataSource、`ProductsBLL`クラス。


[![ProductsBLL クラスを使用する ObjectDataSource を構成します。](caching-data-with-the-objectdatasource-vb/_static/image7.png)](caching-data-with-the-objectdatasource-vb/_static/image6.png)

**図 4**: 構成に使用する ObjectDataSource、`ProductsBLL`クラス ([フルサイズの画像を表示する をクリックします](caching-data-with-the-objectdatasource-vb/_static/image8.png))。


このページでは、s GridView の s インターフェイスを通じて、ObjectDataSource にキャッシュされたデータが変更されたときの動作を検証できるように編集可能な GridView を作成することができます。 既定の設定に選択します タブで、ドロップダウン リストのままに`GetProducts()`、更新プログラム タブを選択した項目の変更が、`UpdateProduct`を受け入れるオーバー ロード`productName`、 `unitPrice`、および`productID`入力パラメーターとして。


[![適切な UpdateProduct オーバー ロードに更新 タブのドロップダウン リストを設定します。](caching-data-with-the-objectdatasource-vb/_static/image10.png)](caching-data-with-the-objectdatasource-vb/_static/image9.png)

**図 5**: 更新 タブのドロップダウン リストを適切に設定`UpdateProduct`オーバー ロード ([フルサイズの画像を表示する をクリックします](caching-data-with-the-objectdatasource-vb/_static/image11.png))。


最後に、(なし) を挿入および削除のタブで、ドロップダウン リストを設定し、[完了] をクリックします。 データ ソース構成ウィザードを完了すると、Visual Studio 設定 ObjectDataSource s`OldValuesParameterFormatString`プロパティを`original_{0}`します。 説明したように、[挿入の概要、更新、およびデータの削除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)チュートリアルでは、このプロパティは、宣言構文から削除するか、その既定値に戻す設定する必要がある`{0}`、更新したワークフローのためにエラーを行わずに続行します。

さらに、ウィザードの完了時に Visual Studio にフィールドを追加、GridView の各製品のデータ フィールドの。 削除以外のすべて、 `ProductName`、 `CategoryName`、および`UnitPrice`BoundFields します。 次に、更新、`HeaderText`各製品カテゴリで、価格、およびこれら BoundFields のプロパティそれぞれします。 以降、`ProductName`フィールドは必須です、BoundField を TemplateField に変換し、RequiredFieldValidator への追加、`EditItemTemplate`します。 同様に、変換、`UnitPrice`を TemplateField BoundField、ユーザーが入力した値が有効な通貨の値より大きいまたは 0 に等しい s であることを確認する CompareValidator を追加します。 これらの変更だけでなく自由に右揃えなど、美的な変更を実行、`UnitPrice`値、または書式設定を指定する、`UnitPrice`その読み取り専用と編集インターフェイスのテキスト。

GridView のスマート タグの編集を有効にするチェック ボックスをオンして、GridView を編集可能です。 ページングを有効にして、並べ替えを有効にするチェック ボックスも確認してください。

> [!NOTE]
> GridView の編集インターフェイスをカスタマイズする方法のレビューが必要ですか。 そうである場合に戻って、[データ変更インターフェイスをカスタマイズ](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md)チュートリアル。


[![編集、並べ替え、およびページングを GridView サポートを有効にします。](caching-data-with-the-objectdatasource-vb/_static/image13.png)](caching-data-with-the-objectdatasource-vb/_static/image12.png)

**図 6**: 編集、並べ替え、およびページングの GridView サポートを有効にする ([フルサイズの画像を表示する をクリックします](caching-data-with-the-objectdatasource-vb/_static/image14.png))。


GridView のこれらの変更を加えたら、GridView コントロールと ObjectDataSource s の宣言型マークアップを次のようになります。


[!code-aspx[Main](caching-data-with-the-objectdatasource-vb/samples/sample2.aspx)]

図 7 に示す、編集可能な GridView には、名前、カテゴリ、および各データベースでは、製品の価格が一覧表示します。 テスト ページの機能の並べ替え結果をそれらを使用してページとレコードを編集します。


[![各製品名、カテゴリ、および価格は、ソート可能、Pageable、編集可能な GridView に表示されます。](caching-data-with-the-objectdatasource-vb/_static/image16.png)](caching-data-with-the-objectdatasource-vb/_static/image15.png)

**図 7**: 各製品の名前、カテゴリ、および価格は、ソート可能、Pageable、編集可能な GridView で表示されます ([フルサイズの画像を表示する をクリックします](caching-data-with-the-objectdatasource-vb/_static/image17.png))。


## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>手順 3: データの要求をときに、ObjectDataSource を調べること

`Products` GridView を呼び出すことによって表示するには、そのデータを取得、`Select`のメソッド、 `ProductsDataSource` ObjectDataSource します。 この ObjectDataSource は、ビジネス ロジック層 s のインスタンスを作成します。`ProductsBLL`クラスと呼び出しの`GetProducts()`メソッドをさらに、データ アクセス層の s を呼び出す`ProductsTableAdapter`s`GetProducts()`メソッド。 DAL メソッドは、Northwind データベースに接続し、構成済みの問題`SELECT`クエリ。 このデータは、DAL では、パッケージ化に返されます、`NorthwindDataTable`します。 DataTable オブジェクトには、GridView に返されます ObjectDataSource に返されます BLL が返されます。 GridView を作成し、`GridViewRow`オブジェクトごとに`DataRow`DataTable には、および各`GridViewRow`がクライアントに返され、秒のユーザーのブラウザーで表示される HTML に最終的にレンダリングされます。

この一連のイベントは、GridView は、その基になるデータにバインドする必要があるたびに発生します。 ページが初めてアクセスした、または、GridView を並べ替えるときに、編集、またはインターフェイスの削除は、その組み込みを使用して GridView のデータを変更するときに、データの 1 つのページから移動するときに場合に起こります。 GridView のビュー ステートが無効になっている場合は、各ポストバックもに、GridView に再バインドできます。 GridView も明示的に再バインドできるそのデータを呼び出すことによってその`DataBind()`メソッド。

データベースからデータを取得する頻度を完全に理解するには、s、データの再取得されたときを示すメッセージを表示することができます。 GridView という名前の上のラベルの Web コントロールを追加`ODSEvents`します。 クリアしますその`Text`プロパティとその`EnableViewState`プロパティを`False`します。 ラベルの下にあるボタンの Web コントロールを追加し、設定、`Text`ポストバックへのプロパティ。


[![GridView の上のページに、ラベルとボタンを追加します。](caching-data-with-the-objectdatasource-vb/_static/image19.png)](caching-data-with-the-objectdatasource-vb/_static/image18.png)

**図 8**: 上、GridView ページに、ラベルとボタンを追加 ([フルサイズの画像を表示する をクリックします](caching-data-with-the-objectdatasource-vb/_static/image20.png))。


ワークフローの間にデータ アクセス、ObjectDataSource の`Selecting`基になるオブジェクトが作成される前に、イベントが発生し、構成されているメソッドが呼び出されます。 このイベントのイベント ハンドラーを作成し、次のコードを追加します。


[!code-vb[Main](caching-data-with-the-objectdatasource-vb/samples/sample3.vb)]

ObjectDataSource は、要求をデータ アーキテクチャを作成するたびに、ラベル テキスト選択イベントを発生が表示されます。

ブラウザーでこのページを参照してください。 ページが初めてアクセスしたときに発生したテキスト選択イベントが表示されます。 ポストバックのボタンをクリックし、テキストが表示されないことに注意してください (と仮定すると GridView s`EnableViewState`プロパティに設定されて`True`、既定値)。 ポストバックの GridView がビューステートから再構築し、そのデータに対して ObjectDataSource には t が有効にするためです。 並べ替え、ページング、または、データを編集する一方、そのデータ ソースを再バインドする GridView とするイベントにテキストが再表示されますが発生したためです。


[![発生するイベントが表示されるたびに、GridView がそのデータ ソースにバインドし、](caching-data-with-the-objectdatasource-vb/_static/image22.png)](caching-data-with-the-objectdatasource-vb/_static/image21.png)

**図 9**: ときに GridView がそのデータ ソースにバインドし、発生するイベントが表示されます ([フルサイズの画像を表示する をクリックします](caching-data-with-the-objectdatasource-vb/_static/image23.png))。


[![ビューステートから再構築する GridView をポストバック ボタンがクリックすると](caching-data-with-the-objectdatasource-vb/_static/image25.png)](caching-data-with-the-objectdatasource-vb/_static/image24.png)

**図 10**: ポストバックのボタンをクリックすると、ビュー状態から再構築する GridView ([フルサイズの画像を表示する をクリックします](caching-data-with-the-objectdatasource-vb/_static/image26.png))。


データがを介してページングまたは並べ替えられて、毎回データベースのデータを取得するは無駄と思われる場合があります。 結局のところ、以降既定のページングを使用して、ObjectDataSource が取得してすべてのレコードの最初のページを表示するときに。 GridView で並べ替えとページングのサポートが提供されない場合でも、データを取得、データベースからすべてのユーザー (および、ビュー ステートが無効になっている場合、すべてのポストバックの) ページが初めてアクセスした各時間。 これらのデータベースを追加要求は余分な GridView には、すべてのユーザーに同じデータが表示されている場合。 返される結果をキャッシュできない、`GetProducts()`メソッドおよび bind、GridView に結果をキャッシュしますか?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>手順 4: ObjectDataSource を使用してデータをキャッシュします。

いくつかのプロパティを設定するだけでは、ASP.NET のデータ キャッシュで取得したデータを自動的にキャッシュする ObjectDataSource を構成できます。 ObjectDataSource のキャッシュに関連するプロパティを次に示します。

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx)に設定する必要があります`True`キャッシュを有効にします。 既定値は `False` です。
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx)時間 (秒)、データがキャッシュされている量。 既定値は 0 です。 場合は、ObjectDataSource にデータはキャッシュのみ`EnableCaching`は`True`と`CacheDuration`0 より大きい値に設定されます。
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx)に設定することができます`Absolute`または`Sliding`します。 場合`Absolute`、ObjectDataSource が取得するデータをキャッシュ`CacheDuration`秒; 場合`Sliding`、データにアクセスされていない後でのみ有効期限が切れる`CacheDuration`秒。 既定値は `Absolute` です。
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx)このプロパティを使用して、既存のキャッシュ依存関係と ObjectDataSource s のキャッシュ エントリを関連付けます。 ObjectDataSource のデータ エントリ途中で削除できるキャッシュから期限切れに関連付けられた`CacheKeyDependency`します。 このプロパティは、SQL キャッシュ依存関係を関連付ける ObjectDataSource のキャッシュに最もよく使用は、トピックについて説明します、将来[を使用して SQL キャッシュ依存関係](using-sql-cache-dependencies-vb.md)チュートリアル。

S を構成できるように、 `ProductsDataSource` ObjectDataSource 絶対スケールで 30 秒間のデータをキャッシュします。 ObjectDataSource s 設定`EnableCaching`プロパティを`True`とその`CacheDuration`30 に設定するプロパティ。 ままに、`CacheExpirationPolicy`プロパティの既定の設定に`Absolute`します。


[![30 秒間のデータをキャッシュする ObjectDataSource を構成します。](caching-data-with-the-objectdatasource-vb/_static/image28.png)](caching-data-with-the-objectdatasource-vb/_static/image27.png)

**図 11**: 30 秒間のデータをキャッシュする ObjectDataSource を構成する ([フルサイズの画像を表示する をクリックします](caching-data-with-the-objectdatasource-vb/_static/image29.png))。


変更を保存し、ブラウザーでこのページを再検討します。 選択すると発生するイベントのテキストは最初に、データがキャッシュ内のページに初めてアクセスするときにも表示されます。 以降のポストバック トリガー並べ替え、ポストバックのボタンをクリックしてページング、または編集 または キャンセル ボタンをクリックすると、*いない*テキストを再表示するイベントが発生します。 これは、ため、 `Selecting` 、ObjectDataSource がその基になるオブジェクトからのデータを取得するときにのみイベントが発生した、`Selecting`データ キャッシュからデータを取り込む場合、イベントは発生しません。

30 秒後、データをキャッシュから削除されます。 場合、データをキャッシュから削除もは ObjectDataSource s `Insert`、 `Update`、または`Delete`メソッドが呼び出されます。 そのため、30 秒が経過または [更新] ボタンがクリックしてされた並べ替え、ページング、またはその基になるオブジェクトからのデータを取得する ObjectDataSource により、編集、または [キャンセル] ボタンをクリックすると後のテキストが発生するイベントを表示するときに、`Selecting`イベントが発生します。 返される結果は、データ キャッシュに配置されます。

> [!NOTE]
> 頻繁に発生するイベントの文字列の選択を表示する場合、キャッシュされたデータで作業を ObjectDataSource が予想される場合もありますメモリ制約によって。 十分な空きメモリがない場合、ObjectDataSource によってキャッシュに追加されたデータを清掃がある可能性があります。 ObjectDataSource されないデータまたはキャッシュのみにキャッシュが正しく表示されない場合は、データは、断続的空きメモリの一部のアプリケーションを閉じるし、もう一度やり直してください。


図 12 は、ワークフローをキャッシュする ObjectDataSource s を示しています。 イベントが発生したときに、画面にテキストが表示される、ため、データがキャッシュに存在していなかったし、基になるオブジェクトから取得する必要があります。 このテキストは、不足しているときにただし、その s データがキャッシュから使用可能なためです。 データがキャッシュから返される場合が s の基になるオブジェクトへの呼び出しと、そのため、データベース クエリではなく実行されません。


![ObjectDataSource ストアとデータ キャッシュからそのデータを取得します](caching-data-with-the-objectdatasource-vb/_static/image30.png)

**図 12**: ObjectDataSource を格納し、データ キャッシュからのデータの取得


各 ASP.NET アプリケーションでは、すべてのページと訪問者の間で共有 s インスタンス、独自のデータ キャッシュがあります。 ObjectDataSource でデータ キャッシュに格納されたデータが同様に、ページにアクセスするすべてのユーザーの間で共有されることを意味します。 これを確認するには、開く、`ObjectDataSource.aspx`ブラウザーでページ。 最初のページにアクセスして、(前のテストによって、キャッシュに追加されたデータが、ここまでで、削除された) すると仮定した場合に選択すると発生するイベントのテキストが表示されます。 2 番目のブラウザー インスタンスとコピーを開き、2 番目の最初のブラウザー インスタンスから URL を貼り付けます。 2 番目のブラウザー インスタンスのため、選択すると発生するイベントのテキストは表示されませんを使用して、同じキャッシュ データを 1 つ目として。

ObjectDataSource がキャッシュのキー値が含まれていますを使用して、取得したデータをキャッシュに挿入するとき:`CacheDuration`と`CacheExpirationPolicy`プロパティの値です指定されていると、ObjectDataSource で使用されている、基になるビジネス オブジェクトの型。使用して、 [ `TypeName`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx)(`ProductsBLL`、この例では); の値、`SelectMethod`名前と値のパラメーターのプロパティ、`SelectParameters`コレクションとそのの値`StartRowIndex`と`MaximumRows`を実装するときに使用されるプロパティ、[カスタム ページング](../paging-and-sorting/paging-and-sorting-report-data-vb.md)します。

これらの値を変更、一意のキャッシュ エントリをにより、これらのプロパティの組み合わせとして、キャッシュ キーの値を作成します。 たとえば、過去のチュートリアルで ve を使用して参照される、`ProductsBLL`クラスの`GetProductsByCategoryID(categoryID)`、指定したカテゴリのすべての製品が返されます。 1 人のユーザーが、ページおよび表示の飲み物を付属可能性がありますが、 `CategoryID` 1。 ObjectDataSource に関わりなく、その結果がキャッシュされている場合、`SelectParameters`値、別のユーザーがこのページにキャッシュ内にあった調味料飲み物の製品の中に表示すると、調味料ではなく、キャッシュされた飲料、d を参照してください。 値を含むがこれらのプロパティによって、キャッシュ キーを変更することで、 `SelectParameters`ObjectDataSource が beverages と調味料の個別のキャッシュ エントリを保持します。

## <a name="stale-data-concerns"></a>古いデータの問題

ObjectDataSource がいずれかの場合のキャッシュからその項目を自動的に削除の`Insert`、 `Update`、または`Delete`メソッドが呼び出されます。 これにより、ページで、データが変更されたときに、キャッシュ エントリをクリアする古いデータから保護します。 ただし、キャッシュを使用した古いデータが表示されたまま、ObjectDataSource 可能性は。 最も簡単な場合は、により、データは、データベース内で直接変更できます。 おそらく、データベース管理者は、データベース内のレコードの一部を変更するスクリプトを実行しました。

このシナリオはより複雑な方法で展開も可能性があります。 ObjectDataSource は、そのデータ変更メソッドのいずれかが呼び出されると、キャッシュからその項目を削除、キャッシュされた項目が削除された、プロパティの値の ObjectDataSource s の特定の組み合わせ (`CacheDuration`、 `TypeName`、 `SelectMethod`、同様に続きます)。 使用して、異なる 2 つの ObjectDataSources があれば`SelectMethods`または`SelectParameters`が、まだ更新できます、同じデータを 1 つの ObjectDataSource 可能性があります行を更新し、独自のキャッシュ エントリが、対応する行の 2 つ目の ObjectDataSource が無効になります。提供されますが、キャッシュされたからです。 この機能が発生するページを作成することをお勧めします。 ObjectDataSource のキャッシュを使用し、データを取得するように構成からのデータをプルする編集可能な GridView を表示するページを作成、`ProductsBLL`クラスの`GetProducts()`メソッド。 もう 1 つ追加編集可能な GridView コントロールと ObjectDataSource をこのページ (または、もう 1 つ) がこの 2 つ目の ObjectDataSource を使用して、それがある、`GetProductsByCategoryID(categoryID)`メソッド。 2 つの ObjectDataSources 以降`SelectMethod`プロパティが異なる場合、これら各 ll 独自のキャッシュされた値があります。 次回 (ページング、並べ替え、およびなど) を他のグリッドにデータをバインドする 1 つのグリッド内の製品を編集する場合も、キャッシュされた古いデータが提供され、他のグリッドから加えられた変更は反映されません。

つまり場合に、のみ使用切れの時間ベースの古いデータの可能性があるし、データの新しさが重要なシナリオの短い切れを使用してもよい。 古いデータが許容されない場合はキャッシュを断念または SQL キャッシュ依存関係を使用して、いずれか (これはデータベースのデータと仮定すると再キャッシュする)。 今後のチュートリアルでは SQL キャッシュ依存関係について説明します。

## <a name="summary"></a>まとめ

このチュートリアルでは、ObjectDataSource s 組み込みキャッシュ機能を確認します。 いくつかのプロパティを設定するだけでは、指定された対象から返される結果をキャッシュする ObjectDataSource を指示できます`SelectMethod`ASP.NET のデータ キャッシュにします。 `CacheDuration`と`CacheExpirationPolicy`プロパティは、項目がキャッシュされる期間とは、絶対またはスライディング有効期限を示します。 `CacheKeyDependency`プロパティが既存のキャッシュ依存関係を ObjectDataSource s のキャッシュ エントリのすべてを関連付けます。 時間ベースの有効期限に達するし、SQL キャッシュ依存関係は通常使用する前に、キャッシュから ObjectDataSource s のエントリを削除するために使用できます。

データ キャッシュには、その値は、単に、ObjectDataSource にキャッシュからプログラムで ObjectDataSource の組み込み機能を複製します。 これは、ObjectDataSource は、既定では、この機能を提供していますが、アーキテクチャの別のレイヤーにキャッシュ機能を実装できるため、プレゼンテーション層でこれには t が合理的です。 これを行うには、ObjectDataSource で使用される同じロジックを繰り返す必要があります。 [次へ]、チュートリアルでは、アーキテクチャ内からのデータ キャッシュをプログラムで操作する方法について説明します。

満足のプログラミングです。

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET のキャッシュ: テクニックとベスト プラクティス](https://msdn.microsoft.com/library/aa478965.aspx)
- [.NET Framework アプリケーション用のキャッシュのアーキテクチャ ガイド](https://msdn.microsoft.com/library/ee817645.aspx)
- [ASP.NET 2.0 で出力キャッシュ](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Teresa Murphy でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](using-sql-cache-dependencies-cs.md)
> [次へ](caching-data-in-the-architecture-vb.md)
