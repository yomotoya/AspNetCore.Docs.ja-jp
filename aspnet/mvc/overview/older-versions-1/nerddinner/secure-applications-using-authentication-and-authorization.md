---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: "認証と承認を使用してアプリケーションをセキュリティで保護された |Microsoft ドキュメント"
author: microsoft
description: "手順 9 ができるように、ユーザーが登録する必要があります、認証と承認を NerdDinner アプリケーションでは、セキュリティで保護を追加する方法を示しますおよび作成するには、サイトにログインしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: a23b2cf4d1728624698c0db49c25ea7efd3af67d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="secure-applications-using-authentication-and-authorization"></a><span data-ttu-id="444d9-103">認証と承認を使用してアプリケーションをセキュリティ保護します。</span><span class="sxs-lookup"><span data-stu-id="444d9-103">Secure Applications Using Authentication and Authorization</span></span>
====================
<span data-ttu-id="444d9-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="444d9-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="444d9-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="444d9-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="444d9-106">これは、無料の手順 9 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="444d9-106">This is step 9 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="444d9-107">手順 9 ができるように、ユーザーが登録する必要があります、認証と承認を NerdDinner アプリケーションでは、セキュリティで保護を追加する方法を示します新しいディナー、および、夕食をホストしているユーザーのみを作成するサイトにログインが後で編集できます。</span><span class="sxs-lookup"><span data-stu-id="444d9-107">Step 9 shows how to add authentication and authorization to secure our NerdDinner application, so that users need to register and login to the site to create new dinners, and only the user who is hosting a dinner can edit it later.</span></span>
> 
> <span data-ttu-id="444d9-108">ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="444d9-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-9-authentication-and-authorization"></a><span data-ttu-id="444d9-109">NerdDinner 手順 9: 認証と承認</span><span class="sxs-lookup"><span data-stu-id="444d9-109">NerdDinner Step 9: Authentication and Authorization</span></span>

<span data-ttu-id="444d9-110">今すぐサイトを作成し、夕食の詳細を編集する機能にアクセスする、NerdDinner がアプリケーションでは、すべてのユーザーを許可します。</span><span class="sxs-lookup"><span data-stu-id="444d9-110">Right now our NerdDinner application grants anyone visiting the site the ability to create and edit the details of any dinner.</span></span> <span data-ttu-id="444d9-111">ユーザーが登録する必要があるようにこれを変更してみましょうと新しいディナーを作成し、夕食をホストしているユーザーのみが後で編集できるように制限を追加するサイトにログインします。</span><span class="sxs-lookup"><span data-stu-id="444d9-111">Let's change this so that users need to register and login to the site to create new dinners, and add a restriction so that only the user who is hosting a dinner can edit it later.</span></span>

<span data-ttu-id="444d9-112">これを有効にするには、アプリケーションをセキュリティで保護するのに認証と承認を使用します。</span><span class="sxs-lookup"><span data-stu-id="444d9-112">To enable this we'll use authentication and authorization to secure our application.</span></span>

### <a name="understanding-authentication-and-authorization"></a><span data-ttu-id="444d9-113">Understanding 認証と承認</span><span class="sxs-lookup"><span data-stu-id="444d9-113">Understanding Authentication and Authorization</span></span>

<span data-ttu-id="444d9-114">*認証*を特定し、アプリケーションにアクセスするクライアントの id を検証するプロセスです。</span><span class="sxs-lookup"><span data-stu-id="444d9-114">*Authentication* is the process of identifying and validating the identity of a client accessing an application.</span></span> <span data-ttu-id="444d9-115">さらに簡単に言うと、「ユーザー」、エンドユーザーは、web サイトを訪問するときに識別することです。</span><span class="sxs-lookup"><span data-stu-id="444d9-115">Put more simply, it is about identifying "who" the end-user is when they visit a website.</span></span> <span data-ttu-id="444d9-116">ASP.NET には、ブラウザーのユーザーを認証するいくつかの方法がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="444d9-116">ASP.NET supports multiple ways to authenticate browser users.</span></span> <span data-ttu-id="444d9-117">インターネットの web アプリケーションでは、使用される最も一般的な認証方法を [フォーム認証] と呼びます。</span><span class="sxs-lookup"><span data-stu-id="444d9-117">For Internet web applications, the most common authentication approach used is called "Forms Authentication".</span></span> <span data-ttu-id="444d9-118">フォーム認証を使用すると、開発者は、アプリケーション内での HTML ログイン フォームを作成し、データベースやその他のパスワードの資格情報ストアに対して、エンドユーザーが送信したユーザー名とパスワードを検証します。</span><span class="sxs-lookup"><span data-stu-id="444d9-118">Forms Authentication enables a developer to author an HTML login form within their application, and then validate the username/password an end-user submits against a database or other password credential store.</span></span> <span data-ttu-id="444d9-119">ユーザー名/パスワードの組み合わせが正しい場合は、開発者は、今後の要求間でユーザーを識別する暗号化された HTTP cookie を発行する ASP.NET を依頼できます。</span><span class="sxs-lookup"><span data-stu-id="444d9-119">If the username/password combination is correct, the developer can then ask ASP.NET to issue an encrypted HTTP cookie to identify the user across future requests.</span></span> <span data-ttu-id="444d9-120">NerdDinner するアプリケーションでフォーム認証を使用して、します。</span><span class="sxs-lookup"><span data-stu-id="444d9-120">We'll by using forms authentication with our NerdDinner application.</span></span>

<span data-ttu-id="444d9-121">*承認*は、認証されたユーザーが特定の URL/リソースへのアクセスまたはいくつかの操作を実行するアクセス許可を持つかどうかを決定するプロセスです。</span><span class="sxs-lookup"><span data-stu-id="444d9-121">*Authorization* is the process of determining whether an authenticated user has permission to access a particular URL/resource or to perform some action.</span></span> <span data-ttu-id="444d9-122">たとえば、NerdDinner アプリケーション内でおしますでログオンしているユーザーのみがアクセスできることを承認するために、*ディナー/作成*URL し、新しいディナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="444d9-122">For example, within our NerdDinner application we'll want to authorize that only users who are logged in can access the */Dinners/Create* URL and create new Dinners.</span></span> <span data-ttu-id="444d9-123">承認ロジックを追加、夕食をホストしているユーザーだけは – を編集したり、他のすべてのユーザーの編集アクセス権を拒否するようにすることもあります。</span><span class="sxs-lookup"><span data-stu-id="444d9-123">We'll also want to add authorization logic so that only the user who is hosting a dinner can edit it – and deny edit access to all other users.</span></span>

### <a name="forms-authentication-and-the-accountcontroller"></a><span data-ttu-id="444d9-124">フォーム認証と、AccountController</span><span class="sxs-lookup"><span data-stu-id="444d9-124">Forms Authentication and the AccountController</span></span>

<span data-ttu-id="444d9-125">ASP.NET MVC の既定の Visual Studio プロジェクト テンプレートは、新しい ASP.NET MVC アプリケーションを作成すると、フォーム認証を自動的に有効にします。</span><span class="sxs-lookup"><span data-stu-id="444d9-125">The default Visual Studio project template for ASP.NET MVC automatically enables forms authentication when new ASP.NET MVC applications are created.</span></span> <span data-ttu-id="444d9-126">自動的に、プロジェクト: を使用して、サイト内のセキュリティを統合する非常に簡単に構築済みのアカウントのログイン ページの実装を追加します。</span><span class="sxs-lookup"><span data-stu-id="444d9-126">It also automatically adds a pre-built account login page implementation to the project – which makes it really easy to integrate security within a site.</span></span>

<span data-ttu-id="444d9-127">既定の Site.master マスター ページにアクセスするユーザーが認証されていないときに、サイトの上部右にある [ログオン] のリンクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="444d9-127">The default Site.master master page displays a "Log On" link at the top-right of the site when the user accessing it is not authenticated:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

<span data-ttu-id="444d9-128">[ログオン] リンクをクリックすると、ユーザーを*/アカウント/ログオン*URL:</span><span class="sxs-lookup"><span data-stu-id="444d9-128">Clicking the "Log On" link takes a user to the */Account/LogOn* URL:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

<span data-ttu-id="444d9-129">訪問者を登録してこれを行う – しに「登録」リンクをクリックして、 */アカウント/レジスタ*URL しできるようにすると、アカウントの詳細を入力してください。</span><span class="sxs-lookup"><span data-stu-id="444d9-129">Visitors who haven't registered can do so by clicking the "Register" link – which will take them to the */Account/Register* URL and allow them to enter account details:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

<span data-ttu-id="444d9-130">「登録」ボタンをクリックし、ASP.NET メンバーシップ システム内で新しいユーザーの作成は、フォーム認証を使用して、サイト上にユーザーを認証します。</span><span class="sxs-lookup"><span data-stu-id="444d9-130">Clicking the "Register" button will create a new user within the ASP.NET Membership system, and authenticate the user onto the site using forms authentication.</span></span>

<span data-ttu-id="444d9-131">Site.master 変更、「ようこそ [username]!」を出力するページの右上でユーザーがログインしている場合、</span><span class="sxs-lookup"><span data-stu-id="444d9-131">When a user is logged-in, the Site.master changes the top-right of the page to output a "Welcome [username]!"</span></span> <span data-ttu-id="444d9-132">メッセージおよびレンダリング、"ログ Off"、"ログ オン"のいずれかの代わりにリンクします。</span><span class="sxs-lookup"><span data-stu-id="444d9-132">message and renders a "Log Off" link instead of a "Log On" one.</span></span> <span data-ttu-id="444d9-133">ユーザーをログアウト「ログオフ」リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="444d9-133">Clicking the "Log Off" link logs out the user:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

<span data-ttu-id="444d9-134">上記のログイン、ログアウト ページ、および登録の機能は、プロジェクト、作成時に Visual Studio によってプロジェクトに追加された AccountController クラス内で実装されます。</span><span class="sxs-lookup"><span data-stu-id="444d9-134">The above login, logout, and registration functionality is implemented within the AccountController class that was added to our project by Visual Studio when it created the project.</span></span> <span data-ttu-id="444d9-135">\Views\Account ディレクトリ内でテンプレートの表示を使用して、AccountController の UI が実装されます。</span><span class="sxs-lookup"><span data-stu-id="444d9-135">The UI for the AccountController is implemented using view templates within the \Views\Account directory:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

<span data-ttu-id="444d9-136">AccountController クラスでは、ASP.NET フォーム認証システムを使用して、暗号化された認証 cookie、およびを保存し、ユーザー名とパスワードを検証する ASP.NET メンバーシップ API を発行します。</span><span class="sxs-lookup"><span data-stu-id="444d9-136">The AccountController class uses the ASP.NET Forms Authentication system to issue encrypted authentication cookies, and the ASP.NET Membership API to store and validate usernames/passwords.</span></span> <span data-ttu-id="444d9-137">ASP.NET メンバーシップと、API は拡張可能使用するパスワード、資格情報ストアを有効にします。</span><span class="sxs-lookup"><span data-stu-id="444d9-137">The ASP.NET Membership API is extensible and enables any password credential store to be used.</span></span> <span data-ttu-id="444d9-138">ASP.NET は、SQL データベース内、または Active Directory 内のユーザー名とパスワードを格納する組み込みのメンバーシップ プロバイダーの実装に付属します。</span><span class="sxs-lookup"><span data-stu-id="444d9-138">ASP.NET ships with built-in membership provider implementations that store username/passwords within a SQL database, or within Active Directory.</span></span>

<span data-ttu-id="444d9-139">NerdDinner アプリケーションは、プロジェクトのルートにある"web.config"ファイルを開き、検索で使用するメンバーシップ プロバイダーを構成して、&lt;メンバーシップ&gt;セクション内にします。</span><span class="sxs-lookup"><span data-stu-id="444d9-139">We can configure which membership provider our NerdDinner application should use by opening the "web.config" file at the root of the project and looking for the &lt;membership&gt; section within it.</span></span> <span data-ttu-id="444d9-140">プロジェクトの作成時に追加された既定の web.config は SQL メンバーシップ プロバイダーを登録し、"ApplicationServices"を名前付き接続文字列を使用するように構成データベースの場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="444d9-140">The default web.config added when the project was created registers the SQL membership provider, and configures it to use a connection-string named "ApplicationServices" to specify the database location.</span></span>

<span data-ttu-id="444d9-141">既定の"ApplicationServices"接続文字列 (内に指定されている、 &lt;connectionStrings&gt; web.config ファイルのセクション) SQL Express を使用するように構成します。</span><span class="sxs-lookup"><span data-stu-id="444d9-141">The default "ApplicationServices" connection string (which is specified within the &lt;connectionStrings&gt; section of the web.config file) is configured to use SQL Express.</span></span> <span data-ttu-id="444d9-142">"ASPNETDB をという名前の SQL Express データベースを指しています。アプリケーションの下にある"MDF"アプリ\_データ"のディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="444d9-142">It points to a SQL Express database named "ASPNETDB.MDF" under the application's "App\_Data" directory.</span></span> <span data-ttu-id="444d9-143">このデータベースは、最初に、アプリケーション内で、メンバーシップ API を使用するときに存在しない、ASP.NET のデータベースの作成は自動的に作成し、適切なメンバーシップ データベース スキーマ内のプロビジョニングします。</span><span class="sxs-lookup"><span data-stu-id="444d9-143">If this database doesn't exist the first time the Membership API is used within the application, ASP.NET will automatically create the database and provision the appropriate membership database schema within it:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

<span data-ttu-id="444d9-144">はなく SQL Express (またはリモート データベースへの接続)、完全な SQL Server インスタンスを使用する場合、すべてが必要になりますタスクは、web.config ファイル内の"ApplicationServices"接続文字列を更新し、ことを確認して、適切なメンバシップ スキーマポイントのデータベースに追加されました。</span><span class="sxs-lookup"><span data-stu-id="444d9-144">If instead of using SQL Express we wanted to use a full SQL Server instance (or connect to a remote database), all we'd need to-do is to update the "ApplicationServices" connection string within the web.config file and make sure that the appropriate membership schema has been added to the database it points at.</span></span> <span data-ttu-id="444d9-145">実行することができます、"aspnet\_regsql.exe"メンバーシップと、その他の ASP.NET アプリケーション サービスの適切なスキーマをデータベースに追加する \Windows\Microsoft.NET\Framework\v2.0.50727\ ディレクトリ内でユーティリティです。</span><span class="sxs-lookup"><span data-stu-id="444d9-145">You can run the "aspnet\_regsql.exe" utility within the \Windows\Microsoft.NET\Framework\v2.0.50727\ directory to add the appropriate schema for membership and the other ASP.NET application services to a database.</span></span>

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a><span data-ttu-id="444d9-146">[Authorize] フィルターを使用して、ディナー/作成 URL の承認</span><span class="sxs-lookup"><span data-stu-id="444d9-146">Authorizing the /Dinners/Create URL using the [Authorize] filter</span></span>

<span data-ttu-id="444d9-147">セキュリティで保護された認証および NerdDinner アプリケーションのアカウント管理の実装を有効にするコードを記述するがありませんでした。</span><span class="sxs-lookup"><span data-stu-id="444d9-147">We didn't have to write any code to enable a secure authentication and account management implementation for the NerdDinner application.</span></span> <span data-ttu-id="444d9-148">ユーザーは、このアプリケーション、およびサイトのログインとログアウトを新しいアカウントを登録できます。</span><span class="sxs-lookup"><span data-stu-id="444d9-148">Users can register new accounts with our application, and login/logout of the site.</span></span>

<span data-ttu-id="444d9-149">今すぐ、アプリケーションに承認ロジックを追加し、認証の状態と訪問者のユーザー名を使用できます、確認と、サイト内で実行できない操作を制御おできます。</span><span class="sxs-lookup"><span data-stu-id="444d9-149">Now we can add authorization logic to the application, and use the authentication status and username of visitors to control what they can and can't do within the site.</span></span> <span data-ttu-id="444d9-150">DinnersController クラスの「作成」のアクション メソッドに承認ロジックを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="444d9-150">Let's begin by adding authorization logic to the "Create" action methods of our DinnersController class.</span></span> <span data-ttu-id="444d9-151">具体的には、私たちが必要にアクセスするユーザー、*ディナー/作成*に URL を記録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="444d9-151">Specifically, we will require that users accessing the */Dinners/Create* URL must be logged in.</span></span> <span data-ttu-id="444d9-152">ログオンしていない場合リダイレクトにログイン ページにすることがサインインできるようにします。</span><span class="sxs-lookup"><span data-stu-id="444d9-152">If they aren't logged in we'll redirect them to the login page so that they can sign-in.</span></span>

<span data-ttu-id="444d9-153">このロジックを実装することは非常に簡単です。</span><span class="sxs-lookup"><span data-stu-id="444d9-153">Implementing this logic is pretty easy.</span></span> <span data-ttu-id="444d9-154">必要なタスクは、アクション メソッドを作成するに [Authorize] フィルター属性を追加する次のようにします。</span><span class="sxs-lookup"><span data-stu-id="444d9-154">All we need to-do is to add an [Authorize] filter attribute to our Create action methods like so:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

<span data-ttu-id="444d9-155">ASP.NET MVC には、アクション メソッドを宣言して適用できる再利用可能なロジックを実装するために使用する、「アクション フィルター」を作成する機能がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="444d9-155">ASP.NET MVC supports the ability to create "action filters" that can be used to implement re-usable logic that can be declaratively applied to action methods.</span></span> <span data-ttu-id="444d9-156">[Authorize] フィルターは、ASP.NET MVC によって提供される組み込みのアクション フィルターのいずれかと、開発者が宣言によってアクション メソッドとコント ローラー クラスに承認規則を適用することができます。</span><span class="sxs-lookup"><span data-stu-id="444d9-156">The [Authorize] filter is one of the built-in action filters provided by ASP.NET MVC, and it enables a developer to declaratively apply authorization rules to action methods and controller classes.</span></span>

<span data-ttu-id="444d9-157">パラメーター (上記のような) を使用せずに適用されるときに [Authorize] フィルター、強制的にアクション メソッド要求を出しているユーザーは、– でログに記録する必要がありますが自動的にリダイレクトされますブラウザー ログイン URL にそうでない場合。</span><span class="sxs-lookup"><span data-stu-id="444d9-157">When applied without any parameters (like above) the [Authorize] filter enforces that the user making the action method request must be logged in – and it will automatically redirect the browser to the login URL if they aren't.</span></span> <span data-ttu-id="444d9-158">最初に要求された URL は、クエリ文字列引数として渡されるこのリダイレクトを実行する場合 (例:/アカウント/ログオンですか?ReturnUrl = % 2fDinners %2fcreate)。</span><span class="sxs-lookup"><span data-stu-id="444d9-158">When doing this redirect the originally requested URL is passed as a querystring argument (for example: /Account/LogOn?ReturnUrl=%2fDinners%2fCreate).</span></span> <span data-ttu-id="444d9-159">ログインすると、AccountController は最初に要求された URL に、ユーザーをリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="444d9-159">The AccountController will then redirect the user back to the originally requested URL once they login.</span></span>

<span data-ttu-id="444d9-160">必要に応じて、[Authorize] フィルターには、必要とするユーザーはいずれのログ記録とで許可されたユーザーや許可されているセキュリティ ロールのメンバーの一覧の中に使用できる、"Users"または「ロール」のプロパティを指定する機能がサポートしています。</span><span class="sxs-lookup"><span data-stu-id="444d9-160">The [Authorize] filter optionally supports the ability to specify a "Users" or "Roles" property that can be used to require that the user is both logged in and within a list of allowed users or a member of an allowed security role.</span></span> <span data-ttu-id="444d9-161">たとえば、次のコードのみが許可ディナー/作成 URL にアクセスするには、次の 2 つの特定のユーザー、"scottgu"および"billg"。</span><span class="sxs-lookup"><span data-stu-id="444d9-161">For example, the code below only allows two specific users, "scottgu" and "billg", to access the /Dinners/Create URL:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

<span data-ttu-id="444d9-162">コード内で特定のユーザー名を埋め込むことがかなりされていない intainable ある傾向があります。</span><span class="sxs-lookup"><span data-stu-id="444d9-162">Embedding specific user names within code tends to be pretty un-maintainable though.</span></span> <span data-ttu-id="444d9-163">適切な方法は、上位レベルの"roles"を定義する、に対してコードをチェックして、そのユーザーをデータベースまたは (コードから外部に格納される実際のユーザー マッピングのリストを有効にする) active directory システムのいずれかを使用して、役割にマップするためにします。</span><span class="sxs-lookup"><span data-stu-id="444d9-163">A better approach is to define higher-level "roles" that the code checks against, and then to map users into the role using either a database or active directory system (enabling the actual user mapping list to be stored externally from the code).</span></span> <span data-ttu-id="444d9-164">ASP.NET には、組み込みのロールの管理 API だけでなく (SQL と Active Directory のものを含む) のロール プロバイダーのうち、このユーザー/ロール マッピングを実行できる一連の組み込みが含まれています。</span><span class="sxs-lookup"><span data-stu-id="444d9-164">ASP.NET includes a built-in role management API as well as a built-in set of role providers (including ones for SQL and Active Directory) that can help perform this user/role mapping.</span></span> <span data-ttu-id="444d9-165">ディナー/作成 URL にアクセスする特定の"admin"ロール内のユーザーのみを許可するコードを更新すること、します。</span><span class="sxs-lookup"><span data-stu-id="444d9-165">We could then update the code to only allow users within a specific "admin" role to access the /Dinners/Create URL:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a><span data-ttu-id="444d9-166">作成するときに、User.Identity.Name プロパティを使用してディナー</span><span class="sxs-lookup"><span data-stu-id="444d9-166">Using the User.Identity.Name property when Creating Dinners</span></span>

<span data-ttu-id="444d9-167">コント ローラーの基本クラスで公開されている User.Identity.Name プロパティを使用して、要求の現在のログイン ユーザーのユーザー名を取得できます。</span><span class="sxs-lookup"><span data-stu-id="444d9-167">We can retrieve the username of the currently logged-in user of a request using the User.Identity.Name property exposed on the Controller base class.</span></span>

<span data-ttu-id="444d9-168">以前は、HTTP POST メソッドのバージョンの Create() アクションが実装された場合が発生しました。 ハードコードされた静的な文字列を Dinner の"HostedBy"プロパティです。</span><span class="sxs-lookup"><span data-stu-id="444d9-168">Earlier when we implemented the HTTP-POST version of our Create() action method we had hardcoded the "HostedBy" property of the Dinner to a static string.</span></span> <span data-ttu-id="444d9-169">お User.Identity.Name プロパティを使用するには、このコードを更新したりできるよう Dinner を作成するホストの出欠に自動的に追加します。</span><span class="sxs-lookup"><span data-stu-id="444d9-169">We can now update this code to instead use the User.Identity.Name property, as well as automatically add an RSVP for the host creating the Dinner:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

<span data-ttu-id="444d9-170">Create() メソッドに [Authorize] 属性を追加したため ASP.NET MVC はアクション メソッドは、サイト上ディナー/作成 URL へのアクセス、ユーザーがログオンしている場合のみが実行されるようにします。</span><span class="sxs-lookup"><span data-stu-id="444d9-170">Because we have added an [Authorize] attribute to the Create() method, ASP.NET MVC ensures that the action method only executes if the user visiting the /Dinners/Create URL is logged in on the site.</span></span> <span data-ttu-id="444d9-171">そのため、User.Identity.Name プロパティの値には、有効なユーザー名は常に含まれます。</span><span class="sxs-lookup"><span data-stu-id="444d9-171">As such, the User.Identity.Name property value will always contain a valid username.</span></span>

### <a name="using-the-useridentityname-property-when-editing-dinners"></a><span data-ttu-id="444d9-172">編集するときに、User.Identity.Name プロパティを使用してディナー</span><span class="sxs-lookup"><span data-stu-id="444d9-172">Using the User.Identity.Name property when Editing Dinners</span></span>

<span data-ttu-id="444d9-173">今すぐディナー ホスト自体のプロパティ編集のみできるようにユーザーを制限するいくつかの承認ロジックを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="444d9-173">Let's now add some authorization logic that restricts users so that they can only edit the properties of dinners they themselves are hosting.</span></span>

<span data-ttu-id="444d9-174">このため、まず"IsHostedBy(username)"ヘルパー メソッド、夕食オブジェクト (前に作成した Dinner.cs 部分クラス) 内に追加します。</span><span class="sxs-lookup"><span data-stu-id="444d9-174">To help with this, we'll first add an "IsHostedBy(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="444d9-175">このヘルパー メソッドには、true または指定されたユーザー名が Dinner HostedBy プロパティと一致して、それらの大文字と小文字の文字列比較を実行する必要なロジックをカプセル化するかどうかに応じて false が返されます。</span><span class="sxs-lookup"><span data-stu-id="444d9-175">This helper method returns true or false depending on whether a supplied username matches the Dinner HostedBy property, and encapsulates the logic necessary to perform a case-insensitive string comparison of them:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

<span data-ttu-id="444d9-176">追加 [Authorize] 属性 Edit() のアクション メソッドに、DinnersController クラス内で。</span><span class="sxs-lookup"><span data-stu-id="444d9-176">We'll then add an [Authorize] attribute to the Edit() action methods within our DinnersController class.</span></span> <span data-ttu-id="444d9-177">これにより、確実要求にユーザーにログインする必要があります、 */Dinners/編集/[id]* URL。</span><span class="sxs-lookup"><span data-stu-id="444d9-177">This will ensure that users must be logged in to request a */Dinners/Edit/[id]* URL.</span></span>

<span data-ttu-id="444d9-178">おできますし、コード メソッドを追加、編集を Dinner.IsHostedBy(username) ヘルパー メソッドを使用してログインしたユーザーが Dinner ホストと一致していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="444d9-178">We can then add code to our Edit methods that uses the Dinner.IsHostedBy(username) helper method to verify that the logged-in user matches the Dinner host.</span></span> <span data-ttu-id="444d9-179">場合は、ユーザーは、ホストではありません、おを"InvalidOwner"ビューを表示し、要求を終了します。</span><span class="sxs-lookup"><span data-stu-id="444d9-179">If the user is not the host, we'll display an "InvalidOwner" view and terminate the request.</span></span> <span data-ttu-id="444d9-180">これを行うコードは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="444d9-180">The code to do this looks like below:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

<span data-ttu-id="444d9-181">お \Views\Dinners ディレクトリを右クリックし、このオプションを選択すると追加-&gt;新しい"InvalidOwner"ビューを作成するメニュー コマンドを表示します。</span><span class="sxs-lookup"><span data-stu-id="444d9-181">We can then right-click on the \Views\Dinners directory and choose the Add-&gt;View menu command to create a new "InvalidOwner" view.</span></span> <span data-ttu-id="444d9-182">そこにありますは、次のエラー メッセージ。</span><span class="sxs-lookup"><span data-stu-id="444d9-182">We'll populate it with the below error message:</span></span>

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

<span data-ttu-id="444d9-183">ユーザーが所有していない dinner を編集しようとしたとき、みましょうエラー メッセージ。</span><span class="sxs-lookup"><span data-stu-id="444d9-183">And now when a user attempts to edit a dinner they don't own, they'll get an error message:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

<span data-ttu-id="444d9-184">Delete() アクション メソッドのと同じ手順を繰り返しますおをロックダウン アクセス許可を同様に、ディナーを削除し、夕食のホストだけと削除することを確認し、コント ローラー内で。</span><span class="sxs-lookup"><span data-stu-id="444d9-184">We can repeat the same steps for the Delete() action methods within our controller to lock down permission to delete Dinners as well, and ensure that only the host of a Dinner can delete it.</span></span>

### <a name="showinghiding-edit-and-delete-links"></a><span data-ttu-id="444d9-185">表示/非表示の編集と削除のリンク</span><span class="sxs-lookup"><span data-stu-id="444d9-185">Showing/Hiding Edit and Delete Links</span></span>

<span data-ttu-id="444d9-186">この詳細 URL から DinnersController クラスの編集および削除アクション メソッドにリンクします。</span><span class="sxs-lookup"><span data-stu-id="444d9-186">We are linking to the Edit and Delete action method of our DinnersController class from our Details URL:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

<span data-ttu-id="444d9-187">現在の詳細の URL に訪問者が dinner のホストであるかに関係なく、編集および削除アクション リンクを示します。</span><span class="sxs-lookup"><span data-stu-id="444d9-187">Currently we are showing the Edit and Delete action links regardless of whether the visitor to the details URL is the host of the dinner.</span></span> <span data-ttu-id="444d9-188">説明されるように変更、訪問したユーザーが dinner の所有者である場合にのみ、リンクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="444d9-188">Let's change this so that the links are only displayed if the visiting user is the owner of the dinner.</span></span>

<span data-ttu-id="444d9-189">DinnersController 内 Details() アクション メソッドは、Dinner オブジェクトを取得し、モデル オブジェクトとして、ビュー テンプレートに渡します。</span><span class="sxs-lookup"><span data-stu-id="444d9-189">The Details() action method within our DinnersController retrieves a Dinner object and then passes it as the model object to our view template:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

<span data-ttu-id="444d9-190">条件付きで表示/非表示、編集および削除のリンク、Dinner.IsHostedBy() 以下のようなヘルパー メソッドを使用して、ビュー テンプレートを更新することをがします。</span><span class="sxs-lookup"><span data-stu-id="444d9-190">We can update our view template to conditionally show/hide the Edit and Delete links by using the Dinner.IsHostedBy() helper method like below:</span></span>

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a><span data-ttu-id="444d9-191">次の手順</span><span class="sxs-lookup"><span data-stu-id="444d9-191">Next Steps</span></span>

<span data-ttu-id="444d9-192">これで、認証されたユーザーに AJAX を使用してディナーの RSVP を達成する方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="444d9-192">Let's now look at how we can enable authenticated users to RSVP for dinners using AJAX.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="444d9-193">[前へ](implement-efficient-data-paging.md)
[次へ](use-ajax-to-deliver-dynamic-updates.md)</span><span class="sxs-lookup"><span data-stu-id="444d9-193">[Previous](implement-efficient-data-paging.md)
[Next](use-ajax-to-deliver-dynamic-updates.md)</span></span>
