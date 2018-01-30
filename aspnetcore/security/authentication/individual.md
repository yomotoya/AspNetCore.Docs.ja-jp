---
title: "個々 のユーザー アカウントで作成したプロジェクトに基づくアーティクル"
author: rick-anderson
description: "このドキュメントでは、個々 のユーザー アカウントで作成したプロジェクトに基づくアーティクルが一覧表示します。"
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: aee18fa08fbc5c8452ca2b401d32858edaf55e7c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a><span data-ttu-id="a5046-103">個々 のユーザー アカウントで作成したプロジェクトに基づくアーティクル</span><span class="sxs-lookup"><span data-stu-id="a5046-103">Articles based on projects created with individual user accounts</span></span>

<span data-ttu-id="a5046-104">ASP.NET Core の Id は、「個々 のユーザー アカウント」オプションを使用して Visual Studio でプロジェクト テンプレートに含まれます。</span><span class="sxs-lookup"><span data-stu-id="a5046-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="a5046-105">認証テンプレートを利用できますと .NET Core CLI で`-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="a5046-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

<span data-ttu-id="a5046-106">次の記事では、個々 のユーザー アカウントを使用する ASP.NET Core テンプレートで生成されたコードを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a5046-106">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="a5046-107">SMS での 2 要素認証</span><span class="sxs-lookup"><span data-stu-id="a5046-107">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="a5046-108">ASP.NET Core でのアカウントの確認とパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="a5046-108">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="a5046-109">認証によって保護されているユーザー データと ASP.NET Core アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="a5046-109">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
