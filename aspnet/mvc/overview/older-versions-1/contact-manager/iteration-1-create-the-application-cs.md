---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
title: イテレーション 1 – (c#) アプリケーションの作成 |Microsoft ドキュメント
author: microsoft
description: '最初のイテレーションでお連絡先のマネージャー最も簡単な方法で可能なを作成します。 基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および D.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: db0f160b-901c-46d3-865e-7ab6cd4ed68d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
msc.type: authoredcontent
ms.openlocfilehash: 30f626511164363fea2195a05e73aeee5764933b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877386"
---
<a name="iteration-1--create-the-application-c"></a><span data-ttu-id="a0ced-104">イテレーション 1 – (c#) アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-104">Iteration #1 – Create the Application (C#)</span></span>
====================
<span data-ttu-id="a0ced-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a0ced-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="a0ced-106">コードをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="a0ced-106">Download Code</span></span>](iteration-1-create-the-application-cs/_static/contactmanager_1_cs1.zip)

> <span data-ttu-id="a0ced-107">最初のイテレーションでお連絡先のマネージャー最も簡単な方法で可能なを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-107">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="a0ced-108">基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。</span><span class="sxs-lookup"><span data-stu-id="a0ced-108">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a><span data-ttu-id="a0ced-109">連絡先管理 ASP.NET MVC アプリケーション (VB) のビルド</span><span class="sxs-lookup"><span data-stu-id="a0ced-109">Building a Contact Management ASP.NET MVC Application (VB)</span></span>

<span data-ttu-id="a0ced-110">この一連のチュートリアルは、連絡先管理アプリケーション全体が開始されてから完了するを構築します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-110">In this series of tutorials, we build an entire Contact Management application from start to finish.</span></span> <span data-ttu-id="a0ced-111">お問い合わせのマネージャー アプリケーションでは、人のユーザーの一覧については使用すると、連絡先情報の名前、電話番号、電子メール アドレスを格納できます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-111">The Contact Manager application enables you to store contact information - names, phone numbers and email addresses - for a list of people.</span></span>

<span data-ttu-id="a0ced-112">私たちは、複数のイテレーションにおける、アプリケーションを構築します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-112">We build the application over multiple iterations.</span></span> <span data-ttu-id="a0ced-113">各イテレーションで、アプリケーション、徐々 に向上します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-113">With each iteration, we gradually improve the application.</span></span> <span data-ttu-id="a0ced-114">この複数のイテレーション アプローチの目的は、各変更の理由を理解するためです。</span><span class="sxs-lookup"><span data-stu-id="a0ced-114">The goal of this multiple iteration approach is to enable you to understand the reason for each change.</span></span>

- <span data-ttu-id="a0ced-115">イテレーション 1 には、アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-115">Iteration #1 - Create the application.</span></span> <span data-ttu-id="a0ced-116">最初のイテレーションでお連絡先のマネージャー最も簡単な方法で可能なを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-116">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="a0ced-117">基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。</span><span class="sxs-lookup"><span data-stu-id="a0ced-117">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>

- <span data-ttu-id="a0ced-118">イテレーション 2 では、素敵に見えるアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-118">Iteration #2 - Make the application look nice.</span></span> <span data-ttu-id="a0ced-119">このイテレーションで、アプリケーションの外観を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。</span><span class="sxs-lookup"><span data-stu-id="a0ced-119">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>

- <span data-ttu-id="a0ced-120">イテレーション 3 - フォーム検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-120">Iteration #3 - Add form validation.</span></span> <span data-ttu-id="a0ced-121">3 番目のイテレーションは、基本フォーム検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-121">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="a0ced-122">おは人が必要なフォームのフィールドを完了しなくても、フォームを送信することを防ぐ。</span><span class="sxs-lookup"><span data-stu-id="a0ced-122">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="a0ced-123">私たちも電子メール アドレスと電話番号を検証します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-123">We also validate email addresses and phone numbers.</span></span>

- <span data-ttu-id="a0ced-124">4: イテレーションは、疎結合アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-124">Iteration #4 - Make the application loosely coupled.</span></span> <span data-ttu-id="a0ced-125">この 3 番目のイテレーションで利用の保守し、連絡先のマネージャー アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかのです。</span><span class="sxs-lookup"><span data-stu-id="a0ced-125">In this third iteration, we take advantage of several software design patterns to make it easier to maintain and modify the Contact Manager application.</span></span> <span data-ttu-id="a0ced-126">たとえば、リポジトリ パターンと依存関係の挿入のパターンを使用するようにアプリケーションをリファクターします。</span><span class="sxs-lookup"><span data-stu-id="a0ced-126">For example, we refactor our application to use the Repository pattern and the Dependency Injection pattern.</span></span>

- <span data-ttu-id="a0ced-127">イテレーション #5 - 単体テストを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-127">Iteration #5 - Create unit tests.</span></span> <span data-ttu-id="a0ced-128">5 番目のイテレーションでおやすく、アプリケーションを維持し、単体テストを追加して変更できます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-128">In the fifth iteration, we make our application easier to maintain and modify by adding unit tests.</span></span> <span data-ttu-id="a0ced-129">データ モデル クラスを模擬表示し、コント ローラーと検証ロジックの単体テストをビルドします。</span><span class="sxs-lookup"><span data-stu-id="a0ced-129">We mock our data model classes and build unit tests for our controllers and validation logic.</span></span>

- <span data-ttu-id="a0ced-130">イテレーション 6 - テスト駆動開発を使用します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-130">Iteration #6 - Use test-driven development.</span></span> <span data-ttu-id="a0ced-131">この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加おには、まず単体テストを記述し、単体テストに対してコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-131">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="a0ced-132">このイテレーションは、連絡先グループを追加します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-132">In this iteration, we add contact groups.</span></span>

- <span data-ttu-id="a0ced-133">イテレーション #7 - Ajax 機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-133">Iteration #7 - Add Ajax functionality.</span></span> <span data-ttu-id="a0ced-134">7 番目のイテレーションでお、応答性およびパフォーマンスの向上、アプリケーションの Ajax のサポートを追加することで。</span><span class="sxs-lookup"><span data-stu-id="a0ced-134">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>

## <a name="this-iteration"></a><span data-ttu-id="a0ced-135">このイテレーション</span><span class="sxs-lookup"><span data-stu-id="a0ced-135">This Iteration</span></span>

<span data-ttu-id="a0ced-136">この最初のイテレーションでは、基本的なアプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="a0ced-136">In this first iteration, we build the basic application.</span></span> <span data-ttu-id="a0ced-137">目標は、可能な限り最速かつ最も簡単な方法で連絡先のマネージャーを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-137">The goal is to build the Contact Manager in the fastest and simplest way possible.</span></span> <span data-ttu-id="a0ced-138">後期のイテレーションでは、アプリケーションの設計を向上します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-138">In later iterations, we improve the design of the application.</span></span>

<span data-ttu-id="a0ced-139">連絡先のマネージャー アプリケーションは、基本的なデータベースに基づくアプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="a0ced-139">The Contact Manager application is a basic database-driven application.</span></span> <span data-ttu-id="a0ced-140">アプリケーションを使用して、新しい連絡先を作成、既存の連絡先を編集および連絡先を削除することができます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-140">You can use the application to create new contacts, edit existing contacts, and delete contacts.</span></span>

<span data-ttu-id="a0ced-141">このイテレーションは、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-141">In this iteration, we complete the following steps:</span></span>

1. <span data-ttu-id="a0ced-142">ASP.NET MVC アプリケーション</span><span class="sxs-lookup"><span data-stu-id="a0ced-142">ASP.NET MVC application</span></span>
2. <span data-ttu-id="a0ced-143">データベースを作成して、連絡先を保存</span><span class="sxs-lookup"><span data-stu-id="a0ced-143">Create a database to store our contacts</span></span>
3. <span data-ttu-id="a0ced-144">Microsoft の Entity Framework と、データベースのモデル クラスを生成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-144">Generate a model class for our database with the Microsoft Entity Framework</span></span>
4. <span data-ttu-id="a0ced-145">コント ローラーのアクションとすべてのデータベースに連絡先の一覧を表示することにより、ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-145">Create a controller action and view that enables us to list all of the contacts in the database</span></span>
5. <span data-ttu-id="a0ced-146">コント ローラーのアクションと、データベースに新しい連絡先を作成することにより、ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-146">Create controller actions and a view that enables us to create a new contact in the database</span></span>
6. <span data-ttu-id="a0ced-147">コント ローラーのアクションと、データベース内の既存の連絡先を編集することにより、ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-147">Create controller actions and a view that enables us to edit an existing contact in the database</span></span>
7. <span data-ttu-id="a0ced-148">コント ローラーのアクションと、データベース内の既存の連絡先を削除することにより、ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-148">Create controller actions and a view that enables us to delete an existing contact in the database</span></span>

## <a name="software-prerequisites"></a><span data-ttu-id="a0ced-149">ソフトウェアの前提条件</span><span class="sxs-lookup"><span data-stu-id="a0ced-149">Software Prerequisites</span></span>

<span data-ttu-id="a0ced-150">ASP.NET MVC アプリケーションでは、Visual Studio 2008 または Visual Web Developer 2008 (Visual Web Developer は、無料版の Visual Studio のすべての Visual Studio の高度な機能は含まれません)、コンピューターにインストールが必要です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-150">In ASP.NET MVC applications, you must have either Visual Studio 2008 or Visual Web Developer 2008 installed on your computer (Visual Web Developer is a free version of Visual Studio that does not include all of the advanced features of Visual Studio).</span></span> <span data-ttu-id="a0ced-151">次のアドレスからは、Visual Studio 2008 の試用版または Visual Web Developer のいずれかをダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-151">You can download either the trial version of Visual Studio 2008 or Visual Web Developer from the following address:</span></span>

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> <span data-ttu-id="a0ced-152">Visual Web Developer と ASP.NET MVC アプリケーションの場合は、Visual Web Developer Service Pack 1 インストールが必要です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-152">For ASP.NET MVC applications with Visual Web Developer, you must have Visual Web Developer Service Pack 1 installed.</span></span> <span data-ttu-id="a0ced-153">Service Pack 1 なしには、Web アプリケーション プロジェクトを作成することはできません。</span><span class="sxs-lookup"><span data-stu-id="a0ced-153">Without Service Pack 1, you cannot create Web Application Projects.</span></span>


<span data-ttu-id="a0ced-154">ASP.NET MVC フレームワーク。</span><span class="sxs-lookup"><span data-stu-id="a0ced-154">ASP.NET MVC framework.</span></span> <span data-ttu-id="a0ced-155">ASP.NET MVC フレームワークは、次のアドレスからダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-155">You can download the ASP.NET MVC framework from the following address:</span></span>

[https://www.asp.net/mvc](../../../index.md)

<span data-ttu-id="a0ced-156">このチュートリアルでは、Microsoft の Entity Framework を使用して、データベースにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="a0ced-156">In this tutorial, we use the Microsoft Entity Framework to access a database.</span></span> <span data-ttu-id="a0ced-157">Entity Framework は、.NET Framework 3.5 Service Pack 1 に含まれています。</span><span class="sxs-lookup"><span data-stu-id="a0ced-157">The Entity Framework is included with .NET Framework 3.5 Service Pack 1.</span></span> <span data-ttu-id="a0ced-158">この service pack は、次の場所からダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-158">You can download this service pack from the following location:</span></span>

[<span data-ttu-id="a0ced-159">https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en</span><span class="sxs-lookup"><span data-stu-id="a0ced-159">https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en</span></span>](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

<span data-ttu-id="a0ced-160">これらのダウンロードを 1 つずつのそれぞれを実行する代わりに、Web Platform Installer (Web PI) の利点を実行できます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-160">As an alternative to performing each of these downloads one by one, you can take advantage of the Web Platform Installer (Web PI).</span></span> <span data-ttu-id="a0ced-161">Web PI では、次のアドレスからダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-161">You can download the Web PI from the following address:</span></span>

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a><span data-ttu-id="a0ced-162">ASP.NET MVC Project</span><span class="sxs-lookup"><span data-stu-id="a0ced-162">ASP.NET MVC Project</span></span>

<span data-ttu-id="a0ced-163">ASP.NET MVC Web アプリケーション プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="a0ced-163">ASP.NET MVC Web Application Project.</span></span> <span data-ttu-id="a0ced-164">Visual Studio を起動し、メニュー オプションを選択**ファイル]、[新しいプロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-164">Launch Visual Studio and select the menu option **File, New Project**.</span></span> <span data-ttu-id="a0ced-165">**新しいプロジェクト**ダイアログが表示されます (図 1)。</span><span class="sxs-lookup"><span data-stu-id="a0ced-165">The **New Project** dialog appears (see Figure 1).</span></span> <span data-ttu-id="a0ced-166">選択、 **Web**プロジェクトの種類と**ASP.NET MVC Web アプリケーション**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="a0ced-166">Select the **Web** project type and the **ASP.NET MVC Web Application** template.</span></span> <span data-ttu-id="a0ced-167">新しいプロジェクトの名前を付けます*ContactManager*し、[ok] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="a0ced-167">Name your new project *ContactManager* and click the OK button.</span></span>


<span data-ttu-id="a0ced-168">上部にあるドロップダウン リストから選択した .NET Framework 3.5 があることを確認の右、**新しいプロジェクト**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="a0ced-168">Make sure that you have .NET Framework 3.5 selected from the dropdown list at the top right of the **New Project** dialog.</span></span> <span data-ttu-id="a0ced-169">それ以外の場合、t が勝利した ASP.NET MVC Web アプリケーション テンプレートが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-169">Otherwise, the ASP.NET MVC Web Application template won t appear.</span></span>


<span data-ttu-id="a0ced-170">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-170">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)</span></span>

<span data-ttu-id="a0ced-171">**図 01**: [新しいプロジェクト] ダイアログ ボックス ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-171">**Figure 01**: The New Project dialog([Click to view full-size image](iteration-1-create-the-application-cs/_static/image2.png))</span></span>


<span data-ttu-id="a0ced-172">ASP.NET MVC アプリケーション、**単体テスト プロジェクトの作成**ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-172">ASP.NET MVC application, the **Create Unit Test Project** dialog appears.</span></span> <span data-ttu-id="a0ced-173">このダイアログを使用して、作成し、ASP.NET MVC アプリケーションを作成するときに、単体テスト プロジェクトをソリューションに追加することを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-173">You can use this dialog to indicate that you want to create and add a unit test project to your solution when you create your ASP.NET MVC application.</span></span> <span data-ttu-id="a0ced-174">T が勝利したことがイテレーションで単体テストをビルドするには、オプションを選択する必要があります**はい、単体テスト プロジェクトを作成**後のイテレーションで単体テストを追加する予定なのでです。</span><span class="sxs-lookup"><span data-stu-id="a0ced-174">Although we won t be building unit tests in this iteration, you should select the option **Yes, create a unit test project** because we plan to add unit tests in a later iteration.</span></span> <span data-ttu-id="a0ced-175">最初に、新しい ASP.NET MVC プロジェクトを作成するときにテスト プロジェクトを追加することは、ASP.NET MVC プロジェクトが作成された後にテスト プロジェクトを追加するよりもはるかに簡単です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-175">Adding a Test project when you first create a new ASP.NET MVC project is much easier than adding a Test project after the ASP.NET MVC project has been created.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a0ced-176">Visual Web Developer では、テスト プロジェクトはサポートされていません、Visual Web Developer を使用するときに単体テスト プロジェクトの作成 ダイアログが得られなかった。</span><span class="sxs-lookup"><span data-stu-id="a0ced-176">Because Visual Web Developer does not support Test projects, you do not get the Create Unit Test Project dialog when using Visual Web Developer.</span></span>


<span data-ttu-id="a0ced-177">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-177">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)</span></span>

<span data-ttu-id="a0ced-178">**図 02**:、単体テスト プロジェクトの作成 ダイアログ ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-178">**Figure 02**: The Create Unit Test Project dialog ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image4.png))</span></span>


<span data-ttu-id="a0ced-179">ASP.NET MVC アプリケーションは、Visual Studio ソリューション エクスプ ローラー ウィンドウに表示されます (図 3 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a0ced-179">ASP.NET MVC application appears in the Visual Studio Solution Explorer window (see Figure 3).</span></span> <span data-ttu-id="a0ced-180">メニュー オプションを選択してこのウィンドウを開くことができますし、表示されないにソリューション エクスプ ローラー ウィンドウが表示される場合**表示]、[ソリューション エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-180">If you don t see the Solution Explorer window then you can open this window by selecting the menu option **View, Solution Explorer**.</span></span> <span data-ttu-id="a0ced-181">2 つのプロジェクトがソリューションに含まれることに注意してください: ASP.NET MVC プロジェクトとテストのプロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="a0ced-181">Notice that the solution contains two projects: the ASP.NET MVC project and the Test project.</span></span> <span data-ttu-id="a0ced-182">ContactManager は ASP.NET MVC プロジェクトの名前し、ContactManager.Tests はテスト プロジェクトの名前します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-182">The ASP.NET MVC project is named ContactManager and the Test project is named ContactManager.Tests.</span></span>


<span data-ttu-id="a0ced-183">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-183">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)</span></span>

<span data-ttu-id="a0ced-184">**図 03**: ソリューション エクスプ ローラー ウィンドウ ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-184">**Figure 03**: The Solution Explorer window ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image6.png))</span></span>


## <a name="deleting-the-project-sample-files"></a><span data-ttu-id="a0ced-185">プロジェクトのサンプル ファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-185">Deleting the Project Sample Files</span></span>

<span data-ttu-id="a0ced-186">ASP.NET MVC プロジェクト テンプレートには、コント ローラーとビューのサンプル ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a0ced-186">The ASP.NET MVC project template includes sample files for controllers and views.</span></span> <span data-ttu-id="a0ced-187">新しい ASP.NET MVC アプリケーションを作成する前に、これらのファイルを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-187">Before creating a new ASP.NET MVC application, you should delete these files.</span></span> <span data-ttu-id="a0ced-188">ソリューション エクスプ ローラー ウィンドウでファイルおよびフォルダーを削除するには、ファイルまたはフォルダーを右クリックし、メニュー オプションを選択して**削除**です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-188">You can delete files and folders in the Solution Explorer window by right-clicking a file or folder and selecting the menu option **Delete**.</span></span>

<span data-ttu-id="a0ced-189">ASP.NET MVC プロジェクトから、次のファイルを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-189">You need to delete the following files from the ASP.NET MVC project:</span></span>

- <span data-ttu-id="a0ced-190">\Controllers\HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="a0ced-190">\Controllers\HomeController.cs</span></span>

- <span data-ttu-id="a0ced-191">\Views\Home\About.aspx</span><span class="sxs-lookup"><span data-stu-id="a0ced-191">\Views\Home\About.aspx</span></span>

- <span data-ttu-id="a0ced-192">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="a0ced-192">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="a0ced-193">また、テスト プロジェクトから次のファイルを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-193">And, you need to delete the following file from the Test project:</span></span>

<span data-ttu-id="a0ced-194">\Controllers\HomeControllerTest.cs</span><span class="sxs-lookup"><span data-stu-id="a0ced-194">\Controllers\HomeControllerTest.cs</span></span>

## <a name="creating-the-database"></a><span data-ttu-id="a0ced-195">データベースの作成</span><span class="sxs-lookup"><span data-stu-id="a0ced-195">Creating the Database</span></span>

<span data-ttu-id="a0ced-196">連絡先のマネージャー アプリケーションは、データベースに基づく web アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="a0ced-196">The Contact Manager application is a database-driven web application.</span></span> <span data-ttu-id="a0ced-197">連絡先情報を格納するデータベースを使用しています。</span><span class="sxs-lookup"><span data-stu-id="a0ced-197">We use a database to store the contact information.</span></span>

<span data-ttu-id="a0ced-198">Microsoft SQL Server、Oracle、MySQL、および IBM DB2 データベースを含む、最新のデータベースと ASP.NET MVC フレームワーク。</span><span class="sxs-lookup"><span data-stu-id="a0ced-198">The ASP.NET MVC framework with any modern database including Microsoft SQL Server, Oracle, MySQL, and IBM DB2 databases.</span></span> <span data-ttu-id="a0ced-199">このチュートリアルでは、Microsoft SQL Server データベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-199">In this tutorial, we use a Microsoft SQL Server database.</span></span> <span data-ttu-id="a0ced-200">Visual Studio をインストールするときに、無料版の Microsoft SQL Server データベースである Microsoft SQL Server Express をインストールするオプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-200">When you install Visual Studio, you are provided with the option of installing Microsoft SQL Server Express which is a free version of the Microsoft SQL Server database.</span></span>

<span data-ttu-id="a0ced-201">アプリケーションを右クリックして新しいデータベースを作成\_メニュー オプションを選択して、ソリューション エクスプ ローラー ウィンドウで、データ フォルダー**追加、新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-201">Create a new database by right-clicking the App\_Data folder in the Solution Explorer window and selecting the menu option **Add, New Item**.</span></span> <span data-ttu-id="a0ced-202">**新しい項目の追加**ダイアログで、選択、**データ**カテゴリおよび**SQL Server データベース**テンプレート (図 4 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a0ced-202">In the **Add New Item** dialog, select the **Data** category and the **SQL Server Database** template (see Figure 4).</span></span> <span data-ttu-id="a0ced-203">新しいデータベース ContactManagerDB.mdf 名し、[ok] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="a0ced-203">Name the new database ContactManagerDB.mdf and click the OK button.</span></span>


<span data-ttu-id="a0ced-204">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-204">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)</span></span>

<span data-ttu-id="a0ced-205">**図 04**: 新しい Microsoft SQL Server Express データベースを作成する ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-205">**Figure 04**: Creating a new Microsoft SQL Server Express database ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image8.png))</span></span>


<span data-ttu-id="a0ced-206">新しいデータベースを作成すると、アプリでに、データベースが表示されます\_ソリューション エクスプ ローラー ウィンドウで、データ フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="a0ced-206">After you create the new database, the database appears in the App\_Data folder in the Solution Explorer window.</span></span> <span data-ttu-id="a0ced-207">サーバー エクスプ ローラー ウィンドウを開き、データベースに接続する ContactManager.mdf ファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="a0ced-207">Double-click the ContactManager.mdf file to open the Server Explorer window and connect to the database.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a0ced-208">サーバー エクスプ ローラー ウィンドウには、Microsoft Visual Web Developer の場合、データベース エクスプ ローラー ウィンドウが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-208">The Server Explorer window is called the Database Explorer window in the case of Microsoft Visual Web Developer.</span></span>


<span data-ttu-id="a0ced-209">サーバー エクスプ ローラー ウィンドウを使用して、データベース テーブル、ビュー、トリガー、ストアド プロシージャなどの新しいデータベース オブジェクトを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-209">You can use the Server Explorer window to create new database objects such as database tables, views, triggers, and stored procedures.</span></span> <span data-ttu-id="a0ced-210">[テーブル] フォルダーを右クリックし、メニュー オプションを選択**新しいテーブルの追加**です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-210">Right-click the Tables folder and select the menu option **Add New Table**.</span></span> <span data-ttu-id="a0ced-211">データベースのテーブル デザイナーでは、(図 5 を参照) が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-211">The Database Table Designer appears (see Figure 5).</span></span>


<span data-ttu-id="a0ced-212">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-212">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)</span></span>

<span data-ttu-id="a0ced-213">**図 05**: データベース テーブル デザイナー ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-213">**Figure 05**: The Database Table Designer ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image10.png))</span></span>


<span data-ttu-id="a0ced-214">次の列を含むテーブルを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-214">We need to create a table that contains the following columns:</span></span>

<a id="0.1_table01"></a>


| <span data-ttu-id="a0ced-215">**列名**</span><span class="sxs-lookup"><span data-stu-id="a0ced-215">**Column Name**</span></span> | <span data-ttu-id="a0ced-216">**データ型**</span><span class="sxs-lookup"><span data-stu-id="a0ced-216">**Data Type**</span></span> | <span data-ttu-id="a0ced-217">**Null を許容します。**</span><span class="sxs-lookup"><span data-stu-id="a0ced-217">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a0ced-218">ID</span><span class="sxs-lookup"><span data-stu-id="a0ced-218">Id</span></span> | <span data-ttu-id="a0ced-219">int</span><span class="sxs-lookup"><span data-stu-id="a0ced-219">int</span></span> | <span data-ttu-id="a0ced-220">False</span><span class="sxs-lookup"><span data-stu-id="a0ced-220">false</span></span> |
| <span data-ttu-id="a0ced-221">FirstName</span><span class="sxs-lookup"><span data-stu-id="a0ced-221">FirstName</span></span> | <span data-ttu-id="a0ced-222">nvarchar(50)</span><span class="sxs-lookup"><span data-stu-id="a0ced-222">nvarchar(50)</span></span> | <span data-ttu-id="a0ced-223">False</span><span class="sxs-lookup"><span data-stu-id="a0ced-223">false</span></span> |
| <span data-ttu-id="a0ced-224">LastName</span><span class="sxs-lookup"><span data-stu-id="a0ced-224">LastName</span></span> | <span data-ttu-id="a0ced-225">nvarchar(50)</span><span class="sxs-lookup"><span data-stu-id="a0ced-225">nvarchar(50)</span></span> | <span data-ttu-id="a0ced-226">False</span><span class="sxs-lookup"><span data-stu-id="a0ced-226">false</span></span> |
| <span data-ttu-id="a0ced-227">電話番号</span><span class="sxs-lookup"><span data-stu-id="a0ced-227">Phone</span></span> | <span data-ttu-id="a0ced-228">nvarchar(50)</span><span class="sxs-lookup"><span data-stu-id="a0ced-228">nvarchar(50)</span></span> | <span data-ttu-id="a0ced-229">False</span><span class="sxs-lookup"><span data-stu-id="a0ced-229">false</span></span> |
| <span data-ttu-id="a0ced-230">電子メール</span><span class="sxs-lookup"><span data-stu-id="a0ced-230">Email</span></span> | <span data-ttu-id="a0ced-231">nvarchar(255)</span><span class="sxs-lookup"><span data-stu-id="a0ced-231">nvarchar(255)</span></span> | <span data-ttu-id="a0ced-232">False</span><span class="sxs-lookup"><span data-stu-id="a0ced-232">false</span></span> |


<span data-ttu-id="a0ced-233">最初の列、Id 列は特殊です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-233">The first column, the Id column, is special.</span></span> <span data-ttu-id="a0ced-234">Id 列と主キー列として Id 列をマークする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-234">You need to mark the Id column as an Identity column and a Primary Key column.</span></span> <span data-ttu-id="a0ced-235">列が Identity 列である列のプロパティ (図 6 の下部にあるを参照) を展開し、Identity の指定 プロパティまでスクロールを指定します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-235">You indicate that a column is an Identity column by expanding Column Properites (look at the bottom of Figure 6) and scrolling down to the Identity Specification property.</span></span> <span data-ttu-id="a0ced-236">設定、 **(Is Identity)** プロパティ値を**はい**です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-236">Set the **(Is Identity)** property to the value **Yes**.</span></span>

<span data-ttu-id="a0ced-237">主キー列として列を選択し、キーのアイコンが付いたボタンをクリックして列をマークするとします。</span><span class="sxs-lookup"><span data-stu-id="a0ced-237">You mark a column as a Primary Key column by selecting the column and clicking the button with the icon of a key.</span></span> <span data-ttu-id="a0ced-238">列が主キー列としてマークされているキーのアイコンが、列の横に表示されます (図 6 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a0ced-238">After a column is marked as a Primary Key column, an icon of a key appears next to the column (see Figure 6).</span></span>

<span data-ttu-id="a0ced-239">テーブルの作成が完了したら、新しいテーブルを保存する、保存ボタン (フロッピー ディスクのアイコンのボタン) をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a0ced-239">After you finish creating the table, click the Save button (the button with an icon of a floppy) to save the new table.</span></span> <span data-ttu-id="a0ced-240">新しいテーブルに名前を付ける*連絡先*です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-240">Give your new table the name *Contacts*.</span></span>

<span data-ttu-id="a0ced-241">連絡先データベース テーブルの作成が完了した後は、テーブルにレコードの一部を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-241">After finish creating the Contacts database table, you should add some records to the table.</span></span> <span data-ttu-id="a0ced-242">サーバー エクスプ ローラー ウィンドウで、Contacts テーブルを右クリックし、メニュー オプションを選択**テーブル データの表示**です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-242">Right-click the Contacts table in the Server Explorer window and select the menu option **Show Table Data**.</span></span> <span data-ttu-id="a0ced-243">表示されるグリッドで 1 つまたは複数の連絡先を入力します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-243">Enter one or more contacts in the grid that appears.</span></span>

## <a name="creating-the-data-model"></a><span data-ttu-id="a0ced-244">データ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-244">Creating the Data Model</span></span>

<span data-ttu-id="a0ced-245">ASP.NET MVC アプリケーションは、モデル、ビュー、およびコント ローラーで構成されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-245">The ASP.NET MVC application consists of Models, Views, and Controllers.</span></span> <span data-ttu-id="a0ced-246">前のセクションで作成した、Contacts テーブルを表すモデル クラスを作成して開始します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-246">We start by creating a Model class that represents the Contacts table that we created in the previous section.</span></span>

<span data-ttu-id="a0ced-247">このチュートリアルでは、Microsoft の Entity Framework を使用して、データベースからモデル クラスを自動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-247">In this tutorial, we use the Microsoft Entity Framework to generate a model class from the database automatically.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a0ced-248">ASP.NET MVC フレームワークは、任意の方法で Microsoft Entity Framework に関連付けられていません。</span><span class="sxs-lookup"><span data-stu-id="a0ced-248">The ASP.NET MVC framework is not tied to the Microsoft Entity Framework in any way.</span></span> <span data-ttu-id="a0ced-249">NHibernate、LINQ to SQL、または ADO.NET を含む別のデータベース アクセス テクノロジでは、ASP.NET MVC を使用できます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-249">You can use ASP.NET MVC with alternative database access technologies including NHibernate, LINQ to SQL, or ADO.NET.</span></span>


<span data-ttu-id="a0ced-250">データ モデル クラスを作成する手順に従います。</span><span class="sxs-lookup"><span data-stu-id="a0ced-250">Follow these steps to create the data model classes:</span></span>

1. <span data-ttu-id="a0ced-251">ソリューション エクスプ ローラー] ウィンドウで [モデル] フォルダーを右クリックし [**追加]、[新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-251">Right-click the Models folder in the Solution Explorer window and select **Add, New Item**.</span></span> <span data-ttu-id="a0ced-252">**新しい項目の追加**(図 6 を参照してください) ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-252">The **Add New Item** dialog appears (see Figure 6).</span></span>
2. <span data-ttu-id="a0ced-253">選択、**データ**カテゴリおよび**ADO.NET エンティティ データ モデル**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="a0ced-253">Select the **Data** category and the **ADO.NET Entity Data Model** template.</span></span> <span data-ttu-id="a0ced-254">データ モデルの名前を付けます*ContactManagerModel.edmx*  をクリックし、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="a0ced-254">Name your data model *ContactManagerModel.edmx* and click the **Add** button.</span></span> <span data-ttu-id="a0ced-255">Entity Data Model ウィザードでは、(図 7 を参照) が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-255">The Entity Data Model wizard appears (see Figure 7).</span></span>
3. <span data-ttu-id="a0ced-256">**モデルのコンテンツ**手順で、**データベースから生成**(図 7 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a0ced-256">In the **Choose Model Contents** step, select **Generate from database** (see Figure 7).</span></span>
4. <span data-ttu-id="a0ced-257">**データ接続の選択**ステップ、ContactManagerDB.mdf データベースを選択し、名前を入力*ContactManagerDBEntities*のエンティティの接続設定 (図 8 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a0ced-257">In the **Choose Your Data Connection** step, select the ContactManagerDB.mdf database and enter the name *ContactManagerDBEntities* for the Entity Connection Settings (see Figure 8).</span></span>
5. <span data-ttu-id="a0ced-258">**データベース オブジェクトの選択**手順、テーブル (図 9 を参照してください) というラベルの付いたチェック ボックスを選択します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-258">In the **Choose Your Database Objects** step, select the checkbox labeled Tables (see Figure 9).</span></span> <span data-ttu-id="a0ced-259">データ モデル (は 1 つだけで、Contacts テーブル)、データベースに格納されているすべてのテーブルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-259">The data model will include all tables contained in your database (there is just one, the Contacts table).</span></span> <span data-ttu-id="a0ced-260">名前空間を入力*モデル*です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-260">Enter the namespace *Models*.</span></span> <span data-ttu-id="a0ced-261">ウィザードを完了するには、[完了] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a0ced-261">Click the Finish button to complete the wizard.</span></span>


<span data-ttu-id="a0ced-262">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-262">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)</span></span>

<span data-ttu-id="a0ced-263">**図 06**: [新しい項目の追加] ダイアログ ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-263">**Figure 06**: The Add New Item dialog ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image12.png))</span></span>


<span data-ttu-id="a0ced-264">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-264">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)</span></span>

<span data-ttu-id="a0ced-265">**図 07**: モデルのコンテンツの選択 ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-265">**Figure 07**: Choose Model Contents ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image14.png))</span></span>


<span data-ttu-id="a0ced-266">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-266">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)</span></span>

<span data-ttu-id="a0ced-267">**図 08**: データ接続の選択 ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image16.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-267">**Figure 08**: Choose Your Data Connection ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image16.png))</span></span>


<span data-ttu-id="a0ced-268">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-268">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)</span></span>

<span data-ttu-id="a0ced-269">**図 09**: データベース オブジェクトの選択 ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-269">**Figure 09**: Choose Your Database Objects ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image18.png))</span></span>


<span data-ttu-id="a0ced-270">Entity Data Model ウィザードを完了すると、Entity Data Model デザイナーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-270">After you complete the Entity Data Model Wizard, the Entity Data Model Designer appears.</span></span> <span data-ttu-id="a0ced-271">デザイナーでは、モデル化されている各テーブルに対応するクラスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-271">The designer displays a class that corresponds to each table being modeled.</span></span> <span data-ttu-id="a0ced-272">連絡先をという名前の 1 つのクラスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-272">You should see one class named Contacts.</span></span>

<span data-ttu-id="a0ced-273">Entity Data Model ウィザードでは、データベース テーブル名に基づくクラス名を生成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-273">The Entity Data Model wizard generates class names based on database table names.</span></span> <span data-ttu-id="a0ced-274">ほとんどの場合、ウィザードによって生成されたクラスの名前を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-274">You almost always need to change the name of the class generated by the wizard.</span></span> <span data-ttu-id="a0ced-275">デザイナーで連絡先クラスを右クリックし、メニュー オプションを選択**の名前を変更**です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-275">Right-click the Contacts class in the designer and select the menu option **Rename**.</span></span> <span data-ttu-id="a0ced-276">(単数) の連絡先に連絡先 (複数) から、クラスの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-276">Change the name of the class from Contacts (plural) to Contact (singular).</span></span> <span data-ttu-id="a0ced-277">クラス名を変更した後、クラス図 10 のようになります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-277">After you change the class name, the class should appear like Figure 10.</span></span>


<span data-ttu-id="a0ced-278">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-278">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)</span></span>

<span data-ttu-id="a0ced-279">**図 10**: 連絡先クラス ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image20.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-279">**Figure 10**: The Contact class ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image20.png))</span></span>


<span data-ttu-id="a0ced-280">この時点では、このデータベース モデルを作成しました。</span><span class="sxs-lookup"><span data-stu-id="a0ced-280">At this point, we have created our database model.</span></span> <span data-ttu-id="a0ced-281">連絡先クラスを使用して、データベース内の特定の連絡先レコードを表すおできます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-281">We can use the Contact class to represent a particular contact record in our database.</span></span>

## <a name="creating-the-home-controller"></a><span data-ttu-id="a0ced-282">Home コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-282">Creating the Home Controller</span></span>

<span data-ttu-id="a0ced-283">次の手順では、Home コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-283">The next step is to create our Home controller.</span></span> <span data-ttu-id="a0ced-284">Home コント ローラーは、ASP.NET MVC アプリケーションで呼び出される既定のコント ローラーです。</span><span class="sxs-lookup"><span data-stu-id="a0ced-284">The Home controller is the default controller invoked in an ASP.NET MVC application.</span></span>

<span data-ttu-id="a0ced-285">Home コント ローラー クラスを作成するには、ソリューション エクスプ ローラー ウィンドウで、Controllers フォルダーを右クリックし、メニュー オプションを選択する**追加、コント ローラー** (図 11 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a0ced-285">Create the Home controller class by right-clicking the Controllers folder in the Solution Explorer window and selecting the menu option **Add, Controller** (see Figure 11).</span></span> <span data-ttu-id="a0ced-286">チェック ボックスに注意してください**アクション メソッドの作成、更新、および詳細シナリオを追加**です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-286">Notice the checkbox labeled **Add action methods for Create, Update, and Details scenarios**.</span></span> <span data-ttu-id="a0ced-287">クリックする前に、このチェック ボックスがオンになっていることを確認、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="a0ced-287">Make sure that this checkbox is checked before clicking the **Add** button.</span></span>


<span data-ttu-id="a0ced-288">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-288">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)</span></span>

<span data-ttu-id="a0ced-289">**図 11**: Home コント ローラーを追加する ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image22.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-289">**Figure 11**: Adding the Home controller ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image22.png))</span></span>


<span data-ttu-id="a0ced-290">Home コント ローラーを作成するときに、1 のリストにクラスを取得します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-290">When you create the Home controller, you get the class in Listing 1.</span></span>

<span data-ttu-id="a0ced-291">**1 - controllers \homecontroller.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a0ced-291">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample1.cs)]

## <a name="listing-the-contacts"></a><span data-ttu-id="a0ced-292">連絡先の一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-292">Listing the Contacts</span></span>

<span data-ttu-id="a0ced-293">連絡先データベース テーブルにレコードを表示するのには、Index() アクションと、インデックス ビューを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-293">In order to display the records in the Contacts database table, we need to create an Index() action and an Index view.</span></span>

<span data-ttu-id="a0ced-294">Home コント ローラーには、既に Index() アクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a0ced-294">The Home controller already contains an Index() action.</span></span> <span data-ttu-id="a0ced-295">このメソッドを一覧表示する 2 のように変更が必要です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-295">We need to modify this method so that it looks like Listing 2.</span></span>

<span data-ttu-id="a0ced-296">**2 - controllers \homecontroller.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a0ced-296">**Listing 2 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample2.cs)]

<span data-ttu-id="a0ced-297">通知を一覧表示する 2 で、ホーム コント ローラー クラスにというプライベート フィールドが含まれている\_エンティティです。</span><span class="sxs-lookup"><span data-stu-id="a0ced-297">Notice that the Home controller class in Listing 2 contains a private field named \_entities.</span></span> <span data-ttu-id="a0ced-298">\_エンティティ フィールドは、データ モデルからエンティティを表します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-298">The \_entities field represents the entities from the data model.</span></span> <span data-ttu-id="a0ced-299">使用して、\_エンティティがデータベースと通信するフィールドです。</span><span class="sxs-lookup"><span data-stu-id="a0ced-299">We use the \_entities field to communicate with the database.</span></span>

<span data-ttu-id="a0ced-300">Index() メソッドでは、連絡先データベース テーブルからすべての連絡先を表すビューを返します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-300">The Index() method returns a view that represents all of the contacts from the Contacts database table.</span></span> <span data-ttu-id="a0ced-301">式\_エンティティです。ContactSet.ToList() は、ジェネリック リストとして連絡先の一覧を返します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-301">The expression \_entities.ContactSet.ToList() returns the list of contacts as a generic list.</span></span>

<span data-ttu-id="a0ced-302">これまで、インデックスのコント ローラーを作成したら次に、インデックス ビューを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-302">Now that we ve created the Index controller, we next need to create the Index view.</span></span> <span data-ttu-id="a0ced-303">インデックス ビューを作成する前に、メニュー オプションを選択してアプリケーションをコンパイルします**ビルド、ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-303">Before creating the Index view, compile your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="a0ced-304">常に表示されるモデル クラスの一覧の順序でビューを追加する前にプロジェクトをコンパイルする必要があります、**ビューの追加**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="a0ced-304">You should always compile your project before adding a view in order for the list of model classes to be displayed in the **Add View** dialog.</span></span>

<span data-ttu-id="a0ced-305">Index() メソッドを右クリックし、メニュー オプションを選択して、インデックス ビューを作成する**ビューの追加**(図 12 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a0ced-305">You create the Index view by right-clicking the Index() method and selecting the menu option **Add View** (see Figure 12).</span></span> <span data-ttu-id="a0ced-306">このメニュー オプションを選択すると開きます、**ビューの追加**ダイアログ (図 13 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a0ced-306">Selecting this menu option opens the **Add View** dialog (see Figure 13).</span></span>


<span data-ttu-id="a0ced-307">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-307">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)</span></span>

<span data-ttu-id="a0ced-308">**図 12**: インデックス ビューの追加 ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-308">**Figure 12**: Adding the Index view ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image24.png))</span></span>


<span data-ttu-id="a0ced-309">**ビューの追加**ダイアログ ボックスで、チェック ボックス ラベルの付いた**厳密に型指定されたビューを作成**です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-309">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="a0ced-310">ビューのデータ クラス ContactManager.Models.Contact とビューのコンテンツの一覧を選択します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-310">Select the View data class ContactManager.Models.Contact and the View content List.</span></span> <span data-ttu-id="a0ced-311">これらのオプションを選択するには、連絡先のレコードの一覧を表示するビューが生成されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-311">Selecting these options generates a view that displays a list of Contact records.</span></span>


<span data-ttu-id="a0ced-312">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-312">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)</span></span>

<span data-ttu-id="a0ced-313">**図 13**: ビューの追加 ダイアログ ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image26.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-313">**Figure 13**: The Add View dialog ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image26.png))</span></span>


<span data-ttu-id="a0ced-314">クリックすると、**追加** ボタン、リスト 3 で、インデックス ビューを生成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-314">When you click the **Add** button, the Index view in Listing 3 is generated.</span></span> <span data-ttu-id="a0ced-315">通知、 &lt;% ページ % @&gt;ディレクティブ ファイルの上部に表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-315">Notice the &lt;%@ Page %&gt; directive that appears at the top of the file.</span></span> <span data-ttu-id="a0ced-316">インデックス ビューは、ViewPage から継承&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt;クラスです。</span><span class="sxs-lookup"><span data-stu-id="a0ced-316">The Index view inherits from the ViewPage&lt;IEnumerable&lt;ContactManager.Models.Contact&gt;&gt; class.</span></span> <span data-ttu-id="a0ced-317">つまり、ビューのモデル クラスは、Contact エンティティの一覧を表します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-317">In other words, the Model class in the view represents a list of Contact entities.</span></span>

<span data-ttu-id="a0ced-318">インデックス ビューの本文には、それぞれのモデル クラスによって表される連絡先を反復処理する foreach ループが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a0ced-318">The body of the Index view contains a foreach loop that iterates through each of the contacts represented by the Model class.</span></span> <span data-ttu-id="a0ced-319">連絡先クラスの各プロパティの値は、HTML テーブル内で表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-319">The value of each property of the Contact class is displayed within an HTML table.</span></span>

<span data-ttu-id="a0ced-320">**3 - Views\Home\Index.aspx (未変更の状態) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a0ced-320">**Listing 3 - Views\Home\Index.aspx (unmodified)**</span></span>

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample3.aspx)]

<span data-ttu-id="a0ced-321">インデックス ビューに 1 つの変更が必要です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-321">We need to make one modification to the Index view.</span></span> <span data-ttu-id="a0ced-322">詳細ビューを作成できませんから詳細情報のリンクは削除できます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-322">Because we are not creating a Details view, we can remove the Details link.</span></span> <span data-ttu-id="a0ced-323">検索して、インデックス ビューから、次のコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-323">Find and remove the following code from the Index view:</span></span>

<span data-ttu-id="a0ced-324">{ id=item.Id })%&gt;</span><span class="sxs-lookup"><span data-stu-id="a0ced-324">{ id=item.Id })%&gt;</span></span>

<span data-ttu-id="a0ced-325">インデックス ビューを変更した後、連絡先のマネージャー アプリケーションを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-325">After you modify the Index view, you can run the Contact Manager application.</span></span> <span data-ttu-id="a0ced-326">[デバッグ開始] メニュー オプション、デバッグを選択するか、単に F5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-326">Select the menu option Debug, Start Debugging or simply press F5.</span></span> <span data-ttu-id="a0ced-327">初めてアプリケーションを実行するダイアログ ボックスを図 14 に取得します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-327">The first time you run the application, you get the dialog in Figure 14.</span></span> <span data-ttu-id="a0ced-328">オプションを選択**Web.config ファイルを変更してデバッグを有効にする**し、[ok] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="a0ced-328">Select the option **Modify the Web.config file to enable debugging** and click the OK button.</span></span>


<span data-ttu-id="a0ced-329">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-329">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)</span></span>

<span data-ttu-id="a0ced-330">**図 14**: デバッグの有効化 ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image28.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-330">**Figure 14**: Enabling debugging ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image28.png))</span></span>


<span data-ttu-id="a0ced-331">既定では、インデックス ビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-331">The Index view is returned by default.</span></span> <span data-ttu-id="a0ced-332">このビューには、すべての連絡先データベース テーブルからデータを一覧表示 (図 15 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a0ced-332">This view lists all of the data from the Contacts database table (see Figure 15).</span></span>


<span data-ttu-id="a0ced-333">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-333">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)</span></span>

<span data-ttu-id="a0ced-334">**図 15**:、インデックス ビュー ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image30.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-334">**Figure 15**: The Index view ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image30.png))</span></span>


<span data-ttu-id="a0ced-335">インデックス ビューに、ビューの下部にある場合は、新規作成というラベルのリンクが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-335">Notice that the Index view includes a link labeled Create New at the bottom of the view.</span></span> <span data-ttu-id="a0ced-336">次のセクションでは、新しい連絡先を作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-336">In the next section, you learn how to create new contacts.</span></span>

## <a name="creating-new-contacts"></a><span data-ttu-id="a0ced-337">新しい連絡先を作成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-337">Creating New Contacts</span></span>

<span data-ttu-id="a0ced-338">新しい連絡先を作成するユーザーを有効にするには、Home コント ローラーに 2 つの Create() アクションを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-338">To enable users to create new contacts, we need to add two Create() actions to the Home controller.</span></span> <span data-ttu-id="a0ced-339">新しい連絡先を作成するための HTML フォームを表す 1 つの Create() アクションを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-339">We need to create one Create() action that returns an HTML form for creating a new contact.</span></span> <span data-ttu-id="a0ced-340">新しい連絡先の実際のデータベースの挿入を実行する 2 番目の Create() アクションを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-340">We need to create a second Create() action that performs the actual database insert of the new contact.</span></span>

<span data-ttu-id="a0ced-341">4 の一覧表示するのには、Home コント ローラーに追加する必要がある新しい Create() メソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a0ced-341">The new Create() methods that we need to add to the Home controller are contained in Listing 4.</span></span>

<span data-ttu-id="a0ced-342">**4 - controllers \homecontroller.cs (のメソッドを作成する) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a0ced-342">**Listing 4 - Controllers\HomeController.cs (with Create methods)**</span></span>

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample4.cs)]

<span data-ttu-id="a0ced-343">最初の Create() メソッド呼び出すことができる HTTP GET 中に、2 番目の Create() メソッドは、HTTP POST によってのみ呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-343">The first Create() method can be invoked with an HTTP GET while the second Create() method can be invoked only by an HTTP POST.</span></span> <span data-ttu-id="a0ced-344">つまり、2 番目の Create() メソッドは、HTML フォームの送信時にのみ起動できます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-344">In other words, the second Create() method can be invoked only when posting an HTML form.</span></span> <span data-ttu-id="a0ced-345">最初の Create() メソッドは、単に、新しい連絡先を作成するための HTML フォームを含むビューを返します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-345">The first Create() method simply returns a view that contains the HTML form for creating a new contact.</span></span> <span data-ttu-id="a0ced-346">2 番目の Create() メソッドがよりわかりやすく: データベースに新しい連絡先を追加します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-346">The second Create() method is much more interesting: it adds the new contact to the database.</span></span>

<span data-ttu-id="a0ced-347">連絡先クラスのインスタンスをそのまま使用する 2 番目の Create() メソッドが変更されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a0ced-347">Notice that the second Create() method has been modified to accept an instance of the Contact class.</span></span> <span data-ttu-id="a0ced-348">HTML フォームのポストされたフォーム値にバインドされますこの連絡先クラス、ASP.NET MVC フレームワークによって自動的に。</span><span class="sxs-lookup"><span data-stu-id="a0ced-348">The form values posted from the HTML form are bound to this Contact class by the ASP.NET MVC framework automatically.</span></span> <span data-ttu-id="a0ced-349">各フォーム フィールド HTML を作成フォームからは、連絡先のパラメーターのプロパティに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-349">Each form field from the HTML Create form is assigned to a property of the Contact parameter.</span></span>

<span data-ttu-id="a0ced-350">連絡先のパラメーターが [バインド] 属性で修飾されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a0ced-350">Notice that the Contact parameter is decorated with a [Bind] attribute.</span></span> <span data-ttu-id="a0ced-351">[バインド] 属性を使用して、バインドからの連絡先 Id プロパティを除外します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-351">The [Bind] attribute is used to exclude the Contact Id property from binding.</span></span> <span data-ttu-id="a0ced-352">Id プロパティを表すため、Identity プロパティ、Id プロパティを設定したくありません。</span><span class="sxs-lookup"><span data-stu-id="a0ced-352">Because the Id property represents an Identity property, we don t want to set the Id property.</span></span>

<span data-ttu-id="a0ced-353">Create() メソッドの本体、データベースに新しい連絡先を挿入する Entity Framework を使用します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-353">In the body of the Create() method, the Entity Framework is used to insert the new Contact into the database.</span></span> <span data-ttu-id="a0ced-354">連絡先の既存のセットに新しい連絡先が追加され、基になるデータベースにこれらの変更をプッシュして戻しますに SaveChanges() メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-354">The new Contact is added to the existing set of Contacts and the SaveChanges() method is called to push these changes back to the underlying database.</span></span>

<span data-ttu-id="a0ced-355">2 つの Create() メソッドのいずれかを右クリックし、メニュー オプションを選択して新しい連絡先を作成するための HTML フォームを生成する**ビューの追加**(図 16 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a0ced-355">You can generate an HTML form for creating new Contacts by right-clicking either of the two Create() methods and selecting the menu option **Add View** (see Figure 16).</span></span>


<span data-ttu-id="a0ced-356">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-356">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)</span></span>

<span data-ttu-id="a0ced-357">**図 16**: 作成ビューの追加 ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image32.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-357">**Figure 16**: Adding the Create view ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image32.png))</span></span>


<span data-ttu-id="a0ced-358">**ビューの追加**ダイアログで、選択、 **ContactManager.Models.Contact**クラスおよび**作成**コンテンツの表示のオプション (図 17 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a0ced-358">In the **Add View** dialog, select the **ContactManager.Models.Contact** class and the **Create** option for view content (see Figure 17).</span></span> <span data-ttu-id="a0ced-359">クリックすると、**追加** ボタン、ビューが自動的に生成される作成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-359">When you click the **Add** button, a Create view is generated automatically.</span></span>


<span data-ttu-id="a0ced-360">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-360">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)</span></span>

<span data-ttu-id="a0ced-361">**図 17**: 分解ページが表示されて ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image34.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-361">**Figure 17**: Seeing a page explode ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image34.png))</span></span>


<span data-ttu-id="a0ced-362">ビュー作成するにはには、各連絡先クラスのプロパティのフォーム フィールドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a0ced-362">The Create view contains form fields for each of the properties of the Contact class.</span></span> <span data-ttu-id="a0ced-363">ビューを作成するためのコードは、5 の一覧に含まれます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-363">The code for the Create view is included in Listing 5.</span></span>

<span data-ttu-id="a0ced-364">**Listing 5 - Views\Home\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="a0ced-364">**Listing 5 - Views\Home\Create.aspx**</span></span>

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample5.aspx)]

<span data-ttu-id="a0ced-365">Create() メソッドを変更し、作成ビューを追加した後は、連絡先のマネージャー アプリケーションを実行し、新しい連絡先を作成します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-365">After you modify the Create() methods and add the Create view, you can run the Contact Manger application and create new contacts.</span></span> <span data-ttu-id="a0ced-366">クリックして、**新規作成**作成ビューに移動するインデックス ビューに表示されるリンク。</span><span class="sxs-lookup"><span data-stu-id="a0ced-366">Click the **Create New** link that appears in the Index view to navigate to the Create view.</span></span> <span data-ttu-id="a0ced-367">図 18 にビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-367">You should see the view in Figure 18.</span></span>


<span data-ttu-id="a0ced-368">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-368">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)</span></span>

<span data-ttu-id="a0ced-369">**図 18**: ビューの作成 ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image36.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-369">**Figure 18**: The Create View ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image36.png))</span></span>


## <a name="editing-contacts"></a><span data-ttu-id="a0ced-370">連絡先を編集</span><span class="sxs-lookup"><span data-stu-id="a0ced-370">Editing Contacts</span></span>

<span data-ttu-id="a0ced-371">連絡先レコードを編集するための機能を追加することは、新しい連絡先レコードを作成するための機能を追加するとよく似ています。</span><span class="sxs-lookup"><span data-stu-id="a0ced-371">Adding the functionality for editing a contact record is very similar to adding the functionality for creating new contact records.</span></span> <span data-ttu-id="a0ced-372">最初に、ホーム コント ローラー クラスに次の 2 つの新しい編集メソッドを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-372">First, we need to add two new Edit methods to the Home controller class.</span></span> <span data-ttu-id="a0ced-373">6 の一覧表示するのには、これらの新しい Edit() メソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a0ced-373">These new Edit() methods are contained in Listing 6.</span></span>

<span data-ttu-id="a0ced-374">**6 - controllers \homecontroller.cs (の編集メソッド) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a0ced-374">**Listing 6 - Controllers\HomeController.cs (with Edit methods)**</span></span>

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample6.cs)]

<span data-ttu-id="a0ced-375">最初の Edit() メソッドが HTTP GET 操作で呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-375">The first Edit() method is invoked by an HTTP GET operation.</span></span> <span data-ttu-id="a0ced-376">Id パラメーターは、この編集されている連絡先のレコードの Id を表すメソッドに渡されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-376">An Id parameter is passed to this method which represents the Id of the contact record being edited.</span></span> <span data-ttu-id="a0ced-377">Entity Framework は、id と一致する連絡先を取得するために使用します。レコードの編集の HTML フォームを含むビューが返されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-377">The Entity Framework is used to retrieve a contact that matches the Id. A view that contains an HTML form for editing a record is returned.</span></span>

<span data-ttu-id="a0ced-378">2 番目の Edit() メソッドでは、データベースへの実際の更新プログラムを実行します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-378">The second Edit() method performs the actual update to the database.</span></span> <span data-ttu-id="a0ced-379">このメソッドは、連絡先クラスのインスタンスをパラメーターとして受け取ります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-379">This method accepts an instance of the Contact class as a parameter.</span></span> <span data-ttu-id="a0ced-380">ASP.NET MVC フレームワークはフォームのフィールド編集用のフォームからこのクラスを自動的にバインドします。</span><span class="sxs-lookup"><span data-stu-id="a0ced-380">The ASP.NET MVC framework binds the form fields from the Edit form to this class automatically.</span></span> <span data-ttu-id="a0ced-381">T がないことに注意してくださいには、(必要があります、Id プロパティの値) の連絡先を編集するときに、[バインド] 属性が含まれます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-381">Notice that you don t include the[Bind] attribute when editing a Contact (we need the value of the Id property).</span></span>

<span data-ttu-id="a0ced-382">Entity Framework を使用すると、変更されたメンバーをデータベースに保存されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-382">The Entity Framework is used to save the modified Contact to the database.</span></span> <span data-ttu-id="a0ced-383">元にお問い合わせくださいは、まず、データベースから取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-383">The original Contact must be retrieved from the database first.</span></span> <span data-ttu-id="a0ced-384">次に、連絡先に変更を記録する Entity Framework ApplyPropertyChanges() メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-384">Next, the Entity Framework ApplyPropertyChanges() method is called to record the changes to the Contact.</span></span> <span data-ttu-id="a0ced-385">最後に、基になるデータベースに変更を保持する Entity Framework SaveChanges() メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-385">Finally, the Entity Framework SaveChanges() method is called to persist the changes to the underlying database.</span></span>

<span data-ttu-id="a0ced-386">Edit() メソッドを右クリックし、追加のビュー メニュー オプションを選択して編集用のフォームを含むビューを生成することができます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-386">You can generate the view that contains the Edit form by right-clicking the Edit() method and selecting the menu option Add View.</span></span> <span data-ttu-id="a0ced-387">ビューの追加 ダイアログ ボックスで、 **ContactManager.Models.Contact**クラスおよび**編集**コンテンツを表示 (図 19 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a0ced-387">In the Add View dialog, select the **ContactManager.Models.Contact** class and the **Edit** view content (see Figure 19).</span></span>


<span data-ttu-id="a0ced-388">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-388">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)</span></span>

<span data-ttu-id="a0ced-389">**図 19**: ビューの編集を追加する ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image38.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-389">**Figure 19**: Adding an Edit View ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image38.png))</span></span>


<span data-ttu-id="a0ced-390">[追加] ボタンをクリックすると、新しい編集ビューが自動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-390">When you click the Add button, a new Edit view is generated automatically.</span></span> <span data-ttu-id="a0ced-391">生成される HTML フォームには、各連絡先クラス (7 のリストを参照) のプロパティに対応するフィールドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a0ced-391">The HTML form that is generated contains fields that correspond to each of the properties of the Contact class (see Listing 7).</span></span>

<span data-ttu-id="a0ced-392">**Listing 7 - Views\Home\Edit.aspx**</span><span class="sxs-lookup"><span data-stu-id="a0ced-392">**Listing 7 - Views\Home\Edit.aspx**</span></span>

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample7.aspx)]

## <a name="deleting-contacts"></a><span data-ttu-id="a0ced-393">連絡先を削除します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-393">Deleting Contacts</span></span>

<span data-ttu-id="a0ced-394">連絡先を削除する場合は、ホーム コント ローラー クラスに次の 2 つの Delete() アクションを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-394">If you want to delete contacts then you need to add two Delete() actions to the Home controller class.</span></span> <span data-ttu-id="a0ced-395">Delete() の最初のアクションには、削除の確認フォームが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-395">The first Delete() action displays a delete confirmation form.</span></span> <span data-ttu-id="a0ced-396">2 番目の Delete() アクションでは、実際の削除を実行します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-396">The second Delete() action performs the actual delete.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a0ced-397">後で、イテレーション 7 では、お変更連絡先のマネージャー Ajax 削除 1 つのステップをサポートできるようにします。</span><span class="sxs-lookup"><span data-stu-id="a0ced-397">Later, in Iteration #7, we modify the Contact Manager so that it supports a one step Ajax delete.</span></span>


<span data-ttu-id="a0ced-398">8 の一覧表示するのには、2 つの新しい Delete() メソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a0ced-398">The two new Delete() methods are contained in Listing 8.</span></span>

<span data-ttu-id="a0ced-399">**8 - controllers \homecontroller.cs (削除メソッド) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a0ced-399">**Listing 8 - Controllers\HomeController.cs (Delete methods)**</span></span>

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample8.cs)]

<span data-ttu-id="a0ced-400">最初の Delete() メソッドが、データベースからの連絡先レコードを削除するための確認形式を返します (Figure20 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a0ced-400">The first Delete() method returns a confirmation form for deleting a contact record from the database (see Figure20).</span></span> <span data-ttu-id="a0ced-401">2 番目の Delete() メソッドでは、データベースに対して、実際の削除操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-401">The second Delete() method performs the actual delete operation against the database.</span></span> <span data-ttu-id="a0ced-402">データベースから元の連絡先の取得が完了した後は、データベースの削除を実行する、Entity Framework DeleteObject() および SaveChanges() メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-402">After the original contact has been retrieved from the database, the Entity Framework DeleteObject() and SaveChanges() methods are called to perform the database delete.</span></span>


<span data-ttu-id="a0ced-403">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-403">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)</span></span>

<span data-ttu-id="a0ced-404">**図 20**: 削除の確認ビュー ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image40.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-404">**Figure 20**: The delete confirmation view ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image40.png))</span></span>


<span data-ttu-id="a0ced-405">(図 21 を参照してください) に連絡先レコードを削除するためのリンクが含まれているように、インデックス ビューを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-405">We need to modify the Index view so that it contains a link for deleting contact records (see Figure 21).</span></span> <span data-ttu-id="a0ced-406">[編集] リンクを含む表のセルに次のコードを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-406">You need to add the following code to the same table cell that contains the Edit link:</span></span>

<span data-ttu-id="a0ced-407">Html.ActionLink( { id=item.Id }) %&gt;</span><span class="sxs-lookup"><span data-stu-id="a0ced-407">Html.ActionLink( { id=item.Id }) %&gt;</span></span>


<span data-ttu-id="a0ced-408">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-408">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)</span></span>

<span data-ttu-id="a0ced-409">**図 21**: 編集リンクを使用してビューにインデックス ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image42.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-409">**Figure 21**: Index view with Edit link ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image42.png))</span></span>


<span data-ttu-id="a0ced-410">次に、削除の確認のビューを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-410">Next, we need to create the delete confirmation view.</span></span> <span data-ttu-id="a0ced-411">Home コント ローラー クラスで Delete() メソッドを右クリックし、メニュー オプションの追加のビューを選択します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-411">Right-click the Delete() method in the Home controller class and select the menu option Add View.</span></span> <span data-ttu-id="a0ced-412">ビューの追加 ダイアログが表示されます (図 22)。</span><span class="sxs-lookup"><span data-stu-id="a0ced-412">The Add View dialog appears (see Figure 22).</span></span>

<span data-ttu-id="a0ced-413">異なりは、リスト、作成、およびエディット ビューの場合、ビューの追加ダイアログ ボックスには、削除ビューを作成するオプションがありません。</span><span class="sxs-lookup"><span data-stu-id="a0ced-413">Unlike in the case of the List, Create, and Edit views, the Add View dialog does not contain an option to create a Delete view.</span></span> <span data-ttu-id="a0ced-414">代わりに、選択、 **ContactManager.Models.Contact**データ クラスおよび**空**コンテンツを表示します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-414">Instead, select the **ContactManager.Models.Contact** data class and the **Empty** view content.</span></span> <span data-ttu-id="a0ced-415">コンテンツのオプションをする必要が社内でビューを作成する空のビューを選択します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-415">Selecting the Empty view content option will require us to create the view ourselves.</span></span>


<span data-ttu-id="a0ced-416">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-416">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)</span></span>

<span data-ttu-id="a0ced-417">**図 22**: 削除の確認ビューの追加 ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image44.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-417">**Figure 22**: Adding the delete confirmation view ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image44.png))</span></span>


<span data-ttu-id="a0ced-418">削除ビューの内容は、9 の一覧に含まれます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-418">The content of the Delete view is contained in Listing 9.</span></span> <span data-ttu-id="a0ced-419">このビューには、ことを確認するフォームが含まれています (図 21 を参照してください) を削除するかどうか、特定の連絡先があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-419">This view contains a form that confirms whether or not a particular contact should be deleted (see Figure 21).</span></span>

<span data-ttu-id="a0ced-420">**Listing 9 - Views\Home\Delete.aspx**</span><span class="sxs-lookup"><span data-stu-id="a0ced-420">**Listing 9 - Views\Home\Delete.aspx**</span></span>

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a><span data-ttu-id="a0ced-421">既定のコント ローラーの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-421">Changing the Name of the Default Controller</span></span>

<span data-ttu-id="a0ced-422">連絡先を操作するため、コント ローラー クラスの名前がテンプレートを使用するクラスをという名前のわざわざ可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-422">It might bother you that the name of our controller class for working with contacts is named the HomeController class.</span></span> <span data-ttu-id="a0ced-423">T、コント ローラーの名前 ContactController しますか。</span><span class="sxs-lookup"><span data-stu-id="a0ced-423">Shouldn t the controller be named ContactController?</span></span>

<span data-ttu-id="a0ced-424">この問題は、簡単に修正します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-424">This issue is easy enough to fix.</span></span> <span data-ttu-id="a0ced-425">最初に、Home コント ローラーの名前をリファクターする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-425">First, we need to refactor the name of the Home controller.</span></span> <span data-ttu-id="a0ced-426">HomeController クラスを Visual Studio コード エディターで開く、クラスの名前を右クリックし、メニュー オプションを選択**名前の変更をリファクタリング**です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-426">Open the HomeController class in the Visual Studio Code Editor, right click the name of the class and select the menu option **Refactor, Rename**.</span></span> <span data-ttu-id="a0ced-427">このメニュー オプションを選択すると、名前の変更 ダイアログが開きます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-427">Selecting this menu option opens the Rename dialog.</span></span>


<span data-ttu-id="a0ced-428">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-428">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)</span></span>

<span data-ttu-id="a0ced-429">**図 23**: コント ローラーの名前をリファクタリング ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image46.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-429">**Figure 23**: Refactoring a controller name ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image46.png))</span></span>


<span data-ttu-id="a0ced-430">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-430">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)</span></span>

<span data-ttu-id="a0ced-431">**図 24**: 名前の変更 ダイアログを使用して ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image48.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-431">**Figure 24**: Using the Rename dialog ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image48.png))</span></span>


<span data-ttu-id="a0ced-432">コント ローラー クラスの名前を変更すると、Visual Studio でのビューのフォルダーで、フォルダーの名前が更新されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-432">If you rename your controller class, Visual Studio will update the name of the folder in the Views folder as well.</span></span> <span data-ttu-id="a0ced-433">Visual Studio は、\Views\Contact フォルダーに \Views\Home フォルダーの名前が変更されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-433">Visual Studio will rename the \Views\Home folder to the \Views\Contact folder.</span></span>

<span data-ttu-id="a0ced-434">この変更を加えた後、アプリケーションは、Home コント ローラーを不要になったがあります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-434">After you make this change, your application will no longer have a Home controller.</span></span> <span data-ttu-id="a0ced-435">アプリケーションを実行するときに図 25 のエラー ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-435">When you run your application, you'll get the error page in Figure 25.</span></span>


<span data-ttu-id="a0ced-436">[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)</span><span class="sxs-lookup"><span data-stu-id="a0ced-436">[![The New Project dialog box](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)</span></span>

<span data-ttu-id="a0ced-437">**図 25**: 既定のコント ローラーなし ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-cs/_static/image50.png))</span><span class="sxs-lookup"><span data-stu-id="a0ced-437">**Figure 25**: No default controller ([Click to view full-size image](iteration-1-create-the-application-cs/_static/image50.png))</span></span>


<span data-ttu-id="a0ced-438">Home コント ローラーではなく、連絡先のコント ローラーを使用する Global.asax ファイルの既定のルートを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-438">We need to update the default route in the Global.asax file to use the Contact controller instead of the Home controller.</span></span> <span data-ttu-id="a0ced-439">Global.asax ファイルを開き、既定のルート (10 のリストを参照) で使用される既定のコント ローラーを変更します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-439">Open the Global.asax file and modify the default controller used by the default route (see Listing 10).</span></span>

<span data-ttu-id="a0ced-440">**10 - Global.asax.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a0ced-440">**Listing 10 - Global.asax.cs**</span></span>

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample10.cs)]

<span data-ttu-id="a0ced-441">これらの変更を加えた後は、連絡先のマネージャーが正しく実行されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-441">After you make these changes, the Contact Manager will run correctly.</span></span> <span data-ttu-id="a0ced-442">ここで、既定のコント ローラーとして連絡先コント ローラーのクラスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-442">Now, it will use the Contact controller class as the default controller.</span></span>

## <a name="summary"></a><span data-ttu-id="a0ced-443">まとめ</span><span class="sxs-lookup"><span data-stu-id="a0ced-443">Summary</span></span>

<span data-ttu-id="a0ced-444">この最初のイテレーションで作成した基本的な連絡先のマネージャー アプリケーション最速の方法で考えられる。</span><span class="sxs-lookup"><span data-stu-id="a0ced-444">In this first iteration, we created a basic Contact Manager application in the fastest way possible.</span></span> <span data-ttu-id="a0ced-445">このコント ローラーとビューの初期コードを自動的に生成する Visual Studio の利点を作成しました。</span><span class="sxs-lookup"><span data-stu-id="a0ced-445">We took advantage of Visual Studio to generate the initial code for our controllers and views automatically.</span></span> <span data-ttu-id="a0ced-446">データベース モデル クラスを自動的に生成する Entity Framework を活用してもかかりました。</span><span class="sxs-lookup"><span data-stu-id="a0ced-446">We also took advantage of the Entity Framework to generate our database model classes automatically.</span></span>

<span data-ttu-id="a0ced-447">現時点では、おことができますを一覧表示を作成、編集、レコード、および削除にお問い合わせにお問い合わせのマネージャー アプリケーションにします。</span><span class="sxs-lookup"><span data-stu-id="a0ced-447">Currently, we can list, create, edit, and delete contact records with the Contact Manager application.</span></span> <span data-ttu-id="a0ced-448">つまり、すべてのデータベースに基づく web アプリケーションで必要な基本的なデータベース操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-448">In other words, we can perform all of the basic database operations required by a database-driven web application.</span></span>

<span data-ttu-id="a0ced-449">残念ながら、このアプリケーションでは、いくつかの問題があります。</span><span class="sxs-lookup"><span data-stu-id="a0ced-449">Unfortunately, our application has some problems.</span></span> <span data-ttu-id="a0ced-450">これを許可して最初になる躊躇、連絡先のマネージャー アプリケーションは、最も魅力的なアプリケーションではありません。</span><span class="sxs-lookup"><span data-stu-id="a0ced-450">First and I hesitate to admit this, the Contact Manager application is not the most attractive application.</span></span> <span data-ttu-id="a0ced-451">いくつかの設計作業が必要です。</span><span class="sxs-lookup"><span data-stu-id="a0ced-451">It needs some design work.</span></span> <span data-ttu-id="a0ced-452">次のイテレーションで、既定のビューのマスター ページとアプリケーションの外観を改善するカスケード スタイル シートを変更して方法について見ていきます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-452">In the next iteration, we'll look at how we can change the default view master page and cascading style sheet to improve the appearance of the application.</span></span>

<span data-ttu-id="a0ced-453">次に、お実装していないすべてのフォーム検証をします。</span><span class="sxs-lookup"><span data-stu-id="a0ced-453">Second, we have not implemented any form validation.</span></span> <span data-ttu-id="a0ced-454">たとえばがあるフォーム フィールドのいずれかの値を入力することがなく連絡先フォームの作成を送信することを防止するため何も行われません。</span><span class="sxs-lookup"><span data-stu-id="a0ced-454">For example, there is nothing to prevent you from submitting the Create contact form without entering values for any of the form fields.</span></span> <span data-ttu-id="a0ced-455">さらに、無効な電話番号と電子メール アドレスを入力することができます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-455">Furthermore, you can enter invalid phone numbers and email addresses.</span></span> <span data-ttu-id="a0ced-456">イテレーション 3 でのフォーム検証の問題に取り組むを開始します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-456">We start to tackle the problem of form validation in iteration #3.</span></span>

<span data-ttu-id="a0ced-457">最後に、最も重要なは、連絡先のマネージャー アプリケーションの現在のイテレーション簡単に変更またはできません保持されます。</span><span class="sxs-lookup"><span data-stu-id="a0ced-457">Finally, and most importantly, the current iteration of the Contact Manager application cannot be easily modified or maintained.</span></span> <span data-ttu-id="a0ced-458">たとえば、データベース アクセス ロジックに組み込まれて右コント ローラーのアクション。</span><span class="sxs-lookup"><span data-stu-id="a0ced-458">For example, the database access logic is baked right into the controller actions.</span></span> <span data-ttu-id="a0ced-459">つまり、コント ローラーを変更することがなく、データ アクセス コードを修正することはできません。</span><span class="sxs-lookup"><span data-stu-id="a0ced-459">This means that we cannot modify our data access code without modifying our controllers.</span></span> <span data-ttu-id="a0ced-460">後のイテレーションでは、連絡先のマネージャーの変更を回復力を高めるに実装できるソフトウェア設計パターンについて説明します。</span><span class="sxs-lookup"><span data-stu-id="a0ced-460">In later iterations, we explore software design patterns that we can implement to make the Contact Manager more resilient to change.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a0ced-461">次へ</span><span class="sxs-lookup"><span data-stu-id="a0ced-461">Next</span></span>](iteration-2-make-the-application-look-nice-cs.md)
