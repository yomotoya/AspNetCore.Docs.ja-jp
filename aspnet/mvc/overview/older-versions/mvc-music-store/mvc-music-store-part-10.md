---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'パート 10: ナビゲーションとサイト デザイン、結論の最終的な更新プログラム |Microsoft Docs'
author: jongalloway
description: このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 パート 10 では、ナビゲーションと S. の最終的な更新プログラムについて説明します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: a5b1d02d268530d6288ed2bc502679336d06d403
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366321"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>パート 10: ナビゲーションとサイト デザイン、結論の最終的な更新プログラム
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC のミュージック ストアは、チュートリアル アプリケーションを紹介し、web 開発用の ASP.NET MVC と Visual Studio を使用する方法をステップ バイ ステップについて説明します。  
>   
> MVC のミュージック ストアは、オンラインで音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインし、買い物カゴの機能を実装する軽量サンプル ストア実装です。  
>   
> このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 パート 10 では、ナビゲーションとサイト デザイン、結論を最終的な更新プログラムについて説明します。


私たちのサイトのすべての主要な機能が終了しましたが、サイト ナビゲーション、ホーム ページで、およびストアの参照ページに追加するいくつかの機能、まだいます。

## <a name="creating-the-shopping-cart-summary-partial-view"></a>ショッピング カート概要部分ビューの作成

サイト全体にわたってユーザーのショッピング カート内の項目の数を公開します。

![](mvc-music-store-part-10/_static/image1.png)

Site.master に追加される部分ビューを作成して簡単に実装できます。

前述のように、ShoppingCart コント ローラーには、部分ビューを返す CartSummary アクション メソッドが含まれています。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

CartSummary 部分ビューを作成するには、ビュー/ShoppingCart フォルダーを右クリックし、ビューの追加を選択します。 CartSummary ビューの名前を指定し、次に示すように"部分ビューを作成する チェック ボックスをオンします。

![](mvc-music-store-part-10/_static/image2.png)

CartSummary 部分ビューは非常に単純 - カートにアイテムの数を示す ShoppingCart インデックス ビューへのリンクだけです。 CartSummary.cshtml の完全なコードは次のとおりです。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

Html.RenderAction メソッドを使用して、サイトのマスターを含め、サイトの任意のページで部分ビューを含めることができます。 RenderAction では、アクションの名前 ("CartSummary") と以下としてコント ローラー名 ("ShoppingCart") を指定する必要があります。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

これをサイトのレイアウトに追加する前にも作成、[ジャンル] メニューの Site.master 更新をすべて同時に確認できるようにします。

## <a name="creating-the-genre-menu-partial-view"></a>ジャンル メニュー部分ビューを作成します。

できることができます、ストアで使用可能なすべてのジャンルの一覧を表示するジャンル メニューを追加することで、ストア内を移動するユーザーをずっと簡単。

![](mvc-music-store-part-10/_static/image3.png)

同じに従って手順も、GenreMenu 部分ビューを作成し、両方をサイトのマスターに追加します。 最初に、次の GenreMenu コント ローラー アクションを StoreController に追加します。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

このアクションは、次で作成されますが、部分ビューによって表示されるジャンルの一覧を返します。

*注: [ChildActionOnly] 属性から、部分ビューを使用するには、この操作のみが必要であることを示します。 このコント ローラー アクションに追加されました。この属性により、コント ローラー アクションから/Store/GenreMenu を参照して実行されています。部分ビューは、必須ではありませんが、意図したように、コント ローラー アクションが使用されるかどうかを確認するために、ことをお勧めです。ビュー エンジンがその他のビュー内に含まれる際に、このビューのレイアウトを使用しないでください、知っておくことができるビューではなく PartialView を返すもいます。*

GenreMenu コント ローラーのアクションを右クリックし、次に示すように、ジャンルのビュー データ クラスを使用して厳密に型指定された GenreMenu をという名前の部分的なビューを作成します。

![](mvc-music-store-part-10/_static/image4.png)

次のように、順序なしリストを使用して項目を表示する GenreMenu 部分ビューのコードの表示を更新します。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>部分ビューを表示するサイトのレイアウトを更新しています

サイトのレイアウトに、部分ビューを追加できます (共有/ビュー/\_Layout.cshtml) Html.RenderAction() を呼び出すことによって。 次に示すように両方、だけでなく、それらを表示するマークアップを追加しますします。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

これでアプリケーションを実行すると、左側のナビゲーション領域のジャンルと上部にあるカートの概要が表示されます。

## <a name="update-to-the-store-browse-page"></a>ストアの参照ページを更新します。

ストアの参照 ページは機能しますが、非常に良いが正しく表示されません。 レイアウトをわかりやすく、コードの表示 (/Views/Store/Browse.cshtml で確認) 次のように更新することで、アルバムを表示するページを更新できます。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

ここで行っています Html.ActionLink ではなく Url.Action を使用してアルバム アートを含むへのリンクを特殊な書式設定を適用できるようにします。

*注: これらのアルバムの汎用のアルバム カバーが表示されます。この情報は、データベースに格納されはストア マネージャーを使用して編集できます。独自のアートワークを追加する、できます。*

今すぐを参照、ジャンル、ときにアルバム アートを持つグリッドに表示されるアルバムが表示されます。

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>上部の販売アルバムを表示するホーム ページを更新しています

販売の売上が増加するホーム ページ上のアルバム、一番上に機能します。 処理を追加するのにもいくつか追加のグラフィックス、HomeController に一部の更新プログラムをしましょう。

まず、EntityFramework が関連付けられていることを認識できるように、アルバム クラスにナビゲーション プロパティを追加します。 最後の数行、**アルバム**クラスが次のようになります。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*注: これは追加する必要を使用して、System.Collections.Generic 名前空間でのステートメント。*

まず、storeDB フィールドと、他のコント ローラーのように、ステートメントを使用して MvcMusicStore.Models を追加します。 次に、HomeController OrderDetails に従って販売上のアルバムを検索するデータベースを照会する次のメソッドを追加します。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

これは、コント ローラーのアクションとして使用できるようにする望ましくないため、プライベート メソッドです。 わかりやすくするために、HomeController に含めることが、必要に応じて別のサービス クラスに、ビジネス ロジックを移動することが推奨されます。

配置して、アルバムを販売して上位 5 つのクエリをビューに戻すインデックス コント ローラー アクション更新できます。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

更新された HomeController の完全なコードは、次に示すようです。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

最後に、モデルの種類を更新して、アルバム一覧を下に追加のアルバムの一覧を表示できるように、ホーム インデックス ビューを更新する必要があります。 私たちもページに見出しセクションがあり、昇格を追加するには、この機会になります。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

今すぐアプリケーションを実行すると販売の最上位のアルバムと、キャンペーンのメッセージを使用、更新のホーム ページが表示されます。

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>まとめ

ASP.NET MVC 簡単見たなどのデータベース アクセス、メンバーシップ、AJAX、高度な web サイトを作成します。 非常に短時間です。 このチュートリアルが願って、独自の ASP.NET MVC アプリケーションの構築を開始する必要があるツールです。


> [!div class="step-by-step"]
> [前へ](mvc-music-store-part-9.md)
