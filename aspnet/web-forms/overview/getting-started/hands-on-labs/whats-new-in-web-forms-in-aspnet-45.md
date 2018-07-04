---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: ASP.NET 4.5 の新機能については Web フォームの |Microsoft Docs
author: rick-anderson
description: ASP.NET Web フォームの新しいバージョンでは、さまざまなデータを扱うときにユーザー エクスペリエンスの向上に重点を置いた機能強化について説明します。 以前のバージョンにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 4e8c8f303851b7f1a01744cab58e27a9b37127a6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389232"
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a><span data-ttu-id="f92f6-104">Asp.net 4.5 Web フォームの新機能新機能</span><span class="sxs-lookup"><span data-stu-id="f92f6-104">What's New in Web Forms in ASP.NET 4.5</span></span>
====================
<span data-ttu-id="f92f6-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="f92f6-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="f92f6-106">ASP.NET Web フォームの新しいバージョンでは、さまざまなデータを扱うときにユーザー エクスペリエンスの向上に重点を置いた機能強化について説明します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-106">The new version of ASP.NET Web Forms introduces a number of improvements focused on improving user experience when working with data.</span></span>
> 
> <span data-ttu-id="f92f6-107">オブジェクトのメンバーの値を出力するデータ バインドを使用する場合は、Web フォームの以前のバージョンでは、Bind() または"eval()"のデータ バインディング式を使用します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-107">In previous versions of Web Forms, when using data-binding to emit the value of an object member, you used the data-binding expressions Bind() or Eval().</span></span> <span data-ttu-id="f92f6-108">新しいバージョンの ASP.NET、データの種類のコントロールに新しい ItemType プロパティを使用してバインドすることを宣言することができます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-108">In the new version of ASP.NET, you are able to declare what type of data a control is going to be bound to by using a new ItemType property.</span></span> <span data-ttu-id="f92f6-109">このプロパティを設定すると、その Visual Studio 開発環境の IntelliSense、ナビゲーションのメンバー、およびコンパイル時チェックなどの特典をフルに厳密に型指定された変数を使用することが有効になります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-109">Setting this property will enable you to use a strongly-typed variable to receive the full benefits of the Visual Studio development experience, such as IntelliSense, member navigation, and compile-time checking.</span></span>
> 
> <span data-ttu-id="f92f6-110">データ バインド コントロールを使用ようになりましたも指定できます、独自のカスタム メソッドを選択すると、更新、削除、および、データの挿入のページ コントロールと、アプリケーションのロジック間の対話を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-110">With the data-bound controls, you can now also specify your own custom methods for selecting, updating, deleting and inserting data, simplifying the interaction between the page controls and your application logic.</span></span> <span data-ttu-id="f92f6-111">さらに、ASP.NET で、メソッド型パラメーターに直接、ページのデータをマップすることを意味するモデル バインド機能が追加されました。</span><span class="sxs-lookup"><span data-stu-id="f92f6-111">Additionally, model binding capabilities have been added to ASP.NET, which means you can map data from the page directly into method type parameters.</span></span>
> 
> <span data-ttu-id="f92f6-112">ユーザー入力の検証は、最新の Web フォームで簡単にあります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-112">Validating user input should also be easier with the latest version of Web Forms.</span></span> <span data-ttu-id="f92f6-113">モデル クラスからの検証属性で注釈を表示できます、 **System.ComponentModel.DataAnnotations**名前空間と、すべてのサイトを制御する要求は、その情報を使用してユーザー入力を検証します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-113">You can now annotate your model classes with validation attributes from the **System.ComponentModel.DataAnnotations** namespace and request that all your site controls validate user input using that information.</span></span> <span data-ttu-id="f92f6-114">Web フォームでのクライアント側検証は、jQuery では、クライアント側コード クリーナーと控えめな JavaScript 機能を提供する統合ようになりました。</span><span class="sxs-lookup"><span data-stu-id="f92f6-114">Client-side validation in Web Forms is now integrated with jQuery, providing cleaner client-side code and unobtrusive JavaScript features.</span></span>
> 
> <span data-ttu-id="f92f6-115">要求検証領域で、選択的に要求の検証、アプリケーションの特定の部分をオフにするか、無効化された要求データを読み取りやすく改良が加えられてがいます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-115">In the request validation area, improvements have been made to make it easier to selectively turn off request validation for specific parts of your applications or read invalidated request data.</span></span>
> 
> <span data-ttu-id="f92f6-116">一部の機能強化が HTML5 の新機能を活用するためにサーバー コントロールを Web フォームに加えられました。</span><span class="sxs-lookup"><span data-stu-id="f92f6-116">Some improvements have been made to Web Forms server controls to take advantage of new features of HTML5:</span></span>
> 
> - <span data-ttu-id="f92f6-117">TextBox コントロールの TextMode プロパティは、電子メール、datetime、およびなどのような新しい HTML5 入力タイプをサポートするために更新されました。</span><span class="sxs-lookup"><span data-stu-id="f92f6-117">The TextMode property of the TextBox control has been updated to support the new HTML5 input types like email, datetime, and so on.</span></span>
> - <span data-ttu-id="f92f6-118">FileUpload コントロールでは、この HTML5 機能をサポートするブラウザーからの複数のファイル アップロードできるようになりました。</span><span class="sxs-lookup"><span data-stu-id="f92f6-118">The FileUpload control now supports multiple file uploads from browsers that support this HTML5 feature.</span></span>
> - <span data-ttu-id="f92f6-119">検証コントロールは、今すぐサポート検証 HTML5 入力要素を制御します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-119">Validator controls now support validating HTML5 input elements.</span></span>
> - <span data-ttu-id="f92f6-120">これで、URL を表す属性を持つ新しい HTML5 の要素をサポートして runat =&quot;server&quot;します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-120">New HTML5 elements that have attributes that represent a URL now support runat=&quot;server&quot;.</span></span> <span data-ttu-id="f92f6-121">その結果、規則を使用できます ASP.NET URL のパスのような ~ を表すアプリケーション ルート演算子 (たとえば、&lt;ビデオ runat =&quot;server&quot; src =&quot;~/myVideo.wmv&quot; &gt; &lt;/ビデオ&gt;)。</span><span class="sxs-lookup"><span data-stu-id="f92f6-121">As a result, you can use ASP.NET conventions in URL paths, like the ~ operator to represent the application root (for example, &lt;video runat=&quot;server&quot; src=&quot;~/myVideo.wmv&quot;&gt;&lt;/video&gt;).</span></span>
> - <span data-ttu-id="f92f6-122">UpdatePanel コントロールは、入力フィールドの転記 HTML5 をサポートするために修正されています。</span><span class="sxs-lookup"><span data-stu-id="f92f6-122">The UpdatePanel control has been fixed to support posting HTML5 input fields.</span></span>
> 
> <span data-ttu-id="f92f6-123">公式の ASP.NET ポータルでは、ASP.NET WebForms 4.5 での新機能の例についてを確認できます: [ASP.NET 4.5 と Visual Studio 2012 では新機能](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)</span><span class="sxs-lookup"><span data-stu-id="f92f6-123">In the official ASP.NET portal you can find more examples of the new features in ASP.NET WebForms 4.5: [What's New in ASP.NET 4.5 and Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)</span></span>
> 
> <span data-ttu-id="f92f6-124">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-124">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="f92f6-125">目的</span><span class="sxs-lookup"><span data-stu-id="f92f6-125">Objectives</span></span>

<span data-ttu-id="f92f6-126">このハンズオン ラボでは、学習する方法。</span><span class="sxs-lookup"><span data-stu-id="f92f6-126">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="f92f6-127">厳密に型指定されたデータ バインディング式を使用します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-127">Use strongly-typed data-binding expressions</span></span>
- <span data-ttu-id="f92f6-128">新しいモデル バインド機能を使用して、Web フォーム</span><span class="sxs-lookup"><span data-stu-id="f92f6-128">Use new model binding features in Web Forms</span></span>
- <span data-ttu-id="f92f6-129">コード分離メソッドにページ データをマッピングするための値プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-129">Use value providers for mapping page data to code-behind methods</span></span>
- <span data-ttu-id="f92f6-130">データ注釈を使用して、ユーザー入力の検証</span><span class="sxs-lookup"><span data-stu-id="f92f6-130">Use Data Annotations for user input validation</span></span>
- <span data-ttu-id="f92f6-131">Web フォームでの jquery クライアント側検証を unobstrusive advange がかかる</span><span class="sxs-lookup"><span data-stu-id="f92f6-131">Take advange of unobstrusive client-side validation with jQuery in Web Forms</span></span>
- <span data-ttu-id="f92f6-132">詳細な要求の検証を実装します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-132">Implement granular request validation</span></span>
- <span data-ttu-id="f92f6-133">非同期ページの Web フォームでの処理の実装します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-133">Implement asynchronous page processing in Web Forms</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="f92f6-134">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="f92f6-134">Prerequisites</span></span>

<span data-ttu-id="f92f6-135">このラボを完成させるのには、次の項目が必要です。</span><span class="sxs-lookup"><span data-stu-id="f92f6-135">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="f92f6-136">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="f92f6-136">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="f92f6-137">セットアップ</span><span class="sxs-lookup"><span data-stu-id="f92f6-137">Setup</span></span>

<span data-ttu-id="f92f6-138">**コード スニペットをインストールします。**</span><span class="sxs-lookup"><span data-stu-id="f92f6-138">**Installing Code Snippets**</span></span>

<span data-ttu-id="f92f6-139">便宜上、このラボに管理するコードの多くは、Visual Studio コード スニペットとして利用できます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-139">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="f92f6-140">実行コード スニペットをインストールする **.\Source\Setup\CodeSnippets.vsi**ファイル。</span><span class="sxs-lookup"><span data-stu-id="f92f6-140">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="f92f6-141">このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 c: を使用してコード スニペット](#AppendixC)&quot;します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-141">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="f92f6-142">演習</span><span class="sxs-lookup"><span data-stu-id="f92f6-142">Exercises</span></span>

<span data-ttu-id="f92f6-143">このハンズオン ラボには、次の演習が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-143">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="f92f6-144">手順 1: モデルの ASP.NET Web フォームでのバインド</span><span class="sxs-lookup"><span data-stu-id="f92f6-144">Exercise 1: Model Binding in ASP.NET Web Forms</span></span>](#Exercise1)
2. [<span data-ttu-id="f92f6-145">手順 2: データの検証</span><span class="sxs-lookup"><span data-stu-id="f92f6-145">Exercise 2: Data Validation</span></span>](#Exercise2)
3. [<span data-ttu-id="f92f6-146">手順 3: フォームの非同期ページの ASP.NET web 処理</span><span class="sxs-lookup"><span data-stu-id="f92f6-146">Exercise 3: Asynchronous Page Processing in ASP.NET Web Forms</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="f92f6-147">各演習が用意されており、**エンド**演習を完了した後に取得する必要があります、結果として得られるソリューションに含まれているフォルダー。</span><span class="sxs-lookup"><span data-stu-id="f92f6-147">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="f92f6-148">作業、演習を通じて追加のヘルプが必要な場合は、このソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-148">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="f92f6-149">この演習の所要時間を推定: **60 分**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-149">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a><span data-ttu-id="f92f6-150">手順 1: モデルの ASP.NET Web フォームでのバインド</span><span class="sxs-lookup"><span data-stu-id="f92f6-150">Exercise 1: Model Binding in ASP.NET Web Forms</span></span>

<span data-ttu-id="f92f6-151">ASP.NET Web フォームの新しいバージョンには、多数のデータを使用する場合、エクスペリエンスの向上に重点を置いた機能が導入されています。</span><span class="sxs-lookup"><span data-stu-id="f92f6-151">The new version of ASP.NET Web Forms introduces a number of enhancements focused on improving the experience when working with data.</span></span> <span data-ttu-id="f92f6-152">この演習では、厳密に型指定されたデータ コントロールについて説明し、モデル バインドします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-152">Throughout this exercise, you will learn about strongly typed data-controls and model binding.</span></span>

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a><span data-ttu-id="f92f6-153">タスク 1 - 厳密に型指定のデータ バインディングの使用</span><span class="sxs-lookup"><span data-stu-id="f92f6-153">Task 1 - Using Strongly-Typed Data-Bindings</span></span>

<span data-ttu-id="f92f6-154">このタスクでは、新しい厳密に型指定されたバインド ASP.NET 4.5 で使用可能なを検出します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-154">In this task, you will discover the new strongly-typed bindings available in ASP.NET 4.5.</span></span>

1. <span data-ttu-id="f92f6-155">開く、**開始**ソリューションがある**ソース/Ex1-ModelBinding/開始/** フォルダー。</span><span class="sxs-lookup"><span data-stu-id="f92f6-155">Open the **Begin** solution located at **Source/Ex1-ModelBinding/Begin/** folder.</span></span>

   1. <span data-ttu-id="f92f6-156">いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行する前にします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-156">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="f92f6-157">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-157">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="f92f6-158">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="f92f6-158">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="f92f6-159">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-159">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f92f6-160">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="f92f6-160">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="f92f6-161">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-161">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="f92f6-162">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-162">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="f92f6-163">開く、 **Customers.aspx**ページ。</span><span class="sxs-lookup"><span data-stu-id="f92f6-163">Open the **Customers.aspx** page.</span></span> <span data-ttu-id="f92f6-164">メインのコントロールに番号なしのリストを配置し、各顧客の一覧を表示するための内部 repeater コントロールが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-164">Place an unnumbered list in the main control and include a repeater control inside for listing each customer.</span></span> <span data-ttu-id="f92f6-165">リピータの名前に設定**customersRepeater**次のコードに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-165">Set the repeater name to **customersRepeater** as shown in the following code.</span></span>

    <span data-ttu-id="f92f6-166">以前のバージョンの Web フォーム、データ バインディングをしているオブジェクトのメンバーの値を出力するデータ バインドを使用する場合は式を使用するデータ バインド、Eval メソッドの呼び出しと共に渡して、メンバーの名前を文字列として。</span><span class="sxs-lookup"><span data-stu-id="f92f6-166">In previous versions of Web Forms, when using data-binding to emit the value of a member on an object you're data-binding to, you would use a data-binding expression, along with a call to the Eval method, passing in the name of the member as a string.</span></span>

    <span data-ttu-id="f92f6-167">、実行時に、Eval をこれらの呼び出しは現在バインドされているオブジェクトに対してリフレクションを使用して、指定した名前を持つメンバーの値を読み取るし、html 形式で結果を表示します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-167">At runtime, these calls to Eval will use reflection against the currently bound object to read the value of the member with the given name, and display the result in the HTML.</span></span> <span data-ttu-id="f92f6-168">このアプローチでは、任意、unshaped データに対してデータをバインドする非常に簡単です。</span><span class="sxs-lookup"><span data-stu-id="f92f6-168">This approach makes it very easy to data-bind against arbitrary, unshaped data.</span></span>

    <span data-ttu-id="f92f6-169">残念ながら、メンバー名、(定義へ移動) などのナビゲーションやコンパイル時チェックのサポート用の IntelliSense を含む、Visual Studio での開発時の優れたエクスペリエンス機能の多くが失われます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-169">Unfortunately, you lose many of the great development-time experience features in Visual Studio, including IntelliSense for member names, support for navigation (like Go To Definition), and compile-time checking.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. <span data-ttu-id="f92f6-170">開く、 **Customers.aspx.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="f92f6-170">Open the **Customers.aspx.cs** file.</span></span>
4. <span data-ttu-id="f92f6-171">次の追加ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-171">Add the following using statement.</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. <span data-ttu-id="f92f6-172">**ページ\_ロード**メソッドでは、顧客のリストと repeater を設定するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-172">In the **Page\_Load** method, add code to populate the repeater with the list of customers.</span></span>

    <span data-ttu-id="f92f6-173">(コード スニペット -*フォーム ラボ - Ex01 - バインドのお客様のデータ ソースを Web*)</span><span class="sxs-lookup"><span data-stu-id="f92f6-173">(Code Snippet - *Web Forms Lab - Ex01 - Bind Customers Data Source*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    <span data-ttu-id="f92f6-174">ソリューションでは、CodeFirst と共に EntityFramework を使用して作成し、データベースへのアクセスします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-174">The solution uses EntityFramework together with CodeFirst to create and access the database.</span></span> <span data-ttu-id="f92f6-175">次のコードで、customersRepeater は、データベースからすべての顧客を返す具体化されたクエリにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-175">In the following code, the customersRepeater is bound to a materialized query that returns all the customers from the database.</span></span>
6. <span data-ttu-id="f92f6-176">キーを押して**F5**にソリューションを実行しに移動、**顧客**アクションで repeater を表示するページ。</span><span class="sxs-lookup"><span data-stu-id="f92f6-176">Press **F5** to run the solution and go to the **Customers** page to see the repeater in action.</span></span> <span data-ttu-id="f92f6-177">CodeFirst、ソリューションを使用するいると、データベースを作成し、アプリケーションを実行するときに、ローカル SQL Express インスタンスに設定されます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-177">As the solution is using CodeFirst, the database will be created and populated in your local SQL Express instance when running the application.</span></span>

    <span data-ttu-id="f92f6-178">![Repeater で顧客を一覧表示する](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "repeater で顧客を一覧表示します。")</span><span class="sxs-lookup"><span data-stu-id="f92f6-178">![Listing the customers with a repeater](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "Listing the customers with a repeater")</span></span>

    <span data-ttu-id="f92f6-179">*Repeater で顧客を一覧表示します。*</span><span class="sxs-lookup"><span data-stu-id="f92f6-179">*Listing the customers with a repeater*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f92f6-180">Visual Studio 2012、IIS Express は、既定の Web 開発サーバーです。</span><span class="sxs-lookup"><span data-stu-id="f92f6-180">In Visual Studio 2012, IIS Express is the default Web development server.</span></span>
7. <span data-ttu-id="f92f6-181">ブラウザーを閉じて、Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-181">Close the browser and go back to Visual Studio.</span></span>
8. <span data-ttu-id="f92f6-182">厳密に型指定されたバインドを使用する実装を置き換えるようになりました。</span><span class="sxs-lookup"><span data-stu-id="f92f6-182">Now replace the implementation to use strongly typed bindings.</span></span> <span data-ttu-id="f92f6-183">開く、 **Customers.aspx**ページし、新しい**ItemType**属性を設定する中継ぎ局で、**顧客**バインディングの種類の型。</span><span class="sxs-lookup"><span data-stu-id="f92f6-183">Open the **Customers.aspx** page and use the new **ItemType** attribute in the repeater to set the **Customer** type as the binding type.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    <span data-ttu-id="f92f6-184">ItemType プロパティでは、データの種類、コントロールにバインドすると厳密に型指定、データ バインド コントロール内でバインドを使用することができますを宣言することができます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-184">The ItemType property enables you to declare which type of data the control is going to be bound to and allows you to use strongly-typed binding inside the data-bound control.</span></span>
9. <span data-ttu-id="f92f6-185">ItemTemplate のコンテンツを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-185">Replace the ItemTemplate content with the following code.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    <span data-ttu-id="f92f6-186">上記のアプローチの 1 つの欠点は、"eval()"を Bind() 呼び出しでは遅延バインディングのプロパティ名を表す文字列を渡すことを意味します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-186">One downside with the above approaches is that the calls to Eval() and Bind() are late-bound - meaning you pass strings to represent the property names.</span></span> <span data-ttu-id="f92f6-187">これは、Intellisense は、メンバー名、コード ナビゲーション (定義へ移動) などのサポートもコンパイル時チェックのサポートを得られないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-187">This means you don't get Intellisense for the member names, support for code navigation (like Go To Definition), nor compile-time checking support.</span></span>

    <span data-ttu-id="f92f6-188">ItemType プロパティを設定すると、データ バインディング式のスコープに、生成する 2 つの新しい型指定された変数:**項目**と**BindItem**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-188">Setting the ItemType property causes two new typed variables to be generated in the scope of the data-binding expressions: **Item** and **BindItem**.</span></span> <span data-ttu-id="f92f6-189">データ バインディング式でこれらの厳密に型指定された変数を使用し、Visual Studio の開発エクスペリエンスの完全なメリットを享受できます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-189">You can use these strongly typed variables in the data-binding expressions and get the full benefits of the Visual Studio development experience.</span></span>

    <span data-ttu-id="f92f6-190">&quot; **:** &quot;式で使用が自動的に HTML エンコード (クロスサイト スクリプティング攻撃など) のセキュリティの問題を回避するために出力します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-190">The &quot;**:** &quot; used in the expression will automatically HTML-encode the output to avoid security issues (for example, cross-site scripting attacks).</span></span> <span data-ttu-id="f92f6-191">この表記法では、使用可能な .NET 4 以降の書き込み、応答しますが、ようになりましたはデータ バインド式で使用できるも。</span><span class="sxs-lookup"><span data-stu-id="f92f6-191">This notation was available since .NET 4 for response writing, but now is also available in data-binding expressions.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f92f6-192">項目のメンバーでは、一方向のバインドは機能します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-192">The Item member works for one-way binding.</span></span> <span data-ttu-id="f92f6-193">双方向のバインドを実行する場合、 **BindItem**メンバー。</span><span class="sxs-lookup"><span data-stu-id="f92f6-193">If you want to perform two-way binding use the **BindItem** member.</span></span>

    <span data-ttu-id="f92f6-194">![厳密に型指定されたバインディングでの IntelliSense サポート](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "厳密に型指定されたバインディングでの IntelliSense のサポート")</span><span class="sxs-lookup"><span data-stu-id="f92f6-194">![IntelliSense support in strongly-typed binding](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "IntelliSense support in strongly-typed binding")</span></span>

    <span data-ttu-id="f92f6-195">*厳密に型指定されたバインディングでの IntelliSense のサポート*</span><span class="sxs-lookup"><span data-stu-id="f92f6-195">*IntelliSense support in strongly-typed binding*</span></span>
10. <span data-ttu-id="f92f6-196">キーを押して**F5**にソリューションを実行し、期待どおりに変更を確認する [顧客] ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-196">Press **F5** to run the solution and go to the Customers page to make sure the changes work as expected.</span></span>

    <span data-ttu-id="f92f6-197">![顧客の詳細を一覧表示する](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "顧客の詳細を一覧表示します。")</span><span class="sxs-lookup"><span data-stu-id="f92f6-197">![Listing customer details](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "Listing customer details")</span></span>

    <span data-ttu-id="f92f6-198">*顧客の詳細を一覧表示します。*</span><span class="sxs-lookup"><span data-stu-id="f92f6-198">*Listing customer details*</span></span>
11. <span data-ttu-id="f92f6-199">ブラウザーを閉じて、Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-199">Close the browser and go back to Visual Studio.</span></span>

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a><span data-ttu-id="f92f6-200">タスク 2 - 概要のモデルを Web フォームでのバインド</span><span class="sxs-lookup"><span data-stu-id="f92f6-200">Task 2 - Introducing Model Binding in Web Forms</span></span>

<span data-ttu-id="f92f6-201">ASP.NET Web フォームの以前のバージョンで取得して、データの更新の両方に双方向データ バインディングを実行する必要な場合にするために必要なデータ ソース オブジェクトを使用しています。</span><span class="sxs-lookup"><span data-stu-id="f92f6-201">In previous versions of ASP.NET Web Forms, when you wanted to perform two-way data-binding, both retrieving and updating data, you needed to use a Data Source object.</span></span> <span data-ttu-id="f92f6-202">これは、オブジェクト データ ソースを SQL データ ソース、LINQ のデータ ソースなど。</span><span class="sxs-lookup"><span data-stu-id="f92f6-202">This could be an Object Data Source, a SQL Data Source, a LINQ Data Source and so on.</span></span> <span data-ttu-id="f92f6-203">ただし、シナリオでは、カスタム コードを必要に応じて、データを処理するため、オブジェクトのデータ ソースを使用する必要があるし、これにより、いくつかの欠点です。</span><span class="sxs-lookup"><span data-stu-id="f92f6-203">However if your scenario required custom code for handling the data, you needed to use the Object Data Source and this brought some drawbacks.</span></span> <span data-ttu-id="f92f6-204">たとえば、複合型を回避するために必要なし、検証ロジックを実行するときに例外を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-204">For example, you needed to avoid complex types and you needed to handle exceptions when executing validation logic.</span></span>

<span data-ttu-id="f92f6-205">ASP.NET Web フォームの新しいバージョンでは、データ バインド コントロールは、モデル バインディングをサポートします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-205">In the new version of ASP.NET Web Forms the data-bound controls support model binding.</span></span> <span data-ttu-id="f92f6-206">つまり、できるし、指定することを選択、更新、挿入、分離コード ファイルまたは別のクラスからは、ロジックを呼び出すにはデータ バインド コントロールで直接メソッドを削除します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-206">This means that you can specify select, update, insert and delete methods directly in the data-bound control to call logic from your code-behind file or from another class.</span></span>

<span data-ttu-id="f92f6-207">これについては、新しい製品カテゴリを一覧表示する GridView を使用するが**SelectMethod**属性。</span><span class="sxs-lookup"><span data-stu-id="f92f6-207">To learn about this, you will use a GridView to list the product categories using the new **SelectMethod** attribute.</span></span> <span data-ttu-id="f92f6-208">この属性では、GridView のデータを取得するための方法を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-208">This attribute enables you to specify a method for retrieving the GridView data.</span></span>

1. <span data-ttu-id="f92f6-209">開く、**ディスク上のファイル**ページし、が含まれて、 **GridView**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-209">Open the **Products.aspx** page and include a **GridView**.</span></span> <span data-ttu-id="f92f6-210">厳密に型指定されたバインディングを使用し、並べ替えとページングを有効にする次のように、GridView を構成します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-210">Configure the GridView as shown below to use strongly-typed bindings and enable sorting and paging.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. <span data-ttu-id="f92f6-211">新たに使用**SelectMethod**を呼び出す GridView を構成する属性、 **GetCategories**メソッド データを選択します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-211">Use the new **SelectMethod** attribute to configure the GridView to call a **GetCategories** method to select the data.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. <span data-ttu-id="f92f6-212">オープン、 **Products.aspx.cs**分離コード ファイルを開き、次を追加するステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-212">Open the **Products.aspx.cs** code-behind file and add the following using statements.</span></span>

    <span data-ttu-id="f92f6-213">(コード スニペット - *Web フォームのラボ - Ex01 - 名前空間*)</span><span class="sxs-lookup"><span data-stu-id="f92f6-213">(Code Snippet - *Web Forms Lab - Ex01 - Namespaces*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. <span data-ttu-id="f92f6-214">プライベート メンバーを追加、**製品**クラスし、の新しいインスタンスを割り当てる**ProductsContext**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-214">Add a private member in the **Products** class and assign a new instance of **ProductsContext**.</span></span> <span data-ttu-id="f92f6-215">このプロパティでは、データベースに接続することができます、Entity Framework データ コンテキストを格納します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-215">This property will store the Entity Framework data context that enables you to connect to the database.</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. <span data-ttu-id="f92f6-216">作成、 **GetCategories** LINQ を使用してカテゴリの一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-216">Create a **GetCategories** method to retrieve the list of categories using LINQ.</span></span> <span data-ttu-id="f92f6-217">クエリが含まれます、**製品**プロパティ GridView は各カテゴリの製品の量を表示できるようにします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-217">The query will include the **Products** property so the GridView can show the amount of products for each category.</span></span> <span data-ttu-id="f92f6-218">メソッドが後でページのライフ サイクルを実行するクエリを表す生の IQueryable オブジェクトを返すことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f92f6-218">Notice that the method returns a raw IQueryable object that represent the query to be executed later on the page lifecycle.</span></span>

    <span data-ttu-id="f92f6-219">(コード スニペット - *Web フォームのラボ - Ex01 - GetCategories*)</span><span class="sxs-lookup"><span data-stu-id="f92f6-219">(Code Snippet - *Web Forms Lab - Ex01 - GetCategories*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > <span data-ttu-id="f92f6-220">ASP.NET Web フォームの以前のバージョンで有効にする並べ替えとページング、データ ソースのオブジェクト コンテキスト内で独自のリポジトリ ロジックを使用してに必要なカスタム コードを記述し、必要なすべてのパラメーターを受け取る。</span><span class="sxs-lookup"><span data-stu-id="f92f6-220">In previous versions of ASP.NET Web Forms, enabling sorting and paging using your own repository logic within an Object Data Source context, required to write your own custom code and receive all the necessary parameters.</span></span> <span data-ttu-id="f92f6-221">ここで、データ バインディングのメソッドが IQueryable を返すことができ、これは、クエリを表します。 適切な並べ替えを追加するクエリとページングのパラメーターを変更することもを実行する ASP.NET を処理できます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-221">Now, as the data-binding methods can return IQueryable and this represents a query still to be executed, ASP.NET can take care of modifying the query to add the proper sorting and paging parameters.</span></span>
6. <span data-ttu-id="f92f6-222">キーを押して**F5**サイトのデバッグを開始し、[製品] ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-222">Press **F5** to start debugging the site and go to the Products page.</span></span> <span data-ttu-id="f92f6-223">GridView には、メソッドによって返されるカテゴリが表示されていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-223">You should see that the GridView is populated with the categories returned by the GetCategories method.</span></span>

    <span data-ttu-id="f92f6-224">![モデル バインドを使用して GridView を設定](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "モデル バインドを使用して GridView を設定")</span><span class="sxs-lookup"><span data-stu-id="f92f6-224">![Populating a GridView using model binding](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "Populating a GridView using model binding")</span></span>

    <span data-ttu-id="f92f6-225">*モデル バインドを使用して GridView を設定*</span><span class="sxs-lookup"><span data-stu-id="f92f6-225">*Populating a GridView using model binding*</span></span>
7. <span data-ttu-id="f92f6-226">キーを押して**SHIFT**+**F5**デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-226">Press **SHIFT**+**F5** Stop debugging.</span></span>

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a><span data-ttu-id="f92f6-227">タスク 3 - モデル バインドの値プロバイダー</span><span class="sxs-lookup"><span data-stu-id="f92f6-227">Task 3 - Value Providers in Model Binding</span></span>

<span data-ttu-id="f92f6-228">モデル バインドだけでなく、データ バインド コントロールで直接データを操作するカスタム メソッドを指定することができますが、ページからデータをこれらのメソッドからのパラメーターにマップすることもできます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-228">Model binding not only enables you to specify custom methods to work with your data directly in the data-bound control, but also allows you to map data from the page into parameters from these methods.</span></span> <span data-ttu-id="f92f6-229">メソッドのパラメーターの値のデータ ソースを指定するのに値プロバイダー属性を使用できます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-229">On the method parameter, you can use value provider attributes to specify the value's data source.</span></span> <span data-ttu-id="f92f6-230">例えば:</span><span class="sxs-lookup"><span data-stu-id="f92f6-230">For example:</span></span>

- <span data-ttu-id="f92f6-231">ページ上のコントロール</span><span class="sxs-lookup"><span data-stu-id="f92f6-231">Controls on the page</span></span>
- <span data-ttu-id="f92f6-232">クエリ文字列の値</span><span class="sxs-lookup"><span data-stu-id="f92f6-232">Query string values</span></span>
- <span data-ttu-id="f92f6-233">データの表示</span><span class="sxs-lookup"><span data-stu-id="f92f6-233">View data</span></span>
- <span data-ttu-id="f92f6-234">セッション状態</span><span class="sxs-lookup"><span data-stu-id="f92f6-234">Session state</span></span>
- <span data-ttu-id="f92f6-235">クッキー</span><span class="sxs-lookup"><span data-stu-id="f92f6-235">Cookies</span></span>
- <span data-ttu-id="f92f6-236">ポストされたフォーム データ</span><span class="sxs-lookup"><span data-stu-id="f92f6-236">Posted form data</span></span>
- <span data-ttu-id="f92f6-237">ビュー ステート</span><span class="sxs-lookup"><span data-stu-id="f92f6-237">View state</span></span>
- <span data-ttu-id="f92f6-238">カスタム値プロバイダーがもサポートされています</span><span class="sxs-lookup"><span data-stu-id="f92f6-238">Custom value providers are supported as well</span></span>

<span data-ttu-id="f92f6-239">ASP.NET MVC 4 を使用している場合、モデルのバインディング サポートは似ていますが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-239">If you have used ASP.NET MVC 4, you will notice the model binding support is similar.</span></span> <span data-ttu-id="f92f6-240">実際には、これらの機能が ASP.NET MVC から取得されに移動された、 **System.Web**も Web フォームで使用できるアセンブリ。</span><span class="sxs-lookup"><span data-stu-id="f92f6-240">Indeed, these features were taken from ASP.NET MVC and moved into the **System.Web** assembly to be able to use them on Web Forms as well.</span></span>

<span data-ttu-id="f92f6-241">このタスクでにはモデル バインドで filter パラメーターを受け取る、各カテゴリの製品の量によって、結果をフィルター処理する GridView 更新されます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-241">In this task, you will update the GridView to filter its results by the amount of products for each category, receiving the filter parameter with model binding.</span></span>

1. <span data-ttu-id="f92f6-242">戻り、**繰り返し**ページ。</span><span class="sxs-lookup"><span data-stu-id="f92f6-242">Go back to the **Products.aspx** page.</span></span>
2. <span data-ttu-id="f92f6-243">GridView の上部にある追加、**ラベル**と**ComboBox**を次に示すように各カテゴリの製品の数を選択します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-243">At the top of the GridView, add a **Label** and a **ComboBox** to select the number of products for each category as shown below.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. <span data-ttu-id="f92f6-244">追加、**後**製品の選択した数のカテゴリがない場合にメッセージを表示する GridView にします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-244">Add an **EmptyDataTemplate** to the GridView to show a message when there are no categories with the selected number of products.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. <span data-ttu-id="f92f6-245">開く、 **Products.aspx.cs**分離コードと、次を追加するステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-245">Open the **Products.aspx.cs** code-behind and add the following using statement.</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. <span data-ttu-id="f92f6-246">変更、 **GetCategories**を整数値を受け取るメソッド**minProductsCount**引数と、返された結果をフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-246">Modify the **GetCategories** method to receive an integer **minProductsCount** argument and filter the returned results.</span></span> <span data-ttu-id="f92f6-247">これを行うには、次のコードでメソッドを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-247">To do this, replace the method with the following code.</span></span>

    <span data-ttu-id="f92f6-248">(コード スニペット - *Web フォーム ラボ - Ex01 - GetCategories 2*)</span><span class="sxs-lookup"><span data-stu-id="f92f6-248">(Code Snippet - *Web Forms Lab - Ex01 - GetCategories 2*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    <span data-ttu-id="f92f6-249">新しい **[Control]** 属性を**minProductsCount**引数は ASP.NET ページにコントロールを使用してその値を設定する必要がありますを知ることができます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-249">The new **[Control]** attribute on the **minProductsCount** argument will let ASP.NET know its value must be populated using a control in the page.</span></span> <span data-ttu-id="f92f6-250">ASP.NET の引数 (minProductsCount) の名前と一致する任意のコントロールを検索および必要なマッピングとコントロールの値をパラメーターに入力への変換を実行します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-250">ASP.NET will look for any control matching the name of the argument (minProductsCount) and perform the necessary mapping and conversion to fill the parameter with the control value.</span></span>

    <span data-ttu-id="f92f6-251">また、属性は、コントロール値を取得する場所からを指定することができます、オーバー ロードされたコンス トラクターを提供します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-251">Alternatively, the attribute provides an overloaded constructor that enables you to specify the control from where to get the value.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f92f6-252">データ バインド機能の 1 つの目的は、ページの相互作用を記述する必要があるコードの量を削減します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-252">One goal of the data-binding features is to reduce the amount of code that needs to be written for page interaction.</span></span> <span data-ttu-id="f92f6-253">別に、[コントロール] の値プロバイダーには、メソッドのパラメーターでその他のモデル バインド プロバイダーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-253">Apart from the [Control] value provider, you can use other model-binding providers in your method parameters.</span></span> <span data-ttu-id="f92f6-254">その一部は、タスクの概要に表示されます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-254">Some of them are listed in the task introduction.</span></span>
6. <span data-ttu-id="f92f6-255">キーを押して**F5**サイトのデバッグを開始し、[製品] ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-255">Press **F5** to start debugging the site and go to the Products page.</span></span> <span data-ttu-id="f92f6-256">ドロップダウン リストで製品の数を選択し、GridView を今すぐ更新方法に注意してください。</span><span class="sxs-lookup"><span data-stu-id="f92f6-256">Select a number of products in the drop-down list and notice how the GridView is now updated.</span></span>

    <span data-ttu-id="f92f6-257">![ドロップダウン リストの値を持つ GridView をフィルタ リング](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "GridView で、ドロップダウン リストの値をフィルター処理")</span><span class="sxs-lookup"><span data-stu-id="f92f6-257">![Filtering the GridView with a drop-down list value](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "Filtering the GridView with a drop-down list value")</span></span>

    <span data-ttu-id="f92f6-258">*ドロップダウン リストの値を持つ GridView をフィルター処理*</span><span class="sxs-lookup"><span data-stu-id="f92f6-258">*Filtering the GridView with a drop-down list value*</span></span>
7. <span data-ttu-id="f92f6-259">デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-259">Stop debugging.</span></span>

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a><span data-ttu-id="f92f6-260">タスク 4 - を使用してモデルをフィルター処理するためのバインド</span><span class="sxs-lookup"><span data-stu-id="f92f6-260">Task 4 - Using Model Binding for Filtering</span></span>

<span data-ttu-id="f92f6-261">このタスクでは、秒、選択したカテゴリに含まれる製品を表示する GridView の子を追加します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-261">In this task, you will add a second, child GridView to show the products within the selected category.</span></span>

1. <span data-ttu-id="f92f6-262">開く、**ディスク上のファイル**ページし、更新プログラムのカテゴリの選択ボタンを自動生成する GridView。</span><span class="sxs-lookup"><span data-stu-id="f92f6-262">Open the **Products.aspx** page and update the categories GridView to auto-generate the Select button.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. <span data-ttu-id="f92f6-263">1 秒あたりの追加**GridView**という**productsGrid**下部にあります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-263">Add a second **GridView** named **productsGrid** at the bottom.</span></span> <span data-ttu-id="f92f6-264">設定、 **ItemType**に**WebFormsLab.Model.Product**、 **DataKeyNames**に**ProductId**と**SelectMethod**に**GetProducts**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-264">Set the **ItemType** to **WebFormsLab.Model.Product**, the **DataKeyNames** to **ProductId** and the **SelectMethod** to **GetProducts**.</span></span> <span data-ttu-id="f92f6-265">設定**AutoGenerateColumns**に**false** ProductId、ProductName、説明、UnitPrice の列を追加します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-265">Set **AutoGenerateColumns** to **false** and add the columns for ProductId, ProductName, Description and UnitPrice.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. <span data-ttu-id="f92f6-266">開く、 **Products.aspx.cs**分離コード ファイル。</span><span class="sxs-lookup"><span data-stu-id="f92f6-266">Open the **Products.aspx.cs** code-behind file.</span></span> <span data-ttu-id="f92f6-267">実装、 **GetProducts**カテゴリ GridView からカテゴリ ID を受信して、製品をフィルター処理するメソッド。</span><span class="sxs-lookup"><span data-stu-id="f92f6-267">Implement the **GetProducts** method to receive the category ID from the category GridView and filter the products.</span></span> <span data-ttu-id="f92f6-268">モデル バインドで選択した行を使用してパラメーター値が設定されます、 **categoriesGrid**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-268">Model binding will set the parameter value using the selected row in the **categoriesGrid**.</span></span> <span data-ttu-id="f92f6-269">コントロールの名前と引数の名前が一致しないために、コントロールの値プロバイダー内コントロールの名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-269">Since the argument name and control name do not match, you should specify the name of the control in the Control value provider.</span></span>

    <span data-ttu-id="f92f6-270">(コード スニペット - *Web フォームのラボ - Ex01 - GetProducts*)</span><span class="sxs-lookup"><span data-stu-id="f92f6-270">(Code Snippet - *Web Forms Lab - Ex01 - GetProducts*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="f92f6-271">このアプローチが容易に単位これらのメソッドをテストします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-271">This approach makes it easier to unit test these methods.</span></span> <span data-ttu-id="f92f6-272">Web フォームが実行されていない、単体テストのコンテキストで、[Control] 属性では、特定のアクションは実行しません。</span><span class="sxs-lookup"><span data-stu-id="f92f6-272">On a unit test context, where Web Forms is not executing, the [Control] attribute will not perform any specific action.</span></span>
4. <span data-ttu-id="f92f6-273">開く、**ディスク上のファイル**ページおよび GridView の製品を検索します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-273">Open the **Products.aspx** page and locate the products GridView.</span></span> <span data-ttu-id="f92f6-274">選択した製品を編集するためのリンクを表示する GridView の製品を更新します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-274">Update the products GridView to show a link for editing the selected product.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. <span data-ttu-id="f92f6-275">開く、 **ProductDetails.aspx**分離コード ページし、置換、 **SelectProduct**メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="f92f6-275">Open the **ProductDetails.aspx** page code-behind and replace the **SelectProduct** method with the following code.</span></span>

    <span data-ttu-id="f92f6-276">(コード スニペット - *Web フォーム ラボ - Ex01 - SelectProduct メソッド*)</span><span class="sxs-lookup"><span data-stu-id="f92f6-276">(Code Snippet - *Web Forms Lab - Ex01 - SelectProduct Method*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="f92f6-277">注意、 **[QueryString]** 属性を使用して、クエリ文字列で productId パラメーターからメソッド パラメーターを入力します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-277">Notice that the **[QueryString]** attribute is used to fill the method parameter from a productId parameter in the query string.</span></span>
6. <span data-ttu-id="f92f6-278">キーを押して**F5**サイトのデバッグを開始し、[製品] ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-278">Press **F5** to start debugging the site and go to the Products page.</span></span> <span data-ttu-id="f92f6-279">GridView のカテゴリから任意のカテゴリを選択し、GridView の製品が更新されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f92f6-279">Select any category from the categories GridView and notice that the products GridView is updated.</span></span>

    <span data-ttu-id="f92f6-280">![選択したカテゴリの製品を示す](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "選択したカテゴリからの製品を表示")</span><span class="sxs-lookup"><span data-stu-id="f92f6-280">![Showing products from the selected category](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "Showing products from the selected category")</span></span>

    <span data-ttu-id="f92f6-281">*選択したカテゴリの製品を表示*</span><span class="sxs-lookup"><span data-stu-id="f92f6-281">*Showing products from the selected category*</span></span>
7. <span data-ttu-id="f92f6-282">をクリックして、**ビュー** ProductDetails.aspx ページを開くの製品にリンクします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-282">Click the **View** link on a product to open the ProductDetails.aspx page.</span></span>

    <span data-ttu-id="f92f6-283">ページが、クエリ文字列から productId パラメーターを使用して、SelectMethod で製品を取得することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f92f6-283">Notice that the page is retrieving the product with the SelectMethod using the productId parameter from the query string.</span></span>

    <span data-ttu-id="f92f6-284">![製品の詳細を表示する](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "製品の詳細を表示します。")</span><span class="sxs-lookup"><span data-stu-id="f92f6-284">![Viewing the product details](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "Viewing the product details")</span></span>

    <span data-ttu-id="f92f6-285">*製品の詳細を表示します。*</span><span class="sxs-lookup"><span data-stu-id="f92f6-285">*Viewing the product details*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f92f6-286">HTML の説明を入力する機能は、次の手順で実装されます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-286">The ability to type an HTML description will be implemented in the next exercise.</span></span>

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a><span data-ttu-id="f92f6-287">タスク 5 - 更新操作のバインディングを使用してモデル</span><span class="sxs-lookup"><span data-stu-id="f92f6-287">Task 5 - Using Model Binding for Update Operations</span></span>

<span data-ttu-id="f92f6-288">前のタスクでは、データを選択するには、主にモデル バインドを使用した、このタスクでは、モデル バインドの更新操作で使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-288">In the previous task, you have used model binding mainly for selecting data, in this task you will learn how to use model binding in update operations.</span></span>

<span data-ttu-id="f92f6-289">カテゴリの更新プログラムのカテゴリのユーザーに GridView が更新されます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-289">You will update the categories GridView to let the user update categories.</span></span>

1. <span data-ttu-id="f92f6-290">開く、**繰り返し**ページし、更新プログラムのカテゴリを編集 ボタンを自動生成し、新しい GridView **UpdateMethod**属性を指定する、 **UpdateCategory**メソッドを選択した項目を更新します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-290">Open the **Products.aspx** page and update the categories GridView to auto-generate the Edit button and use the new **UpdateMethod** attribute to specify an **UpdateCategory** method to update the selected item.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    <span data-ttu-id="f92f6-291">Gridview DataKeyNames 属性を定義、モデル バインド オブジェクトを一意に識別されるメンバーであるし、update メソッドを受け取るには少なくともパラメーターであるため、します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-291">The DataKeyNames attribute in the GridView define which are the members that uniquely identify the model-bound object and therefore, which are the parameters the update method should at least receive.</span></span>
2. <span data-ttu-id="f92f6-292">開く、 **Products.aspx.cs**分離コード ファイルと実装、 **UpdateCategory**メソッド。</span><span class="sxs-lookup"><span data-stu-id="f92f6-292">Open the **Products.aspx.cs** code-behind file and implement the **UpdateCategory** method.</span></span> <span data-ttu-id="f92f6-293">メソッドは、現在のカテゴリを読み込む、GridView から値を設定し、カテゴリを更新するカテゴリ ID を受信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-293">The method should receive the category ID to load the current category, populate the values from the GridView and then update the category.</span></span>

    <span data-ttu-id="f92f6-294">(コード スニペット - *Web フォームのラボ - Ex01 - UpdateCategory*)</span><span class="sxs-lookup"><span data-stu-id="f92f6-294">(Code Snippet - *Web Forms Lab - Ex01 - UpdateCategory*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    <span data-ttu-id="f92f6-295">新しい**tryupdatemodel に渡します**設定 ページでコントロールから値を使用して、モデル オブジェクトのページ クラスのメソッドが担当します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-295">The new **TryUpdateModel** method in the Page class is responsible of populating the model object using the values from the controls in the page.</span></span> <span data-ttu-id="f92f6-296">この場合に編集されている現在の GridView 行から更新された値に置き換わります、**カテゴリ**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f92f6-296">In this case, it will replace the updated values from the current GridView row being edited into the **category** object.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f92f6-297">次の手順では、オブジェクトを編集するときに、ユーザーが入力したデータの検証、ModelState.IsValid の使用方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-297">The next exercise will explain the usage of the ModelState.IsValid for validating the data entered by the user when editing the object.</span></span>
3. <span data-ttu-id="f92f6-298">サイトを実行し、[製品] ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-298">Run the site and go to the Products page.</span></span> <span data-ttu-id="f92f6-299">カテゴリを編集します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-299">Edit a category.</span></span> <span data-ttu-id="f92f6-300">新しい名前を入力し、クリックして**Update**変更を確定します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-300">Type a new name and then click **Update** to persist the changes.</span></span>

    <span data-ttu-id="f92f6-301">![カテゴリを編集](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "カテゴリの編集")</span><span class="sxs-lookup"><span data-stu-id="f92f6-301">![Editing categories](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "Editing categories")</span></span>

    <span data-ttu-id="f92f6-302">*カテゴリの編集*</span><span class="sxs-lookup"><span data-stu-id="f92f6-302">*Editing categories*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a><span data-ttu-id="f92f6-303">手順 2: データの検証</span><span class="sxs-lookup"><span data-stu-id="f92f6-303">Exercise 2: Data Validation</span></span>

<span data-ttu-id="f92f6-304">この演習では、ASP.NET 4.5 でのデータ検証の新機能について学びます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-304">In this exercise, you will learn about the new data validation features in ASP.NET 4.5.</span></span> <span data-ttu-id="f92f6-305">Web フォームで新しい控え目な検証機能を確認します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-305">You will check out the new unobtrusive validation features in Web Forms.</span></span> <span data-ttu-id="f92f6-306">ユーザー入力の検証をアプリケーションのモデル クラスでデータ注釈を使用して、最後に、オンまたはオフを個別のコントロール ページの要求の検証を有効にする方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-306">You will use data annotations in the application model classes for user input validation, and finally, you will learn how to turn on or off request validation to individual controls in a page.</span></span>

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a><span data-ttu-id="f92f6-307">タスク 1 - 控え目な検証</span><span class="sxs-lookup"><span data-stu-id="f92f6-307">Task 1 - Unobtrusive Validation</span></span>

<span data-ttu-id="f92f6-308">フォーム検証コントロールを含む複雑なデータは、コードの約 60% を表すことができます ページで JavaScript コードが多すぎるを生成する傾向があります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-308">Forms with complex data including validators tend to generate too much JavaScript code in the page, which can represent about 60% of the code.</span></span> <span data-ttu-id="f92f6-309">控え目な検証が有効になっている、HTML コードは、見た目と若干見やすくしています。</span><span class="sxs-lookup"><span data-stu-id="f92f6-309">With unobtrusive validation enabled, your HTML code will look cleaner and tidier.</span></span>

<span data-ttu-id="f92f6-310">このセクションでは、両方の構成によって生成された HTML コードを比較する ASP.NET での控えめな検証を有効になります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-310">In this section, you will enable unobtrusive validation in ASP.NET to compare the HTML code generated by both configurations.</span></span>

1. <span data-ttu-id="f92f6-311">開く**Visual Studio 2012**を開くと、**開始**ソリューション、 **Source\Ex2 Validation\Begin**このラボのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="f92f6-311">Open **Visual Studio 2012** and open the **Begin** solution located in the **Source\Ex2-Validation\Begin** folder of this lab.</span></span> <span data-ttu-id="f92f6-312">または、前の演習から既存のソリューションで作業を続けることができます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-312">Alternatively, you can continue working on your existing solution from the previous exercise.</span></span>

   1. <span data-ttu-id="f92f6-313">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-313">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="f92f6-314">ソリューション エクスプ ローラーで、をクリックして、 **WebFormsLab**プロジェクト**NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-314">To do this, in the Solution Explorer, click the **WebFormsLab** project **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="f92f6-315">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="f92f6-315">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="f92f6-316">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-316">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f92f6-317">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="f92f6-317">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="f92f6-318">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-318">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="f92f6-319">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-319">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="f92f6-320">キーを押して**F5** web アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-320">Press **F5** to start the web application.</span></span> <span data-ttu-id="f92f6-321">ページに、顧客 をクリックし、**新しい顧客を追加**リンク。</span><span class="sxs-lookup"><span data-stu-id="f92f6-321">Go to the Customers page and click the **Add a New Customer** link.</span></span>
3. <span data-ttu-id="f92f6-322">ブラウザーのページを右クリックし、選択**ソースの表示**アプリケーションによって生成された HTML コードを開くにはオプションです。</span><span class="sxs-lookup"><span data-stu-id="f92f6-322">Right-click on the browser page, and select **View Source** option to open the HTML code generated by the application.</span></span>

    <span data-ttu-id="f92f6-323">![ページの HTML コードを示す](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "ページの HTML コードの表示")</span><span class="sxs-lookup"><span data-stu-id="f92f6-323">![Showing the page HTML code](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "Showing the page HTML code")</span></span>

    <span data-ttu-id="f92f6-324">*ページの HTML コードの表示*</span><span class="sxs-lookup"><span data-stu-id="f92f6-324">*Showing the page HTML code*</span></span>
4. <span data-ttu-id="f92f6-325">ページのソース コードをスクロールし、ASP.NET の JavaScript コードとデータの検証コントロールがページで、検証を実行して、エラー一覧を表示が挿入されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f92f6-325">Scroll through the page source code and notice that ASP.NET has injected JavaScript code and data validators in the page to perform the validations and show the error list.</span></span>

    <span data-ttu-id="f92f6-326">![CustomerDetails ページの JavaScript コードを検証](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "CustomerDetails ページの JavaScript の検証コード")</span><span class="sxs-lookup"><span data-stu-id="f92f6-326">![Validation JavaScript code in CustomerDetails page](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "Validation JavaScript code in CustomerDetails page")</span></span>

    <span data-ttu-id="f92f6-327">*CustomerDetails ページで JavaScript コードの検証*</span><span class="sxs-lookup"><span data-stu-id="f92f6-327">*Validation JavaScript code in CustomerDetails page*</span></span>
5. <span data-ttu-id="f92f6-328">ブラウザーを閉じて、Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-328">Close the browser and go back to Visual Studio.</span></span>
6. <span data-ttu-id="f92f6-329">今すぐ控え目な検証を有効になります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-329">Now you will enable unobtrusive validation.</span></span> <span data-ttu-id="f92f6-330">開いている**Web.Config**探し**ValidationSettings:UnobtrusiveValidationMode**キー、 **AppSettings**セクション**します。**</span><span class="sxs-lookup"><span data-stu-id="f92f6-330">Open **Web.Config** and locate **ValidationSettings:UnobtrusiveValidationMode** key in the **AppSettings** section **.**</span></span> <span data-ttu-id="f92f6-331">キーの値を設定**WebForms**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-331">Set the key value to **WebForms**.</span></span>

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > <span data-ttu-id="f92f6-332">このプロパティを設定することも、 &quot;**ページ\_ロード**&quot;控え目な検証をいくつかのページに対してのみ有効にする場合のイベント。</span><span class="sxs-lookup"><span data-stu-id="f92f6-332">You can also set this property in the &quot;**Page\_Load**&quot; event in case you want to enable Unobtrusive Validation only for some pages.</span></span>
7. <span data-ttu-id="f92f6-333">開いている**CustomerDetails.aspx**キーを押します**F5** Web アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-333">Open **CustomerDetails.aspx** and press **F5** to start the Web application.</span></span>
8. <span data-ttu-id="f92f6-334">IE の開発者ツールを開く F12 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-334">Press the F12 key to open the IE developer tools.</span></span> <span data-ttu-id="f92f6-335">開発者ツールが開いたら、スクリプト タブを選択します。選択**CustomerDetails.aspx**ローカル サイトからブラウザーに読み込まれたページに jQuery を実行するスクリプトが必要なことに注意してください メニューおよび take から。</span><span class="sxs-lookup"><span data-stu-id="f92f6-335">Once the developer tools is open, select the script tab. Select **CustomerDetails.aspx** from the menu and take note that the scripts required to run jQuery on the page have been loaded into the browser from the local site.</span></span>

    <span data-ttu-id="f92f6-336">![ローカル IIS サーバーから直接ファイルを読み込み、jQuery JavaScript](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "ローカル IIS サーバーから直接ファイル jQuery JavaScript の読み込み")</span><span class="sxs-lookup"><span data-stu-id="f92f6-336">![Loading the jQuery JavaScript files directly from the local IIS server](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "Loading the jQuery JavaScript files directly from the local IIS server")</span></span>

    <span data-ttu-id="f92f6-337">*ローカル IIS サーバーから直接、jQuery JavaScript ファイルの読み込み*</span><span class="sxs-lookup"><span data-stu-id="f92f6-337">*Loading the jQuery JavaScript files directly from the local IIS server*</span></span>
9. <span data-ttu-id="f92f6-338">Visual Studio に戻るにブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-338">Close the browser to return to Visual Studio.</span></span> <span data-ttu-id="f92f6-339">開く、 **Site.Master**ファイルを再び探し、 **ScriptManager**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-339">Open the **Site.Master** file again and locate the **ScriptManager**.</span></span> <span data-ttu-id="f92f6-340">属性を追加**EnableCdn**プロパティ値を持つ**True**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-340">Add the attribute **EnableCdn** property with the value **True**.</span></span> <span data-ttu-id="f92f6-341">これで、ローカル サイトの URL ではなく、オンラインの URL から読み込まれる jQuery です。</span><span class="sxs-lookup"><span data-stu-id="f92f6-341">This will force jQuery to be loaded from the online URL, not from the local site's URL.</span></span>
10. <span data-ttu-id="f92f6-342">開いている**CustomerDetails.aspx** Visual Studio でします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-342">Open **CustomerDetails.aspx** in Visual Studio.</span></span> <span data-ttu-id="f92f6-343">サイトを実行する F5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-343">Press the F5 key to run the site.</span></span> <span data-ttu-id="f92f6-344">Internet Explorer が開いたら、開発者ツールを開く F12 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-344">Once Internet Explorer opens, press the F12 key to open the developer tools.</span></span> <span data-ttu-id="f92f6-345">選択、**スクリプト**タブをクリックし、ドロップダウン リストを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f92f6-345">Select the **Script** tab, and then take a look at the drop-down list.</span></span> <span data-ttu-id="f92f6-346">JQuery JavaScript ファイルが不要になった読み込まれているローカルのサイトからではなくオンライン jQuery CDN に注意してください。</span><span class="sxs-lookup"><span data-stu-id="f92f6-346">Note the jQuery JavaScript files are no longer being loaded from the local site, but rather from the online jQuery CDN.</span></span>

    <span data-ttu-id="f92f6-347">![ファイルを CDN から jQuery JavaScript の読み込み](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "ファイル、CDN から jQuery JavaScript の読み込み")</span><span class="sxs-lookup"><span data-stu-id="f92f6-347">![Loading the jQuery JavaScript files from the CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "Loading the jQuery JavaScript files from the CDN")</span></span>

    <span data-ttu-id="f92f6-348">*CDN から jQuery JavaScript ファイルの読み込み*</span><span class="sxs-lookup"><span data-stu-id="f92f6-348">*Loading the jQuery JavaScript files from the CDN*</span></span>
11. <span data-ttu-id="f92f6-349">もう一度表示ソース オプションを使用して、ブラウザーで HTML ページのソース コードを開きます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-349">Open the HTML page source code again using the View source option in the browser.</span></span> <span data-ttu-id="f92f6-350">通知を控え目な検証を有効にすると ASP.NET が挿入された JavaScript コードに置き換えデータ-\*属性。</span><span class="sxs-lookup"><span data-stu-id="f92f6-350">Notice that by enabling the unobtrusive validation ASP.NET has replaced the injected JavaScript code with data- \*attributes.</span></span>

    <span data-ttu-id="f92f6-351">![控え目な検証コード](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "控え目な検証コード")</span><span class="sxs-lookup"><span data-stu-id="f92f6-351">![Unobtrusive validation code](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "Unobtrusive validation code")</span></span>

    <span data-ttu-id="f92f6-352">*控え目な検証コード*</span><span class="sxs-lookup"><span data-stu-id="f92f6-352">*Unobtrusive validation code*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f92f6-353">この例で HTML および JavaScript 数行のみに簡略化された概要データ注釈検証する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="f92f6-353">In this example, you saw how a validation summary with Data annotations was simplified to only a few HTML and JavaScript lines.</span></span> <span data-ttu-id="f92f6-354">以前は、控え目な検証が、追加すると、複数の検証コントロールのサイズが大きく、JavaScript の検証コードが増えます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-354">Previously, without unobtrusive validation, the more validation controls you add, the bigger your JavaScript validation code will grow.</span></span>

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a><span data-ttu-id="f92f6-355">タスク 2 - データ注釈を使用したモデルの検証</span><span class="sxs-lookup"><span data-stu-id="f92f6-355">Task 2 - Validating the Model with Data Annotations</span></span>

<span data-ttu-id="f92f6-356">ASP.NET 4.5 では、Web フォームのデータ注釈検証について説明します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-356">ASP.NET 4.5 introduces data annotations validation for Web Forms.</span></span> <span data-ttu-id="f92f6-357">各入力の検証コントロールはなく、ここで、モデル クラスで制約を定義し、すべての web アプリケーション全体で使用できます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-357">Instead of having a validation control on each input, you can now define constraints in your model classes and use them across all your web application.</span></span> <span data-ttu-id="f92f6-358">このセクションでは、顧客の新規作成/編集フォームを検証するためのデータ注釈を使用する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-358">In this section, you will learn how to use data annotations for validating a new/edit customer form.</span></span>

1. <span data-ttu-id="f92f6-359">開いている**CustomerDetail.aspx**ページ。</span><span class="sxs-lookup"><span data-stu-id="f92f6-359">Open **CustomerDetail.aspx** page.</span></span> <span data-ttu-id="f92f6-360">顧客名し、2 つ目の名前が、**後**と**後**のセクションでは、RequiredFieldValidator コントロールを使用して検証されます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-360">Notice that the customer first name and second name in the **EditItemTemplate** and **InsertItemTemplate** sections are validated using a RequiredFieldValidator controls.</span></span> <span data-ttu-id="f92f6-361">チェックする条件として、多くの検証コントロールを含める必要があるために、個々 の検証は、特定の条件に関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="f92f6-361">Each validator is associated to a particular condition, so you need to include as many validators as conditions to check.</span></span>
2. <span data-ttu-id="f92f6-362">顧客のモデル クラスを検証するデータの注釈を追加します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-362">Add data annotations to validate the Customer model class.</span></span> <span data-ttu-id="f92f6-363">開いている**Customer.cs**クラス、**モデル**フォルダーと*装飾*データ注釈属性を使用して各プロパティ。</span><span class="sxs-lookup"><span data-stu-id="f92f6-363">Open **Customer.cs** class in the **Model** folder and *decorate* each property using data annotation attributes.</span></span>

    <span data-ttu-id="f92f6-364">(コード スニペット - *Web フォーム ラボ - Ex02 - データ注釈*)</span><span class="sxs-lookup"><span data-stu-id="f92f6-364">(Code Snippet - *Web Forms Lab - Ex02 - Data Annotations*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > <span data-ttu-id="f92f6-365">.NET framework 4.5 では、既存のデータ注釈のコレクションを拡張しました。</span><span class="sxs-lookup"><span data-stu-id="f92f6-365">.NET Framework 4.5 has extended the existing data annotation collection.</span></span> <span data-ttu-id="f92f6-366">これらは、一部のデータ注釈を使用することができます: [CreditCard] [Phone] [EmailAddress]、[範囲] [比較]、[Url] [FileExtensions]、[Required]、[Key]、[正規表現]。</span><span class="sxs-lookup"><span data-stu-id="f92f6-366">These are some of the data annotations you can use: [CreditCard], [Phone], [EmailAddress], [Range], [Compare], [Url], [FileExtensions], [Required], [Key], [RegularExpression].</span></span>
    > 
    > <span data-ttu-id="f92f6-367">いくつかの使用例:</span><span class="sxs-lookup"><span data-stu-id="f92f6-367">Some usage examples:</span></span>
    > 
    > [Key]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > <span data-ttu-id="f92f6-369">各属性内の独自のエラー メッセージを定義することもできます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-369">You can also define your own error messages within each attribute.</span></span>
3. <span data-ttu-id="f92f6-370">開いている**CustomerDetails.aspx**と FormView コントロールの後と後のセクションで、最初と最後の名前フィールドのすべての RequiredFieldvalidators を削除します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-370">Open **CustomerDetails.aspx** and remove all the RequiredFieldvalidators for the first and last name fields in the in EditItemTemplate and InsertItemTemplate sections of the FormView control.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > <span data-ttu-id="f92f6-371">データ注釈を使用する利点の 1 つは、アプリケーション ページに検証ロジックが重複していないことです。</span><span class="sxs-lookup"><span data-stu-id="f92f6-371">One advantage of using data annotations is that validation logic is not duplicated in your application pages.</span></span> <span data-ttu-id="f92f6-372">モデルでは、一度定義し、そのデータを操作するすべてのアプリケーション ページを使用します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-372">You define it once in the model, and use it across all the application pages that manipulate data.</span></span>
4. <span data-ttu-id="f92f6-373">開いている**CustomerDetails.aspx**分離コードと SaveCustomer メソッドを検索します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-373">Open **CustomerDetails.aspx** code-behind and locate the SaveCustomer method.</span></span> <span data-ttu-id="f92f6-374">このメソッドは、新しい顧客を挿入するときに呼び出されるし、FormView コントロールの値から顧客のパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-374">This method is called when inserting a new customer and receives the Customer parameter from the FormView control values.</span></span> <span data-ttu-id="f92f6-375">ページ コントロールとパラメーターのオブジェクトも間のマッピング、ASP.NET は実行時に、モデルのすべてのデータ注釈検証は属性し、存在する場合に発生したエラーのある ModelState ディクショナリを入力します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-375">When the mapping between the page controls and the parameter object occurrs, ASP.NET will execute the model validation against all the data annotation attributes and fill the ModelState dictionary with the errors encountered, if any.</span></span>

    <span data-ttu-id="f92f6-376">ModelState.IsValid は、検証を実行した後、モデルのすべてのフィールドが有効な場合だけです。</span><span class="sxs-lookup"><span data-stu-id="f92f6-376">The ModelState.IsValid will only return true if all the fields on your model are valid after performing the validation.</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. <span data-ttu-id="f92f6-377">追加、 **ValidationSummary**モデル エラーの一覧を表示する CustomerDetails ページの末尾にあるコントロール。</span><span class="sxs-lookup"><span data-stu-id="f92f6-377">Add a **ValidationSummary** control at the end of the CustomerDetails page to show the list of model errors.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    <span data-ttu-id="f92f6-378">**ShowModelStateErrors**に設定すると新しいプロパティを ValidationSummary コントロール**true**、ModelState ディクショナリからのエラーをコントロールが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-378">The **ShowModelStateErrors** is a new property on the ValidationSummary control that when set to **true**, the control will show the errors from the ModelState dictionary.</span></span> <span data-ttu-id="f92f6-379">これらのエラーは、データ注釈検証から取得されます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-379">These errors come from the data annotations validation.</span></span>
6. <span data-ttu-id="f92f6-380">キーを押して**F5** Web アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-380">Press **F5** to run the Web application.</span></span> <span data-ttu-id="f92f6-381">エラーのあるいくつかの値をフォームに入力し、をクリックして**保存**検証を実行します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-381">Complete the form with some erroneous values and click **Save** to execute validation.</span></span> <span data-ttu-id="f92f6-382">エラーの下部にある概要を確認します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-382">Notice the error summary at the bottom.</span></span>

    <span data-ttu-id="f92f6-383">![データ注釈を使用した検証](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "データ注釈を使用した検証")</span><span class="sxs-lookup"><span data-stu-id="f92f6-383">![Validation with Data Annotations](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "Validation with Data Annotations")</span></span>

    <span data-ttu-id="f92f6-384">*データ注釈を使用した検証*</span><span class="sxs-lookup"><span data-stu-id="f92f6-384">*Validation with Data Annotations*</span></span>

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a><span data-ttu-id="f92f6-385">タスク 3 - ModelState とカスタム データベース エラーの処理</span><span class="sxs-lookup"><span data-stu-id="f92f6-385">Task 3 - Handling Custom Database Errors with ModelState</span></span>

<span data-ttu-id="f92f6-386">以前のバージョンの Web フォームでは、長すぎる文字列または一意キー違反などのデータベース エラーの処理が係わりますリポジトリ コードにおける例外のスローとエラーを表示する分離コードで例外を処理します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-386">In previous version of Web Forms, handling database errors such as a too long string or a unique key violation could involve throwing exceptions in your repository code and then handling the exceptions on your code-behind to display an error.</span></span> <span data-ttu-id="f92f6-387">優れたコード量が比較的単純な処理を行う必要です。</span><span class="sxs-lookup"><span data-stu-id="f92f6-387">A great amount of code is required to do something relatively simple.</span></span>

<span data-ttu-id="f92f6-388">Web フォームの 4.5 では、一貫した方法で、ページで、モデルとは、データベースから、エラーを表示する ModelState オブジェクトを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-388">In Web Forms 4.5, the ModelState object can be used to display the errors on the page, either from your model or from the database, in a consistent manner.</span></span>

<span data-ttu-id="f92f6-389">このタスクでは、適切にデータベースの例外を処理し、ModelState オブジェクトを使用して、ユーザーに適切なメッセージを表示するためのコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-389">In this task, you will add code to properly handle database exceptions and show the appropriate message to the user using the ModelState object.</span></span>

1. <span data-ttu-id="f92f6-390">アプリケーションが実行中でも、重複した値を使用してカテゴリの名前を更新してみてください。</span><span class="sxs-lookup"><span data-stu-id="f92f6-390">While the application is still running, try to update the name of a category using a duplicated value.</span></span>

    <span data-ttu-id="f92f6-391">![重複した名前とカテゴリを更新](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "重複した名前とカテゴリを更新しています")</span><span class="sxs-lookup"><span data-stu-id="f92f6-391">![Updating a category with a duplicated name](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "Updating a category with a duplicated name")</span></span>

    <span data-ttu-id="f92f6-392">*重複した名前とカテゴリを更新しています*</span><span class="sxs-lookup"><span data-stu-id="f92f6-392">*Updating a category with a duplicated name*</span></span>

    <span data-ttu-id="f92f6-393">ために、例外がスローされますが、&quot;一意&quot;の制約、 **CategoryName**列。</span><span class="sxs-lookup"><span data-stu-id="f92f6-393">Notice that an exception is thrown due to the &quot;unique&quot; constraint of the **CategoryName** column.</span></span>

    <span data-ttu-id="f92f6-394">![重複しているカテゴリの名前は、例外を](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "重複しているカテゴリの名前は、例外")</span><span class="sxs-lookup"><span data-stu-id="f92f6-394">![Exception for duplicated category names](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "Exception for duplicated category names")</span></span>

    <span data-ttu-id="f92f6-395">*重複しているカテゴリの名前は、例外*</span><span class="sxs-lookup"><span data-stu-id="f92f6-395">*Exception for duplicated category names*</span></span>
2. <span data-ttu-id="f92f6-396">デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-396">Stop debugging.</span></span> <span data-ttu-id="f92f6-397">**Products.aspx.cs**分離コード ファイル、更新、 **UpdateCategory** db によってスローされた例外を処理するメソッド。SaveChanges() メソッドの呼び出しを追加するとエラー、 **ModelState**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="f92f6-397">In the **Products.aspx.cs** code-behind file, update the **UpdateCategory** method to handle the exceptions thrown by the db.SaveChanges() method call and add an error to the **ModelState** object.</span></span>

    <span data-ttu-id="f92f6-398">新しい**tryupdatemodel に渡します**メソッドは、ユーザーによって指定されたフォーム データを使用して、データベースから取得したカテゴリ オブジェクトを更新します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-398">The new **TryUpdateModel** method updates the category object retrieved from the database using the form data provided by the user.</span></span>

    <span data-ttu-id="f92f6-399">(コード スニペット - *Web フォーム ラボ - Ex02 - UpdateCategory ハンドル エラー*)</span><span class="sxs-lookup"><span data-stu-id="f92f6-399">(Code Snippet - *Web Forms Lab - Ex02 - UpdateCategory Handle Errors*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > <span data-ttu-id="f92f6-400">理想的には、DbUpdateException の原因を特定し、根本原因が一意キー制約の違反を確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-400">Ideally, you would have to identify the cause of the DbUpdateException and check if the root cause is the violation of a unique key constraint.</span></span>
3. <span data-ttu-id="f92f6-401">開いている**ディスク上のファイル**を追加し、 **ValidationSummary**モデル エラーの一覧を表示する GridView のカテゴリの下のコントロール。</span><span class="sxs-lookup"><span data-stu-id="f92f6-401">Open **Products.aspx** and add a **ValidationSummary** control below the categories GridView to show the list of model errors.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. <span data-ttu-id="f92f6-402">サイトを実行し、[製品] ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-402">Run the site and go to the Products page.</span></span> <span data-ttu-id="f92f6-403">重複した値を使用してカテゴリの名前を更新しようとしてください。</span><span class="sxs-lookup"><span data-stu-id="f92f6-403">Try to update the name of a category using an duplicated value.</span></span>

    <span data-ttu-id="f92f6-404">例外の処理し、エラー メッセージに表示されますが、 **ValidationSummary**コントロール。</span><span class="sxs-lookup"><span data-stu-id="f92f6-404">Notice that the exception was handled and the error message appears in the **ValidationSummary** control.</span></span>

    <span data-ttu-id="f92f6-405">![カテゴリのエラーが重複して](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "カテゴリのエラーが重複しています")</span><span class="sxs-lookup"><span data-stu-id="f92f6-405">![Duplicated category error](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "Duplicated category error")</span></span>

    <span data-ttu-id="f92f6-406">*重複しているカテゴリのエラー*</span><span class="sxs-lookup"><span data-stu-id="f92f6-406">*Duplicated category error*</span></span>

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a><span data-ttu-id="f92f6-407">タスク 4 - 要求の ASP.NET Web フォーム 4.5 での検証</span><span class="sxs-lookup"><span data-stu-id="f92f6-407">Task 4 - Request Validation in ASP.NET Web Forms 4.5</span></span>

<span data-ttu-id="f92f6-408">Asp.net 要求の検証機能では、特定のレベルのクロス サイト スクリプティング (XSS) 攻撃に対する既定の保護を提供します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-408">The request validation feature in ASP.NET provides a certain level of default protection against cross-site scripting (XSS) attacks.</span></span> <span data-ttu-id="f92f6-409">以前のバージョンの ASP.NET では、要求の検証が既定で有効になっているし、ページ全体を無効にするのみできます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-409">In previous versions of ASP.NET, request validation was enabled by default and could only be disabled for an entire page.</span></span> <span data-ttu-id="f92f6-410">ASP.NET Web フォームの新しいバージョンで今すぐ 1 つのコントロール用要求の検証を無効にする、遅延要求の検証を実行したり、(これを行う場合は慎重に!) が検証されていない要求のデータにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-410">With the new version of ASP.NET Web Forms you can now disable the request validation for a single control, perform lazy request validation or access un-validated request data (be careful if you do so!).</span></span>

1. <span data-ttu-id="f92f6-411">キーを押して**Ctrl + F5**にデバッグなしでサイトを開始し、[製品] ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-411">Press **Ctrl+F5** to start the site without debugging and go to the Products page.</span></span> <span data-ttu-id="f92f6-412">カテゴリを選択し、をクリックし、**編集**製品のいずれかでリンクします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-412">Select a category and then click the **Edit** link on any of the products.</span></span>
2. <span data-ttu-id="f92f6-413">危険性のあるコンテンツを含む、HTML タグを含むインスタンスの説明を入力します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-413">Type a description containing potentially dangerous content, for instance including HTML tags.</span></span> <span data-ttu-id="f92f6-414">要求の検証のためにスローされる例外の通知を実行します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-414">Take notice of the exception thrown due to the request validation.</span></span>

    <span data-ttu-id="f92f6-415">![製品の危険性のあるコンテンツを編集](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "危険性のあるコンテンツを含む製品の編集")</span><span class="sxs-lookup"><span data-stu-id="f92f6-415">![Editing a product with potentially dangerous content](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "Editing a product with potentially dangerous content")</span></span>

    <span data-ttu-id="f92f6-416">*製品の危険性のあるコンテンツの編集*</span><span class="sxs-lookup"><span data-stu-id="f92f6-416">*Editing a product with potentially dangerous content*</span></span>

    <span data-ttu-id="f92f6-417">![要求の検証のためにスローされる例外](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "要求の検証のためにスローされる例外")</span><span class="sxs-lookup"><span data-stu-id="f92f6-417">![Exception thrown due to request validation](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "Exception thrown due to request validation")</span></span>

    <span data-ttu-id="f92f6-418">*要求の検証のためにスローされる例外*</span><span class="sxs-lookup"><span data-stu-id="f92f6-418">*Exception thrown due to request validation*</span></span>
3. <span data-ttu-id="f92f6-419">ページを閉じるし、Visual Studio で、キーを押して**SHIFT + F5**デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-419">Close the page and, in Visual Studio, press **SHIFT+F5** to stop debugging.</span></span>
4. <span data-ttu-id="f92f6-420">開く、 **ProductDetails.aspx**ページし、検索、**説明**テキスト ボックス。</span><span class="sxs-lookup"><span data-stu-id="f92f6-420">Open the **ProductDetails.aspx** page and locate the **Description** TextBox.</span></span>
5. <span data-ttu-id="f92f6-421">新しい追加**ValidateRequestMode**テキスト ボックスにプロパティに値を設定し、**無効になっている**。</span><span class="sxs-lookup"><span data-stu-id="f92f6-421">Add the new **ValidateRequestMode** property to the TextBox and set its value to **Disabled**.</span></span>

    <span data-ttu-id="f92f6-422">新しい**ValidateRequestMode**属性は、各コントロールで細かく、要求の検証を無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-422">The new **ValidateRequestMode** attribute allows you to disable the request validation granularly on each control.</span></span> <span data-ttu-id="f92f6-423">これは、HTML コードが表示される可能性がありますが、残りのページの検証を保持するための入力を使用する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="f92f6-423">This is useful when you want to use an input that may receive HTML code, but want to keep the validation working for the rest of the page.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. <span data-ttu-id="f92f6-424">キーを押して**F5** web アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-424">Press **F5** to run the web application.</span></span> <span data-ttu-id="f92f6-425">製品の編集 ページを再度開くし、HTML タグを含む製品の説明を完了します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-425">Open the edit product page again and complete a product description including HTML tags.</span></span> <span data-ttu-id="f92f6-426">説明に HTML コンテンツを追加できるようになりましたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f92f6-426">Notice that you can now add HTML content to the description.</span></span>

    <span data-ttu-id="f92f6-427">![要求の検証、製品の説明を無効になっている](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "要求の検証、製品の説明を無効になっています")</span><span class="sxs-lookup"><span data-stu-id="f92f6-427">![Request validation disabled for the product description](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "Request validation disabled for the product description")</span></span>

    <span data-ttu-id="f92f6-428">*製品の説明を無効になっている要求の検証*</span><span class="sxs-lookup"><span data-stu-id="f92f6-428">*Request validation disabled for the product description*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f92f6-429">実稼働アプリケーションでは、唯一の安全な HTML タグが入力したかどうかを確認するユーザーが入力した HTML コードをサニタイズする必要があります (たとえば、あるありません&lt;スクリプト&gt;タグ)。</span><span class="sxs-lookup"><span data-stu-id="f92f6-429">In a production application, you should sanitize the HTML code entered by the user to make sure only safe HTML tags are entered (for example, there are no &lt;script&gt; tags).</span></span> <span data-ttu-id="f92f6-430">これを行うには、使用することができます[Microsoft Web Protection Library](https://www.nuget.org/packages/AntiXSS)します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-430">To do this, you can use [Microsoft Web Protection Library](https://www.nuget.org/packages/AntiXSS).</span></span>
7. <span data-ttu-id="f92f6-431">製品をもう一度編集します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-431">Edit the product again.</span></span> <span data-ttu-id="f92f6-432">[名前] フィールドに HTML コードを入力し、をクリックして**保存**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-432">Type HTML code in the Name field and click **Save**.</span></span> <span data-ttu-id="f92f6-433">要求の検証は、説明フィールドののみ無効にし、re フィールドの残りの部分はまだ危険性のあるコンテンツに対する検証に注意してください。</span><span class="sxs-lookup"><span data-stu-id="f92f6-433">Notice that Request Validation is only disabled for the Description field and the rest of the fields re still validated against the potentially dangerous content.</span></span>

    <span data-ttu-id="f92f6-434">![要求の検証の残りのフィールドで有効になっている](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "要求の検証の残りのフィールドで有効になっています。")</span><span class="sxs-lookup"><span data-stu-id="f92f6-434">![Request validation enabled in the rest of the fields](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "Request validation enabled in the rest of the fields")</span></span>

    <span data-ttu-id="f92f6-435">*残りのフィールドで有効になっている要求の検証*</span><span class="sxs-lookup"><span data-stu-id="f92f6-435">*Request validation enabled in the rest of the fields*</span></span>

    <span data-ttu-id="f92f6-436">ASP.NET Web フォーム 4.5 には、遅延要求の検証を実行する新しい要求の検証モードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f92f6-436">ASP.NET Web Forms 4.5 includes a new request validation mode to perform request validation lazily.</span></span> <span data-ttu-id="f92f6-437">設定する要求の検証モード**4.5**、コードへのアクセスの場合は、 *Request.Form [&quot;キー&quot;]*、ASP.NET 4.5 の要求の検証はのみトリガー要求の検証フォームのコレクションでその要素を特定します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-437">With the request validation mode set to **4.5**, if a piece of code accesses *Request.Form[&quot;key&quot;]*, ASP.NET 4.5's request validation will only trigger request validation for that specific element in the form collection.</span></span>

    <span data-ttu-id="f92f6-438">さらに、ASP.NET 4.5 には、Microsoft ANTI-XSS ライブラリ v4.0 からコア エンコーディング ルーチンが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f92f6-438">Additionally, ASP.NET 4.5 now includes core encoding routines from the Microsoft Anti-XSS Library v4.0.</span></span> <span data-ttu-id="f92f6-439">ANTI-XSS エンコーディング ルーチンは、新しいによって実装される*AntiXssEncoder* 、新しい型が見つかった**System.Web.Security.AntiXss**名前空間。</span><span class="sxs-lookup"><span data-stu-id="f92f6-439">The Anti-XSS encoding routines are implemented by the new *AntiXssEncoder* type found in the new **System.Web.Security.AntiXss** namespace.</span></span> <span data-ttu-id="f92f6-440">**EncoderType**パラメーターを使用するように構成*AntiXssEncoder*、すべての出力を新しいエンコード ルーチンを使用して ASP.NET 内で自動的にエンコードします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-440">With the **encoderType** parameter configured to use *AntiXssEncoder*, all output encoding within ASP.NET automatically uses the new encoding routines.</span></span>
8. <span data-ttu-id="f92f6-441">ASP.NET 4.5 の要求の検証には、データを要求されていない検証済みのアクセスもサポートしています。</span><span class="sxs-lookup"><span data-stu-id="f92f6-441">ASP.NET 4.5 request validation also supports un-validated access to request data.</span></span> <span data-ttu-id="f92f6-442">ASP.NET 4.5 で追加、新しいコレクションのプロパティを**HttpRequest**と呼ばれるオブジェクト**Unvalidated**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-442">ASP.NET 4.5 adds a new collection property to the **HttpRequest** object called **Unvalidated**.</span></span> <span data-ttu-id="f92f6-443">移動すると**HttpRequest.Unvalidated**のすべての要求のデータ、フォーム、クエリ文字列、Cookie、Url などの一般的な情報にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-443">When you navigate into **HttpRequest.Unvalidated** you have access to all of the common pieces of request data, including Forms, QueryStrings, Cookies, URLs, and so on.</span></span>

    <span data-ttu-id="f92f6-444">![Request.Unvalidated オブジェクト](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated オブジェクト")</span><span class="sxs-lookup"><span data-stu-id="f92f6-444">![Request.Unvalidated object](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated object")</span></span>

    <span data-ttu-id="f92f6-445">*Request.Unvalidated オブジェクト*</span><span class="sxs-lookup"><span data-stu-id="f92f6-445">*Request.Unvalidated object*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f92f6-446">**注意して HttpRequest.Unvalidated プロパティを使用してください。**</span><span class="sxs-lookup"><span data-stu-id="f92f6-446">**Please use the HttpRequest.Unvalidated property with caution!**</span></span> <span data-ttu-id="f92f6-447">危険なテキストのないをラウンドト リップし、無防備な顧客に表示にする未加工の要求データを慎重にカスタム検証を実行することを確認してください。</span><span class="sxs-lookup"><span data-stu-id="f92f6-447">Make sure you carefully perform custom validation on the raw request data to ensure that dangerous text is not round-tripped and rendered back to unsuspecting customers!</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a><span data-ttu-id="f92f6-448">手順 3: フォームの非同期ページの ASP.NET web 処理</span><span class="sxs-lookup"><span data-stu-id="f92f6-448">Exercise 3: Asynchronous Page Processing in ASP.NET Web Forms</span></span>

<span data-ttu-id="f92f6-449">この演習では、新しい非同期ページの ASP.NET Web フォームで機能を処理する導入する予定です。</span><span class="sxs-lookup"><span data-stu-id="f92f6-449">In this exercise, you will be introduced to the new asynchronous page processing features in ASP.NET Web Forms.</span></span>

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a><span data-ttu-id="f92f6-450">タスク 1 - 製品の更新の詳細をアップロードし、イメージを表示するページ</span><span class="sxs-lookup"><span data-stu-id="f92f6-450">Task 1 - Updating the Product Details Page to Upload and Show Images</span></span>

<span data-ttu-id="f92f6-451">このタスクで、製品の画像の URL を指定し、読み取り専用ビューで表示できるようにする製品の詳細ページが更新されます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-451">In this task, you will update the product details page to allow the user to specify an image URL for the product and display it in the read-only view.</span></span> <span data-ttu-id="f92f6-452">同期的にダウンロードして、指定されたイメージのローカル コピーを作成するされます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-452">You will create a local copy of the specified image by downloading it synchronously.</span></span> <span data-ttu-id="f92f6-453">次のタスクでは、非同期的に動作させるのには、この実装を更新します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-453">In the next task, you will update this implementation to make it work asynchronously.</span></span>

1. <span data-ttu-id="f92f6-454">開いている**Visual Studio 2012**を読み込むと、**開始**ソリューション**Source\Ex3 Async\Begin**このラボのフォルダーから。</span><span class="sxs-lookup"><span data-stu-id="f92f6-454">Open **Visual Studio 2012** and load the **Begin** solution located in **Source\Ex3-Async\Begin** from this lab's folder.</span></span> <span data-ttu-id="f92f6-455">または、前の演習から既存のソリューションで作業を続けることができます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-455">Alternatively, you can continue working on your existing solution from the previous exercises.</span></span>

   1. <span data-ttu-id="f92f6-456">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-456">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="f92f6-457">ソリューション エクスプ ローラーで、をクリックして、 **WebFormsLab**順に選択して**NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-457">To do this, in the Solution Explorer, click the **WebFormsLab** project and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="f92f6-458">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="f92f6-458">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="f92f6-459">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-459">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f92f6-460">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="f92f6-460">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="f92f6-461">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-461">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="f92f6-462">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-462">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="f92f6-463">開く、 **ProductDetails.aspx**ソース ページおよび製品イメージを表示するフォーム ビューの ItemTemplate でフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-463">Open the **ProductDetails.aspx** page source and add a field in the FormView's ItemTemplate to show the product image.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. <span data-ttu-id="f92f6-464">FormView の EditTemplate で画像の URL を指定するフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-464">Add a field to specify the image URL in the FormView's EditTemplate.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. <span data-ttu-id="f92f6-465">開く、 **ProductDetails.aspx.cs**分離コード ファイルを開き、次の名前空間ディレクティブを追加します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-465">Open the **ProductDetails.aspx.cs** code-behind file and add the following namespace directives.</span></span>

    <span data-ttu-id="f92f6-466">(コード スニペット - *Web フォームのラボ - Ex03 - 名前空間*)</span><span class="sxs-lookup"><span data-stu-id="f92f6-466">(Code Snippet - *Web Forms Lab - Ex03 - Namespaces*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. <span data-ttu-id="f92f6-467">作成、 **UpdateProductImage**メソッドをリモートのイメージをローカルに格納**イメージ**フォルダーと新しいイメージの場所の値を持つ製品エンティティを更新します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-467">Create an **UpdateProductImage** method to store remote images in the local **Images** folder and update the product entity with the new image location value.</span></span>

    <span data-ttu-id="f92f6-468">(コード スニペット - *Web フォームのラボ - Ex03 - UpdateProductImage*)</span><span class="sxs-lookup"><span data-stu-id="f92f6-468">(Code Snippet - *Web Forms Lab - Ex03 - UpdateProductImage*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. <span data-ttu-id="f92f6-469">更新プログラム、 **UpdateProduct**メソッドを呼び出す、 **UpdateProductImage**メソッド。</span><span class="sxs-lookup"><span data-stu-id="f92f6-469">Update the **UpdateProduct** method to call the **UpdateProductImage** method.</span></span>

    <span data-ttu-id="f92f6-470">(コード スニペット - *Web フォーム ラボ - Ex03 - UpdateProductImage 呼び出し*)</span><span class="sxs-lookup"><span data-stu-id="f92f6-470">(Code Snippet - *Web Forms Lab - Ex03 - UpdateProductImage Call*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. <span data-ttu-id="f92f6-471">アプリケーションを実行し、製品の画像をアップロードしようとしてください。</span><span class="sxs-lookup"><span data-stu-id="f92f6-471">Run the application and try to upload an image for a product.</span></span> <span data-ttu-id="f92f6-472">たとえば、Office クリップ Arts から次の画像の URL を使用できます。 [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)</span><span class="sxs-lookup"><span data-stu-id="f92f6-472">For example, you can use the following image URL from Office Clip Arts: [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)</span></span>

    <span data-ttu-id="f92f6-473">![製品の画像を設定](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "製品の画像を設定")</span><span class="sxs-lookup"><span data-stu-id="f92f6-473">![Setting an image for a product](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "Setting an image for a product")</span></span>

    <span data-ttu-id="f92f6-474">*製品の画像を設定*</span><span class="sxs-lookup"><span data-stu-id="f92f6-474">*Setting an image for a product*</span></span>

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a><span data-ttu-id="f92f6-475">タスク 2 - 非同期処理の製品詳細ページに追加します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-475">Task 2 - Adding Asynchronous Processing to the Product Details Page</span></span>

<span data-ttu-id="f92f6-476">このタスクで非同期的に連携できるようにする製品の詳細ページが更新されます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-476">In this task, you will update the product details page to make it work asynchronously.</span></span> <span data-ttu-id="f92f6-477">実行時間の長いタスク - イメージのダウンロード プロセスでは、ASP.NET 4.5 の非同期ページ処理を使用して強化されます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-477">You will enhance a long running task - the image download process - by using ASP.NET 4.5 asynchronous page processing.</span></span>

<span data-ttu-id="f92f6-478">Web アプリケーションでの非同期メソッドは、ASP.NET スレッド プールの使用方法を最適化するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-478">Asynchronous methods in web applications can be used to optimize the way ASP.NET thread pools are used.</span></span> <span data-ttu-id="f92f6-479">Asp.net がありますが在籍しているのためにスレッド プールのスレッド数が制限を要求するそのため、すべてのスレッドがビジー状態で ASP.NET 新しい要求を拒否、アプリケーションのエラー メッセージを送信します始まり、サイトを使用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-479">In ASP.NET there are a limited number of threads in the thread pool for attending requests, thus, when all the threads are busy, ASP.NET starts to reject new requests, sends application error messages and makes your site unavailable.</span></span>

<span data-ttu-id="f92f6-480">Web サイトで時間のかかる操作では、長時間割り当てられているスレッドを占有するために、非同期プログラミングの有力候補が。</span><span class="sxs-lookup"><span data-stu-id="f92f6-480">Time-consuming operations on your web site are great candidates for asynchronous programming because they occupy the assigned thread for a long time.</span></span> <span data-ttu-id="f92f6-481">これには、実行時間の長い要求、さまざまな要素の多くのページとオフライン操作、そのようなデータベース クエリを実行または外部 web サーバーへのアクセスを必要とするページが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-481">This includes long running requests, pages with lots of different elements and pages that require offline operations, such querying a database or accessing an external web server.</span></span> <span data-ttu-id="f92f6-482">利点は、こと、ページの処理中にこれらの操作を非同期メソッドを使用する場合、スレッドが解放され、スレッドに返されるプールし、新しいページ要求に使用できます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-482">The advantage is that if you use asynchronous methods for these operations, while the page is processing, the thread is freed and returned to the thread pool and can be used to attend to a new page request.</span></span> <span data-ttu-id="f92f6-483">この場合は、ページで、スレッド プールから 1 つのスレッドで処理を開始し、非同期処理が完了したら、別の処理を完了することがあります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-483">This means, the page will start processing in one thread from the thread pool and might complete processing in a different one, after the async processing completes.</span></span>

1. <span data-ttu-id="f92f6-484">開く、 **ProductDetails.aspx**ページ。</span><span class="sxs-lookup"><span data-stu-id="f92f6-484">Open the **ProductDetails.aspx** page.</span></span> <span data-ttu-id="f92f6-485">追加、 **Async**属性、**ページ**要素に設定し、 **true**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-485">Add the **Async** attribute in the **Page** element and set it to **true**.</span></span> <span data-ttu-id="f92f6-486">この属性は、IHttpAsyncHandler インターフェイスを実装するために ASP.NET に指示します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-486">This attribute tells ASP.NET to implement the IHttpAsyncHandler interface.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. <span data-ttu-id="f92f6-487">ページを実行しているスレッドの詳細を表示するページの下部にラベルを追加します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-487">Add a Label at the bottom of the page to show the details of the threads running the page.</span></span>

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. <span data-ttu-id="f92f6-488">開いて**ProductDetails.aspx.cs**し、次の名前空間ディレクティブを追加します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-488">Open up **ProductDetails.aspx.cs** and add the following namespace directives.</span></span>

    <span data-ttu-id="f92f6-489">(コード スニペット - *Web フォーム ラボ - Ex03 - 名前空間 2*)</span><span class="sxs-lookup"><span data-stu-id="f92f6-489">(Code Snippet - *Web Forms Lab - Ex03 - Namespaces 2*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. <span data-ttu-id="f92f6-490">変更、 **UpdateProductImage**メソッドの非同期タスクでイメージをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-490">Modify the **UpdateProductImage** method to download the image with an asynchronous task.</span></span> <span data-ttu-id="f92f6-491">置換は、 **WebClient** **DownloadFile**メソッドを**DownloadFileTaskAsync**メソッドを含めると、 **await**キーワード。</span><span class="sxs-lookup"><span data-stu-id="f92f6-491">You will replace the **WebClient** **DownloadFile** method with the **DownloadFileTaskAsync** method and include the **await** keyword.</span></span>

    <span data-ttu-id="f92f6-492">(コード スニペット - *Web フォーム ラボ - Ex03 - UpdateProductImage Async*)</span><span class="sxs-lookup"><span data-stu-id="f92f6-492">(Code Snippet - *Web Forms Lab - Ex03 - UpdateProductImage Async*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    <span data-ttu-id="f92f6-493">別のスレッドで実行される新しいページの非同期タスクを RegisterAsyncTask に登録します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-493">The RegisterAsyncTask registers a new page asynchronous task to be executed in a different thread.</span></span> <span data-ttu-id="f92f6-494">タスク (t) を実行すると、ラムダ式を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-494">It receives a lambda expression with the Task (t) to be executed.</span></span> <span data-ttu-id="f92f6-495">**Await**キーワード、 **DownloadFileTaskAsync**メソッド、メソッドの残りの部分は後で非同期的に呼び出されるコールバックに変換、 **DownloadFileTaskAsync**メソッドが完了しました。</span><span class="sxs-lookup"><span data-stu-id="f92f6-495">The **await** keyword in the **DownloadFileTaskAsync** method converts the remainder of the method into a callback that is invoked asynchronously after the **DownloadFileTaskAsync** method has completed.</span></span> <span data-ttu-id="f92f6-496">ASP.NET では、HTTP 要求元のすべての値を自動的に維持することによって、メソッドの実行が再開されます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-496">ASP.NET will resume the execution of the method by automatically maintaining all the HTTP request original values.</span></span> <span data-ttu-id="f92f6-497">.NET 4.5 では、新しい非同期プログラミング モデルでは、次の同期コードの場合とほぼ同じな非同期コードを記述し、コンパイラは、コールバック機能または継続コードの複雑さを処理できるようにできます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-497">The new asynchronous programming model in .NET 4.5 enables you to write asynchronous code that looks very much like synchronous code, and let the compiler handle the complications of callback functions or continuation code.</span></span>
    > [!NOTE]
    > <span data-ttu-id="f92f6-498">RegisterAsyncTask と PageAsyncTask いた使用可能な .NET 2.0 以降。</span><span class="sxs-lookup"><span data-stu-id="f92f6-498">RegisterAsyncTask and PageAsyncTask were already available since .NET 2.0.</span></span> <span data-ttu-id="f92f6-499">Await キーワードは、.NET 4.5 の非同期プログラミング モデルから新しい .NET WebClient オブジェクトから新しい TaskAsync メソッドと共に使用できます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-499">The await keyword is new from the .NET 4.5 asynchronous programming model and can be used together with the new TaskAsync methods from the .NET WebClient object.</span></span>
5. <span data-ttu-id="f92f6-500">これでコードが開始され、実行が終了したスレッドを表示するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-500">Add code to display the threads on which the code started and finished executing.</span></span> <span data-ttu-id="f92f6-501">これを行うには、更新、 **UpdateProductImage**メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="f92f6-501">To do this, update the **UpdateProductImage** method with the following code.</span></span>

    <span data-ttu-id="f92f6-502">(コード スニペット - *Web フォーム ラボ - Ex03 - Show スレッド*)</span><span class="sxs-lookup"><span data-stu-id="f92f6-502">(Code Snippet - *Web Forms Lab - Ex03 - Show threads*)</span></span>

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. <span data-ttu-id="f92f6-503">開く web サイトの**Web.config**ファイル。</span><span class="sxs-lookup"><span data-stu-id="f92f6-503">Open the web site's **Web.config** file.</span></span> <span data-ttu-id="f92f6-504">次の appSetting の変数を追加します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-504">Add the following appSetting variable.</span></span>

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. <span data-ttu-id="f92f6-505">キーを押して**F5**にアプリケーションを実行し、製品の画像をアップロードします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-505">Press **F5** to run the application and upload an image for the product.</span></span> <span data-ttu-id="f92f6-506">スレッド ID、開始および終了コードが異なる場合がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f92f6-506">Notice the threads ID where the code started and finished may be different.</span></span> <span data-ttu-id="f92f6-507">ASP.NET スレッド プールから別のスレッドで非同期タスクを実行するためです。</span><span class="sxs-lookup"><span data-stu-id="f92f6-507">This is because asynchronous tasks run on a separate thread from ASP.NET thread pool.</span></span> <span data-ttu-id="f92f6-508">タスクが完了したら、ASP.NET は、キューに戻し、タスクを配置し、利用可能なスレッドのいずれかを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-508">When the task completes, ASP.NET puts the task back in the queue and assigns any of the available threads.</span></span>

    <span data-ttu-id="f92f6-509">![イメージの非同期的にダウンロード](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "非同期的にイメージのダウンロード")</span><span class="sxs-lookup"><span data-stu-id="f92f6-509">![Downloading an image asynchronously](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "Downloading an image asynchronously")</span></span>

    <span data-ttu-id="f92f6-510">*非同期のイメージのダウンロード*</span><span class="sxs-lookup"><span data-stu-id="f92f6-510">*Downloading an image asynchronously*</span></span>

> [!NOTE]
> <span data-ttu-id="f92f6-511">また、Azure の次に、このアプリケーションを展開できます[付録 b: 発行 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-511">Additionally, you can deploy this application to Azure following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="f92f6-512">まとめ</span><span class="sxs-lookup"><span data-stu-id="f92f6-512">Summary</span></span>

<span data-ttu-id="f92f6-513">このハンズオン ラボで、次の概念が対処して示されています。</span><span class="sxs-lookup"><span data-stu-id="f92f6-513">In this hands-on lab, the following concepts have been addressed and demonstrated:</span></span>

- <span data-ttu-id="f92f6-514">厳密に型指定されたデータ バインディング式を使用します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-514">Use strongly-typed data-binding expressions</span></span>
- <span data-ttu-id="f92f6-515">新しいモデル バインド機能を使用して、Web フォーム</span><span class="sxs-lookup"><span data-stu-id="f92f6-515">Use new model binding features in Web Forms</span></span>
- <span data-ttu-id="f92f6-516">コード分離メソッドにページ データをマッピングするための値プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-516">Use value providers for mapping page data to code-behind methods</span></span>
- <span data-ttu-id="f92f6-517">データ注釈を使用して、ユーザー入力の検証</span><span class="sxs-lookup"><span data-stu-id="f92f6-517">Use Data Annotations for user input validation</span></span>
- <span data-ttu-id="f92f6-518">Web フォームでの jquery クライアント側検証を unobstrusive advange がかかる</span><span class="sxs-lookup"><span data-stu-id="f92f6-518">Take advange of unobstrusive client-side validation with jQuery in Web Forms</span></span>
- <span data-ttu-id="f92f6-519">詳細な要求の検証を実装します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-519">Implement granular request validation</span></span>
- <span data-ttu-id="f92f6-520">非同期ページの Web フォームでの処理の実装します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-520">Implement asynchronous page processing in Web Forms</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="f92f6-521">付録 a: Installing Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="f92f6-521">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="f92f6-522">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="f92f6-522">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="f92f6-523">次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-523">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="f92f6-524">移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-524">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="f92f6-525">または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Azure SDK</em>&quot;します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-525">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="f92f6-526">をクリックして**を今すぐインストール**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-526">Click on **Install Now**.</span></span> <span data-ttu-id="f92f6-527">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-527">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="f92f6-528">1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-528">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="f92f6-529">![Visual Studio Express のインストール](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="f92f6-529">![Install Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="f92f6-530">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="f92f6-530">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="f92f6-531">すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-531">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    <span data-ttu-id="f92f6-533">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="f92f6-533">*Accepting the license terms*</span></span>
5. <span data-ttu-id="f92f6-534">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-534">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    <span data-ttu-id="f92f6-536">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="f92f6-536">*Installation progress*</span></span>
6. <span data-ttu-id="f92f6-537">インストールが完了したら、クリックして**完了**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-537">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    <span data-ttu-id="f92f6-539">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="f92f6-539">*Installation completed*</span></span>
7. <span data-ttu-id="f92f6-540">クリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-540">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="f92f6-541">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-541">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web のタイル](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    <span data-ttu-id="f92f6-543">*VS Express for Web のタイル*</span><span class="sxs-lookup"><span data-stu-id="f92f6-543">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="f92f6-544">付録 b: Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="f92f6-544">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="f92f6-545">この付録では、Azure Portal から新しい web サイトを作成して Azure によって提供される、Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを発行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-545">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="f92f6-546">タスク 1 - Azure Portal から新しい Web サイトの作成</span><span class="sxs-lookup"><span data-stu-id="f92f6-546">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="f92f6-547">移動して、 [Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-547">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f92f6-548">Azure を無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増加に応じてスケールできます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-548">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="f92f6-549">サインアップする[ここ](http://aka.ms/aspnet-hol-azure)します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-549">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="f92f6-550">![Windows Azure ポータルにログオン](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Windows Azure ポータルにログオン")</span><span class="sxs-lookup"><span data-stu-id="f92f6-550">![Log on to Windows Azure portal](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="f92f6-551">*ポータルにログオン*</span><span class="sxs-lookup"><span data-stu-id="f92f6-551">*Log on to the Portal*</span></span>
2. <span data-ttu-id="f92f6-552">クリックして**新規**コマンド バーにします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-552">Click **New** on the command bar.</span></span>

    <span data-ttu-id="f92f6-553">![新しい Web サイトを作成する](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="f92f6-553">![Creating a new Web Site](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="f92f6-554">*新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="f92f6-554">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="f92f6-555">クリックして**コンピューティング** | **Web サイト**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-555">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="f92f6-556">選び**簡易作成**オプション。</span><span class="sxs-lookup"><span data-stu-id="f92f6-556">Then select **Quick Create** option.</span></span> <span data-ttu-id="f92f6-557">新しい web サイトの URL を指定し、をクリックして**Web サイトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="f92f6-557">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f92f6-558">Azure は、制御および管理できる、クラウドで実行されている web アプリケーションのホストです。</span><span class="sxs-lookup"><span data-stu-id="f92f6-558">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="f92f6-559">簡易作成 オプションを使用すると、ポータルの外部から Azure に完成した web アプリケーションをデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-559">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="f92f6-560">データベースを設定するための手順は含まれません。</span><span class="sxs-lookup"><span data-stu-id="f92f6-560">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="f92f6-561">![簡易作成を使用して新しい Web サイトを作成する](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "簡易作成を使用して新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="f92f6-561">![Creating a new Web Site using Quick Create](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="f92f6-562">*簡易作成を使用して新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="f92f6-562">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="f92f6-563">新しいまで待つ**Web サイト**が作成されます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-563">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="f92f6-564">Web サイトを作成した後は、下のリンクをクリックして、 **URL**列。</span><span class="sxs-lookup"><span data-stu-id="f92f6-564">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="f92f6-565">新しい Web サイトが動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-565">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="f92f6-566">![新しい web サイトを参照](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "新しい web サイトを参照")</span><span class="sxs-lookup"><span data-stu-id="f92f6-566">![Browsing to the new web site](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="f92f6-567">*新しい web サイトを参照*</span><span class="sxs-lookup"><span data-stu-id="f92f6-567">*Browsing to the new web site*</span></span>

    <span data-ttu-id="f92f6-568">![Web サイトが実行されている](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "実行している Web サイト")</span><span class="sxs-lookup"><span data-stu-id="f92f6-568">![Web site running](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "Web site running")</span></span>

    <span data-ttu-id="f92f6-569">*実行している web サイト*</span><span class="sxs-lookup"><span data-stu-id="f92f6-569">*Web site running*</span></span>
6. <span data-ttu-id="f92f6-570">ポータルに戻り、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。</span><span class="sxs-lookup"><span data-stu-id="f92f6-570">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="f92f6-571">![Web サイトの管理ページを開く](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "web サイトの管理ページを開く")</span><span class="sxs-lookup"><span data-stu-id="f92f6-571">![Opening the web site management pages](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="f92f6-572">*Web サイトの管理ページを開く*</span><span class="sxs-lookup"><span data-stu-id="f92f6-572">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="f92f6-573">**ダッシュ ボード**ページの**概要**セクションで、、**発行プロファイルのダウンロード**リンク。</span><span class="sxs-lookup"><span data-stu-id="f92f6-573">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f92f6-574">*発行プロファイル*有効になっているパブリケーションの各メソッドには、Azure の web アプリケーションを発行するために必要な情報がすべて含まれます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-574">The *publish profile* contains all of the information required to publish a web application to Azure for each enabled publication method.</span></span> <span data-ttu-id="f92f6-575">発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベースの文字列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="f92f6-575">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="f92f6-576">**Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートと、発行プロファイルのこれらのプログラムの構成を自動化するにはAzure に web アプリケーションを公開します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-576">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="f92f6-577">![発行プロファイルのダウンロード web サイト](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "発行プロファイルの web サイトのダウンロード")</span><span class="sxs-lookup"><span data-stu-id="f92f6-577">![Downloading the web site publish profile](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="f92f6-578">*発行プロファイルの Web サイトのダウンロード*</span><span class="sxs-lookup"><span data-stu-id="f92f6-578">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="f92f6-579">既知の場所に発行プロファイル ファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-579">Download the publish profile file to a known location.</span></span> <span data-ttu-id="f92f6-580">さらにこの演習ではこのファイルを使用して、Visual Studio から Azure への web アプリケーションを発行する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-580">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="f92f6-581">![発行プロファイル ファイルを保存する](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "発行プロファイルを保存しています")</span><span class="sxs-lookup"><span data-stu-id="f92f6-581">![Saving the publish profile file](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "Saving the publish profile")</span></span>

    <span data-ttu-id="f92f6-582">*発行プロファイル ファイルを保存しています*</span><span class="sxs-lookup"><span data-stu-id="f92f6-582">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="f92f6-583">タスク 2 - データベース サーバーの構成</span><span class="sxs-lookup"><span data-stu-id="f92f6-583">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="f92f6-584">アプリケーションが SQL Server を使用する場合のデータベースは SQL Database サーバーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-584">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="f92f6-585">SQL Server を使用しない単純なアプリケーションをデプロイする場合は、このタスクをスキップする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-585">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="f92f6-586">SQL Database サーバーは、アプリケーション データベースを格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-586">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="f92f6-587">SQL Database サーバーを表示するには、サブスクリプションで Azure 管理ポータルで**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**.</span><span class="sxs-lookup"><span data-stu-id="f92f6-587">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="f92f6-588">使用して 1 つを作成するには作成したサーバーがない、**追加**コマンド バーのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-588">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="f92f6-589">メモ、**サーバー名および URL、管理者のログイン名とパスワード**次のタスクで使用すると、します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-589">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="f92f6-590">データベースを作成しない、まだ、後の段階でそれが作成されます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-590">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="f92f6-591">![SQL データベース サーバーのダッシュ ボード](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "SQL データベース サーバーのダッシュ ボード")</span><span class="sxs-lookup"><span data-stu-id="f92f6-591">![SQL Database Server Dashboard](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="f92f6-592">*SQL データベース サーバーのダッシュ ボード*</span><span class="sxs-lookup"><span data-stu-id="f92f6-592">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="f92f6-593">次のタスクでは、サーバーの一覧で、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-593">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="f92f6-594">次のようにクリックします**構成**、から IP アドレスを選択して**現在のクライアント IP アドレス**で貼り付けます、**開始 IP アドレス**と**終了 IP アドレス。** テキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png)ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-594">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) button.</span></span>

    ![クライアント IP アドレスを追加します。](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    <span data-ttu-id="f92f6-596">*クライアント IP アドレスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="f92f6-596">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="f92f6-597">1 回、**クライアント IP アドレス**が許可された IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-597">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![変更を確認します。](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    <span data-ttu-id="f92f6-599">*変更を確認します。*</span><span class="sxs-lookup"><span data-stu-id="f92f6-599">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="f92f6-600">タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="f92f6-600">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="f92f6-601">ASP.NET MVC 4 のソリューションに戻ります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-601">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="f92f6-602">**ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-602">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="f92f6-603">![アプリケーションの発行](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="f92f6-603">![Publishing the Application](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "Publishing the Application")</span></span>

    <span data-ttu-id="f92f6-604">*Web サイトの発行*</span><span class="sxs-lookup"><span data-stu-id="f92f6-604">*Publishing the web site*</span></span>
2. <span data-ttu-id="f92f6-605">最初のタスクで保存した発行プロファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-605">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="f92f6-606">![発行プロファイルをインポートする](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "発行プロファイルをインポートします。")</span><span class="sxs-lookup"><span data-stu-id="f92f6-606">![Importing the publish profile](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "Importing the publish profile")</span></span>

    <span data-ttu-id="f92f6-607">*発行プロファイルのインポート*</span><span class="sxs-lookup"><span data-stu-id="f92f6-607">*Importing publish profile*</span></span>
3. <span data-ttu-id="f92f6-608">クリックして**接続の検証**です。</span><span class="sxs-lookup"><span data-stu-id="f92f6-608">Click **Validate Connection**.</span></span> <span data-ttu-id="f92f6-609">検証が完了したら、クリックして**次**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-609">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f92f6-610">接続の検証ボタンの横に表示される緑のチェック マークが表示されたら、検証が完了しました。</span><span class="sxs-lookup"><span data-stu-id="f92f6-610">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="f92f6-611">![接続の検証](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "接続の検証")</span><span class="sxs-lookup"><span data-stu-id="f92f6-611">![Validating connection](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "Validating connection")</span></span>

    <span data-ttu-id="f92f6-612">*接続の検証*</span><span class="sxs-lookup"><span data-stu-id="f92f6-612">*Validating connection*</span></span>
4. <span data-ttu-id="f92f6-613">**設定**ページの**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックします (つまり**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="f92f6-613">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="f92f6-614">![Web 配置の構成](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Web 配置の構成")</span><span class="sxs-lookup"><span data-stu-id="f92f6-614">![Web deploy configuration](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Web deploy configuration")</span></span>

    <span data-ttu-id="f92f6-615">*Web 配置の構成*</span><span class="sxs-lookup"><span data-stu-id="f92f6-615">*Web deploy configuration*</span></span>
5. <span data-ttu-id="f92f6-616">データベース接続を次のように構成します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-616">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="f92f6-617">**サーバー名**SQL Database サーバーの URL を使用して、入力、 *tcp:* プレフィックス。</span><span class="sxs-lookup"><span data-stu-id="f92f6-617">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="f92f6-618">**ユーザー名**サーバー管理者ログイン名を入力します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-618">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="f92f6-619">**パスワード**サーバー管理者ログイン パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-619">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="f92f6-620">新しいデータベース名を入力します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-620">Type a new database name.</span></span>

     <span data-ttu-id="f92f6-621">![ターゲットの接続文字列を構成する](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "ターゲットの接続文字列を構成します。")</span><span class="sxs-lookup"><span data-stu-id="f92f6-621">![Configuring destination connection string](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="f92f6-622">*ターゲットの接続文字列を構成します。*</span><span class="sxs-lookup"><span data-stu-id="f92f6-622">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="f92f6-623">次に、 **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-623">Then click **OK**.</span></span> <span data-ttu-id="f92f6-624">データベースのクリックを作成するように求められたら**はい**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-624">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="f92f6-625">![データベースを作成する](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "データベース文字列を作成します。")</span><span class="sxs-lookup"><span data-stu-id="f92f6-625">![Creating the database](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "Creating the database string")</span></span>

    <span data-ttu-id="f92f6-626">*データベースの作成*</span><span class="sxs-lookup"><span data-stu-id="f92f6-626">*Creating the database*</span></span>
7. <span data-ttu-id="f92f6-627">Azure SQL データベースへの接続に使用する接続文字列は、接続の既定のテキスト ボックス内に表示されます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-627">The connection string you will use to connect to SQL Database in Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="f92f6-628">その後、 **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-628">Then click **Next**.</span></span>

    <span data-ttu-id="f92f6-629">![SQL データベースを指す接続文字列](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "SQL データベースを指す接続文字列")</span><span class="sxs-lookup"><span data-stu-id="f92f6-629">![Connection string pointing to SQL Database](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="f92f6-630">*SQL データベースを指す接続文字列*</span><span class="sxs-lookup"><span data-stu-id="f92f6-630">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="f92f6-631">**プレビュー** ] ページで [**発行**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-631">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="f92f6-632">![Web アプリケーションの発行](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "web アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="f92f6-632">![Publishing the web application](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "Publishing the web application")</span></span>

    <span data-ttu-id="f92f6-633">*Web アプリケーションの発行*</span><span class="sxs-lookup"><span data-stu-id="f92f6-633">*Publishing the web application*</span></span>
9. <span data-ttu-id="f92f6-634">発行プロセスが完了すると、既定のブラウザーが発行した web サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-634">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="f92f6-635">付録 c: コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="f92f6-635">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="f92f6-636">コードのスニペットでは、指先ひとつで必要なすべてのコードがあります。</span><span class="sxs-lookup"><span data-stu-id="f92f6-636">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="f92f6-637">ラボ ドキュメントがわかりますだけをいつ使用できる、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-637">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="f92f6-638">![Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入する](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "プロジェクトにコードを挿入するコード スニペットを Visual Studio の使用")</span><span class="sxs-lookup"><span data-stu-id="f92f6-638">![Using Visual Studio code snippets to insert code into your project](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="f92f6-639">*Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入するには*</span><span class="sxs-lookup"><span data-stu-id="f92f6-639">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="f92f6-640">***キーボード (c# のみ) を使用するコード スニペットを追加するには***</span><span class="sxs-lookup"><span data-stu-id="f92f6-640">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="f92f6-641">コードを挿入するには、カーソルを置きます。</span><span class="sxs-lookup"><span data-stu-id="f92f6-641">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="f92f6-642">スニペットの名前 (なし、スペースやハイフン) の入力を開始します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-642">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="f92f6-643">スニペットの名前に一致する IntelliSense の表示を確認します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-643">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="f92f6-644">適切なスニペットを選択します (または全体のスニペットの名前が選択されるまで」と入力してください)。</span><span class="sxs-lookup"><span data-stu-id="f92f6-644">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="f92f6-645">カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-645">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="f92f6-646">![スニペットの名前の入力を開始](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "スニペット名の入力を開始")</span><span class="sxs-lookup"><span data-stu-id="f92f6-646">![Start typing the snippet name](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "Start typing the snippet name")</span></span>

<span data-ttu-id="f92f6-647">*スニペットの名前の入力を開始します。*</span><span class="sxs-lookup"><span data-stu-id="f92f6-647">*Start typing the snippet name*</span></span>

<span data-ttu-id="f92f6-648">![強調表示されているスニペットを選択して Tab キーを押して](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "キーを押してタブが強調表示されているスニペットを選択するには")</span><span class="sxs-lookup"><span data-stu-id="f92f6-648">![Press Tab to select the highlighted snippet](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="f92f6-649">*Tab キーを押して、強調表示されているスニペットを選択します*</span><span class="sxs-lookup"><span data-stu-id="f92f6-649">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="f92f6-650">![キーを押して タブで再度とスニペットが展開](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "キーを押して タブで再度とスニペットが展開されます")</span><span class="sxs-lookup"><span data-stu-id="f92f6-650">![Press Tab again and the snippet will expand](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="f92f6-651">*キーを押して タブで再度とスニペットが展開されます。*</span><span class="sxs-lookup"><span data-stu-id="f92f6-651">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="f92f6-652">***(C#、Visual Basic および XML) にマウスを使用するコード スニペットを追加する***1。</span><span class="sxs-lookup"><span data-stu-id="f92f6-652">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="f92f6-653">コード スニペットを挿入するを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="f92f6-653">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="f92f6-654">選択**スニペットの挿入**続けて**マイ コード スニペット**します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-654">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="f92f6-655">クリックして、一覧から関連するスニペットを選択します。</span><span class="sxs-lookup"><span data-stu-id="f92f6-655">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="f92f6-656">![コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリックして](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリック")</span><span class="sxs-lookup"><span data-stu-id="f92f6-656">![Right-click where you want to insert the code snippet and select Insert Snippet](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="f92f6-657">*コード スニペットを挿入して、スニペットの挿入先の選択します。*</span><span class="sxs-lookup"><span data-stu-id="f92f6-657">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="f92f6-658">![クリックして、一覧から関連するスニペットを選択](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "クリックして、一覧から関連するスニペットを選択")</span><span class="sxs-lookup"><span data-stu-id="f92f6-658">![Pick the relevant snippet from the list, by clicking on it](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="f92f6-659">*クリックして、一覧から関連するスニペットを選択します。*</span><span class="sxs-lookup"><span data-stu-id="f92f6-659">*Pick the relevant snippet from the list, by clicking on it*</span></span>
