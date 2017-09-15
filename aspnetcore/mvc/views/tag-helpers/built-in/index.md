---
title: "ASP.NET Core の組み込みタグ ヘルパー"
author: pkellner
description: "ASP.NET Core の組み込みタグ ヘルパー"
keywords: "ASP.NET Core,タグ ヘルパー"
ms.author: riande
manager: wpickett
ms.date: 7/11/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 3f47cc571eff0c522aaf6543de58f158835384d4
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="7fac3-104">ASP.NET Core の組み込みタグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="7fac3-104">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="7fac3-105">著者: [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="7fac3-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="7fac3-106">ASP.NET Core フレームワークには、堅牢なコードの作成の生産性を上げるのに役立つ多くのタグ ヘルパーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="7fac3-106">The ASP.NET Core framework includes many Tag Helpers that can help you be more productive in writing robust code.</span></span> <span data-ttu-id="7fac3-107">このセクションでは、すべての組み込みタグ ヘルパーの概要を説明します。</span><span class="sxs-lookup"><span data-stu-id="7fac3-107">This section provides an overview of all the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="7fac3-108">説明されていない組み込みタグ ヘルパーがありますが、これらは [Razor](xref:mvc/views/razor) ビュー エンジンによって内部的に使われているためです。</span><span class="sxs-lookup"><span data-stu-id="7fac3-108">There are built-in Tag Helpers which are not discussed, since they are used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="7fac3-109">これには、Web サイトのルート パスに展開される ~ 文字のタグ ヘルパーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="7fac3-109">This includes a Tag Helper for the ~ character which expands to the root path of the web site.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="7fac3-110">組み込み ASP.NET Core タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="7fac3-110">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="7fac3-111">**[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="7fac3-111">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper)**</span></span>

<span data-ttu-id="7fac3-112">**[キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/CacheTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="7fac3-112">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/CacheTagHelper)**</span></span>

<span data-ttu-id="7fac3-113">**[分散キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="7fac3-113">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper)**</span></span>

<span data-ttu-id="7fac3-114">**[環境タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="7fac3-114">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/FormActionTagHelper)**

[comment]: **[FormTagTagHelper](xref:mvc/views/tag-helpers/builtin-th/FormTagHelper)**

<span data-ttu-id="7fac3-115">**[イメージ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/ImageTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="7fac3-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/ImageTagHelper)**</span></span>

[comment]: **[InputTagHelper](xref:mvc/views/tag-helpers/builtin-th/InputTagHelper)**

[comment]: **[LabelTagHelper](xref:mvc/views/tag-helpers/builtin-th/LabelTagHelper)**

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/LinkTagHelper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/OptionTagHelper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/ScriptTagTagHelper)**

[comment]: **[SelectTagHelper](xref:mvc/views/tag-helpers/builtin-th/SelectTagTagHelper)**

[comment]: **[TextAreaTagHelper](xref:mvc/views/tag-helpers/builtin-th/TextAreaTagHelper)**

[comment]: **[ValidationMessageTagHelper](xref:mvc/views/tag-helpers/builtin-th/ValidationMessageTagHelper)**

[comment]: **[ValidationSummaryTagHelper](xref:mvc/views/tag-helpers/builtin-th/ValidationSummaryTagHelper)**  
  
  
<!--

## Additional Resources

REQUIRED These must be xref links, not relative, that is ../../
* [Client-Side Development](../../../client-side/index.md)

* [Tag Helpers](xref:mvc/views/tag-helpers/intro)
-->
