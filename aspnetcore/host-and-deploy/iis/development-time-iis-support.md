---
title: Visual Studio for ASP.NET Core の開発時 IIS サポート
author: shirhatti
description: ASP.NET Core アプリが Windows Server の IIS の背後で実行されている場合に、そのデバッグのサポートを検出します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/17/2018
ms.locfileid: "34233080"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="38070-103">Visual Studio for ASP.NET Core の開発時 IIS サポート</span><span class="sxs-lookup"><span data-stu-id="38070-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="38070-104">著者: [Sourabh Shirhatti](https://twitter.com/sshirhatti)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="38070-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="38070-105">この記事では、 Windows Server の IIS の背後で実行されている ASP.NET Core アプリをデバッグするための、[Visual Studio](https://www.visualstudio.com/vs/) のサポートについて説明します。</span><span class="sxs-lookup"><span data-stu-id="38070-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="38070-106">このトピックでは、手順に従ってこの機能を有効にし、プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="38070-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="38070-107">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="38070-107">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="38070-108">IIS を有効にする</span><span class="sxs-lookup"><span data-stu-id="38070-108">Enable IIS</span></span>

1. <span data-ttu-id="38070-109">**[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。</span><span class="sxs-lookup"><span data-stu-id="38070-109">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="38070-110">**[インターネット インフォメーション サービス]** チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="38070-110">Select the **Internet Information Services** check box.</span></span>

![[インターネット インフォメーション サービス] チェック ボックスが黒い四角 (チェックマークではない) で選択され、一部の IIS 機能が有効であることが表示されている [Windows の機能]](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="38070-112">IIS のインストールには、システムの再起動が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="38070-112">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="38070-113">IIS の構成</span><span class="sxs-lookup"><span data-stu-id="38070-113">Configure IIS</span></span>

<span data-ttu-id="38070-114">IIS には、次のように構成された Web サイトが含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="38070-114">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="38070-115">アプリの起動プロファイルの URL ホスト名と一致するホスト名。</span><span class="sxs-lookup"><span data-stu-id="38070-115">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="38070-116">割り当てられた証明書でのポート 443 のバインド。</span><span class="sxs-lookup"><span data-stu-id="38070-116">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="38070-117">たとえば、追加された Web サイトの **[ホスト名]** は "localhost" に設定されます (このトピックの後半では、起動プロファイルも "localhost" を使用します)。</span><span class="sxs-lookup"><span data-stu-id="38070-117">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="38070-118">ポートは "443" (HTTPS) に設定されます。</span><span class="sxs-lookup"><span data-stu-id="38070-118">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="38070-119">**IIS Express Development Certificate** が Web サイトに割り当てられていますが、すべての有効な証明書を使用できます。</span><span class="sxs-lookup"><span data-stu-id="38070-119">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![証明書が割り当てられた、ポート 443 での localhost に対するバインド セットを表示する、IIS の [Web サイトの追加] ウィンドウ](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="38070-121">ホスト名がアプリの起動プロファイルの URL ホスト名と一致する **[既定の Web サイト]** が、IIS のインストールに既に含まれている場合は、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="38070-121">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="38070-122">ポート 443 (HTTPS) に対するポートのバインドを追加する。</span><span class="sxs-lookup"><span data-stu-id="38070-122">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="38070-123">Web サイトに有効な証明書を割り当てる。</span><span class="sxs-lookup"><span data-stu-id="38070-123">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="38070-124">Visual Studio の開発時 IIS サポートを有効にする</span><span class="sxs-lookup"><span data-stu-id="38070-124">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="38070-125">Visual Studio インストーラーを起動します。</span><span class="sxs-lookup"><span data-stu-id="38070-125">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="38070-126">**[開発時 IIS サポート]** コンポーネントを選択します。</span><span class="sxs-lookup"><span data-stu-id="38070-126">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="38070-127">**[ASP.NET と Web 開発]** ワークロードの **[概要]** パネルに、このコンポーネントがオプションとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="38070-127">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="38070-128">このコンポーネントにより、リバース プロキシ構成の IIS の背後で ASP.NET Core アプリを実行するのに必要なネイティブの IIS モジュールである [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="38070-128">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps behind IIS in a reverse proxy configuration.</span></span>

![Visual Studio の機能の変更: [ワークロード] タブが選択されています。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="38070-132">プロジェクトを構成する</span><span class="sxs-lookup"><span data-stu-id="38070-132">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="38070-133">HTTPS リダイレクト</span><span class="sxs-lookup"><span data-stu-id="38070-133">HTTPS redirection</span></span>

<span data-ttu-id="38070-134">新しいプロジェクトの場合、**[新しい ASP.NET Core Web アプリケーション]** ウィンドウで、**[Configure for HTTPS]\(HTTPS 用に構成する\)** のチェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="38070-134">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![HTTPS 用に構成するチェック ボックスが選択された新しい ASP.NET Core Web アプリケーション ウィンドウ](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="38070-136">既存のプロジェクトで、[UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) 拡張メソッドを呼び出すことで、`Startup.Configure` の HTTPS リダイレクト ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="38070-136">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a><span data-ttu-id="38070-137">IIS 起動プロファイル</span><span class="sxs-lookup"><span data-stu-id="38070-137">IIS launch profile</span></span>

<span data-ttu-id="38070-138">新しい起動プロファイルを作成して、開発時 IIS サポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="38070-138">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="38070-139">**[プロファイル]** で、**[新規]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="38070-139">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="38070-140">ポップアップ ウィンドウで、プロファイルに "IIS" という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="38070-140">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="38070-141">**[OK]** をクリックして、プロファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="38070-141">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="38070-142">**[起動]** の設定で、一覧から **[IIS]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="38070-142">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="38070-143">**[ブラウザーの起動]** チェック ボックスをオンにして、エンドポイント URL を指定します。</span><span class="sxs-lookup"><span data-stu-id="38070-143">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="38070-144">HTTPS プロトコルを使用します。</span><span class="sxs-lookup"><span data-stu-id="38070-144">Use the HTTPS protocol.</span></span> <span data-ttu-id="38070-145">この例では `https://localhost/WebApplication1` を使用します。</span><span class="sxs-lookup"><span data-stu-id="38070-145">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="38070-146">**[環境変数]** セクションで、**[追加]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="38070-146">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="38070-147">キーを `ASPNETCORE_ENVIRONMENT`、値を `Development` として環境変数を指定します。</span><span class="sxs-lookup"><span data-stu-id="38070-147">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="38070-148">**[Web サーバー設定]** で **[アプリ URL]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="38070-148">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="38070-149">この例では `https://localhost/WebApplication1` を使用します。</span><span class="sxs-lookup"><span data-stu-id="38070-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="38070-150">プロファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="38070-150">Save the profile.</span></span>

![[デバッグ] タブが選択された、プロジェクト プロパティのウィンドウ。](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="38070-155">または、起動プロファイルをアプリの [launchSettings.json](http://json.schemastore.org/launchsettings) ファイルに手動で追加します。</span><span class="sxs-lookup"><span data-stu-id="38070-155">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

## <a name="run-the-project"></a><span data-ttu-id="38070-156">プロジェクトを実行する</span><span class="sxs-lookup"><span data-stu-id="38070-156">Run the project</span></span>

<span data-ttu-id="38070-157">VS UI で、[実行] ボタンを **[IIS]** プロファイルに設定し、ボタンを選択してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="38070-157">In the VS UI, set the Run button to the **IIS** profile and select the button to start the app:</span></span>

!["IIS" プロファイルに設定された VS ツール バーの実行ボタン](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="38070-159">管理者として実行していない場合、Visual Studio によって再起動を求められる場合があります。</span><span class="sxs-lookup"><span data-stu-id="38070-159">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="38070-160">その場合は、Visual Studio を再起動します。</span><span class="sxs-lookup"><span data-stu-id="38070-160">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="38070-161">信頼されていない開発証明書を使用すると、信頼されていない証明書に対する例外を作成するように、ブラウザーによって求められる場合があります。</span><span class="sxs-lookup"><span data-stu-id="38070-161">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="38070-162">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="38070-162">Additional resources</span></span>

* [<span data-ttu-id="38070-163">IIS を使用した Windows での ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="38070-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="38070-164">ASP.NET Core モジュールの概要</span><span class="sxs-lookup"><span data-stu-id="38070-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="38070-165">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="38070-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="38070-166">HTTPS の適用</span><span class="sxs-lookup"><span data-stu-id="38070-166">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
