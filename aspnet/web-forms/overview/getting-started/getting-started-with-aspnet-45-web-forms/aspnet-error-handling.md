---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: ASP.NET エラー処理 |Microsoft Docs
author: Erikre
description: このチュートリアル シリーズでは、私たちの ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 8d6c19be7c77079b870261d1c4cf0ea62e0e2fd6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807591"
---
<a name="aspnet-error-handling"></a><span data-ttu-id="60f47-103">ASP.NET エラー処理</span><span class="sxs-lookup"><span data-stu-id="60f47-103">ASP.NET Error Handling</span></span>
====================
<span data-ttu-id="60f47-104">によって[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="60f47-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="60f47-105">[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) をダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="60f47-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="60f47-106">このチュートリアル シリーズでは、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="60f47-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="60f47-107">Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)このチュートリアル シリーズをと共に使用できます。</span><span class="sxs-lookup"><span data-stu-id="60f47-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="60f47-108">このチュートリアルでは、エラー処理およびエラーのログ記録を含む Wingtip Toys のサンプル アプリケーションを変更します。</span><span class="sxs-lookup"><span data-stu-id="60f47-108">In this tutorial, you will modify the Wingtip Toys sample application to include error handling and error logging.</span></span> <span data-ttu-id="60f47-109">エラー処理により、アプリケーションを適切にエラーを処理し、それに従ってエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="60f47-109">Error handling will allow the application to gracefully handle errors and display error messages accordingly.</span></span> <span data-ttu-id="60f47-110">エラーのログ記録は検出と発生したエラーを修正できます。</span><span class="sxs-lookup"><span data-stu-id="60f47-110">Error logging will allow you to find and fix errors that have occurred.</span></span> <span data-ttu-id="60f47-111">このチュートリアルでは、「URL ルーティング」前のチュートリアルに基づいており、Wingtip Toys のチュートリアル シリーズのパートです。</span><span class="sxs-lookup"><span data-stu-id="60f47-111">This tutorial builds on the previous tutorial "URL Routing" and is part of the Wingtip Toys tutorial series.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="60f47-112">学習内容。</span><span class="sxs-lookup"><span data-stu-id="60f47-112">What you'll learn:</span></span>

- <span data-ttu-id="60f47-113">グローバル エラー処理アプリケーションの構成に追加する方法。</span><span class="sxs-lookup"><span data-stu-id="60f47-113">How to add global error handling to the application's configuration.</span></span>
- <span data-ttu-id="60f47-114">エラー処理アプリケーション、ページ、およびコードのレベルを追加する方法。</span><span class="sxs-lookup"><span data-stu-id="60f47-114">How to add error handling at the application, page, and code levels.</span></span>
- <span data-ttu-id="60f47-115">後で確認エラーを記録する方法。</span><span class="sxs-lookup"><span data-stu-id="60f47-115">How to log errors for later review.</span></span>
- <span data-ttu-id="60f47-116">セキュリティが低下しないエラー メッセージを表示する方法。</span><span class="sxs-lookup"><span data-stu-id="60f47-116">How to display error messages that do not compromise security.</span></span>
- <span data-ttu-id="60f47-117">エラー ログのモジュールとハンドラー (ELMAH) エラーのログ記録を実装する方法。</span><span class="sxs-lookup"><span data-stu-id="60f47-117">How to implement Error Logging Modules and Handlers (ELMAH) error logging.</span></span>

## <a name="overview"></a><span data-ttu-id="60f47-118">概要</span><span class="sxs-lookup"><span data-stu-id="60f47-118">Overview</span></span>

<span data-ttu-id="60f47-119">ASP.NET アプリケーションでは、一貫した方法で実行中に発生するエラーを処理できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="60f47-119">ASP.NET applications must be able to handle errors that occur during execution in a consistent manner.</span></span> <span data-ttu-id="60f47-120">ASP.NET では、統一された方法でアプリケーションのエラーを通知するための手段を提供する共通言語ランタイム (CLR) を使用します。</span><span class="sxs-lookup"><span data-stu-id="60f47-120">ASP.NET uses the common language runtime (CLR), which provides a way of notifying applications of errors in a uniform way.</span></span> <span data-ttu-id="60f47-121">エラーが発生する例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60f47-121">When an error occurs, an exception is thrown.</span></span> <span data-ttu-id="60f47-122">すべてのエラー、条件、またはアプリケーションが発生した予期しない動作は例外です。</span><span class="sxs-lookup"><span data-stu-id="60f47-122">An exception is any error, condition, or unexpected behavior that an application encounters.</span></span>

<span data-ttu-id="60f47-123">.NET Framework では、例外は `System.Exception` クラスから継承されるオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="60f47-123">In the .NET Framework, an exception is an object that inherits from the `System.Exception` class.</span></span> <span data-ttu-id="60f47-124">例外は問題が発生したコード領域からスローされます。</span><span class="sxs-lookup"><span data-stu-id="60f47-124">An exception is thrown from an area of code where a problem has occurred.</span></span> <span data-ttu-id="60f47-125">例外は、アプリケーション例外を処理するコードを提供している場所にコール スタックの上に渡されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-125">The exception is passed up the call stack to a place where the application provides code to handle the exception.</span></span> <span data-ttu-id="60f47-126">アプリケーションが例外を処理していない場合は、ブラウザーを強制的にエラーの詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="60f47-126">If the application does not handle the exception, the browser is forced to display the error details.</span></span>

<span data-ttu-id="60f47-127">ベスト プラクティスとしては、コード レベルでのエラーを処理`Try` / `Catch` / `Finally`コード内でブロックします。</span><span class="sxs-lookup"><span data-stu-id="60f47-127">As a best practice, handle errors in at the code level in `Try`/`Catch`/`Finally` blocks within your code.</span></span> <span data-ttu-id="60f47-128">ユーザーが発生したコンテキストでの問題を修正できるように、これらのブロックを配置しようとしてください。</span><span class="sxs-lookup"><span data-stu-id="60f47-128">Try to place these blocks so that the user can correct problems in the context in which they occur.</span></span> <span data-ttu-id="60f47-129">エラー処理のブロックが、エラーが発生したから離れすぎている場合に、問題の解決に必要な情報をユーザーに提供することが困難になります。</span><span class="sxs-lookup"><span data-stu-id="60f47-129">If the error handling blocks are too far away from where the error occurred, it becomes more difficult to provide users with the information they need to fix the problem.</span></span>

### <a name="exception-class"></a><span data-ttu-id="60f47-130">例外クラス</span><span class="sxs-lookup"><span data-stu-id="60f47-130">Exception Class</span></span>

<span data-ttu-id="60f47-131">例外クラスは、例外の継承元となる基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="60f47-131">The Exception class is the base class from which exceptions inherit.</span></span> <span data-ttu-id="60f47-132">ほとんどの例外オブジェクトは、例外クラスの派生クラスのインスタンスなど、`SystemException`クラス、`IndexOutOfRangeException`クラス、または`ArgumentNullException`クラス。</span><span class="sxs-lookup"><span data-stu-id="60f47-132">Most exception objects are instances of some derived class of the Exception class, such as the `SystemException` class, the `IndexOutOfRangeException` class, or the `ArgumentNullException` class.</span></span> <span data-ttu-id="60f47-133">例外クラスなどのプロパティがあります、`StackTrace`プロパティ、`InnerException`プロパティ、および`Message`プロパティは、発生したエラーに関する特定の情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="60f47-133">The Exception class has properties, such as the `StackTrace` property, the `InnerException` property, and the `Message` property, that provide specific information about the error that has occurred.</span></span>

### <a name="exception-inheritance-hierarchy"></a><span data-ttu-id="60f47-134">例外の継承階層</span><span class="sxs-lookup"><span data-stu-id="60f47-134">Exception Inheritance Hierarchy</span></span>

<span data-ttu-id="60f47-135">ランタイムに起因する例外の基本セットを`SystemException`ランタイムは、例外が発生した場合にスローするクラス。</span><span class="sxs-lookup"><span data-stu-id="60f47-135">The runtime has a base set of exceptions deriving from the `SystemException` class that the runtime throws when an exception is encountered.</span></span> <span data-ttu-id="60f47-136">など、例外クラスから継承するクラスのほとんどの`IndexOutOfRangeException`クラスおよび`ArgumentNullException`クラス、その他のメンバーを実装していません。</span><span class="sxs-lookup"><span data-stu-id="60f47-136">Most of the classes that inherit from the Exception class, such as the `IndexOutOfRangeException` class and the `ArgumentNullException` class, do not implement additional members.</span></span> <span data-ttu-id="60f47-137">そのため、例外の最も重要な情報は、例外、例外の名前と、例外に含まれる情報の階層で見つかんだことができます。</span><span class="sxs-lookup"><span data-stu-id="60f47-137">Therefore, the most important information for an exception can be found in the hierarchy of exceptions, the exception name, and the information contained in the exception.</span></span>

### <a name="exception-handling-hierarchy"></a><span data-ttu-id="60f47-138">例外処理の階層</span><span class="sxs-lookup"><span data-stu-id="60f47-138">Exception Handling Hierarchy</span></span>

<span data-ttu-id="60f47-139">ASP.NET Web フォーム アプリケーションでは、特定の処理の階層に基づく例外を処理することができます。</span><span class="sxs-lookup"><span data-stu-id="60f47-139">In an ASP.NET Web Forms application, exceptions can be handled based on a specific handling hierarchy.</span></span> <span data-ttu-id="60f47-140">次のレベルでは、例外を処理できます。</span><span class="sxs-lookup"><span data-stu-id="60f47-140">An exception can be handled at the following levels:</span></span>

- <span data-ttu-id="60f47-141">アプリケーション レベル</span><span class="sxs-lookup"><span data-stu-id="60f47-141">Application level</span></span>
- <span data-ttu-id="60f47-142">ページ レベル</span><span class="sxs-lookup"><span data-stu-id="60f47-142">Page level</span></span>
- <span data-ttu-id="60f47-143">コード レベル</span><span class="sxs-lookup"><span data-stu-id="60f47-143">Code level</span></span>

<span data-ttu-id="60f47-144">アプリケーションでは、例外を処理するときに例外クラスから継承される例外に関する追加情報多くの場合、取得でき、ユーザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-144">When an application handles exceptions, additional information about the exception that is inherited from the Exception class can often be retrieved and displayed to the user.</span></span> <span data-ttu-id="60f47-145">に加えて、アプリケーション、ページ、およびコード レベルでは、HTTP モジュール レベルでは、IIS のカスタム ハンドラーを使用して例外を処理することもできます。</span><span class="sxs-lookup"><span data-stu-id="60f47-145">In addition to application, page, and code level, you can also handle exceptions at the HTTP module level and by using an IIS custom handler.</span></span>

### <a name="application-level-error-handling"></a><span data-ttu-id="60f47-146">アプリケーション レベルのエラー処理</span><span class="sxs-lookup"><span data-stu-id="60f47-146">Application Level Error Handling</span></span>

<span data-ttu-id="60f47-147">アプリケーション レベルで既定のエラーを処理するには、アプリケーションの構成を変更または追加することで、`Application_Error`ハンドラーで、 *Global.asax*アプリケーションのファイル。</span><span class="sxs-lookup"><span data-stu-id="60f47-147">You can handle default errors at the application level either by modifying your application's configuration or by adding an `Application_Error` handler in the *Global.asax* file of your application.</span></span>

<span data-ttu-id="60f47-148">追加することで、既定のエラーと HTTP エラーを処理することができます、`customErrors`セクションを*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="60f47-148">You can handle default errors and HTTP errors by adding a `customErrors` section to the *Web.config* file.</span></span> <span data-ttu-id="60f47-149">`customErrors`セクションでは、ユーザーは、エラーが発生したときにリダイレクトされる既定のページを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="60f47-149">The `customErrors` section allows you to specify a default page that users will be redirected to when an error occurs.</span></span> <span data-ttu-id="60f47-150">また、個々 の特定のステータス コードのエラー ページを指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="60f47-150">It also allows you to specify individual pages for specific status code errors.</span></span>

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

<span data-ttu-id="60f47-151">残念ながら、別のページにユーザーをリダイレクトする、構成を使用する場合は、発生したエラーの詳細を必要は。</span><span class="sxs-lookup"><span data-stu-id="60f47-151">Unfortunately, when you use the configuration to redirect the user to a different page, you do not have the details of the error that occurred.</span></span>

<span data-ttu-id="60f47-152">コードを追加して、アプリケーションで任意の場所で発生したエラーをトラップするただし、`Application_Error`ハンドラーで、 *Global.asax*ファイル。</span><span class="sxs-lookup"><span data-stu-id="60f47-152">However, you can trap errors that occur anywhere in your application by adding code to the `Application_Error` handler in the *Global.asax* file.</span></span>

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a><span data-ttu-id="60f47-153">ページ レベルのエラー イベントの処理</span><span class="sxs-lookup"><span data-stu-id="60f47-153">Page Level Error Event Handling</span></span>

<span data-ttu-id="60f47-154">ページ レベルのハンドラーでは、エラーが発生したため、コントロールのインスタンスは保持されませんが、ページにユーザーを返しますには不要になったものページ。</span><span class="sxs-lookup"><span data-stu-id="60f47-154">A page-level handler returns the user to the page where the error occurred, but because instances of controls are not maintained, there will no longer be anything on the page.</span></span> <span data-ttu-id="60f47-155">アプリケーションのユーザーに、エラーの詳細を提供するには、具体的にはページにエラーの詳細を記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60f47-155">To provide the error details to the user of the application, you must specifically write the error details to the page.</span></span>

<span data-ttu-id="60f47-156">通常、ハンドルされないエラーをログに、または、ユーザーに役立つ情報を表示できるページに移動するは、ページ レベルのエラー ハンドラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="60f47-156">You would typically use a page-level error handler to log unhandled errors or to take the user to a page that can display helpful information.</span></span>

<span data-ttu-id="60f47-157">このコード例では、ASP.NET Web ページでエラー イベントのハンドラーを示します。</span><span class="sxs-lookup"><span data-stu-id="60f47-157">This code example shows a handler for the Error event in an ASP.NET Web page.</span></span> <span data-ttu-id="60f47-158">このハンドラー内で既に処理されていないすべての例外をキャッチする`try` / `catch`  ページをブロックします。</span><span class="sxs-lookup"><span data-stu-id="60f47-158">This handler catches all exceptions that are not already handled within `try`/`catch` blocks in the page.</span></span>

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

<span data-ttu-id="60f47-159">エラーを処理した後に呼び出してクリアする必要があります、`ClearError`サーバー オブジェクトのメソッド (`HttpServerUtility`クラス)、それ以外の場合、以前発生したエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-159">After you handle an error, you must clear it by calling the `ClearError` method of the Server object (`HttpServerUtility` class), otherwise you will see an error that has previously occurred.</span></span>

### <a name="code-level-error-handling"></a><span data-ttu-id="60f47-160">コードのレベルのエラー処理</span><span class="sxs-lookup"><span data-stu-id="60f47-160">Code Level Error Handling</span></span>

<span data-ttu-id="60f47-161">Try-catch ステートメントから成る 1 が続く try ブロックまたは以上の catch 句で、さまざまな例外ハンドラーを指定します。</span><span class="sxs-lookup"><span data-stu-id="60f47-161">The try-catch statement consists of a try block followed by one or more catch clauses, which specify handlers for different exceptions.</span></span> <span data-ttu-id="60f47-162">例外がスローされたときに、共通言語ランタイム (CLR) は、この例外を処理する catch ステートメントを検索します。</span><span class="sxs-lookup"><span data-stu-id="60f47-162">When an exception is thrown, the common language runtime (CLR) looks for the catch statement that handles this exception.</span></span> <span data-ttu-id="60f47-163">現在実行中のメソッドに catch ブロックが含まれていない場合、CLR は、現在のメソッドというように、コール スタックの上に呼び出されるメソッドで検索します。</span><span class="sxs-lookup"><span data-stu-id="60f47-163">If the currently executing method does not contain a catch block, the CLR looks at the method that called the current method, and so on, up the call stack.</span></span> <span data-ttu-id="60f47-164">Catch ブロックが見つからない場合、CLR は、ユーザーにハンドルされない例外メッセージを表示し、プログラムの実行を停止します。</span><span class="sxs-lookup"><span data-stu-id="60f47-164">If no catch block is found, then the CLR displays an unhandled exception message to the user and stops execution of the program.</span></span>

<span data-ttu-id="60f47-165">次のコード例は、一般的な使用方法を示しています。 `try` / `catch` / `finally`エラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="60f47-165">The following code example shows a common way of using `try`/`catch`/`finally` to handle errors.</span></span>

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

<span data-ttu-id="60f47-166">上記のコードでは、try ブロックには、可能性のある例外から保護する必要があるコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="60f47-166">In the above code, the try block contains the code that needs to be guarded against a possible exception.</span></span> <span data-ttu-id="60f47-167">例外がスローされたか、ブロックが正常に完了するまで、ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-167">The block is executed until either an exception is thrown or the block is completed successfully.</span></span> <span data-ttu-id="60f47-168">どちらの場合、`FileNotFoundException`例外または`IOException`例外が発生した、実行が別のページに転送します。</span><span class="sxs-lookup"><span data-stu-id="60f47-168">If either a `FileNotFoundException` exception or an `IOException` exception occurs, the execution is transferred to a different page.</span></span> <span data-ttu-id="60f47-169">含まれるコードし、ブロックが最後に実行、エラーが発生したかどうかどうか。</span><span class="sxs-lookup"><span data-stu-id="60f47-169">Then, the code contained in the finally block is executed, whether an error occurred or not.</span></span>

## <a name="adding-error-logging-support"></a><span data-ttu-id="60f47-170">エラー ログ記録のサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="60f47-170">Adding Error Logging Support</span></span>

<span data-ttu-id="60f47-171">Wingtip Toys のサンプル アプリケーションにエラー処理を追加する前に追加してエラーのログ記録のサポートを追加するが、`ExceptionUtility`クラスを*ロジック*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="60f47-171">Before adding error handling to the Wingtip Toys sample application, you will add error logging support by adding an `ExceptionUtility` class to the *Logic* folder.</span></span> <span data-ttu-id="60f47-172">これにより、アプリケーションがエラーを処理するたびに、エラー ログ ファイルにエラーの詳細が追加されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-172">By doing this, each time the application handles an error, the error details will be added to the error log file.</span></span>

1. <span data-ttu-id="60f47-173">右クリックし、*ロジック*クリックしてフォルダー**追加** - &gt; **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="60f47-173">Right-click the *Logic* folder and then select **Add** -&gt; **New Item**.</span></span>   
   <span data-ttu-id="60f47-174">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-174">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="60f47-175">選択、 **Visual c#**  - &gt; **コード**左側のテンプレート グループ。</span><span class="sxs-lookup"><span data-stu-id="60f47-175">Select the **Visual C#** -&gt; **Code** templates group on the left.</span></span> <span data-ttu-id="60f47-176">次に、選択**クラス**中央から一覧表示し、名前を**ExceptionUtility.cs**します。</span><span class="sxs-lookup"><span data-stu-id="60f47-176">Then, select **Class**from the middle list and name it **ExceptionUtility.cs**.</span></span>
3. <span data-ttu-id="60f47-177">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="60f47-177">Choose **Add**.</span></span> <span data-ttu-id="60f47-178">新しいクラス ファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-178">The new class file is displayed.</span></span>
4. <span data-ttu-id="60f47-179">既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="60f47-179">Replace the existing code with the following:</span></span>  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

<span data-ttu-id="60f47-180">例外を呼び出すことによって例外のログ ファイルに書き込むことが例外が発生したときに、`LogException`メソッド。</span><span class="sxs-lookup"><span data-stu-id="60f47-180">When an exception occurs, the exception can be written to an exception log file by calling the `LogException` method.</span></span> <span data-ttu-id="60f47-181">このメソッドは、2 つのパラメーター、例外オブジェクトと、例外の原因に関する詳細を含む文字列を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="60f47-181">This method takes two parameters, the exception object and a string containing details about the source of the exception.</span></span> <span data-ttu-id="60f47-182">例外のログを書き込む、 *ErrorLog.txt*ファイル、*アプリ\_データ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="60f47-182">The exception log is written to the *ErrorLog.txt* file in the *App\_Data* folder.</span></span>

### <a name="adding-an-error-page"></a><span data-ttu-id="60f47-183">エラー ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="60f47-183">Adding an Error Page</span></span>

<span data-ttu-id="60f47-184">Wingtip Toys のサンプル アプリケーションでエラーを表示する 1 つのページが使用されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-184">In the Wingtip Toys sample application, one page will be used to display errors.</span></span> <span data-ttu-id="60f47-185">エラー ページは、サイトのユーザーにセキュリティで保護されたエラー メッセージを表示する設計されています。</span><span class="sxs-lookup"><span data-stu-id="60f47-185">The error page is designed to show a secure error message to users of the site.</span></span> <span data-ttu-id="60f47-186">ただし、ユーザーが、コードが存在するコンピューターのローカルで提供される HTTP 要求を行う開発者の場合は、エラー ページに追加のエラーの詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-186">However, if the user is a developer making an HTTP request that is being served locally on the machine where the code lives, additional error details will be displayed on the error page.</span></span>

1. <span data-ttu-id="60f47-187">プロジェクト名を右クリックして (**Wingtip Toys**) で**ソリューション エクスプ ローラー**選択**追加** - &gt; **新しい項目の**.</span><span class="sxs-lookup"><span data-stu-id="60f47-187">Right-click the project name (**Wingtip Toys**) in **Solution Explorer** and select **Add** -&gt; **New Item**.</span></span>   
   <span data-ttu-id="60f47-188">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-188">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="60f47-189">選択、 **Visual c#**  - &gt; **Web**左側のテンプレート グループ。</span><span class="sxs-lookup"><span data-stu-id="60f47-189">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="60f47-190">真ん中の一覧から選択**マスター ページを使用した Web フォーム**、名前を付けます**ErrorPage.aspx**します。</span><span class="sxs-lookup"><span data-stu-id="60f47-190">From the middle list, select **Web Form with Master Page**,and name it **ErrorPage.aspx**.</span></span>
3. <span data-ttu-id="60f47-191">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="60f47-191">Click **Add**.</span></span>
4. <span data-ttu-id="60f47-192">選択、 *Site.Master*ファイルをマスター ページとして選び、 **OK**。</span><span class="sxs-lookup"><span data-stu-id="60f47-192">Select the *Site.Master* file as the master page, and then choose **OK**.</span></span>
5. <span data-ttu-id="60f47-193">次のように既存のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="60f47-193">Replace the existing markup with the following:</span></span>   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. <span data-ttu-id="60f47-194">分離コードの既存のコードに置き換えます (*ErrorPage.aspx.cs*) 次のように表示されるようにします。</span><span class="sxs-lookup"><span data-stu-id="60f47-194">Replace the existing code of the code-behind (*ErrorPage.aspx.cs*) so that it appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

<span data-ttu-id="60f47-195">エラー ページが表示されたら、`Page_Load`イベント ハンドラーが実行されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-195">When the error page is displayed, the `Page_Load` event handler is executed.</span></span> <span data-ttu-id="60f47-196">`Page_Load`ハンドラーでエラーが処理された最初の場所が決定されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-196">In the `Page_Load` handler, the location of where the error was first handled is determined.</span></span> <span data-ttu-id="60f47-197">呼び出しによって発生した最後のエラーを確認し、`GetLastError`サーバー オブジェクトのメソッド。</span><span class="sxs-lookup"><span data-stu-id="60f47-197">Then, the last error that occurred is determined by call the `GetLastError` method of the Server object.</span></span> <span data-ttu-id="60f47-198">例外が存在しない場合は、汎用的な例外が作成されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-198">If the exception no longer exists, a generic exception is created.</span></span> <span data-ttu-id="60f47-199">次に、HTTP 要求がローカルで行われたすべてのエラーの詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-199">Then, if the HTTP request was made locally, all error details are shown.</span></span> <span data-ttu-id="60f47-200">この場合、これらのエラーの詳細、web アプリケーションを実行するローカル コンピューターのみが表示されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-200">In this case, only the local machine running the web application will see these error details.</span></span> <span data-ttu-id="60f47-201">エラー情報を表示した後、エラーがログ ファイルに追加し、サーバーからエラーをクリアします。</span><span class="sxs-lookup"><span data-stu-id="60f47-201">After the error information has been displayed, the error is added to the log file and the error is cleared from the server.</span></span>

### <a name="displaying-unhandled-error-messages-for-the-application"></a><span data-ttu-id="60f47-202">アプリケーションの未処理のエラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="60f47-202">Displaying Unhandled Error Messages for the Application</span></span>

<span data-ttu-id="60f47-203">追加することで、`customErrors`セクションを*Web.config*ファイル、アプリケーション全体で発生する単純なエラー迅速に処理できます。</span><span class="sxs-lookup"><span data-stu-id="60f47-203">By adding a `customErrors` section to the *Web.config* file, you can quickly handle simple errors that occur throughout the application.</span></span> <span data-ttu-id="60f47-204">404 - ファイルが見つかりません。 など、状態コード値に基づいてエラーを処理する方法を指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="60f47-204">You can also specify how to handle errors based on their status code value, such as 404 - File not found.</span></span>

#### <a name="update-the-configuration"></a><span data-ttu-id="60f47-205">構成を更新します</span><span class="sxs-lookup"><span data-stu-id="60f47-205">Update the Configuration</span></span>

<span data-ttu-id="60f47-206">追加することで、構成の更新を`customErrors`セクションを*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="60f47-206">Update the configuration by adding a `customErrors` section to the *Web.config* file.</span></span>

1. <span data-ttu-id="60f47-207">**ソリューション エクスプ ローラー**を検索して開く、 *Web.config* Wingtip Toys のサンプル アプリケーションのルートにあるファイル。</span><span class="sxs-lookup"><span data-stu-id="60f47-207">In **Solution Explorer**, find and open the *Web.config* file at the root of the Wingtip Toys sample application.</span></span>
2. <span data-ttu-id="60f47-208">追加、`customErrors`セクションを*Web.config*ファイル内で、`<system.web>`ノードとして次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="60f47-208">Add the `customErrors` section to the *Web.config* file within the `<system.web>` node as follows:</span></span>   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. <span data-ttu-id="60f47-209">保存、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="60f47-209">Save the *Web.config* file.</span></span>

<span data-ttu-id="60f47-210">`customErrors`セクションが"On"に設定されていると、モードを指定します。</span><span class="sxs-lookup"><span data-stu-id="60f47-210">The `customErrors` section specifies the mode, which is set to "On".</span></span> <span data-ttu-id="60f47-211">指定します、 `defaultRedirect`、アプリケーション エラーが発生したときに移動するには、どのページを指定します。</span><span class="sxs-lookup"><span data-stu-id="60f47-211">It also specifies the `defaultRedirect`, which tells the application which page to navigate to when an error occurs.</span></span> <span data-ttu-id="60f47-212">さらに、ページが見つからない場合に、404 エラーを処理する方法を指定する特定のエラー要素を追加しました。</span><span class="sxs-lookup"><span data-stu-id="60f47-212">In addition, you have added a specific error element that specifies how to handle a 404 error when a page is not found.</span></span> <span data-ttu-id="60f47-213">このチュートリアルの後半では、追加のエラー処理は、アプリケーション レベルのエラーの詳細をキャプチャを追加します。</span><span class="sxs-lookup"><span data-stu-id="60f47-213">Later in this tutorial, you will add additional error handling that will capture the details of an error at the application level.</span></span>

#### <a name="running-the-application"></a><span data-ttu-id="60f47-214">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="60f47-214">Running the Application</span></span>

<span data-ttu-id="60f47-215">更新されたルートを表示するようになりましたアプリケーションを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="60f47-215">You can run the application now to see the updated routes.</span></span>

1. <span data-ttu-id="60f47-216">キーを押して**F5** Wingtip Toys のサンプル アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="60f47-216">Press **F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="60f47-217">ブラウザーが開きを示しています、 *Default.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="60f47-217">The browser opens and shows the *Default.aspx* page.</span></span>
2. <span data-ttu-id="60f47-218">ブラウザーに次の URL を入力してください (を使用してください、 **ポ** ート番号)。</span><span class="sxs-lookup"><span data-stu-id="60f47-218">Enter the following URL into the browser (be sure to use **your** port number):</span></span>  
    `https://localhost:44300/NoPage.aspx`
3. <span data-ttu-id="60f47-219">レビュー、 *ErrorPage.aspx*ブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-219">Review the *ErrorPage.aspx* displayed in the browser.</span></span> 

    ![ASP.NET エラー処理 - ページが見つからないというエラー](aspnet-error-handling/_static/image1.png)

<span data-ttu-id="60f47-221">要求したときに、 *NoPage.aspx*  ページで、存在していない、エラー ページに表示されます、単純なエラー メッセージと詳細なエラー情報の詳細は、使用可能な場合。</span><span class="sxs-lookup"><span data-stu-id="60f47-221">When you request the *NoPage.aspx* page, which does not exist, the error page will show the simple error message and the detailed error information if additional details are available.</span></span> <span data-ttu-id="60f47-222">ただし、ユーザーには、リモートの場所からの存在しないページ機能が要求された場合、エラー ページはのみ、エラー メッセージ赤で表示されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-222">However, if the user requested a non-existent page from a remote location, the error page would only show the error message in red.</span></span>

### <a name="including-an-exception-for-testing-purposes"></a><span data-ttu-id="60f47-223">など、例外の目的でのテスト</span><span class="sxs-lookup"><span data-stu-id="60f47-223">Including an Exception for Testing Purposes</span></span>

<span data-ttu-id="60f47-224">発生したときにエラーに、アプリケーションがどのように機能することを確認する、ASP.NET でエラー条件を意図的に作成できます。</span><span class="sxs-lookup"><span data-stu-id="60f47-224">To verify how your application will function when an error occurs, you can deliberately create error conditions in ASP.NET.</span></span> <span data-ttu-id="60f47-225">Wingtip Toys のサンプル アプリケーションでは、何が起こるかを確認する既定のページの読み込み時にテスト例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60f47-225">In the Wingtip Toys sample application, you will throw a test exception when the default page loads to see what happens.</span></span>

1. <span data-ttu-id="60f47-226">分離コードを開いて、 *Default.aspx* Visual Studio でのページ。</span><span class="sxs-lookup"><span data-stu-id="60f47-226">Open the code-behind of the *Default.aspx* page in Visual Studio.</span></span>   
   <span data-ttu-id="60f47-227">*Default.aspx.cs*分離コード ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-227">The *Default.aspx.cs* code-behind page will be displayed.</span></span>
2. <span data-ttu-id="60f47-228">`Page_Load`ハンドラー、ハンドラーは次のように表示されるようにコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="60f47-228">In the `Page_Load` handler, add code so that the handler appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

<span data-ttu-id="60f47-229">さまざまな種類の例外を作成することになります。</span><span class="sxs-lookup"><span data-stu-id="60f47-229">It is possible to create various different types of exceptions.</span></span> <span data-ttu-id="60f47-230">上記のコードで作成する、`InvalidOperationException`ときに、 *Default.aspx*ページが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="60f47-230">In the above code, you are creating an `InvalidOperationException` when the *Default.aspx* page is loaded.</span></span>

#### <a name="running-the-application"></a><span data-ttu-id="60f47-231">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="60f47-231">Running the Application</span></span>

<span data-ttu-id="60f47-232">アプリケーションが例外を処理する方法を表示するアプリケーションを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="60f47-232">You can run the application to see how the application handles the exception.</span></span>

1. <span data-ttu-id="60f47-233">キーを押して**CTRL + F5** Wingtip Toys のサンプル アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="60f47-233">Press **CTRL+F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="60f47-234">アプリケーションでは、InvalidOperationException がスローされます。</span><span class="sxs-lookup"><span data-stu-id="60f47-234">The application throws the InvalidOperationException.</span></span> 

    > [!NOTE] 
    > 
    > <span data-ttu-id="60f47-235">キーを押す必要があります**CTRL + F5** Visual Studio で、エラーの原因を表示するコードを損なうことがなくページを表示します。</span><span class="sxs-lookup"><span data-stu-id="60f47-235">You must press **CTRL+F5** to display the page without breaking into the code to view the source of the error in Visual Studio.</span></span>
2. <span data-ttu-id="60f47-236">レビュー、 *ErrorPage.aspx*ブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-236">Review the *ErrorPage.aspx* displayed in the browser.</span></span> 

    ![ASP.NET エラーの処理 - エラー ページ](aspnet-error-handling/_static/image2.png)

<span data-ttu-id="60f47-238">によって、例外がトラップされたエラーの詳細をご覧のとおり、`customError`セクション、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="60f47-238">As you can see in the error details, the exception was trapped by the `customError` section in the *Web.config* file.</span></span>

### <a name="adding-application-level-error-handling"></a><span data-ttu-id="60f47-239">アプリケーション レベルのエラー処理を追加します。</span><span class="sxs-lookup"><span data-stu-id="60f47-239">Adding Application-Level Error Handling</span></span>

<span data-ttu-id="60f47-240">使用して例外をトラップではなく、`customErrors`セクション、 *Web.config*ファイルで、ほとんどの例外情報を取得すると、場所は、アプリケーション レベルでエラーをトラップしてエラーの詳細を取得します。</span><span class="sxs-lookup"><span data-stu-id="60f47-240">Rather than trap the exception using the `customErrors` section in the *Web.config* file, where you gain little information about the exception, you can trap the error at the application level and retrieve error details.</span></span>

1. <span data-ttu-id="60f47-241">**ソリューション エクスプ ローラー**を検索して開く、 *Global.asax.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="60f47-241">In **Solution Explorer**, find and open the *Global.asax.cs* file.</span></span>
2. <span data-ttu-id="60f47-242">追加、**アプリケーション\_エラー**ハンドラーことが次のように表示されるようにします。</span><span class="sxs-lookup"><span data-stu-id="60f47-242">Add an **Application\_Error** handler so that it appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

<span data-ttu-id="60f47-243">アプリケーションでは、エラーが発生したときに、`Application_Error`ハンドラーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-243">When an error occurs in the application, the `Application_Error` handler is called.</span></span> <span data-ttu-id="60f47-244">このハンドラーでは、最後の例外が取得され、レビューします。</span><span class="sxs-lookup"><span data-stu-id="60f47-244">In this handler, the last exception is retrieved and reviewed.</span></span> <span data-ttu-id="60f47-245">例外はハンドルされませんでした。 例外には、内部例外の詳細が含まれている場合 (つまり、`InnerException`が null でない)、アプリケーションは、例外の詳細を表示する場所のエラー ページに実行を転送します。</span><span class="sxs-lookup"><span data-stu-id="60f47-245">If the exception was unhandled and the exception contains inner-exception details (that is, `InnerException` is not null), the application transfers execution to the error page where the exception details are displayed.</span></span>

#### <a name="running-the-application"></a><span data-ttu-id="60f47-246">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="60f47-246">Running the Application</span></span>

<span data-ttu-id="60f47-247">アプリケーション レベルで例外を処理することによって提供される追加のエラーの詳細を表示するアプリケーションを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="60f47-247">You can run the application to see the additional error details provided by handling the exception at the application level.</span></span>

1. <span data-ttu-id="60f47-248">キーを押して**CTRL + F5** Wingtip Toys のサンプル アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="60f47-248">Press **CTRL+F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="60f47-249">アプリケーションによってスロー、`InvalidOperationException`します。</span><span class="sxs-lookup"><span data-stu-id="60f47-249">The application throws the `InvalidOperationException` .</span></span>
2. <span data-ttu-id="60f47-250">レビュー、 *ErrorPage.aspx*ブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-250">Review the *ErrorPage.aspx* displayed in the browser.</span></span> 

    ![ASP.NET エラー処理のアプリケーション レベルのエラー](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a><span data-ttu-id="60f47-252">ページ レベルのエラー処理を追加します。</span><span class="sxs-lookup"><span data-stu-id="60f47-252">Adding Page-Level Error Handling</span></span>

<span data-ttu-id="60f47-253">追加できますページ レベルのエラー処理ページを追加することを使用するか、`ErrorPage`属性を`@Page`または追加することで、ページのディレクティブを`Page_Error`ページの分離コードにイベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="60f47-253">You can add page-level error handling to a page either by using adding an `ErrorPage` attribute to the `@Page` directive of the page, or by adding a `Page_Error` event handler to the code-behind of a page.</span></span> <span data-ttu-id="60f47-254">このセクションでは追加、`Page_Error`転送を実行するイベント ハンドラー、 *ErrorPage.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="60f47-254">In this section, you will add a `Page_Error` event handler that will transfer execution to the *ErrorPage.aspx* page.</span></span>

1. <span data-ttu-id="60f47-255">**ソリューション エクスプ ローラー**を検索して開く、 *Default.aspx.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="60f47-255">In **Solution Explorer**, find and open the *Default.aspx.cs* file.</span></span>
2. <span data-ttu-id="60f47-256">追加、`Page_Error`ハンドラーと分離コードが表示されるように従います。</span><span class="sxs-lookup"><span data-stu-id="60f47-256">Add a `Page_Error` handler so that the code-behind appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

<span data-ttu-id="60f47-257">ページで、エラーが発生したときに、`Page_Error`イベント ハンドラーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-257">When an error occurs on the page, the `Page_Error` event handler is called.</span></span> <span data-ttu-id="60f47-258">このハンドラーでは、最後の例外が取得され、レビューします。</span><span class="sxs-lookup"><span data-stu-id="60f47-258">In this handler, the last exception is retrieved and reviewed.</span></span> <span data-ttu-id="60f47-259">場合、`InvalidOperationException`が発生した、`Page_Error`イベント ハンドラーは例外の詳細を表示する場所のエラー ページに実行を転送します。</span><span class="sxs-lookup"><span data-stu-id="60f47-259">If an `InvalidOperationException` occurs, the `Page_Error` event handler transfers execution to the error page where the exception details are displayed.</span></span>

#### <a name="running-the-application"></a><span data-ttu-id="60f47-260">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="60f47-260">Running the Application</span></span>

<span data-ttu-id="60f47-261">更新されたルートを表示するようになりましたアプリケーションを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="60f47-261">You can run the application now to see the updated routes.</span></span>

1. <span data-ttu-id="60f47-262">キーを押して**CTRL + F5** Wingtip Toys のサンプル アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="60f47-262">Press **CTRL+F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="60f47-263">アプリケーションによってスロー、`InvalidOperationException`します。</span><span class="sxs-lookup"><span data-stu-id="60f47-263">The application throws the `InvalidOperationException` .</span></span>
2. <span data-ttu-id="60f47-264">レビュー、 *ErrorPage.aspx*ブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-264">Review the *ErrorPage.aspx* displayed in the browser.</span></span> 

    ![ASP.NET エラーの処理 - ページ レベルのエラー](aspnet-error-handling/_static/image4.png)
3. <span data-ttu-id="60f47-266">ブラウザー ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="60f47-266">Close your browser window.</span></span>

### <a name="removing-the-exception-used-for-testing"></a><span data-ttu-id="60f47-267">テストに使用される例外を削除します。</span><span class="sxs-lookup"><span data-stu-id="60f47-267">Removing the Exception Used for Testing</span></span>

<span data-ttu-id="60f47-268">このチュートリアルで先ほど追加した例外をスローせずに機能する Wingtip Toys のサンプル アプリケーションを許可するのには、例外を削除します。</span><span class="sxs-lookup"><span data-stu-id="60f47-268">To allow the Wingtip Toys sample application to function without throwing the exception you added earlier in this tutorial, remove the exception.</span></span>

1. <span data-ttu-id="60f47-269">分離コードを開いて、 *Default.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="60f47-269">Open the code-behind of the *Default.aspx* page.</span></span>
2. <span data-ttu-id="60f47-270">`Page_Load`ハンドラー、ハンドラーは次のように表示されるように、例外をスローするコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="60f47-270">In the `Page_Load` handler, remove the code that throws the exception so that the handler appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a><span data-ttu-id="60f47-271">コード レベルのエラー ログ機能の追加</span><span class="sxs-lookup"><span data-stu-id="60f47-271">Adding Code-Level Error Logging</span></span>

<span data-ttu-id="60f47-272">このチュートリアルの前半で説明したように、コードのセクションを実行し、最初に発生するエラーを処理しようとする try/catch ステートメントを追加できます。</span><span class="sxs-lookup"><span data-stu-id="60f47-272">As mentioned earlier in this tutorial, you can add try/catch statements to attempt to run a section of code and handle the first error that occurs.</span></span> <span data-ttu-id="60f47-273">この例ではのみ記述するエラーの詳細エラー ログ ファイルにエラーは、後で確認できるように。</span><span class="sxs-lookup"><span data-stu-id="60f47-273">In this example, you will only write the error details to the error log file so that the error can be reviewed later.</span></span>

1. <span data-ttu-id="60f47-274">**ソリューション エクスプ ローラー**の*ロジック*フォルダー、検索、オープン、 *PayPalFunctions.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="60f47-274">In **Solution Explorer**, in the *Logic* folder, find and open the *PayPalFunctions.cs* file.</span></span>
2. <span data-ttu-id="60f47-275">更新プログラム、`HttpCall`メソッドとして、コードが表示されるように従います。</span><span class="sxs-lookup"><span data-stu-id="60f47-275">Update the `HttpCall` method so that the code appears as follows:</span></span>   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

<span data-ttu-id="60f47-276">上記のコードの呼び出し、`LogException`メソッドに含まれている、`ExceptionUtility`クラス。</span><span class="sxs-lookup"><span data-stu-id="60f47-276">The above code calls the `LogException` method that is contained in the `ExceptionUtility` class.</span></span> <span data-ttu-id="60f47-277">追加した、 *ExceptionUtility.cs*クラス ファイルを*ロジック*このチュートリアルで前にフォルダー。</span><span class="sxs-lookup"><span data-stu-id="60f47-277">You added the *ExceptionUtility.cs* class file to the *Logic* folder earlier in this tutorial.</span></span> <span data-ttu-id="60f47-278">`LogException` は、次の 2 つのパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="60f47-278">The `LogException` method takes two parameters.</span></span> <span data-ttu-id="60f47-279">最初のパラメーターは、例外オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="60f47-279">The first parameter is the exception object.</span></span> <span data-ttu-id="60f47-280">2 番目のパラメーターは、エラーの原因の認識に使用される文字列です。</span><span class="sxs-lookup"><span data-stu-id="60f47-280">The second parameter is a string used to recognize the source of the error.</span></span>

### <a name="inspecting-the-error-logging-information"></a><span data-ttu-id="60f47-281">エラー ログの情報を調べる</span><span class="sxs-lookup"><span data-stu-id="60f47-281">Inspecting the Error Logging Information</span></span>

<span data-ttu-id="60f47-282">前述のように、どのエラー、アプリケーションでは、最初に修正する必要がありますを決定するのに、エラー ログを使用できます。</span><span class="sxs-lookup"><span data-stu-id="60f47-282">As mentioned previously, you can use the error log to determine which errors in your application should be fixed first.</span></span> <span data-ttu-id="60f47-283">もちろん、トラップされ、エラー ログに記録されているエラーのみが記録されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-283">Of course, only errors that have been trapped and written to the error log will be recorded.</span></span>

1. <span data-ttu-id="60f47-284">**ソリューション エクスプ ローラー**を検索して開く、 *ErrorLog.txt*ファイル、*アプリ\_データ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="60f47-284">In **Solution Explorer**, find and open the *ErrorLog.txt* file in the *App\_Data* folder.</span></span>   
 <span data-ttu-id="60f47-285">選択する必要があります、"**すべてのファイル**"オプション、または"**更新**"オプションの先頭から**ソリューション エクスプ ローラー**を参照してください、 *ErrorLog.txt*ファイル。</span><span class="sxs-lookup"><span data-stu-id="60f47-285">You may need to select the "**Show All Files**" option or the "**Refresh**" option from the top of **Solution Explorer** to see the *ErrorLog.txt* file.</span></span>
2. <span data-ttu-id="60f47-286">Visual Studio に表示されるエラー ログを確認します。</span><span class="sxs-lookup"><span data-stu-id="60f47-286">Review the error log displayed in Visual Studio:</span></span> 

    ![ASP.NET エラーの処理 - ErrorLog.txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a><span data-ttu-id="60f47-288">安全なエラー メッセージ</span><span class="sxs-lookup"><span data-stu-id="60f47-288">Safe Error Messages</span></span>

<span data-ttu-id="60f47-289">**に注意して**こと、アプリケーションでは、エラー メッセージが表示されたら、それを提供しないように離れた悪意のあるユーザーがアプリケーションを攻撃するに役立つ情報が。</span><span class="sxs-lookup"><span data-stu-id="60f47-289">It is **important to note** that when your application displays error messages, it should not give away information that a malicious user might find helpful in attacking your application.</span></span> <span data-ttu-id="60f47-290">たとえば、アプリケーションは、失敗したデータベースへ書き込みを試みるを使用しているユーザー名を含む、エラー メッセージが表示されません。</span><span class="sxs-lookup"><span data-stu-id="60f47-290">For example, if your application unsuccessfully tries to write in to a database, it should not display an error message that includes the user name it is using.</span></span> <span data-ttu-id="60f47-291">このため、ユーザーに赤で一般的なエラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-291">For this reason, a generic error message in red is displayed to the user.</span></span> <span data-ttu-id="60f47-292">すべての追加のエラーの詳細は、ローカル コンピューター上の開発者にのみ表示されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-292">All additional error details are only displayed to the developer on the local machine.</span></span>

## <a name="using-elmah"></a><span data-ttu-id="60f47-293">ELMAH を使用します。</span><span class="sxs-lookup"><span data-stu-id="60f47-293">Using ELMAH</span></span>

<span data-ttu-id="60f47-294">ELMAH (Error Logging Modules and ハンドラー) は、NuGet パッケージとして、ASP.NET アプリケーションに接続するエラー ログ機能です。</span><span class="sxs-lookup"><span data-stu-id="60f47-294">ELMAH (Error Logging Modules and Handlers) is an error logging facility that you plug into your ASP.NET application as a NuGet package.</span></span> <span data-ttu-id="60f47-295">ELMAH は、次の機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="60f47-295">ELMAH provides the following capabilities:</span></span>

- <span data-ttu-id="60f47-296">未処理の例外のログを記録します。</span><span class="sxs-lookup"><span data-stu-id="60f47-296">Logging of unhandled exceptions.</span></span>
- <span data-ttu-id="60f47-297">記録された未処理の例外の全体のログを表示する web ページ。</span><span class="sxs-lookup"><span data-stu-id="60f47-297">A web page to view the entire log of recoded unhandled exceptions.</span></span>
- <span data-ttu-id="60f47-298">それぞれの完全な詳細を表示する web ページには、例外が記録されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-298">A web page to view the full details of each logged exception.</span></span>
- <span data-ttu-id="60f47-299">電子メールは、各エラーの発生時に通知します。</span><span class="sxs-lookup"><span data-stu-id="60f47-299">An email notification of each error at the time it occurs.</span></span>
- <span data-ttu-id="60f47-300">最後の 15 のエラー ログからの RSS フィードです。</span><span class="sxs-lookup"><span data-stu-id="60f47-300">An RSS feed of the last 15 errors from the log.</span></span>

<span data-ttu-id="60f47-301">ELMAH を使用することができます、前にインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="60f47-301">Before you can work with the ELMAH, you must install it.</span></span> <span data-ttu-id="60f47-302">これは簡単なを使用して、 *NuGet*パッケージ インストーラー。</span><span class="sxs-lookup"><span data-stu-id="60f47-302">This is easy using the *NuGet* package installer.</span></span> <span data-ttu-id="60f47-303">このチュートリアル シリーズで前に述べた、NuGet は、Visual Studio 拡張機能をインストールし、オープン ソース ライブラリと Visual Studio のツールを更新するが容易にします。</span><span class="sxs-lookup"><span data-stu-id="60f47-303">As mentioned earlier in this tutorial series, NuGet is a Visual Studio extension that makes it easy to install and update open source libraries and tools in Visual Studio.</span></span>

1. <span data-ttu-id="60f47-304">Visual Studio 内から、**ツール**メニューの **ライブラリ パッケージ マネージャー**  - &gt; **ソリューションの NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="60f47-304">Within Visual Studio, from the **Tools** menu, select **Library Package Manager** -&gt; **Manage NuGet Packages for Solution**.</span></span> 

    ![ASP.NET エラー処理 - ソリューションの NuGet パッケージの管理](aspnet-error-handling/_static/image6.png)
2. <span data-ttu-id="60f47-306">**NuGet パッケージの管理**Visual Studio 内でダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-306">The **Manage NuGet Packages** dialog box is displayed within Visual Studio.</span></span>
3. <span data-ttu-id="60f47-307">**NuGet パッケージの管理**] ダイアログ ボックスで、展開**オンライン**左側で、し、[ **nuget.org**します。次に、検索し、インストール、 **ELMAH**利用可能なパッケージをオンラインの一覧からパッケージ。</span><span class="sxs-lookup"><span data-stu-id="60f47-307">In the **Manage NuGet Packages** dialog box, expand **Online** on the left, and then select **nuget.org**. Then, find and install the **ELMAH** package from the list of available packages online.</span></span> 

    ![ASP.NET エラーの処理 - ELMA NuGet パッケージ](aspnet-error-handling/_static/image7.png)
4. <span data-ttu-id="60f47-309">パッケージをダウンロードするインターネットに接続する必要があります。</span><span class="sxs-lookup"><span data-stu-id="60f47-309">You will need to have an internet connection to download the package.</span></span>
5. <span data-ttu-id="60f47-310">**プロジェクトの選択** ダイアログ ボックスで、確認、 **WingtipToys**選択範囲が選択されているし、順にクリックします**ok**します。</span><span class="sxs-lookup"><span data-stu-id="60f47-310">In the **Select Projects** dialog box, make sure the **WingtipToys** selection is selected, and then click **OK**.</span></span> 

    ![ASP.NET エラー処理 - プロジェクト ダイアログ ボックスを選択します。](aspnet-error-handling/_static/image8.png)
6. <span data-ttu-id="60f47-312">クリックして**閉じる**で**NuGet パッケージの管理** ダイアログ ボックスが必要な場合。</span><span class="sxs-lookup"><span data-stu-id="60f47-312">Click **Close** in **the Manage NuGet Packages** dialog box if needed.</span></span>
7. <span data-ttu-id="60f47-313">Visual Studio では、開いているファイルを再読み込みすることを要求する場合は、選択"**すべてはい**"。</span><span class="sxs-lookup"><span data-stu-id="60f47-313">If Visual Studio requests that you reload any open files, select "**Yes to All**".</span></span>
8. <span data-ttu-id="60f47-314">ELMAH パッケージ自体でのエントリを追加する、 *Web.config*プロジェクトのルートにあるファイル。</span><span class="sxs-lookup"><span data-stu-id="60f47-314">The ELMAH package adds entries for itself in the *Web.config* file at the root of your project.</span></span> <span data-ttu-id="60f47-315">Visual Studio メッセージに表示されます、変更を再読み込みする*Web.config*ファイルで、をクリックして**はい**。</span><span class="sxs-lookup"><span data-stu-id="60f47-315">If Visual Studio asks you if you want to reload the modified *Web.config* file, click **Yes**.</span></span>

<span data-ttu-id="60f47-316">ELMAH は、発生する未処理のエラーを格納する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="60f47-316">ELMAH is now ready to store any unhandled errors that occur.</span></span>

### <a name="viewing-the-elmah-log"></a><span data-ttu-id="60f47-317">ELMAH ログの表示</span><span class="sxs-lookup"><span data-stu-id="60f47-317">Viewing the ELMAH Log</span></span>

<span data-ttu-id="60f47-318">ELMAH ログの表示は簡単ですが最初に、ELMAH は、ログに記録した未処理の例外を作成します。</span><span class="sxs-lookup"><span data-stu-id="60f47-318">Viewing the ELMAH log is easy, but first you will create an unhandled exception that will be recorded in the ELMAH log.</span></span>

1. <span data-ttu-id="60f47-319">キーを押して**CTRL + F5** Wingtip Toys のサンプル アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="60f47-319">Press **CTRL+F5** to run the Wingtip Toys sample application.</span></span>
2. <span data-ttu-id="60f47-320">未処理の例外をログに書き込む、ELMAH、(実際のポート番号を使用して) 次の URL をブラウザーに移動します。</span><span class="sxs-lookup"><span data-stu-id="60f47-320">To write an unhandled exception to the ELMAH log, navigate in your browser to the following URL (using your port number):</span></span>  
    <span data-ttu-id="60f47-321">`https://localhost:44300/NoPage.aspx` エラー ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-321">`https://localhost:44300/NoPage.aspx` The error page will be displayed.</span></span>
3. <span data-ttu-id="60f47-322">ELMAH のログを表示するには、(実際のポート番号を使用して) 次の URL をブラウザーで移動します。</span><span class="sxs-lookup"><span data-stu-id="60f47-322">To display the ELMAH log, navigate in your browser to the following URL (using your port number):</span></span>  
    `https://localhost:44300/elmah.axd`

    ![ASP.NET エラーの処理 - ELMAH エラー ログ](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a><span data-ttu-id="60f47-324">まとめ</span><span class="sxs-lookup"><span data-stu-id="60f47-324">Summary</span></span>

<span data-ttu-id="60f47-325">このチュートリアルでは、アプリケーション レベル、ページ レベル、およびコード レベルでのエラーの処理について学習しました。</span><span class="sxs-lookup"><span data-stu-id="60f47-325">In this tutorial, you have learned about handling errors at the application level, the page level, and the code level.</span></span> <span data-ttu-id="60f47-326">後で確認の処理済みおよび未処理のエラー ログに記録する方法についても説明しました。</span><span class="sxs-lookup"><span data-stu-id="60f47-326">You have also learned how to log handled and unhandled errors for later review.</span></span> <span data-ttu-id="60f47-327">例外のログ記録と NuGet を使用して、アプリケーションに通知を提供する、ELMAH ユーティリティを追加しました。</span><span class="sxs-lookup"><span data-stu-id="60f47-327">You added the ELMAH utility to provide exception logging and notification to your application using NuGet.</span></span> <span data-ttu-id="60f47-328">さらに、安全なエラー メッセージの重要性について説明しました。</span><span class="sxs-lookup"><span data-stu-id="60f47-328">Additionally, you have learned about the importance of safe error messages.</span></span>

## <a name="tutorial-series-conclusion"></a><span data-ttu-id="60f47-329">チュートリアル シリーズのまとめ</span><span class="sxs-lookup"><span data-stu-id="60f47-329">Tutorial Series Conclusion</span></span>

<span data-ttu-id="60f47-330">*次に沿ったいただきありがとうございます。この一連のチュートリアルでは、ASP.NET Web フォームを使い慣れてきたら役立つと思います。詳細については、Web フォームの機能 ASP.NET 4.5 と Visual Studio 2013 で使用可能な場合を参照してください* [*ASP.NET および Web Tools for Visual Studio 2013 のリリース ノート*](../../../../visual-studio/overview/2013/release-notes.md)*.また、必ずで説明したチュートリアルを見て、* ***次の手順****セクションと defintely お試し、* [*無料試用版の Azure*](https://azure.microsoft.com/pricing/free-trial/)*.*</span><span class="sxs-lookup"><span data-stu-id="60f47-330">*Thanks for following along. I hope this set of tutorials helped you become more familiar with ASP.NET Web Forms. If you need more information about Web Forms features available in ASP.NET 4.5 and Visual Studio 2013, see* [*ASP.NET and Web Tools for Visual Studio 2013 Release Notes*](../../../../visual-studio/overview/2013/release-notes.md)*. Also, be sure to take a look at the tutorial mentioned in the* ***Next Steps****section and defintely try out the* [*free Azure trial*](https://azure.microsoft.com/pricing/free-trial/)*.*</span></span>

![ありがとうございます - Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a><span data-ttu-id="60f47-332">次の手順</span><span class="sxs-lookup"><span data-stu-id="60f47-332">Next Steps</span></span>

<span data-ttu-id="60f47-333">Microsoft Azure に web アプリケーションのデプロイについてを参照してください[をセキュリティで保護された ASP.NET Web フォーム アプリのメンバーシップ、OAuth、SQL Database と Azure Web サイトへのデプロイ](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)します。</span><span class="sxs-lookup"><span data-stu-id="60f47-333">Learn more about deploying your web application to Microsoft Azure, see [Deploy a Secure ASP.NET Web Forms App with Membership, OAuth, and SQL Database to an Azure Web Site](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).</span></span>

## <a name="free-trial"></a><span data-ttu-id="60f47-334">無料試用版</span><span class="sxs-lookup"><span data-stu-id="60f47-334">Free Trial</span></span>

[<span data-ttu-id="60f47-335">Microsoft Azure の無料試用版</span><span class="sxs-lookup"><span data-stu-id="60f47-335">Microsoft Azure - Free Trial</span></span>](https://azure.microsoft.com/pricing/free-trial/)  
 <span data-ttu-id="60f47-336">Microsoft Azure に web サイトを発行節約メンテナンスの時間とコスト。</span><span class="sxs-lookup"><span data-stu-id="60f47-336">Publishing your website to Microsoft Azure will save you time, maintenance and expense.</span></span> <span data-ttu-id="60f47-337">Web アプリを Azure にデプロイする簡単なプロセスです。</span><span class="sxs-lookup"><span data-stu-id="60f47-337">It's a quick process to deploying your web app to Azure.</span></span> <span data-ttu-id="60f47-338">維持し、web アプリを監視する必要がある場合、Azure は、さまざまなツールとサービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="60f47-338">When you need to maintain and monitor your web app, Azure offers a variety of tools and services.</span></span> <span data-ttu-id="60f47-339">データ、トラフィック、id、メッセージング、メディア、およびパフォーマンスを Azure でのバックアップを管理します。</span><span class="sxs-lookup"><span data-stu-id="60f47-339">Manage data, traffic, identity, backups, messaging, media and performance in Azure.</span></span> <span data-ttu-id="60f47-340">また、非常にコスト効率に優れたアプローチですべて提供されます。</span><span class="sxs-lookup"><span data-stu-id="60f47-340">And, all of this is provided in a very cost effective approach.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="60f47-341">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="60f47-341">Additional Resources</span></span>

<span data-ttu-id="60f47-342">[ASP.NET 状態監視によるエラーの詳細をログ記録](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md) </span><span class="sxs-lookup"><span data-stu-id="60f47-342">[Logging Error Details with ASP.NET Health Monitoring](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md) </span></span>  
[<span data-ttu-id="60f47-343">ELMAH</span><span class="sxs-lookup"><span data-stu-id="60f47-343">ELMAH</span></span>](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a><span data-ttu-id="60f47-344">謝辞</span><span class="sxs-lookup"><span data-stu-id="60f47-344">Acknowledgements</span></span>

<span data-ttu-id="60f47-345">このチュートリアル シリーズのコンテンツに多大な貢献を行った以下の方々 に感謝したいと思います。</span><span class="sxs-lookup"><span data-stu-id="60f47-345">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="60f47-346">Alberto Poblacion、MVP &amp; MCT、スペイン</span><span class="sxs-lookup"><span data-stu-id="60f47-346">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- <span data-ttu-id="60f47-347">[Alex Thissen (オランダ)](http://blog.alexthissen.nl/) (twitter: [ @alexthissen ](http://twitter.com/alexthissen))</span><span class="sxs-lookup"><span data-stu-id="60f47-347">[Alex Thissen, Netherlands](http://blog.alexthissen.nl/) (twitter: [@alexthissen](http://twitter.com/alexthissen))</span></span>
- [<span data-ttu-id="60f47-348">Andre Tournier 州 (米国)</span><span class="sxs-lookup"><span data-stu-id="60f47-348">Andre Tournier, USA</span></span>](http://andret503.wordpress.com/)
- <span data-ttu-id="60f47-349">Apurva Joshi、Microsoft</span><span class="sxs-lookup"><span data-stu-id="60f47-349">Apurva Joshi, Microsoft</span></span>
- [<span data-ttu-id="60f47-350">Bojan Vrhovnik, スロベニア</span><span class="sxs-lookup"><span data-stu-id="60f47-350">Bojan Vrhovnik, Slovenia</span></span>](http://twitter.com/bvrhovnik)
- <span data-ttu-id="60f47-351">[Bruno Sonnino (ブラジル)](http://msmvps.com/blogs/bsonnino) (twitter: [ @bsonnino ](http://twitter.com/bsonnino))</span><span class="sxs-lookup"><span data-stu-id="60f47-351">[Bruno Sonnino, Brazil](http://msmvps.com/blogs/bsonnino) (twitter: [@bsonnino](http://twitter.com/bsonnino))</span></span>
- [<span data-ttu-id="60f47-352">Carlos dos Santos、ブラジル</span><span class="sxs-lookup"><span data-stu-id="60f47-352">Carlos dos Santos, Brazil</span></span>](http://www.carloscds.net/)
- <span data-ttu-id="60f47-353">[Dave Campbell、USA](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))</span><span class="sxs-lookup"><span data-stu-id="60f47-353">[Dave Campbell, USA](http://www.wynapse.com/) (twitter: [@windowsdevnews](http://twitter.com/windowsdevnews))</span></span>
- <span data-ttu-id="60f47-354">[Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))</span><span class="sxs-lookup"><span data-stu-id="60f47-354">[Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [@jongalloway](http://twitter.com/jongalloway))</span></span>
- <span data-ttu-id="60f47-355">[Michael シャープ、USA](http://www.930solutions.com/) (twitter: [ @mrsharps ](http://twitter.com/mrsharps))</span><span class="sxs-lookup"><span data-stu-id="60f47-355">[Michael Sharps, USA](http://www.930solutions.com/) (twitter: [@mrsharps](http://twitter.com/mrsharps))</span></span>
- <span data-ttu-id="60f47-356">Mike 教皇</span><span class="sxs-lookup"><span data-stu-id="60f47-356">Mike Pope</span></span>
- <span data-ttu-id="60f47-357">[Mitchel 販売者、USA](http://www.mitchelsellers.com/) (twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))</span><span class="sxs-lookup"><span data-stu-id="60f47-357">[Mitchel Sellers, USA](http://www.mitchelsellers.com/) (twitter: [@MitchelSellers](http://twitter.com/MitchelSellers))</span></span>
- [<span data-ttu-id="60f47-358">Paul Cociuba、Microsoft</span><span class="sxs-lookup"><span data-stu-id="60f47-358">Paul Cociuba, Microsoft</span></span>](http://linqto.me/Links/pcociuba)
- [<span data-ttu-id="60f47-359">Paulo Morgado (ポルトガル)</span><span class="sxs-lookup"><span data-stu-id="60f47-359">Paulo Morgado, Portugal</span></span>](http://paulomorgado.net/)
- [<span data-ttu-id="60f47-360">Pranav Rastogi、Microsoft</span><span class="sxs-lookup"><span data-stu-id="60f47-360">Pranav Rastogi, Microsoft</span></span>](https://blogs.msdn.com/b/pranav_rastogi)
- [<span data-ttu-id="60f47-361">Tim Ammann、Microsoft</span><span class="sxs-lookup"><span data-stu-id="60f47-361">Tim Ammann, Microsoft</span></span>](https://blogs.iis.net/timamm/default.aspx)
- [<span data-ttu-id="60f47-362">Tom Dykstra、Microsoft</span><span class="sxs-lookup"><span data-stu-id="60f47-362">Tom Dykstra, Microsoft</span></span>](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a><span data-ttu-id="60f47-363">コミュニティへの投稿</span><span class="sxs-lookup"><span data-stu-id="60f47-363">Community Contributions</span></span>

- <span data-ttu-id="60f47-364">Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))</span><span class="sxs-lookup"><span data-stu-id="60f47-364">Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))</span></span>  
  <span data-ttu-id="60f47-365">Visual Studio 2012 に関連する msdn コード サンプル: [Wingtip Toys のナビゲーション](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)</span><span class="sxs-lookup"><span data-stu-id="60f47-365">Visual Studio 2012 related code sample on MSDN: [Navigation Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)</span></span>
- <span data-ttu-id="60f47-366">James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))</span><span class="sxs-lookup"><span data-stu-id="60f47-366">James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))</span></span>  
  <span data-ttu-id="60f47-367">Visual Studio 2012 に関連する msdn コード サンプル: [Visual Basic で ASP.NET 4.5 Web フォームのチュートリアル シリーズ](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)</span><span class="sxs-lookup"><span data-stu-id="60f47-367">Visual Studio 2012 related code sample on MSDN: [ASP.NET 4.5 Web Forms Tutorial Series in Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)</span></span>
- <span data-ttu-id="60f47-368">Andrielle Azevedo - Microsoft テクニカル対象ユーザーの共同作成者 (twitter: @driazevedo)</span><span class="sxs-lookup"><span data-stu-id="60f47-368">Andrielle Azevedo - Microsoft Technical Audience Contributor (twitter: @driazevedo)</span></span>  
  <span data-ttu-id="60f47-369">Visual Studio 2012 の翻訳: [Iniciando com Visão Geral ASP.NET Web フォームの 4.5 - Parte 1 - Introdução e](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)</span><span class="sxs-lookup"><span data-stu-id="60f47-369">Visual Studio 2012 translation: [Iniciando com ASP.NET Web Forms 4.5 - Parte 1 - Introdução e Visão Geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="60f47-370">前へ</span><span class="sxs-lookup"><span data-stu-id="60f47-370">Previous</span></span>](url-routing.md)
