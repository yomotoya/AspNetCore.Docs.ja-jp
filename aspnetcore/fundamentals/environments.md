---
title: ASP.NET Core で複数の環境を使用する
author: rick-anderson
description: ASP.NET Core では、複数の環境にわたり、アプリの動作を制御できます。ここではそのしくみについて説明します。
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: 2c8441db527203aeea516073dae3bc335c335565
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="3818d-103">ASP.NET Core で複数の環境を使用する</span><span class="sxs-lookup"><span data-stu-id="3818d-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="3818d-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3818d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3818d-105">ASP.NET Core では、環境変数を利用し、アプリケーションの実行時動作を設定できます。</span><span class="sxs-lookup"><span data-stu-id="3818d-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="3818d-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="3818d-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="3818d-107">環境</span><span class="sxs-lookup"><span data-stu-id="3818d-107">Environments</span></span>

<span data-ttu-id="3818d-108">ASP.NET Core はアプリケーションの起動時に環境変数 `ASPNETCORE_ENVIRONMENT` を読み込み、その値を [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName) に格納します。</span><span class="sxs-lookup"><span data-stu-id="3818d-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="3818d-109">`ASPNETCORE_ENVIRONMENT` は任意の値に設定できますが、フレームワークは [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0)、[Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0)、[Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0) という [3 つの値](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0)を指定できます。</span><span class="sxs-lookup"><span data-stu-id="3818d-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="3818d-110">`ASPNETCORE_ENVIRONMENT` が設定されていない場合、既定で `Production` が使用されます。</span><span class="sxs-lookup"><span data-stu-id="3818d-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="3818d-111">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="3818d-111">The preceding code:</span></span>

* <span data-ttu-id="3818d-112">`ASPNETCORE_ENVIRONMENT` が `Development` に設定されているとき、[UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) と [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="3818d-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="3818d-113">`ASPNETCORE_ENVIRONMENT` の値が次のいずれかに設定されているとき、[UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="3818d-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="3818d-114">[環境タグ ヘルパー ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) は `IHostingEnvironment.EnvironmentName` の値を使用し、要素にマークアップを追加するか、要素からマークアップを除外します。</span><span class="sxs-lookup"><span data-stu-id="3818d-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="3818d-115">注: Windows と macOS では、環境変数と値で大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="3818d-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="3818d-116">Linux では、環境変数と値について、既定で**大文字と小文字が区別されます**。</span><span class="sxs-lookup"><span data-stu-id="3818d-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="3818d-117">開発</span><span class="sxs-lookup"><span data-stu-id="3818d-117">Development</span></span>

<span data-ttu-id="3818d-118">開発環境では、実稼働で公開すべきではない機能を有効にすることがあります。</span><span class="sxs-lookup"><span data-stu-id="3818d-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="3818d-119">たとえば、ASP.NET Core テンプレートは、開発環境で[開発者例外ページ](xref:fundamentals/error-handling#the-developer-exception-page)を有効にします。</span><span class="sxs-lookup"><span data-stu-id="3818d-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="3818d-120">ローカル コンピューターの開発環境をプロジェクトの *Properties\launchSettings.json* ファイルで設定できます。</span><span class="sxs-lookup"><span data-stu-id="3818d-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="3818d-121">*launchSettings.json* に設定されている環境値で、システム環境に設定されている値がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="3818d-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="3818d-122">次の JSON では、*launchSettings.json* ファイルの 3 つのプロファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3818d-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"
> [!NOTE]
> <span data-ttu-id="3818d-123">*launchSettings.json* の `applicationUrl` プロパティでは、サーバー URL の一覧を指定できます。</span><span class="sxs-lookup"><span data-stu-id="3818d-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="3818d-124">一覧の URL 間には、セミコロンを次のように使用します。</span><span class="sxs-lookup"><span data-stu-id="3818d-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "WebApplication1": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```
::: moniker-end

<span data-ttu-id="3818d-125">アプリケーションが [dotnet run](/dotnet/core/tools/dotnet-run) で起動すると、`"commandName": "Project"` を含む最初のプロファイルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="3818d-125">When the application is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="3818d-126">`commandName` の値により、起動する Web サーバーが指定されます。</span><span class="sxs-lookup"><span data-stu-id="3818d-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="3818d-127">`commandName` は次のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="3818d-127">`commandName` can be one of :</span></span>

* <span data-ttu-id="3818d-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="3818d-128">IIS Express</span></span>
* <span data-ttu-id="3818d-129">IIS</span><span class="sxs-lookup"><span data-stu-id="3818d-129">IIS</span></span>
* <span data-ttu-id="3818d-130">プロジェクト (Kestrel を起動する)</span><span class="sxs-lookup"><span data-stu-id="3818d-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="3818d-131">アプリが [dotnet run](/dotnet/core/tools/dotnet-run) で起動されるとき:</span><span class="sxs-lookup"><span data-stu-id="3818d-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="3818d-132">利用できる場合、*launchSettings.json* が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="3818d-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="3818d-133">*launchSettings.json* の `environmentVariables` 設定により環境変数がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="3818d-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="3818d-134">ホスティング環境が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3818d-134">The hosting environment is displayed.</span></span>


<span data-ttu-id="3818d-135">次の出力には、[dotnet run](/dotnet/core/tools/dotnet-run) で起動したアプリが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3818d-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="3818d-136">Visual Studio の **[デバッグ]** タブには、*launchSettings.json* ファイルを編集するための GUI があります。</span><span class="sxs-lookup"><span data-stu-id="3818d-136">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![プロジェクト プロパティ設定の環境変数](environments/_static/project-properties-debug.png)

<span data-ttu-id="3818d-138">プロジェクト プロファイルを変更した場合、Web サーバーを再起動するまで変更は適用されません。</span><span class="sxs-lookup"><span data-stu-id="3818d-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="3818d-139">Kestrel がその環境に行われた変更を検出するには、先に再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3818d-139">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="3818d-140">*launchSettings.json* にはシークレットを格納しないでください。</span><span class="sxs-lookup"><span data-stu-id="3818d-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="3818d-141">ローカル開発のシークレットを格納するには、[シークレット マネージャー ツール](xref:security/app-secrets)を利用できます。</span><span class="sxs-lookup"><span data-stu-id="3818d-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="3818d-142">実稼働</span><span class="sxs-lookup"><span data-stu-id="3818d-142">Production</span></span>

<span data-ttu-id="3818d-143">実稼働環境は、セキュリティ、パフォーマンス、アプリケーションの堅牢性が最大化されるように構成してください。</span><span class="sxs-lookup"><span data-stu-id="3818d-143">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="3818d-144">開発とは異なる一般的な設定は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="3818d-144">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="3818d-145">キャッシュ。</span><span class="sxs-lookup"><span data-stu-id="3818d-145">Caching.</span></span>
* <span data-ttu-id="3818d-146">クライアント側のリソースはバンドルされ、小さくされ、場合によっては CDN からサービスが提供されます。</span><span class="sxs-lookup"><span data-stu-id="3818d-146">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="3818d-147">診断エラー ページが無効。</span><span class="sxs-lookup"><span data-stu-id="3818d-147">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="3818d-148">フレンドリ エラー ページが有効。</span><span class="sxs-lookup"><span data-stu-id="3818d-148">Friendly error pages enabled.</span></span>
* <span data-ttu-id="3818d-149">実稼働のログ記録と監視が有効。</span><span class="sxs-lookup"><span data-stu-id="3818d-149">Production logging and monitoring enabled.</span></span> <span data-ttu-id="3818d-150">[Application Insights](/azure/application-insights/app-insights-asp-net-core) など。</span><span class="sxs-lookup"><span data-stu-id="3818d-150">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="3818d-151">環境を更新する</span><span class="sxs-lookup"><span data-stu-id="3818d-151">Setting the environment</span></span>

<span data-ttu-id="3818d-152">テスト用の環境を構築すると便利な場合があります。</span><span class="sxs-lookup"><span data-stu-id="3818d-152">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="3818d-153">環境を設定しない場合、既定で `Production` に設定されます。実稼働ではほとんどのデバッグ機能が無効になっています。</span><span class="sxs-lookup"><span data-stu-id="3818d-153">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="3818d-154">環境を設定する手法は、オペレーティング システムによって異なります。</span><span class="sxs-lookup"><span data-stu-id="3818d-154">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="3818d-155">Azure</span><span class="sxs-lookup"><span data-stu-id="3818d-155">Azure</span></span>

<span data-ttu-id="3818d-156">Azure アプリ サービスの場合:</span><span class="sxs-lookup"><span data-stu-id="3818d-156">For Azure app service:</span></span>

* <span data-ttu-id="3818d-157">**[アプリケーションの設定]** ブレードを選択します。</span><span class="sxs-lookup"><span data-stu-id="3818d-157">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="3818d-158">**[アプリの設定]** でキーと値を追加します。</span><span class="sxs-lookup"><span data-stu-id="3818d-158">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="3818d-159">Windows</span><span class="sxs-lookup"><span data-stu-id="3818d-159">Windows</span></span>
<span data-ttu-id="3818d-160">現在のセッションに `ASPNETCORE_ENVIRONMENT` を設定するには、アプリが [dotnet run](/dotnet/core/tools/dotnet-run) で起動する場合、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="3818d-160">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used</span></span>

<span data-ttu-id="3818d-161">**コマンド ライン**</span><span class="sxs-lookup"><span data-stu-id="3818d-161">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="3818d-162">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="3818d-162">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="3818d-163">これらのコマンドは、現在のウィンドウでのみ有効になります。</span><span class="sxs-lookup"><span data-stu-id="3818d-163">These commands take effect only for the current window.</span></span> <span data-ttu-id="3818d-164">ウィンドウが閉じられると、ASPNETCORE_ENVIRONMENT 設定が初期設定またはコンピューター値に戻ります。</span><span class="sxs-lookup"><span data-stu-id="3818d-164">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="3818d-165">Windows でグローバルな値を設定するには、**[コントロール パネル]**、**[システム]**、**[システムの詳細設定]** の順に開き、`ASPNETCORE_ENVIRONMENT` 値を追加または編集します。</span><span class="sxs-lookup"><span data-stu-id="3818d-165">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![システムの詳細プロパティ](environments/_static/systemsetting_environment.png)

![ASPNET Core 環境変数](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="3818d-168">**web.config**</span><span class="sxs-lookup"><span data-stu-id="3818d-168">**web.config**</span></span>

<span data-ttu-id="3818d-169">「[ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)」(ASP.NET Core Module 構成参照) トピックの「*Setting environment variables*」(環境変数の設定) セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3818d-169">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="3818d-170">**IIS アプリケーション プール**</span><span class="sxs-lookup"><span data-stu-id="3818d-170">**Per IIS Application Pool**</span></span>

<span data-ttu-id="3818d-171">分離されたアプリケーション プール (IIS 10.0 以降でサポートされています) で実行する個別アプリに対して環境変数を設定するには、[環境変数 \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) のトピックにある *AppCmd.exe コマンド*のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3818d-171">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="3818d-172">macOS</span><span class="sxs-lookup"><span data-stu-id="3818d-172">macOS</span></span>
<span data-ttu-id="3818d-173">MacOS の現在の環境は、アプリケーション実行時に設定できます。</span><span class="sxs-lookup"><span data-stu-id="3818d-173">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="3818d-174">あるいは、`export` を利用し、アプリの実行前に設定できます。</span><span class="sxs-lookup"><span data-stu-id="3818d-174">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="3818d-175">コンピューター レベルの環境変数は *.bashrc* または *.bash_profile* ファイルに設定されます。</span><span class="sxs-lookup"><span data-stu-id="3818d-175">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="3818d-176">任意のテキスト エディターでファイルを編集し、次のステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="3818d-176">Edit the file using any text editor and add the following statment.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="3818d-177">Linux</span><span class="sxs-lookup"><span data-stu-id="3818d-177">Linux</span></span>
<span data-ttu-id="3818d-178">Linux ディストリビューションの場合、セッション ベースの変数設定にはコマンド ラインで `export` コマンドを使用し、コンピューター レベルの環境設定には *bash_profile* ファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="3818d-178">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="3818d-179">環境別の構成</span><span class="sxs-lookup"><span data-stu-id="3818d-179">Configuration by environment</span></span>

<span data-ttu-id="3818d-180">詳細については、[環境別の構成](xref:fundamentals/configuration/index#configuration-by-environment)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3818d-180">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="3818d-181">環境別の起動のクラスとメソッド</span><span class="sxs-lookup"><span data-stu-id="3818d-181">Environment based Startup class and methods</span></span>

<span data-ttu-id="3818d-182">ASP.NET Core アプリの起動時、[Startup クラス](xref:fundamentals/startup)がアプリをブートストラップします。</span><span class="sxs-lookup"><span data-stu-id="3818d-182">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="3818d-183">クラス `Startup{EnvironmentName}` が存在する場合、その `EnvironmentName` に対してそのクラスが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="3818d-183">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="3818d-184">注: [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) を呼び出すと、構成セクションがオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="3818d-184">Note: Calling [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="3818d-185">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) と [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) は、フォーム `Configure{EnvironmentName}` と `Configure{EnvironmentName}Services` の環境固有バージョンに対応しています。</span><span class="sxs-lookup"><span data-stu-id="3818d-185">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="3818d-186">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="3818d-186">Additional resources</span></span>

* [<span data-ttu-id="3818d-187">アプリケーションの起動</span><span class="sxs-lookup"><span data-stu-id="3818d-187">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="3818d-188">構成</span><span class="sxs-lookup"><span data-stu-id="3818d-188">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="3818d-189">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="3818d-189">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
