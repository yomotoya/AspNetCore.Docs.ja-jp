---
title: ASP.NET Core での Windows 認証を構成します。
author: scottaddie
description: ASP.NET Core での IIS と HTTP.sys は Windows 認証を構成する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/05/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 900bbf5f14b1876ad537b2b77e4ba07d7aa168f2
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750167"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="0d32c-103">ASP.NET Core での Windows 認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="0d32c-104">によって[Scott Addie](https://twitter.com/Scott_Addie)と[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0d32c-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0d32c-105">[Windows 認証](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)でホストされている ASP.NET Core アプリ用に構成できます[IIS](xref:host-and-deploy/iis/index)または[HTTP.sys](xref:fundamentals/servers/httpsys)します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-105">[Windows Authentication](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="0d32c-106">Windows 認証は、ASP.NET Core アプリのユーザーを認証するオペレーティング システムに依存します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-106">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="0d32c-107">ユーザーを識別するために Active Directory ドメインの id または Windows アカウントを使用して、企業ネットワークで、サーバーの実行時に、Windows 認証を使用できます。</span><span class="sxs-lookup"><span data-stu-id="0d32c-107">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="0d32c-108">Windows 認証は、同じ Windows ドメインに属しているユーザー、クライアント アプリ、および web サーバーのイントラネット環境に最適です。</span><span class="sxs-lookup"><span data-stu-id="0d32c-108">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

## <a name="iisiis-express"></a><span data-ttu-id="0d32c-109">IIS または IIS Express</span><span class="sxs-lookup"><span data-stu-id="0d32c-109">IIS/IIS Express</span></span>

<span data-ttu-id="0d32c-110">認証サービスを呼び出すことによって追加<xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>(<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName>名前空間) で`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0d32c-110">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a><span data-ttu-id="0d32c-111">起動設定 (デバッガー)</span><span class="sxs-lookup"><span data-stu-id="0d32c-111">Launch settings (debugger)</span></span>

<span data-ttu-id="0d32c-112">起動設定の構成にのみ影響、 *Properties/launchSettings.json* IIS Express 用のファイルし、IIS の Windows 認証を構成しません。</span><span class="sxs-lookup"><span data-stu-id="0d32c-112">Configuration for launch settings only affects the *Properties/launchSettings.json* file for IIS Express and doesn't configure IIS for Windows Authentication.</span></span> <span data-ttu-id="0d32c-113">サーバーの構成については、 [IIS](#iis)セクション。</span><span class="sxs-lookup"><span data-stu-id="0d32c-113">Server configuration is explained in the [IIS](#iis) section.</span></span>

<span data-ttu-id="0d32c-114">**Web アプリケーション**を更新する Windows 認証をサポートする Visual Studio または .NET Core CLI を使用して利用可能なテンプレートを構成することができます、 *Properties/launchSettings.json*ファイル自動的に。</span><span class="sxs-lookup"><span data-stu-id="0d32c-114">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication, which updates the *Properties/launchSettings.json* file automatically.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0d32c-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d32c-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0d32c-116">**新しいプロジェクト**</span><span class="sxs-lookup"><span data-stu-id="0d32c-116">**New project**</span></span>

1. <span data-ttu-id="0d32c-117">新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-117">Create a new project.</span></span>
1. <span data-ttu-id="0d32c-118">**[ASP.NET Core Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="0d32c-119">**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-119">Select **Next**.</span></span>
1. <span data-ttu-id="0d32c-120">名前を入力、**プロジェクト名**フィールド。</span><span class="sxs-lookup"><span data-stu-id="0d32c-120">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="0d32c-121">確認、**場所**エントリが正しいか、プロジェクトの場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-121">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="0d32c-122">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-122">Select **Create**.</span></span>
1. <span data-ttu-id="0d32c-123">選択**変更** **認証**します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-123">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="0d32c-124">**認証の変更**ウィンドウで、 **Windows 認証**します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-124">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="0d32c-125">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-125">Select **OK**.</span></span>
1. <span data-ttu-id="0d32c-126">**[Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-126">Select **Web Application**.</span></span>
1. <span data-ttu-id="0d32c-127">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-127">Select **Create**.</span></span>

<span data-ttu-id="0d32c-128">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-128">Run the app.</span></span> <span data-ttu-id="0d32c-129">ユーザー名は、レンダリングされたアプリのユーザー インターフェイスに表示されます。</span><span class="sxs-lookup"><span data-stu-id="0d32c-129">The username appears in the rendered app's user interface.</span></span>

<span data-ttu-id="0d32c-130">**既存のプロジェクト**</span><span class="sxs-lookup"><span data-stu-id="0d32c-130">**Existing project**</span></span>

<span data-ttu-id="0d32c-131">プロジェクトのプロパティは、Windows 認証を有効にして、匿名認証を無効にします。</span><span class="sxs-lookup"><span data-stu-id="0d32c-131">The project's properties enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="0d32c-132">**ソリューション エクスプローラー**でプロジェクトを右クリックして、 **[プロパティ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-132">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="0d32c-133">**[デバッグ]** タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-133">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="0d32c-134">チェック ボックスをオフ**匿名認証を有効にする**します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-134">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="0d32c-135">チェック ボックスをオン**Windows 認証を有効にする**します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-135">Select the check box for **Enable Windows Authentication**.</span></span>
1. <span data-ttu-id="0d32c-136">保存して、プロパティ ページを閉じます。</span><span class="sxs-lookup"><span data-stu-id="0d32c-136">Save and close the property page.</span></span>

<span data-ttu-id="0d32c-137">プロパティを構成する代わりに、`iisSettings`のノード、 *launchSettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0d32c-137">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="0d32c-138">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="0d32c-138">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="0d32c-139">**新しいプロジェクト**</span><span class="sxs-lookup"><span data-stu-id="0d32c-139">**New project**</span></span>

<span data-ttu-id="0d32c-140">実行、[新しい dotnet](/dotnet/core/tools/dotnet-new)コマンドと、`webapp`引数 (ASP.NET Core Web アプリ) と`--auth Windows`スイッチします。</span><span class="sxs-lookup"><span data-stu-id="0d32c-140">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

<span data-ttu-id="0d32c-141">**既存のプロジェクト**</span><span class="sxs-lookup"><span data-stu-id="0d32c-141">**Existing project**</span></span>

<span data-ttu-id="0d32c-142">更新プログラム、`iisSettings`のノード、 *launchSettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0d32c-142">Update the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

<span data-ttu-id="0d32c-143">既存のプロジェクトを変更する場合は、プロジェクト ファイルにはへのパッケージ参照が含まれていることを確認、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)**または**、 [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="0d32c-143">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

### <a name="iis"></a><span data-ttu-id="0d32c-144">IIS</span><span class="sxs-lookup"><span data-stu-id="0d32c-144">IIS</span></span>

<span data-ttu-id="0d32c-145">IIS を使用して、 [ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)ASP.NET Core アプリをホストします。</span><span class="sxs-lookup"><span data-stu-id="0d32c-145">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="0d32c-146">使用した IIS の Windows 認証が構成されている、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0d32c-146">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="0d32c-147">以下のセクションで表示する方法。</span><span class="sxs-lookup"><span data-stu-id="0d32c-147">The following sections show how to:</span></span>

* <span data-ttu-id="0d32c-148">ローカルの提供*web.config*ファイルをアプリが展開されると、サーバーで Windows 認証をアクティブにします。</span><span class="sxs-lookup"><span data-stu-id="0d32c-148">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="0d32c-149">IIS マネージャーを使用して、構成、 *web.config*サーバーに既に展開されている ASP.NET Core アプリのファイル。</span><span class="sxs-lookup"><span data-stu-id="0d32c-149">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

<span data-ttu-id="0d32c-150">これをいない場合は、ASP.NET Core アプリをホストする IIS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="0d32c-150">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="0d32c-151">詳細については、「 <xref:host-and-deploy/iis/index> 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0d32c-151">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="0d32c-152">Windows 認証の IIS の役割サービスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="0d32c-152">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="0d32c-153">詳細については、[(手順 2 参照)、IIS の役割サービスで Windows 認証を有効にする](xref:host-and-deploy/iis/index#iis-configuration)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0d32c-153">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="0d32c-154">[IIS 統合ミドルウェア](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)既定で自動的に要求の認証に構成されます。</span><span class="sxs-lookup"><span data-stu-id="0d32c-154">[IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="0d32c-155">詳細については、次を参照してください。 [ASP.NET Core の IIS と Windows ホスト。IIS のオプション (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options)します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-155">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="0d32c-156">ASP.NET Core モジュールは、既定では、アプリに Windows 認証トークンを転送するように構成されます。</span><span class="sxs-lookup"><span data-stu-id="0d32c-156">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="0d32c-157">詳細については、次を参照してください。 [ASP.NET Core モジュール構成リファレンス。AspNetCore 要素の属性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-157">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

<span data-ttu-id="0d32c-158">使用**か**の次の方法。</span><span class="sxs-lookup"><span data-stu-id="0d32c-158">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="0d32c-159">**発行して、プロジェクトを配置する前に**次の追加*web.config*ファイルをプロジェクトのルート。</span><span class="sxs-lookup"><span data-stu-id="0d32c-159">**Before publishing and deploying the project,** add the following *web.config* file to the project root:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  <span data-ttu-id="0d32c-160">プロジェクトが .NET Core SDK によって公開されると (なし、`<IsTransformWebConfigDisabled>`プロパティに設定`true`プロジェクト ファイル内)、公開された*web.config*ファイルが含まれています、`<location><system.webServer><security><authentication>`セクション。</span><span class="sxs-lookup"><span data-stu-id="0d32c-160">When the project is published by the .NET Core SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="0d32c-161">詳細については、`<IsTransformWebConfigDisabled>`プロパティを参照してください<xref:host-and-deploy/iis/index#webconfig-file>します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-161">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

* <span data-ttu-id="0d32c-162">**発行し、プロジェクトを配置した後**サーバー側の構成、IIS マネージャーでを実行します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-162">**After publishing and deploying the project,** perform server-side configuration with the IIS Manager:</span></span>

  1. <span data-ttu-id="0d32c-163">IIS マネージャーで、下にある IIS サイトを選択して、**サイト**のノード、**接続**サイドバーです。</span><span class="sxs-lookup"><span data-stu-id="0d32c-163">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
  1. <span data-ttu-id="0d32c-164">ダブルクリック**認証**で、 **IIS**領域。</span><span class="sxs-lookup"><span data-stu-id="0d32c-164">Double-click **Authentication** in the **IIS** area.</span></span>
  1. <span data-ttu-id="0d32c-165">選択**匿名認証**します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-165">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="0d32c-166">選択**を無効にする**で、**アクション**サイドバーです。</span><span class="sxs-lookup"><span data-stu-id="0d32c-166">Select **Disable** in the **Actions** sidebar.</span></span>
  1. <span data-ttu-id="0d32c-167">選択**Windows 認証**します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-167">Select **Windows Authentication**.</span></span> <span data-ttu-id="0d32c-168">選択**を有効にする**で、**アクション**サイドバーです。</span><span class="sxs-lookup"><span data-stu-id="0d32c-168">Select **Enable** in the **Actions** sidebar.</span></span>

  <span data-ttu-id="0d32c-169">これらのアクションが実行したときに、IIS マネージャーは、アプリを変更します*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0d32c-169">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="0d32c-170">A`<system.webServer><security><authentication>`ノードが更新された設定を使用した追加`anonymousAuthentication`と`windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="0d32c-170">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  <span data-ttu-id="0d32c-171">`<system.webServer>`セクションに追加、 *web.config*ファイルを IIS マネージャーでは、アプリの外部で`<location>`は .NET Core SDK により、アプリが公開されるときに追加されたセクションです。</span><span class="sxs-lookup"><span data-stu-id="0d32c-171">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="0d32c-172">外部のセクションが追加されるため、`<location>`ノード、いずれかで、設定が継承されます[サブ アプリ](xref:host-and-deploy/iis/index#sub-applications)現在のアプリにします。</span><span class="sxs-lookup"><span data-stu-id="0d32c-172">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="0d32c-173">継承を防ぐためには、移動、追加した`<security>`内のセクション、 `<location><system.webServer>` .NET Core SDK が提供されているセクション。</span><span class="sxs-lookup"><span data-stu-id="0d32c-173">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the .NET Core SDK provided.</span></span>

  <span data-ttu-id="0d32c-174">IIS マネージャーを使用するには、IIS の構成を追加する、影響を受けるのみアプリの*web.config*サーバー上のファイル。</span><span class="sxs-lookup"><span data-stu-id="0d32c-174">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="0d32c-175">場合、アプリの後続の配置は、サーバーの設定を上書き可能性があります、サーバーのコピーの*web.config*はプロジェクトの置き換え*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0d32c-175">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="0d32c-176">使用**か**の設定を管理する次の方法。</span><span class="sxs-lookup"><span data-stu-id="0d32c-176">Use **either** of the following approaches to manage the settings:</span></span>

  * <span data-ttu-id="0d32c-177">設定をリセットする IIS マネージャーを使用して、 *web.config*ファイルについては、展開で、ファイルが上書きされます。</span><span class="sxs-lookup"><span data-stu-id="0d32c-177">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
  * <span data-ttu-id="0d32c-178">追加、 *web.config ファイル*アプリの設定でローカルにします。</span><span class="sxs-lookup"><span data-stu-id="0d32c-178">Add a *web.config file* to the app locally with the settings.</span></span>

## <a name="httpsys"></a><span data-ttu-id="0d32c-179">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="0d32c-179">HTTP.sys</span></span>

<span data-ttu-id="0d32c-180">自己ホスト型のシナリオで[Kestrel](xref:fundamentals/servers/kestrel)が Windows 認証をサポートしないを使用できる[HTTP.sys](xref:fundamentals/servers/httpsys)します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-180">In self-hosted scenarios, [Kestrel](xref:fundamentals/servers/kestrel) doesn't support Windows Authentication, but you can use [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="0d32c-181">認証サービスを呼び出すことによって追加<xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>(<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName>名前空間) で`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0d32c-181">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

<span data-ttu-id="0d32c-182">Windows 認証を使用した HTTP.sys を使用するアプリの web ホストを構成する (*Program.cs*)。</span><span class="sxs-lookup"><span data-stu-id="0d32c-182">Configure the app's web host to use HTTP.sys with Windows Authentication (*Program.cs*).</span></span> <span data-ttu-id="0d32c-183"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName>名前空間。</span><span class="sxs-lookup"><span data-stu-id="0d32c-183"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> is in the <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="0d32c-184">HTTP.sys では、Kerberos 認証プロトコルを使用したカーネル モード認証に処理が委任されます。</span><span class="sxs-lookup"><span data-stu-id="0d32c-184">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="0d32c-185">Kerberos および HTTP.sys ではユーザー モード認証がサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="0d32c-185">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="0d32c-186">Active Directory から取得され、クライアントによって、ユーザーを認証するサーバーに転送される Kerberos トークン/チケットを暗号化解除するには、コンピューター アカウントを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0d32c-186">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="0d32c-187">アプリのユーザーではなく、ホストのサービス プリンシパル名 (SPN) を登録します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-187">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="0d32c-188">HTTP.sys は、Nano Server バージョン 1709 以降でサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="0d32c-188">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="0d32c-189">Nano Server の Windows 認証と HTTP.sys を使用する、 [(microsoft/windowsservercore) の Server Core コンテナー](https://hub.docker.com/r/microsoft/windowsservercore/)します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-189">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="0d32c-190">Server Core の詳細については、[Windows Server の Server Core インストール オプションとは何ですか?](/windows-server/administration/server-core/what-is-server-core)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0d32c-190">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="authorize-users"></a><span data-ttu-id="0d32c-191">ユーザーを認証します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-191">Authorize users</span></span>

<span data-ttu-id="0d32c-192">匿名アクセスの構成の状態にする方法が決定します、`[Authorize]`と`[AllowAnonymous]`属性は、アプリで使用します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-192">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="0d32c-193">次の 2 つのセクションでは、匿名アクセスの許可されていないと、許可されている構成の状態を処理する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-193">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="0d32c-194">匿名アクセスを禁止します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-194">Disallow anonymous access</span></span>

<span data-ttu-id="0d32c-195">Windows 認証が有効になっており、匿名アクセスが無効になっているときに、`[Authorize]`と`[AllowAnonymous]`属性は影響ありません。</span><span class="sxs-lookup"><span data-stu-id="0d32c-195">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="0d32c-196">匿名アクセスを禁止する IIS サイトを構成する場合、要求がアプリに到達しません。</span><span class="sxs-lookup"><span data-stu-id="0d32c-196">If an IIS site is configured to disallow anonymous access, the request never reaches the app.</span></span> <span data-ttu-id="0d32c-197">このため、`[AllowAnonymous]`属性には適用されません。</span><span class="sxs-lookup"><span data-stu-id="0d32c-197">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="0d32c-198">匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-198">Allow anonymous access</span></span>

<span data-ttu-id="0d32c-199">Windows 認証と匿名アクセスの両方が有効になっているときに使用して、`[Authorize]`と`[AllowAnonymous]`属性。</span><span class="sxs-lookup"><span data-stu-id="0d32c-199">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="0d32c-200">`[Authorize]`属性では、認証を必要とするアプリのエンドポイントをセキュリティで保護できます。</span><span class="sxs-lookup"><span data-stu-id="0d32c-200">The `[Authorize]` attribute allows you to secure endpoints of the app which require authentication.</span></span> <span data-ttu-id="0d32c-201">`[AllowAnonymous]`属性のオーバーライド、`[Authorize]`への匿名アクセスを許可するアプリ内の属性。</span><span class="sxs-lookup"><span data-stu-id="0d32c-201">The `[AllowAnonymous]` attribute overrides the `[Authorize]` attribute in apps that allow anonymous access.</span></span> <span data-ttu-id="0d32c-202">属性の使用方法の詳細を参照してください。<xref:security/authorization/simple>します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-202">For attribute usage details, see <xref:security/authorization/simple>.</span></span>

> [!NOTE]
> <span data-ttu-id="0d32c-203">既定では、ページにアクセスするための承認を持たないユーザーには、空の HTTP 403 応答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0d32c-203">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="0d32c-204">[StatusCodePages ミドルウェア](xref:fundamentals/error-handling#usestatuscodepages)「アクセスが拒否されました」のより優れたエクスペリエンスをユーザーに提供するように構成できます。</span><span class="sxs-lookup"><span data-stu-id="0d32c-204">The [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

## <a name="impersonation"></a><span data-ttu-id="0d32c-205">偽装</span><span class="sxs-lookup"><span data-stu-id="0d32c-205">Impersonation</span></span>

<span data-ttu-id="0d32c-206">ASP.NET Core では、権限借用を実装しません。</span><span class="sxs-lookup"><span data-stu-id="0d32c-206">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="0d32c-207">アプリは、アプリ プールまたはプロセス id を使用して、すべての要求をアプリの id で実行します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-207">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="0d32c-208">使用の場合は、アプリは、ユーザーの代理としてアクションを実行する必要があります、 [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*)で、[ターミナル インライン ミドルウェア](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)で`Startup.Configure`します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-208">If the app should perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="0d32c-209">このコンテキストで 1 つのアクションを実行し、コンテキストを閉じます。</span><span class="sxs-lookup"><span data-stu-id="0d32c-209">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="0d32c-210">`RunImpersonated` 非同期操作をサポートしていないし、複雑なシナリオでは使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="0d32c-210">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="0d32c-211">全体要求またはミドルウェアのチェーンをラッピングされていないサポートなど、お勧めします。</span><span class="sxs-lookup"><span data-stu-id="0d32c-211">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

## <a name="claims-transformations"></a><span data-ttu-id="0d32c-212">要求の変換</span><span class="sxs-lookup"><span data-stu-id="0d32c-212">Claims transformations</span></span>

<span data-ttu-id="0d32c-213">IIS のインプロセス モードでホストする場合<xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*>されていないユーザーを初期化するために内部的に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="0d32c-213">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="0d32c-214">そのため、認証のたびに要求を変換するための <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 実装は既定で有効になっていません。</span><span class="sxs-lookup"><span data-stu-id="0d32c-214">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="0d32c-215">詳細については、インプロセスをホストする場合は、クレームの変換をアクティブにするためのコード例を参照してください。<xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>します。</span><span class="sxs-lookup"><span data-stu-id="0d32c-215">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0d32c-216">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="0d32c-216">Additional resources</span></span>

* [<span data-ttu-id="0d32c-217">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="0d32c-217">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
