---
title: ASP.NET Core ディレクトリ構造
author: guardrex
description: 発行された ASP.NET Core アプリケーションのディレクトリ構造について説明します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: ac9b777bcc7f4a8634161fc1347a4d0fdc3b4784
ms.sourcegitcommit: 7c8fd9b7445cd77eb7f7d774bfd120c26f3b5d84
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/19/2018
---
# <a name="aspnet-core-directory-structure"></a><span data-ttu-id="45127-103">ASP.NET Core ディレクトリ構造</span><span class="sxs-lookup"><span data-stu-id="45127-103">ASP.NET Core directory structure</span></span>

<span data-ttu-id="45127-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="45127-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="45127-105">ASP.NET Core、公開されたアプリケーション ディレクトリで*発行*、アプリケーション ファイル、構成ファイル、静的なアセット、パッケージ、およびランタイムで構成されます (の[自己完結型の展開](/dotnet/core/deploying/#self-contained-deployments-scd))。</span><span class="sxs-lookup"><span data-stu-id="45127-105">In ASP.NET Core, the published application directory, *publish*, is comprised of application files, config files, static assets, packages, and the runtime (for [self-contained deployments](/dotnet/core/deploying/#self-contained-deployments-scd)).</span></span>


| <span data-ttu-id="45127-106">アプリの種類</span><span class="sxs-lookup"><span data-stu-id="45127-106">App Type</span></span> | <span data-ttu-id="45127-107">ディレクトリ構造</span><span class="sxs-lookup"><span data-stu-id="45127-107">Directory Structure</span></span> |
| -------- | ------------------- |
| [<span data-ttu-id="45127-108">フレームワークに依存する展開</span><span class="sxs-lookup"><span data-stu-id="45127-108">Framework-dependent Deployment</span></span>](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li><span data-ttu-id="45127-109">発行&dagger;</span><span class="sxs-lookup"><span data-stu-id="45127-109">publish&dagger;</span></span><ul><li><span data-ttu-id="45127-110">ログ&dagger;(標準出力ログを受信するために必要な場合を除き、省略可能)</span><span class="sxs-lookup"><span data-stu-id="45127-110">logs&dagger; (optional unless required to receive stdout logs)</span></span></li><li><span data-ttu-id="45127-111">ビュー&dagger; (MVC アプリ; ビューをプリコンパイルいない場合)</span><span class="sxs-lookup"><span data-stu-id="45127-111">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="45127-112">ページ&dagger;(MVC または Razor ページ アプリですページをプリコンパイルいない場合)。</span><span class="sxs-lookup"><span data-stu-id="45127-112">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="45127-113">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="45127-113">wwwroot&dagger;</span></span></li><li><span data-ttu-id="45127-114">\*\.dll ファイル</span><span class="sxs-lookup"><span data-stu-id="45127-114">\*\.dll files</span></span></li><li><span data-ttu-id="45127-115">\<アセンブリ名 >. deps.json</span><span class="sxs-lookup"><span data-stu-id="45127-115">\<assembly-name>.deps.json</span></span></li><li><span data-ttu-id="45127-116">\<アセンブリ名 > .dll</span><span class="sxs-lookup"><span data-stu-id="45127-116">\<assembly-name>.dll</span></span></li><li><span data-ttu-id="45127-117">\<アセンブリ名 > .pdb</span><span class="sxs-lookup"><span data-stu-id="45127-117">\<assembly-name>.pdb</span></span></li><li><span data-ttu-id="45127-118">\<アセンブリ名 >。PrecompiledViews.dll</span><span class="sxs-lookup"><span data-stu-id="45127-118">\<assembly-name>.PrecompiledViews.dll</span></span></li><li><span data-ttu-id="45127-119">\<アセンブリ名 >。PrecompiledViews.pdb</span><span class="sxs-lookup"><span data-stu-id="45127-119">\<assembly-name>.PrecompiledViews.pdb</span></span></li><li><span data-ttu-id="45127-120">\<アセンブリ名 >. runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="45127-120">\<assembly-name>.runtimeconfig.json</span></span></li><li><span data-ttu-id="45127-121">web.config (IIS 展開)</span><span class="sxs-lookup"><span data-stu-id="45127-121">web.config (IIS deployments)</span></span></li></ul></li></ul> |
| [<span data-ttu-id="45127-122">自己完結型の配置</span><span class="sxs-lookup"><span data-stu-id="45127-122">Self-contained Deployment</span></span>](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li><span data-ttu-id="45127-123">発行&dagger;</span><span class="sxs-lookup"><span data-stu-id="45127-123">publish&dagger;</span></span><ul><li><span data-ttu-id="45127-124">ログ&dagger;(標準出力ログを受信するために必要な場合を除き、省略可能)</span><span class="sxs-lookup"><span data-stu-id="45127-124">logs&dagger; (optional unless required to receive stdout logs)</span></span></li><li><span data-ttu-id="45127-125">refs&dagger;</span><span class="sxs-lookup"><span data-stu-id="45127-125">refs&dagger;</span></span></li><li><span data-ttu-id="45127-126">ビュー&dagger; (MVC アプリ; ビューをプリコンパイルいない場合)</span><span class="sxs-lookup"><span data-stu-id="45127-126">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="45127-127">ページ&dagger;(MVC または Razor ページ アプリですページをプリコンパイルいない場合)。</span><span class="sxs-lookup"><span data-stu-id="45127-127">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="45127-128">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="45127-128">wwwroot&dagger;</span></span></li><li><span data-ttu-id="45127-129">\*.dll ファイル</span><span class="sxs-lookup"><span data-stu-id="45127-129">\*.dll files</span></span></li><li><span data-ttu-id="45127-130">\<アセンブリ名 >. deps.json</span><span class="sxs-lookup"><span data-stu-id="45127-130">\<assembly-name>.deps.json</span></span></li><li><span data-ttu-id="45127-131">\<アセンブリ名 > .exe</span><span class="sxs-lookup"><span data-stu-id="45127-131">\<assembly-name>.exe</span></span></li><li><span data-ttu-id="45127-132">\<アセンブリ名 > .pdb</span><span class="sxs-lookup"><span data-stu-id="45127-132">\<assembly-name>.pdb</span></span></li><li><span data-ttu-id="45127-133">\<アセンブリ名 >。PrecompiledViews.dll</span><span class="sxs-lookup"><span data-stu-id="45127-133">\<assembly-name>.PrecompiledViews.dll</span></span></li><li><span data-ttu-id="45127-134">\<アセンブリ名 >。PrecompiledViews.pdb</span><span class="sxs-lookup"><span data-stu-id="45127-134">\<assembly-name>.PrecompiledViews.pdb</span></span></li><li><span data-ttu-id="45127-135">\<アセンブリ名 >. runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="45127-135">\<assembly-name>.runtimeconfig.json</span></span></li><li><span data-ttu-id="45127-136">web.config (IIS 展開)</span><span class="sxs-lookup"><span data-stu-id="45127-136">web.config (IIS deployments)</span></span></li></ul></li></ul> |

<span data-ttu-id="45127-137">&dagger;ディレクトリを示します</span><span class="sxs-lookup"><span data-stu-id="45127-137">&dagger;Indicates a directory</span></span>

<span data-ttu-id="45127-138">*発行*ディレクトリを表します、*コンテンツのルート パス*もという、*アプリケーション ベース パス*デプロイのです。</span><span class="sxs-lookup"><span data-stu-id="45127-138">The *publish* directory represents the *content root path*, also called the *application base path*, of the deployment.</span></span> <span data-ttu-id="45127-139">任意の名前が指定された、*発行*サーバーに配置されたアプリのディレクトリの場所がホスト型アプリへのサーバーの物理パスとして機能します。</span><span class="sxs-lookup"><span data-stu-id="45127-139">Whatever name is given to the *publish* directory of the deployed app on the server, its location serves as the server's physical path to the hosted app.</span></span>

<span data-ttu-id="45127-140">*Wwwroot*ディレクトリに存在する場合のみが含まれる静的な資産です。</span><span class="sxs-lookup"><span data-stu-id="45127-140">The *wwwroot* directory, if present, only contains static assets.</span></span>

<span data-ttu-id="45127-141">Stdout*ログ*ディレクトリに次の 2 つの方法のいずれかを使用して、展開を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="45127-141">The stdout *logs* directory can be created the deployment using one of the following two approaches:</span></span>

* <span data-ttu-id="45127-142">次の追加`<Target>`要素をプロジェクト ファイル。</span><span class="sxs-lookup"><span data-stu-id="45127-142">Add the following `<Target>` element to the project file:</span></span>

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

   <span data-ttu-id="45127-143">`<MakeDir>`要素は、空を作成*ログ*公開済みの出力内のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="45127-143">The `<MakeDir>` element creates an empty *Logs* folder in the published output.</span></span> <span data-ttu-id="45127-144">要素を使用して、`PublishDir`フォルダーを作成するため、ターゲットの場所を決定するプロパティです。</span><span class="sxs-lookup"><span data-stu-id="45127-144">The element uses the `PublishDir` property to determine the target location for creating the folder.</span></span> <span data-ttu-id="45127-145">Web Deploy などのいくつかの展開方法は、配置時に空のフォルダーをスキップします。</span><span class="sxs-lookup"><span data-stu-id="45127-145">Several deployment methods, such as Web Deploy, skip empty folders during deployment.</span></span> <span data-ttu-id="45127-146">`<WriteLinesToFile>`要素内のファイルを生成する、*ログ*フォルダーで、サーバー フォルダーの展開を保証します。</span><span class="sxs-lookup"><span data-stu-id="45127-146">The `<WriteLinesToFile>` element generates a file in the *Logs* folder, which guarantees deployment of the folder to the server.</span></span> <span data-ttu-id="45127-147">ワーカー プロセスは、ターゲット フォルダーに書き込みアクセス権を持っていない場合、まだフォルダーの作成が失敗することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="45127-147">Note that folder creation may still fail if the worker process doesn't have write access to the target folder.</span></span>

* <span data-ttu-id="45127-148">物理的な作成、*ログ*ディレクトリ展開内のサーバーにします。</span><span class="sxs-lookup"><span data-stu-id="45127-148">Physically create the *Logs* directory on the server in the deployment.</span></span>

<span data-ttu-id="45127-149">配置ディレクトリには、読み取り/実行アクセス許可が必要です。</span><span class="sxs-lookup"><span data-stu-id="45127-149">The deployment directory requires Read/Execute permissions.</span></span> <span data-ttu-id="45127-150">*ログ*ディレクトリが読み取り/書き込み権限が必要です。</span><span class="sxs-lookup"><span data-stu-id="45127-150">The *Logs* directory requires Read/Write permissions.</span></span> <span data-ttu-id="45127-151">ファイルが書き込まれる追加のディレクトリでは、読み取り/書き込み権限が必要です。</span><span class="sxs-lookup"><span data-stu-id="45127-151">Additional directories where files are written require Read/Write permissions.</span></span>
