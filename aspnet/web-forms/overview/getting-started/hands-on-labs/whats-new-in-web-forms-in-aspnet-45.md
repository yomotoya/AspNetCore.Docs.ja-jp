---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: ASP.NET 4.5 のフォームの Web の新機能 |Microsoft ドキュメント
author: rick-anderson
description: ASP.NET Web フォームの新しいバージョンには、多数のデータを操作するときに、ユーザー エクスペリエンスを向上させることに重点を置いた機能強化が導入されています。 以前のバージョンのしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: e230faac0dc81b67d74945dc98eee80f83205f65
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/18/2018
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a><span data-ttu-id="dc66b-104">Asp.net 4.5 Web フォームの新機能</span><span class="sxs-lookup"><span data-stu-id="dc66b-104">What's New in Web Forms in ASP.NET 4.5</span></span>
====================
<span data-ttu-id="dc66b-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="dc66b-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="dc66b-106">ASP.NET Web フォームの新しいバージョンには、多数のデータを操作するときに、ユーザー エクスペリエンスを向上させることに重点を置いた機能強化が導入されています。</span><span class="sxs-lookup"><span data-stu-id="dc66b-106">The new version of ASP.NET Web Forms introduces a number of improvements focused on improving user experience when working with data.</span></span>
> 
> <span data-ttu-id="dc66b-107">Web フォーム、データ バインディングを使用してオブジェクトのメンバーの値を出力する場合の以前のバージョンでは、Bind() または Eval() データ バインディング式を使用します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-107">In previous versions of Web Forms, when using data-binding to emit the value of an object member, you used the data-binding expressions Bind() or Eval().</span></span> <span data-ttu-id="dc66b-108">ASP.NET の新しいバージョンでは、データの種類、コントロールがしようとする新しい ItemType プロパティを使用してにバインドを宣言できます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-108">In the new version of ASP.NET, you are able to declare what type of data a control is going to be bound to by using a new ItemType property.</span></span> <span data-ttu-id="dc66b-109">このプロパティを設定すると、その厳密に型指定された変数を使用して、IntelliSense、メンバー ナビゲーション、およびコンパイル時のチェックなど、Visual Studio 開発環境のすべてのメリットを受信することが有効になります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-109">Setting this property will enable you to use a strongly-typed variable to receive the full benefits of the Visual Studio development experience, such as IntelliSense, member navigation, and compile-time checking.</span></span>
> 
> <span data-ttu-id="dc66b-110">データ バインド コントロールを今すぐも指定できます、独自のカスタム メソッドを選択すると、更新、削除して、データの挿入のページ コントロールと、アプリケーション ロジックの間の相互作用を簡略化すること。</span><span class="sxs-lookup"><span data-stu-id="dc66b-110">With the data-bound controls, you can now also specify your own custom methods for selecting, updating, deleting and inserting data, simplifying the interaction between the page controls and your application logic.</span></span> <span data-ttu-id="dc66b-111">さらに、メソッドの型パラメーターに直接ページからデータをマップすることを意味する ASP.NET には、モデル バインディング機能が追加されました。</span><span class="sxs-lookup"><span data-stu-id="dc66b-111">Additionally, model binding capabilities have been added to ASP.NET, which means you can map data from the page directly into method type parameters.</span></span>
> 
> <span data-ttu-id="dc66b-112">ユーザー入力を検証する必要がありますも Web フォームの最新バージョンを簡単にします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-112">Validating user input should also be easier with the latest version of Web Forms.</span></span> <span data-ttu-id="dc66b-113">検証属性を持つモデル クラスを今すぐ注釈を付けることができます、 **System.ComponentModel.DataAnnotations**名前空間と、すべてのサイトを制御する要求は、その情報を使用してユーザー入力を検証します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-113">You can now annotate your model classes with validation attributes from the **System.ComponentModel.DataAnnotations** namespace and request that all your site controls validate user input using that information.</span></span> <span data-ttu-id="dc66b-114">Web フォームでのクライアント側の検証はクライアント側コード クリーナーと控えめな JavaScript 機能を提供する、jQuery と統合されています。</span><span class="sxs-lookup"><span data-stu-id="dc66b-114">Client-side validation in Web Forms is now integrated with jQuery, providing cleaner client-side code and unobtrusive JavaScript features.</span></span>
> 
> <span data-ttu-id="dc66b-115">要求の検証 領域での機能強化に加えられたを選択して、アプリケーションの特定の部分で要求の検証をオフにするまたは無効化された要求データを読みやすきます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-115">In the request validation area, improvements have been made to make it easier to selectively turn off request validation for specific parts of your applications or read invalidated request data.</span></span>
> 
> <span data-ttu-id="dc66b-116">一部の機能強化に加えられた Web フォーム サーバー コントロールの HTML5 の新機能を利用します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-116">Some improvements have been made to Web Forms server controls to take advantage of new features of HTML5:</span></span>
> 
> - <span data-ttu-id="dc66b-117">TextBox コントロールの TextMode プロパティは、電子メール、datetime、およびなどのように、新しい HTML5 入力型をサポートするために更新されました。</span><span class="sxs-lookup"><span data-stu-id="dc66b-117">The TextMode property of the TextBox control has been updated to support the new HTML5 input types like email, datetime, and so on.</span></span>
> - <span data-ttu-id="dc66b-118">ファイルアップロード コントロールは、この機能は HTML5 のブラウザーから複数のファイルのアップロードをサポートします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-118">The FileUpload control now supports multiple file uploads from browsers that support this HTML5 feature.</span></span>
> - <span data-ttu-id="dc66b-119">検証コントロールは、今すぐサポート検証 HTML5 入力要素を制御します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-119">Validator controls now support validating HTML5 input elements.</span></span>
> - <span data-ttu-id="dc66b-120">ここで URL を表す属性を持つ新しい HTML5 要素をサポートして runat =&quot;サーバー&quot;です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-120">New HTML5 elements that have attributes that represent a URL now support runat=&quot;server&quot;.</span></span> <span data-ttu-id="dc66b-121">その結果、規則を使用できます ASP.NET URL のパスと同様に、~、アプリケーション ルートを表す演算子 (たとえば、&lt;ビデオ runat =&quot;サーバー&quot; src =&quot;~/myVideo.wmv&quot; &gt; &lt;/ビデオ&gt;)。</span><span class="sxs-lookup"><span data-stu-id="dc66b-121">As a result, you can use ASP.NET conventions in URL paths, like the ~ operator to represent the application root (for example, &lt;video runat=&quot;server&quot; src=&quot;~/myVideo.wmv&quot;&gt;&lt;/video&gt;).</span></span>
> - <span data-ttu-id="dc66b-122">UpdatePanel コントロールは、HTML5 ポストされた内容は、入力フィールドをサポートするために修正されています。</span><span class="sxs-lookup"><span data-stu-id="dc66b-122">The UpdatePanel control has been fixed to support posting HTML5 input fields.</span></span>
> 
> <span data-ttu-id="dc66b-123">公式の ASP.NET ポータルで ASP.NET WebForms 4.5 での新機能の例についてを見つけることができます: [ASP.NET 4.5 と Visual Studio 2012 の新](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)</span><span class="sxs-lookup"><span data-stu-id="dc66b-123">In the official ASP.NET portal you can find more examples of the new features in ASP.NET WebForms 4.5: [What's New in ASP.NET 4.5 and Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)</span></span>
> 
> <span data-ttu-id="dc66b-124">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-124">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="dc66b-125">目的</span><span class="sxs-lookup"><span data-stu-id="dc66b-125">Objectives</span></span>

<span data-ttu-id="dc66b-126">このハンズオン ラボでは学習する方法。</span><span class="sxs-lookup"><span data-stu-id="dc66b-126">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="dc66b-127">厳密に型指定されたデータ バインディング式を使用します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-127">Use strongly-typed data-binding expressions</span></span>
- <span data-ttu-id="dc66b-128">Web フォームの新しいモデル バインド機能を使用してください。</span><span class="sxs-lookup"><span data-stu-id="dc66b-128">Use new model binding features in Web Forms</span></span>
- <span data-ttu-id="dc66b-129">ページのデータを分離コードのメソッドにマップするための値プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-129">Use value providers for mapping page data to code-behind methods</span></span>
- <span data-ttu-id="dc66b-130">データ注釈を使用して、ユーザー入力の検証の</span><span class="sxs-lookup"><span data-stu-id="dc66b-130">Use Data Annotations for user input validation</span></span>
- <span data-ttu-id="dc66b-131">Web フォームでの jQuery を使用したクライアント側検証を unobstrusive advange になります</span><span class="sxs-lookup"><span data-stu-id="dc66b-131">Take advange of unobstrusive client-side validation with jQuery in Web Forms</span></span>
- <span data-ttu-id="dc66b-132">詳細な要求の検証を実装します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-132">Implement granular request validation</span></span>
- <span data-ttu-id="dc66b-133">非同期のページが Web フォームでの処理を実装します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-133">Implement asynchronous page processing in Web Forms</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="dc66b-134">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="dc66b-134">Prerequisites</span></span>

<span data-ttu-id="dc66b-135">このラボを完成させるのには、次の項目が必要です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-135">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="dc66b-136">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="dc66b-136">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="dc66b-137">セットアップ</span><span class="sxs-lookup"><span data-stu-id="dc66b-137">Setup</span></span>

<span data-ttu-id="dc66b-138">**コード スニペットをインストールします。**</span><span class="sxs-lookup"><span data-stu-id="dc66b-138">**Installing Code Snippets**</span></span>

<span data-ttu-id="dc66b-139">便宜上、このラボに沿ったを管理するコードの多くは、Visual Studio のコード スニペットとして利用できます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-139">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="dc66b-140">実行のコード スニペットをインストールする **.\Source\Setup\CodeSnippets.vsi**ファイル。</span><span class="sxs-lookup"><span data-stu-id="dc66b-140">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="dc66b-141">このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 c: を使用してコード スニペット](#AppendixC)&quot;です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-141">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="dc66b-142">演習</span><span class="sxs-lookup"><span data-stu-id="dc66b-142">Exercises</span></span>

<span data-ttu-id="dc66b-143">このハンズオン ラボには、次の実習が含まれます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-143">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="dc66b-144">手順 1: モデルの ASP.NET Web フォームでのバインディング</span><span class="sxs-lookup"><span data-stu-id="dc66b-144">Exercise 1: Model Binding in ASP.NET Web Forms</span></span>](#Exercise1)
2. [<span data-ttu-id="dc66b-145">手順 2: データの検証</span><span class="sxs-lookup"><span data-stu-id="dc66b-145">Exercise 2: Data Validation</span></span>](#Exercise2)
3. [<span data-ttu-id="dc66b-146">手順 3: 非同期ページの処理に ASP.NET Web フォームします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-146">Exercise 3: Asynchronous Page Processing in ASP.NET Web Forms</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="dc66b-147">各演習が組み込まれた、**終了**演習を完了した後に取得する必要があります、結果として得られるソリューションを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="dc66b-147">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="dc66b-148">演習では操作のヘルプを参照する必要がある場合は、このソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-148">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="dc66b-149">この演習を完了する時間を推定: **60 分**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-149">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a><span data-ttu-id="dc66b-150">手順 1: モデルの ASP.NET Web フォームでのバインディング</span><span class="sxs-lookup"><span data-stu-id="dc66b-150">Exercise 1: Model Binding in ASP.NET Web Forms</span></span>

<span data-ttu-id="dc66b-151">ASP.NET Web フォームの新しいバージョンでは、さまざまなデータを操作するときに、エクスペリエンスを向上させることに重点を置いた機能強化について説明します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-151">The new version of ASP.NET Web Forms introduces a number of enhancements focused on improving the experience when working with data.</span></span> <span data-ttu-id="dc66b-152">この演習では、厳密に型指定のデータ コントロールの概要について説明し、モデル バインディングします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-152">Throughout this exercise, you will learn about strongly typed data-controls and model binding.</span></span>

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a><span data-ttu-id="dc66b-153">タスク 1 の厳密に型指定のデータ バインディングの使用</span><span class="sxs-lookup"><span data-stu-id="dc66b-153">Task 1 - Using Strongly-Typed Data-Bindings</span></span>

<span data-ttu-id="dc66b-154">このタスクでは、新しい厳密に型指定されたバインド ASP.NET 4.5 で利用可能なことがわかります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-154">In this task, you will discover the new strongly-typed bindings available in ASP.NET 4.5.</span></span>

1. <span data-ttu-id="dc66b-155">開く、**開始**ソリューションにある**ソース/Ex1-ModelBinding/開始/** フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="dc66b-155">Open the **Begin** solution located at **Source/Ex1-ModelBinding/Begin/** folder.</span></span>

   1. <span data-ttu-id="dc66b-156">いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行前にします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-156">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="dc66b-157">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-157">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="dc66b-158">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-158">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="dc66b-159">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-159">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="dc66b-160">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="dc66b-160">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="dc66b-161">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-161">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="dc66b-162">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-162">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="dc66b-163">開く、 **Customers.aspx**ページ。</span><span class="sxs-lookup"><span data-stu-id="dc66b-163">Open the **Customers.aspx** page.</span></span> <span data-ttu-id="dc66b-164">メイン コントロールに番号なしのリストを置くし、各顧客の一覧を表示するための内部リピータ コントロールなどがあります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-164">Place an unnumbered list in the main control and include a repeater control inside for listing each customer.</span></span> <span data-ttu-id="dc66b-165">リピータ名を設定します**customersRepeater**次のコードに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-165">Set the repeater name to **customersRepeater** as shown in the following code.</span></span>

    <span data-ttu-id="dc66b-166">以前のバージョンの Web フォーム、データ バインディングをしているオブジェクトのメンバーの値を出力するデータ バインディングを使用するは式を使用するデータ バインド、Eval メソッドの呼び出しと共に渡すメンバーの名前を文字列として。</span><span class="sxs-lookup"><span data-stu-id="dc66b-166">In previous versions of Web Forms, when using data-binding to emit the value of a member on an object you're data-binding to, you would use a data-binding expression, along with a call to the Eval method, passing in the name of the member as a string.</span></span>

    <span data-ttu-id="dc66b-167">実行時に、Eval にこれらの呼び出しは現在バインドされているオブジェクトに対してリフレクションを使用して、指定した名前を持つメンバーの値を読み取るし、結果を HTML で表示します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-167">At runtime, these calls to Eval will use reflection against the currently bound object to read the value of the member with the given name, and display the result in the HTML.</span></span> <span data-ttu-id="dc66b-168">この方法では、任意、unshaped データに対してデータ バインドするには非常に簡単です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-168">This approach makes it very easy to data-bind against arbitrary, unshaped data.</span></span>

    <span data-ttu-id="dc66b-169">残念ながら、コンパイル時のチェック (定義へ移動) などのナビゲーションのサポート、メンバー名の IntelliSense をなど、Visual Studio で効果的に開発時の機能の多くが失われます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-169">Unfortunately, you lose many of the great development-time experience features in Visual Studio, including IntelliSense for member names, support for navigation (like Go To Definition), and compile-time checking.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. <span data-ttu-id="dc66b-170">開く、 **Customers.aspx.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="dc66b-170">Open the **Customers.aspx.cs** file.</span></span>
4. <span data-ttu-id="dc66b-171">次の追加ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-171">Add the following using statement.</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. <span data-ttu-id="dc66b-172">**ページ\_ロード**メソッド、顧客のリストを持つリピータを設定するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-172">In the **Page\_Load** method, add code to populate the repeater with the list of customers.</span></span>

    <span data-ttu-id="dc66b-173">(コード スニペットの*フォーム ラボ - Ex01 - バインド顧客のデータ ソースの Web*)</span><span class="sxs-lookup"><span data-stu-id="dc66b-173">(Code Snippet - *Web Forms Lab - Ex01 - Bind Customers Data Source*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    <span data-ttu-id="dc66b-174">ソリューションでは、CodeFirst と共に EntityFramework を使用して作成し、データベースにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-174">The solution uses EntityFramework together with CodeFirst to create and access the database.</span></span> <span data-ttu-id="dc66b-175">次のコードで、customersRepeater をデータベースからすべての顧客を返す具体化されたクエリにバインドされています。</span><span class="sxs-lookup"><span data-stu-id="dc66b-175">In the following code, the customersRepeater is bound to a materialized query that returns all the customers from the database.</span></span>
6. <span data-ttu-id="dc66b-176">キーを押して**f5 キーを押して**にソリューションを実行しに移動、**顧客**リピータの動作を表示するページ。</span><span class="sxs-lookup"><span data-stu-id="dc66b-176">Press **F5** to run the solution and go to the **Customers** page to see the repeater in action.</span></span> <span data-ttu-id="dc66b-177">ソリューションは CodeFirst を使用して、データベースが作成され、アプリケーションを実行するときに、ローカル SQL Express インスタンスに設定されます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-177">As the solution is using CodeFirst, the database will be created and populated in your local SQL Express instance when running the application.</span></span>

    <span data-ttu-id="dc66b-178">![リピータの顧客を一覧表示する](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "リピータと顧客の一覧を表示します。")</span><span class="sxs-lookup"><span data-stu-id="dc66b-178">![Listing the customers with a repeater](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "Listing the customers with a repeater")</span></span>

    <span data-ttu-id="dc66b-179">*リピータの顧客を一覧表示します。*</span><span class="sxs-lookup"><span data-stu-id="dc66b-179">*Listing the customers with a repeater*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dc66b-180">Visual Studio 2012、IIS Express は、既定 Web 開発サーバーです。</span><span class="sxs-lookup"><span data-stu-id="dc66b-180">In Visual Studio 2012, IIS Express is the default Web development server.</span></span>
7. <span data-ttu-id="dc66b-181">ブラウザーを終了し、Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-181">Close the browser and go back to Visual Studio.</span></span>
8. <span data-ttu-id="dc66b-182">厳密に型指定のバインディングを使用する実装を置き換えるようになりました。</span><span class="sxs-lookup"><span data-stu-id="dc66b-182">Now replace the implementation to use strongly typed bindings.</span></span> <span data-ttu-id="dc66b-183">開く、 **Customers.aspx**ページ使用して、新しい**ItemType**属性を設定する中継ぎ局で、**顧客**バインディングの種類として。</span><span class="sxs-lookup"><span data-stu-id="dc66b-183">Open the **Customers.aspx** page and use the new **ItemType** attribute in the repeater to set the **Customer** type as the binding type.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    <span data-ttu-id="dc66b-184">ItemType プロパティでは、データの種類コントロールにバインドするには、使用できるように厳密に型指定されたデータ バインド コントロール内のバインドを宣言することができます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-184">The ItemType property enables you to declare which type of data the control is going to be bound to and allows you to use strongly-typed binding inside the data-bound control.</span></span>
9. <span data-ttu-id="dc66b-185">次のコードでコンテンツの ItemTemplate を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-185">Replace the ItemTemplate content with the following code.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    <span data-ttu-id="dc66b-186">これらのアプローチの 1 つの欠点は、呼び出し Eval() および Bind() の遅延バインディングのプロパティの名前を表す文字列を渡す意味です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-186">One downside with the above approaches is that the calls to Eval() and Bind() are late-bound - meaning you pass strings to represent the property names.</span></span> <span data-ttu-id="dc66b-187">つまり、メンバー名、(定義へ移動) のようなコード ナビゲーションのサポートもコンパイル時チェックのサポートの Intellisense を取得しません。</span><span class="sxs-lookup"><span data-stu-id="dc66b-187">This means you don't get Intellisense for the member names, support for code navigation (like Go To Definition), nor compile-time checking support.</span></span>

    <span data-ttu-id="dc66b-188">ItemType プロパティを設定すると、データ バインディング式のスコープに、生成する 2 つの新しい型指定された変数:**項目**と**BindItem**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-188">Setting the ItemType property causes two new typed variables to be generated in the scope of the data-binding expressions: **Item** and **BindItem**.</span></span> <span data-ttu-id="dc66b-189">データ バインディング式でこれらの厳密に型指定された変数を使用し、Visual Studio 開発環境のすべてのメリットを取得できます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-189">You can use these strongly typed variables in the data-binding expressions and get the full benefits of the Visual Studio development experience.</span></span>

    <span data-ttu-id="dc66b-190">&quot; **:** &quot;式で使用が自動的に HTML エンコード (クロスサイト スクリプティング攻撃など) のセキュリティの問題を避けるために出力します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-190">The &quot;**:** &quot; used in the expression will automatically HTML-encode the output to avoid security issues (for example, cross-site scripting attacks).</span></span> <span data-ttu-id="dc66b-191">この表記できた .NET 4 以降の書き込み、応答が現在もデータ バインディング式で使用可能なです。</span><span class="sxs-lookup"><span data-stu-id="dc66b-191">This notation was available since .NET 4 for response writing, but now is also available in data-binding expressions.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dc66b-192">項目のメンバーは、一方向のバインドに対して機能します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-192">The Item member works for one-way binding.</span></span> <span data-ttu-id="dc66b-193">双方向のバインドを実行する場合、 **BindItem**メンバー。</span><span class="sxs-lookup"><span data-stu-id="dc66b-193">If you want to perform two-way binding use the **BindItem** member.</span></span>

    <span data-ttu-id="dc66b-194">![厳密に型指定されたバインディングでの IntelliSense サポート](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "厳密に型指定されたバインディングでの IntelliSense のサポート")</span><span class="sxs-lookup"><span data-stu-id="dc66b-194">![IntelliSense support in strongly-typed binding](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "IntelliSense support in strongly-typed binding")</span></span>

    <span data-ttu-id="dc66b-195">*厳密に型指定されたバインディングでの IntelliSense のサポート*</span><span class="sxs-lookup"><span data-stu-id="dc66b-195">*IntelliSense support in strongly-typed binding*</span></span>
10. <span data-ttu-id="dc66b-196">キーを押して**f5 キーを押して**にソリューションを実行し、期待どおりに変更を確認する [顧客] ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-196">Press **F5** to run the solution and go to the Customers page to make sure the changes work as expected.</span></span>

    <span data-ttu-id="dc66b-197">![顧客の詳細を一覧表示する](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "顧客の詳細を一覧表示します。")</span><span class="sxs-lookup"><span data-stu-id="dc66b-197">![Listing customer details](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "Listing customer details")</span></span>

    <span data-ttu-id="dc66b-198">*顧客の詳細を一覧表示します。*</span><span class="sxs-lookup"><span data-stu-id="dc66b-198">*Listing customer details*</span></span>
11. <span data-ttu-id="dc66b-199">ブラウザーを終了し、Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-199">Close the browser and go back to Visual Studio.</span></span>

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a><span data-ttu-id="dc66b-200">作業 2: Web フォームでのバインディング紹介モデル</span><span class="sxs-lookup"><span data-stu-id="dc66b-200">Task 2 - Introducing Model Binding in Web Forms</span></span>

<span data-ttu-id="dc66b-201">以前のバージョンの ASP.NET Web フォーム、取得して、データの更新の両方に双方向データ バインディングの実行に必要な場合にするに必要なデータ ソース オブジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-201">In previous versions of ASP.NET Web Forms, when you wanted to perform two-way data-binding, both retrieving and updating data, you needed to use a Data Source object.</span></span> <span data-ttu-id="dc66b-202">これは、オブジェクト データ ソース、SQL データ ソース、LINQ のデータ ソースなど。</span><span class="sxs-lookup"><span data-stu-id="dc66b-202">This could be an Object Data Source, a SQL Data Source, a LINQ Data Source and so on.</span></span> <span data-ttu-id="dc66b-203">ただし場合、シナリオでは、カスタム コードを必要なデータを処理するため、オブジェクト データ ソースを使用するために必要ないくつかの欠点になったこの</span><span class="sxs-lookup"><span data-stu-id="dc66b-203">However if your scenario required custom code for handling the data, you needed to use the Object Data Source and this brought some drawbacks.</span></span> <span data-ttu-id="dc66b-204">たとえば、複合型を避けるために必要なし、検証ロジックを実行するときに例外を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-204">For example, you needed to avoid complex types and you needed to handle exceptions when executing validation logic.</span></span>

<span data-ttu-id="dc66b-205">ASP.NET Web フォームの新しいバージョンでは、データ バインド コントロールは、モデル バインディングをサポートします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-205">In the new version of ASP.NET Web Forms the data-bound controls support model binding.</span></span> <span data-ttu-id="dc66b-206">つまり、できるし、指定することを選択、更新、挿入、分離コード ファイルまたは別のクラスからロジックを呼び出すにはデータ バインド コントロールで直接メソッドを削除します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-206">This means that you can specify select, update, insert and delete methods directly in the data-bound control to call logic from your code-behind file or from another class.</span></span>

<span data-ttu-id="dc66b-207">これについては、使用する、GridView、new を使用して、製品カテゴリを一覧表示する**SelectMethod**属性。</span><span class="sxs-lookup"><span data-stu-id="dc66b-207">To learn about this, you will use a GridView to list the product categories using the new **SelectMethod** attribute.</span></span> <span data-ttu-id="dc66b-208">この属性では、GridView のデータを取得する方法を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-208">This attribute enables you to specify a method for retrieving the GridView data.</span></span>

1. <span data-ttu-id="dc66b-209">開く、**繰り返し**ページし、が含まれて、 **GridView**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-209">Open the **Products.aspx** page and include a **GridView**.</span></span> <span data-ttu-id="dc66b-210">GridView は、厳密に型指定されたバインディングを使用し、並べ替えとページングを有効にする次のように構成します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-210">Configure the GridView as shown below to use strongly-typed bindings and enable sorting and paging.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. <span data-ttu-id="dc66b-211">使用して、新しい**SelectMethod**を呼び出す GridView を構成する属性、 **GetCategories**データを選択するメソッド。</span><span class="sxs-lookup"><span data-stu-id="dc66b-211">Use the new **SelectMethod** attribute to configure the GridView to call a **GetCategories** method to select the data.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. <span data-ttu-id="dc66b-212">開く、 **Products.aspx.cs**分離コード ファイルし、次の追加ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-212">Open the **Products.aspx.cs** code-behind file and add the following using statements.</span></span>

    <span data-ttu-id="dc66b-213">(コード スニペットの*フォーム ラボ - Ex01 - 名前空間を Web*)</span><span class="sxs-lookup"><span data-stu-id="dc66b-213">(Code Snippet - *Web Forms Lab - Ex01 - Namespaces*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. <span data-ttu-id="dc66b-214">プライベート メンバーを追加、**製品**クラスし、の新しいインスタンスを割り当てる**ProductsContext**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-214">Add a private member in the **Products** class and assign a new instance of **ProductsContext**.</span></span> <span data-ttu-id="dc66b-215">このプロパティでは、データベースに接続することができます、Entity Framework データ コンテキストを格納します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-215">This property will store the Entity Framework data context that enables you to connect to the database.</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. <span data-ttu-id="dc66b-216">作成、 **GetCategories** LINQ を使用してカテゴリの一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-216">Create a **GetCategories** method to retrieve the list of categories using LINQ.</span></span> <span data-ttu-id="dc66b-217">クエリが含まれます、**製品**プロパティ GridView は、各カテゴリの製品の金額を表示できるようにします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-217">The query will include the **Products** property so the GridView can show the amount of products for each category.</span></span> <span data-ttu-id="dc66b-218">メソッドが後でページのライフ サイクルに実行されるにクエリを表す生の IQueryable オブジェクトを返すことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dc66b-218">Notice that the method returns a raw IQueryable object that represent the query to be executed later on the page lifecycle.</span></span>

    <span data-ttu-id="dc66b-219">(コード スニペットの*Web フォーム ラボ - Ex01 - GetCategories*)</span><span class="sxs-lookup"><span data-stu-id="dc66b-219">(Code Snippet - *Web Forms Lab - Ex01 - GetCategories*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > <span data-ttu-id="dc66b-220">以前のバージョンの ASP.NET Web フォームを有効にする並べ替えとページング、オブジェクト データ ソースのコンテキスト内で、自分のリポジトリ ロジックを使用して必要なを独自のカスタム コードを記述し、必要なすべてのパラメーターを受信します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-220">In previous versions of ASP.NET Web Forms, enabling sorting and paging using your own repository logic within an Object Data Source context, required to write your own custom code and receive all the necessary parameters.</span></span> <span data-ttu-id="dc66b-221">ここで、データ バインディングのメソッドは、IQueryable を返すことができ、これは、クエリを表します、実行するには、まだ ASP.NET を守ることが適切な並べ替えを追加するクエリとページングのパラメーターを変更します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-221">Now, as the data-binding methods can return IQueryable and this represents a query still to be executed, ASP.NET can take care of modifying the query to add the proper sorting and paging parameters.</span></span>
6. <span data-ttu-id="dc66b-222">キーを押して**f5 キーを押して**サイトのデバッグを開始し、[製品] ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-222">Press **F5** to start debugging the site and go to the Products page.</span></span> <span data-ttu-id="dc66b-223">GridView に、メソッドによって返されるカテゴリが表示されているはずです。</span><span class="sxs-lookup"><span data-stu-id="dc66b-223">You should see that the GridView is populated with the categories returned by the GetCategories method.</span></span>

    <span data-ttu-id="dc66b-224">![モデル バインディングを使用して GridView を設定する](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "モデル バインディングを使用して GridView を設定します。")</span><span class="sxs-lookup"><span data-stu-id="dc66b-224">![Populating a GridView using model binding](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "Populating a GridView using model binding")</span></span>

    <span data-ttu-id="dc66b-225">*モデル バインディングを使用して GridView を設定します。*</span><span class="sxs-lookup"><span data-stu-id="dc66b-225">*Populating a GridView using model binding*</span></span>
7. <span data-ttu-id="dc66b-226">キーを押して**SHIFT**+**f5 キーを押して**デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-226">Press **SHIFT**+**F5** Stop debugging.</span></span>

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a><span data-ttu-id="dc66b-227">タスク 3 - モデル バインディング内の値プロバイダー</span><span class="sxs-lookup"><span data-stu-id="dc66b-227">Task 3 - Value Providers in Model Binding</span></span>

<span data-ttu-id="dc66b-228">モデル バインドは、データ バインド コントロールで直接データを操作するカスタム メソッドを指定することができますだけでなく、これらのメソッドのパラメーターに、ページからデータをマップすることもできます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-228">Model binding not only enables you to specify custom methods to work with your data directly in the data-bound control, but also allows you to map data from the page into parameters from these methods.</span></span> <span data-ttu-id="dc66b-229">メソッド パラメーターに値のデータ ソースを指定するのに値プロバイダー属性を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-229">On the method parameter, you can use value provider attributes to specify the value's data source.</span></span> <span data-ttu-id="dc66b-230">例えば:</span><span class="sxs-lookup"><span data-stu-id="dc66b-230">For example:</span></span>

- <span data-ttu-id="dc66b-231">ページ上のコントロール</span><span class="sxs-lookup"><span data-stu-id="dc66b-231">Controls on the page</span></span>
- <span data-ttu-id="dc66b-232">クエリ文字列の値</span><span class="sxs-lookup"><span data-stu-id="dc66b-232">Query string values</span></span>
- <span data-ttu-id="dc66b-233">データの表示</span><span class="sxs-lookup"><span data-stu-id="dc66b-233">View data</span></span>
- <span data-ttu-id="dc66b-234">セッション状態</span><span class="sxs-lookup"><span data-stu-id="dc66b-234">Session state</span></span>
- <span data-ttu-id="dc66b-235">クッキー</span><span class="sxs-lookup"><span data-stu-id="dc66b-235">Cookies</span></span>
- <span data-ttu-id="dc66b-236">ポストされたフォーム データ</span><span class="sxs-lookup"><span data-stu-id="dc66b-236">Posted form data</span></span>
- <span data-ttu-id="dc66b-237">ビューの状態</span><span class="sxs-lookup"><span data-stu-id="dc66b-237">View state</span></span>
- <span data-ttu-id="dc66b-238">カスタム値プロバイダーはもサポートされます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-238">Custom value providers are supported as well</span></span>

<span data-ttu-id="dc66b-239">ASP.NET MVC 4 を使用している場合は、モデル バインディングのサポートは似ていますがわかります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-239">If you have used ASP.NET MVC 4, you will notice the model binding support is similar.</span></span> <span data-ttu-id="dc66b-240">実際には、これらの機能が ASP.NET MVC から取得されに移動された、 **System.Web**も Web フォームで使用することであるアセンブリ。</span><span class="sxs-lookup"><span data-stu-id="dc66b-240">Indeed, these features were taken from ASP.NET MVC and moved into the **System.Web** assembly to be able to use them on Web Forms as well.</span></span>

<span data-ttu-id="dc66b-241">このタスクではモデル バインディングで filter パラメーターを受け取る各カテゴリの製品の量によって、結果をフィルター処理する GridView 更新されます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-241">In this task, you will update the GridView to filter its results by the amount of products for each category, receiving the filter parameter with model binding.</span></span>

1. <span data-ttu-id="dc66b-242">戻り、**繰り返し**ページ。</span><span class="sxs-lookup"><span data-stu-id="dc66b-242">Go back to the **Products.aspx** page.</span></span>
2. <span data-ttu-id="dc66b-243">GridView の上部に追加、**ラベル**と**ComboBox**を次に示すように各カテゴリの製品の数を選択します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-243">At the top of the GridView, add a **Label** and a **ComboBox** to select the number of products for each category as shown below.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. <span data-ttu-id="dc66b-244">追加、 **EmptyDataTemplate**製品数を選択したカテゴリが存在しない場合は、メッセージを表示する GridView にします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-244">Add an **EmptyDataTemplate** to the GridView to show a message when there are no categories with the selected number of products.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. <span data-ttu-id="dc66b-245">開く、 **Products.aspx.cs**分離コードと、次の追加ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-245">Open the **Products.aspx.cs** code-behind and add the following using statement.</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. <span data-ttu-id="dc66b-246">変更、 **GetCategories**を整数値を受け取るメソッド**minProductsCount**引数と、返される結果をフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-246">Modify the **GetCategories** method to receive an integer **minProductsCount** argument and filter the returned results.</span></span> <span data-ttu-id="dc66b-247">これを行うには、次のコードでメソッドを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-247">To do this, replace the method with the following code.</span></span>

    <span data-ttu-id="dc66b-248">(コード スニペットの*Web フォーム ラボ - Ex01 - GetCategories 2*)</span><span class="sxs-lookup"><span data-stu-id="dc66b-248">(Code Snippet - *Web Forms Lab - Ex01 - GetCategories 2*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    <span data-ttu-id="dc66b-249">新しい **[コントロール]** 属性を**minProductsCount**引数が ASP.NET ページにコントロールを使用して、その値を設定する必要が次のトピックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-249">The new **[Control]** attribute on the **minProductsCount** argument will let ASP.NET know its value must be populated using a control in the page.</span></span> <span data-ttu-id="dc66b-250">ASP.NET の引数 (minProductsCount) の名前に一致する任意のコントロールを検索および、必要なマッピングとコントロールの値を持つパラメーターを入力への変換を実行します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-250">ASP.NET will look for any control matching the name of the argument (minProductsCount) and perform the necessary mapping and conversion to fill the parameter with the control value.</span></span>

    <span data-ttu-id="dc66b-251">また、属性は、値を取得する場所からコントロールを指定することができますをオーバー ロードされたコンス トラクターを提供します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-251">Alternatively, the attribute provides an overloaded constructor that enables you to specify the control from where to get the value.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dc66b-252">データ バインディング機能の 1 つの目的は、ページの対話用に作成する必要があるコードの量を削減します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-252">One goal of the data-binding features is to reduce the amount of code that needs to be written for page interaction.</span></span> <span data-ttu-id="dc66b-253">別に、[管理] の値プロバイダーには、メソッドのパラメーターでその他のモデル バインド プロバイダーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-253">Apart from the [Control] value provider, you can use other model-binding providers in your method parameters.</span></span> <span data-ttu-id="dc66b-254">それらの一部は、タスクの概要に表示されます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-254">Some of them are listed in the task introduction.</span></span>
6. <span data-ttu-id="dc66b-255">キーを押して**f5 キーを押して**サイトのデバッグを開始し、[製品] ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-255">Press **F5** to start debugging the site and go to the Products page.</span></span> <span data-ttu-id="dc66b-256">ドロップダウン リストで製品の数を選択し、GridView の現在の更新方法に注意してください。</span><span class="sxs-lookup"><span data-stu-id="dc66b-256">Select a number of products in the drop-down list and notice how the GridView is now updated.</span></span>

    <span data-ttu-id="dc66b-257">![ドロップダウン リストの値を持つ GridView をフィルタ リング](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "GridView、ドロップダウン リストの値をフィルター処理")</span><span class="sxs-lookup"><span data-stu-id="dc66b-257">![Filtering the GridView with a drop-down list value](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "Filtering the GridView with a drop-down list value")</span></span>

    <span data-ttu-id="dc66b-258">*ドロップダウン リストの値を持つ GridView をフィルター処理*</span><span class="sxs-lookup"><span data-stu-id="dc66b-258">*Filtering the GridView with a drop-down list value*</span></span>
7. <span data-ttu-id="dc66b-259">デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-259">Stop debugging.</span></span>

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a><span data-ttu-id="dc66b-260">タスク 4 - フィルターのバインドを使用してモデル</span><span class="sxs-lookup"><span data-stu-id="dc66b-260">Task 4 - Using Model Binding for Filtering</span></span>

<span data-ttu-id="dc66b-261">このタスクは、2 番目の子を選択したカテゴリ内の製品を表示する GridView を追加します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-261">In this task, you will add a second, child GridView to show the products within the selected category.</span></span>

1. <span data-ttu-id="dc66b-262">開く、**繰り返し**ページおよび更新のカテゴリを選択 ボタンを自動生成する GridView。</span><span class="sxs-lookup"><span data-stu-id="dc66b-262">Open the **Products.aspx** page and update the categories GridView to auto-generate the Select button.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. <span data-ttu-id="dc66b-263">1 秒あたりの追加**GridView**という**productsGrid**下部にあります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-263">Add a second **GridView** named **productsGrid** at the bottom.</span></span> <span data-ttu-id="dc66b-264">設定、 **ItemType**に**WebFormsLab.Model.Product**、 **DataKeyNames**に**ProductId**と**SelectMethod**に**GetProducts**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-264">Set the **ItemType** to **WebFormsLab.Model.Product**, the **DataKeyNames** to **ProductId** and the **SelectMethod** to **GetProducts**.</span></span> <span data-ttu-id="dc66b-265">設定**AutoGenerateColumns**に**false** ProductId、ProductName、説明、および UnitPrice の列を追加します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-265">Set **AutoGenerateColumns** to **false** and add the columns for ProductId, ProductName, Description and UnitPrice.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. <span data-ttu-id="dc66b-266">開く、 **Products.aspx.cs**分離コード ファイル。</span><span class="sxs-lookup"><span data-stu-id="dc66b-266">Open the **Products.aspx.cs** code-behind file.</span></span> <span data-ttu-id="dc66b-267">実装、 **GetProducts** GridView のカテゴリから、カテゴリの ID を受信し、製品をフィルター処理するメソッド。</span><span class="sxs-lookup"><span data-stu-id="dc66b-267">Implement the **GetProducts** method to receive the category ID from the category GridView and filter the products.</span></span> <span data-ttu-id="dc66b-268">モデル バインディングで選択した行を使用してパラメーター値が設定されます、 **categoriesGrid**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-268">Model binding will set the parameter value using the selected row in the **categoriesGrid**.</span></span> <span data-ttu-id="dc66b-269">コントロールの名前と引数名が一致しないために、コントロール値プロバイダーでコントロールの名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-269">Since the argument name and control name do not match, you should specify the name of the control in the Control value provider.</span></span>

    <span data-ttu-id="dc66b-270">(コード スニペットの*Web フォーム ラボ - Ex01 - GetProducts*)</span><span class="sxs-lookup"><span data-stu-id="dc66b-270">(Code Snippet - *Web Forms Lab - Ex01 - GetProducts*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="dc66b-271">この方法では簡単に単位にこれらのメソッドをテストします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-271">This approach makes it easier to unit test these methods.</span></span> <span data-ttu-id="dc66b-272">Web フォームがない実行されている場合、単体テスト コンテキストで、[コントロール] 属性では、特定のアクションは実行しません。</span><span class="sxs-lookup"><span data-stu-id="dc66b-272">On a unit test context, where Web Forms is not executing, the [Control] attribute will not perform any specific action.</span></span>
4. <span data-ttu-id="dc66b-273">開く、**繰り返し**ページおよび GridView の製品を検索します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-273">Open the **Products.aspx** page and locate the products GridView.</span></span> <span data-ttu-id="dc66b-274">選択した製品を編集するためのリンクを表示する GridView の製品を更新します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-274">Update the products GridView to show a link for editing the selected product.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. <span data-ttu-id="dc66b-275">開く、 **ProductDetails.aspx**分離コード ページし、置換、 **SelectProduct**メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="dc66b-275">Open the **ProductDetails.aspx** page code-behind and replace the **SelectProduct** method with the following code.</span></span>

    <span data-ttu-id="dc66b-276">(コード スニペットの*Web フォーム ラボ - Ex01 - SelectProduct メソッド*)</span><span class="sxs-lookup"><span data-stu-id="dc66b-276">(Code Snippet - *Web Forms Lab - Ex01 - SelectProduct Method*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="dc66b-277">注意して、 **[QueryString]** 属性を使用して、クエリ文字列内の productId パラメーターからメソッド パラメーターを入力します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-277">Notice that the **[QueryString]** attribute is used to fill the method parameter from a productId parameter in the query string.</span></span>
6. <span data-ttu-id="dc66b-278">キーを押して**f5 キーを押して**サイトのデバッグを開始し、[製品] ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-278">Press **F5** to start debugging the site and go to the Products page.</span></span> <span data-ttu-id="dc66b-279">GridView のカテゴリから任意のカテゴリを選択し、GridView の製品が更新されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dc66b-279">Select any category from the categories GridView and notice that the products GridView is updated.</span></span>

    <span data-ttu-id="dc66b-280">![選択したカテゴリの製品を示す](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "選択したカテゴリの製品を表示")</span><span class="sxs-lookup"><span data-stu-id="dc66b-280">![Showing products from the selected category](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "Showing products from the selected category")</span></span>

    <span data-ttu-id="dc66b-281">*選択したカテゴリの製品を表示*</span><span class="sxs-lookup"><span data-stu-id="dc66b-281">*Showing products from the selected category*</span></span>
7. <span data-ttu-id="dc66b-282">クリックして、**ビュー** ProductDetails.aspx ページを開くに製品をリンクします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-282">Click the **View** link on a product to open the ProductDetails.aspx page.</span></span>

    <span data-ttu-id="dc66b-283">ページが、クエリ文字列から productId パラメーターを使用して、SelectMethod で製品を取得することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dc66b-283">Notice that the page is retrieving the product with the SelectMethod using the productId parameter from the query string.</span></span>

    <span data-ttu-id="dc66b-284">![製品の詳細を表示する](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "製品詳細を表示します。")</span><span class="sxs-lookup"><span data-stu-id="dc66b-284">![Viewing the product details](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "Viewing the product details")</span></span>

    <span data-ttu-id="dc66b-285">*製品の詳細を表示します。*</span><span class="sxs-lookup"><span data-stu-id="dc66b-285">*Viewing the product details*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dc66b-286">HTML の説明を入力する機能は、次の手順で実装されます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-286">The ability to type an HTML description will be implemented in the next exercise.</span></span>

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a><span data-ttu-id="dc66b-287">タスク 5 - 更新操作のためのバインドを使用してモデル</span><span class="sxs-lookup"><span data-stu-id="dc66b-287">Task 5 - Using Model Binding for Update Operations</span></span>

<span data-ttu-id="dc66b-288">前のタスクでは、主にデータを選択するモデル バインドを使用した、このタスクでは、更新操作で、モデル バインディングを使用する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-288">In the previous task, you have used model binding mainly for selecting data, in this task you will learn how to use model binding in update operations.</span></span>

<span data-ttu-id="dc66b-289">ユーザーがカテゴリを更新できるようにする GridView のカテゴリが更新されます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-289">You will update the categories GridView to let the user update categories.</span></span>

1. <span data-ttu-id="dc66b-290">開く、**繰り返し**ページおよび更新のカテゴリの [編集] ボタンを自動生成して、新しい GridView **UpdateMethod**を指定する属性、 **UpdateCategory**メソッドを選択した項目を更新します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-290">Open the **Products.aspx** page and update the categories GridView to auto-generate the Edit button and use the new **UpdateMethod** attribute to specify an **UpdateCategory** method to update the selected item.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    <span data-ttu-id="dc66b-291">GridView で DataKeyNames 属性を定義するモデル バインド オブジェクトを一意に識別するメンバーと、update メソッドを受け取るには、少なくともパラメーターであるため、します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-291">The DataKeyNames attribute in the GridView define which are the members that uniquely identify the model-bound object and therefore, which are the parameters the update method should at least receive.</span></span>
2. <span data-ttu-id="dc66b-292">開く、 **Products.aspx.cs**分離コード ファイルと実装、 **UpdateCategory**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="dc66b-292">Open the **Products.aspx.cs** code-behind file and implement the **UpdateCategory** method.</span></span> <span data-ttu-id="dc66b-293">メソッドは、現在のカテゴリを読み込む、GridView から値を設定し、カテゴリを更新するカテゴリ ID を受け取るはずです。</span><span class="sxs-lookup"><span data-stu-id="dc66b-293">The method should receive the category ID to load the current category, populate the values from the GridView and then update the category.</span></span>

    <span data-ttu-id="dc66b-294">(コード スニペットの*Web フォーム ラボ - Ex01 - UpdateCategory*)</span><span class="sxs-lookup"><span data-stu-id="dc66b-294">(Code Snippet - *Web Forms Lab - Ex01 - UpdateCategory*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    <span data-ttu-id="dc66b-295">新しい**TryUpdateModel**ページ クラスのメソッドは、ページにコントロールから値を使用して、モデル オブジェクトを配置します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-295">The new **TryUpdateModel** method in the Page class is responsible of populating the model object using the values from the controls in the page.</span></span> <span data-ttu-id="dc66b-296">この場合に編集されている現在の GridView 行から更新された値に置き換え、**カテゴリ**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="dc66b-296">In this case, it will replace the updated values from the current GridView row being edited into the **category** object.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dc66b-297">次の手順では、オブジェクトを編集するときに、ユーザーが入力したデータの検証 ModelState.IsValid の使用方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-297">The next exercise will explain the usage of the ModelState.IsValid for validating the data entered by the user when editing the object.</span></span>
3. <span data-ttu-id="dc66b-298">サイトを実行し、[製品] ページに進みます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-298">Run the site and go to the Products page.</span></span> <span data-ttu-id="dc66b-299">カテゴリを編集します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-299">Edit a category.</span></span> <span data-ttu-id="dc66b-300">新しい名前を入力し、をクリックして**更新**変更を保持します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-300">Type a new name and then click **Update** to persist the changes.</span></span>

    <span data-ttu-id="dc66b-301">![編集するカテゴリ](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "カテゴリを編集")</span><span class="sxs-lookup"><span data-stu-id="dc66b-301">![Editing categories](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "Editing categories")</span></span>

    <span data-ttu-id="dc66b-302">*編集するカテゴリ*</span><span class="sxs-lookup"><span data-stu-id="dc66b-302">*Editing categories*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a><span data-ttu-id="dc66b-303">手順 2: データの検証</span><span class="sxs-lookup"><span data-stu-id="dc66b-303">Exercise 2: Data Validation</span></span>

<span data-ttu-id="dc66b-304">この演習では、ASP.NET 4.5 で新しいデータの検証機能について学習します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-304">In this exercise, you will learn about the new data validation features in ASP.NET 4.5.</span></span> <span data-ttu-id="dc66b-305">Web フォームで新しい控えめな検証機能を確認します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-305">You will check out the new unobtrusive validation features in Web Forms.</span></span> <span data-ttu-id="dc66b-306">ユーザー入力の検証に、アプリケーション モデルのクラスにデータ注釈を使用して、最後に、オンまたはオフを個別のコントロール ページの要求の検証を有効にする方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-306">You will use data annotations in the application model classes for user input validation, and finally, you will learn how to turn on or off request validation to individual controls in a page.</span></span>

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a><span data-ttu-id="dc66b-307">タスク 1 - 控えめな検証</span><span class="sxs-lookup"><span data-stu-id="dc66b-307">Task 1 - Unobtrusive Validation</span></span>

<span data-ttu-id="dc66b-308">複雑なデータの検証コントロールを含むフォームは、ページで、コードの約 60% を表すことができますが多すぎる JavaScript コードを生成する傾向があります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-308">Forms with complex data including validators tend to generate too much JavaScript code in the page, which can represent about 60% of the code.</span></span> <span data-ttu-id="dc66b-309">控えめな検証が有効になっている、状態では、HTML コードは、クリーナーと整備に探します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-309">With unobtrusive validation enabled, your HTML code will look cleaner and tidier.</span></span>

<span data-ttu-id="dc66b-310">このセクションでは、両方の構成によって生成された HTML コードを比較する ASP.NET での控えめな検証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-310">In this section, you will enable unobtrusive validation in ASP.NET to compare the HTML code generated by both configurations.</span></span>

1. <span data-ttu-id="dc66b-311">開く**Visual Studio 2012**を開くと、**開始**にソリューションがある、 **Source\Ex2 Validation\Begin**このラボのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="dc66b-311">Open **Visual Studio 2012** and open the **Begin** solution located in the **Source\Ex2-Validation\Begin** folder of this lab.</span></span> <span data-ttu-id="dc66b-312">また、前述の手順から既存のソリューションで作業を続行できます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-312">Alternatively, you can continue working on your existing solution from the previous exercise.</span></span>

   1. <span data-ttu-id="dc66b-313">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-313">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="dc66b-314">ソリューション エクスプ ローラーで、これをクリックして、 **WebFormsLab**プロジェクト**NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-314">To do this, in the Solution Explorer, click the **WebFormsLab** project **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="dc66b-315">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-315">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="dc66b-316">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-316">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="dc66b-317">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="dc66b-317">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="dc66b-318">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-318">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="dc66b-319">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-319">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="dc66b-320">キーを押して**f5 キーを押して**web アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-320">Press **F5** to start the web application.</span></span> <span data-ttu-id="dc66b-321">[ページの顧客にしてください] をクリックして、**新しい顧客を追加**リンクします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-321">Go to the Customers page and click the **Add a New Customer** link.</span></span>
3. <span data-ttu-id="dc66b-322">ブラウザー ページで、右クリックし **ソースの表示**オプションを開くには、アプリケーションによって生成された HTML コード。</span><span class="sxs-lookup"><span data-stu-id="dc66b-322">Right-click on the browser page, and select **View Source** option to open the HTML code generated by the application.</span></span>

    <span data-ttu-id="dc66b-323">![ページの HTML コードを示す](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "ページの HTML コードの表示")</span><span class="sxs-lookup"><span data-stu-id="dc66b-323">![Showing the page HTML code](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "Showing the page HTML code")</span></span>

    <span data-ttu-id="dc66b-324">*ページの HTML コードの表示*</span><span class="sxs-lookup"><span data-stu-id="dc66b-324">*Showing the page HTML code*</span></span>
4. <span data-ttu-id="dc66b-325">ページのソース コードをスクロールし、ASP.NET が JavaScript コードとデータの検証コントロールがページで、検証を行い、エラー一覧を表示、挿入ことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dc66b-325">Scroll through the page source code and notice that ASP.NET has injected JavaScript code and data validators in the page to perform the validations and show the error list.</span></span>

    <span data-ttu-id="dc66b-326">![CustomerDetails ページでの JavaScript コードの検証](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "CustomerDetails ページの検証の JavaScript コード")</span><span class="sxs-lookup"><span data-stu-id="dc66b-326">![Validation JavaScript code in CustomerDetails page](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "Validation JavaScript code in CustomerDetails page")</span></span>

    <span data-ttu-id="dc66b-327">*CustomerDetails ページでの JavaScript コードの検証*</span><span class="sxs-lookup"><span data-stu-id="dc66b-327">*Validation JavaScript code in CustomerDetails page*</span></span>
5. <span data-ttu-id="dc66b-328">ブラウザーを終了し、Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-328">Close the browser and go back to Visual Studio.</span></span>
6. <span data-ttu-id="dc66b-329">今すぐ控えめな検証を有効になります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-329">Now you will enable unobtrusive validation.</span></span> <span data-ttu-id="dc66b-330">開いている**Web.Config**を見つけて選択**ValidationSettings:UnobtrusiveValidationMode**キー、 **AppSettings**セクション**です。**</span><span class="sxs-lookup"><span data-stu-id="dc66b-330">Open **Web.Config** and locate **ValidationSettings:UnobtrusiveValidationMode** key in the **AppSettings** section **.**</span></span> <span data-ttu-id="dc66b-331">キーの値を設定**WebForms**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-331">Set the key value to **WebForms**.</span></span>

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > <span data-ttu-id="dc66b-332">このプロパティを設定することもできます、 &quot;**ページ\_ロード**&quot;場合もいくつかのページに対してのみ控えめな検証を有効にするイベントです。</span><span class="sxs-lookup"><span data-stu-id="dc66b-332">You can also set this property in the &quot;**Page\_Load**&quot; event in case you want to enable Unobtrusive Validation only for some pages.</span></span>
7. <span data-ttu-id="dc66b-333">開いている**CustomerDetails.aspx**とキーを押します**f5 キーを押して**Web アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-333">Open **CustomerDetails.aspx** and press **F5** to start the Web application.</span></span>
8. <span data-ttu-id="dc66b-334">開くには、IE developer tools F12 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-334">Press the F12 key to open the IE developer tools.</span></span> <span data-ttu-id="dc66b-335">開発者用ツールが表示されたら、[スクリプト] タブを選択します。選択**CustomerDetails.aspx** take し、メニューからは、スクリプトがページでの jQuery の実行に必要なことに注意してくださいをローカル サイトからブラウザーに読み込まれています。</span><span class="sxs-lookup"><span data-stu-id="dc66b-335">Once the developer tools is open, select the script tab. Select **CustomerDetails.aspx** from the menu and take note that the scripts required to run jQuery on the page have been loaded into the browser from the local site.</span></span>

    <span data-ttu-id="dc66b-336">![ローカル IIS サーバーから直接ファイルを読み込み、jQuery JavaScript](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "ローカル IIS サーバーから直接ファイル jQuery JavaScript の読み込み")</span><span class="sxs-lookup"><span data-stu-id="dc66b-336">![Loading the jQuery JavaScript files directly from the local IIS server](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "Loading the jQuery JavaScript files directly from the local IIS server")</span></span>

    <span data-ttu-id="dc66b-337">*ローカル IIS サーバーから直接、jQuery JavaScript ファイルの読み込み*</span><span class="sxs-lookup"><span data-stu-id="dc66b-337">*Loading the jQuery JavaScript files directly from the local IIS server*</span></span>
9. <span data-ttu-id="dc66b-338">Visual Studio に戻るには、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-338">Close the browser to return to Visual Studio.</span></span> <span data-ttu-id="dc66b-339">開く、 **Site.Master**ファイルを再びと検索、 **ScriptManager**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-339">Open the **Site.Master** file again and locate the **ScriptManager**.</span></span> <span data-ttu-id="dc66b-340">属性を追加**EnableCdn** 、値を持つプロパティ**True**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-340">Add the attribute **EnableCdn** property with the value **True**.</span></span> <span data-ttu-id="dc66b-341">これで、ローカル サイトの URL ではなく、オンラインの URL から読み込まれる jQuery です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-341">This will force jQuery to be loaded from the online URL, not from the local site's URL.</span></span>
10. <span data-ttu-id="dc66b-342">開いている**CustomerDetails.aspx** Visual Studio でします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-342">Open **CustomerDetails.aspx** in Visual Studio.</span></span> <span data-ttu-id="dc66b-343">サイトを実行する F5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-343">Press the F5 key to run the site.</span></span> <span data-ttu-id="dc66b-344">Internet Explorer を開くと後、は、開発者ツールを開く F12 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-344">Once Internet Explorer opens, press the F12 key to open the developer tools.</span></span> <span data-ttu-id="dc66b-345">選択、**スクリプト**タブをクリックし、ドロップダウン リストを見て、します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-345">Select the **Script** tab, and then take a look at the drop-down list.</span></span> <span data-ttu-id="dc66b-346">JQuery JavaScript ファイルが読み込まれなくなりますされている、ローカル サイトからからではなくオンライン jQuery CDN に注意してください。</span><span class="sxs-lookup"><span data-stu-id="dc66b-346">Note the jQuery JavaScript files are no longer being loaded from the local site, but rather from the online jQuery CDN.</span></span>

    <span data-ttu-id="dc66b-347">![CDN からファイルを読み込み、jQuery JavaScript](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "CDN からファイルを読み込み、jQuery JavaScript")</span><span class="sxs-lookup"><span data-stu-id="dc66b-347">![Loading the jQuery JavaScript files from the CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "Loading the jQuery JavaScript files from the CDN")</span></span>

    <span data-ttu-id="dc66b-348">*CDN からの jQuery JavaScript ファイルの読み込み*</span><span class="sxs-lookup"><span data-stu-id="dc66b-348">*Loading the jQuery JavaScript files from the CDN*</span></span>
11. <span data-ttu-id="dc66b-349">ブラウザーで表示ソース オプションを使用して HTML ページのソース コードを開きます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-349">Open the HTML page source code again using the View source option in the browser.</span></span> <span data-ttu-id="dc66b-350">控えめな検証を有効にすると ASP.NET に置き換えること、挿入されたコードを JavaScript データ通知\*属性。</span><span class="sxs-lookup"><span data-stu-id="dc66b-350">Notice that by enabling the unobtrusive validation ASP.NET has replaced the injected JavaScript code with data- \*attributes.</span></span>

    <span data-ttu-id="dc66b-351">![控えめな検証コード](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "控えめな検証コード")</span><span class="sxs-lookup"><span data-stu-id="dc66b-351">![Unobtrusive validation code](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "Unobtrusive validation code")</span></span>

    <span data-ttu-id="dc66b-352">*控えめな検証コード*</span><span class="sxs-lookup"><span data-stu-id="dc66b-352">*Unobtrusive validation code*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dc66b-353">この例では、HTML および JavaScript 数行だけに概要データの注釈で検証がどのように簡略化されたかを確認しました。</span><span class="sxs-lookup"><span data-stu-id="dc66b-353">In this example, you saw how a validation summary with Data annotations was simplified to only a few HTML and JavaScript lines.</span></span> <span data-ttu-id="dc66b-354">以前は、控えめな検証なしで追加すると、複数の検証コントロールのサイズが大きく、JavaScript の検証コードが増えます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-354">Previously, without unobtrusive validation, the more validation controls you add, the bigger your JavaScript validation code will grow.</span></span>

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a><span data-ttu-id="dc66b-355">タスク 2 - データ注釈を使用してモデルを検証します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-355">Task 2 - Validating the Model with Data Annotations</span></span>

<span data-ttu-id="dc66b-356">ASP.NET 4.5 には、Web フォームのデータ注釈検証が導入されています。</span><span class="sxs-lookup"><span data-stu-id="dc66b-356">ASP.NET 4.5 introduces data annotations validation for Web Forms.</span></span> <span data-ttu-id="dc66b-357">各入力の検証コントロールはなく、ようになりましたモデル クラスの制約を定義し、すべての web アプリケーションでして使用できます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-357">Instead of having a validation control on each input, you can now define constraints in your model classes and use them across all your web application.</span></span> <span data-ttu-id="dc66b-358">このセクションでは、顧客の新規作成/編集フォームを検証するためデータ注釈を使用する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-358">In this section, you will learn how to use data annotations for validating a new/edit customer form.</span></span>

1. <span data-ttu-id="dc66b-359">開いている**CustomerDetail.aspx**ページ。</span><span class="sxs-lookup"><span data-stu-id="dc66b-359">Open **CustomerDetail.aspx** page.</span></span> <span data-ttu-id="dc66b-360">顧客名で 2 つ目の名前に注意してください、 **EditItemTemplate**と**後**のセクションでは、RequiredFieldValidator コントロールを使用して検証されます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-360">Notice that the customer first name and second name in the **EditItemTemplate** and **InsertItemTemplate** sections are validated using a RequiredFieldValidator controls.</span></span> <span data-ttu-id="dc66b-361">個々 の検証は、特定の条件に関連付けられている多くの検証コントロールをチェックする条件として含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-361">Each validator is associated to a particular condition, so you need to include as many validators as conditions to check.</span></span>
2. <span data-ttu-id="dc66b-362">Customer モデル クラスを検証するデータ注釈を追加します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-362">Add data annotations to validate the Customer model class.</span></span> <span data-ttu-id="dc66b-363">開いている**Customer.cs**クラス内で、**モデル**フォルダーと*装飾*データ注釈の属性を使用して各プロパティ。</span><span class="sxs-lookup"><span data-stu-id="dc66b-363">Open **Customer.cs** class in the **Model** folder and *decorate* each property using data annotation attributes.</span></span>

    <span data-ttu-id="dc66b-364">(コード スニペットの*フォーム ラボ - Ex02 - データ注釈を Web*)</span><span class="sxs-lookup"><span data-stu-id="dc66b-364">(Code Snippet - *Web Forms Lab - Ex02 - Data Annotations*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > <span data-ttu-id="dc66b-365">.NET framework 4.5 は、既存のデータの注釈のコレクションを拡張します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-365">.NET Framework 4.5 has extended the existing data annotation collection.</span></span> <span data-ttu-id="dc66b-366">使用できるデータ注釈をいくつか: [CreditCard]、[電話]、[EmailAddress] [範囲] [比較]、[Url] [FileExtensions]、[Required]、[キー]、[正規表現] です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-366">These are some of the data annotations you can use: [CreditCard], [Phone], [EmailAddress], [Range], [Compare], [Url], [FileExtensions], [Required], [Key], [RegularExpression].</span></span>
    > 
    > <span data-ttu-id="dc66b-367">いくつかの使用例:</span><span class="sxs-lookup"><span data-stu-id="dc66b-367">Some usage examples:</span></span>
    > 
    > [キー]: Specifies that an attribute is the unique identifier
[Key]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > <span data-ttu-id="dc66b-369">各属性内の独自のエラー メッセージを定義することもできます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-369">You can also define your own error messages within each attribute.</span></span>
3. <span data-ttu-id="dc66b-370">開いている**CustomerDetails.aspx**およびで EditItemTemplate と後のセクションで、フォーム ビュー コントロールの最初と最後の名前のフィールドのすべての RequiredFieldvalidators を削除します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-370">Open **CustomerDetails.aspx** and remove all the RequiredFieldvalidators for the first and last name fields in the in EditItemTemplate and InsertItemTemplate sections of the FormView control.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > <span data-ttu-id="dc66b-371">データ注釈を使用する利点の 1 つは、アプリケーション ページで、検証ロジックが重複していないことです。</span><span class="sxs-lookup"><span data-stu-id="dc66b-371">One advantage of using data annotations is that validation logic is not duplicated in your application pages.</span></span> <span data-ttu-id="dc66b-372">モデルでは、一度定義し、データを操作するすべてのアプリケーション ページで使用するとします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-372">You define it once in the model, and use it across all the application pages that manipulate data.</span></span>
4. <span data-ttu-id="dc66b-373">開いている**CustomerDetails.aspx**分離コードと SaveCustomer メソッドを検索します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-373">Open **CustomerDetails.aspx** code-behind and locate the SaveCustomer method.</span></span> <span data-ttu-id="dc66b-374">このメソッドは、新しい顧客を挿入するときに呼び出されるし、フォーム ビュー コントロールの値から顧客パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-374">This method is called when inserting a new customer and receives the Customer parameter from the FormView control values.</span></span> <span data-ttu-id="dc66b-375">ページ コントロールとパラメーター オブジェクトが発生間のマッピング、ASP.NET は実行時に、データ注釈をすべてに対してモデルの検証は属性を存在する場合に、検出されると、エラーが発生 ModelState ディクショナリを入力します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-375">When the mapping between the page controls and the parameter object occurrs, ASP.NET will execute the model validation against all the data annotation attributes and fill the ModelState dictionary with the errors encountered, if any.</span></span>

    <span data-ttu-id="dc66b-376">ModelState.IsValid には、検証を実行した後、モデルのすべてのフィールドが有効な場合は true を返しますのみです。</span><span class="sxs-lookup"><span data-stu-id="dc66b-376">The ModelState.IsValid will only return true if all the fields on your model are valid after performing the validation.</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. <span data-ttu-id="dc66b-377">追加、 **ValidationSummary**モデル エラーの一覧を表示する CustomerDetails ページの最後に制御します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-377">Add a **ValidationSummary** control at the end of the CustomerDetails page to show the list of model errors.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    <span data-ttu-id="dc66b-378">**ShowModelStateErrors** 、ValidationSummary に新しいプロパティをコントロールに設定した場合によっては、 **true**コントロールが ModelState ディクショナリからのエラーを表示します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-378">The **ShowModelStateErrors** is a new property on the ValidationSummary control that when set to **true**, the control will show the errors from the ModelState dictionary.</span></span> <span data-ttu-id="dc66b-379">これらのエラーは、データ注釈検証から取得されます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-379">These errors come from the data annotations validation.</span></span>
6. <span data-ttu-id="dc66b-380">キーを押して**f5 キーを押して**Web アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-380">Press **F5** to run the Web application.</span></span> <span data-ttu-id="dc66b-381">いくつかの不適切な値をフォームに入力し、をクリックして**保存**検証を実行します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-381">Complete the form with some erroneous values and click **Save** to execute validation.</span></span> <span data-ttu-id="dc66b-382">エラーの下部にある概要を確認します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-382">Notice the error summary at the bottom.</span></span>

    <span data-ttu-id="dc66b-383">![データ注釈を使用した検証](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "データ注釈を使用した検証")</span><span class="sxs-lookup"><span data-stu-id="dc66b-383">![Validation with Data Annotations](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "Validation with Data Annotations")</span></span>

    <span data-ttu-id="dc66b-384">*データ注釈を使用した検証*</span><span class="sxs-lookup"><span data-stu-id="dc66b-384">*Validation with Data Annotations*</span></span>

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a><span data-ttu-id="dc66b-385">タスク 3 - ModelState でカスタム データベース エラーの処理</span><span class="sxs-lookup"><span data-stu-id="dc66b-385">Task 3 - Handling Custom Database Errors with ModelState</span></span>

<span data-ttu-id="dc66b-386">以前のバージョンの Web フォームでは、長すぎる文字列または一意キー違反などのデータベース エラーの処理が係わりますリポジトリ コードで例外をスローし、分離コード エラーとして表示する上で、例外を処理します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-386">In previous version of Web Forms, handling database errors such as a too long string or a unique key violation could involve throwing exceptions in your repository code and then handling the exceptions on your code-behind to display an error.</span></span> <span data-ttu-id="dc66b-387">比較的単純な処理を行うには、大量のコードが必要です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-387">A great amount of code is required to do something relatively simple.</span></span>

<span data-ttu-id="dc66b-388">4.5 では Web フォーム、一貫した方法で、ページで、モデルとは、データベースからのエラーを表示する ModelState オブジェクトを使用できます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-388">In Web Forms 4.5, the ModelState object can be used to display the errors on the page, either from your model or from the database, in a consistent manner.</span></span>

<span data-ttu-id="dc66b-389">このタスクは正しくデータベース例外の処理および ModelState オブジェクトを使用して、ユーザーに適切なメッセージを表示するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-389">In this task, you will add code to properly handle database exceptions and show the appropriate message to the user using the ModelState object.</span></span>

1. <span data-ttu-id="dc66b-390">アプリケーションがまだ実行されているときに重複する値を使用してカテゴリの名前を更新してください。</span><span class="sxs-lookup"><span data-stu-id="dc66b-390">While the application is still running, try to update the name of a category using a duplicated value.</span></span>

    <span data-ttu-id="dc66b-391">![重複する名前のカテゴリを更新](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "重複する名前を持つ、カテゴリの更新")</span><span class="sxs-lookup"><span data-stu-id="dc66b-391">![Updating a category with a duplicated name](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "Updating a category with a duplicated name")</span></span>

    <span data-ttu-id="dc66b-392">*重複する名前を持つ、カテゴリの更新*</span><span class="sxs-lookup"><span data-stu-id="dc66b-392">*Updating a category with a duplicated name*</span></span>

    <span data-ttu-id="dc66b-393">ためにスローされる例外に注意してください、&quot;一意&quot;の制約、 **CategoryName**列です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-393">Notice that an exception is thrown due to the &quot;unique&quot; constraint of the **CategoryName** column.</span></span>

    <span data-ttu-id="dc66b-394">![重複するカテゴリ名については、例外](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "重複するカテゴリ名の例外")</span><span class="sxs-lookup"><span data-stu-id="dc66b-394">![Exception for duplicated category names](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "Exception for duplicated category names")</span></span>

    <span data-ttu-id="dc66b-395">*重複するカテゴリ名の例外*</span><span class="sxs-lookup"><span data-stu-id="dc66b-395">*Exception for duplicated category names*</span></span>
2. <span data-ttu-id="dc66b-396">デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-396">Stop debugging.</span></span> <span data-ttu-id="dc66b-397">**Products.aspx.cs**分離コード ファイル、更新、 **UpdateCategory** db によってスローされる例外を処理するメソッド。SaveChanges() メソッドの呼び出しを追加すると、エラー、 **ModelState**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="dc66b-397">In the **Products.aspx.cs** code-behind file, update the **UpdateCategory** method to handle the exceptions thrown by the db.SaveChanges() method call and add an error to the **ModelState** object.</span></span>

    <span data-ttu-id="dc66b-398">新しい**TryUpdateModel**メソッドは、ユーザーによって指定されたフォーム データを使用して、データベースから取得したカテゴリ オブジェクトを更新します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-398">The new **TryUpdateModel** method updates the category object retrieved from the database using the form data provided by the user.</span></span>

    <span data-ttu-id="dc66b-399">(コード スニペットの*Web フォーム ラボ - Ex02 - UpdateCategory ハンドル エラー*)</span><span class="sxs-lookup"><span data-stu-id="dc66b-399">(Code Snippet - *Web Forms Lab - Ex02 - UpdateCategory Handle Errors*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > <span data-ttu-id="dc66b-400">理想的には、DbUpdateException の原因を特定し、根本原因が一意キー制約の違反を確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-400">Ideally, you would have to identify the cause of the DbUpdateException and check if the root cause is the violation of a unique key constraint.</span></span>
3. <span data-ttu-id="dc66b-401">開いている**繰り返し**を追加し、 **ValidationSummary**モデル エラーの一覧を表示する GridView のカテゴリの下のコントロールです。</span><span class="sxs-lookup"><span data-stu-id="dc66b-401">Open **Products.aspx** and add a **ValidationSummary** control below the categories GridView to show the list of model errors.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. <span data-ttu-id="dc66b-402">サイトを実行し、[製品] ページに進みます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-402">Run the site and go to the Products page.</span></span> <span data-ttu-id="dc66b-403">重複する値を使用してカテゴリの名前を更新しようとしてください。</span><span class="sxs-lookup"><span data-stu-id="dc66b-403">Try to update the name of a category using an duplicated value.</span></span>

    <span data-ttu-id="dc66b-404">通知を例外が処理しで、エラー メッセージが表示されます、 **ValidationSummary**コントロール。</span><span class="sxs-lookup"><span data-stu-id="dc66b-404">Notice that the exception was handled and the error message appears in the **ValidationSummary** control.</span></span>

    <span data-ttu-id="dc66b-405">![カテゴリのエラーが重複して](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "カテゴリのエラーを重複しています")</span><span class="sxs-lookup"><span data-stu-id="dc66b-405">![Duplicated category error](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "Duplicated category error")</span></span>

    <span data-ttu-id="dc66b-406">*重複するカテゴリのエラー*</span><span class="sxs-lookup"><span data-stu-id="dc66b-406">*Duplicated category error*</span></span>

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a><span data-ttu-id="dc66b-407">タスク 4 - 要求の ASP.NET Web フォーム 4.5 での検証</span><span class="sxs-lookup"><span data-stu-id="dc66b-407">Task 4 - Request Validation in ASP.NET Web Forms 4.5</span></span>

<span data-ttu-id="dc66b-408">ASP.NET の要求の検証機能は、特定のレベルのクロス サイト スクリプト (XSS) 攻撃に対する既定の保護を提供します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-408">The request validation feature in ASP.NET provides a certain level of default protection against cross-site scripting (XSS) attacks.</span></span> <span data-ttu-id="dc66b-409">ASP.NET の以前のバージョンで要求の検証が既定で有効になっているし、ページ全体に対してのみ無効にする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-409">In previous versions of ASP.NET, request validation was enabled by default and could only be disabled for an entire page.</span></span> <span data-ttu-id="dc66b-410">ASP.NET Web フォームの新しいバージョンで今すぐ 1 つのコントロールの要求の検証を無効にする、限定的な要求の検証またはを実行できます (これを行う場合は慎重に!) が検証されていない要求のデータにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-410">With the new version of ASP.NET Web Forms you can now disable the request validation for a single control, perform lazy request validation or access un-validated request data (be careful if you do so!).</span></span>

1. <span data-ttu-id="dc66b-411">キーを押して**ctrl キーを押しながら f5 キーを押して**にデバッグを行わず、サイトを起動し、[製品] ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-411">Press **Ctrl+F5** to start the site without debugging and go to the Products page.</span></span> <span data-ttu-id="dc66b-412">カテゴリを選択し、クリックして、**編集**製品のいずれかのリンク。</span><span class="sxs-lookup"><span data-stu-id="dc66b-412">Select a category and then click the **Edit** link on any of the products.</span></span>
2. <span data-ttu-id="dc66b-413">危険性のあるコンテンツを含む、HTML タグを含むインスタンスの説明を入力します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-413">Type a description containing potentially dangerous content, for instance including HTML tags.</span></span> <span data-ttu-id="dc66b-414">要求の検証のためにスローされる例外の通知を実行します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-414">Take notice of the exception thrown due to the request validation.</span></span>

    <span data-ttu-id="dc66b-415">![危険性のあるコンテンツを持つ製品を編集](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "危険性のあるコンテンツを持つ製品の編集")</span><span class="sxs-lookup"><span data-stu-id="dc66b-415">![Editing a product with potentially dangerous content](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "Editing a product with potentially dangerous content")</span></span>

    <span data-ttu-id="dc66b-416">*製品は危険性のあるコンテンツの編集*</span><span class="sxs-lookup"><span data-stu-id="dc66b-416">*Editing a product with potentially dangerous content*</span></span>

    <span data-ttu-id="dc66b-417">![要求の検証のためにスローされる例外](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "要求の検証のためにスローされる例外")</span><span class="sxs-lookup"><span data-stu-id="dc66b-417">![Exception thrown due to request validation](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "Exception thrown due to request validation")</span></span>

    <span data-ttu-id="dc66b-418">*要求の検証のためにスローされる例外*</span><span class="sxs-lookup"><span data-stu-id="dc66b-418">*Exception thrown due to request validation*</span></span>
3. <span data-ttu-id="dc66b-419">ページを閉じるし、Visual Studio で、キーを押します**shift キーを押しながら f5 キーを押して**デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-419">Close the page and, in Visual Studio, press **SHIFT+F5** to stop debugging.</span></span>
4. <span data-ttu-id="dc66b-420">開く、 **ProductDetails.aspx**ページし、検索、**説明** テキスト ボックス。</span><span class="sxs-lookup"><span data-stu-id="dc66b-420">Open the **ProductDetails.aspx** page and locate the **Description** TextBox.</span></span>
5. <span data-ttu-id="dc66b-421">新しい追加**ValidateRequestMode**プロパティ テキスト ボックスをその値に設定し、**無効になっている**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-421">Add the new **ValidateRequestMode** property to the TextBox and set its value to **Disabled**.</span></span>

    <span data-ttu-id="dc66b-422">新しい**ValidateRequestMode**属性では、各コントロールに細かく要求の検証を無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-422">The new **ValidateRequestMode** attribute allows you to disable the request validation granularly on each control.</span></span> <span data-ttu-id="dc66b-423">これは、HTML コードが表示される可能性がありますが、残りのページの検証を維持する入力を使用する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-423">This is useful when you want to use an input that may receive HTML code, but want to keep the validation working for the rest of the page.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. <span data-ttu-id="dc66b-424">キーを押して**f5 キーを押して**web アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-424">Press **F5** to run the web application.</span></span> <span data-ttu-id="dc66b-425">製品の編集 ページを再度開くし、HTML タグを含む製品の説明を完了します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-425">Open the edit product page again and complete a product description including HTML tags.</span></span> <span data-ttu-id="dc66b-426">説明への HTML コンテンツを追加できるようになりましたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dc66b-426">Notice that you can now add HTML content to the description.</span></span>

    <span data-ttu-id="dc66b-427">![要求の検証、製品の説明を無効になっている](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "要求の検証、製品の説明を無効になっています")</span><span class="sxs-lookup"><span data-stu-id="dc66b-427">![Request validation disabled for the product description](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "Request validation disabled for the product description")</span></span>

    <span data-ttu-id="dc66b-428">*製品の説明を無効になっている要求の検証*</span><span class="sxs-lookup"><span data-stu-id="dc66b-428">*Request validation disabled for the product description*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dc66b-429">実稼働アプリケーションでは、唯一の安全な HTML タグが入力したかどうかを確認するユーザーが入力した HTML コードを校正する必要があります (たとえば、あるありません&lt;スクリプト&gt;タグ)。</span><span class="sxs-lookup"><span data-stu-id="dc66b-429">In a production application, you should sanitize the HTML code entered by the user to make sure only safe HTML tags are entered (for example, there are no &lt;script&gt; tags).</span></span> <span data-ttu-id="dc66b-430">これを行うには、使用することができます[Microsoft Web Protection ライブラリ](https://www.nuget.org/packages/AntiXSS)です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-430">To do this, you can use [Microsoft Web Protection Library](https://www.nuget.org/packages/AntiXSS).</span></span>
7. <span data-ttu-id="dc66b-431">製品を再度編集します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-431">Edit the product again.</span></span> <span data-ttu-id="dc66b-432">[名前] フィールドでの HTML コードを入力し、をクリックして**保存**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-432">Type HTML code in the Name field and click **Save**.</span></span> <span data-ttu-id="dc66b-433">[説明] フィールドの要求の検証を無効にのみ、re フィールドの残りの部分は、危険性のあるコンテンツに対してまだ検証に注意してください。</span><span class="sxs-lookup"><span data-stu-id="dc66b-433">Notice that Request Validation is only disabled for the Description field and the rest of the fields re still validated against the potentially dangerous content.</span></span>

    <span data-ttu-id="dc66b-434">![要求の残りのフィールドで有効になっている検証](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "要求の検証の残りのフィールドで有効になっています。")</span><span class="sxs-lookup"><span data-stu-id="dc66b-434">![Request validation enabled in the rest of the fields](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "Request validation enabled in the rest of the fields")</span></span>

    <span data-ttu-id="dc66b-435">*残りのフィールドで有効になっている要求の検証*</span><span class="sxs-lookup"><span data-stu-id="dc66b-435">*Request validation enabled in the rest of the fields*</span></span>

    <span data-ttu-id="dc66b-436">ASP.NET Web フォーム 4.5 には、遅延要求の検証を実行する新しい要求の検証モードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="dc66b-436">ASP.NET Web Forms 4.5 includes a new request validation mode to perform request validation lazily.</span></span> <span data-ttu-id="dc66b-437">要求の検証モードを設定**4.5**コードへのアクセスの場合は、 *Request.Form [&quot;キー&quot;]*、ASP.NET 4.5 の要求の検証はのみトリガー要求の検証フォーム コレクションでその要素を特定します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-437">With the request validation mode set to **4.5**, if a piece of code accesses *Request.Form[&quot;key&quot;]*, ASP.NET 4.5's request validation will only trigger request validation for that specific element in the form collection.</span></span>

    <span data-ttu-id="dc66b-438">さらに、ASP.NET 4.5 には Microsoft アンチ XSS ライブラリ v4.0 からコア エンコード ルーチンが含まれています。</span><span class="sxs-lookup"><span data-stu-id="dc66b-438">Additionally, ASP.NET 4.5 now includes core encoding routines from the Microsoft Anti-XSS Library v4.0.</span></span> <span data-ttu-id="dc66b-439">エンコードのルーチンは、新しいによって実装されるアンチ XSS *AntiXssEncoder*型については、新しい**System.Web.Security.AntiXss**名前空間。</span><span class="sxs-lookup"><span data-stu-id="dc66b-439">The Anti-XSS encoding routines are implemented by the new *AntiXssEncoder* type found in the new **System.Web.Security.AntiXss** namespace.</span></span> <span data-ttu-id="dc66b-440">**EncoderType**パラメーターを使用するように構成*AntiXssEncoder*、すべての出力を新しいエンコード ルーチンを使用して ASP.NET 内で自動的にエンコードします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-440">With the **encoderType** parameter configured to use *AntiXssEncoder*, all output encoding within ASP.NET automatically uses the new encoding routines.</span></span>
8. <span data-ttu-id="dc66b-441">ASP.NET 4.5 の要求の検証には、検証されていないデータにアクセスする要求もサポートしています。</span><span class="sxs-lookup"><span data-stu-id="dc66b-441">ASP.NET 4.5 request validation also supports un-validated access to request data.</span></span> <span data-ttu-id="dc66b-442">ASP.NET 4.5 に新しいコレクション プロパティの追加、 **HttpRequest**と呼ばれるオブジェクト**Unvalidated**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-442">ASP.NET 4.5 adds a new collection property to the **HttpRequest** object called **Unvalidated**.</span></span> <span data-ttu-id="dc66b-443">移動したとき**HttpRequest.Unvalidated**すべての要求のデータ、フォーム、クエリー文字列、Cookie、Url などの共通部分にアクセス権があります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-443">When you navigate into **HttpRequest.Unvalidated** you have access to all of the common pieces of request data, including Forms, QueryStrings, Cookies, URLs, and so on.</span></span>

    <span data-ttu-id="dc66b-444">![Request.Unvalidated オブジェクト](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated オブジェクト")</span><span class="sxs-lookup"><span data-stu-id="dc66b-444">![Request.Unvalidated object](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated object")</span></span>

    <span data-ttu-id="dc66b-445">*Request.Unvalidated オブジェクト*</span><span class="sxs-lookup"><span data-stu-id="dc66b-445">*Request.Unvalidated object*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dc66b-446">**注意して HttpRequest.Unvalidated プロパティを使用してください。**</span><span class="sxs-lookup"><span data-stu-id="dc66b-446">**Please use the HttpRequest.Unvalidated property with caution!**</span></span> <span data-ttu-id="dc66b-447">危険なテキストがないラウンドト リップと無防備な顧客に表示されることを確認要求の生データを慎重にカスタム検証を実行することを確認してください。</span><span class="sxs-lookup"><span data-stu-id="dc66b-447">Make sure you carefully perform custom validation on the raw request data to ensure that dangerous text is not round-tripped and rendered back to unsuspecting customers!</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a><span data-ttu-id="dc66b-448">手順 3: 非同期ページの処理に ASP.NET Web フォームします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-448">Exercise 3: Asynchronous Page Processing in ASP.NET Web Forms</span></span>

<span data-ttu-id="dc66b-449">この演習では、ASP.NET Web フォームの機能を処理する新しい非同期ページに導入される予定です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-449">In this exercise, you will be introduced to the new asynchronous page processing features in ASP.NET Web Forms.</span></span>

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a><span data-ttu-id="dc66b-450">タスク 1: 製品の更新の詳細をアップロードし、イメージを表示する ページ</span><span class="sxs-lookup"><span data-stu-id="dc66b-450">Task 1 - Updating the Product Details Page to Upload and Show Images</span></span>

<span data-ttu-id="dc66b-451">このタスクでは、ユーザーが製品の画像の URL を指定し、読み取り専用ビューで表示できるようにする製品の詳細ページが更新されます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-451">In this task, you will update the product details page to allow the user to specify an image URL for the product and display it in the read-only view.</span></span> <span data-ttu-id="dc66b-452">同期的にダウンロードして、指定されたイメージのローカル コピーを作成します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-452">You will create a local copy of the specified image by downloading it synchronously.</span></span> <span data-ttu-id="dc66b-453">次のタスクでは、非同期的に動作させるのには、この実装を更新します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-453">In the next task, you will update this implementation to make it work asynchronously.</span></span>

1. <span data-ttu-id="dc66b-454">開いている**Visual Studio 2012**と読み込み、**開始**にソリューションがある**Source\Ex3 Async\Begin**このラボのフォルダーからです。</span><span class="sxs-lookup"><span data-stu-id="dc66b-454">Open **Visual Studio 2012** and load the **Begin** solution located in **Source\Ex3-Async\Begin** from this lab's folder.</span></span> <span data-ttu-id="dc66b-455">代わりに、前の演習から既存のソリューションで作業を続行できます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-455">Alternatively, you can continue working on your existing solution from the previous exercises.</span></span>

   1. <span data-ttu-id="dc66b-456">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-456">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="dc66b-457">ソリューション エクスプ ローラーで、これをクリックして、 **WebFormsLab**プロジェクトし、選択**NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-457">To do this, in the Solution Explorer, click the **WebFormsLab** project and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="dc66b-458">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-458">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="dc66b-459">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-459">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="dc66b-460">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="dc66b-460">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="dc66b-461">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-461">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="dc66b-462">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-462">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="dc66b-463">開く、 **ProductDetails.aspx**ソース ページし、製品の画像を表示するフォーム ビューの ItemTemplate でフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-463">Open the **ProductDetails.aspx** page source and add a field in the FormView's ItemTemplate to show the product image.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. <span data-ttu-id="dc66b-464">FormView の EditTemplate で画像の URL を指定するフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-464">Add a field to specify the image URL in the FormView's EditTemplate.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. <span data-ttu-id="dc66b-465">開く、 **ProductDetails.aspx.cs**分離コード ファイルし、次の名前空間ディレクティブを追加します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-465">Open the **ProductDetails.aspx.cs** code-behind file and add the following namespace directives.</span></span>

    <span data-ttu-id="dc66b-466">(コード スニペットの*フォーム ラボ - Ex03 - 名前空間を Web*)</span><span class="sxs-lookup"><span data-stu-id="dc66b-466">(Code Snippet - *Web Forms Lab - Ex03 - Namespaces*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. <span data-ttu-id="dc66b-467">作成、 **UpdateProductImage**イメージを格納するリモート ローカル メソッド**イメージ**フォルダーと、新しいイメージの場所の値を持つ製品エンティティを更新します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-467">Create an **UpdateProductImage** method to store remote images in the local **Images** folder and update the product entity with the new image location value.</span></span>

    <span data-ttu-id="dc66b-468">(コード スニペットの*Web フォーム ラボ - Ex03 - UpdateProductImage*)</span><span class="sxs-lookup"><span data-stu-id="dc66b-468">(Code Snippet - *Web Forms Lab - Ex03 - UpdateProductImage*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. <span data-ttu-id="dc66b-469">更新プログラム、 **UpdateProduct**メソッドを呼び出す、 **UpdateProductImage**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="dc66b-469">Update the **UpdateProduct** method to call the **UpdateProductImage** method.</span></span>

    <span data-ttu-id="dc66b-470">(コード スニペットの*Web フォーム ラボ - Ex03 - UpdateProductImage 呼び出し*)</span><span class="sxs-lookup"><span data-stu-id="dc66b-470">(Code Snippet - *Web Forms Lab - Ex03 - UpdateProductImage Call*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. <span data-ttu-id="dc66b-471">アプリケーションを実行し、製品の画像をアップロードしようとしてください。</span><span class="sxs-lookup"><span data-stu-id="dc66b-471">Run the application and try to upload an image for a product.</span></span> <span data-ttu-id="dc66b-472">たとえば、Office クリップ アートから次の画像の URL を使用できます。 [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)</span><span class="sxs-lookup"><span data-stu-id="dc66b-472">For example, you can use the following image URL from Office Clip Arts: [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)</span></span>

    <span data-ttu-id="dc66b-473">![製品のイメージを設定](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "製品用のイメージの設定")</span><span class="sxs-lookup"><span data-stu-id="dc66b-473">![Setting an image for a product](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "Setting an image for a product")</span></span>

    <span data-ttu-id="dc66b-474">*製品のイメージの設定*</span><span class="sxs-lookup"><span data-stu-id="dc66b-474">*Setting an image for a product*</span></span>

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a><span data-ttu-id="dc66b-475">作業 2: 非同期製品の詳細 ページに処理を追加します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-475">Task 2 - Adding Asynchronous Processing to the Product Details Page</span></span>

<span data-ttu-id="dc66b-476">このタスクで非同期的に動作させるのに製品の詳細ページが更新されます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-476">In this task, you will update the product details page to make it work asynchronously.</span></span> <span data-ttu-id="dc66b-477">イメージのダウンロード プロセスの-実行時間の長いタスクは、ASP.NET 4.5 ページの非同期処理を使用して強化されます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-477">You will enhance a long running task - the image download process - by using ASP.NET 4.5 asynchronous page processing.</span></span>

<span data-ttu-id="dc66b-478">Web アプリケーションで非同期のメソッドは、ASP.NET スレッド プールの使用方法を最適化するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-478">Asynchronous methods in web applications can be used to optimize the way ASP.NET thread pools are used.</span></span> <span data-ttu-id="dc66b-479">Asp.net がありますを推進のスレッド プール内のスレッドの限られた数を要求するためとすべてのスレッドがビジー状態、、ASP.NET が起動新しい要求を拒否する、アプリケーションのエラー メッセージを送信され、サイトを使用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-479">In ASP.NET there are a limited number of threads in the thread pool for attending requests, thus, when all the threads are busy, ASP.NET starts to reject new requests, sends application error messages and makes your site unavailable.</span></span>

<span data-ttu-id="dc66b-480">Web サイトで時間のかかる操作が長時間割り当てられているスレッドを占有するために、非同期プログラミングの優れた候補です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-480">Time-consuming operations on your web site are great candidates for asynchronous programming because they occupy the assigned thread for a long time.</span></span> <span data-ttu-id="dc66b-481">これには、実行時間の長い要求、多数のさまざまな要素を含むページおよびオフライン操作、そのようなデータベースに対するクエリまたは外部 web サーバーへのアクセスを必要とするページが含まれます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-481">This includes long running requests, pages with lots of different elements and pages that require offline operations, such querying a database or accessing an external web server.</span></span> <span data-ttu-id="dc66b-482">利点は、ページの処理中に、これらの操作では、非同期メソッドを使用する場合、スレッドは解放され、スレッドに返されるプールし、新しいページ要求への参加に使用できます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-482">The advantage is that if you use asynchronous methods for these operations, while the page is processing, the thread is freed and returned to the thread pool and can be used to attend to a new page request.</span></span> <span data-ttu-id="dc66b-483">つまり、ページで、スレッド プールから 1 つのスレッドで処理を開始し、非同期処理が完了した後に、別の処理が完了することがあります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-483">This means, the page will start processing in one thread from the thread pool and might complete processing in a different one, after the async processing completes.</span></span>

1. <span data-ttu-id="dc66b-484">開く、 **ProductDetails.aspx**ページ。</span><span class="sxs-lookup"><span data-stu-id="dc66b-484">Open the **ProductDetails.aspx** page.</span></span> <span data-ttu-id="dc66b-485">追加、 **Async**属性、**ページ**要素に設定し、 **true**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-485">Add the **Async** attribute in the **Page** element and set it to **true**.</span></span> <span data-ttu-id="dc66b-486">この属性では、ASP.NET IHttpAsyncHandler インターフェイスを実装するように指示します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-486">This attribute tells ASP.NET to implement the IHttpAsyncHandler interface.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. <span data-ttu-id="dc66b-487">ページを実行しているスレッドの詳細を表示するページの下部にラベルを追加します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-487">Add a Label at the bottom of the page to show the details of the threads running the page.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. <span data-ttu-id="dc66b-488">開いている**ProductDetails.aspx.cs**し、次の名前空間ディレクティブを追加します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-488">Open up **ProductDetails.aspx.cs** and add the following namespace directives.</span></span>

    <span data-ttu-id="dc66b-489">(コード スニペットの*Web フォーム ラボ - Ex03 - 名前空間 2*)</span><span class="sxs-lookup"><span data-stu-id="dc66b-489">(Code Snippet - *Web Forms Lab - Ex03 - Namespaces 2*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. <span data-ttu-id="dc66b-490">変更、 **UpdateProductImage**非同期のタスクでイメージをダウンロードする方法です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-490">Modify the **UpdateProductImage** method to download the image with an asynchronous task.</span></span> <span data-ttu-id="dc66b-491">置換は、 **WebClient** **DownloadFile**メソッドを**DownloadFileTaskAsync**メソッドが含まれて、 **await**キーワード。</span><span class="sxs-lookup"><span data-stu-id="dc66b-491">You will replace the **WebClient** **DownloadFile** method with the **DownloadFileTaskAsync** method and include the **await** keyword.</span></span>

    <span data-ttu-id="dc66b-492">(コード スニペットの*Web フォーム ラボ - Ex03 - UpdateProductImage Async*)</span><span class="sxs-lookup"><span data-stu-id="dc66b-492">(Code Snippet - *Web Forms Lab - Ex03 - UpdateProductImage Async*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    <span data-ttu-id="dc66b-493">RegisterAsyncTask は、新しいページを別のスレッドで実行する非同期タスクを登録します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-493">The RegisterAsyncTask registers a new page asynchronous task to be executed in a different thread.</span></span> <span data-ttu-id="dc66b-494">Task (t) を実行すると、ラムダ式を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-494">It receives a lambda expression with the Task (t) to be executed.</span></span> <span data-ttu-id="dc66b-495">**Await**キーワード、 **DownloadFileTaskAsync**メソッドでは、メソッドの残りの部分を変換した後に非同期的に呼び出されるコールバックに、 **DownloadFileTaskAsync**メソッドが完了します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-495">The **await** keyword in the **DownloadFileTaskAsync** method converts the remainder of the method into a callback that is invoked asynchronously after the **DownloadFileTaskAsync** method has completed.</span></span> <span data-ttu-id="dc66b-496">ASP.NET は、HTTP 要求元のすべての値を自動的に管理して、メソッドの実行が再開されます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-496">ASP.NET will resume the execution of the method by automatically maintaining all the HTTP request original values.</span></span> <span data-ttu-id="dc66b-497">.NET 4.5 で、新しい非同期プログラミング モデルを使用すると、似て同期コードは、非同期コードを記述して、コンパイラがコールバック関数や継続コードの複雑さの一部を処理できます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-497">The new asynchronous programming model in .NET 4.5 enables you to write asynchronous code that looks very much like synchronous code, and let the compiler handle the complications of callback functions or continuation code.</span></span>
    > [!NOTE]
    > <span data-ttu-id="dc66b-498">RegisterAsyncTask と PageAsyncTask いた使用可能な .NET 2.0 以降。</span><span class="sxs-lookup"><span data-stu-id="dc66b-498">RegisterAsyncTask and PageAsyncTask were already available since .NET 2.0.</span></span> <span data-ttu-id="dc66b-499">Await キーワードは、.NET 4.5 の非同期プログラミング モデルから新しい .NET WebClient オブジェクトから新しい TaskAsync メソッドと一緒に使用することができます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-499">The await keyword is new from the .NET 4.5 asynchronous programming model and can be used together with the new TaskAsync methods from the .NET WebClient object.</span></span>
5. <span data-ttu-id="dc66b-500">コードが開始され、実行が完了スレッドを表示するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-500">Add code to display the threads on which the code started and finished executing.</span></span> <span data-ttu-id="dc66b-501">これを行うには、更新、 **UpdateProductImage**メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="dc66b-501">To do this, update the **UpdateProductImage** method with the following code.</span></span>

    <span data-ttu-id="dc66b-502">(コード スニペットの*Web フォーム ラボ - Ex03 - 表示スレッド*)</span><span class="sxs-lookup"><span data-stu-id="dc66b-502">(Code Snippet - *Web Forms Lab - Ex03 - Show threads*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. <span data-ttu-id="dc66b-503">開く web サイトの**Web.config**ファイル。</span><span class="sxs-lookup"><span data-stu-id="dc66b-503">Open the web site's **Web.config** file.</span></span> <span data-ttu-id="dc66b-504">次の appSetting 変数を追加します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-504">Add the following appSetting variable.</span></span>

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. <span data-ttu-id="dc66b-505">キーを押して**f5 キーを押して**にアプリケーションを実行し、製品の画像をアップロードします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-505">Press **F5** to run the application and upload an image for the product.</span></span> <span data-ttu-id="dc66b-506">ここで開始および終了コードが異なる場合があります、スレッド ID に注意してください。</span><span class="sxs-lookup"><span data-stu-id="dc66b-506">Notice the threads ID where the code started and finished may be different.</span></span> <span data-ttu-id="dc66b-507">これは、ASP.NET スレッドのプールから別のスレッドで非同期タスクが実行されるためです。</span><span class="sxs-lookup"><span data-stu-id="dc66b-507">This is because asynchronous tasks run on a separate thread from ASP.NET thread pool.</span></span> <span data-ttu-id="dc66b-508">タスクが完了したら、ASP.NET は、キューに戻し、タスクを配置し、使用可能なスレッドのいずれかを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-508">When the task completes, ASP.NET puts the task back in the queue and assigns any of the available threads.</span></span>

    <span data-ttu-id="dc66b-509">![イメージを非同期的にダウンロード](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "イメージを非同期的にダウンロードします。")</span><span class="sxs-lookup"><span data-stu-id="dc66b-509">![Downloading an image asynchronously](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "Downloading an image asynchronously")</span></span>

    <span data-ttu-id="dc66b-510">*イメージを非同期的にダウンロードします。*</span><span class="sxs-lookup"><span data-stu-id="dc66b-510">*Downloading an image asynchronously*</span></span>

> [!NOTE]
> <span data-ttu-id="dc66b-511">Azure の次にこのアプリケーションを展開するさらに、[付録 b: 公開 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-511">Additionally, you can deploy this application to Azure following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="dc66b-512">まとめ</span><span class="sxs-lookup"><span data-stu-id="dc66b-512">Summary</span></span>

<span data-ttu-id="dc66b-513">このハンズオン ラボでは、次の概念が対処して示されています。</span><span class="sxs-lookup"><span data-stu-id="dc66b-513">In this hands-on lab, the following concepts have been addressed and demonstrated:</span></span>

- <span data-ttu-id="dc66b-514">厳密に型指定されたデータ バインディング式を使用します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-514">Use strongly-typed data-binding expressions</span></span>
- <span data-ttu-id="dc66b-515">Web フォームの新しいモデル バインド機能を使用してください。</span><span class="sxs-lookup"><span data-stu-id="dc66b-515">Use new model binding features in Web Forms</span></span>
- <span data-ttu-id="dc66b-516">ページのデータを分離コードのメソッドにマップするための値プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-516">Use value providers for mapping page data to code-behind methods</span></span>
- <span data-ttu-id="dc66b-517">データ注釈を使用して、ユーザー入力の検証の</span><span class="sxs-lookup"><span data-stu-id="dc66b-517">Use Data Annotations for user input validation</span></span>
- <span data-ttu-id="dc66b-518">Web フォームでの jQuery を使用したクライアント側検証を unobstrusive advange になります</span><span class="sxs-lookup"><span data-stu-id="dc66b-518">Take advange of unobstrusive client-side validation with jQuery in Web Forms</span></span>
- <span data-ttu-id="dc66b-519">詳細な要求の検証を実装します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-519">Implement granular request validation</span></span>
- <span data-ttu-id="dc66b-520">非同期のページが Web フォームでの処理を実装します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-520">Implement asynchronous page processing in Web Forms</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="dc66b-521">付録 a: をインストールする Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="dc66b-521">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="dc66b-522">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="dc66b-522">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="dc66b-523">次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-523">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="dc66b-524">移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-524">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="dc66b-525">または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; <em>Visual Studio Express 2012 for Web と Azure SDK</em>&quot;です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-525">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="dc66b-526">をクリックして**を今すぐインストール**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-526">Click on **Install Now**.</span></span> <span data-ttu-id="dc66b-527">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-527">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="dc66b-528">1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-528">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="dc66b-529">![Visual Studio Express インストール](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="dc66b-529">![Install Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="dc66b-530">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="dc66b-530">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="dc66b-531">すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-531">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    <span data-ttu-id="dc66b-533">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="dc66b-533">*Accepting the license terms*</span></span>
5. <span data-ttu-id="dc66b-534">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-534">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    <span data-ttu-id="dc66b-536">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="dc66b-536">*Installation progress*</span></span>
6. <span data-ttu-id="dc66b-537">インストールが完了したらをクリックして**完了**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-537">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    <span data-ttu-id="dc66b-539">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="dc66b-539">*Installation completed*</span></span>
7. <span data-ttu-id="dc66b-540">をクリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-540">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="dc66b-541">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-541">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web タイルを](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    <span data-ttu-id="dc66b-543">*VS Express Web タイルを*</span><span class="sxs-lookup"><span data-stu-id="dc66b-543">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="dc66b-544">付録 b: Web Deploy を使用して ASP.NET MVC 4 アプリケーションを発行します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-544">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="dc66b-545">この付録では、Azure ポータルから新しい web サイトを作成し、Azure によって提供される Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを公開する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-545">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="dc66b-546">タスク 1 - Azure ポータルから新しい Web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-546">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="dc66b-547">移動して、 [Azure Management Portal](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-547">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dc66b-548">Azure 無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増大に応じて拡大縮小できます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-548">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="dc66b-549">サインアップする[ここ](http://aka.ms/aspnet-hol-azure)です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-549">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="dc66b-550">![Windows Azure ポータルにログオンする](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Windows Azure ポータルにログオンします。")</span><span class="sxs-lookup"><span data-stu-id="dc66b-550">![Log on to Windows Azure portal](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="dc66b-551">*ポータルにログインします*</span><span class="sxs-lookup"><span data-stu-id="dc66b-551">*Log on to the Portal*</span></span>
2. <span data-ttu-id="dc66b-552">をクリックして**新規**コマンド バーでします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-552">Click **New** on the command bar.</span></span>

    <span data-ttu-id="dc66b-553">![新しい Web サイトを作成する](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="dc66b-553">![Creating a new Web Site](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="dc66b-554">*新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="dc66b-554">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="dc66b-555">をクリックして**コンピューティング** | **Web サイト**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-555">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="dc66b-556">選択し、**簡易作成**オプション。</span><span class="sxs-lookup"><span data-stu-id="dc66b-556">Then select **Quick Create** option.</span></span> <span data-ttu-id="dc66b-557">新しい web サイトの利用可能な URL を指定して、をクリックして**Web サイトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-557">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dc66b-558">Azure は、制御および管理できる、クラウドで実行されている web アプリケーションのホストです。</span><span class="sxs-lookup"><span data-stu-id="dc66b-558">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="dc66b-559">簡易作成 オプションでは、ポータルの外部から Azure に完了した web アプリケーションを展開することができます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-559">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="dc66b-560">データベースを設定するための手順は含まれません。</span><span class="sxs-lookup"><span data-stu-id="dc66b-560">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="dc66b-561">![簡易作成を使用して新しい Web サイトを作成する](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "簡易作成を使用して新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="dc66b-561">![Creating a new Web Site using Quick Create](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="dc66b-562">*簡易作成を使用して新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="dc66b-562">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="dc66b-563">新しいまで待つ**Web サイト**を作成します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-563">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="dc66b-564">Web サイトを作成した後は、下のリンクをクリックして、 **URL**列です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-564">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="dc66b-565">新しい Web サイトが動作していることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="dc66b-565">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="dc66b-566">![新しい web サイトを参照して](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "新しい web サイトを参照")</span><span class="sxs-lookup"><span data-stu-id="dc66b-566">![Browsing to the new web site](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="dc66b-567">*新しい web サイトを参照*</span><span class="sxs-lookup"><span data-stu-id="dc66b-567">*Browsing to the new web site*</span></span>

    <span data-ttu-id="dc66b-568">![Web サイトを実行している](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "を実行している Web サイト")</span><span class="sxs-lookup"><span data-stu-id="dc66b-568">![Web site running](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "Web site running")</span></span>

    <span data-ttu-id="dc66b-569">*実行している web サイト*</span><span class="sxs-lookup"><span data-stu-id="dc66b-569">*Web site running*</span></span>
6. <span data-ttu-id="dc66b-570">ポータルに戻るし、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。</span><span class="sxs-lookup"><span data-stu-id="dc66b-570">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="dc66b-571">![Web サイトの管理ページを開く](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "web サイトの管理ページを開く")</span><span class="sxs-lookup"><span data-stu-id="dc66b-571">![Opening the web site management pages](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="dc66b-572">*Web サイトの管理ページを開く*</span><span class="sxs-lookup"><span data-stu-id="dc66b-572">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="dc66b-573">**ダッシュ ボード**] ページの [、**クイック グランス**セクションで、をクリックして、**発行プロファイルのダウンロード**リンクします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-573">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dc66b-574">*発行プロファイル*すべての有効なパブリケーション メソッドごとに Azure に web アプリケーションを発行するために必要な情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="dc66b-574">The *publish profile* contains all of the information required to publish a web application to Azure for each enabled publication method.</span></span> <span data-ttu-id="dc66b-575">発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベース文字列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="dc66b-575">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="dc66b-576">**Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートをこれらのプログラムの構成を自動化するプロファイルの発行Azure に web アプリケーションを公開します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-576">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="dc66b-577">![発行プロファイルを web サイトをダウンロードする](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "発行プロファイルを web サイトをダウンロードします。")</span><span class="sxs-lookup"><span data-stu-id="dc66b-577">![Downloading the web site publish profile](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="dc66b-578">*発行プロファイルを Web サイトをダウンロードします。*</span><span class="sxs-lookup"><span data-stu-id="dc66b-578">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="dc66b-579">既知の場所に発行プロファイル ファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-579">Download the publish profile file to a known location.</span></span> <span data-ttu-id="dc66b-580">さらにこの練習では、このファイルを使用して、Visual Studio から Azure に web アプリケーションを発行する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-580">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="dc66b-581">![発行プロファイル ファイルを保存](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "発行プロファイルの保存")</span><span class="sxs-lookup"><span data-stu-id="dc66b-581">![Saving the publish profile file](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "Saving the publish profile")</span></span>

    <span data-ttu-id="dc66b-582">*発行プロファイル ファイルを保存します。*</span><span class="sxs-lookup"><span data-stu-id="dc66b-582">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="dc66b-583">タスク 2 - データベース サーバーの構成</span><span class="sxs-lookup"><span data-stu-id="dc66b-583">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="dc66b-584">アプリケーションが SQL Server を使用する場合は、データベースの SQL データベース サーバーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-584">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="dc66b-585">SQL Server を使用しない簡単なアプリケーションを展開する場合は、このタスクをスキップする場合があります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-585">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="dc66b-586">SQL データベース サーバーは、アプリケーション データベースを格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-586">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="dc66b-587">SQL データベース サーバーを表示するには、Azure 管理ポータルで自分のサブスクリプションから**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**.</span><span class="sxs-lookup"><span data-stu-id="dc66b-587">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="dc66b-588">使用して 1 つを作成するには作成されたサーバーがない、**追加**コマンド バーのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-588">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="dc66b-589">注意してください、**サーバー名および URL、管理者のログイン名とパスワード**、次のタスクで使用することができます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-589">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="dc66b-590">データベースを作成しない、まだと後の段階で作成されます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-590">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="dc66b-591">![SQL データベース サーバーのダッシュ ボード](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "SQL データベース サーバーのダッシュ ボード")</span><span class="sxs-lookup"><span data-stu-id="dc66b-591">![SQL Database Server Dashboard](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="dc66b-592">*SQL データベース サーバーのダッシュ ボード*</span><span class="sxs-lookup"><span data-stu-id="dc66b-592">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="dc66b-593">次のタスクでは、サーバーの一覧に、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-593">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="dc66b-594">実行するには、クリックして**構成**から IP アドレスを選択する**現在のクライアント IP アドレス**に貼り付けます、**開始 IP アドレス**と**終了IPアドレス**のテキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png)ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-594">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) button.</span></span>

    ![クライアントの IP アドレスを追加します。](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    <span data-ttu-id="dc66b-596">*クライアントの IP アドレスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="dc66b-596">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="dc66b-597">1 回、**クライアント IP アドレス**が許可される IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-597">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![変更を確認します。](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    <span data-ttu-id="dc66b-599">*変更を確認します。*</span><span class="sxs-lookup"><span data-stu-id="dc66b-599">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="dc66b-600">タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="dc66b-600">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="dc66b-601">ASP.NET MVC 4 ソリューションに戻ります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-601">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="dc66b-602">**ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-602">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="dc66b-603">![アプリケーションの発行](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="dc66b-603">![Publishing the Application](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "Publishing the Application")</span></span>

    <span data-ttu-id="dc66b-604">*Web サイトの発行*</span><span class="sxs-lookup"><span data-stu-id="dc66b-604">*Publishing the web site*</span></span>
2. <span data-ttu-id="dc66b-605">最初の作業を保存した発行プロファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-605">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="dc66b-606">![発行プロファイルをインポートする](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "発行プロファイルをインポートします。")</span><span class="sxs-lookup"><span data-stu-id="dc66b-606">![Importing the publish profile](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "Importing the publish profile")</span></span>

    <span data-ttu-id="dc66b-607">*発行プロファイルのインポート*</span><span class="sxs-lookup"><span data-stu-id="dc66b-607">*Importing publish profile*</span></span>
3. <span data-ttu-id="dc66b-608">をクリックして**への接続検証**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-608">Click **Validate Connection**.</span></span> <span data-ttu-id="dc66b-609">検証が完了したらクリックして**次**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-609">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dc66b-610">検証が完了すると、接続の検証 ボタンの横に表示される緑色のチェック マークを参照してください。</span><span class="sxs-lookup"><span data-stu-id="dc66b-610">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="dc66b-611">![接続の検証](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "接続の検証")</span><span class="sxs-lookup"><span data-stu-id="dc66b-611">![Validating connection](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "Validating connection")</span></span>

    <span data-ttu-id="dc66b-612">*接続の検証*</span><span class="sxs-lookup"><span data-stu-id="dc66b-612">*Validating connection*</span></span>
4. <span data-ttu-id="dc66b-613">**設定**] ページの [、**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックして (つまり**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="dc66b-613">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="dc66b-614">![Web 配置の構成](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Web 配置の構成")</span><span class="sxs-lookup"><span data-stu-id="dc66b-614">![Web deploy configuration](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Web deploy configuration")</span></span>

    <span data-ttu-id="dc66b-615">*Web 配置の構成*</span><span class="sxs-lookup"><span data-stu-id="dc66b-615">*Web deploy configuration*</span></span>
5. <span data-ttu-id="dc66b-616">次のように、データベースの接続を構成します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-616">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="dc66b-617">**サーバー名**SQL データベース サーバーの URL を使用して、入力、 *tcp:* プレフィックス。</span><span class="sxs-lookup"><span data-stu-id="dc66b-617">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="dc66b-618">**ユーザー名**サーバー管理者のログイン名を入力します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-618">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="dc66b-619">**パスワード**サーバー管理者のログイン パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-619">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="dc66b-620">新しいデータベース名を入力します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-620">Type a new database name.</span></span>

     <span data-ttu-id="dc66b-621">![対象の接続文字列を構成する](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "対象の接続文字列を構成します。")</span><span class="sxs-lookup"><span data-stu-id="dc66b-621">![Configuring destination connection string](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="dc66b-622">*対象の接続文字列を構成します。*</span><span class="sxs-lookup"><span data-stu-id="dc66b-622">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="dc66b-623">次に、 **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-623">Then click **OK**.</span></span> <span data-ttu-id="dc66b-624">データベースをクリックを作成するように求められたら**はい**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-624">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="dc66b-625">![データベースを作成する](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "データベース文字列を作成します。")</span><span class="sxs-lookup"><span data-stu-id="dc66b-625">![Creating the database](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "Creating the database string")</span></span>

    <span data-ttu-id="dc66b-626">*データベースの作成*</span><span class="sxs-lookup"><span data-stu-id="dc66b-626">*Creating the database*</span></span>
7. <span data-ttu-id="dc66b-627">接続の既定のテキスト ボックス内では、Azure の SQL データベースへの接続に使用する接続文字列が表示されます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-627">The connection string you will use to connect to SQL Database in Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="dc66b-628">その後、 **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-628">Then click **Next**.</span></span>

    <span data-ttu-id="dc66b-629">![SQL データベースを指す接続文字列](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "SQL データベースを指す接続文字列")</span><span class="sxs-lookup"><span data-stu-id="dc66b-629">![Connection string pointing to SQL Database](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="dc66b-630">*SQL データベースを指す接続文字列*</span><span class="sxs-lookup"><span data-stu-id="dc66b-630">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="dc66b-631">**プレビュー** ] ページで [**発行**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-631">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="dc66b-632">![Web アプリケーションの発行](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "web アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="dc66b-632">![Publishing the web application](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "Publishing the web application")</span></span>

    <span data-ttu-id="dc66b-633">*Web アプリケーションの発行*</span><span class="sxs-lookup"><span data-stu-id="dc66b-633">*Publishing the web application*</span></span>
9. <span data-ttu-id="dc66b-634">発行プロセスが終了すると、既定のブラウザーが公開された web サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-634">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="dc66b-635">付録 c: コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="dc66b-635">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="dc66b-636">コード スニペットでは、すぐに利用できる必要があるすべてのコードがあります。</span><span class="sxs-lookup"><span data-stu-id="dc66b-636">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="dc66b-637">ラボ ドキュメントがわかります正確に使用する場合が、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-637">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="dc66b-638">![Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入する](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "プロジェクトにコードを挿入する Visual Studio を使ってコード スニペット")</span><span class="sxs-lookup"><span data-stu-id="dc66b-638">![Using Visual Studio code snippets to insert code into your project](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="dc66b-639">*Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入するには*</span><span class="sxs-lookup"><span data-stu-id="dc66b-639">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="dc66b-640">***キーボード (C# の場合のみ) を使用してコード スニペットを追加するには***</span><span class="sxs-lookup"><span data-stu-id="dc66b-640">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="dc66b-641">コードを挿入するには、カーソルを置きます。</span><span class="sxs-lookup"><span data-stu-id="dc66b-641">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="dc66b-642">(なし、スペースやハイフン) スニペット名を入力してを起動します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-642">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="dc66b-643">スニペットの名前に一致する IntelliSense 表示を確認します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-643">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="dc66b-644">正しいスニペットを選択 (または、全体のスニペットの名前を選択するまで」と入力してください)。</span><span class="sxs-lookup"><span data-stu-id="dc66b-644">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="dc66b-645">カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-645">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="dc66b-646">![スニペット名を入力する開始](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "スニペット名の入力を開始")</span><span class="sxs-lookup"><span data-stu-id="dc66b-646">![Start typing the snippet name](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "Start typing the snippet name")</span></span>

<span data-ttu-id="dc66b-647">*スニペット名の入力を開始します。*</span><span class="sxs-lookup"><span data-stu-id="dc66b-647">*Start typing the snippet name*</span></span>

<span data-ttu-id="dc66b-648">![Tab キーを押して、強調表示されているスニペットを選択する](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "強調表示されているスニペットを選択するキーを押してタブ")</span><span class="sxs-lookup"><span data-stu-id="dc66b-648">![Press Tab to select the highlighted snippet](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="dc66b-649">*Tab キーを押して、強調表示されているスニペットを選択するには*</span><span class="sxs-lookup"><span data-stu-id="dc66b-649">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="dc66b-650">![キーを押して タブで再度と、スニペットが展開](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "キーを押して タブで再度と、スニペットが展開されます")</span><span class="sxs-lookup"><span data-stu-id="dc66b-650">![Press Tab again and the snippet will expand](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="dc66b-651">*キーを押して タブで再度と、スニペットが展開されます。*</span><span class="sxs-lookup"><span data-stu-id="dc66b-651">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="dc66b-652">***(C#、Visual Basic および XML) にマウスを使用してコード スニペットを追加する***1 です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-652">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="dc66b-653">コード スニペットを挿入する場所を右クリックします。</span><span class="sxs-lookup"><span data-stu-id="dc66b-653">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="dc66b-654">選択**スニペットの挿入**続く**マイ コード スニペット**です。</span><span class="sxs-lookup"><span data-stu-id="dc66b-654">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="dc66b-655">クリックして一覧から、関連するスニペットを選択します。</span><span class="sxs-lookup"><span data-stu-id="dc66b-655">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="dc66b-656">![コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックして](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリック")</span><span class="sxs-lookup"><span data-stu-id="dc66b-656">![Right-click where you want to insert the code snippet and select Insert Snippet](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="dc66b-657">*コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックします。*</span><span class="sxs-lookup"><span data-stu-id="dc66b-657">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="dc66b-658">![クリックして一覧から、関連するスニペットを選択](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "クリックして一覧から、関連するスニペットを選択")</span><span class="sxs-lookup"><span data-stu-id="dc66b-658">![Pick the relevant snippet from the list, by clicking on it](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="dc66b-659">*クリックして一覧から、関連するスニペットを選択します。*</span><span class="sxs-lookup"><span data-stu-id="dc66b-659">*Pick the relevant snippet from the list, by clicking on it*</span></span>
