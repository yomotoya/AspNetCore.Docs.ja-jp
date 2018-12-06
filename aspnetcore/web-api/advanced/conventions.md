---
title: Web API 規約を使用する
author: pranavkm
description: ASP.NET Core の Web API 規約について説明します。
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: ede9a46c160cf6a49aa93da710af0bf0b8f59acc
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710076"
---
# <a name="use-web-api-conventions"></a><span data-ttu-id="3653b-103">Web API 規約を使用する</span><span class="sxs-lookup"><span data-stu-id="3653b-103">Use web API conventions</span></span>

<span data-ttu-id="3653b-104">ASP.NET Core 2.2 で、一般的な [API 文書](xref:tutorials/web-api-help-pages-using-swagger)を抽出し、これを複数のアクション、コントローラー、またはアセンブリ内のすべてのコントローラーに適用する方法を導入しました。</span><span class="sxs-lookup"><span data-stu-id="3653b-104">ASP.NET Core 2.2 introduces a way to extract common [API documentation](xref:tutorials/web-api-help-pages-using-swagger) and apply it to multiple actions, controllers, or all controllers within an assembly.</span></span> <span data-ttu-id="3653b-105">Web API 規約は、[[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) による個々のアクションの装飾の代わりになるものです。</span><span class="sxs-lookup"><span data-stu-id="3653b-105">Web API conventions are a substitute for decorating individual actions with [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span></span> <span data-ttu-id="3653b-106">アクションに適用される規約のメソッドを選択して、アクションから戻る特に一般的な "従来の" 戻り値の型と状態コードを定義できるようになります。</span><span class="sxs-lookup"><span data-stu-id="3653b-106">It allows you to define the most common "conventional" return types and status codes that you return from your action with a way to select the convention method that applies to an action.</span></span>

<span data-ttu-id="3653b-107">既定では、ASP.NET Core MVC 2.2 には一連の既定の規約 `Microsoft.AspNetCore.Mvc.DefaultApiConventions` が付属しています。</span><span class="sxs-lookup"><span data-stu-id="3653b-107">By default, ASP.NET Core MVC 2.2 ships with a set of default conventions, `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span></span> <span data-ttu-id="3653b-108">この規約は、ASP.NET Core がスキャフォールディングするコントローラーに基づいています。</span><span class="sxs-lookup"><span data-stu-id="3653b-108">The conventions are based on the controller that ASP.NET Core scaffolds.</span></span> <span data-ttu-id="3653b-109">アクションが成果物をスキャフォールディングするパターンに従う場合は、既定の規約を使用すると成功します。</span><span class="sxs-lookup"><span data-stu-id="3653b-109">If your actions follow the pattern that scaffolding produces, you should be successful using the default conventions.</span></span>

<span data-ttu-id="3653b-110">実行時に、<xref:Microsoft.AspNetCore.Mvc.ApiExplorer> は規約を理解します。</span><span class="sxs-lookup"><span data-stu-id="3653b-110">At runtime, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> understands conventions.</span></span> <span data-ttu-id="3653b-111">`ApiExplorer` は Open API ドキュメントのジェネレーターと通信するために MVC を抽象化したものです。</span><span class="sxs-lookup"><span data-stu-id="3653b-111">`ApiExplorer` is MVC's abstraction to communicate with Open API document generators.</span></span> <span data-ttu-id="3653b-112">適用された規約の属性はアクションと関連付けられ、アクションの Swagger ドキュメントに含まれます。</span><span class="sxs-lookup"><span data-stu-id="3653b-112">Attributes from the applied convention get associated with an action and are included in the action's Swagger documentation.</span></span> <span data-ttu-id="3653b-113">API アナライザーも、規約を理解します。</span><span class="sxs-lookup"><span data-stu-id="3653b-113">API analyzers also understand conventions.</span></span> <span data-ttu-id="3653b-114">従来とは異なるアクションである場合 (適用されている規約で文書化されていない状態コードを返す場合など) は、文書化を促す警告が生成されます。</span><span class="sxs-lookup"><span data-stu-id="3653b-114">If your action is unconventional (for example, it returns a status code that isn't documented by the applied convention), it produces a warning, encouraging you to document it.</span></span>

<span data-ttu-id="3653b-115">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="3653b-115">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="apply-web-api-conventions"></a><span data-ttu-id="3653b-116">Web API 規約を適用する</span><span class="sxs-lookup"><span data-stu-id="3653b-116">Apply web API conventions</span></span>

<span data-ttu-id="3653b-117">規約を適用する方法は 3 つあります。</span><span class="sxs-lookup"><span data-stu-id="3653b-117">There are three ways to apply a convention.</span></span> <span data-ttu-id="3653b-118">規約では作成を行われません。</span><span class="sxs-lookup"><span data-stu-id="3653b-118">Conventions don't compose.</span></span> <span data-ttu-id="3653b-119">各アクションを 1 つの規約だけに関連付けることができます。</span><span class="sxs-lookup"><span data-stu-id="3653b-119">Each action may be associated with exactly one convention.</span></span> <span data-ttu-id="3653b-120">より具体的な規約 (詳細は下記) の方が優先されます。</span><span class="sxs-lookup"><span data-stu-id="3653b-120">More specific conventions (detailed below) take precedence over less specific ones.</span></span> <span data-ttu-id="3653b-121">優先順位が同じである 2 つ以上の規約が 1 つのアクションに適用されていると、選択は不明確になります。</span><span class="sxs-lookup"><span data-stu-id="3653b-121">The selection is non-deterministic when two or more conventions of the same priority apply to an action.</span></span> <span data-ttu-id="3653b-122">次のオプションは、規約をアクションに適用するためにあります。より具体的なものから並んでいます。</span><span class="sxs-lookup"><span data-stu-id="3653b-122">The following options exist to apply a convention to an action, from the most specific to the least specific:</span></span>

1. <span data-ttu-id="3653b-123">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; 個々のアクションに適用され、適用される規約の種類と規約のメソッドが指定されます。</span><span class="sxs-lookup"><span data-stu-id="3653b-123">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Applies to individual actions and specifies the convention type and the convention method that applies.</span></span> <span data-ttu-id="3653b-124">次の例では、規約のメソッド `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` が `Update` のアクションに適用されます。</span><span class="sxs-lookup"><span data-stu-id="3653b-124">In the following sample, the convention method `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` is applied to the `Update` action:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventionmethod&highlight=2-3)]

1. <span data-ttu-id="3653b-125">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` (コントローラーに適用) &mdash; 規約の種類がコントローラー上のすべてのアクションに適用されます。</span><span class="sxs-lookup"><span data-stu-id="3653b-125">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to a controller &mdash; Applies the convention type to all actions on the controller.</span></span> <span data-ttu-id="3653b-126">規約のメソッドは、どのアクションが適用されるかを決定するヒントで装飾されます (詳細は規約の作成に含まれています)。</span><span class="sxs-lookup"><span data-stu-id="3653b-126">Convention methods are decorated with hints that determine which actions it would apply to (details as part of authoring conventions).</span></span> <span data-ttu-id="3653b-127">例:</span><span class="sxs-lookup"><span data-stu-id="3653b-127">For example:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventiontypeattribute)]

1. <span data-ttu-id="3653b-128">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` (アセンブリに適用) &mdash; 規約の種類が現在のアセンブリのすべてのコントローラーに適用されます。</span><span class="sxs-lookup"><span data-stu-id="3653b-128">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to an assembly &mdash; Applies the convention type to all controllers in the current assembly.</span></span> <span data-ttu-id="3653b-129">例:</span><span class="sxs-lookup"><span data-stu-id="3653b-129">For example:</span></span>

    [!code-csharp[](conventions/sample/Startup.cs?name=apiconventiontypeattribute)]

## <a name="create-web-api-conventions"></a><span data-ttu-id="3653b-130">Web API 規約を作成する</span><span class="sxs-lookup"><span data-stu-id="3653b-130">Create web API conventions</span></span>

<span data-ttu-id="3653b-131">規約は、静的な種類でメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3653b-131">A convention is a static type with methods.</span></span> <span data-ttu-id="3653b-132">これらのメソッドには `[ProducesResponseType]` または `[ProducesDefaultResponseType]` 属性の注釈が付けられています。</span><span class="sxs-lookup"><span data-stu-id="3653b-132">These methods are annotated with `[ProducesResponseType]` or `[ProducesDefaultResponseType]` attributes.</span></span>

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

<span data-ttu-id="3653b-133">この規約をアセンブリに適用すると、より具体的なメタデータ属性がない限り、規約のメソッドが `Find` という名前のすべてのアクションに適用され、`id` という名前のパラメーターを 1 つだけ持つようになります。</span><span class="sxs-lookup"><span data-stu-id="3653b-133">Applying this convention to an assembly results in the convention method applying to any action with the name `Find` and having exactly one parameter named `id`, as long as they don't have other more specific metadata attributes.</span></span>

<span data-ttu-id="3653b-134">`[ProducesResponseType]` と `[ProducesDefaultResponseType]` だけでなく、`[ApiConventionNameMatch]` と `[ApiConventionTypeMatch]` も、適用されるメソッドを決定する規約のメソッドに適用することができます。</span><span class="sxs-lookup"><span data-stu-id="3653b-134">In addition to `[ProducesResponseType]` and `[ProducesDefaultResponseType]`, `[ApiConventionNameMatch]` and `[ApiConventionTypeMatch]` can be applied to the convention method that determines the methods they apply to.</span></span> <span data-ttu-id="3653b-135">例:</span><span class="sxs-lookup"><span data-stu-id="3653b-135">For example:</span></span>

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

* <span data-ttu-id="3653b-136">メソッドに適用された `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` オプションは、"Find" というプレフィックスがある限り、規約をどのアクションとも照合できることを示します。</span><span class="sxs-lookup"><span data-stu-id="3653b-136">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` option applied to the method, indicates that the convention can match any action as long as it's prefixed with "Find".</span></span> <span data-ttu-id="3653b-137">これには、`Find`、`FindPet`、`FindById` などのメソッドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="3653b-137">This includes methods such as `Find`, `FindPet`, or `FindById`.</span></span>
* <span data-ttu-id="3653b-138">パラメーターに適用された `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` は、サフィックス識別子で終わる 1 つのパラメーターが含まれるメソッドを規約と照合できることを示します。</span><span class="sxs-lookup"><span data-stu-id="3653b-138">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` applied to the parameter indicates that the convention can match methods with exactly one parameter ending in the suffix identifier.</span></span> <span data-ttu-id="3653b-139">これには、`id` や `petId` などのパラメーターが含まれます。</span><span class="sxs-lookup"><span data-stu-id="3653b-139">This includes parameters such as `id` or `petId`.</span></span> <span data-ttu-id="3653b-140">`ApiConventionTypeMatch` も同様に型に適用して、パラメーターの型に制約を設けることができます。</span><span class="sxs-lookup"><span data-stu-id="3653b-140">`ApiConventionTypeMatch` can be similarly applied to types to constrain the type of the parameter.</span></span> <span data-ttu-id="3653b-141">`params[]` の引数を使用すると、明示的に一致する必要がない残りのパラメーターを示すことができます。</span><span class="sxs-lookup"><span data-stu-id="3653b-141">A `params[]` argument can be used to indicate remaining parameters that don't need to be explicitly matched.</span></span>
