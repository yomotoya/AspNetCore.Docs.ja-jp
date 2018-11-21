---
title: ASP.NET Core でのファイル プロバイダー
author: guardrex
description: ASP.NET Core がファイル プロバイダーを使用してファイル システムへのアクセスを抽象化する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 08/01/2018
uid: fundamentals/file-providers
ms.openlocfilehash: 5d0d46ba82cd84e48e5a9b23d6d330d8888beb41
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570101"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="1c6b5-103">ASP.NET Core でのファイル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="1c6b5-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="1c6b5-104">作成者: [Steve Smith](https://ardalis.com/)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1c6b5-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1c6b5-105">ASP.NET Core は、ファイル プロバイダーを使用してファイル システムへのアクセスを抽象化します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="1c6b5-106">ファイル プロバイダーは、ASP.NET Core フレームワークの全体で使用されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="1c6b5-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) では、アプリのコンテンツ ルートと Web ルートが `IFileProvider` 型として公開されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="1c6b5-108">[静的ファイル ミドルウェア](xref:fundamentals/static-files)では、ファイル プロバイダーを使用して静的なファイルを見つけます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="1c6b5-109">[Razor](xref:mvc/views/razor) では、ファイル プロバイダーを使用してページとビューを見つけます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="1c6b5-110">.NET Core Tooling では、ファイル プロバイダーと glob パターンを使用して、公開するファイルを指定します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="1c6b5-111">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="1c6b5-112">ファイル プロバイダーのインターフェイス</span><span class="sxs-lookup"><span data-stu-id="1c6b5-112">File Provider interfaces</span></span>

<span data-ttu-id="1c6b5-113">主要なインターフェイスは [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) です。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-113">The primary interface is [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> <span data-ttu-id="1c6b5-114">`IFileProvider` では次のためのメソッドが公開されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="1c6b5-115">ファイル情報の取得 ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo))。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-115">Obtain file information ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span></span>
* <span data-ttu-id="1c6b5-116">ディレクトリ情報の取得 ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents))。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-116">Obtain directory information ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span></span>
* <span data-ttu-id="1c6b5-117">変更通知の設定 ([IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) を使用)。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-117">Set up change notifications (using an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span></span>

<span data-ttu-id="1c6b5-118">`IFileInfo` ではファイルを操作するためのメソッドとプロパティが提供されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* [<span data-ttu-id="1c6b5-119">Exists</span><span class="sxs-lookup"><span data-stu-id="1c6b5-119">Exists</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [<span data-ttu-id="1c6b5-120">IsDirectory</span><span class="sxs-lookup"><span data-stu-id="1c6b5-120">IsDirectory</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [<span data-ttu-id="1c6b5-121">Name</span><span class="sxs-lookup"><span data-stu-id="1c6b5-121">Name</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* <span data-ttu-id="1c6b5-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (バイト単位)</span><span class="sxs-lookup"><span data-stu-id="1c6b5-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in bytes)</span></span>
* <span data-ttu-id="1c6b5-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) (日付)</span><span class="sxs-lookup"><span data-stu-id="1c6b5-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) date</span></span>

<span data-ttu-id="1c6b5-124">[IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) メソッドを使用してファイルから読み取ることができます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-124">You can read from the file using the [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) method.</span></span>

<span data-ttu-id="1c6b5-125">サンプル アプリでは、[依存関係の挿入](xref:fundamentals/dependency-injection)を介してアプリ全体で使用するために、`Startup.ConfigureServices` でファイル プロバイダーを構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-125">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="1c6b5-126">ファイル プロバイダーの実装</span><span class="sxs-lookup"><span data-stu-id="1c6b5-126">File Provider implementations</span></span>

<span data-ttu-id="1c6b5-127">利用できる `IFileProvider` の実装は 3 つあります。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-127">Three implementations of `IFileProvider` are available.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="1c6b5-128">実装</span><span class="sxs-lookup"><span data-stu-id="1c6b5-128">Implementation</span></span> | <span data-ttu-id="1c6b5-129">説明</span><span class="sxs-lookup"><span data-stu-id="1c6b5-129">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="1c6b5-130">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="1c6b5-130">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="1c6b5-131">システムの物理ファイルにアクセスするために、物理プロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-131">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="1c6b5-132">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="1c6b5-132">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="1c6b5-133">アセンブリに埋め込まれているファイルにアクセスするために、マニフェストが埋め込まれたプロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-133">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="1c6b5-134">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="1c6b5-134">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="1c6b5-135">コンポジット プロパイダーは、その他の 1 つまたは複数のプロバイダーからのファイルおよびディレクトリに対するアクセスを結合する場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-135">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="1c6b5-136">実装</span><span class="sxs-lookup"><span data-stu-id="1c6b5-136">Implementation</span></span> | <span data-ttu-id="1c6b5-137">説明</span><span class="sxs-lookup"><span data-stu-id="1c6b5-137">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="1c6b5-138">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="1c6b5-138">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="1c6b5-139">システムの物理ファイルにアクセスするために、物理プロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-139">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="1c6b5-140">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="1c6b5-140">EmbeddedFileProvider</span></span>](#embeddedfileprovider) | <span data-ttu-id="1c6b5-141">埋め込みプロバイダーは、アセンブリに埋め込まれているファイルにアクセスする場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-141">The embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="1c6b5-142">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="1c6b5-142">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="1c6b5-143">コンポジット プロパイダーは、その他の 1 つまたは複数のプロバイダーからのファイルおよびディレクトリに対するアクセスを結合する場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-143">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

::: moniker-end

### <a name="physicalfileprovider"></a><span data-ttu-id="1c6b5-144">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="1c6b5-144">PhysicalFileProvider</span></span>

<span data-ttu-id="1c6b5-145">[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) によって、物理ファイル システムへのアクセスが可能になります。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-145">The [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) provides access to the physical file system.</span></span> <span data-ttu-id="1c6b5-146">`PhysicalFileProvider` では (物理プロバイダーに対して) [System.IO.File](/dotnet/api/system.io.file) 型が使用され、すべてのパスのスコープが、1 つのディレクトリとその子ディレクトリに設定されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-146">`PhysicalFileProvider` uses the [System.IO.File](/dotnet/api/system.io.file) type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="1c6b5-147">このスコープ設定により、指定されたディレクトリとその子ディレクトリを除くファイル システムにアクセスできなくなります。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-147">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="1c6b5-148">このプロバイダーをインスタンス化する際には、ディレクトリ パスが必要です。これは、プロバイダーを使用して作成されるすべての要求のベース パスとして機能します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-148">When instantiating this provider, a directory path is required and serves as the base path for all requests made using the provider.</span></span> <span data-ttu-id="1c6b5-149">`PhysicalFileProvider` プロバイダーを直接インスタンス化することも、[依存関係の挿入](xref:fundamentals/dependency-injection)を介してコンストラクター内で `IFileProvider` を要求することもできます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-149">You can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="1c6b5-150">**静的な型**</span><span class="sxs-lookup"><span data-stu-id="1c6b5-150">**Static types**</span></span>

<span data-ttu-id="1c6b5-151">次のコードでは、`PhysicalFileProvider` の作成方法と、これを使ってディレクトリのコンテンツとファイル情報を取得する方法が示されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-151">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="1c6b5-152">前の例の型は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-152">Types in the preceding example:</span></span>

* <span data-ttu-id="1c6b5-153">`provider` は `IFileProvider` です。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-153">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="1c6b5-154">`contents` は `IDirectoryContents` です。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-154">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="1c6b5-155">`fileInfo` は `IFileInfo` です。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-155">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="1c6b5-156">ファイル プロバイダーを使用して、`applicationRoot` で指定したディレクトリ全体を反復処理したり、`GetFileInfo` を呼び出してファイル情報を取得したりできます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-156">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="1c6b5-157">ファイル プロバイダーは、`applicationRoot` ディレクトリの外部にはアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-157">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="1c6b5-158">サンプル アプリの `Startup.ConfigureServices` クラスでは、[IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider) を使用してプロバイダーを作成しています。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-158">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

<span data-ttu-id="1c6b5-159">**依存関係の挿入を使用してファイル プロバイダーの型を取得する**</span><span class="sxs-lookup"><span data-stu-id="1c6b5-159">**Obtain File Provider types with dependency injection**</span></span>

<span data-ttu-id="1c6b5-160">プロバイダーを任意のクラスのコンストラクターに挿入し、それをローカル フィールドに割り当てます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-160">Inject the provider into any class constructor and assign it to a local field.</span></span> <span data-ttu-id="1c6b5-161">クラスのメソッド全体で、ファイルにアクセスするためにフィールドを使用します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-161">Use the field throughout the class's methods to access files.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1c6b5-162">サンプル アプリでは、`IndexModel` クラスは `IFileProvider` インスタンスを受け取り、アプリのベース パスに対するディレクトリのコンテンツを取得します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-162">In the sample app, the `IndexModel` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="1c6b5-163">*Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c6b5-163">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="1c6b5-164">`IDirectoryContents` はページ内で繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-164">The `IDirectoryContents` are iterated in the page.</span></span>

<span data-ttu-id="1c6b5-165">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1c6b5-165">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1c6b5-166">サンプル アプリでは、`HomeController` クラスは `IFileProvider` インスタンスを受け取り、アプリのベース パスに対するディレクトリのコンテンツを取得します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-166">In the sample app, the `HomeController` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="1c6b5-167">*Controllers/HomeController.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c6b5-167">*Controllers/HomeController.cs*:</span></span>

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="1c6b5-168">`IDirectoryContents` はビュー内で繰り返されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-168">The `IDirectoryContents` are iterated in the view.</span></span>

<span data-ttu-id="1c6b5-169">*Views/Home/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1c6b5-169">*Views/Home/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/1.x/FileProviderSample/Views/Home/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="1c6b5-170">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="1c6b5-170">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="1c6b5-171">アセンブリ内に埋め込まれているファイルにアクセスするために、[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-171">The [ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="1c6b5-172">`ManifestEmbeddedFileProvider` では、アセンブリにコンパイルされたマニフェストを使用して、埋め込まれたファイルの元のパスを再構築します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-172">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

> [!NOTE]
> <span data-ttu-id="1c6b5-173">`ManifestEmbeddedFileProvider` は、ASP.NET Core 2.1 以降で使用できます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-173">The `ManifestEmbeddedFileProvider` is available in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="1c6b5-174">ASP.NET Core 2.0 以前のアセンブリに埋め込まれたファイルにアクセスする方法については、[このトピックの ASP.NET Core 1.x バージョン](/aspnet/core/fundamentals/file-providers?view=aspnetcore-1.1)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-174">To access files embedded in assemblies in ASP.NET Core 2.0 or earlier, see the [ASP.NET Core 1.x version of this topic](/aspnet/core/fundamentals/file-providers?view=aspnetcore-1.1).</span></span>

<span data-ttu-id="1c6b5-175">埋め込みファイルのマニフェストを生成するには、`<GenerateEmbeddedFilesManifest>` プロパティを `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-175">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="1c6b5-176">[&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) を使用して埋め込むファイルを指定します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-176">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

<span data-ttu-id="1c6b5-177">[glob パターン](#glob-patterns)を使用して、アセンブリに埋め込むファイルを 1 つまたは複数指定します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-177">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="1c6b5-178">サンプル アプリでは `ManifestEmbeddedFileProvider` を作成して、現在実行しているアセンブリをそのコンストラクターに渡します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-178">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="1c6b5-179">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c6b5-179">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="1c6b5-180">追加のオーバーロードを使用すると、次のことが可能になります。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-180">Additional overloads allow you to:</span></span>

* <span data-ttu-id="1c6b5-181">相対ファイル パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-181">Specify a relative file path.</span></span>
* <span data-ttu-id="1c6b5-182">ファイルのスコープを最終変更日に設定します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-182">Scope files to a last modified date.</span></span>
* <span data-ttu-id="1c6b5-183">埋め込みファイルのマニフェストを含む埋め込みリソースに名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-183">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="1c6b5-184">オーバーロード</span><span class="sxs-lookup"><span data-stu-id="1c6b5-184">Overload</span></span> | <span data-ttu-id="1c6b5-185">説明</span><span class="sxs-lookup"><span data-stu-id="1c6b5-185">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="1c6b5-186">ManifestEmbeddedFileProvider(Assembly, String)</span><span class="sxs-lookup"><span data-stu-id="1c6b5-186">ManifestEmbeddedFileProvider(Assembly, String)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | <span data-ttu-id="1c6b5-187">必要に応じて相対パスのパラメーター `root` を指定できます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-187">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="1c6b5-188">`root` を指定して、[GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) の呼び出しのスコープを指定したパス以下のリソースに設定します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-188">Specify the `root` to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided path.</span></span> |
| [<span data-ttu-id="1c6b5-189">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="1c6b5-189">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | <span data-ttu-id="1c6b5-190">必要に応じて、相対パスのパラメーター `root` と、`lastModified` 日付 ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) パラメーターを指定できます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-190">Accepts an optional `root` relative path parameter and a `lastModified` date ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parameter.</span></span> <span data-ttu-id="1c6b5-191">`lastModified` の日付は、[IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) によって返される [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) インスタンスの最終更新日のスコープを設定します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-191">The `lastModified` date scopes the last modification date for the [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) instances returned by the [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> |
| [<span data-ttu-id="1c6b5-192">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="1c6b5-192">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | <span data-ttu-id="1c6b5-193">必要に応じて、相対パス `root`、日付 `lastModified`、`manifestName` パラメーターを指定できます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-193">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="1c6b5-194">`manifestName` は、マニフェストを含む埋め込みリソースの名前を表します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-194">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

### <a name="embeddedfileprovider"></a><span data-ttu-id="1c6b5-195">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="1c6b5-195">EmbeddedFileProvider</span></span>

<span data-ttu-id="1c6b5-196">アセンブリ内に埋め込まれているファイルにアクセスするために、[EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-196">The [EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="1c6b5-197">プロジェクト ファイルの [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) プロパティを使用して、埋め込むファイルを指定します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-197">Specify the files to embed with the [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) property in the project file:</span></span>

```xml
<ItemGroup>
  <EmbeddedResource Include="Resource.txt" />
</ItemGroup>
```

<span data-ttu-id="1c6b5-198">[glob パターン](#glob-patterns)を使用して、アセンブリに埋め込むファイルを 1 つまたは複数指定します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-198">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="1c6b5-199">サンプル アプリでは `EmbeddedFileProvider` を作成して、現在実行しているアセンブリをそのコンストラクターに渡します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-199">The sample app creates an `EmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="1c6b5-200">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c6b5-200">*Startup.cs*:</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="1c6b5-201">埋め込みリソースではディレクトリは公開されません。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-201">Embedded resources don't expose directories.</span></span> <span data-ttu-id="1c6b5-202">その代わり、リソースへのパス (名前空間を経由する) は、`.` 区切り記号を使用して、リソースのファイル名に埋め込まれます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-202">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span> <span data-ttu-id="1c6b5-203">サンプル アプリでは、`baseNamespace` は `FileProviderSample.` です。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-203">In the sample app, the `baseNamespace` is `FileProviderSample.`.</span></span>

<span data-ttu-id="1c6b5-204">[EmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) コンストラクターには、必要に応じて `baseNamespace` パラメーターを指定できます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-204">The [EmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="1c6b5-205">名前空間を指定して、[GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) の呼び出しのスコープを、指定した名前空間の下のリソースに設定します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-205">Specify the base namespace to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided namespace.</span></span>

::: moniker-end

### <a name="compositefileprovider"></a><span data-ttu-id="1c6b5-206">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="1c6b5-206">CompositeFileProvider</span></span>

<span data-ttu-id="1c6b5-207">[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) は、`IFileProvider` インスタンスを結合し、複数のプロバイダーからのファイルを操作するための 1 つのインターフェイスを公開します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-207">The [CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="1c6b5-208">`CompositeFileProvider` を作成する場合、1 つまたは複数の `IFileProvider` インスタンスをそのコンストラクターに渡します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-208">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1c6b5-209">サンプル アプリでは、`PhysicalFileProvider` と `ManifestEmbeddedFileProvider` が、アプリのサービス コンテナーに登録されている `CompositeFileProvider` にファイルを提供します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-209">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1c6b5-210">サンプル アプリでは、`PhysicalFileProvider` と `EmbeddedFileProvider` が、アプリのサービス コンテナーに登録されている `CompositeFileProvider` にファイルを提供します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-210">In the sample app, a `PhysicalFileProvider` and an `EmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="watch-for-changes"></a><span data-ttu-id="1c6b5-211">変更の監視</span><span class="sxs-lookup"><span data-stu-id="1c6b5-211">Watch for changes</span></span>

<span data-ttu-id="1c6b5-212">[IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) メソッドによって、1 つまたは複数のファイルやディレクトリに変更がないかどうか監視するシナリオが提供されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-212">The [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="1c6b5-213">`Watch` にはパス文字列を指定できます。ここでは、[glob パターン](#glob-patterns)を使用して複数のファイルを指定できます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-213">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="1c6b5-214">`Watch` によって [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) が返されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-214">`Watch` returns an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span></span> <span data-ttu-id="1c6b5-215">変更トークンでは次のものが公開されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-215">The change token exposes:</span></span>

* <span data-ttu-id="1c6b5-216">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): このプロパティを調べることで、変更があったかどうか判断できます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-216">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="1c6b5-217">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): 指定したパス文字列に対して変更が検出されたときに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-217">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="1c6b5-218">各変更トークンは、1 つの変更への応答として、関連付けられたコールバックを呼び出すのみです。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-218">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="1c6b5-219">定数の監視を有効に使用するには、次に示すように [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) を使用するか、変更への応答として `IChangeToken` インスタンスを再作成します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-219">To enable constant monitoring, use a [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="1c6b5-220">サンプル アプリでは、*WatchConsole* コンソール アプリは、テキスト ファイルが変更されるたびにメッセージを表示するように構成されています。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-220">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](file-providers/samples/1.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

<span data-ttu-id="1c6b5-221">Docker コンテナーやネットワーク共有など、一部のファイル システムは、変更通知を確実に送信しない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-221">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="1c6b5-222">`DOTNET_USE_POLLING_FILE_WATCHER` 環境変数を `1` または `true` に設定して、変更がないかどうか、4 秒ごとにファイル システムをポーリングして確認します (構成不可)。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-222">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="1c6b5-223">glob パターン</span><span class="sxs-lookup"><span data-stu-id="1c6b5-223">Glob patterns</span></span>

<span data-ttu-id="1c6b5-224">ファイル システム パスは、"*glob (または globbing) パターン*" と呼ばれるワイルドカード パターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-224">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="1c6b5-225">これらのパターンを使用して、ファイルのグループを指定します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-225">Specify groups of files with these patterns.</span></span> <span data-ttu-id="1c6b5-226">2 つのワイルドカード文字は、`*` と `**` です。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-226">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="1c6b5-227">現在のフォルダー レベルにある任意の要素、任意のファイル名、または任意のファイル拡張子を照合します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-227">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="1c6b5-228">照合はファイル パス内の `/` 文字および `.` 文字によって終了します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-228">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="1c6b5-229">複数のディレクトリ レベルにわたって任意の要素を照合します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-229">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="1c6b5-230">ディレクトリ階層内の多数のファイルを再帰的に照合する場合に使用できます。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-230">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="1c6b5-231">**glob パターンの例**</span><span class="sxs-lookup"><span data-stu-id="1c6b5-231">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="1c6b5-232">特定のディレクトリ内の特定のファイルを照合します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-232">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="1c6b5-233">特定のディレクトリ内の *.txt* 拡張子を持つすべてのファイルを照合します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-233">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="1c6b5-234">*directory* フォルダーのちょうど 1 つ下のレベルにあるディレクトリ内のすべての `appsettings.json` ファイルを照合します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-234">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="1c6b5-235">*directory* フォルダーの下の任意の場所にある、*.txt* 拡張子を持つすべてのファイルを照合します。</span><span class="sxs-lookup"><span data-stu-id="1c6b5-235">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>
