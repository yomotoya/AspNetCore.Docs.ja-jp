---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs
title: マスター ページ (c#) で、タイトル、メタ タグ、およびその他の HTML ヘッダーを指定する |Microsoft Docs
author: rick-anderson
description: さまざまな定義するためのさまざまな手法を見て&lt;ヘッド&gt;コンテンツ ページからマスター ページ内の要素。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 0aa1c84f-c9e2-4699-b009-0e28643ecbc6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 220bc0af79ea6ac5b63f51f9803a40200714642e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398574"
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-c"></a>マスター ページ (c#) で、タイトル、メタ タグ、およびその他の HTML ヘッダーを指定します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_CS.pdf)

> さまざまな定義するためのさまざまな手法を見て&lt;ヘッド&gt;コンテンツ ページからマスター ページ内の要素。


## <a name="introduction"></a>はじめに

Visual Studio 2008 で作成された新しいのマスター ページがある、既定で 2 つのプレース ホルダー コントロール: head ですとに 1 つ、`<head>`要素と 1 つの名前付き`ContentPlaceHolder1`、Web フォーム内に配置します。 目的は、 `ContentPlaceHolder1` Web フォーム ページの単位でカスタマイズ可能な領域を定義することです。 `head` ContentPlaceHolder により、カスタム コンテンツを追加するページ、`<head>`セクション。 (もちろん、これら 2 つの ContentPlaceHolders を変更または削除、およびマスター ページに追加のプレース ホルダーを追加する可能性があります。 マスターのページでは、 `Site.master`、現在 4 つのプレース ホルダー コントロールを保持します)。

HTML`<head>`要素がドキュメント自体の一部でない web ページの文書に関する情報のリポジトリとして機能します。 Web ページのタイトルなどの情報が、ファイルの検索エンジンまたは内部のクローラーや RSS フィード、JavaScript、CSS などの外部リソースへのリンクで使用されるメタ情報。 この情報の一部は、web サイトのすべてのページに関連する可能性があります。 たとえば、グローバルにすべての ASP.NET ページの JavaScript ファイルと同じの CSS 規則をインポートする可能性があります。 ただしの部分がある、`<head>`ページ固有である要素。 ページ タイトルは、典型的な例です。

このチュートリアルではグローバルと特定のページを定義する方法を調べる`<head>`とそのコンテンツ ページのマスター ページのセクションのマークアップ。

## <a name="examining-the-master-pagesheadsection"></a>マスター ページの検査`<head>`セクション

Visual Studio 2008 によって作成された既定マスター ページ ファイルには、次のマークアップにが含まれています。 その`<head>`セクション。


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample1.aspx)]

注意、`<head>`要素が含まれています、`runat="server"`属性には、サーバー コントロール (なく、静的な HTML) であることを示します。 派生してすべての ASP.NET ページ、 [ `Page`クラス](https://msdn.microsoft.com/library/system.web.ui.page.aspx)にある、`System.Web.UI`名前空間。 このクラスが含まれています、`Header`プロパティ ページへのアクセスを提供する`<head>`リージョン。 使用して、 [ `Header`プロパティ](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx)ASP.NET ページのタイトルを設定したり、表示する追加のマークアップを追加しました`<head>`セクション。 可能であれば、その後、コンテンツ ページをカスタマイズする`<head>`要素で、ページのコードを記述して`Page_Load`イベント ハンドラー。 手順 1. でページのタイトルをプログラムで設定する方法を説明します。

マークアップに示すように、`<head>`上の要素には、頭をという名前のプレース ホルダー コントロールも含まれています。 このプレース ホルダー コントロールがコンテンツ ページを追加するカスタム コンテンツと、必要に応じて、`<head>`要素プログラムを使用します。 ただし、コンテンツ ページが static のマークアップを追加する必要がある状況で便利ですが、`<head>`に対応するコンテンツ コントロールではなくプログラムによって、静的マークアップとして要素を宣言によって追加できます。

加え、`<title>`要素と head ContentPlaceHolder、マスター ページの`<head>`要素を含める必要があります`<head>`-すべてのページに共通するレベルのマークアップ。 定義された CSS ルールを使用しているすべてのページ、web サイトで、`Styles.css`ファイル。 その結果、更新、`<head>`内の要素、 [*マスター ページで、サイト全体レイアウトを作成する*](creating-a-site-wide-layout-using-master-pages-cs.md)チュートリアルでは、対応するを含める`<link>`要素。 この`Site.master`マスター ページの現在`<head>`マークアップを次に示します。


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>手順 1: コンテンツ ページのタイトルを設定します。

使用して web ページのタイトルが指定されて、`<title>`要素。 適切な値の各ページのタイトルを設定するのには重要です。 ページにアクセスして、ブラウザーのタイトル バーにそのタイトルが表示されます。 さらに、ページのブックマークを作成するときに、ブラウザーはページのタイトルをブックマークの推奨される名前として使用します。 また、多くの検索エンジンは、検索結果を表示するときに、ページのタイトルを表示します。

> [!NOTE]
> 既定では、Visual Studio の設定、 `<title>` 「無題ページ」にマスター ページ内の要素。 同様に、新しい ASP.NET ページがある、`<title>`すぎます「無題ページ」に設定します。 多くのページをページのタイトルに適切な値を設定し忘れることができる、ので「無題ページ」というタイトルのインターネット上があります。 このタイトルの web ページの Google を検索するには、約 2,460,000 結果が返されます。 Microsoft は、「無題ページ」というタイトルの web ページを発行します。 この記事の執筆時に、Google 検索には、Microsoft.com ドメインで 236 このような web ページが報告されました。


ASP.NET ページは、次の方法のいずれかでそのタイトルを指定できます。

- 内で値を直接配置することで、`<title>`要素
- 使用して、`Title`属性、`<%@ Page %>`ディレクティブ
- プログラムで設定するページの`Title`のようなコードを使用してプロパティ`Page.Title="title"`または`Page.Header.Title="title"`します。

コンテンツ ページがない、`<title>`ほどの要素がマスター ページで定義されています。 そのため、コンテンツ ページのタイトルの設定を使用するか、`<%@ Page %>`ディレクティブの`Title`属性またはプログラムによって設定します。

### <a name="setting-the-pages-title-declaratively"></a>宣言によってページのタイトルを設定

コンテンツ ページのタイトルはを通じて宣言的設定できる、`Title`の属性、 [ `<%@ Page %>`ディレクティブ](https://msdn.microsoft.com/library/ydy4x04a.aspx)します。 直接変更することでこのプロパティを設定することができます、`<%@ Page %>`ディレクティブまたは、プロパティ ウィンドウを使用します。 どちらの方法を見てみましょう。

ソース ビューを見つけ、`<%@ Page %>`ディレクティブは、宣言型マークアップをページの上部にあります。 `<%@ Page %>`ディレクティブを`Default.aspx`に従います。


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample3.aspx)]

`<%@ Page %>`ディレクティブを解析し、ページをコンパイルするときに、ASP.NET エンジンで使用されるページ固有の属性を指定します。 これには、そのマスター ページ ファイル、そのコード ファイルとそのタイトルは、他の情報の場所が含まれます。

既定で Visual Studio の設定の新しいコンテンツ ページを作成するときに、`Title`無題のページに属性します。 変更`Default.aspx`の`Title`「無題ページ」から「マスター ページのチュートリアル」への属性し、、ブラウザーでページを表示します。 図 1 は、新しいページ タイトルを反映するブラウザーのタイトル バーを示します。


![ブラウザーのタイトル バーに表示されます&quot;マスター ページのチュートリアル&quot;の代わりに&quot;無題のページ&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image1.png)

**図 01**: ブラウザーのタイトル バーに「無題ページ」ではなく「マスター ページのチュートリアル」が表示されます


ページのタイトルは、[プロパティ] ウィンドウからも設定できます。 [プロパティ] ウィンドウからドキュメントから選択ドロップダウン リストが含まれています、ロード、ページ レベルのプロパティに、`Title`プロパティ。 図 2 は、後の [プロパティ] ウィンドウを示しています。 `Title` "マスター ページのチュートリアル"に設定されています。


![すぎる [プロパティ] ウィンドウからのタイトルを構成することがあります。](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image2.png)

**図 02**: すぎる [プロパティ] ウィンドウからのタイトルを構成することがあります


### <a name="setting-the-pages-title-programmatically"></a>プログラムによるページのタイトルの設定

マスター ページの`<head runat="server">`マークアップに変換される、 [ `HtmlHead`クラス](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx)インスタンスの場合、ページは、ASP.NET エンジンによってレンダリングされます。 `HtmlHead`クラスには、 [ `Title`プロパティ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx)値が反映されて、レンダリングされたで`<title>`要素。 このプロパティを使用して、ASP.NET ページの分離コード クラスからアクセスできる`Page.Header.Title`; この同じプロパティを使用してアクセスすることも`Page.Title`します。

ページのタイトルをプログラムで設定を練習に移動し、`About.aspx`ページの分離コード クラスし、ページのイベント ハンドラーを作成`Load`イベント。 次に、ページのタイトルを設定"マスター ページのチュートリアル:: について::*日付*"ここで、*日付*は現在の日付です。 このコードを追加した後、`Page_Load`イベント ハンドラーは、次のようになります。


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample4.cs)]

図 3 にアクセスすると、ブラウザーのタイトル バーを示しています、`About.aspx`ページ。


![ページのタイトルはプログラムで設定し、現在の日付が含まれています](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image3.png)

**図 03**:、ページのタイトルはプログラムで設定し、現在の日付が含まれています


## <a name="step-2-automatically-assigning-a-page-title"></a>手順 2: ページ タイトルを自動的に割り当てます

手順 1. で説明したように、ページのタイトルは、宣言またはプログラムによって設定できます。 明示的にわかりやすいタイトルを変更することを忘れてしまった場合ただし、ページが既定のタイトル「無題ページ」。 理想的には、ページのタイトルは設定に自動的に私たちにとってその値を明示的に指定しないこと。 たとえば、実行時に、ページのタイトルは「無題ページ」が場合、する可能性がしたタイトルが ASP.NET ページのファイル名と同じであるに自動的に更新します。 良い知らせは、多少の事前作業が自動的に割り当てられているタイトルが存在することができます。

派生してすべての ASP.NET web ページ、`Page`クラス、`System.Web.UI`名前空間。 `Page`クラスは、ASP.NET ページで必要な最小限の機能を定義しなどの重要なプロパティを公開します`IsPostBack`、 `IsValid`、 `Request`、および`Response`、多数あります。 多くの場合、web アプリケーション内の各ページには、追加の機能または機能が必要です。 これを提供するための一般的な方法では、ベース ページのカスタム クラスを作成します。 ベース ページのカスタム クラスから派生したクラスを作成するは、`Page`クラスし、追加機能が含まれています。 この基本クラスが作成されたら、そこから派生、ASP.NET ページがあることができます (なく`Page`クラス) ため、ASP.NET ページの拡張機能を提供します。

この手順では、タイトル、それ以外の場合に明示的が設定されていない場合、ASP.NET ページのファイル名に、ページのタイトルを自動的に設定される基本ページを作成します。 手順 3 は、サイト マップに基づいてページのタイトルを設定します。

> [!NOTE]
> 徹底的に検証を作成して、ベース ページのカスタム クラスを使用するのでは、このチュートリアル シリーズの範囲外です。 詳細については、読み取る[Your ASP.NET ページの分離コード クラスのカスタムの基本クラスを使用して](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)します。


### <a name="creating-the-base-page-class"></a>基本ページ クラスを作成します。

まず最初に、拡張するクラスは、ページの基本クラスを作成する、`Page`クラス。 追加することで開始、`App_Code`ソリューション エクスプ ローラーでプロジェクト名を右クリックし、ASP.NET フォルダーの追加 を選択し、順に選択して、プロジェクトにフォルダー`App_Code`します。 次に、右クリックし、`App_Code`フォルダーという新しいクラスを追加および`BasePage.cs`します。 図 4 は、後のソリューション エクスプ ローラー、`App_Code`フォルダーと`BasePage.cs`クラスが追加されました。


![App_Code フォルダーと BasePage という名前のクラスを追加します。](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image4.png)

**図 04**: 追加、`App_Code`フォルダーとという名前のクラス `BasePage`


> [!NOTE]
> Visual Studio プロジェクト管理の 2 つのモードをサポートしています。 Web サイト プロジェクトと Web アプリケーション プロジェクト。 `App_Code`フォルダーが Web サイト プロジェクト モデルで使用するように設計します。 Web アプリケーション プロジェクト モデルを使用している場合は、配置、`BasePage.cs`以外の何かという名前のフォルダー内のクラス`App_Code`など`Classes`します。 このトピックの詳細についてを参照してください[Web アプリケーション プロジェクトに Web サイト プロジェクトを移行する](http://webproject.scottgu.com/CSharp/Migration2/Migration2.aspx)します。


カスタム ベース ページは ASP.NET ページの分離コード クラスの基本クラスとして機能する、拡張に必要な`Page`クラス。


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample5.cs)]

ASP.NET ページが要求されるたびに、一連のステージ、HTML にレンダリングされる、要求されたページで進行します。 ステージをタップしてオーバーライドすることで、`Page`クラスの`OnEvent`メソッド。 ベース ページみましょう自動的にタイトルを設定することが指定されていない場合に明示的にして、`LoadComplete`ステージ (、ご想像のとおりと後に発生、`Load`ステージ)。

これを行うには、オーバーライド、`OnLoadComplete`メソッドし、次のコードを入力します。


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample6.cs)]

`OnLoadComplete`メソッドはどうかを確認を開始、`Title`プロパティがまだ設定されていない明示的にします。 場合、`Title`プロパティは`null`、空の文字列か、値「無題ページ」が、要求された ASP.NET ページのファイル名に割り当てられます。 -要求された ASP.NET ページへの物理パス`C:\MySites\Tutorial03\Login.aspx`、たとえば、経由でアクセスできますが、`Request.PhysicalPath`プロパティ。 `Path.GetFileNameWithoutExtension`メソッドを使用して、ファイル名の一部だけをプルおよびにこのファイル名が割り当てられているし、`Page.Title`プロパティ。

> [!NOTE]
> タイトルの書式を向上させるためには、このロジックを強化することをお勧めします。 たとえば、ページのファイル名は`Company-Products.aspx`、上記のコードは「会社の製品」、タイトルが生成されますが、理想的には、dash「会社製品」のように、スペースで置き換えられます。 また、大文字と小文字の変更があるたびに、スペースを追加することを検討してください。 つまり、ファイル名を変換するコードの追加を検討する`OurBusinessHours.aspx`タイトルの「の営業時間」にします。


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>コンテンツ ページを基本 Page クラスの継承

これでカスタム ベース ページから派生する、サイトでの ASP.NET ページを更新する必要があります (`BasePage`) の代わりに、`Page`クラス。 各分離コード クラスには、この移動を実現し、クラス宣言を変更します。


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample7.cs)]

移動先:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample8.cs)]

その後、ブラウザーを使用してサイトを参照してください。 タイトルなど、明示的に設定 ページにアクセスするかどうかは`Default.aspx`または`About.aspx`、明示的に指定されたタイトルが使用されます。 ただし、タイトルが既定値 (「無題ページ」) から変更されていないページを参照してください場合、ベース ページ クラスは、タイトル ページのファイル名を設定します。

図 5 は、`MultipleContentPlaceHolders.aspx`ページをブラウザーで表示する場合。 タイトルは、正確にファイル名 (拡張子)、小さいにページのことに注意してください。"MultipleContentPlaceHolders"。


[![ページのファイル名では、自動的に使用されますが、タイトルが明示的に指定しない場合は、](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image5.png)

**図 05**: ページのファイル名では、自動的に使用されますが、タイトルが明示的に指定しない場合は、([フルサイズの画像を表示する をクリックします](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image7.png))。


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>手順 3: がに基づいてサイト マップのページ タイトル

ASP.NET には、ページ開発者と共に、SiteMapPath などのサイト マップに関する情報を表示するための Web コントロール (XML ファイルまたはデータベース テーブル) などの外部リソースで、階層のサイト マップを定義することができる信頼性の高いサイト マップ フレームワークが用意されていますメニューのおよび TreeView コントロール)。

サイト マップ構造は、ASP.NET ページの分離コード クラスからプログラムでもアクセスできます。 この方法で、サイト マップにページのタイトル、対応するノードのタイトルを自動的に設定できます。 みましょう強化、`BasePage`クラスのこの機能を提供するために、手順 2. で作成します。 まず、サイトのサイト マップを作成する必要があります。

> [!NOTE]
> このチュートリアルでは、リーダーは既に ASP 知識が前提としています。NET のサイト マップ機能です。 サイト マップの使用に関する詳細については、私のシリーズの記事を参照してください。 [ASP を確認します。NET のサイト ナビゲーション](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)します。


### <a name="creating-the-site-map"></a>サイト マップを作成します。

サイト マップのシステムが上に構築される、[プロバイダー モデル](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)、サイト マップ API メモリと永続的なストアの間のサイト マップ情報をシリアル化するロジックからを分離します。 .NET Framework が付属、 [ `XmlSiteMapProvider`クラス](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)、これは、既定のサイト マップ プロバイダー。 名前からわかるように、`XmlSiteMapProvider`サイト マップ ストアとして XML ファイルを使用します。 このプロバイダーを使用して、サイト マップを定義しましょう。

という名前の web サイトのルート フォルダーで、サイト マップ ファイルを作成して開始`Web.sitemap`します。 これを実現するには、ソリューション エクスプ ローラーで web サイトの名前を右クリックし、新しい項目の追加 を選択およびサイト マップ テンプレートを選択します。 ファイルの名前はことを確認します。`Web.sitemap`追加 をクリックします。


[![Web サイトのルート フォルダーへの Web.sitemap という名前のファイルを追加します。](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image8.png)

**図 06**: ファイルの名前を追加`Web.sitemap`web サイトのルート フォルダーに ([フルサイズの画像を表示する をクリックします](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image10.png))。


次の XML を追加、`Web.sitemap`ファイル。


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample9.xml)]

この XML は、図 7 に示すように階層のサイト マップ構造を定義します。


![サイト マップが現在で構成される 3 のサイト マップ ノードです。](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image11.png)

**図 07**:、サイト マップが現在で構成される 3 のサイト マップ ノード


新しい例を追加、今後のチュートリアル サイト マップ構造が更新されます。

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>ナビゲーションの Web コントロールを含めるにマスター ページを更新しています

サイト マップを定義したら、ナビゲーション Web コントロールを含めるにマスター ページを更新しましょう。 具体的には、サイト マップで定義されている各ノードのリスト項目と、順序なしリストを表示する」のレッスンのセクションでは、左側の列にみましょう ListView コントロールを追加します。

> [!NOTE]
> ListView コントロールは ASP.NET バージョン 3.5 に追加されました。 ASP.NET の以前のバージョンを使用している場合は、代わりに、Repeater コントロールを使用します。 ListView コントロールの詳細については、次を参照してください。[を使用して ASP.NET 3.5 の ListView と DataPager コントロール](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)します。


レッスン セクションから既存のマークアップの順序なしリストを削除することで開始します。 次に、ListView コントロールをツールボックスからドラッグし、レッスンの下にドロップ見出し。 その他のビュー コントロールと共に、ツールボックスの [データ] セクションに、ListView がある: GridView、DetailsView、およびフォーム ビュー。 ListView の ID プロパティを設定`LessonsList`します。

という名前の新しい SiteMapDataSource コントロールに、ListView をバインドするを選択して、データ ソース構成ウィザードから`LessonsDataSource`します。 SiteMapDataSource コントロールは、サイト マップのシステムからの階層構造を返します。


[![SiteMapDataSource コントロール LessonsList ListView コントロールをバインドします。](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image12.png)

**図 08**: SiteMapDataSource コントロールをバインド、 `LessonsList` ListView コントロール ([フルサイズの画像を表示する をクリックします](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image14.png))。


SiteMapDataSource コントロールを作成した後は、SiteMapDataSource コントロールによって返される各ノードのリスト項目と順不同のリストが表示されるように、ListView のテンプレートを定義する必要があります。 これは、次のテンプレート マークアップを使用して行うことができます。


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample10.aspx)]

`LayoutTemplate`マークアップ順不同のリストを生成 (`<ul>...</ul>`) 中に、`ItemTemplate`一覧の項目として、SiteMapDataSource によって返される各項目を表示します (`<li>`) 特定のレッスンへのリンクを格納しています。

ListView のテンプレートを構成した後、web サイトを参照してください。 図 9 に示すよう、レッスンでは、1 つの箇条書き項目ホームを説明します。 ContentPlaceHolder の複数のコントロールのレッスンを使用して、バージョン情報の検索 SiteMapDataSource が、データの階層的なセットを返すために設計されていますが、ListView コントロールは、階層の 1 つのレベルのみを表示できます。 その結果、SiteMapDataSource によって返されたサイト マップ ノードの最初のレベルのみが表示されます。


[![レッスン セクションには、1 つのリスト項目が含まれています。](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image15.png)

**図 09**: レッスン セクションには、1 つのリスト項目が含まれています ([フルサイズの画像を表示する をクリックします](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image17.png))。


複数のレベルを表示する内で複数の Listview を入れ子私たちでした、`ItemTemplate`します。 この方法が説明されて、 [*マスター ページとサイト ナビゲーション*チュートリアル](../../data-access/introduction/master-pages-and-site-navigation-cs.md)のマイ[チュートリアル シリーズのデータの操作](../../data-access/index.md)します。 ただし、このチュートリアル シリーズのサイト マップが含まれている 2 つのレベルだけ: ホーム (最上位レベル)。各レッスン ホームの子として。 入れ子になった ListView を作成するのではなく代わりにように指示できますを設定して開始ノードが返されない SiteMapDataSource その[`ShowStartingNode`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx)に`false`します。 実際の効果は、サイト マップ ノードの 2 番目の層を返すことによって、SiteMapDataSource が起動します。

この変更により、ListView の項目を表示しますバージョン情報のおよび複数のプレース ホルダー コントロールを使用して、レッスンしますが、ホームの箇条書き項目が省略されます。 これを解決する明示的に項目を追加する箇条書き自宅での`LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample11.aspx)]

開始ノードを省略する場合、SiteMapDataSource を構成し、ホーム箇条書き項目を明示的に追加する、レッスン セクションには、目的の出力ようになりましたが表示されます。


[![レッスンのセクションには、ホームとそれぞれの子ノードの箇条書き項目が含まれています](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image18.png)

**図 10**: レッスン セクションには、ホームおよびそれぞれの子ノードの箇条書き項目が含まれています ([フルサイズの画像を表示する をクリックします](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image20.png))。


### <a name="setting-the-title-based-on-the-site-map"></a>サイト マップに基づいてタイトルの設定

今後更新できる場所にあるサイト マップで、`BasePage`サイト マップで指定したタイトルを使用するクラス。 手順 2 で行ったようにのみ使用するサイト マップ ノードのタイトル場合は、ページのタイトルがページの開発者によって明示的に設定されていません。 要求されているページを明示的に設定されていない場合は、ページ タイトルと、ここに戻るから (低い拡張子)、要求されたページのファイル名を使用して手順 2 で行ったようし、サイト マップ内で見つかりません。 図 11 は、この意思決定プロセスを示しています。


![明示的に設定 ページのタイトルのない場合は、対応するサイト マップ ノードのタイトルが使用されます。](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image21.png)

**図 11**: 明示的に設定 ページのタイトルのない場合は、対応するサイト マップ ノードのタイトルが使用されます


更新プログラム、`BasePage`クラスの`OnLoadComplete`メソッドに次のコードを含めます。


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample12.cs)]

以前と同様、`OnLoadComplete`メソッドは、ページのタイトルが明示的に設定されているかどうかを決定することで開始します。 場合`Page.Title`は`null`、空の文字列、またはコードでは、値を自動的に割り当てられますし、値「無題ページ」が割り当てられる`Page.Title`します。

を使用するタイトルを確認する、コードの先頭を参照して、 [ `SiteMap`クラス](https://msdn.microsoft.com/library/system.web.sitemap.aspx)の[`CurrentNode`プロパティ](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx)します。 `CurrentNode` 返します、 [ `SiteMapNode` ](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx)現在要求されているページに対応するサイト マップ内のインスタンス。 サイト マップ内で見つかったが現在要求されているページと仮定すると、`SiteMapNode`の`Title`プロパティは、ページの title に割り当てられます。 現在要求されているページが、サイト マップにない場合`CurrentNode`返します`null`され (ステップ 2 で行われていた) として、タイトルとして、要求されたページのファイル名が使用されます。

図 12 は、`MultipleContentPlaceHolders.aspx`ページをブラウザーで表示する場合。 このページのタイトルが明示的に設定されていないため、対応するサイト マップ ノードのタイトルが代わりに使用されます。


![MultipleContentPlaceHolders.aspx ページのタイトルが、サイト マップから取得されます。](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image22.png)

**図 12**:`MultipleContentPlaceHolders.aspx`ページのタイトルが、サイト マップから取得されます。


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>手順 4: 追加するその他のページ固有のマークアップ、`<head>`セクション

手順 1、2、および 3 のカスタマイズについて説明しました、`<title>`ページの単位での要素。 ほかに`<title>`、`<head>`セクションが含まれます`<meta>`要素と`<link>`要素。 このチュートリアルで先ほど説明したように`Site.master`の`<head>`セクションが含まれています、`<link>`要素`Styles.css`します。 ため、この`<link>`要素がマスター ページ内で定義されているに含まれる、`<head>`すべてのコンテンツ ページのセクション。 追加する方法について移動する方法を私たちが`<meta>`と`<link>`ページごとに要素でしょうか。

ページ固有のコンテンツを追加する最も簡単な方法、`<head>`セクションは、マスター ページで ContentPlaceHolder のコントロールを作成することです。 このようなプレース ホルダーが既にある (名前付き`head`)。 そのため、ユーザー設定を追加する`<head>`マークアップ、作成、対応するページのコントロールのコンテンツし、マークアップのある配置。

追加のカスタムを説明するために`<head>`マークアップをページを含めてみましょう、 `<meta>` description 要素を現在のコンテンツ ページのセット。 `<meta>` Description 要素が web ページに関する簡単な説明を提供します。 検索結果を表示するときに、ほとんどの検索エンジンがなんらかの形では、この情報を組み込みます。

A `<meta>` description 要素が、次の形式。


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample13.html)]

コンテンツ ページには、このマークアップを追加するには、マスター ページの head プレース ホルダーに対応するコンテンツ コントロールに上のテキストを追加します。 たとえば、定義するため、`<meta>`の description 要素`Default.aspx`、次のマークアップを追加。


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample14.aspx)]

ヘッド プレース ホルダーはないので、HTML ページの本文内で、コンテンツ コントロールに追加したマークアップは、デザイン ビューでは表示されません。 参照してください、`<meta>`説明要素訪問`Default.aspx`ブラウザーを使用します。 ソースを表示し、注意して、ページが読み込まれた後、`<head>`セクションにコンテンツ コントロールで指定されたマークアップが含まれています。

追加する少し`<meta>`に description 要素`About.aspx`、 `MultipleContentPlaceHolders.aspx`、および`Login.aspx`します。

### <a name="programmatically-adding-markup-to-theheadregion"></a>プログラムを使用するマークアップを追加、`<head>`リージョン

ContentPlaceHolder のヘッドでは、宣言によって、マスター ページにカスタムのマークアップを追加できます。`<head>`リージョン。 カスタム マークアップもプログラムで追加される可能性があります。 いることを思い出してください、`Page`クラスの`Header`プロパティが返す、`HtmlHead`マスター ページで定義されているインスタンス (、 `<head runat="server">`)。

プログラムによってコンテンツを追加すること、`<head>`追加するコンテンツが動的なリージョンが有用です。 ページにアクセスしたユーザーに基づくなどおそらくデータベースから取得されるは。 どのような理由は、コンテンツを追加できます、`HtmlHead`その Controls コレクションにコントロールを追加して次のようにします。


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample15.cs)]

上記のコードを追加、`<meta>`キーワード要素を`<head>`領域で、ページを説明するキーワードのコンマ区切りの一覧を示します。 追加する、`<meta>`タグを作成する、 [ `HtmlMeta` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx)インスタンスを設定、`Name`と`Content`プロパティに追加、`Header`の`Controls`コレクション。 同様に、プログラムで追加する、`<link>`要素を作成、 [ `HtmlLink` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx)オブジェクト、そのプロパティを設定しを追加、`Header`の`Controls`コレクション。

> [!NOTE]
> 任意のマークアップを追加するには、作成、 [ `LiteralControl` ](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx)インスタンスは、設定、`Text`プロパティに追加、`Header`の`Controls`コレクション。


## <a name="summary"></a>まとめ

このチュートリアルではさまざまな方法で追加のしました`<head>`リージョンのマークアップをページごとにします。 マスター ページを含める必要があります、`HtmlHead`インスタンス (`<head runat="server">`) を得ることにします。 `HtmlHead`インスタンスは、コンテンツのページにプログラムでアクセスを許可、`<head>`領域を宣言とプログラミングの設定ページのタイトルは、プレース ホルダー コントロールにより、カスタムのマークアップに追加して、 `<head>`コンテンツ コントロールの宣言によりセクション。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [Asp.net ページのタイトルを動的に設定](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [ASP を調べています。NET のサイト ナビゲーション](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [HTML Meta タグを使用するには、方法](http://searchenginewatch.com/showPage.html?page=2167931)
- [ASP.NET のマスター ページ](http://www.odetocode.com/articles/419.aspx)
- [ASP.NET 3.5 を使用して ListView と DataPager コントロール](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [ASP.NET ページの分離コード クラスのカスタム基底クラスの使用](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)作成者の複数受け取りますブックと 4GuysFromRolla.com の創設者で、携わって Microsoft Web テクノロジ 1998 年からです。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 3.5 in 24 時間*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Zack Jones と Suchi 著でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[ mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)します。

> [!div class="step-by-step"]
> [前へ](multiple-contentplaceholders-and-default-content-cs.md)
> [次へ](urls-in-master-pages-cs.md)
