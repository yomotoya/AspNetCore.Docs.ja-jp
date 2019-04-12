---
title: Razor のコンポーネントのクラス ライブラリ
author: guardrex
description: コンポーネントを含める、外部コンポーネント ライブラリの Razor コンポーネント アプリにする方法を説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 1064ad60d90af15af483ba9bca5ed85fb63c2924
ms.sourcegitcommit: 10e14b85490f064395e9b2f423d21e3c2d39ed8b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/19/2019
ms.locfileid: "59515463"
---
# <a name="razor-components-class-libraries"></a><span data-ttu-id="53c8f-103">Razor のコンポーネントのクラス ライブラリ</span><span class="sxs-lookup"><span data-stu-id="53c8f-103">Razor Components Class Libraries</span></span>

<span data-ttu-id="53c8f-104">によって[Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="53c8f-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="53c8f-105">コンポーネントは、Razor クラス ライブラリ プロジェクト間で共有できます。</span><span class="sxs-lookup"><span data-stu-id="53c8f-105">Components can be shared in Razor class libraries across projects.</span></span> <span data-ttu-id="53c8f-106">コンポーネントから指定できます。</span><span class="sxs-lookup"><span data-stu-id="53c8f-106">Components can be included from:</span></span>

* <span data-ttu-id="53c8f-107">ソリューション内の別のプロジェクト。</span><span class="sxs-lookup"><span data-stu-id="53c8f-107">Another project in the solution.</span></span>
* <span data-ttu-id="53c8f-108">NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="53c8f-108">A NuGet package.</span></span>
* <span data-ttu-id="53c8f-109">参照先の .NET ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="53c8f-109">A referenced .NET library.</span></span>

<span data-ttu-id="53c8f-110">コンポーネントが通常の .NET 型であるのと同様、Razor クラス ライブラリによって提供されるコンポーネントは、通常の .NET アセンブリをします。</span><span class="sxs-lookup"><span data-stu-id="53c8f-110">Just as components are regular .NET types, components provided by Razor class libraries are normal .NET assemblies.</span></span>

<span data-ttu-id="53c8f-111">使用して、 `razorclasslib` (Razor クラス ライブラリ) テンプレートと、[新しい dotnet](/dotnet/core/tools/dotnet-new)コマンド。</span><span class="sxs-lookup"><span data-stu-id="53c8f-111">Use the `razorclasslib` (Razor class library) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command:</span></span>

```console
dotnet new razorclasslib -o MyComponentLib1
```

<span data-ttu-id="53c8f-112">コンポーネントの Razor ファイルの追加 (*.razor*) Razor クラス ライブラリにします。</span><span class="sxs-lookup"><span data-stu-id="53c8f-112">Add Razor Component files (*.razor*) to the Razor class library.</span></span>

<span data-ttu-id="53c8f-113">既存のプロジェクトにライブラリを追加するには、使用、 [dotnet sln](/dotnet/core/tools/dotnet-sln)コマンド。</span><span class="sxs-lookup"><span data-stu-id="53c8f-113">To add the library to an existing project, use the [dotnet sln](/dotnet/core/tools/dotnet-sln) command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="53c8f-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="53c8f-114">Visual Studio</span></span>](#tab/visual-studio)

```console
dotnet sln add .\MyComponentLib1
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="53c8f-115">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="53c8f-115">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet add WebApplication1 reference MyComponentLib1
```

---

> [!NOTE]
> <span data-ttu-id="53c8f-116">Razor クラス ライブラリは、ASP.NET Core の Preview 3 で Blazor アプリと互換性がありません。</span><span class="sxs-lookup"><span data-stu-id="53c8f-116">Razor class libraries aren't compatible with Blazor apps in ASP.NET Core Preview 3.</span></span>
>
> <span data-ttu-id="53c8f-117">Blazor と剃刀コンポーネント アプリで共有できるライブラリのコンポーネントを作成するには、によって作成された Blazor クラス ライブラリを使用、`blazorlib`テンプレート。</span><span class="sxs-lookup"><span data-stu-id="53c8f-117">To create components in a library that can be shared with Blazor and Razor Components apps, use a Blazor class library created by the `blazorlib` template.</span></span>
>
> <span data-ttu-id="53c8f-118">Razor クラス ライブラリは、静的なアセットを ASP.NET Core の Preview 3 でサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="53c8f-118">Razor class libraries don't support static assets in ASP.NET Core Preview 3.</span></span> <span data-ttu-id="53c8f-119">コンポーネントのライブラリを使用して、`blazorlib`テンプレートは、イメージ、JavaScript、およびスタイル シートなどの静的ファイルを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="53c8f-119">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="53c8f-120">ビルド時に、静的ファイルがビルドされたアセンブリ ファイルに埋め込まれる (*.dll*)、そのリソースを追加する方法の詳細について心配することがなく、コンポーネントの消費量をことができます。</span><span class="sxs-lookup"><span data-stu-id="53c8f-120">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="53c8f-121">含まれるすべてのファイル、`content`ディレクトリは、埋め込みリソースとしてマークされます。</span><span class="sxs-lookup"><span data-stu-id="53c8f-121">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="53c8f-122">ライブラリのコンポーネントを使用します。</span><span class="sxs-lookup"><span data-stu-id="53c8f-122">Consume a library component</span></span>

<span data-ttu-id="53c8f-123">別のプロジェクトでのライブラリで定義されているコンポーネントを使用するために、 [ @addTagHelper ](xref:mvc/views/tag-helpers/intro#add-helper-label)ディレクティブを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="53c8f-123">In order to consume components defined in a library in another project, the [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) directive must be used.</span></span> <span data-ttu-id="53c8f-124">名前では、個々 のコンポーネントを追加する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="53c8f-124">Individual components may be added by name.</span></span>

<span data-ttu-id="53c8f-125">ディレクティブの一般的な形式です。</span><span class="sxs-lookup"><span data-stu-id="53c8f-125">The general format of the directive is:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<Component1 />
```

<span data-ttu-id="53c8f-126">たとえば、次のディレクティブが追加されます`Component1`の`MyComponentLib1`:</span><span class="sxs-lookup"><span data-stu-id="53c8f-126">For example, the following directive adds `Component1` of `MyComponentLib1`:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

<span data-ttu-id="53c8f-127">ただし、共通するすべてのワイルドカードを使用してアセンブリからコンポーネントが含まれますが (`*`)。</span><span class="sxs-lookup"><span data-stu-id="53c8f-127">However, it's common to include all of the components from an assembly using a wildcard (`*`):</span></span>

```cshtml
@addTagHelper *, MyComponentLib1
```

<span data-ttu-id="53c8f-128">`@addTagHelper`にディレクティブを含めることが *_ViewImport.cshtml*コンポーネントを 1 つのページまたはフォルダー内のページのセットにプロジェクト全体の使用または適用します。</span><span class="sxs-lookup"><span data-stu-id="53c8f-128">The `@addTagHelper` directive can be included in *_ViewImport.cshtml* to make the components available for an entire project or applied to a single page or set of pages within a folder.</span></span> <span data-ttu-id="53c8f-129">`@addTagHelper`インプレース ディレクティブ、コンポーネント ライブラリのコンポーネント使用できるように格納されている場合、アプリと同じアセンブリ。</span><span class="sxs-lookup"><span data-stu-id="53c8f-129">With the `@addTagHelper` directive in place, the components of the component library can be consumed as if they were in the same assembly as the app.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="53c8f-130">ビルド、パック、および NuGet に出荷</span><span class="sxs-lookup"><span data-stu-id="53c8f-130">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="53c8f-131">コンポーネント ライブラリは、標準の .NET ライブラリであるために、パッケージ化し、それらを NuGet はパッケージ化と配布の任意のライブラリを NuGet 異なりません。</span><span class="sxs-lookup"><span data-stu-id="53c8f-131">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="53c8f-132">使用してパッケージを実行、 [dotnet pack](/dotnet/core/tools/dotnet-pack)コマンド。</span><span class="sxs-lookup"><span data-stu-id="53c8f-132">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command:</span></span>

```console
dotnet pack
```

<span data-ttu-id="53c8f-133">NuGet を使用するパッケージをアップロード、 [dotnet nuget 発行](/dotnet/core/tools/dotnet-nuget-push)コマンド。</span><span class="sxs-lookup"><span data-stu-id="53c8f-133">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="53c8f-134">使用する場合、`blazorlib`テンプレート、静的なリソースが、NuGet パッケージに含まれています。</span><span class="sxs-lookup"><span data-stu-id="53c8f-134">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="53c8f-135">コンシューマーは、リソースを手動でインストールする必要のないように、スクリプトやスタイル シート、ライブラリ使用者は自動的に受け取ります。</span><span class="sxs-lookup"><span data-stu-id="53c8f-135">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>
