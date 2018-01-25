---
title: "ローカライズの移植性のオブジェクトを構成します。"
author: sebastienros
description: "この記事では、ポータブル オブジェクト ファイルを紹介し、Orchard のコア フレームワークと ASP.NET Core アプリケーションで使用するための手順の概要を示します。"
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/portable-object-localization
ms.openlocfilehash: ad68c8a7df5a8ea0f7ef42137c29cd3b37657052
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="configure-portable-object-localization-with-orchard-core"></a><span data-ttu-id="cfa21-103">Orchard コアでポータブル オブジェクト ローカリゼーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-103">Configure portable object localization with Orchard Core</span></span>

<span data-ttu-id="cfa21-104">によって[Sébastien Ros](https://github.com/sebastienros)と[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="cfa21-104">By [Sébastien Ros](https://github.com/sebastienros) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="cfa21-105">この記事で ASP.NET Core アプリケーションのポータブル オブジェクト (PO) ファイルを使用するための手順を追って、 [Orchard コア](https://github.com/OrchardCMS/OrchardCore)フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="cfa21-105">This article walks through the steps for using Portable Object (PO) files in an ASP.NET Core application with the [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span></span>

<span data-ttu-id="cfa21-106">**注:** Orchard コアが、Microsoft 製品はありません。</span><span class="sxs-lookup"><span data-stu-id="cfa21-106">**Note:** Orchard Core isn't a Microsoft product.</span></span> <span data-ttu-id="cfa21-107">したがって、マイクロソフトはサポートされませんこの機能します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-107">Consequently, Microsoft provides no support for this feature.</span></span>

<span data-ttu-id="cfa21-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="cfa21-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-po-file"></a><span data-ttu-id="cfa21-109">PO ファイルとは何ですか。</span><span class="sxs-lookup"><span data-stu-id="cfa21-109">What is a PO file?</span></span>

<span data-ttu-id="cfa21-110">PO ファイルは、特定の言語の翻訳された文字列を含むテキスト ファイルとして配布します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-110">PO files are distributed as text files containing the translated strings for a given language.</span></span> <span data-ttu-id="cfa21-111">PO ファイルを代わりに使用するいくつか利点*.resx*ファイルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="cfa21-111">Some advantages of using PO files instead *.resx* files include:</span></span>
- <span data-ttu-id="cfa21-112">PO ファイルは、複数形化; をサポートします。*.resx*複数形化をサポートしていないファイルです。</span><span class="sxs-lookup"><span data-stu-id="cfa21-112">PO files support pluralization; *.resx* files don't support pluralization.</span></span>
- <span data-ttu-id="cfa21-113">PO ファイルはのようにコンパイルされない*.resx*ファイル。</span><span class="sxs-lookup"><span data-stu-id="cfa21-113">PO files aren't compiled like *.resx* files.</span></span> <span data-ttu-id="cfa21-114">そのため、特別なツールとビルド手順は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="cfa21-114">As such, specialized tooling and build steps aren't required.</span></span>
- <span data-ttu-id="cfa21-115">PO ファイルは、オンライン編集ツールを共同でうまく機能します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-115">PO files work well with collaborative online editing tools.</span></span>

### <a name="example"></a><span data-ttu-id="cfa21-116">例</span><span class="sxs-lookup"><span data-stu-id="cfa21-116">Example</span></span>

<span data-ttu-id="cfa21-117">フランス語、その複数形の 1 つを含む 2 つの文字列の翻訳を含むサンプル PO ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-117">Here is a sample PO file containing the translation for two strings in French, including one with its plural form:</span></span>

<span data-ttu-id="cfa21-118">*fr.po*</span><span class="sxs-lookup"><span data-stu-id="cfa21-118">*fr.po*</span></span>

```text
#: Services/EmailService.cs:29
msgid "Enter a comma separated list of email addresses."
msgstr "Entrez une liste d'emails séparés par une virgule."

#: Views/Email.cshtml:112
msgid "The email address is \"{0}\"."
msgid_plural "The email addresses are \"{0}\"."
msgstr[0] "L'adresse email est \"{0}\"."
msgstr[1] "Les adresses email sont \"{0}\""
```

<span data-ttu-id="cfa21-119">この例では、次の構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-119">This example uses the following syntax:</span></span>

- <span data-ttu-id="cfa21-120">`#:`: 変換対象の文字列のコンテキストを示すコメントです。</span><span class="sxs-lookup"><span data-stu-id="cfa21-120">`#:`: A comment indicating the context of the string to be translated.</span></span> <span data-ttu-id="cfa21-121">同じ文字列が使用されているに応じて異なる方法で変換可能性があります。</span><span class="sxs-lookup"><span data-stu-id="cfa21-121">The same string might be translated differently depending on where it's being used.</span></span>
- <span data-ttu-id="cfa21-122">`msgid`: 無変換文字列。</span><span class="sxs-lookup"><span data-stu-id="cfa21-122">`msgid`: The untranslated string.</span></span>
- <span data-ttu-id="cfa21-123">`msgstr`: 翻訳された文字列。</span><span class="sxs-lookup"><span data-stu-id="cfa21-123">`msgstr`: The translated string.</span></span>

<span data-ttu-id="cfa21-124">複数形化のサポートの場合は、複数のエントリを定義できます。</span><span class="sxs-lookup"><span data-stu-id="cfa21-124">In the case of pluralization support, more entries can be defined.</span></span>

- <span data-ttu-id="cfa21-125">`msgid_plural`: 無変換複数形文字列。</span><span class="sxs-lookup"><span data-stu-id="cfa21-125">`msgid_plural`: The untranslated plural string.</span></span>
- <span data-ttu-id="cfa21-126">`msgstr[0]`: 0 の場合、翻訳された文字列。</span><span class="sxs-lookup"><span data-stu-id="cfa21-126">`msgstr[0]`: The translated string for the case 0.</span></span>
- <span data-ttu-id="cfa21-127">`msgstr[N]`: 大文字の n。 の翻訳された文字列</span><span class="sxs-lookup"><span data-stu-id="cfa21-127">`msgstr[N]`: The translated string for the case N.</span></span>

<span data-ttu-id="cfa21-128">PO ファイルの仕様を参照して[ここ](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html)です。</span><span class="sxs-lookup"><span data-stu-id="cfa21-128">The PO file specification can be found [here](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span></span>

## <a name="configuring-po-file-support-in-aspnet-core"></a><span data-ttu-id="cfa21-129">ASP.NET Core の PO ファイル サポートの構成</span><span class="sxs-lookup"><span data-stu-id="cfa21-129">Configuring PO file support in ASP.NET Core</span></span>

<span data-ttu-id="cfa21-130">この例は、Visual Studio 2017 プロジェクト テンプレートから生成された ASP.NET Core MVC アプリケーションに基づいています。</span><span class="sxs-lookup"><span data-stu-id="cfa21-130">This example is based on an ASP.NET Core MVC application generated from a Visual Studio 2017 project template.</span></span>

### <a name="referencing-the-package"></a><span data-ttu-id="cfa21-131">パッケージを参照します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-131">Referencing the package</span></span>

<span data-ttu-id="cfa21-132">参照を追加、 `OrchardCore.Localization.Core` NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="cfa21-132">Add a reference to the `OrchardCore.Localization.Core` NuGet package.</span></span> <span data-ttu-id="cfa21-133">使用可能になる[MyGet](https://www.myget.org/)次のパッケージ ソースで: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span><span class="sxs-lookup"><span data-stu-id="cfa21-133">It's available on [MyGet](https://www.myget.org/) at the following package source: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span></span>

<span data-ttu-id="cfa21-134">*.Csproj*ファイルにはここで、次のような行が含まれています (バージョン番号が異なる場合があります)。</span><span class="sxs-lookup"><span data-stu-id="cfa21-134">The *.csproj* file now contains a line similar to the following (version number may vary):</span></span>

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a><span data-ttu-id="cfa21-135">サービスを登録します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-135">Registering the service</span></span>

<span data-ttu-id="cfa21-136">必要なサービスを追加、`ConfigureServices`メソッドの*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="cfa21-136">Add the required services to the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

<span data-ttu-id="cfa21-137">必要なミドルウェアを追加、`Configure`メソッドの*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="cfa21-137">Add the required middleware to the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

<span data-ttu-id="cfa21-138">選択肢の Razor ビューには、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-138">Add the following code to your Razor view of choice.</span></span> <span data-ttu-id="cfa21-139">*About.cshtml*は、この例で使用します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-139">*About.cshtml* is used in this example.</span></span>

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

<span data-ttu-id="cfa21-140">`IViewLocalizer`インスタンスが挿入され、テキスト"Hello world!"を変換するために使用します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-140">An `IViewLocalizer` instance is injected and used to translate the text "Hello world!".</span></span>

### <a name="creating-a-po-file"></a><span data-ttu-id="cfa21-141">PO ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-141">Creating a PO file</span></span>

<span data-ttu-id="cfa21-142">という名前のファイルを作成する *<culture code>.po*アプリケーション ルート フォルダー。</span><span class="sxs-lookup"><span data-stu-id="cfa21-142">Create a file named *<culture code>.po* in your application root folder.</span></span> <span data-ttu-id="cfa21-143">この例では、ファイル名は*fr.po*フランス語の言語が使用されるため。</span><span class="sxs-lookup"><span data-stu-id="cfa21-143">In this example, the file name is *fr.po* because the French language is used:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

<span data-ttu-id="cfa21-144">このファイルは、変換する文字列とフランス語に翻訳された文字列の両方を格納します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-144">This file stores both the string to translate and the French-translated string.</span></span> <span data-ttu-id="cfa21-145">翻訳は、必要な場合、その親カルチャに戻します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-145">Translations revert to their parent culture, if necessary.</span></span> <span data-ttu-id="cfa21-146">この例では、 *fr.po*カルチャが要求された場合、ファイルが使用される`fr-FR`または`fr-CA`です。</span><span class="sxs-lookup"><span data-stu-id="cfa21-146">In this example, the *fr.po* file is used if the requested culture is `fr-FR` or `fr-CA`.</span></span>

### <a name="testing-the-application"></a><span data-ttu-id="cfa21-147">アプリケーションのテスト</span><span class="sxs-lookup"><span data-stu-id="cfa21-147">Testing the application</span></span>

<span data-ttu-id="cfa21-148">アプリケーションを実行し、URL に移動して`/Home/About`です。</span><span class="sxs-lookup"><span data-stu-id="cfa21-148">Run your application, and navigate to the URL `/Home/About`.</span></span> <span data-ttu-id="cfa21-149">テキスト**Hello world!**</span><span class="sxs-lookup"><span data-stu-id="cfa21-149">The text **Hello world!**</span></span> <span data-ttu-id="cfa21-150">表示されます。</span><span class="sxs-lookup"><span data-stu-id="cfa21-150">is displayed.</span></span>

<span data-ttu-id="cfa21-151">URL に移動`/Home/About?culture=fr-FR`です。</span><span class="sxs-lookup"><span data-stu-id="cfa21-151">Navigate to the URL `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="cfa21-152">テキスト**Bonjour le monde!**</span><span class="sxs-lookup"><span data-stu-id="cfa21-152">The text **Bonjour le monde!**</span></span> <span data-ttu-id="cfa21-153">表示されます。</span><span class="sxs-lookup"><span data-stu-id="cfa21-153">is displayed.</span></span>

## <a name="pluralization"></a><span data-ttu-id="cfa21-154">複数形化</span><span class="sxs-lookup"><span data-stu-id="cfa21-154">Pluralization</span></span>

<span data-ttu-id="cfa21-155">PO ファイルは、複数形化フォームをサポートし、これは、文字列と同じする必要がある変換基数に応じて異なる場合に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="cfa21-155">PO files support pluralization forms, which is useful when the same string needs to be translated differently based on a cardinality.</span></span> <span data-ttu-id="cfa21-156">このタスクは、各言語が基数に基づいて、使用する文字列を選択するカスタム ルールを定義するため、複雑なが作成されます。</span><span class="sxs-lookup"><span data-stu-id="cfa21-156">This task is made complicated by the fact that each language defines custom rules to select which string to use based on the cardinality.</span></span>

<span data-ttu-id="cfa21-157">Orchard ローカリゼーション パッケージでは、異なるこれら複数形のフォームを自動的に起動する API を提供します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-157">The Orchard Localization package provides an API to invoke these different plural forms automatically.</span></span>

### <a name="creating-pluralization-po-files"></a><span data-ttu-id="cfa21-158">複数形化 PO ファイルの作成</span><span class="sxs-lookup"><span data-stu-id="cfa21-158">Creating pluralization PO files</span></span>

<span data-ttu-id="cfa21-159">次の内容を追加する前に説明した*fr.po*ファイル。</span><span class="sxs-lookup"><span data-stu-id="cfa21-159">Add the following content to the previously mentioned *fr.po* file:</span></span>

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

<span data-ttu-id="cfa21-160">参照してください[PO ファイルは?](#what-is-a-po-file)この例では、各エントリが表す内容の説明。</span><span class="sxs-lookup"><span data-stu-id="cfa21-160">See [What is a PO file?](#what-is-a-po-file) for an explanation of what each entry in this example represents.</span></span>

### <a name="adding-a-language-using-different-pluralization-forms"></a><span data-ttu-id="cfa21-161">異なる複数形化フォームを使用する言語を追加します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-161">Adding a language using different pluralization forms</span></span>

<span data-ttu-id="cfa21-162">英語とフランス語の文字列は、前の例で使用されました。</span><span class="sxs-lookup"><span data-stu-id="cfa21-162">English and French strings were used in the previous example.</span></span> <span data-ttu-id="cfa21-163">英語とフランス語複数形化の 2 つの形式があり、同じ形式の規則を共有するいずれかの基数が最初の複数形にマップされています。</span><span class="sxs-lookup"><span data-stu-id="cfa21-163">English and French have only two pluralization forms and share the same form rules, which is that a cardinality of one is mapped to the first plural form.</span></span> <span data-ttu-id="cfa21-164">その他の任意の基数は、2 番目の複数形にマップされます。</span><span class="sxs-lookup"><span data-stu-id="cfa21-164">Any other cardinality is mapped to the second plural form.</span></span>

<span data-ttu-id="cfa21-165">すべての言語では、同じ規則を共有します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-165">Not all languages share the same rules.</span></span> <span data-ttu-id="cfa21-166">これを複数形の 3 つの形式を持つ、チェコ語の言語を示します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-166">This is illustrated with the Czech language, which has three plural forms.</span></span>

<span data-ttu-id="cfa21-167">作成、`cs.po`次のように、ファイルし、複数形化が次の 3 つの異なる翻訳必要がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="cfa21-167">Create the `cs.po` file as follows, and note how the pluralization needs three different translations:</span></span>

[!code-text[Main](localization/sample/POLocalization/cs.po)]

<span data-ttu-id="cfa21-168">チェコ語のローカライズ版は、そのまま追加`"cs"`でサポートされているカルチャの一覧に、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="cfa21-168">To accept Czech localizations, add `"cs"` to the list of supported cultures in the `ConfigureServices` method:</span></span>

```csharp
var supportedCultures = new List<CultureInfo>
{
    new CultureInfo("en-US"),
    new CultureInfo("en"),
    new CultureInfo("fr-FR"),
    new CultureInfo("fr"),
    new CultureInfo("cs")
};
```

<span data-ttu-id="cfa21-169">編集、 *Views/Home/About.cshtml*いくつかの基数のローカライズされた、複数形の文字列をレンダリングするファイル。</span><span class="sxs-lookup"><span data-stu-id="cfa21-169">Edit the *Views/Home/About.cshtml* file to render localized, plural strings for several cardinalities:</span></span>

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

<span data-ttu-id="cfa21-170">**注:**カウントを表すに実際のシナリオで変数が使用します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-170">**Note:** In a real world scenario, a variable would be used to represent the count.</span></span> <span data-ttu-id="cfa21-171">ここでは、非常に特殊なケースを公開する 3 つの異なる値を使用して同じコードを繰り返します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-171">Here, we repeat the same code with three different values to expose a very specific case.</span></span>

<span data-ttu-id="cfa21-172">カルチャを切り替えると、次をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="cfa21-172">Upon switching cultures, you see the following:</span></span>

<span data-ttu-id="cfa21-173">`/Home/About`の場合:</span><span class="sxs-lookup"><span data-stu-id="cfa21-173">For `/Home/About`:</span></span>

```html
There is one item.
There are 2 items.
There are 5 items.
```

<span data-ttu-id="cfa21-174">`/Home/About?culture=fr`の場合:</span><span class="sxs-lookup"><span data-stu-id="cfa21-174">For `/Home/About?culture=fr`:</span></span>

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

<span data-ttu-id="cfa21-175">`/Home/About?culture=cs`の場合:</span><span class="sxs-lookup"><span data-stu-id="cfa21-175">For `/Home/About?culture=cs`:</span></span>

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

<span data-ttu-id="cfa21-176">チェコ語のカルチャの 3 つの翻訳が異なることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="cfa21-176">Note that for the Czech culture, the three translations are different.</span></span> <span data-ttu-id="cfa21-177">フランス語と英語のカルチャでは、2 つの最後の翻訳された文字列を同じ構造を共有します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-177">The French and English cultures share the same construction for the two last translated strings.</span></span>

## <a name="advanced-tasks"></a><span data-ttu-id="cfa21-178">高度なタスク</span><span class="sxs-lookup"><span data-stu-id="cfa21-178">Advanced tasks</span></span>

### <a name="contextualizing-strings"></a><span data-ttu-id="cfa21-179">文字列を付けた</span><span class="sxs-lookup"><span data-stu-id="cfa21-179">Contextualizing strings</span></span>

<span data-ttu-id="cfa21-180">アプリケーションは、多くの場合、複数の場所で変換するために、文字列を含んでいます。</span><span class="sxs-lookup"><span data-stu-id="cfa21-180">Applications often contain the strings to be translated in several places.</span></span> <span data-ttu-id="cfa21-181">同じ文字列では、アプリ (Razor ビューまたはクラス ファイル) 内の特定の場所に別の翻訳があります。</span><span class="sxs-lookup"><span data-stu-id="cfa21-181">The same string may have a different translation in certain locations within an app (Razor views or class files).</span></span> <span data-ttu-id="cfa21-182">PO ファイルは、表される文字列の分類に使用することができます、ファイルのコンテキストの概念をサポートします。</span><span class="sxs-lookup"><span data-stu-id="cfa21-182">A PO file supports the notion of a file context, which can be used to categorize the string being represented.</span></span> <span data-ttu-id="cfa21-183">ファイルのコンテキストを使用して、文字列を翻訳できますが異なり、ファイルのコンテキスト (またはファイルのコンテキストが不足している) によって異なります。</span><span class="sxs-lookup"><span data-stu-id="cfa21-183">Using a file context, a string can be translated differently, depending on the file context (or lack of a file context).</span></span>

<span data-ttu-id="cfa21-184">PO ローカリゼーション サービスは、完全クラスまたは文字列を変換するときに使用されるビューの名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-184">The PO localization services use the name of the full class or the view that's used when translating a string.</span></span> <span data-ttu-id="cfa21-185">これは、値の設定、`msgctxt`エントリです。</span><span class="sxs-lookup"><span data-stu-id="cfa21-185">This is accomplished by setting the value on the `msgctxt` entry.</span></span>

<span data-ttu-id="cfa21-186">前に、マイナーの追加を検討してください*fr.po*例です。</span><span class="sxs-lookup"><span data-stu-id="cfa21-186">Consider a minor addition to the previous *fr.po* example.</span></span> <span data-ttu-id="cfa21-187">Razor ビューにある*Views/Home/About.cshtml*予約を設定して、ファイルのコンテキストとして定義できる`msgctxt`エントリの値。</span><span class="sxs-lookup"><span data-stu-id="cfa21-187">A Razor view located at *Views/Home/About.cshtml* can be defined as the file context by setting the reserved `msgctxt` entry's value:</span></span>

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

<span data-ttu-id="cfa21-188">`msgctxt`よう設定すると、テキストが変換に移動するとき`/Home/About?culture=fr-FR`です。</span><span class="sxs-lookup"><span data-stu-id="cfa21-188">With the `msgctxt` set as such, text translation occurs when navigating to `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="cfa21-189">移動するときに、変換が発生しない`/Home/Contact?culture=fr-FR`です。</span><span class="sxs-lookup"><span data-stu-id="cfa21-189">The translation won't occur when navigating to `/Home/Contact?culture=fr-FR`.</span></span>

<span data-ttu-id="cfa21-190">指定されたファイルのコンテキストで特定のエントリが一致しないときに、Orchard コアの代替機構は、コンテキストなしの適切なの PO ファイルを探します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-190">When no specific entry is matched with a given file context, Orchard Core's fallback mechanism looks for an appropriate PO file without a context.</span></span> <span data-ttu-id="cfa21-191">に対して定義されている特定のファイル コンテキストがあると仮定した場合は*Views/Home/Contact.cshtml*に間を移動する、`/Home/Contact?culture=fr-FR`など PO ファイルを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="cfa21-191">Assuming there's no specific file context defined for *Views/Home/Contact.cshtml*, navigating to `/Home/Contact?culture=fr-FR` loads a PO file such as:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a><span data-ttu-id="cfa21-192">PO ファイルの場所を変更します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-192">Changing the location of PO files</span></span>

<span data-ttu-id="cfa21-193">PO ファイルの既定の場所で変更できます`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="cfa21-193">The default location of PO files can be changed in `ConfigureServices`:</span></span>

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

<span data-ttu-id="cfa21-194">この例では、PO ファイルが読み込まれた、*ローカリゼーション*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="cfa21-194">In this example, the PO files are loaded from the *Localization* folder.</span></span>

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a><span data-ttu-id="cfa21-195">ローカライズされたファイルを検索するためのカスタム ロジックを実装します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-195">Implementing a custom logic for finding localization files</span></span>

<span data-ttu-id="cfa21-196">PO ファイルを検索するより複雑なロジックが必要なときに、`OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider`インターフェイスは実装されており、サービスとして登録されていることができます。</span><span class="sxs-lookup"><span data-stu-id="cfa21-196">When more complex logic is needed to locate PO files, the `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interface can be implemented and registered as a service.</span></span> <span data-ttu-id="cfa21-197">これは、PO ファイルは、さまざまな場所に保存することができる場合、またはファイルをフォルダー階層内にある必要があるときに便利です。</span><span class="sxs-lookup"><span data-stu-id="cfa21-197">This is useful when PO files can be stored in varying locations or when the files have to be found within a hierarchy of folders.</span></span>

### <a name="using-a-different-default-pluralized-language"></a><span data-ttu-id="cfa21-198">複数化ごとに異なる既定言語を使用してください。</span><span class="sxs-lookup"><span data-stu-id="cfa21-198">Using a different default pluralized language</span></span>

<span data-ttu-id="cfa21-199">パッケージに含まれる、`Plural`は複数形の 2 つの形式に固有の拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="cfa21-199">The package includes a `Plural` extension method that's specific to two plural forms.</span></span> <span data-ttu-id="cfa21-200">他の複数形のフォームを必要とする言語では、拡張メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="cfa21-200">For languages requiring more plural forms, create an extension method.</span></span> <span data-ttu-id="cfa21-201">既定の言語のすべてのローカライズ ファイルを提供する必要はありません、拡張メソッドを持つ&mdash;元の文字列は既に、コード内で直接使用できます。</span><span class="sxs-lookup"><span data-stu-id="cfa21-201">With an extension method, you won't need to provide any localization file for the default language &mdash; the original strings are already available directly in the code.</span></span>

<span data-ttu-id="cfa21-202">汎用的なを使用する`Plural(int count, string[] pluralForms, params object[] arguments)`を翻訳の文字列配列を受け入れるオーバー ロードします。</span><span class="sxs-lookup"><span data-stu-id="cfa21-202">You can use the more generic `Plural(int count, string[] pluralForms, params object[] arguments)` overload which accepts a string array of translations.</span></span>
