---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: "Twitter の ASP.NET Web Pages でヘルパー |Microsoft ドキュメント"
author: tfitzmac
description: "このトピックおよびアプリケーションは、Twitter Helper を WebMatrix 3 プロジェクトに追加する方法を示します。 Twitter のヘルパー コードを含むし、ヘルパーを呼び出す方法を示しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 07d9c4d485c42b78a42c54c9740af5f67cb44763
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="twitter-helper-with-aspnet-web-pages"></a>Twitter でヘルパーを ASP.NET Web ページ
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このトピックおよびアプリケーションは、Twitter Helper を WebMatrix 3 プロジェクトに追加する方法を示します。 Twitter のヘルパー コードを含むし、ヘルパー メソッドを呼び出す方法を示しています。
> 
> Twitter.cshtml ファイルのこのコードは、によって開発された**Tian パン**Microsoft のです。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも動作します。


## <a name="introduction"></a>はじめに

このトピックでは、Twitter Helper をアプリケーションに追加して、ヘルパー メソッドの呼び出しに Razor 構文を使用する方法を示します。 Twitter Helper を使用を簡単に Twitter ボタンと、アプリケーションでウィジェットを反映します。 ユーザーのタイムラインをハッシュタグの検索結果などの Twitter ウィジェットを使用する必要がありますまずを作成する、 [twitter ウィジェット](https://twitter.com/settings/widgets)です。 独自のウィジェットを作成した後は、ウィジェット id を受け取ります。このウィジェット id は、ウィジェットを表示するヘルパー メソッドを呼び出すときにパラメーターとして渡します。

このトピックは、Twitter API のバージョン 1.1 に記述されています。 直接 Twitter ヘルパー コードを追加すると、プロジェクトに、Twitter API が変更された場合、ヘルパー コードを更新できます。

WebMatrix のインストール方法の詳細については、次を参照してください。 [Introducing ASP.NET Web Pages 2 - はじめ](../getting-started/introducing-aspnet-web-pages-2/getting-started.md)です。

## <a name="add-twitter-helper-to-your-project"></a>Twitter Helper プロジェクトに追加します。

Twitter Helper を追加するには、最初に、という名前のフォルダーを追加**アプリ\_コード**をプロジェクトにします。 という名前のファイルを作成し、 **Twitter.cshtml**です。

![App_Code フォルダー](twitter-helper/_static/image1.png)

Twitter.cshtml の既定のコードを次のコードに置き換えます。

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Web ページから Twitter メソッドの呼び出し

次の例では、プロジェクト内のページから Twitter のヘルパー メソッドを使用する方法を示します。 プロジェクトではするパラメーター値は、お客様のニーズに関連する値に置き換えます。 指定されたウィジェット id を使用すると、メソッドの動作を調べるがプロジェクト用の独自のウィジェットを生成します。

すべての次に示すパラメーターが必要です。 省略可能なパラメーターを使用するには、ボタンまたはウィジェットの表示方法をカスタマイズします。 たとえば、以下のボタンに従うには、ユーザー名のみが必要ですが、フォローの番号を含めるし、ボタンと言語のサイズを指定する方法を方法の例に示します。

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>結果を参照してください。

上記のコードには、次のボタンとウィジェットが生成されます。 これらのボタンとウィジェットが完全に機能、スクリーン ショットではありません。 言語パラメーター設定されているためスペイン語で以下のボタンが表示されます**es**です。

### <a name="follow-button"></a>次のボタン

[次の@aspnet)](https://twitter.com/aspnet)<script>! (d、s、id) 関数 {var js、fjs d.getElementsByTagName(s) [0], p =/^http:/.test(d.location) を = ですか?'http': 'https';場合 (! d.getElementById(id)) {js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore (js、fjs);}}(ドキュメント、'script'、' twitter wjs');</script>

### <a name="tweet-button"></a>ツイート ボタン

[ツイート](https://twitter.com/share)<script>! (d、s、id) 関数 {var js、fjs d.getElementsByTagName(s) [0], p =/^http:/.test(d.location) を = ですか?'http': 'https';場合 (! d.getElementById(id)) {js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore (js、fjs);}}(ドキュメント、'script'、' twitter wjs');</script>

### <a name="user-timeline-profile"></a>ユーザー タイムライン (プロファイル)

[によってツイート@aspnet ](https://twitter.com/aspnet) <script>! (d、s、id) 関数 {var js、fjs d.getElementsByTagName(s) [0], p =/^http:/.test(d.location) を = ですか?'http': 'https';場合 (! d.getElementById(id)) {js = d.createElement(s); js.id = id; js.src = p +"://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (js、fjs);}}(ドキュメント、"script"、"twitter wjs") です。</script>

### <a name="favorites"></a>お気に入り

[お気に入りツイート@Microsoft ](https://twitter.com/Microsoft/favorites) <script>! (d、s、id) 関数 {var js、fjs d.getElementsByTagName(s) [0], p =/^http:/.test(d.location) を = ですか?'http': 'https';場合 (! d.getElementById(id)) {js = d.createElement(s); js.id = id; js.src = p +"://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (js、fjs);}}(ドキュメント、"script"、"twitter wjs") です。</script>

### <a name="list"></a>リスト

[ツイート@Microsoft/MS\_コンシューマー\_バンド](https://twitter.com/microsoft/ms-consumer-brands/)<script>! (d、s、id) 関数 {var js、fjs d.getElementsByTagName(s) [0], p =/^http:/.test(d.location) を = ですか?'http': 'https';場合 (! d.getElementById(id)) {js = d.createElement(s); js.id = id; js.src = p +"://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (js、fjs);}}(ドキュメント、"script"、"twitter wjs") です。</script>

### <a name="search"></a>検索

[に関するツイート&quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>! (d、s、id) 関数 {var js、fjs d.getElementsByTagName(s) [0], p =/^http:/.test(d.location) を = ですか?'http': 'https';場合 (! d.getElementById(id)) {js = d.createElement(s); js.id = id; js.src = p +"://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (js、fjs);}}(ドキュメント、"script"、"twitter wjs") です。</script>
