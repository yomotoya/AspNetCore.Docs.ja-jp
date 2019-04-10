---
title: ASP.NET Core での Windows 認証を構成します。
author: scottaddie
description: ASP.NET core で IIS Express、IIS、および HTTP.sys を使用して Windows 認証を構成する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 04/03/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: bd4ffa79c4d1e0070c820fa9c06b0a84c3aaae74
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468654"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="72d9c-103">ASP.NET Core での Windows 認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="72d9c-104">によって[Scott Addie](https://twitter.com/Scott_Addie)と[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="72d9c-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="72d9c-105">[Windows 認証](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)でホストされている ASP.NET Core アプリ用に構成できます[IIS](xref:host-and-deploy/iis/index)または[HTTP.sys](xref:fundamentals/servers/httpsys)します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-105">[Windows Authentication](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="72d9c-106">Windows 認証は、ASP.NET Core アプリのユーザーを認証するオペレーティング システムに依存します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-106">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="72d9c-107">ユーザーを識別するために Active Directory ドメインの id または Windows アカウントを使用して、企業ネットワークで、サーバーの実行時に、Windows 認証を使用できます。</span><span class="sxs-lookup"><span data-stu-id="72d9c-107">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="72d9c-108">Windows 認証は、同じ Windows ドメインに属しているユーザー、クライアント アプリ、および web サーバーのイントラネット環境に最適です。</span><span class="sxs-lookup"><span data-stu-id="72d9c-108">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="72d9c-109">ASP.NET Core アプリで Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="72d9c-109">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="72d9c-110">**Web アプリケーション**Windows 認証をサポートする Visual Studio または .NET Core CLI を使用して利用可能なテンプレートを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="72d9c-110">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication.</span></span>

# [<a name="visual-studio"></a><span data-ttu-id="72d9c-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="72d9c-111">Visual Studio</span></span>](#tab/visual-studio)

### <a name="use-the-windows-authentication-app-template-for-a-new-project"></a><span data-ttu-id="72d9c-112">新しいプロジェクトに Windows 認証アプリ テンプレートを使用</span><span class="sxs-lookup"><span data-stu-id="72d9c-112">Use the Windows Authentication app template for a new project</span></span>

<span data-ttu-id="72d9c-113">Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="72d9c-113">In Visual Studio:</span></span>

1. <span data-ttu-id="72d9c-114">新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-114">Create a new project.</span></span>
1. <span data-ttu-id="72d9c-115">**[ASP.NET Core Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-115">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="72d9c-116">**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-116">Select **Next**.</span></span>
1. <span data-ttu-id="72d9c-117">名前を入力、**プロジェクト名**フィールド。</span><span class="sxs-lookup"><span data-stu-id="72d9c-117">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="72d9c-118">確認、**場所**エントリが正しいか、プロジェクトの場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-118">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="72d9c-119">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-119">Select **Create**.</span></span>
1. <span data-ttu-id="72d9c-120">選択**変更\*\*\*\*認証**します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-120">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="72d9c-121">**認証の変更**ウィンドウで、 **Windows 認証**します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-121">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="72d9c-122">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-122">Select **OK**.</span></span>
1. <span data-ttu-id="72d9c-123">**[Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-123">Select **Web Application**.</span></span>
1. <span data-ttu-id="72d9c-124">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-124">Select **Create**.</span></span>

<span data-ttu-id="72d9c-125">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-125">Run the app.</span></span> <span data-ttu-id="72d9c-126">ユーザー名は、レンダリングされたアプリのユーザー インターフェイスに表示されます。</span><span class="sxs-lookup"><span data-stu-id="72d9c-126">The username appears in the rendered app's user interface.</span></span>

### <a name="manual-configuration-for-an-existing-project"></a><span data-ttu-id="72d9c-127">既存のプロジェクトの手動構成</span><span class="sxs-lookup"><span data-stu-id="72d9c-127">Manual configuration for an existing project</span></span>

<span data-ttu-id="72d9c-128">プロジェクトのプロパティは、Windows 認証を有効にして、匿名認証を無効化を使用します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-128">The project's properties allow you to enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="72d9c-129">Visual studio のプロジェクトを右クリックして**ソリューション エクスプ ローラー**選択**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-129">Right-click the project in Visual Studio's **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="72d9c-130">**[デバッグ]** タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-130">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="72d9c-131">チェック ボックスをオフ**匿名認証を有効にする**します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-131">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="72d9c-132">チェック ボックスをオン**Windows 認証を有効にする**します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-132">Select the check box for **Enable Windows Authentication**.</span></span>

<span data-ttu-id="72d9c-133">プロパティを構成する代わりに、`iisSettings`のノード、 *launchSettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="72d9c-133">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# [<a name="net-core-cli"></a><span data-ttu-id="72d9c-134">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="72d9c-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="72d9c-135">使用して、 **Windows 認証**アプリ テンプレート。</span><span class="sxs-lookup"><span data-stu-id="72d9c-135">Use the **Windows Authentication** app template.</span></span>

<span data-ttu-id="72d9c-136">実行、[新しい dotnet](/dotnet/core/tools/dotnet-new)コマンドと、`webapp`引数 (ASP.NET Core Web アプリ) と`--auth Windows`スイッチします。</span><span class="sxs-lookup"><span data-stu-id="72d9c-136">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

---

<span data-ttu-id="72d9c-137">既存のプロジェクトを変更する場合は、プロジェクト ファイルにはへのパッケージ参照が含まれていることを確認、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)**または**、 [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="72d9c-137">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="72d9c-138">IIS での Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="72d9c-138">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="72d9c-139">IIS を使用して、 [ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)ASP.NET Core アプリをホストします。</span><span class="sxs-lookup"><span data-stu-id="72d9c-139">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="72d9c-140">使用した IIS の Windows 認証が構成されている、 *web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="72d9c-140">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="72d9c-141">以下のセクションで表示する方法。</span><span class="sxs-lookup"><span data-stu-id="72d9c-141">The following sections show how to:</span></span>

* <span data-ttu-id="72d9c-142">ローカルの提供*web.config*ファイルをアプリが展開されると、サーバーで Windows 認証をアクティブにします。</span><span class="sxs-lookup"><span data-stu-id="72d9c-142">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="72d9c-143">IIS マネージャーを使用して、構成、 *web.config*サーバーに既に展開されている ASP.NET Core アプリのファイル。</span><span class="sxs-lookup"><span data-stu-id="72d9c-143">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="72d9c-144">IIS 構成</span><span class="sxs-lookup"><span data-stu-id="72d9c-144">IIS configuration</span></span>

<span data-ttu-id="72d9c-145">これをいない場合は、ASP.NET Core アプリをホストする IIS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="72d9c-145">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="72d9c-146">詳細については、「 <xref:host-and-deploy/iis/index> 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="72d9c-146">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="72d9c-147">Windows 認証の IIS の役割サービスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="72d9c-147">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="72d9c-148">詳細については、[(手順 2 参照)、IIS の役割サービスで Windows 認証を有効にする](xref:host-and-deploy/iis/index#iis-configuration)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="72d9c-148">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="72d9c-149">IIS 統合ミドルウェアは、既定で自動的に要求の認証に構成されます。</span><span class="sxs-lookup"><span data-stu-id="72d9c-149">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="72d9c-150">詳細については、次を参照してください。 [ASP.NET Core の IIS と Windows ホスト。IIS のオプション (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options)します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-150">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="72d9c-151">ASP.NET Core モジュールは、既定では、アプリに Windows 認証トークンを転送するように構成されます。</span><span class="sxs-lookup"><span data-stu-id="72d9c-151">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="72d9c-152">詳細については、次を参照してください。 [ASP.NET Core モジュール構成リファレンス。AspNetCore 要素の属性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-152">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="72d9c-153">新しい IIS サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-153">Create a new IIS site</span></span>

<span data-ttu-id="72d9c-154">名前とフォルダーを指定し、新しいアプリケーション プールを作成することを許可します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-154">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="enable-windows-authentication-for-the-app-in-iis"></a><span data-ttu-id="72d9c-155">IIS でアプリケーションの Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="72d9c-155">Enable Windows Authentication for the app in IIS</span></span>

<span data-ttu-id="72d9c-156">使用**か**の次の方法。</span><span class="sxs-lookup"><span data-stu-id="72d9c-156">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="72d9c-157">[アプリを発行する前に開発側の構成](#development-side-configuration-with-a-local-webconfig-file)(*推奨*)</span><span class="sxs-lookup"><span data-stu-id="72d9c-157">[Development-side configuration before publishing the app](#development-side-configuration-with-a-local-webconfig-file) (*Recommended*)</span></span>
* [<span data-ttu-id="72d9c-158">アプリを発行した後、サーバー側の構成</span><span class="sxs-lookup"><span data-stu-id="72d9c-158">Server-side configuration after publishing the app</span></span>](#server-side-configuration-with-the-iis-manager)

#### <a name="development-side-configuration-with-a-local-webconfig-file"></a><span data-ttu-id="72d9c-159">ローカルの web.config ファイルを使用して開発側の構成</span><span class="sxs-lookup"><span data-stu-id="72d9c-159">Development-side configuration with a local web.config file</span></span>

<span data-ttu-id="72d9c-160">次の手順に従います**する前に**する[発行し、プロジェクトを配置](#publish-and-deploy-your-project-to-the-iis-site-folder)します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-160">Perform the following steps **before** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

<span data-ttu-id="72d9c-161">次の追加*web.config*ファイルをプロジェクトのルート。</span><span class="sxs-lookup"><span data-stu-id="72d9c-161">Add the following *web.config* file to the project root:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_2.config)]

<span data-ttu-id="72d9c-162">プロジェクトが SDK によって公開されると (なし、`<IsTransformWebConfigDisabled>`プロパティに設定`true`プロジェクト ファイル内)、公開された*web.config*ファイルが含まれています、`<location><system.webServer><security><authentication>`セクション。</span><span class="sxs-lookup"><span data-stu-id="72d9c-162">When the project is published by the SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="72d9c-163">詳細については、`<IsTransformWebConfigDisabled>`プロパティを参照してください<xref:host-and-deploy/iis/index#webconfig-file>します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-163">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

#### <a name="server-side-configuration-with-the-iis-manager"></a><span data-ttu-id="72d9c-164">IIS マネージャーでサーバー側の構成</span><span class="sxs-lookup"><span data-stu-id="72d9c-164">Server-side configuration with the IIS Manager</span></span>

<span data-ttu-id="72d9c-165">次の手順に従います**後**する[発行し、プロジェクトを配置](#publish-and-deploy-your-project-to-the-iis-site-folder)します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-165">Perform the following steps **after** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

1. <span data-ttu-id="72d9c-166">IIS マネージャーで、下にある IIS サイトを選択して、**サイト**のノード、**接続**サイドバーです。</span><span class="sxs-lookup"><span data-stu-id="72d9c-166">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
1. <span data-ttu-id="72d9c-167">ダブルクリック**認証**で、 **IIS**領域。</span><span class="sxs-lookup"><span data-stu-id="72d9c-167">Double-click **Authentication** in the **IIS** area.</span></span>
1. <span data-ttu-id="72d9c-168">選択**匿名認証**します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-168">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="72d9c-169">選択**を無効にする**で、**アクション**サイドバーです。</span><span class="sxs-lookup"><span data-stu-id="72d9c-169">Select **Disable** in the **Actions** sidebar.</span></span>
1. <span data-ttu-id="72d9c-170">選択**Windows 認証**します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-170">Select **Windows Authentication**.</span></span> <span data-ttu-id="72d9c-171">選択**を有効にする**で、**アクション**サイドバーです。</span><span class="sxs-lookup"><span data-stu-id="72d9c-171">Select **Enable** in the **Actions** sidebar.</span></span>

<span data-ttu-id="72d9c-172">これらのアクションが実行したときに、IIS マネージャーは、アプリを変更します*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="72d9c-172">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="72d9c-173">A`<system.webServer><security><authentication>`ノードが更新された設定を使用した追加`anonymousAuthentication`と`windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="72d9c-173">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

<span data-ttu-id="72d9c-174">`<system.webServer>`セクションに追加、 *web.config*ファイルを IIS マネージャーでは、アプリの外部で`<location>`は .NET Core SDK により、アプリが公開されるときに追加されたセクションです。</span><span class="sxs-lookup"><span data-stu-id="72d9c-174">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="72d9c-175">外部のセクションが追加されるため、`<location>`ノード、いずれかで、設定が継承されます[サブ アプリ](xref:host-and-deploy/iis/index#sub-applications)現在のアプリにします。</span><span class="sxs-lookup"><span data-stu-id="72d9c-175">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="72d9c-176">継承を防ぐためには、移動、追加した`<security>`内のセクション、`<location><system.webServer>`セクション、SDK が提供されます。</span><span class="sxs-lookup"><span data-stu-id="72d9c-176">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the SDK provided.</span></span>

<span data-ttu-id="72d9c-177">IIS マネージャーを使用するには、IIS の構成を追加する、影響を受けるのみアプリの*web.config*サーバー上のファイル。</span><span class="sxs-lookup"><span data-stu-id="72d9c-177">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="72d9c-178">場合、アプリの後続の配置は、サーバーの設定を上書き可能性があります、サーバーのコピーの*web.config*はプロジェクトの置き換え*web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="72d9c-178">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="72d9c-179">使用**か**の設定を管理する次の方法。</span><span class="sxs-lookup"><span data-stu-id="72d9c-179">Use **either** of the following approaches to manage the settings:</span></span>

* <span data-ttu-id="72d9c-180">設定をリセットする IIS マネージャーを使用して、 *web.config*ファイルについては、展開で、ファイルが上書きされます。</span><span class="sxs-lookup"><span data-stu-id="72d9c-180">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
* <span data-ttu-id="72d9c-181">追加、 *web.config ファイル*アプリの設定でローカルにします。</span><span class="sxs-lookup"><span data-stu-id="72d9c-181">Add a *web.config file* to the app locally with the settings.</span></span> <span data-ttu-id="72d9c-182">詳細については、次を参照してください。、[開発側の構成](#development-side-configuration-with-a-local-webconfig-file)セクション。</span><span class="sxs-lookup"><span data-stu-id="72d9c-182">For more information, see the [Development-side configuration](#development-side-configuration-with-a-local-webconfig-file) section.</span></span>

### <a name="publish-and-deploy-your-project-to-the-iis-site-folder"></a><span data-ttu-id="72d9c-183">発行し、IIS サイトのフォルダーにプロジェクトを配置</span><span class="sxs-lookup"><span data-stu-id="72d9c-183">Publish and deploy your project to the IIS site folder</span></span>

<span data-ttu-id="72d9c-184">Visual Studio または .NET Core CLI を使用して、発行し、目的のフォルダーに、アプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="72d9c-184">Using Visual Studio or the .NET Core CLI, publish and deploy the app to the destination folder.</span></span>

<span data-ttu-id="72d9c-185">IIS でホストの詳細については、発行、および配置では、次のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="72d9c-185">For more information on hosting with IIS, publishing, and deployment, see the following topics:</span></span>

* [<span data-ttu-id="72d9c-186">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="72d9c-186">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>

<span data-ttu-id="72d9c-187">Windows 認証が動作していることを確認するアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-187">Launch the app to verify Windows Authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="72d9c-188">HTTP.sys を使用して Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="72d9c-188">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="72d9c-189">使用することができますが、Kestrel が Windows 認証をサポートしていない[HTTP.sys](xref:fundamentals/servers/httpsys) Windows で自己ホスト型のシナリオをサポートします。</span><span class="sxs-lookup"><span data-stu-id="72d9c-189">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="72d9c-190">次の例は、Windows 認証を使用した HTTP.sys を使用するアプリの web ホストを構成します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-190">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Program.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="72d9c-191">HTTP.sys では、Kerberos 認証プロトコルを使用したカーネル モード認証に処理が委任されます。</span><span class="sxs-lookup"><span data-stu-id="72d9c-191">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="72d9c-192">Kerberos および HTTP.sys ではユーザー モード認証がサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="72d9c-192">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="72d9c-193">Active Directory から取得され、クライアントによって、ユーザーを認証するサーバーに転送される Kerberos トークン/チケットを暗号化解除するには、コンピューター アカウントを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="72d9c-193">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="72d9c-194">アプリのユーザーではなく、ホストのサービス プリンシパル名 (SPN) を登録します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-194">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="72d9c-195">HTTP.sys は、Nano Server バージョン 1709 以降でサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="72d9c-195">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="72d9c-196">Nano Server の Windows 認証と HTTP.sys を使用する、 [(microsoft/windowsservercore) の Server Core コンテナー](https://hub.docker.com/r/microsoft/windowsservercore/)します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-196">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="72d9c-197">Server Core の詳細については、[Windows Server の Server Core インストール オプションとは何ですか?](/windows-server/administration/server-core/what-is-server-core)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="72d9c-197">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="work-with-windows-authentication"></a><span data-ttu-id="72d9c-198">Windows 認証を使用します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-198">Work with Windows Authentication</span></span>

<span data-ttu-id="72d9c-199">匿名アクセスの構成の状態にする方法が決定します、`[Authorize]`と`[AllowAnonymous]`属性は、アプリで使用します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-199">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="72d9c-200">次の 2 つのセクションでは、匿名アクセスの許可されていないと、許可されている構成の状態を処理する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-200">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="72d9c-201">匿名アクセスを禁止します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-201">Disallow anonymous access</span></span>

<span data-ttu-id="72d9c-202">Windows 認証が有効になっており、匿名アクセスが無効になっているときに、`[Authorize]`と`[AllowAnonymous]`属性は影響ありません。</span><span class="sxs-lookup"><span data-stu-id="72d9c-202">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="72d9c-203">IIS サイト (または HTTP.sys) を匿名アクセスを許可しないように構成する場合、要求がアプリに到達しません。</span><span class="sxs-lookup"><span data-stu-id="72d9c-203">If the IIS site (or HTTP.sys) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="72d9c-204">このため、`[AllowAnonymous]`属性には適用されません。</span><span class="sxs-lookup"><span data-stu-id="72d9c-204">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="72d9c-205">匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-205">Allow anonymous access</span></span>

<span data-ttu-id="72d9c-206">Windows 認証と匿名アクセスの両方が有効になっているときに使用して、`[Authorize]`と`[AllowAnonymous]`属性。</span><span class="sxs-lookup"><span data-stu-id="72d9c-206">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="72d9c-207">`[Authorize]`属性では、本当に Windows 認証を必要と、アプリの情報を保護できます。</span><span class="sxs-lookup"><span data-stu-id="72d9c-207">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="72d9c-208">`[AllowAnonymous]`属性のオーバーライド`[Authorize]`属性の匿名アクセスを許可するアプリ内で使用します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-208">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="72d9c-209">参照してください[単純な承認](xref:security/authorization/simple)属性の使用方法の詳細。</span><span class="sxs-lookup"><span data-stu-id="72d9c-209">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="72d9c-210">ASP.NET core 2.x、`[Authorize]`属性に追加の構成が必要です*Startup.cs*匿名要求を Windows 認証チャレンジを。</span><span class="sxs-lookup"><span data-stu-id="72d9c-210">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="72d9c-211">推奨される構成によって若干異なります、web サーバーが使用されています。</span><span class="sxs-lookup"><span data-stu-id="72d9c-211">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="72d9c-212">既定では、ページにアクセスするための承認を持たないユーザーには、空の HTTP 403 応答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="72d9c-212">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="72d9c-213">[StatusCodePages ミドルウェア](xref:fundamentals/error-handling#usestatuscodepages)「アクセスが拒否されました」のより優れたエクスペリエンスをユーザーに提供するように構成できます。</span><span class="sxs-lookup"><span data-stu-id="72d9c-213">The [StatusCodePages middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="72d9c-214">IIS</span><span class="sxs-lookup"><span data-stu-id="72d9c-214">IIS</span></span>

<span data-ttu-id="72d9c-215">IIS を使用する場合に、次を追加、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="72d9c-215">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="72d9c-216">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="72d9c-216">HTTP.sys</span></span>

<span data-ttu-id="72d9c-217">HTTP.sys を使用する場合に、次を追加、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="72d9c-217">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="72d9c-218">偽装</span><span class="sxs-lookup"><span data-stu-id="72d9c-218">Impersonation</span></span>

<span data-ttu-id="72d9c-219">ASP.NET Core では、権限借用を実装しません。</span><span class="sxs-lookup"><span data-stu-id="72d9c-219">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="72d9c-220">アプリは、アプリ プールまたはプロセス id を使用して、すべての要求をアプリの id で実行します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-220">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="72d9c-221">明示的に、ユーザーに代わって操作を実行する必要がある場合を使用して、 [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*)で、[ターミナル インライン ミドルウェア](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)で`Startup.Configure`します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-221">If you need to explicitly perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="72d9c-222">このコンテキストで 1 つのアクションを実行し、コンテキストを閉じます。</span><span class="sxs-lookup"><span data-stu-id="72d9c-222">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated` <span data-ttu-id="72d9c-223">非同期操作をサポートしていないし、複雑なシナリオでは使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="72d9c-223">doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="72d9c-224">全体要求またはミドルウェアのチェーンをラッピングされていないサポートなど、お勧めします。</span><span class="sxs-lookup"><span data-stu-id="72d9c-224">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

### <a name="claims-transformations"></a><span data-ttu-id="72d9c-225">要求の変換</span><span class="sxs-lookup"><span data-stu-id="72d9c-225">Claims transformations</span></span>

<span data-ttu-id="72d9c-226">IIS のインプロセス モードでホストする場合<xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*>されていないユーザーを初期化するために内部的に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="72d9c-226">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="72d9c-227">そのため、認証のたびに要求を変換するための <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 実装は既定で有効になっていません。</span><span class="sxs-lookup"><span data-stu-id="72d9c-227">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="72d9c-228">詳細については、インプロセスをホストする場合は、クレームの変換をアクティブにするためのコード例を参照してください。<xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>します。</span><span class="sxs-lookup"><span data-stu-id="72d9c-228">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>
