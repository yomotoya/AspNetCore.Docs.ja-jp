---
title: "ASP.NET Core ファイル プロバイダー"
author: ardalis
description: "ASP.NET Core ファイル プロバイダーを使用してファイル システムへのアクセスを抽象化する方法について説明します。"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/file-providers
ms.openlocfilehash: db207f19b7ddc24dea36009138840be6efebdb84
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="4851a-103">ASP.NET Core ファイル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="4851a-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="4851a-104">によって[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="4851a-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="4851a-105">ASP.NET Core は、ファイルのプロバイダーを使用してファイル システムへのアクセスを抽象化します。</span><span class="sxs-lookup"><span data-stu-id="4851a-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="4851a-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="4851a-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="4851a-107">ファイル プロバイダーの抽象化</span><span class="sxs-lookup"><span data-stu-id="4851a-107">File Provider abstractions</span></span>

<span data-ttu-id="4851a-108">ファイル プロバイダーは、ファイル システム経由で抽象化です。</span><span class="sxs-lookup"><span data-stu-id="4851a-108">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="4851a-109">メイン インターフェイスは`IFileProvider`します。</span><span class="sxs-lookup"><span data-stu-id="4851a-109">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="4851a-110">`IFileProvider`ファイル情報を取得するメソッドを公開 (`IFileInfo`)、ディレクトリ情報 (`IDirectoryContents`)、変更通知を設定して (を使用して、 `IChangeToken`)。</span><span class="sxs-lookup"><span data-stu-id="4851a-110">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="4851a-111">`IFileInfo`メソッドと個々 のファイルまたはディレクトリのプロパティを提供します。</span><span class="sxs-lookup"><span data-stu-id="4851a-111">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="4851a-112">2 つのブール型プロパティがある`Exists`と`IsDirectory`、ファイルの説明するプロパティだけでなく`Name`、 `Length` (単位: バイト)、および`LastModified`日付。</span><span class="sxs-lookup"><span data-stu-id="4851a-112">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="4851a-113">使用してファイルから読み取ることができます、`CreateReadStream`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="4851a-113">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="4851a-114">ファイル プロバイダーの実装</span><span class="sxs-lookup"><span data-stu-id="4851a-114">File Provider implementations</span></span>

<span data-ttu-id="4851a-115">3 つの実装の`IFileProvider`を利用できます: 物理、埋め込み、および複合です。</span><span class="sxs-lookup"><span data-stu-id="4851a-115">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="4851a-116">実際のシステムのファイルにアクセスするには、物理プロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="4851a-116">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="4851a-117">アセンブリに埋め込まれているファイルにアクセスするには、埋め込みのプロバイダーが使用します。</span><span class="sxs-lookup"><span data-stu-id="4851a-117">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="4851a-118">その他の 1 つまたは複数のプロバイダーからファイルとディレクトリに統合されたアクセスを提供する複合プロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="4851a-118">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="4851a-119">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="4851a-119">PhysicalFileProvider</span></span>

<span data-ttu-id="4851a-120">`PhysicalFileProvider`物理ファイル システムへのアクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="4851a-120">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="4851a-121">それがラップ、`System.IO.File`型 (物理プロバイダー) のすべてのパスのディレクトリとその子のスコープを設定します。</span><span class="sxs-lookup"><span data-stu-id="4851a-121">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="4851a-122">このスコープの設定と、特定のディレクトリとその子は、この境界の外側で、ファイル システムへのアクセスを防止するアクセスが制限されます。</span><span class="sxs-lookup"><span data-stu-id="4851a-122">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="4851a-123">このプロバイダーをインスタンス化するときに、このプロバイダに行われたすべての要求のベース パス (および、このパスの外部アクセスを制限する) として機能するため、ディレクトリのパスに提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4851a-123">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="4851a-124">アプリケーションでは ASP.NET Core、インスタンス化できる、`PhysicalFileProvider`プロバイダーを直接、またはを要求できます、`IFileProvider`を通じてサービスのコンス トラクターまたはコント ローラーで[依存性の注入](dependency-injection.md)です。</span><span class="sxs-lookup"><span data-stu-id="4851a-124">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="4851a-125">後者のアプローチには、通常より柔軟なテストが容易なソリューションが生成されます。</span><span class="sxs-lookup"><span data-stu-id="4851a-125">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="4851a-126">以下のサンプルを作成する方法を示しています、`PhysicalFileProvider`です。</span><span class="sxs-lookup"><span data-stu-id="4851a-126">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="4851a-127">そのディレクトリの内容を繰り返し処理したり、サブパスを提供することによって、特定のファイルの情報を取得できます。</span><span class="sxs-lookup"><span data-stu-id="4851a-127">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="4851a-128">コント ローラーから、プロバイダーを要求するには、コント ローラーのコンス トラクターで指定し、ローカル フィールドを割り当てます。</span><span class="sxs-lookup"><span data-stu-id="4851a-128">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="4851a-129">アクション メソッドからのローカル インスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="4851a-129">Use the local instance from your action methods:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="4851a-130">その結果、アプリのプロバイダーを作成`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="4851a-130">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="4851a-131">*Index.cshtml*を反復処理、表示、`IDirectoryContents`指定します。</span><span class="sxs-lookup"><span data-stu-id="4851a-131">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="4851a-132">結果:</span><span class="sxs-lookup"><span data-stu-id="4851a-132">The result:</span></span>

![ファイル プロバイダー サンプル アプリケーションの一覧の物理ファイルとフォルダー](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="4851a-134">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="4851a-134">EmbeddedFileProvider</span></span>

<span data-ttu-id="4851a-135">`EmbeddedFileProvider`アセンブリに埋め込まれているファイルにアクセスするために使用します。</span><span class="sxs-lookup"><span data-stu-id="4851a-135">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="4851a-136">.NET Core でのアセンブリにファイルを埋め込む、`<EmbeddedResource>`内の要素、 *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4851a-136">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="4851a-137">使用することができます[グロブ パターン](#globbing-patterns)アセンブリに埋め込むファイルを指定する場合。</span><span class="sxs-lookup"><span data-stu-id="4851a-137">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="4851a-138">これらのパターンは、1 つまたは複数のファイルの一致するように使用できます。</span><span class="sxs-lookup"><span data-stu-id="4851a-138">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="4851a-139">実際には、アセンブリでプロジェクトのすべての .js ファイルを埋め込むたい可能性はありません。上記のサンプルでは、デモの目的でのみです。</span><span class="sxs-lookup"><span data-stu-id="4851a-139">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="4851a-140">作成するときに、 `EmbeddedFileProvider`、読み取り、コンス トラクターにアセンブリを渡します。</span><span class="sxs-lookup"><span data-stu-id="4851a-140">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="4851a-141">上記のスニペットは、作成する方法を示します、`EmbeddedFileProvider`現在実行中のアセンブリへのアクセス権を持つ。</span><span class="sxs-lookup"><span data-stu-id="4851a-141">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="4851a-142">使用するサンプル アプリを更新、`EmbeddedFileProvider`次の出力結果します。</span><span class="sxs-lookup"><span data-stu-id="4851a-142">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![ファイル プロバイダーのサンプル アプリケーションが埋め込みファイルを一覧表示します。](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="4851a-144">埋め込みリソースでは、ディレクトリが公開されません。</span><span class="sxs-lookup"><span data-stu-id="4851a-144">Embedded resources do not expose directories.</span></span> <span data-ttu-id="4851a-145">(その名前空間) 経由でリソースへのパスがファイル名を使用して、埋め込まれているではなく、`.`区切り記号。</span><span class="sxs-lookup"><span data-stu-id="4851a-145">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="4851a-146">`EmbeddedFileProvider`コンス トラクターは、省略可能な受け取ります`baseNamespace`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="4851a-146">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="4851a-147">呼び出しのスコープはこれを指定する`GetDirectoryContents`指定された名前空間の下には、そのリソースにします。</span><span class="sxs-lookup"><span data-stu-id="4851a-147">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="4851a-148">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="4851a-148">CompositeFileProvider</span></span>

<span data-ttu-id="4851a-149">`CompositeFileProvider`結合`IFileProvider`インスタンス、複数のプロバイダーからのファイルを操作するための 1 つのインターフェイスを公開します。</span><span class="sxs-lookup"><span data-stu-id="4851a-149">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="4851a-150">作成するときに、 `CompositeFileProvider`、1 つまたは複数を渡す`IFileProvider`コンス トラクターにインスタンス。</span><span class="sxs-lookup"><span data-stu-id="4851a-150">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="4851a-151">使用するサンプル アプリを更新、`CompositeFileProvider`を以前に構成されている物理と埋め込みプロバイダー両方にはが含まれています、次の出力結果します。</span><span class="sxs-lookup"><span data-stu-id="4851a-151">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![ファイル プロバイダー サンプル アプリケーションの物理ファイルおよびフォルダーと埋め込みファイルの両方を一覧表示します。](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="4851a-153">変更の監視</span><span class="sxs-lookup"><span data-stu-id="4851a-153">Watching for changes</span></span>

<span data-ttu-id="4851a-154">`IFileProvider` `Watch`メソッドが 1 つまたは複数のファイルまたはディレクトリの変更を監視する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="4851a-154">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="4851a-155">このメソッドが使用できるパス文字列を受け取ります[グロブ パターン](#globbing-patterns)複数のファイル、および返しますを指定する、`IChangeToken`です。</span><span class="sxs-lookup"><span data-stu-id="4851a-155">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="4851a-156">このトークンを公開、`HasChanged`検査できる、プロパティ、および`RegisterChangeCallback`指定したパス文字列に対する変更が検出されたときに呼び出されるメソッド。</span><span class="sxs-lookup"><span data-stu-id="4851a-156">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that is called when changes are detected to the specified path string.</span></span> <span data-ttu-id="4851a-157">1 つの変更への応答で、関連付けられたコールバックを各変更トークンがのみが呼び出すことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="4851a-157">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="4851a-158">定数の監視を有効に使用することができます、 `TaskCompletionSource` 、次のように作成または再`IChangeToken`インスタンスの変更に応答します。</span><span class="sxs-lookup"><span data-stu-id="4851a-158">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="4851a-159">この記事のサンプルでは、テキスト ファイルが変更されるたびにメッセージを表示するコンソール アプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="4851a-159">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="4851a-160">複数回、ファイルを保存した後の結果:</span><span class="sxs-lookup"><span data-stu-id="4851a-160">The result, after saving the file several times:</span></span>

![Quotes.txt ファイルの変更を監視するアプリケーションを示しています実行 dotnet を実行し、ファイルは 5 回変更された後は、コマンド ウィンドウをします。](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="4851a-162">Docker コンテナー、ネットワーク共有など、一部のファイル システムは、変更通知を確実に送信いない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4851a-162">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="4851a-163">設定、`DOTNET_USE_POLLINGFILEWATCHER`環境変数を`1`または`true`4 秒ごとに、ファイル システムの変更をポーリングします。</span><span class="sxs-lookup"><span data-stu-id="4851a-163">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="4851a-164">グロブ パターン</span><span class="sxs-lookup"><span data-stu-id="4851a-164">Globbing patterns</span></span>

<span data-ttu-id="4851a-165">ファイル システム パスと呼ばれるワイルドカード パターンを使用して*グロブ パターン*です。</span><span class="sxs-lookup"><span data-stu-id="4851a-165">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="4851a-166">ファイルのグループを指定するのには、これらの単純なパターンを使用できます。</span><span class="sxs-lookup"><span data-stu-id="4851a-166">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="4851a-167">2 つのワイルドカード文字は`*`と`**`です。</span><span class="sxs-lookup"><span data-stu-id="4851a-167">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="4851a-168">現在のフォルダー レベル、または任意のファイル名、または任意のファイル拡張子以外に一致します。</span><span class="sxs-lookup"><span data-stu-id="4851a-168">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="4851a-169">一致がによって終了`/`と`.`ファイルのパス内の文字です。</span><span class="sxs-lookup"><span data-stu-id="4851a-169">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="4851a-170">複数のディレクトリ レベルで何かと一致します。</span><span class="sxs-lookup"><span data-stu-id="4851a-170">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="4851a-171">再帰的に使用できるディレクトリ階層内の多数のファイルと一致します。</span><span class="sxs-lookup"><span data-stu-id="4851a-171">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="4851a-172">グロブ パターンの例</span><span class="sxs-lookup"><span data-stu-id="4851a-172">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="4851a-173">特定のディレクトリ内の特定のファイルと一致します。</span><span class="sxs-lookup"><span data-stu-id="4851a-173">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="4851a-174">一致するすべてのファイルが`.txt`特定のディレクトリに拡張します。</span><span class="sxs-lookup"><span data-stu-id="4851a-174">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="4851a-175">すべてを満たす`bower.json`下の 正確に 1 レベルのディレクトリ内のファイル、`directory`ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="4851a-175">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="4851a-176">一致するすべてのファイルが`.txt`拡張機能は、下にある任意の場所が見つかりません、`directory`ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="4851a-176">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="4851a-177">ファイルの ASP.NET Core でプロバイダーの使用</span><span class="sxs-lookup"><span data-stu-id="4851a-177">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="4851a-178">ASP.NET Core のパーツがいくつかは、ファイルのプロバイダーを利用します。</span><span class="sxs-lookup"><span data-stu-id="4851a-178">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="4851a-179">`IHostingEnvironment`アプリのコンテンツのルートととして web ルート公開`IFileProvider`型です。</span><span class="sxs-lookup"><span data-stu-id="4851a-179">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="4851a-180">静的ファイル ミドルウェアでは、ファイルのプロバイダーを使用して、静的なファイルを見つけます。</span><span class="sxs-lookup"><span data-stu-id="4851a-180">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="4851a-181">Razor 多用`IFileProvider`ビューを検索します。</span><span class="sxs-lookup"><span data-stu-id="4851a-181">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="4851a-182">Dotnet のでは、どのファイルをパブリッシュする必要がありますを指定するには、使用してファイルのプロバイダーの機能とグロブ パターンを発行します。</span><span class="sxs-lookup"><span data-stu-id="4851a-182">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="4851a-183">アプリで使用するための推奨事項</span><span class="sxs-lookup"><span data-stu-id="4851a-183">Recommendations for use in apps</span></span>

<span data-ttu-id="4851a-184">ASP.NET Core アプリケーションは、ファイル システムへのアクセスを必要とする場合は、インスタンスを要求することができます`IFileProvider`依存関係の挿入を使用してこのサンプルで示すように、アクセスを実行するメソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="4851a-184">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="4851a-185">これにより、アプリの起動、および実装の種類のアプリをインスタンス化の数が減る場合に、1 回、プロバイダーを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="4851a-185">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
