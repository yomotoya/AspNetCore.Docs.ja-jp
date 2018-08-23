---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
title: フォーム認証 (VB) を使用したユーザー認証 |Microsoft Docs
author: microsoft
description: '[Authorize] 属性を使用する方法について説明します、MVC アプリケーションで特定のページで保護するパスワード。 Web サイトの管理をも使用する方法を学習します.'
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 4341f5b1-6fe5-44c5-8b8a-18fa84f80177
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: af91ae24cae505125dc237adfaa11b0ea4d60922
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836855"
---
<a name="authenticating-users-with-forms-authentication-vb"></a><span data-ttu-id="666b6-104">フォーム認証 (VB) でユーザーを認証します。</span><span class="sxs-lookup"><span data-stu-id="666b6-104">Authenticating Users with Forms Authentication (VB)</span></span>
====================
<span data-ttu-id="666b6-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="666b6-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="666b6-106">[Authorize] 属性を使用する方法について説明します、MVC アプリケーションで特定のページで保護するパスワード。</span><span class="sxs-lookup"><span data-stu-id="666b6-106">Learn how to use the [Authorize] attribute to password protect particular pages in your MVC application.</span></span> <span data-ttu-id="666b6-107">Web サイトの管理ツールを使用して作成し、ユーザーとロールを管理する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="666b6-107">You learn how to use the Web Site Administration Tool to create and manage users and roles.</span></span> <span data-ttu-id="666b6-108">また、ユーザー アカウントとロールの情報が格納される場所を構成する方法も説明します。</span><span class="sxs-lookup"><span data-stu-id="666b6-108">You also learn how to configure where user account and role information is stored.</span></span>


<span data-ttu-id="666b6-109">このチュートリアルの目的は、フォームを使用する方法について説明するパスワードへの認証は、ASP.NET MVC アプリケーション内のビューを保護します。</span><span class="sxs-lookup"><span data-stu-id="666b6-109">The goal of this tutorial is to explain how you can use Forms authentication to password protect the views in your ASP.NET MVC applications.</span></span> <span data-ttu-id="666b6-110">Web サイトの管理ツールを使用して、ユーザーとロールを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="666b6-110">You learn how to use the Web Site Administration Tool to create users and roles.</span></span> <span data-ttu-id="666b6-111">また、不正なユーザーがコント ローラー アクションを呼び出すことを防止する方法も説明します。</span><span class="sxs-lookup"><span data-stu-id="666b6-111">You also learn how to prevent unauthorized users from invoking controller actions.</span></span> <span data-ttu-id="666b6-112">最後に、ユーザー名とパスワードを格納する場所を構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="666b6-112">Finally, you learn how to configure where user names and passwords are stored.</span></span>

#### <a name="using-the-web-site-administration-tool"></a><span data-ttu-id="666b6-113">Web サイトの管理ツールを使用します。</span><span class="sxs-lookup"><span data-stu-id="666b6-113">Using the Web Site Administration Tool</span></span>

<span data-ttu-id="666b6-114">他のことを行う前にいくつかのユーザーとロールの作成から始めます。</span><span class="sxs-lookup"><span data-stu-id="666b6-114">Before we do anything else, we should start by creating some users and roles.</span></span> <span data-ttu-id="666b6-115">新しいユーザーとロールを作成する最も簡単な方法は、Visual Studio 2008 Web サイトの管理ツールを活用するためにです。</span><span class="sxs-lookup"><span data-stu-id="666b6-115">The easiest way to create new users and roles is to take advantage of the Visual Studio 2008 Web Site Administration Tool.</span></span> <span data-ttu-id="666b6-116">このツールを起動するには、メニュー オプションを選択して**プロジェクト、ASP.NET 構成**します。</span><span class="sxs-lookup"><span data-stu-id="666b6-116">You can launch this tool by selecting the menu option **Project, ASP.NET Configuration**.</span></span> <span data-ttu-id="666b6-117">または、ソリューション エクスプ ローラー ウィンドウの上部に表示される、世界中に達するハンマーの (多少恐ろしい) アイコンをクリックして、Web サイト管理ツールを起動できます (図 1 参照)。</span><span class="sxs-lookup"><span data-stu-id="666b6-117">Alternatively, you can launch the Web Site Administration Tool by clicking the (somewhat scary) icon of the hammer hitting the world that appears at the top of the Solution Explorer window (see Figure 1).</span></span>

<span data-ttu-id="666b6-118">**図 1 – Web サイトの管理ツールを起動します。**</span><span class="sxs-lookup"><span data-stu-id="666b6-118">**Figure 1 – Launching the Web Site Administration Tool**</span></span>

![clip_image002[4]](authenticating-users-with-forms-authentication-vb/_static/image1.jpg)

<span data-ttu-id="666b6-120">Web サイト管理ツールで、[セキュリティ] タブを選択して、新しいユーザーとロールを作成します。をクリックして、**ユーザーの作成**Stephen をという名前の新しいユーザーを作成するリンク (図 2 参照)。</span><span class="sxs-lookup"><span data-stu-id="666b6-120">Within the Web Site Administration Tool, you create new users and roles by selecting the Security tab. Click the **Create user** link to create a new user named Stephen (see Figure 2).</span></span> <span data-ttu-id="666b6-121">Stephen ユーザーが任意のパスワードを提供 (たとえば、*シークレット*)。</span><span class="sxs-lookup"><span data-stu-id="666b6-121">Provide the Stephen user with any password that you want (for example, *secret*).</span></span>

<span data-ttu-id="666b6-122">**図 2-新しいユーザーの作成**</span><span class="sxs-lookup"><span data-stu-id="666b6-122">**Figure 2 – Creating a new user**</span></span>

![clip_image004[4]](authenticating-users-with-forms-authentication-vb/_static/image2.jpg)

<span data-ttu-id="666b6-124">新しいロールを作成するには、最初の役割を有効にして 1 つまたは複数のロールを定義します。</span><span class="sxs-lookup"><span data-stu-id="666b6-124">You create new roles by first enabling roles and defining one or more roles.</span></span> <span data-ttu-id="666b6-125">クリックしてロールを有効にする、**の役割を有効にする**リンク。</span><span class="sxs-lookup"><span data-stu-id="666b6-125">Enable roles by clicking the **Enable roles** link.</span></span> <span data-ttu-id="666b6-126">という名前のロールを次に、作成*管理者*をクリックして、**を作成または管理ロール**(図 3 参照) をリンクします。</span><span class="sxs-lookup"><span data-stu-id="666b6-126">Next, create a role named *Administrators* by clicking the **Create or Manage roles** link (see Figure 3).</span></span>

<span data-ttu-id="666b6-127">**図 3 – 新しいロールの作成**</span><span class="sxs-lookup"><span data-stu-id="666b6-127">**Figure 3 – Creating a new role**</span></span>

![clip_image006[4]](authenticating-users-with-forms-authentication-vb/_static/image3.jpg)

<span data-ttu-id="666b6-129">最後に、Sally をという名前の新しいユーザーを作成し、ユーザーの作成リンクをクリックし、Sally を作成するときに、管理者を選択すると、Sally を管理者ロールに関連付ける (図 4 参照)。</span><span class="sxs-lookup"><span data-stu-id="666b6-129">Finally, create a new user named Sally and associate Sally with the Administrators role by clicking the Create User link and selecting Administrators when creating Sally (see Figure 4).</span></span>

<span data-ttu-id="666b6-130">**図 4 – ロールにユーザーを追加します。**</span><span class="sxs-lookup"><span data-stu-id="666b6-130">**Figure 4 – Adding a user to a role**</span></span>

![clip_image008[4]](authenticating-users-with-forms-authentication-vb/_static/image4.jpg)

<span data-ttu-id="666b6-132">すべての作業を行う、ときに、Stephen Sally という 2 つの新しいユーザーが必要です。</span><span class="sxs-lookup"><span data-stu-id="666b6-132">When all is said and done, you should have two new users named Stephen and Sally.</span></span> <span data-ttu-id="666b6-133">管理者をという名前の新しいロールも必要です。</span><span class="sxs-lookup"><span data-stu-id="666b6-133">You should also have a new role named Administrators.</span></span> <span data-ttu-id="666b6-134">Sally 管理者ロールのメンバーであるし、Stephen はありません。</span><span class="sxs-lookup"><span data-stu-id="666b6-134">Sally is a member of the Administrators role and Stephen is not.</span></span>

#### <a name="requiring-authorization"></a><span data-ttu-id="666b6-135">承認を必要とします。</span><span class="sxs-lookup"><span data-stu-id="666b6-135">Requiring Authorization</span></span>

<span data-ttu-id="666b6-136">ユーザーがアクションに [Authorize] 属性を追加することでコント ローラーのアクションを呼び出す前に認証されるユーザーを要求することができます。</span><span class="sxs-lookup"><span data-stu-id="666b6-136">You can require a user to be authenticated before the user invokes a controller action by adding the [Authorize] attribute to the action.</span></span> <span data-ttu-id="666b6-137">[Authorize] 属性を適用するには、個々 のコント ローラー アクションにまたは全体のコント ローラー クラスにこの属性を適用することができます。</span><span class="sxs-lookup"><span data-stu-id="666b6-137">You can apply the [Authorize] attribute to an individual controller action or you can apply this attribute to an entire controller class.</span></span>

<span data-ttu-id="666b6-138">たとえば、リスト 1 で、コント ローラーは、CompanySecrets() という名前のアクションを公開します。</span><span class="sxs-lookup"><span data-stu-id="666b6-138">For example, the controller in Listing 1 exposes an action named CompanySecrets().</span></span> <span data-ttu-id="666b6-139">このアクションが [Authorize] 属性で装飾されているため、この操作は、ユーザーが認証されていない場合に呼び出すことができません。</span><span class="sxs-lookup"><span data-stu-id="666b6-139">Because this action is decorated with the [Authorize] attribute, this action cannot be invoked unless a user is authenticated.</span></span>

<span data-ttu-id="666b6-140">**1 – Controllers\HomeController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="666b6-140">**Listing 1 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample1.vb)]

<span data-ttu-id="666b6-141">かどうかは、ブラウザーのアドレス バーに URL/Home/CompanySecrets を入力して CompanySecrets() アクションを呼び出すと、認証されたユーザーは、自動的にログイン ビューにリダイレクトされます (図 5 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="666b6-141">If you invoke the CompanySecrets() action by entering the URL /Home/CompanySecrets in the address bar of your browser, and you are not an authenticated user, then you will be redirected to the Login view automatically (see Figure 5).</span></span>

<span data-ttu-id="666b6-142">**図 5 – ログイン ビュー**</span><span class="sxs-lookup"><span data-stu-id="666b6-142">**Figure 5 – The Login view**</span></span>

![clip_image010 [4]](authenticating-users-with-forms-authentication-vb/_static/image5.jpg)

<span data-ttu-id="666b6-144">ログイン ビューを使用すると、ユーザー名とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="666b6-144">You can use the Login view to enter your user name and password.</span></span> <span data-ttu-id="666b6-145">登録済みユーザーがいないかどうかは、クリックすることができます、**登録**レジスタに移動するリンク (図 6 参照) を表示します。</span><span class="sxs-lookup"><span data-stu-id="666b6-145">If you are not a registered user then you can click the **register** link to navigate to the Register view (see Figure 6).</span></span> <span data-ttu-id="666b6-146">Register ビューを使用すると、新しいユーザー アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="666b6-146">You can use the Register view to create a new user account.</span></span>

<span data-ttu-id="666b6-147">**図 6 – Register ビュー**</span><span class="sxs-lookup"><span data-stu-id="666b6-147">**Figure 6 – The Register view**</span></span>

![clip_image012](authenticating-users-with-forms-authentication-vb/_static/image6.jpg)

<span data-ttu-id="666b6-149">正常にログインした後 (図 7 を参照してください) を表示する CompanySecrets を確認できます。</span><span class="sxs-lookup"><span data-stu-id="666b6-149">After you successfully log in, you can see the CompanySecrets view (see Figure 7).</span></span> <span data-ttu-id="666b6-150">既定では、ブラウザー ウィンドウを閉じるまでにログインする続けます。</span><span class="sxs-lookup"><span data-stu-id="666b6-150">By default, you will continue to be logged in until you close your browser window.</span></span>

<span data-ttu-id="666b6-151">**図 7-CompanySecrets ビュー**</span><span class="sxs-lookup"><span data-stu-id="666b6-151">**Figure 7 – The CompanySecrets view**</span></span>

![clip_image014](authenticating-users-with-forms-authentication-vb/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a><span data-ttu-id="666b6-153">ユーザー名またはユーザー ロールで認証します。</span><span class="sxs-lookup"><span data-stu-id="666b6-153">Authorizing by User Name or User Role</span></span>

<span data-ttu-id="666b6-154">[Authorize] 属性を使用すると、ユーザーまたは特定のユーザー ロールのセットの特定のセットをコント ローラー アクションへのアクセスを制限します。</span><span class="sxs-lookup"><span data-stu-id="666b6-154">You can use the [Authorize] attribute to restrict access to a controller action to a particular set of users or a particular set of user roles.</span></span> <span data-ttu-id="666b6-155">たとえば、リスト 2 で修正されたホーム コント ローラーには、StephenSecrets() AdministratorSecrets() という 2 つの新しいアクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="666b6-155">For example, the modified Home controller in Listing 2 contains two new actions named StephenSecrets() and AdministratorSecrets().</span></span>

<span data-ttu-id="666b6-156">**2 – Controllers\HomeController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="666b6-156">**Listing 2 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample2.vb)]

<span data-ttu-id="666b6-157">Stephen ユーザー名を持つユーザーのみが StephenSecrets() アクションを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="666b6-157">Only a user with the user name Stephen can invoke the StephenSecrets() action.</span></span> <span data-ttu-id="666b6-158">その他のすべてのユーザー ログイン ビューにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="666b6-158">All other users get redirected to the Login view.</span></span> <span data-ttu-id="666b6-159">ユーザー プロパティは、ユーザー アカウント名のコンマ区切りリストを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="666b6-159">The Users property accepts a comma separated list of user account names.</span></span>

<span data-ttu-id="666b6-160">管理者ロールのユーザーのみが AdministratorSecrets() アクションを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="666b6-160">Only users in the Administrators role can invoke the AdministratorSecrets() action.</span></span> <span data-ttu-id="666b6-161">たとえば、Sally は、Administrators グループのメンバーであるため彼女 AdministratorSecrets() アクションに呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="666b6-161">For example, because Sally is a member of the Administrators group, she can invoke the AdministratorSecrets() action.</span></span> <span data-ttu-id="666b6-162">その他のすべてのユーザー ログイン ビューにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="666b6-162">All other users get redirected to the Login view.</span></span> <span data-ttu-id="666b6-163">ロールのプロパティは、ロール名のコンマ区切りの一覧を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="666b6-163">The Roles property accepts a comma separated list of role names.</span></span>

#### <a name="configuring-authentication"></a><span data-ttu-id="666b6-164">認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="666b6-164">Configuring Authentication</span></span>

<span data-ttu-id="666b6-165">この時点では、かもしれません。 ユーザー アカウントとロールの情報が格納されています。</span><span class="sxs-lookup"><span data-stu-id="666b6-165">At this point, you might be wondering where the user account and role information is being stored.</span></span> <span data-ttu-id="666b6-166">MVC アプリケーションのアプリ内にある名前付き ASPNETDB.mdf (RANU) SQL Express データベースに既定では、情報が格納されている\_データ フォルダー。</span><span class="sxs-lookup"><span data-stu-id="666b6-166">By default, the information is stored in a (RANU) SQL Express database named ASPNETDB.mdf located in your MVC application's App\_Data folder.</span></span> <span data-ttu-id="666b6-167">メンバーシップの使用を開始すると、このデータベースは、ASP.NET フレームワークによって自動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="666b6-167">This database is generated by the ASP.NET framework automatically when you start using membership.</span></span>

<span data-ttu-id="666b6-168">ソリューション エクスプ ローラー ウィンドウにある ASPNETDB.mdf データベースを確認するためには、まず、すべてのファイル メニュー オプション、プロジェクトを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="666b6-168">In order to see the ASPNETDB.mdf database in the Solution Explorer window, you first need to select the menu option Project, Show All Files.</span></span>

<span data-ttu-id="666b6-169">アプリケーションを開発する際に、この既定の SQL Express データベースを使用することは問題ありません。</span><span class="sxs-lookup"><span data-stu-id="666b6-169">Using the default SQL Express database is fine when developing an application.</span></span> <span data-ttu-id="666b6-170">ほとんどの場合、ただし、しませんする実稼働アプリケーションの既定値にある ASPNETDB.mdf データベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="666b6-170">Most likely, however, you won't want to use the default ASPNETDB.mdf database for a production application.</span></span> <span data-ttu-id="666b6-171">その場合は、次の 2 つの手順を実行してユーザー アカウント情報を格納する場所を変更できます。</span><span class="sxs-lookup"><span data-stu-id="666b6-171">In that case, you can change where user account information is stored by completing the following two steps:</span></span>

1. <span data-ttu-id="666b6-172">アプリケーション サービスのデータベース オブジェクトを実稼働データベースに追加する - アプリケーションの運用データベースを指す接続文字列の変更</span><span class="sxs-lookup"><span data-stu-id="666b6-172">Add the Application Services database objects to your production database - Change your application connection string to point to your production database</span></span>

<span data-ttu-id="666b6-173">最初の手順は、実稼働データベースに、すべての必要なデータベース オブジェクト (テーブルとストアド プロシージャ) を追加します。</span><span class="sxs-lookup"><span data-stu-id="666b6-173">The first step is to add all of the necessary database objects (tables and stored procedures) to your production database.</span></span> <span data-ttu-id="666b6-174">新しいデータベースにこれらのオブジェクトを追加する最も簡単な方法が ASP.NET SQL Server セットアップ ウィザードを活用するためには (図 8 参照)。</span><span class="sxs-lookup"><span data-stu-id="666b6-174">The easiest way to add these objects to a new database is to take advantage of the ASP.NET SQL Server Setup Wizard (see Figure 8).</span></span> <span data-ttu-id="666b6-175">このツールを起動するには、Microsoft Visual Studio 2008 プログラム グループから、Visual Studio 2008 コマンド プロンプトを開き、コマンド プロンプトから次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="666b6-175">You can launch this tool by opening the Visual Studio 2008 Command Prompt from the Microsoft Visual Studio 2008 program group and executing the following command from the command prompt:</span></span>

<span data-ttu-id="666b6-176">aspnet\_regsql</span><span class="sxs-lookup"><span data-stu-id="666b6-176">aspnet\_regsql</span></span>

<span data-ttu-id="666b6-177">**8-ASP.NET SQL Server のセットアップ ウィザードを図します。**</span><span class="sxs-lookup"><span data-stu-id="666b6-177">**Figure 8 – The ASP.NET SQL Server Setup Wizard**</span></span>

![clip_image016](authenticating-users-with-forms-authentication-vb/_static/image8.jpg)

<span data-ttu-id="666b6-179">ASP.NET SQL Server セットアップ ウィザードを使用すると、ネットワーク上の SQL Server データベースを選択し、すべての ASP.NET アプリケーション サービスが必要なデータベース オブジェクトをインストールできます。</span><span class="sxs-lookup"><span data-stu-id="666b6-179">The ASP.NET SQL Server Setup Wizard enables you to select a SQL Server database on your network and install all of the database objects required by the ASP.NET application services.</span></span> <span data-ttu-id="666b6-180">データベース サーバーは、ローカル コンピューター上にある必要はありません。</span><span class="sxs-lookup"><span data-stu-id="666b6-180">The database server is not required to be located on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="666b6-181">ASP.NET SQL Server セットアップ ウィザードを使用しない場合は、次のフォルダーで、アプリケーション サービスのデータベース オブジェクトを追加するための SQL スクリプトを見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="666b6-181">If you don't want to use the ASP.NET SQL Server Setup Wizard, then you can find SQL scripts for adding the application services database objects in the following folder:</span></span>
> 
> 
> <span data-ttu-id="666b6-182">C:\Windows\Microsoft.NET\Framework\v2.0.50727</span><span class="sxs-lookup"><span data-stu-id="666b6-182">C:\Windows\Microsoft.NET\Framework\v2.0.50727</span></span>


<span data-ttu-id="666b6-183">必要なデータベース オブジェクトを作成した後は、MVC アプリケーションで使用されるデータベース接続を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="666b6-183">After you create the necessary database objects, you need to modify the database connection used by your MVC application.</span></span> <span data-ttu-id="666b6-184">Web 構成 (web.config) ファイルで、ApplicationServices 接続文字列を変更するは、実稼働データベースを指すようにします。</span><span class="sxs-lookup"><span data-stu-id="666b6-184">Modify the ApplicationServices connection string in your web configuration (web.config) file so that it points to the production database.</span></span> <span data-ttu-id="666b6-185">たとえば、リスト 3 の変更後の接続は MyProductionDB (元の ApplicationServices 接続文字列コメント アウトされている) という名前のデータベースを指します。</span><span class="sxs-lookup"><span data-stu-id="666b6-185">For example, the modified connection in Listing 3 points to a database named MyProductionDB (the original ApplicationServices connection string has been commented out).</span></span>

<span data-ttu-id="666b6-186">**3-Web.config を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="666b6-186">**Listing 3 – Web.config**</span></span>

[!code-xml[Main](authenticating-users-with-forms-authentication-vb/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a><span data-ttu-id="666b6-187">データベースのアクセス許可の構成</span><span class="sxs-lookup"><span data-stu-id="666b6-187">Configuring Database Permissions</span></span>

<span data-ttu-id="666b6-188">統合セキュリティを使用して、データベースに接続する場合は、データベースにログインとして適切な Windows ユーザー アカウントを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="666b6-188">If you use Integrated Security to connect to your database then you will need to add the correct Windows user account as a login to your database.</span></span> <span data-ttu-id="666b6-189">正しいアカウントは、web サーバーとして、ASP.NET 開発サーバーまたはインターネット インフォメーション サービスを使用するかによって異なります。</span><span class="sxs-lookup"><span data-stu-id="666b6-189">The correct account depends on whether you are using the ASP.NET Development Server or Internet Information Services as your web server.</span></span> <span data-ttu-id="666b6-190">適切なユーザー アカウントは、オペレーティング システムによっても異なります。</span><span class="sxs-lookup"><span data-stu-id="666b6-190">The correct user account also depends on your operating system.</span></span>

<span data-ttu-id="666b6-191">ASP.NET 開発サーバー (既定 web サーバー Visual Studio で使用される) を使用している場合、アプリケーションは、Windows ユーザー アカウントのコンテキスト内で実行します。</span><span class="sxs-lookup"><span data-stu-id="666b6-191">If you are using the ASP.NET Development Server (the default web server used by Visual Studio) then your application executes within the context of your Windows user account.</span></span> <span data-ttu-id="666b6-192">その場合は、データベース サーバーのログインとして、Windows ユーザー アカウントを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="666b6-192">In that case, you need to add your Windows user account as a database server login.</span></span>

<span data-ttu-id="666b6-193">または、インターネット インフォメーション サービスを使用している場合する必要があります、ASPNET アカウントまたは NT AUTHORITY/NETWORK SERVICE アカウントをデータベース サーバーのログインとして追加します。</span><span class="sxs-lookup"><span data-stu-id="666b6-193">Alternatively, if you are using Internet Information Services then you need to add either the ASPNET account or the NT AUTHORITY/NETWORK SERVICE account as a database server login.</span></span> <span data-ttu-id="666b6-194">Windows XP を使用している場合は、ASPNET アカウントとしてログインをデータベースに追加します。</span><span class="sxs-lookup"><span data-stu-id="666b6-194">If you are using Windows XP then add the ASPNET account as a login to your database.</span></span> <span data-ttu-id="666b6-195">Windows Vista または Windows Server 2008 より新しいオペレーティング システムを使用している場合は、データベースのログインとして NT AUTHORITY/NETWORK SERVICE アカウントを追加します。</span><span class="sxs-lookup"><span data-stu-id="666b6-195">If you are using a more recent operating system, such as Windows Vista or Windows Server 2008, then add the NT AUTHORITY/NETWORK SERVICE account as the database login.</span></span>

<span data-ttu-id="666b6-196">Microsoft SQL Server Management Studio を使用して、データベースに新しいユーザー アカウントを追加することができます (図 9 参照)。</span><span class="sxs-lookup"><span data-stu-id="666b6-196">You can add a new user account to your database by using Microsoft SQL Server Management Studio (see Figure 9).</span></span>

<span data-ttu-id="666b6-197">**図 9: 新しい Microsoft SQL Server ログインを作成します。**</span><span class="sxs-lookup"><span data-stu-id="666b6-197">**Figure 9 – Creating a new Microsoft SQL Server login**</span></span>

![clip_image018](authenticating-users-with-forms-authentication-vb/_static/image9.jpg)

<span data-ttu-id="666b6-199">必要なログインを作成した後は、適切なデータベースの役割を持つデータベース ユーザー ログインをマップする必要があります。</span><span class="sxs-lookup"><span data-stu-id="666b6-199">After you create the required login, you need to map the login to a database user with the right database roles.</span></span> <span data-ttu-id="666b6-200">ログインをダブルクリックし、ユーザー マッピング タブを選択します。1 つまたは複数のアプリケーション サービス データベース ロールを選択します。</span><span class="sxs-lookup"><span data-stu-id="666b6-200">Double-click the login and select the User Mapping tab. Select one or more application services database roles.</span></span> <span data-ttu-id="666b6-201">たとえば、aspnet を有効にする必要するユーザーを認証するために\_メンバーシップ\_BasicAccess データベース ロール。</span><span class="sxs-lookup"><span data-stu-id="666b6-201">For example, in order to authenticate users, you need to enable the aspnet\_Membership\_BasicAccess database role.</span></span> <span data-ttu-id="666b6-202">新しいユーザーを作成するには、aspnet を有効にする必要があります。\_メンバーシップ\_FullAccess データベース ロール (図 10 参照)。</span><span class="sxs-lookup"><span data-stu-id="666b6-202">In order to create new users, you need to enable the aspnet\_Membership\_FullAccess database role (see Figure 10).</span></span>

<span data-ttu-id="666b6-203">**図 10-アプリケーション サービス データベース ロールの追加**</span><span class="sxs-lookup"><span data-stu-id="666b6-203">**Figure 10 – Adding Application Services database roles**</span></span>

![clip_image020](authenticating-users-with-forms-authentication-vb/_static/image10.jpg)

#### <a name="summary"></a><span data-ttu-id="666b6-205">まとめ</span><span class="sxs-lookup"><span data-stu-id="666b6-205">Summary</span></span>

<span data-ttu-id="666b6-206">このチュートリアルでは、ASP.NET MVC アプリケーションを構築するときに、フォーム認証を使用する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="666b6-206">In this tutorial, you learned how to use Forms authentication when building an ASP.NET MVC application.</span></span> <span data-ttu-id="666b6-207">最初に、Web サイトの管理ツールを活用して、新しいユーザーとロールを作成する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="666b6-207">First, you learned how to create new users and roles by taking advantage of the Web Site Administration Tool.</span></span> <span data-ttu-id="666b6-208">次に、[Authorize] 属性を使用して、不正なユーザーがコント ローラー アクションを呼び出すことを防止する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="666b6-208">Next, you learned how to use the [Authorize] attribute to prevent unauthorized users from invoking controller actions.</span></span> <span data-ttu-id="666b6-209">最後に、実稼働データベースでユーザーとロール情報を格納する MVC アプリケーションを構成する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="666b6-209">Finally, you learned how to configure your MVC application to store user and role information in a production database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="666b6-210">[前へ](preventing-javascript-injection-attacks-cs.md)
> [次へ](authenticating-users-with-windows-authentication-vb.md)</span><span class="sxs-lookup"><span data-stu-id="666b6-210">[Previous](preventing-javascript-injection-attacks-cs.md)
[Next](authenticating-users-with-windows-authentication-vb.md)</span></span>
