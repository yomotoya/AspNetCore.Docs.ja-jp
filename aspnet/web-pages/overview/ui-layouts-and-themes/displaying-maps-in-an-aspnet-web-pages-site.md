---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: ページ (Razor) サイトを ASP.NET web マップを表示する |Microsoft Docs
author: Rick-Anderson
description: この記事では、Bing、Google、Ma によって提供されるサービスのマッピングに基づいて、ASP.NET Web Pages (Razor) web サイトのページに対話型マップを表示する方法について説明しています.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: cde27c54b11ee91b193dffd61e3a354c6cf2449a
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020989"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>ASP.NET Web ページ (Razor) サイトでマップを表示します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、Bing、Google、MapQuest、および Yahoo によって提供されるサービスのマッピングに基づいて、ASP.NET Web Pages (Razor) web サイトのページでインタラクティブ マップを表示する方法について説明します。
> 
> 学習内容。
> 
> - アドレスに基づくマップを生成する方法。
> - 緯度と経度の座標に基づくマップを生成する方法。
> - Bing Maps の開発者アカウントを登録し、Bing Maps を使用するキーを取得する方法。
> 
> これは、この記事で導入された ASP.NET の機能です。
> 
> - `Maps`ヘルパー。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> このチュートリアルは、WebMatrix 3 でも機能します。


Web ページで表示できますマップ、ページを使用して`Maps`ヘルパー。 アドレスまたは経度と緯度の座標のセットのいずれかに基づくマップを生成することができます。 `Maps`クラス Bing、Google、MapQuest、Yahoo などの人気のあるマップ エンジンを呼び出すことができます。

ページへのマッピングを追加する手順は、呼び出すことのマップ エンジンに関係なく同じです。 使用可能なメソッドをマップを表示する JavaScript ファイル参照を追加するだけとのメソッドを呼び出して、`Maps`ヘルパー。

基準となるマップ サービスを選択する`Maps`を使用するヘルパー メソッド。 これらのいずれかを使用することができます。

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>必要なコンポーネントをインストールします。

マップを表示するには、これらの情報が必要です。

- `Maps`ヘルパー。 このヘルパーは、ASP.NET Web Helpers Library のバージョン 2 では。 ライブラリに追加済みしていない場合、NuGet パッケージとしてインストール、サイトのことができます。 詳細については、次を参照してください。 [ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)します。 (ギャラリーで検索、`microsoft-web-helpers`パッケージです)。
- JQuery ライブラリです。 いくつかの WebMatrix サイトのテンプレートでの jQuery ライブラリに既に含まれて、*スクリプト*フォルダー。 これらのライブラリがない場合、は、直接から最新の jQuery ライブラリをダウンロードすることができます、 [jQuery.org](http://jQuery.org)サイト。 テンプレートを使用して新しいサイトを作成することも (たとえば、**スターター サイト**テンプレート) し、現在のサイトにそのサイトから jQuery ファイルをコピーします。

最後に、Bing maps を使用する場合は、最初 (無料) アカウントを作成して、キーを取得します。 キーを取得するには、次の手順に従います。

1. アカウントを作成、 [Bing Maps デベロッパー アカウント](https://www.microsoft.com/maps/developers/web.aspx)します。 Microsoft アカウント (Windows Live ID) も必要です。

    キーを使用することを指定する**評価/テスト**します。 自分の WebMatrix と IIS Express を使用してコンピューターにマッピング関数をテストする場合は、移動、**サイト**ワークスペースと、サイトの URL に注意してください (たとえば、`http://localhost:50408`実際のポート番号は異なるかもしれませんが、)。 これを使用することができます*localhost*として登録するときに、サイトのアドレス。
2. アカウントを登録すると後、は、Bing Maps のアカウント センターに移動し、をクリックして**作成またはビューのキー**:

    ![マッピング 2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Bing を作成するキーを記録します。

## <a name="creating-a-map-based-on-an-address-using-google"></a>(Google を使用して) アドレスに基づくマップを作成します。

次の例では、アドレスに基づくマップを表示するページを作成する方法を示します。 この例では、Google マップを使用する方法を示します。

1. という名前のファイルを作成する*MapAddress.cshtml*サイトのルートにします。 このページに渡すことのできる、アドレスに基づくマップが生成されます。
2. 既存のコンテンツを上書き、ファイルに次のコードをコピーします。

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    ページの次の機能に注目してください。

    - `<script>`内の要素、`<head>`要素。 例では、`<script>`要素の参照、 *jquery 1.6.4.min.js*ファイル。 これは、jQuery ライブラリ バージョン 1.6.4 の縮小 (圧縮) バージョン。 参照にはことが前提とに注意してください、 *.js*ファイルは、*スクリプト*サイトのフォルダー。 

        > [!NOTE]
        > JQuery ライブラリの別のバージョンを使用している場合だけをポイントしているのバージョンに正しくを確認します。
    - 呼び出し、`@Maps.GetGoogleHtml`ページの本文にします。 アドレスをマップするには、アドレス文字列を渡す必要があります。 その他のマップ エンジンのメソッドと同様の方法では動作 (`@Maps.GetYahooHtml`、 `@Maps.GetMapQuestHtml`)。
3. ページを実行し、アドレスを入力します。 ページには、Google マップ、指定した場所を示すに基づいて、マップが表示されます。

     ![-1 のマッピング](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>緯度と経度に基づくマップの作成を調整 (Bing を使用)

この例では、座標に基づいてマップを作成する方法を示します。 この例では、Bing maps を使用する方法と、Bing キーを含める方法を示します。 (Bing キーを使用せず、他のマップ エンジンを使用しても、座標に基づいてマップを作成することができます)。

1. という名前のファイルを作成する*MapCoordinates.cshtml*サイトと置換、既存のコンテンツと、次のコードとマークアップのルートに。

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. 置換`your-key-here`先に生成した Bing Maps キーを使用します。
3. 実行、 *MapCoordinates.cshtml*ページ、緯度と経度の座標を入力し、クリックして、 **Map It!** を追加します。 (任意の座標がわからない場合お試しください以下。 これは、Microsoft のレッドモンド キャンパス上の場所) です。

   - 緯度: 47.6781005859375
   - 経度:-122.158317565918

     指定した座標を使用して、ページが表示されます。

     ![マッピング-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース


[Microsoft.Maps API リファレンス](https://msdn.microsoft.com/library/gg427611.aspx)
