---
title: "モデル バインディング"
author: rachelappel
description: "ASP.NET Core mvc モデル バインディングに関する情報"
ms.author: rachelap
manager: wpickett
ms.date: 01/22/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
uid: mvc/models/model-binding
ms.openlocfilehash: 8fc6ff66d05164c1040f8cc77886357a633a0472
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2018
---
# <a name="model-binding"></a><span data-ttu-id="932d3-103">モデル バインディング</span><span class="sxs-lookup"><span data-stu-id="932d3-103">Model Binding</span></span>

<span data-ttu-id="932d3-104">によって[Rachel Appel](https://github.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="932d3-104">By [Rachel Appel](https://github.com/rachelappel)</span></span>

## <a name="introduction-to-model-binding"></a><span data-ttu-id="932d3-105">モデル バインディングの概要</span><span class="sxs-lookup"><span data-stu-id="932d3-105">Introduction to model binding</span></span>

<span data-ttu-id="932d3-106">ASP.NET Core mvc モデル バインディングでは、アクション メソッド パラメーターに HTTP 要求からデータをマップします。</span><span class="sxs-lookup"><span data-stu-id="932d3-106">Model binding in ASP.NET Core MVC maps data from HTTP requests to action method parameters.</span></span> <span data-ttu-id="932d3-107">パラメーターが文字列、整数、浮動小数点数などの単純型か複合型があります。</span><span class="sxs-lookup"><span data-stu-id="932d3-107">The parameters may be simple types such as strings, integers, or floats, or they may be complex types.</span></span> <span data-ttu-id="932d3-108">これは、データのサイズまたは複雑さに関係なく、多くの場合、繰り返しのシナリオには、対応する受信データにマッピングするための MVC の優れた機能です。</span><span class="sxs-lookup"><span data-stu-id="932d3-108">This is a great feature of MVC because mapping incoming data to a counterpart is an often repeated scenario, regardless of size or complexity of the data.</span></span> <span data-ttu-id="932d3-109">MVC では、開発者のすべてのアプリで同じコードを若干異なるバージョンの再書き換えを保持する必要はありませんので、バインドを取り除くによってこの問題を解決します。</span><span class="sxs-lookup"><span data-stu-id="932d3-109">MVC solves this problem by abstracting binding away so developers don't have to keep rewriting a slightly different version of that same code in every app.</span></span> <span data-ttu-id="932d3-110">コンバーター コードを入力するテキストの書き込みは時間がかかり、およびエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="932d3-110">Writing your own text to type converter code is tedious, and error prone.</span></span>

## <a name="how-model-binding-works"></a><span data-ttu-id="932d3-111">モデル バインディングのしくみ</span><span class="sxs-lookup"><span data-stu-id="932d3-111">How model binding works</span></span>

<span data-ttu-id="932d3-112">HTTP 要求を受信すると、MVC コント ローラーの特定のアクション メソッドにルーティングします。</span><span class="sxs-lookup"><span data-stu-id="932d3-112">When MVC receives an HTTP request, it routes it to a specific action method of a controller.</span></span> <span data-ttu-id="932d3-113">ルート データはに基づいて実行するアクション メソッドが決定し、そのアクション メソッドのパラメーターに HTTP 要求からの値をバインドします。</span><span class="sxs-lookup"><span data-stu-id="932d3-113">It determines which action method to run based on what is in the route data, then it binds values from the HTTP request to that action method's parameters.</span></span> <span data-ttu-id="932d3-114">たとえば、次の URL を考慮してください。</span><span class="sxs-lookup"><span data-stu-id="932d3-114">For example, consider the following URL:</span></span>

`http://contoso.com/movies/edit/2`

<span data-ttu-id="932d3-115">ルート テンプレートは、次のように見えるため`{controller=Home}/{action=Index}/{id?}`、`movies/edit/2`にルーティング、`Movies`コント ローラーとその`Edit`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="932d3-115">Since the route template looks like this, `{controller=Home}/{action=Index}/{id?}`, `movies/edit/2` routes to the `Movies` controller, and its `Edit` action method.</span></span> <span data-ttu-id="932d3-116">呼ばれるオプションのパラメーターも受け入れます`id`です。</span><span class="sxs-lookup"><span data-stu-id="932d3-116">It also accepts an optional parameter called `id`.</span></span> <span data-ttu-id="932d3-117">アクション メソッドのコードは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="932d3-117">The code for the action method should look something like this:</span></span>

```csharp
public IActionResult Edit(int? id)
   ```

<span data-ttu-id="932d3-118">メモ: URL ルート内の文字列は、大文字小文字を区別できません。</span><span class="sxs-lookup"><span data-stu-id="932d3-118">Note: The strings in the URL route are not case sensitive.</span></span>

<span data-ttu-id="932d3-119">MVC は要求データをアクション パラメーターを名前でバインドしようとします。</span><span class="sxs-lookup"><span data-stu-id="932d3-119">MVC will try to bind request data to the action parameters by name.</span></span> <span data-ttu-id="932d3-120">MVC は、パラメーター名とパブリック、設定可能なプロパティの名前を使用して各パラメーターの値を探します。</span><span class="sxs-lookup"><span data-stu-id="932d3-120">MVC will look for values for each parameter using the parameter name and the names of its public settable properties.</span></span> <span data-ttu-id="932d3-121">上記の例でのみアクション パラメーターの名前は`id`MVC は、ルートの値に同じ名前の値にバインドします。</span><span class="sxs-lookup"><span data-stu-id="932d3-121">In the above example, the only action parameter is named `id`, which MVC binds to the value with the same name in the route values.</span></span> <span data-ttu-id="932d3-122">ルートの値だけでなく MVC は、要求のさまざまな部分からデータをバインドは、セットの順序で。</span><span class="sxs-lookup"><span data-stu-id="932d3-122">In addition to route values MVC will bind data from various parts of the request and it does so in a set order.</span></span> <span data-ttu-id="932d3-123">モデル バインディングがそれらを検索する順序でデータ ソースの一覧を次に示します。</span><span class="sxs-lookup"><span data-stu-id="932d3-123">Below is a list of the data sources in the order that model binding looks through them:</span></span>

1. <span data-ttu-id="932d3-124">`Form values`: これらは、POST メソッドを使用して HTTP 要求のフォーム値です。</span><span class="sxs-lookup"><span data-stu-id="932d3-124">`Form values`: These are form values that go in the HTTP request using the POST method.</span></span> <span data-ttu-id="932d3-125">(jQuery POST 要求を含む)。</span><span class="sxs-lookup"><span data-stu-id="932d3-125">(including jQuery POST requests).</span></span>

2. <span data-ttu-id="932d3-126">`Route values`: 一連のルートの値によって提供される[ルーティング](xref:fundamentals/routing)</span><span class="sxs-lookup"><span data-stu-id="932d3-126">`Route values`: The set of route values provided by [Routing](xref:fundamentals/routing)</span></span>

3. <span data-ttu-id="932d3-127">`Query strings`: クエリ文字列の一部の URI。</span><span class="sxs-lookup"><span data-stu-id="932d3-127">`Query strings`: The query string part of the URI.</span></span>

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

<span data-ttu-id="932d3-128">注: フォームの値、ルート データ、およびクエリ文字列はすべての名前と値のペアとして格納します。</span><span class="sxs-lookup"><span data-stu-id="932d3-128">Note: Form values, route data, and query strings are all stored as name-value pairs.</span></span>

<span data-ttu-id="932d3-129">モデル バインディングがという名前のキーを求められた後`id`nothing を使用する必要があるとという名前`id`フォーム値のに移動されたルート値をそのキーを探しています。</span><span class="sxs-lookup"><span data-stu-id="932d3-129">Since model binding asked for a key named `id` and there is nothing named `id` in the form values, it moved on to the route values looking for that key.</span></span> <span data-ttu-id="932d3-130">この例では、一致するものを勧めします。</span><span class="sxs-lookup"><span data-stu-id="932d3-130">In our example, it's a match.</span></span> <span data-ttu-id="932d3-131">バインディングが発生し、値が 2 の整数に変換します。</span><span class="sxs-lookup"><span data-stu-id="932d3-131">Binding happens, and the value is converted to the integer 2.</span></span> <span data-ttu-id="932d3-132">編集 (文字列 id) を使用して要求を同じには「2」の文字列に変換します。</span><span class="sxs-lookup"><span data-stu-id="932d3-132">The same request using Edit(string id) would convert to the string "2".</span></span>

<span data-ttu-id="932d3-133">これまでの例は、単純型を使用します。</span><span class="sxs-lookup"><span data-stu-id="932d3-133">So far the example uses simple types.</span></span> <span data-ttu-id="932d3-134">MVC では、単純型は、任意の .NET プリミティブ型または文字列型コンバーターを使用した型をします。</span><span class="sxs-lookup"><span data-stu-id="932d3-134">In MVC simple types are any .NET primitive type or type with a string type converter.</span></span> <span data-ttu-id="932d3-135">アクション メソッドのパラメーターがクラスをなどがあった場合、`Movie`型で、MVC のモデル バインディングは、プロパティは、それを適切に処理を両方単純型と複合型を含んでいます。</span><span class="sxs-lookup"><span data-stu-id="932d3-135">If the action method's parameter were a class such as the `Movie` type, which contains both simple and complex types as properties, MVC's model binding will still handle it nicely.</span></span> <span data-ttu-id="932d3-136">一致を探して複合型のプロパティを走査するのにリフレクションと再帰を使用します。</span><span class="sxs-lookup"><span data-stu-id="932d3-136">It uses reflection and recursion to traverse the properties of complex types looking for matches.</span></span> <span data-ttu-id="932d3-137">モデル バインディングが、パターンを探して*parameter_name.property_name*プロパティに値をバインドします。</span><span class="sxs-lookup"><span data-stu-id="932d3-137">Model binding looks for the pattern *parameter_name.property_name* to bind values to properties.</span></span> <span data-ttu-id="932d3-138">このフォームの一致する値が見つからない場合は、プロパティ名だけを使用してバインドを試みます。</span><span class="sxs-lookup"><span data-stu-id="932d3-138">If it doesn't find matching values of this form, it will attempt to bind using just the property name.</span></span> <span data-ttu-id="932d3-139">などの種類に対して`Collection`型、モデル バインディングで一致検索を*parameter_name [index]*または*[index]*です。</span><span class="sxs-lookup"><span data-stu-id="932d3-139">For those types such as `Collection` types, model binding looks for matches to *parameter_name[index]* or just *[index]*.</span></span> <span data-ttu-id="932d3-140">モデル バインディング扱います`Dictionary`を求める同様に、型*parameter_name [キー]*または*[キー]*キーは、単純型限り、します。</span><span class="sxs-lookup"><span data-stu-id="932d3-140">Model binding treats  `Dictionary` types similarly, asking for *parameter_name[key]* or just *[key]*, as long as the keys are simple types.</span></span> <span data-ttu-id="932d3-141">サポートされているキーは、HTML のフィールド名と同じモデルの種類に対して生成されるタグ ヘルパーと一致します。</span><span class="sxs-lookup"><span data-stu-id="932d3-141">Keys that are supported match the field names HTML and tag helpers generated for the same model type.</span></span> <span data-ttu-id="932d3-142">維持されるようはフォームのフィールドの利便性のため、ユーザーの入力が入力など、ときに、作成または編集からバインドされたデータの検証に合格しませんでしたラウンド トリップの値が有効にします。</span><span class="sxs-lookup"><span data-stu-id="932d3-142">This enables round-tripping values so that the form fields remain filled with the user's input for their convenience, for example, when bound data from a create or edit did not pass validation.</span></span>

<span data-ttu-id="932d3-143">バインディングが発生するためにクラスにパブリックの既定のコンス トラクターが必要し、バインドされるメンバーはパブリックの書き込み可能なプロパティである必要があります。</span><span class="sxs-lookup"><span data-stu-id="932d3-143">In order for binding to happen the class must have a public default constructor and member to be bound must be public writable properties.</span></span> <span data-ttu-id="932d3-144">クラスはパブリックの既定のコンス トラクターを使用するインスタンスのみをモデル バインディングが発生したときにプロパティを設定することができます。</span><span class="sxs-lookup"><span data-stu-id="932d3-144">When model binding happens the class will only be instantiated using the public default constructor, then the properties can be set.</span></span>

<span data-ttu-id="932d3-145">パラメーターがバインドされている場合は、モデル バインディングがその名前を持つ値を探してを停止し、次のパラメーターをバインドする上を移動します。</span><span class="sxs-lookup"><span data-stu-id="932d3-145">When a parameter is bound, model binding stops looking for values with that name and it moves on to bind the next parameter.</span></span> <span data-ttu-id="932d3-146">それ以外の場合、モデル バインディング動作は、その種類に応じて既定値にパラメーターを設定します。</span><span class="sxs-lookup"><span data-stu-id="932d3-146">Otherwise, the default model binding behavior sets parameters to their default values depending on their type:</span></span>

* <span data-ttu-id="932d3-147">`T[]`: 例外の型の配列`byte[]`、バインディングで型のパラメーターが設定`T[]`に`Array.Empty<T>()`です。</span><span class="sxs-lookup"><span data-stu-id="932d3-147">`T[]`: With the exception of arrays of type `byte[]`, binding sets parameters of type `T[]` to `Array.Empty<T>()`.</span></span> <span data-ttu-id="932d3-148">型の配列`byte[]`に設定されている`null`です。</span><span class="sxs-lookup"><span data-stu-id="932d3-148">Arrays of type `byte[]` are set to `null`.</span></span>

* <span data-ttu-id="932d3-149">参照の種類: バインディング インスタンスを作成クラスの既定のコンス トラクターでプロパティを設定しない場合。</span><span class="sxs-lookup"><span data-stu-id="932d3-149">Reference Types: Binding creates an instance of a class with the default constructor without setting properties.</span></span> <span data-ttu-id="932d3-150">ただし、このモデル バインド セット`string`パラメーター`null`です。</span><span class="sxs-lookup"><span data-stu-id="932d3-150">However, model binding sets `string` parameters to `null`.</span></span>

* <span data-ttu-id="932d3-151">Null 許容型: null 許容型に設定されて`null`です。</span><span class="sxs-lookup"><span data-stu-id="932d3-151">Nullable Types: Nullable types are set to `null`.</span></span> <span data-ttu-id="932d3-152">上記の例では、モデル バインディング セット`id`に`null`型であるため`int?`です。</span><span class="sxs-lookup"><span data-stu-id="932d3-152">In the above example, model binding sets `id` to `null` since it is of type `int?`.</span></span>

* <span data-ttu-id="932d3-153">値の型: 型の null 非許容の値の型`T`に設定されている`default(T)`です。</span><span class="sxs-lookup"><span data-stu-id="932d3-153">Value Types: Non-nullable value types of type `T` are set to `default(T)`.</span></span> <span data-ttu-id="932d3-154">モデル バインディング パラメーターの設定など、`int id`を 0 にします。</span><span class="sxs-lookup"><span data-stu-id="932d3-154">For example, model binding will set a parameter `int id` to 0.</span></span> <span data-ttu-id="932d3-155">既定値に依存するのではなく、モデルの検証または null 許容型を使用して検討してください。</span><span class="sxs-lookup"><span data-stu-id="932d3-155">Consider using model validation or nullable types rather than relying on default values.</span></span>

<span data-ttu-id="932d3-156">バインドが失敗する、MVC では、エラーはスローされません。</span><span class="sxs-lookup"><span data-stu-id="932d3-156">If binding fails, MVC does not throw an error.</span></span> <span data-ttu-id="932d3-157">ユーザー入力を受け付けるすべてのアクションを確認する必要があります、`ModelState.IsValid`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="932d3-157">Every action which accepts user input should check the `ModelState.IsValid` property.</span></span>

<span data-ttu-id="932d3-158">注意: コント ローラーのエントリの各`ModelState`プロパティは、`ModelStateEntry`を含む、`Errors`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="932d3-158">Note: Each entry in the controller's `ModelState` property is a `ModelStateEntry` containing an `Errors` property.</span></span> <span data-ttu-id="932d3-159">このコレクションを照会することはほとんどありません必要があります。</span><span class="sxs-lookup"><span data-stu-id="932d3-159">It's rarely necessary to query this collection yourself.</span></span> <span data-ttu-id="932d3-160">代わりに、`ModelState.IsValid` を使用してください。</span><span class="sxs-lookup"><span data-stu-id="932d3-160">Use `ModelState.IsValid` instead.</span></span>

<span data-ttu-id="932d3-161">また、モデル バインディングを実行するときに、MVC は考慮する必要がありますのあるいくつかの特別なデータ型があります。</span><span class="sxs-lookup"><span data-stu-id="932d3-161">Additionally, there are some special data types that MVC must consider when performing model binding:</span></span>

* <span data-ttu-id="932d3-162">`IFormFile`、 `IEnumerable<IFormFile>`: HTTP 要求の一部である 1 つまたは複数のアップロードされたファイルです。</span><span class="sxs-lookup"><span data-stu-id="932d3-162">`IFormFile`, `IEnumerable<IFormFile>`: One or more uploaded files that are part of the HTTP request.</span></span>

* <span data-ttu-id="932d3-163">`CancellationToken`: 非同期コント ローラーでアクティビティをキャンセルするために使用します。</span><span class="sxs-lookup"><span data-stu-id="932d3-163">`CancellationToken`: Used to cancel activity in asynchronous controllers.</span></span>

<span data-ttu-id="932d3-164">これらの型は、クラス型またはプロパティをアクション パラメーターにバインドできます。</span><span class="sxs-lookup"><span data-stu-id="932d3-164">These types can be bound to action parameters or to properties on a class type.</span></span>

<span data-ttu-id="932d3-165">モデル バインディングが完了したら、[検証](validation.md)に発生します。</span><span class="sxs-lookup"><span data-stu-id="932d3-165">Once model binding is complete, [Validation](validation.md) occurs.</span></span> <span data-ttu-id="932d3-166">モデル バインディングの既定では、膨大な開発シナ リオの優れたは動作します。</span><span class="sxs-lookup"><span data-stu-id="932d3-166">Default model binding works great for the vast majority of development scenarios.</span></span> <span data-ttu-id="932d3-167">独自のニーズがある場合は、組み込みのビヘイビアーをカスタマイズするためにも拡張可能です。</span><span class="sxs-lookup"><span data-stu-id="932d3-167">It is also extensible so if you have unique needs you can customize the built-in behavior.</span></span>

## <a name="customize-model-binding-behavior-with-attributes"></a><span data-ttu-id="932d3-168">属性を持つモデル バインディング動作をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="932d3-168">Customize model binding behavior with attributes</span></span>

<span data-ttu-id="932d3-169">MVC には、別のソースにその既定のモデル バインディング動作を送信するために使用できるいくつかの属性が含まれています。</span><span class="sxs-lookup"><span data-stu-id="932d3-169">MVC contains several attributes that you can use to direct its default model binding behavior to a different source.</span></span> <span data-ttu-id="932d3-170">たとえば、バインディングのプロパティが必要なかどうか、またはを使用してすべての発生する必要がありますしない場合を指定できます、`[BindRequired]`または`[BindNever]`属性。</span><span class="sxs-lookup"><span data-stu-id="932d3-170">For example, you can specify whether binding is required for a property, or if it should never happen at all by using the `[BindRequired]` or `[BindNever]` attributes.</span></span> <span data-ttu-id="932d3-171">または、既定のデータ ソースを上書きし、モデル バインダーのデータ ソースを指定できます。</span><span class="sxs-lookup"><span data-stu-id="932d3-171">Alternatively, you can override the default data source, and specify the model binder's data source.</span></span> <span data-ttu-id="932d3-172">以下には、モデル バインディング属性の一覧を示します。</span><span class="sxs-lookup"><span data-stu-id="932d3-172">Below is a list of model binding attributes:</span></span>

* <span data-ttu-id="932d3-173">`[BindRequired]`: この属性は、バインドできませんが発生した場合、モデル状態エラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="932d3-173">`[BindRequired]`: This attribute adds a model state error if binding cannot occur.</span></span>

* <span data-ttu-id="932d3-174">`[BindNever]`: このパラメーターにバインドしないモデル バインダーに指示します。</span><span class="sxs-lookup"><span data-stu-id="932d3-174">`[BindNever]`: Tells the model binder to never bind to this parameter.</span></span>

* <span data-ttu-id="932d3-175">`[FromHeader]`、 `[FromQuery]`、 `[FromRoute]`、 `[FromForm]`: これらを使用して適用する正確なバインディング ソースを指定します。</span><span class="sxs-lookup"><span data-stu-id="932d3-175">`[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: Use these to specify the exact binding source you want to apply.</span></span>

* <span data-ttu-id="932d3-176">`[FromServices]`: この属性を使用して[依存性の注入](../../fundamentals/dependency-injection.md)services からパラメーターをバインドします。</span><span class="sxs-lookup"><span data-stu-id="932d3-176">`[FromServices]`: This attribute uses [dependency injection](../../fundamentals/dependency-injection.md) to bind parameters from services.</span></span>

* <span data-ttu-id="932d3-177">`[FromBody]`: 要求の本文からデータをバインドに構成されたフォーマッタを使用します。</span><span class="sxs-lookup"><span data-stu-id="932d3-177">`[FromBody]`: Use the configured formatters to bind data from the request body.</span></span> <span data-ttu-id="932d3-178">フォーマッタが要求のコンテンツの種類に基づいて選択されます。</span><span class="sxs-lookup"><span data-stu-id="932d3-178">The formatter is selected based on content type of the request.</span></span>

* <span data-ttu-id="932d3-179">`[ModelBinder]`: 既定のモデル バインダー、バインディング ソース名を変更するために使用します。</span><span class="sxs-lookup"><span data-stu-id="932d3-179">`[ModelBinder]`: Used to override the default model binder, binding source and name.</span></span>

<span data-ttu-id="932d3-180">属性は、モデル バインディングの既定の動作をオーバーライドする必要がある場合に非常に便利なツールです。</span><span class="sxs-lookup"><span data-stu-id="932d3-180">Attributes are very helpful tools when you need to override the default behavior of model binding.</span></span>

## <a name="bind-formatted-data-from-the-request-body"></a><span data-ttu-id="932d3-181">要求本文の書式設定されたデータをバインドします。</span><span class="sxs-lookup"><span data-stu-id="932d3-181">Bind formatted data from the request body</span></span>

<span data-ttu-id="932d3-182">要求データは、さまざまな JSON、XML、およびその他の多くを含む形式で取得できます。</span><span class="sxs-lookup"><span data-stu-id="932d3-182">Request data can come in a variety of formats including JSON, XML and many others.</span></span> <span data-ttu-id="932d3-183">要求本文内のデータにパラメーターをバインドすることを示すために、[FromBody] 属性を使用する場合、MVC は、コンテンツの種類に基づく要求データを処理する構成済みのフォーマッタのセットを使用します。</span><span class="sxs-lookup"><span data-stu-id="932d3-183">When you use the [FromBody] attribute to indicate that you want to bind a parameter to data in the request body, MVC uses a configured set of formatters to handle the request data based on its content type.</span></span> <span data-ttu-id="932d3-184">既定では、MVC が含まれます、`JsonInputFormatter`クラス XML とその他のカスタム形式の処理に関するその他のフォーマッタを追加できますが JSON データを処理します。</span><span class="sxs-lookup"><span data-stu-id="932d3-184">By default MVC includes a `JsonInputFormatter` class for handling JSON data, but you can add additional formatters for handling XML and other custom formats.</span></span>

> [!NOTE]
> <span data-ttu-id="932d3-185">ある多くてで修飾されたアクション パラメーターを 1 つ`[FromBody]`です。</span><span class="sxs-lookup"><span data-stu-id="932d3-185">There can be at most one parameter per action decorated with `[FromBody]`.</span></span> <span data-ttu-id="932d3-186">ASP.NET Core MVC の実行時では、フォーマッタに要求ストリームを読み取る責任を委任します。</span><span class="sxs-lookup"><span data-stu-id="932d3-186">The ASP.NET Core MVC run-time delegates the responsibility of reading the request stream to the formatter.</span></span> <span data-ttu-id="932d3-187">要求ストリームは、パラメーターの読み取りは後、は一般に他のバインド用にもう一度要求ストリームを読み取る`[FromBody]`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="932d3-187">Once the request stream is read for a parameter, it's generally not possible to read the request stream again for binding other `[FromBody]` parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="932d3-188">`JsonInputFormatter`は、既定のフォーマッタとに基づいて[Json.NET](https://www.newtonsoft.com/json)です。</span><span class="sxs-lookup"><span data-stu-id="932d3-188">The `JsonInputFormatter` is the default formatter and is based on [Json.NET](https://www.newtonsoft.com/json).</span></span>

<span data-ttu-id="932d3-189">ASP.NET 選択に基づいて入力フォーマッタ、[コンテンツの種類](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html)ヘッダーおよびパラメーターの型には、それ以外の場合を指定して適用する属性がない限り、します。</span><span class="sxs-lookup"><span data-stu-id="932d3-189">ASP.NET selects input formatters based on the [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) header and the type of the parameter, unless there is an attribute applied to it specifying otherwise.</span></span> <span data-ttu-id="932d3-190">XML を使用するか、または別の形式をする必要があります構成で、 *Startup.cs*への参照を取得するファイルの最初が`Microsoft.AspNetCore.Mvc.Formatters.Xml`NuGet を使用します。</span><span class="sxs-lookup"><span data-stu-id="932d3-190">If you'd like to use XML or another format you must configure it in the *Startup.cs* file, but you may first have to obtain a reference to `Microsoft.AspNetCore.Mvc.Formatters.Xml` using NuGet.</span></span> <span data-ttu-id="932d3-191">スタートアップ コードは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="932d3-191">Your startup code should look something like this:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddXmlSerializerFormatters();
   }
```

<span data-ttu-id="932d3-192">内のコード、 *Startup.cs*ファイルが含まれています、`ConfigureServices`メソッドを`services`ASP.NET アプリのサービスの構築に使用できる引数。</span><span class="sxs-lookup"><span data-stu-id="932d3-192">Code in the *Startup.cs* file contains a `ConfigureServices` method with a `services` argument you can use to build up services for your ASP.NET app.</span></span> <span data-ttu-id="932d3-193">サンプルでは、MVC は、このアプリに提供するサービスとして XML フォーマッタを追加おされます。</span><span class="sxs-lookup"><span data-stu-id="932d3-193">In the sample, we are adding an XML formatter as a service that MVC will provide for this app.</span></span> <span data-ttu-id="932d3-194">`options`に渡される引数、`AddMvc`メソッドでは、追加およびアプリの起動時に、MVC のフィルター、フォーマッタ、およびその他のシステム オプションを管理することができます。</span><span class="sxs-lookup"><span data-stu-id="932d3-194">The `options` argument passed into the `AddMvc` method allows you to add and manage filters, formatters, and other system options from MVC upon app startup.</span></span> <span data-ttu-id="932d3-195">適用し、`Consumes`属性をコント ローラー クラスまたは希望の形式を使用するアクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="932d3-195">Then apply the `Consumes` attribute to controller classes or action methods to work with the format you want.</span></span>

### <a name="custom-model-binding"></a><span data-ttu-id="932d3-196">カスタム モデル バインディング</span><span class="sxs-lookup"><span data-stu-id="932d3-196">Custom Model Binding</span></span>

<span data-ttu-id="932d3-197">モデル バインディングを拡張するには、独自のカスタム モデル バインダーを記述します。</span><span class="sxs-lookup"><span data-stu-id="932d3-197">You can extend model binding by writing your own custom model binders.</span></span> <span data-ttu-id="932d3-198">詳細については[カスタム モデル バインディング](../advanced/custom-model-binding.md)です。</span><span class="sxs-lookup"><span data-stu-id="932d3-198">Learn more about [custom model binding](../advanced/custom-model-binding.md).</span></span>
