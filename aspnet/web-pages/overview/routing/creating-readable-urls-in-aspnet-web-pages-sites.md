---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: "ページ (Razor) サイトの ASP.NET Web で読み取り可能な Url を作成する |Microsoft ドキュメント"
author: tfitzmac
description: "この記事では、ASP.NET Web Pages (Razor) の web サイト、およびこれを使用する方法を読みやすく、優れた SEO の Url を使用してルーティングについて説明します。 新機能を学習しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 7858b7cbd6dccafb2867ed9a1d102561e211e435
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web Pages (Razor) のサイトで読み取り可能な Url を作成します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ASP.NET Web Pages (Razor) の web サイト、およびこれを使用する方法を読みやすく、優れた SEO の Url を使用してルーティングについて説明します。
> 
> 学習する内容。
> 
> - どの ASP.NET を使用してルーティング以上読み取り可能で、検索可能な Url を使用できます。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも動作します。


## <a name="about-routing"></a>ルーティングについて

サイト内のページの Url には、どの程度、サイトの動作に影響を与えることができます。 URL の&quot;フレンドリ&quot;となるサイトを使用するユーザーを簡単にします。 サイトの検索エンジン最適化 (SEO) にも役立ちます。 ASP.NET web サイトには、フレンドリな Url を自動的に使用する機能が含まれます。

ASP.NET では、サーバー上のファイルを指しているだけではなくユーザーの操作について説明するわかりやすい Url を作成することができます。 架空のブログを考えてみましょう Url:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

次のようにこれらの Url を比較します。

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

使用して、ブログが表示されたことを確認する必要があります、ユーザー、最初のペアで、 *blog.cshtml*ページ、および適切なカテゴリまたは日付範囲を取得するクエリ文字列を構築し、必要があります。 2 番目の例のセットは理解し、作成をはるかに簡単です。

最初の例の Url は、特定のファイルを直接もポイント (*blog.cshtml*)。 何らかの理由により、ブログが別のフォルダーにサーバー上の移動、別のページを使用するブログが書き換えられた場合や、リンクが正しくないがあります。 Url の 2 番目のセットは、特定のページを指していませんブログ実装または場所が変更された場合でもそのため、Url もよい無効です。

ASP.NET Web Pages で ASP.NET を使用するためのような前の例でわかりやすい Url を作成できます*ルーティング*です。 ページ (ページ) への要求を満たすことができますを URL から論理マッピングを作成するルーティングします。 マッピングは論理ので (物理、特定のファイルを)、ルーティング非常に柔軟に、サイトの Url を定義する方法を提供します。

## <a name="how-routing-works"></a>ルーティングのしくみ

ASP.NET 要求を処理するときの宛先に転送する方法を決定する URL を読み取ります。 ASP.NET では、左から右へと向かうディスク上のファイルへの URL の個別のセグメントを照合します。 一致がある場合、URL の残りのものとしてそのページへ渡されます。*パス情報*です。

他のユーザーがこの URL を使用して、要求を行うことを想像してください。

`http://www.contoso.com/a/b/c`

検索は、次のようになります。

1. 名前とパスを持つファイルがある*/a/b/c.cshtml*しますか? 場合は、そのページを実行し、情報を渡したりありません。 それ以外の場合.
2. 名前とパスを持つファイルがある*/a/b.cshtml*しますか? そのため、そのページを実行し、値を渡す場合`c`にします。 それ以外の場合.
3. 名前とパスを持つファイルがある*/a.cshtml*しますか? そのため、そのページを実行し、値を渡す場合`b/c`にします。

正確ないいえ、検索に一致する場合*.cshtml*さらにこれらのファイルを探して、指定したフォルダーのファイルは、ASP.NET 継続します。

1. */a/b/c/default.cshtml* (パス情報がない)。
2. */a/b/c/index.cshtml* (パス情報がない)。

> [!NOTE]
> 、明確にする特定のページの要求 (含む要求は、、 *.cshtml*ファイル名拡張子) が期待するのと同じように動作します。 要求と同様に`http://www.contoso.com/a/b.cshtml`ページの実行は*b.cshtml*うまくです。


ページ内には、ページのパス情報を取得することができます`UrlData`プロパティ ディクショナリです。 という名前のファイルがあると*ViewCustomers.cshtml*をサイトがこの要求を取得します。

`http://mysite.com/myWebSite/ViewCustomers/1000`

上記の規則のように、要求は、ページに移動します。 ページ内を取得し、パス情報を表示する次のようにコードを使用することができます (この場合、値&quot;1000&quot;)。

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> ルーティングと、完全なファイル名が含まれまるありません、ためにありますあいまいさが同じであるページがある場合が異なるファイル名拡張子の名前 (たとえば、 *MyPage.cshtml*と*MyPage.html*). ルーティングの問題を回避するために、拡張機能でのみが異なる名前を持つサイトのページがないことを確認することをお勧めします。


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース

[WebMatrix の Url、UrlData およびルーティングの SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO)です。 Mike Brind によるブログ エントリでは、ルーティング機能の仕組み ASP.NET Web Pages での追加の詳細情報を提供します。
