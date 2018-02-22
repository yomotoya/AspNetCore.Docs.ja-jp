---
title: "IIS を使用した Windows での ASP.NET Core のホスト"
author: guardrex
description: "Windows Server インターネット インフォメーション サービス (IIS) での ASP.NET Core アプリをホストする方法を説明します。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/index
ms.openlocfilehash: 620bfefa625f4b39cb2731b4f553caaa4526c71b
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="60617-103">IIS を使用した Windows での ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="60617-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="60617-104">執筆者: [Luke Latham](https://github.com/guardrex)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="60617-104">By [Luke Latham](https://github.com/guardrex) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="60617-105">サポートされるオペレーティング システム</span><span class="sxs-lookup"><span data-stu-id="60617-105">Supported operating systems</span></span>

<span data-ttu-id="60617-106">次のオペレーティング システムがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="60617-106">The following operating systems are supported:</span></span>

* <span data-ttu-id="60617-107">Windows 7 以降</span><span class="sxs-lookup"><span data-stu-id="60617-107">Windows 7 or later</span></span>
* <span data-ttu-id="60617-108">Windows Server 2008 R2 以降&#8224;</span><span class="sxs-lookup"><span data-stu-id="60617-108">Windows Server 2008 R2 or later&#8224;</span></span>

<span data-ttu-id="60617-109">&#8224;概念的には、このドキュメントで説明する IIS 構成は、Nano Server IIS で ASP.NET Core アプリをホストする場合にも当てはまります。</span><span class="sxs-lookup"><span data-stu-id="60617-109">&#8224;Conceptually, the IIS configuration described in this document also applies to hosting ASP.NET Core apps on Nano Server IIS.</span></span> <span data-ttu-id="60617-110">Nano Server に固有の手順については、「[Nano Server の ASP.NET Core と IIS](xref:tutorials/nano-server)」チュートリアルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-110">For instructions specific to Nano Server, see the [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) tutorial.</span></span>

<span data-ttu-id="60617-111">[HTTP.sys サーバー](xref:fundamentals/servers/httpsys) (以前の名称は [WebListener](xref:fundamentals/servers/weblistener)) が、IIS を含むリバース プロキシ構成で動作しません。</span><span class="sxs-lookup"><span data-stu-id="60617-111">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="60617-112">[Kestrel サーバー](xref:fundamentals/servers/kestrel)を使用します。</span><span class="sxs-lookup"><span data-stu-id="60617-112">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

## <a name="application-configuration"></a><span data-ttu-id="60617-113">アプリケーション構成</span><span class="sxs-lookup"><span data-stu-id="60617-113">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="60617-114">IISIntegration コンポーネントを有効にする</span><span class="sxs-lookup"><span data-stu-id="60617-114">Enable the IISIntegration components</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="60617-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="60617-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="60617-116">一般的な *Program.cs* は [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を呼び出してホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="60617-116">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to begin setting up a host.</span></span> <span data-ttu-id="60617-117">`CreateDefaultBuilder` は [Kestrel](xref:fundamentals/servers/kestrel) を Web サーバーとして構成し、ベース パスとポートを [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)に構成することで、IIS 統合を有効にします。</span><span class="sxs-lookup"><span data-stu-id="60617-117">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="60617-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="60617-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="60617-119">アプリの依存関係に [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) パッケージへの依存関係を含めます。</span><span class="sxs-lookup"><span data-stu-id="60617-119">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="60617-120">[UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) 拡張メソッドを [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) に追加して、IIS 統合ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="60617-120">Use IIS Integration middleware by adding the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) extension method to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="60617-121">[UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) と [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) の両方が必要です。</span><span class="sxs-lookup"><span data-stu-id="60617-121">Both [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) and [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) are required.</span></span> <span data-ttu-id="60617-122">`UseIISIntegration` を呼び出すコードはコードの移植性に影響しません。</span><span class="sxs-lookup"><span data-stu-id="60617-122">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="60617-123">アプリが IIS の背後で実行されていない場合 (たとえば、アプリが Kestrel で直接実行されている場合)、`UseIISIntegration` は機能しません。</span><span class="sxs-lookup"><span data-stu-id="60617-123">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` doesn't operate.</span></span>

---

<span data-ttu-id="60617-124">ホスティングの詳細については、「[Hosting in ASP.NET Core](xref:fundamentals/hosting)」(ASP.NET Core でのホスティング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-124">For more information on hosting, see [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span></span>

### <a name="iis-options"></a><span data-ttu-id="60617-125">IIS のオプション</span><span class="sxs-lookup"><span data-stu-id="60617-125">IIS options</span></span>

<span data-ttu-id="60617-126">[IIS](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) のオプションを構成するには、IISOptions のサービス構成を [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices) に含めます。</span><span class="sxs-lookup"><span data-stu-id="60617-126">To configure IIS options, include a service configuration for [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="60617-127">次の例では、`HttpContext.Connection.ClientCertificate` を入力するためのクライアント証明書のアプリへの転送は無効になります。</span><span class="sxs-lookup"><span data-stu-id="60617-127">In the following example, forwarding client certificates to the app to populate `HttpContext.Connection.ClientCertificate` is disabled:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="60617-128">オプション</span><span class="sxs-lookup"><span data-stu-id="60617-128">Option</span></span>                         | <span data-ttu-id="60617-129">既定値</span><span class="sxs-lookup"><span data-stu-id="60617-129">Default</span></span> | <span data-ttu-id="60617-130">設定</span><span class="sxs-lookup"><span data-stu-id="60617-130">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="60617-131">`true` の場合、IIS 統合ミドルウェアが、[Windows 認証](xref:security/authentication/windowsauth)によって `HttpContext.User` を設定します。</span><span class="sxs-lookup"><span data-stu-id="60617-131">If `true`, IIS Integration Middleware sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="60617-132">`false` の場合、ミドルウェアは `HttpContext.User` の ID を提供するだけで、`AuthenticationScheme` によって明示的に要求されたときに課題に応答します。</span><span class="sxs-lookup"><span data-stu-id="60617-132">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="60617-133">`AutomaticAuthentication` を機能させるためには、IIS で Windows 認証を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="60617-133">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="60617-134">詳細については、「[Windows 認証](xref:security/authentication/windowsauth)」のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-134">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="60617-135">ログイン ページでユーザーに表示名が表示されるように設定します。</span><span class="sxs-lookup"><span data-stu-id="60617-135">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="60617-136">`true` の場合、`MS-ASPNETCORE-CLIENTCERT` 要求ヘッダーが指定されていると、`HttpContext.Connection.ClientCertificate` が設定されます。</span><span class="sxs-lookup"><span data-stu-id="60617-136">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="webconfig-file"></a><span data-ttu-id="60617-137">web.config ファイル</span><span class="sxs-lookup"><span data-stu-id="60617-137">web.config file</span></span>

<span data-ttu-id="60617-138">*web.config* ファイルでは、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)を設定します。</span><span class="sxs-lookup"><span data-stu-id="60617-138">The *web.config* file configures the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="60617-139">*web.config* の作成、変換、発行は .NET Core Web SDK (`Microsoft.NET.Sdk.Web`) で処理されます。</span><span class="sxs-lookup"><span data-stu-id="60617-139">Creating, transforming, and publishing *web.config* is handled by the .NET Core Web SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="60617-140">SDK は、プロジェクト ファイルの先頭で設定されています。</span><span class="sxs-lookup"><span data-stu-id="60617-140">The SDK is set  at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="60617-141">プロジェクトに *web.config* ファイルが含まれていない場合、そのファイルは [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)を構成するための正しい *processPath* と *arguments* を使用して作成され、[発行された出力](xref:host-and-deploy/directory-structure)に移行されます。</span><span class="sxs-lookup"><span data-stu-id="60617-141">If a *web.config* file isn't present the project, the file is created with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="60617-142">プロジェクトに *web.config* ファイルが含まれていない場合、そのファイルは ASP.NET Core モジュールを構成するための正しい *processPath* と *arguments* を使用して作成され、発行された出力に移行されます。</span><span class="sxs-lookup"><span data-stu-id="60617-142">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="60617-143">変換によりファイル内の IIS 構成の設定が変わることはありません。</span><span class="sxs-lookup"><span data-stu-id="60617-143">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="60617-144">*web.config* ファイルは、アクティブな IIS モジュールを制御する追加の IIS 構成設定を提供する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="60617-144">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="60617-145">ASP.NET Core アプリを使用して要求を処理できる IIS モジュールの詳細については、「[IIS のモジュールの使用](xref:host-and-deploy/iis/modules)」トピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-145">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [Using IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="60617-146">Web SDK によって *web.config* ファイルが変換されないようにするため、**\<IsTransformWebConfigDisabled>** プロパティをプロジェクト ファイルで使用します。</span><span class="sxs-lookup"><span data-stu-id="60617-146">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="60617-147">Web SDK ファイルの変換を無効にすると、 *processPath*と*引数*開発者によって手動で設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60617-147">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="60617-148">詳細については、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-148">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="60617-149">web.config ファイルの場所</span><span class="sxs-lookup"><span data-stu-id="60617-149">web.config file location</span></span>

<span data-ttu-id="60617-150">ASP.NET Core アプリは、IIS と Kestrel サーバー間のリバース プロキシでホストされます。</span><span class="sxs-lookup"><span data-stu-id="60617-150">ASP.NET Core apps are hosted in a reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="60617-151">リバース プロキシを作成するためには、展開されるアプリのコンテンツ ルート パス (通常は、アプリ ベース パス) に *web.config* ファイルが存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60617-151">In order to create the reverse proxy, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="60617-152">これは、IIS に提供される web サイトの物理パスと同じ場所です。</span><span class="sxs-lookup"><span data-stu-id="60617-152">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="60617-153">*Web.config* ファイルは、Web 配置を使用して複数のアプリの発行を有効にするため、アプリのルートで必要です。</span><span class="sxs-lookup"><span data-stu-id="60617-153">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="60617-154">アプリの物理パスには、*\<assembly>.runtimeconfig.json*、*\<assembly>.xml* (XML ドキュメントのコメント)、*\<assembly>.deps.json* などの機密性の高いファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="60617-154">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="60617-155">*web.config* ファイルが存在し、サイトは通常どおり起動した場合、IIS は、これらの機密性の高いファイルが要求された場合にファイルを提供しません。</span><span class="sxs-lookup"><span data-stu-id="60617-155">When the *web.config* file is present and and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="60617-156">*web.config*ファイルが存在しないか、不適切な名前が付けられているか、または通常の起動用にサイトを構成できない場合、IIS が機密性の高いファイルを公開する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="60617-156">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="60617-157">***web.config*ファイルは、展開環境に常に存在し、適切な名前が付けられ、通常の起動用にサイトを構成できる必要があります。*web.config* ファイルを運用環境の展開から削除しないでください。**</span><span class="sxs-lookup"><span data-stu-id="60617-157">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="60617-158">IIS 構成</span><span class="sxs-lookup"><span data-stu-id="60617-158">IIS configuration</span></span>

<span data-ttu-id="60617-159">**Windows Server オペレーティング システム**</span><span class="sxs-lookup"><span data-stu-id="60617-159">**Windows Server operating systems**</span></span>

<span data-ttu-id="60617-160">**Web サーバー (IIS)** サーバーの役割を有効にし、役割のサービスを設定します。</span><span class="sxs-lookup"><span data-stu-id="60617-160">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="60617-161">**[管理]** メニューから**役割と機能の追加**ウィザードを使用するか、**サーバー マネージャー**にあるリンクを使用します。</span><span class="sxs-lookup"><span data-stu-id="60617-161">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="60617-162">**[サーバーの役割]** のステップで、**[Web サーバー (IIS)]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="60617-162">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![[サーバーの役割の選択] のステップで Web サーバー IIS の役割を選択します。](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="60617-164">**[機能]** のステップの後に、**[役割サービスの]** のステップで Web サーバー (IIS) を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="60617-164">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="60617-165">希望する IIS の役割サービスを選択するか、既定の役割サービスをそのまま使用します。</span><span class="sxs-lookup"><span data-stu-id="60617-165">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![[役割サービスの選択] のステップで既定の役割サービスを選択します。](index/_static/role-services-ws2016.png)

   <span data-ttu-id="60617-167">**Windows 認証 (任意)**</span><span class="sxs-lookup"><span data-stu-id="60617-167">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="60617-168">Windows 認証を有効にするには、**[Web サーバー]** > **[セキュリティ]** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="60617-168">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="60617-169">**[Windows 認証]** 機能を選択します。</span><span class="sxs-lookup"><span data-stu-id="60617-169">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="60617-170">詳細については、「[Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)」および「[Configure Windows authentication](xref:security/authentication/windowsauth)」(Windows 認証の構成) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-170">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="60617-171">**Websocket (省略可能)**</span><span class="sxs-lookup"><span data-stu-id="60617-171">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="60617-172">WebSockets は、ASP.NET Core 1.1 以降でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="60617-172">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="60617-173">Websocket を有効にするには、**[Web サーバー]** > **[アプリケーション開発]** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="60617-173">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="60617-174">**[WebSocket プロトコル]** 機能を選択します。</span><span class="sxs-lookup"><span data-stu-id="60617-174">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="60617-175">詳細については、[WebSockets](xref:fundamentals/websockets) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-175">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="60617-176">**[確認]** のステップを経て Web サーバーの役割とサービスをインストールします。</span><span class="sxs-lookup"><span data-stu-id="60617-176">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="60617-177">**Web サーバー (IIS)** の役割をインストールした後、サーバーと IIS の再起動は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="60617-177">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="60617-178">**Windows デスクトップ オペレーティング システム**</span><span class="sxs-lookup"><span data-stu-id="60617-178">**Windows desktop operating systems**</span></span>

<span data-ttu-id="60617-179">**[IIS 管理コンソール]** と **[World Wide Web サービス]** を有効にします。</span><span class="sxs-lookup"><span data-stu-id="60617-179">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="60617-180">**[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。</span><span class="sxs-lookup"><span data-stu-id="60617-180">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="60617-181">**インターネット インフォメーション サービス (IIS)** ノードを開きます。</span><span class="sxs-lookup"><span data-stu-id="60617-181">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="60617-182">**[Web 管理ツール]** ノードを開きます。</span><span class="sxs-lookup"><span data-stu-id="60617-182">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="60617-183">**[IIS 管理コンソール]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="60617-183">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="60617-184">**[World Wide Web サービス]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="60617-184">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="60617-185">**[World Wide Web サービス]** の既定の機能をそのまま使用するか、IIS 機能をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="60617-185">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="60617-186">**Windows 認証 (任意)**</span><span class="sxs-lookup"><span data-stu-id="60617-186">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="60617-187">Windows 認証を有効にするには、**[World Wide Web サービス]** > **[セキュリティ]** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="60617-187">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="60617-188">**[Windows 認証]** 機能を選択します。</span><span class="sxs-lookup"><span data-stu-id="60617-188">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="60617-189">詳細については、「[Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)」および「[Configure Windows authentication](xref:security/authentication/windowsauth)」(Windows 認証の構成) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-189">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="60617-190">**Websocket (省略可能)**</span><span class="sxs-lookup"><span data-stu-id="60617-190">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="60617-191">WebSockets は、ASP.NET Core 1.1 以降でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="60617-191">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="60617-192">Websocket を有効にするには、**[World Wide Web サービス]** > **[アプリケーション開発機能]** ノードを展開します。</span><span class="sxs-lookup"><span data-stu-id="60617-192">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="60617-193">**[WebSocket プロトコル]** 機能を選択します。</span><span class="sxs-lookup"><span data-stu-id="60617-193">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="60617-194">詳細については、[WebSockets](xref:fundamentals/websockets) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-194">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="60617-195">IIS のインストールに再起動が必要な場合は、システムを再起動します。</span><span class="sxs-lookup"><span data-stu-id="60617-195">If the IIS installation requires a restart, restart the system.</span></span>

![[Windows の機能] で [IIS 管理コンソール] と [World Wide Web サービス] を選択します。](index/_static/windows-features-win10.png)

---

## <a name="install-the-net-core-windows-server-hosting-bundle"></a><span data-ttu-id="60617-197">.NET Core Windows Server ホスティング バンドルのインストール</span><span class="sxs-lookup"><span data-stu-id="60617-197">Install the .NET Core Windows Server Hosting bundle</span></span>

1. <span data-ttu-id="60617-198">ホスティング システムに [.NET Core Windows Server ホスティング バンドル](https://aka.ms/dotnetcore-2-windowshosting)をインストールします。</span><span class="sxs-lookup"><span data-stu-id="60617-198">Install the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore-2-windowshosting) on the hosting system.</span></span> <span data-ttu-id="60617-199">このバンドルをインストールすることで、.NET Core ランタイム、.NET Core ライブラリ、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="60617-199">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="60617-200">このモジュールは、IIS と Kestrel サーバーの間にリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="60617-200">The module creates the reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="60617-201">システムにインターネット接続が設定されていない場合は、.NET Core Windows Server ホスティング バンドルをインストールする前に、[Microsoft Visual C++ 2015 再頒布可能パッケージ](https://www.microsoft.com/download/details.aspx?id=53840)を入手してインストールしてください。</span><span class="sxs-lookup"><span data-stu-id="60617-201">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Windows Server Hosting bundle.</span></span>

   <span data-ttu-id="60617-202">**重要**。</span><span class="sxs-lookup"><span data-stu-id="60617-202">**Important!**</span></span> <span data-ttu-id="60617-203">ホスティングのバンドルが IIS の前にインストールされている場合、バンドルのインストールを修復する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60617-203">If the hosting bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="60617-204">IIS をインストールした後に、ホスティングのバンドルインストーラーをもう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="60617-204">Run the hosting bundle installer again after installing IIS.</span></span>

1. <span data-ttu-id="60617-205">システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行します。</span><span class="sxs-lookup"><span data-stu-id="60617-205">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span> <span data-ttu-id="60617-206">IIS を再起動すると、インストーラーによって行われたシステム パスへの変更が取得されます。</span><span class="sxs-lookup"><span data-stu-id="60617-206">Restarting IIS picks up a change to the system PATH made by the installer.</span></span>

> [!NOTE]
> <span data-ttu-id="60617-207">IIS 共有構成の詳細については、「[ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)」 (IIS 共有構成の ASP.NET Core モジュール) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-207">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="60617-208">Visual Studio で発行する場合の Web 配置のインストール</span><span class="sxs-lookup"><span data-stu-id="60617-208">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="60617-209">[Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用してサーバーにアプリを展開する場合、Web 配置の最新バージョンをサーバーにインストールします。</span><span class="sxs-lookup"><span data-stu-id="60617-209">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="60617-210">Web 配置をインストールするには、[Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) を使用するか、[Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=43717)からインストーラーを取得します。</span><span class="sxs-lookup"><span data-stu-id="60617-210">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="60617-211">WebPI を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="60617-211">The preferred method is to use WebPI.</span></span> <span data-ttu-id="60617-212">WebPI は、スタンドアロンのセットアップとホスティング プロバイダー向けの構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="60617-212">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="60617-213">IIS サイトを作成する</span><span class="sxs-lookup"><span data-stu-id="60617-213">Create the IIS site</span></span>

1. <span data-ttu-id="60617-214">ホスト システムで、アプリの公開フォルダーとファイルを格納するフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="60617-214">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="60617-215">アプリの展開レイアウトについては、「[ディレクトリ構造](xref:host-and-deploy/directory-structure)」のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-215">An app's deployment layout is described in the [Directory Structure](xref:host-and-deploy/directory-structure) topic.</span></span>

1. <span data-ttu-id="60617-216">新しいフォルダー内で、stdout ログが有効になっている場合に ASP.NET Core Module stdout ログを保持するための *logs* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="60617-216">Within the new folder, create a *logs* folder to hold ASP.NET Core Module stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="60617-217">ペイロードに *logs* フォルダーを含めてアプリを配置する場合は、この手順をスキップします。</span><span class="sxs-lookup"><span data-stu-id="60617-217">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="60617-218">プロジェクトがローカルでビルドされるときに、MSBuild を有効にして、*logs* フォルダーを自動的に作成する手順については、「[ディレクトリ構造](xref:host-and-deploy/directory-structure)」のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-218">For instructions on how to enable MSBuild to create the *logs* folder automatically when the project is built locally, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

   <span data-ttu-id="60617-219">**重要**。</span><span class="sxs-lookup"><span data-stu-id="60617-219">**Important!**</span></span> <span data-ttu-id="60617-220">stdout ログは、アプリケーションの起動エラーのトラブルシューティングにのみ使用します。</span><span class="sxs-lookup"><span data-stu-id="60617-220">Only use the stdout log to troubleshoot app startup failures.</span></span> <span data-ttu-id="60617-221">日常的なアプリのログ記録に、stdout ログを使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="60617-221">Never use stdout logging for routine app logging.</span></span> <span data-ttu-id="60617-222">ログ ファイルのサイズまたは作成されるログ ファイルの数に制限はありません。</span><span class="sxs-lookup"><span data-stu-id="60617-222">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="60617-223">stdout ログの詳細については、「[Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)」(ログの作成とリダイレクト) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-223">For more information on the stdout log, see [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span> <span data-ttu-id="60617-224">ASP.NET Core アプリケーションのログ記録の詳細については、「[ログ](xref:fundamentals/logging/index)」のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-224">For information on logging in an ASP.NET Core app, see the [Logging](xref:fundamentals/logging/index) topic.</span></span>

1. <span data-ttu-id="60617-225">**IIS マネージャー**の **[接続]** パネルでサーバーのノードを開きます。</span><span class="sxs-lookup"><span data-stu-id="60617-225">In **IIS Manager**, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="60617-226">**[サイト]** フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="60617-226">Right-click the **Sites** folder.</span></span> <span data-ttu-id="60617-227">コンテキスト メニューで **[Web サイトの追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="60617-227">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="60617-228">**[サイト名]** を指定し、**[物理パス]** にはアプリの配置フォルダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="60617-228">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="60617-229">**[バインド]** の構成を指定して **[OK]** を選択し、Web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="60617-229">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![[Web サイトの追加] のステップでサイト名、物理パス、ホスト名を指定します。](index/_static/add-website-ws2016.png)

1. <span data-ttu-id="60617-231">サーバーのノードでは、**[アプリケーション プール]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="60617-231">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="60617-232">サイトのアプリケーション プールを右クリックし、コンテキスト メニューから **[基本設定]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="60617-232">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="60617-233">**[アプリケーション プールの編集]** ウィンドウで、**[.NET CLR バージョン]** を **[マネージ コードなし]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="60617-233">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![.NET CLR バージョンとして [マネージ コードなし] を設定します。](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="60617-235">ASP.NET Core は、別個のプロセスで実行され、ランタイムを管理します。</span><span class="sxs-lookup"><span data-stu-id="60617-235">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="60617-236">ASP.NET Core を使用するためにデスクトップ CLR を読み込む必要はありません。</span><span class="sxs-lookup"><span data-stu-id="60617-236">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span> <span data-ttu-id="60617-237">**[.NET CLR バージョン]** の **[マネージ コードなし]** への設定は、任意です。</span><span class="sxs-lookup"><span data-stu-id="60617-237">Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span>

1. <span data-ttu-id="60617-238">プロセス モデル ID に適切なアクセス許可があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="60617-238">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="60617-239">アプリ プールの既定の ID (**[プロセス モデル]** > **[ID]**) を **ApplicationPoolIdentity** から別の ID に変更した場合は、アプリのフォルダー、データベース、その他の必要なリソースにアクセスするために要求されるアクセス許可が新しい ID に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="60617-239">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="60617-240">たとえば、アプリケーション プールには、アプリがファイルの読み取りおよび書き込みを行うフォルダーへの読み取りおよび書き込みアクセスが必要です。</span><span class="sxs-lookup"><span data-stu-id="60617-240">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="60617-241">**Windows 認証の構成 (任意)**</span><span class="sxs-lookup"><span data-stu-id="60617-241">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="60617-242">詳細については、「[Windows 認証を構成する](xref:security/authentication/windowsauth)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-242">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="60617-243">アプリを配置する</span><span class="sxs-lookup"><span data-stu-id="60617-243">Deploy the app</span></span>

<span data-ttu-id="60617-244">ホスト システム上に作成したフォルダーにアプリを展開します。</span><span class="sxs-lookup"><span data-stu-id="60617-244">Deploy the app to the folder created on the hosting system.</span></span> <span data-ttu-id="60617-245">[Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)は、推奨される展開のメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="60617-245">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="60617-246">Visual Studio からのアプリの展開</span><span class="sxs-lookup"><span data-stu-id="60617-246">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="60617-247">Web 配置に使用する発行プロファイルの作成方法については、「[Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)」 (ASP.NET Core アプリ展開用の Visual Studio の発行プロファイル) のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-247">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="60617-248">ホスティング プロバイダーから発行プロファイルまたは発行プロファイルを作成するためのサポートが提供されている場合は、そのプロファイルをダウンロードし、Visual Studio の **[発行]** ダイアログを使用してインポートします。</span><span class="sxs-lookup"><span data-stu-id="60617-248">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![[発行] ダイアログ ページ](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="60617-250">Visual Studio 外部での Web 配置</span><span class="sxs-lookup"><span data-stu-id="60617-250">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="60617-251">Visual Studio の外部で、コマンド ラインから [Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="60617-251">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="60617-252">詳細については、[Web 配置ツール](/iis/publish/using-web-deploy/use-the-web-deployment-tool)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-252">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="60617-253">Web 配置の代替手段</span><span class="sxs-lookup"><span data-stu-id="60617-253">Alternatives to Web Deploy</span></span>

<span data-ttu-id="60617-254">手動コピー、Xcopy、Robocopy、PowerShell などの任意の方法を使用して、ホスティング システムにアプリを移動します。</span><span class="sxs-lookup"><span data-stu-id="60617-254">Use any of several methods to move the app to the hosting system, such as manual copy, Xcopy, Robocopy, or PowerShell.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="60617-255">Web サイトを閲覧する</span><span class="sxs-lookup"><span data-stu-id="60617-255">Browse the website</span></span>

![Microsoft Edge ブラウザーに IIS のスタートアップ ページが読み込まれています。](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="60617-257">ロックされた展開ファイル</span><span class="sxs-lookup"><span data-stu-id="60617-257">Locked deployment files</span></span>

<span data-ttu-id="60617-258">アプリが実行中は、展開フォルダー内のファイルがロックされます。</span><span class="sxs-lookup"><span data-stu-id="60617-258">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="60617-259">展開中にロックされたファイルを上書きすることはできません。</span><span class="sxs-lookup"><span data-stu-id="60617-259">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="60617-260">展開でロックされているファイルをリリースするには、次の**いずれか**の方法を使用してアプリ プールを停止します。</span><span class="sxs-lookup"><span data-stu-id="60617-260">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="60617-261">Web 配置を使用し、プロジェクト ファイル内の `Microsoft.NET.Sdk.Web` を参照して停止します。</span><span class="sxs-lookup"><span data-stu-id="60617-261">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="60617-262">*app_offline.htm* ファイルは、Web アプリのディレクトリのルートに配置されます。</span><span class="sxs-lookup"><span data-stu-id="60617-262">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="60617-263">ファイルが存在する場合、ASP.NET Core モジュールはアプリを正常にシャットダウンし、展開中に *app_offline.htm* ファイルを提供します。</span><span class="sxs-lookup"><span data-stu-id="60617-263">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="60617-264">詳細については、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module#appofflinehtm)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-264">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>
* <span data-ttu-id="60617-265">サーバー上の IIS マネージャーで手動によりアプリ プールを停止します。</span><span class="sxs-lookup"><span data-stu-id="60617-265">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="60617-266">PowerShell を使用して、アプリ プールを停止して再起動します (PowerShell 5 以降が必要)。</span><span class="sxs-lookup"><span data-stu-id="60617-266">Use PowerShell to stop and restart the app pool (requires PowerShell 5 or later):</span></span>

  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

## <a name="data-protection"></a><span data-ttu-id="60617-267">データの保護</span><span class="sxs-lookup"><span data-stu-id="60617-267">Data protection</span></span>

<span data-ttu-id="60617-268">[ASP.NET Core データ保護スタック](xref:security/data-protection/index)は、認証で使用されるミドルウェアを含め、いくつかの [ASP.NET Core](xref:fundamentals/middleware/index) ミドルウェアによって使用されます。</span><span class="sxs-lookup"><span data-stu-id="60617-268">The [ASP.NET Core Data Protection stack](xref:security/data-protection/index) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="60617-269">データ保護 API がユーザーのコードから呼び出されない場合でも、配置スクリプトを使用するかユーザー コードで、永続的な暗号化[キー ストア](xref:security/data-protection/implementation/key-management)を作成するようにデータ保護を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60617-269">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="60617-270">データ保護を構成しない場合、既定でキーはメモリ内に保持され、アプリが再起動すると破棄されます。</span><span class="sxs-lookup"><span data-stu-id="60617-270">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="60617-271">キーリングがメモリに格納されている場合、アプリを再起動すると次のことが行われます。</span><span class="sxs-lookup"><span data-stu-id="60617-271">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="60617-272">すべての cookie ベースの認証トークンは無効になります。</span><span class="sxs-lookup"><span data-stu-id="60617-272">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="60617-273">ユーザーは、次回の要求時に再度サインインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="60617-273">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="60617-274">キーリングで保護されているデータは、いずれも復号化できなくなります。</span><span class="sxs-lookup"><span data-stu-id="60617-274">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="60617-275">これには、[CSRF トークン](xref:security/anti-request-forgery#how-does-aspnet-core-mvc-address-csrf)と [ASP.NET Core MVC tempdata cookie](xref:fundamentals/app-state#tempdata) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="60617-275">This may include [CSRF tokens](xref:security/anti-request-forgery#how-does-aspnet-core-mvc-address-csrf) and [ASP.NET Core MVC tempdata cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="60617-276">キーリングを保持するために IIS でのデータ保護を構成するには、次の**いずれか**の方法を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60617-276">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="60617-277">**データ保護のレジストリ キーを作成する**</span><span class="sxs-lookup"><span data-stu-id="60617-277">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="60617-278">ASP.NET Core アプリで使用されるデータ保護キーは、アプリ外部のレジストリに格納されます。</span><span class="sxs-lookup"><span data-stu-id="60617-278">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="60617-279">特定のアプリのキーを保持するには、アプリ プール用のレジストリ キーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60617-279">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="60617-280">非 Web ファーム IIS のスタンドアロン インストールの場合、ASP.NET Core アプリで使用するアプリ プールごとに、[データ保護の PowerShell スクリプト Provision-AutoGenKeys.ps1](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) を使用できます。</span><span class="sxs-lookup"><span data-stu-id="60617-280">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="60617-281">このスクリプトは、HKLM レジストリにレジストリ キーを作成します。そのキーは、アプリのアプリ プールのワーカー プロセス アカウントのみがアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="60617-281">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="60617-282">キーは保存時に、DPAPI を使用して、コンピューター全体に適用するキーによって暗号化されます。</span><span class="sxs-lookup"><span data-stu-id="60617-282">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="60617-283">Web ファームのシナリオでは、UNC パスを使用してデータ保護キー リングを格納するようにアプリを構成できます。</span><span class="sxs-lookup"><span data-stu-id="60617-283">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="60617-284">既定では、データ保護キーは暗号化されません。</span><span class="sxs-lookup"><span data-stu-id="60617-284">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="60617-285">ネットワーク共有のファイルのアクセス許可が、アプリを実行する Windows アカウントに限定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="60617-285">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="60617-286">保存時のキーを保護するために、X509 証明書を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="60617-286">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="60617-287">ユーザーが証明書をアップデートできるメカニズムを検討している場合、ユーザーの信頼できる証明書ストアに証明書を配置し、ユーザーのアプリが実行されるすべてのコンピューターで証明書を利用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="60617-287">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="60617-288">詳細については、「[データ保護の構成](xref:security/data-protection/configuration/overview)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-288">See [Configuring Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="60617-289">**ユーザー プロファイルを読み込むための IIS アプリケーション プールを構成する**</span><span class="sxs-lookup"><span data-stu-id="60617-289">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="60617-290">この設定は、アプリ プールの **[詳細設定]** の **[プロセス モデル]** セクションにあります。</span><span class="sxs-lookup"><span data-stu-id="60617-290">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="60617-291">[ユーザー プロファイルの読み込み] を `True` に設定します。</span><span class="sxs-lookup"><span data-stu-id="60617-291">Set Load User Profile to `True`.</span></span> <span data-ttu-id="60617-292">これによりキーはユーザー プロファイル ディレクトリに格納され、DPAPI を使用して、アプリ プールで使うユーザー アカウントに固有のキーによって保護されます。</span><span class="sxs-lookup"><span data-stu-id="60617-292">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used by the app pool.</span></span>

* <span data-ttu-id="60617-293">**ファイル システムをキー リング ストアとして使用する**</span><span class="sxs-lookup"><span data-stu-id="60617-293">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="60617-294">[ファイル システムをキー リング ストアとして使用](xref:security/data-protection/configuration/overview)するようにアプリ コードを調整します。</span><span class="sxs-lookup"><span data-stu-id="60617-294">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="60617-295">X509 証明書を使用してキー リングを保護し、その証明書が信頼された証明書であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="60617-295">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="60617-296">証明書が自己署名されている場合は、信頼されたルート ストアにその証明書を配置します。</span><span class="sxs-lookup"><span data-stu-id="60617-296">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="60617-297">Web ファームで IIS を使用する場合:</span><span class="sxs-lookup"><span data-stu-id="60617-297">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="60617-298">すべてのコンピューターがアクセスできるファイル共有を使用します。</span><span class="sxs-lookup"><span data-stu-id="60617-298">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="60617-299">X509 証明書を各コンピューターに配置します。</span><span class="sxs-lookup"><span data-stu-id="60617-299">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="60617-300">[コード内にデータ保護](xref:security/data-protection/configuration/overview)を構成します。</span><span class="sxs-lookup"><span data-stu-id="60617-300">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="60617-301">**コンピューター全体に適用するデータ保護ポリシーを設定する**</span><span class="sxs-lookup"><span data-stu-id="60617-301">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="60617-302">データ保護システムでは、データ保護 API を使用するすべてのアプリに対して、[コンピューター全体に適用する既定のポリシー](xref:security/data-protection/configuration/machine-wide-policy)を設定するためのサポートは限定的です。</span><span class="sxs-lookup"><span data-stu-id="60617-302">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="60617-303">詳細については、[データ保護](xref:security/data-protection/index)に関するドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-303">See the [data protection documentation](xref:security/data-protection/index) for details.</span></span>

## <a name="sub-application-configuration"></a><span data-ttu-id="60617-304">サブアプリケーション構成</span><span class="sxs-lookup"><span data-stu-id="60617-304">Sub-application configuration</span></span>

<span data-ttu-id="60617-305">ルート アプリの下に追加したサブアプリに、ハンドラーとして ASP.NET Core モジュールを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="60617-305">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="60617-306">このモジュールをサブアプリの *web.config* ファイルにハンドラーとして追加すると、サブアプリを閲覧しようとすると、エラーのある構成ファイルを参照する *500.19 内部サーバー エラー* が返されます。</span><span class="sxs-lookup"><span data-stu-id="60617-306">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="60617-307">次の例は、ASP.NET Core サブアプリの発行済み *web.config* ファイルを示しています。</span><span class="sxs-lookup"><span data-stu-id="60617-307">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="60617-308">ASP.NET Core アプリの下に ASP.NET Core 以外のサブアプリをホストする場合は、サブアプリの *web.config* ファイルにある継承されたハンドラーを明示的に削除します。</span><span class="sxs-lookup"><span data-stu-id="60617-308">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="60617-309">ASP.NET Core モジュールを構成する方法の詳細については、「[ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)」と「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-309">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="60617-310">web.config による IIS の構成</span><span class="sxs-lookup"><span data-stu-id="60617-310">Configuration of IIS with web.config</span></span>

<span data-ttu-id="60617-311">IIS の構成は、リバース プロキシ構成に適用されるこれらの IIS 機能の *web.config*の **\<system.webServer>** セクションの影響を受けます。</span><span class="sxs-lookup"><span data-stu-id="60617-311">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="60617-312">IIS が動的な圧縮を使用するようにサーバー レベルで構成されている場合、アプリの *web.config* ファイルで **\<urlCompression>** 要素を使用して無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="60617-312">If IIS is configured at the server level to use dynamic compression, the **\<urlCompression>** element in the app's *web.config* file can disable it.</span></span>

<span data-ttu-id="60617-313">詳細については、[\<system.webServer> の構成リファレンス](/iis/configuration/system.webServer/)、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」および「[IIS モジュールと ASP.NET Core の使用](xref:host-and-deploy/iis/modules)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-313">For more information, see the [configuration reference for \<system.webServer>](/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module), and [Using IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="60617-314">分離されたアプリ プール (IIS 10.0 以降でサポートされています) で実行する個別アプリに対して環境変数を設定するには、IIS のリファレンス ドキュメントで、[環境変数\<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) のトピックにある *AppCmd.exe コマンド*のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-314">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="60617-315">web.config の構成のセクション</span><span class="sxs-lookup"><span data-stu-id="60617-315">Configuration sections of web.config</span></span>

<span data-ttu-id="60617-316">*web.config* の ASP.NET 4.x アプリの構成セクションは、ASP.NET Core アプリの構成では使用されません。</span><span class="sxs-lookup"><span data-stu-id="60617-316">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="60617-317">**\<system.web >**</span><span class="sxs-lookup"><span data-stu-id="60617-317">**\<system.web>**</span></span>
* <span data-ttu-id="60617-318">**\<appSettings>**</span><span class="sxs-lookup"><span data-stu-id="60617-318">**\<appSettings>**</span></span>
* <span data-ttu-id="60617-319">**\<connectionStrings>**</span><span class="sxs-lookup"><span data-stu-id="60617-319">**\<connectionStrings>**</span></span>
* <span data-ttu-id="60617-320">**\<location>**</span><span class="sxs-lookup"><span data-stu-id="60617-320">**\<location>**</span></span>

<span data-ttu-id="60617-321">ASP.NET Core アプリは、他の構成プロバイダーを使用して構成されます。</span><span class="sxs-lookup"><span data-stu-id="60617-321">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="60617-322">詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-322">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="60617-323">アプリケーション プール</span><span class="sxs-lookup"><span data-stu-id="60617-323">Application Pools</span></span>

<span data-ttu-id="60617-324">サーバーで複数の Web サイトをホストする場合は、アプリをそれぞれ専用のアプリ プールで実行して、アプリを相互に分離します。</span><span class="sxs-lookup"><span data-stu-id="60617-324">When hosting multiple websites on a server, isolate the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="60617-325">IIS の **[Web サイトの追加]** ダイアログはこの構成の既定です。</span><span class="sxs-lookup"><span data-stu-id="60617-325">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="60617-326">**[サイト名]** を指定すると、入力したテキストが自動的に **[アプリケーション プール]** テキストボックスに設定されます。</span><span class="sxs-lookup"><span data-stu-id="60617-326">When **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="60617-327">サイトが追加されるときに、そのサイト名を使用して新しいアプリ プールが作成されます。</span><span class="sxs-lookup"><span data-stu-id="60617-327">A new app pool is created using the site name when the site is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="60617-328">アプリケーション プール ID</span><span class="sxs-lookup"><span data-stu-id="60617-328">Application Pool Identity</span></span>

<span data-ttu-id="60617-329">アプリ プール ID アカウントを使用すると、ドメインやローカル アカウントを作成して管理する必要なく、一意のアカウントでアプリを実行できます。</span><span class="sxs-lookup"><span data-stu-id="60617-329">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="60617-330">IIS 8.0 以降の IIS 管理者ワーカー プロセス (WAS) は、新しいアプリ プールの名前で仮想アカウントを作成し、既定によってアプリ プールのワーカー プロセスをこのアカウントで実行します。</span><span class="sxs-lookup"><span data-stu-id="60617-330">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="60617-331">IIS 管理コンソールにあるアプリ プールの **[詳細設定]** で、**ID** が **ApplicationPoolIdentity** を使用するように設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="60617-331">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![アプリケーション プールの [詳細設定] ダイアログ](index/_static/apppool-identity.png)

<span data-ttu-id="60617-333">IIS 管理プロセスは、Windows セキュリティ システムでのアプリ プールの名前を使用して、セキュリティで保護された識別子を作成します。</span><span class="sxs-lookup"><span data-stu-id="60617-333">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="60617-334">この ID を使用してリソースを保護することができます。</span><span class="sxs-lookup"><span data-stu-id="60617-334">Resources can be secured using this identity.</span></span> <span data-ttu-id="60617-335">ただし、この ID は実際のユーザー アカウントではなく、Windows ユーザーの管理コンソールに表示されません。</span><span class="sxs-lookup"><span data-stu-id="60617-335">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="60617-336">アプリに対する IIS ワーカー プロセスのアクセス許可を昇格させる必要がある場合は、アプリを含むディレクトリのアクセス制御リスト (ACL) を変更します。</span><span class="sxs-lookup"><span data-stu-id="60617-336">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="60617-337">エクスプローラーを開き、そのディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="60617-337">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="60617-338">そのディレクトリを右クリックし、**[プロパティ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="60617-338">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="60617-339">**[セキュリティ]** タブの **[編集]** ボタンを選択し、**[追加]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="60617-339">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="60617-340">**[場所]** ボタンを選択し、システムが選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="60617-340">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="60617-341">**[選択するオブジェクト名を入力してください]** 領域に、「**IIS AppPool\\<app_pool_name>**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="60617-341">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="60617-342">**[名前の確認]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="60617-342">Select the **Check Names** button.</span></span> <span data-ttu-id="60617-343">*DefaultAppPool* で、**IIS AppPool\DefaultAppPool** を使用して名前を確認します。</span><span class="sxs-lookup"><span data-stu-id="60617-343">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="60617-344">**[名前の確認]** ボタンが選択されているときには、**DefaultAppPool** の値が、オブジェクト名領域に表示されます。</span><span class="sxs-lookup"><span data-stu-id="60617-344">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="60617-345">オブジェクト名の領域に直接アプリケーション プール名を入力することはできません。</span><span class="sxs-lookup"><span data-stu-id="60617-345">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="60617-346">オブジェクト名を確認するときには、**IIS AppPool\\<app_pool_name>** の形式を使用します。</span><span class="sxs-lookup"><span data-stu-id="60617-346">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![アプリ フォルダーのユーザーまたはグループのダイアログを選択します。[名前の確認] を選択する前に、オブジェクト名領域で "DefaultAppPool" のアプリ名が "IIS AppPool\" に適用されます。](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="60617-348">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="60617-348">Select **OK**.</span></span>

   ![アプリ フォルダーのユーザーまたはグループのダイアログを選択します。[名前の確認] を選択した後に、オブジェクト名 "DefaultAppPool" がオブジェクト名領域に表示されます。](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="60617-350">読み取り &amp; 実行アクセス許可は、既定で付与される必要があります。</span><span class="sxs-lookup"><span data-stu-id="60617-350">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="60617-351">必要に応じて、追加のアクセス許可を提供します。</span><span class="sxs-lookup"><span data-stu-id="60617-351">Provide additional permissions as needed.</span></span>

<span data-ttu-id="60617-352">**ICACLS** ツールを使用してコマンド プロンプトでアクセス許可を付与することもできます。</span><span class="sxs-lookup"><span data-stu-id="60617-352">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="60617-353">たとえば、*DefaultAppPool* を使用する場合、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="60617-353">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="60617-354">詳細については、「[icacls](/windows-server/administration/windows-commands/icacls)」のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="60617-354">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="60617-355">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="60617-355">Additional resources</span></span>

* [<span data-ttu-id="60617-356">IIS での ASP.NET Core のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="60617-356">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="60617-357">Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス</span><span class="sxs-lookup"><span data-stu-id="60617-357">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="60617-358">ASP.NET Core モジュールの概要</span><span class="sxs-lookup"><span data-stu-id="60617-358">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="60617-359">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="60617-359">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="60617-360">IIS モジュールと ASP.NET Core の使用</span><span class="sxs-lookup"><span data-stu-id="60617-360">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="60617-361">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="60617-361">Introduction to ASP.NET Core</span></span>](../index.md)
* [<span data-ttu-id="60617-362">Microsoft IIS 公式サイト</span><span class="sxs-lookup"><span data-stu-id="60617-362">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="60617-363">Microsoft TechNet ライブラリ: Windows Server</span><span class="sxs-lookup"><span data-stu-id="60617-363">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
