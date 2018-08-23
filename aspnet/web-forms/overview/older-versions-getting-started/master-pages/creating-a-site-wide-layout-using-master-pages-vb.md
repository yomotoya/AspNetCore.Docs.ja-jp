---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
title: マスター ページ (VB) を使用してサイト全体レイアウトの作成 |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、マスター ページの基本を説明します。 つまり、マスター ページとはどのように 1 つのマスター ページを作成、1 つの cr はコンテンツのプレース ホルダーでは、どのようにしています.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: 30945276-8ed9-4b27-8e50-4309244d3559
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: feb04c19092101bb019883c8b72b40ceb9afc015
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837390"
---
<a name="creating-a-site-wide-layout-using-master-pages-vb"></a>マスター ページ (VB) を使用してサイト全体レイアウトを作成します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_VB.pdf)

> このチュートリアルでは、マスター ページの基本を説明します。 つまり、マスター ページとは、マスター ページはどのように 1 つ作成コンテンツ プレース ホルダーは、方法は 1 つ ASP.NET ページを作成、変更する方法、マスター ページは自動的に反映されます、関連付けられているコンテンツのページでマスター ページを使用します。


## <a name="introduction"></a>はじめに

適切に設計された web サイトの 1 つの属性は、サイト全体の一貫性のあるページ レイアウトです。 www.asp.net web サイトを例にとってみましょう。 この記事の執筆時は、すべてのページには、上部と、ページの下部に同じ内容があります。 図 1 は、各ページの最上部のマイクロソフト コミュニティの一覧が灰色のバーが表示されます。 つまり、サイトのロゴ、先サイトが翻訳済み、言語のコア セクションでは、一覧の下にある: ホーム、概要、説明、ダウンロード、およびなど。 同様に、ページの下部には、 www.asp.net 、著作権、およびプライバシーに関する声明へのリンク上の広告に関する情報が含まれます。


[![www.asp.net web サイトは、すべてのページにわたって一貫したルック アンド フィールを採用しています](creating-a-site-wide-layout-using-master-pages-vb/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image1.png)

<strong>図 01</strong>: 一貫性のある検索とすべてのページ間で感じる www.asp.net web サイトを採用しています ([フルサイズの画像を表示する をクリックします](creating-a-site-wide-layout-using-master-pages-vb/_static/image3.png))。


適切に設計されたサイトの別の属性は、簡単にすると、サイトの外観を変更できます。 図 1 が 2008 年 3 月の時点で www.asp.net ホームページを示していますが、ルック アンド フィールをここで、このチュートリアルのパブリケーションの変更可能性があります。 おそらく上部のメニュー項目には、MVC フレームワークの新しいセクションが含まれますが展開されます。 いるか、おそらくさまざまな色、フォント、およびレイアウトをまったく新しいデザイン出現します。 このような変更をサイト全体に適用すると、高速で単純なプロセス、サイトを構成する web ページの何千もの変更を必要としない必要があります。

ASP.NET でサイト全体のページ テンプレートの作成を使用すると考えられる*マスター ページ*します。 簡単に言うと、マスター ページは、特殊な種類のすべての間で共通のマークアップを定義する ASP.NET ページ*コンテンツ ページ*はコンテンツ ページのコンテンツをページごとにカスタマイズ可能なリージョンとします。 (コンテンツ ページとは、マスター ページにバインドされている ASP.NET ページのことです)。マスター ページのレイアウトや書式設定が変更されるたびにすべてのコンテンツ ページの出力は同様にすぐに更新、更新し、1 つのファイル (つまり、マスター ページ) の配置と同じくらい簡単サイト全体の外観の変更を適用できます。

これは、マスター ページの使用方法について説明するチュートリアル シリーズの最初のチュートリアルです。 このチュートリアル シリーズの過程でします。

- マスター ページと、対応するコンテンツ ページの作成を確認します。
- さまざまなヒント、テクニック、およびトラップ、について説明します
- マスター ページのよくある落とし穴を特定し、回避策の詳細
- コンテンツ ページと逆からマスター ページにアクセスする方法を参照してください。
- 実行時に、コンテンツ ページのマスター ページを指定する方法について説明します、
- その他のマスター ページのトピックの詳細。

簡潔であり、プロセスを視覚的に順番に多数のスクリーン ショットの手順を説明には、これらのチュートリアルが適しています。 各チュートリアルは、c# および Visual Basic バージョンで使用可能なために使用する完全なコードのダウンロードが含まれています。

「このチュートリアルでは、マスター ページの基本を見てから始まります。 マスター ページと Visual Web Developer を使用して、関連付けられているコンテンツ ページの作成について見て、およびマスター ページへの変更がそのコンテンツのページですぐに反映される方法を参照してくださいマスター ページの動作について説明します。 それでは、始めましょう!

## <a name="understanding-how-master-pages-work"></a>マスター ページの動作を理解します。

サイト全体の一貫性のあるページのレイアウトでの web サイトを構築するには、各 web ページがそのカスタム コンテンツだけでなく共通の書式設定マークアップを生成することが必要です。 たとえば、 www.asp.net  の各チュートリアルまたはフォーラムの投稿では、独自の一意のコンテンツがありますが、これらの各ページも表示、一連の一般的な`<div>`の最上位のセクションのリンクを表示する要素: Home、開始、学習、およびなど。

さまざまな一貫したルック アンド フィールで web ページを作成するための手法があります。 未熟なアプローチは単純にコピーし、すべての web ページに共通のレイアウトのマークアップを貼り付けますが、この方法の欠点の数値。 まず、新しいページが作成されるたびに、コピーして、ページに、共有するコンテンツを貼り付けるを忘れないでください。 このようなコピーと貼り付け操作はエラー、誤っての新しいページに共有のマークアップのサブセットのみをコピーすることがあります。 するこのアプローチは、既存のサイト全体の外観を実際のペインの新しいルック アンド フィールを使用するには、サイト内のすべての単一ページを編集する必要があるため、新しいものに置き換えます。

ASP.NET version 2.0 では前のページで一般的なマークアップを配置することがよく開発者[ユーザー コントロール](https://msdn.microsoft.com/library/y6wb1a0e.aspx)され、各ページにこれらのユーザー コントロールを追加します。 ページの開発者は、すべての新しいページにユーザー コントロールを手動で追加するが、サイト全体の変更を簡単に変更する一般的なマークアップを更新するときにユーザー コントロールのみが必要なために、許可されて、このアプローチが必要です。 残念ながら、Visual Studio .NET 2002 および 2003 のバージョンの Visual Studio は、ASP.NET 1.x アプリケーションのユーザー コントロールをデザイン ビューで灰色のボックスとしてレンダリングの作成に使用します。 そのため、このアプローチを使用して、ページ開発者は、WYSIWYG デザイン時環境をお楽しみでしたされません。

ユーザー コントロールを使用する場合の欠点は、ASP.NET version 2.0 と Visual Studio 2005 での導入により解決された*マスター ページ*します。 マスター ページが特別な種類のサイトのマークアップを定義する ASP.NET ページと*リージョン*が関連付けられている*コンテンツ ページ*カスタムのマークアップを定義します。 手順 1. でよう、これらのリージョンは、プレース ホルダー コントロールによって定義されます。 プレース ホルダー コントロールは、単にカスタム コンテンツを挿入して、コンテンツ ページで、マスター ページのコントロール階層内の位置を表します。

> [!NOTE]
> ASP.NET version 2.0 以降には、主要な概念とマスター ページの機能が変更されていません。 ただし、Visual Studio 2008 は、Visual Studio 2005 で欠けている機能、入れ子になったマスター ページのデザイン時サポートを提供します。 今後のチュートリアルでは入れ子になったマスター ページを使用して紹介します。


図 2 は、 www.asp.net のマスター ページの外観を示しています。 マスター ページは、web ページごとの一意のコンテンツが配置されている中間、左、プレース ホルダーと - 上部、下部にあるとすべてのページの右側にマークアップの一般的なサイト全体レイアウトを定義に注意してください。


![マスター ページは、コンテンツ ページのコンテンツをページごとに、サイト全体レイアウトや編集可能な領域を定義します。](creating-a-site-wide-layout-using-master-pages-vb/_static/image4.png)

**図 02**: マスター ページは、コンテンツ ページのコンテンツをページごとに、サイト全体レイアウトや編集可能な領域を定義します。


マスター ページを定義した後は、チェック ボックスのティックで新しい ASP.NET ページにバインドできます。 コンテンツ ページと呼ばれる、これらの ASP.NET ページには、各マスター ページの ContentPlaceHolder のコントロールのコンテンツ コントロールが含まれます。 [コンテンツ] ページがブラウザーからアクセスしたときに、ASP.NET エンジンはマスター ページのコントロール階層を作成しは適切な場所にコンテンツ ページのコントロール階層を挿入します。 この結合されたコントロールの階層が表示され、結果の HTML がエンドユーザーのブラウザーに返されます。 その結果、コンテンツ ページは、そのマスター ページ、プレース ホルダー コントロールの外部で定義されている一般的なマークアップと、独自のコンテンツ コントロール内で定義されているページ固有のマークアップの両方を出力します。 図 3 は、この概念を示します。


[![要求されたページのマークアップは、マスター ページに組み合わされ、](creating-a-site-wide-layout-using-master-pages-vb/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image5.png)

**図 03**: マスター ページに、要求されたページのマークアップが組み合わされ ([フルサイズの画像を表示する をクリックします](creating-a-site-wide-layout-using-master-pages-vb/_static/image7.png))。


マスター ページの動作について説明しましたが、これでマスター ページと Visual Web Developer を使用して、関連付けられているコンテンツ ページの作成を見てをみましょう。

> [!NOTE]
> できるだけ多くを達成するために ASP.NET web サイトを構築するでは、このチュートリアル シリーズで作成 ASP.NET 3.5 と Visual Studio 2008 では、Microsoft の無料版[Visual Web Developer 2008](https://www.microsoft.com/express/vwd/)します。 持っていない - ご心配なく ASP.NET 3.5 にアップグレードする場合は、これらのチュートリアルの作業で説明する概念も同じように asp.net 2.0 と Visual Studio 2005。 ただし、一部のデモ アプリケーションが .NET framework version 3.5; の新機能を使用できます。3.5 に固有の機能を使用している場合は、バージョン 2.0 では、同様の機能を実装する方法について説明しますので、注意にが含まれます。 その結果、.NET Framework version 3.5 では、各チュートリアルの対象から使用可能なデモ アプリケーションをダウンロードするを忘れないでください、 `Web.config` 3.5 に固有の構成要素を含むファイル。 要約すると、ダウンロード可能な web アプリケーションでは、コンピューターに .NET 3.5 をインストールするがまだある場合は、最初から 3.5 固有のマークアップを削除しないでは機能しません`Web.config`します。 参照してください[詳細に分析する ASP.NET Version 3.5 の`Web.config`ファイル](http://www.4guysfromrolla.com/articles/121207-1.aspx)このトピックの詳細についてはします。


## <a name="step-1-creating-a-master-page"></a>手順 1: マスター ページの作成

作成とマスター ページやコンテンツ ページの使用を調査できる前にまず必要がありますの ASP.NET web サイト。 まず新しいファイル システムに基づく ASP.NET web サイトを作成します。 これを実現する Visual Web Developer を起動し、ファイル メニューに移動して、新しい Web サイト ダイアログを表示する新しい Web サイトを選択ボックス (図 4 参照)。 ASP.NET Web サイト テンプレートを選択、場所ドロップダウン リストをファイル システムに設定、web サイトを配置するフォルダーを選択および Visual Basic 言語に設定します。 これで新しい web サイトが作成されます、 `Default.aspx` ASP.NET ページ、`App_Data`フォルダーと`Web.config`ファイル。

> [!NOTE]
> Visual Studio プロジェクト管理の 2 つのモードをサポートしています。 Web サイト プロジェクトと Web アプリケーション プロジェクト。 Web サイト プロジェクトで Visual Studio .NET 2002年/2003 プロジェクト アーキテクチャを模倣する Web アプリケーション プロジェクト - プロジェクト ファイルが含まれていて、プロジェクトのソース コードに配置されている 1 つのアセンブリにコンパイルは、プロジェクト ファイルがない、 `/bin`フォルダー。 Visual Studio 2005 最初に唯一サポートされている Web サイト プロジェクトは、Service Pack 1。 Web アプリケーション プロジェクト モデルが再入がVisual Studio 2008 には、両方のプロジェクト モデルが用意されています。 Visual Web Developer 2005 および 2008 のエディション、ただし、のみがサポート Web サイト プロジェクト。 このチュートリアル シリーズで、デモの Web サイト プロジェクト モデルを使用します。 Express 以外のエディションを使用しているし、使用するかどうか、 [Web アプリケーション プロジェクト モデル](https://msdn.microsoft.com/library/aa730880(vs.80).aspx)代わりを自由に行う可能性があるいくつかの相違点、画面とではなく実行する必要があります手順に表示されるものとの間に注意してください、スクリーン ショットが示すように、これらのチュートリアルで説明する手順。


[![新しいファイル システムに基づく Web サイトを作成します。](creating-a-site-wide-layout-using-master-pages-vb/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image8.png)

**図 04**: New File System-Based Web サイトの作成 ([フルサイズの画像を表示する をクリックします](creating-a-site-wide-layout-using-master-pages-vb/_static/image10.png))。


次に、プロジェクト名を右クリックし、新しい項目の追加 を選択し、マスター ページ テンプレートを選択して、ルート ディレクトリ内のサイトにマスター ページを追加します。 マスター ページが、拡張子で終わることに注意してください。`.master`します。 この新しいマスター ページの名前`Site.master`追加 をクリックします。


[![マスター ページを追加するという名前の web サイトに Site.master](creating-a-site-wide-layout-using-master-pages-vb/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image11.png)

**図 05**: マスター ページの名前付きの追加`Site.master`web サイトに ([フルサイズの画像を表示する をクリックします](creating-a-site-wide-layout-using-master-pages-vb/_static/image13.png))。


Visual Web Developer では新しいマスター ページファイルを追加すると、次の宣言型マークアップでマスター ページが作成されます。

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample1.aspx)]

宣言型マークアップの最初の行は、 [ `@Master`ディレクティブ](https://msdn.microsoft.com/library/ms228176.aspx)します。 `@Master`ディレクティブと似ています、 [ `@Page`ディレクティブ](https://msdn.microsoft.com/library/ydy4x04a.aspx)ASP.NET ページに表示されます。 サーバー側の言語 (VB) と、マスター ページの分離コード クラスの継承と場所に関する情報を定義します。

`DOCTYPE`ページの宣言型マークアップは、下に表示し、`@Master`ディレクティブ。 このページには、4 つのサーバー側コントロールと共に、静的な HTML が含まれます。

- **Web フォーム (、 `<form runat="server">`)** - すべての ASP.NET ページは通常の Web フォームがあるし、ため、マスター ページは、Web フォームで囲む必要があります Web コントロールを含めることができますが、マスター ページ (ではなく電子メールに、Web フォームを追加する Web フォームを追加することを確認するためach コンテンツ ページ)。
- **という名前のプレース ホルダー コントロール`ContentPlaceHolder1`**  -このプレース ホルダー コントロールが Web フォーム内で表示され、コンテンツ ページのユーザー インターフェイスの領域として機能します。
- **サーバー側`<head>`要素**-`<head>`要素には、`runat="server"`属性に、サーバー側コードからアクセスできるようにします。 `<head>`要素はこのように実装されているようにページのタイトル、およびその他の`<head>`-関連マークアップを追加またはプログラムで調整された可能性があります。 たとえば、ASP.NET ページの設定`Title`プロパティの変更、`<title>`によってレンダリングされる要素、`<head>`サーバー コントロール。
- **という名前のプレース ホルダー コントロール`head`**  -内でこのプレース ホルダー コントロールが表示されます、`<head>`サーバーを制御し、宣言的にコンテンツを追加するために使用できる、`<head>`要素。

この既定のマスター ページの宣言型マークアップは、マスター ページを設計するための開始点として機能します。 自由に HTML を編集する、またはその他の Web コントロールまたは ContentPlaceHolders マスター ページを追加します。

> [!NOTE]
> マスター ページを設計することを確認し、マスター ページには、Web フォームが含まれています。 この Web フォーム内でその少なくとも 1 つのプレース ホルダー コントロールに表示されます。


### <a name="creating-a-simple-site-layout"></a>単純なサイト レイアウトを作成します。

展開しましょう`Site.master`のすべてのページが共有サイト レイアウトを作成する既定の宣言型マークアップ: 一般的なヘッダーは左側のナビゲーション、ニュースとその他のサイトのコンテンツとフッターの"Microsoft ASP.NET での電源"アイコンを表示します。 図 6 は、ブラウザーでそのコンテンツ ページのいずれかを表示するときに、マスター ページの最終結果を示します。 図 6 に赤い丸のリージョンにアクセスしたページに固有です (`Default.aspx`)。 その他のコンテンツは、マスター ページで定義され、したがって一貫性のあるすべてのコンテンツ ページ。


[![マスター ページは、上、左、および下の部分のマークアップを定義します](creating-a-site-wide-layout-using-master-pages-vb/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image14.png)

**図 06**: マスター ページは、上、左、および下の部分のマークアップを定義します ([フルサイズの画像を表示する をクリックします](creating-a-site-wide-layout-using-master-pages-vb/_static/image16.png))。


図 6 に示すようにサイトのレイアウトを実現するために更新することで開始、`Site.master`マスター ページを次の宣言型マークアップを格納できるようにします。

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample2.aspx)]

マスター ページのレイアウトで定義された一連の`<div>`HTML 要素。 `topContent` `<div>` 、各ページの上部に表示されるマークアップを格納中に、 `mainContent`、 `leftContent`、および`footerContent``<div>`ページのコンテンツ、左の列と、"電源で Microsoft を表示するために使用されますASP.NET"アイコンでは、それぞれします。 これらを追加するだけでなく`<div>`要素の場合も名前、`ID`からプライマリのプレース ホルダー コントロールのプロパティ`ContentPlaceHolder1`に`MainContent`します。

これらのさまざまな書式設定とレイアウト規則`<div>`に要素が記述された、[カスケード スタイル シート (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets)ファイル`Styles.css`、経由で指定される、`<link>`マスター ページの要素`<head>`要素。 これらのさまざまなルールは、それぞれの外観を定義`<div>`要素上でメモします。 など、 `topContent` `<div>` 「マスター ページのチュートリアル」のテキストとリンクが表示されたらの要素がで指定された書式ルール`Styles.css`次のようにします。

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample3.css)]

このチュートリアルの付属のコードをダウンロードし、追加する必要があります、コンピューターに従う場合、`Styles.css`ファイルをプロジェクト。 という名前のフォルダーを作成する必要も、同様に、`Images`し、ダウンロードしたデモ web サイトから"Microsoft ASP.NET での電源"アイコンをプロジェクトにコピーします。

> [!NOTE]
> CSS と web ページの書式設定の詳細については、この記事の範囲を超えてです。 チェック アウトの詳細については、CSS、 [CSS チュートリアル](http://www.w3schools.com/css/default.asp)で[W3Schools.com](http://www.w3schools.com/)します。 このチュートリアルの付属のコードをダウンロードして、CSS の設定で再生することも`Styles.css`さまざまな書式指定規則の効果を確認します。


### <a name="creating-a-master-page-using-an-existing-design-template"></a>既存のデザイン テンプレートを使用してマスター ページを作成します。

長年にわたり数小規模から中規模企業用の ASP.NET web アプリケーションを構築しました。 一部のクライアントが、;、使用する既存のサイトのレイアウト他のユーザーには、有能なグラフィックス デザイナーとして採用されました。 いくつか委託 web サイトのレイアウトをデザインすることになります。 図 6 でわかるとおり、web サイトのレイアウトをデザインするプログラマのマルチタスクが通常、医師では、税 open-heart surgery 実行、会計士を持つものとして同程度に賢明です。

さいわい、無料の HTML デザイン テンプレート innumerous の web サイトがある - Google では、検索語句「無料の web サイト テンプレートです」600万件以上の結果が返されました 私の好きなものの 1 つは[OpenDesigns.org](http://opendesigns.org/)します。たい web サイト テンプレートを見つけたら、CSS ファイルとイメージを web サイト プロジェクトに追加し、マスター ページに、テンプレートの HTML を統合します。

> [!NOTE]
> Microsoft はいくつかありますも[ASP.NET デザイン スタート キット テンプレートを無料](https://msdn.microsoft.com/asp.net/aa336613.aspx)Visual Studio で新しい Web サイト ダイアログ ボックスに統合されます。


## <a name="step-2-creating-associated-content-pages"></a>手順 2: コンテンツ ページは関連付けを作成します。

作成、マスター ページとマスター ページにバインドされている ASP.NET ページの作成を開始する準備が整いました。 このようなページとして参照されます*コンテンツ ページ*します。

新しい ASP.NET ページをプロジェクトに追加してにバインドしましょう、`Site.master`マスター ページ。 ソリューション エクスプ ローラーでプロジェクト名を右クリックし、新しい項目の追加オプションを選択します。 Web フォーム テンプレートを選択して、名前を入力します`About.aspx`図 7 に示すように「マスター ページの選択」チェック ボックスを確認します。 これによりダイアログが表示されます、Select マスター ページ (図 8 参照) をボックスから使用するマスター ページを選択できます。

> [!NOTE]
> Web サイト プロジェクト モデルではなく、Web アプリケーション プロジェクト モデルを使用して ASP.NET web サイトを作成した場合は、図 7 に示すように新しい項目の追加 ダイアログ ボックスの"マスター ページの選択 チェック ボックスは表示されません。 コンテンツを作成するには、ページで、Web アプリケーション プロジェクトを使用してモデルは、Web フォーム テンプレートではなく、Web コンテンツ フォーム テンプレートを選択してください。 Web コンテンツ フォーム テンプレートを選択し、追加をクリックすると、図 8 ダイアログ ボックスが表示されますマスター ページの選択と同じです。


[![新しいコンテンツ ページを追加します。](creating-a-site-wide-layout-using-master-pages-vb/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image17.png)

**図 07**: 新しいコンテンツ ページの追加 ([フルサイズの画像を表示する をクリックします](creating-a-site-wide-layout-using-master-pages-vb/_static/image19.png))。


[![Site.master マスター ページを選択します。](creating-a-site-wide-layout-using-master-pages-vb/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image20.png)

**図 08**: 選択、`Site.master`マスター ページ ([フルサイズの画像を表示する をクリックします](creating-a-site-wide-layout-using-master-pages-vb/_static/image22.png))。


新しいコンテンツ ページには、次の宣言型マークアップに示すよう、`@Page`ことを指すマスター ページとコンテンツ コントロール、マスター ページの ContentPlaceHolder のコントロールの各ディレクティブ。

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample4.aspx)]

> [!NOTE]
> 名前「を作成する、単純なサイト レイアウト」セクションで手順 1. で`ContentPlaceHolder1`に`MainContent`します。 このプレース ホルダー コントロールの名前を変更するがない場合`ID`同様に、コンテンツ ページの宣言型マークアップが少し異なります、マークアップの前に示したからです。 つまり、2 つ目コンテンツ コントロールの`ContentPlaceHolderID`が反映されます、`ID`マスター ページの対応するプレース ホルダーを制御します。


ページのコンテンツ ページを表示するときに、ASP.NET エンジンする必要があります fuse のコントロールをそのマスター ページの ContentPlaceHolder のコントロールのコンテンツします。 ASP.NET エンジンからのコンテンツ ページのマスター ページを決定する、`@Page`ディレクティブの`MasterPageFile`属性。 このコンテンツ ページが連結、上記のマークアップに示す`~/Site.master`します。

マスター ページが 2 つのプレース ホルダー コントロール -`head`と`MainContent`-Visual Web Developer は、2 つのコンテンツ コントロールを生成します。 各コンテンツ コントロールを使用して特定のプレース ホルダーを参照してその`ContentPlaceHolderID`プロパティ。

ここで、デザイン時サポート、サイト全体のテンプレートの以前の技術上のマスター ページ光沢です。 図 9 は、 `About.aspx` Visual Web Developer のデザイン ビューで表示した場合、コンテンツ ページです。 マスター ページのコンテンツが表示されることに注意してください、これは淡色表示と変更ことはできません。 マスター ページの ContentPlaceHolders に対応するコンテンツ コントロールは、ただし、編集できます。 同様の他の ASP.NET ページでソースまたはデザイン ビューを通じて Web コントロールを追加して、コンテンツ ページのインターフェイスを作成できます。


[![コンテンツ ページの [デザイン] ビューは、両方の特定のページとマスター ページ内容を表示します。](creating-a-site-wide-layout-using-master-pages-vb/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image23.png)

**図 09**: The のコンテンツ ページのデザイン ビューが表示されます、ページ固有と両方マスター ページの内容 ([フルサイズの画像を表示する をクリックします](creating-a-site-wide-layout-using-master-pages-vb/_static/image25.png))。


### <a name="adding-markup-and-web-controls-to-the-content-page"></a>コンテンツのページにマークアップおよび Web コントロールを追加します。

一部のコンテンツを作成する少し、`About.aspx`ページ。 図 10 で、"について、Author"見出しと、いくつかの段落のテキストを入力したを参照してください。 が、自由にも、Web コントロールを追加できます。 このインターフェイスを作成した後、次を参照してください。、`About.aspx`ページがブラウザーを使用します。


[![ブラウザーを使用して About.aspx ページを参照してください。](creating-a-site-wide-layout-using-master-pages-vb/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image26.png)

**図 10**: を参照してください、 `About.aspx` 、ブラウザーでページ ([フルサイズの画像を表示する をクリックします](creating-a-site-wide-layout-using-master-pages-vb/_static/image28.png))。


要求されたコンテンツ ページと関連付けられているマスター ページが組み合わされ、し、web サーバーに完全に全体として表示されることを理解しておく必要があります。 エンドユーザーのブラウザーで、結果として得られる、融合型の HTML が送信されます。 これを確認するには、[表示] メニューに移動し、ソースを選択して、ブラウザーが受信した HTML を表示します。 フレームなしまたは 1 つのウィンドウに 2 つの異なる web ページを表示するための他の特殊な手法ことに注意してください。

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>既存の ASP.NET ページへのマスター ページのバインド

この手順で説明したようには、"マスター ページの選択 チェック ボックスをオンし、マスター ページの選択と同じくらい簡単では新しいコンテンツ ページを ASP.NET web アプリケーションに追加します。 残念ながら、既存の ASP.NET ページをマスター ページに変換するは、簡単ではありません。

マスター ページを既存の ASP.NET ページにバインドするには、次の手順を実行する必要があります。

1. 追加、`MasterPageFile`属性を ASP.NET ページの`@Page`ディレクティブ、適切なマスター ページを指定します。
2. マスター ページの ContentPlaceHolders の各コンテンツ コントロールを追加します。
3. 選択的にカットし、既存の ASP.NET ページのコンテンツを適切なコンテンツ コントロールに貼り付けます。 ASP.NET ページの可能性がありますのででなど表現、マスター ページに既にマークアップが含まれています「選択的に」ここで言って、 `DOCTYPE`、`<html>`要素、および Web フォーム。

スクリーン ショットと共にこのプロセスの詳細については、チェック アウト[Scott Guthrie](https://weblogs.asp.net/scottgu/)の[を使用してマスター ページとサイト ナビゲーション](http://webproject.scottgu.com/VisualBasic/MasterPages/MasterPages.aspx)チュートリアル。 "Update`Default.aspx`と`DataSample.aspx`マスター ページを使用する"セクションでは、次の手順をについて説明します。

コンテンツ ページに既存の ASP.NET ページを変換するよりも新しいコンテンツ ページを作成する簡単なので、作成するたびに新しい ASP.NET web サイト マスター ページを追加、サイトにお勧めします。 このマスター ページには、すべての新しい ASP.NET ページをバインドします。 最初のマスター ページが非常に単純なまたはプレーンな; である場合、心配しないでください。後でマスター ページを更新することができます。

> [!NOTE]
> Visual Web Developer を追加、新しい ASP.NET アプリケーションを作成するときに、`Default.aspx`マスター ページにバインドされていないページ。 さあ、使用の演習用の既存の ASP.NET ページのコンテンツ ページに変換する場合は、`Default.aspx`します。 または、削除`Default.aspx`「マスター ページの選択」 をチェックします。 この時間が、再度追加します。


## <a name="step-3-updating-the-master-pages-markup"></a>手順 3: マスター ページのマークアップを更新しています

マスター ページの主な利点の 1 つは、単一のマスター ページは、サイトで多数のページの全体的なレイアウトを定義する使用可能性があります。 そのため、サイトの外観を更新するには、1 つのファイルのマスター ページを更新が必要です。

この動作を示すためには、更新、マスター ページで、左側の列の上部にある現在の日付が含まれます。 という名前のラベルを追加`DateDisplay`を`leftContent``<div>`します。

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample5.aspx)]

次に、作成、`Page_Load`イベント ハンドラーをマスター ページし、次のコードを追加します。

[!code-vb[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample6.vb)]

上記のコードの設定、ラベルの`Text`プロパティを現在の日付と時間、曜日、月、および 2 桁の日の名前として書式設定されます (図 11 を参照してください)。 この変更により、ページのいずれかを見直します。 図 11 に示すように、マスター ページに変更が含まれて、結果として得られるマークアップが更新されます。


[![マスター ページへの変更が反映されるときに表示する、コンテンツ ページ](creating-a-site-wide-layout-using-master-pages-vb/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image29.png)

**図 11**: マスター ページに、変更が反映されるときに表示する、コンテンツ ページ ([フルサイズの画像を表示する をクリックします](creating-a-site-wide-layout-using-master-pages-vb/_static/image31.png))。


> [!NOTE]
> この例に示すように、マスター ページはサーバー側の Web コントロール、コード、およびイベント ハンドラーを含めることができます。


## <a name="summary"></a>まとめ

マスター ページでは、ASP.NET 開発者が簡単に更新可能な一貫性のあるサイト全体レイアウトのデザインを有効にします。 Visual Web Developer が豊富なデザイン時サポートを提供できるとは、マスター ページと、対応するコンテンツ ページを作成すると標準の ASP.NET ページを作成するだけです。

このチュートリアルで作成したマスター ページの例では、2 つのプレース ホルダー コントロール、ヘッド ノード、および MainContent 必要があります。 のみに指定した MainContent プレース ホルダー コントロールのマークアップをコンテンツのページでは、ただしします。 次のチュートリアルでは、複数のコンテンツを使用して見てコンテンツ ページ内のコントロール。 既定値を定義する方法もわかりますコンテンツのマークアップ内で制御マスター ページも既定値の使用を切り替えるマークアップで定義されているマスター ページと [コンテンツ] ページからカスタムのマークアップを提供する方法。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [デザイナー向けの ASP.NET: 無料の Web 標準を使用して ASP.NET web サイトの構築にデザイン テンプレートとガイダンス](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [ASP.NET マスター ページの概要](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [カスケード スタイル シート (CSS) のチュートリアル](http://www.w3schools.com/css/default.asp)
- [ページのタイトルを動的に設定](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [ASP.NET のマスター ページ](http://www.odetocode.com/articles/419.aspx)
- [マスター ページのクイック スタート チュートリアル](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)作成者の複数受け取りますブックと 4GuysFromRolla.com の創設者で、携わって Microsoft Web テクノロジ 1998 年からです。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 3.5 in 24 時間*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別なに感謝します。

今後、MSDN の記事を確認したいですか。 場合は、筆者に[ mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)します。

> [!div class="step-by-step"]
> [前へ](nested-master-pages-cs.md)
> [次へ](multiple-contentplaceholders-and-default-content-vb.md)
