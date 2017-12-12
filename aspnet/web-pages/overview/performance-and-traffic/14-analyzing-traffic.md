---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: "ASP.NET Web Pages (Razor) サイトの訪問者情報 (分析) の追跡 |Microsoft ドキュメント"
author: tfitzmac
description: "予定 web サイトを取得した、後に、web サイト トラフィックを分析することができます。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>ASP.NET Web Pages (Razor) サイトの訪問者情報 (分析) の追跡
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ヘルパーを使用して ASP.NET Web Pages (Razor) の web サイトのページに web サイトの分析機能を追加する方法について説明します。
> 
> 学習する内容。
> 
> - 分析プロバイダーを web サイト トラフィックに関する情報を送信する方法です。
> 
> これらは、ASP.NET のアーティクルで導入された機能をプログラミングします。
> 
> - `Analytics`ヘルパー。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - ASP.NET Web Helpers Library (NuGet パッケージ)


分析は、ユーザーが、サイトを使用する方法を理解できるように、web サイト上のトラフィックを測定するテクノロジの一般的な用語です。 分析サービスの多くは、使用可能な Google、Yahoo、StatCounter、およびその他のユーザーからのサービスを含むです。

サインアップするアカウントのプロバイダーでは、分析、サイトを登録することはように分析は、追跡するためにします。プロバイダーでは、ID または追跡、アカウントのコードを含む JavaScript コードのスニペットを送信します。 JavaScript スニペットを追跡する、サイト上の web ページに追加します。(通常、analytics スニペットを追加、ページ フッターまたはレイアウト ページや、サイト内の各ページに表示される HTML マークアップ。)ユーザーは、これらの JavaScript スニペットのいずれかを含むページを要求するときにスニペットは、ページのさまざまな詳細を記録するユーザーの分析プロバイダーを現在のページに関する情報を送信します。

サイトの統計情報を参照する場合は、分析プロバイダーの web サイトにログインします。 同様に、サイトに関する、あらゆる種類のレポートを表示できます。

- 個々 のページのページ ビューの数。 これは、通知するユーザーの数 (約) は、サイト、サイトのページが、最も人気の高いです。
- どのくらいの期間ユーザーが特定のページに短縮されます。 ホーム ページがユーザーの関心を維持するかどうかのようなこれ指示できます。
- どのようなサイトのユーザーは、サイトにアクセスする前に載っていました。 これにより、検索、およびなどからのリンクから、トラフィックが送られているかどうかを理解することができます。
- ユーザーが、サイトを訪問するときと期間します。
- どのような国の利用者はからです。
- どのようなブラウザーとオペレーティング システム、利用者が使用されています。

    ![Ch14traffic 1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>ヘルパーを使用して、ページに分析を追加するには

ASP.NET Web ページには、いくつかの分析ヘルパーが含まれています (`Analytics.GetGoogleHtml`、 `Analytics.GetYahooHtml`、および`Analytics.GetStatCounterHtml`) を簡単に分析のために使用する JavaScript スニペットを管理します。 方法と場所を見つけ出しの代わりに、JavaScript コードを配置する行う必要があるすべてがヘルパーをページに追加します。 提供する必要があります。 情報は、アカウント名、ID、または追跡コードです。 (StatCounter にも指定する必要がいくつかの追加の値です。)

この手順で使用するレイアウト ページを作成、`GetGoogleHtml`ヘルパー。 その他の分析プロバイダーのいずれかのアカウントを既にがある場合は、代わりに、そのアカウントを使用してし、必要に応じて若干調整します。

> [!NOTE]
> Analytics アカウントを作成するときに、追跡する場合、サイトの URL を登録します。 (トラフィックのみユーザーは)、実際のトラフィックが追跡するされない場合は、ローカル コンピューター上のすべてをテストしている、ため、サイトの統計情報を記録および表示することはできません。 この手順は、ページに分析ヘルパーを追加する方法を示しています。 サイトを発行するときに、実際のサイトは、分析プロバイダーに情報が送信されます。


1. 説明に従って、ASP.NET Web Helpers Library を web サイトに追加[ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)、既に追加していない場合。
2. Google アナリティクスでアカウントを作成し、アカウント名を記録します。
3. 名前付きレイアウト ページを作成する*Analytics.cshtml*し、次のマークアップを追加します。

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > 呼び出しを配置する必要があります、 `Analytics` web ページの本文のヘルパー (前に、`</body>`タグ)。 それ以外の場合、ブラウザーでは、スクリプトは実行されません。

    別の分析プロバイダーを使用している場合は、代わりに使用、次のヘルパーのいずれか。

    - (Yahoo)`@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`
4. 置き換える`myaccount`アカウント、ID、または手順 1. で作成した追跡コードの名前に置き換えます。
5. ブラウザーでページを実行します。 (ページが選択されて、必ず、**ファイル**ワークスペースを実行する前にします)。
6. ブラウザーでページ ソースを表示します。 レンダリングされた分析コードを確認することができます。

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Google アナリティクス サイトにログオンし、サイトの統計情報を確認します。 実際のサイトでページを実行した場合、ページへのアクセスをログに記録されているエントリを参照してください。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース

- [Google アナリティクス サイト](https://www.google.com/analytics/)
- [Yahoo!Web サイトの分析](http://help.yahoo.com/l/us/yahoo/ywa/)
- [StatCounter サイト](http://statcounter.com/)
