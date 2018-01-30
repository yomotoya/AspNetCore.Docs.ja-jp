---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: "手順 7: メンバーシップと承認 |Microsoft ドキュメント"
author: jongalloway
description: "このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 第 7 部では、メンバーシップと承認について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: db459de687db862be00a9b59ff5b1b238fa75061
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
<a name="part-7-membership-and-authorization"></a><span data-ttu-id="cde0d-104">手順 7: メンバーシップと承認</span><span class="sxs-lookup"><span data-stu-id="cde0d-104">Part 7: Membership and Authorization</span></span>
====================
<span data-ttu-id="cde0d-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="cde0d-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="cde0d-106">MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="cde0d-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="cde0d-107">MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。</span><span class="sxs-lookup"><span data-stu-id="cde0d-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="cde0d-108">このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="cde0d-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="cde0d-109">第 7 部では、メンバーシップと承認について説明します。</span><span class="sxs-lookup"><span data-stu-id="cde0d-109">Part 7 covers Membership and Authorization.</span></span>


<span data-ttu-id="cde0d-110">このストア マネージャー コント ローラーは現在、サイトを訪問だれにもアクセス可能なです。</span><span class="sxs-lookup"><span data-stu-id="cde0d-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="cde0d-111">サイト管理者に権限を制限するこれを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="cde0d-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="cde0d-112">AccountController とビューの追加</span><span class="sxs-lookup"><span data-stu-id="cde0d-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="cde0d-113">ASP.NET MVC 3 Web アプリケーション テンプレートのすべてと、ASP.NET MVC 3 空の Web アプリケーション テンプレートの 1 つの違いは、空のテンプレートが、アカウント コント ローラーが含まれていないことです。</span><span class="sxs-lookup"><span data-stu-id="cde0d-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="cde0d-114">すべての ASP.NET MVC 3 Web アプリケーション テンプレートから作成された新しい ASP.NET MVC アプリケーションからいくつかのファイルをコピーして、アカウント コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="cde0d-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="cde0d-115">完全 ASP.NET MVC 3 Web アプリケーション テンプレートを使用して、新しい ASP.NET MVC アプリケーションを作成し、次のファイルをプロジェクトに同じディレクトリにコピーします。</span><span class="sxs-lookup"><span data-stu-id="cde0d-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="cde0d-116">コント ローラーのディレクトリにコピー AccountController.cs</span><span class="sxs-lookup"><span data-stu-id="cde0d-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="cde0d-117">モデルのディレクトリにコピー AccountModels</span><span class="sxs-lookup"><span data-stu-id="cde0d-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="cde0d-118">Views ディレクトリ内のアカウント ディレクトリを作成し、内のすべての 4 つのビューをコピー</span><span class="sxs-lookup"><span data-stu-id="cde0d-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="cde0d-119">コント ローラーとモデルのクラスの名前空間を変更して、名前の先頭 MvcMusicStore ようにします。</span><span class="sxs-lookup"><span data-stu-id="cde0d-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="cde0d-120">AccountController クラスが MvcMusicStore.Controllers 名前空間を使用する必要があり、AccountModels クラスが MvcMusicStore.Models 名前空間を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cde0d-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="cde0d-121">*注: これらのファイルも、チュートリアルの先頭に、サイト設計のファイルをコピーした MvcMusicStore Assets.zip ダウンロードで使用できます。メンバーシップのファイルはコード ディレクトリにあります。*</span><span class="sxs-lookup"><span data-stu-id="cde0d-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="cde0d-122">更新されたソリューションは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="cde0d-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="cde0d-123">ASP.NET 構成サイトを管理ユーザーを追加します。</span><span class="sxs-lookup"><span data-stu-id="cde0d-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="cde0d-124">当社の web サイトの承認を必要としてにアクセス権を持つユーザーを作成する必要になります。</span><span class="sxs-lookup"><span data-stu-id="cde0d-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="cde0d-125">ユーザーを作成する最も簡単な方法では、組み込みの ASP.NET の構成 web サイトを使用します。</span><span class="sxs-lookup"><span data-stu-id="cde0d-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="cde0d-126">次のソリューション エクスプ ローラーで、アイコンをクリックして、ASP.NET の構成 web サイトを起動します。</span><span class="sxs-lookup"><span data-stu-id="cde0d-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="cde0d-127">これは、構成の web サイトを起動します。</span><span class="sxs-lookup"><span data-stu-id="cde0d-127">This launches a configuration website.</span></span> <span data-ttu-id="cde0d-128">ホーム画面で、セキュリティ タブをクリックし、画面の中央に「の役割を有効にする」リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cde0d-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="cde0d-129">「ロールの作成または管理」のリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cde0d-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="cde0d-130">ロール名として"Administrator"を入力し、[役割の追加] ボタンを押します。</span><span class="sxs-lookup"><span data-stu-id="cde0d-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="cde0d-131">戻るボタンをクリックし、左側にあるユーザーの作成リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cde0d-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="cde0d-132">左側の次の情報を使用してユーザー情報フィールドを入力します。</span><span class="sxs-lookup"><span data-stu-id="cde0d-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="cde0d-133">**フィールド**</span><span class="sxs-lookup"><span data-stu-id="cde0d-133">**Field**</span></span> | <span data-ttu-id="cde0d-134">**[値]**</span><span class="sxs-lookup"><span data-stu-id="cde0d-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="cde0d-135">**ユーザー名**</span><span class="sxs-lookup"><span data-stu-id="cde0d-135">**User Name**</span></span> | <span data-ttu-id="cde0d-136">管理者</span><span class="sxs-lookup"><span data-stu-id="cde0d-136">Administrator</span></span> |
| <span data-ttu-id="cde0d-137">**パスワード**</span><span class="sxs-lookup"><span data-stu-id="cde0d-137">**Password**</span></span> | <span data-ttu-id="cde0d-138">password123!</span><span class="sxs-lookup"><span data-stu-id="cde0d-138">password123!</span></span> |
| <span data-ttu-id="cde0d-139">**パスワードの確認入力**</span><span class="sxs-lookup"><span data-stu-id="cde0d-139">**Confirm Password**</span></span> | <span data-ttu-id="cde0d-140">password123!</span><span class="sxs-lookup"><span data-stu-id="cde0d-140">password123!</span></span> |
| <span data-ttu-id="cde0d-141">**電子メール**</span><span class="sxs-lookup"><span data-stu-id="cde0d-141">**E-mail**</span></span> | <span data-ttu-id="cde0d-142">(任意の電子メール アドレスは機能)</span><span class="sxs-lookup"><span data-stu-id="cde0d-142">(any email address will work)</span></span> |
| <span data-ttu-id="cde0d-143">**セキュリティの質問**</span><span class="sxs-lookup"><span data-stu-id="cde0d-143">**Security Question**</span></span> | <span data-ttu-id="cde0d-144">(お好きなよう)</span><span class="sxs-lookup"><span data-stu-id="cde0d-144">(whatever you like)</span></span> |
| <span data-ttu-id="cde0d-145">**セキュリティ返答**</span><span class="sxs-lookup"><span data-stu-id="cde0d-145">**Security Answer**</span></span> | <span data-ttu-id="cde0d-146">(お好きなよう)</span><span class="sxs-lookup"><span data-stu-id="cde0d-146">(whatever you like)</span></span> |

<span data-ttu-id="cde0d-147">*メモ: たい任意のパスワードを使用することができますもちろんです。既定のパスワード セキュリティ設定では、7 文字の長さは、1 つの英数字以外の文字を含むパスワードが必要です。*</span><span class="sxs-lookup"><span data-stu-id="cde0d-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="cde0d-148">このユーザーの管理者ロールを選択し、ユーザーの作成 ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="cde0d-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="cde0d-149">この時点では、ユーザーが正常に作成されたことを示すメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="cde0d-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="cde0d-150">これで、ブラウザー ウィンドウを閉じることができます。</span><span class="sxs-lookup"><span data-stu-id="cde0d-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="cde0d-151">ロール ベースの承認</span><span class="sxs-lookup"><span data-stu-id="cde0d-151">Role-based Authorization</span></span>

<span data-ttu-id="cde0d-152">これで、ユーザーがクラスのどのコント ローラー アクションへのアクセスを管理者ロールにする必要がありますを指定する [Authorize] 属性を使用して StoreManagerController へのアクセスを制限おことができます。</span><span class="sxs-lookup"><span data-stu-id="cde0d-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="cde0d-153">*注: コント ローラーのクラス レベルだけでなく、特定のアクション メソッドでは、[Authorize] 属性を配置できます。*</span><span class="sxs-lookup"><span data-stu-id="cde0d-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="cde0d-154">ログオン ダイアログ/StoreManager を参照するようになりましたが表示されます。</span><span class="sxs-lookup"><span data-stu-id="cde0d-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="cde0d-155">新しい管理者アカウントでログオンした後にする前として、アルバムの編集画面に移動することができました。</span><span class="sxs-lookup"><span data-stu-id="cde0d-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="cde0d-156">[前へ](mvc-music-store-part-6.md)
[次へ](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="cde0d-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
