---
title: ASP.NET Core プロジェクトをトラブルシューティングします。
author: Rick-Anderson
description: 理解し、警告および ASP.NET Core プロジェクトによるエラーのトラブルシューティングを行います。
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: test/troubleshoot
ms.openlocfilehash: 8ff8ddaf4a35a02db650ff429ffbbf4e76a53ecf
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613006"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="4d506-103">ASP.NET Core プロジェクトをトラブルシューティングします。</span><span class="sxs-lookup"><span data-stu-id="4d506-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="4d506-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4d506-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4d506-105">次のリンクは、トラブルシューティングのガイダンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="4d506-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="4d506-106">Azure App Service での ASP.NET Core のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="4d506-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="4d506-107">IIS での ASP.NET Core のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="4d506-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="4d506-108">Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス</span><span class="sxs-lookup"><span data-stu-id="4d506-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="4d506-109">ASP.NET Core アプリケーションでの問題の診断 NDC 会議 (ロンドン、2018)。</span><span class="sxs-lookup"><span data-stu-id="4d506-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="4d506-110">ASP.NET ブログ: ASP.NET Core パフォーマンス問題のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="4d506-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="4d506-111">.NET core SDK 警告</span><span class="sxs-lookup"><span data-stu-id="4d506-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="4d506-112">32 ビットと 64 ビット バージョンの .NET Core SDK の両方がインストールされています。</span><span class="sxs-lookup"><span data-stu-id="4d506-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="4d506-113">**新しいプロジェクト**ダイアログ ASP.NET Core、表示される、次の警告。</span><span class="sxs-lookup"><span data-stu-id="4d506-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="4d506-114">両方 32 バージョンと 64 ビット バージョンの .NET Core SDK がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="4d506-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="4d506-115">インストールされている 64 ビット版からのみテンプレート 'c:\\Program Files\\dotnet\\sdk\\' が表示されます。</span><span class="sxs-lookup"><span data-stu-id="4d506-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![警告メッセージを示す OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="4d506-117">この警告を表示するに 32 ビット (x86) と 64 ビット (x64) バージョンの両方、 [.NET Core SDK](https://www.microsoft.com/net/download/all)がインストールされています。</span><span class="sxs-lookup"><span data-stu-id="4d506-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="4d506-118">両方のバージョンはインストールされている一般的な理由は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="4d506-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="4d506-119">もともと 32 ビット コンピューターを使用して、.NET Core SDK インストーラーのダウンロードが間でそれをコピーし、64 ビット コンピューターにインストールします。</span><span class="sxs-lookup"><span data-stu-id="4d506-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="4d506-120">別のアプリケーションによって、32 ビットの .NET Core SDK がインストールされました。</span><span class="sxs-lookup"><span data-stu-id="4d506-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="4d506-121">間違ったバージョンがダウンロードされ、インストールします。</span><span class="sxs-lookup"><span data-stu-id="4d506-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="4d506-122">この警告を回避するのには、32 ビット .NET Core SDK をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="4d506-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="4d506-123">アンインストール**コントロール パネルの** > **プログラムと機能** > **のアンインストールと変更プログラム**です。</span><span class="sxs-lookup"><span data-stu-id="4d506-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="4d506-124">警告が発生した理由とその影響を理解している場合は、警告を無視できます。</span><span class="sxs-lookup"><span data-stu-id="4d506-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="4d506-125">複数の場所では、.NET Core SDK がインストールされています。</span><span class="sxs-lookup"><span data-stu-id="4d506-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="4d506-126">**新しいプロジェクト**ダイアログ ASP.NET Core、表示される、次の警告。</span><span class="sxs-lookup"><span data-stu-id="4d506-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="4d506-127">.NET Core SDK は、複数の場所にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="4d506-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="4d506-128">インストールされている SDK(s) からテンプレートのみ 'c:\\Program Files\\dotnet\\sdk\\' が表示されます。</span><span class="sxs-lookup"><span data-stu-id="4d506-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![警告メッセージを示す OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="4d506-130">.NET Core SDK の少なくとも 1 つのインストール ディレクトリの外部にある場合にこのメッセージを参照してください*c:\\Program Files\\dotnet\\sdk\\*です。</span><span class="sxs-lookup"><span data-stu-id="4d506-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="4d506-131">通常は、.NET Core SDK は、MSI インストーラーではなくコピー/貼り付けを使用してコンピューターに配置されている場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="4d506-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="4d506-132">この警告を回避するのには、32 ビット .NET Core SDK をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="4d506-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="4d506-133">アンインストール**コントロール パネルの** > **プログラムと機能** > **のアンインストールと変更プログラム**です。</span><span class="sxs-lookup"><span data-stu-id="4d506-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="4d506-134">警告が発生した理由とその影響を理解している場合は、警告を無視できます。</span><span class="sxs-lookup"><span data-stu-id="4d506-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="4d506-135">.NET Core Sdk は検出されませんでした。</span><span class="sxs-lookup"><span data-stu-id="4d506-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="4d506-136">**新しいプロジェクト**ダイアログ ASP.NET Core、表示される、次の警告。</span><span class="sxs-lookup"><span data-stu-id="4d506-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="4d506-137">.NET Core Sdk は検出されませんでした、環境変数 'PATH' に含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="4d506-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![警告メッセージを示す OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="4d506-139">この警告を表示するには環境変数`PATH`コンピューターで、.NET Core の Sdk を指していません。</span><span class="sxs-lookup"><span data-stu-id="4d506-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="4d506-140">この問題を解決するには。</span><span class="sxs-lookup"><span data-stu-id="4d506-140">To resolve this problem:</span></span>

* <span data-ttu-id="4d506-141">インストールまたは .NET Core SDK がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="4d506-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="4d506-142">確認、`PATH`環境変数が SDK のインストール場所をポイントします。</span><span class="sxs-lookup"><span data-stu-id="4d506-142">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="4d506-143">インストーラーが通常の設定、`PATH`です。</span><span class="sxs-lookup"><span data-stu-id="4d506-143">The installer normally sets the `PATH`.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a><span data-ttu-id="4d506-144">IHtmlHelper.Partial の使い方はアプリケーション デッドロックになる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4d506-144">Use of IHtmlHelper.Partial may result in app deadlocks</span></span>

<span data-ttu-id="4d506-145">ASP.NET Core 2.1 以降では、呼び出す`Html.Partial`デッドロックの可能性があるのため、アナライザー警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="4d506-145">In ASP.NET Core 2.1 and later, calling `Html.Partial` results in an analyzer warning due to the potential for deadlocks.</span></span> <span data-ttu-id="4d506-146">警告メッセージは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="4d506-146">The warning message is:</span></span>

> <span data-ttu-id="4d506-147">IHtmlHelper.Partial を使用すると、アプリケーションにデッドロックの可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4d506-147">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="4d506-148">使用を検討して`<partial>`タグ ヘルパーまたは`IHtmlHelper.PartialAsync`です。</span><span class="sxs-lookup"><span data-stu-id="4d506-148">Consider using `<partial>` Tag Helper or `IHtmlHelper.PartialAsync`.</span></span>

<span data-ttu-id="4d506-149">呼び出す`@Html.Partial`で置き換える必要があります`@await Html.PartialAsync`または部分的なタグ ヘルパー`<partial name="_Partial" />`です。</span><span class="sxs-lookup"><span data-stu-id="4d506-149">Calls to `@Html.Partial` should be replaced by `@await Html.PartialAsync` or the partial tag helper `<partial name="_Partial" />`.</span></span>

::: moniker-end
