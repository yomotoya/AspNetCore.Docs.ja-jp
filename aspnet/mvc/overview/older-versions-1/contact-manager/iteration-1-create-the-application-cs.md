---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
title: '繰り返し #1 – アプリケーションを作成する (c#) |Microsoft Docs'
author: microsoft
description: '最初のイテレーションを作成、連絡先マネージャー最も簡単な方法で考えられるします。 基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および D.'
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: db0f160b-901c-46d3-865e-7ab6cd4ed68d
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
msc.type: authoredcontent
ms.openlocfilehash: 0d756541d2dc911154afb427e52eb5cfdd5d59b4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821363"
---
<a name="iteration-1--create-the-application-c"></a><span data-ttu-id="49406-104">繰り返し #1 – アプリケーションを作成する (c#)</span><span class="sxs-lookup"><span data-stu-id="49406-104">Iteration #1 – Create the Application (C#)</span></span>
====================
<span data-ttu-id="49406-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="49406-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="49406-106">コードをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="49406-106">Download Code</span></span>](iteration-1-create-the-application-cs/_static/contactmanager_1_cs1.zip)

> <span data-ttu-id="49406-107">最初のイテレーションを作成、連絡先マネージャー最も簡単な方法で考えられるします。</span><span class="sxs-lookup"><span data-stu-id="49406-107">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="49406-108">基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。</span><span class="sxs-lookup"><span data-stu-id="49406-108">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a><span data-ttu-id="49406-109">ASP.NET MVC 連絡先管理アプリケーション (VB) の構築</span><span class="sxs-lookup"><span data-stu-id="49406-109">Building a Contact Management ASP.NET MVC Application (VB)</span></span>

<span data-ttu-id="49406-110">このチュートリアル シリーズでは、開始から終了に全体連絡先管理アプリケーションを構築します。</span><span class="sxs-lookup"><span data-stu-id="49406-110">In this series of tutorials, we build an entire Contact Management application from start to finish.</span></span> <span data-ttu-id="49406-111">Contact Manager アプリケーションは、ユーザーの一覧については店舗連絡先情報の名前、電話番号、電子メール アドレスにするようにことができます。</span><span class="sxs-lookup"><span data-stu-id="49406-111">The Contact Manager application enables you to store contact information - names, phone numbers and email addresses - for a list of people.</span></span>

<span data-ttu-id="49406-112">複数のイテレーションにおける、アプリケーションを構築します。</span><span class="sxs-lookup"><span data-stu-id="49406-112">We build the application over multiple iterations.</span></span> <span data-ttu-id="49406-113">反復処理ごとに、アプリケーション徐々 に向上します。</span><span class="sxs-lookup"><span data-stu-id="49406-113">With each iteration, we gradually improve the application.</span></span> <span data-ttu-id="49406-114">この複数のイテレーションのアプローチの目的は、各変更の理由を理解するためです。</span><span class="sxs-lookup"><span data-stu-id="49406-114">The goal of this multiple iteration approach is to enable you to understand the reason for each change.</span></span>

- <span data-ttu-id="49406-115">繰り返し #1 - は、アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="49406-115">Iteration #1 - Create the application.</span></span> <span data-ttu-id="49406-116">最初のイテレーションを作成、連絡先マネージャー最も簡単な方法で考えられるします。</span><span class="sxs-lookup"><span data-stu-id="49406-116">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="49406-117">基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。</span><span class="sxs-lookup"><span data-stu-id="49406-117">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>

- <span data-ttu-id="49406-118">繰り返し #2 - は、素敵に見えるアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="49406-118">Iteration #2 - Make the application look nice.</span></span> <span data-ttu-id="49406-119">このイテレーションで、アプリケーションの見た目を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。</span><span class="sxs-lookup"><span data-stu-id="49406-119">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>

- <span data-ttu-id="49406-120">繰り返し #3 - フォーム検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="49406-120">Iteration #3 - Add form validation.</span></span> <span data-ttu-id="49406-121">3 番目のイテレーションでは、基本的なフォーム検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="49406-121">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="49406-122">ユーザーは、必要なフォームのフィールドを完了しなくても、フォームを送信できないようにようにします。</span><span class="sxs-lookup"><span data-stu-id="49406-122">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="49406-123">また、電子メール アドレスと電話番号を検証しました。</span><span class="sxs-lookup"><span data-stu-id="49406-123">We also validate email addresses and phone numbers.</span></span>

- <span data-ttu-id="49406-124">繰り返し #4 - は、アプリケーションを疎結合を作成します。</span><span class="sxs-lookup"><span data-stu-id="49406-124">Iteration #4 - Make the application loosely coupled.</span></span> <span data-ttu-id="49406-125">この 3 番目のイテレーションで、保守し、Contact Manager アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかの利点を実行します。</span><span class="sxs-lookup"><span data-stu-id="49406-125">In this third iteration, we take advantage of several software design patterns to make it easier to maintain and modify the Contact Manager application.</span></span> <span data-ttu-id="49406-126">たとえば、Repository パターンと依存関係の注入パターンを使用するようにアプリケーションをリファクタリングします。</span><span class="sxs-lookup"><span data-stu-id="49406-126">For example, we refactor our application to use the Repository pattern and the Dependency Injection pattern.</span></span>

- <span data-ttu-id="49406-127">繰り返し #5 - 単体テストを作成します。</span><span class="sxs-lookup"><span data-stu-id="49406-127">Iteration #5 - Create unit tests.</span></span> <span data-ttu-id="49406-128">5 番目のイテレーションでアプリケーションと簡単に維持単体テストを追加して変更します。</span><span class="sxs-lookup"><span data-stu-id="49406-128">In the fifth iteration, we make our application easier to maintain and modify by adding unit tests.</span></span> <span data-ttu-id="49406-129">データ モデル クラスの模擬テストを実行し、コント ローラーと検証ロジックの単体テストをビルドします。</span><span class="sxs-lookup"><span data-stu-id="49406-129">We mock our data model classes and build unit tests for our controllers and validation logic.</span></span>

- <span data-ttu-id="49406-130">繰り返し #6 - は、テスト駆動開発を使用します。</span><span class="sxs-lookup"><span data-stu-id="49406-130">Iteration #6 - Use test-driven development.</span></span> <span data-ttu-id="49406-131">この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加には、まず単体テストの記述、単体テストに対してコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="49406-131">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="49406-132">このイテレーションは、連絡先グループを追加します。</span><span class="sxs-lookup"><span data-stu-id="49406-132">In this iteration, we add contact groups.</span></span>

- <span data-ttu-id="49406-133">繰り返し #7 - Ajax 機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="49406-133">Iteration #7 - Add Ajax functionality.</span></span> <span data-ttu-id="49406-134">7 番目のイテレーションで改良、応答性と、アプリケーションのパフォーマンスの Ajax のサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="49406-134">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>

## <a name="this-iteration"></a><span data-ttu-id="49406-135">このイテレーション</span><span class="sxs-lookup"><span data-stu-id="49406-135">This Iteration</span></span>

<span data-ttu-id="49406-136">この最初のイテレーションでは、基本的なアプリケーションを構築します。</span><span class="sxs-lookup"><span data-stu-id="49406-136">In this first iteration, we build the basic application.</span></span> <span data-ttu-id="49406-137">目標は、最も高速かつ最も簡単な方法で、連絡先マネージャーを作成します。</span><span class="sxs-lookup"><span data-stu-id="49406-137">The goal is to build the Contact Manager in the fastest and simplest way possible.</span></span> <span data-ttu-id="49406-138">後期のイテレーションでは、アプリケーションのデザインを改善します。</span><span class="sxs-lookup"><span data-stu-id="49406-138">In later iterations, we improve the design of the application.</span></span>

<span data-ttu-id="49406-139">Contact Manager アプリケーションとは、基本的なデータベース駆動型アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="49406-139">The Contact Manager application is a basic database-driven application.</span></span> <span data-ttu-id="49406-140">アプリケーションを使用して、新しい連絡先を作成、既存の連絡先を編集および連絡先を削除することができます。</span><span class="sxs-lookup"><span data-stu-id="49406-140">You can use the application to create new contacts, edit existing contacts, and delete contacts.</span></span>

<span data-ttu-id="49406-141">このイテレーションでは、次の手順を完了しました。</span><span class="sxs-lookup"><span data-stu-id="49406-141">In this iteration, we complete the following steps:</span></span>

1. <span data-ttu-id="49406-142">ASP.NET MVC アプリケーション</span><span class="sxs-lookup"><span data-stu-id="49406-142">ASP.NET MVC application</span></span>
2. <span data-ttu-id="49406-143">当社の連絡先を格納するデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="49406-143">Create a database to store our contacts</span></span>
3. <span data-ttu-id="49406-144">Microsoft Entity Framework で、データベースのモデル クラスを生成します。</span><span class="sxs-lookup"><span data-stu-id="49406-144">Generate a model class for our database with the Microsoft Entity Framework</span></span>
4. <span data-ttu-id="49406-145">コント ローラー アクションとのすべてのデータベースに連絡先の一覧を表示できるビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="49406-145">Create a controller action and view that enables us to list all of the contacts in the database</span></span>
5. <span data-ttu-id="49406-146">コント ローラー アクションと、データベースに新しい連絡先を作成することができるビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="49406-146">Create controller actions and a view that enables us to create a new contact in the database</span></span>
6. <span data-ttu-id="49406-147">コント ローラー アクションと、データベース内の既存の連絡先の編集可能なビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="49406-147">Create controller actions and a view that enables us to edit an existing contact in the database</span></span>
7. <span data-ttu-id="49406-148">コント ローラー アクションと、データベース内の既存の連絡先を削除することができるビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="49406-148">Create controller actions and a view that enables us to delete an existing contact in the database</span></span>

## <a name="software-prerequisites"></a><span data-ttu-id="49406-149">ソフトウェアの前提条件</span><span class="sxs-lookup"><span data-stu-id="49406-149">Software Prerequisites</span></span>

<span data-ttu-id="49406-150">ASP.NET MVC アプリケーションでは、Visual Studio 2008 または Visual Web Developer 2008 が (Visual Web Developer は、すべての Visual Studio の高度な機能を含まない Visual Studio の無料版が)、コンピューターにインストールされているいずれかが必要です。</span><span class="sxs-lookup"><span data-stu-id="49406-150">In ASP.NET MVC applications, you must have either Visual Studio 2008 or Visual Web Developer 2008 installed on your computer (Visual Web Developer is a free version of Visual Studio that does not include all of the advanced features of Visual Studio).</span></span> <span data-ttu-id="49406-151">Visual Studio 2008 の評価版または Visual Web Developer のいずれかは、次のアドレスからダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="49406-151">You can download either the trial version of Visual Studio 2008 or Visual Web Developer from the following address:</span></span>

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> <span data-ttu-id="49406-152">Visual Web Developer と ASP.NET MVC アプリケーションの場合は、Visual Web Developer Service Pack 1 インストールが必要です。</span><span class="sxs-lookup"><span data-stu-id="49406-152">For ASP.NET MVC applications with Visual Web Developer, you must have Visual Web Developer Service Pack 1 installed.</span></span> <span data-ttu-id="49406-153">Service Pack 1 では、Web アプリケーション プロジェクトを作成することはできません。</span><span class="sxs-lookup"><span data-stu-id="49406-153">Without Service Pack 1, you cannot create Web Application Projects.</span></span>


<span data-ttu-id="49406-154">ASP.NET MVC フレームワーク。</span><span class="sxs-lookup"><span data-stu-id="49406-154">ASP.NET MVC framework.</span></span> <span data-ttu-id="49406-155">ASP.NET MVC フレームワークは、次のアドレスからダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="49406-155">You can download the ASP.NET MVC framework from the following address:</span></span>

[https://www.asp.net/mvc](../../../index.md)

<span data-ttu-id="49406-156">このチュートリアルでは、データベースにアクセスするのに Microsoft Entity Framework を使用します。</span><span class="sxs-lookup"><span data-stu-id="49406-156">In this tutorial, we use the Microsoft Entity Framework to access a database.</span></span> <span data-ttu-id="49406-157">Entity Framework は .NET Framework 3.5 Service Pack 1 に含まれています。</span><span class="sxs-lookup"><span data-stu-id="49406-157">The Entity Framework is included with .NET Framework 3.5 Service Pack 1.</span></span> <span data-ttu-id="49406-158">このサービス パックは、次の場所からダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="49406-158">You can download this service pack from the following location:</span></span>

[<span data-ttu-id="49406-159">https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&ampdisplaylang = en</span><span class="sxs-lookup"><span data-stu-id="49406-159">https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en</span></span>](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

<span data-ttu-id="49406-160">これらのダウンロードを 1 つずつのそれぞれを実行する代わりに、Web Platform Installer (Web PI) の利点を実行できます。</span><span class="sxs-lookup"><span data-stu-id="49406-160">As an alternative to performing each of these downloads one by one, you can take advantage of the Web Platform Installer (Web PI).</span></span> <span data-ttu-id="49406-161">Web PI は、次のアドレスからダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="49406-161">You can download the Web PI from the following address:</span></span>

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a><span data-ttu-id="49406-162">ASP.NET MVC プロジェクト</span><span class="sxs-lookup"><span data-stu-id="49406-162">ASP.NET MVC Project</span></span>

<span data-ttu-id="49406-163">ASP.NET MVC Web アプリケーション プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="49406-163">ASP.NET MVC Web Application Project.</span></span> <span data-ttu-id="49406-164">Visual Studio を起動し、メニュー オプションを選択**ファイル]、[新しいプロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="49406-164">Launch Visual Studio and select the menu option **File, New Project**.</span></span> <span data-ttu-id="49406-165">**新しいプロジェクト**ダイアログでは、(図 1 参照) が表示されます。</span><span class="sxs-lookup"><span data-stu-id="49406-165">The **New Project** dialog appears (see Figure 1).</span></span> <span data-ttu-id="49406-166">選択、 **Web**プロジェクトの種類と**ASP.NET MVC Web アプリケーション**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="49406-166">Select the **Web** project type and the **ASP.NET MVC Web Application** template.</span></span> <span data-ttu-id="49406-167">新しいプロジェクトに名前を*ContactManager* [ok] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="49406-167">Name your new project *ContactManager* and click the OK button.</span></span>


<span data-ttu-id="49406-168">上部にあるドロップダウン リストから選択されている .NET Framework 3.5 があることを確認の右、**新しいプロジェクト**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="49406-168">Make sure that you have .NET Framework 3.5 selected from the dropdown list at the top right of the **New Project** dialog.</span></span> <span data-ttu-id="49406-169">それ以外の場合、t が勝利した ASP.NET MVC Web アプリケーション テンプレートが表示されます。</span><span class="sxs-lookup"><span data-stu-id="49406-169">Otherwise, the ASP.NET MVC Web Application template won t appear.</span></span>


<span data-ttu-id="49406-170">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="49406-170">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)</span></span>

<span data-ttu-id="49406-171">**図 01**: [新しいプロジェクト] ダイアログ ボックス ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image2.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-171">**Figure 01**: The New Project dialog([Click to view full-size image](iteration-1-create-the-application-cs/_static/image2.png))</span></span>


<span data-ttu-id="49406-172">ASP.NET MVC アプリケーション、**単体テスト プロジェクトの作成**ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="49406-172">ASP.NET MVC application, the **Create Unit Test Project** dialog appears.</span></span> <span data-ttu-id="49406-173">このダイアログ ボックスを使用すると、作成し、ASP.NET MVC アプリケーションを作成するときに、単体テスト プロジェクトをソリューションに追加することを指定します。</span><span class="sxs-lookup"><span data-stu-id="49406-173">You can use this dialog to indicate that you want to create and add a unit test project to your solution when you create your ASP.NET MVC application.</span></span> <span data-ttu-id="49406-174">獲得した t はこのイテレーションの単体テストを構築するには、オプションを選択する必要があります**はい、単体テスト プロジェクトを作成**後のイテレーションで単体テストを追加する予定なので。</span><span class="sxs-lookup"><span data-stu-id="49406-174">Although we won t be building unit tests in this iteration, you should select the option **Yes, create a unit test project** because we plan to add unit tests in a later iteration.</span></span> <span data-ttu-id="49406-175">最初に新しい ASP.NET MVC プロジェクトを作成するときは、テスト プロジェクトを追加するは、ASP.NET MVC プロジェクトが作成された後にテスト プロジェクトを追加するよりもはるかに簡単です。</span><span class="sxs-lookup"><span data-stu-id="49406-175">Adding a Test project when you first create a new ASP.NET MVC project is much easier than adding a Test project after the ASP.NET MVC project has been created.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="49406-176">Visual Web Developer では、テスト プロジェクトはサポートされていません、Visual Web Developer を使用する場合、単体テスト プロジェクトの作成ダイアログを取得しないできません。</span><span class="sxs-lookup"><span data-stu-id="49406-176">Because Visual Web Developer does not support Test projects, you do not get the Create Unit Test Project dialog when using Visual Web Developer.</span></span>


<span data-ttu-id="49406-177">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="49406-177">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)</span></span>

<span data-ttu-id="49406-178">**図 02**:、単体テスト プロジェクトの作成 ダイアログ ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image4.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-178">**Figure 02**: The Create Unit Test Project dialog ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image4.png))</span></span>


<span data-ttu-id="49406-179">ASP.NET MVC アプリケーションは、Visual Studio ソリューション エクスプ ローラー ウィンドウに表示されます (図 3 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="49406-179">ASP.NET MVC application appears in the Visual Studio Solution Explorer window (see Figure 3).</span></span> <span data-ttu-id="49406-180">表示されないが、ソリューション エクスプ ローラー ウィンドウを表示 メニュー オプションを選択して、このウィンドウを開くことができますし、場合**ビュー、ソリューション エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="49406-180">If you don t see the Solution Explorer window then you can open this window by selecting the menu option **View, Solution Explorer**.</span></span> <span data-ttu-id="49406-181">2 つのプロジェクトがソリューションに含まれることに注意してください。 ASP.NET MVC プロジェクトとテストのプロジェクト。</span><span class="sxs-lookup"><span data-stu-id="49406-181">Notice that the solution contains two projects: the ASP.NET MVC project and the Test project.</span></span> <span data-ttu-id="49406-182">ContactManager は ASP.NET MVC プロジェクトの名前し、ContactManager.Tests はテスト プロジェクトの名前します。</span><span class="sxs-lookup"><span data-stu-id="49406-182">The ASP.NET MVC project is named ContactManager and the Test project is named ContactManager.Tests.</span></span>


<span data-ttu-id="49406-183">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="49406-183">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)</span></span>

<span data-ttu-id="49406-184">**図 03**: ソリューション エクスプ ローラー ウィンドウ ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-184">**Figure 03**: The Solution Explorer window ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image6.png))</span></span>


## <a name="deleting-the-project-sample-files"></a><span data-ttu-id="49406-185">プロジェクトのサンプル ファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="49406-185">Deleting the Project Sample Files</span></span>

<span data-ttu-id="49406-186">ASP.NET MVC プロジェクト テンプレートには、コント ローラーとビューのサンプル ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="49406-186">The ASP.NET MVC project template includes sample files for controllers and views.</span></span> <span data-ttu-id="49406-187">新しい ASP.NET MVC アプリケーションを作成する前に、これらのファイルを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-187">Before creating a new ASP.NET MVC application, you should delete these files.</span></span> <span data-ttu-id="49406-188">ソリューション エクスプ ローラー ウィンドウでファイルとフォルダーを削除するには、ファイルまたはフォルダーを右クリックし、メニュー オプションを選択して**削除**します。</span><span class="sxs-lookup"><span data-stu-id="49406-188">You can delete files and folders in the Solution Explorer window by right-clicking a file or folder and selecting the menu option **Delete**.</span></span>

<span data-ttu-id="49406-189">ASP.NET MVC プロジェクトから、次のファイルを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-189">You need to delete the following files from the ASP.NET MVC project:</span></span>

- <span data-ttu-id="49406-190">\Controllers\HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="49406-190">\Controllers\HomeController.cs</span></span>

- <span data-ttu-id="49406-191">\Views\Home\About.aspx</span><span class="sxs-lookup"><span data-stu-id="49406-191">\Views\Home\About.aspx</span></span>

- <span data-ttu-id="49406-192">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="49406-192">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="49406-193">また、テスト プロジェクトから次のファイルを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-193">And, you need to delete the following file from the Test project:</span></span>

<span data-ttu-id="49406-194">\Controllers\HomeControllerTest.cs</span><span class="sxs-lookup"><span data-stu-id="49406-194">\Controllers\HomeControllerTest.cs</span></span>

## <a name="creating-the-database"></a><span data-ttu-id="49406-195">データベースの作成</span><span class="sxs-lookup"><span data-stu-id="49406-195">Creating the Database</span></span>

<span data-ttu-id="49406-196">Contact Manager アプリケーションとは、データベース駆動型 web アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="49406-196">The Contact Manager application is a database-driven web application.</span></span> <span data-ttu-id="49406-197">連絡先情報を格納するのにデータベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="49406-197">We use a database to store the contact information.</span></span>

<span data-ttu-id="49406-198">Microsoft SQL Server、Oracle、MySQL、および IBM DB2 データベースを含む、最新のデータベースでの ASP.NET MVC フレームワーク。</span><span class="sxs-lookup"><span data-stu-id="49406-198">The ASP.NET MVC framework with any modern database including Microsoft SQL Server, Oracle, MySQL, and IBM DB2 databases.</span></span> <span data-ttu-id="49406-199">このチュートリアルでは、Microsoft SQL Server データベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="49406-199">In this tutorial, we use a Microsoft SQL Server database.</span></span> <span data-ttu-id="49406-200">Visual Studio をインストールするときに、Microsoft SQL Server データベースの無料版である Microsoft SQL Server Express をインストールするオプションが提供されます。</span><span class="sxs-lookup"><span data-stu-id="49406-200">When you install Visual Studio, you are provided with the option of installing Microsoft SQL Server Express which is a free version of the Microsoft SQL Server database.</span></span>

<span data-ttu-id="49406-201">アプリを右クリックして新しいデータベースを作成\_メニュー オプションを選択して、ソリューション エクスプ ローラー ウィンドウで、データ フォルダー**追加]、[新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="49406-201">Create a new database by right-clicking the App\_Data folder in the Solution Explorer window and selecting the menu option **Add, New Item**.</span></span> <span data-ttu-id="49406-202">**新しい項目の追加**ダイアログ ボックスで、**データ**カテゴリと**SQL Server データベース**テンプレート (図 4 参照)。</span><span class="sxs-lookup"><span data-stu-id="49406-202">In the **Add New Item** dialog, select the **Data** category and the **SQL Server Database** template (see Figure 4).</span></span> <span data-ttu-id="49406-203">新しいデータベース ContactManagerDB.mdf 名し、[ok] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="49406-203">Name the new database ContactManagerDB.mdf and click the OK button.</span></span>


<span data-ttu-id="49406-204">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="49406-204">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)</span></span>

<span data-ttu-id="49406-205">**図 04**: 新しい Microsoft SQL Server Express データベースを作成する ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image8.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-205">**Figure 04**: Creating a new Microsoft SQL Server Express database ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image8.png))</span></span>


<span data-ttu-id="49406-206">新しいデータベースを作成した後、データベースがアプリに表示\_ソリューション エクスプ ローラー ウィンドウで、データ フォルダー。</span><span class="sxs-lookup"><span data-stu-id="49406-206">After you create the new database, the database appears in the App\_Data folder in the Solution Explorer window.</span></span> <span data-ttu-id="49406-207">サーバー エクスプ ローラー ウィンドウを開き、データベースに接続する ContactManager.mdf ファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="49406-207">Double-click the ContactManager.mdf file to open the Server Explorer window and connect to the database.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="49406-208">サーバー エクスプ ローラー ウィンドウには、Microsoft Visual Web Developer の場合、データベース エクスプ ローラー ウィンドウが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="49406-208">The Server Explorer window is called the Database Explorer window in the case of Microsoft Visual Web Developer.</span></span>


<span data-ttu-id="49406-209">サーバー エクスプ ローラー ウィンドウを使用すると、データベース テーブル、ビュー、トリガー、ストアド プロシージャなどの新しいデータベース オブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="49406-209">You can use the Server Explorer window to create new database objects such as database tables, views, triggers, and stored procedures.</span></span> <span data-ttu-id="49406-210">[テーブル] フォルダーを右クリックし、メニュー オプションを選択**新しいテーブルの追加**します。</span><span class="sxs-lookup"><span data-stu-id="49406-210">Right-click the Tables folder and select the menu option **Add New Table**.</span></span> <span data-ttu-id="49406-211">(図 5 を参照してください)、データベースのテーブル デザイナーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="49406-211">The Database Table Designer appears (see Figure 5).</span></span>


<span data-ttu-id="49406-212">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="49406-212">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)</span></span>

<span data-ttu-id="49406-213">**図 05**: データベースのテーブル デザイナー ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image10.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-213">**Figure 05**: The Database Table Designer ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image10.png))</span></span>


<span data-ttu-id="49406-214">次の列を含むテーブルを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-214">We need to create a table that contains the following columns:</span></span>

<a id="0.1_table01"></a>


| <span data-ttu-id="49406-215">**列名**</span><span class="sxs-lookup"><span data-stu-id="49406-215">**Column Name**</span></span> | <span data-ttu-id="49406-216">**データ型**</span><span class="sxs-lookup"><span data-stu-id="49406-216">**Data Type**</span></span> | <span data-ttu-id="49406-217">**Null を許容します。**</span><span class="sxs-lookup"><span data-stu-id="49406-217">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="49406-218">ID</span><span class="sxs-lookup"><span data-stu-id="49406-218">Id</span></span> | <span data-ttu-id="49406-219">int</span><span class="sxs-lookup"><span data-stu-id="49406-219">int</span></span> | <span data-ttu-id="49406-220">False</span><span class="sxs-lookup"><span data-stu-id="49406-220">false</span></span> |
| <span data-ttu-id="49406-221">FirstName</span><span class="sxs-lookup"><span data-stu-id="49406-221">FirstName</span></span> | <span data-ttu-id="49406-222">nvarchar (50)</span><span class="sxs-lookup"><span data-stu-id="49406-222">nvarchar(50)</span></span> | <span data-ttu-id="49406-223">False</span><span class="sxs-lookup"><span data-stu-id="49406-223">false</span></span> |
| <span data-ttu-id="49406-224">LastName</span><span class="sxs-lookup"><span data-stu-id="49406-224">LastName</span></span> | <span data-ttu-id="49406-225">nvarchar (50)</span><span class="sxs-lookup"><span data-stu-id="49406-225">nvarchar(50)</span></span> | <span data-ttu-id="49406-226">False</span><span class="sxs-lookup"><span data-stu-id="49406-226">false</span></span> |
| <span data-ttu-id="49406-227">電話番号</span><span class="sxs-lookup"><span data-stu-id="49406-227">Phone</span></span> | <span data-ttu-id="49406-228">nvarchar (50)</span><span class="sxs-lookup"><span data-stu-id="49406-228">nvarchar(50)</span></span> | <span data-ttu-id="49406-229">False</span><span class="sxs-lookup"><span data-stu-id="49406-229">false</span></span> |
| <span data-ttu-id="49406-230">電子メール</span><span class="sxs-lookup"><span data-stu-id="49406-230">Email</span></span> | <span data-ttu-id="49406-231">nvarchar(255)</span><span class="sxs-lookup"><span data-stu-id="49406-231">nvarchar(255)</span></span> | <span data-ttu-id="49406-232">False</span><span class="sxs-lookup"><span data-stu-id="49406-232">false</span></span> |


<span data-ttu-id="49406-233">最初の列、Id 列は特殊です。</span><span class="sxs-lookup"><span data-stu-id="49406-233">The first column, the Id column, is special.</span></span> <span data-ttu-id="49406-234">Id 列と主キー列として Id 列をマークする必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-234">You need to mark the Id column as an Identity column and a Primary Key column.</span></span> <span data-ttu-id="49406-235">列にある列のプロパティ (図 6 下部にあるを参照) を展開し、Identity の指定のプロパティの下にスクロールして、Id 列を指定します。</span><span class="sxs-lookup"><span data-stu-id="49406-235">You indicate that a column is an Identity column by expanding Column Properites (look at the bottom of Figure 6) and scrolling down to the Identity Specification property.</span></span> <span data-ttu-id="49406-236">設定、 **(Id)** プロパティ値を**はい**します。</span><span class="sxs-lookup"><span data-stu-id="49406-236">Set the **(Is Identity)** property to the value **Yes**.</span></span>

<span data-ttu-id="49406-237">主キー列として列を選択し、キーのアイコンのボタンをクリックして列をマークするとします。</span><span class="sxs-lookup"><span data-stu-id="49406-237">You mark a column as a Primary Key column by selecting the column and clicking the button with the icon of a key.</span></span> <span data-ttu-id="49406-238">列が主キー列としてマークされている、キーのアイコンが、列の横に表示されます (図 6 参照)。</span><span class="sxs-lookup"><span data-stu-id="49406-238">After a column is marked as a Primary Key column, an icon of a key appears next to the column (see Figure 6).</span></span>

<span data-ttu-id="49406-239">テーブルの作成が完了したら、新しいテーブルを保存する (フロッピー ディスクのアイコンのボタン) の保存 ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="49406-239">After you finish creating the table, click the Save button (the button with an icon of a floppy) to save the new table.</span></span> <span data-ttu-id="49406-240">新しいテーブルの名前を付けて*連絡先*します。</span><span class="sxs-lookup"><span data-stu-id="49406-240">Give your new table the name *Contacts*.</span></span>

<span data-ttu-id="49406-241">連絡先データベースのテーブルの作成を完了するには後、は、テーブルにレコードの一部を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-241">After finish creating the Contacts database table, you should add some records to the table.</span></span> <span data-ttu-id="49406-242">サーバー エクスプ ローラー ウィンドウで、Contacts テーブルを右クリックし、メニュー オプションを選択**テーブル データの表示**します。</span><span class="sxs-lookup"><span data-stu-id="49406-242">Right-click the Contacts table in the Server Explorer window and select the menu option **Show Table Data**.</span></span> <span data-ttu-id="49406-243">表示されたグリッドで 1 つまたは複数の連絡先を入力します。</span><span class="sxs-lookup"><span data-stu-id="49406-243">Enter one or more contacts in the grid that appears.</span></span>

## <a name="creating-the-data-model"></a><span data-ttu-id="49406-244">データ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="49406-244">Creating the Data Model</span></span>

<span data-ttu-id="49406-245">ASP.NET MVC アプリケーションは、モデル、ビュー、およびコント ローラーで構成されます。</span><span class="sxs-lookup"><span data-stu-id="49406-245">The ASP.NET MVC application consists of Models, Views, and Controllers.</span></span> <span data-ttu-id="49406-246">まず、前のセクションで作成した Contacts テーブルを表すモデル クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="49406-246">We start by creating a Model class that represents the Contacts table that we created in the previous section.</span></span>

<span data-ttu-id="49406-247">このチュートリアルでは、データベースからモデル クラスを自動的に生成するのに Microsoft Entity Framework を使用します。</span><span class="sxs-lookup"><span data-stu-id="49406-247">In this tutorial, we use the Microsoft Entity Framework to generate a model class from the database automatically.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="49406-248">ASP.NET MVC フレームワークは、任意の方法で Microsoft Entity Framework には関連付けられません。</span><span class="sxs-lookup"><span data-stu-id="49406-248">The ASP.NET MVC framework is not tied to the Microsoft Entity Framework in any way.</span></span> <span data-ttu-id="49406-249">ASP.NET MVC は、NHibernate、LINQ to SQL、または ADO.NET を含む別のデータベース アクセス テクノロジで使用できます。</span><span class="sxs-lookup"><span data-stu-id="49406-249">You can use ASP.NET MVC with alternative database access technologies including NHibernate, LINQ to SQL, or ADO.NET.</span></span>


<span data-ttu-id="49406-250">データ モデル クラスを作成する次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="49406-250">Follow these steps to create the data model classes:</span></span>

1. <span data-ttu-id="49406-251">ソリューション エクスプ ローラー ウィンドウで、Models フォルダーを右クリックして**追加、新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="49406-251">Right-click the Models folder in the Solution Explorer window and select **Add, New Item**.</span></span> <span data-ttu-id="49406-252">**新しい項目の追加**ダイアログでは、(図 6 参照) が表示されます。</span><span class="sxs-lookup"><span data-stu-id="49406-252">The **Add New Item** dialog appears (see Figure 6).</span></span>
2. <span data-ttu-id="49406-253">選択、**データ**カテゴリと**ADO.NET Entity Data Model**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="49406-253">Select the **Data** category and the **ADO.NET Entity Data Model** template.</span></span> <span data-ttu-id="49406-254">データ モデルの名前を付けます*ContactManagerModel.edmx*  をクリックし、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="49406-254">Name your data model *ContactManagerModel.edmx* and click the **Add** button.</span></span> <span data-ttu-id="49406-255">(図 7 を参照してください)、Entity Data Model ウィザードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="49406-255">The Entity Data Model wizard appears (see Figure 7).</span></span>
3. <span data-ttu-id="49406-256">**モデルのコンテンツの選択**手順で、**データベースから生成**(図 7 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="49406-256">In the **Choose Model Contents** step, select **Generate from database** (see Figure 7).</span></span>
4. <span data-ttu-id="49406-257">**データ接続の選択**ステップ、ContactManagerDB.mdf データベースを選択し、名前を入力*ContactManagerDBEntities*エンティティ接続設定が (図 8 参照)。</span><span class="sxs-lookup"><span data-stu-id="49406-257">In the **Choose Your Data Connection** step, select the ContactManagerDB.mdf database and enter the name *ContactManagerDBEntities* for the Entity Connection Settings (see Figure 8).</span></span>
5. <span data-ttu-id="49406-258">**データベース オブジェクトの選択**ステップで、テーブル (図 9 参照) というラベルの付いたチェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="49406-258">In the **Choose Your Database Objects** step, select the checkbox labeled Tables (see Figure 9).</span></span> <span data-ttu-id="49406-259">データ モデル (だけ 1 つである、Contacts テーブル)、データベースに含まれるすべてのテーブルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="49406-259">The data model will include all tables contained in your database (there is just one, the Contacts table).</span></span> <span data-ttu-id="49406-260">名前空間を入力*モデル*します。</span><span class="sxs-lookup"><span data-stu-id="49406-260">Enter the namespace *Models*.</span></span> <span data-ttu-id="49406-261">ウィザードを完了するには、[完了] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="49406-261">Click the Finish button to complete the wizard.</span></span>


<span data-ttu-id="49406-262">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="49406-262">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)</span></span>

<span data-ttu-id="49406-263">**図 06**: [新しい項目の追加] ダイアログ ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image12.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-263">**Figure 06**: The Add New Item dialog ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image12.png))</span></span>


<span data-ttu-id="49406-264">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="49406-264">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)</span></span>

<span data-ttu-id="49406-265">**図 07**: [モデルのコンテンツ ([フルサイズの画像を表示する] をクリックします](iteration-1-create-the-application-cs/_static/image14.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-265">**Figure 07**: Choose Model Contents ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image14.png))</span></span>


<span data-ttu-id="49406-266">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="49406-266">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)</span></span>

<span data-ttu-id="49406-267">**図 08**: データ接続の選択 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image16.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-267">**Figure 08**: Choose Your Data Connection ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image16.png))</span></span>


<span data-ttu-id="49406-268">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="49406-268">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)</span></span>

<span data-ttu-id="49406-269">**図 09**: データベース オブジェクトの選択 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image18.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-269">**Figure 09**: Choose Your Database Objects ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image18.png))</span></span>


<span data-ttu-id="49406-270">Entity Data Model ウィザードを完了すると、Entity Data Model のデザイナーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="49406-270">After you complete the Entity Data Model Wizard, the Entity Data Model Designer appears.</span></span> <span data-ttu-id="49406-271">デザイナーでは、モデル化される各テーブルに対応するクラスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="49406-271">The designer displays a class that corresponds to each table being modeled.</span></span> <span data-ttu-id="49406-272">連絡先をという名前の 1 つのクラスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="49406-272">You should see one class named Contacts.</span></span>

<span data-ttu-id="49406-273">Entity Data Model ウィザードでは、データベース テーブル名に基づくクラス名を生成します。</span><span class="sxs-lookup"><span data-stu-id="49406-273">The Entity Data Model wizard generates class names based on database table names.</span></span> <span data-ttu-id="49406-274">ほとんどの場合、ウィザードによって生成されたクラスの名前を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-274">You almost always need to change the name of the class generated by the wizard.</span></span> <span data-ttu-id="49406-275">デザイナーで、Contacts クラスを右クリックし、メニュー オプションを選択**の名前を変更**します。</span><span class="sxs-lookup"><span data-stu-id="49406-275">Right-click the Contacts class in the designer and select the menu option **Rename**.</span></span> <span data-ttu-id="49406-276">連絡先 (複数) から、クラスの名前を変更するには (単数) にお問い合わせください。</span><span class="sxs-lookup"><span data-stu-id="49406-276">Change the name of the class from Contacts (plural) to Contact (singular).</span></span> <span data-ttu-id="49406-277">クラス名を変更した後は、図 10 のようなクラスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="49406-277">After you change the class name, the class should appear like Figure 10.</span></span>


<span data-ttu-id="49406-278">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="49406-278">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)</span></span>

<span data-ttu-id="49406-279">**図 10**: Contact クラス ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image20.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-279">**Figure 10**: The Contact class ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image20.png))</span></span>


<span data-ttu-id="49406-280">この時点で、データベース モデルを作成しました。</span><span class="sxs-lookup"><span data-stu-id="49406-280">At this point, we have created our database model.</span></span> <span data-ttu-id="49406-281">データベースに特定の連絡先レコードを表す、Contact クラスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="49406-281">We can use the Contact class to represent a particular contact record in our database.</span></span>

## <a name="creating-the-home-controller"></a><span data-ttu-id="49406-282">Home コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="49406-282">Creating the Home Controller</span></span>

<span data-ttu-id="49406-283">次の手順では、ホーム コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="49406-283">The next step is to create our Home controller.</span></span> <span data-ttu-id="49406-284">ホーム コント ローラーは、ASP.NET MVC アプリケーションで呼び出される既定のコント ローラーです。</span><span class="sxs-lookup"><span data-stu-id="49406-284">The Home controller is the default controller invoked in an ASP.NET MVC application.</span></span>

<span data-ttu-id="49406-285">ホーム コント ローラーのクラスを作成するには、ソリューション エクスプ ローラー ウィンドウで、Controllers フォルダーを右クリックし、メニュー オプションを選択する**追加、コント ローラー** (図 11 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="49406-285">Create the Home controller class by right-clicking the Controllers folder in the Solution Explorer window and selecting the menu option **Add, Controller** (see Figure 11).</span></span> <span data-ttu-id="49406-286">チェック ボックスに注意してください**アクション メソッドの作成、更新、および詳細のシナリオを追加**します。</span><span class="sxs-lookup"><span data-stu-id="49406-286">Notice the checkbox labeled **Add action methods for Create, Update, and Details scenarios**.</span></span> <span data-ttu-id="49406-287">クリックする前にこのチェック ボックスがオンになっていることを確認、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="49406-287">Make sure that this checkbox is checked before clicking the **Add** button.</span></span>


<span data-ttu-id="49406-288">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="49406-288">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)</span></span>

<span data-ttu-id="49406-289">**図 11**: Home コント ローラーの追加 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image22.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-289">**Figure 11**: Adding the Home controller ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image22.png))</span></span>


<span data-ttu-id="49406-290">Home コント ローラーを作成するときに、リスト 1 で、クラスを取得します。</span><span class="sxs-lookup"><span data-stu-id="49406-290">When you create the Home controller, you get the class in Listing 1.</span></span>

<span data-ttu-id="49406-291">**1 - controllers \homecontroller.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="49406-291">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample1.cs)]

## <a name="listing-the-contacts"></a><span data-ttu-id="49406-292">連絡先の一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="49406-292">Listing the Contacts</span></span>

<span data-ttu-id="49406-293">連絡先のデータベース テーブル内のレコードを表示するためには、Index() アクションと、インデックス ビューを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-293">In order to display the records in the Contacts database table, we need to create an Index() action and an Index view.</span></span>

<span data-ttu-id="49406-294">Home コント ローラーには、既に Index() アクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="49406-294">The Home controller already contains an Index() action.</span></span> <span data-ttu-id="49406-295">リスト 2 のように見えるように、このメソッドを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-295">We need to modify this method so that it looks like Listing 2.</span></span>

<span data-ttu-id="49406-296">**2 - controllers \homecontroller.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="49406-296">**Listing 2 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample2.cs)]

<span data-ttu-id="49406-297">リスト 2 で、ホーム コント ローラー クラスにというプライベート フィールドが含まれている通知\_エンティティ。</span><span class="sxs-lookup"><span data-stu-id="49406-297">Notice that the Home controller class in Listing 2 contains a private field named \_entities.</span></span> <span data-ttu-id="49406-298">\_エンティティ フィールドは、データ モデルからエンティティを表します。</span><span class="sxs-lookup"><span data-stu-id="49406-298">The \_entities field represents the entities from the data model.</span></span> <span data-ttu-id="49406-299">使用して、\_エンティティのフィールドに、データベースと通信します。</span><span class="sxs-lookup"><span data-stu-id="49406-299">We use the \_entities field to communicate with the database.</span></span>

<span data-ttu-id="49406-300">Index() メソッドは、ビューを連絡先データベースのテーブルからすべての連絡先を表すを返します。</span><span class="sxs-lookup"><span data-stu-id="49406-300">The Index() method returns a view that represents all of the contacts from the Contacts database table.</span></span> <span data-ttu-id="49406-301">式\_エンティティ。ContactSet.ToList() は、ジェネリック リストとして、連絡先の一覧を返します。</span><span class="sxs-lookup"><span data-stu-id="49406-301">The expression \_entities.ContactSet.ToList() returns the list of contacts as a generic list.</span></span>

<span data-ttu-id="49406-302">これまで作成インデックス コント ローラーで、次に、インデックス ビューを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-302">Now that we ve created the Index controller, we next need to create the Index view.</span></span> <span data-ttu-id="49406-303">インデックス ビューを作成する前に、メニュー オプションを選択してアプリケーションをコンパイルします**ソリューションのビルドをビルドします。** します。</span><span class="sxs-lookup"><span data-stu-id="49406-303">Before creating the Index view, compile your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="49406-304">常に表示されるモデル クラスの一覧の順序でビューを追加する前にプロジェクトをコンパイルする必要があります、**ビューの追加**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="49406-304">You should always compile your project before adding a view in order for the list of model classes to be displayed in the **Add View** dialog.</span></span>

<span data-ttu-id="49406-305">Index() メソッドを右クリックし、メニュー オプションを選択して、インデックス ビューを作成する**ビューの追加**(図 12 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="49406-305">You create the Index view by right-clicking the Index() method and selecting the menu option **Add View** (see Figure 12).</span></span> <span data-ttu-id="49406-306">このメニュー オプションを選択すると表示、**ビューの追加**ダイアログ (図 13 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="49406-306">Selecting this menu option opens the **Add View** dialog (see Figure 13).</span></span>


<span data-ttu-id="49406-307">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="49406-307">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)</span></span>

<span data-ttu-id="49406-308">**図 12**: インデックス ビューの追加 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image24.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-308">**Figure 12**: Adding the Index view ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image24.png))</span></span>


<span data-ttu-id="49406-309">**ビューの追加**ダイアログ ボックスで、チェック ボックスをラベル付け**厳密に型指定されたビューを作成**。</span><span class="sxs-lookup"><span data-stu-id="49406-309">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="49406-310">ビューのデータ クラス ContactManager.Models.Contact とコンテンツ リストの表示を選択します。</span><span class="sxs-lookup"><span data-stu-id="49406-310">Select the View data class ContactManager.Models.Contact and the View content List.</span></span> <span data-ttu-id="49406-311">これらのオプションを選択するには、連絡先のレコードの一覧を表示するビューが生成されます。</span><span class="sxs-lookup"><span data-stu-id="49406-311">Selecting these options generates a view that displays a list of Contact records.</span></span>


<span data-ttu-id="49406-312">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="49406-312">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)</span></span>

<span data-ttu-id="49406-313">**図 13**: ビューの追加 ダイアログ ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image26.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-313">**Figure 13**: The Add View dialog ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image26.png))</span></span>


<span data-ttu-id="49406-314">クリックすると、**追加** ボタン、リスト 3 のインデックス ビューを生成します。</span><span class="sxs-lookup"><span data-stu-id="49406-314">When you click the **Add** button, the Index view in Listing 3 is generated.</span></span> <span data-ttu-id="49406-315">通知、&lt;ページ % @ %&gt;ディレクティブ ファイルの先頭に表示されます。</span><span class="sxs-lookup"><span data-stu-id="49406-315">Notice the &lt;%@ Page %&gt; directive that appears at the top of the file.</span></span> <span data-ttu-id="49406-316">インデックス ビューが継承、ViewPage&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt;クラス。</span><span class="sxs-lookup"><span data-stu-id="49406-316">The Index view inherits from the ViewPage&lt;IEnumerable&lt;ContactManager.Models.Contact&gt;&gt; class.</span></span> <span data-ttu-id="49406-317">つまり、ビュー モデル クラスは、Contact エンティティの一覧を表します。</span><span class="sxs-lookup"><span data-stu-id="49406-317">In other words, the Model class in the view represents a list of Contact entities.</span></span>

<span data-ttu-id="49406-318">Index ビューの本文には、各モデルのクラスによって表されるメンバーを反復処理する foreach ループが含まれています。</span><span class="sxs-lookup"><span data-stu-id="49406-318">The body of the Index view contains a foreach loop that iterates through each of the contacts represented by the Model class.</span></span> <span data-ttu-id="49406-319">連絡先クラスの各プロパティの値は、HTML テーブル内で表示されます。</span><span class="sxs-lookup"><span data-stu-id="49406-319">The value of each property of the Contact class is displayed within an HTML table.</span></span>

<span data-ttu-id="49406-320">**3 - Views\Home\Index.aspx (未変更の状態) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="49406-320">**Listing 3 - Views\Home\Index.aspx (unmodified)**</span></span>

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample3.aspx)]

<span data-ttu-id="49406-321">インデックス ビューに 1 つの変更を加える必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-321">We need to make one modification to the Index view.</span></span> <span data-ttu-id="49406-322">詳細ビューを作成することはありません、ため、[詳細] リンクを削除できます。</span><span class="sxs-lookup"><span data-stu-id="49406-322">Because we are not creating a Details view, we can remove the Details link.</span></span> <span data-ttu-id="49406-323">検索し、インデックス ビューから、次のコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="49406-323">Find and remove the following code from the Index view:</span></span>

<span data-ttu-id="49406-324">{ id=item.Id })%&gt;</span><span class="sxs-lookup"><span data-stu-id="49406-324">{ id=item.Id })%&gt;</span></span>

<span data-ttu-id="49406-325">インデックス ビューを変更した後は、Contact Manager アプリケーションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="49406-325">After you modify the Index view, you can run the Contact Manager application.</span></span> <span data-ttu-id="49406-326">[デバッグ開始] メニュー オプション、デバッグを選択するか、f5 キーを押すだけです。</span><span class="sxs-lookup"><span data-stu-id="49406-326">Select the menu option Debug, Start Debugging or simply press F5.</span></span> <span data-ttu-id="49406-327">初めてアプリケーションを実行するダイアログ ボックスを図 14 に取得します。</span><span class="sxs-lookup"><span data-stu-id="49406-327">The first time you run the application, you get the dialog in Figure 14.</span></span> <span data-ttu-id="49406-328">オプションを選択**デバッグを有効にする Web.config ファイルを変更**[ok] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="49406-328">Select the option **Modify the Web.config file to enable debugging** and click the OK button.</span></span>


<span data-ttu-id="49406-329">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)</span><span class="sxs-lookup"><span data-stu-id="49406-329">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)</span></span>

<span data-ttu-id="49406-330">**図 14**: デバッグの有効化 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image28.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-330">**Figure 14**: Enabling debugging ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image28.png))</span></span>


<span data-ttu-id="49406-331">既定では、インデックス ビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="49406-331">The Index view is returned by default.</span></span> <span data-ttu-id="49406-332">このビューでは、連絡先データベースのテーブルからデータをすべて一覧表示されます (図 15 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="49406-332">This view lists all of the data from the Contacts database table (see Figure 15).</span></span>


<span data-ttu-id="49406-333">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)</span><span class="sxs-lookup"><span data-stu-id="49406-333">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)</span></span>

<span data-ttu-id="49406-334">**図 15**: The Index ビュー ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image30.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-334">**Figure 15**: The Index view ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image30.png))</span></span>


<span data-ttu-id="49406-335">インデックス ビューに、ビューの下部にある場合は、新規作成というラベルの付いたリンクが含まれることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="49406-335">Notice that the Index view includes a link labeled Create New at the bottom of the view.</span></span> <span data-ttu-id="49406-336">次のセクションでは、新しい連絡先を作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="49406-336">In the next section, you learn how to create new contacts.</span></span>

## <a name="creating-new-contacts"></a><span data-ttu-id="49406-337">新しい連絡先を作成します。</span><span class="sxs-lookup"><span data-stu-id="49406-337">Creating New Contacts</span></span>

<span data-ttu-id="49406-338">新しい連絡先を作成するユーザーを有効にするのには、Home コント ローラーに 2 つの Create() アクションを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-338">To enable users to create new contacts, we need to add two Create() actions to the Home controller.</span></span> <span data-ttu-id="49406-339">新しい連絡先を作成するための HTML フォームを返す 1 つの Create() アクションを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-339">We need to create one Create() action that returns an HTML form for creating a new contact.</span></span> <span data-ttu-id="49406-340">新しい連絡先の実際のデータベースの挿入を実行する 2 番目の Create() アクションを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-340">We need to create a second Create() action that performs the actual database insert of the new contact.</span></span>

<span data-ttu-id="49406-341">Home コント ローラーに追加する必要がある新しい Create() メソッドは、リスト 4 に含まれます。</span><span class="sxs-lookup"><span data-stu-id="49406-341">The new Create() methods that we need to add to the Home controller are contained in Listing 4.</span></span>

<span data-ttu-id="49406-342">**4 - (メソッドを作成する) で、controllers \homecontroller.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="49406-342">**Listing 4 - Controllers\HomeController.cs (with Create methods)**</span></span>

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample4.cs)]

<span data-ttu-id="49406-343">2 番目の Create() メソッドは、HTTP POST によってのみ呼び出すことがときに、HTTP GET を使用 Create() の最初のメソッドを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="49406-343">The first Create() method can be invoked with an HTTP GET while the second Create() method can be invoked only by an HTTP POST.</span></span> <span data-ttu-id="49406-344">つまり、2 番目の Create() メソッドは、HTML フォームを投稿する場合にのみ起動できます。</span><span class="sxs-lookup"><span data-stu-id="49406-344">In other words, the second Create() method can be invoked only when posting an HTML form.</span></span> <span data-ttu-id="49406-345">最初の Create() メソッドは、単に、新しい連絡先を作成するための HTML フォームを含むビューを返します。</span><span class="sxs-lookup"><span data-stu-id="49406-345">The first Create() method simply returns a view that contains the HTML form for creating a new contact.</span></span> <span data-ttu-id="49406-346">2 番目の Create() メソッドにずっと興味深いものです。 データベースに新しい連絡先を追加します。</span><span class="sxs-lookup"><span data-stu-id="49406-346">The second Create() method is much more interesting: it adds the new contact to the database.</span></span>

<span data-ttu-id="49406-347">連絡先クラスのインスタンスをそのまま使用する 2 番目の Create() メソッドが変更されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="49406-347">Notice that the second Create() method has been modified to accept an instance of the Contact class.</span></span> <span data-ttu-id="49406-348">HTML フォームからポストされたフォーム値にバインドされますこの連絡先クラス、ASP.NET MVC フレームワークによって自動的に。</span><span class="sxs-lookup"><span data-stu-id="49406-348">The form values posted from the HTML form are bound to this Contact class by the ASP.NET MVC framework automatically.</span></span> <span data-ttu-id="49406-349">作成する HTML フォームから各フォーム フィールドは、連絡先のパラメーターのプロパティに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="49406-349">Each form field from the HTML Create form is assigned to a property of the Contact parameter.</span></span>

<span data-ttu-id="49406-350">連絡先のパラメーターが [Bind] 属性で修飾されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="49406-350">Notice that the Contact parameter is decorated with a [Bind] attribute.</span></span> <span data-ttu-id="49406-351">[バインド] 属性は、連絡先 Id プロパティをバインドから除外に使用されます。</span><span class="sxs-lookup"><span data-stu-id="49406-351">The [Bind] attribute is used to exclude the Contact Id property from binding.</span></span> <span data-ttu-id="49406-352">Id プロパティは、Identity プロパティを表すため、Id プロパティを設定することはありません。</span><span class="sxs-lookup"><span data-stu-id="49406-352">Because the Id property represents an Identity property, we don t want to set the Id property.</span></span>

<span data-ttu-id="49406-353">Create() メソッドの本文には、Entity Framework がデータベースに新しい連絡先の挿入に使用されます。</span><span class="sxs-lookup"><span data-stu-id="49406-353">In the body of the Create() method, the Entity Framework is used to insert the new Contact into the database.</span></span> <span data-ttu-id="49406-354">新しい連絡先が連絡先の既存のセットに追加され、SaveChanges() メソッドは、基になるデータベースにこれらの変更をプッシュ バックします。</span><span class="sxs-lookup"><span data-stu-id="49406-354">The new Contact is added to the existing set of Contacts and the SaveChanges() method is called to push these changes back to the underlying database.</span></span>

<span data-ttu-id="49406-355">2 つの Create() メソッドのいずれかを右クリックし、メニュー オプションを選択して新しい連絡先を作成するための HTML フォームを生成する**ビューの追加**(図 16 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="49406-355">You can generate an HTML form for creating new Contacts by right-clicking either of the two Create() methods and selecting the menu option **Add View** (see Figure 16).</span></span>


<span data-ttu-id="49406-356">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)</span><span class="sxs-lookup"><span data-stu-id="49406-356">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)</span></span>

<span data-ttu-id="49406-357">**図 16**: 作成ビューの追加 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image32.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-357">**Figure 16**: Adding the Create view ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image32.png))</span></span>


<span data-ttu-id="49406-358">**ビューの追加**ダイアログ ボックスで、 **ContactManager.Models.Contact**クラスおよび**作成**コンテンツの表示のオプション (図 17 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="49406-358">In the **Add View** dialog, select the **ContactManager.Models.Contact** class and the **Create** option for view content (see Figure 17).</span></span> <span data-ttu-id="49406-359">クリックすると、**追加**ボタン、作成ビューが自動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="49406-359">When you click the **Add** button, a Create view is generated automatically.</span></span>


<span data-ttu-id="49406-360">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)</span><span class="sxs-lookup"><span data-stu-id="49406-360">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)</span></span>

<span data-ttu-id="49406-361">**図 17**: 展開のページが表示 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image34.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-361">**Figure 17**: Seeing a page explode ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image34.png))</span></span>


<span data-ttu-id="49406-362">ビュー作成するにはには、各連絡先クラスのプロパティのフォームのフィールドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="49406-362">The Create view contains form fields for each of the properties of the Contact class.</span></span> <span data-ttu-id="49406-363">ビューを作成するためのコードは、リスト 5 に含まれます。</span><span class="sxs-lookup"><span data-stu-id="49406-363">The code for the Create view is included in Listing 5.</span></span>

<span data-ttu-id="49406-364">**Listing 5 - Views\Home\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="49406-364">**Listing 5 - Views\Home\Create.aspx**</span></span>

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample5.aspx)]

<span data-ttu-id="49406-365">Create() メソッドを変更し、作成ビューを追加した後は、連絡先マネージャー アプリケーションを実行し、新しい連絡先を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="49406-365">After you modify the Create() methods and add the Create view, you can run the Contact Manger application and create new contacts.</span></span> <span data-ttu-id="49406-366">をクリックして、**新規作成**作成ビューに移動するインデックス ビューに表示されるリンク。</span><span class="sxs-lookup"><span data-stu-id="49406-366">Click the **Create New** link that appears in the Index view to navigate to the Create view.</span></span> <span data-ttu-id="49406-367">図 18 に表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-367">You should see the view in Figure 18.</span></span>


<span data-ttu-id="49406-368">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)</span><span class="sxs-lookup"><span data-stu-id="49406-368">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)</span></span>

<span data-ttu-id="49406-369">**図 18**: ビューの作成 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image36.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-369">**Figure 18**: The Create View ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image36.png))</span></span>


## <a name="editing-contacts"></a><span data-ttu-id="49406-370">連絡先の編集</span><span class="sxs-lookup"><span data-stu-id="49406-370">Editing Contacts</span></span>

<span data-ttu-id="49406-371">連絡先のレコードを編集するための機能の追加は、新しい連絡先のレコードを作成するための機能を追加するとよく似ています。</span><span class="sxs-lookup"><span data-stu-id="49406-371">Adding the functionality for editing a contact record is very similar to adding the functionality for creating new contact records.</span></span> <span data-ttu-id="49406-372">最初に、ホーム コント ローラーのクラスに新しい 2 つの Edit メソッドを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-372">First, we need to add two new Edit methods to the Home controller class.</span></span> <span data-ttu-id="49406-373">これらの新しい Edit() メソッドは、6 の一覧に含まれます。</span><span class="sxs-lookup"><span data-stu-id="49406-373">These new Edit() methods are contained in Listing 6.</span></span>

<span data-ttu-id="49406-374">**6 - (Edit メソッド) で、controllers \homecontroller.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="49406-374">**Listing 6 - Controllers\HomeController.cs (with Edit methods)**</span></span>

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample6.cs)]

<span data-ttu-id="49406-375">最初の Edit() メソッドは、HTTP GET 操作で呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="49406-375">The first Edit() method is invoked by an HTTP GET operation.</span></span> <span data-ttu-id="49406-376">Id パラメーターは編集されている連絡先のレコードの Id を表します。 このメソッドに渡されます。</span><span class="sxs-lookup"><span data-stu-id="49406-376">An Id parameter is passed to this method which represents the Id of the contact record being edited.</span></span> <span data-ttu-id="49406-377">Entity Framework は、id と一致する連絡先を取得するために使用します。レコードを編集するための HTML フォームを含むビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="49406-377">The Entity Framework is used to retrieve a contact that matches the Id. A view that contains an HTML form for editing a record is returned.</span></span>

<span data-ttu-id="49406-378">2 番目の Edit() メソッドは、データベースに実際の更新プログラムを実行します。</span><span class="sxs-lookup"><span data-stu-id="49406-378">The second Edit() method performs the actual update to the database.</span></span> <span data-ttu-id="49406-379">このメソッドは、Contact クラスのインスタンスをパラメーターとして受け取ります。</span><span class="sxs-lookup"><span data-stu-id="49406-379">This method accepts an instance of the Contact class as a parameter.</span></span> <span data-ttu-id="49406-380">ASP.NET MVC フレームワーク フォーム フィールド編集フォームからこのクラスに自動的にバインドします。</span><span class="sxs-lookup"><span data-stu-id="49406-380">The ASP.NET MVC framework binds the form fields from the Edit form to this class automatically.</span></span> <span data-ttu-id="49406-381">T がないことに注意してくださいには、(必要があります、Id プロパティの値) の連絡先を編集するときに、[バインド] 属性が含まれます。</span><span class="sxs-lookup"><span data-stu-id="49406-381">Notice that you don t include the[Bind] attribute when editing a Contact (we need the value of the Id property).</span></span>

<span data-ttu-id="49406-382">Entity Framework は、データベースに変更された連絡先を保存に使用されます。</span><span class="sxs-lookup"><span data-stu-id="49406-382">The Entity Framework is used to save the modified Contact to the database.</span></span> <span data-ttu-id="49406-383">元の連絡先は、まずデータベースから取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-383">The original Contact must be retrieved from the database first.</span></span> <span data-ttu-id="49406-384">次に、Entity Framework ApplyPropertyChanges() メソッドが呼び出され、連絡先に変更を記録します。</span><span class="sxs-lookup"><span data-stu-id="49406-384">Next, the Entity Framework ApplyPropertyChanges() method is called to record the changes to the Contact.</span></span> <span data-ttu-id="49406-385">最後に、基になるデータベースの変更を永続化、Entity Framework SaveChanges() メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="49406-385">Finally, the Entity Framework SaveChanges() method is called to persist the changes to the underlying database.</span></span>

<span data-ttu-id="49406-386">Edit() メソッドを右クリックし、追加のビュー メニュー オプションを選択して編集フォームを含むビューを生成することができます。</span><span class="sxs-lookup"><span data-stu-id="49406-386">You can generate the view that contains the Edit form by right-clicking the Edit() method and selecting the menu option Add View.</span></span> <span data-ttu-id="49406-387">ビューの追加] ダイアログ ボックスで、[、 **ContactManager.Models.Contact**クラスおよび**編集**コンテンツを表示 (図 19 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="49406-387">In the Add View dialog, select the **ContactManager.Models.Contact** class and the **Edit** view content (see Figure 19).</span></span>


<span data-ttu-id="49406-388">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)</span><span class="sxs-lookup"><span data-stu-id="49406-388">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)</span></span>

<span data-ttu-id="49406-389">**図 19**: ビューの編集の追加 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image38.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-389">**Figure 19**: Adding an Edit View ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image38.png))</span></span>


<span data-ttu-id="49406-390">[追加] ボタンをクリックすると、新しい編集ビューが自動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="49406-390">When you click the Add button, a new Edit view is generated automatically.</span></span> <span data-ttu-id="49406-391">生成される HTML フォームには、各連絡先クラス (7 のリストを参照) のプロパティに対応するフィールドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="49406-391">The HTML form that is generated contains fields that correspond to each of the properties of the Contact class (see Listing 7).</span></span>

<span data-ttu-id="49406-392">**Listing 7 - Views\Home\Edit.aspx**</span><span class="sxs-lookup"><span data-stu-id="49406-392">**Listing 7 - Views\Home\Edit.aspx**</span></span>

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample7.aspx)]

## <a name="deleting-contacts"></a><span data-ttu-id="49406-393">連絡先を削除します。</span><span class="sxs-lookup"><span data-stu-id="49406-393">Deleting Contacts</span></span>

<span data-ttu-id="49406-394">連絡先を削除する場合は、ホーム コント ローラー クラスに 2 つの Delete() アクションを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-394">If you want to delete contacts then you need to add two Delete() actions to the Home controller class.</span></span> <span data-ttu-id="49406-395">Delete() の最初のアクションは、削除の確認フォームを表示します。</span><span class="sxs-lookup"><span data-stu-id="49406-395">The first Delete() action displays a delete confirmation form.</span></span> <span data-ttu-id="49406-396">2 番目の Delete() アクションは、実際の削除を実行します。</span><span class="sxs-lookup"><span data-stu-id="49406-396">The second Delete() action performs the actual delete.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="49406-397">後で、7 では、イテレーションでよう変更 Contact Manager Ajax の削除、1 つの手順をサポートするようします。</span><span class="sxs-lookup"><span data-stu-id="49406-397">Later, in Iteration #7, we modify the Contact Manager so that it supports a one step Ajax delete.</span></span>


<span data-ttu-id="49406-398">2 つの新しい Delete() メソッドは、8 の一覧に含まれます。</span><span class="sxs-lookup"><span data-stu-id="49406-398">The two new Delete() methods are contained in Listing 8.</span></span>

<span data-ttu-id="49406-399">**8 - controllers \homecontroller.cs (削除メソッド) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="49406-399">**Listing 8 - Controllers\HomeController.cs (Delete methods)**</span></span>

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample8.cs)]

<span data-ttu-id="49406-400">最初の Delete() メソッドは、連絡先のレコードをデータベースから削除するための確認フォームを返します (Figure20 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="49406-400">The first Delete() method returns a confirmation form for deleting a contact record from the database (see Figure20).</span></span> <span data-ttu-id="49406-401">2 番目の Delete() メソッドは、データベースに対して実際の削除操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="49406-401">The second Delete() method performs the actual delete operation against the database.</span></span> <span data-ttu-id="49406-402">データベースから元の連絡先の取得が完了した後は、データベースの削除を実行する、Entity Framework DeleteObject() と SaveChanges() メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="49406-402">After the original contact has been retrieved from the database, the Entity Framework DeleteObject() and SaveChanges() methods are called to perform the database delete.</span></span>


<span data-ttu-id="49406-403">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)</span><span class="sxs-lookup"><span data-stu-id="49406-403">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)</span></span>

<span data-ttu-id="49406-404">**図 20**: 削除確定ビュー ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image40.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-404">**Figure 20**: The delete confirmation view ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image40.png))</span></span>


<span data-ttu-id="49406-405">(図 21 を参照してください) に連絡先レコードを削除するためのリンクが含まれているように、インデックス ビューを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-405">We need to modify the Index view so that it contains a link for deleting contact records (see Figure 21).</span></span> <span data-ttu-id="49406-406">[編集] リンクを含む同じテーブル セルに次のコードを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-406">You need to add the following code to the same table cell that contains the Edit link:</span></span>

<span data-ttu-id="49406-407">Html.ActionLink( { id=item.Id }) %&gt;</span><span class="sxs-lookup"><span data-stu-id="49406-407">Html.ActionLink( { id=item.Id }) %&gt;</span></span>


<span data-ttu-id="49406-408">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)</span><span class="sxs-lookup"><span data-stu-id="49406-408">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)</span></span>

<span data-ttu-id="49406-409">**図 21**: 編集リンクを使用してビューにインデックス ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image42.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-409">**Figure 21**: Index view with Edit link ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image42.png))</span></span>


<span data-ttu-id="49406-410">次に、削除確定ビューを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-410">Next, we need to create the delete confirmation view.</span></span> <span data-ttu-id="49406-411">ホーム コント ローラー クラスの Delete() メソッドを右クリックし、追加のビュー メニュー オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="49406-411">Right-click the Delete() method in the Home controller class and select the menu option Add View.</span></span> <span data-ttu-id="49406-412">(図 22 を参照してください) ビューの追加 ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="49406-412">The Add View dialog appears (see Figure 22).</span></span>

<span data-ttu-id="49406-413">異なりは、リスト、作成、編集ビューの場合、ビューの追加ダイアログには、削除ビューを作成するオプションがありません。</span><span class="sxs-lookup"><span data-stu-id="49406-413">Unlike in the case of the List, Create, and Edit views, the Add View dialog does not contain an option to create a Delete view.</span></span> <span data-ttu-id="49406-414">代わりに、選択、 **ContactManager.Models.Contact**データ クラスと**空**コンテンツを表示します。</span><span class="sxs-lookup"><span data-stu-id="49406-414">Instead, select the **ContactManager.Models.Contact** data class and the **Empty** view content.</span></span> <span data-ttu-id="49406-415">コンテンツのオプションでは、自分たちが、ビューを作成することが必要、空のビューを選択します。</span><span class="sxs-lookup"><span data-stu-id="49406-415">Selecting the Empty view content option will require us to create the view ourselves.</span></span>


<span data-ttu-id="49406-416">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)</span><span class="sxs-lookup"><span data-stu-id="49406-416">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)</span></span>

<span data-ttu-id="49406-417">**図 22**: 削除確認ビューの追加 ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image44.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-417">**Figure 22**: Adding the delete confirmation view ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image44.png))</span></span>


<span data-ttu-id="49406-418">一覧表示する 9 Delete ビューのコンテンツが含まれています。</span><span class="sxs-lookup"><span data-stu-id="49406-418">The content of the Delete view is contained in Listing 9.</span></span> <span data-ttu-id="49406-419">このビューには、確認のフォームが含まれます (図 21 を参照) を削除するかどうか、特定の連絡先があります。</span><span class="sxs-lookup"><span data-stu-id="49406-419">This view contains a form that confirms whether or not a particular contact should be deleted (see Figure 21).</span></span>

<span data-ttu-id="49406-420">**Listing 9 - Views\Home\Delete.aspx**</span><span class="sxs-lookup"><span data-stu-id="49406-420">**Listing 9 - Views\Home\Delete.aspx**</span></span>

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a><span data-ttu-id="49406-421">既定のコント ローラーの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="49406-421">Changing the Name of the Default Controller</span></span>

<span data-ttu-id="49406-422">連絡先を操作するため、コント ローラー クラスの名前が HomeController クラスということをわざわざ可能性があります。</span><span class="sxs-lookup"><span data-stu-id="49406-422">It might bother you that the name of our controller class for working with contacts is named the HomeController class.</span></span> <span data-ttu-id="49406-423">べきで t、コント ローラーは、ContactController を指定しますか。</span><span class="sxs-lookup"><span data-stu-id="49406-423">Shouldn t the controller be named ContactController?</span></span>

<span data-ttu-id="49406-424">この問題は、簡単に修正します。</span><span class="sxs-lookup"><span data-stu-id="49406-424">This issue is easy enough to fix.</span></span> <span data-ttu-id="49406-425">最初に、Home コント ローラーの名前をリファクターする必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-425">First, we need to refactor the name of the Home controller.</span></span> <span data-ttu-id="49406-426">HomeController クラスにでは、Visual Studio コード エディターを開き、クラスの名前を右クリックしてメニュー オプションを選択**名前の変更のリファクタリング**します。</span><span class="sxs-lookup"><span data-stu-id="49406-426">Open the HomeController class in the Visual Studio Code Editor, right click the name of the class and select the menu option **Refactor, Rename**.</span></span> <span data-ttu-id="49406-427">このメニュー オプションを選択すると、名前の変更 ダイアログが開きます。</span><span class="sxs-lookup"><span data-stu-id="49406-427">Selecting this menu option opens the Rename dialog.</span></span>


<span data-ttu-id="49406-428">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)</span><span class="sxs-lookup"><span data-stu-id="49406-428">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)</span></span>

<span data-ttu-id="49406-429">**図 23**: コント ローラーの名前のリファクタリング ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image46.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-429">**Figure 23**: Refactoring a controller name ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image46.png))</span></span>


<span data-ttu-id="49406-430">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)</span><span class="sxs-lookup"><span data-stu-id="49406-430">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)</span></span>

<span data-ttu-id="49406-431">**図 24**: 名前の変更 ダイアログを使用して ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image48.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-431">**Figure 24**: Using the Rename dialog ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image48.png))</span></span>


<span data-ttu-id="49406-432">コント ローラー クラスの名前を変更すると、Visual Studio にも、Views フォルダー内のフォルダーの名前が更新されます。</span><span class="sxs-lookup"><span data-stu-id="49406-432">If you rename your controller class, Visual Studio will update the name of the folder in the Views folder as well.</span></span> <span data-ttu-id="49406-433">\Views\Contact フォルダーには、visual Studio は \Views\Home フォルダーを変更します。</span><span class="sxs-lookup"><span data-stu-id="49406-433">Visual Studio will rename the \Views\Home folder to the \Views\Contact folder.</span></span>

<span data-ttu-id="49406-434">この変更を行った後、アプリケーションは、ホーム コント ローラーを不要になったがあります。</span><span class="sxs-lookup"><span data-stu-id="49406-434">After you make this change, your application will no longer have a Home controller.</span></span> <span data-ttu-id="49406-435">アプリケーションを実行すると、図 25 のエラー ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="49406-435">When you run your application, you'll get the error page in Figure 25.</span></span>


<span data-ttu-id="49406-436">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)</span><span class="sxs-lookup"><span data-stu-id="49406-436">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)</span></span>

<span data-ttu-id="49406-437">**図 25**: 既定のコント ローラーなし ([フルサイズの画像を表示する をクリックします](iteration-1-create-the-application-cs/_static/image50.png))。</span><span class="sxs-lookup"><span data-stu-id="49406-437">**Figure 25**: No default controller ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image50.png))</span></span>


<span data-ttu-id="49406-438">Home コント ローラーではなく、連絡先のコント ローラーを使用する Global.asax ファイル内の既定のルートを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-438">We need to update the default route in the Global.asax file to use the Contact controller instead of the Home controller.</span></span> <span data-ttu-id="49406-439">Global.asax ファイルを開き、既定のルート (10 のリストを参照) で使用される既定のコント ローラーを変更します。</span><span class="sxs-lookup"><span data-stu-id="49406-439">Open the Global.asax file and modify the default controller used by the default route (see Listing 10).</span></span>

<span data-ttu-id="49406-440">**10 - Global.asax.cs の一覧を表示します。**</span><span class="sxs-lookup"><span data-stu-id="49406-440">**Listing 10 - Global.asax.cs**</span></span>

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample10.cs)]

<span data-ttu-id="49406-441">これらの変更を行った後、連絡先マネージャーが正しく実行されます。</span><span class="sxs-lookup"><span data-stu-id="49406-441">After you make these changes, the Contact Manager will run correctly.</span></span> <span data-ttu-id="49406-442">ここで、既定のコント ローラーとして、連絡先のコント ローラー クラスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="49406-442">Now, it will use the Contact controller class as the default controller.</span></span>

## <a name="summary"></a><span data-ttu-id="49406-443">まとめ</span><span class="sxs-lookup"><span data-stu-id="49406-443">Summary</span></span>

<span data-ttu-id="49406-444">この最初のイテレーションで作成した基本的な Contact Manager アプリケーションをすばやく可能です。</span><span class="sxs-lookup"><span data-stu-id="49406-444">In this first iteration, we created a basic Contact Manager application in the fastest way possible.</span></span> <span data-ttu-id="49406-445">このコント ローラーとビューの最初のコードを自動的に生成する Visual Studio を活用しました。</span><span class="sxs-lookup"><span data-stu-id="49406-445">We took advantage of Visual Studio to generate the initial code for our controllers and views automatically.</span></span> <span data-ttu-id="49406-446">データベース モデル クラスを自動的に生成する Entity Framework の利点をしました。</span><span class="sxs-lookup"><span data-stu-id="49406-446">We also took advantage of the Entity Framework to generate our database model classes automatically.</span></span>

<span data-ttu-id="49406-447">現時点では、一覧表示、作成、編集、および Contact Manager アプリケーションに連絡先レコードを削除をことができますします。</span><span class="sxs-lookup"><span data-stu-id="49406-447">Currently, we can list, create, edit, and delete contact records with the Contact Manager application.</span></span> <span data-ttu-id="49406-448">つまり、すべてのデータベース駆動型 web アプリケーションで必要な基本的なデータベース操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="49406-448">In other words, we can perform all of the basic database operations required by a database-driven web application.</span></span>

<span data-ttu-id="49406-449">残念ながら、このアプリケーションでは、いくつかの問題があります。</span><span class="sxs-lookup"><span data-stu-id="49406-449">Unfortunately, our application has some problems.</span></span> <span data-ttu-id="49406-450">最初はこれを認めるお、Contact Manager アプリケーションが最も魅力的なアプリケーションではありません。</span><span class="sxs-lookup"><span data-stu-id="49406-450">First and I hesitate to admit this, the Contact Manager application is not the most attractive application.</span></span> <span data-ttu-id="49406-451">何らかのデザイン作業が必要があります。</span><span class="sxs-lookup"><span data-stu-id="49406-451">It needs some design work.</span></span> <span data-ttu-id="49406-452">次のイテレーションでは、既定ビュー マスター ページと、アプリケーションの外観を向上させるためにカスケード スタイル シートを変更した方法を紹介します。</span><span class="sxs-lookup"><span data-stu-id="49406-452">In the next iteration, we'll look at how we can change the default view master page and cascading style sheet to improve the appearance of the application.</span></span>

<span data-ttu-id="49406-453">次に、すべてのフォーム検証を実装していません。</span><span class="sxs-lookup"><span data-stu-id="49406-453">Second, we have not implemented any form validation.</span></span> <span data-ttu-id="49406-454">たとえば、何もない、フォームのフィールドのいずれかの値を入力せずにお問い合わせフォームを作成するを送信することを防止するためです。</span><span class="sxs-lookup"><span data-stu-id="49406-454">For example, there is nothing to prevent you from submitting the Create contact form without entering values for any of the form fields.</span></span> <span data-ttu-id="49406-455">さらに、無効な電話番号と電子メール アドレスを入力することができます。</span><span class="sxs-lookup"><span data-stu-id="49406-455">Furthermore, you can enter invalid phone numbers and email addresses.</span></span> <span data-ttu-id="49406-456">繰り返し #3 で、フォーム検証の問題に取り組むまずします。</span><span class="sxs-lookup"><span data-stu-id="49406-456">We start to tackle the problem of form validation in iteration #3.</span></span>

<span data-ttu-id="49406-457">最後に、および最も重要なは、Contact Manager アプリケーションの現在のイテレーションを簡単に変更または維持することはできません。</span><span class="sxs-lookup"><span data-stu-id="49406-457">Finally, and most importantly, the current iteration of the Contact Manager application cannot be easily modified or maintained.</span></span> <span data-ttu-id="49406-458">たとえば、データベース アクセス ロジックには、コント ローラー アクションに右が組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="49406-458">For example, the database access logic is baked right into the controller actions.</span></span> <span data-ttu-id="49406-459">つまり、これらのコント ローラーを変更することがなく、データ アクセス コードを修正することはできません。</span><span class="sxs-lookup"><span data-stu-id="49406-459">This means that we cannot modify our data access code without modifying our controllers.</span></span> <span data-ttu-id="49406-460">後のイテレーションで実装できる、連絡先マネージャーに変更に柔軟に対応するソフトウェア設計のパターンについて説明します。</span><span class="sxs-lookup"><span data-stu-id="49406-460">In later iterations, we explore software design patterns that we can implement to make the Contact Manager more resilient to change.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="49406-461">次へ</span><span class="sxs-lookup"><span data-stu-id="49406-461">Next</span></span>](iteration-2-make-the-application-look-nice-cs.md)
