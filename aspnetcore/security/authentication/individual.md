---
title: "個々 のユーザー アカウントで作成したプロジェクトに基づくアーティクル"
author: rick-anderson
description: "このドキュメントでは、個々 のユーザー アカウントで作成したプロジェクトに基づくアーティクルが一覧表示します。"
keywords: "ASP.NET Core、承認、IAuthorizationService"
ms.author: riande
manager: wpickett
ms.date: 11/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/individual
ms.openlocfilehash: 1864625e0ad6b4ec6fc2ada3fa7d93edec91b633
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/01/2017
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a><span data-ttu-id="db600-104">個々 のユーザー アカウントで作成したプロジェクトに基づくアーティクル</span><span class="sxs-lookup"><span data-stu-id="db600-104">Articles based on projects created with individual user accounts</span></span>

<span data-ttu-id="db600-105">ASP.NET Core の Id は、「個々 のユーザー アカウント」オプションを使用して Visual Studio でプロジェクト テンプレートに含まれます。</span><span class="sxs-lookup"><span data-stu-id="db600-105">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="db600-106">認証テンプレートを利用できますと .NET Core CLI で`-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="db600-106">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

<span data-ttu-id="db600-107">次の記事では、個々 のユーザー アカウントを使用する ASP.NET Core テンプレートで生成されたコードを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="db600-107">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="db600-108">SMS での 2 要素認証</span><span class="sxs-lookup"><span data-stu-id="db600-108">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="db600-109">ASP.NET Core でのアカウントの確認とパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="db600-109">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="db600-110">認証によって保護されているユーザー データと ASP.NET Core アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="db600-110">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)