---
title: ASP.NET Core Blazor のホストと展開
author: guardrex
description: Blazor アプリをホストおよびデプロイする方法を説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 8a5ac5c58e7ceab07e55da8b61ebb01f7ac984bc
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/17/2019
ms.locfileid: "67153196"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a><span data-ttu-id="57b4f-103">ASP.NET Core Blazor のホストと展開</span><span class="sxs-lookup"><span data-stu-id="57b4f-103">Host and deploy ASP.NET Core Blazor</span></span>

<span data-ttu-id="57b4f-104">作成者: [Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com)、[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="57b4f-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="57b4f-105">アプリの発行</span><span class="sxs-lookup"><span data-stu-id="57b4f-105">Publish the app</span></span>

<span data-ttu-id="57b4f-106">アプリは、リリース構成での展開のために発行されます。</span><span class="sxs-lookup"><span data-stu-id="57b4f-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="57b4f-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="57b4f-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="57b4f-108">**[ビルド]**  >  **[Publish {APPLICATION}]\({アプリケーション} を発行する\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="57b4f-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="57b4f-109">*[publish target]\(発行先\)* を選択します。</span><span class="sxs-lookup"><span data-stu-id="57b4f-109">Select the *publish target*.</span></span> <span data-ttu-id="57b4f-110">ローカルに発行するには、 **[フォルダー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="57b4f-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="57b4f-111">**[フォルダーの選択]** フィールド内で既定の場所を受け入れるか、または別の場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="57b4f-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="57b4f-112">**[発行]** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="57b4f-112">Select the **Publish** button.</span></span>

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="57b4f-113">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="57b4f-113">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="57b4f-114">[dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドを使用して、リリース構成によってアプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="57b4f-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```console
dotnet publish -c Release
```

---

<span data-ttu-id="57b4f-115">アプリを発行すると、プロジェクトの依存関係の[復元](/dotnet/core/tools/dotnet-restore)がトリガーされ、展開されるアセットを作成する前にプロジェクトが[ビルド](/dotnet/core/tools/dotnet-build)されます。</span><span class="sxs-lookup"><span data-stu-id="57b4f-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="57b4f-116">ビルド プロセスの一環として、アプリのダウンロード サイズを縮小し読み込み時間を短縮するため、未使用のメソッドおよびアセンブリが削除されます。</span><span class="sxs-lookup"><span data-stu-id="57b4f-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="57b4f-117">Blazor クライアント側アプリは、" */bin/Release/{ターゲット フレームワーク}/publish/{アセンブリ名}/dist*" フォルダーに発行されます。</span><span class="sxs-lookup"><span data-stu-id="57b4f-117">A Blazor client-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span> <span data-ttu-id="57b4f-118">Blazor サーバー側アプリは、 */bin/Release/{TARGET FRAMEWORK}/publish* フォルダーに発行されます。</span><span class="sxs-lookup"><span data-stu-id="57b4f-118">A Blazor server-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="57b4f-119">フォルダー内のアセットは、Web サーバーに展開されます。</span><span class="sxs-lookup"><span data-stu-id="57b4f-119">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="57b4f-120">展開のプロセスが手動であるか自動であるかは、ご使用の展開ツールによって異なります。</span><span class="sxs-lookup"><span data-stu-id="57b4f-120">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="57b4f-121">配置</span><span class="sxs-lookup"><span data-stu-id="57b4f-121">Deployment</span></span>

<span data-ttu-id="57b4f-122">展開のガイダンスについては、次のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="57b4f-122">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>

## <a name="blazor-serverless-hosting-with-azure-storage"></a><span data-ttu-id="57b4f-123">Azure Storage を使った Blazor サーバーレス ホスティング</span><span class="sxs-lookup"><span data-stu-id="57b4f-123">Blazor serverless hosting with Azure Storage</span></span>

<span data-ttu-id="57b4f-124">クライアント側 Blazor アプリは、ストレージ コンテナーから直接静的コンテンツとして、[Azure Storage](https://azure.microsoft.com/services/storage/) からサービスを提供できます。</span><span class="sxs-lookup"><span data-stu-id="57b4f-124">Blazor client-side apps can be served from [Azure Storage](https://azure.microsoft.com/services/storage/) as static content directly from a storage container.</span></span>

<span data-ttu-id="57b4f-125">詳細については、[クライアント側 ASP.NET Core Blazor のホストと展開 (スタンドアロン展開):Azure Storage](xref:host-and-deploy/blazor/client-side#azure-storage) に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="57b4f-125">For more information, see [Host and deploy ASP.NET Core Blazor client-side (Standalone deployment): Azure Storage](xref:host-and-deploy/blazor/client-side#azure-storage).</span></span>
