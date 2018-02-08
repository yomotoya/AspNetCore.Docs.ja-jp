---
title: "ASP.NET Core のグローバリゼーションおよびローカリゼーション"
author: rick-anderson
description: "ASP.NET Core がコンテンツをさまざまな言語と文化にローカライズするために提供するサービスとミドルウェアについて説明します。"
manager: wpickett
ms.author: riande
ms.date: 01/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/localization
ms.openlocfilehash: 766cec5dd00b7b464eef31a3bc1721f522697608
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2018
---
# <a name="globalization-and-localization-in-aspnet-core"></a><span data-ttu-id="7ac3d-103">ASP.NET Core のグローバリゼーションおよびローカリゼーション</span><span class="sxs-lookup"><span data-stu-id="7ac3d-103">Globalization and localization in ASP.NET Core</span></span>

<span data-ttu-id="7ac3d-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)、[Damien Bowden](https://twitter.com/damien_bod)、[Bart Calixto](https://twitter.com/bartmax)、[Nadeem Afana](https://twitter.com/NadeemAfana)、[Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="7ac3d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [Bart Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://twitter.com/NadeemAfana), and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="7ac3d-105">ASP.NET Core で多言語の Web サイトを作成すると、より幅広い対象者がサイトにアクセスできるようになります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-105">Creating a multilingual website with ASP.NET Core will allow your site to reach a wider audience.</span></span> <span data-ttu-id="7ac3d-106">ASP.NET Core は、さまざまな言語と文化にローカライズするためのサービスとミドルウェアを提供します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-106">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="7ac3d-107">国際化には、[グローバリゼーション](https://docs.microsoft.com/dotnet/api/system.globalization)と[ローカリゼーション](https://docs.microsoft.com/dotnet/standard/globalization-localization/localization)が含まれます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-107">Internationalization involves [Globalization](https://docs.microsoft.com/dotnet/api/system.globalization) and [Localization](https://docs.microsoft.com/dotnet/standard/globalization-localization/localization).</span></span> <span data-ttu-id="7ac3d-108">グローバリゼーションとは異なるカルチャをサポートするアプリを設計するプロセスです。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-108">Globalization is the process of designing apps that support different cultures.</span></span> <span data-ttu-id="7ac3d-109">グローバリゼーションによって、特定の地域に関連する定義済みの言語セットの入出力と表示のサポートが追加されます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-109">Globalization adds support for input, display, and output of a defined set of language scripts that relate to specific geographic areas.</span></span>

<span data-ttu-id="7ac3d-110">ローカリゼーションとは、ローカライズのために既に処理されているグローバル化されたアプリを特定のカルチャ/ロケールに適合させるプロセスです。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-110">Localization is the process of adapting a globalized app, which you have already processed for localizability, to a particular culture/locale.</span></span> <span data-ttu-id="7ac3d-111">詳細については、本ドキュメントの末尾にある「**グローバリゼーションとローカリゼーションの用語**」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-111">For more information see **Globalization and localization terms** near the end of this document.</span></span>

<span data-ttu-id="7ac3d-112">アプリのローカリゼーションには、以下のものが関与します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-112">App localization involves the following:</span></span>

1. <span data-ttu-id="7ac3d-113">アプリのコンテンツをローカライズできるようにする</span><span class="sxs-lookup"><span data-stu-id="7ac3d-113">Make the app's content localizable</span></span>

2. <span data-ttu-id="7ac3d-114">サポートする言語およびカルチャのローカライズされたリソースを提供する</span><span class="sxs-lookup"><span data-stu-id="7ac3d-114">Provide localized resources for the languages and cultures you support</span></span>

3. <span data-ttu-id="7ac3d-115">要求ごとに言語/カルチャを選択するための戦略を実装する</span><span class="sxs-lookup"><span data-stu-id="7ac3d-115">Implement a strategy to select the language/culture for each request</span></span>

## <a name="make-the-apps-content-localizable"></a><span data-ttu-id="7ac3d-116">アプリのコンテンツをローカライズできるようにする</span><span class="sxs-lookup"><span data-stu-id="7ac3d-116">Make the app's content localizable</span></span>

<span data-ttu-id="7ac3d-117">ASP.NET Core で導入された `IStringLocalizer` と `IStringLocalizer<T>` は、ローカライズされたアプリの開発時に生産性を向上させるように設計されています。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-117">Introduced in ASP.NET Core, `IStringLocalizer` and `IStringLocalizer<T>` were architected to improve productivity when developing localized apps.</span></span> <span data-ttu-id="7ac3d-118">`IStringLocalizer` は、[ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager) と [ResourceReader](https://docs.microsoft.com/dotnet/api/system.resources.resourcereader) を使用して、実行時にカルチャ固有のリソースを提供します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-118">`IStringLocalizer` uses the [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager) and [ResourceReader](https://docs.microsoft.com/dotnet/api/system.resources.resourcereader) to provide culture-specific resources at run time.</span></span> <span data-ttu-id="7ac3d-119">シンプルなインターフェイスには、ローカライズされた文字列を返すためのインデクサーと `IEnumerable` があります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-119">The simple interface has an indexer and an `IEnumerable` for returning localized strings.</span></span> <span data-ttu-id="7ac3d-120">`IStringLocalizer` では、リソース ファイルに既定の言語文字列を格納する必要がありません。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-120">`IStringLocalizer` doesn't require you to store the default language strings in a resource file.</span></span> <span data-ttu-id="7ac3d-121">ローカリゼーションの対象となるアプリを開発することができ、開発の早い段階でリソース ファイルを作成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-121">You can develop an app targeted for localization and not need to create resource files early in development.</span></span> <span data-ttu-id="7ac3d-122">次のコードは、ローカリゼーションのために文字列 "About Title" をラップする方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-122">The code below shows how to wrap the string "About Title" for localization.</span></span>

[!code-csharp[Main](localization/sample/Localization/Controllers/AboutController.cs)]

<span data-ttu-id="7ac3d-123">上記のコードで、`IStringLocalizer<T>` の実装は、[依存関係の挿入](dependency-injection.md)によって実行されます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-123">In the code above, the `IStringLocalizer<T>` implementation comes from [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="7ac3d-124">"About Title" のローカライズされた値が見つからない場合、インデクサーのキー、つまり文字列 "About Title" が返されます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-124">If the localized value of "About Title" isn't found, then the indexer key is returned, that is, the string "About Title".</span></span> <span data-ttu-id="7ac3d-125">アプリで、既定の言語のリテラル文字列をそのままにし、ローカライザーにそれらをラップすることができるので、アプリの開発に専念することができます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-125">You can leave the default language literal strings in the app and wrap them in the localizer, so that you can focus on developing the app.</span></span> <span data-ttu-id="7ac3d-126">既定の言語でアプリを開発し、既定のリソース ファイルを最初に作成せずに、ローカリゼーション手順用に準備します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-126">You develop your app with your default language and prepare it for the localization step without first creating a default resource file.</span></span> <span data-ttu-id="7ac3d-127">または、従来のアプローチを使用し、既定の言語文字列を取得するキーを提供できます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-127">Alternatively, you can use the traditional approach and provide a key to retrieve the default language string.</span></span> <span data-ttu-id="7ac3d-128">既定の言語の *.resx* ファイルを使用せずに、文字列リテラルをラップするだけの新しいワークフローは、多くの開発者にとって、アプリをローカライズする際のオーバーヘッドの削減になります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-128">For many developers the new workflow of not having a default language *.resx* file and simply wrapping the string literals can reduce the overhead of localizing an app.</span></span> <span data-ttu-id="7ac3d-129">他の開発者は、長い文字列リテラルの操作が容易で、ローカライズされた文字列を更新しやすい従来のワークフローを好みます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-129">Other developers will prefer the traditional work flow as it can make it easier to work with longer string literals and make it easier to update localized strings.</span></span>

<span data-ttu-id="7ac3d-130">HTML を格納しているリソースには、`IHtmlLocalizer<T>` の実装を使用します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-130">Use the `IHtmlLocalizer<T>` implementation for resources that contain HTML.</span></span> <span data-ttu-id="7ac3d-131">`IHtmlLocalizer` HTML は、リソース文字列でフォーマットされた引数をエンコードしますが、リソース文字列自体は HTML エンコードしません。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-131">`IHtmlLocalizer` HTML encodes arguments that are formatted in the resource string, but doesn't HTML encode the resource string itself.</span></span> <span data-ttu-id="7ac3d-132">下の強調表示されたサンプルでは、`name` パラメーターの値のみが HTML エンコードされます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-132">In the sample highlighted below, only the value of `name` parameter is HTML encoded.</span></span>

[!code-csharp[Main](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

<span data-ttu-id="7ac3d-133">**注:** 一般的に、HTML ではなく、テキストのみをローカライズする必要があります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-133">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="7ac3d-134">最下位のレベルでは、[依存関係の挿入](dependency-injection.md)から `IStringLocalizerFactory` を取得できます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-134">At the lowest level, you can get `IStringLocalizerFactory` out of [Dependency Injection](dependency-injection.md):</span></span>

[!code-csharp[Main](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

<span data-ttu-id="7ac3d-135">上記のコードは、2 つの各ファクトリ作成メソッドを示しています。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-135">The code above demonstrates each of the two factory create methods.</span></span>

<span data-ttu-id="7ac3d-136">ローカライズされた文字列は、コントローラーまたは領域で仕切るか、1 つのコンテナーにすることができます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-136">You can partition your localized strings by controller, area, or have just one container.</span></span> <span data-ttu-id="7ac3d-137">サンプル アプリでは、共有されるリソースのために `SharedResource` というダミー クラスを使用しています。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-137">In the sample app, a dummy class named `SharedResource` is used for shared resources.</span></span>

[!code-csharp[Main](localization/sample/Localization/Resources/SharedResource.cs)]

<span data-ttu-id="7ac3d-138">一部の開発者は、`Startup` クラスを使用してグローバル文字列または共有文字列を格納します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-138">Some developers use the `Startup` class to contain global or shared strings.</span></span> <span data-ttu-id="7ac3d-139">下のサンプルでは、`InfoController` と `SharedResource` のローカライザーが使用されています。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-139">In the sample below, the `InfoController` and the `SharedResource` localizers are used:</span></span>

[!code-csharp[Main](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a><span data-ttu-id="7ac3d-140">ビューのローカリゼーション</span><span class="sxs-lookup"><span data-stu-id="7ac3d-140">View localization</span></span>

<span data-ttu-id="7ac3d-141">`IViewLocalizer` サービスは、[ビュー](https://docs.microsoft.com/aspnet/core)のローカライズされた文字列を提供します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-141">The `IViewLocalizer` service provides localized strings for a [view](https://docs.microsoft.com/aspnet/core).</span></span> <span data-ttu-id="7ac3d-142">`ViewLocalizer` クラスは、このインターフェイスを実装し、ビューのファイル パスからリソースの場所を見つけます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-142">The `ViewLocalizer` class implements this interface and finds the resource location from the view file path.</span></span> <span data-ttu-id="7ac3d-143">次のコードは、`IViewLocalizer` の既定の実装の使用方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-143">The following code shows how to use the default implementation of `IViewLocalizer`:</span></span>

[!code-cshtml[Main](localization/sample/Localization/Views/Home/About.cshtml)]

<span data-ttu-id="7ac3d-144">`IViewLocalizer` の既定の実装は、ビューのファイル名に基づいてリソース ファイルを見つけます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-144">The default implementation of `IViewLocalizer` finds the resource file based on the view's file name.</span></span> <span data-ttu-id="7ac3d-145">グローバルな共有リソース ファイルを使用するオプションはありません。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-145">There's no option to use a global shared resource file.</span></span> <span data-ttu-id="7ac3d-146">`ViewLocalizer` は、`IHtmlLocalizer` を使用してローカライザーを実装するので、Razor は、ローカライズされた文字列を HTML エンコードしません。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-146">`ViewLocalizer` implements the localizer using `IHtmlLocalizer`, so Razor doesn't HTML encode the localized string.</span></span> <span data-ttu-id="7ac3d-147">リソース文字列をパラメーター化することができます。`IViewLocalizer` は、パラメーターを HTML エンコードしますが、リソース文字列は HTML エンコードしません。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-147">You can parameterize resource strings and `IViewLocalizer` will HTML encode the parameters, but not the resource string.</span></span> <span data-ttu-id="7ac3d-148">次の Razor マークアップについて考えます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-148">Consider the following Razor markup:</span></span>

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

<span data-ttu-id="7ac3d-149">フランス語のリソース ファイルには次の値が含まれます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-149">A French resource file could contain the following:</span></span>

| <span data-ttu-id="7ac3d-150">キー</span><span class="sxs-lookup"><span data-stu-id="7ac3d-150">Key</span></span> | <span data-ttu-id="7ac3d-151">[値]</span><span class="sxs-lookup"><span data-stu-id="7ac3d-151">Value</span></span> |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b> ` |

<span data-ttu-id="7ac3d-152">描画されたビューには、リソース ファイルからの HTML マークアップが含まれます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-152">The rendered view would contain the HTML markup from the resource file.</span></span>

<span data-ttu-id="7ac3d-153">**注:** 一般的に、HTML ではなく、テキストのみをローカライズする必要があります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-153">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="7ac3d-154">ビュー内の共有リソース ファイルを使用するには、`IHtmlLocalizer<T>` を挿入します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-154">To use a shared resource file in a view, inject `IHtmlLocalizer<T>`:</span></span>

[!code-cshtml[Main](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a><span data-ttu-id="7ac3d-155">DataAnnotations のローカリゼーション</span><span class="sxs-lookup"><span data-stu-id="7ac3d-155">DataAnnotations localization</span></span>

<span data-ttu-id="7ac3d-156">DataAnnotations エラー メッセージは `IStringLocalizer<T>` を使用してローカライズします。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-156">DataAnnotations error messages are localized with `IStringLocalizer<T>`.</span></span> <span data-ttu-id="7ac3d-157">オプション `ResourcesPath = "Resources"` を使用して、`RegisterViewModel` のエラー メッセージを次のいずれかのパスに格納できます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-157">Using the option `ResourcesPath = "Resources"`, the error messages in `RegisterViewModel` can be stored in either of the following paths:</span></span>

* <span data-ttu-id="7ac3d-158">Resources/ViewModels.Account.RegisterViewModel.fr.resx</span><span class="sxs-lookup"><span data-stu-id="7ac3d-158">Resources/ViewModels.Account.RegisterViewModel.fr.resx</span></span>
* <span data-ttu-id="7ac3d-159">Resources/ViewModels/Account/RegisterViewModel.fr.resx</span><span class="sxs-lookup"><span data-stu-id="7ac3d-159">Resources/ViewModels/Account/RegisterViewModel.fr.resx</span></span>

[!code-csharp[Main](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

<span data-ttu-id="7ac3d-160">ASP.NET Core MVC 1.1.0 以上では、非検証属性がローカライズされます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-160">In ASP.NET Core MVC 1.1.0 and higher, non-validation attributes are localized.</span></span> <span data-ttu-id="7ac3d-161">ASP.NET Core MVC 1.0 は、非検証属性のローカライズされた文字列を検索**しません**。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-161">ASP.NET Core MVC 1.0 does **not** look up localized strings for non-validation attributes.</span></span>

<a name="one-resource-string-multiple-classes"></a>
### <a name="using-one-resource-string-for-multiple-classes"></a><span data-ttu-id="7ac3d-162">複数のクラスに 1 つのリソース文字列を使用する</span><span class="sxs-lookup"><span data-stu-id="7ac3d-162">Using one resource string for multiple classes</span></span>

<span data-ttu-id="7ac3d-163">次のコードは、複数のクラスに検証属性の 1 つのリソース文字列を使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-163">The following code shows how to use one resource string for validation attributes with multiple classes:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddDataAnnotationsLocalization(options => {
            options.DataAnnotationLocalizerProvider = (type, factory) =>
                factory.Create(typeof(SharedResource));
        });
}
```

<span data-ttu-id="7ac3d-164">上記のコードでは、`SharedResource` は、resx に対応するクラスであり、ここに検証メッセージが格納されます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-164">In the preceeding code, `SharedResource` is the class corresponding to the resx where your validation messages are stored.</span></span> <span data-ttu-id="7ac3d-165">この方法では、DataAnnotations は、各クラスのリソースではなく、`SharedResource` のみを使用します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-165">With this approach, DataAnnotations will only use `SharedResource`, rather than the resource for each class.</span></span> 

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a><span data-ttu-id="7ac3d-166">サポートする言語およびカルチャのローカライズされたリソースを提供する</span><span class="sxs-lookup"><span data-stu-id="7ac3d-166">Provide localized resources for the languages and cultures you support</span></span>  

### <a name="supportedcultures-and-supporteduicultures"></a><span data-ttu-id="7ac3d-167">SupportedCultures と SupportedUICultures</span><span class="sxs-lookup"><span data-stu-id="7ac3d-167">SupportedCultures and SupportedUICultures</span></span>

<span data-ttu-id="7ac3d-168">ASP.NET Core では、`SupportedCultures` と `SupportedUICultures` という 2 つのカルチャ値を指定できます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-168">ASP.NET Core allows you to specify two culture values, `SupportedCultures` and `SupportedUICultures`.</span></span> <span data-ttu-id="7ac3d-169">日付、数値、および通貨の書式設定など、カルチャに依存する関数の結果は、`SupportedCultures` の [CultureInfo](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo) オブジェクトによって決まります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-169">The [CultureInfo](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo) object for `SupportedCultures` determines the results of culture-dependent functions, such as date, time, number, and currency formatting.</span></span> <span data-ttu-id="7ac3d-170">テキストの並べ替え順序、大文字と小文字の表記規則、文字列比較も `SupportedCultures` によって決まります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-170">`SupportedCultures` also determines the sorting order of text, casing conventions, and string comparisons.</span></span> <span data-ttu-id="7ac3d-171">サーバーがカルチャを取得する方法の詳細については、「[CultureInfo.CurrentCulture](https://docs.microsoft.com/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-171">See [CultureInfo.CurrentCulture](https://docs.microsoft.com/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) for more info on how the server gets the Culture.</span></span> <span data-ttu-id="7ac3d-172">`SupportedUICultures` は、*.resx* ファイルからのどの翻訳文字列が [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager) によって検索されるかを決定します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-172">The `SupportedUICultures` determines which translates strings (from *.resx* files) are looked up by the [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager).</span></span> <span data-ttu-id="7ac3d-173">`ResourceManager` は、`CurrentUICulture` によって決定されるカルチャ固有の文字列を単に検索します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-173">The `ResourceManager` simply looks up culture-specific strings that's determined by `CurrentUICulture`.</span></span> <span data-ttu-id="7ac3d-174">.NET のすべてのスレッドに `CurrentCulture` オブジェクトと `CurrentUICulture`オブジェクトがあります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-174">Every thread in .NET has `CurrentCulture` and `CurrentUICulture` objects.</span></span> <span data-ttu-id="7ac3d-175">ASP.NET Core は、カルチャに依存する関数を表示するときに、これらの値を検査します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-175">ASP.NET Core inspects these values when rendering culture-dependent functions.</span></span> <span data-ttu-id="7ac3d-176">たとえば、現在のスレッドのカルチャが "en-US" (英語、米国) に設定されている場合は、`DateTime.Now.ToLongDateString()` は、"Thursday, February 18, 2016" を表示しますが、`CurrentCulture` が "es-ES" (スペイン語、スペイン) に設定されている場合、出力は "jueves, 18 de febrero de 2016" になります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-176">For example, if the current thread's culture is set to "en-US" (English, United States), `DateTime.Now.ToLongDateString()` displays "Thursday, February 18, 2016", but if `CurrentCulture` is set to "es-ES" (Spanish, Spain) the output will be "jueves, 18 de febrero de 2016".</span></span>

## <a name="resource-files"></a><span data-ttu-id="7ac3d-177">リソース ファイル</span><span class="sxs-lookup"><span data-stu-id="7ac3d-177">Resource files</span></span>

<span data-ttu-id="7ac3d-178">リソース ファイルは、ローカライズ可能な文字列をコードから分離するために役立つメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-178">A resource file is a useful mechanism for separating localizable strings from code.</span></span> <span data-ttu-id="7ac3d-179">既定以外の言語の翻訳された文字列は、分離された *.resx* リソース ファイルです。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-179">Translated strings for the non-default language are isolated *.resx* resource files.</span></span> <span data-ttu-id="7ac3d-180">たとえば、翻訳された文字列を含む *Welcome.es.resx* という名前のスペイン語のリソース ファイルを作成したい場合があります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-180">For example, you might want to create Spanish resource file named *Welcome.es.resx* containing translated strings.</span></span> <span data-ttu-id="7ac3d-181">"es" は、スペイン語の言語コードです。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-181">"es" is the language code for Spanish.</span></span> <span data-ttu-id="7ac3d-182">Visual Studio でこのリソース ファイルを作成するには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-182">To create this resource file in Visual Studio:</span></span>

1. <span data-ttu-id="7ac3d-183">**ソリューション エクスプローラー**で、リソース ファイルが格納されているフォルダーを右クリックし、**[追加]** > **[新しい項目]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-183">In **Solution Explorer**, right click on the folder which will contain the resource file > **Add** > **New Item**.</span></span>

    ![入れ子になったコンテキスト メニュー: ソリューション エクスプローラーでリソースのコンテキスト メニューが開かれます。](localization/_static/newi.png)

2. <span data-ttu-id="7ac3d-186">**インストールされているテンプレートの検索**ボックスに、「resource」と入力し、ファイルに名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-186">In the **Search installed templates** box, enter "resource" and name the file.</span></span>

    ![[新しい項目の追加] ダイアログ](localization/_static/res.png)

3. <span data-ttu-id="7ac3d-188">キーの値 (ネイティブの文字列) を **[名前]** 列に入力し、翻訳された文字列を **[値]** 列に入力します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-188">Enter the key value (native string) in the **Name** column and the translated string in the **Value** column.</span></span>

    ![Welcome.es.resx ファイル (スペイン語の Welcome リソース ファイル) と、[名前] 列の Hello という単語と、[値] 列の Hola という単語 (スペイン語のこんにちは)](localization/_static/hola.png)

    <span data-ttu-id="7ac3d-190">Visual Studio に *Welcome.es.resx* ファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-190">Visual Studio shows the *Welcome.es.resx* file.</span></span>

    ![Welcome スペイン語リソース ファイルを表示しているソリューション エクスプローラー](localization/_static/se.png)

<a name="error"></a>

<span data-ttu-id="7ac3d-192">Visual Studio 2017 Preview バージョン 15.3 を使用している場合、リソース エディターでエラー インジケーターが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-192">If you are using Visual Studio 2017 Preview version 15.3, you'll get an error indicator in the resource editor.</span></span> <span data-ttu-id="7ac3d-193">このエラー メッセージを防ぐために、*[カスタム ツール]* プロパティから *ResXFileCodeGenerator* 値を削除します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-193">Remove the *ResXFileCodeGenerator*  value from the *Custom Tool* properties grid to prevent this error message:</span></span>

![Resx エディター](localization/_static/err.png)

<span data-ttu-id="7ac3d-195">または、このエラーを無視することができます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-195">Alternatively, you can ignore this error.</span></span> <span data-ttu-id="7ac3d-196">次のリリースでこの問題を解決する予定です。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-196">We hope to fix this in the next release.</span></span>

## <a name="resource-file-naming"></a><span data-ttu-id="7ac3d-197">リソース ファイルの名前付け</span><span class="sxs-lookup"><span data-stu-id="7ac3d-197">Resource file naming</span></span>

<span data-ttu-id="7ac3d-198">リソースの名前は、クラスの完全な型名からアセンブリ名を除いたものになります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-198">Resources are named for the full type name of their class minus the assembly name.</span></span> <span data-ttu-id="7ac3d-199">たとえば、メイン アセンブリが `LocalizationWebsite.Web.dll` であるプロジェクト内のクラス `LocalizationWebsite.Web.Startup` のフランス語のリソースは、*Startup.fr.resx* という名前になります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-199">For example, a French resource in a project whose main assembly is `LocalizationWebsite.Web.dll` for the class `LocalizationWebsite.Web.Startup` would be named *Startup.fr.resx*.</span></span> <span data-ttu-id="7ac3d-200">クラス `LocalizationWebsite.Web.Controllers.HomeController` のリソースは、*Controllers.HomeController.fr.resx* という名前になります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-200">A resource for the class `LocalizationWebsite.Web.Controllers.HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="7ac3d-201">対象となるクラスの名前空間が、アセンブリ名と同じでない場合は、完全な型名が必要です。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-201">If your targeted class's namespace isn't the same as the assembly name you will need the full type name.</span></span> <span data-ttu-id="7ac3d-202">たとえば、同じプロジェクトで、型 `ExtraNamespace.Tools` のリソースは、*ExtraNamespace.Tools.fr.resx* という名前になります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-202">For example, in the sample project a resource for the type `ExtraNamespace.Tools` would be named *ExtraNamespace.Tools.fr.resx*.</span></span>

<span data-ttu-id="7ac3d-203">サンプル プロジェクトで、`ConfigureServices` メソッドは `ResourcesPath` を "Resources" に設定するので、ホーム コントローラーのフランス語のりソース ファイルの相対パスは、*Resources/Controllers.HomeController.fr.resx* です。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-203">In the sample project, the `ConfigureServices` method sets the `ResourcesPath` to "Resources", so the project relative path for the home controller's French resource file is *Resources/Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="7ac3d-204">あるいは、フォルダーを使用してリソース ファイルを整理することもできます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-204">Alternatively, you can use folders to organize resource files.</span></span> <span data-ttu-id="7ac3d-205">Home コント ローラーのパスは、*Resources/Controllers/HomeController.fr.resx* です。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-205">For the home controller, the path would be *Resources/Controllers/HomeController.fr.resx*.</span></span> <span data-ttu-id="7ac3d-206">`ResourcesPath` オプションを使用しない場合、*.resx* ファイルは、プロジェクトの基本ディレクトリに置かれます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-206">If you don't use the `ResourcesPath` option, the *.resx* file would go in the project base directory.</span></span> <span data-ttu-id="7ac3d-207">`HomeController` のリソース ファイルは、*Controllers.HomeController.fr.resx* という名前になります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-207">The resource file for `HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="7ac3d-208">ドットまたはパスの名前付け規則の選択は、リソース ファイルを整理する方法によって決まります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-208">The choice of using the dot or path naming convention depends on how you want to organize your resource files.</span></span>

| <span data-ttu-id="7ac3d-209">リソース名</span><span class="sxs-lookup"><span data-stu-id="7ac3d-209">Resource name</span></span> | <span data-ttu-id="7ac3d-210">ドットまたはパスの名前付け</span><span class="sxs-lookup"><span data-stu-id="7ac3d-210">Dot or path naming</span></span> |
| ------------   | ------------- |
| <span data-ttu-id="7ac3d-211">Resources/Controllers.HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="7ac3d-211">Resources/Controllers.HomeController.fr.resx</span></span> | <span data-ttu-id="7ac3d-212">ドット</span><span class="sxs-lookup"><span data-stu-id="7ac3d-212">Dot</span></span>  |
| <span data-ttu-id="7ac3d-213">Resources/Controllers/HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="7ac3d-213">Resources/Controllers/HomeController.fr.resx</span></span>  | <span data-ttu-id="7ac3d-214">パス</span><span class="sxs-lookup"><span data-stu-id="7ac3d-214">Path</span></span> |
|    |     |

<span data-ttu-id="7ac3d-215">Razor ビューの `@inject IViewLocalizer` を使用するリソース ファイルは同様のパターンに従います。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-215">Resource files using `@inject IViewLocalizer` in Razor views follow a similar pattern.</span></span> <span data-ttu-id="7ac3d-216">ビューのリソース ファイルには、ドットの名前付けまたはパスの名前付けを使用して名前を付けることができます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-216">The resource file for a view can be named using either dot naming or path naming.</span></span> <span data-ttu-id="7ac3d-217">Razor ビューのリソース ファイルは、関連するビュー ファイルのパスを模倣します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-217">Razor view resource files mimic the path of their associated view file.</span></span> <span data-ttu-id="7ac3d-218">`ResourcesPath` を "Resources" に設定すると仮定すると、*Views/Home/About.cshtml* ビューに関連付けられるフランス語のリソース ファイルは、次のいずれかになります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-218">Assuming we set the `ResourcesPath` to "Resources", the French resource file associated with the *Views/Home/About.cshtml* view could be either of the following:</span></span>

* <span data-ttu-id="7ac3d-219">Resources/Views/Home/About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="7ac3d-219">Resources/Views/Home/About.fr.resx</span></span>

* <span data-ttu-id="7ac3d-220">Resources/Views.Home.About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="7ac3d-220">Resources/Views.Home.About.fr.resx</span></span>

<span data-ttu-id="7ac3d-221">`ResourcesPath` オプションを使用しない場合、ビューの *.resx* ファイルは、ビューと同じフォルダーに配置されます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-221">If you don't use the `ResourcesPath` option, the *.resx* file for a view would be located in the same folder as the view.</span></span>

## <a name="culture-fallback-behavior"></a><span data-ttu-id="7ac3d-222">カルチャ フォールバック動作</span><span class="sxs-lookup"><span data-stu-id="7ac3d-222">Culture fallback behavior</span></span>

<span data-ttu-id="7ac3d-223">例として、".fr" カルチャ指定子を削除し、カルチャを French に設定した場合、既定のリソース ファイルは read で文字列がローカライズされます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-223">As an example, if you remove the ".fr" culture designator and you have the culture set to French, the default resource file is read and strings are localized.</span></span> <span data-ttu-id="7ac3d-224">リソース マネージャーは、要求されたカルチャを満たすものがない場合、既定またはフォールバックのリソースを指定します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-224">The Resource manager designates a default or fallback resource for when nothing meets your requested culture.</span></span> <span data-ttu-id="7ac3d-225">要求されたカルチャのリソースが見つからないときにキーのみを返す場合は、既定のリソース ファイルを使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-225">If you want to just return the key when missing a resource for the requested culture you must not have a default resource file.</span></span>

### <a name="generate-resource-files-with-visual-studio"></a><span data-ttu-id="7ac3d-226">Visual Studio でリソース ファイルを生成する</span><span class="sxs-lookup"><span data-stu-id="7ac3d-226">Generate resource files with Visual Studio</span></span>

<span data-ttu-id="7ac3d-227">Visual Studio で、ファイル名にカルチャを指定せずにリソース ファイルを作成する場合 (たとえば、*Welcome.resx*)、Visual Studio は各文字列のプロパティを使用して、C# クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-227">If you create a resource file in Visual Studio without a culture in the file name (for example, *Welcome.resx*), Visual Studio will create a C# class with a property for each string.</span></span> <span data-ttu-id="7ac3d-228">通常、ASP.NET Core ではこれを行いません。一般的に既定の *.resx* リソース ファイル (カルチャ名のない *.resx* ファイル) は使用しません。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-228">That's usually not what you want with ASP.NET Core; you typically don't have a default *.resx* resource file (A *.resx* file without the culture name).</span></span> <span data-ttu-id="7ac3d-229">カルチャ名を含む *.resx* ファイル (たとえば *Welcome.fr.resx*) を作成することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-229">We suggest you create the *.resx* file with a culture name (for example *Welcome.fr.resx*).</span></span> <span data-ttu-id="7ac3d-230">カルチャ名を含む *.resx* ファイルを作成すると、Visual Studio はクラス ファイルを生成しません。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-230">When you create a *.resx* file with a culture name, Visual Studio won't generate the class file.</span></span> <span data-ttu-id="7ac3d-231">多くの開発者は既定の言語リソース ファイルを作成**しない**と予想されます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-231">We anticipate that many developers will **not** create a default language resource file.</span></span>

### <a name="add-other-cultures"></a><span data-ttu-id="7ac3d-232">その他のカルチャを追加する</span><span class="sxs-lookup"><span data-stu-id="7ac3d-232">Add Other Cultures</span></span>

<span data-ttu-id="7ac3d-233">各言語とカルチャの組み合わせ (既定の言語以外) ごとに一意のリソース ファイルが必要です。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-233">Each language and culture combination (other than the default language) requires a unique resource file.</span></span> <span data-ttu-id="7ac3d-234">ISO 言語コードがファイル名の一部となるリソース ファイル (たとえば、**en-us**、**fr-ca**、**en-gb**) を新規作成することで、異なるカルチャとロケールのリソース ファイルを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-234">You create resource files for different cultures and locales by creating new resource files in which the ISO language codes are part of the file name (for example, **en-us**, **fr-ca**, and **en-gb**).</span></span> <span data-ttu-id="7ac3d-235">*Welcome.es-MX.resx* (スペイン語/メキシコ) のように、ファイル名と *.resx* ファイル名拡張子の間にこれらの ISO コードが置かれます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-235">These ISO codes are placed between the file name and the *.resx* file name extension, as in *Welcome.es-MX.resx* (Spanish/Mexico).</span></span> <span data-ttu-id="7ac3d-236">カルチャ的にニュートラルな言語を指定するには、国コード (前の例では `MX`) を削除します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-236">To specify a culturally neutral language, remove the country code (`MX` in the preceding example).</span></span> <span data-ttu-id="7ac3d-237">カルチャ的にニュートラルなスペイン語のリソース ファイル名は *Welcome.es.resx* です。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-237">The culturally neutral Spanish resource file name is *Welcome.es.resx*.</span></span>

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a><span data-ttu-id="7ac3d-238">要求ごとに言語/カルチャを選択するための戦略を実装する</span><span class="sxs-lookup"><span data-stu-id="7ac3d-238">Implement a strategy to select the language/culture for each request</span></span>  

### <a name="configure-localization"></a><span data-ttu-id="7ac3d-239">ローカリゼーションを構成する</span><span class="sxs-lookup"><span data-stu-id="7ac3d-239">Configure localization</span></span>

<span data-ttu-id="7ac3d-240">ローカリゼーションは、`ConfigureServices` メソッドで構成されます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-240">Localization is configured in the `ConfigureServices` method:</span></span>

[!code-csharp[Main](localization/sample/Localization/Program.cs?name=snippet1)]

* <span data-ttu-id="7ac3d-241">`AddLocalization` ローカリゼーション サービスをサービス コンテナーに追加します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-241">`AddLocalization` Adds the localization services to the services container.</span></span> <span data-ttu-id="7ac3d-242">上記のコードは、リソース パスを "Resources" に設定します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-242">The code above also sets the resources path to "Resources".</span></span>

* <span data-ttu-id="7ac3d-243">`AddViewLocalization` ローカライズされたビュー ファイルのサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-243">`AddViewLocalization` Adds support for localized view files.</span></span> <span data-ttu-id="7ac3d-244">このサンプル ビューでは、ローカリゼーションは、ビュー ファイルのサフィックスに基づいています。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-244">In this sample view localization is based on the view file suffix.</span></span> <span data-ttu-id="7ac3d-245">たとえば、*Index.fr.cshtml* ファイルの "fr" です。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-245">For example "fr" in the *Index.fr.cshtml* file.</span></span>

* <span data-ttu-id="7ac3d-246">`AddDataAnnotationsLocalization` `IStringLocalizer` 抽象化を介してローカライズされた `DataAnnotations` 検証メッセージのサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-246">`AddDataAnnotationsLocalization` Adds support for localized `DataAnnotations` validation messages through `IStringLocalizer` abstractions.</span></span>

### <a name="localization-middleware"></a><span data-ttu-id="7ac3d-247">ローカリゼーション ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="7ac3d-247">Localization middleware</span></span>

<span data-ttu-id="7ac3d-248">要求時に現在のカルチャが、ローカリゼーション [ミドルウェア](xref:fundamentals/middleware/index)で設定されます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-248">The current culture on a request is set in the localization [Middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="7ac3d-249">ローカリゼーション ミドルウェアは、`Configure` メソッドで有効になります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-249">The localization middleware is enabled in the `Configure` method.</span></span> <span data-ttu-id="7ac3d-250">要求のカルチャをチェックする可能性があるすべてのミドルウェア (たとえば `app.UseMvcWithDefaultRoute()`) の前に、ローカリゼーション ミドルウェアを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-250">The localization middleware must be configured before any middleware which might check the request culture (for example, `app.UseMvcWithDefaultRoute()`).</span></span>

[!code-csharp[Main](localization/sample/Localization/Program.cs?name=snippet2)]

<span data-ttu-id="7ac3d-251">`UseRequestLocalization` は `RequestLocalizationOptions` オブジェクトを初期化します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-251">`UseRequestLocalization` initializes a `RequestLocalizationOptions` object.</span></span> <span data-ttu-id="7ac3d-252">すべての要求で、`RequestLocalizationOptions` の `RequestCultureProvider` のリストが列挙され、要求のカルチャを正常に決定できる最初のプロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-252">On every request the list of `RequestCultureProvider` in the `RequestLocalizationOptions` is enumerated and the first provider that can successfully determine the request culture is used.</span></span> <span data-ttu-id="7ac3d-253">既定のプロバイダーは `RequestLocalizationOptions` クラスから派生します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-253">The default providers come from the `RequestLocalizationOptions` class:</span></span>

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

<span data-ttu-id="7ac3d-254">既定のリストは、最も具体的なものから最も具体的でないものの順番になります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-254">The default list goes from most specific to least specific.</span></span> <span data-ttu-id="7ac3d-255">記事の後半では、この順序を変更する方法、さらにカスタム カルチャ プロバイダーを追加する方法も説明します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-255">Later in the article we'll see how you can change the order and even add a custom culture provider.</span></span> <span data-ttu-id="7ac3d-256">要求のカルチャを判断できるプロバイダーがない場合、`DefaultRequestCulture` が使用されます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-256">If none of the providers can determine the request culture, the `DefaultRequestCulture` is used.</span></span>

### <a name="querystringrequestcultureprovider"></a><span data-ttu-id="7ac3d-257">QueryStringRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="7ac3d-257">QueryStringRequestCultureProvider</span></span>

<span data-ttu-id="7ac3d-258">いくつかのアプリでは、クエリ文字列を使用して、[カルチャおよび UI カルチャ](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)を設定します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-258">Some apps will use a query string to set the [culture and UI culture](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx).</span></span> <span data-ttu-id="7ac3d-259">Cookie または Accept-language ヘッダーのアプローチを使用するアプリの場合、URL にクエリ文字列を追加すると、デバッグおよびコードのテストに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-259">For apps that use the cookie or Accept-Language header approach, adding a query string to the URL is useful for debugging and testing code.</span></span> <span data-ttu-id="7ac3d-260">既定では、`QueryStringRequestCultureProvider` が、`RequestCultureProvider` リストの最初のローカリゼーション プロバイダーとして登録されます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-260">By default, the `QueryStringRequestCultureProvider` is registered as the first localization provider in the `RequestCultureProvider` list.</span></span> <span data-ttu-id="7ac3d-261">クエリ文字列パラメーター `culture` と `ui-culture` を渡します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-261">You pass the query string parameters `culture` and `ui-culture`.</span></span> <span data-ttu-id="7ac3d-262">次の例では、特定のカルチャ (言語および地域) をスペイン語/メキシコに設定します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-262">The following example sets the specific culture (language and region) to Spanish/Mexico:</span></span>

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

<span data-ttu-id="7ac3d-263">2 つのいずれかのみ (`culture` または `ui-culture`) を渡した場合、クエリ文字列プロバイダーは、渡した 1 つのパラメーターを使用して両方の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-263">If you only pass in one of the two (`culture` or `ui-culture`), the query string provider will set both values using the one you passed in.</span></span> <span data-ttu-id="7ac3d-264">たとえば、カルチャだけを設定すると、`Culture` と `UICulture` の両方が設定されます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-264">For example, setting just the culture will set both the `Culture` and the `UICulture`:</span></span>

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a><span data-ttu-id="7ac3d-265">CookieRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="7ac3d-265">CookieRequestCultureProvider</span></span>

<span data-ttu-id="7ac3d-266">多くの場合、実稼働アプリケーションは、ASP.NET Core カルチャ Cookie を使用してカルチャを設定するためのメカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-266">Production apps will often provide a mechanism to set the culture with the ASP.NET Core culture cookie.</span></span> <span data-ttu-id="7ac3d-267">Cookie を作成するには、`MakeCookieValue` メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-267">Use the `MakeCookieValue` method to create a cookie.</span></span>

<span data-ttu-id="7ac3d-268">`CookieRequestCultureProvider` `DefaultCookieName` は、ユーザーの優先するカルチャ情報を追跡するために使用される既定の Cookie 名を返します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-268">The `CookieRequestCultureProvider` `DefaultCookieName` returns the default cookie name used to track the user's preferred culture information.</span></span> <span data-ttu-id="7ac3d-269">既定の Cookie 名は `.AspNetCore.Culture` です。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-269">The default cookie name is `.AspNetCore.Culture`.</span></span>

<span data-ttu-id="7ac3d-270">Cookie の形式は `c=%LANGCODE%|uic=%LANGCODE%` です。ここで、`c` は `Culture` であり、`uic` は `UICulture` です。たとえば、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-270">The cookie format is `c=%LANGCODE%|uic=%LANGCODE%`, where `c` is `Culture` and `uic` is `UICulture`, for example:</span></span>

    c=en-UK|uic=en-US

<span data-ttu-id="7ac3d-271">カルチャ情報と UI カルチャの 1 つのみを指定した場合、指定したカルチャが、カルチャ情報と UI カルチャの両方に使用されます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-271">If you only specify one of culture info and UI culture, the specified culture will be used for both culture info and UI culture.</span></span>

### <a name="the-accept-language-http-header"></a><span data-ttu-id="7ac3d-272">Accept-Language HTTP ヘッダー</span><span class="sxs-lookup"><span data-stu-id="7ac3d-272">The Accept-Language HTTP header</span></span>

<span data-ttu-id="7ac3d-273">[Accept-language ヘッダー](https://www.w3.org/International/questions/qa-accept-lang-locales)は、ほとんどのブラウザーで設定可能であり、当初はユーザーの言語を指定するためのものでした。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-273">The [Accept-Language header](https://www.w3.org/International/questions/qa-accept-lang-locales) is settable in most browsers and was originally intended to specify the user's language.</span></span> <span data-ttu-id="7ac3d-274">この設定は、ブラウザーが何を送信するように設定されているか、基になるオペレーティング システムから何を継承するかを示します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-274">This setting indicates what the browser has been set to send or has inherited from the underlying operating system.</span></span> <span data-ttu-id="7ac3d-275">ブラウザーの要求からの Accept Language HTTP ヘッダーは、ユーザーの優先言語を検出するための確実な方法ではありません (「[Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php)」(ブラウザーの優先言語を設定する) を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-275">The Accept-Language HTTP header from a browser request isn't an infallible way to detect the user's preferred language (see [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php)).</span></span> <span data-ttu-id="7ac3d-276">実稼働アプリには、ユーザーがカルチャの選択をカスタマイズする方法を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-276">A production app should include a way for a user to customize their choice of culture.</span></span>

### <a name="set-the-accept-language-http-header-in-ie"></a><span data-ttu-id="7ac3d-277">IE で Accept-Language HTTP ヘッダーを設定する</span><span class="sxs-lookup"><span data-stu-id="7ac3d-277">Set the Accept-Language HTTP header in IE</span></span>

1. <span data-ttu-id="7ac3d-278">歯車アイコンから、**[インターネット オプション]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-278">From the gear icon, tap **Internet Options**.</span></span>

2. <span data-ttu-id="7ac3d-279">**[言語]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-279">Tap **Languages**.</span></span>

    ![インターネット オプション](localization/_static/lang.png)

3. <span data-ttu-id="7ac3d-281">**[言語の優先順位の設定]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-281">Tap **Set Language Preferences**.</span></span>

4. <span data-ttu-id="7ac3d-282">**[言語の追加]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-282">Tap **Add a language**.</span></span>

5. <span data-ttu-id="7ac3d-283">言語を追加します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-283">Add the language.</span></span>

6. <span data-ttu-id="7ac3d-284">言語をタップして、**[上へ移動]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-284">Tap the language, then tap **Move Up**.</span></span>

### <a name="use-a-custom-provider"></a><span data-ttu-id="7ac3d-285">カスタム プロバイダーを使用する</span><span class="sxs-lookup"><span data-stu-id="7ac3d-285">Use a custom provider</span></span>

<span data-ttu-id="7ac3d-286">お客様がデータベースに言語とカルチャを格納できるようにしたいとします。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-286">Suppose you want to let your customers store their language and culture in your databases.</span></span> <span data-ttu-id="7ac3d-287">ユーザーのためにこれらの値を検索するプロバイダーを記述することもできます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-287">You could write a provider to look up these values for the user.</span></span> <span data-ttu-id="7ac3d-288">次のコードは、カスタム プロバイダーを追加する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-288">The following code shows how to add a custom provider:</span></span>

```csharp
private const string enUSCulture = "en-US";

services.Configure<RequestLocalizationOptions>(options =>
{
    var supportedCultures = new[]
    {
        new CultureInfo(enUSCulture),
        new CultureInfo("fr")
    };

    options.DefaultRequestCulture = new RequestCulture(culture: enUSCulture, uiCulture: enUSCulture);
    options.SupportedCultures = supportedCultures;
    options.SupportedUICultures = supportedCultures;

    options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
});
```

<span data-ttu-id="7ac3d-289">ローカリゼーション プロバイダーを追加または削除するには、`RequestLocalizationOptions` を使用します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-289">Use `RequestLocalizationOptions` to add or remove localization providers.</span></span>

### <a name="set-the-culture-programmatically"></a><span data-ttu-id="7ac3d-290">プログラムでカルチャを設定する</span><span class="sxs-lookup"><span data-stu-id="7ac3d-290">Set the culture programmatically</span></span>

<span data-ttu-id="7ac3d-291">この [GitHub](https://github.com/aspnet/entropy) のサンプル **Localization.StarterWeb** プロジェクトには、`Culture` を設定するための UI が含まれています。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-291">This sample **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) contains UI to set the `Culture`.</span></span> <span data-ttu-id="7ac3d-292">*Views/Shared/_SelectLanguagePartial.cshtml* ファイルを使用して、サポートされているカルチャの一覧からカルチャを選択することができます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-292">The *Views/Shared/_SelectLanguagePartial.cshtml* file allows you to select the culture from the list of supported cultures:</span></span>

[!code-cshtml[Main](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

<span data-ttu-id="7ac3d-293">*Views/Shared/_SelectLanguagePartial.cshtml* ファイルは、レイアウト ファイルの `footer` セクションに追加されるので、すべてのビューで使用できます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-293">The *Views/Shared/_SelectLanguagePartial.cshtml* file is added to the `footer` section of the layout file so it will be available to all views:</span></span>

[!code-cshtml[Main](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

<span data-ttu-id="7ac3d-294">`SetLanguage` メソッドはカルチャ Cookie を設定します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-294">The `SetLanguage` method sets the culture cookie.</span></span>

[!code-csharp[Main](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

<span data-ttu-id="7ac3d-295">*_SelectLanguagePartial.cshtml* をこのプロジェクトのサンプル コードに接続することはできません。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-295">You can't plug in the *_SelectLanguagePartial.cshtml* to sample code for this project.</span></span> <span data-ttu-id="7ac3d-296">[GitHub](https://github.com/aspnet/entropy) の **Localization.StarterWeb** プロジェクトには、[依存関係の挿入](dependency-injection.md)コンテナーを介して Razor 部分に `RequestLocalizationOptions` を挿入するコードがあります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-296">The **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) has code to flow the `RequestLocalizationOptions` to a Razor partial through the [Dependency Injection](dependency-injection.md) container.</span></span>

## <a name="globalization-and-localization-terms"></a><span data-ttu-id="7ac3d-297">グローバリゼーションとローカリゼーションの用語</span><span class="sxs-lookup"><span data-stu-id="7ac3d-297">Globalization and localization terms</span></span>

<span data-ttu-id="7ac3d-298">アプリをローカライズするプロセスでは、最新のソフトウェア開発でよく使用される関連する文字セットの基本的な理解と、それらに関連した問題の理解も必要です。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-298">The process of localizing your app also requires a basic understanding of relevant character sets commonly used in modern software development and an understanding of the issues associated with them.</span></span> <span data-ttu-id="7ac3d-299">すべてのコンピューターは、テキストを数値 (コード) として格納しますが、さまざまなシステムが異なる数値を使用して同じテキストを格納します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-299">Although all computers store text as numbers (codes), different systems store the same text using different numbers.</span></span> <span data-ttu-id="7ac3d-300">ローカリゼーション プロセスとは、特定のカルチャ/ロケール用にアプリのユーザー インターフェイス (UI) を変換することを指します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-300">The localization process refers to translating the app user interface (UI) for a specific culture/locale.</span></span>

<span data-ttu-id="7ac3d-301">[ローカライズ化](https://docs.microsoft.com/dotnet/standard/globalization-localization/localizability-review)は、グローバル化されたアプリのローカリゼーションの準備ができていることを確認するための中間プロセスです。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-301">[Localizability](https://docs.microsoft.com/dotnet/standard/globalization-localization/localizability-review) is an intermediate process for verifying that a globalized app is ready for localization.</span></span>

<span data-ttu-id="7ac3d-302">カルチャ名の [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) 形式は `<languagecode2>-<country/regioncode2>` です。ここで、`<languagecode2>` は言語コードであり、`<country/regioncode2>` はサブカルチャ コードです。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-302">The [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) format for the culture name is `<languagecode2>-<country/regioncode2>`, where `<languagecode2>` is the language code and `<country/regioncode2>` is the subculture code.</span></span> <span data-ttu-id="7ac3d-303">たとえば、スペイン語 (チリ) は `es-CL`、英語 (米国) は `en-US`、英語 (オーストラリア) は `en-AU` です。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-303">For example, `es-CL` for Spanish (Chile), `en-US` for English (United States), and `en-AU` for English (Australia).</span></span> <span data-ttu-id="7ac3d-304">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) は、言語に関連付けられた ISO 639 の 2 文字の小文字カルチャ コードと、国または地域に関連付けられた ISO 3166 の 2 文字の大文字サブカルチャ コードの組み合わせです。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-304">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) is a combination of an ISO 639 two-letter lowercase culture code associated with a language and an ISO 3166 two-letter uppercase subculture code associated with a country or region.</span></span> <span data-ttu-id="7ac3d-305">「[Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx)」(言語カルチャ名) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-305">See [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).</span></span>

<span data-ttu-id="7ac3d-306">多くの場合、国際化は "I18N" に省略されます。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-306">Internationalization is often abbreviated to "I18N".</span></span> <span data-ttu-id="7ac3d-307">省略形は、最初と最後の文字およびそれらの間の文字の数になります。したがって、18 は、最初の "I" と最後の "N" の間の文字の数を表します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-307">The abbreviation takes the first and last letters and the number of letters between them, so 18 stands for the number of letters between the first "I" and the last "N".</span></span> <span data-ttu-id="7ac3d-308">同じことが、グローバリゼーション (G11N) とローカリゼーション (L10N) にも当てはまります。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-308">The same applies to Globalization (G11N), and Localization (L10N).</span></span>

<span data-ttu-id="7ac3d-309">用語:</span><span class="sxs-lookup"><span data-stu-id="7ac3d-309">Terms:</span></span>

* <span data-ttu-id="7ac3d-310">グローバリゼーション (G11N): アプリが別の言語と地域をサポートするようにするプロセス。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-310">Globalization (G11N): The process of making an app support different languages and regions.</span></span>
* <span data-ttu-id="7ac3d-311">ローカリゼーション (L10N): 特定の言語と地域向けにアプリをカスタマイズするプロセス。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-311">Localization (L10N): The process of customizing an app for a given language and region.</span></span>
* <span data-ttu-id="7ac3d-312">国際化 (I18N): グローバリゼーションとローカリゼーションの両方を示します。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-312">Internationalization (I18N): Describes both globalization and localization.</span></span>
* <span data-ttu-id="7ac3d-313">カルチャ: 言語および必要に応じて地域です。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-313">Culture: It's a language and, optionally, a region.</span></span>
* <span data-ttu-id="7ac3d-314">ニュートラル カルチャ: 指定した言語のみを含み、地域は含まないカルチャ </span><span class="sxs-lookup"><span data-stu-id="7ac3d-314">Neutral culture: A culture that has a specified language, but not a region.</span></span> <span data-ttu-id="7ac3d-315">(例: "en"、"es")。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-315">(for example "en", "es")</span></span>
* <span data-ttu-id="7ac3d-316">特定のカルチャ: 指定した言語と地域を含むカルチャ </span><span class="sxs-lookup"><span data-stu-id="7ac3d-316">Specific culture: A culture that has a specified language and region.</span></span> <span data-ttu-id="7ac3d-317">(例: "en-US"、"en-GB"、"es-CL")。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-317">(for example "en-US", "en-GB", "es-CL")</span></span>
* <span data-ttu-id="7ac3d-318">親カルチャ: 特定のカルチャを含むニュートラル カルチャ </span><span class="sxs-lookup"><span data-stu-id="7ac3d-318">Parent culture: The neutral culture that contains a specific culture.</span></span> <span data-ttu-id="7ac3d-319">(たとえば、"en" は "en-US" および "en-GB" の親カルチャです)。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-319">(for example, "en" is the parent culture of "en-US" and "en-GB")</span></span>
* <span data-ttu-id="7ac3d-320">ロケール: ロケールはカルチャと同じです。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-320">Locale: A locale is the same as a culture.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7ac3d-321">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="7ac3d-321">Additional resources</span></span>

* <span data-ttu-id="7ac3d-322">記事で使用されている [Localization.StarterWeb プロジェクト](https://github.com/aspnet/entropy)。</span><span class="sxs-lookup"><span data-stu-id="7ac3d-322">[Localization.StarterWeb project](https://github.com/aspnet/entropy) used in the article.</span></span>
* [<span data-ttu-id="7ac3d-323">Visual Studio のリソース ファイル</span><span class="sxs-lookup"><span data-stu-id="7ac3d-323">Resource Files in Visual Studio</span></span>](https://docs.microsoft.com/cpp/windows/resource-files-visual-studio)
* [<span data-ttu-id="7ac3d-324">.resx ファイル内のリソース</span><span class="sxs-lookup"><span data-stu-id="7ac3d-324">Resources in .resx Files</span></span>](https://docs.microsoft.com/dotnet/framework/resources/working-with-resx-files-programmatically)
