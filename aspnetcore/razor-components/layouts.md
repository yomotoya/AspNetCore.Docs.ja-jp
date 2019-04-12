---
title: Razor のコンポーネントのレイアウト
author: guardrex
description: Razor コンポーネント向けのレイアウトを再利用可能なコンポーネントを作成する方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/layouts
ms.openlocfilehash: 31ed940ce40e3ae6e3744418cf241d396308f4fe
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515643"
---
# <a name="razor-components-layouts"></a><span data-ttu-id="46308-103">Razor のコンポーネントのレイアウト</span><span class="sxs-lookup"><span data-stu-id="46308-103">Razor Components layouts</span></span>

<span data-ttu-id="46308-104">によって[Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="46308-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="46308-105">通常、アプリには、1 つ以上のコンポーネントが含まれます。</span><span class="sxs-lookup"><span data-stu-id="46308-105">Apps typically contain more than one component.</span></span> <span data-ttu-id="46308-106">メニューの著作権のメッセージ、およびロゴなどのレイアウト要素は、すべてのコンポーネントに存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="46308-106">Layout elements, such as menus, copyright messages, and logos, must be present on all components.</span></span> <span data-ttu-id="46308-107">すべてのアプリのコンポーネントにこれらのレイアウト要素のコードをコピー、効率的な方法はありません。</span><span class="sxs-lookup"><span data-stu-id="46308-107">Copying the code of these layout elements into all of the components of an app isn't an efficient approach.</span></span> <span data-ttu-id="46308-108">このような重複では、管理するが難しいと、おそらく時間の経過と共に一貫性のないコンテンツにつながります。</span><span class="sxs-lookup"><span data-stu-id="46308-108">Such duplication is hard to maintain and probably leads to inconsistent content over time.</span></span> <span data-ttu-id="46308-109">*レイアウト*この問題を解決します。</span><span class="sxs-lookup"><span data-stu-id="46308-109">*Layouts* solve this problem.</span></span>

<span data-ttu-id="46308-110">技術的には、レイアウトには、もう 1 つのコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="46308-110">Technically, a layout is just another component.</span></span> <span data-ttu-id="46308-111">Razor テンプレートまたはレイアウトが定義されているC#コードし、含めることができます[データ バインディング](xref:razor-components/components#data-binding)、[依存関係の注入](xref:razor-components/dependency-injection)、およびその他のコンポーネントの通常の機能。</span><span class="sxs-lookup"><span data-stu-id="46308-111">A layout is defined in a Razor template or in C# code and can contain [data binding](xref:razor-components/components#data-binding), [dependency injection](xref:razor-components/dependency-injection), and other ordinary features of components.</span></span>

<span data-ttu-id="46308-112">その他の 2 つの側面が有効にする、*コンポーネント*に、*レイアウト*</span><span class="sxs-lookup"><span data-stu-id="46308-112">Two additional aspects turn a *component* into a *layout*</span></span>

* <span data-ttu-id="46308-113">レイアウト コンポーネントが継承する必要があります`LayoutComponentBase`します。</span><span class="sxs-lookup"><span data-stu-id="46308-113">The layout component must inherit from `LayoutComponentBase`.</span></span> `LayoutComponentBase` <span data-ttu-id="46308-114">定義、`Body`レイアウト内にレンダリングされるコンテンツを含むプロパティ。</span><span class="sxs-lookup"><span data-stu-id="46308-114">defines a `Body` property that contains the content to be rendered inside the layout.</span></span>
* <span data-ttu-id="46308-115">レイアウト コンポーネントを使用して、`Body`プロパティを本文の内容が表示されるはずの場所を指定する、Razor 構文を使用してレンダリング`@Body`します。</span><span class="sxs-lookup"><span data-stu-id="46308-115">The layout component uses the `Body` property to specify where the body content should be rendered using the Razor syntax `@Body`.</span></span> <span data-ttu-id="46308-116">レンダリング中に`@Body`レイアウトのコンテンツで置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="46308-116">During rendering, `@Body` is replaced by the content of the layout.</span></span>

<span data-ttu-id="46308-117">レイアウト コンポーネントの Razor テンプレートを次のコード サンプルに示します。</span><span class="sxs-lookup"><span data-stu-id="46308-117">The following code sample shows the Razor template of a layout component.</span></span> <span data-ttu-id="46308-118">使用に注意してください`LayoutComponentBase`と`@Body`:</span><span class="sxs-lookup"><span data-stu-id="46308-118">Note the use of `LayoutComponentBase` and `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.cshtml)]

## <a name="use-a-layout-in-a-component"></a><span data-ttu-id="46308-119">コンポーネントでのレイアウトを使用します。</span><span class="sxs-lookup"><span data-stu-id="46308-119">Use a layout in a component</span></span>

<span data-ttu-id="46308-120">Razor ディレクティブを使用して、`@layout`コンポーネントにレイアウトを適用します。</span><span class="sxs-lookup"><span data-stu-id="46308-120">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="46308-121">コンパイラは変換には、このディレクティブを`LayoutAttribute`、これコンポーネント クラスに適用されます。</span><span class="sxs-lookup"><span data-stu-id="46308-121">The compiler converts this directive into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="46308-122">次のコード サンプルでは、概念を示します。</span><span class="sxs-lookup"><span data-stu-id="46308-122">The following code sample demonstrates the concept.</span></span> <span data-ttu-id="46308-123">このコンポーネントの内容が挿入、 *MasterLayout*の位置にある`@Body`:</span><span class="sxs-lookup"><span data-stu-id="46308-123">The content of this component is inserted into the *MasterLayout* at the position of `@Body`:</span></span>

```cshtml
@layout MasterLayout
@page "/master-list"

<h2>Master Episode List</h2>
```

## <a name="centralized-layout-selection"></a><span data-ttu-id="46308-124">一元的なレイアウトの選択</span><span class="sxs-lookup"><span data-stu-id="46308-124">Centralized layout selection</span></span>

<span data-ttu-id="46308-125">すべてのフォルダーのアプリは、という名前のテンプレート ファイルを含めることができます必要に応じて *_ViewImports.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="46308-125">Every folder of a an app can optionally contain a template file named *_ViewImports.cshtml*.</span></span> <span data-ttu-id="46308-126">コンパイラには、すべての同じフォルダーに Razor テンプレートとそのすべてのサブフォルダーで再帰的にビューのインポートのファイルで指定されたディレクティブが含まれています。</span><span class="sxs-lookup"><span data-stu-id="46308-126">The compiler includes the directives specified in the view imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="46308-127">そのため、 *_ViewImports.cshtml*ファイルを含む`@layout MainLayout`でフォルダー内のコンポーネントのすべてのこと、 *MainLayout*レイアウト。</span><span class="sxs-lookup"><span data-stu-id="46308-127">Therefore, a *_ViewImports.cshtml* file containing `@layout MainLayout` ensures that all of the components in a folder use the *MainLayout* layout.</span></span> <span data-ttu-id="46308-128">繰り返しを追加する必要はありません`@layout`のすべてに、 *.razor*ファイル。</span><span class="sxs-lookup"><span data-stu-id="46308-128">There's no need to repeatedly add `@layout` to all of the *.razor* files.</span></span>

<span data-ttu-id="46308-129">既定のテンプレートを使用している、 *_ViewImports.cshtml*レイアウトの選択するためのメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="46308-129">Note that the default template uses the *_ViewImports.cshtml* mechanism for layout selection.</span></span> <span data-ttu-id="46308-130">新しく作成されたアプリが含まれる、 *_ViewImports.cshtml*ファイル、*コンポーネント、またはページ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="46308-130">A newly created app contains the *_ViewImports.cshtml* file in the *Components/Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="46308-131">入れ子になったレイアウト</span><span class="sxs-lookup"><span data-stu-id="46308-131">Nested layouts</span></span>

<span data-ttu-id="46308-132">アプリは、入れ子になったレイアウトで構成できます。</span><span class="sxs-lookup"><span data-stu-id="46308-132">Apps can consist of nested layouts.</span></span> <span data-ttu-id="46308-133">コンポーネントは、さらに別のレイアウトを参照するレイアウトを参照できます。</span><span class="sxs-lookup"><span data-stu-id="46308-133">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="46308-134">たとえば、入れ子のレイアウトは、複数レベルのメニュー構造を反映するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="46308-134">For example, nesting layouts can be used to reflect a multi-level menu structure.</span></span>

<span data-ttu-id="46308-135">次のコード サンプルでは、入れ子になったレイアウトを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="46308-135">The following code samples show how to use nested layouts.</span></span> <span data-ttu-id="46308-136">*EpisodesComponent.cshtml*ファイルは、コンポーネントを表示します。</span><span class="sxs-lookup"><span data-stu-id="46308-136">The *EpisodesComponent.cshtml* file is the component to display.</span></span> <span data-ttu-id="46308-137">コンポーネントがレイアウトを参照することに注意してください`MasterListLayout`します。</span><span class="sxs-lookup"><span data-stu-id="46308-137">Note that the component references the layout `MasterListLayout`.</span></span>

<span data-ttu-id="46308-138">*EpisodesComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="46308-138">*EpisodesComponent.cshtml*:</span></span>

```cshtml
@layout MasterListLayout
@page "/master-list/episodes"

<h1>Episodes</h1>
```

<span data-ttu-id="46308-139">*MasterListLayout.cshtml*ファイルは、提供、`MasterListLayout`します。</span><span class="sxs-lookup"><span data-stu-id="46308-139">The *MasterListLayout.cshtml* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="46308-140">レイアウトは、別のレイアウトを参照`MasterLayout`埋め込まれる進んでいます。</span><span class="sxs-lookup"><span data-stu-id="46308-140">The layout references another layout, `MasterLayout`, where it's going to be embedded.</span></span>

<span data-ttu-id="46308-141">*MasterListLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="46308-141">*MasterListLayout.cshtml*:</span></span>

```cshtml
@layout MasterLayout
@inherits LayoutComponentBase

<nav>
    <!-- Menu structure of master list -->
    ...
</nav>

@Body
```

<span data-ttu-id="46308-142">最後に、`MasterLayout`ヘッダー、フッター、およびメイン メニューなど、最上位のレイアウト要素が含まれています。</span><span class="sxs-lookup"><span data-stu-id="46308-142">Finally, `MasterLayout` contains the top-level layout elements, such as the header, footer, and main menu.</span></span>

<span data-ttu-id="46308-143">*MasterLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="46308-143">*MasterLayout.cshtml*:</span></span>

```cshtml
@inherits LayoutComponentBase

<header>...</header>
<nav>...</nav>

@Body
```
