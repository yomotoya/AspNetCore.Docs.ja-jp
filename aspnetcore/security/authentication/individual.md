---
title: 個々 のユーザー アカウントで作成した ASP.NET Core プロジェクトに基づく記事
author: rick-anderson
description: 個々 のユーザー アカウントで作成した ASP.NET Core プロジェクトに基づくアーティクルを検出します。
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: c73365eafaf2c38ef02c3c83ccf5ced4264f7dc0
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2019
ms.locfileid: "56743775"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="1bf7b-103">個々 のユーザー アカウントで作成した ASP.NET Core プロジェクトに基づく記事</span><span class="sxs-lookup"><span data-stu-id="1bf7b-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="1bf7b-104">ASP.NET Core Identity は、「個々 のユーザー アカウント」オプションを使用して Visual Studio でプロジェクト テンプレートに含まれます。</span><span class="sxs-lookup"><span data-stu-id="1bf7b-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="1bf7b-105">認証のテンプレートは .NET Core CLI を`-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="1bf7b-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="1bf7b-106">参照してください[この GitHub の問題](https://github.com/aspnet/AspNetCore/issues/5833)web API 認証します。</span><span class="sxs-lookup"><span data-stu-id="1bf7b-106">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5833) for web API authentication.</span></span>

<a name="no"></a>
## <a name="no-authentication"></a><span data-ttu-id="1bf7b-107">認証なし</span><span class="sxs-lookup"><span data-stu-id="1bf7b-107">No Authentication</span></span>

<span data-ttu-id="1bf7b-108">.NET Core CLI で認証が指定された、`-au`オプション。</span><span class="sxs-lookup"><span data-stu-id="1bf7b-108">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="1bf7b-109">Visual Studio で、**認証の変更**ダイアログは、新しい web アプリケーションで利用できます。</span><span class="sxs-lookup"><span data-stu-id="1bf7b-109">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="1bf7b-110">Visual Studio で新しい web アプリの既定値は**認証なし**します。</span><span class="sxs-lookup"><span data-stu-id="1bf7b-110">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="1bf7b-111">認証なしで作成されたプロジェクト:</span><span class="sxs-lookup"><span data-stu-id="1bf7b-111">Projects created with no authentication:</span></span>

* <span data-ttu-id="1bf7b-112">Web ページ、サインインし、サインアウトの UI が含まれていません。</span><span class="sxs-lookup"><span data-stu-id="1bf7b-112">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="1bf7b-113">認証コードが含まれていません。</span><span class="sxs-lookup"><span data-stu-id="1bf7b-113">Don't contain authentication code.</span></span>

<a name="win"></a>
## <a name="windows-authentication"></a><span data-ttu-id="1bf7b-114">Windows 認証</span><span class="sxs-lookup"><span data-stu-id="1bf7b-114">Windows Authentication</span></span>

<span data-ttu-id="1bf7b-115">.NET Core CLI で新しい web アプリの Windows 認証が指定されて、`-au Windows`オプション。</span><span class="sxs-lookup"><span data-stu-id="1bf7b-115">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="1bf7b-116">Visual Studio で、**認証の変更**ダイアログ ボックスには、 **Windows 認証**オプション。</span><span class="sxs-lookup"><span data-stu-id="1bf7b-116">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="1bf7b-117">使用するアプリが構成されている Windows 認証が選択されている場合、 [Windows 認証の IIS モジュール](xref:host-and-deploy/iis/modules)します。</span><span class="sxs-lookup"><span data-stu-id="1bf7b-117">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="1bf7b-118">Windows 認証は、イントラネットの web サイトを対象としています。</span><span class="sxs-lookup"><span data-stu-id="1bf7b-118">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1bf7b-119">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="1bf7b-119">Additional resources</span></span>

<span data-ttu-id="1bf7b-120">次の記事では、個々 のユーザー アカウントを使用する ASP.NET Core テンプレートで生成されたコードを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1bf7b-120">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="1bf7b-121">SMS での 2 要素認証</span><span class="sxs-lookup"><span data-stu-id="1bf7b-121">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="1bf7b-122">ASP.NET Core でのアカウントの確認とパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="1bf7b-122">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="1bf7b-123">承認によって保護されたユーザー データと ASP.NET Core アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="1bf7b-123">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
