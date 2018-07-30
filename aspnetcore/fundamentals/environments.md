---
title: ASP.NET Core で複数の環境を使用する
author: rick-anderson
description: ASP.NET Core アプリで複数の環境にわたりアプリの動作を制御する方法について説明します。
ms.author: riande
ms.date: 07/03/2018
uid: fundamentals/environments
ms.openlocfilehash: eaa6fa44ed90d0c85a11f5e67a4bb9a91e84c196
ms.sourcegitcommit: c8e62aa766641aa55105f7db79cdf2b27a6e5977
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/25/2018
ms.locfileid: "39254871"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="8482a-103">ASP.NET Core で複数の環境を使用する</span><span class="sxs-lookup"><span data-stu-id="8482a-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="8482a-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8482a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8482a-105">ASP.NET Core は、環境変数を利用し、ランタイム環境に基づいてアプリの動作を構成します。</span><span class="sxs-lookup"><span data-stu-id="8482a-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="8482a-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="8482a-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="8482a-107">環境</span><span class="sxs-lookup"><span data-stu-id="8482a-107">Environments</span></span>

<span data-ttu-id="8482a-108">ASP.NET Core はアプリの起動時に環境変数 `ASPNETCORE_ENVIRONMENT` を読み込み、その値を [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) に格納します。</span><span class="sxs-lookup"><span data-stu-id="8482a-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span></span> <span data-ttu-id="8482a-109">`ASPNETCORE_ENVIRONMENT` は任意の値に設定できますが、このフレームワークでは [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development)、[Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging)、[Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production) という [3 つの値](/dotnet/api/microsoft.aspnetcore.hosting.environmentname)を指定できます。</span><span class="sxs-lookup"><span data-stu-id="8482a-109">You can set `ASPNETCORE_ENVIRONMENT` to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span></span> <span data-ttu-id="8482a-110">`ASPNETCORE_ENVIRONMENT` が設定されていない場合、既定で `Production` になります。</span><span class="sxs-lookup"><span data-stu-id="8482a-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it defaults to `Production`.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="8482a-111">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="8482a-111">The preceding code:</span></span>

* <span data-ttu-id="8482a-112">`ASPNETCORE_ENVIRONMENT` が `Development` に設定されているとき、[UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) と [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="8482a-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="8482a-113">`ASPNETCORE_ENVIRONMENT` の値が次のいずれかに設定されているとき、[UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="8482a-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="8482a-114">[環境タグ ヘルパー ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) は `IHostingEnvironment.EnvironmentName` の値を使用し、要素にマークアップを追加するか、要素からマークアップを除外します。</span><span class="sxs-lookup"><span data-stu-id="8482a-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="8482a-115">Windows と macOS では、環境変数と値で大文字と小文字が区別されません。</span><span class="sxs-lookup"><span data-stu-id="8482a-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="8482a-116">Linux では、環境変数と値について、既定で**大文字と小文字が区別されます**。</span><span class="sxs-lookup"><span data-stu-id="8482a-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="8482a-117">開発</span><span class="sxs-lookup"><span data-stu-id="8482a-117">Development</span></span>

<span data-ttu-id="8482a-118">開発環境では、実稼働で公開すべきではない機能を有効にすることがあります。</span><span class="sxs-lookup"><span data-stu-id="8482a-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="8482a-119">たとえば、ASP.NET Core テンプレートは、開発環境で[開発者例外ページ](xref:fundamentals/error-handling#the-developer-exception-page)を有効にします。</span><span class="sxs-lookup"><span data-stu-id="8482a-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="8482a-120">ローカル コンピューターの開発環境をプロジェクトの *Properties\launchSettings.json* ファイルで設定できます。</span><span class="sxs-lookup"><span data-stu-id="8482a-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="8482a-121">*launchSettings.json* に設定されている環境値で、システム環境に設定されている値がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="8482a-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="8482a-122">次の JSON では、*launchSettings.json* ファイルの 3 つのプロファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8482a-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:54339/",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      }
    },
    "EnvironmentsSample": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:54340/"
    },
    "Kestrel Staging": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:51997/"
    }
  }
}
```

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="8482a-123">*launchSettings.json* の `applicationUrl` プロパティでは、サーバー URL の一覧を指定できます。</span><span class="sxs-lookup"><span data-stu-id="8482a-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="8482a-124">一覧の URL 間には、セミコロンを次のように使用します。</span><span class="sxs-lookup"><span data-stu-id="8482a-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "EnvironmentsSample": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```

::: moniker-end

<span data-ttu-id="8482a-125">アプリが [dotnet run](/dotnet/core/tools/dotnet-run) で起動すると、`"commandName": "Project"` を含む最初のプロファイルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="8482a-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="8482a-126">`commandName` の値により、起動する Web サーバーが指定されます。</span><span class="sxs-lookup"><span data-stu-id="8482a-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="8482a-127">`commandName` は次のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="8482a-127">`commandName` can be any one of the following:</span></span>

* <span data-ttu-id="8482a-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="8482a-128">IIS Express</span></span>
* <span data-ttu-id="8482a-129">IIS</span><span class="sxs-lookup"><span data-stu-id="8482a-129">IIS</span></span>
* <span data-ttu-id="8482a-130">プロジェクト (Kestrel を起動する)</span><span class="sxs-lookup"><span data-stu-id="8482a-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="8482a-131">アプリが [dotnet run](/dotnet/core/tools/dotnet-run) で起動されるとき:</span><span class="sxs-lookup"><span data-stu-id="8482a-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="8482a-132">利用できる場合、*launchSettings.json* が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="8482a-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="8482a-133">*launchSettings.json* の `environmentVariables` 設定により環境変数がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="8482a-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="8482a-134">ホスティング環境が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8482a-134">The hosting environment is displayed.</span></span>

<span data-ttu-id="8482a-135">次の出力には、[dotnet run](/dotnet/core/tools/dotnet-run) で起動したアプリが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8482a-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="8482a-136">Visual Studio のプロジェクト プロパティの **[デバッグ]** タブには、*launchSettings.json* ファイルを編集するための GUI があります。</span><span class="sxs-lookup"><span data-stu-id="8482a-136">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![プロジェクト プロパティ設定の環境変数](environments/_static/project-properties-debug.png)

<span data-ttu-id="8482a-138">プロジェクト プロファイルを変更した場合、Web サーバーを再起動するまで変更は適用されません。</span><span class="sxs-lookup"><span data-stu-id="8482a-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="8482a-139">Kestrel がその環境に行われた変更を検出するには、先に再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8482a-139">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="8482a-140">*launchSettings.json* にはシークレットを格納しないでください。</span><span class="sxs-lookup"><span data-stu-id="8482a-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="8482a-141">ローカル開発のシークレットを格納するには、[シークレット マネージャー ツール](xref:security/app-secrets)を利用できます。</span><span class="sxs-lookup"><span data-stu-id="8482a-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="8482a-142">[Visual Studio Code](https://code.visualstudio.com/) を使用するとき、環境変数を *.vscode/launch.json* ファイルで設定できます。</span><span class="sxs-lookup"><span data-stu-id="8482a-142">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="8482a-143">次の例では、環境が `Development` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="8482a-143">The following example sets the environment to `Development`:</span></span>

```json
{
   "version": "0.2.0",
   "configurations": [
        {
            "name": ".NET Core Launch (web)",

            ... additional VS Code configuration settings ...

            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    ]
}
```

<span data-ttu-id="8482a-144">*Properties/launchSettings.json* と同じように `dotnet run` でアプリを起動すると、プロジェクトの *.vscode/launch.json* ファイルは読み込まれません。</span><span class="sxs-lookup"><span data-stu-id="8482a-144">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="8482a-145">*launchSettings.json* ファイルがない開発中アプリを起動するとき、環境変数で環境を設定するか、コマンドライン引数を `dotnet run` コマンドに設定します。</span><span class="sxs-lookup"><span data-stu-id="8482a-145">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="8482a-146">実稼働</span><span class="sxs-lookup"><span data-stu-id="8482a-146">Production</span></span>

<span data-ttu-id="8482a-147">実稼働環境は、セキュリティ、パフォーマンス、アプリの堅牢性が最大化されるように構成してください。</span><span class="sxs-lookup"><span data-stu-id="8482a-147">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="8482a-148">開発とは異なる一般的な設定は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="8482a-148">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="8482a-149">キャッシュ。</span><span class="sxs-lookup"><span data-stu-id="8482a-149">Caching.</span></span>
* <span data-ttu-id="8482a-150">クライアント側のリソースはバンドルされ、小さくされ、場合によっては CDN からサービスが提供されます。</span><span class="sxs-lookup"><span data-stu-id="8482a-150">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="8482a-151">診断エラー ページが無効。</span><span class="sxs-lookup"><span data-stu-id="8482a-151">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="8482a-152">フレンドリ エラー ページが有効。</span><span class="sxs-lookup"><span data-stu-id="8482a-152">Friendly error pages enabled.</span></span>
* <span data-ttu-id="8482a-153">実稼働のログ記録と監視が有効。</span><span class="sxs-lookup"><span data-stu-id="8482a-153">Production logging and monitoring enabled.</span></span> <span data-ttu-id="8482a-154">[Application Insights](/azure/application-insights/app-insights-asp-net-core) など。</span><span class="sxs-lookup"><span data-stu-id="8482a-154">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="8482a-155">環境を設定する</span><span class="sxs-lookup"><span data-stu-id="8482a-155">Set the environment</span></span>

<span data-ttu-id="8482a-156">テスト用の環境を構築すると便利な場合があります。</span><span class="sxs-lookup"><span data-stu-id="8482a-156">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="8482a-157">環境を設定しない場合、既定で `Production` に設定されます。ほとんどのデバッグ機能が無効になります。</span><span class="sxs-lookup"><span data-stu-id="8482a-157">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="8482a-158">環境を設定する手法は、オペレーティング システムによって異なります。</span><span class="sxs-lookup"><span data-stu-id="8482a-158">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="8482a-159">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="8482a-159">Azure App Service</span></span>

<span data-ttu-id="8482a-160">[Azure App Service](https://azure.microsoft.com/services/app-service/) で環境を設定するには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="8482a-160">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="8482a-161">**[App Services]** ブレードからアプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="8482a-161">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="8482a-162">**[設定]** グループで **[アプリケーションの設定]** ブレードを選択します。</span><span class="sxs-lookup"><span data-stu-id="8482a-162">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="8482a-163">**[アプリケーションの設定]** 領域で、**[新しい設定の追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8482a-163">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="8482a-164">**[名前の入力]** には、`ASPNETCORE_ENVIRONMENT` を指定します。</span><span class="sxs-lookup"><span data-stu-id="8482a-164">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="8482a-165">**[値の入力]** には、環境を指定します (`Staging` など)。</span><span class="sxs-lookup"><span data-stu-id="8482a-165">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="8482a-166">デプロイ スロットがスワップされるとき、環境設定に現在のスロットを与える場合、**[スロットの設定]** チェック ボックスを選択します。</span><span class="sxs-lookup"><span data-stu-id="8482a-166">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="8482a-167">詳細については、[設定のスワップに関する Azure ドキュメント](/azure/app-service/web-sites-staged-publishing)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8482a-167">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="8482a-168">ブレードの一番上にある **[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8482a-168">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="8482a-169">Azure App Service は、アプリ設定 (環境変数) が Azure Portal で追加、変更、削除された後、アプリを自動的に再起動します。</span><span class="sxs-lookup"><span data-stu-id="8482a-169">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

### <a name="windows"></a><span data-ttu-id="8482a-170">Windows</span><span class="sxs-lookup"><span data-stu-id="8482a-170">Windows</span></span>

<span data-ttu-id="8482a-171">[dotnet run](/dotnet/core/tools/dotnet-run) を使用してアプリを起動するときに現在のセッションに `ASPNETCORE_ENVIRONMENT` を設定するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="8482a-171">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="8482a-172">**コマンド プロンプト**</span><span class="sxs-lookup"><span data-stu-id="8482a-172">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="8482a-173">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="8482a-173">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="8482a-174">これらのコマンドは、現在のウィンドウでのみ有効になります。</span><span class="sxs-lookup"><span data-stu-id="8482a-174">These commands only take effect for the current window.</span></span> <span data-ttu-id="8482a-175">ウィンドウが閉じられると、`ASPNETCORE_ENVIRONMENT` 設定が初期設定またはコンピューター値に戻ります。</span><span class="sxs-lookup"><span data-stu-id="8482a-175">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="8482a-176">Windows でグローバルな値を設定するには、次の方法のいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="8482a-176">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="8482a-177">**[コントロール パネル]** > **[システム]** > **[システムの詳細設定]** の順に開き、`ASPNETCORE_ENVIRONMENT` 値を追加または編集します。</span><span class="sxs-lookup"><span data-stu-id="8482a-177">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![システムの詳細プロパティ](environments/_static/systemsetting_environment.png)

  ![ASPNET Core 環境変数](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="8482a-180">管理コマンド プロンプトを開き、`setx` コマンドを使用するか、または管理用の PowerShell コマンド プロンプトを開いて、`[Environment]::SetEnvironmentVariable` を使用します。</span><span class="sxs-lookup"><span data-stu-id="8482a-180">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="8482a-181">**コマンド プロンプト**</span><span class="sxs-lookup"><span data-stu-id="8482a-181">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="8482a-182">`/M` スイッチは、システム レベルで環境変数を設定することを示します。</span><span class="sxs-lookup"><span data-stu-id="8482a-182">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="8482a-183">`/M` スイッチが使用されない場合、ユーザー アカウントに対する環境変数が設定されます。</span><span class="sxs-lookup"><span data-stu-id="8482a-183">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="8482a-184">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="8482a-184">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="8482a-185">`Machine` オプションの値は、システム レベルで環境変数を設定することを示します。</span><span class="sxs-lookup"><span data-stu-id="8482a-185">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="8482a-186">オプションの値が `User` に変更されると、ユーザー アカウントに対する環境変数が設定されます。</span><span class="sxs-lookup"><span data-stu-id="8482a-186">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="8482a-187">グローバルに設定されている `ASPNETCORE_ENVIRONMENT` の環境変数は、値の設定後に開いているすべてのコマンド ウィンドウで `dotnet run` に対して有効になります。</span><span class="sxs-lookup"><span data-stu-id="8482a-187">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="8482a-188">**web.config**</span><span class="sxs-lookup"><span data-stu-id="8482a-188">**web.config**</span></span>

<span data-ttu-id="8482a-189">`ASPNETCORE_ENVIRONMENT` 環境変数を *web.config* で設定するには、<xref:host-and-deploy/aspnet-core-module#setting-environment-variables>の「*環境変数の設定*」のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8482a-189">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span> <span data-ttu-id="8482a-190">`ASPNETCORE_ENVIRONMENT` 環境変数が *web.config* で設定されている場合、その値は、システム レベルの設定をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="8482a-190">When the `ASPNETCORE_ENVIRONMENT` environment variable is set with *web.config*, its value overrides a setting at the system level.</span></span>

<span data-ttu-id="8482a-191">**IIS アプリケーション プール**</span><span class="sxs-lookup"><span data-stu-id="8482a-191">**Per IIS Application Pool**</span></span>

<span data-ttu-id="8482a-192">分離されたアプリケーション プール (IIS 10.0 以降でサポートされています) で実行するアプリに対して `ASPNETCORE_ENVIRONMENT` 環境変数を設定するには、[環境変数 &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) のトピックにある *AppCmd.exe コマンド*のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8482a-192">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="8482a-193">`ASPNETCORE_ENVIRONMENT` 環境変数がアプリ プールで設定されている場合、その値は、システム レベルの設定をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="8482a-193">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8482a-194">IIS でアプリをホストして `ASPNETCORE_ENVIRONMENT` 環境変数を追加または変更するときは、次のいずれかの方法を使用してアプリで選択された新しい値を設定してください。</span><span class="sxs-lookup"><span data-stu-id="8482a-194">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="8482a-195">アプリのアプリ プールを再起動します。</span><span class="sxs-lookup"><span data-stu-id="8482a-195">Restart an app's app pool.</span></span>
> * <span data-ttu-id="8482a-196">コマンド プロンプトで `net stop was /y` を実行した後、`net start w3svc` を実行します。</span><span class="sxs-lookup"><span data-stu-id="8482a-196">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="8482a-197">サーバーを再起動します。</span><span class="sxs-lookup"><span data-stu-id="8482a-197">Restart the server.</span></span>

### <a name="macos"></a><span data-ttu-id="8482a-198">macOS</span><span class="sxs-lookup"><span data-stu-id="8482a-198">macOS</span></span>

<span data-ttu-id="8482a-199">macOS の現在の環境は、アプリ実行時にインラインで設定できます。</span><span class="sxs-lookup"><span data-stu-id="8482a-199">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="8482a-200">あるいは、アプリを実行する前に `export` で環境を設定します。</span><span class="sxs-lookup"><span data-stu-id="8482a-200">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="8482a-201">コンピューター レベルの環境変数は *.bashrc* または *.bash_profile* ファイルに設定されます。</span><span class="sxs-lookup"><span data-stu-id="8482a-201">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="8482a-202">任意のテキスト エディターを利用し、ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="8482a-202">Edit the file using any text editor.</span></span> <span data-ttu-id="8482a-203">次のステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="8482a-203">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="8482a-204">Linux</span><span class="sxs-lookup"><span data-stu-id="8482a-204">Linux</span></span>

<span data-ttu-id="8482a-205">Linux ディストリビューションの場合、セッション ベースの変数設定にはコマンド プロンプトで `export` コマンドを使用し、コンピューター レベルの環境設定には *bash_profile* ファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="8482a-205">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="8482a-206">環境別の構成</span><span class="sxs-lookup"><span data-stu-id="8482a-206">Configuration by environment</span></span>

<span data-ttu-id="8482a-207"><xref:fundamentals/configuration/index#configuration-by-environment> の「*環境別の構成*」セクションを設定します。</span><span class="sxs-lookup"><span data-stu-id="8482a-207">See the *Configuration by environment* section of <xref:fundamentals/configuration/index#configuration-by-environment>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="8482a-208">環境別の起動のクラスとメソッド</span><span class="sxs-lookup"><span data-stu-id="8482a-208">Environment-based Startup class and methods</span></span>

### <a name="startup-class-conventions"></a><span data-ttu-id="8482a-209">Startup クラスの規約</span><span class="sxs-lookup"><span data-stu-id="8482a-209">Startup class conventions</span></span>

<span data-ttu-id="8482a-210">ASP.NET Core アプリの起動時、[Startup クラス](xref:fundamentals/startup)がアプリをブートストラップします。</span><span class="sxs-lookup"><span data-stu-id="8482a-210">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="8482a-211">アプリケーションでは、環境 (たとえば `StartupDevelopment`) ごとに個別の `Startup` クラスを定義することができます。実行時に適切な `Startup` クラスが選択されます。</span><span class="sxs-lookup"><span data-stu-id="8482a-211">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`), and the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="8482a-212">名前のサフィックスが現在の環境と一致するクラスが優先されます。</span><span class="sxs-lookup"><span data-stu-id="8482a-212">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="8482a-213">一致する `Startup{EnvironmentName}` クラスが見つからない場合、`Startup` クラスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="8482a-213">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span>

<span data-ttu-id="8482a-214">環境ベースの `Startup` クラスを実装するには、使用中の各環境に対して `Startup{EnvironmentName}` クラスと、フォールバックの `Startup` クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="8482a-214">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}
```

<span data-ttu-id="8482a-215">アセンブリ名を受け入れる [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) オーバーロードを使用します。</span><span class="sxs-lookup"><span data-stu-id="8482a-215">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName);
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    CreateWebHost(args).Run();
}

public static IWebHost CreateWebHost(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName)
        .Build();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

        var host = new WebHostBuilder()
            .UseStartup(assemblyName)
            .Build();

        host.Run();
    }
}
```

::: moniker-end

### <a name="startup-method-conventions"></a><span data-ttu-id="8482a-216">Startup メソッドの規約</span><span class="sxs-lookup"><span data-stu-id="8482a-216">Startup method conventions</span></span>

<span data-ttu-id="8482a-217">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) と [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) は、フォーム `Configure<EnvironmentName>` と `Configure<EnvironmentName>Services` の環境固有バージョンに対応しています。</span><span class="sxs-lookup"><span data-stu-id="8482a-217">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`:</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a><span data-ttu-id="8482a-218">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="8482a-218">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="8482a-219">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="8482a-219">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
