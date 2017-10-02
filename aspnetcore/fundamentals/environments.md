---
title: "ASP.NET Core での複数の環境での作業"
author: ardalis
description: "ASP.NET Core が複数の環境間でのアプリの動作を制御するためのサポートを提供する方法について説明します。"
keywords: "ASP.NET Core、環境の設定、ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b5bba985-be12-4464-9a01-df3599b2a6f1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 054b3e9f1e2bcfe1e4a75eca4d9dc6326ee6e44f
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2017
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="f5520-104">複数の環境での作業</span><span class="sxs-lookup"><span data-stu-id="f5520-104">Working with multiple environments</span></span>

<span data-ttu-id="f5520-105">によって[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f5520-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f5520-106">ASP.NET Core は、開発、ステージング、運用環境など、複数の環境間でのアプリの動作を制御するためのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="f5520-106">ASP.NET Core provides support for controlling app behavior across multiple environments, such as development, staging, and production.</span></span> <span data-ttu-id="f5520-107">環境変数は、その環境用に構成するアプリを許可する、ランタイム環境を示すために使用されます。</span><span class="sxs-lookup"><span data-stu-id="f5520-107">Environment variables are used to indicate the runtime environment, allowing the app to be configured for that environment.</span></span>

<span data-ttu-id="f5520-108">[表示またはダウンロードするサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f5520-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="development-staging-production"></a><span data-ttu-id="f5520-109">開発、ステージング、実稼働環境</span><span class="sxs-lookup"><span data-stu-id="f5520-109">Development, Staging, Production</span></span>

<span data-ttu-id="f5520-110">ASP.NET Core を参照して、特定[環境変数](https://github.com/aspnet/Home/wiki)、`ASPNETCORE_ENVIRONMENT`にで、アプリケーションが実行されている環境について説明します。</span><span class="sxs-lookup"><span data-stu-id="f5520-110">ASP.NET Core references a particular [environment variable](https://github.com/aspnet/Home/wiki), `ASPNETCORE_ENVIRONMENT` to describe the environment the application is currently running in.</span></span> <span data-ttu-id="f5520-111">この変数を設定する任意の値が、通常使用される 3 つの値: `Development`、 `Staging`、および`Production`です。</span><span class="sxs-lookup"><span data-stu-id="f5520-111">This variable can be set to any value you like, but three values are used by convention: `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="f5520-112">これらのサンプルで使用される値と ASP.NET Core で提供されるテンプレートが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f5520-112">You will find these values used in the samples and templates provided with ASP.NET Core.</span></span>

<span data-ttu-id="f5520-113">現在の環境設定を検出できますプログラムから、アプリケーション内で。</span><span class="sxs-lookup"><span data-stu-id="f5520-113">The current environment setting can be detected programmatically from within your application.</span></span> <span data-ttu-id="f5520-114">環境を使用してさらに、[タグ ヘルパー](../mvc/views/tag-helpers/index.md)に含める特定のセクションで、[ビュー](../mvc/views/index.md)現在アプリケーション環境に基づきます。</span><span class="sxs-lookup"><span data-stu-id="f5520-114">In addition, you can use the Environment [tag helper](../mvc/views/tag-helpers/index.md) to include certain sections in your [view](../mvc/views/index.md) based on the current application environment.</span></span>

<span data-ttu-id="f5520-115">注: Windows および macOS、環境名を指定は大文字小文字を区別します。</span><span class="sxs-lookup"><span data-stu-id="f5520-115">Note: On Windows and macOS, the specified environment name is case insensitive.</span></span> <span data-ttu-id="f5520-116">変数を設定するかどうか`Development`または`development`または`DEVELOPMENT`結果は同じになります。</span><span class="sxs-lookup"><span data-stu-id="f5520-116">Whether you set the variable to `Development` or `development` or `DEVELOPMENT` the results will be the same.</span></span> <span data-ttu-id="f5520-117">ただしは、Linux、**大文字小文字を区別**既定では OS。</span><span class="sxs-lookup"><span data-stu-id="f5520-117">However, Linux is a **case sensitive** OS by default.</span></span> <span data-ttu-id="f5520-118">環境変数、ファイル名と設定は、大文字小文字の区別を必要とします。</span><span class="sxs-lookup"><span data-stu-id="f5520-118">Environment variables, file names and settings require case sensitivity.</span></span>

### <a name="development"></a><span data-ttu-id="f5520-119">開発</span><span class="sxs-lookup"><span data-stu-id="f5520-119">Development</span></span>

<span data-ttu-id="f5520-120">これは、アプリケーションを開発するときに使用する環境でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="f5520-120">This should be the environment used when developing an application.</span></span> <span data-ttu-id="f5520-121">通常はありませんにするなど、実稼働環境でアプリの実行時に使用できる機能を有効に使用、[開発者例外ページ](xref:fundamentals/error-handling#the-developer-exception-page)です。</span><span class="sxs-lookup"><span data-stu-id="f5520-121">It is typically used to enable features that you wouldn't want to be available when the app runs in production, such as the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page).</span></span>

<span data-ttu-id="f5520-122">Visual Studio を使用している場合は、プロジェクトのデバッグ · プロファイルで、環境を構成できます。</span><span class="sxs-lookup"><span data-stu-id="f5520-122">If you're using Visual Studio, the environment can be configured in your project's debug profiles.</span></span> <span data-ttu-id="f5520-123">デバッグ プロファイルを指定して、[サーバー](xref:fundamentals/servers/index)を設定するアプリケーションやその環境変数を起動するときに使用します。</span><span class="sxs-lookup"><span data-stu-id="f5520-123">Debug profiles specify the [server](xref:fundamentals/servers/index) to use when launching the application and any environment variables to be set.</span></span> <span data-ttu-id="f5520-124">プロジェクトには、複数のデバッグ プロファイル環境変数を異なる方法で設定することができます。</span><span class="sxs-lookup"><span data-stu-id="f5520-124">Your project can have multiple debug profiles that set environment variables differently.</span></span> <span data-ttu-id="f5520-125">使用してこれらのプロファイルを管理する、**デバッグ**web アプリケーション プロジェクトのタブ**プロパティ**メニュー。</span><span class="sxs-lookup"><span data-stu-id="f5520-125">You manage these profiles by using the **Debug** tab of your web application project's **Properties** menu.</span></span> <span data-ttu-id="f5520-126">プロジェクトのプロパティで設定する値が永続化、 *launchSettings.json*ファイル、およびすることができますもプロファイルを構成ファイルを直接編集することによってです。</span><span class="sxs-lookup"><span data-stu-id="f5520-126">The values you set in project properties are persisted in the *launchSettings.json* file, and you can also configure profiles by editing that file directly.</span></span>

<span data-ttu-id="f5520-127">IIS Express のプロファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="f5520-127">The profile for IIS Express is shown here:</span></span>

![プロジェクト プロパティ設定の環境変数](environments/_static/project-properties-debug.png)

<span data-ttu-id="f5520-129">ここでは、`launchSettings.json`用のプロファイルを含むファイル`Development`と`Staging`:</span><span class="sxs-lookup"><span data-stu-id="f5520-129">Here is a `launchSettings.json` file that includes profiles for `Development` and `Staging`:</span></span>

[!code-json[Main](../fundamentals/environments/sample/src/Environments/Properties/launchSettings.json?highlight=15,22)]

<span data-ttu-id="f5520-130">プロジェクトのプロファイルに加えられた変更は反映されません使用される web サーバーが再起動されるまで (具体的には、Kestrel 再起動する必要がその環境に加えられた変更が検出する前に)。</span><span class="sxs-lookup"><span data-stu-id="f5520-130">Changes made to project profiles may not take effect until the web server used is restarted (in particular, Kestrel must be restarted before it will detect changes made to its environment).</span></span>

>[!WARNING]
> <span data-ttu-id="f5520-131">環境変数に格納*launchSettings.json*任意の方法でセキュリティ保護されていないと、1 つを使用する場合に、プロジェクトのソース コード リポジトリの一部になります。</span><span class="sxs-lookup"><span data-stu-id="f5520-131">Environment variables stored in *launchSettings.json* are not secured in any way and will be part of the source code repository for your project, if you use one.</span></span> <span data-ttu-id="f5520-132">**このファイルに資格情報またはその他の機密データを保存しないでください。**</span><span class="sxs-lookup"><span data-stu-id="f5520-132">**Never store credentials or other secret data in this file.**</span></span> <span data-ttu-id="f5520-133">このようなデータを格納する場所を必要がある場合、*シークレット Manager*ツール」に記載[アプリ シークレットは、開発中の安全な保管](../security/app-secrets.md#security-app-secrets)です。</span><span class="sxs-lookup"><span data-stu-id="f5520-133">If you need a place to store such data, use the *Secret Manager* tool described in [Safe storage of app secrets during development](../security/app-secrets.md#security-app-secrets).</span></span>

### <a name="staging"></a><span data-ttu-id="f5520-134">ステージング</span><span class="sxs-lookup"><span data-stu-id="f5520-134">Staging</span></span>

<span data-ttu-id="f5520-135">慣例により、`Staging`環境とは、実稼働前環境の実稼働環境に展開する前に最終的なテストに使用します。</span><span class="sxs-lookup"><span data-stu-id="f5520-135">By convention, a `Staging` environment is a pre-production environment used for final testing before deployment to production.</span></span> <span data-ttu-id="f5520-136">理想的には、物理的な特性をミラー化する実稼働環境でのユーザーに影響を与えずに対処できますが、ステージング環境で最初に実稼働環境で発生する可能性のある問題が発生するようにします。</span><span class="sxs-lookup"><span data-stu-id="f5520-136">Ideally, its physical characteristics should mirror that of production, so that any issues that may arise in production occur first in the staging environment, where they can be addressed without impact to users.</span></span>

### <a name="production"></a><span data-ttu-id="f5520-137">実稼働</span><span class="sxs-lookup"><span data-stu-id="f5520-137">Production</span></span>

<span data-ttu-id="f5520-138">`Production`環境はライブであるときに、アプリケーションを実行する環境エンドユーザーによって使用されているとします。</span><span class="sxs-lookup"><span data-stu-id="f5520-138">The `Production` environment is the environment in which the application runs when it is live and being used by end users.</span></span> <span data-ttu-id="f5520-139">この環境は、セキュリティ、パフォーマンス、およびアプリケーションの堅牢性を最大化するように構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f5520-139">This environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="f5520-140">一般的な運用環境が存在する可能性のある設定の開発とは異なりますは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="f5520-140">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="f5520-141">キャッシュを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f5520-141">Turn on caching</span></span>

* <span data-ttu-id="f5520-142">クライアント側のすべてのリソースがバンドルされている、縮小、および CDN から供給される可能性があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f5520-142">Ensure all client-side resources are bundled, minified, and potentially served from a CDN</span></span>

* <span data-ttu-id="f5520-143">診断 ErrorPages をオフにします。</span><span class="sxs-lookup"><span data-stu-id="f5520-143">Turn off diagnostic ErrorPages</span></span>

* <span data-ttu-id="f5520-144">わかりやすいエラー ページで有効にします。</span><span class="sxs-lookup"><span data-stu-id="f5520-144">Turn on friendly error pages</span></span>

* <span data-ttu-id="f5520-145">運用ログおよび監視を有効にする (たとえば、 [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/))</span><span class="sxs-lookup"><span data-stu-id="f5520-145">Enable production logging and monitoring (for example, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/))</span></span>

<span data-ttu-id="f5520-146">これは、操作は、完全な一覧を示すものではではありません。</span><span class="sxs-lookup"><span data-stu-id="f5520-146">This is by no means meant to be a complete list.</span></span> <span data-ttu-id="f5520-147">なら、アプリケーションの多くの部分で環境のチェックを回避することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="f5520-147">It's best to avoid scattering environment checks in many parts of your application.</span></span> <span data-ttu-id="f5520-148">推奨される方法は、アプリケーション内でそのようなチェックを実行する代わりに、`Startup`クラス可能な限り</span><span class="sxs-lookup"><span data-stu-id="f5520-148">Instead, the recommended approach is to perform such checks within the application's `Startup` class(es) wherever possible</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="f5520-149">環境の設定</span><span class="sxs-lookup"><span data-stu-id="f5520-149">Setting the environment</span></span>

<span data-ttu-id="f5520-150">環境を設定するためのメソッドは、オペレーティング システムによって異なります。</span><span class="sxs-lookup"><span data-stu-id="f5520-150">The method for setting the environment depends on the operating system.</span></span>

### <a name="windows"></a><span data-ttu-id="f5520-151">Windows</span><span class="sxs-lookup"><span data-stu-id="f5520-151">Windows</span></span>
<span data-ttu-id="f5520-152">設定する、`ASPNETCORE_ENVIRONMENT`を使用して、アプリが開始された場合、現在のセッションの`dotnet run`、次のコマンドを使用</span><span class="sxs-lookup"><span data-stu-id="f5520-152">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="f5520-153">**コマンド ライン**</span><span class="sxs-lookup"><span data-stu-id="f5520-153">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="f5520-154">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="f5520-154">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="f5520-155">これらのコマンドは、現在のウィンドウに対してのみ有効になります。</span><span class="sxs-lookup"><span data-stu-id="f5520-155">These commands take effect only for the current window.</span></span> <span data-ttu-id="f5520-156">ウィンドウが閉じられたときに、ASPNETCORE_ENVIRONMENT 設定は、既定の設定またはコンピューターの値に戻ります。</span><span class="sxs-lookup"><span data-stu-id="f5520-156">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="f5520-157">Windows を開いたとき、値をグローバルに設定するために、**コントロール パネルの**  > **システム** > **システムの詳細設定**を追加または編集、 `ASPNETCORE_ENVIRONMENT`値。</span><span class="sxs-lookup"><span data-stu-id="f5520-157">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![システムの詳細プロパティ](environments/_static/systemsetting_environment.png)

![ASPNET コア環境変数](environments/_static/windows_aspnetcore_environment.png) 

<span data-ttu-id="f5520-160">**web.config**</span><span class="sxs-lookup"><span data-stu-id="f5520-160">**web.config**</span></span>

<span data-ttu-id="f5520-161">参照してください、*環境変数の設定*のセクションで、 [ASP.NET Core モジュール構成リファレンス](xref:hosting/aspnet-core-module#setting-environment-variables)トピックです。</span><span class="sxs-lookup"><span data-stu-id="f5520-161">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="f5520-162">**IIS アプリケーション プール単位**</span><span class="sxs-lookup"><span data-stu-id="f5520-162">**Per IIS Application Pool**</span></span>

<span data-ttu-id="f5520-163">分離されたアプリケーション プール (IIS 10.0 以降でサポートされています) で実行する個別アプリに対して環境変数を設定する必要がある場合は、IIS のリファレンス ドキュメントで、[環境変数\<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) のトピックにある *AppCmd.exe コマンド*のセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f5520-163">If you need to set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

### <a name="macos"></a><span data-ttu-id="f5520-164">MacOS</span><span class="sxs-lookup"><span data-stu-id="f5520-164">macOS</span></span>
<span data-ttu-id="f5520-165">MacOS の現在の環境を設定する場合に実行できます行で、アプリケーションを実行しています。</span><span class="sxs-lookup"><span data-stu-id="f5520-165">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="f5520-166">使用してまたは`export`をアプリを実行する前に設定します。</span><span class="sxs-lookup"><span data-stu-id="f5520-166">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
``` 
<span data-ttu-id="f5520-167">コンピューターのレベルの環境変数が設定されて、*なる*または*.bash_profile*ファイル。</span><span class="sxs-lookup"><span data-stu-id="f5520-167">Machine level environment variables are set in the *.bashrc*  or *.bash_profile* file.</span></span> <span data-ttu-id="f5520-168">任意のテキスト エディターを使用して、ファイルを編集し、次のステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="f5520-168">Edit the file using any text editor and add the following statment.</span></span>

```
export ASPNETCORE_ENVIRONMENT=Development
```  

### <a name="linux"></a><span data-ttu-id="f5520-169">Linux</span><span class="sxs-lookup"><span data-stu-id="f5520-169">Linux</span></span>
<span data-ttu-id="f5520-170">Linux ディストリビューションの場合を使用して、`export`セッション ベースの変数の設定のコマンドラインでコマンドと*bash_profile*マシン レベルの環境設定のファイルです。</span><span class="sxs-lookup"><span data-stu-id="f5520-170">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

## <a name="determining-the-environment-at-runtime"></a><span data-ttu-id="f5520-171">実行時に環境を決定します。</span><span class="sxs-lookup"><span data-stu-id="f5520-171">Determining the environment at runtime</span></span>

<span data-ttu-id="f5520-172">`IHostingEnvironment`環境を操作するための中核となる抽象型を提供します。</span><span class="sxs-lookup"><span data-stu-id="f5520-172">The `IHostingEnvironment` service provides the core abstraction for working with environments.</span></span> <span data-ttu-id="f5520-173">このサービスは、提供、ASP.NET によってホスト レイヤーとを使用して、スタートアップ ロジックに挿入できます[依存性の注入](dependency-injection.md)です。</span><span class="sxs-lookup"><span data-stu-id="f5520-173">This service is provided by the ASP.NET hosting layer, and can be injected into your startup logic via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="f5520-174">Visual Studio での ASP.NET Core web サイト テンプレートは、(存在する場合)、環境固有の構成ファイルをロードして、アプリのエラー処理設定をカスタマイズする、このアプローチを使用します。</span><span class="sxs-lookup"><span data-stu-id="f5520-174">The ASP.NET Core web site template in Visual Studio uses this approach to load environment-specific configuration files (if present) and to customize the app's error handling settings.</span></span> <span data-ttu-id="f5520-175">どちらの場合も、この動作には、呼び出すことによって、現在指定されている環境を参照する`EnvironmentName`または`IsEnvironment`のインスタンスで`IHostingEnvironment`適切なメソッドに渡されます。</span><span class="sxs-lookup"><span data-stu-id="f5520-175">In both cases, this behavior is achieved by referring to the currently specified environment by calling `EnvironmentName` or `IsEnvironment` on the instance of `IHostingEnvironment` passed into the appropriate method.</span></span>

> [!NOTE]
> <span data-ttu-id="f5520-176">使用して、特定の環境で、アプリケーションが実行されているかどうかを確認する必要がある場合`env.IsEnvironment("environmentname")`正しくの場合は無視されますので (確認する場合ではなく`env.EnvironmentName == "Development"`など)。</span><span class="sxs-lookup"><span data-stu-id="f5520-176">If you need to check whether the application is running in a particular environment, use `env.IsEnvironment("environmentname")` since it will correctly ignore case (instead of checking if `env.EnvironmentName == "Development"` for example).</span></span>

<span data-ttu-id="f5520-177">たとえば、環境固有のエラー処理をセットアップするのに、構成メソッドに次のコードを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="f5520-177">For example, you can use the following code in your Configure method to setup environment specific error handling:</span></span>

[!code-csharp[Main](environments/sample/src/Environments/Startup.cs?range=19-30)]

<span data-ttu-id="f5520-178">アプリが実行されている場合、`Development`その環境では、Visual Studio、開発に固有のエラー ページ (通常は実行できません実稼働環境で) および特別なデータベース エラーに"BrowserLink"機能を使用するために必要なランタイム サポートを有効にページ (移行を適用する方法を提供し、開発でのみ使用するため)。</span><span class="sxs-lookup"><span data-stu-id="f5520-178">If the app is running in a `Development` environment, then it enables the runtime support necessary to use the "BrowserLink" feature in Visual Studio, development-specific error pages (which typically should not be run in production) and special database error pages (which provide a way to apply migrations and should therefore only be used in development).</span></span> <span data-ttu-id="f5520-179">それ以外の場合、アプリは開発環境で実行されていない、ハンドルされない例外への応答に表示される標準的なエラー処理 ページが構成されます。</span><span class="sxs-lookup"><span data-stu-id="f5520-179">Otherwise, if the app is not running in a development environment, a standard error handling page is configured to be displayed in response to any unhandled exceptions.</span></span>

<span data-ttu-id="f5520-180">現在の環境に応じて、実行時にクライアントに送信するコンテンツを決定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f5520-180">You may need to determine which content to send to the client at runtime, depending on the current environment.</span></span> <span data-ttu-id="f5520-181">たとえば、開発環境で一般に提供する最小化されていないスクリプトとスタイル シートは、デバッグが簡単には。</span><span class="sxs-lookup"><span data-stu-id="f5520-181">For example, in a development environment you generally serve non-minimized scripts and style sheets, which makes debugging easier.</span></span> <span data-ttu-id="f5520-182">運用環境とテスト環境が縮小されたバージョンを提供する必要がありますと、CDN から一般にします。</span><span class="sxs-lookup"><span data-stu-id="f5520-182">Production and test environments should serve the minified versions and generally from a CDN.</span></span> <span data-ttu-id="f5520-183">これを行う環境を使用して[タグ ヘルパー](../mvc/views/tag-helpers/intro.md)です。</span><span class="sxs-lookup"><span data-stu-id="f5520-183">You can do this using the Environment [tag helper](../mvc/views/tag-helpers/intro.md).</span></span> <span data-ttu-id="f5520-184">現在の環境を使用して指定された環境のいずれかと一致する場合は、環境タグ ヘルパーにその内容は表示のみ、`names`属性。</span><span class="sxs-lookup"><span data-stu-id="f5520-184">The Environment tag helper will only render its contents if the current environment matches one of the environments specified using the `names` attribute.</span></span>

[!code-html[Main](environments/sample/src/Environments/Views/Shared/_Layout.cshtml?range=13-22)]

<span data-ttu-id="f5520-185">アプリケーションを参照してくださいタグ ヘルパーの使用を開始する[タグ ヘルパーの概要](../mvc/views/tag-helpers/intro.md)です。</span><span class="sxs-lookup"><span data-stu-id="f5520-185">To get started with using tag helpers in your application see [Introduction to Tag Helpers](../mvc/views/tag-helpers/intro.md).</span></span>

## <a name="startup-conventions"></a><span data-ttu-id="f5520-186">スタートアップの表記規則</span><span class="sxs-lookup"><span data-stu-id="f5520-186">Startup conventions</span></span>

<span data-ttu-id="f5520-187">ASP.NET Core では、現在の環境に基づくアプリケーションのスタートアップの構成、規則ベースのアプローチをサポートします。</span><span class="sxs-lookup"><span data-stu-id="f5520-187">ASP.NET Core supports a convention-based approach to configuring an application's startup based on the current environment.</span></span> <span data-ttu-id="f5520-188">アプリケーションの動作に基づいてどの環境には、作成し、独自の規則を管理することができますもプログラムで制御できます。</span><span class="sxs-lookup"><span data-stu-id="f5520-188">You can also programmatically control how your application behaves according to which environment it is in, allowing you to create and manage your own conventions.</span></span>

<span data-ttu-id="f5520-189">ASP.NET Core アプリケーションの起動時、`Startup`ブートス トラップ アプリケーション、その構成設定などをロードするクラスを使用 ([の詳細については、ASP.NET スタートアップ](startup.md))。</span><span class="sxs-lookup"><span data-stu-id="f5520-189">When an ASP.NET Core application starts, the `Startup` class is used to bootstrap the application, load its configuration settings, etc. ([learn more about ASP.NET startup](startup.md)).</span></span> <span data-ttu-id="f5520-190">ただし、クラスが存在する場合が名前付き`Startup{EnvironmentName}`(たとえば`StartupDevelopment`)、および`ASPNETCORE_ENVIRONMENT`環境変数は、その名前をそのと一致する`Startup`クラスは、代わりに使用します。</span><span class="sxs-lookup"><span data-stu-id="f5520-190">However, if a class exists named `Startup{EnvironmentName}` (for example `StartupDevelopment`), and the `ASPNETCORE_ENVIRONMENT` environment variable matches that name, then that `Startup` class is used instead.</span></span> <span data-ttu-id="f5520-191">したがって、構成することも`Startup`開発では、独立した`StartupProduction`を実稼働環境でアプリの実行時に使用されます。</span><span class="sxs-lookup"><span data-stu-id="f5520-191">Thus, you could configure `Startup` for development, but have a separate `StartupProduction` that would be used when the app is run in production.</span></span> <span data-ttu-id="f5520-192">またはその逆です。</span><span class="sxs-lookup"><span data-stu-id="f5520-192">Or vice versa.</span></span>

> [!NOTE]
> <span data-ttu-id="f5520-193">呼び出す`WebHostBuilder.UseStartup<TStartup>()`構成セクションをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="f5520-193">Calling `WebHostBuilder.UseStartup<TStartup>()` overrides configuration sections.</span></span>

<span data-ttu-id="f5520-194">まったく別の使用に加えて`Startup`クラスの現在の環境に基づく内でアプリケーションを構成する方法の調整を行うことも、`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="f5520-194">In addition to using an entirely separate `Startup` class based on the current environment, you can also make adjustments to how the application is configured within a `Startup` class.</span></span> <span data-ttu-id="f5520-195">`Configure()`と`ConfigureServices()`メソッドのような環境固有のバージョンのサポート、`Startup`クラス、フォームの自体`Configure{EnvironmentName}()`と`Configure{EnvironmentName}Services()`です。</span><span class="sxs-lookup"><span data-stu-id="f5520-195">The `Configure()` and `ConfigureServices()` methods support environment-specific versions similar to the `Startup` class itself, of the form `Configure{EnvironmentName}()` and `Configure{EnvironmentName}Services()`.</span></span> <span data-ttu-id="f5520-196">メソッドを定義する場合`ConfigureDevelopment()`の代わりに呼び出されます`Configure()`開発環境を設定するとします。</span><span class="sxs-lookup"><span data-stu-id="f5520-196">If you define a method `ConfigureDevelopment()` it will be called instead of `Configure()` when the environment is set to development.</span></span> <span data-ttu-id="f5520-197">同様に、`ConfigureDevelopmentServices()`はの代わりに呼び出されます`ConfigureServices()`同じ環境内でします。</span><span class="sxs-lookup"><span data-stu-id="f5520-197">Likewise, `ConfigureDevelopmentServices()` would be called instead of `ConfigureServices()` in the same environment.</span></span>

## <a name="summary"></a><span data-ttu-id="f5520-198">概要</span><span class="sxs-lookup"><span data-stu-id="f5520-198">Summary</span></span>

<span data-ttu-id="f5520-199">ASP.NET Core では、さまざまな機能と開発者はさまざまな環境で、アプリケーションの動作を簡単に制御を許可する規則を提供します。</span><span class="sxs-lookup"><span data-stu-id="f5520-199">ASP.NET Core provides a number of features and conventions that allow developers to easily control how their applications behave in different environments.</span></span> <span data-ttu-id="f5520-200">実稼働環境にステージングするには、開発環境からアプリケーションを発行するときに環境変数を設定適切に環境は、必要に応じて、デバッグ、テスト、または実稼働環境で使用するアプリケーションの最適化できます。</span><span class="sxs-lookup"><span data-stu-id="f5520-200">When publishing an application from development to staging to production, environment variables set appropriately for the environment allow for optimization of the application for debugging, testing, or production use, as appropriate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f5520-201">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="f5520-201">Additional Resources</span></span>

* [<span data-ttu-id="f5520-202">構成</span><span class="sxs-lookup"><span data-stu-id="f5520-202">Configuration</span></span>](configuration.md)

* [<span data-ttu-id="f5520-203">Tag Helpers の概要</span><span class="sxs-lookup"><span data-stu-id="f5520-203">Introduction to Tag Helpers</span></span>](../mvc/views/tag-helpers/intro.md)
