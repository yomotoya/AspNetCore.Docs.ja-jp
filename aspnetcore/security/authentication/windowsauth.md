---
title: "ASP.NET Core で Windows 認証を構成します。"
author: ardalis
description: "ASP.NET Core で Windows 認証を構成する方法"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 7/5/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: 008a647295334e957c33c6db7f80687645b3b928
ms.sourcegitcommit: 69b3255f8b6f5db9e7d21f391420602d7ba9f4db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2017
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="24a20-104">ASP.NET Core で Windows 認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="24a20-104">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="24a20-105">によって[Steve Smith](https://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="24a20-105">By [Steve Smith](https://ardalis.com)</span></span>

<span data-ttu-id="24a20-106">IIS または WebListener でホストされている ASP.NET Core アプリケーションの Windows 認証を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="24a20-106">Windows authentication can be configured for ASP.NET Core apps hosted with IIS or WebListener.</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="24a20-107">Windows 認証とは</span><span class="sxs-lookup"><span data-stu-id="24a20-107">What is Windows authentication</span></span>

<span data-ttu-id="24a20-108">Windows 認証は、ASP.NET Core アプリケーションのユーザーを認証するオペレーティング システムに依存します。</span><span class="sxs-lookup"><span data-stu-id="24a20-108">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="24a20-109">ユーザーを識別する Active Directory ドメインの id またはその他の Windows アカウントを使用して、企業ネットワークで実行されると、サーバーは、Windows 認証を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="24a20-109">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="24a20-110">Windows 認証は、ユーザー、クライアント アプリケーションおよび web サーバーを同じ Windows ドメインに属している、イントラネット環境に適した認証最善のセキュリティで保護された形式です。</span><span class="sxs-lookup"><span data-stu-id="24a20-110">Windows authentication is a secure form of authentication best suited to intranet environments where users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="24a20-111">[詳細については、Windows 認証と IIS のインストール](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)です。</span><span class="sxs-lookup"><span data-stu-id="24a20-111">[Learn more about Windows Authentication and installing it for IIS](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

## <a name="enabling-windows-authentication-in-an-aspnet-core-application"></a><span data-ttu-id="24a20-112">ASP.NET Core アプリケーションで Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="24a20-112">Enabling Windows authentication in an ASP.NET Core application</span></span>

<span data-ttu-id="24a20-113">Visual Studio Web アプリケーション テンプレートは、Windows 認証をサポートするために構成されていることができます。</span><span class="sxs-lookup"><span data-stu-id="24a20-113">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="using-the-windows-authentication-app-template"></a><span data-ttu-id="24a20-114">Windows 認証アプリ テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="24a20-114">Using the Windows authentication app template</span></span>

<span data-ttu-id="24a20-115">Visual Studio で。</span><span class="sxs-lookup"><span data-stu-id="24a20-115">In Visual Studio:</span></span>
* <span data-ttu-id="24a20-116">新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="24a20-116">Create a new ASP.NET Core Web Application.</span></span> 
* <span data-ttu-id="24a20-117">テンプレートの一覧から Web アプリケーションを選択します。</span><span class="sxs-lookup"><span data-stu-id="24a20-117">Select Web Application from the list of templates.</span></span>
* <span data-ttu-id="24a20-118">認証の変更 ボタンを選択し、選択**Windows 認証**です。</span><span class="sxs-lookup"><span data-stu-id="24a20-118">Select the Change Authentication button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="24a20-119">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="24a20-119">Run the app.</span></span> <span data-ttu-id="24a20-120">上部にある ユーザー名が表示されるアプリの右。</span><span class="sxs-lookup"><span data-stu-id="24a20-120">The username appears in the top right of the app.</span></span>

![Windows 認証のブラウザーのスクリーン ショット](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="24a20-122">IIS Express を使用して開発作業では、テンプレートは、Windows 認証を使用するために必要なすべての構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="24a20-122">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="24a20-123">次のセクションでは、Windows 認証を手動で ASP.NET Core アプリケーションを構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="24a20-123">The next section shows how to configure an ASP.NET Core app manually for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="24a20-124">Windows および匿名認証用 visual Studio の設定</span><span class="sxs-lookup"><span data-stu-id="24a20-124">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="24a20-125">Visual Studio のプロパティ ページで、デバッグ タブは、Windows 認証と匿名認証のチェック ボックスを提供します。</span><span class="sxs-lookup"><span data-stu-id="24a20-125">The Visual Studio properties page, debug tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Windows 認証のブラウザーのスクリーン ショット](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="24a20-127">これらのプロパティを構成することも、`launchSettings.json`ファイル。</span><span class="sxs-lookup"><span data-stu-id="24a20-127">You can also configure these properties in the `launchSettings.json` file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": true,
    "anonymousAuthentication": false,
    "iisExpress": {
      "applicationUrl": "http://localhost:52171/",
      "sslPort": 0
    }
  } // additional options trimmed
}
```

## <a name="enabling-windows-authentication-with-iis"></a><span data-ttu-id="24a20-128">IIS での Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="24a20-128">Enabling Windows Authentication with IIS</span></span>

<span data-ttu-id="24a20-129">IIS を使用して、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)ASP.NET Core アプリケーションをホストするには、(ANCM)。</span><span class="sxs-lookup"><span data-stu-id="24a20-129">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) (ANCM) to host ASP.NET Core apps.</span></span> <span data-ttu-id="24a20-130">ANCM フロー Windows 認証を IIS に既定でします。</span><span class="sxs-lookup"><span data-stu-id="24a20-130">The ANCM flows Windows authentication to IIS by default.</span></span> <span data-ttu-id="24a20-131">Windows 認証の構成については、アプリケーション プロジェクトではなく、IIS 内で実行します。</span><span class="sxs-lookup"><span data-stu-id="24a20-131">Configuration of Windows authentication is done within IIS, not the application project.</span></span> <span data-ttu-id="24a20-132">次のセクションでは、IIS マネージャーを使用して、Windows 認証を使用する ASP.NET Core アプリケーションを構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="24a20-132">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication:</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="24a20-133">新しい IIS サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="24a20-133">Create a new IIS site</span></span>

<span data-ttu-id="24a20-134">名前とフォルダーを指定し、新しいアプリケーション プールを作成することを許可します。</span><span class="sxs-lookup"><span data-stu-id="24a20-134">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="24a20-135">認証をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="24a20-135">Customize Authentication</span></span>

<span data-ttu-id="24a20-136">サイトの [認証] メニューを開きます。</span><span class="sxs-lookup"><span data-stu-id="24a20-136">Open the Authentication menu for the site.</span></span>

![IIS 認証 メニュー](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="24a20-138">匿名認証を無効にして、Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="24a20-138">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![IIS の認証設定](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="24a20-140">IIS サイトのフォルダーにプロジェクトを発行します。</span><span class="sxs-lookup"><span data-stu-id="24a20-140">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="24a20-141">Visual Studio または .NET Core CLI を使用して*発行*アプリ インストール先フォルダーをします。</span><span class="sxs-lookup"><span data-stu-id="24a20-141">Using Visual Studio or the .NET Core CLI, *publish* your app to the destination folder.</span></span>

![Visual Studio 発行のダイアログ ボックス](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="24a20-143">詳細については[を IIS に発行](https://docs.microsoft.com/aspnet/core/publishing/iis)です。</span><span class="sxs-lookup"><span data-stu-id="24a20-143">Learn more about [publishing to IIS](https://docs.microsoft.com/aspnet/core/publishing/iis).</span></span>

<span data-ttu-id="24a20-144">Windows 認証が動作していることを確認するアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="24a20-144">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enabling-windows-authentication-with-weblistener"></a><span data-ttu-id="24a20-145">WebListener で Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="24a20-145">Enabling Windows authentication with WebListener</span></span>

<span data-ttu-id="24a20-146">Kestrel が Windows 認証をサポートしていないは使用できます[WebListener](xref:fundamentals/servers/weblistener) Windows では自己ホスト型のシナリオをサポートするためにします。</span><span class="sxs-lookup"><span data-stu-id="24a20-146">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="24a20-147">次の例は、Windows 認証で WebListener を使用するアプリの web ホストを構成します。</span><span class="sxs-lookup"><span data-stu-id="24a20-147">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var host = new WebHostBuilder()
            .UseWebListener(options =>
            {
                options.ListenerSettings.Authentication.Schemes = 
                    AuthenticationSchemes.Negotiate | AuthenticationSchemes.NTLM;
                options.ListenerSettings.Authentication.AllowAnonymous = false;
            })
            .UseContentRoot(Directory.GetCurrentDirectory())
            .UseStartup<Startup>()
            .Build();

        host.Run();
    }
}
```

## <a name="working-with-windows-authentication"></a><span data-ttu-id="24a20-148">Windows 認証の使用</span><span class="sxs-lookup"><span data-stu-id="24a20-148">Working with Windows authentication</span></span>

<span data-ttu-id="24a20-149">使用することができます、アプリが Windows 認証と匿名アクセスを使用する場合、``[Authorize]``と``[AllowAnonymous]``属性。</span><span class="sxs-lookup"><span data-stu-id="24a20-149">If your app uses Windows authentication and anonymous access, you can use the ``[Authorize]`` and ``[AllowAnonymous]`` attributes.</span></span> <span data-ttu-id="24a20-150">アプリを必要としない匿名の有効な操作を持たない``[Authorize]``以外の場合は、アプリは認証を要求として扱われます、匿名の要求は拒否されます。</span><span class="sxs-lookup"><span data-stu-id="24a20-150">Apps that do not have anonymous enabled do not require ``[Authorize]``; the  app is treated as requiring authentication, anonymous requests are rejected.</span></span> <span data-ttu-id="24a20-151">IIS サイトが構成されている場合に、注意してください**いない**匿名アクセスを許可する、``[AllowAnonymous]``属性は**いない**匿名の要求を許可します。</span><span class="sxs-lookup"><span data-stu-id="24a20-151">Note, if the IIS site is configured **not** to allow anonymous access, the ``[AllowAnonymous]`` attribute does **not** allow anonymous requests.</span></span> <span data-ttu-id="24a20-152">``[AllowAnonymous]``属性のオーバーライド``[Authorize]``属性の匿名アクセスを許可するアプリ内で使用します。</span><span class="sxs-lookup"><span data-stu-id="24a20-152">The ``[AllowAnonymous]`` attribute overrides ``[Authorize]`` attribute usage within apps that allow anonymous access.</span></span>

### <a name="impersonation"></a><span data-ttu-id="24a20-153">偽装</span><span class="sxs-lookup"><span data-stu-id="24a20-153">Impersonation</span></span>

<span data-ttu-id="24a20-154">ASP.NET Core は、権限借用を実装していません。</span><span class="sxs-lookup"><span data-stu-id="24a20-154">ASP.NET Core does not implement impersonation.</span></span> <span data-ttu-id="24a20-155">アプリは、アプリケーション プールまたはプロセス id を使用して、すべての要求のアプリケーション id で実行されます。</span><span class="sxs-lookup"><span data-stu-id="24a20-155">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="24a20-156">ユーザーの代理としてアクションを明示的に実行する必要がある場合を使用して``WindowsIdentity.RunImpersonated``です。</span><span class="sxs-lookup"><span data-stu-id="24a20-156">If you need to explicitly perform an action on behalf of a user, use ``WindowsIdentity.RunImpersonated``.</span></span> <span data-ttu-id="24a20-157">このコンテキストで 1 つのアクションを実行し、コンテキストを閉じます。</span><span class="sxs-lookup"><span data-stu-id="24a20-157">Run a single action in this context and then close the context.</span></span> <span data-ttu-id="24a20-158">なお``RunImpersonated``非同期をサポートしておらず、複雑なシナリオでは使用できません。</span><span class="sxs-lookup"><span data-stu-id="24a20-158">Note that ``RunImpersonated`` does not support async and should not be used for complex scenarios.</span></span> <span data-ttu-id="24a20-159">たとえば、全体の要求またはミドルウェア チェーンをラッピングは not サポートまたはことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="24a20-159">For example, wrapping entire requests or middleware chains is not supported or recommended.</span></span>
