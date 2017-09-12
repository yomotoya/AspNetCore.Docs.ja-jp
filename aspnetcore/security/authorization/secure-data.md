---
title: "認証によって保護されているユーザー データと ASP.NET Core アプリケーションを作成します。"
author: rick-anderson
keywords: "ASP.NET Core、MVC、承認、ロール、セキュリティ、管理者"
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.assetid: abeb2f8e-dfbf-4398-a04c-338a613a65bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: db05ffb585022c3d9512d32da28c54788f97129c
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="eeb62-103">認証によって保護されているユーザー データと ASP.NET Core アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-103">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="eeb62-104">によって[Rick Anderson](https://twitter.com/RickAndMSFT)と[Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="eeb62-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="eeb62-105">このチュートリアルでは、認証によって保護されているユーザー データと web アプリを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-105">This tutorial shows how to create a web app with user data protected by authorization.</span></span> <span data-ttu-id="eeb62-106">(登録済み) のユーザーを認証した連絡先の一覧を表示を作成します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-106">It  displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="eeb62-107">これには次の 3 つのセキュリティ グループがあります。</span><span class="sxs-lookup"><span data-stu-id="eeb62-107">There are three security groups:</span></span>

* <span data-ttu-id="eeb62-108">登録済みのユーザーには、承認されているすべての連絡先データを表示できます。</span><span class="sxs-lookup"><span data-stu-id="eeb62-108">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="eeb62-109">登録済みユーザーを編集/削除が独自のデータ。</span><span class="sxs-lookup"><span data-stu-id="eeb62-109">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="eeb62-110">管理者では、承認したり、連絡先データを拒否することができます。</span><span class="sxs-lookup"><span data-stu-id="eeb62-110">Managers can approve or reject contact data.</span></span> <span data-ttu-id="eeb62-111">許可されている連絡先のみがユーザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="eeb62-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="eeb62-112">および管理者が承認または却下とすべてのデータを編集/削除します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-112">Administrators can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="eeb62-113">次の図では、ユーザー Rick (`rick@example.com`) 署名します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="eeb62-114">ユーザー Rick ことができますのみビュー連絡先を承認し、連絡先を編集/削除します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-114">User Rick can only view approved contacts and edit/delete his contacts.</span></span> <span data-ttu-id="eeb62-115">のみ、最後のレコードは、Rick によって作成され、編集を表示のリンクを削除</span><span class="sxs-lookup"><span data-stu-id="eeb62-115">Only the last record, created by Rick, displays edit and delete links</span></span>

![上記で説明したイメージ](secure-data/_static/rick.png)

<span data-ttu-id="eeb62-117">次の図の`manager@contoso.com`とマネージャーの役割では署名されています。</span><span class="sxs-lookup"><span data-stu-id="eeb62-117">In the following image, `manager@contoso.com` is signed in and in the managers role.</span></span> 

![上記で説明したイメージ](secure-data/_static/manager1.png)

<span data-ttu-id="eeb62-119">次の図は、連絡先の詳細ビューをマネージャーには。</span><span class="sxs-lookup"><span data-stu-id="eeb62-119">The following image shows the  managers details view of a contact.</span></span>

![上記で説明したイメージ](secure-data/_static/manager.png)

<span data-ttu-id="eeb62-121">管理者と管理者のみ、承認があるし、ボタンを拒否します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-121">Only managers and administrators have the approve and reject buttons.</span></span>

<span data-ttu-id="eeb62-122">次の図の`admin@contoso.com`と管理者の役割では署名されています。</span><span class="sxs-lookup"><span data-stu-id="eeb62-122">In the following image, `admin@contoso.com` is signed in and in the administrator’s role.</span></span> 

![上記で説明したイメージ](secure-data/_static/admin.png)

<span data-ttu-id="eeb62-124">管理者は、すべての特権を持ちます。</span><span class="sxs-lookup"><span data-stu-id="eeb62-124">The administrator has all privileges.</span></span> <span data-ttu-id="eeb62-125">彼女は、読み取り/編集/削除のいずれかの連絡先をことができ、連絡先の状態を変更します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-125">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="eeb62-126">アプリがによって作成された[スキャフォールディング](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)次`Contact`モデル。</span><span class="sxs-lookup"><span data-stu-id="eeb62-126">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)  the following `Contact` model:</span></span>

<span data-ttu-id="eeb62-127">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-127">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span></span>

<span data-ttu-id="eeb62-128">A`ContactIsOwnerAuthorizationHandler`承認ハンドラーにより、ユーザーがそのデータを編集できるのみです。</span><span class="sxs-lookup"><span data-stu-id="eeb62-128">A `ContactIsOwnerAuthorizationHandler` authorization handler ensures that a user can only edit their data.</span></span> <span data-ttu-id="eeb62-129">A`ContactManagerAuthorizationHandler`承認ハンドラーは、承認または却下の連絡先のマネージャーを使用します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-129">A `ContactManagerAuthorizationHandler` authorization handler allows managers to approve or reject contacts.</span></span>  <span data-ttu-id="eeb62-130">A`ContactAdministratorsAuthorizationHandler`承認ハンドラーは、管理者の承認または拒否の連絡先を編集/削除の連絡先を使用します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-130">A `ContactAdministratorsAuthorizationHandler` authorization handler allows administrators to approve or reject contacts and to edit/delete contacts.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="eeb62-131">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="eeb62-131">Prerequisites</span></span>

<span data-ttu-id="eeb62-132">これは最初のチュートリアルではありません。</span><span class="sxs-lookup"><span data-stu-id="eeb62-132">This is not a beginning tutorial.</span></span> <span data-ttu-id="eeb62-133">理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="eeb62-133">You should be familiar with:</span></span>

* [<span data-ttu-id="eeb62-134">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="eeb62-134">ASP.NET Core MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="eeb62-135">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="eeb62-135">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="eeb62-136">Starter および完成したアプリ</span><span class="sxs-lookup"><span data-stu-id="eeb62-136">The starter and completed app</span></span>

<span data-ttu-id="eeb62-137">[ダウンロード](xref:tutorials/index#how-to-download-a-sample)、[完了](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final)アプリ。</span><span class="sxs-lookup"><span data-stu-id="eeb62-137">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app.</span></span> <span data-ttu-id="eeb62-138">[テスト](#test-the-completed-app)完成したアプリのセキュリティ機能を理解しておくようにします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-138">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span> 

### <a name="the-starter-app"></a><span data-ttu-id="eeb62-139">スターター アプリ</span><span class="sxs-lookup"><span data-stu-id="eeb62-139">The starter app</span></span>

<span data-ttu-id="eeb62-140">完成したサンプル コードを比較することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-140">It's helpful to compare your code with the completed sample.</span></span>

<span data-ttu-id="eeb62-141">[ダウンロード](xref:tutorials/index#how-to-download-a-sample)、[スターター](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter)アプリ。</span><span class="sxs-lookup"><span data-stu-id="eeb62-141">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app.</span></span> 

<span data-ttu-id="eeb62-142">参照してください[スターター アプリを作成する](#create-the-starter-app)をゼロから作成したい場合。</span><span class="sxs-lookup"><span data-stu-id="eeb62-142">See [Create the starter app](#create-the-starter-app) if you'd like to create it from scratch.</span></span>

<span data-ttu-id="eeb62-143">データベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-143">Update the database:</span></span>

```none
   dotnet ef database update
```

<span data-ttu-id="eeb62-144">アプリを実行する、タップ、 **ContactManager**リンク、および作成、編集、および連絡先を削除することを確認します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-144">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

<span data-ttu-id="eeb62-145">このチュートリアルでは、すべての主要な手順、セキュリティで保護されたユーザー データのアプリを作成するには。</span><span class="sxs-lookup"><span data-stu-id="eeb62-145">This tutorial has all the major steps to create the secure user data app.</span></span> <span data-ttu-id="eeb62-146">完成したプロジェクトを参照すると便利です。</span><span class="sxs-lookup"><span data-stu-id="eeb62-146">You may find it helpful to refer to the completed project.</span></span>

## <a name="modify-the-app-to-secure-user-data"></a><span data-ttu-id="eeb62-147">ユーザー データを保護するアプリを変更します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-147">Modify the app to secure user data</span></span>

<span data-ttu-id="eeb62-148">次のセクションでは、セキュリティで保護されたユーザー データのアプリを作成するすべての主要な手順があります。</span><span class="sxs-lookup"><span data-stu-id="eeb62-148">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="eeb62-149">完成したプロジェクトを参照すると便利です。</span><span class="sxs-lookup"><span data-stu-id="eeb62-149">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="eeb62-150">ユーザーに連絡先データを関連付ける</span><span class="sxs-lookup"><span data-stu-id="eeb62-150">Tie the contact data to the user</span></span>

<span data-ttu-id="eeb62-151">ASP.NET を使用して[Identity](xref:security/authentication/identity)のユーザーのユーザー ID を編集できますが、そのデータを他のユーザー データは表示されません。</span><span class="sxs-lookup"><span data-stu-id="eeb62-151">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="eeb62-152">追加`OwnerID`を`Contact`モデル。</span><span class="sxs-lookup"><span data-stu-id="eeb62-152">Add `OwnerID` to the `Contact` model:</span></span>

<span data-ttu-id="eeb62-153">[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-153">[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]</span></span>

<span data-ttu-id="eeb62-154">`OwnerID`ユーザーの id、`AspNetUser`テーブルに、 [Identity](xref:security/authentication/identity)データベース。</span><span class="sxs-lookup"><span data-stu-id="eeb62-154">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="eeb62-155">`Status`フィールドは、連絡先が一般のユーザーによって表示可能であるかどうか。</span><span class="sxs-lookup"><span data-stu-id="eeb62-155">The `Status` field determines if a contact is viewable by general users.</span></span> 

<span data-ttu-id="eeb62-156">新しい移行をスキャフォールディングして、データベースの更新します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-156">Scaffold a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="eeb62-157">SSL と認証済みユーザーが必要</span><span class="sxs-lookup"><span data-stu-id="eeb62-157">Require SSL and authenticated users</span></span>

<span data-ttu-id="eeb62-158">`ConfigureServices`のメソッド、 *Startup.cs*ファイルに追加し、 [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api)承認フィルター。</span><span class="sxs-lookup"><span data-stu-id="eeb62-158">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api) authorization filter:</span></span>

<span data-ttu-id="eeb62-159">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-159">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]</span></span>

<span data-ttu-id="eeb62-160">Visual Studio を使用している場合は、次を参照してください。 [SSL または HTTPS の IIS Express をセットアップ](xref:security/enforcing-ssl#set-up-iis-express-for-sslhttps)です。</span><span class="sxs-lookup"><span data-stu-id="eeb62-160">If you're using Visual Studio, see [Set up IIS Express for SSL/HTTPS](xref:security/enforcing-ssl#set-up-iis-express-for-sslhttps).</span></span> <span data-ttu-id="eeb62-161">HTTP 要求を HTTPS にリダイレクトするを参照してください。 [URL 書き換えミドルウェア](xref:fundamentals/url-rewriting)です。</span><span class="sxs-lookup"><span data-stu-id="eeb62-161">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="eeb62-162">Visual Studio のコードを使用してまたはローカルのプラットフォームでテストする場合は、テスト証明書を SSL の含まれていません。</span><span class="sxs-lookup"><span data-stu-id="eeb62-162">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="eeb62-163">設定`"LocalTest:skipSSL": true`で、*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="eeb62-163">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="eeb62-164">ユーザーが認証されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="eeb62-164">Require authenticated users</span></span>

<span data-ttu-id="eeb62-165">ユーザー認証を必要とする既定の認証ポリシーを設定します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-165">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="eeb62-166">選択を解除とコント ローラーまたはアクション メソッドに認証する、`[AllowAnonymous]`属性。</span><span class="sxs-lookup"><span data-stu-id="eeb62-166">You can opt out of authentication at the controller or action method with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="eeb62-167">この方法で、新しいコント ローラーを追加が自動的に認証が必要に含める新しいコント ローラーに依存するよりも安全である、`[Authorize]`属性。</span><span class="sxs-lookup"><span data-stu-id="eeb62-167">With this approach, any new controllers added will automatically require authentication, which is safer than relying on new controllers to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="eeb62-168">次の追加、`ConfigureServices`のメソッド、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="eeb62-168">Add the following to  the `ConfigureServices` method of the *Startup.cs* file:</span></span>

<span data-ttu-id="eeb62-169">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-169">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]</span></span>

<span data-ttu-id="eeb62-170">追加`[AllowAnonymous]`home コント ローラーを登録する前に、匿名ユーザーはサイトに関する情報を取得できるようにします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-170">Add `[AllowAnonymous]` to the home controller so anonymous users can get information about the site before they register.</span></span>

<span data-ttu-id="eeb62-171">[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-171">[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="eeb62-172">テスト アカウントを構成します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-172">Configure the test account</span></span>

<span data-ttu-id="eeb62-173">`SeedData`クラスは、2 つのアカウント、管理者およびマネージャーを作成します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-173">The `SeedData` class creates two accounts,  administrator and manager.</span></span> <span data-ttu-id="eeb62-174">使用して、[シークレット マネージャー ツール](xref:security/app-secrets)これらのアカウントのパスワードを設定します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-174">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="eeb62-175">これは、プロジェクト ディレクトリから実行 (ディレクトリを含む*Program.cs*)。</span><span class="sxs-lookup"><span data-stu-id="eeb62-175">Do this from the project directory (the directory containing *Program.cs*).</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="eeb62-176">更新`Configure`テスト パスワードを使用します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-176">Update `Configure` to use the test password:</span></span>

<span data-ttu-id="eeb62-177">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-177">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]</span></span>

<span data-ttu-id="eeb62-178">管理者のユーザー ID を追加し、`Status = ContactStatus.Approved`連絡先にします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-178">Add the administrator user ID and `Status = ContactStatus.Approved` to the contacts.</span></span> <span data-ttu-id="eeb62-179">1 人だけが表示されると、すべての連絡先にユーザー ID を追加します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-179">Only one contact is shown, add the user ID to all contacts:</span></span>

<span data-ttu-id="eeb62-180">[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-180">[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]</span></span>

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="eeb62-181">所有者、マネージャー、および管理者の承認のハンドラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-181">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="eeb62-182">作成、`ContactIsOwnerAuthorizationHandler`クラス内で、*承認*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="eeb62-182">Create a `ContactIsOwnerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="eeb62-183">`ContactIsOwnerAuthorizationHandler`はリソースで動作しているユーザーは、リソースを所有していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-183">The `ContactIsOwnerAuthorizationHandler` will verify the user acting on the resource owns the resource.</span></span>

<span data-ttu-id="eeb62-184">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-184">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]</span></span>

<span data-ttu-id="eeb62-185">`ContactIsOwnerAuthorizationHandler`呼び出し`context.Succeed`現在の認証済みユーザーがメンバーの所有者である場合。</span><span class="sxs-lookup"><span data-stu-id="eeb62-185">The `ContactIsOwnerAuthorizationHandler` calls `context.Succeed` if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="eeb62-186">認証ハンドラーが通常返す`context.Succeed`要件を満たしている場合にします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-186">Authorization handlers generally return `context.Succeed` when the requirements are met.</span></span> <span data-ttu-id="eeb62-187">返される`Task.FromResult(0)`要件が満たされないときにします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-187">They return `Task.FromResult(0)` when requirements are not met.</span></span> <span data-ttu-id="eeb62-188">`Task.FromResult(0)`成功または失敗のどちらもその他の認証ハンドラーを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="eeb62-188">`Task.FromResult(0)` is neither success or failure, it allows other authorization handler to run.</span></span> <span data-ttu-id="eeb62-189">明示的に失敗する場合は、返す`context.Fail()`です。</span><span class="sxs-lookup"><span data-stu-id="eeb62-189">If you need to explicitly fail, return `context.Fail()`.</span></span>

<span data-ttu-id="eeb62-190">ここには、ため、要件のパラメーターに渡す操作を確認する必要はありませんに自分のデータを編集/削除する連絡先の所有者ができるようにします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-190">We allow contact owners to edit/delete their own data, so we don't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="eeb62-191">マネージャーの認証ハンドラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-191">Create a manager authorization handler</span></span>

<span data-ttu-id="eeb62-192">作成、`ContactManagerAuthorizationHandler`クラス内で、*承認*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="eeb62-192">Create a `ContactManagerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="eeb62-193">`ContactManagerAuthorizationHandler`リソースに対して機能しているユーザーは、マネージャーが確認されます。</span><span class="sxs-lookup"><span data-stu-id="eeb62-193">The `ContactManagerAuthorizationHandler` will verify the user acting on the resource is a manager.</span></span> <span data-ttu-id="eeb62-194">マネージャーだけでは、承認したり、コンテンツの変更 (新しいまたは変更された) を拒否することができます。</span><span class="sxs-lookup"><span data-stu-id="eeb62-194">Only managers can approve or reject content changes (new or changed).</span></span>

<span data-ttu-id="eeb62-195">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-195">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]</span></span>

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="eeb62-196">管理者認証ハンドラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-196">Create an administrator authorization handler</span></span>

<span data-ttu-id="eeb62-197">作成、`ContactAdministratorsAuthorizationHandler`クラス内で、*承認*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="eeb62-197">Create a `ContactAdministratorsAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="eeb62-198">`ContactAdministratorsAuthorizationHandler`リソースに対して機能しているユーザーは管理者が確認されます。</span><span class="sxs-lookup"><span data-stu-id="eeb62-198">The `ContactAdministratorsAuthorizationHandler` will verify the user acting on the resource is a administrator.</span></span> <span data-ttu-id="eeb62-199">管理者は、すべての操作を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="eeb62-199">Administrator can do all operations.</span></span>

<span data-ttu-id="eeb62-200">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-200">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]</span></span>

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="eeb62-201">認証ハンドラーを登録します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-201">Register the authorization handlers</span></span>

<span data-ttu-id="eeb62-202">Entity Framework のコアを使用してサービスを登録する必要があります[依存性の注入](xref:fundamentals/dependency-injection)を使用して[AddScoped](https://docs.microsoft.com/aspnet/core/api)です。</span><span class="sxs-lookup"><span data-stu-id="eeb62-202">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](https://docs.microsoft.com/aspnet/core/api).</span></span> <span data-ttu-id="eeb62-203">`ContactIsOwnerAuthorizationHandler` ASP.NET Core を使用して[Identity](xref:security/authentication/identity)、これは Entity Framework Core 上に構築します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-203">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="eeb62-204">ハンドラー コレクションに登録サービスをで使用できるように、`ContactsController`を通じて[依存性の注入](xref:fundamentals/dependency-injection)です。</span><span class="sxs-lookup"><span data-stu-id="eeb62-204">Register the handlers with the service collection so they will be available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="eeb62-205">末尾に次のコードを追加`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="eeb62-205">Add the following code to the end of `ConfigureServices`:</span></span>

<span data-ttu-id="eeb62-206">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-206">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]</span></span>

<span data-ttu-id="eeb62-207">`ContactAdministratorsAuthorizationHandler`および`ContactManagerAuthorizationHandler`シングルトンとして追加されます。</span><span class="sxs-lookup"><span data-stu-id="eeb62-207">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="eeb62-208">これらはシングルトンの EF を使用していないし、では、必要なすべての情報が、`Context`のパラメーター、`HandleRequirementAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="eeb62-208">They are singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

<span data-ttu-id="eeb62-209">完全な`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="eeb62-209">The complete `ConfigureServices`:</span></span>

<span data-ttu-id="eeb62-210">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-210">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]</span></span>

## <a name="update-the-code-to-support-authorization"></a><span data-ttu-id="eeb62-211">承認をサポートするコードを更新します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-211">Update the code to support authorization</span></span>

<span data-ttu-id="eeb62-212">このセクションでは、コント ローラーとビューを更新し、操作の要件クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-212">In this section, you update the controller and views and add an operations requirements class.</span></span>

### <a name="update-the-contacts-controller"></a><span data-ttu-id="eeb62-213">連絡先のコント ローラーを更新します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-213">Update the Contacts controller</span></span>

<span data-ttu-id="eeb62-214">更新プログラム、`ContactsController`コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="eeb62-214">Update the `ContactsController` constructor:</span></span>

* <span data-ttu-id="eeb62-215">追加、`IAuthorizationService`承認ハンドラーへのアクセスをサービスします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-215">Add the `IAuthorizationService` service to  access to the authorization handlers.</span></span> 
* <span data-ttu-id="eeb62-216">追加、 `Identity` `UserManager`サービス。</span><span class="sxs-lookup"><span data-stu-id="eeb62-216">Add the `Identity` `UserManager` service:</span></span>

<span data-ttu-id="eeb62-217">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-217">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]</span></span>

### <a name="add-a-contact-operations-requirements-class"></a><span data-ttu-id="eeb62-218">連絡先の操作の要件のクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-218">Add a contact operations requirements class</span></span>

<span data-ttu-id="eeb62-219">追加、`ContactOperationsRequirements`クラスを*承認*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="eeb62-219">Add the `ContactOperationsRequirements` class to the *Authorization* folder.</span></span> <span data-ttu-id="eeb62-220">このクラスは、要件を含む、アプリケーションがサポートします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-220">This class  contain the requirements our app supports:</span></span>

<span data-ttu-id="eeb62-221">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-221">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span></span>

### <a name="update-create"></a><span data-ttu-id="eeb62-222">更新プログラムを作成します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-222">Update Create</span></span>

<span data-ttu-id="eeb62-223">更新プログラム、`HTTP POST Create`メソッド。</span><span class="sxs-lookup"><span data-stu-id="eeb62-223">Update the `HTTP POST Create` method to:</span></span>

* <span data-ttu-id="eeb62-224">ユーザー ID を追加、`Contact`モデル。</span><span class="sxs-lookup"><span data-stu-id="eeb62-224">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="eeb62-225">ユーザーに連絡先を所有していることを確認する認証ハンドラーを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-225">Call the authorization handler to verify the user owns the contact.</span></span>

<span data-ttu-id="eeb62-226">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-226">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]</span></span>

### <a name="update-edit"></a><span data-ttu-id="eeb62-227">更新プログラムの編集</span><span class="sxs-lookup"><span data-stu-id="eeb62-227">Update Edit</span></span>

<span data-ttu-id="eeb62-228">両方を更新`Edit`ユーザーを確認する認証ハンドラーを使用する方法を連絡先を所有しています。</span><span class="sxs-lookup"><span data-stu-id="eeb62-228">Update both `Edit` methods to use the authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="eeb62-229">リソースの承認を実行しているために使用できません、`[Authorize]`属性。</span><span class="sxs-lookup"><span data-stu-id="eeb62-229">Because we are performing resource authorization we cannot use the `[Authorize]` attribute.</span></span> <span data-ttu-id="eeb62-230">属性が評価されるときに、リソースへのアクセスはありません。</span><span class="sxs-lookup"><span data-stu-id="eeb62-230">We don't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="eeb62-231">リソース ベースの承認は、命令型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="eeb62-231">Resource based authorization must be imperative.</span></span> <span data-ttu-id="eeb62-232">コント ローラーに読み込むか、ハンドラー自体内で読み込むことにより、リソースへのアクセスを取得したら、チェックを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="eeb62-232">Checks must be performed once we have access to the resource, either by loading it in our controller, or by loading it within the handler itself.</span></span> <span data-ttu-id="eeb62-233">頻繁には、リソースをリソース キーを渡すことによってアクセスします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-233">Frequently you will access the resource by passing in the resource key.</span></span>

<span data-ttu-id="eeb62-234">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-234">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]</span></span>

### <a name="update-the-delete-method"></a><span data-ttu-id="eeb62-235">Update、Delete メソッド</span><span class="sxs-lookup"><span data-stu-id="eeb62-235">Update the Delete method</span></span>

<span data-ttu-id="eeb62-236">両方を更新`Delete`ユーザーを確認する認証ハンドラーを使用する方法を連絡先を所有しています。</span><span class="sxs-lookup"><span data-stu-id="eeb62-236">Update both `Delete` methods to use the authorization handler to verify the user owns the contact.</span></span>

<span data-ttu-id="eeb62-237">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-237">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]</span></span>

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="eeb62-238">承認サービスをビューに挿入します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-238">Inject the authorization service into the views</span></span>

<span data-ttu-id="eeb62-239">現在、UI に示すは編集し、ユーザーが変更できないデータへのリンクを削除します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-239">Currently the UI shows edit and delete links for data the user cannot modify.</span></span> <span data-ttu-id="eeb62-240">認証ハンドラーをビューに適用することによって修正されます。</span><span class="sxs-lookup"><span data-stu-id="eeb62-240">We'll fix that by applying the authorization handler to the views.</span></span>

<span data-ttu-id="eeb62-241">承認サービスでの挿入、 *Views/_ViewImports.cshtml*ファイルのすべてのビューに利用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-241">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it will be available to all views:</span></span>

<span data-ttu-id="eeb62-242">[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-242">[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]</span></span>

<span data-ttu-id="eeb62-243">更新プログラム、 *Views/Contacts/Index.cshtml* Razor ビューのみを表示、編集し編集/削除の連絡先がユーザー用のリンクを削除します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-243">Update the *Views/Contacts/Index.cshtml* Razor view to only display the edit and delete links for users who can edit/delete the contact.</span></span>

<span data-ttu-id="eeb62-244">追加`@using ContactManager.Authorization;`</span><span class="sxs-lookup"><span data-stu-id="eeb62-244">Add `@using ContactManager.Authorization;`</span></span>

<span data-ttu-id="eeb62-245">更新プログラム、`Edit`と`Delete`リンクを編集および連絡先を削除するアクセス許可を持つユーザーのみレンダリングされるようにします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-245">Update the `Edit` and `Delete` links so they are only rendered for users with permission to edit and delete the contact.</span></span>

<span data-ttu-id="eeb62-246">[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-246">[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]</span></span>

<span data-ttu-id="eeb62-247">警告: データ編集または削除するアクセス許可がないユーザーからのリンクを非表示にしても、アプリは保護しません。</span><span class="sxs-lookup"><span data-stu-id="eeb62-247">Warning: Hiding links from users that do not have permission to edit or delete data does not secure the app.</span></span> <span data-ttu-id="eeb62-248">リンクを非表示にするはアプリよりユーザー フレンドリによって唯一の有効なリンクを表示します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-248">Hiding links makes the app more user friendly by displaying only valid links.</span></span> <span data-ttu-id="eeb62-249">ユーザーは、編集を呼び出すし、自分が所有しないデータの操作を削除するには、生成された Url を切断できます。</span><span class="sxs-lookup"><span data-stu-id="eeb62-249">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span>  <span data-ttu-id="eeb62-250">コント ローラーは、アクセスをセキュリティで保護するチェックを繰り返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="eeb62-250">The controller must repeat the access checks to be secure.</span></span>

### <a name="update-the-details-view"></a><span data-ttu-id="eeb62-251">更新プログラムの詳細ビュー</span><span class="sxs-lookup"><span data-stu-id="eeb62-251">Update the Details view</span></span>

<span data-ttu-id="eeb62-252">マネージャーが承認または連絡先を拒否するため、詳細ビューを更新します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-252">Update the details view so managers can approve or reject contacts:</span></span>

<span data-ttu-id="eeb62-253">[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-253">[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="eeb62-254">完成したアプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-254">Test the completed app</span></span>

<span data-ttu-id="eeb62-255">Visual Studio のコードを使用してまたはローカルのプラットフォームでテストする場合は、テスト証明書を SSL の含まれていません。</span><span class="sxs-lookup"><span data-stu-id="eeb62-255">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="eeb62-256">設定`"LocalTest:skipSSL": true`で、*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="eeb62-256">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

<span data-ttu-id="eeb62-257">アプリを実行した連絡先がある場合は、内のすべてのレコードを削除、`Contact`テーブルが表示され、データベースのシードにアプリを再起動します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-257">If you have run the app and have contacts, delete all the records in the `Contact` table and restart the app to seed the database.</span></span> <span data-ttu-id="eeb62-258">Visual Studio を使用している場合は、終了して、データベースのシードに IIS Express を再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="eeb62-258">If you are using Visual Studio, you need to exit and restart IIS Express to seed the database.</span></span>

<span data-ttu-id="eeb62-259">連絡先を参照するユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-259">Register a user to browse the contacts.</span></span>

<span data-ttu-id="eeb62-260">完成したアプリをテストする簡単な方法では、3 つの異なるブラウザー (または incognito/inprivate ブラウズのバージョン) を起動します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-260">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="eeb62-261">1 つのブラウザーで新しいユーザーの例では、登録`test@example.com`です。</span><span class="sxs-lookup"><span data-stu-id="eeb62-261">In one browser, register a new user, for example, `test@example.com`.</span></span> <span data-ttu-id="eeb62-262">別のユーザーに各ブラウザーにサインインします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-262">Sign in to each browser with a different user.</span></span> <span data-ttu-id="eeb62-263">次のことを検証します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-263">Verify the following:</span></span>

* <span data-ttu-id="eeb62-264">登録済みのユーザーには、承認されているすべての連絡先データを表示できます。</span><span class="sxs-lookup"><span data-stu-id="eeb62-264">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="eeb62-265">登録済みユーザーを編集/削除が独自のデータ。</span><span class="sxs-lookup"><span data-stu-id="eeb62-265">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="eeb62-266">管理者では、承認したり、連絡先データを拒否することができます。</span><span class="sxs-lookup"><span data-stu-id="eeb62-266">Managers can approve or reject contact data.</span></span> <span data-ttu-id="eeb62-267">`Details`ビューには、**承認**と**拒否**ボタン。</span><span class="sxs-lookup"><span data-stu-id="eeb62-267">The `Details` view shows **Approve** and **Reject** buttons.</span></span> 
* <span data-ttu-id="eeb62-268">および管理者が承認または却下とすべてのデータを編集/削除します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-268">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="eeb62-269">ユーザー</span><span class="sxs-lookup"><span data-stu-id="eeb62-269">User</span></span>| <span data-ttu-id="eeb62-270">オプション</span><span class="sxs-lookup"><span data-stu-id="eeb62-270">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="eeb62-271">データの編集、または削除できます。</span><span class="sxs-lookup"><span data-stu-id="eeb62-271">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="eeb62-272">承認または拒否し、編集/削除を所有できるデータ</span><span class="sxs-lookup"><span data-stu-id="eeb62-272">Can approve/reject and edit/delete own data</span></span>  |
| admin@contoso.com | <span data-ttu-id="eeb62-273">編集/削除してすべてのデータの承認または却下</span><span class="sxs-lookup"><span data-stu-id="eeb62-273">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="eeb62-274">管理者のブラウザーで連絡先を作成します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-274">Create a contact in the administrators browser.</span></span> <span data-ttu-id="eeb62-275">削除の URL をコピーし、管理者の連絡先から編集します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-275">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="eeb62-276">これらのリンクをテスト ユーザーは、これらの操作を実行できないことを確認するテスト ユーザーのブラウザーに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="eeb62-276">Paste these links into the test user's browser to verify the test user cannot perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="eeb62-277">スターター アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-277">Create the starter app</span></span>

<span data-ttu-id="eeb62-278">スタート アプリケーションを作成する手順に従います。</span><span class="sxs-lookup"><span data-stu-id="eeb62-278">Follow these instructions to create the starter app.</span></span>

* <span data-ttu-id="eeb62-279">作成、 **ASP.NET Core Web アプリケーション**を使用して[Visual Studio 2017](https://www.visualstudio.com/) "ContactManager"という名前</span><span class="sxs-lookup"><span data-stu-id="eeb62-279">Create an **ASP.NET Core Web Application** using [Visual Studio 2017](https://www.visualstudio.com/) named "ContactManager"</span></span>

  * <span data-ttu-id="eeb62-280">使用してアプリを作成する**個々 のユーザー アカウント**です。</span><span class="sxs-lookup"><span data-stu-id="eeb62-280">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="eeb62-281">名前を付けます"ContactManager"名前空間は、名前空間で使用するサンプルと一致するようにします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-281">Name it "ContactManager" so your namespace will match the namespace use in the sample.</span></span>

* <span data-ttu-id="eeb62-282">次の追加`Contact`モデル。</span><span class="sxs-lookup"><span data-stu-id="eeb62-282">Add the following `Contact` model:</span></span>

  <span data-ttu-id="eeb62-283">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-283">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span></span>

* <span data-ttu-id="eeb62-284">Scaffold、`Contact`エンティティ フレームワークのコアを使用するモデルと`ApplicationDbContext`データ コンテキスト。</span><span class="sxs-lookup"><span data-stu-id="eeb62-284">Scaffold the `Contact` model using Entity Framework Core and the `ApplicationDbContext` data context.</span></span> <span data-ttu-id="eeb62-285">すべてのスキャフォールディング既定値をそのまま使用します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-285">Accept all the scaffolding defaults.</span></span> <span data-ttu-id="eeb62-286">使用して`ApplicationDbContext`データ コンテキストのクラス、contact テーブルに、 [Identity](xref:security/authentication/identity)データベース。</span><span class="sxs-lookup"><span data-stu-id="eeb62-286">Using `ApplicationDbContext` for the data context class  puts the contact table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="eeb62-287">参照してください[モデルを追加する](xref:tutorials/first-mvc-app/adding-model)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-287">See [Adding a model](xref:tutorials/first-mvc-app/adding-model) for more information.</span></span>

* <span data-ttu-id="eeb62-288">更新プログラム、 **ContactManager**でアンカー、 *Views/Shared/_Layout.cshtml*ファイルから`asp-controller="Home"`に`asp-controller="Contacts"`ためをタップすると、 **ContactManager**リンク連絡先コント ローラーを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-288">Update the **ContactManager** anchor in the *Views/Shared/_Layout.cshtml* file from `asp-controller="Home"` to `asp-controller="Contacts"` so tapping the **ContactManager** link will invoke the Contacts controller.</span></span> <span data-ttu-id="eeb62-289">元のマークアップ。</span><span class="sxs-lookup"><span data-stu-id="eeb62-289">The original markup:</span></span>

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

<span data-ttu-id="eeb62-290">更新されたマークアップ:</span><span class="sxs-lookup"><span data-stu-id="eeb62-290">The updated markup:</span></span>

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* <span data-ttu-id="eeb62-291">初期の移行をスキャフォールディングして、データベースの更新</span><span class="sxs-lookup"><span data-stu-id="eeb62-291">Scaffold the initial migration and update the database</span></span>

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* <span data-ttu-id="eeb62-292">アプリをテストするには、作成、編集、および連絡先を削除します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-292">Test the app by creating, editing and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="eeb62-293">データベースをシードします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-293">Seed the database</span></span>

<span data-ttu-id="eeb62-294">追加、`SeedData`クラスを*データ*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="eeb62-294">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="eeb62-295">サンプル、ダウンロードした場合は、コピー、 *SeedData.cs*ファイルの名前を*データ*のスタート プロジェクトのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="eeb62-295">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="eeb62-296">[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-296">[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]</span></span>

<span data-ttu-id="eeb62-297">末尾に強調表示されたコードを追加、`Configure`メソッドで、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="eeb62-297">Add the highlighted code to the end of the `Configure` method in the *Startup.cs* file:</span></span>

<span data-ttu-id="eeb62-298">[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-298">[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]</span></span>

<span data-ttu-id="eeb62-299">アプリに、データベースがシード処理をテストします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-299">Test that the app seeded the database.</span></span> <span data-ttu-id="eeb62-300">Seed メソッドは、DB の連絡先のすべての行がある場合に実行されません。</span><span class="sxs-lookup"><span data-stu-id="eeb62-300">The seed method does not run if there are any rows in the contact DB.</span></span>

### <a name="create-a-class-used-in-the-tutorial"></a><span data-ttu-id="eeb62-301">このチュートリアルで使用されるクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-301">Create a class used in the tutorial</span></span>

* <span data-ttu-id="eeb62-302">という名前のフォルダーを作成する*承認*です。</span><span class="sxs-lookup"><span data-stu-id="eeb62-302">Create a folder named *Authorization*.</span></span>
* <span data-ttu-id="eeb62-303">コピー、 *Authorization\ContactOperations.cs*完成したプロジェクトのダウンロードからファイルまたは次のコードをコピーします。</span><span class="sxs-lookup"><span data-stu-id="eeb62-303">Copy the *Authorization\ContactOperations.cs* file from the completed project download, or copy the following code:</span></span>

<span data-ttu-id="eeb62-304">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span><span class="sxs-lookup"><span data-stu-id="eeb62-304">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span></span>

<a name=secure-data-add-resources-label></a>

### <a name="additional-resources"></a><span data-ttu-id="eeb62-305">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="eeb62-305">Additional resources</span></span>

* <span data-ttu-id="eeb62-306">[ASP.NET Core 承認ラボ](https://github.com/blowdart/AspNetAuthorizationWorkshop)です。</span><span class="sxs-lookup"><span data-stu-id="eeb62-306">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="eeb62-307">このラボでは、このチュートリアルで導入されたセキュリティ機能の詳細に移動します。</span><span class="sxs-lookup"><span data-stu-id="eeb62-307">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="eeb62-308">ASP.NET Core での承認: 単純な場合、ロール、および信頼性情報ベースのカスタム</span><span class="sxs-lookup"><span data-stu-id="eeb62-308">Authorization in ASP.NET Core : Simple, role, claims-based and custom</span></span>](index.md)
* [<span data-ttu-id="eeb62-309">カスタム ポリシー ベースの承認</span><span class="sxs-lookup"><span data-stu-id="eeb62-309">Custom Policy-Based Authorization</span></span>](policies.md)
