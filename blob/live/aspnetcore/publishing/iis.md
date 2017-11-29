---
title: "IIS を使用した Windows での ASP.NET Core のホスト"
author: guardrex
description: "Windows Server インターネット インフォメーション サービス (IIS) の構成と ASP.NET Core アプリケーションの展開について説明します。"
keywords: "ASP.NET Core, インターネット インフォメーション サービス, iis, windows server, ホスティング バンドル, asp.net core モジュール, web 配置"
ms.author: riande
manager: wpickett
ms.date: 03/13/2017
ms.topic: article
ms.assetid: a4449ad3-5bad-410c-afa7-dc32d832b552
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/iis
ms.openlocfilehash: e9e9019d5b879498e8800bb579c177dd3ad64061
ms.sourcegitcommit: 96af03c9f44f7c206e68ae3ef8596068e6b4e5fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/15/2017
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="e9a15-104">IIS を使用した Windows での ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="e9a15-104">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="e9a15-105">執筆者: [Luke Latham](https://github.com/guardrex)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e9a15-105">By [Luke Latham](https://github.com/guardrex) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="e9a15-106">サポートされるオペレーティング システム</span><span class="sxs-lookup"><span data-stu-id="e9a15-106">Supported operating systems</span></span>

<span data-ttu-id="e9a15-107">次のオペレーティング システムがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="e9a15-107">The following operating systems are supported:</span></span>

* <span data-ttu-id="e9a15-108">Windows 7 およびそれ以降</span><span class="sxs-lookup"><span data-stu-id="e9a15-108">Windows 7 and newer</span></span>
* <span data-ttu-id="e9a15-109">Windows Server 2008 R2 およびそれ以降&#8224;</span><span class="sxs-lookup"><span data-stu-id="e9a15-109">Windows Server 2008 R2 and newer&#8224;</span></span>

<span data-ttu-id="e9a15-110">&#8224;概念的には、このドキュメントで説明する IIS 構成は、Nano Server IIS で ASP.NET Core アプリケーションをホストする場合にも当てはまりますが、詳細な手順については「[ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server)」 (IIS を使用した Nano Server での ASP.NET Core) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-110">&#8224;Conceptually, the IIS configuration described in this document also applies to hosting ASP.NET Core applications on Nano Server IIS, but refer to [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) for specific instructions.</span></span>

<span data-ttu-id="e9a15-111">[HTTP.sys サーバー](xref:fundamentals/servers/httpsys) (以前の名称は [WebListener](xref:fundamentals/servers/weblistener)) が、IIS を含むリバース プロキシ構成で動作しません。</span><span class="sxs-lookup"><span data-stu-id="e9a15-111">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) won't work in a reverse-proxy configuration with IIS.</span></span> <span data-ttu-id="e9a15-112">[Kestrel サーバー](xref:fundamentals/servers/kestrel)を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-112">You must use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="e9a15-113">IIS 構成</span><span class="sxs-lookup"><span data-stu-id="e9a15-113">IIS configuration</span></span>

<span data-ttu-id="e9a15-114">**Web サーバー (IIS)** の役割を有効にし、役割のサービスを設定します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-114">Enable the **Web Server (IIS)** role and establish role services.</span></span>

### <a name="windows-desktop-operating-systems"></a><span data-ttu-id="e9a15-115">Windows デスクトップ オペレーティング システム</span><span class="sxs-lookup"><span data-stu-id="e9a15-115">Windows desktop operating systems</span></span>

<span data-ttu-id="e9a15-116">**[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-116">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="e9a15-117">**[インターネット インフォメーション サービス]** と **[Web 管理ツール]** のグループを開きます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-117">Open the group for **Internet Information Services** and **Web Management Tools**.</span></span> <span data-ttu-id="e9a15-118">**[IIS 管理コンソール]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-118">Check the box for **IIS Management Console**.</span></span> <span data-ttu-id="e9a15-119">**[World Wide Web サービス]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-119">Check the box for **World Wide Web Services**.</span></span> <span data-ttu-id="e9a15-120">**[World Wide Web サービス]** の既定の機能をそのまま使用するか、ニーズに合わせて IIS 機能をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-120">Accept the default features for **World Wide Web Services** or customize the IIS features to suit your needs.</span></span>

![[Windows の機能] で [IIS 管理コンソール] と [World Wide Web サービス] を選択します。](iis/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a><span data-ttu-id="e9a15-122">Windows Server オペレーティング システム</span><span class="sxs-lookup"><span data-stu-id="e9a15-122">Windows Server operating systems</span></span>

<span data-ttu-id="e9a15-123">サーバー オペレーティング システムでは、**[管理]** メニューから**役割と機能の追加**ウィザードを使用するか、**サーバー マネージャー**にあるリンクを使用します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-123">For server operating systems, use the **Add Roles and Features** wizard via the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="e9a15-124">**[サーバーの役割]** のステップで、**[Web サーバー (IIS)]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-124">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

![[サーバーの役割の選択] のステップで Web サーバー IIS の役割を選択します。](iis/_static/server-roles-ws2016.png)

<span data-ttu-id="e9a15-126">**[役割サービス]** のステップで、希望する IIS の役割サービスを選択するか、既定の役割サービスをそのまま使用します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-126">On the **Role services** step, select the IIS role services you desire or accept the default role services provided.</span></span>

![[役割サービスの選択] のステップで既定の役割サービスを選択します。](iis/_static/role-services-ws2016.png)

<span data-ttu-id="e9a15-128">**[確認]** のステップを経て Web サーバーの役割とサービスをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-128">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="e9a15-129">Web サーバー (IIS) の役割をインストールした後、サーバーと IIS の再起動は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="e9a15-129">A server/IIS restart isn't required after installing the Web Server (IIS) role.</span></span>

## <a name="install-the-net-core-windows-server-hosting-bundle"></a><span data-ttu-id="e9a15-130">.NET Core Windows Server ホスティング バンドルのインストール</span><span class="sxs-lookup"><span data-stu-id="e9a15-130">Install the .NET Core Windows Server Hosting bundle</span></span>

1. <span data-ttu-id="e9a15-131">ホスティング システムに [.NET Core Windows Server ホスティング バンドル](https://download.microsoft.com/download/5/C/1/5C190037-632B-443D-842D-39085F02E1E8/DotNetCore.2.0.3-WindowsHosting.exe)をインストールします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-131">Install the [.NET Core Windows Server Hosting bundle](https://download.microsoft.com/download/5/C/1/5C190037-632B-443D-842D-39085F02E1E8/DotNetCore.2.0.3-WindowsHosting.exe) on the hosting system.</span></span> <span data-ttu-id="e9a15-132">このバンドルをインストールすることで、.NET Core ランタイム、.NET Core ライブラリ、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-132">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="e9a15-133">このモジュールは、IIS と Kestrel サーバーの間にリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-133">The module creates the reverse-proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="e9a15-134">システムにインターネット接続が設定されていない場合は、.NET Core Windows Server ホスティング バンドルをインストールする前に、[Microsoft Visual C++ 2015 再頒布可能パッケージ](https://www.microsoft.com/download/details.aspx?id=53840)を入手してインストールしてください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-134">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Windows Server Hosting bundle.</span></span>

2. <span data-ttu-id="e9a15-135">システムを再起動するか、コマンド プロンプトから **net stop was /y** の後に続けて **net start w3svc** を実行して、システム PATH への変更を適用します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-135">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt to pick up a change to the system PATH.</span></span>

> [!NOTE]
> <span data-ttu-id="e9a15-136">IIS 共有構成を使用する場合は、「[ASP.NET Core Module with IIS Shared Configuration](xref:hosting/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)」 (IIS 共有構成の ASP.NET Core モジュール) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-136">If you use an IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:hosting/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="e9a15-137">Visual Studio で発行する場合の Web 配置のインストール</span><span class="sxs-lookup"><span data-stu-id="e9a15-137">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="e9a15-138">[Visual Studio](https://www.visualstudio.com/vs/) の [Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用してアプリケーションを展開する場合は、ホスティング システムに最新バージョンの Web 配置をインストールします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-138">If you intend to deploy your applications with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) in [Visual Studio](https://www.visualstudio.com/vs/), install the latest version of Web Deploy on the hosting system.</span></span> <span data-ttu-id="e9a15-139">Web 配置をインストールするには、[Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) を使用するか、[Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=43717)からインストーラーを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-139">To install Web Deploy, you can use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="e9a15-140">WebPI を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-140">The preferred method is to use WebPI.</span></span> <span data-ttu-id="e9a15-141">WebPI は、スタンドアロンのセットアップとホスティング プロバイダー向けの構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-141">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="e9a15-142">アプリケーション構成</span><span class="sxs-lookup"><span data-stu-id="e9a15-142">Application configuration</span></span>

### <a name="enabling-the-iisintegration-components"></a><span data-ttu-id="e9a15-143">IISIntegration コンポーネントを有効にする</span><span class="sxs-lookup"><span data-stu-id="e9a15-143">Enabling the IISIntegration components</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e9a15-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e9a15-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e9a15-145">一般的な *Program.cs* は [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を呼び出してホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-145">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to begin setting up a host.</span></span> <span data-ttu-id="e9a15-146">`CreateDefaultBuilder` は [Kestrel](xref:fundamentals/servers/kestrel) を Web サーバーとして構成し、ベース パスとポートを [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)に構成することで、IIS 統合を有効にします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-146">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e9a15-147">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e9a15-147">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e9a15-148">アプリの依存関係に [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) パッケージへの依存関係を含めます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-148">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in your app dependencies.</span></span> <span data-ttu-id="e9a15-149">*UseIISIntegration* 拡張メソッドを *WebHostBuilder* に追加して、IIS 統合ミドルウェアをアプリに組み込みます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-149">Incorporate IIS Integration middleware into the app by adding the *UseIISIntegration* extension method to *WebHostBuilder*:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="e9a15-150">`UseKestrel` と `UseIISIntegration` の両方が必要です。</span><span class="sxs-lookup"><span data-stu-id="e9a15-150">Both `UseKestrel` and `UseIISIntegration` are required.</span></span> <span data-ttu-id="e9a15-151">*UseIISIntegration()* を呼び出すコードはコードの移植性に影響しません。</span><span class="sxs-lookup"><span data-stu-id="e9a15-151">Code calling *UseIISIntegration* doesn't affect code portability.</span></span> <span data-ttu-id="e9a15-152">アプリが IIS の背後で実行されていない場合 (たとえば、アプリが Kestrel で直接実行されている場合)、`UseIISIntegration` は機能しません。</span><span class="sxs-lookup"><span data-stu-id="e9a15-152">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` no-ops.</span></span>

---

<span data-ttu-id="e9a15-153">ホスティングの詳細については、「[Hosting in ASP.NET Core](xref:fundamentals/hosting)」(ASP.NET Core でのホスティング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-153">For more information on hosting, see [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span></span>

### <a name="iis-options"></a><span data-ttu-id="e9a15-154">IIS のオプション</span><span class="sxs-lookup"><span data-stu-id="e9a15-154">IIS options</span></span>

<span data-ttu-id="e9a15-155">*IISIntegration* サービスのオプションを構成するには、*IISOptions* のサービス構成を *ConfigureServices*に含めます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-155">To configure *IISIntegration* service options, include a service configuration for *IISOptions* in *ConfigureServices*:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| <span data-ttu-id="e9a15-156">オプション</span><span class="sxs-lookup"><span data-stu-id="e9a15-156">Option</span></span>                         | <span data-ttu-id="e9a15-157">既定値</span><span class="sxs-lookup"><span data-stu-id="e9a15-157">Default</span></span> | <span data-ttu-id="e9a15-158">設定</span><span class="sxs-lookup"><span data-stu-id="e9a15-158">Setting</span></span> |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="e9a15-159">`true` の場合、認証ミドルウェアが `HttpContext.User` を設定し、一般的な課題に応答します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-159">If `true`, the authentication middleware sets the `HttpContext.User` and responds to generic challenges.</span></span> <span data-ttu-id="e9a15-160">`false` の場合、認証ミドルウェアは ID (`HttpContext.User`) を提供するだけで、`AuthenticationScheme` によって明示的に要求されたときに課題に応答します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-160">If `false`, the authentication middleware only provides an identity (`HttpContext.User`) and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="e9a15-161">`AutomaticAuthentication` を機能させるためには、IIS で Windows 認証を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-161">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="e9a15-162">ログイン ページでユーザーに表示名が表示されるように設定します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-162">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="e9a15-163">`true` の場合、`MS-ASPNETCORE-CLIENTCERT` 要求ヘッダーが指定されていると、`HttpContext.Connection.ClientCertificate` が設定されます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-163">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="webconfig"></a><span data-ttu-id="e9a15-164">web.config</span><span class="sxs-lookup"><span data-stu-id="e9a15-164">web.config</span></span>

<span data-ttu-id="e9a15-165">*web.config* ファイルでは、主に ASP.NET Core モジュールを設定します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-165">The *web.config* file primarily configures the ASP.NET Core Module.</span></span> <span data-ttu-id="e9a15-166">必要に応じて、追加の IIS の構成設定を指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-166">It may optionally provide additional IIS configuration settings.</span></span> <span data-ttu-id="e9a15-167">*web.config* の作成、変換、発行は .NET Core Web SDK (`Microsoft.NET.Sdk.Web`) で処理されます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-167">Creating, transforming, and publishing *web.config* is handled by the .NET Core Web SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="e9a15-168">SDK は、プロジェクト ファイル (*.csproj*) の先頭 (`<Project Sdk="Microsoft.NET.Sdk.Web">`) で設定されています。</span><span class="sxs-lookup"><span data-stu-id="e9a15-168">The SDK is set  at the top of the project file (*.csproj*), `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span></span> <span data-ttu-id="e9a15-169">SDK によって *web.config* ファイルが変換されないようにするため、`true` に設定した **\<IsTransformWebConfigDisabled>** プロパティをプロジェクト ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-169">To prevent the SDK from transforming the *web.config* file, add the **\<IsTransformWebConfigDisabled>** property to the project file with a setting of `true`:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="e9a15-170">プロジェクトに *web.config* ファイルが存在せず、*dotnet publish* または Visual Studio の公開機能を使用して発行する場合、ファイルは[発行された出力](xref:hosting/directory-structure)内に自動的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-170">If you don't have a *web.config* file in the project when you publish with *dotnet publish* or with Visual Studio publish, the file is created for you in [published output](xref:hosting/directory-structure).</span></span> <span data-ttu-id="e9a15-171">プロジェクトに *web.config* ファイルが含まれている場合、そのファイルは [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)を構成するための正しい *processPath* と *arguments* を使用して変換され、発行された出力に移行されます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-171">If you have a *web.config* file in your project, it's transformed with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to published output.</span></span> <span data-ttu-id="e9a15-172">ファイルに含めた IIS 構成の設定は、変換後も変わりません。</span><span class="sxs-lookup"><span data-stu-id="e9a15-172">The transformation doesn't touch IIS configuration settings that you've included in the file.</span></span>

## <a name="create-the-iis-website"></a><span data-ttu-id="e9a15-173">IIS Web サイトを作成する</span><span class="sxs-lookup"><span data-stu-id="e9a15-173">Create the IIS Website</span></span>

1. <span data-ttu-id="e9a15-174">[ディレクトリ構造](xref:hosting/directory-structure)に関するページで説明されている、アプリの公開フォルダーとファイルを格納するためのフォルダーを、ターゲットの IIS システムで作成します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-174">On the target IIS system, create a folder to contain the app's published folders and files, which are described in [Directory Structure](xref:hosting/directory-structure).</span></span>

2. <span data-ttu-id="e9a15-175">作成したフォルダー内に、stdout ログを保存するための *logs* フォルダーを作成します (起動問題の解決のためにログ作成を有効にする場合)。</span><span class="sxs-lookup"><span data-stu-id="e9a15-175">Within the folder you created, create a *logs* folder to hold stdout logs (if you plan to enable logging to troubleshoot start-up issues).</span></span> <span data-ttu-id="e9a15-176">ペイロードに *logs* フォルダーを含めてアプリケーションを配置する場合は、このステップをスキップすることができます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-176">If you plan to deploy your application with a *logs* folder in the payload, you may skip this step.</span></span> <span data-ttu-id="e9a15-177">[フォルダーの自動作成には未解決の問題](https://github.com/aspnet/AspNetCoreModule/issues/30)があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-177">There's an [open issue to create the folder automatically](https://github.com/aspnet/AspNetCoreModule/issues/30).</span></span> <span data-ttu-id="e9a15-178">MSBuild で *log* フォルダーを自動作成するには、プロジェクト ファイルに次の `Target` を追加します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-178">If you would like MSBuild to create the *log* folder for you, add the following `Target` to your project file:</span></span>

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="AfterPublish">
     <MakeDir Directories="$(PublishDir)logs" Condition="!Exists('$(PublishDir)logs')" />
     <MakeDir Directories="$(PublishUrl)logs" Condition="!Exists('$(PublishUrl)logs')" />
   </Target>
   ```

3. <span data-ttu-id="e9a15-179">**IIS マネージャー**で新しい Web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-179">In **IIS Manager**, create a new website.</span></span> <span data-ttu-id="e9a15-180">**[サイト名]** を指定し、**[物理パス]** には作成したアプリの配置フォルダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-180">Provide a **Site name** and set the **Physical path** to the app's deployment folder that you created.</span></span> <span data-ttu-id="e9a15-181">**[バインド]** の構成を指定して Web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-181">Provide the **Binding** configuration and create the website.</span></span>

4. <span data-ttu-id="e9a15-182">アプリケーション プールを **[マネージ コードなし]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-182">Set the application pool to **No Managed Code**.</span></span> <span data-ttu-id="e9a15-183">ASP.NET Core は、別個のプロセスで実行され、ランタイムを管理します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-183">ASP.NET Core runs in a separate process and manages the runtime.</span></span>

5. <span data-ttu-id="e9a15-184">**[Web サイトの追加]** ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-184">Open the **Add Website** window.</span></span>

   ![サイトのコンテキスト メニューで [Web サイトの追加] をクリックします。](iis/_static/add-website-context-menu-ws2016.png)

6. <span data-ttu-id="e9a15-186">Web サイトを構成します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-186">Configure the website.</span></span>

   ![[Web サイトの追加] のステップでサイト名、物理パス、ホスト名を指定します。](iis/_static/add-website-ws2016.png)

7. <span data-ttu-id="e9a15-188">**[アプリケーション プール]** パネルで、**[アプリケーション プールの編集]** ウィンドウを開きます。そのためには、Web サイトのアプリ プール上で右クリックし、ポップアップ メニューから **[基本設定]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-188">In the **Application Pools** panel, open the **Edit Application Pool** window by right-clicking on the website's app pool and selecting **Basic Settings...** from the popup menu.</span></span>

   ![アプリケーション プールのコンテキスト メニューから [基本設定] を選択します。](iis/_static/apppools-basic-settings-ws2016.png)

8. <span data-ttu-id="e9a15-190">**[.NET CLR バージョン]** を **[マネージ コードなし]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-190">Set the **.NET CLR version** to **No Managed Code**.</span></span>

   ![.NET CLR バージョンとして [マネージ コードなし] を設定します。](iis/_static/edit-apppool-ws2016.png)
     
    <span data-ttu-id="e9a15-192">注: **[.NET CLR バージョン]** の **[マネージ コードなし]** への設定は、任意です。</span><span class="sxs-lookup"><span data-stu-id="e9a15-192">Note: Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span> <span data-ttu-id="e9a15-193">ASP.NET Core を使用するためにデスクトップ CLR を読み込む必要はありません。</span><span class="sxs-lookup"><span data-stu-id="e9a15-193">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span>

9. <span data-ttu-id="e9a15-194">プロセス モデル ID に適切なアクセス許可があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-194">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="e9a15-195">アプリ プールの既定の ID (**[プロセス モデル]** > **[ID]**) を **ApplicationPoolIdentity** から別の ID に変更した場合は、アプリのフォルダー、データベース、その他の必要なリソースにアクセスするために要求されるアクセス許可が新しい ID に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-195">If you change the default identity of the app pool (**Process Model** > **Identity**) from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span>
   
## <a name="deploy-the-application"></a><span data-ttu-id="e9a15-196">アプリケーションを展開する</span><span class="sxs-lookup"><span data-stu-id="e9a15-196">Deploy the application</span></span>
<span data-ttu-id="e9a15-197">ターゲット IIS システム上に作成したフォルダーにアプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-197">Deploy the application to the folder you created on the target IIS system.</span></span> <span data-ttu-id="e9a15-198">[Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)は、推奨される展開のメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="e9a15-198">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span> <span data-ttu-id="e9a15-199">Web 配置に代わる方法は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="e9a15-199">Alternatives to Web Deploy are listed below.</span></span>

<span data-ttu-id="e9a15-200">配置用に発行したアプリが実行されていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-200">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="e9a15-201">アプリが実行中は、*publish* フォルダー内のファイルがロックされます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-201">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="e9a15-202">ロックされているファイルはコピーできないため、配置は行われません。</span><span class="sxs-lookup"><span data-stu-id="e9a15-202">Deployment can't occur because locked files can't be copied.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="e9a15-203">Visual Studio からのアプリの展開</span><span class="sxs-lookup"><span data-stu-id="e9a15-203">Web Deploy with Visual Studio</span></span>
<span data-ttu-id="e9a15-204">Web 配置に使用する発行プロファイルの作成方法については、「[Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps](xref:publishing/web-publishing-vs#publish-profiles)」 (ASP.NET Core アプリを展開するために Visual Studio と MSBuild 用の発行プロファイルを作成する) のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-204">See [Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps](xref:publishing/web-publishing-vs#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="e9a15-205">ホスティング プロバイダーから発行プロファイルまたは発行プロファイルを作成するためのサポートが提供されている場合は、そのプロファイルをダウンロードし、Visual Studio の **[発行]** ダイアログを使用してインポートします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-205">If your hosting provider supplies a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![[発行] ダイアログ ページ](iis/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="e9a15-207">Visual Studio 外部での Web 配置</span><span class="sxs-lookup"><span data-stu-id="e9a15-207">Web Deploy outside of Visual Studio</span></span>
<span data-ttu-id="e9a15-208">Visual Studio の外部で、コマンド ラインから [Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-208">You can also use [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) outside of Visual Studio from the command line.</span></span> <span data-ttu-id="e9a15-209">詳細については、[Web 配置ツール](/iis/publish/using-web-deploy/use-the-web-deployment-tool)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-209">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="e9a15-210">Web 配置の代替手段</span><span class="sxs-lookup"><span data-stu-id="e9a15-210">Alternatives to Web Deploy</span></span>
<span data-ttu-id="e9a15-211">Web 配置を使用しないか、Visual Studio を使用しない場合は、複数の方法のいずれかを使用して、Xcopy、Robocopy、PowerShell などのホスティング システムにアプリケーションを移行できます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-211">If you don't wish to use Web Deploy or are not using Visual Studio, you may use any of several methods to move the application to the hosting system, such as Xcopy, Robocopy, or PowerShell.</span></span> <span data-ttu-id="e9a15-212">Visual Studio ユーザーは、[発行サンプル](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md)を利用できます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-212">Visual Studio users may use the [Publish Samples](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="e9a15-213">Web サイトを閲覧する</span><span class="sxs-lookup"><span data-stu-id="e9a15-213">Browse the website</span></span>
![Microsoft Edge ブラウザーに IIS のスタートアップ ページが読み込まれています。](iis/_static/browsewebsite.png)
   
> [!WARNING]
> <span data-ttu-id="e9a15-215">.NET Core アプリは、IIS と Kestrel サーバー間のリバース プロキシによってホストされます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-215">.NET Core apps are hosted via a reverse-proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="e9a15-216">リバース プロキシを作成するためには、展開されるアプリケーションのコンテンツ ルート パス (通常は、アプリ ベース パス) に *web.config* ファイルが存在する必要があります。これは IIS に対して指定される Web サイトの物理パスです。</span><span class="sxs-lookup"><span data-stu-id="e9a15-216">In order to create the reverse-proxy, the *web.config* file must be present at the content root path (typically the app base path) of the deployed application, which is the website physical path provided to IIS.</span></span> <span data-ttu-id="e9a15-217">アプリの物理パスには、*my_application.runtimeconfig.json*、*my_application.xml* (XML ドキュメントのコメント)、*my_application.deps.json*などのサブフォルダーを含め、機密性の高いファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-217">Sensitive files exist on the app's physical path, including subfolders, such as *my_application.runtimeconfig.json*, *my_application.xml* (XML Documentation comments), and *my_application.deps.json*.</span></span> <span data-ttu-id="e9a15-218">Kestrel に対するリバース プロキシを作成するためには *web.config* ファイルが必要であり、そのため IIS は機密性の高いこれらのファイルやその他のファイルを処理できません。</span><span class="sxs-lookup"><span data-stu-id="e9a15-218">The *web.config* file is required to create the reverse proxy to Kestrel, which prevents IIS from serving these and other sensitive files.</span></span> <span data-ttu-id="e9a15-219">**したがって、*web.config* ファイルを誤って名前変更したり展開から削除したりしないようにすることが重要です。**</span><span class="sxs-lookup"><span data-stu-id="e9a15-219">**Therefore, it's important that the *web.config* file isn't accidently renamed or removed from the deployment.**</span></span>

## <a name="data-protection"></a><span data-ttu-id="e9a15-220">データの保護</span><span class="sxs-lookup"><span data-stu-id="e9a15-220">Data protection</span></span>

<span data-ttu-id="e9a15-221">ASP.NET Core アプリケーションは、次の場合にキーリングをメモリに格納します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-221">An ASP.NET Core application stores the keyring in memory under the following conditions:</span></span>

* <span data-ttu-id="e9a15-222">Web サイトが IIS の背後でホストされている。</span><span class="sxs-lookup"><span data-stu-id="e9a15-222">A website is hosted behind IIS.</span></span>
* <span data-ttu-id="e9a15-223">データ保護スタックが、永続的なストアにキーリングを格納するように構成されていない。</span><span class="sxs-lookup"><span data-stu-id="e9a15-223">The Data Protection stack hasn't been configured to store the keyring in a persistent store.</span></span>

<span data-ttu-id="e9a15-224">キーリングがメモリに格納されている場合、アプリを再起動すると次のことが行われます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-224">If the keyring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="e9a15-225">すべてのフォーム認証トークンは無効になります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-225">All forms authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="e9a15-226">ユーザーは、次回の要求時に再ログインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-226">Users are required to login again on their next request.</span></span> 
* <span data-ttu-id="e9a15-227">キーリングで保護されていたデータは保護されなくなります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-227">Any data you protected with the keyring are no longer protected.</span></span>

> [!WARNING]
> <span data-ttu-id="e9a15-228">データ保護は、認証で使用されるものを含め、いくつかの ASP.NET ミドルウェアによって使用されます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-228">Data Protection is used by several ASP.NET middlewares, including those used in authentication.</span></span> <span data-ttu-id="e9a15-229">独自のコードからデータ保護 API を呼び出さない場合でも、配置スクリプトを使用して、またはコード内にデータ保護を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-229">Even if you don't call Data Protection APIs from your own code, you should configure Data Protection with a deployment script or in your code.</span></span> <span data-ttu-id="e9a15-230">データ保護を構成しない場合、既定でキーはメモリ内に保持され、アプリが再起動すると破棄されます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-230">If you don't configure data protection, by default the keys are held in memory and discarded when your app restarts.</span></span> <span data-ttu-id="e9a15-231">再起動すると、Cookie 認証ミドルウェアによって作成された Cookie は無効になり、ユーザーは再度ログインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-231">Restarting invalidates cookies written by the cookie authentication middleware and users must login again.</span></span>

<span data-ttu-id="e9a15-232">IIS でのデータ保護を構成するには、次のいずれかの方法を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-232">To configure Data Protection under IIS, you must use one of the following approaches:</span></span>

* <span data-ttu-id="e9a15-233">[powershell スクリプト](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1)を実行し、適切なレジストリ エントリ (たとえば `.\Provision-AutoGenKeys.ps1 DefaultAppPool`) を作成します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-233">Run a [powershell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) to create suitable registry entries (For example, `.\Provision-AutoGenKeys.ps1 DefaultAppPool`).</span></span> <span data-ttu-id="e9a15-234">これによりキーはレジストリに格納され、DPAPI を使用して、コンピューター全体に適用するキーによって保護されます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-234">This stores keys in the registry, protected using DPAPI with a machine-wide key.</span></span>
* <span data-ttu-id="e9a15-235">ユーザー プロファイルを読み込むための IIS アプリケーション プールを構成します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-235">Configure the IIS Application Pool to load the user profile.</span></span> <span data-ttu-id="e9a15-236">この設定は、アプリケーション プールの **[詳細設定]** の **[プロセス モデル]** セクションにあります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-236">This setting is in the **Process Model** section under the **Advanced Settings** for the application pool.</span></span> <span data-ttu-id="e9a15-237">**[ユーザー プロファイルの読み込み]** を `True` に設定します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-237">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="e9a15-238">これによりキーはユーザー プロファイル ディレクトリに格納され、DPAPI を使用して、アプリ プールで使うユーザー アカウントに固有のキーによって保護されます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-238">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used for the app pool.</span></span>
* <span data-ttu-id="e9a15-239">[ファイル システムをキー リング ストアとして使用](xref:security/data-protection/configuration/overview)するようにアプリ コードを調整します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-239">Adjust your app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="e9a15-240">X509 証明書を使用してキー リングを保護し、それが信頼された証明書であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-240">Use an X509 certificate to protect the keyring and ensure it's a trusted certificate.</span></span> <span data-ttu-id="e9a15-241">証明書が自己署名証明書である場合は、信頼されたルート ストアに配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-241">If it's a self signed certificate, you must place it in the Trusted Root store.</span></span>

<span data-ttu-id="e9a15-242">Web ファームで IIS を使用する場合:</span><span class="sxs-lookup"><span data-stu-id="e9a15-242">When using IIS in a web farm:</span></span>

* <span data-ttu-id="e9a15-243">すべてのコンピューターがアクセスできるファイル共有を使用します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-243">Use a file share that all machines can access.</span></span>
* <span data-ttu-id="e9a15-244">X509 証明書を各コンピューターに配置します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-244">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="e9a15-245">[コード内にデータ保護](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview)を構成します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-245">Configure [data protection in code](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview).</span></span>

### <a name="1-create-a-data-protection-registry-hive"></a><span data-ttu-id="e9a15-246">1.データ保護のレジストリ ハイブを作成する</span><span class="sxs-lookup"><span data-stu-id="e9a15-246">1. Create a Data Protection Registry Hive</span></span>

<span data-ttu-id="e9a15-247">ASP.NET アプリケーションで使用されるデータ保護キーは、アプリケーション外部のレジストリ ハイブに格納されます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-247">Data Protection keys used by ASP.NET applications are stored in registry hives external to the applications.</span></span> <span data-ttu-id="e9a15-248">特定のアプリケーションのキーを保持するには、アプリケーションのアプリ プール用のレジストリ ハイブを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-248">To persist the keys for a given application, you must create a registry hive for the app's application pool.</span></span>

<span data-ttu-id="e9a15-249">IIS のスタンドアロン インストールの場合、ASP.NET Core アプリで使用するアプリ プールごとに、[データ保護の PowerShell スクリプト Provision-AutoGenKeys.ps1](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) を使用できます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-249">For standalone IIS installations, you may use the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="e9a15-250">このスクリプトは、HKLM レジストリに特別なレジストリ キーを作成します。そのキーは、ワーカー プロセス アカウントに対してのみ、ACL に追加されます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-250">This script creates a special registry key in the HKLM registry that is ACLed only to the worker process account.</span></span> <span data-ttu-id="e9a15-251">キーは DPAPI を使用して保存時に暗号化されます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-251">Keys are encrypted at rest using DPAPI.</span></span>

<span data-ttu-id="e9a15-252">Web ファームのシナリオでは、UNC パスを使用してデータ保護キー リングを格納するようにアプリを構成できます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-252">In web farm scenarios, an app can be configured to use a UNC path to store its data protection keyring.</span></span> <span data-ttu-id="e9a15-253">既定では、データ保護キーは暗号化されません。</span><span class="sxs-lookup"><span data-stu-id="e9a15-253">By default, the data protection keys are not encrypted.</span></span> <span data-ttu-id="e9a15-254">このような共有のファイル アクセス許可は、アプリを実行する Windows アカウントに限定されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-254">You should ensure that the file permissions for such a share are limited to the Windows account the app runs as.</span></span> <span data-ttu-id="e9a15-255">また、X509 証明書を使用して保存時のキーを保護するように選択することもできます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-255">In addition, you may choose to protect keys at rest using an X509 certificate.</span></span> <span data-ttu-id="e9a15-256">ユーザーが証明書をアップデートできるメカニズムを検討している場合、ユーザーの信頼できる証明書ストアに証明書を配置し、ユーザーのアプリが実行されるすべてのコンピューターで証明書を利用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-256">You may wish to consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="e9a15-257">詳細については、「[データ保護の構成](xref:security/data-protection/configuration/overview)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-257">See [Configuring Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

### <a name="2-configure-the-iis-application-pool-to-load-the-user-profile"></a><span data-ttu-id="e9a15-258">2.ユーザー プロファイルを読み込むように IIS アプリケーション プールを構成する</span><span class="sxs-lookup"><span data-stu-id="e9a15-258">2. Configure the IIS Application Pool to load the user profile</span></span>

<span data-ttu-id="e9a15-259">この設定は、アプリ プールの **[詳細設定]** の **[プロセス モデル]** セクションにあります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-259">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="e9a15-260">[ユーザー プロファイルの読み込み] を `True` に設定します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-260">Set Load User Profile to `True`.</span></span> <span data-ttu-id="e9a15-261">これによりキーはユーザー プロファイル ディレクトリに格納され、DPAPI を使用して、アプリ プールで使うユーザー アカウントに固有のキーによって保護されます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-261">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used for the app pool.</span></span>

### <a name="3-machine-wide-policy-for-data-protection"></a><span data-ttu-id="e9a15-262">3.コンピューター全体に適用するデータ保護ポリシー</span><span class="sxs-lookup"><span data-stu-id="e9a15-262">3. Machine-wide policy for data protection</span></span>

<span data-ttu-id="e9a15-263">データ保護システムでは、データ保護 API を使用するすべてのアプリに対して、[コンピューター全体に適用する既定のポリシー](xref:security/data-protection/configuration/machine-wide-policy)を設定するためのサポートは限定的です。</span><span class="sxs-lookup"><span data-stu-id="e9a15-263">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="e9a15-264">詳細については、[データ保護](xref:security/data-protection/index)に関するドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-264">See the [data protection](xref:security/data-protection/index) documentation for more details.</span></span>

## <a name="configuration-of-sub-applications"></a><span data-ttu-id="e9a15-265">サブアプリケーションの構成</span><span class="sxs-lookup"><span data-stu-id="e9a15-265">Configuration of sub-applications</span></span>

<span data-ttu-id="e9a15-266">ルート アプリケーションの下に追加したサブアプリケーションに、ハンドラーとして ASP.NET Core モジュールを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="e9a15-266">Sub applications added under the root application shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="e9a15-267">サブアプリケーションの *web.config* ファイルにこのモジュールをハンドラーとして追加した場合、サブアプリを閲覧しようとすると、エラーのある構成ファイルを参照する 500.19 (内部サーバー エラー) が返されます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-267">If you add the module as a handler in a sub-application's *web.config* file, you receive a 500.19 (Internal Server Error) referencing the faulty config file when you attempt to browse the sub-app.</span></span> <span data-ttu-id="e9a15-268">次の例は、ASP.NET Core サブアプリの発行済み *web.config* ファイルの内容を示しています。</span><span class="sxs-lookup"><span data-stu-id="e9a15-268">The following example shows the contents of a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="e9a15-269">ASP.NET Core アプリの下に ASP.NET Core 以外のサブアプリをホストする場合は、サブアプリの *web.config* ファイルにある継承されたハンドラーを明示的に削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-269">If you intend to host a non-ASP.NET Core sub-app underneath an ASP.NET Core app, you must explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="e9a15-270">*web.config* ファイルを使用して ASP.NET Core モジュールを構成する方法の詳細については、「[Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module)」 (ASP.NET Core モジュールの概要) のトピックと「[ASP.NET Core モジュール構成リファレンス](xref:hosting/aspnet-core-module)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-270">For more information on configuring the ASP.NET Core Module with the *web.config* file, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="e9a15-271">web.config による IIS の構成</span><span class="sxs-lookup"><span data-stu-id="e9a15-271">Configuration of IIS with web.config</span></span>

<span data-ttu-id="e9a15-272">リバース プロキシ構成に適用される IIS 機能については、*web.config*の `<system.webServer>` セクションも、IIS の構成に影響します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-272">IIS configuration is still influenced by the `<system.webServer>` section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="e9a15-273">たとえば、IIS が動的な圧縮を使用するようにシステム レベルで構成されている場合でも、アプリの *web.config* ファイルで `<urlCompression>` 要素を使用して、アプリに対してその設定を無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-273">For example, you may have IIS configured at the system level to use dynamic compression, but you could disable that setting for an app with the `<urlCompression>` element in the app's *web.config* file.</span></span> <span data-ttu-id="e9a15-274">詳細については、[`<system.webServer>` の構成リファレンス](https://docs.microsoft.com/iis/configuration/system.webServer/)、「[ASP.NET Core モジュール構成の参照](xref:hosting/aspnet-core-module)」および「[ASP.NET Core で IIS のモジュールの使用](xref:hosting/iis-modules)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-274">For more information, see the [configuration reference for `<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:hosting/aspnet-core-module) and [Using IIS Modules with ASP.NET Core](xref:hosting/iis-modules).</span></span> <span data-ttu-id="e9a15-275">分離されたアプリケーション プール (IIS 10.0 以降でサポートされています) で実行する個別アプリに対して環境変数を設定する必要がある場合は、IIS のリファレンス ドキュメントで、[環境変数\<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) のトピックにある *AppCmd.exe コマンド*のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-275">If you need to set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="e9a15-276">web.config の構成のセクション</span><span class="sxs-lookup"><span data-stu-id="e9a15-276">Configuration sections of web.config</span></span>

<span data-ttu-id="e9a15-277">`<system.web>`、`<appSettings>`、`<connectionStrings>`、`<location>` の各要素を使用して *web.config* で構成される .NET Framework アプリケーションとは異なり、ASP.NET Core アプリは、その他の構成プロバイダーを使用して構成されます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-277">Unlike .NET Framework applications that are configured with the `<system.web>`, `<appSettings>`, `<connectionStrings>`, and `<location>` elements in *web.config*, ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="e9a15-278">詳細については、[構成](xref:fundamentals/configuration)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-278">For more information, see [Configuration](xref:fundamentals/configuration).</span></span>

## <a name="application-pools"></a><span data-ttu-id="e9a15-279">アプリケーション プール</span><span class="sxs-lookup"><span data-stu-id="e9a15-279">Application Pools</span></span>

<span data-ttu-id="e9a15-280">1 つのシステムで複数の Web サイトをホストする場合は、アプリをそれぞれ専用のアプリケーション プールで実行して、アプリを相互に分離する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-280">When hosting multiple websites on a single system, you should isolate the apps from each other by running each app in its own application pool.</span></span> <span data-ttu-id="e9a15-281">これは、IIS の **[Web サイトの追加]** ダイアログの既定の設定です。</span><span class="sxs-lookup"><span data-stu-id="e9a15-281">The IIS **Add Website** dialog defaults to this behavior.</span></span> <span data-ttu-id="e9a15-282">**[サイト名]** を指定すると、入力したテキストが自動的に **[アプリケーション プール]** テキストボックスに設定されます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-282">When you provide a **Site name**, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="e9a15-283">Web サイトを追加するときに、そのサイト名を使用して新しいアプリケーション プールが作成されます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-283">A new application pool is created using the site name when you add the website.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="e9a15-284">アプリケーション プール ID</span><span class="sxs-lookup"><span data-stu-id="e9a15-284">Application Pool Identity</span></span>

<span data-ttu-id="e9a15-285">アプリケーション プール ID アカウントを使用すると、ドメインやローカル アカウントを作成して管理する必要なく、一意のアカウントでアプリケーションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-285">An application pool identity account allows you to run an application under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="e9a15-286">IIS 8.0 以降の IIS 管理者ワーカー プロセス (WAS) は、新しいアプリケーション プールの名前で仮想アカウントを作成し、既定によってアプリ プールのワーカー プロセスをこのアカウントで実行します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-286">On IIS 8.0+, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new application pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="e9a15-287">IIS 管理コンソールにあるアプリ プールの **[詳細設定]** で、次の画像に示すように、**ID** が **ApplicationPoolIdentity** を使用するように設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-287">In the IIS Management Console under **Advanced Settings** for your app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity** as shown in the image below.</span></span>

![アプリケーション プールの [詳細設定] ダイアログ](iis/_static/apppool-identity.png)

<span data-ttu-id="e9a15-289">IIS 管理プロセスは、Windows セキュリティ システムでのアプリ プールの名前を使用して、セキュリティで保護された識別子を作成します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-289">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="e9a15-290">この ID を使用してリソースを保護することができます。ただし、この ID は実際のユーザー アカウントではなく、Windows ユーザーの管理コンソールに表示されません。</span><span class="sxs-lookup"><span data-stu-id="e9a15-290">Resources can be secured by using this identity; however, this identity isn't a real user account and won't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="e9a15-291">アプリに対する IIS ワーカー プロセスのアクセス許可を昇格させる必要がある場合は、アプリケーションを含むディレクトリのアクセス制御リスト (ACL) を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-291">If you need to grant the IIS worker process elevated access to your app, you must modify the Access Control List (ACL) for the directory containing your application.</span></span>

1. <span data-ttu-id="e9a15-292">エクスプローラーを開き、そのディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-292">Open Windows Explorer and navigate to the directory.</span></span>

2. <span data-ttu-id="e9a15-293">ディレクトリ上で右クリックし、**[プロパティ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-293">Right click on the directory and click **Properties**.</span></span>

3. <span data-ttu-id="e9a15-294">**[セキュリティ]** タブの **[編集]** ボタンをクリックし、**[追加]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-294">Under the **Security** tab, click the **Edit** button and then the **Add** button.</span></span>

4. <span data-ttu-id="e9a15-295">**[場所]** ボタンをクリックし、使用しているシステムを選択します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-295">Click the **Locations** button and make sure you select your system.</span></span>

5. <span data-ttu-id="e9a15-296">**[選択するオブジェクト名を入力してください]** テキストボックスに「**IIS AppPool\DefaultAppPool**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-296">Enter **IIS AppPool\DefaultAppPool** in **Enter the object names to select** textbox.</span></span>

  ![アプリケーション フォルダーの [ユーザーまたはグループの選択] ダイアログ](iis/_static/select-users-or-groups-1.png)

6. <span data-ttu-id="e9a15-298">**[名前の確認]** ボタンをクリックし、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-298">Click the **Check Names** button and then click **OK**.</span></span>

  ![アプリケーション フォルダーの [ユーザーまたはグループの選択] ダイアログ](iis/_static/select-users-or-groups-2.png)

<span data-ttu-id="e9a15-300">この操作は、コマンド プロンプトから **ICACLS** ツールを使用して行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-300">You can also do this via a command prompt using **ICACLS** tool:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="troubleshooting-tips"></a><span data-ttu-id="e9a15-301">トラブルシューティングのヒント</span><span class="sxs-lookup"><span data-stu-id="e9a15-301">Troubleshooting tips</span></span>

<span data-ttu-id="e9a15-302">IIS 展開の問題を診断する方法:</span><span class="sxs-lookup"><span data-stu-id="e9a15-302">To diagnose problems with IIS deployments:</span></span>

* <span data-ttu-id="e9a15-303">ブラウザーの出力を調べます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-303">Study browser output.</span></span>
* <span data-ttu-id="e9a15-304">**イベント ビューアー**でシステムの**アプリケーション** ログを調べます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-304">Examine the system's **Application** log through **Event Viewer**.</span></span>
* <span data-ttu-id="e9a15-305">`stdout` ログを有効にします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-305">Enable `stdout` logging.</span></span> <span data-ttu-id="e9a15-306">**ASP.NET Core モジュール**のログは、*web.config*で `<aspNetCore>`要素の *stdoutLogFile* 属性に指定したパスにあります。この属性の値で指定されたパスにあるすべてのフォルダーが、展開環境に存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-306">The **ASP.NET Core Module** log is found on the path provided in the *stdoutLogFile* attribute of the `<aspNetCore>` element in *web.config*. Any folders on the path provided in the attribute value must exist in the deployment.</span></span> <span data-ttu-id="e9a15-307">また、*stdoutLogEnabled="true"*を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-307">You must also set *stdoutLogEnabled="true"*.</span></span> <span data-ttu-id="e9a15-308">`Microsoft.NET.Sdk.Web` SDK を使用して *web.config* ファイルを作成するアプリでは、既定により *stdoutLogEnabled* が *false* に設定されるため、`stdout` のログ記録を有効にするには、*web.config* ファイルを手動で作成するか、ファイルを編集する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-308">Apps that use the the `Microsoft.NET.Sdk.Web` SDK to create the *web.config* file default the *stdoutLogEnabled* setting to *false*, so you must manually provide the *web.config* file or modify the file in order to enable `stdout` logging.</span></span>

<span data-ttu-id="e9a15-309">一般的なエラーのいくつかは、*startupTimeLimit* (既定値: 120 秒) と *startupRetryCount* (既定値: 2) が経過するまで、ブラウザー、アプリケーション ログ、ASP.NET Core モジュールのログに表示されません。</span><span class="sxs-lookup"><span data-stu-id="e9a15-309">Several of the common errors don't appear in the browser, Application Log, and ASP.NET Core Module Log until the module *startupTimeLimit* (default: 120 seconds) and *startupRetryCount* (default: 2) have passed.</span></span> <span data-ttu-id="e9a15-310">したがって、モジュールがアプリのプロセスを開始できなかったと判断する前に、完全に 6 分が経過するまで待ってください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-310">Therefore, wait a full six minutes before deducing that the module has failed to start a process for the app.</span></span>

<span data-ttu-id="e9a15-311">アプリが正常に動作しているかどうかを決定する 1 つの簡単な方法は、アプリを Kestrel で直接実行することです。</span><span class="sxs-lookup"><span data-stu-id="e9a15-311">One quick way to determine if the app is working properly is to run the app directly on Kestrel.</span></span> <span data-ttu-id="e9a15-312">アプリがフレームワークに依存する展開 (FDD) として発行された場合は、展開フォルダー (アプリの IIS 物理パス) の `dotnet my_application.dll` を実行します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-312">If the app was published as a framework-dependent deployment (FDD), execute `dotnet my_application.dll` in the deployment folder, which is the IIS physical path to the app.</span></span> <span data-ttu-id="e9a15-313">アプリが自己完結型の展開 (SCD) として発行された場合は、展開フォルダーにあるアプリケーションの実行可能ファイル (`my_application.exe`) をコマンド プロンプトから直接実行します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-313">If the app was published as a self-contained deployment (SCD), run the app's executable directly from a command prompt, `my_application.exe`, in the deployment folder.</span></span> <span data-ttu-id="e9a15-314">Kestrel が既定のポート 5000 でリッスンしている場合は、`http://localhost:5000/` でアプリを閲覧できます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-314">If Kestrel is listening on default port 5000, you should be able to browse the app at `http://localhost:5000/`.</span></span> <span data-ttu-id="e9a15-315">アプリが通常は Kestrel エンドポイント アドレスで応答する場合、問題は IIS、ASP.NET Core モジュール、Kestrel の構成に関連している可能性が高く、アプリ自体に関連する可能性は低くなります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-315">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the IIS-ASP.NET Core Module-Kestrel configuration and less likely within the app itself.</span></span>

<span data-ttu-id="e9a15-316">Kestrel サーバーに対する IIS リバース プロキシが正常に動作しているかどうかを確認する 1 つの方法は、[静的ファイル ミドルウェア](xref:fundamentals/static-files)を使用して、*wwwroot* にあるアプリの静的ファイルのスタイルシート、スクリプト、または画像に対する簡単な静的ファイル要求を実行することです。</span><span class="sxs-lookup"><span data-stu-id="e9a15-316">One way to determine if the IIS reverse proxy to the Kestrel server is working properly is to perform a simple static file request for a stylesheet, script, or image from the app's static files in *wwwroot* using [Static File middleware](xref:fundamentals/static-files).</span></span> <span data-ttu-id="e9a15-317">アプリの静的ファイルを取得できたにもかかわらず、MVC ビューおよびその他のエンドポイントでエラーになる場合は、問題が IIS、ASP.NET Core モジュール、Kestrel 構成に関連している可能性は低く、アプリ自体に関連している可能性が高くなります (たとえば、MVC ルーティングや内部サーバー エラー 500)。</span><span class="sxs-lookup"><span data-stu-id="e9a15-317">If the app can serve static files but MVC Views and other endpoints are failing, the problem is less likely related to the IIS-ASP.NET Core Module-Kestrel configuration and more likely within the app itself (for example, MVC routing or 500 Internal Server Error).</span></span>

<span data-ttu-id="e9a15-318">Kestrel が IIS の背後で正常に開始される一方、ローカルで正常に実行できたアプリをシステムで実行できない場合は、*web.config* に、`ASPNETCORE_ENVIRONMENT` を `Development` に設定する環境変数を一時的に追加することができます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-318">When Kestrel starts normally behind IIS but the app won't run on the system after successfully running locally, you can temporarily add an environment variable to *web.config* to set the `ASPNETCORE_ENVIRONMENT` to `Development`.</span></span> <span data-ttu-id="e9a15-319">これにより、アプリの起動時に環境をオーバーライドしない限り、アプリをシステムで実行するときに[開発者例外ページ](xref:fundamentals/error-handling)を表示できます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-319">As long as you don't override the environment in app startup, this allows the [developer exception page](xref:fundamentals/error-handling) to appear when the app is run on the system.</span></span> <span data-ttu-id="e9a15-320">`ASPNETCORE_ENVIRONMENT` の環境変数をこのように設定する方法は、インターネットに公開されないステージング システムやテスト用システムについてのみ推奨される方法です。</span><span class="sxs-lookup"><span data-stu-id="e9a15-320">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` in this way is only recommended for staging/testing systems that aren't exposed to the Internet.</span></span> <span data-ttu-id="e9a15-321">目的が完了したら、必ず *web.config* ファイルから環境変数を削除してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-321">Be sure you remove the environment variable from the *web.config* file when finished.</span></span> <span data-ttu-id="e9a15-322">*web.config* を使用してリバース プロキシ用の環境変数を設定する方法の詳細については、「[environmentVariables child element of aspNetCore](xref:hosting/aspnet-core-module#setting-environment-variables)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-322">For information on setting environment variables via *web.config* for the reverse proxy, see [environmentVariables child element of aspNetCore](xref:hosting/aspnet-core-module#setting-environment-variables).</span></span>

<span data-ttu-id="e9a15-323">ほとんどの場合、アプリケーションのログ記録を有効にすると、アプリまたはリバース プロキシに関する問題のトラブルシューティングに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-323">In most cases, enabling application logging assists in troubleshooting problems with the app or the reverse proxy.</span></span> <span data-ttu-id="e9a15-324">詳細については、[ログ記録](xref:fundamentals/logging/index)に関する説明を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-324">See [Logging](xref:fundamentals/logging/index) for more information.</span></span>

<span data-ttu-id="e9a15-325">最後に紹介するトラブルシューティングのヒントは、開発用コンピューター上の .NET Core SDK またはアプリ内のパッケージ バージョンのいずれかをアップグレード後に実行が失敗するアプリに関するものです。</span><span class="sxs-lookup"><span data-stu-id="e9a15-325">Our last troubleshooting tip pertains to apps that fail to run after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="e9a15-326">場合によっては、パッケージに統一性がないと、メジャー アップグレード実行時にアプリが破壊されることがあります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-326">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="e9a15-327">このような問題の大部分は、次の方法で解決できます。まず、プロジェクトの `bin` フォルダーと `obj` フォルダーを削除し、`%UserProfile%\.nuget\packages\` と `%LocalAppData%\Nuget\v3-cache` にあるパッケージのキャッシュをクリアします。その後、プロジェクトを復元し、システムで以前の展開が完全に削除されたことを確認してから、アプリを再展開します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-327">You can fix most of these issues by deleting the `bin` and `obj` folders in the project, clearing package caches at `%UserProfile%\.nuget\packages\` and `%LocalAppData%\Nuget\v3-cache`, restoring the project, and confirming that your prior deployment on the system has been completely deleted prior to re-deploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="e9a15-328">パッケージのキャッシュをクリアする便利な方法として、[NuGet.org](https://www.nuget.org/) から *NuGet.exe* ツールを入手し、システム PATH に追加して、コマンド プロンプトから `nuget locals all -clear` を実行します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-328">A convenient way to clear package caches is to obtain the *NuGet.exe* tool from [NuGet.org](https://www.nuget.org/), add it to your system PATH, and execute `nuget locals all -clear` from a command prompt.</span></span> <span data-ttu-id="e9a15-329">*NuGet.exe* を入手せず、コマンド プロンプトから `dotnet nuget locals all --clear` コマンドを実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-329">You can also execute the `dotnet nuget locals all --clear` command from a command prompt without obtaining *NuGet.exe*.</span></span>

## <a name="common-errors"></a><span data-ttu-id="e9a15-330">一般的なエラー</span><span class="sxs-lookup"><span data-stu-id="e9a15-330">Common errors</span></span>

<span data-ttu-id="e9a15-331">次の一覧に含まれていないエラーもあります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-331">The following isn't a complete list of errors.</span></span> <span data-ttu-id="e9a15-332">この一覧にないエラーが発生した場合は、以下のコメント セクションに詳細なエラー メッセージを記録してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-332">Should you encounter an error not listed here, please leave a detailed error message in the comments section below.</span></span>

### <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="e9a15-333">インストーラーが VC++ 再頒布可能パッケージを取得できない</span><span class="sxs-lookup"><span data-stu-id="e9a15-333">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="e9a15-334">**インストーラー例外:** 0x80072efd または 0x80072f76 - 特定できないエラー</span><span class="sxs-lookup"><span data-stu-id="e9a15-334">**Installer Exception:** 0x80072efd or 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="e9a15-335">**インストーラー ログ例外&#8224;:** エラー 0x80072efd または 0x80072f76: Failed to execute EXE package (EXE パッケージを実行できませんでした)</span><span class="sxs-lookup"><span data-stu-id="e9a15-335">**Installer Log Exception&#8224;:** Error 0x80072efd or 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="e9a15-336">&#8224;ログの場所は C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log です。</span><span class="sxs-lookup"><span data-stu-id="e9a15-336">&#8224;The log is located at C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span></span>

<span data-ttu-id="e9a15-337">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e9a15-337">Troubleshooting:</span></span>

* <span data-ttu-id="e9a15-338">サーバー ホスティング バンドルのインストール時にインストーラーがインターネットにアクセスできない場合、インストーラーは *Microsoft Visual C++ 2015 再頒布可能パッケージ*を取得できず、この例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-338">If the system doesn't have Internet access while installing the server hosting bundle, this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="e9a15-339">インストーラーは [Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=53840)から入手できます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-339">You may obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="e9a15-340">インストーラーが失敗した場合、フレームワークに依存する展開 (FDD) をホストするために必要な .NET Core ランタイムを受け取れない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-340">If the installer fails, you may not receive the .NET Core runtime required to host a framework-dependent deployment (FDD).</span></span> <span data-ttu-id="e9a15-341">FDD をホストする場合は、[プログラムと機能] でランタイムがインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-341">If you plan to host an FDD, confirm that the runtime is installed in Programs &amp; Features.</span></span> <span data-ttu-id="e9a15-342">ランタイム インストーラーは [.NET ダウンロード](https://www.microsoft.com/net/download/core)のページから入手できます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-342">You may obtain a runtime installer from [.NET Downloads](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="e9a15-343">ランタイムのインストール後、システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-343">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

### <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="e9a15-344">OS のアップグレードによって 32 ビット ASP.NET Core モジュールが削除された</span><span class="sxs-lookup"><span data-stu-id="e9a15-344">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

* <span data-ttu-id="e9a15-345">**アプリケーション ログ:** モジュール DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** を読み込めませんでした。</span><span class="sxs-lookup"><span data-stu-id="e9a15-345">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="e9a15-346">このデータはエラーです。</span><span class="sxs-lookup"><span data-stu-id="e9a15-346">The data is the error.</span></span>

<span data-ttu-id="e9a15-347">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e9a15-347">Troubleshooting:</span></span>

* <span data-ttu-id="e9a15-348">**C:\Windows\SysWOW64\inetsrv** ディレクトリにある OS ファイルでないファイルは、OS アップグレード時に保持されません。</span><span class="sxs-lookup"><span data-stu-id="e9a15-348">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="e9a15-349">OS アップグレードより前に ASP.NET Core モジュールをインストールしていた場合、OS アップグレード後に 32 ビット モードで AppPool を実行しようとすると、この問題が発生します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-349">If you have the ASP.NET Core Module installed prior to an OS upgrade and then try to run any AppPool in 32-bit mode after an OS upgrade, you encounter this issue.</span></span> <span data-ttu-id="e9a15-350">OS アップグレード後に ASP.NET Core モジュールを修復してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-350">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="e9a15-351">「[.NET Core Windows Server ホスティング バンドルのインストール](#install-the-net-core-windows-server-hosting-bundle)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-351">See [Install the .NET Core Windows Server Hosting bundle](#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="e9a15-352">インストーラーを実行するときに **[修復]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-352">Select **Repair** when you run the installer.</span></span>

### <a name="platform-conflicts-with-rid"></a><span data-ttu-id="e9a15-353">プラットフォームが RID と競合している</span><span class="sxs-lookup"><span data-stu-id="e9a15-353">Platform conflicts with RID</span></span>

* <span data-ttu-id="e9a15-354">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="e9a15-354">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e9a15-355">**アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ', ErrorCode = '0x80004005 : ff. (物理ルート 'C:{PATH}\' のアプリケーション 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' は、コマンド ライン '"C:\{PATH}\my_application.{exe|dll}" ' でプロセスを開始できませんでした。ErrorCode = '0x80004005 : ff)</span><span class="sxs-lookup"><span data-stu-id="e9a15-355">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="e9a15-356">**ASP.NET Core モジュールのログ:** ハンドルされない例外: System.BadImageFormatException: Could not load file or assembly 'my_application.dll'. (ファイルまたはアセンブリ 'my_application.dll' が読み込めませんでした)</span><span class="sxs-lookup"><span data-stu-id="e9a15-356">**ASP.NET Core Module Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly 'my_application.dll'.</span></span> <span data-ttu-id="e9a15-357">正しくない形式のプログラムを読み込もうとしました。</span><span class="sxs-lookup"><span data-stu-id="e9a15-357">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="e9a15-358">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e9a15-358">Troubleshooting:</span></span>

* <span data-ttu-id="e9a15-359">Kestrel でアプリケーションをローカルに実行できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-359">Confirm that the application runs locally on Kestrel.</span></span> <span data-ttu-id="e9a15-360">プロセスのエラーは、アプリケーション内の問題の結果である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-360">A process failure might be the result of a problem within the application.</span></span> <span data-ttu-id="e9a15-361">詳細については、「[トラブルシューティングのヒント](#troubleshooting-tips)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-361">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

* <span data-ttu-id="e9a15-362">RID と競合する `<PlatformTarget>` を *.csproj* に設定していないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-362">Confirm that you didn't set a `<PlatformTarget>` in your *.csproj* that conflicts with the RID.</span></span> <span data-ttu-id="e9a15-363">たとえば、`x86` の `<PlatformTarget>`を指定した場合は、*dotnet publish -c Release -r win10-x64* を使用するか、*.csproj*で `<RuntimeIdentifiers>` を `win10-x64`に設定するかを問わず、`win10-x64` の RID を使用して発行しないでください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-363">For example, don't specify a `<PlatformTarget>` of `x86` and publish with an RID of `win10-x64`, either by using *dotnet publish -c Release -r win10-x64* or by setting the `<RuntimeIdentifiers>` in your *.csproj* to `win10-x64`.</span></span> <span data-ttu-id="e9a15-364">プロジェクトは警告やエラーにならずに発行されますが、上記のログに記録されたシステムの例外によって失敗します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-364">The project publishes without warning or error but fails with the above logged exceptions on the system.</span></span>

* <span data-ttu-id="e9a15-365">Azure Apps 展開で、アプリケーションをアップグレードして新しいアセンブリを展開しようとしたときにこの例外が発生した場合は、以前の展開からすべてのファイルを手動で削除してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-365">If this exception occurs for an Azure Apps deployment when upgrading an application and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="e9a15-366">アップグレードしたアプリを展開するとき、互換性のないアセンブリが残っていると、`System.BadImageFormatException` 例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-366">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

### <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="e9a15-367">URI のエンドポイントが間違っているか、Web サイトが停止している</span><span class="sxs-lookup"><span data-stu-id="e9a15-367">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="e9a15-368">**ブラウザー:** ERR_CONNECTION_REFUSED</span><span class="sxs-lookup"><span data-stu-id="e9a15-368">**Browser:** ERR_CONNECTION_REFUSED</span></span>

* <span data-ttu-id="e9a15-369">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="e9a15-369">**Application Log:** No entry</span></span>

* <span data-ttu-id="e9a15-370">**ASP.NET Core モジュールのログ:** ログ ファイルは作成されません</span><span class="sxs-lookup"><span data-stu-id="e9a15-370">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="e9a15-371">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e9a15-371">Troubleshooting:</span></span>

* <span data-ttu-id="e9a15-372">アプリケーションに対して正しい URI エンドポイントを使用していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-372">Confirm you are using the correct URI endpoint for the application.</span></span> <span data-ttu-id="e9a15-373">バインドを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-373">Check your bindings.</span></span>

* <span data-ttu-id="e9a15-374">IIS Web サイトが *[停止]* 状態でないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-374">Confirm that the IIS website is not in the *Stopped* state.</span></span>

### <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="e9a15-375">CoreWebEngine または W3SVC サーバー機能が無効</span><span class="sxs-lookup"><span data-stu-id="e9a15-375">CoreWebEngine or W3SVC server features disabled</span></span>

* <span data-ttu-id="e9a15-376">**OS の例外:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module. (ASP.NET Core モジュールを使用するには、IIS 7.0 CoreWebEngine および W3SVC の機能をインストールする必要があります。)</span><span class="sxs-lookup"><span data-stu-id="e9a15-376">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="e9a15-377">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e9a15-377">Troubleshooting:</span></span>

* <span data-ttu-id="e9a15-378">適切な役割と機能を有効にしていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-378">Confirm that you have enabled the proper role and features.</span></span> <span data-ttu-id="e9a15-379">「[IIS 構成](#iis-configuration)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-379">See [IIS Configuration](#iis-configuration).</span></span>

### <a name="incorrect-website-physical-path-or-application-missing"></a><span data-ttu-id="e9a15-380">Web サイト物理パスが間違っているか、アプリケーションがない</span><span class="sxs-lookup"><span data-stu-id="e9a15-380">Incorrect website physical path or application missing</span></span>

* <span data-ttu-id="e9a15-381">**ブラウザー:** 403 許可されていません - アクセスが拒否されました **--または--** 403.14 許可されていません - Web サーバーは、このディレクトリの内容の一覧を表示しないように構成されています。</span><span class="sxs-lookup"><span data-stu-id="e9a15-381">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="e9a15-382">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="e9a15-382">**Application Log:** No entry</span></span>

* <span data-ttu-id="e9a15-383">**ASP.NET Core モジュールのログ:** ログ ファイルは作成されません</span><span class="sxs-lookup"><span data-stu-id="e9a15-383">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="e9a15-384">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e9a15-384">Troubleshooting:</span></span>

* <span data-ttu-id="e9a15-385">IIS Web サイトの**基本設定**と物理アプリケーション フォルダーを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-385">Check the IIS website **Basic Settings** and the physical application folder.</span></span> <span data-ttu-id="e9a15-386">アプリケーションが IIS Web サイトの**物理パス**にあるフォルダー内に配置されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-386">Confirm that the application is in the folder at the IIS website **Physical path**.</span></span>

### <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="e9a15-387">役割が正しくない、モジュールがインストールされていない、または不適切なアクセス許可</span><span class="sxs-lookup"><span data-stu-id="e9a15-387">Incorrect role, module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="e9a15-388">**ブラウザー:** 500.19 内部サーバー エラー - ページに関連する構成データが無効であるため、要求されたページにアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="e9a15-388">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>

* <span data-ttu-id="e9a15-389">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="e9a15-389">**Application Log:** No entry</span></span>

* <span data-ttu-id="e9a15-390">**ASP.NET Core モジュールのログ:** ログ ファイルは作成されません</span><span class="sxs-lookup"><span data-stu-id="e9a15-390">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="e9a15-391">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e9a15-391">Troubleshooting:</span></span>

* <span data-ttu-id="e9a15-392">適切な役割を有効にしていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-392">Confirm that you've enabled the proper role.</span></span> <span data-ttu-id="e9a15-393">「[IIS 構成](#iis-configuration)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-393">See [IIS Configuration](#iis-configuration).</span></span>

* <span data-ttu-id="e9a15-394">**[プログラムと機能]** をチェックし、**Microsoft ASP.NET Core モジュール**がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-394">Check **Programs &amp; Features** and confirm that the **Microsoft ASP.NET Core Module** has been installed.</span></span> <span data-ttu-id="e9a15-395">インストールされているプログラムの一覧に **Microsoft ASP.NET Core モジュール**が表示されない場合は、モジュールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-395">If the **Microsoft ASP.NET Core Module** isn't present in the list of installed programs, install the module.</span></span> <span data-ttu-id="e9a15-396">「[.NET Core Windows Server ホスティング バンドルのインストール](#install-the-net-core-windows-server-hosting-bundle)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-396">See [Install the .NET Core Windows Server Hosting bundle](#install-the-net-core-windows-server-hosting-bundle).</span></span>

* <span data-ttu-id="e9a15-397">**[アプリケーション プール]**、**[プロセス モデル]**、**[ID]** が **ApplicationPoolIdentity** に設定されていることを確認します。または、アプリの展開フォルダーにアクセスするための正しいアクセス許可がカスタム ID に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-397">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or your custom identity has the correct permissions to access the app's deployment folder.</span></span>

### <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="e9a15-398">processPath の誤り、PATH 変数の欠如、ホスティング バンドルが未インストール、システムまたは IIS が再起動されていない、VC++ 再頒布可能パッケージが未インストール、dotnet.exe アクセス違反</span><span class="sxs-lookup"><span data-stu-id="e9a15-398">Incorrect processPath, missing PATH variable, hosting bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="e9a15-399">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="e9a15-399">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e9a15-400">**アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\my_application.exe" ', ErrorCode = '0x80070002 : 0. (物理ルート 'C:\{PATH}\' のアプリケーション 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' は、コマンド ライン '".\my_application.exe" ' でプロセスを開始できませんでした。ErrorCode = '0x80070002 : 0)</span><span class="sxs-lookup"><span data-stu-id="e9a15-400">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\my_application.exe" ', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="e9a15-401">**ASP.NET Core モジュールのログ:** ログ ファイルが作成されましたが空です</span><span class="sxs-lookup"><span data-stu-id="e9a15-401">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="e9a15-402">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e9a15-402">Troubleshooting:</span></span>

* <span data-ttu-id="e9a15-403">Kestrel でアプリケーションをローカルに実行できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-403">Confirm that the application runs locally on Kestrel.</span></span> <span data-ttu-id="e9a15-404">プロセスのエラーは、アプリケーション内の問題の結果である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-404">A process failure might be the result of a problem within the application.</span></span> <span data-ttu-id="e9a15-405">詳細については、「[トラブルシューティングのヒント](#troubleshooting-tips)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-405">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

* <span data-ttu-id="e9a15-406">*web.config* で `<aspNetCore>` 要素の *processPath* 属性を調べ、フレームワークに依存する展開 (FDD) を示す *dotnet*、または自己完結型の展開 (SCD) を示す *.\my_application.exe* になっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-406">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's *dotnet* for a framework-dependent deployment (FDD) or *.\my_application.exe* for a self-contained deployment (SCD).</span></span>

* <span data-ttu-id="e9a15-407">FDD の場合、PATH 設定で *dotnet.exe* にアクセスできていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-407">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="e9a15-408">*C:\Program Files\dotnet\* がシステムの PATH 設定に含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-408">Confirm that *C:\Program Files\dotnet\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="e9a15-409">FDD では、アプリケーション プールのユーザー ID で *dotnet.exe* にアクセスできていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-409">For an FDD, *dotnet.exe* might not be accessible for the user identity of the Application Pool.</span></span> <span data-ttu-id="e9a15-410">AppPool ユーザー ID に、*C:\Program Files\dotnet* ディレクトリへのアクセス許可が設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-410">Confirm that the AppPool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="e9a15-411">*C:\Program Files\dotnet* とアプリケーション ディレクトリに、AppPool ユーザー ID に対する拒否ルールが構成されていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-411">Confirm that there are no deny rules configured for the AppPool user identity on the *C:\Program Files\dotnet* and application directories.</span></span>

* <span data-ttu-id="e9a15-412">FDD を配置し、IIS を再起動せずに .NET Core をインストールした可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-412">You may have deployed an FDD and installed .NET Core without restarting IIS.</span></span> <span data-ttu-id="e9a15-413">サーバーを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-413">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="e9a15-414">ホスト システム .NET Core ランタイムをインストールせずに、FDD を配置した可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-414">You may have deployed an FDD without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="e9a15-415">FDD を配置しようとしていて、.NET Core ランタイムをインストールしていない場合は、**.NET Core Windows Server ホスティング バンドル インストーラー**をシステムで実行します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-415">If you're attempting to deploy a FDD and haven't installed the .NET Core runtime, run the **.NET Core Windows Server Hosting bundle installer** on the system.</span></span> <span data-ttu-id="e9a15-416">「[.NET Core Windows Server ホスティング バンドルのインストール](#install-the-net-core-windows-server-hosting-bundle)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-416">See [Install the .NET Core Windows Server Hosting bundle](#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="e9a15-417">インターネットに接続せずに、.NET Core ランタイムをシステムにインストールする場合は、[.NET ダウンロード](https://www.microsoft.com/net/download/core)のページからランタイムを入手し、ホスティング バンドル インストーラーを実行して ASP.NET Core モジュールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-417">If you're attempting to install the .NET Core runtime on a system without an Internet connection, obtain the runtime from [.NET Downloads](https://www.microsoft.com/net/download/core) and run the hosting bundle installer to install the ASP.NET Core Module.</span></span> <span data-ttu-id="e9a15-418">インストールを完了するために、システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-418">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="e9a15-419">FDD を配置し、システム/IIS を再起動せずに .NET Core をインストールした可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-419">You may have deployed an FDD and installed .NET Core without restarting the system/IIS.</span></span> <span data-ttu-id="e9a15-420">システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-420">Either restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="e9a15-421">FDD を配置し、*Microsoft Visual C++ 2015 再頒布可能パッケージ (x64)* がシステムにインストールされていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-421">You may have deployed an FDD and the *Microsoft Visual C++ 2015 Redistributable (x64)* is not installed on the system.</span></span> <span data-ttu-id="e9a15-422">インストーラーは [Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=53840)から入手できます。</span><span class="sxs-lookup"><span data-stu-id="e9a15-422">You may obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

### <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="e9a15-423">\<aspNetCore\> 要素の引数の誤り</span><span class="sxs-lookup"><span data-stu-id="e9a15-423">Incorrect arguments of \<aspNetCore\> element</span></span>

* <span data-ttu-id="e9a15-424">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="e9a15-424">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e9a15-425">**アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\my_application.dll', ErrorCode = '0x80004005 : 80008081. (物理ルート 'C:\{PATH}\' のアプリケーション 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' は、コマンド ライン '"dotnet" .\my_application.dll' でプロセスを開始できませんでした。ErrorCode = '0x80004005 : 80008081)</span><span class="sxs-lookup"><span data-stu-id="e9a15-425">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\my_application.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="e9a15-426">**ASP.NET Core モジュールのログ:** The application to execute does not exist: 'PATH\my_application.dll' (実行するアプリケーションが存在しません: 'PATH\my_application.dll')</span><span class="sxs-lookup"><span data-stu-id="e9a15-426">**ASP.NET Core Module Log:** The application to execute does not exist: 'PATH\my_application.dll'</span></span>

<span data-ttu-id="e9a15-427">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e9a15-427">Troubleshooting:</span></span>

* <span data-ttu-id="e9a15-428">Kestrel でアプリケーションをローカルに実行できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-428">Confirm that the application runs locally on Kestrel.</span></span> <span data-ttu-id="e9a15-429">プロセスのエラーは、アプリケーション内の問題の結果である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-429">A process failure might be the result of a problem within the application.</span></span> <span data-ttu-id="e9a15-430">詳細については、「[トラブルシューティングのヒント](#troubleshooting-tips)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-430">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

* <span data-ttu-id="e9a15-431">*web.config* で `<aspNetCore>` 要素の *arguments* 属性を調べ、次のいずれかになっていることを確認します。(a) フレームワークに依存する展開 (FDD) の場合は *.\my_applciation.dll*、または (b) 自己完結型の展開 (SCD) の場合は、未指定の空の文字列 (*arguments=""*) か、アプリの引数のリスト (*arguments="arg1, arg2, ..."*)。</span><span class="sxs-lookup"><span data-stu-id="e9a15-431">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it is either (a) *.\my_application.dll* for a framework-dependent deployment (FDD); or (b) not present, an empty string (*arguments=""*), or a list of your app's arguments (*arguments="arg1, arg2, ..."*) for a self-contained deployment (SCD).</span></span>

### <a name="missing-net-framework-version"></a><span data-ttu-id="e9a15-432">.NET Framework バージョンの欠落</span><span class="sxs-lookup"><span data-stu-id="e9a15-432">Missing .NET Framework version</span></span>

* <span data-ttu-id="e9a15-433">**ブラウザー:** 502.3 ゲートウェイが正しくありません - There was a connection error while trying to route the request. (要求のルーティング中に接続エラーが発生しました。)</span><span class="sxs-lookup"><span data-stu-id="e9a15-433">**Browser:** 502.3 Bad Gateway - There was a connection error while trying to route the request.</span></span>

* <span data-ttu-id="e9a15-434">**アプリケーション ログ:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\my_application.dll', ErrorCode = '0x80004005 : 80008081. (ErrorCode = 物理ルート 'C:\{PATH}\' のアプリケーション 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' は、コマンド ライン '"dotnet" .\my_application.dll' でプロセスを開始できませんでした。ErrorCode = '0x80004005 : 80008081)</span><span class="sxs-lookup"><span data-stu-id="e9a15-434">**Application Log:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\my_application.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="e9a15-435">**ASP.NET Core モジュールのログ:** メソッド、ファイル、またはアセンブリの欠如の例外。</span><span class="sxs-lookup"><span data-stu-id="e9a15-435">**ASP.NET Core Module Log:** Missing method, file, or assembly exception.</span></span> <span data-ttu-id="e9a15-436">例外で特定されたメソッド、ファイル、またはアセンブリは、.NET Framework のメソッド、ファイル、またはアセンブリです。</span><span class="sxs-lookup"><span data-stu-id="e9a15-436">The method, file, or assembly specified in the exception is a .NET Framework method, file, or assembly.</span></span>

<span data-ttu-id="e9a15-437">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="e9a15-437">Troubleshooting:</span></span>

* <span data-ttu-id="e9a15-438">システムにない .NET Framework のバージョンをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-438">Install the .NET Framework version missing from the system.</span></span>

* <span data-ttu-id="e9a15-439">フレームワークに依存する展開 (FDD) では、正しいランタイムがシステムにインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-439">For a framework-dependent deployment (FDD), confirm that you have the correct runtime installed on the system.</span></span> <span data-ttu-id="e9a15-440">プロジェクトを 1.1 から 2.0 にアップグレードしてからホスティング システムに展開し、この例外を受け取った場合は、ホスティング システムに 2.0 のフレームワークをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e9a15-440">If you upgrade a project from 1.1 to 2.0, deploy to the hosting system, and receive this exception, ensure you install the 2.0 framework on the hosting system.</span></span>

### <a name="stopped-application-pool"></a><span data-ttu-id="e9a15-441">アプリケーション プールの停止</span><span class="sxs-lookup"><span data-stu-id="e9a15-441">Stopped Application Pool</span></span>

* <span data-ttu-id="e9a15-442">**ブラウザー:** 503 サービスを利用できません</span><span class="sxs-lookup"><span data-stu-id="e9a15-442">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="e9a15-443">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="e9a15-443">**Application Log:** No entry</span></span>

* <span data-ttu-id="e9a15-444">**ASP.NET Core モジュールのログ:** ログ ファイルは作成されません</span><span class="sxs-lookup"><span data-stu-id="e9a15-444">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="e9a15-445">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="e9a15-445">Troubleshooting</span></span>

* <span data-ttu-id="e9a15-446">アプリケーション プールが *[停止]* 状態でないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-446">Confirm that the Application Pool is not in the *Stopped* state.</span></span>

### <a name="iis-integration-middleware-not-implemented"></a><span data-ttu-id="e9a15-447">IIS 統合ミドルウェアが未実装</span><span class="sxs-lookup"><span data-stu-id="e9a15-447">IIS Integration middleware not implemented</span></span>

* <span data-ttu-id="e9a15-448">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="e9a15-448">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e9a15-449">**アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ' but either crashed or did not response or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4' (物理ルート 'C:\{PATH}\' のアプリケーション 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' は、コマンド ライン '"C:\{PATH}\my_application.{exe|dll}" ' でプロセスを作成しましたが、クラッシュしたか、応答しなかったか、指定されたポート '{PORT}' でリッスンしていません。ErrorCode = '0x800705b4')</span><span class="sxs-lookup"><span data-stu-id="e9a15-449">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="e9a15-450">**ASP.NET Core モジュールのログ:** ログ ファイルが作成され、正常動作を示しています。</span><span class="sxs-lookup"><span data-stu-id="e9a15-450">**ASP.NET Core Module Log:** Log file created and shows normal operation.</span></span>

<span data-ttu-id="e9a15-451">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="e9a15-451">Troubleshooting</span></span>

* <span data-ttu-id="e9a15-452">Kestrel でアプリをローカルに実行できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-452">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="e9a15-453">プロセスのエラーは、アプリ内の問題の結果である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e9a15-453">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="e9a15-454">詳細については、「[トラブルシューティングのヒント](#troubleshooting-tips)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-454">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

* <span data-ttu-id="e9a15-455">アプリケーションの *WebHostBuilder()* (ASP.NET Core 1.x) で *.UseIISIntegration()* メソッドを呼び出して IIS 統合ミドルウェアを正しく参照していることを、あるいは `CreateDefaultBuilder` メソッド (ASP.NET Core 2.x) を使用していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-455">Confirm that you've correctly referenced the IIS Integration middleware by calling the *.UseIISIntegration()* method on the application's *WebHostBuilder()* (ASP.NET Core 1.x) or used the `CreateDefaultBuilder` method (ASP.NET Core 2.x).</span></span> <span data-ttu-id="e9a15-456">詳細については、「[ASP.NET Core でのホスティング](xref:fundamentals/hosting)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-456">See [Hosting in ASP.NET Core](xref:fundamentals/hosting) for details.</span></span>

### <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="e9a15-457">サブアプリケーションに \<handlers\> セクションが含まれている</span><span class="sxs-lookup"><span data-stu-id="e9a15-457">Sub-application includes a \<handlers\> section</span></span>

* <span data-ttu-id="e9a15-458">**ブラウザー:** HTTP エラー 500.19 - 内部サーバー エラー</span><span class="sxs-lookup"><span data-stu-id="e9a15-458">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="e9a15-459">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="e9a15-459">**Application Log:** No entry</span></span>

* <span data-ttu-id="e9a15-460">**ASP.NET Core モジュールのログ:** ログ ファイルが作成され、ルート アプリケーションの正常動作を示しています。</span><span class="sxs-lookup"><span data-stu-id="e9a15-460">**ASP.NET Core Module Log:** Log file created and shows normal operation for the root application.</span></span> <span data-ttu-id="e9a15-461">サブアプリケーションにログ ファイルは作成されません。</span><span class="sxs-lookup"><span data-stu-id="e9a15-461">Log file not created for the sub-application.</span></span>

<span data-ttu-id="e9a15-462">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="e9a15-462">Troubleshooting</span></span>

* <span data-ttu-id="e9a15-463">サブアプリの *web.config* ファイルに `<handlers>` セクションが含まれていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-463">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

### <a name="application-configuration-general-issue"></a><span data-ttu-id="e9a15-464">アプリケーション構成の一般的な問題</span><span class="sxs-lookup"><span data-stu-id="e9a15-464">Application configuration general issue</span></span>

* <span data-ttu-id="e9a15-465">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="e9a15-465">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="e9a15-466">**アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ' but either crashed or did not response or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4' (物理ルート 'C:\{PATH}\' のアプリケーション 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' は、コマンド ライン '"C:\{PATH}\my_application.{exe|dll}" ' でプロセスを作成しましたが、クラッシュしたか、応答しなかったか、指定されたポート '{PORT}' でリッスンしていません。ErrorCode = '0x800705b4')</span><span class="sxs-lookup"><span data-stu-id="e9a15-466">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="e9a15-467">**ASP.NET Core モジュールのログ:** ログ ファイルが作成されましたが空です</span><span class="sxs-lookup"><span data-stu-id="e9a15-467">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="e9a15-468">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="e9a15-468">Troubleshooting</span></span>

* <span data-ttu-id="e9a15-469">この一般例外はプロセスを開始できなかったことを示し、ほとんどの場合はアプリケーション構成の問題が原因です。</span><span class="sxs-lookup"><span data-stu-id="e9a15-469">This general exception indicates that the process failed to start, most likely due to an application configuration issue.</span></span> <span data-ttu-id="e9a15-470">[ディレクトリ構造](xref:hosting/directory-structure)に関する説明を参照して、アプリの展開済みファイルとフォルダーが適切であること、アプリの構成ファイルが存在し、そのファイルにアプリと環境に応じた正しい設定が含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e9a15-470">Referring to [Directory Structure](xref:hosting/directory-structure), confirm that your app's deployed files and folders are appropriate and that your app's configuration files are present and contain the correct settings for your app and environment.</span></span> <span data-ttu-id="e9a15-471">詳細については、「[トラブルシューティングのヒント](#troubleshooting-tips)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e9a15-471">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

## <a name="resources"></a><span data-ttu-id="e9a15-472">リソース</span><span class="sxs-lookup"><span data-stu-id="e9a15-472">Resources</span></span>

* [<span data-ttu-id="e9a15-473">ASP.NET Core モジュールの概要</span><span class="sxs-lookup"><span data-stu-id="e9a15-473">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="e9a15-474">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="e9a15-474">ASP.NET Core Module configuration reference</span></span>](xref:hosting/aspnet-core-module)
* [<span data-ttu-id="e9a15-475">IIS モジュールと ASP.NET Core の使用</span><span class="sxs-lookup"><span data-stu-id="e9a15-475">Using IIS Modules with ASP.NET Core</span></span>](xref:hosting/iis-modules)
* [<span data-ttu-id="e9a15-476">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="e9a15-476">Introduction to ASP.NET Core</span></span>](../index.md)
* [<span data-ttu-id="e9a15-477">Microsoft IIS 公式サイト</span><span class="sxs-lookup"><span data-stu-id="e9a15-477">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="e9a15-478">Microsoft TechNet ライブラリ: Windows Server</span><span class="sxs-lookup"><span data-stu-id="e9a15-478">Microsoft TechNet Library: Windows Server</span></span>](https://docs.microsoft.com/windows-server/windows-server-versions)
