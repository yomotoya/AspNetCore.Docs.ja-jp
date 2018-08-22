---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: 認証と承認を使用してアプリケーションをセキュリティ保護 |Microsoft Docs
author: microsoft
description: 手順 9 は、ユーザーが登録する必要が、認証と承認 NerdDinner アプリケーションでは、セキュリティで保護するを追加する方法を説明し、作成するには、サイトにログインしています.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: d5f1b26312f11fd6d4ab500c7f24a4d89d428e38
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829851"
---
<a name="secure-applications-using-authentication-and-authorization"></a><span data-ttu-id="cdcd1-103">認証と承認を使用してアプリケーションをセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-103">Secure Applications Using Authentication and Authorization</span></span>
====================
<span data-ttu-id="cdcd1-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="cdcd1-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="cdcd1-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="cdcd1-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="cdcd1-106">これは、無料の手順 9 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-106">This is step 9 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="cdcd1-107">手順 9 は、ユーザーが登録する必要があるように、認証と承認 NerdDinner アプリケーションでは、セキュリティで保護するを追加する方法を示します新しい dinners と、dinner をホストしているユーザーのみを作成するには、サイトへのログインが後で編集できます。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-107">Step 9 shows how to add authentication and authorization to secure our NerdDinner application, so that users need to register and login to the site to create new dinners, and only the user who is hosting a dinner can edit it later.</span></span>
> 
> <span data-ttu-id="cdcd1-108">次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-9-authentication-and-authorization"></a><span data-ttu-id="cdcd1-109">NerdDinner 手順 9: 認証と承認</span><span class="sxs-lookup"><span data-stu-id="cdcd1-109">NerdDinner Step 9: Authentication and Authorization</span></span>

<span data-ttu-id="cdcd1-110">今すぐ、NerdDinner をアプリケーションに許可されたすべてのユーザーは、サイトを作成し、任意の dinner の詳細を編集する機能にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-110">Right now our NerdDinner application grants anyone visiting the site the ability to create and edit the details of any dinner.</span></span> <span data-ttu-id="cdcd1-111">ユーザーが登録する必要がありますようにこれを変更してみましょうと新しい dinners を作成し、夕食をホストしているユーザーのみが後で編集できるように、制限を追加するサイトにログインします。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-111">Let's change this so that users need to register and login to the site to create new dinners, and add a restriction so that only the user who is hosting a dinner can edit it later.</span></span>

<span data-ttu-id="cdcd1-112">これを有効にするには、認証と承認をアプリケーションをセキュリティで保護するのにに使用します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-112">To enable this we'll use authentication and authorization to secure our application.</span></span>

### <a name="understanding-authentication-and-authorization"></a><span data-ttu-id="cdcd1-113">認証と承認</span><span class="sxs-lookup"><span data-stu-id="cdcd1-113">Understanding Authentication and Authorization</span></span>

<span data-ttu-id="cdcd1-114">*認証*は識別およびアプリケーションにアクセスするクライアントの id を検証するプロセスです。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-114">*Authentication* is the process of identifying and validating the identity of a client accessing an application.</span></span> <span data-ttu-id="cdcd1-115">さらに簡単に言うと、「ユーザー」、エンドユーザーが web サイトを訪問するときに識別することです。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-115">Put more simply, it is about identifying "who" the end-user is when they visit a website.</span></span> <span data-ttu-id="cdcd1-116">ASP.NET では、複数のブラウザーのユーザーを認証する方法をサポートします。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-116">ASP.NET supports multiple ways to authenticate browser users.</span></span> <span data-ttu-id="cdcd1-117">インターネットの web アプリケーション、使用される最も一般的な認証方法には「フォーム認証」が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-117">For Internet web applications, the most common authentication approach used is called "Forms Authentication".</span></span> <span data-ttu-id="cdcd1-118">フォーム認証を使用すると、開発者は自社のアプリケーションの HTML ログイン フォームを作成し、データベースやその他のパスワード資格情報ストアに対して、エンドユーザーが送信するユーザー名とパスワードを検証します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-118">Forms Authentication enables a developer to author an HTML login form within their application, and then validate the username/password an end-user submits against a database or other password credential store.</span></span> <span data-ttu-id="cdcd1-119">ユーザー名/パスワードの組み合わせが正しい場合は、開発者は、今後の要求間でユーザーを識別するために暗号化された HTTP クッキーを発行するための ASP.NET を依頼できます。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-119">If the username/password combination is correct, the developer can then ask ASP.NET to issue an encrypted HTTP cookie to identify the user across future requests.</span></span> <span data-ttu-id="cdcd1-120">NerdDinner アプリケーションでフォーム認証を使用して作成します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-120">We'll by using forms authentication with our NerdDinner application.</span></span>

<span data-ttu-id="cdcd1-121">*承認*は、認証されたユーザーが特定の URL/リソースへのアクセスまたはアクションを実行するアクセス許可を持つかどうかを決定するプロセスです。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-121">*Authorization* is the process of determining whether an authenticated user has permission to access a particular URL/resource or to perform some action.</span></span> <span data-ttu-id="cdcd1-122">など、NerdDinner アプリケーション内で必要ありますでログオンしているユーザーのみがアクセスできることを承認するために、 */Dinners/作成*URL と新しい Dinners を作成します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-122">For example, within our NerdDinner application we'll want to authorize that only users who are logged in can access the */Dinners/Create* URL and create new Dinners.</span></span> <span data-ttu-id="cdcd1-123">私たちは、夕食をホストしているユーザーのみが編集および他のすべてのユーザーに対して編集のアクセスが拒否されるように、承認ロジックを追加する必要もあります。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-123">We'll also want to add authorization logic so that only the user who is hosting a dinner can edit it – and deny edit access to all other users.</span></span>

### <a name="forms-authentication-and-the-accountcontroller"></a><span data-ttu-id="cdcd1-124">フォーム認証と、AccountController</span><span class="sxs-lookup"><span data-stu-id="cdcd1-124">Forms Authentication and the AccountController</span></span>

<span data-ttu-id="cdcd1-125">ASP.NET MVC の既定の Visual Studio プロジェクト テンプレートは、新しい ASP.NET MVC アプリケーションを作成すると、フォーム認証を自動的に有効にします。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-125">The default Visual Studio project template for ASP.NET MVC automatically enables forms authentication when new ASP.NET MVC applications are created.</span></span> <span data-ttu-id="cdcd1-126">– プロジェクトを使用して、サイト内のセキュリティを統合する非常に簡単に構築済みのアカウントのログイン ページの実装も自動的に追加します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-126">It also automatically adds a pre-built account login page implementation to the project – which makes it really easy to integrate security within a site.</span></span>

<span data-ttu-id="cdcd1-127">既定の Site.master マスター ページでは、それにアクセスするユーザーが認証されていない場合、サイトの右上にある [ログオン] のリンクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-127">The default Site.master master page displays a "Log On" link at the top-right of the site when the user accessing it is not authenticated:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

<span data-ttu-id="cdcd1-128">ユーザーには、[ログオン] リンクをクリックすると、 */アカウント/ログオン*URL:</span><span class="sxs-lookup"><span data-stu-id="cdcd1-128">Clicking the "Log On" link takes a user to the */Account/LogOn* URL:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

<span data-ttu-id="cdcd1-129">訪問者に登録していないようにすることができる –「登録」リンクをクリックして、 */アカウント/登録*URL とアカウントの詳細を入力するようにします。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-129">Visitors who haven't registered can do so by clicking the "Register" link – which will take them to the */Account/Register* URL and allow them to enter account details:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

<span data-ttu-id="cdcd1-130">「登録」 ボタンをクリックすると、ASP.NET メンバーシップ システム内で新しいユーザーの作成し、フォーム認証を使用して、サイトにユーザーを認証します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-130">Clicking the "Register" button will create a new user within the ASP.NET Membership system, and authenticate the user onto the site using forms authentication.</span></span>

<span data-ttu-id="cdcd1-131">Site.master 変更、「ようこそ [username]!」を出力するページの右上でユーザーがログインしている場合、</span><span class="sxs-lookup"><span data-stu-id="cdcd1-131">When a user is logged-in, the Site.master changes the top-right of the page to output a "Welcome [username]!"</span></span> <span data-ttu-id="cdcd1-132">メッセージと表示を「オン」、「ログの」いずれかの代わりにリンクします。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-132">message and renders a "Log Off" link instead of a "Log On" one.</span></span> <span data-ttu-id="cdcd1-133">「ログオフ」リンクをクリックすると、ユーザーをログアウトします。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-133">Clicking the "Log Off" link logs out the user:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

<span data-ttu-id="cdcd1-134">上記のログイン、ログアウト、および登録機能は、プロジェクトの作成時に Visual Studio によってプロジェクトに追加された AccountController クラス内で実装されます。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-134">The above login, logout, and registration functionality is implemented within the AccountController class that was added to our project by Visual Studio when it created the project.</span></span> <span data-ttu-id="cdcd1-135">AccountController の UI は \Views\Account ディレクトリ内のビュー テンプレートを使用して実装されます。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-135">The UI for the AccountController is implemented using view templates within the \Views\Account directory:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

<span data-ttu-id="cdcd1-136">AccountController クラスでは、ASP.NET フォーム認証システムを使用して、暗号化された認証 cookie、および格納し、ユーザー名/パスワードの検証に ASP.NET メンバーシップ API を発行します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-136">The AccountController class uses the ASP.NET Forms Authentication system to issue encrypted authentication cookies, and the ASP.NET Membership API to store and validate usernames/passwords.</span></span> <span data-ttu-id="cdcd1-137">ASP.NET メンバーシップ API は、拡張により、任意のパスワード資格情報ストアの使用します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-137">The ASP.NET Membership API is extensible and enables any password credential store to be used.</span></span> <span data-ttu-id="cdcd1-138">ASP.NET は、SQL database、または Active Directory 内のユーザー名とパスワードを格納する組み込みのメンバーシップ プロバイダーの実装では出荷されます。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-138">ASP.NET ships with built-in membership provider implementations that store username/passwords within a SQL database, or within Active Directory.</span></span>

<span data-ttu-id="cdcd1-139">NerdDinner アプリケーションは、プロジェクトのルートに"web.config"ファイルを開き、検索で使用するメンバーシップ プロバイダーを構成することができます、&lt;メンバーシップ&gt;セクション内にします。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-139">We can configure which membership provider our NerdDinner application should use by opening the "web.config" file at the root of the project and looking for the &lt;membership&gt; section within it.</span></span> <span data-ttu-id="cdcd1-140">追加プロジェクトの作成時に既定の web.config が SQL メンバーシップ プロバイダーを登録し、"ApplicationServices"という名前の接続文字列を使用するように構成データベースの場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-140">The default web.config added when the project was created registers the SQL membership provider, and configures it to use a connection-string named "ApplicationServices" to specify the database location.</span></span>

<span data-ttu-id="cdcd1-141">既定の"ApplicationServices"接続文字列 (内で指定される、 &lt;connectionStrings&gt; web.config ファイルのセクション) SQL Express を使用するように構成します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-141">The default "ApplicationServices" connection string (which is specified within the &lt;connectionStrings&gt; section of the web.config file) is configured to use SQL Express.</span></span> <span data-ttu-id="cdcd1-142">"ASPNETDB をという名前の SQL Express データベースを指します。アプリケーションの下で"MDF"アプリ\_データ"のディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-142">It points to a SQL Express database named "ASPNETDB.MDF" under the application's "App\_Data" directory.</span></span> <span data-ttu-id="cdcd1-143">このデータベースが最初に、アプリケーション内で、メンバーシップ API を使用するときに存在しない場合 ASP.NET は自動的にデータベースを作成し、内に適切なメンバーシップ データベース スキーマをプロビジョニングします。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-143">If this database doesn't exist the first time the Membership API is used within the application, ASP.NET will automatically create the database and provision the appropriate membership database schema within it:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

<span data-ttu-id="cdcd1-144">SQL Express を使用して、完全な SQL Server インスタンス (またはリモート データベースへの接続) を使用せずにすべてが必要になります to do 場合は、web.config ファイル内で"ApplicationServices"接続文字列を更新し、ことを確認しますが、適切なメンバーシップ スキーマポイントのデータベースに追加されました。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-144">If instead of using SQL Express we wanted to use a full SQL Server instance (or connect to a remote database), all we'd need to-do is to update the "ApplicationServices" connection string within the web.config file and make sure that the appropriate membership schema has been added to the database it points at.</span></span> <span data-ttu-id="cdcd1-145">実行することができます、"aspnet\_regsql.exe"、データベースにメンバーシップと、その他の ASP.NET アプリケーション サービスの適切なスキーマを追加する \Windows\Microsoft.NET\Framework\v2.0.50727\ ディレクトリ内でユーティリティ。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-145">You can run the "aspnet\_regsql.exe" utility within the \Windows\Microsoft.NET\Framework\v2.0.50727\ directory to add the appropriate schema for membership and the other ASP.NET application services to a database.</span></span>

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a><span data-ttu-id="cdcd1-146">[Authorize] フィルターを使用して、Dinners/作成 URL の承認</span><span class="sxs-lookup"><span data-stu-id="cdcd1-146">Authorizing the /Dinners/Create URL using the [Authorize] filter</span></span>

<span data-ttu-id="cdcd1-147">NerdDinner アプリケーションのアカウント管理の実装をセキュリティで保護された認証を有効にするコードを記述することがありませんでした。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-147">We didn't have to write any code to enable a secure authentication and account management implementation for the NerdDinner application.</span></span> <span data-ttu-id="cdcd1-148">ユーザーは、当社のアプリケーション、およびサイトのログイン/ログアウトを新しいアカウントを登録できます。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-148">Users can register new accounts with our application, and login/logout of the site.</span></span>

<span data-ttu-id="cdcd1-149">今すぐ、アプリケーションに承認ロジックを追加して認証の状態と訪問者のユーザー名を使用できる、確認と、サイト内で実行できないことを制御します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-149">Now we can add authorization logic to the application, and use the authentication status and username of visitors to control what they can and can't do within the site.</span></span> <span data-ttu-id="cdcd1-150">まず、DinnersController クラスの「作成」のアクション メソッドに承認ロジックを追加することから始めます。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-150">Let's begin by adding authorization logic to the "Create" action methods of our DinnersController class.</span></span> <span data-ttu-id="cdcd1-151">具体的には、私たちがする必要がありますにアクセスするユーザー、 */Dinners/作成*に URL を記録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-151">Specifically, we will require that users accessing the */Dinners/Create* URL must be logged in.</span></span> <span data-ttu-id="cdcd1-152">ログオンしていない場合リダイレクトにログイン ページにことがサインインできるようにします。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-152">If they aren't logged in we'll redirect them to the login page so that they can sign-in.</span></span>

<span data-ttu-id="cdcd1-153">このロジックを実装することは非常に簡単です。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-153">Implementing this logic is pretty easy.</span></span> <span data-ttu-id="cdcd1-154">To-do は、Create アクション メソッドに [Authorize] フィルター属性を追加するようになります。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-154">All we need to-do is to add an [Authorize] filter attribute to our Create action methods like so:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

<span data-ttu-id="cdcd1-155">ASP.NET MVC には、宣言でアクション メソッドに適用できる再利用可能なロジックを実装するために使用できる「アクション フィルター」を作成する機能がサポートしています。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-155">ASP.NET MVC supports the ability to create "action filters" that can be used to implement re-usable logic that can be declaratively applied to action methods.</span></span> <span data-ttu-id="cdcd1-156">[Authorize] フィルターは、ASP.NET MVC、によって提供される組み込みのアクション フィルターの 1 つでき、開発者は宣言でアクション メソッドとコント ローラー クラスに承認規則を適用できます。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-156">The [Authorize] filter is one of the built-in action filters provided by ASP.NET MVC, and it enables a developer to declaratively apply authorization rules to action methods and controller classes.</span></span>

<span data-ttu-id="cdcd1-157">(上記のような) パラメーターを指定せずに適用されるときに自動的にリダイレクトする必要がブラウザーのログイン URL にこれらができない場合、アクション メソッド要求を行っているユーザーは、– でログに記録する必要があります [Authorize] フィルターを適用します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-157">When applied without any parameters (like above) the [Authorize] filter enforces that the user making the action method request must be logged in – and it will automatically redirect the browser to the login URL if they aren't.</span></span> <span data-ttu-id="cdcd1-158">最初に要求された URL は、クエリ文字列引数として渡されるこのリダイレクトを実行する場合 (例:/アカウント/ログオンでしょうか。ReturnUrl = % 2fDinners %2fcreate)。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-158">When doing this redirect the originally requested URL is passed as a querystring argument (for example: /Account/LogOn?ReturnUrl=%2fDinners%2fCreate).</span></span> <span data-ttu-id="cdcd1-159">ログインしたら、AccountController は最初に要求された URL にし、ユーザーをリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-159">The AccountController will then redirect the user back to the originally requested URL once they login.</span></span>

<span data-ttu-id="cdcd1-160">[Authorize] フィルターには、必要に応じて、ユーザーが両方に記録する許可されたユーザーまたは許可されているセキュリティ ロールのメンバーの一覧内で要求するように使用できる「ユーザー」または「ロール」のプロパティを指定する機能がサポートしています。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-160">The [Authorize] filter optionally supports the ability to specify a "Users" or "Roles" property that can be used to require that the user is both logged in and within a list of allowed users or a member of an allowed security role.</span></span> <span data-ttu-id="cdcd1-161">たとえば、次のコードは、Dinners/作成 URL にアクセスするには、2 つの特定のユーザー、"scottgu"および「billg のことです」をのみできます。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-161">For example, the code below only allows two specific users, "scottgu" and "billg", to access the /Dinners/Create URL:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

<span data-ttu-id="cdcd1-162">埋め込みコード内で特定のユーザー名は、ただし非常されていない intainable ある傾向があります。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-162">Embedding specific user names within code tends to be pretty un-maintainable though.</span></span> <span data-ttu-id="cdcd1-163">優れたアプローチより高度な"roles"を定義する、コードをチェックし、データベースまたは active directory システムの (コードから外部に格納される実際のユーザー マッピングの一覧を有効にする) を使用してロールにユーザーをマップします。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-163">A better approach is to define higher-level "roles" that the code checks against, and then to map users into the role using either a database or active directory system (enabling the actual user mapping list to be stored externally from the code).</span></span> <span data-ttu-id="cdcd1-164">ASP.NET には、組み込みのロール管理 API と組み込みの (SQL と Active Directory のものを含む)、このユーザー/ロール マッピングを実行できるロール プロバイダーのセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-164">ASP.NET includes a built-in role management API as well as a built-in set of role providers (including ones for SQL and Active Directory) that can help perform this user/role mapping.</span></span> <span data-ttu-id="cdcd1-165">Dinners/作成 URL にアクセスする特定の「管理者」ロール内のユーザーのみを許可するコードを更新し、でした。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-165">We could then update the code to only allow users within a specific "admin" role to access the /Dinners/Create URL:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a><span data-ttu-id="cdcd1-166">作成するときに、User.Identity.Name プロパティを使用して Dinners</span><span class="sxs-lookup"><span data-stu-id="cdcd1-166">Using the User.Identity.Name property when Creating Dinners</span></span>

<span data-ttu-id="cdcd1-167">コント ローラーの基本クラスで公開されている User.Identity.Name プロパティを使用して、要求の現在のログイン ユーザーのユーザー名を取得できます。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-167">We can retrieve the username of the currently logged-in user of a request using the User.Identity.Name property exposed on the Controller base class.</span></span>

<span data-ttu-id="cdcd1-168">以前、Create() アクション メソッドの HTTP POST のバージョンを実装したときにありましたハードコードされた静的な文字列を Dinner の"HostedBy"プロパティです。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-168">Earlier when we implemented the HTTP-POST version of our Create() action method we had hardcoded the "HostedBy" property of the Dinner to a static string.</span></span> <span data-ttu-id="cdcd1-169">私たちできます User.Identity.Name プロパティを使用するには、このコードを更新するようになりましただけでなく、夕食を作成するホストに、予約を自動的に追加。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-169">We can now update this code to instead use the User.Identity.Name property, as well as automatically add an RSVP for the host creating the Dinner:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

<span data-ttu-id="cdcd1-170">Create() メソッドに [Authorize] 属性を追加したため、ASP.NET MVC は、アクション メソッドは、Dinners/作成の URL にアクセスするユーザーがサイトにログインしている場合のみが実行されるを確認します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-170">Because we have added an [Authorize] attribute to the Create() method, ASP.NET MVC ensures that the action method only executes if the user visiting the /Dinners/Create URL is logged in on the site.</span></span> <span data-ttu-id="cdcd1-171">そのため、User.Identity.Name プロパティの値には、有効なユーザー名は常に含まれます。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-171">As such, the User.Identity.Name property value will always contain a valid username.</span></span>

### <a name="using-the-useridentityname-property-when-editing-dinners"></a><span data-ttu-id="cdcd1-172">編集するときに、User.Identity.Name プロパティを使用して Dinners</span><span class="sxs-lookup"><span data-stu-id="cdcd1-172">Using the User.Identity.Name property when Editing Dinners</span></span>

<span data-ttu-id="cdcd1-173">今すぐ dinners ホスト自体のプロパティしか編集するためにユーザーを制限するいくつかの承認ロジックを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-173">Let's now add some authorization logic that restricts users so that they can only edit the properties of dinners they themselves are hosting.</span></span>

<span data-ttu-id="cdcd1-174">このため、最初"IsHostedBy(username)"のヘルパー メソッド (前に作成した Dinner.cs 部分クラス) 内、Dinner オブジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-174">To help with this, we'll first add an "IsHostedBy(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="cdcd1-175">True または false かどうか指定されたユーザー名は、Dinner HostedBy プロパティと一致して、それらの大文字の文字列比較を実行するために必要なロジックをカプセル化によって、このヘルパー メソッドが返されます。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-175">This helper method returns true or false depending on whether a supplied username matches the Dinner HostedBy property, and encapsulates the logic necessary to perform a case-insensitive string comparison of them:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

<span data-ttu-id="cdcd1-176">DinnersController クラス内で Edit() のアクション メソッドに [Authorize] 属性から追加します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-176">We'll then add an [Authorize] attribute to the Edit() action methods within our DinnersController class.</span></span> <span data-ttu-id="cdcd1-177">要求にユーザーに記録する必要があることにより、これを */Dinners/編集/[id]* URL。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-177">This will ensure that users must be logged in to request a */Dinners/Edit/[id]* URL.</span></span>

<span data-ttu-id="cdcd1-178">コード Dinner.IsHostedBy(username) ヘルパー メソッドを使用してログインしたユーザーが Dinner ホストと一致していることを確認する、Edit メソッドを追加できます。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-178">We can then add code to our Edit methods that uses the Dinner.IsHostedBy(username) helper method to verify that the logged-in user matches the Dinner host.</span></span> <span data-ttu-id="cdcd1-179">ユーザーが、ホストでない場合が"InvalidOwner"ビューを表示され、要求の終了します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-179">If the user is not the host, we'll display an "InvalidOwner" view and terminate the request.</span></span> <span data-ttu-id="cdcd1-180">これを行うコードは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-180">The code to do this looks like below:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

<span data-ttu-id="cdcd1-181">私たちし \Views\Dinners ディレクトリを右クリックし、選択追加-&gt;新しい"InvalidOwner"ビューを作成するメニュー コマンドを表示します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-181">We can then right-click on the \Views\Dinners directory and choose the Add-&gt;View menu command to create a new "InvalidOwner" view.</span></span> <span data-ttu-id="cdcd1-182">それを設定しますが、次のエラー メッセージ。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-182">We'll populate it with the below error message:</span></span>

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

<span data-ttu-id="cdcd1-183">ユーザーが所有していない夕食を編集しようとした場合、みましょうエラー メッセージ。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-183">And now when a user attempts to edit a dinner they don't own, they'll get an error message:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

<span data-ttu-id="cdcd1-184">Delete() アクション メソッドに対して同じ手順を繰り返し Dinners を同様に、削除して、Dinner のホストのみと削除することを確認するためのアクセス許可をロックダウンするコント ローラー内で。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-184">We can repeat the same steps for the Delete() action methods within our controller to lock down permission to delete Dinners as well, and ensure that only the host of a Dinner can delete it.</span></span>

### <a name="showinghiding-edit-and-delete-links"></a><span data-ttu-id="cdcd1-185">表示/非表示の編集と削除のリンク</span><span class="sxs-lookup"><span data-stu-id="cdcd1-185">Showing/Hiding Edit and Delete Links</span></span>

<span data-ttu-id="cdcd1-186">目的の詳細 URL から、DinnersController クラスの編集、削除のアクション メソッドにリンクします。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-186">We are linking to the Edit and Delete action method of our DinnersController class from our Details URL:</span></span>

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

<span data-ttu-id="cdcd1-187">現在の詳細 URL への訪問者が、dinner のホストであるかに関係なく、編集、削除アクション リンクを示しています。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-187">Currently we are showing the Edit and Delete action links regardless of whether the visitor to the details URL is the host of the dinner.</span></span> <span data-ttu-id="cdcd1-188">みましょうされるように変更、リンクは、訪問したユーザーが、dinner の所有者である場合にのみ表示されます。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-188">Let's change this so that the links are only displayed if the visiting user is the owner of the dinner.</span></span>

<span data-ttu-id="cdcd1-189">DinnersController 内 Details() アクション メソッドでは、Dinner オブジェクトを取得し、モデル オブジェクトとしてビュー テンプレートに渡します。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-189">The Details() action method within our DinnersController retrieves a Dinner object and then passes it as the model object to our view template:</span></span>

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

<span data-ttu-id="cdcd1-190">条件付きで表示/非表示、編集、削除のリンク Dinner.IsHostedBy() の次のようなヘルパー メソッドを使用して、ビュー テンプレートを更新することができます。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-190">We can update our view template to conditionally show/hide the Edit and Delete links by using the Dinner.IsHostedBy() helper method like below:</span></span>

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a><span data-ttu-id="cdcd1-191">次の手順</span><span class="sxs-lookup"><span data-stu-id="cdcd1-191">Next Steps</span></span>

<span data-ttu-id="cdcd1-192">これで、認証されたユーザーの dinners AJAX を使用して予約を達成する方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="cdcd1-192">Let's now look at how we can enable authenticated users to RSVP for dinners using AJAX.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cdcd1-193">[前へ](implement-efficient-data-paging.md)
> [次へ](use-ajax-to-deliver-dynamic-updates.md)</span><span class="sxs-lookup"><span data-stu-id="cdcd1-193">[Previous](implement-efficient-data-paging.md)
[Next](use-ajax-to-deliver-dynamic-updates.md)</span></span>
