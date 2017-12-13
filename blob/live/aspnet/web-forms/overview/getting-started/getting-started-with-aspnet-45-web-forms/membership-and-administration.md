---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: "メンバーシップと管理 |Microsoft ドキュメント"
author: Erikre
description: "このチュートリアルの系列では、お用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: a10dbfe1ca49baee1604aac8dd9a1f93ccfcb7f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="membership-and-administration"></a><span data-ttu-id="33cbc-103">メンバーシップと管理</span><span class="sxs-lookup"><span data-stu-id="33cbc-103">Membership and Administration</span></span>
====================
<span data-ttu-id="33cbc-104">によって[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="33cbc-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="33cbc-105">[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="33cbc-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="33cbc-106">このチュートリアルの系列では、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="33cbc-107">Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)はこのチュートリアルのシリーズに付随する使用できます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="33cbc-108">このチュートリアルでは、カスタム ロールを追加して、ASP.NET Identity Wingtip Toys サンプル アプリケーションを更新する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-108">This tutorial shows you how to update the Wingtip Toys sample application to add a custom role and use ASP.NET Identity.</span></span> <span data-ttu-id="33cbc-109">元のカスタム ロールを持つユーザーを追加したり、web サイトから製品を削除、管理ページを実装する方法も示します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-109">It also shows you how to implement an administration page from which the user with a custom role can add and remove products from the website.</span></span>

<span data-ttu-id="33cbc-110">[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) ASP.NET web アプリケーションをビルドするために使用するメンバーシップ システムは、ASP.NET 4.5 で使用できます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-110">[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) is the membership system used to build ASP.NET web application and is available in ASP.NET 4.5.</span></span> <span data-ttu-id="33cbc-111">用のテンプレートと同様に、Visual Studio 2013 の Web フォーム プロジェクト テンプレートで使用される ASP.NET Id [ASP.NET MVC](../../../../mvc/index.md)、 [ASP.NET Web API](../../../../web-api/index.md)、および[ASP.NET Single Page Application](../../../../single-page-application/index.md).</span><span class="sxs-lookup"><span data-stu-id="33cbc-111">ASP.NET Identity is used in the Visual Studio 2013 Web Forms project template, as well as the templates for [ASP.NET MVC](../../../../mvc/index.md), [ASP.NET Web API](../../../../web-api/index.md), and [ASP.NET Single Page Application](../../../../single-page-application/index.md).</span></span> <span data-ttu-id="33cbc-112">具体的にも空の Web アプリケーションを開始するときに、NuGet を使用して、ASP.NET Identity システムをインストールできます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-112">You can also specifically install the ASP.NET Identity system using NuGet when you start with an empty Web application.</span></span> <span data-ttu-id="33cbc-113">ただし、このチュートリアルの系列に使用する、 **Web フォーム**projecttemplate で、ASP.NET Identity システムが含まれています。</span><span class="sxs-lookup"><span data-stu-id="33cbc-113">However, in this tutorial series you use the **Web Forms**projecttemplate, which includes the ASP.NET Identity system.</span></span> <span data-ttu-id="33cbc-114">ASP.NET Identity しやすいアプリケーションのデータとユーザー固有のプロファイル データを統合します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-114">ASP.NET Identity makes it easy to integrate user-specific profile data with application data.</span></span> <span data-ttu-id="33cbc-115">また、ASP.NET Identity には、アプリケーションでユーザー プロファイルの永続化モデルを選択することができます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-115">Also, ASP.NET Identity allows you to choose the persistence model for user profiles in your application.</span></span> <span data-ttu-id="33cbc-116">データを保存するには、SQL Server データベースまたは別のデータ ストアに含む*NoSQL* Windows Azure ストレージ テーブルなどのデータを格納します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-116">You can store the data in a SQL Server database or another data store, including *NoSQL* data stores such as Windows Azure Storage Tables.</span></span>

<span data-ttu-id="33cbc-117">このチュートリアルは、一連のチュートリアルについて、Wingtip Toys"チェック アウトと支払いの PayPal"というタイトルの前のチュートリアルに基づいています。</span><span class="sxs-lookup"><span data-stu-id="33cbc-117">This tutorial builds on the previous tutorial titled "Checkout and Payment with PayPal" in the Wingtip Toys tutorial series.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="33cbc-118">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="33cbc-118">What you'll learn:</span></span>

- <span data-ttu-id="33cbc-119">アプリケーションにカスタムのロールとユーザーを追加するコードを使用する方法。</span><span class="sxs-lookup"><span data-stu-id="33cbc-119">How to use code to add a custom role and a user to the application.</span></span>
- <span data-ttu-id="33cbc-120">管理フォルダーとページへのアクセスを制限する方法です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-120">How to restrict access to the administration folder and page.</span></span>
- <span data-ttu-id="33cbc-121">カスタム ロールに属しているユーザーのナビゲーションを提供する方法です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-121">How to provide navigation for the user that belongs to the custom role.</span></span>
- <span data-ttu-id="33cbc-122">モデル バインディングを使用して設定する方法、 [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx)製品カテゴリを制御します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-122">How to use model binding to populate a [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) control with product categories.</span></span>
- <span data-ttu-id="33cbc-123">Web アプリケーションを使用して、ファイルをアップロードする方法、[ファイルアップロード](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx)コントロール。</span><span class="sxs-lookup"><span data-stu-id="33cbc-123">How to upload a file to the web application using the [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) control.</span></span>
- <span data-ttu-id="33cbc-124">検証コントロールを使用して、入力の検証を実装する方法。</span><span class="sxs-lookup"><span data-stu-id="33cbc-124">How to use validation controls to implement input validation.</span></span>
- <span data-ttu-id="33cbc-125">追加し、アプリケーションから製品を削除する方法です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-125">How to add and remove products from the application.</span></span>

## <a name="these-features-are-included-in-the-tutorial"></a><span data-ttu-id="33cbc-126">このチュートリアルでは、これらの機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="33cbc-126">These features are included in the tutorial:</span></span>

- <span data-ttu-id="33cbc-127">ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="33cbc-127">ASP.NET Identity</span></span>
- <span data-ttu-id="33cbc-128">構成と承認</span><span class="sxs-lookup"><span data-stu-id="33cbc-128">Configuration and Authorization</span></span>
- <span data-ttu-id="33cbc-129">モデル バインディング</span><span class="sxs-lookup"><span data-stu-id="33cbc-129">Model Binding</span></span>
- <span data-ttu-id="33cbc-130">控えめな検証</span><span class="sxs-lookup"><span data-stu-id="33cbc-130">Unobtrusive Validation</span></span>

<span data-ttu-id="33cbc-131">ASP.NET Web フォームでは、メンバーシップ機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-131">ASP.NET Web Forms provides membership capabilities.</span></span> <span data-ttu-id="33cbc-132">既定のテンプレートを使用するには、アプリケーションの実行時にすぐに使用できる組み込みのメンバーシップ機能があります。</span><span class="sxs-lookup"><span data-stu-id="33cbc-132">By using the default template, you have built-in membership functionality that you can immediately use when the application runs.</span></span> <span data-ttu-id="33cbc-133">このチュートリアルでは、ASP.NET Identity を使用してカスタム ロールを追加し、そのロールにユーザーを割り当てる方法を示します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-133">This tutorial shows you how to use ASP.NET Identity to add a custom role and assign a user to that role.</span></span> <span data-ttu-id="33cbc-134">管理フォルダーへのアクセスを制限する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-134">You will learn how to restrict access to the administration folder.</span></span> <span data-ttu-id="33cbc-135">カスタム ロールを持つユーザーを追加および削除の製品、および追加された後に製品をプレビューする管理フォルダーには、ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-135">You'll add a page to the administration folder that allows a user with a custom role to add and remove products, and to preview a product after it has been added.</span></span>

## <a name="adding-a-custom-role"></a><span data-ttu-id="33cbc-136">カスタム ロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-136">Adding a Custom Role</span></span>

<span data-ttu-id="33cbc-137">ASP.NET の Id を使用して、カスタム ロールを追加してコードを使用してそのロールにユーザーを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-137">Using ASP.NET Identity, you can add a custom role and assign a user to that role using code.</span></span>

1. <span data-ttu-id="33cbc-138">**ソリューション エクスプ ローラー**を右クリックし、*ロジック*フォルダーと、新しいクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-138">In **Solution Explorer**, right-click on the *Logic* folder and create a new class.</span></span>
2. <span data-ttu-id="33cbc-139">新しいクラスの名前を*RoleActions.cs*です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-139">Name the new class *RoleActions.cs*.</span></span>
3. <span data-ttu-id="33cbc-140">次のように表示されるようにコードを変更します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-140">Modify the code so that it appears as follows:</span></span>  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. <span data-ttu-id="33cbc-141">**ソリューション エクスプ ローラー**を開き、 *Global.asax.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="33cbc-141">In **Solution Explorer**, open the *Global.asax.cs* file.</span></span>
5. <span data-ttu-id="33cbc-142">変更、 *Global.asax.cs*黄色で強調表示されている次のように表示されるようにコードを追加することによってファイル。</span><span class="sxs-lookup"><span data-stu-id="33cbc-142">Modify the *Global.asax.cs* file by adding the code highlighted in yellow so that it appears as follows:</span></span>  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. <span data-ttu-id="33cbc-143">注意して`AddUserAndRole`赤い下線が引かれます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-143">Notice that `AddUserAndRole` is underlined in red.</span></span> <span data-ttu-id="33cbc-144">AddUserAndRole コードをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="33cbc-144">Double-click the AddUserAndRole code.</span></span>  
 <span data-ttu-id="33cbc-145">強調表示されているメソッドの先頭に文字"A"は下線が表示されます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-145">The letter "A" at the beginning of the highlighted method will be underlined.</span></span>
7. <span data-ttu-id="33cbc-146">文字"A"ポインターを合わせるし、UI のメソッド スタブを生成できるようにする をクリックして、`AddUserAndRole`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="33cbc-146">Hover over the letter "A" and click the UI that allows you to generate a method stub for the `AddUserAndRole` method.</span></span> 

    ![メンバーシップと Advministration - メソッド スタブを生成します。](membership-and-administration/_static/image1.png)
8. <span data-ttu-id="33cbc-148">という名前のオプションをクリックします。</span><span class="sxs-lookup"><span data-stu-id="33cbc-148">Click the option titled:</span></span>  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. <span data-ttu-id="33cbc-149">開く、 *RoleActions.cs*ファイルから、*ロジック*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="33cbc-149">Open the *RoleActions.cs* file from the *Logic* folder.</span></span>  
 <span data-ttu-id="33cbc-150">`AddUserAndRole`メソッドがクラス ファイルに追加されました。</span><span class="sxs-lookup"><span data-stu-id="33cbc-150">The `AddUserAndRole` method has been added to the class file.</span></span>
10. <span data-ttu-id="33cbc-151">変更、 *RoleActions.cs*ファイルを削除することによって、`NotImplementedeException`し、次のように表示されるように、黄色で強調表示されているコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-151">Modify the *RoleActions.cs* file by removing the `NotImplementedeException` and adding the code highlighted in yellow, so that it appears as follows:</span></span>  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

<span data-ttu-id="33cbc-152">上記のコードは、まず、メンバーシップ データベースのデータベース コンテキストを確立します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-152">The above code first establishes a database context for the membership database.</span></span> <span data-ttu-id="33cbc-153">メンバーシップ データベースとしても格納されている、 *.mdf*ファイルで、*アプリ\_データ*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="33cbc-153">The membership database is also stored as an *.mdf* file in the *App\_Data* folder.</span></span> <span data-ttu-id="33cbc-154">最初のユーザーがこの web アプリケーションにサインインしたら、このデータベースを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-154">You will be able to view this database once the first user has signed in to this web application.</span></span> 

> [!NOTE] 
> 
> <span data-ttu-id="33cbc-155">使用して、同じを検討するには、製品データと共にメンバーシップ データを格納する場合は、 **DbContext**上記のコードで、製品データを格納するために使用することです。</span><span class="sxs-lookup"><span data-stu-id="33cbc-155">If you wish to store the membership data along with the product data, you can consider using the same **DbContext** that you used to store the product data in the above code.</span></span>


 <span data-ttu-id="33cbc-156">*内部*キーワードは、アクセス修飾子 (クラス) などの型と型のメンバー (メソッド、プロパティなど)。</span><span class="sxs-lookup"><span data-stu-id="33cbc-156">The *internal* keyword is an access modifier for types (such as classes) and type members (such as methods or properties).</span></span> <span data-ttu-id="33cbc-157">内部型またはメンバーは、同じアセンブリに含まれているファイル内でのみアクセス*(.dll*ファイル)。</span><span class="sxs-lookup"><span data-stu-id="33cbc-157">Internal types or members are accessible only within files contained in the same assembly *(.dll* file).</span></span> <span data-ttu-id="33cbc-158">アプリケーションをアセンブリ ファイルをビルドするときに*(.dll*) が作成されるアプリケーションを実行するときに実行されるコードを含むです。</span><span class="sxs-lookup"><span data-stu-id="33cbc-158">When you build your application, an assembly file *(.dll*) is created that contains the code that is executed when you run your application.</span></span> 

<span data-ttu-id="33cbc-159">A`RoleStore`ロール管理を提供するオブジェクトは、データベース コンテキストをに基づいて作成します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-159">A `RoleStore` object, which provides role management, is created based on the database context.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="33cbc-160">場合に、`RoleStore`オブジェクトが作成される、ジェネリックを使用して`IdentityRole`型です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-160">Notice that when the `RoleStore` object is created it uses a Generic `IdentityRole` type.</span></span> <span data-ttu-id="33cbc-161">つまり、`RoleStore`を含むことがのみ`IdentityRole`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="33cbc-161">This means that the `RoleStore` is only allowed to contain `IdentityRole` objects.</span></span> <span data-ttu-id="33cbc-162">ジェネリックを使用すると、メモリ内のリソースの処理が向上します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-162">Also by using Generics, resources in memory are handled better.</span></span>


<span data-ttu-id="33cbc-163">次に、`RoleManager`オブジェクト、に基づいて作成されて、`RoleStore`先ほど作成したオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="33cbc-163">Next, the `RoleManager` object, is created based on the `RoleStore` object that you just created.</span></span> <span data-ttu-id="33cbc-164">`RoleManager`オブジェクト公開ロール関連 API を自動的に変更を保存するために使用する、`RoleStore`です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-164">the `RoleManager` object exposes role related API which can be used to automatically save changes to the `RoleStore`.</span></span> <span data-ttu-id="33cbc-165">`RoleManager`を含むことがのみ`IdentityRole`コードを使用するためのオブジェクト、`<IdentityRole>`ジェネリック型です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-165">The `RoleManager` is only allowed to contain `IdentityRole` objects because the code uses the `<IdentityRole>` Generic type.</span></span>

<span data-ttu-id="33cbc-166">呼び出す、`RoleExists`メソッドが"canEdit"ロールのメンバーシップ データベース内にあるかを判断します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-166">You call the `RoleExists` method to determine if the "canEdit" role is present in the membership database.</span></span> <span data-ttu-id="33cbc-167">そうでない場合は、ロールを作成します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-167">If it is not, you create the role.</span></span>

<span data-ttu-id="33cbc-168">作成する、`UserManager`よりも複雑であるオブジェクトが表示されます、`RoleManager`を制御することがほぼ同じです。</span><span class="sxs-lookup"><span data-stu-id="33cbc-168">Creating the `UserManager` object appears to be more complicated than the `RoleManager` control, however it is nearly the same.</span></span> <span data-ttu-id="33cbc-169">だけのいくつかではなく 1 つの行に記述されています。</span><span class="sxs-lookup"><span data-stu-id="33cbc-169">It is just coded on one line rather than several.</span></span> <span data-ttu-id="33cbc-170">ここでは、渡されたパラメーターは、かっこ内に含まれる新しいオブジェクトとしてインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-170">Here, the parameter that you are passing is instantiating as a new object contained in the parenthesis.</span></span>

<span data-ttu-id="33cbc-171">次に作成する"canEditUser"ユーザーを作成、新しい`ApplicationUser`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="33cbc-171">Next you create the "canEditUser" user by creating a new `ApplicationUser` object.</span></span> <span data-ttu-id="33cbc-172">次に、ユーザーを正常に作成する場合は、新しいロールにユーザーを追加します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-172">Then, if you successfully create the user, you add the user to the new role.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="33cbc-173">エラー処理は、このチュートリアル シリーズの後で「ASP.NET エラーの処理」チュートリアルの中で更新されます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-173">The error handling will be updated during the "ASP.NET Error Handling" tutorial later in this tutorial series.</span></span>


<span data-ttu-id="33cbc-174">アプリケーションの起動時、次回"canEditUser"をという名前のユーザーは、アプリケーションの"canEdit"という名前のロールとして追加されます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-174">The next time the application starts, the user named "canEditUser" will be added as the role named "canEdit" of the application.</span></span> <span data-ttu-id="33cbc-175">このチュートリアルの後半は、このチュートリアル中に追加する追加機能を表示する"canEditUser"のユーザーとしてログインします。</span><span class="sxs-lookup"><span data-stu-id="33cbc-175">Later in this tutorial, you will login as the "canEditUser" user to display additional capabilities that you will added during this tutorial.</span></span> <span data-ttu-id="33cbc-176">ASP.NET Identity の API 詳細は、次を参照してください。、 [Microsoft.AspNet.Identity Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-176">For API details about ASP.NET Identity, see the [Microsoft.AspNet.Identity Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx).</span></span> <span data-ttu-id="33cbc-177">追加の詳細については、ASP.NET Identity システムの初期化には、次を参照してください。、 [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs)です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-177">For additional details about initializing the ASP.NET Identity system, see the [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).</span></span>

### <a name="restricting-access-to-the-administration-page"></a><span data-ttu-id="33cbc-178">[管理] ページへのアクセスを制限</span><span class="sxs-lookup"><span data-stu-id="33cbc-178">Restricting Access to the Administration Page</span></span>

<span data-ttu-id="33cbc-179">Wingtip Toys のサンプル アプリケーションは、匿名ユーザーとユーザーのログインの両方を表示および製品を購入できます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-179">The Wingtip Toys sample application allows both anonymous users and logged-in users to view and purchase products.</span></span> <span data-ttu-id="33cbc-180">ただし、カスタム"canEdit"ロールを持つログインしているユーザーは追加して、製品を削除するために制限されているページにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-180">However, the logged-in user that has the custom "canEdit" role can access a restricted page in order to add and remove products.</span></span>

#### <a name="add-an-administration-folder-and-page"></a><span data-ttu-id="33cbc-181">管理フォルダーとページを追加します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-181">Add an Administration Folder and Page</span></span>

<span data-ttu-id="33cbc-182">次に、という名前のフォルダーを作成、 *Admin* Wingtip Toys のカスタム ロールに属している"canEditUser"ユーザーのサンプル アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="33cbc-182">Next, you will create a folder named *Admin* for the "canEditUser" user belonging to the custom role of the Wingtip Toys sample application.</span></span>

1. <span data-ttu-id="33cbc-183">プロジェクト名を右クリックして (**Wingtip Toys**) で**ソリューション エクスプ ローラー**選択**追加** - &gt; **の新しいフォルダー**.</span><span class="sxs-lookup"><span data-stu-id="33cbc-183">Right-click the project name (**Wingtip Toys**) in **Solution Explorer** and select **Add** -&gt; **New Folder**.</span></span>
2. <span data-ttu-id="33cbc-184">新しいフォルダーの名前を付けます*Admin*です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-184">Name the new folder *Admin*.</span></span>
3. <span data-ttu-id="33cbc-185">右クリックし、 *Admin*クリックしてフォルダー**追加** - &gt; **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-185">Right-click the *Admin* folder and then select **Add** -&gt; **New Item**.</span></span>   
 <span data-ttu-id="33cbc-186">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-186">The **Add New Item** dialog box is displayed.</span></span>
4. <span data-ttu-id="33cbc-187">選択、 **Visual c#** - &gt; **Web**左側のテンプレートのグループです。</span><span class="sxs-lookup"><span data-stu-id="33cbc-187">Select the **Visual C#**-&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="33cbc-188">中央のリストから選択**マスター ページを含む Web フォーム**、名前を付けます*AdminPage.aspx***、**し、**追加**です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-188">From the middle list, select **Web Form with Master Page**,name it *AdminPage.aspx***,** and then select **Add**.</span></span>
5. <span data-ttu-id="33cbc-189">選択、 *Site.Master*クリックしてファイルをマスター ページとして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-189">Select the *Site.Master* file as the master page, and then choose **OK**.</span></span>

#### <a name="add-a-webconfig-file"></a><span data-ttu-id="33cbc-190">Web.config ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-190">Add a Web.config File</span></span>

<span data-ttu-id="33cbc-191">追加することによって、 *Web.config*ファイルの名前を*Admin*フォルダー、フォルダー内のページにアクセスを制限することができます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-191">By adding a *Web.config* file to the *Admin* folder, you can restrict access to the page contained in the folder.</span></span>

1. <span data-ttu-id="33cbc-192">右クリックし、 *Admin*フォルダーと選択**追加** - &gt; **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-192">Right-click the *Admin* folder and select **Add** -&gt; **New Item**.</span></span>  
 <span data-ttu-id="33cbc-193">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-193">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="33cbc-194">、Visual c# web テンプレートの一覧から選択**Web 構成ファイル**中央のリストからの既定の名前を受け入れる*Web.config***、**し、 **追加**です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-194">From the list of Visual C# web templates, select **Web Configuration File**from the middle list, accept the default name of *Web.config***,** and then select **Add**.</span></span>
3. <span data-ttu-id="33cbc-195">既存の XML の内容を置き換える、 *Web.config*を次のファイル。</span><span class="sxs-lookup"><span data-stu-id="33cbc-195">Replace the existing XML content in the *Web.config* file with the following:</span></span>  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

<span data-ttu-id="33cbc-196">保存、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="33cbc-196">Save the *Web.config* file.</span></span> <span data-ttu-id="33cbc-197">*Web.config*ファイルは、その唯一のユーザーがアプリケーションの"canEdit"ロールに所属することができますに含まれているページへのアクセスを指定します、 *Admin*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="33cbc-197">The *Web.config* file specifies that only user belonging to the "canEdit" role of the application can access the page contained in the *Admin* folder.</span></span>

### <a name="including-custom-role-navigation"></a><span data-ttu-id="33cbc-198">カスタム ロールのナビゲーションを含む</span><span class="sxs-lookup"><span data-stu-id="33cbc-198">Including Custom Role Navigation</span></span>

<span data-ttu-id="33cbc-199">アプリケーションの管理 セクションに移動する、カスタム"canEdit"ロールのユーザーを有効にするへのリンクを追加する必要があります、 *Site.Master*ページ。</span><span class="sxs-lookup"><span data-stu-id="33cbc-199">To enable the user of the custom "canEdit" role to navigate to the administration section of the application, you must add a link to the *Site.Master* page.</span></span> <span data-ttu-id="33cbc-200">"CanEdit"ロールに属しているユーザーが参照できるのみ、 **Admin**リンクし、[管理] セクションにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="33cbc-200">Only users that belong to the "canEdit" role will be able to see the **Admin** link and access the administration section.</span></span>

1. <span data-ttu-id="33cbc-201">ソリューション エクスプ ローラーで、検索して開く、 *Site.Master*ページ。</span><span class="sxs-lookup"><span data-stu-id="33cbc-201">In Solution Explorer, find and open the *Site.Master* page.</span></span>
2. <span data-ttu-id="33cbc-202">"CanEdit"ロールのユーザーへのリンクを作成するには、次の順序なしリストを黄色で強調表示のマークアップを追加`<ul>`要素として一覧が表示されるように依存します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-202">To create a link for the user of the "canEdit" role, add the markup highlighted in yellow to the following unordered list `<ul>` element so that the list appears as follows:</span></span>  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. <span data-ttu-id="33cbc-203">開く、 *Site.Master.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="33cbc-203">Open the *Site.Master.cs* file.</span></span> <span data-ttu-id="33cbc-204">ように、 **Admin**に黄色で強調表示されているコードを追加することで"canEditUser"のユーザーにのみ表示されるリンク、`Page_Load`ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="33cbc-204">Make the **Admin** link visible only to the "canEditUser" user by adding the code highlighted in yellow to the `Page_Load` handler.</span></span> <span data-ttu-id="33cbc-205">`Page_Load`ハンドラーは次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-205">The `Page_Load` handler will appear as follows:</span></span>   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

<span data-ttu-id="33cbc-206">ページが読み込まれると、コードは、ログイン ユーザーが"canEdit"の役割を持つかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-206">When the page loads, the code checks whether the logged-in user has the role of "canEdit".</span></span> <span data-ttu-id="33cbc-207">ユーザーが、"canEdit"の役割へのリンクを含む span 要素に属している場合、 *AdminPage.aspx*ページとその範囲内のリンク) が表示されます。 します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-207">If the user belongs to the "canEdit" role, the span element containing the link to the *AdminPage.aspx* page (and consequently the link inside the span) is made visible.</span></span>

### <a name="enabling-product-administration"></a><span data-ttu-id="33cbc-208">製品の管理を有効にします。</span><span class="sxs-lookup"><span data-stu-id="33cbc-208">Enabling Product Administration</span></span>

<span data-ttu-id="33cbc-209">これまで"canEdit"ロールを作成し、"canEditUser"ユーザー、[管理] フォルダー、および管理ページを追加しています。</span><span class="sxs-lookup"><span data-stu-id="33cbc-209">So far, you have created the "canEdit" role and added an "canEditUser" user, an administration folder, and an administration page.</span></span> <span data-ttu-id="33cbc-210">ページで、管理フォルダーのアクセス権を設定して、アプリケーションに"canEdit"ロールのユーザーのナビゲーション リンクを追加しました。</span><span class="sxs-lookup"><span data-stu-id="33cbc-210">You have set access rights for the administration folder and page, and have added a navigation link for the user of the "canEdit" role to the application.</span></span> <span data-ttu-id="33cbc-211">次に、するマークアップを追加、 *AdminPage.aspx*ページし、コードを*AdminPage.aspx.cs*を追加して、製品の削除"canEdit"の役割を持つユーザーを有効にするには、分離コード ファイル。</span><span class="sxs-lookup"><span data-stu-id="33cbc-211">Next, you will add markup to the *AdminPage.aspx* page and code to the *AdminPage.aspx.cs* code-behind file that will enable the user with the "canEdit" role to add and remove products.</span></span>

1. <span data-ttu-id="33cbc-212">**ソリューション エクスプ ローラー**を開き、 *AdminPage.aspx*ファイルから、 *Admin*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="33cbc-212">In **Solution Explorer**, open the *AdminPage.aspx* file from the *Admin* folder.</span></span>
2. <span data-ttu-id="33cbc-213">既存のマークアップを次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-213">Replace the existing markup with the following:</span></span>  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. <span data-ttu-id="33cbc-214">次に、開く、 *AdminPage.aspx.cs*分離コード ファイルを右クリックして、 *AdminPage.aspx*をクリックすると、**コードの表示**です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-214">Next, open the *AdminPage.aspx.cs* code-behind file by right-clicking the *AdminPage.aspx* and clicking **View Code**.</span></span>
4. <span data-ttu-id="33cbc-215">既存のコードを置き換える、 *AdminPage.aspx.cs*を次のコードの分離コード ファイル。</span><span class="sxs-lookup"><span data-stu-id="33cbc-215">Replace the existing code in the *AdminPage.aspx.cs* code-behind file with the following code:</span></span>  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

<span data-ttu-id="33cbc-216">に対して入力したコードで、 *AdminPage.aspx.cs*分離コード ファイルと呼ばれるクラス`AddProducts`データベースに製品の追加の実際の作業です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-216">In the code that you entered for the *AdminPage.aspx.cs* code-behind file, a class called `AddProducts` does the actual work of adding products to the database.</span></span> <span data-ttu-id="33cbc-217">このクラスは、今すぐ作成は、まだ、存在しません。</span><span class="sxs-lookup"><span data-stu-id="33cbc-217">This class doesn't exist yet, so you will create it now.</span></span>

1. <span data-ttu-id="33cbc-218">**ソリューション エクスプ ローラー**を右クリックし、*ロジック*クリックしてフォルダー**追加** - &gt; **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-218">In **Solution Explorer**, right-click the *Logic* folder and then select **Add** -&gt; **New Item**.</span></span>   
 <span data-ttu-id="33cbc-219">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-219">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="33cbc-220">選択、 **Visual c#**  - &gt; **コード**左側のテンプレートのグループです。</span><span class="sxs-lookup"><span data-stu-id="33cbc-220">Select the **Visual C#** -&gt; **Code** templates group on the left.</span></span> <span data-ttu-id="33cbc-221">次に、選択**クラス**中央から一覧表示し、名前を付けます*AddProducts.cs*です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-221">Then, select **Class**from the middle list and name it *AddProducts.cs*.</span></span>   
 <span data-ttu-id="33cbc-222">新しいクラス ファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-222">The new class file is displayed.</span></span>
3. <span data-ttu-id="33cbc-223">既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-223">Replace the existing code with the following:</span></span>  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

<span data-ttu-id="33cbc-224">*AdminPage.aspx*ページでは、"canEdit"ロールに所属するユーザーを追加して、製品を削除します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-224">The *AdminPage.aspx* page allows the user belonging to the "canEdit" role to add and remove products.</span></span> <span data-ttu-id="33cbc-225">新しい製品が追加されると、製品に関する詳細が検証し、データベースに入力します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-225">When a new product is added, the details about the product are validated and then entered into the database.</span></span> <span data-ttu-id="33cbc-226">新しい製品では、web アプリケーションのすべてのユーザーにすぐに使用できます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-226">The new product is immediately available to all users of the web application.</span></span>

#### <a name="unobtrusive-validation"></a><span data-ttu-id="33cbc-227">控えめな検証</span><span class="sxs-lookup"><span data-stu-id="33cbc-227">Unobtrusive Validation</span></span>

<span data-ttu-id="33cbc-228">製品の詳細については、ユーザーを*AdminPage.aspx*ページは、検証コントロールを使用して検証されます (`RequiredFieldValidator`と`RegularExpressionValidator`)。</span><span class="sxs-lookup"><span data-stu-id="33cbc-228">The product details that the user provides on the *AdminPage.aspx* page are validated using validation controls (`RequiredFieldValidator` and `RegularExpressionValidator`).</span></span> <span data-ttu-id="33cbc-229">これらのコントロールは、控えめな検証を自動的に使用します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-229">These controls automatically use unobtrusive validation.</span></span> <span data-ttu-id="33cbc-230">控えめな検証では、ため、ページが検証に使用するサーバーへのトリップが必要ないクライアント側の検証ロジックは、JavaScript を使用する検証コントロールを使用します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-230">Unobtrusive validation allows the validation controls to use JavaScript for client-side validation logic, which means the page does not require a trip to the server to be validated.</span></span> <span data-ttu-id="33cbc-231">既定では、控えめな検証が含まれている、 *Web.config*ファイルは、次の構成設定に基づきます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-231">By default, unobtrusive validation is included in the *Web.config* file based on the following configuration setting:</span></span>

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a><span data-ttu-id="33cbc-232">正規表現</span><span class="sxs-lookup"><span data-stu-id="33cbc-232">Regular Expressions</span></span>

<span data-ttu-id="33cbc-233">製品の価格、 *AdminPage.aspx*を使用してページを検証、 **RegularExpressionValidator**コントロール。</span><span class="sxs-lookup"><span data-stu-id="33cbc-233">The product price on the *AdminPage.aspx* page is validated using a **RegularExpressionValidator** control.</span></span> <span data-ttu-id="33cbc-234">このコントロールは、関連付けられた入力コントロール ("AddProductPrice"TextBox) の値が正規表現で指定されたパターンに一致するかどうかを検証します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-234">This control validates whether the value of the associated input control (the "AddProductPrice" TextBox) matches the pattern specified by the regular expression.</span></span> <span data-ttu-id="33cbc-235">正規表現とは、パターン一致表記法をすばやく検索および特定の文字パターンに一致することができます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-235">A regular expression is a pattern-matching notation that enables you to quickly find and match specific character patterns.</span></span> <span data-ttu-id="33cbc-236">**RegularExpressionValidator**コントロールには、という名前のプロパティが含まれています。`ValidationExpression`次に示すように、価格の入力の検証に使用される正規表現を格納しています。</span><span class="sxs-lookup"><span data-stu-id="33cbc-236">The **RegularExpressionValidator** control includes a property named `ValidationExpression` that contains the regular expression used to validate price input, as shown below:</span></span>

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a><span data-ttu-id="33cbc-237">ファイルアップロード コントロール</span><span class="sxs-lookup"><span data-stu-id="33cbc-237">FileUpload Control</span></span>

<span data-ttu-id="33cbc-238">追加しただけでなく、入力との検証コントロール、**ファイルアップロード**コントロールを*AdminPage.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="33cbc-238">In addition to the input and validation controls, you added the **FileUpload** control to the *AdminPage.aspx* page.</span></span> <span data-ttu-id="33cbc-239">このコントロールは、ファイルをアップロードする機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-239">This control provides the capability to upload files.</span></span> <span data-ttu-id="33cbc-240">この場合、のみを許可するイメージ ファイルをアップロードします。</span><span class="sxs-lookup"><span data-stu-id="33cbc-240">In this case, you are only allowing image files to be uploaded.</span></span> <span data-ttu-id="33cbc-241">分離コード ファイル内 (*AdminPage.aspx.cs*)、ときに、`AddProductButton`がクリックすると、コードのチェック、`HasFile`のプロパティ、**ファイルアップロード**コントロール。</span><span class="sxs-lookup"><span data-stu-id="33cbc-241">In the code-behind file (*AdminPage.aspx.cs*), when the `AddProductButton` is clicked, the code checks the `HasFile` property of the **FileUpload** control.</span></span> <span data-ttu-id="33cbc-242">コントロールにファイルがあると、イメージに保存 (ファイル拡張子に基づいた) ファイルの種類が許可された場合、*イメージ*フォルダーおよび*イメージ/親指*アプリケーションのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="33cbc-242">If the control has a file and if the file type (based on file extension) is allowed, the image is saved to the *Images* folder and the *Images/Thumbs* folder of the application.</span></span>

#### <a name="model-binding"></a><span data-ttu-id="33cbc-243">モデル バインディング</span><span class="sxs-lookup"><span data-stu-id="33cbc-243">Model Binding</span></span>

<span data-ttu-id="33cbc-244">このチュートリアルの系列で前を設定するモデル バインドを使用して、 **ListView**コントロール、 **FormsView**コントロール、 **GridView**コントロール、および**DetailView**コントロール。</span><span class="sxs-lookup"><span data-stu-id="33cbc-244">Earlier in this tutorial series you used model binding to populate a **ListView** control, a **FormsView** control, a **GridView** control, and a **DetailView** control.</span></span> <span data-ttu-id="33cbc-245">このチュートリアルではモデル バインディングを使用する設定、 **DropDownList**製品カテゴリの一覧を制御します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-245">In this tutorial, you use model binding to populate a **DropDownList** control with a list of product categories.</span></span>

<span data-ttu-id="33cbc-246">追加するマークアップを*AdminPage.aspx*ファイルが含まれています、 **DropDownList**と呼ばれるコントロール`DropDownAddCategory`:</span><span class="sxs-lookup"><span data-stu-id="33cbc-246">The markup that you added to the *AdminPage.aspx* file contains a **DropDownList** control called `DropDownAddCategory`:</span></span>

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

<span data-ttu-id="33cbc-247">これを設定するモデル バインドを使用する**DropDownList**を設定して、`ItemType`属性および`SelectMethod`属性。</span><span class="sxs-lookup"><span data-stu-id="33cbc-247">You use model binding to populate this **DropDownList** by setting the `ItemType` attribute and the `SelectMethod` attribute.</span></span> <span data-ttu-id="33cbc-248">`ItemType`属性は、使用することを指定します、`WingtipToys.Models.Category`コントロールを設定するときに入力します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-248">The `ItemType` attribute specifies that you use the `WingtipToys.Models.Category` type when populating the control.</span></span> <span data-ttu-id="33cbc-249">このチュートリアル シリーズの先頭には、この型を作成することで定義した、`Category`クラス (下図参照)。</span><span class="sxs-lookup"><span data-stu-id="33cbc-249">You defined this type at the beginning of this tutorial series by creating the `Category` class (shown below).</span></span> <span data-ttu-id="33cbc-250">`Category`クラスが含まれて、*モデル*内のフォルダー、 *Category.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="33cbc-250">The `Category` class is in the *Models* folder inside the *Category.cs* file.</span></span>

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

<span data-ttu-id="33cbc-251">`SelectMethod`の属性、 **DropDownList**コントロールは、使用することを指定します、`GetCategories`メソッド (下図参照) は、分離コード ファイルに含まれる (*AdminPage.aspx.cs*)。</span><span class="sxs-lookup"><span data-stu-id="33cbc-251">The `SelectMethod` attribute of the **DropDownList** control specifies that you use the `GetCategories` method (shown below) that is included in the code-behind file (*AdminPage.aspx.cs*).</span></span>

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

<span data-ttu-id="33cbc-252">このメソッドを指定する、`IQueryable`に対するクエリを評価するインターフェイスを使用する`Category`型です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-252">This method specifies that an `IQueryable` interface is used to evaluate a query against a `Category` type.</span></span> <span data-ttu-id="33cbc-253">返された値が設定に使用されます、 **DropDownList**ページのマークアップ (*AdminPage.aspx*)。</span><span class="sxs-lookup"><span data-stu-id="33cbc-253">The returned value is used to populate the **DropDownList** in the markup of the page (*AdminPage.aspx*).</span></span>

<span data-ttu-id="33cbc-254">設定して、リスト内の各項目が指定されて表示されるテキスト、`DataTextField`属性。</span><span class="sxs-lookup"><span data-stu-id="33cbc-254">The text displayed for each item in the list is specified by setting the `DataTextField` attribute.</span></span> <span data-ttu-id="33cbc-255">`DataTextField`属性の使用、`CategoryName`の`Category`クラス内の各カテゴリを表示する (上記)、 **DropDownList**コントロール。</span><span class="sxs-lookup"><span data-stu-id="33cbc-255">The `DataTextField` attribute uses the `CategoryName` of the `Category` class (shown above) to display each category in the **DropDownList** control.</span></span> <span data-ttu-id="33cbc-256">項目が選択したときに渡される実際の値、 **DropDownList**コントロールがに基づいて、`DataValueField`属性。</span><span class="sxs-lookup"><span data-stu-id="33cbc-256">The actual value that is passed when an item is selected in the **DropDownList** control is based on the `DataValueField` attribute.</span></span> <span data-ttu-id="33cbc-257">`DataValueField`属性に設定されている、`CategoryID`での定義、 `Category` (上記) クラス。</span><span class="sxs-lookup"><span data-stu-id="33cbc-257">The `DataValueField` attribute is set to the `CategoryID` as define in the `Category` class (shown above).</span></span>

### <a name="how-the-application-will-work"></a><span data-ttu-id="33cbc-258">アプリケーションがどのように動作</span><span class="sxs-lookup"><span data-stu-id="33cbc-258">How the Application Will Work</span></span>

<span data-ttu-id="33cbc-259">"CanEdit"ロールに属しているユーザーが最初に、ページに移動すると、 `DropDownAddCategory` **DropDownList**前述のようにコントロールが設定されます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-259">When the user belonging to the "canEdit" role navigates to the page for the first time, the `DropDownAddCategory`**DropDownList** control is populated as described above.</span></span> <span data-ttu-id="33cbc-260">`DropDownRemoveProduct` **DropDownList**コントロールが、これと同じアプローチを使用して製品も表示されます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-260">The `DropDownRemoveProduct`**DropDownList** control is also populated with products using the same approach.</span></span> <span data-ttu-id="33cbc-261">"CanEdit"ロールに属しているユーザーは、カテゴリの種類を選択し、製品の詳細を追加します (**名前**、**説明**、**価格**、および**のイメージファイル**).</span><span class="sxs-lookup"><span data-stu-id="33cbc-261">The user belonging to the "canEdit" role selects the category type and adds product details (**Name**, **Description**, **Price**, and **Image File**).</span></span> <span data-ttu-id="33cbc-262">"CanEdit"ロールに属しているユーザーがクリックしたとき、**製品の追加** ボタン、`AddProductButton_Click`のイベント ハンドラーを起動します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-262">When the user belonging to the "canEdit" role clicks the **Add Product** button, the `AddProductButton_Click` event handler is triggered.</span></span> <span data-ttu-id="33cbc-263">`AddProductButton_Click`イベント ハンドラーが、分離コード ファイルにあります (*AdminPage.aspx.cs*) 許可されているファイルの種類と一致するかどうかを確認するイメージ ファイルをチェック*(.gif*、 *.png*、 *.jpeg*、または*.jpg*)。</span><span class="sxs-lookup"><span data-stu-id="33cbc-263">The `AddProductButton_Click` event handler located in the code-behind file (*AdminPage.aspx.cs*) checks the image file to make sure it matches the allowed file types *(.gif*, *.png*, *.jpeg*, or *.jpg*).</span></span> <span data-ttu-id="33cbc-264">その後、イメージ ファイルは、Wingtip Toys のサンプル アプリケーションのフォルダーに保存されます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-264">Then, the image file is saved into a folder of the Wingtip Toys sample application.</span></span> <span data-ttu-id="33cbc-265">次に、新しい製品は、データベースに追加されます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-265">Next, the new product is added to the database.</span></span> <span data-ttu-id="33cbc-266">新しい製品の新しいインスタンスを追加するために、`AddProducts`クラスが作成され、製品をという名前です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-266">To accomplish adding a new product, a new instance of the `AddProducts` class is created and named products.</span></span> <span data-ttu-id="33cbc-267">`AddProducts`クラスという名前のメソッドには`AddProduct`、製品オブジェクトが製品をデータベースに追加するには、このメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-267">The `AddProducts` class has a method named `AddProduct`, and the products object calls this method to add products to the database.</span></span>

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

<span data-ttu-id="33cbc-268">クエリ文字列の値、ページが再読み込みされるコードが正常に追加する場合、新しい製品をデータベースに`ProductAction=add`です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-268">If the code successfully adds the new product to the database, the page is reloaded with the query string value `ProductAction=add`.</span></span>

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

<span data-ttu-id="33cbc-269">ページが再読み込み時に、URL にクエリ文字列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="33cbc-269">When the page reloads, the query string is included in the URL.</span></span> <span data-ttu-id="33cbc-270">ページを再度読み込んで、によって"canEdit"ロールに属しているユーザーはすぐに内の更新プログラムを確認、 **DropDownList**コントロールに対して、 *AdminPage.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="33cbc-270">By reloading the page, the user belonging to the "canEdit" role can immediately see the updates in the **DropDownList** controls on the *AdminPage.aspx* page.</span></span> <span data-ttu-id="33cbc-271">また、URL を使用してクエリ文字列に含めても、ページを表示できます、成功メッセージ"canEdit"ロールに属しているユーザー。</span><span class="sxs-lookup"><span data-stu-id="33cbc-271">Also, by including the query string with the URL, the page can display a success message to the user belonging to the "canEdit" role.</span></span>

<span data-ttu-id="33cbc-272">ときに、 *AdminPage.aspx*再読み込みをページ、`Page_Load`イベントが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-272">When the *AdminPage.aspx* page reloads, the `Page_Load` event is called.</span></span>

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

<span data-ttu-id="33cbc-273">`Page_Load`イベント ハンドラーは、クエリ文字列値をチェックし、成功メッセージを表示するかどうかを決定します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-273">The `Page_Load` event handler checks the query string value and determines whether to show a success message.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="33cbc-274">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="33cbc-274">Running the Application</span></span>

<span data-ttu-id="33cbc-275">今すぐアプリケーションを追加する方法を表示、削除、および更新アイテムは、ショッピング カートで実行できます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-275">You can run the application now to see how you can add, delete, and update items in the shopping cart.</span></span> <span data-ttu-id="33cbc-276">ショッピング カートの合計金額、ショッピング カート内のすべての項目の総コストが反映されます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-276">The shopping cart total will reflect the total cost of all items in the shopping cart.</span></span>

1. <span data-ttu-id="33cbc-277">ソリューション エクスプ ローラーでキーを押して**f5 キーを押して**Wingtip Toys サンプル アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-277">In Solution Explorer, press **F5** to run the Wingtip Toys sample application.</span></span>  
 <span data-ttu-id="33cbc-278">ブラウザーが開き、表示、 *Default.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="33cbc-278">The browser opens and shows the *Default.aspx* page.</span></span>
2. <span data-ttu-id="33cbc-279">クリックして、**ログイン**ページの上部にあるリンクします。</span><span class="sxs-lookup"><span data-stu-id="33cbc-279">Click the **Log in** link at the top of the page.</span></span> 

    ![メンバーシップと管理 - ログイン リンク](membership-and-administration/_static/image2.png)

 <span data-ttu-id="33cbc-281">*Login.aspx*ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="33cbc-281">The *Login.aspx* page is displayed.</span></span>
3. <span data-ttu-id="33cbc-282">次のユーザー名とパスワードを使用します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-282">Use the following user name and password:</span></span>  
 <span data-ttu-id="33cbc-283">ユーザー名:canEditUser@wingtiptoys.com</span><span class="sxs-lookup"><span data-stu-id="33cbc-283">User name: canEditUser@wingtiptoys.com</span></span>  
 <span data-ttu-id="33cbc-284">パスワード: Pa $word1</span><span class="sxs-lookup"><span data-stu-id="33cbc-284">Password: Pa$$word1</span></span> 

    ![メンバーシップと管理 - ログイン ページ](membership-and-administration/_static/image3.png)
4. <span data-ttu-id="33cbc-286">クリックして、**ログイン**ページの下部にあるボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="33cbc-286">Click the **Log in** button near the bottom of the page.</span></span>
5. <span data-ttu-id="33cbc-287">次のページの上部には、選択、 **Admin**に移動するリンク、 *AdminPage.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="33cbc-287">At the top of the next page, select the **Admin** link to navigate to the *AdminPage.aspx* page.</span></span> 

    ![メンバーシップと管理 - Admin リンク](membership-and-administration/_static/image4.png)
6. <span data-ttu-id="33cbc-289">入力の検証をテストするには、クリックして、**製品の追加**製品詳細を追加することがなくボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="33cbc-289">To test the input validation, click the **Add Product** button without adding any product details.</span></span> 

    ![メンバーシップと管理 - [管理] ページ](membership-and-administration/_static/image5.png)

 <span data-ttu-id="33cbc-291">必須フィールドのメッセージが表示されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="33cbc-291">Notice that the required field messages are displayed.</span></span>
7. <span data-ttu-id="33cbc-292">新しい製品では、詳細を追加して、をクリックして、**製品の追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="33cbc-292">Add the details for a new product, and then click the **Add Product** button.</span></span> 

    ![メンバーシップと管理 - 製品を追加します。](membership-and-administration/_static/image6.png)
8. <span data-ttu-id="33cbc-294">選択**製品**追加してから、上部のナビゲーション メニューを新しい製品を表示します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-294">Select **Products** from the top navigation menu to view the new product you added.</span></span> 

    ![メンバーシップと管理 - 新しい製品を表示します。](membership-and-administration/_static/image7.png)
9. <span data-ttu-id="33cbc-296">をクリックして、 **Admin**管理ページに戻るにリンクします。</span><span class="sxs-lookup"><span data-stu-id="33cbc-296">Click the **Admin** link to return to the administration page.</span></span>
10. <span data-ttu-id="33cbc-297">**削除製品**セクションで追加した新しい製品を選択、ページの**DropDownListBox**です。</span><span class="sxs-lookup"><span data-stu-id="33cbc-297">In the **Remove Product** section of the page, select the new product you added in the **DropDownListBox**.</span></span>
11. <span data-ttu-id="33cbc-298">をクリックして、**製品の削除**アプリケーションから、新しい製品を削除するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="33cbc-298">Click the **Remove Product** button to remove the new product from the application.</span></span> 

    ![メンバーシップと管理 - 削除製品](membership-and-administration/_static/image8.png)
12. <span data-ttu-id="33cbc-300">選択**製品**製品が削除されたことを確認する、上部のナビゲーション メニューからです。</span><span class="sxs-lookup"><span data-stu-id="33cbc-300">Select **Products** from the top navigation menu to confirm that the product has been removed.</span></span>
13. <span data-ttu-id="33cbc-301">をクリックして**ログオフ**管理モードが存在します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-301">Click **Log off** to exist administration mode.</span></span>   
 <span data-ttu-id="33cbc-302">上部のナビゲーション ウィンドウは表示されなくなりますに注意してください、 **Admin**メニュー項目。</span><span class="sxs-lookup"><span data-stu-id="33cbc-302">Notice that the top navigation pane no longer shows the **Admin** menu item.</span></span>

## <a name="summary"></a><span data-ttu-id="33cbc-303">概要</span><span class="sxs-lookup"><span data-stu-id="33cbc-303">Summary</span></span>

<span data-ttu-id="33cbc-304">このチュートリアルでは、カスタムのロールとカスタムのロール ページで、管理フォルダーへのアクセス制限に属しているユーザーを追加し、カスタムのロールに属しているユーザーのナビゲーションを指定しました。</span><span class="sxs-lookup"><span data-stu-id="33cbc-304">In this tutorial, you added a custom role and a user belonging to the custom role, restricted access to the administration folder and page, and provided navigation for the user belonging to the custom role.</span></span> <span data-ttu-id="33cbc-305">モデル バインディングの設定に使用する、 **DropDownList**コントロールにデータ。</span><span class="sxs-lookup"><span data-stu-id="33cbc-305">You used model binding to populate a **DropDownList** control with data.</span></span> <span data-ttu-id="33cbc-306">実装する、**ファイルアップロード**コントロールと検証コントロール。</span><span class="sxs-lookup"><span data-stu-id="33cbc-306">You implemented the **FileUpload** control and validation controls.</span></span> <span data-ttu-id="33cbc-307">また、追加し、データベースから製品を削除する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="33cbc-307">Also, you have learned how to add and remove products from a database.</span></span> <span data-ttu-id="33cbc-308">次のチュートリアルでは、ASP.NET のルーティングを実装する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-308">In the next tutorial, you'll learn how to implement ASP.NET routing.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="33cbc-309">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="33cbc-309">Additional Resources</span></span>

<span data-ttu-id="33cbc-310">[Web.config で承認要素](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="33cbc-310">[Web.config - authorization Element](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)</span></span>  
[<span data-ttu-id="33cbc-311">ASP.NET Id</span><span class="sxs-lookup"><span data-stu-id="33cbc-311">ASP.NET Identity</span></span>](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[<span data-ttu-id="33cbc-312">Azure の Web サイトにメンバーシップ、OAuth、SQL データベースでのセキュリティで保護された ASP.NET Web フォーム アプリケーションを展開します。</span><span class="sxs-lookup"><span data-stu-id="33cbc-312">Deploy a Secure ASP.NET Web Forms App with Membership, OAuth, and SQL Database to an Azure Web Site</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[<span data-ttu-id="33cbc-313">Microsoft Azure の無料試用版</span><span class="sxs-lookup"><span data-stu-id="33cbc-313">Microsoft Azure - Free Trial</span></span>](https://azure.microsoft.com/pricing/free-trial/)

>[!div class="step-by-step"]
<span data-ttu-id="33cbc-314">[前へ](checkout-and-payment-with-paypal.md)
[次へ](url-routing.md)</span><span class="sxs-lookup"><span data-stu-id="33cbc-314">[Previous](checkout-and-payment-with-paypal.md)
[Next](url-routing.md)</span></span>
