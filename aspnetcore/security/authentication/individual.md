---
title: 個々 のユーザー アカウントで作成した ASP.NET Core プロジェクトに基づく記事
author: rick-anderson
description: 個々 のユーザー アカウントで作成した ASP.NET Core プロジェクトに基づくアーティクルを検出します。
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: ac843342ffc73632fbf9f6359c6c1a5878dcef0d
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523065"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="eaf94-103">個々 のユーザー アカウントで作成した ASP.NET Core プロジェクトに基づく記事</span><span class="sxs-lookup"><span data-stu-id="eaf94-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="eaf94-104">ASP.NET Core Identity は、「個々 のユーザー アカウント」オプションを使用して Visual Studio でプロジェクト テンプレートに含まれます。</span><span class="sxs-lookup"><span data-stu-id="eaf94-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="eaf94-105">認証のテンプレートは .NET Core CLI を`-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="eaf94-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<a name="no"></a>
## <a name="no-authentication"></a><span data-ttu-id="eaf94-106">認証なし</span><span class="sxs-lookup"><span data-stu-id="eaf94-106">No Authentication</span></span>

<span data-ttu-id="eaf94-107">.NET Core CLI で認証が指定された、`-au`オプション。</span><span class="sxs-lookup"><span data-stu-id="eaf94-107">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="eaf94-108">Visual Studio で、**認証の変更**ダイアログは、新しい web アプリケーションで利用できます。</span><span class="sxs-lookup"><span data-stu-id="eaf94-108">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="eaf94-109">Visual Studio で新しい web アプリの既定値は**認証なし**します。</span><span class="sxs-lookup"><span data-stu-id="eaf94-109">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="eaf94-110">認証なしで作成されたプロジェクト:</span><span class="sxs-lookup"><span data-stu-id="eaf94-110">Projects created with no authentication:</span></span>

* <span data-ttu-id="eaf94-111">Web ページ、サインインし、サインアウトの UI が含まれていません。</span><span class="sxs-lookup"><span data-stu-id="eaf94-111">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="eaf94-112">認証コードが含まれていません。</span><span class="sxs-lookup"><span data-stu-id="eaf94-112">Don't contain authentication code.</span></span>

<a name="win"></a>
## <a name="windows-authentication"></a><span data-ttu-id="eaf94-113">Windows 認証</span><span class="sxs-lookup"><span data-stu-id="eaf94-113">Windows Authentication</span></span>

<span data-ttu-id="eaf94-114">.NET Core CLI で新しい web アプリの Windows 認証が指定されて、`-au Windows`オプション。</span><span class="sxs-lookup"><span data-stu-id="eaf94-114">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="eaf94-115">Visual Studio で、**認証の変更**ダイアログ ボックスには、 **Windows 認証**オプション。</span><span class="sxs-lookup"><span data-stu-id="eaf94-115">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="eaf94-116">使用するアプリが構成されている Windows 認証が選択されている場合、 [Windows 認証の IIS モジュール](xref:host-and-deploy/iis/modules)します。</span><span class="sxs-lookup"><span data-stu-id="eaf94-116">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="eaf94-117">Windows 認証は、イントラネットの web サイトを対象としています。</span><span class="sxs-lookup"><span data-stu-id="eaf94-117">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="eaf94-118">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="eaf94-118">Additional resources</span></span>

<span data-ttu-id="eaf94-119">次の記事では、個々 のユーザー アカウントを使用する ASP.NET Core テンプレートで生成されたコードを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="eaf94-119">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="eaf94-120">SMS での 2 要素認証</span><span class="sxs-lookup"><span data-stu-id="eaf94-120">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="eaf94-121">ASP.NET Core でのアカウントの確認とパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="eaf94-121">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="eaf94-122">承認によって保護されたユーザー データと ASP.NET Core アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="eaf94-122">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
