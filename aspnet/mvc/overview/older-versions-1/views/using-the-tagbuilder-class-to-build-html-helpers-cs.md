---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: TagBuilder クラスを使用して、HTML ヘルパー (c#) をビルドする |Microsoft Docs
author: StephenWalther
description: Stephen Walther TagBuilder クラスという名前の ASP.NET MVC フレームワークで便利なユーティリティ クラスを紹介します。 TagBuilder クラスを簡単に使用できます.
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 9990fc7ad8093643a564a5e02ff65264d4a7fe15
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813227"
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a>TagBuilder クラスを使用して、HTML ヘルパー (c#) をビルドするには
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther TagBuilder クラスという名前の ASP.NET MVC フレームワークで便利なユーティリティ クラスを紹介します。 TagBuilder クラスを使用すると、HTML タグを簡単に構築します。


ASP.NET MVC フレームワークには、TagBuilder クラスで HTML ヘルパーを作成するときに使用できるという便利なユーティリティ クラスが含まれています。 TagBuilder クラスでは、クラスの名前からわかるように、使用する HTML タグを簡単に構築できます。 この簡単なチュートリアルでは、TagBuilder クラスの概要が提供され、HTML を表示する単純な HTML ヘルパーをビルドするときに、このクラスを使用する方法を学習します&lt;img&gt;タグ。

## <a name="overview-of-the-tagbuilder-class"></a>TagBuilder クラスの概要

TagBuilder クラスは、System.Web.Mvc 名前空間に含まれます。 5 つのメソッドがあります。

- 新しいを追加することができます - AddCssClass()*クラス =""* タグに属性します。
- GenerateId() - には、id 属性、タグを追加することができます。 このメソッドは id の期間を自動的に置き換えられます (既定では、ピリオド、アンダー スコアに置換)
- MergeAttribute() - には、タグに属性を追加することができます。 このメソッドの複数のオーバー ロードがあります。
- SetInnerText() - には、タグの内部テキ ストを設定することができます。 内部テキ ストは、HTML が自動的にエンコードします。
- ToString() のでは、タグをレンダリングすることができます。 通常のタグ、開始タグ、終了タグ、または自己終了タグを作成するかどうかを指定することができます。
  

TagBuilder クラスでは、次の 4 つの重要なプロパティがあります。

- [属性]-すべてのタグの属性を表します。
- IdAttributeDotReplacement - は、期間 (既定ではアンダー スコア) を置換する GenerateId() メソッドによって使用される文字を表します。
- InnerHTML には、タグの内部の内容を表します。 このプロパティに文字列を割り当てる*しない*HTML 文字列をエンコードします。
- タグ名には、タグの名前を表します。

これらのメソッドとプロパティをすべて提供基本メソッドとプロパティを HTML タグを作成する必要があります。 本当に TagBuilder クラスを使用する必要はありません。 StringBuilder クラスは、代わりに使用する可能性があります。 ただし、TagBuilder クラスで、容易に少し。

## <a name="creating-an-image-html-helper"></a>イメージの HTML ヘルパーの作成

TagBuilder クラスのインスタンスを作成するときは、TagBuilder コンス トラクターにビルドするタグの名前を渡します。 次に、タグの属性を変更する AddCssClass と MergeAttribute() メソッドと同様のメソッドを呼び出すことができます。 最後に、タグを表示するために、ToString() メソッドを呼び出します。

たとえば、リスト 1 には、イメージの HTML ヘルパーが含まれています。 イメージ ヘルパーは HTML を表す TagBuilder で内部的に実装&lt;img&gt;タグ。

**1 - Helpers\ImageHelper.cs を一覧表示します。**

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

リスト 1 でクラスには、2 つの静的なオーバー ロードされたメソッドという名前のイメージにはが含まれています。 Image() メソッドを呼び出すときにをかどうかは、一連の HTML 属性を表すオブジェクトを渡すことができます。

TagBuilder.MergeAttribute() メソッドを使用して、TagBuilder に src 属性などの個々 の属性を追加する方法に注意してください。 確認、さらに、TagBuilder.MergeAttributes() メソッドを使用して、TagBuilder に属性のコレクションを追加する方法。 MergeAttributes() メソッドは、辞書を受け取ります&lt;object&gt;パラメーター。 ディクショナリに属性のコレクションを表すオブジェクトを変換する RouteValueDictionary クラスが使用される&lt;object&gt;します。

イメージ ヘルパーを作成した後は、その他の標準の HTML ヘルパーのいずれかのように、ASP.NET MVC ビューで、ヘルパーを使用できます。 リスト 2 での表示では、イメージ ヘルパーを使用して、2 回、Xbox の同じイメージを表示 (図 1 参照)。 HTML 属性のコレクションの有無にかかわらず、Image() ヘルパーが呼び出されます。

**2 - Home\Index.aspx を一覧表示します。**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


[![[新しいプロジェクト] ダイアログ ボックス](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)

**図 01**: イメージ ヘルパーを使用して ([フルサイズの画像を表示する をクリックします](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))。


Index.aspx ビューの上部にあるイメージ ヘルパーに関連付けられている名前空間をインポートする必要がありますに注意してください。 次のディレクティブを使用して、ヘルパーがインポートされます。

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> [前へ](creating-custom-html-helpers-cs.md)
> [次へ](creating-page-layouts-with-view-master-pages-cs.md)
