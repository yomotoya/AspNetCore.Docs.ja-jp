---
title: ASP.NET Core をトラブルシューティングします。
author: Rick-Anderson
description: 理解し、警告および ASP.NET Core プロジェクトによるエラーのトラブルシューティングを行います。
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: testing/troubleshoot
ms.openlocfilehash: a75dc666621600e1e2fe36c29acbe7484bae9229
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="642ec-103">ASP.NET Core プロジェクトをトラブルシューティングします。</span><span class="sxs-lookup"><span data-stu-id="642ec-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="642ec-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="642ec-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="642ec-105">次のリンクは、トラブルシューティングのガイダンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="642ec-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="642ec-106">Azure App Service での ASP.NET Core のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="642ec-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="642ec-107">IIS での ASP.NET Core のトラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="642ec-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="642ec-108">Azure App Service および IIS と ASP.NET Core の一般的なエラーのリファレンス</span><span class="sxs-lookup"><span data-stu-id="642ec-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="642ec-109">YouTube: ASP.NET Core アプリケーションでの問題を診断します。</span><span class="sxs-lookup"><span data-stu-id="642ec-109">YouTube: Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="642ec-110">.NET core SDK 警告</span><span class="sxs-lookup"><span data-stu-id="642ec-110">.NET Core SDK warnings</span></span>

### <a name="both-the-32-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="642ec-111">両方の 32 と 64 ビット バージョンの .NET Core SDK がインストールされています。</span><span class="sxs-lookup"><span data-stu-id="642ec-111">Both the 32 and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="642ec-112">**新しいプロジェクト**ダイアログ ASP.NET Core の最上部に表示する次の警告が表示。</span><span class="sxs-lookup"><span data-stu-id="642ec-112">In the **New Project** dialog for ASP.NET Core, you may see the following warning appear at the top:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![警告メッセージを示す OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="642ec-114">この警告を表示するに 32 ビット (x86) と 64 ビット (x64) バージョンの両方、 [.NET Core SDK](https://www.microsoft.com/net/download/all)がインストールされています。</span><span class="sxs-lookup"><span data-stu-id="642ec-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="642ec-115">両方のバージョンをインストールする一般的な理由は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="642ec-115">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="642ec-116">最初、32 ビット コンピューターを使用して、.NET Core SDK インストーラーのダウンロードが間でそれをコピーし、64 ビット コンピューターにインストールします。</span><span class="sxs-lookup"><span data-stu-id="642ec-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="642ec-117">別のアプリケーションによって、32 ビットの .NET Core SDK がインストールされました。</span><span class="sxs-lookup"><span data-stu-id="642ec-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="642ec-118">間違ったバージョンがダウンロードされ、インストールします。</span><span class="sxs-lookup"><span data-stu-id="642ec-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="642ec-119">この警告を回避するのには、32 ビット .NET Core SDK をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="642ec-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="642ec-120">アンインストール**コントロール パネルの** > **プログラムと機能** > **のアンインストールと変更プログラム**です。</span><span class="sxs-lookup"><span data-stu-id="642ec-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="642ec-121">警告が発生した理由とその影響を理解している場合は、警告を無視できます。</span><span class="sxs-lookup"><span data-stu-id="642ec-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="642ec-122">複数の場所では、.NET Core SDK がインストールされています。</span><span class="sxs-lookup"><span data-stu-id="642ec-122">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="642ec-123">**新しいプロジェクト**の ASP.NET Core の最上部に表示する次の警告が表示されるダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="642ec-123">In the **New Project** dialog for ASP.NET Core you may see the following warning appear at the top:</span></span> 

 <span data-ttu-id="642ec-124">.NET Core SDK は、複数の場所にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="642ec-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="642ec-125">インストールされている SDK(s) からのみテンプレート ' C:\Program Files\dotnet\sdk\'が表示されます。</span><span class="sxs-lookup"><span data-stu-id="642ec-125">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![警告メッセージを示す OneASP.NET ダイアログのスクリーン ショット](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="642ec-127">.NET Core SDK の少なくとも 1 つのインストール ディレクトリの外部にあるために、このメッセージが表示されている * C:\Program Files\dotnet\sdk\*です。</span><span class="sxs-lookup"><span data-stu-id="642ec-127">You are seeing this message because you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="642ec-128">通常を .NET Core SDK は、MSI インストーラーではなくコピー/貼り付けを使用してコンピューターに配置されている場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="642ec-128">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="642ec-129">この警告を回避するのには、32 ビット .NET Core SDK をアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="642ec-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="642ec-130">アンインストール**コントロール パネルの** > **プログラムと機能** > **のアンインストールと変更プログラム**です。</span><span class="sxs-lookup"><span data-stu-id="642ec-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="642ec-131">警告が発生した理由とその影響を理解している場合は、警告を無視できます。</span><span class="sxs-lookup"><span data-stu-id="642ec-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>
