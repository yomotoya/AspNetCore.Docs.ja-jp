---
title: Razor コンポーネント フォームと検証
author: guardrex
description: フォームおよびフィールドの検証シナリオを Razor コンポーネントで使用する方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/forms-validation
ms.openlocfilehash: a4ed75efc6b5a733ce4ff4e82f354a8e2fd48500
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515667"
---
# <a name="razor-components-forms-and-validation"></a><span data-ttu-id="2b620-103">Razor コンポーネント フォームと検証</span><span class="sxs-lookup"><span data-stu-id="2b620-103">Razor Components forms and validation</span></span>

<span data-ttu-id="2b620-104">作成者: [Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2b620-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2b620-105">フォームと検証を使用して Razor コンポーネントでサポートされます[データ注釈](xref:mvc/models/validation)します。</span><span class="sxs-lookup"><span data-stu-id="2b620-105">Forms and validation are supported in Razor Components using [data annotations](xref:mvc/models/validation).</span></span>

> [!NOTE]
> <span data-ttu-id="2b620-106">フォームと検証のシナリオでは、各プレビュー リリースで変更する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2b620-106">Forms and validation scenarios are likely to change with each preview release.</span></span>

<span data-ttu-id="2b620-107">次`ExampleModel`型データの注釈を使用して検証ロジックを定義します。</span><span class="sxs-lookup"><span data-stu-id="2b620-107">The following `ExampleModel` type defines validation logic using data annotations:</span></span>

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

<span data-ttu-id="2b620-108">使用してフォームを定義、`<EditForm>`コンポーネント。</span><span class="sxs-lookup"><span data-stu-id="2b620-108">A form is defined using the `<EditForm>` component.</span></span> <span data-ttu-id="2b620-109">次の形式では、一般的な要素、コンポーネント、および Razor コードを示しています。</span><span class="sxs-lookup"><span data-stu-id="2b620-109">The following form demonstrates typical elements, components, and Razor code:</span></span>

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" bind-Value="@exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@functions {
    private ExampleModel exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

* <span data-ttu-id="2b620-110">フォームでのユーザー入力の検証、`name`フィールドで定義されている検証を使用して、`ExampleModel`型。</span><span class="sxs-lookup"><span data-stu-id="2b620-110">The form validates user input in the `name` field using the validation defined in the `ExampleModel` type.</span></span> <span data-ttu-id="2b620-111">コンポーネントのモデルが作成された`@functions`をブロックし、プライベート フィールドに保持されている (`exampleModel`)。</span><span class="sxs-lookup"><span data-stu-id="2b620-111">The model is created in the component's `@functions` block and held in a private field (`exampleModel`).</span></span> <span data-ttu-id="2b620-112">フィールドに割り当てられていますが、`Model`の属性、`<EditForm>`します。</span><span class="sxs-lookup"><span data-stu-id="2b620-112">The field is assigned to the `Model` attribute of the `<EditForm>`.</span></span>
* <span data-ttu-id="2b620-113">データ注釈検証コントロール コンポーネント (`<DataAnnotationsValidator>`) データ注釈を使用して検証のサポートをアタッチします。</span><span class="sxs-lookup"><span data-stu-id="2b620-113">The Data Annotations Validator component (`<DataAnnotationsValidator>`) attaches validation support using data annotations.</span></span>
* <span data-ttu-id="2b620-114">検証の概要のコンポーネント (`<ValidationSummary>`) 検証メッセージをまとめたものです。</span><span class="sxs-lookup"><span data-stu-id="2b620-114">The Validation Summary component (`<ValidationSummary>`) summarizes validation messages.</span></span>
* `HandleValidSubmit` <span data-ttu-id="2b620-115">フォーム (パスの検証) を正常に送信するときにトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="2b620-115">is triggered when the form successfully submits (passes validation).</span></span>

<span data-ttu-id="2b620-116">組み込みの入力コンポーネントのセットを受信し、ユーザー入力の検証を利用できます。</span><span class="sxs-lookup"><span data-stu-id="2b620-116">A set of built-in input components are available to receive and validate user input.</span></span> <span data-ttu-id="2b620-117">フォームが送信されたときに、これらを変更しているときに、入力が検証されます。</span><span class="sxs-lookup"><span data-stu-id="2b620-117">Inputs are validated when they're changed and when a form is submitted.</span></span> <span data-ttu-id="2b620-118">使用可能な入力コンポーネントは、次の表に表示されます。</span><span class="sxs-lookup"><span data-stu-id="2b620-118">Available input components are shown in the following table.</span></span>

| <span data-ttu-id="2b620-119">コンポーネントの入力</span><span class="sxs-lookup"><span data-stu-id="2b620-119">Input component</span></span>   | <span data-ttu-id="2b620-120">としてレンダリングされます。&hellip;</span><span class="sxs-lookup"><span data-stu-id="2b620-120">Rendered as&hellip;</span></span>       |
| ----------------- | ------------------------- |
| `<InputText>`     | `<input>`                 |
| `<InputTextArea>` | `<textarea>`              |
| `<InputSelect>`   | `<select>`                |
| `<InputNumber>`   | `<input type="number">`   |
| `<InputCheckbox>` | `<input type="checkbox">` |
| `<InputDate>`     | `<input type="date">`     |

<span data-ttu-id="2b620-121">入力のコンポーネントは、編集時に検証し、フィールドの状態を反映するように、CSS クラスを変更するための既定の動作を提供します。</span><span class="sxs-lookup"><span data-stu-id="2b620-121">Input components provide default behavior for validating on edit and changing their CSS class to reflect the field state.</span></span> <span data-ttu-id="2b620-122">一部のコンポーネントには、有効な解析ロジックが含まれます。</span><span class="sxs-lookup"><span data-stu-id="2b620-122">Some components include useful parsing logic.</span></span> <span data-ttu-id="2b620-123">たとえば、`<InputDate>`と`<InputNumber>`検証エラーとして登録することにより、解析不能な値を適切に処理します。</span><span class="sxs-lookup"><span data-stu-id="2b620-123">For example, `<InputDate>` and `<InputNumber>` handle unparseable values gracefully by registering them as validation errors.</span></span> <span data-ttu-id="2b620-124">Null 値を受け入れることができる型もサポートして、ターゲット フィールドの null 値許容属性 (たとえば、 `int?`)。</span><span class="sxs-lookup"><span data-stu-id="2b620-124">Types that can accept null values also support nullability of the target field (for example, `int?`).</span></span>

<span data-ttu-id="2b620-125">次`Starship`型では、大規模な一連のプロパティを使用して検証ロジックを定義しますおよび[データ注釈](xref:mvc/models/validation)、以前より`ExampleModel`:。</span><span class="sxs-lookup"><span data-stu-id="2b620-125">The following `Starship` type defines validation logic using a larger set of properties and [data annotations](xref:mvc/models/validation) than the earlier `ExampleModel`:</span></span>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [Required]
    [StringLength(16, 
        ErrorMessage = "Identifier too long (16 character limit).")]
    public string Identifier { get; set; }

    // Optional (no data annotations)
    public string Description { get; set; }

    [Required]
    public string Classification { get; set; }

    [Range(1, 100000, ErrorMessage = "Accommodation invalid (1-100000).")]
    public int MaximumAccommodation { get; set; }

    [Required]
    [Range(typeof(bool), "true", "true", 
        ErrorMessage = "Form disallowed for unapproved ships.")]
    public bool IsValidatedDesign { get; set; }

    [Required]
    public DateTime ProductionDate { get; set; }
}
```

<span data-ttu-id="2b620-126">次の形式で定義されている検証を使用してユーザー入力の検証、`Starship`モデル。</span><span class="sxs-lookup"><span data-stu-id="2b620-126">The following form validates user input using the validation defined in the `Starship` model:</span></span>

```cshtml
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@starship" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label for="identifier">Identifier: </label>
        <InputText id="identifier" bind-Value="@starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea Id="description" bind-Value="@starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" bind-Value="@starship.Classification">
            <option value"">Select classification ...</option>
            <option value="Defense">Defense</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
        </InputSelect>
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" 
            bind-Value="@starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" bind-Value="@starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate Id="productionDate" bind-Value="@starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="http://www.paramount.com">Paramount Pictures</a>
    </p>
</EditForm>

@functions {
    private Starship starship = new Starship();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

<span data-ttu-id="2b620-127">`<EditForm>`作成、`EditContext`として、[カスケード値](xref:razor-components/components#cascading-values-and-parameters)編集プロセスは、どのようなフィールドが変更されたを含むに関するメタデータおよび現在の検証メッセージを追跡します。</span><span class="sxs-lookup"><span data-stu-id="2b620-127">The `<EditForm>` creates an `EditContext` as a [cascading value](xref:razor-components/components#cascading-values-and-parameters) that tracks metadata about the edit process, including what fields have been modified and the current validation messages.</span></span> <span data-ttu-id="2b620-128">`<EditForm>`も便利なイベントは、有効と無効の送信の (`OnValidSubmit`、 `OnInvalidSubmit`)。</span><span class="sxs-lookup"><span data-stu-id="2b620-128">The `<EditForm>` also provides convenient events for valid and invalid submits (`OnValidSubmit`, `OnInvalidSubmit`).</span></span> <span data-ttu-id="2b620-129">また、使用して`OnSubmit`検証をトリガーし、カスタム検証コードを含むフィールドの値を確認します。</span><span class="sxs-lookup"><span data-stu-id="2b620-129">Alternatively, use `OnSubmit` to trigger the validation and check field values with custom validation code.</span></span>

<span data-ttu-id="2b620-130">データ注釈検証コントロール コンポーネント (`<DataAnnotationsValidator>`) に、カスケード データ注釈を使用する検証のサポートをアタッチします`EditContext`します。</span><span class="sxs-lookup"><span data-stu-id="2b620-130">The Data Annotations Validator component (`<DataAnnotationsValidator>`) attaches validation support using data annotations to the cascaded `EditContext`.</span></span> <span data-ttu-id="2b620-131">この明示的なジェスチャでは、現在のデータ注釈を使用して検証のサポートを有効にする必要がありますが、これをオーバーライドし、既定の動作を行うことを検討しています。</span><span class="sxs-lookup"><span data-stu-id="2b620-131">Enabling support for validation using data annotations currently requires this explicit gesture, but we're considering making this the default behavior that you can then override.</span></span> <span data-ttu-id="2b620-132">データ注釈よりも異なる検証システムを使用するには、データ注釈検証コントロールをカスタム実装に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2b620-132">To use a different validation system than data annotations, replace the Data Annotations Validator with a custom implementation.</span></span> <span data-ttu-id="2b620-133">ASP.NET Core の実装では、参照ソースで検査できます。[DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs)します。</span><span class="sxs-lookup"><span data-stu-id="2b620-133">The ASP.NET Core implementation is available for inspection in the reference source: [DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs).</span></span> *<span data-ttu-id="2b620-134">ASP.NET Core の実装は、プレビュー リリースの期間中の迅速な更新プログラムが適用されます。</span><span class="sxs-lookup"><span data-stu-id="2b620-134">The ASP.NET Core implementation is subject to rapid updates during the preview release period.</span></span>*

<span data-ttu-id="2b620-135">検証の概要のコンポーネント (`<ValidationSummary>`) に似ていますが、すべての検証メッセージをまとめたものです、[検証概要タグ ヘルパー](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)します。</span><span class="sxs-lookup"><span data-stu-id="2b620-135">The Validation Summary component (`<ValidationSummary>`) summarizes all validation messages, which is similar to the [Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper).</span></span>

<span data-ttu-id="2b620-136">検証メッセージ コンポーネント (`<ValidationMessage>`) に似ていますが、特定のフィールドの検証メッセージが表示されます、[検証メッセージ タグ ヘルパー](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)します。</span><span class="sxs-lookup"><span data-stu-id="2b620-136">The Validation Message component (`<ValidationMessage>`) displays validation messages for a specific field, which is similar to the [Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper).</span></span> <span data-ttu-id="2b620-137">検証でフィールドを指定します、`For`属性とモデルのプロパティの名前を付け、ラムダ式。</span><span class="sxs-lookup"><span data-stu-id="2b620-137">Specify the field for validation with the `For` attribute and a lambda expression naming the model property:</span></span>

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

> [!NOTE]
> <span data-ttu-id="2b620-138">組み込みの入力コンポーネントには、将来のリリースで解決する予定の制限があります。</span><span class="sxs-lookup"><span data-stu-id="2b620-138">Built-in input components have limitations that we expect to resolve in future releases.</span></span> <span data-ttu-id="2b620-139">たとえばで、生成された任意の属性を指定することはできません`<input>`タグ。</span><span class="sxs-lookup"><span data-stu-id="2b620-139">For example, you can't specify arbitrary attributes on the generated `<input>` tags.</span></span> <span data-ttu-id="2b620-140">使用できないシナリオに対応するコンポーネント サブクラスを独自に作成します。</span><span class="sxs-lookup"><span data-stu-id="2b620-140">Build your own component subclasses to handle unavailable scenarios.</span></span>
