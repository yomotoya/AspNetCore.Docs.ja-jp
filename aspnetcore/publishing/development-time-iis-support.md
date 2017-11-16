---
title: "Visual Studio for ASP.NET Core の開発時 IIS サポート"
author: shirhatti
description: "ASP.NET Core アプリケーションが Windows Server の IIS の背後で実行されている場合に、そのデバッグのサポートを検出します。"
keywords: "ASP.NET Core,インターネット インフォメーション サービス,iis,windows server,asp.net core モジュール,デバッグ"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.assetid: 83d98477-9d10-4a78-a54a-f325ad67d13b
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/development-time-iis-support
ms.openlocfilehash: a35a6fd9896c4c110d1b6680b6aaf718d29a18ab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="ea635-104">Visual Studio for ASP.NET Core の開発時 IIS サポート</span><span class="sxs-lookup"><span data-stu-id="ea635-104">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="ea635-105">[Sourabh Shirhatti](https://twitter.com/sshirhatti) による投稿</span><span class="sxs-lookup"><span data-stu-id="ea635-105">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="ea635-106">この記事では、 Windows Server の IIS の背後で実行されている ASP.NET Core アプリケーションをデバッグするための、[Visual Studio](https://www.visualstudio.com/vs/) のサポートについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ea635-106">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="ea635-107">このトピックでは、手順に従ってこの機能を有効にし、プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="ea635-107">This topic walks you through enabling this feature and setting up your project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ea635-108">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="ea635-108">Prerequisites</span></span>

* <span data-ttu-id="ea635-109">Visual Studio (2017/バージョン 15.3 以降)</span><span class="sxs-lookup"><span data-stu-id="ea635-109">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="ea635-110">ASP.NET および Web 開発ワークロード*または* .NET Core クロスプラットフォームの開発ワークロード</span><span class="sxs-lookup"><span data-stu-id="ea635-110">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="ea635-111">IIS を有効にする</span><span class="sxs-lookup"><span data-stu-id="ea635-111">Enable IIS</span></span>

<span data-ttu-id="ea635-112">システムで IIS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="ea635-112">Enable IIS on your system.</span></span> <span data-ttu-id="ea635-113">**[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。</span><span class="sxs-lookup"><span data-stu-id="ea635-113">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="ea635-114">**[インターネット インフォメーション サービス]**チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="ea635-114">Select the **Internet Information Services** checkbox.</span></span>

![[インターネット インフォメーション サービス] チェックボックスが黒い四角 (チェックマークではない) で選択され、一部の IIS 機能が有効であることが表示されている [Windows の機能]](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="ea635-116">IIS のインストールで再起動が必要な場合は、システムを再起動します。</span><span class="sxs-lookup"><span data-stu-id="ea635-116">If your IIS installation requires a reboot, reboot your system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="ea635-117">開発時 IIS サポートを有効にする</span><span class="sxs-lookup"><span data-stu-id="ea635-117">Enable development-time IIS support</span></span>

<span data-ttu-id="ea635-118">IIS をインストールしたら、Visual Studio インストーラーを起動して既存の Visual Studio のインストールを変更します。</span><span class="sxs-lookup"><span data-stu-id="ea635-118">Once you've installed IIS, launch the Visual Studio installer to modify your existing Visual Studio installation.</span></span> <span data-ttu-id="ea635-119">インストーラーで、**[開発時 IIS サポート]** コンポーネントを選択します。</span><span class="sxs-lookup"><span data-stu-id="ea635-119">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="ea635-120">**[ASP.NET と Web 開発]** ワークロードの **[概要]** パネルに、このコンポーネントが省略可能なコンポーネントとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="ea635-120">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="ea635-121">これにより、ASP.NET Core アプリケーションの実行に必要なネイティブの IIS モジュールである [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="ea635-121">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Visual Studio の機能の変更: [ワークロード] タブが選択されています。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="ea635-125">プロジェクトを構成する</span><span class="sxs-lookup"><span data-stu-id="ea635-125">Configure the project</span></span>

<span data-ttu-id="ea635-126">新しい起動プロファイルを作成して、開発時 IIS サポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="ea635-126">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="ea635-127">Visual Studio の**ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[プロパティ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ea635-127">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="ea635-128">**[デバッグ]** タブを選択します。**[起動]** ドロップダウンから **[IIS]**を選択します。</span><span class="sxs-lookup"><span data-stu-id="ea635-128">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="ea635-129">**[Launch browser]\(ブラウザーの起動\)** 機能が正しい URL で有効になっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="ea635-129">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![[デバッグ] タブが選択された、プロジェクト プロパティのウィンドウ。](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="ea635-134">または、起動プロファイルをアプリの [launchSettings.json](http://json.schemastore.org/launchsettings) ファイルに手動で追加することもできます。</span><span class="sxs-lookup"><span data-stu-id="ea635-134">Alternatively, you can manually add a launch profile to your [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

```json
{
    "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iis": {
            "applicationUrl": "http://localhost/WebApplication2",
            "sslPort": 0
        }
    },
    "profiles": {
        "IIS": {
            "commandName": "IIS",
            "launchBrowser": "true",
            "launchUrl": "http://localhost/WebApplication2",
            "environmentVariables": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    }
}
```

<span data-ttu-id="ea635-135">管理者として実行していなかった場合は、Visual Studio の再起動を求められる場合があります。</span><span class="sxs-lookup"><span data-stu-id="ea635-135">You may be prompted to restart Visual Studio if you weren't running as an administrator.</span></span> <span data-ttu-id="ea635-136">その場合は、Visual Studio を再起動します。</span><span class="sxs-lookup"><span data-stu-id="ea635-136">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="ea635-137">おめでとうございます! </span><span class="sxs-lookup"><span data-stu-id="ea635-137">Congratulations!</span></span> <span data-ttu-id="ea635-138">これで、開発時 IIS サポートのためのプロジェクトが構成されました。</span><span class="sxs-lookup"><span data-stu-id="ea635-138">At this point, your project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ea635-139">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="ea635-139">Additional resources</span></span>

* [<span data-ttu-id="ea635-140">IIS を使用した Windows での ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="ea635-140">Host ASP.NET Core on Windows with IIS</span></span>](xref:publishing/iis)
* [<span data-ttu-id="ea635-141">ASP.NET Core モジュールの概要</span><span class="sxs-lookup"><span data-stu-id="ea635-141">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="ea635-142">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="ea635-142">ASP.NET Core Module configuration reference</span></span>](xref:hosting/aspnet-core-module)
