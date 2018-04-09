---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: (Razor) サイト マップを表示する ASP.NET web Pages |Microsoft ドキュメント
author: tfitzmac
description: この記事では、Bing、Google、Ma によって提供されるサービスのマッピングに基づいて、ASP.NET Web Pages (Razor) web サイトのページに対話型のマップを表示する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 608dab8760bad7b877ab6fd4f89b21e980f5b1db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>ASP.NET Web Pages (Razor) サイトにマップを表示します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、Bing、Google、MapQuest、および yahoo から提供されるサービスのマッピングに基づいて、ASP.NET Web Pages (Razor) web サイトのページに対話型のマップを表示する方法について説明します。
> 
> 学習する内容。
> 
> - アドレスに基づくマップを生成する方法です。
> - 緯度と経度座標に基づくマップを生成する方法です。
> - Bing マップは、開発者アカウントを登録し、Bing マップで使用するキーを取得する方法。
> 
> これは、アーティクルで導入された ASP.NET の機能です。
> 
> - `Maps`ヘルパー。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> このチュートリアルは、WebMatrix 3 でも動作します。


Web ページで表示できますマップ ページを使用して`Maps`ヘルパー。 アドレスまたは緯度と経度座標のセットに基づいてマップを生成することができます。 `Maps`クラスには、Bing、Google、MapQuest、yahoo などの一般的なマップ エンジンを呼び出すことができます。

ページへのマッピングを追加する手順は、呼び出すマップ エンジンに関係なく同じです。 マップを表示する使用可能なメソッドは、JavaScript ファイル参照を追加するだけと、その後のメソッドを呼び出す、`Maps`ヘルパー。

基になるマップ サービスを選択する`Maps`ヘルパー メソッドを使用します。 これらのいずれかを使用することができます。

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>必要なコンポーネントをインストールします。

マップを表示するには、これらの要素が必要です。

- `Maps`ヘルパー。 このヘルパーは、ASP.NET Web Helpers Library のバージョン 2 でです。 ライブラリを既に追加していない場合は、NuGet パッケージとして、サイトでインストールすることができます。 詳細については、「 [ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)です。 (ギャラリーで、検索、`microsoft-web-helpers`パッケージです)。
- JQuery ライブラリです。 いくつかの WebMatrix サイト テンプレートがあらかじめ含まれてで jQuery ライブラリには、*スクリプト*フォルダーです。 これらのライブラリがない場合、は、最新の jQuery ライブラリから直接をダウンロードすることができます、 [jQuery.org](http://jQuery.org)サイトです。 テンプレートを使用して新しいサイトを作成することもできます (たとえば、**スターター サイト**テンプレート) し、jQuery ファイルをそのサイトから、現在のサイトにコピーします。

最後に、Bing maps を使用する場合は、最初 (無料) アカウントを作成して、キーを取得します。 キーの取得、これらの手順に従います。

1. アカウントを作成、 [Bing Maps の開発者アカウント](https://www.microsoft.com/maps/developers/web.aspx)です。 Microsoft アカウント (Windows Live ID) も必要です。

    キーを使用することを指定できます**評価/テスト**です。 WebMatrix と IIS Express を使用して、自分のコンピューターで、マッピング関数をテストする場合は、移動、**サイト**ワークスペースとは、サイトの URL (たとえば、`http://localhost:50408`ポート番号が異なる可能性がありますが、)。 これを行うこともできます*localhost*として登録する場合は、サイトのアドレス。
2. アカウントの登録した後、Bing マップのアカウント センターに移動し、クリックして**作成またはビューのキー**:

    ![mapping-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Bing を作成するキーを記録します。

## <a name="creating-a-map-based-on-an-address-using-google"></a>(Google を使用)、アドレスに基づくマップを作成します。

次の例では、アドレスに基づくマップを表示するページを作成する方法を示します。 この例では、Google マップを使用する方法を示します。

1. という名前のファイルを作成する*MapAddress.cshtml*サイトのルートにします。 このページに渡されたアドレスに基づくマップが生成されます。
2. 既存のコンテンツを上書きして、ファイルに次のコードをコピーします。

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    ページの次の機能を確認してください。

    - `<script>`内の要素、`<head>`要素。 例では、`<script>`要素の参照、 *jquery 1.6.4.min.js* jQuery ライブラリ バージョン 1.6.4 の縮小された (圧縮) バージョンではファイルです。 参照を前提とする、 *.js*ファイルは、*スクリプト*サイトのフォルダーです。 

        > [!NOTE]
        > JQuery ライブラリの別のバージョンを使用している場合のみをポイントしているそのバージョンを正しく確認してください。
    - 呼び出し、`@Maps.GetGoogleHtml`ページの本文にします。 アドレスをマップするには、アドレス文字列を渡す必要があります。 機能の他のマップ エンジンのメソッドは同様の方法で (`@Maps.GetYahooHtml`、 `@Maps.GetMapQuestHtml`)。
3. ページを実行し、アドレスを入力します。 ページは、Google マップ、指定した場所を表示するに基づく、マップを表示します。

     ![mapping-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>緯度と経度に基づいてマップを作成する座標を (Bing を使用)

この例では、座標に基づくマップを作成する方法を示します。 この例では、Bing maps を使用する方法と、Bing キーを含める方法を示します。 (Bing キーを使用せず、他のマップのエンジンを使用しても、座標に基づくマップを作成することができます)。

1. という名前のファイルを作成する*MapCoordinates.cshtml*サイトと置換と、次のコードとマークアップ、既存のコンテンツのルートに。

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. 置き換える`your-key-here`以前に生成した Bing Maps キーを使用します。
3. 実行、 *MapCoordinates.cshtml*ページ、緯度と経度座標を入力し、クリックして、 **Map It!** ボタンをクリックします。 (任意の座標がわからない場合を再試行してください以下です。 これは Microsoft レッドモンド上の場所です。)

   - Latitude: 47.6781005859375
   - 経度:-122.158317565918

     指定した座標を使用して、ページが表示されます。

     ![マッピング-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース


[Microsoft.Maps API リファレンス](https://msdn.microsoft.com/library/gg427611.aspx)
