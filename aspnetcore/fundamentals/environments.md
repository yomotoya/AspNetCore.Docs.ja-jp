---
title: "ASP.NET Core での複数の環境での作業"
author: rick-anderson
description: "ASP.NET Core が複数の環境間でのアプリの動作を制御するためのサポートを提供する方法について説明します。"
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 60a1543ce11d08490e6df0eb84f980672ecfe672
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="ff5dd-103">複数の環境での作業</span><span class="sxs-lookup"><span data-stu-id="ff5dd-103">Working with multiple environments</span></span>

<span data-ttu-id="ff5dd-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ff5dd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ff5dd-105">ASP.NET Core は、環境変数と実行時にアプリケーションの動作を設定するためのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="ff5dd-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="ff5dd-107">環境</span><span class="sxs-lookup"><span data-stu-id="ff5dd-107">Environments</span></span>

<span data-ttu-id="ff5dd-108">ASP.NET Core が環境変数を読み取る`ASPNETCORE_ENVIRONMENT`でアプリケーションの起動およびストア内の値が[IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)です。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="ff5dd-109">`ASPNETCORE_ENVIRONMENT`任意の値に設定することができますが、 [3 つの値](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0)フレームワークでサポートされて:[開発](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0)、[ステージング](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0)、および[運用](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0)です。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="ff5dd-110">場合`ASPNETCORE_ENVIRONMENT`設定に既定ではない`Production`です。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="ff5dd-111">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-111">The preceding code:</span></span>

* <span data-ttu-id="ff5dd-112">呼び出し[UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_)と[UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_)とき`ASPNETCORE_ENVIRONMENT`に設定されている`Development`です。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-112">Calls [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="ff5dd-113">呼び出し[UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_)ときの値`ASPNETCORE_ENVIRONMENT`設定されている、次のいずれか。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-113">Calls [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="ff5dd-114">[環境タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)の値を使用して`IHostingEnvironment.EnvironmentName`要素内のマークアップを追加または除外します。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="ff5dd-115">注: Windows および macOS で環境変数と値いない大文字と小文字が区別されます。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="ff5dd-116">Linux 環境変数と値は**大文字小文字を区別**既定です。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="ff5dd-117">開発</span><span class="sxs-lookup"><span data-stu-id="ff5dd-117">Development</span></span>

<span data-ttu-id="ff5dd-118">開発環境では、実稼働環境で公開するべきではありません機能を有効にできます。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="ff5dd-119">たとえば、ASP.NET Core テンプレートを有効にする、[開発者例外ページ](xref:fundamentals/error-handling#the-developer-exception-page)開発環境でします。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="ff5dd-120">ローカル コンピューターの開発環境を設定することができます、 *Properties\launchSettings.json*プロジェクトのファイルです。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="ff5dd-121">環境の値で設定*launchSettings.json*システムの環境で設定値をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="ff5dd-122">次の XML は次の 3 つのプロファイルから、 *launchSettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-122">The following XML shows three profiles from a *launchSettings.json* file:</span></span>

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

<span data-ttu-id="ff5dd-123">アプリケーションを起動するときに`dotnet run`、最初のプロファイルが`"commandName": "Project"`使用されます。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-123">When the application is launched with `dotnet run`, the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="ff5dd-124">値`commandName`を起動する web サーバーを指定します。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-124">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="ff5dd-125">`commandName`いずれかを指定できます。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-125">`commandName` can be one of :</span></span>

* <span data-ttu-id="ff5dd-126">IIS Express</span><span class="sxs-lookup"><span data-stu-id="ff5dd-126">IIS Express</span></span>
* <span data-ttu-id="ff5dd-127">IIS</span><span class="sxs-lookup"><span data-stu-id="ff5dd-127">IIS</span></span>
* <span data-ttu-id="ff5dd-128">プロジェクトを起動 Kestrel)</span><span class="sxs-lookup"><span data-stu-id="ff5dd-128">Project (which launches Kestrel)</span></span>

<span data-ttu-id="ff5dd-129">アプリを起動するときに`dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="ff5dd-129">When an app is launched with `dotnet run`:</span></span>

* <span data-ttu-id="ff5dd-130">*launchSettings.json*は読み取り可能な場合です。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="ff5dd-131">`environmentVariables`設定*launchSettings.json*環境変数をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="ff5dd-132">ホスティング環境が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-132">The hosting environment is displayed.</span></span>


<span data-ttu-id="ff5dd-133">次の出力は、アプリの使用を開始を示します`dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="ff5dd-133">The following output shows an app started with `dotnet run`:</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="ff5dd-134">Visual Studio**デバッグ**タブを編集する GUI を使用する、 *launchSettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-134">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![プロジェクト プロパティ設定の環境変数](environments/_static/project-properties-debug.png)

<span data-ttu-id="ff5dd-136">プロジェクトのプロファイルに加えられた変更は可能性があります、web サーバーが再起動されるまで有効になりません。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="ff5dd-137">自体は、その環境に加えられた変更を検出するには、kestrel を再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-137">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="ff5dd-138">*launchSettings.json*機密情報を格納しないでください。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="ff5dd-139">[シークレット マネージャー ツール](xref:security/app-secrets)ローカル開発用のシークレットを格納するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="ff5dd-140">実稼働</span><span class="sxs-lookup"><span data-stu-id="ff5dd-140">Production</span></span>

<span data-ttu-id="ff5dd-141">実稼働環境は、セキュリティ、パフォーマンス、およびアプリケーションの堅牢性を最大化するように構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-141">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="ff5dd-142">一般的な運用環境が存在する可能性のある設定の開発とは異なりますは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-142">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="ff5dd-143">キャッシュします。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-143">Caching.</span></span>
* <span data-ttu-id="ff5dd-144">クライアント側のリソースのバンドル、縮小、および CDN から供給される可能性がありますしていること。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-144">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="ff5dd-145">診断エラー ページが無効になっています。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-145">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="ff5dd-146">わかりやすいエラー ページが有効にします。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-146">Friendly error pages enabled.</span></span>
* <span data-ttu-id="ff5dd-147">実稼働のログ記録と監視を有効にします。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-147">Production logging and monitoring enabled.</span></span> <span data-ttu-id="ff5dd-148">たとえば、 [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/)です。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-148">For example, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="ff5dd-149">環境の設定</span><span class="sxs-lookup"><span data-stu-id="ff5dd-149">Setting the environment</span></span>

<span data-ttu-id="ff5dd-150">テストするための特定の環境を設定すると便利です。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-150">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="ff5dd-151">環境が設定されていない場合を既定`Production`ほとんどのデバッグ機能を無効にします。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-151">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="ff5dd-152">環境を設定するためのメソッドは、オペレーティング システムによって異なります。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-152">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="ff5dd-153">Azure</span><span class="sxs-lookup"><span data-stu-id="ff5dd-153">Azure</span></span>

<span data-ttu-id="ff5dd-154">Azure アプリケーション サービスの場合</span><span class="sxs-lookup"><span data-stu-id="ff5dd-154">For Azure app service:</span></span>

* <span data-ttu-id="ff5dd-155">選択、**アプリケーション設定**ブレードです。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-155">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="ff5dd-156">キーを追加して、値で**アプリ設定**です。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-156">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="ff5dd-157">Windows</span><span class="sxs-lookup"><span data-stu-id="ff5dd-157">Windows</span></span>
<span data-ttu-id="ff5dd-158">設定する、`ASPNETCORE_ENVIRONMENT`を使用して、アプリが開始された場合、現在のセッションの`dotnet run`、次のコマンドを使用</span><span class="sxs-lookup"><span data-stu-id="ff5dd-158">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="ff5dd-159">**コマンド ライン**</span><span class="sxs-lookup"><span data-stu-id="ff5dd-159">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="ff5dd-160">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="ff5dd-160">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="ff5dd-161">これらのコマンドは、現在のウィンドウに対してのみ有効になります。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-161">These commands take effect only for the current window.</span></span> <span data-ttu-id="ff5dd-162">ウィンドウが閉じられたときに、ASPNETCORE_ENVIRONMENT 設定は、既定の設定またはコンピューターの値に戻ります。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-162">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="ff5dd-163">Windows を開いたとき、値をグローバルに設定するために、**コントロール パネルの**  > **システム** > **システムの詳細設定**を追加または編集、 `ASPNETCORE_ENVIRONMENT`値。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-163">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![システムの詳細プロパティ](environments/_static/systemsetting_environment.png)

![ASPNET コア環境変数](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="ff5dd-166">**web.config**</span><span class="sxs-lookup"><span data-stu-id="ff5dd-166">**web.config**</span></span>

<span data-ttu-id="ff5dd-167">参照してください、*環境変数の設定*のセクションで、 [ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)トピックです。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-167">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="ff5dd-168">**IIS アプリケーション プール単位**</span><span class="sxs-lookup"><span data-stu-id="ff5dd-168">**Per IIS Application Pool**</span></span>

<span data-ttu-id="ff5dd-169">(IIS 10.0 以降でサポート) 分離のアプリケーション プールで実行されている個々 のアプリの環境変数を設定するを参照してください、 *AppCmd.exe コマンド*のセクションで、[環境変数\<。environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe)トピックです。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-169">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="ff5dd-170">macOS</span><span class="sxs-lookup"><span data-stu-id="ff5dd-170">macOS</span></span>
<span data-ttu-id="ff5dd-171">MacOS の現在の環境を設定する場合に実行できます行で、アプリケーションを実行しています。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-171">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="ff5dd-172">使用してまたは`export`をアプリを実行する前に設定します。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-172">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="ff5dd-173">コンピューターのレベルの環境変数が設定されて、*なる*または*.bash_profile*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-173">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="ff5dd-174">任意のテキスト エディターを使用して、ファイルを編集し、次のステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-174">Edit the file using any text editor and add the following statment.</span></span>

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="ff5dd-175">Linux</span><span class="sxs-lookup"><span data-stu-id="ff5dd-175">Linux</span></span>
<span data-ttu-id="ff5dd-176">Linux ディストリビューションの場合を使用して、`export`セッション ベースの変数の設定のコマンドラインでコマンドと*bash_profile*マシン レベルの環境設定のファイルです。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-176">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="ff5dd-177">環境別の構成</span><span class="sxs-lookup"><span data-stu-id="ff5dd-177">Configuration by environment</span></span>

<span data-ttu-id="ff5dd-178">参照してください[環境によって構成](xref:fundamentals/configuration/index#configuration-by-environment)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-178">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="ff5dd-179">環境がスタートアップ クラスおよびメソッドに基づく</span><span class="sxs-lookup"><span data-stu-id="ff5dd-179">Environment based Startup class and methods</span></span>

<span data-ttu-id="ff5dd-180">ASP.NET Core アプリケーションの起動時、[スタートアップ クラス](xref:fundamentals/startup)アプリをブートス トラップします。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-180">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="ff5dd-181">クラス`Startup{EnvironmentName}`存在する場合、そのクラスが呼び出されること`EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="ff5dd-181">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="ff5dd-182">注: 呼び出す[WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)構成セクションをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="ff5dd-182">Note: Calling [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="ff5dd-183">[構成](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_)と[ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0)フォームの環境の特定バージョンをサポートして`Configure{EnvironmentName}`と`Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="ff5dd-183">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="ff5dd-184">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="ff5dd-184">Additional Resources</span></span>

* [<span data-ttu-id="ff5dd-185">アプリケーションの起動</span><span class="sxs-lookup"><span data-stu-id="ff5dd-185">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="ff5dd-186">構成</span><span class="sxs-lookup"><span data-stu-id="ff5dd-186">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="ff5dd-187">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="ff5dd-187">IHostingEnvironment.EnvironmentName</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
