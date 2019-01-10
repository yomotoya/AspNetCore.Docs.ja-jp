---
title: Web API アナライザーを使用する
author: pranavkm
description: Microsoft.AspNetCore.Mvc.Api.Analyzers の Web API アナライザーについて説明します。
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 12/14/2018
uid: web-api/advanced/analyzers
ms.openlocfilehash: 7558552586d3056c43d8bfd9ef74cbcb3396726f
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425095"
---
# <a name="use-web-api-analyzers"></a><span data-ttu-id="68347-103">Web API アナライザーを使用する</span><span class="sxs-lookup"><span data-stu-id="68347-103">Use web API analyzers</span></span>

<span data-ttu-id="68347-104">ASP.NET Core 2.2 以降に、Web API のアナライザーを含む [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet パッケージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="68347-104">ASP.NET Core 2.2 and later includes the [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet package containing analyzers for web APIs.</span></span> <span data-ttu-id="68347-105">アナライザーは、[API 規約](xref:web-api/advanced/conventions)の設定中に <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> の注釈が付けられたコントローラーで動作します。</span><span class="sxs-lookup"><span data-stu-id="68347-105">The analyzers work with controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, while building on [API conventions](xref:web-api/advanced/conventions).</span></span>

## <a name="package-installation"></a><span data-ttu-id="68347-106">パッケージ インストール</span><span class="sxs-lookup"><span data-stu-id="68347-106">Package installation</span></span>

<span data-ttu-id="68347-107">`Microsoft.AspNetCore.Mvc.Api.Analyzers` は、次のいずれかの方法で追加できます。</span><span class="sxs-lookup"><span data-stu-id="68347-107">`Microsoft.AspNetCore.Mvc.Api.Analyzers` can be added with one of the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="68347-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="68347-108">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="68347-109">**[パッケージ マネージャー コンソール]** ウィンドウから:</span><span class="sxs-lookup"><span data-stu-id="68347-109">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="68347-110">**[ビュー]** > **[Other Windows]** \(その他の Windows\) > **[パッケージ マネージャー コンソール]** に移動します。</span><span class="sxs-lookup"><span data-stu-id="68347-110">Go to **View** > **Other Windows** > **Package Manager Console**.</span></span>
  * <span data-ttu-id="68347-111">*ApiConventions.csproj* ファイルが存在するディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="68347-111">Navigate to the directory in which the *ApiConventions.csproj* file exists.</span></span>
  * <span data-ttu-id="68347-112">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="68347-112">Execute the following command:</span></span>

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

* <span data-ttu-id="68347-113">**[NuGet パッケージの管理]** ダイアログ ボックスから:</span><span class="sxs-lookup"><span data-stu-id="68347-113">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="68347-114">**[ソリューション エクスプローラー]** > **[NuGet パッケージの管理]** でプロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="68347-114">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**.</span></span>
  * <span data-ttu-id="68347-115">**パッケージ ソース**を "nuget.org" に設定します。</span><span class="sxs-lookup"><span data-stu-id="68347-115">Set the **Package source** to "nuget.org".</span></span>
  * <span data-ttu-id="68347-116">検索ボックスに「Microsoft.AspNetCore.Mvc.Api.Analyzers」と入力します。</span><span class="sxs-lookup"><span data-stu-id="68347-116">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
  * <span data-ttu-id="68347-117">**[参照]** タブから "Microsoft.AspNetCore.Mvc.Api.Analyzers" パッケージを選択して、**[インストール]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="68347-117">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the **Browse** tab and click **Install**.</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="68347-118">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="68347-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="68347-119">**[Solution Pad]** > **[パッケージを追加...]** で [*パッケージ*] フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="68347-119">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**.</span></span>
* <span data-ttu-id="68347-120">**[パッケージを追加]** ウィンドウの **[ソース]** ドロップダウンを "nuget.org" に設定します。</span><span class="sxs-lookup"><span data-stu-id="68347-120">Set the **Add Packages** window's **Source** drop-down to "nuget.org".</span></span>
* <span data-ttu-id="68347-121">検索ボックスに「Microsoft.AspNetCore.Mvc.Api.Analyzers」と入力します。</span><span class="sxs-lookup"><span data-stu-id="68347-121">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
* <span data-ttu-id="68347-122">結果ウィンドウから "Microsoft.AspNetCore.Mvc.Api.Analyzers" パッケージを選択して、**[パッケージを追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="68347-122">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the results pane and click **Add Package**.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="68347-123">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="68347-123">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="68347-124">**統合端末**からから次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="68347-124">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="68347-125">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="68347-125">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="68347-126">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="68347-126">Run the following command:</span></span>

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

## <a name="analyzers-for-api-conventions"></a><span data-ttu-id="68347-127">API 規約のアナライザー</span><span class="sxs-lookup"><span data-stu-id="68347-127">Analyzers for API conventions</span></span>

<span data-ttu-id="68347-128">OpenAPI ドキュメントには、アクションによって返される可能性のある状態コードと応答の種類が含まれます。</span><span class="sxs-lookup"><span data-stu-id="68347-128">OpenAPI documents contain status codes and response types that an action may return.</span></span> <span data-ttu-id="68347-129">ASP.NET Core MVC では、<xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> や <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> などの属性がアクションの文書化に使用されます。</span><span class="sxs-lookup"><span data-stu-id="68347-129">In ASP.NET Core MVC, attributes such as <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> and <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> are used to document an action.</span></span> <span data-ttu-id="68347-130"><xref:tutorials/web-api-help-pages-using-swagger> では、API の文書化についてさらに詳しく説明しています。</span><span class="sxs-lookup"><span data-stu-id="68347-130"><xref:tutorials/web-api-help-pages-using-swagger> goes into further detail on documenting your API.</span></span>

<span data-ttu-id="68347-131">パッケージのいずれかのアナライザーが、<xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> の注釈が付けられたコントローラーを検査し、応答全体を文書化していないアクションを特定します。</span><span class="sxs-lookup"><span data-stu-id="68347-131">One of the analyzers in the package inspects controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> and identifies actions that don't entirely document their responses.</span></span> <span data-ttu-id="68347-132">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="68347-132">Consider the following example:</span></span>

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=9)]

<span data-ttu-id="68347-133">前のアクションでは、HTTP 200 の成功の戻り値の型は文書化していますが、HTTP 404 のエラー状態コードは文書化していません。</span><span class="sxs-lookup"><span data-stu-id="68347-133">The preceding action documents the HTTP 200 success return type but doesn't document the HTTP 404 failure status code.</span></span> <span data-ttu-id="68347-134">アナライザーは、HTTP 404 状態コードの文書化の不備を警告として報告します。</span><span class="sxs-lookup"><span data-stu-id="68347-134">The analyzer reports the missing documentation for the HTTP 404 status code as a warning.</span></span> <span data-ttu-id="68347-135">問題を解決するためのオプションが提供されます。</span><span class="sxs-lookup"><span data-stu-id="68347-135">An option to fix the problem is provided.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="68347-136">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="68347-136">Additional resources</span></span>

* <xref:web-api/advanced/conventions>
* <xref:tutorials/web-api-help-pages-using-swagger>
* [<span data-ttu-id="68347-137">ApiController 属性を使用した注釈</span><span class="sxs-lookup"><span data-stu-id="68347-137">Annotation with ApiController attribute</span></span>](xref:web-api/index#annotation-with-apicontroller-attribute)
