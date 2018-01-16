---
title: "IIS を使用した Windows での ASP.NET Core のホスト"
author: guardrex
description: "Windows Server インターネット インフォメーション サービス (IIS) での ASP.NET Core アプリをホストする方法を説明します。"
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/index
ms.openlocfilehash: 01cedb4e3abb35670d2908fe8cb4367c3fd58b33
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="e1f7b-103">IIS を使用した Windows での ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="e1f7b-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="e1f7b-104">執筆者: [Luke Latham](https://github.com/guardrex)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e1f7b-104">By [Luke Latham](https://github.com/guardrex) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="e1f7b-105">サポートされるオペレーティング システム</span><span class="sxs-lookup"><span data-stu-id="e1f7b-105">Supported operating systems</span></span>

<span data-ttu-id="e1f7b-106">次のオペレーティング システムがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-106">The following operating systems are supported:</span></span>

* <span data-ttu-id="e1f7b-107">Windows 7 およびそれ以降</span><span class="sxs-lookup"><span data-stu-id="e1f7b-107">Windows 7 and newer</span></span>
* <span data-ttu-id="e1f7b-108">Windows Server 2008 R2 およびそれ以降&#8224;</span><span class="sxs-lookup"><span data-stu-id="e1f7b-108">Windows Server 2008 R2 and newer&#8224;</span></span>

<span data-ttu-id="e1f7b-109">&#8224;概念的には、このドキュメントで説明する IIS 構成は、Nano Server IIS で ASP.NET Core アプリをホストする場合にも当てはまりますが、詳細な手順については「[Nano Server の ASP.NET Core と IIS](xref:tutorials/nano-server)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-109">&#8224;Conceptually, the IIS configuration described in this document also applies to hosting ASP.NET Core apps on Nano Server IIS, but refer to [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) for specific instructions.</span></span>

<span data-ttu-id="e1f7b-110">[HTTP.sys サーバー](xref:fundamentals/servers/httpsys) (以前の名称は [WebListener](xref:fundamentals/servers/weblistener)) が、IIS を含むリバース プロキシ構成で動作しません。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) won't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="e1f7b-111">[Kestrel サーバー](xref:fundamentals/servers/kestrel)を使用します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="e1f7b-112">IIS 構成</span><span class="sxs-lookup"><span data-stu-id="e1f7b-112">IIS configuration</span></span>

<span data-ttu-id="e1f7b-113">**Web サーバー (IIS)** の役割を有効にし、役割のサービスを設定します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-113">Enable the **Web Server (IIS)** role and establish role services.</span></span>

### <a name="windows-desktop-operating-systems"></a><span data-ttu-id="e1f7b-114">Windows デスクトップ オペレーティング システム</span><span class="sxs-lookup"><span data-stu-id="e1f7b-114">Windows desktop operating systems</span></span>

<span data-ttu-id="e1f7b-115">**[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-115">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="e1f7b-116">**[インターネット インフォメーション サービス]** と **[Web 管理ツール]** のグループを開きます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-116">Open the group for **Internet Information Services** and **Web Management Tools**.</span></span> <span data-ttu-id="e1f7b-117">**[IIS 管理コンソール]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-117">Check the box for **IIS Management Console**.</span></span> <span data-ttu-id="e1f7b-118">**[World Wide Web サービス]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-118">Check the box for **World Wide Web Services**.</span></span> <span data-ttu-id="e1f7b-119">**[World Wide Web サービス]** の既定の機能をそのまま使用するか、IIS 機能をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-119">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

![[Windows の機能] で [IIS 管理コンソール] と [World Wide Web サービス] を選択します。](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a><span data-ttu-id="e1f7b-121">Windows Server オペレーティング システム</span><span class="sxs-lookup"><span data-stu-id="e1f7b-121">Windows Server operating systems</span></span>

<span data-ttu-id="e1f7b-122">サーバー オペレーティング システムでは、**[管理]** メニューから**役割と機能の追加**ウィザードを使用するか、**サーバー マネージャー**にあるリンクを使用します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-122">For server operating systems, use the **Add Roles and Features** wizard via the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="e1f7b-123">**[サーバーの役割]** のステップで、**[Web サーバー (IIS)]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-123">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

![[サーバーの役割の選択] のステップで Web サーバー IIS の役割を選択します。](index/_static/server-roles-ws2016.png)

<span data-ttu-id="e1f7b-125">**[役割サービス]** のステップで、希望する IIS の役割サービスを選択するか、既定の役割サービスをそのまま使用します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-125">On the **Role services** step, select the IIS role services desired or accept the default role services provided.</span></span>

![[役割サービスの選択] のステップで既定の役割サービスを選択します。](index/_static/role-services-ws2016.png)

<span data-ttu-id="e1f7b-127">**[確認]** のステップを経て Web サーバーの役割とサービスをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-127">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="e1f7b-128">Web サーバー (IIS) の役割をインストールした後、サーバーと IIS の再起動は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-128">A server/IIS restart isn't required after installing the Web Server (IIS) role.</span></span>

## <a name="install-the-net-core-windows-server-hosting-bundle"></a><span data-ttu-id="e1f7b-129">.NET Core Windows Server ホスティング バンドルのインストール</span><span class="sxs-lookup"><span data-stu-id="e1f7b-129">Install the .NET Core Windows Server Hosting bundle</span></span>

1. <span data-ttu-id="e1f7b-130">ホスティング システムに [.NET Core Windows Server ホスティング バンドル](https://aka.ms/dotnetcore-2-windowshosting)をインストールします。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-130">Install the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore-2-windowshosting) on the hosting system.</span></span> <span data-ttu-id="e1f7b-131">このバンドルをインストールすることで、.NET Core ランタイム、.NET Core ライブラリ、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-131">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="e1f7b-132">このモジュールは、IIS と Kestrel サーバーの間にリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-132">The module creates the reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="e1f7b-133">システムにインターネット接続が設定されていない場合は、.NET Core Windows Server ホスティング バンドルをインストールする前に、[Microsoft Visual C++ 2015 再頒布可能パッケージ](https://www.microsoft.com/download/details.aspx?id=53840)を入手してインストールしてください。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-133">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Windows Server Hosting bundle.</span></span>

2. <span data-ttu-id="e1f7b-134">システムを再起動するか、コマンド プロンプトから **net stop was /y** の後に続けて **net start w3svc** を実行して、システム PATH への変更を適用します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-134">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt to pick up a change to the system PATH.</span></span>

> [!NOTE]
> <span data-ttu-id="e1f7b-135">IIS 共有構成の詳細については、「[ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)」 (IIS 共有構成の ASP.NET Core モジュール) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-135">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="e1f7b-136">Visual Studio で発行する場合の Web 配置のインストール</span><span class="sxs-lookup"><span data-stu-id="e1f7b-136">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="e1f7b-137">[Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用してサーバーにアプリを展開する場合、Web 配置の最新バージョンをサーバーにインストールします。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-137">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="e1f7b-138">Web 配置をインストールするには、[Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) を使用するか、[Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=43717)からインストーラーを取得します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-138">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="e1f7b-139">WebPI を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-139">The preferred method is to use WebPI.</span></span> <span data-ttu-id="e1f7b-140">WebPI は、スタンドアロンのセットアップとホスティング プロバイダー向けの構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-140">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="e1f7b-141">アプリケーション構成</span><span class="sxs-lookup"><span data-stu-id="e1f7b-141">Application configuration</span></span>

### <a name="enabling-the-iisintegration-components"></a><span data-ttu-id="e1f7b-142">IISIntegration コンポーネントを有効にする</span><span class="sxs-lookup"><span data-stu-id="e1f7b-142">Enabling the IISIntegration components</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e1f7b-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e1f7b-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e1f7b-144">一般的な *Program.cs* は [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を呼び出してホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-144">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to begin setting up a host.</span></span> <span data-ttu-id="e1f7b-145">`CreateDefaultBuilder` は [Kestrel](xref:fundamentals/servers/kestrel) を Web サーバーとして構成し、ベース パスとポートを [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)に構成することで、IIS 統合を有効にします。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-145">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e1f7b-146">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e1f7b-146">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e1f7b-147">アプリの依存関係に [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) パッケージへの依存関係を含めます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-147">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="e1f7b-148">*UseIISIntegration* 拡張メソッドを *WebHostBuilder* に追加して、IIS 統合ミドルウェアをアプリに組み込みます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-148">Incorporate IIS Integration middleware into the app by adding the *UseIISIntegration* extension method to *WebHostBuilder*:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="e1f7b-149">`UseKestrel` と `UseIISIntegration` の両方が必要です。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-149">Both `UseKestrel` and `UseIISIntegration` are required.</span></span> <span data-ttu-id="e1f7b-150">`UseIISIntegration` を呼び出すコードはコードの移植性に影響しません。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-150">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="e1f7b-151">アプリが IIS の背後で実行されていない場合 (たとえば、アプリが Kestrel で直接実行されている場合)、`UseIISIntegration` は機能しません。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-151">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` no-ops.</span></span>

---

<span data-ttu-id="e1f7b-152">ホスティングの詳細については、「[Hosting in ASP.NET Core](xref:fundamentals/hosting)」(ASP.NET Core でのホスティング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-152">For more information on hosting, see [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span></span>

### <a name="iis-options"></a><span data-ttu-id="e1f7b-153">IIS のオプション</span><span class="sxs-lookup"><span data-stu-id="e1f7b-153">IIS options</span></span>

<span data-ttu-id="e1f7b-154">IIS オプションを構成するには、`IISOptions` 用のサービス構成を `ConfigureServices` に含めます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-154">To configure IIS options, include a service configuration for `IISOptions` in `ConfigureServices`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| <span data-ttu-id="e1f7b-155">オプション</span><span class="sxs-lookup"><span data-stu-id="e1f7b-155">Option</span></span>                         | <span data-ttu-id="e1f7b-156">既定値</span><span class="sxs-lookup"><span data-stu-id="e1f7b-156">Default</span></span> | <span data-ttu-id="e1f7b-157">設定</span><span class="sxs-lookup"><span data-stu-id="e1f7b-157">Setting</span></span> |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="e1f7b-158">`true` の場合、認証ミドルウェアが `HttpContext.User` を設定し、一般的な課題に応答します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-158">If `true`, the authentication middleware sets the `HttpContext.User` and responds to generic challenges.</span></span> <span data-ttu-id="e1f7b-159">`false` の場合、認証ミドルウェアは ID (`HttpContext.User`) を提供するだけで、`AuthenticationScheme` によって明示的に要求されたときに課題に応答します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-159">If `false`, the authentication middleware only provides an identity (`HttpContext.User`) and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="e1f7b-160">`AutomaticAuthentication` を機能させるためには、IIS で Windows 認証を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-160">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="e1f7b-161">ログイン ページでユーザーに表示名が表示されるように設定します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-161">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="e1f7b-162">`true` の場合、`MS-ASPNETCORE-CLIENTCERT` 要求ヘッダーが指定されていると、`HttpContext.Connection.ClientCertificate` が設定されます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-162">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="webconfig"></a><span data-ttu-id="e1f7b-163">web.config</span><span class="sxs-lookup"><span data-stu-id="e1f7b-163">web.config</span></span>

<span data-ttu-id="e1f7b-164">*web.config* ファイルの主な目的は、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)を構成することです。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-164">The *web.config* file's primary purpose is to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="e1f7b-165">必要に応じて、追加の IIS の構成設定を指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-165">It may optionally provide additional IIS configuration settings.</span></span> <span data-ttu-id="e1f7b-166">*web.config* の作成、変換、発行は .NET Core Web SDK (`Microsoft.NET.Sdk.Web`) で処理されます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-166">Creating, transforming, and publishing *web.config* is handled by the .NET Core Web SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="e1f7b-167">SDK は、プロジェクト ファイル `<Project Sdk="Microsoft.NET.Sdk.Web">` の先頭で設定されています。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-167">The SDK is set  at the top of the project file, `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span></span> <span data-ttu-id="e1f7b-168">SDK によって *web.config* ファイルが変換されないようにするため、`true` に設定した **\<IsTransformWebConfigDisabled>** プロパティをプロジェクト ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-168">To prevent the SDK from transforming the *web.config* file, add the **\<IsTransformWebConfigDisabled>** property to the project file with a setting of `true`:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="e1f7b-169">プロジェクトに *web.config* ファイルが含まれている場合、そのファイルは [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)を構成するための正しい *processPath* と *arguments* を使用して変換され、[発行された出力](xref:host-and-deploy/directory-structure)に移行されます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-169">If a *web.config* file is in the project, it's transformed with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span> <span data-ttu-id="e1f7b-170">変換によりファイル内の IIS 構成の設定が変わることはありません。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-170">The transformation doesn't modify IIS configuration settings in the file.</span></span>

### <a name="webconfig-location"></a><span data-ttu-id="e1f7b-171">web.config の場所</span><span class="sxs-lookup"><span data-stu-id="e1f7b-171">web.config location</span></span>

<span data-ttu-id="e1f7b-172">.NET Core アプリは、IIS と Kestrel サーバー間のリバース プロキシによってホストされます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-172">.NET Core apps are hosted via a reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="e1f7b-173">リバース プロキシを作成するためには、展開されるアプリのコンテンツ ルート パス (通常は、アプリ ベース パス) に *web.config* ファイルが存在する必要があります。これは IIS に対して指定される Web サイトの物理パスです。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-173">In order to create the reverse proxy, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app, which is the website physical path provided to IIS.</span></span> <span data-ttu-id="e1f7b-174">*Web.config* ファイルは、Web 配置を使用して複数のアプリの発行を有効にするため、アプリのルートで必要です。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-174">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="e1f7b-175">アプリの物理パスには、*\<assembly_name>.runtimeconfig.json*、*\<assembly_name>.xml* (XML ドキュメントのコメント)、*\<assembly_name>.deps.json* などのサブフォルダーを含め、機密性の高いファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-175">Sensitive files exist on the app's physical path, including subfolders, such as *\<assembly_name>.runtimeconfig.json*, *\<assembly_name>.xml* (XML Documentation comments), and *\<assembly_name>.deps.json*.</span></span> <span data-ttu-id="e1f7b-176">*web.config* ファイルが存在し、サイトを構成している場合、IIS がこれらの機密性の高いファイルが使用されるのを防止します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-176">When the *web.config* file is present and configures the site, IIS prevents these sensitive files from being served.</span></span> <span data-ttu-id="e1f7b-177">**したがって、*web.config* ファイルを誤って名前変更したり展開から削除したりしないようにすることが重要です。**</span><span class="sxs-lookup"><span data-stu-id="e1f7b-177">**Therefore, it's important that the *web.config* file isn't accidently renamed or removed from the deployment.**</span></span>

## <a name="create-the-iis-website"></a><span data-ttu-id="e1f7b-178">IIS Web サイトを作成する</span><span class="sxs-lookup"><span data-stu-id="e1f7b-178">Create the IIS Website</span></span>

1. <span data-ttu-id="e1f7b-179">[ディレクトリ構造](xref:host-and-deploy/directory-structure)に関するページで説明されている、アプリの公開フォルダーとファイルを格納するためのフォルダーを、ターゲットの IIS システムで作成します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-179">On the target IIS system, create a folder to contain the app's published folders and files, which are described in [Directory Structure](xref:host-and-deploy/directory-structure).</span></span>

2. <span data-ttu-id="e1f7b-180">そのフォルダー内で、stdout ログが有効になっている場合に stdout ログを保持するための *logs* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-180">Within the folder, create a *logs* folder to hold stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="e1f7b-181">ペイロードに *logs* フォルダーを含めてアプリを配置する場合は、この手順をスキップします。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-181">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="e1f7b-182">MSBuild で *logs* フォルダーを作成する手順については、[ディレクトリ構造](xref:host-and-deploy/directory-structure)に関するトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-182">For instructions on how to make MSBuild create the *logs* folder, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

3. <span data-ttu-id="e1f7b-183">**IIS マネージャー**で新しい Web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-183">In **IIS Manager**, create a new website.</span></span> <span data-ttu-id="e1f7b-184">**[サイト名]** を指定し、**[物理パス]** にはアプリの配置フォルダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-184">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="e1f7b-185">**[バインド]** の構成を指定して Web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-185">Provide the **Binding** configuration and create the website.</span></span>

4. <span data-ttu-id="e1f7b-186">アプリケーション プールを **[マネージ コードなし]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-186">Set the application pool to **No Managed Code**.</span></span> <span data-ttu-id="e1f7b-187">ASP.NET Core は、別個のプロセスで実行され、ランタイムを管理します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-187">ASP.NET Core runs in a separate process and manages the runtime.</span></span>

5. <span data-ttu-id="e1f7b-188">**[Web サイトの追加]** ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-188">Open the **Add Website** window.</span></span>

   ![サイトのコンテキスト メニューで [Web サイトの追加] を選択します。](index/_static/add-website-context-menu-ws2016.png)

6. <span data-ttu-id="e1f7b-190">Web サイトを構成します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-190">Configure the website.</span></span>

   ![[Web サイトの追加] のステップでサイト名、物理パス、ホスト名を指定します。](index/_static/add-website-ws2016.png)

7. <span data-ttu-id="e1f7b-192">**[アプリケーション プール]** パネルで、**[アプリケーション プールの編集]** ウィンドウを開きます。そのためには、Web サイトのアプリ プール上で右クリックし、ポップアップ メニューから **[基本設定]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-192">In the **Application Pools** panel, open the **Edit Application Pool** window by right-clicking on the website's app pool and selecting **Basic Settings...** from the popup menu.</span></span>

   ![アプリケーション プールのコンテキスト メニューから [基本設定] を選択します。](index/_static/apppools-basic-settings-ws2016.png)

8. <span data-ttu-id="e1f7b-194">**[.NET CLR バージョン]** を **[マネージ コードなし]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-194">Set the **.NET CLR version** to **No Managed Code**.</span></span>

   ![.NET CLR バージョンとして [マネージ コードなし] を設定します。](index/_static/edit-apppool-ws2016.png)
     
    <span data-ttu-id="e1f7b-196">注: **[.NET CLR バージョン]** の **[マネージ コードなし]** への設定は、任意です。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-196">Note: Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span> <span data-ttu-id="e1f7b-197">ASP.NET Core を使用するためにデスクトップ CLR を読み込む必要はありません。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-197">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span>

9. <span data-ttu-id="e1f7b-198">プロセス モデル ID に適切なアクセス許可があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-198">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="e1f7b-199">アプリ プールの既定の ID (**[プロセス モデル]** > **[ID]**) を **ApplicationPoolIdentity** から別の ID に変更した場合は、アプリのフォルダー、データベース、その他の必要なリソースにアクセスするために要求されるアクセス許可が新しい ID に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-199">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span>
   
## <a name="deploy-the-app"></a><span data-ttu-id="e1f7b-200">アプリを配置する</span><span class="sxs-lookup"><span data-stu-id="e1f7b-200">Deploy the app</span></span>

<span data-ttu-id="e1f7b-201">ターゲット IIS システム上に作成したフォルダーにアプリを展開します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-201">Deploy the app to the folder created on the target IIS system.</span></span> <span data-ttu-id="e1f7b-202">[Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)は、推奨される展開のメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-202">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

<span data-ttu-id="e1f7b-203">配置用に発行したアプリが実行されていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-203">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="e1f7b-204">アプリが実行中は、*publish* フォルダー内のファイルがロックされます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-204">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="e1f7b-205">ロックされたファイルを上書きすることはできません。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-205">Locked files can't be overwritten.</span></span> <span data-ttu-id="e1f7b-206">展開でロックされているファイルをリリースするには、次の方法でアプリ プールを停止します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-206">To release locked files in a deployment, stop the app pool:</span></span>

* <span data-ttu-id="e1f7b-207">サーバー上の IIS マネージャーで手動により停止します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-207">Manually in the IIS Manager on the server.</span></span>
* <span data-ttu-id="e1f7b-208">Web 配置を使用し、プロジェクト ファイル内の `Microsoft.NET.Sdk.Web` を参照して停止します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-208">Using Web Deploy and referencing `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="e1f7b-209">*app_offline.htm* ファイルは、Web アプリのディレクトリのルートに配置されます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-209">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="e1f7b-210">ファイルが存在する場合、ASP.NET Core モジュールはアプリを正常にシャットダウンし、展開中に *app_offline.htm* ファイルを提供します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-210">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="e1f7b-211">詳細については、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module#appofflinehtm)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-211">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>
* <span data-ttu-id="e1f7b-212">PowerShell を使用して、アプリ プールを停止して再起動します (PowerShell 5 以降が必要)。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-212">Use PowerShell to stop and restart the app pool (requires PowerShell 5 or later):</span></span>
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

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="e1f7b-213">Visual Studio からのアプリの展開</span><span class="sxs-lookup"><span data-stu-id="e1f7b-213">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="e1f7b-214">Web 配置に使用する発行プロファイルの作成方法については、「[Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)」 (ASP.NET Core アプリ展開用の Visual Studio の発行プロファイル) のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-214">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="e1f7b-215">ホスティング プロバイダーから発行プロファイルまたは発行プロファイルを作成するためのサポートが提供されている場合は、そのプロファイルをダウンロードし、Visual Studio の **[発行]** ダイアログを使用してインポートします。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-215">If the hosting provider supplies a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![[発行] ダイアログ ページ](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="e1f7b-217">Visual Studio 外部での Web 配置</span><span class="sxs-lookup"><span data-stu-id="e1f7b-217">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="e1f7b-218">Visual Studio の外部で、コマンド ラインから [Web 配置](/iis/publish/using-web-deploy/introduction-to-web-deploy)を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-218">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="e1f7b-219">詳細については、[Web 配置ツール](/iis/publish/using-web-deploy/use-the-web-deployment-tool)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-219">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="e1f7b-220">Web 配置の代替手段</span><span class="sxs-lookup"><span data-stu-id="e1f7b-220">Alternatives to Web Deploy</span></span>

<span data-ttu-id="e1f7b-221">Xcopy、Robocopy、PowerShell などの任意の方法を使用して、ホスティング システムにアプリを移動します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-221">Use any of several methods to move the app to the hosting system, such as Xcopy, Robocopy, or PowerShell.</span></span> <span data-ttu-id="e1f7b-222">Visual Studio ユーザーは、[発行サンプル](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md)を利用できます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-222">Visual Studio users may use the [Publish Samples](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="e1f7b-223">Web サイトを閲覧する</span><span class="sxs-lookup"><span data-stu-id="e1f7b-223">Browse the website</span></span>

![Microsoft Edge ブラウザーに IIS のスタートアップ ページが読み込まれています。](index/_static/browsewebsite.png)

## <a name="data-protection"></a><span data-ttu-id="e1f7b-225">データの保護</span><span class="sxs-lookup"><span data-stu-id="e1f7b-225">Data protection</span></span>

<span data-ttu-id="e1f7b-226">データ保護は、認証で使用されるものを含め、いくつかの ASP.NET ミドルウェアによって使用されます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-226">Data Protection is used by several ASP.NET middlewares, including those used in authentication.</span></span> <span data-ttu-id="e1f7b-227">データ保護 API がユーザーのコードから呼び出されない場合でも、配置スクリプトを使用するかユーザー コードで、永続的なキー ストアを作成するようにデータ保護を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-227">Even if Data Protection APIs aren't called from user's code, Data Protection should be configured with a deployment script or in user code to create a persistent key store.</span></span> <span data-ttu-id="e1f7b-228">データ保護を構成しない場合、既定でキーはメモリ内に保持され、アプリが再起動すると破棄されます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-228">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="e1f7b-229">キーリングがメモリに格納されている場合、アプリを再起動すると次のことが行われます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-229">If the keyring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="e1f7b-230">すべてのフォーム認証トークンは無効になります。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-230">All forms authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="e1f7b-231">ユーザーは、次回の要求時に再度サインインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-231">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="e1f7b-232">キーリングで保護されているデータは、いずれも復号化できなくなります。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-232">Any data protected with the keyring can no longer be decrypted.</span></span>

<span data-ttu-id="e1f7b-233">IIS でのデータ保護を構成するには、次の**いずれか**の方法を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-233">To configure Data Protection under IIS, use **one** of the following approaches:</span></span>

### <a name="create-a-data-protection-registry-hive"></a><span data-ttu-id="e1f7b-234">データ保護のレジストリ ハイブを作成する</span><span class="sxs-lookup"><span data-stu-id="e1f7b-234">Create a Data Protection Registry Hive</span></span>

<span data-ttu-id="e1f7b-235">ASP.NET アプリで使用されるデータ保護キーは、アプリ外部のレジストリ ハイブに格納されます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-235">Data Protection keys used by ASP.NET apps are stored in registry hives external to the apps.</span></span> <span data-ttu-id="e1f7b-236">特定のアプリのキーを保持するには、アプリ プール用のレジストリ ハイブを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-236">To persist the keys for a given app, create a registry hive for the app pool.</span></span>

<span data-ttu-id="e1f7b-237">非 Web ファーム IIS のスタンドアロン インストールの場合、ASP.NET Core アプリで使用するアプリ プールごとに、[データ保護の PowerShell スクリプト Provision-AutoGenKeys.ps1](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) を使用できます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-237">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="e1f7b-238">このスクリプトは、HKLM レジストリに特別なレジストリ キーを作成します。そのキーは、ワーカー プロセス アカウントに対してのみ、ACL に追加されます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-238">This script creates a special registry key in the HKLM registry that is ACLed only to the worker process account.</span></span> <span data-ttu-id="e1f7b-239">キーは保存時に、DPAPI を使用して、コンピューター全体に適用するキーによって暗号化されます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-239">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

<span data-ttu-id="e1f7b-240">Web ファームのシナリオでは、UNC パスを使用してデータ保護キー リングを格納するようにアプリを構成できます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-240">In web farm scenarios, an app can be configured to use a UNC path to store its data protection keyring.</span></span> <span data-ttu-id="e1f7b-241">既定では、データ保護キーは暗号化されません。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-241">By default, the data protection keys are not encrypted.</span></span> <span data-ttu-id="e1f7b-242">このような共有のファイルのアクセス許可が、アプリを実行する Windows アカウントに限定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-242">Ensure that the file permissions for such a share are limited to the Windows account the app runs as.</span></span> <span data-ttu-id="e1f7b-243">また、保存時のキーを保護するために、X509 証明書を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-243">In addition, an X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="e1f7b-244">ユーザーが証明書をアップデートできるメカニズムを検討している場合、ユーザーの信頼できる証明書ストアに証明書を配置し、ユーザーのアプリが実行されるすべてのコンピューターで証明書を利用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-244">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="e1f7b-245">詳細については、「[データ保護の構成](xref:security/data-protection/configuration/overview)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-245">See [Configuring Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a><span data-ttu-id="e1f7b-246">ユーザー プロファイルを読み込むように IIS アプリケーション プールを構成する</span><span class="sxs-lookup"><span data-stu-id="e1f7b-246">Configure the IIS Application Pool to load the user profile</span></span>

<span data-ttu-id="e1f7b-247">この設定は、アプリ プールの **[詳細設定]** の **[プロセス モデル]** セクションにあります。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-247">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="e1f7b-248">[ユーザー プロファイルの読み込み] を `True` に設定します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-248">Set Load User Profile to `True`.</span></span> <span data-ttu-id="e1f7b-249">これによりキーはユーザー プロファイル ディレクトリに格納され、DPAPI を使用して、アプリ プールで使うユーザー アカウントに固有のキーによって保護されます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-249">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used for the app pool.</span></span>

### <a name="use-the-file-system-as-a-key-ring-store"></a><span data-ttu-id="e1f7b-250">ファイル システムをキー リング ストアとして使用する</span><span class="sxs-lookup"><span data-stu-id="e1f7b-250">Use the file system as a key ring store</span></span>

<span data-ttu-id="e1f7b-251">[ファイル システムをキー リング ストアとして使用](xref:security/data-protection/configuration/overview)するようにアプリ コードを調整します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-251">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="e1f7b-252">X509 証明書を使用してキー リングを保護し、その証明書が信頼された証明書であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-252">Use an X509 certificate to protect the keyring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="e1f7b-253">証明書が自己署名証明書である場合は、信頼されたルート ストアにその証明書を配置します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-253">If it's a self signed certificate, place the certificate in the Trusted Root store.</span></span>

<span data-ttu-id="e1f7b-254">Web ファームで IIS を使用する場合:</span><span class="sxs-lookup"><span data-stu-id="e1f7b-254">When using IIS in a web farm:</span></span>

* <span data-ttu-id="e1f7b-255">すべてのコンピューターがアクセスできるファイル共有を使用します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-255">Use a file share that all machines can access.</span></span>
* <span data-ttu-id="e1f7b-256">X509 証明書を各コンピューターに配置します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-256">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="e1f7b-257">[コード内にデータ保護](xref:security/data-protection/configuration/overview)を構成します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-257">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

### <a name="set-a-machine-wide-policy-for-data-protection"></a><span data-ttu-id="e1f7b-258">コンピューター全体に適用するデータ保護ポリシーを設定する</span><span class="sxs-lookup"><span data-stu-id="e1f7b-258">Set a machine-wide policy for data protection</span></span>

<span data-ttu-id="e1f7b-259">データ保護システムでは、データ保護 API を使用するすべてのアプリに対して、[コンピューター全体に適用する既定のポリシー](xref:security/data-protection/configuration/machine-wide-policy)を設定するためのサポートは限定的です。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-259">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="e1f7b-260">詳細については、[データ保護](xref:security/data-protection/index)に関するドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-260">See the [data protection](xref:security/data-protection/index) documentation for more details.</span></span>

## <a name="configuration-of-sub-applications"></a><span data-ttu-id="e1f7b-261">サブアプリケーションの構成</span><span class="sxs-lookup"><span data-stu-id="e1f7b-261">Configuration of sub-applications</span></span>

<span data-ttu-id="e1f7b-262">ルート アプリの下に追加したサブアプリに、ハンドラーとして ASP.NET Core モジュールを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-262">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="e1f7b-263">このモジュールをサブアプリの *web.config* ファイルにハンドラーとして追加すると、サブアプリを閲覧しようとすると、エラーのある構成ファイルを参照する 500.19 (内部サーバー エラー) が返されます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-263">If the module is added as a handler in a sub-app's *web.config* file, a 500.19 (Internal Server Error) referencing the faulty config file is received when attempting to browse the sub-app.</span></span> <span data-ttu-id="e1f7b-264">次の例は、ASP.NET Core サブアプリの発行済み *web.config* ファイルの内容を示しています。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-264">The following example shows the contents of a published *web.config* file for an ASP.NET Core sub-app:</span></span>

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

<span data-ttu-id="e1f7b-265">ASP.NET Core アプリの下に ASP.NET Core 以外のサブアプリをホストする場合は、サブアプリの *web.config* ファイルにある継承されたハンドラーを明示的に削除します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-265">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="e1f7b-266">ASP.NET Core モジュールを構成する方法の詳細については、「[ASP.NET Core モジュールの概要](xref:fundamentals/servers/aspnet-core-module)」と「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-266">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="e1f7b-267">web.config による IIS の構成</span><span class="sxs-lookup"><span data-stu-id="e1f7b-267">Configuration of IIS with web.config</span></span>

<span data-ttu-id="e1f7b-268">IIS の構成は、リバース プロキシ構成に適用されるこれらの IIS 機能の *web.config*の **\<system.webServer>** セクションの影響を受けます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-268">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="e1f7b-269">IIS が動的な圧縮を使用するようにシステム レベルで構成されている場合、アプリの *web.config* ファイルで **\<urlCompression>** 要素を使用して、アプリに対してその設定を無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-269">If IIS is configured at the system level to use dynamic compression, that setting can be disabled for an app with the **\<urlCompression>** element in the app's *web.config* file.</span></span> <span data-ttu-id="e1f7b-270">詳細については、[\<system.webServer> の構成リファレンス](https://docs.microsoft.com/iis/configuration/system.webServer/)、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module)」および「[IIS モジュールと ASP.NET Core の使用](xref:host-and-deploy/iis/modules)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-270">For more information, see the [configuration reference for \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module) and [Using IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="e1f7b-271">分離されたアプリ プール (IIS 10.0 以降でサポートされています) で実行する個別アプリに対して環境変数を設定する必要がある場合は、IIS のリファレンス ドキュメントで、[環境変数\<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) のトピックにある *AppCmd.exe コマンド*のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-271">If there's a need to set environment variables for individual apps running in isolated app pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="e1f7b-272">web.config の構成のセクション</span><span class="sxs-lookup"><span data-stu-id="e1f7b-272">Configuration sections of web.config</span></span>

<span data-ttu-id="e1f7b-273">*web.config* の ASP.NET フレームワーク アプリの構成セクションは、ASP.NET Core アプリの構成では使用されません。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-273">Configruation sections of ASP.NET Framework apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="e1f7b-274">**\<system.web >**</span><span class="sxs-lookup"><span data-stu-id="e1f7b-274">**\<system.web>**</span></span>
* <span data-ttu-id="e1f7b-275">**\<appSettings>**</span><span class="sxs-lookup"><span data-stu-id="e1f7b-275">**\<appSettings>**</span></span>
* <span data-ttu-id="e1f7b-276">**\<connectionStrings>**</span><span class="sxs-lookup"><span data-stu-id="e1f7b-276">**\<connectionStrings>**</span></span>
* <span data-ttu-id="e1f7b-277">**\<location>**</span><span class="sxs-lookup"><span data-stu-id="e1f7b-277">**\<location>**</span></span>

<span data-ttu-id="e1f7b-278">ASP.NET Core アプリは、他の構成プロバイダーを使用して構成されます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-278">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="e1f7b-279">詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-279">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="e1f7b-280">アプリケーション プール</span><span class="sxs-lookup"><span data-stu-id="e1f7b-280">Application Pools</span></span>

<span data-ttu-id="e1f7b-281">1 つのシステムで複数の Web サイトをホストする場合は、アプリをそれぞれ専用のアプリ プールで実行して、アプリを相互に分離します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-281">When hosting multiple websites on a single system, isolate the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="e1f7b-282">IIS の **[Web サイトの追加]** ダイアログはこの構成の既定です。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-282">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="e1f7b-283">**[サイト名]** を指定すると、入力したテキストが自動的に **[アプリケーション プール]** テキストボックスに設定されます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-283">When **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="e1f7b-284">Web サイトを追加するときに、そのサイト名を使用して新しいアプリ プールが作成されます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-284">A new app pool is created using the site name when the website is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="e1f7b-285">アプリケーション プール ID</span><span class="sxs-lookup"><span data-stu-id="e1f7b-285">Application Pool Identity</span></span>

<span data-ttu-id="e1f7b-286">アプリ プール ID アカウントを使用すると、ドメインやローカル アカウントを作成して管理する必要なく、一意のアカウントでアプリを実行できます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-286">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="e1f7b-287">IIS 8.0 以降の IIS 管理者ワーカー プロセス (WAS) は、新しいアプリ プールの名前で仮想アカウントを作成し、既定によってアプリ プールのワーカー プロセスをこのアカウントで実行します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-287">On IIS 8.0+, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="e1f7b-288">IIS 管理コンソールにあるアプリ プールの **[詳細設定]** で、**ID** が **ApplicationPoolIdentity** を使用するように設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-288">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![アプリケーション プールの [詳細設定] ダイアログ](index/_static/apppool-identity.png)

<span data-ttu-id="e1f7b-290">IIS 管理プロセスは、Windows セキュリティ システムでのアプリ プールの名前を使用して、セキュリティで保護された識別子を作成します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-290">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="e1f7b-291">この ID を使用してリソースを保護することができます。ただし、この ID は実際のユーザー アカウントではなく、Windows ユーザーの管理コンソールに表示されません。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-291">Resources can be secured by using this identity; however, this identity isn't a real user account and won't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="e1f7b-292">アプリに対する IIS ワーカー プロセスのアクセス許可を昇格させる必要がある場合は、アプリを含むディレクトリのアクセス制御リスト (ACL) を変更します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-292">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="e1f7b-293">エクスプローラーを開き、そのディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-293">Open Windows Explorer and navigate to the directory.</span></span>

2. <span data-ttu-id="e1f7b-294">そのディレクトリを右クリックし、**[プロパティ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-294">Right-click on the directory and select **Properties**.</span></span>

3. <span data-ttu-id="e1f7b-295">**[セキュリティ]** タブの **[編集]** ボタンを選択し、**[追加]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-295">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

4. <span data-ttu-id="e1f7b-296">**[場所]** ボタンを選択し、システムが選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-296">Select the **Locations** button and make sure the system is selected.</span></span>

5. <span data-ttu-id="e1f7b-297">**[選択するオブジェクト名を入力してください]** テキストボックスに「**IIS AppPool\DefaultAppPool**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-297">Enter **IIS AppPool\DefaultAppPool** in **Enter the object names to select** textbox.</span></span>

   ![アプリ フォルダーの [ユーザーまたはグループの選択] ダイアログ](index/_static/select-users-or-groups-1.png)

6. <span data-ttu-id="e1f7b-299">**[名前の確認]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-299">Select the **Check Names** button.</span></span> <span data-ttu-id="e1f7b-300">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-300">Select **OK**.</span></span>

   ![アプリ フォルダーの [ユーザーまたはグループの選択] ダイアログ](index/_static/select-users-or-groups-2.png)

<span data-ttu-id="e1f7b-302">これは、**ICACLS** ツールを使用してコマンド プロンプトからも実行できます。</span><span class="sxs-lookup"><span data-stu-id="e1f7b-302">This can also be accomplished via a command prompt using the **ICACLS** tool:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a><span data-ttu-id="e1f7b-303">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="e1f7b-303">Additional resources</span></span>

* [<span data-ttu-id="e1f7b-304">IIS での ASP.NET Core のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="e1f7b-304">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="e1f7b-305">Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス</span><span class="sxs-lookup"><span data-stu-id="e1f7b-305">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="e1f7b-306">ASP.NET Core モジュールの概要</span><span class="sxs-lookup"><span data-stu-id="e1f7b-306">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="e1f7b-307">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="e1f7b-307">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="e1f7b-308">IIS モジュールと ASP.NET Core の使用</span><span class="sxs-lookup"><span data-stu-id="e1f7b-308">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="e1f7b-309">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="e1f7b-309">Introduction to ASP.NET Core</span></span>](../index.md)
* [<span data-ttu-id="e1f7b-310">Microsoft IIS 公式サイト</span><span class="sxs-lookup"><span data-stu-id="e1f7b-310">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="e1f7b-311">Microsoft TechNet ライブラリ: Windows Server</span><span class="sxs-lookup"><span data-stu-id="e1f7b-311">Microsoft TechNet Library: Windows Server</span></span>](https://docs.microsoft.com/windows-server/windows-server-versions)
