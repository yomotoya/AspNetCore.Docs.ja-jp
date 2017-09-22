---
title: "ASP.NET Core アプリケーションで SSL を適用します。"
author: rick-anderson
description: "Web アプリの ASP.NET Core での SSL を要求する方法を示します"
keywords: "ASP.NET Core、SSL、HTTPS、RequireHttpsAttribute、IIS Express"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: e8e7d4a69fd681534fb313ff113805bfd6a6d44e
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
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

HTTPS をグローバルに必要とする (`options.Filters.Add(new RequireHttpsAttribute());`) は、セキュリティのベスト プラクティスです。 適用する、`[RequireHttps]`属性をすべてのコント ローラーがグローバルに HTTPS を必要とすると、セキュリティで保護されたと見なされない。 保証できない場合、アプリに追加された新しいコント ローラーを忘れずに適用、`[RequireHttps]`属性。

## <a name="set-up-iis-express-for-sslhttps"></a>SSL または HTTPS の IIS Express を設定します。

参照してください[ASP.NET Core での開発の HTTPS を設定](xref:security/https#iisxpress)です。
