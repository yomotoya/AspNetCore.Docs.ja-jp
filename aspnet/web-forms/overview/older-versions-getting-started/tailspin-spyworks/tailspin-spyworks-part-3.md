---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'パート 3: レイアウトとカテゴリ メニュー |Microsoft Docs'
author: JoeStagner
description: このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 3 部では、追加のレイアウトとカテゴリ メニューについて説明します。
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3047c53e21a418aef8617bd772a247bc46edb98f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817526"
---
<a name="part-3-layout-and-category-menu"></a>パート 3: レイアウトとカテゴリ メニュー
====================
によって[Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks では、.NET プラットフォーム用の強力でスケーラブルなアプリケーションを作成するはどの非常に単純なを示します。 ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアを構築する方法を示します。
> 
> このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 3 部では、追加のレイアウトとカテゴリ メニューについて説明します。


## <a id="_Toc260221669"></a>  一部のレイアウトとカテゴリ メニューに追加します。

このサイトのマスター ページで、製品カテゴリ メニューを格納するための左側にある列の div を追加します。

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

目的のアラインメントとその他の書式を Style.css ファイルに追加した CSS クラスによって提供されることに注意してください。

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

製品カテゴリ メニューは、実行時で動的に既存の製品カテゴリとメニュー項目を作成、および対応するリンクの Commerce データベースのクエリを実行して作成されます。

これを実現するには、2 つの ASP を使用します。NET の強力なデータを制御します。 「エンティティのデータ ソース」コントロールは、"ListView"コントロール。

![](tailspin-spyworks-part-3/_static/image1.jpg)

「デザイン ビュー」に、ヘルパーを使用して、コントロールを構成してしましょう。

![](tailspin-spyworks-part-3/_static/image2.jpg)

Eds EntityDataSource ID プロパティを設定しましょう\_カテゴリ\_メニューと"データ ソースの構成 をクリックします。

![](tailspin-spyworks-part-3/_static/image3.jpg)

Commerce データベースのエンティティのデータ ソースのモデルを作成したときに、私たちに対して作成された CommerceEntities 接続を選択し、[次へ] をクリックします。

![](tailspin-spyworks-part-3/_static/image4.jpg)

「カテゴリ」のエンティティ セットの名前を選択し、オプションの残りを既定値のままにします。 [完了] をクリックします。

これで、ページ ListView を掲載する ListView コントロールのインスタンスの ID プロパティを設定しましょう\_ProductsMenu とそのヘルパーをアクティブ化します。

![](tailspin-spyworks-part-3/_static/image5.jpg)

ただしデータ項目の表示形式を設定するコントロールのオプションを使用できます、書式設定、メニューの作成のみが必要し、なりますのシンプルなマークアップこれにより、ソース ビューで、コードが入力されます。

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

"Eval"ステートメントに注意してください: &lt;%# Eval("CategoryName") %&gt;

ASP.NET 構文&lt;%# %&gt;内に含まれるものと、"行"の結果を出力の実行に、ランタイムに指示する短縮形規則。

エンティティ モデル アイテムの名前の値は"CatagoryName"フェッチ Eval("CategoryName") ステートメントは、バインドされたデータ項目のコレクションの現在のエントリのように指示します。 これは、非常に強力な機能の簡潔な構文です。

これでアプリケーションを実行することができます。

![](tailspin-spyworks-part-3/_static/image6.jpg)

製品カテゴリ メニューが表示されるようになりましたしと ProductsList.aspx ときにメニュー項目のリンク先を実装するには、まだ私たちがページに表示するカテゴリ メニュー項目の 1 つ以上ポイントという名前を格納する場合は組み込み、動的なクエリ文字列引数を カテゴリの id。

> [!div class="step-by-step"]
> [前へ](tailspin-spyworks-part-2.md)
> [次へ](tailspin-spyworks-part-4.md)
