---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dynamic v します。 ビューを厳密に型指定 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: bd00fccc44019b2e401247de1532d2abcb5dd396
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578504"
---
<a name="dynamic-v-strongly-typed-views"></a>Dynamic v します。 厳密に型指定されたビュー
====================
によって[Rick Anderson]((https://twitter.com/RickAndMSFT))

ASP.NET MVC 3 でビュー コント ローラーから情報を渡すための 3 つの方法はあります。

1. 厳密に型指定されたモデル オブジェクト。
2. 動的な型として (を使用して@model動的)
3. ViewBag を使用します。

動的かつ厳密に型指定されたビューを比較して単純な MVC 3 上位ブログ アプリケーション作成しました。 ブログの単純なリストから始まって、コント ローラー。

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

IndexNotStonglyTyped() メソッド内を右クリックし、Razor ビューを追加します。

[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

確認、**厳密に型指定されたビューを作成**ボックスがオンになっていません。 結果のビューには、ほとんどが含まれていません。

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

動的なと厳密に型指定されたビューではなくを使用するため、intellisense が役立ちます。 完成したコードは、以下に示します。

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

厳密に型指定されたビューを追加しましょう。 コント ローラーには、次のコードを追加します。

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


正確に、同じ戻り View(topBlogs); に注目してください。非厳密に型指定されたビューとして呼び出します。 内側を右クリックして*StonglyTypedIndex()* 選択**ビューの追加**します。 この時間を選択、**ブログ**モデル クラスと選択**一覧**スキャフォールディング テンプレートとして。

[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

新しいビュー テンプレートの内部では、intellisense サポートを取得します。

[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

C# プロジェクトをダウンロードできます[ここ](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip)します。
