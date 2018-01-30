---
title: "ASP.NET Core アプリケーションで SSL を適用します。"
author: rick-anderson
description: "Web アプリの ASP.NET Core での SSL を要求する方法を示します"
manager: wpickett
ms.author: riande
ms.date: 07/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 2e0a2f4732e574c80ceef8fd21a530a11aef254c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a>ASP.NET Core アプリケーションで SSL を適用します。

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

このドキュメントで説明する方法。

- (HTTPS 要求のみ) のすべての要求に対して SSL を要求します。
- すべての HTTP 要求を HTTPS にリダイレクトします。

## <a name="require-ssl"></a>SSL を必須にする

[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) SSL を要求するために使用します。 コント ローラーまたはこの属性を持つメソッドを装飾することができますか、次のようにグローバルに適用できます。

次のコードを追加`ConfigureServices`で`Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

上記の強調表示されたコードでは、すべての要求を使用して必要があります`HTTPS`、したがって HTTP 要求は無視されます。 次の強調表示されたコードは、すべての HTTP 要求を HTTPS にリダイレクトします。

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

参照してください[URL 書き換えミドルウェア](xref:fundamentals/url-rewriting)詳細についてはします。

HTTPS をグローバルに必要とする (`options.Filters.Add(new RequireHttpsAttribute());`) は、セキュリティのベスト プラクティスです。 適用する、`[RequireHttps]`属性すべてのコント ローラーをグローバルに HTTPS を必要とすると安全はありません。 保証できない場合、アプリに追加された新しいコント ローラーを忘れずに適用、`[RequireHttps]`属性。
