---
title: "環境タグ ヘルパーの ASP.NET Core"
author: pkellner
description: "すべてのプロパティを含む ASP.Net Core 環境タグ ヘルパーの定義"
keywords: "ASP.NET Core、タグ ヘルパー"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper
ms.openlocfilehash: 5870d9ebd02becf29f892c91310022d3b9a6b7af
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="95a16-104">環境タグ ヘルパーの ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="95a16-104">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="95a16-105">によって[Peter Kellner](http://peterkellner.net)と[Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="95a16-105">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="95a16-106">環境タグ ヘルパーは、現在のホスト環境に基づいて囲まれたコンテンツを条件付きで表示します。</span><span class="sxs-lookup"><span data-stu-id="95a16-106">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="95a16-107">その 1 つの属性`names`環境のコンマ区切り一覧を示します、された名前と現在の環境に一致するいずれかの場合は、かっこで囲んだ表示される内容がトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="95a16-107">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="95a16-108">環境タグ ヘルパー属性</span><span class="sxs-lookup"><span data-stu-id="95a16-108">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="95a16-109">名前</span><span class="sxs-lookup"><span data-stu-id="95a16-109">names</span></span>

<span data-ttu-id="95a16-110">1 つのホスティング環境名またはホスティング収録されているコンテンツのレンダリングをトリガーする環境名のコンマ区切りの一覧を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="95a16-110">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="95a16-111">これらの値は、ASP.NET Core の静的プロパティから返された現在の値と比較されます`HostingEnvironment.EnvironmentName`です。</span><span class="sxs-lookup"><span data-stu-id="95a16-111">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="95a16-112">この値は、次のいずれかの:**ステージング**です。**開発**または**運用**です。</span><span class="sxs-lookup"><span data-stu-id="95a16-112">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="95a16-113">比較では、小文字を無視します。</span><span class="sxs-lookup"><span data-stu-id="95a16-113">The comparison ignores case.</span></span>

<span data-ttu-id="95a16-114">有効な例`environment`タグ ヘルパーは。</span><span class="sxs-lookup"><span data-stu-id="95a16-114">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="95a16-115">属性を含めたり除外したり</span><span class="sxs-lookup"><span data-stu-id="95a16-115">include and exclude attributes</span></span>

<span data-ttu-id="95a16-116">ASP.NET Core 2.x の追加、 `include`  &  `exclude`属性。</span><span class="sxs-lookup"><span data-stu-id="95a16-116">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="95a16-117">属性のコントロールが含まれるか除外されたホスティング環境名に基づいて含まれているコンテンツのレンダリングこれらです。</span><span class="sxs-lookup"><span data-stu-id="95a16-117">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="95a16-118">ASP.NET Core 2.0 以降が含まれます</span><span class="sxs-lookup"><span data-stu-id="95a16-118">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="95a16-119">`include`プロパティの同様の動作には、 `names` ASP.NET Core 1.0 での属性です。</span><span class="sxs-lookup"><span data-stu-id="95a16-119">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="95a16-120">ASP.NET Core 2.0 以降を除外します。</span><span class="sxs-lookup"><span data-stu-id="95a16-120">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="95a16-121">これに対し、`exclude`プロパティにより、`EnvironmentTagHelper`指定したそのを除くすべてのホスティング環境の名前をかっこで囲んだコンテンツをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="95a16-121">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="95a16-122">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="95a16-122">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>