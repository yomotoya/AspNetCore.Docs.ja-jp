---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
title: マスター ページとサイトのナビゲーション (VB) |Microsoft ドキュメント
author: rick-anderson
description: わかりやすい web サイトの 1 つの一般的な特性は、一貫性のある、サイト全体のページ レイアウトとナビゲーション スキームがあることです。 このチュートリアルの検索方法で y しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 022801d8-a327-4d0c-8780-6094c9cee00d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
msc.type: authoredcontent
ms.openlocfilehash: 45fdbf70f7981c0faefef2603d21f913022e2a8f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887562"
---
<a name="master-pages-and-site-navigation-vb"></a>マスター ページとサイトのナビゲーション (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_3_VB.exe)または[PDF のダウンロード](master-pages-and-site-navigation-vb/_static/datatutorial03vb1.pdf)

> わかりやすい web サイトの 1 つの一般的な特性は、一貫性のある、サイト全体のページ レイアウトとナビゲーション スキームがあることです。 このチュートリアルは、簡単に更新可能なすべてのページにわたって一貫したルック アンド フィールを作成する方法で検索します。


## <a name="introduction"></a>はじめに

わかりやすい web サイトの 1 つの一般的な特性は、一貫性のある、サイト全体のページ レイアウトとナビゲーション スキームがあることです。 ASP.NET 2.0 には両方のサイト全体のページ レイアウトとナビゲーション スキームを実装することを大幅に簡略化する 2 つの新しい機能が導入されています。 マスター ページとサイトのナビゲーションです。 指定した編集可能な領域を持つサイトのテンプレートを作成する開発者向けのマスター ページを使用します。 このテンプレートは、サイト内の ASP.NET ページに適用できます。 このような ASP.NET ページのみ指定する必要はコンテンツの編集可能な領域で指定されたマスター ページ、マスター ページで、その他のすべてのマークアップが、マスター ページを使用するすべての ASP.NET ページ間で同じです。 このモデルを定義して、簡単に更新可能なすべてのページにわたって一貫したルック アンド フィールを作成するを簡単に、サイト全体のページ レイアウトを集中管理をすることができます。

[サイト ナビゲーション システム](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)機構を開発者に、サイト マップを定義するページとプログラムでクエリを実行するには、そのサイト マップの API の両方を提供します。 メニューの TreeView、およびサイト マップ パス簡単に新しいナビゲーション Web コントロールでは、一般的なナビゲーション ユーザー インターフェイス要素内のサイト マップのすべてまたは一部をレンダリングします。 使用する、既定のサイト ナビゲーション プロバイダー、マイクロソフトのサイト マップが、XML 形式のファイルで定義されることを意味します。

これらの概念を示すため、このチュートリアルの web サイトをより使いやすくにするには、サイト全体のページ レイアウトを定義する、サイト マップを実装および UI のナビゲーションを追加することは、このレッスンをについて説明します。 このチュートリアルの最後で、チュートリアルの web ページを構築するための洗練された web サイト デザインをしました。


[![このチュートリアルの最終結果](master-pages-and-site-navigation-vb/_static/image2.png)](master-pages-and-site-navigation-vb/_static/image1.png)

**図 1**: このチュートリアルの「終了結果 ([フルサイズのイメージを表示するには、をクリックして](master-pages-and-site-navigation-vb/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>手順 1: マスター ページの作成

最初の手順では、サイトのマスター ページを作成します。 現在、当社の web サイトは、型指定されたデータセットのみで構成されます (`Northwind.xsd`で、`App_Code`フォルダー)、BLL クラス (`ProductsBLL.vb`、`CategoriesBLL.vb`など、すべて、`App_Code`フォルダー)、データベース (`NORTHWND.MDF`で、 `App_Data`フォルダー)、構成ファイル (`Web.config`)、および CSS スタイル シート ファイル (`Styles.css`)。 ページとを使用して、DAL BLL 最初の 2 つのチュートリアルからおがするを再調査するこれらの例より詳細に今後のチュートリアルで後を示すファイルはクリーンアップします。


![このプロジェクト内のファイル](master-pages-and-site-navigation-vb/_static/image4.png)

**図 2**: プロジェクト内のファイル


マスター ページを作成するには、ソリューション エクスプ ローラーでプロジェクト名を右クリックし、新しい項目の追加を選択します。 テンプレートの一覧から、マスター ページの種類を選択し、名前を付けます`Site.master`です。


[![新しいマスター ページを web サイトに追加します。](master-pages-and-site-navigation-vb/_static/image6.png)](master-pages-and-site-navigation-vb/_static/image5.png)

**図 3**: 新しいマスター ページ、web サイトに追加 ([フルサイズのイメージを表示するをクリックして](master-pages-and-site-navigation-vb/_static/image7.png))


マスター ページで、ここで、サイト全体のページ レイアウトを定義します。 デザイン ビューを使用して、必要なレイアウトまたは Web コントロールを追加または、ソース ビューで手動でマークアップを手動で追加することができます。 マスター ページで使用して、[カスケード スタイル シート](http://www.w3schools.com/css/default.asp)の配置と外部のファイルで定義された CSS 設定でスタイル`Style.css`です。 CSS 規則が定義されているマークアップを次に示すのかを判断できないときにするよう、ナビゲーション`<div>`の左側に表示し、200 ピクセルの固定幅ができるように、コンテンツが絶対位置に配置します。

Site.master


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample1.aspx)]

マスター ページは、静的ページ レイアウトやマスター ページを使用する ASP.NET ページが編集可能な領域の両方を定義します。 これらのコンテンツの編集可能な領域は、プレース ホルダー コントロール、コンテンツを確認することができますで示されます`<div>`です。 マスター ページが 1 つ ContentPlaceHolder (`MainContent`) をマスター ページが複数 contentplaceholders にあります。

上記で入力したマークアップを含む、デザイン ビューに切り替えると、マスター ページのレイアウトを示します。 このマスター ページを使用するすべての ASP.NET ページのマークアップを指定することでこの統一されたレイアウトになります、`MainContent`領域。


[![デザイン ビューで表示したときに、マスター ページ](master-pages-and-site-navigation-vb/_static/image9.png)](master-pages-and-site-navigation-vb/_static/image8.png)

**図 4**:、マスター ページ、ときに表示をデザイン ビュー ([フルサイズのイメージを表示するをクリックして](master-pages-and-site-navigation-vb/_static/image10.png))


## <a name="step-2-adding-a-homepage-to-the-website"></a>手順 2: web サイトに、ホーム ページを追加します。

定義されているマスター ページ、web サイトの ASP.NET ページを追加する準備ができました。 追加することによって開始しましょう`Default.aspx`、当社の web サイトのホーム ページです。 ソリューション エクスプ ローラーでプロジェクト名を右クリックし、新しい項目の追加を選択します。 ファイルの名前とテンプレートの一覧から Web フォーム オプションを選んで`Default.aspx`です。 また、「マスター ページを選択」チェック ボックスを確認します。


[![マスター ページを選択 チェック ボックスの確認、新しい Web フォームを追加します。](master-pages-and-site-navigation-vb/_static/image12.png)](master-pages-and-site-navigation-vb/_static/image11.png)

**図 5**: マスター ページを選択 チェック ボックスの確認、新しい Web フォームに追加 ([フルサイズのイメージを表示するをクリックして](master-pages-and-site-navigation-vb/_static/image13.png))


[Ok] ボタンをクリックすると、この新規の ASP.NET ページを使用する必要がありますマスター ページの選択を求めます。 複数のマスター ページを保持するには、プロジェクトで、1 つだけがあります。


[![この ASP.NET ページを使用する必要があります、マスター ページの選択します。](master-pages-and-site-navigation-vb/_static/image15.png)](master-pages-and-site-navigation-vb/_static/image14.png)

**図 6**: この ASP.NET ページする必要がありますを使用してマスター ページを選択 ([フルサイズのイメージを表示するをクリックして](master-pages-and-site-navigation-vb/_static/image16.png))


新規の ASP.NET ページでは、マスター ページを選択した後、次のマークアップが含まれます。

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample2.aspx)]

`@Page`ディレクティブがありますが使用されるマスター ページ ファイルへの参照を (`MasterPageFile="~/Site.master"`)、ASP.NET ページのマークアップには、各コントロールので、マスター ページで定義されているプレース ホルダー コントロールのコンテンツ コントロールが含まれていますと`ContentPlaceHolderID`特定 ContentPlaceHolder へのコンテンツのマッピングを制御します。 コンテンツ コントロールは、マークアップを配置した場所に対応する ContentPlaceHolder で表示します。 設定、`@Page`ディレクティブの`Title`ホームに属性を歓迎のコンテンツをコンテンツ コントロールに追加します。

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample3.aspx)]

`Title`属性、`@Page`ディレクティブを使用していても、ASP.NET ページから、ページのタイトルを設定すること、`<title>`マスター ページの要素が定義されています。 私たちもタイトル プログラムでは、使用して設定できます`Page.Title`です。 またを注意してください。 スタイル シートへのマスター ページの参照 (など`Style.css`) マスター ページから相対的に、ASP.NET ページがどのようなディレクトリに関係なく、任意の ASP.NET ページで動作させることが自動的に更新します。

ブラウザーで、ページがどのように表示されるかが分かりますデザイン ビューに切り替えます。 なお、設計で非 ContentPlaceHolder マークアップをマスター ページで定義されているコンテンツの編集可能な領域のみが編集可能である ASP.NET ページの表示は淡色表示されます。


[![ASP.NET ページのデザイン ビューは、編集可能な編集不可の両方の領域を示しています。](master-pages-and-site-navigation-vb/_static/image18.png)](master-pages-and-site-navigation-vb/_static/image17.png)

**図 7**: 非編集可能領域と、ASP.NET ページを示しています両方、編集可能、デザイン ビュー ([フルサイズのイメージを表示するをクリックして](master-pages-and-site-navigation-vb/_static/image19.png))


ときに、`Default.aspx`ページがブラウザーで表示した、ページのマスター ページのコンテンツと、ASP、ASP.NET エンジンが自動的にマージします。NET はのコンテンツ、および要求側のブラウザーに送信される最終的な HTML に結合された内容を表示します。 マスター ページのコンテンツが更新されると、このマスター ページを使用するすべての ASP.NET ページには、次回要求されたコンテンツは新しいマスター ページと remerged、コンテンツがあります。 つまり、マスター ページのモデルでは、1 つのページ (マスター ページ) があるレイアウト テンプレートに定義されているサイト全体にわたって変更が直ちに反映されます。

## <a name="adding-additional-aspnet-pages-to-the-website"></a>その他の ASP.NET ページ、web サイトへの追加

サイトに追加するその他の ASP.NET ページのスタブ、さまざまなレポートのデモを保持する最終的にはしばらくを見てみましょう。 ある詳細みましょうのすべてのスタブ ページを作成するのではなくという時ための合計、35 デモは、最初のいくつかを作成します。 デモの多くのカテゴリもある、ので効率よく管理するデモ フォルダーに追加のカテゴリの。 ここでは、次の 3 つのフォルダーを追加します。

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

ソリューション エクスプ ローラーで、図 8 に示すように最後に、新しいファイルを追加します。 各ファイルを追加する場合は、「マスター ページを選択」チェック ボックスをオンにしてください。


![次のファイルを追加します。](master-pages-and-site-navigation-vb/_static/image20.png)

**図 8**: 次のファイルを追加


## <a name="step-2-creating-a-site-map"></a>手順 2: サイト マップを作成します。

Web サイトが、いくつかのページの複数の構成を管理するための課題の 1 つは、サイト内を移動への訪問者の簡単な方法を提供しています。 まず始めに、サイトのナビゲーション構造を定義する必要があります。 次に、この構造体は、メニューまたはパンくずリストなど、ナビゲート可能なユーザー インターフェイス要素に変換する必要があります。 最後に、このプロセス全体は、管理、サイトと既存のテーブルを削除する新しいページが追加されるとを更新する必要があります。 ASP.NET 2.0 では、前に開発者は、それを維持し、ナビゲート可能なユーザー インターフェイス要素に変換する、サイトのナビゲーションの構造を作成する、独自の方法でした。 ASP.NET 2.0 では、ただし、開発者を利用できます、非常に柔軟なナビゲーションのサイト システムに組み込まれています。

ASP.NET 2.0 サイト ナビゲーション システムでは、サイト マップを定義して、プログラムによる API を使用し、この情報にアクセスする開発者のための手段を提供します。 ASP.NET は、サイト マップ データを特定の方法で書式設定された XML ファイルに格納されるものと想定する、サイト マップ プロバイダーに付属します。 ただし、サイトのナビゲーション システムが組み込まれているため、[プロバイダー モデル](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)サイト マップ情報をシリアル化するための代替方法をサポートするために拡張することができます。 Jeff Prosise 記事[、SQL サイト マップ プロバイダーする 've されて待機中の](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)サイト マップ プロバイダーを作成する SQL Server データベースにサイト マップを格納する; を作成することもできる方法を示しています[サイト マップ プロバイダーに基づいてファイル システム構造](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)です。

このチュートリアルでは、ただし、みましょうを使用して付属する既定のサイト マップ プロバイダー ASP.NET 2.0 を使用します。 サイト マップを作成するには、だけで、ソリューション エクスプ ローラーでプロジェクト名を右クリックし、新しい項目の追加 を選択およびサイト マップ オプションを選択します。 として名前をそのまま`Web.sitemap`追加 をクリックします。


[![サイト マップをプロジェクトに追加します。](master-pages-and-site-navigation-vb/_static/image22.png)](master-pages-and-site-navigation-vb/_static/image21.png)

**図 9**: プロジェクトへのサイト マップの追加 ([フルサイズのイメージを表示するをクリックして](master-pages-and-site-navigation-vb/_static/image23.png))


サイト マップ ファイルは、XML ファイルです。 Visual Studio が、サイト マップ構造体の IntelliSense を提供することに注意してください。 サイト マップ ファイルが必要、`<siteMap>`正確に 1 つ含める必要がありますルート ノードとしてノード`<siteMapNode>`子要素です。 最初に`<siteMapNode>`要素は、任意の数の子孫を含めることができますし、`<siteMapNode>`要素。

ファイル システムの構造を模倣するために、サイト マップを定義します。 つまり、追加、 `<siteMapNode>` 、3 つのフォルダーと子の各要素`<siteMapNode>`ようそれらのフォルダー内の ASP.NET ページの各要素。

Web.sitemap


[!code-xml[Main](master-pages-and-site-navigation-vb/samples/sample4.xml)]

サイト マップでは、サイトのさまざまなセクションについて説明している階層、web サイトのナビゲーション構造を定義します。 各`<siteMapNode>`内の要素`Web.sitemap`サイトのナビゲーション構造体のセクションを表します。


[![サイト マップが階層ナビゲーション構造体を表します](master-pages-and-site-navigation-vb/_static/image25.png)](master-pages-and-site-navigation-vb/_static/image24.png)

**図 10**: サイト マップが階層ナビゲーション構造体を表します ([フルサイズのイメージを表示するをクリックして](master-pages-and-site-navigation-vb/_static/image26.png))


ASP.NET、.NET Framework のによってサイト マップの構造を公開する[SiteMap クラス](https://msdn.microsoft.com/library/system.web.sitemap.aspx)です。 このクラスには、`CurrentNode`プロパティで、ユーザーがアクセスした現在;、セクションに関する情報を返します、`RootNode`プロパティは、サイト マップのルートを返します (自宅、マイクロソフトのサイト マップに)。 両方、`CurrentNode`と`RootNode`プロパティの戻り値[SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx)インスタンスで、プロパティを持つように`ParentNode`、 `ChildNodes`、 `NextSibling`、`PreviousSibling`など、サイト マップ可能にします。処理する階層。

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>手順 3: サイト マップに基づくメニューを表示します。

ASP.NET 2.0 のデータにアクセスできる asp.net などのプログラムで行われますが、1.x では、または宣言は、新しい[データ ソース コントロール](https://msdn.microsoft.com/library/ms227679.aspx)です。 リレーショナル データベースのデータ、クラス、およびその他のユーザーからのデータにアクセスするため、ObjectDataSource コントロールにアクセスするため、SqlDataSource コントロールなどのいくつかの組み込みのデータ ソース コントロールがあります。 独自に作成することも[カスタム データ ソース コントロール](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp)です。

データ ソース コントロールは、ASP.NET ページと、基になるデータ間のプロキシとして機能します。 データ ソース コントロールの取得したデータを表示するためにあります通常別の Web コントロールをページに追加し、データ ソース コントロールにバインドします。 Web コントロールをデータ ソース コントロールをバインドする単に設定、Web コントロールの`DataSourceID`プロパティのデータ ソース コントロールの値を`ID`プロパティです。

サイト マップのデータを操作するために役立てるため、ASP.NET には、web サイトのサイト マップに対する Web コントロールをバインドすることが可能 SiteMapDataSource コントロールが含まれています。 2 つの Web コントロール、TreeView やメニューは、ナビゲーションのユーザー インターフェイスを提供するよく使用されます。 これら 2 つのコントロールのいずれかにサイト マップ データをバインドする、TreeView と共にページに、SiteMapDataSource を単純に追加またはいるメニュー コントロール`DataSourceID`プロパティが適宜設定されます。 たとえば、メニュー コントロールを追加すると、次のマークアップを使用して、マスター ページをでしたお。


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample5.aspx)]

出力された HTML が細かく制御細かく、バインドできる SiteMapDataSource コントロール Repeater コントロール次のようにします。


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample6.aspx)]

SiteMapDataSource コントロールはルート サイト マップ ノードから始まるは時に、サイト マップの 1 つの階層レベルを返します (自宅、マイクロソフトのサイト マップに)、し、次のレベル (基本レポート、レポートのフィルター処理、およびカスタマイズされた書式設定)、およびなです。 最初のレベルを列挙しますリピータを SiteMapDataSource をバインドするときに返され、インスタンス化、`ItemTemplate`各`SiteMapNode`最初のレベルのインスタンス。 特定のプロパティにアクセスする、`SiteMapNode`を使用して`Eval(propertyName)`、これは、取得する方法の各`SiteMapNode`の`Url`と`Title`ハイパーリンク コントロールのプロパティです。

上記のリピータ例は、次のマークアップにレンダリングされます。


[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample7.html)]

(基本レポート、レポートのフィルター処理、および書式設定のカスタマイズ) これらのサイト マップ ノードを構成する、 *2 番目*最初、表示されるサイト マップのレベルです。 これは、ため SiteMapDataSource の`ShowStartingNode`プロパティをルート サイト マップ ノードをバイパスし、サイト マップ階層内の 2 番目のレベルを返す代わりにまず SiteMapDataSource の原因を False に設定します。

基本レポート、レポートのフィルター処理、およびカスタマイズされた書式指定子を表示する`SiteMapNode`s、別のリピータ初期リピータに追加できる`ItemTemplate`です。 この 2 つ目のリピータにバインドされます、`SiteMapNode`インスタンスの`ChildNodes`プロパティ、次のようにします。


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample8.aspx)]

これら 2 つのリピータ (一部のマークアップが簡略化のため削除された) 次のマークアップで生成されます。


[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample9.html)]

選択した CSS を使用するスタイルから[Rachel Andrew](http://www.rachelandrew.co.uk/)book's [、CSS 禅僧: 101 に関する重要なヒント、テクニック、&amp;ハック](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance)、`<ul>`と`<li>`要素のスタイルを設定するようマークアップでは、次の visual の出力が生成されます。


![2 つのリピータといくつかの CSS から構成されるメニュー](master-pages-and-site-navigation-vb/_static/image27.png)

**図 11**: メニューは、次の 2 つのリピータといくつかの CSS から構成されます。


このメニューはで定義されているサイト マップにバインドして、マスター ページでは`Web.sitemap`つまりページを使用するすべてのサイト マップへの変更を即時に反映すること、`Site.master`マスター ページ。

## <a name="disabling-viewstate"></a>ViewState を無効にします。

すべての ASP.NET コントロールがその状態を永続化できる必要に応じて、[ビューステート](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/)、レンダリングされる HTML 非表示のフォームのフィールドとしてシリアル化されます。 ビュー ステート コントロールが使用ポストバック間で、プログラムで変更の状態を保存するデータ Web コントロールにバインドされているデータなどです。 ビュー ステートをポストバック間で保存する情報を許可するときに、クライアントに送信する必要がありますと重大なページの肥大化につながる可能性がない場合を綿密に監視するマークアップのサイズを増やします。 GridView 特に Web コントロールのデータは、マークアップの余分なキロバイト数十をページに追加するため、特によく知られたです。 このような増加は、ブロード バンドまたはイントラネットのユーザーのごくわずかであり、ビュー ステートは、ダイヤルアップ ユーザーのラウンド トリップに数秒を追加できます。

状態を表示、ブラウザーでページを参照してくださいおよび web ページによって送信されたソースを表示する影響を確認する (Internet Explorer で、[表示] メニューに移動し、ソース オプションを選択) します。 にすることもできます。[ページ トレース](https://msdn.microsoft.com/library/sfbfw58f.aspx)を各ページ上のコントロールで使用される割り当ての表示状態を確認します。 状態情報の表示がという名前の隠しフォーム フィールドのシリアル化される`__VIEWSTATE`内にある、`<div>`要素の開始後すぐに`<form>`タグ。 使用されている Web フォームがある場合にのみ、ビュー状態が保存します。ASP.NET ページが含まれていない場合、`<form runat="server">`は表示されませんその宣言の構文で、`__VIEWSTATE`表示されるマークアップ内の非表示のフォーム フィールドです。

`__VIEWSTATE`マスター ページによって生成されたフォーム フィールドがページの生成されたマークアップに約 1,800 バイトを追加します。 この余分な膨張期限は、主にリピータ コントロールように状態を表示する SiteMapDataSource コントロールの内容が保存されます。 余分な 1,800 バイトがあまり多くのフィールドとレコードを GridView を使用する場合に興奮しており、取得するように見えない可能性があります、中にビュー ステートが 10 以上の要素によって swell 簡単にことができます。

ビュー ステートはページまたはコントロールのレベルを設定して無効にすることができます、`EnableViewState`プロパティを`False`、それによって、表示されるマークアップのサイズを小さきます。 Web コントロールがポストバック間でデータ Web コントロールにバインドされたデータを保持するデータのビュー ステートを以降 Web コントロールのデータのビュー ステートを無効にするときにデータは、それぞれのポストバック時にバインドする必要があります。 ASP.NET のバージョンでこの責任は、ページの開発の肩が 1.xASP.NET 2.0 では、ただし、Web コントロールのデータが再バインド ポストバックのたびに、データ ソースの管理に必要な場合。

Repeater コントロールを設定しましょうページのビュー ステートを削減する`EnableViewState`プロパティを`False`です。 これは、デザイナーで、または宣言によって、ソース ビューでは、[プロパティ] ウィンドウから実行できます。 この変更を行った後リピータの宣言型マークアップようになります。


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample10.aspx)]

この変更は、ページの状態のサイズが自動的に圧縮たったにビューの表示後に 97% 削減、52 バイト ビューステートへのサイズです。 この系列全体でのチュートリアルでを無効にし Web コントロールのデータのビュー ステート既定では、表示されるマークアップのサイズを小さくためにします。 例のほとんどでは、`EnableViewState`プロパティ設定されます`False`記載せず行ってとします。 だけの時間ビュー状態については、説明が、データの順序で有効する必要がありますのシナリオでは Web は、その必要な機能を提供する制御します。

## <a name="step-4-adding-breadcrumb-navigation"></a>手順 4: 階層リンク バー ナビゲーションを追加します。

マスター ページを完了するには、各ページに、階層リンク ナビゲーション UI 要素を追加してみましょう。 ある階層リンクでは、サイト階層内の現在位置のユーザーがすぐにわかります。 ASP.NET 2.0 で階層リンク バーはだけ簡単を追加すると、サイト マップ パス コントロールは、ページに追加コードは必要ありません。

ヘッダーにこのコントロールを追加のサイトの`<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample11.aspx)]

ある階層リンクを示しています、現在のページ、ユーザーが、ルートまでサイト マップ階層だけでなくそのサイト マップ ノードの「先祖、」訪問している (自宅、マイクロソフトのサイト マップに)。


![階層リンクには、現在のページが表示されます。 および、サイトでは、その先祖が階層を割り当てる](master-pages-and-site-navigation-vb/_static/image28.png)

**図 12**: 階層リンクには、現在のページが表示されますおよび、サイトでは、その先祖が階層を割り当てる。


## <a name="step-5-adding-the-default-page-for-each-section"></a>手順 5: 各セクションの既定のページを追加します。

サイトのチュートリアルは、さまざまな基本的なレポートでは、フィルター処理、カスタムの書式設定などのカテゴリそれぞれのカテゴリおよびそのフォルダー内の ASP.NET ページと対応するチュートリアル用のフォルダーに分割されます。 さらに、各フォルダーが含まれています、`Default.aspx`ページ。 この既定のページをすべて現在のセクションのチュートリアルの表示しましょう。 つまり、用、`Default.aspx`で、`BasicReporting`フォルダーへのリンクを必要が`SimpleDisplay.aspx`、 `DeclarativeParams.aspx`、および`ProgrammaticParams.aspx`です。 ここでは、もう一度、使用、`SiteMap`で定義されているクラスとデータをサイト マップに基づいてこの情報を表示する Web コントロール`Web.sitemap`です。

リピータを使用して再び、今度は、タイトルと説明のチュートリアルの表示順序なしリストを表示してみましょう。 このを実行するコードとマークアップがごとに繰り返される必要があるため`Default.aspx` ページで、おをカプセル化できるは、この UI ロジック、[ユーザー コントロール](https://msdn.microsoft.com/library/y6wb1a0e.aspx)です。 呼ばれる web サイトにフォルダーを作成`UserControls`という名前の Web ユーザー コントロールの種類の新しい項目を追加および`SectionLevelTutorialListing.ascx`、し、次のマークアップを追加します。


[![ユーザー コントロール フォルダーに新しい Web ユーザー コントロールを追加します。](master-pages-and-site-navigation-vb/_static/image30.png)](master-pages-and-site-navigation-vb/_static/image29.png)

**図 13**: する新しい Web ユーザー コントロールを追加、`UserControls`フォルダー ([フルサイズのイメージを表示するをクリックして](master-pages-and-site-navigation-vb/_static/image31.png))


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.vb


[!code-vb[Main](master-pages-and-site-navigation-vb/samples/sample13.vb)]

リピータの前の例ではバインドされている、`SiteMap`リピータへのデータ宣言以外の場合は、`SectionLevelTutorialListing`ユーザー コントロール、ただしはプログラムでします。 `Page_Load`イベント ハンドラーで、チェックを行うこのページの URL は、サイト マップ内のノードにマップすることを確認します。 このユーザー コントロールがない、対応するページで使用する場合`<siteMapNode>`エントリ、`SiteMap.CurrentNode`が返されます`Nothing`リピータにデータがバインドされずします。 ある場合、 `CurrentNode`、バインド、`ChildNodes`リピータするコレクション。 サイト マップが設定されているためになるよう、`Default.aspx`各セクション内のページのチュートリアル セクション内のすべての親ノードは、このコードは、表示へのリンクと、セクションのチュートリアルのすべての説明については以下のスクリーン ショットに示すようにします。

このリピータが作成されて、開く、`Default.aspx`デザイン ビューに移動してし、ドラッグ ユーザー コントロールをデザイン画面に、ソリューション エクスプ ローラーからチュートリアルの一覧を表示するページの各フォルダーでします。


[![ユーザー コントロールは、Default.aspx に追加されました](master-pages-and-site-navigation-vb/_static/image33.png)](master-pages-and-site-navigation-vb/_static/image32.png)

**図 14**: ユーザー コントロールに追加された`Default.aspx`([フルサイズのイメージを表示するをクリックして](master-pages-and-site-navigation-vb/_static/image34.png))


[![基本的なレポート作成のチュートリアルの一覧が表示されます。](master-pages-and-site-navigation-vb/_static/image36.png)](master-pages-and-site-navigation-vb/_static/image35.png)

**図 15**:、基本的なレポート作成のチュートリアルの一覧が表示されます ([フルサイズのイメージを表示するをクリックして](master-pages-and-site-navigation-vb/_static/image37.png))


## <a name="summary"></a>まとめ

サイト マップの定義と完全なマスター ページ、お今すぐ方式を使用して一貫したページ レイアウトとナビゲーション データに関連するチュートリアルについては、します。 サイトに追加のページ数、サイト全体のページ レイアウトやサイト ナビゲーションの情報の更新はこの情報を集中管理されているために、迅速かつ簡単なプロセスになります。 マスター ページのページのレイアウト情報が定義されている具体的には、`Site.master`とのマップにサイト`Web.sitemap`です。 書き込む必要はありませんでした*任意*このサイト全体のページ レイアウトとナビゲーション メカニズムを実現するためにコードを Visual Studio でのフル WYSIWYG デザイナー サポートを保持することです。

データ アクセス層とビジネス ロジック層が完了済み定義されている一貫したページ レイアウトとサイトのナビゲーションを持つ、私たちはレポートの一般的なパターンの調査を開始する準備ができています。 次の 3 つのチュートリアルでは、GridView、DetailsView、フォーム ビュー コントロールで BLL から取得されたデータを表示する基本的なレポート タスクを紹介します。

満足プログラミング!

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET マスター ページの概要](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [ASP.NET 2.0 のマスター ページ](http://odetocode.com/Articles/419.aspx)
- [ASP.NET 2.0 のデザインのテンプレート](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [ASP.NET サイト ナビゲーションの概要](https://msdn.microsoft.com/library/e468hxky.aspx)
- [サイト ナビゲーションの ASP.NET 2.0 を確認します。](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [ASP.NET 2.0 サイト ナビゲーション機能](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [ASP.NET の Viewstate を理解します。](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [方法: ASP.NET ページのトレースを有効にします。](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [ASP.NET ユーザー コントロール](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者には、Liz Shulok、Dennis Patterson Hilton Giesenow がされていました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](creating-a-business-logic-layer-vb.md)
