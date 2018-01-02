---
title: "グローバリゼーションとローカリゼーション ASP.NET Core"
author: rick-anderson
description: "コンテンツをさまざまな言語およびカルチャにローカライズするため、ASP.NET Core がミドルウェアとサービスを提供する方法について説明します。"
keywords: "ASP.NET Core、ローカリゼーション、カルチャ、言語、リソース ファイル、グローバリゼーション、国際化、ロケール"
ms.author: riande
manager: wpickett
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 7f275a09-f118-41c9-88d1-8de52d6a5aa1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/localization
ms.openlocfilehash: a3fdbf8a1ab4ca397824a46da445fa34ddd35204
ms.sourcegitcommit: 4be61844141d3cfb6f263636a36aebd26e90fb28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/22/2017
---
# <a name="globalization-and-localization-in-aspnet-core"></a><span data-ttu-id="0e878-104">グローバリゼーションとローカリゼーション ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e878-104">Globalization and localization in ASP.NET Core</span></span>

<span data-ttu-id="0e878-105">によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Damien Bowden](https://twitter.com/damien_bod)、 [Bart Calixto](https://twitter.com/bartmax)、 [Nadeem Afana](https://twitter.com/NadeemAfana)、および[Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="0e878-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [Bart Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://twitter.com/NadeemAfana), and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="0e878-106">ASP.NET Core 多言語 web サイトを作成すると、多数の対象者に到達するようにサイトが許可されます。</span><span class="sxs-lookup"><span data-stu-id="0e878-106">Creating a multilingual website with ASP.NET Core will allow your site to reach a wider audience.</span></span> <span data-ttu-id="0e878-107">ASP.NET Core は、さまざまな言語と文化にローカライズするためのサービスとミドルウェアを提供します。</span><span class="sxs-lookup"><span data-stu-id="0e878-107">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="0e878-108">国際化では、[グローバリゼーション](https://docs.microsoft.com/dotnet/api/system.globalization)と[ローカリゼーション](https://docs.microsoft.com/dotnet/standard/globalization-localization/localization)です。</span><span class="sxs-lookup"><span data-stu-id="0e878-108">Internationalization involves [Globalization](https://docs.microsoft.com/dotnet/api/system.globalization) and [Localization](https://docs.microsoft.com/dotnet/standard/globalization-localization/localization).</span></span> <span data-ttu-id="0e878-109">グローバリゼーションとは異なるカルチャをサポートするアプリを設計するプロセスです。</span><span class="sxs-lookup"><span data-stu-id="0e878-109">Globalization is the process of designing apps that support different cultures.</span></span> <span data-ttu-id="0e878-110">グローバリゼーションでは、入力、表示、および定義済みの一連の特定の地理的領域に関連する言語のスクリプトの出力のサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="0e878-110">Globalization adds support for input, display, and output of a defined set of language scripts that relate to specific geographic areas.</span></span>

<span data-ttu-id="0e878-111">ローカリゼーションとは、グローバライズされたアプリ、特定のカルチャとロケールに、ローカライズの可能性が既に処理に対応させるプロセス。</span><span class="sxs-lookup"><span data-stu-id="0e878-111">Localization is the process of adapting a globalized app, which you have already processed for localizability, to a particular culture/locale.</span></span>  <span data-ttu-id="0e878-112">詳細については、次を参照してください。**グローバリゼーションおよびローカリゼーションの条項**このドキュメントの末尾近くです。</span><span class="sxs-lookup"><span data-stu-id="0e878-112">For more information see **Globalization and localization terms** near the end of this document.</span></span>

<span data-ttu-id="0e878-113">アプリのローカライズには、次の項目が含まれます。</span><span class="sxs-lookup"><span data-stu-id="0e878-113">App localization involves the following:</span></span>

1. <span data-ttu-id="0e878-114">アプリのコンテンツをローカライズできるようにします</span><span class="sxs-lookup"><span data-stu-id="0e878-114">Make the app's content localizable</span></span>

2. <span data-ttu-id="0e878-115">言語およびサポートするカルチャのローカライズされたリソースを提供します。</span><span class="sxs-lookup"><span data-stu-id="0e878-115">Provide localized resources for the languages and cultures you support</span></span>

3. <span data-ttu-id="0e878-116">要求ごとに言語/カルチャを選択するための戦略を実装します。</span><span class="sxs-lookup"><span data-stu-id="0e878-116">Implement a strategy to select the language/culture for each request</span></span>

## <a name="make-the-apps-content-localizable"></a><span data-ttu-id="0e878-117">アプリのコンテンツをローカライズできるようにします</span><span class="sxs-lookup"><span data-stu-id="0e878-117">Make the app's content localizable</span></span>

<span data-ttu-id="0e878-118">ASP.NET Core で導入された`IStringLocalizer`と`IStringLocalizer<T>`ローカライズされたアプリの開発時に、生産性を向上させるために設計されています。</span><span class="sxs-lookup"><span data-stu-id="0e878-118">Introduced in ASP.NET Core, `IStringLocalizer` and `IStringLocalizer<T>` were architected to improve productivity when developing localized apps.</span></span> <span data-ttu-id="0e878-119">`IStringLocalizer`使用して、 [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager)と[ResourceReader](https://docs.microsoft.com/dotnet/api/system.resources.resourcereader)を実行時にカルチャ固有のリソースを提供します。</span><span class="sxs-lookup"><span data-stu-id="0e878-119">`IStringLocalizer` uses the [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager) and [ResourceReader](https://docs.microsoft.com/dotnet/api/system.resources.resourcereader) to provide culture-specific resources at run time.</span></span> <span data-ttu-id="0e878-120">シンプルなインターフェイスが、インデクサー、および`IEnumerable`ローカライズされた文字列を返すためです。</span><span class="sxs-lookup"><span data-stu-id="0e878-120">The simple interface has an indexer and an `IEnumerable` for returning localized strings.</span></span> <span data-ttu-id="0e878-121">`IStringLocalizer`リソース ファイルに既定の言語識別文字列を格納するには不要です。</span><span class="sxs-lookup"><span data-stu-id="0e878-121">`IStringLocalizer` doesn't require you to store the default language strings in a resource file.</span></span> <span data-ttu-id="0e878-122">開発の早い段階でリソース ファイルを作成する必要はなく、ローカリゼーションの対象となるアプリを開発できます。</span><span class="sxs-lookup"><span data-stu-id="0e878-122">You can develop an app targeted for localization and not need to create resource files early in development.</span></span> <span data-ttu-id="0e878-123">次のコードでは、ローカライズ文字列「タイトルは」をラップする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0e878-123">The code below shows how to wrap the string "About Title" for localization.</span></span>

[!code-csharp[Main](localization/sample/Localization/Controllers/AboutController.cs)]

<span data-ttu-id="0e878-124">上記のコードで、`IStringLocalizer<T>`実装に由来[依存性の注入](dependency-injection.md)です。</span><span class="sxs-lookup"><span data-stu-id="0e878-124">In the code above, the `IStringLocalizer<T>` implementation comes from [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="0e878-125">「タイトルは」のローカライズされた値が見つからないかどうかは、インデクサーのキーが返されます、つまり、文字列「タイトルは」です。</span><span class="sxs-lookup"><span data-stu-id="0e878-125">If the localized value of "About Title" is not found, then the indexer key is returned, that is, the string "About Title".</span></span> <span data-ttu-id="0e878-126">アプリで、既定の言語のリテラル文字列をそのままし、ローカライザーにラップされるよう、アプリの開発に専念することができます。</span><span class="sxs-lookup"><span data-stu-id="0e878-126">You can leave the default language literal strings in the app and wrap them in the localizer, so that you can focus on developing the app.</span></span> <span data-ttu-id="0e878-127">既定の言語でアプリを開発し、既定のリソース ファイルを作成しなくても、ローカリゼーション手順用に準備します。</span><span class="sxs-lookup"><span data-stu-id="0e878-127">You develop your app with your default language and prepare it for the localization step without first creating a default resource file.</span></span> <span data-ttu-id="0e878-128">または、従来のアプローチを使用し、既定の言語文字列を取得するキーを指定できます。</span><span class="sxs-lookup"><span data-stu-id="0e878-128">Alternatively, you can use the traditional approach and provide a key to retrieve the default language string.</span></span> <span data-ttu-id="0e878-129">多くの開発者にとって新しいワークフローの既定の言語がない*.resx*ファイルと、文字列リテラルをラップすることだけがアプリをローカライズするオーバーヘッドを削減できます。</span><span class="sxs-lookup"><span data-stu-id="0e878-129">For many developers the new workflow of not having a default language *.resx* file and simply wrapping the string literals can reduce the overhead of localizing an app.</span></span> <span data-ttu-id="0e878-130">他の開発者には、そのやすく長い文字列リテラルを操作およびローカライズされた文字列を更新しやすくように従来の作業の流れを選びます。</span><span class="sxs-lookup"><span data-stu-id="0e878-130">Other developers will prefer the traditional work flow as it can make it easier to work with longer string literals and make it easier to update localized strings.</span></span>

<span data-ttu-id="0e878-131">使用して、 `IHtmlLocalizer<T>` HTML を格納しているリソースの実装です。</span><span class="sxs-lookup"><span data-stu-id="0e878-131">Use the `IHtmlLocalizer<T>` implementation for resources that contain HTML.</span></span> <span data-ttu-id="0e878-132">`IHtmlLocalizer`HTML エンコードされたリソース文字列で書式設定される引数が、HTML ではありませんは、リソース文字列そのものをエンコードします。</span><span class="sxs-lookup"><span data-stu-id="0e878-132">`IHtmlLocalizer` HTML encodes arguments that are formatted in the resource string, but does not HTML encode the resource string itself.</span></span> <span data-ttu-id="0e878-133">サンプルでは、次の値のみ強調表示されます`name`パラメーターは、HTML エンコードします。</span><span class="sxs-lookup"><span data-stu-id="0e878-133">In the sample highlighted below, only the value of `name` parameter is HTML encoded.</span></span>

[!code-csharp[Main](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

<span data-ttu-id="0e878-134">**注:**のみテキストと HTML ではないをローカライズする場合、通常します。</span><span class="sxs-lookup"><span data-stu-id="0e878-134">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="0e878-135">最下位のレベルを取得できます`IStringLocalizerFactory`不在[依存性の注入](dependency-injection.md):</span><span class="sxs-lookup"><span data-stu-id="0e878-135">At the lowest level, you can get `IStringLocalizerFactory` out of [Dependency Injection](dependency-injection.md):</span></span>

[!code-csharp[Main](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

<span data-ttu-id="0e878-136">上記のコードを示します、2 つのファクトリの各メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="0e878-136">The code above demonstrates each of the two factory create methods.</span></span>

<span data-ttu-id="0e878-137">領域で、コント ローラーによって、ローカライズされた文字列をパーティション分割または 1 つのコンテナーを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="0e878-137">You can partition your localized strings by controller, area, or have just one container.</span></span> <span data-ttu-id="0e878-138">サンプル アプリで、ダミーという名前のクラス`SharedResource`共有リソースのために使用します。</span><span class="sxs-lookup"><span data-stu-id="0e878-138">In the sample app, a dummy class named `SharedResource` is used for shared resources.</span></span>

[!code-csharp[Main](localization/sample/Localization/Resources/SharedResource.cs)]

<span data-ttu-id="0e878-139">一部の開発者が使用して、`Startup`グローバルまたは共有の文字列を格納するクラス。</span><span class="sxs-lookup"><span data-stu-id="0e878-139">Some developers use the `Startup` class to contain global or shared strings.</span></span> <span data-ttu-id="0e878-140">以下のサンプルで、`InfoController`と`SharedResource`ローカライザーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="0e878-140">In the sample below, the `InfoController` and the `SharedResource` localizers are used:</span></span>

[!code-csharp[Main](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a><span data-ttu-id="0e878-141">ビューのローカライズ</span><span class="sxs-lookup"><span data-stu-id="0e878-141">View localization</span></span>

<span data-ttu-id="0e878-142">`IViewLocalizer`サービス提供のローカライズされた文字列、[ビュー](https://docs.microsoft.com/aspnet/core)です。</span><span class="sxs-lookup"><span data-stu-id="0e878-142">The `IViewLocalizer` service provides localized strings for a [view](https://docs.microsoft.com/aspnet/core).</span></span> <span data-ttu-id="0e878-143">`ViewLocalizer`クラスは、このインターフェイスを実装し、ビューのファイル パスからリソースの場所を検索します。</span><span class="sxs-lookup"><span data-stu-id="0e878-143">The `ViewLocalizer` class implements this interface and finds the resource location from the view file path.</span></span> <span data-ttu-id="0e878-144">次のコードは、の既定の実装を使用する方法を示しています`IViewLocalizer`:。</span><span class="sxs-lookup"><span data-stu-id="0e878-144">The following code shows how to use the default implementation of `IViewLocalizer`:</span></span>

[!code-cshtml[Main](localization/sample/Localization/Views/Home/About.cshtml)]

<span data-ttu-id="0e878-145">既定の実装`IViewLocalizer`ビューのファイルの名前に基づいてリソース ファイルを検索します。</span><span class="sxs-lookup"><span data-stu-id="0e878-145">The default implementation of `IViewLocalizer` finds the resource file based on the view's file name.</span></span> <span data-ttu-id="0e878-146">共有のグローバル リソース ファイルを使用するオプションはありません。</span><span class="sxs-lookup"><span data-stu-id="0e878-146">There is no option to use a global shared resource file.</span></span> <span data-ttu-id="0e878-147">`ViewLocalizer`実装を使用してローカライザー `IHtmlLocalizer`Razor しない HTML のため、ローカライズされた文字列をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="0e878-147">`ViewLocalizer` implements the localizer using `IHtmlLocalizer`, so Razor doesn't HTML encode the localized string.</span></span> <span data-ttu-id="0e878-148">リソース文字列をパラメーター化できると`IViewLocalizer`HTML エンコードされますが、パラメーター、リソース文字列ではありません。</span><span class="sxs-lookup"><span data-stu-id="0e878-148">You can parameterize resource strings and `IViewLocalizer` will HTML encode the parameters, but not the resource string.</span></span> <span data-ttu-id="0e878-149">次の Razor マークアップを考慮してください。</span><span class="sxs-lookup"><span data-stu-id="0e878-149">Consider the following Razor markup:</span></span>

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

<span data-ttu-id="0e878-150">フランス語のリソース ファイルに次が可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0e878-150">A French resource file could contain the following:</span></span>

| <span data-ttu-id="0e878-151">キー</span><span class="sxs-lookup"><span data-stu-id="0e878-151">Key</span></span> | <span data-ttu-id="0e878-152">[値]</span><span class="sxs-lookup"><span data-stu-id="0e878-152">Value</span></span> |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b> ` |

<span data-ttu-id="0e878-153">描画のビューには、リソース ファイルから HTML マークアップが含まれます。</span><span class="sxs-lookup"><span data-stu-id="0e878-153">The rendered view would contain the HTML markup from the resource file.</span></span>

<span data-ttu-id="0e878-154">**注:**のみテキストと HTML ではないをローカライズする場合、通常します。</span><span class="sxs-lookup"><span data-stu-id="0e878-154">**Note:** You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="0e878-155">ビュー内の共有リソース ファイルを使用するのには、挿入`IHtmlLocalizer<T>`:</span><span class="sxs-lookup"><span data-stu-id="0e878-155">To use a shared resource file in a view, inject `IHtmlLocalizer<T>`:</span></span>

[!code-cshtml[Main](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a><span data-ttu-id="0e878-156">DataAnnotations ローカリゼーション</span><span class="sxs-lookup"><span data-stu-id="0e878-156">DataAnnotations localization</span></span>

<span data-ttu-id="0e878-157">DataAnnotations エラー メッセージとローカライズ`IStringLocalizer<T>`です。</span><span class="sxs-lookup"><span data-stu-id="0e878-157">DataAnnotations error messages are localized with `IStringLocalizer<T>`.</span></span> <span data-ttu-id="0e878-158">オプションを使用して`ResourcesPath = "Resources"`、エラー メッセージで`RegisterViewModel`は次のパスのいずれかで格納されていることができます。</span><span class="sxs-lookup"><span data-stu-id="0e878-158">Using the option `ResourcesPath = "Resources"`, the error messages in `RegisterViewModel` can be stored in either of the following paths:</span></span>

* <span data-ttu-id="0e878-159">Resources/ViewModels.Account.RegisterViewModel.fr.resx</span><span class="sxs-lookup"><span data-stu-id="0e878-159">Resources/ViewModels.Account.RegisterViewModel.fr.resx</span></span>
* <span data-ttu-id="0e878-160">Resources/ViewModels/Account/RegisterViewModel.fr.resx</span><span class="sxs-lookup"><span data-stu-id="0e878-160">Resources/ViewModels/Account/RegisterViewModel.fr.resx</span></span>

[!code-csharp[Main](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

<span data-ttu-id="0e878-161">ASP.NET Core MVC 1.1.0 および以上、非検証属性のローカライズされています。</span><span class="sxs-lookup"><span data-stu-id="0e878-161">In ASP.NET Core MVC 1.1.0 and higher, non-validation attributes are localized.</span></span> <span data-ttu-id="0e878-162">ASP.NET Core MVC 1.0 は**いない**の非検証属性のローカライズされた文字列を検索します。</span><span class="sxs-lookup"><span data-stu-id="0e878-162">ASP.NET Core MVC 1.0 does **not** look up localized strings for non-validation attributes.</span></span>

<a name="one-resource-string-multiple-classes"></a>
### <a name="using-one-resource-string-for-multiple-classes"></a><span data-ttu-id="0e878-163">複数のクラスの 1 つのリソース文字列を使用します。</span><span class="sxs-lookup"><span data-stu-id="0e878-163">Using one resource string for multiple classes</span></span>

<span data-ttu-id="0e878-164">次のコードは、複数のクラスに検証属性の 1 つのリソース文字列を使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="0e878-164">The following code shows how to use one resource string for validation attributes with multiple classes:</span></span>

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

<span data-ttu-id="0e878-165">上記のコード、 `SharedResource` resx に対応するクラスには、検証メッセージを格納します。</span><span class="sxs-lookup"><span data-stu-id="0e878-165">In the preceeding code, `SharedResource` is the class corresponding to the resx where your validation messages are stored.</span></span> <span data-ttu-id="0e878-166">この方法を DataAnnotations を使ってのみ`SharedResource`、各クラスのリソースではなくです。</span><span class="sxs-lookup"><span data-stu-id="0e878-166">With this approach, DataAnnotations will only use `SharedResource`, rather than the resource for each class.</span></span> 

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a><span data-ttu-id="0e878-167">言語およびサポートするカルチャのローカライズされたリソースを提供します。</span><span class="sxs-lookup"><span data-stu-id="0e878-167">Provide localized resources for the languages and cultures you support</span></span>  

### <a name="supportedcultures-and-supporteduicultures"></a><span data-ttu-id="0e878-168">SupportedCultures と SupportedUICultures</span><span class="sxs-lookup"><span data-stu-id="0e878-168">SupportedCultures and SupportedUICultures</span></span>

<span data-ttu-id="0e878-169">ASP.NET Core では、2 つのカルチャ値を指定できます。`SupportedCultures`と`SupportedUICultures`です。</span><span class="sxs-lookup"><span data-stu-id="0e878-169">ASP.NET Core allows you to specify two culture values, `SupportedCultures` and `SupportedUICultures`.</span></span> <span data-ttu-id="0e878-170">[CultureInfo](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo)オブジェクトに対する`SupportedCultures`日付、時刻、数、および通貨の書式設定など、カルチャに依存する関数の結果を決定します。</span><span class="sxs-lookup"><span data-stu-id="0e878-170">The [CultureInfo](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo) object for `SupportedCultures` determines the results of culture-dependent functions, such as date, time, number, and currency formatting.</span></span> <span data-ttu-id="0e878-171">`SupportedCultures`テキスト、大文字と小文字の表記規則、および文字列比較の並べ替え順序を決定します。</span><span class="sxs-lookup"><span data-stu-id="0e878-171">`SupportedCultures` also determines the sorting order of text, casing conventions, and string comparisons.</span></span> <span data-ttu-id="0e878-172">参照してください[CultureInfo.CurrentCulture](https://docs.microsoft.com/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)サーバーがカルチャを取得する方法の詳細。</span><span class="sxs-lookup"><span data-stu-id="0e878-172">See [CultureInfo.CurrentCulture](https://docs.microsoft.com/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) for more info on how the server gets the Culture.</span></span> <span data-ttu-id="0e878-173">`SupportedUICultures`を決定し、これは文字列 (から*.resx*ファイル) を検索、 [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager)です。</span><span class="sxs-lookup"><span data-stu-id="0e878-173">The `SupportedUICultures` determines which translates strings (from *.resx* files) are looked up by the [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager).</span></span> <span data-ttu-id="0e878-174">`ResourceManager`によって決定されるカルチャ固有の文字列を単に検索`CurrentUICulture`です。</span><span class="sxs-lookup"><span data-stu-id="0e878-174">The `ResourceManager` simply looks up culture-specific strings that is determined by `CurrentUICulture`.</span></span> <span data-ttu-id="0e878-175">.NET のすべてのスレッドが`CurrentCulture`と`CurrentUICulture`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="0e878-175">Every thread in .NET has `CurrentCulture` and `CurrentUICulture` objects.</span></span> <span data-ttu-id="0e878-176">ASP.NET Core は、カルチャに依存する関数を表示するときに、これらの値を検査します。</span><span class="sxs-lookup"><span data-stu-id="0e878-176">ASP.NET Core inspects these values when rendering culture-dependent functions.</span></span> <span data-ttu-id="0e878-177">たとえば、現在のスレッドのカルチャが"EN-US"(英語、米国) に設定されている`DateTime.Now.ToLongDateString()`場合は、「木曜日、2016 年 2 月 18日」が表示されます`CurrentCulture`設定されている"ES-ES"(スペイン語、スペイン) に出力になります"jueves、18 de febrero de 2016" です。</span><span class="sxs-lookup"><span data-stu-id="0e878-177">For example, if the current thread's culture is set to "en-US" (English, United States), `DateTime.Now.ToLongDateString()` displays "Thursday, February 18, 2016", but if `CurrentCulture` is set to "es-ES" (Spanish, Spain) the output will be "jueves, 18 de febrero de 2016".</span></span>

## <a name="resource-files"></a><span data-ttu-id="0e878-178">リソース ファイル</span><span class="sxs-lookup"><span data-stu-id="0e878-178">Resource files</span></span>

<span data-ttu-id="0e878-179">リソース ファイルは、ローカライズ可能な文字列をコードから分離するために役立ちますメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="0e878-179">A resource file is a useful mechanism for separating localizable strings from code.</span></span> <span data-ttu-id="0e878-180">既定以外の言語の翻訳された文字列は分離*.resx*リソース ファイル。</span><span class="sxs-lookup"><span data-stu-id="0e878-180">Translated strings for the non-default language are isolated *.resx* resource files.</span></span> <span data-ttu-id="0e878-181">という名前のスペイン語のリソース ファイルを作成するなど、 *Welcome.es.resx*を含む文字列を変換します。</span><span class="sxs-lookup"><span data-stu-id="0e878-181">For example, you might want to create Spanish resource file named *Welcome.es.resx* containing translated strings.</span></span> <span data-ttu-id="0e878-182">"es"では、スペイン語の言語コードを示します。</span><span class="sxs-lookup"><span data-stu-id="0e878-182">"es" is the language code for Spanish.</span></span> <span data-ttu-id="0e878-183">Visual Studio でこのリソース ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="0e878-183">To create this resource file in Visual Studio:</span></span>

1. <span data-ttu-id="0e878-184">**ソリューション エクスプ ローラー**、リソース ファイルを格納するフォルダーを右クリックして >**追加** > **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="0e878-184">In **Solution Explorer**, right click on the folder which will contain the resource file > **Add** > **New Item**.</span></span>

    ![入れ子になったコンテキスト メニュー: ソリューション エクスプ ローラーでコンテキスト メニューは、リソース用に開かれています。](localization/_static/newi.png)

2. <span data-ttu-id="0e878-187">**検索には、テンプレートがインストールされている**ボックスで、「リソース」を入力し、ファイルの名前します。</span><span class="sxs-lookup"><span data-stu-id="0e878-187">In the **Search installed templates** box, enter "resource" and name the file.</span></span>

    ![[新しい項目の追加] ダイアログ](localization/_static/res.png)

3. <span data-ttu-id="0e878-189">キーの値 (ネイティブの文字列) で入力、**名前**列と翻訳された文字列を**値**列です。</span><span class="sxs-lookup"><span data-stu-id="0e878-189">Enter the key value (native string) in the **Name** column and the translated string in the **Value** column.</span></span>

    ![Hello の 名前 列と 値 列内の単語 Hola (スペイン語でのこんにちは) Welcome.es.resx ファイル (へようこそ のリソース ファイルのスペイン語)](localization/_static/hola.png)

    <span data-ttu-id="0e878-191">Visual Studio の表示、 *Welcome.es.resx*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0e878-191">Visual Studio shows the *Welcome.es.resx* file.</span></span>

    ![ソリューション エクスプ ローラーのようこそスペイン語 (es) のリソース ファイルを表示](localization/_static/se.png)

<a name="error"></a>

<span data-ttu-id="0e878-193">Visual Studio 2017 Preview バージョン 15.3 を使用している場合、リソース エディターでエラー インジケーターが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0e878-193">If you are using Visual Studio 2017 Preview version 15.3, you'll get an error indicator in the resource editor.</span></span> <span data-ttu-id="0e878-194">削除、 *ResXFileCodeGenerator*値から、*カスタム ツール*このエラー メッセージを防ぐためにプロパティ グリッド。</span><span class="sxs-lookup"><span data-stu-id="0e878-194">Remove the *ResXFileCodeGenerator*  value from the *Custom Tool* properties grid to prevent this error message:</span></span>

![Resx エディター](localization/_static/err.png)

<span data-ttu-id="0e878-196">また、このエラーを無視することができます。</span><span class="sxs-lookup"><span data-stu-id="0e878-196">Alternatively, you can ignore this error.</span></span> <span data-ttu-id="0e878-197">次のリリースでこの問題を解決する予定です。</span><span class="sxs-lookup"><span data-stu-id="0e878-197">We hope to fix this in the next release.</span></span>

## <a name="resource-file-naming"></a><span data-ttu-id="0e878-198">リソース ファイルの名前付け</span><span class="sxs-lookup"><span data-stu-id="0e878-198">Resource file naming</span></span>

<span data-ttu-id="0e878-199">リソースは、アセンブリ名を除いて、クラスの完全な型名の名前です。</span><span class="sxs-lookup"><span data-stu-id="0e878-199">Resources are named for the full type name of their class minus the assembly name.</span></span> <span data-ttu-id="0e878-200">フランス語のリソースをメインのアセンブリがプロジェクトになど`LocalizationWebsite.Web.dll`クラスの`LocalizationWebsite.Web.Startup`という名前になります*Startup.fr.resx*です。</span><span class="sxs-lookup"><span data-stu-id="0e878-200">For example, a French resource in a project whose main assembly is `LocalizationWebsite.Web.dll` for the class `LocalizationWebsite.Web.Startup` would be named *Startup.fr.resx*.</span></span> <span data-ttu-id="0e878-201">クラスのリソース`LocalizationWebsite.Web.Controllers.HomeController`という名前になります*Controllers.HomeController.fr.resx*です。</span><span class="sxs-lookup"><span data-stu-id="0e878-201">A resource for the class `LocalizationWebsite.Web.Controllers.HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="0e878-202">対象となるクラスの名前空間は、アセンブリ名と同じではない場合は、完全な型名を必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e878-202">If your targeted class's namespace is not the same as the assembly name you will need the full type name.</span></span> <span data-ttu-id="0e878-203">たとえば、サンプルには、プロジェクトの種類用のリソース`ExtraNamespace.Tools`という名前になります*ExtraNamespace.Tools.fr.resx*です。</span><span class="sxs-lookup"><span data-stu-id="0e878-203">For example, in the sample project a resource for the type `ExtraNamespace.Tools` would be named *ExtraNamespace.Tools.fr.resx*.</span></span>

<span data-ttu-id="0e878-204">サンプル プロジェクトで、`ConfigureServices`メソッドのセット、 `ResourcesPath` 「リソース」をそのため、home コント ローラーのフランス語りソース ファイルのプロジェクトの相対パスは*Resources/Controllers.HomeController.fr.resx*です。</span><span class="sxs-lookup"><span data-stu-id="0e878-204">In the sample project, the `ConfigureServices` method sets the `ResourcesPath` to "Resources", so the project relative path for the home controller's French resource file is *Resources/Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="0e878-205">また、リソース ファイルを整理するのにフォルダーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="0e878-205">Alternatively, you can use folders to organize resource files.</span></span> <span data-ttu-id="0e878-206">Home コント ローラーで、パスはなります*Resources/Controllers/HomeController.fr.resx*です。</span><span class="sxs-lookup"><span data-stu-id="0e878-206">For the home controller, the path would be *Resources/Controllers/HomeController.fr.resx*.</span></span> <span data-ttu-id="0e878-207">使用しない場合、`ResourcesPath`オプション、 *.resx*ファイルは、プロジェクトの基本ディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="0e878-207">If you don't use the `ResourcesPath` option, the *.resx* file would go in the project base directory.</span></span> <span data-ttu-id="0e878-208">リソース ファイルの`HomeController`という名前になります*Controllers.HomeController.fr.resx*です。</span><span class="sxs-lookup"><span data-stu-id="0e878-208">The resource file for `HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="0e878-209">ドットまたはパスの名前付け規則を選択した場合は、リソース ファイルを整理する方法によって異なります。</span><span class="sxs-lookup"><span data-stu-id="0e878-209">The choice of using the dot or path naming convention depends on how you want to organize your resource files.</span></span>

| <span data-ttu-id="0e878-210">リソース名</span><span class="sxs-lookup"><span data-stu-id="0e878-210">Resource name</span></span> | <span data-ttu-id="0e878-211">ドットまたはパスの名前を付ける</span><span class="sxs-lookup"><span data-stu-id="0e878-211">Dot or path naming</span></span> |
| ------------   | ------------- |
| <span data-ttu-id="0e878-212">Resources/Controllers.HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="0e878-212">Resources/Controllers.HomeController.fr.resx</span></span> | <span data-ttu-id="0e878-213">ドット</span><span class="sxs-lookup"><span data-stu-id="0e878-213">Dot</span></span>  |
| <span data-ttu-id="0e878-214">Resources/Controllers/HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="0e878-214">Resources/Controllers/HomeController.fr.resx</span></span>  | <span data-ttu-id="0e878-215">パス</span><span class="sxs-lookup"><span data-stu-id="0e878-215">Path</span></span> |
|    |     |

<span data-ttu-id="0e878-216">リソース ファイルを使用して`@inject IViewLocalizer`Razor ビューで、同様のパターンに従います。</span><span class="sxs-lookup"><span data-stu-id="0e878-216">Resource files using `@inject IViewLocalizer` in Razor views follow a similar pattern.</span></span> <span data-ttu-id="0e878-217">ビュー用のリソース ファイルは、ドット名前またはパスの名前付けを使用して名前付きことができます。</span><span class="sxs-lookup"><span data-stu-id="0e878-217">The resource file for a view can be named using either dot naming or path naming.</span></span> <span data-ttu-id="0e878-218">Razor ビューのリソース ファイルは、関連するビューのファイルのパスを模倣します。</span><span class="sxs-lookup"><span data-stu-id="0e878-218">Razor view resource files mimic the path of their associated view file.</span></span> <span data-ttu-id="0e878-219">設定と仮定した場合、`ResourcesPath`に「リソース」、フランス語のリソース ファイルに関連付けられて、 *Views/Home/About.cshtml*ビューでは、次のいずれかの可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0e878-219">Assuming we set the `ResourcesPath` to "Resources", the French resource file associated with the *Views/Home/About.cshtml* view could be either of the following:</span></span>

* <span data-ttu-id="0e878-220">Resources/Views/Home/About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="0e878-220">Resources/Views/Home/About.fr.resx</span></span>

* <span data-ttu-id="0e878-221">Resources/Views.Home.About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="0e878-221">Resources/Views.Home.About.fr.resx</span></span>

<span data-ttu-id="0e878-222">使用しない場合、`ResourcesPath`オプション、 *.resx*ファイルのビューは、ビューと同じフォルダーに配置するとします。</span><span class="sxs-lookup"><span data-stu-id="0e878-222">If you don't use the `ResourcesPath` option, the *.resx* file for a view would be located in the same folder as the view.</span></span>

## <a name="culture-fallback-behavior"></a><span data-ttu-id="0e878-223">カルチャ フォールバック動作</span><span class="sxs-lookup"><span data-stu-id="0e878-223">Culture fallback behavior</span></span>

<span data-ttu-id="0e878-224">例として、既定のリソース ファイルは読み取り".fr"カルチャの指定子を削除し、カルチャをフランス語に設定を使用している場合と、文字列はローカライズします。</span><span class="sxs-lookup"><span data-stu-id="0e878-224">As an example, if you remove the ".fr" culture designator and you have the culture set to French, the default resource file is read and strings are localized.</span></span> <span data-ttu-id="0e878-225">リソース マネージャーでは、既定値またはフォールバック リソースを何も行われませんが、要求されたカルチャを満たす場合に指定します。</span><span class="sxs-lookup"><span data-stu-id="0e878-225">The Resource manager designates a default or fallback resource for when nothing meets your requested culture.</span></span> <span data-ttu-id="0e878-226">要求されたカルチャのリソースが見つかりません必要があります、既定のリソース ファイルは持っていない場合にだけ、キーを返す場合は。</span><span class="sxs-lookup"><span data-stu-id="0e878-226">If you want to just return the key when missing a resource for the requested culture you must not have a default resource file.</span></span>

### <a name="generate-resource-files-with-visual-studio"></a><span data-ttu-id="0e878-227">Visual Studio でのリソース ファイルを生成します。</span><span class="sxs-lookup"><span data-stu-id="0e878-227">Generate resource files with Visual Studio</span></span>

<span data-ttu-id="0e878-228">Visual Studio で、ファイル名にカルチャせず、リソース ファイルを作成した場合 (たとえば、 *Welcome.resx*)、Visual Studio は各文字列のプロパティを使用して、c# クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="0e878-228">If you create a resource file in Visual Studio without a culture in the file name (for example, *Welcome.resx*), Visual Studio will create a C# class with a property for each string.</span></span> <span data-ttu-id="0e878-229">通常、たくないと ASP.NET Core です。通常、既定値がない*.resx*リソース ファイル (A *.resx*カルチャ名のないファイル)。</span><span class="sxs-lookup"><span data-stu-id="0e878-229">That's usually not what you want with ASP.NET Core; you typically don't have a default *.resx* resource file (A *.resx* file without the culture name).</span></span> <span data-ttu-id="0e878-230">作成することをお勧めします。、 *.resx*カルチャ名を持つファイル (たとえば*Welcome.fr.resx*)。</span><span class="sxs-lookup"><span data-stu-id="0e878-230">We suggest you create the *.resx* file with a culture name (for example *Welcome.fr.resx*).</span></span> <span data-ttu-id="0e878-231">作成するときに、 *.resx*カルチャ名、Visual Studio でのファイルは、クラス ファイルを生成しません。</span><span class="sxs-lookup"><span data-stu-id="0e878-231">When you create a *.resx* file with a culture name, Visual Studio will not generate the class file.</span></span> <span data-ttu-id="0e878-232">多くの開発者に**いない**既定の言語リソース ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="0e878-232">We anticipate that many developers will **not** create a default language resource file.</span></span>

### <a name="add-other-cultures"></a><span data-ttu-id="0e878-233">その他のカルチャを追加します。</span><span class="sxs-lookup"><span data-stu-id="0e878-233">Add Other Cultures</span></span>

<span data-ttu-id="0e878-234">(既定の言語) 以外の言語とカルチャの組み合わせごとに一意のリソース ファイルが必要です。</span><span class="sxs-lookup"><span data-stu-id="0e878-234">Each language and culture combination (other than the default language) requires a unique resource file.</span></span> <span data-ttu-id="0e878-235">ISO 言語コードがファイル名の一部となるリソース ファイルの新規作成して異なるカルチャとロケールのリソース ファイルを作成する (たとえば、 **en-us**、 **fr ca**、および**en gb**)。</span><span class="sxs-lookup"><span data-stu-id="0e878-235">You create resource files for different cultures and locales by creating new resource files in which the ISO language codes are part of the file name (for example, **en-us**, **fr-ca**, and **en-gb**).</span></span> <span data-ttu-id="0e878-236">これらの ISO コードを配置しているファイル名の間、 *.resx*ファイル名拡張子として*Welcome.es MX.resx* (スペイン語/メキシコ)。</span><span class="sxs-lookup"><span data-stu-id="0e878-236">These ISO codes are placed between the file name and the *.resx* file name extension, as in *Welcome.es-MX.resx* (Spanish/Mexico).</span></span> <span data-ttu-id="0e878-237">カルチャ ニュートラル言語を指定するには、国コードを削除 (`MX`前の例で)。</span><span class="sxs-lookup"><span data-stu-id="0e878-237">To specify a culturally neutral language, remove the country code (`MX` in the preceding example).</span></span> <span data-ttu-id="0e878-238">スペイン語のニュートラル カルチャのリソース ファイル名が*Welcome.es.resx*です。</span><span class="sxs-lookup"><span data-stu-id="0e878-238">The culturally neutral Spanish resource file name is *Welcome.es.resx*.</span></span>

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a><span data-ttu-id="0e878-239">要求ごとに言語/カルチャを選択するための戦略を実装します。</span><span class="sxs-lookup"><span data-stu-id="0e878-239">Implement a strategy to select the language/culture for each request</span></span>  

### <a name="configure-localization"></a><span data-ttu-id="0e878-240">ローカリゼーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="0e878-240">Configure localization</span></span>

<span data-ttu-id="0e878-241">ローカリゼーションがで構成されている、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="0e878-241">Localization is configured in the `ConfigureServices` method:</span></span>

[!code-csharp[Main](localization/sample/Localization/Program.cs?name=snippet1)]

* <span data-ttu-id="0e878-242">`AddLocalization`ローカリゼーション サービスをサービス コンテナーに追加します。</span><span class="sxs-lookup"><span data-stu-id="0e878-242">`AddLocalization` Adds the localization services to the services container.</span></span> <span data-ttu-id="0e878-243">上記のコードは、「リソース」のリソース パスを設定します。</span><span class="sxs-lookup"><span data-stu-id="0e878-243">The code above also sets the resources path to "Resources".</span></span>

* <span data-ttu-id="0e878-244">`AddViewLocalization`ファイルのローカライズされた表示のサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="0e878-244">`AddViewLocalization` Adds support for localized view files.</span></span> <span data-ttu-id="0e878-245">このサンプル ビューでのローカライズをビュー ファイルのサフィックスに基づきます。</span><span class="sxs-lookup"><span data-stu-id="0e878-245">In this sample view localization is based on the view file suffix.</span></span> <span data-ttu-id="0e878-246">たとえば"fr"で、 *Index.fr.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0e878-246">For example "fr" in the *Index.fr.cshtml* file.</span></span>

* <span data-ttu-id="0e878-247">`AddDataAnnotationsLocalization`ローカライズのサポートが追加されて`DataAnnotations`を介してメッセージを検証`IStringLocalizer`抽象化します。</span><span class="sxs-lookup"><span data-stu-id="0e878-247">`AddDataAnnotationsLocalization` Adds support for localized `DataAnnotations` validation messages through `IStringLocalizer` abstractions.</span></span>

### <a name="localization-middleware"></a><span data-ttu-id="0e878-248">ローカリゼーション ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="0e878-248">Localization middleware</span></span>

<span data-ttu-id="0e878-249">要求時に、現在のカルチャは、ローカリゼーションで設定[ミドルウェア](middleware.md)です。</span><span class="sxs-lookup"><span data-stu-id="0e878-249">The current culture on a request is set in the localization [Middleware](middleware.md).</span></span> <span data-ttu-id="0e878-250">ローカライズ ミドルウェアが有効になっている、`Configure`メソッドの*Program.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0e878-250">The localization middleware is enabled in the `Configure` method of *Program.cs* file.</span></span> <span data-ttu-id="0e878-251">要求のカルチャをチェックするすべてのミドルウェアの前に、ローカリゼーション ミドルウェアを構成する必要があります、注意してください (たとえば、 `app.UseMvcWithDefaultRoute()`)。</span><span class="sxs-lookup"><span data-stu-id="0e878-251">Note, the localization middleware must be configured before any middleware which might check the request culture (for example, `app.UseMvcWithDefaultRoute()`).</span></span>

[!code-csharp[Main](localization/sample/Localization/Program.cs?name=snippet2)]

<span data-ttu-id="0e878-252">`UseRequestLocalization`初期化、`RequestLocalizationOptions`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="0e878-252">`UseRequestLocalization` initializes a `RequestLocalizationOptions` object.</span></span> <span data-ttu-id="0e878-253">すべての要求リストの`RequestCultureProvider`で、`RequestLocalizationOptions`が列挙され、要求のカルチャが正常に決定できる最初のプロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="0e878-253">On every request the list of `RequestCultureProvider` in the `RequestLocalizationOptions` is enumerated and the first provider that can successfully determine the request culture is used.</span></span> <span data-ttu-id="0e878-254">既定のプロバイダーに由来、`RequestLocalizationOptions`クラス。</span><span class="sxs-lookup"><span data-stu-id="0e878-254">The default providers come from the `RequestLocalizationOptions` class:</span></span>

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

<span data-ttu-id="0e878-255">既定の一覧は、最も限定的に最も固有から移動します。</span><span class="sxs-lookup"><span data-stu-id="0e878-255">The default list goes from most specific to least specific.</span></span> <span data-ttu-id="0e878-256">記事の後半では、順序を変更し、であっても、カスタム プロバイダーを追加する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0e878-256">Later in the article we'll see how you can change the order and even add a custom culture provider.</span></span> <span data-ttu-id="0e878-257">要求のカルチャを判断、プロバイダーの場合、`DefaultRequestCulture`を使用します。</span><span class="sxs-lookup"><span data-stu-id="0e878-257">If none of the providers can determine the request culture, the `DefaultRequestCulture` is used.</span></span>

### <a name="querystringrequestcultureprovider"></a><span data-ttu-id="0e878-258">QueryStringRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="0e878-258">QueryStringRequestCultureProvider</span></span>

<span data-ttu-id="0e878-259">いくつかのアプリでは、クエリ文字列を使用して設定は、[カルチャおよび UI カルチャ](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="0e878-259">Some apps will use a query string to set the [culture and UI culture](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx).</span></span> <span data-ttu-id="0e878-260">をクッキーまたは Accept-language ヘッダーのアプローチを使用するアプリの URL にクエリ文字列を追加することはデバッグおよびコードのテストに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="0e878-260">For apps that use the cookie or Accept-Language header approach, adding a query string to the URL is useful for debugging and testing code.</span></span> <span data-ttu-id="0e878-261">既定では、`QueryStringRequestCultureProvider`の最初のローカリゼーション プロバイダーとして登録されて、 `RequestCultureProvider`  ボックスの一覧です。</span><span class="sxs-lookup"><span data-stu-id="0e878-261">By default, the `QueryStringRequestCultureProvider` is registered as the first localization provider in the `RequestCultureProvider` list.</span></span> <span data-ttu-id="0e878-262">クエリ文字列パラメーターを渡す`culture`と`ui-culture`です。</span><span class="sxs-lookup"><span data-stu-id="0e878-262">You pass the query string parameters `culture` and `ui-culture`.</span></span> <span data-ttu-id="0e878-263">次の例では、特定のカルチャ (言語および地域) をスペイン語/メキシコに設定します。</span><span class="sxs-lookup"><span data-stu-id="0e878-263">The following example sets the specific culture (language and region) to Spanish/Mexico:</span></span>

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

<span data-ttu-id="0e878-264">2 つのいずれかのみを渡す場合 (`culture`または`ui-culture`)、クエリ文字列のプロバイダーに渡された 1 つを使用して両方の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="0e878-264">If you only pass in one of the two (`culture` or `ui-culture`), the query string provider will set both values using the one you passed in.</span></span> <span data-ttu-id="0e878-265">たとえば、カルチャだけを設定は両方を設定、`Culture`と`UICulture`:</span><span class="sxs-lookup"><span data-stu-id="0e878-265">For example, setting just the culture will set both the `Culture` and the `UICulture`:</span></span>

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a><span data-ttu-id="0e878-266">CookieRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="0e878-266">CookieRequestCultureProvider</span></span>

<span data-ttu-id="0e878-267">実稼働アプリケーションは、ASP.NET Core カルチャ cookie を使用してカルチャを設定するためのメカニズムを提供して多くの場合、します。</span><span class="sxs-lookup"><span data-stu-id="0e878-267">Production apps will often provide a mechanism to set the culture with the ASP.NET Core culture cookie.</span></span> <span data-ttu-id="0e878-268">使用して、 `MakeCookieValue` cookie を作成します。</span><span class="sxs-lookup"><span data-stu-id="0e878-268">Use the `MakeCookieValue` method to create a cookie.</span></span>

<span data-ttu-id="0e878-269">`CookieRequestCultureProvider` `DefaultCookieName`カルチャ情報を指定したユーザーを追跡するために使用する既定の cookie 名を返します。</span><span class="sxs-lookup"><span data-stu-id="0e878-269">The `CookieRequestCultureProvider` `DefaultCookieName` returns the default cookie name used to track the user’s preferred culture information.</span></span> <span data-ttu-id="0e878-270">既定の cookie 名は"です。AspNetCore.Culture"です。</span><span class="sxs-lookup"><span data-stu-id="0e878-270">The default cookie  name is ".AspNetCore.Culture".</span></span>

<span data-ttu-id="0e878-271">Cookie の形式が`c=%LANGCODE%|uic=%LANGCODE%`ここで、`c`は`Culture`と`uic`は`UICulture`例。</span><span class="sxs-lookup"><span data-stu-id="0e878-271">The cookie format is `c=%LANGCODE%|uic=%LANGCODE%`, where `c` is `Culture` and `uic` is `UICulture`, for example:</span></span>

    c=en-UK|uic=en-US

<span data-ttu-id="0e878-272">のみを指定するカルチャ情報と UI カルチャのいずれかの場合、指定されたカルチャはカルチャ情報と UI カルチャの両方に使用されます。</span><span class="sxs-lookup"><span data-stu-id="0e878-272">If you only specify one of culture info and UI culture, the specified culture will be used for both culture info and UI culture.</span></span>

### <a name="the-accept-language-http-header"></a><span data-ttu-id="0e878-273">ブラウザーの言語の HTTP ヘッダー</span><span class="sxs-lookup"><span data-stu-id="0e878-273">The Accept-Language HTTP header</span></span>

<span data-ttu-id="0e878-274">[Accept-language ヘッダー](https://www.w3.org/International/questions/qa-accept-lang-locales)ほとんどのブラウザーで設定可能であり、ユーザーの言語を指定するためのものが最初。</span><span class="sxs-lookup"><span data-stu-id="0e878-274">The [Accept-Language header](https://www.w3.org/International/questions/qa-accept-lang-locales) is settable in most browsers and was originally intended to specify the user's language.</span></span> <span data-ttu-id="0e878-275">この設定は、ブラウザーが送信に設定されているかは、基になるオペレーティング システムから継承する新機能を示します。</span><span class="sxs-lookup"><span data-stu-id="0e878-275">This setting indicates what the browser has been set to send or has inherited from the underlying operating system.</span></span> <span data-ttu-id="0e878-276">ブラウザーの要求で Accept Language HTTP ヘッダーは、ユーザーの優先言語を検出するない方法ではありません (を参照してください[ブラウザーの言語設定を設定する](https://www.w3.org/International/questions/qa-lang-priorities.en.php))。</span><span class="sxs-lookup"><span data-stu-id="0e878-276">The Accept-Language HTTP header from a browser request is not an infallible way to detect the user's preferred language (see [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php)).</span></span> <span data-ttu-id="0e878-277">運用アプリには、ユーザーの任意のカルチャをカスタマイズする方法を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e878-277">A production app should include a way for a user to customize their choice of culture.</span></span>

### <a name="set-the-accept-language-http-header-in-ie"></a><span data-ttu-id="0e878-278">IE で Accept Language HTTP ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="0e878-278">Set the Accept-Language HTTP header in IE</span></span>

1. <span data-ttu-id="0e878-279">歯車アイコンをタップ**インターネット オプション**です。</span><span class="sxs-lookup"><span data-stu-id="0e878-279">From the gear icon, tap **Internet Options**.</span></span>

2. <span data-ttu-id="0e878-280">タップ**言語**します。</span><span class="sxs-lookup"><span data-stu-id="0e878-280">Tap **Languages**.</span></span>

    ![インターネット オプション](localization/_static/lang.png)

3. <span data-ttu-id="0e878-282">タップ**言語設定**です。</span><span class="sxs-lookup"><span data-stu-id="0e878-282">Tap **Set Language Preferences**.</span></span>

4. <span data-ttu-id="0e878-283">タップ**言語を追加**です。</span><span class="sxs-lookup"><span data-stu-id="0e878-283">Tap **Add a language**.</span></span>

5. <span data-ttu-id="0e878-284">言語を追加します。</span><span class="sxs-lookup"><span data-stu-id="0e878-284">Add the language.</span></span>

6. <span data-ttu-id="0e878-285">言語をタップしてタップ**上へ移動**です。</span><span class="sxs-lookup"><span data-stu-id="0e878-285">Tap the language, then tap **Move Up**.</span></span>

### <a name="use-a-custom-provider"></a><span data-ttu-id="0e878-286">カスタム プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="0e878-286">Use a custom provider</span></span>

<span data-ttu-id="0e878-287">お客様は、データベース内の言語とカルチャを格納できるようにするとします。</span><span class="sxs-lookup"><span data-stu-id="0e878-287">Suppose you want to let your customers store their language and culture in your databases.</span></span> <span data-ttu-id="0e878-288">ユーザーの姓名を検索するプロバイダーを記述することもできます。</span><span class="sxs-lookup"><span data-stu-id="0e878-288">You could write a provider to look up these values for the user.</span></span> <span data-ttu-id="0e878-289">次のコードは、カスタム プロバイダーを追加する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="0e878-289">The following code shows how to add a custom provider:</span></span>

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

<span data-ttu-id="0e878-290">使用して`RequestLocalizationOptions`を追加またはローカライズのプロバイダーを削除します。</span><span class="sxs-lookup"><span data-stu-id="0e878-290">Use `RequestLocalizationOptions` to add or remove localization providers.</span></span>

### <a name="set-the-culture-programmatically"></a><span data-ttu-id="0e878-291">カルチャをプログラムで設定します。</span><span class="sxs-lookup"><span data-stu-id="0e878-291">Set the culture programmatically</span></span>

<span data-ttu-id="0e878-292">このサンプル**Localization.StarterWeb**プロジェクトでは、 [GitHub](https://github.com/aspnet/entropy)を設定するための UI が含まれています、`Culture`です。</span><span class="sxs-lookup"><span data-stu-id="0e878-292">This sample **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) contains UI to set the `Culture`.</span></span> <span data-ttu-id="0e878-293">*Views/Shared/_SelectLanguagePartial.cshtml*ファイルでは、サポートされているカルチャの一覧から、カルチャを選択することができます。</span><span class="sxs-lookup"><span data-stu-id="0e878-293">The *Views/Shared/_SelectLanguagePartial.cshtml* file allows you to select the culture from the list of supported cultures:</span></span>

[!code-cshtml[Main](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

<span data-ttu-id="0e878-294">*Views/Shared/_SelectLanguagePartial.cshtml*にファイルが追加、`footer`できるすべてのビューに使用できるようにレイアウト ファイルのセクション。</span><span class="sxs-lookup"><span data-stu-id="0e878-294">The *Views/Shared/_SelectLanguagePartial.cshtml* file is added to the `footer` section of the layout file so it will be available to all views:</span></span>

[!code-cshtml[Main](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

<span data-ttu-id="0e878-295">`SetLanguage`メソッドはカルチャ cookie を設定します。</span><span class="sxs-lookup"><span data-stu-id="0e878-295">The `SetLanguage` method sets the culture cookie.</span></span>

[!code-csharp[Main](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

<span data-ttu-id="0e878-296">接続することはできません、 *_SelectLanguagePartial.cshtml*このプロジェクトのサンプル コードにします。</span><span class="sxs-lookup"><span data-stu-id="0e878-296">You can't plug in the *_SelectLanguagePartial.cshtml* to sample code for this project.</span></span> <span data-ttu-id="0e878-297">**Localization.StarterWeb**プロジェクトでは、 [GitHub](https://github.com/aspnet/entropy)フローするコードが含ま、`RequestLocalizationOptions`を通じて部分 Razor を[依存性の注入](dependency-injection.md)コンテナーです。</span><span class="sxs-lookup"><span data-stu-id="0e878-297">The **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) has code to flow the `RequestLocalizationOptions` to a Razor partial through the [Dependency Injection](dependency-injection.md) container.</span></span>

## <a name="globalization-and-localization-terms"></a><span data-ttu-id="0e878-298">グローバリゼーションとローカリゼーションの用語</span><span class="sxs-lookup"><span data-stu-id="0e878-298">Globalization and localization terms</span></span>

<span data-ttu-id="0e878-299">アプリをローカライズするプロセスは、最新のソフトウェアの開発でよく使用される、関連する文字セットの基本的な理解とそれらに関連した問題の理解にも必要です。</span><span class="sxs-lookup"><span data-stu-id="0e878-299">The process of localizing your app also requires a basic understanding of relevant character sets commonly used in modern software development and an understanding of the issues associated with them.</span></span> <span data-ttu-id="0e878-300">すべてのコンピューターは、数値 (コード) としてテキストを格納、さまざまなシステムは別の番号を使用して、同じテキストを格納します。</span><span class="sxs-lookup"><span data-stu-id="0e878-300">Although all computers store text as numbers (codes), different systems store the same text using different numbers.</span></span> <span data-ttu-id="0e878-301">ローカリゼーション処理は、特定のカルチャとロケールのアプリのユーザー インターフェイス (UI) の変換を指します。</span><span class="sxs-lookup"><span data-stu-id="0e878-301">The localization process refers to translating the app user interface (UI) for a specific culture/locale.</span></span>

<span data-ttu-id="0e878-302">[ローカライズ化](https://docs.microsoft.com/dotnet/standard/globalization-localization/localizability-review)は、グローバライズされたアプリのローカライズ可能を確認するための中間プロセスです。</span><span class="sxs-lookup"><span data-stu-id="0e878-302">[Localizability](https://docs.microsoft.com/dotnet/standard/globalization-localization/localizability-review) is an intermediate process for verifying that a globalized app is ready for localization.</span></span>

<span data-ttu-id="0e878-303">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt)カルチャ名の書式を設定"<languagecode2>-< country/regioncode2 >"ここで、<languagecode2>言語コードは、< country/regioncode2 > となりです。</span><span class="sxs-lookup"><span data-stu-id="0e878-303">The [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) format for the culture name is "<languagecode2>-<country/regioncode2>", where <languagecode2> is the language code and <country/regioncode2> is the subculture code.</span></span> <span data-ttu-id="0e878-304">たとえば、`es-CL`スペイン語 (チリ) 用`en-US`英語 (米国) の場合と`en-AU`英語 (オーストラリア) 用です。</span><span class="sxs-lookup"><span data-stu-id="0e878-304">For example, `es-CL` for Spanish (Chile), `en-US` for English (United States), and `en-AU` for English (Australia).</span></span> <span data-ttu-id="0e878-305">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) ISO 639 言語に関連付けられている 2 文字の小文字のカルチャ コードと国または地域に関連付けられている 2 文字の大文字となり、ISO 3166 の組み合わせです。</span><span class="sxs-lookup"><span data-stu-id="0e878-305">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) is a combination of an ISO 639 two-letter lowercase culture code associated with a language and an ISO 3166 two-letter uppercase subculture code associated with a country or region.</span></span>  <span data-ttu-id="0e878-306">参照してください[言語カルチャ名](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="0e878-306">See [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).</span></span>

<span data-ttu-id="0e878-307">国際化は、"I18N"に多くの場合、省略されています。</span><span class="sxs-lookup"><span data-stu-id="0e878-307">Internationalization is often abbreviated to "I18N".</span></span> <span data-ttu-id="0e878-308">省略形は、最初と最後の文字を受け取り、それらの間でため 18 文字の数は、最後の"N"、"I"、最初の文字の数を意味します。</span><span class="sxs-lookup"><span data-stu-id="0e878-308">The abbreviation takes the first and last letters and the number of letters between them, so 18 stands for the number of letters between the first "I" and the last "N".</span></span> <span data-ttu-id="0e878-309">(G11N) のグローバリゼーションおよびローカリゼーション (L10N) にも当てはまります。</span><span class="sxs-lookup"><span data-stu-id="0e878-309">The same applies to Globalization (G11N), and Localization (L10N).</span></span>

<span data-ttu-id="0e878-310">条件:</span><span class="sxs-lookup"><span data-stu-id="0e878-310">Terms:</span></span>

* <span data-ttu-id="0e878-311">グローバリゼーション (G11N): アプリ別の言語と地域をサポートするためのプロセスです。</span><span class="sxs-lookup"><span data-stu-id="0e878-311">Globalization (G11N): The process of making an app support different languages and regions.</span></span>
* <span data-ttu-id="0e878-312">ローカライズ (L10N): のカスタマイズ プロセスの特定の言語および地域のアプリです。</span><span class="sxs-lookup"><span data-stu-id="0e878-312">Localization (L10N): The process of customizing an app for a given language and region.</span></span>
* <span data-ttu-id="0e878-313">国際化 (I18N): は、グローバリゼーションおよびローカリゼーションの両方について説明します。</span><span class="sxs-lookup"><span data-stu-id="0e878-313">Internationalization (I18N): Describes both globalization and localization.</span></span>
* <span data-ttu-id="0e878-314">カルチャ: は、言語および、必要に応じて、領域です。</span><span class="sxs-lookup"><span data-stu-id="0e878-314">Culture: It is a language and, optionally, a region.</span></span>
* <span data-ttu-id="0e878-315">ニュートラル カルチャ: 指定した言語が地域ではないカルチャ。</span><span class="sxs-lookup"><span data-stu-id="0e878-315">Neutral culture: A culture that has a specified language, but not a region.</span></span> <span data-ttu-id="0e878-316">(たとえば"en"、"es")</span><span class="sxs-lookup"><span data-stu-id="0e878-316">(for example "en", "es")</span></span>
* <span data-ttu-id="0e878-317">特定のカルチャ: 指定された言語と地域を持つカルチャ。</span><span class="sxs-lookup"><span data-stu-id="0e878-317">Specific culture: A culture that has a specified language and region.</span></span> <span data-ttu-id="0e878-318">(例"EN-US"、"EN-GB"、"es CL") の</span><span class="sxs-lookup"><span data-stu-id="0e878-318">(for example "en-US", "en-GB", "es-CL")</span></span>
* <span data-ttu-id="0e878-319">カルチャの親: を含む特定のカルチャ ニュートラル カルチャです。</span><span class="sxs-lookup"><span data-stu-id="0e878-319">Parent culture: The neutral culture that contains a specific culture.</span></span> <span data-ttu-id="0e878-320">(たとえば、"en"は"EN-US"と"EN-GB"の親カルチャ)</span><span class="sxs-lookup"><span data-stu-id="0e878-320">(for example, "en" is the parent culture of "en-US" and "en-GB")</span></span>
* <span data-ttu-id="0e878-321">ロケール: ロケールとは、カルチャと同じです。</span><span class="sxs-lookup"><span data-stu-id="0e878-321">Locale: A locale is the same as a culture.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0e878-322">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="0e878-322">Additional resources</span></span>

* <span data-ttu-id="0e878-323">[Localization.StarterWeb プロジェクト](https://github.com/aspnet/entropy)アーティクルで使用します。</span><span class="sxs-lookup"><span data-stu-id="0e878-323">[Localization.StarterWeb project](https://github.com/aspnet/entropy) used in the article.</span></span>
* [<span data-ttu-id="0e878-324">Visual Studio でのリソース ファイル</span><span class="sxs-lookup"><span data-stu-id="0e878-324">Resource Files in Visual Studio</span></span>](https://docs.microsoft.com/cpp/windows/resource-files-visual-studio)
* [<span data-ttu-id="0e878-325">.Resx ファイル内のリソース</span><span class="sxs-lookup"><span data-stu-id="0e878-325">Resources in .resx Files</span></span>](https://docs.microsoft.com/dotnet/framework/resources/working-with-resx-files-programmatically)
