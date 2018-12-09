---
title: ASP.NET Core での Windows 認証を構成します。
author: scottaddie
description: ASP.NET core で IIS Express、IIS、HTTP.sys は、および WebListener を使用して Windows 認証を構成する方法について説明します。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/01/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: 342759a6ff4b5679e0d54c979188ae66d339562d
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121298"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="b90cf-103">ASP.NET Core での Windows 認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="b90cf-104">作成者: [Steve Smith](https://ardalis.com)、[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="b90cf-104">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="b90cf-105">Windows 認証は、IIS でホストされている ASP.NET Core アプリ用に構成できます[HTTP.sys](xref:fundamentals/servers/httpsys)、または[WebListener](xref:fundamentals/servers/weblistener)します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-105">Windows Authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="windows-authentication"></a><span data-ttu-id="b90cf-106">Windows 認証</span><span class="sxs-lookup"><span data-stu-id="b90cf-106">Windows Authentication</span></span>

<span data-ttu-id="b90cf-107">Windows 認証は、ASP.NET Core アプリのユーザーを認証するオペレーティング システムに依存します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="b90cf-108">ユーザーを識別するために Active Directory ドメインの id またはその他の Windows アカウントを使用して、企業ネットワークで、サーバーの実行時に、Windows 認証を使用できます。</span><span class="sxs-lookup"><span data-stu-id="b90cf-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="b90cf-109">Windows 認証は、同じ Windows ドメインにユーザー、クライアント アプリケーション、および web サーバーが属している、イントラネット環境に最適です。</span><span class="sxs-lookup"><span data-stu-id="b90cf-109">Windows Authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="b90cf-110">[Windows 認証の詳細について説明し、IIS のインストール、](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-110">[Learn more about Windows Authentication and installing it for IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="b90cf-111">ASP.NET Core アプリで Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="b90cf-111">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="b90cf-112">Visual Studio Web アプリケーション テンプレートは、Windows 認証をサポートするために構成できます。</span><span class="sxs-lookup"><span data-stu-id="b90cf-112">The Visual Studio Web Application template can be configured to support Windows Authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="b90cf-113">Windows 認証アプリ テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-113">Use the Windows Authentication app template</span></span>

<span data-ttu-id="b90cf-114">Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="b90cf-114">In Visual Studio:</span></span>

1. <span data-ttu-id="b90cf-115">新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-115">Create a new ASP.NET Core Web Application.</span></span>
1. <span data-ttu-id="b90cf-116">テンプレートの一覧から Web アプリケーションを選択します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-116">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="b90cf-117">選択、**認証の変更**ボタンをクリックし、選択**Windows 認証**します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-117">Select the **Change Authentication** button and select **Windows Authentication**.</span></span>

<span data-ttu-id="b90cf-118">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-118">Run the app.</span></span> <span data-ttu-id="b90cf-119">上部にある ユーザー名が表示されます、アプリの右。</span><span class="sxs-lookup"><span data-stu-id="b90cf-119">The username appears in the top right of the app.</span></span>

![Windows 認証のブラウザーのスクリーン ショット](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="b90cf-121">IIS Express を使用して開発作業では、テンプレートは、Windows 認証を使用するために必要なすべての構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-121">For development work using IIS Express, the template provides all the configuration necessary to use Windows Authentication.</span></span> <span data-ttu-id="b90cf-122">次のセクションでは、Windows 認証用の ASP.NET Core アプリを手動で構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-122">The following section shows how to manually configure an ASP.NET Core app for Windows Authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="b90cf-123">Windows と匿名認証用 visual Studio の設定</span><span class="sxs-lookup"><span data-stu-id="b90cf-123">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="b90cf-124">Visual Studio プロジェクト**プロパティ**ページの**デバッグ** タブでは、Windows 認証と匿名認証 チェック ボックスが用意されています。</span><span class="sxs-lookup"><span data-stu-id="b90cf-124">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows Authentication and anonymous authentication.</span></span>

![強調表示されている認証オプションを使用して、Windows 認証ブラウザーのスクリーン ショット](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="b90cf-126">これら 2 つのプロパティを構成する代わりに、 *launchSettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b90cf-126">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="b90cf-127">IIS での Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="b90cf-127">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="b90cf-128">IIS を使用して、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)ASP.NET Core アプリをホストします。</span><span class="sxs-lookup"><span data-stu-id="b90cf-128">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="b90cf-129">Windows 認証は、アプリケーションではなく、IIS で構成されます。</span><span class="sxs-lookup"><span data-stu-id="b90cf-129">Windows Authentication is configured in IIS, not the app.</span></span> <span data-ttu-id="b90cf-130">次のセクションでは、IIS マネージャーを使用して、Windows 認証を使用する ASP.NET Core アプリを構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-130">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows Authentication.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="b90cf-131">IIS 構成</span><span class="sxs-lookup"><span data-stu-id="b90cf-131">IIS configuration</span></span>

<span data-ttu-id="b90cf-132">Windows 認証の IIS の役割サービスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="b90cf-132">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="b90cf-133">詳細については、次を参照してください。 [(手順 2 参照)、IIS の役割サービスで Windows 認証を有効にする](xref:host-and-deploy/iis/index#iis-configuration)します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-133">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="b90cf-134">IIS 統合ミドルウェアは、既定で自動的に要求の認証に構成されます。</span><span class="sxs-lookup"><span data-stu-id="b90cf-134">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="b90cf-135">詳細については、次を参照してください。 [ASP.NET Core の IIS と Windows ホスト: IIS のオプション (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options)します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-135">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="b90cf-136">ASP.NET Core モジュールは、既定では、アプリに Windows 認証トークンを転送するように構成されます。</span><span class="sxs-lookup"><span data-stu-id="b90cf-136">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="b90cf-137">詳細については、次を参照してください。 [ASP.NET Core モジュール構成リファレンス: aspNetCore 要素の属性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-137">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="b90cf-138">新しい IIS サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-138">Create a new IIS site</span></span>

<span data-ttu-id="b90cf-139">名前とフォルダーを指定し、新しいアプリケーション プールを作成することを許可します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-139">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="b90cf-140">認証をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="b90cf-140">Customize authentication</span></span>

<span data-ttu-id="b90cf-141">サイトの認証機能を開きます。</span><span class="sxs-lookup"><span data-stu-id="b90cf-141">Open the Authentication features for the site.</span></span>

![IIS 認証 メニュー](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="b90cf-143">匿名認証を無効にして、Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="b90cf-143">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![IIS 認証の設定](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="b90cf-145">IIS サイトのフォルダーにプロジェクトを発行します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-145">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="b90cf-146">目的のフォルダーにアプリを発行する Visual Studio または .NET Core CLI を使用して。</span><span class="sxs-lookup"><span data-stu-id="b90cf-146">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Visual Studio の発行 ダイアログ](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="b90cf-148">詳細については[IIS への発行](xref:host-and-deploy/iis/index)します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-148">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="b90cf-149">Windows 認証が動作していることを確認するアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-149">Launch the app to verify Windows Authentication is working.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="b90cf-150">HTTP.sys を使用して Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="b90cf-150">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="b90cf-151">使用することができますが、Kestrel が Windows 認証をサポートしていない[HTTP.sys](xref:fundamentals/servers/httpsys) Windows で自己ホスト型のシナリオをサポートします。</span><span class="sxs-lookup"><span data-stu-id="b90cf-151">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="b90cf-152">次の例は、Windows 認証を使用した HTTP.sys を使用するアプリの web ホストを構成します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-152">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="b90cf-153">HTTP.sys では、Kerberos 認証プロトコルを使用したカーネル モード認証に処理が委任されます。</span><span class="sxs-lookup"><span data-stu-id="b90cf-153">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="b90cf-154">Kerberos および HTTP.sys ではユーザー モード認証がサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="b90cf-154">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="b90cf-155">Active Directory から取得され、クライアントによって、ユーザーを認証するサーバーに転送される Kerberos トークン/チケットを暗号化解除するには、コンピューター アカウントを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b90cf-155">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="b90cf-156">アプリのユーザーではなく、ホストのサービス プリンシパル名 (SPN) を登録します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-156">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="b90cf-157">HTTP.sys は、Nano Server バージョン 1709 以降でサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="b90cf-157">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="b90cf-158">Nano Server の Windows 認証と HTTP.sys を使用する、 [(microsoft/windowsservercore) の Server Core コンテナー](https://hub.docker.com/r/microsoft/windowsservercore/)します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-158">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="b90cf-159">Server Core の詳細については、次を参照してください。 [Windows Server の Server Core インストール オプションとは何ですか?](/windows-server/administration/server-core/what-is-server-core)します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-159">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

## <a name="enable-windows-authentication-with-weblistener"></a><span data-ttu-id="b90cf-160">WebListener での Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="b90cf-160">Enable Windows Authentication with WebListener</span></span>

<span data-ttu-id="b90cf-161">使用することができますが、Kestrel が Windows 認証をサポートしていない[WebListener](xref:fundamentals/servers/weblistener) Windows で自己ホスト型のシナリオをサポートします。</span><span class="sxs-lookup"><span data-stu-id="b90cf-161">Although Kestrel doesn't support Windows Authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="b90cf-162">次の例は、Windows 認証で WebListener を使用するアプリの web ホストを構成します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-162">The following example configures the app's web host to use WebListener with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

> [!NOTE]
> <span data-ttu-id="b90cf-163">WebListener では、Kerberos 認証プロトコルを使用したカーネル モード認証に処理が委任されます。</span><span class="sxs-lookup"><span data-stu-id="b90cf-163">WebListener delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="b90cf-164">Kerberos および WebListener ではユーザー モード認証がサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="b90cf-164">User mode authentication isn't supported with Kerberos and WebListener.</span></span> <span data-ttu-id="b90cf-165">Active Directory から取得され、クライアントによって、ユーザーを認証するサーバーに転送される Kerberos トークン/チケットを暗号化解除するには、コンピューター アカウントを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b90cf-165">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="b90cf-166">アプリのユーザーではなく、ホストのサービス プリンシパル名 (SPN) を登録します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-166">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

::: moniker-end

## <a name="work-with-windows-authentication"></a><span data-ttu-id="b90cf-167">Windows 認証を使用します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-167">Work with Windows Authentication</span></span>

<span data-ttu-id="b90cf-168">匿名アクセスの構成の状態にする方法が決定します、`[Authorize]`と`[AllowAnonymous]`属性は、アプリで使用します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-168">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="b90cf-169">次の 2 つのセクションでは、匿名アクセスの許可されていないと、許可されている構成の状態を処理する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-169">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="b90cf-170">匿名アクセスを禁止します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-170">Disallow anonymous access</span></span>

<span data-ttu-id="b90cf-171">Windows 認証が有効になっており、匿名アクセスが無効になっているときに、`[Authorize]`と`[AllowAnonymous]`属性は影響ありません。</span><span class="sxs-lookup"><span data-stu-id="b90cf-171">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="b90cf-172">IIS サイト (または HTTP.sys または WebListener サーバー) を匿名アクセスを許可しないように構成する場合、要求がアプリに到達しません。</span><span class="sxs-lookup"><span data-stu-id="b90cf-172">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="b90cf-173">このため、`[AllowAnonymous]`属性には適用されません。</span><span class="sxs-lookup"><span data-stu-id="b90cf-173">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="b90cf-174">匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-174">Allow anonymous access</span></span>

<span data-ttu-id="b90cf-175">Windows 認証と匿名アクセスの両方が有効になっているときに使用して、`[Authorize]`と`[AllowAnonymous]`属性。</span><span class="sxs-lookup"><span data-stu-id="b90cf-175">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="b90cf-176">`[Authorize]`属性では、本当に Windows 認証を必要と、アプリの情報を保護できます。</span><span class="sxs-lookup"><span data-stu-id="b90cf-176">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="b90cf-177">`[AllowAnonymous]`属性のオーバーライド`[Authorize]`属性の匿名アクセスを許可するアプリ内で使用します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-177">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="b90cf-178">参照してください[単純な承認](xref:security/authorization/simple)属性の使用方法の詳細。</span><span class="sxs-lookup"><span data-stu-id="b90cf-178">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="b90cf-179">ASP.NET core 2.x、`[Authorize]`属性に追加の構成が必要です*Startup.cs*匿名要求を Windows 認証チャレンジを。</span><span class="sxs-lookup"><span data-stu-id="b90cf-179">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="b90cf-180">推奨される構成によって若干異なります、web サーバーが使用されています。</span><span class="sxs-lookup"><span data-stu-id="b90cf-180">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="b90cf-181">既定では、ページにアクセスするための承認を持たないユーザーには、空の HTTP 403 応答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b90cf-181">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="b90cf-182">[StatusCodePages ミドルウェア](xref:fundamentals/error-handling#configure-status-code-pages)「アクセスが拒否されました」のより優れたエクスペリエンスをユーザーに提供するように構成できます。</span><span class="sxs-lookup"><span data-stu-id="b90cf-182">The [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="b90cf-183">IIS</span><span class="sxs-lookup"><span data-stu-id="b90cf-183">IIS</span></span>

<span data-ttu-id="b90cf-184">IIS を使用する場合に、次を追加、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="b90cf-184">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="b90cf-185">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="b90cf-185">HTTP.sys</span></span>

<span data-ttu-id="b90cf-186">HTTP.sys を使用する場合に、次を追加、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="b90cf-186">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="b90cf-187">偽装</span><span class="sxs-lookup"><span data-stu-id="b90cf-187">Impersonation</span></span>

<span data-ttu-id="b90cf-188">ASP.NET Core では、権限借用を実装しません。</span><span class="sxs-lookup"><span data-stu-id="b90cf-188">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="b90cf-189">アプリのアプリ プールまたはプロセス id を使用して、すべての要求 id をアプリケーションで実行されます。</span><span class="sxs-lookup"><span data-stu-id="b90cf-189">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="b90cf-190">明示的に、ユーザーに代わって操作を実行する必要がある場合を使用して、`WindowsIdentity.RunImpersonated`します。</span><span class="sxs-lookup"><span data-stu-id="b90cf-190">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="b90cf-191">このコンテキストで 1 つのアクションを実行し、コンテキストを閉じます。</span><span class="sxs-lookup"><span data-stu-id="b90cf-191">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="b90cf-192">なお`RunImpersonated`非同期操作をサポートしていないし、複雑なシナリオでは使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="b90cf-192">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="b90cf-193">全体要求またはミドルウェアのチェーンをラッピングされていないサポートなど、お勧めします。</span><span class="sxs-lookup"><span data-stu-id="b90cf-193">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>
