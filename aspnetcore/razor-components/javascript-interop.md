---
title: Razor の JavaScript のコンポーネントの相互運用
author: guardrex
description: .NET および .NET から JavaScript 関数を呼び出す方法について説明します Blazor と剃刀コンポーネントのアプリの JavaScript からのメソッド。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/javascript-interop
ms.openlocfilehash: f2588f4ed1ec2f01218283625fae4632d0a8ae58
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515682"
---
# <a name="razor-components-javascript-interop"></a><span data-ttu-id="8fe69-103">Razor の JavaScript のコンポーネントの相互運用</span><span class="sxs-lookup"><span data-stu-id="8fe69-103">Razor Components JavaScript interop</span></span>

<span data-ttu-id="8fe69-104">によって[Javier Calvarro Nelson](https://github.com/javiercn)、 [Daniel Roth](https://github.com/danroth27)、および[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8fe69-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="8fe69-105">Razor のコンポーネント アプリが .NET および .NET から JavaScript 関数を呼び出すことができますの JavaScript コードからメソッド。</span><span class="sxs-lookup"><span data-stu-id="8fe69-105">A Razor Components app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="8fe69-106">.NET のメソッドからの JavaScript 関数を呼び出す</span><span class="sxs-lookup"><span data-stu-id="8fe69-106">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="8fe69-107">.NET コードが JavaScript 関数を呼び出すために必要な場合もあります。</span><span class="sxs-lookup"><span data-stu-id="8fe69-107">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="8fe69-108">たとえば、JavaScript の呼び出しでは、ブラウザーの機能または JavaScript ライブラリから、アプリの機能を公開できます。</span><span class="sxs-lookup"><span data-stu-id="8fe69-108">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="8fe69-109">.NET から JavaScript を呼び出すを使用して、`IJSRuntime`抽象化します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-109">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="8fe69-110">`InvokeAsync<T>`を任意の数の JSON シリアル化可能な引数と共に起動される JavaScript 関数の識別子を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="8fe69-110">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="8fe69-111">関数識別子は、グローバル スコープの相対的な (`window`)。</span><span class="sxs-lookup"><span data-stu-id="8fe69-111">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="8fe69-112">呼び出す場合`window.someScope.someFunction`、識別子は`someScope.someFunction`します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-112">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="8fe69-113">これを呼び出す前に、関数を登録する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="8fe69-113">There's no need to register the function before it's called.</span></span> <span data-ttu-id="8fe69-114">戻り値の型`T`も JSON シリアル化できなければなりません。</span><span class="sxs-lookup"><span data-stu-id="8fe69-114">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="8fe69-115">サーバー側の ASP.NET Core の Razor コンポーネント向け。</span><span class="sxs-lookup"><span data-stu-id="8fe69-115">For server-side ASP.NET Core Razor Components apps:</span></span>

* <span data-ttu-id="8fe69-116">複数のユーザー要求は、サーバー側のアプリによって処理されます。</span><span class="sxs-lookup"><span data-stu-id="8fe69-116">Multiple user requests are processed by the server-side app.</span></span> <span data-ttu-id="8fe69-117">呼び出さない`JSRuntime.Current`で JavaScript 関数を呼び出すためのコンポーネント。</span><span class="sxs-lookup"><span data-stu-id="8fe69-117">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="8fe69-118">挿入、`IJSRuntime`抽象化と相互運用機能の JavaScript 呼び出しを発行するため、挿入されたオブジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-118">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>

<span data-ttu-id="8fe69-119">次の例がに基づいて[TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder)、試験段階の JavaScript ベースのデコーダーです。</span><span class="sxs-lookup"><span data-stu-id="8fe69-119">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="8fe69-120">例からの JavaScript 関数を呼び出す方法を示します、C#メソッド。</span><span class="sxs-lookup"><span data-stu-id="8fe69-120">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="8fe69-121">JavaScript 関数はからのバイト配列を受け取り、C#メソッドは、配列をデコードし、コンポーネントの表示にテキストを返します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-121">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="8fe69-122">内で、`<head>`要素の*wwwroot/index.html*を使用する関数を提供`TextDecoder`渡された配列をデコードします。</span><span class="sxs-lookup"><span data-stu-id="8fe69-122">Inside the `<head>` element of *wwwroot/index.html*, provide a function that uses `TextDecoder` to decode a passed array:</span></span>

```html
<script>
  window.ConvertArray = (win1251Array) => {
    var win1251decoder = new TextDecoder('windows-1251');
    var bytes = new Uint8Array(win1251Array);
    var decodedArray = win1251decoder.decode(bytes);
    console.log(decodedArray);
    return decodedArray;
  };
</script>
```

<span data-ttu-id="8fe69-123">前の例に示すようにコードなどの JavaScript コードは、JavaScript ファイルから読み込むこともできます (*.js*) でスクリプト ファイルへの参照、 *wwwroot/index.html*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8fe69-123">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file in the *wwwroot/index.html* file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="8fe69-124">次のコンポーネント:</span><span class="sxs-lookup"><span data-stu-id="8fe69-124">The following component:</span></span>

* <span data-ttu-id="8fe69-125">呼び出す、 `ConvertArray` JavaScript 関数を使用して`JsRuntime`コンポーネント ボタン (**変換配列**) が選択されています。</span><span class="sxs-lookup"><span data-stu-id="8fe69-125">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="8fe69-126">JavaScript 関数が呼び出されると、渡された配列を文字列に変換されます。</span><span class="sxs-lookup"><span data-stu-id="8fe69-126">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="8fe69-127">表示コンポーネントに、文字列が返されます。</span><span class="sxs-lookup"><span data-stu-id="8fe69-127">The string is returned to the component for display.</span></span>

```cshtml
@page "/"
@inject IJSRuntime JsRuntime;

<h1>Call JavaScript Function Example</h1>

<button type="button" class="btn btn-primary" onclick="@ConvertArray">
    Convert Array
</button>

<p class="mt-2" style="font-size:1.6em">
    <span class="badge badge-success">
        @ConvertedText
    </span>
</p>

@functions {
    // Quote (c)2005 Universal Pictures: Serenity
    // https://www.uphe.com/movies/serenity
    // David Krumholtz on IMDB: https://www.imdb.com/name/nm0472710/

    private MarkupString ConvertedText =
        new MarkupString("Select the <b>Convert Array</b> button.");

    private uint[] QuoteArray = new uint[]
        {
            60, 101, 109, 62, 67, 97, 110, 39, 116, 32, 115, 116, 111, 112, 32,
            116, 104, 101, 32, 115, 105, 103, 110, 97, 108, 44, 32, 77, 97,
            108, 46, 60, 47, 101, 109, 62, 32, 45, 32, 77, 114, 46, 32, 85, 110,
            105, 118, 101, 114, 115, 101, 10, 10,
        };

    private async void ConvertArray()
    {
        var text =
            await JsRuntime.InvokeAsync<string>("ConvertArray", QuoteArray);

        ConvertedText = new MarkupString(text);

        StateHasChanged();
    }
}
```

<span data-ttu-id="8fe69-128">使用する、`IJSRuntime`抽象化では、次の方法のいずれかを採用します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-128">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="8fe69-129">挿入、 `IJSRuntime` Razor ファイルに抽象化 (*.razor*、 *.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="8fe69-129">Inject the `IJSRuntime` abstraction into the Razor file (*.razor*, *.cshtml*):</span></span>

  ```cshtml
  @inject IJSRuntime JSRuntime

  @functions {
      public override void OnInit()
      {
          StocksService.OnStockTickerUpdated += stockUpdate =>
          {
              JSRuntime.InvokeAsync<object>(
                  "handleTickerChanged",
                  stockUpdate.symbol,
                  stockUpdate.price);
          };
      }
  }
  ```

* <span data-ttu-id="8fe69-130">挿入、`IJSRuntime`クラスに抽象化 (*.cs*)。</span><span class="sxs-lookup"><span data-stu-id="8fe69-130">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  ```csharp
  public class JsInteropClasses
  {
      private readonly IJSRuntime _jsRuntime;

      public JsInteropClasses(IJSRuntime jsRuntime)
      {
          _jsRuntime = jsRuntime;
      }

      public Task<string> TickerChanged(string data)
      {
          // The handleTickerChanged JavaScript method is implemented
          // in a JavaScript file, such as 'wwwroot/tickerJsInterop.js'.
          return _jsRuntime.InvokeAsync<object>(
              "handleTickerChanged",
              stockUpdate.symbol,
              stockUpdate.price);
      }
  }
  ```

* <span data-ttu-id="8fe69-131">動的なコンテンツ生成の`BuildRenderTree`を使用して、`[Inject]`属性。</span><span class="sxs-lookup"><span data-stu-id="8fe69-131">For dynamic content generation with `BuildRenderTree`, use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject] IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="8fe69-132">このトピックに付属しているクライアント側のサンプル アプリでは、2 つの JavaScript 関数をユーザー入力を受信して、ようこそメッセージを表示する DOM と対話するクライアント側のアプリを使用できます。</span><span class="sxs-lookup"><span data-stu-id="8fe69-132">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the client-side app that interact with the DOM to receive user input and display a welcome message:</span></span>

* `showPrompt` <span data-ttu-id="8fe69-133">&ndash; ユーザー入力 (ユーザーの名前) に同意を求めるメッセージを生成し、呼び出し元に名前を返します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-133">&ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* `displayWelcome` <span data-ttu-id="8fe69-134">&ndash; ようこそメッセージを呼び出し元からの DOM オブジェクトに割り当てて、`id`の`welcome`します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-134">&ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="8fe69-135">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="8fe69-135">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="8fe69-136">場所、`<script>`タグで JavaScript ファイルを参照する、 *wwwroot/index.html*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8fe69-136">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file:</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="8fe69-137">配置しないように、`<script>`コンポーネント ファイルにタグを付けるため、`<script>`タグを動的に更新することはできません。</span><span class="sxs-lookup"><span data-stu-id="8fe69-137">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="8fe69-138">JavaScript と .NET のメソッドの相互運用機能、 *exampleJsInterop.js*を呼び出してファイル`IJSRuntime.InvokeAsync<T>`します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-138">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="8fe69-139">`IJSRuntime`抽象化はサーバー側のシナリオ用に非同期です。</span><span class="sxs-lookup"><span data-stu-id="8fe69-139">The `IJSRuntime` abstraction is asynchronous to allow for server-side scenarios.</span></span> <span data-ttu-id="8fe69-140">クライアント側でアプリを実行して、同期的に、JavaScript 関数を呼び出す場合にダウン キャスト`IJSInProcessRuntime`を呼び出すと`Invoke<T>`代わりにします。</span><span class="sxs-lookup"><span data-stu-id="8fe69-140">If the app runs client-side and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="8fe69-141">ほとんどの JavaScript の相互運用ライブラリが、ライブラリがすべてのシナリオ、クライアント側またはサーバー側で利用可能であることを確認する非同期 Api を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="8fe69-141">We recommend that most JavaScript interop libraries use the async APIs to ensure that the libraries are available in all scenarios, client-side or server-side.</span></span>

<span data-ttu-id="8fe69-142">サンプル アプリには、JavaScript の相互運用機能を示すために、コンポーネントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8fe69-142">The sample app includes a component to demonstrate JavaScript interop.</span></span> <span data-ttu-id="8fe69-143">コンポーネント:</span><span class="sxs-lookup"><span data-stu-id="8fe69-143">The component:</span></span>

* <span data-ttu-id="8fe69-144">JavaScript のプロンプトを使用してユーザー入力を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="8fe69-144">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="8fe69-145">コンポーネントの処理にテキストを返します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-145">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="8fe69-146">ようこそメッセージを表示する DOM とやり取りする 2 つ目の JavaScript 関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-146">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="8fe69-147">*Pages/JSInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8fe69-147">*Pages/JSInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="8fe69-148">ときに`TriggerJsPrompt`ことで、コンポーネントの実行は**トリガー JavaScript プロンプト**ボタン、JavaScript`showPrompt`で提供される関数、 *wwwroot/exampleJsInterop.js*ファイルと呼ばれる。</span><span class="sxs-lookup"><span data-stu-id="8fe69-148">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="8fe69-149">`showPrompt`関数は HTML でエンコードされ、コンポーネントに返されるは、ユーザー入力 (ユーザーの名前) を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="8fe69-149">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="8fe69-150">コンポーネントがローカル変数では、ユーザーの名前を格納`name`します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-150">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="8fe69-151">格納されている文字列`name`JavaScript 関数に渡されると、ようこそメッセージに組み込まれます`displayWelcome`、ようこそメッセージに見出しタグをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="8fe69-151">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="capture-references-to-elements"></a><span data-ttu-id="8fe69-152">要素への参照をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="8fe69-152">Capture references to elements</span></span>

<span data-ttu-id="8fe69-153">いくつか[JavaScript の相互運用機能](xref:razor-components/javascript-interop)シナリオには、HTML 要素への参照が必要があります。</span><span class="sxs-lookup"><span data-stu-id="8fe69-153">Some [JavaScript interop](xref:razor-components/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="8fe69-154">たとえば、UI ライブラリでは、初期化の要素の参照がありますまたはなど、要素をコマンドのような Api を呼び出す必要があります`focus`または`play`します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-154">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="8fe69-155">追加することで、コンポーネントでの HTML 要素への参照をキャプチャすることができます、 `ref` HTML 要素に属性し、型のフィールドを定義し、`ElementRef`名前の値に一致する、`ref`属性。</span><span class="sxs-lookup"><span data-stu-id="8fe69-155">You can capture references to HTML elements in a component by adding a `ref` attribute to the HTML element and then defining a field of type `ElementRef` whose name matches the value of the `ref` attribute.</span></span>

<span data-ttu-id="8fe69-156">次の例では、ユーザー名の入力要素への参照をキャプチャは示しています。</span><span class="sxs-lookup"><span data-stu-id="8fe69-156">The following example shows capturing a reference to the username input element:</span></span>

```cshtml
<input ref="username" ...>

@functions {
    ElementRef username;
}
```

> [!NOTE]
> <span data-ttu-id="8fe69-157">**いない**DOM を読み込む方法としてキャプチャされた要素の参照を使用</span><span class="sxs-lookup"><span data-stu-id="8fe69-157">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="8fe69-158">そうは、宣言型のレンダリング モデルに干渉する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8fe69-158">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="8fe69-159">.NET コードに関する限り、`ElementRef`は、不透明なハンドルです。</span><span class="sxs-lookup"><span data-stu-id="8fe69-159">As far as .NET code is concerned, an `ElementRef` is an opaque handle.</span></span> <span data-ttu-id="8fe69-160">*のみ*で行えること`ElementRef`JavaScript の相互運用機能を介して JavaScript コードに渡すからです。</span><span class="sxs-lookup"><span data-stu-id="8fe69-160">The *only* thing you can do with `ElementRef` is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="8fe69-161">これを行うときに、JavaScript 側コードを受け取る、`HTMLElement`インスタンスで、通常の DOM Api を使用できます。</span><span class="sxs-lookup"><span data-stu-id="8fe69-161">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="8fe69-162">たとえば、次のコードでは、要素にフォーカスを設定できるようにする .NET 拡張機能メソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-162">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="8fe69-163">*exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="8fe69-163">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="8fe69-164">使用`IJSRuntime.InvokeAsync<T>`を呼び出すと`exampleJsFunctions.focusElement`で、`ElementRef`要素に注目します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-164">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementRef` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.cshtml?highlight=1,3,7,11-12)]

<span data-ttu-id="8fe69-165">要素に注目する拡張メソッドを使用するには、受信する静的拡張メソッドを作成、`IJSRuntime`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="8fe69-165">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementRef elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="8fe69-166">メソッドは、オブジェクトで直接呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="8fe69-166">The method is called directly on the object.</span></span> <span data-ttu-id="8fe69-167">次の例では、静的な`Focus`メソッドは、`JsInteropClasses`名前空間。</span><span class="sxs-lookup"><span data-stu-id="8fe69-167">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.cshtml?highlight=1,4,8,12)]

> [!IMPORTANT]
> <span data-ttu-id="8fe69-168">`username`コンポーネントをレンダリングし、その出力に含まれる後にのみ変数が設定されます、`>`要素。</span><span class="sxs-lookup"><span data-stu-id="8fe69-168">The `username` variable is only populated after the component renders and its output includes the `>` element.</span></span> <span data-ttu-id="8fe69-169">値を渡そうとすると場合`ElementRef`、JavaScript コードを JavaScript コードを受け取る`null`します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-169">If you try to pass an unpopulated `ElementRef` to JavaScript code, the JavaScript code receives `null`.</span></span> <span data-ttu-id="8fe69-170">コンポーネントでの表示 (要素で初期フォーカスを設定する) の使用が終了した後、要素の参照を操作する、`OnAfterRenderAsync`または`OnAfterRender`[コンポーネントのライフ サイクル メソッド](xref:razor-components/components#lifecycle-methods)します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-170">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:razor-components/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="8fe69-171">JavaScript 関数からの .NET メソッドを呼び出し</span><span class="sxs-lookup"><span data-stu-id="8fe69-171">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="8fe69-172">.NET の静的メソッド呼び出し</span><span class="sxs-lookup"><span data-stu-id="8fe69-172">Static .NET method call</span></span>

<span data-ttu-id="8fe69-173">JavaScript から静的 .NET メソッドを呼び出すを使用して、`DotNet.invokeMethod`または`DotNet.invokeMethodAsync`関数。</span><span class="sxs-lookup"><span data-stu-id="8fe69-173">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="8fe69-174">静的メソッドを呼び出すには、関数、および任意の引数を含むアセンブリの名前の識別子を渡します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-174">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="8fe69-175">サーバー側のシナリオをサポートするために、非同期バージョンが優先されます。</span><span class="sxs-lookup"><span data-stu-id="8fe69-175">The asynchronous version is preferred to support server-side scenarios.</span></span> <span data-ttu-id="8fe69-176">パブリック (静的) とで装飾するには JavaScript から呼び出し可能な .NET メソッドがあります`[JSInvokable]`します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-176">To be invokable from JavaScript, the .NET method must be public, static, and decorated with `[JSInvokable]`.</span></span> <span data-ttu-id="8fe69-177">既定では、メソッドの識別子には、メソッド名を使用して、別の識別子を指定することができます、`JSInvokableAttribute`コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="8fe69-177">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="8fe69-178">オープン ジェネリック メソッドを呼び出すことは現在サポートされていません。</span><span class="sxs-lookup"><span data-stu-id="8fe69-178">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="8fe69-179">サンプル アプリでは、C#の配列を返すメソッドを`int`秒。</span><span class="sxs-lookup"><span data-stu-id="8fe69-179">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="8fe69-180">メソッドがで修飾された、`JSInvokable`属性。</span><span class="sxs-lookup"><span data-stu-id="8fe69-180">The method is decorated with the `JSInvokable` attribute.</span></span>

<span data-ttu-id="8fe69-181">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8fe69-181">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="8fe69-182">クライアントに提供する JavaScript を呼び出す、 C# .NET メソッド。</span><span class="sxs-lookup"><span data-stu-id="8fe69-182">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="8fe69-183">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="8fe69-183">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="8fe69-184">ときに、**トリガー .NET 静的メソッド ReturnArrayAsync**ボタンが選択されている場合、ブラウザーの web 開発ツールでコンソール出力を確認します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-184">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="8fe69-185">コンソール出力は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="8fe69-185">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="8fe69-186">4 番目の配列の値が配列にプッシュされます (`data.push(4);`) によって返される`ReturnArrayAsync`します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-186">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="8fe69-187">インスタンス メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="8fe69-187">Instance method call</span></span>

<span data-ttu-id="8fe69-188">JavaScript から .NET インスタンス メソッドを呼び出すこともできます。</span><span class="sxs-lookup"><span data-stu-id="8fe69-188">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="8fe69-189">JavaScript から .NET インスタンス メソッドを呼び出すには、まず、.NET インスタンスに渡す JavaScript でラップして、`DotNetObjectRef`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="8fe69-189">To invoke a .NET instance method from JavaScript, first pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectRef` instance.</span></span> <span data-ttu-id="8fe69-190">.NET のインスタンスが、JavaScript への参照によって渡され、.NET のインスタンスを使用して、インスタンス メソッドを呼び出すことができます、`invokeMethod`または`invokeMethodAsync`関数。</span><span class="sxs-lookup"><span data-stu-id="8fe69-190">The .NET instance is passed by reference to JavaScript, and you can invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="8fe69-191">JavaScript からその他の .NET メソッドを呼び出すときに、.NET インスタンスを引数として渡すもできます。</span><span class="sxs-lookup"><span data-stu-id="8fe69-191">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="8fe69-192">サンプル アプリでは、クライアント側のコンソールにメッセージを記録します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-192">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="8fe69-193">サンプル アプリで示されている次の例では、ブラウザーの開発者ツールで、ブラウザーのコンソール出力を調べてください。</span><span class="sxs-lookup"><span data-stu-id="8fe69-193">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="8fe69-194">ときに、**トリガー .NET インスタンス メソッド HelloHelper.SayHello**ボタンを選択すると、`ExampleJsInterop.CallHelloHelperSayHello`と呼びます。 名前を渡します`Blazor`、メソッドにします。</span><span class="sxs-lookup"><span data-stu-id="8fe69-194">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="8fe69-195">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8fe69-195">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop3&highlight=8-9)]

`CallHelloHelperSayHello` <span data-ttu-id="8fe69-196">JavaScript 関数を呼び出す`sayHello`の新しいインスタンスを使って`HelloHelper`します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-196">invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="8fe69-197">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="8fe69-197">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="8fe69-198">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="8fe69-198">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="8fe69-199">名前が渡される`HelloHelper`のコンス トラクターは、設定、`HelloHelper.Name`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="8fe69-199">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="8fe69-200">ときに、JavaScript 関数`sayHello`が実行される`HelloHelper.SayHello`を返します、`Hello, {Name}!`メッセージで、JavaScript 関数で、コンソールに書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="8fe69-200">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="8fe69-201">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="8fe69-201">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="8fe69-202">コンソールがブラウザーの web 開発者ツールで出力する場合:</span><span class="sxs-lookup"><span data-stu-id="8fe69-202">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a><span data-ttu-id="8fe69-203">Razor のコンポーネントのクラス ライブラリでの相互運用機能のコードを共有します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-203">Share interop code in a Razor Component class library</span></span>

<span data-ttu-id="8fe69-204">相互運用機能の JavaScript コードは Razor コンポーネントのクラス ライブラリに含めることができます (`dotnet new razorclasslib`)、NuGet パッケージのコードを共有できます。</span><span class="sxs-lookup"><span data-stu-id="8fe69-204">JavaScript interop code can be included in a Razor Component class library (`dotnet new razorclasslib`), which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="8fe69-205">Razor のコンポーネントのクラス ライブラリでは、ビルドされたアセンブリに埋め込みの JavaScript リソースを処理します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-205">The Razor Component class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="8fe69-206">JavaScript ファイルに配置されます、 *wwwroot*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="8fe69-206">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="8fe69-207">ツール、ライブラリのビルド時にリソースを埋め込む処理されます。</span><span class="sxs-lookup"><span data-stu-id="8fe69-207">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="8fe69-208">組み込みの NuGet パッケージは、通常、NuGet パッケージが参照されていると同様に、アプリのプロジェクト ファイルで参照されます。</span><span class="sxs-lookup"><span data-stu-id="8fe69-208">The built NuGet package is referenced in the project file of the app just as any normal NuGet package is referenced.</span></span> <span data-ttu-id="8fe69-209">場合と、アプリのコードを JavaScript を呼び出すことが、アプリを復元したらC#します。</span><span class="sxs-lookup"><span data-stu-id="8fe69-209">After the app is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="8fe69-210">詳細については、「 <xref:razor-components/class-libraries> 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8fe69-210">For more information, see <xref:razor-components/class-libraries>.</span></span>
