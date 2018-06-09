---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dynamic v します。 ビューを厳密に型指定 |Microsoft ドキュメント
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "26504091"
---
<a name="dynamic-v-strongly-typed-views"></a>Dynamic v します。 厳密に型指定されたビュー
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

ASP.NET MVC 3 でビューに、コント ローラーから情報を渡すための 3 つの方法があります。

1. として厳密に型指定されたモデル オブジェクトです。
2. 動的な型として (を使用して@model動的)
3. ViewBag を使用します。

比較検討して動的および厳密に型指定されたビューの単純な MVC 3 上位ブログ アプリケーションを作成しました。 ブログの単純なリストから始まり、コント ローラー。

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

IndexNotStonglyTyped() メソッドを右クリックし、Razor ビューを追加します。

[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

確認してください、**厳密に型指定されたビューを作成する**ボックスをオフになっています。 結果のビューには、大部分が含まれていません。

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

動的なと厳密に型指定されたビューではなくを使用しているため intellisense しないにご協力します。 完成したコードは、次に示します。

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

厳密に型指定されたビューを追加します。 コント ローラーに次のコードを追加します。

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


正確に、同じ戻り View(topBlogs); であることを確認します。非厳密に型指定されたビューを呼び出します。 内側を右クリックして*StonglyTypedIndex()* 選択**ビューの追加**です。 この時間を選択、**ブログ**モデル クラスを選択**リスト**Scaffold テンプレートとして。

[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

新しいビュー テンプレートの内部は、intellisense のサポートを取得します。

[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

C# プロジェクトをダウンロードできる[ここ](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip)です。
