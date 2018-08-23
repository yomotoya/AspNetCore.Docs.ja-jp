---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Twitter ヘルパーと ASP.NET Web Pages |Microsoft Docs
author: tfitzmac
description: このトピックで、アプリケーションは、Twitter ヘルパーを WebMatrix 3 プロジェクトに追加する方法を示します。 Twitter ヘルパー コードが含まれ、ヘルパーを呼び出す方法を示しています.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 06223826c2682ffd62d5a1717f34242f39be5eda
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833361"
---
<a name="twitter-helper-with-aspnet-web-pages"></a>Twitter ヘルパーと ASP.NET Web ページ
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このトピックで、アプリケーションは、Twitter ヘルパーを WebMatrix 3 プロジェクトに追加する方法を示します。 Twitter ヘルパー コードが含まれ、ヘルパー メソッドを呼び出す方法を示しています。
> 
> Twitter.cshtml ファイル用のこのコードは、開発された**Tian パン**Microsoft の。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも機能します。


## <a name="introduction"></a>はじめに

このトピックでは、アプリケーションに Twitter ヘルパーを追加して、ヘルパー メソッドを呼び出す Razor 構文を使用する方法を示します。 Twitter ヘルパーでは、簡単に Twitter ボタンと、アプリケーションでウィジェットを組み込めます。 ユーザーのタイムラインや、ハッシュタグの検索結果などの Twitter ウィジェットを使用する必要があります最初に作成する、 [twitter ウィジェット](https://twitter.com/settings/widgets)します。 ウィジェットを作成した後は、ウィジェット id を受け取ります。ウィジェットを表示するヘルパー メソッドを呼び出すときに、このウィジェット id をパラメーターとして渡します。

このトピックでは、Twitter API のバージョン 1.1 用に記述されています。 直接 Twitter ヘルパー コードを追加すると、プロジェクトに、Twitter API が変更された場合、ヘルパー コードを更新できます。

WebMatrix のインストール方法の詳細については、次を参照してください。 [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md)します。

## <a name="add-twitter-helper-to-your-project"></a>Twitter ヘルパーをプロジェクトに追加します。

Twitter ヘルパーを追加するには、最初に、という名前のフォルダーを追加**アプリ\_コード**をプロジェクトにします。 という名前のファイルを作成し、 **Twitter.cshtml**します。

![App_Code フォルダー](twitter-helper/_static/image1.png)

Twitter.cshtml の既定のコードを次のコードに置き換えます。

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Web ページから Twitter メソッドの呼び出し

次の例では、プロジェクトで、ページから Twitter ヘルパー メソッドを使用する方法を示します。 プロジェクトのニーズに関連する値を持つパラメーターの値を置換するは。 指定されたウィジェット id を使用するには、メソッドの動作を探索するが、プロジェクトの独自のウィジェットを生成します。

すべての以下のパラメーターが必要です。 省略可能なパラメーターは、ボタンまたはウィジェットの表示方法をカスタマイズに使用されます。 たとえば、次のボタンにするには、ユーザー名のみが必要ですが、フォロワー数を含めるし、ボタンと言語のサイズを指定する方法する方法を示します。

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>結果を参照してください。

上記のコードには、次のボタンとウィジェットが生成されます。 これらのボタンとウィジェットは完全に機能、スクリーン ショットではありません。 言語パラメーター設定されているために、スペイン語で以下のボタンが表示されます**es**します。

### <a name="follow-button"></a>次のボタン

[次の@aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="tweet-button"></a>ツイート ボタン

[ツイート](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="user-timeline-profile"></a>ユーザー タイムライン (プロファイル)

[別のツイート @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="favorites"></a>お気に入り

[ツイートのお気に入り @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="list"></a>リスト

[ツイート@Microsoft/MS\_コンシューマー\_バンド](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="search"></a>検索

[についてツイート&quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`
