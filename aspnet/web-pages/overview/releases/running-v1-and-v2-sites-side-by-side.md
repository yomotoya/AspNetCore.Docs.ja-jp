---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: 異なるバージョンの ASP.NET Web Pages (Razor) サイド バイ サイド実行 |Microsoft Docs
author: tfitzmac
description: この記事では、さまざまなバージョンを使用する web サイトが構成されている場合、同じコンピューターまたはサーバーで ASP.NET Web Pages (Razor) の web サイトを実行する方法について説明しています.
ms.author: aspnetcontent
ms.date: 02/10/2014
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 86b1d5babac747eb4fa7ba8abde0e6155c8b17fb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810057"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>ASP.NET Web Pages (Razor) のさまざまなバージョンを並行して実行しています。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、異なるバージョンの ASP.NET Web ページを使用する web サイトが構成されている場合、同じコンピューターまたはサーバーで ASP.NET Web Pages (Razor) の web サイトを実行する方法について説明します。
> 
> 学習内容。
> 
> - 既定の動作は ASP.NET で ASP.NET Web Pages を使用して構築されたサイトがある場合です。
> - 以前のバージョンの ASP.NET Web ページで実行する新しいサイトを構成する方法。
>   
> 
> これは、この記事で導入された ASP.NET の機能です。
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
> このチュートリアルは、ASP.NET Web Pages 2 および ASP.NET Web Pages 1.0 により連携します。


ASP.NET Web ページには、サイド バイ サイドの web サイトを実行する機能がサポートされています。 これにより、古いバージョンの ASP.NET Web Pages アプリケーションを実行し、新しい ASP.NET Web Pages アプリケーションの構築など、いずれも、同じコンピューター上で実行を続行できます。

WebMatrix を使用した Web ページをインストールするときの注意点を次に示します。

- 既定では、既存の Web ページ アプリケーションは、コンピューターに最新バージョンとして実行されます。 (アセンブリはグローバル アセンブリ キャッシュ (GAC) がインストールされ自動的に使用されます)。
- 異なるバージョンの ASP.NET Web ページを使用してサイトを実行する場合は、そのためには、サイトを構成できます。 サイトを持っていない場合、 *web.config*サイトのルートにファイル、新しいものを作成および既存のコンテンツを上書きするのには、次の XML をコピーします。 サイトが既に含まれている場合、 *web.config*ファイルを追加、`<appSettings>`要素に次のいずれかのように、`<configuration>`セクション。

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  '-で、バージョンを指定しない場合、 *web.config*ファイル、サイトは、最新バージョンとして配置されます。 (、アセンブリにコピー、 *bin*デプロイされたサイトのフォルダーです)。
- Web Matrix でサイト テンプレートを使用して作成する新しいアプリケーションが、サイトの Web ページのバージョンのアセンブリを含める*bin*フォルダー。

サイトに適切なアセンブリをインストールする NuGet を使用して、サイトで使用する Web ページのバージョンを常に制御する一般に、 *bin*フォルダー。 パッケージを検索するには、次を参照してください。 [NuGet.org](http://NuGet.org)します。

## <a name="additional-resources"></a>その他のリソース

[ASP.NET Web Pages 2 の主な機能](top-features-in-web-pages-2.md)
