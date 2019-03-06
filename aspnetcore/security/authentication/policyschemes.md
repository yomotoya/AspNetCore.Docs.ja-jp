---
title: ASP.NET Core でのポリシー設定
author: rick-anderson
description: ポリシーの認証スキーム容易に 1 つの論理認証方式を使用して
ms.author: riande
ms.date: 2/28/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: c310b61e14df2b7846e32a602bb75914a5850aff
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346779"
---
# <a name="policy-schemes-in-aspnet-core"></a>ASP.NET Core でのポリシー設定

ポリシーの認証スキームでは、複数のアプローチを使用して、可能性のある 1 つの論理認証方式を使用してやすいようにします。 たとえば、ポリシー スキーム可能性がありますの課題、Google の認証と認証を使用 cookie 以外のすべての。 ポリシーの認証スキームを使用すると、そのように。

* 簡単に認証操作を別の構成を転送します。
* 要求に基づいて動的に転送します。

使用して派生したすべての認証スキーム<xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions>および関連付けられた[ `AuthenticationHandler<TOptions>` ](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):

* ASP.NET Core 2.1 以降のポリシー設定は自動的に。
* スキームのオプションの構成を使用して有効にすることができます。

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a>使用例

次の例では、下位レベルのスキームを組み合わせてより高いレベルの体系を示します。 課題、Google 認証を使用して、他のすべての cookie 認証を使用します。

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

次の例で、要求ごとにパターンを動的に選択します。 つまり、cookie、および API の認証を混在させる方法。

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
