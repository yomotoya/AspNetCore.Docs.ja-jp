---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: "モバイル デバイスのページ (Razor) サイトを ASP.NET Web をレンダリング |Microsoft ドキュメント"
author: tfitzmac
description: "この記事では、モバイル デバイスで適切に表示される ASP.NET Web Pages (Razor) サイトのページを作成する方法について説明します。 学習内容: する方法."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 08b714eb2ffaefc7c7e2e5c9a7428106b231e5b7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>モバイル デバイス用の ASP.NET Web Pages (Razor) サイトの表示
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、モバイル デバイスで適切に表示される ASP.NET Web Pages (Razor) サイトのページを作成する方法について説明します。
> 
> 学習する内容。
> 
> - 名前付け規則を使用して、ページがモバイル デバイス向けに設計されているを指定する方法です。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも動作します。


ASP.NET Web ページを使用して、モバイル デバイスまたはその他のデバイスでのコンテンツ表示のカスタムの表示を作成できます。

ASP.NET Web Pages サイトでデバイスに固有のページを作成する最も簡単な方法は次のようにファイルの名前付けパターンを使用して、:*ファイル名*。*Mobile**.cshtml*です。 ページの 2 つのバージョンを作成することができます (たとえば、1 つという名前の*MyFile.cshtml*という名前の 1 つ*MyFile.Mobile.cshtml*)。 実行時に、モバイル デバイスを要求したとき*MyFile.cshtml*、ASP.NET のコンテンツを表示する*MyFile.Mobile.cshtml*です。 それ以外の場合、 *MyFile.cshtml*が表示されます。

次の例では、モバイル デバイスのコンテンツ ページを追加することで、モバイルのレンダリングを有効にする方法を示します。 *Page1.cshtml*コンテンツにはナビゲーション サイド バーが含まれています。 *Page1.Mobile.cshtml*が同じコンテンツが含まれていますが、サイドバーを省略します。

1. ASP.NET Web Pages サイトは、という名前のファイルを作成する*Page1.cshtml*して現在のコンテンツを次のマークアップに置き換えます。

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. という名前のファイルを作成する*Page1.Mobile.cshtml*し、既存のコンテンツを次のマークアップに置き換えます。 ページのモバイル バージョンが小さい画面できれいに表示の [ナビゲーション] セクションを省略することに注意してください。

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. デスクトップのブラウザーを実行しを参照*Page1.cshtml*です。 ![mobilesites 1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. モバイル ブラウザー (または、モバイル デバイス エミュレーター) を実行しを参照*Page1.cshtml*です。 (含めないようにすることを確認*.mobile です。* URL の一部として、します。)要求内容がにもかかわらず*Page1.cshtml*、ASP.NET 表示*Page1.Mobile.cshtml*です。

    ![mobilesites 2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> モバイル ページをテストするには、デスクトップ コンピューターで実行されているモバイル デバイス シミュレーターを使用できます。 このツールを使用して、それらのモバイル デバイスの表示は、web ページをテストできます (つまり、通常より小さな領域を表示)。 シミュレーターの 1 つの例は、[ユーザー エージェント スイッチャー アドオン](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/)Mozilla Firefox に対応することができます Firefox のデスクトップ バージョンからのさまざまなモバイル ブラウザーをエミュレートします。


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース


[Windows Phone エミュレーター](https://msdn.microsoft.com/en-us/library/ff402563(v=VS.92).aspx)
