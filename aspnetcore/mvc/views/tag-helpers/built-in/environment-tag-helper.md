---
title: ASP.NET Core の環境タグ ヘルパー
author: pkellner
description: すべてのプロパティを含む、定義済みの ASP.NET Core 環境タグ ヘルパー
manager: wpickett
ms.author: riande
ms.date: 07/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 7a99ee0e59c7f49a3208d2c86c11cabce4294889
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
ms.locfileid: "28901184"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="2714e-103">ASP.NET Core の環境タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="2714e-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="2714e-104">著者: [Peter Kellner](http://peterkellner.net)、[Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="2714e-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="2714e-105">環境タグ ヘルパーは、現在のホスティング環境に基づき、囲まれたコンテンツを条件付きでレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="2714e-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="2714e-106">その単一の属性 `names` は環境名のコンマ区切りリストであり、現在の環境に一致した場合は囲まれたコンテンツのレンダリングがトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="2714e-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="2714e-107">環境タグ ヘルパーの属性</span><span class="sxs-lookup"><span data-stu-id="2714e-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="2714e-108">名前</span><span class="sxs-lookup"><span data-stu-id="2714e-108">names</span></span>

<span data-ttu-id="2714e-109">囲まれたコンテンツのレンダリングをトリガーする単一のホスティング環境名またはホスティング環境名のコンマ区切りリストを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="2714e-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="2714e-110">これらの値は、ASP.NET Core の静的プロパティ `HostingEnvironment.EnvironmentName` から返される現在の値と比較されます。</span><span class="sxs-lookup"><span data-stu-id="2714e-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="2714e-111">この値は、**Staging**、**Development** または **Production** のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="2714e-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="2714e-112">比較では大文字と小文字の区別は無視されます。</span><span class="sxs-lookup"><span data-stu-id="2714e-112">The comparison ignores case.</span></span>

<span data-ttu-id="2714e-113">有効な `environment` タグ ヘルパーの例を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="2714e-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="2714e-114">include および exclude 属性</span><span class="sxs-lookup"><span data-stu-id="2714e-114">include and exclude attributes</span></span>

<span data-ttu-id="2714e-115">ASP.NET Core 2.x では `include` & `exclude` 属性が追加されます。</span><span class="sxs-lookup"><span data-stu-id="2714e-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="2714e-116">これらの属性は、含めるまたは除外するホスティング環境名に基づいて、囲まれたコンテンツのレンダリングを制御します。</span><span class="sxs-lookup"><span data-stu-id="2714e-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="2714e-117">include (ASP.NET Core 2.0 以降)</span><span class="sxs-lookup"><span data-stu-id="2714e-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="2714e-118">`include` プロパティの動作は、ASP.NET Core 1.0 の `names` 属性の動作と同様です。</span><span class="sxs-lookup"><span data-stu-id="2714e-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="2714e-119">exclude (ASP.NET Core 2.0 以降)</span><span class="sxs-lookup"><span data-stu-id="2714e-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="2714e-120">一方、`exclude` プロパティでは、ユーザーが指定したものを除き、`EnvironmentTagHelper` ですべてのホスティング環境名の囲まれたコンテンツをレンダリングできます。</span><span class="sxs-lookup"><span data-stu-id="2714e-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="2714e-121">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="2714e-121">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
