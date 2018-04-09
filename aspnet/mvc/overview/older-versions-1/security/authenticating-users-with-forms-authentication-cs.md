---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
title: フォーム認証 (c#) を持つユーザーを認証 |Microsoft ドキュメント
author: microsoft
description: '[Authorize] 属性を使用する方法について、MVC アプリケーションで特定のページで保護するパスワード。 Web サイトの管理にも使用する方法を学習するとしています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 239fd3ca-5630-4b8d-bc4b-2f906b1d3504
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: e1def84bbf48847339e89b239b026d053640b935
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="authenticating-users-with-forms-authentication-c"></a><span data-ttu-id="c31ab-104">フォーム認証 (c#) でユーザーの認証</span><span class="sxs-lookup"><span data-stu-id="c31ab-104">Authenticating Users with Forms Authentication (C#)</span></span>
====================
<span data-ttu-id="c31ab-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c31ab-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c31ab-106">[Authorize] 属性を使用する方法について、MVC アプリケーションで特定のページで保護するパスワード。</span><span class="sxs-lookup"><span data-stu-id="c31ab-106">Learn how to use the [Authorize] attribute to password protect particular pages in your MVC application.</span></span> <span data-ttu-id="c31ab-107">Web サイト管理ツールを使用して作成し、ユーザーおよびロールを管理する方法について学びます。</span><span class="sxs-lookup"><span data-stu-id="c31ab-107">You learn how to use the Web Site Administration Tool to create and manage users and roles.</span></span> <span data-ttu-id="c31ab-108">ユーザー アカウントとロール情報を格納する場所を構成する方法についても説明します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-108">You also learn how to configure where user account and role information is stored.</span></span>


<span data-ttu-id="c31ab-109">このチュートリアルの目的は、フォームを使用する方法を説明する認証のパスワードを ASP.NET MVC アプリケーション内のビューを保護します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-109">The goal of this tutorial is to explain how you can use Forms authentication to password protect the views in your ASP.NET MVC applications.</span></span> <span data-ttu-id="c31ab-110">Web サイト管理ツールを使用して、ユーザーおよびロールを作成する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="c31ab-110">You learn how to use the Web Site Administration Tool to create users and roles.</span></span> <span data-ttu-id="c31ab-111">承認されていないユーザーがコント ローラーのアクションを呼び出すことを防止する方法についても説明します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-111">You also learn how to prevent unauthorized users from invoking controller actions.</span></span> <span data-ttu-id="c31ab-112">最後に、ユーザー名とパスワードの格納場所を構成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-112">Finally, you learn how to configure where user names and passwords are stored.</span></span>

#### <a name="using-the-web-site-administration-tool"></a><span data-ttu-id="c31ab-113">Web サイト管理ツールを使用します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-113">Using the Web Site Administration Tool</span></span>

<span data-ttu-id="c31ab-114">何でも前に一部のユーザーとロールを作成することで開始する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c31ab-114">Before we do anything else, we should start by creating some users and roles.</span></span> <span data-ttu-id="c31ab-115">新しいユーザーとロールを作成する最も簡単な方法は、Visual Studio 2008 Web サイト管理ツールを活用するためにです。</span><span class="sxs-lookup"><span data-stu-id="c31ab-115">The easiest way to create new users and roles is to take advantage of the Visual Studio 2008 Web Site Administration Tool.</span></span> <span data-ttu-id="c31ab-116">このツールを起動するには、メニュー オプションを選択して**プロジェクトで、ASP.NET 構成**です。</span><span class="sxs-lookup"><span data-stu-id="c31ab-116">You can launch this tool by selecting the menu option **Project, ASP.NET Configuration**.</span></span> <span data-ttu-id="c31ab-117">または、ソリューション エクスプ ローラー ウィンドウの上部に表示される、世界のヒット ハンマーの (ある程度恐ろしい) アイコンをクリックして、Web サイト管理ツールを起動できます (図 1 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="c31ab-117">Alternatively, you can launch the Web Site Administration Tool by clicking the (somewhat scary) icon of the hammer hitting the world that appears at the top of the Solution Explorer window (see Figure 1).</span></span>

<span data-ttu-id="c31ab-118">**図 1 – Web サイト管理ツールを起動します。**</span><span class="sxs-lookup"><span data-stu-id="c31ab-118">**Figure 1 – Launching the Web Site Administration Tool**</span></span>

![clip_image002](authenticating-users-with-forms-authentication-cs/_static/image1.jpg)

<span data-ttu-id="c31ab-120">Web サイト管理ツール内で、[セキュリティ] タブを選択して、新しいユーザーとロールを作成します。クリックして、 **Create user** Stephen をという名前の新しいユーザーを作成するリンク (図 2 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="c31ab-120">Within the Web Site Administration Tool, you create new users and roles by selecting the Security tab. Click the **Create user** link to create a new user named Stephen (see Figure 2).</span></span> <span data-ttu-id="c31ab-121">Stephen ユーザー パスワードを入力、使用する (たとえば、*シークレット*)。</span><span class="sxs-lookup"><span data-stu-id="c31ab-121">Provide the Stephen user with any password that you want (for example, *secret*).</span></span>

<span data-ttu-id="c31ab-122">**図 2 – 新しいユーザーを作成します。**</span><span class="sxs-lookup"><span data-stu-id="c31ab-122">**Figure 2 – Creating a new user**</span></span>

![clip_image004](authenticating-users-with-forms-authentication-cs/_static/image2.jpg)

<span data-ttu-id="c31ab-124">新しいロールを作成するには、最初の役割を有効にして 1 つまたは複数のロールを定義します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-124">You create new roles by first enabling roles and defining one or more roles.</span></span> <span data-ttu-id="c31ab-125">クリックしてロールを有効にする、**ロールを有効にする**リンクします。</span><span class="sxs-lookup"><span data-stu-id="c31ab-125">Enable roles by clicking the **Enable roles** link.</span></span> <span data-ttu-id="c31ab-126">次に、という名前のロールを作成*管理者* をクリックして、**作成または管理ロール**(図 3 を参照してください) をリンクします。</span><span class="sxs-lookup"><span data-stu-id="c31ab-126">Next, create a role named *Administrators* by clicking the **Create or Manage roles** link (see Figure 3).</span></span>

<span data-ttu-id="c31ab-127">**図 3 – 新しいロールを作成します。**</span><span class="sxs-lookup"><span data-stu-id="c31ab-127">**Figure 3 – Creating a new role**</span></span>

![clip_image006](authenticating-users-with-forms-authentication-cs/_static/image3.jpg)

<span data-ttu-id="c31ab-129">最後に、Sally をという名前の新しいユーザーを作成し、ユーザーの作成 リンクをクリックし、Sally を作成するときに管理者を選択すると、管理者ロールに Sally を関連付ける (図 4 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="c31ab-129">Finally, create a new user named Sally and associate Sally with the Administrators role by clicking the Create User link and selecting Administrators when creating Sally (see Figure 4).</span></span>

<span data-ttu-id="c31ab-130">**図 4: ロールにユーザーを追加します。**</span><span class="sxs-lookup"><span data-stu-id="c31ab-130">**Figure 4 – Adding a user to a role**</span></span>

![clip_image008](authenticating-users-with-forms-authentication-cs/_static/image4.jpg)

<span data-ttu-id="c31ab-132">すべてが完了して、ときに、Stephen および Sally という 2 つの新しいユーザーが必要です。</span><span class="sxs-lookup"><span data-stu-id="c31ab-132">When all is said and done, you should have two new users named Stephen and Sally.</span></span> <span data-ttu-id="c31ab-133">管理者をという名前の新しいロールも必要です。</span><span class="sxs-lookup"><span data-stu-id="c31ab-133">You should also have a new role named Administrators.</span></span> <span data-ttu-id="c31ab-134">Sally 管理者ロールのメンバーは、Stephen です。</span><span class="sxs-lookup"><span data-stu-id="c31ab-134">Sally is a member of the Administrators role and Stephen is not.</span></span>

#### <a name="requiring-authorization"></a><span data-ttu-id="c31ab-135">承認を必要とします。</span><span class="sxs-lookup"><span data-stu-id="c31ab-135">Requiring Authorization</span></span>

<span data-ttu-id="c31ab-136">ユーザーが、アクションに [Authorize] 属性を追加することでコント ローラーのアクションを呼び出す前に認証されるユーザーを要求できます。</span><span class="sxs-lookup"><span data-stu-id="c31ab-136">You can require a user to be authenticated before the user invokes a controller action by adding the [Authorize] attribute to the action.</span></span> <span data-ttu-id="c31ab-137">個々 のコント ローラー アクションに [Authorize] 属性を適用することができますか、全体のコント ローラー クラスにこの属性を適用することができます。</span><span class="sxs-lookup"><span data-stu-id="c31ab-137">You can apply the [Authorize] attribute to an individual controller action or you can apply this attribute to an entire controller class.</span></span>

<span data-ttu-id="c31ab-138">たとえば、リスト 1 のコント ローラーは、CompanySecrets() をという名前のアクションを公開します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-138">For example, the controller in Listing 1 exposes an action named CompanySecrets().</span></span> <span data-ttu-id="c31ab-139">このアクションが [Authorize] 属性で装飾されているために、ユーザーが認証される場合を除き、この操作を呼び出すことができません。</span><span class="sxs-lookup"><span data-stu-id="c31ab-139">Because this action is decorated with the [Authorize] attribute, this action cannot be invoked unless a user is authenticated.</span></span>

<span data-ttu-id="c31ab-140">**1 – controllers \homecontroller.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="c31ab-140">**Listing 1 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample1.cs)]

<span data-ttu-id="c31ab-141">かどうかは CompanySecrets() 動作を実行して、ブラウザーのアドレス バーに URL/Home/CompanySecrets を入力して、認証されたユーザーが自動的にログイン ビューにリダイレクトされます (図 5 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="c31ab-141">If you invoke the CompanySecrets() action by entering the URL /Home/CompanySecrets in the address bar of your browser, and you are not an authenticated user, then you will be redirected to the Login view automatically (see Figure 5).</span></span>

<span data-ttu-id="c31ab-142">**図 5 – ログイン ビュー**</span><span class="sxs-lookup"><span data-stu-id="c31ab-142">**Figure 5 – The Login view**</span></span>

![clip_image010](authenticating-users-with-forms-authentication-cs/_static/image5.jpg)

<span data-ttu-id="c31ab-144">ログイン ビューを使用すると、ユーザー名とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-144">You can use the Login view to enter your user name and password.</span></span> <span data-ttu-id="c31ab-145">登録済みユーザーがいないかどうかをクリックして、**登録**レジスタに移動するリンク (図 6 を参照してください) を表示します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-145">If you are not a registered user then you can click the **register** link to navigate to the Register view (see Figure 6).</span></span> <span data-ttu-id="c31ab-146">レジスタ ビューを使用すると、新しいユーザー アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-146">You can use the Register view to create a new user account.</span></span>

<span data-ttu-id="c31ab-147">**図 6 – レジスタ ビュー**</span><span class="sxs-lookup"><span data-stu-id="c31ab-147">**Figure 6 – The Register view**</span></span>

![clip_image012](authenticating-users-with-forms-authentication-cs/_static/image6.jpg)

<span data-ttu-id="c31ab-149">正常にログインした後は、(図 7 を参照してください) を表示 CompanySecrets を表示できます。</span><span class="sxs-lookup"><span data-stu-id="c31ab-149">After you successfully log in, you can see the CompanySecrets view (see Figure 7).</span></span> <span data-ttu-id="c31ab-150">既定では、引き続き、ブラウザー ウィンドウを閉じるまでに記録されます。</span><span class="sxs-lookup"><span data-stu-id="c31ab-150">By default, you will continue to be logged in until you close your browser window.</span></span>

<span data-ttu-id="c31ab-151">**図 7-CompanySecrets ビュー**</span><span class="sxs-lookup"><span data-stu-id="c31ab-151">**Figure 7 – The CompanySecrets view**</span></span>

![clip_image014](authenticating-users-with-forms-authentication-cs/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a><span data-ttu-id="c31ab-153">ユーザー名またはユーザー ロールを承認します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-153">Authorizing by User Name or User Role</span></span>

<span data-ttu-id="c31ab-154">[Authorize] 属性を使用すると、ユーザーまたは特定のユーザー ロールのセットの特定のセットをコント ローラー アクションへのアクセスを制限します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-154">You can use the [Authorize] attribute to restrict access to a controller action to a particular set of users or a particular set of user roles.</span></span> <span data-ttu-id="c31ab-155">たとえば、リスト 2 での変更、Home コント ローラーには、2 つの新しいアクション StephenSecrets() および AdministratorSecrets() という名前が含まれています。</span><span class="sxs-lookup"><span data-stu-id="c31ab-155">For example, the modified Home controller in Listing 2 contains two new actions named StephenSecrets() and AdministratorSecrets().</span></span>

<span data-ttu-id="c31ab-156">**2 – controllers \homecontroller.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="c31ab-156">**Listing 2 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample2.cs)]

<span data-ttu-id="c31ab-157">Stephen ユーザー名を持つユーザーのみが StephenSecrets() アクションを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="c31ab-157">Only a user with the user name Stephen can invoke the StephenSecrets() action.</span></span> <span data-ttu-id="c31ab-158">その他のすべてのユーザー ログイン ビューにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="c31ab-158">All other users get redirected to the Login view.</span></span> <span data-ttu-id="c31ab-159">ユーザーのプロパティは、ユーザー アカウント名のコンマ区切り一覧を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="c31ab-159">The Users property accepts a comma separated list of user account names.</span></span>

<span data-ttu-id="c31ab-160">管理者ロールのユーザーだけでは、AdministratorSecrets() アクションを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="c31ab-160">Only users in the Administrators role can invoke the AdministratorSecrets() action.</span></span> <span data-ttu-id="c31ab-161">たとえば、Sally は Administrators グループのメンバーであるため、彼女に、AdministratorSecrets() アクションを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-161">For example, because Sally is a member of the Administrators group, she can invoke the AdministratorSecrets() action.</span></span> <span data-ttu-id="c31ab-162">その他のすべてのユーザー ログイン ビューにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="c31ab-162">All other users get redirected to the Login view.</span></span> <span data-ttu-id="c31ab-163">ロール プロパティは、ロール名のコンマ区切り一覧を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="c31ab-163">The Roles property accepts a comma separated list of role names.</span></span>

#### <a name="configuring-authentication"></a><span data-ttu-id="c31ab-164">認証を構成します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-164">Configuring Authentication</span></span>

<span data-ttu-id="c31ab-165">この時点では、思うかもしれませんユーザー アカウントとロールの情報が格納されています。</span><span class="sxs-lookup"><span data-stu-id="c31ab-165">At this point, you might be wondering where the user account and role information is being stored.</span></span> <span data-ttu-id="c31ab-166">MVC アプリケーションのアプリ内にある名前付き ASPNETDB.mdf (RANU) SQL Express データベースに既定では、情報が格納されている\_データ フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="c31ab-166">By default, the information is stored in a (RANU) SQL Express database named ASPNETDB.mdf located in your MVC application's App\_Data folder.</span></span> <span data-ttu-id="c31ab-167">メンバーシップの使用を開始するときに、このデータベースは、ASP.NET フレームワークによって自動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="c31ab-167">This database is generated by the ASP.NET framework automatically when you start using membership.</span></span>

<span data-ttu-id="c31ab-168">ソリューション エクスプ ローラー ウィンドウで、ASPNETDB.mdf データベースを表示するのには、まず、すべてのファイル メニュー オプション、プロジェクトを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c31ab-168">In order to see the ASPNETDB.mdf database in the Solution Explorer window, you first need to select the menu option Project, Show All Files.</span></span>

<span data-ttu-id="c31ab-169">アプリケーションを開発するときに、既定の SQL Express データベースを使用しては正常です。</span><span class="sxs-lookup"><span data-stu-id="c31ab-169">Using the default SQL Express database is fine when developing an application.</span></span> <span data-ttu-id="c31ab-170">ほとんどの場合、ただし、されませんする ASPNETDB.mdf は既定のデータベースを使用して、実稼働アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="c31ab-170">Most likely, however, you won't want to use the default ASPNETDB.mdf database for a production application.</span></span> <span data-ttu-id="c31ab-171">その場合は、次の 2 つの手順を完了してユーザー アカウント情報を格納する場所を変更できます。</span><span class="sxs-lookup"><span data-stu-id="c31ab-171">In that case, you can change where user account information is stored by completing the following two steps:</span></span>

1. <span data-ttu-id="c31ab-172">アプリケーション サービスのデータベース オブジェクトを実稼働データベースに追加する -、実稼働データベースを指すようにアプリケーションの接続文字列の変更</span><span class="sxs-lookup"><span data-stu-id="c31ab-172">Add the Application Services database objects to your production database - Change your application connection string to point to your production database</span></span>

<span data-ttu-id="c31ab-173">最初の手順は、すべての必要なデータベース オブジェクト (テーブルとストアド プロシージャ) を追加する、実稼働データベースには。</span><span class="sxs-lookup"><span data-stu-id="c31ab-173">The first step is to add all of the necessary database objects (tables and stored procedures) to your production database.</span></span> <span data-ttu-id="c31ab-174">ASP.NET SQL Server セットアップ ウィザードを活用するためには、新しいデータベースにこれらのオブジェクトを追加する最も簡単な方法 (図 8 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="c31ab-174">The easiest way to add these objects to a new database is to take advantage of the ASP.NET SQL Server Setup Wizard (see Figure 8).</span></span> <span data-ttu-id="c31ab-175">このツールを起動するには、Microsoft Visual Studio 2008 プログラム グループから、Visual Studio 2008 コマンド プロンプトを開き、コマンド プロンプトから次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-175">You can launch this tool by opening the Visual Studio 2008 Command Prompt from the Microsoft Visual Studio 2008 program group and executing the following command from the command prompt:</span></span>

<span data-ttu-id="c31ab-176">aspnet\_regsql</span><span class="sxs-lookup"><span data-stu-id="c31ab-176">aspnet\_regsql</span></span>

<span data-ttu-id="c31ab-177">**図 8: に、ASP.NET SQL Server セットアップ ウィザード**</span><span class="sxs-lookup"><span data-stu-id="c31ab-177">**Figure 8 – The ASP.NET SQL Server Setup Wizard**</span></span>

![clip_image016](authenticating-users-with-forms-authentication-cs/_static/image8.jpg)

<span data-ttu-id="c31ab-179">ASP.NET SQL Server セットアップ ウィザードを使用すると、ネットワーク上の SQL Server データベースを選択し、すべての ASP.NET アプリケーション サービスで必要なデータベース オブジェクトをインストールできます。</span><span class="sxs-lookup"><span data-stu-id="c31ab-179">The ASP.NET SQL Server Setup Wizard enables you to select a SQL Server database on your network and install all of the database objects required by the ASP.NET application services.</span></span> <span data-ttu-id="c31ab-180">データベース サーバーは、ローカル コンピューター上に配置する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="c31ab-180">The database server is not required to be located on your local machine.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="c31ab-181">ASP.NET SQL Server セットアップ ウィザードを使用しない場合は、次のフォルダーに、アプリケーション サービスのデータベース オブジェクトを追加するための SQL スクリプトを見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="c31ab-181">If you don't want to use the ASP.NET SQL Server Setup Wizard, then you can find SQL scripts for adding the application services database objects in the following folder:</span></span>
> 
> > <span data-ttu-id="c31ab-182">C:\Windows\Microsoft.NET\Framework\v2.0.50727</span><span class="sxs-lookup"><span data-stu-id="c31ab-182">C:\Windows\Microsoft.NET\Framework\v2.0.50727</span></span>


<span data-ttu-id="c31ab-183">必要なデータベース オブジェクトを作成した後は、MVC アプリケーションで使用されるデータベース接続を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c31ab-183">After you create the necessary database objects, you need to modify the database connection used by your MVC application.</span></span> <span data-ttu-id="c31ab-184">実稼働データベースを指すように、web の構成 (web.config) ファイルで、ApplicationServices 接続文字列を変更します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-184">Modify the ApplicationServices connection string in your web configuration (web.config) file so that it points to the production database.</span></span> <span data-ttu-id="c31ab-185">たとえば、3 の一覧表示するのには、変更後の接続は、MyProductionDB (元の ApplicationServices 接続文字列コメント アウトされています) という名前のデータベースを指します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-185">For example, the modified connection in Listing 3 points to a database named MyProductionDB (the original ApplicationServices connection string has been commented out).</span></span>

<span data-ttu-id="c31ab-186">**3-Web.config を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="c31ab-186">**Listing 3 – Web.config**</span></span>

[!code-xml[Main](authenticating-users-with-forms-authentication-cs/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a><span data-ttu-id="c31ab-187">データベースのアクセス許可を構成します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-187">Configuring Database Permissions</span></span>

<span data-ttu-id="c31ab-188">データベースへの接続に統合セキュリティを使用する場合は、データベースにログインとして適切な Windows ユーザー アカウントを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c31ab-188">If you use Integrated Security to connect to your database then you will need to add the correct Windows user account as a login to your database.</span></span> <span data-ttu-id="c31ab-189">正しいアカウントは、web サーバーとして、ASP.NET 開発サーバーまたはインターネット インフォメーション サービスを使用するかによって異なります。</span><span class="sxs-lookup"><span data-stu-id="c31ab-189">The correct account depends on whether you are using the ASP.NET Development Server or Internet Information Services as your web server.</span></span> <span data-ttu-id="c31ab-190">正しいユーザー アカウントは、オペレーティング システムによっても異なります。</span><span class="sxs-lookup"><span data-stu-id="c31ab-190">The correct user account also depends on your operating system.</span></span>

<span data-ttu-id="c31ab-191">ASP.NET 開発サーバー (既定の web サーバー Visual Studio で使用される) を使用している場合は、アプリケーションが、Windows ユーザー アカウントのコンテキスト内で実行されます。</span><span class="sxs-lookup"><span data-stu-id="c31ab-191">If you are using the ASP.NET Development Server (the default web server used by Visual Studio) then your application executes within the context of your Windows user account.</span></span> <span data-ttu-id="c31ab-192">その場合は、データベース サーバーのログインとして使用する Windows ユーザー アカウントを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c31ab-192">In that case, you need to add your Windows user account as a database server login.</span></span>

<span data-ttu-id="c31ab-193">または、インターネット インフォメーション サービスを使用している場合、ASPNET アカウントまたは NT AUTHORITY/ネットワーク サービス アカウントをデータベース サーバーのログインとして追加します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-193">Alternatively, if you are using Internet Information Services then you need to add either the ASPNET account or the NT AUTHORITY/NETWORK SERVICE account as a database server login.</span></span> <span data-ttu-id="c31ab-194">Windows XP を使用している場合は、ASPNET アカウントとしてログインをデータベースに追加します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-194">If you are using Windows XP then add the ASPNET account as a login to your database.</span></span> <span data-ttu-id="c31ab-195">Windows Vista または Windows Server 2008 より新しいオペレーティング システムを使用している場合は、データベースのログインとして NT AUTHORITY/ネットワーク サービス アカウントを追加します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-195">If you are using a more recent operating system, such as Windows Vista or Windows Server 2008, then add the NT AUTHORITY/NETWORK SERVICE account as the database login.</span></span>

<span data-ttu-id="c31ab-196">Microsoft SQL Server Management Studio を使用して、データベースに新しいユーザー アカウントを追加することができます (図 9 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="c31ab-196">You can add a new user account to your database by using Microsoft SQL Server Management Studio (see Figure 9).</span></span>

<span data-ttu-id="c31ab-197">**図 9 – 新しい Microsoft SQL Server ログインを作成します。**</span><span class="sxs-lookup"><span data-stu-id="c31ab-197">**Figure 9 – Creating a new Microsoft SQL Server login**</span></span>

![clip_image018](authenticating-users-with-forms-authentication-cs/_static/image9.jpg)

<span data-ttu-id="c31ab-199">必要なログインを作成した後は、適切なデータベース ロールを持つデータベース ユーザーにログインをマップする必要があります。</span><span class="sxs-lookup"><span data-stu-id="c31ab-199">After you create the required login, you need to map the login to a database user with the right database roles.</span></span> <span data-ttu-id="c31ab-200">ログインをダブルクリックし、[ユーザー マッピング] タブを選択します。1 つまたは複数のアプリケーション サービス データベース ロールを選択します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-200">Double-click the login and select the User Mapping tab. Select one or more application services database roles.</span></span> <span data-ttu-id="c31ab-201">たとえば、ユーザーを認証するために有効にする必要は aspnet\_メンバーシップ\_BasicAccess データベース ロール。</span><span class="sxs-lookup"><span data-stu-id="c31ab-201">For example, in order to authenticate users, you need to enable the aspnet\_Membership\_BasicAccess database role.</span></span> <span data-ttu-id="c31ab-202">新しいユーザーを作成するために、aspnet を有効にする必要があります。\_メンバーシップ\_FullAccess データベース ロール (図 10 参照)。</span><span class="sxs-lookup"><span data-stu-id="c31ab-202">In order to create new users, you need to enable the aspnet\_Membership\_FullAccess database role (see Figure 10).</span></span>

<span data-ttu-id="c31ab-203">**図 10-アプリケーション サービス データベース ロールの追加**</span><span class="sxs-lookup"><span data-stu-id="c31ab-203">**Figure 10 – Adding Application Services database roles**</span></span>

![clip_image020](authenticating-users-with-forms-authentication-cs/_static/image10.jpg)

#### <a name="summary"></a><span data-ttu-id="c31ab-205">まとめ</span><span class="sxs-lookup"><span data-stu-id="c31ab-205">Summary</span></span>

<span data-ttu-id="c31ab-206">このチュートリアルでは、ASP.NET MVC アプリケーションをビルドする際に、フォーム認証を使用する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="c31ab-206">In this tutorial, you learned how to use Forms authentication when building an ASP.NET MVC application.</span></span> <span data-ttu-id="c31ab-207">最初に、Web サイト管理ツールを活用して、新しいユーザーとロールを作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-207">First, you learned how to create new users and roles by taking advantage of the Web Site Administration Tool.</span></span> <span data-ttu-id="c31ab-208">次に、[Authorize] 属性を使用して、不正なユーザーがコント ローラーのアクションを呼び出すことを防止する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="c31ab-208">Next, you learned how to use the [Authorize] attribute to prevent unauthorized users from invoking controller actions.</span></span> <span data-ttu-id="c31ab-209">最後に、実稼働データベースでユーザーとロール情報を格納する MVC アプリケーションを構成する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="c31ab-209">Finally, you learned how to configure your MVC application to store user and role information in a production database.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c31ab-210">次へ</span><span class="sxs-lookup"><span data-stu-id="c31ab-210">Next</span></span>](authenticating-users-with-windows-authentication-cs.md)
