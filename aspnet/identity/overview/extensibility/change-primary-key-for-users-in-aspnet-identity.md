---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: ASP.NET Identity でユーザーのプライマリ キーの変更 |Microsoft Docs
author: tfitzmac
description: Visual Studio 2013 では、既定の web アプリケーションは、ユーザー アカウントのキーの文字列値を使用します。 ASP.NET Identity では、型を変更することができます、.
ms.author: riande
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 2ec9894df2f9a48ef482715ce71bb09fb4758127
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837140"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="2c1df-104">ASP.NET Identity でユーザーのプライマリ キーの変更</span><span class="sxs-lookup"><span data-stu-id="2c1df-104">Change Primary Key for Users in ASP.NET Identity</span></span>
====================
<span data-ttu-id="2c1df-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2c1df-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2c1df-106">Visual Studio 2013 では、既定の web アプリケーションは、ユーザー アカウントのキーの文字列値を使用します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="2c1df-107">ASP.NET Identity では、データの要件を満たすために、キーの種類を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="2c1df-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="2c1df-108">たとえば、文字列から整数に、キーの種類を変更できます。</span><span class="sxs-lookup"><span data-stu-id="2c1df-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="2c1df-109">このトピックでは、既定の web アプリケーションとユーザーのアカウント キーを整数に変更から始める方法を示します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="2c1df-110">プロジェクト内の任意の種類のキーを実装するために、同じ変更を使用できます。</span><span class="sxs-lookup"><span data-stu-id="2c1df-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="2c1df-111">既定の web アプリケーションでこれらを変更する方法を示しますが、カスタマイズされたアプリケーションのような変更を適用する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2c1df-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="2c1df-112">MVC または Web フォームを使用する場合に必要な変更が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2c1df-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2c1df-113">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="2c1df-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2c1df-114">Visual Studio 2013 with Update 2 (またはそれ以降)</span><span class="sxs-lookup"><span data-stu-id="2c1df-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="2c1df-115">ASP.NET Identity 2.1 以降</span><span class="sxs-lookup"><span data-stu-id="2c1df-115">ASP.NET Identity 2.1 or later</span></span>


<span data-ttu-id="2c1df-116">このチュートリアルでは、手順を実行するには、Visual Studio 2013 Update 2 (またはそれ以降) と ASP.NET Web アプリケーション テンプレートから作成された web アプリケーションが必要です。</span><span class="sxs-lookup"><span data-stu-id="2c1df-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="2c1df-117">更新プログラム 3 で変更するテンプレートです。</span><span class="sxs-lookup"><span data-stu-id="2c1df-117">The template changed in Update 3.</span></span> <span data-ttu-id="2c1df-118">このトピックでは、更新プログラム 2 と Update 3 でテンプレートを変更する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="2c1df-119">このトピックは、次のセクションで構成されています。</span><span class="sxs-lookup"><span data-stu-id="2c1df-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="2c1df-120">Id のユーザー クラスのキーの種類を変更します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="2c1df-121">キーの種類を使用して、カスタマイズされた Id のクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="2c1df-122">キーの種類を使用するコンテキスト クラスおよびユーザーのマネージャーを変更します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="2c1df-123">キーの種類を使用するスタートアップ構成の変更</span><span class="sxs-lookup"><span data-stu-id="2c1df-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="2c1df-124">Update 2 と MVC でのキーの型を渡す AccountController を変更します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="2c1df-125">更新プログラム 3 での MVC、キーの型を渡す AccountController と ManageController を変更します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="2c1df-126">更新プログラム 2 で、Web フォームのキーの型を渡すアカウント ページを変更します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="2c1df-127">Update 3 では、Web フォームのキーの型を渡すアカウント ページを変更します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="2c1df-128">アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-128">Run application</span></span>](#run)
- [<span data-ttu-id="2c1df-129">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="2c1df-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="2c1df-130">Id のユーザー クラスのキーの種類を変更します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="2c1df-131">ASP.NET Web アプリケーション テンプレートから作成されたプロジェクトで、ApplicationUser クラスで整数を使用して、ユーザー アカウントのキーのことを指定します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="2c1df-132">IdentityModels.cs、変更、ApplicationUser クラスの型を持つ IdentityUser から継承する**int** TKey ジェネリック パラメーター。</span><span class="sxs-lookup"><span data-stu-id="2c1df-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="2c1df-133">また、まだ実装していない場合が 3 つのカスタマイズしたクラスの名前を渡します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="2c1df-134">キーの種類を変更しましたが、既定では、アプリケーションの残りの部分も前提としています、キーは、文字列です。</span><span class="sxs-lookup"><span data-stu-id="2c1df-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="2c1df-135">文字列が想定するコード内のキーの型を明示的に示す必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c1df-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="2c1df-136">**ApplicationUser**クラス、変更、 **GenerateUserIdentityAsync**メソッドは以下の強調表示されたコードに示すように、int を含めます。</span><span class="sxs-lookup"><span data-stu-id="2c1df-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="2c1df-137">この変更は、更新プログラム 3 のテンプレートを使用した Web フォーム プロジェクトの必要はありません。</span><span class="sxs-lookup"><span data-stu-id="2c1df-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="2c1df-138">キーの種類を使用して、カスタマイズされた Id のクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="2c1df-139">IdentityUserRole、IdentityUserClaim、IdentityUserLogin、IdentityRole、UserStore、RoleStore など、他の Id クラスは、文字列キーを使用するをまだ設定します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="2c1df-140">これらのキーを示す整数を指定するクラスの新しいバージョンを作成します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="2c1df-141">これらのクラスで多くの実装コードを提供する必要はありません、キーとして int を設定するだけでは主にします。</span><span class="sxs-lookup"><span data-stu-id="2c1df-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="2c1df-142">IdentityModels.cs ファイルに次のクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="2c1df-143">キーの種類を使用するコンテキスト クラスおよびユーザーのマネージャーを変更します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="2c1df-144">IdentityModels.cs での定義を変更、 **ApplicationDbContext**クラスを使用して、新しいクラスをカスタマイズして、 **int**の強調表示されたコードに示すように、キー。</span><span class="sxs-lookup"><span data-stu-id="2c1df-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="2c1df-145">ThrowIfV1Schema パラメーターは、コンス トラクターでは有効ではありません。</span><span class="sxs-lookup"><span data-stu-id="2c1df-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="2c1df-146">ThrowIfV1Schema 値に合格していないため、コンス トラクターを変更します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="2c1df-147">IdentityConfig.cs を開き、変更、 **ApplicationUserManger**クラスの新しいユーザーを使用するストアのデータを保持するクラスと**int**キー。</span><span class="sxs-lookup"><span data-stu-id="2c1df-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="2c1df-148">更新プログラム 3 のテンプレートでは、ApplicationSignInManager クラスを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c1df-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="2c1df-149">キーの種類を使用するスタートアップ構成の変更</span><span class="sxs-lookup"><span data-stu-id="2c1df-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="2c1df-150">Startup.Auth.cs で以下の強調表示されている、OnValidateIdentity コードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2c1df-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="2c1df-151">GetUserIdCallback の定義を整数に文字列値を解析することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="2c1df-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="2c1df-152">プロジェクトがの汎用実装を認識しないかどうか、 **GetUserId**メソッドでは、バージョン 2.1 に ASP.NET Identity の NuGet パッケージを更新する必要があります</span><span class="sxs-lookup"><span data-stu-id="2c1df-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="2c1df-153">ASP.NET Identity で使用されるインフラストラクチャ クラスに多くの変更を加えました。</span><span class="sxs-lookup"><span data-stu-id="2c1df-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="2c1df-154">プロジェクトをコンパイルしようとすると、多くのエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2c1df-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="2c1df-155">さいわい、残りのエラーは、すべてと似ています。</span><span class="sxs-lookup"><span data-stu-id="2c1df-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="2c1df-156">Id クラスには、キーの整数が必要がありますが、コント ローラー (または Web フォーム) が文字列値を渡すこと。</span><span class="sxs-lookup"><span data-stu-id="2c1df-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="2c1df-157">各ケースでは、呼び出すことによって、文字列と整数から変換する必要があります**GetUserId&lt;int&gt;** します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="2c1df-158">コンパイルのエラー一覧を使用するか、以下の変更を追跡します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="2c1df-159">残りの変更は、プロジェクトを作成して、Visual Studio でインストールした更新プログラムの種類によって異なります。</span><span class="sxs-lookup"><span data-stu-id="2c1df-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="2c1df-160">次のリンクから、関連するセクションに直接移動することができます。</span><span class="sxs-lookup"><span data-stu-id="2c1df-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="2c1df-161">Update 2 と MVC でのキーの型を渡す AccountController を変更します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="2c1df-162">更新プログラム 3 での MVC、キーの型を渡す AccountController と ManageController を変更します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="2c1df-163">更新プログラム 2 で、Web フォームのキーの型を渡すアカウント ページを変更します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="2c1df-164">Update 3 では、Web フォームのキーの型を渡すアカウント ページを変更します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="2c1df-165">Update 2 と MVC でのキーの型を渡す AccountController を変更します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="2c1df-166">AccountController.cs ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="2c1df-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="2c1df-167">次のメソッドを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c1df-167">You need to change the following methods.</span></span>

<span data-ttu-id="2c1df-168">**ConfirmEmail**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="2c1df-169">**関連付けを解除**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="2c1df-170">**Manage(ManageUserViewModel)** メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="2c1df-171">**LinkLoginCallback**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="2c1df-172">**RemoveAccountList**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="2c1df-173">**HasPassword**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="2c1df-174">できるようになりました[アプリケーションを実行](#run)し、新しいユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="2c1df-175">更新プログラム 3 での MVC、キーの型を渡す AccountController と ManageController を変更します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="2c1df-176">AccountController.cs ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="2c1df-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="2c1df-177">次の方法を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c1df-177">You need to change the following method.</span></span>

<span data-ttu-id="2c1df-178">**ConfirmEmail**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="2c1df-179">**SendCode**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="2c1df-180">ManageController.cs ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="2c1df-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="2c1df-181">次のメソッドを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c1df-181">You need to change the following methods.</span></span>

<span data-ttu-id="2c1df-182">**インデックス**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="2c1df-183">**RemoveLogin**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="2c1df-184">**AddPhoneNumber**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="2c1df-185">**EnableTwoFactorAuthentication**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="2c1df-186">**DisableTwoFactorAuthentication**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="2c1df-187">**VerifyPhoneNumber**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="2c1df-188">**RemovePhoneNumber**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="2c1df-189">**ChangePassword**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="2c1df-190">**SetPassword**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="2c1df-191">**ManageLogins**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="2c1df-192">**LinkLoginCallback**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="2c1df-193">**HasPassword**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="2c1df-194">**HasPhoneNumber**メソッド</span><span class="sxs-lookup"><span data-stu-id="2c1df-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="2c1df-195">できるようになりました[アプリケーションを実行](#run)し、新しいユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="2c1df-196">更新プログラム 2 で、Web フォームのキーの型を渡すアカウント ページを変更します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="2c1df-197">Update 2 の Web フォーム、次のページを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c1df-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="2c1df-198">**Confirm.aspx.cx**</span><span class="sxs-lookup"><span data-stu-id="2c1df-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="2c1df-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="2c1df-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="2c1df-200">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="2c1df-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="2c1df-201">できるようになりました[アプリケーションを実行](#run)し、新しいユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="2c1df-202">Update 3 では、Web フォームのキーの型を渡すアカウント ページを変更します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="2c1df-203">Update 3 では、Web フォーム、次のページを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c1df-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="2c1df-204">**Confirm.aspx.cx**</span><span class="sxs-lookup"><span data-stu-id="2c1df-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="2c1df-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="2c1df-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="2c1df-206">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="2c1df-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="2c1df-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="2c1df-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="2c1df-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="2c1df-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="2c1df-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="2c1df-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="2c1df-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="2c1df-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="2c1df-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="2c1df-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="2c1df-212">アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-212">Run application</span></span>

<span data-ttu-id="2c1df-213">すべての既定の Web アプリケーション テンプレートに必要な変更が完了しました。</span><span class="sxs-lookup"><span data-stu-id="2c1df-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="2c1df-214">アプリケーションを実行し、新しいユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-214">Run the application and register a new user.</span></span> <span data-ttu-id="2c1df-215">ユーザーを登録すると、AspNetUsers テーブルの整数である Id 列であることがわかります。</span><span class="sxs-lookup"><span data-stu-id="2c1df-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![新しいプライマリ キー](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="2c1df-217">以前に作成した ASP.NET Identity テーブル別のプライマリ キーを持つ場合は、いくつか変更を加える必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c1df-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="2c1df-218">可能であれば、既存のデータベースを削除します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="2c1df-219">Web アプリケーションを実行し、新しいユーザーを追加する場合に、データベースは、適切なデザインで再作成になります。</span><span class="sxs-lookup"><span data-stu-id="2c1df-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="2c1df-220">削除できない場合は、code first migrations のテーブルを変更するを実行します。</span><span class="sxs-lookup"><span data-stu-id="2c1df-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="2c1df-221">ただし、新しい整数の主キーがある設定されていません、データベース内の SQL ID プロパティとして。</span><span class="sxs-lookup"><span data-stu-id="2c1df-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="2c1df-222">Id 列は、ID として手動で設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c1df-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="2c1df-223">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="2c1df-223">Other resources</span></span>

- [<span data-ttu-id="2c1df-224">ASP.NET Identity のカスタム ストレージ プロバイダーの概要</span><span class="sxs-lookup"><span data-stu-id="2c1df-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="2c1df-225">既存 Web サイトを SQL メンバーシップから ASP.NET Identity に移行する</span><span class="sxs-lookup"><span data-stu-id="2c1df-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="2c1df-226">メンバーシップと ASP.NET identity のユーザー プロファイル用のユニバーサル プロバイダー データの移行</span><span class="sxs-lookup"><span data-stu-id="2c1df-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="2c1df-227">[サンプル アプリケーション](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt)主キーが変更されました。</span><span class="sxs-lookup"><span data-stu-id="2c1df-227">[Sample application](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) with changed primary key</span></span>
