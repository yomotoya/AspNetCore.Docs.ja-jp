---
title: Blazor をホストおよび展開する
author: guardrex
description: Blazor アプリをホストおよびデプロイする方法を説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: a7739e2b240d7fd6c85e68105892c802228ebeb5
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614588"
---
# <a name="host-and-deploy-blazor"></a><span data-ttu-id="18447-103">Blazor をホストおよび展開する</span><span class="sxs-lookup"><span data-stu-id="18447-103">Host and deploy Blazor</span></span>

<span data-ttu-id="18447-104">作成者: [Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com)、[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="18447-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="18447-105">アプリの発行</span><span class="sxs-lookup"><span data-stu-id="18447-105">Publish the app</span></span>

<span data-ttu-id="18447-106">アプリは [dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドによって、リリース構成での展開のために発行されます。</span><span class="sxs-lookup"><span data-stu-id="18447-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="18447-107">`dotnet publish` コマンドの実行は、統合開発環境 (IDE) により組み込みの発行機能を使用して自動的に行われることがあります。そのため、ご使用の展開ツールによっては、このコマンドを手動でコマンド プロンプトから実行する必要がない場合があります。</span><span class="sxs-lookup"><span data-stu-id="18447-107">An integrated development environment (IDE) may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="18447-108">`dotnet publish` により、プロジェクトの依存関係の[復元](/dotnet/core/tools/dotnet-restore)がトリガーされ、展開に使用されるアセットの作成前にプロジェクトが[ビルド](/dotnet/core/tools/dotnet-build)されます。</span><span class="sxs-lookup"><span data-stu-id="18447-108">`dotnet publish` triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="18447-109">ビルド プロセスの一環として、アプリのダウンロード サイズを縮小し読み込み時間を短縮するため、未使用のメソッドおよびアセンブリが削除されます。</span><span class="sxs-lookup"><span data-stu-id="18447-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="18447-110">展開は、*/bin/Release/{ターゲット フレームワーク}/publish* フォルダーに作成されます。</span><span class="sxs-lookup"><span data-stu-id="18447-110">The deployment is created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span>

<span data-ttu-id="18447-111">*publish* フォルダー内のアセットは、Web サーバーに展開されます。</span><span class="sxs-lookup"><span data-stu-id="18447-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="18447-112">展開のプロセスが手動であるか自動であるかは、ご使用の展開ツールによって異なります。</span><span class="sxs-lookup"><span data-stu-id="18447-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="deployment"></a><span data-ttu-id="18447-113">配置</span><span class="sxs-lookup"><span data-stu-id="18447-113">Deployment</span></span>

<span data-ttu-id="18447-114">展開のガイダンスについては、次のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="18447-114">For deployment guidance, see the following topics:</span></span>

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>
