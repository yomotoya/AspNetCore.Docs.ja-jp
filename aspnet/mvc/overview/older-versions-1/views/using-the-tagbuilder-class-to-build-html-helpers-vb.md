---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
title: "HTML ヘルパー (VB) をビルドする TagBuilder クラスを使用して |Microsoft ドキュメント"
author: StephenWalther
description: "Stephen Walther には、名前付き TagBuilder クラス、ASP.NET MVC フレームワークで便利なユーティリティ クラスをについて説明します。 クラスを使用して、TagBuilder を簡単にしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: ec26f264-d0ea-4031-9943-825505a3ac4b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 8d0b3665e9bac6856a3fe1b50b05215f2747e354
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-vb"></a>HTML ヘルパー (VB) をビルドするのに TagBuilder クラスの使用
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther には、名前付き TagBuilder クラス、ASP.NET MVC フレームワークで便利なユーティリティ クラスをについて説明します。 HTML タグを簡単にビルドするのにには、TagBuilder クラスを使用します。


ASP.NET MVC フレームワークには、名前付き TagBuilder クラス HTML ヘルパーを構築するときに使用できる便利なユーティリティ クラスが含まれています。 TagBuilder クラス、クラスの名前からわかるように、使用する HTML タグを簡単に構築できます。 この簡単なチュートリアルでは提供 TagBuilder クラスの概要の説明と HTML を表示する単純な HTML ヘルパーを構築するときに、このクラスを使用する方法について学びます&lt;img&gt;タグ。

## <a name="overview-of-the-tagbuilder-class"></a>TagBuilder クラスの概要

TagBuilder クラスは、System.Web.Mvc 名前空間に含まれています。 5 つの方法があります。

- AddCssClass() – を使用すると、新しい*クラス =""*属性をタグにします。
- GenerateId() – では、id 属性をタグに追加することができます。 このメソッドは、id でピリオドを自動的に置き換えられます (既定では、ピリオド、アンダー スコアに置き換え)
- MergeAttribute() – では、タグに属性を追加することができます。 このメソッドの複数のオーバー ロードがあります。
- SetInnerText() – では、タグの内部テキ ストを設定することができます。 その内部テキ ストは、HTML が自動的にエンコードします。
- ToString() – では、タグをレンダリングすることができます。 通常、タグ、開始タグ、終了タグ、または自己終了タグを作成するかどうかを指定することができます。
  

TagBuilder クラスには、次の 4 つの重要なプロパティがあります。

- 属性: すべてのタグの属性を表します。
- IdAttributeDotReplacement – は、期間 (既定では、アンダー スコア) を置き換える GenerateId() メソッドで使用される文字を表します。
- InnerHTML – は、タグの内部コンテンツを表します。 このプロパティに指定できる文字列*しない*HTML 文字列をエンコードします。
- タグ名 – は、タグの名前を表します。

これらのメソッドとプロパティをすべて提供、基本的なするメソッドとプロパティに HTML タグを作成する必要があります。 本当に TagBuilder クラスを使用する必要はありません。 StringBuilder クラスは、代わりに使用する可能性があります。 ただし、TagBuilder クラス容易に少しです。

## <a name="creating-an-image-html-helper"></a>イメージ HTML ヘルパーの作成

TagBuilder クラスのインスタンスを作成するときは、TagBuilder コンス トラクターにビルドするタグの名前を渡します。 次に、タグの属性を変更する AddCssClass および MergeAttribute() メソッドなどのメソッドを呼び出すことができます。 最後に、タグを表示するために ToString() メソッドを呼び出します。

たとえば、1 一覧表示するにはには、イメージの HTML ヘルパーが含まれています。 イメージ ヘルパーは、HTML を表す TagBuilder で内部的に実装される&lt;img&gt;タグ。

**1 – Helpers\ImageHelper.vb を一覧表示します。**

[!code-vb[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample1.vb)]

リスト 1 のモジュールには、Image() という 2 つのオーバー ロードされたメソッドが含まれています。 Image() メソッドを呼び出すときに、か HTML 属性のセットを表すオブジェクトを渡すことができます。

TagBuilder.MergeAttribute() メソッドを使用、TagBuilder に src 属性などの個々 の属性を追加する方法に注意してください。 注意してください、さらに追加する属性のコレクション、TagBuilder TagBuilder.MergeAttributes() メソッドを使用する方法です。 MergeAttributes() メソッドは、ディクショナリを受け取ります&lt;文字列, オブジェクト&gt;パラメーター。 ディクショナリに属性のコレクションを表すオブジェクトを変換する、RouteValueDictionary クラスが使用される&lt;文字列, オブジェクト&gt;です。

イメージ ヘルパーを作成した後は、その他の標準の HTML ヘルパーのいずれかのように、ASP.NET MVC ビューで、ヘルパーを使用できます。 ビューを一覧表示する 2 で、2 回 Xbox の同じイメージを表示するイメージ ヘルパーを使用して (図 1 を参照してください)。 Image() ヘルパーは、両方および HTML 属性のコレクションを使用せずに呼び出されます。

**2 – Home\Index.aspx を一覧表示します。**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample2.aspx)]


[![[新しいプロジェクト] ダイアログ ボックス](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)

**図 01**: イメージ ヘルパーの使用 ([フルサイズのイメージを表示するをクリックして](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))


Index.aspx ビューの上部にあるイメージ ヘルパーに関連付けられている名前空間をインポートする必要がありますに注意してください。 ヘルパーは、次のディレクティブを使用してインポートは。

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample3.aspx)]

Visual Basic アプリケーションでは、既定の名前空間は、アプリケーションの名前と同じです。

>[!div class="step-by-step"]
[前へ](creating-custom-html-helpers-vb.md)
[次へ](creating-page-layouts-with-view-master-pages-vb.md)
