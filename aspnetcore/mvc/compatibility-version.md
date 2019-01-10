---
title: ASP.NET Core MVC の互換バージョン
author: rick-anderson
description: ASP.NET Core の Startup クラスがサービスとアプリケーションの要求パイプラインをどのように構成しているかを説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/10/2018
uid: mvc/compatibility-version
ms.openlocfilehash: 63243d99c7cb74a7e594cd309a808455c6611fc0
ms.sourcegitcommit: 68a3081dd175d6518d1bfa31b4712bd8a2dd3864
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/18/2018
ms.locfileid: "53577852"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a><span data-ttu-id="8046c-103">ASP.NET Core MVC の互換バージョン</span><span class="sxs-lookup"><span data-stu-id="8046c-103">Compatibility version for ASP.NET Core MVC</span></span>

<span data-ttu-id="8046c-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8046c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8046c-105"><xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> メソッドを使用すると、ASP.NET Core MVC 2.1 以降に導入されている、互換性に影響する重大な変更をオプトインまたはオプトアウトすることができます。</span><span class="sxs-lookup"><span data-stu-id="8046c-105">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span> <span data-ttu-id="8046c-106">互換性に影響する可能性のあるこれらの重大な変更は、ほとんどの場合、MVC サブシステムの動作方法と、ランタイムで**ユーザーのコード**が呼び出される方法についてです。</span><span class="sxs-lookup"><span data-stu-id="8046c-106">These potentially breaking behavior changes are generally in how the MVC subsystem behaves and how **your code** is called by the runtime.</span></span> <span data-ttu-id="8046c-107">オプトインした場合、最新の動作と ASP.NET Core の最新の動作を得ることができます。</span><span class="sxs-lookup"><span data-stu-id="8046c-107">By opting in, you get the latest behavior, and the long-term behavior of ASP.NET Core.</span></span>

<span data-ttu-id="8046c-108">次のコードにより、互換性モードは ASP.NET Core 2.2 に設定されます。</span><span class="sxs-lookup"><span data-stu-id="8046c-108">The following code sets the compatibility mode to ASP.NET Core 2.2:</span></span>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

<span data-ttu-id="8046c-109">最新のバージョン (`CompatibilityVersion.Version_2_2`) を使用してアプリをテストすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="8046c-109">We recommend you test your app using the latest version (`CompatibilityVersion.Version_2_2`).</span></span> <span data-ttu-id="8046c-110">最新のバージョンを使用しても、ほとんどのアプリで重大な動作変更はないと見込まれます。</span><span class="sxs-lookup"><span data-stu-id="8046c-110">We anticipate that most apps won't have breaking behavior changes using the latest version.</span></span>

<span data-ttu-id="8046c-111">`SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` を呼び出すアプリは、ASP.NET Core 2.1 MVC およびそれ以降の 2. バージョンで導入された重大な動作変更の可能性から保護されています。</span><span class="sxs-lookup"><span data-stu-id="8046c-111">Apps that call `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` are protected from potentially breaking behavior changes introduced in the ASP.NET Core 2.1 MVC and later 2.x versions.</span></span> <span data-ttu-id="8046c-112">この保護とは、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="8046c-112">This protection:</span></span>

* <span data-ttu-id="8046c-113">2.1 以降のすべての変更には該当しません。これは、MVC サブシステムの ASP.NET Core ランタイムの互換性に影響する可能性のある重大な変更のみを対象としています。</span><span class="sxs-lookup"><span data-stu-id="8046c-113">Does not apply to all 2.1 and later changes, it's targeted to potentially breaking ASP.NET Core runtime behavior changes in the MVC subsystem.</span></span>
* <span data-ttu-id="8046c-114">次のメジャー バージョンには適用されません。</span><span class="sxs-lookup"><span data-stu-id="8046c-114">Does not extend to the next major version.</span></span>

<span data-ttu-id="8046c-115">`SetCompatibilityVersion` を呼び出さ**ない** ASP.NET Core 2.1 およびそれ以降の 2.x アプリケーションの既定の互換性は、2.0 の互換性です。</span><span class="sxs-lookup"><span data-stu-id="8046c-115">The default compatibility for ASP.NET Core 2.1 and later 2.x apps that do **not** call `SetCompatibilityVersion` is 2.0 compatibility.</span></span> <span data-ttu-id="8046c-116">つまり、`SetCompatibilityVersion` を呼び出さないことは、`SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` を呼び出すことと同じです。</span><span class="sxs-lookup"><span data-stu-id="8046c-116">That is, not calling `SetCompatibilityVersion` is the same as calling `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span></span>

<span data-ttu-id="8046c-117">次のコードでは、以下の動作を除き、互換性モードを ASP.NET Core 2.2 に設定します。</span><span class="sxs-lookup"><span data-stu-id="8046c-117">The following code sets the compatibility mode to ASP.NET Core 2.2, except for the following behaviors:</span></span>

* [<span data-ttu-id="8046c-118">AllowCombiningAuthorizeFilters</span><span class="sxs-lookup"><span data-stu-id="8046c-118">AllowCombiningAuthorizeFilters</span></span>](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [<span data-ttu-id="8046c-119">InputFormatterExceptionPolicy</span><span class="sxs-lookup"><span data-stu-id="8046c-119">InputFormatterExceptionPolicy</span></span>](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

<span data-ttu-id="8046c-120">互換性に影響する重大な変更があったアプリでは、適切な互換性スイッチを使用することにより、次が得られます。</span><span class="sxs-lookup"><span data-stu-id="8046c-120">For apps that encounter breaking behavior changes, using the appropriate compatibility switches:</span></span>

* <span data-ttu-id="8046c-121">最新のリリースを使用し、互換性に影響する特定の重大な変更をオプトアウトできます。</span><span class="sxs-lookup"><span data-stu-id="8046c-121">Allows you to use the latest release and opt out of specific breaking behavior changes.</span></span>
* <span data-ttu-id="8046c-122">アプリが最新の変更に対応するよう、更新を行う時間を得られます。</span><span class="sxs-lookup"><span data-stu-id="8046c-122">Gives you time to update your app so it works with the latest changes.</span></span>

<span data-ttu-id="8046c-123">[MvcOptions](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) クラス ソースのコメントには、変更があった個所とそれらの改善が多くのユーザーに与えるメリットについて説明しています。</span><span class="sxs-lookup"><span data-stu-id="8046c-123">The [MvcOptions](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) class source comments have a good explanation of what changed and why the changes are an improvement for most users.</span></span>

<span data-ttu-id="8046c-124">将来的に、[ASP.NET Core 3.0 バージョン](https://github.com/aspnet/Home/wiki/Roadmap)がリリースされます。</span><span class="sxs-lookup"><span data-stu-id="8046c-124">At some future date, there will be an [ASP.NET Core 3.0 version](https://github.com/aspnet/Home/wiki/Roadmap).</span></span> <span data-ttu-id="8046c-125">3.0 バージョンでは、互換性スイッチによってサポートされている古い動作は削除されます。</span><span class="sxs-lookup"><span data-stu-id="8046c-125">Old behaviors supported by compatibility switches will be removed in the 3.0 version.</span></span> <span data-ttu-id="8046c-126">これらの正の変更は、ほぼすべてのユーザーにとってメリットとなると感じています。</span><span class="sxs-lookup"><span data-stu-id="8046c-126">We feel these are positive changes benefitting nearly all users.</span></span> <span data-ttu-id="8046c-127">これらの変更を導入することにより、ほとんどのアプリでメリットを得られるようになり、またその他のユーザーにはアプリをアップデートするための時間ができます。</span><span class="sxs-lookup"><span data-stu-id="8046c-127">By introducing these changes now, most apps can benefit now, and the others will have time to update their apps.</span></span>
