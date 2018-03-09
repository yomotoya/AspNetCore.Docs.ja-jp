---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: "異なるバージョンの ASP.NET Web Pages (Razor) サイド バイ サイド実行 |Microsoft ドキュメント"
author: tfitzmac
description: "この記事では、さまざまなバージョンを使用する web サイトが構成されている場合、同じコンピューターまたはサーバーの ASP.NET Web Pages (Razor) web サイトを実行する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: c11399b0bde59d18fa378ed48c15844454c1f956
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>ASP.NET Web Pages (Razor) のさまざまなバージョンをサイド バイ サイドで実行されています。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、異なるバージョンの ASP.NET Web Pages を使用する web サイトが構成されている場合、同じコンピューターまたはサーバーの ASP.NET Web Pages (Razor) web サイトを実行する方法について説明します。
> 
> 学習する内容。
> 
> - どのような既定の動作は ASP.NET で構築された ASP.NET Web Pages をサイトがある場合です。
> - 古いバージョンの ASP.NET Web Pages を使用して実行する新しいサイトを構成する方法。
>   
> 
> これは、アーティクルで導入された ASP.NET の機能です。
> 
> - `webPages:Version`構成設定。
>   
> 
> ## <a name="software-versions"></a>ソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 と ASP.NET Web Pages 1.0 でも動作します。


ASP.NET Web ページには、サイド バイ サイドの web サイトを実行する機能がサポートされています。 これにより、古いバージョンの ASP.NET Web Pages アプリケーションを実行、新規の ASP.NET Web Pages アプリケーションをビルドおよびそれらのすべてを同じコンピューターで実行し続けることができます。

WebMatrix で Web ページをインストールする際にいくつかの点を次に示します。

- 既定では、既存の Web Pages アプリケーションは、コンピューターに最新バージョンとして実行されます。 (アセンブリがグローバル アセンブリ キャッシュ (GAC) にインストールされているし、自動的に使用されます。)
- 別のバージョンの ASP.NET Web Pages を使用してサイトを実行する場合を行うには、サイトを構成することができます。 サイトを持っていない場合、 *web.config*サイトのルートにファイル、新たに作成し、既存のコンテンツを上書きするのには、次の XML をコピーします。 サイトが既に含まれている場合、 *web.config*ファイルに追加し、`<appSettings>`要素に次のいずれかのように、`<configuration>`セクションです。

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
'-でバージョンが指定されていない場合、 *web.config*ファイル、サイトの最新バージョンとして配置します。 (、アセンブリにコピー、 *bin*配置済みのサイトのフォルダーです)。
- Web マトリックス内のサイト テンプレートを使用して作成する新しいアプリケーションが、サイトの Web ページ バージョンのアセンブリを含める*bin*フォルダーです。

NuGet を使用して、サイトに適切なアセンブリをインストールすることによって、サイトで使用する Web ページのバージョンを常に制御する一般に、 *bin*フォルダーです。 パッケージを検索するには、次を参照してください。 [NuGet.org](http://NuGet.org)です。

## <a name="additional-resources"></a>その他のリソース

[ASP.NET Web Pages 2 で、上位の特徴](top-features-in-web-pages-2.md)
