---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
title: マスター ページとサイト ナビゲーション (VB) |Microsoft Docs
author: rick-anderson
description: ユーザー フレンドリな web サイトの一般的な特性の 1 つは、一貫性があり、サイト全体のページ レイアウトとナビゲーション スキームを備えることです。 このチュートリアルの検索方法を y.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 022801d8-a327-4d0c-8780-6094c9cee00d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
msc.type: authoredcontent
ms.openlocfilehash: c823421a6b934951dd08656b48801c3d26247c5c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372045"
---
<a name="master-pages-and-site-navigation-vb"></a>マスター ページとサイト ナビゲーション (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_3_VB.exe)または[PDF のダウンロード](master-pages-and-site-navigation-vb/_static/datatutorial03vb1.pdf)

> ユーザー フレンドリな web サイトの一般的な特性の 1 つは、一貫性があり、サイト全体のページ レイアウトとナビゲーション スキームを備えることです。 このチュートリアルでは、簡単に更新可能なすべてのページにわたって一貫したルック アンド フィールを作成する方法で検索します。


## <a name="introduction"></a>はじめに

ユーザー フレンドリな web サイトの一般的な特性の 1 つは、一貫性があり、サイト全体のページ レイアウトとナビゲーション スキームを備えることです。 ASP.NET 2.0 を両方サイト全体のページ レイアウトとナビゲーション スキームの実装を大幅に簡略化する 2 つの新しい機能が導入されています。 マスター ページとサイト ナビゲーション。 指定された編集可能な領域を持つサイト全体のテンプレートを作成する開発者のマスター ページを使用します。 このテンプレートは、サイト内の ASP.NET ページに適用できます。 マスター ページの編集可能な領域を指定したため、このような ASP.NET ページはコンテンツを提供のみ必要がありますが、マスター ページの他のすべてのマークアップをマスター ページを使用するすべての ASP.NET ページの全体で同一です。 このモデルを定義し、サイト全体のページ レイアウトを簡単に更新可能なすべてのページにわたって一貫したルック アンド フィールを作成するを簡単に一元管理できます。

[サイト ナビゲーション システム](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)機構を開発者に、サイト マップを定義するページとプログラムでクエリを実行するには、そのサイト マップの API の両方を提供します。 新しいナビゲーション Web コントロールが Menu、TreeView、および SiteMapPath 簡単にでは、一般的なナビゲーション ユーザー インターフェイス要素に、サイト マップのすべてまたは一部をレンダリングします。 つまり、XML フォーマット ファイルで、サイト マップを定義するように、既定のサイト ナビゲーション プロバイダー使用します。

これらの概念を説明し、チュートリアル web サイトをより使いやすく、サイト全体のページ レイアウトを定義する、サイト マップの実装、およびナビゲーション UI を追加するこのレッスンをについて説明します。 このチュートリアルの最後で、チュートリアルの web ページを構築するための光沢のある web サイトのデザインがあります。


[![このチュートリアルの最終結果](master-pages-and-site-navigation-vb/_static/image2.png)](master-pages-and-site-navigation-vb/_static/image1.png)

**図 1**: このチュートリアルの「最後の結果 ([フルサイズ画像を表示する をクリックします。](master-pages-and-site-navigation-vb/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>手順 1: マスター ページを作成します。

最初の手順では、サイトのマスター ページを作成します。 ここでは、web サイトは、型指定されたデータセットのみで構成されています (`Northwind.xsd`で、`App_Code`フォルダー)、BLL クラス (`ProductsBLL.vb`、`CategoriesBLL.vb`で、すべてで、`App_Code`フォルダー)、データベース (`NORTHWND.MDF`の`App_Data`フォルダー)、構成ファイル (`Web.config`)、および CSS スタイル シート ファイル (`Styles.css`)。 これらのページと私たちがするを再調査するこれらの例で詳しく今後のチュートリアルであるために、最初の 2 つのチュートリアルから DAL と BLL を使用するかを示すファイルはクリーンアップします。


![このプロジェクト内のファイル](master-pages-and-site-navigation-vb/_static/image4.png)

**図 2**: このプロジェクト内のファイル


マスター ページを作成するには、ソリューション エクスプ ローラーでプロジェクト名を右クリックし、新しい項目の追加を選択します。 テンプレートの一覧からマスター ページの種類を選択し、という名前を付けます`Site.master`します。


[![新しいマスター ページ、web サイトを追加します。](master-pages-and-site-navigation-vb/_static/image6.png)](master-pages-and-site-navigation-vb/_static/image5.png)

**図 3**: web サイトに新しいマスター ページを追加 ([フルサイズの画像を表示する をクリックします](master-pages-and-site-navigation-vb/_static/image7.png))。


マスター ページで、ここで、サイト全体のページ レイアウトを定義します。 デザイン ビューを使用して、必要なレイアウトまたは Web コントロールを追加またはソース ビューで手動でマークアップを手動で追加することができます。 マスター ページを使用して[カスケード スタイル シート](http://www.w3schools.com/css/default.asp)配置と外部のファイルで定義された CSS 設定でスタイル`Style.css`します。 CSS 規則が定義されているときに、次に示すマークアップからわかることはできません、ようにナビゲーション`<div>`のようにし、左側に表示が固定幅 200 ピクセルに絶対にコンテンツが配置されています。

Site.master


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample1.aspx)]

マスター ページは、静的なページ レイアウトやマスター ページを使用する ASP.NET ページが編集可能なリージョンの両方を定義します。 これらのコンテンツの編集可能なリージョンは、コンテンツ内で示しているようにプレース ホルダー コントロールで示されます`<div>`します。 このマスター ページが 1 つのプレース ホルダー (`MainContent`) をマスター ページの複数の ContentPlaceHolders があります。

上記で入力したマークアップをデザイン ビューに切り替えると、マスター ページのレイアウトを示します。 このマスター ページを使用する任意の ASP.NET ページのマークアップを指定する機能によりこの均一なレイアウトになります、`MainContent`リージョン。


[![デザイン ビューで表示した場合、マスター ページ](master-pages-and-site-navigation-vb/_static/image9.png)](master-pages-and-site-navigation-vb/_static/image8.png)

**図 4**:、マスター ページ、ときに表示をデザイン ビュー ([フルサイズの画像を表示する をクリックします](master-pages-and-site-navigation-vb/_static/image10.png))。


## <a name="step-2-adding-a-homepage-to-the-website"></a>手順 2: ホーム ページを web サイトに追加します。

定義されているマスター ページ、web サイトの ASP.NET ページを追加する準備ができました。 追加してみましょう`Default.aspx`、当社の web サイトのホーム ページ。 ソリューション エクスプ ローラーでプロジェクト名を右クリックし、新しい項目の追加を選択します。 ファイルの名前とテンプレートの一覧から Web フォームのオプションを選択`Default.aspx`します。 また、「マスター ページの選択」チェック ボックスをオンします。


[![マスター ページのチェック ボックスをチェックする、新しい Web フォームの追加します。](master-pages-and-site-navigation-vb/_static/image12.png)](master-pages-and-site-navigation-vb/_static/image11.png)

**図 5**: [マスター ページのチェック ボックスをチェックする、新しい Web フォームの追加 ([フルサイズの画像を表示する] をクリックします](master-pages-and-site-navigation-vb/_static/image13.png))。


[Ok] ボタンをクリックすると、この新しい ASP.NET ページを使用する必要がありますマスター ページの選択を求めます。 プロジェクトで複数のマスター ページを保持できますが、1 つだけあります。


[![この ASP.NET ページを使用する必要があります、マスター ページの選択します。](master-pages-and-site-navigation-vb/_static/image15.png)](master-pages-and-site-navigation-vb/_static/image14.png)

**図 6**: マスター ページにこの ASP.NET ページは使用を選択 ([フルサイズの画像を表示する をクリックします](master-pages-and-site-navigation-vb/_static/image16.png))。


新しい ASP.NET ページでは、マスター ページを選択した後、次のマークアップが含まれます。

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample2.aspx)]

`@Page`ディレクティブがありますが使用されるマスター ページ ファイルへの参照 (`MasterPageFile="~/Site.master"`)、ASP.NET ページのマークアップには、コンテンツ コントロールの各コントロールの使用のマスター ページで定義されているプレース ホルダー コントロールが含まれている`ContentPlaceHolderID`特定のプレース ホルダーへのコンテンツのマッピングを制御します。 コンテンツ コントロールには、マークアップの配置に対応するプレース ホルダーで表示します。 設定、`@Page`ディレクティブの`Title`ホームに属性し、コンテンツ コントロールに歓迎内容を追加します。

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample3.aspx)]

`Title`属性、`@Page`ディレクティブにより場合でも、ASP.NET ページからページのタイトルを設定する、`<title>`マスター ページの要素が定義されています。 設定できますタイトル プログラムを使用して`Page.Title`します。 またを注意スタイル シートへのマスター ページの参照 (など`Style.css`) は、ASP.NET ページがマスター ページから相対的ではどのようなディレクトリに関係なく、任意の ASP.NET ページで機能するように自動的に更新します。

ブラウザーで、ページがどのように表示されるかがわかりますデザイン ビューに切り替えます。 設計のコンテンツの編集可能なリージョンのみが編集可能な ASP.NET ページ用マスター ページで定義されている非プレース ホルダーのマークアップを表示するメモが淡色表示されます。


[![ASP.NET ページのデザイン ビューは、編集可能なが編集可能なリージョンの両方を示しています。](master-pages-and-site-navigation-vb/_static/image18.png)](master-pages-and-site-navigation-vb/_static/image17.png)

**図 7**: 非編集可能領域と、ASP.NET ページを示しています両方、編集可能なデザイン ビュー ([フルサイズの画像を表示する をクリックします](master-pages-and-site-navigation-vb/_static/image19.png))。


ときに、`Default.aspx`ページがブラウザーでアクセスした、ページのマスター ページのコンテンツと、ASP、ASP.NET エンジンを自動的にマージします。NET はのコンテンツ、および要求側のブラウザーに送信される最終的な HTML に結合されたコンテンツを表示します。 マスター ページのコンテンツが更新されたときに、このマスター ページを使用するすべての ASP.NET ページは、次回ユーザーが要求したコンテンツに新しいマスター ページ remerged、コンテンツがあります。 マスター ページ モデルで 1 つのページは、簡単に言えば、レイアウト テンプレートには、(マスター ページ) が定義されている変更はすぐにサイト全体に反映されます。

## <a name="adding-additional-aspnet-pages-to-the-website"></a>その他の ASP.NET ページ、web サイトへの追加

最終的にさまざまなレポート作成のデモを保持するサイトに追加の ASP.NET ページのスタブを追加するには、少し見てみましょう。 複数存在するがみましょうのスタブのページを作成するのではなくという時そのため、合計で 35 のデモは、最初のいくつかを作成します。 デモの多くのカテゴリもある、ために管理を支援するデモ フォルダーを追加のカテゴリ。 ここでは、次の 3 つのフォルダーを追加します。

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

図 8 ソリューション エクスプ ローラーで示すように最後に、新しいファイルを追加します。 各ファイルを追加する場合は、「マスター ページの選択」チェック ボックスをオンにしてください。


![次のファイルを追加します。](master-pages-and-site-navigation-vb/_static/image20.png)

**図 8**: 次のファイルの追加


## <a name="step-2-creating-a-site-map"></a>手順 2: サイト マップの作成

少数のページから成る web サイトの管理の課題の 1 つは、サイトをナビゲートする訪問者の簡単な方法を提供しています。 まず始めに、サイトのナビゲーション構造を定義する必要があります。 次に、この構造体は、階層リンク メニューなどのナビゲート可能なユーザー インターフェイス要素に変換する必要があります。 最後に、このプロセス全体は、メンテナンスおよび更新と削除、既存のサイトに新しいページを追加する必要があります。 ASP.NET 2.0 では、前に開発者はそれを維持し、ナビゲート可能なユーザー インターフェイス要素に変換に独自のサイトのナビゲーション構造を作成する方法でした。 ASP.NET 2.0 でただし、開発者を利用できます非常に柔軟なサイト ナビゲーション システムに組み込まれています。

ASP.NET 2.0 のサイト ナビゲーション システムは、サイト マップを定義して、プログラムによる API を使用してこの情報にアクセスし、開発者のための手段を提供します。 ASP.NET は、特定の方法で書式設定された XML ファイルに格納されるサイト マップ データが必要とするサイト マップ プロバイダーに同梱されています。 サイト ナビゲーション システムがに構築されているため、[プロバイダー モデル](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)サイト マップ情報をシリアル化するための別の方法をサポートするように拡張できます。 Jeff Prosise の記事では、 [SQL サイト マップ プロバイダーをしたされて待機 For](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)どのサイト マップ プロバイダーを作成する SQL Server データベースにサイト マップを格納する; を作成することも示しています[サイト マップ プロバイダーに基づいてファイル システム構造](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)します。

このチュートリアルでは、ただし、みましょうを使用して、付属する既定のサイト マップ プロバイダー ASP.NET 2.0 を使用します。 サイト マップを作成するに単にソリューション エクスプ ローラーでプロジェクト名を右クリックし、新しい項目の追加 を選択およびサイト マップのオプションを選択します。 として名前をそのまま`Web.sitemap`追加 ボタンをクリックします。


[![サイト マップをプロジェクトに追加します。](master-pages-and-site-navigation-vb/_static/image22.png)](master-pages-and-site-navigation-vb/_static/image21.png)

**図 9**: プロジェクトへのサイト マップの追加 ([フルサイズの画像を表示する をクリックします](master-pages-and-site-navigation-vb/_static/image23.png))。


サイト マップ ファイルは、XML ファイルです。 Visual Studio が、サイト マップ構造体の IntelliSense を提供することに注意してください。 サイト マップ ファイルが必要、`<siteMap>`正確に 1 つ含める必要がありますルート ノードとしてノード`<siteMapNode>`子要素。 まず`<siteMapNode>`要素は、任意の数の子孫を含めることができますし、`<siteMapNode>`要素。

ファイル システムの構造を模倣するために、サイト マップを定義します。 つまり、追加、`<siteMapNode>`の 3 つのフォルダーと子の各要素`<siteMapNode>`などのため、これらのフォルダー内の ASP.NET ページの各要素。

Web.sitemap


[!code-xml[Main](master-pages-and-site-navigation-vb/samples/sample4.xml)]

サイト マップは、サイトのさまざまなセクションについて説明している階層、web サイトのナビゲーション構造を定義します。 各`<siteMapNode>`要素`Web.sitemap`サイトのナビゲーション構造でのセクションを表します。


[![階層ナビゲーション構造を表すサイト マップ](master-pages-and-site-navigation-vb/_static/image25.png)](master-pages-and-site-navigation-vb/_static/image24.png)

**図 10**: 階層ナビゲーション構造を表すサイト マップ ([フルサイズの画像を表示する をクリックします](master-pages-and-site-navigation-vb/_static/image26.png))。


ASP.NET、.NET Framework のサイト マップの構造を公開する[サイトマップ クラス](https://msdn.microsoft.com/library/system.web.sitemap.aspx)します。 このクラスには、`CurrentNode`プロパティで、ユーザーが現在アクセスは、セクションに関する情報を返します、`RootNode`プロパティは、サイト マップのルートを返します (ホームで、サイト マップ) します。 両方、`CurrentNode`と`RootNode`プロパティの戻り値[SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx)プロパティのインスタンスのような`ParentNode`、 `ChildNodes`、 `NextSibling`、`PreviousSibling`でのサイト マップを使用できます。処理する階層。

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>手順 3: サイト マップに基づくメニューを表示します。

ASP.NET 2.0 でのデータへのアクセスは、asp.net などのプログラムによって行われますが、1.x では、宣言は、新しいまたは[データ ソース コントロール](https://msdn.microsoft.com/library/ms227679.aspx)します。 ObjectDataSource コントロール、クラス、およびその他のユーザーからのデータにアクセスするため、リレーショナル データベースのデータにアクセスするため、SqlDataSource コントロールなどのいくつかの組み込みのデータ ソース コントロールがあります。 独自に作成することも[カスタム データ ソース コントロール](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp)します。

データ ソース コントロールは、ASP.NET ページと基になるデータ間のプロキシとして機能します。 データ ソース コントロールの取得したデータを表示するために通常のページに別の Web コントロールを追加し、データ ソース コントロールにバインドします。 Web コントロールをデータ ソース コントロールをバインドする Web コントロールの設定`DataSourceID`プロパティのデータ ソース コントロールの値を`ID`プロパティ。

サイト マップのデータを操作するために役立てるため、ASP.NET には、SiteMapDataSource コントロールには、web サイトのサイト マップに対して Web コントロールをバインドすることが含まれています。 2 つの Web コントロール、ツリー ビューとメニューは、ナビゲーション ユーザー インターフェイスを提供するよく使用されます。 これら 2 つのコントロールのいずれかにサイト マップ データをバインドする、SiteMapDataSource TreeView と共にページに追加またはいるメニュー コントロール`DataSourceID`プロパティが適宜設定されます。 たとえば、次のマークアップを使用して、マスター ページに、メニュー コントロールを追加しますでした。


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample5.aspx)]

Repeater コントロール、SiteMapDataSource コントロールをバインドします細かく、生成される HTML の制御では、次のようにします。


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample6.aspx)]

SiteMapDataSource コントロールはルート サイト マップ ノード以降の時に、サイト マップの 1 つの階層レベルを返します (ホーム、弊社サイト マップ内)、[次へ] レベル (基本レポート、レポートのフィルター処理と書式設定をカスタマイズ)、という具合です。 SiteMapDataSource を Repeater にバインドするとき、最初のレベルを列挙します。 返されると、インスタンス化、`ItemTemplate`各`SiteMapNode`最初のレベルのインスタンス。 特定のプロパティにアクセスする、`SiteMapNode`を使用して`Eval(propertyName)`をそれぞれ取得する方法は`SiteMapNode`の`Url`と`Title`ハイパーリンク コントロールのプロパティ。

上記の例で Repeater は、次のマークアップでレンダリングされます。


[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample7.html)]

(基本的なレポート、レポートのフィルター処理、および書式設定のカスタマイズ) これらのサイト マップ ノードの構成、 *2 番目*最初、表示されるサイト マップのレベル。 これは、SiteMapDataSource の`ShowStartingNode`プロパティが False に設定が原因で、SiteMapDataSource をルートのサイト マップ ノードをバイパスし、代わりに、サイト マップの階層内の 2 番目のレベルを返すことから始めます。

基本レポート、レポートのフィルター処理、および書式設定のカスタマイズされた子を表示する`SiteMapNode`s に初期 Repeater の別の Repeater を追加できます`ItemTemplate`します。 この 2 つ目の Repeater にバインドされます、`SiteMapNode`インスタンスの`ChildNodes`プロパティでは、次のようにします。


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample8.aspx)]

これら 2 つのリピータは結果 (いくつかのマークアップが簡潔にするために削除された) 次のマークアップになります。


[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample9.html)]

選択した CSS を使用してスタイル設定から[Rachel Andrew](http://www.rachelandrew.co.uk/)予約 's [、CSS 禅僧: 101 の重要なヒント、テクニック、&amp;ハック](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance)、`<ul>`と`<li>`要素のスタイルを設定するようマークアップでは、次のビジュアルの出力が生成されます。


![2 つのリピータと CSS から構成メニュー](master-pages-and-site-navigation-vb/_static/image27.png)

**図 11**: メニューは、2 つのリピータといくつかの CSS から構成されます。


このメニューは、マスター ページとで定義されているサイト マップにバインドされている`Web.sitemap`つまりページを使用するすべてのサイト マップの変更がすぐに反映されますを`Site.master`マスター ページ。

## <a name="disabling-viewstate"></a>ViewState を無効にします。

すべての ASP.NET コントロールの状態を永続化できる必要に応じて、[ビューステート](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/)がレンダリングされた html フォームの非表示フィールドとしてシリアル化先。 ビュー ステートは、データ Web コントロールにバインドされているデータなど、ポストバック間で、プログラムで変更の状態を記憶するコントロールによって使用されます。 ビュー ステートは、ポストバック間で記憶しておくに情報を許可するときに、クライアントに送信する必要があり、重大なページが肥大化する可能性がない場合厳重に監視するためのマークアップのサイズが増加します。 データ Web コントロール、GridView では特には特に何十もの追加のキロバイトのマークアップをページに追加することで有名です。 このような増加は、ブロード バンドまたはイントラネットのユーザーのごくわずかであり、ビュー ステートはダイヤルアップ ユーザーのラウンド トリップを数秒を追加できます。

状態を表示、ブラウザーでページを参照してください、および web ページによって送信されたソースを表示し、影響を確認する (Internet Explorer で、[表示] メニューに移動し、ソース オプションを選択)。 にすることもできます。[ページ トレース](https://msdn.microsoft.com/library/sfbfw58f.aspx)を各ページ上のコントロールで使用される割り当ての表示状態を確認します。 という名前の隠しフォーム フィールドで、ビュー状態情報がシリアル化`__VIEWSTATE`にある、`<div>`要素の開始後すぐに`<form>`タグ。 ビュー ステートは、使用されている Web フォームがある場合にのみ保存します。ASP.NET ページが含まれていない場合、`<form runat="server">`がその宣言構文内で、`__VIEWSTATE`隠しフォーム フィールドで、表示されるマークアップ。

`__VIEWSTATE`マスター ページによって生成されたフォーム フィールドは、ページの生成されたマークアップを約 1,800 バイトを追加します。 この余分な肥大化は主に、Repeater コントロール、SiteMapDataSource コントロールの内容が状態を表示する永続化します。 余分な 1,800 バイト数が多くのフィールドとレコードを GridView を使用する場合、ワクワクさせる多くのように見えない可能性があります、中にビュー ステートが 10 個以上の倍数で swell 簡単にできます。

ビュー ステートはページまたはコントロールのレベルで設定して無効にすることができます、`EnableViewState`プロパティを`False`のため、出力されるマークアップのサイズを小さきます。 データ Web コントロールがポストバック間でデータ Web コントロールにバインドされたデータを永続化のビュー ステートから Web コントロールのデータのビュー ステートを無効にするときにデータは、それぞれのポストバック時にバインドする必要があります。 ASP.NET のバージョンでこの役割は、ページの開発者の肩にかかってリンゴ 1.xASP.NET 2.0 でただし、Web コントロールのデータが再バインド ポストバックごとに、データ ソースの管理に必要な場合。

Repeater コントロールの設定ページのビューステートを削減しましょう`EnableViewState`プロパティを`False`します。 これは、デザイナーまたは宣言によって、ソース ビューでは、[プロパティ] ウィンドウから実行できます。 この変更を行った後よう Repeater の宣言型マークアップになります。


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample10.aspx)]

この変更は、ページのビュー ステートのサイズが自動的に圧縮をたったのレンダリング後にビューに状態 97% の節約、52 バイトのサイズ。 チュートリアルではこのシリーズ全体でを無効にしますデータ Web コントロールのビューステート既定では、表示されるマークアップのサイズを小さくためにします。 ほとんどの例では、`EnableViewState`プロパティに設定する`False`言及せず行ってとします。 その期待される機能を提供する Web を制御するだけの時間の状態については、説明の表示はシナリオのデータの順序で有効する必要があります。

## <a name="step-4-adding-breadcrumb-navigation"></a>手順 4: 階層リンク ナビゲーションを追加します。

マスター ページを完了するには、各ページに、階層リンク ナビゲーション UI 要素を追加してみましょう。 階層リンクでは、サイト階層内の現在位置をユーザーがすぐにわかります。 ページに SiteMapPath コントロールを追加する ASP.NET 2.0 での階層リンクは簡単に追加します。コードは必要ありません。

私たちのサイトのヘッダーにこのコントロールを追加`<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample11.aspx)]

階層リンクは、ユーザーは、ルートまでサイト マップの階層とそのサイト マップ ノードの「先祖、」にアクセスして、現在のページを示します (ホームで、サイト マップ) します。


![階層リンクには、現在のページが表示されます。 とその親サイトの階層をマッピングします。](master-pages-and-site-navigation-vb/_static/image28.png)

**図 12**: 階層リンクには、現在のページが表示されますとその親サイトの階層をマッピングする。


## <a name="step-5-adding-the-default-page-for-each-section"></a>手順 5: 各セクションの既定のページを追加します。

チュートリアルでは、サイトでは、さまざまなカテゴリに基本的なレポートでは、フィルター処理、カスタムの書式設定、各カテゴリとそのフォルダー内の ASP.NET ページと対応するチュートリアル用のフォルダーとこれに分類されます。 さらに、各フォルダーが含まれています、`Default.aspx`ページ。 この既定のページの現在のセクションのチュートリアルのすべてのことを表示してみましょう。 つまりの`Default.aspx`で、`BasicReporting`フォルダーへのリンクになります`SimpleDisplay.aspx`、 `DeclarativeParams.aspx`、および`ProgrammaticParams.aspx`します。 ここでは、もう一度使える、`SiteMap`クラスとデータ サイト マップに基づいてこの情報を表示する Web コントロールで定義されている`Web.sitemap`します。

今回のチュートリアルでは、説明、タイトルを表示しますが、もう一度、Repeater を使用して、順序なしリストを表示してみましょう。 これを実行するコードとマークアップがごとに繰り返される必要があるため`Default.aspx` ページで、この UI ロジックをカプセル化ことができます、[ユーザー コントロール](https://msdn.microsoft.com/library/y6wb1a0e.aspx)します。 呼ばれる web サイトにフォルダーを作成`UserControls`という名前の Web ユーザー コントロールの種類の新しい項目を追加および`SectionLevelTutorialListing.ascx`、し、次のマークアップを追加します。


[![UserControls フォルダーに新しい Web ユーザー コントロールを追加します。](master-pages-and-site-navigation-vb/_static/image30.png)](master-pages-and-site-navigation-vb/_static/image29.png)

**図 13**: 新しい Web ユーザー コントロールを追加、`UserControls`フォルダー ([フルサイズの画像を表示する をクリックします](master-pages-and-site-navigation-vb/_static/image31.png))。


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.vb


[!code-vb[Main](master-pages-and-site-navigation-vb/samples/sample13.vb)]

Repeater の前の例ではバインドされている、`SiteMap`データ、Repeater を宣言によって、`SectionLevelTutorialListing`ユーザー コントロール、ただしはプログラムでします。 `Page_Load`イベント ハンドラーで、チェックを行うこのページの URL は、サイト マップ内のノードにマップすることを確認します。 ない、対応するページでこのユーザー コントロールを使用する場合`<siteMapNode>`エントリ、`SiteMap.CurrentNode`戻ります`Nothing`データは、Repeater にバインドしないとします。 ある場合、 `CurrentNode`、バインド、`ChildNodes`リピータするコレクション。 サイト マップが設定されているためように、 `Default.aspx`  ページの各セクションは、チュートリアルでは、そのセクション内のすべての親ノードには、以下のスクリーン ショットに示すように、このコードをへのリンクと、セクションのチュートリアルでは、すべての説明が表示されます。

この Repeater を作成すると、開く、`Default.aspx`の各、フォルダー内のページが、デザイン ビューに移動し、ドラッグ ユーザー コントロールをデザイン画面に ソリューション エクスプ ローラーからチュートリアルの一覧を表示します。


[![ユーザー コントロールが Default.aspx に追加されています](master-pages-and-site-navigation-vb/_static/image33.png)](master-pages-and-site-navigation-vb/_static/image32.png)

**図 14**: ユーザー コントロールに追加された`Default.aspx`([フルサイズの画像を表示する をクリックします](master-pages-and-site-navigation-vb/_static/image34.png))。


[![基本的なレポート作成のチュートリアルの一覧が表示されます。](master-pages-and-site-navigation-vb/_static/image36.png)](master-pages-and-site-navigation-vb/_static/image35.png)

**図 15**:、基本的なレポート作成のチュートリアルの一覧が表示されます ([フルサイズの画像を表示する をクリックします](master-pages-and-site-navigation-vb/_static/image37.png))。


## <a name="summary"></a>まとめ

サイト マップの定義と完全なマスター ページ、ある一貫したページ レイアウトとナビゲーション スキームのデータに関連するチュートリアル。 私たちのサイトに追加のページ数に関係なくこの情報を集中管理されているために、迅速かつ簡単なプロセスは、サイト全体のページ レイアウトまたはサイトのナビゲーション情報を更新します。 マスター ページでページのレイアウト情報が定義されている具体的には、`Site.master`とのマップにサイト`Web.sitemap`します。 記述する必要はなく*任意*このサイト全体のページ レイアウトとナビゲーション メカニズムを実現するためにコードし、Visual Studio での完全な WYSIWYG デザイナー サポートを保持します。

データ アクセス層とビジネス ロジック層を実施することで定義されている一貫したページ レイアウトとサイト ナビゲーションが発生しました準備ができているレポートの一般的なパターンの調査を開始するとします。 次の 3 つのチュートリアルでは、GridView、DetailsView、FormView コントロールで BLL から取得されたデータを表示するレポートの基本的なタスクを紹介します。

満足のプログラミングです。

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET マスター ページの概要](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [ASP.NET 2.0 のマスター ページ](http://odetocode.com/Articles/419.aspx)
- [ASP.NET 2.0 のデザイン テンプレート](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [ASP.NET サイト ナビゲーションの概要](https://msdn.microsoft.com/library/e468hxky.aspx)
- [サイトのナビゲーションでの ASP.NET 2.0 の確認](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [ASP.NET 2.0 のサイト ナビゲーション機能](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [ASP.NET ビュー ステートの理解](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [方法: ASP.NET ページのトレースを有効にします。](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [ASP.NET ユーザー コントロール](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Liz Shulok、Dennis Patterson、および Hilton Giesenow でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](creating-a-business-logic-layer-vb.md)
