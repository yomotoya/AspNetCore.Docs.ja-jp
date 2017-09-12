---
title: "ASP.NET Core Knockout.js MVVM フレームワーク"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b20e3b23-1c51-47bf-adac-91b5048567e0
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/knockout
ms.openlocfilehash: d1c5cbd430587b757bb550f8f04355e67f04eb54
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a><span data-ttu-id="93aab-103">ASP.NET Core Knockout.js MVVM フレームワーク</span><span class="sxs-lookup"><span data-stu-id="93aab-103">Knockout.js MVVM Framework in ASP.NET Core</span></span>

<span data-ttu-id="93aab-104">によって[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="93aab-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="93aab-105">Knockout は、複雑なデータに基づくユーザー インターフェイスの作成を簡略化する一般的な JavaScript ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="93aab-105">Knockout is a popular JavaScript library that simplifies the creation of complex data-based user interfaces.</span></span> <span data-ttu-id="93aab-106">これは、単独で、または jQuery などその他のライブラリに使用できます。</span><span class="sxs-lookup"><span data-stu-id="93aab-106">It can be used alone or with other libraries, such as jQuery.</span></span> <span data-ttu-id="93aab-107">その主な目的は、UI に変更が加えられた、モデルが更新されるとするように、JavaScript オブジェクトとして定義されている基になるデータ モデルに UI 要素をバインドして、その逆です。</span><span class="sxs-lookup"><span data-stu-id="93aab-107">Its primary purpose is to bind UI elements to an underlying data model defined as a JavaScript object, such that when changes are made to the UI, the model is updated, and vice versa.</span></span> <span data-ttu-id="93aab-108">Knockout には、web アプリケーションのクライアント側の動作でモデル View-viewmodel (MVVM) パターンの使用が容易になります。</span><span class="sxs-lookup"><span data-stu-id="93aab-108">Knockout facilitates the use of a Model-View-ViewModel (MVVM) pattern in a web application's client-side behavior.</span></span> <span data-ttu-id="93aab-109">Knockout の MVVM 実装を使用するときに 1 つについて説明する必要があります、2 つの主要な概念は、観測可能なオブジェクトとバインドされます。</span><span class="sxs-lookup"><span data-stu-id="93aab-109">The two main concepts one must learn when working with Knockout's MVVM implementation are Observables and Bindings.</span></span>

## <a name="getting-started"></a><span data-ttu-id="93aab-110">作業の開始</span><span class="sxs-lookup"><span data-stu-id="93aab-110">Getting started</span></span>

<span data-ttu-id="93aab-111">インストールするため、1 つの JavaScript ファイルとして knockout が展開され、それを使用して、非常に簡単を使用して[bower](bower.md)です。</span><span class="sxs-lookup"><span data-stu-id="93aab-111">Knockout is deployed as a single JavaScript file, so installing and using it is very straightforward using [bower](bower.md).</span></span> <span data-ttu-id="93aab-112">既にあると仮定して[bower](bower.md)と[gulp](using-gulp.md)開くように構成、 *bower.json* ASP.NET Core プロジェクトし、次に示すように、knockout 依存関係を追加。</span><span class="sxs-lookup"><span data-stu-id="93aab-112">Assuming you already have [bower](bower.md) and [gulp](using-gulp.md) configured, open *bower.json* in your ASP.NET Core project and add the knockout dependency as shown here:</span></span>

```json
{
  "name": "KnockoutDemo",
  "private": true,
  "dependencies": {
    "knockout" : "^3.3.0"
  },
  "exportsOverride": {
  }
}
```

<span data-ttu-id="93aab-113">こうすると、使用することで、手動で (ビュー ‣ その他のウィンドウ ‣ タスク ランナー エクスプ ローラー) の下のタスク ランナー エクスプ ローラーを開くことで bower を実行し、bower を右クリックして タスク と実行 を選択します。</span><span class="sxs-lookup"><span data-stu-id="93aab-113">With this in place, you can then manually run bower by opening the Task Runner Explorer (under View ‣ Other Windows ‣ Task Runner Explorer) and then under Tasks, right-click on bower and select Run.</span></span> <span data-ttu-id="93aab-114">次のような結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="93aab-114">The result should appear similar to this:</span></span>

![タスク ランナー エクスプ ローラーで実行中の knockout を bower します。](knockout/_static/bower-knockout.png)

<span data-ttu-id="93aab-116">ここでは、プロジェクトの検索`wwwroot`フォルダー、knockout lib フォルダーの下にインストールを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="93aab-116">Now if you look in your project's `wwwroot` folder, you should see knockout installed under the lib folder.</span></span>

![knockout lib フォルダーにインストールされています。](knockout/_static/wwwroot-knockout.png)

<span data-ttu-id="93aab-118">ユーザーがファイルのキャッシュされたコピーが既に与えられてし、そのため、ダウンロードする必要はありません、その確率を増加させる、実稼働環境で knockout コンテンツ配信ネットワーク、または、CDN を使用してを参照することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="93aab-118">It's recommended that in your production environment you reference knockout via a Content Delivery Network, or CDN, as this increases the likelihood that your users will already have a cached copy of the file and thus will not need to download it at all.</span></span> <span data-ttu-id="93aab-119">Knockout は、ここで Microsoft Ajax CDN を含め、いくつかの Cdn で入手できます。</span><span class="sxs-lookup"><span data-stu-id="93aab-119">Knockout is available on several CDNs, including the Microsoft Ajax CDN, here:</span></span>

[<span data-ttu-id="93aab-120">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="93aab-120">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

<span data-ttu-id="93aab-121">ページに含める Knockout、それを使用する、単に追加、`<script>`任意の場所をホストすること (アプリケーションを使用して、または、CDN によって) からファイルを参照する要素。</span><span class="sxs-lookup"><span data-stu-id="93aab-121">To include Knockout on a page that will use it, simply add a `<script>` element referencing the file from wherever you will be hosting it (with your application, or via a CDN):</span></span>

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a><span data-ttu-id="93aab-122">観測可能なオブジェクト、ViewModels、および単純バインディング</span><span class="sxs-lookup"><span data-stu-id="93aab-122">Observables, ViewModels, and simple binding</span></span>

<span data-ttu-id="93aab-123">JavaScript を使用して、DOM に直接アクセスするか、web ページの要素を操作するや jQuery などのライブラリを使用してに習熟してあります。</span><span class="sxs-lookup"><span data-stu-id="93aab-123">You may already be familiar with using JavaScript to manipulate elements on a web page, either via direct access to the DOM or using a library like jQuery.</span></span> <span data-ttu-id="93aab-124">通常このような動作は、特定のユーザー アクションへの応答内の要素の値を直接設定するコードの記述によって実現されます。</span><span class="sxs-lookup"><span data-stu-id="93aab-124">Typically this kind of behavior is achieved by writing code to directly set element values in response to certain user actions.</span></span> <span data-ttu-id="93aab-125">使用すると Knockout、宣言型の方法は取得代わりに、使用するページ上の要素はプロパティにバインドされたオブジェクトの。</span><span class="sxs-lookup"><span data-stu-id="93aab-125">With Knockout, a declarative approach is taken instead, through which elements on the page are bound to properties on an object.</span></span> <span data-ttu-id="93aab-126">DOM 要素を操作するコードを記述する代わりにユーザーの操作は、単に ViewModel オブジェクトと対話し、Knockout はページの要素を同期させるに対処します。</span><span class="sxs-lookup"><span data-stu-id="93aab-126">Instead of writing code to manipulate DOM elements, user actions simply interact with the ViewModel object, and Knockout takes care of ensuring the page elements are synchronized.</span></span>

<span data-ttu-id="93aab-127">単純な例としては、次のページの一覧を検討してください。</span><span class="sxs-lookup"><span data-stu-id="93aab-127">As a simple example, consider the page list below.</span></span> <span data-ttu-id="93aab-128">含まれている、`<span>`を持つ要素を`data-bind`"作成者"テキストの内容をバインドすることを示す属性です。</span><span class="sxs-lookup"><span data-stu-id="93aab-128">It includes a `<span>` element with a `data-bind` attribute indicating that the text content should be bound to authorName.</span></span> <span data-ttu-id="93aab-129">次に、1 つのプロパティを持つ変数 viewModel が定義されている JavaScript ブロック`authorName`、いくつかの値に設定します。</span><span class="sxs-lookup"><span data-stu-id="93aab-129">Next, in a JavaScript block a variable viewModel is defined with a single property, `authorName`, set to some value.</span></span> <span data-ttu-id="93aab-130">呼び出しでは最後に、`ko.applyBindings`行われると、この viewModel 変数を渡します。</span><span class="sxs-lookup"><span data-stu-id="93aab-130">Finally, a call to `ko.applyBindings` is made, passing in this viewModel variable.</span></span>

```html
<html>
<head>
    <script type="text/javascript" src="lib/knockout/knockout.js"></script>
</head>
<body>
    <h1>Some Article</h1>
    <p>
        By <span data-bind="text: authorName"></span>
    </p>
    <script type="text/javascript">
      var viewModel = {
        authorName: 'Steve Smith'
      };
      ko.applyBindings(viewModel);
    </script>
</body>
</html>
```

<span data-ttu-id="93aab-131">コンテンツはブラウザーに表示されるときに、 <span> viewModel 変数に値を持つ要素が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="93aab-131">When viewed in the browser, the content of the <span> element is replaced with the value in the viewModel variable:</span></span>

![knockout 単純バインディング](knockout/_static/simple-binding-screenshot.png)

<span data-ttu-id="93aab-133">単純な一方向のバインディングの作業があるようになりました。</span><span class="sxs-lookup"><span data-stu-id="93aab-133">We now have simple one-way binding working.</span></span> <span data-ttu-id="93aab-134">ほど、コードででしたお書き込み値を割り当てようスパンのコンテンツに JavaScript に注意してください。</span><span class="sxs-lookup"><span data-stu-id="93aab-134">Notice that nowhere in the code did we write JavaScript to assign a value to the span's contents.</span></span> <span data-ttu-id="93aab-135">ViewModel を操作する場合、ステップを実行するこの、さらに、HTML 入力用テキスト ボックスを追加したりできますと同様に、その値にバインドようにします。</span><span class="sxs-lookup"><span data-stu-id="93aab-135">If we want to manipulate the ViewModel, we can take this a step further and add an HTML input textbox, and bind to its value, like so:</span></span>

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

<span data-ttu-id="93aab-136">ページを再度読み込んでを参照して、入力ボックスにこの値が実際にバインドされています。</span><span class="sxs-lookup"><span data-stu-id="93aab-136">Reloading the page, we see that this value is indeed bound to the input box:</span></span>

![knockout 入力バインディング](knockout/_static/input-binding-screenshot.png)

<span data-ttu-id="93aab-138">ただし、textbox 内の値を変更すると場合の対応する値で、`<span>`要素は変更されません。</span><span class="sxs-lookup"><span data-stu-id="93aab-138">However, if we change the value in the textbox, the corresponding value in the `<span>` element doesn't change.</span></span> <span data-ttu-id="93aab-139">なぜですか。</span><span class="sxs-lookup"><span data-stu-id="93aab-139">Why not?</span></span>

<span data-ttu-id="93aab-140">問題が何も通知を受け取ること、`<span>`を更新するために必要です。</span><span class="sxs-lookup"><span data-stu-id="93aab-140">The issue is that nothing notified the `<span>` that it needed to be updated.</span></span> <span data-ttu-id="93aab-141">ViewModel のプロパティは、特殊な種類でラップされていない限り、不十分な場合にそれ自体では、ViewModel を更新するだけです。</span><span class="sxs-lookup"><span data-stu-id="93aab-141">Simply updating the ViewModel isn't by itself sufficient, unless the ViewModel's properties are wrapped in a special type.</span></span> <span data-ttu-id="93aab-142">使用する必要があります**オブサーバー**で行った変更を発生するが自動的に更新する必要があるプロパティに対して ViewModel です。</span><span class="sxs-lookup"><span data-stu-id="93aab-142">We need to use **observables** in the ViewModel for any properties that need to have changes automatically updated as they occur.</span></span> <span data-ttu-id="93aab-143">使用する ViewModel を変更することによって`ko.observable("value")`ViewModel だけ「値」ではなく、変更が発生するたびに、その値にバインドされている HTML 要素が更新されます。</span><span class="sxs-lookup"><span data-stu-id="93aab-143">By changing the ViewModel to use `ko.observable("value")` instead of just "value", the ViewModel will update any HTML elements that are bound to its value whenever a change occurs.</span></span> <span data-ttu-id="93aab-144">入力すると、バインド要素への変更は表示されませんのでにフォーカスが失われますまでに、入力ボックスがその値を更新しないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="93aab-144">Note that input boxes don't update their value until they lose focus, so you won't see changes to bound elements as you type.</span></span>

> [!NOTE]
> <span data-ttu-id="93aab-145">キーを押すたびに追加するだけの後に、ライブ更新のサポートを追加する`valueUpdate: "afterkeydown"`を`data-bind`属性の内容。</span><span class="sxs-lookup"><span data-stu-id="93aab-145">Adding support for live updating after each keypress is simply a matter of adding `valueUpdate: "afterkeydown"` to the `data-bind` attribute's contents.</span></span> <span data-ttu-id="93aab-146">使用して、この動作を取得することもできます。`data-bind="textInput: authorName"`値のインスタント更新プログラムを取得します。</span><span class="sxs-lookup"><span data-stu-id="93aab-146">You can also get this behavior by using `data-bind="textInput: authorName"` to get instant updates of values.</span></span> 

<span data-ttu-id="93aab-147">当社 viewModel、ko.observable を使用するように更新した後:</span><span class="sxs-lookup"><span data-stu-id="93aab-147">Our viewModel, after updating it to use ko.observable:</span></span>

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="93aab-148">Knockout には、さまざまな種類のバインドの数がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="93aab-148">Knockout supports a number of different kinds of bindings.</span></span> <span data-ttu-id="93aab-149">バインドする方法を見たところ`text`にされ、`value`です。</span><span class="sxs-lookup"><span data-stu-id="93aab-149">So far we've seen how to bind to `text` and to `value`.</span></span> <span data-ttu-id="93aab-150">指定された属性にバインドすることもできます。</span><span class="sxs-lookup"><span data-stu-id="93aab-150">You can also bind to any given attribute.</span></span> <span data-ttu-id="93aab-151">たとえば、アンカー タグを含むハイパーリンクを作成する、 `src` viewModel に属性をバインドできます。</span><span class="sxs-lookup"><span data-stu-id="93aab-151">For instance, to create a hyperlink with an anchor tag, the `src` attribute can be bound to the viewModel.</span></span> <span data-ttu-id="93aab-152">Knockout には、関数へのバインドもサポートしています。</span><span class="sxs-lookup"><span data-stu-id="93aab-152">Knockout also supports binding to functions.</span></span> <span data-ttu-id="93aab-153">これを示すためには、作成者の twitter のハンドルを含める viewModel を更新して、著者の twitter ページへのリンクとして twitter のハンドルを表示してみましょう。</span><span class="sxs-lookup"><span data-stu-id="93aab-153">To demonstrate this, let's update the viewModel to include the author's twitter handle, and display the twitter handle as a link to the author's twitter page.</span></span> <span data-ttu-id="93aab-154">この 3 つの段階で実行します。</span><span class="sxs-lookup"><span data-stu-id="93aab-154">We'll do this in three stages.</span></span>

<span data-ttu-id="93aab-155">まず、著者の名の後にかっこで紹介するハイパーリンクを表示する HTML を追加します。</span><span class="sxs-lookup"><span data-stu-id="93aab-155">First, add the HTML to display the hyperlink, which we'll show in parentheses after the author's name:</span></span>

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

<span data-ttu-id="93aab-156">TwitterUrl と twitterAlias プロパティを含める viewModel を次に、更新するには。</span><span class="sxs-lookup"><span data-stu-id="93aab-156">Next, update the viewModel to include the twitterUrl and twitterAlias properties:</span></span>

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith'),
  twitterAlias: ko.observable('@ardalis'),
  twitterUrl: ko.computed(function() {
    return "https://twitter.com/";
  }, this)
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="93aab-157">まだしていないこの twitter エイリアスの正しい URL に移動する twitterUrl 更新されましたおこの時点で: twitter.com でが指してだけのことに注意してください。また、新しい Knockout 関数を使用することに注意してください`computed`twitterUrl 用です。</span><span class="sxs-lookup"><span data-stu-id="93aab-157">Notice that at this point we haven't yet updated the twitterUrl to go to the correct URL for this twitter alias – it's just pointing at twitter.com. Also notice that we're using a new Knockout function, `computed`, for twitterUrl.</span></span> <span data-ttu-id="93aab-158">これは、監視可能な関数が変更された場合、UI 要素を通知します。</span><span class="sxs-lookup"><span data-stu-id="93aab-158">This is an observable function that will notify any UI elements if it changes.</span></span> <span data-ttu-id="93aab-159">ただし、viewModel であるその他のプロパティにアクセスするため、独自のステートメントの各プロパティはできるように、viewModel は作成する方法を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="93aab-159">However, for it to have access to other properties in the viewModel, we need to change how we are creating the viewModel, so that each property is its own statement.</span></span>

<span data-ttu-id="93aab-160">改訂 viewModel 宣言を次に示します。</span><span class="sxs-lookup"><span data-stu-id="93aab-160">The revised viewModel declaration is shown below.</span></span> <span data-ttu-id="93aab-161">これで、関数として宣言されています。</span><span class="sxs-lookup"><span data-stu-id="93aab-161">It is now declared as a function.</span></span> <span data-ttu-id="93aab-162">各プロパティが独自ステートメントであるようになりましたが、セミコロンで終わることを確認します。</span><span class="sxs-lookup"><span data-stu-id="93aab-162">Notice that each property is its own statement now, ending with a semicolon.</span></span> <span data-ttu-id="93aab-163">TwitterAlias プロパティの値にアクセスする必要があるため、その参照には、() が含まれています。 を実行することを確認します。</span><span class="sxs-lookup"><span data-stu-id="93aab-163">Also notice that to access the twitterAlias property value, we need to execute it, so its reference includes ().</span></span>

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this)
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="93aab-164">結果は、ブラウザーで想定どおりに動作します。</span><span class="sxs-lookup"><span data-stu-id="93aab-164">The result works as expected in the browser:</span></span>

![knockout ハイパーリンク](knockout/_static/hyperlink-screenshot.png)

<span data-ttu-id="93aab-166">Knockout には、クリック イベントなど、特定の UI 要素イベントへのバインドもサポートしています。</span><span class="sxs-lookup"><span data-stu-id="93aab-166">Knockout also supports binding to certain UI element events, such as the click event.</span></span> <span data-ttu-id="93aab-167">これにより、簡単かつ宣言によって UI 要素をアプリケーションの viewModel 内の関数にバインドできます。</span><span class="sxs-lookup"><span data-stu-id="93aab-167">This allows you to easily and declaratively bind UI elements to functions within the application's viewModel.</span></span> <span data-ttu-id="93aab-168">単純な例としては、ボタンを追加できますをクリックすると、すべて大文字である、著者の twitterAlias を変更します。</span><span class="sxs-lookup"><span data-stu-id="93aab-168">As a simple example, we can add a button that, when clicked, modifies the author's twitterAlias to be all caps.</span></span>

<span data-ttu-id="93aab-169">最初に、ボタンを追加、ボタンへのバインドは、イベント、およびは viewModel に追加することに関数名を参照する をクリックします。</span><span class="sxs-lookup"><span data-stu-id="93aab-169">First, we add the button, binding to the button's click event, and referencing the function name we're going to add to the viewModel:</span></span>

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

<span data-ttu-id="93aab-170">関数、viewModel を追加し、viewModel の状態を変更するに接続します。</span><span class="sxs-lookup"><span data-stu-id="93aab-170">Then, add the function to the viewModel, and wire it up to modify the viewModel's state.</span></span> <span data-ttu-id="93aab-171">TwitterAlias プロパティに新しい値を設定するにはメソッドとして呼び出すし、新しい値で渡すことがわかります。</span><span class="sxs-lookup"><span data-stu-id="93aab-171">Notice that to set a new value to the twitterAlias property, we call it as a method and pass in the new value.</span></span>

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this);

  this.capitalizeTwitterAlias = function() {
    var currentValue = this.twitterAlias();
    this.twitterAlias(currentValue.toUpperCase());
  }
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="93aab-172">コードを実行して、ボタンをクリックするとは、期待どおりに表示されたリンクを変更します。</span><span class="sxs-lookup"><span data-stu-id="93aab-172">Running the code and clicking the button modifies the displayed link as expected:</span></span>

![ハイパーリンクを大文字にします。](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a><span data-ttu-id="93aab-174">制御フロー</span><span class="sxs-lookup"><span data-stu-id="93aab-174">Control flow</span></span>

<span data-ttu-id="93aab-175">Knockout には、条件とループ操作を実行できるバインディングが含まれています。</span><span class="sxs-lookup"><span data-stu-id="93aab-175">Knockout includes bindings that can perform conditional and looping operations.</span></span> <span data-ttu-id="93aab-176">ループ操作は、データのリストを UI 一覧、メニューのおよびグリッドやテーブルにバインドするため便利です。</span><span class="sxs-lookup"><span data-stu-id="93aab-176">Looping operations are especially useful for binding lists of data to UI lists, menus, and grids or tables.</span></span> <span data-ttu-id="93aab-177">Foreach バインディングは、配列を反復処理されます。</span><span class="sxs-lookup"><span data-stu-id="93aab-177">The foreach binding will iterate over an array.</span></span> <span data-ttu-id="93aab-178">監視可能な配列を併用すると、項目の追加または UI ツリー内のすべての要素を再作成せずに、配列から削除する場合が UI 要素に自動的に更新されます。</span><span class="sxs-lookup"><span data-stu-id="93aab-178">When used with an observable array, it will automatically update the UI elements when items are added or removed from the array, without re-creating every element in the UI tree.</span></span> <span data-ttu-id="93aab-179">次の例では、ゲームの結果の監視可能な配列を含む新しい viewModel を使用します。</span><span class="sxs-lookup"><span data-stu-id="93aab-179">The following example uses a new viewModel which includes an observable array of game results.</span></span> <span data-ttu-id="93aab-180">使用して 2 つの列を含む、単純なテーブルにバインドされている、`foreach`上のバインド、`<tbody>`要素。</span><span class="sxs-lookup"><span data-stu-id="93aab-180">It is bound to a simple table with two columns using a `foreach` binding on the `<tbody>` element.</span></span> <span data-ttu-id="93aab-181">各`<tr>`内の要素`<tbody>`gameResults コレクションの要素にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="93aab-181">Each `<tr>` element within `<tbody>` will be bound to an element of the gameResults collection.</span></span>

```html
<h1>Record</h1>
<table>
    <thead>
        <tr>
            <th>Opponent</th>
            <th>Result</th>
        </tr>
    </thead>
    <tbody data-bind="foreach: gameResults">
        <tr>
            <td data-bind="text:opponent"></td>
            <td data-bind="text:result"></td>
        </tr>
    </tbody>
</table>
<script type="text/javascript">
  function GameResult(opponent, result) {
    var self = this;
    self.opponent = opponent;
    self.result = ko.observable(result);
  }

  function ViewModel() {
    var self = this;

    self.resultChoices = ["Win", "Loss", "Tie"];

    self.gameResults = ko.observableArray([
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Michelle", self.resultChoices[1])
    ]);
  };
  ko.applyBindings(new ViewModel);
</script>
```

<span data-ttu-id="93aab-182">この時間を使用すること ViewModel 大文字の"V"(applyBindings 呼び出し) で「新規」を使用して構築するためになる予定のために注意してください。</span><span class="sxs-lookup"><span data-stu-id="93aab-182">Notice that this time we're using ViewModel with a capital “V" because we expect to construct it using “new" (in the applyBindings call).</span></span> <span data-ttu-id="93aab-183">実行すると、ページは、次の出力が得られます。</span><span class="sxs-lookup"><span data-stu-id="93aab-183">When executed, the page results in the following output:</span></span>

![knockout レコード ビュー モデル](knockout/_static/record-screenshot.png)

<span data-ttu-id="93aab-185">監視可能なコレクションが動作していることを示すためには、少し多くの機能を追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="93aab-185">To demonstrate that the observable collection is working, let's add a bit more functionality.</span></span> <span data-ttu-id="93aab-186">ViewModel するもう 1 つのゲームの結果を記録する機能を含めるし、ボタンと新しい関数を使用する一部の UI を追加おことができます。</span><span class="sxs-lookup"><span data-stu-id="93aab-186">We can include the ability to record the results of another game to the ViewModel, and then add a button and some UI to work with this new function.</span></span>  <span data-ttu-id="93aab-187">まず、addResult メソッドを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="93aab-187">First, let's create the addResult method:</span></span>

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

<span data-ttu-id="93aab-188">このメソッドにバインドを使用して、ボタン、`click`バインド。</span><span class="sxs-lookup"><span data-stu-id="93aab-188">Bind this method to a button using the `click` binding:</span></span>

```html
<button data-bind="click: addResult">Add New Result</button>
```

<span data-ttu-id="93aab-189">ブラウザーで、ページを開き をクリックした回数、いくつかクリックするたびに新しいテーブルの行の結果として得られます。</span><span class="sxs-lookup"><span data-stu-id="93aab-189">Open the page in the browser and click the button a couple of times, resulting in a new table row with each click:</span></span>

![結果を追加します。](knockout/_static/record-addresult-screenshot.png)

<span data-ttu-id="93aab-191">UI では、通常、インライン、または別のフォームで新しいレコードの追加をサポートするために、いくつかの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="93aab-191">There are a few ways to support adding new records in the UI, typically either inline or in a separate form.</span></span> <span data-ttu-id="93aab-192">全体が編集可能になるように、テキスト ボックスおよび dropdownlists を使用するテーブルに簡単に変更できます。</span><span class="sxs-lookup"><span data-stu-id="93aab-192">We can easily modify the table to use textboxes and dropdownlists so that the whole thing is editable.</span></span> <span data-ttu-id="93aab-193">変更するだけ、`<tr>`に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="93aab-193">Just change the `<tr>` element as shown:</span></span>

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

<span data-ttu-id="93aab-194">なお`$root`可能な選択肢が公開される場所はルート ViewModel を参照します。</span><span class="sxs-lookup"><span data-stu-id="93aab-194">Note that `$root` refers to the root ViewModel, which is where the possible choices are exposed.</span></span> <span data-ttu-id="93aab-195">`$data`この場合は、単純な文字列をそれぞれが、resultChoices 配列の個々 の要素を参照して、特定のコンテキスト内では、どのような現在のモデルを参照します。</span><span class="sxs-lookup"><span data-stu-id="93aab-195">`$data` refers to whatever the current model is within a given context - in this case it refers to an individual element of the resultChoices array, each of which is a simple string.</span></span>

<span data-ttu-id="93aab-196">この変更により、グリッド全体は編集可能になります。</span><span class="sxs-lookup"><span data-stu-id="93aab-196">With this change, the entire grid becomes editable:</span></span>

![編集可能なグリッド](knockout/_static/editable-grid-screenshot.png)

<span data-ttu-id="93aab-198">Knockout を使用していないこと、おを得ることがこれはすべて、jQuery の使用が、ほとんどの場合とないほぼ同程度に効率的です。</span><span class="sxs-lookup"><span data-stu-id="93aab-198">If we weren't using Knockout, we could achieve all of this using jQuery, but most likely it would not be nearly as efficient.</span></span> <span data-ttu-id="93aab-199">Knockout は、どの UI 要素に、ViewModel 内の項目の対応するバインドされたデータを追跡し、追加、削除、または更新する必要があるこれらの要素のみが更新します。</span><span class="sxs-lookup"><span data-stu-id="93aab-199">Knockout tracks which bound data items in the ViewModel correspond to which UI elements, and only updates those elements that need to be added, removed, or updated.</span></span> <span data-ttu-id="93aab-200">これを実現する jQuery または直接の DOM 操作を使用して社内で大幅な残存作業時間がかかるし、場合でも、テーブルのデータに基づく (win 損失レコードなど) の集計結果を表示する必要は必要がありますをもう一度ループ処理、および解析する、HTML 要素です。</span><span class="sxs-lookup"><span data-stu-id="93aab-200">It would take significant effort to achieve this ourselves using jQuery or direct DOM manipulation, and even then if we then wanted to display aggregate results (such as a win-loss record) based on the table's data, we would need to once more loop through it and parse the HTML elements.</span></span>  <span data-ttu-id="93aab-201">Knockout で win が失われるレコードの表示は簡単です。</span><span class="sxs-lookup"><span data-stu-id="93aab-201">With Knockout, displaying the win-loss record is trivial.</span></span> <span data-ttu-id="93aab-202">ViewModel 自体内で計算を実行し、単純なテキストのバインディングでそれを表示し、`<span>`です。</span><span class="sxs-lookup"><span data-stu-id="93aab-202">We can perform the calculations within the ViewModel itself, and then display it with a simple text binding and a `<span>`.</span></span>

<span data-ttu-id="93aab-203">Win が失われるレコードの文字列を構築するには、計算観測可能なオブジェクトを使用できます。</span><span class="sxs-lookup"><span data-stu-id="93aab-203">To build the win-loss record string, we can use a computed observable.</span></span> <span data-ttu-id="93aab-204">関数呼び出しをする必要があります、ViewModel 内で監視可能なプロパティへの参照をそれ以外の場合は取得しません、観測可能なオブジェクトの値 (つまり`gameResults()`いない`gameResults`に示すコードで)。</span><span class="sxs-lookup"><span data-stu-id="93aab-204">Note that references to observable properties within the ViewModel must be function calls, otherwise they will not retrieve the value of the observable (i.e. `gameResults()` not `gameResults` in the code shown):</span></span>

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

<span data-ttu-id="93aab-205">この関数をバインド内の範囲を`<h1>`ページの上部にある要素。</span><span class="sxs-lookup"><span data-stu-id="93aab-205">Bind this function to a span within the `<h1>` element at the top of the page:</span></span>

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

<span data-ttu-id="93aab-206">結果:</span><span class="sxs-lookup"><span data-stu-id="93aab-206">The result:</span></span>

![Win 損失](knockout/_static/record-winloss-screenshot.png)

<span data-ttu-id="93aab-208">行を追加または任意の行の結果の列で選択した要素を変更するには、ウィンドウの上部に表示レコードが更新されます。</span><span class="sxs-lookup"><span data-stu-id="93aab-208">Adding rows or modifying the selected element in any row's Result column will update the record shown at the top of the window.</span></span>

<span data-ttu-id="93aab-209">値へのバインド、に加えて、バインディング内の法的なほとんどの JavaScript 式を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="93aab-209">In addition to binding to values, you can also use almost any legal JavaScript expression within a binding.</span></span> <span data-ttu-id="93aab-210">たとえば場合は、UI 要素に値が特定のしきい値を超えた場合など、特定の条件下でのみ表示されますできますこの論理的に内で指定したバインディング式。</span><span class="sxs-lookup"><span data-stu-id="93aab-210">For example, if a UI element should only appear under certain conditions, such as when a value exceeds a certain threshold, you can specify this logically within the binding expression:</span></span>

```html
<div data-bind="visible: customerValue > 100"></div>
```

<span data-ttu-id="93aab-211">これは、`<div>`のみが表示されます、customerValue が 100 以上である場合。</span><span class="sxs-lookup"><span data-stu-id="93aab-211">This `<div>` will only be visible when the customerValue is over 100.</span></span>

## <a name="templates"></a><span data-ttu-id="93aab-212">テンプレート</span><span class="sxs-lookup"><span data-stu-id="93aab-212">Templates</span></span>

<span data-ttu-id="93aab-213">Knockout ではの動作と UI を簡単に分離したり、オンデマンドでの大規模なアプリケーションへの UI 要素の増分読み込みできるように、テンプレートのサポートがあります。</span><span class="sxs-lookup"><span data-stu-id="93aab-213">Knockout has support for templates, so that you can easily separate your UI from your behavior, or incrementally load UI elements into a large application on demand.</span></span> <span data-ttu-id="93aab-214">前の例に各行独自のテンプレートをテンプレートに HTML をアウト プルでデータ バインドの呼び出しで名前によって、テンプレートを指定するだけで更新することが`<tbody>`です。</span><span class="sxs-lookup"><span data-stu-id="93aab-214">We can update our previous example to make each row its own template by simply pulling the HTML out into a template and specifying the template by name in the data-bind call on `<tbody>`.</span></span>

```html
<tbody data-bind="template: { name: 'rowTemplate', foreach: gameResults }">
</tbody>
<script type="text/html" id="rowTemplate">
  <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
  </tr>
</script>
```

<span data-ttu-id="93aab-215">Knockout には、jQuery.tmpl ライブラリなど、他のテンプレート エンジンと Underscore.js のテンプレート エンジンもサポートしています。</span><span class="sxs-lookup"><span data-stu-id="93aab-215">Knockout also supports other templating engines, such as the jQuery.tmpl library and Underscore.js's templating engine.</span></span>

## <a name="components"></a><span data-ttu-id="93aab-216">コンポーネント</span><span class="sxs-lookup"><span data-stu-id="93aab-216">Components</span></span>

<span data-ttu-id="93aab-217">コンポーネントを使用すると、整理し、UI コードが依存している ViewModel データと共に通常の UI コードを再利用できます。</span><span class="sxs-lookup"><span data-stu-id="93aab-217">Components allow you to organize and reuse UI code, usually along with the ViewModel data on which the UI code depends.</span></span> <span data-ttu-id="93aab-218">コンポーネントを作成するには、単にそのテンプレートとその viewModel 指定し、名前を付けます必要があります。</span><span class="sxs-lookup"><span data-stu-id="93aab-218">To create a component, you simply need to specify its template and its viewModel, and give it a name.</span></span> <span data-ttu-id="93aab-219">これは、`ko.components.register()` を呼び出すことによって行われます。</span><span class="sxs-lookup"><span data-stu-id="93aab-219">This is done by calling `ko.components.register()`.</span></span> <span data-ttu-id="93aab-220">テンプレートおよび viewmodel インラインを定義するだけでなく読み込むことができるようにライブラリを使用して外部のファイルから*require.js*、その結果に非常にクリーンで効率的なコードです。</span><span class="sxs-lookup"><span data-stu-id="93aab-220">In addition to defining the templates and viewmodel inline, they can be loaded from external files using a library like *require.js*, resulting in very clean and efficient code.</span></span>

## <a name="communicating-with-apis"></a><span data-ttu-id="93aab-221">Api との通信</span><span class="sxs-lookup"><span data-stu-id="93aab-221">Communicating with APIs</span></span>

<span data-ttu-id="93aab-222">Knockout は JSON 形式でデータを操作できます。</span><span class="sxs-lookup"><span data-stu-id="93aab-222">Knockout can work with any data in JSON format.</span></span> <span data-ttu-id="93aab-223">取得し、Knockout を使用してデータを保存するための一般的な方法は、jQuery をサポートするには、`$.getJSON()`データを取得する関数、および`$.post()`API エンドポイントにブラウザーからデータを送信する方法です。</span><span class="sxs-lookup"><span data-stu-id="93aab-223">A common way to retrieve and save data using Knockout is with jQuery, which supports the `$.getJSON()` function to retrieve data, and the `$.post()` method to send data from the browser to an API endpoint.</span></span> <span data-ttu-id="93aab-224">もちろん、別の方法を JSON データを送受信する場合は、Knockout しても同様に機能します。</span><span class="sxs-lookup"><span data-stu-id="93aab-224">Of course, if you prefer a different way to send and receive JSON data, Knockout will work with it as well.</span></span>

## <a name="summary"></a><span data-ttu-id="93aab-225">概要</span><span class="sxs-lookup"><span data-stu-id="93aab-225">Summary</span></span>

<span data-ttu-id="93aab-226">Knockout、ViewModel で定義されている、クライアント アプリケーションの現在の状態に UI 要素をバインドする単純で簡潔な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="93aab-226">Knockout provides a simple, elegant way to bind UI elements to the current state of the client application, defined in a ViewModel.</span></span> <span data-ttu-id="93aab-227">Knockout のバインディングの構文は、処理するのには、HTML 要素に適用される、データ バインド属性を使用します。</span><span class="sxs-lookup"><span data-stu-id="93aab-227">Knockout's binding syntax uses the data-bind attribute, applied to HTML elements that are to be processed.</span></span> <span data-ttu-id="93aab-228">Knockout が効率的にレンダリングして、UI 要素を追跡することによって大規模なデータ セットを更新して要素の影響を受けるのみへの変更を処理します。</span><span class="sxs-lookup"><span data-stu-id="93aab-228">Knockout is able to efficiently render and update large data sets by tracking UI elements and only processing changes to affected elements.</span></span> <span data-ttu-id="93aab-229">大規模なアプリケーションは、テンプレートとコンポーネントは、外部ファイルからの要求時に読み込むことができますが使用する UI ロジックを分割できます。</span><span class="sxs-lookup"><span data-stu-id="93aab-229">Large applications can break up UI logic using templates and components, which can be loaded on demand from external files.</span></span> <span data-ttu-id="93aab-230">現在のバージョン 3 Knockout は、安定した JavaScript ライブラリをリッチ クライアントの対話機能を必要とする web アプリケーションを向上させることができます。</span><span class="sxs-lookup"><span data-stu-id="93aab-230">Currently version 3, Knockout is a stable JavaScript library that can improve web applications that require rich client interactivity.</span></span>
