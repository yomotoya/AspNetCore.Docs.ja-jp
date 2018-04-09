---
title: Visual Studio for ASP.NET Core の開発時 IIS サポート
author: shirhatti
description: Windows Server 上の IIS の背後にある実行時に ASP.NET Core アプリケーションのデバッグのサポートを検出します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 218bb2653b92cd7b1cf2c6726b2d4bedbf307a62
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="0300a-103">Visual Studio for ASP.NET Core の開発時 IIS サポート</span><span class="sxs-lookup"><span data-stu-id="0300a-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="0300a-104">[Sourabh Shirhatti](https://twitter.com/sshirhatti) による投稿</span><span class="sxs-lookup"><span data-stu-id="0300a-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="0300a-105">この記事の内容について説明します[Visual Studio](https://www.visualstudio.com/vs/) Windows サーバーで IIS 内側で実行される ASP.NET Core アプリケーションのデバッグをサポートします。</span><span class="sxs-lookup"><span data-stu-id="0300a-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="0300a-106">このトピックでは、この機能を有効にして、プロジェクトの設定を処理します。</span><span class="sxs-lookup"><span data-stu-id="0300a-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0300a-107">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="0300a-107">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="0300a-108">IIS を有効にする</span><span class="sxs-lookup"><span data-stu-id="0300a-108">Enable IIS</span></span>

<span data-ttu-id="0300a-109">IIS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="0300a-109">Enable IIS.</span></span> <span data-ttu-id="0300a-110">**[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。</span><span class="sxs-lookup"><span data-stu-id="0300a-110">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="0300a-111">**[インターネット インフォメーション サービス]**チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="0300a-111">Select the **Internet Information Services** checkbox.</span></span>

![[インターネット インフォメーション サービス] チェックボックスが黒い四角 (チェックマークではない) で選択され、一部の IIS 機能が有効であることが表示されている [Windows の機能]](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="0300a-113">IIS のインストールに再起動が必要な場合は、システムを再起動します。</span><span class="sxs-lookup"><span data-stu-id="0300a-113">If the IIS installation requires a restart, restart the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="0300a-114">開発時 IIS サポートを有効にする</span><span class="sxs-lookup"><span data-stu-id="0300a-114">Enable development-time IIS support</span></span>

<span data-ttu-id="0300a-115">Visual Studio インストーラーを起動します。</span><span class="sxs-lookup"><span data-stu-id="0300a-115">Launch the Visual Studio installer.</span></span> <span data-ttu-id="0300a-116">選択、 **IIS サポート開発時間**コンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="0300a-116">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="0300a-117">コンポーネントが一覧表示でオプションとして、**概要**パネル、 **ASP.NET および web 開発**ワークロード。</span><span class="sxs-lookup"><span data-stu-id="0300a-117">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="0300a-118">これにより、インストール、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)、ASP.NET Core アプリケーションの実行に必要なネイティブの IIS モジュールがあります。</span><span class="sxs-lookup"><span data-stu-id="0300a-118">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps.</span></span>

![Visual Studio の機能の変更: [ワークロード] タブが選択されています。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="0300a-122">プロジェクトを構成する</span><span class="sxs-lookup"><span data-stu-id="0300a-122">Configure the project</span></span>

<span data-ttu-id="0300a-123">新しい起動プロファイルを作成して、開発時 IIS サポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="0300a-123">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="0300a-124">Visual Studio の**ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[プロパティ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0300a-124">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="0300a-125">**[デバッグ]** タブを選択します。**[起動]** ドロップダウンから **[IIS]**を選択します。</span><span class="sxs-lookup"><span data-stu-id="0300a-125">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="0300a-126">**[Launch browser]\(ブラウザーの起動\)** 機能が正しい URL で有効になっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="0300a-126">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![[デバッグ] タブが選択された、プロジェクト プロパティのウィンドウ。](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="0300a-131">また、起動プロファイルを手動で追加、 [launchSettings.json](http://json.schemastore.org/launchsettings)アプリ内のファイル。</span><span class="sxs-lookup"><span data-stu-id="0300a-131">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="0300a-132">Visual Studio は、管理者として実行されていない場合、再起動を要求があります。</span><span class="sxs-lookup"><span data-stu-id="0300a-132">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="0300a-133">その場合は、Visual Studio を再起動します。</span><span class="sxs-lookup"><span data-stu-id="0300a-133">If prompted, restart Visual Studio.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0300a-134">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="0300a-134">Additional resources</span></span>

* [<span data-ttu-id="0300a-135">IIS を使用した Windows での ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="0300a-135">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="0300a-136">ASP.NET Core モジュールの概要</span><span class="sxs-lookup"><span data-stu-id="0300a-136">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="0300a-137">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="0300a-137">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
