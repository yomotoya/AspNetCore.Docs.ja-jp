---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: ページ (Razor) サイトを ASP.NET Web で読み取り可能な Url を作成する |Microsoft Docs
author: Rick-Anderson
description: この記事では、ルーティングでは、ASP.NET Web Pages (Razor) の web サイト、およびこの使用がより読みやすく、優れた SEO の Url を使用する方法について説明します。 何を学習しています.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 26d8f94b2e38fe5205a37e3d37b4e3bd509a3874
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021083"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web Pages (Razor) サイトでわかりやすい Url を作成します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ルーティングでは、ASP.NET Web Pages (Razor) の web サイト、およびこの使用がより読みやすく、優れた SEO の Url を使用する方法について説明します。
> 
> 学習内容。
> 
> - どの ASP.NET ルーティングを使用してより読みやすく、検索可能な Url を使用することができます。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも機能します。


## <a name="about-routing"></a>ルーティングについて

サイト内のページの Url には、どの程度、サイトの動作に影響を与えることができます。 URL の&quot;フレンドリ&quot;サイトを使用するユーザーを簡単にできるようにします。 サイトの検索エンジン最適化 (SEO) にも役立ちます。 ASP.NET web サイトには、フレンドリな Url を自動的に使用する機能が含まれます。

ASP.NET では、サーバー上のファイルを指すだけではなくユーザーの操作について説明するわかりやすい Url を作成できます。 架空のブログを考慮して Url。

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

次の Url を比較します。

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

最初のペアでユーザーが を使用して、ブログを表示することを確認する必要があります、 *blog.cshtml*ページ、および適切なカテゴリまたは日付範囲を取得するクエリ文字列を生成する必要があります、します。 2 つ目の例は、理解して、作成にはるかに簡単です。

最初の例の Url が特定のファイルを直接ポイントも (*blog.cshtml*)。 何らかの理由、ブログはサーバーで、移動を別のフォルダーが、ブログは、別のページを使用する書き換えられた場合や、リンクが正しくないでしょう。 Url の 2 番目のセットは、特定のページを指していませんブログ実装または場所が変更された場合でもそのため、Url が有効になります。

ASP.NET Web Pages で ASP.NET が使用するため、上記の例のようなわかりやすい Url を作成できます*ルーティング*します。 ルーティングは、要求を満たすことができますをページ (またはページ) への URL から論理マッピングを作成します。 マッピングは論理ため (物理、特定のファイルを)、ルーティング、サイトの Url を定義する方法の柔軟性を提供します。

## <a name="how-routing-works"></a>ルーティングの動作

ASP.NET 要求を処理するときにルーティングする方法を特定する URL を読み取ります。 ASP.NET は、左から右へと向かうディスク上のファイルへの URL の個別のセグメントの一致を試みます。 一致がある場合、URL に残っているものとしてページに渡されます。*パス情報*します。

ユーザーが、この URL を使用して、要求することを想像してください。

`http://www.contoso.com/a/b/c`

検索では、次のようにします。

1. 名前とパスを持つファイルがある */a/b/c.cshtml*でしょうか。 そうである場合は、そのページを実行し、情報を渡したりありません。 それ以外の場合.
2. 名前とパスを持つファイルがある */a/b.cshtml*でしょうか。 そのため、そのページを実行し、値を渡す場合`c`にします。 それ以外の場合.
3. 名前とパスを持つファイルがある */a.cshtml*でしょうか。 そのため、そのページを実行し、値を渡す場合`b/c`にします。

ない正確な検出、検索に一致する場合 *.cshtml*さらにこれらのファイルを探して、指定したフォルダーのファイルは、ASP.NET 継続します。

1. */a/b/c/default.cshtml* (パス情報がありません)。
2. */a/b/c/index.cshtml* (パス情報がありません)。

> [!NOTE]
> 明確にする特定のページ要求 (を含む要求、 *.cshtml*ファイル名拡張子) が期待するのと同じように動作します。 ような要求`http://www.contoso.com/a/b.cshtml`ページの実行は*b.cshtml*問題なく。


ページ内では、ページのパス情報を取得できます`UrlData`ディクショナリであるプロパティ。 という名前のファイルがあると*ViewCustomers.cshtml*し、サイトがこの要求を取得します。

`http://mysite.com/myWebSite/ViewCustomers/1000`

上記のルールのように、要求は、ページに移動します。 ページ内で取得し、パス情報を表示する、次のようなコードを使用することができます (この例では、値で&quot;1000&quot;)。

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> ルーティングと、完全なファイル名が関与しない、ためにありますあいまいさが同じであるページがあるとが異なるファイル名拡張子の名前 (たとえば、 *MyPage.cshtml*と*MyPage.html*). ルーティングの問題を回避するために、その拡張機能でのみが異なる名前を持つサイトのページがないことを確認することをお勧めします。


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース

[WebMatrix - Url、UrlData およびルーティングの SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO)します。 Mike Brind によるブログ エントリでは、ルーティングの動作では、ASP.NET Web ページに追加の詳細情報を提供します。
