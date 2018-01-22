---
title: "環境タグ ヘルパーの ASP.NET Core"
author: pkellner
description: "すべてのプロパティを含む ASP.NET Core 環境タグ ヘルパーの定義"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 32646f1fdaf840f796da1ec573459157a41a86d1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="0e7fa-103">環境タグ ヘルパーの ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e7fa-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="0e7fa-104">によって[Peter Kellner](http://peterkellner.net)と[Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="0e7fa-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="0e7fa-105">環境タグ ヘルパーは、現在のホスト環境に基づいて囲まれたコンテンツを条件付きで表示します。</span><span class="sxs-lookup"><span data-stu-id="0e7fa-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="0e7fa-106">その 1 つの属性`names`環境のコンマ区切り一覧を示します、された名前と現在の環境に一致するいずれかの場合は、かっこで囲んだ表示される内容がトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="0e7fa-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="0e7fa-107">環境タグ ヘルパー属性</span><span class="sxs-lookup"><span data-stu-id="0e7fa-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="0e7fa-108">名前</span><span class="sxs-lookup"><span data-stu-id="0e7fa-108">names</span></span>

<span data-ttu-id="0e7fa-109">1 つのホスティング環境名またはホスティング収録されているコンテンツのレンダリングをトリガーする環境名のコンマ区切りの一覧を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="0e7fa-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="0e7fa-110">これらの値は、ASP.NET Core の静的プロパティから返された現在の値と比較されます`HostingEnvironment.EnvironmentName`です。</span><span class="sxs-lookup"><span data-stu-id="0e7fa-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="0e7fa-111">この値は、次のいずれかの:**ステージング**です。**開発**または**運用**です。</span><span class="sxs-lookup"><span data-stu-id="0e7fa-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="0e7fa-112">比較では、小文字を無視します。</span><span class="sxs-lookup"><span data-stu-id="0e7fa-112">The comparison ignores case.</span></span>

<span data-ttu-id="0e7fa-113">有効な例`environment`タグ ヘルパーは。</span><span class="sxs-lookup"><span data-stu-id="0e7fa-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="0e7fa-114">属性を含めたり除外したり</span><span class="sxs-lookup"><span data-stu-id="0e7fa-114">include and exclude attributes</span></span>

<span data-ttu-id="0e7fa-115">ASP.NET Core 2.x の追加、 `include`  &  `exclude`属性。</span><span class="sxs-lookup"><span data-stu-id="0e7fa-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="0e7fa-116">属性のコントロールが含まれるか除外されたホスティング環境名に基づいて含まれているコンテンツのレンダリングこれらです。</span><span class="sxs-lookup"><span data-stu-id="0e7fa-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="0e7fa-117">ASP.NET Core 2.0 以降が含まれます</span><span class="sxs-lookup"><span data-stu-id="0e7fa-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="0e7fa-118">`include`プロパティの同様の動作には、 `names` ASP.NET Core 1.0 での属性です。</span><span class="sxs-lookup"><span data-stu-id="0e7fa-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="0e7fa-119">ASP.NET Core 2.0 以降を除外します。</span><span class="sxs-lookup"><span data-stu-id="0e7fa-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="0e7fa-120">これに対し、`exclude`プロパティにより、`EnvironmentTagHelper`指定したそのを除くすべてのホスティング環境の名前をかっこで囲んだコンテンツをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="0e7fa-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="0e7fa-121">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="0e7fa-121">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
