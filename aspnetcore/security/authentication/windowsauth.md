---
title: ASP.NET Core で Windows 認証を構成します。
author: ardalis
description: この記事では、IIS Express、IIS、HTTP.sys および WebListener を使用して、ASP.NET Core で Windows 認証を構成する方法について説明します。
manager: wpickett
ms.author: riande
ms.date: 10/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/windowsauth
ms.openlocfilehash: dbcef095561fe656bdd28c4fa6560c6b269a2db0
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/31/2018
ms.locfileid: "34689010"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="2ef23-103">ASP.NET Core で Windows 認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-103">Configure Windows authentication in ASP.NET Core</span></span>

<span data-ttu-id="2ef23-104">作成者: [Steve Smith](https://ardalis.com)、[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="2ef23-104">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="2ef23-105">Windows 認証は、IIS でホストされている ASP.NET Core アプリケーションに対して設定できる[HTTP.sys](xref:fundamentals/servers/httpsys)、または[WebListener](xref:fundamentals/servers/weblistener)です。</span><span class="sxs-lookup"><span data-stu-id="2ef23-105">Windows authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="2ef23-106">Windows 認証とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="2ef23-106">What is Windows authentication?</span></span>

<span data-ttu-id="2ef23-107">Windows 認証は、ASP.NET Core アプリケーションのユーザーを認証するオペレーティング システムに依存します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-107">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="2ef23-108">ユーザーを識別する Active Directory ドメインの id またはその他の Windows アカウントを使用して、企業ネットワークで実行されると、サーバーは、Windows 認証を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="2ef23-108">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="2ef23-109">Windows 認証は、同じ Windows ドメインにユーザー、クライアント アプリケーションおよび web サーバーが属している、イントラネット環境に最も適しています。</span><span class="sxs-lookup"><span data-stu-id="2ef23-109">Windows authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="2ef23-110">[Windows 認証と IIS のインストールの詳細について](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)です。</span><span class="sxs-lookup"><span data-stu-id="2ef23-110">[Learn more about Windows authentication and installing it for IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="2ef23-111">ASP.NET Core アプリケーションの Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="2ef23-111">Enable Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="2ef23-112">Visual Studio Web アプリケーション テンプレートは、Windows 認証をサポートするために構成されていることができます。</span><span class="sxs-lookup"><span data-stu-id="2ef23-112">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="2ef23-113">Windows 認証アプリ テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-113">Use the Windows authentication app template</span></span>

<span data-ttu-id="2ef23-114">Visual Studio で。</span><span class="sxs-lookup"><span data-stu-id="2ef23-114">In Visual Studio:</span></span>
1. <span data-ttu-id="2ef23-115">新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-115">Create a new ASP.NET Core Web Application.</span></span> 
1. <span data-ttu-id="2ef23-116">テンプレートの一覧から Web アプリケーションを選択します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-116">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="2ef23-117">選択、**認証の変更**ボタンをクリックして**Windows 認証**です。</span><span class="sxs-lookup"><span data-stu-id="2ef23-117">Select the **Change Authentication** button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="2ef23-118">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-118">Run the app.</span></span> <span data-ttu-id="2ef23-119">上部にある ユーザー名が表示されるアプリの右。</span><span class="sxs-lookup"><span data-stu-id="2ef23-119">The username appears in the top right of the app.</span></span>

![Windows 認証のブラウザーのスクリーン ショット](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="2ef23-121">IIS Express を使用して開発作業では、テンプレートは、Windows 認証を使用するために必要なすべての構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-121">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="2ef23-122">次のセクションでは、Windows 認証用の ASP.NET Core アプリを手動で構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-122">The following section shows how to manually configure an ASP.NET Core app for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="2ef23-123">Windows および匿名認証用 visual Studio の設定</span><span class="sxs-lookup"><span data-stu-id="2ef23-123">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="2ef23-124">Visual Studio プロジェクト**プロパティ**ページの**デバッグ** タブは、Windows 認証と匿名認証のチェック ボックスを表示します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-124">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Windows 認証のブラウザーのスクリーン ショット](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="2ef23-126">これら 2 つのプロパティを構成する代わりに、 *launchSettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="2ef23-126">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="2ef23-127">IIS での Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="2ef23-127">Enable Windows authentication with IIS</span></span>

<span data-ttu-id="2ef23-128">IIS を使用して、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)ASP.NET Core アプリケーションをホストします。</span><span class="sxs-lookup"><span data-stu-id="2ef23-128">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="2ef23-129">モジュールは、既定では IIS にフローする Windows 認証を許可します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-129">The module allows Windows authentication to flow to IIS by default.</span></span> <span data-ttu-id="2ef23-130">Windows 認証は、アプリケーションではなく、IIS で構成されます。</span><span class="sxs-lookup"><span data-stu-id="2ef23-130">Windows authentication is configured in IIS, not the app.</span></span> <span data-ttu-id="2ef23-131">次のセクションでは、IIS マネージャーを使用して、Windows 認証を使用する ASP.NET Core アプリケーションを構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-131">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication.</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="2ef23-132">新しい IIS サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-132">Create a new IIS site</span></span>

<span data-ttu-id="2ef23-133">名前とフォルダーを指定し、新しいアプリケーション プールを作成することを許可します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-133">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="2ef23-134">認証をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="2ef23-134">Customize authentication</span></span>

<span data-ttu-id="2ef23-135">サイトの [認証] メニューを開きます。</span><span class="sxs-lookup"><span data-stu-id="2ef23-135">Open the Authentication menu for the site.</span></span>

![IIS 認証 メニュー](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="2ef23-137">匿名認証を無効にして、Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="2ef23-137">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![IIS の認証設定](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="2ef23-139">IIS サイトのフォルダーにプロジェクトを発行します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-139">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="2ef23-140">Visual Studio または .NET Core CLI を使用して、インストール先フォルダーをアプリの発行します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-140">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Visual Studio 発行のダイアログ ボックス](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="2ef23-142">詳細については[を IIS に発行](xref:host-and-deploy/iis/index)です。</span><span class="sxs-lookup"><span data-stu-id="2ef23-142">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="2ef23-143">Windows 認証が動作していることを確認するアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-143">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a><span data-ttu-id="2ef23-144">HTTP.sys や WebListener で Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="2ef23-144">Enable Windows authentication with HTTP.sys or WebListener</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2ef23-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2ef23-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="2ef23-146">Kestrel が Windows 認証をサポートしていないは使用できます[HTTP.sys](xref:fundamentals/servers/httpsys) Windows では自己ホスト型のシナリオをサポートするためにします。</span><span class="sxs-lookup"><span data-stu-id="2ef23-146">Although Kestrel doesn't support Windows authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="2ef23-147">次の例は、Windows 認証での HTTP.sys を使用するアプリの web ホストを構成します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-147">The following example configures the app's web host to use HTTP.sys with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2ef23-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2ef23-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="2ef23-149">Kestrel が Windows 認証をサポートしていないは使用できます[WebListener](xref:fundamentals/servers/weblistener) Windows では自己ホスト型のシナリオをサポートするためにします。</span><span class="sxs-lookup"><span data-stu-id="2ef23-149">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="2ef23-150">次の例は、Windows 認証で WebListener を使用するアプリの web ホストを構成します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-150">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a><span data-ttu-id="2ef23-151">Windows 認証を使用します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-151">Work with Windows authentication</span></span>

<span data-ttu-id="2ef23-152">匿名アクセスの構成の状態を決定する方法、`[Authorize]`と`[AllowAnonymous]`属性は、アプリで使用します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-152">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="2ef23-153">次の 2 つのセクションでは、匿名アクセスの許可されていないと許可されている構成の状態を処理する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-153">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="2ef23-154">匿名アクセスを許可しません。</span><span class="sxs-lookup"><span data-stu-id="2ef23-154">Disallow anonymous access</span></span>

<span data-ttu-id="2ef23-155">Windows 認証が有効になっており、匿名アクセスが無効になっているときに、`[Authorize]`と`[AllowAnonymous]`属性が影響を与えるありません。</span><span class="sxs-lookup"><span data-stu-id="2ef23-155">When Windows authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="2ef23-156">IIS サイト (または HTTP.sys または WebListener サーバー) への匿名アクセスを許可しないように構成する場合、要求がアプリに到達しません。</span><span class="sxs-lookup"><span data-stu-id="2ef23-156">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="2ef23-157">このため、`[AllowAnonymous]`属性は適用されません。</span><span class="sxs-lookup"><span data-stu-id="2ef23-157">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="2ef23-158">匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-158">Allow anonymous access</span></span>

<span data-ttu-id="2ef23-159">Windows 認証と匿名アクセスの両方が有効になっているときに使用して、`[Authorize]`と`[AllowAnonymous]`属性。</span><span class="sxs-lookup"><span data-stu-id="2ef23-159">When both Windows authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="2ef23-160">`[Authorize]`属性では、Windows 認証を必要と本当にアプリの部分をセキュリティで保護することができます。</span><span class="sxs-lookup"><span data-stu-id="2ef23-160">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows authentication.</span></span> <span data-ttu-id="2ef23-161">`[AllowAnonymous]`属性のオーバーライド`[Authorize]`属性の匿名アクセスを許可するアプリ内で使用します。</span><span class="sxs-lookup"><span data-stu-id="2ef23-161">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="2ef23-162">参照してください[単純な承認](xref:security/authorization/simple)属性の使用方法の詳細。</span><span class="sxs-lookup"><span data-stu-id="2ef23-162">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="2ef23-163">ASP.NET Core で 2.x、`[Authorize]`属性で追加の構成が必要です。 *Startup.cs* Windows 認証の匿名要求を身にします。</span><span class="sxs-lookup"><span data-stu-id="2ef23-163">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows authentication.</span></span> <span data-ttu-id="2ef23-164">推奨される構成によって若干異なります、web サーバーが使用されています。</span><span class="sxs-lookup"><span data-stu-id="2ef23-164">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="2ef23-165">既定では、空の HTTP 403 応答ページにアクセスするための承認を持たないユーザーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2ef23-165">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="2ef23-166">[StatusCodePages ミドルウェア](xref:fundamentals/error-handling#configuring-status-code-pages)ユーザー エクスペリエンスを向上させる「アクセス拒否」に提供するように構成できます。</span><span class="sxs-lookup"><span data-stu-id="2ef23-166">The [StatusCodePages middleware](xref:fundamentals/error-handling#configuring-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="2ef23-167">IIS</span><span class="sxs-lookup"><span data-stu-id="2ef23-167">IIS</span></span>

<span data-ttu-id="2ef23-168">IIS を使用する場合に、次を追加、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="2ef23-168">If using IIS, add the following to the `ConfigureServices` method:</span></span> 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="2ef23-169">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="2ef23-169">HTTP.sys</span></span>

<span data-ttu-id="2ef23-170">HTTP.sys を使用する場合に、次の追加、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="2ef23-170">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="2ef23-171">偽装</span><span class="sxs-lookup"><span data-stu-id="2ef23-171">Impersonation</span></span>

<span data-ttu-id="2ef23-172">ASP.NET Core は、権限借用を実装していません。</span><span class="sxs-lookup"><span data-stu-id="2ef23-172">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="2ef23-173">アプリは、アプリケーション プールまたはプロセス id を使用して、すべての要求のアプリケーション id で実行されます。</span><span class="sxs-lookup"><span data-stu-id="2ef23-173">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="2ef23-174">ユーザーの代理としてアクションを明示的に実行する必要がある場合を使用して`WindowsIdentity.RunImpersonated`です。</span><span class="sxs-lookup"><span data-stu-id="2ef23-174">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="2ef23-175">このコンテキストで 1 つのアクションを実行し、コンテキストを閉じます。</span><span class="sxs-lookup"><span data-stu-id="2ef23-175">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="2ef23-176">なお`RunImpersonated`非同期操作をサポートしないし、複雑なシナリオで使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="2ef23-176">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="2ef23-177">全体の要求またはミドルウェア チェーンをラッピングされていないはサポートされてなど、ことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="2ef23-177">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

---
