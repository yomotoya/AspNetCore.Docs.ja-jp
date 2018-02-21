---
title: "Visual Studio for ASP.NET Core の開発時 IIS サポート"
author: shirhatti
description: "Windows Server 上の IIS の背後にある実行時に ASP.NET Core アプリケーションのデバッグのサポートを検出します。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: a8bdf4c0c0399c62666e6e61e70c0298a42c2c12
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="5fd91-103">Visual Studio for ASP.NET Core の開発時 IIS サポート</span><span class="sxs-lookup"><span data-stu-id="5fd91-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="5fd91-104">[Sourabh Shirhatti](https://twitter.com/sshirhatti) による投稿</span><span class="sxs-lookup"><span data-stu-id="5fd91-104">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="5fd91-105">この記事の内容について説明します[Visual Studio](https://www.visualstudio.com/vs/) Windows サーバーで IIS 内側で実行される ASP.NET Core アプリケーションのデバッグをサポートします。</span><span class="sxs-lookup"><span data-stu-id="5fd91-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="5fd91-106">このトピックでは、この機能を有効にして、プロジェクトの設定を処理します。</span><span class="sxs-lookup"><span data-stu-id="5fd91-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5fd91-107">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="5fd91-107">Prerequisites</span></span>

* <span data-ttu-id="5fd91-108">Visual Studio (2017/バージョン 15.3 以降)</span><span class="sxs-lookup"><span data-stu-id="5fd91-108">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="5fd91-109">ASP.NET および Web 開発ワークロード*または* .NET Core クロスプラットフォームの開発ワークロード</span><span class="sxs-lookup"><span data-stu-id="5fd91-109">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="5fd91-110">IIS を有効にする</span><span class="sxs-lookup"><span data-stu-id="5fd91-110">Enable IIS</span></span>

<span data-ttu-id="5fd91-111">IIS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="5fd91-111">Enable IIS.</span></span> <span data-ttu-id="5fd91-112">**[コントロール パネル]** > **[プログラム]** > **[プログラムと機能]** > **[Windows の機能の有効化または無効化]** (画面の左側) に移動します。</span><span class="sxs-lookup"><span data-stu-id="5fd91-112">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="5fd91-113">**[インターネット インフォメーション サービス]**チェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="5fd91-113">Select the **Internet Information Services** checkbox.</span></span>

![[インターネット インフォメーション サービス] チェックボックスが黒い四角 (チェックマークではない) で選択され、一部の IIS 機能が有効であることが表示されている [Windows の機能]](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="5fd91-115">IIS のインストールには、再起動が必要とする場合は、システムを再起動します。</span><span class="sxs-lookup"><span data-stu-id="5fd91-115">If the IIS installation requires a restart, restart the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="5fd91-116">開発時 IIS サポートを有効にする</span><span class="sxs-lookup"><span data-stu-id="5fd91-116">Enable development-time IIS support</span></span>

<span data-ttu-id="5fd91-117">Visual Studio インストーラーを起動します。</span><span class="sxs-lookup"><span data-stu-id="5fd91-117">Launch the Visual Studio installer.</span></span> <span data-ttu-id="5fd91-118">選択、 **IIS サポート開発時間**コンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="5fd91-118">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="5fd91-119">コンポーネントが一覧表示でオプションとして、**概要**パネル、 **ASP.NET および web 開発**ワークロード。</span><span class="sxs-lookup"><span data-stu-id="5fd91-119">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="5fd91-120">これにより、インストール、 [ASP.NET Core モジュール](xref:fundamentals/servers/aspnet-core-module)、ASP.NET Core アプリケーションの実行に必要なネイティブの IIS モジュールがあります。</span><span class="sxs-lookup"><span data-stu-id="5fd91-120">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps.</span></span>

![Visual Studio の機能の変更: [ワークロード] タブが選択されています。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="5fd91-124">プロジェクトを構成する</span><span class="sxs-lookup"><span data-stu-id="5fd91-124">Configure the project</span></span>

<span data-ttu-id="5fd91-125">新しい起動プロファイルを作成して、開発時 IIS サポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="5fd91-125">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="5fd91-126">Visual Studio の**ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[プロパティ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5fd91-126">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="5fd91-127">**[デバッグ]** タブを選択します。**[起動]** ドロップダウンから **[IIS]**を選択します。</span><span class="sxs-lookup"><span data-stu-id="5fd91-127">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="5fd91-128">**[Launch browser]\(ブラウザーの起動\)** 機能が正しい URL で有効になっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="5fd91-128">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![[デバッグ] タブが選択された、プロジェクト プロパティのウィンドウ。](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="5fd91-133">また、起動プロファイルを手動で追加、 [launchSettings.json](http://json.schemastore.org/launchsettings)アプリ内のファイル。</span><span class="sxs-lookup"><span data-stu-id="5fd91-133">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="5fd91-134">Visual Studio は、管理者として実行されていない場合、再起動を要求があります。</span><span class="sxs-lookup"><span data-stu-id="5fd91-134">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="5fd91-135">その場合は、Visual Studio を再起動します。</span><span class="sxs-lookup"><span data-stu-id="5fd91-135">If prompted, restart Visual Studio.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5fd91-136">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="5fd91-136">Additional resources</span></span>

* [<span data-ttu-id="5fd91-137">IIS を使用した Windows での ASP.NET Core のホスト</span><span class="sxs-lookup"><span data-stu-id="5fd91-137">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="5fd91-138">ASP.NET Core モジュールの概要</span><span class="sxs-lookup"><span data-stu-id="5fd91-138">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="5fd91-139">ASP.NET Core モジュール構成リファレンス</span><span class="sxs-lookup"><span data-stu-id="5fd91-139">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
