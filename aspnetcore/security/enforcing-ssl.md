---
title: ASP.NET Core で HTTPS を適用します。
author: rick-anderson
description: Web アプリで ASP.NET Core HTTPS/TLS を必要とする方法を示します。
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 2ebb975e1ea17698cee13ca00d3f5df4a5135e38
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a>ASP.NET Core で HTTPS を適用します。

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

このドキュメントでは次の方法について説明します:

- すべての要求に HTTPS を必要とさせる。
- すべての HTTP 要求を HTTPS にリダイレクトさせる。

> [!WARNING]
> 機密情報を受信する Web Api で `RequireHttpsAttribute` を使用**しない**でください。 `RequireHttpsAttribute` はブラウザーを HTTP から HTTPS へリダイレクトするために HTTP ステータス コードを使用します。 API クライアントはこれを理解しない場合や、HTTP から HTTPS へのリダイレクトに従わない場合があります。 このようなクライアントは、HTTP 経由で情報を送信することがあります。 Web API は次のいずれかの対策を講じるべきです:
>
>* HTTP をリッスンしない。
>* ステータス コード 400 (無効な要求) で接続を閉じ、要求に応答しない。


## <a name="require-https"></a>HTTPS が必要

[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) HTTPS を要求するために使用します。 `[RequireHttpsAttribute]` 装飾できるは、メソッドまたはコント ローラーまたはグローバルに適用することができます。 属性をグローバルに適用するには、次のコードを追加`ConfigureServices`で`Startup`:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

前の強調表示されたコードでは、すべての要求を使用して`HTTPS`です。 そのため、HTTP 要求は無視されます。 次の強調表示されたコードは、すべての HTTP 要求を HTTPS にリダイレクトします。

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

さらに詳しい情報は、[URL 書き換えミドルウェア](xref:fundamentals/url-rewriting) を参照してください。

HTTPS をグローバルに必要とする (`options.Filters.Add(new RequireHttpsAttribute());`) は、セキュリティのベスト プラクティスです。 適用する、`[RequireHttps]`属性をすべてのコント ローラー/Razor ページされていないグローバルに HTTPS を必要とすると、セキュリティで保護されたと見なされます。 保証できません、`[RequireHttps]`属性は、新しいコント ローラーおよび Razor ページが追加されたときに適用します。
