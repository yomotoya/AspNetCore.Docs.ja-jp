---
title: ASP.NET Core MVC の互換バージョン
author: rick-anderson
description: ASP.NET Core の Startup クラスがサービスとアプリケーションの要求パイプラインをどのように構成しているかを説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2019
uid: mvc/compatibility-version
ms.openlocfilehash: 7c4189db435088e0803b35add82fa0eb9372e664
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410146"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a><span data-ttu-id="7a2ac-103">ASP.NET Core MVC の互換バージョン</span><span class="sxs-lookup"><span data-stu-id="7a2ac-103">Compatibility version for ASP.NET Core MVC</span></span>

<span data-ttu-id="7a2ac-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7a2ac-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7a2ac-105"><xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> メソッドを使用すると、ASP.NET Core MVC 2.1 以降に導入されている、互換性に影響する重大な変更をオプトインまたはオプトアウトすることができます。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-105">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span> <span data-ttu-id="7a2ac-106">互換性に影響する可能性のあるこれらの重大な変更は、ほとんどの場合、MVC サブシステムの動作方法と、ランタイムで**ユーザーのコード**が呼び出される方法についてです。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-106">These potentially breaking behavior changes are generally in how the MVC subsystem behaves and how **your code** is called by the runtime.</span></span> <span data-ttu-id="7a2ac-107">オプトインした場合、最新の動作と ASP.NET Core の最新の動作を得ることができます。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-107">By opting in, you get the latest behavior, and the long-term behavior of ASP.NET Core.</span></span>

<span data-ttu-id="7a2ac-108">次のコードにより、互換性モードは ASP.NET Core 2.2 に設定されます。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-108">The following code sets the compatibility mode to ASP.NET Core 2.2:</span></span>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

<span data-ttu-id="7a2ac-109">最新のバージョン (`CompatibilityVersion.Version_2_2`) を使用してアプリをテストすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-109">We recommend you test your app using the latest version (`CompatibilityVersion.Version_2_2`).</span></span> <span data-ttu-id="7a2ac-110">最新のバージョンを使用しても、ほとんどのアプリで重大な動作変更はないと見込まれます。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-110">We anticipate that most apps won't have breaking behavior changes using the latest version.</span></span>

<span data-ttu-id="7a2ac-111">`SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` を呼び出すアプリは、ASP.NET Core 2.1 MVC およびそれ以降の 2. バージョンで導入された重大な動作変更の可能性から保護されています。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-111">Apps that call `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` are protected from potentially breaking behavior changes introduced in the ASP.NET Core 2.1 MVC and later 2.x versions.</span></span> <span data-ttu-id="7a2ac-112">この保護とは、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-112">This protection:</span></span>

* <span data-ttu-id="7a2ac-113">2.1 以降のすべての変更には該当しません。これは、MVC サブシステムの ASP.NET Core ランタイムの互換性に影響する可能性のある重大な変更のみを対象としています。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-113">Does not apply to all 2.1 and later changes, it's targeted to potentially breaking ASP.NET Core runtime behavior changes in the MVC subsystem.</span></span>
* <span data-ttu-id="7a2ac-114">次のメジャー バージョンには適用されません。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-114">Does not extend to the next major version.</span></span>

<span data-ttu-id="7a2ac-115">`SetCompatibilityVersion` を呼び出さ**ない** ASP.NET Core 2.1 およびそれ以降の 2.x アプリケーションの既定の互換性は、2.0 の互換性です。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-115">The default compatibility for ASP.NET Core 2.1 and later 2.x apps that do **not** call `SetCompatibilityVersion` is 2.0 compatibility.</span></span> <span data-ttu-id="7a2ac-116">つまり、`SetCompatibilityVersion` を呼び出さないことは、`SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` を呼び出すことと同じです。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-116">That is, not calling `SetCompatibilityVersion` is the same as calling `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span></span>

<span data-ttu-id="7a2ac-117">次のコードでは、以下の動作を除き、互換性モードを ASP.NET Core 2.2 に設定します。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-117">The following code sets the compatibility mode to ASP.NET Core 2.2, except for the following behaviors:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowCombiningAuthorizeFilters>
* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.InputFormatterExceptionPolicy>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

<span data-ttu-id="7a2ac-118">互換性に影響する重大な変更があったアプリでは、適切な互換性スイッチを使用することにより、次が得られます。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-118">For apps that encounter breaking behavior changes, using the appropriate compatibility switches:</span></span>

* <span data-ttu-id="7a2ac-119">最新のリリースを使用し、互換性に影響する特定の重大な変更をオプトアウトできます。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-119">Allows you to use the latest release and opt out of specific breaking behavior changes.</span></span>
* <span data-ttu-id="7a2ac-120">アプリが最新の変更に対応するよう、更新を行う時間を得られます。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-120">Gives you time to update your app so it works with the latest changes.</span></span>

<span data-ttu-id="7a2ac-121">[MvcOptions](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Mvc/Mvc.Core/src/MvcOptions.cs) クラス ソースのコメントには、変更があった個所とそれらの改善が多くのユーザーに与えるメリットについて説明しています。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-121">The [MvcOptions](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Mvc/Mvc.Core/src/MvcOptions.cs) class source comments have a good explanation of what changed and why the changes are an improvement for most users.</span></span>

<span data-ttu-id="7a2ac-122">将来的に、[ASP.NET Core 3.0 バージョン](https://github.com/aspnet/Home/wiki/Roadmap)がリリースされます。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-122">At some future date, there will be an [ASP.NET Core 3.0 version](https://github.com/aspnet/Home/wiki/Roadmap).</span></span> <span data-ttu-id="7a2ac-123">3.0 バージョンでは、互換性スイッチによってサポートされている古い動作は削除されます。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-123">Old behaviors supported by compatibility switches will be removed in the 3.0 version.</span></span> <span data-ttu-id="7a2ac-124">これらの正の変更は、ほぼすべてのユーザーにとってメリットとなると感じています。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-124">We feel these are positive changes benefitting nearly all users.</span></span> <span data-ttu-id="7a2ac-125">これらの変更を導入することにより、ほとんどのアプリでメリットを得られるようになり、またその他のユーザーにはアプリをアップデートするための時間ができます。</span><span class="sxs-lookup"><span data-stu-id="7a2ac-125">By introducing these changes now, most apps can benefit now, and the others will have time to update their apps.</span></span>
