---
title: "単一ページ アプリケーション (SPAs) に AngularJS を使用します。"
author: rick-anderson
description: "AngularJS を使用して SPA スタイルの ASP.NET アプリケーションをビルドする方法をについてください。"
keywords: "ASP.NET Core、AngularJS、SPA"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/angular
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ccdf1625cdaf2400780500ac5ab86f41537964a9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a><span data-ttu-id="fd2de-104">AngularJS を使用して ASP.NET Core を使用する単一ページ アプリケーション (SPAs)</span><span class="sxs-lookup"><span data-stu-id="fd2de-104">Using AngularJS for Single Page Applications (SPAs) with ASP.NET Core</span></span>


<span data-ttu-id="fd2de-105">によって[Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)と[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="fd2de-105">By [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="fd2de-106">この記事では、AngularJS を使用して SPA スタイルの ASP.NET アプリケーションをビルドする方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-106">In this article, you will learn how to build a SPA-style ASP.NET application using AngularJS.</span></span>

<span data-ttu-id="fd2de-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="fd2de-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-angularjs"></a><span data-ttu-id="fd2de-108">AngularJS とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="fd2de-108">What is AngularJS?</span></span>

<span data-ttu-id="fd2de-109">[AngularJS](https://angularjs.org/)シングル ページ アプリケーション (SPAs) 操作に使用される一般的な Google からの最新の JavaScript フレームワークがします。</span><span class="sxs-lookup"><span data-stu-id="fd2de-109">[AngularJS](https://angularjs.org/) is a modern JavaScript framework from Google commonly used to work with Single Page Applications (SPAs).</span></span> <span data-ttu-id="fd2de-110">AngularJS MIT ライセンスの下でソースに、AngularJS の開発の進行状況を後に[、GitHub リポジトリ](https://github.com/angular/angular.js)です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-110">AngularJS is open sourced under MIT license, and the development progress of AngularJS can be followed on [its GitHub repository](https://github.com/angular/angular.js).</span></span> <span data-ttu-id="fd2de-111">ライブラリは HTML の形の角かっこを使用しているために、角速度と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-111">The library is called Angular because HTML uses angular-shaped brackets.</span></span>

<span data-ttu-id="fd2de-112">AngularJS は jQuery などの DOM 操作ライブラリではありませんが、jQLite と呼ばれる jQuery のサブセットを使用しています。</span><span class="sxs-lookup"><span data-stu-id="fd2de-112">AngularJS is not a DOM manipulation library like jQuery, but it uses a subset of jQuery called jQLite.</span></span> <span data-ttu-id="fd2de-113">AngularJS は、HTML タグを追加できますされる宣言型の HTML 属性に、主に基づいています。</span><span class="sxs-lookup"><span data-stu-id="fd2de-113">AngularJS is primarily based on declarative HTML attributes that you can add to your HTML tags.</span></span> <span data-ttu-id="fd2de-114">使用して、ブラウザーに AngularJS を試みることができます、[コード学校 web サイト](https://www.codeschool.com/courses/shaping-up-with-angularjs)または[W3Schools web サイト](https://www.w3schools.com/angular/)です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-114">You can try AngularJS in your browser using the [Code School website](https://www.codeschool.com/courses/shaping-up-with-angularjs) or  [W3Schools website](https://www.w3schools.com/angular/).</span></span>

<span data-ttu-id="fd2de-115">この記事では、角の見出しで ノートのいくつかの AngularJS について説明します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-115">This article focuses on AngularJS with some notes on where Angular is heading.</span></span>

## <a name="getting-started"></a><span data-ttu-id="fd2de-116">作業の開始</span><span class="sxs-lookup"><span data-stu-id="fd2de-116">Getting started</span></span>

<span data-ttu-id="fd2de-117">AngularJS を使用してを ASP.NET アプリケーションを開始するには、プロジェクトの一部としてインストールか、コンテンツ配信ネットワーク (CDN) から参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd2de-117">To start using AngularJS in your ASP.NET application, you must either install it as part of your project, or reference it from a content delivery network (CDN).</span></span>

### <a name="installation"></a><span data-ttu-id="fd2de-118">インストール</span><span class="sxs-lookup"><span data-stu-id="fd2de-118">Installation</span></span>

<span data-ttu-id="fd2de-119">アプリケーションに AngularJS を追加するいくつかの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="fd2de-119">There are several ways to add AngularJS to your application.</span></span> <span data-ttu-id="fd2de-120">Visual Studio で新しい ASP.NET Core web アプリケーションを開始している場合、は、組み込みを使用して AngularJS を追加することができます[Bower](bower.md)をサポートします。</span><span class="sxs-lookup"><span data-stu-id="fd2de-120">If you’re starting a new ASP.NET Core web application in Visual Studio, you can add AngularJS using the built-in [Bower](bower.md) support.</span></span> <span data-ttu-id="fd2de-121">開いている*bower.json*、エントリを追加し、`dependencies`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="fd2de-121">Open *bower.json*, and add an entry to the `dependencies` property:</span></span>

<a name="angular-bower-json"></a>

[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]

<span data-ttu-id="fd2de-122">保存時に、 *bower.json*ファイル、角をプロジェクトのインストールは*wwwroot/lib*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="fd2de-122">Upon saving the *bower.json* file, Angular will be installed in your project's *wwwroot/lib* folder.</span></span> <span data-ttu-id="fd2de-123">さらに、それが内にリストアップされます、`Dependencies/Bower`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="fd2de-123">Additionally, it will be listed within the `Dependencies/Bower` folder.</span></span> <span data-ttu-id="fd2de-124">次のスクリーン ショットを参照してください。</span><span class="sxs-lookup"><span data-stu-id="fd2de-124">See the screenshot below.</span></span>

![AngularJS プロジェクト ソリューション エクスプ ローラー](angular/_static/angular-solution-explorer.png)

<span data-ttu-id="fd2de-126">次に、追加、`<script>`の一番下への参照、 `<body>` HTML ページのセクションまたは*_Layout.cshtml*次に示すように、ファイルします。</span><span class="sxs-lookup"><span data-stu-id="fd2de-126">Next, add a `<script>` reference to the bottom of the `<body>` section of your HTML page or *_Layout.cshtml* file, as shown here:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]

<span data-ttu-id="fd2de-127">実稼働アプリケーションで Cdn を利用して、AngularJS のような一般的なライブラリのことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="fd2de-127">It's recommended that production applications utilize CDNs for common libraries like AngularJS.</span></span> <span data-ttu-id="fd2de-128">AngularJS は、この 1 など、いくつかの Cdn のいずれかから参照できます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-128">You can reference AngularJS from one of several CDNs, such as this one:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]

<span data-ttu-id="fd2de-129">参照を作成したら、 *angular.js* web ページに AngularJS を使用して作業を開始する準備ができたら、スクリプト ファイル。</span><span class="sxs-lookup"><span data-stu-id="fd2de-129">Once you have a reference to the *angular.js* script file, you're ready to begin using AngularJS in your web pages.</span></span>

## <a name="key-components"></a><span data-ttu-id="fd2de-130">主要なコンポーネント</span><span class="sxs-lookup"><span data-stu-id="fd2de-130">Key components</span></span>

<span data-ttu-id="fd2de-131">AngularJS などの主要なコンポーネントの数を含む*ディレクティブ*、*テンプレート*、*リピータ*、*モジュール*、 *コント ローラー*、*コンポーネント*、*コンポーネント ルーター*などです。</span><span class="sxs-lookup"><span data-stu-id="fd2de-131">AngularJS includes a number of major components, such as *directives*, *templates*, *repeaters*, *modules*, *controllers*, *components*, *component router* and more.</span></span> <span data-ttu-id="fd2de-132">これらのコンポーネント連携するしくみに、web ページの動作を追加するかを調べてみましょう。</span><span class="sxs-lookup"><span data-stu-id="fd2de-132">Let's examine how these components work together to add behavior to your web pages.</span></span>

### <a name="directives"></a><span data-ttu-id="fd2de-133">ディレクティブ</span><span class="sxs-lookup"><span data-stu-id="fd2de-133">Directives</span></span>

<span data-ttu-id="fd2de-134">AngularJS を使用して[ディレクティブ](https://docs.angularjs.org/guide/directive)のカスタム属性と要素の HTML を拡張します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-134">AngularJS uses [directives](https://docs.angularjs.org/guide/directive) to extend HTML with custom attributes and elements.</span></span> <span data-ttu-id="fd2de-135">AngularJS ディレクティブが経由で定義されている`data-ng-*`または`ng-*`プレフィックス (`ng`の短縮形は、角運動)。</span><span class="sxs-lookup"><span data-stu-id="fd2de-135">AngularJS directives are defined via `data-ng-*` or `ng-*` prefixes (`ng` is short for angular).</span></span> <span data-ttu-id="fd2de-136">AngularJS ディレクティブの 2 つの種類があります。</span><span class="sxs-lookup"><span data-stu-id="fd2de-136">There are two types of AngularJS directives:</span></span>

   1. <span data-ttu-id="fd2de-137">**プリミティブ ディレクティブ**: Angular チームによって事前に定義し、AngularJS フレームワークの一部であるこれらです。</span><span class="sxs-lookup"><span data-stu-id="fd2de-137">**Primitive Directives**: These are predefined by the Angular team and are part of the AngularJS framework.</span></span>

   2. <span data-ttu-id="fd2de-138">**カスタム ディレクティブ**: これらは、カスタム ディレクティブを定義できます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-138">**Custom Directives**: These are custom directives that you can define.</span></span>

<span data-ttu-id="fd2de-139">AngularJS のすべてのアプリケーションで使用されるプリミティブ ディレクティブのいずれかが、`ng-app`ディレクティブで、AngularJS アプリケーションをブートス トラップします。</span><span class="sxs-lookup"><span data-stu-id="fd2de-139">One of the primitive directives used in all AngularJS applications is the `ng-app` directive, which bootstraps the AngularJS application.</span></span> <span data-ttu-id="fd2de-140">このディレクティブに適用できます、`<body>`タグまたは本文の子要素です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-140">This directive can be applied to the `<body>` tag or to a child element of the body.</span></span> <span data-ttu-id="fd2de-141">アクションの例を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="fd2de-141">Let's see an example in action.</span></span> <span data-ttu-id="fd2de-142">ASP.NET プロジェクトで開いている場合、行うことができますか、追加する HTML ファイルを`wwwroot`フォルダー、または新しいコント ローラーのアクションと関連するビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-142">Assuming you're in an ASP.NET project, you can either add an HTML file to the `wwwroot` folder, or add a new controller action and an associated view.</span></span> <span data-ttu-id="fd2de-143">この場合、追加した新しい`Directives`アクション メソッド`HomeController.cs`です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-143">In this case, I've added a new `Directives` action method to `HomeController.cs`.</span></span> <span data-ttu-id="fd2de-144">関連するビューを次に示します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-144">The associated view is shown here:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]

<span data-ttu-id="fd2de-145">これらのサンプルを互いに独立するおくと、使用せずに、共有のレイアウト ファイルです。</span><span class="sxs-lookup"><span data-stu-id="fd2de-145">To keep these samples independent of one another, I'm not using the shared layout file.</span></span> <span data-ttu-id="fd2de-146">Body タグを装飾おを参照してください、`ng-app`このページを示すディレクティブは、AngularJS アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="fd2de-146">You can see that we decorated the body tag with the `ng-app` directive to indicate this page is an AngularJS application.</span></span> <span data-ttu-id="fd2de-147">`{{2+2}}`よりについて学びますしばらくを角運動のデータ バインディング式を指定します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-147">The `{{2+2}}` is an Angular data binding expression that you will learn more about in a moment.</span></span> <span data-ttu-id="fd2de-148">このアプリケーションを実行する場合、結果を次に示します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-148">Here is the result if you run this application:</span></span>

![単純な Angular ディレクティブ](angular/_static/simple-directive.png)

<span data-ttu-id="fd2de-150">その他のプリミティブ AngularJS ディレクティブは、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="fd2de-150">Other primitive directives in AngularJS include:</span></span>

<span data-ttu-id="fd2de-151">`ng-controller`JavaScript コント ローラーはどのビューにバインドを決定します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-151">`ng-controller` Determines which JavaScript controller is bound to which view.</span></span>

<span data-ttu-id="fd2de-152">`ng-model`HTML 要素のプロパティの値がバインドされているモデルを決定します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-152">`ng-model` Determines the model to which the values of an HTML element's properties are bound.</span></span>

<span data-ttu-id="fd2de-153">`ng-init`現在のスコープを表す式の形式でアプリケーション データを初期化するために使用します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-153">`ng-init` Used to initialize the application data in the form of an expression for the current scope.</span></span>

<span data-ttu-id="fd2de-154">`ng-if`削除するか、指定された HTML 要素に指定された式の truthiness に基づいて DOM 内で再作成します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-154">`ng-if` Removes or recreates the given HTML element in the DOM based on the truthiness of the expression provided.</span></span>

<span data-ttu-id="fd2de-155">`ng-repeat`データのセットで指定された HTML ブロックを繰り返します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-155">`ng-repeat` Repeats a given block of HTML over a set of data.</span></span>

<span data-ttu-id="fd2de-156">`ng-show`指定した式に基づいて、指定された HTML 要素の表示と非表示を切り替えます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-156">`ng-show` Shows or hides the given HTML element based on the expression provided.</span></span>

<span data-ttu-id="fd2de-157">AngularJS でサポートされるすべてのプリミティブ ディレクティブの完全な一覧を参照してください、 [AngularJS ドキュメント web サイトでのディレクティブのドキュメント セクション](https://docs.angularjs.org/api/ng/directive)です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-157">For a full list of all primitive directives supported in AngularJS, please refer to the [directive documentation section on the AngularJS documentation website](https://docs.angularjs.org/api/ng/directive).</span></span>

### <a name="data-binding"></a><span data-ttu-id="fd2de-158">データ バインディング</span><span class="sxs-lookup"><span data-stu-id="fd2de-158">Data binding</span></span>

<span data-ttu-id="fd2de-159">AngularJS 提供[データ バインディング](https://docs.angularjs.org/guide/databinding)サポートの-すぐいずれかを使用して、`ng-bind`ディレクティブまたは式の構文をなど、バインド データ`{{expression}}`です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-159">AngularJS provides [data binding](https://docs.angularjs.org/guide/databinding) support out-of-the-box using either the `ng-bind` directive or a data binding expression syntax such as `{{expression}}`.</span></span> <span data-ttu-id="fd2de-160">AngularJS には、常にテンプレートの表示との同期のモデルからのデータが保存されている双方向データ バインディングがサポートしています。</span><span class="sxs-lookup"><span data-stu-id="fd2de-160">AngularJS supports two-way data binding where data from a model is kept in synchronization with a view template at all times.</span></span> <span data-ttu-id="fd2de-161">ビューへの変更は、モデルに自動的に反映されます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-161">Any changes to the view are automatically reflected in the model.</span></span> <span data-ttu-id="fd2de-162">同様に、モデル内のすべての変更は、ビューに反映されます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-162">Likewise, any changes in the model are reflected in the view.</span></span>

<span data-ttu-id="fd2de-163">という名前の付随するビューを HTML ファイルまたはコント ローラーのアクションのいずれかを作成する`Databinding`です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-163">Create either an HTML file or a controller action with an accompanying view named `Databinding`.</span></span> <span data-ttu-id="fd2de-164">ビューには、次を含めます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-164">Include the following in the view:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]

<span data-ttu-id="fd2de-165">ディレクティブまたはデータのバインドを使用してモデルの値を表示することに注意してください (`ng-bind`)。</span><span class="sxs-lookup"><span data-stu-id="fd2de-165">Notice that you can display model values using either directives or data binding (`ng-bind`).</span></span> <span data-ttu-id="fd2de-166">結果のページは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="fd2de-166">The resulting page should look like this:</span></span>

![単純データ バインディング](angular/_static/simple-databinding.png)

### <a name="templates"></a><span data-ttu-id="fd2de-168">テンプレート</span><span class="sxs-lookup"><span data-stu-id="fd2de-168">Templates</span></span>

<span data-ttu-id="fd2de-169">[テンプレート](https://docs.angularjs.org/guide/templates)AngularJS で単の HTML ページで装飾 AngularJS ディレクティブと成果物です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-169">[Templates](https://docs.angularjs.org/guide/templates) in AngularJS are just plain HTML pages decorated with AngularJS directives and artifacts.</span></span> <span data-ttu-id="fd2de-170">AngularJS のテンプレートは、ディレクティブ、式、フィルター、およびと HTML フォームのビューを結合するコントロールの組み合わせです。</span><span class="sxs-lookup"><span data-stu-id="fd2de-170">A template in AngularJS is a mixture of directives, expressions, filters, and controls that combine with HTML to form the view.</span></span>

<span data-ttu-id="fd2de-171">テンプレートを示すために別のビューを追加し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-171">Add another view to demonstrate templates, and add the following to it:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]

<span data-ttu-id="fd2de-172">テンプレートに 1 つのような AngularJS ディレクティブ`ng-app`、 `ng-init`、`ng-model`とバインドするデータ バインド式の構文、`personName`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="fd2de-172">The template has AngularJS directives like `ng-app`, `ng-init`, `ng-model` and data binding expression syntax to bind the `personName` property.</span></span> <span data-ttu-id="fd2de-173">ビューは、ブラウザーで実行されている、ため、次のスクリーン ショットのようになります。</span><span class="sxs-lookup"><span data-stu-id="fd2de-173">Running in the browser, the view looks like the screenshot below:</span></span>

![テンプレートの簡単な例 1](angular/_static/simple-templates-1.png)

<span data-ttu-id="fd2de-175">入力フィールドに入力して名前を変更する場合、入力フィールドの横にあるテキストを動的に表示されますアクションで角運動の双方向データ バインディングの表示、更新します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-175">If you change the name by typing in the input field, you will see the text next to the input field dynamically update, showing Angular two-way data binding in action.</span></span>

![単純なテンプレート例 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a><span data-ttu-id="fd2de-177">式</span><span class="sxs-lookup"><span data-stu-id="fd2de-177">Expressions</span></span>

<span data-ttu-id="fd2de-178">[式](https://docs.angularjs.org/guide/expression)AngularJS 内に記述された JavaScript に似たコード スニペットは、`{{ expression }}`構文です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-178">[Expressions](https://docs.angularjs.org/guide/expression) in AngularJS are JavaScript-like code snippets that are written inside the `{{ expression }}` syntax.</span></span> <span data-ttu-id="fd2de-179">これらの式からのデータと同じ方法を HTML にバインドされた`ng-bind`ディレクティブです。</span><span class="sxs-lookup"><span data-stu-id="fd2de-179">The data from these expressions is bound to HTML the same way as `ng-bind` directives.</span></span> <span data-ttu-id="fd2de-180">AngularJS 式および JavaScript の正規表現の主な違いがその AngularJS に対して式が評価される、 `$scope` AngularJS 内のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="fd2de-180">The main difference between AngularJS expressions and regular JavaScript expressions is that AngularJS expressions are evaluated against the `$scope` object in AngularJS.</span></span>

<span data-ttu-id="fd2de-181">AngularJS 式バインドの下のサンプルで`personName`と、JavaScript の単純な式を計算します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-181">The AngularJS expressions in the sample below bind `personName` and a simple JavaScript calculated expression:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]

<span data-ttu-id="fd2de-182">ブラウザーの表示での実行など、`personName`データと計算の結果。</span><span class="sxs-lookup"><span data-stu-id="fd2de-182">The example running in the browser displays the `personName` data and the results of the calculation:</span></span>

![単純な式](angular/_static/simple-expressions.png)

### <a name="repeaters"></a><span data-ttu-id="fd2de-184">リピータ</span><span class="sxs-lookup"><span data-stu-id="fd2de-184">Repeaters</span></span>

<span data-ttu-id="fd2de-185">呼ばれるプリミティブ ディレクティブを使用して行うは AngularJS の繰り返し`ng-repeat`です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-185">Repeating in AngularJS is done via a primitive directive called `ng-repeat`.</span></span> <span data-ttu-id="fd2de-186">`ng-repeat`ディレクティブは、繰り返しのデータ配列の長さで、ビューで指定された HTML 要素が繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-186">The `ng-repeat` directive repeats a given HTML element in a view over the length of a repeating data array.</span></span> <span data-ttu-id="fd2de-187">リピータ AngularJS では、文字列またはオブジェクトの配列を繰り返すことができます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-187">Repeaters in AngularJS can repeat over an array of strings or objects.</span></span> <span data-ttu-id="fd2de-188">文字列の配列を繰り返しの使用例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-188">Here is a sample usage of repeating over an array of strings:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]

<span data-ttu-id="fd2de-189">[Repeat ディレクティブ](https://docs.angularjs.org/api/ng/directive/ngRepeat)このスクリーン ショットに示すように、開発者ツールでわかるように、順序なしのリストのリスト項目の系列を出力します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-189">The [repeat directive](https://docs.angularjs.org/api/ng/directive/ngRepeat) outputs a series of list items in an unordered list, as you can see in the developer tools shown in this screenshot:</span></span>

![リピータの例](angular/_static/repeater.png)

<span data-ttu-id="fd2de-191">オブジェクトの配列でが繰り返さ 例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-191">Here is an example that repeats over an array of objects.</span></span> <span data-ttu-id="fd2de-192">`ng-init`ディレクティブの確立、`names`の各要素が最初に格納するオブジェクトし、姓と名の配列。</span><span class="sxs-lookup"><span data-stu-id="fd2de-192">The `ng-init` directive establishes a `names` array, where each element is an object containing first and last names.</span></span> <span data-ttu-id="fd2de-193">`ng-repeat`割り当て、`name in names`配列のすべての要素の一覧項目を出力します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-193">The `ng-repeat` assignment, `name in names`, outputs a list item for every array element.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]

<span data-ttu-id="fd2de-194">出力では、前の例のように同じここでです。</span><span class="sxs-lookup"><span data-stu-id="fd2de-194">The output in this case is the same as in the previous example.</span></span>

<span data-ttu-id="fd2de-195">角度は、その実行に、ループがある場合に基づく動作のために役立ついくつかの追加ディレクティブを提供します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-195">Angular provides some additional directives that can help provide behavior based on where the loop is in its execution.</span></span>

`$index`

<span data-ttu-id="fd2de-196">使用`$index`で、`ng-repeat`では、ループを使用している、インデックス、ループの現在位置を決定します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-196">Use `$index` in the `ng-repeat` loop to determine which index position your loop currently is on.</span></span>

<span data-ttu-id="fd2de-197">`$even` および `$odd`</span><span class="sxs-lookup"><span data-stu-id="fd2de-197">`$even` and `$odd`</span></span>

<span data-ttu-id="fd2de-198">使用して`$even`で、`ng-repeat`ループを使用して、ループ内の現在のインデックスがあってもインデックスの行であるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-198">Use `$even` in the `ng-repeat` loop to determine whether the current index in your loop is an even indexed row.</span></span> <span data-ttu-id="fd2de-199">同様に、使用`$odd`現在のインデックスが奇数のインデックス付き行かどうかを決定します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-199">Similarly, use `$odd` to determine if the current index is an odd indexed row.</span></span>

<span data-ttu-id="fd2de-200">`$first` および `$last`</span><span class="sxs-lookup"><span data-stu-id="fd2de-200">`$first` and `$last`</span></span>

<span data-ttu-id="fd2de-201">使用して`$first`で、`ng-repeat`ループを使用して、ループ内の現在のインデックスが最初の行であるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-201">Use `$first` in the `ng-repeat` loop to determine whether the current index in your loop is the first row.</span></span> <span data-ttu-id="fd2de-202">同様に、使用`$last`現在のインデックスが最後の行を判断します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-202">Similarly, use `$last` to determine if the current index is the last row.</span></span>

<span data-ttu-id="fd2de-203">示すサンプルを次に示します`$index`、 `$even`、 `$odd`、 `$first`、および`$last`の動作。</span><span class="sxs-lookup"><span data-stu-id="fd2de-203">Below is a sample that shows `$index`, `$even`, `$odd`, `$first`, and `$last` in action:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]

<span data-ttu-id="fd2de-204">結果の出力を次に示します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-204">Here is the resulting output:</span></span>

![リピータ例 2](angular/_static/repeaters2.png)

### <a name="scope"></a><span data-ttu-id="fd2de-206">$scope</span><span class="sxs-lookup"><span data-stu-id="fd2de-206">$scope</span></span>

<span data-ttu-id="fd2de-207">`$scope`JavaScript オブジェクト (テンプレート) ビューと (下記参照)、コント ローラーの間でグルーとして機能します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-207">`$scope` is a JavaScript object that acts as glue between the view (template) and the controller (explained below).</span></span> <span data-ttu-id="fd2de-208">AngularJS のビュー テンプレートのみが知ってに割り当てられた値、`$scope`コント ローラー内のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="fd2de-208">A view template in AngularJS only knows about the values attached to the `$scope` object in the controller.</span></span>

> [!NOTE]
> <span data-ttu-id="fd2de-209">MVVM 世界では、 `$scope` AngularJS 内のオブジェクトは多くの場合、ViewModel として定義されています。</span><span class="sxs-lookup"><span data-stu-id="fd2de-209">In the MVVM world, the `$scope` object in AngularJS is often defined as the ViewModel.</span></span> <span data-ttu-id="fd2de-210">AngularJS チームを指す、`$scope`データ モデルとしてオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="fd2de-210">The AngularJS team refers to the `$scope` object as the Data-Model.</span></span> <span data-ttu-id="fd2de-211">[AngularJS のスコープの詳細について](https://docs.angularjs.org/guide/scope)です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-211">[Learn more about Scopes in AngularJS](https://docs.angularjs.org/guide/scope).</span></span>

<span data-ttu-id="fd2de-212">プロパティを設定する方法を示す簡単な例を次に示します`$scope`別の JavaScript ファイル内で*scope.js*:</span><span class="sxs-lookup"><span data-stu-id="fd2de-212">Below is a simple example showing how to set properties on `$scope` within a separate JavaScript file, *scope.js*:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]

<span data-ttu-id="fd2de-213">確認、`$scope`パラメーターが 2 行目のコント ローラーに渡されます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-213">Observe the `$scope` parameter passed to the controller on line 2.</span></span> <span data-ttu-id="fd2de-214">このオブジェクトは、ビューの知ってます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-214">This object is what the view knows about.</span></span> <span data-ttu-id="fd2de-215">"Mary Jane"に"name"と呼ばれるプロパティを設定すると 3 行目におはします。</span><span class="sxs-lookup"><span data-stu-id="fd2de-215">On line 3, we are setting a property called "name" to "Mary Jane".</span></span>

<span data-ttu-id="fd2de-216">新機能は、特定のプロパティはビューによって検出されなかった場合にどうなりますか。</span><span class="sxs-lookup"><span data-stu-id="fd2de-216">What happens when a particular property is not found by the view?</span></span> <span data-ttu-id="fd2de-217">定義を次に、ビューは、"name"および"age"プロパティを参照します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-217">The view defined below refers to "name" and "age" properties:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]

<span data-ttu-id="fd2de-218">式の構文を使用して、"name"プロパティを表示する角度をいただきます 9 行目に注意してください。</span><span class="sxs-lookup"><span data-stu-id="fd2de-218">Notice on line 9 that we are asking Angular to show the "name" property using expression syntax.</span></span> <span data-ttu-id="fd2de-219">行 10 は、"age"、存在しないプロパティを参照します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-219">Line 10 then refers to "age", a property that does not exist.</span></span> <span data-ttu-id="fd2de-220">実行中の例では、年齢の"Mary Jane"および nothing に設定した名前を示します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-220">The running example shows the name set to "Mary Jane" and nothing for age.</span></span> <span data-ttu-id="fd2de-221">不足しているプロパティは無視されます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-221">Missing properties are ignored.</span></span>

![スコープの例](angular/_static/scope.png)

### <a name="modules"></a><span data-ttu-id="fd2de-223">モジュール</span><span class="sxs-lookup"><span data-stu-id="fd2de-223">Modules</span></span>

<span data-ttu-id="fd2de-224">A[モジュール](https://docs.angularjs.org/guide/module)で AngularJS のコント ローラー、サービス、ディレクティブなどのコレクション。`angular.module()`を作成し、登録、および、AngularJS モジュールを取得する関数呼び出しを使用します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-224">A [module](https://docs.angularjs.org/guide/module) in AngularJS is a collection of controllers, services, directives, etc. The `angular.module()` function call is used to create, register, and retrieve modules in AngularJS.</span></span> <span data-ttu-id="fd2de-225">使用して、AngularJS チーム、およびサードパーティのライブラリ、出荷を含め、すべてのモジュールを登録する必要があります、`angular.module()`関数。</span><span class="sxs-lookup"><span data-stu-id="fd2de-225">All modules, including those shipped by the AngularJS team and third party libraries, should be registered using the `angular.module()` function.</span></span>

<span data-ttu-id="fd2de-226">以下の AngularJS で新しいモジュールを作成する方法を示すコード スニペットに示します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-226">Below is a snippet of code that shows how to create a new module in AngularJS.</span></span> <span data-ttu-id="fd2de-227">最初のパラメーターは、モジュールの名前です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-227">The first parameter is the name of the module.</span></span> <span data-ttu-id="fd2de-228">2 番目のパラメーターは、他のモジュールの依存関係を定義します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-228">The second parameter defines dependencies on other modules.</span></span> <span data-ttu-id="fd2de-229">この記事で後述おが表示されるにこれらの依存関係を渡す方法、`angular.module()`メソッドの呼び出しです。</span><span class="sxs-lookup"><span data-stu-id="fd2de-229">Later in this article, we will be showing how to pass these dependencies to an `angular.module()` method call.</span></span>

```javascript
var personApp = angular.module('personApp', []);
```

<span data-ttu-id="fd2de-230">使用して、 `ng-app`  ページで、AngularJS モジュールを表すためのディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="fd2de-230">Use the `ng-app` directive to represent an AngularJS module on the page.</span></span> <span data-ttu-id="fd2de-231">モジュールを使用して、モジュールの名前を割り当て、`personApp`この例を`ng-app`テンプレートのディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="fd2de-231">To use a module, assign the name of the module, `personApp` in this example, to the `ng-app` directive in our template.</span></span>

```html
<body ng-app="personApp">
```

### <a name="controllers"></a><span data-ttu-id="fd2de-232">コント ローラー</span><span class="sxs-lookup"><span data-stu-id="fd2de-232">Controllers</span></span>

<span data-ttu-id="fd2de-233">[コント ローラー](https://docs.angularjs.org/guide/controller) AngularJS では、コードのエントリの最初のポイント。</span><span class="sxs-lookup"><span data-stu-id="fd2de-233">[Controllers](https://docs.angularjs.org/guide/controller) in AngularJS are the first point of entry for your code.</span></span> <span data-ttu-id="fd2de-234">`<module name>.controller()`関数呼び出しの使用を作成し、AngularJS コント ローラーに登録します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-234">The `<module name>.controller()` function call is used to create and register controllers in AngularJS.</span></span> <span data-ttu-id="fd2de-235">`ng-controller`ディレクティブを使用する HTML ページで、AngularJS コント ローラーを表します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-235">The `ng-controller` directive is used to represent an AngularJS controller on the HTML page.</span></span> <span data-ttu-id="fd2de-236">角のコント ローラーの役割は、データ モデルの状態と動作を設定する (`$scope`)。</span><span class="sxs-lookup"><span data-stu-id="fd2de-236">The role of the controller in Angular is to set state and behavior of the data model (`$scope`).</span></span> <span data-ttu-id="fd2de-237">コント ローラーは、DOM を直接操作するのには使用できません。</span><span class="sxs-lookup"><span data-stu-id="fd2de-237">Controllers should not be used to manipulate the DOM directly.</span></span>

<span data-ttu-id="fd2de-238">新しいコント ローラーを登録するコードの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-238">Below is a snippet of code that registers a new controller.</span></span> <span data-ttu-id="fd2de-239">`personApp` Angular モジュールで、2 行目の定義を参照して、スニペット内の変数です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-239">The `personApp` variable in the snippet references an Angular module, which is defined on line 2.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]

<span data-ttu-id="fd2de-240">使用して、ビュー、`ng-controller`ディレクティブには、コント ローラー名が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-240">The view using the `ng-controller` directive assigns the controller name:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]

<span data-ttu-id="fd2de-241">"Mary"と"Jane"に対応するページを示しています、`firstName`と`lastName`にアタッチされているプロパティ、`$scope`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="fd2de-241">The page shows "Mary" and "Jane" that correspond to the `firstName` and `lastName` properties attached to the `$scope` object:</span></span>

![コント ローラーの例](angular/_static/controllers.png)

### <a name="components"></a><span data-ttu-id="fd2de-243">コンポーネント</span><span class="sxs-lookup"><span data-stu-id="fd2de-243">Components</span></span>

<span data-ttu-id="fd2de-244">[コンポーネント](https://docs.angularjs.org/guide/component)角で 1.5.x をカプセル化し個々 の HTML 要素を作成する機能を許可します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-244">[Components](https://docs.angularjs.org/guide/component) in Angular 1.5.x allow for the encapsulation and capability of creating individual HTML elements.</span></span> <span data-ttu-id="fd2de-245">Angular 1.4.x で .directive() メソッドを使用して、同じ機能を得ることができます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-245">In Angular 1.4.x you could achieve the same feature using the .directive() method.</span></span>

<span data-ttu-id="fd2de-246">.Component() メソッドを使用すると、開発が簡素化されますディレクティブとコント ローラーの機能を取得します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-246">By using the .component() method, development is simplified gaining the functionality of the directive and the controller.</span></span> <span data-ttu-id="fd2de-247">その他の利点があります。スコープの分離のベスト プラクティスは、本質的なと角運動 2 への移行をより簡単な作業になります。</span><span class="sxs-lookup"><span data-stu-id="fd2de-247">Other benefits include; scope isolation, best practices are inherent, and migration to Angular 2 becomes an easier task.</span></span> <span data-ttu-id="fd2de-248">`<module name>.component()`関数呼び出しの使用を作成し、AngularJS にコンポーネントを登録します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-248">The `<module name>.component()` function call is used to create and register components in AngularJS.</span></span>

<span data-ttu-id="fd2de-249">新しいコンポーネントを登録するコードの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-249">Below is a snippet of code that registers a new component.</span></span> <span data-ttu-id="fd2de-250">`personApp` Angular モジュールで、2 行目の定義を参照して、スニペット内の変数です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-250">The `personApp` variable in the snippet references an Angular module, which is defined on line 2.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]

<span data-ttu-id="fd2de-251">ビューでは、カスタムの HTML 要素を示されています。</span><span class="sxs-lookup"><span data-stu-id="fd2de-251">The view where we are displaying the custom HTML element.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]

<span data-ttu-id="fd2de-252">コンポーネントによって使用される関連付けられているテンプレート:</span><span class="sxs-lookup"><span data-stu-id="fd2de-252">The associated template used by component:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]

<span data-ttu-id="fd2de-253">"Aftab"と"Ansari"に対応するページを示しています、`firstName`と`lastName`にアタッチされているプロパティ、`vm`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="fd2de-253">The page shows "Aftab" and "Ansari" that correspond to the `firstName` and `lastName` properties attached to the `vm` object:</span></span>

![コンポーネントの例](angular/_static/components.png)

### <a name="services"></a><span data-ttu-id="fd2de-255">サービス</span><span class="sxs-lookup"><span data-stu-id="fd2de-255">Services</span></span>

<span data-ttu-id="fd2de-256">[サービス](https://docs.angularjs.org/guide/services)AngularJS で一般的に使用され角運動のアプリケーションの有効期間全体で使用できるファイルに抽象化は、共有コード。</span><span class="sxs-lookup"><span data-stu-id="fd2de-256">[Services](https://docs.angularjs.org/guide/services) in AngularJS are commonly used for shared code that is abstracted away into a file which can be used throughout the lifetime of an Angular application.</span></span> <span data-ttu-id="fd2de-257">サービスは遅延インスタンス化、つまりことがありますするには、サービスのインスタンスは、サービスに依存するコンポーネントを使用します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-257">Services are lazily instantiated, meaning that there will not be an instance of a service unless a component that depends on the service gets used.</span></span> <span data-ttu-id="fd2de-258">ファクトリには、AngularJS アプリケーションで使用するサービスの例があります。</span><span class="sxs-lookup"><span data-stu-id="fd2de-258">Factories are an example of a service used in AngularJS applications.</span></span> <span data-ttu-id="fd2de-259">使用してファクトリを作成、`myApp.factory()`関数呼び出し、ここで`myApp`モジュールは、します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-259">Factories are created using the `myApp.factory()` function call, where `myApp` is the module.</span></span>

<span data-ttu-id="fd2de-260">以下には、AngularJS ファクトリを使用する方法を示す例を示します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-260">Below is an example that shows how to use factories in AngularJS:</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]

<span data-ttu-id="fd2de-261">コント ローラーからこのファクトリを呼び出す、渡す`personFactory`へのパラメーターとして、`controller`関数。</span><span class="sxs-lookup"><span data-stu-id="fd2de-261">To call this factory from the controller, pass `personFactory` as a parameter to the `controller` function:</span></span>

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a><span data-ttu-id="fd2de-262">サービスを使用して REST エンドポイントと通信</span><span class="sxs-lookup"><span data-stu-id="fd2de-262">Using services to talk to a REST endpoint</span></span>

<span data-ttu-id="fd2de-263">以下は、エンド ツー エンドの例サービスを使用して AngularJS ASP.NET Core Web API エンドポイントとの対話にします。</span><span class="sxs-lookup"><span data-stu-id="fd2de-263">Below is an end-to-end example using services in AngularJS to interact with an ASP.NET Core Web API endpoint.</span></span> <span data-ttu-id="fd2de-264">この例では、Web API からデータを取得し、ビューのテンプレートで、データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-264">The example gets data from the Web API and displays the data in a view template.</span></span> <span data-ttu-id="fd2de-265">まず、ビューを開始しましょう。</span><span class="sxs-lookup"><span data-stu-id="fd2de-265">Let's start with the view first:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]

<span data-ttu-id="fd2de-266">このビューであると呼ばれる、Angular モジュール`PersonsApp`、コント ローラーと呼ばれます`personController`です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-266">In this view, we have an Angular module called `PersonsApp` and a controller called `personController`.</span></span> <span data-ttu-id="fd2de-267">使用している`ng-repeat`担当者のリストを反復処理します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-267">We are using `ng-repeat` to iterate over the list of persons.</span></span> <span data-ttu-id="fd2de-268">私たちは、17 ~ 19 の行に 3 つのカスタム JavaScript ファイルを参照しています。</span><span class="sxs-lookup"><span data-stu-id="fd2de-268">We are referencing three custom JavaScript files on lines 17-19.</span></span>

<span data-ttu-id="fd2de-269">*PersonApp.js*ファイルを登録するため、`PersonsApp`モジュールであると、構文は、前の例に似ています。</span><span class="sxs-lookup"><span data-stu-id="fd2de-269">The *personApp.js* file is used to register the `PersonsApp` module; and, the syntax is similar to previous examples.</span></span> <span data-ttu-id="fd2de-270">使用して、`angular.module`おりますが、モジュールの新しいインスタンスを作成する関数。</span><span class="sxs-lookup"><span data-stu-id="fd2de-270">We are using the `angular.module` function to create a new instance of the module that we will be working with.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]

<span data-ttu-id="fd2de-271">見てみましょう*personFactory.js*、後述します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-271">Let's take a look at *personFactory.js*, below.</span></span> <span data-ttu-id="fd2de-272">モジュールを呼び出している`factory`ファクトリを作成するメソッド。</span><span class="sxs-lookup"><span data-stu-id="fd2de-272">We are calling the module’s `factory` method to create a factory.</span></span> <span data-ttu-id="fd2de-273">12 行目は、組み込みの角を示しています。`$http`サービス、web サービスからユーザー情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-273">Line 12 shows the built-in Angular `$http` service retrieving people information from a web service.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]

<span data-ttu-id="fd2de-274">*PersonController.js*、呼び出しているモジュールの`controller`コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-274">In *personController.js*, we are calling the module’s `controller` method to create the controller.</span></span> <span data-ttu-id="fd2de-275">`$scope`オブジェクトの`people`プロパティには、personFactory (13 の行) から返されたデータが割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-275">The `$scope` object's `people` property is assigned the data returned from the personFactory (line 13).</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]

<span data-ttu-id="fd2de-276">Web API とその背後にあるモデルを簡単に見てをみましょう。</span><span class="sxs-lookup"><span data-stu-id="fd2de-276">Let's take a quick look at the Web API and the model behind it.</span></span> <span data-ttu-id="fd2de-277">`Person`モデルは POCO (Plain Old CLR Object) `Id`、 `FirstName`、および`LastName`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="fd2de-277">The `Person` model is a POCO (Plain Old CLR Object) with `Id`, `FirstName`, and `LastName` properties:</span></span>

[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]

<span data-ttu-id="fd2de-278">`Person`コント ローラーは、JSON 形式の一覧を返します`Person`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="fd2de-278">The `Person` controller returns a JSON-formatted list of `Person` objects:</span></span>

[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]

<span data-ttu-id="fd2de-279">アプリケーションの動作を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="fd2de-279">Let's see the application in action:</span></span>

![残りの部分を表示するコント ローラーを結果します。](angular/_static/rest-bound.png)

<span data-ttu-id="fd2de-281">実行できます[GitHub で、アプリケーションの構造を表示](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample)です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-281">You can [view the application's structure on GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span></span>

> [!NOTE]
> <span data-ttu-id="fd2de-282">詳細 AngularJS アプリケーションを構成するのには、次を参照してください[John Papa Angular スタイル ガイド。](https://github.com/johnpapa/angular-styleguide)</span><span class="sxs-lookup"><span data-stu-id="fd2de-282">For more on structuring AngularJS applications, see [John Papa's Angular Style Guide](https://github.com/johnpapa/angular-styleguide)</span></span>

&nbsp;

> [!NOTE]
> <span data-ttu-id="fd2de-283">AngularJS モジュール、コント ローラー ファクトリを作成するディレクティブとビューのファイルを簡単にする Sayed Hashimi を確認してください[for Visual Studio SideWaffle テンプレート パック](http://sidewaffle.com/)です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-283">To create AngularJS module, controller, factory, directive and view files easily, be sure to check out Sayed Hashimi's [SideWaffle template pack for Visual Studio](http://sidewaffle.com/).</span></span> <span data-ttu-id="fd2de-284">Sayed Hashimi は、Microsoft の Visual Studio Web チームのシニア プログラム マネージャー、SideWaffle テンプレートには、最高水準と見なされます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-284">Sayed Hashimi is a Senior Program Manager on the Visual Studio Web Team at Microsoft and SideWaffle templates are considered the gold standard.</span></span> <span data-ttu-id="fd2de-285">この記事の執筆時に、SideWaffle は 2013 および 2015、Visual Studio 2012 を使用できます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-285">At the time of this writing, SideWaffle is available for Visual Studio 2012, 2013, and 2015.</span></span>

### <a name="routing-and-multiple-views"></a><span data-ttu-id="fd2de-286">ルーティングと複数のビュー</span><span class="sxs-lookup"><span data-stu-id="fd2de-286">Routing and multiple views</span></span>

<span data-ttu-id="fd2de-287">AngularJS には、SPA (Single Page Application) ベースのナビゲーションを処理する組み込みのルートのプロバイダーが存在します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-287">AngularJS has a built-in route provider to handle SPA (Single Page Application) based navigation.</span></span> <span data-ttu-id="fd2de-288">AngularJS ルーティングを使用して作業を追加する必要があります、`angular-route`ライブラリ Bower を使用します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-288">To work with routing in AngularJS, you must add the `angular-route` library using Bower.</span></span> <span data-ttu-id="fd2de-289">表示、 [bower.json](#angular-bower-json)おは既に参照していること、プロジェクトにこの記事の開始時に参照されているファイル。</span><span class="sxs-lookup"><span data-stu-id="fd2de-289">You can see in the [bower.json](#angular-bower-json) file referenced at the start of this article that we are already referencing it in our project.</span></span>

<span data-ttu-id="fd2de-290">パッケージをインストールした後のスクリプト参照の追加 (*角度 route.js*) の表示にします。</span><span class="sxs-lookup"><span data-stu-id="fd2de-290">After you install the package, add the script reference (*angular-route.js*) to your view.</span></span>

<span data-ttu-id="fd2de-291">今すぐを構築しておナビゲーションを追加して、ユーザー アプリを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="fd2de-291">Now let's take the Person App we have been building and add navigation to it.</span></span> <span data-ttu-id="fd2de-292">アプリのコピーを作成新規作成することで最初に、`PeopleController`呼び出すアクション`Spa`と対応する`Spa.cshtml`Index.cshtml のビューをコピーして、ビュー、`People`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="fd2de-292">First, we will make a copy of the app by creating a new `PeopleController` action called `Spa` and a corresponding `Spa.cshtml` view by copying the Index.cshtml view in the `People` folder.</span></span> <span data-ttu-id="fd2de-293">スクリプト参照を追加`angular-route`(11 行目を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="fd2de-293">Add a script reference to `angular-route` (see line 11).</span></span> <span data-ttu-id="fd2de-294">追加も、`div`でマークされた、`ng-view`ディレクティブ (6 行目を参照してください) でビューを配置するプレース ホルダーとして。</span><span class="sxs-lookup"><span data-stu-id="fd2de-294">Also add a `div` marked with the `ng-view` directive (see line 6) as a placeholder to place views in.</span></span> <span data-ttu-id="fd2de-295">ここでは、いくつかの追加を*.js* 13 ~ 16 の行に参照されるファイルです。</span><span class="sxs-lookup"><span data-stu-id="fd2de-295">We are going to be using several additional *.js* files which are referenced on lines 13-16.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]

<span data-ttu-id="fd2de-296">見てみましょう*personModule.js*ファイルをどのようにルーティング モジュールをインスタンス化おを参照します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-296">Let's take a look at *personModule.js* file to see how we are instantiating the module with routing.</span></span> <span data-ttu-id="fd2de-297">渡している`ngRoute`モジュールにライブラリとして。</span><span class="sxs-lookup"><span data-stu-id="fd2de-297">We are passing `ngRoute` as a library into the module.</span></span> <span data-ttu-id="fd2de-298">このモジュールは、アプリケーションでのルーティングを処理します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-298">This module handles routing in our application.</span></span>

[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]

<span data-ttu-id="fd2de-299">*PersonRoutes.js*ファイルを次に、ルート プロバイダーに基づいてルートを定義します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-299">The *personRoutes.js* file, below, defines routes based on the route provider.</span></span> <span data-ttu-id="fd2de-300">行 4 ~ 7 効果的を言うことにより、ときに使用したの URL ナビゲーションを定義する`/persons`がという名前のテンプレートを使用して、要求された`partials/personlist`を通じて`personListController`です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-300">Lines 4-7 define navigation by effectively saying, when a URL with `/persons` is requested, use a template called `partials/personlist` by working through `personListController`.</span></span> <span data-ttu-id="fd2de-301">8 11 行目のルート パラメーターを持つ詳細ページを示す`personId`です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-301">Lines 8-11 indicate a detail page with a route parameter of `personId`.</span></span> <span data-ttu-id="fd2de-302">URL は、いずれのパターンと一致しません場合、角速度既定値は、`/persons`ビュー。</span><span class="sxs-lookup"><span data-stu-id="fd2de-302">If the URL doesn't match one of the patterns, Angular defaults to the `/persons` view.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]

<span data-ttu-id="fd2de-303">`personlist.html`ファイルがユーザーの一覧を表示するために必要な HTML のみを含む部分ビュー。</span><span class="sxs-lookup"><span data-stu-id="fd2de-303">The `personlist.html` file is a partial view containing only the HTML needed to display person list.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]

<span data-ttu-id="fd2de-304">コント ローラーが、モジュールを使用して定義されている`controller`関数*personListController.js*です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-304">The controller is defined by using the module's `controller` function in *personListController.js*.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]

<span data-ttu-id="fd2de-305">このアプリケーションを実行するに移動し、 `people/spa#/persons` URL が表示されます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-305">If we run this application and navigate to the `people/spa#/persons` URL, we will see:</span></span>

![担当者のリスト ビュー](angular/_static/spa-persons.png)

<span data-ttu-id="fd2de-307">たとえばに詳細ページに移動したかどうかは`people/spa#/persons/2`、部分の詳細ビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-307">If we navigate to a detail page, for example `people/spa#/persons/2`, we will see the detail partial view:</span></span>

![ユーザーの詳細ビュー](angular/_static/spa-persons-2.png)

<span data-ttu-id="fd2de-309">完全なソースおよびにこの記事で表示されないすべてのファイルを表示できます[GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample)です。</span><span class="sxs-lookup"><span data-stu-id="fd2de-309">You can view the full source and any files not shown in this article on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span></span>

### <a name="event-handlers"></a><span data-ttu-id="fd2de-310">イベント ハンドラー</span><span class="sxs-lookup"><span data-stu-id="fd2de-310">Event Handlers</span></span>

<span data-ttu-id="fd2de-311">各入力要素の HTML DOM にイベント処理機能を追加する AngularJS ディレクティブの数があります。</span><span class="sxs-lookup"><span data-stu-id="fd2de-311">There are a number of directives in AngularJS that add event-handling capabilities to the input elements in your HTML DOM.</span></span> <span data-ttu-id="fd2de-312">以下の AngularJS に組み込まれているイベントの一覧を示します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-312">Below is a list of the events that are built into AngularJS.</span></span>

   * `ng-click`

   * `ng-dbl-click`

   * `ng-mousedown`

   * `ng-mouseup`

   * `ng-mouseenter`

   * `ng-mouseleave`

   * `ng-mousemove`

   * `ng-keydown`

   * `ng-keyup`

   * `ng-keypress`

   * `ng-change`

> [!NOTE]
> <span data-ttu-id="fd2de-313">使用して、独自のイベント ハンドラーを追加することができます、[カスタム ディレクティブの AngularJS 機能](https://docs.angularjs.org/guide/directive)します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-313">You can add your own event handlers using the [custom directives feature in AngularJS](https://docs.angularjs.org/guide/directive).</span></span>

<span data-ttu-id="fd2de-314">方法を見て、`ng-click`をイベントがワイヤード (有線)。</span><span class="sxs-lookup"><span data-stu-id="fd2de-314">Let's look at how the `ng-click` event is wired up.</span></span> <span data-ttu-id="fd2de-315">という名前の新しい JavaScript ファイルを作成する*eventHandlerController.js*、し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="fd2de-315">Create a new JavaScript file named *eventHandlerController.js*, and add the following to it:</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]

<span data-ttu-id="fd2de-316">新しい`sayName`関数`eventHandlerController`上記の 5 を行にします。</span><span class="sxs-lookup"><span data-stu-id="fd2de-316">Notice the new `sayName` function in `eventHandlerController` on line 5 above.</span></span> <span data-ttu-id="fd2de-317">すべてのメソッドの実行のようになりましたが表示されている JavaScript 通知、ようこそメッセージを持つユーザーにします。</span><span class="sxs-lookup"><span data-stu-id="fd2de-317">All the method is doing for now is showing a JavaScript alert to the user with a welcome message.</span></span>

<span data-ttu-id="fd2de-318">以下のビューは、AngularJS イベントにコント ローラー関数をバインドします。</span><span class="sxs-lookup"><span data-stu-id="fd2de-318">The view below binds a controller function to an AngularJS event.</span></span> <span data-ttu-id="fd2de-319">9 行目にボタンがある、 `ng-click` Angular ディレクティブが適用されています。</span><span class="sxs-lookup"><span data-stu-id="fd2de-319">Line 9 has a button on which the `ng-click` Angular directive has been applied.</span></span> <span data-ttu-id="fd2de-320">呼び出す、`sayName`にアタッチされている関数、`$scope`このビューに渡されるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="fd2de-320">It calls our `sayName` function, which is attached to the `$scope` object passed to this view.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]

<span data-ttu-id="fd2de-321">実行中の例では、ことを示します、コント ローラーの`sayName`関数は、ボタンがクリックされたときに自動的に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="fd2de-321">The running example demonstrates that the controller's `sayName` function is called automatically when the button is clicked.</span></span>

![Click イベント](angular/_static/events.png)

<span data-ttu-id="fd2de-323">AngularJS 組み込みのイベント ハンドラーのディレクティブの詳細については、必ずヘッドを移動する、[ドキュメント web サイト](https://docs.angularjs.org/api/ng/directive/ngClick)AngularJS のです。</span><span class="sxs-lookup"><span data-stu-id="fd2de-323">For more detail on AngularJS built-in event handler directives, be sure to head to the [documentation website](https://docs.angularjs.org/api/ng/directive/ngClick) of AngularJS.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fd2de-324">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="fd2de-324">Additional resources</span></span>

* [<span data-ttu-id="fd2de-325">Angular Docs</span><span class="sxs-lookup"><span data-stu-id="fd2de-325">Angular Docs</span></span>](https://docs.angularjs.org)

* [<span data-ttu-id="fd2de-326">Angular 2 情報</span><span class="sxs-lookup"><span data-stu-id="fd2de-326">Angular 2 Info</span></span>](https://angular.io/)
