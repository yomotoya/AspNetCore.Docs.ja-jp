---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
title: "マスター ページ (VB) で、タイトル、メタ タグ、およびその他の HTML ヘッダーを指定する |Microsoft ドキュメント"
author: rick-anderson
description: "さまざまな定義をさまざまな手法を見ます&lt;ヘッド&gt;コンテンツ ページから、マスター ページ内の要素。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: ea8196f5-039d-43ec-8447-8997ad4d3900
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 1bbc2efc67d2d828dd0a5c1fcfe95145e8ffb2cb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb"></a>マスター ページ (VB) で、タイトル、メタ タグ、およびその他の HTML ヘッダーの指定
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_VB.pdf)

> さまざまな定義をさまざまな手法を見ます&lt;ヘッド&gt;コンテンツ ページから、マスター ページ内の要素。


## <a name="introduction"></a>はじめに

Visual Studio 2008 で作成される新しいのマスター ページがある、既定では、2 つのプレース ホルダー コントロール: という名前の 1 つ`head`、内に配置し、`<head>`要素と 1 つの名前付き`ContentPlaceHolder1`、Web フォーム内に設定します。 目的は、`ContentPlaceHolder1`は、Web フォーム ページ単位ごとにカスタマイズ可能な領域を定義します。 `head` ContentPlaceHolder により、カスタム コンテンツを追加するページ、`<head>`セクションです。 (もちろん、これら 2 つの contentplaceholders にも変更や削除、および追加 ContentPlaceHolder はマスター ページに追加できます。 マスターのページでは、 `Site.master`、4 つのプレース ホルダー コントロールには現在はします)。

HTML`<head>`要素は、ドキュメント自体の一部ではない web ページのドキュメントに関する情報のリポジトリとして機能します。 これは、web ページのタイトルなどの情報、メタ情報が検索エンジンまたは内部のクローラーと RSS フィード、JavaScript、CSS などの外部リソースへのリンクがファイルを使用します。 この情報の一部は、web サイトのすべてのページに関連する可能性があります。 たとえば、JavaScript ファイルのすべての ASP.NET ページと同じ CSS 規則をグローバルにインポートすることができます。 ただしの部分がある、`<head>`ページ固有の要素。 ページのタイトルは、主要な例です。

このチュートリアルではグローバルとページ固有の定義方法を調べてお`<head>`マスター ページとそのコンテンツ ページ セクション マークアップ。

## <a name="examining-the-master-pagesheadsection"></a>マスター ページを確認する`<head>`セクション

既定のマスター ページファイル Visual Studio 2008 で作成にはで次のマークアップが含まれています。 その`<head>`セクション。


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample1.aspx)]

注意して、`<head>`要素が含まれています、`runat="server"`属性は、サーバー コントロール (なく、静的な HTML) であることを示します。 派生してすべての ASP.NET ページ、 [ `Page`クラス](https://msdn.microsoft.com/en-us/library/system.web.ui.page.aspx)にある、`System.Web.UI`名前空間。 このクラスに含まれる、 [ `Header`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.page.header.aspx)をページのアクセスを提供する`<head>`領域。 使用して、`Header`プロパティ ASP.NET ページのタイトルを設定したり、表示するマークアップを追加お`<head>`セクションです。 可能であれば、その後、コンテンツ ページのカスタマイズを`<head>`要素で、ページの少量のコードを記述して`Page_Load`イベント ハンドラー。 手順 1. でページのタイトルをプログラムで設定する方法を説明します。

マークアップ、`<head>`上の要素は、という名前のプレース ホルダー コントロールも含まれています。`head`です。 ContentPlaceHolder はこの制御を必要に応じて、コンテンツ ページがカスタム コンテンツを追加できるよう、`<head>`要素プログラムでします。 ただし、コンテンツ ページが static のマークアップを追加する必要がある場合に、便利です、`<head>`対応するコンテンツ コントロールにはなく、プログラムによって、static のマークアップとしての要素を宣言によって追加できます。

加え、`<title>`要素および`head`ContentPlaceHolder、マスター ページの`<head>`要素を含める必要があります`<head>`-すべてのページに共通するレベルのマークアップ。 すべてのページ、web サイトで定義された CSS 規則を使用して、`Styles.css`ファイル。 その結果、更新された、`<head>`内の要素、 [*マスター ページで、サイト全体のレイアウトを作成する*](creating-a-site-wide-layout-using-master-pages-vb.md) 、対応するを含めるには、チュートリアル`<link>`要素。 当社`Site.master`マスター ページの現在`<head>`マークアップを次に示します。


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>手順 1: コンテンツ ページのタイトルを設定します。

Web ページのタイトルを指定した、`<title>`要素。 これが、適切な値の各ページのタイトルを設定する重要です。 ページを訪問すると、ブラウザーのタイトル バーにそのタイトルが表示されます。 さらに、ページのブックマークを作成するときに、ブラウザーはページのタイトルをブックマークの推奨される名前として使用します。 また、多くの検索エンジンは、検索結果を表示するときに、ページのタイトルを表示します。

> [!NOTE]
> 既定では、Visual Studio の設定、 `<title>` 「無題」マスター ページ内の要素。 同様に、新規の ASP.NET ページがその`<title>`すぎます「無題ページ」に設定します。 できるので、適切な値をページのタイトルを設定することを忘れがち、多くのページには「無題」というタイトルのインターネット。 Google を探してこのタイトルを含む web ページには、ほぼ 2,460,000 結果が返されます。 マイクロソフトでもは「無題」というタイトルの発行の web ページを受けやすくなります。 この記事の執筆時に、Google 検索には、Microsoft.com ドメインでこのような 236 の web ページが報告されました。


ASP.NET ページは、次の方法のいずれかで、タイトルを指定できます。

- 内で値を直接配置することによって、`<title>`要素
- 使用して、`Title`属性、`<%@ Page %>`ディレクティブ
- プログラムによって設定ページの`Title`のようなコードを使用してプロパティ`Page.Title="title"`または`Page.Header.Title="title"`です。

ページがないコンテンツ、`<title>`マスター ページでは、要素が定義されています。 そのため、コンテンツ ページのタイトルの設定を使用するか、`<%@ Page %>`ディレクティブの`Title`属性またはプログラムによって設定します。

### <a name="setting-the-pages-title-declaratively"></a>ページのタイトルを宣言して設定

コンテンツ ページのタイトルはを通じて宣言的設定できる、`Title`の属性、 [ `<%@ Page %>`ディレクティブ](https://msdn.microsoft.com/en-us/library/ydy4x04a.aspx)です。 直接変更することによってこのプロパティを設定することができます、`<%@ Page %>`ディレクティブまたはのプロパティ ウィンドウを使用します。 両方の方法を見てみましょう。

ソース ビューから検索、`<%@ Page %>`ページの宣言型マークアップの上部にあるディレクティブです。 `<%@ Page %>`ディレクティブを`Default.aspx`に従います。


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample3.aspx)]

`<%@ Page %>`ディレクティブを解析し、ページをコンパイルするときに、ASP.NET エンジンによって使用されるページ固有の属性を指定します。 これには、そのマスター ページファイル、そのコード ファイル、およびその他の情報のタイトルの位置が含まれます。

既定では、Visual Studio の設定の新しいコンテンツ ページを作成するときに、`Title`属性を「無題」です。 変更`Default.aspx`の`Title`「無題」から「マスター ページ チュートリアル」を属性し、ブラウザーでページを表示します。 図 1 は、ブラウザーのタイトル バーに、新しいページ タイトルが反映されますを示します。


![ブラウザーのタイトル バーの表示&quot;マスター ページ チュートリアル&quot;の代わりに&quot;無題のページ&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image1.png)

**図 01**: ブラウザーのタイトル バー「無題」ではなく「マスター ページのチュートリアル」の表示


ページのタイトルは、[プロパティ] ウィンドウからも設定することがあります。 [プロパティ] ウィンドウからドキュメントを選択ドロップダウン リストから、ロード、ページ レベル プロパティが含まれます、`Title`プロパティです。 図 2 は、後の [プロパティ] ウィンドウを示しています。 `Title` "マスター ページ チュートリアル"に設定されています。


![すぎます [プロパティ] ウィンドウのタイトルを構成することがあります。](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image2.png)

**図 02**: すぎます [プロパティ] ウィンドウのタイトルを構成することがあります


### <a name="setting-the-pages-title-programmatically"></a>プログラムによるページのタイトルの設定

マスター ページの`<head runat="server">`マークアップに変換される、 [ `HtmlHead`クラス](https://msdn.microsoft.com/en-us/library/system.web.ui.htmlcontrols.htmlhead.aspx)ASP.NET エンジンによって、ページが表示される場合をインスタンス化します。 `HtmlHead`クラスには、 [ `Title`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.htmlcontrols.htmlhead.title.aspx)値が反映される、レンダリングされたで`<title>`要素。 このプロパティは、ASP.NET ページの分離コード クラスを使用してからアクセスできる`Page.Header.Title`以外の場合はこの同じプロパティを使用してアクセスすることも`Page.Title`します。

ページのタイトルをプログラムで設定を練習するに移動、`About.aspx`ページの分離コード クラスし、ページのイベント ハンドラーを作成`Load`イベント。 次に、設定ページのタイトル"マスター ページのチュートリアル:: に関する::*日付*"ここで、*日付*は、現在の日付。 このコードを追加した後、`Page_Load`イベント ハンドラーは、次のようになります。


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample4.vb)]

図 3 は、ブラウザーのタイトル バーにアクセスすると、`About.aspx`ページ。


![ページのタイトルをプログラムで設定され、現在の日付が含まれています](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image3.png)

**図 03**: ページのタイトルをプログラムで設定され、現在の日付が含まれています


## <a name="step-2-automatically-assigning-a-page-title"></a>手順 2: ページのタイトルを自動的に割り当てること

手順 1. で説明したとおり、宣言またはプログラムによってページのタイトルを設定することができます。 忘れた場合は明示的にわかりやすいタイトルを変更する、ただし、ページが既定のタイトル「無題」です。 理想的には、ページのタイトルは設定に自動的にご利用の米国の値は明示的に指定しています。 たとえば、実行時に、ページのタイトルは「無題」は、場合おタイトルを自動的に更新され、ASP.NET ページのファイル名と同じである可能性があります。 良いニュースは、自動的に割り当てられているタイトルが存在することは初期の作業のほんの少しです。

派生してすべての ASP.NET web ページ、`Page`派生クラス。 `Page`クラスは、ASP.NET ページで必要な最小限の機能を定義しと同様に重要なプロパティを公開`IsPostBack`、 `IsValid`、 `Request`、および`Response`、その他の多くの間でします。 多くの場合、web アプリケーション内の各ページには、追加の機能や機能が必要です。 これを提示するための一般的な方法では、ベース ページのカスタム クラスを作成します。 ベース ページのカスタム クラスから派生するクラスを作成するは、`Page`クラスし、その他の機能が含まれます。 この基本クラスが作成されたら、ASP.NET ページがそこから派生することが (ではなく、`Page`クラス)、これにより、ASP.NET ページへの拡張機能を提供します。

この手順では、タイトル、それ以外の場合に明示的が設定されていない場合に、ASP.NET ページのファイル名に、ページのタイトルを自動的に設定する基本ページを作成します。 手順 3 サイト マップに基づく、ページのタイトルの設定を見ます。

> [!NOTE]
> 徹底的に検証を作成して、ベース ページのカスタム クラスを使用するは、このチュートリアル シリーズのスコープ外です。 詳細については、「 [Your ASP.NET ページの分離コード クラス カスタムの基本クラスを使用して](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)です。


### <a name="creating-the-base-page-class"></a>ページの基本クラスを作成します。

まず最初に、拡張するクラスであるページの基本クラスを作成する、`Page`クラスです。 追加することで開始、 `App_Code` 、ソリューション エクスプ ローラーでプロジェクト名を右クリックし、ASP.NET フォルダーの追加 を選択するを選択してプロジェクトにフォルダー`App_Code`です。 次を右クリックし、`App_Code`フォルダーという名前の新しいクラスを追加および`BasePage.vb`です。 図 4 は、後に、ソリューション エクスプ ローラー、`App_Code`フォルダーと`BasePage.vb`クラスが追加されました。


![App_Code フォルダーと BasePage をという名前のクラスを追加します。](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image4.png)

**図 04**: 追加、`App_Code`フォルダーとという名前のクラス`BasePage`


> [!NOTE]
> Visual Studio には、プロジェクト管理の 2 つのモードがサポートされています。 Web サイト プロジェクトと Web アプリケーション プロジェクト。 `App_Code`フォルダーが、Web サイト プロジェクトのモデルで使用するように設計されています。 Web アプリケーション プロジェクトのモデルを使用している場合は、配置、`BasePage.vb`以外の名前のフォルダー内のクラス`App_Code`など`Classes`です。 このトピックの詳細についてを参照してください[Web アプリケーション プロジェクトに Web サイト プロジェクトを移行する](http://webproject.scottgu.com/VisualBasic/Migration2/Migration2.aspx)です。


拡張する必要があるカスタム基本ページは、ASP.NET ページの分離コード クラスの基底クラスとして機能、`Page`クラスです。


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample5.vb)]

ASP.NET ページが要求されるたびに一連のステージをすると、HTML に表示される、要求されたページに進みます。 おをタップしてできますをステージにオーバーライドすることで、`Page`クラスの`OnEvent`メソッドです。 ベース ページみましょう場合は、明示的が指定されていませんが、タイトルを自動的に設定、`LoadComplete`ステージ (、想像のとおり、発生した後、`Load`ステージ)。

これを実現する、オーバーライド、`OnLoadComplete`メソッドと、次のコードを入力します。


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample6.vb)]

`OnLoadComplete`メソッドは場合を決定することにより、開始、`Title`プロパティがまだ設定されていない明示的にします。 場合、`Title`プロパティは`Nothing`、空の文字列か「無題」の値が、要求された ASP.NET ページのファイル名に割り当てられます。 -要求された ASP.NET ページへの物理パス`C:\MySites\Tutorial03\Login.aspx`、たとえば - は経由でアクセスでき、`Request.PhysicalPath`プロパティです。 `Path.GetFileNameWithoutExtension`メソッドを使用して、ファイル名の部分だけをプルしてにこのファイル名が割り当てられます、`Page.Title`プロパティです。

> [!NOTE]
> 私たちの目標をタイトルの形式を改善するには、このロジックを強化するためにします。 たとえば、ページのファイル名は`Company-Products.aspx`、"会社-Products"など、タイトルを上記のコードが生成されますが、理想的には、ダッシュ"Products"などの会社と同様に、スペースで置き換えられます。 また、ケースが変更されるたびに、スペースを追加することを検討してください。 つまり、ファイル名を変換するコードを追加することを検討`OurBusinessHours.aspx`タイトルの"の"営業時間にします。


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>コンテンツ ページをページの基本クラスを継承

これでカスタム ベース ページから派生する、サイト内の ASP.NET ページを更新する必要があります (`BasePage`) の代わりに、`Page`クラスです。 各分離コード クラスには、この移動を実行してから、クラス宣言を変更します。


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample7.vb)]

移動先:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample8.vb)]

その後、ブラウザーを使用してサイトを参照してください。 タイトルなどに明示的に設定 ページにアクセスするかどうかは`Default.aspx`または`About.aspx`、明示的に指定されたタイトルが使用されます。 ただし、タイトルが既定値 (「無題」) から変更されていないページをアクセスする場合は、ページの基本クラスは、タイトル ページのファイル名を設定します。

図 5 は、`MultipleContentPlaceHolders.aspx`ページをブラウザーで表示する場合。 (低くなりますが、拡張機能)、ページのファイル名厳密には、タイトルはことに注意してください"MultipleContentPlaceHolders"です。


[![ページのファイル名では、自動的に使用されますが、タイトルが明示的に指定しない場合は、](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image5.png)

**図 05**: ページのファイル名では、自動的に使用されますが、タイトルが明示的に指定しない場合は、([フルサイズのイメージを表示するをクリックして](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>手順 3: がに基づいて、サイト マップのページ タイトル

ASP.NET には、ページの開発者 (など、サイト マップ パスは、サイト マップに関する情報を表示する Web コントロールと共に (XML ファイルまたはデータベース テーブル) などの外部リソースの階層構造のサイト マップを定義することができる信頼性の高いサイト マップ フレームワークが用意されていますメニューのコントロールと TreeView コントロール)。

サイト マップ構造体は、ASP.NET ページの分離コード クラスをプログラムからもアクセスできます。 この方法で、サイト マップに、対応するノードのタイトルにページのタイトルを自動的に設定できます。 みましょう強化、`BasePage`クラス手順 2. で作成されるため、この機能を提供します。 最初のサイトのサイト マップを作成する必要があります。

> [!NOTE]
> このチュートリアルでは、リーダーは既に ASP 使い慣れてが前提としています。NET のサイト マップ機能です。 サイト マップを使用する方法については、マルチパート記事連載を参照してください。 [ASP を調べることです。NET のサイトのナビゲーション](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)です。


### <a name="creating-the-site-map"></a>サイト マップを作成します。

マップのサイト システムが上に構築される、[プロバイダー モデル](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)メモリと永続的なストア間でのサイト マップ情報をシリアル化するロジックからサイト マップ API を分離します。 .NET Framework が付属しています、 [ `XmlSiteMapProvider`クラス](https://msdn.microsoft.com/en-us/library/system.web.xmlsitemapprovider.aspx)、これは既定のサイト マップ プロバイダー。 その名前からわかるように、`XmlSiteMapProvider`サイト マップ ストアとして XML ファイルを使用します。 マイクロソフトのサイト マップを定義するため、このプロバイダーを使用してみましょう。

という名前の web サイトのルート フォルダーで、サイト マップ ファイルを作成して開始`Web.sitemap`です。 これを実現するには、ソリューション エクスプ ローラーで web サイトの名前を右クリックし、新しい項目の追加 を選択し、サイト マップ テンプレートを選択します。 ファイルの名前はことを確認してください。`Web.sitemap`追加 をクリックします。


[![Web サイトのルート フォルダーに Web.sitemap をという名前のファイルを追加します。](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image8.png)

**図 06**: ファイルの名前を追加`Web.sitemap`、web サイトのルート フォルダーに ([フルサイズのイメージを表示するをクリックして](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image10.png))


次の XML を追加、`Web.sitemap`ファイル。


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample9.xml)]

この XML は、図 7 に示すように、階層構造のサイト マップ構造を定義します。


![サイト マップは現在で構成される 3 のサイト マップ ノードには](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image11.png)

**図 07**:、サイト マップは、現在で構成される 3 のサイト マップ ノード


新しい例を追加して、今後のチュートリアルで、サイト マップ構造が更新されます。

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>マスター ページのナビゲーション Web コントロールの更新

これでが定義されているサイト マップ、みましょうナビゲーション Web コントロールを挿入するマスター ページを更新します。 具体的には、サイト マップで定義されている各ノードのリスト項目の順序なしリストを表示する」のレッスンのセクションでは、左の列にみましょう ListView コントロールを追加します。

> [!NOTE]
> ListView コントロールは ASP.NET 3.5 が新たに追加します。 ASP.NET の以前のバージョンを使用している場合は、代わりにリピータ コントロールを使用します。 ListView コントロールの詳細については、次を参照してください。[を使用して ASP.NET 3.5 の ListView コントロールと DataPager コントロール](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)です。


レッスン セクションから既存の順序なしリストのマークアップを削除することで開始します。 次に、ListView コントロールをツールボックスからドラッグし、レッスンの下にドロップ見出し。 ListView が他のビュー コントロールと共に、ツールボックスの [データ] セクションにある: GridView、DetailsView、およびフォーム ビュー。 ListView の設定`ID`プロパティを`LessonsList`です。

ListView コントロールにバインドする、新しい SiteMapDataSource という名前を選択して、データ ソース構成ウィザードから`LessonsDataSource`です。 SiteMapDataSource コントロールは、サイト マップ システムからの階層構造を返します。


[![SiteMapDataSource コントロール LessonsList ListView コントロールをバインドします。](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image12.png)

**図 08**: SiteMapDataSource コントロール LessonsList ListView コントロールをバインド ([フルサイズのイメージを表示するをクリックして](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image14.png))


SiteMapDataSource コントロールを作成した後に SiteMapDataSource コントロールによって返される各ノードのリスト項目の順序なしリストをレンダリングするように、ListView のテンプレートを定義する必要があります。 これは、次のテンプレートのマークアップを使用して実行できます。


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample10.aspx)]

`LayoutTemplate`順序なしリストのマークアップを生成 (`<ul>...</ul>`) 中に、`ItemTemplate`一覧の項目として SiteMapDataSource によって返される各項目を表示 (`<li>`) の特定のレッスンへのリンクを格納しています。

ListView のテンプレートを構成した後は、web サイトを参照してください。 図 9 に示すレッスン セクションには、1 つの箇条書きアイテム ホームが含まれています。 複数のプレース ホルダー コントロールのレッスンを使用してバージョン情報はどこにか SiteMapDataSource が、データの階層的なセットを返すように設計されていますが、ListView コントロールは、階層の 1 つのレベルのみを表示できます。 その結果、SiteMapDataSource によって返されたサイト マップ ノードの最初のレベルのみが表示されます。


[![レッスン セクションには、1 つのリスト項目が含まれています。](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image15.png)

**図 09**: レッスン セクションには、1 つのリスト項目が含まれています ([フルサイズのイメージを表示するをクリックして](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image17.png))


複数のレベルを表示するには、内の複数の Listview を入れ子おでした、`ItemTemplate`です。 この手法で確認された、 [*マスター ページとサイトのナビゲーション*チュートリアル](../../data-access/introduction/master-pages-and-site-navigation-vb.md)のマイ[チュートリアル シリーズのデータの操作](../../data-access/index.md)です。 ただし、このチュートリアル シリーズのサイト マップには 2 つのレベルだけ: ホーム (最上位レベル) です。各レッスン ホームの子として。 入れ子になった ListView を作成するではなく代わりにように指示できますを設定していない開始ノードを返す SiteMapDataSource その[`ShowStartingNode`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx)に`False`です。 実質的な影響は、サイト マップ ノードの 2 番目の層を返すことによって、SiteMapDataSource が開始されます。

この変更により、ListView がバージョン情報の記号付きの項目を表示し、複数 ContentPlaceHolder のコントロールを使用して、レッスンしますが、ホームの箇条書きの項目を除外します。 この問題を解決するお明示的に追加できます箇条書きの項目を自宅での`LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample11.aspx)]

開始ノードを省略する SiteMapDataSource を構成し、明示的にホームの項目の追加、レッスン セクションでは、目的の出力がここで表示されます。


[![レッスン セクションには、ホーム組織とそれぞれの子ノードの箇条書きの項目が含まれます](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image18.png)

**図 10**: レッスン セクションには、ホーム組織とそれぞれの子ノードの箇条書きの項目が含まれます ([フルサイズのイメージを表示するをクリックして](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>サイト マップに基づいたタイトルの設定

場所にあるサイト マップを更新することが、`BasePage`サイト マップで指定したタイトルを使用するクラス。 手順 2 で行ったようにのみが必要場合に使用するサイト マップ ノードのタイトル、ページのタイトルがページの開発者によって明示的に設定されていません。 要求されたページに明示的に設定があるない場合は、ページ タイトルとおありますを使用する切り替え (低くなりますが、拡張機能)、ファイル名を要求されたページの手順 2 で行ったようにしてから、サイト マップに格納されていません。 図 11 は、この意思決定プロセスを示しています。


![明示的に設定 ページのタイトルのない場合は、対応するサイト マップ ノードのタイトルが使用されます。](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image21.png)

**図 11**: 明示的に設定 ページのタイトルのない場合は、対応するサイト マップ ノードのタイトルが使用されます


更新プログラム、`BasePage`クラスの`OnLoadComplete`メソッドに次のコードを含めます。


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample12.vb)]

前とに、、`OnLoadComplete`メソッドは、ページのタイトルが明示的に設定されているかどうかを決定することで開始します。 場合`Page.Title`は`Nothing`、空の文字列が割り当てられている値「無題ページ」に値を自動的に代入し、または`Page.Title`です。

コードを参照することによって開始に使用するタイトルを確認するのには[`SiteMap`クラス](https://msdn.microsoft.com/en-us/library/system.web.sitemap.aspx)の[`CurrentNode`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.sitemap.currentnode.aspx)です。 `CurrentNode`返します、 [ `SiteMapNode` ](https://msdn.microsoft.com/en-us/library/system.web.sitemapnode.aspx)現在の要求されたページに対応するサイト マップ内のインスタンス。 サイト マップ内で見つかったが、現在の要求されたページと仮定した場合、`SiteMapNode`の`Title`プロパティは、ページのタイトルに割り当てられています。 現在の要求されたページが、サイト マップにない場合`CurrentNode`返します`Nothing`され (ステップ 2 で行った) として、タイトルとして、要求されたページのファイル名が使用されます。

図 12 を示しています、`MultipleContentPlaceHolders.aspx`ページをブラウザーで表示する場合。 このページのタイトルが明示的に設定されていないため、対応するサイト マップ ノードのタイトルが代わりに使用されます。


![MultipleContentPlaceHolders.aspx ページのタイトルが、サイト マップからプルされました。](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image22.png)

**図 12**:「MultipleContentPlaceHolders.aspx ページのタイトル、サイト マップからプルされました。


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>手順 4: 追加するには、その他のページ固有のマークアップ、`<head>`セクション

カスタマイズに手順 1、2、および 3 が検査、`<title>`ページの単位での要素。 加え`<title>`、`<head>`セクションが含まれます`<meta>`要素および`<link>`要素。 このチュートリアルで既に説明したとおり`Site.master`の`<head>`セクションが含まれています、`<link>`要素を`Styles.css`です。 この`<link>`要素が、マスター ページ内で定義されているに含まれている、`<head>`すべてのコンテンツ ページのセクションでします。 すればを追加する方法が`<meta>`と`<link>`要素をページ単位ごとにしますか?

ページ固有のコンテンツを追加する最も簡単な方法、`<head>`セクションは、マスター ページ プレース ホルダー コントロールを作成します。 このような ContentPlaceHolder がすでにある (という`head`)。 そのため、ユーザー設定を追加する`<head>`マークアップ、作成、対応するページのコントロールのコンテンツがマークアップに置きです。

追加のユーザー設定を説明するために`<head>`マークアップをページ、説明が含まれて、`<meta>`コンテンツ ページの現在のセットに要素を説明します。 `<meta>` Description 要素が、web ページに関する簡単な説明を提供します。 ほとんどの検索エンジンが検索結果を表示するときに、何らかの形では、この情報を組み込みます。

A `<meta>` description 要素が、次の形式。


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample13.html)]

コンテンツ ページには、このマークアップを追加するには、マスター ページにマップされるコンテンツ コントロールに上記のテキストを追加`head`ContentPlaceHolder です。 例については、定義する、`<meta>`の description 要素`Default.aspx`、次のマークアップを追加します。


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample14.aspx)]

`head` ContentPlaceHolder は HTML ページの本文内では、コンテンツ コントロールに追加したマークアップは、デザイン ビューでは表示されません。 表示する、`<meta>`説明要素訪問`Default.aspx`ブラウザーを使用します。 ソースを表示し、なお、ページが読み込まれた後、`<head>`セクションには、コンテンツ コントロールに指定されたマークアップが含まれています。

追加するには、しばらく時間かかる`<meta>`説明要素`About.aspx`、 `MultipleContentPlaceHolders.aspx`、および`Login.aspx`です。

### <a name="programmatically-adding-markup-to-theheadregion"></a>プログラムでマークアップを追加する、`<head>`地域

`head` ContentPlaceHolder により、宣言的に、マスター ページのカスタム マークアップを追加する`<head>`領域。 カスタム マークアップもプログラムで追加することがあります。 注意してください、`Page`クラスの`Header`プロパティから返される、`HtmlHead`マスター ページで定義されているインスタンス (、 `<head runat="server">`)。

コンテンツをプログラムで追加すること、`<head>`を追加するコンテンツが動的な領域が有用です。 ページにアクセスしたユーザーに基づくなど状況によって異なるデータベースからプルされているがします。 コンテンツを追加するの理由に関係なく、`HtmlHead`にコントロールを追加してその`Controls`コレクション次のように。


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample15.vb)]

上記のコードを追加、`<meta>`キーワード要素を`<head>`領域で、ページを説明するキーワードのコンマ区切りの一覧を提供します。 追加することに注意してください、`<meta>`タグを作成する、 [ `HtmlMeta` ](https://msdn.microsoft.com/en-us/library/system.web.ui.htmlcontrols.htmlmeta.aspx)インスタンスは、設定、`Name`と`Content`プロパティに追加し、`Header`の`Controls`コレクション。 同様に、プログラムで追加する、`<link>`要素、作成、 [ `HtmlLink` ](https://msdn.microsoft.com/en-us/library/system.web.ui.htmlcontrols.htmllink.aspx)オブジェクト、そのプロパティを設定しを追加、`Header`の`Controls`コレクション。

> [!NOTE]
> 任意のマークアップを追加するには、作成、 [ `LiteralControl` ](https://msdn.microsoft.com/en-us/library/system.web.ui.literalcontrol.aspx)インスタンスは、設定、`Text`プロパティに追加、`Header`の`Controls`コレクション。


## <a name="summary"></a>概要

このチュートリアルで追加する方法のさまざまなに考えた`<head>`ページ単位ごとに地域マークアップ。 マスター ページを含める必要があります、`HtmlHead`インスタンス (`<head runat="server">`)、ContentPlaceHolder とします。 `HtmlHead`インスタンスには、コンテンツ、ページにプログラムでアクセスができるように、`<head>`地域の宣言とプログラムでは、ページを設定する 's タイトルです ContentPlaceHolder コントロールに追加するカスタムのマークアップを使用すると、 `<head>` 。コンテンツ コントロールを宣言して参照してください。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET のページのタイトルの動的設定](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [ASP を検査中です。NET のサイトのナビゲーション](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [HTML のメタ タグを使用するには、方法](http://searchenginewatch.com/showPage.html?page=2167931)
- [ASP.NET マスター ページ](http://www.odetocode.com/articles/419.aspx)
- [ASP.NET 3.5 を使用して ListView コントロールと DataPager コントロール](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [ASP.NET ページの分離コード クラスのカスタムの基本クラスの使用](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、作成者複数受け取ります書籍や 4GuysFromRolla.com の創設者を操作した Microsoft Web テクノロジ 1998 年です。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 3.5 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログでを介して[http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Zack Jones および Suchi Banerjee がいました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[ mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)です。

>[!div class="step-by-step"]
[前へ](multiple-contentplaceholders-and-default-content-vb.md)
[次へ](urls-in-master-pages-vb.md)
