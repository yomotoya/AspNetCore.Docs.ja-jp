---
title: ASP.NET Core プロジェクトでスキャフォールディング Id
author: rick-anderson
description: ASP.NET Core プロジェクトでの Id をスキャフォールディングする方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 07163941d0bd1fea6f9b3d9867536580d8a9e9d8
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063274"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="03c3b-103">ASP.NET Core プロジェクトでスキャフォールディング Id</span><span class="sxs-lookup"><span data-stu-id="03c3b-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="03c3b-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="03c3b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="03c3b-105">ASP.NET Core 2.1 以降では[ASP.NET Core Identity](xref:security/authentication/identity)として、 [Razor クラス ライブラリ](xref:razor-pages/ui-class)します。</span><span class="sxs-lookup"><span data-stu-id="03c3b-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="03c3b-106">Id を含むアプリケーションでは、選択的に Identity Razor クラス ライブラリ (RCL) に含まれているソース コードを追加する scaffolder を適用できます。</span><span class="sxs-lookup"><span data-stu-id="03c3b-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="03c3b-107">コードを変更して動作を変更できるように、ソース コードを生成できます。</span><span class="sxs-lookup"><span data-stu-id="03c3b-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="03c3b-108">たとえば、登録で使用するコードを生成するようにスキャフォルダーに指示できます。</span><span class="sxs-lookup"><span data-stu-id="03c3b-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="03c3b-109">生成されたコードは、Identity RCL の同じコードよりも優先されます。</span><span class="sxs-lookup"><span data-stu-id="03c3b-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="03c3b-110">UI のフル コントロールを取得し、既定 RCL を使用しない、セクションをご覧ください。 [UI のソースの完全な id 作成](#full)です。</span><span class="sxs-lookup"><span data-stu-id="03c3b-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="03c3b-111">実行するアプリケーション**いない**含める認証 RCL Identity パッケージを追加する scaffolder を適用できます。</span><span class="sxs-lookup"><span data-stu-id="03c3b-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="03c3b-112">生成される Identity コードの選択オプションがあります。</span><span class="sxs-lookup"><span data-stu-id="03c3b-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="03c3b-113">スキャフォルダーは、必要なコードの大部分を生成するが、プロセスを完了するプロジェクトを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="03c3b-113">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="03c3b-114">このドキュメントでは、Id のスキャフォールディング更新の完了に必要な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="03c3b-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="03c3b-115">Identity scaffolder が実行される、 *ScaffoldingReadme.txt*プロジェクト ディレクトリにファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="03c3b-115">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="03c3b-116">*ScaffoldingReadme.txt*ファイルには、一般的な手順について Id スキャフォールディングの更新の完了に必要なものが含まれています。</span><span class="sxs-lookup"><span data-stu-id="03c3b-116">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="03c3b-117">このドキュメントより詳細な手順が含まれています、 *ScaffoldingReadme.txt*ファイル。</span><span class="sxs-lookup"><span data-stu-id="03c3b-117">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="03c3b-118">ファイルの相違点を示し、変更をバックアップすることができますをソース管理システムを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="03c3b-118">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="03c3b-119">Identity scaffolder を実行した後、変更を検査します。</span><span class="sxs-lookup"><span data-stu-id="03c3b-119">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="03c3b-120">空のプロジェクトにスキャフォールディング identity</span><span class="sxs-lookup"><span data-stu-id="03c3b-120">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="03c3b-121">次の強調表示されている呼び出しを追加、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="03c3b-121">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="03c3b-122">既存の承認なし Razor プロジェクトにスキャフォールディング identity</span><span class="sxs-lookup"><span data-stu-id="03c3b-122">Scaffold identity into a Razor project without existing authorization</span></span>

<!--
set projNam=RPnoAuth
set projType=razor
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="03c3b-123">Id が構成されて*Areas/Identity/IdentityHostingStartup.cs*します。</span><span class="sxs-lookup"><span data-stu-id="03c3b-123">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="03c3b-124">詳細については、次を参照してください。 [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)します。</span><span class="sxs-lookup"><span data-stu-id="03c3b-124">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="03c3b-125">移行、UseAuthentication、およびレイアウト</span><span class="sxs-lookup"><span data-stu-id="03c3b-125">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="03c3b-126">認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="03c3b-126">Enable authentication</span></span>

<span data-ttu-id="03c3b-127">`Configure`のメソッド、`Startup`クラスを呼び出す[UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)後`UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="03c3b-127">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="03c3b-128">レイアウトの変更</span><span class="sxs-lookup"><span data-stu-id="03c3b-128">Layout changes</span></span>

<span data-ttu-id="03c3b-129">省略可能: 部分のログインを追加する (`_LoginPartial`)、レイアウト ファイルに。</span><span class="sxs-lookup"><span data-stu-id="03c3b-129">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="03c3b-130">権限を持つ Razor プロジェクトにスキャフォールディング identity</span><span class="sxs-lookup"><span data-stu-id="03c3b-130">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register

[!INCLUDE[](~/includes/webapp-alias-notice.md)]
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]<span data-ttu-id="03c3b-131"> いくつかの Id オプションが構成されている*Areas/Identity/IdentityHostingStartup.cs*します。</span><span class="sxs-lookup"><span data-stu-id="03c3b-131"> Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="03c3b-132">詳細については、次を参照してください。 [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration)します。</span><span class="sxs-lookup"><span data-stu-id="03c3b-132">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="03c3b-133">MVC プロジェクトの既存の承認なしにスキャフォールディング identity</span><span class="sxs-lookup"><span data-stu-id="03c3b-133">Scaffold identity into an MVC project without existing authorization</span></span>

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="03c3b-134">省略可能: 部分のログインを追加する (`_LoginPartial`) に、 *Views/Shared/_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="03c3b-134">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="03c3b-135">移動、 *Pages/Shared/_LoginPartial.cshtml*ファイルを*Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="03c3b-135">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="03c3b-136">Id が構成されて*Areas/Identity/IdentityHostingStartup.cs*します。</span><span class="sxs-lookup"><span data-stu-id="03c3b-136">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="03c3b-137">詳細については、IHostingStartup を参照してください。</span><span class="sxs-lookup"><span data-stu-id="03c3b-137">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="03c3b-138">呼び出す[UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)後`UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="03c3b-138">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="03c3b-139">MVC プロジェクトの承認にスキャフォールディング identity</span><span class="sxs-lookup"><span data-stu-id="03c3b-139">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="03c3b-140">削除、*ページ/共有*フォルダーとそのフォルダー内のファイル。</span><span class="sxs-lookup"><span data-stu-id="03c3b-140">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="03c3b-141">完全な id の UI のソースを作成します。</span><span class="sxs-lookup"><span data-stu-id="03c3b-141">Create full identity UI source</span></span>

<span data-ttu-id="03c3b-142">Identity UI を完全に制御を維持するために、Identity scaffolder を実行して選択して**すべてのファイルを上書き**します。</span><span class="sxs-lookup"><span data-stu-id="03c3b-142">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="03c3b-143">次の強調表示されたコードは、ASP.NET Core 2.1 の web アプリで Id を持つ既定の Identity の UI を置換する変更を示しています。</span><span class="sxs-lookup"><span data-stu-id="03c3b-143">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="03c3b-144">これは、Identity UI の完全に制御をする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="03c3b-144">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="03c3b-145">既定の Id は、次のコードに置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="03c3b-145">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="03c3b-146">次のコード セット、 [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath)、 [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)、および[AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="03c3b-146">The following the code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="03c3b-147">登録、`IEmailSender`例については、実装します。</span><span class="sxs-lookup"><span data-stu-id="03c3b-147">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]
