---
title: "ビューの概要"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 668c320d-c050-45e3-8161-2f460dc93b2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: a93ee8165be52e33c2e7da4d3fee2c8225864db9
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="rendering-html-with-views-in-aspnet-core-mvc"></a><span data-ttu-id="e13db-103">ASP.NET Core MVC でビューを使用して HTML をレンダリング</span><span class="sxs-lookup"><span data-stu-id="e13db-103">Rendering HTML with views in ASP.NET Core MVC</span></span>

<span data-ttu-id="e13db-104">によって[Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="e13db-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="e13db-105">ASP.NET Core MVC コント ローラーを使用して書式設定された結果を返すことができます*ビュー*です。</span><span class="sxs-lookup"><span data-stu-id="e13db-105">ASP.NET Core MVC controllers can return formatted results using *views*.</span></span>

## <a name="what-are-views"></a><span data-ttu-id="e13db-106">ビューとは</span><span class="sxs-lookup"><span data-stu-id="e13db-106">What are Views?</span></span>

<span data-ttu-id="e13db-107">モデル ビュー コント ローラー (MVC) パターンでは、*ビュー*アプリでのユーザーの操作のプレゼンテーションの詳細情報をカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="e13db-107">In the Model-View-Controller (MVC) pattern, the *view* encapsulates the presentation details of the user's interaction with the app.</span></span> <span data-ttu-id="e13db-108">ビューは、埋め込みコードをクライアントに送信するコンテンツを生成する HTML テンプレートです。</span><span class="sxs-lookup"><span data-stu-id="e13db-108">Views are HTML templates with embedded code that generate content to send to the client.</span></span> <span data-ttu-id="e13db-109">ビューを使用して[Razor 構文](razor.md)、これにより、最小限のコードまたは手続きに HTML と対話するコードです。</span><span class="sxs-lookup"><span data-stu-id="e13db-109">Views use [Razor syntax](razor.md), which allows code to interact with HTML with minimal code or ceremony.</span></span>

<span data-ttu-id="e13db-110">ASP.NET Core MVC ビューは*.cshtml*既定で格納されているファイル、*ビュー*アプリケーション内のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="e13db-110">ASP.NET Core MVC views are *.cshtml* files stored by default in a *Views* folder within the application.</span></span> <span data-ttu-id="e13db-111">通常、各コント ローラーは、特定のコント ローラー アクションのビューは、独自のフォルダーがあります。</span><span class="sxs-lookup"><span data-stu-id="e13db-111">Typically, each controller will have its own folder, in which are views for specific controller actions.</span></span>

![ソリューション エクスプ ローラー ビュー フォルダー](overview/_static/views_solution_explorer.png)

<span data-ttu-id="e13db-113">アクションに固有のビューだけでなく[部分ビュー](partial.md)、[レイアウト、およびその他の特殊なビュー ファイル](layout.md)繰り返しが削減され、アプリのビューの中で再利用できるようにするために使用できます。</span><span class="sxs-lookup"><span data-stu-id="e13db-113">In addition to action-specific views, [partial views](partial.md), [layouts, and other special view files](layout.md) can be used to help reduce repetition and allow for reuse within the app's views.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="e13db-114">ビューを使用する利点</span><span class="sxs-lookup"><span data-stu-id="e13db-114">Benefits of Using Views</span></span>

<span data-ttu-id="e13db-115">ビューを提供[関心の分離](http://deviq.com/separation-of-concerns/)MVC アプリでユーザー インターフェイスのビジネス ロジックから個別にレベル マークアップをカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="e13db-115">Views provide [separation of concerns](http://deviq.com/separation-of-concerns/) within an MVC app, encapsulating user interface level markup separately from business logic.</span></span> <span data-ttu-id="e13db-116">ASP.NET MVC ビューを使用して[Razor 構文](razor.md)HTML マークアップとサーバー側ロジック簡単に切り替えることです。</span><span class="sxs-lookup"><span data-stu-id="e13db-116">ASP.NET MVC views use [Razor syntax](razor.md) to make switching between HTML markup and server side logic painless.</span></span> <span data-ttu-id="e13db-117">使用してビューの間でのアプリのユーザー インターフェイスの一般的な反復的な側面を簡単に再利用する[レイアウトと共有ディレクティブ](layout.md)または[部分ビュー](partial.md)です。</span><span class="sxs-lookup"><span data-stu-id="e13db-117">Common, repetitive aspects of the app's user interface can easily be reused between views using [layout and shared directives](layout.md) or [partial views](partial.md).</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="e13db-118">ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="e13db-118">Creating a View</span></span>

<span data-ttu-id="e13db-119">コント ローラーに固有のビューを作成するのには*ビュー/[ControllerName]*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="e13db-119">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="e13db-120">コント ローラー間で共有されるビューについてに、 */ビュー/共有*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="e13db-120">Views that are shared among controllers are placed in the */Views/Shared* folder.</span></span> <span data-ttu-id="e13db-121">ファイルの表示を関連付けられたコント ローラー アクションの場合と同じ名前し、追加、 *.cshtml*ファイル拡張子。</span><span class="sxs-lookup"><span data-stu-id="e13db-121">Name the view file the same as its associated controller action, and add the *.cshtml* file extension.</span></span> <span data-ttu-id="e13db-122">例については、ビューを作成する、*に関する*アクションを*ホーム*コント ローラーを作成、 *About.cshtml*ファイルで、   */ビュー/ホーム*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="e13db-122">For example, to create a view for the *About* action on the *Home* controller, you would create the *About.cshtml* file in the */Views/Home* folder.</span></span>

<span data-ttu-id="e13db-123">サンプル ファイルの表示 (*About.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="e13db-123">A sample view file (*About.cshtml*):</span></span>

<span data-ttu-id="e13db-124">[!code-html[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="e13db-124">[!code-html[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]</span></span>

<span data-ttu-id="e13db-125">*Razor*でコードが示される、`@`シンボル。</span><span class="sxs-lookup"><span data-stu-id="e13db-125">*Razor* code is denoted by the `@` symbol.</span></span> <span data-ttu-id="e13db-126">(C#) ステートメントが Razor コード ブロックは中かっこで off に設定内で実行されます (`{` `}`)、"About"の割り当てなどに、`ViewData["Title"]`上に示す要素。</span><span class="sxs-lookup"><span data-stu-id="e13db-126">C# statements are run within Razor code blocks set off by curly braces (`{` `}`), such as the assignment of "About" to the `ViewData["Title"]` element shown above.</span></span> <span data-ttu-id="e13db-127">Razor を使用してで値を単に参照によって HTML 内の値を表示すること、 `@` symbol, 内で示すように、`<h2>`と`<h3>`要素上です。</span><span class="sxs-lookup"><span data-stu-id="e13db-127">Razor can be used to display values within HTML by simply referencing the value with the `@` symbol, as shown within the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="e13db-128">このビューは、担当するは、出力の部分だけについて説明します。</span><span class="sxs-lookup"><span data-stu-id="e13db-128">This view focuses on just the portion of the output for which it is responsible.</span></span> <span data-ttu-id="e13db-129">ページのレイアウトの残りの部分と、ビューの他の一般的な側面は、他の場所で指定されます。</span><span class="sxs-lookup"><span data-stu-id="e13db-129">The rest of the page's layout, and other common aspects of the view, are specified elsewhere.</span></span> <span data-ttu-id="e13db-130">詳細については[レイアウトと共有のビュー ロジック](layout.md)です。</span><span class="sxs-lookup"><span data-stu-id="e13db-130">Learn more about [layout and shared view logic](layout.md).</span></span>

## <a name="how-do-controllers-specify-views"></a><span data-ttu-id="e13db-131">コント ローラーを指定してビューを操作する方法</span><span class="sxs-lookup"><span data-stu-id="e13db-131">How do Controllers Specify Views?</span></span>

<span data-ttu-id="e13db-132">ビューは通常の操作から返される、`ViewResult`です。</span><span class="sxs-lookup"><span data-stu-id="e13db-132">Views are typically returned from actions as a `ViewResult`.</span></span> <span data-ttu-id="e13db-133">アクション メソッドが作成して返すことができます、`ViewResult`を直接がより一般的なコント ローラーから継承している場合`Controller`、単に使用する、`View`ヘルパー メソッドを次の例として示します。</span><span class="sxs-lookup"><span data-stu-id="e13db-133">Your action method can create and return a `ViewResult` directly, but more commonly if your controller inherits from `Controller`, you'll simply use the `View` helper method, as this example demonstrates:</span></span>

<span data-ttu-id="e13db-134">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="e13db-134">*HomeController.cs*</span></span>

<span data-ttu-id="e13db-135">[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]</span><span class="sxs-lookup"><span data-stu-id="e13db-135">[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]</span></span>

<span data-ttu-id="e13db-136">`View`ヘルパー メソッドが返すビュー容易にするためアプリ開発者にとっていくつかのオーバー ロードします。</span><span class="sxs-lookup"><span data-stu-id="e13db-136">The `View` helper method has several overloads to make returning views easier for app developers.</span></span> <span data-ttu-id="e13db-137">ビューに渡すモデル オブジェクトと同様に戻るには、ビューを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="e13db-137">You can optionally specify a view to return, as well as a model object to pass to the view.</span></span>

<span data-ttu-id="e13db-138">この操作から制御が戻るとき、 *About.cshtml*上に表示されるビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e13db-138">When this action returns, the *About.cshtml* view shown above is rendered:</span></span>

![ページについて](overview/_static/about-page.png)

### <a name="view-discovery"></a><span data-ttu-id="e13db-140">ビューの検出</span><span class="sxs-lookup"><span data-stu-id="e13db-140">View Discovery</span></span>

<span data-ttu-id="e13db-141">アクションは、ビューを返します、ときに、プロセスを呼び出す*ビュー検出*行われます。</span><span class="sxs-lookup"><span data-stu-id="e13db-141">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="e13db-142">このプロセスでは、どのビュー ファイルを使用してを決定します。</span><span class="sxs-lookup"><span data-stu-id="e13db-142">This process determines which view file will be used.</span></span> <span data-ttu-id="e13db-143">ランタイムはコント ローラーに固有のビューはまず、し、検索に一致するビューの名前の検索、特定のビュー ファイルが指定されていない限り、 *Shared*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="e13db-143">Unless a specific view file is specified, the runtime looks for a controller-specific view first, then looks for matching view name in the *Shared* folder.</span></span>

<span data-ttu-id="e13db-144">アクションが返されるときに、`View`メソッドでは、次のように`return View();`アクション名は、ビュー名として使用します。</span><span class="sxs-lookup"><span data-stu-id="e13db-144">When an action returns the `View` method, like so `return View();`, the action name is used as the view name.</span></span> <span data-ttu-id="e13db-145">たとえば、これは、"Index"という名前のアクション メソッドから呼び出された、なります"Index"の表示名を渡すことに相当します。</span><span class="sxs-lookup"><span data-stu-id="e13db-145">For example, if this were called from an action method named "Index", it would be equivalent to passing in a view name of "Index".</span></span> <span data-ttu-id="e13db-146">メソッドに、ビュー名を明示的に渡すことができます (`return View("SomeView");`)。</span><span class="sxs-lookup"><span data-stu-id="e13db-146">A view name can be explicitly passed to the method (`return View("SomeView");`).</span></span> <span data-ttu-id="e13db-147">このような場合の両方で一致するビューでのファイルの検索を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e13db-147">In both of these cases, view discovery searches for a matching view file in:</span></span>

   1. <span data-ttu-id="e13db-148">ビュー/<ControllerName>/<ViewName>.cshtml</span><span class="sxs-lookup"><span data-stu-id="e13db-148">Views/<ControllerName>/<ViewName>.cshtml</span></span>

   2. <span data-ttu-id="e13db-149">ビュー/共有/<ViewName>.cshtml</span><span class="sxs-lookup"><span data-stu-id="e13db-149">Views/Shared/<ViewName>.cshtml</span></span>

>[!TIP]
> <span data-ttu-id="e13db-150">次の規則だけを返すことをお勧め`View()`可能であればより柔軟で簡単にコードをリファクタリングのためのアクションからです。</span><span class="sxs-lookup"><span data-stu-id="e13db-150">We recommend following the convention of simply returning `View()` from actions when possible, as it results in more flexible, easier to refactor code.</span></span>

<span data-ttu-id="e13db-151">ビュー名の代わりに、ビューのファイル パスを指定できます。</span><span class="sxs-lookup"><span data-stu-id="e13db-151">A view file path can be provided, instead of a view name.</span></span> <span data-ttu-id="e13db-152">ここで、 *.cshtml*拡張機能は、ファイルのパスの一部として指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e13db-152">In this case, the *.cshtml* extension must be specified as part of the file path.</span></span> <span data-ttu-id="e13db-153">アプリケーション ルートに対する相対パスでなければなりません (先頭に使用できます必要に応じて「/」または"~/") です。</span><span class="sxs-lookup"><span data-stu-id="e13db-153">The path should be relative to the application root (and can optionally start with "/" or "~/").</span></span> <span data-ttu-id="e13db-154">例: `return View("Views/Home/About.cshtml");`</span><span class="sxs-lookup"><span data-stu-id="e13db-154">For example: `return View("Views/Home/About.cshtml");`</span></span>

> [!NOTE]
> <span data-ttu-id="e13db-155">[部分ビュー](partial.md)と[コンポーネントを表示](view-components.md)似ています (ただしと一致しない) の検出メカニズムを使用します。</span><span class="sxs-lookup"><span data-stu-id="e13db-155">[Partial views](partial.md) and [view components](view-components.md) use similar (but not identical) discovery mechanisms.</span></span>

> [!NOTE]
> <span data-ttu-id="e13db-156">ビューが配置されているアプリ内でカスタムを使用してに関する既定の規則をカスタマイズする`IViewLocationExpander`です。</span><span class="sxs-lookup"><span data-stu-id="e13db-156">You can customize the default convention regarding where views are located within the app by using a custom `IViewLocationExpander`.</span></span>

>[!TIP]
> <span data-ttu-id="e13db-157">ビューの名前は、基になるファイル システムによって大文字小文字を区別可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e13db-157">View names may be case sensitive depending on the underlying file system.</span></span> <span data-ttu-id="e13db-158">オペレーティング システム間で互換性のため、常に大文字小文字をコント ローラーとアクション名と関連するビューのフォルダーとファイル名の間です。</span><span class="sxs-lookup"><span data-stu-id="e13db-158">For compatibility across operating systems, always match case between controller and action names and associated view folders and filenames.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="e13db-159">ビューにデータを渡す</span><span class="sxs-lookup"><span data-stu-id="e13db-159">Passing Data to Views</span></span>

<span data-ttu-id="e13db-160">いくつかのメカニズムを使用して、ビューにデータを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="e13db-160">You can pass data to views using several mechanisms.</span></span> <span data-ttu-id="e13db-161">最も堅牢な方法は、指定する、*モデル*ビュー内の型 (一般に呼ば、 *viewmodel*、ビジネス ドメイン モデルの種類を区別するために)、ビューにこの型のインスタンスを渡すとアクション。</span><span class="sxs-lookup"><span data-stu-id="e13db-161">The most robust approach is to specify a *model* type in the view (commonly referred to as a *viewmodel*, to distinguish it from business domain model types), and then pass an instance of this type to the view from the action.</span></span> <span data-ttu-id="e13db-162">モデルまたはモデルの表示を使用してデータをビューに渡すことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e13db-162">We recommend you use a model or view model to pass data to a view.</span></span> <span data-ttu-id="e13db-163">これにより、厳密な型チェックを活用するために表示できます。</span><span class="sxs-lookup"><span data-stu-id="e13db-163">This allows the view to take advantage of strong type checking.</span></span> <span data-ttu-id="e13db-164">使用してビューのモデルを指定することができます、`@model`ディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="e13db-164">You can specify a model for a view using the `@model` directive:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1]}} -->

```html
@model WebApplication1.ViewModels.Address
   <h2>Contact</h2>
   <address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

<span data-ttu-id="e13db-165">ビューに送信されるインスタンスを使用して、厳密に型指定された方法でアクセスできるビューのモデルを指定すると、`@Model`上記のようにします。</span><span class="sxs-lookup"><span data-stu-id="e13db-165">Once a model has been specified for a view, the instance sent to the view can be accessed in a strongly-typed manner using `@Model` as shown above.</span></span> <span data-ttu-id="e13db-166">ビューにモデルの種類のインスタンスを提供するには、コント ローラーは、それをパラメーターとして渡します。</span><span class="sxs-lookup"><span data-stu-id="e13db-166">To provide an instance of the model type to the view, the controller passes it as a parameter:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

```csharp
public IActionResult Contact()
   {
       ViewData["Message"] = "Your contact page.";

       var viewModel = new Address()
       {
           Name = "Microsoft",
           Street = "One Microsoft Way",
           City = "Redmond",
           State = "WA",
           PostalCode = "98052-6399"
       };
       return View(viewModel);
   }
   ```

<span data-ttu-id="e13db-167">ビュー モデルとしてを指定できる型に制限はありません。</span><span class="sxs-lookup"><span data-stu-id="e13db-167">There are no restrictions on the types that can be provided to a view as a model.</span></span> <span data-ttu-id="e13db-168">ビジネス ロジックは、アプリで他の場所でカプセル化できますように POCO Plain Old CLR Object () でほとんどまたはまったく動作では、モデルの表示を渡すことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e13db-168">We recommend passing Plain Old CLR Object (POCO) view models with little or no behavior, so that business logic can be encapsulated elsewhere in the app.</span></span> <span data-ttu-id="e13db-169">このアプローチの例は、*アドレス*viewmodel 上記の例で使用します。</span><span class="sxs-lookup"><span data-stu-id="e13db-169">An example of this approach is the *Address* viewmodel used in the example above:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

```csharp
namespace WebApplication1.ViewModels
   {
       public class Address
       {
           public string Name { get; set; }
           public string Street { get; set; }
           public string City { get; set; }
           public string State { get; set; }
           public string PostalCode { get; set; }
       }
   }
   ```

> [!NOTE]
> <span data-ttu-id="e13db-170">同じクラスを使用して、ビジネス モデルの種類と、表示の種類のモデルを何もできません。</span><span class="sxs-lookup"><span data-stu-id="e13db-170">Nothing prevents you from using the same classes as your business model types and your display model types.</span></span> <span data-ttu-id="e13db-171">ただし、分離しておくことに、ドメインまたは永続化モデルと無関係に変更するためにビューは、によってセキュリティ上の利点も提供できます (モデルを使用して、アプリにユーザーを送信する[モデル バインディング](../models/model-binding.md))。</span><span class="sxs-lookup"><span data-stu-id="e13db-171">However, keeping them separate allows your views to vary independently from your domain or persistence model, and can offer some security benefits as well (for models that users will send to the app using [model binding](../models/model-binding.md)).</span></span>

### <a name="loosely-typed-data"></a><span data-ttu-id="e13db-172">弱く型指定されたデータ</span><span class="sxs-lookup"><span data-stu-id="e13db-172">Loosely Typed Data</span></span>

<span data-ttu-id="e13db-173">厳密に型指定されたビューは、以外のすべてのビューにデータの弱い型指定されたコレクションへのアクセスがあります。</span><span class="sxs-lookup"><span data-stu-id="e13db-173">In addition to strongly typed views, all views have access to a loosely typed collection of data.</span></span> <span data-ttu-id="e13db-174">この同じコレクションは、いずれかで参照できる、`ViewData`または`ViewBag`コント ローラーとビューのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="e13db-174">This same collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="e13db-175">`ViewBag`プロパティはラッパー`ViewData`コレクションを動的ビューを提供します。</span><span class="sxs-lookup"><span data-stu-id="e13db-175">The `ViewBag` property is a wrapper around `ViewData` that provides a dynamic view over that collection.</span></span> <span data-ttu-id="e13db-176">別のコレクションではありません。</span><span class="sxs-lookup"><span data-stu-id="e13db-176">It is not a separate collection.</span></span>

<span data-ttu-id="e13db-177">`ViewData`ディクショナリ オブジェクトを通じてアクセス`string`キー。</span><span class="sxs-lookup"><span data-stu-id="e13db-177">`ViewData` is a dictionary object accessed through `string` keys.</span></span> <span data-ttu-id="e13db-178">格納し、内のオブジェクトを取得でき、それらを抽出する場合は、特定の型にキャストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e13db-178">You can store and retrieve objects in it, and you'll need to cast them to a specific type when you extract them.</span></span> <span data-ttu-id="e13db-179">使用することができます`ViewData`ビューに、ビュー (部分ビュー、およびレイアウト) 内や、コント ローラーからデータを渡す。</span><span class="sxs-lookup"><span data-stu-id="e13db-179">You can use `ViewData` to pass data from a controller to views, as well as within views (and partial views and layouts).</span></span> <span data-ttu-id="e13db-180">文字列データの格納され、キャストがなくても、直接使用されることができます。</span><span class="sxs-lookup"><span data-stu-id="e13db-180">String data can be stored and used directly, without the need for a cast.</span></span>

<span data-ttu-id="e13db-181">いくつかの値を設定`ViewData`アクションで。</span><span class="sxs-lookup"><span data-stu-id="e13db-181">Set some values for `ViewData` in an action:</span></span>

```csharp
public IActionResult SomeAction()
   {
       ViewData["Greeting"] = "Hello";
       ViewData["Address"]  = new Address()
       {
           Name = "Steve",
           Street = "123 Main St",
           City = "Hudson",
           State = "OH",
           PostalCode = "44236"
       };

       return View();
   }
   ```

<span data-ttu-id="e13db-182">ビューでデータを操作します。</span><span class="sxs-lookup"><span data-stu-id="e13db-182">Work with the data in a view:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3, 6]}} -->

```html
@{
       // Requires cast
       var address = ViewData["Address"] as Address;
   }

   @ViewData["Greeting"] World!

   <address>
       @address.Name<br />
       @address.Street<br />
       @address.City, @address.State @address.PostalCode
   </address>
   ```

<span data-ttu-id="e13db-183">`ViewBag`オブジェクトに格納されているオブジェクトへの動的アクセスを提供する`ViewData`です。</span><span class="sxs-lookup"><span data-stu-id="e13db-183">The `ViewBag` objects provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="e13db-184">簡単にキャストする必要がないため、操作を指定できます。</span><span class="sxs-lookup"><span data-stu-id="e13db-184">This can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="e13db-185">使用して、上記と同じ例`ViewBag`厳密に型指定ではなく`address`ビュー内のインスタンス。</span><span class="sxs-lookup"><span data-stu-id="e13db-185">The same example as above, using `ViewBag` instead of a strongly typed `address` instance in the view:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1, 4, 5, 6]}} -->

```html
@ViewBag.Greeting World!

   <address>
       @ViewBag.Address.Name<br />
       @ViewBag.Address.Street<br />
       @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
   </address>
   ```

> [!NOTE]
> <span data-ttu-id="e13db-186">いずれも、同じ基になるを参照するので`ViewData`コレクションすることができますを混在させるし、の間で一致`ViewData`と`ViewBag`便利な場合、値を読み書きするときにします。</span><span class="sxs-lookup"><span data-stu-id="e13db-186">Since both refer to the same underlying `ViewData` collection, you can mix and match between `ViewData` and `ViewBag` when reading and writing values, if convenient.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="e13db-187">動的ビュー</span><span class="sxs-lookup"><span data-stu-id="e13db-187">Dynamic Views</span></span>

<span data-ttu-id="e13db-188">ビューがモデルの型を宣言しませんが、渡されたモデルのインスタンスは、このインスタンスを動的に参照できます。</span><span class="sxs-lookup"><span data-stu-id="e13db-188">Views that do not declare a model type but have a model instance passed to them can reference this instance dynamically.</span></span> <span data-ttu-id="e13db-189">場合のインスタンスなど、`Address`宣言していないので、ビューに渡される、`@model`ビューが示すように動的にインスタンスのプロパティを参照してもよい。</span><span class="sxs-lookup"><span data-stu-id="e13db-189">For example, if an instance of `Address` is passed to a view that doesn't declare an `@model`, the view would still be able to refer to the instance's properties dynamically as shown:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [13, 16, 17, 18]}} -->

```html
<address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

<span data-ttu-id="e13db-190">この機能は、いくつかの柔軟性を提供できますが、IntelliSense でもコンパイル保護は提供しません。</span><span class="sxs-lookup"><span data-stu-id="e13db-190">This feature can offer some flexibility, but does not offer any compilation protection or IntelliSense.</span></span> <span data-ttu-id="e13db-191">プロパティが存在しない場合、ページは実行時に失敗します。</span><span class="sxs-lookup"><span data-stu-id="e13db-191">If the property doesn't exist, the page will fail at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="e13db-192">詳細ビュー</span><span class="sxs-lookup"><span data-stu-id="e13db-192">More View Features</span></span>

<span data-ttu-id="e13db-193">[タグ ヘルパー](tag-helpers/intro.md)簡単にカスタム コードまたはビューの中でヘルパーを使用する必要がある、既存の HTML タグをサーバー側の動作を追加します。</span><span class="sxs-lookup"><span data-stu-id="e13db-193">[Tag helpers](tag-helpers/intro.md) make it easy to add server-side behavior to existing HTML tags, avoiding the need to use custom code or helpers within views.</span></span> <span data-ttu-id="e13db-194">タグ ヘルパーは、編集して、さまざまなツールで表示するビューのマークアップを許可するのには、馴染みがないエディターでは無視されますが、HTML 要素に属性として適用されます。</span><span class="sxs-lookup"><span data-stu-id="e13db-194">Tag helpers are applied as attributes to HTML elements, which are ignored by editors that aren't familiar with them, allowing view markup to be edited and rendered in a variety of tools.</span></span> <span data-ttu-id="e13db-195">タグ ヘルパーは、多くの用途し、具体的には行うことができます[フォームを使用する](working-with-forms.md)はるかに簡単です。</span><span class="sxs-lookup"><span data-stu-id="e13db-195">Tag helpers have many uses, and in particular can make [working with forms](working-with-forms.md) much easier.</span></span>

<span data-ttu-id="e13db-196">カスタムの HTML マークアップを生成する行うには多くの組み込み HTML ヘルパーおよびで (場合によっては、独自のデータ要件) のより複雑な UI ロジックをカプセル化できます[ビュー コンポーネント](view-components.md)です。</span><span class="sxs-lookup"><span data-stu-id="e13db-196">Generating custom HTML markup can be achieved with many built-in HTML Helpers, and more complex UI logic (potentially with its own data requirements) can be encapsulated in [View Components](view-components.md).</span></span> <span data-ttu-id="e13db-197">ビュー コンポーネントは、同じコント ローラーとビューを提供して、関心の分離を提供され、共通の UI 要素で使用されるデータを扱うアクションおよびビューは、する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e13db-197">View components provide the same separation of concerns that controllers and views offer, and can eliminate the need for actions and views to deal with data used by common UI elements.</span></span>

<span data-ttu-id="e13db-198">ASP.NET Core の他の多くの側面と同様にサポートをビュー[依存性の注入](../../fundamentals/dependency-injection.md)、するサービスを許可する[ビューに挿入される](dependency-injection.md)です。</span><span class="sxs-lookup"><span data-stu-id="e13db-198">Like many other aspects of ASP.NET Core, views support [dependency injection](../../fundamentals/dependency-injection.md), allowing services to be [injected into views](dependency-injection.md).</span></span>
