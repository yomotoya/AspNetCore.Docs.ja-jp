---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: キャッシュされたページ (VB) への動的なコンテンツの追加 |Microsoft ドキュメント
author: microsoft
description: 同じページで、動的およびキャッシュされたコンテンツを混在させる方法を説明します。 キャッシュ後置換では、バナー広告 o などの動的なコンテンツを表示することができます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 89421b4bec2170e408ded87ccc918a7a16844a98
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879765"
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a>キャッシュされたページ (VB) に動的なコンテンツを追加します。
====================
によって[Microsoft](https://github.com/microsoft)

> 同じページで、動的およびキャッシュされたコンテンツを混在させる方法を説明します。 キャッシュ後置換では、バナー広告またはキャッシュされた出力されているページ内のニュース項目などの動的なコンテンツを表示することができます。


出力キャッシュの利用して、ASP.NET MVC アプリケーションのパフォーマンスを大幅に向上できます。 ページ、ページが要求されるたびに、再生成するのではなくページを 1 回生成し、複数のユーザー用のメモリにキャッシュすることができます。

問題があります。 場合、ページに動的なコンテンツを表示する必要がありますか。 たとえば、バナー広告をページに表示することを想像してください。 すべてのユーザーが非常に同じ提供情報を表示するようにキャッシュされるバナー広告をしたくありません。 このように、money を行うはありません!

幸いにも、簡単な解決策があります。 呼ばれる、ASP.NET フレームワークの機能の活用*キャッシュ後置換*です。 キャッシュ後置換では、メモリにキャッシュされているページの動的コンテンツを代用することができます。


通常、出力する場合、このページを使用して、キャッシュ、 &lt;OutputCache&gt;サーバーとクライアント (web ブラウザー) の両方で、属性、ページがキャッシュされます。 キャッシュ後置換を使用すると、ページは、サーバー上でのみキャッシュされます。


#### <a name="using-post-cache-substitution"></a>キャッシュ後置換を使用します。

キャッシュ後置換を使用するには、2 つの手順が必要です。 最初に、キャッシュされたページに表示する動的なコンテンツを表す文字列を返すメソッドを定義する必要があります。 次に、ページに動的なコンテンツを注入 HttpResponse.WriteSubstitution() メソッドを呼び出します。

たとえば、キャッシュされたページの異なるニュース項目をランダムに表示することに想像してください。 リスト 1 のクラスは、ランダムに 3 つのニュース項目の一覧から 1 つのニュース項目を返す RenderNews() をという名前の 1 つのメソッドを公開します。

**1 – Models\News.vb を一覧表示します。**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

キャッシュ後置換を利用するには、HttpResponse.WriteSubstitution() メソッドを呼び出します。 WriteSubstitution() メソッドでコードを設定すると、動的コンテンツを含む、キャッシュされたページの領域を置換します。 WriteSubstitution() メソッドがビューを一覧表示する 2 で、ランダムなニュース項目を表示するために使用します。

**Listing 2 – Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

RenderNews メソッドは、WriteSubstitution() メソッドに渡されます。 RenderNews メソッドが呼び出されないことに注意してください。 代わりに、メソッドへの参照は、AddressOf 演算子を利用して WriteSubstitution() に渡されます。

インデックス ビューがキャッシュされます。 ビューがリスト 3 のコント ローラーが返されます。 Index() アクションで装飾されてことに注意してください、 &lt;OutputCache&gt;を 60 秒間キャッシュに保存するインデックス ビューの原因となる属性。

**3 – Controllers\HomeController.vb を一覧表示します。**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

場合でも、インデックス ビューをキャッシュすると、インデックス ページを要求するときに別のランダムなニュース項目が表示されます。 インデックス ページを要求するときにページが表示される時刻は 60 秒間の (図 1 を参照してください) は変更されません。 時刻が変更されないという事実は、ページがキャッシュされていることを証明します。 各要求と一緒に WriteSubstitution() メソッド – ランダムなニュース項目 – 変更によって、コンテンツが挿入ただし、します。

**図 1 – キャッシュされたページ内の動的なニュース項目を挿入**

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>ヘルパー メソッドでのキャッシュ後置換の使用

キャッシュ後置換を活用する簡単な方法では、カスタム ヘルパー メソッド内で WriteSubstitution() メソッドへの呼び出しをカプセル化します。 このアプローチは、ヘルパー メソッドを一覧表示する 4 で説明されています。

**4 – Helpers\AdHelper.vb を一覧表示します。**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

2 つのメソッドを公開する Visual Basic モジュールを含むリスト 4: RenderBanner() と RenderBannerInternal() です。 RenderBanner() メソッドは、実際のヘルパー メソッドを表します。 このメソッドは、他のヘルパー メソッドと同じようにビューで Html.RenderBanner() を呼び出すことができるように、標準の ASP.NET MVC の HtmlHelper クラスを拡張します。

RenderBanner() メソッドは、RenderBannerInternal() メソッドを WriteSubsitution() メソッドに渡す HttpResponse.WriteSubstitution() メソッドを呼び出します。

RenderBannerInternal() メソッドは、プライベート メソッドです。 このメソッドは、ヘルパー メソッドとして公開されません。 RenderBannerInternal() メソッドは、バナー広告の 3 つのイメージの一覧から、バナー広告の 1 つのイメージをランダムに返します。

インデックス ビューを一覧表示する 5 では、RenderBanner() ヘルパー メソッドを使用する方法を示しています。 注意して、追加&lt;% @ インポート %&gt;ディレクティブは MvcApplication1.Helpers 名前空間をインポートするビューの上部に含まれます。 この名前空間をインポートした場合、RenderBanner() メソッドは Html プロパティのメソッドとして表示されません。

**5 – Views\Home\Index.aspx で (RenderBanner() メソッド) を一覧表示します。**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

5 の一覧表示するビューで表示されたページを要求すると、要求ごとに異なるバナー広告が表示されます (図 2 を参照してください)。 ページがキャッシュされるが、RenderBanner() ヘルパー メソッドによって、バナー広告が動的に挿入します。

**図 2 – ランダム バナー広告を表示するインデックス ビュー**

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a>まとめ

このチュートリアルでは、キャッシュされたページのコンテンツを動的に更新する方法について説明します。 HttpResponse.WriteSubstitution() メソッドを使用して、キャッシュされたページに挿入する動的なコンテンツを有効にする方法を学習しました。 HTML ヘルパー メソッド内で WriteSubstitution() メソッドへの呼び出しをカプセル化する方法も学習しました。

利用可能な限り – 持つ、web アプリケーションのパフォーマンスに大幅な影響を受けることができますをキャッシュします。 このチュートリアルで説明、キャッシュ、ページに動的なコンテンツを表示する必要がある場合にも利用できます。

> [!div class="step-by-step"]
> [前へ](improving-performance-with-output-caching-vb.md)
> [次へ](creating-a-controller-vb.md)
