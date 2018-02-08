---
title: "ASP.NET Core でのファイル プロバイダー"
author: ardalis
description: "ASP.NET Core がファイル プロバイダーを使用してファイル システムへのアクセスを抽象化する方法について説明します。"
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/file-providers
ms.openlocfilehash: 06197f967e111d75531e9c3bcbcbdb971cb9f99b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="e3419-103">ASP.NET Core でのファイル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e3419-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="e3419-104">作成者: [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e3419-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e3419-105">ASP.NET Core は、ファイル プロバイダーを使用してファイル システムへのアクセスを抽象化します。</span><span class="sxs-lookup"><span data-stu-id="e3419-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="e3419-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="e3419-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="e3419-107">ファイル プロバイダー抽象化</span><span class="sxs-lookup"><span data-stu-id="e3419-107">File Provider abstractions</span></span>

<span data-ttu-id="e3419-108">ファイル プロバイダーは、ファイル システムに対する抽象化です。</span><span class="sxs-lookup"><span data-stu-id="e3419-108">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="e3419-109">メイン インターフェイスは `IFileProvider` です。</span><span class="sxs-lookup"><span data-stu-id="e3419-109">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="e3419-110">`IFileProvider` はファイル情報を取得するメソッド (`IFileInfo`)、ディレクトリ情報を取得するメソッド (`IDirectoryContents`) を公開し、変更通知を設定します (`IChangeToken` を使用)。</span><span class="sxs-lookup"><span data-stu-id="e3419-110">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="e3419-111">`IFileInfo` は個々のファイルまたはディレクトリに関するメソッドおよびプロパティを提供します。</span><span class="sxs-lookup"><span data-stu-id="e3419-111">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="e3419-112">そのプロパティとしては、2 つのブール型プロパティ (`Exists` および `IsDirectory`) と、ファイルの名前 (`Name`)、バイト単位でのファイルの長さ (`Length`)、および日付 (`LastModified`) を示すプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="e3419-112">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="e3419-113">この `CreateReadStream` メソッドを使用することで、ファイルから情報を読み取ることができます。</span><span class="sxs-lookup"><span data-stu-id="e3419-113">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="e3419-114">ファイル プロバイダーの実装</span><span class="sxs-lookup"><span data-stu-id="e3419-114">File Provider implementations</span></span>

<span data-ttu-id="e3419-115">`IFileProvider` の実装には、物理、埋め込み、およびコンポジットの 3 つがあります。</span><span class="sxs-lookup"><span data-stu-id="e3419-115">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="e3419-116">物理プロバイダーは、実際のシステム ファイルにアクセスする場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="e3419-116">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="e3419-117">埋め込みプロバイダーは、アセンブリに埋め込まれているファイルにアクセスする場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="e3419-117">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="e3419-118">コンポジット プロパイダーは、その他の 1 つまたは複数のプロバイダーからのファイルおよびディレクトリに対するアクセスを結合する場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="e3419-118">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="e3419-119">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="e3419-119">PhysicalFileProvider</span></span>

<span data-ttu-id="e3419-120">`PhysicalFileProvider` は、物理ファイル システムへのアクセス許可を提供します。</span><span class="sxs-lookup"><span data-stu-id="e3419-120">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="e3419-121">これは、`System.IO.File` 型をラップし (物理プロバイダーの場合)、ディレクトリとその子へのすべてのパスのスコープを設定します。</span><span class="sxs-lookup"><span data-stu-id="e3419-121">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="e3419-122">このスコープ設定により、特定のディレクトリとその子へのアクセスが制限され、この境界の外側でのファイル システムへのアクセスが防止されます。</span><span class="sxs-lookup"><span data-stu-id="e3419-122">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="e3419-123">このプロバイダーをインスタンス化する場合は、ディレクトリ パスを指定する必要があります。ディレクトリ パスは、このプロバイダーに送られるすべての要求のベース パスとして機能します (また、このパス以外からのアクセスを制限します)。</span><span class="sxs-lookup"><span data-stu-id="e3419-123">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="e3419-124">ASP.NET Core アプリでは、`PhysicalFileProvider` プロバイダーを直接インスタンス化することも、[依存関係挿入](dependency-injection.md)を介してコントローラーまたはサーバーのコンストラクター内で `IFileProvider` を要求することもできます。</span><span class="sxs-lookup"><span data-stu-id="e3419-124">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="e3419-125">後者のアプローチでは、通常、柔軟でテストが容易なソリューションがもたらされます。</span><span class="sxs-lookup"><span data-stu-id="e3419-125">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="e3419-126">次のサンプルは、`PhysicalFileProvider` の作成方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e3419-126">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="e3419-127">そのディレクトリの内容を繰り返し処理したり、サブパスを提供することによって、特定のファイルの情報を取得できます。</span><span class="sxs-lookup"><span data-stu-id="e3419-127">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="e3419-128">コントローラーからプロバイダーを要求するには、コントローラーのコンストラクター内でプロバイダーを指定し、ローカル フィールドに割り当てます。</span><span class="sxs-lookup"><span data-stu-id="e3419-128">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="e3419-129">アクション メソッドからのローカル インスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="e3419-129">Use the local instance from your action methods:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="e3419-130">次に、アプリの `Startup` クラスでプロバイダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="e3419-130">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="e3419-131">*Index.cshtml* ビューで、指定された `IDirectoryContents` を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="e3419-131">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="e3419-132">結果は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="e3419-132">The result:</span></span>

![物理ファイルおよびフォルダーを一覧したファイル プロバイダー サンプル アプリケーション](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="e3419-134">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="e3419-134">EmbeddedFileProvider</span></span>

<span data-ttu-id="e3419-135">`EmbeddedFileProvider` は、アセンブリに埋め込まれているファイルにアクセスする場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="e3419-135">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="e3419-136">.NET Core では、*.csproj* ファイルの `<EmbeddedResource>` 要素を使用してアセンブリにファイルを埋め込みます。</span><span class="sxs-lookup"><span data-stu-id="e3419-136">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="e3419-137">アセンブリに埋め込むファイルを指定する場合は、[glob パターン](#globbing-patterns)を使用できます。</span><span class="sxs-lookup"><span data-stu-id="e3419-137">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="e3419-138">これらのパターンを使用すれば、1 つまたは複数のファイルを照合することができます。</span><span class="sxs-lookup"><span data-stu-id="e3419-138">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="e3419-139">ほとんどの場合、プロジェクト内のすべての .js ファイルを、そのアセンブリに実際に埋め込むことはありません。上記のサンプルは、デモのみを目的としています。</span><span class="sxs-lookup"><span data-stu-id="e3419-139">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="e3419-140">`EmbeddedFileProvider` を作成する場合、それが読み取るアセンブリをそのコンストラクターに渡します。</span><span class="sxs-lookup"><span data-stu-id="e3419-140">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="e3419-141">上記のスニペットは、現在実行中のアセンブリへのアクセス許可を使用して `EmbeddedFileProvider` を作成する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="e3419-141">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="e3419-142">`EmbeddedFileProvider` を使用するサンプル アプリを更新すると、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="e3419-142">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![埋め込みファイルを一覧表示するファイル プロバイダー サンプル アプリケーション](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="e3419-144">埋め込みリソースではディレクトリは公開されません。</span><span class="sxs-lookup"><span data-stu-id="e3419-144">Embedded resources don't expose directories.</span></span> <span data-ttu-id="e3419-145">その代わり、リソースへのパス (名前空間を経由する) は、`.` 区切り記号を使用して、リソースのファイル名に埋め込まれます。</span><span class="sxs-lookup"><span data-stu-id="e3419-145">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="e3419-146">`EmbeddedFileProvider` では、`baseNamespace` という省略可能なパラメーターも受け入れます。</span><span class="sxs-lookup"><span data-stu-id="e3419-146">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="e3419-147">このパラメーターを指定すると、`GetDirectoryContents` の呼び出しが、指定した名前空間の下にあるリソースにスコープされます。</span><span class="sxs-lookup"><span data-stu-id="e3419-147">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="e3419-148">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="e3419-148">CompositeFileProvider</span></span>

<span data-ttu-id="e3419-149">`CompositeFileProvider` は、`IFileProvider` インスタンスを結合し、複数のプロバイダーからのファイルを操作するための 1 つのインターフェイスを公開します。</span><span class="sxs-lookup"><span data-stu-id="e3419-149">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="e3419-150">`CompositeFileProvider` を作成する場合、1 つまたは複数の `IFileProvider` インスタンスをコンストラクターに渡します。</span><span class="sxs-lookup"><span data-stu-id="e3419-150">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="e3419-151">以前に構成した物理プロバイダーと埋め込みプロバイダーの両方を含んだ `CompositeFileProvider` を使用するようにサンプル アプリを構成すると、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="e3419-151">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![物理ファイル/フォルダーと埋め込みファイルを両方とも一覧表示しているファイル プロバイダー サンプル アプリケーション](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="e3419-153">変更の監視</span><span class="sxs-lookup"><span data-stu-id="e3419-153">Watching for changes</span></span>

<span data-ttu-id="e3419-154">`IFileProvider` `Watch` メソッドは、変更がないかどうか 1 つ以上のファイルまたはディレクトリを監視する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="e3419-154">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="e3419-155">このメソッドは、パス文字列 ([glob パターン](#globbing-patterns)を使用して複数のファイルを指定することができる) を受け取り、`IChangeToken` を返します。</span><span class="sxs-lookup"><span data-stu-id="e3419-155">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="e3419-156">このトークンは、検査可能な `HasChanged` プロパティと、変更が検出されたときに呼び出される `RegisterChangeCallback` メソッドを、指定したパス文字列に公開します。</span><span class="sxs-lookup"><span data-stu-id="e3419-156">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that's called when changes are detected to the specified path string.</span></span> <span data-ttu-id="e3419-157">各変更トークンは、単一の変更に応じて、関連付けられたコールバックを呼び出すのみとなります。</span><span class="sxs-lookup"><span data-stu-id="e3419-157">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="e3419-158">定数の監視を有効に使用するには、次に示すように `TaskCompletionSource` を使用するか、変更に応じて `IChangeToken` インスタンスを再作成します。</span><span class="sxs-lookup"><span data-stu-id="e3419-158">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="e3419-159">この記事のサンプルでは、テキスト ファイルが変更されるたびにメッセージを表示するようにコンソール アプリケーションが構成されています。</span><span class="sxs-lookup"><span data-stu-id="e3419-159">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="e3419-160">ファイルを複数回保存した後の結果:</span><span class="sxs-lookup"><span data-stu-id="e3419-160">The result, after saving the file several times:</span></span>

![dotnet run を実行した後のコマンド ウィンドウであり、変更がないかどうか quotes.txt ファイルを監視するアプリケーションを示しています。ファイルは 5 回変更されています。](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="e3419-162">Docker コンテナーやネットワーク共有など、一部のファイル システムは、変更通知を確実に送信しない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e3419-162">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="e3419-163">`DOTNET_USE_POLLINGFILEWATCHER` 環境変数を `1` または `true` に設定することで、4 秒ごとにファイル システムをポーリングして変更がないかどうか確認します。</span><span class="sxs-lookup"><span data-stu-id="e3419-163">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="e3419-164">glob パターン</span><span class="sxs-lookup"><span data-stu-id="e3419-164">Globbing patterns</span></span>

<span data-ttu-id="e3419-165">ファイル システム パスは、"*glob パターン*" と呼ばれるワイルドカード パターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="e3419-165">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="e3419-166">このような単純なパターンを使用すれば、ファイルのグループを指定できます。</span><span class="sxs-lookup"><span data-stu-id="e3419-166">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="e3419-167">2 つのワイルドカード文字は、`*` と `**` です。</span><span class="sxs-lookup"><span data-stu-id="e3419-167">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="e3419-168">現在のフォルダー レベルにある任意の要素、または任意のファイル名、または任意のファイル拡張子を照合します。</span><span class="sxs-lookup"><span data-stu-id="e3419-168">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="e3419-169">照合はファイル パス内の `/` 文字および `.` 文字によって終了します。</span><span class="sxs-lookup"><span data-stu-id="e3419-169">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="e3419-170">複数のディレクトリ レベルにわたって任意の要素を照合します。</span><span class="sxs-lookup"><span data-stu-id="e3419-170">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="e3419-171">ディレクトリ階層内の多数のファイルを再帰的に照合する場合に使用できます。</span><span class="sxs-lookup"><span data-stu-id="e3419-171">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="e3419-172">glob パターンの例</span><span class="sxs-lookup"><span data-stu-id="e3419-172">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="e3419-173">特定のディレクトリ内の特定のファイルを照合します。</span><span class="sxs-lookup"><span data-stu-id="e3419-173">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="e3419-174">特定のディレクトリ内の `.txt` 拡張子を持つすべてのファイルを照合します。</span><span class="sxs-lookup"><span data-stu-id="e3419-174">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="e3419-175">`directory` ディレクトリのちょうど 1 つ下のレベルにあるディレクトリ内のすべての `bower.json` ファイルを照合します。</span><span class="sxs-lookup"><span data-stu-id="e3419-175">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="e3419-176">`directory` ディレクトリの下の任意の場所で見つかった `.txt` 拡張子を持つすべてのファイルを照合します。</span><span class="sxs-lookup"><span data-stu-id="e3419-176">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="e3419-177">ASP.NET Core でのファイル プロバイダーの使用</span><span class="sxs-lookup"><span data-stu-id="e3419-177">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="e3419-178">ASP.NET Core の複数の部分でファイルのプロバイダーが利用されます。</span><span class="sxs-lookup"><span data-stu-id="e3419-178">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="e3419-179">`IHostingEnvironment` では、アプリのコンテンツ ルートと Web ルートを `IFileProvider` 型として公開します。</span><span class="sxs-lookup"><span data-stu-id="e3419-179">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="e3419-180">静的ファイル ミドルウェアでは、ファイル プロバイダーを使用して静的なファイルを見つけます。</span><span class="sxs-lookup"><span data-stu-id="e3419-180">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="e3419-181">Razor では、ビューを検索する際に `IFileProvider` を多用します。</span><span class="sxs-lookup"><span data-stu-id="e3419-181">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="e3419-182">Dotnet の公開機能では、ファイル プロバイダーと glob パターンを使用して公開する必要があるファイルを指定します。</span><span class="sxs-lookup"><span data-stu-id="e3419-182">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="e3419-183">アプリで使用する場合の推奨事項</span><span class="sxs-lookup"><span data-stu-id="e3419-183">Recommendations for use in apps</span></span>

<span data-ttu-id="e3419-184">ASP.NET Core アプリでファイル システムへのアクセスを必要とする場合は、サンプルに示されているように、依存関係挿入を通して `IFileProvider` のインスタンスを要求し、そのメソッドを使用してアクセスを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="e3419-184">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="e3419-185">この場合、アプリが起動し、ご使用のアプリによってインスタンス化された実装の種類の数が削減されたときに、プロバイダーを 1 回構成することができます。</span><span class="sxs-lookup"><span data-stu-id="e3419-185">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
