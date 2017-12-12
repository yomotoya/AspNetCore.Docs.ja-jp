---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: "パート 3: レイアウトとカテゴリ メニュー |Microsoft ドキュメント"
author: JoeStagner
description: "このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 パート 3 は、追加のレイアウトとカテゴリのメニューについて説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 57c0342efb67b94a0d8c8b06dc13a727e7184db8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-3-layout-and-category-menu"></a>パート 3: レイアウトとカテゴリのメニュー
====================
によって[行える](https://github.com/JoeStagner)

> Tailspin Spyworks では、.NET プラットフォーム用の強力な拡張性の高いアプリケーションを作成するはどのように非常に単純なを示しています。 ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアをビルドする方法を示します。
> 
> このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 パート 3 は、追加のレイアウトとカテゴリのメニューについて説明します。


## <a id="_Toc260221669"></a>一部のレイアウトとカテゴリのメニューを追加します。

このサイトのマスター ページで、左側にあるを含む列に、製品カテゴリ メニューの div を追加します。

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

目的の整列やその他の書式設定を Style.css ファイルに追加した CSS クラスによって提供されることに注意してください。

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

製品のカテゴリのメニューは、実行時で動的に既存の製品カテゴリとメニュー項目を作成、および対応するリンクの Commerce データベースのクエリを実行して作成されます。

これを実現するには、2 つの ASP を使用します。NET の強力なデータを制御します。 [エンティティ データ ソース] コントロールは、"ListView"コントロール。

![](tailspin-spyworks-part-3/_static/image1.jpg)

「デザイン ビュー」にし、コントロールを設定するヘルパーを使用してみましょう。

![](tailspin-spyworks-part-3/_static/image2.jpg)

Eds EntityDataSource ID プロパティを設定してみましょう\_カテゴリ\_メニューと「データ ソースの構成」をクリックします。

![](tailspin-spyworks-part-3/_static/image3.jpg)

ご利用の米国 Commerce データベースのエンティティのデータ ソースのモデルを作成したときに作成された CommerceEntities 接続を選択し、[次へ] をクリックします。

![](tailspin-spyworks-part-3/_static/image4.jpg)

"Categories"エンティティ セットの名前を選択し、既定値としての残りのオプションのままにします。 [完了] をクリックします。

ここで、ページを ListView に配置しているお ListView コントロールのインスタンスの ID プロパティを設定してみましょう\_ProductsMenu とそのヘルパーをアクティブ化します。

![](tailspin-spyworks-part-3/_static/image5.jpg)

ただし管理のオプションを使用してデータ項目の表示の書式を設定して、書式設定、メニューの作成のみが必要になります単純なマークアップのため、コード、ソース ビューに入力されます。

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

"Eval"ステートメントに注意してください: &lt;%# Eval("CategoryName") %&gt;

ASP.NET の構文&lt;%# %&gt;内に含まれるものと、"行"の結果を出力を実行するランタイムに指示するためのショートハンド規約です。

エンティティ モデル アイテム名の値を"CatagoryName"にフェッチ、Eval("CategoryName") ステートメントをデータ項目のバインドされたコレクションの現在のエントリのです。 これは、非常に強力な機能の簡潔な構文です。

今すぐアプリケーションを実行することができます。

![](tailspin-spyworks-part-3/_static/image6.jpg)

当社製品カテゴリ メニューに表示されていますしおよび ProductsList.aspx ときにマウス ポインターを移動 メニュー項目のリンク先を実装するには、まだあるページに表示カテゴリ メニュー項目の 1 つという名前を作成した、動的なクエリ文字列引数を含む、 カテゴリ id。

>[!div class="step-by-step"]
[前へ](tailspin-spyworks-part-2.md)
[次へ](tailspin-spyworks-part-4.md)
