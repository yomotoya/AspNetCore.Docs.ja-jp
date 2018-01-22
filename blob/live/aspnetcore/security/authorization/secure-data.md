---
title: "認証によって保護されているユーザー データと ASP.NET Core アプリケーションを作成します。"
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: 861ac619c7f5fb19a56c59536e20724d96bbddca
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="02ebc-102">認証によって保護されているユーザー データと ASP.NET Core アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-102">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="02ebc-103">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="02ebc-103">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="02ebc-104">このチュートリアルでは、認証によって保護されているユーザー データと web アプリを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-104">This tutorial shows how to create a web app with user data protected by authorization.</span></span> <span data-ttu-id="02ebc-105">(登録済み) のユーザーを認証した連絡先の一覧を表示を作成します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-105">It  displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="02ebc-106">これには次の 3 つのセキュリティ グループがあります。</span><span class="sxs-lookup"><span data-stu-id="02ebc-106">There are three security groups:</span></span>

* <span data-ttu-id="02ebc-107">登録済みのユーザーには、承認されているすべての連絡先データを表示できます。</span><span class="sxs-lookup"><span data-stu-id="02ebc-107">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="02ebc-108">登録済みユーザーを編集/削除が独自のデータ。</span><span class="sxs-lookup"><span data-stu-id="02ebc-108">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="02ebc-109">管理者では、承認したり、連絡先データを拒否することができます。</span><span class="sxs-lookup"><span data-stu-id="02ebc-109">Managers can approve or reject contact data.</span></span> <span data-ttu-id="02ebc-110">許可されている連絡先のみがユーザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="02ebc-110">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="02ebc-111">および管理者が承認または却下とすべてのデータを編集/削除します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-111">Administrators can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="02ebc-112">次の図では、ユーザー Rick (`rick@example.com`) 署名します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-112">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="02ebc-113">ユーザー Rick ことができますのみビュー連絡先を承認し、連絡先を編集/削除します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-113">User Rick can only view approved contacts and edit/delete his contacts.</span></span> <span data-ttu-id="02ebc-114">のみ、最後のレコードは、Rick によって作成され、編集を表示のリンクを削除</span><span class="sxs-lookup"><span data-stu-id="02ebc-114">Only the last record, created by Rick, displays edit and delete links</span></span>

![上記で説明したイメージ](secure-data/_static/rick.png)

<span data-ttu-id="02ebc-116">次の図の`manager@contoso.com`とマネージャーの役割では署名されています。</span><span class="sxs-lookup"><span data-stu-id="02ebc-116">In the following image, `manager@contoso.com` is signed in and in the managers role.</span></span> 

![上記で説明したイメージ](secure-data/_static/manager1.png)

<span data-ttu-id="02ebc-118">次の図は、連絡先の詳細ビューをマネージャーには。</span><span class="sxs-lookup"><span data-stu-id="02ebc-118">The following image shows the  managers details view of a contact.</span></span>

![上記で説明したイメージ](secure-data/_static/manager.png)

<span data-ttu-id="02ebc-120">管理者と管理者のみ、承認があるし、ボタンを拒否します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-120">Only managers and administrators have the approve and reject buttons.</span></span>

<span data-ttu-id="02ebc-121">次の図の`admin@contoso.com`と管理者の役割では署名されています。</span><span class="sxs-lookup"><span data-stu-id="02ebc-121">In the following image, `admin@contoso.com` is signed in and in the administrator’s role.</span></span> 

![上記で説明したイメージ](secure-data/_static/admin.png)

<span data-ttu-id="02ebc-123">管理者は、すべての特権を持ちます。</span><span class="sxs-lookup"><span data-stu-id="02ebc-123">The administrator has all privileges.</span></span> <span data-ttu-id="02ebc-124">彼女は、読み取り/編集/削除のいずれかの連絡先をことができ、連絡先の状態を変更します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-124">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="02ebc-125">アプリがによって作成された[スキャフォールディング](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)次`Contact`モデル。</span><span class="sxs-lookup"><span data-stu-id="02ebc-125">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)  the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="02ebc-126">A`ContactIsOwnerAuthorizationHandler`承認ハンドラーにより、ユーザーがそのデータを編集できるのみです。</span><span class="sxs-lookup"><span data-stu-id="02ebc-126">A `ContactIsOwnerAuthorizationHandler` authorization handler ensures that a user can only edit their data.</span></span> <span data-ttu-id="02ebc-127">A`ContactManagerAuthorizationHandler`承認ハンドラーは、承認または却下の連絡先のマネージャーを使用します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-127">A `ContactManagerAuthorizationHandler` authorization handler allows managers to approve or reject contacts.</span></span>  <span data-ttu-id="02ebc-128">A`ContactAdministratorsAuthorizationHandler`承認ハンドラーは、管理者の承認または拒否の連絡先を編集/削除の連絡先を使用します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-128">A `ContactAdministratorsAuthorizationHandler` authorization handler allows administrators to approve or reject contacts and to edit/delete contacts.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="02ebc-129">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="02ebc-129">Prerequisites</span></span>

<span data-ttu-id="02ebc-130">これは最初のチュートリアルではありません。</span><span class="sxs-lookup"><span data-stu-id="02ebc-130">This is not a beginning tutorial.</span></span> <span data-ttu-id="02ebc-131">理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="02ebc-131">You should be familiar with:</span></span>

* [<span data-ttu-id="02ebc-132">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="02ebc-132">ASP.NET Core MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="02ebc-133">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="02ebc-133">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="02ebc-134">Starter および完成したアプリ</span><span class="sxs-lookup"><span data-stu-id="02ebc-134">The starter and completed app</span></span>

<span data-ttu-id="02ebc-135">[ダウンロード](xref:tutorials/index#how-to-download-a-sample)、[完了](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final)アプリ。</span><span class="sxs-lookup"><span data-stu-id="02ebc-135">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app.</span></span> <span data-ttu-id="02ebc-136">[テスト](#test-the-completed-app)完成したアプリのセキュリティ機能を理解しておくようにします。</span><span class="sxs-lookup"><span data-stu-id="02ebc-136">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span> 

### <a name="the-starter-app"></a><span data-ttu-id="02ebc-137">スターター アプリ</span><span class="sxs-lookup"><span data-stu-id="02ebc-137">The starter app</span></span>

<span data-ttu-id="02ebc-138">完成したサンプル コードを比較することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="02ebc-138">It's helpful to compare your code with the completed sample.</span></span>

<span data-ttu-id="02ebc-139">[ダウンロード](xref:tutorials/index#how-to-download-a-sample)、[スターター](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter)アプリ。</span><span class="sxs-lookup"><span data-stu-id="02ebc-139">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app.</span></span> 

<span data-ttu-id="02ebc-140">参照してください[スターター アプリを作成する](#create-the-starter-app)をゼロから作成したい場合。</span><span class="sxs-lookup"><span data-stu-id="02ebc-140">See [Create the starter app](#create-the-starter-app) if you'd like to create it from scratch.</span></span>

<span data-ttu-id="02ebc-141">データベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-141">Update the database:</span></span>

```none
   dotnet ef database update
```

<span data-ttu-id="02ebc-142">アプリを実行する、タップ、 **ContactManager**リンク、および作成、編集、および連絡先を削除することを確認します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-142">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

<span data-ttu-id="02ebc-143">このチュートリアルでは、すべての主要な手順、セキュリティで保護されたユーザー データのアプリを作成するには。</span><span class="sxs-lookup"><span data-stu-id="02ebc-143">This tutorial has all the major steps to create the secure user data app.</span></span> <span data-ttu-id="02ebc-144">完成したプロジェクトを参照すると便利です。</span><span class="sxs-lookup"><span data-stu-id="02ebc-144">You may find it helpful to refer to the completed project.</span></span>

## <a name="modify-the-app-to-secure-user-data"></a><span data-ttu-id="02ebc-145">ユーザー データを保護するアプリを変更します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-145">Modify the app to secure user data</span></span>

<span data-ttu-id="02ebc-146">次のセクションでは、セキュリティで保護されたユーザー データのアプリを作成するすべての主要な手順があります。</span><span class="sxs-lookup"><span data-stu-id="02ebc-146">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="02ebc-147">完成したプロジェクトを参照すると便利です。</span><span class="sxs-lookup"><span data-stu-id="02ebc-147">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="02ebc-148">ユーザーに連絡先データを関連付ける</span><span class="sxs-lookup"><span data-stu-id="02ebc-148">Tie the contact data to the user</span></span>

<span data-ttu-id="02ebc-149">ASP.NET を使用して[Identity](xref:security/authentication/identity)のユーザーのユーザー ID を編集できますが、そのデータを他のユーザー データは表示されません。</span><span class="sxs-lookup"><span data-stu-id="02ebc-149">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="02ebc-150">追加`OwnerID`を`Contact`モデル。</span><span class="sxs-lookup"><span data-stu-id="02ebc-150">Add `OwnerID` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

<span data-ttu-id="02ebc-151">`OwnerID`ユーザーの id、`AspNetUser`テーブルに、 [Identity](xref:security/authentication/identity)データベース。</span><span class="sxs-lookup"><span data-stu-id="02ebc-151">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="02ebc-152">`Status`フィールドは、連絡先が一般のユーザーによって表示可能であるかどうか。</span><span class="sxs-lookup"><span data-stu-id="02ebc-152">The `Status` field determines if a contact is viewable by general users.</span></span> 

<span data-ttu-id="02ebc-153">新しい移行をスキャフォールディングして、データベースの更新します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-153">Scaffold a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="02ebc-154">SSL と認証済みユーザーが必要</span><span class="sxs-lookup"><span data-stu-id="02ebc-154">Require SSL and authenticated users</span></span>

<span data-ttu-id="02ebc-155">`ConfigureServices`のメソッド、 *Startup.cs*ファイルに追加し、 [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute)承認フィルター。</span><span class="sxs-lookup"><span data-stu-id="02ebc-155">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

<span data-ttu-id="02ebc-156">HTTP 要求を HTTPS にリダイレクトするを参照してください。 [URL 書き換えミドルウェア](xref:fundamentals/url-rewriting)です。</span><span class="sxs-lookup"><span data-stu-id="02ebc-156">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="02ebc-157">Visual Studio のコードを使用してまたはローカルのプラットフォームでテストする場合は、テスト証明書を SSL の含まれていません。</span><span class="sxs-lookup"><span data-stu-id="02ebc-157">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="02ebc-158">設定`"LocalTest:skipSSL": true`で、*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="02ebc-158">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="02ebc-159">ユーザーが認証されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="02ebc-159">Require authenticated users</span></span>

<span data-ttu-id="02ebc-160">ユーザー認証を必要とする既定の認証ポリシーを設定します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-160">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="02ebc-161">選択を解除とコント ローラーまたはアクション メソッドに認証する、`[AllowAnonymous]`属性。</span><span class="sxs-lookup"><span data-stu-id="02ebc-161">You can opt out of authentication at the controller or action method with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="02ebc-162">この方法で、新しいコント ローラーを追加が自動的に認証が必要に含める新しいコント ローラーに依存するよりも安全である、`[Authorize]`属性。</span><span class="sxs-lookup"><span data-stu-id="02ebc-162">With this approach, any new controllers added will automatically require authentication, which is safer than relying on new controllers to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="02ebc-163">次の追加、`ConfigureServices`のメソッド、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="02ebc-163">Add the following to  the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

<span data-ttu-id="02ebc-164">追加`[AllowAnonymous]`home コント ローラーを登録する前に、匿名ユーザーはサイトに関する情報を取得できるようにします。</span><span class="sxs-lookup"><span data-stu-id="02ebc-164">Add `[AllowAnonymous]` to the home controller so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="02ebc-165">テスト アカウントを構成します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-165">Configure the test account</span></span>

<span data-ttu-id="02ebc-166">`SeedData`クラスは、2 つのアカウント、管理者およびマネージャーを作成します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-166">The `SeedData` class creates two accounts,  administrator and manager.</span></span> <span data-ttu-id="02ebc-167">使用して、[シークレット マネージャー ツール](xref:security/app-secrets)これらのアカウントのパスワードを設定します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-167">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="02ebc-168">これは、プロジェクト ディレクトリから実行 (ディレクトリを含む*Program.cs*)。</span><span class="sxs-lookup"><span data-stu-id="02ebc-168">Do this from the project directory (the directory containing *Program.cs*).</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="02ebc-169">更新`Configure`テスト パスワードを使用します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-169">Update `Configure` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

<span data-ttu-id="02ebc-170">管理者のユーザー ID を追加し、`Status = ContactStatus.Approved`連絡先にします。</span><span class="sxs-lookup"><span data-stu-id="02ebc-170">Add the administrator user ID and `Status = ContactStatus.Approved` to the contacts.</span></span> <span data-ttu-id="02ebc-171">1 人だけが表示されると、すべての連絡先にユーザー ID を追加します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-171">Only one contact is shown, add the user ID to all contacts:</span></span>

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="02ebc-172">所有者、マネージャー、および管理者の承認のハンドラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-172">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="02ebc-173">作成、`ContactIsOwnerAuthorizationHandler`クラス内で、*承認*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="02ebc-173">Create a `ContactIsOwnerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="02ebc-174">`ContactIsOwnerAuthorizationHandler`はリソースで動作しているユーザーは、リソースを所有していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-174">The `ContactIsOwnerAuthorizationHandler` will verify the user acting on the resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="02ebc-175">`ContactIsOwnerAuthorizationHandler`呼び出し`context.Succeed`現在の認証済みユーザーがメンバーの所有者である場合。</span><span class="sxs-lookup"><span data-stu-id="02ebc-175">The `ContactIsOwnerAuthorizationHandler` calls `context.Succeed` if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="02ebc-176">認証ハンドラーが通常返す`context.Succeed`要件を満たしている場合にします。</span><span class="sxs-lookup"><span data-stu-id="02ebc-176">Authorization handlers generally return `context.Succeed` when the requirements are met.</span></span> <span data-ttu-id="02ebc-177">返される`Task.FromResult(0)`要件が満たされないときにします。</span><span class="sxs-lookup"><span data-stu-id="02ebc-177">They return `Task.FromResult(0)` when requirements are not met.</span></span> <span data-ttu-id="02ebc-178">`Task.FromResult(0)`成功または失敗のどちらもその他の認証ハンドラーを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="02ebc-178">`Task.FromResult(0)` is neither success or failure, it allows other authorization handler to run.</span></span> <span data-ttu-id="02ebc-179">明示的に失敗する場合は、返す`context.Fail()`です。</span><span class="sxs-lookup"><span data-stu-id="02ebc-179">If you need to explicitly fail, return `context.Fail()`.</span></span>

<span data-ttu-id="02ebc-180">ここには、ため、要件のパラメーターに渡す操作を確認する必要はありませんに自分のデータを編集/削除する連絡先の所有者ができるようにします。</span><span class="sxs-lookup"><span data-stu-id="02ebc-180">We allow contact owners to edit/delete their own data, so we don't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="02ebc-181">マネージャーの認証ハンドラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-181">Create a manager authorization handler</span></span>

<span data-ttu-id="02ebc-182">作成、`ContactManagerAuthorizationHandler`クラス内で、*承認*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="02ebc-182">Create a `ContactManagerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="02ebc-183">`ContactManagerAuthorizationHandler`リソースに対して機能しているユーザーは、マネージャーが確認されます。</span><span class="sxs-lookup"><span data-stu-id="02ebc-183">The `ContactManagerAuthorizationHandler` will verify the user acting on the resource is a manager.</span></span> <span data-ttu-id="02ebc-184">マネージャーだけでは、承認したり、コンテンツの変更 (新しいまたは変更された) を拒否することができます。</span><span class="sxs-lookup"><span data-stu-id="02ebc-184">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="02ebc-185">管理者認証ハンドラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-185">Create an administrator authorization handler</span></span>

<span data-ttu-id="02ebc-186">作成、`ContactAdministratorsAuthorizationHandler`クラス内で、*承認*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="02ebc-186">Create a `ContactAdministratorsAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="02ebc-187">`ContactAdministratorsAuthorizationHandler`リソースに対して機能しているユーザーは管理者が確認されます。</span><span class="sxs-lookup"><span data-stu-id="02ebc-187">The `ContactAdministratorsAuthorizationHandler` will verify the user acting on the resource is a administrator.</span></span> <span data-ttu-id="02ebc-188">管理者は、すべての操作を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="02ebc-188">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="02ebc-189">認証ハンドラーを登録します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-189">Register the authorization handlers</span></span>

<span data-ttu-id="02ebc-190">Entity Framework のコアを使用してサービスを登録する必要があります[依存性の注入](xref:fundamentals/dependency-injection)を使用して[AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)です。</span><span class="sxs-lookup"><span data-stu-id="02ebc-190">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="02ebc-191">`ContactIsOwnerAuthorizationHandler` ASP.NET Core を使用して[Identity](xref:security/authentication/identity)、これは Entity Framework Core 上に構築します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-191">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="02ebc-192">ハンドラー コレクションに登録サービスをで使用できるように、`ContactsController`を通じて[依存性の注入](xref:fundamentals/dependency-injection)です。</span><span class="sxs-lookup"><span data-stu-id="02ebc-192">Register the handlers with the service collection so they will be available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="02ebc-193">末尾に次のコードを追加`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="02ebc-193">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

<span data-ttu-id="02ebc-194">`ContactAdministratorsAuthorizationHandler`および`ContactManagerAuthorizationHandler`シングルトンとして追加されます。</span><span class="sxs-lookup"><span data-stu-id="02ebc-194">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="02ebc-195">これらはシングルトンの EF を使用していないし、では、必要なすべての情報が、`Context`のパラメーター、`HandleRequirementAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="02ebc-195">They are singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

<span data-ttu-id="02ebc-196">完全な`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="02ebc-196">The complete `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a><span data-ttu-id="02ebc-197">承認をサポートするコードを更新します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-197">Update the code to support authorization</span></span>

<span data-ttu-id="02ebc-198">このセクションでは、コント ローラーとビューを更新し、操作の要件クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-198">In this section, you update the controller and views and add an operations requirements class.</span></span>

### <a name="update-the-contacts-controller"></a><span data-ttu-id="02ebc-199">連絡先のコント ローラーを更新します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-199">Update the Contacts controller</span></span>

<span data-ttu-id="02ebc-200">更新プログラム、`ContactsController`コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="02ebc-200">Update the `ContactsController` constructor:</span></span>

* <span data-ttu-id="02ebc-201">追加、`IAuthorizationService`承認ハンドラーへのアクセスをサービスします。</span><span class="sxs-lookup"><span data-stu-id="02ebc-201">Add the `IAuthorizationService` service to  access to the authorization handlers.</span></span> 
* <span data-ttu-id="02ebc-202">追加、 `Identity` `UserManager`サービス。</span><span class="sxs-lookup"><span data-stu-id="02ebc-202">Add the `Identity` `UserManager` service:</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a><span data-ttu-id="02ebc-203">連絡先の操作の要件のクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-203">Add a contact operations requirements class</span></span>

<span data-ttu-id="02ebc-204">追加、`ContactOperations`クラスを*承認*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="02ebc-204">Add the `ContactOperations` class to the *Authorization* folder.</span></span> <span data-ttu-id="02ebc-205">このクラスは、要件を含む、アプリケーションがサポートします。</span><span class="sxs-lookup"><span data-stu-id="02ebc-205">This class  contain the requirements our app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a><span data-ttu-id="02ebc-206">更新プログラムを作成します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-206">Update Create</span></span>

<span data-ttu-id="02ebc-207">更新プログラム、`HTTP POST Create`メソッド。</span><span class="sxs-lookup"><span data-stu-id="02ebc-207">Update the `HTTP POST Create` method to:</span></span>

* <span data-ttu-id="02ebc-208">ユーザー ID を追加、`Contact`モデル。</span><span class="sxs-lookup"><span data-stu-id="02ebc-208">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="02ebc-209">ユーザーに連絡先を所有していることを確認する認証ハンドラーを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-209">Call the authorization handler to verify the user owns the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a><span data-ttu-id="02ebc-210">更新プログラムの編集</span><span class="sxs-lookup"><span data-stu-id="02ebc-210">Update Edit</span></span>

<span data-ttu-id="02ebc-211">両方を更新`Edit`ユーザーを確認する認証ハンドラーを使用する方法を連絡先を所有しています。</span><span class="sxs-lookup"><span data-stu-id="02ebc-211">Update both `Edit` methods to use the authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="02ebc-212">リソースの承認を実行しているために使用できません、`[Authorize]`属性。</span><span class="sxs-lookup"><span data-stu-id="02ebc-212">Because we are performing resource authorization we cannot use the `[Authorize]` attribute.</span></span> <span data-ttu-id="02ebc-213">属性が評価されるときに、リソースへのアクセスはありません。</span><span class="sxs-lookup"><span data-stu-id="02ebc-213">We don't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="02ebc-214">リソース ベースの承認は、命令型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="02ebc-214">Resource based authorization must be imperative.</span></span> <span data-ttu-id="02ebc-215">コント ローラーに読み込むか、ハンドラー自体内で読み込むことにより、リソースへのアクセスを取得したら、チェックを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="02ebc-215">Checks must be performed once we have access to the resource, either by loading it in our controller, or by loading it within the handler itself.</span></span> <span data-ttu-id="02ebc-216">頻繁には、リソースをリソース キーを渡すことによってアクセスします。</span><span class="sxs-lookup"><span data-stu-id="02ebc-216">Frequently you will access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a><span data-ttu-id="02ebc-217">Update、Delete メソッド</span><span class="sxs-lookup"><span data-stu-id="02ebc-217">Update the Delete method</span></span>

<span data-ttu-id="02ebc-218">両方を更新`Delete`ユーザーを確認する認証ハンドラーを使用する方法を連絡先を所有しています。</span><span class="sxs-lookup"><span data-stu-id="02ebc-218">Update both `Delete` methods to use the authorization handler to verify the user owns the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="02ebc-219">承認サービスをビューに挿入します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-219">Inject the authorization service into the views</span></span>

<span data-ttu-id="02ebc-220">現在、UI に示すは編集し、ユーザーが変更できないデータへのリンクを削除します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-220">Currently the UI shows edit and delete links for data the user cannot modify.</span></span> <span data-ttu-id="02ebc-221">認証ハンドラーをビューに適用することによって修正されます。</span><span class="sxs-lookup"><span data-stu-id="02ebc-221">We'll fix that by applying the authorization handler to the views.</span></span>

<span data-ttu-id="02ebc-222">承認サービスでの挿入、 *Views/_ViewImports.cshtml*ファイルのすべてのビューに利用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="02ebc-222">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it will be available to all views:</span></span>

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

<span data-ttu-id="02ebc-223">更新プログラム、 *Views/Contacts/Index.cshtml* Razor ビューのみを表示、編集し編集/削除の連絡先がユーザー用のリンクを削除します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-223">Update the *Views/Contacts/Index.cshtml* Razor view to only display the edit and delete links for users who can edit/delete the contact.</span></span>

<span data-ttu-id="02ebc-224">追加`@using ContactManager.Authorization;`</span><span class="sxs-lookup"><span data-stu-id="02ebc-224">Add `@using ContactManager.Authorization;`</span></span>

<span data-ttu-id="02ebc-225">更新プログラム、`Edit`と`Delete`リンクを編集および連絡先を削除するアクセス許可を持つユーザーのみレンダリングされるようにします。</span><span class="sxs-lookup"><span data-stu-id="02ebc-225">Update the `Edit` and `Delete` links so they are only rendered for users with permission to edit and delete the contact.</span></span>

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

<span data-ttu-id="02ebc-226">警告: データ編集または削除するアクセス許可がないユーザーからのリンクを非表示にしても、アプリは保護しません。</span><span class="sxs-lookup"><span data-stu-id="02ebc-226">Warning: Hiding links from users that do not have permission to edit or delete data does not secure the app.</span></span> <span data-ttu-id="02ebc-227">リンクを非表示にするはアプリよりユーザー フレンドリによって唯一の有効なリンクを表示します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-227">Hiding links makes the app more user friendly by displaying only valid links.</span></span> <span data-ttu-id="02ebc-228">ユーザーは、編集を呼び出すし、自分が所有しないデータの操作を削除するには、生成された Url を切断できます。</span><span class="sxs-lookup"><span data-stu-id="02ebc-228">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span>  <span data-ttu-id="02ebc-229">コント ローラーは、アクセスをセキュリティで保護するチェックを繰り返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="02ebc-229">The controller must repeat the access checks to be secure.</span></span>

### <a name="update-the-details-view"></a><span data-ttu-id="02ebc-230">更新プログラムの詳細ビュー</span><span class="sxs-lookup"><span data-stu-id="02ebc-230">Update the Details view</span></span>

<span data-ttu-id="02ebc-231">マネージャーが承認または連絡先を拒否するため、詳細ビューを更新します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-231">Update the details view so managers can approve or reject contacts:</span></span>

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a><span data-ttu-id="02ebc-232">完成したアプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="02ebc-232">Test the completed app</span></span>

<span data-ttu-id="02ebc-233">Visual Studio のコードを使用してまたはローカルのプラットフォームでテストする場合は、テスト証明書を SSL の含まれていません。</span><span class="sxs-lookup"><span data-stu-id="02ebc-233">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="02ebc-234">設定`"LocalTest:skipSSL": true`で、*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="02ebc-234">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

<span data-ttu-id="02ebc-235">アプリを実行した連絡先がある場合は、内のすべてのレコードを削除、`Contact`テーブルが表示され、データベースのシードにアプリを再起動します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-235">If you have run the app and have contacts, delete all the records in the `Contact` table and restart the app to seed the database.</span></span> <span data-ttu-id="02ebc-236">Visual Studio を使用している場合は、終了して、データベースのシードに IIS Express を再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="02ebc-236">If you are using Visual Studio, you need to exit and restart IIS Express to seed the database.</span></span>

<span data-ttu-id="02ebc-237">連絡先を参照するユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-237">Register a user to browse the contacts.</span></span>

<span data-ttu-id="02ebc-238">完成したアプリをテストする簡単な方法では、3 つの異なるブラウザー (または incognito/inprivate ブラウズのバージョン) を起動します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-238">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="02ebc-239">1 つのブラウザーで新しいユーザーの例では、登録`test@example.com`です。</span><span class="sxs-lookup"><span data-stu-id="02ebc-239">In one browser, register a new user, for example, `test@example.com`.</span></span> <span data-ttu-id="02ebc-240">別のユーザーに各ブラウザーにサインインします。</span><span class="sxs-lookup"><span data-stu-id="02ebc-240">Sign in to each browser with a different user.</span></span> <span data-ttu-id="02ebc-241">次のことを検証します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-241">Verify the following:</span></span>

* <span data-ttu-id="02ebc-242">登録済みのユーザーには、承認されているすべての連絡先データを表示できます。</span><span class="sxs-lookup"><span data-stu-id="02ebc-242">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="02ebc-243">登録済みユーザーを編集/削除が独自のデータ。</span><span class="sxs-lookup"><span data-stu-id="02ebc-243">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="02ebc-244">管理者では、承認したり、連絡先データを拒否することができます。</span><span class="sxs-lookup"><span data-stu-id="02ebc-244">Managers can approve or reject contact data.</span></span> <span data-ttu-id="02ebc-245">`Details`ビューには、**承認**と**拒否**ボタン。</span><span class="sxs-lookup"><span data-stu-id="02ebc-245">The `Details` view shows **Approve** and **Reject** buttons.</span></span> 
* <span data-ttu-id="02ebc-246">および管理者が承認または却下とすべてのデータを編集/削除します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-246">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="02ebc-247">ユーザー</span><span class="sxs-lookup"><span data-stu-id="02ebc-247">User</span></span>| <span data-ttu-id="02ebc-248">オプション</span><span class="sxs-lookup"><span data-stu-id="02ebc-248">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="02ebc-249">データの編集、または削除できます。</span><span class="sxs-lookup"><span data-stu-id="02ebc-249">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="02ebc-250">承認または拒否し、編集/削除を所有できるデータ</span><span class="sxs-lookup"><span data-stu-id="02ebc-250">Can approve/reject and edit/delete own data</span></span>  |
| admin@contoso.com | <span data-ttu-id="02ebc-251">編集/削除してすべてのデータの承認または却下</span><span class="sxs-lookup"><span data-stu-id="02ebc-251">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="02ebc-252">管理者のブラウザーで連絡先を作成します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-252">Create a contact in the administrators browser.</span></span> <span data-ttu-id="02ebc-253">削除の URL をコピーし、管理者の連絡先から編集します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-253">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="02ebc-254">これらのリンクをテスト ユーザーは、これらの操作を実行できないことを確認するテスト ユーザーのブラウザーに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="02ebc-254">Paste these links into the test user's browser to verify the test user cannot perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="02ebc-255">スターター アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-255">Create the starter app</span></span>

<span data-ttu-id="02ebc-256">スタート アプリケーションを作成する手順に従います。</span><span class="sxs-lookup"><span data-stu-id="02ebc-256">Follow these instructions to create the starter app.</span></span>

* <span data-ttu-id="02ebc-257">作成、 **ASP.NET Core Web アプリケーション**を使用して[Visual Studio 2017](https://www.visualstudio.com/) "ContactManager"という名前</span><span class="sxs-lookup"><span data-stu-id="02ebc-257">Create an **ASP.NET Core Web Application** using [Visual Studio 2017](https://www.visualstudio.com/) named "ContactManager"</span></span>

  * <span data-ttu-id="02ebc-258">使用してアプリを作成する**個々 のユーザー アカウント**です。</span><span class="sxs-lookup"><span data-stu-id="02ebc-258">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="02ebc-259">名前を付けます"ContactManager"名前空間は、名前空間で使用するサンプルと一致するようにします。</span><span class="sxs-lookup"><span data-stu-id="02ebc-259">Name it "ContactManager" so your namespace will match the namespace use in the sample.</span></span>

* <span data-ttu-id="02ebc-260">次の追加`Contact`モデル。</span><span class="sxs-lookup"><span data-stu-id="02ebc-260">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="02ebc-261">Scaffold、`Contact`エンティティ フレームワークのコアを使用するモデルと`ApplicationDbContext`データ コンテキスト。</span><span class="sxs-lookup"><span data-stu-id="02ebc-261">Scaffold the `Contact` model using Entity Framework Core and the `ApplicationDbContext` data context.</span></span> <span data-ttu-id="02ebc-262">すべてのスキャフォールディング既定値をそのまま使用します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-262">Accept all the scaffolding defaults.</span></span> <span data-ttu-id="02ebc-263">使用して`ApplicationDbContext`データ コンテキストのクラス、contact テーブルに、 [Identity](xref:security/authentication/identity)データベース。</span><span class="sxs-lookup"><span data-stu-id="02ebc-263">Using `ApplicationDbContext` for the data context class  puts the contact table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="02ebc-264">参照してください[モデルを追加する](xref:tutorials/first-mvc-app/adding-model)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="02ebc-264">See [Adding a model](xref:tutorials/first-mvc-app/adding-model) for more information.</span></span>

* <span data-ttu-id="02ebc-265">更新プログラム、 **ContactManager**でアンカー、 *Views/Shared/_Layout.cshtml*ファイルから`asp-controller="Home"`に`asp-controller="Contacts"`ためをタップすると、 **ContactManager**リンク連絡先コント ローラーを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-265">Update the **ContactManager** anchor in the *Views/Shared/_Layout.cshtml* file from `asp-controller="Home"` to `asp-controller="Contacts"` so tapping the **ContactManager** link will invoke the Contacts controller.</span></span> <span data-ttu-id="02ebc-266">元のマークアップ。</span><span class="sxs-lookup"><span data-stu-id="02ebc-266">The original markup:</span></span>

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

<span data-ttu-id="02ebc-267">更新されたマークアップ:</span><span class="sxs-lookup"><span data-stu-id="02ebc-267">The updated markup:</span></span>

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* <span data-ttu-id="02ebc-268">初期の移行をスキャフォールディングして、データベースの更新</span><span class="sxs-lookup"><span data-stu-id="02ebc-268">Scaffold the initial migration and update the database</span></span>

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* <span data-ttu-id="02ebc-269">アプリをテストするには、作成、編集、および連絡先を削除します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-269">Test the app by creating, editing and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="02ebc-270">データベースのシード</span><span class="sxs-lookup"><span data-stu-id="02ebc-270">Seed the database</span></span>

<span data-ttu-id="02ebc-271">追加、`SeedData`クラスを*データ*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="02ebc-271">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="02ebc-272">サンプル、ダウンロードした場合は、コピー、 *SeedData.cs*ファイルの名前を*データ*のスタート プロジェクトのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="02ebc-272">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

<span data-ttu-id="02ebc-273">末尾に強調表示されたコードを追加、`Configure`メソッドで、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="02ebc-273">Add the highlighted code to the end of the `Configure` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

<span data-ttu-id="02ebc-274">アプリに、データベースがシード処理をテストします。</span><span class="sxs-lookup"><span data-stu-id="02ebc-274">Test that the app seeded the database.</span></span> <span data-ttu-id="02ebc-275">Seed メソッドは、DB の連絡先のすべての行がある場合に実行されません。</span><span class="sxs-lookup"><span data-stu-id="02ebc-275">The seed method does not run if there are any rows in the contact DB.</span></span>

### <a name="create-a-class-used-in-the-tutorial"></a><span data-ttu-id="02ebc-276">このチュートリアルで使用されるクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-276">Create a class used in the tutorial</span></span>

* <span data-ttu-id="02ebc-277">という名前のフォルダーを作成する*承認*です。</span><span class="sxs-lookup"><span data-stu-id="02ebc-277">Create a folder named *Authorization*.</span></span>
* <span data-ttu-id="02ebc-278">コピー、 *Authorization\ContactOperations.cs*完成したプロジェクトのダウンロードからファイルまたは次のコードをコピーします。</span><span class="sxs-lookup"><span data-stu-id="02ebc-278">Copy the *Authorization\ContactOperations.cs* file from the completed project download, or copy the following code:</span></span>

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="02ebc-279">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="02ebc-279">Additional resources</span></span>

* <span data-ttu-id="02ebc-280">[ASP.NET Core 承認ラボ](https://github.com/blowdart/AspNetAuthorizationWorkshop)です。</span><span class="sxs-lookup"><span data-stu-id="02ebc-280">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="02ebc-281">このラボでは、このチュートリアルで導入されたセキュリティ機能の詳細に移動します。</span><span class="sxs-lookup"><span data-stu-id="02ebc-281">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="02ebc-282">ASP.NET Core での承認: 単純な場合、ロール、および信頼性情報ベースのカスタム</span><span class="sxs-lookup"><span data-stu-id="02ebc-282">Authorization in ASP.NET Core : Simple, role, claims-based and custom</span></span>](index.md)
* [<span data-ttu-id="02ebc-283">カスタム ポリシー ベースの承認</span><span class="sxs-lookup"><span data-stu-id="02ebc-283">Custom policy-based authorization</span></span>](policies.md)
