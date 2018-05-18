---
title: Visual Studio for ASP.NET Core の開発時 IIS サポート
author: shirhatti
description: Windows Server 上の IIS の背後にある実行時に ASP.NET Core アプリケーションのデバッグのサポートを検出します。
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
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/17/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="f53d5-103">Visual Studio for ASP.NET Core の開発時 IIS サポート</span><span class="sxs-lookup"><span data-stu-id="f53d5-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="f53d5-104">によって[Sourabh Shirhatti](https://twitter.com/sshirhatti)と[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f53d5-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f53d5-105">この記事の内容について説明します[Visual Studio](https://www.visualstudio.com/vs/) Windows サーバーで IIS 内側で実行される ASP.NET Core アプリケーションのデバッグをサポートします。</span><span class="sxs-lookup"><span data-stu-id="f53d5-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="f53d5-106">このトピックでは、この機能を有効にして、プロジェクトの設定を処理します。</span><span class="sxs-lookup"><span data-stu-id="f53d5-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f53d5-107">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="f53d5-107">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="f53d5-108">IIS を有効にする</span><span class="sxs-lookup"><span data-stu-id="f53d5-108">Enable IIS</span></span>

1. <span data-ttu-id="f53d5-109">**[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。</span><span class="sxs-lookup"><span data-stu-id="f53d5-109">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="f53d5-110">選択、**インターネット インフォメーション サービス**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="f53d5-110">Select the **Internet Information Services** check box.</span></span>

![黒の四角形 (チェック マークされません) の一部の IIS 機能が有効になっていることを示すチェック Windows の機能を示すインターネット インフォメーション サービス チェック ボックス](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="f53d5-112">IIS のインストールには、システムの再起動を必要があります。</span><span class="sxs-lookup"><span data-stu-id="f53d5-112">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="f53d5-113">IIS の構成</span><span class="sxs-lookup"><span data-stu-id="f53d5-113">Configure IIS</span></span>

<span data-ttu-id="f53d5-114">IIS は、次のように構成されている web サイトである必要があります。</span><span class="sxs-lookup"><span data-stu-id="f53d5-114">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="f53d5-115">アプリの起動プロファイルの URL ホスト名と一致するホスト名です。</span><span class="sxs-lookup"><span data-stu-id="f53d5-115">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="f53d5-116">割り当てられている証明書でポート 443 をバインドします。</span><span class="sxs-lookup"><span data-stu-id="f53d5-116">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="f53d5-117">たとえば、**ホスト名**"localhost"(起動プロファイルも使用します"localhost"このトピックで後述) に追加の web サイトを設定します。</span><span class="sxs-lookup"><span data-stu-id="f53d5-117">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="f53d5-118">ポートは、「443」(HTTPS) に設定されます。</span><span class="sxs-lookup"><span data-stu-id="f53d5-118">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="f53d5-119">**IIS Express 開発証明書**works、web サイトが有効な証明書に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="f53d5-119">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![割り当てられている証明書でポート 443 で localhost バインディング セットを表示する IIS の web サイトのウィンドウを追加します。](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="f53d5-121">既に IIS のインストールがある場合、 **Default Web Site**アプリの起動プロファイルの URL ホスト名と一致するホスト名を持つ。</span><span class="sxs-lookup"><span data-stu-id="f53d5-121">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="f53d5-122">ポート 443 (HTTPS) ポートのバインドを追加します。</span><span class="sxs-lookup"><span data-stu-id="f53d5-122">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="f53d5-123">Web サイトに有効な証明書を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="f53d5-123">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="f53d5-124">Visual Studio での開発時 IIS サポートを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f53d5-124">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="f53d5-125">Visual Studio インストーラーを起動します。</span><span class="sxs-lookup"><span data-stu-id="f53d5-125">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="f53d5-126">選択、 **IIS サポート開発時間**コンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="f53d5-126">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="f53d5-127">コンポーネントが一覧表示でオプションとして、**概要**パネル、 **ASP.NET および web 開発**ワークロード。</span><span class="sxs-lookup"><span data-stu-id="f53d5-127">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="f53d5-128">コンポーネントのインストール、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)、ネイティブの IIS モジュールがリバース プロキシの構成で ASP.NET Core IIS の背後にあるアプリを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f53d5-128">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps behind IIS in a reverse proxy configuration.</span></span>

![Visual Studio の機能の変更: [ワークロード] タブが選択されています。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="f53d5-132">プロジェクトを構成する</span><span class="sxs-lookup"><span data-stu-id="f53d5-132">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="f53d5-133">HTTPS のリダイレクト</span><span class="sxs-lookup"><span data-stu-id="f53d5-133">HTTPS redirection</span></span>

<span data-ttu-id="f53d5-134">新しいプロジェクトの場合、チェック ボックスをオン**HTTPS の構成**で、 **ASP.NET Core Web アプリケーションを新しい**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="f53d5-134">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![新しい ASP.NET Core Web アプリケーション ウィンドウ HTTPS チェック ボックスをオンに設定します。](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="f53d5-136">既存のプロジェクトで内でリダイレクト ミドルウェアを HTTPS を使用して`Startup.Configure`を呼び出して、 [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="f53d5-136">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

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

### <a name="iis-launch-profile"></a><span data-ttu-id="f53d5-137">IIS は、プロファイルを起動します。</span><span class="sxs-lookup"><span data-stu-id="f53d5-137">IIS launch profile</span></span>

<span data-ttu-id="f53d5-138">開発時 IIS サポートを追加する新しい起動プロファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="f53d5-138">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="f53d5-139">**プロファイル**、select、**新規**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f53d5-139">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="f53d5-140">ポップアップ ウィンドウで、プロファイル"IIS"という名前です。</span><span class="sxs-lookup"><span data-stu-id="f53d5-140">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="f53d5-141">選択**OK**プロファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="f53d5-141">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="f53d5-142">**起動**選択を設定する**IIS**一覧からです。</span><span class="sxs-lookup"><span data-stu-id="f53d5-142">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="f53d5-143">チェック ボックスをオン**起動ブラウザー**エンドポイント URL を提供しています。</span><span class="sxs-lookup"><span data-stu-id="f53d5-143">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="f53d5-144">HTTPS プロトコルを使用します。</span><span class="sxs-lookup"><span data-stu-id="f53d5-144">Use the HTTPS protocol.</span></span> <span data-ttu-id="f53d5-145">この例では `https://localhost/WebApplication1` を使用します。</span><span class="sxs-lookup"><span data-stu-id="f53d5-145">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="f53d5-146">**環境変数** セクションで、select、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f53d5-146">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="f53d5-147">環境変数を指定のキーを持つ`ASPNETCORE_ENVIRONMENT`と値の`Development`します。</span><span class="sxs-lookup"><span data-stu-id="f53d5-147">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="f53d5-148">**Web サーバー設定**領域で、設定、**アプリの URL**です。</span><span class="sxs-lookup"><span data-stu-id="f53d5-148">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="f53d5-149">この例では `https://localhost/WebApplication1` を使用します。</span><span class="sxs-lookup"><span data-stu-id="f53d5-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="f53d5-150">プロファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="f53d5-150">Save the profile.</span></span>

![[デバッグ] タブが選択された、プロジェクト プロパティのウィンドウ。](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="f53d5-155">また、起動プロファイルを手動で追加、 [launchSettings.json](http://json.schemastore.org/launchsettings)アプリ内のファイル。</span><span class="sxs-lookup"><span data-stu-id="f53d5-155">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="f53d5-156">プロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="f53d5-156">Run the project</span></span>

<span data-ttu-id="f53d5-157">VS UI で、実行ボタンに設定、 **IIS**をプロファイルし、アプリを起動するボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="f53d5-157">In the VS UI, set the Run button to the **IIS** profile and select the button to start the app:</span></span>

!["IIS"プロファイルへの設定、VS ツールバーのボタンを実行します。](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="f53d5-159">Visual Studio は、管理者として実行されていない場合、再起動を要求があります。</span><span class="sxs-lookup"><span data-stu-id="f53d5-159">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="f53d5-160">その場合は、Visual Studio を再起動します。</span><span class="sxs-lookup"><span data-stu-id="f53d5-160">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="f53d5-161">信頼されていない開発の証明書を使用すると、ブラウザーには、信頼されていない証明書の例外を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f53d5-161">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f53d5-162">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="f53d5-162">Additional resources</span></span>

* [<span data-ttu-id="f53d5-163">IIS を使用した Windows での ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="f53d5-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="f53d5-164">ASP.NET Core モジュールの概要</span><span class="sxs-lookup"><span data-stu-id="f53d5-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="f53d5-165">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="f53d5-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="f53d5-166">HTTPS の適用</span><span class="sxs-lookup"><span data-stu-id="f53d5-166">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
