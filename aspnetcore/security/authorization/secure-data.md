---
title: 承認によって保護されたユーザー データと ASP.NET Core アプリを作成します。
author: rick-anderson
description: 承認によって保護されたユーザー データと Razor ページ アプリを作成する方法について説明します。 HTTPS、認証、セキュリティ、ASP.NET Core Identity が含まれています。
ms.author: riande
ms.date: 12/18/2018
ms.custom: seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: bdba706c1ef24ebe35129cb8bb2d9949196245a1
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098923"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="10b5e-104">承認によって保護されたユーザー データと ASP.NET Core アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="10b5e-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="10b5e-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="10b5e-106">参照してください[この PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core MVC のバージョン。</span><span class="sxs-lookup"><span data-stu-id="10b5e-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="10b5e-107">このチュートリアルの ASP.NET Core 1.1 バージョンは[この](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data)フォルダー。</span><span class="sxs-lookup"><span data-stu-id="10b5e-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="10b5e-108">ASP.NET Core サンプルについては、1.1、[サンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="10b5e-109">参照してください[この pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="10b5e-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="10b5e-110">このチュートリアルでは、承認によって保護されたユーザー データと ASP.NET Core web アプリを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="10b5e-111">認証済み (登録済み) のユーザーの連絡先の一覧を表示を作成します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="10b5e-112">これには 3 つのセキュリティ グループがあります。</span><span class="sxs-lookup"><span data-stu-id="10b5e-112">There are three security groups:</span></span>

* <span data-ttu-id="10b5e-113">**ユーザーが登録されている**承認済みのすべてのデータを表示することができ、独自のデータ編集、または削除できます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="10b5e-114">**マネージャー**を承認または連絡先データを拒否します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="10b5e-115">承認されたメンバーのみがユーザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="10b5e-116">**管理者**承認または却下と編集/削除のすべてのデータをことができます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="10b5e-117">次の図では、ユーザー Rick の (`rick@example.com`) がサインインしています。</span><span class="sxs-lookup"><span data-stu-id="10b5e-117">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="10b5e-118">Rick は許可されている連絡先のみを表示し、**編集**/**削除**/**新規作成**彼の連絡先へのリンク。</span><span class="sxs-lookup"><span data-stu-id="10b5e-118">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="10b5e-119">最後のレコードのみが Rick、表示によって作成された**編集**と**削除**リンク。</span><span class="sxs-lookup"><span data-stu-id="10b5e-119">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="10b5e-120">他のユーザーは、管理者は、状態を"Approved"に変わるまで、最後のレコードを表示されません。</span><span class="sxs-lookup"><span data-stu-id="10b5e-120">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![サインインして Rick を示すスクリーン ショット](secure-data/_static/rick.png)

<span data-ttu-id="10b5e-122">次の図の`manager@contoso.com`では、managers ロールでは署名します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-122">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![スクリーン ショットmanager@contoso.comサインイン](secure-data/_static/manager1.png)

<span data-ttu-id="10b5e-124">次の図は、連絡先の詳細ビューをマネージャーには。</span><span class="sxs-lookup"><span data-stu-id="10b5e-124">The following image shows the managers details view of a contact:</span></span>

![連絡先のマネージャーの表示](secure-data/_static/manager.png)

<span data-ttu-id="10b5e-126">**承認**と**拒否**ボタンは、管理者と管理者のみ表示されます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-126">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="10b5e-127">次の図の`admin@contoso.com`では、管理者ロールには署名します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-127">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![スクリーン ショットadmin@contoso.comサインイン](secure-data/_static/admin.png)

<span data-ttu-id="10b5e-129">管理者は、すべての特権を持ちます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-129">The administrator has all privileges.</span></span> <span data-ttu-id="10b5e-130">彼女は、読み取り/編集/削除のいずれかの連絡先をことができ、連絡先の状態を変更します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-130">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="10b5e-131">によって、アプリが作成された[スキャフォールディング](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model)次`Contact`モデル。</span><span class="sxs-lookup"><span data-stu-id="10b5e-131">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="10b5e-132">サンプルには、次の承認ハンドラーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="10b5e-132">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="10b5e-133">`ContactIsOwnerAuthorizationHandler`:により、ユーザーは、そのデータにのみ編集できます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-133">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="10b5e-134">`ContactManagerAuthorizationHandler`:承認または拒否の連絡先のマネージャーを使用します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-134">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="10b5e-135">`ContactAdministratorsAuthorizationHandler`:管理者を承認または拒否の連絡先と連絡先の編集/削除に使用できます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-135">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="10b5e-136">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="10b5e-136">Prerequisites</span></span>

<span data-ttu-id="10b5e-137">このチュートリアルは、高度なです。</span><span class="sxs-lookup"><span data-stu-id="10b5e-137">This tutorial is advanced.</span></span> <span data-ttu-id="10b5e-138">理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="10b5e-138">You should be familiar with:</span></span>

* [<span data-ttu-id="10b5e-139">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="10b5e-139">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="10b5e-140">認証</span><span class="sxs-lookup"><span data-stu-id="10b5e-140">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="10b5e-141">アカウントの確認とパスワードの回復</span><span class="sxs-lookup"><span data-stu-id="10b5e-141">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="10b5e-142">承認</span><span class="sxs-lookup"><span data-stu-id="10b5e-142">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="10b5e-143">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="10b5e-143">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="10b5e-144">ASP.NET Core 2.1 で`User.IsInRole`を使用する場合は失敗`AddDefaultIdentity`します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-144">In ASP.NET Core 2.1, `User.IsInRole` fails when using `AddDefaultIdentity`.</span></span> <span data-ttu-id="10b5e-145">このチュートリアルでは`AddDefaultIdentity`のため 2.2 またはそれ以降の ASP.NET Core 必要があります。</span><span class="sxs-lookup"><span data-stu-id="10b5e-145">This tutorial uses `AddDefaultIdentity` and therefore requires ASP.NET Core 2.2 or later.</span></span> <span data-ttu-id="10b5e-146">参照してください[この GitHub の問題](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909)を回避します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-146">See [this GitHub issue](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) for a work-around.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="10b5e-147">Starter および完成したアプリ</span><span class="sxs-lookup"><span data-stu-id="10b5e-147">The starter and completed app</span></span>

<span data-ttu-id="10b5e-148">[ダウンロード](xref:index#how-to-download-a-sample)、[完了](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)アプリ。</span><span class="sxs-lookup"><span data-stu-id="10b5e-148">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="10b5e-149">[テスト](#test-the-completed-app)完成したアプリのセキュリティ機能を理解するようにします。</span><span class="sxs-lookup"><span data-stu-id="10b5e-149">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="10b5e-150">スターター アプリ</span><span class="sxs-lookup"><span data-stu-id="10b5e-150">The starter app</span></span>

<span data-ttu-id="10b5e-151">[ダウンロード](xref:index#how-to-download-a-sample)、[スターター](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2)アプリ。</span><span class="sxs-lookup"><span data-stu-id="10b5e-151">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="10b5e-152">アプリを実行し、タップ、 **ContactManager**リンク、および作成、編集、および連絡先を削除することを確認します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-152">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="10b5e-153">セキュリティで保護されたユーザー データ</span><span class="sxs-lookup"><span data-stu-id="10b5e-153">Secure user data</span></span>

<span data-ttu-id="10b5e-154">次のセクションでは、セキュリティで保護されたユーザー データ アプリを作成するすべての主要な手順があります。</span><span class="sxs-lookup"><span data-stu-id="10b5e-154">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="10b5e-155">完成したプロジェクトを参照すると便利です。</span><span class="sxs-lookup"><span data-stu-id="10b5e-155">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="10b5e-156">ユーザーに連絡先データを関連付ける</span><span class="sxs-lookup"><span data-stu-id="10b5e-156">Tie the contact data to the user</span></span>

<span data-ttu-id="10b5e-157">ASP.NET を使用して、 [Identity](xref:security/authentication/identity)データが他のユーザー データではなく、ユーザーのユーザー ID を編集できます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-157">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="10b5e-158">追加`OwnerID`と`ContactStatus`を`Contact`モデル。</span><span class="sxs-lookup"><span data-stu-id="10b5e-158">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="10b5e-159">`OwnerID` ユーザーの id、`AspNetUser`テーブルに、 [Identity](xref:security/authentication/identity)データベース。</span><span class="sxs-lookup"><span data-stu-id="10b5e-159">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="10b5e-160">`Status`連絡先が一般ユーザーが表示可能なかどうかにフィールドが決定します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-160">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="10b5e-161">新しい移行を作成し、データベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-161">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="10b5e-162">役割サービスの Id を追加します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-162">Add Role services to Identity</span></span>

<span data-ttu-id="10b5e-163">追加[AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1)役割サービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-163">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="10b5e-164">認証されたユーザーが必要です。</span><span class="sxs-lookup"><span data-stu-id="10b5e-164">Require authenticated users</span></span>

<span data-ttu-id="10b5e-165">ユーザー認証を必要とする既定の認証ポリシーを設定します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-165">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="10b5e-166">Razor ページ、コントローラ、またはアクション メソッド レベルでの認証をオプトアウトすることができます、`[AllowAnonymous]`属性。</span><span class="sxs-lookup"><span data-stu-id="10b5e-166">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="10b5e-167">ユーザー認証を必要とする既定の認証ポリシーの設定は、新しく追加された Razor ページとコント ローラーを保護します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-167">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="10b5e-168">既定で必要な認証は、新しいコント ローラーと Razor ページで証明書利用者より安全ですが、`[Authorize]`属性。</span><span class="sxs-lookup"><span data-stu-id="10b5e-168">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="10b5e-169">追加[AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)インデックス、および連絡先について、ページ匿名ユーザーは登録前に、サイトに関する情報を取得できるようにします。</span><span class="sxs-lookup"><span data-stu-id="10b5e-169">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="10b5e-170">テスト アカウントを構成します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-170">Configure the test account</span></span>

<span data-ttu-id="10b5e-171">`SeedData`クラスは、2 つのアカウントを作成します。 管理者とマネージャー。</span><span class="sxs-lookup"><span data-stu-id="10b5e-171">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="10b5e-172">使用して、 [Secret Manager ツール](xref:security/app-secrets)これらのアカウントのパスワードを設定します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-172">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="10b5e-173">プロジェクト ディレクトリから、パスワードの設定 (含まれるディレクトリ*Program.cs*)。</span><span class="sxs-lookup"><span data-stu-id="10b5e-173">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="10b5e-174">例外がときにスローされる場合は、強力なパスワードが指定されていない`SeedData.Initialize`が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-174">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="10b5e-175">Update`Main`テストのパスワードを使用します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-175">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="10b5e-176">テスト アカウントを作成し、連絡先の更新</span><span class="sxs-lookup"><span data-stu-id="10b5e-176">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="10b5e-177">更新プログラム、`Initialize`メソッドで、`SeedData`テスト アカウントを作成するクラス。</span><span class="sxs-lookup"><span data-stu-id="10b5e-177">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="10b5e-178">管理者のユーザー ID を追加し、`ContactStatus`連絡先にします。</span><span class="sxs-lookup"><span data-stu-id="10b5e-178">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="10b5e-179">「送信済み」と「拒否」の 1 つの連絡先を作成します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-179">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="10b5e-180">すべての連絡先に、ユーザー ID と状態を追加します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-180">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="10b5e-181">1 人だけが表示されます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-181">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="10b5e-182">所有者、マネージャー、および管理者の承認ハンドラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-182">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="10b5e-183">作成、`ContactIsOwnerAuthorizationHandler`クラス、*承認*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="10b5e-183">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="10b5e-184">`ContactIsOwnerAuthorizationHandler`リソースで動作しているユーザーがリソースを所有していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-184">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="10b5e-185">`ContactIsOwnerAuthorizationHandler`呼び出し[コンテキスト。成功](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)現在認証済みユーザーがメンバーの所有者である場合。</span><span class="sxs-lookup"><span data-stu-id="10b5e-185">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="10b5e-186">承認ハンドラー通常。</span><span class="sxs-lookup"><span data-stu-id="10b5e-186">Authorization handlers generally:</span></span>

* <span data-ttu-id="10b5e-187">返す`context.Succeed`要件を満たしている場合。</span><span class="sxs-lookup"><span data-stu-id="10b5e-187">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="10b5e-188">返す`Task.CompletedTask`要件が満たされていません。</span><span class="sxs-lookup"><span data-stu-id="10b5e-188">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="10b5e-189">`Task.CompletedTask` 成功または失敗のどちらも&mdash;他の承認ハンドラーを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-189">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="10b5e-190">明示的に失敗する場合は、返す[コンテキスト。失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail)します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-190">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="10b5e-191">アプリは、独自のデータを編集/削除/作成所有者が連絡先にできます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-191">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="10b5e-192">`ContactIsOwnerAuthorizationHandler` 要求パラメーターで渡された操作を確認する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="10b5e-192">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="10b5e-193">マネージャーの承認ハンドラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-193">Create a manager authorization handler</span></span>

<span data-ttu-id="10b5e-194">作成、`ContactManagerAuthorizationHandler`クラス、*承認*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="10b5e-194">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="10b5e-195">`ContactManagerAuthorizationHandler`リソースで動作しているユーザーが、マネージャーであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-195">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="10b5e-196">管理者だけでは、承認したり、(新規または変更された) コンテンツの変更を拒否することができます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-196">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="10b5e-197">管理者の承認ハンドラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-197">Create an administrator authorization handler</span></span>

<span data-ttu-id="10b5e-198">作成、`ContactAdministratorsAuthorizationHandler`クラス、*承認*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="10b5e-198">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="10b5e-199">`ContactAdministratorsAuthorizationHandler`リソースで動作しているユーザーが管理者であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-199">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="10b5e-200">管理者は、すべての操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-200">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="10b5e-201">承認ハンドラーを登録します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-201">Register the authorization handlers</span></span>

<span data-ttu-id="10b5e-202">Entity Framework Core を使用してサービスを登録する必要があります[依存関係の注入](xref:fundamentals/dependency-injection)を使用して[AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-202">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="10b5e-203">`ContactIsOwnerAuthorizationHandler` ASP.NET Core を使用して[Identity](xref:security/authentication/identity)、これは Entity Framework Core 上に構築します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-203">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="10b5e-204">使用するため、サービスのコレクションを使用してハンドラーを登録、`ContactsController`を通じて[依存関係の注入](xref:fundamentals/dependency-injection)します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-204">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="10b5e-205">末尾に次のコードを追加`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="10b5e-205">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="10b5e-206">`ContactAdministratorsAuthorizationHandler` `ContactManagerAuthorizationHandler`シングルトンとして追加されます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-206">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="10b5e-207">シングルトンになるため、EF を使用していないし、必要なすべての情報は、`Context`のパラメーター、`HandleRequirementAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="10b5e-207">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="10b5e-208">承認をサポートします。</span><span class="sxs-lookup"><span data-stu-id="10b5e-208">Support authorization</span></span>

<span data-ttu-id="10b5e-209">このセクションでは、Razor ページを更新し、操作の要件クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-209">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="10b5e-210">連絡先の操作の要件のクラスを確認してください。</span><span class="sxs-lookup"><span data-stu-id="10b5e-210">Review the contact operations requirements class</span></span>

<span data-ttu-id="10b5e-211">レビュー、`ContactOperations`クラス。</span><span class="sxs-lookup"><span data-stu-id="10b5e-211">Review the `ContactOperations` class.</span></span> <span data-ttu-id="10b5e-212">このクラスには、要件が含まれています。 アプリがサポートされます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-212">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="10b5e-213">連絡先の Razor ページの基本クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-213">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="10b5e-214">連絡先の Razor ページで使用されるサービスを含む基本クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-214">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="10b5e-215">基底クラスでは、1 つの場所に、初期化コードが配置します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-215">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="10b5e-216">上のコードでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-216">The preceding code:</span></span>

* <span data-ttu-id="10b5e-217">追加、`IAuthorizationService`承認ハンドラーへのアクセスをサービスします。</span><span class="sxs-lookup"><span data-stu-id="10b5e-217">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="10b5e-218">Id を追加します。`UserManager`サービス。</span><span class="sxs-lookup"><span data-stu-id="10b5e-218">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="10b5e-219">`ApplicationDbContext` を追加します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-219">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="10b5e-220">CreateModel の更新します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-220">Update the CreateModel</span></span>

<span data-ttu-id="10b5e-221">作成のページ モデルを使用するコンス トラクターを更新、`DI_BasePageModel`基本クラス。</span><span class="sxs-lookup"><span data-stu-id="10b5e-221">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="10b5e-222">更新プログラム、`CreateModel.OnPostAsync`メソッド。</span><span class="sxs-lookup"><span data-stu-id="10b5e-222">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="10b5e-223">ユーザー ID を追加、`Contact`モデル。</span><span class="sxs-lookup"><span data-stu-id="10b5e-223">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="10b5e-224">ユーザーが連絡先を作成するためのアクセス許可を確認する認証ハンドラーを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-224">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="10b5e-225">更新プログラム、IndexModel</span><span class="sxs-lookup"><span data-stu-id="10b5e-225">Update the IndexModel</span></span>

<span data-ttu-id="10b5e-226">更新プログラム、`OnGetAsync`メソッドのため、一般ユーザーにのみ許可されている連絡先が表示されます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-226">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="10b5e-227">更新プログラム、EditModel</span><span class="sxs-lookup"><span data-stu-id="10b5e-227">Update the EditModel</span></span>

<span data-ttu-id="10b5e-228">ユーザーが連絡先を所有していることを確認するための承認ハンドラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-228">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="10b5e-229">リソース承認が検証されているため、`[Authorize]`属性が十分ではありません。</span><span class="sxs-lookup"><span data-stu-id="10b5e-229">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="10b5e-230">アプリは、属性が評価されたときに、リソースへのアクセスにすることがありません。</span><span class="sxs-lookup"><span data-stu-id="10b5e-230">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="10b5e-231">リソース ベースの承認は、命令型である必要があります。</span><span class="sxs-lookup"><span data-stu-id="10b5e-231">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="10b5e-232">ページ モデルに読み込んで、または読み込みハンドラー自体内で、アプリが、リソースへのアクセスを持つ、チェックを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="10b5e-232">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="10b5e-233">リソース キーを渡すことで、リソースを頻繁にアクセスするとします。</span><span class="sxs-lookup"><span data-stu-id="10b5e-233">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="10b5e-234">更新プログラム、DeleteModel</span><span class="sxs-lookup"><span data-stu-id="10b5e-234">Update the DeleteModel</span></span>

<span data-ttu-id="10b5e-235">承認ハンドラーを使用して、ユーザーが連絡先に削除するアクセス許可を持ってことを確認する delete ページ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-235">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="10b5e-236">承認サービスをビューに挿入します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-236">Inject the authorization service into the views</span></span>

<span data-ttu-id="10b5e-237">現時点では、UI の表示では、編集し、ユーザーが変更できない連絡先へのリンクを削除します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-237">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="10b5e-238">承認サービスを挿入、 *Views/_ViewImports.cshtml*ファイルのすべてのビューに使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="10b5e-238">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="10b5e-239">上記のマークアップにいくつか追加`using`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="10b5e-239">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="10b5e-240">更新プログラム、**編集**と**削除**でリンク*Pages/Contacts/Index.cshtml*適切なアクセス許可を持つユーザーのみレンダリングするように。</span><span class="sxs-lookup"><span data-stu-id="10b5e-240">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="10b5e-241">変更データへのアクセス許可がないユーザーからのリンクを非表示と、アプリをセキュリティで保護しません。</span><span class="sxs-lookup"><span data-stu-id="10b5e-241">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="10b5e-242">リンクを非表示アプリよりユーザー フレンドリな唯一の有効なリンクを表示することで。</span><span class="sxs-lookup"><span data-stu-id="10b5e-242">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="10b5e-243">ユーザーは、編集を呼び出すし、所有していないデータの操作を削除するには、生成された Url を切断できます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-243">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="10b5e-244">Razor ページまたはコント ローラーは、データを保護するアクセス チェックを適用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="10b5e-244">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="10b5e-245">更新プログラムの詳細</span><span class="sxs-lookup"><span data-stu-id="10b5e-245">Update Details</span></span>

<span data-ttu-id="10b5e-246">マネージャーが承認または連絡先を拒否するために、詳細ビューを更新します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-246">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="10b5e-247">詳細のページ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-247">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="10b5e-248">追加またはロールにユーザーを削除します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-248">Add or remove a user to a role</span></span>

<span data-ttu-id="10b5e-249">参照してください[今月](https://github.com/aspnet/Docs/issues/8502)について。</span><span class="sxs-lookup"><span data-stu-id="10b5e-249">See [this issue](https://github.com/aspnet/Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="10b5e-250">ユーザーから権限を削除しています。</span><span class="sxs-lookup"><span data-stu-id="10b5e-250">Removing privileges from a user.</span></span> <span data-ttu-id="10b5e-251">たとえばのチャット アプリケーションのユーザーをミュートします。</span><span class="sxs-lookup"><span data-stu-id="10b5e-251">For example muting a user in a chat app.</span></span>
* <span data-ttu-id="10b5e-252">ユーザーに特権を追加します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-252">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="10b5e-253">完成したアプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="10b5e-253">Test the completed app</span></span>

<span data-ttu-id="10b5e-254">シード処理されたユーザー アカウントのパスワードを設定していない場合は、使用、 [Secret Manager ツール](xref:security/app-secrets#secret-manager)パスワードを設定します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-254">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="10b5e-255">強力なパスワードを選択します。以上の文字し、少なくとも 1 つの大文字の文字数、およびシンボルまたは 8 を使用しています。</span><span class="sxs-lookup"><span data-stu-id="10b5e-255">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="10b5e-256">たとえば、`Passw0rd!`強力なパスワード要件を満たしています。</span><span class="sxs-lookup"><span data-stu-id="10b5e-256">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="10b5e-257">プロジェクトのフォルダーから次のコマンドを実行、`<PW>`パスワードです。</span><span class="sxs-lookup"><span data-stu-id="10b5e-257">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="10b5e-258">場合は、アプリでは、連絡先があります。</span><span class="sxs-lookup"><span data-stu-id="10b5e-258">If the app has contacts:</span></span>

* <span data-ttu-id="10b5e-259">すべてのレコードの削除、`Contact`テーブル。</span><span class="sxs-lookup"><span data-stu-id="10b5e-259">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="10b5e-260">データベースをシードするアプリを再起動します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-260">Restart the app to seed the database.</span></span>

<span data-ttu-id="10b5e-261">完成したアプリをテストする簡単な方法では、次の 3 つの異なるブラウザー (または incognito/inprivate ブラウズ セッション) を起動します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-261">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="10b5e-262">1 つのブラウザーで新しいユーザーを登録します (たとえば、 `test@example.com`)。</span><span class="sxs-lookup"><span data-stu-id="10b5e-262">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="10b5e-263">ブラウザーごとに別のユーザーでサインインします。</span><span class="sxs-lookup"><span data-stu-id="10b5e-263">Sign in to each browser with a different user.</span></span> <span data-ttu-id="10b5e-264">次の操作を確認します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-264">Verify the following operations:</span></span>

* <span data-ttu-id="10b5e-265">登録済みユーザーには、すべての承認済みの連絡先データを表示できます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-265">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="10b5e-266">登録済みユーザーは編集/削除が自分のデータ。</span><span class="sxs-lookup"><span data-stu-id="10b5e-266">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="10b5e-267">管理者では、連絡先データの承認または却下をします。</span><span class="sxs-lookup"><span data-stu-id="10b5e-267">Managers can approve/reject contact data.</span></span> <span data-ttu-id="10b5e-268">`Details`ビューには、**承認**と**拒否**ボタン。</span><span class="sxs-lookup"><span data-stu-id="10b5e-268">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="10b5e-269">管理者承認または却下して編集/削除のすべてのデータ。</span><span class="sxs-lookup"><span data-stu-id="10b5e-269">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="10b5e-270">ユーザー</span><span class="sxs-lookup"><span data-stu-id="10b5e-270">User</span></span>                | <span data-ttu-id="10b5e-271">アプリによってシード処理</span><span class="sxs-lookup"><span data-stu-id="10b5e-271">Seeded by the app</span></span> | <span data-ttu-id="10b5e-272">オプション</span><span class="sxs-lookup"><span data-stu-id="10b5e-272">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="10b5e-273">いいえ</span><span class="sxs-lookup"><span data-stu-id="10b5e-273">No</span></span>                | <span data-ttu-id="10b5e-274">独自のデータを編集/削除します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-274">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="10b5e-275">[はい]</span><span class="sxs-lookup"><span data-stu-id="10b5e-275">Yes</span></span>               | <span data-ttu-id="10b5e-276">承認または却下と編集/削除は、データを所有します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-276">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="10b5e-277">[はい]</span><span class="sxs-lookup"><span data-stu-id="10b5e-277">Yes</span></span>               | <span data-ttu-id="10b5e-278">承認または却下し、すべてのデータの編集/削除します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-278">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="10b5e-279">管理者のブラウザーで連絡先を作成します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-279">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="10b5e-280">削除の URL をコピーし、管理者の連絡先から管理者を編集します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-280">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="10b5e-281">テスト ユーザーのブラウザー テスト ユーザーがこれらの操作を実行できないことを確認するには、これらのリンクを貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="10b5e-281">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="10b5e-282">スターター アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-282">Create the starter app</span></span>

* <span data-ttu-id="10b5e-283">「ContactManager」という名前の Razor ページ アプリの作成</span><span class="sxs-lookup"><span data-stu-id="10b5e-283">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="10b5e-284">使用してアプリを作成する**個々 のユーザー アカウント**します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-284">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="10b5e-285">ファイル名「ContactManager」名前空間サンプルで使用する名前空間が一致するようにします。</span><span class="sxs-lookup"><span data-stu-id="10b5e-285">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="10b5e-286">`-uld` SQLite の代わりに LocalDB を指定します</span><span class="sxs-lookup"><span data-stu-id="10b5e-286">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="10b5e-287">追加*Models/Contact.cs*:</span><span class="sxs-lookup"><span data-stu-id="10b5e-287">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="10b5e-288">スキャフォールディング、`Contact`モデル。</span><span class="sxs-lookup"><span data-stu-id="10b5e-288">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="10b5e-289">最初の移行を作成し、データベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-289">Create initial migration and update the database:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="10b5e-290">更新プログラム、 **ContactManager**で固定、 *Pages/_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="10b5e-290">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="10b5e-291">作成、編集、および連絡先を削除して、アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="10b5e-291">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="10b5e-292">データベースのシード</span><span class="sxs-lookup"><span data-stu-id="10b5e-292">Seed the database</span></span>

<span data-ttu-id="10b5e-293">追加、 [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs)クラスを*データ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="10b5e-293">Add the [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="10b5e-294">呼び出す`SeedData.Initialize`から`Main`:</span><span class="sxs-lookup"><span data-stu-id="10b5e-294">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="10b5e-295">アプリが、データベースをシード処理をテストします。</span><span class="sxs-lookup"><span data-stu-id="10b5e-295">Test that the app seeded the database.</span></span> <span data-ttu-id="10b5e-296">DB の連絡先に行がある場合は、seed メソッドは実行されません。</span><span class="sxs-lookup"><span data-stu-id="10b5e-296">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="10b5e-297">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="10b5e-297">Additional resources</span></span>

* [<span data-ttu-id="10b5e-298">Azure App Service で .NET Core および SQL Database web アプリを構築します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-298">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="10b5e-299">[ASP.NET Core 承認ラボ](https://github.com/blowdart/AspNetAuthorizationWorkshop)します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-299">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="10b5e-300">このラボでは、このチュートリアルで導入されたセキュリティ機能の詳細に移動します。</span><span class="sxs-lookup"><span data-stu-id="10b5e-300">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="10b5e-301">カスタム ポリシー ベースの承認</span><span class="sxs-lookup"><span data-stu-id="10b5e-301">Custom policy-based authorization</span></span>](xref:security/authorization/policies)

::: moniker-end
