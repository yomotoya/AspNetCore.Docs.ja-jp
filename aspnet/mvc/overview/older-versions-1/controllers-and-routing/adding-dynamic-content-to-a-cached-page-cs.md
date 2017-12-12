---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: "キャッシュされたページ (c#) への動的なコンテンツの追加 |Microsoft ドキュメント"
author: microsoft
description: "同じページで、動的およびキャッシュされたコンテンツを混在させる方法を説明します。 キャッシュ後置換では、バナー広告 o などの動的なコンテンツを表示することができます。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: bee7e17ee16d75419c215558b1deb7d6f0d79448
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a>動的なコンテンツをキャッシュされたページ (c#) に追加します。
====================
によって[Microsoft](https://github.com/microsoft)

> 同じページで、動的およびキャッシュされたコンテンツを混在させる方法を説明します。 キャッシュ後置換では、バナー広告またはキャッシュされた出力されているページ内のニュース項目などの動的なコンテンツを表示することができます。


出力キャッシュの利用して、ASP.NET MVC アプリケーションのパフォーマンスを大幅に向上できます。 ページ、ページが要求されるたびに、再生成するのではなくページを 1 回生成し、複数のユーザー用のメモリにキャッシュすることができます。

問題があります。 場合、ページに動的なコンテンツを表示する必要がありますか。 たとえば、バナー広告をページに表示することを想像してください。 すべてのユーザーが非常に同じ提供情報を表示するようにキャッシュされるバナー広告をしたくありません。 このように、money を行うはありません!

幸いにも、簡単な解決策があります。 呼ばれる、ASP.NET フレームワークの機能の活用*キャッシュ後置換*です。 キャッシュ後置換では、メモリにキャッシュされているページの動的コンテンツを代用することができます。


通常は、[OutputCache] 属性を使用してキャッシュ ページを出力すると、ページは、サーバーとクライアント (web ブラウザー) の両方でキャッシュされます。 キャッシュ後置換を使用すると、ページは、サーバー上でのみキャッシュされます。


#### <a name="using-post-cache-substitution"></a>キャッシュ後置換を使用します。

キャッシュ後置換を使用するには、2 つの手順が必要です。 最初に、キャッシュされたページに表示する動的なコンテンツを表す文字列を返すメソッドを定義する必要があります。 次に、ページに動的なコンテンツを注入 HttpResponse.WriteSubstitution() メソッドを呼び出します。

たとえば、キャッシュされたページの異なるニュース項目をランダムに表示することに想像してください。 リスト 1 のクラスは、ランダムに 3 つのニュース項目の一覧から 1 つのニュース項目を返す RenderNews() をという名前の 1 つのメソッドを公開します。

**1 – Models\News.cs を一覧表示します。**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

キャッシュ後置換を利用するには、HttpResponse.WriteSubstitution() メソッドを呼び出します。 WriteSubstitution() メソッドでコードを設定すると、動的コンテンツを含む、キャッシュされたページの領域を置換します。 WriteSubstitution() メソッドがビューを一覧表示する 2 で、ランダムなニュース項目を表示するために使用します。

**2 – Views\Home\Index.aspx を一覧表示します。**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

RenderNews メソッドは、WriteSubstitution() メソッドに渡されます。 RenderNews メソッドが呼び出されないことに注意してください (かっこはありません)。 代わりに、メソッドへの参照は、WriteSubstitution() に渡されます。

インデックス ビューがキャッシュされます。 ビューがリスト 3 のコント ローラーが返されます。 Index() アクションが 60 秒間キャッシュに保存するインデックス ビューの原因となる [OutputCache] 属性で修飾されたことに注意してください。

**3 – controllers \homecontroller.cs を一覧表示します。**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

場合でも、インデックス ビューをキャッシュすると、インデックス ページを要求するときに別のランダムなニュース項目が表示されます。 インデックス ページを要求するときにページが表示される時刻は 60 秒間の (図 1 を参照してください) は変更されません。 時刻が変更されないという事実は、ページがキャッシュされていることを証明します。 各要求と一緒に WriteSubstitution() メソッド – ランダムなニュース項目 – 変更によって、コンテンツが挿入ただし、します。

**図 1 – キャッシュされたページ内の動的なニュース項目を挿入**

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>ヘルパー メソッドでのキャッシュ後置換の使用

キャッシュ後置換を活用する簡単な方法では、カスタム ヘルパー メソッド内で WriteSubstitution() メソッドへの呼び出しをカプセル化します。 このアプローチは、ヘルパー メソッドを一覧表示する 4 で説明されています。

**4 – AdHelper.cs を一覧表示します。**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

4 を一覧表示するには、2 つのメソッドを公開する静的クラスが含まれます: RenderBanner() と RenderBannerInternal() です。 RenderBanner() メソッドは、実際のヘルパー メソッドを表します。 このメソッドは、他のヘルパー メソッドと同じようにビューで Html.RenderBanner() を呼び出すことができるように、標準の ASP.NET MVC の HtmlHelper クラスを拡張します。

RenderBanner() メソッドは、RenderBannerInternal() メソッドを WriteSubsitution() メソッドに渡す HttpResponse.WriteSubstitution() メソッドを呼び出します。

RenderBannerInternal() メソッドは、プライベート メソッドです。 このメソッドは、ヘルパー メソッドとして公開されません。 RenderBannerInternal() メソッドは、バナー広告の 3 つのイメージの一覧から、バナー広告の 1 つのイメージをランダムに返します。

インデックス ビューを一覧表示する 5 では、RenderBanner() ヘルパー メソッドを使用する方法を示しています。 注意して、追加&lt;% @ インポート %&gt;ディレクティブは MvcApplication1.Helpers 名前空間をインポートするビューの上部に含まれます。 この名前空間をインポートした場合、RenderBanner() メソッドは Html プロパティのメソッドとして表示されません。

**5 – Views\Home\Index.aspx で (RenderBanner() メソッド) を一覧表示します。**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

5 の一覧表示するビューで表示されたページを要求すると、要求ごとに異なるバナー広告が表示されます (図 2 を参照してください)。 ページがキャッシュされるが、RenderBanner() ヘルパー メソッドによって、バナー広告が動的に挿入します。

**図 2 – ランダム バナー広告を表示するインデックス ビュー**

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a>概要

このチュートリアルでは、キャッシュされたページのコンテンツを動的に更新する方法について説明します。 HttpResponse.WriteSubstitution() メソッドを使用して、キャッシュされたページに挿入する動的なコンテンツを有効にする方法を学習しました。 HTML ヘルパー メソッド内で WriteSubstitution() メソッドへの呼び出しをカプセル化する方法も学習しました。

利用可能な限り – 持つ、web アプリケーションのパフォーマンスに大幅な影響を受けることができますをキャッシュします。 このチュートリアルで説明、キャッシュ、ページに動的なコンテンツを表示する必要がある場合にも利用できます。

## 

## 

>[!div class="step-by-step"]
[前へ](improving-performance-with-output-caching-cs.md)
[次へ](creating-a-controller-cs.md)
