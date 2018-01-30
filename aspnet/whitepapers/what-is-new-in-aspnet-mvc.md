---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: "ASP.NET MVC 2 の新機能 |Microsoft ドキュメント"
author: rick-anderson
description: "このドキュメントでは、新しい機能と ASP.NET MVC 2 で導入された機能強化について説明します。 このドキュメントはダウンロードもできます。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/20/2010
ms.topic: article
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: 808f51b48b31e21848d76e7ded436ca1b17901d2
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
<a name="whats-new-in-aspnet-mvc-2"></a><span data-ttu-id="d4847-104">ASP.NET MVC 2 の新機能</span><span class="sxs-lookup"><span data-stu-id="d4847-104">What's New in ASP.NET MVC 2</span></span>
====================
> <span data-ttu-id="d4847-105">このドキュメントでは、新しい機能と ASP.NET MVC 2 で導入された機能強化について説明します。</span><span class="sxs-lookup"><span data-stu-id="d4847-105">This document describes new features and improvements introduced in ASP.NET MVC 2.</span></span> <span data-ttu-id="d4847-106">このドキュメントも[ダウンロード](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)</span><span class="sxs-lookup"><span data-stu-id="d4847-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)</span></span>


<span data-ttu-id="d4847-107">[概要](#_TOC1) </span><span class="sxs-lookup"><span data-stu-id="d4847-107">[Introduction](#_TOC1) </span></span>  
<span data-ttu-id="d4847-108">[ASP.NET MVC 2 に ASP.NET MVC 1.0 プロジェクトをアップグレードします。](#_TOC2) </span><span class="sxs-lookup"><span data-stu-id="d4847-108">[Upgrading an ASP.NET MVC 1.0 Project to ASP.NET MVC 2](#_TOC2) </span></span>  
<span data-ttu-id="d4847-109">[新機能](#_TOC3) </span><span class="sxs-lookup"><span data-stu-id="d4847-109">[New Features](#_TOC3) </span></span>  
<span data-ttu-id="d4847-110">[テンプレート化されたヘルパー](#_TOC3_1) </span><span class="sxs-lookup"><span data-stu-id="d4847-110">[Templated Helpers](#_TOC3_1) </span></span>  
<span data-ttu-id="d4847-111">[領域](#_TOC3_2) </span><span class="sxs-lookup"><span data-stu-id="d4847-111">[Areas](#_TOC3_2) </span></span>  
<span data-ttu-id="d4847-112">[非同期コント ローラーのサポート](#_TOC3_3) </span><span class="sxs-lookup"><span data-stu-id="d4847-112">[Support for Asynchronous Controllers](#_TOC3_3) </span></span>  
<span data-ttu-id="d4847-113">[DefaultValueAttribute アクション メソッド パラメーターのサポート](#_TOC3_4) </span><span class="sxs-lookup"><span data-stu-id="d4847-113">[Support for DefaultValueAttribute in Action-Method Parameters](#_TOC3_4) </span></span>  
<span data-ttu-id="d4847-114">[モデル バインダーのバイナリ データをバインドするためのサポート](#_TOC3_5) </span><span class="sxs-lookup"><span data-stu-id="d4847-114">[Support for Binding Binary Data with Model Binders](#_TOC3_5) </span></span>  
<span data-ttu-id="d4847-115">[ModelMetadata と ModelMetadataProvider クラス](#_TOC3_6) </span><span class="sxs-lookup"><span data-stu-id="d4847-115">[ModelMetadata and ModelMetadataProvider Classes](#_TOC3_6) </span></span>  
<span data-ttu-id="d4847-116">[DataAnnotations 属性のサポート](#_TOC3_7) </span><span class="sxs-lookup"><span data-stu-id="d4847-116">[Support for DataAnnotations Attributes](#_TOC3_7) </span></span>  
<span data-ttu-id="d4847-117">[モデルの検証コントロール プロバイダー](#_TOC3_8) </span><span class="sxs-lookup"><span data-stu-id="d4847-117">[Model-Validator Providers](#_TOC3_8) </span></span>  
<span data-ttu-id="d4847-118">[クライアント側の検証](#_TOC3_9) </span><span class="sxs-lookup"><span data-stu-id="d4847-118">[Client-Side Validation](#_TOC3_9) </span></span>  
<span data-ttu-id="d4847-119">[Visual Studio 2010 用の新しいコード スニペット](#_TOC3_10) </span><span class="sxs-lookup"><span data-stu-id="d4847-119">[New Code Snippets for Visual Studio 2010](#_TOC3_10) </span></span>  
<span data-ttu-id="d4847-120">[新しい RequireHttpsAttribute アクション フィルター](#_TOC3_11) </span><span class="sxs-lookup"><span data-stu-id="d4847-120">[New RequireHttpsAttribute Action Filter](#_TOC3_11) </span></span>  
<span data-ttu-id="d4847-121">[HTTP メソッドの動詞のオーバーライド](#_TOC3_12) </span><span class="sxs-lookup"><span data-stu-id="d4847-121">[Overriding the HTTP Method Verb](#_TOC3_12) </span></span>  
<span data-ttu-id="d4847-122">[テンプレート化されたヘルパーの新しい HiddenInputAttribute クラス](#_TOC3_13) </span><span class="sxs-lookup"><span data-stu-id="d4847-122">[New HiddenInputAttribute Class for Templated Helpers](#_TOC3_13) </span></span>  
<span data-ttu-id="d4847-123">[Html.ValidationSummary ヘルパー メソッドはモデル レベルのエラーを表示できます。](#_TOC3_14) </span><span class="sxs-lookup"><span data-stu-id="d4847-123">[Html.ValidationSummary Helper Method Can Display Model-Level Errors](#_TOC3_14) </span></span>  
<span data-ttu-id="d4847-124">[Visual Studio の生成コードに固有では T4 テンプレートを .NET Framework のターゲット バージョンに](#_TOC3_15)[API の機能強化](#_TOC4)</span><span class="sxs-lookup"><span data-stu-id="d4847-124">[T4 Templates in Visual Studio Generate Code that is Specific to the Target Version of the .NET Framework](#_TOC3_15)[API Improvements](#_TOC4)</span></span>  
[<span data-ttu-id="d4847-125">重大な変更</span><span class="sxs-lookup"><span data-stu-id="d4847-125">Breaking Changes</span></span>](#_TOC5)  
[<span data-ttu-id="d4847-126">免責事項</span><span class="sxs-lookup"><span data-stu-id="d4847-126">Disclaimer</span></span>](#_TOC6)  

## <a id="_TOC1"></a><span data-ttu-id="d4847-127">概要</span><span class="sxs-lookup"><span data-stu-id="d4847-127">Introduction</span></span>

<span data-ttu-id="d4847-128">ASP.NET MVC 2 は、ASP.NET MVC 1.0 を上に構築し、豊富な機能強化と生産性を高めることに注目している機能を紹介します。</span><span class="sxs-lookup"><span data-stu-id="d4847-128">ASP.NET MVC 2 builds on ASP.NET MVC 1.0 and introduces a large set of enhancements and features that are focused on increasing productivity.</span></span> <span data-ttu-id="d4847-129">このリリースは ASP.NET MVC 1.0 と互換性のあるすべてのナレッジ、スキル、コード、および ASP.NET MVC 1.0 用の拡張機能の適用を続行します。</span><span class="sxs-lookup"><span data-stu-id="d4847-129">This release is compatible with ASP.NET MVC 1.0, so all your knowledge, skills, code, and extensions for ASP.NET MVC 1.0 continue to apply.</span></span>

<span data-ttu-id="d4847-130">ASP.NET MVC の詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4847-130">For more information about ASP.NET MVC, visit the following resources:</span></span>

- [<span data-ttu-id="d4847-131">ASP.NET MVC の MSDN ドキュメントへ</span><span class="sxs-lookup"><span data-stu-id="d4847-131">ASP.NET MVC documentation on MSDN</span></span>](https://go.microsoft.com/fwlink/?LinkId=159758)
- [<span data-ttu-id="d4847-132">ASP.NET MVC Web サイト</span><span class="sxs-lookup"><span data-stu-id="d4847-132">The ASP.NET MVC Web site</span></span>](https://asp.net/mvc/)
- [<span data-ttu-id="d4847-133">ASP.NET MVC フォーラム</span><span class="sxs-lookup"><span data-stu-id="d4847-133">The ASP.NET MVC forums</span></span>](https://forums.asp.net/1146.aspx)

## <a id="_TOC2"></a><span data-ttu-id="d4847-134">ASP.NET MVC 2 に ASP.NET MVC 1.0 プロジェクトをアップグレードします。</span><span class="sxs-lookup"><span data-stu-id="d4847-134">Upgrading an ASP.NET MVC 1.0 Project to ASP.NET MVC 2</span></span>

<span data-ttu-id="d4847-135">ASP.NET MVC 2 は、ASP.NET MVC 2 を ASP.NET MVC 1.0 アプリケーションをアップグレードするタイミングを選択するには、アプリケーション開発者の柔軟性、同じサーバー上の ASP.NET MVC 1.0 サイド バイ サイドでインストールできます。</span><span class="sxs-lookup"><span data-stu-id="d4847-135">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server, which gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span> <span data-ttu-id="d4847-136">アップグレードする方法については、ドキュメントを参照してください。 [ASP.NET MVC 2 には、ASP.NET MVC 1.0 アプリケーションをアップグレードする](https://go.microsoft.com/fwlink/?LinkID=185459)です。</span><span class="sxs-lookup"><span data-stu-id="d4847-136">For information on how to upgrade, see the document [Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185459).</span></span>

## <a id="_TOC3"></a><span data-ttu-id="d4847-137">新機能</span><span class="sxs-lookup"><span data-stu-id="d4847-137">New Features</span></span>

<span data-ttu-id="d4847-138">このセクションの内容が導入された機能について説明します、MVC 2 リリースでします。</span><span class="sxs-lookup"><span data-stu-id="d4847-138">This section describes features that have been introduced in the MVC 2 release.</span></span>

### <a id="_TOC3_1"></a><span data-ttu-id="d4847-139">テンプレート化されたヘルパー</span><span class="sxs-lookup"><span data-stu-id="d4847-139">Templated Helpers</span></span>

<span data-ttu-id="d4847-140">テンプレート化されたヘルパーは、編集用に自動的に関連付ける HTML 要素を使用してのデータ型を表示します。</span><span class="sxs-lookup"><span data-stu-id="d4847-140">Templated helpers let you automatically associate HTML elements for edit and display with data types.</span></span> <span data-ttu-id="d4847-141">たとえば、System.DateTime 型のデータが表示されたら、ビューで、日付選択カレンダー UI 要素に自動的に表示できます。</span><span class="sxs-lookup"><span data-stu-id="d4847-141">For example, when data of type System.DateTime is displayed in a view, a date-picker UI element can be automatically rendered.</span></span> <span data-ttu-id="d4847-142">ASP.NET 動的データでのフィールド テンプレートの作業に似ています。</span><span class="sxs-lookup"><span data-stu-id="d4847-142">This is similar to how field templates work in ASP.NET Dynamic Data.</span></span> <span data-ttu-id="d4847-143">詳細については、次を参照してください。[データを表示するテンプレート化されたヘルパーを使用して](https://go.microsoft.com/fwlink/?LinkId=159062)MSDN Web サイトです。</span><span class="sxs-lookup"><span data-stu-id="d4847-143">For more information, see [Using Templated Helpers to Display Data](https://go.microsoft.com/fwlink/?LinkId=159062) on the MSDN Web site.</span></span>

### <a id="_TOC3_2"></a><span data-ttu-id="d4847-144">領域</span><span class="sxs-lookup"><span data-stu-id="d4847-144">Areas</span></span>

<span data-ttu-id="d4847-145">領域では、大規模な Web アプリケーションの複雑さを管理するために複数の小さなセクションに、大規模なプロジェクトを整理できます。</span><span class="sxs-lookup"><span data-stu-id="d4847-145">Areas let you organize a large project into multiple smaller sections in order to manage the complexity of a large Web application.</span></span> <span data-ttu-id="d4847-146">(「領域」) の各セクションは通常、大規模な Web サイトの別のセクションを表すし、コント ローラーとビューの関連する設定をグループ化するために使用します。</span><span class="sxs-lookup"><span data-stu-id="d4847-146">Each section ("area") typically represents a separate section of a large Web site and is used to group related sets of controllers and views.</span></span> <span data-ttu-id="d4847-147">詳細については、次を参照してください。[チュートリアル: ASP.NET MVC アプリケーションによって、アプリケーションの領域を整理する](https://go.microsoft.com/fwlink/?LinkId=158978)MSDN Web サイトです。</span><span class="sxs-lookup"><span data-stu-id="d4847-147">For more information, see [Walkthrough: Organizing an ASP.NET MVC Application by Areas](https://go.microsoft.com/fwlink/?LinkId=158978) on the MSDN Web site.</span></span>

<span data-ttu-id="d4847-148">ソリューション エクスプ ローラーで、新しい領域を作成するにプロジェクトを右クリックして、[追加] をクリックしておよび領域をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d4847-148">To create a new area, in Solution Explorer, right-click the project, click Add, and then click Area.</span></span> <span data-ttu-id="d4847-149">これには、領域名の入力を求めるダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d4847-149">This displays a dialog box that prompts you for the area name.</span></span> <span data-ttu-id="d4847-150">領域の名前を入力した後、Visual Studio はプロジェクトに新しい領域を追加します。</span><span class="sxs-lookup"><span data-stu-id="d4847-150">After you enter the area name, Visual Studio adds a new area to the project.</span></span>

<span data-ttu-id="d4847-151">次の図は、管理者、ブログの次の 2 つの領域を持つプロジェクトのレイアウトの例を示します。</span><span class="sxs-lookup"><span data-stu-id="d4847-151">The following figure shows an example layout for a project with two areas, Admin and Blogs.</span></span>

![](what-is-new-in-aspnet-mvc/_static/image1.png)

<span data-ttu-id="d4847-152">領域を作成するときに、Visual Studio は、各領域に AreaRegistration から派生するクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="d4847-152">When you create an area, Visual Studio adds a class that derives from AreaRegistration to each area.</span></span> <span data-ttu-id="d4847-153">次の例で示すようにこのクラスは、領域とそのルートを登録するために必要です。</span><span class="sxs-lookup"><span data-stu-id="d4847-153">This class is required in order to register the area and its routes, as shown in the following example:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

<span data-ttu-id="d4847-154">ASP.NET MVC 2 の既定のプロジェクト テンプレートには、Global.asax ファイルのコードで RegisterAllAreas メソッドへの呼び出しが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d4847-154">The default project template for ASP.NET MVC 2 includes a call to the RegisterAllAreas method in the code for the Global.asax file.</span></span> <span data-ttu-id="d4847-155">このメソッドは、AreaRegistration クラスから派生したすべての型を探して、インスタンス化して、型のインスタンスをインスタンスで RegisterArea メソッドを呼び出して、プロジェクト内の各領域を登録します。</span><span class="sxs-lookup"><span data-stu-id="d4847-155">This method registers each area in the project by looking for all types that derive from the AreaRegistration class, instantiating an instance of the type, and then calling the RegisterArea method on the instance.</span></span> <span data-ttu-id="d4847-156">次の例では、これを行う方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d4847-156">The following example shows how this is done.</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

<span data-ttu-id="d4847-157">RegisterArea メソッドでコンテキストを呼び出すことによって、名前空間を指定しないでください場合。Namespaces.Add メソッド、登録クラスの名前空間は、既定で使用されます。</span><span class="sxs-lookup"><span data-stu-id="d4847-157">If you do not specify the namespace in the RegisterArea method by calling the context.Namespaces.Add method, the namespace of the registration class is used by default.</span></span>

### <a id="_TOC3_3"></a><span data-ttu-id="d4847-158">非同期コント ローラーのサポート</span><span class="sxs-lookup"><span data-stu-id="d4847-158">Support for Asynchronous Controllers</span></span>

<span data-ttu-id="d4847-159">ASP.NET MVC 2 では、要求を非同期的に処理するコント ローラーができるようになりました。</span><span class="sxs-lookup"><span data-stu-id="d4847-159">ASP.NET MVC 2 now allows controllers to process requests asynchronously.</span></span> <span data-ttu-id="d4847-160">これについては、非ブロッキング古くてを呼び出してくださいを頻繁に (ネットワーク要求) などのブロッキング操作を呼び出すことがサーバーを許可することでパフォーマンスの向上につながります。</span><span class="sxs-lookup"><span data-stu-id="d4847-160">This can lead to performance gains by allowing servers which frequently call blocking operations (like network requests) to call non-blocking counterparts instead.</span></span> <span data-ttu-id="d4847-161">詳細については、次を参照してください。、 [ASP.NET MVC での非同期コント ローラーを使用して](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx)MSDN のトピックです。</span><span class="sxs-lookup"><span data-stu-id="d4847-161">For more information, see the [Using an Asynchronous Controller in ASP.NET MVC](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx) topic on MSDN.</span></span>

### <a id="_TOC3_4"></a><span data-ttu-id="d4847-162">DefaultValueAttribute アクション メソッド パラメーターのサポート</span><span class="sxs-lookup"><span data-stu-id="d4847-162">Support for DefaultValueAttribute in Action-Method Parameters</span></span>

<span data-ttu-id="d4847-163">System.ComponentModel.DefaultValueAttribute クラスは、アクション メソッドの引数パラメーターに対して指定する既定値を使用できます。</span><span class="sxs-lookup"><span data-stu-id="d4847-163">The System.ComponentModel.DefaultValueAttribute class allows a default value to be supplied for the argument parameter to an action method.</span></span> <span data-ttu-id="d4847-164">たとえば、次の既定のルートが定義されていると仮定します。</span><span class="sxs-lookup"><span data-stu-id="d4847-164">For example, assume that the following default route is defined:</span></span>

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

<span data-ttu-id="d4847-165">次のコント ローラーとアクション メソッドが定義されていると想定されます。</span><span class="sxs-lookup"><span data-stu-id="d4847-165">Also assume that the following controller and action method is defined:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

<span data-ttu-id="d4847-166">次のような要求の Url はメソッドを呼び出すビュー アクション前の例で定義されています。</span><span class="sxs-lookup"><span data-stu-id="d4847-166">Any of the following request URLs will invoke the View action method that is defined in the preceding example.</span></span>

- <span data-ttu-id="d4847-167">/Article/View/123</span><span class="sxs-lookup"><span data-stu-id="d4847-167">/Article/View/123</span></span>
- <span data-ttu-id="d4847-168">/記事/ビュー/123 ですか? ページ (実質的に同じ、前回の要求) 1 を =</span><span class="sxs-lookup"><span data-stu-id="d4847-168">/Article/View/123?page=1 (Effectively the same as the previous request)</span></span>
- <span data-ttu-id="d4847-169">/記事/ビュー/123 ですか? ページ 2 を =</span><span class="sxs-lookup"><span data-stu-id="d4847-169">/Article/View/123?page=2</span></span>

<span data-ttu-id="d4847-170">DefaultValueAttribute 属性がない、上記の一覧から最初の URL は機能しません、ページ引数は値が指定されていない null 非許容値型であるためです。</span><span class="sxs-lookup"><span data-stu-id="d4847-170">Without the DefaultValueAttribute attribute, the first URL from the preceding list would not work, because the page argument is a non-nullable value type whose value has not been provided.</span></span>

<span data-ttu-id="d4847-171">Visual Basic 2010 または Visual c# 2010 で、コードが書き込まれると場合、は、次の例のように、DefaultValueAttribute 属性ではなく省略可能なパラメーターを使用できます。</span><span class="sxs-lookup"><span data-stu-id="d4847-171">If your code is written in Visual Basic 2010 or Visual C# 2010, you can use optional parameters instead of the DefaultValueAttribute attribute, as shown in the following example:</span></span>

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a id="_TOC3_5"></a><span data-ttu-id="d4847-172">モデル バインダーのバイナリ データをバインドするためのサポート</span><span class="sxs-lookup"><span data-stu-id="d4847-172">Support for Binding Binary Data with Model Binders</span></span>

<span data-ttu-id="d4847-173">これには 2 つの新しいオーバー ロードされた Html.Hidden ヘルパー base 64 エンコード文字列としてのバイナリ値をエンコードするがあります。</span><span class="sxs-lookup"><span data-stu-id="d4847-173">There are two new overloads of the Html.Hidden helper that encode binary values as base-64-encoded strings:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

<span data-ttu-id="d4847-174">一般的な使用は、ビューでオブジェクトのタイムスタンプを埋め込むにです。</span><span class="sxs-lookup"><span data-stu-id="d4847-174">A typical use is to embed a timestamp for an object in the view.</span></span> <span data-ttu-id="d4847-175">たとえば、アプリケーションには、次の製品オブジェクトが含まれます。</span><span class="sxs-lookup"><span data-stu-id="d4847-175">For example, your application might include the following Product object:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

<span data-ttu-id="d4847-176">編集フォームは、次の例で示すように、フォームでタイムスタンプ プロパティを表示します。</span><span class="sxs-lookup"><span data-stu-id="d4847-176">An edit form can render the TimeStamp property in the form as shown in the following example:</span></span>

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

<span data-ttu-id="d4847-177">このマークアップは、次の例のような base 64 エンコードされた文字列としてのタイムスタンプ値で、非表示の input 要素を表示します。</span><span class="sxs-lookup"><span data-stu-id="d4847-177">This markup renders a hidden input element with the timestamp value as a base-64-encoded string that resembles the following example:</span></span>

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

<span data-ttu-id="d4847-178">このフォームは、次の例で示すように、型の引数を持つアクション メソッドへポストされた可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d4847-178">This form might be posted to an action method that has an argument of type Product, as shown in the following example:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

<span data-ttu-id="d4847-179">アクション メソッドに、タイムスタンプ プロパティが設定されます正しくポストの base 64 エンコード文字列がバイト配列に変換されるためです。</span><span class="sxs-lookup"><span data-stu-id="d4847-179">In the action method, the TimeStamp property is populated correctly because the posted base-64-encoded string is converted to a byte array.</span></span>

### <a id="_TOC3_6"></a><span data-ttu-id="d4847-180">ModelMetadata と ModelMetadataProvider クラス</span><span class="sxs-lookup"><span data-stu-id="d4847-180">ModelMetadata and ModelMetadataProvider Classes</span></span>

<span data-ttu-id="d4847-181">ModelMetadataProvider クラスでは、ビュー内のモデルのメタデータを取得するための抽象化を提供します。</span><span class="sxs-lookup"><span data-stu-id="d4847-181">The ModelMetadataProvider class provides an abstraction for obtaining metadata for the model within a view.</span></span> <span data-ttu-id="d4847-182">MVC 2 には System.ComponentModel.DataAnnotations 名前空間内の属性によって公開されるメタデータを利用できるようにする既定のプロバイダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d4847-182">MVC 2 includes a default provider that makes available the metadata that is exposed by the attributes in the System.ComponentModel.DataAnnotations namespace.</span></span> <span data-ttu-id="d4847-183">データベースまたは XML ファイルなど、他のデータ ストアからメタデータを提供するメタデータ プロバイダーを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="d4847-183">It is possible to create metadata providers that provide metadata from other data stores, such as databases or XML files.</span></span>

<span data-ttu-id="d4847-184">ViewDataDictionary クラスでは、ModelMetadataProvider クラスによって、モデルから抽出されたメタデータを含む ModelMetadata オブジェクトを公開します。</span><span class="sxs-lookup"><span data-stu-id="d4847-184">The ViewDataDictionary class exposes a ModelMetadata object that contains the metadata that is extracted from the model by the ModelMetadataProvider class.</span></span> <span data-ttu-id="d4847-185">これにより、テンプレート化されたヘルパーをこのメタデータを使用し、その出力は必要に応じて調整できます。</span><span class="sxs-lookup"><span data-stu-id="d4847-185">This enables the templated helpers to consume this metadata and adjust their output accordingly.</span></span>

<span data-ttu-id="d4847-186">詳細については、ドキュメントを参照して、 [ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx)と[ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx)クラスです。</span><span class="sxs-lookup"><span data-stu-id="d4847-186">For more information, see the documentation for the [ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) and [ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) classes.</span></span>

### <a id="_TOC3_7"></a><span data-ttu-id="d4847-187">DataAnnotations 属性のサポート</span><span class="sxs-lookup"><span data-stu-id="d4847-187">Support for DataAnnotations Attributes</span></span>

<span data-ttu-id="d4847-188">ASP.NET MVC 2 (System.ComponentModel.DataAnnotations 名前空間で定義されている) RangeAttribute、RequiredAttribute、StringLengthAttribute、および RegexAttribute 検証属性の使用をサポートしている入力を提供するために、モデルへのバインド検証します。</span><span class="sxs-lookup"><span data-stu-id="d4847-188">ASP.NET MVC 2 supports using the RangeAttribute, RequiredAttribute, StringLengthAttribute, and RegexAttribute validation attributes (defined in the System.ComponentModel.DataAnnotations namespace) when you bind to a model in order to provide input validation.</span></span>

<span data-ttu-id="d4847-189">詳細については、次を参照してください。[する方法: 検証モデル データ DataAnnotations 属性を使用した](https://go.microsoft.com/fwlink/?LinkId=159063)MSDN Web サイトです。</span><span class="sxs-lookup"><span data-stu-id="d4847-189">For more information, see [How to: Validate Model Data Using DataAnnotations Attributes](https://go.microsoft.com/fwlink/?LinkId=159063) on the MSDN Web site.</span></span> <span data-ttu-id="d4847-190">これらの属性の使用方法について説明するサンプル プロジェクトはダウンロード[https://go.microsoft.com/fwlink/?LinkId=157753](https://go.microsoft.com/fwlink/?LinkId=157753)です。</span><span class="sxs-lookup"><span data-stu-id="d4847-190">A sample project that illustrates the use of these attributes is available for download at [https://go.microsoft.com/fwlink/?LinkId=157753](https://go.microsoft.com/fwlink/?LinkId=157753).</span></span>

### <a id="_TOC3_8"></a><span data-ttu-id="d4847-191">モデルの検証コントロール プロバイダー</span><span class="sxs-lookup"><span data-stu-id="d4847-191">Model-Validator Providers</span></span>

<span data-ttu-id="d4847-192">モデル検証プロバイダー クラスは、モデルの検証ロジックを提供する抽象型を表します。</span><span class="sxs-lookup"><span data-stu-id="d4847-192">The model-validation provider class represents an abstraction that provides validation logic for the model.</span></span> <span data-ttu-id="d4847-193">ASP.NET MVC には、System.ComponentModel.DataAnnotations 名前空間に含まれている検証の属性に基づいて既定のプロバイダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d4847-193">ASP.NET MVC includes a default provider based on validation attributes that are included in the System.ComponentModel.DataAnnotations namespace.</span></span> <span data-ttu-id="d4847-194">モデルへのカスタム検証規則と検証規則のカスタム マッピングを定義する、独自の検証プロバイダーを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="d4847-194">You can also create your own validation providers that define custom validation rules and custom mappings of validation rules to the model.</span></span> <span data-ttu-id="d4847-195">詳細については、ドキュメントを参照して、 [ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx)クラスです。</span><span class="sxs-lookup"><span data-stu-id="d4847-195">For more information, see the documentation for the [ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx) class.</span></span>

### <a id="_TOC3_9"></a><span data-ttu-id="d4847-196">クライアント側の検証</span><span class="sxs-lookup"><span data-stu-id="d4847-196">Client-Side Validation</span></span>

<span data-ttu-id="d4847-197">モデルの検証コントロール プロバイダー クラスは、クライアント側検証ライブラリで利用できる JSON でシリアル化されたデータの形でブラウザーに検証メタデータを公開します。</span><span class="sxs-lookup"><span data-stu-id="d4847-197">The model-validator provider class exposes validation metadata to the browser in the form of JSON-serialized data that can be consumed by a client-side validation library.</span></span> <span data-ttu-id="d4847-198">ASP.NET MVC 2 には、クライアント検証ライブラリおよび前に述べた DataAnnotations 名前空間の検証属性をサポートするアダプターが含まれます。</span><span class="sxs-lookup"><span data-stu-id="d4847-198">ASP.NET MVC 2 includes a client validation library and adapter that supports the DataAnnotations namespace validation attributes noted earlier.</span></span> <span data-ttu-id="d4847-199">プロバイダー クラスを使用して、JSON データと代替のライブラリへの呼び出しを処理するアダプターを記述して他のクライアント検証ライブラリを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="d4847-199">The provider class also enables you to use other client-validation libraries by writing an adapter that processes the JSON data and calls into the alternate library.</span></span>

### <a id="_TOC3_10"></a><span data-ttu-id="d4847-200">Visual Studio 2010 用の新しいコード スニペット</span><span class="sxs-lookup"><span data-stu-id="d4847-200">New Code Snippets for Visual Studio 2010</span></span>

<span data-ttu-id="d4847-201">ASP.NET MVC 2 の HTML コード スニペットのセットは、Visual Studio 2010 と共にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="d4847-201">A set of HTML code snippets for ASP.NET MVC 2 is installed with Visual Studio 2010.</span></span> <span data-ttu-id="d4847-202">[ツール] メニューで、これらのスニペットの一覧を表示するには、コード スニペット マネージャーを選択します。</span><span class="sxs-lookup"><span data-stu-id="d4847-202">To view a list of these snippets, in the Tools menu, select Code Snippets Manager.</span></span> <span data-ttu-id="d4847-203">言語については、HTML を選択し、場所、ASP.NET MVC 2 を選択します。</span><span class="sxs-lookup"><span data-stu-id="d4847-203">For the language, select HTML, and for location, select ASP.NET MVC 2.</span></span> <span data-ttu-id="d4847-204">コード スニペットを使用する方法の詳細については、Visual Studio のマニュアルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d4847-204">For more information about how to use code snippets, see the Visual Studio documentation.</span></span>

### <a id="_TOC3_11"></a><span data-ttu-id="d4847-205">新しい RequireHttpsAttribute アクション フィルター</span><span class="sxs-lookup"><span data-stu-id="d4847-205">New RequireHttpsAttribute Action Filter</span></span>

<span data-ttu-id="d4847-206">ASP.NET MVC 2 には、アクション メソッドとコント ローラーに適用できる新しい RequireHttpsAttribute クラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d4847-206">ASP.NET MVC 2 includes a new RequireHttpsAttribute class that can be applied to action methods and controllers.</span></span> <span data-ttu-id="d4847-207">既定では、フィルターは、SSL 対応 (HTTPS) に相当する、SSL 以外の HTTP 要求をリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="d4847-207">By default, the filter redirects a non-SSL (HTTP) request to the SSL-enabled (HTTPS) equivalent.</span></span>

### <a id="_TOC3_12"></a><span data-ttu-id="d4847-208">HTTP メソッドの動詞のオーバーライド</span><span class="sxs-lookup"><span data-stu-id="d4847-208">Overriding the HTTP Method Verb</span></span>

<span data-ttu-id="d4847-209">REST アーキテクチャ スタイルを使用して Web サイトを構築するときは、HTTP 動詞を使用してリソースに対して実行するアクションを決定します。</span><span class="sxs-lookup"><span data-stu-id="d4847-209">When you build a Web site by using the REST architectural style, HTTP verbs are used to determine which action to perform for a resource.</span></span> <span data-ttu-id="d4847-210">残りの部分では、アプリケーションのサポートを含む、共通の HTTP 動詞のすべての範囲の GET、PUT、POST、および削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4847-210">REST requires that applications support the full range of common HTTP verbs, including GET, PUT, POST, and DELETE.</span></span>

<span data-ttu-id="d4847-211">ASP.NET MVC 2 には、アクション メソッドとその機能のコンパクトな構文に適用できる新しい属性が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d4847-211">ASP.NET MVC 2 includes new attributes that you can apply to action methods and that feature compact syntax.</span></span> <span data-ttu-id="d4847-212">これらの属性には、ASP.NET MVC を HTTP 動詞に基づいてアクション メソッドの選択が有効にします。</span><span class="sxs-lookup"><span data-stu-id="d4847-212">These attributes enable ASP.NET MVC to select an action method based on the HTTP verb.</span></span> <span data-ttu-id="d4847-213">次の例で POST 要求が最初のアクション メソッドを呼び出すし、PUT 要求では、2 つ目のアクション メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d4847-213">In the following example, a POST request will call the first action method and a PUT request will call the second action method.</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

<span data-ttu-id="d4847-214">以前のバージョンの ASP.NET MVC では、これらのアクション メソッドでは、次の例で示すようにより詳細な構文が必要です。</span><span class="sxs-lookup"><span data-stu-id="d4847-214">In earlier versions of ASP.NET MVC, these action methods required more verbose syntax, as shown in the following example:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

<span data-ttu-id="d4847-215">ブラウザー GET と POST の HTTP 動詞のみサポートされているために、別の動詞が必要なアクションに投稿することはできません。</span><span class="sxs-lookup"><span data-stu-id="d4847-215">Because browsers support only the GET and POST HTTP verbs, it is not possible to post to an action that requires a different verb.</span></span> <span data-ttu-id="d4847-216">したがって rest ベースのすべての要求をネイティブにサポートすることはできません。</span><span class="sxs-lookup"><span data-stu-id="d4847-216">Thus it is not possible to natively support all RESTful requests.</span></span>

<span data-ttu-id="d4847-217">ただし、POST 操作、ASP.NET MVC 2 進行中の要求は、RESTful をサポートするために、新しい HttpMethodOverride HTML ヘルパー メソッドを紹介します。</span><span class="sxs-lookup"><span data-stu-id="d4847-217">However, to support RESTful requests during POST operations, ASP.NET MVC 2 introduces a new HttpMethodOverride HTML helper method.</span></span> <span data-ttu-id="d4847-218">このメソッドは、フォームが任意の HTTP メソッドを効果的にエミュレートするために非表示の input 要素を表示します。</span><span class="sxs-lookup"><span data-stu-id="d4847-218">This method renders a hidden input element that causes the form to effectively emulate any HTTP method.</span></span> <span data-ttu-id="d4847-219">たとえば、HttpMethodOverride HTML ヘルパー メソッドを使用すると、することが表示されるフォームの送信 PUT、または DELETE 要求であります。</span><span class="sxs-lookup"><span data-stu-id="d4847-219">For example, by using the HttpMethodOverride HTML helper method, you can have a form submission appear be a PUT or DELETE request.</span></span> <span data-ttu-id="d4847-220">HttpMethodOverride の動作では、次の属性に影響します。</span><span class="sxs-lookup"><span data-stu-id="d4847-220">The behavior of HttpMethodOverride affects the following attributes:</span></span>

- <span data-ttu-id="d4847-221">HttpPostAttribute</span><span class="sxs-lookup"><span data-stu-id="d4847-221">HttpPostAttribute</span></span>
- <span data-ttu-id="d4847-222">HttpPutAttribute</span><span class="sxs-lookup"><span data-stu-id="d4847-222">HttpPutAttribute</span></span>
- <span data-ttu-id="d4847-223">HttpGetAttribute</span><span class="sxs-lookup"><span data-stu-id="d4847-223">HttpGetAttribute</span></span>
- <span data-ttu-id="d4847-224">HttpDeleteAttribute</span><span class="sxs-lookup"><span data-stu-id="d4847-224">HttpDeleteAttribute</span></span>
- <span data-ttu-id="d4847-225">AcceptVerbsAttribute</span><span class="sxs-lookup"><span data-stu-id="d4847-225">AcceptVerbsAttribute</span></span>

<span data-ttu-id="d4847-226">非表示の input 要素は、X HTTP メソッド オーバーライドの名前とその値をエミュレートするために、HTTP 動詞に設定します。</span><span class="sxs-lookup"><span data-stu-id="d4847-226">The hidden input element has its name X-HTTP-Method-Override and its value set to the HTTP verb to emulate.</span></span> <span data-ttu-id="d4847-227">名前/値のペアとして、HTTP ヘッダー内、またはクエリ文字列の値で上書き値を指定もできます。</span><span class="sxs-lookup"><span data-stu-id="d4847-227">The override value can also be specified in an HTTP header or in a query string value as a name/value pair.</span></span>

<span data-ttu-id="d4847-228">オーバーライドは、実際の要求が POST 要求である場合にのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="d4847-228">The override can only be used when the real request is a POST request.</span></span> <span data-ttu-id="d4847-229">その他の HTTP 動詞を使用する要求のオーバーライド値が無視されます。</span><span class="sxs-lookup"><span data-stu-id="d4847-229">The override value will be ignored for requests that use any other HTTP verb.</span></span>

### <a id="_TOC3_13"></a><span data-ttu-id="d4847-230">テンプレート化されたヘルパーの新しい HiddenInputAttribute クラス</span><span class="sxs-lookup"><span data-stu-id="d4847-230">New HiddenInputAttribute Class for Templated Helpers</span></span>

<span data-ttu-id="d4847-231">エディター テンプレートで、モデルを表示するときに、非表示の input 要素を表示するかどうかを示すためにモデル プロパティには、新しい HiddenInputAttribute 属性を適用できます。</span><span class="sxs-lookup"><span data-stu-id="d4847-231">You can apply the new HiddenInputAttribute attribute to a model property to indicate whether a hidden input element should be rendered when displaying the model in an editor template.</span></span> <span data-ttu-id="d4847-232">(属性は、HiddenInput の暗黙の UIHint 値を設定します。)</span><span class="sxs-lookup"><span data-stu-id="d4847-232">(The attribute sets an implicit UIHint value of HiddenInput).</span></span> <span data-ttu-id="d4847-233">属性の値のプロパティでは、エディターで、値が表示されるかどうかを指定し、モードを表示できます。</span><span class="sxs-lookup"><span data-stu-id="d4847-233">The attribute's DisplayValue property lets you specify whether the value is displayed in editor and display modes.</span></span> <span data-ttu-id="d4847-234">何も表示値が false に設定されている場合、HTML マークアップを通常のフィールドを囲むであってもです。</span><span class="sxs-lookup"><span data-stu-id="d4847-234">When DisplayValue is set to false, nothing is displayed, not even the HTML markup that normally surrounds a field.</span></span> <span data-ttu-id="d4847-235">値の既定値は true です。</span><span class="sxs-lookup"><span data-stu-id="d4847-235">The default value for DisplayValue is true.</span></span>

<span data-ttu-id="d4847-236">次のシナリオで HiddenInputAttribute 属性を使用する場合があります。</span><span class="sxs-lookup"><span data-stu-id="d4847-236">You might use HiddenInputAttribute attribute in the following scenarios:</span></span>

- <span data-ttu-id="d4847-237">ユーザー オブジェクトの ID を編集できるように、ビューとコント ローラーに渡すことができるように、古い ID を格納する非表示の input 要素を提供するためにも値を表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4847-237">When a view lets users edit the ID of an object and it is necessary to display the value as well as to provide a hidden input element that contains the old ID so that it can be passed back to the controller.</span></span>
- <span data-ttu-id="d4847-238">ビューにより、ユーザーはタイムスタンプ プロパティなど、表示されることのあるバイナリ プロパティを編集します。</span><span class="sxs-lookup"><span data-stu-id="d4847-238">When a view lets users edit a binary property that should never be displayed, such as a timestamp property.</span></span> <span data-ttu-id="d4847-239">その場合は、値とラベルと値の場合) などの周囲の HTML マークアップは表示されません。</span><span class="sxs-lookup"><span data-stu-id="d4847-239">In that case, the value and surrounding HTML markup (such as the label and value) are not displayed.</span></span>

<span data-ttu-id="d4847-240">次の例では、HiddenInputAttribute クラスを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d4847-240">The following example shows how to use the HiddenInputAttribute class.</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

<span data-ttu-id="d4847-241">属性が設定されている場合に true (またはパラメーターが指定されていない)、次のように。</span><span class="sxs-lookup"><span data-stu-id="d4847-241">When the attribute is set to true (or no parameter is specified), the following occurs:</span></span>

- <span data-ttu-id="d4847-242">表示テンプレートでラベルを表示し、値は、ユーザーに表示します。</span><span class="sxs-lookup"><span data-stu-id="d4847-242">In display templates, a label is rendered and the value is displayed to the user.</span></span>
- <span data-ttu-id="d4847-243">エディター テンプレートで、ラベルが表示され、非表示の input 要素で、値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d4847-243">In editor templates, a label is rendered and the value is rendered in a hidden input element.</span></span>

<span data-ttu-id="d4847-244">属性が false に設定されている場合、以下の処理が行われます。</span><span class="sxs-lookup"><span data-stu-id="d4847-244">When the attribute is set to false, the following occurs:</span></span>

- <span data-ttu-id="d4847-245">そのフィールドに対しては何も行われませんが、画面テンプレートでレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="d4847-245">In display templates, nothing is rendered for that field.</span></span>
- <span data-ttu-id="d4847-246">エディター テンプレートでラベルは表示されませんし、値は、非表示の input 要素で表示されます。</span><span class="sxs-lookup"><span data-stu-id="d4847-246">In editor templates, no label is rendered and the value is rendered in a hidden input element.</span></span>

### <a id="_TOC3_14"></a><span data-ttu-id="d4847-247">Html.ValidationSummary ヘルパー メソッドはモデル レベルのエラーを表示できます。</span><span class="sxs-lookup"><span data-stu-id="d4847-247">Html.ValidationSummary Helper Method Can Display Model-Level Errors</span></span>

<span data-ttu-id="d4847-248">Html.ValidationSummary ヘルパー メソッドではすべての検証エラーを常に表示するには、代わりに、モデル レベル エラーのみを表示する新しいオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="d4847-248">Instead of always displaying all validation errors, the Html.ValidationSummary helper method has a new option to display only model-level errors.</span></span> <span data-ttu-id="d4847-249">これにより、検証概要に表示されるモデル レベル エラーおよび各フィールドの横に表示されるフィールド固有のエラーです。</span><span class="sxs-lookup"><span data-stu-id="d4847-249">This enables model-level errors to be displayed in the validation summary and field-specific errors to be displayed next to each field.</span></span>

### <a id="_TOC3_15"></a><span data-ttu-id="d4847-250">Visual Studio の生成コードに固有では T4 テンプレートを .NET Framework のターゲット バージョン</span><span class="sxs-lookup"><span data-stu-id="d4847-250">T4 Templates in Visual Studio Generate Code that is Specific to the Target Version of the .NET Framework</span></span>

<span data-ttu-id="d4847-251">新しいプロパティが使用可能な T4 ファイルを ASP.NET MVC の T4 ホスト アプリケーションによって使用される .NET Framework のバージョンを指定するからです。</span><span class="sxs-lookup"><span data-stu-id="d4847-251">A new property is available to T4 files from the ASP.NET MVC T4 host that specifies the version of the .NET Framework that is used by the application.</span></span> <span data-ttu-id="d4847-252">これにより、T4 テンプレート コードとは、.NET Framework のバージョンに固有のマークアップを生成できます。</span><span class="sxs-lookup"><span data-stu-id="d4847-252">This enables T4 templates to generate code and markup that is specific to a version of the .NET Framework.</span></span> <span data-ttu-id="d4847-253">Visual Studio 2008 では、値と、.NET 3.5 では常にします。</span><span class="sxs-lookup"><span data-stu-id="d4847-253">In Visual Studio 2008, the value is always .NET 3.5.</span></span> <span data-ttu-id="d4847-254">Visual Studio 2010 では、値は、.NET 3.5 または .NET 4 のいずれかがします。</span><span class="sxs-lookup"><span data-stu-id="d4847-254">In Visual Studio 2010, the value is either .NET 3.5 or .NET 4.</span></span>

## <a id="_TOC4"></a><span data-ttu-id="d4847-255">API の機能強化</span><span class="sxs-lookup"><span data-stu-id="d4847-255">API Improvements</span></span>

<span data-ttu-id="d4847-256">このセクションでは、既存の ASP.NET MVC 型とメンバーへの変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="d4847-256">This section describes changes to existing ASP.NET MVC types and members.</span></span>

- <span data-ttu-id="d4847-257">コント ローラー クラスにプロテクト仮想 CreateActionInvoker メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="d4847-257">Added a protected virtual CreateActionInvoker method in the Controller class.</span></span> <span data-ttu-id="d4847-258">このメソッドは、コント ローラーの ActionInvoker プロパティによって呼び出され、呼び出し元が既に設定されていない場合、呼び出し元の限定的なインスタンス化できます。</span><span class="sxs-lookup"><span data-stu-id="d4847-258">This method is invoked by the ActionInvoker property of Controller and allows for lazy instantiation of the invoker if no invoker is already set.</span></span>
- <span data-ttu-id="d4847-259">AuthorizeAttribute クラスにプロテクト仮想 HandleUnauthorizedRequest メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="d4847-259">Added a protected virtual HandleUnauthorizedRequest method in the AuthorizeAttribute class.</span></span> <span data-ttu-id="d4847-260">これにより、承認に失敗したときに、動作を制御する AuthorizeAttribute から派生するフィルターが有効です。</span><span class="sxs-lookup"><span data-stu-id="d4847-260">This enables filters that derive from AuthorizeAttribute to control the behavior when authorization fails.</span></span>
- <span data-ttu-id="d4847-261">ValueProviderDictionary クラスで、追加 (文字列キー、オブジェクトの値) のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="d4847-261">Added an Add(string key, object value) method in the ValueProviderDictionary class.</span></span> <span data-ttu-id="d4847-262">これにより、次の例のように ValueProviderDictionary に辞書初期化子構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="d4847-262">This enables you to use the dictionary initializer syntax for ValueProviderDictionary, as in the following example:</span></span>

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- <span data-ttu-id="d4847-263">追加の get\_Sys.Mvc.AjaxContext クラスのメソッドのオブジェクトします。</span><span class="sxs-lookup"><span data-stu-id="d4847-263">Added a get\_object method in the Sys.Mvc.AjaxContext class.</span></span> <span data-ttu-id="d4847-264">これは JavaScript メソッド get に類似した\_場合、応答のコンテンツ タイプは application/json には、データのメソッドを取得\_JSON オブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="d4847-264">This is a JavaScript method that is similar to the get\_data method, but if the content type of the response is application/json, get\_object returns the JSON object.</span></span>
- <span data-ttu-id="d4847-265">AuthorizationContext クラスで ActionDescriptor プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="d4847-265">Added an ActionDescriptor property in the AuthorizationContext class.</span></span>
- <span data-ttu-id="d4847-266">問題を回避するプロパティがない場合に ID プロパティが含まれるモデルをバインドするときに使用できる UrlParameter.Optional トークンを追加、フォーム ポストにします。</span><span class="sxs-lookup"><span data-stu-id="d4847-266">Added a UrlParameter.Optional token that can be used to work around problems when binding to a model that contains an ID property when the property is absent in a form post.</span></span> <span data-ttu-id="d4847-267">詳細については、エントリを参照してください。 [ASP.NET MVC 2 省略可能な URL パラメーター](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) Phil Haack のブログです。</span><span class="sxs-lookup"><span data-stu-id="d4847-267">For more detail, see the entry [ASP.NET MVC 2 Optional URL Parameters](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) on Phil Haack's blog.</span></span>

## <a id="_TOC5"></a><span data-ttu-id="d4847-268">重大な変更</span><span class="sxs-lookup"><span data-stu-id="d4847-268">Breaking Changes</span></span>

<span data-ttu-id="d4847-269">次の変更は、既存の ASP.NET MVC 1.0 アプリケーションでエラーを発生させる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d4847-269">The following changes might cause errors in existing ASP.NET MVC 1.0 applications.</span></span>

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a><span data-ttu-id="d4847-270">IDataErrorInfo を実装するクラスのプロパティの検証の動作の変更します。</span><span class="sxs-lookup"><span data-stu-id="d4847-270">Change in property validation behavior for classes that implement IDataErrorInfo</span></span>

<span data-ttu-id="d4847-271">IDataErrorInfo を使用して検証を実行するモデル オブジェクトの場合は、新しい値が設定されているかどうかに関係なく、すべてのプロパティを検証します。</span><span class="sxs-lookup"><span data-stu-id="d4847-271">For model objects that use IDataErrorInfo to perform validation, every property is validated, regardless of whether a new value was set.</span></span> <span data-ttu-id="d4847-272">ASP.NET MVC 1.0 では、設定した新しい値のあるプロパティのみが検証されました。</span><span class="sxs-lookup"><span data-stu-id="d4847-272">In ASP.NET MVC 1.0, only properties that had new values set were validated.</span></span> <span data-ttu-id="d4847-273">ASP.NET MVC 2 では、すべてのプロパティの検証コントロールが成功した場合にのみ IDataErrorInfo のエラーのプロパティが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="d4847-273">In ASP.NET MVC 2, the Error property of IDataErrorInfo is called only if all the property validators were successful.</span></span>

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a><span data-ttu-id="d4847-274">IIS スクリプト マッピングのスクリプトは、インストーラーで使用できなく</span><span class="sxs-lookup"><span data-stu-id="d4847-274">IIS script mapping script is no longer available in the installer</span></span>

<span data-ttu-id="d4847-275">IIS スクリプト マッピング スクリプトは、クラシック モードで IIS 6 と IIS 7 のスクリプト マップを構成に使用されるコマンド ライン スクリプトです。</span><span class="sxs-lookup"><span data-stu-id="d4847-275">The IIS script-mapping script is a command-line script that is used to configure script maps for IIS 6 and for IIS 7 in Classic mode.</span></span> <span data-ttu-id="d4847-276">スクリプト マッピング スクリプトには、Visual Studio 開発サーバーを使用する場合、または統合モードで IIS 7 を使用する場合は不要です。</span><span class="sxs-lookup"><span data-stu-id="d4847-276">The script-mapping script is not needed if you use the Visual Studio Development Server or if you use IIS 7 in Integrated mode.</span></span> <span data-ttu-id="d4847-277">スクリプトは、個別にサポートされていないダウンロードで使用可能な[ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack)です。</span><span class="sxs-lookup"><span data-stu-id="d4847-277">The scripts are available as a separate unsupported download on the [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).</span></span>

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a><span data-ttu-id="d4847-278">MVC フューチャで Html.Substitute ヘルパー メソッドは現在利用できません。</span><span class="sxs-lookup"><span data-stu-id="d4847-278">The Html.Substitute helper method in MVC Futures is no longer available</span></span>

<span data-ttu-id="d4847-279">MVC ビュー エンジンのレンダリング動作の変更により、Html.Substitute ヘルパー メソッドは機能しません、削除されました。</span><span class="sxs-lookup"><span data-stu-id="d4847-279">Due to changes in the rendering behavior of MVC view engines, the Html.Substitute helper method does not work and has been removed.</span></span>

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a><span data-ttu-id="d4847-280">IValueProvider インターフェイスは、IDictionary のすべての使用を置き換えます</span><span class="sxs-lookup"><span data-stu-id="d4847-280">The IValueProvider interface replaces all uses of IDictionary</span></span>

<span data-ttu-id="d4847-281">MVC 1.0 では IDictionary を今すぐ受け入れすべてプロパティまたはメソッドの引数は、IValueProvider を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="d4847-281">Every property or method argument that accepted IDictionary in MVC 1.0 now accepts IValueProvider.</span></span> <span data-ttu-id="d4847-282">この変更では、カスタム値プロバイダーまたはカスタム モデル バインダーを含むアプリケーションのみに影響します。</span><span class="sxs-lookup"><span data-stu-id="d4847-282">This change affects only applications that include custom value providers or custom model binders.</span></span> <span data-ttu-id="d4847-283">プロパティと、この変更によって影響を受ける方法の例については、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d4847-283">Examples of properties and methods that are affected by this change include the following:</span></span>

- <span data-ttu-id="d4847-284">ControllerBase および ModelBindingContext クラスの ValueProvider プロパティです。</span><span class="sxs-lookup"><span data-stu-id="d4847-284">The ValueProvider property of the ControllerBase and ModelBindingContext classes.</span></span>
- <span data-ttu-id="d4847-285">コント ローラー クラスの TryUpdateModel メソッドです。</span><span class="sxs-lookup"><span data-stu-id="d4847-285">The TryUpdateModel methods of the Controller class.</span></span>

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a><span data-ttu-id="d4847-286">Site.css ファイルで追加された新しい CSS クラス</span><span class="sxs-lookup"><span data-stu-id="d4847-286">New CSS classes were added in the Site.css file</span></span>

<span data-ttu-id="d4847-287">テンプレート化されたヘルパーと、検証機能で使用される新しいスタイルを含めます、ASP.NET MVC プロジェクト テンプレートで Site.css ファイルが更新されました。</span><span class="sxs-lookup"><span data-stu-id="d4847-287">The Site.css file in the ASP.NET MVC project templates has been updated to include new styles used by the validation functionality and by the templated helpers.</span></span>

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a><span data-ttu-id="d4847-288">ヘルパーが MvcHtmlString オブジェクトを返すようになりました</span><span class="sxs-lookup"><span data-stu-id="d4847-288">Helpers now return an MvcHtmlString object</span></span>

<span data-ttu-id="d4847-289">ASP.NET 4 で、新しい HTML エンコーディング式の構文を利用するために HTML ヘルパーの戻り値の型は今すぐ MvcHtmlString ではなく文字列です。</span><span class="sxs-lookup"><span data-stu-id="d4847-289">In order to take advantage of the new HTML-encoding expression syntax in ASP.NET 4, the return type for HTML helpers is now MvcHtmlString instead of a string.</span></span> <span data-ttu-id="d4847-290">HTML エンコードの構文を活用できませんする ASP.NET 3.5 に ASP.NET MVC 2 と新しいヘルパーを使用する場合新しい構文は、ASP.NET 4 で ASP.NET MVC 2 を実行する場合にのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="d4847-290">If you use ASP.NET MVC 2 and the new helpers on ASP.NET 3.5, you will not be able to take advantage of the HTML-encoding syntax; the new syntax is available only when you run ASP.NET MVC 2 on ASP.NET 4.</span></span>

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a><span data-ttu-id="d4847-291">JsonResult は、HTTP POST 要求にのみ応答します。</span><span class="sxs-lookup"><span data-stu-id="d4847-291">JsonResult now responds only to HTTP POST requests</span></span>

<span data-ttu-id="d4847-292">既定では、情報漏えいの可能性のある攻撃をハイジャックしている JSON を軽減するために JsonResult クラスは、今すぐ HTTP POST 要求にのみ応答します。</span><span class="sxs-lookup"><span data-stu-id="d4847-292">In order to mitigate JSON hijacking attacks that have the potential for information disclosure, by default, the JsonResult class now responds only to HTTP POST requests.</span></span> <span data-ttu-id="d4847-293">代わりに POST を使用するには、Ajax GET JsonResult オブジェクトを返すアクション メソッドの呼び出しを変更してください。</span><span class="sxs-lookup"><span data-stu-id="d4847-293">Ajax GET calls to action methods that return a JsonResult object should be changed to use POST instead.</span></span> <span data-ttu-id="d4847-294">必要に応じて、JsonResult の新しい JsonRequestBehavior プロパティを設定して動作をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="d4847-294">If necessary, you can override this behavior by setting the new JsonRequestBehavior property of JsonResult.</span></span> <span data-ttu-id="d4847-295">潜在的な脆弱性の詳細については、ブログの投稿を参照してください。 [JSON をハイジャックしている](http://haacked.com/archive/2009/06/25/json-hijacking.aspx)Phil Haack のブログです。</span><span class="sxs-lookup"><span data-stu-id="d4847-295">For more information about the potential exploit, see the blog post [JSON Hijacking](http://haacked.com/archive/2009/06/25/json-hijacking.aspx) on Phil Haack's blog.</span></span>

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a><span data-ttu-id="d4847-296">モデルおよび ModelBindingContext の ModelType プロパティ set アクセス操作子が今後使用しません</span><span class="sxs-lookup"><span data-stu-id="d4847-296">Model and ModelType property setters on ModelBindingContext are obsolete</span></span>

<span data-ttu-id="d4847-297">新しい設定可能な ModelMetadata プロパティは ModelBindingContext クラスに追加されました。</span><span class="sxs-lookup"><span data-stu-id="d4847-297">A new settable ModelMetadata property has been added to the ModelBindingContext class.</span></span> <span data-ttu-id="d4847-298">新しいプロパティには、モデルと ModelType プロパティの両方がカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="d4847-298">The new property encapsulates both the Model and the ModelType properties.</span></span> <span data-ttu-id="d4847-299">モデルと ModelType プロパティは今後使用しませんが、旧バージョンとの互換性のためプロパティ get アクセス操作子も機能です。ModelMetadata プロパティに委任するには値を取得します。</span><span class="sxs-lookup"><span data-stu-id="d4847-299">Although the Model and ModelType properties are obsolete, for backward compatibility the property getters still work; they delegate to the ModelMetadata property to retrieve the value.</span></span>

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a><span data-ttu-id="d4847-300">DefaultControllerFactory クラスへの変更は、そこから派生するカスタム コント ローラー ファクトリを中断します。</span><span class="sxs-lookup"><span data-stu-id="d4847-300">Changes to the DefaultControllerFactory class break custom controller factories that derive from it</span></span>

<span data-ttu-id="d4847-301">DefaultControllerFactory クラスは、RequestContext プロパティを削除することによって修正されました。</span><span class="sxs-lookup"><span data-stu-id="d4847-301">The DefaultControllerFactory class was fixed by removing the RequestContext property.</span></span> <span data-ttu-id="d4847-302">このプロパティは、代わりに、要求コンテキストのインスタンスは、保護された仮想 GetControllerInstance と GetControllerType メソッドに渡されます。</span><span class="sxs-lookup"><span data-stu-id="d4847-302">In place of this property, the request context instance is passed to the protected virtual GetControllerInstance and GetControllerType methods.</span></span> <span data-ttu-id="d4847-303">この変更では、DefaultControllerFactory から派生したカスタム コント ローラー ファクトリに影響します。</span><span class="sxs-lookup"><span data-stu-id="d4847-303">This change affects custom controller factories that derive from DefaultControllerFactory.</span></span>

<span data-ttu-id="d4847-304">ASP.NET MVC の依存関係の挿入をアプリケーションに提供するカスタム コント ローラー ファクトリが使用されます。</span><span class="sxs-lookup"><span data-stu-id="d4847-304">Custom controller factories are often used to provide dependency injection for ASP.NET MVC applications.</span></span> <span data-ttu-id="d4847-305">ASP.NET MVC 2 をサポートするためにカスタム コント ローラー ファクトリを更新するには、メソッドのシグネチャまたは新規のシグネチャと一致する署名を変更し、プロパティではなく、要求コンテキスト パラメーターを使用します。</span><span class="sxs-lookup"><span data-stu-id="d4847-305">To update the custom controller factories to support ASP.NET MVC 2, change the method signature or signatures to match the new signatures, and use the request context parameter instead of the property.</span></span>

#### <a name="area-is-a-now-a-reserved-route-value-key"></a><span data-ttu-id="d4847-306">「区分」予約済みのルート値のキーを今すぐには</span><span class="sxs-lookup"><span data-stu-id="d4847-306">"Area" is a now a reserved route-value key</span></span>

<span data-ttu-id="d4847-307">ルートの値の文字列「領域」ようになりました特別な意味 ASP.NET mvc で"controller"と"action"を実行するのと同じようにします。</span><span class="sxs-lookup"><span data-stu-id="d4847-307">The string "area" in Route values now has special meaning in ASP.NET MVC, in the same way that "controller" and "action" do.</span></span> <span data-ttu-id="d4847-308">1 つは、HTML ヘルパーは、「区分」を含むルート値ディクショナリで提供されている、している場合、ヘルパーはクエリ文字列に「領域」を不要になったに追加するされます。</span><span class="sxs-lookup"><span data-stu-id="d4847-308">One implication is that if HTML helpers are supplied with a route-value dictionary containing "area", the helpers will no longer append "area" in the query string.</span></span>

<span data-ttu-id="d4847-309">領域の機能を使用している場合は、ルートの URL の一部として {領域} を使用することを確認してください。</span><span class="sxs-lookup"><span data-stu-id="d4847-309">If you are using the Areas feature, make sure to not use {area} as part of your route URL.</span></span>


## <a id="_TOC6"></a><span data-ttu-id="d4847-310">免責事項</span><span class="sxs-lookup"><span data-stu-id="d4847-310">Disclaimer</span></span>

<span data-ttu-id="d4847-311">このドキュメントは暫定版であり、ここで説明するソフトウェアの最終的な商業リリースより前に大幅に変更される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d4847-311">This is a preliminary document and may be changed substantially prior to final commercial release of the software described herein.</span></span>

<span data-ttu-id="d4847-312">このドキュメントに記載されている情報は、説明されている問題に関する Microsoft Corporation の発行日時点における最新の見解を表します。</span><span class="sxs-lookup"><span data-stu-id="d4847-312">The information contained in this document represents the current view of Microsoft Corporation on the issues discussed as of the date of publication.</span></span> <span data-ttu-id="d4847-313">マイクロソフトは変化する市場の状況に対応する必要があるため、本ドキュメントをマイクロソフトの確約と解釈することはできず、発行日以降は、提示されている情報の正確性を保証できません。</span><span class="sxs-lookup"><span data-stu-id="d4847-313">Because Microsoft must respond to changing market conditions, it should not be interpreted to be a commitment on the part of Microsoft, and Microsoft cannot guarantee the accuracy of any information presented after the date of publication.</span></span>

<span data-ttu-id="d4847-314">このホワイト ペーパーは、情報提供のみを目的としています。</span><span class="sxs-lookup"><span data-stu-id="d4847-314">This White Paper is for informational purposes only.</span></span> <span data-ttu-id="d4847-315">マイクロソフトは、このドキュメント内の情報に関して、明示的、暗黙的、または法的ないかなる保証も行いません。</span><span class="sxs-lookup"><span data-stu-id="d4847-315">MICROSOFT MAKES NO WARRANTIES, EXPRESS, IMPLIED OR STATUTORY, AS TO THE INFORMATION IN THIS DOCUMENT.</span></span>

<span data-ttu-id="d4847-316">お客様ご自身の責任において、適用されるすべての著作権関連法規に従ったご使用を願います。</span><span class="sxs-lookup"><span data-stu-id="d4847-316">Complying with all applicable copyright laws is the responsibility of the user.</span></span> <span data-ttu-id="d4847-317">このドキュメントのいかなる部分も、米国 Microsoft Corporation の書面による許諾を受けることなく、その目的を問わず、どのような形態であっても、複製または譲渡することは禁じられています。ここでいう形態とは、複写や記録など、電子的な、または物理的なすべての手段を含みます。ただしこれは、著作権法上のお客様の権利を制限するものではありません。</span><span class="sxs-lookup"><span data-stu-id="d4847-317">Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.</span></span>

<span data-ttu-id="d4847-318">マイクロソフトは、このドキュメントに記載されている内容に関し、特許、特許申請、商標、著作権、またはその他の無体財産権を有する場合があります。</span><span class="sxs-lookup"><span data-stu-id="d4847-318">Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document.</span></span> <span data-ttu-id="d4847-319">別途マイクロソフトのライセンス契約上に明示の規定のない限り、このドキュメントはこれらの特許、商標、著作権、またはその他の無体財産権に関する権利をお客様に許諾するものではありません。</span><span class="sxs-lookup"><span data-stu-id="d4847-319">Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.</span></span>

<span data-ttu-id="d4847-320">特に明記されない限り、使用している会社、組織、製品、ドメイン名、電子メール アドレス、ロゴ、人物、場所、およびイベント実在は、架空のもの、および関連会社、組織、製品、ドメイン名、電子メールをアドレス、ロゴ、人物、場所またはイベントはまたは意図して推論する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4847-320">Unless otherwise noted, the example companies, organizations, products, domain names, email addresses, logos, people, places and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, email address, logo, person, place or event is intended or should be inferred.</span></span>

<span data-ttu-id="d4847-321">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="d4847-321">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="d4847-322">All rights reserved.</span><span class="sxs-lookup"><span data-stu-id="d4847-322">All rights reserved.</span></span>

<span data-ttu-id="d4847-323">Microsoft および Windows は、米国および他の国における Microsoft Corporation の登録商標または商標です。</span><span class="sxs-lookup"><span data-stu-id="d4847-323">Microsoft and Windows are either registered trademarks or trademarks of Microsoft Corporation in the United States and/or other countries.</span></span>

<span data-ttu-id="d4847-324">本ドキュメントで説明する実際の製造元および製品名は、それぞれの所有者の商標である場合があります。</span><span class="sxs-lookup"><span data-stu-id="d4847-324">The names of actual companies and products mentioned herein may be the trademarks of their respective owners.</span></span>
