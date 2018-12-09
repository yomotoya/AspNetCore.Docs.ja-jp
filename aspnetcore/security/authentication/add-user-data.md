---
title: 追加、ダウンロード、および Id に、ASP.NET Core プロジェクト内のユーザー データの削除
author: rick-anderson
description: ASP.NET Core プロジェクトでのユーザーにカスタム ユーザー データを追加する方法について説明します。 GDPR ごとのデータを削除します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
ms.custom: seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 5465117e5db880e8298e6c2075a27699e4081894
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121102"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="b32b8-104">追加、ダウンロード、および Id に、ASP.NET Core プロジェクトでのカスタム ユーザー データの削除</span><span class="sxs-lookup"><span data-stu-id="b32b8-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="b32b8-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b32b8-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b32b8-106">この記事では方法。</span><span class="sxs-lookup"><span data-stu-id="b32b8-106">This article shows how to:</span></span>

* <span data-ttu-id="b32b8-107">ASP.NET Core web アプリにカスタム ユーザー データを追加します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="b32b8-108">カスタム ユーザー データ モデルに装飾、 [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1)属性にダウンロードおよび削除のために自動的に使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="b32b8-108">Decorate the custom user data model with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="b32b8-109">データをダウンロードして、削除することを行うには、満たす助けとなる[GDPR](xref:security/gdpr)要件。</span><span class="sxs-lookup"><span data-stu-id="b32b8-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="b32b8-110">プロジェクト サンプルは、Razor ページ web アプリから作成されますが、手順は ASP.NET Core MVC web アプリと同様。</span><span class="sxs-lookup"><span data-stu-id="b32b8-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="b32b8-111">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="b32b8-111">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b32b8-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="b32b8-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="b32b8-113">Razor Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="b32b8-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b32b8-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b32b8-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b32b8-115">Visual Studio の **[ファイル]** メニューから、**[新規作成]** > **[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="b32b8-116">プロジェクトに名前を**WebApp1**にする場合の名前空間と一致、[サンプルをダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample)コード。</span><span class="sxs-lookup"><span data-stu-id="b32b8-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) code.</span></span>
* <span data-ttu-id="b32b8-117">選択**ASP.NET Core Web アプリケーション** > **OK**</span><span class="sxs-lookup"><span data-stu-id="b32b8-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="b32b8-118">選択**ASP.NET Core 2.1**ドロップダウン</span><span class="sxs-lookup"><span data-stu-id="b32b8-118">Select **ASP.NET Core 2.1** in the dropdown</span></span>
* <span data-ttu-id="b32b8-119">選択**Web アプリケーション**  > **OK**</span><span class="sxs-lookup"><span data-stu-id="b32b8-119">Select **Web Application**  > **OK**</span></span>
* <span data-ttu-id="b32b8-120">プロジェクトをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-120">Build and run the project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b32b8-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b32b8-121">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="b32b8-122">Identity scaffolder を実行します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-122">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b32b8-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b32b8-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b32b8-124">**ソリューション エクスプ ローラー**、プロジェクトを右クリックして >**追加** > **スキャフォールディングされた新しい項目**します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-124">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="b32b8-125">左側のウィンドウから、**スキャフォールディングの追加**ダイアログ ボックスで、 **Identity** > **追加**します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-125">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="b32b8-126">**ADD アイデンティティ**ダイアログ ボックスで、次のオプション。</span><span class="sxs-lookup"><span data-stu-id="b32b8-126">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="b32b8-127">既存のレイアウト ファイルを選択して *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="b32b8-127">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="b32b8-128">オーバーライドする次のファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-128">Select the following files to override:</span></span>
    * <span data-ttu-id="b32b8-129">**アカウントまたは登録**</span><span class="sxs-lookup"><span data-stu-id="b32b8-129">**Account/Register**</span></span>
    * <span data-ttu-id="b32b8-130">**アカウント/管理/インデックス**</span><span class="sxs-lookup"><span data-stu-id="b32b8-130">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="b32b8-131">選択、 **+** 新たに作成するボタン**データ コンテキスト クラス**します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-131">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="b32b8-132">型を受け入れる (**WebApp1.Models.WebApp1Context**場合は、プロジェクトの名前は**WebApp1**)。</span><span class="sxs-lookup"><span data-stu-id="b32b8-132">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="b32b8-133">選択、 **+** 新たに作成するボタン**ユーザー クラス**します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-133">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="b32b8-134">型を受け入れる (**WebApp1User**場合は、プロジェクトの名前は**WebApp1**) >**追加**します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-134">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="b32b8-135">選択**追加**します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-135">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b32b8-136">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b32b8-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b32b8-137">ASP.NET Core scaffolder を以前インストールしていない場合は、今すぐインストールします。</span><span class="sxs-lookup"><span data-stu-id="b32b8-137">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="b32b8-138">パッケージ参照を追加[Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)プロジェクト (.csproj) ファイル。</span><span class="sxs-lookup"><span data-stu-id="b32b8-138">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="b32b8-139">プロジェクト ディレクトリに、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-139">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="b32b8-140">Identity scaffolder オプションを一覧表示するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-140">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="b32b8-141">プロジェクト フォルダーでは、Identity scaffolder を実行します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-141">In the project folder, run the Identity scaffolder:</span></span>

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

<span data-ttu-id="b32b8-142">指示に従って、[移行、UseAuthentication、およびレイアウト](xref:security/authentication/scaffold-identity#efm)次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-142">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="b32b8-143">移行を作成し、データベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-143">Create a migration and update the database.</span></span>
* <span data-ttu-id="b32b8-144">`UseAuthentication` に `Startup.Configure` を追加します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-144">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="b32b8-145">追加`<partial name="_LoginPartial" />`レイアウト ファイルにします。</span><span class="sxs-lookup"><span data-stu-id="b32b8-145">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="b32b8-146">アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="b32b8-146">Test the app:</span></span>
  * <span data-ttu-id="b32b8-147">ユーザーを登録する</span><span class="sxs-lookup"><span data-stu-id="b32b8-147">Register a user</span></span>
  * <span data-ttu-id="b32b8-148">新しいユーザー名を選択します (次に、**ログアウト**リンク)。</span><span class="sxs-lookup"><span data-stu-id="b32b8-148">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="b32b8-149">ウィンドウを拡大またはユーザー名とその他のリンクを表示するナビゲーション バーのアイコンを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b32b8-149">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="b32b8-150">選択、**個人データ**タブ。</span><span class="sxs-lookup"><span data-stu-id="b32b8-150">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="b32b8-151">選択、**ダウンロード**ボタンをクリックし、調査、 *PersonalData.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b32b8-151">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="b32b8-152">テスト、**削除**ボタンで、ユーザーのログオンを削除します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-152">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="b32b8-153">Identity DB へのカスタム ユーザー データを追加します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-153">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="b32b8-154">更新プログラム、`IdentityUser`カスタム プロパティを持つクラスを派生します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-154">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="b32b8-155">ファイルの名前は WebApp1 プロジェクトの名前を付けた場合*Areas/Identity/Data/WebApp1User.cs*します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-155">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="b32b8-156">次のコード ファイルを更新します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-156">Update the file with the following code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

<span data-ttu-id="b32b8-157">修飾されたプロパティ、 [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1)属性には。</span><span class="sxs-lookup"><span data-stu-id="b32b8-157">Properties decorated with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute are:</span></span>

* <span data-ttu-id="b32b8-158">削除されたときに、 *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor ページを呼び出して`UserManager.Delete`します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-158">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="b32b8-159">によってデータのダウンロードに含まれる、 *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor ページ。</span><span class="sxs-lookup"><span data-stu-id="b32b8-159">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="b32b8-160">Account/Manage/Index.cshtml ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-160">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="b32b8-161">更新プログラム、`InputModel`で*Areas/Identity/Pages/Account/Manage/Index.cshtml.cs*次のようにコードを強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="b32b8-161">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95,120)]

<span data-ttu-id="b32b8-162">更新プログラム、 *Areas/Identity/Pages/Account/Manage/Index.cshtml*を次の強調表示されているマークアップ。</span><span class="sxs-lookup"><span data-stu-id="b32b8-162">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="b32b8-163">Account/Register.cshtml ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-163">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="b32b8-164">更新プログラム、`InputModel`で*Areas/Identity/Pages/Account/Register.cshtml.cs*次のようにコードを強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="b32b8-164">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

<span data-ttu-id="b32b8-165">更新プログラム、 *Areas/Identity/Pages/Account/Register.cshtml*を次の強調表示されているマークアップ。</span><span class="sxs-lookup"><span data-stu-id="b32b8-165">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

<span data-ttu-id="b32b8-166">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="b32b8-166">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="b32b8-167">カスタム ユーザー データの移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-167">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b32b8-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b32b8-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b32b8-169">Visual Studio で**パッケージ マネージャー コンソール**:</span><span class="sxs-lookup"><span data-stu-id="b32b8-169">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b32b8-170">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b32b8-170">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="b32b8-171">テストを作成、表示、ダウンロード、カスタム ユーザー データの削除</span><span class="sxs-lookup"><span data-stu-id="b32b8-171">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="b32b8-172">アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="b32b8-172">Test the app:</span></span>

* <span data-ttu-id="b32b8-173">新しいユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="b32b8-173">Register a new user.</span></span>
* <span data-ttu-id="b32b8-174">カスタムのユーザー データを表示、`/Identity/Account/Manage`ページ。</span><span class="sxs-lookup"><span data-stu-id="b32b8-174">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="b32b8-175">ダウンロードしてから、ユーザーの個人データを表示、`/Identity/Account/Manage/PersonalData`ページ。</span><span class="sxs-lookup"><span data-stu-id="b32b8-175">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
