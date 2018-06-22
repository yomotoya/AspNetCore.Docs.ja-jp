---
title: 個々 のユーザー アカウントで作成した ASP.NET Core プロジェクトに基づくアーティクル
author: rick-anderson
description: 個々 のユーザー アカウントで作成した ASP.NET Core プロジェクトに基づくアーティクルを検出します。
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: f7fb9e8cd1b5c4cc3283ddd7606a0bbd30f554d5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274428"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="b94d2-103">個々 のユーザー アカウントで作成した ASP.NET Core プロジェクトに基づくアーティクル</span><span class="sxs-lookup"><span data-stu-id="b94d2-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="b94d2-104">ASP.NET Core の Id は、「個々 のユーザー アカウント」オプションを使用して Visual Studio でプロジェクト テンプレートに含まれます。</span><span class="sxs-lookup"><span data-stu-id="b94d2-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="b94d2-105">認証テンプレートを利用できますと .NET Core CLI で`-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="b94d2-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="b94d2-107">次の記事では、個々 のユーザー アカウントを使用する ASP.NET Core テンプレートで生成されたコードを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b94d2-107">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="b94d2-108">SMS での 2 要素認証</span><span class="sxs-lookup"><span data-stu-id="b94d2-108">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="b94d2-109">ASP.NET Core でのアカウントの確認とパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="b94d2-109">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="b94d2-110">認証によって保護されているユーザー データと ASP.NET Core アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="b94d2-110">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
