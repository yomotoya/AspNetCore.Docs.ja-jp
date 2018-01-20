---
title: "ASP.NET Core の組み込みタグ ヘルパー"
author: pkellner
description: "ASP.NET Core の組み込みタグ ヘルパー"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 8ffc748ec3d4eed35871543f5ceccc86aadee661
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="1be52-103">ASP.NET Core の組み込みタグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="1be52-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="1be52-104">著者: [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="1be52-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="1be52-105">ASP.NET Core には、生産性を向上させる組み込みタグ ヘルパーが多数含まれます。</span><span class="sxs-lookup"><span data-stu-id="1be52-105">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="1be52-106">このセクションでは、組み込みタグ ヘルパーの概要を説明します。</span><span class="sxs-lookup"><span data-stu-id="1be52-106">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="1be52-107">説明されていない組み込みタグ ヘルパーがありますが、これらは [Razor](xref:mvc/views/razor) ビュー エンジンによって内部的に使われているためです。</span><span class="sxs-lookup"><span data-stu-id="1be52-107">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="1be52-108">これには、Web サイトのルート パスに展開される ~ 文字のタグ ヘルパーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="1be52-108">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="1be52-109">組み込み ASP.NET Core タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="1be52-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="1be52-110">**[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1be52-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="1be52-111">**[キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1be52-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="1be52-112">**[分散キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1be52-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="1be52-113">**[環境タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1be52-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="1be52-114">**[フォーム タグ ヘルパー](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1be52-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="1be52-115">**[イメージ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1be52-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="1be52-116">**[入力タグ ヘルパー](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1be52-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="1be52-117">**[ラベル タグ ヘルパー](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1be52-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="1be52-118">**[選択タグ ヘルパー](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1be52-118">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="1be52-119">**[Textarea タグ ヘルパー](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1be52-119">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="1be52-120">**[検証メッセージ タグ ヘルパー](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1be52-120">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="1be52-121">**[検証概要タグ ヘルパー](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="1be52-121">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1be52-122">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="1be52-122">Additional resources</span></span>

* [<span data-ttu-id="1be52-123">クライアント側の開発</span><span class="sxs-lookup"><span data-stu-id="1be52-123">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="1be52-124">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="1be52-124">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
