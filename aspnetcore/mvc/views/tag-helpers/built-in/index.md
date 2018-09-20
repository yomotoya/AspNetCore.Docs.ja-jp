---
title: ASP.NET Core の組み込みタグ ヘルパー
author: pkellner
description: タグ ヘルパーに組み込まれた ASP.NET Core によって生産性がどのように向上するかをご確認ください。
ms.author: riande
ms.date: 09/18/2018
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 5d9425e0b68578c86a6f9a691322e0bb63a860fb
ms.sourcegitcommit: c684eb6c0999d11d19e15e65939e5c7f99ba47df
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/19/2018
ms.locfileid: "46292311"
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="2a920-103">ASP.NET Core の組み込みタグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="2a920-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="2a920-104">著者: [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="2a920-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="2a920-105">ASP.NET Core には、生産性を向上させる組み込みタグ ヘルパーが多数含まれます。</span><span class="sxs-lookup"><span data-stu-id="2a920-105">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="2a920-106">このセクションでは、組み込みタグ ヘルパーの概要を説明します。</span><span class="sxs-lookup"><span data-stu-id="2a920-106">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="2a920-107">説明されていない組み込みタグ ヘルパーがありますが、これらは [Razor](xref:mvc/views/razor) ビュー エンジンによって内部的に使われているためです。</span><span class="sxs-lookup"><span data-stu-id="2a920-107">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="2a920-108">これには、Web サイトのルート パスに展開される ~ 文字のタグ ヘルパーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="2a920-108">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="2a920-109">組み込み ASP.NET Core タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="2a920-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="2a920-110">**[アンカー タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a920-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="2a920-111">**[キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a920-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="2a920-112">**[分散キャッシュ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a920-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="2a920-113">**[環境タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a920-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="2a920-114">**[フォーム タグ ヘルパー](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a920-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="2a920-115">**[イメージ タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a920-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="2a920-116">**[入力タグ ヘルパー](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a920-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="2a920-117">**[ラベル タグ ヘルパー](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a920-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="2a920-118">**[部分タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a920-118">**[Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span></span>

<span data-ttu-id="2a920-119">**[選択タグ ヘルパー](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a920-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="2a920-120">**[Textarea タグ ヘルパー](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a920-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="2a920-121">**[検証メッセージ タグ ヘルパー](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a920-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="2a920-122">**[検証概要タグ ヘルパー](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="2a920-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2a920-123">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="2a920-123">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/th-components>
