---
title: "ASP.NET Core の組み込みタグ ヘルパー"
author: pkellner
description: "ASP.NET Core の組み込みタグ ヘルパー"
keywords: "ASP.NET Core,タグ ヘルパー"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: e7c8c64283ca3740698300689b10497f984cfd3e
ms.sourcegitcommit: d022d4b96795ee473fa3847a1d8a8c7430423a86
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/13/2017
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="021b9-104">ASP.NET Core の組み込みタグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="021b9-104">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="021b9-105">著者: [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="021b9-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="021b9-106">ASP.NET Core には、生産性を向上させる組み込みタグ ヘルパーが多数含まれます。</span><span class="sxs-lookup"><span data-stu-id="021b9-106">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="021b9-107">このセクションでは、組み込みタグ ヘルパーの概要を説明します。</span><span class="sxs-lookup"><span data-stu-id="021b9-107">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="021b9-108">説明されていない組み込みタグ ヘルパーがありますが、これらは [Razor](xref:mvc/views/razor) ビュー エンジンによって内部的に使われているためです。</span><span class="sxs-lookup"><span data-stu-id="021b9-108">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="021b9-109">これには、Web サイトのルート パスに展開される ~ 文字のタグ ヘルパーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="021b9-109">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="021b9-110">組み込み ASP.NET Core タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="021b9-110">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="021b9-111">**[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="021b9-111">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper)**</span></span>

<span data-ttu-id="021b9-112">**[キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/CacheTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="021b9-112">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/CacheTagHelper)**</span></span>

<span data-ttu-id="021b9-113">**[分散キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="021b9-113">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper)**</span></span>

<span data-ttu-id="021b9-114">**[環境タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="021b9-114">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/FormActionTagHelper)**

<span data-ttu-id="021b9-115">**[フォーム タグ ヘルパー](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="021b9-115">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="021b9-116">**[イメージ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/ImageTagHelper)**</span><span class="sxs-lookup"><span data-stu-id="021b9-116">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/ImageTagHelper)**</span></span>

<span data-ttu-id="021b9-117">**[入力タグ ヘルパー](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="021b9-117">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="021b9-118">**[ラベル タグ ヘルパー](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="021b9-118">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/LinkTagHelper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/OptionTagHelper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/ScriptTagTagHelper)**

<span data-ttu-id="021b9-119">**[選択タグ ヘルパー](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="021b9-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="021b9-120">**[Textarea タグ ヘルパー](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="021b9-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="021b9-121">**[検証メッセージ タグ ヘルパー](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="021b9-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="021b9-122">**[検証概要タグ ヘルパー](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="021b9-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="021b9-123">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="021b9-123">Additional resources</span></span>

* [<span data-ttu-id="021b9-124">クライアント側の開発</span><span class="sxs-lookup"><span data-stu-id="021b9-124">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="021b9-125">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="021b9-125">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
