---
title: ASP.NET Core のディレクトリ構造
author: guardrex
description: 発行された ASP.NET Core アプリのディレクトリ構造について説明します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: a5cc1f23d624643facddc9e2006fb246e5ae66dc
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
ms.locfileid: "33838437"
---
# <a name="aspnet-core-directory-structure"></a><span data-ttu-id="e9f0b-103">ASP.NET Core のディレクトリ構造</span><span class="sxs-lookup"><span data-stu-id="e9f0b-103">ASP.NET Core directory structure</span></span>

<span data-ttu-id="e9f0b-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e9f0b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e9f0b-105">ASP.NET Core では、公開されるアプリケーション ディレクトリ *publish* は、アプリケーション ファイル、構成ファイル、静的資産、パッケージ、およびランタイムで構成されます ([自己完結型の展開](/dotnet/core/deploying/#self-contained-deployments-scd)の場合)。</span><span class="sxs-lookup"><span data-stu-id="e9f0b-105">In ASP.NET Core, the published application directory, *publish*, is comprised of application files, config files, static assets, packages, and the runtime (for [self-contained deployments](/dotnet/core/deploying/#self-contained-deployments-scd)).</span></span>


| <span data-ttu-id="e9f0b-106">アプリの種類</span><span class="sxs-lookup"><span data-stu-id="e9f0b-106">App Type</span></span> | <span data-ttu-id="e9f0b-107">ディレクトリの構造</span><span class="sxs-lookup"><span data-stu-id="e9f0b-107">Directory Structure</span></span> |
| -------- | ------------------- |
| [<span data-ttu-id="e9f0b-108">フレームワークに依存する展開</span><span class="sxs-lookup"><span data-stu-id="e9f0b-108">Framework-dependent Deployment</span></span>](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li><span data-ttu-id="e9f0b-109">publish&dagger;</span><span class="sxs-lookup"><span data-stu-id="e9f0b-109">publish&dagger;</span></span><ul><li><span data-ttu-id="e9f0b-110">logs&dagger; (stdout ログを受け取るために必要な場合を除きオプション)</span><span class="sxs-lookup"><span data-stu-id="e9f0b-110">logs&dagger; (optional unless required to receive stdout logs)</span></span></li><li><span data-ttu-id="e9f0b-111">Views&dagger; (MVC アプリ、ビューがプリコンパイルされていない場合)</span><span class="sxs-lookup"><span data-stu-id="e9f0b-111">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="e9f0b-112">Pages&dagger; (MVC または Razor ページ アプリ、ページがプリコンパイルされていない場合)</span><span class="sxs-lookup"><span data-stu-id="e9f0b-112">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="e9f0b-113">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="e9f0b-113">wwwroot&dagger;</span></span></li><li><span data-ttu-id="e9f0b-114">\*\.dll ファイル</span><span class="sxs-lookup"><span data-stu-id="e9f0b-114">\*\.dll files</span></span></li><li><span data-ttu-id="e9f0b-115">\<アセンブリ名>.deps.json</span><span class="sxs-lookup"><span data-stu-id="e9f0b-115">\<assembly-name>.deps.json</span></span></li><li><span data-ttu-id="e9f0b-116">\<アセンブリ名>.dll</span><span class="sxs-lookup"><span data-stu-id="e9f0b-116">\<assembly-name>.dll</span></span></li><li><span data-ttu-id="e9f0b-117">\<アセンブリ名>.pdb</span><span class="sxs-lookup"><span data-stu-id="e9f0b-117">\<assembly-name>.pdb</span></span></li><li><span data-ttu-id="e9f0b-118">\<アセンブリ名>.PrecompiledViews.dll</span><span class="sxs-lookup"><span data-stu-id="e9f0b-118">\<assembly-name>.PrecompiledViews.dll</span></span></li><li><span data-ttu-id="e9f0b-119">\<アセンブリ名>.PrecompiledViews.pdb</span><span class="sxs-lookup"><span data-stu-id="e9f0b-119">\<assembly-name>.PrecompiledViews.pdb</span></span></li><li><span data-ttu-id="e9f0b-120">\<アセンブリ名>.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="e9f0b-120">\<assembly-name>.runtimeconfig.json</span></span></li><li><span data-ttu-id="e9f0b-121">web.config (IIS 展開)</span><span class="sxs-lookup"><span data-stu-id="e9f0b-121">web.config (IIS deployments)</span></span></li></ul></li></ul> |
| [<span data-ttu-id="e9f0b-122">自己完結型の展開</span><span class="sxs-lookup"><span data-stu-id="e9f0b-122">Self-contained Deployment</span></span>](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li><span data-ttu-id="e9f0b-123">publish&dagger;</span><span class="sxs-lookup"><span data-stu-id="e9f0b-123">publish&dagger;</span></span><ul><li><span data-ttu-id="e9f0b-124">logs&dagger; (stdout ログを受け取るために必要な場合を除きオプション)</span><span class="sxs-lookup"><span data-stu-id="e9f0b-124">logs&dagger; (optional unless required to receive stdout logs)</span></span></li><li><span data-ttu-id="e9f0b-125">refs&dagger;</span><span class="sxs-lookup"><span data-stu-id="e9f0b-125">refs&dagger;</span></span></li><li><span data-ttu-id="e9f0b-126">Views&dagger; (MVC アプリ、ビューがプリコンパイルされていない場合)</span><span class="sxs-lookup"><span data-stu-id="e9f0b-126">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="e9f0b-127">Pages&dagger; (MVC または Razor ページ アプリ、ページがプリコンパイルされていない場合)</span><span class="sxs-lookup"><span data-stu-id="e9f0b-127">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="e9f0b-128">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="e9f0b-128">wwwroot&dagger;</span></span></li><li><span data-ttu-id="e9f0b-129">\*.dll ファイル</span><span class="sxs-lookup"><span data-stu-id="e9f0b-129">\*.dll files</span></span></li><li><span data-ttu-id="e9f0b-130">\<アセンブリ名>.deps.json</span><span class="sxs-lookup"><span data-stu-id="e9f0b-130">\<assembly-name>.deps.json</span></span></li><li><span data-ttu-id="e9f0b-131">\<アセンブリ名>.exe</span><span class="sxs-lookup"><span data-stu-id="e9f0b-131">\<assembly-name>.exe</span></span></li><li><span data-ttu-id="e9f0b-132">\<アセンブリ名>.pdb</span><span class="sxs-lookup"><span data-stu-id="e9f0b-132">\<assembly-name>.pdb</span></span></li><li><span data-ttu-id="e9f0b-133">\<アセンブリ名>.PrecompiledViews.dll</span><span class="sxs-lookup"><span data-stu-id="e9f0b-133">\<assembly-name>.PrecompiledViews.dll</span></span></li><li><span data-ttu-id="e9f0b-134">\<アセンブリ名>.PrecompiledViews.pdb</span><span class="sxs-lookup"><span data-stu-id="e9f0b-134">\<assembly-name>.PrecompiledViews.pdb</span></span></li><li><span data-ttu-id="e9f0b-135">\<アセンブリ名>.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="e9f0b-135">\<assembly-name>.runtimeconfig.json</span></span></li><li><span data-ttu-id="e9f0b-136">web.config (IIS 展開)</span><span class="sxs-lookup"><span data-stu-id="e9f0b-136">web.config (IIS deployments)</span></span></li></ul></li></ul> |

<span data-ttu-id="e9f0b-137">&dagger; ディレクトリを示します</span><span class="sxs-lookup"><span data-stu-id="e9f0b-137">&dagger;Indicates a directory</span></span>

<span data-ttu-id="e9f0b-138">*publish* ディレクトリは、展開の "*コンテンツ ルート パス*" ("*アプリケーション ベース パス*" とも呼ばれます) を表します。</span><span class="sxs-lookup"><span data-stu-id="e9f0b-138">The *publish* directory represents the *content root path*, also called the *application base path*, of the deployment.</span></span> <span data-ttu-id="e9f0b-139">サーバー上で展開されたアプリの *publish* ディレクトリにどのような名前が指定されても、その場所がホストされたアプリへのサーバーの物理パスとして機能します。</span><span class="sxs-lookup"><span data-stu-id="e9f0b-139">Whatever name is given to the *publish* directory of the deployed app on the server, its location serves as the server's physical path to the hosted app.</span></span>

<span data-ttu-id="e9f0b-140">*Wwwroot* ディレクトリが存在する場合は、静的資産のみが含まれます。</span><span class="sxs-lookup"><span data-stu-id="e9f0b-140">The *wwwroot* directory, if present, only contains static assets.</span></span>

<span data-ttu-id="e9f0b-141">stdout の *logs* ディレクトリは、次の 2 つの方法のいずれかを使って展開用に作成できます。</span><span class="sxs-lookup"><span data-stu-id="e9f0b-141">The stdout *logs* directory can be created for the deployment using one of the following two approaches:</span></span>

* <span data-ttu-id="e9f0b-142">プロジェクト ファイルに次の `<Target>` 要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="e9f0b-142">Add the following `<Target>` element to the project file:</span></span>

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="Publish">
     <MakeDir Directories="$(PublishDir)Logs" 
              Condition="!Exists('$(PublishDir)Logs')" />
     <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                       Lines="Generated file" 
                       Overwrite="True" 
                       Condition="!Exists('$(PublishDir)Logs\.log')" />
   </Target>
   ```

   <span data-ttu-id="e9f0b-143">`<MakeDir>` 要素は、公開される出力に空の *Logs* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="e9f0b-143">The `<MakeDir>` element creates an empty *Logs* folder in the published output.</span></span> <span data-ttu-id="e9f0b-144">この要素は、`PublishDir` プロパティを使って、フォルダーを作成するためのターゲットの場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="e9f0b-144">The element uses the `PublishDir` property to determine the target location for creating the folder.</span></span> <span data-ttu-id="e9f0b-145">Web 配置などの複数の展開方法は、展開の間に空のフォルダーをスキップします。</span><span class="sxs-lookup"><span data-stu-id="e9f0b-145">Several deployment methods, such as Web Deploy, skip empty folders during deployment.</span></span> <span data-ttu-id="e9f0b-146">`<WriteLinesToFile>` 要素は *Logs* フォルダーにファイルを生成します。これは、サーバーへのフォルダーの展開を保証します。</span><span class="sxs-lookup"><span data-stu-id="e9f0b-146">The `<WriteLinesToFile>` element generates a file in the *Logs* folder, which guarantees deployment of the folder to the server.</span></span> <span data-ttu-id="e9f0b-147">ワーカー プロセスにターゲット フォルダーへの書き込みアクセス許可がない場合、フォルダーの作成が失敗する可能性があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e9f0b-147">Note that folder creation may still fail if the worker process doesn't have write access to the target folder.</span></span>

* <span data-ttu-id="e9f0b-148">展開内のサーバー上に *Logs* ディレクトリを物理的に作成します。</span><span class="sxs-lookup"><span data-stu-id="e9f0b-148">Physically create the *Logs* directory on the server in the deployment.</span></span>

<span data-ttu-id="e9f0b-149">展開ディレクトリには、読み取り/実行アクセス許可が必要です。</span><span class="sxs-lookup"><span data-stu-id="e9f0b-149">The deployment directory requires Read/Execute permissions.</span></span> <span data-ttu-id="e9f0b-150">*Logs* ディレクトリには、読み取り/書き込みアクセス許可が必要です。</span><span class="sxs-lookup"><span data-stu-id="e9f0b-150">The *Logs* directory requires Read/Write permissions.</span></span> <span data-ttu-id="e9f0b-151">ファイルが書き込まれる追加のディレクトリには、読み取り/書き込みアクセス許可が必要です。</span><span class="sxs-lookup"><span data-stu-id="e9f0b-151">Additional directories where files are written require Read/Write permissions.</span></span>
