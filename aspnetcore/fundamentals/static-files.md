---
title: "ASP.NET Core の静的ファイルを操作します。"
author: rick-anderson
description: "サービスを提供し、静的なファイルをセキュリティで保護および ASP.NET Core web アプリでのミドルウェアの動作をホストしている静的ファイルを構成する方法を説明します。"
keywords: "ASP.NET Core、静的なファイル、HTML、CSS、JavaScript の静的なアセット"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 912923860939a1d1dd91ccc79862e23f9095d161
ms.sourcegitcommit: a3e88639a6bcf8fb4d634036dac93130c464a097
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a><span data-ttu-id="36998-104">ASP.NET Core の静的ファイルを操作します。</span><span class="sxs-lookup"><span data-stu-id="36998-104">Work with static files in ASP.NET Core</span></span>

<span data-ttu-id="36998-105">によって[Rick Anderson](https://twitter.com/RickAndMSFT)と[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="36998-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="36998-106">HTML、CSS、画像、および、JavaScript などの静的ファイルは、資産の ASP.NET Core アプリケーションが直接クライアントに機能します。</span><span class="sxs-lookup"><span data-stu-id="36998-106">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="36998-107">いくつかの構成は、これらのファイルの提供を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="36998-107">Some configuration is required to enable to serving of these files.</span></span>

<span data-ttu-id="36998-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="36998-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="36998-109">静的ファイルを提供します</span><span class="sxs-lookup"><span data-stu-id="36998-109">Serve static files</span></span>

<span data-ttu-id="36998-110">静的ファイルは、プロジェクトの web ルート ディレクトリ内に格納されます。</span><span class="sxs-lookup"><span data-stu-id="36998-110">Static files are stored within your project's web root directory.</span></span> <span data-ttu-id="36998-111">既定のディレクトリは *\<content_root >/wwwroot*を使用して変更できますが、 [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_)メソッドです。</span><span class="sxs-lookup"><span data-stu-id="36998-111">The default directory is *\<content_root>/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="36998-112">参照してください[コンテンツのルート](xref:fundamentals/index#content-root)と[Web ルート](xref:fundamentals/index#web-root)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="36998-112">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span>

<span data-ttu-id="36998-113">アプリの web ホストは、コンテンツのルート ディレクトリの対応に行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="36998-113">The app's web host must be made aware of the content root directory.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="36998-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="36998-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="36998-115">`WebHost.CreateDefaultBuilder`メソッドは、現在のディレクトリへのコンテンツのルートを設定します。</span><span class="sxs-lookup"><span data-stu-id="36998-115">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="36998-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="36998-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="36998-117">呼び出すことによって、現在のディレクトリにコンテンツのルートを設定[UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_)内の`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="36998-117">Set the content root to the current directory by invoking [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) inside of `Program.Main`:</span></span>

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

<span data-ttu-id="36998-118">静的なファイルは、web ルートに対する相対パスを経由してアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="36998-118">Static files are accessible via a path relative to the web root.</span></span> <span data-ttu-id="36998-119">たとえば、 **Web アプリケーション**プロジェクト テンプレートには内でいくつかのフォルダーが含まれています、 *wwwroot*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="36998-119">For example, the **Web Application** project template contains several folders within the *wwwroot* folder:</span></span>

* <span data-ttu-id="36998-120">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="36998-120">**wwwroot**</span></span>
  * <span data-ttu-id="36998-121">**css**</span><span class="sxs-lookup"><span data-stu-id="36998-121">**css**</span></span>
  * <span data-ttu-id="36998-122">**images**</span><span class="sxs-lookup"><span data-stu-id="36998-122">**images**</span></span>
  * <span data-ttu-id="36998-123">**js**</span><span class="sxs-lookup"><span data-stu-id="36998-123">**js**</span></span>

<span data-ttu-id="36998-124">URI の形式でファイルへのアクセス、*イメージ*サブフォルダーは*http://\<server_address >/images/\<画像 >*です。</span><span class="sxs-lookup"><span data-stu-id="36998-124">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="36998-125">たとえば、 *http://localhost:9189/images/banner3.svg*です。</span><span class="sxs-lookup"><span data-stu-id="36998-125">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="36998-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="36998-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="36998-127">.NET Framework を対象にする場合は、追加、 [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/)をプロジェクトにパッケージします。</span><span class="sxs-lookup"><span data-stu-id="36998-127">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span> <span data-ttu-id="36998-128">.NET Core をターゲットとする場合、 [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage)このパッケージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="36998-128">If targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) includes this package.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="36998-129">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="36998-129">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="36998-130">追加、 [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/)をプロジェクトにパッケージします。</span><span class="sxs-lookup"><span data-stu-id="36998-130">Add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span>

---

<span data-ttu-id="36998-131">構成、[ミドルウェア](xref:fundamentals/middleware)静的ファイル サービスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="36998-131">Configure the [middleware](xref:fundamentals/middleware) which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="36998-132">Web ルート内でファイルを提供します</span><span class="sxs-lookup"><span data-stu-id="36998-132">Serve files inside of web root</span></span>

<span data-ttu-id="36998-133">呼び出す、 [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)メソッド内で`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="36998-133">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="36998-134">パラメーターなし`UseStaticFiles`メソッドのオーバー ロードが含んだとして web ルート内のファイルをマークします。</span><span class="sxs-lookup"><span data-stu-id="36998-134">The parameterless `UseStaticFiles` method overload marks the files in web root as servable.</span></span> <span data-ttu-id="36998-135">次のマークアップ参照*wwwroot/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="36998-135">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="36998-136">Web ルートの外部でファイルを提供します</span><span class="sxs-lookup"><span data-stu-id="36998-136">Serve files outside of web root</span></span>

<span data-ttu-id="36998-137">Web ルートの外部で処理される静的ファイルが存在するディレクトリの階層を考慮してください。</span><span class="sxs-lookup"><span data-stu-id="36998-137">Consider a directory hierarchy in which the static files to be served reside outside of the web root:</span></span>

* <span data-ttu-id="36998-138">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="36998-138">**wwwroot**</span></span>
  * <span data-ttu-id="36998-139">**css**</span><span class="sxs-lookup"><span data-stu-id="36998-139">**css**</span></span>
  * <span data-ttu-id="36998-140">**images**</span><span class="sxs-lookup"><span data-stu-id="36998-140">**images**</span></span>
  * <span data-ttu-id="36998-141">**js**</span><span class="sxs-lookup"><span data-stu-id="36998-141">**js**</span></span>
* <span data-ttu-id="36998-142">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="36998-142">**MyStaticFiles**</span></span>
  * <span data-ttu-id="36998-143">**images**</span><span class="sxs-lookup"><span data-stu-id="36998-143">**images**</span></span>
      * <span data-ttu-id="36998-144">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="36998-144">*banner1.svg*</span></span>

<span data-ttu-id="36998-145">要求にアクセスできる、 *banner1.svg*静的ファイル ミドルウェアを次のように構成することによってファイル。</span><span class="sxs-lookup"><span data-stu-id="36998-145">A request can access the *banner1.svg* file by configuring the static file middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="36998-146">上記のコードでは、 *MyStaticFiles*を介してディレクトリ階層をパブリックに公開される、 *StaticFiles* URI セグメント。</span><span class="sxs-lookup"><span data-stu-id="36998-146">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="36998-147">要求を*http://\<server_address >/StaticFiles/images/banner1.svg*機能、 *banner1.svg*ファイル。</span><span class="sxs-lookup"><span data-stu-id="36998-147">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="36998-148">次のマークアップ参照*MyStaticFiles/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="36998-148">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="36998-149">HTTP 応答ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="36998-149">Set HTTP response headers</span></span>

<span data-ttu-id="36998-150">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions)を HTTP 応答ヘッダーを設定するオブジェクトを使用できます。</span><span class="sxs-lookup"><span data-stu-id="36998-150">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="36998-151">に加えて、web のルートからの静的なファイル サーバーを構成するには、次のコード セット、`Cache-Control`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="36998-151">In addition to configuring static file serving from the web root, the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="36998-152">[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append)にメソッドが存在する、 [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/)パッケージです。</span><span class="sxs-lookup"><span data-stu-id="36998-152">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="36998-153">ファイルが加えられましたキャッシュ可能なパブリックに 10 分 (600 秒)。</span><span class="sxs-lookup"><span data-stu-id="36998-153">The files have been made publicly cacheable for 10 minutes (600 seconds):</span></span>

![Cache-control ヘッダーを示す応答ヘッダーが追加されました](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="36998-155">静的ファイルの承認</span><span class="sxs-lookup"><span data-stu-id="36998-155">Static file authorization</span></span>

<span data-ttu-id="36998-156">静的ファイル ミドルウェアは、承認チェックを提供していません。</span><span class="sxs-lookup"><span data-stu-id="36998-156">The static file middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="36998-157">すべてのファイル サービスを提供し、下にあるものを含む*wwwroot*はパブリックにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="36998-157">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="36998-158">ファイルを提供するには、承認に基づいています。</span><span class="sxs-lookup"><span data-stu-id="36998-158">To serve files based on authorization:</span></span>

* <span data-ttu-id="36998-159">外部に保存する*wwwroot*と静的ファイル ミドルウェアにアクセスできる任意のディレクトリ**と**</span><span class="sxs-lookup"><span data-stu-id="36998-159">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>
* <span data-ttu-id="36998-160">承認を適用するアクション メソッドを使用して、それらを提供します。</span><span class="sxs-lookup"><span data-stu-id="36998-160">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="36998-161">返す、 [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult)オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="36998-161">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="36998-162">ディレクトリの参照を有効にします。</span><span class="sxs-lookup"><span data-stu-id="36998-162">Enable directory browsing</span></span>

<span data-ttu-id="36998-163">により、ディレクトリの一覧を表示する web アプリのユーザーと指定したディレクトリ内にあるファイルをディレクトリの参照します。</span><span class="sxs-lookup"><span data-stu-id="36998-163">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="36998-164">既定ではセキュリティ上の理由からディレクトリの参照が無効になっています (を参照してください[に関する考慮事項](#considerations))。</span><span class="sxs-lookup"><span data-stu-id="36998-164">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="36998-165">有効にするディレクトリの参照を呼び出すことによって、 [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_)メソッド`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="36998-165">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="36998-166">呼び出すことによって必要なサービスを追加、 [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_)メソッドから`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="36998-166">Add required services by invoking the [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="36998-167">上記のコードにより、ディレクトリの参照の*wwwroot/イメージ*フォルダーの URL を使用して*http://\<server_address >/により*、各ファイルおよびフォルダーへのリンク。</span><span class="sxs-lookup"><span data-stu-id="36998-167">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![ディレクトリの参照](static-files/_static/dir-browse.png)

<span data-ttu-id="36998-169">参照してください[に関する考慮事項](#considerations)ブラウズを有効にする場合は、セキュリティ上のリスクにします。</span><span class="sxs-lookup"><span data-stu-id="36998-169">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="36998-170">2 つに注意してください`UseStaticFiles`は次の例で呼び出します。</span><span class="sxs-lookup"><span data-stu-id="36998-170">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="36998-171">最初の呼び出しにより、内の静的ファイルの提供、 *wwwroot*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="36998-171">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="36998-172">2 番目の呼び出しにより、ディレクトリの参照の*wwwroot/イメージ*フォルダーの URL を使用して*http://\<server_address >/により*:</span><span class="sxs-lookup"><span data-stu-id="36998-172">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="36998-173">既定のドキュメントを提供します。</span><span class="sxs-lookup"><span data-stu-id="36998-173">Serve a default document</span></span>

<span data-ttu-id="36998-174">既定のホーム ページの設定を提供訪問者論理の開始点は、サイトを訪問するとします。</span><span class="sxs-lookup"><span data-stu-id="36998-174">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="36998-175">既定のページは、URI の完全修飾ユーザーなし機能を呼び出して、 [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)メソッドから`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="36998-175">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="36998-176">`UseDefaultFiles`前に呼び出す必要があります`UseStaticFiles`を既定のファイルを提供します。</span><span class="sxs-lookup"><span data-stu-id="36998-176">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="36998-177">`UseDefaultFiles`実際には、ファイルに貢献しない URL リライターは、します。</span><span class="sxs-lookup"><span data-stu-id="36998-177">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="36998-178">使用して、静的ファイル ミドルウェアを有効にする`UseStaticFiles`にファイルを提供します。</span><span class="sxs-lookup"><span data-stu-id="36998-178">Enable the static file middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="36998-179">`UseDefaultFiles`フォルダーを検索する要求。</span><span class="sxs-lookup"><span data-stu-id="36998-179">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="36998-180">*default.htm*</span><span class="sxs-lookup"><span data-stu-id="36998-180">*default.htm*</span></span>
* <span data-ttu-id="36998-181">*default.html*</span><span class="sxs-lookup"><span data-stu-id="36998-181">*default.html*</span></span>
* <span data-ttu-id="36998-182">*index.htm*</span><span class="sxs-lookup"><span data-stu-id="36998-182">*index.htm*</span></span>
* <span data-ttu-id="36998-183">*index.html*</span><span class="sxs-lookup"><span data-stu-id="36998-183">*index.html*</span></span>

<span data-ttu-id="36998-184">一覧から見つかった最初のファイルは、要求が完全修飾 URI であるかのように処理されます。</span><span class="sxs-lookup"><span data-stu-id="36998-184">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="36998-185">ブラウザーの URL は、要求された URI を反映するように続行します。</span><span class="sxs-lookup"><span data-stu-id="36998-185">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="36998-186">次のコードを既定のファイル名を変更する*mydefault.html*:</span><span class="sxs-lookup"><span data-stu-id="36998-186">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="36998-187">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="36998-187">UseFileServer</span></span>

<span data-ttu-id="36998-188">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_)の機能を結合`UseStaticFiles`、 `UseDefaultFiles`、および`UseDirectoryBrowser`です。</span><span class="sxs-lookup"><span data-stu-id="36998-188">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="36998-189">次のコードでは、静的なファイルおよび既定のファイルのサービスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="36998-189">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="36998-190">ディレクトリの参照が有効になっています。</span><span class="sxs-lookup"><span data-stu-id="36998-190">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="36998-191">次のコードは、ディレクトリの参照を有効にするによって、パラメーターなしのオーバー ロード時に作成されます。</span><span class="sxs-lookup"><span data-stu-id="36998-191">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="36998-192">次のディレクトリ階層を考慮してください。</span><span class="sxs-lookup"><span data-stu-id="36998-192">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="36998-193">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="36998-193">**wwwroot**</span></span>
  * <span data-ttu-id="36998-194">**css**</span><span class="sxs-lookup"><span data-stu-id="36998-194">**css**</span></span>
  * <span data-ttu-id="36998-195">**images**</span><span class="sxs-lookup"><span data-stu-id="36998-195">**images**</span></span>
  * <span data-ttu-id="36998-196">**js**</span><span class="sxs-lookup"><span data-stu-id="36998-196">**js**</span></span>
* <span data-ttu-id="36998-197">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="36998-197">**MyStaticFiles**</span></span>
  * <span data-ttu-id="36998-198">**images**</span><span class="sxs-lookup"><span data-stu-id="36998-198">**images**</span></span>
      * <span data-ttu-id="36998-199">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="36998-199">*banner1.svg*</span></span>
  * <span data-ttu-id="36998-200">*default.html*</span><span class="sxs-lookup"><span data-stu-id="36998-200">*default.html*</span></span>

<span data-ttu-id="36998-201">次のコードにより、静的なファイル、既定のファイルおよびディレクトリの参照の`MyStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="36998-201">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="36998-202">`AddDirectoryBrowser`呼び出す必要がある場合に、`EnableDirectoryBrowsing`プロパティの値が`true`:</span><span class="sxs-lookup"><span data-stu-id="36998-202">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="36998-203">ファイル階層を使用して、前のコード、Url を解決するには、次のように。</span><span class="sxs-lookup"><span data-stu-id="36998-203">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="36998-204">URI</span><span class="sxs-lookup"><span data-stu-id="36998-204">URI</span></span>            |                             <span data-ttu-id="36998-205">応答</span><span class="sxs-lookup"><span data-stu-id="36998-205">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="36998-206">*http://\<server_address>/StaticFiles/images/banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="36998-206">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="36998-207">MyStaticFiles/images/banner1.svg</span><span class="sxs-lookup"><span data-stu-id="36998-207">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="36998-208">*http://\<server_address>/StaticFiles*</span><span class="sxs-lookup"><span data-stu-id="36998-208">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="36998-209">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="36998-209">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="36998-210">既定の名前のファイルが存在しない場合、 *MyStaticFiles*ディレクトリ、 *http://\<server_address >/StaticFiles*ディレクトリでクリック可能なリンクの一覧を返します。</span><span class="sxs-lookup"><span data-stu-id="36998-210">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![静的ファイルの一覧](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="36998-212">`UseDefaultFiles`および`UseDirectoryBrowser`URL を使用して*http://\<server_address >/StaticFiles* 、末尾のスラッシュをクライアント側をトリガーせずにリダイレクト*http://\<server_address >/StaticFiles/*です。</span><span class="sxs-lookup"><span data-stu-id="36998-212">`UseDefaultFiles` and `UseDirectoryBrowser` use the URL *http://\<server_address>/StaticFiles* without the trailing slash to trigger a client-side redirect to *http://\<server_address>/StaticFiles/*.</span></span> <span data-ttu-id="36998-213">末尾のスラッシュの追加を確認します。</span><span class="sxs-lookup"><span data-stu-id="36998-213">Notice the addition of the trailing slash.</span></span> <span data-ttu-id="36998-214">ドキュメント内の相対 Url は、末尾のスラッシュは無効です。 と考えられます。</span><span class="sxs-lookup"><span data-stu-id="36998-214">Relative URLs within the documents are deemed invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="36998-215">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="36998-215">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="36998-216">[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider)クラスが含まれています、`Mappings`プロパティの MIME コンテンツの種類にファイル拡張子のマッピングとして機能します。</span><span class="sxs-lookup"><span data-stu-id="36998-216">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="36998-217">次の例では、いくつかのファイル拡張子が既知の MIME の種類に登録されます。</span><span class="sxs-lookup"><span data-stu-id="36998-217">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="36998-218">*.Rtf*拡張機能を置き換え、および*.mp4*を削除します。</span><span class="sxs-lookup"><span data-stu-id="36998-218">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="36998-219">参照してください[MIME コンテンツ タイプ](http://www.iana.org/assignments/media-types/media-types.xhtml)です。</span><span class="sxs-lookup"><span data-stu-id="36998-219">See [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="36998-220">非標準のコンテンツの種類</span><span class="sxs-lookup"><span data-stu-id="36998-220">Non-standard content types</span></span>

<span data-ttu-id="36998-221">静的ファイル ミドルウェアは、ほぼ 400 の既知のファイル コンテンツの種類を認識しています。</span><span class="sxs-lookup"><span data-stu-id="36998-221">The static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="36998-222">ユーザーは、不明なファイル種類のファイルを要求している場合、静的ファイル ミドルウェアは、HTTP 404 (Not Found) 応答を返します。</span><span class="sxs-lookup"><span data-stu-id="36998-222">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not Found) response.</span></span> <span data-ttu-id="36998-223">ディレクトリの参照が有効になっている場合、ファイルへのリンクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="36998-223">If directory browsing is enabled, a link to the file is displayed.</span></span> <span data-ttu-id="36998-224">URI には、HTTP 404 エラーが返されます。</span><span class="sxs-lookup"><span data-stu-id="36998-224">The URI returns an HTTP 404 error.</span></span>

<span data-ttu-id="36998-225">次のコードでは、不明な型の機能を有効にし、イメージとして不明なファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="36998-225">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="36998-226">上記のコードでは、不明なコンテンツの種類のファイルに対する要求は、イメージとして返されます。</span><span class="sxs-lookup"><span data-stu-id="36998-226">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="36998-227">有効にする[ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes)セキュリティ上のリスクです。</span><span class="sxs-lookup"><span data-stu-id="36998-227">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="36998-228">既定では、無効になっているとその使用はお勧めします。</span><span class="sxs-lookup"><span data-stu-id="36998-228">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="36998-229">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider)非標準の拡張子を持つファイルを処理するより安全な代替手段を提供します。</span><span class="sxs-lookup"><span data-stu-id="36998-229">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="36998-230">注意事項</span><span class="sxs-lookup"><span data-stu-id="36998-230">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="36998-231">`UseDirectoryBrowser`および`UseStaticFiles`機密データがリークすることができます。</span><span class="sxs-lookup"><span data-stu-id="36998-231">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="36998-232">無効にするディレクトリの実稼働環境で参照することを強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="36998-232">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="36998-233">使用して有効にするディレクトリを注意深く確認`UseStaticFiles`または`UseDirectoryBrowser`です。</span><span class="sxs-lookup"><span data-stu-id="36998-233">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="36998-234">全体のディレクトリとそのサブディレクトリにパブリックにアクセスできるようになります。</span><span class="sxs-lookup"><span data-stu-id="36998-234">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="36998-235">ストア ファイルをパブリック サービスに適した専用のディレクトリによう *\<content_root >/wwwroot*です。</span><span class="sxs-lookup"><span data-stu-id="36998-235">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="36998-236">MVC ビュー、構成ファイルなど、Razor ページ (2.x のみ)、これらのファイルと区別されます。</span><span class="sxs-lookup"><span data-stu-id="36998-236">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="36998-237">Url で公開されているコンテンツを`UseDirectoryBrowser`と`UseStaticFiles`は大文字小文字の区別および基になるファイル システムの文字の制限に従って提供します。</span><span class="sxs-lookup"><span data-stu-id="36998-237">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="36998-238">たとえば、Windows は大文字小文字を区別&mdash;Mac と Linux されません。</span><span class="sxs-lookup"><span data-stu-id="36998-238">For example, Windows is case insensitive&mdash;Mac and Linux aren't.</span></span>

* <span data-ttu-id="36998-239">ASP.NET Core アプリケーションの使用の IIS でホストされている、 [ASP.NET Core モジュール (ANCM)](xref:fundamentals/servers/aspnet-core-module)静的ファイルの要求を含む、アプリへのすべての要求を転送します。</span><span class="sxs-lookup"><span data-stu-id="36998-239">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module (ANCM)](xref:fundamentals/servers/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="36998-240">IIS の静的ファイル ハンドラーは使用されません。</span><span class="sxs-lookup"><span data-stu-id="36998-240">The IIS static file handler isn't used.</span></span> <span data-ttu-id="36998-241">ANCM で処理する前に、要求を処理する機会はありません。</span><span class="sxs-lookup"><span data-stu-id="36998-241">It has no chance to handle requests before they're handled by the ANCM.</span></span>

* <span data-ttu-id="36998-242">IIS マネージャーでサーバーまたは web サイト レベルで IIS の静的ファイル ハンドラーを削除する次の手順を実行するには。</span><span class="sxs-lookup"><span data-stu-id="36998-242">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="36998-243">移動し、**モジュール**機能します。</span><span class="sxs-lookup"><span data-stu-id="36998-243">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="36998-244">選択**: StaticFileModule**一覧にします。</span><span class="sxs-lookup"><span data-stu-id="36998-244">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="36998-245">をクリックして**削除**で、**アクション**サイドバーです。</span><span class="sxs-lookup"><span data-stu-id="36998-245">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="36998-246">IIS の静的ファイル ハンドラーが有効になっている場合**と**ANCM が正しく構成されていない、静的なファイルが処理されます。</span><span class="sxs-lookup"><span data-stu-id="36998-246">If the IIS static file handler is enabled **and** the ANCM is configured incorrectly, static files are served.</span></span> <span data-ttu-id="36998-247">これは、場合、たとえば、 *web.config*ファイルが配置されていません。</span><span class="sxs-lookup"><span data-stu-id="36998-247">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="36998-248">コード ファイルの配置 (含む*.cs*と*.cshtml*) アプリ プロジェクトの web ルートの外部でします。</span><span class="sxs-lookup"><span data-stu-id="36998-248">Place code files (including *.cs* and *.cshtml*) outside of the app project's web root.</span></span> <span data-ttu-id="36998-249">そのため、論理的な分離がアプリのクライアント側のコンテンツとサーバー側コード間で作成されます。</span><span class="sxs-lookup"><span data-stu-id="36998-249">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="36998-250">これはサーバー側コードが漏洩することを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="36998-250">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="36998-251">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="36998-251">Additional resources</span></span>

* [<span data-ttu-id="36998-252">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="36998-252">Middleware</span></span>](xref:fundamentals/middleware)

* [<span data-ttu-id="36998-253">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="36998-253">Introduction to ASP.NET Core</span></span>](xref:index)