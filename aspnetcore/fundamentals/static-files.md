---
title: "ASP.NET Core での静的ファイルの操作"
author: rick-anderson
description: "ASP.NET Core の静的ファイルを操作する方法を説明します。"
keywords: "ASP.NET Core、静的なファイル、HTML、CSS、JavaScript の静的なアセット"
ms.author: riande
manager: wpickett
ms.date: 4/07/2017
ms.topic: article
ms.assetid: e32245c7-4eee-4831-bd2e-915dbf9f5f70
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/static-files
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0751576a1391f26f045c3f8c42ea39c0ff6e5d9
ms.sourcegitcommit: e4fb6b13be56a0fb2f2778623740a047d6489227
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2017
---
# <a name="working-with-static-files-in-aspnet-core"></a><span data-ttu-id="09d35-104">ASP.NET Core での静的ファイルの操作</span><span class="sxs-lookup"><span data-stu-id="09d35-104">Working with static files in ASP.NET Core</span></span>

<a name="fundamentals-static-files"></a>

<span data-ttu-id="09d35-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="09d35-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="09d35-106">HTML、CSS、画像、および、JavaScript などの静的ファイルは、ASP.NET Core アプリケーションが直接クライアントに使用できる資産です。</span><span class="sxs-lookup"><span data-stu-id="09d35-106">Static files, such as HTML, CSS, image, and JavaScript, are assets that an ASP.NET Core app can serve directly to clients.</span></span>

<span data-ttu-id="09d35-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="09d35-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="09d35-108">静的ファイルの提供</span><span class="sxs-lookup"><span data-stu-id="09d35-108">Serving static files</span></span>

<span data-ttu-id="09d35-109">静的ファイルにある、 `web root` (*\<コンテンツ ルート >/wwwroot*) フォルダー。</span><span class="sxs-lookup"><span data-stu-id="09d35-109">Static files are typically located in the `web root` (*\<content-root>/wwwroot*) folder.</span></span> <span data-ttu-id="09d35-110">参照してください[コンテンツのルート](xref:fundamentals/index#content-root)と[Web ルート](xref:fundamentals/index#web-root)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="09d35-110">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span> <span data-ttu-id="09d35-111">通常、現在のディレクトリのようにコンテンツのルートを設定できるように、プロジェクトの`web root`開発中に検出されます。</span><span class="sxs-lookup"><span data-stu-id="09d35-111">You generally set the content root to be the current directory so that your project's `web root` will be found while in development.</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]

<span data-ttu-id="09d35-112">静的なファイルは任意のフォルダーの下に置くことができます、`web root`し、そのルートへの相対パスを使用してアクセスします。</span><span class="sxs-lookup"><span data-stu-id="09d35-112">Static files can be stored in any folder under the `web root` and accessed with a relative path to that root.</span></span> <span data-ttu-id="09d35-113">たとえば、Visual Studio を使用して既定の Web アプリケーション プロジェクトを作成する場合があるいくつかのフォルダー内に作成された、 *wwwroot*フォルダー - *css*、*イメージ*、および*js*です。</span><span class="sxs-lookup"><span data-stu-id="09d35-113">For example, when you create a default Web application project using Visual Studio, there are several folders created within the *wwwroot*  folder - *css*, *images*, and *js*.</span></span> <span data-ttu-id="09d35-114">イメージにアクセスする URI、*イメージ*サブフォルダー。</span><span class="sxs-lookup"><span data-stu-id="09d35-114">The URI to access an image in the *images* subfolder:</span></span>

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

<span data-ttu-id="09d35-115">静的なファイルを提供するためには、構成する必要があります、[ミドルウェア](middleware.md)をパイプラインに静的なファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="09d35-115">In order for static files to be served, you must configure the [Middleware](middleware.md) to add static files to the pipeline.</span></span> <span data-ttu-id="09d35-116">依存関係を追加することによって静的ファイル ミドルウェアを構成することができます、 *Microsoft.AspNetCore.StaticFiles*パッケージ、プロジェクトと、呼び出し元に、`UseStaticFiles`から拡張メソッド`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="09d35-116">The static file middleware can be configured by adding a dependency on the *Microsoft.AspNetCore.StaticFiles* package to your project and then calling the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

<span data-ttu-id="09d35-117">`app.UseStaticFiles();`内のファイルは、 `web root` (*wwwroot*既定で) 含んだです。</span><span class="sxs-lookup"><span data-stu-id="09d35-117">`app.UseStaticFiles();` makes the files in `web root` (*wwwroot* by default) servable.</span></span> <span data-ttu-id="09d35-118">後で説明するその他のディレクトリの内容を含んだ方法`UseStaticFiles`です。</span><span class="sxs-lookup"><span data-stu-id="09d35-118">Later I'll show how to make other directory contents servable with `UseStaticFiles`.</span></span>

<span data-ttu-id="09d35-119">NuGet パッケージ"Microsoft.AspNetCore.StaticFiles"を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="09d35-119">You must include the NuGet package "Microsoft.AspNetCore.StaticFiles".</span></span>

> [!NOTE]
> <span data-ttu-id="09d35-120">`web root`既定値は、 *wwwroot*がディレクトリを設定できる、`web root`ディレクトリが`UseWebRoot`です。</span><span class="sxs-lookup"><span data-stu-id="09d35-120">`web root` defaults to the *wwwroot* directory, but you can set the `web root` directory with `UseWebRoot`.</span></span>

<span data-ttu-id="09d35-121">外部サービスを提供する静的ファイルがプロジェクトの階層があると、`web root`です。</span><span class="sxs-lookup"><span data-stu-id="09d35-121">Suppose you have a project hierarchy where the static files you wish to serve are outside the `web root`.</span></span> <span data-ttu-id="09d35-122">例:</span><span class="sxs-lookup"><span data-stu-id="09d35-122">For example:</span></span>

* <span data-ttu-id="09d35-123">wwwroot</span><span class="sxs-lookup"><span data-stu-id="09d35-123">wwwroot</span></span>
  * <span data-ttu-id="09d35-124">css</span><span class="sxs-lookup"><span data-stu-id="09d35-124">css</span></span>
  * <span data-ttu-id="09d35-125">イメージ</span><span class="sxs-lookup"><span data-stu-id="09d35-125">images</span></span>
  * <span data-ttu-id="09d35-126">...</span><span class="sxs-lookup"><span data-stu-id="09d35-126">...</span></span>
* <span data-ttu-id="09d35-127">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="09d35-127">MyStaticFiles</span></span>
  * <span data-ttu-id="09d35-128">test.png</span><span class="sxs-lookup"><span data-stu-id="09d35-128">test.png</span></span>

<span data-ttu-id="09d35-129">アクセス要求に対して*test.png*、静的ファイル ミドルウェアを次のように構成します。</span><span class="sxs-lookup"><span data-stu-id="09d35-129">For a request to access *test.png*, configure the static files middleware as follows:</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]

<span data-ttu-id="09d35-130">要求を`http://<app>/StaticFiles/test.png`果たす、 *test.png*ファイル。</span><span class="sxs-lookup"><span data-stu-id="09d35-130">A request to `http://<app>/StaticFiles/test.png` will serve the *test.png* file.</span></span>

<span data-ttu-id="09d35-131">`StaticFileOptions()`応答ヘッダーを設定できます。</span><span class="sxs-lookup"><span data-stu-id="09d35-131">`StaticFileOptions()` can set response headers.</span></span> <span data-ttu-id="09d35-132">たとえば、次のコード設定静的ファイルの提供から、 *wwwroot*フォルダーと設定、 `Cache-Control` 10 分 (600 秒) をパブリックにキャッシュできるようにするヘッダー。</span><span class="sxs-lookup"><span data-stu-id="09d35-132">For example, the code below sets up static file serving from the *wwwroot* folder and sets the `Cache-Control` header to make them publicly cacheable for 10 minutes (600 seconds):</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]

<span data-ttu-id="09d35-133">[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append)メソッドはから利用可能な[Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/)パッケージです。</span><span class="sxs-lookup"><span data-stu-id="09d35-133">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method is available from the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span> <span data-ttu-id="09d35-134">追加`using Microsoft.AspNetCore.Http;`を*csharp*ファイルの場合は、メソッドは使用できません。</span><span class="sxs-lookup"><span data-stu-id="09d35-134">Add `using Microsoft.AspNetCore.Http;` to your *csharp* file if the method is unavailable.</span></span>

![Cache-control ヘッダーを示す応答ヘッダーが追加されました](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="09d35-136">静的ファイルの承認</span><span class="sxs-lookup"><span data-stu-id="09d35-136">Static file authorization</span></span>

<span data-ttu-id="09d35-137">静的ファイル モジュールでは、**ありません**承認チェックします。</span><span class="sxs-lookup"><span data-stu-id="09d35-137">The static file module provides **no** authorization checks.</span></span> <span data-ttu-id="09d35-138">すべてのファイル サービスを提供し、下にあるものを含む*wwwroot*は一般に公開します。</span><span class="sxs-lookup"><span data-stu-id="09d35-138">Any files served by it, including those under *wwwroot* are publicly available.</span></span> <span data-ttu-id="09d35-139">ファイルを提供するには、承認に基づいています。</span><span class="sxs-lookup"><span data-stu-id="09d35-139">To serve files based on authorization:</span></span>

* <span data-ttu-id="09d35-140">外部に保存する*wwwroot*と静的ファイル ミドルウェアにアクセスできる任意のディレクトリ**と**</span><span class="sxs-lookup"><span data-stu-id="09d35-140">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>

* <span data-ttu-id="09d35-141">返す、コント ローラーのアクションを提供する`FileResult`承認が適用されています。</span><span class="sxs-lookup"><span data-stu-id="09d35-141">Serve them through a controller action, returning a `FileResult` where authorization is applied</span></span>

## <a name="enabling-directory-browsing"></a><span data-ttu-id="09d35-142">ディレクトリの参照を有効にします。</span><span class="sxs-lookup"><span data-stu-id="09d35-142">Enabling directory browsing</span></span>

<span data-ttu-id="09d35-143">ディレクトリの参照ディレクトリおよび指定したディレクトリ内のファイルの一覧を表示する web アプリのユーザーに許可します。</span><span class="sxs-lookup"><span data-stu-id="09d35-143">Directory browsing allows the user of your web app to see a list of directories and files within a specified directory.</span></span> <span data-ttu-id="09d35-144">既定ではセキュリティ上の理由からディレクトリの参照が無効になっています (を参照してください[に関する考慮事項](#considerations))。</span><span class="sxs-lookup"><span data-stu-id="09d35-144">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="09d35-145">ディレクトリの参照を有効にするを呼び出して、`UseDirectoryBrowser`から拡張メソッド`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="09d35-145">To enable directory browsing, call the `UseDirectoryBrowser` extension method from  `Startup.Configure`:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]

<span data-ttu-id="09d35-146">必要なサービスを呼び出すことによって追加および`AddDirectoryBrowser`から拡張メソッド`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="09d35-146">And add required services by calling `AddDirectoryBrowser` extension method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]

<span data-ttu-id="09d35-147">上記のコードにより、ディレクトリの参照の*wwwroot/イメージ*URL http:// を使用してフォルダー\<アプリ >/により、各ファイルおよびフォルダーへのリンク。</span><span class="sxs-lookup"><span data-stu-id="09d35-147">The code above allows directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages, with links to each file and folder:</span></span>

![ディレクトリの参照](static-files/_static/dir-browse.png)

<span data-ttu-id="09d35-149">参照してください[に関する考慮事項](#considerations)ブラウズを有効にする場合は、セキュリティ上のリスクにします。</span><span class="sxs-lookup"><span data-stu-id="09d35-149">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="09d35-150">2 つに注意してください`app.UseStaticFiles`呼び出しです。</span><span class="sxs-lookup"><span data-stu-id="09d35-150">Note the two `app.UseStaticFiles` calls.</span></span> <span data-ttu-id="09d35-151">最初の 1 つが、CSS、画像とでの JavaScript を処理に必要な*wwwroot*フォルダー、およびディレクトリの参照のための 2 番目の呼び出し、 *wwwroot/イメージ*URL http:// を使用してフォルダー\<アプリ>/により。</span><span class="sxs-lookup"><span data-stu-id="09d35-151">The first one is required to serve the CSS, images and JavaScript in the *wwwroot* folder, and the second call for directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]

## <a name="serving-a-default-document"></a><span data-ttu-id="09d35-152">既定のドキュメントを提供しています。</span><span class="sxs-lookup"><span data-stu-id="09d35-152">Serving a default document</span></span>

<span data-ttu-id="09d35-153">サイトの訪問者にサイトを訪問するときに開始する場所は、既定のホーム ページを設定できます。</span><span class="sxs-lookup"><span data-stu-id="09d35-153">Setting a default home page gives site visitors a place to start when visiting your site.</span></span> <span data-ttu-id="09d35-154">Web アプリをユーザーが、URI の完全修飾する必要がなく、既定のページを処理するためには、呼び出し、`UseDefaultFiles`から拡張メソッド`Startup.Configure`次のようにします。</span><span class="sxs-lookup"><span data-stu-id="09d35-154">In order for your Web app to serve a default page without the user having to fully qualify the URI, call the `UseDefaultFiles` extension method from `Startup.Configure` as follows.</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]

> [!NOTE]
> <span data-ttu-id="09d35-155">`UseDefaultFiles`前に呼び出す必要があります`UseStaticFiles`を既定のファイルを提供します。</span><span class="sxs-lookup"><span data-stu-id="09d35-155">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="09d35-156">`UseDefaultFiles`実際には、ファイルに貢献しない URL 再ライターです。</span><span class="sxs-lookup"><span data-stu-id="09d35-156">`UseDefaultFiles` is a URL re-writer that doesn't actually serve the file.</span></span> <span data-ttu-id="09d35-157">静的ファイル ミドルウェアを有効にする必要があります (`UseStaticFiles`) ファイルを提供します。</span><span class="sxs-lookup"><span data-stu-id="09d35-157">You must enable the static file middleware (`UseStaticFiles`) to serve the file.</span></span>

<span data-ttu-id="09d35-158">`UseDefaultFiles`フォルダーへの要求が検索されます。</span><span class="sxs-lookup"><span data-stu-id="09d35-158">With `UseDefaultFiles`, requests to a folder will search for:</span></span>

* <span data-ttu-id="09d35-159">default.htm</span><span class="sxs-lookup"><span data-stu-id="09d35-159">default.htm</span></span>
* <span data-ttu-id="09d35-160">default.html</span><span class="sxs-lookup"><span data-stu-id="09d35-160">default.html</span></span>
* <span data-ttu-id="09d35-161">index.htm</span><span class="sxs-lookup"><span data-stu-id="09d35-161">index.htm</span></span>
* <span data-ttu-id="09d35-162">index.html</span><span class="sxs-lookup"><span data-stu-id="09d35-162">index.html</span></span>

<span data-ttu-id="09d35-163">要求が (ブラウザーの URL は、要求された URI を表示し続けます) には、完全修飾 URI が場合と、見つかった最初のファイルの一覧からに提供します。</span><span class="sxs-lookup"><span data-stu-id="09d35-163">The first file found from the list will be served as if the request was the fully qualified URI (although the browser URL will continue to show the URI requested).</span></span>

<span data-ttu-id="09d35-164">次のコードを既定のファイル名を変更する方法を示しています。 *mydefault.html*です。</span><span class="sxs-lookup"><span data-stu-id="09d35-164">The following code shows how to change the default file name to *mydefault.html*.</span></span>

[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]

## <a name="usefileserver"></a><span data-ttu-id="09d35-165">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="09d35-165">UseFileServer</span></span>

<span data-ttu-id="09d35-166">`UseFileServer`機能を結合`UseStaticFiles`、 `UseDefaultFiles`、および`UseDirectoryBrowser`です。</span><span class="sxs-lookup"><span data-stu-id="09d35-166">`UseFileServer` combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="09d35-167">次のコードによって、静的なファイルを提供する既定のファイルはディレクトリの参照を許可しません。</span><span class="sxs-lookup"><span data-stu-id="09d35-167">The following code enables static files and the default file to be served, but does not allow directory browsing:</span></span>

```csharp
app.UseFileServer();
   ```

<span data-ttu-id="09d35-168">次のコードには、静的なファイル、既定のファイルとディレクトリの参照が有効にします。</span><span class="sxs-lookup"><span data-stu-id="09d35-168">The following code enables static files, default files and  directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

<span data-ttu-id="09d35-169">参照してください[に関する考慮事項](#considerations)ブラウズを有効にする場合は、セキュリティ上のリスクにします。</span><span class="sxs-lookup"><span data-stu-id="09d35-169">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span> <span data-ttu-id="09d35-170">同様に`UseStaticFiles`、 `UseDefaultFiles`、および`UseDirectoryBrowser`外部に存在するファイルを提供する場合、`web root`のインスタンスを作成して構成する、`FileServerOptions`へのパラメーターとして渡されたオブジェクト`UseFileServer`です。</span><span class="sxs-lookup"><span data-stu-id="09d35-170">As with `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`, if you wish to serve files that exist outside the `web root`, you instantiate and configure an `FileServerOptions` object that you pass as a parameter to `UseFileServer`.</span></span> <span data-ttu-id="09d35-171">たとえば、Web アプリで、次のディレクトリ階層を指定します。</span><span class="sxs-lookup"><span data-stu-id="09d35-171">For example, given the following directory hierarchy in your Web app:</span></span>

* <span data-ttu-id="09d35-172">wwwroot</span><span class="sxs-lookup"><span data-stu-id="09d35-172">wwwroot</span></span>

  * <span data-ttu-id="09d35-173">css</span><span class="sxs-lookup"><span data-stu-id="09d35-173">css</span></span>

  * <span data-ttu-id="09d35-174">イメージ</span><span class="sxs-lookup"><span data-stu-id="09d35-174">images</span></span>

  * <span data-ttu-id="09d35-175">...</span><span class="sxs-lookup"><span data-stu-id="09d35-175">...</span></span>

* <span data-ttu-id="09d35-176">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="09d35-176">MyStaticFiles</span></span>

  * <span data-ttu-id="09d35-177">test.png</span><span class="sxs-lookup"><span data-stu-id="09d35-177">test.png</span></span>

  * <span data-ttu-id="09d35-178">default.html</span><span class="sxs-lookup"><span data-stu-id="09d35-178">default.html</span></span>

<span data-ttu-id="09d35-179">上記の階層の例を使用して、次のように静的なファイル、既定のファイルとの参照を有効にする可能性があります、`MyStaticFiles`ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="09d35-179">Using the hierarchy example above, you might want to enable static files, default files, and browsing for the `MyStaticFiles` directory.</span></span> <span data-ttu-id="09d35-180">次のコード スニペットで 1 回の呼び出しで実現を`FileServerOptions`です。</span><span class="sxs-lookup"><span data-stu-id="09d35-180">In the following code snippet, that is accomplished with a single call to `FileServerOptions`.</span></span>

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]

<span data-ttu-id="09d35-181">場合`enableDirectoryBrowsing`に設定されている`true`呼び出しに必要な`AddDirectoryBrowser`から拡張メソッド`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="09d35-181">If `enableDirectoryBrowsing` is set to `true` you are required to call `AddDirectoryBrowser` extension method from  `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]

<span data-ttu-id="09d35-182">ファイルの階層と上記のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="09d35-182">Using the file hierarchy and code above:</span></span>

| <span data-ttu-id="09d35-183">URI</span><span class="sxs-lookup"><span data-stu-id="09d35-183">URI</span></span>            |                             <span data-ttu-id="09d35-184">応答</span><span class="sxs-lookup"><span data-stu-id="09d35-184">Response</span></span>  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      <span data-ttu-id="09d35-185">MyStaticFiles/test.png</span><span class="sxs-lookup"><span data-stu-id="09d35-185">MyStaticFiles/test.png</span></span> |
| `http://<app>/StaticFiles`              |     <span data-ttu-id="09d35-186">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="09d35-186">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="09d35-187">既定の名前付きのファイルがない場合、 *MyStaticFiles*ディレクトリ、http://\<アプリ >/StaticFiles がディレクトリでクリック可能なリンクの一覧を返します。</span><span class="sxs-lookup"><span data-stu-id="09d35-187">If no default named files are in the *MyStaticFiles* directory, http://\<app>/StaticFiles returns the directory listing with clickable links:</span></span>

![静的ファイルの一覧](static-files/_static/db2.PNG)

> [!NOTE]
> <span data-ttu-id="09d35-189">`UseDefaultFiles``UseDirectoryBrowser` url http:// かかります\<アプリ > http:// に末尾のスラッシュと原因せず StaticFiles クライアント側のリダイレクト/\<アプリ >/StaticFiles/(末尾にスラッシュを追加する)。</span><span class="sxs-lookup"><span data-stu-id="09d35-189">`UseDefaultFiles` and `UseDirectoryBrowser` will take the url http://\<app>/StaticFiles without the trailing slash and cause a client side redirect to http://\<app>/StaticFiles/ (adding the trailing slash).</span></span> <span data-ttu-id="09d35-190">なし、ドキュメント内の末尾のスラッシュ相対 Url が正しくないあります。</span><span class="sxs-lookup"><span data-stu-id="09d35-190">Without the trailing slash relative URLs within the documents would be incorrect.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="09d35-191">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="09d35-191">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="09d35-192">`FileExtensionContentTypeProvider`クラスには、ファイル拡張子を MIME コンテンツの種類にマップするコレクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="09d35-192">The `FileExtensionContentTypeProvider` class contains a  collection that maps file extensions to MIME content types.</span></span> <span data-ttu-id="09d35-193">次のサンプルでは、既知の MIME の種類に複数のファイル拡張子が登録されている、".rtf"が置き換えられ、".mp4"が削除されます。</span><span class="sxs-lookup"><span data-stu-id="09d35-193">In the following sample, several file extensions are registered to known MIME types, the ".rtf" is replaced, and ".mp4" is removed.</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]

<span data-ttu-id="09d35-194">参照してください[MIME コンテンツ タイプ](http://www.iana.org/assignments/media-types/media-types.xhtml)です。</span><span class="sxs-lookup"><span data-stu-id="09d35-194">See   [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="09d35-195">非標準のコンテンツの種類</span><span class="sxs-lookup"><span data-stu-id="09d35-195">Non-standard content types</span></span>

<span data-ttu-id="09d35-196">ASP.NET の静的ファイル ミドルウェアは、ほぼ 400 の既知のファイル コンテンツの種類を認識しています。</span><span class="sxs-lookup"><span data-stu-id="09d35-196">The ASP.NET static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="09d35-197">ユーザーは、不明なファイル種類のファイルを要求している場合、静的ファイル ミドルウェアは、HTTP 404 (Not found) 応答を返します。</span><span class="sxs-lookup"><span data-stu-id="09d35-197">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not found) response.</span></span> <span data-ttu-id="09d35-198">ディレクトリの参照が有効になっている場合は、ファイルへのリンクが表示されますが、URI は、HTTP 404 エラーを返します。</span><span class="sxs-lookup"><span data-stu-id="09d35-198">If directory browsing is enabled, a link to the file will be displayed, but the URI will return an HTTP 404 error.</span></span>

<span data-ttu-id="09d35-199">次のコードでは、不明な型の機能を有効にし、不明なファイルをイメージとして表示されます。</span><span class="sxs-lookup"><span data-stu-id="09d35-199">The following code enables serving unknown types and will render the unknown file as an image.</span></span>

[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]

<span data-ttu-id="09d35-200">上記のコードでは、イメージとして、不明なコンテンツの種類のファイルの要求が返されます。</span><span class="sxs-lookup"><span data-stu-id="09d35-200">With the code above, a request for a file with an unknown content type will be returned as an image.</span></span>

>[!WARNING]
> <span data-ttu-id="09d35-201">有効にする`ServeUnknownFileTypes`セキュリティ上のリスクは、使用することはお勧めします。</span><span class="sxs-lookup"><span data-stu-id="09d35-201">Enabling `ServeUnknownFileTypes` is a security risk and using it is discouraged.</span></span>  <span data-ttu-id="09d35-202">`FileExtensionContentTypeProvider`(上記で説明した) 非標準の拡張子を持つファイルを処理するより安全な代替手段を提供します。</span><span class="sxs-lookup"><span data-stu-id="09d35-202">`FileExtensionContentTypeProvider`  (explained above) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="09d35-203">注意事項</span><span class="sxs-lookup"><span data-stu-id="09d35-203">Considerations</span></span>

>[!WARNING]
> <span data-ttu-id="09d35-204">`UseDirectoryBrowser`および`UseStaticFiles`機密データがリークすることができます。</span><span class="sxs-lookup"><span data-stu-id="09d35-204">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="09d35-205">お勧めする**いない**有効にするディレクトリの実稼働環境で参照します。</span><span class="sxs-lookup"><span data-stu-id="09d35-205">We recommend that you **not** enable directory browsing in production.</span></span> <span data-ttu-id="09d35-206">注意が必要で有効にするディレクトリに関する`UseStaticFiles`または`UseDirectoryBrowser`ようにディレクトリ全体とすべてのサブディレクトリはアクセスできなくなります。</span><span class="sxs-lookup"><span data-stu-id="09d35-206">Be careful about which directories you enable with `UseStaticFiles` or `UseDirectoryBrowser` as the entire directory and all sub-directories will be accessible.</span></span> <span data-ttu-id="09d35-207">独自のディレクトリなどのパブリック コンテンツを保持することをお勧め*\<コンテンツのルート >/wwwroot*アプリケーションのビューや構成ファイルなどから離れた場所。</span><span class="sxs-lookup"><span data-stu-id="09d35-207">We recommend keeping public content in its own directory such as *\<content root>/wwwroot*, away from application views, configuration files, etc.</span></span>

* <span data-ttu-id="09d35-208">Url で公開されているコンテンツを`UseDirectoryBrowser`と`UseStaticFiles`は大文字小文字の区別および基になるファイル システムの文字の制限に従って提供します。</span><span class="sxs-lookup"><span data-stu-id="09d35-208">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of their underlying file system.</span></span> <span data-ttu-id="09d35-209">たとえば、Windows では大文字と小文字が Mac と Linux。</span><span class="sxs-lookup"><span data-stu-id="09d35-209">For example, Windows is case insensitive, but Mac and Linux are not.</span></span>

* <span data-ttu-id="09d35-210">IIS でホストされている ASP.NET Core アプリケーションでは、ASP.NET のコア モジュールを使用して、静的ファイルに対する要求を含むアプリケーションにすべての要求を転送します。</span><span class="sxs-lookup"><span data-stu-id="09d35-210">ASP.NET Core applications hosted in IIS use the ASP.NET Core Module to forward all requests to the application including requests for static files.</span></span> <span data-ttu-id="09d35-211">ASP.NET Core モジュールによって処理される前に、要求を処理する機会を取得しないために、IIS の静的ファイル ハンドラーは使用されません。</span><span class="sxs-lookup"><span data-stu-id="09d35-211">The IIS static file handler is not used because it doesn't get a chance to handle requests before they are handled by the ASP.NET Core Module.</span></span>

* <span data-ttu-id="09d35-212">(サーバーまたは web サイト レベルで IIS の静的ファイル ハンドラーを削除するには。</span><span class="sxs-lookup"><span data-stu-id="09d35-212">To remove the IIS static file handler (at the server or website level):</span></span>

     * <span data-ttu-id="09d35-213">移動し、**モジュール**機能</span><span class="sxs-lookup"><span data-stu-id="09d35-213">Navigate to the **Modules** feature</span></span>

     * <span data-ttu-id="09d35-214">選択**: StaticFileModule**一覧で</span><span class="sxs-lookup"><span data-stu-id="09d35-214">Select **StaticFileModule** in the list</span></span>

     * <span data-ttu-id="09d35-215">タップ**削除**で、**アクション**サイド バー</span><span class="sxs-lookup"><span data-stu-id="09d35-215">Tap **Remove** in the **Actions** sidebar</span></span>

>[!WARNING]
> <span data-ttu-id="09d35-216">IIS 静的ファイル ハンドラーが有効になっている場合**と**ASP.NET Core モジュール (ANCM) が正しく構成されていない (たとえば場合*web.config*が展開されていない)、静的ファイルが処理されます。</span><span class="sxs-lookup"><span data-stu-id="09d35-216">If the IIS static file handler is enabled **and** the ASP.NET Core Module (ANCM) is not correctly configured (for example if *web.config* was not deployed), static files will be served.</span></span>

* <span data-ttu-id="09d35-217">コード ファイル (c# および Razor を含む) は、アプリ プロジェクトの外側に配置する必要があります`web root`(*wwwroot*既定)。</span><span class="sxs-lookup"><span data-stu-id="09d35-217">Code files (including c# and Razor) should be placed outside of the app project's `web root` (*wwwroot* by default).</span></span> <span data-ttu-id="09d35-218">これには、明確な区別する、アプリのクライアント側のコンテンツとサーバー側コードが漏洩するを防ぎますサーバー側ソース コードが作成されます。</span><span class="sxs-lookup"><span data-stu-id="09d35-218">This creates a clean separation between your app's client side content and server side source code, which prevents server side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="09d35-219">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="09d35-219">Additional Resources</span></span>

* [<span data-ttu-id="09d35-220">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="09d35-220">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="09d35-221">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="09d35-221">Introduction to ASP.NET Core</span></span>](../index.md)
