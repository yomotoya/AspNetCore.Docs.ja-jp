---
title: "Visual Studio for ASP.NET Core の開発時 IIS サポート"
author: shirhatti
description: "ASP.NET Core アプリケーションが Windows Server の IIS の背後で実行されている場合に、そのデバッグのサポートを検出します。"
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 264be49e8aff72f913c22508150e424541d49fa0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="a82af-103">Visual Studio for ASP.NET Core の開発時 IIS サポート</span><span class="sxs-lookup"><span data-stu-id="a82af-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="a82af-104">[Sourabh Shirhatti](https://twitter.com/sshirhatti) による投稿</span><span class="sxs-lookup"><span data-stu-id="a82af-104">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="a82af-105">この記事では、 Windows Server の IIS の背後で実行されている ASP.NET Core アプリケーションをデバッグするための、[Visual Studio](https://www.visualstudio.com/vs/) のサポートについて説明します。</span><span class="sxs-lookup"><span data-stu-id="a82af-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="a82af-106">このトピックでは、この機能を有効にして、プロジェクトの設定を処理します。</span><span class="sxs-lookup"><span data-stu-id="a82af-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a82af-107">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="a82af-107">Prerequisites</span></span>

* <span data-ttu-id="a82af-108">Visual Studio (2017/バージョン 15.3 以降)</span><span class="sxs-lookup"><span data-stu-id="a82af-108">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="a82af-109">ASP.NET および Web 開発ワークロード*または* .NET Core クロスプラットフォームの開発ワークロード</span><span class="sxs-lookup"><span data-stu-id="a82af-109">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="a82af-110">IIS を有効にする</span><span class="sxs-lookup"><span data-stu-id="a82af-110">Enable IIS</span></span>

<span data-ttu-id="a82af-111">IIS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="a82af-111">Enable IIS.</span></span> <span data-ttu-id="a82af-112">**[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。</span><span class="sxs-lookup"><span data-stu-id="a82af-112">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="a82af-113">**[インターネット インフォメーション サービス]**チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="a82af-113">Select the **Internet Information Services** checkbox.</span></span>

![[インターネット インフォメーション サービス] チェックボックスが黒い四角 (チェックマークではない) で選択され、一部の IIS 機能が有効であることが表示されている [Windows の機能]](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="a82af-115">IIS のインストールには、再起動が必要とする場合は、システムを再起動します。</span><span class="sxs-lookup"><span data-stu-id="a82af-115">If the IIS installation requires a reboot, reboot the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="a82af-116">開発時 IIS サポートを有効にする</span><span class="sxs-lookup"><span data-stu-id="a82af-116">Enable development-time IIS support</span></span>

<span data-ttu-id="a82af-117">IIS がインストールされると、既存の Visual Studio インストールを変更する Visual Studio インストーラーを起動します。</span><span class="sxs-lookup"><span data-stu-id="a82af-117">Once IIS is installed, launch the Visual Studio installer to modify the existing Visual Studio installation.</span></span> <span data-ttu-id="a82af-118">インストーラーで、**[開発時 IIS サポート]** コンポーネントを選択します。</span><span class="sxs-lookup"><span data-stu-id="a82af-118">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="a82af-119">**[ASP.NET と Web 開発]** ワークロードの **[概要]** パネルに、このコンポーネントが省略可能なコンポーネントとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="a82af-119">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="a82af-120">これにより、ASP.NET Core アプリケーションの実行に必要なネイティブの IIS モジュールである [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="a82af-120">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![Visual Studio の機能の変更: [ワークロード] タブが選択されています。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="a82af-124">プロジェクトを構成する</span><span class="sxs-lookup"><span data-stu-id="a82af-124">Configure the project</span></span>

<span data-ttu-id="a82af-125">新しい起動プロファイルを作成して、開発時 IIS サポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="a82af-125">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="a82af-126">Visual Studio の**ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[プロパティ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a82af-126">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="a82af-127">**[デバッグ]** タブを選択します。**[起動]** ドロップダウンから **[IIS]**を選択します。</span><span class="sxs-lookup"><span data-stu-id="a82af-127">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="a82af-128">**[Launch browser]\(ブラウザーの起動\)** 機能が正しい URL で有効になっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="a82af-128">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![[デバッグ] タブが選択された、プロジェクト プロパティのウィンドウ。](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="a82af-133">また、起動プロファイルを手動で追加、 [launchSettings.json](http://json.schemastore.org/launchsettings)アプリ内のファイル。</span><span class="sxs-lookup"><span data-stu-id="a82af-133">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="a82af-134">Visual Studio は、管理者として実行されていない場合、再起動を要求があります。</span><span class="sxs-lookup"><span data-stu-id="a82af-134">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="a82af-135">その場合は、Visual Studio を再起動します。</span><span class="sxs-lookup"><span data-stu-id="a82af-135">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="a82af-136">おめでとうございます! </span><span class="sxs-lookup"><span data-stu-id="a82af-136">Congratulations!</span></span> <span data-ttu-id="a82af-137">この時点では、プロジェクトでは、開発時の IIS のサポートのためには構成されます。</span><span class="sxs-lookup"><span data-stu-id="a82af-137">At this point, the project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a82af-138">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="a82af-138">Additional resources</span></span>

* [<span data-ttu-id="a82af-139">IIS を使用した Windows での ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="a82af-139">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="a82af-140">ASP.NET Core モジュールの概要</span><span class="sxs-lookup"><span data-stu-id="a82af-140">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="a82af-141">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="a82af-141">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
