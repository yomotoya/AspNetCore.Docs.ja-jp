---
title: 認証によって保護されているユーザー データと ASP.NET Core アプリケーションを作成します。
author: rick-anderson
description: 認証によって保護されているユーザー データと Razor ページ アプリケーションを作成する方法を説明します。 HTTPS、認証、セキュリティ、ASP.NET Core Id が含まれます。
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: 36475776966cfb0cb3bb40477798f6e24df9725d
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34688453"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="63509-104">認証によって保護されているユーザー データと ASP.NET Core アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="63509-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="63509-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="63509-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="63509-106">このチュートリアルでは、認証によって保護されているユーザー データと ASP.NET Core web アプリを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="63509-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="63509-107">(登録済み) のユーザーを認証した連絡先の一覧を表示を作成します。</span><span class="sxs-lookup"><span data-stu-id="63509-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="63509-108">これには次の 3 つのセキュリティ グループがあります。</span><span class="sxs-lookup"><span data-stu-id="63509-108">There are three security groups:</span></span>

* <span data-ttu-id="63509-109">**ユーザーが登録されている**承認済みのすべてのデータを表示することができ、自分のデータ編集、または削除できます。</span><span class="sxs-lookup"><span data-stu-id="63509-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="63509-110">**マネージャー**保護者の連絡先データを拒否することができます。</span><span class="sxs-lookup"><span data-stu-id="63509-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="63509-111">許可されている連絡先のみがユーザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="63509-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="63509-112">**管理者**承認または拒否でき、すべてのデータを編集/削除します。</span><span class="sxs-lookup"><span data-stu-id="63509-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="63509-113">次の図では、ユーザー Rick (`rick@example.com`) 署名します。</span><span class="sxs-lookup"><span data-stu-id="63509-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="63509-114">Rick は許可されている連絡先だけを表示でき、**編集**/**削除**/**新規作成**連絡先へのリンク。</span><span class="sxs-lookup"><span data-stu-id="63509-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="63509-115">Rick、ディスプレイによってのみ、最後のレコードが作成された**編集**と**削除**リンクします。</span><span class="sxs-lookup"><span data-stu-id="63509-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="63509-116">その他のユーザーは、マネージャーまたは管理者は、ステータスが"Approved"に変更されるまで、最後のレコードを表示されません。</span><span class="sxs-lookup"><span data-stu-id="63509-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![イメージは、前に説明されています。](secure-data/_static/rick.png)

<span data-ttu-id="63509-118">次の図の`manager@contoso.com`とマネージャーの役割では署名します。</span><span class="sxs-lookup"><span data-stu-id="63509-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![イメージは、前に説明されています。](secure-data/_static/manager1.png)

<span data-ttu-id="63509-120">次の図は、連絡先の詳細ビューをマネージャーには。</span><span class="sxs-lookup"><span data-stu-id="63509-120">The following image shows the managers details view of a contact:</span></span>

![イメージは、前に説明されています。](secure-data/_static/manager.png)

<span data-ttu-id="63509-122">**承認**と**拒否**ボタンは、管理者、および管理者のみ表示されます。</span><span class="sxs-lookup"><span data-stu-id="63509-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="63509-123">次の図の`admin@contoso.com`では、管理者ロールでは署名します。</span><span class="sxs-lookup"><span data-stu-id="63509-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![イメージは、前に説明されています。](secure-data/_static/admin.png)

<span data-ttu-id="63509-125">管理者は、すべての特権を持ちます。</span><span class="sxs-lookup"><span data-stu-id="63509-125">The administrator has all privileges.</span></span> <span data-ttu-id="63509-126">彼女は、読み取り/編集/削除のいずれかの連絡先をことができ、連絡先の状態を変更します。</span><span class="sxs-lookup"><span data-stu-id="63509-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="63509-127">アプリがによって作成された[スキャフォールディング](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)次`Contact`モデル。</span><span class="sxs-lookup"><span data-stu-id="63509-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="63509-128">このサンプルには、次の認証ハンドラーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="63509-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="63509-129">`ContactIsOwnerAuthorizationHandler`: ユーザーが、データを編集のみできることを確認します。</span><span class="sxs-lookup"><span data-stu-id="63509-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="63509-130">`ContactManagerAuthorizationHandler`: を承認または拒否の連絡先のマネージャーをできるようにします。</span><span class="sxs-lookup"><span data-stu-id="63509-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="63509-131">`ContactAdministratorsAuthorizationHandler`: 管理者の連絡先を編集/削除し、承認または連絡先を拒否するようにします。</span><span class="sxs-lookup"><span data-stu-id="63509-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="63509-132">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="63509-132">Prerequisites</span></span>

<span data-ttu-id="63509-133">このチュートリアルを進められます。</span><span class="sxs-lookup"><span data-stu-id="63509-133">This tutorial is advanced.</span></span> <span data-ttu-id="63509-134">理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="63509-134">You should be familiar with:</span></span>

* [<span data-ttu-id="63509-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="63509-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="63509-136">認証</span><span class="sxs-lookup"><span data-stu-id="63509-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="63509-137">アカウントの確認とパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="63509-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="63509-138">承認</span><span class="sxs-lookup"><span data-stu-id="63509-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="63509-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="63509-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="63509-140">参照してください[この PDF ファイル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf)ASP.NET Core MVC のバージョンについてはします。</span><span class="sxs-lookup"><span data-stu-id="63509-140">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="63509-141">このチュートリアルの ASP.NET Core の 1.1 バージョンでは[この](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data)フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="63509-141">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="63509-142">ASP.NET Core サンプルでは、1.1、[サンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)です。</span><span class="sxs-lookup"><span data-stu-id="63509-142">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="63509-143">Starter および完成したアプリ</span><span class="sxs-lookup"><span data-stu-id="63509-143">The starter and completed app</span></span>

<span data-ttu-id="63509-144">[ダウンロード](xref:tutorials/index#how-to-download-a-sample)、[完了](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)アプリ。</span><span class="sxs-lookup"><span data-stu-id="63509-144">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="63509-145">[テスト](#test-the-completed-app)完成したアプリのセキュリティ機能を理解しておくようにします。</span><span class="sxs-lookup"><span data-stu-id="63509-145">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="63509-146">スターター アプリ</span><span class="sxs-lookup"><span data-stu-id="63509-146">The starter app</span></span>

<span data-ttu-id="63509-147">[ダウンロード](xref:tutorials/index#how-to-download-a-sample)、[スターター](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2)アプリ。</span><span class="sxs-lookup"><span data-stu-id="63509-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="63509-148">アプリを実行する、タップ、 **ContactManager**リンク、および作成、編集、および連絡先を削除することを確認します。</span><span class="sxs-lookup"><span data-stu-id="63509-148">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="63509-149">ユーザー データを保護します。</span><span class="sxs-lookup"><span data-stu-id="63509-149">Secure user data</span></span>

<span data-ttu-id="63509-150">次のセクションでは、セキュリティで保護されたユーザー データのアプリを作成するすべての主要な手順があります。</span><span class="sxs-lookup"><span data-stu-id="63509-150">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="63509-151">完成したプロジェクトを参照すると便利です。</span><span class="sxs-lookup"><span data-stu-id="63509-151">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="63509-152">ユーザーに連絡先データを関連付ける</span><span class="sxs-lookup"><span data-stu-id="63509-152">Tie the contact data to the user</span></span>

<span data-ttu-id="63509-153">ASP.NET を使用して[Identity](xref:security/authentication/identity)のユーザーのユーザー ID を編集できますが、そのデータを他のユーザー データは表示されません。</span><span class="sxs-lookup"><span data-stu-id="63509-153">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="63509-154">追加`OwnerID`と`ContactStatus`を`Contact`モデル。</span><span class="sxs-lookup"><span data-stu-id="63509-154">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="63509-155">`OwnerID` ユーザーの id、`AspNetUser`テーブルに、 [Identity](xref:security/authentication/identity)データベース。</span><span class="sxs-lookup"><span data-stu-id="63509-155">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="63509-156">`Status`フィールドは、連絡先が一般のユーザーによって表示可能であるかどうか。</span><span class="sxs-lookup"><span data-stu-id="63509-156">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="63509-157">新しい移行を作成し、データベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="63509-157">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-https-and-authenticated-users"></a><span data-ttu-id="63509-158">HTTPS と認証済みユーザー</span><span class="sxs-lookup"><span data-stu-id="63509-158">Require HTTPS and authenticated users</span></span>

<span data-ttu-id="63509-159">追加[IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)に`Startup`:</span><span class="sxs-lookup"><span data-stu-id="63509-159">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="63509-160">`ConfigureServices`のメソッド、 *Startup.cs*ファイルに追加し、 [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)承認フィルター。</span><span class="sxs-lookup"><span data-stu-id="63509-160">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=10-999)]

<span data-ttu-id="63509-161">Visual Studio を使用している場合は、HTTPS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="63509-161">If you're using Visual Studio, enable HTTPS.</span></span>

<span data-ttu-id="63509-162">HTTP 要求を HTTPS にリダイレクトするを参照してください。 [URL 書き換えミドルウェア](xref:fundamentals/url-rewriting)です。</span><span class="sxs-lookup"><span data-stu-id="63509-162">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="63509-163">Visual Studio のコードを使用してローカルのプラットフォームでテストしている場合は、テスト証明書を HTTPS に含まれていません。</span><span class="sxs-lookup"><span data-stu-id="63509-163">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

  <span data-ttu-id="63509-164">設定`"LocalTest:skipSSL": true`で、 *appsettings です。Developement.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="63509-164">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="63509-165">ユーザーが認証されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="63509-165">Require authenticated users</span></span>

<span data-ttu-id="63509-166">ユーザー認証を必要とする既定の認証ポリシーを設定します。</span><span class="sxs-lookup"><span data-stu-id="63509-166">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="63509-167">Razor ページ、コントローラ、またはアクション メソッド レベルでの認証を省略することができます、`[AllowAnonymous]`属性。</span><span class="sxs-lookup"><span data-stu-id="63509-167">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="63509-168">ユーザー認証を必要とする既定の認証ポリシーの設定と、新しく追加された Razor ページおよびコント ローラーが保護されます。</span><span class="sxs-lookup"><span data-stu-id="63509-168">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="63509-169">既定では必要な認証を新しいコント ローラーおよび Razor ページで証明書利用者のより安全ですが、`[Authorize]`属性。</span><span class="sxs-lookup"><span data-stu-id="63509-169">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> 

<span data-ttu-id="63509-170">すべてのユーザーを認証する必要のある、 [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)と[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0)呼び出しは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="63509-170">With the requirement of all users authenticated, the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) and [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) calls are not required.</span></span>

<span data-ttu-id="63509-171">更新`ConfigureServices`で、次の変更。</span><span class="sxs-lookup"><span data-stu-id="63509-171">Update `ConfigureServices` with the following changes:</span></span>

* <span data-ttu-id="63509-172">コメント アウト`AuthorizeFolder`と`AuthorizePage`です。</span><span class="sxs-lookup"><span data-stu-id="63509-172">Comment out `AuthorizeFolder` and `AuthorizePage`.</span></span>
* <span data-ttu-id="63509-173">ユーザー認証を必要とする既定の認証ポリシーを設定します。</span><span class="sxs-lookup"><span data-stu-id="63509-173">Set the default authentication policy to require users to be authenticated.</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=23-27,31-999)]

<span data-ttu-id="63509-174">追加[AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)インデックス、および連絡先について、ページを登録する前に、匿名ユーザーはサイトに関する情報を取得できるようにします。</span><span class="sxs-lookup"><span data-stu-id="63509-174">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="63509-175">追加`[AllowAnonymous]`を[LoginModel と RegisterModel](https://github.com/aspnet/templating/issues/238)です。</span><span class="sxs-lookup"><span data-stu-id="63509-175">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="63509-176">テスト アカウントを構成します。</span><span class="sxs-lookup"><span data-stu-id="63509-176">Configure the test account</span></span>

<span data-ttu-id="63509-177">`SeedData`クラスは 2 つのアカウントを作成します。 管理者と管理者です。</span><span class="sxs-lookup"><span data-stu-id="63509-177">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="63509-178">使用して、[シークレット マネージャー ツール](xref:security/app-secrets)これらのアカウントのパスワードを設定します。</span><span class="sxs-lookup"><span data-stu-id="63509-178">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="63509-179">プロジェクト ディレクトリから、パスワードの設定 (ディレクトリを含む*Program.cs*)。</span><span class="sxs-lookup"><span data-stu-id="63509-179">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="63509-180">更新`Main`テスト パスワードを使用します。</span><span class="sxs-lookup"><span data-stu-id="63509-180">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="63509-181">テスト アカウントを作成し、連絡先の更新</span><span class="sxs-lookup"><span data-stu-id="63509-181">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="63509-182">更新プログラム、`Initialize`メソッドで、`SeedData`テスト アカウントを作成するクラス。</span><span class="sxs-lookup"><span data-stu-id="63509-182">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="63509-183">管理者のユーザー ID を追加し、`ContactStatus`連絡先にします。</span><span class="sxs-lookup"><span data-stu-id="63509-183">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="63509-184">「送信済み」と「拒否」の 1 つの連絡先のいずれかを作成します。</span><span class="sxs-lookup"><span data-stu-id="63509-184">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="63509-185">すべての連絡先に、ユーザー ID と状態を追加します。</span><span class="sxs-lookup"><span data-stu-id="63509-185">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="63509-186">1 人だけが表示されます。</span><span class="sxs-lookup"><span data-stu-id="63509-186">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="63509-187">所有者、マネージャー、および管理者の承認のハンドラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="63509-187">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="63509-188">作成、`ContactIsOwnerAuthorizationHandler`クラス内で、*承認*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="63509-188">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="63509-189">`ContactIsOwnerAuthorizationHandler`リソースで動作しているユーザーがリソースを所有していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="63509-189">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="63509-190">`ContactIsOwnerAuthorizationHandler`呼び出し[コンテキスト。成功](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)現在の認証済みユーザーがメンバーの所有者である場合。</span><span class="sxs-lookup"><span data-stu-id="63509-190">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="63509-191">認証ハンドラー通常。</span><span class="sxs-lookup"><span data-stu-id="63509-191">Authorization handlers generally:</span></span>

* <span data-ttu-id="63509-192">返す`context.Succeed`要件を満たしている場合にします。</span><span class="sxs-lookup"><span data-stu-id="63509-192">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="63509-193">返す`Task.CompletedTask`要件を満たしていない場合にします。</span><span class="sxs-lookup"><span data-stu-id="63509-193">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="63509-194">`Task.CompletedTask` 成功または失敗のどちらも&mdash;他の認証ハンドラーを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="63509-194">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="63509-195">明示的に失敗する場合は、返す[コンテキスト。失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail)です。</span><span class="sxs-lookup"><span data-stu-id="63509-195">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="63509-196">アプリケーションが独自のデータ編集/削除/作成所有者にお問い合わせください。 できます。</span><span class="sxs-lookup"><span data-stu-id="63509-196">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="63509-197">`ContactIsOwnerAuthorizationHandler` 要求パラメーターに渡された操作を確認する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="63509-197">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="63509-198">マネージャーの認証ハンドラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="63509-198">Create a manager authorization handler</span></span>

<span data-ttu-id="63509-199">作成、`ContactManagerAuthorizationHandler`クラス内で、*承認*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="63509-199">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="63509-200">`ContactManagerAuthorizationHandler`リソースに対して機能しているユーザーが管理者であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="63509-200">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="63509-201">マネージャーだけでは、承認したり、コンテンツの変更 (新しいまたは変更された) を拒否することができます。</span><span class="sxs-lookup"><span data-stu-id="63509-201">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="63509-202">管理者認証ハンドラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="63509-202">Create an administrator authorization handler</span></span>

<span data-ttu-id="63509-203">作成、`ContactAdministratorsAuthorizationHandler`クラス内で、*承認*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="63509-203">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="63509-204">`ContactAdministratorsAuthorizationHandler`リソースに対して機能しているユーザーが管理者であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="63509-204">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="63509-205">管理者は、すべての操作を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="63509-205">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="63509-206">認証ハンドラーを登録します。</span><span class="sxs-lookup"><span data-stu-id="63509-206">Register the authorization handlers</span></span>

<span data-ttu-id="63509-207">Entity Framework のコアを使用してサービスを登録する必要があります[依存性の注入](xref:fundamentals/dependency-injection)を使用して[AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)です。</span><span class="sxs-lookup"><span data-stu-id="63509-207">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="63509-208">`ContactIsOwnerAuthorizationHandler` ASP.NET Core を使用して[Identity](xref:security/authentication/identity)、これは Entity Framework Core 上に構築します。</span><span class="sxs-lookup"><span data-stu-id="63509-208">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="63509-209">ハンドラー コレクションに登録サービスを使用するため、`ContactsController`を通じて[依存性の注入](xref:fundamentals/dependency-injection)です。</span><span class="sxs-lookup"><span data-stu-id="63509-209">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="63509-210">末尾に次のコードを追加`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="63509-210">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

<span data-ttu-id="63509-211">`ContactAdministratorsAuthorizationHandler` および`ContactManagerAuthorizationHandler`シングルトンとして追加されます。</span><span class="sxs-lookup"><span data-stu-id="63509-211">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="63509-212">シングルトンを務める EF を使用していないし、必要なすべての情報があるため、`Context`のパラメーター、`HandleRequirementAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="63509-212">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="63509-213">承認をサポートします。</span><span class="sxs-lookup"><span data-stu-id="63509-213">Support authorization</span></span>

<span data-ttu-id="63509-214">このセクションでは、Razor ページを更新し、操作の要件クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="63509-214">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="63509-215">連絡先の操作の要件のクラスを確認します。</span><span class="sxs-lookup"><span data-stu-id="63509-215">Review the contact operations requirements class</span></span>

<span data-ttu-id="63509-216">確認、`ContactOperations`クラスです。</span><span class="sxs-lookup"><span data-stu-id="63509-216">Review the `ContactOperations` class.</span></span> <span data-ttu-id="63509-217">このクラスには、要件が含まれています。 アプリケーションがサポートします。</span><span class="sxs-lookup"><span data-stu-id="63509-217">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="63509-218">Razor ページの基本クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="63509-218">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="63509-219">連絡先 Razor ページで使用するサービスを含む基本クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="63509-219">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="63509-220">基本クラスは、1 つの場所でその初期化コードを配置します。</span><span class="sxs-lookup"><span data-stu-id="63509-220">The base class puts that initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="63509-221">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="63509-221">The preceding code:</span></span>

* <span data-ttu-id="63509-222">追加、`IAuthorizationService`承認ハンドラーへのアクセスをサービスします。</span><span class="sxs-lookup"><span data-stu-id="63509-222">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="63509-223">Id を追加`UserManager`サービス。</span><span class="sxs-lookup"><span data-stu-id="63509-223">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="63509-224">`ApplicationDbContext` を追加します。</span><span class="sxs-lookup"><span data-stu-id="63509-224">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="63509-225">更新プログラム、CreateModel</span><span class="sxs-lookup"><span data-stu-id="63509-225">Update the CreateModel</span></span>

<span data-ttu-id="63509-226">使用するモデル コンス トラクターを作成する ページを更新、`DI_BasePageModel`基本クラス。</span><span class="sxs-lookup"><span data-stu-id="63509-226">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="63509-227">更新プログラム、`CreateModel.OnPostAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="63509-227">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="63509-228">ユーザー ID を追加、`Contact`モデル。</span><span class="sxs-lookup"><span data-stu-id="63509-228">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="63509-229">ユーザーが連絡先を作成するアクセス許可を確認する認証ハンドラーを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="63509-229">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="63509-230">更新プログラム、IndexModel</span><span class="sxs-lookup"><span data-stu-id="63509-230">Update the IndexModel</span></span>

<span data-ttu-id="63509-231">更新プログラム、`OnGetAsync`メソッドため、一般的なユーザーにのみ許可されている連絡先が表示されますの。</span><span class="sxs-lookup"><span data-stu-id="63509-231">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="63509-232">更新プログラム、EditModel</span><span class="sxs-lookup"><span data-stu-id="63509-232">Update the EditModel</span></span>

<span data-ttu-id="63509-233">ユーザーに連絡先を所有していることを確認する認証ハンドラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="63509-233">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="63509-234">リソース承認が検証されているため、`[Authorize]`属性では不十分です。</span><span class="sxs-lookup"><span data-stu-id="63509-234">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="63509-235">アプリは、属性が評価されるときに、リソースへのアクセスにすることがありません。</span><span class="sxs-lookup"><span data-stu-id="63509-235">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="63509-236">リソース ベースの承認は、命令型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="63509-236">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="63509-237">ページのモデルに読み込んで、またはハンドラー自体内で読み込むことにより、アプリが、リソースへのアクセスを持つ、チェックを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="63509-237">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="63509-238">リソース キーを渡すことによって、リソースにアクセスする頻度。</span><span class="sxs-lookup"><span data-stu-id="63509-238">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="63509-239">更新プログラム、DeleteModel</span><span class="sxs-lookup"><span data-stu-id="63509-239">Update the DeleteModel</span></span>

<span data-ttu-id="63509-240">ユーザーが連絡先の削除アクセス権を持つことを確認する認証ハンドラーを使用して削除ページ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="63509-240">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="63509-241">承認サービスをビューに挿入します。</span><span class="sxs-lookup"><span data-stu-id="63509-241">Inject the authorization service into the views</span></span>

<span data-ttu-id="63509-242">現時点では、UI の表示では、編集し、ユーザーが変更できないデータへのリンクを削除します。</span><span class="sxs-lookup"><span data-stu-id="63509-242">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="63509-243">認証ハンドラーをビューに適用することによって、UI が固定されています。</span><span class="sxs-lookup"><span data-stu-id="63509-243">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="63509-244">承認サービスを挿入、 *Views/_ViewImports.cshtml*ファイルのすべてのビューに使用可能になるようにします。</span><span class="sxs-lookup"><span data-stu-id="63509-244">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="63509-245">上記のマークアップに追加のいくつか`using`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="63509-245">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="63509-246">更新プログラム、**編集**と**削除**でリンク*Pages/Contacts/Index.cshtml*のため、適切なアクセス許可を持つユーザー、レンダリング中のみ。</span><span class="sxs-lookup"><span data-stu-id="63509-246">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> <span data-ttu-id="63509-247">データを変更する権限がないユーザーからのリンクを非表示にすると、アプリをセキュリティで保護しません。</span><span class="sxs-lookup"><span data-stu-id="63509-247">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="63509-248">リンクを非表示にするにより、アプリをユーザーにわかりやすい唯一の有効なリンクを表示します。</span><span class="sxs-lookup"><span data-stu-id="63509-248">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="63509-249">ユーザーは、編集を呼び出すし、自分が所有しないデータの操作を削除するには、生成された Url を切断できます。</span><span class="sxs-lookup"><span data-stu-id="63509-249">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="63509-250">Razor ページまたはコント ローラーは、データを保護するアクセス チェックを適用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="63509-250">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="63509-251">更新プログラムの詳細</span><span class="sxs-lookup"><span data-stu-id="63509-251">Update Details</span></span>

<span data-ttu-id="63509-252">マネージャーが承認または連絡先を拒否するため、詳細ビューを更新します。</span><span class="sxs-lookup"><span data-stu-id="63509-252">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

<span data-ttu-id="63509-253">詳細ページのモデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="63509-253">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="63509-254">完成したアプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="63509-254">Test the completed app</span></span>

<span data-ttu-id="63509-255">Visual Studio のコードを使用してローカルのプラットフォームでテストしている場合は、テスト証明書を HTTPS に含まれていません。</span><span class="sxs-lookup"><span data-stu-id="63509-255">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

* <span data-ttu-id="63509-256">設定`"LocalTest:skipSSL": true`で、 *appsettings です。Developement.json* HTTPS 要件をスキップするファイル。</span><span class="sxs-lookup"><span data-stu-id="63509-256">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file to skip the HTTPS requirement.</span></span> <span data-ttu-id="63509-257">開発用コンピューターでのみ Skip HTTPS です。</span><span class="sxs-lookup"><span data-stu-id="63509-257">Skip HTTPS only on a development machine.</span></span>

<span data-ttu-id="63509-258">場合は、アプリには、連絡先があります。</span><span class="sxs-lookup"><span data-stu-id="63509-258">If the app has contacts:</span></span>

* <span data-ttu-id="63509-259">内のすべてのレコードを削除、`Contact`テーブル。</span><span class="sxs-lookup"><span data-stu-id="63509-259">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="63509-260">データベースのシードにアプリを再起動します。</span><span class="sxs-lookup"><span data-stu-id="63509-260">Restart the app to seed the database.</span></span>

<span data-ttu-id="63509-261">連絡先を参照するためには、ユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="63509-261">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="63509-262">完成したアプリをテストする簡単な方法では、3 つの異なるブラウザー (または incognito/inprivate ブラウズのバージョン) を起動します。</span><span class="sxs-lookup"><span data-stu-id="63509-262">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="63509-263">1 つのブラウザーで、新しいユーザーを登録します (たとえば、 `test@example.com`)。</span><span class="sxs-lookup"><span data-stu-id="63509-263">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="63509-264">別のユーザーに各ブラウザーにサインインします。</span><span class="sxs-lookup"><span data-stu-id="63509-264">Sign in to each browser with a different user.</span></span> <span data-ttu-id="63509-265">次の操作を確認します。</span><span class="sxs-lookup"><span data-stu-id="63509-265">Verify the following operations:</span></span>

* <span data-ttu-id="63509-266">登録済みのユーザーには、承認されているすべての連絡先データを表示できます。</span><span class="sxs-lookup"><span data-stu-id="63509-266">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="63509-267">登録済みユーザーを編集/削除が独自のデータ。</span><span class="sxs-lookup"><span data-stu-id="63509-267">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="63509-268">管理者では、承認したり、連絡先データを拒否することができます。</span><span class="sxs-lookup"><span data-stu-id="63509-268">Managers can approve or reject contact data.</span></span> <span data-ttu-id="63509-269">`Details`ビューには、**承認**と**拒否**ボタン。</span><span class="sxs-lookup"><span data-stu-id="63509-269">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="63509-270">および管理者が承認または却下とすべてのデータを編集/削除します。</span><span class="sxs-lookup"><span data-stu-id="63509-270">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="63509-271">ユーザー</span><span class="sxs-lookup"><span data-stu-id="63509-271">User</span></span>| <span data-ttu-id="63509-272">オプション</span><span class="sxs-lookup"><span data-stu-id="63509-272">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="63509-273">データの編集、または削除できます。</span><span class="sxs-lookup"><span data-stu-id="63509-273">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="63509-274">承認または拒否し、編集/削除を所有できるデータ</span><span class="sxs-lookup"><span data-stu-id="63509-274">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="63509-275">編集/削除してすべてのデータの承認または却下</span><span class="sxs-lookup"><span data-stu-id="63509-275">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="63509-276">管理者のブラウザーで連絡先を作成します。</span><span class="sxs-lookup"><span data-stu-id="63509-276">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="63509-277">削除の URL をコピーし、管理者の連絡先から編集します。</span><span class="sxs-lookup"><span data-stu-id="63509-277">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="63509-278">これらのリンクをテスト ユーザーは、これらの操作を実行できないことを確認するテスト ユーザーのブラウザーに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="63509-278">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="63509-279">スターター アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="63509-279">Create the starter app</span></span>

* <span data-ttu-id="63509-280">"ContactManager"をという名前の Razor ページ アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="63509-280">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="63509-281">使用してアプリを作成する**個々 のユーザー アカウント**です。</span><span class="sxs-lookup"><span data-stu-id="63509-281">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="63509-282">名前を付けます"ContactManager"ので、名前空間のサンプルで使用される名前空間に一致します。</span><span class="sxs-lookup"><span data-stu-id="63509-282">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

::: moniker range=">= aspnetcore-2.1"

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

::: moniker-end

  * <span data-ttu-id="63509-283">`-uld` SQLite ではなく LocalDB を指定します</span><span class="sxs-lookup"><span data-stu-id="63509-283">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="63509-284">次の追加`Contact`モデル。</span><span class="sxs-lookup"><span data-stu-id="63509-284">Add the following `Contact` model:</span></span>

  [!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="63509-285">Scaffold、`Contact`モデル。</span><span class="sxs-lookup"><span data-stu-id="63509-285">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="63509-286">更新プログラム、 **ContactManager**でアンカー、 *Pages/_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="63509-286">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="63509-287">初期の移行をスキャフォールディングして、データベースの更新します。</span><span class="sxs-lookup"><span data-stu-id="63509-287">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="63509-288">アプリをテストするには、作成、編集、および連絡先を削除します。</span><span class="sxs-lookup"><span data-stu-id="63509-288">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="63509-289">データベースのシード</span><span class="sxs-lookup"><span data-stu-id="63509-289">Seed the database</span></span>

<span data-ttu-id="63509-290">追加、`SeedData`クラスを*データ*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="63509-290">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="63509-291">サンプル、ダウンロードした場合は、コピー、 *SeedData.cs*ファイルの名前を*データ*のスタート プロジェクトのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="63509-291">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="63509-292">呼び出す`SeedData.Initialize`から`Main`:</span><span class="sxs-lookup"><span data-stu-id="63509-292">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="63509-293">アプリに、データベースがシード処理をテストします。</span><span class="sxs-lookup"><span data-stu-id="63509-293">Test that the app seeded the database.</span></span> <span data-ttu-id="63509-294">DB の連絡先に任意の行がある場合は、シード メソッドは実行されません。</span><span class="sxs-lookup"><span data-stu-id="63509-294">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="63509-295">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="63509-295">Additional resources</span></span>

* <span data-ttu-id="63509-296">[ASP.NET Core 承認ラボ](https://github.com/blowdart/AspNetAuthorizationWorkshop)です。</span><span class="sxs-lookup"><span data-stu-id="63509-296">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="63509-297">このラボでは、このチュートリアルで導入されたセキュリティ機能の詳細に移動します。</span><span class="sxs-lookup"><span data-stu-id="63509-297">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="63509-298">ASP.NET Core での承認: 単純な場合、ロール、クレームに基づく、およびカスタム</span><span class="sxs-lookup"><span data-stu-id="63509-298">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="63509-299">カスタム ポリシー ベースの承認</span><span class="sxs-lookup"><span data-stu-id="63509-299">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
