---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: モバイル デバイスのページ (Razor) サイトを ASP.NET Web の表示 |Microsoft Docs
author: Rick-Anderson
description: 'この記事では、モバイル デバイスで適切にレンダリングする ASP.NET Web Pages (Razor) サイトでページを作成する方法について説明します。 学習内容: する方法.'
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: dd06a54d396bd85eeef7c004ee115828d541a2c1
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020938"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>モバイル デバイス用の ASP.NET Web Pages (Razor) サイトの表示
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、モバイル デバイスで適切にレンダリングする ASP.NET Web Pages (Razor) サイトでページを作成する方法について説明します。
> 
> 学習内容。
> 
> - 名前付け規則を使用して、ページが特にモバイル デバイス向けに設計されているを指定する方法。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも機能します。


ASP.NET Web ページを使用して、モバイル デバイスまたはその他のデバイスでコンテンツの描画のカスタムの表示を作成できます。

このようなファイルの名前付けパターンを使用して ASP.NET Web Pages サイトでデバイスに固有のページを作成する最も簡単な方法は、:<em>ファイル名</em>。<em>Mobile</em><em>.cshtml</em>します。 ページの 2 つのバージョンを作成することができます (たとえば、1 つという名前<em>MyFile.cshtml</em>という名前の 1 つ<em>MyFile.Mobile.cshtml</em>)。 実行時に、モバイル デバイスを要求したとき<em>MyFile.cshtml</em>、ASP.NET からコンテンツをレンダリングする<em>MyFile.Mobile.cshtml</em>します。 それ以外の場合、 <em>MyFile.cshtml</em>を表示します。

次の例では、モバイル デバイスのコンテンツ ページを追加して、モバイルのレンダリングを有効にする方法を示します。 *Page1.cshtml*コンテンツにはナビゲーション サイド バーが含まれています。 *Page1.Mobile.cshtml* 、同じコンテンツが含まれていますが、サイドバーが省略されます。

1. という名前のファイルを作成する ASP.NET Web Pages サイトで*Page1.cshtml*現在のコンテンツを次のマークアップに置き換えます。

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. という名前のファイルを作成する*Page1.Mobile.cshtml*既存のコンテンツを次のマークアップに置き換えます。 モバイル バージョンのページが小さい画面できれいに表示のナビゲーション セクションを省略することに注意してください。

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. デスクトップ ブラウザーを実行しを参照*Page1.cshtml*します。 ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. モバイル ブラウザー (または、モバイル デバイス エミュレーター) を実行しを参照*Page1.cshtml*します。 (通知を組み込んでいない *.mobile します。* URL の一部として、します。)要求されている場合でも*Page1.cshtml*、ASP.NET のレンダリング*Page1.Mobile.cshtml*します。

    ![mobilesites 2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> モバイル ページをテストするには、デスクトップ コンピューターで実行されているモバイル デバイス シミュレーターを使用できます。 このツールを使用して、モバイル デバイスでになりますが、web ページをテストできます (つまり、通常ははるかに小さいで領域を表示)。 シミュレーターの 1 つの例は、[ユーザー エージェント切り替えアドオン](http://addons.mozilla.org/firefox/addon/user-agent-switcher/)、Mozilla firefox に対応することができます Firefox のデスクトップ バージョンからのさまざまなモバイル ブラウザーをエミュレートします。


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース


[Windows Phone エミュレーター](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
