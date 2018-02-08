---
title: "ASP.NET Core でタグ ヘルパーを作成する"
author: rick-anderson
description: "ASP.NET Core でのタグ ヘルパーの作成方法を説明します。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 01/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/authoring
ms.openlocfilehash: 040c26bfccb8f258b0941bed4bc936cf7a16324a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="author-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a><span data-ttu-id="72eda-103">ASP.NET Core でのタグ ヘルパーの作成、チュートリアルとサンプル</span><span class="sxs-lookup"><span data-stu-id="72eda-103">Author Tag Helpers in ASP.NET Core, a walkthrough with samples</span></span>

<span data-ttu-id="72eda-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="72eda-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="72eda-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="72eda-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started-with-tag-helpers"></a><span data-ttu-id="72eda-106">タグ ヘルパーの概要</span><span class="sxs-lookup"><span data-stu-id="72eda-106">Get started with Tag Helpers</span></span>

<span data-ttu-id="72eda-107">このチュートリアルでは、タグ ヘルパーのプログラミングの概要を提供します。</span><span class="sxs-lookup"><span data-stu-id="72eda-107">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="72eda-108">[タグ ヘルパーの概要](intro.md)に関するページでは、タグ ヘルパーが提供する利点について説明しています。</span><span class="sxs-lookup"><span data-stu-id="72eda-108">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="72eda-109">タグ ヘルパーは、`ITagHelper` インターフェイス実装する任意のクラスです。</span><span class="sxs-lookup"><span data-stu-id="72eda-109">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="72eda-110">ただし、タグ ヘルパーを作成する場合は、通常は `TagHelper` から派生させます。こうすることで、`Process` メソッドにアクセスできるようになります。</span><span class="sxs-lookup"><span data-stu-id="72eda-110">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="72eda-111">**AuthoringTagHelpers** という新しい ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="72eda-111">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="72eda-112">このプロジェクトに認証は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="72eda-112">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="72eda-113">タグ ヘルパーを保持する *TagHelpers* というフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="72eda-113">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="72eda-114">*TagHelpers* フォルダーは必須では*ありません*が、あると便利です。</span><span class="sxs-lookup"><span data-stu-id="72eda-114">The *TagHelpers* folder is *not* required, but it's a reasonable convention.</span></span> <span data-ttu-id="72eda-115">それでは、いくつか単純なタグ ヘルパーを記述してみましょう。</span><span class="sxs-lookup"><span data-stu-id="72eda-115">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="72eda-116">最小のタグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="72eda-116">A minimal Tag Helper</span></span>

<span data-ttu-id="72eda-117">このセクションでは、電子メール タグを更新するタグ ヘルパーを記述します。</span><span class="sxs-lookup"><span data-stu-id="72eda-117">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="72eda-118">例:</span><span class="sxs-lookup"><span data-stu-id="72eda-118">For example:</span></span>

```html
<email>Support</email>
   ```

<span data-ttu-id="72eda-119">サーバーは、電子メール タグ ヘルパーを使用して、そのマークアップを以下に変換します。</span><span class="sxs-lookup"><span data-stu-id="72eda-119">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

<span data-ttu-id="72eda-120">つまり、これを電子メール リンクにするアンカー タグです。</span><span class="sxs-lookup"><span data-stu-id="72eda-120">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="72eda-121">ブログ エンジンを記述していて、マーケティング、サポート、およびその他の連絡先の電子メールをすべて同じドメインに送信するためにそれが必要な場合に、これを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="72eda-121">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1.  <span data-ttu-id="72eda-122">次の `EmailTagHelper` クラスを *TagHelpers* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="72eda-122">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    <span data-ttu-id="72eda-123">**注:**</span><span class="sxs-lookup"><span data-stu-id="72eda-123">**Notes:**</span></span>
    
    * <span data-ttu-id="72eda-124">タグ ヘルパーは、ルート クラス名の要素 (クラス名部分から *TagHelper* 部分を引いたもの) をターゲットとする名前付け規則です。</span><span class="sxs-lookup"><span data-stu-id="72eda-124">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="72eda-125">この例では、**Email**TagHelper のルート名は *email* となるため、`<email>` タグがターゲットとなります。</span><span class="sxs-lookup"><span data-stu-id="72eda-125">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="72eda-126">この命名規則は、ほとんどのタグ ヘルパーで機能します。上書きの方法については、後述します。</span><span class="sxs-lookup"><span data-stu-id="72eda-126">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
    * <span data-ttu-id="72eda-127">`EmailTagHelper` クラスは `TagHelper` から派生したものです。</span><span class="sxs-lookup"><span data-stu-id="72eda-127">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="72eda-128">`TagHelper` クラスはタグ ヘルパーを記述するためのメソッドとプロパティを提供します。</span><span class="sxs-lookup"><span data-stu-id="72eda-128">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
    * <span data-ttu-id="72eda-129">オーバーライドされた `Process` メソッドは、実行時のタグ ヘルパーの動作を制御します。</span><span class="sxs-lookup"><span data-stu-id="72eda-129">The overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="72eda-130">`TagHelper` クラスには、同じパラメーターを使用する非同期バージョン (`ProcessAsync`) も用意されています。</span><span class="sxs-lookup"><span data-stu-id="72eda-130">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
    * <span data-ttu-id="72eda-131">`Process` (および `ProcessAsync`) のコンテキスト パラメーターには、現在の HTML タグの実行に関連付けられている情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="72eda-131">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
    * <span data-ttu-id="72eda-132">`Process` (および `ProcessAsync`) の出力パラメーターには、HTML タグとコンテンツの生成に使用された元のソースを代表するステートフル HTML 要素が含まれています。</span><span class="sxs-lookup"><span data-stu-id="72eda-132">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
    * <span data-ttu-id="72eda-133">クラス名には **TagHelper** のサフィックスが付けられています。これは必須*ではありません*が、ベスト プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="72eda-133">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="72eda-134">クラスは、次のように宣言できます。</span><span class="sxs-lookup"><span data-stu-id="72eda-134">You could declare the class as:</span></span>
    
    ```csharp
    public class Email : TagHelper
    ```

2.  <span data-ttu-id="72eda-135">すべての Razor ビューで `EmailTagHelper` クラスを使用可能にするには、`addTagHelper` ディレクティブを *Views/_ViewImports.cshtml* ファイルに追加します。[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="72eda-135">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
    <span data-ttu-id="72eda-136">上記のコードでは、ワイルドカードの構文を使用して、アセンブリ内のすべてのタグ ヘルパーが使用可能になるように指定しています。</span><span class="sxs-lookup"><span data-stu-id="72eda-136">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="72eda-137">`@addTagHelper` の後の最初の文字列は、読み込むタグ ヘルパーを指定します (すべてのタグ ヘルパーを指定するには、"\*" を使用します)。2 番目の文字列 "AuthoringTagHelpers" は、タグ ヘルパーが存在するアセンブリを指定します。</span><span class="sxs-lookup"><span data-stu-id="72eda-137">The first string after `@addTagHelper` specifies the tag helper to load (Use "\*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="72eda-138">また、2 行目で、ワイルドカードの構文を使用して ASP.NET Core MVC タグ ヘルパーを取り込むことに注目してください (これらのヘルパーについては、[タグ ヘルパーの概要](intro.md)に関するページで説明されています)。Razor ビューでタグ ヘルパーを使用可能にするのが、`@addTagHelper` ディレクティブです。</span><span class="sxs-lookup"><span data-stu-id="72eda-138">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="72eda-139">または、次に示すように、タグ ヘルパーの完全修飾名 (FQN) を指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="72eda-139">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```
    
<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->
    
<span data-ttu-id="72eda-140">FQN を使用してタグ ヘルパーをビューに追加するには、最初に FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) を追加してから、アセンブリ名 (*AuthoringTagHelpers*) を追加します。</span><span class="sxs-lookup"><span data-stu-id="72eda-140">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="72eda-141">ほとんどの開発者は、ワイルドカードの構文を使用する方を選びます。</span><span class="sxs-lookup"><span data-stu-id="72eda-141">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="72eda-142">[タグ ヘルパーの概要](intro.md)に関するページでは、タグ ヘルパーの追加、削除、階層、およびワイルドカードの構文について詳しく説明されています。</span><span class="sxs-lookup"><span data-stu-id="72eda-142">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3.  <span data-ttu-id="72eda-143">これらの変更を加えて、*Views/Home/Contact.cshtml* ファイル内のマークアップを更新します。</span><span class="sxs-lookup"><span data-stu-id="72eda-143">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  <span data-ttu-id="72eda-144">アプリを実行して、お好みのブラウザーを使用して HTML ソースを表示すると、電子メール タグがアンカー マークアップ (`<a>Support</a>` など) に置き換えられていることが確認できます。</span><span class="sxs-lookup"><span data-stu-id="72eda-144">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="72eda-145">*Support* と *Marketing* はリンクとして表示されますが、リンクを機能させる `href` 属性を持っていません。</span><span class="sxs-lookup"><span data-stu-id="72eda-145">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="72eda-146">次のセクションでこれを修正します。</span><span class="sxs-lookup"><span data-stu-id="72eda-146">We'll fix that in the next section.</span></span>

<span data-ttu-id="72eda-147">注: HTML タグや属性と同様に、Razor および C# では、タグ、クラス名および属性の大文字と小文字は区別されません。</span><span class="sxs-lookup"><span data-stu-id="72eda-147">Note: Like HTML tags and attributes, tags, class names and attributes in Razor, and C# are not case-sensitive.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="72eda-148">SetAttribute と SetContent</span><span class="sxs-lookup"><span data-stu-id="72eda-148">SetAttribute and SetContent</span></span>

<span data-ttu-id="72eda-149">このセクションでは、`EmailTagHelper` を更新して、電子メール用の有効なアンカー タグが作成できるようにします。</span><span class="sxs-lookup"><span data-stu-id="72eda-149">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="72eda-150">これを更新して、Razor ビューから (`mail-to` 属性の形式で) 情報を取得し、それを使用してアンカーを生成します。</span><span class="sxs-lookup"><span data-stu-id="72eda-150">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="72eda-151">次のように `EmailTagHelper` クラスを更新します。</span><span class="sxs-lookup"><span data-stu-id="72eda-151">Update the `EmailTagHelper` class with the following:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

<span data-ttu-id="72eda-152">**注:**</span><span class="sxs-lookup"><span data-stu-id="72eda-152">**Notes:**</span></span>

* <span data-ttu-id="72eda-153">タグ ヘルパーのパスカル ケースのクラス名とプロパティ名は、それぞれ[小文字のケバブ ケース](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)に変換されます。</span><span class="sxs-lookup"><span data-stu-id="72eda-153">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="72eda-154">そのため、`MailTo` 属性を使用するには、相当する `<email mail-to="value"/>` を使用します。</span><span class="sxs-lookup"><span data-stu-id="72eda-154">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="72eda-155">最後の行は、最低限機能するタグ ヘルパーの完成したコンテンツを設定します。</span><span class="sxs-lookup"><span data-stu-id="72eda-155">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="72eda-156">強調表示されている行は、属性を追加するための構文を示しています。</span><span class="sxs-lookup"><span data-stu-id="72eda-156">The highlighted line shows the syntax for adding attributes:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

<span data-ttu-id="72eda-157">この方法は、属性 "href" が属性コレクションに現存していない場合に限り、属性 "href" に対して有効です。</span><span class="sxs-lookup"><span data-stu-id="72eda-157">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="72eda-158">`output.Attributes.Add` メソッドを使用してタグ ヘルパー属性をタグ属性のコレクションの末尾に追加することもできます。</span><span class="sxs-lookup"><span data-stu-id="72eda-158">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1.  <span data-ttu-id="72eda-159">これらの変更を加えて *Views/Home/Contact.cshtml* ファイル内のマークアップを更新します。[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="72eda-159">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2.  <span data-ttu-id="72eda-160">アプリを実行し、適切なリンクが生成されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="72eda-160">Run the app and verify that it generates the correct links.</span></span>
    
    > [!NOTE]
    ><span data-ttu-id="72eda-161">自己終了の電子メール タグ (`<email mail-to="Rick" />`) を記述すると、最終的な出力も自己終了になります。</span><span class="sxs-lookup"><span data-stu-id="72eda-161">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="72eda-162">開始タグのみを持つタグ (`<email mail-to="Rick">`) を記述する機能を有効にするには、次のようにクラスを装飾する必要があります。</span><span class="sxs-lookup"><span data-stu-id="72eda-162">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    <span data-ttu-id="72eda-163">自己終了の電子メール タグ ヘルパーでは、出力は `<a href="mailto:Rick@contoso.com" />` になります。</span><span class="sxs-lookup"><span data-stu-id="72eda-163">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="72eda-164">自己終了のアンカー タグは有効な HTML ではないため、作成することはないかもしれませんが、自己終了のタグ ヘルパーは必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="72eda-164">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that's self-closing.</span></span> <span data-ttu-id="72eda-165">タグ ヘルパーは、タグを読み取った後に、`TagMode` プロパティの型を設定します。</span><span class="sxs-lookup"><span data-stu-id="72eda-165">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="72eda-166">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="72eda-166">ProcessAsync</span></span>

<span data-ttu-id="72eda-167">このセクションでは非同期の電子メール ヘルパーを記述します。</span><span class="sxs-lookup"><span data-stu-id="72eda-167">In this section, we'll write an asynchronous email helper.</span></span>

1.  <span data-ttu-id="72eda-168">`EmailTagHelper` クラスを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="72eda-168">Replace the `EmailTagHelper` class with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    <span data-ttu-id="72eda-169">**注:**</span><span class="sxs-lookup"><span data-stu-id="72eda-169">**Notes:**</span></span>

    * <span data-ttu-id="72eda-170">このバージョンでは、非同期の `ProcessAsync` メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="72eda-170">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="72eda-171">非同期の `GetChildContentAsync` は `TagHelperContent` を含む `Task` を返します。</span><span class="sxs-lookup"><span data-stu-id="72eda-171">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

    * <span data-ttu-id="72eda-172">`output` パラメーターを使用して、HTML 要素のコンテンツを取得します。</span><span class="sxs-lookup"><span data-stu-id="72eda-172">Use the `output` parameter to get contents of the HTML element.</span></span>

2.  <span data-ttu-id="72eda-173">*Views/Home/Contact.cshtml* ファイルに次の変更を行って、タグ ヘルパーがターゲットの電子メールを取得できるようにします。</span><span class="sxs-lookup"><span data-stu-id="72eda-173">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  <span data-ttu-id="72eda-174">アプリを実行し、有効な電子メール リンクが生成されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="72eda-174">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="72eda-175">RemoveAll、PreContent.SetHtmlContent、PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="72eda-175">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1.  <span data-ttu-id="72eda-176">次の `BoldTagHelper` クラスを *TagHelpers* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="72eda-176">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    <span data-ttu-id="72eda-177">**注:**</span><span class="sxs-lookup"><span data-stu-id="72eda-177">**Notes:**</span></span>
    
    * <span data-ttu-id="72eda-178">`[HtmlTargetElement]` 属性は、"bold" という名前の HTML 属性を含むすべての HTML 要素を照合することを指定する属性パラメーターを渡し、クラス内で `Process` オーバーライド メソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="72eda-178">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="72eda-179">このサンプルでは、`Process` メソッドによって "bold" 属性が削除され、中にあるマークアップが `<strong></strong>` で囲まれます。</span><span class="sxs-lookup"><span data-stu-id="72eda-179">In our sample, the `Process` method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
    * <span data-ttu-id="72eda-180">既存のタグ コンテンツは置き換えたくないので、`PreContent.SetHtmlContent` メソッドを使用して開始タグ `<strong>` を記述し、`PostContent.SetHtmlContent` メソッドを使用して終了タグ `</strong>` を記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="72eda-180">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2.  <span data-ttu-id="72eda-181">`bold` 属性値を含めるように *About.cshtml* ビューを変更します。</span><span class="sxs-lookup"><span data-stu-id="72eda-181">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="72eda-182">完成したコードを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="72eda-182">The completed code is shown below.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  <span data-ttu-id="72eda-183">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="72eda-183">Run the app.</span></span> <span data-ttu-id="72eda-184">お好みのブラウザーを使用して、ソースを検査し、マークアップを確認できます。</span><span class="sxs-lookup"><span data-stu-id="72eda-184">You can use your favorite browser to inspect the source and verify the markup.</span></span>

    <span data-ttu-id="72eda-185">上記の `[HtmlTargetElement]` 属性は、"bold" という属性名を提供する HTML マークアップのみをターゲットにしています。</span><span class="sxs-lookup"><span data-stu-id="72eda-185">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="72eda-186">`<bold>` 要素は、タグ ヘルパーによって変更されませんでした。</span><span class="sxs-lookup"><span data-stu-id="72eda-186">The `<bold>` element wasn't modified by the tag helper.</span></span>

4. <span data-ttu-id="72eda-187">`[HtmlTargetElement]` 属性の行をコメント アウトすると、既定の `<bold>` タグ (`<bold>` 形式の HTML マークアップ) が再度ターゲットになります。</span><span class="sxs-lookup"><span data-stu-id="72eda-187">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="72eda-188">既定の名前付け規則は、クラス名 **Bold**TagHelper を `<bold>` タグと一致させることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="72eda-188">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="72eda-189">アプリを実行して、`<bold>` タグがタグ ヘルパーによって処理されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="72eda-189">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="72eda-190">複数の `[HtmlTargetElement]` 属性でクラスを修飾すると、結果はターゲットの論理 OR になります。</span><span class="sxs-lookup"><span data-stu-id="72eda-190">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="72eda-191">たとえば、次のコードを使用すると、bold タグまたは bold 属性が一致します。</span><span class="sxs-lookup"><span data-stu-id="72eda-191">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

<span data-ttu-id="72eda-192">同じステートメントに複数の属性を追加すると、ランタイムではそれらが論理 AND として扱われます。</span><span class="sxs-lookup"><span data-stu-id="72eda-192">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="72eda-193">たとえば、次のコードでは、一致させるために、"bold" という名前の属性 (`<bold bold />`) を使って HTML 要素に "bold" と名前を付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="72eda-193">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="72eda-194">また、`[HtmlTargetElement]` を使用してターゲット要素の名前を変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="72eda-194">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="72eda-195">たとえば、`BoldTagHelper` で `<MyBold>` タグをターゲットにするには、次の属性を使用します。</span><span class="sxs-lookup"><span data-stu-id="72eda-195">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="pass-a-model-to-a-tag-helper"></a><span data-ttu-id="72eda-196">タグ ヘルパーにモデルを渡す</span><span class="sxs-lookup"><span data-stu-id="72eda-196">Pass a model to a Tag Helper</span></span>

1.  <span data-ttu-id="72eda-197">*Models* フォルダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="72eda-197">Add a *Models* folder.</span></span>

2.  <span data-ttu-id="72eda-198">次の `WebsiteContext` クラスを *Models* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="72eda-198">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  <span data-ttu-id="72eda-199">次の `WebsiteInformationTagHelper` クラスを *TagHelpers* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="72eda-199">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    <span data-ttu-id="72eda-200">**注:**</span><span class="sxs-lookup"><span data-stu-id="72eda-200">**Notes:**</span></span>
    
    * <span data-ttu-id="72eda-201">前述したように、タグ ヘルパーは、パスカルケースの C# クラス名とタグ ヘルパーのプロパティを[小文字のケバブ ケース](http://wiki.c2.com/?KebabCase)に変換します。</span><span class="sxs-lookup"><span data-stu-id="72eda-201">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="72eda-202">そのため、`WebsiteInformationTagHelper` を Razor で使用するには、`<website-information />` を記述します。</span><span class="sxs-lookup"><span data-stu-id="72eda-202">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
    * <span data-ttu-id="72eda-203">`[HtmlTargetElement]` 属性を使用してターゲット要素を明示的に識別しないため、`website-information` の既定がターゲットになります。</span><span class="sxs-lookup"><span data-stu-id="72eda-203">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="72eda-204">次の属性を適用した場合 (ケバブ ケースではありませんが、クラス名が一致します):</span><span class="sxs-lookup"><span data-stu-id="72eda-204">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    <span data-ttu-id="72eda-205">小文字のケバブ ケース タグ `<website-information />` は一致しません。</span><span class="sxs-lookup"><span data-stu-id="72eda-205">The lower kebab case tag `<website-information />` wouldn't match.</span></span> <span data-ttu-id="72eda-206">`[HtmlTargetElement]` 属性を使用する場合は、次に示すようにケバブ ケースを使用します。</span><span class="sxs-lookup"><span data-stu-id="72eda-206">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * <span data-ttu-id="72eda-207">自己終了の要素にはコンテンツがありません。</span><span class="sxs-lookup"><span data-stu-id="72eda-207">Elements that are self-closing have no content.</span></span> <span data-ttu-id="72eda-208">この例では、Razor マークアップは自己終了タグを使用しますが、タグ ヘルパーは [section](http://www.w3.org/TR/html5/sections.html#the-section-element) 要素を作成します (これは自己終了ではなく、`section` 要素内のコンテンツを記述しています)。</span><span class="sxs-lookup"><span data-stu-id="72eda-208">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which isn't self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="72eda-209">そのため、`TagMode` を `StartTagAndEndTag` に設定して出力を記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="72eda-209">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="72eda-210">または、`TagMode` を設定する行をコメント アウトして、終了タグを使ってマークアップを記述することもできます。</span><span class="sxs-lookup"><span data-stu-id="72eda-210">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="72eda-211">(サンプルのマークアップは、後ほどこのチュートリアルで提供します。)</span><span class="sxs-lookup"><span data-stu-id="72eda-211">(Example markup is provided later in this tutorial.)</span></span>
    
    * <span data-ttu-id="72eda-212">次の行の `$` (ドル記号) は、[挿入文字列](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings)を使用しています。</span><span class="sxs-lookup"><span data-stu-id="72eda-212">The `$` (dollar sign) in the following line uses an [interpolated string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  <span data-ttu-id="72eda-213">*About.cshtml* ビューに次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="72eda-213">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="72eda-214">強調表示されたマークアップは、Web サイトの情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="72eda-214">The highlighted markup displays the web site information.</span></span>
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > <span data-ttu-id="72eda-215">Razor マークアップを次に示します。</span><span class="sxs-lookup"><span data-stu-id="72eda-215">In the Razor markup shown below:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    ><span data-ttu-id="72eda-216">Razor は `info` 属性を文字列ではなくクラスとして認識し、ユーザーは C# コードを記述します。</span><span class="sxs-lookup"><span data-stu-id="72eda-216">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="72eda-217">文字列以外のタグ ヘルパー属性はすべて、`@` 文字を使わずに記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="72eda-217">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5.  <span data-ttu-id="72eda-218">アプリを実行し、[バージョン情報] ビューに移動して、Web サイトの情報を確認します。</span><span class="sxs-lookup"><span data-stu-id="72eda-218">Run the app, and navigate to the About view to see the web site information.</span></span>

    >[!NOTE]
    ><span data-ttu-id="72eda-219">終了タグを持つ次のマークアップを使用して、タグ ヘルパー内の `TagMode.StartTagAndEndTag` を持つ行を削除できます。</span><span class="sxs-lookup"><span data-stu-id="72eda-219">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a><span data-ttu-id="72eda-220">条件タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="72eda-220">Condition Tag Helper</span></span>

<span data-ttu-id="72eda-221">条件タグ ヘルパーは、true 値が渡されたときに出力をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="72eda-221">The condition tag helper renders output when passed a true value.</span></span>

1.  <span data-ttu-id="72eda-222">次の `ConditionTagHelper` クラスを *TagHelpers* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="72eda-222">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  <span data-ttu-id="72eda-223">*Views/Home/Index.cshtml* ファイルの内容を次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="72eda-223">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=@Model />
        <div condition="@Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  <span data-ttu-id="72eda-224">`Home` コントローラーの `Index` メソッドを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="72eda-224">Replace the `Index` method in the `Home` controller with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  <span data-ttu-id="72eda-225">アプリを実行して、ホーム ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="72eda-225">Run the app and browse to the home page.</span></span> <span data-ttu-id="72eda-226">条件付き `div` ではマークアップはレンダリングされません。</span><span class="sxs-lookup"><span data-stu-id="72eda-226">The markup in the conditional `div` won't be rendered.</span></span> <span data-ttu-id="72eda-227">クエリ文字列 `?approved=true` を URL に付加します (例: `http://localhost:1235/Home/Index?approved=true`)。</span><span class="sxs-lookup"><span data-stu-id="72eda-227">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="72eda-228">`approved` は true に設定され、条件付きマークアップが表示されます。</span><span class="sxs-lookup"><span data-stu-id="72eda-228">`approved` is set to true and the conditional markup will be displayed.</span></span>

>[!NOTE]
><span data-ttu-id="72eda-229">[nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) 演算子を使用して、太字タグ ヘルパーで行ったように文字列を指定するのではなく、ターゲットにする属性を指定します。</span><span class="sxs-lookup"><span data-stu-id="72eda-229">Use the [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
><span data-ttu-id="72eda-230">[nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) 演算子は、コードがリファクタリングされないように保護します (名前は `RedCondition` に変更できます)。</span><span class="sxs-lookup"><span data-stu-id="72eda-230">The [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoid-tag-helper-conflicts"></a><span data-ttu-id="72eda-231">タグ ヘルパーの競合を回避する</span><span class="sxs-lookup"><span data-stu-id="72eda-231">Avoid Tag Helper conflicts</span></span>

<span data-ttu-id="72eda-232">このセクションでは、自動リンク タグ ヘルパーのペアを記述します。</span><span class="sxs-lookup"><span data-stu-id="72eda-232">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="72eda-233">1 つ目のヘルパーは HTTP で始まる URL を含むマークアップを、同じ URL を含む HTML アンカー タグに置き換えます (この URL へのリンクも生成されます)。</span><span class="sxs-lookup"><span data-stu-id="72eda-233">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="72eda-234">2 つ目のヘルパーは WWW で始まる URL に対して同じことを行います。</span><span class="sxs-lookup"><span data-stu-id="72eda-234">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="72eda-235">これら 2 つのヘルパーは密接に関連しており、将来的にそれらをリファクターする可能性があるため、これらを同じファイルで保持します。</span><span class="sxs-lookup"><span data-stu-id="72eda-235">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1.  <span data-ttu-id="72eda-236">次の `AutoLinkerHttpTagHelper` クラスを *TagHelpers* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="72eda-236">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    ><span data-ttu-id="72eda-237">`AutoLinkerHttpTagHelper` クラスは `p` 要素をターゲットとし、[Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) を使用してアンカーを作成します。</span><span class="sxs-lookup"><span data-stu-id="72eda-237">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

2.  <span data-ttu-id="72eda-238">次のマークアップを *Views/Home/Contact.cshtml* ファイルの末尾に追加します。</span><span class="sxs-lookup"><span data-stu-id="72eda-238">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  <span data-ttu-id="72eda-239">アプリを実行し、タグ ヘルパーがアンカーを正しくレンダリングすることを確認します。</span><span class="sxs-lookup"><span data-stu-id="72eda-239">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4.  <span data-ttu-id="72eda-240">`AutoLinker` クラスを更新して `AutoLinkerWwwTagHelper` を含めます。これは www テキストを元の www テキストも含むアンカー タグに変換します。</span><span class="sxs-lookup"><span data-stu-id="72eda-240">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="72eda-241">以下では、更新されたコードが強調表示されています。</span><span class="sxs-lookup"><span data-stu-id="72eda-241">The updated code is highlighted below:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  <span data-ttu-id="72eda-242">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="72eda-242">Run the app.</span></span> <span data-ttu-id="72eda-243">www テキストはリンクとしてレンダリングされていますが、HTTP テキストはそのようになっていないことに注目してください。</span><span class="sxs-lookup"><span data-stu-id="72eda-243">Notice the www text is rendered as a link but the HTTP text isn't.</span></span> <span data-ttu-id="72eda-244">両方のクラスにブレークポイントを配置すると、HTTP タグ ヘルパー クラスが最初に実行されることがわかります。</span><span class="sxs-lookup"><span data-stu-id="72eda-244">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="72eda-245">問題は、タグ ヘルパーの出力がキャッシュされ、WWW タグ ヘルパーを実行するときに、WWW タグ ヘルパーが HTTP タグ ヘルパーからのキャッシュされた出力を上書きすることです。</span><span class="sxs-lookup"><span data-stu-id="72eda-245">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="72eda-246">タグ ヘルパーの実行順序を制御する方法については、このチュートリアルで後ほど説明します。</span><span class="sxs-lookup"><span data-stu-id="72eda-246">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="72eda-247">コードを次のように修正します。</span><span class="sxs-lookup"><span data-stu-id="72eda-247">We'll fix the code with the following:</span></span>

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    ><span data-ttu-id="72eda-248">自動リンク タグ ヘルパーの最初のエディションで、次のコードを使用してターゲットのコンテンツを取得しました。</span><span class="sxs-lookup"><span data-stu-id="72eda-248">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    ><span data-ttu-id="72eda-249">つまり、`ProcessAsync` メソッドに渡された `TagHelperOutput` を使用して `GetChildContentAsync` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="72eda-249">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="72eda-250">前述したように、出力はキャッシュされるため、最後に実行されたタグ ヘルパーによって上書きされます。</span><span class="sxs-lookup"><span data-stu-id="72eda-250">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="72eda-251">次のコードを使用してこの問題を修正しました。</span><span class="sxs-lookup"><span data-stu-id="72eda-251">You fixed that problem with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    ><span data-ttu-id="72eda-252">上記のコードは、コンテンツが変更されているかどうかをチェックし、変更されている場合は、出力バッファーからコンテンツを取得します。</span><span class="sxs-lookup"><span data-stu-id="72eda-252">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6.  <span data-ttu-id="72eda-253">アプリを実行して、2 つのリンクが期待どおりに動作することを確認します。</span><span class="sxs-lookup"><span data-stu-id="72eda-253">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="72eda-254">自動リンカー タグ ヘルパーが正しく完全に機能しているように見えても、微妙な問題があります。</span><span class="sxs-lookup"><span data-stu-id="72eda-254">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="72eda-255">WWW タグ ヘルパーを最初に実行すると、www リンクが不正確になります。</span><span class="sxs-lookup"><span data-stu-id="72eda-255">If the WWW tag helper runs first, the www links won't be correct.</span></span> <span data-ttu-id="72eda-256">`Order` オーバーロードを追加してコードを更新し、タグの実行順序を制御します。</span><span class="sxs-lookup"><span data-stu-id="72eda-256">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="72eda-257">`Order` プロパティを、同じ要素をターゲットとする他のタグ ヘルパーと比べることで実行順序が決まります。</span><span class="sxs-lookup"><span data-stu-id="72eda-257">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="72eda-258">既定の順序の値は 0 で、より低い値を持つインスタンスの方が先に実行されます。</span><span class="sxs-lookup"><span data-stu-id="72eda-258">The default order value is zero and instances with lower values are executed first.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    <span data-ttu-id="72eda-259">上記のコードにより、WWW タグ ヘルパーの前に、HTTP タグ ヘルパーが実行されることが保証されます。</span><span class="sxs-lookup"><span data-stu-id="72eda-259">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="72eda-260">`Order` を `MaxValue` に変更し、WWW タグの生成されたマークアップが正しいことを確認します。</span><span class="sxs-lookup"><span data-stu-id="72eda-260">Change `Order` to `MaxValue` and verify that the markup generated for the WWW tag is incorrect.</span></span>

## <a name="inspect-and-retrieve-child-content"></a><span data-ttu-id="72eda-261">子コンテンツを検査して取得する</span><span class="sxs-lookup"><span data-stu-id="72eda-261">Inspect and retrieve child content</span></span>

<span data-ttu-id="72eda-262">タグ ヘルパーには、コンテンツを取得するための複数のプロパティが用意されています。</span><span class="sxs-lookup"><span data-stu-id="72eda-262">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="72eda-263">`GetChildContentAsync` の結果を `output.Content` に付加できます。</span><span class="sxs-lookup"><span data-stu-id="72eda-263">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="72eda-264">`GetContent` を使用して `GetChildContentAsync` の結果を検査できます。</span><span class="sxs-lookup"><span data-stu-id="72eda-264">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="72eda-265">`output.Content` を変更すると、この自動リンカー サンプルのように、`GetChildContentAsync` を呼び出すまで、TagHelper ボディは実行またはレンダリングされません。</span><span class="sxs-lookup"><span data-stu-id="72eda-265">If you modify `output.Content`, the TagHelper body won't be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  <span data-ttu-id="72eda-266">`GetChildContentAsync` を複数回呼び出すと、同じ値が返され、キャッシュされた結果を使用しないことを示す false パラメーターを渡さない限り、`TagHelper` ボディは再実行されません。</span><span class="sxs-lookup"><span data-stu-id="72eda-266">Multiple calls to `GetChildContentAsync` returns the same value and doesn't re-execute the `TagHelper` body unless you pass in a false parameter indicating not to use the cached result.</span></span>
