---
title: ASP.NET Core プロジェクトをトラブルシューティングします。
author: Rick-Anderson
description: ASP.NET Core プロジェクトでの警告とエラーについて説明し、トラブルシューティングを行います。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: test/troubleshoot
ms.openlocfilehash: 150f2192bb4b6dd0d330fd678d9c5fa0bf31673e
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090112"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="d7766-103">ASP.NET Core プロジェクトをトラブルシューティングします。</span><span class="sxs-lookup"><span data-stu-id="d7766-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="d7766-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d7766-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d7766-105">次のリンクは、トラブルシューティングの指針を提供します。</span><span class="sxs-lookup"><span data-stu-id="d7766-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="d7766-106">NDC カンファレンス (ロンドン、2018): ASP.NET Core アプリケーションで問題の診断</span><span class="sxs-lookup"><span data-stu-id="d7766-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="d7766-107">ASP.NET ブログ: ASP.NET Core のパフォーマンスの問題のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="d7766-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="d7766-108">.NET core SDK の警告</span><span class="sxs-lookup"><span data-stu-id="d7766-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="d7766-109">32 ビットと 64 ビット バージョンの .NET Core SDK の両方がインストールされています。</span><span class="sxs-lookup"><span data-stu-id="d7766-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="d7766-110">**新しいプロジェクト**ダイアログの ASP.NET Core では、次の警告を表示可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d7766-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="d7766-111">32 と 64 ビット版の両方、.NET Core SDK がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="d7766-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="d7766-112">インストールされている 64 ビット バージョンからのテンプレートのみ 'c:\\Program Files\\dotnet\\sdk\\' が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d7766-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![警告メッセージを表示する OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="d7766-114">この警告が表示されるときに 32 ビット (x86) と 64 ビット (x64) 版の両方、 [.NET Core SDK](https://www.microsoft.com/net/download/all)がインストールされています。</span><span class="sxs-lookup"><span data-stu-id="d7766-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="d7766-115">両方のバージョンがインストールされている可能性がありますが、一般的な理由は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d7766-115">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="d7766-116">最初、32 ビット コンピューターを使用して、.NET Core SDK インストーラーをダウンロードが間でコピーし、64 ビット コンピューターにインストールされていること。</span><span class="sxs-lookup"><span data-stu-id="d7766-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="d7766-117">別のアプリケーションで 32 ビットの .NET Core SDK がインストールされました。</span><span class="sxs-lookup"><span data-stu-id="d7766-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="d7766-118">間違ったバージョンがダウンロードされ、インストールされています。</span><span class="sxs-lookup"><span data-stu-id="d7766-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="d7766-119">この警告を回避するには、32 ビット .NET Core SDK をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="d7766-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="d7766-120">アンインストール**コントロール パネルの** > **プログラムと機能** > **のアンインストールと変更プログラム**です。</span><span class="sxs-lookup"><span data-stu-id="d7766-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="d7766-121">警告が発生した理由とその影響を理解している場合は、警告を無視できます。</span><span class="sxs-lookup"><span data-stu-id="d7766-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="d7766-122">複数の場所に、.NET Core SDK がインストールされています。</span><span class="sxs-lookup"><span data-stu-id="d7766-122">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="d7766-123">**新しいプロジェクト**ダイアログの ASP.NET Core では、次の警告を表示可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d7766-123">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="d7766-124">.NET Core SDK は、複数の場所にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="d7766-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="d7766-125">インストールされた SDK からのテンプレートのみ 'c:\\Program Files\\dotnet\\sdk\\' が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d7766-125">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![警告メッセージを表示する OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="d7766-127">.NET Core SDK の少なくとも 1 つのインストール ディレクトリの外部にある場合、このメッセージが表示*c:\\Program Files\\dotnet\\sdk\\*します。</span><span class="sxs-lookup"><span data-stu-id="d7766-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="d7766-128">通常は、.NET Core SDK が、MSI インストーラーではなくコピー/貼り付けを使用して、マシンにデプロイされている場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="d7766-128">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="d7766-129">この警告を回避するには、32 ビット .NET Core SDK をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="d7766-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="d7766-130">アンインストール**コントロール パネルの** > **プログラムと機能** > **のアンインストールと変更プログラム**です。</span><span class="sxs-lookup"><span data-stu-id="d7766-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="d7766-131">警告が発生した理由とその影響を理解している場合は、警告を無視できます。</span><span class="sxs-lookup"><span data-stu-id="d7766-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="d7766-132">.NET Core Sdk は検出されませんでした。</span><span class="sxs-lookup"><span data-stu-id="d7766-132">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="d7766-133">**新しいプロジェクト**ダイアログの ASP.NET Core では、次の警告を表示可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d7766-133">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="d7766-134">.NET Core Sdk は検出されませんでした、環境変数 'PATH' に含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d7766-134">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![警告メッセージを表示する OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="d7766-136">この警告が表示されるときに、環境変数`PATH`コンピューターの任意の .NET Core 用 Sdk を指していません。</span><span class="sxs-lookup"><span data-stu-id="d7766-136">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="d7766-137">この問題を解決するには。</span><span class="sxs-lookup"><span data-stu-id="d7766-137">To resolve this problem:</span></span>

* <span data-ttu-id="d7766-138">インストールまたは .NET Core SDK がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d7766-138">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="d7766-139">いることを確認、`PATH`環境変数が、SDK がインストールされている場所を指します。</span><span class="sxs-lookup"><span data-stu-id="d7766-139">Verify that the `PATH` environment variable points to the location in which the SDK is installed.</span></span> <span data-ttu-id="d7766-140">インストーラーが通常の設定、`PATH`します。</span><span class="sxs-lookup"><span data-stu-id="d7766-140">The installer normally sets the `PATH`.</span></span>
