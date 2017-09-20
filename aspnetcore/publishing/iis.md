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
ms.openlocfilehash: 8ffadc1dede4053faa129a3b224aace901e70e14
ms.sourcegitcommit: ad01283f299d346cf757c4f4744c48634dc27e73
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-windows-with-iis-and-deploy-to-it"></a><span data-ttu-id="fd399-104">IIS を使用している Windows に ASP.NET Core 用ホスティング環境をセットアップし、その環境に展開する</span><span class="sxs-lookup"><span data-stu-id="fd399-104">Set up a hosting environment for ASP.NET Core on Windows with IIS, and deploy to it</span></span>

<span data-ttu-id="fd399-105">執筆者: [Luke Latham](https://github.com/GuardRex)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fd399-105">By [Luke Latham](https://github.com/GuardRex) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="fd399-106">サポートされるオペレーティング システム</span><span class="sxs-lookup"><span data-stu-id="fd399-106">Supported operating systems</span></span>

<span data-ttu-id="fd399-107">次のオペレーティング システムがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="fd399-107">The following operating systems are supported:</span></span>

* <span data-ttu-id="fd399-108">Windows 7 およびそれ以降</span><span class="sxs-lookup"><span data-stu-id="fd399-108">Windows 7 and newer</span></span>

* <span data-ttu-id="fd399-109">Windows Server 2008 R2 およびそれ以降&#8224;</span><span class="sxs-lookup"><span data-stu-id="fd399-109">Windows Server 2008 R2 and newer&#8224;</span></span>

<span data-ttu-id="fd399-110">&#8224;概念的には、このドキュメントで説明する IIS 構成は、Nano Server IIS で ASP.NET Core アプリケーションをホストする場合にも当てはまりますが、詳細な手順については「[ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server)」 (IIS を使用した Nano Server での ASP.NET Core) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-110">&#8224;Conceptually, the IIS configuration described in this document also applies to hosting ASP.NET Core applications on Nano Server IIS, but refer to [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) for specific instructions.</span></span>

<span data-ttu-id="fd399-111">[WebListener サーバー](xref:fundamentals/servers/weblistener)は、IIS を使用したリバース プロキシ構成では動作しません。</span><span class="sxs-lookup"><span data-stu-id="fd399-111">[WebListener server](xref:fundamentals/servers/weblistener) will not work in a reverse-proxy configuration with IIS.</span></span> <span data-ttu-id="fd399-112">[Kestrel サーバー](xref:fundamentals/servers/kestrel)を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-112">You must use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="fd399-113">IIS 構成</span><span class="sxs-lookup"><span data-stu-id="fd399-113">IIS configuration</span></span>

<span data-ttu-id="fd399-114">**Web サーバー (IIS)** の役割を有効にし、役割のサービスを設定します。</span><span class="sxs-lookup"><span data-stu-id="fd399-114">Enable the **Web Server (IIS)** role and establish role services.</span></span>

### <a name="windows-desktop-operating-systems"></a><span data-ttu-id="fd399-115">Windows デスクトップ オペレーティング システム</span><span class="sxs-lookup"><span data-stu-id="fd399-115">Windows desktop operating systems</span></span>

<span data-ttu-id="fd399-116">**[コントロール パネル] > [プログラム] > [プログラムと機能] > [Windows の機能の有効化または無効化]** (画面の左側) に移動します。</span><span class="sxs-lookup"><span data-stu-id="fd399-116">Navigate to **Control Panel > Programs > Programs and Features > Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="fd399-117">**[インターネット インフォメーション サービス]** と **[Web 管理ツール]** のグループを開きます。</span><span class="sxs-lookup"><span data-stu-id="fd399-117">Open the group for **Internet Information Services** and **Web Management Tools**.</span></span> <span data-ttu-id="fd399-118">**[IIS 管理コンソール]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="fd399-118">Check the box for **IIS Management Console**.</span></span> <span data-ttu-id="fd399-119">**[World Wide Web サービス]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="fd399-119">Check the box for **World Wide Web Services**.</span></span> <span data-ttu-id="fd399-120">**[World Wide Web サービス]** の既定の機能をそのまま使用するか、ニーズに合わせて IIS 機能をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="fd399-120">Accept the default features for **World Wide Web Services** or customize the IIS features to suit your needs.</span></span>

![[Windows の機能] で [IIS 管理コンソール] と [World Wide Web サービス] を選択します。](iis/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a><span data-ttu-id="fd399-122">Windows Server オペレーティング システム</span><span class="sxs-lookup"><span data-stu-id="fd399-122">Windows Server operating systems</span></span>

<span data-ttu-id="fd399-123">サーバー オペレーティング システムでは、**[管理]** メニューから**役割と機能の追加**ウィザードを使用するか、**サーバー マネージャー**にあるリンクを使用します。</span><span class="sxs-lookup"><span data-stu-id="fd399-123">For server operating systems, use the **Add Roles and Features** wizard via the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="fd399-124">**[サーバーの役割]** のステップで、**[Web サーバー (IIS)]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="fd399-124">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

![[サーバーの役割の選択] のステップで Web サーバー IIS の役割を選択します。](iis/_static/server-roles-ws2016.png)

<span data-ttu-id="fd399-126">**[役割サービス]** のステップで、希望する IIS の役割サービスを選択するか、既定の役割サービスをそのまま使用します。</span><span class="sxs-lookup"><span data-stu-id="fd399-126">On the **Role services** step, select the IIS role services you desire or accept the default role services provided.</span></span>

![[役割サービスの選択] のステップで既定の役割サービスを選択します。](iis/_static/role-services-ws2016.png)

<span data-ttu-id="fd399-128">**[確認]** のステップを経て Web サーバーの役割とサービスをインストールします。</span><span class="sxs-lookup"><span data-stu-id="fd399-128">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="fd399-129">Web サーバー (IIS) の役割をインストールした後、サーバーと IIS の再起動は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="fd399-129">A server/IIS restart is not required after installing the Web Server (IIS) role.</span></span>

## <a name="install-the-net-core-windows-server-hosting-bundle"></a><span data-ttu-id="fd399-130">.NET Core Windows Server ホスティング バンドルのインストール</span><span class="sxs-lookup"><span data-stu-id="fd399-130">Install the .NET Core Windows Server Hosting bundle</span></span>

1. <span data-ttu-id="fd399-131">ホスティング システムに [.NET Core Windows Server ホスティング バンドル](https://aka.ms/dotnetcore.2.0.0-windowshosting)をインストールします。</span><span class="sxs-lookup"><span data-stu-id="fd399-131">Install the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore.2.0.0-windowshosting) on the hosting system.</span></span> <span data-ttu-id="fd399-132">このバンドルをインストールすることで、.NET Core ランタイム、.NET Core ライブラリ、[ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="fd399-132">The bundle will install the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="fd399-133">このモジュールは、IIS と Kestrel サーバーの間にリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="fd399-133">The module creates the reverse-proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="fd399-134">注: システムにインターネット接続が設定されていない場合は、.NET Core Windows Server ホスティング モジュールをインストールする前に、*[Microsoft Visual C++ 2015 再頒布可能パッケージ](https://www.microsoft.com/download/details.aspx?id=53840)*を入手してインストールしてください。</span><span class="sxs-lookup"><span data-stu-id="fd399-134">Note: If the system doesn't have an Internet connection, obtain and install the *[Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840)* before installing the .NET Core Windows Server Hosting bundle.</span></span>

2. <span data-ttu-id="fd399-135">システムを再起動するか、コマンド プロンプトから **net stop was /y** の後に続けて **net start w3svc** を実行して、システム PATH への変更を適用します。</span><span class="sxs-lookup"><span data-stu-id="fd399-135">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt to pick up a change to the system PATH.</span></span>

> [!NOTE]
> <span data-ttu-id="fd399-136">IIS 共有構成を使用する場合は、「[ASP.NET Core Module with IIS Shared Configuration](xref:hosting/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)」 (IIS 共有構成の ASP.NET Core モジュール) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-136">If you use an IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:hosting/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="fd399-137">Visual Studio で発行する場合の Web 配置のインストール</span><span class="sxs-lookup"><span data-stu-id="fd399-137">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="fd399-138">Visual Studio の Web 配置を使用してアプリケーションを展開する場合は、ホスティング システムに最新バージョンの Web 配置をインストールします。</span><span class="sxs-lookup"><span data-stu-id="fd399-138">If you intend to deploy your applications with Web Deploy in Visual Studio, install the latest version of Web Deploy on the hosting system.</span></span> <span data-ttu-id="fd399-139">Web 配置をインストールするには、[Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) を使用するか、[Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=43717)からインストーラーを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="fd399-139">To install Web Deploy, you can use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="fd399-140">WebPI を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="fd399-140">The preferred method is to use WebPI.</span></span> <span data-ttu-id="fd399-141">WebPI は、スタンドアロンのセットアップとホスティング プロバイダー向けの構成を提供します。</span><span class="sxs-lookup"><span data-stu-id="fd399-141">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="fd399-142">アプリケーション構成</span><span class="sxs-lookup"><span data-stu-id="fd399-142">Application configuration</span></span>

### <a name="enabling-the-iisintegration-components"></a><span data-ttu-id="fd399-143">IISIntegration コンポーネントを有効にする</span><span class="sxs-lookup"><span data-stu-id="fd399-143">Enabling the IISIntegration components</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fd399-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd399-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fd399-145">一般的な *Program.cs* は [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) を呼び出してホストの設定を開始します。</span><span class="sxs-lookup"><span data-stu-id="fd399-145">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to begin setting up a host.</span></span> <span data-ttu-id="fd399-146">`CreateDefaultBuilder` は [Kestrel](xref:fundamentals/servers/kestrel) を Web サーバーとして構成し、ベース パスとポートを [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)に構成することで、IIS 統合を有効にします。</span><span class="sxs-lookup"><span data-stu-id="fd399-146">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fd399-147">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fd399-147">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fd399-148">アプリケーションの依存関係に [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) パッケージへの依存関係を含めます。</span><span class="sxs-lookup"><span data-stu-id="fd399-148">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the application dependencies.</span></span> <span data-ttu-id="fd399-149">*UseIISIntegration* 拡張メソッドを *WebHostBuilder* に追加して、IIS 統合ミドルウェアをアプリケーションに組み込みます。</span><span class="sxs-lookup"><span data-stu-id="fd399-149">Incorporate IIS Integration middleware into the application by adding the *UseIISIntegration* extension method to *WebHostBuilder*:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="fd399-150">`UseKestrel` と `UseIISIntegration` の両方が必要です。</span><span class="sxs-lookup"><span data-stu-id="fd399-150">Both `UseKestrel` and `UseIISIntegration` are required.</span></span> <span data-ttu-id="fd399-151">*UseIISIntegration()* を呼び出すコードはコードの移植性に影響しません。</span><span class="sxs-lookup"><span data-stu-id="fd399-151">Code calling *UseIISIntegration* doesn't affect code portability.</span></span> <span data-ttu-id="fd399-152">アプリが IIS の背後で実行されていない場合 (たとえば、アプリが Kestrel で直接実行されている場合)、`UseIISIntegration` は機能しません。</span><span class="sxs-lookup"><span data-stu-id="fd399-152">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` no-ops.</span></span>

---

<span data-ttu-id="fd399-153">ホスティングの詳細については、「[Hosting in ASP.NET Core](xref:fundamentals/hosting)」(ASP.NET Core でのホスティング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-153">For more information on hosting, see [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span></span>

### <a name="iis-options"></a><span data-ttu-id="fd399-154">IIS のオプション</span><span class="sxs-lookup"><span data-stu-id="fd399-154">IIS options</span></span>

<span data-ttu-id="fd399-155">*IISIntegration* サービスのオプションを構成するには、*IISOptions* のサービス構成を *ConfigureServices*に含めます。</span><span class="sxs-lookup"><span data-stu-id="fd399-155">To configure *IISIntegration* service options, include a service configuration for *IISOptions* in *ConfigureServices*:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| <span data-ttu-id="fd399-156">オプション</span><span class="sxs-lookup"><span data-stu-id="fd399-156">Option</span></span>                         | <span data-ttu-id="fd399-157">既定値</span><span class="sxs-lookup"><span data-stu-id="fd399-157">Default</span></span> | <span data-ttu-id="fd399-158">設定</span><span class="sxs-lookup"><span data-stu-id="fd399-158">Setting</span></span> |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="fd399-159">`true` の場合、認証ミドルウェアが `HttpContext.User` を設定し、一般的な課題に応答します。</span><span class="sxs-lookup"><span data-stu-id="fd399-159">If `true`, the authentication middleware sets the `HttpContext.User` and responds to generic challenges.</span></span> <span data-ttu-id="fd399-160">`false` の場合、認証ミドルウェアは ID (`HttpContext.User`) を提供するだけで、`AuthenticationScheme` によって明示的に要求されたときに課題に応答します。</span><span class="sxs-lookup"><span data-stu-id="fd399-160">If `false`, the authentication middleware only provides an identity (`HttpContext.User`) and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="fd399-161">`AutomaticAuthentication` を機能させるためには、IIS で Windows 認証を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-161">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="fd399-162">ログイン ページでユーザーに表示名が表示されるように設定します。</span><span class="sxs-lookup"><span data-stu-id="fd399-162">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="fd399-163">`true` の場合、`MS-ASPNETCORE-CLIENTCERT` 要求ヘッダーが指定されていると、`HttpContext.Connection.ClientCertificate` が設定されます。</span><span class="sxs-lookup"><span data-stu-id="fd399-163">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="webconfig"></a><span data-ttu-id="fd399-164">web.config</span><span class="sxs-lookup"><span data-stu-id="fd399-164">web.config</span></span>

<span data-ttu-id="fd399-165">*web.config* ファイルでは、ASP.NET Core モジュールを構成し、その他の IIS 構成を指定します。</span><span class="sxs-lookup"><span data-stu-id="fd399-165">The *web.config* file configures the ASP.NET Core Module and provides other IIS configuration.</span></span> <span data-ttu-id="fd399-166">*web.config* の作成、変換、発行は `Microsoft.NET.Sdk.Web` によって処理されます。この SDK は、*.csproj* ファイル (`<Project Sdk="Microsoft.NET.Sdk.Web">`) の先頭でプロジェクトの SDK を設定した場合に組み込まれます。</span><span class="sxs-lookup"><span data-stu-id="fd399-166">Creating, transforming, and publishing *web.config* is handled by `Microsoft.NET.Sdk.Web`, which is included when you set your project's SDK at the top of your *.csproj* file, `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span></span> <span data-ttu-id="fd399-167">MSBuild のターゲットによって *web.config* ファイルが変換されないようにするため、`true` に設定した **\<IsTransformWebConfigDisabled>** プロパティをプロジェクト ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="fd399-167">To prevent the MSBuild target from transforming your *web.config* file, add the **\<IsTransformWebConfigDisabled>** property to your project file with a setting of `true`:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="fd399-168">プロジェクトに *web.config* ファイルが存在せず、*dotnet publish* または Visual Studio の公開機能を使用して発行する場合、ファイルは発行された出力内に自動的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="fd399-168">If you don't have a *web.config* file in the project when you publish with *dotnet publish* or with Visual Studio publish, the file is created for you in published output.</span></span> <span data-ttu-id="fd399-169">プロジェクトにこのファイルが含まれている場合、そのファイルは ASP.NET Core モジュールを構成するための正しい *processPath* と *arguments* を使用して変換され、発行された出力に移行されます。</span><span class="sxs-lookup"><span data-stu-id="fd399-169">If you have the file in your project, it's transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="fd399-170">ファイルに含めた IIS 構成の設定は、変換後も変わりません。</span><span class="sxs-lookup"><span data-stu-id="fd399-170">The transformation doesn't touch IIS configuration settings that you've included in the file.</span></span>

## <a name="create-the-iis-website"></a><span data-ttu-id="fd399-171">IIS Web サイトを作成する</span><span class="sxs-lookup"><span data-stu-id="fd399-171">Create the IIS Website</span></span>

1. <span data-ttu-id="fd399-172">ターゲットの IIS システムで、「[ディレクトリ構造](xref:hosting/directory-structure)」で説明されている、アプリケーションの公開フォルダーとファイルを格納するためのフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="fd399-172">On the target IIS system, create a folder to contain the application's published folders and files, which are described in [Directory Structure](xref:hosting/directory-structure).</span></span>

2. <span data-ttu-id="fd399-173">作成したフォルダー内に、アプリケーション ログを保存するための *logs* フォルダーを作成します (ログ作成を有効にする場合)。</span><span class="sxs-lookup"><span data-stu-id="fd399-173">Within the folder you created, create a *logs* folder to hold application logs (if you plan to enable logging).</span></span> <span data-ttu-id="fd399-174">ペイロードに *logs* フォルダーを含めてアプリケーションを配置する場合は、このステップをスキップすることができます。</span><span class="sxs-lookup"><span data-stu-id="fd399-174">If you plan to deploy your application with a *logs* folder in the payload, you may skip this step.</span></span>

3. <span data-ttu-id="fd399-175">**IIS マネージャー**で新しい Web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="fd399-175">In **IIS Manager**, create a new website.</span></span> <span data-ttu-id="fd399-176">**[サイト名]** を指定し、**[物理パス]** には作成したアプリケーションの配置フォルダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="fd399-176">Provide a **Site name** and set the **Physical path** to the application's deployment folder that you created.</span></span> <span data-ttu-id="fd399-177">**[バインド]** の構成を指定して Web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="fd399-177">Provide the **Binding** configuration and create the website.</span></span>

4. <span data-ttu-id="fd399-178">アプリケーション プールを **[マネージ コードなし]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="fd399-178">Set the application pool to **No Managed Code**.</span></span> <span data-ttu-id="fd399-179">ASP.NET Core は、別個のプロセスで実行され、ランタイムを管理します。</span><span class="sxs-lookup"><span data-stu-id="fd399-179">ASP.NET Core runs in a separate process and manages the runtime.</span></span>

5. <span data-ttu-id="fd399-180">**[Web サイトの追加]** ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="fd399-180">Open the **Add Website** window.</span></span>

   ![サイトのコンテキスト メニューで [Web サイトの追加] をクリックします。](iis/_static/add-website-context-menu-ws2016.png)

6. <span data-ttu-id="fd399-182">Web サイトを構成します。</span><span class="sxs-lookup"><span data-stu-id="fd399-182">Configure the website.</span></span>

   ![[Web サイトの追加] のステップでサイト名、物理パス、ホスト名を指定します。](iis/_static/add-website-ws2016.png)

7. <span data-ttu-id="fd399-184">**[アプリケーション プール]** パネルで、**[アプリケーション プールの編集]** ウィンドウを開きます。そのためには、Web サイトのアプリケーション プール上で右クリックし、ポップアップ メニューから **[基本設定]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="fd399-184">In the **Application Pools** panel, open the **Edit Application Pool** window by right-clicking on the website's application pool and selecting **Basic Settings...** from the popup menu.</span></span>

   ![アプリケーション プールのコンテキスト メニューから [基本設定] を選択します。](iis/_static/apppools-basic-settings-ws2016.png)

8. <span data-ttu-id="fd399-186">**[.NET CLR バージョン]** を **[マネージ コードなし]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="fd399-186">Set the **.NET CLR version** to **No Managed Code**.</span></span>

   ![.NET CLR バージョンとして [マネージ コードなし] を設定します。](iis/_static/edit-apppool-ws2016.png)
     
    <span data-ttu-id="fd399-188">注: **[.NET CLR バージョン]** の **[マネージ コードなし]** への設定は、任意です。</span><span class="sxs-lookup"><span data-stu-id="fd399-188">Note: Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span> <span data-ttu-id="fd399-189">ASP.NET Core を使用するためにデスクトップ CLR を読み込む必要はありません。</span><span class="sxs-lookup"><span data-stu-id="fd399-189">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span>

9. <span data-ttu-id="fd399-190">プロセス モデル ID に適切なアクセス許可があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-190">Confirm the process model identity has the proper permissions.</span></span>

    <span data-ttu-id="fd399-191">アプリケーション プールの既定の ID (**[プロセス モデル]** > **[ID]**) を **ApplicationPoolIdentity** から別の ID に変更した場合は、アプリケーションのフォルダー、データベース、その他の必要なリソースにアクセスするために要求されるアクセス許可が新しい ID に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-191">If you change the default identity of the application pool (**Process Model** > **Identity**) from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the application's folder, database, and other required resources.</span></span>
   
## <a name="deploy-the-application"></a><span data-ttu-id="fd399-192">アプリケーションを展開する</span><span class="sxs-lookup"><span data-stu-id="fd399-192">Deploy the application</span></span>
<span data-ttu-id="fd399-193">ターゲット IIS システム上に作成したフォルダーにアプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="fd399-193">Deploy the application to the folder you created on the target IIS system.</span></span> <span data-ttu-id="fd399-194">Web 配置は、推奨される展開のメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="fd399-194">Web Deploy is the recommended mechanism for deployment.</span></span> <span data-ttu-id="fd399-195">Web 配置に代わる方法は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="fd399-195">Alternatives to Web Deploy are listed below.</span></span>

<span data-ttu-id="fd399-196">配置用に発行したアプリが実行されていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-196">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="fd399-197">アプリが実行中は、*publish* フォルダー内のファイルがロックされます。</span><span class="sxs-lookup"><span data-stu-id="fd399-197">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="fd399-198">ロックされているファイルはコピーできないため、配置は行われません。</span><span class="sxs-lookup"><span data-stu-id="fd399-198">Deployment can't occur because locked files can't be copied.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="fd399-199">Visual Studio からのアプリの展開</span><span class="sxs-lookup"><span data-stu-id="fd399-199">Web Deploy with Visual Studio</span></span>
<span data-ttu-id="fd399-200">Web 配置に使用する発行プロファイルの作成方法については、「[Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps](xref:publishing/web-publishing-vs#publish-profiles)」 (ASP.NET Core アプリを展開するために Visual Studio と MSBuild 用の発行プロファイルを作成する) のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-200">See [Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps](xref:publishing/web-publishing-vs#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="fd399-201">ホスティング プロバイダーから発行プロファイルまたは発行プロファイルを作成するためのサポートが提供されている場合は、そのプロファイルをダウンロードし、Visual Studio の **[発行]** ダイアログを使用してインポートします。</span><span class="sxs-lookup"><span data-stu-id="fd399-201">If your hosting provider supplies a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![[発行] ダイアログ ページ](iis/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="fd399-203">Visual Studio 外部での Web 配置</span><span class="sxs-lookup"><span data-stu-id="fd399-203">Web Deploy outside of Visual Studio</span></span>
<span data-ttu-id="fd399-204">Visual Studio の外部で、コマンド ラインから Web 配置を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="fd399-204">You can also use Web Deploy outside of Visual Studio from the command line.</span></span> <span data-ttu-id="fd399-205">詳細については、[Web 配置ツール](https://docs.microsoft.com/iis/publish/using-web-deploy/use-the-web-deployment-tool)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-205">For more information, see [Web Deployment Tool](https://docs.microsoft.com/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="fd399-206">Web 配置の代替手段</span><span class="sxs-lookup"><span data-stu-id="fd399-206">Alternatives to Web Deploy</span></span>
<span data-ttu-id="fd399-207">Web 配置を使用しないか、Visual Studio を使用しない場合は、複数の方法のいずれかを使用して、Xcopy、Robocopy、PowerShell などのホスティング システムにアプリケーションを移行できます。</span><span class="sxs-lookup"><span data-stu-id="fd399-207">If you don't wish to use Web Deploy or are not using Visual Studio, you may use any of several methods to move the application to the hosting system, such as Xcopy, Robocopy, or PowerShell.</span></span> <span data-ttu-id="fd399-208">Visual Studio ユーザーは、[発行サンプル](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md)を利用できます。</span><span class="sxs-lookup"><span data-stu-id="fd399-208">Visual Studio users may use the [Publish Samples](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="fd399-209">Web サイトを閲覧する</span><span class="sxs-lookup"><span data-stu-id="fd399-209">Browse the website</span></span>
![Microsoft Edge ブラウザーに IIS のスタートアップ ページが読み込まれています。](iis/_static/browsewebsite.png)
   
>[!WARNING]
> <span data-ttu-id="fd399-211">.NET Core アプリケーションは、IIS と Kestrel サーバー間のリバース プロキシによってホストされます。</span><span class="sxs-lookup"><span data-stu-id="fd399-211">.NET Core applications are hosted via a reverse-proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="fd399-212">リバース プロキシを作成するためには、展開されるアプリケーションのコンテンツ ルート パス (通常は、アプリ ベース パス) に *web.config* ファイルが存在する必要があります。これは IIS に対して指定される Web サイトの物理パスです。</span><span class="sxs-lookup"><span data-stu-id="fd399-212">In order to create the reverse-proxy, the *web.config* file must be present at the content root path (typically the app base path) of the deployed application, which is the website physical path provided to IIS.</span></span> <span data-ttu-id="fd399-213">アプリの物理パスには、*my_application.runtimeconfig.json*、*my_application.xml* (XML ドキュメントのコメント)、*my_application.deps.json*などのサブフォルダーを含め、機密性の高いファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="fd399-213">Sensitive files exist on the app's physical path, including subfolders, such as *my_application.runtimeconfig.json*, *my_application.xml* (XML Documentation comments), and *my_application.deps.json*.</span></span> <span data-ttu-id="fd399-214">Kestrel に対するリバース プロキシを作成するためには *web.config* ファイルが必要であり、そのため IIS は機密性の高いこれらのファイルやその他のファイルを処理できません。</span><span class="sxs-lookup"><span data-stu-id="fd399-214">The *web.config* file is required to create the reverse proxy to Kestrel, which prevents IIS from serving these and other sensitive files.</span></span> <span data-ttu-id="fd399-215">**したがって、*web.config* ファイルを誤って名前変更したり展開から削除したりしないようにすることが重要です。**</span><span class="sxs-lookup"><span data-stu-id="fd399-215">**Therefore, it is important that the *web.config* file is never accidently renamed or removed from the deployment.**</span></span>

## <a name="data-protection"></a><span data-ttu-id="fd399-216">データの保護</span><span class="sxs-lookup"><span data-stu-id="fd399-216">Data protection</span></span>

<span data-ttu-id="fd399-217">ASP.NET Core アプリケーションは、次の場合にキーリングをメモリに格納します。</span><span class="sxs-lookup"><span data-stu-id="fd399-217">An ASP.NET Core application will store the keyring in memory under the following condition:</span></span>

* <span data-ttu-id="fd399-218">Web サイトが IIS の背後でホストされている。</span><span class="sxs-lookup"><span data-stu-id="fd399-218">A website is hosted behind IIS.</span></span>
* <span data-ttu-id="fd399-219">データ保護スタックが、永続的なストアにキーリングを格納するように構成されていない。</span><span class="sxs-lookup"><span data-stu-id="fd399-219">The Data Protection stack has not been configured to store the keyring in a persistent store.</span></span>

<span data-ttu-id="fd399-220">キーリングがメモリに格納されている場合、アプリを再起動すると次のことが行われます。</span><span class="sxs-lookup"><span data-stu-id="fd399-220">If the keyring is stored in memory, when the app restarts:</span></span>

* <span data-ttu-id="fd399-221">すべてのフォーム認証トークンは無効になります。</span><span class="sxs-lookup"><span data-stu-id="fd399-221">All forms authentication tokens will be invalid.</span></span> 
* <span data-ttu-id="fd399-222">ユーザーは、次回の要求時に再ログインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-222">Users will need to login again on their next request.</span></span> 
* <span data-ttu-id="fd399-223">キーリングで保護されていたデータは保護されなくなります。</span><span class="sxs-lookup"><span data-stu-id="fd399-223">Any data you protected with the keyring will no longer be unprotected.</span></span>

> [!WARNING]
> <span data-ttu-id="fd399-224">データ保護は、認証で使用されるものを含め、いくつかの ASP.NET ミドルウェアによって使用されます。</span><span class="sxs-lookup"><span data-stu-id="fd399-224">Data Protection is used by several ASP.NET middlewares, including those used in authentication.</span></span> <span data-ttu-id="fd399-225">独自のコードからデータ保護 API を明示的に呼び出さない場合でも、配置スクリプトを使用して、または独自コード内にデータ保護を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-225">Even if you do not specifically call any Data Protection APIs from your own code you should configure Data Protection with a deployment script or in your own code.</span></span> <span data-ttu-id="fd399-226">データ保護を構成しない場合、既定でキーはメモリ内に保持され、アプリが再起動すると破棄されます。</span><span class="sxs-lookup"><span data-stu-id="fd399-226">If you do not configure data protection, by default the keys will be held in memory and discarded when your app restarts.</span></span> <span data-ttu-id="fd399-227">再起動すると、Cookie 認証によって作成された Cookie は無効になり、ユーザーは再度ログインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-227">Restarting will invalidate any cookies written by the cookie authentication and users will have to login again.</span></span>

<span data-ttu-id="fd399-228">IIS でのデータ保護を構成するには、次のいずれかの方法を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-228">To configure Data Protection under IIS you must use one of the following approaches:</span></span>

* <span data-ttu-id="fd399-229">[powershell スクリプト](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1)を実行し、適切なレジストリ エントリ (たとえば `.\Provision-AutoGenKeys.ps1 DefaultAppPool`) を作成します。</span><span class="sxs-lookup"><span data-stu-id="fd399-229">Run a [powershell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) to create suitable registry entries (For example,  `.\Provision-AutoGenKeys.ps1 DefaultAppPool`).</span></span> <span data-ttu-id="fd399-230">これによりキーはレジストリに格納され、DPAPI を使用して、コンピューター全体に適用するキーによって保護されます。</span><span class="sxs-lookup"><span data-stu-id="fd399-230">This will store keys in the registry, protected using DPAPI with a machine wide key.</span></span>
* <span data-ttu-id="fd399-231">ユーザー プロファイルを読み込むための IIS アプリケーション プールを構成します。</span><span class="sxs-lookup"><span data-stu-id="fd399-231">Configure the IIS Application Pool to load the user profile.</span></span> <span data-ttu-id="fd399-232">この設定は、アプリケーション プールの **[詳細設定]** の **[プロセス モデル]** セクションにあります。</span><span class="sxs-lookup"><span data-stu-id="fd399-232">This setting is in the **Process Model** section under the **Advanced Settings** for the application pool.</span></span> <span data-ttu-id="fd399-233">**[ユーザー プロファイルの読み込み]** を `True` に設定します。</span><span class="sxs-lookup"><span data-stu-id="fd399-233">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="fd399-234">これによりキーはユーザー プロファイル ディレクトリに格納され、DPAPI を使用して、アプリ プールで使うユーザー アカウントに固有のキーによって保護されます。</span><span class="sxs-lookup"><span data-stu-id="fd399-234">This will store keys under the user profile directory, and protected using DPAPI with a key specific to the user account used for the app pool.</span></span>
* <span data-ttu-id="fd399-235">[ファイル システムをキー リング ストアとして使用](xref:security/data-protection/configuration/overview)するようにアプリケーション コードを調整します。</span><span class="sxs-lookup"><span data-stu-id="fd399-235">Adjust your application code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="fd399-236">X509 証明書を使用してキー リングを保護し、それが信頼された証明書であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-236">Use an X509 certificate to protect the key ring and ensure it is a trusted certificate.</span></span> <span data-ttu-id="fd399-237">たとえば、証明書が自己署名証明書である場合は、信頼されたルート ストアに配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-237">For example, if it is a self signed certificate you must place it in the Trusted Root store.</span></span>

<span data-ttu-id="fd399-238">Web ファームで IIS を使用する場合:</span><span class="sxs-lookup"><span data-stu-id="fd399-238">When using IIS in a web farm:</span></span>

* <span data-ttu-id="fd399-239">すべてのコンピューターがアクセスできるファイル共有を使用します。</span><span class="sxs-lookup"><span data-stu-id="fd399-239">Use a file share all machines can access.</span></span>
* <span data-ttu-id="fd399-240">X509 証明書を各コンピューターに配置します。</span><span class="sxs-lookup"><span data-stu-id="fd399-240">Deploy an X509 certificate to each machine.</span></span>  <span data-ttu-id="fd399-241">[コード内にデータ保護](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview)を構成します。</span><span class="sxs-lookup"><span data-stu-id="fd399-241">Configure [data protection in code](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview).</span></span>

### <a name="1-create-a-data-protection-registry-hive"></a><span data-ttu-id="fd399-242">1.データ保護のレジストリ ハイブを作成する</span><span class="sxs-lookup"><span data-stu-id="fd399-242">1. Create a Data Protection Registry Hive</span></span>

<span data-ttu-id="fd399-243">ASP.NET アプリケーションで使用されるデータ保護キーは、アプリケーション外部のレジストリ ハイブに格納されます。</span><span class="sxs-lookup"><span data-stu-id="fd399-243">Data Protection keys used by ASP.NET applications are stored in registry hives external to the applications.</span></span> <span data-ttu-id="fd399-244">特定のアプリケーションのキーを保持するには、アプリケーションのアプリケーション プール用のレジストリ ハイブを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-244">To persist the keys for a given application, you must create a registry hive for the application's application pool.</span></span>

<span data-ttu-id="fd399-245">IIS のスタンドアロン インストールの場合、ASP.NET Core アプリケーションで使用するアプリケーション プールごとに、[データ保護の PowerShell スクリプト Provision-AutoGenKeys.ps1](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) を使用できます。</span><span class="sxs-lookup"><span data-stu-id="fd399-245">For standalone IIS installations, you may use the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) for each application pool used with an ASP.NET Core application.</span></span> <span data-ttu-id="fd399-246">このスクリプトは、HKLM レジストリに特別なレジストリ キーを作成します。そのキーは、ワーカー プロセス アカウントに対してのみ、ACL に追加されます。</span><span class="sxs-lookup"><span data-stu-id="fd399-246">This script will create a special registry key in the HKLM registry that is ACLed only to the worker process account.</span></span> <span data-ttu-id="fd399-247">キーは DPAPI を使用して保存時に暗号化されます。</span><span class="sxs-lookup"><span data-stu-id="fd399-247">Keys are encrypted at rest using DPAPI.</span></span>

<span data-ttu-id="fd399-248">Web ファームのシナリオでは、UNC パスを使用してデータ保護キー リングを格納するようにアプリケーションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="fd399-248">In web farm scenarios, an application can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="fd399-249">既定では、データ保護キーは暗号化されません。</span><span class="sxs-lookup"><span data-stu-id="fd399-249">By default, the data protection keys are not encrypted.</span></span> <span data-ttu-id="fd399-250">このような共有のファイル アクセス許可は、アプリケーションを実行する Windows アカウントに限定されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-250">You should ensure that the file permissions for such a share are limited to the Windows account the application runs as.</span></span> <span data-ttu-id="fd399-251">また、X509 証明書を使用して保存時のキーを保護するように選択することもできます。</span><span class="sxs-lookup"><span data-stu-id="fd399-251">In addition you may choose to protect keys at rest using an X509 certificate.</span></span> <span data-ttu-id="fd399-252">ユーザーが証明書をアップロードして各自の信頼された証明書ストアに配置できるメカニズムを検討し、ユーザーのアプリケーションが実行されるすべてのコンピューターでそのメカニズムが使用できることを確認するようお勧めします。</span><span class="sxs-lookup"><span data-stu-id="fd399-252">You may wish to consider a mechanism to allow users to upload certificates, place them into the user's trusted certificate store, and ensure they are available on all machines the user's application will run on.</span></span> <span data-ttu-id="fd399-253">詳細については、「[データ保護の構成](xref:security/data-protection/configuration/overview#data-protection-configuring)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-253">See [Configuring Data Protection](xref:security/data-protection/configuration/overview#data-protection-configuring) for details.</span></span>

### <a name="2-configure-the-iis-application-pool-to-load-the-user-profile"></a><span data-ttu-id="fd399-254">2.ユーザー プロファイルを読み込むように IIS アプリケーション プールを構成する</span><span class="sxs-lookup"><span data-stu-id="fd399-254">2. Configure the IIS Application Pool to load the user profile</span></span>
<span data-ttu-id="fd399-255">この設定は、アプリケーション プールの [詳細設定] の [プロセス モデル] セクションにあります。</span><span class="sxs-lookup"><span data-stu-id="fd399-255">This setting is in the Process Model section under the Advanced Settings for the application pool.</span></span> <span data-ttu-id="fd399-256">[ユーザー プロファイルの読み込み] を True に設定します。</span><span class="sxs-lookup"><span data-stu-id="fd399-256">Set Load User Profile to True.</span></span> <span data-ttu-id="fd399-257">これによりキーはユーザー プロファイル ディレクトリに格納され、DPAPI を使用して、アプリ プールで使うユーザー アカウントに固有のキーによって保護されます。</span><span class="sxs-lookup"><span data-stu-id="fd399-257">This will store keys under the user profile directory, and protected using DPAPI with a key specific to the user account used for the app pool.</span></span>

### <a name="3-machine-wide-policy-for-data-protection"></a><span data-ttu-id="fd399-258">3.コンピューター全体に適用するデータ保護ポリシー</span><span class="sxs-lookup"><span data-stu-id="fd399-258">3. Machine-wide policy for data protection</span></span>

<span data-ttu-id="fd399-259">データ保護システムでは、データ保護 API を使用するすべてのアプリケーションに対して、[コンピューター全体に適用する既定のポリシー](xref:security/data-protection/configuration/machine-wide-policy#data-protection-configuration-machinewidepolicy)を設定するためのサポートは限定的です。</span><span class="sxs-lookup"><span data-stu-id="fd399-259">The data protection system has limited support for setting default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy#data-protection-configuration-machinewidepolicy) for all applications that consume the data protection APIs.</span></span> <span data-ttu-id="fd399-260">詳細については、[データ保護](xref:security/data-protection/index)に関するドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-260">See the [data protection](xref:security/data-protection/index) documentation for more details.</span></span>

## <a name="configuration-of-sub-applications"></a><span data-ttu-id="fd399-261">サブアプリケーションの構成</span><span class="sxs-lookup"><span data-stu-id="fd399-261">Configuration of sub-applications</span></span>

<span data-ttu-id="fd399-262">ルート アプリケーションの下に追加したサブアプリケーションに、ハンドラーとして ASP.NET Core モジュールを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="fd399-262">Sub applications added under the root application shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="fd399-263">サブアプリケーションの *web.config* ファイルにこのモジュールをハンドラーとして追加した場合、サブアプリを閲覧しようとすると、エラーのある構成ファイルを参照する 500.19 (内部サーバー エラー) が返されます。</span><span class="sxs-lookup"><span data-stu-id="fd399-263">If you add the module as a handler in a sub-application's *web.config* file, you receive a 500.19 (Internal Server Error) referencing the faulty config file when you attempt to browse the sub app.</span></span> <span data-ttu-id="fd399-264">次の例は、ASP.NET Core サブアプリの発行済み *web.config* ファイルの内容を示しています。</span><span class="sxs-lookup"><span data-stu-id="fd399-264">The following example shows the contents of a published *web.config* file for an ASP.NET Core sub app:</span></span>

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

<span data-ttu-id="fd399-265">ASP.NET Core アプリの下に ASP.NET Core 以外のサブアプリをホストする場合は、サブアプリの *web.config* ファイルにある継承されたハンドラーを明示的に削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-265">If you intend to host a non-ASP.NET Core sub app underneath an ASP.NET Core app, you must explicitly remove the inherited handler in the sub app *web.config* file:</span></span>

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

<span data-ttu-id="fd399-266">*web.config* ファイルを使用して ASP.NET Core モジュールを構成する方法の詳細については、「[Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module)」 (ASP.NET Core モジュールの概要) のトピックと「[ASP.NET Core モジュール構成リファレンス](xref:hosting/aspnet-core-module)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-266">For more information on configuring the ASP.NET Core Module with the *web.config* file, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="fd399-267">web.config による IIS の構成</span><span class="sxs-lookup"><span data-stu-id="fd399-267">Configuration of IIS with web.config</span></span>

<span data-ttu-id="fd399-268">リバース プロキシ構成に適用される IIS 機能については、*web.config*の `<system.webServer>` セクションも、IIS の構成に影響します。</span><span class="sxs-lookup"><span data-stu-id="fd399-268">IIS configuration is still influenced by the `<system.webServer>` section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="fd399-269">たとえば、IIS が動的な圧縮を使用するようにシステム レベルで構成されている場合でも、アプリの *web.config* ファイルで `<urlCompression>` 要素を使用して、アプリに対してその設定を無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="fd399-269">For example, you may have IIS configured at the system level to use dynamic compression, but you could disable that setting for an app with the `<urlCompression>` element in the app's *web.config* file.</span></span> <span data-ttu-id="fd399-270">詳細については、[`<system.webServer>` の構成リファレンス](https://docs.microsoft.com/iis/configuration/system.webServer/)、「[ASP.NET Core Module Configuration Reference](xref:hosting/aspnet-core-module)」 (ASP.NET Core モジュールの構成リファレンス)、「[Using IIS Modules with ASP.NET Core](xref:hosting/iis-modules)」 (ASP.NET Core での IIS モジュールの使用) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-270">For more information, see the [configuration reference for `<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:hosting/aspnet-core-module), and [Using IIS Modules with ASP.NET Core](xref:hosting/iis-modules).</span></span> <span data-ttu-id="fd399-271">分離されたアプリケーション プール (IIS 10.0 以降でサポートされています) で実行する個別アプリに対して環境変数を設定する必要がある場合は、IIS のリファレンス ドキュメントで、[環境変数\<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) のトピックにある *AppCmd.exe コマンド*のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-271">If you need to set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="fd399-272">web.config の構成のセクション</span><span class="sxs-lookup"><span data-stu-id="fd399-272">Configuration sections of web.config</span></span>

<span data-ttu-id="fd399-273">`<system.web>`、`<appSettings>`、`<connectionStrings>`、`<location>` の各要素を使用して *web.config* で構成される .NET Framework アプリケーションとは異なり、ASP.NET Core アプリは、その他の構成プロバイダーを使用して構成されます。</span><span class="sxs-lookup"><span data-stu-id="fd399-273">Unlike .NET Framework applications that are configured with the `<system.web>`, `<appSettings>`, `<connectionStrings>`, and `<location>` elements in *web.config*, ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="fd399-274">詳細については、[構成](xref:fundamentals/configuration)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-274">For more information, see [Configuration](xref:fundamentals/configuration).</span></span>

## <a name="application-pools"></a><span data-ttu-id="fd399-275">アプリケーション プール</span><span class="sxs-lookup"><span data-stu-id="fd399-275">Application Pools</span></span>

<span data-ttu-id="fd399-276">1 つのシステムで複数の Web サイトをホストする場合は、アプリをそれぞれ専用のアプリケーション プールで実行して、アプリケーションを相互に分離する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-276">When hosting multiple websites on a single system, you should isolate the applications from each other by running each app in its own application pool.</span></span> <span data-ttu-id="fd399-277">これは、IIS の **[Web サイトの追加]** ダイアログの既定の設定です。</span><span class="sxs-lookup"><span data-stu-id="fd399-277">The IIS **Add Website** dialog defaults to this behavior.</span></span> <span data-ttu-id="fd399-278">**[サイト名]** を指定すると、入力したテキストが自動的に **[アプリケーション プール]** テキストボックスに設定されます。</span><span class="sxs-lookup"><span data-stu-id="fd399-278">When you provide a **Site name**, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="fd399-279">Web サイトを追加するときに、そのサイト名を使用して新しいアプリケーション プールが作成されます。</span><span class="sxs-lookup"><span data-stu-id="fd399-279">A new application pool will be created using the site name when you add the website.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="fd399-280">アプリケーション プール ID</span><span class="sxs-lookup"><span data-stu-id="fd399-280">Application Pool Identity</span></span>

<span data-ttu-id="fd399-281">アプリケーション プール ID アカウントを使用すると、ドメインやローカル アカウントを作成して管理する必要なく、一意のアカウントでアプリケーションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="fd399-281">An application pool identity account allows you to run an application under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="fd399-282">IIS 8.0 以降の IIS 管理者ワーカー プロセス (WAS) は、新しいアプリケーション プールの名前で仮想アカウントを作成し、既定によってアプリケーション プールのワーカー プロセスをこのアカウントで実行します。</span><span class="sxs-lookup"><span data-stu-id="fd399-282">On IIS 8.0+, the IIS Admin Worker Process (WAS) will create a virtual account with the name of the new application pool and run the application pool's worker processes under this account by default.</span></span> <span data-ttu-id="fd399-283">IIS 管理コンソールにあるアプリケーション プールの [詳細設定] で、次の画像に示すように、ID が **ApplicationPoolIdentity** を使用するように設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-283">In the IIS Management Console, under Advanced Settings for your application pool, ensure that the Identity is set to use **ApplicationPoolIdentity** as shown in the image below.</span></span>

![アプリケーション プールの [詳細設定] ダイアログ](iis/_static/apppool-identity.png)

<span data-ttu-id="fd399-285">IIS 管理プロセスは、Windows セキュリティ システムでのアプリケーション プールの名前を使用して、セキュリティで保護された識別子を作成します。</span><span class="sxs-lookup"><span data-stu-id="fd399-285">The IIS management process creates a secure identifier with the name of the application pool in the Windows Security System.</span></span> <span data-ttu-id="fd399-286">この ID を使用してリソースを保護することができます。ただし、この ID は実際のユーザー アカウントではなく、Windows ユーザーの管理コンソールに表示されません。</span><span class="sxs-lookup"><span data-stu-id="fd399-286">Resources can be secured by using this identity; however, this identity is not a real user account and won't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="fd399-287">アプリケーションに対する IIS ワーカー プロセスのアクセス許可を昇格させる必要がある場合は、アプリケーションを含むディレクトリのアクセス制御リスト (ACL) を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-287">If you need to grant the IIS worker process elevated access to your application, you will need to modify the Access Control List (ACL) for the directory containing your application.</span></span>

1. <span data-ttu-id="fd399-288">エクスプローラーを開き、そのディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="fd399-288">Open Windows Explorer and navigate to the directory.</span></span>

2. <span data-ttu-id="fd399-289">ディレクトリ上で右クリックし、**[プロパティ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="fd399-289">Right click on the directory and click **Properties**.</span></span>

3. <span data-ttu-id="fd399-290">**[セキュリティ]** タブの **[編集]** ボタンをクリックし、**[追加]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="fd399-290">Under the **Security** tab, click the **Edit** button and then the **Add** button.</span></span>

4. <span data-ttu-id="fd399-291">**[場所]** ボタンをクリックし、使用しているシステムを選択します。</span><span class="sxs-lookup"><span data-stu-id="fd399-291">Click the **Locations** button and make sure you select your system.</span></span>

5. <span data-ttu-id="fd399-292">**[選択するオブジェクト名を入力してください]** テキストボックスに「**IIS AppPool\DefaultAppPool**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="fd399-292">Enter **IIS AppPool\DefaultAppPool** in **Enter the object names to select** textbox.</span></span>

  ![アプリケーション フォルダーの [ユーザーまたはグループの選択] ダイアログ](iis/_static/select-users-or-groups-1.png)

6. <span data-ttu-id="fd399-294">**[名前の確認]** ボタンをクリックし、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="fd399-294">Click the **Check Names** button and then click **OK**.</span></span>

  ![アプリケーション フォルダーの [ユーザーまたはグループの選択] ダイアログ](iis/_static/select-users-or-groups-2.png)

<span data-ttu-id="fd399-296">この操作は、コマンド プロンプトから **ICACLS** ツールを使用して行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="fd399-296">You can also do this via a command prompt using **ICACLS** tool:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="troubleshooting-tips"></a><span data-ttu-id="fd399-297">トラブルシューティングのヒント</span><span class="sxs-lookup"><span data-stu-id="fd399-297">Troubleshooting tips</span></span>

<span data-ttu-id="fd399-298">IIS の点かいに関する問題を診断するには、ブラウザーの出力を調査し、システムの**アプリケーション** ログを**イベント ビューアー**で確認して、`stdout` のログ記録を有効にします。</span><span class="sxs-lookup"><span data-stu-id="fd399-298">To diagnose problems with IIS deployments, study browser output, examine the system's **Application** log through **Event Viewer**, and enable `stdout` logging.</span></span> <span data-ttu-id="fd399-299">**ASP.NET Core モジュール**のログは、*web.config*で `<aspNetCore>`要素の *stdoutLogFile* 属性に指定したパスにあります。この属性の値で指定されたパスにあるすべてのフォルダーが、展開環境に存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-299">The **ASP.NET Core Module** log will be found on the path provided in the *stdoutLogFile* attribute of the `<aspNetCore>` element in *web.config*. Any folders on the path provided in the attribute value must exist in the deployment.</span></span> <span data-ttu-id="fd399-300">また、*stdoutLogEnabled="true"*を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-300">You must also set *stdoutLogEnabled="true"*.</span></span> <span data-ttu-id="fd399-301">`Microsoft.NET.Sdk.Web` SDK を使用して *web.config* ファイルを作成するアプリケーションでは、既定により *stdoutLogEnabled* が *false* に設定されるため、`stdout` のログ記録を有効にするには、*web.config* ファイルを手動で作成するか、ファイルを編集する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-301">Applications that use the `Microsoft.NET.Sdk.Web` SDK to create the *web.config* file will default the *stdoutLogEnabled* setting to *false*, so you must manually provide the *web.config* file or modify the file in order to enable `stdout` logging.</span></span>

<span data-ttu-id="fd399-302">一般的なエラーのいくつかは、*startupTimeLimit* (既定値: 120 秒) と *startupRetryCount* (既定値: 2) が経過するまで、ブラウザー、アプリケーション ログ、ASP.NET Core モジュールのログに表示されません。</span><span class="sxs-lookup"><span data-stu-id="fd399-302">Several of the common errors do not appear in the browser, Application Log, and ASP.NET Core Module Log until the module *startupTimeLimit* (default: 120 seconds) and *startupRetryCount* (default: 2) have passed.</span></span> <span data-ttu-id="fd399-303">したがって、モジュールがアプリケーションのプロセスを開始できなかったと判断する前に、完全に 6 分が経過するまで待ってください。</span><span class="sxs-lookup"><span data-stu-id="fd399-303">Therefore, wait a full six minutes before deducing that the module has failed to start a process for the application.</span></span>

<span data-ttu-id="fd399-304">アプリケーションが正常に動作しているかどうかを決定する 1 つの簡単な方法は、アプリケーションを Kestrel で直接実行することです。</span><span class="sxs-lookup"><span data-stu-id="fd399-304">One quick way to determine if the application is working properly is to run the application directly on Kestrel.</span></span> <span data-ttu-id="fd399-305">アプリケーションがフレームワークに依存する展開として発行された場合は、展開フォルダー (アプリケーションの IIS 物理パス) の **dotnet my_application.dll** を実行します。</span><span class="sxs-lookup"><span data-stu-id="fd399-305">If the application was published as a framework-dependent deployment, execute **dotnet my_application.dll** in the deployment folder, which is the IIS physical path to the application.</span></span> <span data-ttu-id="fd399-306">アプリケーションが自己完結型の展開として発行された場合は、展開フォルダーにあるアプリケーションの実行可能ファイル (**my_application.exe**) をコマンド プロンプトから直接実行します。</span><span class="sxs-lookup"><span data-stu-id="fd399-306">If the application was published as a self-contained deployment, run the application's executable directly from a command prompt, **my_application.exe**, in the deployment folder.</span></span> <span data-ttu-id="fd399-307">Kestrel が既定のポート 5000 でリッスンしている場合は、`http://localhost:5000/` でアプリケーションを閲覧できます。</span><span class="sxs-lookup"><span data-stu-id="fd399-307">If Kestrel is listening on default port 5000, you should be able to browse the application at `http://localhost:5000/`.</span></span> <span data-ttu-id="fd399-308">アプリケーションが通常は Kestrel エンドポイント アドレスで応答する場合、問題は IIS、ASP.NET Core モジュール、Kestrel の構成に関連している可能性が高く、アプリケーション自体に関連する可能性は低くなります。</span><span class="sxs-lookup"><span data-stu-id="fd399-308">If the application responds normally at the Kestrel endpoint address, the problem is more likely related to the IIS-ASP.NET Core Module-Kestrel configuration and less likely within the application itself.</span></span>

<span data-ttu-id="fd399-309">Kestrel サーバーに対する IIS リバース プロキシが正常に動作しているかどうかを確認する 1 つの方法は、[静的ファイル ミドルウェア](xref:fundamentals/static-files)を使用して、*wwwroot* にあるアプリケーションの静的ファイルのスタイルシート、スクリプト、または画像に対する簡単な静的ファイル要求を実行することです。</span><span class="sxs-lookup"><span data-stu-id="fd399-309">One way to determine if the IIS reverse proxy to the Kestrel server is working properly is to perform a simple static file request for a stylesheet, script, or image from the application's static files in *wwwroot* using [Static File middleware](xref:fundamentals/static-files).</span></span> <span data-ttu-id="fd399-310">アプリケーションの静的ファイルを取得できたにもかかわらず、MVC ビューおよびその他のエンドポイントでエラーになる場合は、問題が IIS、ASP.NET Core モジュール、Kestrel 構成に関連している可能性は低く、アプリケーション自体に関連している可能性が高くなります (たとえば、MVC ルーティングや内部サーバー エラー 500)。</span><span class="sxs-lookup"><span data-stu-id="fd399-310">If the application can serve static files but MVC Views and other endpoints are failing, the problem is less likely related to the IIS-ASP.NET Core Module-Kestrel configuration and more likely within the application itself (for example, MVC routing or 500 Internal Server Error).</span></span>

<span data-ttu-id="fd399-311">Kestrel が IIS の背後で正常に開始される一方、ローカルで正常に実行できたアプリをシステムで実行できない場合は、*web.config* に、`ASPNETCORE_ENVIRONMENT` を `Development` に設定する環境変数を一時的に追加することができます。</span><span class="sxs-lookup"><span data-stu-id="fd399-311">When Kestrel starts normally behind IIS but the app won't run on the system after successfully running locally, you can temporarily add an environment variable to *web.config* to set the `ASPNETCORE_ENVIRONMENT` to `Development`.</span></span> <span data-ttu-id="fd399-312">これにより、アプリの起動時に環境をオーバーライドしない限り、アプリをシステムで実行するときに[開発者例外ページ](xref:fundamentals/error-handling)を表示できます。</span><span class="sxs-lookup"><span data-stu-id="fd399-312">As long as you don't override the environment in app startup, this will allow the [developer exception page](xref:fundamentals/error-handling) to appear when the app is run on the system.</span></span> <span data-ttu-id="fd399-313">`ASPNETCORE_ENVIRONMENT` の環境変数をこのように設定する方法は、インターネットに公開されないステージング システムやテスト用システムについてのみ推奨される方法です。</span><span class="sxs-lookup"><span data-stu-id="fd399-313">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` in this way is only recommended for staging/testing systems that are not exposed to the Internet.</span></span> <span data-ttu-id="fd399-314">目的が完了したら、必ず *web.config* ファイルから環境変数を削除してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-314">Be sure you remove the environment variable from the *web.config* file when finished.</span></span> <span data-ttu-id="fd399-315">*web.config* を使用してリバース プロキシ用の環境変数を設定する方法の詳細については、「[environmentVariables child element of aspNetCore](xref:hosting/aspnet-core-module#setting-environment-variables)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-315">For information on setting environment variables via *web.config* for the reverse proxy, see [environmentVariables child element of aspNetCore](xref:hosting/aspnet-core-module#setting-environment-variables).</span></span>

<span data-ttu-id="fd399-316">ほとんどの場合、アプリケーションのログ記録を有効にすると、アプリケーションまたはリバース プロキシに関する問題のトラブルシューティングに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="fd399-316">In most cases, enabling application logging will assist in troubleshooting problems with application or the reverse proxy.</span></span> <span data-ttu-id="fd399-317">詳細については、[ログ記録](xref:fundamentals/logging)に関する説明を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-317">See [Logging](xref:fundamentals/logging) for more information.</span></span>

<span data-ttu-id="fd399-318">最後に紹介するトラブルシューティングのヒントは、開発用コンピューター上の .NET Core SDK またはアプリ内のパッケージ バージョンのいずれかをアップグレード後に実行が失敗するアプリに関するものです。</span><span class="sxs-lookup"><span data-stu-id="fd399-318">Our last troubleshooting tip pertains to apps that fail to run after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="fd399-319">場合によっては、パッケージに統一性がないと、メジャー アップグレード実行時にアプリが破壊されることがあります。</span><span class="sxs-lookup"><span data-stu-id="fd399-319">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="fd399-320">このような問題の大部分は、次の方法で解決できます。まず、プロジェクトの `bin` フォルダーと `obj` フォルダーを削除し、`%UserProfile%\.nuget\packages\` と `%LocalAppData%\Nuget\v3-cache` にあるパッケージのキャッシュをクリアします。その後、プロジェクトを復元し、システムで以前の展開が完全に削除されたことを確認してから、アプリを再展開します。</span><span class="sxs-lookup"><span data-stu-id="fd399-320">You can fix most of these issues by deleting the `bin` and `obj` folders in the project, clearing package caches at `%UserProfile%\.nuget\packages\` and `%LocalAppData%\Nuget\v3-cache`, restoring the project, and confirming that your prior deployment on the system has been completely deleted prior to re-deploying the app.</span></span>

>[!TIP]
> <span data-ttu-id="fd399-321">パッケージのキャッシュをクリアする便利な方法として、[NuGet.org](https://www.nuget.org/) から `NuGet.exe` ツールを入手し、システム PATH に追加して、コマンド プロンプトから `nuget locals all -clear` を実行します。</span><span class="sxs-lookup"><span data-stu-id="fd399-321">A convenient way to clear package caches is to obtain the `NuGet.exe` tool from [NuGet.org](https://www.nuget.org/), add it to your system PATH, and execute `nuget locals all -clear` from a command prompt.</span></span>

## <a name="common-errors"></a><span data-ttu-id="fd399-322">一般的なエラー</span><span class="sxs-lookup"><span data-stu-id="fd399-322">Common errors</span></span>

<span data-ttu-id="fd399-323">次の一覧に含まれていないエラーもあります。</span><span class="sxs-lookup"><span data-stu-id="fd399-323">The following is not a complete list of errors.</span></span> <span data-ttu-id="fd399-324">この一覧にないエラーが発生した場合は、以下のコメント セクションに詳細なエラー メッセージを記録してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-324">Should you encounter an error not listed here, please leave a detailed error message in the comments section below.</span></span>

### <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="fd399-325">インストーラーが VC++ 再頒布可能パッケージを取得できない</span><span class="sxs-lookup"><span data-stu-id="fd399-325">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="fd399-326">**インストーラー例外:** 0x80072efd または 0x80072f76 - 特定できないエラー</span><span class="sxs-lookup"><span data-stu-id="fd399-326">**Installer Exception:** 0x80072efd or 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="fd399-327">**インストーラー ログ例外&#8224;:** エラー 0x80072efd または 0x80072f76: Failed to execute EXE package (EXE パッケージを実行できませんでした)</span><span class="sxs-lookup"><span data-stu-id="fd399-327">**Installer Log Exception&#8224;:** Error 0x80072efd or 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="fd399-328">&#8224;ログの場所は C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log です。</span><span class="sxs-lookup"><span data-stu-id="fd399-328">&#8224;The log is located at C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span></span>

<span data-ttu-id="fd399-329">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="fd399-329">Troubleshooting:</span></span>

* <span data-ttu-id="fd399-330">サーバー ホスティング バンドルのインストール時にインストーラーがインターネットにアクセスできない場合、インストーラーは *Microsoft Visual C++ 2015 再頒布可能パッケージ*を取得できず、この例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="fd399-330">If the system does not have Internet access while installing the server hosting bundle, this exception will occur when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="fd399-331">インストーラーは [Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=53840)から入手できます。</span><span class="sxs-lookup"><span data-stu-id="fd399-331">You may obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="fd399-332">インストーラーが失敗した場合、フレームワークに依存する展開をホストするために必要な .NET Core ランタイムを受け取れない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-332">If the installer fails, you may not receive the .NET Core runtime required to host framework-dependent deployments.</span></span> <span data-ttu-id="fd399-333">フレームワークに依存する展開をホストする場合は、[プログラムと機能] でランタイムがインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-333">If you plan to host framework-dependent deployments, confirm that the runtime is installed in Programs &amp; Features.</span></span> <span data-ttu-id="fd399-334">ランタイム インストーラーは [.NET ダウンロード](https://www.microsoft.com/net/download/core)のページから入手できます。</span><span class="sxs-lookup"><span data-stu-id="fd399-334">You may obtain a runtime installer from [.NET Downloads](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="fd399-335">ランタイムのインストール後、システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。</span><span class="sxs-lookup"><span data-stu-id="fd399-335">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

### <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="fd399-336">OS のアップグレードによって 32 ビット ASP.NET Core モジュールが削除された</span><span class="sxs-lookup"><span data-stu-id="fd399-336">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

* <span data-ttu-id="fd399-337">**アプリケーション ログ:** モジュール DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** を読み込めませんでした。</span><span class="sxs-lookup"><span data-stu-id="fd399-337">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="fd399-338">このデータはエラーです。</span><span class="sxs-lookup"><span data-stu-id="fd399-338">The data is the error.</span></span>

<span data-ttu-id="fd399-339">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="fd399-339">Troubleshooting:</span></span>

* <span data-ttu-id="fd399-340">**C:\Windows\SysWOW64\inetsrv** ディレクトリにある OS ファイルでないファイルは、OS アップグレード時に保持されません。</span><span class="sxs-lookup"><span data-stu-id="fd399-340">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory are not preserved during an OS upgrade.</span></span> <span data-ttu-id="fd399-341">OS アップグレードより前に ASP.NET Core モジュールをインストールしていた場合、OS アップグレード後に 32 ビット モードで AppPool を実行しようとすると、この問題が発生します。</span><span class="sxs-lookup"><span data-stu-id="fd399-341">If you have the ASP.NET Core Module installed prior to an OS upgrade and then try to run any AppPool in 32-bit mode after an OS upgrade, you will encounter this issue.</span></span> <span data-ttu-id="fd399-342">OS アップグレード後に ASP.NET Core モジュールを修復してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-342">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="fd399-343">「[.NET Core Windows Server ホスティング バンドルのインストール](#install-the-net-core-windows-server-hosting-bundle)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-343">See [Install the .NET Core Windows Server Hosting bundle](#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="fd399-344">インストーラーを実行するときに **[修復]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="fd399-344">Select **Repair** when you run the installer.</span></span>

### <a name="platform-conflicts-with-rid"></a><span data-ttu-id="fd399-345">プラットフォームが RID と競合している</span><span class="sxs-lookup"><span data-stu-id="fd399-345">Platform conflicts with RID</span></span>

* <span data-ttu-id="fd399-346">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="fd399-346">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="fd399-347">**アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ', ErrorCode = '0x80004005 : ff. (物理ルート 'C:{PATH}\' のアプリケーション 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' は、コマンド ライン '"C:\{PATH}\my_application.{exe|dll}" ' でプロセスを開始できませんでした。ErrorCode = '0x80004005 : ff)</span><span class="sxs-lookup"><span data-stu-id="fd399-347">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="fd399-348">**ASP.NET Core モジュールのログ:** ハンドルされない例外: System.BadImageFormatException: Could not load file or assembly 'my_application.dll'. (ファイルまたはアセンブリ 'my_application.dll' が読み込めませんでした)</span><span class="sxs-lookup"><span data-stu-id="fd399-348">**ASP.NET Core Module Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly 'my_application.dll'.</span></span> <span data-ttu-id="fd399-349">正しくない形式のプログラムを読み込もうとしました。</span><span class="sxs-lookup"><span data-stu-id="fd399-349">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="fd399-350">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="fd399-350">Troubleshooting:</span></span>

* <span data-ttu-id="fd399-351">Kestrel でアプリケーションをローカルに実行できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-351">Confirm that the application runs locally on Kestrel.</span></span> <span data-ttu-id="fd399-352">プロセスのエラーは、アプリケーション内の問題の結果である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-352">A process failure might be the result of a problem within the application.</span></span> <span data-ttu-id="fd399-353">詳細については、「[トラブルシューティングのヒント](#troubleshooting-tips)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-353">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

* <span data-ttu-id="fd399-354">RID と競合する `<PlatformTarget>` を *.csproj* に設定していないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-354">Confirm that you didn't set a `<PlatformTarget>` in your *.csproj* that conflicts with the RID.</span></span> <span data-ttu-id="fd399-355">たとえば、`x86` の `<PlatformTarget>`を指定した場合は、*dotnet publish -c Release -r win10-x64* を使用するか、*.csproj*で `<RuntimeIdentifiers>` を `win10-x64`に設定するかを問わず、`win10-x64` の RID を使用して発行しないでください。</span><span class="sxs-lookup"><span data-stu-id="fd399-355">For example, don't specify a `<PlatformTarget>` of `x86` and publish with an RID of `win10-x64`, either by using *dotnet publish -c Release -r win10-x64* or by setting the `<RuntimeIdentifiers>` in your *.csproj* to `win10-x64`.</span></span> <span data-ttu-id="fd399-356">プロジェクトは警告やエラーにならずに発行されますが、上記のログに記録されたシステムの例外によって失敗します。</span><span class="sxs-lookup"><span data-stu-id="fd399-356">The project will publish without warning or error but fail with the above logged exceptions on the system.</span></span>

* <span data-ttu-id="fd399-357">Azure Apps 展開で、アプリケーションをアップグレードして新しいアセンブリを展開しようとしたときにこの例外が発生した場合は、以前の展開からすべてのファイルを手動で削除してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-357">If this exception occurs for an Azure Apps deployment when upgrading an application and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="fd399-358">アップグレードしたアプリを展開するとき、互換性のないアセンブリが残っていると、`System.BadImageFormatException` 例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="fd399-358">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

### <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="fd399-359">URI のエンドポイントが間違っているか、Web サイトが停止している</span><span class="sxs-lookup"><span data-stu-id="fd399-359">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="fd399-360">**ブラウザー:** ERR_CONNECTION_REFUSED</span><span class="sxs-lookup"><span data-stu-id="fd399-360">**Browser:** ERR_CONNECTION_REFUSED</span></span>

* <span data-ttu-id="fd399-361">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="fd399-361">**Application Log:** No entry</span></span>

* <span data-ttu-id="fd399-362">**ASP.NET Core モジュールのログ:** ログ ファイルは作成されません</span><span class="sxs-lookup"><span data-stu-id="fd399-362">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="fd399-363">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="fd399-363">Troubleshooting:</span></span>

* <span data-ttu-id="fd399-364">アプリケーションに対して正しい URI エンドポイントを使用していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-364">Confirm you are using the correct URI endpoint for the application.</span></span> <span data-ttu-id="fd399-365">バインドを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-365">Check your bindings.</span></span>

* <span data-ttu-id="fd399-366">IIS Web サイトが *[停止]* 状態でないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-366">Confirm that the IIS website is not in the *Stopped* state.</span></span>

### <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="fd399-367">CoreWebEngine または W3SVC サーバー機能が無効</span><span class="sxs-lookup"><span data-stu-id="fd399-367">CoreWebEngine or W3SVC server features disabled</span></span>

* <span data-ttu-id="fd399-368">**OS の例外:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module. (ASP.NET Core モジュールを使用するには、IIS 7.0 CoreWebEngine および W3SVC の機能をインストールする必要があります。)</span><span class="sxs-lookup"><span data-stu-id="fd399-368">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="fd399-369">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="fd399-369">Troubleshooting:</span></span>

* <span data-ttu-id="fd399-370">適切な役割と機能を有効にしていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-370">Confirm that you have enabled the proper role and features.</span></span> <span data-ttu-id="fd399-371">「[IIS 構成](#iis-configuration)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-371">See [IIS Configuration](#iis-configuration).</span></span>

### <a name="incorrect-website-physical-path-or-application-missing"></a><span data-ttu-id="fd399-372">Web サイト物理パスが間違っているか、アプリケーションがない</span><span class="sxs-lookup"><span data-stu-id="fd399-372">Incorrect website physical path or application missing</span></span>

* <span data-ttu-id="fd399-373">**ブラウザー:** 403 許可されていません - アクセスが拒否されました **--または--** 403.14 許可されていません - Web サーバーは、このディレクトリの内容の一覧を表示しないように構成されています。</span><span class="sxs-lookup"><span data-stu-id="fd399-373">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="fd399-374">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="fd399-374">**Application Log:** No entry</span></span>

* <span data-ttu-id="fd399-375">**ASP.NET Core モジュールのログ:** ログ ファイルは作成されません</span><span class="sxs-lookup"><span data-stu-id="fd399-375">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="fd399-376">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="fd399-376">Troubleshooting:</span></span>

* <span data-ttu-id="fd399-377">IIS Web サイトの**基本設定**と物理アプリケーション フォルダーを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-377">Check the IIS website **Basic Settings** and the physical application folder.</span></span> <span data-ttu-id="fd399-378">アプリケーションが IIS Web サイトの**物理パス**にあるフォルダー内に配置されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-378">Confirm that the application is in the folder at the IIS website **Physical path**.</span></span>

### <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="fd399-379">役割が正しくない、モジュールがインストールされていない、または不適切なアクセス許可</span><span class="sxs-lookup"><span data-stu-id="fd399-379">Incorrect role, module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="fd399-380">**ブラウザー:** 500.19 内部サーバー エラー - ページに関連する構成データが無効であるため、要求されたページにアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="fd399-380">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>

* <span data-ttu-id="fd399-381">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="fd399-381">**Application Log:** No entry</span></span>

* <span data-ttu-id="fd399-382">**ASP.NET Core モジュールのログ:** ログ ファイルは作成されません</span><span class="sxs-lookup"><span data-stu-id="fd399-382">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="fd399-383">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="fd399-383">Troubleshooting:</span></span>

* <span data-ttu-id="fd399-384">適切な役割を有効にしていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-384">Confirm that you have enabled the proper role.</span></span> <span data-ttu-id="fd399-385">「[IIS 構成](#iis-configuration)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-385">See [IIS Configuration](#iis-configuration).</span></span>

* <span data-ttu-id="fd399-386">**[プログラムと機能]** をチェックし、**Microsoft ASP.NET Core モジュール**がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-386">Check **Programs &amp; Features** and confirm that the **Microsoft ASP.NET Core Module** has been installed.</span></span> <span data-ttu-id="fd399-387">インストールされているプログラムの一覧に **Microsoft ASP.NET Core モジュール**が表示されない場合は、モジュールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="fd399-387">If the **Microsoft ASP.NET Core Module** is not present in the list of installed programs, install the module.</span></span> <span data-ttu-id="fd399-388">「[.NET Core Windows Server ホスティング バンドルのインストール](#install-the-net-core-windows-server-hosting-bundle)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-388">See [Install the .NET Core Windows Server Hosting bundle](#install-the-net-core-windows-server-hosting-bundle).</span></span>

* <span data-ttu-id="fd399-389">**[アプリケーション プール] > [プロセス モデル] > [ID]** が **ApplicationPoolIdentity** に設定されていることを確認します。または、アプリケーションの展開フォルダーにアクセスするための正しいアクセス許可がカスタム ID に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-389">Make sure that the **Application Pool > Process Model > Identity** is set to **ApplicationPoolIdentity** or your custom identity has the correct permissions to access the application's deployment folder.</span></span>

### <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="fd399-390">processPath の誤り、PATH 変数の欠如、ホスティング バンドルが未インストール、システムまたは IIS が再起動されていない、VC++ 再頒布可能パッケージが未インストール、dotnet.exe アクセス違反</span><span class="sxs-lookup"><span data-stu-id="fd399-390">Incorrect processPath, missing PATH variable, hosting bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="fd399-391">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="fd399-391">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="fd399-392">**アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\my_application.exe" ', ErrorCode = '0x80070002 : 0. (物理ルート 'C:\{PATH}\' のアプリケーション 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' は、コマンド ライン '".\my_application.exe" ' でプロセスを開始できませんでした。ErrorCode = '0x80070002 : 0)</span><span class="sxs-lookup"><span data-stu-id="fd399-392">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\my_application.exe" ', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="fd399-393">**ASP.NET Core モジュールのログ:** ログ ファイルが作成されましたが空です</span><span class="sxs-lookup"><span data-stu-id="fd399-393">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="fd399-394">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="fd399-394">Troubleshooting:</span></span>

* <span data-ttu-id="fd399-395">Kestrel でアプリケーションをローカルに実行できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-395">Confirm that the application runs locally on Kestrel.</span></span> <span data-ttu-id="fd399-396">プロセスのエラーは、アプリケーション内の問題の結果である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-396">A process failure might be the result of a problem within the application.</span></span> <span data-ttu-id="fd399-397">詳細については、「[トラブルシューティングのヒント](#troubleshooting-tips)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-397">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

* <span data-ttu-id="fd399-398">*web.config* で `<aspNetCore>` 要素の *processPath* 属性を調べ、フレームワークに依存する展開を示す *dotnet*、または自己完結型の展開を示す *.\my_application.exe* になっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-398">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it is *dotnet* for a framework-dependent deployment or *.\my_application.exe* for a self-contained deployment.</span></span>

* <span data-ttu-id="fd399-399">フレームワークに依存する展開の場合、PATH 設定で *dotnet.exe* にアクセスできていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-399">For a framework-dependent deployment, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="fd399-400">*C:\Program Files\dotnet\* がシステムの PATH 設定に含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-400">Confirm that *C:\Program Files\dotnet\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="fd399-401">フレームワークに依存する展開では、アプリケーション プールのユーザー ID で *dotnet.exe* にアクセスできていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-401">For a framework-dependent deployment, *dotnet.exe* might not be accessible for the user identity of the Application Pool.</span></span> <span data-ttu-id="fd399-402">AppPool ユーザー ID に、*C:\Program Files\dotnet* ディレクトリへのアクセス許可が設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-402">Confirm that the AppPool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="fd399-403">*C:\Program Files\dotnet* とアプリケーション ディレクトリに、AppPool ユーザー ID に対する拒否ルールが構成されていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-403">Confirm that there are no deny rules configured for the AppPool user identity on the *C:\Program Files\dotnet* and application directories.</span></span>

* <span data-ttu-id="fd399-404">フレームワークに依存する展開を配置し、IIS を再起動せずに .NET Core をインストールした可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-404">You may have deployed a framework-dependent deployment and installed .NET Core without restarting IIS.</span></span> <span data-ttu-id="fd399-405">サーバーを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。</span><span class="sxs-lookup"><span data-stu-id="fd399-405">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="fd399-406">ホスト システム .NET Core ランタイムをインストールせずに、フレームワークに依存する展開を配置した可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-406">You may have deployed a framework-dependent deployment without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="fd399-407">フレームワークに依存する展開を配置しようとしていて、.NET Core ランタイムをインストールしていない場合は、**.NET Core Windows Server ホスティング バンドル インストーラー**をシステムで実行します。</span><span class="sxs-lookup"><span data-stu-id="fd399-407">If you're attempting to deploy a framework-dependent deployment and have not installed the .NET Core runtime, run the **.NET Core Windows Server Hosting bundle installer** on the system.</span></span> <span data-ttu-id="fd399-408">「[.NET Core Windows Server ホスティング バンドルのインストール](#install-the-net-core-windows-server-hosting-bundle)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-408">See [Install the .NET Core Windows Server Hosting bundle](#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="fd399-409">インターネットに接続せずに、.NET Core ランタイムをシステムにインストールする場合は、[.NET ダウンロード](https://www.microsoft.com/net/download/core)のページからランタイムを入手し、ホスティング バンドル インストーラーを実行して ASP.NET Core モジュールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="fd399-409">If you are attempting to install the .NET Core runtime on a system without an Internet connection, obtain the runtime from [.NET Downloads](https://www.microsoft.com/net/download/core) and run the hosting bundle installer to install the ASP.NET Core Module.</span></span> <span data-ttu-id="fd399-410">インストールを完了するために、システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。</span><span class="sxs-lookup"><span data-stu-id="fd399-410">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="fd399-411">フレームワークに依存する展開を配置し、システムまたは IIS を再起動せずに .NET Core をインストールした可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-411">You may have deployed a framework-dependent deployment and installed .NET Core without restarting the system/IIS.</span></span> <span data-ttu-id="fd399-412">システムを再起動するか、コマンド プロンプトから **net stop was /y** に続けて **net start w3svc** を実行して IIS を再起動します。</span><span class="sxs-lookup"><span data-stu-id="fd399-412">Either restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="fd399-413">フレームワークに依存する展開を配置し、*Microsoft Visual C++ 2015 再頒布可能パッケージ (x64)* がシステムにインストールされていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-413">You may have deployed a framework-dependent deployment and the *Microsoft Visual C++ 2015 Redistributable (x64)* is not installed on the system.</span></span> <span data-ttu-id="fd399-414">インストーラーは [Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=53840)から入手できます。</span><span class="sxs-lookup"><span data-stu-id="fd399-414">You may obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

### <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="fd399-415">\<aspNetCore\> 要素の引数の誤り</span><span class="sxs-lookup"><span data-stu-id="fd399-415">Incorrect arguments of \<aspNetCore\> element</span></span>

* <span data-ttu-id="fd399-416">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="fd399-416">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="fd399-417">**アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\my_application.dll', ErrorCode = '0x80004005 : 80008081. (物理ルート 'C:\{PATH}\' のアプリケーション 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' は、コマンド ライン '"dotnet" .\my_application.dll' でプロセスを開始できませんでした。ErrorCode = '0x80004005 : 80008081)</span><span class="sxs-lookup"><span data-stu-id="fd399-417">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\my_application.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="fd399-418">**ASP.NET Core モジュールのログ:** The application to execute does not exist: 'PATH\my_application.dll' (実行するアプリケーションが存在しません: 'PATH\my_application.dll')</span><span class="sxs-lookup"><span data-stu-id="fd399-418">**ASP.NET Core Module Log:** The application to execute does not exist: 'PATH\my_application.dll'</span></span>

<span data-ttu-id="fd399-419">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="fd399-419">Troubleshooting:</span></span>

* <span data-ttu-id="fd399-420">Kestrel でアプリケーションをローカルに実行できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-420">Confirm that the application runs locally on Kestrel.</span></span> <span data-ttu-id="fd399-421">プロセスのエラーは、アプリケーション内の問題の結果である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-421">A process failure might be the result of a problem within the application.</span></span> <span data-ttu-id="fd399-422">詳細については、「[トラブルシューティングのヒント](#troubleshooting-tips)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-422">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

* <span data-ttu-id="fd399-423">*web.config* で `<aspNetCore>` 要素の *arguments* 属性を調べ、次のいずれかになっていることを確認します。(a) フレームワークに依存する展開の場合は *.\my_applciation.dll*、または (b) 自己完結型の展開の場合は、未指定の空の文字列 (*arguments=""*) か、アプリケーションの引数のリスト (*arguments="arg1, arg2, ..."*)。</span><span class="sxs-lookup"><span data-stu-id="fd399-423">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it is either (a) *.\my_application.dll* for a framework-dependent deployment; or (b) not present, an empty string (*arguments=""*), or a list of your application's arguments (*arguments="arg1, arg2, ..."*) for a self-contained deployment.</span></span>

### <a name="missing-net-framework-version"></a><span data-ttu-id="fd399-424">.NET Framework バージョンの欠落</span><span class="sxs-lookup"><span data-stu-id="fd399-424">Missing .NET Framework version</span></span>

* <span data-ttu-id="fd399-425">**ブラウザー:** 502.3 ゲートウェイが正しくありません - There was a connection error while trying to route the request. (要求のルーティング中に接続エラーが発生しました。)</span><span class="sxs-lookup"><span data-stu-id="fd399-425">**Browser:** 502.3 Bad Gateway - There was a connection error while trying to route the request.</span></span>

* <span data-ttu-id="fd399-426">**アプリケーション ログ:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\my_application.dll', ErrorCode = '0x80004005 : 80008081. (ErrorCode = 物理ルート 'C:\{PATH}\' のアプリケーション 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' は、コマンド ライン '"dotnet" .\my_application.dll' でプロセスを開始できませんでした。ErrorCode = '0x80004005 : 80008081)</span><span class="sxs-lookup"><span data-stu-id="fd399-426">**Application Log:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\my_application.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="fd399-427">**ASP.NET Core モジュールのログ:** メソッド、ファイル、またはアセンブリの欠如の例外。</span><span class="sxs-lookup"><span data-stu-id="fd399-427">**ASP.NET Core Module Log:** Missing method, file, or assembly exception.</span></span> <span data-ttu-id="fd399-428">例外で特定されたメソッド、ファイル、またはアセンブリは、.NET Framework のメソッド、ファイル、またはアセンブリです。</span><span class="sxs-lookup"><span data-stu-id="fd399-428">The method, file, or assembly specified in the exception is a .NET Framework method, file, or assembly.</span></span>

<span data-ttu-id="fd399-429">トラブルシューティング:</span><span class="sxs-lookup"><span data-stu-id="fd399-429">Troubleshooting:</span></span>

* <span data-ttu-id="fd399-430">システムにない .NET Framework のバージョンをインストールします。</span><span class="sxs-lookup"><span data-stu-id="fd399-430">Install the .NET Framework version missing from the system.</span></span>

* <span data-ttu-id="fd399-431">フレームワークに依存する展開では、正しいランタイムがシステムにインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-431">For a framework-dependent deployment, confirm that you have the correct runtime installed on the system.</span></span> <span data-ttu-id="fd399-432">たとえば、プロジェクトを 1.0 から 1.1 にアップグレードしてからホスティング システムに展開し、この例外を受け取った場合は、ホスティング システムに 1.1 のフレームワークをインストールします。</span><span class="sxs-lookup"><span data-stu-id="fd399-432">For example if you upgrade a project from 1.0 to 1.1, deploy to the hosting system, and receive this exception, ensure you install the 1.1 framework on the hosting system.</span></span>

### <a name="stopped-application-pool"></a><span data-ttu-id="fd399-433">アプリケーション プールの停止</span><span class="sxs-lookup"><span data-stu-id="fd399-433">Stopped Application Pool</span></span>

* <span data-ttu-id="fd399-434">**ブラウザー:** 503 サービスを利用できません</span><span class="sxs-lookup"><span data-stu-id="fd399-434">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="fd399-435">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="fd399-435">**Application Log:** No entry</span></span>

* <span data-ttu-id="fd399-436">**ASP.NET Core モジュールのログ:** ログ ファイルは作成されません</span><span class="sxs-lookup"><span data-stu-id="fd399-436">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="fd399-437">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="fd399-437">Troubleshooting</span></span>

* <span data-ttu-id="fd399-438">アプリケーション プールが *[停止]* 状態でないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-438">Confirm that the Application Pool is not in the *Stopped* state.</span></span>

### <a name="iis-integration-middleware-not-implemented"></a><span data-ttu-id="fd399-439">IIS 統合ミドルウェアが未実装</span><span class="sxs-lookup"><span data-stu-id="fd399-439">IIS Integration middleware not implemented</span></span>

* <span data-ttu-id="fd399-440">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="fd399-440">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="fd399-441">**アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ' but either crashed or did not response or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4' (物理ルート 'C:\{PATH}\' のアプリケーション 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' は、コマンド ライン '"C:\{PATH}\my_application.{exe|dll}" ' でプロセスを作成しましたが、クラッシュしたか、応答しなかったか、指定されたポート '{PORT}' でリッスンしていません。ErrorCode = '0x800705b4')</span><span class="sxs-lookup"><span data-stu-id="fd399-441">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="fd399-442">**ASP.NET Core モジュールのログ:** ログ ファイルが作成され、正常動作を示しています。</span><span class="sxs-lookup"><span data-stu-id="fd399-442">**ASP.NET Core Module Log:** Log file created and shows normal operation.</span></span>

<span data-ttu-id="fd399-443">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="fd399-443">Troubleshooting</span></span>

* <span data-ttu-id="fd399-444">Kestrel でアプリケーションをローカルに実行できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-444">Confirm that the application runs locally on Kestrel.</span></span> <span data-ttu-id="fd399-445">プロセスのエラーは、アプリケーション内の問題の結果である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fd399-445">A process failure might be the result of a problem within the application.</span></span> <span data-ttu-id="fd399-446">詳細については、「[トラブルシューティングのヒント](#troubleshooting-tips)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-446">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

* <span data-ttu-id="fd399-447">アプリケーションの *WebHostBuilder()*で *.UseIISIntegration()* メソッドを呼び出して、IIS 統合ミドルウェアを正しく参照していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-447">Confirm that you have correctly referenced the IIS Integration middleware by calling the *.UseIISIntegration()* method on the application's *WebHostBuilder()*.</span></span>

### <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="fd399-448">サブアプリケーションに \<handlers\> セクションが含まれている</span><span class="sxs-lookup"><span data-stu-id="fd399-448">Sub-application includes a \<handlers\> section</span></span>

* <span data-ttu-id="fd399-449">**ブラウザー:** HTTP エラー 500.19 - 内部サーバー エラー</span><span class="sxs-lookup"><span data-stu-id="fd399-449">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="fd399-450">**アプリケーション ログ:** エントリはありません</span><span class="sxs-lookup"><span data-stu-id="fd399-450">**Application Log:** No entry</span></span>

* <span data-ttu-id="fd399-451">**ASP.NET Core モジュールのログ:** ログ ファイルが作成され、ルート アプリケーションの正常動作を示しています。</span><span class="sxs-lookup"><span data-stu-id="fd399-451">**ASP.NET Core Module Log:** Log file created and shows normal operation for the root application.</span></span> <span data-ttu-id="fd399-452">サブアプリケーションにログ ファイルは作成されません。</span><span class="sxs-lookup"><span data-stu-id="fd399-452">Log file not created for the sub-application.</span></span>

<span data-ttu-id="fd399-453">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="fd399-453">Troubleshooting</span></span>

* <span data-ttu-id="fd399-454">サブアプリケーションの *web.config* ファイルに `<handlers>` セクションが含まれていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-454">Confirm that the sub-application's *web.config* file doesn't include a `<handlers>` section.</span></span>

### <a name="application-configuration-general-issue"></a><span data-ttu-id="fd399-455">アプリケーション構成の一般的な問題</span><span class="sxs-lookup"><span data-stu-id="fd399-455">Application configuration general issue</span></span>

* <span data-ttu-id="fd399-456">**ブラウザー:** HTTP エラー 502.5 - 処理エラー</span><span class="sxs-lookup"><span data-stu-id="fd399-456">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="fd399-457">**アプリケーション ログ:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ' but either crashed or did not response or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4' (物理ルート 'C:\{PATH}\' のアプリケーション 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' は、コマンド ライン '"C:\{PATH}\my_application.{exe|dll}" ' でプロセスを作成しましたが、クラッシュしたか、応答しなかったか、指定されたポート '{PORT}' でリッスンしていません。ErrorCode = '0x800705b4')</span><span class="sxs-lookup"><span data-stu-id="fd399-457">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="fd399-458">**ASP.NET Core モジュールのログ:** ログ ファイルが作成されましたが空です</span><span class="sxs-lookup"><span data-stu-id="fd399-458">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="fd399-459">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="fd399-459">Troubleshooting</span></span>

* <span data-ttu-id="fd399-460">この一般例外はプロセスを開始できなかったことを示し、ほとんどの場合はアプリケーション構成の問題が原因です。</span><span class="sxs-lookup"><span data-stu-id="fd399-460">This general exception indicates that the process failed to start, most likely due to an application configuration issue.</span></span> <span data-ttu-id="fd399-461">[ディレクトリ構造](xref:hosting/directory-structure)に関する説明を参照して、アプリケーションの展開済みファイルとフォルダーが適切であること、アプリケーションの構成ファイルが存在し、そのファイルにアプリと環境に応じた正しい設定が含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd399-461">Referring to [Directory Structure](xref:hosting/directory-structure), confirm that your application's deployed files and folders are appropriate and that your application's configuration files are present and contain the correct settings for your app and environment.</span></span> <span data-ttu-id="fd399-462">詳細については、「[トラブルシューティングのヒント](#troubleshooting-tips)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd399-462">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

## <a name="resources"></a><span data-ttu-id="fd399-463">リソース</span><span class="sxs-lookup"><span data-stu-id="fd399-463">Resources</span></span>

* [<span data-ttu-id="fd399-464">ASP.NET Core モジュールの概要</span><span class="sxs-lookup"><span data-stu-id="fd399-464">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)

* [<span data-ttu-id="fd399-465">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="fd399-465">ASP.NET Core Module configuration reference</span></span>](xref:hosting/aspnet-core-module)

* [<span data-ttu-id="fd399-466">IIS モジュールと ASP.NET Core の使用</span><span class="sxs-lookup"><span data-stu-id="fd399-466">Using IIS Modules with ASP.NET Core</span></span>](xref:hosting/iis-modules)

* [<span data-ttu-id="fd399-467">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="fd399-467">Introduction to ASP.NET Core</span></span>](../index.md)

* [<span data-ttu-id="fd399-468">Microsoft IIS 公式サイト</span><span class="sxs-lookup"><span data-stu-id="fd399-468">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)

* [<span data-ttu-id="fd399-469">Microsoft TechNet ライブラリ: Windows Server</span><span class="sxs-lookup"><span data-stu-id="fd399-469">Microsoft TechNet Library: Windows Server</span></span>](https://docs.microsoft.com/windows-server/windows-server-versions)
