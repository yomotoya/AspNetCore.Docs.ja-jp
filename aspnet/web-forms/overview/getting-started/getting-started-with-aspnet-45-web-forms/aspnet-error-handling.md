---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: "ASP.NET エラー処理 |Microsoft ドキュメント"
author: Erikre
description: "このチュートリアルの系列では、お用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 3f732ae6f1b7845bcae88912b4a4fe26574c10de
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
<a name="aspnet-error-handling"></a><span data-ttu-id="db724-103">ASP.NET エラー処理</span><span class="sxs-lookup"><span data-stu-id="db724-103">ASP.NET Error Handling</span></span>
====================
<span data-ttu-id="db724-104">によって[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="db724-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="db724-105">[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="db724-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="db724-106">このチュートリアルの系列では、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="db724-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="db724-107">Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)はこのチュートリアルのシリーズに付随する使用できます。</span><span class="sxs-lookup"><span data-stu-id="db724-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="db724-108">このチュートリアルでは、エラー処理およびエラーのログ記録を含めて、Wingtip Toys サンプル アプリケーションを変更します。</span><span class="sxs-lookup"><span data-stu-id="db724-108">In this tutorial, you will modify the Wingtip Toys sample application to include error handling and error logging.</span></span> <span data-ttu-id="db724-109">エラー処理により、アプリケーションを正常にエラーを処理し、それに従ってエラー メッセージを表示できます。</span><span class="sxs-lookup"><span data-stu-id="db724-109">Error handling will allow the application to gracefully handle errors and display error messages accordingly.</span></span> <span data-ttu-id="db724-110">エラーのログ記録を使用すると、検出し、発生したエラーを修正できます。</span><span class="sxs-lookup"><span data-stu-id="db724-110">Error logging will allow you to find and fix errors that have occurred.</span></span> <span data-ttu-id="db724-111">このチュートリアルでは、「URL ルーティング」前のチュートリアルを上に構築され、Wingtip Toys チュートリアル シリーズの一部です。</span><span class="sxs-lookup"><span data-stu-id="db724-111">This tutorial builds on the previous tutorial "URL Routing" and is part of the Wingtip Toys tutorial series.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="db724-112">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="db724-112">What you'll learn:</span></span>

- <span data-ttu-id="db724-113">グローバルにエラー処理アプリケーションの構成を追加する方法。</span><span class="sxs-lookup"><span data-stu-id="db724-113">How to add global error handling to the application's configuration.</span></span>
- <span data-ttu-id="db724-114">エラー処理アプリケーション、ページ、およびコードのレベルを追加する方法。</span><span class="sxs-lookup"><span data-stu-id="db724-114">How to add error handling at the application, page, and code levels.</span></span>
- <span data-ttu-id="db724-115">後で確認できるエラーを記録する方法。</span><span class="sxs-lookup"><span data-stu-id="db724-115">How to log errors for later review.</span></span>
- <span data-ttu-id="db724-116">セキュリティが脅かされないエラー メッセージを表示する方法。</span><span class="sxs-lookup"><span data-stu-id="db724-116">How to display error messages that do not compromise security.</span></span>
- <span data-ttu-id="db724-117">エラー ログ モジュールとハンドラー (ELMAH) エラーのログ記録を実装する方法。</span><span class="sxs-lookup"><span data-stu-id="db724-117">How to implement Error Logging Modules and Handlers (ELMAH) error logging.</span></span>

## <a name="overview"></a><span data-ttu-id="db724-118">概要</span><span class="sxs-lookup"><span data-stu-id="db724-118">Overview</span></span>

<span data-ttu-id="db724-119">ASP.NET アプリケーションでは、一貫した方法で実行中に発生したエラーを処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="db724-119">ASP.NET applications must be able to handle errors that occur during execution in a consistent manner.</span></span> <span data-ttu-id="db724-120">ASP.NET では、一貫した方法で、エラーのアプリケーションに通知する方法を提供する共通言語ランタイム (CLR) を使用します。</span><span class="sxs-lookup"><span data-stu-id="db724-120">ASP.NET uses the common language runtime (CLR), which provides a way of notifying applications of errors in a uniform way.</span></span> <span data-ttu-id="db724-121">エラーが発生するときに例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="db724-121">When an error occurs, an exception is thrown.</span></span> <span data-ttu-id="db724-122">例外は、任意のエラー、条件、またはアプリケーションが予期しない動作です。</span><span class="sxs-lookup"><span data-stu-id="db724-122">An exception is any error, condition, or unexpected behavior that an application encounters.</span></span>

<span data-ttu-id="db724-123">.NET Framework では、例外は `System.Exception` クラスから継承されるオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="db724-123">In the .NET Framework, an exception is an object that inherits from the `System.Exception` class.</span></span> <span data-ttu-id="db724-124">例外は問題が発生したコード領域からスローされます。</span><span class="sxs-lookup"><span data-stu-id="db724-124">An exception is thrown from an area of code where a problem has occurred.</span></span> <span data-ttu-id="db724-125">例外は、アプリケーションが例外を処理するコードを提供する場所の場所へ呼び出しスタックに渡されます。</span><span class="sxs-lookup"><span data-stu-id="db724-125">The exception is passed up the call stack to a place where the application provides code to handle the exception.</span></span> <span data-ttu-id="db724-126">アプリケーションで例外が処理されないエラーの詳細を表示する、ブラウザーが強制されます。</span><span class="sxs-lookup"><span data-stu-id="db724-126">If the application does not handle the exception, the browser is forced to display the error details.</span></span>

<span data-ttu-id="db724-127">ベスト プラクティスとして、レベルのコードでエラーを処理`Try` / `Catch` / `Finally`コード内にブロックします。</span><span class="sxs-lookup"><span data-stu-id="db724-127">As a best practice, handle errors in at the code level in `Try`/`Catch`/`Finally` blocks within your code.</span></span> <span data-ttu-id="db724-128">ユーザーが行われるコンテキストの問題を修正できるように、これらのブロックを配置しようとしてください。</span><span class="sxs-lookup"><span data-stu-id="db724-128">Try to place these blocks so that the user can correct problems in the context in which they occur.</span></span> <span data-ttu-id="db724-129">エラー処理のブロックが、エラーが発生したから離れすぎている場合に、問題の解決に必要な情報をユーザーに提供することが困難になります。</span><span class="sxs-lookup"><span data-stu-id="db724-129">If the error handling blocks are too far away from where the error occurred, it becomes more difficult to provide users with the information they need to fix the problem.</span></span>

### <a name="exception-class"></a><span data-ttu-id="db724-130">例外クラス</span><span class="sxs-lookup"><span data-stu-id="db724-130">Exception Class</span></span>

<span data-ttu-id="db724-131">例外クラスは、例外の継承元となる基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="db724-131">The Exception class is the base class from which exceptions inherit.</span></span> <span data-ttu-id="db724-132">ほとんどの例外オブジェクトは、例外クラスの派生クラスのインスタンスなど、`SystemException`クラス、`IndexOutOfRangeException`クラス、または`ArgumentNullException`クラスです。</span><span class="sxs-lookup"><span data-stu-id="db724-132">Most exception objects are instances of some derived class of the Exception class, such as the `SystemException` class, the `IndexOutOfRangeException` class, or the `ArgumentNullException` class.</span></span> <span data-ttu-id="db724-133">例外クラスなどのプロパティがあります、 `StackTrace` 、プロパティ、`InnerException`プロパティ、および`Message`が発生したエラーに関する情報を提供するプロパティです。</span><span class="sxs-lookup"><span data-stu-id="db724-133">The Exception class has properties, such as the `StackTrace` property, the `InnerException` property, and the `Message` property, that provide specific information about the error that has occurred.</span></span>

### <a name="exception-inheritance-hierarchy"></a><span data-ttu-id="db724-134">例外の継承階層</span><span class="sxs-lookup"><span data-stu-id="db724-134">Exception Inheritance Hierarchy</span></span>

<span data-ttu-id="db724-135">ランタイムはから派生した例外の基本セットを`SystemException`クラス、例外が発生したときに、ランタイムをスローします。</span><span class="sxs-lookup"><span data-stu-id="db724-135">The runtime has a base set of exceptions deriving from the `SystemException` class that the runtime throws when an exception is encountered.</span></span> <span data-ttu-id="db724-136">などの例外クラスから継承するクラスのほとんどの`IndexOutOfRangeException`クラスおよび`ArgumentNullException`クラス、追加のメンバーを実装していません。</span><span class="sxs-lookup"><span data-stu-id="db724-136">Most of the classes that inherit from the Exception class, such as the `IndexOutOfRangeException` class and the `ArgumentNullException` class, do not implement additional members.</span></span> <span data-ttu-id="db724-137">そのため、例外の最も重要な情報は含まれて、階層の例外、例外の名前と、例外に含まれる情報です。</span><span class="sxs-lookup"><span data-stu-id="db724-137">Therefore, the most important information for an exception can be found in the hierarchy of exceptions, the exception name, and the information contained in the exception.</span></span>

### <a name="exception-handling-hierarchy"></a><span data-ttu-id="db724-138">例外処理の階層構造</span><span class="sxs-lookup"><span data-stu-id="db724-138">Exception Handling Hierarchy</span></span>

<span data-ttu-id="db724-139">ASP.NET Web フォーム アプリケーションでは、特定の処理階層に基づいて例外を処理することができます。</span><span class="sxs-lookup"><span data-stu-id="db724-139">In an ASP.NET Web Forms application, exceptions can be handled based on a specific handling hierarchy.</span></span> <span data-ttu-id="db724-140">例外は、次のレベルで処理できます。</span><span class="sxs-lookup"><span data-stu-id="db724-140">An exception can be handled at the following levels:</span></span>

- <span data-ttu-id="db724-141">アプリケーション レベル</span><span class="sxs-lookup"><span data-stu-id="db724-141">Application level</span></span>
- <span data-ttu-id="db724-142">ページ レベル</span><span class="sxs-lookup"><span data-stu-id="db724-142">Page level</span></span>
- <span data-ttu-id="db724-143">コードのレベル</span><span class="sxs-lookup"><span data-stu-id="db724-143">Code level</span></span>

<span data-ttu-id="db724-144">アプリケーションでは、例外を処理するときに例外クラスから継承される例外に関する追加情報多くの場合、取得して、ユーザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="db724-144">When an application handles exceptions, additional information about the exception that is inherited from the Exception class can often be retrieved and displayed to the user.</span></span> <span data-ttu-id="db724-145">アプリケーション、ページ、およびコード レベル、だけでなく、HTTP モジュール レベルでと IIS のカスタム ハンドラーを使用して、例外も処理できます。</span><span class="sxs-lookup"><span data-stu-id="db724-145">In addition to application, page, and code level, you can also handle exceptions at the HTTP module level and by using an IIS custom handler.</span></span>

### <a name="application-level-error-handling"></a><span data-ttu-id="db724-146">アプリケーション レベルのエラー処理</span><span class="sxs-lookup"><span data-stu-id="db724-146">Application Level Error Handling</span></span>

<span data-ttu-id="db724-147">アプリケーション レベルで既定のエラーを処理するには、アプリケーションの構成を変更することによって、または追加することによって、`Application_Error`ハンドラー、 *Global.asax*アプリケーションのファイルです。</span><span class="sxs-lookup"><span data-stu-id="db724-147">You can handle default errors at the application level either by modifying your application's configuration or by adding an `Application_Error` handler in the *Global.asax* file of your application.</span></span>

<span data-ttu-id="db724-148">追加することで、既定のエラーと HTTP エラーを処理することができます、`customErrors`セクション、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="db724-148">You can handle default errors and HTTP errors by adding a `customErrors` section to the *Web.config* file.</span></span> <span data-ttu-id="db724-149">`customErrors`セクションでは、エラーが発生したときにそのユーザーにリダイレクトされる既定のページを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="db724-149">The `customErrors` section allows you to specify a default page that users will be redirected to when an error occurs.</span></span> <span data-ttu-id="db724-150">また、個々 のページの特定の状態コード エラーを指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="db724-150">It also allows you to specify individual pages for specific status code errors.</span></span>

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

<span data-ttu-id="db724-151">残念ながら、構成を使用して別のページにユーザーをリダイレクトするときにありませんが発生したエラーの詳細。</span><span class="sxs-lookup"><span data-stu-id="db724-151">Unfortunately, when you use the configuration to redirect the user to a different page, you do not have the details of the error that occurred.</span></span>

<span data-ttu-id="db724-152">コードを追加して、アプリケーションで任意の場所で発生したエラーをトラップするただし、`Application_Error`ハンドラー、 *Global.asax*ファイル。</span><span class="sxs-lookup"><span data-stu-id="db724-152">However, you can trap errors that occur anywhere in your application by adding code to the `Application_Error` handler in the *Global.asax* file.</span></span>

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a><span data-ttu-id="db724-153">ページ レベルのエラー イベントの処理</span><span class="sxs-lookup"><span data-stu-id="db724-153">Page Level Error Event Handling</span></span>

<span data-ttu-id="db724-154">ページ レベルのハンドラーで返される、ユーザーのページに、エラーが発生した場所が、コントロールのインスタンスは変更されないため、存在しなくなるもの ページでします。</span><span class="sxs-lookup"><span data-stu-id="db724-154">A page-level handler returns the user to the page where the error occurred, but because instances of controls are not maintained, there will no longer be anything on the page.</span></span> <span data-ttu-id="db724-155">アプリケーションのユーザーに、エラーの詳細を提供するには、具体的には、ページにエラーの詳細を記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="db724-155">To provide the error details to the user of the application, you must specifically write the error details to the page.</span></span>

<span data-ttu-id="db724-156">通常、有用な情報を表示できるページにユーザーを移動または未処理のエラーをログには、ページ レベルのエラー ハンドラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="db724-156">You would typically use a page-level error handler to log unhandled errors or to take the user to a page that can display helpful information.</span></span>

<span data-ttu-id="db724-157">このコード例は、ASP.NET Web ページで、エラー イベントのハンドラーを示します。</span><span class="sxs-lookup"><span data-stu-id="db724-157">This code example shows a handler for the Error event in an ASP.NET Web page.</span></span> <span data-ttu-id="db724-158">このハンドラーは内で既に処理されていないすべての例外をキャッチ`try` / `catch`ページ内のブロックです。</span><span class="sxs-lookup"><span data-stu-id="db724-158">This handler catches all exceptions that are not already handled within `try`/`catch` blocks in the page.</span></span>

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

<span data-ttu-id="db724-159">エラーを処理した後に呼び出すことによってクリアする必要があります、`ClearError`サーバー オブジェクトのメソッド (`HttpServerUtility`クラス)、それ以外の場合、以前に発生したエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="db724-159">After you handle an error, you must clear it by calling the `ClearError` method of the Server object (`HttpServerUtility` class), otherwise you will see an error that has previously occurred.</span></span>

### <a name="code-level-error-handling"></a><span data-ttu-id="db724-160">コードのレベルのエラー処理</span><span class="sxs-lookup"><span data-stu-id="db724-160">Code Level Error Handling</span></span>

<span data-ttu-id="db724-161">Try-catch ステートメント、try ブロックの後に 1 つから成るまたは以上の catch 句で、さまざまな例外のハンドラーを指定します。</span><span class="sxs-lookup"><span data-stu-id="db724-161">The try-catch statement consists of a try block followed by one or more catch clauses, which specify handlers for different exceptions.</span></span> <span data-ttu-id="db724-162">例外がスローされると、共通言語ランタイム (CLR) は、この例外を処理する catch ステートメントを検索します。</span><span class="sxs-lookup"><span data-stu-id="db724-162">When an exception is thrown, the common language runtime (CLR) looks for the catch statement that handles this exception.</span></span> <span data-ttu-id="db724-163">現在実行中のメソッドに catch ブロックが含まれていない場合、CLR は、現在のメソッドと呼び出し履歴の上に、呼び出されるメソッドを検索します。</span><span class="sxs-lookup"><span data-stu-id="db724-163">If the currently executing method does not contain a catch block, the CLR looks at the method that called the current method, and so on, up the call stack.</span></span> <span data-ttu-id="db724-164">Catch ブロックが見つからない、CLR をユーザーにハンドルされない例外メッセージを表示して、プログラムの実行を停止します。</span><span class="sxs-lookup"><span data-stu-id="db724-164">If no catch block is found, then the CLR displays an unhandled exception message to the user and stops execution of the program.</span></span>

<span data-ttu-id="db724-165">次のコード例を使用する一般的な方法を示しています。 `try` / `catch` / `finally`エラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="db724-165">The following code example shows a common way of using `try`/`catch`/`finally` to handle errors.</span></span>

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

<span data-ttu-id="db724-166">上記のコードでは、try ブロックには、可能性のある例外に対して保護されている必要があるコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="db724-166">In the above code, the try block contains the code that needs to be guarded against a possible exception.</span></span> <span data-ttu-id="db724-167">例外がスローされるか、ブロックが正常に完了するまで、ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="db724-167">The block is executed until either an exception is thrown or the block is completed successfully.</span></span> <span data-ttu-id="db724-168">どちらの場合、`FileNotFoundException`例外または`IOException`例外が発生する、実行が別のページに転送します。</span><span class="sxs-lookup"><span data-stu-id="db724-168">If either a `FileNotFoundException` exception or an `IOException` exception occurs, the execution is transferred to a different page.</span></span> <span data-ttu-id="db724-169">含まれるコード、finally ブロックが実行される、エラーが発生したかどうかどうか。</span><span class="sxs-lookup"><span data-stu-id="db724-169">Then, the code contained in the finally block is executed, whether an error occurred or not.</span></span>

## <a name="adding-error-logging-support"></a><span data-ttu-id="db724-170">エラー ログのサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="db724-170">Adding Error Logging Support</span></span>

<span data-ttu-id="db724-171">Wingtip Toys のサンプル アプリケーションにエラー処理を追加する前に、追加することでエラーのログ記録のサポートを追加するが、`ExceptionUtility`クラスを*ロジック*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="db724-171">Before adding error handling to the Wingtip Toys sample application, you will add error logging support by adding an `ExceptionUtility` class to the *Logic* folder.</span></span> <span data-ttu-id="db724-172">これにより、アプリケーションがエラーを処理するたびに、エラーの詳細をエラー ログ ファイルに追加されます。</span><span class="sxs-lookup"><span data-stu-id="db724-172">By doing this, each time the application handles an error, the error details will be added to the error log file.</span></span>

1. <span data-ttu-id="db724-173">右クリックし、*ロジック*クリックしてフォルダー**追加** - &gt; **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="db724-173">Right-click the *Logic* folder and then select **Add** -&gt; **New Item**.</span></span>   
 <span data-ttu-id="db724-174">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="db724-174">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="db724-175">選択、 **Visual c#**  - &gt; **コード**左側のテンプレートのグループです。</span><span class="sxs-lookup"><span data-stu-id="db724-175">Select the **Visual C#** -&gt; **Code** templates group on the left.</span></span> <span data-ttu-id="db724-176">次に、選択**クラス**中央から一覧表示し、名前を付けます**ExceptionUtility.cs**です。</span><span class="sxs-lookup"><span data-stu-id="db724-176">Then, select **Class**from the middle list and name it **ExceptionUtility.cs**.</span></span>
3. <span data-ttu-id="db724-177">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="db724-177">Choose **Add**.</span></span> <span data-ttu-id="db724-178">新しいクラス ファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="db724-178">The new class file is displayed.</span></span>
4. <span data-ttu-id="db724-179">既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="db724-179">Replace the existing code with the following:</span></span>  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

<span data-ttu-id="db724-180">例外が発生するときに例外を書き込む例外ログ ファイルを呼び出すことによって、`LogException`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="db724-180">When an exception occurs, the exception can be written to an exception log file by calling the `LogException` method.</span></span> <span data-ttu-id="db724-181">このメソッドは、2 つのパラメーター、例外オブジェクトおよび例外の原因に関する詳細を含む文字列を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="db724-181">This method takes two parameters, the exception object and a string containing details about the source of the exception.</span></span> <span data-ttu-id="db724-182">例外のログを書き込む、 *ErrorLog.txt*ファイルで、*アプリ\_データ*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="db724-182">The exception log is written to the *ErrorLog.txt* file in the *App\_Data* folder.</span></span>

### <a name="adding-an-error-page"></a><span data-ttu-id="db724-183">エラー ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="db724-183">Adding an Error Page</span></span>

<span data-ttu-id="db724-184">Wingtip Toys サンプル アプリケーションでは、エラーを表示する 1 つのページが使用されます。</span><span class="sxs-lookup"><span data-stu-id="db724-184">In the Wingtip Toys sample application, one page will be used to display errors.</span></span> <span data-ttu-id="db724-185">エラー ページはサイトのユーザーにセキュリティで保護されたエラー メッセージを表示するよう設計されています。</span><span class="sxs-lookup"><span data-stu-id="db724-185">The error page is designed to show a secure error message to users of the site.</span></span> <span data-ttu-id="db724-186">ただし、ユーザーが、コードが存在するコンピューターにローカルで提供される HTTP 要求を行う開発者の場合は、エラー ページに追加のエラーの詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="db724-186">However, if the user is a developer making an HTTP request that is being served locally on the machine where the code lives, additional error details will be displayed on the error page.</span></span>

1. <span data-ttu-id="db724-187">プロジェクト名を右クリックして (**Wingtip Toys**) で**ソリューション エクスプ ローラー**選択**追加** - &gt; **新しい項目の**.</span><span class="sxs-lookup"><span data-stu-id="db724-187">Right-click the project name (**Wingtip Toys**) in **Solution Explorer** and select **Add** -&gt; **New Item**.</span></span>   
 <span data-ttu-id="db724-188">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="db724-188">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="db724-189">選択、 **Visual c#**  - &gt; **Web**左側のテンプレートのグループです。</span><span class="sxs-lookup"><span data-stu-id="db724-189">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="db724-190">中央のリストから選択**マスター ページを含む Web フォーム**、名前を付けます**ErrorPage.aspx**です。</span><span class="sxs-lookup"><span data-stu-id="db724-190">From the middle list, select **Web Form with Master Page**,and name it **ErrorPage.aspx**.</span></span>
3. <span data-ttu-id="db724-191">**[追加]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="db724-191">Click **Add**.</span></span>
4. <span data-ttu-id="db724-192">選択、 *Site.Master*クリックしてファイルをマスター ページとして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="db724-192">Select the *Site.Master* file as the master page, and then choose **OK**.</span></span>
5. <span data-ttu-id="db724-193">既存のマークアップを次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="db724-193">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. <span data-ttu-id="db724-194">分離コードの既存のコードに置き換えます (*ErrorPage.aspx.cs*) 次のように表示されるようにします。</span><span class="sxs-lookup"><span data-stu-id="db724-194">Replace the existing code of the code-behind (*ErrorPage.aspx.cs*) so that it appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

<span data-ttu-id="db724-195">エラー ページが表示されたら、`Page_Load`イベント ハンドラーが実行されます。</span><span class="sxs-lookup"><span data-stu-id="db724-195">When the error page is displayed, the `Page_Load` event handler is executed.</span></span> <span data-ttu-id="db724-196">`Page_Load`ハンドラーでエラーが処理された最初の場所を特定します。</span><span class="sxs-lookup"><span data-stu-id="db724-196">In the `Page_Load` handler, the location of where the error was first handled is determined.</span></span> <span data-ttu-id="db724-197">次に、最後のエラーが発生したは、呼び出しによって決定されます、`GetLastError`サーバー オブジェクトのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="db724-197">Then, the last error that occurred is determined by call the `GetLastError` method of the Server object.</span></span> <span data-ttu-id="db724-198">例外が存在しない場合は、汎用的な例外が作成されます。</span><span class="sxs-lookup"><span data-stu-id="db724-198">If the exception no longer exists, a generic exception is created.</span></span> <span data-ttu-id="db724-199">次に、HTTP 要求がローカルで行われたすべてのエラーの詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="db724-199">Then, if the HTTP request was made locally, all error details are shown.</span></span> <span data-ttu-id="db724-200">この場合、web アプリケーションを実行して、ローカル コンピューターだけでは、これらのエラーの詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="db724-200">In this case, only the local machine running the web application will see these error details.</span></span> <span data-ttu-id="db724-201">エラー情報が表示された後に、エラーがログ ファイルに追加し、サーバーから、エラーをクリアします。</span><span class="sxs-lookup"><span data-stu-id="db724-201">After the error information has been displayed, the error is added to the log file and the error is cleared from the server.</span></span>

### <a name="displaying-unhandled-error-messages-for-the-application"></a><span data-ttu-id="db724-202">アプリケーションの未処理のエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="db724-202">Displaying Unhandled Error Messages for the Application</span></span>

<span data-ttu-id="db724-203">追加することによって、`customErrors`セクション、 *Web.config*ファイル、迅速にエラーを処理する単純なアプリケーション全体で発生します。</span><span class="sxs-lookup"><span data-stu-id="db724-203">By adding a `customErrors` section to the *Web.config* file, you can quickly handle simple errors that occur throughout the application.</span></span> <span data-ttu-id="db724-204">404 - ファイルが見つかりません。 など、状態コード値に基づいてエラーを処理する方法を指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="db724-204">You can also specify how to handle errors based on their status code value, such as 404 - File not found.</span></span>

#### <a name="update-the-configuration"></a><span data-ttu-id="db724-205">構成を更新します。</span><span class="sxs-lookup"><span data-stu-id="db724-205">Update the Configuration</span></span>

<span data-ttu-id="db724-206">追加することで、構成を更新、`customErrors`セクション、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="db724-206">Update the configuration by adding a `customErrors` section to the *Web.config* file.</span></span>

1. <span data-ttu-id="db724-207">**ソリューション エクスプ ローラー**を検索して開く、 *Web.config* Wingtip Toys のサンプル アプリケーションのルートにあるファイルです。</span><span class="sxs-lookup"><span data-stu-id="db724-207">In **Solution Explorer**, find and open the *Web.config* file at the root of the Wingtip Toys sample application.</span></span>
2. <span data-ttu-id="db724-208">追加、`customErrors`セクション、 *Web.config*ファイルの場所、`<system.web>`次のようにノード。</span><span class="sxs-lookup"><span data-stu-id="db724-208">Add the `customErrors` section to the *Web.config* file within the `<system.web>` node as follows:</span></span>   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. <span data-ttu-id="db724-209">保存、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="db724-209">Save the *Web.config* file.</span></span>

<span data-ttu-id="db724-210">`customErrors`セクションが「オン」に設定されているモードを指定します。</span><span class="sxs-lookup"><span data-stu-id="db724-210">The `customErrors` section specifies the mode, which is set to "On".</span></span> <span data-ttu-id="db724-211">また、指定、`defaultRedirect`エラーが発生したときに移動するページ、アプリケーションに通知します。</span><span class="sxs-lookup"><span data-stu-id="db724-211">It also specifies the `defaultRedirect`, which tells the application which page to navigate to when an error occurs.</span></span> <span data-ttu-id="db724-212">さらに、ページが見つからない場合に、404 エラーを処理する方法を指定する特定のエラー要素を追加しました。</span><span class="sxs-lookup"><span data-stu-id="db724-212">In addition, you have added a specific error element that specifies how to handle a 404 error when a page is not found.</span></span> <span data-ttu-id="db724-213">このチュートリアルの後半では、追加のエラーを処理では、アプリケーション レベルでのエラーの詳細情報をキャプチャを追加します。</span><span class="sxs-lookup"><span data-stu-id="db724-213">Later in this tutorial, you will add additional error handling that will capture the details of an error at the application level.</span></span>

#### <a name="running-the-application"></a><span data-ttu-id="db724-214">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="db724-214">Running the Application</span></span>

<span data-ttu-id="db724-215">今すぐ更新済みのルートを表示するアプリケーションを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="db724-215">You can run the application now to see the updated routes.</span></span>

1. <span data-ttu-id="db724-216">キーを押して**f5 キーを押して**Wingtip Toys サンプル アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="db724-216">Press **F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="db724-217">ブラウザーが開き、表示、 *Default.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="db724-217">The browser opens and shows the *Default.aspx* page.</span></span>
2. <span data-ttu-id="db724-218">ブラウザーに次の URL を入力してください (を使用してください**、**ポート番号)。</span><span class="sxs-lookup"><span data-stu-id="db724-218">Enter the following URL into the browser (be sure to use **your** port number):</span></span>  
    `https://localhost:44300/NoPage.aspx`
3. <span data-ttu-id="db724-219">確認、 *ErrorPage.aspx*ブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="db724-219">Review the *ErrorPage.aspx* displayed in the browser.</span></span> 

    ![ASP.NET エラー処理のページには見つからないエラー](aspnet-error-handling/_static/image1.png)

<span data-ttu-id="db724-221">要求すると、 *NoPage.aspx*  ページで、存在しない、エラー ページに表示されます、単純なエラー メッセージと詳細なエラー情報の詳細は、使用可能な場合です。</span><span class="sxs-lookup"><span data-stu-id="db724-221">When you request the *NoPage.aspx* page, which does not exist, the error page will show the simple error message and the detailed error information if additional details are available.</span></span> <span data-ttu-id="db724-222">ただし場合は、ユーザーは、リモートの場所から、存在しないページを要求した、エラー ページがのみ、エラー メッセージ赤で表示します。</span><span class="sxs-lookup"><span data-stu-id="db724-222">However, if the user requested a non-existent page from a remote location, the error page would only show the error message in red.</span></span>

### <a name="including-an-exception-for-testing-purposes"></a><span data-ttu-id="db724-223">テスト目的での例外を含む</span><span class="sxs-lookup"><span data-stu-id="db724-223">Including an Exception for Testing Purposes</span></span>

<span data-ttu-id="db724-224">発生したときにエラーに、アプリケーションがどのように機能することを確認する、ASP.NET のエラー条件を意図的に作成できます。</span><span class="sxs-lookup"><span data-stu-id="db724-224">To verify how your application will function when an error occurs, you can deliberately create error conditions in ASP.NET.</span></span> <span data-ttu-id="db724-225">Wingtip Toys サンプル アプリケーションでは、動作を表示する既定のページを読み込むときにテスト例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="db724-225">In the Wingtip Toys sample application, you will throw a test exception when the default page loads to see what happens.</span></span>

1. <span data-ttu-id="db724-226">分離コードを開き、 *Default.aspx* Visual Studio でのページです。</span><span class="sxs-lookup"><span data-stu-id="db724-226">Open the code-behind of the *Default.aspx* page in Visual Studio.</span></span>   
 <span data-ttu-id="db724-227">*Default.aspx.cs*分離コード ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="db724-227">The *Default.aspx.cs* code-behind page will be displayed.</span></span>
2. <span data-ttu-id="db724-228">`Page_Load`ハンドラー、ハンドラーが次のように表示されるようにコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="db724-228">In the `Page_Load` handler, add code so that the handler appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

<span data-ttu-id="db724-229">さまざまな種類の例外を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="db724-229">It is possible to create various different types of exceptions.</span></span> <span data-ttu-id="db724-230">上記のコードで作成する、`InvalidOperationException`ときに、 *Default.aspx*ページが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="db724-230">In the above code, you are creating an `InvalidOperationException` when the *Default.aspx* page is loaded.</span></span>

#### <a name="running-the-application"></a><span data-ttu-id="db724-231">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="db724-231">Running the Application</span></span>

<span data-ttu-id="db724-232">アプリケーションが例外を処理する方法を表示するアプリケーションを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="db724-232">You can run the application to see how the application handles the exception.</span></span>

1. <span data-ttu-id="db724-233">キーを押して**ctrl キーを押しながら f5 キーを押して**Wingtip Toys サンプル アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="db724-233">Press **CTRL+F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="db724-234">アプリケーションが、InvalidOperationException をスローします。</span><span class="sxs-lookup"><span data-stu-id="db724-234">The application throws the InvalidOperationException.</span></span> 

    > [!NOTE] 
    > 
    > <span data-ttu-id="db724-235">キーを押す必要があります**ctrl キーを押しながら f5 キーを押して**を Visual Studio で、エラーの発生元を表示するコードを分断することがなく、ページを表示します。</span><span class="sxs-lookup"><span data-stu-id="db724-235">You must press **CTRL+F5** to display the page without breaking into the code to view the source of the error in Visual Studio.</span></span>
2. <span data-ttu-id="db724-236">確認、 *ErrorPage.aspx*ブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="db724-236">Review the *ErrorPage.aspx* displayed in the browser.</span></span> 

    ![ASP.NET エラーの処理 - エラー ページ](aspnet-error-handling/_static/image2.png)

<span data-ttu-id="db724-238">例外がによってトラップされたエラーの詳細に示すように、 `customError` 」の「、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="db724-238">As you can see in the error details, the exception was trapped by the `customError` section in the *Web.config* file.</span></span>

### <a name="adding-application-level-error-handling"></a><span data-ttu-id="db724-239">アプリケーション レベルのエラー処理を追加します。</span><span class="sxs-lookup"><span data-stu-id="db724-239">Adding Application-Level Error Handling</span></span>

<span data-ttu-id="db724-240">使用して例外トラップではなく、 `customErrors` 」の「、 *Web.config*ファイル、例外に関する情報を過不足なくを取得する場所は、アプリケーション レベル エラーをトラップして、エラーの詳細を取得することができます。</span><span class="sxs-lookup"><span data-stu-id="db724-240">Rather than trap the exception using the `customErrors` section in the *Web.config* file, where you gain little information about the exception, you can trap the error at the application level and retrieve error details.</span></span>

1. <span data-ttu-id="db724-241">**ソリューション エクスプ ローラー**を検索して開く、 *Global.asax.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="db724-241">In **Solution Explorer**, find and open the *Global.asax.cs* file.</span></span>
2. <span data-ttu-id="db724-242">追加、**アプリケーション\_エラー**ハンドラー it が次のように表示されるようにします。</span><span class="sxs-lookup"><span data-stu-id="db724-242">Add an **Application\_Error** handler so that it appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

<span data-ttu-id="db724-243">アプリケーションでは、エラーが発生したときに、`Application_Error`ハンドラーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="db724-243">When an error occurs in the application, the `Application_Error` handler is called.</span></span> <span data-ttu-id="db724-244">このハンドラーでは、最後の例外が取得され、確認します。</span><span class="sxs-lookup"><span data-stu-id="db724-244">In this handler, the last exception is retrieved and reviewed.</span></span> <span data-ttu-id="db724-245">例外はハンドルされませんでした。 例外には、内部例外の詳細が含まれている場合 (つまり、`InnerException`が null でない)、アプリケーションは例外の詳細を表示する場所のエラー ページに実行を転送します。</span><span class="sxs-lookup"><span data-stu-id="db724-245">If the exception was unhandled and the exception contains inner-exception details (that is, `InnerException` is not null), the application transfers execution to the error page where the exception details are displayed.</span></span>

#### <a name="running-the-application"></a><span data-ttu-id="db724-246">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="db724-246">Running the Application</span></span>

<span data-ttu-id="db724-247">アプリケーション レベルで例外を処理することによって提供される追加のエラー詳細を表示するアプリケーションを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="db724-247">You can run the application to see the additional error details provided by handling the exception at the application level.</span></span>

1. <span data-ttu-id="db724-248">キーを押して**ctrl キーを押しながら f5 キーを押して**Wingtip Toys サンプル アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="db724-248">Press **CTRL+F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="db724-249">アプリケーションをスロー、`InvalidOperationException`です。</span><span class="sxs-lookup"><span data-stu-id="db724-249">The application throws the `InvalidOperationException` .</span></span>
2. <span data-ttu-id="db724-250">確認、 *ErrorPage.aspx*ブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="db724-250">Review the *ErrorPage.aspx* displayed in the browser.</span></span> 

    ![ASP.NET エラー処理のアプリケーション レベルのエラー](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a><span data-ttu-id="db724-252">ページ レベルのエラー処理を追加します。</span><span class="sxs-lookup"><span data-stu-id="db724-252">Adding Page-Level Error Handling</span></span>

<span data-ttu-id="db724-253">追加できますページ レベルのエラー処理、ページの追加を使用するか、`ErrorPage`属性を`@Page`ディレクティブのページ、または追加することによって、`Page_Error`分離コード ページのイベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="db724-253">You can add page-level error handling to a page either by using adding an `ErrorPage` attribute to the `@Page` directive of the page, or by adding a `Page_Error` event handler to the code-behind of a page.</span></span> <span data-ttu-id="db724-254">このセクションでは追加、`Page_Error`に実行を転送するイベント ハンドラー、 *ErrorPage.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="db724-254">In this section, you will add a `Page_Error` event handler that will transfer execution to the *ErrorPage.aspx* page.</span></span>

1. <span data-ttu-id="db724-255">**ソリューション エクスプ ローラー**を検索して開く、 *Default.aspx.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="db724-255">In **Solution Explorer**, find and open the *Default.aspx.cs* file.</span></span>
2. <span data-ttu-id="db724-256">追加、`Page_Error`ハンドラーとして、分離コードが表示されるように依存します。</span><span class="sxs-lookup"><span data-stu-id="db724-256">Add a `Page_Error` handler so that the code-behind appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

<span data-ttu-id="db724-257">ページで、エラーが発生したときに、`Page_Error`イベント ハンドラーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="db724-257">When an error occurs on the page, the `Page_Error` event handler is called.</span></span> <span data-ttu-id="db724-258">このハンドラーでは、最後の例外が取得され、確認します。</span><span class="sxs-lookup"><span data-stu-id="db724-258">In this handler, the last exception is retrieved and reviewed.</span></span> <span data-ttu-id="db724-259">場合、`InvalidOperationException`が発生した、`Page_Error`イベント ハンドラーは例外の詳細を表示する場所のエラー ページに実行を転送します。</span><span class="sxs-lookup"><span data-stu-id="db724-259">If an `InvalidOperationException` occurs, the `Page_Error` event handler transfers execution to the error page where the exception details are displayed.</span></span>

#### <a name="running-the-application"></a><span data-ttu-id="db724-260">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="db724-260">Running the Application</span></span>

<span data-ttu-id="db724-261">今すぐ更新済みのルートを表示するアプリケーションを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="db724-261">You can run the application now to see the updated routes.</span></span>

1. <span data-ttu-id="db724-262">キーを押して**ctrl キーを押しながら f5 キーを押して**Wingtip Toys サンプル アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="db724-262">Press **CTRL+F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="db724-263">アプリケーションをスロー、`InvalidOperationException`です。</span><span class="sxs-lookup"><span data-stu-id="db724-263">The application throws the `InvalidOperationException` .</span></span>
2. <span data-ttu-id="db724-264">確認、 *ErrorPage.aspx*ブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="db724-264">Review the *ErrorPage.aspx* displayed in the browser.</span></span> 

    ![ASP.NET エラー処理でページ レベルのエラー](aspnet-error-handling/_static/image4.png)
3. <span data-ttu-id="db724-266">ブラウザー ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="db724-266">Close your browser window.</span></span>

### <a name="removing-the-exception-used-for-testing"></a><span data-ttu-id="db724-267">テストに使用される例外を削除します。</span><span class="sxs-lookup"><span data-stu-id="db724-267">Removing the Exception Used for Testing</span></span>

<span data-ttu-id="db724-268">このチュートリアルで先ほど追加した例外をスローせずに機能する Wingtip Toys サンプル アプリケーションを許可するのには、例外を削除します。</span><span class="sxs-lookup"><span data-stu-id="db724-268">To allow the Wingtip Toys sample application to function without throwing the exception you added earlier in this tutorial, remove the exception.</span></span>

1. <span data-ttu-id="db724-269">分離コードを開き、 *Default.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="db724-269">Open the code-behind of the *Default.aspx* page.</span></span>
2. <span data-ttu-id="db724-270">`Page_Load`ハンドラー、ハンドラーが次のように表示されるように、例外をスローするコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="db724-270">In the `Page_Load` handler, remove the code that throws the exception so that the handler appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a><span data-ttu-id="db724-271">コード レベルのエラー ログ機能の追加</span><span class="sxs-lookup"><span data-stu-id="db724-271">Adding Code-Level Error Logging</span></span>

<span data-ttu-id="db724-272">このチュートリアルで既に説明したように、コードのセクションを実行し、最初に発生するエラーを処理しようとする try ブロックと catch ステートメントに追加できます。</span><span class="sxs-lookup"><span data-stu-id="db724-272">As mentioned earlier in this tutorial, you can add try/catch statements to attempt to run a section of code and handle the first error that occurs.</span></span> <span data-ttu-id="db724-273">この例ではのみ記述するエラーの詳細エラー ログ ファイルにエラーは後で確認できるようにします。</span><span class="sxs-lookup"><span data-stu-id="db724-273">In this example, you will only write the error details to the error log file so that the error can be reviewed later.</span></span>

1. <span data-ttu-id="db724-274">**ソリューション エクスプ ローラー**で、*ロジック*フォルダー、検索して開く、 *PayPalFunctions.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="db724-274">In **Solution Explorer**, in the *Logic* folder, find and open the *PayPalFunctions.cs* file.</span></span>
2. <span data-ttu-id="db724-275">更新プログラム、`HttpCall`メソッドとして、コードが表示されるように依存します。</span><span class="sxs-lookup"><span data-stu-id="db724-275">Update the `HttpCall` method so that the code appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

<span data-ttu-id="db724-276">上記のコードの呼び出し、`LogException`メソッドに含まれている、`ExceptionUtility`クラスです。</span><span class="sxs-lookup"><span data-stu-id="db724-276">The above code calls the `LogException` method that is contained in the `ExceptionUtility` class.</span></span> <span data-ttu-id="db724-277">追加する、 *ExceptionUtility.cs*クラス ファイルを*ロジック*このチュートリアルで先ほどフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="db724-277">You added the *ExceptionUtility.cs* class file to the *Logic* folder earlier in this tutorial.</span></span> <span data-ttu-id="db724-278">`LogException` は、次の 2 つのパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="db724-278">The `LogException` method takes two parameters.</span></span> <span data-ttu-id="db724-279">最初のパラメーターは、例外オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="db724-279">The first parameter is the exception object.</span></span> <span data-ttu-id="db724-280">2 番目のパラメーターは、エラーの原因を認識するように使用される文字列です。</span><span class="sxs-lookup"><span data-stu-id="db724-280">The second parameter is a string used to recognize the source of the error.</span></span>

### <a name="inspecting-the-error-logging-information"></a><span data-ttu-id="db724-281">エラー ログの情報を検査</span><span class="sxs-lookup"><span data-stu-id="db724-281">Inspecting the Error Logging Information</span></span>

<span data-ttu-id="db724-282">前述のように、まず、アプリケーションでエラーを修正してくださいを決定するのに、エラー ログを使用できます。</span><span class="sxs-lookup"><span data-stu-id="db724-282">As mentioned previously, you can use the error log to determine which errors in your application should be fixed first.</span></span> <span data-ttu-id="db724-283">もちろん、トラップされ、エラー ログに記録されているエラーのみが記録されます。</span><span class="sxs-lookup"><span data-stu-id="db724-283">Of course, only errors that have been trapped and written to the error log will be recorded.</span></span>

1. <span data-ttu-id="db724-284">**ソリューション エクスプ ローラー**を検索して開く、 *ErrorLog.txt*ファイルで、*アプリ\_データ*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="db724-284">In **Solution Explorer**, find and open the *ErrorLog.txt* file in the *App\_Data* folder.</span></span>   
 <span data-ttu-id="db724-285">選択する必要があります、"**すべてのファイル**"オプションまたは"**更新**"オプションの最上位から**ソリューション エクスプ ローラー**を表示する、 *ErrorLog.txt*ファイル。</span><span class="sxs-lookup"><span data-stu-id="db724-285">You may need to select the "**Show All Files**" option or the "**Refresh**" option from the top of **Solution Explorer** to see the *ErrorLog.txt* file.</span></span>
2. <span data-ttu-id="db724-286">Visual Studio に表示されるエラー ログを確認します。</span><span class="sxs-lookup"><span data-stu-id="db724-286">Review the error log displayed in Visual Studio:</span></span> 

    ![ASP.NET エラーの処理 - ErrorLog.txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a><span data-ttu-id="db724-288">安全なエラー メッセージ</span><span class="sxs-lookup"><span data-stu-id="db724-288">Safe Error Messages</span></span>

<span data-ttu-id="db724-289">**に注意して**こと、アプリケーションには、エラー メッセージが表示されたら、その必要がありますいない寄付悪意のあるユーザーは、アプリケーションを攻撃するに役に立つ情報。</span><span class="sxs-lookup"><span data-stu-id="db724-289">It is **important to note** that when your application displays error messages, it should not give away information that a malicious user might find helpful in attacking your application.</span></span> <span data-ttu-id="db724-290">たとえば、アプリケーションが、データベースへ書き込みに失敗したしようとすると、使用されているユーザー名を含むエラー メッセージいない表示されます。</span><span class="sxs-lookup"><span data-stu-id="db724-290">For example, if your application unsuccessfully tries to write in to a database, it should not display an error message that includes the user name it is using.</span></span> <span data-ttu-id="db724-291">このため、赤で一般的なエラー メッセージがユーザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="db724-291">For this reason, a generic error message in red is displayed to the user.</span></span> <span data-ttu-id="db724-292">すべての追加のエラーの詳細は、ローカル コンピューター上の開発者にのみ表示されます。</span><span class="sxs-lookup"><span data-stu-id="db724-292">All additional error details are only displayed to the developer on the local machine.</span></span>

## <a name="using-elmah"></a><span data-ttu-id="db724-293">ELMAH を使用します。</span><span class="sxs-lookup"><span data-stu-id="db724-293">Using ELMAH</span></span>

<span data-ttu-id="db724-294">ELMAH (エラー Logging Modules and Handlers) は、NuGet パッケージとして、ASP.NET アプリケーションに接続エラーのログ記録機能です。</span><span class="sxs-lookup"><span data-stu-id="db724-294">ELMAH (Error Logging Modules and Handlers) is an error logging facility that you plug into your ASP.NET application as a NuGet package.</span></span> <span data-ttu-id="db724-295">ELMAH では、次の機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="db724-295">ELMAH provides the following capabilities:</span></span>

- <span data-ttu-id="db724-296">未処理の例外のログを記録します。</span><span class="sxs-lookup"><span data-stu-id="db724-296">Logging of unhandled exceptions.</span></span>
- <span data-ttu-id="db724-297">記録された未処理の例外の全体のログを表示する web ページです。</span><span class="sxs-lookup"><span data-stu-id="db724-297">A web page to view the entire log of recoded unhandled exceptions.</span></span>
- <span data-ttu-id="db724-298">それぞれの完全な詳細情報を表示する web ページには、例外が記録されます。</span><span class="sxs-lookup"><span data-stu-id="db724-298">A web page to view the full details of each logged exception.</span></span>
- <span data-ttu-id="db724-299">電子メールは、各エラーの発生時に通知します。</span><span class="sxs-lookup"><span data-stu-id="db724-299">An email notification of each error at the time it occurs.</span></span>
- <span data-ttu-id="db724-300">最後の 15 のエラー ログからの RSS フィード。</span><span class="sxs-lookup"><span data-stu-id="db724-300">An RSS feed of the last 15 errors from the log.</span></span>

<span data-ttu-id="db724-301">ELMAH を使用することができます、前にインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="db724-301">Before you can work with the ELMAH, you must install it.</span></span> <span data-ttu-id="db724-302">これは簡単なを使用して、 *NuGet*インストーラーをパッケージ化します。</span><span class="sxs-lookup"><span data-stu-id="db724-302">This is easy using the *NuGet* package installer.</span></span> <span data-ttu-id="db724-303">既に述べたようこのチュートリアルのシリーズ、NuGet は Visual Studio の拡張のオープン ソース ライブラリおよび Visual Studio のツールをインストールおよび更新を簡単にします。</span><span class="sxs-lookup"><span data-stu-id="db724-303">As mentioned earlier in this tutorial series, NuGet is a Visual Studio extension that makes it easy to install and update open source libraries and tools in Visual Studio.</span></span>

1. <span data-ttu-id="db724-304">Visual Studio 内から、**ツール**メニューの **ライブラリ パッケージ マネージャー**  - &gt; **Manage NuGet Packages for Solution**です。</span><span class="sxs-lookup"><span data-stu-id="db724-304">Within Visual Studio, from the **Tools** menu, select **Library Package Manager** -&gt; **Manage NuGet Packages for Solution**.</span></span> 

    ![ASP.NET エラーの処理 - ソリューションの NuGet パッケージの管理](aspnet-error-handling/_static/image6.png)
2. <span data-ttu-id="db724-306">**NuGet パッケージの管理** ダイアログ ボックスが Visual Studio 内で表示されます。</span><span class="sxs-lookup"><span data-stu-id="db724-306">The **Manage NuGet Packages** dialog box is displayed within Visual Studio.</span></span>
3. <span data-ttu-id="db724-307">**NuGet パッケージの管理**] ダイアログ ボックスで、展開**オンライン**、左側にし、[ **nuget.org**です。次に、検索してインストール、 **ELMAH**使用可能なパッケージをオンラインの一覧からパッケージです。</span><span class="sxs-lookup"><span data-stu-id="db724-307">In the **Manage NuGet Packages** dialog box, expand **Online** on the left, and then select **nuget.org**. Then, find and install the **ELMAH** package from the list of available packages online.</span></span> 

    ![ASP.NET エラーの処理 - ELMA NuGet パッケージ](aspnet-error-handling/_static/image7.png)
4. <span data-ttu-id="db724-309">パッケージのダウンロードをインターネットに接続する必要があります。</span><span class="sxs-lookup"><span data-stu-id="db724-309">You will need to have an internet connection to download the package.</span></span>
5. <span data-ttu-id="db724-310">**プロジェクトの選択** ダイアログ ボックスで確認、 **WingtipToys**選択範囲を選択して、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="db724-310">In the **Select Projects** dialog box, make sure the **WingtipToys** selection is selected, and then click **OK**.</span></span> 

    ![ASP.NET エラーの処理 - プロジェクト ダイアログ ボックスを選択](aspnet-error-handling/_static/image8.png)
6. <span data-ttu-id="db724-312">をクリックして**閉じる**で**NuGet パッケージの管理**ダイアログ ボックスで必要な場合です。</span><span class="sxs-lookup"><span data-stu-id="db724-312">Click **Close** in **the Manage NuGet Packages** dialog box if needed.</span></span>
7. <span data-ttu-id="db724-313">Visual Studio では、開いているファイルを再読み込みすることを要求する場合は、選択"**すべてはい**"です。</span><span class="sxs-lookup"><span data-stu-id="db724-313">If Visual Studio requests that you reload any open files, select "**Yes to All**".</span></span>
8. <span data-ttu-id="db724-314">ELMAH パッケージ自体でのエントリを追加する、 *Web.config*プロジェクトのルートにあるファイルです。</span><span class="sxs-lookup"><span data-stu-id="db724-314">The ELMAH package adds entries for itself in the *Web.config* file at the root of your project.</span></span> <span data-ttu-id="db724-315">かどうかは Visual Studio が表示されたら、変更の再読み込みする*Web.config*ファイルをクリックして**はい**です。</span><span class="sxs-lookup"><span data-stu-id="db724-315">If Visual Studio asks you if you want to reload the modified *Web.config* file, click **Yes**.</span></span>

<span data-ttu-id="db724-316">ELMAH は、発生する未処理のエラーを格納する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="db724-316">ELMAH is now ready to store any unhandled errors that occur.</span></span>

### <a name="viewing-the-elmah-log"></a><span data-ttu-id="db724-317">ELMAH ログを表示します。</span><span class="sxs-lookup"><span data-stu-id="db724-317">Viewing the ELMAH Log</span></span>

<span data-ttu-id="db724-318">ELMAH ログの表示は簡単ですが最初に ELMAH ログに記録する未処理の例外を作成します。</span><span class="sxs-lookup"><span data-stu-id="db724-318">Viewing the ELMAH log is easy, but first you will create an unhandled exception that will be recorded in the ELMAH log.</span></span>

1. <span data-ttu-id="db724-319">キーを押して**ctrl キーを押しながら f5 キーを押して**Wingtip Toys サンプル アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="db724-319">Press **CTRL+F5** to run the Wingtip Toys sample application.</span></span>
2. <span data-ttu-id="db724-320">未処理の例外をログに書き込む、ELMAH、(使用するポート番号を使用して) 次の URL をブラウザーに移動します。</span><span class="sxs-lookup"><span data-stu-id="db724-320">To write an unhandled exception to the ELMAH log, navigate in your browser to the following URL (using your port number):</span></span>  
    <span data-ttu-id="db724-321">`https://localhost:44300/NoPage.aspx`エラー ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="db724-321">`https://localhost:44300/NoPage.aspx` The error page will be displayed.</span></span>
3. <span data-ttu-id="db724-322">ELMAH のログを表示するには、(使用するポート番号を使用して) 次の URL をブラウザーに移動します。</span><span class="sxs-lookup"><span data-stu-id="db724-322">To display the ELMAH log, navigate in your browser to the following URL (using your port number):</span></span>  
    `https://localhost:44300/elmah.axd`

    ![ASP.NET エラーの処理 - ELMAH エラー ログ](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a><span data-ttu-id="db724-324">まとめ</span><span class="sxs-lookup"><span data-stu-id="db724-324">Summary</span></span>

<span data-ttu-id="db724-325">このチュートリアルでは、アプリケーション レベル、ページ レベル、およびコード レベルでのエラーの処理について学習しました。</span><span class="sxs-lookup"><span data-stu-id="db724-325">In this tutorial, you have learned about handling errors at the application level, the page level, and the code level.</span></span> <span data-ttu-id="db724-326">後で確認の処理および未処理のエラー ログに記録する方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="db724-326">You have also learned how to log handled and unhandled errors for later review.</span></span> <span data-ttu-id="db724-327">例外ログに記録して NuGet を使用して、アプリケーションに通知を提供する ELMAH ユーティリティが追加されます。</span><span class="sxs-lookup"><span data-stu-id="db724-327">You added the ELMAH utility to provide exception logging and notification to your application using NuGet.</span></span> <span data-ttu-id="db724-328">さらに、安全なエラー メッセージの重要性について学習しました。</span><span class="sxs-lookup"><span data-stu-id="db724-328">Additionally, you have learned about the importance of safe error messages.</span></span>

## <a name="tutorial-series-conclusion"></a><span data-ttu-id="db724-329">最後に一連のチュートリアル</span><span class="sxs-lookup"><span data-stu-id="db724-329">Tutorial Series Conclusion</span></span>

<span data-ttu-id="db724-330">*次に沿ったいただきありがとうございます。このチュートリアルのセットを使用して、ASP.NET Web フォームに慣れることを望みます。詳細については、Web フォームの機能 ASP.NET 4.5 と Visual Studio 2013 で使用可能な場合を参照してください* [ *ASP.NET および Web Tools for Visual Studio 2013 のリリース ノート*](../../../../visual-studio/overview/2013/release-notes.md)  *.また、必ずで説明したチュートリアルを見て、* \***次の手順 \* \* \* セクションと defintely お試し、* [*無料試用版の Azure*](https://azure.microsoft.com/pricing/free-trial/)*.\*</span><span class="sxs-lookup"><span data-stu-id="db724-330">*Thanks for following along. I hope this set of tutorials helped you become more familiar with ASP.NET Web Forms. If you need more information about Web Forms features available in ASP.NET 4.5 and Visual Studio 2013, see* [*ASP.NET and Web Tools for Visual Studio 2013 Release Notes*](../../../../visual-studio/overview/2013/release-notes.md)*. Also, be sure to take a look at the tutorial mentioned in the* ***Next Steps****section and defintely try out the* [*free Azure trial*](https://azure.microsoft.com/pricing/free-trial/)*.*</span></span>

![ありがとうございます - Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a><span data-ttu-id="db724-332">次の手順</span><span class="sxs-lookup"><span data-stu-id="db724-332">Next Steps</span></span>

<span data-ttu-id="db724-333">Microsoft Azure に web アプリケーションの配置の詳細については、「 [Azure Web サイトをセキュリティで保護された ASP.NET Web フォームを使用したアプリのメンバーシップ OAuth、および SQL データベースを展開](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)です。</span><span class="sxs-lookup"><span data-stu-id="db724-333">Learn more about deploying your web application to Microsoft Azure, see [Deploy a Secure ASP.NET Web Forms App with Membership, OAuth, and SQL Database to an Azure Web Site](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).</span></span>

## <a name="free-trial"></a><span data-ttu-id="db724-334">無料試用版</span><span class="sxs-lookup"><span data-stu-id="db724-334">Free Trial</span></span>

[<span data-ttu-id="db724-335">Microsoft Azure の無料試用版</span><span class="sxs-lookup"><span data-stu-id="db724-335">Microsoft Azure - Free Trial</span></span>](https://azure.microsoft.com/pricing/free-trial/)  
 <span data-ttu-id="db724-336">Microsoft Azure への web サイトの発行は保存すると、時間、メンテナンスとコストします。</span><span class="sxs-lookup"><span data-stu-id="db724-336">Publishing your website to Microsoft Azure will save you time, maintenance and expense.</span></span> <span data-ttu-id="db724-337">これは、Azure に web アプリを配置するクイック プロセスです。</span><span class="sxs-lookup"><span data-stu-id="db724-337">It's a quick process to deploying your web app to Azure.</span></span> <span data-ttu-id="db724-338">維持し、web アプリを監視する必要がある場合、Azure はさまざまなツールやサービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="db724-338">When you need to maintain and monitor your web app, Azure offers a variety of tools and services.</span></span> <span data-ttu-id="db724-339">データ、トラフィック、identity、メッセージング、Azure でのパフォーマンスとメディアのバックアップを管理します。</span><span class="sxs-lookup"><span data-stu-id="db724-339">Manage data, traffic, identity, backups, messaging, media and performance in Azure.</span></span> <span data-ttu-id="db724-340">このすべては非常にコスト効果の高い方法で提供します。</span><span class="sxs-lookup"><span data-stu-id="db724-340">And, all of this is provided in a very cost effective approach.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="db724-341">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="db724-341">Additional Resources</span></span>

<span data-ttu-id="db724-342">[ASP.NET の状態監視によるエラーの詳細をログ記録](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md) </span><span class="sxs-lookup"><span data-stu-id="db724-342">[Logging Error Details with ASP.NET Health Monitoring](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md) </span></span>  
[<span data-ttu-id="db724-343">ELMAH</span><span class="sxs-lookup"><span data-stu-id="db724-343">ELMAH</span></span>](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a><span data-ttu-id="db724-344">謝辞</span><span class="sxs-lookup"><span data-stu-id="db724-344">Acknowledgements</span></span>

<span data-ttu-id="db724-345">このチュートリアル シリーズのコンテンツに多大な協力者次の方々 に感謝したいと思います。</span><span class="sxs-lookup"><span data-stu-id="db724-345">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="db724-346">Alberto Poblacion、MVP &amp; MCT、スペイン</span><span class="sxs-lookup"><span data-stu-id="db724-346">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- <span data-ttu-id="db724-347">[Alex Thissen、オランダ](http://blog.alexthissen.nl/)(twitter: [ @alexthissen ](http://twitter.com/alexthissen))</span><span class="sxs-lookup"><span data-stu-id="db724-347">[Alex Thissen, Netherlands](http://blog.alexthissen.nl/) (twitter: [@alexthissen](http://twitter.com/alexthissen))</span></span>
- [<span data-ttu-id="db724-348">Andre Tournier (米国)</span><span class="sxs-lookup"><span data-stu-id="db724-348">Andre Tournier, USA</span></span>](http://andret503.wordpress.com/)
- <span data-ttu-id="db724-349">Apurva Joshi、Microsoft</span><span class="sxs-lookup"><span data-stu-id="db724-349">Apurva Joshi, Microsoft</span></span>
- [<span data-ttu-id="db724-350">Bojan Vrhovnik、スロベニア</span><span class="sxs-lookup"><span data-stu-id="db724-350">Bojan Vrhovnik, Slovenia</span></span>](http://twitter.com/bvrhovnik)
- <span data-ttu-id="db724-351">[Bruno Sonnino、ブラジル](http://msmvps.com/blogs/bsonnino)(twitter: [ @bsonnino ](http://twitter.com/bsonnino))</span><span class="sxs-lookup"><span data-stu-id="db724-351">[Bruno Sonnino, Brazil](http://msmvps.com/blogs/bsonnino) (twitter: [@bsonnino](http://twitter.com/bsonnino))</span></span>
- [<span data-ttu-id="db724-352">Carlos dos Santos、ブラジル</span><span class="sxs-lookup"><span data-stu-id="db724-352">Carlos dos Santos, Brazil</span></span>](http://www.carloscds.net/)
- <span data-ttu-id="db724-353">[Dave Campbell、USA](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))</span><span class="sxs-lookup"><span data-stu-id="db724-353">[Dave Campbell, USA](http://www.wynapse.com/) (twitter: [@windowsdevnews](http://twitter.com/windowsdevnews))</span></span>
- <span data-ttu-id="db724-354">[Jon Galloway、Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))</span><span class="sxs-lookup"><span data-stu-id="db724-354">[Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [@jongalloway](http://twitter.com/jongalloway))</span></span>
- <span data-ttu-id="db724-355">[Michael シャープ、USA](http://www.930solutions.com/) (twitter: [ @mrsharps ](http://twitter.com/mrsharps))</span><span class="sxs-lookup"><span data-stu-id="db724-355">[Michael Sharps, USA](http://www.930solutions.com/) (twitter: [@mrsharps](http://twitter.com/mrsharps))</span></span>
- <span data-ttu-id="db724-356">Mike 教皇</span><span class="sxs-lookup"><span data-stu-id="db724-356">Mike Pope</span></span>
- <span data-ttu-id="db724-357">[Mitchel Sellers、USA](http://www.mitchelsellers.com/) (twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))</span><span class="sxs-lookup"><span data-stu-id="db724-357">[Mitchel Sellers, USA](http://www.mitchelsellers.com/) (twitter: [@MitchelSellers](http://twitter.com/MitchelSellers))</span></span>
- [<span data-ttu-id="db724-358">Paul Cociuba、Microsoft</span><span class="sxs-lookup"><span data-stu-id="db724-358">Paul Cociuba, Microsoft</span></span>](http://linqto.me/Links/pcociuba)
- [<span data-ttu-id="db724-359">サンパウロ Morgado、ポルトガル</span><span class="sxs-lookup"><span data-stu-id="db724-359">Paulo Morgado, Portugal</span></span>](http://paulomorgado.net/)
- [<span data-ttu-id="db724-360">Pranav Rastogi、Microsoft</span><span class="sxs-lookup"><span data-stu-id="db724-360">Pranav Rastogi, Microsoft</span></span>](https://blogs.msdn.com/b/pranav_rastogi)
- [<span data-ttu-id="db724-361">Tim Ammann、Microsoft</span><span class="sxs-lookup"><span data-stu-id="db724-361">Tim Ammann, Microsoft</span></span>](https://blogs.iis.net/timamm/default.aspx)
- [<span data-ttu-id="db724-362">Tom Dykstra、Microsoft</span><span class="sxs-lookup"><span data-stu-id="db724-362">Tom Dykstra, Microsoft</span></span>](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a><span data-ttu-id="db724-363">コミュニティからの投稿</span><span class="sxs-lookup"><span data-stu-id="db724-363">Community Contributions</span></span>

- <span data-ttu-id="db724-364">Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))</span><span class="sxs-lookup"><span data-stu-id="db724-364">Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))</span></span>  
 <span data-ttu-id="db724-365">Visual Studio 2012 に関連する msdn コード サンプル:[ナビゲーション Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)</span><span class="sxs-lookup"><span data-stu-id="db724-365">Visual Studio 2012 related code sample on MSDN: [Navigation Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)</span></span>
- <span data-ttu-id="db724-366">James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))</span><span class="sxs-lookup"><span data-stu-id="db724-366">James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))</span></span>  
 <span data-ttu-id="db724-367">Visual Studio 2012 に関連する msdn コード サンプル: [Visual Basic では ASP.NET 4.5 Web フォーム チュートリアル シリーズ](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)</span><span class="sxs-lookup"><span data-stu-id="db724-367">Visual Studio 2012 related code sample on MSDN: [ASP.NET 4.5 Web Forms Tutorial Series in Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)</span></span>
- <span data-ttu-id="db724-368">Andrielle Azevedo - Microsoft Technical Audience 共同作成者 (twitter: @driazevedo)</span><span class="sxs-lookup"><span data-stu-id="db724-368">Andrielle Azevedo - Microsoft Technical Audience Contributor (twitter: @driazevedo)</span></span>  
 <span data-ttu-id="db724-369">Visual Studio 2012 翻訳: [Iniciando com Visão Geral ASP.NET Web フォーム 4.5 - Parte 1 - Introdução e](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)</span><span class="sxs-lookup"><span data-stu-id="db724-369">Visual Studio 2012 translation: [Iniciando com ASP.NET Web Forms 4.5 - Parte 1 - Introdução e Visão Geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="db724-370">前へ</span><span class="sxs-lookup"><span data-stu-id="db724-370">Previous</span></span>](url-routing.md)
