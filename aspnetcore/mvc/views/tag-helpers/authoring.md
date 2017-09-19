---
title: "ASP.NET Core のタグ ヘルパーの作成"
author: rick-anderson
description: "ASP.NET Core のタグ ヘルパーの作成方法を説明します。"
keywords: "ASP.NET Core、タグ ヘルパー"
ms.author: riande
manager: wpickett
ms.date: 06/14/2017
ms.topic: article
ms.assetid: 4f16d978-5695-4abf-a785-fdaabf3bbcb9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1a5222da1380c2fe768b287bfa1a49b300c02f2b
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/19/2017
---
# <a name="authoring-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a><span data-ttu-id="0967f-104">ASP.NET Core、サンプルとチュートリアルのタグ ヘルパーの作成</span><span class="sxs-lookup"><span data-stu-id="0967f-104">Authoring Tag Helpers in ASP.NET Core, a walkthrough with samples</span></span>

<span data-ttu-id="0967f-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0967f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="0967f-106">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="0967f-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample)

## <a name="getting-started-with-tag-helpers"></a><span data-ttu-id="0967f-107">タグ ヘルパーの使用を開始します。</span><span class="sxs-lookup"><span data-stu-id="0967f-107">Getting started with Tag Helpers</span></span>

<span data-ttu-id="0967f-108">このチュートリアルでは、タグ ヘルパーのプログラミングの概要を提供します。</span><span class="sxs-lookup"><span data-stu-id="0967f-108">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="0967f-109">[タグ ヘルパーの概要](intro.md)タグ ヘルパーを提供する利点について説明します。</span><span class="sxs-lookup"><span data-stu-id="0967f-109">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="0967f-110">タグ ヘルパーを実装するクラス、`ITagHelper`インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="0967f-110">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="0967f-111">ただし、タグ ヘルパーを作成する場合は、一般にから派生した`TagHelper`、これにアクセスするにより、`Process`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0967f-111">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="0967f-112">いう新しい ASP.NET Core プロジェクトを作成する**AuthoringTagHelpers**です。</span><span class="sxs-lookup"><span data-stu-id="0967f-112">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="0967f-113">認証は、このプロジェクトの必要はありません。</span><span class="sxs-lookup"><span data-stu-id="0967f-113">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="0967f-114">呼ばれるタグ ヘルパーを保持するフォルダーを作成する*TagHelpers*です。</span><span class="sxs-lookup"><span data-stu-id="0967f-114">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="0967f-115">*TagHelpers*フォルダーは*いない*必要に応じてが、妥当な規則です。</span><span class="sxs-lookup"><span data-stu-id="0967f-115">The *TagHelpers* folder is *not* required, but it is a reasonable convention.</span></span> <span data-ttu-id="0967f-116">今すぐ始めましょういくつかの単純なタグ ヘルパーを記述します。</span><span class="sxs-lookup"><span data-stu-id="0967f-116">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="0967f-117">最小タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="0967f-117">A minimal Tag Helper</span></span>

<span data-ttu-id="0967f-118">このセクションでは、電子メール タグを更新するタグ ヘルパーを記述します。</span><span class="sxs-lookup"><span data-stu-id="0967f-118">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="0967f-119">例:</span><span class="sxs-lookup"><span data-stu-id="0967f-119">For example:</span></span>

```html
<email>Support</email>
   ```

<span data-ttu-id="0967f-120">サーバーは次のように、そのマークアップを変換するのに、電子メール タグ ヘルパーを使用します。</span><span class="sxs-lookup"><span data-stu-id="0967f-120">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
   ```

<span data-ttu-id="0967f-121">つまり、アンカー タグにより、この電子メールのリンク。</span><span class="sxs-lookup"><span data-stu-id="0967f-121">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="0967f-122">これを行う場合は、ブログ エンジンを作成して、同じドメインにすべてマーケティング、サポート、およびその他の連絡先の電子メールを送信する際に必要する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0967f-122">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1.  <span data-ttu-id="0967f-123">次の追加`EmailTagHelper`クラスを*TagHelpers*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="0967f-123">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    <span data-ttu-id="0967f-124">**注:**</span><span class="sxs-lookup"><span data-stu-id="0967f-124">**Notes:**</span></span>
    
    * <span data-ttu-id="0967f-125">タグ ヘルパーの使用をルート クラスの名前を持つ要素を対象とする名前付け規則 (マイナス、 *TagHelper*クラス名の部分)。</span><span class="sxs-lookup"><span data-stu-id="0967f-125">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="0967f-126">この例では、ルート名で**電子メール**TagHelper は*電子メール*ので、`<email>`タグの対象となります。</span><span class="sxs-lookup"><span data-stu-id="0967f-126">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="0967f-127">後でこのメソッドをオーバーライドする方法を示しますが、この名前付け規則がほとんどタグ ヘルパーの動作する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0967f-127">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
    * <span data-ttu-id="0967f-128">`EmailTagHelper` クラスは `TagHelper` から派生したものです。</span><span class="sxs-lookup"><span data-stu-id="0967f-128">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="0967f-129">`TagHelper`クラス タグ ヘルパーを記述するためのメソッドとプロパティを提供します。</span><span class="sxs-lookup"><span data-stu-id="0967f-129">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
    * <span data-ttu-id="0967f-130">オーバーライドされた`Process`タグ ヘルパーが実行時にメソッドを制御します。</span><span class="sxs-lookup"><span data-stu-id="0967f-130">The  overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="0967f-131">`TagHelper`クラスには、非同期バージョンも用意されています (`ProcessAsync`) 同じパラメーターを使用します。</span><span class="sxs-lookup"><span data-stu-id="0967f-131">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
    * <span data-ttu-id="0967f-132">コンテキスト パラメーターを`Process`(および`ProcessAsync`) 現在の HTML タグの実行に関連付けられている情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="0967f-132">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
    * <span data-ttu-id="0967f-133">出力パラメーターを`Process`(および`ProcessAsync`)、HTML タグとコンテンツの生成に使用される元のソースの代表的なステートフルな HTML 要素が含まれています。</span><span class="sxs-lookup"><span data-stu-id="0967f-133">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
    * <span data-ttu-id="0967f-134">クラス名のサフィックスを持つ**TagHelper**、これは*いない*、必要なことがベスト プラクティス規則と考えられますが、します。</span><span class="sxs-lookup"><span data-stu-id="0967f-134">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="0967f-135">クラスを宣言します。</span><span class="sxs-lookup"><span data-stu-id="0967f-135">You could declare the class as:</span></span>
    
    ```csharp
    public class Email : TagHelper
    ```

2.  <span data-ttu-id="0967f-136">させる、 `EmailTagHelper` 、すべての Razor ビューに使用可能なクラスを追加、`addTagHelper`ディレクティブを*Views/_ViewImports.cshtml*ファイル。[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="0967f-136">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
    <span data-ttu-id="0967f-137">上記のコードは、使用可能なアセンブリ内のすべてのタグ ヘルパーを指定するのにワイルドカードの構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="0967f-137">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="0967f-138">後の最初の文字列`@addTagHelper`を読み込むタグ ヘルパーを指定します (使用する"*"すべてのタグ ヘルパーの)、2 番目の文字列"AuthoringTagHelpers"アセンブリを指定するのには、タグ ヘルパーとします。</span><span class="sxs-lookup"><span data-stu-id="0967f-138">The first string after `@addTagHelper` specifies the tag helper to load (Use "*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="0967f-139">また、ワイルドカードの構文を使用して ASP.NET Core MVC タグ ヘルパーの 2 番目の行がもたらすことに注意してください (これらのヘルパーは、後ほど[タグ ヘルパーの概要](intro.md))。`@addTagHelper`タグ ヘルパーの Razor ビューを使用できるようにするディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="0967f-139">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="0967f-140">また、次に示すように、タグ ヘルパーの完全修飾名 (FQN) を指定できます。</span><span class="sxs-lookup"><span data-stu-id="0967f-140">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
    
    <span data-ttu-id="0967f-141">タグ ヘルパーに追加する、FQN を使用するビューを最初に追加する、FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`)、およびアセンブリ名では、(*AuthoringTagHelpers*)。</span><span class="sxs-lookup"><span data-stu-id="0967f-141">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="0967f-142">ほとんどの開発者は、ワイルドカードの構文を使用するを選びます。</span><span class="sxs-lookup"><span data-stu-id="0967f-142">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="0967f-143">[タグ ヘルパーの概要](intro.md)タグ ヘルパーの追加、削除、階層、およびワイルドカードの構文の詳細を見ていきます。</span><span class="sxs-lookup"><span data-stu-id="0967f-143">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3.  <span data-ttu-id="0967f-144">内のマークアップを更新、 *Views/Home/Contact.cshtml*のこれらの変更されたファイル。</span><span class="sxs-lookup"><span data-stu-id="0967f-144">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  <span data-ttu-id="0967f-145">アプリを実行して、アンカー マークアップを含む電子メール タグが置換されることを確認できるように HTML ソースの表示をお気に入りのブラウザーを使用して (たとえば、 `<a>Support</a>`)。</span><span class="sxs-lookup"><span data-stu-id="0967f-145">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="0967f-146">*サポート*と*マーケティング*は、リンクとして表示はありません、`href`ように機能する属性。</span><span class="sxs-lookup"><span data-stu-id="0967f-146">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="0967f-147">解決する次のセクションでします。</span><span class="sxs-lookup"><span data-stu-id="0967f-147">We'll fix that in the next section.</span></span>

<span data-ttu-id="0967f-148">注: HTML タグと属性、タグ、クラス名および属性で Razor、および c# のように小文字は区別されません。</span><span class="sxs-lookup"><span data-stu-id="0967f-148">Note: Like HTML tags and attributes, tags, class names and attributes in Razor, and C# are not case-sensitive.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="0967f-149">SetAttribute と SetContent</span><span class="sxs-lookup"><span data-stu-id="0967f-149">SetAttribute and SetContent</span></span>

<span data-ttu-id="0967f-150">このセクションで更新されます、`EmailTagHelper`電子メール用の有効なアンカー タグが作成できるようにします。</span><span class="sxs-lookup"><span data-stu-id="0967f-150">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="0967f-151">Razor ビューから情報に更新されます (の形式で、`mail-to`属性) と、アンカーの生成に使用します。</span><span class="sxs-lookup"><span data-stu-id="0967f-151">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="0967f-152">更新プログラム、`EmailTagHelper`を次のクラス。</span><span class="sxs-lookup"><span data-stu-id="0967f-152">Update the `EmailTagHelper` class with the following:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

<span data-ttu-id="0967f-153">**注:**</span><span class="sxs-lookup"><span data-stu-id="0967f-153">**Notes:**</span></span>

* <span data-ttu-id="0967f-154">タグ ヘルパーのクラスおよびプロパティを pascal で名前に変換されます、 [kebab ケースを下げる](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)です。</span><span class="sxs-lookup"><span data-stu-id="0967f-154">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="0967f-155">そのため、使用する、`MailTo`属性を使用する`<email mail-to="value"/>`と同等です。</span><span class="sxs-lookup"><span data-stu-id="0967f-155">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="0967f-156">最後の行は、最低限の機能のタグ ヘルパーの完了したコンテンツを設定します。</span><span class="sxs-lookup"><span data-stu-id="0967f-156">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="0967f-157">強調表示された行は、属性を追加するための構文を示しています。</span><span class="sxs-lookup"><span data-stu-id="0967f-157">The highlighted line shows the syntax for adding attributes:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

<span data-ttu-id="0967f-158">属性コレクションに現在存在しない限り、その方法は、属性"href"できます。</span><span class="sxs-lookup"><span data-stu-id="0967f-158">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="0967f-159">使用することも、`output.Attributes.Add`タグ ヘルパー属性をタグの属性のコレクションの末尾に追加します。</span><span class="sxs-lookup"><span data-stu-id="0967f-159">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1.  <span data-ttu-id="0967f-160">内のマークアップを更新、 *Views/Home/Contact.cshtml*のこれらの変更されたファイル。[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="0967f-160">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2.  <span data-ttu-id="0967f-161">アプリケーションを実行し、適切なリンクが生成されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="0967f-161">Run the app and verify that it generates the correct links.</span></span>
    
    > [!NOTE]
    ><span data-ttu-id="0967f-162">電子メール タグ自己終了を記述する場合は (`<email mail-to="Rick" />`)、最終的な出力は自己終了になります。</span><span class="sxs-lookup"><span data-stu-id="0967f-162">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="0967f-163">開始タグのみを持つタグを記述する機能を有効にする (`<email mail-to="Rick">`) に次のクラスを装飾する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0967f-163">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    <span data-ttu-id="0967f-164">自己終了電子メール タグ ヘルパーでは、出力ようになります`<a href="mailto:Rick@contoso.com" />`です。</span><span class="sxs-lookup"><span data-stu-id="0967f-164">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="0967f-165">自己終了アンカー タグは、有効な HTML を作成するのにはしたいはありませんが、自己終了タグのヘルパーを作成する場合があります。</span><span class="sxs-lookup"><span data-stu-id="0967f-165">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that is self-closing.</span></span> <span data-ttu-id="0967f-166">タグ ヘルパーの種類の設定、`TagMode`タグを読み取った後のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="0967f-166">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="0967f-167">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="0967f-167">ProcessAsync</span></span>

<span data-ttu-id="0967f-168">このセクションでは非同期電子メール ヘルパーを記述します。</span><span class="sxs-lookup"><span data-stu-id="0967f-168">In this section, we'll write an asynchronous email helper.</span></span>

1.  <span data-ttu-id="0967f-169">置換、`EmailTagHelper`クラスを次のコードで。</span><span class="sxs-lookup"><span data-stu-id="0967f-169">Replace the `EmailTagHelper` class with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    <span data-ttu-id="0967f-170">**注:**</span><span class="sxs-lookup"><span data-stu-id="0967f-170">**Notes:**</span></span>

    * <span data-ttu-id="0967f-171">このバージョンでは、非同期`ProcessAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0967f-171">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="0967f-172">非同期の`GetChildContentAsync`を返します、`Task`を含む、`TagHelperContent`です。</span><span class="sxs-lookup"><span data-stu-id="0967f-172">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

    * <span data-ttu-id="0967f-173">使用して、`output`パラメーターを HTML 要素の内容を取得します。</span><span class="sxs-lookup"><span data-stu-id="0967f-173">Use the `output` parameter to get contents of the HTML element.</span></span>

2.  <span data-ttu-id="0967f-174">次の変更を行う、 *Views/Home/Contact.cshtml*ファイルのタグ ヘルパーは、ターゲット電子メールを取得できるようにします。</span><span class="sxs-lookup"><span data-stu-id="0967f-174">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  <span data-ttu-id="0967f-175">アプリを実行して、有効な電子メールのリンクが生成されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="0967f-175">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="0967f-176">RemoveAll、PreContent.SetHtmlContent および PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="0967f-176">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1.  <span data-ttu-id="0967f-177">次の追加`BoldTagHelper`クラスを*TagHelpers*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="0967f-177">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    <span data-ttu-id="0967f-178">**注:**</span><span class="sxs-lookup"><span data-stu-id="0967f-178">**Notes:**</span></span>
    
    * <span data-ttu-id="0967f-179">`[HtmlTargetElement]`属性のパスを示す HTML 属性を含む任意の HTML 要素が"bold"という名前の属性パラメーターは一致と`Process`クラスでオーバーライド メソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="0967f-179">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="0967f-180">この例で、`Process`メソッドは、"bold"属性を削除しとを含むマークアップを囲む`<strong></strong>`です。</span><span class="sxs-lookup"><span data-stu-id="0967f-180">In our sample, the `Process`  method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
    * <span data-ttu-id="0967f-181">既存のタグはコンテンツを交換しないため、開始を書き込む必要があります`<strong>`とタグ付け、`PreContent.SetHtmlContent`メソッドと終了`</strong>`とタグ付け、`PostContent.SetHtmlContent`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0967f-181">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2.  <span data-ttu-id="0967f-182">変更、 *About.cshtml*を含むビュー、`bold`属性の値。</span><span class="sxs-lookup"><span data-stu-id="0967f-182">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="0967f-183">完成したコードは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="0967f-183">The completed code is shown below.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  <span data-ttu-id="0967f-184">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="0967f-184">Run the app.</span></span> <span data-ttu-id="0967f-185">好みのブラウザーを使用するには、ソースを検査して、マークアップを確認してください。</span><span class="sxs-lookup"><span data-stu-id="0967f-185">You can use your favorite browser to inspect the source and verify the markup.</span></span>

    <span data-ttu-id="0967f-186">`[HtmlTargetElement]`上の属性のみが対象"bold"の属性名を提供する HTML マークアップ。</span><span class="sxs-lookup"><span data-stu-id="0967f-186">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="0967f-187">`<bold>`タグ ヘルパーによって要素が変更されていません。</span><span class="sxs-lookup"><span data-stu-id="0967f-187">The `<bold>` element was not modified by the tag helper.</span></span>

4. <span data-ttu-id="0967f-188">コメント アウト、`[HtmlTargetElement]`属性行とそれは、対象とする既定`<bold>`タグ、フォームの HTML マークアップは、`<bold>`です。</span><span class="sxs-lookup"><span data-stu-id="0967f-188">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="0967f-189">ただし、既定の名前付け規則は、クラス名に一致**太字**に TagHelper`<bold>`タグ。</span><span class="sxs-lookup"><span data-stu-id="0967f-189">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="0967f-190">アプリを実行していることを確認、`<bold>`タグがタグ ヘルパーによって処理されます。</span><span class="sxs-lookup"><span data-stu-id="0967f-190">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="0967f-191">複数のクラスを装飾`[HtmlTargetElement]`ターゲットの論理 OR で結果の属性です。</span><span class="sxs-lookup"><span data-stu-id="0967f-191">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="0967f-192">たとえば、次のコードを使用するには、太字のタグまたは太字属性は一致します。</span><span class="sxs-lookup"><span data-stu-id="0967f-192">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

<span data-ttu-id="0967f-193">同じステートメントに複数の属性を追加するときに、ランタイムとして扱います論理 and。</span><span class="sxs-lookup"><span data-stu-id="0967f-193">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="0967f-194">たとえば、次のコードで HTML 要素必要がありますという名前が"bold"という名前の属性を"bold"(`<bold bold />`) 一致するようにします。</span><span class="sxs-lookup"><span data-stu-id="0967f-194">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="0967f-195">使用することも、`[HtmlTargetElement]`を対象となる要素の名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="0967f-195">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="0967f-196">場合の例については、`BoldTagHelper`ターゲットに`<MyBold>`タグ、次の属性を使用すると。</span><span class="sxs-lookup"><span data-stu-id="0967f-196">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="passing-a-model-to-a-tag-helper"></a><span data-ttu-id="0967f-197">タグ ヘルパーにモデルを渡す</span><span class="sxs-lookup"><span data-stu-id="0967f-197">Passing a model to a Tag Helper</span></span>

1.  <span data-ttu-id="0967f-198">追加、*モデル*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="0967f-198">Add a *Models* folder.</span></span>

2.  <span data-ttu-id="0967f-199">次の `WebsiteContext` クラスを *Models* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="0967f-199">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  <span data-ttu-id="0967f-200">次の追加`WebsiteInformationTagHelper`クラスを*TagHelpers*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="0967f-200">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    <span data-ttu-id="0967f-201">**注:**</span><span class="sxs-lookup"><span data-stu-id="0967f-201">**Notes:**</span></span>
    
    * <span data-ttu-id="0967f-202">前述のように、タグ ヘルパーにより、c# を pascal クラス名とプロパティのでにタグ ヘルパー [kebab ケースを下げる](http://wiki.c2.com/?KebabCase)です。</span><span class="sxs-lookup"><span data-stu-id="0967f-202">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="0967f-203">そのため、使用する、 `WebsiteInformationTagHelper` Razor での記述`<website-information />`です。</span><span class="sxs-lookup"><span data-stu-id="0967f-203">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
    * <span data-ttu-id="0967f-204">ターゲット要素に明示的に識別しない、`[HtmlTargetElement]`属性、そのため、既定の`website-information`の対象となります。</span><span class="sxs-lookup"><span data-stu-id="0967f-204">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="0967f-205">場合は、次の属性 (kebab ケースではありませんが、クラス名と一致することに注意) が適用されます。</span><span class="sxs-lookup"><span data-stu-id="0967f-205">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    <span data-ttu-id="0967f-206">低い kebab ケース タグ`<website-information />`は一致しません。</span><span class="sxs-lookup"><span data-stu-id="0967f-206">The lower kebab case tag `<website-information />` would not match.</span></span> <span data-ttu-id="0967f-207">使用する場合、`[HtmlTargetElement]`属性、kebab ケース次に示すようを使用するとします。</span><span class="sxs-lookup"><span data-stu-id="0967f-207">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * <span data-ttu-id="0967f-208">自己終了要素のコンテンツがあるありません。</span><span class="sxs-lookup"><span data-stu-id="0967f-208">Elements that are self-closing have no content.</span></span> <span data-ttu-id="0967f-209">この例では、Razor マークアップでは、自己終了タグですがタグ ヘルパーの作成は、[セクション](http://www.w3.org/TR/html5/sections.html#the-section-element)要素 (自己終了ではないと、対応するには、内のコンテンツを記述する、`section`要素)。</span><span class="sxs-lookup"><span data-stu-id="0967f-209">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which is not self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="0967f-210">そのため、設定する必要があります`TagMode`に`StartTagAndEndTag`出力に書き込みます。</span><span class="sxs-lookup"><span data-stu-id="0967f-210">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="0967f-211">行の設定をコメントする代わりに、`TagMode`と終了タグのマークアップを記述します。</span><span class="sxs-lookup"><span data-stu-id="0967f-211">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="0967f-212">(例マークアップがこのチュートリアルの後半で提供されています。)</span><span class="sxs-lookup"><span data-stu-id="0967f-212">(Example markup is provided later in this tutorial.)</span></span>
    
    * <span data-ttu-id="0967f-213">`$` (ドル記号) で、次の行を使用して、[補間文字列](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span><span class="sxs-lookup"><span data-stu-id="0967f-213">The `$` (dollar sign) in the following line uses an [interpolated string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  <span data-ttu-id="0967f-214">次のマークアップを追加、 *About.cshtml*ビュー。</span><span class="sxs-lookup"><span data-stu-id="0967f-214">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="0967f-215">強調表示されたマークアップでは、web サイトの情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="0967f-215">The highlighted markup displays the web site information.</span></span>
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > <span data-ttu-id="0967f-216">次に示す Razor マークアップ。</span><span class="sxs-lookup"><span data-stu-id="0967f-216">In the Razor markup shown below:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    ><span data-ttu-id="0967f-217">Razor、`info`属性は、文字列ではなく、クラスと c# コードを記述します。</span><span class="sxs-lookup"><span data-stu-id="0967f-217">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="0967f-218">しなくても、任意の文字列ではないタグ ヘルパー属性を記述する必要があります、`@`文字です。</span><span class="sxs-lookup"><span data-stu-id="0967f-218">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5.  <span data-ttu-id="0967f-219">アプリを実行し、バージョン情報を表示する web サイトの情報に移動します。</span><span class="sxs-lookup"><span data-stu-id="0967f-219">Run the app, and navigate to the About view to see the web site information.</span></span>

    >[!NOTE]
    ><span data-ttu-id="0967f-220">終了タグで、次のマークアップを使用してでは、行を削除する`TagMode.StartTagAndEndTag`タグ ヘルパーで。</span><span class="sxs-lookup"><span data-stu-id="0967f-220">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a><span data-ttu-id="0967f-221">タグ ヘルパーを条件します。</span><span class="sxs-lookup"><span data-stu-id="0967f-221">Condition Tag Helper</span></span>

<span data-ttu-id="0967f-222">条件タグ ヘルパーは、true 値を渡されるときに出力を表示します。</span><span class="sxs-lookup"><span data-stu-id="0967f-222">The condition tag helper renders output when passed a true value.</span></span>

1.  <span data-ttu-id="0967f-223">次の追加`ConditionTagHelper`クラスを*TagHelpers*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="0967f-223">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  <span data-ttu-id="0967f-224">内容を置き換える、 *Views/Home/Index.cshtml*次のマークアップ ファイル。</span><span class="sxs-lookup"><span data-stu-id="0967f-224">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

    <!-- literal_block {"xml:space": "preserve", "source": "mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml", "ids": [], "linenos": false, "highlight_args": {"linenostart": 1}} -->
    
    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=Model />
        <div condition="Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  <span data-ttu-id="0967f-225">置換、`Index`メソッドで、`Home`コント ローラーを次のコード。</span><span class="sxs-lookup"><span data-stu-id="0967f-225">Replace the `Index` method in the `Home` controller with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  <span data-ttu-id="0967f-226">アプリを実行して、ホーム ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="0967f-226">Run the app and browse to the home page.</span></span> <span data-ttu-id="0967f-227">条件付きでマークアップ`div`はレンダリングされません。</span><span class="sxs-lookup"><span data-stu-id="0967f-227">The markup in the conditional `div` will not be rendered.</span></span> <span data-ttu-id="0967f-228">クエリ文字列の追加`?approved=true`URL に (たとえば、 `http://localhost:1235/Home/Index?approved=true`)。</span><span class="sxs-lookup"><span data-stu-id="0967f-228">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="0967f-229">`approved`true に設定され、条件付きマークアップが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0967f-229">`approved` is set to true and the conditional markup will be displayed.</span></span>

>[!NOTE]
><span data-ttu-id="0967f-230">使用して、 [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof)演算子を太字タグ ヘルパーで行ったように文字列を指定するのではなく、対象の属性を指定します。</span><span class="sxs-lookup"><span data-stu-id="0967f-230">Use the [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
><span data-ttu-id="0967f-231">[Nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof)演算子は、コードを保護する必要があります、これまでリファクタリングを行うこと (名前を変更する可能性があります`RedCondition`)。</span><span class="sxs-lookup"><span data-stu-id="0967f-231">The [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoiding-tag-helper-conflicts"></a><span data-ttu-id="0967f-232">タグ ヘルパーの競合を避ける</span><span class="sxs-lookup"><span data-stu-id="0967f-232">Avoiding Tag Helper conflicts</span></span>

<span data-ttu-id="0967f-233">このセクションでは、自動リンク タグ ヘルパーのペアを作成します。</span><span class="sxs-lookup"><span data-stu-id="0967f-233">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="0967f-234">最初は、マークアップを含む、HTML アンカー タグ同じ URL を含む (および、URL へのリンクを生成するため) に HTTP で始まる URL で置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="0967f-234">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="0967f-235">2 つ目は同じ操作を行いますは URL の www を開始します。</span><span class="sxs-lookup"><span data-stu-id="0967f-235">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="0967f-236">これら 2 つのヘルパーが密接に関連し、それらを今後リファクターする場合があります、ためにではこのままに同じファイルにします。</span><span class="sxs-lookup"><span data-stu-id="0967f-236">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1.  <span data-ttu-id="0967f-237">次の追加`AutoLinkerHttpTagHelper`クラスを*TagHelpers*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="0967f-237">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    ><span data-ttu-id="0967f-238">`AutoLinkerHttpTagHelper`クラス ターゲット`p`要素および使用[Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)アンカーを作成します。</span><span class="sxs-lookup"><span data-stu-id="0967f-238">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

2.  <span data-ttu-id="0967f-239">末尾に次のマークアップを追加、 *Views/Home/Contact.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0967f-239">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  <span data-ttu-id="0967f-240">アプリケーションを実行し、タグ ヘルパー アンカーが正しく表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="0967f-240">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4.  <span data-ttu-id="0967f-241">更新プログラム、`AutoLinker`クラスに含める、 `AutoLinkerWwwTagHelper` www テキスト www の元のテキストが含まれているアンカー タグに変換します。</span><span class="sxs-lookup"><span data-stu-id="0967f-241">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="0967f-242">更新されたコードは、以下が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="0967f-242">The updated code is highlighted below:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  <span data-ttu-id="0967f-243">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="0967f-243">Run the app.</span></span> <span data-ttu-id="0967f-244">通知 www テキストがリンクとしてレンダリングされますが、HTTP テキストではありません。</span><span class="sxs-lookup"><span data-stu-id="0967f-244">Notice the www text is rendered as a link but the HTTP text is not.</span></span> <span data-ttu-id="0967f-245">両方のクラスでブレークポイントを配置した場合は、HTTP タグ ヘルパー クラスが最初に実行されることが確認できます。</span><span class="sxs-lookup"><span data-stu-id="0967f-245">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="0967f-246">問題には、タグ ヘルパーの出力がキャッシュされると、HTTP タグ ヘルパーからキャッシュされた出力 WWW タグ ヘルパーの実行時に上書きされます。</span><span class="sxs-lookup"><span data-stu-id="0967f-246">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="0967f-247">このチュートリアルで後ほどおでタグ ヘルパーが実行される順序を制御する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0967f-247">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="0967f-248">コードは、次のように修正されます。</span><span class="sxs-lookup"><span data-stu-id="0967f-248">We'll fix the code with the following:</span></span>

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    ><span data-ttu-id="0967f-249">Edition では、まず自動リンク タグ ヘルパーの次のコードで、ターゲットのコンテンツを取得しました。</span><span class="sxs-lookup"><span data-stu-id="0967f-249">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    ><span data-ttu-id="0967f-250">つまり、呼び出す`GetChildContentAsync`を使用して、`TagHelperOutput`に渡される、`ProcessAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0967f-250">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="0967f-251">説明したように以前は、出力がキャッシュされるため、最後にタグ付け wins を実行するためのヘルパー。</span><span class="sxs-lookup"><span data-stu-id="0967f-251">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="0967f-252">次のコードに問題が修正されました。</span><span class="sxs-lookup"><span data-stu-id="0967f-252">You fixed that problem with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    ><span data-ttu-id="0967f-253">上記のコードは、コンテンツが変更されており場合は、出力バッファーからコンテンツを取得するかどうかをチェックします。</span><span class="sxs-lookup"><span data-stu-id="0967f-253">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6.  <span data-ttu-id="0967f-254">アプリを実行して、2 つのリンクが期待どおりに動作することを確認します。</span><span class="sxs-lookup"><span data-stu-id="0967f-254">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="0967f-255">この自動リンカー タグ ヘルパーが適切な表示される、中に微妙な問題があります。</span><span class="sxs-lookup"><span data-stu-id="0967f-255">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="0967f-256">WWW タグ ヘルパーが実行される場合、最初 www へのリンクは不正確になります。</span><span class="sxs-lookup"><span data-stu-id="0967f-256">If the WWW tag helper runs first, the www links will not be correct.</span></span> <span data-ttu-id="0967f-257">追加することで、コードを更新、`Order`オーバー ロードで、タグが実行される順序を制御します。</span><span class="sxs-lookup"><span data-stu-id="0967f-257">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="0967f-258">`Order`プロパティが同じ要素を対象とするその他のタグ ヘルパーの基準とした実行順序を決定します。</span><span class="sxs-lookup"><span data-stu-id="0967f-258">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="0967f-259">既定の順序の値は 0 と低い値を持つインスタンスが最初に実行されます。</span><span class="sxs-lookup"><span data-stu-id="0967f-259">The default order value is zero and instances with lower values are executed first.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    <span data-ttu-id="0967f-260">上記のコードは、WWW タグ ヘルパーの前に、HTTP タグ ヘルパーが実行される保証されます。</span><span class="sxs-lookup"><span data-stu-id="0967f-260">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="0967f-261">変更`Order`に`MaxValue`WWW タグの生成されたマークアップが正しいことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="0967f-261">Change `Order` to `MaxValue` and verify that the markup generated for the  WWW tag is incorrect.</span></span>

## <a name="inspecting-and-retrieving-child-content"></a><span data-ttu-id="0967f-262">検査および子コンテンツを取得します。</span><span class="sxs-lookup"><span data-stu-id="0967f-262">Inspecting and retrieving child content</span></span>

<span data-ttu-id="0967f-263">タグ ヘルパーは、コンテンツを取得するいくつかのプロパティを提供します。</span><span class="sxs-lookup"><span data-stu-id="0967f-263">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="0967f-264">結果`GetChildContentAsync`に追加できます`output.Content`です。</span><span class="sxs-lookup"><span data-stu-id="0967f-264">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="0967f-265">結果を調査して`GetChildContentAsync`で`GetContent`です。</span><span class="sxs-lookup"><span data-stu-id="0967f-265">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="0967f-266">変更した場合`output.Content`、TagHelper 本体は実行またはない限り、表示されません`GetChildContentAsync`自動リンカー サンプルのようにします。</span><span class="sxs-lookup"><span data-stu-id="0967f-266">If you modify `output.Content`, the TagHelper body will not be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  <span data-ttu-id="0967f-267">複数回呼び出す`GetChildContentAsync`同じ値を返すし、再実行されません、`TagHelper`本文のキャッシュされた結果を使用しないことを示す場合は false にパラメーターを渡す場合を除き、します。</span><span class="sxs-lookup"><span data-stu-id="0967f-267">Multiple calls to `GetChildContentAsync` will return the same value and will not re-execute the `TagHelper` body unless you pass in a false parameter indicating  not use the cached result.</span></span>
