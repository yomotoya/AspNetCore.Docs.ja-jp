---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: ASP.NET MVC 4 にアップグレードしてから Web API プロジェクトを ASP.NET MVC 5 と Web API 2 の方法 |Microsoft Docs
author: Rick-Anderson
description: ASP.NET MVC 5 と Web API 2 は、属性ルーティング、認証フィルター、およびその他を含む新しい機能のホストをもたらします。
ms.author: riande
ms.date: 10/17/2013
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: e313fc2229026f1b11d2f6a046a8b550c08ba96b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835214"
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a><span data-ttu-id="30e1e-103">ASP.NET MVC 5 と Web API 2 に、ASP.NET MVC 4 と Web API プロジェクトをアップグレードする方法</span><span class="sxs-lookup"><span data-stu-id="30e1e-103">How to Upgrade an ASP.NET MVC 4 and Web API Project to ASP.NET MVC 5 and Web API 2</span></span>
====================
<span data-ttu-id="30e1e-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="30e1e-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="30e1e-105">ASP.NET MVC 5 と Web API 2 は、属性ルーティング、認証フィルター、およびその他を含む新しい機能のホストをもたらします。</span><span class="sxs-lookup"><span data-stu-id="30e1e-105">ASP.NET MVC 5 and Web API 2 bring a host of new features, including attribute routing, authentication filters, and much more.</span></span> <span data-ttu-id="30e1e-106">参照してください[ https://www.asp.net/vnext ](https://www.asp.net/core)の詳細。</span><span class="sxs-lookup"><span data-stu-id="30e1e-106">See [https://www.asp.net/vnext](https://www.asp.net/core) for more details.</span></span>
> 
> <span data-ttu-id="30e1e-107">このチュートリアルでは、アプリケーションを最新バージョンのアップグレードに必要な手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-107">This walkthrough will guide you with the steps required to upgrade your application to the latest version.</span></span>  
> 
> > [!NOTE]
> > <span data-ttu-id="30e1e-108">参照してください[ASP.NET および Web Tools for Visual Studio 2013 リリース ノート](../../../visual-studio/overview/2013/release-notes.md)についての最新の MVC 4 と Web API から次のバージョンに変更します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-108">Please see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md) for information on breaking changes from MVC 4 and Web API to the next version.</span></span>
> 
>   
> 
> <span data-ttu-id="30e1e-109">この記事は Youngjune 香港特別行政区、Rick Anderson によって書き込まれました ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="30e1e-109">This article was written by Youngjune Hong and Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>


## <a name="upgrade-steps"></a><span data-ttu-id="30e1e-110">アップグレード手順</span><span class="sxs-lookup"><span data-stu-id="30e1e-110">Upgrade Steps</span></span>

1. <span data-ttu-id="30e1e-111">プロジェクトをバックアップします。</span><span class="sxs-lookup"><span data-stu-id="30e1e-111">Backup your project.</span></span> <span data-ttu-id="30e1e-112">このチュートリアルでは、プロジェクト ファイル、パッケージの構成、および web.config ファイルを変更することが必要です。</span><span class="sxs-lookup"><span data-stu-id="30e1e-112">This walkthrough will require you to make changes to your project file, package configuration, and web.config files.</span></span>
2. <span data-ttu-id="30e1e-113">Web API 2 に、global.asax で Web API からのアップグレードの次のように変更します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-113">For upgrading from Web API to Web API 2, in global.asax, change:</span></span>

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

   <span data-ttu-id="30e1e-114">から</span><span class="sxs-lookup"><span data-stu-id="30e1e-114">to</span></span>

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. <span data-ttu-id="30e1e-115">プロジェクトを使用するすべてのパッケージは、MVC 5 と Web API 2 と互換性があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-115">Make sure all the packages that your projects use are compatible with MVC 5 and Web API 2.</span></span> <span data-ttu-id="30e1e-116">次の表に示します、MVC 4 と Web API を変更する必要があるパッケージに関連します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-116">The following table shows the MVC 4 and Web API related packages than need to be changed.</span></span> <span data-ttu-id="30e1e-117">以下に示すパッケージのいずれかに依存するパッケージがあれば、MVC 5 と Web API 2 と互換性がある新しいバージョンを取得する発行元にお問い合わせください。</span><span class="sxs-lookup"><span data-stu-id="30e1e-117">If you have a package that is dependent on one of the packages listed below, please contact the publishers to get the newer versions that are compatible with MVC 5 and Web API 2.</span></span> <span data-ttu-id="30e1e-118">それらのパッケージのソース コードがあれば、MVC 5 と Web API 2 の新しいアセンブリを再コンパイルする必要があります。</span><span class="sxs-lookup"><span data-stu-id="30e1e-118">If you have the source code for those packages, you should recompile them with the new assemblies of MVC 5 and Web API 2.</span></span>   

    | <span data-ttu-id="30e1e-119">**パッケージ Id**</span><span class="sxs-lookup"><span data-stu-id="30e1e-119">**Package Id**</span></span> | <span data-ttu-id="30e1e-120">**以前のバージョン**</span><span class="sxs-lookup"><span data-stu-id="30e1e-120">**Old version**</span></span> | <span data-ttu-id="30e1e-121">**新しいバージョン**</span><span class="sxs-lookup"><span data-stu-id="30e1e-121">**New version**</span></span> |
    | --- | --- | --- |
    | <span data-ttu-id="30e1e-122">Microsoft.AspNet.Razor</span><span class="sxs-lookup"><span data-stu-id="30e1e-122">Microsoft.AspNet.Razor</span></span> | <span data-ttu-id="30e1e-123">2.0.x.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-123">2.0.x.x</span></span> | <span data-ttu-id="30e1e-124">3.0.0</span><span class="sxs-lookup"><span data-stu-id="30e1e-124">3.0.0</span></span> |
    | <span data-ttu-id="30e1e-125">Microsoft.AspNet.WebPages</span><span class="sxs-lookup"><span data-stu-id="30e1e-125">Microsoft.AspNet.WebPages</span></span> | <span data-ttu-id="30e1e-126">2.0.x.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-126">2.0.x.x</span></span> | <span data-ttu-id="30e1e-127">3.0.0</span><span class="sxs-lookup"><span data-stu-id="30e1e-127">3.0.0</span></span> |
    | <span data-ttu-id="30e1e-128">Microsoft.AspNet.WebPages.WebData</span><span class="sxs-lookup"><span data-stu-id="30e1e-128">Microsoft.AspNet.WebPages.WebData</span></span> | <span data-ttu-id="30e1e-129">2.0.x.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-129">2.0.x.x</span></span> | <span data-ttu-id="30e1e-130">3.0.0</span><span class="sxs-lookup"><span data-stu-id="30e1e-130">3.0.0</span></span> |
    | <span data-ttu-id="30e1e-131">Microsoft.AspNet.WebPages.OAuth</span><span class="sxs-lookup"><span data-stu-id="30e1e-131">Microsoft.AspNet.WebPages.OAuth</span></span> | <span data-ttu-id="30e1e-132">2.0.x.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-132">2.0.x.x</span></span> | <span data-ttu-id="30e1e-133">3.0.0</span><span class="sxs-lookup"><span data-stu-id="30e1e-133">3.0.0</span></span> |
    | <span data-ttu-id="30e1e-134">Microsoft.AspNet.Mvc</span><span class="sxs-lookup"><span data-stu-id="30e1e-134">Microsoft.AspNet.Mvc</span></span> | <span data-ttu-id="30e1e-135">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-135">4.0.x.x</span></span> | <span data-ttu-id="30e1e-136">5.0.0</span><span class="sxs-lookup"><span data-stu-id="30e1e-136">5.0.0</span></span> |
    | <span data-ttu-id="30e1e-137">Microsoft.AspNet.Mvc.Facebook</span><span class="sxs-lookup"><span data-stu-id="30e1e-137">Microsoft.AspNet.Mvc.Facebook</span></span> | <span data-ttu-id="30e1e-138">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-138">4.0.x.x</span></span> | <span data-ttu-id="30e1e-139">5.0.0</span><span class="sxs-lookup"><span data-stu-id="30e1e-139">5.0.0</span></span> |
    | <span data-ttu-id="30e1e-140">Microsoft.AspNet.WebApi.Core</span><span class="sxs-lookup"><span data-stu-id="30e1e-140">Microsoft.AspNet.WebApi.Core</span></span> | <span data-ttu-id="30e1e-141">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-141">4.0.x.x</span></span> | <span data-ttu-id="30e1e-142">5.0.0</span><span class="sxs-lookup"><span data-stu-id="30e1e-142">5.0.0</span></span> |
    | <span data-ttu-id="30e1e-143">Microsoft.AspNet.WebApi.SelfHost</span><span class="sxs-lookup"><span data-stu-id="30e1e-143">Microsoft.AspNet.WebApi.SelfHost</span></span> | <span data-ttu-id="30e1e-144">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-144">4.0.x.x</span></span> | <span data-ttu-id="30e1e-145">5.0.0</span><span class="sxs-lookup"><span data-stu-id="30e1e-145">5.0.0</span></span> |
    | <span data-ttu-id="30e1e-146">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="30e1e-146">Microsoft.AspNet.WebApi.Client</span></span> | <span data-ttu-id="30e1e-147">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-147">4.0.x.x</span></span> | <span data-ttu-id="30e1e-148">5.0.0</span><span class="sxs-lookup"><span data-stu-id="30e1e-148">5.0.0</span></span> |
    | <span data-ttu-id="30e1e-149">Microsoft.AspNet.WebApi.OData</span><span class="sxs-lookup"><span data-stu-id="30e1e-149">Microsoft.AspNet.WebApi.OData</span></span> | <span data-ttu-id="30e1e-150">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-150">4.0.x.x</span></span> | <span data-ttu-id="30e1e-151">5.0.0</span><span class="sxs-lookup"><span data-stu-id="30e1e-151">5.0.0</span></span> |
    | <span data-ttu-id="30e1e-152">Microsoft.AspNet.WebApi</span><span class="sxs-lookup"><span data-stu-id="30e1e-152">Microsoft.AspNet.WebApi</span></span> | <span data-ttu-id="30e1e-153">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-153">4.0.x.x</span></span> | <span data-ttu-id="30e1e-154">5.0.0</span><span class="sxs-lookup"><span data-stu-id="30e1e-154">5.0.0</span></span> |
    | <span data-ttu-id="30e1e-155">Microsoft.AspNet.WebApi.WebHost</span><span class="sxs-lookup"><span data-stu-id="30e1e-155">Microsoft.AspNet.WebApi.WebHost</span></span> | <span data-ttu-id="30e1e-156">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-156">4.0.x.x</span></span> | <span data-ttu-id="30e1e-157">5.0.0</span><span class="sxs-lookup"><span data-stu-id="30e1e-157">5.0.0</span></span> |
    | <span data-ttu-id="30e1e-158">Microsoft.AspNet.WebApi.Tracing</span><span class="sxs-lookup"><span data-stu-id="30e1e-158">Microsoft.AspNet.WebApi.Tracing</span></span> | <span data-ttu-id="30e1e-159">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-159">4.0.x.x</span></span> | <span data-ttu-id="30e1e-160">5.0.0</span><span class="sxs-lookup"><span data-stu-id="30e1e-160">5.0.0</span></span> |
    | <span data-ttu-id="30e1e-161">Microsoft.AspNet.WebApi.HelpPage</span><span class="sxs-lookup"><span data-stu-id="30e1e-161">Microsoft.AspNet.WebApi.HelpPage</span></span> | <span data-ttu-id="30e1e-162">4.0.x.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-162">4.0.x.x</span></span> | <span data-ttu-id="30e1e-163">5.0.0</span><span class="sxs-lookup"><span data-stu-id="30e1e-163">5.0.0</span></span> |
    | <span data-ttu-id="30e1e-164">Microsoft.Net.Http</span><span class="sxs-lookup"><span data-stu-id="30e1e-164">Microsoft.Net.Http</span></span> | <span data-ttu-id="30e1e-165">2.0.x します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-165">2.0.x.</span></span> | <span data-ttu-id="30e1e-166">2.2.x します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-166">2.2.x.</span></span> |
    | <span data-ttu-id="30e1e-167">Microsoft.Data.OData</span><span class="sxs-lookup"><span data-stu-id="30e1e-167">Microsoft.Data.OData</span></span> | <span data-ttu-id="30e1e-168">5.2.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-168">5.2.x</span></span> | <span data-ttu-id="30e1e-169">5.6.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-169">5.6.x</span></span> |
    | <span data-ttu-id="30e1e-170">System.Spatial</span><span class="sxs-lookup"><span data-stu-id="30e1e-170">System.Spatial</span></span> | <span data-ttu-id="30e1e-171">5.2.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-171">5.2.x</span></span> | <span data-ttu-id="30e1e-172">5.6.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-172">5.6.x</span></span> |
    | <span data-ttu-id="30e1e-173">Microsoft.Data.Edm</span><span class="sxs-lookup"><span data-stu-id="30e1e-173">Microsoft.Data.Edm</span></span> | <span data-ttu-id="30e1e-174">5.2.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-174">5.2.x</span></span> | <span data-ttu-id="30e1e-175">5.6.x</span><span class="sxs-lookup"><span data-stu-id="30e1e-175">5.6.x</span></span> |
    | <span data-ttu-id="30e1e-176">Microsoft.AspNet.Mvc.FixedDisplayModes</span><span class="sxs-lookup"><span data-stu-id="30e1e-176">Microsoft.AspNet.Mvc.FixedDisplayModes</span></span> | <span data-ttu-id="30e1e-177"><o:p> </o:p></span><span class="sxs-lookup"><span data-stu-id="30e1e-177"><o:p> </o:p></span></span> | <span data-ttu-id="30e1e-178">削除済み</span><span class="sxs-lookup"><span data-stu-id="30e1e-178">Removed</span></span> |
    | <span data-ttu-id="30e1e-179">Microsoft.AspNet.WebPages.Administration</span><span class="sxs-lookup"><span data-stu-id="30e1e-179">Microsoft.AspNet.WebPages.Administration</span></span> | <span data-ttu-id="30e1e-180"><o:p> </o:p></span><span class="sxs-lookup"><span data-stu-id="30e1e-180"><o:p> </o:p></span></span> | <span data-ttu-id="30e1e-181">削除済み</span><span class="sxs-lookup"><span data-stu-id="30e1e-181">Removed</span></span> |
    | <span data-ttu-id="30e1e-182">Microsoft の Web Helpers</span><span class="sxs-lookup"><span data-stu-id="30e1e-182">Microsoft-Web-Helpers</span></span> | <span data-ttu-id="30e1e-183"><o:p> </o:p></span><span class="sxs-lookup"><span data-stu-id="30e1e-183"><o:p> </o:p></span></span> | <span data-ttu-id="30e1e-184">Microsoft.AspNet.WebHelpers</span><span class="sxs-lookup"><span data-stu-id="30e1e-184">Microsoft.AspNet.WebHelpers</span></span> |

    > [!NOTE]
    > <span data-ttu-id="30e1e-185">Microsoft の Web Helpers が Microsoft.AspNet.WebHelpers に置き換えられました。</span><span class="sxs-lookup"><span data-stu-id="30e1e-185">Microsoft-Web-Helpers has been replaced with Microsoft.AspNet.WebHelpers.</span></span> <span data-ttu-id="30e1e-186">最初に、古いパッケージを削除して、新しいパッケージをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="30e1e-186">You should remove the old package first, and then install the newer package.</span></span>   
    >   
    > <span data-ttu-id="30e1e-187">ASP.NET の主要なパッケージ間で相互のバージョンの互換性はありません。</span><span class="sxs-lookup"><span data-stu-id="30e1e-187">There is no cross version compatibility among major ASP.NET packages.</span></span> <span data-ttu-id="30e1e-188">たとえば、MVC 5 は、Razor の 3 としない Razor 2 のみと互換性のあります。</span><span class="sxs-lookup"><span data-stu-id="30e1e-188">For example, MVC 5 is compatible with only Razor 3, and not Razor 2.</span></span>
4. <span data-ttu-id="30e1e-189">Visual Studio 2013 でプロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="30e1e-189">Open your project in Visual Studio 2013.</span></span>
5. <span data-ttu-id="30e1e-190">インストールされている ASP.NET の NuGet パッケージの次のいずれかを削除します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-190">Remove any of the following ASP.NET NuGet packages that are installed.</span></span> <span data-ttu-id="30e1e-191">パッケージ マネージャー コンソール (PMC) を使用して、これらが削除されます。</span><span class="sxs-lookup"><span data-stu-id="30e1e-191">You will remove these using the Package Manager Console (PMC).</span></span> <span data-ttu-id="30e1e-192">PMC を開くには、次のように選択します。、**ツール**メニューから選択し、**ライブラリ パッケージ マネージャー、** 選び**パッケージ マネージャー コンソール**。</span><span class="sxs-lookup"><span data-stu-id="30e1e-192">To open the PMC, select the **Tools** menu and then select **Library Package Manager,** then select **Package Manager Console**.</span></span> <span data-ttu-id="30e1e-193">プロジェクトでは、これらすべてのなどがありますされません。</span><span class="sxs-lookup"><span data-stu-id="30e1e-193">Your project might not include all of these.</span></span>

    1. `Microsoft.AspNet.WebPages.Administration`  
   <span data-ttu-id="30e1e-194">このパッケージは通常、MVC 3 から MVC 4 にアップグレードするときに追加されます。</span><span class="sxs-lookup"><span data-stu-id="30e1e-194">This package is typically added when upgrading from MVC 3 to MVC 4.</span></span> <span data-ttu-id="30e1e-195">削除するには、PMC で、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-195">To remove it, run the following command in the PMC:</span></span>  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
   <span data-ttu-id="30e1e-196">このパッケージとしてブランドされて`Microsoft.AspNet.WebHelpers`します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-196">This package has been rebranded as `Microsoft.AspNet.WebHelpers`.</span></span> <span data-ttu-id="30e1e-197">削除するには、PMC で、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-197">To remove it, run the following command in the PMC:</span></span>  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
   <span data-ttu-id="30e1e-198">このパッケージには、MVC 4 MVC 5 で修正されたバグの回避が含まれています。</span><span class="sxs-lookup"><span data-stu-id="30e1e-198">This package contains a work around for a bug in MVC 4 that has been fixed in MVC 5.</span></span> <span data-ttu-id="30e1e-199">削除するには、PMC で、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-199">To remove it, run the following command in the PMC:</span></span>  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. <span data-ttu-id="30e1e-200">PMC を使用してすべての ASP.NET の NuGet パッケージをアップグレードします。</span><span class="sxs-lookup"><span data-stu-id="30e1e-200">Upgrade all the ASP.NET NuGet packages using the PMC.</span></span> <span data-ttu-id="30e1e-201">PMC では、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-201">In the PMC, run the following command:</span></span>  
    `Update-Package`  
   <span data-ttu-id="30e1e-202">`Update-Package`コマンドを任意のパラメーターはすべてのパッケージを更新します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-202">The `Update-Package` command without any parameters will update every package.</span></span> <span data-ttu-id="30e1e-203">ID の引数を使用してパッケージを個別に更新できます。</span><span class="sxs-lookup"><span data-stu-id="30e1e-203">You can update packages individually by using the ID argument.</span></span> <span data-ttu-id="30e1e-204">Update コマンドの詳細については、実行`get-help update-package`します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-204">For more information about the update command, run     `get-help update-package` .</span></span>

## <a name="update-the-application-webconfig-file"></a><span data-ttu-id="30e1e-205">アプリケーションを更新して*web.config*ファイル</span><span class="sxs-lookup"><span data-stu-id="30e1e-205">Update the Application *web.config* File</span></span>

<span data-ttu-id="30e1e-206">アプリでこれらの変更を必ず*web.config*ファイルではありません、 *web.config*ファイル、*ビュー*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="30e1e-206">Be sure to make these changes in the app *web.config* file, not the *web.config* file in the *Views* folder.</span></span>

<span data-ttu-id="30e1e-207">検索、`<runtime>/<assemblyBinding>`セクションし、次の変更します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-207">Locate the `<runtime>/<assemblyBinding>` section, and make the following changes:</span></span>

1. <span data-ttu-id="30e1e-208">Name 属性が"System.Web.Mvc"要素では、「5.0.0.0」を「4.0.0.0」からバージョン番号を変更します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-208">In the elements with the name attribute "System.Web.Mvc", change the version number from "4.0.0.0" to "5.0.0.0".</span></span> <span data-ttu-id="30e1e-209">(その要素の 2 つの変更。 を)</span><span class="sxs-lookup"><span data-stu-id="30e1e-209">(Two changes in that element.)</span></span>
2. <span data-ttu-id="30e1e-210">名前属性を持つ要素で&quot;System.Web.Helpers"と&quot;System.Web.WebPages&quot; 「2.0.0.0」から「3.0.0.0」のバージョン番号を変更します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-210">In elements with the name attribute &quot;System.Web.Helpers" and &quot;System.Web.WebPages&quot; change the version number from "2.0.0.0" to "3.0.0.0".</span></span> <span data-ttu-id="30e1e-211">4 つの変更が発生、2 つの各要素にします。</span><span class="sxs-lookup"><span data-stu-id="30e1e-211">Four changes will occur, two in each of the elements.</span></span>

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. <span data-ttu-id="30e1e-212">検索、`<appSettings>`セクションし、次に示すように 3.0.0.0 2.0.0.0.0 から webpages:version を更新します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-212">Locate the `<appSettings>` section and update the webpages:version from 2.0.0.0.0 to 3.0.0.0 as shown below:</span></span>

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. <span data-ttu-id="30e1e-213">フル以外の任意の信頼レベルを削除します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-213">Remove any trust levels other than Full.</span></span> <span data-ttu-id="30e1e-214">例えば:</span><span class="sxs-lookup"><span data-stu-id="30e1e-214">For example:</span></span>

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a><span data-ttu-id="30e1e-215">更新プログラム、 *web.config* Views フォルダーの下のファイル</span><span class="sxs-lookup"><span data-stu-id="30e1e-215">Update the *web.config* files under the Views folder</span></span>

<span data-ttu-id="30e1e-216">アプリケーションの領域を使用している場合も必要がありますは各更新*web.config*ファイル、*ビュー*各区分フォルダーのサブフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="30e1e-216">If your application is using areas, you will also need to update each *web.config* file in the *Views* sub-folder of each Area folder.</span></span>

1. <span data-ttu-id="30e1e-217">バージョン「5.0.0.0」バージョン「4.0.0.0」から"System.Web.Mvc"が含まれているすべての要素を更新します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-217">Update all elements that contain "System.Web.Mvc" from version "4.0.0.0" to version "5.0.0.0".</span></span>  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. <span data-ttu-id="30e1e-218">Version 3.0.0.0「から」バージョン「2.0.0.0」から"System.Web.WebPages.Razor"を格納するすべての要素を更新します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-218">Update all elements that contain "System.Web.WebPages.Razor" from version "2.0.0.0" to version"3.0.0.0".</span></span> <span data-ttu-id="30e1e-219">このセクションに"System.Web.WebPages"が含まれている場合は、version 3.0.0.0「から」を「2.0.0.0」のバージョンからそれらの要素を更新します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-219">If this section contains "System.Web.WebPages", update those elements from version "2.0.0.0" to version"3.0.0.0"</span></span>  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. <span data-ttu-id="30e1e-220">削除した場合、`Microsoft-Web-Helpers`前の手順で NuGet パッケージをインストール`Microsoft.AspNet.WebHelpers`PMC で次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-220">If you removed the `Microsoft-Web-Helpers` NuGet package in a previous step, install `Microsoft.AspNet.WebHelpers` with the following command in the PMC:</span></span>  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. <span data-ttu-id="30e1e-221">アプリで使用する場合、 [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx)メソッドに次のコードを追加、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="30e1e-221">If your app uses the [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx) method, add the following to the *Web.config* file.</span></span>

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a><span data-ttu-id="30e1e-222">最後の手順</span><span class="sxs-lookup"><span data-stu-id="30e1e-222">Final Steps</span></span>

<span data-ttu-id="30e1e-223">ビルドして、アプリケーションをテストします。</span><span class="sxs-lookup"><span data-stu-id="30e1e-223">Build and test the application.</span></span>

<span data-ttu-id="30e1e-224">プロジェクト ファイルから MVC 4 プロジェクト型 GUID を削除します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-224">Remove the MVC 4 project type GUID from the project files.</span></span>

1. <span data-ttu-id="30e1e-225">ソリューション エクスプ ローラーでプロジェクト名を右クリックし、**プロジェクトのアンロード**します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-225">In Solution Explorer, right-click the project name and then select **Unload Project**.</span></span>
2. <span data-ttu-id="30e1e-226">プロジェクトを右クリックし、&gt;.csproj の編集します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-226">Right-click the project and select Edit ProjectName.csproj.</span></span>
3. <span data-ttu-id="30e1e-227">検索、`ProjectTypeGuids`要素とし、削除、MVC 4 プロジェクト GUID、`{E3E379DF-F4C6-4180-9B81-6769533ABE47}`します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-227">Locate the `ProjectTypeGuids` element and then remove the MVC 4 project GUID, `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`.</span></span>
4. <span data-ttu-id="30e1e-228">保存してプロジェクトを開くファイルを閉じます。</span><span class="sxs-lookup"><span data-stu-id="30e1e-228">Save and close the open project file.</span></span>
5. <span data-ttu-id="30e1e-229">プロジェクトを右クリックして**プロジェクトの再読み込み**します。</span><span class="sxs-lookup"><span data-stu-id="30e1e-229">Right-click the project and select **Reload Project**.</span></span>
