---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: "手順 10: 最終的な更新プログラムのナビゲーションおよびサイト設計、結論 |Microsoft ドキュメント"
author: jongalloway
description: "このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 10 の一部では、ナビゲーションと S を最終的な更新プログラムについて説明."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: 2a65e4b793b615c45cdf31166e0a000ae72ee534
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2018
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>手順 10: 最終的な更新プログラムのナビゲーションおよびサイト設計、結論
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。  
>   
> MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。  
>   
> このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 10 の一部では、ナビゲーションとサイトの設計、結論を最終的な更新プログラムについて説明します。


サイトのすべての主要な機能が終了しましたが、お、まだ一部の機能をサイトのナビゲーション、ホーム ページで、ストアの参照 ページに追加します。

## <a name="creating-the-shopping-cart-summary-partial-view"></a>ショッピング カート概要部分ビューの作成

サイト全体にわたって、ユーザーのショッピング カート内の項目数を公開することができます。

![](mvc-music-store-part-10/_static/image1.png)

当社 Site.master に追加されている部分ビューを作成することで簡単に実装できます。

上記のように、ShoppingCart コント ローラーには、部分ビューを返す、CartSummary アクション メソッドが含まれています。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

CartSummary 部分ビューを作成するには、ビュー/ShoppingCart フォルダーを右クリックし、ビューの追加を選択します。 CartSummary ビューを指定し、次に示すように、「部分ビューの作成」のチェック ボックスを確認します。

![](mvc-music-store-part-10/_static/image2.png)

CartSummary 部分ビューは実に簡単 - カートにアイテムの数を示す ShoppingCart インデックス ビューへのリンクだけです。 CartSummary.cshtml の完全なコードは次のとおりです。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

マスターを含む、サイト、Html.RenderAction メソッドを使用して、サイト内の任意のページには、部分ビューを含めることができます。 RenderAction では、アクション名 ("CartSummary") と、コント ローラー名 ("ShoppingCart") として以下を指定することが必要です。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

これをサイトのレイアウトに追加する前にも作成、[ジャンル] メニューを行えるように、Site.master 更新をすべて一度に 1 つです。

## <a name="creating-the-genre-menu-partial-view"></a>ジャンル メニュー部分ビューを作成します。

おやすく多くをストアで利用可能なすべてのジャンルの一覧を表示するジャンル メニューを追加することで、ストア内を移動するユーザー向けです。

![](mvc-music-store-part-10/_static/image3.png)

同じに従って手順も GenreMenu、部分ビューを作成し、その両方をサイトに、マスター追加おし、します。 最初に、次の GenreMenu コント ローラーのアクションを StoreController に追加します。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

この操作は、部分のビューは、次を作成して表示されるジャンルの一覧を返します。

*注: [ChildActionOnly] 属性のみ、部分ビューから使用するには、この操作が必要ことを示すこのコント ローラー アクションに追加されました。この属性は、/Store/GenreMenu を参照して実行されるをコント ローラーのアクションをできなくなります。部分ビューは、必須ではありませんが、ことをお勧めは、意図したとおりに使用する、コント ローラーのアクションを確認するためです。ビュー エンジンが他のビューの対象は、このビューのレイアウトを使用しないでくださいことを確認できるように、ビューではなく PartialView おも返します。*

GenreMenu コント ローラーのアクションを右クリックし、次に示すように、ジャンル ビューのデータ クラスを使用して型指定された厳密 GenreMenu をという名前の部分ビューを作成します。

![](mvc-music-store-part-10/_static/image4.png)

次のように、順序なしリストを使用して項目を表示する GenreMenu 部分ビューのコードの表示を更新します。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>部分ビューを表示するサイトのレイアウトの更新

サイトのレイアウトに、部分ビューを追加できます (/ビュー/共有\_Layout.cshtml) Html.RenderAction() を呼び出すことによってです。 次に示すよう、これらの両方をだけでなく、それらを表示するマークアップを追加するいくつかを追加おします。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

今すぐアプリケーションを実行したときに、左側のナビゲーション領域のジャンル、上部にあるカート概要が表示されます。

## <a name="update-to-the-store-browse-page"></a>ストアの参照 ページを更新します。

ストアを参照ページでは、機能は、非常に良いが正しく表示されません。 優れたレイアウトで、コードの表示 (/Views/Store/Browse.cshtml にあります) 次のように更新することで、アルバムを表示するページを更新することをがします。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

ここで行っているお Html.ActionLink ではなく Url.Action の使用して、アルバム アートを含むへのリンクに特殊な書式設定を適用したようにします。

*注: これらのアルバムの汎用のアルバム カバーが表示されます。この情報は、データベースに格納され、ストア マネージャーを使用して編集可能な。独自のアートワークを追加するへようこそ があります。*

今すぐジャンルをブラウズ、ときにアルバム アートを持つグリッドに表示されるアルバムが表示されます。

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>上部の販売アルバムを表示するホーム ページの更新

売上増大をホーム ページ上のアルバムを販売している、上位の機能をさせていただきます。 ハンドルを追加するいくつか追加のグラフィック同様に、テンプレートを使用するには一部の更新プログラムにしましょう。

最初に、EntityFramework が関連付けられていることを認識できるように、アルバム クラスにナビゲーション プロパティを追加します。 最後の数行、**アルバム**クラスは次のようになります。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*注: これを追加する必要を使用して、ステートメントを System.Collections.Generic 名前空間にします。*

まず、storeDB フィールドと、他のコント ローラーと同様に、ステートメントを使用して MvcMusicStore.Models 追加します。 次に、次のメソッドを OrderDetails に従ってトップ販売アルバムを検索するデータベースを照会する HomeController お追加します。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

これは、コント ローラーのアクションとして使用できるようにしたくありませんので、プライベート メソッドです。 わかりやすくするため、テンプレートを使用するに含めることが、必要に応じて別のサービス クラスに、ビジネス ロジックを移動することをお勧めします。

インプレースで、Index コント ローラー アクションは、上位 5 販売アルバムをクエリおよびビューに戻すに更新することができます。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

更新された HomeController の完全なコードは、次に示すようはします。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

最後に、モデルの種類を更新して、最下位にアルバム リストを追加することによってアルバムの一覧を表示できるように、ホーム インデックス ビューを更新する必要があります。 私たちもページに見出しセクションがあり、プロモーションを追加するには、この機会になります。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

今すぐアプリケーションを実行したときにお最上位の販売アルバムやプロモーション、メッセージ、更新のホーム ページが表示されます。

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>まとめ

ある ASP.NET MVC 簡単これまで見てきたなどのデータベース アクセス、メンバーシップ、AJAX、高度な web サイトを作成します。 非常に短時間です。 おそらくこのチュートリアルが提供されているツールを独自の ASP.NET MVC アプリケーションの構築を開始する必要があります!


>[!div class="step-by-step"]
[前へ](mvc-music-store-part-9.md)
