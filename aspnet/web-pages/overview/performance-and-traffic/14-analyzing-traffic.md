---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: ASP.NET Web Pages (Razor) サイトの訪問者の情報 (分析) の追跡 |Microsoft Docs
author: Rick-Anderson
description: Web サイトから取得した後、web サイト トラフィックを分析する可能性があります。
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 57e6a0d4681f147faa5e9ca3b6ed0ef287d6a381
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020885"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>ASP.NET Web Pages (Razor) サイトの訪問者情報 (分析) の追跡
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ヘルパーを使用して ASP.NET Web Pages (Razor) の web サイトのページに web サイト分析を追加する方法について説明します。
> 
> 学習内容。
> 
> - 分析プロバイダーに、web サイト トラフィックに関する情報を送信する方法。
> 
> この記事で導入された機能のプログラミング、ASP.NET を次に示します。
> 
> - `Analytics`ヘルパー。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - ASP.NET Web Helpers Library (NuGet パッケージ)


Analytics は、ユーザーが、サイトを使用する方法を理解できるように、web サイト上のトラフィックを測定するテクノロジの一般的な用語です。 多くの分析サービスは、使用可能な Google、Yahoo、StatCounter、およびその他のユーザーからサービスを含みます。

追跡することへのサインアップ、アカウント分析プロバイダーは、サイトを登録することはように分析が必要。プロバイダーでは、ID またはアカウントのコードの追跡が含まれる JavaScript コードのスニペットを送信します。 JavaScript のスニペットを追跡しサイト上の web ページに追加します。(通常、スニペットを追加する analytics フッターまたはレイアウト ページまたはサイト内の各ページに表示されるその他の HTML マークアップ。)ユーザーは、これらの JavaScript スニペットのいずれかを含むページを要求するときに、スニペットは分析プロバイダーは、ページのさまざまな詳細を記録するに、現在のページに関する情報を送信します。

サイトの統計情報を確認する場合は、分析プロバイダーの web サイトにログインします。 ように、サイトに関する、あらゆる種類のレポートを表示できます。

- 個々 のページのページ ビューの数。 これは、(ほぼ) 多くの人たちが、サイトにアクセスして、サイトでは、どのページが最も人気のあります。
- どのくらいの期間のユーザーは、特定のページに費やしています。 ホーム ページがユーザーの関心を維持するかどうかなどが確認できます。
- どのようなサイトのユーザーは、サイトを訪問する前ででした。 これにより、検索、およびなどからのリンクから、トラフィックが送信されるかどうかを理解することができます。
- ユーザーが、サイトを訪問するときと期間します。
- どのような国の利用者はからです。
- どのようなブラウザーとオペレーティング システム、訪問者が使用しています。

    ![Ch14traffic 1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>ヘルパーを使用して分析ページを追加するには

ASP.NET Web ページには、いくつかの analytics ヘルパーが含まれています (`Analytics.GetGoogleHtml`、 `Analytics.GetYahooHtml`、および`Analytics.GetStatCounterHtml`) を簡単に分析するための JavaScript スニペットを管理します。 方法と場所を把握するのではなく、JavaScript コードを配置する行う必要があるすべてがページに、ヘルパーを追加します。 唯一の情報を提供する必要があるは、アカウント名、ID、または追跡コードです。 (StatCounter、するも指定する必要あります、いくつか追加の値。)

この手順では、使用するレイアウト ページを作成します、`GetGoogleHtml`ヘルパー。 その他の分析プロバイダーのいずれかのアカウントを既にがある場合は、代わりにそのアカウントを使用し、必要に応じて、若干の調整を行います。

> [!NOTE]
> Analytics アカウントを作成するときにをトラッキングするサイトの URL を登録します。 (トラフィックのみユーザーは)、実際のトラフィックが追跡するされない場合は、ローカル コンピューター上のすべてをテストするため、サイトの統計情報を記録および表示することはできません。 この手順は、ページを analytics ヘルパーを追加する方法を示します。 サイトを発行するときに、ライブ サイトは、分析プロバイダーに情報が送信されます。


1. 」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)、既に追加していない場合。
2. Google アナリティクスでアカウントを作成し、アカウント名を記録します。
3. という名前のレイアウト ページを作成する*Analytics.cshtml*し、次のマークアップを追加します。

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > 呼び出しを配置する必要があります、 `Analytics` 、web ページの本文でヘルパー (の前に、`</body>`タグ)。 それ以外の場合、ブラウザーでは、スクリプトは実行されません。

    別の分析プロバイダーを使用している場合は、代わりに使用のヘルパーを次のいずれか。

    - (Yahoo) `@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`
4. 置換`myaccount`アカウント、ID、または手順 1. で作成した追跡コードの名前に置き換えます。
5. ブラウザーでページを実行します。 (内でページが選択されていることを確認、**ファイル**ワークスペースを実行する前にします)。
6. ブラウザーでページ ソースを表示します。 レンダリングされた分析コードを表示することができます。

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Google アナリティクスのサイトにログオンし、サイトの統計を確認します。 ライブ サイトでページを実行している場合、ページへのアクセス ログに記録するエントリを参照してください。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース

- [Google Analytics サイト](https://www.google.com/analytics/)
- [Yahoo!サイトを Web Analytics](http://help.yahoo.com/l/us/yahoo/ywa/)
- [StatCounter サイト](http://statcounter.com/)
