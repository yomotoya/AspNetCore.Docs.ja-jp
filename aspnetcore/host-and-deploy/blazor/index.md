---
title: Blazor をホストおよび展開する
author: guardrex
description: Blazor アプリをホストおよびデプロイする方法を説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/23/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 0fc7643c65b93a63d7a594d35e4013eab76e9db8
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376385"
---
# <a name="host-and-deploy-blazor"></a><span data-ttu-id="1311b-103">Blazor をホストおよび展開する</span><span class="sxs-lookup"><span data-stu-id="1311b-103">Host and deploy Blazor</span></span>

<span data-ttu-id="1311b-104">作成者: [Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com)、[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="1311b-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="1311b-105">アプリの発行</span><span class="sxs-lookup"><span data-stu-id="1311b-105">Publish the app</span></span>

<span data-ttu-id="1311b-106">アプリは、リリース構成での展開のために発行されます。</span><span class="sxs-lookup"><span data-stu-id="1311b-106">Apps are published for deployment in Release configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1311b-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1311b-107">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="1311b-108">**[ビルド]**  >  **[Publish {APPLICATION}]\({アプリケーション} を発行する\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1311b-108">Select **Build** > **Publish {APPLICATION}** from the navigation bar.</span></span>
1. <span data-ttu-id="1311b-109">*[publish target]\(発行先\)* を選択します。</span><span class="sxs-lookup"><span data-stu-id="1311b-109">Select the *publish target*.</span></span> <span data-ttu-id="1311b-110">ローカルに発行するには、 **[フォルダー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1311b-110">To publish locally, select **Folder**.</span></span>
1. <span data-ttu-id="1311b-111">**[フォルダーの選択]** フィールド内で既定の場所を受け入れるか、または別の場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="1311b-111">Accept the default location in the **Choose a folder** field or specify a different location.</span></span> <span data-ttu-id="1311b-112">**[発行]** ボタンを選びます。</span><span class="sxs-lookup"><span data-stu-id="1311b-112">Select the **Publish** button.</span></span>

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="1311b-113">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1311b-113">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="1311b-114">[dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドを使用して、リリース構成によってアプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="1311b-114">Use the [dotnet publish](/dotnet/core/tools/dotnet-publish) command to publish the app with a Release configuration:</span></span>

```console
dotnet publish -c Release
```

---

<span data-ttu-id="1311b-115">アプリを発行すると、プロジェクトの依存関係の[復元](/dotnet/core/tools/dotnet-restore)がトリガーされ、展開されるアセットを作成する前にプロジェクトが[ビルド](/dotnet/core/tools/dotnet-build)されます。</span><span class="sxs-lookup"><span data-stu-id="1311b-115">Publishing the app triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="1311b-116">ビルド プロセスの一環として、アプリのダウンロード サイズを縮小し読み込み時間を短縮するため、未使用のメソッドおよびアセンブリが削除されます。</span><span class="sxs-lookup"><span data-stu-id="1311b-116">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span>

<span data-ttu-id="1311b-117">Blazor クライアント側アプリは、" */bin/Release/{ターゲット フレームワーク}/publish/{アセンブリ名}/dist*" フォルダーに発行されます。</span><span class="sxs-lookup"><span data-stu-id="1311b-117">A Blazor client-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span> <span data-ttu-id="1311b-118">Blazor サーバー側アプリは、 */bin/Release/{TARGET FRAMEWORK}/publish* フォルダーに発行されます。</span><span class="sxs-lookup"><span data-stu-id="1311b-118">A Blazor server-side app is published to the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="1311b-119">フォルダー内のアセットは、Web サーバーに展開されます。</span><span class="sxs-lookup"><span data-stu-id="1311b-119">The assets in the folder are deployed to the web server.</span></span> <span data-ttu-id="1311b-120">展開のプロセスが手動であるか自動であるかは、ご使用の展開ツールによって異なります。</span><span class="sxs-lookup"><span data-stu-id="1311b-120">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="1311b-121">配置</span><span class="sxs-lookup"><span data-stu-id="1311b-121">Deployment</span></span>

<span data-ttu-id="1311b-122">展開のガイダンスについては、次のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="1311b-122">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>

## <a name="blazor-serverless-hosting-with-azure-storage"></a><span data-ttu-id="1311b-123">Azure Storage を使った Blazor サーバーレス ホスティング</span><span class="sxs-lookup"><span data-stu-id="1311b-123">Blazor serverless hosting with Azure Storage</span></span>

<span data-ttu-id="1311b-124">クライアント側 Blazor アプリは、ストレージ コンテナーから直接静的コンテンツとして、[Azure Storage](https://azure.microsoft.com/services/storage/) からサービスを提供できます。</span><span class="sxs-lookup"><span data-stu-id="1311b-124">Blazor client-side apps can be served from [Azure Storage](https://azure.microsoft.com/services/storage/) as static content directly from a storage container.</span></span>

<span data-ttu-id="1311b-125">詳細については、[クライアント側 Blazor のホストと展開 (スタンドアロン展開):Azure Storage](xref:host-and-deploy/blazor/client-side#azure-storage) に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="1311b-125">For more information, see [Host and deploy Blazor client-side (Standalone deployment): Azure Storage](xref:host-and-deploy/blazor/client-side#azure-storage).</span></span>
